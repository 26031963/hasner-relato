# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 14:08** · publicado em **06/11/2026 14:10** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 14:08 · sessao df46b1c224d2 · V3-IMPACTO-COM-DONO · leitura · origem=Code

**Nos caminhos de produto examinados, o leitor continua em `motor`; a ausência de `oraculo` não altera o número exibido nem cria alerta específico.** O fechamento escreve as versões separadamente, e os leitores usam `motor` por padrão ou filtro explícito (`app/ponto/services/fechamento.py:1211`, `:1278`; `app/ponto/services/dia_pago.py:548`, `:750`, `:831`; `app/folha/porta_export.py:589`). As contagens informadas da sombra e a situação atual do banco: **[não verificado]**.

**1. Quais entradas deixam campos sem dono**

A porta considera dono a **presença da chave**, tanto em `valores` quanto em `nao_decide`; também aceita os mapas externos vazios, desde que diferentes de `None`. A união é feita sobre **todos os dias da chamada**, não individualmente por dia (`app/ponto/services/dia_pago.py:50–75`).

No fechamento, falta e os três mapas de grade nascem como `{}` e são passados à lavratura; `do_motor` não é informado. Portanto, quando `rubricas_por_dia` devolve `{}`, ficam **11 campos sem dono** (`app/ponto/services/fechamento.py:844–849`, `:1261–1278`; `app/ponto/services/dia_pago.py:139–140`, `:34–41`, `:60–75`):

```text
horas_trabalhadas
horas_noturnas
horas_extras_50
horas_extras_50_noturna
horas_extras_100
horas_extras_100_feriado
horas_extras_100_noturna
horas_folga_trabalhada
horas_atraso
horas_saida_antecipada
horas_intra_indenizada
```

O universo visitado por `rubricas_por_dia` é `v.minutos` mais os dias publicados pelo motor como bloqueados, recortado à janela. Se esse universo ficar vazio, retorna `{}` (`app/ponto/calculador/alimentacao.py:449–475`, `:533`).

| Entrada/cenário | Efeito sobre os donos |
|---|---|
| Nenhum par fechado na competência; somente turnos abertos; pares existentes apenas fora da janela | Pode resultar em `{}`: o núcleo não acrescenta minutos para par aberto, e a alimentação filtra os dias pela janela (`app/ponto/calculador/nucleo.py:139–155`; `app/ponto/calculador/alimentacao.py:467–475`). |
| Períodos sem dia no mapa, sem carimbo efetivo, ou invertidos/colapsados sem publicação como bloqueados | Esses períodos podem não fornecer pares ao núcleo; se nenhum dia restar, resulta em `{}` (`app/ponto/calculador/alimentacao.py:73–92`, `:109–123`, `:449–471`). |
| Colaborador desligado | O estado sozinho não determina rubricas vazias. O universo exclui desligado sem data ou com demissão anterior ao início da janela da empresa; quem entra ainda depende dos períodos efetivos (`app/ponto/services/fechamento.py:419–436`; `app/ponto/calculador/alimentacao.py:449–475`). |
| Afastado o mês inteiro, sem pares fechados nem dias bloqueados | Pode resultar em `{}`. Afastamento não exclui automaticamente do universo; ausências que suprimem previsto são tratadas no fechamento, mas não acrescentam dias a `rubricas_por_dia` (`app/ponto/services/fechamento.py:391–401`, `:684–692`; `app/ponto/calculador/alimentacao.py:459–475`). |
| Único dia bloqueado por HE100 | **Já está contemplado nesta árvore**: a alimentação visita o dia com zero. Há teste escrito exigindo lavrar esse zero com donos completos (`app/ponto/calculador/alimentacao.py:459–471`; `app/ponto/tests/test_he100_dia_sem_previsto.py:242–248`, `:277–287`). |
| Somente folgas trabalhadas, com escala certa respondida | **Já está contemplado**: o ramo declara também os dois splits noturnos; há teste exigindo existência do oráculo (`app/ponto/calculador/regras.py:704–739`; `app/ponto/tests/test_he100_dia_sem_previsto.py:233–240`). |
| Dia sem trabalho previsto, não intermitente, com `certa_cache=None` | A alimentação passa `escala_certa=None`; o ramo retorna `Rubricas({}, _nd, ...)`, deixando pelo menos trabalhadas, intra e HE50 sem declaração. É uma entrada possível da função, mas o fechamento mostrado inicializa e passa um cache `{}` (`app/ponto/calculador/alimentacao.py:524`; `app/ponto/calculador/regras.py:643–645`, `:664`, `:740–741`; `app/ponto/services/fechamento.py:923`, `:1257`). |

Falta de CCT, pontas de HE ou pontualidade **não significa automaticamente campo sem dono**: esses campos entram em `nao_decide`, que a porta contabiliza como declaração (`app/ponto/calculador/regras.py:604–644`; `app/ponto/services/dia_pago.py:63`).

**O que deveria aceitar ou recusar pela lei escrita:**

- **Aceitar resposta explicitamente vazia/zero, sustentada pela autoridade.** A casa distingue “perguntei e não havia” de “não perguntei”; exige zero declarado para dia sem turno pareado e prevê uma linha de ajuste mesmo quando zerada (`app/ponto/services/dia_pago.py:54–58`, `:346–356`; `app/docs/LEIS.md:129`).
- **Recusar campo omitido sem valor, causa ou proveniência declarada.** Essa é precisamente a finalidade escrita da porta; regra ausente não pode virar zero (`app/ponto/services/dia_pago.py:303–319`; `app/ponto/calculador/regras.py:9–13`).
- **`rubricas={}` sozinho não distingue essas situações.** Hoje a porta recusa ambos os casos; liberar indiscriminadamente esse dicionário esconderia também falha de alimentação. A distinção precisa vir da autoridade que fornece os dias, preservando a recusa de campos sem declaração (`app/ponto/calculador/alimentacao.py:459–466`; `app/ponto/services/dia_pago.py:310–319`; `CLAUDE.md:17–21`, `:35–36`).

A recusa acontece **antes de apagar as linhas anteriores**. Logo, pode deixar o colaborador sem primeira lavratura ou preservar um oráculo antigo; o `except` apenas registra ERROR e o fechamento incrementa `processados` (`app/ponto/services/dia_pago.py:310–319`, `:357–358`; `app/ponto/services/fechamento.py:1281–1286`).

**2. Quem lê e o que aparece sem oráculo**

Nos caminhos abaixo, **não há seleção de `versao='oraculo'`**; ausência dessa versão é invisível para o leitor indicado, pelas consultas citadas em cada linha.

| Leitor | Fonte e comportamento quando falta apenas `oraculo` |
|---|---|
| Tela do espelho | Continua mostrando linhas `motor`; o helper é chamado sem alterar a versão padrão. O resumo usa fechamento e, sem fechamento, soma da lavratura `motor` (`app/ponto/services/espelho.py:406–410`; `app/ponto/services/dia_pago.py:831–854`; `app/relatorios/cartao_pela_celula.py:323–345`, `:141–143`). |
| Calendário do colaborador | Continua mostrando rubricas `motor`, obtidas por `linhas_do_periodo` (`app/colaboradores/services/calendario.py:290`, `:655–656`; `app/ponto/services/dia_pago.py:831–854`). |
| PDF/cartão | Continua imprimindo `pago_h` e rubricas preparados pelo espelho; os totais seguem a mesma seleção entre fechamento e lavratura `motor` (`app/relatorios/pdf_espelho.py:837–839`, `:901–914`; `app/relatorios/cartao_pela_celula.py:323–345`). |
| Extrato parcial | Continua somando `motor`, pois chama `soma_do_periodo` sem versão alternativa (`app/relatorios/views.py:481–487`; `app/ponto/services/dia_pago.py:750`, `:782–785`). |
| Gestão de HE e informação ao DP na autorização por período | Continuam lendo a soma `motor` e o respectivo rótulo de disponibilidade (`app/ponto/services/gestao_he.py:256–257`, `:297–298`; `app/ponto/services/autorizacao_he_periodo.py:172–175`; `app/ponto/services/dia_pago.py:750`). |
| App/API | Continua lendo `motor`; o payload recebe horas e `dia_pago_lavrada` dessa fonte (`app/api/views.py:1454–1457`, `:1539–1558`; `app/ponto/services/dia_pago.py:831`). |
| TXT/export | As rubricas saem do `FechamentoMensal`, não da versão oráculo. A montagem percorre os fechamentos e seus eventos (`app/folha/export.py:703–724`). |
| Conferência/placar S2 e R4 | Comparam/somam `motor`. A falta apenas de oráculo não entra em `colabs_sem_lavratura`; esse contador nasce quando a soma `motor` retorna `None` (`app/folha/porta_export.py:388–393`, `:588–597`; `app/ponto/services/dia_pago.py:548–559`). O comando publica o contador, que permanece fora de `falhas` (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:67–69`, `:88–107`; `app/folha/porta_export.py:687–701`). |
| Termômetro R1/E6 | O “oráculo” aqui é o cálculo independente sobre batidas apuráveis; não é a linha `DiaPago.versao='oraculo'`. A comparação continua usando esse resultado (`app/ponto/management/commands/e6_oraculo.py:144–146`, `:304–318`; `app/core/placar_estrutural.py:115–116`). |

Se **também faltar `motor`**, há comportamentos distintos: espelho mostra “sem apuração ainda”; extrato distingue “sem lavratura” de “sem apuração ainda”; a conferência registra `colabs_sem_lavratura`; o app entrega horas do período com `dia_pago_lavrada=False` (`app/templates/ponto/espelho.html:268–271`; `app/relatorios/views.py:516–536`; `app/folha/porta_export.py:388–393`; `app/ponto/services/espelho.py:129–136`).

**3. Sonda integral — somente leitura**

O script abaixo emite um `SELECT`, com tabelas qualificadas no schema `juliani`. Conta colaboradores distintos com **qualquer linha da versão**, inclusive ajuste; filtra a competência por `mes`/`ano`, pois `data` é nula no ajuste (`app/ponto/models.py:311–323`; `app/ponto/services/dia_pago.py:355–356`). A empresa é a vinculada atualmente ao colaborador (`app/colaboradores/models.py:321`).

A janela usa `Empresa.dia_inicio_competencia` pela função existente; com corte 21, outubro é **21/09–20/10** e novembro **21/10–20/11** (`app/colaboradores/models.py:46`; `app/ponto/janelas.py:10–21`, `:36–57`). **Script não executado; resultados [não verificado].**

```python
from collections import defaultdict
from types import SimpleNamespace

from django.db import connection

from colaboradores.models import Colaborador, Empresa
from ponto.janelas import janela_fechamento
from ponto.models import DiaPago


SCHEMA = "juliani"
COMPETENCIAS = ((10, 2026), (11, 2026))

# Campos:
# app/ponto/models.py:315-323
# app/colaboradores/models.py:46,321
#
# Janela:
# app/ponto/janelas.py:10-21,36-57
#
# As tabelas são qualificadas pelo schema; o script não troca search_path.
q = connection.ops.quote_name


def tabela(modelo):
    return "%s.%s" % (q(SCHEMA), q(modelo._meta.db_table))


def coluna(modelo, campo):
    if campo == "pk":
        return q(modelo._meta.pk.column)
    return q(modelo._meta.get_field(campo).column)


sql = f"""
SELECT DISTINCT
    e.{coluna(Empresa, "pk")},
    e.{coluna(Empresa, "dia_inicio_competencia")},
    dp.{coluna(DiaPago, "mes")},
    dp.{coluna(DiaPago, "ano")},
    dp.{coluna(DiaPago, "versao")},
    dp.{coluna(DiaPago, "colaborador")}
FROM {tabela(Empresa)} AS e
LEFT JOIN {tabela(Colaborador)} AS c
    ON c.{coluna(Colaborador, "empresa")} = e.{coluna(Empresa, "pk")}
LEFT JOIN {tabela(DiaPago)} AS dp
    ON dp.{coluna(DiaPago, "colaborador")} = c.{coluna(Colaborador, "pk")}
   AND dp.{coluna(DiaPago, "ano")} = %s
   AND dp.{coluna(DiaPago, "mes")} IN (%s, %s)
   AND dp.{coluna(DiaPago, "versao")} IN (%s, %s)
ORDER BY 1, 3, 4, 5, 6
"""

# Única consulta de dados: SELECT.
with connection.cursor() as cursor:
    cursor.execute(sql, (2026, 10, 11, "motor", "oraculo"))
    linhas = cursor.fetchall()

cortes = {}
presenca = defaultdict(set)

for empresa_id, corte, mes, ano, versao, colaborador_id in linhas:
    cortes[empresa_id] = corte
    if colaborador_id is not None:
        presenca[(empresa_id, mes, ano, versao)].add(colaborador_id)

for mes, ano in COMPETENCIAS:
    for empresa_id in sorted(cortes):
        empresa = SimpleNamespace(
            pk=empresa_id,
            dia_inicio_competencia=cortes[empresa_id],
        )
        inicio, fim = janela_fechamento(mes, ano, empresa)

        motor = presenca[(empresa_id, mes, ano, "motor")]
        oraculo = presenca[(empresa_id, mes, ano, "oraculo")]
        faltantes = sorted(motor - oraculo)
        rotulos = ["col%d" % pk for pk in faltantes]

        print(
            "tenant=%s empresa=emp%d competencia=%02d/%d "
            "janela=%s..%s dia_inicio_competencia=%s "
            "n_colabs_motor=%d n_colabs_oraculo=%d "
            "n_motor_sem_oraculo=%d motor_sem_oraculo=%s"
            % (
                SCHEMA,
                empresa_id,
                mes,
                ano,
                inicio.strftime("%d/%m/%Y"),
                fim.strftime("%d/%m/%Y"),
                cortes[empresa_id],
                len(motor),
                len(oraculo),
                len(faltantes),
                "[" + ", ".join(rotulos) + "]",
            )
        )
```

## 06/11 14:08 · sessao 4a8660024d33 · - · - · origem=Ronald (tmux)

Achados para consolidação, todos por leitura estática; não rodei testes nem escrevi arquivos. [nao verificado]

1. **A publicação exagera o que “o próprio comando cobra”.** `SOMA.numero` afirma “36 linhas SOMA True”, “as HORAS também fecham” e “o PROPRIO comando a cobra”; `SOMA.fonte` repete “a soma e cobrada no proprio comando” ([app/core/placar_estrutural.py:106](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/core/placar_estrutural.py:106), :108–109). O comando da cópia apenas imprime a soma dos donos e a soma das classes, sem comparação booleana, `assert`, exceção ou falha; não imprime verificações de horas nem união ([app/ponto/management/commands/e6_oraculo.py:415](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/ponto/management/commands/e6_oraculo.py:415), :424–429). As 36 verificações constam do **anexo**, portanto são sustentadas enquanto saída fornecida; sua atribuição ao próprio comando desta cópia é incorreta. A sonda que acrescentou `UNIVERSO_PARTE`, `PARCIAL`, `PARCIAL_TOTAL`, `SOMA ... HORAS ... True`, `SOMA ... COLABS_UNIAO ... True` não foi fornecida [nao verificado]. Isso também afeta a frase de R1 em :118.

2. **Nenhuma inversão de chave em E1:** `esp_zero_e6_trabalho` soma **35/28**, conforme publicado ([app/core/espelho_verdade.py:49](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/core/espelho_verdade.py:49); anexo p1…p6, existência física [nao verificado]). A classe corresponde a `m_esp == 0`, com diferença absoluta superior a dez minutos; a chave inversa corresponde a `m_e6 == 0` e soma **19/14** ([app/ponto/management/commands/e6_oraculo.py:353](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/ponto/management/commands/e6_oraculo.py:353), :358–360). `None` no espelho não é zero: tenta `pago_h`, e, faltando ambos, retorna ausência de lavratura (:251–257).

3. **Horas publicadas são horas de diferença absoluta, não trabalho.** O acumulador acrescenta `abs(m_e6 - m_esp)` por divergência; o comando divide por sessenta ([app/ponto/management/commands/e6_oraculo.py:350](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/ponto/management/commands/e6_oraculo.py:350), :368, :423). Os números de horas do diff são compatíveis com essa unidade; não se deve interpretá-los como horas trabalhadas ([app/core/placar_estrutural.py:106](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/core/placar_estrutural.py:106), :118; soma do anexo, origem física [nao verificado]).

4. **Subconjunto precisa ficar claro em E6-batida-impar.** `198+1=199` e `137+2=139` repartem apenas os **ímpares divergentes**, não todos os 347/262 comparados; o código só incrementa `impar_no_conjunto_do_dia`/`impar_SO_POR_CORTE_do_oraculo` depois de rejeitar BATE ([app/ponto/management/commands/e6_oraculo.py:353](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/ponto/management/commands/e6_oraculo.py:353), :370–382). O mesmo vale para **31/25 FORA**: pertencem aos 199/139 divergentes (:383–387). Os valores de [app/core/espelho_verdade.py:177](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/core/espelho_verdade.py:177) estão corretos, mas o texto poderia explicitar “dos que divergem”.

5. **“Nenhum fica fora em silencio” exige delimitar o universo comparável.** O comando elimina dias sem linha, com isenção/ausência, sem lavratura, zero dos dois lados ou diferença explicada pelo intervalo declarado **antes** de classificar a paridade ([app/ponto/management/commands/e6_oraculo.py:324](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/ponto/management/commands/e6_oraculo.py:324), :327–349). A medida mostra que `dia_batida_impar = impar_que_BATE + impar_que_DIVERGE` entre comparados; não informa quantos excluídos também eram ímpares [nao verificado]. A frase universal de [app/core/espelho_verdade.py:177](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/core/espelho_verdade.py:177) não prova ausência de todos os tipos de exclusão.

6. **Causa operacional não demonstrada:** “a saída que falta e cobranca, nao cura de motor” permanece em [app/core/espelho_verdade.py:82](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/core/espelho_verdade.py:82). Contagem ímpar e faixa não demonstram essa exclusividade: o próprio comando reconhece artefato do corte como suspeita do oráculo ([app/ponto/management/commands/e6_oraculo.py:372](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/ponto/management/commands/e6_oraculo.py:372), :379–385); a docstring cita casos em que o oráculo corta jornada verdadeira e a tela está certa (:25–31). A medida fornecida inclui **1/2** divergentes ímpares só pelo corte [nao verificado quanto arquivos físicos]. Portanto o dado não identifica “só cobrança” como causa/cura.

7. **Texto velho continua em R1:** a nota ainda diz “a medicao da competencia aberta e de 09/10” e que a cadência “foi CUMPRIDA”, embora o número/prova passem a 06/11 ([app/core/placar_estrutural.py:119](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/core/placar_estrutural.py:119), em contraste com :118/:120). É história possivelmente intencional, mas está no presente e cita a medição antiga. Não localizei os antigos 95,8%, 333, 92/449,2/39 nos seis itens novos; o `194` em placar :150 pertence a R3, outra medida.

8. **Destino declarado como regra versus medido:** “CADASTRO ... BATIDA ... vao a lista do admin” em [app/core/espelho_verdade.py:173](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/core/espelho_verdade.py:173) tem respaldo como política em L-099 ([app/docs/LEIS.md:122](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/docs/LEIS.md:122)); o anexo não demonstra que todos os 32/113 e 33/78 tenham efetivamente entrada ou chamado [nao verificado]. O comando imprime o rótulo “lista do admin”, mas não realiza escrita nela ([app/ponto/management/commands/e6_oraculo.py:418](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/ponto/management/commands/e6_oraculo.py:418), :419–423).

9. **“0 erro” não foi reafirmado no novo E6-sistema-bate.** O comando tem contador específico de exceções de `espelho_do_colab`, imprime “erros no espelho”, mas o anexo não apresenta essa linha ([app/ponto/management/commands/e6_oraculo.py:303](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/ponto/management/commands/e6_oraculo.py:303), :305–307, :396). `rc_sombra=0`/ausência de `INCOMPLETO` no anexo não prova, isoladamente, zero exceções capturadas [nao verificado]. A nova prova corretamente suprimiu “0 erro” ([app/core/espelho_verdade.py:169](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/core/espelho_verdade.py:169)).

10. **União somada depende das partes serem disjuntas:** a medida permite reproduzir 144/105 pela soma dos seis `PARCIAL_TOTAL.colabs`; não permite conferir identidades ou ausência de sobreposição entre partes [nao verificado]. O comando normal mantém conjuntos por dono e calcula união real do seu próprio `div` ([app/ponto/management/commands/e6_oraculo.py:369](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/ponto/management/commands/e6_oraculo.py:369), :429). A implementação do particionamento não consta deste comando (:264–270 não têm `--parte`/`--partes`).

## 06/11 14:07 · sessao bc8feadb57e1 · - · - · origem=Ronald (tmux)

Achados procedentes:

1. **O selo novo valida uma família de nomes, não uma única medição.** `_cita_a_medicao` usa `any(fnmatch(...))`; provas com `r1_dono_...20261106.txt` e `r1_dono_...20261024.txt` passam simultaneamente, assim como a prova literal contendo o próprio glob. Além disso, `_medido_em` escolhe a maior data entre os arquivos existentes, sem conferir qual arquivo a prova cita. Logo, o selo pode ficar verde com número/prova antigos e data de outro arquivo. Procedência: execução em memória das funções reais, sem gravação; `app/core/tests/test_r1_termometro_uma_medicao.py:29`, `:45`, `:50`, `:57`; `bin/gerar_estado.py:51`, `:56`.

2. **A publicação dos seis itens elimina os números e a referência completa.** O publicador corta cada prova em 150 caracteres; todos os seis textos novos têm exatamente o mesmo prefixo de 150 caracteres, terminando em `578 na 11 = soma das partes,`. A tabela pública não mostra BATE, divergentes, ímpares, zeros ou caminho da medição nesses itens. Procedência: recorte em memória dos textos reais; `bin/gerar_estado.py:164`; `app/core/espelho_verdade.py:49`, `:82`, `:169`, `:173`, `:177`, `:179`.

3. **Existe outro publicador de números E6 fora da costura nova.** O ESTADO continua carregando `logs/e6_placar.json` e publicando `pct_bate`, `dias_comparados`, `colabs_divergentes` e `quando`, independentemente da fonte R1. O selo novo só seleciona provas começando com `TERMOMETRO R1`. Se esse JSON contém números velhos, eles continuam aparecendo; o conteúdo atual é [nao verificado]. Procedência: `bin/gerar_estado.py:145`, `:168`; `app/core/espelho_verdade.py:208`; `app/core/tests/test_r1_termometro_uma_medicao.py:37`.

Censo dos campos:

| Campo | Escritor canônico | Leitores reais |
|---|---|---|
| SOMA.numero/prova | `app/core/placar_estrutural.py:106`, `:107` | `app/core/placar_estrutural.py:634`, `:637`; `bin/gerar_estado.py:99`, `:100`; selo novo lê prova em `app/core/tests/test_r1_termometro_uma_medicao.py:50`; host varre strings em `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:65` |
| R1.numero/prova | `app/core/placar_estrutural.py:118`, `:120` | `app/core/placar_estrutural.py:634`, `:637`; número publicado em `bin/gerar_estado.py:116`; prova conferida pelo selo novo em `app/core/tests/test_r1_termometro_uma_medicao.py:45`; host em `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:65` |
| R1.fonte_do_medido | `app/core/placar_estrutural.py:111` | `bin/gerar_estado.py:47`; selo novo em `app/core/tests/test_r1_termometro_uma_medicao.py:45`, `:50`, `:54`; presença exigida em `app/core/tests/test_l114_placar_papel_e_termometro.py:98`; host varre strings em `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:65` |
| ETAPAS[…].prova | `app/core/espelho_verdade.py:49`, `:82`, `:169`, `:173`, `:177`, `:179` | `app/core/espelho_verdade.py:201`, `:203`; `bin/gerar_estado.py:164`; selo novo em `app/core/tests/test_r1_termometro_uma_medicao.py:39`, `:57` |

Leitores acessórios e selos:

- O teste L114 lê o registro R1, a separação de papéis e o vencimento, sem fixar a pasta antiga; substitui `RESULTADOS` apenas por fixture sintética e restaura depois. `app/core/tests/test_l114_placar_papel_e_termometro.py:31`, `:65`, `:96`, `:116`.
- O host de hash só acusa frases declaradas de “fora do ar” na mesma sentença de hash reconhecido e ancestral. Os textos novos não introduzem essa combinação. `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:47`, `:65`, `:72`, `:75`; textos em `app/core/placar_estrutural.py:106`, `:118`, `:120`.
- O selo de data percorre código por regex; a conferência real em memória retornou **nenhuma data cravada no placar**. Datas em caminhos `20261106` e prosa `06/11` não casam esses padrões. `app/core/datas_cravadas.py:26`, `:117`; exigência específica em `app/core/tests/test_l114_placar_papel_e_termometro.py:90`.
- `handoff_sessao.sh` lê o arquivo inteiro, mas extrai somente a frase `contratos_estruturais: N/T verdes` do R6, fora dos campos alterados. O respectivo teste também só lê R6.numero. `bin/handoff_sessao.sh:178`; `app/core/tests/test_selo_r6_frase_do_juiz.py:17`.
- A publicação passa pelo raspador e o selo de nomes/CPF examina o repositório público usando nomes do banco; não valida a pasta dos logs. Resultado real de privacidade: [nao verificado], pois exige banco e publicação. `bin/relato.sh:103`, `:105`; `bin/tests/test_publicacao_sem_nome.sh:39`, `:47`, `:60`, `:72`.
- Número sem prova continua rebaixando o placar; FEITO sem prova continua rebaixando o espelho. As novas provas são não vazias. `app/core/placar_estrutural.py:637`; `app/core/espelho_verdade.py:201`; prova nova em `app/core/placar_estrutural.py:120`, `app/core/espelho_verdade.py:177`.

A mudança para `logs/r1remede/r1_dono_*.txt` é compatível com o leitor de glob/data, desde que o arquivo datado exista no host; existência e retorno em produção são [nao verificado]. `bin/gerar_estado.py:47`, `:51`, `:52`.

## 06/11 14:07 · sessao c673e8cb9b39 · - · - · origem=Ronald (tmux)

Achado procedente: o selo novo não garante “uma medição”; garante apenas que alguma referência na prova casa com a glob. `_cita_a_medicao` usa somente `any(fnmatch.fnmatch(...))`, sem identificar o arquivo/data comum entre leitores (`app/core/tests/test_r1_termometro_uma_medicao.py:29`). A fonte permite múltiplos arquivos (`app/core/placar_estrutural.py:111`), enquanto o publicador escolhe a maior data entre **todos** os arquivos existentes nessa glob, independentemente da prova citada (`bin/gerar_estado.py:51`, `bin/gerar_estado.py:56`).

Contraexemplo reproduzido em memória: com o padrão atual, a prova `logs/r1remede/r1_dono_10_e_11_20261024.txt` passa, assim como uma referência sem data e até `logs/r1remede/r1_dono_*.txt`. Portanto, provas de 24/10 e 06/11 podem coexistir com o selo verde e carimbo escolhido pelo máximo do diretório. Origem do defeito: matcher em `app/core/tests/test_r1_termometro_uma_medicao.py:29`; ligação ausente com o arquivo efetivamente selecionado por `bin/gerar_estado.py:51`.

Outro escape reproduzido: retirando o prefixo `TERMOMETRO R1` de uma das seis provas, esse item deixa de ser fiscalizado, pois o censo usa `startswith` (`app/core/tests/test_r1_termometro_uma_medicao.py:37`). A proteção contra vacuidade exige apenas que **algum** item permaneça no conjunto (`app/core/tests/test_r1_termometro_uma_medicao.py:56`).

Validação limitada, somente leitura: carreguei por `compile/exec` os módulos puros e substituí apenas a base `django.test.SimpleTestCase` por `unittest.TestCase`, executando os quatro métodos do selo. Atual: 4 casos, 0 falhas, 0 erros. Mutante mesma glob/outra data em uma prova: 4/0/0. Mutante removendo o prefixo desse item: 4/0/0. Isso não comprova execução da suite Django/canônica [nao verificado]. [nao verificado]

O selo tem mordida real para a mudança **de pasta**: o caso adversarial rejeita `logs/sombra/...` e aceita `logs/x/r1_dono_...`, incluindo pontuação final (`app/core/tests/test_r1_termometro_uma_medicao.py:60`). Não copia valores medidos atuais nem data/caminho atual nas asserções; consulta as estruturas do código e usa um caminho fictício no adversarial (`app/core/tests/test_r1_termometro_uma_medicao.py:43`, `app/core/tests/test_r1_termometro_uma_medicao.py:62`). O literal “39 dia-colab” não é conferido pelo matcher, que só extrai caminhos (`app/core/tests/test_r1_termometro_uma_medicao.py:22`, `app/core/tests/test_r1_termometro_uma_medicao.py:64`).

Não há leitura de relógio no selo novo: suas importações e helpers usam `fnmatch`, regex e os dois módulos de dados (`app/core/tests/test_r1_termometro_uma_medicao.py:14`). O relógio continua na borda publicadora (`bin/gerar_estado.py:79`); o cálculo do vencimento recebe `hoje` e `medido_em` por parâmetro (`app/core/placar_estrutural.py:617`, `app/core/placar_estrutural.py:647`).

O RED histórico alegado no commit, com `FAILED failures=1` e `logs/r1pub/red.log`, não pode ser confirmado com o material disponível [nao verificado]. A ausência de sustentação desse histórico não elimina a mordida estática contra a pasta antiga, mas também não comprova a execução alegada (`app/core/tests/test_r1_termometro_uma_medicao.py:60`).

Pequena inconsistência documental introduzida: a docstring diz “cinco itens” antigos (`app/core/tests/test_r1_termometro_uma_medicao.py:9`), enquanto o diff fornecido troca seis provas — E1-sem-previsao, E3-turno-aberto e quatro itens E6 (`app/core/espelho_verdade.py:49`, `app/core/espelho_verdade.py:82`, `app/core/espelho_verdade.py:169`, `app/core/espelho_verdade.py:173`, `app/core/espelho_verdade.py:177`, `app/core/espelho_verdade.py:179`).

Das classes listadas, a ocorrência comprovável é **selo com mordida insuficiente para a propriedade declarada** (`app/core/tests/test_r1_termometro_uma_medicao.py:29`). O diff de produto altera dados declarados, sem introduzir nova função de julgamento/escrita ou travas (`app/core/placar_estrutural.py:106`, `app/core/placar_estrutural.py:118`, `app/core/espelho_verdade.py:49`). Pouso, ordem efetivamente executada, suite verde e leitura final não estão demonstrados [nao verificado].

## 06/11 13:57 · sessao 0b5c7db967e3 · V3-IMPACTO-COM-DONO · leitura · origem=Code

1. **Os 3191 registros não identificam nenhum `raise LacunaDeMedicao()` literal.** No anexo, `RegistroErros.dados` escreve `sonda` quando não recebe traceback; um `raise` capturado teria origem com linha. Condicionado ao anexo ser o código executado, os registros vieram destas chamadas **sem lançamento** de `falha(LacunaDeMedicao())`. A repartição integral entre elas não foi preservada. [anexo `q3_script_p1.py`, `RegistroErros.dados` e `executar`; nao verificado]

| Sítio do anexo | O que significa | Classificação |
|---|---|---|
| Varredura dos contadores `historico["T"]` | Uma ocorrência por **chave não zero**, não por unidade do contador. Mistura falhas diferentes do comando. | O registro genérico é defeito do instrumento; a causa subjacente exige identificar o contador. [nao verificado] |
| `if not c["emitido"][versao]` | Confunde chamada ausente com chamada bem sucedida sem objetos `TIPO_DIA`. | Defeito do instrumento: `lavrar` sempre pode emitir somente AJUSTE. [app/ponto/services/dia_pago.py:346] |
| `if alvo not in c["fechos"]` | O fechamento não entregou objeto para o alvo. | Pode ser fato de escopo do produto: o fechamento aplica seu próprio universo e restrições. A causa destes casos não está demonstrada. [app/ponto/services/fechamento.py:545] [app/ponto/services/fechamento.py:555] [nao verificado] |
| `if v2 is None or v3 is None` | Uma data pertence à união dos dias emitidos, mas falta em uma versão. | Depois de uma recusa conhecida, é **lacuna secundária fabricada pelo instrumento**; depois de duas lavras bem sucedidas, é diferença real dos conjuntos emitidos, a registrar separadamente. [app/ponto/services/fechamento.py:1281] [nao verificado quanto aos casos individuais] |

O defeito concreto está na captura: o anexo guarda `rubricas`, `cegos` e `colab` **depois** de `REAL_LAVRAR`. A recusa impede essa captura; o fechamento captura a exceção e continua. O instrumento então acusa oráculo vazio e falta de par para os dias do motor. A cura é capturar a entrada **antes**, registrar tentativa e sucesso separadamente e excluir uma vez pela recusa. [anexo `q3_script_p1.py`, `emissor_capturado`; nao verificado] [app/ponto/services/fechamento.py:1272] [app/ponto/services/fechamento.py:1281]

Em P1, `SEM_PAR` permite atribuir **237** lacunas de outubro e **329** de novembro ao teste diário; restam **47** e **24**, respectivamente, sem repartição recuperável entre os demais sítios sintéticos. Nada permite estender essa decomposição às outras partes. [anexos `q3_s_p1.r` e `agrega.r`; nao verificado] [nao verificado]

Portanto, **um DIA vira lacuna nesse teste pela ausência de uma das duas linhas emitidas**, independentemente de valor, batida ou divergência. A data exemplificada não prova falta de decisão da autoridade. Os universos também diferem: a porta visita dias do núcleo mais bloqueados publicados pelo motor; a lavra cria dias adicionais por furos e grade. [anexo `q3_script_p1.py`, laço de `chaves`; nao verificado] [app/ponto/calculador/alimentacao.py:467] [app/ponto/calculador/alimentacao.py:469] [app/ponto/services/dia_pago.py:280] [app/ponto/services/dia_pago.py:286] [nao verificado]

2. **Os 740 não são demonstradamente falhas de `porta_ok`.** O anexo usa a mesma classe na conferência mensal e na conferência porta/emissor; ambas são registradas como `sonda`. O `raise` de `n != len(capturadas)` teria traceback, portanto não corresponde à origem publicada, condicionado ao anexo executado. Além disso, `False` em `SOMA V3_PORTA_EMISSOR` pode resultar apenas de `and cobertura`. [anexo `q3_script_p1.py`, `acumular`/`publicar`; nao verificado] [nao verificado]

Quem escreve os lados:

- **`porta`**: `rubricas_por_dia` escreve cada dia chamando `regras.do_dia`; o fechamento entrega o mapa a `lavrar`. [app/ponto/calculador/alimentacao.py:514] [app/ponto/services/fechamento.py:1255] [app/ponto/services/fechamento.py:1272]
- **`emitido_decidido`**: `lavrar` lê os valores da porta, atribui ao campo do dia e constrói os objetos `DiaPago`. Nesse caminho, o chamador não passa `do_motor`; a atribuição é direta. [app/ponto/services/dia_pago.py:322] [app/ponto/services/dia_pago.py:328] [app/ponto/services/dia_pago.py:343] [app/ponto/services/fechamento.py:1261]

Assim, uma diferença **comprovada na mesma célula decidida** indicaria incoerência do produto, provavelmente **ESTRUTURA**, no campo identificado pela comparação. O dado atual não identifica essas células nem demonstra que essa igualdade falhou. [CLAUDE.md:176] [nao verificado]

Há também defeito instrumental em `fm_ok = bool(e2) ...`: uma lavra somente de AJUSTE, com mensal zero, produz falsamente uma conservação quebrada **por rubrica**. A ocorrência concreta desses casos não pode ser contada pelo dado. [anexo `q3_script_p1.py`, `acumular`; nao verificado] [app/ponto/services/dia_pago.py:346]

P1 mostra uma diferença mensal específica em novembro/trabalhadas: **5478,32 versus 5478,33**. O campo é `horas_trabalhadas`; o mensal tem arredondamento inicial e pode arredondar novamente ao adicionar folga sem escala. Isso oferece uma hipótese de **ESTRUTURA**, mas não prova a causa daquele centésimo. [anexo `q3_s_p1.r`; nao verificado] [app/ponto/services/fechamento.py:893] [app/ponto/services/fechamento.py:968]

3. O caminho é `diff_calculador` → fechamento → `lavrar(..., rubricas=_rub_o, versao="oraculo")`. O comando instala normalmente `_nada`; o captador do anexo reinstala a chamada real durante o fechamento. [app/ponto/management/commands/diff_calculador.py:137] [app/ponto/management/commands/diff_calculador.py:140] [app/ponto/services/fechamento.py:1261] [app/ponto/services/fechamento.py:1272] [anexo `q3_script_p1.py`; nao verificado]

**A recusa é esperada para entrada sem proveniência; “lei do ancoramento pendente” não descreve a lei vigente desta cópia.** A lei já manda ancorar por período, pela mesma `he_noturna`, e isso está implementado. [app/ponto/services/dia_pago.py:311] [app/ponto/services/dia_pago.py:315] [app/ponto/calculador/regras.py:414] [app/ponto/calculador/regras.py:825] [app/ponto/calculador/regras.py:832]

O conjunto de donos depende das declarações encontradas em `rubricas.values()`, incluindo `nao_decide`. Um mapa vazio não declara os onze campos do calculador; a própria alimentação documenta esse mecanismo. Por isso a recusa depende dos insumos de cada colaborador, não ocorre indistintamente. **Não é possível afirmar que todas as 182 recusas sejam mapa vazio**, pois os campos recusados não foram publicados. As frequências fornecidas variam de 11 a 21 por parte/competência. [app/ponto/services/dia_pago.py:61] [app/ponto/services/dia_pago.py:64] [app/ponto/calculador/alimentacao.py:459] [anexo `agrega.r`; nao verificado] [nao verificado]

O guard está correto ao recusar. Se a integração entrega um mês vazio sem declarar sua proveniência, o defeito é do protocolo de alimentação; não se resolve preenchendo zero nem suspendendo o guard. Qual entrada explica cada recusa permanece [nao verificado]. [app/ponto/services/dia_pago.py:308]

4. O mapa escritor/leitor é este; a tabela de juízes aponta cálculo para motor/CCT e janela para `janelas`, sem substituir o código vivo. [app/docs/JUIZES.md:14] [app/docs/JUIZES.md:29] [app/docs/JUIZES.md:35]

| Valor somado pelo anexo | Quem escreve | Quem lê |
|---|---|---|
| `v2_h` publicado | Lavra `motor`: períodos, folgas, órfãos e demais mapas; objetos em `:343`. [app/ponto/services/dia_pago.py:173] [app/ponto/services/dia_pago.py:234] [app/ponto/services/dia_pago.py:267] [app/ponto/services/dia_pago.py:343] | Captador → `acumular` → `comparados` → `publicar`. É somente o subconjunto decidido da comparação. [anexo `q3_script_p1.py`; nao verificado] | [nao verificado]
| `v3_h` publicado | A mesma lavra, substituindo campos decididos pela porta. [app/ponto/services/dia_pago.py:328] | O mesmo percurso do anexo, versão `oraculo`. [nao verificado] | [nao verificado]
| `porta` | `regras.do_dia`, pela alimentação chamada pelo fechamento. [app/ponto/calculador/alimentacao.py:514] [app/ponto/services/fechamento.py:1255] | `lavrar`, o DIFF e o captador do anexo. [app/ponto/services/dia_pago.py:322] [app/ponto/management/commands/diff_calculador.py:469] [nao verificado quanto ao anexo] |
| `emitido_decidido` | Objetos produzidos pelo único emissor. [app/ponto/services/dia_pago.py:343] | O anexo seleciona os campos presentes e não cegos. [nao verificado] |
| `INSTRUMENTO_HISTORICO` | `m_dia` de períodos e `c_dia` da porta; `S` soma essas leituras. [app/ponto/management/commands/diff_calculador.py:253] [app/ponto/management/commands/diff_calculador.py:501] [app/ponto/management/commands/diff_calculador.py:536] | Tracer do anexo. **Não é a soma dos objetos emitidos.** [nao verificado] |

Leitores produtivos concretos: calendário lê linhas e `rubricas_do_dia`; export lê dias da versão `motor`; a conferência da competência agrega os campos lavrados. Isso não constitui censo global dos leitores de `oraculo`. [app/colaboradores/services/calendario.py:290] [app/colaboradores/services/calendario.py:656] [app/folha/porta_export.py:588] [app/ponto/services/dia_pago.py:560] [nao verificado quanto ao censo global]

Segue a proposta completa, **não executada**. Ela admite o colaborador somente depois das conferências, aceita lavra bem sucedida sem DIA e não usa o registro global de exceções para invalidar os medidos. `SOMA COBERTURA` passa a conferir a contabilização **medidos + excluídos**; `COBERTURA_INTEGRAL` informa separadamente se houve exclusões. Os limites de tempo/RSS são cooperativos; cabimento efetivo em 2 GB/15 minutos permanece [nao verificado].

```python
# PROPOSTA NAO EXECUTADA. Todo comportamento/performance desta reescrita:
# [nao verificado].
#
# Medida de IMPACTO, nunca validacao: CLAUDE.md:50,58,415.
# LEI-AKITA: origem=captura antes de lavrar;
# testemunha=lavrar real + retorno real do fechamento;
# RED=[nao verificado]; quem-mais-le=JUIZES.md:29,35 e leitores citados;
# juizes novos=0; padrao=n/a (instrumento).
#
# Nenhum arquivo e aberto para escrita por este script.
# Executar somente pelo envelope de leitura ja autorizado/configurado:
# bin/sonda_leitura.sh:237-240.
#
# PRODUTO.n_dias conta datas observadas associadas a classe, sem multiplicar
# por rubrica. Na recusa: dias preparados pela lavra + chaves da porta.
# Na conservacao mensal: dias que contribuiram ao total daquele campo.
# Ausencia de data observavel aparece adicionalmente como DIAS_NAO_LOCALIZADOS.
# Classes de PRODUTO podem sobrepor: nao somar suas contagens entre classes.

import collections
import datetime as dt
import gc
import linecache
import logging
import math
import resource
import sys
import time
from contextlib import redirect_stdout, redirect_stderr
from decimal import Decimal
from unittest.mock import Mock, patch

from django.conf import settings
from django.db import connection, reset_queries, transaction
from django.utils import timezone

from ponto.calculador import regras
from ponto.catalogo import ausencias as CAT
from ponto.management.commands import diff_calculador as DC
from ponto.management.commands.e6_oraculo import InsumosDoDono
from ponto.models import DiaPago
from ponto.services import dia_pago as DP
from ponto.services import fechamento as F
PARTE = 1
PARTES = 6

sys.dont_write_bytecode = True

COMPETENCIAS = ((2026, 10), (2026, 11))
RUBRICAS = tuple(r for r in regras.RUBRICAS if r not in regras.NAO_DECIDE)
DONOS = {"BATIDA", "CADASTRO", "ESTRUTURA"}
LIMIAR_H = 0.02  # diff_calculador.py:538
INICIO = time.monotonic()
PRAZO = INICIO + 14 * 60       # reserva para diagnostico/publicacao
RSS_LIMITE_KIB = 1900 * 1024  # Linux; controle cooperativo
HOJE, AGORA = timezone.localdate(), timezone.now()

REAL_RECALC = F.recalcular_fechamento_mes
REAL_LAVRAR = DP.lavrar
COD_COMANDO = DC.Command.handle.__code__
COD_LAVRAR = REAL_LAVRAR.__code__
RAIZ_APP = DC.__file__.split("/ponto/management/commands/")[0] + "/"

SOMAS = []
TOTAL_EXCLUIDOS = 0
TOTAL_INSTRUMENTO = 0


class FalhaInstrumento(Exception):
    pass


class Limite(BaseException):
    def __init__(self, causa):
        self.causa = causa


class Descartar:
    def write(self, texto):
        return len(texto)

    def flush(self):
        pass

    def isatty(self):
        return False


DESCARTAR = Descartar()


def rotulo(cid):
    # Mesma convencao do anexo. Nunca imprimir cid isolado, modelos,
    # nomes, CPF, argumentos SQL ou str(exc).
    if not isinstance(cid, int) or isinstance(cid, bool):
        raise FalhaInstrumento("IDENTIFICADOR_INVALIDO")
    return "col%d" % cid


def limites():
    # Nao interrompe, por si so, consulta bloqueada em codigo nativo.
    if time.monotonic() >= PRAZO:
        raise Limite("PRAZO")
    if resource.getrusage(resource.RUSAGE_SELF).ru_maxrss >= RSS_LIMITE_KIB:
        raise Limite("MEMORIA")


def numero(valor):
    try:
        n = float(valor)
    except (TypeError, ValueError, OverflowError):
        raise FalhaInstrumento("NUMERO_INVALIDO") from None
    if not math.isfinite(n):
        raise FalhaInstrumento("NUMERO_NAO_FINITO")
    return n


def soma(xs):
    return math.fsum(xs)


def igual(a, b):
    return math.isclose(a, b, rel_tol=1e-12, abs_tol=1e-8)


def centavos(valor):
    # Mantem o criterio do anexo, por colaborador.
    return Decimal(str(round(numero(valor), 2)))


def datas(xs):
    return {x for x in xs if type(x) is dt.date}


def tipo_seguro(exc):
    nome = type(exc).__name__
    if nome.isascii() and nome.isidentifier() and len(nome) <= 80:
        return nome
    return "Excecao"


def ultimo_tb(exc):
    tb = exc.__traceback__
    while tb is not None and tb.tb_next is not None:
        tb = tb.tb_next
    return tb


def origem(exc):
    tb = ultimo_tb(exc)
    if tb is None:
        return "sem_traceback"
    nome = tb.tb_frame.f_code.co_filename
    if nome.startswith(RAIZ_APP):
        return "app/%s:%d" % (nome[len(RAIZ_APP):], tb.tb_lineno)
    return "fora_do_app:%d" % tb.tb_lineno


def novo_resultado():
    return {
        "ids": set(), "cortes": 0, "completo": False, "historico": None,
        "janela": None, "colab": None, "fecho": None,
        "tentativas": {"motor": 0, "oraculo": 0},
        "sucessos": {"motor": 0, "oraculo": 0},
        "emitido": {"motor": {}, "oraculo": {}},
        "porta": {}, "cegos": {},
        "faltas_porta_dc": set(),
        "produto": {}, "instrumento": {},
        "nao_localizados": set(), "causas": set(),
        "excecoes": [], "recusas": [], "parada": None,
    }


def registrar(c, natureza, classe, dias=(), bloqueia=False, localizado=True):
    conjunto = datas(dias)
    c[natureza].setdefault(classe, set()).update(conjunto)
    if not localizado:
        c["nao_localizados"].add((natureza, classe))
    if bloqueia:
        c["causas"].add(natureza.upper() + ":" + classe)


def instrumental(c, classe, dias=()):
    registrar(c, "instrumento", classe, dias, bloqueia=True)


def produto(c, classe, dias=(), bloqueia=False, localizado=True):
    registrar(c, "produto", classe, dias, bloqueia, localizado)


def falhou(c, exc, operacao, dias=()):
    if isinstance(exc, Limite):
        instrumental(c, exc.causa, dias)
        c["parada"] = exc.causa
    elif isinstance(exc, MemoryError):
        instrumental(c, "MEMORIA", dias)
        c["parada"] = "MEMORIA"
    elif isinstance(exc, FalhaInstrumento):
        instrumental(c, exc.args[0], dias)
    else:
        classe = "FALHA_%s_%s" % (operacao, tipo_seguro(exc))
        produto(c, classe, dias, bloqueia=True,
                localizado=bool(datas(dias)))
        c["excecoes"].append((operacao, tipo_seguro(exc), origem(exc)))


def limpar():
    # Catalogo: cache de modulo; demais referencias sao de um colaborador.
    # app/ponto/catalogo/ausencias.py:257,261.
    CAT.invalidar_cache()
    reset_queries()
    gc.collect()


def executar(ano, mes, alvo=None):
    c = novo_resultado()

    def observar(frame, evento, arg):
        # A sonda precisa de linhas somente no comando.
        # Excecao interna tratada nao e, por si, falha da medicao.
        if frame.f_code is not COD_COMANDO:
            return None
        frame.f_trace_lines = True
        loc = frame.f_locals

        if evento == "line":
            limites()
            if frame.f_lineno == 116:
                # Antes do fechamento e do laco: diff_calculador.py:116.
                lista = loc.get("ids")
                if not isinstance(lista, list):
                    raise FalhaInstrumento("IDS_NAO_LISTA")
                if any(not isinstance(x, int) or isinstance(x, bool)
                       for x in lista):
                    raise FalhaInstrumento("IDS_INVALIDOS")
                if len(lista) != len(set(lista)):
                    raise FalhaInstrumento("IDS_DUPLICADOS")

                c["cortes"] += 1
                if alvo is None:
                    if c["ids"].intersection(lista):
                        raise FalhaInstrumento("IDS_ENTRE_EMPRESAS")
                    c["ids"].update(lista)
                    lista[:] = []
                else:
                    lista[:] = [
                        x for x in lista
                        if x == alvo and x % PARTES == PARTE - 1
                    ]
                    if len(lista) > 1 or c["ids"].intersection(lista):
                        raise FalhaInstrumento("ALVO_DUPLICADO")
                    c["ids"].update(lista)

            if frame.f_lineno == 471:
                # Dia do nucleo sem resposta da porta, nao "dia sem batida".
                # diff_calculador.py:469-472.
                c["faltas_porta_dc"].update(datas([loc.get("k")]))

            if frame.f_lineno == 550:
                c["completo"] = True

        elif evento == "return":
            S, T = loc.get("S"), loc.get("T")
            if isinstance(S, dict) and isinstance(T, collections.Counter):
                c["historico"] = {
                    "S": {r: tuple(v) for r, v in S.items()},
                    "T": dict(T),
                }
        return observar

    def fechamento_capturado(m, a, **kw):
        limites()
        chamador = sys._getframe(1)
        if chamador.f_code is not COD_COMANDO:
            raise FalhaInstrumento("CHAMADOR_FECHAMENTO")
        loc = chamador.f_locals
        ids = set(loc.get("ids", ()))
        if not ids:
            return {}
        if alvo is None or ids != {alvo}:
            raise FalhaInstrumento("ESCOPO_FECHAMENTO")
        if (m, a) != (mes, ano):
            raise FalhaInstrumento("COMPETENCIA_FECHAMENTO")
        if not kw.get("somente_leitura") or not kw.get("lavrar_dias_pagos"):
            raise FalhaInstrumento("MODO_FECHAMENTO")

        ini, fim = loc.get("ini"), loc.get("fim")
        if type(ini) is not dt.date or type(fim) is not dt.date or ini > fim:
            raise FalhaInstrumento("JANELA_INVALIDA")
        c["janela"] = (ini, fim)

        def emissor_capturado(colab, mm, aa, **dados):
            limites()
            versao = dados.get("versao", "motor")
            if (colab.pk != alvo or (mm, aa) != (mes, ano)
                    or versao not in ("motor", "oraculo")):
                raise FalhaInstrumento("ESCOPO_LAVRAR")
            if c["tentativas"][versao]:
                raise FalhaInstrumento("LAVRAR_REPETIDA_" + versao.upper())

            c["tentativas"][versao] += 1
            c["colab"] = colab

            # CURA DA CAPTURA: guardar a entrada ANTES da chamada real.
            # Fonte: fechamento.py:1255,1272.
            if versao == "oraculo":
                rubricas = dados.get("rubricas")
                if not isinstance(rubricas, dict):
                    raise FalhaInstrumento("RUBRICAS_NAO_CAPTURADAS")
                if dados.get("do_motor"):
                    raise FalhaInstrumento("PROVENIENCIA_DO_MOTOR_MUDOU")
                for dia, rub in rubricas.items():
                    if type(dia) is not dt.date:
                        raise FalhaInstrumento("DATA_PORTA_INVAL

[truncado em 20000 de 45545 caracteres]

## 06/11 13:51 · sessao cd0d5a68c7a6 · - · - · origem=Ronald (tmux)

A dedução está correta para o script anexo: `sonda` significa `tb=None`. Um `raise` preservaria traceback e produziria um sítio com linha; portanto os 3191 registros `sonda LacunaDeMedicao` não demonstram disparo de nenhum `raise LacunaDeMedicao()` literal. Eles vêm de `falha(LacunaDeMedicao())` sem lançamento: contadores `T`, emissão DIA vazia, fecho ausente ou par diário ausente. [nao verificado]

Em P1, `SEM_PAR` permite atribuir 237 lacunas de outubro e 329 de novembro ao teste diário; restam 47 e 24 lacunas sintéticas cuja divisão entre os outros testes não é recuperável. [nao verificado] [nao verificado]

Analogamente, os `sonda ConservacaoNaoFecha` vêm das chamadas sintéticas de conferência mensal ou porta/emissor; o `raise` de `n != len(capturadas)` teria sítio com linha. [nao verificado]

## 06/11 13:49 · sessao cd0d5a68c7a6 · - · - · origem=Ronald (tmux)

A falha importante seria excluir por qualquer exceção propagada capturada pelo tracer. A alimentação trata exceções e continua com abstenção/default declarado em `app/ponto/calculador/alimentacao.py:389` e `app/ponto/calculador/alimentacao.py:500`; o tracer deve manter diagnóstico separado. Exclusão exige falha da operação ou violação concreta do contrato.

“Tentativa sem retorno” só deve receber classe PRODUTO quando a falha veio da autoridade real; exceção levantada pelo captador é INSTRUMENTO. [nao verificado]

Também preserve `SOMA` independente do registro global de erros e da totalidade do universo: valide antes de acumular e julgue apenas os colaboradores admitidos. Caso contrário, a exclusão declarada recria o `False` geral. [nao verificado]

## 06/11 13:47 · sessao a01dce60e12a · - · - · origem=Ronald (tmux)

Mapa para a pergunta 4:

| Valor | Quem escreve a fonte | Quem lê |
|---|---|---|
| `v2_h` da `RUBRICA` do anexo | `DP.lavrar`: períodos normais em `app/ponto/services/dia_pago.py:173`; campos em `:178`–`:186`; FT em `:234`, `:244`, `:257`; atraso em `:190`; órfãos em `:267`. Constrói `DiaPago` em `:343` e persiste em `:358`. Chamada real em `app/ponto/services/fechamento.py:1185`, versão motor em `:1211`. | No anexo: `emissor_capturado` → `c["emitido"]["motor"]` → `acumular` → `comparados` → `publicar`; numeração do anexo [nao verificado]. |
| `v3_h` da `RUBRICA` do anexo | A mesma `DP.lavrar`, substituindo valores decididos da porta em `app/ponto/services/dia_pago.py:320`–`:328`, construindo em `:343` e persistindo em `:358`; chamada em `app/ponto/services/fechamento.py:1261`, `rubricas` em `:1272`, versão em `:1278`. | No anexo: `emissor_capturado` → `c["emitido"]["oraculo"]` → `acumular` → `comparados` → `publicar`; numeração [nao verificado]. |
| `porta` | `rubricas_por_dia` é a fronteira, `app/ponto/calculador/alimentacao.py:353`; chamada do fechamento em `app/ponto/services/fechamento.py:1255`; o diff tem sua própria chamada real dessa mesma função em `app/ponto/management/commands/diff_calculador.py:417`. | DP lê os `Rubricas.valores` em `app/ponto/services/dia_pago.py:322`; diff lê em `app/ponto/management/commands/diff_calculador.py:469` e guarda em `:501`; anexo lê kwargs de `lavrar`, atualmente somente após retorno bem-sucedido [nao verificado]. |
| `emitido_decidido` | Objetos `DiaPago` construídos pelo único emissor `app/ponto/services/dia_pago.py:343`; horas de origem do motor e substituições descritas acima. | Anexo seleciona apenas células com `r in valores` e `r not in cegos`, somando `b[r]`; numeração [nao verificado]. |
| `INSTRUMENTO_HISTORICO` | Diferente de `v2_h` emitido: `m_dia` montado de períodos em `app/ponto/management/commands/diff_calculador.py:246`–`:268`; `c_dia` vem da porta em `:501`; `S` acumula em `:535`–`:537`. | Tracer lê `S` no retorno do comando; anexo [nao verificado]. |

Autoridades de referência: `app/docs/JUIZES.md:29` nomeia motor/fábrica CCT; `:35` nomeia janela de folha; `:20` nomeia geometria. O resultado composto nasce em `app/ponto/services/fechamento.py:182`, cada fatia chama o motor em `:277`, e a composição ocorre em `:285`–`:291`. O `setdefault` do tracer nativo do comando em `app/ponto/management/commands/diff_calculador.py:131`–`:135` continua capturando a primeira instância composta.

Leitores produtivos concretos:

- Calendário pede as linhas em `app/colaboradores/services/calendario.py:290` e lê a rubrica em `:655`–`:656`; a porta tem versão padrão motor em `app/ponto/services/dia_pago.py:831`.
- `rubricas_do_dia` lê campos em `app/ponto/services/dia_pago.py:912`–`:924`; não soma variantes noturna/feriado aos totais, conforme `:902`–`:904`.
- Porta export consulta dias motor em `app/folha/porta_export.py:588`–`:589`.
- `soma_da_competencia` agrega campos de `DiaPago` em `app/ponto/services/dia_pago.py:557`–`:561`, com arredondamento final.
- O comentário “nenhum leitor lê oráculo” de `app/ponto/services/fechamento.py:1231` é histórico; não entreguei censo global de todos os leitores [nao verificado].

Defeito de captura confirmado na lógica do anexo: rubricas/cegos são guardados **depois** de `REAL_LAVRAR`. A recusa impede a captura e depois aparece como emissão ausente/SEM_PAR, produzindo falhas secundárias [nao verificado]. A correção deve capturar kwargs antes da chamada real e registrar a recusa à parte; não inventar emissão. O produto captura a exceção e continua em `app/ponto/services/fechamento.py:1281`.

## 06/11 13:46 · sessao cd0d5a68c7a6 · - · - · origem=Ronald (tmux)

- **A saída não permite repartir exatamente as 3191 lacunas por sítio.** O registro sintético usa `tb=None` e agrupa em `sonda`; não conserva a causa nem a linha chamadora. A associação integral das contagens aos sítios é [nao verificado]. P1 informa 237 dias `SEM_PAR` em outubro e 329 em novembro; o loop do anexo registra uma lacuna por chave com um emissor ausente, mas outras lacunas vêm dos testes de vazio, `T`, fechos e donos. A decomposição restante é [nao verificado]. [nao verificado]

- **Defeito concreto da captura:** o anexo registra `colabs`, `rubricas` e `nao_decide` apenas depois de `REAL_LAVRAR` retornar. A recusa em `dia_pago.py:315` impede essa captura, embora o chamador tenha fornecido `_rub_o` em `fechamento.py:1272`. O próprio fechamento captura a recusa em `fechamento.py:1281` e continua. Assim, uma recusa declarada gera artificialmente “oráculo vazio” e lacunas para todos os dias do motor no script anexo. Capture argumentos antes da chamada, registre tentativa/retorno independentemente e classifique a recusa uma única vez.

- **Outro defeito lógico do instrumento:** `if not c["emitido"][versao]` não distingue chamada ausente de lavratura bem sucedida sem linhas DIA. A função sempre emite AJUSTE (`dia_pago.py:346`, `dia_pago.py:355`), enquanto os dias dependem de períodos, furos e mapas (`dia_pago.py:173`, `dia_pago.py:267`, `dia_pago.py:272`, `dia_pago.py:280`, `dia_pago.py:286`). Ocorrência desse segundo caso nos anexos: [nao verificado].

- **O que torna um dia lacuna no anexo:** pertencer à união de chaves DIA motor/oráculo e faltar em um dos mapas; não depende de valor diferente, presença de batida ou rubrica divergente. O exemplo com data é produzido por esse teste ou pelos testes de janela/dono; a saída agregada não guarda o sítio para distinguir exatamente [nao verificado]. A captura proposta deve registrar classe explícita por dia. [nao verificado]

- **Proveniência real do caminho:** o comando chama o fechamento em `diff_calculador.py:140`, após instalar `_nada` em `diff_calculador.py:137`; o wrapper do anexo substitui temporariamente essa função por seu captador durante a chamada real. O fechamento chama `lavrar` motor em `fechamento.py:1185` e oráculo em `fechamento.py:1261`; este recebe `rubricas=_rub_o` em `fechamento.py:1272`. A alimentação escreve `_rub_o` pela chamada `fechamento.py:1255`; a função retorna o mapa em `calculador/alimentacao.py:533` e escreve cada entrada com `regras.do_dia` em `calculador/alimentacao.py:514`.

- **Os universos de dias são distintos:** a porta do calculador visita os dias do núcleo mais os dias bloqueados publicados pelo motor (`calculador/alimentacao.py:467`, `calculador/alimentacao.py:469`, `calculador/alimentacao.py:471`); a lavratura também cria dias por furos e grade (`dia_pago.py:280`, `dia_pago.py:286`). Consequentemente, ausência na porta é abstenção ou diferença de universo a investigar, não automaticamente defeito de captura.

- **Histórico do DIFF não é a soma emitida:** o comando escreve `m_dia` diretamente de períodos em `diff_calculador.py:253`, escreve `c_dia` a partir das rubricas em `diff_calculador.py:501`, exclui cegueiras em `diff_calculador.py:532` e soma `S` em `diff_calculador.py:536`. O script soma objetos DIA capturados de `lavrar`; são grandezas/caminhos distintos.

- Para a reescrita: capture a porta **antes** da chamada; conte chamada e retorno explicitamente; preserve recusa esperada como `RECUSA_PORTA_SEM_DONO`; não transforme recusa em centenas de lacunas derivadas; valide cada colaborador antes de acumular ou imprimir `DIA`, `DONO` e `RESIDUO`; publique classes de produto separadamente; mantenha contagem exata dos dias afetados por conjuntos, evitando multiplicar um dia por rubrica. São recomendações de desenho [nao verificado].

## 06/11 13:46 · sessao 435a81aabac3 · - · - · origem=Ronald (tmux)

Referências confirmadas, sem executar sonda ou alterar arquivos:

- `porta`: escritor primário é `regras.do_dia`, chamado por `rubricas_por_dia` em `app/ponto/calculador/alimentacao.py:514`; retorno em `app/ponto/calculador/regras.py:874`. O fechamento chama essa porta em `app/ponto/services/fechamento.py:1255` e passa `rubricas=_rub_o` a `lavrar` em `app/ponto/services/fechamento.py:1272`.
- `emitido`: `lavrar` primeiro distribui motor em `app/ponto/services/dia_pago.py:178`, extras em `:180`, noturnas em `:181`/`:184`, folgas em `:239`, feriado em `:249`, ligação/grade em `:267`/`:286`; substitui cada campo decidido pela porta em `:328`; constrói `DiaPago` em `:343`; persiste em `:357`/`:358`.
- Neste caminho, o fechamento não passa `do_motor` (`app/ponto/services/fechamento.py:1261`). Portanto cada célula decidida capturada da porta deve reaparecer idêntica no objeto emitido pela atribuição `float(_valor or 0)` (`app/ponto/services/dia_pago.py:328`). Uma diferença atribuída especificamente a essa igualdade exigiria identificar células, campos e conjuntos; a saída fornecida não identifica isso. [nao verificado]
- Os 740 `ConservacaoNaoFecha` não provam 740 falhas porta/emissor: o anexo usa a mesma classe em três lugares (`n != capturadas`, motor/dias versus fechamento, porta versus emissor) e publica todos como `sonda`, sem sítio. Distribuição entre os três locais: [nao verificado].
- P1 publica diferença real do seu próprio instrumento em novembro/trabalhadas: `dias=5478.32`, `retorno=5478.33`; causa exata e colaboradores responsáveis não constam. [nao verificado] O mensal arredonda os totais em `app/ponto/services/fechamento.py:909` e seguintes; trabalhadas também pode receber segunda soma/arredondamento em `:968`. Comparar `round(fsum(DiaPago),2)` com mensal que já arredondou parcela antes de somar outra pode revelar diferença de produto, mas não autoriza nomear todas as 740 assim. [nao verificado]
- Se mesmos insumos e mesmo campo produzido pela ligação interna divergem, o dono provável é `ESTRUTURA`, conforme coerência exigida pela L-099 em `CLAUDE.md:176`; campo concreto de cada ocorrência permanece [nao verificado].

A recusa:

- `diff_calculador` chama `_recalc` em `app/ponto/management/commands/diff_calculador.py:140`; ele normalmente substitui `lavrar` por `_nada` em `:137`, mas o wrapper anexo reinstala seu captador dentro da chamada real. [nao verificado quanto à execução]
- O chamador real com `rubricas` é `app/ponto/services/fechamento.py:1261`/`:1272`. O próprio fechamento captura a exceção em `:1281`, registra e segue; assim `rc=0` não prova lavratura completa (`:1293`).
- A recusa em `app/ponto/services/dia_pago.py:315` protege campos sem proveniência; `_DONOS_DECLARADOS` reúne chaves de `valores` **e** `nao_decide` em `:61`–`:75`. Recusa é comportamento esperado do guard.
- A mensagem da recusa ainda fala em lei do ancoramento pendente (`app/ponto/services/dia_pago.py:317`), mas é texto histórico: a lei vigente foi respondida, com ancoramento por período, em `app/ponto/calculador/regras.py:414`–`:423`; a implementação chama a mesma `he_noturna` em `:825`/`:832`.
- Só parte dos colaboradores recusa porque o conjunto de declarações depende de `rubricas.values()`. Se o mapa é vazio, não há declaração de nenhum dos onze campos do calculador, embora grade/falta possam estar declaradas (`app/ponto/services/dia_pago.py:61`, `:64`, `:66`). A própria porta documenta o caso `rubricas={}` e onze campos sem dono em `app/ponto/calculador/alimentacao.py:459`–`:466`; seu universo é núcleo com pares válidos mais dias bloqueados publicados pelo motor (`:467`–`:475`).
- Não se pode assegurar que todas as 182 recusas sejam mapa vazio: nomes dos campos recusados não aparecem no dado raspado. [nao verificado] A nova captura deve calcular `_sem_dono` **antes** de `REAL_LAVRAR`, guardar classe própria e quantidade de dias do universo emitido/motor, e excluir o colaborador inteiro.
- FOLGA com splits sem valor/causa já tem declaração explícita em `app/ponto/calculador/regras.py:704`–`:715`; não atribuir automaticamente recusas atuais a esse defeito histórico.
- As frequências “16–21” valem para exemplos de algumas partes; no agregado fornecido variam 11–21 ocorrências por parte/competência. [nao verificado]

JUIZES aponta cálculo para motor/CCT em `app/docs/JUIZES.md:29`, régua em `:30`, janela em `:35`. A medida deve permanecer IMPACTO, nunca validação, por `CLAUDE.md:58` e `CLAUDE.md:415`.

## 06/11 13:37 · sessao 6a744852f01e · R1-CAUSAD-V3 · leitura · origem=Code

A consolidação abaixo mede **impacto**, conforme a distinção da L-110; não certifica o v3. [CLAUDE.md:49](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/CLAUDE.md:49)

Os números e suas contas derivam exclusivamente dos seis anexos, sem verificação independente. Nas tabelas, cada célula `Δ/C/D` significa **delta em horas / colaboradores divergentes / dias-colaborador divergentes**. Os nomes das rubricas omitem somente o prefixo `horas_`. [nao verificado]

A coluna **Σ emitido** soma os `delta_h` de `RUBRICA`, preservando todas as casas publicadas. A última coluna calcula separadamente `Σ(PARCIAL.v3_h − PARCIAL.v2_h)`, usando integralmente os decimais transcritos, sem arredondamento intermediário. [nao verificado] [nao verificado]

Para **10/2026**, a conta parte a parte é: [nao verificado]

| Rubrica | P1 Δ/C/D | P2 Δ/C/D | P3 Δ/C/D | P4 Δ/C/D | P5 Δ/C/D | P6 Δ/C/D | Σ emitido Δ/C/D | Δ pelos PARCIAL integrais | Evidência |
|---|---|---|---|---|---|---|---|---|---|
| trabalhadas | −2.542890556/1/1 | −2.842305278/1/1 | −3.035321667/2/2 | 0/0/0 | −9.540302056/3/3 | −4.528065278/1/1 | **−22.488884835/8/8** | indisponível integralmente¹ | [nao verificado] |
| intra_indenizada | −2/1/2 | 0/0/0 | −4.056666944/4/9 | −0.635273333/2/3 | −8.166666667/1/7 | −8/2/8 | **−22.858606944/10/29** | −22.85860694444437 | [nao verificado] |
| extras_50 | 0/0/0 | 0/0/0 | 0/0/0 | −0.502948333/1/2 | +1.473515854/2/5 | +0.224154213/1/1 | **+1.194721734/4/8** | +1.1947217330555591 | [nao verificado] |
| extras_100 | 0/0/0 | +10.985971389/1/1 | +12.149131944/1/1 | 0/0/0 | 0/0/0 | 0/0/0 | **+23.135103333/2/2** | +23.135103333333337 | [nao verificado] |
| extras_100_feriado | 0/0/0 | +10.985971389/1/1 | +12.149131944/1/1 | 0/0/0 | 0/0/0 | 0/0/0 | **+23.135103333/2/2** | +23.135103333333337 | [nao verificado] |
| extras_50_noturna | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | **0/0/0** | 0 | [nao verificado] |
| extras_100_noturna | +2.061344224/3/3 | +7.021076119/3/3 | +0.014159722/0/0 | +1.404236338/2/2 | +0.911578611/1/1 | +1/1/1 | **+12.412395014/10/10** | +12.4123950152777756 | [nao verificado] |
| folga_trabalhada | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | **0/0/0** | 0 | [nao verificado] |
| noturnas | 0/0/0 | −2.724547857/3/11 | 0/0/0 | +2.571669984/1/3 | +0.561815714/1/4 | +2.164548292/1/8 | **+2.573486133/6/26** | +2.5734861329370 | [nao verificado] |
| atraso | +2.368802113/1/1 | +4.579204284/3/4 | +1.106468535/1/1 | +0.186039155/1/1 | +2.329948153/1/1 | −0.000374092/0/0 | **+10.570088148/7/8** | +10.570088148888894 | [nao verificado] |
| saida_antecipada | +0.000684535/0/0 | +0.000106703/0/0 | −0.557056843/1/1 | −0.186263275/1/1 | +0.000085790/0/0 | +0.000069986/0/0 | **−0.742373104/2/2** | −0.742373103611115 | [nao verificado] |

¹ O `PARCIAL` de P2/trabalhadas traz `v2_h=11749.[cpf]`. Sem reconstruir esse valor, a soma integral das seis partes fica indisponível. A soma dos cinco deltas integrais disponíveis é `−19.646579556389`; acrescentar o delta explícito de `RUBRICA` de P2, `−2.842305278`, produz **−22.488884834389**, com precisão mista. [nao verificado]

Para **11/2026**, a conta parte a parte é: [nao verificado]

| Rubrica | P1 Δ/C/D | P2 Δ/C/D | P3 Δ/C/D | P4 Δ/C/D | P5 Δ/C/D | P6 Δ/C/D | Σ emitido Δ/C/D | Δ pelos PARCIAL integrais | Evidência |
|---|---|---|---|---|---|---|---|---|---|
| trabalhadas | −0.000000000/0/0 | −2.842521667/1/1 | −3.993019444/1/1 | 0/0/0 | −17.080034312/3/5 | 0/0/0 | **−23.915575423/5/7** | −23.9155754230560 | [nao verificado] |
| intra_indenizada | −1.166666667/1/1 | 0/0/0 | −1/1/1 | +0.403343056/1/2 | −9.628472778/3/10 | −6/2/6 | **−17.391796389/8/20** | −17.39179638888888 | [nao verificado] |
| extras_50 | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | −3.336169674/3/3 | +0.380845000/1/2 | **−2.955324674/4/5** | −2.9553246744444423 | [nao verificado] |
| extras_100 | 0/0/0 | −7.052801863/1/1 | +12.066969812/1/1 | +3.958275278/1/1 | +20.045843333/1/3 | 0/0/0 | **+29.018286560/4/6** | +29.018286560000008 | [nao verificado] |
| extras_100_feriado | 0/0/0 | −7.052801863/1/1 | +12.066969812/1/1 | +3.958275278/1/1 | +16.043546944/1/1 | 0/0/0 | **+25.015990171/4/4** | +25.015990171111121 | [nao verificado] |
| extras_50_noturna | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | **0/0/0** | 0 | [nao verificado] |
| extras_100_noturna | +1.137680830/2/2 | +7.819367573/3/3 | +1/1/1 | +1.499958300/2/2 | +0.002409167/0/0 | +1.861278889/1/1 | **+13.320694759/9/9** | +13.3206947588888876 | [nao verificado] |
| folga_trabalhada | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | **0/0/0** | 0 | [nao verificado] |
| noturnas | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | **0/0/0** | 0 | [nao verificado] |
| atraso | −0.000158524/0/0 | +0.622596847/1/1 | +0.000051118/0/0 | +1.128749770/2/4 | +0.076214444/1/1 | −0.000105974/0/0 | **+1.827347681/4/6** | +1.827347681944451 | [nao verificado] |
| saida_antecipada | −0.000158889/0/0 | +0.000201111/0/0 | +0.000008889/0/0 | −0.755222121/1/3 | −0.076319289/1/1 | +4.331691892/1/1 | **+3.500201593/3/5** | +3.500201593055542 | [nao verificado] |

As duas colunas de soma preservam a diferença de precisão existente nos anexos. Por exemplo, outubro/extras_50 soma **+1.194721734** pelos deltas impressos; pelos `PARCIAL`, soma **+1.1947217330555591**, que somente ao final, em nove casas, resulta em **+1.194721733**. [nao verificado]

Os **donos de 10/2026** seguem abaixo. Cada parcela mantém `Δ/C/D`; partes omitidas não publicaram uma linha `DONO` para aquela combinação. A linha conjunta de extras_100/feriado vale separadamente para cada uma das duas rubricas. [nao verificado]

| Rubrica | Dono | Conta por parte — Δ/C/D | Total Δ/C/D | Evidência |
|---|---|---|---|---|
| trabalhadas | CADASTRO | P1(−2.542890556/1/1) + P2(−2.842305278/1/1) + P5(−9.540302056/3/3) + P6(−4.528065278/1/1) | **−19.453563168/6/6** | [nao verificado] |
| trabalhadas | BATIDA | P3(−8/1/1) | **−8/1/1** | [nao verificado] |
| trabalhadas | ESTRUTURA | P3(+4.964678333/1/1) | **+4.964678333/1/1** | [nao verificado] |
| intra_indenizada | BATIDA | P1(−2/1/2) + P3(−1.550810000/2/2) | **−3.550810000/3/4** | [nao verificado] |
| intra_indenizada | CADASTRO | P3(−3.505856944/3/6) + P4(−0.635273333/2/3) + P5(−8.166666667/1/7) + P6(−8/2/8) | **−20.307796944/8/24** | [nao verificado] |
| intra_indenizada | ESTRUTURA | P3(+1/1/1) | **+1/1/1** | [nao verificado] |
| extras_50 | CADASTRO | P4(−0.502948333/1/2) + P5(+1.473515854/2/5) + P6(+0.224154212/1/1) | **+1.194721733/4/8** | [nao verificado] |
| extras_100 e extras_100_feriado | ESTRUTURA | P2(+10.985971389/1/1) + P3(+12.149131944/1/1) | **+23.135103333/2/2**, cada rubrica | [nao verificado] |
| extras_100_noturna | CADASTRO | P2(+6.164677786/2/2) + P6(+1/1/1) | **+7.164677786/3/3** | [nao verificado] |
| extras_100_noturna | ESTRUTURA | P1(+2.063954502/3/3) + P2(+0.856398333/1/1) + P4(+1.404236338/2/2) + P5(+0.909870556/1/1) | **+5.234459729/7/7** | [nao verificado] |
| noturnas | BATIDA | P5(+0.145124127/1/1) | **+0.145124127/1/1** | [nao verificado] |
| noturnas | CADASTRO | P5(+0.134280516/1/1) + P6(+1.069427260/1/4) | **+1.203707776/2/5** | [nao verificado] |
| noturnas | ESTRUTURA | P2(−2.714647817/3/11) + P4(+2.571669984/1/3) + P5(+0.282411071/1/2) + P6(+1.095121032/1/4) | **+1.234554270/6/20** | [nao verificado] |
| atraso | CADASTRO | P2(+1/1/1) + P4(+0.186000000/1/1) | **+1.186000000/2/2** | [nao verificado] |
| atraso | ESTRUTURA | P1(+2.368500000/1/1) + P2(+3.578715833/2/3) + P3(+1.106833333/1/1) + P5(+2.330333333/1/1) | **+9.384382499/5/6** | [nao verificado] |
| saida_antecipada | CADASTRO | P4(−0.186166667/1/1) | **−0.186166667/1/1** | [nao verificado] |
| saida_antecipada | ESTRUTURA | P3(−0.557166667/1/1) | **−0.557166667/1/1** | [nao verificado] |

Os **donos de 11/2026**, com a mesma notação, são: [nao verificado]

| Rubrica | Dono | Conta por parte — Δ/C/D | Total Δ/C/D | Evidência |
|---|---|---|---|---|
| trabalhadas | CADASTRO | P2(−2.842521667/1/1) + P5(−17.080034312/3/5) | **−19.922555979/4/6** | [nao verificado] |
| trabalhadas | BATIDA | P3(−3.993019444/1/1) | **−3.993019444/1/1** | [nao verificado] |
| intra_indenizada | CADASTRO | P1(−1.166666667/1/1) + P3(−1/1/1) + P4(+0.403343056/1/2) + P5(−7.628472778/3/8) + P6(−6/2/6) | **−15.391796389/8/18** | [nao verificado] |
| intra_indenizada | BATIDA | P5(−2/1/2) | **−2/1/2** | [nao verificado] |
| extras_50 | CADASTRO | P5(−3.336169674/3/3) + P6(+0.380845000/1/2) | **−2.955324674/4/5** | [nao verificado] |
| extras_100 e extras_100_feriado | ESTRUTURA | P2(−7.052801863/1/1) + P3(+12.066969812/1/1) + P4(+3.958275278/1/1) | **+8.972443227/3/3**, cada rubrica | [nao verificado] |
| extras_100 | BATIDA | P5(+10.045843333/1/2) | **+10.045843333/1/2** | [nao verificado] |
| extras_100 | CADASTRO | P5(+10/1/1) | **+10/1/1** | [nao verificado] |
| extras_100_feriado | CADASTRO | P5(+16.043546944/1/1) | **+16.043546944/1/1** | [nao verificado] |
| extras_100_noturna | ESTRUTURA | P1(+1.138725830/2/2) + P2(+6.809822454/2/2) + P4(+1.485842467/2/2) | **+9.434390751/6/6** | [nao verificado] |
| extras_100_noturna | CADASTRO | P2(+1/1/1) + P3(+1/1/1) + P6(+1.861278889/1/1) | **+3.861278889/3/3** | [nao verificado] |
| atraso | ESTRUTURA | P2(+0.622824167/1/1) + P4(+0.373195000/1/1) | **+0.996019167/2/2** | [nao verificado] |
| atraso | CADASTRO | P4(+0.755333333/1/3) + P5(+0.076333333/1/1) | **+0.831666666/2/4** | [nao verificado] |
| saida_antecipada | CADASTRO | P4(−0.755333333/1/3) + P5(−0.076333333/1/1) | **−0.831666666/2/4** | [nao verificado] |
| saida_antecipada | ESTRUTURA | P6(+4.331833611/1/1) | **+4.331833611/1/1** | [nao verificado] |

Não foram publicadas linhas `DONO` para extras_50_noturna ou folga_trabalhada nas duas competências, nem para noturnas em novembro. [nao verificado] [nao verificado]

As contagens somam entre partes disjuntas, mas **colaboradores podem aparecer em mais de um dono da mesma rubrica**: outubro/intra soma 12 incidências por dono para 10 colaboradores da rubrica; outubro/noturnas, 9 para 6; novembro/intra, 9 para 8; novembro/extras_100, 5 para 4. [nao verificado]

Também há delta fora das linhas `DONO`, em `RESIDUO|ATE_LIMIAR`. Exemplo de outubro/P1/extras_100_noturna: `+2.063954502 − 0.002610278 = +2.061344224`. Outubro/P3 dessa rubrica apresenta `+0.014159722` com `0/0` divergentes; novembro/P5 apresenta `+0.002409167` com `0/0`. Esses valores permanecem na medida de impacto. [nao verificado]

Por parte, os registros `ERRO`, `INCOMPLETO`, retorno e linhas `SOMA` ficam assim. Nas contas de erro, **V = ValueError**, **C = ConservacaoNaoFecha**, **L = LacunaDeMedicao**; V aparece em `dia_pago.py:315`, enquanto C e L aparecem com origem `sonda`. São contagens de ocorrências reportadas, sem identificação de colaboradores únicos. [nao verificado]

| Parte | rc_sombra | Outubro: V + C + L | Novembro: V + C + L | INCOMPLETO: outubro + novembro | Linhas SOMA | Evidência |
|---|---:|---|---|---|---|---|
| P1 | 0 | 18 + 121 + 284 = **423** | 21 + 34 + 353 = **408** | 423 + 408 = **831** | 113 `False`; 0 `True` | [nao verificado] |
| P2 | 0 | 19 + 102 + 341 = **462** | 16 + 36 + 291 = **343** | 462 + 343 = **805** | 113 `False`; 0 `True` | [nao verificado] |
| P3 | 0 | 13 + 57 + 158 = **228** | 17 + 34 + 295 = **346** | 228 + 346 = **574** | 113 `False`; 0 `True` | [nao verificado] |
| P4 | 0 | 11 + 89 + 225 = **325** | 16 + 34 + 338 = **388** | 325 + 388 = **713** | 113 `False`; 0 `True` | [nao verificado] |
| P5 | 0 | 11 + 99 + 196 = **306** | 12 + 22 + 256 = **290** | 306 + 290 = **596** | 113 `False`; 0 `True` | [nao verificado] |
| P6 | 0 | 13 + 78 + 210 = **301** | 15 + 34 + 244 = **293** | 301 + 293 = **594** | 113 `False`; 0 `True` | [nao verificado] |
| Total | 0 nas seis partes | **85 + 546 + 1414 = 2045** | **97 + 194 + 1777 = 2068** | **2045 + 2068 = 4113** | **678 `False`; 0 `True`** | [nao verificado] |

A soma explícita dos `INCOMPLETO` é `831 + 805 + 574 + 713 + 596 + 594 = 4113`, também igual a `182 V + 740 C + 3191 L`. [nao verificado]

Cada parte publica, por competência, `1 COBERTURA + 11 rubricas × 5 famílias = 56` linhas `SOMA`. As cinco famílias são `PARTES_EMISSOR`, `DIVERGENCIA`, `COLABS_UNIAO_DIAS`, `V3_PORTA_EMISSOR` e `MOTOR_FECHAMENTO`. Portanto: `2 × 56 + 1 ERROS_ZERO = 113` por parte; `6 × 113 = 678` no conjunto. Todos os booleanos publicados são `False`. [nao verificado] [nao verificado]

O wrapper retorna o código da execução Docker e termina com `exit "$rc"`; isso explica por que o retorno do processo deve ser lido separadamente dos resultados publicados pela sonda. [bin/sonda_leitura.sh:241](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/bin/sonda_leitura.sh:241) [bin/sonda_leitura.sh:251](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/bin/sonda_leitura.sh:251)

O código do `script.py` específico não foi localizado na cópia; assim, não atribuo aos `False` um significado adicional de aprovação ou reprovação. [nao verificado]

As ocorrências **TRATADA_LOCAL** estão publicadas para a parte inteira, sem separação por competência: [nao verificado] [nao verificado]

| Sítio / tipo | P1 | P2 | P3 | P4 | P5 | P6 | Soma | Evidência |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| leitor_celula.py:25 / ValueError | 2552 | 2962 | 2290 | 2228 | 3014 | 1881 | **14927** | [nao verificado] |
| juiz_batida.py:75 / ValueError | 154382 | 172335 | 162681 | 145226 | 154656 | 146215 | **935495** | [nao verificado] |
| motor_calculo_v2.py:1075 / AttributeError | 3483 | 3816 | 3625 | 3602 | 4016 | 3819 | **22361** | [nao verificado] |
| fechamento.py:1029 / _GradeSemCelula | 5 | 2 | 4 | não publicada | não publicada | 4 | **15 reportadas** | [nao verificado] | [nao verificado]

P4 e P5 não publicam a linha `_GradeSemCelula`; essa ausência não comprova zero. As contagens locais não foram acrescentadas aos `INCOMPLETO`. [nao verificado]

O confronto **UNIVERSO_PARTE × UNIVERSO** é: [nao verificado]

| Competência | Conta P1 + P2 + P3 + P4 + P5 + P6 | UNIVERSO informado | Diferença | Evidência |
|---|---|---:|---:|---|
| 10/2026 | 110 + 109 + 102 + 96 + 107 + 100 = **624** | 624 | **0** | [nao verificado] |
| 11/2026 | 96 + 99 + 97 + 91 + 99 + 96 = **578** | 578 | **0** | [nao verificado] |

Essa igualdade numérica coexiste com `SOMA COBERTURA=False` em todas as partes; não elimina os `INCOMPLETO`. [nao verificado]

Quanto ao **`!`**, as saídas **não trazem a lista fechada exigida para a troca**, nem o estado de exportação por empresa/competência, nem valores monetários. Logo, não permitem identificar quais divergências desta medida moveriam dinheiro em competência exportada. [nao verificado] A lista fechada — “resto dos 38, ! do v2, ESTRUTURA do v3” — consta como entrega da obra no backlog. [app/docs/BACKLOG.md:1750](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/docs/BACKLOG.md:1750)

O código da cópia permite nomear estes pontos, sem atribuir-lhes alcance atual de produção:

- **Alterar o gravado de competência exportada** ou **editar `motor_calculo_v2.py`** exige `!` pela regra local. [CLAUDE.md:262](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/CLAUDE.md:262)
- O calculador depende do v2 para funções/classes/fator noturno, métodos de pontualidade e `he_noturna`. A dependência existe; eventual alteração no arquivo v2 está sujeita à regra acima. [app/ponto/calculador/regras.py:35](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/calculador/regras.py:35) [app/ponto/calculador/regras.py:112](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/calculador/regras.py:112) [app/ponto/calculador/regras.py:825](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/calculador/regras.py:825)
- A cópia documenta a **reancoragem da janela** e a **remoção da cópia de par na origem v2** como correções destinadas ao `!`. Seus números históricos não foram incorporados à presente medida. [app/ponto/calculador/periodo.py:659](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/calculador/periodo.py:659) [app/ponto/calculador/periodo.py:726](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/calculador/periodo.py:726)

`horas_falta`, `horas_reflexo_dsr` e `saldo_banco_horas` estão expressamente fora da medida nos seis anexos; seu impacto permanece indisponível. [nao verificado] Também não cabe somar as rubricas apresentadas como um total financeiro: o código declara que as variantes noturna e de feriado são subconjuntos das extras totais. [app/ponto/services/dia_pago.py:902](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/services/dia_pago.py:902)

## 06/11 13:35 · sessao 95d3b2f91445 · R1-CAUSAD-V3 · leitura · origem=Code

O consolidado resulta em **303 dias divergentes em 10/2026** e **201 em 11/2026**, com BATE de **95,950821862889%** e **94,820922442669%**, respectivamente. [nao verificado]

Todas as contas abaixo derivam exclusivamente do texto fornecido, sem arquivo:linha disponível. Os termos das somas seguem a ordem **P1 + P2 + P3 + P4 + P5 + P6**; as horas usam os decimais de `PARCIAL` e `PARCIAL_TOTAL`, sem arredondamento intermediário. [nao verificado]

Para **10/2026**, a soma de cada chave de `[1]` é: [nao verificado]

| Chave | Conta por parte = total | Referência |
|---|---|---|
| dias_comparados | 1175 + 1267 + 1232 + 1235 + 1282 + 1292 = **7483** | [nao verificado] |
| bate_ate_10min | 1136 + 1226 + 1165 + 1187 + 1213 + 1253 = **7180** | [nao verificado] |
| esp_zero_e6_trabalho | 1 + 2 + 12 + 6 + 9 + 5 = **35** | [nao verificado] |
| e6_zero_esp_trabalho | 3 + 4 + 6 + 1 + 3 + 2 = **19** | [nao verificado] |
| diverge_10_60 | 11 + 7 + 2 + 5 + 2 + 1 = **28** | [nao verificado] |
| diverge_acima_60 | 24 + 28 + 47 + 36 + 55 + 31 = **221** | [nao verificado] |
| dia_batida_impar | 32 + 64 + 66 + 56 + 76 + 53 = **347** | [nao verificado] |
| impar_que_BATE | 15 + 34 + 24 + 22 + 28 + 25 = **148** | [nao verificado] |
| impar_que_DIVERGE | 17 + 30 + 42 + 34 + 48 + 28 = **199** | [nao verificado] |
| impar_dentro_da_faixa | 14 + 24 + 33 + 30 + 42 + 25 = **168** | [nao verificado] |
| impar_FORA_da_faixa | 3 + 6 + 9 + 4 + 6 + 3 = **31** | [nao verificado] |
| impar_no_conjunto_do_dia | 17 + 30 + 42 + 33 + 48 + 28 = **198** | [nao verificado] |
| impar_SO_POR_CORTE_do_oraculo | 0 + 0 + 0 + 1 + 0 + 0 = **1** | [nao verificado] |
| dia_sem_linha_no_espelho | 0 + 0 + 0 + 0 + 0 + 0 = **0** | [nao verificado] |
| dia_sem_lavratura_no_espelho | 1719 + 1568 + 1422 + 1285 + 1539 + 1396 = **8929** | [nao verificado] |
| dia_isento_ou_ausencia | 329 + 349 + 345 + 305 + 271 + 279 = **1878** | [nao verificado] |
| dia_sem_trabalho_ambos | 34 + 24 + 22 + 18 + 45 + 12 = **155** | [nao verificado] |
| esp_desconta_janela_declarada | 43 + 62 + 39 + 37 + 73 + 21 = **275** | [nao verificado] |
| esp_lido_pelo_pago_h | 0 + 0 + 0 + 0 + 0 + 0 = **0** | [nao verificado] |

Para **11/2026**, a soma de cada chave de `[1]` é: [nao verificado]

| Chave | Conta por parte = total | Referência |
|---|---|---|
| dias_comparados | 603 + 675 + 629 + 610 + 690 + 674 = **3881** | [nao verificado] |
| bate_ate_10min | 568 + 641 + 599 + 596 + 629 + 647 = **3680** | [nao verificado] |
| esp_zero_e6_trabalho | 11 + 3 + 4 + 4 + 4 + 2 = **28** | [nao verificado] |
| e6_zero_esp_trabalho | 5 + 0 + 5 + 0 + 2 + 2 = **14** | [nao verificado] |
| diverge_10_60 | 1 + 2 + 0 + 0 + 6 + 1 = **10** | [nao verificado] |
| diverge_acima_60 | 18 + 29 + 21 + 10 + 49 + 22 = **149** | [nao verificado] |
| dia_batida_impar | 34 + 52 + 42 + 32 + 63 + 39 = **262** | [nao verificado] |
| impar_que_BATE | 14 + 24 + 22 + 22 + 23 + 18 = **123** | [nao verificado] |
| impar_que_DIVERGE | 20 + 28 + 20 + 10 + 40 + 21 = **139** | [nao verificado] |
| impar_dentro_da_faixa | 13 + 27 + 17 + 9 + 32 + 16 = **114** | [nao verificado] |
| impar_FORA_da_faixa | 7 + 1 + 3 + 1 + 8 + 5 = **25** | [nao verificado] |
| impar_no_conjunto_do_dia | 20 + 28 + 19 + 10 + 40 + 20 = **137** | [nao verificado] |
| impar_SO_POR_CORTE_do_oraculo | 0 + 0 + 1 + 0 + 0 + 1 = **2** | [nao verificado] |
| dia_sem_linha_no_espelho | 1344 + 1386 + 1358 + 1274 + 1386 + 1344 = **8092** | [nao verificado] |
| dia_sem_lavratura_no_espelho | 805 + 834 + 814 + 756 + 785 + 767 = **4761** | [nao verificado] |
| dia_isento_ou_ausencia | 174 + 120 + 147 + 148 + 130 + 155 = **874** | [nao verificado] |
| dia_sem_trabalho_ambos | 33 + 17 + 21 + 19 + 24 + 20 = **134** | [nao verificado] |
| esp_desconta_janela_declarada | 17 + 37 + 38 + 14 + 54 + 16 = **176** | [nao verificado] |
| esp_lido_pelo_pago_h | 0 + 0 + 0 + 0 + 0 + 0 = **0** | [nao verificado] |

As contas por dono de **10/2026**, incluindo TOTAL, são: [nao verificado]

| Dono | Medida | Conta por parte = total | Referência |
|---|---|---|---|
| ESTRUTURA | dia-colab | 10 + 5 + 3 + 3 + 3 + 2 = **26** | [nao verificado] |
| ESTRUTURA | horas | 9.116283817777775 + 11.084600277777778 + 15.209666666666665 + 15.566319135833336 + 29.23270916666667 + 8.15 = **88.359579064722224** | [nao verificado] |
| ESTRUTURA | colabs | 3 + 3 + 3 + 3 + 3 + 1 = **16** | [nao verificado] |
| CADASTRO | dia-colab | 10 + 8 + 7 + 23 + 21 + 9 = **78** | [nao verificado] |
| CADASTRO | horas | 38.355213611111104 + 20.397520712222224 + 35.88209174 + 107.60888681222221 + 60.14375256333333 + 44.14612722222221 = **306.533592661111078** | [nao verificado] |
| CADASTRO | colabs | 3 + 6 + 4 + 8 + 8 + 3 = **32** | [nao verificado] |
| BATIDA | dia-colab | 19 + 28 + 57 + 22 + 45 + 28 = **199** | [nao verificado] |
| BATIDA | horas | 108.81011656222222 + 136.0465618775 + 302.3993460441667 + 101.15787729194444 + 190.17304430305552 + 122.57225449861109 = **961.15920057749997** | [nao verificado] |
| BATIDA | colabs | 12 + 18 + 25 + 17 + 25 + 16 = **113** | [nao verificado] |
| TOTAL | dia-colab | 39 + 41 + 67 + 48 + 69 + 39 = **303** | [nao verificado] |
| TOTAL | horas | 156.281613991111099 + 167.528682867500002 + 353.491104450833365 + 224.333083239999986 + 279.54950603305552 + 174.86838172083330 = **1356.052372303333272** | [nao verificado] |
| TOTAL | colabs | 16 + 23 + 30 + 26 + 29 + 20 = **144** | [nao verificado] |

As contas por dono de **11/2026**, incluindo TOTAL, são: [nao verificado]

| Dono | Medida | Conta por parte = total | Referência |
|---|---|---|---|
| ESTRUTURA | dia-colab | 1 + 0 + 3 + 0 + 4 + 1 = **9** | [nao verificado] |
| ESTRUTURA | horas | 6.2 + 0.0 + 11.75 + 0.0 + 20.109676988055558 + 2.5604991666666668 = **40.6201761547222248** | [nao verificado] |
| ESTRUTURA | colabs | 1 + 0 + 2 + 0 + 3 + 1 = **7** | [nao verificado] |
| CADASTRO | dia-colab | 20 + 10 + 5 + 5 + 27 + 5 = **72** | [nao verificado] |
| CADASTRO | horas | 106.69305642305555 + 28.19539852888889 + 15.826795833333335 + 39.726189220555554 + 106.31719708361109 + 50.7295725 = **347.488209589444419** | [nao verificado] |
| CADASTRO | colabs | 7 + 7 + 5 + 3 + 7 + 4 = **33** | [nao verificado] |
| BATIDA | dia-colab | 14 + 24 + 22 + 9 + 30 + 21 = **120** | [nao verificado] |
| BATIDA | horas | 81.76095535388889 + 122.76838809305558 + 113.35641307083331 + 50.69357468888889 + 164.14298169666668 + 110.33564643499999 = **643.05795933833334** | [nao verificado] |
| BATIDA | colabs | 10 + 16 + 10 + 8 + 19 + 15 = **78** | [nao verificado] |
| TOTAL | dia-colab | 35 + 34 + 30 + 14 + 61 + 27 = **201** | [nao verificado] |
| TOTAL | horas | 194.65401177694444 + 150.96378662194447 + 140.933208904166645 + 90.419763909444444 + 290.569855768333328 + 163.6257181016666568 = **1031.1663450824999838** | [nao verificado] |
| TOTAL | colabs | 15 + 21 + 15 + 10 + 26 + 18 = **105** | [nao verificado] |

Para os colabs de TOTAL, somei as uniões informadas por parte, considerando a disjunção entre partes declarada no pedido. [nao verificado]

As conferências agregadas e os percentuais são: [nao verificado]

| Conferência | 10/2026 | 11/2026 | Referência |
|---|---|---|---|
| UNIVERSO_PARTE × UNIVERSO | 110 + 109 + 102 + 96 + 107 + 100 = **624**, igual a 624 | 96 + 99 + 97 + 91 + 99 + 96 = **578**, igual a 578 | [nao verificado] |
| BATE % | 7180 ÷ 7483 × 100 ≈ **95,950821862889%** | 3680 ÷ 3881 × 100 ≈ **94,820922442669%** | [nao verificado] |
| Comparados − BATE | 7483 − 7180 = **303** | 3881 − 3680 = **201** | [nao verificado] |
| Categorias divergentes de `[1]` | 35 + 19 + 28 + 221 = **303** | 28 + 14 + 10 + 149 = **201** | [nao verificado] |
| Dias dos donos | 26 + 78 + 199 = **303** | 9 + 72 + 120 = **201** | [nao verificado] |
| Horas dos donos | 88.359579064722224 + 306.533592661111078 + 961.15920057749997 = **1356.052372303333272** | 40.6201761547222248 + 347.488209589444419 + 643.05795933833334 = **1031.1663450824999838** | [nao verificado] |

Os diagnósticos finais aparecem por execução de parte, abrangendo as duas competências, sem separação mensal de `TRATADA_LOCAL`. A coluna “atravessaram frame” abaixo conta somente linhas explícitas desse diagnóstico exibidas nas saídas; não comprova ocorrências fora do texto fornecido. [nao verificado]

| Parte | rc_sombra | Marcador INCOMPLETO na saída | Linhas TRATADA_LOCAL | Ocorrências somadas de TRATADA_LOCAL | Linhas explícitas de erros que atravessaram frame | Referência |
|---|---:|---|---:|---:|---:|---|
| P1 | 0 | Ausente | 27 | 777169 | 0 | [nao verificado] |
| P2 | 0 | Ausente | 27 | 835848 | 0 | [nao verificado] |
| P3 | 0 | Ausente | 28 | 813047 | 0 | [nao verificado] |
| P4 | 0 | Ausente | 27 | 752749 | 0 | [nao verificado] |
| P5 | 0 | Ausente | 26 | 869997 | 0 | [nao verificado] |
| P6 | 0 | Ausente | 26 | 746948 | 0 | [nao verificado] |

As contas das ocorrências `TRATADA_LOCAL`, na ordem em que aparecem em cada saída, são: [nao verificado]

- P1: 232873 + 214 + 945 + 5201 + 57 + 2 + 5766 + 3 + 4 + 3444 + 3444 + 649 + 180 + 433065 + 505 + 2567 + 161 + 10520 + 104 + 3607 + 191 + 5 + 20459 + 4354 + 33979 + 8 + 14862 = **777169**. [nao verificado]
- P2: 249865 + 13 + 1341 + 5749 + 66 + 4 + 8 + 6433 + 2 + 6 + 4777 + 4777 + 498 + 185 + 459090 + 579 + 2775 + 192 + 11736 + 97 + 3584 + 197 + 2 + 23442 + 5013 + 37362 + 18055 = **835848**. [nao verificado]
- P3: 240100 + 250 + 1365 + 4623 + 41 + 4 + 2 + 6434 + 4 + 6 + 4324 + 4324 + 512 + 179 + 16 + 452220 + 554 + 2847 + 157 + 11514 + 101 + 3979 + 190 + 4 + 22023 + 4399 + 36499 + 16376 = **813047**. [nao verificado]
- P4: 220720 + 551 + 1090 + 4668 + 49 + 1 + 34 + 6712 + 8 + 3 + 3792 + 3792 + 284 + 168 + 1126 + 409650 + 558 + 2635 + 165 + 11428 + 89 + 3666 + 177 + 23759 + 3165 + 37032 + 17427 = **752749**. [nao verificado]
- P5: 257916 + 434 + 1676 + 6042 + 79 + 2 + 20 + 7771 + 6 + 5601 + 5601 + 310 + 190 + 471429 + 788 + 3044 + 194 + 12714 + 143 + 4139 + 195 + 27833 + 3152 + 41156 + 2 + 19560 = **869997**. [nao verificado]
- P6: 219208 + 16 + 1158 + 3928 + 56 + 2 + 8 + 6308 + 6 + 3771 + 3771 + 520 + 177 + 414465 + 470 + 2680 + 185 + 11236 + 73 + 3903 + 186 + 4 + 20152 + 3840 + 35418 + 15407 = **746948**. [nao verificado]

Total de linhas `TRATADA_LOCAL`: 27 + 27 + 28 + 27 + 26 + 26 = **161**; total de ocorrências: 777169 + 835848 + 813047 + 752749 + 869997 + 746948 = **4795758**. [nao verificado]

As linhas `SOMA` fornecidas ficam transcritas em colunas abaixo; cada célula preserva o valor e o booleano da respectiva linha. [nao verificado]

| Parte | Competência | SOMA dias | SOMA HORAS | SOMA COLABS_UNIAO | Referência |
|---|---|---|---|---|---|
| P1 | 10/2026 | 39 — True | 156.281613991111099 — True | 16 — True | [nao verificado] |
| P1 | 11/2026 | 35 — True | 194.65401177694444 — True | 15 — True | [nao verificado] |
| P2 | 10/2026 | 41 — True | 167.528682867500002 — True | 23 — True | [nao verificado] |
| P2 | 11/2026 | 34 — True | 150.96378662194447 — True | 21 — True | [nao verificado] |
| P3 | 10/2026 | 67 — True | 353.491104450833365 — True | 30 — True | [nao verificado] |
| P3 | 11/2026 | 30 — True | 140.933208904166645 — True | 15 — True | [nao verificado] |
| P4 | 10/2026 | 48 — True | 224.333083239999986 — True | 26 — True | [nao verificado] |
| P4 | 11/2026 | 14 — True | 90.419763909444444 — True | 10 — True | [nao verificado] |
| P5 | 10/2026 | 69 — True | 279.54950603305552 — True | 29 — True | [nao verificado] |
| P5 | 11/2026 | 61 — True | 290.569855768333328 — True | 26 — True | [nao verificado] |
| P6 | 10/2026 | 39 — True | 174.86838172083330 — True | 20 — True | [nao verificado] |
| P6 | 11/2026 | 27 — True | 163.6257181016666568 — True | 18 — True | [nao verificado] |

Contagem de `SOMA True`: 6 + 6 + 6 + 6 + 6 + 6 = **36**; de `SOMA False`: 0 + 0 + 0 + 0 + 0 + 0 = **0**. [nao verificado]

Não há linhas de **listas de dias** nem itens **ESPELHO-VERDADE** nos seis blocos de saída fornecidos; portanto, não há conteúdo dessas categorias para transcrever ou agregar. [nao verificado]

A tabela final por competência é: [nao verificado]

| Medida | 10/2026 | 11/2026 | Referência |
|---|---:|---:|---|
| Janela informada | 2026-09-21 a 2026-10-20 | 2026-10-21 a 2026-11-20 | [nao verificado] |
| UNIVERSO_PARTE somado / UNIVERSO | 624 / 624 — confere | 578 / 578 — confere | [nao verificado] |
| Dias comparados | 7483 | 3881 | [nao verificado] |
| Bate até 10 min | 7180 | 3680 | [nao verificado] |
| BATE % | 95,950821862889% | 94,820922442669% | [nao verificado] |
| ESTRUTURA — dia-colab / horas / colabs | 26 / 88.359579064722224 / 16 | 9 / 40.6201761547222248 / 7 | [nao verificado] |
| CADASTRO — dia-colab / horas / colabs | 78 / 306.533592661111078 / 32 | 72 / 347.488209589444419 / 33 | [nao verificado] |
| BATIDA — dia-colab / horas / colabs | 199 / 961.15920057749997 / 113 | 120 / 643.05795933833334 / 78 | [nao verificado] |
| **TOTAL — dia-colab / horas / colabs** | **303 / 1356.052372303333272 / 144** | **201 / 1031.1663450824999838 / 105** | [nao verificado] |
| SOMA, por competência | 3 + 3 + 3 + 3 + 3 + 3 = 18 True; nenhum False | 3 + 3 + 3 + 3 + 3 + 3 = 18 True; nenhum False | [nao verificado] |

## 06/11 13:34 · sessao 8ee384f1b5ff · - · - · origem=Ronald (tmux)

Todas as contas abaixo são derivadas dos anexos, em memória, com `Decimal` dos valores textuais de `PARCIAL`, sem arredondamento intermediário. `[nao verificado]`

Ordem dos vetores: **P1, P2, P3, P4, P5, P6**. `[nao verificado]`

| Competência/rubrica | Deltas integrais por parte | Total integral | Arredondamento somente final, 9 casas |
|---|---|---:|---:|
| 10 trabalhadas | −2.542890555555; **−2.842305278***; −3.035321666668; 0; −9.540302056388; −4.528065277778 | **−22.488884834389*** | −22.488884834* |
| 10 intra | −2; 0; −4.05666694444443; −0.63527333333331; −8.16666666666663; −8 | −22.85860694444437 | −22.858606944 |
| 10 HE50 | 0; 0; 0; −0.502948333333335; +1.4735158538888901; +0.224154212500004 | +1.1947217330555591 | +1.194721733 |
| 10 HE100 | 0; +10.985971388888896; +12.149131944444441; 0; 0; 0 | +23.135103333333337 | +23.135103333 |
| 10 HE100 feriado | mesmo vetor de HE100 | +23.135103333333337 | +23.135103333 |
| 10 HE100 noturna | +2.061344224166667; +7.021076119444445; +0.014159722222222; +1.4042363383333330; +0.9115786111111086; +1 | +12.4123950152777756 | +12.412395015 |
| 10 noturnas | 0; −2.7245478571431; 0; +2.5716699836512; +0.5618157142858; +2.1645482921431 | +2.5734861329370 | +2.573486133 |
| 10 atraso | +2.368802113333331; +4.579204284166672; +1.106468535277777; +0.186039155000003; +2.329948153055554; −0.000374091944443 | +10.570088148888894 | +10.570088149 |
| 10 saída | +0.000684534722225; +0.000106703055551; −0.557056842500004; −0.186263274722222; +0.000085790277780; +0.000069985555555 | −0.742373103611115 | −0.742373104 |
| 11 trabalhadas | −0.0000000000009; −2.8425216666665; −3.9930194444441; 0; −17.0800343119445; 0 | −23.9155754230560 | −23.915575423 |
| 11 intra | −1.16666666666666; 0; −1; +0.40334305555555; −9.62847277777777; −6 | −17.39179638888888 | −17.391796389 |
| 11 HE50 | 0; 0; 0; 0; −3.3361696744444433; +0.380845000000001 | −2.9553246744444423 | −2.955324674 |
| 11 HE100 | +0.000000000000015; −7.052801863055549; +12.066969811944446; +3.958275277777773; +20.045843333333323; 0 | +29.018286560000008 | +29.018286560 |
| 11 HE100 feriado | +0.000000000000015; −7.052801863055549; +12.066969811944446; +3.958275277777773; +16.043546944444436; 0 | +25.015990171111121 | +25.015990171 |
| 11 HE100 noturna | +1.137680829722223; +7.819367573333334; +1; +1.4999583002777781; +0.002409166666664; +1.8612788888888885 | +13.3206947588888876 | +13.320694759 |
| 11 atraso | −0.000158523888888; +0.622596846944447; +0.000051118333336; +1.128749770277779; +0.076214444444446; −0.000105974166669 | +1.827347681944451 | +1.827347682 |
| 11 saída | −0.000158888888890; +0.000201111111110; +0.000008888888885; −0.755222120833338; −0.076319288888886; +4.331691891666661 | +3.500201593055542 | +3.500201593 |

Fonte de todas as linhas da tabela: anexos `PARCIAL` fornecidos. `[nao verificado]`

\* **Outubro/trabalhadas tem precisão mista:** o `v2_h` do `PARCIAL` P2 foi raspado como `11749.[cpf]`; utilizei exclusivamente o delta explícito `RUBRICA`, −2.842305278. O total de precisão integral das seis partes é indisponível; −22.488884834389 é apenas o total misto declarado. `[nao verificado]`

As rubricas `horas_extras_50_noturna` e `horas_folga_trabalhada` têm delta integral zero nas duas competências; `horas_noturnas` tem delta integral zero em novembro. `[nao verificado]`

Comparação dos totais finais em 9 casas com a soma dos `delta_h` já emitidos por `RUBRICA`: `[nao verificado]`

| Competência/rubrica | Soma de `RUBRICA` | Soma `PARCIAL`, arredondada só ao final |
|---|---:|---:|
| 10 trabalhadas | −22.488884835 | −22.488884834* |
| 10 HE50 | +1.194721734 | +1.194721733 |
| 10 HE100 noturna | +12.412395014 | +12.412395015 |
| 10 atraso | +10.570088148 | +10.570088149 |
| 11 atraso | +1.827347681 | +1.827347682 |

Fonte: contas em memória dos anexos; demais rubricas coincidem em nove casas entre as duas formas. `[nao verificado]`

## 06/11 13:33 · sessao 6fec615fd82c · - · - · origem=Ronald (tmux)

Consolidação dos anexos, sem verificação independente. [nao verificado]

| Item | P1 | P2 | P3 | P4 | P5 | P6 | Total |
|---|---:|---:|---:|---:|---:|---:|---:|
| Erros outubro | 423 | 462 | 228 | 325 | 306 | 301 | 2045 |
| Erros novembro | 408 | 343 | 346 | 388 | 290 | 293 | 2068 |
| INCOMPLETO | 831 | 805 | 574 | 713 | 596 | 594 | 4113 |
| TRATADA_LOCAL leitor | 2552 | 2962 | 2290 | 2228 | 3014 | 1881 | 14927 |
| TRATADA_LOCAL juiz | 154382 | 172335 | 162681 | 145226 | 154656 | 146215 | 935495 |
| TRATADA_LOCAL motor | 3483 | 3816 | 3625 | 3602 | 4016 | 3819 | 22361 |
| TRATADA_LOCAL fechamento | 5 | 2 | 4 | ausente | ausente | 4 | 15 reportados |
| Linhas SOMA | 113 | 113 | 113 | 113 | 113 | 113 | 678 |

Toda a tabela deriva dos anexos. Ausência da linha de fechamento em P4/P5 não comprova zero. As 678 linhas `SOMA` exibem `False`; nenhuma exibe `True`. Cada parte contém `2 × (1 COBERTURA + 11 rubricas × 5 famílias) + 1 ERROS_ZERO = 113` linhas. [nao verificado]

Erros por competência: outubro `85 ValueError + 546 ConservacaoNaoFecha + 1414 LacunaDeMedicao = 2045`; novembro `97 + 194 + 1777 = 2068`; total `182 + 740 + 3191 = 4113`, igual à soma dos `INCOMPLETO`. [nao verificado]

Todos os vetores DONO, deltas exatos e contagens por parte foram enviados nas mensagens anteriores. Há sobreposição de colaboradores entre donos: outubro intra `12 incidências DONO / 10 colaboradores RUBRICA`; outubro noturnas `9/6`; novembro intra `9/8`; novembro extras100 `5/4`. Não somar esses donos para obter colaboradores únicos da rubrica. [nao verificado]

## 06/11 13:32 · sessao f98b37141a96 · - · - · origem=Ronald (tmux)

Achados de código, sem banco/log e sem escrita:

- **DIFF é impacto, não validação**; a regra exige dono L-099 para divergências. [CLAUDE.md:49](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/CLAUDE.md:49) [CLAUDE.md:52](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/CLAUDE.md:52)
- `rc=0` é retorno da execução Docker; o wrapper não avalia as linhas `SOMA` nem `INCOMPLETO`. O pipeline captura o retorno e termina com `exit "$rc"`. [bin/sonda_leitura.sh:241](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/bin/sonda_leitura.sh:241) [bin/sonda_leitura.sh:251](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/bin/sonda_leitura.sh:251)
- Não localizei na cópia o `script.py` específico nem os emissores de `SOMA COBERTURA`, `SOMA PARTES_EMISSOR`, `SOMA ERROS_ZERO`, `TRATADA_LOCAL` ou `ERROS_POR_TIPO`; portanto o significado booleano de `False` além do literal fornecido permanece `[nao verificado]`.
- O backlog registra expressamente a entrega pedida: DIFF 10/11 por rubrica/dono e “lista fechada do que a troca exige (resto dos 38, ! do v2, ESTRUTURA do v3)”; ali está em curso, sem lista preenchida. [app/docs/BACKLOG.md:1750](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/docs/BACKLOG.md:1750)

Os sítios de erro fornecidos correspondem a:

| Sítio | Comportamento no código |
|---|---|
| `dia_pago.py:315` | Levanta `ValueError` quando falta dono declarado de campo do dia. [app/ponto/services/dia_pago.py:310](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/services/dia_pago.py:310) |
| `leitor_celula.py:25` | Faz parse `HH:MM`; captura `Exception` e retorna `None`. [app/escala/services/leitor_celula.py:23](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/escala/services/leitor_celula.py:23) |
| `juiz_batida.py:75` | Faz parse `HH:MM`; captura `ValueError/AttributeError` e retorna `None`. [app/ponto/juiz_batida.py:72](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/juiz_batida.py:72) |
| `motor_calculo_v2.py:1075` | Atribui mapa `dia_da_jornada`; captura `Exception` e faz `pass`. [app/ponto/motor_calculo_v2.py:1068](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/motor_calculo_v2.py:1068) |
| `fechamento.py:1029` | Levanta `_GradeSemCelula` se todos os dias dentro do vínculo são `sem_celula`. [app/ponto/services/fechamento.py:1027](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/services/fechamento.py:1027) |

Para a lista `!`:

- Tocar o **gravado de competência exportada** e editar `motor_calculo_v2.py` exige `!` pela regra local. [CLAUDE.md:262](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/CLAUDE.md:262)
- As saídas não informam quais empresas/competências estão exportadas; não permitem atribuir `!` financeiro a colaboradores específicos. `[nao verificado]` O código pergunta isso por empresa e competência, via `empresas_exportadas_no_escopo`. [app/ponto/services/fechamento.py:1480](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/services/fechamento.py:1480)
- Há dependência concreta de v2: funções/classes/fator noturno importados, métodos de pontualidade emprestados, constantes de tolerância e função `he_noturna`. [app/ponto/calculador/regras.py:35](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/calculador/regras.py:35) [app/ponto/calculador/regras.py:112](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/calculador/regras.py:112) [app/ponto/calculador/periodo.py:163](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/calculador/periodo.py:163) [app/ponto/calculador/regras.py:825](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/calculador/regras.py:825)
- O releitor também recebe períodos do resultado atual, nos campos `periodos/periodos_ft`; a porta recusa campo ausente. [app/ponto/calculador/periodo.py:609](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/calculador/periodo.py:609) [app/ponto/calculador/nucleo.py:393](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/calculador/nucleo.py:393)
- O código documenta pendências v2 de reancoragem de janela e remoção de par duplicado na origem, explicitamente destinadas ao `!`; seus números históricos não devem ser trazidos como medida atual. [app/ponto/calculador/periodo.py:659](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/calculador/periodo.py:659) [app/ponto/calculador/periodo.py:726](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/calculador/periodo.py:726)
- Não somar rubricas como um total de dinheiro: extras noturnas/feriado são subconjuntos das extras totais; somá-las ao total duplica horas. [app/ponto/services/dia_pago.py:902](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/services/dia_pago.py:902)

## 06/11 13:23 · sessao c1ab63920e77 · R1-CAUSAD-V3 · sonda · origem=Code

Saída integral fornecida da parte 1 de 6: [nao verificado]

```text
sonda_leitura: script.py [27c01d2784902123] no cpuset 4-7, lendo sombra como leitor (LEITURA)

PARTE 1 PARTES 6
[0] UNIVERSO 10/2026 624
UNIVERSO_PARTE 10/2026 110
JANELA 10/2026 2026-09-21 2026-10-20
[0] UNIVERSO 11/2026 578
UNIVERSO_PARTE 11/2026 96
JANELA 11/2026 2026-10-21 2026-11-20
ESTIMATIVA processados 25 universo 206 minutos_total 2.7 ate_40_min True ate_15_min True
ESTIMATIVA processados 50 universo 206 minutos_total 3.0 ate_40_min True ate_15_min True
ESTIMATIVA processados 75 universo 206 minutos_total 2.9 ate_40_min True ate_15_min True
ESTIMATIVA processados 100 universo 206 minutos_total 2.7 ate_40_min True ate_15_min True
COMPETENCIA 10/2026
[1] {'dias_comparados': 1175, 'bate_ate_10min': 1136, 'esp_zero_e6_trabalho': 1, 'e6_zero_esp_trabalho': 3, 'diverge_10_60': 11, 'diverge_acima_60': 24, 'dia_batida_impar': 32, 'impar_que_BATE': 15, 'impar_que_DIVERGE': 17, 'impar_dentro_da_faixa': 14, 'impar_FORA_da_faixa': 3, 'impar_no_conjunto_do_dia': 17, 'impar_SO_POR_CORTE_do_oraculo': 0, 'dia_sem_linha_no_espelho': 0, 'dia_sem_lavratura_no_espelho': 1719, 'dia_isento_ou_ausencia': 329, 'dia_sem_trabalho_ambos': 34, 'esp_desconta_janela_declarada': 43, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 96.7% de 1175 dias comparados
[3] ESTRUTURA    10 dia-colab       9.1 h    3 colabs
[3] CADASTRO     10 dia-colab      38.4 h    3 colabs
[3] BATIDA       19 dia-colab     108.8 h   12 colabs
[4] TOTAL divergentes 39
[5] colabs com divergencia: 16
erros_no_espelho 0
PARCIAL|10/2026|ESTRUTURA|dias=10|horas=9.116283817777775|colabs=3
PARCIAL|10/2026|CADASTRO|dias=10|horas=38.355213611111104|colabs=3
PARCIAL|10/2026|BATIDA|dias=19|horas=108.81011656222222|colabs=12
PARCIAL_TOTAL|10/2026|dias=39|horas=156.281613991111099|colabs=16
ESTIMATIVA processados 125 universo 206 minutos_total 2.8 ate_40_min True ate_15_min True
ESTIMATIVA processados 150 universo 206 minutos_total 2.9 ate_40_min True ate_15_min True
ESTIMATIVA processados 175 universo 206 minutos_total 3.0 ate_40_min True ate_15_min True
ESTIMATIVA processados 200 universo 206 minutos_total 3.0 ate_40_min True ate_15_min True
COMPETENCIA 11/2026
[1] {'dias_comparados': 603, 'bate_ate_10min': 568, 'esp_zero_e6_trabalho': 11, 'e6_zero_esp_trabalho': 5, 'diverge_10_60': 1, 'diverge_acima_60': 18, 'dia_batida_impar': 34, 'impar_que_BATE': 14, 'impar_que_DIVERGE': 20, 'impar_dentro_da_faixa': 13, 'impar_FORA_da_faixa': 7, 'impar_no_conjunto_do_dia': 20, 'impar_SO_POR_CORTE_do_oraculo': 0, 'dia_sem_linha_no_espelho': 1344, 'dia_sem_lavratura_no_espelho': 805, 'dia_isento_ou_ausencia': 174, 'dia_sem_trabalho_ambos': 33, 'esp_desconta_janela_declarada': 17, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 94.2% de 603 dias comparados
[3] ESTRUTURA     1 dia-colab       6.2 h    1 colabs
[3] CADASTRO     20 dia-colab     106.7 h    7 colabs
[3] BATIDA       14 dia-colab      81.8 h   10 colabs
[4] TOTAL divergentes 35
[5] colabs com divergencia: 15
erros_no_espelho 0
PARCIAL|11/2026|ESTRUTURA|dias=1|horas=6.2|colabs=1
PARCIAL|11/2026|CADASTRO|dias=20|horas=106.69305642305555|colabs=7
PARCIAL|11/2026|BATIDA|dias=14|horas=81.76095535388889|colabs=10
PARCIAL_TOTAL|11/2026|dias=35|horas=194.65401177694444|colabs=15
TEMPO_SEGUNDOS 178.0
TRATADA_LOCAL app/escala/alimentacao.py:279 GeneratorExit 232873
TRATADA_LOCAL app/escala/models.py:1282 GeneratorExit 214
TRATADA_LOCAL app/escala/services/fase_12x36.py:114 GeneratorExit 945
TRATADA_LOCAL app/escala/services/leitor_celula.py:25 ValueError 5201
TRATADA_LOCAL app/escala/services/leitor_celula.py:81 GeneratorExit 57
TRATADA_LOCAL app/escala/utils.py:1521 GeneratorExit 2
TRATADA_LOCAL app/escala/utils.py:2012 GeneratorExit 5766
TRATADA_LOCAL app/escala/utils.py:2054 GeneratorExit 3
TRATADA_LOCAL app/escala/utils.py:452 GeneratorExit 4
TRATADA_LOCAL app/escala/utils.py:524 GeneratorExit 3444
TRATADA_LOCAL app/escala/utils.py:525 GeneratorExit 3444
TRATADA_LOCAL app/escala/utils.py:888 GeneratorExit 649
TRATADA_LOCAL app/folha/causas.py:58 GeneratorExit 180
TRATADA_LOCAL app/ponto/juiz_batida.py:75 ValueError 433065
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:1131 GeneratorExit 505
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:2604 GeneratorExit 2567
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:677 GeneratorExit 161
TRATADA_LOCAL app/ponto/services/dia_pago.py:890 GeneratorExit 10520
TRATADA_LOCAL app/ponto/services/espelho.py:473 GeneratorExit 104
TRATADA_LOCAL app/ponto/services/espelho.py:657 GeneratorExit 3607
TRATADA_LOCAL app/ponto/services/fechamento.py:1028 GeneratorExit 191
TRATADA_LOCAL app/ponto/services/fechamento.py:1029 _GradeSemCelula 5
TRATADA_LOCAL app/ponto/turnos.py:1293 GeneratorExit 20459
TRATADA_LOCAL app/ponto/turnos.py:1654 GeneratorExit 4354
TRATADA_LOCAL app/ponto/turnos.py:1697 GeneratorExit 33979
TRATADA_LOCAL app/ponto/turnos.py:2639 GeneratorExit 8
TRATADA_LOCAL app/ponto/turnos.py:677 GeneratorExit 14862
SOMA 10/2026 39   <- tem de ser igual a soma das classes (39) True
SOMA 10/2026 HORAS 156.281613991111099 True
SOMA 10/2026 COLABS_UNIAO 16 True
SOMA 11/2026 35   <- tem de ser igual a soma das classes (35) True
SOMA 11/2026 HORAS 194.65401177694444 True
SOMA 11/2026 COLABS_UNIAO 15 True
rc_sombra=0
```

As seis linhas `SOMA` da parte estão `True`. [nao verificado]

Não há ocorrência de `INCOMPLETO` na saída fornecida. [nao verificado]

## 06/11 13:23 · sessao 8e833b06368f · E-SEM-VIVA-B · verificador · origem=Code

**5a está curado; 5b cura o M8 específico, mas mantém sobreviventes e introduz falsos VERMELHOS.** (`app/escala/services/cadastro_realidade.py:135–137`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:190–207`.)

1. **5a — informativa, procedente: docstring corrigida.** Agora distingue uma consulta **de células por empresa** das consultas adicionais do retentor: duas competências, cada uma carregando células e vínculos, até quatro adicionais por desligado elegível. Isso não representa o total geral: existe também a seleção dos desligados. (`app/escala/services/cadastro_realidade.py:135–137,165–172`; `app/folha/export.py:1206–1212`; `app/escala/alimentacao.py:148–154,185–186`.)

2. **5b — informativa, procedente: cura específica confirmada.** O selo agora detecta os quatro campos quando escritos como literais completos: `getattr(c, 'data_demissao')`, subscrição e `.only('data_demissao')`, por exemplo. Também deixa de aceitar como testemunha única uma chamada sob `if False`, `if 0` ou outra constante falsa. A nova costura exige justamente a rejeição dessa combinação. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:165,190–198,218–228`.)

   As duas costuras passaram executando os métodos reais em memória, com base `unittest.TestCase`. Suíte Django completa e reprodução dos logs M8 anexados: **[não verificado]**. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:209–228`.)

3. **Média — procedente: derivação paralela relevante ainda passa pelo selo.** Na fonte produtiva, substituir a chamada de rescisão por este predicado e acrescentar a chamada morta abaixo mantém o selo **VERDE**, confirmado em memória: [nao verificado]

   ```python
   if 0 == 1:
       sai_pela_rescisao(None, None)

   # No lugar de sai_pela_rescisao(colab, fim):
   getattr(colab, "data_" + "demissao") is not None and \
       getattr(colab, "data_" + "demissao") <= fim
   ```

   O campo completo não aparece como `Constant`; a condição morta é `Compare`, não `Constant`. A cópia reproduz a autoridade atual, portanto equivalência comportamental hoje não demonstra ausência de derivação paralela. Sobrevivência na suíte completa: **[não verificado]**. (`app/escala/services/cadastro_realidade.py:171`; `app/folha/export.py:60`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:187–207`.)

   Também continua possível executar a autoridade, descartar seu retorno e decidir por uma cópia: o selo verifica presença da chamada, sem verificar seu uso. Isso vale para rescisão e retentor. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:187–207`.)

4. **Média — procedente: falsos VERMELHOS introduzidos pelo endurecimento.**

   - **Texto legítimo:** `logger.debug("campo: %s", "data_demissao")` reprova sem ler campo algum. Uma docstring cujo conteúdo inteiro seja `data_demissao` também reprova. A comparação usa igualdade do literal inteiro: uma frase maior contendo esse nome passa. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:192,206`.)
   - **Chamada viva apagada do conjunto:** acrescentar `if False: sai_pela_rescisao(...)`, mantendo a chamada produtiva, reprova. O `discard` remove a identidade da função globalmente, sem distinguir ocorrências. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:186–198`.)
   - **`else` vivo tratado como morto:** `ast.walk(n)` percorre também o `else`; assim, `if False: pass; else: autoridade(...)` perde uma chamada executável. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:195–198`.)

   **A fonte produtiva atual passa**, confirmado em memória. Sua docstring é uma constante longa, diferente dos quatro nomes proibidos; comentários não entram nessa coleta AST. (`app/escala/services/cadastro_realidade.py:132–151,171–172`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:190–207`.)

5. **Item 3 — média, mecanismo procedente; justificativa do descarte insuficiente.** Uma exceção no retentor interrompe o mapa antes do retorno; `_nunca_bateu()` não entrega resultado e `lista()` captura antes de incorporar qualquer C5, inclusive dos ativos. A lista continua após registrar a exceção. (`app/escala/services/cadastro_realidade.py:171–183,813–819`.)

   O comentário “falha ALTA” citado trata especificamente de valores inesperadamente nulos nos campos do fechamento, não da disponibilidade da C5. Portanto, **descartar como regressão deste delta documental/teste procede; descartar a ressalva operacional por essa justificativa não procede**. Ocorrência concreta: **[não verificado]**. (`app/folha/export.py:816–825`; `app/escala/services/cadastro_realidade.py:813–819`; anexo `v3b.diff`.)

6. **Item 2 — informativa, descartes procedem dentro do recorte declarado.** “Até ontem” está no contrato e no filtro da C5; o ramo acusador do retentor não impõe esse teto. A equivalência exige essa interseção temporal, sem configurar regressão nova. (`app/escala/services/cadastro_realidade.py:132,160–167`; `app/folha/export.py:1225–1255`.)

   O classificador inteiro responde uma pergunta mais ampla: usa universo de fechamentos e pode sair por código ausente, férias com batida ou cadastro zero antes do retentor. A C5 declara consultar especificamente rescisão + retentor. **O descarte de escopo procede; igualdade com todo `classificar_export` continua sem demonstração.** (`app/escala/services/cadastro_realidade.py:142–147,171–172`; `app/folha/export.py:775–780,858–876,951–960`; `app/core/juizes.py:288–299`.)

7. **Escritores, leitores e classes — informativa, procedente.** O delta não acrescenta escritor nem leitor de estado produtivo: altera documentação e a inspeção de fonte nos testes. O novo leitor é `_veredito`, que passa a examinar constantes e ramos; ele modifica apenas os conjuntos locais `campos` e `chamadas`, consumidos pelas asserções. (`app/escala/services/cadastro_realidade.py:135–137`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:179–207,218–228`; anexo `v3b.diff`.)

   **Segundo juiz, segundo escritor e derivação paralela produtiva: ausentes no delta.** A decisão produtiva continua usando as duas autoridades; o fallback de relógio permanece anterior ao delta. **Selo totalmente sem mordida: não; garantia incompleta e falsos VERMELHOS: sim**, conforme os achados acima. (`app/escala/services/cadastro_realidade.py:160,171–172`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:190–228`; anexo `v3b.diff`.)

## 06/11 13:22 · sessao 386f7d06277f · - · - · origem=Ronald (tmux)

Achados confirmados por execução, em memória, da classe real com `unittest.TestCase` no lugar da base Django; suíte Django e produção: [não verificado]. [nao verificado]

- **Informativa — procedente: M8 específico está morto.** A leitura `getattr(c, "data_demissao")` agora aparece em `campos`, e `if False: sai_pela_rescisao(...)` deixa de satisfazer a chamada exigida. As duas costuras atuais passaram na sonda. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:190–207,218–228`.)

- **Média — procedente: sobrevivente de derivação paralela.** Substituí, apenas na string em memória, `not sai_pela_rescisao(colab, fim)` por `not (getattr(colab, 'data_' + 'demissao') is not None and getattr(colab, 'data_' + 'demissao') <= fim)` e acrescentei `if 1 == 0: sai_pela_rescisao(None, None)`. O selo ficou **VERDE**: o campo completo não é um `Constant`, e `Compare` não é o teste `Constant` eliminado. A cópia reproduz o predicado atual da autoridade; sobrevivência na suíte completa: [não verificado]. (`app/escala/services/cadastro_realidade.py:171`; `app/folha/export.py:60`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:192,195–207`.)

- **Média — procedente: uso do retorno continua desprotegido.** Outro mutante verde executa a autoridade, descarta seu resultado e usa a cópia: `not (sai_pela_rescisao(colab, fim), (getattr(colab, 'data_' + 'demissao') is not None and getattr(colab, 'data_' + 'demissao') <= fim))[1]`. A coleta exige presença de chamadas, sem verificar o valor usado. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:187–207`; `app/folha/export.py:60`.)

- **Média — procedente: falso VERMELHO para texto legítimo.** `logger.debug('data_demissao')` ficou vermelho sem ler campo algum; docstring cujo conteúdo inteiro seja `'data_demissao'` também. Comentário `# data_demissao`, docstring `'campo data_demissao'` e log com essa frase maior ficaram verdes: a comparação usa igualdade do valor inteiro de cada `Constant`, sem contexto de leitura. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:192,206`.)

- **Média — procedente: remoção global e varredura do `else` dão falso VERMELHO.** Acrescentar uma chamada morta homônima, mantendo intacta a chamada produtiva, remove ambas do conjunto; `if False: pass; else: sai_pela_rescisao(...)` também perde a chamada viva. O algoritmo percorre o `If` inteiro, incluindo `orelse`, e faz `discard` pela identidade da função, sem contar ocorrências. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:186–198`.)

- **Informativa — procedente: a fonte produtiva atual passa.** A sonda retornou ambas as autoridades presentes, `campos=set()` e `nomes=set()`. Sua docstring é um único texto maior, não uma constante igual aos quatro campos proibidos; as chamadas estão no fluxo produtivo da compreensão. (`app/escala/services/cadastro_realidade.py:132–151,171–172`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:165–166,192,202–207`.)

## 06/11 13:21 · sessao 1916fbd0fdff · - · - · origem=Ronald (tmux)

- **5a — informativa, curado com precisão de escopo.** A docstring agora declara uma consulta **de células** por empresa e até quatro adicionais **do retentor por desligado**. São duas competências, cada uma com consulta de células e vínculos; há ainda consulta de empresas e lote dos desligados, portanto não representa o total geral. (`app/escala/services/cadastro_realidade.py:135–137,162–172`; `app/folha/export.py:1206–1212`; `app/escala/alimentacao.py:148–154,163–186`.)

- **Item 3 — média, mecanismo procedente; justificativa do descarte insuficiente.** Exceção em um retentor interrompe a construção do mapa; `_nunca_bateu` não retorna e `lista()` captura antes de incorporar qualquer C5, inclusive dos ativos. O código registra a exceção e continua a lista. (`app/escala/services/cadastro_realidade.py:171–183,813–819`.) A “falha ALTA” citada refere-se especificamente a `None` em campos de fechamento considerados não nulos e à comparação na trava de aprovação; não estabelece política geral para C5. (`app/folha/export.py:816–825`.) O descarte procede **como ausência de regressão neste delta**, restrito a docstring/teste segundo o anexo; não elimina a ressalva factual. Ocorrência concreta: **[não verificado]**.

- **Item 2 — informativa, descarte como defeito novo procede.** “Até ontem” consta no contrato e no filtro SQL; o ramo acusador do retentor não possui essa restrição, logo equivalência exige o recorte temporal declarado. (`app/escala/services/cadastro_realidade.py:132,160–167`; `app/folha/export.py:1225–1255`.) A C5 consulta efetivamente rescisão e retentor, sem copiar seus predicados. (`app/escala/services/cadastro_realidade.py:171–172`.)

- **Classificador inteiro — informativa, fora do recorte declarado, sem igualdade demonstrada.** O contrato explicita as duas perguntas consultadas. (`app/escala/services/cadastro_realidade.py:142–147`.) `classificar_export` tem universo de fechamentos e saídas anteriores ao retentor por código ausente, férias com batida e cadastro zero; portanto não pode ser tratado como sinônimo da composição restrita. (`app/folha/export.py:775–780,858–876,951–960`.) JUIZES confirma `classificar_export` como autoridade para **quem entra no TXT**, pergunta mais ampla. (`app/docs/JUIZES.md:74–77`.)

## 06/11 13:19 · sessao 02aea6a7e4ff · R1-CAUSAD-V3 · sonda · origem=Code

As linhas `SOMA` desta parte não são `True`: todas indicam `False`. Há `INCOMPLETO: 594`. [nao verificado]

Transcrição integral do dado fornecido, sem nova execução da sonda. [nao verificado]

```text
sonda_leitura: script.py [fda4c3378d0e47ce] no cpuset 4-7, lendo sombra como leitor (LEITURA)

PARTE|6|6
ATO|hoje=2026-11-06|agora=2026-11-06T13:12:10.739545-03:00
FORA_DA_MEDIDA|horas_falta
FORA_DA_MEDIDA|horas_reflexo_dsr
FORA_DA_MEDIDA|saldo_banco_horas
UNIVERSO|2026-10|colabs_porta=624|segundos=0.1
UNIVERSO_PARTE 2026-10 100
RUBRICA|competencia|rubrica|v2_h|v3_h|delta_h|colabs_div|dias_colab_div
DONO|competencia|rubrica|dono|v2_h|v3_h|delta_h|colabs|dias_colab
DIA|2026-10|horas_extras_100_noturna|col191|2026-10-12|CADASTRO|0.000000000|1.000000000|+1.000000000
DIA|2026-10|horas_noturnas|col245|2026-09-21|CADASTRO|1.867681376|2.134493001|+0.266811625
DIA|2026-10|horas_noturnas|col245|2026-09-23|CADASTRO|1.869965836|2.137103813|+0.267137977
DIA|2026-10|horas_noturnas|col245|2026-09-24|ESTRUTURA|1.851872500|2.116425714|+0.264553214
DIA|2026-10|horas_noturnas|col245|2026-09-25|CADASTRO|1.863168056|2.129334921|+0.266166865
DIA|2026-10|horas_noturnas|col245|2026-09-26|ESTRUTURA|1.915446667|2.189081905|+0.273635238
DIA|2026-10|horas_noturnas|col245|2026-09-27|CADASTRO|1.885175556|2.154486349|+0.269310794
DIA|2026-10|horas_noturnas|col245|2026-09-28|ESTRUTURA|1.942230278|2.219691746|+0.277461468
DIA|2026-10|horas_noturnas|col245|2026-09-30|ESTRUTURA|1.956297778|2.235768889|+0.279471111
DIA|2026-10|horas_trabalhadas|col599|2026-10-20|CADASTRO|10.100982500|5.572917222|-4.528065278
DIA|2026-10|horas_intra_indenizada|col599|2026-09-22|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-10|horas_intra_indenizada|col599|2026-09-24|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-10|horas_intra_indenizada|col599|2026-09-26|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-10|horas_intra_indenizada|col599|2026-09-28|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-10|horas_intra_indenizada|col599|2026-09-30|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-10|horas_intra_indenizada|col599|2026-10-02|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-10|horas_intra_indenizada|col599|2026-10-06|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-10|horas_extras_50|col599|2026-10-16|CADASTRO|0.000000000|0.224154212|+0.224154212
DIA|2026-10|horas_intra_indenizada|col941|2026-10-01|CADASTRO|1.000000000|0.000000000|-1.000000000
PARCIAL|2026-10|horas_trabalhadas|v2_h=12120.982897860556|v3_h=12116.454832582778|colabs_div=1|dias_colab_div=1
RUBRICA|2026-10|horas_trabalhadas|12120.982897861|12116.454832583|-4.528065278|1|1
DONO|2026-10|horas_trabalhadas|CADASTRO|10.100982500|5.572917222|-4.528065278|1|1
RESIDUO|2026-10|horas_trabalhadas|ATE_LIMIAR|12110.881915361|12110.881915361|+0.000000000|84|1295
RESIDUO|2026-10|horas_trabalhadas|SEM_DECISAO|55.446929781|55.446929781|+0.000000000|53|231
RESIDUO|2026-10|horas_trabalhadas|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|181
INSTRUMENTO_HISTORICO|2026-10|horas_trabalhadas|v2_h=12092.499011373|v3_h=12116.454832583|delta_h=+23.955821209
PARCIAL|2026-10|horas_intra_indenizada|v2_h=445.77514605361114|v3_h=437.77514605361114|colabs_div=2|dias_colab_div=8
RUBRICA|2026-10|horas_intra_indenizada|445.775146054|437.775146054|-8.000000000|2|8
DONO|2026-10|horas_intra_indenizada|CADASTRO|8.000000000|0.000000000|-8.000000000|2|8
RESIDUO|2026-10|horas_intra_indenizada|ATE_LIMIAR|437.775146054|437.775146054|+0.000000000|84|1288
RESIDUO|2026-10|horas_intra_indenizada|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|53|231
RESIDUO|2026-10|horas_intra_indenizada|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|181
INSTRUMENTO_HISTORICO|2026-10|horas_intra_indenizada|v2_h=445.775146054|v3_h=437.775146054|delta_h=-8.000000000
PARCIAL|2026-10|horas_extras_50|v2_h=50.156459266388886|v3_h=50.38061347888889|colabs_div=1|dias_colab_div=1
RUBRICA|2026-10|horas_extras_50|50.156459266|50.380613479|+0.224154213|1|1
DONO|2026-10|horas_extras_50|CADASTRO|0.000000000|0.224154212|+0.224154212|1|1
RESIDUO|2026-10|horas_extras_50|ATE_LIMIAR|50.156459266|50.156459266|+0.000000000|84|1271
RESIDUO|2026-10|horas_extras_50|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|55|255
RESIDUO|2026-10|horas_extras_50|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|181
INSTRUMENTO_HISTORICO|2026-10|horas_extras_50|v2_h=50.156459266|v3_h=50.380613479|delta_h=+0.224154213
PARCIAL|2026-10|horas_extras_100|v2_h=85.194816959722218|v3_h=85.194816959722218|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_extras_100|85.194816960|85.194816960|+0.000000000|0|0
RESIDUO|2026-10|horas_extras_100|ATE_LIMIAR|85.194816960|85.194816960|+0.000000000|84|1272
RESIDUO|2026-10|horas_extras_100|SEM_DECISAO|3.430878889|3.430878889|+0.000000000|55|255
RESIDUO|2026-10|horas_extras_100|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|181
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100|v2_h=88.625695849|v3_h=95.626922793|delta_h=+7.001226944
PARCIAL|2026-10|horas_extras_100_feriado|v2_h=82.29819010833333|v3_h=82.29819010833333|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_extras_100_feriado|82.298190108|82.298190108|+0.000000000|0|0
RESIDUO|2026-10|horas_extras_100_feriado|ATE_LIMIAR|82.298190108|82.298190108|-0.000000000|84|1272
RESIDUO|2026-10|horas_extras_100_feriado|SEM_DECISAO|3.430878889|3.430878889|+0.000000000|55|255
RESIDUO|2026-10|horas_extras_100_feriado|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|181
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100_feriado|v2_h=0.000000000|v3_h=92.730295942|delta_h=+92.730295942
PARCIAL|2026-10|horas_extras_50_noturna|v2_h=0|v3_h=0|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_extras_50_noturna|0.000000000|0.000000000|+0.000000000|0|0
RESIDUO|2026-10|horas_extras_50_noturna|ATE_LIMIAR|0.000000000|0.000000000|+0.000000000|84|1272
RESIDUO|2026-10|horas_extras_50_noturna|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|55|255
RESIDUO|2026-10|horas_extras_50_noturna|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|181
INSTRUMENTO_HISTORICO|2026-10|horas_extras_50_noturna|v2_h=0.000000000|v3_h=0.000000000|delta_h=+0.000000000
PARCIAL|2026-10|horas_extras_100_noturna|v2_h=11.499323055555555|v3_h=12.499323055555555|colabs_div=1|dias_colab_div=1
RUBRICA|2026-10|horas_extras_100_noturna|11.499323056|12.499323056|+1.000000000|1|1
DONO|2026-10|horas_extras_100_noturna|CADASTRO|0.000000000|1.000000000|+1.000000000|1|1
RESIDUO|2026-10|horas_extras_100_noturna|ATE_LIMIAR|11.499323056|11.499323056|+0.000000000|84|1271
RESIDUO|2026-10|horas_extras_100_noturna|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|55|255
RESIDUO|2026-10|horas_extras_100_noturna|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|181
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100_noturna|v2_h=0.000000000|v3_h=12.499323056|delta_h=+12.499323056
PARCIAL|2026-10|horas_folga_trabalhada|v2_h=0|v3_h=0|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_folga_trabalhada|0.000000000|0.000000000|+0.000000000|0|0
RESIDUO|2026-10|horas_folga_trabalhada|ATE_LIMIAR|0.000000000|0.000000000|+0.000000000|84|1296
RESIDUO|2026-10|horas_folga_trabalhada|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|53|231
RESIDUO|2026-10|horas_folga_trabalhada|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|181
INSTRUMENTO_HISTORICO|2026-10|horas_folga_trabalhada|v2_h=83.930816268|v3_h=0.000000000|delta_h=-83.930816268
PARCIAL|2026-10|horas_noturnas|v2_h=2938.3726663505554|v3_h=2940.5372146426985|colabs_div=1|dias_colab_div=8
RUBRICA|2026-10|horas_noturnas|2938.372666351|2940.537214643|+2.164548292|1|8
DONO|2026-10|horas_noturnas|CADASTRO|7.485990823|8.555418083|+1.069427260|1|4
DONO|2026-10|horas_noturnas|ESTRUTURA|7.665847222|8.760968254|+1.095121032|1|4
RESIDUO|2026-10|horas_noturnas|ATE_LIMIAR|2923.220828306|2923.220828306|-0.000000000|84|1275
RESIDUO|2026-10|horas_noturnas|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|56|244
RESIDUO|2026-10|horas_noturnas|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|181
INSTRUMENTO_HISTORICO|2026-10|horas_noturnas|v2_h=2926.418086596|v3_h=2948.309629008|delta_h=+21.891542412
PARCIAL|2026-10|horas_atraso|v2_h=53.192499999999995|v3_h=53.192125908055552|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_atraso|53.192500000|53.192125908|-0.000374092|0|0
RESIDUO|2026-10|horas_atraso|ATE_LIMIAR|53.192500000|53.192125908|-0.000374092|83|1263
RESIDUO|2026-10|horas_atraso|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|56|264
RESIDUO|2026-10|horas_atraso|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|181
INSTRUMENTO_HISTORICO|2026-10|horas_atraso|v2_h=52.163666667|v3_h=52.163459241|delta_h=-0.000207425
PARCIAL|2026-10|horas_saida_antecipada|v2_h=56.545666666666669|v3_h=56.545736652222224|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_saida_antecipada|56.545666667|56.545736652|+0.000069986|0|0
RESIDUO|2026-10|horas_saida_antecipada|ATE_LIMIAR|56.545666667|56.545736652|+0.000069986|83|1263
RESIDUO|2026-10|horas_saida_antecipada|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|56|264
RESIDUO|2026-10|horas_saida_antecipada|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|181
INSTRUMENTO_HISTORICO|2026-10|horas_saida_antecipada|v2_h=53.068166667|v3_h=53.068318597|delta_h=+0.000151930
UNIVERSO|2026-11|colabs_porta=578|segundos=97.7
UNIVERSO_PARTE 2026-11 96
RUBRICA|competencia|rubrica|v2_h|v3_h|delta_h|colabs_div|dias_colab_div
DONO|competencia|rubrica|dono|v2_h|v3_h|delta_h|colabs|dias_colab
DIA|2026-11|horas_intra_indenizada|col599|2026-10-22|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-11|horas_intra_indenizada|col599|2026-10-28|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-11|horas_intra_indenizada|col707|2026-10-29|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-11|horas_intra_indenizada|col707|2026-10-31|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-11|horas_intra_indenizada|col707|2026-11-02|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-11|horas_intra_indenizada|col707|2026-11-04|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-11|horas_extras_50|col707|2026-10-31|CADASTRO|0.000000000|0.187750000|+0.187750000
DIA|2026-11|horas_extras_50|col707|2026-11-02|CADASTRO|0.000000000|0.193095000|+0.193095000
DIA|2026-11|horas_extras_100_noturna|col923|2026-11-02|CADASTRO|2.916764722|4.778043611|+1.861278889
DIA|2026-11|horas_saida_antecipada|col1013|2026-11-05|ESTRUTURA|0.000000000|4.331833611|+4.331833611
PARCIAL|2026-11|horas_trabalhadas|v2_h=6235.4115330605555|v3_h=6235.4115330605555|colabs_div=0|dias_colab_div=0
RUBRICA|2026-11|horas_trabalhadas|6235.411533061|6235.411533061|+0.000000000|0|0
RESIDUO|2026-11|horas_trabalhadas|ATE_LIMIAR|6235.411533061|6235.411533061|+0.000000000|80|675
RESIDUO|2026-11|horas_trabalhadas|SEM_DECISAO|57.838073125|57.838073125|+0.000000000|79|889
RESIDUO|2026-11|horas_trabalhadas|SEM_PAR|0.000000000|0.000000000|+0.000000000|13|223
INSTRUMENTO_HISTORICO|2026-11|horas_trabalhadas|v2_h=6219.295985835|v3_h=6235.411533061|delta_h=+16.115547226
PARCIAL|2026-11|horas_intra_indenizada|v2_h=224.49000823722221|v3_h=218.49000823722221|colabs_div=2|dias_colab_div=6
RUBRICA|2026-11|horas_intra_indenizada|224.490008237|218.490008237|-6.000000000|2|6
DONO|2026-11|horas_intra_indenizada|CADASTRO|6.000000000|0.000000000|-6.000000000|2|6
RESIDUO|2026-11|horas_intra_indenizada|ATE_LIMIAR|218.490008237|218.490008237|+0.000000000|80|669
RESIDUO|2026-11|horas_intra_indenizada|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|79|889
RESIDUO|2026-11|horas_intra_indenizada|SEM_PAR|0.000000000|0.000000000|+0.000000000|13|223
INSTRUMENTO_HISTORICO|2026-11|horas_intra_indenizada|v2_h=224.311379904|v3_h=218.490008237|delta_h=-5.821371667
PARCIAL|2026-11|horas_extras_50|v2_h=13.622823055555555|v3_h=14.003668055555556|colabs_div=1|dias_colab_div=2
RUBRICA|2026-11|horas_extras_50|13.622823056|14.003668056|+0.380845000|1|2
DONO|2026-11|horas_extras_50|CADASTRO|0.000000000|0.380845000|+0.380845000|1|2
RESIDUO|2026-11|horas_extras_50|ATE_LIMIAR|13.622823056|13.622823056|+0.000000000|80|656
RESIDUO|2026-11|horas_extras_50|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|79|906
RESIDUO|2026-11|horas_extras_50|SEM_PAR|0.000000000|0.000000000|+0.000000000|13|223
INSTRUMENTO_HISTORICO|2026-11|horas_extras_50|v2_h=13.622823056|v3_h=14.003668056|delta_h=+0.380845000
PARCIAL|2026-11|horas_extras_100|v2_h=66.021159506111104|v3_h=66.021159506111104|colabs_div=0|dias_colab_div=0
RUBRICA|2026-11|horas_extras_100|66.021159506|66.021159506|+0.000000000|0|0
RESIDUO|2026-11|horas_extras_100|ATE_LIMIAR|66.021159506|66.021159506|+0.000000000|80|658
RESIDUO|2026-11|horas_extras_100|SEM_DECISAO|3.152894444|3.152894444|+0.000000000|79|906
RESIDUO|2026-11|horas_extras_100|SEM_PAR|0.000000000|0.000000000|+0.000000000|13|223
INSTRUMENTO_HISTORICO|2026-11|horas_extras_100|v2_h=69.174053951|v3_h=66.021159506|delta_h=-3.152894444
PARCIAL|2026-11|horas_extras_100_feriado|v2_h=65.215916172777781|v3_h=65.215916172777781|colabs_div=0|dias_colab_div=0
RUBRICA|2026-11|horas_extras_100_feriado|65.215916173|65.215916173|+0.000000000|0|0
RESIDUO|2026-11|horas_extras_100_feriado|ATE_LIMIAR|65.215916173|65.215916173|+0.000000000|80|658
RESIDUO|2026-11|horas_extras_100_feriado|SEM_DECISAO|3.152894444|3.152894444|+0.000000000|79|906
RESIDUO|2026-11|horas_extras_100_feriado|SEM_PAR|0.000000000|0.000000000|+0.000000000|13|223
INSTRUMENTO_HISTORICO|2026-11|horas_extras_100_feriado|v2_h=0.000000000|v3_h=65.215916173|delta_h=+65.215916173
PARCIAL|2026-11|horas_extras_50_noturna|v2_h=0|v3_h=0|colabs_div=0|dias_colab_div=0
RUBRICA|2026-11|horas_extras_50_noturna|0.000000000|0.000000000|+0.000000000|0|0
RESIDUO|2026-11|horas_extras_50_noturna|ATE_LIMIAR|0.000000000|0.000000000|+0.000000000|80|658
RESIDUO|2026-11|horas_extras_50_noturna|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|79|906
RESIDUO|2026-11|horas_extras_50_noturna|SEM_PAR|0.000000000|0.000000000|+0.000000000|13|223
INSTRUMENTO_HISTORICO|2026-11|horas_extras_50_noturna|v2_h=0.000000000|v3_h=0.000000000|delta_h=+0.000000000
PARCIAL|2026-11|horas_extras_100_noturna|v2_h=9.8086850000000005|v3_h=11.669963888888889|colabs_div=1|dias_colab_div=1
RUBRICA|2026-11|horas_extras_100_noturna|9.808685000|11.669963889|+1.861278889|1|1
DONO|2026-11|horas_extras_100_noturna|CADASTRO|2.916764722|4.778043611|+1.861278889|1|1
RESIDUO|2026-11|horas_extras_100_noturna|ATE_LIMIAR|6.891920278|6.891920278|+0.000000000|80|657
RESIDUO|2026-11|horas_extras_100_noturna|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|79|906
RESIDUO|2026-11|horas_extras_100_noturna|SEM_PAR|0.000000000|0.000000000|+0.000000000|13|223
INSTRUMENTO_HISTORICO|2026-11|horas_extras_100_noturna|v2_h=0.000000000|v3_h=11.669963889|delta_h=+11.669963889
PARCIAL|2026-11|horas_folga_trabalhada|v2_h=0|v3_h=0|colabs_div=0|dias_colab_div=0
RUBRICA|2026-11|horas_folga_trabalhada|0.000000000|0.000000000|+0.000000000|0|0
RESIDUO|2026-11|horas_folga_trabalhada|ATE_LIMIAR|0.000000000|0.000000000|+0.000000000|80|675
RESIDUO|2026-11|horas_folga_trabalhada|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|79|889
RESIDUO|2026-11|horas_folga_trabalhada|SEM_PAR|0.000000000|0.000000000|+0.000000000|13|223
INSTRUMENTO_HISTORICO|2026-11|horas_folga_trabalhada|v2_h=73.953620351|v3_h=0.000000000|delta_h=-73.953620351
PARCIAL|2026-11|horas_noturnas|v2_h=1424.117478798373|v3_h=1424.117478798373|colabs_div=0|dias_colab_div=0
RUBRICA|2026-11|horas_noturnas|1424.117478798|1424.117478798|+0.000000000|0|0
RESIDUO|2026-11|horas_noturnas|ATE_LIMIAR|1424.117478798|1424.117478798|-0.000000000|80|667
RESIDUO|2026-11|horas_noturnas|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|80|897
RESIDUO|2026-11|horas_noturnas|SEM_PAR|0.000000000|0.000000000|+0.000000000|13|223
INSTRUMENTO_HISTORICO|2026-11|horas_noturnas|v2_h=1424.117478798|v3_h=1424.117478798|delta_h=+0.000000000
PARCIAL|2026-11|horas_atraso|v2_h=26.881499999999999|v3_h=26.88139402583333|colabs_div=0|dias_colab_div=0
RUBRICA|2026-11|horas_atraso|26.881500000|26.881394026|-0.000105974|0|0
RESIDUO|2026-11|horas_atraso|ATE_LIMIAR|26.881500000|26.881394026|-0.000105974|79|655
RESIDUO|2026-11|horas_atraso|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|80|909
RESIDUO|2026-11|horas_atraso|SEM_PAR|0.000000000|0.000000000|+0.000000000|13|223
INSTRUMENTO_HISTORICO|2026-11|horas_atraso|v2_h=26.881500000|v3_h=26.881394026|delta_h=-0.000105974
PARCIAL|2026-11|horas_saida_antecipada|v2_h=31.359500000000001|v3_h=35.691191891666662|colabs_div=1|dias_colab_div=1
RUBRICA|2026-11|horas_saida_antecipada|31.359500000|35.691191892|+4.331691892|1|1
DONO|2026-11|horas_saida_antecipada|ESTRUTURA|0.000000000|4.331833611|+4.331833611|1|1
RESIDUO|2026-11|horas_saida_antecipada|ATE_LIMIAR|31.359500000|31.359358281|-0.000141719|78|654
RESIDUO|2026-11|horas_saida_antecipada|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|80|909
RESIDUO|2026-11|horas_saida_antecipada|SEM_PAR|0.000000000|0.000000000|+0.000000000|13|223
INSTRUMENTO_HISTORICO|2026-11|horas_saida_antecipada|v2_h=31.359500000|v3_h=35.691191892|delta_h=+4.331691892
ERRO 2026-10 app/ponto/services/dia_pago.py:315 ValueError 13 exemplos=[('col41', '-'), ('col53', '-'), ('col83', '-'), ('col209', '-'), ('col257', '-')]
ERRO 2026-10 sonda ConservacaoNaoFecha 78 exemplos=[('col143', '-'), ('col245', '-'), ('col437', '-'), ('col641', '-'), ('col659', '-')]
ERRO 2026-10 sonda LacunaDeMedicao 210 exemplos=[('col41', '-'), ('col41', '2026-09-21'), ('col41', '2026-09-22'), ('col41', '2026-09-23'), ('col41', '2026-09-24')]
ERRO 2026-11 app/ponto/services/dia_pago.py:315 ValueError 15 exemplos=[('col41', '-'), ('col53', '-'), ('col83', '-'), ('col119', '-'), ('col209', '-')]
ERRO 2026-11 sonda ConservacaoNaoFecha 34 exemplos=[('col641', '-'), ('col677', '-'), ('col899', '-'), ('col929', '-')]
ERRO 2026-11 sonda LacunaDeMedicao 244 exemplos=[('col41', '-'), ('col41', '2026-10-21'), ('col41', '2026-10-22'), ('col41', '2026-10-23'), ('col41', '2026-10-24')]
TRATADA_LOCAL app/escala/services/leitor_celula.py:25 ValueError 1881
TRATADA_LOCAL app/ponto/juiz_batida.py:75 ValueError 146215
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:1075 AttributeError 3819
TRATADA_LOCAL app/ponto/services/fechamento.py:1029 _GradeSemCelula 4
ERROS_POR_TIPO ConservacaoNaoFecha 112
ERROS_POR_TIPO LacunaDeMedicao 454
ERROS_POR_TIPO ValueError 28
SOMA COBERTURA|2026-10|False
SOMA PARTES_EMISSOR|2026-10|horas_trabalhadas|v2=False|v3=False
SOMA DIVERGENCIA|2026-10|horas_trabalhadas|partes_h=-4.528065278|autoridade_menos_residuo_h=-4.528065278|False
SOMA COLABS_UNIAO_DIAS|2026-10|horas_trabalhadas|colabs=False|dias=False
SOMA V3_PORTA_EMISSOR|2026-10|horas_trabalhadas|False
SOMA MOTOR_FECHAMENTO|2026-10|horas_trabalhadas|dias=12176.46|retorno=12176.47|False
SOMA PARTES_EMISSOR|2026-10|horas_intra_indenizada|v2=False|v3=False
SOMA DIVERGENCIA|2026-10|horas_intra_indenizada|partes_h=-8.000000000|autoridade_menos_residuo_h=-8.000000000|False
SOMA COLABS_UNIAO_DIAS|2026-10|horas_intra_indenizada|colabs=False|dias=False
SOMA V3_PORTA_EMISSOR|2026-10|horas_intra_indenizada|False
SOMA MOTOR_FECHAMENTO|2026-10|horas_intra_indenizada|dias=445.77|retorno=445.77|False
SOMA PARTES_EMISSOR|2026-10|horas_extras_50|v2=False|v3=False
SOMA DIVERGENCIA|2026-10|horas_extras_50|partes_h=0.224154212|autoridade_menos_residuo_h=0.224154213|False
SOMA COLABS_UNIAO_DIAS|2026-10|horas_extras_50|colabs=False|dias=False
SOMA V3_PORTA_EMISSOR|2026-10|horas_extras_50|False
SOMA MOTOR_FECHAMENTO|2026-10|horas_extras_50|dias=50.15|retorno=50.15|False
SOMA PARTES_EMISSOR|2026-10|horas_extras_100|v2=False|v3=False
SOMA DIVERGENCIA|2026-10|horas_extras_100|partes_h=0.000000000|autoridade_menos_residuo_h=-0.000000000|False
SOMA COLABS_UNIAO_DIAS|2026-10|horas_extras_100|colabs=False|dias=False
SOMA V3_PORTA_EMISSOR|2026-10|horas_extras_100|False
SOMA MOTOR_FECHAMENTO|2026-10|horas_extras_100|dias=88.64|retorno=88.64|False
SOMA PARTES_EMISSOR|2026-10|horas_extras_100_feriado|v2=False|v3=False
SOMA DIVERGENCIA|2026-10|horas_extras_100_feriado

[truncado em 20000 de 27017 caracteres]

_2306 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
