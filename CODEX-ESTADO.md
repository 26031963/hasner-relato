# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 18:10** · publicado em **13/11/2026 18:10** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 18:10 · sessao 380f2191aded · - · - · origem=Ronald (tmux)

**LEU: o ponto do complemento não existe ainda no HEAD.** `vigiar_fio_morto.py:176` é `with tranca_em_lote()`, e o caminho chama `censo_alcance` em `:178`; o censo usa `alimentar_portas` em `vigia_fio.py:609` e `porta_alcancavel_do_dia` em `:640`. AST encontrou **zero** chamadas `carga_da_tarefa` dentro de `censo_alcance` (`app/chamados/services/vigia_fio.py:542`). Assim, o último leitor da carga no caminho **pós-E3** permanece **[nao verificado]**; não se deve transplantar liberação pela linha antiga do anexo. O registro vivo afirma que os cinco respondedores seguem vivos até E3 e nenhum leitor migrou em E2 (`app/core/juizes.py:150`; `app/docs/JUIZES.md:85`).

**Origem das provas locais do complemento:** backlog registra literalmente pós-E3, sem construção agora, soltar carga, DIFF zero e RSS esperado <=1500 (`app/docs/BACKLOG.md:738`); o RELATO registra os números RSS fornecidos no anexo (`app/docs/RELATO.md:14`) e o recebimento do complemento sem mudar E3 (`app/docs/RELATO.md:5`). Reproduzir esses números no HEAD ou atribuir o pico atual à carga permanece **[nao verificado]**.

**Censo AST executado, produção Python, sem importar app** (busca sintática por nomes; `_carimbar` de `aplicar_janela_he_total` foi falso homônimo, excluído manualmente após leitura; `app/ponto/management/commands/aplicar_janela_he_total.py:75`):

```text
app/chamados/juizes.py:1576 CALL alimentar_fila
app/chamados/juizes.py:1656 CALL alimentar_portas
app/chamados/management/commands/backfill_fio_morto_e4b.py:78 CALL censo_alcance
app/chamados/management/commands/backfill_fio_morto_e4b.py:87 CALL T.tarefa_do_dia
app/chamados/management/commands/diff_e3_sombra.py:114 CALL alimentar_portas
app/chamados/management/commands/diff_e3_sombra.py:115 CALL carga_da_tarefa
app/chamados/management/commands/diff_e3_sombra.py:125 CALL tarefa_do_dia
app/chamados/management/commands/vigiar_fio_morto.py:132 CALL caminho_do_carimbo
app/chamados/management/commands/vigiar_fio_morto.py:178 CALL censo_alcance
app/chamados/management/commands/vigiar_fio_morto.py:232 CALL _carimbar
app/chamados/services/tarefa_do_dia.py:569 CALL carga_da_tarefa
app/chamados/services/tarefa_do_dia.py:529 CALL alimentar_fila
app/chamados/services/vigia_fio.py:609 CALL alimentar_portas
app/chamados/services/vigia_fio.py:640 CALL porta_alcancavel_do_dia
app/core/management/commands/pipeline_placar.py:92 CALL caminho_do_carimbo
app/escala/services/cadastro_realidade.py:456 CALL alimentar_portas
app/escala/services/cadastro_realidade.py:460 CALL porta_alcancavel_do_dia
app/folha/fora_da_folha.py:62 CALL alimentar_portas
app/folha/fora_da_folha.py:67 CALL porta_alcancavel_do_dia
```

O script usado para essa parte foi:

```python
import ast
from pathlib import Path

wanted = {
    "carga_da_tarefa", "tarefa_do_dia", "_carimbar",
    "caminho_do_carimbo", "censo_alcance",
    "alimentar_portas", "alimentar_fila", "porta_alcancavel_do_dia",
}
for p in sorted(Path("app").rglob("*.py")):
    if "tests" in p.parts or p.name.startswith("test"):
        continue
    try:
        tree = ast.parse(p.read_text())
    except (SyntaxError, UnicodeError):
        continue
    for node in ast.walk(tree):
        if not isinstance(node, ast.Call):
            continue
        f = node.func
        name = (f.id if isinstance(f, ast.Name)
                else f.attr if isinstance(f, ast.Attribute) else "")
        if name in wanted:
            print(f"{p}:{node.lineno} CALL {ast.unparse(f)}")
```

**Juízes e derivações:**

- `tarefa_do_dia` está no lookup e no registro vivo (`app/docs/JUIZES.md:78`; `app/core/juizes.py:295`). A carga é seu lote, com ação pura e relógio de borda (`app/chamados/services/tarefa_do_dia.py:562`).
- `porta_alcancavel_do_dia` está registrado como `_C17`, com pergunta diferente: chegar ao dia pela fila/app (`app/core/juizes.py:132`; `:293`). Seu corpo pede chamado da célula na fila ou pergunta viva no app ancorada na fila (`app/chamados/juizes.py:1622`).
- `censo_alcance` conta e categoriza a saída do juiz; documenta que não deriva porta (`app/chamados/services/vigia_fio.py:542`), mas suas próprias categorias e decisões a/b/c/d/e são derivadas em `:668`–`:704`.
- `ListaDoAdmin` lê lista e dias lavrados e memoriza o resultado, incluindo falha (`app/chamados/services/vigia_fio.py:205`; `:222`).
- `_resolver_da_rodada` junta dias por colaborador e consulta `resolver_dia_oferecido_nas_datas`; não cria juiz novo (`app/chamados/services/vigia_fio.py:258`).
- `_carimbar` é escritor de fotografia, e não juiz; serializa o registro inteiro em `.novo` e faz `os.replace` (`app/chamados/management/commands/vigiar_fio_morto.py:120`).

**Campo/semântica com decisões para o construtor:**

1. **RSS e operador. LEU:** leitor genérico do ESTADO compara todos os `esperado` por **igualdade**, `censo.get(k) != v` (`bin/fio_estado.py:125`). O comando monta `gritou` verificando verdade dos contadores, sem operador de limite (`app/chamados/management/commands/vigiar_fio_morto.py:186`). **INFERIU:** gravar `esperado.pico_rss_mb=1500` nesse esquema fará 1237 aparecer fora do esperado, embora complemento peça <=1500. **DECISÃO:** contrato de memória separado com limite/operador explícitos, ou extensão dos leitores para comparadores tipados; não escolher silenciosamente nem alterar igualdade dos contadores existentes (`bin/fio_estado.py:125`; `app/docs/BACKLOG.md:738`).

2. **Pico/processo/rodada/unidade. LEU:** HEAD `_carimbar` recebe um registro e só o serializa; não coleta RSS (`app/chamados/management/commands/vigiar_fio_morto.py:120`). **DECISÃO:** especificar no instrumento fonte de RSS, conversão exata para MB, janela de acumulação, instante da amostra e identificação do processo/rodada; MB decimal versus MiB e disponibilidade da fonte permanecem **[nao verificado]**. Complemento já escolheu unidade de trabalho rodada e pico do próprio processo; não perguntar isso como lei nova (`app/docs/BACKLOG.md:738`; anexo complementar **[nao verificado: localização integral do aval fora da cópia]**).

3. **Soltar carga e medir pico. LEU:** carga materializa dias de toda a janela para todos os colaboradores (`app/chamados/services/tarefa_do_dia.py:420`) e retém esses pares em `self._pares` (`:427`); `_ler_vida` percorre os pares (`:458`). **INFERIU:** liberação depois do último uso resolve retenção posterior, mas ganho no pico da rodada não pode ser inferido só do -392MB do anexo. **DECISÃO:** desenhar redução do tempo de vida no chamador pós-E3, ou escopo auxiliar que devolva só resultados; demonstrar último leitor e medir RSS retido e pico separadamente. O caminho futuro e os aliases que reterão a carga estão **[nao verificado]**.

4. **Janela. LEU:** `[ini,fim]` é fechada, perguntas filtradas por `data_evento__gte/__lte` (`app/chamados/services/tarefa_do_dia.py:374`); carga rejeita dia fora de `ini <= dia <= fim` (`:517`). `dia_do_turno` é explicitamente dia do turno; `agora` é instante e a casca rejeita carga com instante diferente (`:562`). Não há divergência nesses contratos lidos; preservá-los ao liberar carga (`:570`).

5. **Histórico no instante. LEU:** a carga corta decisão e efeitos pelo instante, mas documenta leitura de estado atual para identidade da pergunta, ata, cobertura, fila, cadastro, vínculo e tranca (`app/chamados/services/tarefa_do_dia.py:350`). **INFERIU:** tratar carga como fotografia histórica completa seria falso; otimização de tempo de vida não autoriza mudar horizonte ou restringir retângulo (`:354`; `:420`).

6. **Data civil versus instante. LEU:** borda lê `timezone.now()` e deriva `hoje=localdate(agora)`; carimbo grava `localtime(agora).isoformat()` e `hoje.isoformat()` (`app/chamados/management/commands/vigiar_fio_morto.py:168`; `:233`). ESTADO converte offset ao fuso do host para medir idade (`bin/fio_estado.py:129`). Já `pipeline_placar` usa `date.today()` do host e exige igualdade do `hoje` do carimbo com o dia civil julgado (`app/core/management/commands/pipeline_placar.py:94`; `:148`). Fuso efetivo igual nos dois processos é **[nao verificado]**; não ajustar relógios nesta obra sem prova de impacto.

**Contrato existente do carimbo e quem mais lê:**

- Escritor `_carimbar` é chamado apenas em frota e antes de `SystemExit`; o registro contém `medido_em`, `hoje`, banco/schema, esperados, listas de alarmes, gritou, censo e alcance completos (`app/chamados/management/commands/vigiar_fio_morto.py:231`).
- Recorte `--empresa` não carimba; mensagem declara isso (`app/chamados/management/commands/vigiar_fio_morto.py:253`).
- Falha de escrita não é engolida; JSON usa troca atômica (`app/chamados/management/commands/vigiar_fio_morto.py:123`).
- `bin/fio_estado.py::_ler_carimbo` lê censo/esperado/tempo e a manchete do alcance; `linha` publica (`bin/fio_estado.py:121`; `:165`).
- `pipeline_placar.alarmes_fio_mudo` lê mesmo caminho e os campos `hoje`, `censo[nome]`, `esperado[nome]`; acusa ausência/forma errada (`app/core/management/commands/pipeline_placar.py:85`). Só o placar chama os nomes fio_mudo e nunca_bateu, portanto ele ainda não verifica memória (`:135`).
- CI aponta para carimbo temporário para não sobrescrever produção (`app/config/settings/ci.py:31`).
- Cron chama o vigia com `--alarme --alarme-passivo`, teto 900s, e dependências declaradas; não é necessário inferir novo cron para o alarme de memória (`app/config/crons.py:1721`).

**Arquivos comuns, segundo anexo de voo [nao verificado: não encontrei arquivo local correspondente]:**

- Necessário para instrumento RSS: `app/chamados/management/commands/vigiar_fio_morto.py` é compartilhado com `wt-bugpar-124848`, `wt-codex-hj-090-152256`, `wt-e3-b6-114947`.
- Provável para liberação pós-E3: `app/chamados/services/vigia_fio.py` está em `wt-bugpar-124848`. Local preciso pós-E3 **[nao verificado]**.
- Se desenho da liberação pedir alterar o juiz/lote: `tarefa_do_dia.py` cruza `wt-bugpar-124848` e `wt-e3-b6-114947`; isto ampliaria escopo, porque complemento pede soltar carga no chamador (`app/docs/BACKLOG.md:738`).
- Juiz `chamados/juizes.py` cruza `wt-bugpar-124848`, `wt-o168v6-233014`, `wt-e3-b6-114947`; nenhuma mudança nele decorre necessariamente do complemento.
- `config/crons.py` cruza `wt-bugpar-124848` e `wt-codex-hj-090-152256`; só precisa tocar se mudar encadeamento/opções/teto, o que o complemento não pede (`app/config/crons.py:1721`; `app/docs/BACKLOG.md:738`).
- Se usar selo existente `test_o261_vigia_fio.py`, cruza bugpar/HJ090/E3; não executar esse teste nesta preparação (`app/docs/BACKLOG.md:739`).

**Brief complementar sugerido:** pós-E3, confirmar censo e último leitor; reduzir só retenção da carga no chamador; RED mede antes/depois e DIFF completo do vigia zero; RSS é instrumento separado, contrato antes, unidade/operador/falha/quem-lê explícitos; não usar igualdade `esperado=1500`; não mudar autoridades, retângulo de carga, horizonte `agora`, contadores de negócio, alarme passivo ou encadeamento; medir pico e retenção no caminho real, incluindo aliases e partes após o censo (`app/docs/BACKLOG.md:738`; `app/chamados/services/tarefa_do_dia.py:350`; `bin/fio_estado.py:125`; `app/chamados/management/commands/vigiar_fio_morto.py:176`).

## 13/11 18:10 · sessao 83dfb72e585c · - · - · origem=Ronald (tmux)

LEU — pontos críticos para consolidar:

| Campo/contrato | Escritor e leitores no HEAD |
|---|---|
| `Justificativa.data_turno` | `DateField`, “data do turno”: `app/ponto/models.py:144`. API recebe data do payload e escreve: `app/api/views.py:1948`, `app/api/views.py:1995`; web procura primeira entrada pelo dia civil `timestamp__date`: `app/ponto/views.py:1598`; regularização usa dia local da entrada: `app/ponto/services/regularizacao.py:73`. |
| `tipo` | Choices em `app/ponto/models.py:116`; web aceita texto e escreve em `app/ponto/views.py:1591`, `app/ponto/views.py:1625`; API escreve em `app/api/views.py:1986`; cron automático grava `esqueci_saida`: `app/ponto/management/commands/processar_alertas_turno.py:399`. |
| `saida_informada` / `batida_entrada` / `batida_gerada` | Saída é instante completo, suporta madrugada: `app/ponto/models.py:149`; entrada é FK do turno: `app/ponto/models.py:139`; gerada é FK única da saída materializada: `app/ponto/models.py:167`. Web lê texto e torna aware: `app/ponto/views.py:1606`; regularização grava entrada e saída: `app/ponto/services/regularizacao.py:93`; API não entrega esses dois campos na criação: `app/api/views.py:1992`. |
| `status` | Aprovação avulsa e materialização: `app/ponto/services/justificativa.py:205`, `app/ponto/services/justificativa.py:383`; lote aprova antes de materializar saída no mesmo atomic: `app/ponto/views.py:1796`; rejeição avulsa/lote: `app/ponto/services/justificativa.py:283`, `app/ponto/views.py:1875`; regularização recoloca pendente: `app/ponto/services/regularizacao.py:105`; auto-rejeições: `app/ponto/services/entrada_sincronizada.py:41`, `app/ponto/services/triagem_batida.py:608`, `app/chamados/reconciliador.py:533`. |
| Aprovação → cartório | Todo save da justificativa dispara julgamento no seu `data_turno`, independentemente de status: `app/ponto/signals.py:162`, `app/ponto/signals.py:165`; aprovação invalida fechamento: `app/ponto/services/justificativa.py:214`. |
| `justificado` da grade | `_marcar_ausencias` consulta somente datas aprovadas, sem tipo: `app/escala/utils.py:1197`; limpa todos os missing, apaga órfãs e escreve True: `app/escala/utils.py:1254`. |
| `delta` | Escritor `_celula_do_marco` usa diferença entre instantes absolutos, arredondada em minutos: `app/escala/utils.py:325`; leitor da ata refaz diferença de HH:MM pela menor distância ±720: `app/escala/services/leitor_celula.py:14`, `app/escala/services/leitor_celula.py:32`. |
| `acesa`, `luz`, `dispensada` | Ata traduz células em lâmpadas: `app/ponto/services/cartorio.py:711`, `app/ponto/services/cartorio.py:722`; dispensa significa acesa True, luz None e motivo, escrito por `dispensar_por_ausencia`: `app/ponto/services/cartorio.py:807`; leitor preserva `dispensada`: `app/escala/services/leitor_celula.py:145`. |
| `justificado` reconstruído | `_justificado_lavrado` usa **qualquer** lâmpada dispensada por justificativa: `app/escala/services/leitor_celula.py:81`; `grade_da_celula` escreve True: `app/escala/services/leitor_celula.py:439`; `supra_juiz.classificar_dia` usa True para retirar acusações FURO_*: `app/ponto/supra_juiz.py:371`. |

INFERIU — consequências de portar literalmente o diff:

1. **Dispensar um marco viraria novamente dia inteiro justificado no leitor da ata.** A cadeia é `any(dispensada='justificativa')` → `dia.justificado=True` → retirada de FURO_*. Exigir RED pela grade reconstruída da ata, além da grade fresca. Fontes: `app/escala/services/leitor_celula.py:81`, `app/escala/services/leitor_celula.py:439`, `app/ponto/supra_juiz.py:371`.

2. **O diff usa contrato temporal já superado.** `_marcar_ausencias` hoje recebe colaborador e mapa de deslocamentos, não marcos: `app/escala/utils.py:1156`. Os builders entregam os deslocamentos em `app/escala/utils.py:807`, `app/escala/utils.py:1848`. A coluna não é cadeia cronológica do turno; a própria função documenta E19/S02/E03/S07 contra régua E03/S07/E19/S02: `app/escala/utils.py:353`. A origem existente é `deslocamentos_na_cadeia`, que casa coluna com instante já encadeado: `app/escala/utils.py:366`. `horas_reais_do_dia` documenta explicitamente que reencadear régua foi removido: `app/ponto/turnos.py:2655`.

3. **A guarda anti-push do diff duplicaria um predicado que já nasceu no HEAD.** `parcial_so_lista` lê carimbos e `houve_fato_novo`: `app/ponto/services/cartorio.py:398`, `app/ponto/services/cartorio.py:423`. Os portões atuais passam instantâneo anterior do julgamento e o mesmo `agora`: `app/ponto/services/cartorio.py:1757`. A escolha do construtor é compor o alcance da justificativa nessa porta ou criar representação específica que ela leia; copiar a guarda do diff perde a evolução do instantâneo/relógio. Fontes: `app/ponto/services/cartorio.py:417`, `app/ponto/services/cartorio.py:1757`.

Divergências a apresentar como DECISÃO de contrato, sem decidir:

- Data de payload/dia civil versus dia local da entrada/dia do turno: transportar a instância do marco/turno existente ou continuar contrato por data com alcance explicitado; sítios `app/api/views.py:1948`, `app/ponto/views.py:1598`, `app/ponto/services/regularizacao.py:73`, `app/ponto/models.py:144`.
- `delta` absoluto arredondado versus HH:MM com menor distância: preservar valor/proveniência lavrada ou assumir formalmente domínio limitado; sítios `app/escala/utils.py:325`, `app/escala/services/leitor_celula.py:32`. O dado de produção fornecido não permite medir casos divergentes. [nao verificado]
- Ausência de missing versus dispensa versus batida real: usar carimbo de cobertura integral explícito ou composição da cobertura na autoridade; manter “qualquer dispensa” deixa significado incorreto para cobertura parcial. Sítios `app/escala/utils.py:1256`, `app/ponto/services/cartorio.py:807`, `app/escala/services/leitor_celula.py:81`.
- Período/valor não legível versus zero devido: a raia retorna zero quando falta delta [nao verificado: implementação só no anexo]; o HEAD trata hora ilegível como None e distingue `nao_sei`: `app/ponto/turnos.py:2624`, `app/ponto/turnos.py:2663`. Falta definir como o dinheiro transportará essa falta de proveniência, sem inventar período.

Juízes:

- JUIZES lista marcos soberanos na célula e casamento na grade: `app/docs/JUIZES.md:22`, `app/docs/JUIZES.md:27`; cobertura de ausência: `app/docs/JUIZES.md:55`.
- O nascimento de `marcos_cobertos_por_justificativa` está autorizado por escrito: `app/docs/CORTES.md:319`, `app/docs/CORTES.md:320`; ainda não aparece no bloco vivo de ausência onde o diff pretende inseri-lo: `app/core/juizes.py:251`, `app/core/juizes.py:261`. A ausência integral da função foi encontrada por busca, sem uma linha positiva para citar. [nao verificado]
- `_justificado_lavrado` e `_delta_min` derivam por conta própria: respectivamente any de motivos e distância circular das horas: `app/escala/services/leitor_celula.py:81`, `app/escala/services/leitor_celula.py:32`.

Arquivo adicional necessário ao desenho e ausente de S3: **`app/escala/services/leitor_celula.py`**, por semântica de `justificado` e transporte dos campos novos; evidência em `app/escala/services/leitor_celula.py:439`. Não consta do anexo arquivos_em_voo. [nao verificado: comparação com anexo textual]

Escritores de justificativa são dependências para medir; não inferir automaticamente necessidade de editar API/views: a cobertura pode ser corrigida na origem sem mudar emissão. Fontes dos escritores: `app/api/views.py:1992`, `app/ponto/views.py:1635`, `app/ponto/services/regularizacao.py:91`, `app/ponto/management/commands/processar_alertas_turno.py:394`.

## 13/11 18:10 · sessao 4b2bbf1f4bdb · - · - · origem=Ronald (tmux)

LEU os trechos abaixo na cópia; a identificação deles como HEAD Git não foi verificada por mim. [nao verificado]

**Semântica de férias e espelho**

| Campo/parâmetro | LEU: escritores e leitores | Divergência / decisão a deixar ao construtor |
|---|---|---|
| Identidade da origem | O espelhador recebe UM agendamento, mas seleciona qualquer `Ausencia(tipo='ferias')` do colaborador cuja `data` esteja na janela, e pula todo dia encontrado, sem verificar origem nem status. A descrição escrita contém `#pk`. `app/ponto/services/ausencia.py:1161`, `:1181`, `:1183`, `:1200`, `:1207`. | **DECISÃO técnica:** como transmitir a identidade do agendamento ao produtor genérico: parâmetro interno ou caminho de criação específico. A exigência de não reaproveitar outro agendamento vem do complemento fornecido; implementação ainda [nao verificado]. |
| Reabertura indireta | `criar_ausencia` busca rejeitada por `(colaborador,tipo,data)` e retorna `_reabrir`, sem identidade de agendamento. `app/ponto/services/ausencia.py:254`, `:291`. | **INFERIU:** filtrar somente `existentes` por FK não fecha a obra: o criador pode reabrir rejeitada de outro agendamento. **DECISÃO:** propagar a identidade à busca/reabertura ou impedir esse reaproveitamento no caminho do espelho. RED próprio: outra origem + rejeitada no mesmo dia. `app/ponto/services/ausencia.py:254`, `:291`, `:1203`. |
| `status` de Ausencia versus agendamento | Agendamento ativo para o espelho significa `agendado/em_gozo/concluido`. Qualquer Ausencia existente, inclusive rejeitada, satisfaz o espelho. Cobertura para folha exige aprovada; para cobrança basta não rejeitada. `app/ponto/services/ausencia.py:1181`, `:1183`, `:1200`; `app/ponto/turnos.py:2511`, `:2516`, `:2518`. | **DECISÃO de desenho:** no fluxo futuro, presença de linha rejeitada da mesma origem será apenas denunciada pelo contador ou será escrita pelo agendamento em ato próprio? O complemento manda listar o passivo e proíbe correção automática; não converter a introdução da FK em cura histórica. Fonte do mandato: anexo `complemento_hj_091.txt:4` (numeração por linha textual fornecida). |
| Cancelamento/rejeição | A tela “cancelar Ausencia” faz flip para `rejeitada`; cancelar agendamento muda seu status e o espelho apaga as ausências selecionadas. Cancelamento admin primeiro rejeita a janela e depois salva cancelado, disparando remoção. `app/ponto/views.py:2721`, `:2734`; `app/ferias/services.py:384`, `:385`, `:411`, `:417`; `app/ponto/services/ausencia.py:1189`. | **DECISÃO técnica:** concentrar a remoção por identidade no espelho ou manter auxiliar chamado pelo agendamento com identidade explícita. Não confundir o status “cancelado” do agendamento com o estado “rejeitada” da Ausencia. `app/ferias/models.py:293`; `app/ponto/views.py:2721`. |
| Janela versus propriedade | Espelho cancelado apaga somente na janela corrente. `delete()` do agendamento e auxiliares também selecionam por colaborador/tipo/data, sem origem. `app/ponto/services/ausencia.py:1183`, `:1189`, `:1263`, `:1274`; `app/ferias/models.py:453`. | **INFERIU:** linha deslocada para fora da janela sobrevive; linha de outra origem dentro da janela pode ser apagada/rejeitada. **DECISÃO:** como compatibilizar operação por FK com restos legados sem FK; listar irresolvidos, sem adivinhar origem por coincidência de data. `app/ponto/services/ausencia.py:1183`, `:1189`, `:1267`, `:1282`. |
| `_minhas` na guarda | O comentário diz “Ausencias que ESTE agendamento materializou”; a consulta considera todas as férias do colaborador na janela antiga e as exclui do veto. `app/ferias/models.py:422`, `:430`, `:435`. | **DECISÃO técnica:** excluir somente IDs com FK deste agendamento ou criar mecanismo de alimentação equivalente para o veto. Com FK, preservar o critério atual por janela continuaria dispensando possíveis linhas de outra origem. `app/ferias/models.py:430`, `:437`. |
| Remarcação e `exceto` | `_trocar_janela` é declarado escritor único das datas; lê agendamento/PA sob trava, calcula saldo, apaga trecho antigo fora da janela nova e salva novo intervalo. Apagador não recebe agendamento. `app/ferias/services.py:422`, `:434`, `:436`, `:445`, `:450`; `app/ponto/services/ausencia.py:1263`. | **DECISÃO técnica:** acrescentar identidade ao auxiliar ou delegar a limpeza ao espelho, preservando trava/atomic e ordem documentada. A mudança cruza `app/ferias/services.py`, que consta da HJ-090 no anexo do usuário. `app/ferias/services.py:423`, `:425`; anexo `arquivos_em_voo.txt` [numeração não calculada por mim]. |
| `data_inicio/data_fim/dias` | Datas do agendamento são `DateField`; dias são corridos, inclusivos: `(fim-inicio).days+1`. `clean` confere coerência; criação deriva fim por `dias-1`. `app/ferias/models.py:309`, `:310`, `:369`, `:380`; `app/ferias/services.py:313`. | **Sem divergência lida** sobre unidade ou inclusão. Não transformar dias em úteis nem usar fim exclusivo. `app/ferias/models.py:380`; `app/ferias/services.py:313`. |
| Dia de Ausencia versus período inteiro | Espelhador cria uma Ausencia diária com `dias_corridos=1`, `minutos=0`, sem `data_fim`. Painel busca irmãos por documento e mostra intervalo desses irmãos ou data/fim da Ausencia. `app/ponto/services/ausencia.py:1203`; `app/ponto/views.py:2073`; `app/templates/ponto/partials/painel_ausencia.html:50`. | **DECISÃO de apresentação:** obter período/dias/status do agendamento ligado, mantendo a data diária como dia consultado. Não escrever período inteiro nos campos da Ausencia diária apenas para servir ao painel: alteraria significado consumido pelos juízes. `app/ponto/turnos.py:2538`, `:2554`; `app/ponto/models.py:894`. |
| `data_fim=NULL` | Help text genérico diz “indeterminado/em aberto”; leitor explica que NULL significa dia único fora de `TIPOS_EM_ABERTO`. `app/ponto/models.py:751`; `app/ponto/turnos.py:3150`. | **DECISÃO de texto/contexto:** painel específico de férias deve distinguir ausência diária do período do agendamento; sem derivar “indeterminado” pelo NULL isolado. `app/ponto/turnos.py:3150`; `app/ponto/services/ausencia.py:1204`. |
| Status do agendamento versus datas | `status_por_data` deriva agendado/em_gozo/concluído; cancelado é decisão humana fora da função. `ferias_na_janela` usa interseção inclusiva das datas e exclui só cancelado, inclusive com status concluído em data viva. `app/ferias/models.py:273`; `app/ferias/services.py:47`, `:57`. | **Sem lei nova:** contador do par deve usar o juiz de férias na janela como testemunha, evitando `ATIVOS=('agendado','em_gozo')` como substituto para cobertura histórica. `app/ferias/services.py:44`, `:47`, `:58`. |
| Hoje / fuso | Defaults de `status_por_data`, `em_gozo_hoje` e remarcação usam `datetime.date.today()`; cron passa `timezone.localdate()`. Configuração base é São Paulo e `USE_TZ=True`. `app/ferias/models.py:280`; `app/ferias/services.py:62`, `:438`; `app/ferias/management/commands/sincronizar_status_ferias.py:27`; `app/config/settings/base.py:93`, `:95`. | **DECISÃO de contrato para nova sonda:** hoje explícito proveniente de qual borda, se ela precisar de “hoje”? Datas concretas da janela dispensam isso. **INFERIU:** APIs de data podem divergir perto da meia-noite se fuso do processo e Django diferirem; fuso do processo em produção [nao verificado]. Não ampliar HJ091 para corrigir relógios existentes sem mandato. |
| Dia civil versus turno | Folha cruza timestamp de batida por `timezone.localdate` com datas inclusivas do agendamento; ausências cobrem a data fornecida. Parcial noturna tem contrato distinto: madrugada é deslocamento do dia inicial do turno. `app/folha/export.py:491`, `:496`, `:500`; `app/ponto/turnos.py:2554`, `:2627`, `:2630`. | **DECISÃO de explicitação:** contadores do vínculo diário devem comparar datas do agendamento/Ausencia, sem reinterpretar a regra de batida como dia do turno. Folha está expressamente fora da mudança no complemento. Fonte do mandato: anexo `complemento_hj_091.txt:2`. |
| FK nullable / remoção da origem | Há `AgendamentoFerias.delete()` que limpa janela e depois remove agendamento; ainda não existe FK de férias na definição lida de Ausencia. `app/ferias/models.py:453`; `app/ponto/models.py:743`, `:786`, `:791`. | **Possível lacuna de alcance**, a submeter ao exame das leis: `on_delete=PROTECT`, `SET_NULL` ou `CASCADE` têm efeitos diferentes sobre prova da origem e linhas deslocadas. Não escolhi. Se lei escrita já responder, vira implementação, não pergunta de lei. |

**Pontos adicionais para o brief**

- O painel atual só oferece evidência de grupo documental, não de agendamento; `#N` é texto exibido. `app/ponto/views.py:2073`, `:2079`; `app/templates/ponto/partials/painel_ausencia.html:50`, `:57`.
- O selo precisa seguir o alcance indireto de criação/reabertura, não somente atribuições no espelhador: `criar_ausencia → _reabrir` pode escapar da nova identidade. `app/ponto/services/ausencia.py:254`, `:291`.
- `dias_ferias_com_batida` já usa `ferias_na_janela`, recorte da competência, datas civis locais e batidas apuráveis; não precisa consumir FK de Ausencia para responder à pergunta que faz. `app/folha/export.py:484`, `:486`, `:491`, `:499`, `:500`.
- Regra de unicidade da Ausencia continua `(colaborador,tipo,data)` apenas entre não rejeitadas; acrescentar FK não permite presumir coexistência livre de duas linhas vivas de férias no mesmo dia. `app/ponto/models.py:826`.
- “Não reutilizar outra origem” e “não criar sobre fato conflitante” devem coexistir: a criação futura continua passando pelo veto, cuja recusa gera Pauta DP. `app/ponto/services/ausencia.py:1164`, `:1203`, `:1210`, `:1216`.
- A lista raspada tem caso com data `0026-10-07`; validade ou causa desse valor não foi examinada no código/banco. Anexo `passivo.r` [numeração não calculada por mim]; causa [nao verificado].

## 13/11 18:10 · sessao 4922a0386829 · - · - · origem=Ronald (tmux)

LEU no código da cópia; não houve escrita. `AGENTS.md` não apareceu na busca por nome; inexistência universal [nao verificado]. HEAD Git não verificável nesta árvore sem `.git` [nao verificado].

**Portas/escritores confirmados, linhas atuais**

| Porta | Caminho efetivo e alcance |
|---|---|
| Editar | Busca apenas PK (`app/ponto/views.py:2761`), chama editor (`:2794`); editor altera tipo/data/fim/dias/descrição/minutos e salva (`app/ponto/services/ausencia.py:498`, `:509`). Não há guarda de origem nessa sequência. |
| Cancelar individual/lote | Seleciona qualquer não rejeitada e decide rejeitada, preservando/prefixando descrição (`app/ponto/views.py:2728`, `:2729`, `:2734`; lote `:2345`, `:2351`). |
| Rejeitar individual/lote | Seleciona abertas e passa pelo decisor (`app/ponto/views.py:2687`, `:2701`; lote `:2316`, `:2319`). Individual também alcança irmãos por `documento_referencia` (`:2688`, `:2690`). |
| Reverter rejeição | Busca apenas PK e chama reversor (`app/ponto/views.py:2583`, `:2584`), que chega a `_reabrir` (`app/ponto/services/ausencia.py:1103`). |
| Aprovar individual/lote | Busca abertas, gate documental e decisão; sem filtro de origem (`app/ponto/views.py:2606`, `:2608`, `:2643`; lote `:2282`, `:2284`, `:2290`). |
| Anexar web | Busca qualquer PK, chama anexador (`app/ponto/views.py:2140`, `:2151`); grava documento e pode mudar estado (`app/ponto/services/ausencia.py:858`, `:860`, `:875`). |
| Editar período | Busca aprovada e chama decisor (`app/ponto/views.py:2670`, `:2673`); só passa se juiz a julgar parcial (`app/ponto/services/ausencia.py:888`). |
| Lançar/submeter web | Tipo vem do POST (`app/ponto/views.py:2378`, `:2476`), criador chamado em `:2415` / `:2536`; lançar força aprovada (`:2420`). |
| API criar/reenvio | Tipo recebido (`app/api/views.py:1705`), cria (`:1805`), reenvio existente pode anexar (`:1833`, `:1838`). |
| API documento | Busca PK+colaborador (`app/api/views.py:3069`), exige aberta (`:3079`), chama anexador (`:3108`). |
| Veredito célula | Cria pelo mesmo produtor (`app/ponto/services/veredito_celula.py:337`). |
| IA | Escritor genérico cria (`app/ponto/services/checar_ausencia.py:46`), mas prévia normal recusa férias (`:21`). |

**Achado de desvio indireto relevante:** filtrar existentes do espelho pela nova FK não basta para garantir “não reaproveitar outro agendamento”: `criar_ausencia` procura rejeitada por pessoa/tipo/data (`app/ponto/services/ausencia.py:254`) e a reabre antes de criar (`:291`), sem origem. INFERIU: o contrato da nova FK precisa atravessar esse caminho também, preservando o escritor único (`CLAUDE.md:32`).

**AST executado (stdlib, sem importar app)** encontrou chamadas adicionais atuais:

- `curar_ausencia_sem_fim.handle → rederivar_fim` (`app/ponto/management/commands/curar_ausencia_sem_fim.py:46`).
- `rejulgar_status_por_tipo.handle → rejulgar_status_pelo_tipo` (`app/ponto/management/commands/rejulgar_status_por_tipo.py:31`).
- `escalonar_documentos_ausencia.handle → escalonar_aguardando_documento` (`app/ponto/management/commands/escalonar_documentos_ausencia.py:26`).
- `rederivar_minutos_h06_comp11.tratar_linha → editar_ausencia_registro` (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169`).
- `zerar_dados_operacionais.handle → apagar_ausencias_em_ensaio` (`app/ponto/management/commands/zerar_dados_operacionais.py:90`).

Busca AST por nome simples `decidir` também encontrou homônimos em chamados/core; não são prova de chamada ao decisor de Ausencia. Deve resolver imports ou rotular candidatos [nao verificado].

**Campos e leitores do painel**

- Modelo presente tem `tipo`, `data`, `data_fim`, `minutos`, `dias_corridos`, `descricao`, `status`, `documento_referencia`; FK para agendamento ainda não aparece no trecho da classe lido (`app/ponto/models.py:749`, `:750`, `:751`, `:755`, `:761`, `:776`, `:791`, `:799`).
- Painel agrupa por documento, não por férias/origem (`app/ponto/views.py:2074`, `:2077`).
- Exibe intervalo de irmãos, senão range, senão dia (`app/templates/ponto/partials/painel_ausencia.html:50`, `:51`, `:52`); descrição é texto (`:57`).
- Mostra dias da Ausencia (`:123`), origem chamado/documento (`:126`, `:127`); form editar permanece disponível por permissão geral (`:131`, `:134`).
- Form de anexo não tem guarda de tipo no template (`:90`).
- `AusenciaForm.clean_tipo` recusa férias (`app/ponto/forms.py:65`), mas as portas acima passam POST diretamente ao produtor/editor (`app/ponto/views.py:2415`, `:2536`, `:2794`); o veto do form não protege essas chamadas.
- Histórico deriva duração localmente de `data_fim/data` ou `dias_corridos` (`app/ponto/services/dia_decidido.py:616`, `:621`) e mostra descrição (`:624`).

**Divergências/DECISÕES técnicas a levar ao construtor**

1. Origem por agendamento × unicidade pessoa/tipo/dia: constraint atual não permite duas férias vivas no mesmo dia, independentemente da origem (`app/ponto/models.py:827`, `:829`). Opções de encaixe: recusar/listar colisão pela porta existente; outras alternativas que dupliquem viva exigem revisão do contrato, não são consequência automática da nova FK. Não decidi.
2. Nova FK × reabertura genérica: identidade atual para reabrir rejeitada é pessoa/tipo/data (`app/ponto/services/ausencia.py:254`), enquanto aval exige mesmo agendamento [nao verificado no HEAD; anexo]. Opções: parametrizar identidade de origem na porta ou impedir caminho genérico de alcançar derivada; não contornar com escrita direta.
3. Dia consultado × período: tela usa o registro diário e irmãos documentais (`app/ponto/views.py:2074`; template `:50`), enquanto férias deverão mostrar período do agendamento [anexo, não verificado no HEAD]. Opções de apresentação devem preservar dia consultado separadamente; período não deve ser reconstruído contando irmãos documentais.
4. “Cancelada” da UI é `status='rejeitada'` no dado (`app/ponto/views.py:2721`, `:2734`), diferente do vocabulário de AgendamentoFerias; não usar igualdade textual de status entre modelos.
5. `data/data_fim` são DateField (`app/ponto/models.py:750`, `:751`), range inclui ambas pontas (`:899`); horas parciais pertencem ao início do turno quando cruzam meia-noite (`:767`). `aprovada_em` é instante (`:810`, `:873`); não usar esse carimbo para decidir dia de cobertura.

**Lei/alcance:** L-007 + aval “escrita SO pelo agendamento” respondem extensão da proteção a aprovar e anexar derivada: ambos escrevem estado/documento (`CLAUDE.md:32`; `app/ponto/services/ausencia.py:858`, `:875`, `:912`). Não contar como nova pergunta de lei. Legado sem FK deve ser lista, conforme anexo; não inferir “sem FK = avulsa” [nao verificado no HEAD].

**Cruzamentos condicionais com anexo**

- Se guardas forem nas portas API, `app/api/views.py` cruza `wt-bugpar-124848` e `wt-e3-b6-114947` [anexo arquivos_em_voo, não verificável no HEAD].
- Se a proteção ficar na porta canônica de Ausencia, as chamadas API podem permanecer consumidoras sem tocar `app/api/views.py`; escolha técnica [INFERIU de `app/api/views.py:1805`, `:1838`, `:3108`].
- `app/ponto/views.py`, `app/ponto/services/ausencia.py`, `app/ponto/models.py` e painel não aparecem no anexo colado [anexo, não verificável no HEAD].
- Admin já é somente leitura (`app/ponto/admin.py:38`; proibições `app/core/admin.py:59`, `:62`, `:65`); não precisa virar nova porta corretiva.

## 13/11 18:10 · sessao 4be5e8c3e569 · - · - · origem=Ronald (tmux)

LEU no HEAD:

- A OTIMIZA-1 continua registrada como PRONTO 6 construído somente na raia, faltando suite, replay, verificador e pouso próprio; contrato identifica escritor `trava_teste.sh`, leitor `pista_estado.py::ler/linha`, razão/meta e tratamento de ausência. (`app/docs/RELATO.md:1645`, `app/docs/RELATO.md:1648`)
- O gerador HEAD passa diretamente de `recusa_txt` a `codex_txt`, sem seção de pista no molde ou nos argumentos. (`bin/gerar_estado.py:265`, `bin/gerar_estado.py:267`, `bin/gerar_estado.py:333`, `bin/gerar_estado.py:337`, `bin/gerar_estado.py:375`)
- A trava HEAD grava somente seu arquivo `.quem` e executa o comando; não há emissão de `suites.log` nesse corpo. (`bin/trava_teste.sh:40`, `bin/trava_teste.sh:41`, `bin/trava_teste.sh:42`)
- Existência de `.git`, `bin/pista_estado.py`, `bin/suite_impacto.py`, `bin/suite_impacto.sh` e diretório `bin/suite_impacto`: todos `False` na inspeção por `Path.exists`; não há arquivo:linha possível para ausência. **[nao verificado: identidade do HEAD/ref e blobs da raia]**
- Não encontrei juiz de pista/suite/pouso nos registros consultados; “juiz da seleção” do anexo não equivale a juiz registrado. **[nao verificado: ausência universal]** O contrato atribui pousos ao Git. (`app/docs/RELATO.md:1648`)

Mapa de escritores/leitores confirmado:

| Dado/argumento | Origem HEAD | Leitor HEAD / mudança no anexo |
|---|---|---|
| `ESTEIRA_QUEM` | `suite.sh:226`; `regua.sh:165`; `pre-push.sh:142` | `trava_teste.sh:23` gera `QUEM` com PID; anexo remove PID para a coluna | [nao verificado]
| `ONLY` | `suite.sh:56` ou helper `suite.sh:88`; `regua.sh:51` | executor `suite.sh:232`, `regua.sh:168`; guarda efetiva `suite.sh:155` | [nao verificado]
| `LABELS` | fonte única declarada em `regua.sh` | `suite.sh:77`; `pre-push.sh:184`; contrato pista em `RELATO.md:1648` | [nao verificado]
| status/veredito | log do comando | `regua.sh:170`, `regua.sh:178`, `regua.sh:179`; recorte não carimba em `regua.sh:181` |
| identidade certificada | árvore da régua | escritor `regua.sh:196`; leitor `pre-push.sh:65` e `pre-push.sh:68` | [nao verificado]
| `SUITE_ESCOPO`, `SUITES_LOG`, campos de pista | não implementados nos corpos acima | somente anexo **[nao verificado no HEAD]** |

DECISÕES sem decidir:

1. **`escopo`: presença de `--only` versus seleção efetiva.** O anexo diz recorte sempre que `ONLY` não vazio; HEAD já reconhece seleção que cobre a descoberta como inteira, inclusive labels de classe/método. Opções técnicas: reutilizar resultado do julgador existente; transportar descrição mais rica da corrida. Não clonar a decisão. (`bin/suite.sh:133`, `bin/suite.sh:140`, `bin/suite.sh:155`, `bin/suite.sh:173`; comportamento do anexo **[nao verificado no HEAD]**)

2. **Chamadas da raia versus motivo obrigatório.** `_porta` do anexo chama inteira sem `--motivo`; HEAD recusa antes do custo, e desconhece `--cobertura`. Opções: adaptar interface mantendo motivo e guarda; separar porta necessária à geração certificada com contrato compatível. Retirar a guarda não é opção autorizada pela lei escrita. (`bin/suite.sh:53`, `bin/suite.sh:66`, `bin/suite.sh:173`, `CLAUDE.md:101`; `_porta` **[nao verificado no HEAD]**)

3. **Inteiras CI e control-plane.** O mesmo `_teste_irmao` roda CI/LABELS e tenants/teste_motor. O anexo marca ambas como inteira. O control-plane roda mesmo quando arsenal foi evitado. Decisão de alcance: uma contagem agregada de todos os runs ou partições explicitamente identificadas; não presumir que “evitada” significa push sem run. (`bin/pre-push.sh:142`, `bin/pre-push.sh:159`, `bin/pre-push.sh:184`, `bin/pre-push.sh:186`, `bin/pre-push.sh:190`, `bin/pre-push.sh:198`, `app/docs/RELATO.md:1648`)

4. **Janela `24 h` versus runs terminados/iniciados.** Anexo escreve linha ao final, mas filtra por início; corrida ainda viva não aparece; corrida atravessando borda é contada pelo início. Opções: início, conclusão ou outra unidade explicitamente contratada; não são dia civil nem dia do turno. O contrato HEAD enumera início/fim e janela, sem resolver esta borda. (`app/docs/RELATO.md:1648`; algoritmo **[nao verificado no HEAD]**)

5. **Limites temporais e fuso.** Anexo usa `datetime.now()` ingênuo, timestamps shell sem offset, `ini >= desde` e nenhum teto `<= agora`; merges usam `git --since` sem offset. Opções: um instante/fuso explícito e intervalo comum; documentar preservação da convenção atual após medi-la. A semântica real de Git e timezone de prod não foi medida. (`bin/trava_teste.sh:40`, `app/docs/RELATO.md:1648`; algoritmo/análises de produção **[nao verificado no HEAD]**)

6. **Git ilegível versus zero pousos.** No anexo `_pousos` devolve `None`, mas `linha` publica “sem pouso” tanto para `None` como zero. Opções: separar indisponibilidade de contagem zero; propagar estado de medida da fonte. HEAD já exige “sem medida” para log ilegível, e falta de sinal não autoriza zero. (`app/docs/RELATO.md:1648`, `bin/gerar_estado.py:258`; defeito do anexo **[nao verificado no HEAD]**)

7. **Log vazio/parcial e fontes auxiliares ausentes.** Anexo distingue `suites.log` ilegível, mas linhas inválidas viram descartes e `medida.log` ilegível vira lista vazia: evitadas/ESCAPOU zero. Opções: estados por fonte e contagem de descartes; contrato explícito de parcialidade. (`app/docs/RELATO.md:1648`; comportamento **[nao verificado no HEAD]**)

8. **Razão arredondada versus meta.** Anexo calcula razão arredondada antes de comparar com `1.5`; valores pouco acima podem sair sem “ACIMA”. Opções: comparar fração crua e arredondar exibição; definir tolerância se aval explícito vier. Meta escrita é `<=1,5`, sem tolerância. (`app/docs/RELATO.md:1648`; algoritmo **[nao verificado no HEAD]**)

9. **Pouso Git versus mero merge first-parent.** Definição operacional já está escrita: pouso `merge --no-ff --no-commit`, com instrumento em pouso próprio. Logo não perguntar novamente se FF conta ou se instrumento pode pousar com produto. Registro de evento Git não comprova sucesso de deploy; essa possível exigência nova de denominação requer alcance explícito, não inferência. (`CLAUDE.md:325`, `CLAUDE.md:326`, `CLAUDE.md:327`, `app/docs/RELATO.md:1648`)

10. **`rc`, resultado e ausência de execução.** Contrato manda contar run com rc real, sem filtrar só sucesso; não perguntar se vermelho conta como inteira. Rc75 sai antes do comando na trava, logo não é corrida executada. Falha do Docker depois da concessão da trava é execução do comando, ainda que testes não tenham começado; decidir apresentação/partição sem alterar rc. (`app/docs/RELATO.md:1648`, `bin/trava_teste.sh:36`, `bin/trava_teste.sh:38`, `bin/trava_teste.sh:42`, `bin/pre-push.sh:148`, `bin/pre-push.sh:150`)

Rebase/replay: escolha já registrada entre commit atual com escape reproduzível ou mapa regenerado; mapa velho deixa IMPACTO vazio; não afirmar pronto nem repetir resultado histórico como atual. (`app/docs/RELATO.md:1635`, `app/docs/RELATO.md:1637`)

## 13/11 18:10 · sessao e205bf074ada · - · - · origem=Ronald (tmux)

LEU — leis que já respondem e não devem aumentar `perguntas de lei: N`:

| Pergunta | Resposta escrita no HEAD |
|---|---|
| Justificativa aprovada cobre dia inteiro automaticamente? | Cobre o marco que seu tipo nomeia; o abono deve consultar o mesmo juiz. `app/docs/CORTES.md:319`, `app/docs/CORTES.md:320` |
| Pode implantar cobertura estreita antes de pagar o período? | Não: a parte justificativa foi separada e retida até haver pagamento; virar falta inteira com DSR perdido foi expressamente recusado. `app/docs/CORTES.md:330` |
| Motor pode ler período pago? | Sim, pelo aval posterior: mesma fonte do PDF, uma mudança no motor, DIFF novo antes, limites explícitos. O aval de 18:4x supera “motor intocado” dos cortes anteriores nesse alcance. `app/docs/CORTES.md:353` |
| Furo residual após mudança de regra emite push retroativo? | Mudança de regra não é fato novo; lista do admin sem push. Piso fixo de data foi recusado. `app/docs/CORTES.md:323` |
| Sem batida casada a marco? | Classe CADASTRO, lista do admin, sem cura por código. `app/docs/CORTES.md:336` |
| Dia de parcial noturna? | `hora_fim < hora_ini` cruza meia-noite e pertence ao dia de início. `app/docs/CORTES.md:330` |
| DSR pode piorar indiscriminadamente? | Condição posterior é zero semana de DSR perdida a mais fora dos dias CADASTRO/BATIDA listados. `app/docs/CORTES.md:353` |
| Exportada e col709 25/08? | TXT intacto; col709 é BATIDA e vira Pauta DP com pago/devido. `app/docs/CORTES.md:353`; regra geral de gravado exportado em `CLAUDE.md:411` |
| Data civil ou dia de início para jornada/DSR? | Dia de início, inclusive semana/DSR. `app/docs/LEIS.md:117` |
| BATIDA.tipo ou papel da ata? | Instante da batida contra DNA/ata; tipo gravado não decide. `app/docs/LEIS.md:119` |
| Previsto inventado fora da grade? | Proibido, grade fonte única. `CLAUDE.md:148`, `app/docs/LEIS.md:75` |
| Caso CADASTRO/BATIDA pode ser curado por código? | Proibido; só ESTRUTURA é fatia. `CLAUDE.md:179` |

INFERIU — duas candidatas de alcance, sem decisão:

1. **Uma JUST ancorada cobre todos os turnos elegíveis daquele dia, ou apenas seu episódio?** O modelo declara `batida_entrada` como “entrada do turno com problema”, `data_turno` e `chamado_origem`, mas o diff fornecido colapsa justificativas em `{data: tipos}` e perde as âncoras. Opções: alcance por dia/tipo; alcance pelo turno da âncora; sem âncora encaminhar para cadastro. O corte diz “marco que o tipo nomeia”, sem descrever resolução de múltiplos episódios quando uma aprovação tem âncora específica. `app/ponto/models.py:139`, `app/ponto/models.py:144`, `app/ponto/models.py:173`, `app/docs/CORTES.md:319`; algoritmo da raia apenas no anexo, correspondência HEAD **[nao verificado]**.

2. **Se Just parcial, Ausência paga e realizado se sobrepõem, qual precedência/união de cobertura?** O HEAD soma minutos de Ausência e trata Just como conjunto de dias; a raia acrescentaria outra soma de minutos. Opções: união líquida por período; prioridade de uma fonte; manter efeitos distintos com exclusões explícitas. Há obrigação geral de partes coerentes, mas não localizei nas buscas em LEIS/CORTES/CLAUDE regra específica entre Just parcial e Ausência paga sobreposta. `app/ponto/services/efeito_ausencia.py:40`, `app/ponto/services/efeito_ausencia.py:47`, `app/ponto/services/efeito_ausencia.py:53`, `app/docs/LEIS.md:136`. Ocorrência em produção **[nao verificado]**. Antes de contar como pergunta efetiva, medir se a classe existe no universo ou fixar seu contrato de alcance.

Alertas para o desenho do construtor:

- O HEAD já **não tem** o acumulador `_min_parcial_pagos` no fechamento: foi removido como órfão, e o crédito é por dia via `credito_parcial.creditos_por_dia`. Portanto o endereço nominal do aval não pode ser restaurado cegamente. `app/ponto/services/fechamento.py:672`, `app/ponto/services/fechamento.py:994`, `app/ponto/services/fechamento.py:999`.
- O crédito existente abate atraso do próprio dia, sem transportar saldo para outros dias; o leitor atribui período pela data civil local da entrada, o que precisa ser confrontado com L-085 no censo semântico. `app/ponto/services/credito_parcial.py:32`, `app/ponto/services/credito_parcial.py:50`, `app/docs/LEIS.md:117`.
- Preservar a cura posterior H12H06-B: parcial é `cobre_parte_do_dia`, não `minutos` truthy como no diff antigo. `app/ponto/services/efeito_ausencia.py:35`, `app/ponto/services/efeito_ausencia.py:48`, `app/ponto/services/credito_parcial.py:23`, `app/ponto/services/fechamento.py:688`.
- Motor é zona inviolável; batida só pela porta, motor não fabrica batida; UTC no banco/UTC-3 na apresentação; DNA congelado e universo `batidas_apuraveis`. `CLAUDE.md:141`, `CLAUDE.md:143`, `CLAUDE.md:146`, `CLAUDE.md:149`, `CLAUDE.md:158`.
- L-126 ainda proíbe ao papel **construtor Codex** motor, core/juizes, portas/célula, exportada e itens que pedem `!`; o aval de motor não equivale, por si, a ampliar esse papel. `app/docs/LEIS.md:147`.
- RED antes de implementação, casos pela regra e adversarial; DIFF serve impacto, não certificação. `CLAUDE.md:28`, `CLAUDE.md:45`, `app/docs/LEIS.md:136`.
- `marcos_cobertos_por_justificativa`, `minutos_do_periodo_justificado` e `efeito_da_justificativa` não apareceram nas buscas em turnos/core/JUIZES do HEAD; criação aceita em corte é diferente de implementação presente. `app/docs/CORTES.md:320`; ausência no HEAD deve ser sustentada pelo script AST do pai, não por linha inexistente. **[nao verificado: resultado negativo do censo AST ainda não recebido]**.

## 13/11 18:09 · sessao bfd4d78ec20a · - · - · origem=Ronald (tmux)

LEU nesta cópia:

- **Escritor único / trilha:** `CLAUDE.md:32` e `app/docs/LEIS.md:65` (L-007).
- **Testemunha não recalcula:** `CLAUDE.md:20`; origem da cura em `CLAUDE.md:17`.
- **RED antes / quem mais lê:** `CLAUDE.md:28`; casos pela regra em `CLAUDE.md:45`.
- **Idempotência:** “2x = mesmo estado, 1 linha de trilha, 0 duplicata”, `CLAUDE.md:178`.
- **Cadastro/batida não se curam por código:** `CLAUDE.md:179`.
- **Juiz novo exige corte:** `CLAUDE.md:74`; registro vivo prevalece sobre lookup em `app/docs/JUIZES.md:11` e `:14`.
- **Zonas invioláveis:** motor só com aval em `CLAUDE.md:142`; batida em `:143`; CPF/nome completo em `:145`; UTC/apresentação UTC-3 em `:146`; grade/célula em `:148`; universo apurável em `:158`.
- **Exportada:** L-092 está em `app/docs/LEIS.md:125`, mas é alterada por L-113 em `:153`; forma consolidada vigente em `CLAUDE.md:411` e `:422`. O anexo HJ091 restringe o próprio passivo a lista/Pauta DP; não ampliar esse escopo.
- **Escolhas técnicas do construtor:** `CLAUDE.md:276`; não contar automaticamente escolha de FK/on_delete/encaixe como pergunta de lei.
- **Pergunta já respondida pela lei escrita não conta:** `CLAUDE.md:269`.

**Autoridades registradas pertinentes:** `app/core/juizes.py:251` a `:274` inclui `efeito_vigente`, `cobertura_ausencia_periodo`, `veto_de_lancamento`, `estado_inicial`, `ABERTAS`, `em_gozo_hoje`, saldo, `AgendamentoFerias.clean`, `fim_de`; lookup resumido em `app/docs/JUIZES.md:55`. `status_por_data` se descreve como juiz em `app/ferias/models.py:274`, mas não consta nesse bloco do registro vivo; `espelhar_ferias`, `_guard_sobreposicao` e `ferias_na_janela` tampouco constam nele. Ausência universal no registro fora do bloco: busca textual executada, sem ocorrência desses nomes.

**Selo atual insuficiente para HJ091:** aceita qualquer escrita dentro do arquivo autorizado (`app/ponto/tests/test_chokepoint_ausencia_gate.py:29`), e declara a porta de Ausencia em `:65`; não distingue escritores internos. O espelhador chama `criar_ausencia` em `app/ponto/services/ausencia.py:1203`, portanto o selo futuro precisa representar essa chamada transitiva legítima, além de detectar entradas genéricas.

**INFERIU — perguntas de lei: 0 no escopo expresso.** Exclusividade de escrita das derivadas já decorre do aval fornecido e L-007; aprovar/reabrir/anexar também precisa entrar no desenho de alcance. FK nula não prova ausência avulsa: o aval manda listar o vínculo não resolvido. `on_delete`, localização da guarda e representação AST da escrita transitiva são decisões técnicas; nenhuma resposta encontrada em LEIS/CORTES que autorize inventar vínculo de origem por coincidência de janela.

`AGENTS.md` não apareceu em `rg --files -g AGENTS.md`; existência fora do universo dessa busca: **[nao verificado]**.

## 13/11 18:09 · sessao 323f94675b3f · - · - · origem=Ronald (tmux)

LEU — fontes e limites:

- A obra principal permanece interrompida, faltando suite, replay, verificador e pouso próprio; a preparação é leitura/desenho sem suite/sombra/prod. (`app/docs/BACKLOG.md:2083`; `app/docs/BACKLOG.md:739`)
- O complemento só entra depois do pouso da E3; soltar carga exige DIFF do vigia zero; pico RSS tem teto ≤1500 MB e exige contrato de cinco linhas/pouso próprio. A E3 não muda. (`app/docs/BACKLOG.md:738`; `app/docs/RELATO.md:5`)
- O contrato original de OTIMIZA-1 está integralmente em `app/docs/RELATO.md:1648`: uma linha TAB por run, início/fim/quem/escopo/rc; escopo declarado pelo chamador; pousos são merges first-parent do main; meta ≤1,5; arquivo ilegível significa “sem medida”; escrita da medida não altera rc; nada no pouso depende dessa seção.
- A construção da raia e as pendências estão documentadas em `app/docs/RELATO.md:1645`. O diagnóstico PRONTO 5 registra mapa velho e escolha pendente entre replay com escape reproduzível em commit atual ou mapa regenerado. (`app/docs/RELATO.md:1635`; `app/docs/RELATO.md:1637`)

Leis que já respondem — não contar como perguntas de lei:

| Questão | Lei escrita |
|---|---|
| Fonte do contador, sem derivador paralelo | `CLAUDE.md:17`; `CLAUDE.md:20`; `CLAUDE.md:35` |
| RED antes da cura e censo de quem mais lê | `CLAUDE.md:28` |
| Escopo literal, sem ampliar obra | `CLAUDE.md:37`; `app/docs/LEIS.md:62` |
| Contrato de entrada com fonte/unidade/universo/exclusões | `CLAUDE.md:192` |
| Cron somente vigia, sem decidir negócio | `CLAUDE.md:172` |
| Contrato de instrumento com quem chama/espera, falha/limite, medida, quem desliga | `CLAUDE.md:394` |
| Instrumento em pouso separado de produto | `CLAUDE.md:327`; `app/docs/LEIS.md:131` |
| Mesma árvore certificada pela suite é a que pousa | `CLAUDE.md:320` | [nao verificado]
| Pouso por merge sem fast-forward | `CLAUDE.md:326` | [nao verificado]
| Escolhas de encaixe/caminho são decisões técnicas | `CLAUDE.md:276` |
| Nenhuma lei nova por iniciativa própria | `CLAUDE.md:269` |
| Juiz novo exige corte nominal | `CLAUDE.md:74` |

INFERIU — para o escopo literal OTIMIZA-1 + complemento, não identifiquei lacuna de lei que obrigue inventar regra: **candidata a contagem “perguntas de lei: 0”**. As escolhas abaixo são semântica/encaixe de instrumento, não autorização para alterar o contrato. (`app/docs/RELATO.md:1648`; `app/docs/BACKLOG.md:738`; `CLAUDE.md:276`)

Autoridade:

- `JUIZES.md` é lookup; a lei está em CLAUDE e o registro vivo em `core/juizes.py`. (`app/docs/JUIZES.md:7`)
- `tarefa_do_dia` está registrado: puro, zero escritor, relógio `agora`, lote `CargaDaTarefa`; pergunta por instância de marco. (`app/docs/JUIZES.md:78`; `app/docs/JUIZES.md:84`; `app/core/juizes.py:150`; `app/core/juizes.py:295`)
- O carimbo do vigia é fotografia, não fonte de julgamento; sua autoridade é o censo ao vivo. (`app/chamados/management/commands/vigiar_fio_morto.py:21`)

Divergências/decisões para transmitir:

1. **Teto RSS ≤1500 versus igualdade dos leitores existentes.** `fio_estado._ler_carimbo` compara todos os valores de `esperado` por `!=`; inserir `pico=1500` faria 1237 virar divergência. Candidatas técnicas: representar operador/unidade no contrato e leitores; ou representar excesso acima do teto como contador esperado zero. Não escolhi. (`bin/fio_estado.py:121`; `bin/fio_estado.py:125`; `app/docs/BACKLOG.md:738`)
2. **Ancoragem obsoleta do complemento.** No HEAD, `vigiar_fio_morto.py:176` abre `tranca_em_lote()`; não contém `CargaDaTarefa` nem sua liberação. A localização pós-E3 precisa ser refeita após o pouso. (`app/chamados/management/commands/vigiar_fio_morto.py:176`; `app/docs/BACKLOG.md:738`)
3. **RSS da rodada/processo versus momento do carimbo.** `_carimbar` hoje apenas serializa e faz replace; o carimbo nasce depois dos censos. O método e a unidade exata do pico ainda precisam constar no contrato do complemento. (`app/chamados/management/commands/vigiar_fio_morto.py:120`; `app/chamados/management/commands/vigiar_fio_morto.py:232`; `app/docs/BACKLOG.md:738`)
4. **Universo de carimbo.** O carimbo só nasce em frota; `--empresa` declara que não carimbou. O encaixe do alarme de RSS em recortes precisa ser explicitado sem transformar recorte em fotografia da frota. (`app/chamados/management/commands/vigiar_fio_morto.py:27`; `app/chamados/management/commands/vigiar_fio_morto.py:231`; `app/chamados/management/commands/vigiar_fio_morto.py:253`)
5. **Instante/data civil.** O vigia lê `agora` uma vez, deriva `hoje` local e grava `medido_em` com offset; o leitor converte o offset ao fuso do host. O fuso real do host é [nao verificado]. (`app/chamados/management/commands/vigiar_fio_morto.py:164`; `app/chamados/management/commands/vigiar_fio_morto.py:233`; `bin/fio_estado.py:129`)

Zonas proibidas para o brief:

- Motor v2 só com aval explícito; nenhuma edição exigida pelo escopo apresentado. (`CLAUDE.md:142`; `app/docs/BACKLOG.md:738`)
- Não tocar gravado exportado, escala/vínculo ou regra de negócio fora do pedido; não pular testes. (`CLAUDE.md:265`) [nao verificado]
- Não rejulgar por vigia nem derivar previsto fora da grade/célula. (`CLAUDE.md:148`; `CLAUDE.md:172`)
- A árvore certificada da E3 permanece intocada pelo complemento. (`app/docs/BACKLOG.md:738`; `app/docs/RELATO.md:5`)

## 13/11 18:09 · sessao 7870d3302bf5 · - · - · origem=Ronald (tmux)

Leis e autoridades lidas para consolidar:

| Tema | Resposta já escrita / evidência |
|---|---|
| Juiz/escritor único; parâmetro consumido | `CLAUDE.md:178`; testemunha lê autoridade e não deriva em paralelo em `CLAUDE.md:20-22`; consultar lei existente antes de novo corte em `CLAUDE.md:25-27`. |
| Identidade BATIDA | Catálogo declara `(batida_origem,motivo)`, episódio, separado de MARCO em `app/chamados/catalogo/motivos.py:12-19`; `foto_ausente` é BATIDA em `app/chamados/catalogo/motivos.py:43`. Não é lacuna de lei. |
| Episódio imutável | Escrita uma vez no nascimento em `app/chamados/models.py:2437-2442`; origem usa `ultima_batida.id`, validada contra colaborador em `app/chamados/models.py:3453-3468`. |
| Contexto fundido/acervo | O comentário do consumidor proíbe derivar posteriormente o episódio do contexto fundido, porque já pode apontar outra anomalia; o acervo não ganha episódio assim: `app/chamados/models.py:3457-3459`. |
| Dia do turno versus civil | `abrir` exige dia do turno e `abrir_conversa` usa dia civil: `app/chamados/models.py:2784-2803`. Contrato de saída mantém essa distinção em `app/chamados/models.py:2811-2817`. |
| Juiz do dia | Registro vivo aponta `turnos_do_colab` em `app/core/juizes.py:207-208`; lookup registra geometria em `app/docs/JUIZES.md:20-25`. Adaptador `dia_do_turno_da_batida` chama o mapa da autoridade em `app/ponto/turnos.py:2927-2936`, não deriva outra regra. |
| Batida não reclamada por turno | Dia em que aconteceu, pela autoridade, em `app/ponto/turnos.py:2886-2893`; implementação em `app/ponto/turnos.py:2945-2955`. Selo de fonte única do dia está aprovado em `app/docs/CORTES.md:369`. |
| Fuso | UTC no banco, apresentação UTC-3: `CLAUDE.md:146-147`; `app/docs/LEIS.md:107` (L-080). |
| Prazo selfie | PWA zero, APK/NULL 90 minutos em `app/ponto/registro_batida.py:69-77`; decorrido em minutos e comparação `>=` em `app/ponto/registro_batida.py:80-91`. |
| Janela retro | `[agora − dias, agora]` inclusiva, não dias de turno: `app/ponto/management/commands/marcar_foto_ausente_retro.py:63-77`. |
| Prazo versus teto do turno | Comparar instante ou exigir fato encerrado, nunca só data: `CLAUDE.md:188`; L-073 em `app/docs/LEIS.md:100`. |
| Alcance | L-124: horizonte para frente, passado FOTO até fato novo, backfill único em `app/docs/LEIS.md:145`; reafirmada em `app/docs/CORTES.md:419`. |
| Exportada | L-113 permite nascimento em competência exportada, expressamente: `app/docs/LEIS.md:153`. Dinheiro/gravado seguem regras consolidadas em `CLAUDE.md:406-422`; não usar isoladamente o texto histórico de L-092. |
| Dono | L-099: só ESTRUTURA vira cura; CADASTRO/BATIDA vão ao admin: `CLAUDE.md:179`, `app/docs/LEIS.md:122`. |
| RED | Caso pela regra antes do código: `CLAUDE.md:28-29`, `CLAUDE.md:45-51`; L-110 em `app/docs/LEIS.md:136`; certificado/impacto/termômetro separados em `app/docs/LEIS.md:152`. |

Autoridades versus derivações:

- **Registradas no lookup:** `turnos_do_colab` e demais geometria em `app/docs/JUIZES.md:20-25`; `data_do_chamado` em `app/docs/JUIZES.md:59-63`; validar em `app/docs/JUIZES.md:38-39`; fio em `app/docs/JUIZES.md:40`.
- **Registrada apenas no vivo, na leitura realizada:** `selfie_ausente` em `app/core/juizes.py:1196-1199`, com corte em `app/docs/CORTES.md:283`. A busca nominal por `selfie_ausente` na cópia de `JUIZES.md` não encontrou entrada; lookup diz que o vivo manda em `app/docs/JUIZES.md:7-15`.
- **Consumidores que selecionam dados, sem registro de juiz encontrado:** `gerar_perguntas_disputa` escolhe `motivo.data` antes de perguntar pelo episódio em `app/chamados/services/disputa_emissao.py:371-392`; `batida_origem_do_nascimento` escolhe apenas `ultima_batida.id` em `app/chamados/models.py:3461-3468`. Classificá-los como autoridades novas seria **INFERIU**, não registro lido.
- **Emissor retro delega a ausência ao juiz:** `app/ponto/management/commands/marcar_foto_ausente_retro.py:73-77`; seu payload incompatível está em `:96-104`. Não deriva ausência por conta própria.
- **Apresentação fora da costura:** cabeçalho do retro formata `agora`/`corte` crus em `app/ponto/management/commands/marcar_foto_ausente_retro.py:80-82`; diverge da apresentação local exigida em `CLAUDE.md:146-147`. Não precisa ser tocado para entregar episódio (**INFERIU**).

**perguntas de lei: 0**, no escopo B e nas fontes acima (**INFERIU**). Episódio, dia do turno, fuso, identidade, nascimento em exportada e alcance já têm resposta; formato de transporte/chave e organização do selo permanecem decisões de desenho, não lei nova. A eventual rota concreta para recuperar passivo sem episódio confiável ainda precisa medir e desenhar; não autoriza reconstrução pela última batida do contexto fundido (`app/chamados/models.py:3457-3459`).

## 13/11 18:08 · sessao 86202b36139d · BUG-PAR · censo · origem=Code

**B3 é (b); o portão de produção do item 5 é (c). B1 e B2 não cabem nas cinco classes: o Codex viu e a triagem acolheu, deixando a cura em aberto.** Classificá-los como falhas de detecção ou descartes contrariaria os anexos. (`preverif4/r.r:7,64,68,83`; `triagem4.txt:2–13`; `veredito.txt:5`.)

Uso `preverifN/q.q` e `preverifN/r.r` para distinguir os blocos repetidos colados na pergunta. A numeração começa na primeira linha de conteúdo de cada anexo e inclui linhas vazias; correspondência com os arquivos originais: **[não verificado]**.

1. **B1 — visto e acolhido; sem classe aplicável.**

   **Pergunta:** “Contra-exemplo: chokepoint devolve COLISAO com batida ALHEIA (nao a irma) no meio do par -- a pergunta deveria ser fato_ocupado?” (`preverif4/q.q:2`.)

   **Resposta:** “O ramo pergunta somente se `_conflito` traduz como COLISAO. Não identifica a batida ofensora.” A resposta também identifica o “ciclo operacional de revalidação”. (`preverif4/r.r:7,16`.)

   **Triagem:** “1 PROCEDENTE”; depois, “Cura em aberto (desenho)”. Portanto, houve reconhecimento e encaminhamento, sem descarte. (`triagem4.txt:2,4`.)

2. **B2 — visto e acolhido; sem classe aplicável.**

   **Pergunta:** “selo/RED sem mordida (RED_2c morde se a cura marcar PARIDADE tambem para competencia fechada?” (`preverif4/q.q:5`.)

   **Resposta:** “O mutante ‘toda recusa vira PARIDADE’ satisfaz suas asserções; RED_2b cobra rollback, mas não cobra o código da competência.” A mesma linha aponta a injeção de COLISAO em 01:00 com irmã às 02:00. (`preverif4/r.r:64`.)

   **Triagem:** “4a RED_2c sem mordida” e “PROCEDENTE”. A insuficiência foi detectada por inspeção; não dependeu da execução dos REDs para aparecer no relatório. (`triagem4.txt:8`; `app/chamados/tests/test_bug_par_projetado.py:690–707`.)

3. **B3 — (b), falha da triagem.**

   **Pergunta:** “QUEM ESCREVE e QUEM LE cada campo/valor” incluía explicitamente “gravou do explicador, msg da casca”. A pergunta final repetiu o censo dos escritores e leitores. (`preverif1/q.q:3`; `preverif4/q.q:4`.)

   **Resposta:** “BLOQUEIA adicional: a Central ainda mente em `VIA_FATO_OCUPADO`.” O trecho descreve precisamente o encerramento sem batida apresentado como “Tudo validado e gravado.” (`preverif4/r.r:68`.)

   **Triagem e motivo:** “PRE-EXISTENTE ao diff”, sustentado por “(nao toca juizes.py:679-691 nem explicador.py:101); descartado -> fila, anexo ao opus.” (`triagem4.txt:9–10`.)

   O veredito declara essa premissa falsa e atribui expressamente a falha à triagem. A cópia confirma que o explicador restringe `gravou=False` a `recusada_paridade`. (`veredito.txt:4,6`; `app/chamados/services/explicador.py:96–102`.)

4. **Item 5 de fila — (c), quanto ao portão de produção literalmente exigido.**

   **Pergunta:** “O `--apply` toca gravado de competencia exportada?” (`preverif3/q.q:5`.)

   **Resposta:** apresentou o caminho financeiro, mas delimitou a evidência: “Os valores são sintéticos; existência desse desvio na produção **[não verificado]**.” (`preverif4/r.r:78–83`.)

   O item de fila exige “DIFF de frota cobrindo validacao recusada, hash TXT/ExportacaoDominio 10, trilha”. Esses resultados não constam do dado fornecido: **[não verificado]**. Por isso, o **portão de produção** é (c). O risco estático antecedente já fora visto e descartado como bloqueio por ser considerado preexistente e regido pela L-113. (`veredito.txt:5`; `triagem4.txt:11–13`.)

| Achado | Classe | Volta | Evidência |
|---|---|---|---|
| B1 | Sem classe prevista: visto e acolhido | 4 | `q.q:2`; `r.r:7,16`; `triagem4.txt:2–4` |
| B2 | Sem classe prevista: visto e acolhido | 4 | `q.q:5`; `r.r:64`; `triagem4.txt:8` |
| B3 | **(b) — falha da triagem** | 4 | `r.r:68`; `triagem4.txt:9–10`; `veredito.txt:6` |
| Item 5 — portão de produção | **(c) — fora do alcance** | 3/4 | `preverif3/q.q:5`; `preverif4/r.r:83`; `veredito.txt:5` |

## 13/11 18:04 · sessao 07de28eaa58b · RAIA-PREPARACAO-LASTRO · censo · origem=Code

**O escritor do estado já está concentrado; a decisão de lastro ainda está repartida.** O núcleo é `_gravar_estado`; o coletor mais próximo de uma autoridade única é `premissa_regularizada`; o degrau 2 de `lastro.julgar` e a proximidade dentro da lavratura continuam respondendo ao casamento batida × marco por critérios próprios. **LEU:** `app/chamados/models.py:201`, `app/chamados/models.py:1253`, `app/ponto/services/lastro.py:299`, `app/ponto/services/cartorio.py:666`.

Convenções: **LEU** indica leitura da cópia; **INFERIU** indica conclusão ou desenho apoiado nas referências. Os testes abaixo foram **lidos**; sua execução nesta cópia é **[não verificado]**. Dados atuais de banco, logs e resultado de deploy são **[não verificado]**. [nao verificado]

**1. Matriz de transições**

O campo temporal do chamado é **`resolvido_em`**, não `fechado_em`. `VIVOS = aberto/em_analise/registrado`; cobrança efetiva exclui `registrado`; `ENCERRADOS = resolvido/fechado`. **LEU:** `app/chamados/models.py:52`, `app/chamados/models.py:54`, `app/chamados/models.py:60`, `app/chamados/catalogo/motor.py:24`.

Nas linhas abaixo, **ritual T** significa: `_gravar_estado` grava `status_local`, `resolvido_em`, `via_encerramento`, `atualizado_em`; terminal recebe via e rebaixa urgência; vivo limpa carimbo/via e restaura urgência. Quando solicitado, grava também `respondido_por`. T solicita `LogAuditoria.acao='estado_chamado'`, com estado anterior, novo estado e via, antes do `save`, no mesmo `atomic`. **LEU:** `app/chamados/models.py:228`, `app/chamados/models.py:233`, `app/chamados/models.py:247`, `app/chamados/models.py:280`, `app/chamados/models.py:292`.

| Transição / gatilho | JUIZ que decide | OPERAÇÃO que escreve | Persistência / trilha | Retorno | Prova existente, lida |
|---|---|---|---|---|---|
| Inexistente → `aberto` | `decidir(EMITIR)`; classificação acionável em `_evento_nascimento`. `app/chamados/catalogo/motor.py:130`; `app/chamados/models.py:1047` | `abrir`, por `get_or_create`; ou `criar`, por `create`. `app/chamados/models.py:972`; `app/chamados/models.py:1096` | Chamado, vínculo com célula e `nascimento_chamado`; módulos de turno com célula emitem `EventoCelula`. `app/chamados/models.py:978`; `app/chamados/models.py:1098`; `app/chamados/models.py:1219` | `abrir`: `(ch, criado)`; `criar`: `ch`; recusas podem devolver ausência. `app/chamados/models.py:966`; `app/chamados/models.py:982`; `app/chamados/models.py:1075`; `app/chamados/models.py:1101` | `app/chamados/tests/test_chamado_nasce_com_trilha.py:30`; deduplicação: `app/chamados/tests/test_selo_dedup_premissa_viva.py:114` |
| Inexistente → `registrado`, módulo sem ação devida | `decidir(REGISTRAR)`. `app/chamados/catalogo/motor.py:133`; `_evento_nascimento`, `app/chamados/models.py:1047` | Mesmas portas de nascimento. `app/chamados/models.py:972`; `app/chamados/models.py:1096` | Nascimento com `status_local=registrado`; trilha de nascimento. `app/chamados/models.py:939`; `app/chamados/models.py:1219` | Mesmos retornos acima. `app/chamados/models.py:982`; `app/chamados/models.py:1101` | `app/chamados/tests/test_chamado_nasce_com_trilha.py:38` |
| `aberto/registrado` → `em_analise`, evento `ANALISAR` | `decidir`. `app/chamados/catalogo/motor.py:112` | `analisar` → T. `app/chamados/models.py:801` | Ritual T. `app/chamados/models.py:807` | `bool`. `app/chamados/models.py:806` | `app/chamados/tests/test_e1_v5_ordem_trava.py:100` |
| `aberto` → `em_analise`, ação humana | `pode_transicionar` → `decidir(TRANSICAO_HUMANA)`. `app/chamados/models.py:119`; `app/chamados/catalogo/motor.py:100` | `transicionar` → `_aplicar` → T. `app/chamados/models.py:199`; `app/chamados/models.py:447` | T, via `admin`, autor. `app/chamados/models.py:450` | `None`; recusa levanta `ValueError`. `app/chamados/models.py:128`; `app/chamados/models.py:199` | `app/chamados/tests/test_transicoes.py:57` |
| `aberto/em_analise` → `resolvido`, ação humana | Matriz humana, guardas de justificativa/disputa e S122; S122 pergunta `premissa_regularizada`. `app/chamados/catalogo/motor.py:100`; `app/chamados/models.py:136`; `app/chamados/models.py:181` | `transicionar`/`fechar` → `_aplicar` → T. `app/chamados/models.py:199`; `app/chamados/models.py:452` | T via `admin`; `fechar` pode anexar nota de fechamento. `app/chamados/models.py:450`; `app/chamados/models.py:456` | `None`; recusa pode levantar. `app/chamados/models.py:192`; `app/chamados/models.py:455` | `app/chamados/tests/test_transicoes.py:94`; `app/chamados/tests/test_transicoes.py:178` |
| `aberto/em_analise/registrado/resolvido` → `fechado`, ação humana | Matriz humana; S122 está condicionado ao destino `resolvido`, não a todo fechamento. `app/chamados/catalogo/motor.py:99`; `app/chamados/models.py:181` | `transicionar` → `_aplicar` → T. `app/chamados/models.py:199` | T via `admin`. `app/chamados/models.py:450` | `None`. `app/chamados/models.py:199` | `app/chamados/tests/test_contract_status_nucleo.py:49` |
| `em_analise` → `aberto`, ação humana | `decidir`, matriz humana. `app/chamados/catalogo/motor.py:101` | `transicionar` → `_aplicar` → T. `app/chamados/models.py:199` | T; permanece vivo, sem via de encerramento. `app/chamados/models.py:247` | `None`. `app/chamados/models.py:199` | Matriz exercitada em `app/chamados/tests/test_transicoes.py:57`; caso específico desta aresta **[não verificado]** |
| `resolvido` → `em_analise`, ação humana | Matriz humana. **Não passa por `recusa_da_reabertura`.** `app/chamados/catalogo/motor.py:102`; `app/chamados/models.py:199` | `transicionar` → `_aplicar` → T | T; limpa carimbo/via. `app/chamados/models.py:247` | `None`. `app/chamados/models.py:199` | Caso funcional específico **[não verificado]** |
| `fechado` → estado permitido por override `admin_ti` | Ramo especial de `decidir`. `app/chamados/catalogo/motor.py:150` | `transicionar` → `_aplicar` → T. `app/chamados/models.py:199` | T; campos dependem do destino. `app/chamados/models.py:233` | `None` ou recusa. `app/chamados/models.py:124` | `app/chamados/tests/test_transicoes.py:178` |
| VIVO → `resolvido`, premissa regularizada | `_decisao_do_fechamento`: motor + premissa + pergunta viva. Coletor reconhece justificativa, decisão admin, dia sem trabalho, marcos acesos/inexistentes e fatos do dia. `app/chamados/models.py:664`; `app/chamados/models.py:1253` | `reconciliar` → T. `app/chamados/models.py:642` | T via `sistema`; eventual reaponte de perguntas precede fechamento autorizado. `app/chamados/models.py:655`; `app/chamados/models.py:686` | `bool`; `reconciliaria` fornece o mesmo sim/não sem escrita. `app/chamados/models.py:656`; `app/chamados/models.py:658` | `app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:95`; `:113`; `:244` |
| `batida_ausente` VIVO → `resolvido`, marco aceso no evento | `marcos_acesos is True`, depois o gate de `reconciliar`. `app/ponto/services/lastro.py:395`; `app/ponto/services/lastro.py:401` | `fechar_pelo_marco_colab` → `transicionar_com_pauta(ato=reconciliar)` | T + ocorrência `lastro_encontrado`, no mesmo `atomic`. `app/ponto/services/lastro.py:400` | Número de fechados; falhas podem ser acumuladas. `app/ponto/services/lastro.py:407`; `app/ponto/services/lastro.py:409` | `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:97`; idempotência `:112`; rollback da nota `:152` |
| VIVO → `resolvido`, cartório: marco satisfeito/dia sem trabalho | **Sem juiz registrado para a composição `_protesta`**; combina `trabalha` e `marcos_acesos`, depois pergunta o gate. `app/ponto/services/cartorio.py:1458`; `app/ponto/services/cartorio.py:1539` | `_julgar_colab_corpo` → `reconciliar` → T | T; atualiza contadores e agenda rejulgamento. `app/ponto/services/cartorio.py:1541` | Resultado incorporado ao `out` do julgamento. `app/ponto/services/cartorio.py:1541` | Marco individual: `app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:185`; prova específica de toda a composição **[não verificado]** |
| VIVO → `resolvido`, marcos antigos inexistentes após troca | `marcos_inexistentes`, depois gate. `app/ponto/services/cartorio.py:1550`; `app/chamados/models.py:1298` | Cartório → `reconciliar` → T | T + nota de troca de escala; rejulga célula. `app/ponto/services/cartorio.py:1552`; `app/ponto/services/cartorio.py:1556` | Contagem em `out`. `app/ponto/services/cartorio.py:1563` | Referência aos REDs no próprio coletor: `app/chamados/models.py:1291`; execução **[não verificado]** |
| VIVO → `resolvido`, dedup/agrupado, mesmo com furo no dia | Pai vivo ou sobrevivente escolhido por C8. `app/chamados/models.py:739`; `app/chamados/models.py:749` | `reconciliar` → T | T; dedup acrescenta `fechamento_por_dedup`, com sobrevivente. `app/chamados/models.py:691`; `app/chamados/models.py:757` | `bool`. `app/chamados/models.py:656` | `app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:80`; agrupado `:143` |
| VIVO → `resolvido`, furo sem dia apurável | **Sem juiz do lastro:** exceção inline permite sem julgar premissa; guarda da pergunta viva ainda participa. `app/chamados/models.py:733`; `app/chamados/models.py:686` | `reconciliar` → T | T + `fechamento_sem_dia_do_fato`. `app/chamados/models.py:735` | `bool`. `app/chamados/models.py:656` | `app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:130` |
| VIVO → `resolvido`, retratação da premissa | `_veredito_premissa` → catálogo `avaliar`; `_retratar` aplica retenções. `app/chamados/reconciliador.py:248`; `app/chamados/reconciliador.py:504` | `retratar_premissa_morta_colab` → `_retratar` → `reconciliar` | T, tratamento das perguntas/justificativa pendente e nota `retratado`. `app/chamados/reconciliador.py:524`; `app/chamados/reconciliador.py:533`; `app/chamados/reconciliador.py:539` | Unidade `bool`; lote `int`. `app/chamados/reconciliador.py:542`; `app/chamados/reconciliador.py:1471` | `app/chamados/tests/test_selo_retratador_turno_do_chamado.py:81`; `app/chamados/tests/test_retratar_rejeita_justificativa.py:48` |
| Chamado de turno VIVO → `resolvido`, célula absolveu | `_absolvido_pela_celula`: veredito não acusador e nenhum marco pendente. `app/chamados/reconciliador.py:1019` | `retratar_chamados_absolvidos_por_celula` → `_retratar` | Mesmo ritual de retratação. `app/chamados/reconciliador.py:962`; `app/chamados/reconciliador.py:504` | Número de retratados. `app/chamados/reconciliador.py:1016` | `app/chamados/tests/test_selo_retratador_turno_do_chamado.py:95`; `:117` |
| Origem VIVA → `resolvido`, justificativa aprovada | Aprovação é o fato; retenção é da unidade de retratação e regularização é do coletor. `app/ponto/services/justificativa.py:16` | `encerrar_chamado_de_origem` → `retratar`, dentro da porta de aprovação | T + nota da aprovação; pode registrar retenção sem mudar estado. `app/ponto/services/justificativa.py:27`; `app/ponto/services/justificativa.py:46` | `bool`. `app/ponto/services/justificativa.py:33` | `app/ponto/tests/test_justificativa_encerra_origem.py:74`; `:119`; `:141` |
| VIVO → `resolvido`, ausência aprovada | C11 `reconciliar_por_ausencia`: cobertura + `_premissa_morre` + gate. `app/core/juizes.py:287`; `app/chamados/services/silencio_ausencia.py:94` | `reconciliar_por_ausencia` → `reconciliar` | T + nota `falta_redundante_ausencia`, trilha `reconciliar_ausencia_lancada`; pode levantar silêncio. `app/chamados/services/silencio_ausencia.py:102` | `'fechou'`, outros resultados nomeados ou `None`. `app/chamados/services/silencio_ausencia.py:53` | `app/ponto/tests/test_reavaliar_catalogo_premissa.py:98` |
| VIVO → `resolvido`, feriado/folga | `espurios` pergunta A2 e C3; gate confirma. `app/chamados/services/feriado_reavaliar.py:24` | `encerrar` do serviço → `reconciliar` | T + nota `falta_espuria_feriado` + auditoria `reconciliar_feriado`. `app/chamados/services/feriado_reavaliar.py:48` | Unidade `bool`; lote `int`. `app/chamados/services/feriado_reavaliar.py:57`; `:86` | `app/ponto/tests/test_reavaliar_catalogo_premissa.py:98` prova ausência; prova específica de feriado nesta leitura **[não verificado]** |
| VIVO → `resolvido`, tentativa por ordem de cobertura/substituição | C3 recebe fatos falsos do chamador, mas gate pode recusar porque o dia permanece furado. `app/ponto/services/cobertura.py:118`; `app/ponto/services/cobertura.py:312` | Portas de cobertura → `reconciliar` | T somente se aceito; nota distingue coberto de chamado ainda aberto. `app/ponto/services/cobertura.py:123`; `app/ponto/services/cobertura.py:316` | Resultado da porta inclui recusados. `app/ponto/services/cobertura.py:319` | `app/ponto/tests/test_cobertura_followup.py:20`; cobertura completa das recusas **[não verificado]** |
| Pai VIVO → `resolvido`, disputa fechada | Receiver lê `fechada_em`; a premissa é decidida pelo gate. `app/chamados/signals.py:20`; `app/chamados/signals.py:36` | Receiver → `reconciliar` | T + nota `fechado_pela_disputa`, em `atomic`. `app/chamados/signals.py:42` | Receiver retorna `None`. `app/chamados/signals.py:44` | `app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:268`; `:278` |
| VIVO/`resolvido` → `fechado`, cascata | `decidir(ENCERRAR)` + guarda da pergunta viva; não pergunta o gate da premissa. `app/chamados/models.py:1615` | `encerrar` → T | T via `cascade`. `app/chamados/models.py:1620` | `bool`. `app/chamados/models.py:1621` | `app/chamados/tests/test_contract_status_nucleo.py:61`; `app/ponto/tests/test_justificativa_encerra_origem.py:298` |
| VIVO → `cancelado` | `decidir(CANCELAR)` + guarda da pergunta viva. `app/chamados/models.py:784` | `cancelar` → T | T via `cancelado`; nota se houver motivo. `app/chamados/models.py:789` | `bool`; terminal absoluto. `app/chamados/models.py:793`; `app/chamados/catalogo/motor.py:142` | `app/chamados/tests/test_motor_cancelado.py:22` |
| VIVO → `fechado`, competência trancada | Serviço `medir` seleciona competência e exclusões; gate pergunta motor/fio. `app/chamados/services/competencia_trancada.py:66`; `app/chamados/models.py:1623` | Serviço `encerrar` → `encerrar_por_competencia_trancada` → T | T via `competencia_trancada`, nota e auditoria própria. `app/chamados/models.py:1640`; `app/chamados/services/competencia_trancada.py:210` | Gate `bool`; serviço devolve medição com aplicadas. `app/chamados/models.py:1641`; `app/chamados/services/competencia_trancada.py:234` | `app/chamados/tests/test_competencia_trancada.py:68`; `:106`; `:211` |
| `resolvido/fechado` → `em_analise`, reabertura pela porta própria | `recusa_da_reabertura`: veto, admin sem fato novo, premissa, agrupado, irmão e motor. `app/chamados/models.py:1319` | `reabrir` → T → `ressuscitar_perguntas` | T; limpa via/carimbo; ressuscita apenas universo aprovado pelos juízes. `app/chamados/models.py:1443`; `app/chamados/models.py:1469` | `bool`; recusa pode registrar motivo/redirecionar perguntas. `app/chamados/models.py:1395`; `:1412`; `:1453` | `app/chamados/tests/test_reabrir_pergunta_a_premissa.py:61`; `app/chamados/tests/test_reabrir_decide_pelo_banco.py:1` |
| Encerrado reencontrado pelo emissor → `em_analise`, renascimento | `_renascer_por_premissa_viva` aplica restrições e pergunta `reabrir`. `app/chamados/models.py:984` | Wrapper → `reabrir` → T | T + nota `renascido`. `app/chamados/models.py:1020` | `bool`; `abrir` devolve chamado existente. `app/chamados/models.py:1025`; `:982` | `app/chamados/tests/test_selo_dedup_premissa_viva.py:85`; `:128` |
| Encerrado pela tranca → `em_analise`, competência reaberta/desarquivamento | Serviço seleciona via/competência; `reabrir(humano=True)` conserva as demais guardas. `app/chamados/services/competencia_trancada.py:260`; `:290` | Serviço → `reabrir`; desarquivamento também levanta véu | T + nota `competencia_reaberta` + auditoria própria. `app/chamados/services/competencia_trancada.py:291` | Serviço `int`; tela JSON. `app/chamados/services/competencia_trancada.py:298`; `app/chamados/views_arquivar.py:89` | `app/chamados/tests/test_tranca_de_verdade.py:74`; `app/chamados/tests/test_competencia_trancada.py:164` |
| Arquivar/suspender **sem mudar estado** | Prazo do motor e motivo do chamador. `app/chamados/services/arquivo.py:107`; `:141` | `arquivo.silenciar` | `silenciado_ate/por/motivo`, `atualizado_em` e auditoria da ação. `app/chamados/services/arquivo.py:166` | `bool`. `app/chamados/services/arquivo.py:174` | `app/chamados/tests/test_porta_arquivo_silencio.py:314`; estado intacto `app/chamados/tests/test_f1_arquivar.py:42` |
| Levantar arquivo/suspensão **sem mudar estado** | Guardas da porta de arquivo. `app/chamados/services/arquivo.py:177` | `arquivo.levantar` | Limpa silêncio; pode carimbar primeiro `resgatado_em`; auditoria. `app/chamados/services/arquivo.py:202` | `bool`. `app/chamados/services/arquivo.py:212` | `app/chamados/tests/test_porta_arquivo_silencio.py:233`; `:336` |

`superado` permanece como estado de acervo, mas a porta `superar` foi retirada; não o conto como transição executável corrente. **LEU:** `app/chamados/models.py:795`.

`EventoCelula` é a entrega do fato ao consumidor, não a trilha da transição do chamado: nasce por `emitir_evento_celula`; o consumidor executa retratações/lastro e grava `processado_em`, ou incrementa `tentativas/erro`, ou adia pelo teto. **LEU:** `app/ponto/portas/celula.py:945`, `app/chamados/reconciliador.py:1364`, `app/chamados/reconciliador.py:1383`, `app/chamados/reconciliador.py:1393`.

**Limite da persistência:** T solicita auditoria, mas `evento` é explicitamente *best-effort* e retorna `None` se falhar. **INFERIU:** o HEAD não garante incondicionalmente “estado persistido implica uma trilha”. **LEU:** `app/core/observ.py:94`, `app/core/observ.py:105`, `app/core/observ.py:133`.

---

**2. Censo AST: script e resultado**

O script abaixo lê e analisa texto Python; não importa aplicações nem acessa banco. A parte de escritores procura atribuições e chamadas ORM, incluindo `defaults`/`update_fields` em variáveis locais. A parte de leitores indexa o manifesto de decisões encontrado na leitura e compara chamadas diretas com o registro. **INFERIU — instrumento proposto.** Chamadas dinâmicas, `setattr` com nome calculado e resolução completa de tipos são **[não verificado]** por esse instrumento.

```python
import ast
from pathlib import Path

APP = Path("app")
CAMPOS = {"status_local", "via_encerramento", "fechado_em"}

LEITORES = {
    "ponto/services/lastro.py":
        "marcos_inexistentes marcos_acesos julgar",
    "chamados/models.py":
        "_decisao_do_fechamento _gate_premissa_do_fechamento "
        "premissa_regularizada",
    "chamados/reconciliador.py":
        "turnos_abertos_de turno_do_chamado_aberto _tem_turno_aberto "
        "_par_integro _ancora_ausente _filhos_vivos _veredito_premissa "
        "_absolvido_pela_celula veredito_orfa",
    "chamados/services/silencio_ausencia.py":
        "_premissa_morre reconciliar_por_ausencia",
    "chamados/services/feriado_reavaliar.py": "espurios",
    "ponto/services/cartorio.py": "_protesta",
    "ponto/services/cluster_espurio.py":
        "_leitura_da_celula _vivos_que_a_premissa_matou",
    "chamados/juizes.py":
        "_celula_furada _pergunta_materializada_de_fato "
        "_pergunta_viva_calc via_pelo_fato",
}

def producao(p):
    return not (
        "migrations" in p.parts
        or any(x == "tests" or x == "tests.py"
               or x.startswith(("test_", "tests_")) for x in p.parts)
    )

def strings(n):
    return {
        x.value for x in ast.walk(n)
        if isinstance(x, ast.Constant) and isinstance(x.value, str)
    }

registro = ast.parse((APP / "core/juizes.py").read_text())
jui = next(
    n.value for n in registro.body
    if isinstance(n, ast.Assign)
    and any(isinstance(t, ast.Name) and t.id == "JUIZES"
            for t in n.targets)
)
JUIZES = {s for s in strings(jui) if "::" in s}

for p in sorted(APP.rglob("*.py")):
    if not producao(p):
        continue
    tree = ast.parse(p.read_text())
    pais = {c: n for n in ast.walk(tree)
            for c in ast.iter_child_nodes(n)}

    def funcao(n):
        while n in pais:
            n = pais[n]
 

[truncado em 20000 de 46357 caracteres]

## 13/11 18:00 · sessao 751ef6d6ed02 · O168 · verificador · origem=Code

**INFERIU: o BLOQ 1 específico foi curado; a igualdade geral entre CSF e retratação não foi estabelecida, porque os universos continuam diferentes.** Os dois ramos do CSF agora ignoram a tranca, como a retratação. (`app/ponto/supra_juiz.py:573–588`; `app/chamados/juizes.py:2033–2035`)

1. **CSF × retratação**

   **LEU:** o predicado comum exige ausência de validação e materialização, mais `respondida_em` preenchido **OU** texto não vazio. Com `ignorar_tranca=True`, não consulta a tranca. (`app/chamados/models.py:1898–1902`)

   **INFERIU:** para o mesmo chamado e o mesmo estado lido, os conjuntos são:

   | Pergunta pendente | CSF segura? | `irmas_respondidas_sem_consumo` segura? |
   |---|---:|---:|
   | Do pai, destinada a outro chamado **vivo** — portanto não creditada ao pai | Sim | Não |
   | Do pai, com destino nulo ou não vivo | Sim | Sim |
   | Destinada ao chamado, embora pertença a outro pai | Sim | Sim |

   A tabela decorre do CSF ler **pai inteiro ∪ destino**, enquanto a retratação lê **destino ∪ pai sem outro destino vivo**. (`app/ponto/supra_juiz.py:558`, `:573–574`; `app/chamados/juizes.py:2033–2035`)

   **INFERIU:** a retratação é subconjunto do CSF: desaparece “CSF libera/retratação segura” por essa diferença de tranca; permanece “CSF segura/retratação libera” no primeiro caso da tabela. Isso compara as decisões sobre perguntas, sem garantir igualdade com todas as demais condições do executor. (`app/ponto/supra_juiz.py:573–588`; `app/chamados/reconciliador.py:438–455`)

2. **C7 item 8, leitores e saída da fila**

   **LEU:** a isenção permanece no comportamento default do juiz e no C6: pergunta factual de dia trancado deixa de bloquear por leitura, sem receber carimbo. (`app/chamados/models.py:1896–1902`; `app/chamados/juizes.py:1403–1409`)

   **LEU/INFERIU:** estes são os leitores diretos encontrados e o efeito do delta:

   | Leitor | Regime atual | Mudança direta |
   |---|---|---|
   | `pode_fechar` e `fechar` | Default: respeitam a isenção | Nenhuma. (`app/chamados/models.py:1909`, `:1942`) |
   | Desligamento | Default | Nenhuma. (`app/colaboradores/services/desligamento.py:141–143`) |
   | Ficha do colaborador | Default, intersectado com perguntas do app | Nenhuma. (`app/colaboradores/services/ficha.py:166–171`) |
   | Gaveta `respondido_a_decidir` | Default | Nenhuma. (`app/chamados/services/fila_por_causa.py:225–227`) |
   | Comando `validar_classe_a` | Default | Nenhuma. (`app/chamados/management/commands/validar_classe_a.py:44–55`) |
   | Reconciliador de órfãs | Default | Nenhuma. (`app/chamados/reconciliador.py:1164–1173`) |
   | Medição da tranca/Pauta DP | Já usava `True` | Nenhuma. (`app/chamados/services/competencia_trancada.py:124–139`) |
   | Retratação | Já usava `True` | Nenhuma. (`app/chamados/juizes.py:2033–2035`) |
   | CSF, pai e destino | Agora usa `True` | Passa a segurar resposta pendente mesmo quando a tranca a isentaria. (`app/ponto/supra_juiz.py:573–574`) |

   **INFERIU:** existe mudança **indireta** na Defesa 1: a disputa pode fechar pela isenção C7, mas o pai permanecer vivo. O signal tenta `ch.reconciliar`; esse caminho passa pelo gate de conversa, que agora segura a resposta trancada. (`app/chamados/signals.py:43–46`; `app/chamados/models.py:686`, `:719–740`)

   **LEU:** a saída existente é `competencia_trancada.encerrar`: seleciona chamados elegíveis da competência, sem perguntas vivas, e chama `encerrar_por_competencia_trancada`. Essa porta consulta `_gate_pergunta_viva`, **sem consultar o gate de conversa**, e grava a via `competencia_trancada`. (`app/chamados/services/competencia_trancada.py:101–118`, `:160–171`, `:206–220`; `app/chamados/models.py:1669–1686`)

   **LEU:** Pauta DP não encerra o chamado: registra as perguntas para decisão no calendário, depois do processamento dos encerramentos. Existe retorno anterior à Pauta quando `acoes == 0`. (`app/chamados/services/competencia_trancada.py:190–193`, `:232–257`)

   **INFERIU:** há caminho de saída, mas **não há garantia de saída automática para tranca já viva, sem novo ato**. O disparo ocorre somente ao criar/retrancar; o comando do passivo não tem cron. Portanto o pai pode continuar retido até uma execução dessa via ou outro ato aplicável. (`app/ponto/services/fechamento.py:1411–1428`; `app/config/crons.py:1276–1279`; `app/chamados/management/commands/encerrar_por_competencia_trancada.py:29`)

   **LEU, anexo:** a frota v8 informa quatro `liberado__conversa_pendente`; ela não demonstra o destino desses quatro na v9, nem identifica um caso sem saída automática. **[não verificado]**

3. **Selo de custo**

   **LEU:** o novo caso deixa **todas** as N perguntas pendentes e compara a quantidade de queries com 1 e 3 disputas, chamando o gate real em `registrar=False`. Ele cobre o buraco da fixture anterior, que deixava apenas uma pendente. (`app/chamados/tests/test_o168_custo_da_guarda_constante.py:47–58`, `:69–75`)

   **LEU:** `_perguntas_do_pai` não tem `select_related` de disputa/colaborador. **INFERIU:** isso não produz N+1 neste caminho atual: `ignorar_tranca=True` interrompe o `or` antes de `dia_da_pergunta_trancado`, e o predicado só acessa colunas da pergunta. O acesso às relações estaria no leitor da tranca. (`app/ponto/supra_juiz.py:558`, `:573–574`; `app/chamados/models.py:1898–1902`; `app/chamados/juizes.py:991–992`)

   **LEU/INFERIU:** a nota também não relê cada pergunta: recebe `_pend` já calculado e formata seus PKs. Contudo, o selo novo não mede a escrita da nota, pois usa `registrar=False`; prova constância de queries, não custo constante de processamento ou tamanho da lista. (`app/chamados/models.py:719`, `:735–739`; `app/chamados/tests/test_o168_custo_da_guarda_constante.py:56–61`)

   Quantidades SQL efetivamente executadas em Django nesta verificação: **[não verificado]**.

4. **RED e mutantes de um único universo**

   **INFERIU:** ambos os mutantes são pegos pelo **mesmo `assertEqual` da linha 307**, em iterações diferentes:

   | Mutante | Primeira falha |
   |---|---|
   | Retirar o kwarg do universo **destino**, `supra_juiz.py:574` | Linha 307, `quem='destino'` |
   | Retirar o kwarg do universo **pai**, `supra_juiz.py:573` | Destino passa; linha 307, `quem='pai'` |

   A fixture distingue pai e destino, simula tranca verdadeira e exige `[p.pk]` em ambos. O `assertFalse` da linha 310 não chega a executar na iteração que já falhou na linha 307. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:44–57`, `:300–310`)

   **LEU:** o RED consulta `irmas_respondidas_sem_consumo(destino)`, mas não consulta `irmas_respondidas_sem_consumo(pai)`; portanto não certifica igualdade entre os universos. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:304–310`)

   Confirmei os dois resultados em ensaio em memória com os corpos extraídos; execução do `TestCase` Django e dos mutantes contra ORM: **[não verificado]**.

5. **Quem escreve e quem lê os campos**

   **LEU:** o delta altera a opção de leitura da tranca no CSF; o predicado continua lendo os quatro campos abaixo. (`app/ponto/supra_juiz.py:573–574`; `app/chamados/models.py:1898–1902`)

   | Campo | Escritores e reversões | Leitores relevantes |
   |---|---|---|
   | `resposta_colab` | Resposta humana; automática sem sobrescrever texto; reabertura limpa. (`app/chamados/services/disputa_emissao.py:1467–1469`, `:1647–1656`, `:1593`) | Juiz O168, C6 e conteúdo do plantio. (`app/chamados/models.py:1900–1901`; `app/chamados/juizes.py:1417`; `app/chamados/services/materializacao.py:412–419`) |
   | `respondida_em` | Resposta humana/automática; reparo pelo instante testemunhado na trilha; `carimbar` preenche nulo com texto; reabertura limpa. (`app/chamados/services/disputa_emissao.py:1462–1468`, `:1652–1653`, `:1661–1678`, `:1593`; `app/chamados/models.py:3186–3189`) | O168 lê nulidade; validação exige preenchido; materializador também. (`app/chamados/models.py:1900`; `app/chamados/services/validacao.py:416–418`; `app/chamados/services/materializacao.py:1375–1378`) |
   | `validada_em` | `_carimbar_validacao`; preenchimento por `carimbar(resposta='validar')`; nova resposta, recusa e reabertura desfazem. (`app/chamados/services/validacao.py:150–164`, `:218–228`; `app/chamados/models.py:3190–3193`; `app/chamados/services/disputa_emissao.py:1480–1482`, `:1593`) | O168 exclui; C6 encerra; materializador seleciona. (`app/chamados/models.py:1899`; `app/chamados/juizes.py:1400–1402`; `app/chamados/services/materializacao.py:1375–1378`) |
   | `materializada_em` | Atribuição central `_escrever_carimbo`, usada pelas portas normal/identidade; descarimbo avulso/lote limpa. (`app/chamados/models.py:3153–3157`, `:3180–3208`, `:3224–3246`, `:3259–3268`, `:3307–3322`) | O168 exclui; C6 lê junto da via; fila de materialização exige nulo. (`app/chamados/models.py:1899`; `app/chamados/juizes.py:1385–1395`, `:1412–1413`; `app/chamados/services/materializacao.py:1375–1378`) |
   | Tranca | `trancar_periodo` cria/retranca; `reabrir_periodo` preenche `reaberto_em`. (`app/ponto/services/fechamento.py:1400–1419`, `:1432–1441`) | Juiz do período lê `reaberto_em IS NULL`; juiz da pergunta combina competência e ausência de toque posterior; C6 e juiz default leem essa resposta. (`app/ponto/services/fechamento.py:1464–1480`; `app/chamados/juizes.py:1062–1079`, `:1237–1251`, `:1405`; `app/chamados/models.py:1902`) |

   **LEU:** os carimbos são instantes dos atos, não o dia discutido. A pergunta factual recebe dia do turno; conversa recebe dia civil do pedido. O leitor da tranca devolve “sem dia de turno” para conversa e usa `data_evento` para fato. (`app/chamados/services/disputa_emissao.py:1462–1468`; `app/chamados/services/validacao.py:157`; `app/chamados/models.py:3180`, `:2840–2858`; `app/chamados/juizes.py:334–338`, `:989–992`)

   **INFERIU:** não há vocabulário uniforme: O168 aceita **carimbo OU texto**; validar exige carimbo; a cláusula de resposta do C6 exige **carimbo E texto**. Texto sem `respondida_em` pode segurar o fechamento e receber `sem_resposta` na validação. Há reparo pela trilha, cuja disponibilidade nos casos de produção é **[não verificado]**. (`app/chamados/models.py:1900–1901`; `app/chamados/services/validacao.py:416–418`; `app/chamados/juizes.py:1417`; `app/chamados/services/disputa_emissao.py:1661–1678`)

   **INFERIU:** `materializada_em` também não prova batida criada: a porta de identidade atende admin/dedup/colisão, embora o help text prometa “batida criada + chamado fechado”. (`app/chamados/models.py:3224–3246`, `:3449–3451`)

6. **LEI-AKITA, commit e dinheiro** [nao verificado]

   A tentativa de `git -C` na cópia retornou “not a git repository”. A mensagem de e050e522f e sua linha LEI-AKITA permanecem **[não verificado]**; a afirmação da triagem sobre oito commits não substitui essa conferência. **[não verificado]**

   **LEU:** a lei exige origem, testemunha, RED, censo, juízes novos e padrão; exige também conferir se o padrão nomeado corresponde ao diff. **INFERIU:** o delta mantém o predicado compartilhado e muda a política de tranca dos dois leitores CSF, acrescentando os dois selos; não demonstra igualdade de universos nem cria outra autoridade. O juiz existente está registrado. (`CLAUDE.md:62–75`; `app/ponto/supra_juiz.py:573–574`; `app/chamados/tests/test_o168_custo_da_guarda_constante.py:69–75`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:291–310`; `app/core/juizes.py:294–295`)

   **INFERIU:** o impacto demonstrável é na retenção de chamados e na cascata de fechamento. Não há escrita monetária nessas duas chamadas alteradas, mas isso não certifica impacto financeiro zero: a retratação também controla resolução de perguntas e rejeição automática de justificativa pendente. O valor financeiro efetivo, por pessoa/competência/rubrica, permanece **[não verificado]**. (`app/ponto/supra_juiz.py:573–588`; `app/chamados/models.py:719–740`; `app/chamados/reconciliador.py:519–537`)

## 13/11 17:59 · sessao 1c27186a31ab · - · - · origem=Ronald (tmux)

LEU: as exceções estruturais já estão vazias: `app/core/juizes.py:470` contém `PENDENTES['chamado'] = ()`; alias em `:1366`; `app/config/crons.py:2035` a `:2100` contém somente comentários, nenhum elemento da tupla. A célula permanece declarada falsa porque omite `verde=True`, cujo default é `False` (`app/core/contratos_estruturais.py:62`, `:262`); a nota ainda exige O168/O169/LASTRO (`:270`). Não há conteúdo residual nessas duas listas. O teste cobra pendentes ≤0 (`app/chamados/tests/test_contract_juiz_chamado.py:85`, `:98`).

LEU: `lastro.fechar` não existe no HEAD; quem escreve pelo lastro é `fechar_pelo_marco_colab` (`app/ponto/services/lastro.py:364`). O cron é vigia, aceita `--alarme`, mede uma vez e sai 2 se houver escape (`app/ponto/management/commands/fechar_cobranca_com_lastro.py:44`, `:53`, `:77`); configuração `--alarme` em `app/config/crons.py:731`.

O censo lido identifica estes decisores:

| Sítio | Pergunta registrada ou deriva |
|---|---|
| `app/ponto/services/lastro.py:98`, `marcos_inexistentes` | Deriva: todos os marcos cobrados ausentes de ata com lâmpadas. Não consta em `JUIZES` (`app/core/juizes.py:276`). |
| `app/ponto/services/lastro.py:153`, `marcos_acesos` | Deriva: todos os marcos nomeados acesos; `None` sem marco/tipo/lâmpada; não registrado (`app/core/juizes.py:276`). |
| `app/ponto/services/lastro.py:196`, `julgar` | Lê lâmpada em `:289`; sem ela deriva casamento linear de tipo e distância ≤90 min em `:304`. Pergunta `reconciliaria` em `:318`. |
| `app/chamados/models.py:1253`, `premissa_regularizada` | Coletor misto: justificativa aprovada `:1267`, admin `:1283`, C18 `dia_sem_trabalho` `:1285`, `marcos_acesos` `:1289`, `marcos_inexistentes` `:1298`, `_celula_furada` `:1300`, A2/C3 via `espurios` `:1306`, C3 `avaliar` `:1315`. |
| `app/chamados/models.py:718`, `_gate_premissa_do_fechamento` | Delega ao coletor `:739`; acrescenta dedup C8 `:749`; sem dia permite fechar com trilha `:733`. |
| `app/chamados/models.py:664`, `_decisao_do_fechamento` | Motor registrado `:674`; combina gate premissa `:683` e pergunta viva `:686`. |
| `app/chamados/reconciliador.py:83`, `turnos_abertos_de` | Deriva query entrada presente/saída ausente `:97`; veto por `dia_posicional_completo` `:113`. |
| `app/chamados/reconciliador.py:118`, `turno_do_chamado_aberto` | Delega ao anterior `:143`; recorta entrada declarada `:146` ou data_turno `:156`. |
| `app/chamados/reconciliador.py:162`, `_tem_turno_aberto` | Delega com dia `:173`; sem dia deriva `.exists()` global `:174`. |
| `app/chamados/reconciliador.py:182`, `_par_integro` | Deriva identificação por hora única `:213`, retratação `:217` e tipos alternados `:222`. |
| `app/chamados/reconciliador.py:227`, `_ancora_ausente` | Deriva vínculo ativo sem âncora `:233`. |
| `app/chamados/reconciliador.py:239`, `_filhos_vivos` | Lê VIVOS registrados `:247`; sem IDs devolve desconhecido `:244`. |
| `app/chamados/reconciliador.py:248`, `_veredito_premissa` | Delega à implementação da C3, `avaliar`, em `:258` e `:302`; coleta turno `:288`, âncora `:292`, filhos `:294`, par `:296`, geofence `:301`. Falta/cluster/intervalo não são coletados aqui: ficam desconhecidos pela C3 (`app/chamados/catalogo/premissa.py:127`). |
| `app/chamados/reconciliador.py:1019`, `_absolvido_pela_celula` | Deriva veredito não acusador `:1028` e ausência de marcos pendentes `:1030`. |
| `app/chamados/reconciliador.py:1061`, `veredito_orfa` | Compõe fato do marco `:1085`, C6 `:1087`, teto civil `:1089` e dia encerrado `:1091`. |
| `app/chamados/services/silencio_ausencia.py:29`, `_premissa_morre` | Chama C3 com três fatos falsos `:37`; deriva veredito não acusador `:41`. `None` também satisfaz o teste `:42`. |
| `app/chamados/services/silencio_ausencia.py:52`, `reconciliar_por_ausencia` | Autoridade registrada C11 (`app/core/juizes.py:287`); pergunta cobertura `:66`; aprovada passa pelo anterior `:94`. |
| `app/chamados/services/feriado_reavaliar.py:24`, `espurios` | Pergunta A2 `:34`, deriva faixa e previsto `:35`, e pergunta C3 `:39`. |
| `app/ponto/services/cartorio.py:1458`, `_protesta` | Deriva `trabalha is not False` e lê `marcos_acesos` `:1459`. Exceção declarada à segunda cláusula do C18 em `app/core/juizes.py:145`. |
| `app/ponto/services/cluster_espurio.py:15`, `_leitura_da_celula` | C18 `:25`; depois deriva assinatura ≥2 órfãs em 15 min e ≥1 apagada `:31`. |
| `app/ponto/services/cluster_espurio.py:53`, `_vivos_que_a_premissa_matou` | Lê próprio dia sem limite de idade `:68`; preserva desconhecido `:75`; pergunta C3 `:76`. |
| `app/chamados/juizes.py:459`, `_celula_furada` | Pergunta cobertura `:476` e C18 `:494`; deriva apagadas `:508` e órfãs `:510`. |
| `app/chamados/juizes.py:662`, `_pergunta_materializada_de_fato` | Implementação interna do C6: vias `:679`, plantio `:755`, destino encerrado `:780`, lei do dia `:790`. |
| `app/chamados/juizes.py:1399`, `_pergunta_viva_calc` | Implementação do juiz registrado C6; tranca C20 `:1405`. |
| `app/chamados/juizes.py:1715`, `via_pelo_fato` | Classifica via; consulta cobertura em `:1756`. |

“Sem juiz registrado direto” não equivale automaticamente a duplicação: `avaliar` é a implementação chamada pelo juiz registrado `premissa_morta` (`app/chamados/catalogo/premissa.py:149`); os auxiliares do C6 pertencem ao corpo de sua autoridade (`app/core/juizes.py:282`). Isso precisa aparecer na interpretação do AST.

INFERIU: a função existente mais próxima da autoridade ampla do lastro é `ChamadoColaborador.premissa_regularizada`, pois fechar e reabrir já compartilham sua resposta (`app/chamados/models.py:1254`, `:1261`, `:739`, `:1350`). A autoridade da transição é `_decisao_do_fechamento`, que já combina motor, premissa e fio (`app/chamados/models.py:674`, `:683`, `:686`). Falta ao coletor retorno rico, desconhecido explícito, fonte/prova, relógio/carga explícitos e alcance às classes coletadas separadamente pelo reconciliador (`app/chamados/models.py:1253`, `:1317`; `app/chamados/reconciliador.py:263`).

INFERIU: contrato proposto pode separar parecer puro e aplicação. Parecer com identidade do chamado/fato/dia/marcos, resultado `sustentado | regularizado | desconhecido`, fundamento e evidências; plano da transição com destino/via/recusa/perguntas a reatar. A aplicação continua na porta existente; o consumidor e vigia leem o mesmo parecer. Bases existentes: tríade da C3 (`app/chamados/catalogo/premissa.py:37`, `:124`), tríade dos marcos (`app/ponto/services/lastro.py:154`), plano da pergunta viva (`app/chamados/models.py:374`) e decisão comum dry/apply (`app/chamados/models.py:658`).

Provas existentes, lidas sem execução: fecho e nota no evento (`app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:97`), segunda passada sem nota extra (`:112`), marco apagado (`:122`), todos ou nenhum (`:130`), recusa sem nota (`:137`), falha de nota desfaz fecho (`:152`), degrau 2 continua escape mesmo com evento (`:234`), cron vigia (`:275`). A2 mede estado/nota/contador, não conta explicitamente `LogAuditoria.estado_chamado` (`:117`–`:120`).

Pontos a confrontar nas perguntas de lei, sem inventar regra:

- Desconhecido: C3 conserva fato não coletado (`app/chamados/catalogo/premissa.py:127`); gate sem dia fecha com trilha (`app/chamados/models.py:733`). São dois decisores com tratamentos distintos de ignorância.
- Lâmpada `acesa=None`: `marcos_acesos` rejeita (`app/ponto/services/lastro.py:179`); `_celula_furada` só prende por `is False` e pode regularizar (`app/chamados/juizes.py:507`); reconciliador exige zero pendentes (`app/chamados/reconciliador.py:1030`). Três sítios.
- Degrau 2 linear versus papel do marco é dívida já decidida: juiz do par registrado (`app/core/juizes.py:211`), regra posicional L-115 (`app/docs/LEIS.md:151`), conta própria ainda em `app/ponto/services/lastro.py:304`. Não contar como lei nova aberta.

## 13/11 17:58 · sessao 998bf5247530 · - · - · origem=Ronald (tmux)

LEU = código/documento da cópia. INFERIU = comparação dos anexos com esse código. Os diffs anexos não têm arquivo local verificável; suas mudanças são “LEU anexo; [não verificado no HEAD]” até a comparação abaixo.

**Resultado medido da comparação.** Calculei em memória o SHA1 no formato de blob Git dos 21 arquivos de produção dos diffs 164050/215253: 20 são byte a byte iguais ao lado anterior de pelo menos um dos diffs. A exceção é `app/folha/export.py`, cujo arquivo diverge, mas o hunk de `prontidao` continua igual (`app/folha/export.py:1095`). O HEAD de `ausencia.py` e `cartorio.py` coincide com a base da 164050, enquanto a 215253 os tinha rebasedos em bases diferentes. Não foi realizado merge ou apply; conflito mecânico de merge é [não verificado].

| Raia | O que resolve no anexo | Situação lida no HEAD | Sobreposição/conflito |
|---|---|---|---|
| `lastro3` rescaldo | Troca recuo linear pelo juiz do par; extrai compositor `grade_do_julgamento`; exige papel/cadeia, trata noturno e final versus intervalo; elimina tolerância local. | Pendência continua: `TOLERANCIA_MIN` em `lastro.py:59`; recuo próprio em `:299`, comparação por tipo/instante linear em `:307`; grade montada inline no cartório em `cartorio.py:1156` e `:1175`. | **Parte superada:** o diff altera `fechar()` e `--apply`, que não são a porta atual. Hoje comando é vigia (`commands/fechar_cobranca_com_lastro.py:20`, `:44`) e escritor por evento é `lastro.py:364`. Builder exige `agora=` (`escala/utils.py:2084`), ausente no compositor do diff antigo. Não transplantar literalmente: INFERIU destas referências. |
| `204847`, c3 | Mapa explícito, `TipoSemFaixa`, contador de tipos ativos sem faixa. | Continua o default `'atestado'` em `precedencia.py:76`; mapa pequeno em `:52`; contador não aparece na lista que começa em `vigia_ausencia.py:154`. | É a mesma raiz c3 incluída no histórico 215253; aplicar duas vezes seria duplicar mudança (LEU anexo; INFERIU). |
| `164050`, c3–c12 | Além da c3: separa cobertura de rótulo, preserva efeito histórico de inativos, transporta campos pela ata, recusa nomeada em leitores e cadastro, isola colaborador no cartório; recusa fica em buraco. | Os sítios correspondentes continuam com o comportamento anterior, detalhados abaixo. | É base documental escolhida pelo redesenho, `BACKLOG.md:1837`. Sobrepõe quase toda a 215253 até c12; não são duas obras independentes (LEU anexo; INFERIU). |
| `215253`, c3–c19 | Inclui c3–c12; depois remove montagem própria de grade no PDF, conserva recusa no builder, põe instante com fuso em órfãs desde o juiz, transporta ata→tela→PDF, identifica órfã por instante nos três consumidores e na reconciliação. | Continua só HH:MM no produtor (`escala/utils.py:326`), na ata (`cartorio.py:745`), no leitor (`leitor_celula.py:187`), no PDF (`pdf_espelho.py:174`, `:472`, `:609`, `:716`, `:960`). | Diverge da 164050 justamente nos consumidores posteriores e formato. **Documento mais novo interrompe este caminho:** `BACKLOG.md:1833` diz “orfas_instantes sai” e obra parada; `:1837` diz c20 encerrada, voltar à base 164050 e construir após acessor. `:2089` proíbe formato e leitor no mesmo pouso. Conflito de desenho/ordem LEU; aptidão de merge [não verificado]. |
| `lastro-folga`, c2a–c2c | Guarda `Ausencia.save`: recusa introduzir tipo fora do catálogo vivo; cache por schema; portas traduzem recusa; `update_fields` sem tipo não pergunta. | `Ausencia.save` começa por carimbar decisão e termina em `super().save`, sem guarda de tipo (`ponto/models.py:868`, `:908`). Cache do catálogo é único no processo (`catalogo/ausencias.py:257`). | Complementa guarda de faixa, mas não a substitui: “tipo no catálogo” e “tipo tem faixa” são perguntas distintas; guarda de faixa no cadastro também não existe (`services/ausencia.py:1302`). Conflito textual com FAIXA no catálogo/cache e portas de ausência; precisa composição, não sobrepor versões (INFERIU). Diff integral dessa raia não foi fornecido; merge exato [não verificado]. |

**Censo dos sítios dos diffs FAIXA que ainda estão antes da mudança.** Todos os caminhos abaixo têm prefixo `app/`; são LEU.

| Sítio HEAD | Antes ainda observado / mudança correspondente nos anexos |
|---|---|
| `ponto/precedencia.py:52`, `:76`, `:259`, `:273`, `:381` | Mapa/default atestado; sem canal de recusas; universo exclui rejeitada, sem exceção `troca_de_plantao`; acumula faixa sem recusa. |
| `ponto/catalogo/ausencias.py:240`, `:249`, `:257`, `:267`, `:293` | Só ativos, cache único, efeito de existente usa o mesmo catálogo ativo. |
| `ponto/services/ausencia.py:1302` | Cadastro copia campos/salva sem recusa de código/sem faixa. |
| `ponto/views_tipos_ausencia.py:82`, `:86`, `:94` | Chama cadastro sem tratar `AusenciaInvalida`. |
| `ponto/services/vigia_ausencia.py:154`; `pautas/semanal.py:55` | Lista e rótulos sem `tipos_ativos_sem_faixa`. |
| `escala/utils.py:1156`, `:1202`, `:1220` | Overlay não chama A2, rotula todo não-falta como `ausencia`; não transporta recusa. |
| `ponto/services/cartorio.py:730`, `:766`, `:772`, `:1244` | Ata transporta parcial, não os três campos `ausencia_tipo/id/label`; cobertura inteira usa rótulo ausencia/falta. |
| `escala/services/leitor_celula.py:429`, `:433` | Transporta parcial, não campos de cobertura. |
| `ponto/services/dia_pago.py:387`, `:395` | Universo trabalho/ausencia; abono condicionado ao rótulo. |
| `folha/export.py:1095` | Prontidão ainda deriva seus mapas inline; é o único arquivo cujo blob inteiro difere da base dos anexos. |
| `ponto/services/fechamento.py:1046`, `:1081`, `:1087`, `:1303` | Universo pelo rótulo e exceções genéricas; nenhuma propagação específica de `TipoSemFaixa`. |
| `ponto/management/commands/processar_cartorio.py:65`; `cartorio.py:1954` | Sem isolamento/contador por colaborador recusado. |
| `ponto/status_dia.py:221`, `:298`, `:301`, `:307` | Só fatos, sem recusas. |
| `colaboradores/services/calendario.py:499`, `:606`, `:661`, `:715` | Só fatos; chama `do_dia` diretamente. |
| `escala/services/semana_do_posto.py:151`, `:168`, `:178` | Só explicado/buraco; exceção genérica vira trabalho. |
| `ferias/situacao.py:72`, `:102`, `:246`, `:320` | Sem canal de aviso por tipo sem faixa. |
| `ponto/services/dia_decidido.py:371` | Reconhece somente `tipo == 'folga'`, sem `folga_da_escala`. |
| `ponto/services/dia_do_colab.py:15` | Consulta A2 avulsa, sem representação da recusa. |
| `ponto/services/espelho.py:297`, `:489`, `:644`, `:648` | Sem recusa do builder/da A2; sem publicação de instantes das órfãs. |
| `relatorios/pdf_espelho.py:475`, `:543`, `:598` | Faz duas perguntas à grade antes de consultar a tela. |
| `escala/utils.py:1650`, `:1675`, `:2005`, `:2039` | Dry-run e reconciliação de órfã continuam identificando/removendo por HH:MM. |
| `escala/utils.py:1084` | Adaptador perde as órfãs quando não há marcos. |

**Pendências que os anexos não resolvem, comprovadas no HEAD.**

- Dependência da grade da janela continua registrada como aberta, não curada: `BACKLOG.md:1066`. O rescaldo escolhe janela unitária, mas isso não prova invariância do construtor (INFERIU; `cartorio.py:1156`, `:1175`).
- Cartório tem outro casamento próprio: soma horas de colunas e órfãs (`cartorio.py:627`), quando falta coluna casa por proximidade linear ≤30 min (`:666`) e acende (`:672`); ata continua gravando órfãs (`:745`). A própria documentação chama isso de segundo juiz (`BACKLOG.md:2091`) e o comentário descreve a invariante disjunta/teste (`cartorio.py:682`, `:702`). Nenhuma das cópias fornecidas remove esse ramo (LEU anexo; INFERIU). [nao verificado]
- `como_acendeu` ainda substitui `luz` faltante pela hora prevista (`lastro.py:340`); backlog afirma que não procede por ausência de escritor produtivo (`BACKLOG.md:1787`), mas uma sonda de produção atual não está disponível: [não verificado].
- As provas c3–c19 são listadas nos anexos, mas os diffs de testes não foram fornecidos para as raias FAIXA e não estão na árvore; que passam/mordem nesta cópia é [não verificado]. [nao verificado]
- Acessor tipado está apenas no plano/fila (`BACKLOG.md:1833`, `:2088`); arquivo `escala/services/ata.py` ausente nesta cópia: existência de implementação pousada [não verificado].

**Perguntas de lei: separar lacuna de regra já respondida.**

1. **Efeito de tipo desconhecido é lacuna candidata, 4 sítios.** Fecha suprimindo (`fechamento.py:698`–`:701`), coletor não o inclui em nenhum efeito (`efeito_ausencia.py:57`–`:62`), visual chama ABONADO (`dia_decidido.py:468`–`:473`), precedência chama atestado (`precedencia.py:76`). A divergência está registrada em `BACKLOG.md:1812`; regra única explícita para `efeito=None` não localizada [não verificado]. Não equiparar faixa de precedência a efeito financeiro: as perguntas são diferentes (`precedencia.py:45`; `dia_pago.py:372`).
2. **Não contar como nova pergunta** raio versus juiz do par: a ordem de substituição está expressa e a opção raio foi superada (`BACKLOG.md:1756`).
3. **Não contar como nova pergunta** cluster morrer por idade: já decidiu L-125 (`LEIS.md:146`; `CORTES.md:430`).
4. **Não contar como conflito vigente** reconstruir passado versus FOTO: L-124 declara a revogação, preserva fato novo e exportada (`LEIS.md:145`; `CORTES.md:419`). O redesenho fixa ausência de bump na cópia, mas prova de que a nova regra atua só no horizonte é [não verificado].
5. **Não inventar decisão** sobre quantas das demais divergências são “perguntas de lei”: proximidade órfã/acesa é violação potencial de juiz único/invariante já escrita (`cartorio.py:691`, `:702`; `LEIS.md:60`), não demonstra por si duas leis conflitantes.

## 13/11 17:57 · sessao e38e884bd20f · - · - · origem=Ronald (tmux)

Q2 — leitura estática, sem executar nem escrever.

**LEU:** a isenção C7 item 8 permanece no predicado default: respostas pendentes de dia trancado são excluídas por `not dia_da_pergunta_trancado(q)`; `ignorar_tranca=True` desativa essa exclusão. O delta muda somente os dois leitores CSF para `True`. [app/chamados/models.py:1889](app/chamados/models.py:1889), [app/chamados/models.py:1902](app/chamados/models.py:1902), [app/ponto/supra_juiz.py:573](app/ponto/supra_juiz.py:573), [app/ponto/supra_juiz.py:574](app/ponto/supra_juiz.py:574).

**LEU — leitores diretos e efeito:**

| Leitor | Regime atual | Efeito do delta |
|---|---|---|
| `DisputaSupervisao.pode_fechar` | Default, preserva C7; exige também ausência de fio vivo | Nenhuma mudança direta. [app/chamados/models.py:1909](app/chamados/models.py:1909) |
| `DisputaSupervisao.fechar` | Default na recusa de resposta sem veredito | Nenhuma mudança direta; fechamento da disputa pode ocorrer em dia trancado. [app/chamados/models.py:1942](app/chamados/models.py:1942) |
| Desligamento | Default antes de fechar disputa | Nenhuma mudança direta. [app/colaboradores/services/desligamento.py:141](app/colaboradores/services/desligamento.py:141) |
| Ficha do colaborador | Default, intersectado com perguntas do app | Nenhuma mudança direta. [app/colaboradores/services/ficha.py:169](app/colaboradores/services/ficha.py:169) |
| Gaveta “respondido_a_decidir” | Default | Nenhuma mudança direta. [app/chamados/services/fila_por_causa.py:226](app/chamados/services/fila_por_causa.py:226) |
| Reconciliador de órfãs | Default antes de `pode_fechar/fechar` | Nenhuma mudança direta. [app/chamados/reconciliador.py:1164](app/chamados/reconciliador.py:1164), [app/chamados/reconciliador.py:1171](app/chamados/reconciliador.py:1171) |
| Comando `validar_classe_a` | Default, a partir da gaveta | Nenhuma mudança direta. [app/chamados/management/commands/validar_classe_a.py:44](app/chamados/management/commands/validar_classe_a.py:44), [app/chamados/management/commands/validar_classe_a.py:54](app/chamados/management/commands/validar_classe_a.py:54) |
| Medição da tranca/Pauta DP | `ignorar_tranca=True` | Já ignorava a isenção; nenhuma mudança direta. [app/chamados/services/competencia_trancada.py:126](app/chamados/services/competencia_trancada.py:126) |
| Retratação `irmas_respondidas_sem_consumo` | `ignorar_tranca=True` | Já ignorava a isenção; nenhuma mudança direta. [app/chamados/juizes.py:2035](app/chamados/juizes.py:2035) |
| `perguntas_que_seguram_csf` | `True` nos universos pai e destino | Passa a segurar também resposta sem veredito cujo dia foi calado pela tranca. [app/ponto/supra_juiz.py:573](app/ponto/supra_juiz.py:573), [app/ponto/supra_juiz.py:574](app/ponto/supra_juiz.py:574) |

**LEU:** o C6 também preserva C7: antes de considerar resposta, `dia_da_pergunta_trancado` torna a pergunta morta. O juiz real exige dia de turno em competência trancada e nenhum toque posterior do admin; conversa não tem competência de fato. [app/chamados/juizes.py:1403](app/chamados/juizes.py:1403), [app/chamados/juizes.py:974](app/chamados/juizes.py:974), [app/chamados/juizes.py:334](app/chamados/juizes.py:334), [app/chamados/juizes.py:1078](app/chamados/juizes.py:1078).

**INFERIU:** a disputa pode fechar normalmente por C7 enquanto seu pai permanece vivo pela guarda CSF alterada: o signal chama `reconciliar`, que passa pela guarda da conversa; essa guarda usa a lista CSF inteira. Portanto há mudança indireta no resultado da Defesa 1, embora `fechar/pode_fechar` não mudem. [app/chamados/signals.py:43](app/chamados/signals.py:43), [app/chamados/models.py:686](app/chamados/models.py:686), [app/chamados/models.py:719](app/chamados/models.py:719), [app/ponto/supra_juiz.py:573](app/ponto/supra_juiz.py:573).

**LEU — saída existente:** `competencia_trancada.medir` seleciona chamados vivos cuja **competência do chamado** coincide com a trancada, exclui resgatados/exceções e retém os que tenham pergunta viva; os demais entram em `r['chamados']`. `encerrar` chama `encerrar_por_competencia_trancada`, cuja guarda é `_gate_pergunta_viva`, sem `_gate_conversa_do_pai`; grava `via=competencia_trancada`. Assim a nova recusa CSF não barra essa via. [app/chamados/services/competencia_trancada.py:101](app/chamados/services/competencia_trancada.py:101), [app/chamados/services/competencia_trancada.py:109](app/chamados/services/competencia_trancada.py:109), [app/chamados/services/competencia_trancada.py:160](app/chamados/services/competencia_trancada.py:160), [app/chamados/services/competencia_trancada.py:209](app/chamados/services/competencia_trancada.py:209), [app/chamados/models.py:1684](app/chamados/models.py:1684), [app/chamados/models.py:1686](app/chamados/models.py:1686).

**LEU — limite crucial:** essa saída não é cron automático para acervo já trancado: dispara somente no **ato** de criação/retranca; `ato = criado or reaberto_em is not None`. O passivo depende do comando `encerrar_por_competencia_trancada --apply`; o registro de crons declara expressamente que ele não é cron. [app/ponto/services/fechamento.py:1411](app/ponto/services/fechamento.py:1411), [app/ponto/services/fechamento.py:1420](app/ponto/services/fechamento.py:1420), [app/config/crons.py:1276](app/config/crons.py:1276), [app/chamados/management/commands/encerrar_por_competencia_trancada.py:29](app/chamados/management/commands/encerrar_por_competencia_trancada.py:29).

**LEU/INFERIU:** Pauta DP **não tira chamado da fila por si**: recebe ids e escreve texto pedindo decisão no calendário; o fechamento foi feito antes pela via `competencia_trancada`. Há inclusive retorno anterior à criação da Pauta se `r['acoes'] == 0`. Portanto “vai para Pauta DP” não prova saída automática de um pai retido agora em tranca antiga. [app/chamados/services/competencia_trancada.py:192](app/chamados/services/competencia_trancada.py:192), [app/chamados/services/competencia_trancada.py:232](app/chamados/services/competencia_trancada.py:232), [app/chamados/services/competencia_trancada.py:248](app/chamados/services/competencia_trancada.py:248), [app/chamados/services/competencia_trancada.py:256](app/chamados/services/competencia_trancada.py:256).

**INFERIU:** não há beco absoluto demonstrado — existe via de encerramento da tranca —, mas não há garantia de progresso automático sob a hipótese “tranca já viva, admin nunca toca”. A nova guarda não dispara essa limpeza; se o chamado estiver fora da competência selecionada, sem dia, resgatado ou em módulo excluído, nem essa passagem o seleciona. A competência do chamado é julgada separadamente e pode divergir da competência de uma pergunta. [app/ponto/supra_juiz.py:573](app/ponto/supra_juiz.py:573), [app/chamados/services/competencia_trancada.py:105](app/chamados/services/competencia_trancada.py:105), [app/chamados/services/competencia_trancada.py:113](app/chamados/services/competencia_trancada.py:113), [app/chamados/services/competencia_trancada.py:161](app/chamados/services/competencia_trancada.py:161), [app/chamados/juizes.py:2352](app/chamados/juizes.py:2352). Chamado específico da frota nessa situação: **[não verificado]**.

## 13/11 17:57 · sessao d93793f5cd28 · - · - · origem=Ronald (tmux)

**5 — LEU, mapa de escrita/leitura.** O predicado usado pelos dois caminhos lê `validada_em`, `materializada_em`, `respondida_em`, `resposta_colab` e, se `ignorar_tranca=False`, a tranca; o delta muda somente a opção de leitura da tranca, sem escrever esses campos: `app/chamados/models.py:1889-1902`, `app/ponto/supra_juiz.py:562-577`.

| Campo | Quem escreve | Quem lê / significado |
|---|---|---|
| `resposta_colab` | Resposta humana em `responder_pergunta_disputa_core`, junto de `respondida_em`: `app/chamados/services/disputa_emissao.py:1462-1469`; resposta automática sem sobrescrever texto existente: `:1647-1656`; reabertura limpa: `:1593`. | Juiz O168 aceita texto não vazio como fala mesmo sem carimbo: `app/chamados/models.py:1898-1901`; C6 exige carimbo **e** texto na sua cláusula da resposta: `app/chamados/juizes.py:1417`; plantio usa o conteúdo como hora: `app/chamados/services/materializacao.py:412-419`. |
| `respondida_em` | Resposta humana recebe `now()`: `app/chamados/services/disputa_emissao.py:1462-1468`; automática recebe instante fornecido: `:1652-1653`; cura de limbo recebe o instante testemunhado pela trilha: `:1661-1678`; porta `carimbar(resposta=...)` preenche nulo somente com texto: `app/chamados/models.py:3186-3189`; reabertura limpa: `app/chamados/services/disputa_emissao.py:1593`; ressuscitação limpa em lote: `app/chamados/models.py:1613`. | O168 lê somente nulidade: `app/chamados/models.py:1900`; validar recusa sem ele: `app/chamados/services/validacao.py:416-418`; materialização seleciona apenas não nulo: `app/chamados/services/materializacao.py:1375-1378`; estado da pergunta o usa para distinguir pendente/respondida: `app/chamados/models.py:3097-3104`. |
| `validada_em` | Ato de validar em `_carimbar_validacao`, `timezone.now()` e linha da sequência com mesmo instante: `app/chamados/services/validacao.py:150-164`; nova resposta desfaz validação antiga: `app/chamados/services/disputa_emissao.py:1470-1482`; devolução por recusa limpa: `app/chamados/services/validacao.py:218-228`; reabertura limpa: `app/chamados/services/disputa_emissao.py:1593`; porta de simetria `carimbar(resposta='validar')` preenche nulo: `app/chamados/models.py:3186-3193`; ressuscitação limpa: `:1613`. | O168 considera qualquer não nulo julgado: `app/chamados/models.py:1899`; C6 encerra antes dos demais degraus: `app/chamados/juizes.py:1400-1402`; validar recusa repetição: `app/chamados/services/validacao.py:419-421`; materializador exige não nulo: `app/chamados/services/materializacao.py:1375-1378`. |
| `materializada_em` | Atribuição central em `_escrever_carimbo`: `app/chamados/models.py:3153-3157`; `carimbar` usa instante fornecido ou `now()` e consulta C6 antes de salvar: `:3180-3208`; identidade carimba sem consultar C6: `:3224-3246`; descarimbar avulso e lote limpam junto de autor/via: `:3259-3268`, `:3307-3322`. | O168 exclui qualquer pergunta materializada: `app/chamados/models.py:1899`; C6 lê carimbo e via: `app/chamados/juizes.py:1385-1395`, `:1412-1413`; fila de plantio exige nulo: `app/chamados/services/materializacao.py:1375-1378`. |
| Tranca (`PeriodoFechado`, `reaberto_em`, `fechado_em`) | Nascimento/retranca em `trancar_periodo`: `app/ponto/services/fechamento.py:1400-1419`; `fechado_em` nasce com `auto_now_add`: `app/ponto/models.py:926`; reabertura preenche `reaberto_em` e desativa: `app/ponto/services/fechamento.py:1432-1441`. | `periodo_trancado`/`competencia_trancada` leem existência com `reaberto_em IS NULL`; `ativo` é documental: `app/ponto/services/fechamento.py:1464-1480`. Pergunta trancada exige também ausência de toque humano posterior: `app/chamados/juizes.py:1062-1079`, `:1237-1251`. O168 atual ignora essa leitura nos dois universos CSF: `app/ponto/supra_juiz.py:575-576`; retratação também: `app/chamados/juizes.py:2033-2035`. |

**LEU — toque que muda o resultado da tranca:** o leitor distingue cobrança manual, resolução do dia e desarquivamento; resolução exige assinatura `via='admin'` ou chave `acoes`, excluindo lavra do cartório herdando usuário humano: `app/chamados/juizes.py:1083-1093`, `:1245-1251`, `:1292-1298`. Escritas localizadas: cobrança manual `app/chamados/services/cobrar_dia.py:39`; desarquivar chama `arquivo.levantar(...acao='desarquivar_chamado')` em `app/chamados/views_arquivar.py:77`; constante da via humana `app/ponto/services/veredito_celula.py:76`.

**LEU — dia do turno versus dia civil:** carimbos são instantes dos atos, não datas do turno, pelas atribuições `now()` acima. `data_evento` da conversa é dia civil do pedido (`localdate()`): `app/chamados/models.py:2850-2858`; o contrato distingue explicitamente esse dia do dia do turno da pergunta factual: `:2868-2870`. A tranca usa `_dia_de_turno_da_pergunta`, que devolve `None` para conversa e `data_evento` para fato: `app/chamados/juizes.py:334-338`, `:989-992`; calcula competência por `janela_atual(d, empresa)`, nunca pelo mês civil cru: `:1062-1066`.

**INFERIU — não há significado uniforme em todos os leitores:** o juiz O168 aceita **carimbo OU texto** (`app/chamados/models.py:1900-1901`), validar exige carimbo (`app/chamados/services/validacao.py:416-418`) e C6 exige carimbo **E texto** na cláusula de resposta (`app/chamados/juizes.py:1417`). Logo texto sem `respondida_em` pode segurar CSF/retratação e ser recusado como `sem_resposta` na validação; é divergência herdada, não alteração deste delta (mesmas referências). A cura existente para esse meio estado exige instante testemunhado pela trilha: `app/chamados/services/disputa_emissao.py:1661-1678`; disponibilidade dessa trilha para casos de produção: **[não verificado]**.

**INFERIU — `materializada_em` também tem vocabulário excessivo:** o help text promete “batida criada + chamado fechado” (`app/chamados/models.py:3449-3451`), mas a porta de identidade atende admin/dedup/colisão e grava o carimbo sem plantio obrigatório (`:3224-3246`). Portanto carimbo não demonstra dinheiro nem batida nova; operacionalmente O168 o trata como processamento suficiente para excluir a pergunta (`:1899`).

## 13/11 17:57 · sessao cb5af247cf08 · - · - · origem=Ronald (tmux)

LEU: a matriz e as provas foram enviadas ao agente principal, com estes pontos críticos:

- O chamado usa `resolvido_em`, não `fechado_em`; os campos estão em `app/chamados/models.py:52`, `:54` e `:60`.
- A escrita persistente de transição passa por `_gravar_estado`: `app/chamados/models.py:201`, `:228`, `:245`, `:249`, `:296`. O nascimento escreve por `abrir`/`criar`: `:939`, `:972`, `:1096`.
- `_vivas_entre` altera `status_local` somente em memória e restaura no `finally`: `app/chamados/models.py:356`, `:371`.
- O coletor existente mais próximo da pergunta única de lastro é `premissa_regularizada`: `app/chamados/models.py:1253`; a decisão operacional compartilhada por `reconciliar` e `reconciliaria` é `_decisao_do_fechamento`: `:664`.
- A trilha de estado é chamada antes do `save`, no mesmo atomic: `app/chamados/models.py:280`, `:292`, `:296`; entretanto, `evento` é best-effort e pode devolver `None`: `app/core/observ.py:94`, `:105`, `:133`, `:139`. INFERIU: o HEAD não garante incondicionalmente “uma trilha por estado persistido”.
- As recusas repetidas de reabertura por premissa regularizada/admin sem fato novo gravam evento a cada chamada: `app/chamados/models.py:1395`, `:1401`. O fechamento possui deduplicação específica de recusas: `:397`.
- Sem dia do fato, o gate do fechamento permite encerrar e registra sua ignorância: `app/chamados/models.py:733`; o coletor `premissa_regularizada` responde `False`: `:1272`.
- INFERIU: há alcance de lei a esclarecer entre reabertura pelo gate `reabrir`, que exige fato novo para fechamento de admin (`app/chamados/models.py:1343`), e a transição humana, cuja matriz permite `resolvido → em_analise` e override de admin TI (`app/chamados/catalogo/motor.py:102`, `:150`), aplicada sem aquela guarda (`app/chamados/models.py:199`).
- Arquivar por idade é véu, preservando estado: `app/chamados/services/arquivo.py:141`, `:154`, `:166`. Arquivar pela tranca encerra com via própria: `app/chamados/models.py:1623`, `:1640`.
- `superado` permanece no catálogo, mas sua porta foi retirada por ausência de chamador: `app/chamados/models.py:795`; não deve aparecer como transição executável corrente.

Não executei Django nem testes; as provas reportadas são testes lidos, com referências enviadas ao principal. [não verificado: execução] [nao verificado]

## 13/11 17:57 · sessao 95147ecea9fa · - · - · origem=Ronald (tmux)

**1 — LEU:** O predicado ficou igual nos dois caminhos: CSF e retratação chamam `sem_veredito_entre(..., ignorar_tranca=True)`; `pode_executar_csf` nega a lista resultante. (`app/ponto/supra_juiz.py:573–588`; `app/chamados/juizes.py:2033–2035`.)

**1 — INFERIU:** O BLOQ “CSF libera / retratação segura” foi eliminado para a mesma pergunta nas mesmas condições, mas os universos continuam distintos:

- CSF = perguntas de todas as disputas do pai **∪** perguntas com `chamado_destino=ch`. (`app/ponto/supra_juiz.py:558`, `:573–574`.)
- Retratação = perguntas com `chamado_destino=ch` **∪** perguntas do pai cujo destino não está VIVO. (`app/chamados/juizes.py:2033–2035`.)
- Portanto a retratação é subconjunto do CSF. Ainda existe **CSF segura / retratação libera**: resposta pendente do pai creditada a outro destino vivo. Pergunta creditada ao chamado mas fora do pai entra em ambos pelo destino. (`app/ponto/supra_juiz.py:558`, `:573–574`; `app/chamados/juizes.py:2033–2035`.)

**3 — LEU:** `_perguntas_do_pai` não usa `select_related`. Entretanto o delta passa `ignorar_tranca=True` nos dois universos; o `or` evita executar `dia_da_pergunta_trancado`, e só se leem campos da própria pergunta. O caminho que acessaria disputa/colaborador está em `dia_da_pergunta_trancado`. (`app/ponto/supra_juiz.py:558`, `:573–574`; `app/chamados/models.py:1898–1902`; `app/chamados/juizes.py:991–992`.)

**3 — INFERIU:** Não sobra query por pergunta nesse gate atual. A nota utiliza a lista `_pend` já calculada e seus PKs, sem recarregar pergunta por pergunta. (`app/chamados/models.py:719`, `:735–739`.)

**3 — LEU:** O novo selo deixa todas as N perguntas pendentes e compara queries com 1 e 3 disputas. Ele chama `registrar=False`, portanto não mede a escrita da nota. (`app/chamados/tests/test_o168_custo_da_guarda_constante.py:47`, `:56–57`, `:69–75`.) Execução e contagens reais: **[não verificado]**.

**4 — INFERIU:** Os dois mutantes são pegos pelo **mesmo `assertEqual` da linha 307**, em iterações diferentes:

- Retirar o kwarg do **destino** (`supra_juiz.py:574`): falha primeiro com `quem='destino'`.
- Retirar o kwarg do **pai** (`supra_juiz.py:573`): destino passa; falha com `quem='pai'`.

A fixture tem pai e destino distintos, uma pergunta ligada aos dois, e tranca simulada como verdadeira; o predicado sem kwarg exclui a pergunta. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:44–57`, `:300–309`; `app/chamados/models.py:1902`.)

**4 — LEU:** O `assertFalse(pode_executar_csf(...))` fica na linha 310; o mutante já abortaria no `assertEqual` anterior. O RED verifica `irmas_respondidas_sem_consumo(destino)`, mas **não verifica `irmas_respondidas_sem_consumo(pai)`**, portanto não demonstra igualdade dos universos. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:304–310`.) Execução dos mutantes: **[não verificado]**.

## 13/11 17:56 · sessao a5c16e32d31f · BUG-PAR · verificador · origem=Code

**BLOQUEIA.** Há generalização indevida de COLISÃO para PARIDADE, um caminho que pode regravar competência 10 exportada e uma recusa terminal ainda apresentada como “Tudo validado e gravado”. (`app/chamados/services/materializacao.py:1238`; `app/chamados/services/validacao.py:555`; `app/chamados/services/desenlace.py:72`)

Suíte, execução dos REDs, mutantes e ocorrência dos contraexemplos na produção: **[não verificado]**. [nao verificado]

**1 — BLOQUEIA: a primeira cura fecha o caso da irmã, mas altera também a colisão alheia.**

O ramo pergunta somente se `_conflito` traduz como COLISAO. Não identifica a batida ofensora. Entretanto, o chokepoint procura **qualquer** batida oposta na janela e devolve texto sem o identificador dela. Logo, “colisão pode ser com ela” não prova que o rollback removeu a causa. (`app/chamados/services/materializacao.py:1237`; `app/ponto/registro_batida.py:139`; `app/ponto/registro_batida.py:146`)

Contraexemplo sintético:

- Chão: `E 18:30`, **E alheia 01:08**, `S 06:30`; respostas a plantar: `S 01:10` e `E 02:00`.
- Chamado cobra `S 01:10`, enquanto o DNA vigente tem o marco S às `01:00`. Essa diferença é representável: o plantio prefere o marco cobrado no chamado; a exceção de colisão consulta os marcos vigentes e tolera cinco minutos. (`app/chamados/services/materializacao.py:488`; `app/chamados/services/materializacao.py:649`; `app/ponto/registro_batida.py:35`)
- A projeção aceita a vizinhança das duas metades; a irmã `E 02:00` grava. A tentativa de `S 01:10` colide com a **E alheia 01:08**, a dois minutos, fora da exceção do marco próprio. O rollback remove a irmã, mas preserva a E alheia. (`app/chamados/services/materializacao.py:227`; `app/chamados/services/materializacao.py:1442`; `app/ponto/registro_batida.py:145`)
- Mesmo assim, a cura grava motivo PARIDADE e deixa `materializada_em` nulo; o contrato existente manda COLISAO para `VIA_FATO_OCUPADO`. Na validação humana, a devolução também limpa `validada_em`. (`app/chamados/services/materializacao.py:1240`; `app/chamados/services/materializacao.py:1402`; `app/chamados/services/validacao.py:192`; `app/chamados/services/validacao.py:201`)

Portanto, para a colisão alheia persistente, deveria continuar valendo `fato_ocupado`. Com os mesmos insumos, sucessivas tentativas repetem a recusa e a devolução ao admin. Isso é um ciclo operacional de revalidação; não há laço computacional infinito demonstrado. (`app/chamados/services/validacao.py:570`; `app/chamados/services/validacao.py:575`)

**OK quanto ao catálogo e ao corte:** a frase nova casa a PARIDADE correta. A única regra anterior também retorna PARIDADE; COMPETENCIA e COLISAO vêm depois. A prioridade é da regra, pois o catálogo usa primeira correspondência com `any()`, independentemente da posição da expressão no texto. (`app/chamados/catalogo/recusa.py:29`; `app/chamados/catalogo/recusa.py:34`; `app/chamados/catalogo/recusa.py:89`)

Para `Pergunta #51530`, timestamp sem microssegundos e origem `disputa_s84_retro`, a composição nova tem **235 caracteres**. Se uma composição maior ultrapassar 240, o prefixo classificatório continua preservado; detalhes finais podem ser cortados. (`app/chamados/services/materializacao.py:1240`; `app/ponto/registro_batida.py:147`; `app/chamados/services/materializacao.py:1399`)

**2 — OK: os cinco chamadores querem o universo apurável.**

| Chamador | O que julga |
|---|---|
| `app/chamados/services/materializacao.py:215` | Se a metade isolada exige o par. |
| `app/chamados/services/materializacao.py:228` | Paridade com a irmã projetada. |
| `app/chamados/services/materializacao.py:712` | Antevisão do efeito do plantio na tela. |
| `app/chamados/services/materializacao.py:1150` | Guarda efetiva do plantio. |
| `app/chamados/services/materializacao.py:1161` | Rechecagem depois da irmã gravada. |

Nenhum desses usos apresenta o espelho cru. A autoridade distingue explicitamente apuração, que exclui retratadas, de exibição de marcação pela Portaria 671. A cura consulta essa autoridade nos dois lados. (`app/ponto/turnos.py:2856`; `app/ponto/turnos.py:2876`; `app/chamados/services/materializacao.py:110`; `app/chamados/services/materializacao.py:113`)

`passivo_validado.py:497` descreve **outra guarda, adicional**: comparar a geometria do dia antes/depois pelo pareador. Ela continua necessária: alternância local pode passar enquanto um plantio avulso transforma `E* S` em `E* S E`. Não há divergência introduzida pela troca de universo. (`app/chamados/services/passivo_validado.py:492`; `app/chamados/services/passivo_validado.py:527`; `app/chamados/services/passivo_validado.py:557`; `app/chamados/services/materializacao.py:454`)

Não confundir com `_batida_existente_na_janela`: esse leitor permanece deliberadamente cru para impedir replantio de marca já retratada, pergunta diferente da paridade apurável. (`app/chamados/juizes.py:243`; `app/chamados/juizes.py:806`)

**3 — BLOQUEIA no significado de `recusa_motivo`; portas de carimbo preservadas.**

Censo dos escritores e leitores decisórios:

| Campo | Quem escreve | Quem lê para decidir |
|---|---|---|
| `via_resolucao` | Porta central `_escrever_carimbo`; descarimbo em lote; troca por fato no espelho; relavra. (`app/chamados/models.py:3127`; `app/chamados/models.py:3308`; `app/chamados/models.py:3317`; `app/chamados/services/relavra_via.py:125`) | C6/identidade, lastro, estado de aplicação, universo 103 e flip automático. (`app/chamados/juizes.py:1438`; `app/chamados/juizes.py:679`; `app/chamados/services/materializacao.py:692`; `app/chamados/services/materializacao.py:276`; `app/ponto/services/flip_auto.py:129`) |
| `materializada_em` | `_escrever_carimbo` e descarimbo em lote; a porta 103 chama `descarimbar`. (`app/chamados/models.py:3125`; `app/chamados/models.py:3308`; `app/chamados/services/validacao.py:268`) | Estado, C6, aplicação, seleção do materializador, terminalidade da recusa, devolução e elegibilidade 103. (`app/chamados/models.py:3069`; `app/chamados/juizes.py:1438`; `app/chamados/services/materializacao.py:688`; `app/chamados/services/materializacao.py:1744`; `app/chamados/services/materializacao.py:1402`; `app/chamados/services/validacao.py:192`; `app/chamados/services/materializacao.py:276`) |
| `validada_em` | Validação canônica; devoluções comum/103; resposta nova; reabertura individual/lote; simetria da porta de carimbo. (`app/chamados/services/validacao.py:136`; `app/chamados/services/validacao.py:201`; `app/chamados/services/validacao.py:271`; `app/chamados/services/disputa_emissao.py:1480`; `app/chamados/models.py:1508`; `app/chamados/models.py:1567`; `app/chamados/models.py:3161`) | C6, estado, impedimento de nova validação, exigência de irmã validada e fila de materialização. (`app/chamados/juizes.py:1401`; `app/chamados/models.py:3067`; `app/chamados/services/validacao.py:498`; `app/chamados/services/materializacao.py:195`; `app/chamados/services/materializacao.py:1743`) |
| `recusa_motivo` | Recusa do materializador; pendência de marco; recusa humana; contestação respondida; duplicata, usando também a porta de identidade. (`app/chamados/services/materializacao.py:1411`; `app/chamados/services/materializacao.py:1125`; `app/chamados/services/validacao.py:705`; `app/chamados/services/disputa_emissao.py:1497`; `app/chamados/services/fio.py:1399`; `app/chamados/models.py:3214`) | Classificador/terminalidade, contador de paridade/103, presença exigida pela devolução, explicador, flip por regex e alarmes de ausência de motivo. (`app/chamados/services/materializacao.py:1400`; `app/chamados/services/materializacao.py:262`; `app/chamados/services/validacao.py:194`; `app/chamados/services/explicador.py:90`; `app/ponto/services/flip_auto.py:132`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1075`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1112`) |
| `Batida.retratada_em` | Escrita física por `anotar`; retratação fornece o instante; desfazer humano fornece `None`. (`app/ponto/registro_batida.py:253`; `app/ponto/registro_batida.py:274`; `app/ponto/services/retratar_humano.py:207`) | Universo apurável, deduplicação/colisão do chokepoint, pareamento e lastro. (`app/ponto/turnos.py:2876`; `app/ponto/registro_batida.py:109`; `app/ponto/registro_batida.py:141`; `app/ponto/turnos.py:1864`; `app/chamados/juizes.py:755`) |
| Carimbo `BUG-PAR-103` | Porta trava, reconsulta elegibilidade, descarimba, limpa validação e registra DEVOLUCAO/evento antes/depois; falta de evento aborta. (`app/chamados/services/validacao.py:259`; `app/chamados/services/validacao.py:268`; `app/chamados/services/validacao.py:276`; `app/chamados/services/validacao.py:281`; `app/chamados/services/validacao.py:288`) | Rótulo consulta precisamente DEVOLUCAO cujo motivo começa com `BUG-PAR-103`; estado de aplicação incorpora esse rótulo. (`app/chamados/services/materializacao.py:285`; `app/chamados/services/materializacao.py:697`) |

O problema de dois significados é concreto: texto livre de recusa humana compartilha campo com expressões que determinam código e terminalidade. A mensagem nova muda **estado**, contador e orientação ao admin, além da redação. (`app/chamados/services/validacao.py:705`; `app/chamados/catalogo/recusa.py:89`; `app/chamados/services/materializacao.py:1402`; `app/chamados/services/materializacao.py:262`)

Outros consumidores de tradução são o passivo e as respostas de tela. Eles passam a apresentar “vizinha do MESMO tipo” para uma causa original de **tipo oposto**. (`app/chamados/services/passivo_validado.py:144`; `app/chamados/services/fio.py:741`; `app/chamados/services/acoes_disputa.py:34`; `app/chamados/services/acoes_disputa.py:174`; `app/chamados/services/validacao.py:643`; `app/chamados/catalogo/recusa.py:35`; `app/chamados/catalogo/recusa.py:75`)

O regex independente do flip exige `batida vizinha #…`; a frase nova não fornece isso. Contudo, o leitor também exige `VIA_PARIDADE_RECUSADA`, que o ramo novo não grava: não há novo flip demonstrado por essa mensagem. (`app/ponto/services/flip_auto.py:129`; `app/ponto/services/flip_auto.py:132`; `app/chamados/services/materializacao.py:1402`)

Completude de leitores indiretos/dinâmicos: **[não verificado]**.

**4 — BLOQUEIA: regressões sem proteção suficiente e complemento da Central incompleto.**

- **Segundo escritor/juiz — OK no caminho examinado.** A irmã passa por `_materializar_uma`, a batida por `registrar_batida`, e tela/validação/materialização consultam `par_do_intervalo`. A projeção é temporária e restaurada em `finally`. (`app/chamados/services/materializacao.py:178`; `app/chamados/services/materializacao.py:1219`; `app/chamados/services/materializacao.py:1440`)
- **Universo manual — OK.** Os dois lados usam `batidas_apuraveis`; não há cópia manual do filtro nessa cura. (`app/chamados/services/materializacao.py:110`; `app/chamados/services/materializacao.py:113`)
- **Relógio — NAO-BLOQUEIA adicional.** O corte 103 compara o instante do carimbo com limite fixo no fuso da casa; não depende de “agora” durante a classificação. (`app/chamados/services/materializacao.py:273`; `app/chamados/services/materializacao.py:277`)
- **RED_2c — proteção insuficiente.** Injeta COLISAO em `01:00`, com irmã às `02:00`: não demonstra colisão real com a irmã dentro da janela de até 300 segundos. Também não exige preservar COMPETENCIA_APROVADA. O mutante “toda recusa vira PARIDADE” satisfaz suas asserções; RED_2b cobra rollback, mas não cobra o código da competência. (`app/chamados/tests/test_bug_par_projetado.py:690`; `app/chamados/tests/test_bug_par_projetado.py:695`; `app/chamados/tests/test_bug_par_projetado.py:707`; `app/chamados/tests/test_bug_par_projetado.py:672`; `app/chamados/tests/test_bug_par_projetado.py:679`; `app/ponto/registro_batida.py:31`)
- **RED_2d — OK por inspeção.** Se somente o anterior for curado, o primeiro cenário falha no posterior retratado; se somente o posterior for curado, o segundo cenário falha no anterior retratado. Execução dos mutantes: **[não verificado]**. (`app/chamados/tests/test_bug_par_projetado.py:715`; `app/chamados/tests/test_bug_par_projetado.py:717`; `app/chamados/tests/test_bug_par_projetado.py:721`; `app/chamados/tests/test_bug_par_projetado.py:723`)
- **Exceção convertida em prosa — NAO-BLOQUEIA adicional, mas permanece.** `_materializar_uma` captura qualquer `Exception`, transforma em texto e entrega esse texto ao classificador de estado. Uma exceção concreta engolida na produção: **[não verificado]**. (`app/chamados/services/materializacao.py:1636`; `app/chamados/services/materializacao.py:1648`)

**BLOQUEIA adicional: a Central ainda mente em `VIA_FATO_OCUPADO`.** Contraexemplo: pergunta HORA recusada por colisão alheia, encerrada com essa via, sem batida plantada. O lastro aceita a identidade; `estado_aplicacao_pergunta` retorna `aplicada` para toda materializada exceto PARIDADE; o explicador só marca `gravou=False` para `recusada_paridade`. Assim, com a disputa quitada, o desenlace continua dizendo **“Tudo validado e gravado.”** (`app/chamados/models.py:3823`; `app/chamados/juizes.py:679`; `app/chamados/juizes.py:691`; `app/chamados/services/materializacao.py:688`; `app/chamados/services/explicador.py:101`; `app/chamados/services/desenlace.py:65`; `app/chamados/services/desenlace.py:72`)

O teste da Central cobre somente `VIA_PARIDADE_RECUSADA`; o controle seguinte presume “gravado” quando o item não traz a marca. (`app/chamados/tests/test_bug_par_projetado.py:261`; `app/chamados/tests/test_bug_par_projetado.py:274`)

**5 — BLOQUEIA: há efeito potencial em dinheiro, inclusive exportada.**

**Competência aberta:** sim, uma colisão antes terminal agora pode ficar reaplicável. Se o obstáculo alheio for posteriormente corrigido/retratado, uma nova validação pode plantar o par e alterar a hora apurada. O escritor de batida dispara recálculo por evento. Ocorrência e valor na frota: **[não verificado]**. (`app/chamados/services/materializacao.py:1238`; `app/chamados/services/materializacao.py:1402`; `app/chamados/services/validacao.py:201`; `app/ponto/registro_batida.py:160`; `app/ponto/services/fechamento.py:1809`)

**Competência 10 exportada:** há problema mesmo sem plantar batida:

1. A porta 103 devolve a pergunta para validação do admin. (`app/chamados/services/validacao.py:268`)
2. `validar_pergunta` agenda recálculo **antes** da tentativa de materialização. (`app/chamados/services/validacao.py:544`; `app/chamados/services/validacao.py:555`)
3. O chokepoint recusa a batida em competência aprovada/exportada. A devolução limpa a validação, mas a transação externa termina sem rollback. (`app/ponto/janelas.py:87`; `app/ponto/registro_batida.py:132`; `app/chamados/services/validacao.py:201`; `app/chamados/services/validacao.py:575`)
4. No commit, o callback chama o recálculo, que atualiza os campos calculados do fechamento aprovado/exportado. A guarda especial cobre somente **09/2026**. (`app/ponto/services/fechamento.py:1807`; `app/ponto/services/fechamento.py:1824`; `app/ponto/services/fechamento.py:1160`; `app/ponto/services/fechamento.py:36`)

Contraexemplo: fechamento 10 exportado com 100 horas gravadas e derivação atual de 110 horas. Validar uma pergunta devolvida pode regravar 110, embora o plantio seja recusado. Os valores são sintéticos; existência desse desvio na produção **[não verificado]**. O caminho permite a escrita, contrariando a condição específica de preservar o gravado do aval fornecido. (`app/ponto/services/fechamento.py:1162`; `app/chamados/services/validacao.py:555`)

**6 — Os NAO-BLOQ da triagem3: um sobe para BLOQUEIA.**

| Achado anterior | Veredito desta volta |
|---|---|
| Mutantes RED5/RED7 escapam | **NAO-BLOQUEIA isoladamente; sobrevivência exata [não verificado].** RED5 cobra conteúdo renderizado; RED7 cobra dois arquivos e conteúdo `[0,1]`. Isso não demonstra quais mutações foram executadas. (`app/chamados/tests/test_bug_par_projetado.py:766`; `app/chamados/tests/test_bug_par_projetado.py:838`) |
| Texto `FORA_DE_PIPELINE` | **NAO-BLOQUEIA isoladamente; divergência permanece.** Continua “exportada segue a L-113”. Alterar a frase não corrige o caminho financeiro acima. (`app/config/crons.py:774`; `app/chamados/services/validacao.py:555`) |
| Caminho de dinheiro | **BLOQUEIA**, pelos passos do item 5. DRY default não protege a validação humana posterior. O DRY retorna antes da escrita do arquivo. (`app/chamados/services/validacao.py:555`; `app/chamados/management/commands/bug_par_103.py:65`) |
| Foto de reversão antes da trava | **NAO-BLOQUEIA isoladamente, risco confirmado.** O comando fotografa/grava antes de chamar a porta; só nela ocorre a trava. A trilha efetiva antes/depois é lida sob trava. (`app/chamados/management/commands/bug_par_103.py:48`; `app/chamados/management/commands/bug_par_103.py:77`; `app/chamados/management/commands/bug_par_103.py:85`; `app/chamados/services/validacao.py:259`; `app/chamados/services/validacao.py:263`) |

Contraexemplo da reversão: A fotografa; B devolve a pergunta; A adquire a trava e retorna `não_caso`. O arquivo de A inclui uma pergunta que A não alterou. Restaurá-lo cegamente desfaria B; aplicador desse JSON e ocorrência concorrente **[não verificado]**. (`app/chamados/management/commands/bug_par_103.py:55`; `app/chamados/services/validacao.py:261`; `app/chamados/management/commands/bug_par_103.py:91`)

**7 — NAO-BLOQUEIA adicional; LEI-AKITA do commit [não verificado].** [nao verificado]

A mensagem/metadados do commit não estão disponíveis nesta cópia sem `.git`; portanto, **presença e teor da linha LEI-AKITA de `4ab30d147` [não verificado]**. Os campos obrigatórios são `origem`, `testemunha`, `RED`, `quem-mais-le`, `juizes novos` e `padrao`. (`CLAUDE.md:62`)

Contra o código, a testemunha do universo foi centralizada corretamente, e a irmã usa o escritor existente. Entretanto, uma declaração de cura completa da classe COLISAO, de RED adversarial suficiente ou de preservação da exportada não seria verdadeira pelos contraexemplos acima. (`app/chamados/services/materializacao.py:110`; `app/chamados/services/materializacao.py:1442`; `app/chamados/services/materializacao.py:1238`; `app/chamados/tests/test_bug_par_projetado.py:695`; `app/chamados/services/validacao.py:555`)

## 13/11 17:55 · sessao 4725a19810ec · - · - · origem=Ronald (tmux)

**BLOQUEIA — complemento incompleto.** Contraexemplo: pergunta de hora respondida, recusada por `COLISAO`, com carimbo `VIA_FATO_OCUPADO`; `_carimbar_recusa` produz esse estado sem criar batida (`app/chamados/services/materializacao.py:1402`, `app/chamados/services/materializacao.py:1408`).

Esse carimbo passa no lastro por identidade (`app/chamados/models.py:2531`, `app/chamados/models.py:3823`, `app/chamados/juizes.py:679`). A testemunha devolve `aplicada`, porque excepciona somente `VIA_PARIDADE_RECUSADA` (`app/chamados/services/materializacao.py:688`). O explicador consequentemente não define `gravou=False` (`app/chamados/services/explicador.py:99`), e a Central imprime **“Tudo validado e gravado.”** (`app/chamados/services/desenlace.py:65`, `app/chamados/services/desenlace.py:72`).

Reprodução em memória com os quatro corpos reais extraídos por AST confirmou: `lastro=True → aplicada → resolvida sem gravou → Tudo validado e gravado`. O `pode_fechar=True` foi alimentação sintética, seguindo o molde do teste existente (`app/chamados/tests/test_bug_par_projetado.py:256`); ocorrência em produção: **[não verificado]**.

O teste do complemento cobre exclusivamente paridade recusada (`app/chamados/tests/test_bug_par_projetado.py:261`); o outro presume que ausência da marca significa gravação (`app/chamados/tests/test_bug_par_projetado.py:272`).

_4742 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
