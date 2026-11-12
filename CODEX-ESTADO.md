# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 11:30** · publicado em **12/11/2026 11:31** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 11:30 · sessao 1cbff62e7def · AUDITORIA-DE-TRAVAS-CORTE · verificador · origem=Code

REPROVADO

**1. O diff não realiza literalmente o item 2.** O aval diz “para só em DESENHO”, mas o disparo continua dependendo da quantidade de reprovações, sem consultar sua classe; inclusive a chamada que dispara retorna PARADA antes de examinar liberações. A classe aparece apenas na nova linha `AUTOLIBERA`. Referências: `aval_auditoria_de_travas_corte_1211.txt:4` do anexo; `bin/disjuntor.py:652`, `bin/disjuntor.py:1020`, `bin/disjuntor.py:1028`, `bin/disjuntor.py:763`.

A exclusividade da segunda ocorrência também não está garantida: a cota protege somente `tipo == 'auto'`. O Code pode liberar pela antiga `ERRO-BOBO`, sem cota nem prova de autoria Ronald; o próprio teste exige essa saída. Isso corresponde à preservação das portas antigas pelo contrato, mas diverge do aval literal. Referências: `contrato_item2.txt:2` do anexo; `bin/disjuntor.py:757`, `bin/disjuntor.py:1044`, `bin/disjuntor.py:1049`, `bin/tests/test_disjuntor.sh:1213`.

Há outra divergência textual: o contrato pede “só 1 FALHA por sha recusado”; implementação e teste usam **SOBE**. Referências: `contrato_item2.txt:4` do anexo; `bin/disjuntor.py:1047`, `bin/tests/test_disjuntor.sh:1177`. As linhas dos anexos foram contadas a partir do primeiro conteúdo de cada anexo.

**2. Escritores e leitores.**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `est['b3'][obra]['auto']` | `decide_b3` acrescenta SHA na liberação automática; `_com_estado` persiste o JSON. `bin/disjuntor.py:1051`, `bin/disjuntor.py:379`. | Guarda da cota; testes da primeira liberação e sobrevivência ao rearme. `bin/disjuntor.py:1044`, `bin/tests/test_disjuntor.sh:1170`, `bin/tests/test_disjuntor.sh:1180`. |
| `auto_sobe` | `decide_b3` acrescenta SHA recusado. `bin/disjuntor.py:1046`. | Deduplicação do aviso. `bin/disjuntor.py:1045`. |
| `lib['auto']` / `b3_libera['auto']` | `ler()` inicializa e conta eventos `LIBERA (auto)`, na janela de 24 horas. `bin/disjuntor.py:1996`, `bin/disjuntor.py:2000`, `bin/disjuntor.py:2025`. | `linha()` formata; `gerar_estado.py` usa `ler()/linha()`; teste antigo consulta o resultado. `bin/disjuntor.py:2285`, `bin/disjuntor.py:2289`, `bin/gerar_estado.py:280`, `bin/gerar_estado.py:281`, `bin/tests/test_disjuntor.sh:430`. |
| Evento `SOBE` | `decide_b3` prepara; `_anuncia` publica após persistência, através de `trilha()`. `bin/disjuntor.py:1047`, `bin/disjuntor.py:1064`, `bin/disjuntor.py:1067`, `bin/disjuntor.py:205`. | Leitor específico encontrado: teste `conta()`. O ESTADO não enumera `SOBE`; o leitor de censo também o descarta. `bin/tests/test_disjuntor.sh:1156`, `bin/tests/test_disjuntor.sh:1177`, `bin/disjuntor.py:2002`, `bin/disjuntor.py:2115`. |
| SHA consumido | `_sha_b3()` normaliza/calcula; `decide_b3` grava em `consumidas`. `bin/disjuntor.py:677`, `bin/disjuntor.py:1039`, `bin/disjuntor.py:1055`. | Guarda de reutilização lê `consumidas` e `topo_no_disparo`. A fotografia produz os SHA anteriores ao disparo. `bin/disjuntor.py:1040`, `bin/disjuntor.py:832`. |
| `B3_AUTO_COTA` | Constante `1`. `bin/disjuntor.py:147`. | Guarda, texto de `SOBE` e mensagem do B3. `bin/disjuntor.py:1044`, `bin/disjuntor.py:1048`, `bin/disjuntor.py:1176`. |

Os consumidores decisórios fora do diff convergem na mesma decisão, podendo provocar a gravação da liberação:

- **Stop:** `hook_stop_fila1.py` chama `julga_b3`, que chama `decide_b3`; recebe paradas e mensagens. `bin/hook_stop_fila1.py:1204`, `bin/hook_stop_fila1.py:1205`, `bin/disjuntor.py:1096`.
- **PreToolUse:** `_nega_b3 → estado_b3 → julga_b3 → decide_b3`. `bin/disjuntor.py:1801`, `bin/disjuntor.py:1105`, `bin/disjuntor.py:1096`.
- **Wrapper Codex:** `--b3-parada → b3_parada → estado_b3`; recusa retorno diferente de sucesso. `bin/codex_pergunta.sh:378`, `bin/disjuntor.py:2383`, `bin/disjuntor.py:1158`.
- **ESTADO:** recebe a contagem de eventos, incorpora `dj_txt` e grava a página; não lê a cota persistida como medida. `bin/gerar_estado.py:281`, `bin/gerar_estado.py:378`, `bin/gerar_estado.py:390`, `bin/disjuntor.py:2025`.

Há ainda leitores genéricos do JSON: `_com_estado` e `ler_estado`; os consumidores deste último selecionam B1, B4 e históricos, sem interpretar `auto`. A referência do hook ao arquivo serve para rejeitá-lo como prova de espera. Referências: `bin/disjuntor.py:367`, `bin/disjuntor.py:401`, `bin/disjuntor.py:596`, `bin/disjuntor.py:1882`, `bin/disjuntor.py:2213`, `bin/hook_stop_fila1.py:664`.

**3. Caminhos e contraexemplos.**

- **Segunda AUTOLIBERA, sozinha:** permanece PARADA, registra `auto_sobe` e não consome seu SHA. A recusa dá `continue` antes da liberação e do consumo. `bin/disjuntor.py:1044`, `bin/disjuntor.py:1046`, `bin/disjuntor.py:1049`, `bin/disjuntor.py:1055`.
- **Segunda ocorrência liberada:** após gastar a primeira AUTO, disparar novamente e escrever no topo:
  ```
  AUTOLIBERA OBRA-A: classe=ERRO-BOBO segundo erro
  ERRO-BOBO OBRA-A: segundo erro escrito pelo Code
  ```
  A primeira linha gera `SOBE`; a segunda libera na mesma decisão, sem autenticar Ronald. Esse caminho está expressamente exercitado pelo teste. `bin/disjuntor.py:757`, `bin/disjuntor.py:1047`, `bin/disjuntor.py:1049`, `bin/disjuntor.py:1056`, `bin/tests/test_disjuntor.sh:1214`.
- **`classe=DESENHO`:** não entra pela porta nova. Porém `classe=FORMA erro de desenho` satisfaz o reconhecedor: há validação do rótulo, não da natureza real do defeito. `bin/disjuntor.py:146`, `bin/disjuntor.py:763`. Classificação correta dos defeitos em produção: `[nao verificado]`.
- **Rearme/reinício normal:** não zeram a cota; rearme preserva `auto`, e cada transição relê o estado persistido. Se o estado estiver ausente, a inicialização usa `{}` e a ausência de `auto` vale zero; isso é perda/troca de estado, não efeito do reinício normal. `bin/disjuntor.py:1134`, `bin/disjuntor.py:1136`, `bin/disjuntor.py:367`, `bin/disjuntor.py:368`, `bin/disjuntor.py:1044`. Ocorrência dessa perda em produção: `[nao verificado]`.

**4. As linhas antigas mudam em alguma composição.** Seus reconhecedores individuais continuam iguais, mas uma AUTO válida anterior ganha a seleção e encerra o loop. Com AUTO seguida de `ERRO-BOBO`, passa a consumir o SHA da AUTO, gastar cota e registrar `LIBERA (auto)`; antes, a candidata seria `ERRO-BOBO`, com outro SHA e outra contagem. Referências: `bin/disjuntor.py:746`, `bin/disjuntor.py:757`, `bin/disjuntor.py:1038`, `bin/disjuntor.py:1051`, `bin/disjuntor.py:1055`, `bin/disjuntor.py:1059`.

Com cota esgotada, a antiga abaixo continua liberando, conforme o controle `c5`. `bin/disjuntor.py:1049`, `bin/tests/test_disjuntor.sh:1213`, `bin/tests/test_disjuntor.sh:1215`.

**5. Os três mutantes declarados têm mordida identificável.**

| Mutante | Verificação que o rejeita |
|---|---|
| Libera a segunda | `c2` exige duas recusas, um SOBE e recusa após rearme. `bin/tests/test_disjuntor.sh:1942`, `bin/tests/test_disjuntor.sh:1186`. |
| Aceita DESENHO | `c3` exige PARADA e reconhecimento vazio. `bin/tests/test_disjuntor.sh:1943`, `bin/tests/test_disjuntor.sh:1195`. |
| Aceita sem classe | A alteração também aceita DESENHO, atingindo `c3`; os inválidos atingem `c4`. `bin/tests/test_disjuntor.sh:1944`, `bin/tests/test_disjuntor.sh:1195`, `bin/tests/test_disjuntor.sh:1206`. |

O harness confere âncora única e inclui controle da cópia sem mutação. `bin/tests/test_disjuntor.sh:1626`, `bin/tests/test_disjuntor.sh:1630`, `bin/tests/test_disjuntor.sh:1652`.

Não há `all([])` nesses controles: `rs` recebe quatro resultados e `ctrl`, três. Entretanto, **a medição positiva fica sem prova**: o teste de `ler()` exige apenas `auto=0`, enquanto `b3_auto` conta a trilha diretamente. Retirar `auto` da regex do medidor mantém esse assert satisfeito. Referências: `bin/tests/test_disjuntor.sh:1201`, `bin/tests/test_disjuntor.sh:1204`, `bin/tests/test_disjuntor.sh:1210`, `bin/tests/test_disjuntor.sh:1215`, `bin/tests/test_disjuntor.sh:431`, `bin/tests/test_disjuntor.sh:1156`, `bin/disjuntor.py:2023`.

O controle da porta antiga também ignora o retorno de `dispara()` e verifica apenas ausência no mapa, sem exigir `LIBERA (erro-bobo)`. É uma fragilidade; A já deveria estar PARADA pelo caso anterior. `bin/tests/test_disjuntor.sh:1186`, `bin/tests/test_disjuntor.sh:1213`, `bin/tests/test_disjuntor.sh:1215`. Execução integral do selo com arquivos e travas reais: `[nao verificado]`.

**6. Classes de reprovação.**

| Classe | Resultado |
|---|---|
| Segundo escritor fora da porta | Não identificado para `auto`: a atribuição específica está em `decide_b3`, persistida pela porta comum. `bin/disjuntor.py:1051`, `bin/disjuntor.py:1075`. |
| Relógio fora da borda | Há leitura interna herdada: `decide_b3` usa `time.time()` quando falta `agora`, como ocorre na via `b3_parada`; `trilha()` usa `datetime.now()`. A cota nova não depende de relógio. `bin/disjuntor.py:988`, `bin/disjuntor.py:1158`, `bin/disjuntor.py:206`, `bin/disjuntor.py:1044`. |
| Selo sem mordida | Os três declarados mordem; falta testemunha positiva do contador `auto` no ESTADO. `bin/tests/test_disjuntor.sh:1186`, `bin/tests/test_disjuntor.sh:1195`, `bin/tests/test_disjuntor.sh:1206`, `bin/tests/test_disjuntor.sh:431`. |
| Campo com dois significados | Existem dois `auto` distintos: lista cumulativa por obra e contador de eventos em 24 horas. Não são a mesma célula; zero no painel não significa cota disponível. `bin/disjuntor.py:1051`, `bin/disjuntor.py:1996`, `bin/disjuntor.py:2025`. |
| Ordem de trava | Sem inversão acrescentada: decisão e persistência ficam sob `LOCK_EX`, com anúncio ainda sob a trava. Wrapper e triagem adquirem resposta antes de estado. `bin/disjuntor.py:359`, `bin/disjuntor.py:374`, `bin/disjuntor.py:390`, `bin/codex_pergunta.sh:376`, `bin/codex_pergunta.sh:378`, `bin/disjuntor.py:897`, `bin/disjuntor.py:929`. |

“Sobe ao Ronald” produz uma linha de trilha que o ESTADO não trata como evento próprio; entrega ou notificação efetiva ao Ronald permanece `[nao verificado]`. `bin/disjuntor.py:1047`, `bin/disjuntor.py:2002`.

## 12/11 11:28 · sessao 23a31f2a2d8d · - · - · origem=Ronald (tmux)

Achados, todos limitados à cópia:

- **Divergência literal do aval:** B3 continua disparando por quantidade de reprovações, sem consultar classe: `n >= B3_REPROVACOES` ou `nc >= B3_CODEX_VOLTAS`, marcando PARADA e dando `continue` (`bin/disjuntor.py:1018`, `bin/disjuntor.py:1020`, `bin/disjuntor.py:1028`). A leitura de veredito recolhe obra e APROVADO/REPROVADO, não FORMA/DESENHO (`bin/disjuntor.py:652`, `bin/disjuntor.py:669`). Portanto “para só em DESENHO” não foi implementado literalmente.

- **Segunda ocorrência pode liberar pela porta antiga:** cota protege exclusivamente `tipo == 'auto'`; esgotada, dá `continue` (`bin/disjuntor.py:1044`, `bin/disjuntor.py:1049`). `ERRO-BOBO` antigo não exige autoria Ronald nem cota (`bin/disjuntor.py:757`); entra na liberação genérica (`bin/disjuntor.py:1052`, `bin/disjuntor.py:1058`). O próprio selo exige esse comportamento (`bin/tests/test_disjuntor.sh:1213`, `bin/tests/test_disjuntor.sh:1217`).

  Reproduzi exclusivamente em memória, com IO substituído: primeira AUTO de OBRA-A libera e grava um SHA; nova reprovação, topo contendo AUTO classe=ERRO-BOBO e `ERRO-BOBO OBRA-A: erro 2 escrito pelo Code`, retorna livre e emite **SOBE + LIBERA(erro-bobo)** na mesma decisão. Isso contraria a exclusividade “2a sobe a mim”, mas corresponde ao contrato que preserva a porta antiga (`bin/disjuntor.py:1047`, `bin/disjuntor.py:1056`).

- **DESENHO na nova porta:** `classe=DESENHO` é recusada pelo conjunto fechado FORMA/ERRO-BOBO (`bin/disjuntor.py:146`, `bin/disjuntor.py:763`). Porém só se verifica o rótulo: `classe=FORMA erro de desenho` é reconhecida como auto; classe real do defeito não é auditada (`bin/disjuntor.py:762`, `bin/disjuntor.py:764`). Confirmei ambos os resultados em memória. A ocorrência desse uso em produção é `[nao verificado]`.

- **Rearme/reinício:** rearme altera rearmado, reprovados, codex e disparado; não altera auto (`bin/disjuntor.py:1134`, `bin/disjuntor.py:1136`). Reinício normal relê JSON persistido (`bin/disjuntor.py:366`). Estado ausente vira `{}` e auto ausente equivale a zero (`bin/disjuntor.py:368`, `bin/disjuntor.py:1044`); isso depende de perda/troca/remoção de estado, não é reset causado por reinício normal. Ocorrência real de perda: `[nao verificado]`.

- **Linhas antigas têm mudança observável em combinação com a nova:** reconhecedores DESENHO/ERRO-BOBO estão intactos (`bin/disjuntor.py:746`, `bin/disjuntor.py:757`), mas AUTO válida anterior ganha a seleção e encerra o loop (`bin/disjuntor.py:1038`, `bin/disjuntor.py:1059`). Confirmado em memória:
  - topo `AUTOLIBERA OBRA-A: classe=FORMA ajuste` seguido de `ERRO-BOBO OBRA-A: ajuste`: código novo consome SHA AUTO, gasta cota e registra LIBERA(auto);
  - simulação da base filtrando candidatos AUTO: consome SHA ERRO-BOBO, não gasta cota e registra LIBERA(erro-bobo).
  
  Portanto “comportamento das antigas não mudou em nenhum caso” seria incorreto: consumo, contador e cota mudam nessa combinação (`bin/disjuntor.py:1051`, `bin/disjuntor.py:1055`, `bin/disjuntor.py:1056`).

- **Travas:** leitura, incremento da cota e persistência permanecem sob a mesma LOCK_EX (`bin/disjuntor.py:359`, `bin/disjuntor.py:374`, `bin/disjuntor.py:379`); anúncio é executado ainda sob essa trava (`bin/disjuntor.py:390`, `bin/disjuntor.py:1064`). O diff não acrescenta outra trava nem inversão de ordem nessas operações (`bin/disjuntor.py:1044`, `bin/disjuntor.py:1051`).

- **Relógio:** novo gate usa comprimento da lista e constante, sem leitura temporal (`bin/disjuntor.py:1044`). Existe fallback herdado `time.time()` dentro de `decide_b3`, antes da aquisição da trava (`bin/disjuntor.py:988`), utilizado pela via `b3_parada`, que não passa `agora` (`bin/disjuntor.py:1158`). `trilha` também lê `datetime.now()` (`bin/disjuntor.py:206`). Não há contraexemplo temporal novo encontrado; fotografia usa máximo do relógio recebido e dos mtimes observados (`bin/disjuntor.py:828`, `bin/disjuntor.py:841`).

- **Contrato, evento recusado:** contrato fornecido exige “só 1 FALHA por sha recusado”; implementação usa **SOBE**, deduplicado em auto_sobe (`bin/disjuntor.py:1045`, `bin/disjuntor.py:1047`), e teste também espera SOBE (`bin/tests/test_disjuntor.sh:1177`). Leitor `ler()` não enumera SOBE (`bin/disjuntor.py:2002`). Entrega/notificação efetiva ao Ronald é `[nao verificado]`.

## 12/11 11:28 · sessao 467d3fa641a2 · - · - · origem=Ronald (tmux)

O caso `b3_auto` morde os três mutantes declarados; há lacunas de cobertura fora deles (`bin/tests/test_disjuntor.sh:1942`, `bin/tests/test_disjuntor.sh:1943`, `bin/tests/test_disjuntor.sh:1944`).

| Mutação | Resultado em memória | Assert que morde |
|---|---|---|
| Libera a 2ª | saída 2; falha `[2]` | `bin/tests/test_disjuntor.sh:1186` |
| Aceita DESENHO | saída 3; falha `[3]` | `bin/tests/test_disjuntor.sh:1195` |
| Aceita sem classe | saída 3; falhas `[3,4]` | `bin/tests/test_disjuntor.sh:1195`, `bin/tests/test_disjuntor.sh:1206` |
| `continue` vira `break` | saída 5; falha `[5]` | `bin/tests/test_disjuntor.sh:1215` |
| SOBE duplicado | saída 2; falha `[2]` | `bin/tests/test_disjuntor.sh:1177`, `bin/tests/test_disjuntor.sh:1186` |
| Remove gravação de `consumidas` | passa | ausência de conferência desse campo em `bin/tests/test_disjuntor.sh:1170`, `bin/tests/test_disjuntor.sh:1186`, `bin/tests/test_disjuntor.sh:1217` |
| Retira `auto` da regex de `ler()` | passa | `b3_auto` conta diretamente a trilha, sem chamar `ler()`: `bin/tests/test_disjuntor.sh:1156`, `bin/tests/test_disjuntor.sh:1170`; alteração em `bin/disjuntor.py:2023` |

O teste antigo de medição exige somente `auto: 0`: contador permanentemente zero satisfaz esse assert. Falta testemunha positiva `ler()['b3_libera']['auto'] > 0` (`bin/tests/test_disjuntor.sh:430`, `bin/tests/test_disjuntor.sh:431`).

Não há `all([])` na sequência normal: `rs` recebe quatro resultados e `ctrl` recebe três (`bin/tests/test_disjuntor.sh:1201`, `bin/tests/test_disjuntor.sh:1204`, `bin/tests/test_disjuntor.sh:1210`, `bin/tests/test_disjuntor.sh:1215`). O controle da porta antiga ignora o retorno de `dispara()` e exige apenas ausência no mapa, sem comprovar `LIBERA (erro-bobo)` ou estado persistido; porém A já ficou parada ao final de c2, portanto isso é fragilidade, não falso verde vazio demonstrado na sequência normal (`bin/tests/test_disjuntor.sh:1186`, `bin/tests/test_disjuntor.sh:1213`, `bin/tests/test_disjuntor.sh:1215`).

O harness verifica âncora única e executa controle da cópia sem mutação, evitando queda apenas por árvore incompleta (`bin/tests/test_disjuntor.sh:1623`, `bin/tests/test_disjuntor.sh:1626`, `bin/tests/test_disjuntor.sh:1630`, `bin/tests/test_disjuntor.sh:1652`).

Execução realizada com Python `-B`, fontes compiladas em strings, corpo original de `b3_auto` e substituições de IO/estado/relógio/triagem em memória; integração real de arquivos e travas, execução integral da suíte e resultados de produção: `[nao verificado]`. [nao verificado]

## 12/11 11:27 · sessao fc614e3aba36 · LASTRO-FAIXA-SEM-DEFAULT · verificador · origem=Code

**Não fecharia a c14:** há perda da data na guarda de órfãs, a prova integrada de (d) permanece incompleta e o selo sem célula enfraquece “nunca turno aberto”. (`app/relatorios/pdf_espelho.py:476`, `:512`; `app/ponto/tests/test_lastro_c14_sem_banco.py:295`; `app/relatorios/tests/test_cartao_marca_espuria.py:136`)

1. **(a) PDF — bloqueia: HH:MM de outro dia suprime turno legítimo.**

   A grade própria saiu: órfãs vêm de `orfas_grade`, e a recusa recebida permanece até o compositor comum. Sem achado de outra fonte para **essa classificação de batida/recusa**. (`app/relatorios/pdf_espelho.py:475`, `:561`, `:594`)

   Contudo, `_orfas_hhmm` apaga a data e `_entrada_espuria` compara somente HH:MM. **Contraexemplo sintético:** órfã `07:02` em **10/11**; entrada legítima aberta `07:02` em **11/11**, cujo `orfas_grade=[]`. O resumo exclui o turno de 11/11, mas `batida.orfa=False` nessa linha, que continua imprimindo “turno aberto”. No período livre, a troca pelos totais da competência não corrige esse contador. (`app/relatorios/pdf_espelho.py:178`, `:476`, `:512`, `:565`, `:920`, `:927`; `app/relatorios/cartao_pela_celula.py:322`)

   Portanto, **muda o resultado do código real**; um dia efetivamente atingido em produção não foi demonstrado pelo dado fornecido: **[nao verificado]**.

   Sobra uma leitura literal de outra espécie de órfã: `datas_saida_orfa(_turnos_juiz, ...)`. Ela identifica **saída sem entrada**, não `ata.orfas`; a tela consulta o mesmo juiz. **Sem achado de segundo juiz nesse ponto**, embora o PDF ainda não leia esse campo do retorno da tela. (`app/relatorios/pdf_espelho.py:578`; `app/ponto/turnos.py:234`; `app/ponto/services/espelho.py:622`)

2. **(b) Três sítios — sem achado de perda do canal corrigido.**

   O mapa nasce antes da seleção, entra em `_pela_grade`, atende pré-apuração e `AtaAgregadaSemMarcos` e sobrevive à composição posterior. A2 escreve a recusa por data e deixa esse dia sem conclusão de precedência; o overlay transporta o rótulo com `tipo_dia=None`. (`app/ponto/services/espelho.py:486`, `:498`, `:536`, `:565`, `:655`, `:881`; `app/ponto/precedencia.py:423`, `:425`; `app/escala/utils.py:1218`)

   **Nenhum `except` novo que engole no delta.** Permanecem os genéricos antigos; `TipoSemFaixa` fora do transporte por data é explicitamente relançado. Os testes exercitam seleção e handlers reais, mas não um request HTTP inteiro. Ausência de qualquer outro caminho de 500: **[nao verificado]**. (`app/ponto/services/espelho.py:574`, `:657`, `:660`; `app/ponto/tests/test_lastro_c14_sem_banco.py:113`, `:124`)

   **Fila — visibilidade na UI:** ausência `fantasma_xyz` com batida ou status anteriormente lavrado pode conservar título genérico/antigo. D4 retorna antes das recusas e o template lê `status_rotulo`. Isso não invalida o transporte de `palavra_dia`, mas impede afirmar que o aviso aparece sempre na faixa. (`app/ponto/status_dia.py:290`, `:295`, `:317`; `app/templates/ponto/espelho.html:151`; `app/ponto/tests/test_lastro_c14_sem_banco.py:169`)

3. **QUEM ESCREVE / QUEM LÊ.**

   | Campo | Escritor | Leitor |
   |---|---|---|
   | `recusas[data]` | A2 grava `TipoSemFaixa`, cujo construtor fornece código/rótulo. (`app/ponto/precedencia.py:99`, `:423`) | Overlay, espelho e D4. (`app/escala/utils.py:1218`; `app/ponto/services/espelho.py:881`; `app/ponto/status_dia.py:317`) |
   | `_recusa_do_dia` | Espelho transporta o rótulo. (`app/ponto/services/espelho.py:881`) | Compositor escreve palavra/veredito/cor; último leitor remove o transporte. PDF chama esse compositor. (`app/ponto/services/espelho.py:339`, `:350`; `app/relatorios/pdf_espelho.py:594`) |
   | `orfas_grade` | Cartório compõe `ata.orfas`; leitor transporta; adaptador devolve; espelho publica. (`app/ponto/services/cartorio.py:736`; `app/escala/services/leitor_celula.py:420`; `app/escala/utils.py:1113`; `app/ponto/services/espelho.py:854`) | HTML e mapa do PDF. (`app/templates/ponto/espelho.html:240`; `app/relatorios/pdf_espelho.py:475`) |
   | `palavra_dia` | Compositor comum, inclusive ramo de recusa. (`app/ponto/services/espelho.py:340`, `:347`) | Desenho do PDF. A faixa HTML lê outro campo. (`app/relatorios/pdf_espelho.py:879`; `app/templates/ponto/espelho.html:151`) |
   | `batida.orfa` | Coletor PDF, em memória, por comparação HH:MM na linha. (`app/relatorios/pdf_espelho.py:565`) | Separação visual e supressão de alertas. (`app/relatorios/pdf_espelho.py:832`, `:916`, `:920`) |

   **Bloqueia — ESMERIL-SEMÂNTICA:** uma hora órfã de determinado dia passa a significar “qualquer entrada desse horário na janela”. É a perda demonstrada no item 1. A própria classificação visual ainda usa o mapa por data, criando duas respostas no mesmo documento. (`app/relatorios/pdf_espelho.py:476`, `:512`, `:563`)

   **Fila — desconhecido versus negativo:** sem célula, `orfas_grade=[]` termina em `batida.orfa=False`; isso significa “sem marca disponível”, não prova de batida legitimamente casada. Atas antigas também podem transportar `'?'`. O campo deve ser entendido como marca de apresentação. (`app/escala/services/leitor_celula.py:190`, `:394`; `app/escala/utils.py:1087`; `app/relatorios/pdf_espelho.py:565`)

   **Sem achado de mudança do significado de `data` no delta:** a tela agrupa pelo juiz `dia_das_batidas`, que distingue dia do turno e dia civil da batida não reclamada. O defeito identificado é eliminar essa chave posteriormente. (`app/ponto/turnos.py:2886`; `app/ponto/services/espelho.py:580`; `app/relatorios/pdf_espelho.py:476`)

4. **(c)/(d) Mordida das provas.**

   **(c), sem achado de núcleo novo puramente congelado:** o carregador padrão lê e compila os fontes da cópia; mutantes alteram AST produtiva do canal, reset, classificação e metadados. Há mordida sobre essas operações. (`app/ponto/tests/test_lastro_c5_sem_banco.py:39`, `:108`; `app/ponto/tests/test_lastro_c14_sem_banco.py:35`, `:45`; `app/ponto/tests/test_lastro_c13_sem_banco.py:46`)

   **Fila — alcance parcial:** F1c/cartão continuam contendo selos textuais. A prova dos canais recorta as chamadas e substitui o restante dos builders. **Contraexemplo:** quebrar a geometria antes do repasse pode escapar desse ensaio, embora retirar `recusas` seja detectável. (`app/escala/tests/test_f1c_contrato.py:68`; `app/relatorios/tests/test_cartao_pela_celula.py:170`; `app/ponto/tests/test_lastro_c13_sem_banco.py:194`, `:198`)

   REDs históricos dependem de `git show`; sua reexecução histórica nesta cópia e as saídas dos arquivos de evidência mencionados no relato: **[nao verificado]**. (`app/ponto/tests/test_lastro_c13_sem_banco.py:26`; `app/ponto/tests/test_lastro_c14_sem_banco.py:29`)

   **(d), há transporte real em memória:** builder → `ata_do_dia` → porta `save=False` → leitor → mapas/trecho do fechamento. Não é apenas um duplo; verifica classificação, metadados e limpeza das órfãs. (`app/ponto/tests/test_lastro_c14_sem_banco.py:204`, `:216`, `:235`, `:255`, `:269`)

   **Bloqueia a comprovação integrada de (d):** o simulador recebe `Mock(return_value={17: fech})`. **Contraexemplo:** o recalculador produtivo parar de ler a ata nova e retornar `{}` não reprovaria esse teste. A ata antiga também é sintetizada da nova, mudando apenas `tipo_dia`. Portanto, prova transporte real parcial, mas não a sequência integrada ausência → relavra → recalculador → simulador. (`app/ponto/tests/test_lastro_c14_sem_banco.py:218`, `:295`, `:301`; `app/folha/management/commands/simular_folha.py:130`)

5. **Retarget — preserva o positivo com ata; enfraquece o universo original sem célula.**

   Com ata, continuam marca positiva e zero turno aberto. **Sem célula, o mesmo cenário de 10/06 às 16:19 agora exige ausência de marca e um turno aberto.** Assim, “nunca turno aberto” não foi preservado integralmente. O precedente do calendário documenta perda do asterisco, não essa garantia adicional. **Bloqueia declarar a pergunta preservada.** (`app/relatorios/tests/test_cartao_marca_espuria.py:55`, `:81`, `:97`, `:122`, `:128`, `:136`; `app/colaboradores/tests/test_calendario_orfa.py:18`)

   **É como a tela no mesmo dia?** A tela conta períodos abertos sem a guarda de espúria; com uma única entrada aberta e órfãs vazias, ambos podem contar 1. Porém o novo selo não chama a tela nem compara a data: a igualdade integrada nessa fixture permanece **[nao verificado]**. (`app/ponto/services/espelho.py:436`; `app/relatorios/pdf_espelho.py:512`; `app/relatorios/tests/test_cartao_marca_espuria.py:135`)

   **Fila — mordida visual:** manter `orfa=True` e `16:19` no texto, mas desenhar a batida junto das comuns, escaparia dos asserts. A implementação atual separa e acrescenta asterisco; o selo não prova toda essa disposição. A classe sem célula ainda substitui a emissão do PDF por checagem do resumo. (`app/relatorios/tests/test_cartao_marca_espuria.py:81`, `:109`, `:131`; `app/relatorios/pdf_espelho.py:832`, `:841`)

6. **Demais classes de reprovação.**

   **Sem achado de novo escritor produtivo fora da porta ou nova ordem de trava:** o retarget escreve fixtures; a prova de relavra chama a porta real sem persistência. A afirmação “ata que o cartório lavraria” não é demonstrada pela fixture pronta. (`app/relatorios/tests/test_cartao_marca_espuria.py:66`; `app/ponto/tests/test_lastro_c14_sem_banco.py:235`; `app/ponto/portas/celula.py:1032`)

   **Sem achado de faixa inventada/default novo:** `faixa_de` recusa código desconhecido; o transporte mantém `tipo_dia=None`. Derivação paralela, campo ambíguo e selo sem mordida estão discriminados acima. (`app/ponto/precedencia.py:105`; `app/escala/utils.py:1220`)

   **Fila herdada — relógio fora da borda:** o coletor recebe `agora`, mas `folha_manda` consulta turno vivo e teto sem repassá-lo; estes podem ler o relógio corrente. **Contraexemplo:** ato simulado antes da meia-noite executado depois dela pode usar outro teto/dia vivo. Não nasceu na c14; ocorrência concreta em produção: **[nao verificado]**. (`app/relatorios/pdf_espelho.py:584`; `app/relatorios/cartao_pela_celula.py:398`, `:412`, `:40`; `app/ponto/turnos.py:2291`)

## 12/11 11:27 · sessao 38008ed6103d · - · - · origem=Ronald (tmux)

Mapa dos campos e dos caminhos de produção:

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `est['b3'][obra]['auto']` | `decide_b3`, acrescenta SHA somente ao aceitar `tipo == 'auto'`: `bin/disjuntor.py:1050-1051`; persistência por `_com_estado`: `bin/disjuntor.py:374-380`. | Cota em `decide_b3`: `bin/disjuntor.py:1044`. Teste lê estado em `bin/tests/test_disjuntor.sh:1162-1170` e `:1180`. |
| `auto_sobe` | `decide_b3`, acrescenta SHA recusado: `bin/disjuntor.py:1046`. | Deduplicação do aviso: `bin/disjuntor.py:1045`. |
| `lib['auto']` | `ler` inicializa contador em zero e soma eventos `LIBERA` com `(auto)`: `bin/disjuntor.py:2000`, `:2022-2025`. | Exportado em `b3_libera`: `bin/disjuntor.py:2068`; `linha` lê e imprime: `:2285-2289`. `bin/gerar_estado.py:280-281` chama `ler`/`linha`, incorpora resultado em `:378` e grava página em `:390`. Teste lê em `bin/tests/test_disjuntor.sh:430-431`. |
| Evento `SOBE` | Produzido ao recusar AUTOLIBERA excedente: `bin/disjuntor.py:1047-1048`; publicado somente depois da gravação do estado: `:1064-1067`. | Leitor explícito encontrado é o teste: `bin/tests/test_disjuntor.sh:1156-1161`, `:1177`. O medidor de ESTADO enumera eventos sem `SOBE`: `bin/disjuntor.py:2002-2003`; o outro leitor da trilha aceita somente regex de censo: `:2072`, `:2113-2117`. Assim, `SOBE` não aparece como contador/encaminhamento no ESTADO por esse código. |
| SHA consumido (`consumidas`) | Criado com `_sha_b3` em `bin/disjuntor.py:1039`; acrescentado a `consumidas` em `:1055`. SHA da fotografia também é produzido em `:832`. | Aceitação testa `consumidas` e `topo_no_disparo`: `:1040`; recusa excedente usa `continue` antes do consumo: `:1049`. |
| `B3_AUTO_COTA` | Constante igual a 1: `bin/disjuntor.py:147`. | Guarda em `:1044`, texto do evento em `:1048`, mensagem do B3 em `:1176`. |

Todos os consumidores decisórios de produção convergem no mesmo escritor:

- Stop: `bin/hook_stop_fila1.py:1204` → `julga_b3` → `decide_b3` (`bin/disjuntor.py:1093-1099`). O hook recebe as paradas e compõe mensagens em `bin/hook_stop_fila1.py:1205-1208`.
- PreToolUse: `_nega_b3` chama `estado_b3` em `bin/disjuntor.py:1801`; este chama `julga_b3` em `:1105`, que chama `decide_b3` em `:1096`.
- Wrapper: `bin/codex_pergunta.sh:378` executa `--b3-parada`; CLI encaminha em `bin/disjuntor.py:2382-2383`; `b3_parada` usa `estado_b3` em `:1158`.
- Recorte das duas linhas úteis continua no hook: `bin/hook_stop_fila1.py:451-464`; disjuntor o chama sobre o RELATO já lido em `bin/disjuntor.py:797`.
- Referência do hook ao arquivo de estado em `bin/hook_stop_fila1.py:666-669` serve para impedir que o próprio estado prove espera; não interpreta `auto`.
- `ler_estado` lê o JSON inteiro em `bin/disjuntor.py:397-408`; seus leitores de B1/B4 e dos históricos selecionam outros campos (`:596`, `:1882`, `:2213`), sem interpretar `b3[obra].auto`.

Persistência e travas:

- `_com_estado` toma `LOCK_EX` antes de ler o JSON (`bin/disjuntor.py:355-374`), grava por substituição (`:377-380`) e anuncia ainda sob a trava (`:390-394`). A decisão nova usa essa porta em `:1075`.
- Rearme não limpa `auto`, `auto_sobe` ou `consumidas`: altera `rearmado`, `reprovados`, `codex` e `disparado` em `bin/disjuntor.py:1134-1136`. A migração também preserva esses campos (`:804-821`).
- Reinício normal relê o JSON em `bin/disjuntor.py:366-374`; ausência do arquivo vira `{}` (`:368-369`), mas ilegibilidade levanta erro (`:370-373`). A ausência real desse arquivo em produção é `[nao verificado]`.
- Wrapper toma o slot antes da porta (`bin/codex_pergunta.sh:350-364`), depois a trava da resposta (`:374-376`), e então pergunta B3 (`:378`). Triagem toma a mesma trava de resposta compartilhada antes da trava do estado (`bin/disjuntor.py:889-900`, `:915-929`).

Duas observações para a consolidação:

1. `auto` persistido e `auto` do painel têm unidades diferentes: lista cumulativa de SHA por obra (`bin/disjuntor.py:1051`) versus número de eventos LIBERA na janela de 24 h (`:1996`, `:2000`, `:2025`). São estruturas distintas; o painel não é um leitor da cota.
2. “Sobe ao Ronald” está implementado como texto na trilha (`bin/disjuntor.py:1047-1048`), sem leitor de produção específico para `SOBE` no mapa encontrado. Entrega/notificação externa a Ronald é `[nao verificado]`.

## 12/11 11:25 · sessao dc6cc4c65e84 · - · - · origem=Ronald (tmux)

1. **Bloqueia a certificação literal de “pergunta preservada”**: o caso com ATA conserva marca positiva e zero turno aberto, mas o caso original sem célula foi enfraquecido: agora exige marca ausente e um turno aberto. Contraexemplo concreto: a mesma batida espúria de 10/06/2026 16:19, apagadas as células, satisfaz o novo selo com `turnos_abertos=1`, enquanto contrariaria o selo anterior com expectativa zero. A fonte única justifica ler a ausência de classificação; não demonstra preservação da garantia “nunca turno aberto” (`app/relatorios/tests/test_cartao_marca_espuria.py:53`, `:77`, `:97`, `:119`, `:124`, `:131`). A referência do calendário documenta perda da marca, não autorização para acusar turno aberto (`app/colaboradores/tests/test_calendario_orfa.py:18`).

2. **Fila — igualdade com a tela não provada pelo selo.** O comentário afirma “como é na tela”, mas o método só chama o coletor e exige contador 1; não chama a tela nem compara a data da lista (`app/relatorios/tests/test_cartao_marca_espuria.py:131`). O código explica por que 1 é plausível: tela incrementa cada `p.turno_aberto`, enquanto PDF exclui os horários classificados como órfãos; sem célula, a lista de órfãs é vazia (`app/ponto/services/espelho.py:424`, `:436`; `app/relatorios/pdf_espelho.py:512`; `app/escala/services/leitor_celula.py:394`; `app/escala/utils.py:1087`). O resultado integrado para essa fixture e o mesmo dia é **[nao verificado]**.

3. **Fila — mordida visual incompleta.** Mesmo o caso positivo verifica `batida.orfa`, ausência de “turno aberto” no texto e presença de “16:19”; não verifica asterisco, legenda ou separação da linha (`app/relatorios/tests/test_cartao_marca_espuria.py:81`, `:109`). Contraexemplo: renderizar a órfã junto das marcações comuns, mantendo o atributo e o horário no texto, não violaria esses asserts. O código atual efetivamente separa as marcações e aplica asterisco/cinza; portanto é lacuna de prova, não defeito visual demonstrado (`app/relatorios/pdf_espelho.py:832`, `:841`, `:851`).

4. **Fila — nova classe perde prova de emissão.** O override substitui a emissão/extração do PDF do método pai por uma checagem apenas do resumo. A presença da batida sem célula é verificada só no dict do coletor (`app/relatorios/tests/test_cartao_marca_espuria.py:103`, `:126`, `:131`). Contraexemplo: o renderer descartar a marcação sem célula preservaria esses asserts.

5. **Sem achado de segundo escritor produtivo neste retarget.** A criação direta de `CelulaDia` é fixture de teste expressamente informada pela sessão; não demonstra escritor produtivo fora da porta (`app/relatorios/tests/test_cartao_marca_espuria.py:66`). Porém “ATA que o cartório lavraria” permanece **[nao verificado]**: a fixture entrega lâmpadas e órfã prontas, sem executar builder/cartório para comparar a classificação (`app/relatorios/tests/test_cartao_marca_espuria.py:56`).

Leituras adicionais: o dia apresentado vem do juiz `dia_das_batidas`, que distingue batida reclamada por turno e batida sem turno; a tela agrupa por esse mapa, e não por dia civil próprio (`app/ponto/turnos.py:2886`; `app/ponto/services/espelho.py:453`, `:579`). Nenhuma execução integrada certificada nesta análise **[nao verificado]**.

## 12/11 11:25 · sessao c96f7f36ec7e · - · - · origem=Ronald (tmux)

(c) **Sem achado de RED puramente congelado no núcleo novo.** O carregador padrão lê os fontes desta cópia e compila AST real (`app/ponto/tests/test_lastro_c5_sem_banco.py:33-49`, `:108-118`). Os mutantes mudam AST produtiva: canal `_contrato`, reset do mapa, tipo no overlay e metadados da ata (`app/ponto/tests/test_lastro_c14_sem_banco.py:35-55`); overlay/canais/PDF da c13 também (`app/ponto/tests/test_lastro_c13_sem_banco.py:46-80`).

**Fila — alcance parcial e selos textuais.** Os dois selos F1c/cartão exercitados por c13 continuam sendo testes de presença/ausência de texto; executar suas funções não os transforma em prova comportamental (`app/ponto/tests/test_lastro_c13_sem_banco.py:127-149`; `app/relatorios/tests/test_cartao_pela_celula.py:170-181`). A prova dos canais extrai apenas chamadas reais dos builders e substitui o resto do corpo (`app/ponto/tests/test_lastro_c13_sem_banco.py:190-199`). Contraexemplo: quebrar a geometria antes da chamada `_marcar_ausencias` no builder não é percebido por esse recorte. Isso limita a prova, mas não elimina sua mordida sobre o repasse de `recusas` (`app/ponto/tests/test_lastro_c13_sem_banco.py:194-208`).

**RED histórico: reexecução nesta cópia [nao verificado].** Exige `git show` dos objetos c12/c13 (`app/ponto/tests/test_lastro_c13_sem_banco.py:26-45`; `app/ponto/tests/test_lastro_c14_sem_banco.py:29-34`). Os arquivos `docs/evidencia_c14_*.txt` citados no relato não constam nesta cópia; conteúdo/saídas históricas além do anexo [nao verificado].

(d) **Há transporte real em memória; não é só duplo.** O teste chama o builder single real e verifica folga, metadados, limpeza das órfãs e conservação da batida externa; a contraprova desliga apenas o overlay e reencontra `23:00` (`app/ponto/tests/test_lastro_c14_sem_banco.py:199-215`). Chama `ata_do_dia`, `lavrar_veredito(save=False)`, leitor, `grade_do_fechamento`, mapas financeiros e trecho real do fechamento; verifica mudança da ata antiga ausência → folga (`app/ponto/tests/test_lastro_c14_sem_banco.py:216-273`). Os mutantes removem classificação/metadados nos fontes reais (`:45-55`).

**Bloqueia o fechamento literal de (d), caso o requisito seja simular a cadeia integrada.** A passagem ao simulador substitui o recalculador real por `Mock(return_value={17: fech})`, classificador/TXT por duplos e roda `instantaneo(lido=True)` sobre esse fechamento já preparado (`app/ponto/tests/test_lastro_c14_sem_banco.py:280-306`). Contraexemplo: se o recalculador real deixar de ler a ata relavrada e retornar `{}`, esse teste continua verde, porque a implementação real foi substituída em `:295`. O simulador real solicita o recalculador em `app/folha/management/commands/simular_folha.py:129-134`; esse elo não é exercitado.

A antiga ata também é sintetizada a partir da nova, mudando só `tipo_dia`; não nasce de um primeiro julgamento ausência (`app/ponto/tests/test_lastro_c14_sem_banco.py:217-222`). Persistência é explicitamente proibida no teste (`:223-244`). Portanto: prova classificação/transporte/lavra/leitores em memória; execução `processar_cartorio` → persistência → recalcular → fotos/comparação [nao verificado].

O caminho shell capaz de atravessar a relavra existe: `--antes` roda antes do simulador; em `par`, aplica somente à foto depois (`bin/simular_folha.sh:65-74`, `:94-99`). Isso não é executado pelo teste c14 (`app/ponto/tests/test_lastro_c14_sem_banco.py:183-308`).

Execuções feitas agora, com `-B`/`PYTHONDONTWRITEBYTECODE`: c14 8/8, c13 10/10; mutante `relavra_tipo` falha na asserção `c14:207`; `ata_metadados` falha com `KeyError` em `c14:243`. Não há artefato gravado para esses resultados: [nao verificado].

## 12/11 11:25 · sessao c020ff845a76 · AUDITORIA-DE-TRAVAS-CORTE · censo · origem=Code

Regra: mantenho as regras 1–3 e o recorte da volta 1 (`medida_8_v1.r:3–5`). Aplico **A primeiro**: arquivo de algum teste vermelho alterado na mesma obra, com `run < commit ≤ pouso`. Depois **B**: nenhuma alteração elegível e **todos os IDs completos** repetidos em outras obras com distância absoluta ≤24 h. **C** é o restante. Associações de obra por pasta/assunto permanecem **[nao verificado]**. [nao verificado]

| Classe | Número | Linhas de `suites_14d.tsv` |
|---|---:|---|
| **A — MUDOU DECISAO** | **63** | `suites_14d.tsv:1,3,7,23–24,27,40,42,54,58,94,118,124,145,151–152,160,186–187,213,219,228,232,239,254,268–269,279,309,340,342,344,346,363–365,368,370,372,376,379,390,395–396,398,400–402,405,439,444–445,447,457–458,462,466,468,478,485,496–497,506` |
| **B — RUIDO** | **8** | `suites_14d.tsv:6,30,65,79,107,130,291,451` |
| **C — SEM SINAL** | **38** | `suites_14d.tsv:20,22,45,59,63,72,74–75,78,89–90,98,116,163,203,214,216–217,244,256,259,265,302,304,310,318,354,358,380,403–404,419,428,430,470,472,498–499` |

Correção da volta 1: **470 fica em C**. Seu vermelho de `test_selo_mapa_contrato` só reaparece fora da janela de 24 h nas outras obras identificadas (`suites_14d_falhas.tsv:30,79,114`; identidade das obras **[nao verificado]**).

Limite declarado: cura apenas em código de produção cai em **C**, não em A; a causa concreta dos casos C permanece **[nao verificado]**.

## 12/11 11:25 · sessao f2eb9b860cab · - · - · origem=Ronald (tmux)

(b) **Sem achado novo nos três sítios.** `_recusas_tela` nasce antes da seleção, entra no helper e sobrevive até o compositor; pré-apuração e `AtaAgregadaSemMarcos` usam esse helper (`app/ponto/services/espelho.py:486`, `:496`, `:498`, `:536`, `:565`, `:654`, `:655`, `:881`). A2 entrega a exceção no mapa e não conclui precedência nesse dia; overlay transporta o rótulo e `tipo_dia=None` (`app/ponto/precedencia.py:420`, `:423`, `:425`; `app/escala/utils.py:1218`).

**Sem except novo que engole.** Permanecem os genéricos antigos da seleção e da palavra; a captura explícita de `TipoSemFaixa` na composição relança (`app/ponto/services/espelho.py:574`, `:657`, `:660`; delta fornecido do arquivo). Falha HTTP integrada/ausência de outros 500: **[nao verificado]**; o handler chama o serviço diretamente (`app/ponto/views.py:157`).

**Fila — rótulo visível não garantido na UI.** Contraexemplo: ausência `fantasma_xyz` + batida em dia encerrado sem lavra dá título genérico; com `status_dia='ok'` previamente lavrado, mantém título antigo. D4 retorna antes de consultar recusas; HTML lê `status_rotulo` (`app/ponto/status_dia.py:290`, `:295`, `:317`; `app/templates/ponto/espelho.html:151`; caracterização `app/ponto/tests/test_lastro_c14_sem_banco.py:169`).

**Quem escreve/lê:**

- `recusas[data]`: `TipoSemFaixa` cria código/rótulo; A2 escreve mapa; overlay e espelho leem; D4 possui seu mapa e leitor (`app/ponto/precedencia.py:99`, `:423`; `app/escala/utils.py:1218`; `app/ponto/services/espelho.py:881`; `app/ponto/status_dia.py:301`, `:317`).
- `_recusa_do_dia`: espelho escreve; compositor lê e opcionalmente limpa; PDF chama esse mesmo compositor como último leitor (`app/ponto/services/espelho.py:881`, `:339`, `:349`; `app/relatorios/pdf_espelho.py:592`, `:594`).
- `palavra_dia`: inicializada vazia, preenchida pelo compositor comum; PDF lê para desenhar (`app/ponto/services/espelho.py:906`, `:340`, `:347`, `:1302`; `app/relatorios/pdf_espelho.py:879`). HTML da faixa usa outro campo (`app/templates/ponto/espelho.html:151`).

**Mordida real, com limites.** O ensaio executa seleção/handlers/helper reais e o valor real de `_recusa_do_dia`; builder e leitor/geometria são duplos; A2 posterior é propositalmente vazia para expor perda do primeiro transporte (`app/ponto/tests/test_lastro_c14_sem_banco.py:97`, `:103`, `:108`, `:113`, `:124`, `:136`, `:143`). Mutantes alteram AST produtivo do canal/reset, sem modificar arquivo (`app/ponto/tests/test_lastro_c14_sem_banco.py:24`, `:35`, `:39`). Executei quatro testes pertinentes: OK; canal removido deu dois erros e uma falha, reset deu três falhas. Registro persistido desta execução: **[nao verificado]**.

## 12/11 11:24 · sessao 89b671245bc2 · - · - · origem=Ronald (tmux)

Fatia `suites_14d.tsv:150–345`: **32 FAILED intermediárias**, com associação de obras por nome **[nao verificado]**.

| Classe | Total | Linhas de `suites_14d.tsv` |
|---|---:|---|
| A | 18 | 151, 152, 160, 186, 187, 213, 219, 228, 232, 239, 254, 268, 269, 279, 309, 340, 342, 344 |
| B | 1 | 291 |
| C | 13 | 163, 203, 214, 216, 217, 244, 256, 259, 265, 302, 304, 310, 318 |

A linha **279**, `jv3/suite_73a27d96_wip.log`, associa-se ao commit `790648d84`, assunto C20-JUIZ-3, pelo hash da suite seguinte; essa identidade de obra permanece **[nao verificado]**. O arquivo vermelho alterado é `app/ponto/tests/test_o_outbox_celula.py` **[nao verificado: número da linha no gitlog]**.

**309 é A; 310 é C**: a primeira run tem horário 05:37 e a cópia 05:55 (`suites_14d.tsv:309–310`); o commit `41e5dea51` que altera os três arquivos vermelhos tem horário 05:38 **[nao verificado: número da linha no gitlog; associação nominal]**.

Para **B291**, o ID completo `chamados.tests.test_contract_crons.ContratoPipelineB6Test.test_todo_command_tem_casa` também aparece na obra `f3_beco1`, 12h29 antes (`suites_14d.tsv:259`, `:291`; IDs em `suites_14d_falhas.tsv`, números das linhas **[nao verificado]**; distinção nominal das obras **[nao verificado]**).

## 12/11 11:24 · sessao d75940a3c4e3 · - · - · origem=Ronald (tmux)

Na fatia `suites_14d.tsv:346–526`, as 44 intermediárias FAILED dividem-se assim; associações entre obra e assunto **[nao verificado]**:

| Classe | Total | Linhas de `suites_14d.tsv` |
|---|---:|---|
| A | 31 | 346, 363–365, 368, 370, 372, 376, 379, 390, 395–396, 398, 400–402, 405, 439, 444–445, 447, 457–458, 462, 466, 468, 478, 485, 496–497, 506 |
| B | 1 | 451 |
| C | 12 | 354, 358, 380, 403–404, 419, 428, 430, 470, 472, 498–499 |

Correção da volta 1: **470 é C**. O ID vermelho de `test_selo_mapa_contrato` aparece somente em `suites_14d_falhas.tsv:30`, `:79` e `:114`; as outras duas ocorrências ficam fora de 24 horas da run 470 (`suites_14d.tsv:99`, `:358`, `:470`).

## 12/11 11:22 · sessao f5779ce1b0ab · - · - · origem=Ronald (tmux)

Nas linhas 1–149, são **33 INTERMEDIÁRIAS FAILED**, excluídas as FINAL e indeterminadas da volta 1; associações nominais **[nao verificado]**.

| Classe | Número | Linhas em `suites_14d.tsv` |
|---|---:|---|
| A | **14** | 1, 3, 7, 23, 24, 27, 40, 42, 54, 58, 94, 118, 124, 145 |
| B | **6** | 6, 30, 65, 79, 107, 130 |
| C | **13** | 20, 22, 45, 59, 63, 72, 74, 75, 78, 89, 90, 98, 116 |

Testemunhas A, todas posteriores à run e anteriores à FINAL candidata associada, conforme os anexos; números de linha do gitlog **[nao verificado]**:

| Linha da suite | Commit | Arquivo vermelho alterado | [nao verificado]
|---:|---|---|
| 1 | `caaf0e3ec`, 29/10 00:55 | `app/ponto/tests/test_passe_turno_aberto_retroativo.py` |
| 3 | `378481fd3`, 29/10 02:54 | `app/chamados/tests/test_contract_leitores_vitalidade.py` |
| 7 | `a1a19ce86`, 29/10 04:09 | `app/chamados/tests/test_contract_leitores_vitalidade.py` |
| 23 | `786a09283`, 29/10 17:33 | `app/escala/tests/test_ata_x_turno_entrada.py` |
| 24 | `786a09283`, 29/10 17:33 | `app/ponto/tests/test_a2_marco_morto.py` |
| 27 | `cbedeb91b`, 29/10 17:54 | `app/ponto/tests/test_selo_chokepoint_escrita.py` |
| 40 | `673ecaf79`, 30/10 12:17 | `app/api/tests/test_chamados_v2_destino.py` |
| 42 | `bfbc2aba7`, 30/10 13:04 | `app/chamados/tests/test_agrupado_em.py` |
| 54 | `8cb8a4c55`, 30/10 17:40 | `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py` |
| 58 | `4f0476aa5`, 30/10 19:51 | `app/chamados/tests/test_b2d_resposta_visivel.py` |
| 94 | `1d01bd905`, 31/10 07:39 | `app/ponto/tests/test_a2_marco_morto.py` |
| 118 | `973753a24`, 31/10 17:34 | `app/ponto/tests/test_contract_no_batida_date.py` |
| 124 | `f6f420ec3`, 31/10 23:54 | `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py` |
| 145 | `5ec1220ad`, 01/11 12:33 | `app/ponto/tests/test_cobparc_marco_no_periodo.py` |

Os vínculos `grupo1` + `grupo1p1` nas linhas 23–24 e `v3per` + `v3p5` na linha 124 seguem o agrupamento da volta 1 **[nao verificado]**.

## 12/11 11:21 · sessao 94df418df845 · LASTRO-FAIXA-SEM-DEFAULT · construtor · origem=Code

Curas, REDs, mutantes e comando para a sombra estão no [LASTRO_C14.txt](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/LASTRO_C14.txt:7).

**81 testes sem banco e ruff passaram**, com RC=0 ([saídas](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/evidencia_c14_validacao.txt:403)). [nao verificado]

A entrega permanece parcial: parei o ponto do selo de marca espúria porque sua premissa depende da grade própria, conforme sua condição literal ([pendência](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/LASTRO_C14.txt:36)). Os vizinhos Django não carregaram por falta da dependência ([saída](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/evidencia_c14_validacao.txt:109)).

Sem execução da sombra ou commit ([escopo](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/evidencia_c14_escopo.txt:35)). [nao verificado]

## 12/11 11:19 · sessao 49c101e1bfcf · AUDITORIA-DE-TRAVAS-CORTE · censo · origem=Code

Regra adotada, antes dos números:

1. Uma linha do TSV = uma run, incluindo cópias em `opus_final`. A numeração abaixo começa na primeira linha de cada anexo.
2. Associação pela pasta; separação por nome quando a pasta reúne obras diferentes, como E1/E2/E1-FILA em `fio_e0_v4`. Associações e agrupamentos por nome são **[nao verificado]**.
3. FINAL candidata = última linha com horário **estritamente anterior** ao pouso associado. Empate entre runs: última linha do TSV. Empate com o minuto do commit, ausência de pouso identificável ou execução posterior ao pouso: classificação temporal indeterminada **[nao verificado]**. [nao verificado]
4. FAILED intermediária: primeiro procurar alteração posterior do arquivo/módulo em commit da mesma obra; depois verificar se **todos os IDs completos** dos testes vermelhos aparecem em outras obras com distância absoluta ≤24 h; restante indeterminado. Coincidência não prova causalidade **[nao verificado]**. [nao verificado]

**O censo fecha por linha; o número de “mudaram decisão” não é verificável com o material fornecido.** O gitlog não informa arquivos alterados por commit, e o código atual não permite reconstruir essas alterações históricas **[nao verificado]**. [nao verificado]

| Classificação nominal | Total | FAILED | OK |
|---|---:|---:|---:|
| FINAL candidatas | 88 | 3 | 85 |
| INTERMEDIARIAS | 344 | 109 | 235 |
| Classificação temporal indeterminada | 94 | 28 | 66 |
| **Total** | **526** | **140** | **386** |

Fonte: `suites_14d.tsv:1–526`; classificação por associação de nomes **[nao verificado]**.

São **525 linhas com rótulo de pasta/obra e 1 SEM obra**, `integrador.log`. Rótulo de pasta não comprova identidade da obra **[nao verificado]**. Fontes: `suites_14d.tsv:1–526`, especialmente `suites_14d.tsv:526`.

As **94 temporalmente indeterminadas** são: 84 sem pouso identificável, oito empatadas no minuto do pouso, uma posterior ao pouso conhecido (`lastro2/construcao_green.log`) e uma cujo commit associado diz “cura”, sem pouso explícito (`o261/suite_bd0aef09.log`) **[nao verificado]**.

Para classificar cada linha sem reproduzir os 526 registros: estas são as **88 FINAL candidatas**; a lista seguinte contém as 94 temporalmente indeterminadas; **todas as demais linhas são as 344 INTERMEDIARIAS**, segundo a regra acima **[nao verificado]**.

| Data | Linhas FINAL candidatas em `suites_14d.tsv` |
|---|---|
| 29/10 | 2, 19, 21, 29, 32 |
| 30/10 | 39, 50, 56, 60, 71 |
| 31/10 | 83, 84, 88, 92, 93, 97, 99, 108, 112, 114, 119, 123, 138 |
| 01/11 | 141, 149, 159 |
| 02/11 | 170, 173, 181, 183, 185, 194 |
| 03/11 | 199 |
| 04/11 | 204, 207, 208, 210–212, 215, 218, 222, 224–225 |
| 05/11 | 235, 238, 248, 251–252, 260, 263–264, 273, 275, 277, 280, 283, 289 |
| 06/11 | 303, 312–313, 319–320, 322, 330–331, 335, 343 |
| 07/11 | 353, 357, 378, 389 |
| 08/11 | 407, 437 |
| 09/11 | 443, 446, 448, 456, 465, 469, 471 |
| 10/11 | 474, 477, 487 |
| 11/11 | 502–503, 509, 516 |

As três FINAL candidatas FAILED são ORFAS, O261-V e LASTRO-3 (`suites_14d.tsv:19`, `suites_14d.tsv:71`, `suites_14d.tsv:313`); associação aos pousos **[nao verificado]**.

As 94 linhas temporalmente indeterminadas, em `suites_14d.tsv`, são **[nao verificado: associação ao pouso]**: [nao verificado]

```text
33–34, 36–38, 57, 61–62, 73, 96, 100, 102, 150, 171,
174–180, 191, 193, 198, 200–201, 209, 226–227, 243, 253,
284, 287, 290, 292–296, 298–300, 305–308, 315, 317,
323–326, 332–334, 336, 339, 366, 409, 449–450, 453–454,
460, 463–464, 480, 488–495, 504–505, 508, 510–515, 517–526
```

**Das 109 INTERMEDIARIAS FAILED, nenhuma pode ser atribuída com comprovação a “mudou decisão” ou a “ruído exclusivo”.** Todas permanecem indeterminadas quanto a essa divisão **[nao verificado]**.

Dentro delas, **17 atendem ao critério observável de coincidência integral em outras obras em até 24 h**: linhas 6, 30, 65, 79, 107, 130, 291, 365, 368, 439, 447, 451, 466, 468, 470, 478 e 506 de `suites_14d.tsv`. Os IDs constam de `suites_14d_falhas.tsv:3`, `:11`, `:20`, `:26`, `:31`, `:35`, `:63`, `:82–83`, `:103`, `:106`, `:108`, `:112–114`, `:116` e `:132`; distinção entre obras **[nao verificado]**.

Essas 17 são **candidatas a ruído**, pois falta excluir alteração posterior na própria obra. As outras **92** não preencheram esse critério integral; isso também não demonstra cura **[nao verificado]**.

Das OK, são **235 intermediárias**, sem mudança de decisão pela definição solicitada. As **85 OK escolhidas como últimas antes do pouso** estão entre as FINAL candidatas; outras **66 OK** têm classificação temporal indeterminada (`suites_14d.tsv:1–526`; associação **[nao verificado]**). [nao verificado]

| Tabela final solicitada | Resultado |
|---|---:|
| Total | **526** |
| Intermediárias na reconstrução nominal | **344** **[nao verificado]** |
| Intermediárias que mudaram decisão | **[nao verificado] — 0 comprovadas** |
| Ruído exclusivo | **[nao verificado] — 0 comprovadas** |
| FAILED intermediárias indeterminadas | **109**, incluindo 17 candidatas a ruído |
| Runs sem classificação FINAL/INTERMEDIARIA resolvida | **94** **[nao verificado]** |

Fonte das contagens: `suites_14d.tsv:1–526` e `suites_14d_falhas.tsv:1–140`. “0 comprovadas” expressa ausência de comprovação, **não zero ocorrências reais [nao verificado]**.

As dez obras com mais intermediárias **na reconstrução nominal**, excluídas as 94 linhas temporalmente indeterminadas, são **[nao verificado: associações por nome]**:

| Obra/agrupamento adotado | Intermediárias | Fonte em `suites_14d.tsv` |
|---|---:|---|
| FIO-E0-V4-E1 | 61 | `:346–437` |
| FIO-E0-V4-E2 | 14 | `:358–448` |
| COBPARC | 13 | `:142–170` |
| GRADE-FOLGA-JUIZ (`grade_folga` + `grade_juiz`) | 13 | `:106–149` |
| O261-E-F5C (`F5` + `F5C`) | 13 | `:63–88` |
| JUIZ-VARREDURA-1 | 10 | `:228–264` |
| O261-VETO-A | 10 | `:40–50` |
| ORFAS | 10 | `:3–19` |
| V3-P5 (`v3per` + `v3p5`) | 10 | `:124–141` |
| V2-BUGS-DO-MOTOR, item 2 | 9 | `:338–357` |

O ranking definitivo e a quantidade causal pedida no item 8 permanecem **[nao verificado]**, pelas associações nominais, pelas 94 posições temporais não resolvidas e pela ausência dos arquivos alterados por commit. [nao verificado]

## 12/11 11:15 · sessao 78f2590fba58 · - · - · origem=Ronald (tmux)

Mapa consolidado: **88 FINAL estritas: 85 OK e 3 FAILED**; **94 com classificação temporal indeterminada**; sobre 526 linhas, restam **344 INTERMEDIÁRIAS**. Todas as associações obra↔pasta/pouso são por nome **[nao verificado]**. [nao verificado]

Os três FAILED finais são `orfas/suite_inteira_6.log` (10-29 08:53), `o261v/suite3_9c9e030e.log` (10-30 23:07) e `lastro3/suite_v3.log` (11-06 09:00). **[nao verificado: associação por nome]**

As 94 linhas temporalmente indeterminadas compreendem:

- 84 linhas em 25 grupos sem pouso identificável posterior. [nao verificado]
- 8 linhas empatadas no minuto do pouso: `o261v`, `medida_programa`, `o261e/d3`, `disjuntor`, `e3solo`, `mc`, `jv2p`, `h12h06b/pauta08`. [nao verificado]
- `lastro2/construcao_green.log`, posterior ao pouso conhecido.
- `o261/suite_bd0aef09.log`, cuja associação depende de tratar um commit “cura” como pouso. **[nao verificado]**

Para as duas linhas `lastro2/.../suite_v2.log` de 11-06 05:55, a FINAL única usa a última linha do TSV, `lastro2/suite_v2.log`; a ordem real entre ambas é **[nao verificado]**.

A lista completa das 88 FINAL e os hashes dos pousos foi enviada nas mensagens anteriores.

## 12/11 11:15 · sessao 628c0cf32473 · - · - · origem=Ronald (tmux)

Não há `.git` nem anexos locais nesta cópia; não foi possível verificar arquivos alterados por commit. O gitlog fornecido contém assunto, data e hash, sem diff. Portanto, “mudou decisão” e “ruído exclusivo” não são quantificáveis com comprovação. [nao verificado] [nao verificado]

Coincidências integrais candidatas a ruído, contando IDs completos de teste e distância absoluta ≤24 h: [nao verificado]

| Linhas em `suites_14d_falhas.tsv` | Contrapartes |
|---|---|
| 3, 5, 11 | Entre si: diagrama |
| 20 | 18: import resolve |
| 26 | 18: diagrama |
| 31 | 26 ou 35: diagrama |
| 35 | 31 ou 37: diagrama |
| 63 | 57 ou 73: cron |
| 69 | 77: chokepoint |
| 82 | 83: signal âncora |
| 83 | 80 ou 82: signal âncora |
| 103 | 105; alternativamente 112 para `classificar` e 104 para `linhas` |
| 106, 107, 108 | 103–105 e entre si: catraca linhas |
| 112, 113 | 103 ou 105: classificar |
| 114 | 109: diagrama e mapa |
| 116 | 121, 124, 127 ou 132: catraca linhas |
| 132 | 116, 121, 124, 127 ou 135: catraca linhas |

São **20 candidatas**, separando E1/E2/FILA dentro de `fio_e0_v4`; **18** se usar somente pasta raiz. Todas as separações de obra por nome são [nao verificado]. Referências: linhas indicadas de `suites_14d_falhas.tsv`.

Duas candidatas parecem FINAL FAILED, devendo sair das intermediárias: linha 5, `orfas/suite_inteira_6.log`, antes de `ac7216078` em 29/10 09:24; linha 69, `lastro3/suite_v3.log`, antes de `24946096d` em 06/11 09:43. Associação por nome [nao verificado]. Isso deixa **18 candidatas intermediárias** com divisão E1/E2/FILA, ou **16** com pasta raiz.

A precedência “cura antes de ruído” continua impossível de verificar: as candidatas podem também ter recebido alterações posteriores na própria obra. A classificação estrita deve manter todo FAILED intermediário como indeterminado e apresentar as coincidências como subtotal observável. [nao verificado]

O código atual permite resolver módulos, por exemplo `app/core/tests/test_medida_turno_do_par.py:544`, `app/core/tests/test_medida_turno_do_par.py:550` e `app/core/tests/test_selo_diagrama_do_codigo.py:42`; ele não fornece o histórico de alterações. [nao verificado]

Há inconsistência na linha 140 do anexo de falhas: veredito `failures=5`, mas lista extensa de FAIL e dois ERROR. Conta como uma linha/run; composição histórica dessa lista [nao verificado].

## 12/11 11:14 · sessao ae97398abee9 · - · - · origem=Ronald (tmux)

Censo por linha do anexo: **526 runs = 140 FAILED + 386 OK; 111 pastas/raízes distintas**, incluindo `integrador.log` como raiz sem pasta. Fontes: `suites_14d.tsv:1–526`, `suites_14d_falhas.tsv:1–140` (numeração do anexo textual).

Formato abaixo: `pasta total/FAILED`; OK = total − FAILED. Fonte de todas as contagens: `suites_14d.tsv:1–526`.

```text
fio_e0_v4 83/27
o261e 40/13
cobparc 14/3
o261 12/3
orfas 11/3
e4cal 11/2
jv1 11/2
c20_fiomudo 11/2
v2bugs 10/2
grade_juiz 9/1
BO-0611-05 9/2
col129 8/2
v3p5 8/0
f3_beco1 8/3
dme 7/0
otimiza3 7/0
FIO-E0-V4-E3 7/4
calendario_hoje 6/1
cobparc_relogio 6/1
jv3 6/2
jv4 6/0
h12h06 6/1
h12h06b 6/1
fio_e3 6/3
BO-DISPUTA-SEM-PERGUNTA 6/6
E4B-ALCANCE 6/4
grupo1 5/3
v3retoma 5/0
grade_folga 5/1
f1c4emis 5/1
lastro2 5/2
esemvivab 5/0
LASTRO-FAIXA 5/5
justif 4/2
o261n 4/1
o261v 4/2
vfase_asof 4/0
mc 4/1
n1im 4/1
f346 4/0
plantio_mais1 4/3
motor_turno 4/0
par_nao_adjacente 4/1
selos_canon 4/1
FIO-E0-V4-E4-ESCRITORES 4/2
FIO-E0-V4-E4A-2 4/1
disjuntor 3/1
e4_7iii 3/1
v3per 3/1
lenvivos 3/0
toast 3/0
e4app 3/2
juizvarredura2 3/0
n1alim 3/1
snc2 3/1
pep3 3/1
o10c 3/0
codexneg 3/0
cxr_tela 3/1
UI-PAINEL 3/1
DATA-EVENTO-CRU 3/0
TRIAGEM-OFFLINE-SEM-CAMERA 3/0
outbox 2/1
grupo1p1 2/0
suite_impacto 2/1
medida_programa 2/0
e3status 2/1
comboio 2/0
e3isento 2/0
cobparc_rotulo 2/0
isento_h10 2/0
noreverse 2/0
n1pna 2/2
he 2/0
cronsmarca 2/0
c20_o169 2/0
pep 2/2
akmed 2/0
ferias2x 2/1
e1fila_v22 2/1
CODEX-2-RAIAS 2/0
MODO-COMUM-PASSADO-SEM-TURNO 2/1
O169-FECHADA 2/0
COMBOIO-1556 2/1
fioadmin 1/0
e3solo 1/0
reprova_desenho 1/0
instr 1/0
comboio_instr 1/0
e4_comboio 1/0
contencao 1/0
cods_ato 1/0
e4_8 1/0
e4esp 1/0
cron15 1/0
sw 1/0
jv2p 1/0
sombra_c1 1/0
lastro3 1/1
fiomudob 1/0
pep5 1/0
o10b 1/0
r1pub 1/0
comboio_0611 1/0
esminst 1/0
ata_pausa_0811 1/1
auditoria_0911 1/0
REVERTER-DIA-SEM-RECALCULO 1/0
FIO-E0-V4-E4B 1/1
BO-ROTULO-ENCERRADO 1/0
integrador.log 1/1
```

Separação solicitada por nome de arquivo — identidade de obra [nao verificado]:

| Pasta | Stream inferido | Runs | FAILED | OK |
|---|---|---:|---:|---:|
| fio_e0_v4 | E1 | 62 | 20 | 42 |
| fio_e0_v4 | E2 | 15 | 6 | 9 |
| fio_e0_v4 | E1FILA | 6 | 1 | 5 |
| o261e | F1 | 4 | 4 | 0 |
| o261e | D3 | 1 | 0 | 1 |
| o261e | F5 | 8 | 2 | 6 |
| o261e | F5C | 6 | 1 | 5 |
| o261e | F1C4 | 5 | 2 | 3 |
| o261e | F1C4-3 | 3 | 1 | 2 |
| o261e | F2 | 9 | 2 | 7 |
| o261e | F2FILA | 3 | 1 | 2 |
| o261e | dj → DISJUNTOR | 1 | 0 | 1 |

E2 inclui 11 `green_e2*` em 07/11, `e2_grade_recorte` em 08/11 e três `e2_rebase` em 09/11. E1FILA inclui quatro `e1_fila*` em 08/11 e dois em 09/11. Fonte: `suites_14d.tsv:347–448`. Associações [nao verificado].

Há **10 linhas em `opus_final`** com Ran/tempo/veredito repetidos fora da subpasta: 3 FAILED e 7 OK; mantidas na contagem. Distribuição: jv3 1, c20_fiomudo 4, pep 1, otimiza3 2, lastro2 2. Fonte: `suites_14d.tsv:282–312`; identidade física da execução [nao verificado].

Faixas do anexo úteis para citações:

```text
29/10: 1–33
30/10: 34–74
31/10: 75–139
01/11: 140–168
02/11: 169–198
03/11: 199
04/11: 200–228
05/11: 229–289
06/11: 290–346
07/11: 347–397
08/11: 398–441
09/11: 442–473
10/11: 474–490
11/11: 491–520
12/11: 521–526
```

## 12/11 11:06 · sessao e7500f65d2e9 · AUDITORIA-DE-TRAVAS-CORTE · censo · origem=Code

Numerei cada anexo desde a primeira linha de dados. Usei **p50 = mediana** e **p95 = valor na posição `ceil(0,95 × n)`**, após ordenar.

**Item 6 — chamadas do wrapper**

“Total” abaixo significa últimas 20 chamadas globais; leitura/censo/sonda formam um grupo.

| Janela | n | p50 (s) | p95 (s) | Máximo (s) | rc=124 | >900 s | Fonte |
|---|---:|---:|---:|---:|---:|---:|---|
| Construtor | 20 | 715,5 | 1.630 | 1.928 | 0 | 7 | `chamadas_fim_vivo.tsv:316–395`, filtrando papel |
| Verificador | 20 | 325,5 | 594 | 656 | 0 | 0 | `chamadas_fim_vivo.tsv:335–400`, filtrando papel |
| Leitura/censo/sonda | 20 | 204,5 | 891 | 1.178 | 0 | 1 | `chamadas_fim_vivo.tsv:351–399`, filtrando grupo |
| Total global | 20 | 332 | 1.068 | 1.928 | 0 | 2 | `chamadas_fim_vivo.tsv:381–400` |
| União das três janelas | 60 | 386,5 | 1.206 | 1.928 | 0 | 8 | Mesmas seleções acima |

No anexo inteiro, os **três rc=124** são: construtor/904 s, construtor/905 s e censo/905 s; estão fora dessas janelas. (`chamadas_fim_vivo.tsv:66,130,317`)

| Suites | n | p50 (s) | p95 (s) | Máximo (s) | Fonte |
|---|---:|---:|---:|---:|---|
| Últimas 20 linhas `Ran` | 20 | 1.306,5525 | 1.638,001 | 1.788,073 | `suites_14d.tsv:507–526` |
| Últimas 20 aparentemente completas¹ | 20 | 1.316,237 | 1.638,001 | 1.788,073 | `suites_14d.tsv:500–525`, seleção por nome/tamanho |

¹ A classificação como completa usa o nome do log e o número de testes; a fase efetiva da execução não consta do anexo. [não verificado] [nao verificado]

O **timeout de 900 s do wrapper deve ser avaliado pelas chamadas Codex**. Ele envolve `codex exec`, tanto no construtor quanto nos demais papéis; o construtor recebe `-k 30`. Nesta cópia, os locais são `bin/codex_pergunta.sh:401` e `:408`, e o default permanece em `:83`. O próprio cabeçalho atribui a certificação por suite ao Code. (`bin/codex_pergunta.sh:331`)

Há uma ressalva na medida: `duracao_s` começa antes da preparação e da espera por vagas/locks. Portanto, **passar de 900 s não demonstra que o `codex exec` excedeu seu timeout**, nem revela o `CODEX_TIMEOUT` efetivamente usado naquela chamada. (`bin/codex_pergunta.sh:252,260,348–376`; valor efetivo por chamada: [não verificado])

Outro teto de **900 s** existe em **`bin/encadeado.sh:71`**: espera carimbos de conclusão de cron e sai com rc=2 quando vence; não é timeout de suite nem de Codex. (`bin/encadeado.sh:84–96`)

As contas das margens são:

| Cobertura pretendida | Conta `p95 + margem` | Teto empírico | Fonte |
|---|---|---:|---|
| Últimas 20 suites | `1.638,001 + (1.788,073 − 1.638,001)` | **1.788,073 s → 1.789 s inteiros** | `suites_14d.tsv:514,524` |
| Últimos 20 construtores | `1.630 + (1.928 − 1.630)` | **1.928 s** | `chamadas_fim_vivo.tsv:365,386` |
| Wrapper cobrindo também o maior rc=0 de todo o anexo | `1.630 + (2.806 − 1.630)` | **2.806 s** | `chamadas_fim_vivo.tsv:214,365` |

**Proposta empírica para o wrapper: 2.807 s**, isto é, `1.630 + 1.177`; a margem é `(2.806 − 1.630) + 1 s`, ficando estritamente acima do máximo registrado, com resolução inteira. Usar apenas a janela recente daria 1.929 s pelo mesmo critério. (`chamadas_fim_vivo.tsv:214,365,386`)

Isso cobre os máximos de **duração registrada**; a afirmação de que algum dos três rc=124 foi falso, ou de que esse teto evitaria falsos rc=124 futuros, permanece **[não verificado]**.

**Item 8 — “mudaram decisão”: não medido; não significa zero.**

Há **526 registros `Ran`, 140 deles FAILED**. O anexo informa totais de falhas, mas não identifica os testes; também não marca sistematicamente intermediária/final. Há possíveis cópias em diretórios `opus_final`, impedindo equiparar linhas a execuções independentes. (`suites_14d.tsv:1–526`; exemplos de cópias: `281–282,302–304,309–312`) [nao verificado]

O caso E3 mostra o limite: a suite tem sete falhas, um commit posterior diz “pós-suite: S1–S7 curados”, depois aparecem três falhas e finalmente verde. O título truncado não identifica os casos da execução nem fecha a prova pedida até a final. (`suites_14d.tsv:497–500`; `gitlog_14d.txt:59,30`) [nao verificado]

A tabela abaixo cobre **todos os agrupamentos com algum FAILED**. **NM = não medido** em ambas as colunas solicitadas. Os números entre parênteses são o censo verificável: **registros / FAILED**. Minutos são a soma bruta de `Ran ... in Xs ÷ 60`, **não minutos comprovados de intermediárias ou de economia**. Não tratei a última observação como final. (`suites_14d.tsv:1–526`)

| Obra/prefixo do anexo | Intermediárias (registros/FAILED) | Mudaram decisão | Minutos brutos | Linhas em `suites_14d.tsv` |
|---|---:|---|---:|---|
| outbox | NM (2/1) | NM | 18,640 | 1–2 |
| orfas | NM (11/3) | NM | 119,192 | 3–19, prefixo |
| col129 | NM (8/2) | NM | 93,913 | 6–21, prefixo |
| grupo1 | NM (5/3) | NM | 72,541 | 22–26 |
| justif | NM (4/2) | NM | 59,898 | 27,30–32 |
| o261n | NM (4/1) | NM | 33,198 | 34,36–38 |
| o261v | NM (4/2) | NM | 63,096 | 35,59,71,73 |
| o261/veto_a2 | NM (11/3) | NM | 166,253 | 40–50 |
| calendario_hoje | NM (6/1) | NM | 85,975 | 51–56 |
| suite_impacto — gerações distintas | NM (2/1) | NM | 49,138 | 57,226 |
| o261e, exceto f2/f2fila | NM (28/10) | NM | 327,878 | 58–97, prefixo |
| disjuntor | NM (3/1) | NM | 44,711 | 78,84,96 |
| e3status | NM (2/1) | NM | 30,745 | 98–99 |
| grade_folga | NM (5/1) | NM | 69,002 | 106,111,116,120,122 |
| f1c4emis | NM (5/1) | NM | 84,507 | 107,109,113,117,119 |
| e4_7iii | NM (3/1) | NM | 43,106 | 118,121,123 |
| v3per | NM (3/1) | NM | 39,316 | 124–126 |
| grade_juiz | NM (9/1) | NM | 150,402 | 127–149, prefixo |
| cobparc | NM (14/3) | NM | 208,564 | 142–170, prefixo |
| o261e/f2 | NM (9/2) | NM | 108,488 | 151–159 |
| cobparc_relogio | NM (6/1) | NM | 79,354 | 174–179 |
| e4cal | NM (11/2) | NM | 168,778 | 186–199, prefixo |
| mc | NM (4/1) | NM | 69,594 | 203,205,207,209 |
| e4app | NM (3/2) | NM | 36,395 | 213–215 |
| n1pna | NM (2/2) | NM | 41,197 | 216–217 |
| n1im | NM (4/1) | NM | 40,556 | 219–222 |
| jv1 | NM (11/2) | NM | 169,889 | 228–264, prefixo |
| n1alim | NM (3/1) | NM | 28,680 | 232–233,235 |
| jv3 | NM (6/2) | NM | 93,137 | 239,241,279,281–283 |
| o261e/f2fila | NM (3/1) | NM | 41,743 | 244,246,248 |
| f3_beco1 | NM (8/3) | NM | 152,549 | 256–275, prefixo |
| h12h06 | NM (6/1) | NM | 99,348 | 265–280, prefixo |
| snc2 | NM (3/1) | NM | 37,106 | 268,271,273 |
| c20_fiomudo | NM (11/2) | NM | 119,175 | 284–300, prefixo |
| h12h06b, incluindo pauta08 | NM (6/1) | NM | 103,853 | 291,297,301,303,331–332 |
| pep | NM (2/2) | NM | 34,120 | 302,304 |
| lastro2 | NM (5/2) | NM | 76,021 | 309–312,508 |
| lastro3 | NM (1/1) | NM | 17,170 | 313 |
| pep3 | NM (3/1) | NM | 26,640 | 314,316,318 |
| plantio_mais1 | NM (4/3) | NM | 63,081 | 333–334,336,339 |
| v2bugs/i2 | NM (10/2) | NM | 82,524 | 338–357, prefixo |
| ferias2x | NM (2/1) | NM | 36,728 | 342–343 |
| fio_e0_v4 — mistura E1/E2/fila/rebase | NM (83/27) | NM | 1.055,536 | 346–448, prefixo |
| cxr_tela | NM (3/1) | NM | 64,876 | 405–407 |
| ata_pausa_0811 | NM (1/1) | NM | 19,673 | 409 |
| e1fila_v22 | NM (2/1) | NM | 38,836 | 444,446 |
| par_nao_adjacente | NM (4/1) | NM | 87,109 | 449–450,453–454 |
| selos_canon | NM (4/1) | NM | 82,189 | 451–452,455–456 |
| fio_e3 — 09/11 | NM (6/3) | NM | 124,964 | 457–465, prefixo |
| FIO-E0-V4-E4-ESCRITORES | NM (4/2) | NM | 78,317 | 466–469 |
| MODO-COMUM-PASSADO-SEM-TURNO | NM (2/1) | NM | 39,387 | 470–471 |
| FIO-E0-V4-E4A-2 | NM (4/1) | NM | 79,135 | 472–473,476–477 |
| BO-0611-05 | NM (9/2) | NM | 132,060 | 478–487, prefixo |
| BO-DISPUTA-SEM-PERGUNTA | NM (6/6) | NM | 112,739 | 480,488,490–491,493,495 |
| FIO-E0-V4-E4B | NM (1/1) | NM | 3,754 | 489 |
| E4B-ALCANCE | NM (6/4) | NM | 128,479 | 492,494,504–505,515,519 |
| FIO-E0-V4-E3 — 11/11 | NM (7/4) | NM | 161,441 | 496–502 |
| UI-PAINEL | NM (3/1) | NM | 69,216 | 506–507,516 |
| COMBOIO-1556 | NM (2/1) | NM | 49,174 | 513–514 |
| LASTRO-FAIXA | NM (5/5) | NM | 98,717 | 521–525 |
| integrador | NM (1/1) | NM | 6,606 | 526 |

**Falta, para cada linha NM:** identificar a execução e sua fase, os testes que ficaram vermelhos, o commit que curou esses mesmos testes e a execução final que confirma a cura. A correspondência entre prefixos e obras também precisa fechar onde há agrupamentos mistos. [não verificado] [nao verificado]

Além disso:

- **BO-DISPUTA e LASTRO-FAIXA:** a última observação ainda é FAILED; falta a final. (`suites_14d.tsv:495,525`)
- **FIO-E4B ↔ E4B-ALCANCE:** falta comprovar a associação entre esses nomes. [não verificado]
- **jv3, c20_fiomudo, pep e lastro2:** falta distinguir cópia de log de execução independente. (`suites_14d.tsv:281–282,287,292–295,298–299,302,304,309–312`)
- **Demais prefixos:** seus registros não contêm FAILED; isso não identifica quantas execuções foram intermediárias nem comprova a final. (`suites_14d.tsv:1–526`, excluindo os agrupamentos da tabela)

Assim, **140 linhas FAILED não podem ser convertidas em 140 suites intermediárias que mudaram decisão**. O número solicitado no item 8 permanece **não medido** pelos anexos fornecidos. (`suites_14d.tsv:1–526`; vínculo causal: [não verificado])

## 12/11 11:05 · sessao f9aecd196290 · - · - · origem=Ronald (tmux)

Proposta concreta, sem escrita:

1. Use builder completo, sem reaproveitar `julgar_sem_adesao`: esse harness força ausência de batidas e testa outro caminho (`ponto/tests/test_lastro_c5_sem_banco.py:566-567`).

2. O builder completo mais curto é `montar_grade_prevista`: aceita escala/horários e batidas em memória; executa pareamento, cobertura e classificação para folga (`escala/utils.py:668`, `escala/utils.py:805-814`). Limite: esse builder não publica minutos previstos/realizados; não atribuir a ele uma prova financeira positiva (`escala/utils.py:791`). Para provar esses valores, usar o builder por turno (`escala/utils.py:1301`).

3. Crie célula em memória com ata antiga `tipo_dia='ausencia'`; rode builder com ausência `folga` integral e batida. A cobertura deve produzir `tipo_dia='folga'`, transportar ausência e limpar órfãs/células, preservando a batida de entrada externa (`escala/utils.py:1243-1254`, `escala/utils.py:1275-1277`).

4. Execute `ata_do_dia`, depois a porta real por AST: `lavrar_veredito`, `veredito_que_a_lavra_escreve`, `_status_alvo`, `_aplicar_status`; substitua somente emissão/persistência por duplos. Assert obrigatório: retorno contém `ata`, ata mudou de ausência para folga e metadados sobreviveram (`ponto/services/cartorio.py:721-768`, `ponto/portas/celula.py:912`, `ponto/portas/celula.py:951-1054`). Ler esse arquivo não exige editá-lo.

5. Para provar leitores: `grade_do_fechamento` real → `grade_da_celula` real → `por_dia_da_grade` real → bloco do fechamento que publica mapas e causa. Execute `_retidos` real de `simular_folha` com esse fechamento e `prontidao` real; isso demonstra o leitor do simulador, com integração completa/motor/TXT ainda `[nao verificado]` (`folha/export.py:329-338`, `escala/services/leitor_celula.py:397-438`, `ponto/services/fechamento.py:1014-1085`, `folha/management/commands/simular_folha.py:68-93`).

Dois pontos que precisam constar do relato:

- Batida isolada não implica `dias_trocados`: esse ramo exige órfã e condições anteriores; cobertura integral limpa órfãs (`folha/causas.py:92-106`, `escala/utils.py:1275-1277`).
- Célula `trabalha=True`, mesmo com ata relavrada para folga, fica fora do tripwire de ata fantasma (`ponto/management/commands/tripwire_celulas.py:338-342`).

## 12/11 11:05 · sessao 8f90fa80b83a · - · - · origem=Ronald (tmux)

Mapeamento somente leitura; nenhum arquivo escrito.

- Curas c13 mantidas: transporte `recusas` nas seis costuras dos builders e tratamento no overlay, preservando geometria e removendo conclusão de faixa (`escala/utils.py:807`, `:1009`, `:1018`, `:1205`, `:1212`, `:1218`, `:1872`, `:2112`, `:2364`). Harness de pacotes previamente carregados (`ponto/tests/test_lastro_c12_sem_banco.py:87`).
- RED histórico executado: `--red-c12` com os três testes `LastroC13.test_grade_recusa_nomeada_preserva_orfa_e_vizinhos_sem_inventar_faixa`, `test_recusa_nao_depende_de_cobertura_aprovada_nem_de_quem_a_venceu` e `test_canal_atravessa_dispatcher_e_costuras_dos_dois_builders`: rc 1, `FAILED (errors=6)`, por ausência do argumento `recusas` na c12 (`ponto/tests/test_lastro_c13_sem_banco.py:39`).
- Nove mutantes executados, todos rc 1: `recusa_apagada`, `recusa_gate`, `recusa_faixa`, `canal_grade`, `canal_dispatcher`, `canal_turno`, `canal_legacy`, `canal_single`, `canal_overlay` (`ponto/tests/test_lastro_c13_sem_banco.py:49`).
- `--mutante harness_c12` executa o `setUp` histórico exato. Dois testes API falharam: `busca real proibida` e `grade real proibida`, rc 1 (`ponto/tests/test_lastro_c13_sem_banco.py:26`, `:89`).

Comando comum:

```sh
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.test_lastro_c13_sem_banco --mutante NOME TESTE -v [nao verificado]
```

Selos vizinhos podem exigir delegação a `espelho_do_colab`, consumo de `orfas_grade` e proibição da grade própria no PDF, preservando as exigências financeiras (`escala/tests/test_f1c_contrato.py:42`; `relatorios/tests/test_cartao_pela_celula.py:170`).

Há incompatibilidade concreta na fixture de marca espúria: cria vínculo e batidas, sem `CelulaDia`; a tela entrega `sem_celula`, e o adaptador retorna órfãs vazias sem marcos (`relatorios/tests/test_cartao_marca_espuria.py:33`; `escala/services/leitor_celula.py:394`; `escala/utils.py:1087`). Comparar apenas vazios perderia a mordida positiva. Para preservá-la, a fixture precisa entregar tela com célula/ata/lâmpadas/órfã e manter tanto a marca quanto a supressão do turno aberto e a prova textual do PDF (`relatorios/tests/test_cartao_marca_espuria.py:55`, `:70`).

_4200 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
