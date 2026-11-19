# CODEX-ESTADO

> Ultima resposta processada: **19/11/2026 00:45** · publicado em **19/11/2026 00:44** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 19/11 00:45 · sessao f260e7597d8a · - · - · origem=Ronald (tmux)

As declarações diretas da classe e das outras entidades foram enviadas ao agente principal, com os respectivos números de linha. Também enviei as atribuições diretas encontradas em código de produção; a cobertura completa dessas atribuições por AST permanece [não verificado].

## 19/11 00:42 · sessao 39074cf4870a · ESPINHA-NOITE · verificador · origem=Code

**AVAL-DEVOLVIDO — item 2:** a cópia não tem o contrato `atos_pergunta.leitura` / `LeituraDosAtos` que o aval manda usar. O contrato existente é diferente, e o caso SILÊNCIO exige mudança de comportamento que a leitura de R/V, sozinha, não expressa. A migração precisa ter seu alvo e seus testes reconferidos nesta ponta. ([models.py:19](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:19), [models.py:4295](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4295), [tarefa_do_dia.py:182](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:182))

**Item 3:** a premissa de `marcos_do_lote` e o selo inverso têm sítio; a ordem F9 → F5 → F3 está registrada. Não encontrei, entre as fontes disponíveis, outro proibido vigente que impeça especificamente esse selo. A composição git e o estado real da fila permanecem **[não verificado]**. ([veredito_lavrado.py:151](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:151), [BACKLOG.md:2018](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/BACKLOG.md:2018))

Nesta revisão, **PROCEDE** significa que o achado apontado exige devolução ou correção do aval; **NÃO PROCEDE** significa que a objeção examinada não foi demonstrada. Os anexos fornecidos no pedido não existem como arquivos verificáveis nesta cópia; suas afirmações sobre produção são identificadas como **[não verificado]**. A árvore não contém `.git`, portanto hash, ancestralidade e merge-tree também estão **[não verificado]**.

## 1. Premissas na cópia

### Item 2

1. **PROCEDE — autoridade e tipo nomeados não existem como descritos.** O código define `LeituraDoAto` como `'validada_em respondida_em resposta_colab'` e implementa `DecisaoPergunta.leitura`, que devolve `LeituraDoAto(validada, respondida, resposta)`. Não é o tipo de oito campos descrito no brief: não traz `ultima_decisao`, materialização nem autor da validação. A leitura efetiva combina atos reais com projeção sem linha correspondente e preserva validação em respostas marcadas para isso. ([models.py:19](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:19), [models.py:4281](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4281), [models.py:4295](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4295))

2. **PROCEDE — o mapa de leitores e o RED da partição envelheceram.** A partição já chama `not _pergunta_respondida(p)`; esse juiz ainda lê `p.respondida_em` e `p.resposta_colab` da projeção. Também passaram pelo juiz, entre outros, `becos`, a guarda de regularização e o agrupamento do fio: não se deve aplicar literalmente o inventário antigo de acessos crus a esses sítios. Continuam leituras diretas, por exemplo, no fio, no explicador, na declaração, no histórico e em SQL no reconciliador. ([pergunta_no_app.py:80](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/pergunta_no_app.py:80), [juizes.py:1544](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/juizes.py:1544), [becos.py:25](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/becos.py:25), [fio.py:708](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:708), [fio.py:188](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:188), [declaracao_texto.py:228](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/declaracao_texto.py:228), [reconciliador.py:671](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/reconciliador.py:671))

3. **PROCEDE — as linhas e a solução do caso SILÊNCIO precisam ser refeitas.** Nesta cópia, `:330` cria o chamado e `:334` abre o patch; a igualdade está em `:338`: `self.assertEqual(self._fila(p), antes, ...)`. O caso SILÊNCIO espera `silencio_decidido` em `:343–344`, e `_pela_decisao` atualmente devolve `'silencio_decidido', []`. Como `DecisaoPergunta.leitura` devolve só R/V e texto, trocar a fonte da partição por essa leitura não identifica, por si, SILÊNCIO nem cria a ação do admin pedida pelo aval. ([test_f1nova_b1_juiz_respondida.py:328](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_f1nova_b1_juiz_respondida.py:328), [test_f1nova_b1_juiz_respondida.py:338](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_f1nova_b1_juiz_respondida.py:338), [tarefa_do_dia.py:182](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:182), [models.py:4326](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4326))

4. **NÃO PROCEDE — objeção de que C em SQL teria de sair.** Há filtros `materializada_em__isnull=True` no universo absolvível e nas órfãs, lado a lado com o filtro de validação; a face SQL de validação existente se chama `q_validada_pelo_ato`. Manter o filtro C é compatível com o literal do item 2. ([reconciliador.py:671](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/reconciliador.py:671), [reconciliador.py:1092](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/reconciliador.py:1092), [models.py:3539](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:3539))

5. **PROCEDE — a medida das 213 não está fechada.** O dado fornecido declara literalmente `CLASSE_ULT silencio 213` e “Não medido: quantas caem em `passivo`” **[não verificado]**. A cópia mostra que a tarefa calcula `passivo` por linhas de projeção sem correspondência, cortadas por `agora`; isso não permite inferir a quantidade das 213 sem os dados e a medição. ([tarefa_do_dia.py:425](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:425))

### Item 3

6. **NÃO PROCEDE — objeção à premissa de que há um sítio sem leitura de `DecisaoPergunta`.** O corpo de `marcos_do_lote` consulta `Batida`, `CelulaDia` e `PerguntaDisputa` e termina em `return marcos`; as duas pontas da pergunta leem `materializada_em` e `respondida_em`. Não há consulta a `DecisaoPergunta` nesse corpo. `carimbar_resposta_pela_trilha` mora em `services/disputa_emissao.py`, onde grava resposta e ato, fora do escopo do selo proposto. ([veredito_lavrado.py:151](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:151), [veredito_lavrado.py:197](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:197), [disputa_emissao.py:1692](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/disputa_emissao.py:1692))

## 2. Colisão com aval ou proibido vigente

### Item 2

7. **PROCEDE — há outro proibido do construtor que a exceção nominal não cobre.** O wrapper proíbe expressamente `models` de qualquer app, `migrations`, `bin/` e arquivos fora de `app/`; o aviso diz que tocar esses caminhos descarta a volta. Como o juiz `pergunta_respondida` é compartilhado e continua lendo projeção, uma solução que exija modificar `models.py`, uma migration ou `bin/` não caberia no construtor Codex autorizado sem resolver essa restrição. **Não está demonstrado que modificar esses arquivos seja inevitável**: o censo e o desenho devem demonstrar uma solução permitida ou devolver o impedimento. ([bin/codex_pergunta.sh:121](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/bin/codex_pergunta.sh:121), [bin/codex_pergunta.sh:346](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/bin/codex_pergunta.sh:346), [juizes.py:1544](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/juizes.py:1544))

8. **NÃO PROCEDE — colisão demonstrada com a ordem de pouso da simulação.** O BACKLOG registra F5 na espinha e F3 depois de F9 e F5; o registro posterior diz que F6F7 pousa depois da simulação. O aval fornecido também condiciona o pouso da F5 ao fim da simulação e aos pousos de F6F7 e F9. O texto integral de `SANDUICHE-DO-20` e o estado vigente fora desta cópia estão **[não verificado]**; não é possível certificar ausência de colisão com um texto que não foi fornecido nem localizado. ([BACKLOG.md:2325](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/BACKLOG.md:2325), [BACKLOG.md:2018](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/BACKLOG.md:2018), [PROMPTS.md:2572](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/PROMPTS.md:2572))

### Item 3

9. **NÃO PROCEDE — colisão demonstrada do selo com a fila registrada.** O BACKLOG diz literalmente “Construir SO o selo com mutante, vaga 1 depois de F9 e F5”; o item 3 reproduz essa ordem. A exceção nominal ao `RETOMA-POS-CLEAR` consta do dado fornecido **[não verificado]**; o texto integral de `SANDUICHE-DO-20` e eventual outro proibido específico do selo estão **[não verificado]**. ([BACKLOG.md:2018](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/BACKLOG.md:2018))

## 3. Condições literais e mutantes

### Item 2

10. **PROCEDE — “só leitores, nenhuma escrita nova” precisa ser delimitado quanto ao efeito.** A guarda em `aplicar_declaracao` decide se chama `responder_pergunta_disputa_core`; depois chama `validar_pergunta`. Alterar o valor da leitura pode alterar **quando portas de escrita existentes são acionadas**, embora não seja necessário criar um escritor novo. A condição fecha se “nenhuma escrita nova” proíbe novos caminhos de escrita, mas não se significar ausência de mudança em escritas efetivas; o aval precisa registrar qual sentido governa e o censo precisa examinar esse efeito. ([declaracao_texto.py:688](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/declaracao_texto.py:688))

11. **NÃO PROCEDE — objeção de que o mutante da conservação não teria o que morder.** A leitura real acrescenta `linhas_da_projecao_sem_linha`; o teste da F2 documenta e cobre resposta só na projeção. Uma partição migrada que ignore essa conservação pode ser distinguida pelo controle proposto, desde que o teste exercite a partição e prove a morte do mutante. A morte efetiva está **[não verificado]**. ([models.py:4308](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4308), [test_e4c_f2_leitura_do_ato.py:172](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_e4c_f2_leitura_do_ato.py:172))

### Item 3

12. **NÃO PROCEDE — objeção de que o selo inverso não tem sítio.** O corpo delimitado de `marcos_do_lote` fornece o alvo, e o mutante pode introduzir ali uma leitura de `DecisaoPergunta` para demonstrar que o selo a detecta. O selo e sua mordida ainda não estão construídos **[não verificado]**. ([veredito_lavrado.py:151](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:151), [BACKLOG.md:2018](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/BACKLOG.md:2018))

13. **NÃO PROCEDE — contradição necessária em “mesma ponta”.** O literal é executável como F3 sobre a ponta resultante da entrega da F5, preservando a sequência exigida pelo BACKLOG. Se “mesma ponta” exigir o **mesmo hash anterior** à construção da F5, a interpretação muda; o hash e a composição efetiva estão **[não verificado]**. ([BACKLOG.md:2018](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/BACKLOG.md:2018))

## 4. Arquivos e áreas protegidas

### Item 2

O conjunto **candidato**, ainda sujeito ao censo exigido pelo próprio aval, é:

- [fio.py:188](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:188), [reconciliador.py:671](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/reconciliador.py:671), [explicador.py:102](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/explicador.py:102), [declaracao_texto.py:228](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/declaracao_texto.py:228), [vigia_fio.py:738](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/vigia_fio.py:738), [raiox.py:135](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/raiox.py:135), [pergunta_no_app.py:80](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/pergunta_no_app.py:80), [becos.py:25](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/becos.py:25), [historico.py:96](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/historico.py:96) e [vigia_carimbo.py:98](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/vigia_carimbo.py:98).
- [tarefa_do_dia.py:142](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:142) é candidato pela mudança explícita de SILÊNCIO; testes e selo também precisam mudar. A lista final de arquivos e de leitores externos está **[não verificado]** até o censo.

14. **NÃO PROCEDE — toque direto demonstrado nos arquivos de motor, folha, export, DiaPago, escala ou vínculo.** Os candidatos acima estão em `chamados`, mas isso **não prova ausência de efeito indireto**: a guarda de declaração pode acionar portas existentes, como no achado 10. A lei proíbe ao construtor competência exportada e item que pede `!`; o alcance efetivo em exportadas e dinheiro permanece **[não verificado]** até o censo e as medidas requeridas. ([declaracao_texto.py:688](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/declaracao_texto.py:688), [LEIS.md:147](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/LEIS.md:147))

### Item 3

15. **NÃO PROCEDE — toque de produto demonstrado pelo selo proposto.** O escopo registrado é “SO o selo com mutante” e o leitor “nao muda”. O arquivo exato do teste ainda é **[não verificado]**; se colocado em `app/chamados/tests/`, o sítio protegido continua sendo inspecionado sem alterar o leitor. ([BACKLOG.md:2018](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/BACKLOG.md:2018), [veredito_lavrado.py:151](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:151))

REVISAO item 2: PROCEDE 6 / NAO PROCEDE 4.

REVISAO item 3: PROCEDE 0 / NAO PROCEDE 5.

## 19/11 00:35 · sessao 3eee19a55d53 · CADEIA-20-PASSO1 · sonda · origem=Code

A origem das **453 sobras** permanece sem subdivisão: `_marca_intra_duracao` marca `_intra_dur` tanto no par S→E dentro de `intra_max_s` quanto no caso S,S; o laço de `parear_turnos` também usa esse mesmo atributo para absorção pelo marco de intervalo e outros caminhos. Os carimbos `_papel_ata` e `_intra_ata` identificam contribuição da ata, mas não registram qual dos caminhos escreveu primeiro `_intra_dur`. Assim, eles não distinguem as duas origens pedidas. [app/ponto/turnos.py:385](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:385), [app/ponto/turnos.py:476](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:476), [app/ponto/turnos.py:1310](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1310), [app/ponto/turnos.py:1356](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1356), [app/ponto/turnos.py:1408](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1408), [app/ponto/turnos.py:1466](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1466). O total de 453 é o informado em `pronto.md` fornecido no pedido. [não verificado]

Para a direção inversa, a sonda lê as lâmpadas casadas por `periodos_do_dia`, que devolve o instante da luz e trata a virada de dia; exige uma única batida apurável naquele minuto e uma única associação ao turno, e conta apenas a entrada ou saída sem contraparte. Dias são dias de ata por colaborador; batidas são contadas uma vez por empresa. Ambiguidades e atas sem alinhamento são informadas separadamente. [app/ponto/juiz_batida.py:86](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/juiz_batida.py:86), [app/ponto/juiz_batida.py:149](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/juiz_batida.py:149), [app/ponto/turnos.py:43](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:43), [app/ponto/turnos.py:2868](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:2868). A contagem inversa e a execução da sonda permanecem **[não verificado]**.

```python
# Executar pelo envelope de leitura da sombra, no tenant juliani.
# Não cria, altera ou apaga arquivos. Não imprime nome nem CPF.
# Dias da inversa: (colaborador, data da célula da ata).
# Batidas da inversa: PK distinta por empresa, mesmo se duas atas a reivindicarem.

import collections
import datetime
import os
import sys

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone

from escala.models import CelulaDia
from ponto.juiz_batida import periodos_do_dia
from ponto.turnos import batidas_apuraveis, turnos_do_colab


INI = datetime.date(2026, 10, 21)
FIM = datetime.date(2026, 11, 20)
UM_DIA = datetime.timedelta(days=1)


def guarda_sombra():
    if os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra":
        raise RuntimeError("settings")
    banco = settings.DATABASES["default"]
    if banco.get("NAME") != "sombra" or banco.get("HOST") != "sombra_db":
        raise RuntimeError("configuracao")
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION READ ONLY")
        cursor.execute("SELECT current_database(), current_schema()")
        banco_real, schema_real = cursor.fetchone()
    if banco_real != "sombra" or schema_real != "juliani":
        raise RuntimeError("conexao")


def minuto_local(instante):
    if timezone.is_aware(instante):
        instante = timezone.localtime(instante).replace(tzinfo=None)
    return instante.replace(second=0, microsecond=0)


def indice_turnos(turnos):
    por_pk = collections.defaultdict(list)
    for turno in turnos:
        for batida in turno.batidas:
            por_pk[batida.pk].append((turno, batida))
    return por_pk


def situacao_pareador(registros):
    # Mesma precedência da sonda2; não altera as classes já medidas.
    if len(registros) != 1:
        return "NAO_MEDIDA_MULTIPLOS_OU_SEM_TURNO"
    turno, batida = registros[0]
    if getattr(batida, "_intra_dur", False):
        return "NAO_MEDIDA_ORIGEM_PAUSA"
    if turno.entrada is not None and turno.saida is not None:
        return "PAR_FECHADO"
    if turno.entrada is not None and turno.saida is None:
        if batida.pk == turno.entrada.pk:
            return "TURNO_ABERTO"
        return "NAO_MEDIDA_BATIDA_INTERNA_TURNO_ABERTO"
    if turno.entrada is None and turno.saida is not None:
        if batida.pk == turno.saida.pk:
            return "SAIDA_SEM_ENTRADA"
        return "NAO_MEDIDA_BATIDA_INTERNA_SEM_ENTRADA"
    return "FORA_DA_ESCALA_SEM_ENQUADRAMENTO"


def hora_valida(hora):
    if not isinstance(hora, str) or len(hora) != 5 or hora[2] != ":":
        return False
    try:
        return datetime.datetime.strptime(hora, "%H:%M").strftime("%H:%M") == hora
    except ValueError:
        return False


def main():
    classe_erro = "GUARDA_SOMBRA"
    try:
        with transaction.atomic():
            guarda_sombra()

            classe_erro = "CELULAS"
            celulas = list(
                CelulaDia.objects.filter(data__gte=INI, data__lte=FIM)
                .select_related("colaborador", "colaborador__empresa")
                .order_by("colaborador_id", "data")
            )
            por_pessoa = collections.defaultdict(list)
            for celula in celulas:
                por_pessoa[celula.colaborador_id].append(celula)

            pausa_total = 0
            empresas = set()
            inversa_dias = collections.defaultdict(set)
            inversa_batidas = collections.defaultdict(set)
            exemplos = {}
            nao_medida = collections.Counter()

            for cid, cels in sorted(por_pessoa.items()):
                colab = cels[0].colaborador
                emp = colab.empresa_id
                empresas.add(emp)

                classe_erro = "BATIDAS"
                # A primeira janela reproduz a sonda2 para o total de pausas.
                lo = timezone.make_aware(
                    datetime.datetime.combine(INI, datetime.time.min)
                )
                hi = timezone.make_aware(
                    datetime.datetime.combine(FIM + UM_DIA, datetime.time.min)
                )
                batidas = list(batidas_apuraveis(colab, lo, hi))
                por_minuto_sonda2 = collections.defaultdict(list)
                for batida in batidas:
                    local = timezone.localtime(batida.timestamp)
                    if INI <= local.date() <= FIM:
                        por_minuto_sonda2[
                            (local.date(), local.strftime("%H:%M"))
                        ].append(batida)

                # A luz de uma ata pode cair fora da data civil da célula.
                lo_pad = timezone.make_aware(
                    datetime.datetime.combine(INI - UM_DIA, datetime.time.min)
                )
                hi_pad = timezone.make_aware(
                    datetime.datetime.combine(
                        FIM + 2 * UM_DIA, datetime.time.min
                    )
                )
                batidas_pad = list(batidas_apuraveis(colab, lo_pad, hi_pad))
                por_minuto = collections.defaultdict(list)
                for batida in batidas_pad:
                    por_minuto[minuto_local(batida.timestamp)].append(batida)

                classe_erro = "TURNOS"
                turnos = turnos_do_colab(
                    colab, INI - UM_DIA, FIM + UM_DIA
                )
                por_pk = indice_turnos(turnos)

                for cel in cels:
                    classe_erro = "ATA"
                    if not isinstance(cel.ata, dict):
                        nao_medida[(emp, "ATA_AUSENTE")] += 1
                        continue

                    orfas = cel.ata.get("orfas")
                    if orfas is not None and not isinstance(orfas, list):
                        raise RuntimeError("orfas_formato")
                    if orfas is not None:
                        for hora in orfas:
                            if not hora_valida(hora):
                                raise RuntimeError("hora_orfa")
                            candidatos = por_minuto_sonda2.get(
                                (cel.data, hora), []
                            )
                            if len(candidatos) == 1:
                                situacao = situacao_pareador(
                                    por_pk.get(candidatos[0].pk, [])
                                )
                                if situacao == "NAO_MEDIDA_ORIGEM_PAUSA":
                                    pausa_total += 1

                    classe_erro = "INVERSA_ATA"
                    # tipo_escala=None não interfere na construção dos papéis;
                    # o cadastro de intervalo não é objeto desta medida.
                    resultado = periodos_do_dia(cel, None)
                    if resultado.get("desalinhado"):
                        nao_medida[(emp, "ATA_DESALINHADA")] += 1
                        continue

                    papeis = resultado.get("papeis")
                    if not isinstance(papeis, list):
                        raise RuntimeError("papeis_formato")
                    if orfas is None:
                        nao_medida[(emp, "LISTA_ORFAS_AUSENTE")] += 1
                        continue

                    for papel in papeis:
                        if not isinstance(papel, dict):
                            raise RuntimeError("papel_formato")
                        if papel.get("acesa") is not True:
                            continue
                        instante = papel.get("instante_luz")
                        if instante is None:
                            nao_medida[(emp, "LUZ_SEM_INSTANTE")] += 1
                            continue
                        if not isinstance(instante, datetime.datetime):
                            raise RuntimeError("instante_formato")
                        minuto = minuto_local(instante)
                        candidatos = por_minuto.get(minuto, [])
                        if len(candidatos) != 1:
                            motivo = (
                                "LUZ_SEM_BATIDA"
                                if not candidatos
                                else "MINUTO_AMBIGUO"
                            )
                            nao_medida[(emp, motivo)] += 1
                            continue

                        # A ata registra sobras em HH:MM, sem identidade.
                        # Se coincidem, não se afirma qual batida ela casou.
                        if minuto.strftime("%H:%M") in orfas:
                            nao_medida[(emp, "LUZ_TAMBEM_EM_ORFAS")] += 1
                            continue

                        batida = candidatos[0]
                        registros = por_pk.get(batida.pk, [])
                        if len(registros) != 1:
                            nao_medida[
                                (emp, "MULTIPLOS_OU_SEM_TURNO")
                            ] += 1
                            continue

                        turno, batida_turno = registros[0]
                        entrada_orfa = (
                            turno.entrada is not None
                            and turno.saida is None
                            and batida_turno.pk == turno.entrada.pk
                        )
                        saida_orfa = (
                            turno.entrada is None
                            and turno.saida is not None
                            and batida_turno.pk == turno.saida.pk
                        )
                        if not (entrada_orfa or saida_orfa):
                            continue

                        inversa_dias[emp].add((cid, cel.data))
                        inversa_batidas[emp].add(batida.pk)
                        exemplo = (cid, minuto.date(), minuto.strftime("%H:%M"))
                        if emp not in exemplos or exemplo < exemplos[emp]:
                            exemplos[emp] = exemplo

            classe_erro = "SAIDA"
            linhas = [
                "A NAO_MEDIDA_ORIGEM_PAUSA sobras=%d" % pausa_total,
                "A REFERENCIA_SONDA2 sobras=453 confere=%s"
                % ("s" if pausa_total == 453 else "n"),
            ]
            for emp in sorted(empresas, key=lambda valor: str(valor)):
                linhas.append(
                    "B_INVERSA emp=%s dias=%d batidas=%d"
                    % (emp, len(inversa_dias[emp]), len(inversa_batidas[emp]))
                )
                if emp in exemplos:
                    cid, dia, hora = exemplos[emp]
                    linhas.append(
                        "B_INVERSA_EXEMPLO emp=%s col%d %s %s"
                        % (emp, cid, dia.strftime("%d/%m"), hora)
                    )
                else:
                    linhas.append(
                        "B_INVERSA_EXEMPLO emp=%s nenhum" % emp
                    )
            for (emp, motivo), total in sorted(
                nao_medida.items(), key=lambda item: (str(item[0][0]), item[0][1])
            ):
                linhas.append(
                    "B_INVERSA_NAO_MEDIDA emp=%s motivo=%s ocorrencias=%d"
                    % (emp, motivo, total)
                )

        for linha in linhas:
            print(linha)
        print("ERROS=0")
        print("FIM_SONDA_PASSO1_V3")
        return 0

    except Exception:
        print("ERRO " + classe_erro)
        print("ERROS=1")
        print("FIM_SONDA_PASSO1_V3")
        return 1


sys.exit(main())
```

## 19/11 00:34 · sessao e6d8cd33e14a · AUDITORIA-ESPURIAS · sonda · origem=Code

`item["motivo"]` é criado por `classificar_export` com valor inicial `None`; ele pode permanecer assim ou receber um destes códigos fixos: `rescisao_modulo_proprio`, `sem_codigo_dominio`, `ferias_com_batida`, `cadastro_zero`, `furo_espelho` ou `nao_homologado`. Não é texto livre; os detalhes descritivos ficam em outro campo. [app/folha/export.py:837](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/folha/export.py:837), [app/folha/export.py:866](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/folha/export.py:866), [app/folha/export.py:960](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/folha/export.py:960), [app/folha/export.py:968](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/folha/export.py:968)

A saída fornecida não identifica qual dos três valores foi recusado. [não verificado] Na sonda abaixo, `len=0` para um valor que não seja string significa que não há caracteres de string a examinar; `chars_fora` é o conjunto vazio nesse caso. [não verificado]

```python
import logging
import os
import unicodedata
from unittest.mock import patch

from django.conf import settings
from django.db import connection, transaction

from colaboradores.models import Colaborador, Empresa
from folha.export import classificar_export, montar_linhas_export
from ponto.services.fechamento import recalcular_fechamento_mes

MES = 11
ANO = 2026
COL = 950
ONDE = "inicio"


class FalhaRegistrada(BaseException):
    pass


def erro_registrado(self, *args, **kwargs):
    raise FalhaRegistrada()


def sim_nao(valor):
    return "sim" if valor else "nao"


def guarda_sombra():
    if (
        os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra"
        or not getattr(settings, "HASNER_SOMBRA", False)
        or connection.settings_dict.get("NAME") != "sombra"
        or connection.settings_dict.get("HOST") != "sombra_db"
        or getattr(connection, "schema_name", None) != "juliani"
    ):
        raise RuntimeError("configuracao_sombra")


def campo_seguro(campo, valor):
    if isinstance(valor, str):
        fora = [
            c for c in valor
            if not (c.isascii() and (c.isalnum() or c == "_"))
        ]
        if not fora:
            return f"{campo}={valor}"
        categorias = "{" + ",".join(
            sorted({unicodedata.category(c) for c in fora})
        ) + "}"
        return (
            f"{campo}=RECUSADO classe=str len={len(valor)} "
            f"chars_fora={categorias}"
        )

    return (
        f"{campo}=RECUSADO classe={type(valor).__name__} "
        "len=0 chars_fora={}"
    )


def s6():
    global ONDE

    ONDE = "guarda_sombra"
    guarda_sombra()

    with transaction.atomic():
        ONDE = "transacao_sombra"
        with connection.cursor() as cursor:
            cursor.execute(
                "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
            )
            cursor.execute(
                "SELECT current_database(), current_user, current_schema()"
            )
            banco, usuario, schema = cursor.fetchone()
        if (banco, usuario, schema) != ("sombra", "leitor", "juliani"):
            raise RuntimeError("conexao_sombra")

        ONDE = "universo"
        colab = Colaborador.objects.get(pk=COL)
        empresas = list(Empresa.objects.order_by("pk"))
        if not empresas:
            raise RuntimeError("sem_empresas")
        if not any(e.pk == colab.empresa_id for e in empresas):
            raise RuntimeError("empresa_do_colaborador_ausente")

        ONDE = "export_lido"
        encontrou_export = False
        for empresa in empresas:
            with (
                patch.object(logging.Logger, "exception", erro_registrado),
                patch.object(logging.Logger, "error", erro_registrado),
            ):
                lidos = recalcular_fechamento_mes(
                    MES,
                    ANO,
                    empresa_id=empresa.pk,
                    somente_leitura=True,
                    lavrar_dias_pagos=False,
                    retornar_objeto=True,
                )
                fechamentos = list(lidos.values())
                itens = classificar_export(
                    empresa, MES, ANO, fechamentos=fechamentos
                )
                linhas_todas, _ = montar_linhas_export(
                    empresa, MES, ANO,
                    itens=itens, fechamentos=fechamentos,
                )
                alvo = [
                    f for f in fechamentos if f.colaborador_id == COL
                ]
                linhas_alvo, _ = montar_linhas_export(
                    empresa, MES, ANO, itens=itens, fechamentos=alvo
                )

            if len(alvo) > 1 or len(linhas_alvo) > len(linhas_todas):
                raise RuntimeError("atribuicao_de_linhas")

            item_alvo = [
                i for i in itens if i["colaborador_id"] == COL
            ]
            if len(item_alvo) > 1:
                raise RuntimeError("classificacao_duplicada")

            if not item_alvo:
                detalhes = "motivo=sem_fechamento_lido"
            else:
                item = item_alvo[0]
                status = alvo[0].status if alvo else "ausente"
                detalhes = " ".join(
                    (
                        campo_seguro("classificacao", item["status"]),
                        campo_seguro("motivo", item["motivo"]),
                        campo_seguro("status_fechamento", status),
                        f"linhas={len(linhas_alvo)}",
                    )
                )

            sai = bool(linhas_alvo)
            encontrou_export = encontrou_export or sai
            print(
                f"S6 emp={empresa.pk} col{COL} "
                f"export_lido={sim_nao(sai)} "
                f"filtro=app/folha/export.py:716 "
                f"{detalhes}"
            )

        print(
            f"S6 col{COL} export_lido_alguma_empresa="
            f"{sim_nao(encontrou_export)}"
        )


try:
    s6()
except FalhaRegistrada:
    print(f"ERRO S6 {ONDE}: falha_registrada")
except Exception as exc:
    print(f"ERRO S6 {ONDE}: {type(exc).__name__}")
else:
    print("ERROS_S6=0")
    print("FIM_SONDA_DUAS_PONTAS_A5")
```

## 19/11 00:28 · sessao 255a6e96c464 · DST-L2-JZ1 · censo · origem=Code

```python
#!/usr/bin/env python3
# Uso: python3 -B inventario.py /caminho/da/raiz
# SOMENTE LEITURA. O resultado e um inventario sintatico conservador:
# AMBIGUO exige revisao; chamadas dinamicas e fluxo entre funcoes nao sao resolvidos.
import ast
import os
import re
import sys

sys.dont_write_bytecode = True

if len(sys.argv) != 2:
    sys.stderr.write("Uso: python3 -B inventario.py RAIZ\n")
    sys.exit(2)

raiz = os.path.abspath(sys.argv[1])
app = os.path.join(raiz, "app")
templates = os.path.join(app, "templates")

if not os.path.isdir(app):
    sys.stderr.write("ERRO: argv[1] deve conter app/\n")
    sys.exit(2)

campos = {
    "status": "Justificativa.status",
    "chamado_origem": "Justificativa.chamado_origem",
    "chamado_origem_id": "Justificativa.chamado_origem",
    "justificativas_geradas": "Justificativa.chamado_origem",
    "justificativa_relacionada": "ChamadoColaborador.justificativa_relacionada",
    "justificativa_relacionada_id": "ChamadoColaborador.justificativa_relacionada",
    "apontar_justificativa": "ChamadoColaborador.justificativa_relacionada",
}
estados = {"pendente", "rejeitada", "aprovada"}
nomes = sorted(campos, key=len, reverse=True)
padrao = re.compile(
    r"(?<![A-Za-z0-9_])(" + "|".join(re.escape(x) for x in nomes) +
    r")(?:__[A-Za-z0-9_]+)*(?![A-Za-z0-9_])"
)
padrao_estado = re.compile(
    r"(?<![A-Za-z0-9_])(pendente|rejeitada|aprovada)(?![A-Za-z0-9_])"
)
sql_rx = re.compile(
    r"\b(select|insert|update|delete|with|join|where|from|returning)\b",
    re.I,
)
metodos = {
    "filter", "exclude", "get", "create", "get_or_create",
    "update", "update_or_create", "values", "values_list", "only",
}
registros = set()
fechos = []
falhas = []

def classe(caminho):
    partes = caminho.replace("\\", "/").lower().split("/")
    nome = partes[-1]
    if "migrations" in partes:
        return "migration"
    if (
        "tests" in partes or "test" in partes
        or nome.startswith("test_") or nome.endswith("_test.py")
    ):
        return "teste" [nao verificado]
    return "prod"

def trecho(linhas, numero):
    if numero < 1 or numero > len(linhas):
        return ""
    return re.sub(r"\s+", " ", linhas[numero - 1].strip())[:60]

def limpar(valor):
    return str(valor).replace("\t", " ").replace("\r", " ").replace("\n", " ")

def operacoes_metodo(nome):
    if nome in ("create", "update"):
        return ("ESCREVE",)
    if nome in ("get_or_create", "update_or_create"):
        return ("LE", "ESCREVE")
    return ("LE",)

def nome_chamada(no):
    if not isinstance(no, ast.Call):
        return ""
    if isinstance(no.func, ast.Name):
        return no.func.id
    if isinstance(no.func, ast.Attribute):
        return no.func.attr
    return ""

def texto_no(no):
    try:
        return ast.unparse(no)
    except Exception:
        return ""

def nome_campo(texto):
    m = padrao.search(texto)
    return m.group(1) if m else None

def indicio_justificativa(texto):
    return bool(re.search(r"justificativa|\bjust\b|\bjustif\b", texto, re.I))

def detectar_status(no, pais):
    # Valor do mesmo enunciado ou receptor sugestivo. Outros casos sao ambiguos.
    atual = no
    while atual in pais and not isinstance(
        atual, (ast.stmt, ast.comprehension, ast.Lambda)
    ):
        atual = pais[atual]
    texto = texto_no(atual)
    return bool(padrao_estado.search(texto) or indicio_justificativa(texto))

def emitir(arquivo, linhas, numero, qualname, nome, op, forma, categoria,
           ambiguo=False):
    campo = campos[nome]
    if ambiguo:
        campo += " [AMBIGUO]"
    registros.add((
        arquivo, numero, qualname or "<modulo>", campo, op,
        forma, categoria, trecho(linhas, numero)
    ))

def visitar_python(caminho):
    relativo = os.path.relpath(caminho, raiz).replace(os.sep, "/")
    categoria = classe(relativo)
    try:
        with open(caminho, "rb") as f:
            fonte = f.read()
        arvore = ast.parse(fonte, filename=relativo)
        linhas = fonte.decode(
            # tokenize nao esta entre os imports autorizados.
            "utf-8-sig"
        ).splitlines()
    except (OSError, SyntaxError, UnicodeError) as erro:
        falhas.append((relativo, str(erro)))
        return

    pais = {}
    escopos = {}
    for pai in ast.walk(arvore):
        for filho in ast.iter_child_nodes(pai):
            pais[filho] = pai

    def indexar(no, pilha):
        if isinstance(no, (ast.FunctionDef, ast.AsyncFunctionDef, ast.ClassDef)):
            pilha = pilha + [no.name]
        escopos[no] = ".".join(pilha) or "<modulo>"
        for filho in ast.iter_child_nodes(no):
            indexar(filho, pilha)

    indexar(arvore, [])

    def registrar(no, nome, operacoes, forma, numero=None):
        if nome not in campos:
            return
        amb = nome == "status" and not detectar_status(no, pais)
        for op in operacoes:
            emitir(
                relativo, linhas, numero or getattr(no, "lineno", 1),
                escopos.get(no, "<modulo>"), nome, op, forma, categoria, amb
            )

    def contexto_string(no):
        atual = no
        while atual in pais:
            pai = pais[atual]
            if isinstance(pai, ast.Call):
                chamada = nome_chamada(pai)
                if chamada in ("getattr", "setattr"):
                    return (
                        ("ESCREVE",) if chamada == "setattr" else ("LE",),
                        chamada + " com string",
                    )
                if chamada in ("Q", "F"):
                    return ("LE",), chamada + "()"
                if chamada in ("raw", "execute", "executemany"):
                    return ("LE", "ESCREVE"), "SQL raw [AMBIGUO]"
                if chamada in ("values", "values_list", "only"):
                    return ("LE",), chamada
                if chamada in metodos:
                    return operacoes_metodo(chamada), "string em " + chamada
                if chamada == "save":
                    return ("ESCREVE",), "update_fields"
            if isinstance(pai, ast.keyword):
                if pai.arg in ("defaults", "create_defaults"):
                    return ("ESCREVE",), "defaults"
                if pai.arg == "update_fields":
                    return ("ESCREVE",), "update_fields"
            if isinstance(pai, ast.stmt):
                break
            atual = pai
        return ("LE", "ESCREVE"), "string [AMBIGUO]"

    for no in ast.walk(arvore):
        if isinstance(no, ast.Attribute) and no.attr in campos:
            ops = ("ESCREVE",) if isinstance(no.ctx, (ast.Store, ast.Del)) else ("LE",)
            if no.attr == "apontar_justificativa":
                ops = ("LE", "ESCREVE")
            registrar(no, no.attr, ops, "atributo")

        elif isinstance(no, ast.Name) and no.id in campos:
            # Inclui declaracoes de modelo e nomes locais; sua identidade pode
            # exigir revisao. O nome sozinho nao prova o modelo.
            ops = ("ESCREVE",) if isinstance(no.ctx, (ast.Store, ast.Del)) else ("LE",)
            registrar(no, no.id, ops, "nome [AMBIGUO]")

        elif isinstance(no, ast.keyword) and no.arg:
            nome = nome_campo(no.arg)
            if not nome:
                continue
            pai = pais.get(no)
            chamada = nome_chamada(pai)
            if chamada in ("Q", "F"):
                ops, forma = ("LE",), chamada + "()"
            elif chamada in metodos:
                ops, forma = operacoes_metodo(chamada), "kwarg de " + chamada
            else:
                ops, forma = ("LE", "ESCREVE"), "kwarg [AMBIGUO]"
            registrar(no, nome, ops, forma, getattr(pai, "lineno", 1))

        elif isinstance(no, ast.Constant) and isinstance(no.value, str):
            valor = no.value
            achados = list(padrao.finditer(valor))
            if not achados:
                continue
            ops, forma = contexto_string(no)
            if sql_rx.search(valor):
                ops, forma = ("LE", "ESCREVE"), "SQL raw [AMBIGUO]"
            for achado in achados:
                nome = achado.group(1)
                linha = no.lineno + valor[:achado.start()].count("\n")
                if "__" in achado.group(0) and forma == "string [AMBIGUO]":
                    forma_atual = "string 'campo__...' [AMBIGUO]"
                else:
                    forma_atual = forma
                registrar(no, nome, ops, forma_atual, linha)

    # Secao 2: leitura DIRETA de _protesta na menor instrucao que contem
    # a chamada. Nao infere dependencia atraves de listas, saidas ou funcoes.
    if relativo == "app/ponto/services/cartorio.py":
        for no in ast.walk(arvore):
            if not isinstance(no, ast.Call) or nome_chamada(no) != "transicionar_com_pauta":
                continue
            if len(no.args) < 2 or not (
                isinstance(no.args[1], ast.Constant)
                and no.args[1].value == "resolvido"
            ):
                continue
            ato = next((k.value for k in no.keywords if k.arg == "ato"), None)
            if not (
                no.args and isinstance(ato, ast.Attribute)
                and ato.attr == "reconciliar"
                and ast.dump(ato.value, include_attributes=False)
                == ast.dump(no.args[0], include_attributes=False)
            ):
                continue
            instrucao = no
            while instrucao in pais and not isinstance(instrucao, ast.stmt):
                instrucao = pais[instrucao]
            le = any(
                isinstance(x, ast.Name) and x.id == "_protesta"
                and isinstance(x.ctx, ast.Load)
                for x in ast.walk(instrucao)
            )
            fechos.append((relativo, no.lineno, "sim" if le else "nao"))

def visitar_template(caminho):
    relativo = os.path.relpath(caminho, raiz).replace(os.sep, "/")
    try:
        with open(caminho, "r", encoding="utf-8-sig") as f:
            linhas = f.read().splitlines()
    except (OSError, UnicodeError) as erro:
        falhas.append((relativo, str(erro)))
        return
    for numero, linha in enumerate(linhas, 1):
        for achado in padrao.finditer(linha):
            nome = achado.group(1)
            amb = nome == "status" and not (
                padrao_estado.search(linha) or indicio_justificativa(linha)
            )
            emitir(
                relativo, linhas, numero, "<template>", nome, "LE",
                "regex template", "template", amb
            )

for diretorio, dirs, arquivos in os.walk(app):
    dirs.sort()
    for arquivo in sorted(arquivos):
        if arquivo.endswith(".py"):
            visitar_python(os.path.join(diretorio, arquivo))

if os.path.isdir(templates):
    for diretorio, dirs, arquivos in os.walk(templates):
        dirs.sort()
        for arquivo in sorted(arquivos):
            if arquivo.endswith(".html"):
                visitar_template(os.path.join(diretorio, arquivo))
else:
    falhas.append(("app/templates", "diretorio ausente"))

print("arquivo\tlinha\tfuncao\tcampo\toperacao\tforma\tclasse\ttrecho")
contagens = {}
for registro in sorted(registros):
    print("\t".join(limpar(x) for x in registro))
    _, _, _, campo, op, _, categoria, _ = registro
    chave = (campo, op, categoria)
    contagens[chave] = contagens.get(chave, 0) + 1

print("SECAO_2\tCARTORIO_FECHO\tle_protesta=leitura_direta_na_instrucao")
for arquivo, linha, le in sorted(fechos):
    print("CARTORIO_FECHO\t%s\t%d\tle_protesta=%s" % (arquivo, linha, le))

print("CONTAGENS\tcampo\toperacao\tclasse\tquantidade")
for (campo, op, categoria), quantidade in sorted(contagens.items()):
    print("CONTAGEM\t%s\t%s\t%s\t%d" % (campo, op, categoria, quantidade))

if falhas:
    for arquivo, erro in falhas:
        sys.stderr.write("ERRO\t%s\t%s\n" % (limpar(arquivo), limpar(erro)))
    sys.stderr.write("INVENTARIO_INCOMPLETO\n")
    sys.exit(1)

print("FIM_INVENTARIO")
```

`le_protesta` indica leitura **direta na instrução**; dependências indiretas não são resolvidas pelo script. A exaustividade semântica do inventário, inclusive acessos dinâmicos e fluxo entre funções, é [não verificado].

- [cartorio.py:1011](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1011): leitura direta não.
- [cartorio.py:1546](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1546): leitura direta não.
- [cartorio.py:1559](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1559): leitura direta não.
- [cartorio.py:1580](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1580): leitura direta não.
- [cartorio.py:1659](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1659): leitura direta não.
- [cartorio.py:1674](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1674): leitura direta não.

## 19/11 00:25 · sessao ae9e1c795542 · DST-L2-JZ1 · censo · origem=Code

**perguntas de lei: 2**

1. “O fixture do selo JZ1 passa a criar a justificativa com `chamado_origem`, como os três escritores de produção? Sim/não.” Esta pergunta já está expressamente aberta no aval fornecido. [não verificado: o aval foi fornecido no texto da solicitação]

2. Quando o ponteiro do chamado encerrado aponta para uma pendente cuja `chamado_origem` é **outro** chamado, ela deve ser rejeitada por D3 ou preservada por D4? D3 manda rejeitar a do ponteiro; D4 declara que o reuso com origem alheia permanece pendente. O código atual rejeita a pendente do ponteiro sem conferir sua origem. [reconciliador.py:573](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:573)

**O censo abaixo é parcial, não fechado.** Varri com `rg` a cópia, incluindo pesquisas por nomes dos campos, valores de estado e referências a `Justificativa`, além de leitura dirigida dos caminhos encontrados. A revisão dos usos indiretos, SQL/raw, bulk, testes, migrações, relatórios e exports não foi concluída; portanto não posso afirmar que a lista contém todo sítio. Não usei AST e não há script AST a devolver. O diretório não contém metadados Git acessíveis; o HEAD informado não pôde ser verificado com `git rev-parse`. [não verificado] [nao verificado]

Não houve acesso a banco ou logs. O texto fornecido não traz contagem D4; o número de hoje é **[não verificado]**. Nenhum arquivo foi criado ou alterado.

### Divergências que mudam o desenho

- **D1:** o Fix A atual rejeita somente a justificativa do **ponteiro**, se estiver pendente. Ele não procura linhas por `chamado_origem`; portanto não demonstra a rejeição por ambas as testemunhas que D3 propõe. Ele acrescenta à `descricao` o texto `[Auto-rejeitada: premissa do chamado retratada pelo reconciliador.]` e salva apenas `status` e `descricao`. [reconciliador.py:563](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:563)
- **D2:** há ponto de sucesso nos dois percursos. No imediato, `_porm` contém os chamados cuja transição retornou verdadeiro; no adiado, `_fech` tem a mesma propriedade após E2. [cartorio.py:1540](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1540), [cartorio.py:1006](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1006)
- **D3:** a multiplicidade por origem e a união deduplicada com o ponteiro são desenho novo, não comportamento demonstrado pelo Fix A atual. A tensão com D4a está na segunda pergunta de lei. [ponto/models.py:173](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/models.py:173), [reconciliador.py:573](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:573)
- **D4:** o estado persistido disponível nos campos examinados não identifica exclusivamente o fecho por não-protesto. `reconciliar` grava via `sistema`, também usada por outras reconciliações. A sonda abaixo mede um **superconjunto**, não o contador exato pedido. [models.py:642](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:642), [motor.py:86](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/catalogo/motor.py:86)
- **D5:** os dois sítios chamam `reconciliar` pela porta de transição; não chamam ali o corpo S128 que resolve perguntas e anexa ocorrência. Não se deve atribuir esses efeitos ao fecho cru. [cartorio.py:1011](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1011), [cartorio.py:1546](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1546), [reconciliador.py:580](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:580)

A retenção S128 de irmã respondida e não validada é uma guarda distinta dos gates do fecho cru. Não está demonstrado que a presença da irmã, sozinha, faça o gate cru recusar; o resultado de R4 precisa ser demonstrado no caso concreto, e a rejeição projetada só pode ocorrer após sucesso. [reconciliador.py:538](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:538), [models.py:664](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:664)

### Inventário parcial dos três campos

A unidade de `status` é **uma justificativa**; seus estados declarados são `pendente`, `aprovada` e `rejeitada`, com nascimento padrão pendente. A origem é uma FK opcional e não única; o ponteiro é uma FK opcional no chamado, para uma justificativa formal. [ponto/models.py:130](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/models.py:130), [ponto/models.py:159](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/models.py:159), [ponto/models.py:173](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/models.py:173), [chamados/models.py:69](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:69)

| Campo e sítio | Operação e significado no momento |
|---|---|
| `status`: [api/views.py:1989](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/api/views.py:1989), [ponto/views.py:1624](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1624), [processar_alertas_turno.py:394](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/management/commands/processar_alertas_turno.py:394) | **LÊ/ESCREVE:** encontra ou cria pendente durante os três fluxos de produção. |
| `status`: [triagem_batida.py:258](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/triagem_batida.py:258), [regularizacao.py:91](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/regularizacao.py:91) | **ESCREVE:** fluxos de triagem e regularização podem estabelecer novamente `pendente` em linha reutilizada; o estado anterior exige atenção no caso concreto. |
| `status`: [justificativa.py:205](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:205), [ponto/views.py:1797](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1797) | **ESCREVE:** aprovação individual ou em lote pelo DP. |
| `status`: [reconciliador.py:573](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:573), [triagem_batida.py:605](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/triagem_batida.py:605), [entrada_sincronizada.py:38](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/entrada_sincronizada.py:38) | **LÊ/ESCREVE:** rejeição automática da pendente apontada no contexto da retratação ou da batida. |
| `status`: [justificativa.py:277](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:277), [ponto/views.py:1877](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1877) | **LÊ/ESCREVE:** rejeição pelo DP, individual ou em lote. |
| `status`: [ponto/views.py:1712](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1712), [ponto/views.py:1739](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1739), [alertar_justificativas_sla.py:19](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/management/commands/alertar_justificativas_sla.py:19) | **LÊ:** seleção da fila, contadores e alerta de SLA usam o estado atual; a seleção do alerta restringe a pendentes. |
| `status`: [chamados/models.py:136](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:136), [chamados/models.py:1313](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:1313) | **LÊ:** gate humano e premissa regularizada consultam o estado da justificativa apontada. O gate também menciona literais que não constam das três choices do modelo. |
| `chamado_origem`: [api/views.py:1989](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/api/views.py:1989), [ponto/views.py:1624](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1624), [processar_alertas_turno.py:394](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/management/commands/processar_alertas_turno.py:394) | **LÊ/ESCREVE:** API e tela criam ou preenchem origem vazia; o cron informa origem nos defaults de criação, sem presumir que a troca ao reutilizar uma linha. |
| `chamado_origem`: [chamados/utils.py:2](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/utils.py:2), [justificativa.py:33](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:33), [painel_justificativa.html:128](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/templates/ponto/partials/painel_justificativa.html:128) | **LÊ:** followup, tratamento da aprovação e apresentação da origem. |
| Ponteiro: [vinculos.py:81](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/services/vinculos.py:81) | **LÊ/ESCREVE:** `apontar_justificativa` preserva o primeiro apontamento; chamados pelos fluxos da [API:2009](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/api/views.py:2009), [tela:1651](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1651) e [cron:426](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/management/commands/processar_alertas_turno.py:426). |
| Ponteiro: [reconciliador.py:453](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:453), [reconciliador.py:573](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:573), [models.py:136](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:136), [models.py:1313](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:1313) | **LÊ:** retenção de justificativa alheia, Fix A, gate humano e premissa aprovada. A unidade aqui é a justificativa escolhida pelo chamado, não todas as que o têm como origem. |
| Ponteiro: [triagem_batida.py:605](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/triagem_batida.py:605), [entrada_sincronizada.py:38](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/entrada_sincronizada.py:38) | **LÊ:** caminhos de batida examinam a justificativa apontada antes de rejeitá-la. |

Outros leitores de pendente encontrados incluem [chamados/views.py:596](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/views.py:596), [core/views.py:205](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/core/views.py:205), [situacional.py:126](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/colaboradores/services/situacional.py:126) e [utils_turno_orfao.py:53](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/utils_turno_orfao.py:53). Leitores de aprovada encontrados incluem [escala/utils.py:1197](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/escala/utils.py:1197), [efeito_ausencia.py:41](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/efeito_ausencia.py:41) e [fechamento.py:661](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/fechamento.py:661). A classificação função por função e a exaustividade continuam **[não verificado]**.

### Cartório: seleção, sucesso e recusa

`_protesta` pergunta ao juiz do dia sem trabalho e ao leitor de marcos acesos, usando a célula e a ata **deste julgamento**. É lido para `_protestam`, para a lâmpada `cobrado` e para `_nao_protestam`. [cartorio.py:1462](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1462), [cartorio.py:1509](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1509), [cartorio.py:1540](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1540)

Se qualquer não-protestante tiver fio, o lote inteiro de não-protestantes é adiado; após E2, o filtro de `_fech` conserva os sucessos. Sem adiamento, o filtro de `_porm` conserva os sucessos imediatos. Há ainda fechos crus por marcos inexistentes, por códigos do dia e por outros caminhos; não são automaticamente equivalentes aos dois sítios de D2. [cartorio.py:1540](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1540), [cartorio.py:1008](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1008), [cartorio.py:1557](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1557), [cartorio.py:1572](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1572), [cartorio.py:1650](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1650)

A porta `transicionar_com_pauta`, quando recebe `ato`, devolve o resultado do ato nas linhas [160 e 169](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/services/fecho_runtime.py:160). `reconciliar` devolve **False** na [linha 654](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:654), grava na linha 655 e devolve **True** na [linha 656](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:656). A decisão recusa pelo motor na [linha 676](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:676), premissa na [684](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:684), conversa do pai na [688](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:688) e pergunta viva na [693](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:693); entrega a decisão permitida na [697](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:697). A lista de retornos internos de todos os subgates não foi fechada. [não verificado]

### Efeito visível de `pendente → rejeitada`

A API do colaborador retorna as últimas justificativas com estado, descrição e motivo; a tela de atendimento mostra o estado, a descrição truncada e o motivo separado. Assim, o texto do Fix A pode aparecer como parte da descrição, não necessariamente no campo de motivo de rejeição. [api/views.py:2095](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/api/views.py:2095), [meu_atendimento.html:89](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/templates/chamados/meu_atendimento.html:89)

No DP, a fila aceita filtro por estado e o painel mostra descrição e motivo separadamente; as ações do painel são condicionadas à pendência. A mudança retira a linha das consultas e contadores restritos a pendentes e da seleção futura do alerta de SLA. O contador “hoje” usa `atualizado_em`; o `save(update_fields=['status', 'descricao'])` do Fix A não inclui esse campo, de modo que não se deve presumir incremento naquele contador no instante da rejeição. [ponto/views.py:1702](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1702), [painel_justificativa.html:29](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/templates/ponto/partials/painel_justificativa.html:29), [ponto/views.py:1739](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1739), [alertar_justificativas_sla.py:19](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/management/commands/alertar_justificativas_sla.py:19), [reconciliador.py:579](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:579)

O painel de fechamento também apresenta estado; há apresentação no dashboard para justificativa ligada a batida. A rejeição humana tem caminho explícito de notificação, mas o bloco Fix A citado não chama esse caminho: notificação equivalente pelo Fix A **[não verificado]**. Cobertura de todos os relatórios e exports **[não verificado]**. [painel_fechamento.html:69](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/templates/ponto/partials/painel_fechamento.html:69), [dashboard2.html:248](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/templates/core/dashboard2.html:248), [justificativa.py:277](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:277)

### Sonda D4 — superconjunto, não medição exata

O escritor grava `status_local`, `resolvido_em` e `via_encerramento`; para `RECONCILIAR`, a via é `sistema`. Essa marca não distingue não-protesto de outros encerramentos pelo mesmo evento. Não encontrei, nos campos examinados, marcador persistido exclusivo do não-protesto; a linha `_w` do cartório, isoladamente, não demonstra uma coluna consultável por ORM. [models.py:228](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:228), [models.py:655](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:655), [motor.py:95](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/catalogo/motor.py:95), [cartorio.py:1551](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1551)

Abaixo, cada unidade é um **par `(justificativa, chamado)`**, deduplicado para as duas testemunhas. `d4a` significa ponteiro com origem preenchida para outro chamado; `d4b` compara criação com `resolvido_em`. Uma mesma justificativa ligada a dois chamados pode integrar dois pares. O script conta apenas candidatos atualmente `resolvido/sistema`, portanto também pode **omitir** um caso cujo estado ou via tenha mudado depois; não é limite superior garantido do histórico. Ele não foi executado nem validado contra banco. [não verificado]

```python
# Executar no ambiente Django da copia, a partir de app/.
# Saida: apenas contagens, identificadores colN e marcadores de termino.
import os
import sys

sys.dont_write_bytecode = True
os.environ["DJANGO_SETTINGS_MODULE"] = "config.settings.sombra"

erros = 0

try:
    import django
    django.setup()

    from django.conf import settings
    from django.db import connection, transaction
    from django_tenants.utils import schema_context

    from chamados.catalogo.furo_modulos import FURO_MODULES
    from chamados.models import ChamadoColaborador

    cfg = connection.settings_dict
    if (
        settings.SETTINGS_MODULE != "config.settings.sombra"
        or cfg.get("NAME") != "sombra"
        or cfg.get("HOST") != "sombra_db"
        or str(cfg.get("PORT")) != "5432"
    ):
        raise RuntimeError("guarda_sombra")

    # Cada chave e (pk justificativa, pk chamado); os PKs nao sao impressos.
    pares = {}

    with transaction.atomic():
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION READ ONLY")
            cursor.execute("SELECT current_database()")
            if cursor.fetchone()[0] != "sombra":
                raise RuntimeError("banco_incorreto")

        with schema_context("juliani"):
            with connection.cursor() as cursor:
                cursor.execute("SELECT current_schema()")
                if cursor.fetchone()[0] != "juliani":
                    raise RuntimeError("schema_incorreto")

            chamados = ChamadoColaborador.objects.filter(
                modulo_origem__in=FURO_MODULES,
                status_local="resolvido",
                v

[truncado em 20000 de 23598 caracteres]

## 19/11 00:23 · sessao 454253e1f855 · - · - · origem=Ronald (tmux)

Enviei ao agente principal os sítios do ponteiro, das telas, da API, dos contadores e do SLA, incluindo os usos em testes e a distinção entre o Fix A e a rejeição humana. [nao verificado]

Há uma tensão literal entre D3 e D4 quando o ponteiro aponta para uma justificativa cuja origem é outro chamado: D3 inclui a apontada, enquanto D4 manda preservar a pendente reutilizada; o Fix A atual rejeita a apontada sem consultar a origem. [reconciliador.py:573](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:573)

## 19/11 00:23 · sessao 70251811ffac · - · - · origem=Ronald (tmux)

Censo parcial comunicado ao agente principal, com sítios de escrita, leitura e apresentação. A exaustividade de usos indiretos, SQL, testes e arquivos fora do recorte pesquisado permanece **[não verificado]**. [nao verificado]

## 19/11 00:23 · sessao bfe2ddb1c8ef · - · - · origem=Ronald (tmux)

Resultados enviados ao agente principal, incluindo os retornos dos gates, os demais fechos crus, a ausência de um discriminador persistido para não-protesto e a tensão entre D3 e D4a.

## 18/11 23:35 · sessao a973dccb431c · AUDITORIA-ESPURIAS · sonda · origem=Code

`_GradeSemCelula` nasce quando todos os dias dentro do vínculo têm `tipo_dia='sem_celula'` e é tratada pelo `except Exception`, que preenche os campos da grade com `None` ([app/ponto/services/fechamento.py:1043](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/services/fechamento.py:1043), [app/ponto/services/fechamento.py:1087](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/services/fechamento.py:1087)).

A montagem das linhas pode ser chamada em leitura; a view completa também aprova fechamentos e registra a exportação ([app/folha/export.py:690](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/folha/export.py:690), [app/folha/views.py:248](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/folha/views.py:248), [app/folha/views.py:298](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/folha/views.py:298)). O resultado de S6 na sombra é **[nao verificado]**: a saída fornecida parou antes de imprimi-lo [nao verificado].

A sonda abaixo imprime os fechamentos de todas as empresas antes de qualquer export. Também consulta o fechamento gravado e o TXT vigente; a atribuição no TXT usa o código Domínio atual e informa ambiguidade ou impossibilidade de atribuição, sem expor o código ([app/folha/export.py:224](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/folha/export.py:224), [app/folha/export.py:557](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/folha/export.py:557)). Se a leitura calculada registrar ou propagar falha, ela sinaliza erro e preserva as respostas já impressas; os marcadores de sucesso só aparecem se S6 terminar sem falha [nao verificado].

```python
import logging
import os
from unittest.mock import patch

from django.conf import settings
from django.db import connection, transaction

from colaboradores.models import Colaborador, Empresa
from folha.export import (
    classificar_export,
    competencia_entregue,
    montar_linhas_export,
    partes_da_linha,
)
from ponto.models import FechamentoMensal
from ponto.services.fechamento import (
    recalcular_fechamento_mes,
    universo_da_competencia,
)

MES = 11
ANO = 2026
COL = 950
ONDE = "inicio"


class FalhaRegistrada(BaseException):
    pass


def erro_registrado(self, *args, **kwargs):
    # BaseException atravessa os except Exception da funcao chamada.
    # Nunca imprime a mensagem de log, que pode conter dados pessoais.
    raise FalhaRegistrada()


def sim_nao(valor):
    return "sim" if valor else "nao"


def guarda_sombra():
    if (
        os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra"
        or not getattr(settings, "HASNER_SOMBRA", False)
        or connection.settings_dict.get("NAME") != "sombra"
        or connection.settings_dict.get("HOST") != "sombra_db"
        or getattr(connection, "schema_name", None) != "juliani"
    ):
        raise RuntimeError("configuracao_sombra")


def exportacao_gravada(empresa, colab):
    # O juiz retorna apenas a exportacao vigente e nao invalidada.
    registro = competencia_entregue(empresa.pk, MES, ANO, registro=True)
    if registro is None:
        return "sem_exportacao_vigente"

    codigo = (colab.codigo_dominio or "").strip()
    if not codigo or not codigo.isdigit() or len(codigo) > 10:
        return "atribuicao_nao_verificada_codigo_atual"

    matricula = codigo.zfill(10)
    # A comparacao respeita a mesma normalizacao usada na montagem.
    candidatos = 0
    for outro_codigo in Colaborador.objects.filter(
        empresa_id=empresa.pk
    ).values_list("codigo_dominio", flat=True):
        if (outro_codigo or "").strip().zfill(10) == matricula:
            candidatos += 1
    if candidatos != 1:
        return "atribuicao_nao_verificada_codigo_ambiguo"

    if not registro.conteudo:
        return "conteudo_indisponivel"

    encontrou = False
    for linha in registro.conteudo.splitlines():
        if not linha:
            continue
        partes = partes_da_linha(linha)
        if partes is None:
            return "conteudo_nao_interpretado"
        if partes["reg"] == "10":
            if partes["competencia"] != f"{ANO}{MES:02d}":
                return "competencia_no_conteudo_inconsistente"
            if partes["matricula"] == matricula:
                encontrou = True

    return "sim_por_codigo_atual" if encontrou else "nao_por_codigo_atual"


def s6():
    global ONDE

    ONDE = "guarda_sombra"
    guarda_sombra()

    with transaction.atomic():
        ONDE = "transacao_sombra"
        with connection.cursor() as cursor:
            cursor.execute(
                "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
            )
            cursor.execute(
                "SELECT current_database(), current_user, current_schema()"
            )
            banco, usuario, schema = cursor.fetchone()
        if (banco, usuario, schema) != ("sombra", "leitor", "juliani"):
            raise RuntimeError("conexao_sombra")

        ONDE = "universo"
        colab = Colaborador.objects.get(pk=COL)
        empresas = list(Empresa.objects.order_by("pk"))
        if not empresas:
            raise RuntimeError("sem_empresas")
        if not any(e.pk == colab.empresa_id for e in empresas):
            raise RuntimeError("empresa_do_colaborador_ausente")

        # Todas as respostas de fechamento saem antes de tentar qualquer export.
        for empresa in empresas:
            no_universo = universo_da_competencia(
                MES, ANO, empresa.pk
            ).filter(pk=COL).exists()
            gravado = FechamentoMensal.objects.filter(
                colaborador_id=COL,
                colaborador__empresa_id=empresa.pk,
                mes=MES,
                ano=ANO,
            ).exists()
            print(
                f"S6 emp={empresa.pk} col{COL} "
                f"fechamento={sim_nao(no_universo)} "
                f"filtro=app/ponto/services/fechamento.py:439"
            )
            print(
                f"S6 emp={empresa.pk} col{COL} "
                f"fechamento_gravado={sim_nao(gravado)} "
                f"filtro=app/folha/export.py:785"
            )

        # Leitura do TXT efetivamente gravado, independente do recalculo.
        ONDE = "exportacao_gravada"
        for empresa in empresas:
            if empresa.pk != colab.empresa_id:
                resultado = "atribuicao_nao_verificada_empresa_atual"
            else:
                resultado = exportacao_gravada(empresa, colab)
            print(
                f"S6 emp={empresa.pk} col{COL} "
                f"export_gravado={resultado} "
                f"filtro=app/folha/export.py:588,"
                f"app/folha/export.py:246"
            )

        ONDE = "export_lido"
        encontrou_export = False
        for empresa in empresas:
            # Falhas registradas ou propagadas abortam a secao. Excecoes
            # tratadas sem registro seguem o fluxo normal da funcao.
            with (
                patch.object(logging.Logger, "exception", erro_registrado),
                patch.object(logging.Logger, "error", erro_registrado),
            ):
                lidos = recalcular_fechamento_mes(
                    MES,
                    ANO,
                    empresa_id=empresa.pk,
                    somente_leitura=True,
                    lavrar_dias_pagos=False,
                    retornar_objeto=True,
                )
                fechamentos = list(lidos.values())
                itens = classificar_export(
                    empresa, MES, ANO, fechamentos=fechamentos
                )
                linhas_todas, _ = montar_linhas_export(
                    empresa, MES, ANO,
                    itens=itens, fechamentos=fechamentos,
                )
                alvo = [
                    f for f in fechamentos if f.colaborador_id == COL
                ]
                linhas_alvo, _ = montar_linhas_export(
                    empresa, MES, ANO, itens=itens, fechamentos=alvo
                )

            if len(alvo) > 1 or len(linhas_alvo) > len(linhas_todas):
                raise RuntimeError("atribuicao_de_linhas")

            item_alvo = [
                i for i in itens if i["colaborador_id"] == COL
            ]
            if len(item_alvo) > 1:
                raise RuntimeError("classificacao_duplicada")

            if not item_alvo:
                motivo = "sem_fechamento_lido"
            else:
                item = item_alvo[0]
                # Apenas valores internos controlados; nao imprimir detalhe.
                classificacao = item["status"]
                motivo_codigo = item["motivo"]
                status = alvo[0].status if alvo else "ausente"
                for valor in (classificacao, motivo_codigo, status):
                    if not isinstance(valor, str) or not all(
                        c.isascii() and (c.isalnum() or c == "_")
                        for c in valor
                    ):
                        raise ValueError("token_de_saida")
                motivo = (
                    f"classificacao:{classificacao},"
                    f"motivo:{motivo_codigo},"
                    f"status_fechamento:{status},"
                    f"linhas:{len(linhas_alvo)}"
                )

            sai = bool(linhas_alvo)
            encontrou_export = encontrou_export or sai
            print(
                f"S6 emp={empresa.pk} col{COL} "
                f"export_lido={sim_nao(sai)} "
                f"filtro=app/folha/export.py:716 "
                f"motivo={motivo}"
            )

        print(
            f"S6 col{COL} export_lido_alguma_empresa="
            f"{sim_nao(encontrou_export)}"
        )


try:
    s6()
except FalhaRegistrada:
    print(f"ERRO S6 {ONDE}: falha_registrada")
except Exception as exc:
    print(f"ERRO S6 {ONDE}: {type(exc).__name__}")
else:
    print("ERROS_S6=0")
    print("FIM_SONDA_DUAS_PONTAS_A4")
```

## 18/11 23:32 · sessao 5f78fdef0b58 · AUDITORIA-ESPURIAS · sonda · origem=Code

A sonda abaixo **não foi executada**; os resultados de S4, a existência do fechamento gravado e a entrada de col950 no fechamento ou em linhas do export permanecem **[não verificado]**.

- `InsumosDoDono.dono` chama `dono_da_divergencia` com os insumos do cadastro, dos turnos e da fase; quando necessário, `cxr()` obtém a lista do motor por `espelho_do_colab`. A célula é lida por `celulas_do_periodo`. O juiz devolve o dono, mas não um texto de motivo; a sonda identifica a própria chamada como fonte do motivo. [app/ponto/management/commands/e6_oraculo.py:113](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/management/commands/e6_oraculo.py:113), [app/ponto/management/commands/e6_oraculo.py:162](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/management/commands/e6_oraculo.py:162), [app/ponto/management/commands/e6_oraculo.py:182](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/management/commands/e6_oraculo.py:182), [app/escala/alimentacao.py:54](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/alimentacao.py:54).
- `universo_da_competencia` é o universo usado pelo recálculo; seu filtro considera situação, demissão e empresa. [app/ponto/services/fechamento.py:382](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/services/fechamento.py:382), [app/ponto/services/fechamento.py:439](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/services/fechamento.py:439), [app/ponto/services/fechamento.py:554](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/services/fechamento.py:554).
- Para o TXT, a view chama o recálculo com `somente_leitura=True`, passa os objetos a `classificar_export` e depois a `montar_linhas_export`. A montagem filtra classificação e status e só produz linhas para eventos ou faltas aplicáveis; pertencer a `entra_set` sozinho não prova que há linha. [app/folha/views.py:209](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/folha/views.py:209), [app/folha/views.py:267](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/folha/views.py:267), [app/folha/export.py:714](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/folha/export.py:714).
- O recálculo em leitura usa o fechamento gravado como semente se existir e, caso contrário, constrói um objeto em memória; o ramo normal usa `get_or_create`, `save` e `update`. A sonda pede leitura, desliga a lavratura e não chama a view que aprova e registra o ato. O recálculo contém capturas internas de exceção: o rastreador abaixo interrompe a seção quando uma exceção chega ao corpo dessa função, inclusive antes de ela prosseguir após uma captura. Escritas transitivas fora desse corpo permanecem **[não verificado]**. [app/ponto/services/fechamento.py:890](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/services/fechamento.py:890), [app/ponto/services/fechamento.py:1081](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/services/fechamento.py:1081), [app/ponto/services/fechamento.py:1128](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/services/fechamento.py:1128), [app/ponto/services/fechamento.py:1191](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/services/fechamento.py:1191), [app/ponto/services/fechamento.py:1303](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/services/fechamento.py:1303), [app/folha/views.py:248](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/folha/views.py:248).

```python
DELE = [  # (colN, data_turno, pk_entrada, pk_saida, criterio) -- lista do auditor, prod 18/11 22:36
    (32, '2026-11-01', 144660, 144661, 'AMBAS_PLANTADAS'),
    (39, '2026-10-25', 139515, 139514, 'AMBAS_PLANTADAS'),
    (67, '2026-10-21', 139860, 139861, 'AMBAS_PLANTADAS'),
    (113, '2026-10-30', 147496, 147498, 'AMBAS_PLANTADAS'),
    (113, '2026-11-01', 147499, 147564, 'AMBAS_PLANTADAS'),
    (169, '2026-11-06', 156359, 156360, 'AMBAS_PLANTADAS'),
    (237, '2026-11-10', 156297, 156298, 'AMBAS_PLANTADAS'),
    (301, '2026-11-03', 152805, 152808, 'AMBAS_PLANTADAS'),
    (301, '2026-11-04', 152818, 152821, 'AMBAS_PLANTADAS'),
    (301, '2026-11-10', 152813, 152816, 'AMBAS_PLANTADAS'),
    (389, '2026-10-22', 139857, 139858, 'AMBAS_PLANTADAS'),
    (389, '2026-10-24', 139855, 139856, 'AMBAS_PLANTADAS'),
    (389, '2026-10-26', 139853, 139854, 'AMBAS_PLANTADAS'),
    (389, '2026-10-28', 139852, 145044, 'AMBAS_PLANTADAS'),
    (389, '2026-10-30', 145042, 145043, 'AMBAS_PLANTADAS'),
    (389, '2026-11-01', 145040, 145041, 'AMBAS_PLANTADAS'),
    (438, '2026-11-10', 152836, 152838, 'AMBAS_PLANTADAS'),
    (438, '2026-11-14', 156366, 156368, 'AMBAS_PLANTADAS'),
    (439, '2026-11-03', 152841, 152842, 'AMBAS_PLANTADAS'),
    (450, '2026-11-06', 151812, 151813, 'AMBAS_PLANTADAS'),
    (451, '2026-10-29', 144646, 149862, 'AMBAS_PLANTADAS'),
    (451, '2026-10-31', 144647, 149863, 'AMBAS_PLANTADAS'),
    (451, '2026-11-02', 147492, 149867, 'AMBAS_PLANTADAS'),
    (451, '2026-11-04', 147493, 149866, 'AMBAS_PLANTADAS'),
    (451, '2026-11-06', 149864, 149865, 'AMBAS_PLANTADAS'),
    (451, '2026-11-08', 151818, 151819, 'AMBAS_PLANTADAS'),
    (451, '2026-11-14', 156369, 156370, 'AMBAS_PLANTADAS'),
    (455, '2026-11-09', 151829, 152845, 'AMBAS_PLANTADAS'),
    (549, '2026-11-01', 144651, 144653, 'AMBAS_PLANTADAS'),
    (569, '2026-10-23', 137919, 137923, 'AMBAS_PLANTADAS'),
    (569, '2026-10-30', 144648, 144649, 'AMBAS_PLANTADAS'),
    (634, '2026-10-28', 140812, 151821, 'AMBAS_PLANTADAS'),
    (707, '2026-10-27', 152916, 151794, 'AMBAS_PLANTADAS'),
    (709, '2026-10-22', 140875, 140878, 'AMBAS_PLANTADAS'),
    (709, '2026-10-23', 140871, 140874, 'AMBAS_PLANTADAS'),
    (709, '2026-10-28', 140865, 140868, 'AMBAS_PLANTADAS'),
    (712, '2026-10-22', 137782, 137790, 'AMBAS_PLANTADAS'),
    (769, '2026-10-30', 144635, 144636, 'AMBAS_PLANTADAS'),
    (833, '2026-11-04', 153868, 153869, 'AMBAS_PLANTADAS'),
    (873, '2026-10-26', 140604, 140607, 'AMBAS_PLANTADAS'),
    (873, '2026-10-28', 140608, 140611, 'AMBAS_PLANTADAS'),
    (876, '2026-10-23', 137824, 139590, 'AMBAS_PLANTADAS'),
    (876, '2026-10-24', 139584, 139587, 'AMBAS_PLANTADAS'),
    (922, '2026-11-14', 157410, 157411, 'AMBAS_PLANTADAS'),
    (947, '2026-11-10', 151782, 151839, 'AMBAS_PLANTADAS'),
    (948, '2026-11-07', 151803, 151804, 'AMBAS_PLANTADAS'),
    (950, '2026-10-26', 147526, 147525, 'AMBAS_PLANTADAS'),
    (950, '2026-11-03', 156111, 156107, 'AMBAS_PLANTADAS'),
    (950, '2026-11-04', 155167, 155169, 'AMBAS_PLANTADAS'),
    (950, '2026-11-05', 154092, 155165, 'AMBAS_PLANTADAS'),
    (950, '2026-11-06', 156099, 156103, 'AMBAS_PLANTADAS'),
    (950, '2026-11-09', 156110, 156106, 'AMBAS_PLANTADAS'),
    (950, '2026-11-11', 156321, 156324, 'AMBAS_PLANTADAS'),
    (950, '2026-11-12', 156102, 156113, 'AMBAS_PLANTADAS'),
    (950, '2026-11-13', 155172, 155170, 'AMBAS_PLANTADAS'),
    (957, '2026-11-06', 151805, 151807, 'AMBAS_PLANTADAS'),
    (968, '2026-11-14', 158539, 158537, 'AMBAS_PLANTADAS'),
    (993, '2026-10-28', 144948, 144949, 'AMBAS_PLANTADAS'),
    (993, '2026-10-30', 144941, 144944, 'AMBAS_PLANTADAS'),
    (993, '2026-11-01', 144937, 144940, 'AMBAS_PLANTADAS'),
    (993, '2026-11-05', 151892, 151893, 'AMBAS_PLANTADAS'),
    (993, '2026-11-07', 151890, 151891, 'AMBAS_PLANTADAS'),
    (995, '2026-10-24', 137855, 137854, 'AMBAS_PLANTADAS'),
    (1002, '2026-10-26', 151835, 151836, 'AMBAS_PLANTADAS'),
    (1013, '2026-11-01', 151799, 151800, 'AMBAS_PLANTADAS'),
    (1013, '2026-11-03', 151801, 151802, 'AMBAS_PLANTADAS'),
    (1024, '2026-11-04', 146933, 146935, 'AMBAS_PLANTADAS'),
    (1039, '2026-11-09', 156442, 156443, 'AMBAS_PLANTADAS'),
    (1039, '2026-11-11', 156444, 156445, 'AMBAS_PLANTADAS'),
    (1040, '2026-11-12', 156336, 156335, 'AMBAS_PLANTADAS'),
    (399, '2026-11-09', 150884, 150341, 'ORIGEM_MISTA'),
    (502, '2026-10-28', 139463, 141682, 'ORIGEM_MISTA'),
    (516, '2026-10-23', 144710, 135016, 'ORIGEM_MISTA'),
    (995, '2026-11-03', 151977, 145341, 'ORIGEM_MISTA'),
    (995, '2026-11-07', 148485, 148964, 'ORIGEM_MISTA'),
]
FOLGA_COMPLETA = {  # colN -> (data_turno_estrito, [datas de turno real completo em folga]) -- coluna datas_turnos_reais_folga
    (237, '2026-11-10'): ['2026-11-07'],
    (301, '2026-11-03'): ['2026-10-22', '2026-10-29'],
    (301, '2026-11-04'): ['2026-10-22', '2026-10-29'],
    (301, '2026-11-10'): ['2026-10-29', '2026-11-05'],
    (451, '2026-10-29'): ['2026-10-16', '2026-10-18', '2026-10-20', '2026-10-24', '2026-10-26', '2026-10-28'],
    (451, '2026-10-31'): ['2026-10-18', '2026-10-20', '2026-10-24', '2026-10-26', '2026-10-28'],
    (451, '2026-11-02'): ['2026-10-20', '2026-10-24', '2026-10-26', '2026-10-28', '2026-11-01'],
    (451, '2026-11-04'): ['2026-10-24', '2026-10-26', '2026-10-28', '2026-11-01', '2026-11-03'],
    (451, '2026-11-06'): ['2026-10-24', '2026-10-26', '2026-10-28', '2026-11-01', '2026-11-03', '2026-11-05'],
    (451, '2026-11-08'): ['2026-10-26', '2026-10-28', '2026-11-01', '2026-11-03', '2026-11-05', '2026-11-07'],
    (451, '2026-11-14'): ['2026-11-01', '2026-11-03', '2026-11-05', '2026-11-07', '2026-11-09', '2026-11-11', '2026-11-13'],
    (634, '2026-10-28'): ['2026-10-27'],
    (922, '2026-11-14'): ['2026-10-31'],
    (968, '2026-11-14'): ['2026-11-02', '2026-11-03', '2026-11-05', '2026-11-10', '2026-11-12'],
}

import collections
import datetime
import os
import sys

from django.conf import settings
from django.db import connection
from django.utils import timezone

from colaboradores.models import Colaborador, Empresa
from escala.alimentacao import celulas_do_periodo
from folha.export import classificar_export, montar_linhas_export
from ponto.management.commands.e6_oraculo import InsumosDoDono
from ponto.models import FechamentoMensal
from ponto.services.fechamento import (
    recalcular_fechamento_mes,
    universo_da_competencia,
)

MES = 11
ANO = 2026


class FalhaInterna(BaseException):
    def __init__(self, tipo):
        self.tipo = tipo


def guarda_sombra():
    if os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra":
        raise RuntimeError("settings")
    if settings.DATABASES["default"]["NAME"] != "sombra":
        raise RuntimeError("banco_configurado")
    if settings.DATABASES["default"]["HOST"] != "sombra_db":
        raise RuntimeError("host_configurado")
    with connection.cursor() as cursor:
        cursor.execute(
            "SELECT current_database(), current_user, current_schema()"
        )
        banco, usuario, schema = cursor.fetchone()
    if (banco, usuario, schema) != ("sombra", "leitor", "juliani"):
        raise RuntimeError("conexao")


def executar_secao(secao, funcao):
    onde = "guarda_sombra"
    try:
        guarda_sombra()
        onde = funcao.__name__
        funcao()
    except FalhaInterna as exc:
        print(f"ERRO {secao} {onde}: {exc.tipo}")
        return False
    except Exception as exc:
        print(f"ERRO {secao} {onde}: {type(exc).__name__}")
        return False
    print(f"ERROS_{secao}=0")
    return True


def s4():
    pares = sorted({
        (col_id, datetime.date.fromisoformat(dia))
        for (col_id, _turno), dias in FOLGA_COMPLETA.items()
        for dia in dias
    })
    agora = timezone.now()
    hoje = timezone.localtime(agora).date()
    por_colab = collections.defaultdict(list)
    for col_id, dia in pares:
        por_colab[col_id].append(dia)

    contagem = collections.Counter()
    linhas = []
    for col_id, dias in sorted(por_colab.items()):
        colab = Colaborador.objects.get(pk=col_id)
        inicio, fim = min(dias), max(dias)
        celulas = celulas_do_periodo([col_id], inicio, fim)
        insumos = InsumosDoDono(
            colab, inicio, fim, hoje, agora=agora
        )
        for dia in dias:
            celula = celulas.get((col_id, dia))
            dono = insumos.dono(dia)
            if dono is None:
                raise RuntimeError("dono_sem_resposta")
            trabalha = (
                "None" if celula is None or celula.trabalha is None
                else "T" if celula.trabalha else "F"
            )
            origem = "None" if celula is None else celula.origem
            linhas.append(
                f"S4 col{col_id} {dia.isoformat()} "
                f"trabalha_celula={trabalha} origem_celula={origem} "
                f"dono={dono} motivo=juiz_dono_da_divergencia"
            )
            contagem[dono] += 1

    for linha in linhas:
        print(linha)
    resumo = ",".join(
        f"{dono}:{quantidade}" for dono, quantidade in sorted(contagem.items())
    )
    print(f"S4_TOTAL dias={len(pares)} por_dono={resumo}")


def recalcular_sem_engolir_excecao(empresa_id):
    # A funcao real captura Exception em seu proprio corpo. A interrupcao
    # deriva de BaseException para atravessar essas capturas e abortar S6.
    codigo = recalcular_fechamento_mes.__code__
    anterior = sys.gettrace()

    def rastrear(frame, evento, argumento):
        if frame.f_code is codigo and evento == "exception":
            tipo, _valor, _traceback = argumento
            raise FalhaInterna(tipo.__name__)
        return rastrear

    try:
        sys.settrace(rastrear)
        return recalcular_fechamento_mes(
            MES,
            ANO,
            empresa_id=empresa_id,
            somente_leitura=True,
            lavrar_dias_pagos=False,
            retornar_objeto=True,
        )
    finally:
        sys.settrace(anterior)


def s6():
    colab = Colaborador.objects.get(pk=950)
    empresas = list(Empresa.objects.order_by("pk"))
    if not empresas:
        raise RuntimeError("sem_empresas")

    linhas_saida = []
    encontrou_export = False
    encontrou_empresa = False

    for empresa in empresas:
        universo = universo_da_competencia(MES, ANO, empresa.pk)
        no_fechamento = universo.filter(pk=950).exists()
        if empresa.pk == colab.empresa_id:
            encontrou_empresa = True
        linhas_saida.append(
            f"S6 emp={empresa.pk} col950 "
            f"fechamento={'sim' if no_fechamento else 'nao'} "
            f"filtro=app/ponto/services/fechamento.py:439 "
            f"motivo=empresa_id:{colab.empresa_id},"
            f"situacao:{colab.situacao},"
            f"data_demissao:{colab.data_demissao}"
        )

        gravado = FechamentoMensal.objects.filter(
            colaborador_id=950,
            colaborador__empresa_id=empresa.pk,
            mes=MES,
            ano=ANO,
        ).first()
        linhas_saida.append(
            f"S6 emp={empresa.pk} col950 "
            f"fechamento_gravado={'sim' if gravado is not None else 'nao'}"
        )

        lidos = recalcular_sem_engolir_excecao(empresa.pk)
        fechamentos = list(lidos.values())
        itens = classificar_export(
            empresa, MES, ANO, fechamentos=fechamentos
        )
        # Executa a montagem completa da view para a empresa.
        linhas_todas, _entra = montar_linhas_export(
            empresa, MES, ANO, itens=itens, fechamentos=fechamentos
        )
        # A mesma montagem, com os mesmos itens e apenas o objeto de col950,
        # atribui as linhas sem expor matricula, nomes ou conteudo do TXT.
        alvo = [f for f in fechamentos if f.colaborador_id == 950]
        linhas_alvo, _ = montar_linhas_export(
            empresa, MES, ANO, itens=itens, fechamentos=alvo
        )
        if len(linhas_alvo) > len(linhas_todas):
            raise RuntimeError("atribuicao_de_linhas")

        item = next(
            (i for i in itens if i["colaborador_id"] == 950), None
        )
        if item is None:
            motivo = "sem_fechamento_lido"
        else:
            status_fechamento = alvo[0].status if alvo else "ausente"
            motivo = (
                f"classificacao:{item['status']},"
                f"motivo:{item['motivo']},"
                f"status_fechamento:{status_fechamento},"
                f"linhas:{len(linhas_alvo)}"
            )
        sai = bool(linhas_alvo)
        encontrou_export = encontrou_export or sai
        linhas_saida.append(
            f"S6 emp={empresa.pk} col950 export={'sim' if sai else 'nao'} "
            f"filtro=app/folha/export.py:716 "
            f"motivo={motivo}"
        )

    if not encontrou_empresa:
        raise RuntimeError("empresa_do_colaborador_ausente")

    for linha in linhas_saida:
        print(linha)
    print(
        f"S6 col950 export_alguma_empresa="
        f"{'sim' if encontrou_export else 'nao'}"
    )


ok_s4 = executar_secao("S4", s4)
ok_s6 = executar_secao("S6", s6)
if ok_s4 and ok_s6:
    print("FIM_SONDA_DUAS_PONTAS_A3")
```

## 18/11 23:32 · sessao 2408491228a7 · AUDITORIA-ESPURIAS · sonda · origem=Code

O universo de S3 será enumerado pelos colaboradores das empresas 2, 3 e 4 que tenham alguma batida na janela local de 21/10 a 20/11; para cada um, a sonda chamará `turnos_do_colab` e usará os pares de entrada e saída que ele devolver. A enumeração considera também batidas retratadas, para não excluir um colaborador antes de chamar o pareador; o pareador exclui as retratadas. [app/colaboradores/models.py:321](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/colaboradores/models.py:321), [app/ponto/turnos.py:1850](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1850), [app/ponto/turnos.py:1872](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1872).

O pareador é a autoridade do par; `batidas_apuraveis` fornece o conjunto sem retratação. Cada ponta aponta para sua pergunta por `Batida.pergunta_origem`; a sonda chama `pergunta_respondida` e verifica `validada_em`, o campo de validação da pergunta. Para o tipo, usa o literal `hora` solicitado no critério. [app/ponto/turnos.py:1](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1), [app/ponto/turnos.py:2868](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:2868), [app/ponto/models.py:40](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/models.py:40), [app/chamados/juizes.py:1544](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/juizes.py:1544), [app/chamados/models.py:2631](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2631).

Uma batida `app` pode receber `pergunta_origem` quando a materialização reaproveita uma batida; esse vínculo não prova, sozinho, que a batida foi criada pela declaração. Por isso S5 usa `origem` para classificar `app` como `REAL` e `disputa_s84_retro` como `DECLARADA`, deixando outras origens como `INDEFINIDA`. A causa concreta do par col995 em 07/11 continua **[não verificado]** até a leitura das duas PKs; as duas origens `app`, informadas no anexo, não satisfazem o critério estrito. [app/chamados/services/materializacao.py:1537](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/materializacao.py:1537), [app/ponto/models.py:13](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/models.py:13).

A guarda confere settings, host, banco conectado, role e schema; a sonda não chama o materializador nem portas de gravação. Não há modo `somente_leitura` nas funções chamadas de S3. A sonda não foi executada, portanto seus resultados e eventuais erros permanecem **[não verificado]**. [app/config/settings/sombra.py:29](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/config/settings/sombra.py:29), [bin/sonda_leitura.sh:13](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/bin/sonda_leitura.sh:13), [bin/sonda_leitura.sh:239](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/bin/sonda_leitura.sh:239).

```python
DELE = [  # (colN, data_turno, pk_entrada, pk_saida, criterio) -- lista do auditor, prod 18/11 22:36
    (32, '2026-11-01', 144660, 144661, 'AMBAS_PLANTADAS'),
    (39, '2026-10-25', 139515, 139514, 'AMBAS_PLANTADAS'),
    (67, '2026-10-21', 139860, 139861, 'AMBAS_PLANTADAS'),
    (113, '2026-10-30', 147496, 147498, 'AMBAS_PLANTADAS'),
    (113, '2026-11-01', 147499, 147564, 'AMBAS_PLANTADAS'),
    (169, '2026-11-06', 156359, 156360, 'AMBAS_PLANTADAS'),
    (237, '2026-11-10', 156297, 156298, 'AMBAS_PLANTADAS'),
    (301, '2026-11-03', 152805, 152808, 'AMBAS_PLANTADAS'),
    (301, '2026-11-04', 152818, 152821, 'AMBAS_PLANTADAS'),
    (301, '2026-11-10', 152813, 152816, 'AMBAS_PLANTADAS'),
    (389, '2026-10-22', 139857, 139858, 'AMBAS_PLANTADAS'),
    (389, '2026-10-24', 139855, 139856, 'AMBAS_PLANTADAS'),
    (389, '2026-10-26', 139853, 139854, 'AMBAS_PLANTADAS'),
    (389, '2026-10-28', 139852, 145044, 'AMBAS_PLANTADAS'),
    (389, '2026-10-30', 145042, 145043, 'AMBAS_PLANTADAS'),
    (389, '2026-11-01', 145040, 145041, 'AMBAS_PLANTADAS'),
    (438, '2026-11-10', 152836, 152838, 'AMBAS_PLANTADAS'),
    (438, '2026-11-14', 156366, 156368, 'AMBAS_PLANTADAS'),
    (439, '2026-11-03', 152841, 152842, 'AMBAS_PLANTADAS'),
    (450, '2026-11-06', 151812, 151813, 'AMBAS_PLANTADAS'),
    (451, '2026-10-29', 144646, 149862, 'AMBAS_PLANTADAS'),
    (451, '2026-10-31', 144647, 149863, 'AMBAS_PLANTADAS'),
    (451, '2026-11-02', 147492, 149867, 'AMBAS_PLANTADAS'),
    (451, '2026-11-04', 147493, 149866, 'AMBAS_PLANTADAS'),
    (451, '2026-11-06', 149864, 149865, 'AMBAS_PLANTADAS'),
    (451, '2026-11-08', 151818, 151819, 'AMBAS_PLANTADAS'),
    (451, '2026-11-14', 156369, 156370, 'AMBAS_PLANTADAS'),
    (455, '2026-11-09', 151829, 152845, 'AMBAS_PLANTADAS'),
    (549, '2026-11-01', 144651, 144653, 'AMBAS_PLANTADAS'),
    (569, '2026-10-23', 137919, 137923, 'AMBAS_PLANTADAS'),
    (569, '2026-10-30', 144648, 144649, 'AMBAS_PLANTADAS'),
    (634, '2026-10-28', 140812, 151821, 'AMBAS_PLANTADAS'),
    (707, '2026-10-27', 152916, 151794, 'AMBAS_PLANTADAS'),
    (709, '2026-10-22', 140875, 140878, 'AMBAS_PLANTADAS'),
    (709, '2026-10-23', 140871, 140874, 'AMBAS_PLANTADAS'),
    (709, '2026-10-28', 140865, 140868, 'AMBAS_PLANTADAS'),
    (712, '2026-10-22', 137782, 137790, 'AMBAS_PLANTADAS'),
    (769, '2026-10-30', 144635, 144636, 'AMBAS_PLANTADAS'),
    (833, '2026-11-04', 153868, 153869, 'AMBAS_PLANTADAS'),
    (873, '2026-10-26', 140604, 140607, 'AMBAS_PLANTADAS'),
    (873, '2026-10-28', 140608, 140611, 'AMBAS_PLANTADAS'),
    (876, '2026-10-23', 137824, 139590, 'AMBAS_PLANTADAS'),
    (876, '2026-10-24', 139584, 139587, 'AMBAS_PLANTADAS'),
    (922, '2026-11-14', 157410, 157411, 'AMBAS_PLANTADAS'),
    (947, '2026-11-10', 151782, 151839, 'AMBAS_PLANTADAS'),
    (948, '2026-11-07', 151803, 151804, 'AMBAS_PLANTADAS'),
    (950, '2026-10-26', 147526, 147525, 'AMBAS_PLANTADAS'),
    (950, '2026-11-03', 156111, 156107, 'AMBAS_PLANTADAS'),
    (950, '2026-11-04', 155167, 155169, 'AMBAS_PLANTADAS'),
    (950, '2026-11-05', 154092, 155165, 'AMBAS_PLANTADAS'),
    (950, '2026-11-06', 156099, 156103, 'AMBAS_PLANTADAS'),
    (950, '2026-11-09', 156110, 156106, 'AMBAS_PLANTADAS'),
    (950, '2026-11-11', 156321, 156324, 'AMBAS_PLANTADAS'),
    (950, '2026-11-12', 156102, 156113, 'AMBAS_PLANTADAS'),
    (950, '2026-11-13', 155172, 155170, 'AMBAS_PLANTADAS'),
    (957, '2026-11-06', 151805, 151807, 'AMBAS_PLANTADAS'),
    (968, '2026-11-14', 158539, 158537, 'AMBAS_PLANTADAS'),
    (993, '2026-10-28', 144948, 144949, 'AMBAS_PLANTADAS'),
    (993, '2026-10-30', 144941, 144944, 'AMBAS_PLANTADAS'),
    (993, '2026-11-01', 144937, 144940, 'AMBAS_PLANTADAS'),
    (993, '2026-11-05', 151892, 151893, 'AMBAS_PLANTADAS'),
    (993, '2026-11-07', 151890, 151891, 'AMBAS_PLANTADAS'),
    (995, '2026-10-24', 137855, 137854, 'AMBAS_PLANTADAS'),
    (1002, '2026-10-26', 151835, 151836, 'AMBAS_PLANTADAS'),
    (1013, '2026-11-01', 151799, 151800, 'AMBAS_PLANTADAS'),
    (1013, '2026-11-03', 151801, 151802, 'AMBAS_PLANTADAS'),
    (1024, '2026-11-04', 146933, 146935, 'AMBAS_PLANTADAS'),
    (1039, '2026-11-09', 156442, 156443, 'AMBAS_PLANTADAS'),
    (1039, '2026-11-11', 156444, 156445, 'AMBAS_PLANTADAS'),
    (1040, '2026-11-12', 156336, 156335, 'AMBAS_PLANTADAS'),
    (399, '2026-11-09', 150884, 150341, 'ORIGEM_MISTA'),
    (502, '2026-10-28', 139463, 141682, 'ORIGEM_MISTA'),
    (516, '2026-10-23', 144710, 135016, 'ORIGEM_MISTA'),
    (995, '2026-11-03', 151977, 145341, 'ORIGEM_MISTA'),
    (995, '2026-11-07', 148485, 148964, 'ORIGEM_MISTA'),
]
FOLGA_COMPLETA = {  # colN -> (data_turno_estrito, [datas de turno real completo em folga]) -- coluna datas_turnos_reais_folga
    (237, '2026-11-10'): ['2026-11-07'],
    (301, '2026-11-03'): ['2026-10-22', '2026-10-29'],
    (301, '2026-11-04'): ['2026-10-22', '2026-10-29'],
    (301, '2026-11-10'): ['2026-10-29', '2026-11-05'],
    (451, '2026-10-29'): ['2026-10-16', '2026-10-18', '2026-10-20', '2026-10-24', '2026-10-26', '2026-10-28'],
    (451, '2026-10-31'): ['2026-10-18', '2026-10-20', '2026-10-24', '2026-10-26', '2026-10-28'],
    (451, '2026-11-02'): ['2026-10-20', '2026-10-24', '2026-10-26', '2026-10-28', '2026-11-01'],
    (451, '2026-11-04'): ['2026-10-24', '2026-10-26', '2026-10-28', '2026-11-01', '2026-11-03'],
    (451, '2026-11-06'): ['2026-10-24', '2026-10-26', '2026-10-28', '2026-11-01', '2026-11-03', '2026-11-05'],
    (451, '2026-11-08'): ['2026-10-26', '2026-10-28', '2026-11-01', '2026-11-03', '2026-11-05', '2026-11-07'],
    (451, '2026-11-14'): ['2026-11-01', '2026-11-03', '2026-11-05', '2026-11-07', '2026-11-09', '2026-11-11', '2026-11-13'],
    (634, '2026-10-28'): ['2026-10-27'],
    (922, '2026-11-14'): ['2026-10-31'],
    (968, '2026-11-14'): ['2026-11-02', '2026-11-03', '2026-11-05', '2026-11-10', '2026-11-12'],
}

import os
from datetime import date, datetime, time, timedelta

from django.conf import settings
from django.db import connection
from django.utils import timezone

from chamados.juizes import pergunta_respondida
from colaboradores.models import Colaborador
from ponto.models import Batida
from ponto.turnos import batidas_apuraveis, turnos_do_colab


INI = date(2026, 10, 21)
FIM = date(2026, 11, 20)
EMPRESAS = (2, 3, 4)
ORIGEM_DECLARADA = "disputa_s84_retro"
ONDE = "guarda"


def guarda():
    if os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra":
        raise RuntimeError("settings")
    cfg = settings.DATABASES["default"]
    if cfg["NAME"] != "sombra" or cfg["HOST"] != "sombra_db":
        raise RuntimeError("configuracao_banco")
    with connection.cursor() as cursor:
        cursor.execute("SELECT current_database(), current_user, current_schema()")
        banco, usuario, schema = cursor.fetchone()
    if (banco, usuario, schema) != ("sombra", "leitor", "juliani"):
        raise RuntimeError("conexao")


def instante_local(dia):
    return timezone.make_aware(
        datetime.combine(dia, time.min), timezone.get_current_timezone()
    )


def local(valor):
    return timezone.localtime(valor).isoformat() if valor is not None else "None"


def pergunta_elegivel(b):
    if b.pergunta_origem_id is None:
        return False
    p = b.pergunta_origem
    return (
        p.tipo_resposta == "hora"
        and pergunta_respondida(p)
        and p.validada_em is not None
    )


def s3():
    global ONDE
    ONDE = "literal"
    externo = {}
    for col, dia, entrada, saida, criterio in DELE:
        chave = (entrada, saida)
        if chave in externo:
            raise ValueError("par_duplicado")
        externo[chave] = (col, dia, criterio)

    ONDE = "universo"
    inicio_ts = instante_local(INI)
    fim_exclusivo = instante_local(FIM + timedelta(days=1))
    ids = (
        Batida.objects.filter(
            colaborador__empresa_id__in=EMPRESAS,
            timestamp__gte=inicio_ts,
            timestamp__lt=fim_exclusivo,
        )
        .values_list("colaborador_id", flat=True)
        .distinct()
    )
    colaboradores = list(
        Colaborador.objects.filter(pk__in=ids).order_by("pk")
    )

    ONDE = "pareamento"
    nosso = {}
    pares_todos = set()
    por_ponta = {}
    por_empresa = {emp: [0, 0] for emp in EMPRESAS}

    for colab in colaboradores:
        apuraveis = set(
            batidas_apuraveis(colab).values_list("pk", flat=True)
        )
        for turno in turnos_do_colab(colab, INI, FIM):
            e, s = turno.entrada, turno.saida
            if e is None or s is None:
                continue
            chave = (e.pk, s.pk)
            pares_todos.add(chave)
            por_ponta.setdefault(e.pk, set()).add(chave)
            por_ponta.setdefault(s.pk, set()).add(chave)
            if e.pk not in apuraveis or s.pk not in apuraveis:
                raise ValueError("pareador_incluiu_retratada")
            if not (pergunta_elegivel(e) and pergunta_elegivel(s)):
                continue
            if chave in nosso:
                raise ValueError("par_nosso_duplicado")
            estrito = (
                e.origem == ORIGEM_DECLARADA
                and s.origem == ORIGEM_DECLARADA
            )
            nosso[chave] = (
                colab.pk, turno.data_turno.isoformat(), colab.empresa_id, estrito
            )
            por_empresa[colab.empresa_id][0] += 1
            por_empresa[colab.empresa_id][1] += int(estrito)

    ONDE = "reconciliacao"
    chaves_dele = set(externo)
    chaves_nossas = set(nosso)
    ambos = chaves_dele & chaves_nossas
    so_dele = chaves_dele - chaves_nossas
    so_nosso = chaves_nossas - chaves_dele
    estritos = [v for v in nosso.values() if v[3]]

    print(
        "S3_TOTAL "
        f"nosso_amplo={len(nosso)}/{len({v[0] for v in nosso.values()})} "
        f"nosso_estrito={len(estritos)}/{len({v[0] for v in estritos})} "
        f"nos_dois={len(ambos)} so_dele={len(so_dele)} "
        f"so_nosso={len(so_nosso)}"
    )
    for emp in EMPRESAS:
        amplo, estrito = por_empresa[emp]
        print(f"S3_EMP emp={emp} amplo={amplo} estrito={estrito}")

    todos_pks = {pk for par in chaves_dele | chaves_nossas for pk in par}
    batidas = {
        b.pk: b
        for b in Batida.objects.filter(pk__in=todos_pks)
        .select_related("colaborador", "pergunta_origem")
    }
    corte = timezone.make_aware(
        datetime(2026, 11, 18, 4, 0), timezone.get_current_timezone()
    )

    def causa(lado, par):
        bs = [batidas.get(pk) for pk in par]
        if lado == "dele" and any(b is None for b in bs):
            return "HORA_DA_EXTRACAO"
        if any(
            b is not None and b.colaborador.empresa_id not in EMPRESAS
            for b in bs
        ):
            return "EMPRESA"
        if any(
            outro != par
            for pk in par
            for outro in por_ponta.get(pk, ())
        ):
            return "PAREAMENTO"
        # Uma origem mista, por si só, não explica diferença entre conjuntos
        # amplos. Só informa a diferença entre critérios amplo e estrito.
        # Carimbo posterior é evidência temporal apenas para o lado do
        # auditor: ele está em produção e não integrava o dump das 04:00.
        if lado == "dele":
            for b in bs:
                if b is None:
                    continue
                p = b.pergunta_origem if b.pergunta_origem_id else None
                carimbos = [b.criado_em]
                if p is not None:
                    carimbos.extend([p.respondida_em, p.validada_em])
                if any(x is not None and x > corte for x in carimbos):
                    return "HORA_DA_EXTRACAO"
        return "SEM_CAUSA"

    sem_causa = 0
    for lado, pares in (("dele", so_dele), ("nosso", so_nosso)):
        for par in sorted(pares):
            if lado == "dele":
                col, dia, _ = externo[par]
            else:
                col, dia, _, _ = nosso[par]
            c = causa(lado, par)
            sem_causa += int(c == "SEM_CAUSA")
            print(
                f"S3_DIF lado={lado} col{col} {dia} "
                f"{par[0]} {par[1]} causa={c}"
            )
    print(f"S3_SEM_CAUSA={sem_causa}")


def s5():
    global ONDE
    ONDE = "literal"
    mistos = [linha for linha in DELE if linha[4] == "ORIGEM_MISTA"]
    if len(mistos) != 5:
        raise ValueError("quantidade_mistos")
    pks = [pk for _, _, e, s, _ in mistos for pk in (e, s)]
    if len(set(pks)) != len(pks):
        raise ValueError("pk_mista_duplicada")

    ONDE = "batidas"
    batidas = {
        b.pk: b
        for b in Batida.objects.filter(pk__in=pks)
    }
    if set(batidas) != set(pks):
        raise LookupError("pk_ausente_na_sombra")

    linhas = []
    for col, dia, entrada, saida, _ in mistos:
        for ponta, pk in (("E", entrada), ("S", saida)):
            b = batidas[pk]
            if b.colaborador_id != col:
                raise ValueError("colaborador_divergente")
            if b.origem == "app":
                classe = "REAL"
            elif b.origem == ORIGEM_DECLARADA:
                classe = "DECLARADA"
            else:
                classe = "INDEFINIDA"
            linhas.append(
                f"S5 col{col} {dia} pk={pk} ponta={ponta} "
                f"origem={b.origem} pergunta_origem={b.pergunta_origem_id} "
                f"retratada={'s' if b.retratada_em is not None else 'n'} "
                f"criado_em={local(b.criado_em)} "
                f"real_ou_declarada={classe} por=origem"
            )
    for linha in linhas:
        print(linha)


def executar(secao, funcao):
    global ONDE
    ONDE = "guarda"
    try:
        guarda()
        funcao()
    except Exception as exc:
        print(f"ERRO {secao} {ONDE}: {type(exc).__name__}")
        return False
    print(f"ERROS_{secao}=0")
    return True


ok_s3 = executar("S3", s3)
ok_s5 = executar("S5", s5)
if ok_s3 and ok_s5:
    print("FIM_SONDA_DUAS_PONTAS_A2")
```

## 18/11 23:30 · sessao fd58a4018d8e · CADEIA-20-PASSO1 · sonda · origem=Code

Não há dados de produção no material fornecido para imprimir as contagens: A, B, o caso col81 e C continuam **[não verificado]**. A sonda abaixo mede o que consegue ler e deixa em classe explícita a pausa cuja origem não é distinguível pelo carimbo preservado. [app/ponto/turnos.py:415; app/ponto/turnos.py:486; app/ponto/turnos.py:1317; app/ponto/turnos.py:1370]

As funções e campos usados são:

- `classificar_orfas` recebe um dia com `orfas` em `HH:MM`; a ata gravada está em `CelulaDia.ata`, e a grade conserva órfãs nesse formato. A sonda lê a ata soberana e chama o classificador existente sem usar sua classificação de procedência como razão da sobra. [app/escala/orfa_classe.py:53; app/escala/models.py:1778; app/escala/utils.py:1827]
- `batidas_apuraveis` fornece as batidas não retratadas. `turnos_do_colab` alimenta e chama `parear_turnos` com marcos e papel da ata; o retorno contém entrada, saída e batidas do turno. [app/ponto/turnos.py:2868; app/ponto/turnos.py:1850; app/ponto/turnos.py:1978; app/ponto/turnos.py:43]
- O envelope de 90 minutos está em `parear_turnos`: o pareador compara também a proximidade da borda e carimba a batida absorvida em `_intra_dur`. Esse mesmo carimbo também é usado por outras decisões, inclusive papel da ata e outras regras; portanto, sua presença isolada não prova que a sobra pertence a uma das duas pausas autorizadas. A sonda conta esses casos como `NAO_MEDIDA_ORIGEM_PAUSA` em vez de reaplicar o envelope ou presumir a origem. [app/ponto/turnos.py:1353; app/ponto/turnos.py:1370; app/ponto/turnos.py:1313; app/ponto/turnos.py:1413; app/ponto/turnos.py:1477]
- `autoridade_do_periodo` devolve o `ResultadoMes` produzido pelo motor para a janela pedida; a sonda lê `dias_cadastro_x_realidade` desse resultado. O caminho mostrado calcula em memória; não identifiquei escrita explícita nele, mas não pude verificar exaustivamente todos os efeitos transitivos de suas chamadas **[não verificado]**. Por isso, a transação é declarada `READ ONLY` no banco antes de chamar qualquer função de domínio. [app/ponto/services/espelho.py:970; app/ponto/services/espelho.py:1179; app/ponto/motor_calculo_v2.py:2719]
- `ler_lavra` lê o snapshot mais recente do esmeril e verifica o marcador da sombra; `lavrar` e `gravar_lavra` escrevem e não são chamados. A saída C abaixo responde à presença em `por_colab` **da lavra especificada no pedido**, com os códigos de assinatura como motivo. [app/ponto/services/esmeril_espelho.py:334; app/ponto/services/esmeril_espelho.py:296; app/ponto/services/esmeril_espelho.py:317; app/ponto/services/esmeril_espelho.py:227]

Há uma limitação em B: a sonda compara as órfãs da ata com sua presença nos turnos e conta como divergência quando o pareador as incluiu em turno fechado. A direção inversa — uma marca casada pela ata e deixada sem par pelo pareador — não é agregada, pois a identidade da lâmpada no minuto e a semântica de todas as formas de ata não estão demonstradas aqui **[não verificado]**. O script declara essa limitação na saída; seus totais B são, portanto, parciais **[não verificado]**.

```python
# Executar pelo envelope de leitura da sombra, no tenant juliani.
# Não cria, altera nem apaga arquivos. Não imprime nome ou CPF.

import collections
import datetime
import os
import sys

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone

from escala.models import CelulaDia
from escala.orfa_classe import classificar_orfas
from ponto.services.esmeril_espelho import ler_lavra
from ponto.services.espelho import autoridade_do_periodo
from ponto.turnos import batidas_apuraveis, turnos_do_colab


INI = datetime.date(2026, 10, 21)
FIM = datetime.date(2026, 11, 20)
UM_DIA = datetime.timedelta(days=1)


def instante_local(b):
    return timezone.localtime(b.timestamp)


def chave_minuto(b):
    local = instante_local(b)
    return local.date(), local.strftime("%H:%M")


def guarda_sombra():
    if os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra":
        raise RuntimeError("settings")
    banco = settings.DATABASES["default"]
    if banco.get("NAME") != "sombra" or banco.get("HOST") != "sombra_db":
        raise RuntimeError("configuracao")
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION READ ONLY")
        cursor.execute("SELECT current_database(), current_schema()")
        banco_real, schema_real = cursor.fetchone()
    if banco_real != "sombra" or schema_real != "juliani":
        raise RuntimeError("conexao")


def indice_turnos(turnos):
    por_pk = collections.defaultdict(list)
    for turno in turnos:
        for batida in turno.batidas:
            por_pk[batida.pk].append((turno, batida))
    return por_pk


def situacao_pareador(registros):
    if len(registros) != 1:
        return "NAO_MEDIDA_MULTIPLOS_OU_SEM_TURNO"
    turno, batida = registros[0]
    if getattr(batida, "_intra_dur", False):
        return "NAO_MEDIDA_ORIGEM_PAUSA"
    if turno.entrada is not None and turno.saida is not None:
        return "PAR_FECHADO"
    if turno.entrada is not None and turno.saida is None:
        if batida.pk == turno.entrada.pk:
            return "TURNO_ABERTO"
        return "NAO_MEDIDA_BATIDA_INTERNA_TURNO_ABERTO"
    if turno.entrada is None and turno.saida is not None:
        if batida.pk == turno.saida.pk:
            return "SAIDA_SEM_ENTRADA"
        return "NAO_MEDIDA_BATIDA_INTERNA_SEM_ENTRADA"
    return "FORA_DA_ESCALA_SEM_ENQUADRAMENTO"


def main():
    onde = "guarda_sombra"
    try:
        with transaction.atomic():
            guarda_sombra()

            onde = "lavra"
            lavra = ler_lavra()
            if lavra is None:
                raise RuntimeError("lavra_ausente")
            por_colab = lavra.get("por_colab")
            if not isinstance(por_colab, dict):
                raise RuntimeError("lavra_formato")
            for cid in (87, 975):
                assinaturas = por_colab.get(str(cid))
                if assinaturas is None:
                    print("C col%d na_lista=n motivo=ausente_da_lavra_esmeril" % cid)
                else:
                    if not isinstance(assinaturas, dict):
                        raise RuntimeError("assinaturas_formato")
                    codigos = ",".join(sorted(assinaturas)) or "sem_assinaturas"
                    print("C col%d na_lista=s motivo=%s" % (cid, codigos))

            onde = "celulas"
            celulas = list(
                CelulaDia.objects.filter(data__gte=INI, data__lte=FIM)
                .select_related("colaborador", "colaborador__empresa")
                .order_by("colaborador_id", "data")
            )
            por_pessoa = collections.defaultdict(list)
            for celula in celulas:
                por_pessoa[celula.colaborador_id].append(celula)

            a_contagem = collections.Counter()
            a_dias = collections.defaultdict(set)
            b_dias = collections.defaultdict(set)
            b_sobras = collections.Counter()
            casos = []
            col81 = []
            sem_batida = ambiguo = 0

            for cid, cels in sorted(por_pessoa.items()):
                colab = cels[0].colaborador
                emp = colab.empresa_id
                onde = "col%d_batidas" % cid
                lo = timezone.make_aware(datetime.datetime.combine(INI, datetime.time.min))
                hi = timezone.make_aware(
                    datetime.datetime.combine(FIM + UM_DIA, datetime.time.min)
                )
                batidas = list(batidas_apuraveis(colab, lo, hi))
                por_minuto = collections.defaultdict(list)
                for b in batidas:
                    local = instante_local(b)
                    if INI <= local.date() <= FIM:
                        por_minuto[chave_minuto(b)].append(b)

                onde = "col%d_turnos" % cid
                turnos = turnos_do_colab(colab, INI - UM_DIA, FIM + UM_DIA)
                por_pk = indice_turnos(turnos)

                onde = "col%d_motor" % cid
                resultado = autoridade_do_periodo(colab, INI, FIM).resultado
                publicados = getattr(resultado, "dias_cadastro_x_realidade", None) [nao verificado]
                if publicados is None: [nao verificado]
                    dias_cadastro = None
                else:
                    dias_cadastro = set()
                    for item in publicados: [nao verificado]
                        if not isinstance(item, dict) or item.get("dia") is None:
                            raise RuntimeError("dia_cadastro_invalido")
                        dias_cadastro.add(str(item["dia"])[:10])

                for cel in cels:
                    onde = "col%d_ata_%s" % (cid, cel.data.isoformat())
                    if not isinstance(cel.ata, dict):
                        # Sem ata não existe lista soberana de sobras para contar.
                        continue
                    orfas = cel.ata.get("orfas")
                    if orfas is None:
                        continue
                    if not isinstance(orfas, list):
                        raise RuntimeError("orfas_formato")

                    # A chamada conserva a fonte existente; não usa o rótulo
                    # de procedência para decidir a razão da sobra.
                    classificar_orfas({"orfas": orfas})

                    for hora in orfas:
                        if (
                            not isinstance(hora, str)
                            or len(hora) != 5
                            or hora[2] != ":"
                        ):
                            raise RuntimeError("hora_formato")
                        candidatos = por_minuto.get((cel.data, hora), [])
                        if not candidatos:
                            classe = "MINUTO_SEM_BATIDA"
                            sem_batida += 1
                            situacao = "SEM_BATIDA"
                        elif len(candidatos) >= 2:
                            classe = "MINUTO_AMBIGUO"
                            ambiguo += 1
                            situacao = "AMBIGUO"
                        else:
                            b = candidatos[0]
                            situacao = situacao_pareador(por_pk.get(b.pk, []))
                            if situacao == "PAR_FECHADO":
                                if dias_cadastro is None:
                                    classe = "CADASTRO_OU_ADICIONAL_NAO_LIDO"
                                elif cel.data.isoformat() in dias_cadastro:
                                    classe = "PERIODO_FORA_ESCALA_CADASTRO"
                                else:
                                    classe = "JORNADA_ADICIONAL"
                                b_dias[emp].add((cid, cel.data))
                                b_sobras[emp] += 1
                                casos.append(
                                    (cid, cel.data, hora, situacao, "ORFA")
                                )
                            else:
                                classe = situacao

                        a_contagem[(emp, classe)] += 1
                        a_dias[(emp, classe)].add((cid, cel.data))
                        if cid == 81 and cel.data == datetime.date(2026, 11, 1):
                            col81.append((hora, situacao, "ORFA"))

            onde = "saida"
            for (emp, classe), n in sorted(a_contagem.items()):
                print(
                    "A emp=%s classe=%s sobras=%d dias=%d"
                    % (emp, classe, n, len(a_dias[(emp, classe)]))
                )
            total = sum(a_contagem.values())
            sem_enquadramento = sum(
                n for (emp, classe), n in a_contagem.items()
                if classe == "FORA_DA_ESCALA_SEM_ENQUADRAMENTO"
            )
            print(
                "A_TOTAL sobras=%d sem_enquadramento=%d"
                % (total, sem_enquadramento)
            )
            print("A_MINUTO sem_batida=%d ambiguo=%d" % (sem_batida, ambiguo))

            for emp in sorted(b_dias):
                print(
                    "B emp=%s dias=%d sobras=%d"
                    % (emp, len(b_dias[emp]), b_sobras[emp])
                )
            print("B_LIMITACAO direcao_inversa=NAO_MEDIDA")
            for cid, dia, hora, pareador, ata in casos[:20]:
                print(
                    "B_CASO col%d %s %s pareador=%s ata=%s"
                    % (cid, dia.isoformat(), hora, pareador, ata)
                )
            if col81:
                for hora, pareador, ata in col81:
                    print(
                        "B_COL81 col81 2026-11-01 %s pareador=%s ata=%s"
                        % (hora, pareador, ata)
                    )
            else:
                print(
                    "B_COL81 col81 2026-11-01 "
                    "pareador=NAO_VERIFICADO ata=SEM_ORFA_IDENTIFICADA"
                )

        return 0
    except Exception:
        print("ERRO " + onde)
        return 1
    finally:
        print("FIM_SONDA_PASSO1")


sys.exit(main())
```

A sonda não foi executada contra a sombra e sua execução integral permanece **[não verificado]**. Também não demonstrou a classificação `PAUSA` para os casos em que `_intra_dur` mistura origens, nem a direção inversa de B; esses pontos ainda exigem uma interface que exponha a decisão correspondente ou uma instrução adicional de leitura **[não verificado]**.

## 18/11 23:30 · sessao 5af7ad33f2c3 · E4C-F6F7 · verificador · origem=Code

**Resultado desta volta: OK para a morte comportamental dos três mutantes exibidos; aprovação final da obra [não verificado].** Os tracebacks mostram uma falha de `test_sequencias` para cada mutante, além de falhas de âncora que não devem ser contadas como mordidas. A correspondência dos patches com os mutantes definidos no teste é verificável; o texto original que nomeia os casos (ii)–(iv) não foi fornecido. [anexo_logs.txt, trechos `test_sequencias`: numeração das linhas do anexo não fornecida; não verificado] [nao verificado]

1. **Patches — OK quanto à correspondência com os mutantes do teste; correspondência literal com o aval original [não verificado].** [nao verificado]

   - **(ii):** troca `while` por `if` em [tarefa_do_dia.py:161](/home/ronald/codex_ws/a555d027babd2d7318e7f86025e346eb6f91c354/app/chamados/services/tarefa_do_dia.py:161); corresponde ao mutante do caso `validação → resposta(true) → resposta(true)` em [teste:207](/home/ronald/codex_ws/a555d027babd2d7318e7f86025e346eb6f91c354/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:207).
   - **(iii):** substitui a guarda `leitura.validada_em is not None` por `True` em [tarefa_do_dia.py:163](/home/ronald/codex_ws/a555d027babd2d7318e7f86025e346eb6f91c354/app/chamados/services/tarefa_do_dia.py:163); corresponde ao mutante do caso `silêncio → resposta(true)` em [teste:209](/home/ronald/codex_ws/a555d027babd2d7318e7f86025e346eb6f91c354/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:209).
   - **(iv):** remove o teste de `resposta_mantem_validacao` do laço e amplia a guarda, alterando [tarefa_do_dia.py:161](/home/ronald/codex_ws/a555d027babd2d7318e7f86025e346eb6f91c354/app/chamados/services/tarefa_do_dia.py:161) e [tarefa_do_dia.py:163](/home/ronald/codex_ws/a555d027babd2d7318e7f86025e346eb6f91c354/app/chamados/services/tarefa_do_dia.py:163); corresponde ao mutante composto do caso `validação → resposta(false)` em [teste:211](/home/ronald/codex_ws/a555d027babd2d7318e7f86025e346eb6f91c354/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:211).

   O complemento fornecido exige um “MUTANTE proprio que morre, com log”, mas não especifica nele próprio as transformações; o aval original referido não está no dado. [aval_complemento1.txt: linha não fornecida; não verificado]

2. **Logs — OK para morte por comportamento nos três.** Em todos, o traceback chega ao `assertEqual` de resultado e ações em [teste:52](/home/ronald/codex_ws/a555d027babd2d7318e7f86025e346eb6f91c354/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:52), seguido de `AssertionError: Tuples differ` no trecho correspondente de `anexo_logs.txt`. A numeração das linhas dos logs não foi fornecida. [não verificado]

   - **(ii):** em `test_sequencias`, obteve `aguarda_validacao` e esperava `validada_sem_efeito` no caso de duas respostas que mantêm. As duas falhas de `test_cada_mutante_morde_seu_caso` são `fonte.count` em [teste:218](/home/ronald/codex_ws/a555d027babd2d7318e7f86025e346eb6f91c354/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:218), não provas adicionais de comportamento. [anexo_logs.txt, bloco ii: linha não fornecida; não verificado]
   - **(iii):** em `test_sequencias`, obteve `silencio_decidido` e esperava `aguarda_validacao`. Também há falha comportamental em [teste:149](/home/ronald/codex_ws/a555d027babd2d7318e7f86025e346eb6f91c354/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:149); as duas falhas de `test_cada_mutante...` e a de `test_mutante_adjacencia_morre` são de âncora, respectivamente em [teste:218](/home/ronald/codex_ws/a555d027babd2d7318e7f86025e346eb6f91c354/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:218) e [teste:90](/home/ronald/codex_ws/a555d027babd2d7318e7f86025e346eb6f91c354/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:90). [anexo_logs.txt, bloco iii: linha não fornecida; não verificado]
   - **(iv):** em `test_sequencias`, obteve `validada_sem_efeito` e esperava `aguarda_validacao`. As três falhas de `test_cada_mutante...` são de âncora em [teste:218](/home/ronald/codex_ws/a555d027babd2d7318e7f86025e346eb6f91c354/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:218). [anexo_logs.txt, bloco iv: linha não fornecida; não verificado]

   Os blocos exibidos não dependem de erro de importação ou fixture para essas três mortes: cada um mostra a comparação de comportamento executada. A pista anexada descreve aplicar cada mutação, salvar o diff, rodar o módulo e restaurar o arquivo; a execução e a identidade da cópia são evidência relatada pelos anexos, não verificáveis independentemente nesta árvore. [anexo_patches.txt e anexo_logs.txt: linhas não fornecidas; não verificado]

3. **(i) — OK quanto ao critério; evidência RED direta [não verificada].** As duas falhas de âncora de `MUT_ADJ.log` reduzem a contagem de falhas comportamentais atribuíveis àquele log, mas o complemento mantém para (i) a exigência de **RED na ponta atual**; não exige usar essas duas falhas de âncora para prová-lo. O opus 3 relata RED correto em `c14/red.log:86` e também nas sequências com retração e absorção em `:118` e `:150`; o próprio `red.log` não foi fornecido para conferência direta. [aval_complemento1.txt e veredito_opus3.md, itens 2–3: linhas dos anexos não fornecidas; não verificado]

4. **Mutante composto e ausência de mordida isolada — FILA pelo literal fornecido.** O mutante de (iv) altera duas condições, como mostra [teste:211](/home/ronald/codex_ws/a555d027babd2d7318e7f86025e346eb6f91c354/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:211). O literal do complemento é: **“para (ii), (iii) e (iv) a prova e MUTANTE proprio que morre, com log”** e manda conferir “que cada um de (ii)-(iv) tem mutante morto com log”; não diz, no trecho fornecido, que o mutante deva ser simples ou que cada condição tenha mutante isolado. A sobrevivência do mutante que tira *somente* `mantem` foi relatada como não rodada; continua [não verificado]. [aval_complemento1.txt e veredito_opus3.md, item 5: linhas não fornecidas; não verificado]

5. **Cobertura restante — FILA / [não verificado].** Os novos logs exercitam somente o módulo indicado e não substituem a conferência da árvore, do diff completo, da suite c18 ou dos demais itens do opus 2. O complemento proíbe tocar `.py`, pede a mesma árvore e diz que a c18 vale; esta cópia não tem metadados Git disponíveis para confirmar o hash ou comparar o diff, e o resultado da c18 não foi fornecido. Assim, **não há base aqui para declarar esses requisitos cumpridos nem para identificar uma violação concreta**. [anexo_patches.txt, pista `--only`; aval_complemento1.txt; veredito_opus3.md, seção “Nao verificado”: linhas dos anexos não fornecidas; não verificado] [nao verificado]

## 18/11 23:29 · sessao e42f51f189c2 · AUDITORIA-ESPURIAS · sonda · origem=Code

A função real é [cadastro_realidade.py:854](/home/ronald/codex_ws/68d17aa7d9433a60e334e896b7c4535f1095b64c/app/escala/services/cadastro_realidade.py:854): `lista()` combina assinaturas de cadastro e de outras lavras com a lavra do esmeril, e devolve as linhas efetivamente exibidas; ela pode devolver `None` quando não há assinaturas ([cadastro_realidade.py:960](/home/ronald/codex_ws/68d17aa7d9433a60e334e896b7c4535f1095b64c/app/escala/services/cadastro_realidade.py:960), [cadastro_realidade.py:1030](/home/ronald/codex_ws/68d17aa7d9433a60e334e896b7c4535f1095b64c/app/escala/services/cadastro_realidade.py:1030)). Ela é descrita como leitura; contudo, a própria lista pode consultar o juiz em tempo de execução para resolver donos divergentes ([cadastro_realidade.py:7](/home/ronald/codex_ws/68d17aa7d9433a60e334e896b7c4535f1095b64c/app/escala/services/cadastro_realidade.py:7), [cadastro_realidade.py:1080](/home/ronald/codex_ws/68d17aa7d9433a60e334e896b7c4535f1095b64c/app/escala/services/cadastro_realidade.py:1080)).

`ler_lavra()` lê o snapshot mais recente por `data_ref`, chave `esmeril_espelho`, no banco; na sombra, verifica o marcador da cópia de produção ([esmeril_espelho.py:291](/home/ronald/codex_ws/68d17aa7d9433a60e334e896b7c4535f1095b64c/app/ponto/services/esmeril_espelho.py:291), [esmeril_espelho.py:359](/home/ronald/codex_ws/68d17aa7d9433a60e334e896b7c4535f1095b64c/app/ponto/services/esmeril_espelho.py:359)). `anomalias_da_frota()` calcula o retrato em leitura; quem grava é `lavrar()` ([esmeril_espelho.py:215](/home/ronald/codex_ws/68d17aa7d9433a60e334e896b7c4535f1095b64c/app/ponto/services/esmeril_espelho.py:215), [esmeril_espelho.py:321](/home/ronald/codex_ws/68d17aa7d9433a60e334e896b7c4535f1095b64c/app/ponto/services/esmeril_espelho.py:321)). A autoridade da lavra mora no snapshot do banco; o host também produz `logs/sombra/lavra_prod.json` ao copiá-la para a sombra ([esmeril_espelho.py:359](/home/ronald/codex_ws/68d17aa7d9433a60e334e896b7c4535f1095b64c/app/ponto/services/esmeril_espelho.py:359), [sombra.sh:136](/home/ronald/codex_ws/68d17aa7d9433a60e334e896b7c4535f1095b64c/bin/sombra.sh:136)).

A presença dos seis colaboradores e os totais são **[não verificado]**: o dado fornecido não contém a lavra nem a saída da lista. O script abaixo não foi executado. O envelope indicado monta a árvore pela variável `SONDA_ARVORE`, mas ele próprio cria arquivos temporários e acrescenta uma trilha no host; por isso sua execução não seria compatível com a regra desta sessão de não criar nem alterar arquivos ([sonda_leitura.sh:216](/home/ronald/codex_ws/68d17aa7d9433a60e334e896b7c4535f1095b64c/bin/sonda_leitura.sh:216), [sonda_leitura.sh:226](/home/ronald/codex_ws/68d17aa7d9433a60e334e896b7c4535f1095b64c/bin/sonda_leitura.sh:226), [sonda_leitura.sh:245](/home/ronald/codex_ws/68d17aa7d9433a60e334e896b7c4535f1095b64c/bin/sonda_leitura.sh:245)).

```python
import datetime
import logging
import os
import re
import sys
from unittest.mock import patch

ONDE = "inicio"


def falhar(onde):
    print("ERRO %s" % onde)
    raise SystemExit(1)


def instante(valor):
    if not isinstance(valor, str):
        raise ValueError("instante")
    datetime.datetime.fromisoformat(valor)
    return valor


def numero(valor):
    if isinstance(valor, bool) or not isinstance(valor, (int, float)):
        raise ValueError("numero")
    return str(valor)


def erro_registrado(self, *args, **kwargs):
    # A lista captura algumas excecoes e as registra: transforma essas falhas
    # em erro da sonda, sem imprimir mensagens que possam conter dados pessoais.
    raise RuntimeError("falha capturada pela lista")


try:
    ONDE = "imports"
    from django.conf import settings
    from django.db import connection, transaction
    from ponto.services.esmeril_espelho import ler_lavra
    from escala.services.cadastro_realidade import lista

    ONDE = "guarda_sombra"
    if (
        os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra"
        or not getattr(settings, "HASNER_SOMBRA", False)
        or connection.settings_dict.get("NAME") != "sombra"
        or connection.settings_dict.get("HOST") != "sombra_db"
        or getattr(connection, "schema_name", None) != "juliani"
    ):
        falhar(ONDE)

    saida = []
    with transaction.atomic():
        ONDE = "transacao"
        with connection.cursor() as cursor:
            cursor.execute(
                "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
            )
            cursor.execute("SELECT current_database(), current_schema()")
            banco, schema = cursor.fetchone()
            if (banco, schema) != ("sombra", "juliani"):
                falhar("guarda_banco_schema")

        ONDE = "ler_lavra"
        lavra = ler_lavra()

        if lavra is None:
            saida.append("B_SEM_LAVRA")
        else:
            ONDE = "validar_lavra"
            if not isinstance(lavra, dict):
                raise ValueError("lavra")
            lavrado_em = instante(lavra["lavrado_em"])

            ONDE = "lista"
            # A lista chama ler_lavra() de novo dentro da mesma transacao,
            # portanto enxerga o mesmo snapshot do banco.
            with patch.object(logging.Logger, "exception", erro_registrado):
                dados = lista(empresas=None)

            ONDE = "validar_lista"
            if dados is None:
                linhas = []
                universo = lavra["universo"]
                listados = 0
            else:
                if dados.get("lavrado_em") != lavrado_em:
                    raise ValueError("lavra divergente")
                linhas = dados["linhas"]
                universo = dados["universo"]
                listados = dados["colabs_listados"]
                if listados != len(linhas):
                    raise ValueError("total divergente")

            if (
                isinstance(universo, bool)
                or not isinstance(universo, int)
                or universo < 0
                or isinstance(listados, bool)
                or not isinstance(listados, int)
                or listados < 0
            ):
                raise ValueError("contadores")

            por_colab = {}
            for linha in linhas:
                cid = linha["colab_id"]
                if isinstance(cid, bool) or not isinstance(cid, int):
                    raise ValueError("colab_id")
                if cid in por_colab:
                    raise ValueError("colaborador duplicado")
                por_colab[cid] = linha

            ONDE = "formatar"
            for cid in (237, 301, 451, 634, 922, 968):
                linha = por_colab.get(cid)
                if linha is None:
                    saida.append(
                        "B col%d na_lista=n linha=- lavrado_em=%s"
                        % (cid, lavrado_em)
                    )
                    continue

                assinaturas = []
                for assinatura in linha["assinaturas"]:
                    codigo = assinatura["codigo"]
                    if not isinstance(codigo, str) or not re.fullmatch(
                        r"[AC][0-9]+", codigo
                    ):
                        raise ValueError("codigo")
                    ocorrencias = assinatura.get("ocorrencias")
                    item = codigo
                    if ocorrencias is not None:
                        if (
                            isinstance(ocorrencias, bool)
                            or not isinstance(ocorrencias, int)
                            or ocorrencias < 0
                        ):
                            raise ValueError("ocorrencias")
                        item += ":%d" % ocorrencias
                    data = assinatura.get("data")
                    if data is not None:
                        data = str(data)
                        datetime.date.fromisoformat(data)
                        item += "@" + data
                    assinaturas.append(item)

                if not assinaturas:
                    raise ValueError("linha sem assinatura")

                carga = linha.get("carga")
                carga_texto = "sem_medida" if carga is None else numero(carga)

                proposta = linha.get("proposta")
                proposta_texto = "-"
                if proposta is not None:
                    if not isinstance(proposta, dict):
                        raise ValueError("proposta")
                    tipo_id = proposta["tipo_escala_id"]
                    if (
                        isinstance(tipo_id, bool)
                        or not isinstance(tipo_id, int)
                        or tipo_id <= 0
                    ):
                        raise ValueError("tipo_escala_id")
                    proposta_texto = "tipo_escala_id:%d" % tipo_id
                    confianca = proposta.get("confianca")
                    if confianca is not None:
                        # Apenas um token seguro; nunca imprime texto livre.
                        confianca = str(confianca)
                        if not re.fullmatch(r"[A-Za-z0-9_.-]{1,40}", confianca):
                            raise ValueError("confianca")
                        proposta_texto += ",confianca:" + confianca

                saida.append(
                    "B col%d na_lista=s linha=%s carga=%s proposta=%s "
                    "lavrado_em=%s"
                    % (
                        cid,
                        "/".join(assinaturas),
                        carga_texto,
                        proposta_texto,
                        lavrado_em,
                    )
                )

            saida.append(
                "B_LAVRA lavrado_em=%s universo=%d listados=%d"
                % (lavrado_em, universo, listados)
            )

    for texto in saida:
        print(texto)
    print("ERROS_B=0")
    print("FIM_SONDA_DUAS_PONTAS_B")

except SystemExit:
    raise
except Exception:
    falhar(ONDE)
```

## 18/11 23:29 · sessao 098413d357d3 · AUDITORIA-ESPURIAS · sonda · origem=Code

Não consigo entregar a sonda completa com segurança: **não verifiquei todas as chamadas transitivas nem montei e conferi o literal dos 75 pares**. Entregá-la como pronta poderia produzir causas ou donos sem a prova exigida. S3 a S6, incluindo os resultados para col950, permanecem **[não verificado]**.

As autoridades que consegui identificar na cópia são:

- `turnos_do_colab`: devolve os turnos pelo pareador, com `entrada` e `saida`; cada ponta é uma `Batida`, ligada à pergunta por `pergunta_origem`. [app/ponto/turnos.py:43](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:43), [app/ponto/turnos.py:1850](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1850), [app/ponto/models.py:40](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/models.py:40).
- `batidas_apuraveis`: fornece o universo sem batidas retratadas; `pergunta_respondida` é o juiz de resposta com instante e texto. [app/ponto/turnos.py:2868](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:2868), [app/chamados/juizes.py:1544](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/juizes.py:1544).
- `InsumosDoDono.dono` monta os insumos e chama `dono_da_divergencia`; quando precisa da lista do motor, pode obter o resultado via `espelho_do_colab`. A verificação completa de ausência de escrita nesse caminho ficou pendente. [app/ponto/management/commands/e6_oraculo.py:113](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/management/commands/e6_oraculo.py:113), [app/ponto/management/commands/e6_oraculo.py:162](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/management/commands/e6_oraculo.py:162), [app/ponto/management/commands/e6_oraculo.py:182](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/management/commands/e6_oraculo.py:182).
- `universo_da_competencia` é o filtro do fechamento. Para o TXT, a view calcula fechamentos em modo de leitura e passa os objetos a `classificar_export`; `montar_linhas_export` aplica também o filtro de status. A resposta de col950 depende dos dados da sombra. [app/ponto/services/fechamento.py:428](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/services/fechamento.py:428), [app/folha/views.py:209](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/folha/views.py:209), [app/folha/export.py:714](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/folha/export.py:714).

O par de col995 em 07/11 está no grupo misto porque o critério estrito exige **ambas** as origens `disputa_s84_retro`, enquanto o anexo informa `app` nas duas pontas **[não verificado]**. O código permite vincular uma pergunta a uma batida reaproveitada pela materialização; isso explica a possibilidade, mas não comprova que tenha sido o caminho concreto dessas duas PKs. [app/chamados/services/materializacao.py:1542](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/materializacao.py:1542).

Também não executei o envelope: ele cria uma cópia temporária da sonda e acrescenta uma trilha em arquivo, operações incompatíveis com a regra desta sessão de não criar nem alterar arquivos. O código de fechamento tem um modo `somente_leitura=True`, mas sua execução normal contém `save` e `update`; nenhuma dessas chamadas foi executada aqui. [bin/sonda_leitura.sh:216](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/bin/sonda_leitura.sh:216), [bin/sonda_leitura.sh:245](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/bin/sonda_leitura.sh:245), [app/ponto/services/fechamento.py:1128](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/services/fechamento.py:1128).

Não houve execução da sonda nem emissão de `ERROS_*=0` ou `FIM_SONDA_DUAS_PONTAS` **[não verificado]**.

## 18/11 23:27 · sessao c3985e8d6861 · DST-L2-JZ1 · leitura · origem=Code

**Perguntas de lei: 0.** O aval fornecido decide as duas alternativas da pergunta de desenho: o fecho segue cru e a correção usa uma função compartilhada que procura a pendente pelo ponteiro ou por `chamado_origem`. As questões de alcance e prova abaixo são de fechamento do desenho perante o código; não identifiquei, no material disponível, outra pergunta de lei formulada literalmente. [não verificado]

**Ressalva central:** o desenho alcança a justificativa que **já existe quando o fecho acontece**, mesmo que o ponteiro ainda não exista, desde que `chamado_origem` tenha sido gravado. Ele não alcança, por esses dois chamadores, uma justificativa que **nasça depois de concluído o fecho**. E o fixture JZ1 cria a justificativa sem `chamado_origem`: a busca alternativa não encontra esse fixture enquanto o ponteiro ainda não foi gravado. Portanto, não está demonstrado que o desenho, sozinho e sem editar o selo, faça JZ1 passar. [api/views.py:1999](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/api/views.py:1999), [test_jz1_autoridade_unica.py:41](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/tests/test_jz1_autoridade_unica.py:41), [vinculos.py:81](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/services/vinculos.py:81)

A cópia atual contém DST-L2 em `_protesta`; a descrição do anexo 7 de uma cópia que ainda lia apenas `trabalha` não corresponde a este arquivo. A cópia não contém metadados Git, portanto o HEAD informado é **[não verificado]**. Não executei testes nem acessei banco ou logs. [cartorio.py:1457](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1457)

### 1. Matriz dos três casos DST-L2 × JZ1

Em todos os casos, o fecho cru passa por `reconciliar`, cujo motor e gates podem recusar. O lote com fio pode ser adiado: a chamada compartilhada precisaria alcançar tanto o fecho imediato quanto o adiado para cobrir todos os “não protestam”. [cartorio.py:1540](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1540), [cartorio.py:1008](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1008), [models.py:642](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:642)

| Entrada e juiz | Hoje | Com o desenho decidido | Prova existente |
|---|---|---|---|
| **1. FURO vivo, ponteiro já presente, justificativa pendente; gate permite.** `_protesta` usa `dia_sem_trabalho` e marcos; `reconciliar` decide o encerramento. [cartorio.py:1462](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1462), [models.py:664](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:664) | Fecho cru: `_gravar_estado` persiste `ChamadoColaborador.status_local`, `resolvido_em` e `via_encerramento`; a transição retorna sucesso. O bloco Fix A não é chamado nesse sítio: a justificativa continua pendente por esse caminho. [cartorio.py:1546](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1546), [models.py:228](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:228) | Após fecho bem-sucedido, a função projetada encontra o ponteiro e deve persistir `Justificativa.status='rejeitada'` e a anotação em `descricao`, como o Fix A atual. O fecho continua cru: não se conclui que ganhe followup S128 ou resolver de perguntas. Retorno exato da função nova: **[não verificado]**. [reconciliador.py:569](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:569) | JZ1 exige rejeição, followup e efeitos sobre pergunta/disputa em **retratação de satélite**, não prova a nova chamada do fecho cru. **Sem prova existente identificada para o desenho.** [test_jz1_autoridade_unica.py:50](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/tests/test_jz1_autoridade_unica.py:50) |
| **2. Mesma entrada, com irmã respondida e não validada.** S128 consulta `retencao_da_retratacao`; o fecho cru consulta os gates de `reconciliar`. [reconciliador.py:439](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:439), [models.py:672](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:672) | Pela S128, BUG 31 registra retenção e retorna `(False, 'retencao')`, sem executar Fix A. Pelo cartório cru, essa retenção específica não é chamada; se o gate concreto permite o fecho, **[não verificado]**. [reconciliador.py:538](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:538) | A S128 conserva a retenção. No cartório, se o gate permitir e a função for chamada após o sucesso, ela rejeitará a pendente encontrada; se recusar, não se pode afirmar a rejeição. Resultado concreto, retorno e eventual trilha da nova função: **[não verificado]**. [reconciliador.py:563](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:563) | Há testes de retenção BUG 31, mas **sem prova identificada da combinação irmã + fecho cru + função nova**. [test_laco_hx_x_bug31.py:125](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/tests/test_laco_hx_x_bug31.py:125) |
| **3. Chamado encerrado e justificativa automática posterior.** É necessário distinguir “create iniciou antes do fecho e o signal fecha no meio” de “create começou depois do fecho”. [signals.py:161](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/signals.py:161) | No primeiro caso, o create dispara o cartório antes de `apontar_justificativa`; hoje o fecho cru não executa Fix A e a porta depois grava o ponteiro, sem rejeitar. A grade só seleciona `OPEN`. No segundo, Fix A já passou antes de a justificativa existir. [processar_alertas_turno.py:394](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/management/commands/processar_alertas_turno.py:394), [reconciliar_grade.py:207](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/management/commands/reconciliar_grade.py:207) | **Create em curso:** se a linha já tem `chamado_origem`, a busca projetada pode encontrá-la e rejeitá-la antes do ponteiro. **Nascimento realmente posterior:** os dois pontos de chamada decididos já passaram; não há efeito demonstrado. No fixture JZ1, também falta `chamado_origem` no create, de modo que a alternativa não o encontra nesse instante. Retorno: **[não verificado]**. [ponto/models.py:173](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/models.py:173), [test_jz1_autoridade_unica.py:41](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/tests/test_jz1_autoridade_unica.py:41) | JZ1 cobre o resultado esperado da grade com `--apply`, mas seu fixture não fornece a testemunha de origem. **Sem prova existente identificada para a busca por origem ou para nascimento realmente posterior.** [test_jz1_autoridade_unica.py:96](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/tests/test_jz1_autoridade_unica.py:96) |

A S128, quando alcançada e bem-sucedida, também chama o resolvedor de perguntas e anexa ocorrência; a cerca pode desfazer o ato e devolver recusa de competência entregue. Não se deve atribuir esses efeitos à função extraída, nem presumir que toda pergunta será materializada. [reconciliador.py:542](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:542), [reconciliador.py:580](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:580)

### 2. Reentrada

**Não há laço recursivo desse julgamento pelo `save` interno.** O `save(update_fields=['status', 'descricao'])` do Fix A dispara `post_save`; o receiver chama `_julgar_dias`, que consulta `cartorio.em_julgamento()` e retorna quando o cartório já está ativo. `julgar_colab` arma a flag e a limpa no `finally`. A guarda é a **flag de julgamento**, não `update_fields`, `update()` nem `on_commit`. [reconciliador.py:573](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:573), [signals.py:85](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/signals.py:85), [cartorio.py:973](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:973), [cartorio.py:1122](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1122)

Fora de um julgamento ativo, o mesmo `save` pode iniciar um julgamento síncrono; o teste do evento cobre o save de rejeição e a chamada ao dia, mas não prova o novo percurso aninhado. [signals.py:161](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/signals.py:161), [test_evento_justificativa_rejulga_o_dia.py:39](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/tests/test_evento_justificativa_rejulga_o_dia.py:39)

### 3. Tetos do chokepoint

Os tetos são limites de **queries capturadas**, cobrados por `CaptureQueriesContext` e `_cobrar`; o selo também fixa separadamente a quantidade de registros `on_commit` para cenários de batida. [test_selo_chokepoint_escrita.py:64](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/tests/test_selo_chokepoint_escrita.py:64), [test_selo_chokepoint_escrita.py:287](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/tests/test_selo_chokepoint_escrita.py:287)

| Teto | O que o selo mede | Efeito provável do desenho |
|---|---|---|
| **30** | `ch.reconciliar()` no cenário `_gravar_estado`, sem pergunta. [selo:124](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/tests/test_selo_chokepoint_escrita.py:124), [selo:361](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/tests/test_selo_chokepoint_escrita.py:361) | A chamada nova no cartório, fora de `reconciliar`, não integra diretamente essa medição. Delta real: **[não verificado]**. |
| **62** | `ch.reconciliar()` com gate que fecha FURO com dia e célula. [selo:134](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/tests/test_selo_chokepoint_escrita.py:134), [selo:394](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/tests/test_selo_chokepoint_escrita.py:394) | Mesma ressalva: a chamada externa ao método não é automaticamente contada pelo teste direto. Delta real: **[não verificado]**. |
| **224 / 288** | `registrar_batida`, respectivamente com um e três furos vivos fechados durante o julgamento síncrono. [selo:196](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/tests/test_selo_chokepoint_escrita.py:196), [selo:210](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/tests/test_selo_chokepoint_escrita.py:210), [selo:406](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/tests/test_selo_chokepoint_escrita.py:406) | **São os tetos expostos:** a busca por origem pode acrescentar consulta por chamado fechado mesmo sem justificativa; se encontrar pendente, há também escrita e receivers. Os fixtures mostrados não criam justificativa, portanto o custo depende de como a busca for implementada. Se ultrapassam os números e por quanto: **[não verificado]**, sem implementação e medição. [cartorio.py:1540](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1540), [selo:416](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/tests/test_selo_chokepoint_escrita.py:416) |

### 4. Grade e chamadores da S128

A configuração da cópia agenda `reconciliar_grade` às 06:20 para empresas 2,3,4 **sem `--apply`**. O argumento é `store_true` e a chamada a `retratar` está dentro de `if apply`: nessa linha configurada, a grade é dry. O crontab efetivamente instalado e as execuções em produção são **[não verificado]**. [crons.py:460](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/config/crons.py:460), [reconciliar_grade.py:66](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/management/commands/reconciliar_grade.py:66), [reconciliar_grade.py:303](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/management/commands/reconciliar_grade.py:303)

Chamadores de escrita encontrados na cópia:

- **Signal de batida** → `reconciliar_chamados_colab` → premissa morta → S128, sem flag `apply`. O signal de filho encerrado também aciona a mesma porta em `on_commit`. [ponto/signals.py:206](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/signals.py:206), [chamados/signals.py:351](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/signals.py:351), [reconciliador.py:1490](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:1490)
- **Consumidor de evento de célula**, cron de minuto em minuto, chama as duas varreduras que chegam à S128, sem `--apply` na linha configurada. [crons.py:394](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/config/crons.py:394), [reconciliador.py:1410](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:1410)
- **Aprovação de justificativa**, inclusive lote, chama `encerrar_chamado_de_origem` e S128 para origem viva, sem `apply`. [justificativa.py:16](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:16), [justificativa.py:217](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:217), [ponto/views.py:1840](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1840)
- **`processar_alertas_turno`**, cron a cada cinco minutos, chama S128 para turno encerrado, sem `apply` nessa linha. [crons.py:362](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/config/crons.py:362), [processar_alertas_turno.py:88](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/management/commands/processar_alertas_turno.py:88), [processar_alertas_turno.py:216](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/management/commands/processar_alertas_turno.py:216)
- **Mudança de posto** → signal `on_commit` → geofence com `aplicar=True` → S128. O comando geofence também escreve com `--apply`, mas o cron configurado usa `--alarme`, que desliga o apply. [ponto/signals.py:291](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/signals.py:291), [geofence.py:438](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/geofence.py:438), [reconciliar_geofence.py:23](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/management/commands/reconciliar_geofence.py:23), [crons.py:486](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/config/crons.py:486)
- **Comandos acionáveis com `--apply`:** `reconciliar_grade`, `reconciliar_fantasmas`, `reconciliar_vinculo` e `retratar_chamados_lei_cala`. Os crons configurados de fantasmas e vínculo usam `--alarme`, que impede apply; o último consta como lote sob ordem. [reconciliar_grade.py:305](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/management/commands/reconciliar_grade.py:305), [reconciliar_fantasmas.py:148](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/management/commands/reconciliar_fantasmas.py:148), [reconciliar_vinculo.py:29](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/management/commands/reconciliar_vinculo.py:29), [retratar_chamados_lei_cala.py:20](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/management/commands/retratar_chamados_lei_cala.py:20), [crons.py:392](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/config/crons.py:392), [crons.py:489](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/config/crons.py:489), [crons.py:1415](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/config/crons.py:1415)

### 5. Quem escreve, quem lê e semântica

Este é o censo dos **sítios relevantes encontrados**, não uma prova de exaustividade de todos os leitores indiretos do estado FURO ou de todas as telas de status. Exaustividade completa: **[não verificado]**.

- **`Justificativa.chamado_origem`:** FK opcional, não única: pode haver várias justificativas para um chamado. API e tela preenchem no create e também numa justificativa pendente existente quando a origem está vazia; o cron informa a origem nos defaults do `get_or_create`, sem preenchê-la quando reutiliza uma linha existente. Leitores encontrados incluem followup, tela e encerramento após aprovação. [ponto/models.py:173](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/models.py:173), [api/views.py:1989](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/api/views.py:1989), [ponto/views.py:1624](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1624), [processar_alertas_turno.py:394](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/management/commands/processar_alertas_turno.py:394), [chamados/utils.py:2](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/utils.py:2), [justificativa.py:33](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:33)
- **Ponteiro `ChamadoColaborador.justificativa_relacionada`:** a porta de produção encontrada é `apontar_justificativa`, chamada pelos três fluxos; ela preserva o primeiro ponteiro e não testa estado do chamado. Leem-no Fix A, retenção de justificativa alheia, gate humano de fechamento, premissa regularizada e os dois caminhos de batida que rejeitam a apontada. [vinculos.py:81](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/services/vinculos.py:81), [reconciliador.py:453](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:453), [models.py:135](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:135), [models.py:1313](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:1313), [triagem_batida.py:604](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/triagem_batida.py:604), [entrada_sincronizada.py:38](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/entrada_sincronizada.py:38)
- **`Justificativa.status`:** nasce `pendente`; API, tela e cron também explicitam esse estado. Fix A, triagem e entrada sincronizada escrevem `rejeitada`; portas de DP e lote escrevem `aprovada` ou `rejeitada`. Fila, contadores e SLA leem `pendente`; a aprovação lê a origem e o estado aprovado. [ponto/models.py:130](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/models.py:130), [reconciliador.py:575](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:575), [justificativa.py:205](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:205), [justificativa.py:277](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:277), [ponto/views.py:1739](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1739), [alertar_justificativas_sla.py:19](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/management/commands/alertar_justificativas_sla.py:19)
- **`ChamadoColaborador.status_local` do FURO:** nasce pelo fluxo de criação e transições persistem pela porta `_gravar_estado`; cartório, grade, gates e retratador consul

[truncado em 20000 de 21750 caracteres]

## 18/11 23:21 · sessao b014772ae95d · CADEIA-20-PASSO1 · sonda · origem=Code

**Não consigo entregar uma sonda que produza as medidas A e B com as restrições dadas sem inventar uma regra ou atribuir identidade que a fonte não preserva.** O script abaixo é uma **sonda de bloqueio**, não a medição pedida: verifica a conexão e aborta antes de emitir contagens. [app/escala/utils.py:631; app/escala/utils.py:673; app/ponto/turnos.py:1606]

Os pontos que impedem a medição são:

- A grade produz as órfãs como `HH:MM`, e `classificar_orfas(dia)` recebe essa lista e devolve um dicionário por hora; ela não devolve o instante ou a identidade da batida. O próprio leitor da ata reconhece que duas batidas no mesmo minuto não são individualmente endereçáveis. Isso impede garantir a comparação sobre a **mesma sobra por instante** em todos os casos. [app/escala/utils.py:631; app/escala/utils.py:673; app/escala/orfa_classe.py:53; app/ponto/turnos.py:1606]
- Não identifiquei nesta cópia uma função existente que devolva, para cada par fechado, a decisão geral “fora dos marcos” exigida por A. A regra L-084 existente em `_match_marcos` responde a uma pergunta específica de cadastro e não demonstra essa classificação geral; `dias_cadastro_x_realidade` é a lista publicada pelo motor, a ser lida. [app/escala/utils.py:610; app/ponto/motor_calculo_v2.py:289; app/ponto/motor_calculo_v2.py:2719; não verificado]
- A referência `turnos.py:1761–1765` está dentro de `papel_por_minuto_da_ata`, não identifica uma função pública chamada `PAR_DE_PAUSA_DEDUZIDO`; e `TETO_PAUSA_SEGUNDOS` vale **três horas** nesta cópia. O aval fornecido também menciona 90 minutos; não está demonstrado como resolver essa diferença para a classe PAUSA sem acrescentar interpretação. [app/ponto/turnos.py:382; app/ponto/turnos.py:1565; app/ponto/turnos.py:1761; não verificado]

Para a sonda completa, as fontes reais a integrar seriam `montar_grade_prevista_periodo` e `classificar_orfas` para a grade e suas órfãs; `batidas_apuraveis` e `parear_turnos` para o universo e os turnos; `CelulaDia.ata` para a ata gravada; e `ler_lavra` para o snapshot mais recente. `ler_lavra` lê o banco; `lavrar` e `gravar_lavra` escrevem e não devem ser chamadas. A sonda de bloqueio não chama nenhuma dessas funções, pois não pode concluir a medida pedida. [app/escala/utils.py:2095; app/escala/orfa_classe.py:53; app/ponto/turnos.py:1192; app/ponto/turnos.py:2868; app/escala/models.py:1774; app/ponto/services/esmeril_espelho.py:296; app/ponto/services/esmeril_espelho.py:317; app/ponto/services/esmeril_espelho.py:334]

A conexão real, as contagens, o caso col81 e a presença de col87 e col975 na lavra permanecem **[não verificado]**. O envelope previsto executa o Python no tenant `juliani` com `config.settings.sombra` e uma role de leitura; a configuração fixa o banco `sombra` e o host `sombra_db`. [bin/sonda_leitura.sh:225; bin/sonda_leitura.sh:235; app/config/settings/sombra.py:29; app/config/settings/sombra.py:47]

```python
# Sonda de bloqueio: não produz A, B ou C.
# Executar somente pelo envelope de leitura da sombra.

import os
import sys

from django.conf import settings
from django.db import connection


def main():
    onde = "guarda_sombra"

    try:
        if os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra":
            raise RuntimeError("settings")

        banco = settings.DATABASES["default"]
        if banco.get("NAME") != "sombra" or banco.get("HOST") != "sombra_db":
            raise RuntimeError("configuracao_do_banco")

        with connection.cursor() as cursor:
            cursor.execute("SELECT current_database(), current_schema()")
            banco_real, schema_real = cursor.fetchone()

        if banco_real != "sombra" or schema_real != "juliani":
            raise RuntimeError("conexao_ou_schema")

        # A identidade por instante das órfãs e o juiz geral de par fora
        # dos marcos não foram demonstrados nas interfaces examinadas.
        # Não emitir contagens nem inferir uma resposta.
        onde = "medicao_sem_autoridade_demonstrada"
        raise RuntimeError("sonda_incompleta")

    except Exception:
        print("ERRO " + onde)
        return 1
    finally:
        print("FIM_SONDA_PASSO1")


sys.exit(main())
```

## 18/11 22:47 · sessao a5c9a5d9b30f · E4C-F9 · verificador · origem=Code

**(a) Os dois BLOQUEIA fecham para a evidência apresentada**, com uma ressalva: não foi possível verificar independentemente o hash, o delta completo nem repetir a aplicação dos patches; a cópia disponível não tem metadados Git, e os anexos e logs foram fornecidos no enunciado [não verificado].

- **M7 — controle corrigido:** o teste cria duas perguntas respondidas e pendentes; registra resposta do ato divergente para uma e concordante para o controle, e exige que só o controle seja enviado à validação ([teste:195](/home/ronald/codex_ws/ac955a260f19b688bb674fd9d1ce4ebc3bddcf42/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:195)). A lista real exclui perguntas validadas pelo ato e exige resposta, ausência de materialização e ausência de trava ([models.py:2003](/home/ronald/codex_ws/ac955a260f19b688bb674fd9d1ce4ebc3bddcf42/app/chamados/models.py:2003)).
- **Evidência de mutação:** os patches e os `AssertionError` fornecidos identificam os sete mutantes e as falhas; para M7, uso o `MUT2_M7`, posterior à correção, cuja remoção declarada corresponde à guarda presente na cópia ([validar_classe_a.py:63](/home/ronald/codex_ws/ac955a260f19b688bb674fd9d1ce4ebc3bddcf42/app/chamados/management/commands/validar_classe_a.py:63); anexos fornecidos [não verificado]).

**(b) Sim: os sete morrem em teste do próprio sítio, além de eventual selo**, segundo os logs fornecidos [não verificado]. [nao verificado]

| Mutante | Teste funcional e efeito registrado | [nao verificado]
|---|---|
| M1 | O teste da contraprova encontra indevidamente `validada_em 1` para a divergente ([teste:93](/home/ronald/codex_ws/ac955a260f19b688bb674fd9d1ce4ebc3bddcf42/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:93); log fornecido [não verificado]). |
| M2–M5 | Cada teste do auditor inteiro passa de um para dois IDs na violação do seu sítio; há também falha individual do predicado extraído ([teste:134](/home/ronald/codex_ws/ac955a260f19b688bb674fd9d1ce4ebc3bddcf42/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:134); logs fornecidos [não verificado]). |
| M6 | O dry-run conta quatro em vez de duas, e o apply altera a via do controle validado ([teste:107](/home/ronald/codex_ws/ac955a260f19b688bb674fd9d1ce4ebc3bddcf42/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:107); log fornecido [não verificado]). |
| M7 | Sem a guarda, o teste de escolha registra duas validações e não encontra a divergência; o teste de texto também falha ([teste:178](/home/ronald/codex_ws/ac955a260f19b688bb674fd9d1ce4ebc3bddcf42/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:178); `MUT2_M7` fornecido [não verificado]). |

**(c) BLOQUEIA:** nenhum novo identificado no trecho do delta fornecido [não verificado].

**FILA — cobertura integrada de M7:** o teste injeta por `lambda` a lista `[p, controle]`; portanto prova o comportamento do comando com essa entrada, mas não prova que as duas perguntas concretas passam pela lista real, inclusive pela trava ([teste:206](/home/ronald/codex_ws/ac955a260f19b688bb674fd9d1ce4ebc3bddcf42/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:206); [models.py:2003](/home/ronald/codex_ws/ac955a260f19b688bb674fd9d1ce4ebc3bddcf42/app/chamados/models.py:2003)). Isso não invalida a mordida da guarda demonstrada pelo mutante fornecido [não verificado].

Os casos M2–M5 não passam vazios: exigem exatamente o ID divergente, e os logs fornecidos mostram o controle adicional após cada mutação ([teste:162](/home/ronald/codex_ws/ac955a260f19b688bb674fd9d1ce4ebc3bddcf42/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:162); logs [não verificado]). Os mocks do auditor isolam outras decisões, mas não substituem o filtro de validação testado ([teste:154](/home/ronald/codex_ws/ac955a260f19b688bb674fd9d1ce4ebc3bddcf42/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:154); [auditor:1586](/home/ronald/codex_ws/ac955a260f19b688bb674fd9d1ce4ebc3bddcf42/app/chamados/management/commands/auditar_invariantes_chamados.py:1586)). O relógio da classe está congelado em `AGORA`, posterior às datas das fixtures ([teste:17](/home/ronald/codex_ws/ac955a260f19b688bb674fd9d1ce4ebc3bddcf42/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:17); [teste:59](/home/ronald/codex_ws/ac955a260f19b688bb674fd9d1ce4ebc3bddcf42/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:59); [apoio:30](/home/ronald/codex_ws/ac955a260f19b688bb674fd9d1ce4ebc3bddcf42/app/chamados/tests/cortes_f2_f9_apoio.py:30)).

## 18/11 22:44 · sessao da9902bd730b · E4C-F9 · verificador · origem=Code

**BLOQUEIA: M7 usa um controle validado que o fluxo real não entregaria ao comando.** O teste força a disputa a devolver a pergunta validada e força `validar_pergunta` a aceitá-la; assim, a chamada esperada para o controle não demonstra o comportamento integrado exigido pelo aval. O seletor real exclui perguntas validadas pelo ato. ([teste:195](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:195), [models.py:2003](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/models.py:2003), [validacao.py:685](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/services/validacao.py:685))

**BLOQUEIA: a evidência fornecida não permite certificar os logs exigidos.** O anexo textual relata GREEN e falhas para M1–M7, mas não traz os patches dos mutantes, os tracebacks nem o log Django do RED. Também relata `validar_classe_a.py` modificado ao final; não mostra o conteúdo dessa modificação ou se ela estava presente em cada execução. A cópia não dispõe de metadados Git para conferir o hash ou o delta. Portanto, morte *pelo mutante “sem o filtro do sítio”*, RED em arquivo e correspondência exata das execuções com a árvore permanecem **[não verificado]**.

O anexo `delta.diff` apresentado declara alteração somente no teste; a conformidade dele com os commits indicados é **[não verificado]**. Não executei testes: a instrução era somente leitura e o teste M6 faz escrita em banco de teste. ([teste:120](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:120))

**1. Sete sítios e casos**

- **M1 — atende por leitura do código.** `_contraprova_f1t` rotula pela validação lida do ato; o teste verifica divergente e sem validação fora do rótulo e validada dentro dele. O mock de pergunta viva abre o caminho até o rótulo; não substitui a leitura examinada. ([auditor:202](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/management/commands/auditar_invariantes_chamados.py:202), [teste:93](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:93))

- **M2 — atende por leitura:** I21 filtra não validadas pelo ato e o auditor é chamado inteiro; a saída exige precisamente o ID divergente e exclui o controle validado. ([auditor:1586](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/management/commands/auditar_invariantes_chamados.py:1586), [teste:138](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:138))

- **M3 — atende por leitura:** C2 aplica o mesmo filtro em perguntas não respondidas com célula; o caso cria a célula e verifica a linha C2. ([auditor:1615](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/management/commands/auditar_invariantes_chamados.py:1615), [teste:141](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:141))

- **M4 — atende por leitura:** F2 filtra perguntas de disputas fechadas; o caso fecha as duas disputas e verifica a linha F2. ([auditor:1780](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/management/commands/auditar_invariantes_chamados.py:1780), [teste:144](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:144))

- **M5 — atende por leitura:** F3 aplica o filtro no universo dos motivos da lei; o caso dá motivos distintos às perguntas e verifica a linha F3. ([auditor:1792](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/management/commands/auditar_invariantes_chamados.py:1792), [teste:149](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:149))

- **M6 — atende por leitura:** o dry-run conta dois não validados, inclusive o divergente, e exclui duas validações pelo ato; o apply confere a via do divergente, a via preservada do controle e o total gravado. O filtro está na seleção anterior aos mocks. ([comando:31](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/management/commands/encerrar_perguntas_com_fato.py:31), [teste:107](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:107))

- **M7 — BLOQUEIA a cobertura integrada do controle validado.** O primeiro caso testa uma resposta do ato diferente da projeção e um controle sem validação; verifica que a divergência é contada e que o controle aparece. O segundo tem controle validado, mas o introduz por lista falsa e simula sucesso da validação, apesar de o fluxo real excluí-lo. As asserções sobre divergência de texto não são vazias, mas não demonstram o caminho real do controle validado pedido no aval. ([comando:54](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/management/commands/validar_classe_a.py:54), [comando:63](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/management/commands/validar_classe_a.py:63), [teste:178](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:178), [teste:195](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:195))

Para M2–M5, há ainda teste isolado que extrai de cada sítio o predicado de validação e verifica inclusão do divergente e exclusão da validada. A evidência de execução efetiva dos mutantes permanece **[não verificado]** além do resumo fornecido. ([teste:23](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:23), [teste:78](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:78))

**2. Mocks do auditor**

**Sem BLOQUEIA identificado no filtro dos sítios M2–M5.** Os quatro mocks facilitam critérios posteriores ou distintos: `_cobre_ausencia_i21` impede a separação para o piso I21a; `_piso_f3_antes_do_gerador` encaminha candidatos à violação; `pergunta_viva=True` satisfaz a guarda de F2/F3; `classificar_falta` faz a lei calar em F3. Nenhum substitui `q_nao_validada_pelo_ato()` nos quatro `filter` que selecionam os candidatos. As asserções exigem contagem e ID, de modo que a inclusão indevida do controle alteraria a linha esperada. ([teste:153](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:153), [auditor:1592](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/management/commands/auditar_invariantes_chamados.py:1592), [auditor:1781](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/management/commands/auditar_invariantes_chamados.py:1781), [auditor:1843](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/management/commands/auditar_invariantes_chamados.py:1843))

**3. Transação de M6**

**Sem BLOQUEIA identificado:** `ComandosLeemAtoTest` herda de `BaseCortes`, que herda de Django `TestCase`; o `--apply` é chamado dentro desse teste e o comando ainda usa `transaction.atomic()` nas escritas. O isolamento efetivo da execução relatada depende da configuração e do ambiente usados, **[não verificado]**. ([teste:59](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:59), [apoio:37](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/tests/cortes_f2_f9_apoio.py:37), [comando:66](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/management/commands/encerrar_perguntas_com_fato.py:66))

**4. Escritores, leitores e significado**

| Campo | Escrita relevante | Leitura nos três comandos e correspondência com o teste | [nao verificado]
|---|---|---|
| `validada_em` | A porta de validação grava a coluna e registra o ato; há outros escritores, inclusive `carimbar`. ([validacao.py:161](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/services/validacao.py:161), [models.py:3330](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/models.py:3330)) | Auditor usa leitura do ato em M1 e predicado SQL em M2–M5; encerrar usa o predicado em M6; validar recebe respostas sem veredito pela disputa. Os helpers criam ato e projeção para exercitar essa distinção. ([auditor:206](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/management/commands/auditar_invariantes_chamados.py:206), [models.py:3539](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/models.py:3539), [teste:61](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:61)) |
| `via_resolucao` | A porta geral atribui junto ao carimbo; M6 usa a porta específica que altera e salva somente a via. ([models.py:3293](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/models.py:3293), [models.py:3489](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/models.py:3489)) | Auditor lê a via, inclusive em M1; encerrar seleciona e relata a via e, em apply, chama a porta de escrita. O teste M6 confere a via, coerentemente com essa porta. Em `validar_classe_a`, leitura direta da via **não identificada**. ([auditor:203](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/management/commands/auditar_invariantes_chamados.py:203), [comando:42](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/management/commands/encerrar_perguntas_com_fato.py:42), [teste:128](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:128)) |
| `materializada_em` | A porta `_escrever_carimbo` atribui o instante; M6 exige a coluna preenchida e sua porta específica não altera esse campo. ([models.py:3293](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/models.py:3293), [models.py:3489](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/models.py:3489)) | Auditor e encerrar leem a coluna; a lista real de respostas sem veredito também a consulta para `validar_classe_a`. O teste M6 fornece `materializada_em=T1`, coerente com o universo selecionado. ([auditor:1586](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/management/commands/auditar_invariantes_chamados.py:1586), [encerrar:33](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/management/commands/encerrar_perguntas_com_fato.py:33), [models.py:2003](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/models.py:2003)) |
| `resposta_colab` | A resposta humana escreve texto e instante; a sequência registra o texto do ato. ([disputa_emissao.py:1474](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/services/disputa_emissao.py:1474), [models.py:4295](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/models.py:4295)) | Auditor consulta a resposta, inclusive na inspeção crua I20 e pelo predicado de resposta em I21. `validar_classe_a` compara ato e projeção antes de classificar e imprime o texto do ato; o classificador de escolha lê a projeção após essa guarda. O teste M7 exercita essa diferença, com a limitação do controle acima. Leitura direta por encerrar **não identificada**. ([auditor:1114](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/management/commands/auditar_invariantes_chamados.py:1114), [validar:63](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/management/commands/validar_classe_a.py:63), [juizes.py:2041](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/juizes.py:2041)) |

A tabela aponta escritores relevantes para os caminhos examinados; um censo exaustivo de todos os escritores dos quatro campos é **[não verificado]**.

**5. Alcance**

**FILA — alcance do delta:** o diff fornecido mostra apenas o arquivo de teste, mas sua correspondência com os commits é **[não verificado]**. O teste chama comandos e juízes existentes; o auditor consulta competências exportadas, portanto há leitura ligada a exportação durante sua execução. O comando também possui opção `--html` que escreve arquivo, não passada pelos testes examinados. Alteração de folha, exportação ou dinheiro pelo delta fornecido não aparece; efeitos transitivos completos das execuções são **[não verificado]**. ([teste:154](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:154), [auditor:372](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/management/commands/auditar_invariantes_chamados.py:372), [auditor:1858](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/management/commands/auditar_invariantes_chamados.py:1858))

**6. Relógio e imports**

**Sem FILA identificada no teste examinado:** a classe congela o tempo em `AGORA`, e `T1`/`T2` são instantes fixos; os imports do arquivo têm uso visível. O auditor consulta relógio, inclusive para `_hoje`, sob esse congelamento quando chamado por esses testes. Relógios e imports em toda a árvore estão **[não verificado]**. ([teste:2](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:2), [teste:17](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:17), [teste:59](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:59), [auditor:1606](/home/ronald/codex_ws/8d3b99d8828274c7c8177faea872740deb29b3b1/app/chamados/management/commands/auditar_invariantes_chamados.py:1606))

_7192 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
