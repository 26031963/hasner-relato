# CODEX-ESTADO

> Ultima resposta processada: **18/11/2026 23:21** · publicado em **18/11/2026 23:22** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

## 18/11 22:36 · sessao 57f4dfc992d1 · E4C-F9 · construtor · origem=Code

Atualizei [test_e4c_f9_comandos_leem_o_ato.py](/home/ronald/wt-codex-e4c-f9-223418/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:70) com controles validados pelo ato, execução integral do auditor para M2–M5 e cenário `--apply` para M6. O cenário `--apply` de M7 usa a pergunta divergente e um controle validado ([teste](/home/ronald/wt-codex-e4c-f9-223418/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:193)). [nao verificado]

**Entrega incompleta:** não executei os testes Django nem os mutantes, pois a instrução proíbe acesso a banco. Portanto, as mortes e a ausência de testes vermelhos no código atual são **[não verificado]**; também não há log RED Django. A análise sintática com `python3` passou; Ruff não está instalado **[não verificado: lint]**. Os patches abaixo são propostas para a sessão executar, não mutações aplicadas. [nao verificado]

Para cada mutante, aplicar seu patch isoladamente com `patch -p1` a partir de `/home/ronald/wt-codex-e4c-f9-223418` e rodar o comando indicado no ambiente autorizado. Os comandos abaixo assumem, **[não verificado]**, que `python` e as configurações Django de teste estão disponíveis: [nao verificado]

```bash
cd /home/ronald/wt-codex-e4c-f9-223418/app
export PYTHONDONTWRITEBYTECODE=1
export DJANGO_SETTINGS_MODULE=config.settings_test
```

**M1** — deve morrer `test_M1_rotulo_e_controle` ([teste](/home/ronald/wt-codex-e4c-f9-223418/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:93)) **[não verificado]**. [nao verificado]

```diff
--- a/app/chamados/management/commands/auditar_invariantes_chamados.py
+++ b/app/chamados/management/commands/auditar_invariantes_chamados.py
@@ -206,1 +206,1 @@
-            degraus['validada_em' if DecisaoPergunta.leitura(_p).validada_em is not None
+            degraus['validada_em' if _p.validada_em is not None
```

```bash
python manage.py test chamados.tests.test_e4c_f9_comandos_leem_o_ato.ComandosLeemAtoTest.test_M1_rotulo_e_controle
```

**M2** — deve morrer `test_M2_auditor_inteiro` ([teste](/home/ronald/wt-codex-e4c-f9-223418/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:164)) **[não verificado]**. [nao verificado]

```diff
--- a/app/chamados/management/commands/auditar_invariantes_chamados.py
+++ b/app/chamados/management/commands/auditar_invariantes_chamados.py
@@ -1586,1 +1586,1 @@
-        _i21_cru = list(PD.objects.filter(q_respondida_com_texto(), PD.q_nao_validada_pelo_ato(),
+        _i21_cru = list(PD.objects.filter(q_respondida_com_texto(),
```

```bash
python manage.py test chamados.tests.test_e4c_f9_comandos_leem_o_ato.ComandosLeemAtoTest.test_M2_auditor_inteiro
```

**M3** — deve morrer `test_M3_auditor_inteiro` ([teste](/home/ronald/wt-codex-e4c-f9-223418/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:167)) **[não verificado]**. [nao verificado]

```diff
--- a/app/chamados/management/commands/auditar_invariantes_chamados.py
+++ b/app/chamados/management/commands/auditar_invariantes_chamados.py
@@ -1617,1 +1617,1 @@
-            PD.objects.filter(_q_nao_resp(), PD.q_nao_validada_pelo_ato(), materializada_em__isnull=True,
+            PD.objects.filter(_q_nao_resp(), materializada_em__isnull=True,
```

```bash
python manage.py test chamados.tests.test_e4c_f9_comandos_leem_o_ato.ComandosLeemAtoTest.test_M3_auditor_inteiro
```

**M4** — deve morrer `test_M4_auditor_inteiro` ([teste](/home/ronald/wt-codex-e4c-f9-223418/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:170)) **[não verificado]**. [nao verificado]

```diff
--- a/app/chamados/management/commands/auditar_invariantes_chamados.py
+++ b/app/chamados/management/commands/auditar_invariantes_chamados.py
@@ -1781,1 +1781,1 @@
-            [p.pk for p in PD.objects.filter(PD.q_nao_validada_pelo_ato(), disputa__fechada_em__isnull=False,
+            [p.pk for p in PD.objects.filter(disputa__fechada_em__isnull=False,
```

```bash
python manage.py test chamados.tests.test_e4c_f9_comandos_leem_o_ato.ComandosLeemAtoTest.test_M4_auditor_inteiro
```

**M5** — deve morrer `test_M5_auditor_inteiro` ([teste](/home/ronald/wt-codex-e4c-f9-223418/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:173)) **[não verificado]**. [nao verificado]

```diff
--- a/app/chamados/management/commands/auditar_invariantes_chamados.py
+++ b/app/chamados/management/commands/auditar_invariantes_chamados.py
@@ -1794,1 +1794,1 @@
-        for p in (PD.objects.filter(PD.q_nao_validada_pelo_ato(), data_evento__isnull=False, motivo__in=_mlei,
+        for p in (PD.objects.filter(data_evento__isnull=False, motivo__in=_mlei,
```

```bash
python manage.py test chamados.tests.test_e4c_f9_comandos_leem_o_ato.ComandosLeemAtoTest.test_M5_auditor_inteiro
```

**M6** — devem morrer os testes dry e `--apply` ([teste](/home/ronald/wt-codex-e4c-f9-223418/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:107)) **[não verificado]**. [nao verificado]

```diff
--- a/app/chamados/management/commands/encerrar_perguntas_com_fato.py
+++ b/app/chamados/management/commands/encerrar_perguntas_com_fato.py
@@ -33,1 +33,1 @@
-              .filter(PerguntaDisputa.q_nao_validada_pelo_ato(), materializada_em__isnull=False)
+              .filter(materializada_em__isnull=False)
```

```bash
python manage.py test chamados.tests.test_e4c_f9_comandos_leem_o_ato.ComandosLeemAtoTest.test_M6_dry_run_universo_e_controle chamados.tests.test_e4c_f9_comandos_leem_o_ato.ComandosLeemAtoTest.test_M6_apply_carimba_apenas_nao_validada
```

**M7** — deve morrer `test_M7_divergencia_escolha_nao_valida` ([teste](/home/ronald/wt-codex-e4c-f9-223418/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:193)) **[não verificado]**. [nao verificado]

```diff
--- a/app/chamados/management/commands/validar_classe_a.py
+++ b/app/chamados/management/commands/validar_classe_a.py
@@ -63,3 +63,0 @@
-            if DecisaoPergunta.leitura(q).resposta_colab != q.resposta_colab:
-                cls['divergente ato x projecao'] += 1
-                continue
```

```bash
python manage.py test chamados.tests.test_e4c_f9_comandos_leem_o_ato.ComandosLeemAtoTest.test_M7_divergencia_escolha_nao_valida
```

## 18/11 22:33 · sessao 88b1c8259ae5 · E4C-F9 · verificador · origem=Code

**AVAL-DEVOLVIDO**

**Item 1 — PROCEDE (devolver).** A cura precisa especificar os critérios por sítio: nem todos têm um filtro de validação, e M7 exige um tratamento diferente.

1. **Premissas.** Conferem a ausência de pergunta validada em M6 e o teste isolado do predicado em M2–M5. M1 também não tem controle validado. Já a afirmação de que M6 é o único cujo comando grava com `--apply` não fecha: M7 também oferece `--apply` e chama `validar_pergunta`. [teste:70](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:70), [teste:85](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:85), [teste:96](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:96), [validar_classe_a.py:32](app/chamados/management/commands/validar_classe_a.py:32), [validar_classe_a.py:100](app/chamados/management/commands/validar_classe_a.py:100).

   Os sete sítios são:

   | Sítio | Leitura ou filtro |
   |---|---|
   | M1 | Ramo que rotula conforme validação lida do ato, não filtro ORM: [auditor:206](app/chamados/management/commands/auditar_invariantes_chamados.py:206). |
   | M2 | I21, `q_nao_validada_pelo_ato()`: [auditor:1586](app/chamados/management/commands/auditar_invariantes_chamados.py:1586). |
   | M3 | C2, mesmo predicado: [auditor:1617](app/chamados/management/commands/auditar_invariantes_chamados.py:1617). |
   | M4 | F2, mesmo predicado: [auditor:1781](app/chamados/management/commands/auditar_invariantes_chamados.py:1781). |
   | M5 | F3, mesmo predicado: [auditor:1794](app/chamados/management/commands/auditar_invariantes_chamados.py:1794). |
   | M6 | Seleção de materializadas não validadas: [encerrar:33](app/chamados/management/commands/encerrar_perguntas_com_fato.py:33). |
   | M7 | Guarda de divergência de **resposta** entre ato e projeção; há outra leitura para impressão: [validar_classe_a.py:63](app/chamados/management/commands/validar_classe_a.py:63), [validar_classe_a.py:83](app/chamados/management/commands/validar_classe_a.py:83). |

2. **Colisão com o aval vigente.** Não há mudança de F9=(A) nem autorização para tocar os comandos além do RED no texto fornecido; a execução só respeita esse limite se eventuais alterações dos comandos ficarem dentro do que o RED exigir. A existência e o teor independente do aval vigente na cópia são **[não verificado]**; a regra foi fornecida no anexo `aval.txt`, sem numeração de linhas.

3. **Literal.** M1 é um ramo de classificação, e M7 é uma guarda de divergência de resposta: “sem o filtro do sítio” não identifica uma mutação uniforme para os sete. Remover a guarda de M7 é uma mutação definível e, por inspeção, os testes que exigem o contador de divergência devem fazê-la morrer; sua execução é **[não verificado]**. Trocar a leitura da impressão pela projeção é outra mutação: a guarda anterior já exige igualdade para os itens impressos, e o selo estrutural conta a leitura crua. [auditor:206](app/chamados/management/commands/auditar_invariantes_chamados.py:206), [validar_classe_a.py:62](app/chamados/management/commands/validar_classe_a.py:62), [teste:105](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:105), [teste:194](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:194).

   M2–M5 pertencem ao mesmo auditor, que pode ser executado inteiro em modo de leitura por padrão, mas não declara flag `--dry` nem `--apply`; `--html` escreve arquivo e não deve ser usado sob a regra desta revisão. [auditor:1](app/chamados/management/commands/auditar_invariantes_chamados.py:1), [auditor:552](app/chamados/management/commands/auditar_invariantes_chamados.py:552), [auditor:1869](app/chamados/management/commands/auditar_invariantes_chamados.py:1869).

4. **Exportada, dinheiro ou `!`.** O item descreve testes e mutantes; não declara alteração de exportação ou dinheiro. Se uma implementação concreta tocaria esses domínios ou algum `!` fora do declarado é **[não verificado]**; não há delta proposto para examinar. Os comandos de M6 e M7 têm caminhos de escrita quando aplicados, por isso testes precisam respeitar o escopo autorizado. [encerrar:54](app/chamados/management/commands/encerrar_perguntas_com_fato.py:54), [validar_classe_a.py:88](app/chamados/management/commands/validar_classe_a.py:88).

**Item 2 — NAO PROCEDE.** A exigência de guardar o RED Django em arquivo é compreensível e, no texto fornecido, não apresenta por si só colisão com F9=(A) ou com o limite dos comandos.

1. **Premissas.** O anexo `veredito_opus.md` fornecido afirma que o RED da c3 ficou declarado sem log Django. A existência e o conteúdo de um log independente são **[não verificado]**; o anexo fornecido não tem linhas para citar.
2. **Colisão.** Guardar a saída de um RED não exige, por si só, mudar F9=(A). Alterações dos três comandos continuariam limitadas ao que o RED exigir, conforme `aval.txt` fornecido; confirmação independente do aval vigente é **[não verificado]**.
3. **Literal.** “RED com log Django guardado em arquivo” é executável em princípio, mas o comando, a falha observada e o conteúdo do futuro log são **[não verificado]**.
4. **Exportada, dinheiro ou `!`.** O item declara um artefato de log, sem declarar alteração nesses domínios; efeitos de uma execução concreta são **[não verificado]**.

A revisão foi somente por leitura. Nenhum teste ou mutante foi executado, nenhum arquivo foi criado, e a correspondência da cópia ao hash informado é **[não verificado]**. [nao verificado]

## 18/11 22:33 · sessao 49baaaf366a7 · - · - · origem=Ronald (tmux)

- A premissa de que M6 é o único comando que grava com `--apply` é falsa: M7 também aceita a opção e chama a porta de validação ([validar_classe_a.py:32](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/management/commands/validar_classe_a.py:32), [validar_classe_a.py:100](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/management/commands/validar_classe_a.py:100), [validacao.py:661](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/services/validacao.py:661)).
- O auditor de M2–M5 declara leitura sem escrita em dados e não oferece `--dry` nem `--apply`; `--html` escreve um arquivo. A execução sem `--html` é o modo de leitura existente ([auditar_invariantes_chamados.py:1](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/management/commands/auditar_invariantes_chamados.py:1), [auditar_invariantes_chamados.py:552](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/management/commands/auditar_invariantes_chamados.py:552), [auditar_invariantes_chamados.py:1858](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/management/commands/auditar_invariantes_chamados.py:1858)).
- O teste do selo monta mutações da fonte em memória; isso não comprova a existência ou ausência do log Django histórico ([test_e4c_f9_comandos_leem_o_ato.py:207](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:207)). Log histórico: [não verificado].
- A decisão F9=(A), o limite de alterações e o conteúdo dos anexos não foram localizados como arquivos na cópia; como informação fornecida no enunciado, sua vigência independente é [não verificado].
- Uma alteração futura pode ser avaliada quanto a exportada, dinheiro e `!` somente com o delta concreto: [não verificado]. A porta de validação menciona a proteção da competência exportada ([validacao.py:688](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/services/validacao.py:688)).

## 18/11 22:33 · sessao b1328dde7609 · - · - · origem=Ronald (tmux)

- O sítio M7 é a guarda de divergência entre ato e projeção em [validar_classe_a.py:62](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/management/commands/validar_classe_a.py:62); a leitura usada na impressão é outra operação, em [validar_classe_a.py:80](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/management/commands/validar_classe_a.py:80).
- Na lista real, uma pergunta com validação vigente é excluída antes de chegar à guarda M7, em [models.py:2003](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/models.py:2003); o teste atual substitui essa lista por um mock em [test_e4c_f9_comandos_leem_o_ato.py:110](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:110).
- Remover a guarda é um mutante definível; por inspeção, os testes atuais exigem o contador que ela incrementa e falhariam sem ela, em [test_e4c_f9_comandos_leem_o_ato.py:118](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:118) e [test_e4c_f9_comandos_leem_o_ato.py:136](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:136). Execução do mutante: [não verificado].
- `validar_classe_a` também aceita `--apply` e chama a porta de validação, em [validar_classe_a.py:32](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/management/commands/validar_classe_a.py:32) e [validar_classe_a.py:100](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/management/commands/validar_classe_a.py:100); a porta pode salvar a validação e registrar o ato em [validacao.py:161](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/services/validacao.py:161) e [validacao.py:729](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/services/validacao.py:729).

## 18/11 22:33 · sessao c6f25b7ba7ed · - · - · origem=Ronald (tmux)

Inspeção concluída e comunicada ao agente principal, com os sete sítios, os testes existentes e as condições literais de dry e mutação. [nao verificado]

## 18/11 22:31 · sessao 6ebd0212ed71 · ESCALA-NAO-DESCREVE · verificador · origem=Code

**Veredito: FILA para corrigir a caracterização dos predicados; nenhum BLOQUEIA comprovado na linha da parte 2(b).** A árvore disponível não contém metadados Git: HEAD, commit vazio e igualdade com `a179c141e` ficam **[não verificado]**. A execução da suíte e o conteúdo do RED também ficam **[não verificado]**.

1. **Linha LEI-AKITA — FILA, por precisão em `juizes novos=0`.**

   - **`origem`: confere.** `resumo_da_carga` existe e monta a frase e seus componentes; `lista` publica seu resultado em `carga_resumo` ([cadastro_realidade.py:788](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/escala/services/cadastro_realidade.py:788), [cadastro_realidade.py:1067](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/escala/services/cadastro_realidade.py:1067)).
   - **`testemunha`: confere.** A lista monta `carga` e `carga_desde` a partir de `orfas` e `orfas_desde` da lavra; o esmeril escreve esses mapas a partir da medida e do início da janela ([cadastro_realidade.py:969](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/escala/services/cadastro_realidade.py:969), [cadastro_realidade.py:981](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/escala/services/cadastro_realidade.py:981), [esmeril_espelho.py:236](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/ponto/services/esmeril_espelho.py:236)).
   - **`quem-mais-le`: confere para leitores de produção encontrados por busca textual.** São o template e `pdf_bytes`; testes também leem a chave ([cadastro_x_realidade.html:17](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/templates/escala/cadastro_x_realidade.html:17), [cadastro_realidade.py:1184](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/escala/services/cadastro_realidade.py:1184), [test_escala_nao_descreve.py:252](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/escala/tests/test_escala_nao_descreve.py:252)). Leitores indiretos ou dinâmicos não identificáveis por essa busca: **[não verificado]**.
   - **`RED`: o log e seus três erros são [não verificado].** Existem três testes nomeados `test_MORDE_R2b...`; qual deles o log cobriu é **[não verificado]** ([test_escala_nao_descreve.py:246](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/escala/tests/test_escala_nao_descreve.py:246), [test_escala_nao_descreve.py:258](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/escala/tests/test_escala_nao_descreve.py:258), [test_escala_nao_descreve.py:265](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/escala/tests/test_escala_nao_descreve.py:265)).

2. **`juizes novos=0` — FILA para corrigir “predicados locais de tela”.** `botao_da_proposta` também é consultado no POST e pode impedir a escrita; depois disso a view chama a porta do vínculo ([views.py:475](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/escala/views.py:475), [views.py:498](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/escala/views.py:498), [views.py:502](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/escala/views.py:502)). Há sobreposição concreta: o predicado e a porta recusam destino cíclico ([cadastro_realidade.py:849](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/escala/services/cadastro_realidade.py:849), [regua_defesa.py:313](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/escala/regua_defesa.py:313)). Não identifiquei, nessa comparação, outro juiz **registrado** respondendo à mesma pergunta completa; o registro distingue a pergunta sobre substituir proposta da guarda da porta ([juizes.py:1375](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/core/juizes.py:1375)). `folga_trabalhada_no_6x1` é chamado pela ação e pelo predicado do botão, chegando assim também ao POST ([cadastro_realidade.py:718](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/escala/services/cadastro_realidade.py:718), [cadastro_realidade.py:845](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/escala/services/cadastro_realidade.py:845), [views.py:498](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/escala/views.py:498)).

3. **`padrao=fonte unica` — nada.** O termo existe como alias de *Single Source of Truth* no glossário; o código produz uma frase em `resumo_da_carga` e tela e PDF leem essa frase ([GLOSSARIO.md:11](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/docs/GLOSSARIO.md:11), [cadastro_realidade.py:801](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/escala/services/cadastro_realidade.py:801), [cadastro_x_realidade.html:17](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/templates/escala/cadastro_x_realidade.html:17), [cadastro_realidade.py:1184](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/escala/services/cadastro_realidade.py:1184)). É uma descrição compatível com a realização de 2(b); não há evidência aqui de que outro padrão seja necessário.

4. **Escritores e leitores — nada, com escopo desta carga.**

   - **`carga_resumo`:** `lista` escreve; template e PDF leem; testes também leem ([cadastro_realidade.py:1067](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/escala/services/cadastro_realidade.py:1067), [cadastro_x_realidade.html:17](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/templates/escala/cadastro_x_realidade.html:17), [cadastro_realidade.py:1184](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/escala/services/cadastro_realidade.py:1184), [test_escala_nao_descreve.py:252](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/escala/tests/test_escala_nao_descreve.py:252)).
   - **`carga` e `carga_desde`:** `lista` escreve nas linhas; `resumo_da_carga` lê ambas e a ordenação lê `carga`; o template também lê `carga` ([cadastro_realidade.py:981](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/escala/services/cadastro_realidade.py:981), [cadastro_realidade.py:796](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/escala/services/cadastro_realidade.py:796), [cadastro_realidade.py:1056](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/escala/services/cadastro_realidade.py:1056), [cadastro_x_realidade.html:29](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/templates/escala/cadastro_x_realidade.html:29)). A apresentação textual por linha é montada como `carga_texto`, lida pela tela e pelo PDF ([cadastro_realidade.py:984](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/escala/services/cadastro_realidade.py:984), [cadastro_realidade.py:1197](/home/ronald/codex_ws/a365646fc4bb03683d0378b366a45592189eb4fe/app/escala/services/cadastro_realidade.py:1197)).

## 18/11 22:09 · sessao 9280fcbf61a8 · E4C-F6F7 · verificador · origem=Code

1. **nada — comportamento exercitado preservado no laço mostrado.** Em cada iteração não ignorada, `outras_ds` recebe uma lista nova, `p.decisoes.all()` devolve essa mesma lista e `LeituraReal.leitura(p)` a consome antes da próxima atribuição a `outras_ds` ([teste:230](/home/ronald/codex_ws/a555d027babd2d7318e7f86025e346eb6f91c354/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:230), [models.py:4308](/home/ronald/codex_ws/a555d027babd2d7318e7f86025e346eb6f91c354/app/chamados/models.py:4308)). A diferença é que a lambda nova guarda a referência à lista no argumento padrão; a anterior consultava a variável quando chamada. Assim, uma chamada tardia da lambda antiga poderia devolver outra lista, mas essa chamada tardia não aparece no trecho ([teste:232](/home/ronald/codex_ws/a555d027babd2d7318e7f86025e346eb6f91c354/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:232); versão anterior: anexo `delta.diff`, [não verificado] na cópia).

2. **FILA — resultado do Ruff não verificado.** Não identifiquei outra closure em laço que capture tardiamente variável desse laço: as lambdas em [teste:48](/home/ronald/codex_ws/a555d027babd2d7318e7f86025e346eb6f91c354/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:48) e [teste:221](/home/ronald/codex_ws/a555d027babd2d7318e7f86025e346eb6f91c354/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:221) fixam `ds` por argumento padrão; a função em [teste:177](/home/ronald/codex_ws/a555d027babd2d7318e7f86025e346eb6f91c354/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:177) faz o mesmo com `antigo` e `novo`. Não identifiquei outra acusação provável de B, F ou E9 na inspeção [não verificado por Ruff]. A configuração seleciona F, E9 e B, exceto B905 e B007 ([ruff.toml:4](/home/ronald/codex_ws/a555d027babd2d7318e7f86025e346eb6f91c354/app/ruff.toml:4)); o Ruff não estava disponível para execução [não verificado].

3. **nada — fluxo identificado.** `sequencia(*outros)` produz a lista atribuída a `outras_ds` ([teste:26](/home/ronald/codex_ws/a555d027babd2d7318e7f86025e346eb6f91c354/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:26), [teste:232](/home/ronald/codex_ws/a555d027babd2d7318e7f86025e346eb6f91c354/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:232)). A linha seguinte cria `N(all=...)` e o atribui a `p.decisoes`; a lambda devolve a lista ([teste:233](/home/ronald/codex_ws/a555d027babd2d7318e7f86025e346eb6f91c354/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:233)). `LeituraReal.leitura(p)` lê `p.decisoes.all()` ([teste:234](/home/ronald/codex_ws/a555d027babd2d7318e7f86025e346eb6f91c354/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:234), [models.py:4308](/home/ronald/codex_ws/a555d027babd2d7318e7f86025e346eb6f91c354/app/chamados/models.py:4308)). O próprio teste também passa `outras_ds` às duas funções comparadas ([teste:235](/home/ronald/codex_ws/a555d027babd2d7318e7f86025e346eb6f91c354/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:235)).

4. **nada — no delta fornecido**, somente a lambda mudou; não há mudança de asserção, caso ou tolerância no diff anexado [não verificado independentemente]. A cópia atual contém a asserção e os casos mostrados em [teste:225](/home/ronald/codex_ws/a555d027babd2d7318e7f86025e346eb6f91c354/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:225) e [teste:236](/home/ronald/codex_ws/a555d027babd2d7318e7f86025e346eb6f91c354/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:236). A cópia não está em um repositório Git acessível; HEAD e comparação com `f0ddfbe4c` permanecem [não verificado].

## 18/11 22:07 · sessao 360a48b03fa9 · E4C-F6F7 · construtor · origem=Code

A correção vincula `outras_ds` no argumento default da lambda, em [test_e4c_f6f7_c14_lei3_pela_leitura.py:233](/home/ronald/wt-codex-e4c-f6f7-220647/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:233):

```diff
-                    p.decisoes = N(all=lambda: outras_ds)
+                    p.decisoes = N(all=lambda outras_ds=outras_ds: outras_ds)
```

- Li o arquivo inteiro. Não identifiquei outra closure dentro de laço que capture variável do laço sem vinculá-la; as demais ocorrências relevantes já usam argumentos default em [linha 49](/home/ronald/wt-codex-e4c-f6f7-220647/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:49), [linha 174](/home/ronald/wt-codex-e4c-f6f7-220647/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:174) e [linha 221](/home/ronald/wt-codex-e4c-f6f7-220647/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:221).
- A closure da linha 233 é consumida na mesma iteração: a chamada seguinte a `LeituraReal.leitura(p)` está na [linha 234](/home/ronald/wt-codex-e4c-f6f7-220647/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:234), e `leitura` chama `p.decisoes.all()` em [models.py:4308](/home/ronald/wt-codex-e4c-f6f7-220647/app/chamados/models.py:4308).
- Não rodei suíte nem banco. O executável `ruff` não estava disponível no PATH; os comandos abaixo estão **[não verificados]**. O mutante é a própria linha original, que deve produzir B023: [nao verificado]

```bash
git show f0ddfbe4c3c46c5232fdc75fe7663d7f4fbc0f8c:app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py | ruff check --no-cache --isolated --select B023 --stdin-filename chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py -
```

Para verificar a correção:

```bash
ruff check --no-cache --isolated --select B023 chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py
```

## 18/11 22:05 · sessao 9432ef28a8ce · E4C-F6F7 · verificador · origem=Code

1. **OK — preservação das trocas, por leitura.** As duas lambdas continuam devolvendo a sequência da iteração, e a função mutada continua usando o par daquela iteração nas chamadas feitas pelo teste. A atribuição direta e o antigo `setattr` acionam o mesmo `VistaNoHorizonte.__setattr__`, que sempre levanta `AttributeError`; os slots também existem, mas não são o motivo dessa exceção. O `assertRaises` continua verificando o mesmo comportamento. [teste:48](/home/ronald/codex_ws/f0ddfbe4c3c46c5232fdc75fe7663d7f4fbc0f8c/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:48) [teste:146](/home/ronald/codex_ws/f0ddfbe4c3c46c5232fdc75fe7663d7f4fbc0f8c/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:146) [vista:403](/home/ronald/codex_ws/f0ddfbe4c3c46c5232fdc75fe7663d7f4fbc0f8c/app/chamados/services/tarefa_do_dia.py:403)

2. **OK — os quatro usos apontados como B023 são consumidos na mesma iteração.** Nas duas lambdas, `leitura(p)` chama `p.decisoes.all()` antes da próxima iteração; na função mutada, tanto `antigo` quanto `novo` são usados durante a chamada do teste dentro do `patch`. Não identifiquei vacuidade causada pela captura tardia nesses casos. A morte efetiva de cada mutante em execução é **[não verificado]**. [teste:44](/home/ronald/codex_ws/f0ddfbe4c3c46c5232fdc75fe7663d7f4fbc0f8c/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:44) [teste:174](/home/ronald/codex_ws/f0ddfbe4c3c46c5232fdc75fe7663d7f4fbc0f8c/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:174) [teste:216](/home/ronald/codex_ws/f0ddfbe4c3c46c5232fdc75fe7663d7f4fbc0f8c/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:216) [models.py:4308](/home/ronald/codex_ws/f0ddfbe4c3c46c5232fdc75fe7663d7f4fbc0f8c/app/chamados/models.py:4308)

3. **FILA — zero avisos do Ruff não está demonstrado.** A configuração seleciona `F`, `E9` e `B`, exceto `B905` e `B007`. Há outra lambda em laço que captura `outras_ds`; ela também é consumida na mesma iteração, mas se o Ruff da casa a sinaliza é **[não verificado]**. Não identifiquei outro aviso concreto por leitura; a execução do Ruff é **[não verificado]**. [ruff.toml:4](/home/ronald/codex_ws/f0ddfbe4c3c46c5232fdc75fe7663d7f4fbc0f8c/app/ruff.toml:4) [teste:226](/home/ronald/codex_ws/f0ddfbe4c3c46c5232fdc75fe7663d7f4fbc0f8c/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:226)

4. **FILA — alcance completo do delta [não verificado].** O anexo fornecido mostra alterações somente no arquivo de teste e não mostra mudança de asserção, caso ou tolerância; a comparação independente dos commits e a confirmação de que o anexo é o delta completo são **[não verificado]**. O arquivo atual conserva os casos e as asserções examinados. [teste:35](/home/ronald/codex_ws/f0ddfbe4c3c46c5232fdc75fe7663d7f4fbc0f8c/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:35) [teste:145](/home/ronald/codex_ws/f0ddfbe4c3c46c5232fdc75fe7663d7f4fbc0f8c/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:145) [teste:206](/home/ronald/codex_ws/f0ddfbe4c3c46c5232fdc75fe7663d7f4fbc0f8c/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:206)

## 18/11 21:38 · sessao 238ae9d67693 · E4C-F6F7 · construtor · origem=Code

A correção foi aplicada somente em [test_e4c_f6f7_c14_lei3_pela_leitura.py](/home/ronald/wt-codex-e4c-f6f7-213751/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:48). O diff é: [nao verificado]

```diff
- decisoes=N(all=lambda: ds))
+ decisoes=N(all=lambda ds=ds: ds))

- setattr(vista, 'respondida_em', instante)
+ vista.respondida_em = instante

- def funcao_mutada(arquivo, nome, *args, **kw):
+ def funcao_mutada(arquivo, nome, *args, antigo=antigo, novo=novo, **kw):

- decisoes=N(all=lambda: ds))
+ decisoes=N(all=lambda ds=ds: ds))
```

Sobre cada B023:

- **`ds`, linha 48:** a leitura é feita na linha 49, dentro da mesma iteração. O fechamento tardio não mudava o caso testado. [arquivo:linhas 44–49](/home/ronald/wt-codex-e4c-f6f7-213751/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:44)
- **`antigo` e `novo`, linha 179:** a função mutada é usada pelo teste chamado na linha 182, dentro da mesma iteração. O fechamento tardio não mudava o mutante testado. [arquivo:linhas 174–182](/home/ronald/wt-codex-e4c-f6f7-213751/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:174) [nao verificado]
- **`ds`, linha 221:** a leitura é feita na linha 222, dentro da mesma iteração. O fechamento tardio não mudava o caso testado. [arquivo:linhas 216–225](/home/ronald/wt-codex-e4c-f6f7-213751/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:216)

A verificação do Ruff ficou **não verificada**: o executável não está no PATH e `python3 -m ruff` também informou que o módulo não está instalado. Saída do comando solicitado, tentado antes da edição:

```text
$ PYTHONDONTWRITEBYTECODE=1 ruff check --config ruff.toml --no-cache chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py
/bin/bash: line 1: ruff: command not found
```

Não foi possível executar o RED nem os mutantes do lint pelo mesmo motivo. [não verificado] A suíte e o banco não foram executados, conforme solicitado. [nao verificado]

## 18/11 21:37 · sessao 954a693d4c56 · ESCALA-NAO-DESCREVE · verificador · origem=Code

1. **OK — a chamada nova chega à guarda da linha 313.** O teste chama diretamente `realocar_vinculo_para_destino` com o PK de `t12`, criado como `12x36`; a função extrai o ID, busca o template e avalia sua inclusão em `CICLICOS`, que contém `12x36`. [app/escala/tests/test_escala_nao_descreve.py:131](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/tests/test_escala_nao_descreve.py:131), [app/escala/tests/test_escala_nao_descreve.py:320](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/tests/test_escala_nao_descreve.py:320), [app/escala/models.py:386](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/models.py:386), [app/escala/regua_defesa.py:307](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/regua_defesa.py:307). Os únicos retornos anteriores são “Sem destino sugerido.” e “Template sugerido nao existe mais.”; nenhum contém `ciclico`. [app/escala/regua_defesa.py:308](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/regua_defesa.py:308). Após a guarda, a função monta o POST interno, chama o juiz, trata exceção e `no_op`, e então chama o executor; esse trecho não é alcançado no caso cíclico atual. [app/escala/regua_defesa.py:313](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/regua_defesa.py:313).

2. **OK — para esta chamada e estes dados, a mensagem que satisfaz a asserção é a da guarda:** `Destino ENDL-12 e ciclico: informe o Inicio do turno no wizard de escalas (nao se infere fase).` [app/escala/tests/test_escala_nao_descreve.py:131](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/tests/test_escala_nao_descreve.py:131), [app/escala/regua_defesa.py:314](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/regua_defesa.py:314). A regra de fase do juiz produziria outra mensagem, sem `ciclico`, e só seria chamada depois dessa guarda; a mensagem da view contém `ciclica`, mas esta chamada direta não passa pela view. [app/colaboradores/services/vinculo.py:126](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/colaboradores/services/vinculo.py:126), [app/escala/views.py:498](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/views.py:498). Em geral, a função interpola código ou descrição nas mensagens e devolve texto de exceções; portanto, a substring isolada não identificaria necessariamente a origem para dados arbitrários. [app/escala/regua_defesa.py:314](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/regua_defesa.py:314), [app/escala/regua_defesa.py:320](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/regua_defesa.py:320).

3. **OK, com limite — o controle prova uma gravação observável nesse cenário.** `x` começa em 5x2 desde setembro; o teste exige sucesso, uma linha ativa de `x` em 6x1 com início em `HOJE` e aumento de um no total do histórico. Assim, a existência exigida não é a linha inicial da fixture. [app/escala/tests/test_escala_nao_descreve.py:127](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/tests/test_escala_nao_descreve.py:127), [app/escala/tests/test_escala_nao_descreve.py:133](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/tests/test_escala_nao_descreve.py:133), [app/escala/tests/test_escala_nao_descreve.py:326](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/tests/test_escala_nao_descreve.py:326). A classe congela o relógio e passa `HOJE` explicitamente; o juiz e o executor ainda leem o relógio internamente, mas essas leituras ficam sob o congelamento neste teste. [app/escala/tests/test_escala_nao_descreve.py:30](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/tests/test_escala_nao_descreve.py:30), [app/escala/tests/test_escala_nao_descreve.py:116](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/tests/test_escala_nao_descreve.py:116), [app/colaboradores/services/vinculo.py:71](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/colaboradores/services/vinculo.py:71), [app/colaboradores/services/vinculo.py:194](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/colaboradores/services/vinculo.py:194). O caso troca tanto o colaborador quanto o destino e não verifica células: ele não isola qual diferença explica o resultado, nem prova regeneração de células. [app/escala/tests/test_escala_nao_descreve.py:320](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/tests/test_escala_nao_descreve.py:320).

4. **FILA — as contagens demonstram menos que uma comparação das linhas.** O próprio teste lê as contagens globais antes e depois da recusa e, no controle, filtra `EscalaColaborador` por colaborador, tipo, início e ativa; lê apenas o total global de `HistoricoVinculo`. [app/escala/tests/test_escala_nao_descreve.py:319](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/tests/test_escala_nao_descreve.py:319). No caminho de sucesso, o executor fecha vínculos anteriores por `fechar_vigencia`, que escreve `ativa` e `data_fim`, e cria a nova linha com os campos filtrados pelo teste. [app/colaboradores/services/vinculo.py:211](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/colaboradores/services/vinculo.py:211), [app/colaboradores/services/vinculo.py:641](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/colaboradores/services/vinculo.py:641), [app/colaboradores/services/vinculo.py:287](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/colaboradores/services/vinculo.py:287). O executor chama `registrar_mudanca`, que cria a linha de histórico; a tela de colaboradores lê esse histórico por colaborador. [app/colaboradores/services/vinculo.py:349](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/colaboradores/services/vinculo.py:349), [app/colaboradores/services/historico_vinculo.py:7](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/colaboradores/services/historico_vinculo.py:7), [app/colaboradores/views.py:317](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/colaboradores/views.py:317). Contagem igual na recusa não excluiria edição de linha existente; o `+1` global tampouco identifica, sozinho, o conteúdo ou colaborador da linha criada. [app/escala/tests/test_escala_nao_descreve.py:324](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/tests/test_escala_nao_descreve.py:324).

5. **FILA — há mordida na guarda, com limites de contrato.** O anexo informa que o mutante `mut_sem_recusa_ciclico` falhou nesse teste, mas não mostra a alteração do mutante nem a asserção que falhou; esses detalhes ficam **[não verificado]**. O teste fixa a recusa observável da porta para `12x36`, sem exigir que o julgamento more especificamente na linha 313; o juiz de fase em `vinculo.py` usa outra condição e outra mensagem, e o predicado da lista continua separado. [app/escala/tests/test_escala_nao_descreve.py:320](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/tests/test_escala_nao_descreve.py:320), [app/colaboradores/services/vinculo.py:128](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/colaboradores/services/vinculo.py:128), [app/escala/services/cadastro_realidade.py:849](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/services/cadastro_realidade.py:849). A outra consulta a `CICLICOS` na régua pertence ao diagnóstico, fora dessa chamada direta. [app/escala/regua_defesa.py:198](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/regua_defesa.py:198), [app/escala/regua_defesa.py:291](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/regua_defesa.py:291). O congelamento impede que este teste exercite uma virada de dia entre as leituras internas do relógio. [app/escala/tests/test_escala_nao_descreve.py:116](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/tests/test_escala_nao_descreve.py:116), [app/colaboradores/services/vinculo.py:189](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/colaboradores/services/vinculo.py:189). Quanto ao significado de `motivo`, o argumento textual vai para `confirm_txt` e depois `obs`, enquanto `HistoricoVinculo.motivo` recebe a categoria `mudanca` ou `correcao`; o caso novo só conta linhas e não verifica esses valores. [app/escala/regua_defesa.py:301](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/regua_defesa.py:301), [app/colaboradores/services/vinculo.py:354](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/colaboradores/services/vinculo.py:354).

## 18/11 21:30 · sessao 7732a47fe90e · ESCALA-NAO-DESCREVE · construtor · origem=Code

Acrescentei um teste que chama a porta diretamente com destino 12x36, verifica a mensagem e as contagens, e usa uma realocação para 6x1 como controle ([teste:311](/home/ronald/wt-codex-escala-nao-descreve-212957/app/escala/tests/test_escala_nao_descreve.py:311)). A data é fixa ([teste:33](/home/ronald/wt-codex-escala-nao-descreve-212957/app/escala/tests/test_escala_nao_descreve.py:33)). [nao verificado]

O teste e o mutante **não foram executados**, conforme solicitado. `git diff --check` passou [não verificado por certificação]. [nao verificado]

Comando do teste: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python manage.py test escala.tests.test_escala_nao_descreve.ListaPorCargaTest.test_MORDE_R5_porta_recusa_ciclico_e_realoca_nao_ciclico
```

O mutante remove, em memória, a recusa de [regua_defesa.py:313](/home/ronald/wt-codex-escala-nao-descreve-212957/app/escala/regua_defesa.py:313), sem editar produção:

```bash
PYTHONDONTWRITEBYTECODE=1 python - <<'PY'
import inspect
import os
import sys

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'config.settings.saas')
import django
django.setup()

import escala.regua_defesa as regua
fonte = inspect.getsource(regua.realocar_vinculo_para_destino)
linhas = fonte.splitlines(keepends=True)
indices = [i for i, linha in enumerate(linhas)
           if "if te.tipo_ciclo in getattr(te, 'CICLICOS'" in linha]
assert len(indices) == 1
i = indices[0]
assert "return False" in linhas[i + 1] and "ciclico" in linhas[i + 1]
del linhas[i:i + 2]
exec(compile(''.join(linhas), regua.__file__, 'exec'), regua.__dict__)

from django.core.management import execute_from_command_line
execute_from_command_line([
    sys.argv[0], 'test',
    'escala.tests.test_escala_nao_descreve.ListaPorCargaTest.'
    'test_MORDE_R5_porta_recusa_ciclico_e_realoca_nao_ciclico',
])
PY
```

A reprovação esperada é `AssertionError` na busca por `'ciclico'` ([teste:323](/home/ronald/wt-codex-escala-nao-descreve-212957/app/escala/tests/test_escala_nao_descreve.py:323)): sem a recusa da porta, o juiz inferior retorna outra mensagem para a ausência de início do turno ([vinculo.py:128](/home/ronald/wt-codex-escala-nao-descreve-212957/app/colaboradores/services/vinculo.py:128)). **Resultado e classe exata da reprovação: [não verificado].** [nao verificado]

LEI-AKITA: origem=n/a (selo; porta inalterada); testemunha=`realocar_vinculo_para_destino` ([regua_defesa.py:291](/home/ronald/wt-codex-escala-nao-descreve-212957/app/escala/regua_defesa.py:291)); RED=`test_MORDE_R5_porta_recusa_ciclico_e_realoca_nao_ciclico` ([teste:311](/home/ronald/wt-codex-escala-nao-descreve-212957/app/escala/tests/test_escala_nao_descreve.py:311), mordida [não verificada]); quem-mais-le=[views.py:471](/home/ronald/wt-codex-escala-nao-descreve-212957/app/escala/views.py:471), [views.py:502](/home/ronald/wt-codex-escala-nao-descreve-212957/app/escala/views.py:502), [regua_defesa.py:332](/home/ronald/wt-codex-escala-nao-descreve-212957/app/escala/regua_defesa.py:332), [test_termometro_camada3.py:100](/home/ronald/wt-codex-escala-nao-descreve-212957/app/escala/tests/test_termometro_camada3.py:100), [test_termometro_camada3.py:105](/home/ronald/wt-codex-escala-nao-descreve-212957/app/escala/tests/test_termometro_camada3.py:105); juizes novos=0 ([diff do teste](/home/ronald/wt-codex-escala-nao-descreve-212957/app/escala/tests/test_escala_nao_descreve.py:311)); padrao=n/a (instrumento). [nao verificado]

## 18/11 21:27 · sessao 429e0eff8383 · RAIA-ESPURIAS · leitura · origem=Code

A interface de `logs/espuria/envelope.sh` não está disponível na cópia consultada; a execução do script por esse envelope está [não verificado]. A sonda abaixo pressupõe um ambiente Django configurado e inicializa o Django caso necessário [não verificado]. A função de batidas filtra marcas não retratadas ([app/ponto/turnos.py](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:2868)); a grade e a classificação seguem as funções da cópia ([app/escala/utils.py](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/utils.py:2334), [app/escala/orfa_classe.py](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/orfa_classe.py:53)). Efeitos indiretos de todas as funções chamadas estão [não verificado].

`DIAS` conta pares colaborador-dia com órfãs encontrados na grade, como no anexo fornecido [não verificado]. A sonda não foi executada [não verificado].

```python
import collections
import datetime as dt
import re

ERROS = 0
TOTAL_ORFAS = 0
TOTAL_DIAS = 0
POR_CLASSE = collections.Counter()


def erro(col, etapa, exc):
    global ERROS
    ERROS += 1
    print(f"ERRO;{col};{etapa};{type(exc).__name__}", flush=True)


def executar():
    global TOTAL_ORFAS, TOTAL_DIAS

    try:
        import django
        from django.apps import apps

        if not apps.ready:
            django.setup()

        from django.db import connection

        if connection.settings_dict.get("NAME") != "sombra":
            print("NAO_FEITO;guarda_banco", flush=True)
            return

        from django.utils import timezone
        from colaboradores.models import Colaborador
        from ponto.turnos import batidas_apuraveis
        from escala.utils import grade_espelho_janela
        from escala.orfa_classe import classificar_orfas
    except Exception as exc:
        erro("col?", "inicializacao", exc)
        print("NAO_FEITO;inicializacao", flush=True)
        return

    try:
        agora = timezone.now()
        ini = dt.date(2026, 10, 21)
        fim = dt.date(2026, 11, 18)
        lo = (
            timezone.make_aware(dt.datetime.combine(ini, dt.time.min))
            - dt.timedelta(days=1)
        )
        hi = (
            timezone.make_aware(dt.datetime.combine(fim, dt.time.max))
            + dt.timedelta(days=1)
        )
        colaboradores = iter(
            Colaborador.objects.select_related("empresa", "posto")
            .filter(empresa_id__in=[1, 2, 3, 4])
            .order_by("pk")
            .iterator()
        )
    except Exception as exc:
        erro("col?", "preparacao", exc)
        print("NAO_FEITO;preparacao", flush=True)
        return

    while True:
        try:
            c = next(colaboradores)
        except StopIteration:
            break
        except Exception as exc:
            erro("col?", "iteracao_colaboradores", exc)
            print("NAO_FEITO;iteracao_colaboradores_restante", flush=True)
            break

        col = f"col{c.pk}"

        try:
            bs = list(batidas_apuraveis(c, lo, hi))
        except Exception as exc:
            erro(col, "batidas_apuraveis", exc)
            continue

        try:
            grade = grade_espelho_janela(c, bs, ini, fim, agora=agora)
            dias = grade.get("dias") or []
        except Exception as exc:
            erro(col, "grade_espelho_janela", exc)
            continue

        for d in dias:
            try:
                match = re.search(r"(\d{2})/(\d{2})", d.get("label", ""))
                if not match:
                    continue
                dia = dt.date(2026, int(match.group(2)), int(match.group(1)))
                horas = d.get("orfas") or []
                if not horas:
                    continue
                TOTAL_DIAS += 1
            except Exception as exc:
                erro(col, "dia_grade", exc)
                continue

            try:
                classes = classificar_orfas(d)
            except Exception as exc:
                erro(col, "classificar_orfas", exc)
                continue

            for hora in horas:
                try:
                    classe = classes.get(hora, "indefinida")
                    print(
                        f"VIVA;{col};{c.empresa_id};{dia.isoformat()};"
                        f"{hora};{classe}",
                        flush=True,
                    )
                    TOTAL_ORFAS += 1
                    POR_CLASSE[classe] += 1
                except Exception as exc:
                    erro(col, "emitir_orfa", exc)


try:
    executar()
except Exception as exc:
    erro("col?", "execucao", exc)
    print("NAO_FEITO;execucao_restante", flush=True)
finally:
    print(f"TOTAL_ORFAS={TOTAL_ORFAS}", flush=True)
    print(f"DIAS={TOTAL_DIAS}", flush=True)
    for classe, quantidade in sorted(POR_CLASSE.items()):
        print(f"CLASSE;{classe};{quantidade}", flush=True)
    print(f"ERROS={ERROS}", flush=True)
    print("FIM_FERNANDO_SV", flush=True)
```

## 18/11 21:25 · sessao e28abb7896fc · RAIA-ESPURIAS · leitura · origem=Code

O script abaixo preserva as regras e os agregados do anexo e acrescenta a emissão `NOSSA`; a comparação com `s1_v4_bruto.out`, a execução pelo envelope e `ERROS=0` permanecem **[não verificado]**, pois não há acesso ao ambiente de produção. O anexo `s1_v4.py` e o envelope não foram encontrados como arquivos nesta cópia **[não verificado]**.

As linhas acrescentadas estão marcadas com `# ACRESCENTADA`; seus números físicos são **[não verificado]**. A última linha de saída foi alterada para `FIM_FERNANDO_SN`. Quando uma falha impede obter a empresa ou consultar todos os minutos, o campo correspondente recebe `NAO_FEITO`.

```python
# ITEM 1 — AUDITORIA-ESPURIAS — medida, nao juiz.
# Universo: CelulaDia de empresas 2, 3 e 4, 2026-10-21 a 2026-11-20.
# Fonte das ocorrencias: CelulaDia.ata; porta da classificacao: grade_da_celula.
# Casamento: Batida viva do mesmo colab no minuto exato da data da celula.
# Exclusoes anteriores as classes: sem_hora; nao_mensuravel (a) e (b).
# F: tipo_dia == folga; le grade_da_celula.
# T6: trabalho e grade demonstra ausencia de marcos; le grade_da_celula.
# T1: trabalho e algum marco missing a <=90 min; le grade_da_celula e Batida.
# T2: trabalho, ha marco missing e todos estao a >90 min; le grade_da_celula e Batida.
# T5: outra Batida viva do mesmo tipo a <=10 min; le Batida.
# T4: entre as restantes, pares de orfas E seguidas da S seguinte, em ordem;
#     le grade_da_celula e Batida.
# T3: restante com instante e tipo decidiveis; le grade_da_celula e Batida.
# T5 auxiliar: outra Batida viva de qualquer tipo a <=5 min; le Batida.
# Diagnostico adicional: turnos_do_colab; nao decide as classes acima.
# Precedencia: F, T6, T1, T2, T5, T4, T3.
# Dias e pessoas sao conjuntos por classe e podem aparecer em varias classes.

from collections import Counter, defaultdict
from datetime import date, datetime, time, timedelta
import re

from django.db import DatabaseError
from django.utils import timezone

from colaboradores.models import Colaborador
from escala.models import CelulaDia
from escala.services.leitor_celula import grade_da_celula
from ponto.models import Batida
from ponto.turnos import turnos_do_colab

INI = date(2026, 10, 21)
FIM = date(2026, 11, 20)
EMPRESAS = (2, 3, 4)
CLASSES = ("F", "T6", "T1", "T2", "T5", "T4", "T3",
           "nao_mensuravel_a", "nao_mensuravel_b",
           "sem_hora", "NAO_CLASSIFICADA")

erros = 0
totais = Counter()
dias = defaultdict(set)
pessoas = defaultdict(set)
verdes = Counter()
motivos = Counter()
t5_aux = Counter()
total_ata = 0
turnos_encontrados = 0
universo_completo = True


def campos(modelo):
    print("CAMPOS", modelo._meta.label,
          " ".join(f.name for f in modelo._meta.get_fields()
                   if getattr(f, "concrete", False)))


def registrar(classe, cid, dia, verde, motivo=None): [nao verificado]
    totais[classe] += 1
    dias[classe].add((cid, dia))
    pessoas[classe].add(cid)
    if verde is True: [nao verificado]
        verdes[classe] += 1
    elif verde is None: [nao verificado]
        verdes[classe + "_verde_indeterminado"] += 1
    if motivo:
        motivos[(classe, motivo)] += 1


def falha(etapa, exc, cid=None):
    global erros
    erros += 1
    # Nao imprimir mensagem da excecao: pode conter nome ou outro dado pessoal.
    print("ERRO", "etapa=" + etapa, "tipo=" + type(exc).__name__,
          "col=" + ("col" + str(cid) if cid is not None else "universo"))


def perm_negada(exc):
    atual = exc
    while atual is not None:
        if getattr(atual, "pgcode", None) == "42501":
            return True
        if getattr(atual, "sqlstate", None) == "42501":
            return True
        atual = getattr(atual, "__cause__", None)
    return False


def hhmm(valor):
    if not isinstance(valor, str) or not re.fullmatch(r"\d{2}:\d{2}", valor):
        return None
    h, m = map(int, valor.split(":"))
    if h > 23 or m > 59:
        return None
    return time(h, m)


def local_minuto(ts):
    return timezone.localtime(ts).replace(second=0, microsecond=0)


def instante(dia, hora):
    return timezone.make_aware(datetime.combine(dia, hora),
                               timezone.get_current_timezone())


def emitir_nossa(cid, empresa_id, cel, valor, classe, motivo, por_minuto):  # ACRESCENTADA
    hora = hhmm(valor)  # ACRESCENTADA
    if hora is None:  # ACRESCENTADA
        pks = ""  # ACRESCENTADA
    elif por_minuto is None:  # ACRESCENTADA
        pks = "NAO_FEITO"  # ACRESCENTADA
    else:  # ACRESCENTADA
        batidas_minuto = por_minuto.get(instante(cel.data, hora), [])  # ACRESCENTADA
        pks = ",".join(str(b.pk) + ":" + str(b.tipo) for b in batidas_minuto)  # ACRESCENTADA
    print("NOSSA", "col" + str(cid), empresa_id, cel.data.isoformat(),  # ACRESCENTADA
          valor if hora is not None else "", classe,  # ACRESCENTADA
          (motivo or "") if classe == "NAO_CLASSIFICADA" else "", pks, sep=";")  # ACRESCENTADA


def ocorrencias_da_ata(ata):
    # O cartorio normalmente grava ambos; divergencia e conservada e avisada.
    if not isinstance(ata, dict):
        raise ValueError("ata nao e objeto")
    lista = ata.get("orfas") or []
    if not isinstance(lista, list):
        raise ValueError("ata.orfas nao e lista")
    lista = [str(x) if x else "?" for x in lista]
    n = ata.get("n_orfas")
    n = len(lista) if n is None else int(n)
    if n < 0:
        raise ValueError("n_orfas negativo")
    if lista and len(lista) != n:
        print("ATA_CONTAGEM_DIVERGENTE",
              "lista=" + str(len(lista)), "n_orfas=" + str(n))
    # A lista lavrada e a fonte quando existe, como em _orfas_da_ata.
    return lista if lista else ["?"] * n


def virada(cel, dia_grade):
    # True/False so quando ha os dois extremos; None significa nao sei.
    pares = {}
    for marco in dia_grade.get("marcos") or []:
        if isinstance(marco, dict) and marco.get("tipo") in ("E", "S"):
            h = hhmm(marco.get("hora"))
            if h is not None:
                pares.setdefault(marco["tipo"], []).append(h)
    if pares.get("E") and pares.get("S"):
        return pares["S"][-1] <= pares["E"][0]
    dna = cel.dna if isinstance(cel.dna, dict) else {}
    marcos = dna.get("marcos") or {}
    if isinstance(marcos, dict):
        hi, hf = hhmm(marcos.get("hi")), hhmm(marcos.get("hf"))
        if hi is not None and hf is not None:
            return hf <= hi
    return None


def verde_do_dia(d):
    if d.get("tipo_dia") != "trabalho":
        return False
    celulas = d.get("celulas")
    if not isinstance(celulas, list):
        return None
    if not celulas:
        return None
    if any(not isinstance(c, dict) or "missing" not in c for c in celulas):
        return None
    return not any(c["missing"] for c in celulas)


def distancias_missing(d, data_celula, ts_orfa):
    # A ordem dos marcos permite datar marcos apos a virada.
    celulas = d.get("celulas")
    if not isinstance(celulas, list):
        return None, "celulas da grade ausentes"
    if any(not isinstance(c, dict) or "missing" not in c for c in celulas):
        return None, "estado missing desconhecido"
    if not any(c["missing"] for c in celulas):
        return [], None
    resultado = []
    dia_marco = data_celula
    anterior = None
    for c in celulas:
        h = hhmm(c.get("hora_prevista"))
        if h is None:
            if c["missing"]:
                return None, "marco missing sem hora prevista"
            # Sem hora de um marco intermediario, a virada posterior e incerta.
            return None, "marco sem hora impede datar os missing"
        if anterior is not None and h < anterior:
            dia_marco += timedelta(days=1)
        anterior = h
        if c["missing"]:
            resultado.append(abs((instante(dia_marco, h) - ts_orfa)
                                 .total_seconds()) / 60)
    return resultado, None


def tem_outra(batida, batidas, minutos, mesmo_tipo):
    for outra in batidas:
        if outra.pk == batida.pk:
            continue
        if mesmo_tipo and outra.tipo != batida.tipo:
            continue
        if abs((outra.timestamp - batida.timestamp).total_seconds()) <= minutos * 60:
            return True
    return False


for modelo in (Colaborador, CelulaDia, Batida):
    campos(modelo)

print("UNIVERSO", INI.isoformat(), FIM.isoformat(), "empresas=2,3,4")
print("REFERENCIA_17_11=1267")
print("DIA_VERDE_REFERENCIA=83_dias/50_pessoas/105_orfas [nao verificado]")

try:
    ids = list(
        CelulaDia.objects.filter(
            data__range=(INI, FIM),
            colaborador__empresa_id__in=EMPRESAS,
        ).values_list("colaborador_id", flat=True).distinct().order_by("colaborador_id")
    )
except Exception as exc:
    falha("listar_colaboradores_da_celula", exc)
    ids = []
    universo_completo = False

for cid in ids:
    etiqueta = "col" + str(cid)
    etapa = "carregar_celulas"
    cels = None
    ocorrencias = []
    colab = None  # ACRESCENTADA
    minutos_completos = None  # ACRESCENTADA
    try:
        cels = list(CelulaDia.objects.filter(
            colaborador_id=cid, data__range=(INI, FIM)
        ).order_by("data", "pk"))
        for cel in cels:
            for valor in ocorrencias_da_ata(cel.ata or {}):
                ocorrencias.append((cel, valor))
        total_ata += len(ocorrencias)
        if not ocorrencias:
            continue

        etapa = "carregar_colaborador"
        colab = Colaborador.objects.get(pk=cid)

        etapa = "grade_da_celula(colab, INI, FIM)"
        grade = grade_da_celula(colab, INI, FIM)
        por_data = {d["data"]: d for d in grade["dias"]}

        etapa = "turnos_do_colab(colab, INI, FIM)"
        turnos = turnos_do_colab(colab, INI, FIM)
        turnos_encontrados += len(turnos)

        etapa = "consultar_Batida_viva"
        # Margem cobre T5 nas bordas e casamento no dia seguinte.
        batidas = list(Batida.objects.filter(
            colaborador_id=cid,
            retratada_em__isnull=True,
            timestamp__date__gte=INI - timedelta(days=1),
            timestamp__date__lte=FIM + timedelta(days=1),
        ).order_by("timestamp", "pk"))
        por_minuto = defaultdict(list)
        for b in batidas:
            por_minuto[local_minuto(b.timestamp)].append(b)
        minutos_completos = por_minuto  # ACRESCENTADA

        # Primeiro decide exclusoes e classes anteriores a T4.
        registros = []
        for cel, valor in ocorrencias:
            d = por_data.get(cel.data)
            r = {"cel": cel, "valor": valor, "classe": None,
                 "motivo": None, "batida": None, "verde": None} [nao verificado]
            registros.append(r)
            if d is None:
                r["classe"], r["motivo"] = "NAO_CLASSIFICADA", "dia ausente da grade"
                continue
            r["verde"] = verde_do_dia(d) [nao verificado]
            hora = hhmm(valor)
            if valor == "?":
                r["classe"] = "sem_hora"
                continue
            if hora is None:
                r["classe"], r["motivo"] = "NAO_CLASSIFICADA", "hora da ata invalida"
                continue

            alvo = instante(cel.data, hora)
            hoje = por_minuto.get(alvo, [])
            amanha = por_minuto.get(alvo + timedelta(days=1), [])
            atravessa = virada(cel, d)

            # (b) tem prioridade quando o proprio minuto da celula e multiplo.
            if len(hoje) > 1:
                r["classe"], r["motivo"] = "nao_mensuravel_b", "multiplas batidas no minuto da celula"
                continue
            if hoje and amanha:
                if atravessa is True:
                    r["classe"], r["motivo"] = "nao_mensuravel_a", "virada e batidas nos dois dias"
                    continue
                if atravessa is None:
                    r["classe"], r["motivo"] = "NAO_CLASSIFICADA", "virada desconhecida com batidas nos dois dias"
                    continue
            if not hoje:
                r["classe"], r["motivo"] = "NAO_CLASSIFICADA", "sem batida viva no minuto da data da celula"
                continue

            b = hoje[0]
            r["batida"] = b
            r["instante"] = local_minuto(b.timestamp)
            if b.tipo not in ("E", "S"):
                r["classe"], r["motivo"] = "NAO_CLASSIFICADA", "tipo da batida desconhecido"
                continue

            if tem_outra(b, batidas, 5, False):
                t5_aux["5min_qualquer_tipo"] += 1
            if tem_outra(b, batidas, 10, True):
                t5_aux["10min_mesmo_tipo"] += 1

            tipo = d.get("tipo_dia")
            if tipo == "folga":
                r["classe"] = "F"
                continue
            if tipo != "trabalho":
                r["classe"], r["motivo"] = "NAO_CLASSIFICADA", "tipo_dia=" + str(tipo)
                continue

            celdia = d.get("celulas")
            marcos = d.get("marcos")
            if not isinstance(celdia, list) or not isinstance(marcos, list):
                r["classe"], r["motivo"] = "NAO_CLASSIFICADA", "marcos ou celulas ausentes"
                continue
            if not marcos:
                if not celdia:
                    r["classe"] = "T6"
                else:
                    r["classe"], r["motivo"] = "NAO_CLASSIFICADA", "ata agregada nao identifica marcos"
                continue

            dist, motivo = distancias_missing(d, cel.data, alvo)
            if dist is None:
                r["classe"], r["motivo"] = "NAO_CLASSIFICADA", motivo
            elif dist:
                r["classe"] = "T1" if min(dist) <= 90 else "T2"
            elif tem_outra(b, batidas, 10, True):
                r["classe"] = "T5"

        # T4: pareia E com a S seguinte entre ocorrencias ainda elegiveis.
        # Se uma ocorrencia nao decidida pode interferir, nao forcar T4/T3.
        por_dia = defaultdict(list)
        for r in registros:
            por_dia[r["cel"].data].append(r)
        for data_celula, itens in por_dia.items():
            pendentes = [r for r in itens if r["classe"] is None]
            incerto = any(
                r["classe"] in ("NAO_CLASSIFICADA", "nao_mensuravel_a",
                                 "nao_mensuravel_b", "sem_hora")
                for r in itens
            )
            if incerto:
                for r in pendentes:
                    r["classe"] = "NAO_CLASSIFICADA"
                    r["motivo"] = "outra orfa incerta pode interferir no pareamento T4"
                continue
            pendentes.sort(key=lambda r: (r["instante"], r["batida"].pk))
            entrada = None
            for r in pendentes:
                if r["batida"].tipo == "E":
                    if entrada is not None:
                        entrada["classe"] = "T3"
                    entrada = r
                elif entrada is not None:
                    entrada["classe"] = r["classe"] = "T4"
                    entrada = None
                else:
                    r["classe"] = "T3"
            if entrada is not None:
                entrada["classe"] = "T3"

        for r in registros:
            registrar(r["classe"], cid, r["cel"].data,
                      r["verde"], r["motivo"]) [nao verificado]
            emitir_nossa(cid, colab.empresa_id, r["cel"], r["valor"],  # ACRESCENTADA
                         r["classe"], r["motivo"], minutos_completos)  # ACRESCENTADA

    except Exception as exc:
        falha(etapa, exc, cid)
        if perm_negada(exc):
            print("PERMISSAO_DE_ESCRITA_NEGADA",
                  "chamada=" + etapa, "col=" + etiqueta,
                  "acao=parar_este_colab")
        # Se a ata ja foi contada, preserva todas as suas ocorrencias.
        if ocorrencias:
            for cel, valor in ocorrencias:
                registrar("NAO_CLASSIFICADA", cid, cel.data, None,
                          "erro na etapa " + etapa)
                emitir_nossa(cid, colab.empresa_id if colab is not None else "NAO_FEITO",  # ACRESCENTADA
                             cel, valor, "NAO_CLASSIFICADA",  # ACRESCENTADA
                             "erro na etapa " + etapa, minutos_completos)  # ACRESCENTADA
        else:
            universo_completo = False

print("TURNOS_REAIS_RETORNADOS", turnos_encontrados)
print("T5_AUX_5MIN_QUALQUER_TIPO", t5_aux["5min_qualquer_tipo"])
print("T5_AUX_10MIN_MESMO_TIPO", t5_aux["10min_mesmo_tipo"])
print("T5_AUX_ESCOPO=orfas_casadas_com_tipo_valido_antes_da_precedencia")

for classe in CLASSES:
    print("CLASSE", classe,
          "dias=" + str(len(dias[classe])),
          "pessoas=" + str(len(pessoas[classe])),
          "orfas=" + str(totais[classe]),
          "dia_verde_orfas=" + str(verdes[classe]),
          "verde_indeterminado=" + str(verdes[classe + "_verde_indeterminado"]))

for (classe, motivo), n in sorted(motivos.items()):
    print("MOTIVO", classe, "orfas=" + str(n), motivo)

soma = sum(totais.values())
print("TOTAL_ORFAS_ATA", total_ata,
      "universo_completo=" + str(universo_completo))
print("SOMA_CLASSES_E_EXCLUSOES", soma)
print("SOMA_CONFERE", soma == total_ata and universo_completo)
if universo_completo:
    print("DIFERENCA_PARA_1267", total_ata - 1267)
else:
    print("DIFERENCA_PARA_1267 [nao verificado]: universo incompleto")
print("ERROS=" + str(erros))
print("FIM_FERNANDO_SN")
```

## 18/11 21:22 · sessao 2197ad35232a · RAIA-ESPURIAS · leitura · origem=Code

A sonda abaixo está **não executada**; seus resultados e `ERROS=N` permanecem [não verificado]. Ela conserva as ocorrências duplicadas dos dois literais e usa a janela local `dia..dia+1` para as candidatas físicas. O leitor fornece a lâmpada lavrada, mas não sua PK; a PK emitida em `OCUPADO_PK` resulta do critério de unicidade solicitado. ([leitor_celula.py:114](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:114), [turnos.py:2868](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:2868))

```python
import json
from collections import Counter
from datetime import date, datetime, time, timedelta

from django.db import connection
from django.utils import timezone

from colaboradores.models import Colaborador
from escala.models import CelulaDia
from escala.services.leitor_celula import grade_da_celula
from ponto.management.commands.e6_oraculo import InsumosDoDono
from ponto.services.cxr_na_lista import cxr_lavrado
from ponto.turnos import batidas_apuraveis, turnos_do_colab


# Projecao literal: colaborador_id;dia_turno;hora_orfa;marco_proximo;papel_marco.
# A duplicata de col451 em 31/10 e intencional.
ATE5 = """\
col63;2026-10-31;19:05;19:00;S
col67;2026-11-14;23:59;00:00;S
col67;2026-11-10;00:01;00:00;S
col81;2026-11-11;00:05;00:00;S
col118;2026-11-13;12:05;12:00;S
col121;2026-10-30;02:04;02:00;S
col211;2026-11-16;21:00;21:00;S
col211;2026-10-29;21:00;21:00;S
col211;2026-10-25;02:55;03:00;S
col220;2026-11-13;06:00;06:00;S
col220;2026-11-11;06:00;06:00;S
col278;2026-11-09;11:58;12:00;S
col281;2026-11-09;06:58;07:00;S
col282;2026-11-02;07:03;07:00;S
col300;2026-10-21;06:00;06:00;S
col366;2026-11-09;02:00;02:00;S
col366;2026-11-09;03:00;03:00;E
col366;2026-11-09;07:00;07:00;S
col366;2026-11-05;02:00;02:00;S
col366;2026-11-05;03:00;03:00;E
col366;2026-11-05;07:00;07:00;S
col366;2026-11-03;02:00;02:00;S
col366;2026-11-03;03:00;03:00;E
col366;2026-11-03;07:00;07:00;S
col366;2026-10-30;02:00;02:00;S
col366;2026-10-30;03:00;03:00;E
col366;2026-10-30;07:00;07:00;S
col366;2026-10-28;02:00;02:00;S
col366;2026-10-28;03:00;03:00;E
col366;2026-10-28;07:00;07:00;S
col407;2026-10-23;07:00;07:00;S
col444;2026-11-16;13:55;14:00;E
col451;2026-11-14;07:01;07:00;S
col451;2026-11-12;07:02;07:00;S
col451;2026-11-10;06:59;07:00;S
col451;2026-11-06;07:02;07:00;S
col451;2026-11-04;06:59;07:00;S
col451;2026-11-02;06:59;07:00;S
col451;2026-10-31;07:01;07:00;S
col451;2026-10-31;07:01;07:00;S
col451;2026-10-29;06:58;07:00;S
col451;2026-10-25;06:58;07:00;S
col451;2026-10-21;06:55;07:00;S
col489;2026-11-10;00:00;00:00;S
col491;2026-10-26;02:02;02:00;S
col491;2026-10-26;03:02;03:00;E
col552;2026-11-04;19:02;19:00;S
col587;2026-10-22;19:03;19:00;S
col627;2026-10-22;02:00;02:00;S
col627;2026-10-22;03:00;03:00;E
col627;2026-10-22;07:00;07:00;S
col673;2026-10-21;06:57;07:00;S
col843;2026-11-08;03:00;03:00;S
col843;2026-11-08;04:03;04:00;E
col843;2026-11-08;07:01;07:00;S
col859;2026-11-09;07:00;07:00;S
col876;2026-11-07;06:55;07:00;S
col881;2026-11-13;12:00;12:00;S
col909;2026-11-07;12:00;12:00;S
col913;2026-10-26;01:00;01:00;S
col913;2026-10-26;02:00;02:00;E
col913;2026-10-26;07:01;07:00;S
col922;2026-11-01;07:00;07:00;S
col947;2026-10-31;13:00;13:00;E
col1002;2026-11-09;18:59;19:00;S
col1010;2026-11-05;05:59;06:00;S
col1031;2026-11-17;06:55;07:00;S
col1031;2026-11-15;00:01;00:00;E
col1031;2026-11-15;06:55;07:00;S
"""

# Projecao literal: colaborador_id;dia_turno;hora_orfa.
NOMINAIS = """\
col673;2026-10-21;06:57
col82;2026-10-21;07:01
col82;2026-10-22;06:56
col82;2026-10-23;06:58
col82;2026-10-24;06:58
col82;2026-10-25;06:50
col89;2026-10-25;06:55
col107;2026-10-25;07:01
col107;2026-10-25;15:38
col129;2026-10-25;17:30
col155;2026-10-25;17:28
col214;2026-10-29;21:13
col214;2026-10-30;13:54
col82;2026-10-30;06:56
col214;2026-11-01;06:59
col107;2026-11-01;07:54
col107;2026-11-01;09:02
col129;2026-11-01;17:30
col155;2026-11-01;17:29
col129;2026-11-02;17:31
col214;2026-11-04;14:08
col89;2026-11-08;06:55
col129;2026-11-08;17:30
col214;2026-11-15;07:00
col82;2026-11-15;09:55
col82;2026-11-15;14:05
col82;2026-11-15;15:00
col82;2026-11-15;18:22
col89;2026-11-15;06:55
col107;2026-11-15;07:05
col107;2026-11-15;09:15
col129;2026-11-15;17:30
col155;2026-11-15;17:29
col107;2026-11-17;10:42
"""

INI = date(2026, 10, 21)
FIM = date(2026, 11, 20)
ERROS = Counter()
parados = set()
colabs = {}
grades = {}
fisicas = {}


def emitir(*campos):
    # Os campos de saida sao produzidos pela propria sonda; nunca imprime
    # mensagem de excecao, nome, CPF ou matricula.
    print(";".join(str(x) for x in campos))


def erro(col, etapa, exc):
    ERROS[(etapa, type(exc).__name__)] += 1
    emitir("ERRO", col, etapa, type(exc).__name__)
    atual = exc
    vistos = set()
    while atual is not None and id(atual) not in vistos:
        vistos.add(id(atual))
        codigo = getattr(atual, "sqlstate", None) or getattr(atual, "pgcode", None)
        if codigo in ("42501", "25006") or isinstance(atual, PermissionError):
            parados.add(col)
            break
        atual = getattr(atual, "__cause__", None) or getattr(atual, "__context__", None)


def obter_colab(col):
    if col not in colabs:
        if not col.startswith("col") or not col[3:].isdigit():
            raise ValueError("identificador invalido")
        colabs[col] = Colaborador.objects.get(pk=int(col[3:]))
    return colabs[col]


def obter_dia(col, dia):
    chave = (col, dia)
    if chave not in grades:
        colaborador = obter_colab(col)
        cels = list(CelulaDia.objects.filter(colaborador=colaborador, data=dia))
        if len(cels) > 1:
            raise ValueError("celulas duplicadas")
        grade = grade_da_celula(colaborador, dia, dia)
        dias = [d for d in grade["dias"] if d["data"] == dia]
        if len(dias) != 1:
            raise ValueError("dia ausente ou duplicado na grade")
        grades[chave] = (cels[0] if cels else None, dias[0])
    return grades[chave]


def instante(dia):
    return timezone.make_aware(
        datetime.combine(dia, time.min), timezone.get_current_timezone()
    )


def obter_batidas(col, dia):
    chave = (col, dia)
    if chave not in fisicas:
        lo = instante(dia)
        hi = instante(dia + timedelta(days=2))
        # A porta usa fim inclusivo; o filtro final exclui a meia-noite
        # posterior ao segundo dia.
        qs = batidas_apuraveis(obter_colab(col), lo, hi)
        fisicas[chave] = sorted(
            (b for b in qs if b.timestamp < hi),
            key=lambda b: (b.timestamp, b.pk),
        )
    return fisicas[chave]


def minuto(valor):
    if not isinstance(valor, str) or len(valor) != 5 or valor[2] != ":":
        return None
    try:
        h, m = int(valor[:2]), int(valor[3:])
    except ValueError:
        return None
    if not (0 <= h < 24 and 0 <= m < 60):
        return None
    return 60 * h + m


def distancia(a, b):
    # Menor distancia circular entre horarios; o sinal indica orfa - marco.
    return (a - b + 720) % 1440 - 720


def retrato(b):
    local = timezone.localtime(b.timestamp)
    return "{}:{}:{}".format(b.pk, b.tipo, local.strftime("%H:%M:%S"))


def no_minuto(batidas, hhmm, tipo=None):
    return [
        b for b in batidas
        if timezone.localtime(b.timestamp).strftime("%H:%M") == hhmm
        and (tipo is None or b.tipo == tipo)
    ]


def lampada_crua(cel, indice):
    # Somente campos da lampada pertinentes a pergunta. A ata pode conter
    # outros dados; nao serializar a ata inteira.
    if cel is None or not isinstance(cel.ata, dict):
        return "None"
    lampadas = cel.ata.get("lampadas")
    if not isinstance(lampadas, list) or indice >= len(lampadas):
        return "None"
    item = lampadas[indice]
    if not isinstance(item, dict):
        return "None"
    chaves = ("tipo", "hora", "luz", "tipo_real", "acesa", "dispensada", "isencao")
    seguro = {k: item.get(k) for k in chaves if k in item}
    return json.dumps(seguro, ensure_ascii=True, separators=(",", ":"))


def secao_c():
    linhas_ate5 = [x.split(";") for x in ATE5.strip().splitlines()]
    linhas_nominais = [x.split(";") for x in NOMINAIS.strip().splitlines()]
    if len(linhas_ate5) != 69 or len(linhas_nominais) != 34:
        raise ValueError("contagem dos literais incorreta")

    for linha in linhas_ate5 + linhas_nominais:
        col, data_txt, orfa = linha[:3]
        if col in parados:
            emitir("NAO_FEITO", col, data_txt, orfa, "permissao_negada")
            continue

        try:
            dia = date.fromisoformat(data_txt)
            bs = obter_batidas(col, dia)
            candidatas_orfa = no_minuto(bs, orfa)
            emitir(
                "ORFA_FISICA", col, data_txt, orfa,
                ",".join(retrato(b) for b in candidatas_orfa),
            )
        except Exception as exc:
            erro(col, "ORFA_FISICA", exc)
            emitir("ORFA_FISICA", col, data_txt, orfa, "NAO_FEITO")
            emitir(
                "MARCO", col, data_txt, orfa, "", "", "NAO_DETERMINAVEL",
                "", "", "", "", "None",
            )
            continue

        try:
            cel, d = obter_dia(col, dia)
            alvo = minuto(orfa)
            celulas = d.get("celulas") or []
            opcoes = []
            for indice, luz in enumerate(celulas):
                if not isinstance(luz, dict):
                    continue
                previsto = minuto(luz.get("hora_prevista"))
                if previsto is not None and alvo is not None:
                    opcoes.append((abs(distancia(alvo, previsto)), indice, luz))

            marco_hh = papel = pk = tipo = dist = candidatos = ""
            crua = "None"
            estado = "NAO_DETERMINAVEL"

            if opcoes:
                menor = min(x[0] for x in opcoes)
                proximas = [x for x in opcoes if x[0] == menor]
                # Empate entre marcos nao autoriza escolher um ocupante.
                if len(proximas) == 1:
                    _, indice, luz = proximas[0]
                    marco_hh = luz.get("hora_prevista") or ""
                    papel = luz.get("papel") or luz.get("tipo") or ""
                    dist = distancia(alvo, minuto(marco_hh))
                    crua = lampada_crua(cel, indice)

                    if (
                        cel is not None
                        and orfa in (d.get("orfas") or [])
                        and not luz.get("dispensada")
                        and not luz.get("isencao")
                    ):
                        acesa = luz.get("acesa_lavrada")
                        if acesa is False:
                            estado = "VAZIO"
                        elif acesa is True:
                            hora_real = luz.get("hora")
                            tipo_real = luz.get("tipo_real")
                            if minuto(hora_real) is not None and tipo_real in ("E", "S"):
                                matches = no_minuto(bs, hora_real, tipo_real)
                                candidatos = ",".join(retrato(b) for b in matches)
                                if len(matches) == 1:
                                    estado = "OCUPADO_PK"
                                    pk = matches[0].pk
                                    tipo = matches[0].tipo
                                else:
                                    estado = "OCUPADO_SEM_PK"

            emitir(
                "MARCO", col, data_txt, orfa, marco_hh, papel, estado,
                pk, tipo, dist, candidatos, crua,
            )
        except Exception as exc:
            erro(col, "MARCO", exc)
            emitir(
                "MARCO", col, data_txt, orfa, "", "", "NAO_DETERMINAVEL",
                "", "", "", "", "None",
            )


def secao_d():
    pedidos = {
        "col673": ("2026-10-21", "2026-10-22"),
        "col129": ("2026-10-25", "2026-11-01", "2026-11-02",
                   "2026-11-08", "2026-11-15"),
        "col155": ("2026-10-25", "2026-11-01", "2026-11-02",
                   "2026-11-15"),
        "col82": ("2026-11-15",),
        "col107": ("2026-11-15",),
        "col89": ("2026-11-15",),
        "col214": ("2026-11-15",),
    }
    try:
        ler_cxr = cxr_lavrado()
    except Exception as exc:
        erro("GLOBAL", "cxr_lavrado", exc)
        for col, datas in pedidos.items():
            for data_txt in datas:
                emitir("DONO", col, data_txt, "NAO_FEITO", "NAO_FEITO")
        return

    agora = timezone.now()
    hoje = timezone.localtime(agora).date()
    for col, datas in pedidos.items():
        if col in parados:
            for data_txt in datas:
                emitir("DONO", col, data_txt, "NAO_FEITO", "NAO_FEITO")
            continue
        try:
            colaborador = obter_colab(col)
            cxr = ler_cxr(colaborador.pk, INI)
            ins = InsumosDoDono(
                colaborador, INI, FIM, hoje, agora=agora,
                sem_motor=True, cxr=cxr,
            )
        except Exception as exc:
            erro(col, "InsumosDoDono", exc)
            for data_txt in datas:
                emitir("DONO", col, data_txt, "NAO_FEITO", "NAO_FEITO")
            continue

        for data_txt in datas:
            if col in parados:
                emitir("DONO", col, data_txt, "NAO_FEITO", "NAO_FEITO")
                continue
            try:
                dia = date.fromisoformat(data_txt)
                _, d = obter_dia(col, dia)
                dono = ins.dono(dia)
                emitir("DONO", col, data_txt, dono, d.get("tipo_dia"))
            except Exception as exc:
                erro(col, "dono", exc)
                emitir("DONO", col, data_txt, "NAO_FEITO", "NAO_FEITO")


def secao_e():
    col = "col214"
    pk_alvo = 155565
    if col in parados:
        emitir("TURNO214", "NAO_FEITO", "permissao_negada")
        emitir("PAR214", pk_alvo, "NAO_FEITO")
        return

    try:
        colaborador = obter_colab(col)
        dia = date(2026, 11, 15)
        alvos = [b for b in no_minuto(obter_batidas(col, dia), "07:00")
                 if b.pk == pk_alvo
                 and timezone.localtime(b.timestamp).date() == dia]
        turnos = turnos_do_colab(
            colaborador, date(2026, 11, 14), date(2026, 11, 16)
        )
        for t in turnos:
            emitir(
                "TURNO214", t.data_turno,
                t.entrada.pk if t.entrada is not None else "None",
                t.saida.pk if t.saida is not None else "None",
                ",".join(str(b.pk) for b in t.batidas),
                t.aberto, t.cross_meianoite,
            )

        ocorrencias = []
        for t in turnos:
            e = t.entrada.pk if t.entrada is not None else None
            s = t.saida.pk if t.saida is not None else None
            if pk_alvo == e or pk_alvo == s:
                ocorrencias.append((e, s))

        if len(alvos) != 1 or len(ocorrencias) != 1:
            emitir("PAR214", pk_alvo, "NAO_DETERMINAVEL")
        else:
            e, s = ocorrencias[0]
            outro = s if e == pk_alvo else e
            emitir(
                "PAR214", pk_alvo,
                "pareada_com={}".format(outro) if outro is not None else "SOZINHA",
            )
    except Exception as exc:
        erro(col, "turnos_do_colab", exc)
        emitir("TURNO214", "NAO_FEITO", "erro")
        emitir("PAR214", pk_alvo, "NAO_FEITO")


try:
    if connection.settings_dict.get("NAME") != "sombra":
        raise RuntimeError("banco nao e sombra")
    for nome, funcao in (("C", secao_c), ("D", secao_d), ("E", secao_e)):
        try:
            funcao()
        except Exception as exc:
            erro("GLOBAL", "secao_" + nome, exc)
            emitir(nome, "NAO_FEITO", type(exc).__name__)
except Exception as exc:
    erro("GLOBAL", "guarda_sombra", exc)
    for nome in ("C", "D", "E"):
        emitir(nome, "NAO_FEITO", "guarda_sombra")
finally:
    print("ERROS={}".format(sum(ERROS.values())))
    print("FIM_FERNANDO_S54")
```

**C — o que não prova:** a PK encontrada pela coincidência de minuto e tipo não demonstra qual batida o casador efetivamente usou; empate entre marcos também fica indeterminado. O literal não demonstra que a ata da sombra ainda contém a mesma órfã; isso depende da execução. ([leitor_celula.py:114](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:114), [não verificado])

**D — o que não prova:** `None` é uma resposta possível quando a lavra necessária não cobre o caso e o motor está desligado; não equivale a um dono identificado. Os valores específicos permanecem [não verificado]. ([e6_oraculo.py:182](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/management/commands/e6_oraculo.py:182))

**E — o que não prova:** o resultado descreve os turnos devolvidos pela porta na janela pedida, não estabelece o estado atual da produção. O par de `155565` permanece [não verificado]. ([turnos.py:1850](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1850))

O envelope indicado não foi encontrado na cópia; a garantia de não escrever em `logs/` do host permanece [não verificado]. O script direto citado pelo pedido escreve arquivos temporários e mantém trilha, de modo que essa garantia depende do envelope. ([sonda_leitura.sh:52](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/bin/sonda_leitura.sh:52), [sonda_leitura.sh:217](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/bin/sonda_leitura.sh:217), [não verificado])

## 18/11 21:19 · sessao 572fd5abab45 · RAIA-ESPURIAS · leitura · origem=Code

Não concluí a montagem e a revisão da sonda única. As duas faltas apontadas na volta 1 foram respondidas pelo material desta volta; não seria correto repeti-las como bloqueios. As contagens de referência e os quatro estados de MARCO estão no dado fornecido. [não verificado]

Não vou apresentar um script parcial como se cumprisse o contrato. Em particular, ainda falta integrar e conferir a transcrição completa de `s1_v4`, os dois literais de casos, o tratamento de todas as lâmpadas e a emissão de erros sem expor dados pessoais. A execução na sombra e o resultado `ERROS=N` permanecem [não verificado].

O que consegui confirmar na cópia para orientar a conclusão:

- **NOSSA:** a porta `grade_da_celula` lê as células e devolve os dias com órfãs da ata; isso não estabelece, sem rodar a sonda, a igualdade das contagens com a referência fornecida. ([leitor_celula.py:350](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:350), [leitor_celula.py:418](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:418))
- **VIVA:** `grade_espelho_janela` é a porta de recálculo e `classificar_orfas` é a porta de classificação; uma execução na sombra não prova o resultado da extração de produção. ([utils.py:2334](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/utils.py:2334), [orfa_classe.py:53](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/orfa_classe.py:53))
- **MARCO:** o leitor transporta `acesa_lavrada`, hora, tipo real, dispensa e isenção, mas não uma PK de ocupante. A regra de candidato único fornecida permite emitir a PK segundo o critério pedido; não prova por si só a decisão interna do casador. ([leitor_celula.py:114](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:114))
- **DONO:** `InsumosDoDono.dono` pode retornar `None` com `sem_motor=True`; esse valor deve ser mantido. O resultado para cada caso continua [não verificado]. ([e6_oraculo.py:182](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/management/commands/e6_oraculo.py:182))
- **col214:** `turnos_do_colab` chama `parear_turnos` e retorna turnos com entrada, saída e batidas; o par da batida indicada continua [não verificado] sem a execução. ([turnos.py:43](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:43), [turnos.py:1850](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1850), [turnos.py:1997](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1997))

Também não verifiquei o `envelope.sh` na cópia. O `bin/sonda_leitura.sh` diretamente cria ou reescreve arquivos em `logs/` e acrescenta uma trilha; portanto, a exigência de não escrever em `logs/` do host depende do envelope mencionado no contrato. ([sonda_leitura.sh:115](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/bin/sonda_leitura.sh:115), [sonda_leitura.sh:248](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/bin/sonda_leitura.sh:248), [não verificado])

## 18/11 21:17 · sessao 9d4eba666fe9 · RAIA-ESPURIAS · leitura · origem=Code

Não consigo entregar uma sonda que satisfaça integralmente o contrato de **MARCO** e a conferência classe a classe com as evidências disponíveis. Por isso, não apresento um script incompleto como se estivesse pronto para a rodada na sombra.

O bloqueio principal é a identidade do ocupante. `grade_da_celula` devolve, para cada lâmpada da ata, a hora realizada, o tipo real e o estado, mas não devolve a PK da batida; tampouco uma hora, por si só, identifica uma batida quando há mais de uma candidata ([leitor_celula.py:114](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:114), [cartorio.py:699](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/services/cartorio.py:699)). Além disso, `acesa_lavrada` pode ser `None`, e há lâmpadas dispensadas ou de isenção; portanto, reduzir todos os casos a **OCUPADO por uma PK** ou **VAZIO** pode inventar uma conclusão ([leitor_celula.py:130](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:130)).

A referência fornecida informa **1.292 no total**, mas não traz as contagens de referência de cada classe do `s1_v4`; logo, a sonda pode calcular as contagens na sombra e a diferença do total, mas não determinar a diferença *por classe* em relação à rodada anterior. [não verificado]

O que as portas verificadas permitem medir e seus limites:

- **NOSSA:** a ata fornece as órfãs e a grade as reapresenta; a lógica integral de classificação está no `s1_v4` fornecido. O resultado seria o da sombra na nova execução, sem provar que coincide classe a classe com a rodada de referência ([leitor_celula.py:180](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:180)). A referência de 1.292 é do texto fornecido. [não verificado]
- **VIVA:** `grade_espelho_janela` e `classificar_orfas` permitem recalcular e classificar a grade, alimentada pelas batidas apuráveis; isso não reproduz por si só o estado ou o instante da extração de produção ([utils.py:2334](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/utils.py:2334), [orfa_classe.py:53](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/orfa_classe.py:53), [turnos.py:2868](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:2868)).
- **MARCO:** a ata permite relatar marcos, papéis, horas e estados lavrados disponíveis, além de candidatos físicos separados; não prova necessariamente qual PK ocupou um marco nem por que o casador tomou uma decisão ([leitor_celula.py:114](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:114)).
- **DONO:** `InsumosDoDono.dono(dia)` é a porta pedida; com `sem_motor=True`, pode devolver `None` quando falta a lavra necessária, e isso deve permanecer sem resposta ([e6_oraculo.py:128](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/management/commands/e6_oraculo.py:128), [e6_oraculo.py:182](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/management/commands/e6_oraculo.py:182)). Para col214, a porta de produção é `turnos_do_colab`, que alimenta `parear_turnos`; o resultado específico permanece [não verificado] sem execução ([turnos.py:1850](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1850), [turnos.py:1997](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1997)).

Para fechar a entrega sem aproximar, falta permitir um estado explícito **NÃO DETERMINÁVEL** em MARCO, com candidatos identificados como candidatos, e fornecer as contagens por classe da rodada `s1_v4` de referência. A execução e seus `ERROS=N` continuam [não verificado].

_7175 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
