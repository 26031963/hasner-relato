# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 20:20** · publicado em **06/11/2026 20:21** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 20:20 · sessao 3487bdb015de · - · - · origem=Ronald (tmux)

Considere abaixo “aceita” como `julgar` devolvendo `None` para `padrao=n/a (medida)`, com o arquivo no conjunto de negócio e sem outro bloqueador no commit. A exceção remove arquivos cujo callback `medida(a)` retorna verdadeiro. (`bin/tests/test_lei_akita.sh:186-191`)

| Caso | Resultado e fundamento |
|---|---|
| Dict com chamada, `R={'n': fechar()}` | **Aceita**: qualquer `Assign`/`AnnAssign` de topo sem `Lambda` é livre; não examina chamadas nem efeitos. (`bin/tests/test_lei_akita.sh:152-155`) |
| Reatribuir global usado por função | **Aceita**, inclusive `FATOR=2`, sem alterar a função: não analisa dependências dos leitores. (`bin/tests/test_lei_akita.sh:152-155`) |
| Monkeypatch de topo, `motor.fechar=outro` | **Aceita**: alvos de atribuição não são restringidos a nomes de dados. (`bin/tests/test_lei_akita.sh:152-155`) |
| Anotação executável, `x: fechar()=1` | **Aceita**: `AnnAssign` inteiro é livre, exceto se houver `Lambda`. (`bin/tests/test_lei_akita.sh:152-155`) |
| Chamada solta `fechar()` | **Recusa**: `Expr(Call)` entra em `preso`. (`bin/tests/test_lei_akita.sh:151-155,174-175`) |
| Função/classe/import mexido | **Recusa** se houver linha desse nó efetivamente alterada: seu intervalo fica preso. (`bin/tests/test_lei_akita.sh:148-155,174-175`) |
| Declaração seguida de `PAPEL_DO_MODULO='negocio'` | **Aceita**: basta existir *algum* `Assign` literal de topo igual a `'medida'`; não verifica a última atribuição nem o valor efetivo. (`bin/tests/test_lei_akita.sh:139-144`) |
| Decorador | **Recusa**: intervalo preso da função começa no menor `lineno` entre função e decoradores. (`bin/tests/test_lei_akita.sh:149-155`) |
| Continuação de atribuição com chamada | **Aceita**: todo o intervalo até `end_lineno` fica livre. (`bin/tests/test_lei_akita.sh:150-155`) |
| Continuação de import/def/class | **Recusa**: todo o intervalo fica preso. (`bin/tests/test_lei_akita.sh:148-155`) |
| Docstring inicial | **Aceita**; comentários anteriores não contam como elementos de `t.body`. (`bin/tests/test_lei_akita.sh:148-153`) |
| String que não é o primeiro nó do módulo | **Recusa**: `eh_doc` exige `i == 0`. (`bin/tests/test_lei_akita.sh:151-155`) |
| Comentário/branco dentro de def/class | **Recusa**: `soltas` perde para `preso`. (`bin/tests/test_lei_akita.sh:154-155`) |
| Hunk `-U0` com contagem zero | **Ignora corretamente esse lado**: `if n` impede cobrar a linha âncora; o outro lado continua sendo conferido. Não é, sozinho, um bypass. (`bin/tests/test_lei_akita.sh:171-175`) |
| Arquivo novo que se declara medida | **Aceita se todo o arquivo novo for livre**, inclusive atribuições com chamadas/monkeypatch; lado pai ausente é ignorado por `-0,0`. Novo arquivo com def/import/classe é recusado. Commit raiz sem primeiro pai é recusado. (`bin/tests/test_lei_akita.sh:158-175`) |
| Arquivo deletado e ainda ausente no ALVO | **Recusa** antes do diff: não existe declaração no ALVO. (`bin/tests/test_lei_akita.sh:131-144,158-159`) |
| Deleção histórica, arquivo restaurado no ALVO | **Pode aceitar**: declaração restaurada satisfaz a guarda; exclusão de arquivo composto somente por linhas livres passa, pois `+0,0` ignora AST ausente em `h`. Excluir função/import/classe continua recusado pelo lado pai. (`bin/tests/test_lei_akita.sh:139-175`) |
| Rename | `--no-renames` conserva origem e destino na lista; origem ausente no ALVO normalmente faz **recusar**. Se origem for restaurada no ALVO e ambos os lados forem compostos de linhas livres, pode **aceitar**. (`bin/tests/test_lei_akita.sh:139-175,238-242,255`) |
| Merge | Usa diff contra primeiro pai. Depois remove caminhos iguais a qualquer outro pai já publicado; diferenças inéditas restantes passam pelo mesmo callback vulnerável a atribuições executáveis. Não há aceitação geral de merges, mas tampouco proteção adicional dos RHS. (`bin/tests/test_lei_akita.sh:236-255`) |
| Diff bem-sucedido sem nenhum `@@` | **Aceita** depois da declaração e da existência do pai, pois o laço não roda e chega a `return True`. Não exige ao menos um hunk nem rejeita saída “Binary files differ”; alteração apenas de modo também não tem linhas para cobrar. (`bin/tests/test_lei_akita.sh:158-176`) |

A leitura no ALVO abre **classificação retroativa**: adicionar hoje a declaração a um arquivo de negócio faz commits anteriores de atribuições desse arquivo passarem como medida; tirar hoje a declaração faz commits anteriores de medida serem recusados. A remoção em `h` também pode passar se o ALVO posterior restaurou a declaração. Não migra a análise das linhas: os dois ASTs ainda são pai e `h`, mas a autorização vem do ALVO. (`bin/tests/test_lei_akita.sh:139-144,156-175,255`)

Confirmei em memória as classificações AST e, com `subprocess.run` simulado, os casos novo só atribuições=True; novo com def=False; deleção ausente no ALVO=False; deleção restaurada=True; diff sem hunk=True. Isso valida a implementação da função, **não** a produção real dessas saídas pelo Git nesta cópia sem `.git`. (`bin/tests/test_lei_akita.sh:139-176`; execução Git real desses cenários: [não verificado])

## 06/11 20:20 · sessao 42b265fa899f · - · - · origem=Ronald (tmux)

Achados para consolidação:

- **“Medida” está semanticamente alargada:** os módulos dizem “NUMERO MEDIDO e texto de PROVA” (`app/core/placar_estrutural.py:33`, `app/core/espelho_verdade.py:25`), mas o selo libera qualquer `Assign`/`AnnAssign` sem lambda (`bin/tests/test_lei_akita.sh:152`). Isso inclui `CADENCIA_TERMOMETRO_DIAS = 7` (`app/core/placar_estrutural.py:61`), usada para decidir vencimento (`app/core/placar_estrutural.py:667`) e cor/veredito (`app/core/placar_estrutural.py:685`). Trocar a cadência recebe a exceção apesar de alterar decisão do leitor, sem tocar `def`. (`bin/tests/test_lei_akita.sh:188`)

- **Outros dados liberados também decidem:** `RESULTADOS` alimenta estado efetivo e papel (`app/core/placar_estrutural.py:643`), e a linha pública conta estados usando constantes globais (`app/core/placar_estrutural.py:699`). No espelho, `ETAPAS` e `FEITO` determinam feitos e percentual (`app/core/espelho_verdade.py:205`, `app/core/espelho_verdade.py:210`, `app/core/espelho_verdade.py:213`). O selo não distingue esses controles de número/prova. (`bin/tests/test_lei_akita.sh:152`)

- **Escritores de `PAPEL_DO_MODULO`:** declarações reais em `app/core/placar_estrutural.py:38` e `app/core/espelho_verdade.py:30`; fixture do MORDE escreve o literal em `bin/tests/test_lei_akita.sh:314` e produz sua retirada em `bin/tests/test_lei_akita.sh:338`. **Leitor semântico:** `declara_medida`, por AST no `ALVO`, em `bin/tests/test_lei_akita.sh:139`. Não lê o valor executado: basta existir algum `Assign` correspondente no corpo. (`bin/tests/test_lei_akita.sh:141`)

- **Escritor de `medida_do_commit`:** definição em `bin/tests/test_lei_akita.sh:156`; **leitores/chamadores:** universo real em `bin/tests/test_lei_akita.sh:255`; casos MORDE em `bin/tests/test_lei_akita.sh:347`; retirada da declaração no ALVO em `bin/tests/test_lei_akita.sh:351`. A exceção é aplicada por `_julgar_linha` ao callback `medida`, em `bin/tests/test_lei_akita.sh:188`.

- **O nome novo, isoladamente, não interfere nos leitores identificados:** `gerar_estado` executa os módulos e escolhe atributos explícitos, sem enumerar nomes globais (`bin/gerar_estado.py:75`, `bin/gerar_estado.py:80`, `bin/gerar_estado.py:142`, `bin/gerar_estado.py:145`). Core testes escolhem `RESULTADOS`, `ETAPAS`, funções e constantes explícitas (`app/core/tests/test_r1_termometro_uma_medicao.py:40`, `app/core/tests/test_r1_termometro_uma_medicao.py:44`, `app/core/tests/test_selo_r6_frase_do_juiz.py:16`, `app/core/tests/test_l114_placar_papel_e_termometro.py:30`). O selo de afirmações lê só `mod.RESULTADOS` (`bin/tests/test_placar_nao_afirma_fora_do_ar.sh:80`); handoff usa regex específica da frase dos contratos (`bin/handoff_sessao.sh:178`). Contrato que enumere exatamente o conjunto de nomes globais desses dois módulos não foi encontrado; ausência absoluta e execução integral dos testes: **[não verificado]**.

- **`regua` e pre-push são consumidores indiretos do resultado do selo:** a régua percorre `bin/tests/test_*.sh` (`bin/regua.sh:138`) e o pre-push chama especificamente o selo com o SHA empurrado (`bin/pre-push.sh:38`); nenhum desses trechos interpreta `PAPEL_DO_MODULO`. (`bin/regua.sh:140`, `bin/pre-push.sh:38`)

- **Juiz duplicado/derivação paralela:** a mudança reaproveita `_julgar_linha` e o universo existente, adicionando callback e predicado de elegibilidade (`bin/tests/test_lei_akita.sh:177`, `bin/tests/test_lei_akita.sh:255`). Não demonstra uma segunda autoridade de cálculo/dinheiro/batida. O registro vivo define autoridades por pergunta (`app/core/juizes.py:3`, `app/core/juizes.py:7`), enquanto o anexo de consulta declara sua própria natureza de lookup (`app/docs/JUIZES.md:7`). A reprovação demonstrável é a divergência entre a promessa de “medida” e a sintaxe aceita, acima. (`bin/tests/test_lei_akita.sh:124`, `bin/tests/test_lei_akita.sh:152`)

## 06/11 20:19 · sessao edb5b413dba1 · - · - · origem=Ronald (tmux)

O MORDE reutiliza o juiz real `julgar` e `medida_do_commit`; o portão rejeita qualquer `MORDE_FALHOU`, embora `MORDE_OK` seja impresso ao final independentemente das falhas. Portanto a impressão de `MORDE_OK` isoladamente não prova sucesso. (`bin/tests/test_lei_akita.sh:347`, `bin/tests/test_lei_akita.sh:473`, `bin/tests/test_lei_akita.sh:488`)

| Caso | Mutante que o caso distingue, por inspeção |
|---|---|
| `so_numero`, `comentario`, `doc` verdes | Aceite desligado, ou recusar toda atribuição/comentário/docstring. (`bin/tests/test_lei_akita.sh:330`, `bin/tests/test_lei_akita.sh:341`) |
| `na_def` vermelho | Liberar corpo de função; também mata “todas as linhas livres”. (`bin/tests/test_lei_akita.sh:333`, `bin/tests/test_lei_akita.sh:342`) |
| `com_import` vermelho | Liberar import de topo. (`bin/tests/test_lei_akita.sh:334`, `bin/tests/test_lei_akita.sh:342`) |
| `com_lambda` vermelho | Remover veto a lambda dentro de atribuição. (`bin/tests/test_lei_akita.sh:335`, `bin/tests/test_lei_akita.sh:342`) |
| `sem_decl` vermelho | Dispensar declaração. (`bin/tests/test_lei_akita.sh:336`, `bin/tests/test_lei_akita.sh:343`) |
| `mais_fech` vermelho | Ignorar outro arquivo de negócio ou examinar só o primeiro arquivo. (`bin/tests/test_lei_akita.sh:337`, `bin/tests/test_lei_akita.sh:343`) |
| Número com `n/a (instrumento)` vermelho | Estender a exceção ao outro rótulo. (`bin/tests/test_lei_akita.sh:344`) |
| ALVO sem declaração, julgando `so_numero`, vermelho | Ler declaração em `h` em vez de ALVO. (`bin/tests/test_lei_akita.sh:349`) |

São distinções deduzidas do código; a execução dos dois mutantes alegados, seus resultados vermelhos e os três commits verdes permanecem **[não verificado]**. O caminho `logs/akmed/red_vivo.log` citado no anexo não está disponível nesta cópia **[não verificado]**.

O bloco não cobre individualmente classe, decorador, docstring não inicial, chamada em atribuição, `AnnAssign`, reatribuição de nome funcional, monkeypatch, declaração anulada depois, novo arquivo, exclusão, rename, merge de medida, erro git nem concessão retroativa de declaração pelo ALVO. Seu catálogo completo contém somente os nove casos e a checagem adicional descritos acima. (`bin/tests/test_lei_akita.sh:330`, `bin/tests/test_lei_akita.sh:340`, `bin/tests/test_lei_akita.sh:349`)

Há uma atribuição multilinha positiva (`PL`/`so_numero`) e inserção de comentário; falta caso negativo que dependa exclusivamente do lado antigo do diff. `na_def` tem linhas presas nos dois lados, enquanto import e lambda são negativos pelo lado novo. (`bin/tests/test_lei_akita.sh:314`, `bin/tests/test_lei_akita.sh:330`, `bin/tests/test_lei_akita.sh:331`, `bin/tests/test_lei_akita.sh:333`)

Os casos de medida não testam a integração com a enumeração do universo: os commits recebem mensagem `"c"` e o teste chama diretamente `julgar` com mensagem e lista de arquivos construídas manualmente. (`bin/tests/test_lei_akita.sh:325`, `bin/tests/test_lei_akita.sh:340`, `bin/tests/test_lei_akita.sh:347`)

A cláusula “erro de git = PADRAO_ERRO” não é cumprida literalmente por todas as novas operações: falha de `git show` vira AST ausente; qualquer falha de `rev-parse h^1` vira `False`; apenas o novo `git diff` levanta explicitamente `PADRAO_ERRO`. Esses dois primeiros caminhos normalmente recusam a exceção, mas não emitem a classificação de erro prometida. (`bin/tests/test_lei_akita.sh:131`, `bin/tests/test_lei_akita.sh:160`, `bin/tests/test_lei_akita.sh:164`)

## 06/11 20:16 · sessao 3375283c7246 · - · - · origem=Ronald (tmux)

Chamadores diretos de produção de `minutos_noturnos`:

- `MotorBase.calcular_noturnas`, definição `app/ponto/motor_calculo_v2.py:969`, chamada `:983`: recebe `[e,s]` após `_janela_do_par`, aplicado em `:2759`, `:2886`, `:3211`. Os escritores chamam essa função em `:2792`, `:2896`, `:3247`.
- `he_noturna`, definição `app/ponto/motor_calculo_v2.py:409`, chamada `:441`: recebe `[_inicio_do_trecho_extra(...), saida]`, calculado em `:440`. `_aplicar_he_noturna` passa `periodo.entrada/saida` em `:1015`.
- `minutos_noturnos_dos_intervalos`, definição `app/ponto/motor_calculo_v2.py:121`: mede prefixos `[entrada,b]` e `[entrada,a]` em `:191–192`, além do total `[entrada,saida]` em `:196`.
- `noturnas_do_dia`, definição `app/ponto/calculador/regras.py:66`, chamada `:85`: recebe cada par fornecido pelo calculador. O caminho de produção pode fornecer segmentos cortados pelos intervalos: `app/ponto/calculador/alimentacao.py:128`, `:133`, `:137`, `:158`; a função `noturno_no_par` fornece o par inteiro: `app/ponto/calculador/periodo.py:488`, `:510`.

`MotorPlantao` usa a conta de `Motor12x36.calcular_periodo`, por delegação explícita em `app/ponto/motor_calculo_v2.py:3510–3511`; portanto não constitui outro chamador direto.

O universo real exige mais que `batidas_apuraveis + parear_turnos`: `calcular_mes` remove duplicatas em `app/ponto/motor_calculo_v2.py:2351`, monta períodos em `:2354`, e `_periodos_via_autoridade` aplica `periodos_de_turnos(turnos_via_autoridade(...))` em `:530`, seguido de `_periodos_pelo_marco` em `:531`. O filtro por dia da jornada antecede a chamada de `calcular_periodo`: `:2403`, `:2429–2432`, `:2458`.

Recomendação de instrumentação: `sys.setprofile` observando os objetos `__code__` reais permite coletar os argumentos e retornos de `minutos_noturnos`, sem reconstruir os recortes e sem alterar módulos. Contar separadamente os canais principal, HE e prefixos de intervalos; prefixos não são novos turnos. A HE pode ser chamada novamente sobre o último período após agregação diária/semanal: `app/ponto/motor_calculo_v2.py:3147`, `:3178`, `:3399`.

Para o excesso de (b), reproduzir apenas a geometria da sobreposição, após verificar o limite de 26h de `app/ponto/motor_calculo_v2.py:94`. Para cada dia civil com a condição de prorrogação de `:110`, somar:

```python
max(0.0, (min(fim, meia_noite_seguinte)
          - max(inicio, inicio_do_dia, hora_22)).total_seconds() / 60)
```

As duas parcelas que produzem a duplicidade são explicitamente somadas em `app/ponto/motor_calculo_v2.py:112–116`.

O fechamento com `somente_leitura=True` e `lavrar_dias_pagos=False` não percorre o calculador. O chamador do calculador fica no ramo de lavratura em `app/ponto/services/fechamento.py:1253`, `:1255`, condicionado a competência ≥10/2026 em `:1249`. O calculador de HE chama a mesma `he_noturna` em `app/ponto/calculador/regras.py:832`, usando `he_pontas` formadas com as pontas efetivas em `app/ponto/calculador/alimentacao.py:83`, `:101–106`. Portanto medir somente o fechamento em leitura não autoriza imprimir zero para os chamadores do calculador.

## 06/11 20:16 · sessao 5f84fc60e33a · - · - · origem=Ronald (tmux)

Desenho recomendado para o SCRIPT 2:

1. Chamar `recalcular_fechamento_mes(mes, 2026, empresa_id=empresa, colaborador_ids=[pk], somente_leitura=True, lavrar_dias_pagos=False, retornar_objeto=True)`. O universo é `universo_da_competencia`; a flag `lavrar_dias_pagos=True` altera o comportamento da guarda de competências exportadas, portanto prejudica a equivalência do universo. `app/ponto/services/fechamento.py:373`, `app/ponto/services/fechamento.py:530`, `app/ponto/services/fechamento.py:545`.

2. Capturar `dict(frame.f_locals)` apenas no evento `return` de `recalcular_fechamento_mes.__code__`, mediante `sys.setprofile`. Isso entrega o `resultado` composto e os mapas usados pelo escritor de dias: `_ft_certa`, `_ft_feriado`, `_ft_sem_escala`, `_orfaos_por_dia`, `_prev_dia_m`, `_real_dia_m`. As entradas efetivas da lavratura estão em `app/ponto/services/fechamento.py:1185`.

3. Para obter valores diários sem copiar aritmética, chamar a função **real** `dia_pago.lavrar` sobre uma fachada temporária de `ponto.models.DiaPago`. A fachada instancia objetos reais não salvos; somente `.objects.filter(...).delete()` e `.objects.bulk_create(...)` são substituídos por operações em memória. A função real monta todos os números antes dessas duas operações. `app/ponto/services/dia_pago.py:135`, `app/ponto/services/dia_pago.py:173`, `app/ponto/services/dia_pago.py:267`, `app/ponto/services/dia_pago.py:283`, `app/ponto/services/dia_pago.py:333`, `app/ponto/services/dia_pago.py:357`.

```python
from unittest.mock import patch
import ponto.models as modelos
from ponto.services.dia_pago import lavrar

RealDiaPago = modelos.DiaPago
capturadas = []

class Memoria:
    def filter(self, **kwargs):
        return self

    def delete(self):
        return 0, {}

    def bulk_create(self, objetos):
        capturadas.extend(objetos)
        return objetos

class DiaPagoEmMemoria:
    TIPO_DIA = RealDiaPago.TIPO_DIA
    TIPO_AJUSTE = RealDiaPago.TIPO_AJUSTE
    objects = Memoria()

    def __new__(cls, *args, **kwargs):
        return RealDiaPago(*args, **kwargs)

# L = snapshot dos locals de recalcular_fechamento_mes,
# rodado exclusivamente para este colaborador.
r = L["resultado"]
with patch.object(modelos, "DiaPago", DiaPagoEmMemoria):
    lavrar(
        colab, mes, 2026,
        periodos=r.periodos,
        ft_certa=L["_ft_certa"],
        ft_feriado=L["_ft_feriado"],
        ft_sem_escala=L["_ft_sem_escala"],
        orfaos_por_dia=L["_orfaos_por_dia"],
        previsto_por_dia=L["_prev_dia_m"],
        realizado_por_dia=L["_real_dia_m"],
        dia_da_jornada=getattr(r, "dia_da_jornada", None),
        versao="motor",
    )
```

4. Detectar aberto pela saída **real** de `MotorBase._turnos_do_juiz`, capturada no evento `return` pelo objeto de código. O retorno contém `Turno(data_turno, entrada, saida, batidas, aberto, cross_meianoite)`; filtrar `entrada is not None`, `saida is None`, `aberto`, e `data_ini <= data_turno <= data_fim` dos argumentos desse frame. A geometria desta porta responde pelo dia e pelo intervalo; os períodos e minutos continuam sendo os do motor. `app/ponto/turnos.py:41`, `app/ponto/motor_calculo_v2.py:867`, `app/ponto/motor_calculo_v2.py:882`, `app/ponto/motor_calculo_v2.py:2346`.

5. Como informação adicional, comparar com `p.turno_aberto` nos períodos finais, agrupados por `resultado.dia_da_jornada[id(p)]`. O mapa preserva os mesmos objetos na composição de fatias. `app/ponto/services/fechamento.py:292`, `app/ponto/services/fechamento.py:302`.

O código já admite crédito de horas em período ainda marcado aberto, a partir dos pares fechados existentes no turno; isso impede interpretar automaticamente `aberto E horas>0` como pagamento de um dia cheio. A conclusão para col882 e para a frota depende da execução da sonda, ainda [não verificado]. `app/ponto/motor_calculo_v2.py:2495`, `app/ponto/motor_calculo_v2.py:2514`, `app/ponto/motor_calculo_v2.py:2558`.

Erros internos do fechamento podem ser capturados e consumidos pelo próprio serviço; retorno ausente para o colaborador deve virar erro, nunca zero. A grade também pode ficar sem resposta (`minutos_* = None`), e os mapas vazios não devem ser apresentados como minutos zero. `app/ponto/services/fechamento.py:1071`, `app/ponto/services/fechamento.py:1078`, `app/ponto/services/fechamento.py:1287`.

## 06/11 20:11 · sessao f922a2f8576e · PUSH-PRESO-14 · verificador · origem=Code

**Meu veredito: os três commits R1 publicam ou corrigem medições; o quarto implementa idempotência pela chave do fato e reutiliza autoridades existentes.** Evidências abaixo usam `hash:arquivo:linha` com a numeração nova dos diffs anexados. (`36c90132:app/core/placar_estrutural.py:106–120`; `6ed6adbe:app/core/placar_estrutural.py:106–119`; `a16637ca:app/core/placar_estrutural.py:108–118`; `5a3f8d0e:app/pautas/management/commands/pauta_pago_x_devido_exportada.py:130–139,190–213`)

1. **`36c90132` — nenhum termo arquitetural é realizado pelo diff mostrado.** Ele substitui números e provas, troca `fonte_do_medido` e remove afirmações sobre destinos que a nova medição não responde; os estados dos itens permanecem iguais. (`36c90132:app/core/placar_estrutural.py:106–120`; `36c90132:app/core/espelho_verdade.py:168–180`)

   Dois candidatos seriam **Single Source of Truth / fonte única** e **Architectural Invariants + Fitness Functions**, existentes no vocabulário. Porém, atualizar referências para a mesma medição não implementa uma autoridade única; publicar o resultado da SOMA também não implementa sua verificação. Neste diff, ambos permanecem conteúdo do placar. (`app/docs/GLOSSARIO.md:11,14`; `36c90132:app/core/placar_estrutural.py:106–120`)

2. **`6ed6adbe` — nenhum termo arquitetural é realizado pelo diff mostrado.** Ele reorganiza provas para apresentar números primeiro, restringe afirmações ao universo medido e corrige a atribuição da conferência da SOMA para a sonda. (`6ed6adbe:app/core/espelho_verdade.py:49,82,168–180`; `6ed6adbe:app/core/placar_estrutural.py:106–119`)

   Os candidatos seriam novamente **fonte única** e **fitness function / selo**. O diff corrige a descrição das fontes e verificações, mas não altera o mecanismo que impõe uma fonte única nem acrescenta a verificação executável; as mudanças de teste narradas na mensagem não aparecem no diff anexado. (`app/docs/GLOSSARIO.md:11`; `app/docs/PLANO-PADROES.md:48`; `6ed6adbe:app/core/placar_estrutural.py:106–119`; `6ed6adbe:app/core/espelho_verdade.py:49,82,168–180`)

3. **`a16637ca` — nenhum termo arquitetural é realizado pelo diff mostrado.** Ele compacta números e ressalvas nas provas, corrige `SOMA.fonte` e documenta a partição `pk % 6`; não implementa essa partição. A compactação tem efeito concreto na publicação porque o leitor recorta a prova em 150 caracteres. (`a16637ca:app/core/espelho_verdade.py:49,82,168–180`; `a16637ca:app/core/placar_estrutural.py:108–118`; `bin/gerar_estado.py:164`)

   Dois candidatos seriam **fitness function / selo** e **Shadow mode / Parallel run — sombra**. O diff apenas relata a conferência e a execução em sombra; não acrescenta um selo nem um caminho de execução paralela. (`app/docs/PLANO-PADROES.md:48`; `app/docs/GLOSSARIO.md:19`; `a16637ca:app/core/placar_estrutural.py:108–118`)

4. **`5a3f8d0e` — realiza principalmente `idempotencia pela chave do fato` / `dedup do fato`.** A chave contém marca, colaborador e competência; a busca acrescenta a âncora da empresa/competência e considera cabeças não canceladas, inclusive feitas. Dentro da transação, trava o colaborador, consulta novamente a existência e só cria quando ausente. Isso implementa o mecanismo, além de declará-lo. (`app/docs/GLOSSARIO.md:27`; `5a3f8d0e:app/pautas/management/commands/pauta_pago_x_devido_exportada.py:53–54,173–174,190–213,240–243`)

   O segundo candidato, **single source of truth / um juiz por pergunta**, também é realizado no consumo das autoridades: PAGO vem de `valores_do_gravado`, DEVIDO do recalculador existente em modo leitura, DIFERE de `_diferente` e ENTREGUE de `competencia_entregue`. Para escolher um único rótulo principal, escolheria **`padrao=idempotencia pela chave do fato`**, pela implementação explícita de deduplicação serializada. (`app/docs/PLANO-PADROES.md:46–47`; `5a3f8d0e:app/pautas/management/commands/pauta_pago_x_devido_exportada.py:109–139,190–213`)

Quanto ao comportamento:

- **Os três R1 alteram informação e apresentação do placar, sem modificar cálculo financeiro ou registro de batidas.** No primeiro, trocar a fonte também pode mudar a data reconhecida e, consequentemente, o veredito de cadência do termômetro; portanto há efeito no placar além da prosa. (`36c90132:app/core/placar_estrutural.py:111`; `6ed6adbe:app/core/espelho_verdade.py:49`; `a16637ca:app/core/espelho_verdade.py:168`; `bin/gerar_estado.py:47–56`; `app/core/placar_estrutural.py:676–684`)
- **O quarto não é só medida:** por padrão é leitura, mas `--escrever` cria pautas DP e `--reverter` cancela pautas. Ele consulta o cálculo financeiro em `somente_leitura=True`, sem habilitar a lavratura de `DiaPago`; não introduz alteração de regra de dinheiro nem escrita de batidas. (`5a3f8d0e:app/pautas/management/commands/pauta_pago_x_devido_exportada.py:135,161–188,207–217,259–283`; `app/ponto/services/fechamento.py:439–442,1112–1128,1175`)
- Execução efetiva desses caminhos e efeitos observados em produção: **[não verificado]**.

A recusa mecânica dos três primeiros decorre da classificação por caminho: `app/`, fora de testes e documentos, é “negócio” e recusa `n/a`. Isso não demonstra que realizem um padrão; a própria L-123 exige conferir o rótulo contra o diff. (`bin/tests/test_lei_akita.sh:122–135`; `app/docs/PLANO-PADROES.md:53,58`)

## 06/11 20:07 · sessao 631ab37d7510 · BUG-PLANTIO-NOTURNO-MAIS-1 · verificador · origem=Code

**As três curas fecham os caminhos locais apontados, com ressalvas:** (a) acrescenta um caso de push sem pergunta; (b) evita a exceção, mas não garante recuperação posterior; (c) cobre inserção recente, enquanto reaproveitamento conserva a criação antiga. `app/chamados/services/disputa_emissao.py:411`, `app/chamados/services/disputa_emissao.py:640`, `app/ponto/services/triagem_batida.py:730`, `app/chamados/services/materializacao.py:922`.

RED real, execução Django contra o pai e ocorrência em produção: **[nao verificado]**. A avaliação dos testes abaixo é estática, usando o delta fornecido e os caminhos citados. [nao verificado]

**1. Curas e alcance de (a)**

- **(a) Fecha o fallback defeituoso:** sem dia, uma pergunta `hora` agora para antes de `abrir_conversa`; perguntas de outros tipos continuam podendo nascer como conversa. `app/chamados/services/disputa_emissao.py:410`, `app/chamados/services/disputa_emissao.py:417`.
- **(b) Fecha `None.isoformat()` na triagem:** `_T1` é consultado uma vez e a disputa só nasce quando há dia. `app/ponto/services/triagem_batida.py:726`, `app/ponto/services/triagem_batida.py:739`.
- **(c) Fecha a exclusão da inserção retroativa recente:** troca o filtro temporal, preservando os demais critérios e a consulta ao mesmo juiz. Não achei juiz ou escritor novo nessa alteração. `app/ponto/management/commands/vigia_plantio_overnight.py:73`, `app/ponto/management/commands/vigia_plantio_overnight.py:76`, `app/ponto/management/commands/vigia_plantio_overnight.py:87`.

**Caminho legítimo “nasce conversa-hora e outro escritor preenche o dia depois”: nao achei.** A conversa já recebe `localdate()` no nascimento; `reapontar_resolvedora` só escreve a data quando ela está vazia. Portanto, esse escritor posterior não corrigiria o “hoje” daquela conversa. `app/chamados/models.py:2426`, `app/chamados/services/fio.py:835`.

Os quatro motivos `hora` do catálogo são `orfao_14h`, `saida_sem_entrada`, `intervalo_saida` e `intervalo_volta`. Os intervalos sem dia já eram recusados antes da guarda nova; o caso demonstrável atingido por ela é órfão/SSE com texto produzido, mas sem data válida após os fallbacks. `app/chamados/catalogo/perguntas.py:48`, `app/chamados/catalogo/perguntas.py:56`, `app/chamados/catalogo/perguntas.py:124`, `app/chamados/catalogo/perguntas.py:132`, `app/chamados/catalogo/query.py:66`, `app/chamados/services/disputa_emissao.py:387`.

**A acusação não desaparece deixando somente log:** motivos e contexto são persistidos antes da geração. A recusa específica, entretanto, fica apenas no `logger.info`, sem pergunta ou pendência própria. `app/chamados/services/disputa_emissao.py:532`, `app/chamados/services/disputa_emissao.py:557`, `app/chamados/services/disputa_emissao.py:632`, `app/chamados/services/disputa_emissao.py:415`.

**Efeito adicional concreto:** após recusar a única pergunta, `criar_ou_atualizar_disputa` ainda pode mandar “Toque para responder”. A decisão de enviar push ignora o resultado do gerador. Esse desacoplamento já existia; a cura acrescenta um caso que o alcança. `app/chamados/services/disputa_emissao.py:640`, `app/chamados/services/disputa_emissao.py:652`, `app/chamados/services/disputa_emissao.py:656`, `app/chamados/services/disputa_emissao.py:1577`.

Chamadores diretos encontrados fora dos testes: [nao verificado]

| Chamador | O que espera |
|---|---|
| `criar_ou_atualizar_disputa` | Geração como efeito; ignora a lista retornada e devolve a disputa. `app/chamados/services/disputa_emissao.py:640`, `app/chamados/services/disputa_emissao.py:652`, `app/chamados/services/disputa_emissao.py:658`. |
| `perguntar_entradas_pendentes` | Injeta dia obtido da batida via `_T1`, acumula a lista e salva contexto se ela não estiver vazia. `app/chamados/services/disputa_emissao.py:282`, `app/chamados/services/disputa_emissao.py:292`, `app/chamados/services/disputa_emissao.py:294`. |
| Fábrica `perguntar` | Fornece dia explícito; chama criação/atualização e novamente o gerador; ignora a lista. `app/chamados/services/fabrica_da_celula.py:139`, `app/chamados/services/fabrica_da_celula.py:141`. |
| `smoke_chamados` | Fornece data quando exigida e espera uma pergunta com o tipo do catálogo. `app/chamados/management/commands/smoke_chamados.py:96`, `app/chamados/management/commands/smoke_chamados.py:102`, `app/chamados/management/commands/smoke_chamados.py:104`. |

O contrato documentado é “lista das perguntas criadas”, mas a implementação também acrescenta pergunta reaproveitada, com `_criada=False`; isso antecede o delta. `app/chamados/services/disputa_emissao.py:304`, `app/chamados/services/disputa_emissao.py:449`, `app/chamados/models.py:2497`.

**2. SSE sem disputa: há recuperação?**

**Recuperador específico dessa tentativa/batida retratada: nao achei.** `registrar_batida` pode devolver a mesma chave retratada sem inserir nem salvar; `_T1` exclui retratadas. A guarda então pula a disputa e a triagem pode chegar à indicação de sucesso, sem registrar pendência própria nesse ramo. `app/ponto/registro_batida.py:120`, `app/ponto/turnos.py:2905`, `app/ponto/turnos.py:2925`, `app/ponto/services/triagem_batida.py:730`, `app/ponto/services/triagem_batida.py:757`.

**Há recuperação condicional pelo dia:** o código declara `vigia_de_hora --apply` a cada dez minutos e cartório diário. O julgamento pode chamar a fábrica, que emite perguntas pela célula. Isso não equivale a reprocessar aquela tentativa. `app/config/crons.py:367`, `app/config/crons.py:616`, `app/ponto/services/cartorio.py:981`, `app/ponto/services/cartorio.py:997`.

Não há garantia universal: o vigia seleciona células `trabalha=True`; a fábrica pode recusar por situação, veredito ou parcial, e só pergunta motivos ainda ausentes. Portanto, **pode permanecer sem pergunta; “sempre invisível” também não está demonstrado**. `app/ponto/services/vigia_de_hora.py:21`, `app/chamados/services/fabrica_da_celula.py:91`, `app/chamados/services/fabrica_da_celula.py:99`, `app/chamados/services/fabrica_da_celula.py:102`, `app/chamados/services/fabrica_da_celula.py:126`. Recuperação efetiva em produção: **[nao verificado]**.

A cura é local à triagem: API e clone ainda chamam `.isoformat()` sem essa guarda. `app/api/views.py:782`, `app/api/views_core.py:782`.

**3. Quem escreve `Batida.criado_em`; reabertura e reaproveitamento**

O escritor normal é `auto_now_add=True`, na inserção da linha. A porta cria por `Batida.objects.create`; `timestamp` é um campo separado. `app/ponto/models.py:23`, `app/ponto/models.py:79`, `app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`.

**Reescritor Python específico de `Batida.criado_em`: nao achei.** A porta de anotação não aceita esse campo e salva somente os campos solicitados. `app/ponto/registro_batida.py:203`, `app/ponto/registro_batida.py:241`, `app/ponto/registro_batida.py:254`.

Há restauração genérica de schema por dump: ela restaura os valores arquivados, sem passar pelo `auto_now_add` ou renovar a criação para o horário do restore. Uso efetivo em produção: **[nao verificado]**. `scripts/backup_tenant.sh:10`, `scripts/restore_tenant.sh:8`.

**Batida nova tem criação do novo plantio; reaproveitada conserva a criação original.** O materializador procura existente antes de criar e, no reaproveitamento, pode trocar apenas `pergunta_origem`. A reabertura do questionário limpa resposta/validação, sem renovar a batida. `app/chamados/services/materializacao.py:835`, `app/chamados/services/materializacao.py:847`, `app/chamados/services/materializacao.py:903`, `app/chamados/services/materializacao.py:922`, `app/chamados/services/disputa_emissao.py:1434`.

Assim, uma `disputa_s84_retro` antiga revinculada hoje pode ficar fora da janela; uma `app` reaproveitada permanece fora pelo filtro de origem. Esse limite impede interpretar o contador como cobertura de **toda aplicação recente**. `app/ponto/management/commands/vigia_plantio_overnight.py:16`, `app/ponto/management/commands/vigia_plantio_overnight.py:76`, `app/chamados/services/materializacao.py:924`.

**4. Os testes mordem o pai?** [nao verificado]

As conclusões desta tabela são previsões estáticas contra os trechos anteriores mostrados no delta; execução no pai: **[nao verificado]**.

| Teste | Resultado sem a cura correspondente | [nao verificado]
|---|---|
| `PerguntaDeHoraSemDiaDoTurnoTest.test_MORDE_orfao...` | **FALHA.** Data malformada produz texto, mas não dia; sem a guarda, nasce conversa datada de hoje e viola `assertFalse`. `app/chamados/tests/test_plantio_noturno_mais1.py:203`, `app/chamados/tests/test_plantio_noturno_mais1.py:206`, `app/chamados/tests/test_plantio_noturno_mais1.py:207`, `app/chamados/catalogo/query.py:66`, `app/chamados/models.py:2426`. |
| `test_contra_exemplo_escolha_sem_dia_segue_conversa` | **PASSA mesmo sem a cura.** O motivo é `escolha`; a conversa já era o resultado anterior. `app/chamados/tests/test_plantio_noturno_mais1.py:210`, `app/chamados/tests/test_plantio_noturno_mais1.py:214`, `app/chamados/catalogo/perguntas.py:84`. |
| `VigiaContaPeloPlantioNaoPeloInstanteTest.test_MORDE...` | **FALHA.** Em 01/12, a saída com instante de 23/10 fica fora do corte de trinta dias do pai; o novo `criado_em` de 01/12 a inclui. `app/chamados/tests/test_plantio_noturno_mais1.py:30`, `app/chamados/tests/test_plantio_noturno_mais1.py:32`, `app/chamados/tests/test_plantio_noturno_mais1.py:192`, `app/chamados/tests/test_plantio_noturno_mais1.py:195`, `app/ponto/management/commands/vigia_plantio_overnight.py:76`. |
| Caso congelado de `test_a2` | **PASSA no pai com esse congelamento.** Não é RED das curas: o motivo é `escolha` e a ausência da linha já levava à conversa. O congelamento elimina a contradição que ocorreria em 12/08/2026 entre `!=12/08` e `==localdate()`. `app/chamados/tests/test_a2_data_evento_batida.py:51`, `app/chamados/tests/test_a2_data_evento_batida.py:56`, `app/chamados/tests/test_a2_data_evento_batida.py:59`, `app/chamados/tests/test_a2_data_evento_batida.py:60`, `app/chamados/services/disputa_emissao.py:383`. |

O teste de hora mata precisamente o fallback antigo, mas só proíbe nascimento **hoje**, não qualquer nascimento indevido. O teste anterior do vigia continua aceitando qualquer delta `>=1`. `app/chamados/tests/test_plantio_noturno_mais1.py:207`, `app/chamados/tests/test_plantio_noturno_mais1.py:178`.

**5. Escritores, leitores e ESMERIL-SEMANTICA**

| Campo | Quem escreve/fornece | Quem lê |
|---|---|---|
| `PerguntaDisputa.data_evento` | Gravação efetiva em `_nascer`; preenchimento de pergunta sem dia em `reapontar_resolvedora`. Gerador, agrupador e declarações fornecem o valor; conversa fornece `localdate()`. `app/chamados/models.py:2498`, `app/chamados/services/fio.py:837`, `app/chamados/services/disputa_emissao.py:430`, `app/chamados/services/disputa_emissao.py:1029`, `app/chamados/services/declaracao_texto.py:327`, `app/chamados/models.py:2426`. | Plantio/slot como dia do turno; dedup/fábrica como identidade do dia; juízes/reconciliador como dia do fato; folha/calendário como agrupamento; API como apresentação. `app/chamados/services/materializacao.py:344`, `app/chamados/juizes.py:319`, `app/chamados/models.py:2396`, `app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/reconciliador.py:1080`, `app/folha/previa.py:67`, `app/colaboradores/services/calendario.py:477`, `app/api/views.py:1174`. |
| `contexto['saida_sem_entrada']` | Triagem fornece dia de `_T1`; APIs substituem a data civil transitória por `_T1`; fábrica fornece dia da célula; agregador persiste/mescla. `app/ponto/services/triagem_batida.py:739`, `app/api/views.py:782`, `app/api/views_core.py:782`, `app/chamados/services/fabrica_da_celula.py:139`, `app/chamados/services/disputa_emissao.py:529`, `app/chamados/services/disputa_emissao.py:560`. | Texto, data da pergunta, seleção de chamado e fallback de plantio/completude. `app/chamados/catalogo/query.py:58`, `app/chamados/services/disputa_emissao.py:364`, `app/chamados/services/disputa_emissao.py:196`, `app/chamados/services/materializacao.py:343`, `app/chamados/juizes.py:365`. |
| `Batida.criado_em` | Inserção automática; restore recupera valor arquivado. `app/ponto/models.py:79`, `scripts/restore_tenant.sh:8`. | Vigia como criação recente; detector de flush como chegada ao servidor; contador de sequência como corte de criação; cache/cartório como marco de fato novo. `app/ponto/management/commands/vigia_plantio_overnight.py:76`, `app/ponto/turnos.py:355`, `app/ponto/turnos.py:366`, `app/ponto/turnos.py:2384`, `app/chamados/veredito_lavrado.py:138`, `app/ponto/services/cartorio.py:353`. |

**Divergências encontradas:**

- **Permanece a conhecida conversa × turno:** nascimento civil da conversa e leitores de identidade/célula sem discriminador persistido. A cura elimina o caminho `hora` do gerador, não a colisão geral já declarada fora do escopo. `app/chamados/models.py:2426`, `app/chamados/models.py:2453`, `app/chamados/services/fabrica_da_celula.py:118`.
- **Criação × reaplicação:** `criado_em` significa nascimento da linha; o vigia o chama de plantio recente, embora a aplicação possa reaproveitar linha antiga. `app/ponto/models.py:79`, `app/ponto/management/commands/vigia_plantio_overnight.py:67`, `app/chamados/services/materializacao.py:835`.
- **Leitor atual de `saida_sem_entrada.data` especificamente como dia civil da saída: nao achei.** Texto pergunta entrada naquele dia, e plantio usa dia do turno. Permanece a diferença de chave nas APIs: motivo `orfao_14h`, contexto `saida_sem_entrada`. `app/chamados/catalogo/perguntas.py:57`, `app/chamados/juizes.py:319`, `app/api/views.py:654`, `app/api/views.py:655`.

**6. Classes que já reprovaram obra**

| Classe | Resultado nesta volta |
|---|---|
| Segundo juiz | **Novo no delta: nao achei.** Triagem e vigia consultam `_T1`/`dia_das_batidas`. `app/ponto/services/triagem_batida.py:729`, `app/ponto/management/commands/vigia_plantio_overnight.py:87`. |
| Segundo escritor fora da porta | **Efetivo novo: nao achei.** Escritas encontradas continuam no nascimento e costura. `app/chamados/models.py:2498`, `app/chamados/services/fio.py:837`. |
| Relógio fora da borda | **Permanece no helper do vigia**, sem instante recebido do chamador; a troca do filtro não o criou. `app/ponto/management/commands/vigia_plantio_overnight.py:76`. |
| Derivação paralela | **Não acrescentada pelo delta; permanece na fila:** parser de contexto no gerador/agrupador e contexto fraco. `app/chamados/services/disputa_emissao.py:367`, `app/chamados/services/disputa_emissao.py:1107`, `app/chamados/data_evento.py:27`, `app/chamados/juizes.py:162`. |
| Selo/RED sem mordida | **Os novos casos principais de (a) e (c) têm mordida estática.** Permanecem as lacunas do selo para `**dict` e `setattr` por variável; o caso congelado é estabilização, não RED comportamental do delta. `app/chamados/tests/test_plantio_noturno_mais1.py:195`, `app/chamados/tests/test_plantio_noturno_mais1.py:207`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:86`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:91`, `app/chamados/tests/test_a2_data_evento_batida.py:51`. |
| Campo com dois significados | **Persistem conversa/turno e o limite criação/reaplicação descritos acima.** O delta não resolve o discriminador nem registra um instante próprio de reaproveitamento. `app/chamados/models.py:2450`, `app/chamados/models.py:2498`, `app/chamados/services/materializacao.py:924`. |

## 06/11 20:05 · sessao 8a7c282839f5 · ESMERIL-SEM-INSTANTE · verificador · origem=Code

**Não encontrei bloqueante novo no delta fornecido; os três achados procedentes foram corrigidos.** (`app/ponto/tests/test_composicao_de_fatias_e_total.py:281`, `:333`; `app/ponto/tests/test_contract_no_batida_date.py:72`)

1. **As três curas conferem.**

   - **Teste 3:** agora exige exatamente `[utc[0], utc[1], utc[3]]` em A. Revertendo a conversão para `b.timestamp.date()`, A recebe `[utc[2], utc[4]]`: o novo assert falha mesmo isolado da igualdade anterior. Se o filtro entregar `[]`, também falha; se ambos os resultados forem `{}`, ocorre `KeyError` em `A.pk`, portanto não passa silenciosamente. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:325`, `:330`, `:333`; `app/ponto/services/fechamento.py:204`, `:254`, `:261`)
   - **Gravador:** deixou de ordenar e registra a sequência recebida pelo motor. Está curado o mascaramento introduzido pelo próprio capturador. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:279`, `:281`)
   - **ALLOWLIST:** a justificativa agora aponta precisamente para `_com_batida`, que ainda contém `timezone.localtime(b.timestamp).date()` e é reconhecido pelo visitante. (`app/ponto/tests/test_contract_no_batida_date.py:72`, `:208`; `app/ponto/services/fechamento.py:793`)

2. **Não há relógio solto, import morto novo ou dependência indevida de ordem nas alterações fornecidas.** Os instantes são literais, o fuso fica sob `override`, e a ordem esperada vem da lista entregue explicitamente pelo teste. As compreensões da porta preservam essa sequência; não dependem da ordem de conjuntos ou de empates no banco. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:245`, `:325`, `:327`, `:328`, `:333`; `app/ponto/services/fechamento.py:204`, `:253`, `:260`, `:262`)

   **Limite da cobertura:** o gravador foi corrigido, mas o teste ainda não prova preservação de uma entrada arbitrariamente embaralhada: os três sobreviventes já estão em ordem cronológica. Uma ordenação indevida na porta ainda poderia passar neste caso. Isso não invalida a cura do capturador. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:281`, `:325`, `:326`, `:333`)

3. **Não mantenho os descartes como bloqueantes desta fatia.** A restrição para naive existe, mas os três caminhos encontrados carregam listas pelo ORM, com `DateTimeField` e `USE_TZ=True`; não há origem naive demonstrada nesses caminhos. Isso sustenta o contrato aware sem provar os valores efetivamente recebidos em produção **[nao verificado]**. (`app/ponto/services/fechamento.py:633`; `app/ponto/services/espelho.py:1022`; `app/relatorios/pdf_espelho.py:272`; `app/ponto/models.py:23`; `app/config/settings/base.py:95`; `app/ponto/turnos.py:44`)

   `None` continua inválido: a conversão devolve `None`, que falha na comparação com datas; não há tratamento novo que prometa aceitá-lo. (`app/ponto/turnos.py:46`; `app/ponto/services/fechamento.py:254`)

4. **Nenhum bloqueante novo identificado.** O residual de saída após meia-noite sob células alternadas permanece: o filtro ainda pode excluir a saída pelo dia civil de outro vínculo. A correção desse residual não é demonstrada por estes asserts. (`app/ponto/services/fechamento.py:255`, `:261`; `app/ponto/tests/test_composicao_de_fatias_e_total.py:333`)

**Validação:** em memória, com dependências substituídas, os três testes passaram na cópia e falharam nos mutantes `.date()` e filtro vazio; o assert novo também falhou isoladamente. Os quatro testes do contrato passaram isolados. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:291`, `:306`, `:316`, `:333`; `app/ponto/tests/test_contract_no_batida_date.py:236`)

Suíte Django real, correspondência Git com o hash/base e integralidade do delta: **[nao verificado]**. [nao verificado]

## 06/11 20:05 · sessao 2d4a03cfa6b6 · - · - · origem=Ronald (tmux)

**(b) fecha a exceção na triagem, mas não garante uma pergunta posterior.** O retorno de `registrar_batida` pode ser a mesma chave já retratada, sem criação ou `save`; `_T1` não inclui retratadas e devolve `None`; a nova guarda evita `.isoformat()` nesse retorno. Referências: `app/ponto/registro_batida.py:116`, `app/ponto/registro_batida.py:120`, `app/ponto/turnos.py:2905`, `app/ponto/turnos.py:2917`, `app/ponto/turnos.py:2925`, `app/ponto/services/triagem_batida.py:726`, `app/ponto/services/triagem_batida.py:730`.

**Outro emissor que retome especificamente essa tentativa/batida retratada: nao achei.** O retorno antecipado de mesma chave não dispara os sinais de criação, e a triagem segue até informar sucesso; a nova guarda não grava pendência nem emite log próprio. Referências: `app/ponto/registro_batida.py:120`, `app/ponto/services/triagem_batida.py:730`, `app/ponto/services/triagem_batida.py:757`.

Há emissores independentes capazes de perguntar **pelo dia**, sob condições: vigia de hora declarado a cada dez minutos e cartório diário; o cartório chama a fábrica depois do julgamento, e a fábrica exige célula elegível, veredito acusador e motivo ainda sem pergunta. Portanto, não cabe afirmar “fica sempre invisível”, nem “o cron garante recuperação”. Referências: `app/config/crons.py:367`, `app/config/crons.py:616`, `app/ponto/services/cartorio.py:981`, `app/ponto/services/cartorio.py:997`, `app/chamados/services/fabrica_da_celula.py:91`, `app/chamados/services/fabrica_da_celula.py:99`, `app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/fabrica_da_celula.py:164`.

Limites concretos: `julgar_celula` recusa célula inexistente; vigia de hora seleciona `trabalha=True`; a fábrica também pode calar por parcial/lista; o lote `disparar_perguntas_competencia` apenas conta o caminho da célula e só emite no caminho de chamado vivo já existente com célula. Referências: `app/ponto/services/cartorio.py:1976`, `app/ponto/services/vigia_de_hora.py:21`, `app/chamados/services/fabrica_da_celula.py:102`, `app/chamados/management/commands/disparar_perguntas_competencia.py:86`, `app/chamados/management/commands/disparar_perguntas_competencia.py:141`, `app/chamados/management/commands/disparar_perguntas_competencia.py:158`. Recuperação efetiva em produção: **[nao verificado]**.

**Censo `DisputaSupervisao.contexto['saida_sem_entrada']`:**

| Papel | Evidência |
|---|---|
| PWA fornece `data` de `_T1` e hora da tentativa | `app/ponto/services/triagem_batida.py:734`, `app/ponto/services/triagem_batida.py:739` |
| API e clone montam data civil transitória e a substituem por `_T1` antes de persistir | `app/api/views.py:655`, `app/api/views.py:780`, `app/api/views_core.py:652`, `app/api/views_core.py:780` |
| Fábrica fornece data explícita da célula, pelo motivo | `app/chamados/services/fabrica_da_celula.py:139` |
| Agregador persiste/mescla o contexto | `app/chamados/services/disputa_emissao.py:518`, `app/chamados/services/disputa_emissao.py:532`, `app/chamados/services/disputa_emissao.py:560` |
| Texto lê `data` para perguntar entrada naquele dia | `app/chamados/catalogo/query.py:58`, `app/chamados/catalogo/perguntas.py:57` |
| Gerador lê `data` como dia da pergunta datada | `app/chamados/services/disputa_emissao.py:364` |
| Seleção de chamado compara `data` com dia do fato | `app/chamados/services/disputa_emissao.py:191`, `app/chamados/services/disputa_emissao.py:231` |
| Materializador usa como fallback do dia do turno | `app/chamados/services/materializacao.py:343`, `app/chamados/services/materializacao.py:346` |
| Juiz de completude usa como fallback do turno | `app/chamados/juizes.py:363`, `app/chamados/juizes.py:365`, `app/chamados/juizes.py:372` |

**Leitor atual desse `data` especificamente como dia civil da saída: nao achei.** A conversão de overnight usa dia do turno e marcos; o recuo antigo por `hora_tentativa` está removido. Referências: `app/chamados/juizes.py:301`, `app/chamados/juizes.py:312`, `app/chamados/juizes.py:319`, `app/chamados/services/materializacao.py:419`.

Duas ressalvas preexistentes: API/clone acrescentam motivo `orfao_14h`, mas gravam a chave de contexto `saida_sem_entrada`; ambos continuam chamando `.isoformat()` sem guarda de `None`. Não são mudanças do delta fornecido. Referências: `app/api/views.py:654`, `app/api/views.py:655`, `app/api/views.py:782`, `app/api/views_core.py:651`, `app/api/views_core.py:652`, `app/api/views_core.py:782`.

## 06/11 20:04 · sessao be24a03c3f1c · - · - · origem=Ronald (tmux)

**Cura (c): fecha a omissão de inserção retroativa recente; não cobre toda reaplicação recente de pergunta.** O filtro passa a usar `criado_em`, preservando os demais critérios, e consulta o mesmo juiz `dia_das_batidas`; não achei segundo juiz nem escritor novo nessa mudança. Referências: `app/ponto/management/commands/vigia_plantio_overnight.py:73`, `app/ponto/management/commands/vigia_plantio_overnight.py:76`, `app/ponto/management/commands/vigia_plantio_overnight.py:87`.

**Escrita de `Batida.criado_em`:**

- É `DateTimeField(auto_now_add=True)`: registra nascimento da linha, enquanto `timestamp` registra instante da marcação. A porta insere por `Batida.objects.create`. Referências: `app/ponto/models.py:23`, `app/ponto/models.py:79`, `app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`.
- Reescritor Python específico por atribuição/update/SQL/cópia/restauração de `Batida.criado_em`: **nao achei**. A porta de anotação não aceita esse campo e salva somente os campos pedidos. Referências: `app/ponto/registro_batida.py:203`, `app/ponto/registro_batida.py:241`, `app/ponto/registro_batida.py:254`.
- Existe carga sintética em lote, restrita a banco de ensaio, construída com os dicionários recebidos. Referências: `app/ponto/registro_batida.py:316`, `app/ponto/registro_batida.py:326`, `app/ponto/registro_batida.py:331`, `app/ponto/registro_batida.py:335`.
- Há restauração genérica de schema: backup por `pg_dump`, restauração por `pg_restore --clean --if-exists`. Esse caminho restaura o valor do dump, sem passar pelo `auto_now_add`; não carimba a hora do restore. Uso efetivo e valores restaurados em produção: **[nao verificado]**. Referências: `scripts/backup_tenant.sh:10`, `scripts/restore_tenant.sh:8`.

**Reabertura/reaproveitamento:**

- Materialização procura batida existente antes de criar; aceita tanto real quanto de disputa. Se reaproveita, apenas pode mudar `pergunta_origem`, mantendo `criado_em` e `origem`. Referências: `app/chamados/juizes.py:230`, `app/chamados/juizes.py:238`, `app/chamados/services/materializacao.py:835`, `app/chamados/services/materializacao.py:847`, `app/chamados/services/materializacao.py:922`, `app/ponto/registro_batida.py:254`.
- A própria porta também pode devolver linha antiga pelo dedup ±60s ou pela mesma chave, sem inserir. Referências: `app/ponto/registro_batida.py:107`, `app/ponto/registro_batida.py:113`, `app/ponto/registro_batida.py:120`.
- Portanto, **batida realmente criada na nova aplicação tem `criado_em` desse plantio; batida reaproveitada mantém o nascimento original**. Uma linha `disputa_s84_retro` antiga revinculada hoje pode permanecer fora da janela; uma batida `app` reaproveitada continua fora pelo filtro de origem. Isso é limitação do significado “plantio recente”, não regressão criada pela troca do filtro. Referências: `app/ponto/models.py:79`, `app/chamados/services/materializacao.py:903`, `app/chamados/services/materializacao.py:922`, `app/ponto/management/commands/vigia_plantio_overnight.py:16`, `app/ponto/management/commands/vigia_plantio_overnight.py:76`.
- A reabertura do questionário limpa resposta/validação; não renova nascimento da batida. Referências: `app/chamados/services/acoes_disputa.py:102`, `app/chamados/services/disputa_emissao.py:1434`, `app/chamados/services/disputa_emissao.py:1454`.

**Teste `VigiaContaPeloPlantioNaoPeloInstanteTest`: FALHA sem a cura, por análise estática.** Cria a saída em 01/12, com `timestamp` 23/10, e exige seu PK em janela de 30 dias. No pai, o filtro `timestamp >= 01/11` fornecido no delta a exclui e produz `[]`; no delta, `criado_em` 01/12 a inclui. Referências: `app/chamados/tests/test_plantio_noturno_mais1.py:30`, `app/chamados/tests/test_plantio_noturno_mais1.py:32`, `app/chamados/tests/test_plantio_noturno_mais1.py:192`, `app/chamados/tests/test_plantio_noturno_mais1.py:195`, `app/ponto/management/commands/vigia_plantio_overnight.py:76`; código exato do pai: **[nao verificado por Git; disponível apenas no delta fornecido]**. Execução Django/RED real: **[nao verificado]**.

**Leitores de `Batida.criado_em` encontrados:**

| Leitor | Significado |
|---|---|
| Vigia novo | Nascimento recente da linha tomado como plantio recente. `app/ponto/management/commands/vigia_plantio_overnight.py:76`. |
| `_marca_ecos_flush` | Hora de chegada ao servidor versus instante do evento, para identificar flush offline. `app/ponto/turnos.py:355`, `app/ponto/turnos.py:362`, `app/ponto/turnos.py:366`. |
| `sequencia_relogio_x_jornada` | Universo de linhas criadas desde corte. `app/ponto/turnos.py:2378`, `app/ponto/turnos.py:2384`. |
| `marcos_do_lote` | Invalidação de cache por nascimento de linha; retração tem relógio separado. `app/chamados/veredito_lavrado.py:137`, `app/chamados/veredito_lavrado.py:140`. |
| Cartório | Fato novo desde julgamento: nascimento ou retração de batida na janela. `app/ponto/services/cartorio.py:350`, `app/ponto/services/cartorio.py:352`. |

**Classes regressivas pertinentes:** segundo juiz e segundo escritor novo, **nao achei**; relógio dentro do helper permanece (`timezone.now()`), e o delta apenas troca o campo filtrado. O teste novo tem mordida contra essa troca; o teste anterior continua exigindo só `delta >= 1`. Referências: `app/ponto/management/commands/vigia_plantio_overnight.py:76`, `app/ponto/management/commands/vigia_plantio_overnight.py:87`, `app/chamados/tests/test_plantio_noturno_mais1.py:178`, `app/chamados/tests/test_plantio_noturno_mais1.py:195`. A divergência semântica relevante é **nascimento da linha versus última aplicação/revinculação**, demonstrada pelo reaproveitamento sem atualização do carimbo. Referências: `app/ponto/models.py:79`, `app/chamados/services/materializacao.py:835`, `app/chamados/services/materializacao.py:922`.

## 06/11 20:04 · sessao 7d8006827f3a · - · - · origem=Ronald (tmux)

**Cura (a): fecha o caminho de conversa-hora; caminho legítimo de preencher o dia depois: nao achei.** Todas as perguntas de catálogo `hora` são `orfao_14h`, `saida_sem_entrada`, `intervalo_saida` e `intervalo_volta`; todas pedem `{data}`. Os dois intervalos sem dia já são recusados antes da cura; a guarda nova efetivamente alcança os motivos órfão/SSE que ainda chegam sem dia após os fallbacks. Referências: `app/chamados/catalogo/perguntas.py:48`, `:56`, `:124`, `:132`; `app/chamados/services/disputa_emissao.py:338`, `:387`, `:411`.

O caminho demonstrável é data malformada: o formatador aceita o texto bruto, o parser não produz dia, falta batida válida e falta chamado datado. Antes abriria conversa; agora registra `info` e continua. Referências: `app/chamados/catalogo/query.py:62`, `:66`; `app/chamados/services/disputa_emissao.py:364`, `:379`, `:399`, `:410`, `:415`.

A conversa anterior **já nascia com `localdate()`**; o redatador encontrado só escreve `data_evento` quando a pergunta não tem data. Logo o caminho “nasce conversa, depois outro escritor põe o dia correto” não se sustentava nesse código. Referências: `app/chamados/models.py:2426`; `app/chamados/services/fio.py:835`, `:837`. Outros escritores efetivos encontrados por busca: nao achei além do nascimento e dessa porta; fluxo dinâmico não coberto pela busca: **[nao verificado]**.

Não some toda a acusação: a criação/agregação grava motivos e contexto **antes** de gerar perguntas, além da lavra de abertura da disputa. A recusa específica não gera registro de pergunta. Referências: `app/chamados/services/disputa_emissao.py:511`, `:532`, `:535`, `:557`, `:567`, `:640`, `:415`.

**Efeito novo concreto a registrar:** o chamador ignora a lista retornada e continua enviando push “Toque para responder” mesmo se o gerador recusou a única pergunta. Isso já era possível por outras recusas, mas a cura acrescenta este caso. Referências: `app/chamados/services/disputa_emissao.py:640`, `:641`, `:652`, `:656`, `:1575`, `:1580`. Ocorrência/impacto em produção: **[nao verificado]**.

**Chamadores diretos encontrados fora de testes:** [nao verificado]

| Chamador | Expectativa |
|---|---|
| `criar_ou_atualizar_disputa` | Chama no nascimento e na agregação; ignora retorno e retorna disputa, com push separado. `app/chamados/services/disputa_emissao.py:640`, `:652`, `:658`. |
| `perguntar_entradas_pendentes` | Calcula dia via batida/`_T1`, injeta contexto e soma lista retornada; só salva contexto se recebeu perguntas. `app/chamados/services/disputa_emissao.py:282`, `:292`, `:294`, `:295`. |
| Fábrica `perguntar` | Fornece dia explícito, chama criar/atualizar e chama gerador novamente; ignora lista, retorna número de carimbos desfeitos. `app/chamados/services/fabrica_da_celula.py:129`, `:139`, `:141`, `:142`. |
| Comando `smoke_chamados` | Fornece data válida quando template exige, espera exatamente uma pergunta por motivo e tipo igual ao catálogo. `app/chamados/management/commands/smoke_chamados.py:90`, `:96`, `:102`, `:104`, `:109`. |

O gerador documenta “lista das perguntas criadas”, mas adiciona `p` mesmo quando `_criada=False`, comportamento anterior ao delta. Isso explica que lista não é prova estrita de criação. Referências: `app/chamados/services/disputa_emissao.py:304`, `:423`, `:449`; `app/chamados/models.py:2497`.

**Mordida dos testes — inferência estática, execução Django/RED no pai: [nao verificado].** [nao verificado]

- `PerguntaDeHoraSemDiaDoTurnoTest.test_MORDE...`: **falharia sem a cura (a)**. A fixture fornece motivo `orfao_14h`, data malformada, sem vínculo; o caminho anterior abriria conversa com o dia congelado e violaria `assertFalse(...data_evento=localdate()).exists()`. Referências: `app/chamados/tests/test_plantio_noturno_mais1.py:198`, `:203`, `:205`, `:206`, `:207`; `app/chamados/catalogo/query.py:66`; `app/chamados/models.py:2426`; guarda nova `app/chamados/services/disputa_emissao.py:411`.
- `test_contra_exemplo_escolha_sem_dia_segue_conversa`: **passaria sem a cura**. O catálogo dá `escolha`, e o fallback conversa anterior é precisamente o comportamento exigido. Referências: `app/chamados/tests/test_plantio_noturno_mais1.py:210`, `:213`, `:214`; `app/chamados/catalogo/perguntas.py:82`, `:84`; `app/chamados/models.py:2426`.
- Caso congelado `test_a2...test_MORDE_batida_sem_linha_nao_inventa_dia_pelo_timestamp`: **passaria na árvore pai em dias locais diferentes de 12/08/2026**; o delta só fixa o relógio em outubro. Não é RED das curas (a)-(c), pois motivo é `escolha` e ausência da linha já levava à conversa. Sem congelamento, no dia 12/08/2026 os asserts `!=12/08` e `==localdate()` eram incompatíveis. Referências: `app/chamados/tests/test_a2_data_evento_batida.py:51`, `:55`, `:56`, `:59`, `:60`; `app/chamados/catalogo/perguntas.py:84`; `app/chamados/services/disputa_emissao.py:379`, `:383`, `:417`.

Limite do teste de hora: ele proíbe somente nascimento **na data de hoje**, não qualquer nascimento errado em outra data; é suficiente para matar precisamente o fallback antigo. Referências: `app/chamados/tests/test_plantio_noturno_mais1.py:207`, `:208`; `app/chamados/models.py:2426`.

## 06/11 20:03 · sessao 65cdc82cca83 · - · - · origem=Ronald (tmux)

- **Sem relógio solto/import morto novos no delta:** as alterações executáveis só capturam timestamps sem `sorted` e acrescentam o assert; todos os instantes do caso 3 são literais aware e o fuso é fixado pelo `override`. O delta não altera imports. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:243`, `:280`, `:281`, `:324`, `:325`, `:327`, `:333`; diff fornecido [nao verificado] quanto à integralidade Git)
- **A ordem exigida é sustentada pelo chamador do teste:** `_rodar` recebe explicitamente a lista `utc` nessa sequência e a porta usa compreensões que preservam a ordem recebida. O assert espera os sobreviventes `[utc[0], utc[1], utc[3]]`. Não depende da ordem de `set`/dict nem de empate do banco. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:325`, `:328`, `:333`; `app/ponto/services/fechamento.py:204`, `:253`, `:260`, `:262`)
- **Ressalva não bloqueante:** retirar `sorted` cura o mascaramento no gravador, mas este novo assert ainda não detecta uma ordenação cronológica introduzida na porta: os três sobreviventes já estão em ordem crescente. O elemento final fora de ordem é excluído pelo filtro. Portanto, “gravador não ordena” está curado; “teste prova preservação de ordem para entrada arbitrária” não está demonstrado. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:281`, `:325`, `:326`, `:333`; `app/ponto/services/fechamento.py:254`)
- **Censo de produção:** os três chamadores encontrados carregam listas por `batidas_apuraveis`, cujo retorno usa `order_by('timestamp')`. O motor deduplica sequencialmente pela batida anterior, justificando preservar a ordem. (`app/ponto/services/fechamento.py:633`, `:754`; `app/ponto/services/espelho.py:1022`, `:1164`; `app/relatorios/pdf_espelho.py:272`, `:419`; `app/ponto/turnos.py:2871`; `app/ponto/motor_calculo_v2.py:905`)
- **Naive não sustentado como bloqueante:** a nova porta chama `data_local`, documentado para aware; a origem estática dos três caminhos é ORM, com `DateTimeField` sem `null=True` e `USE_TZ=True`. Valores realmente devolvidos em produção: **[nao verificado]**. Nada no delta reabre essa interface. (`app/ponto/services/fechamento.py:203`, `:204`; `app/ponto/turnos.py:43`, `:46`; `app/ponto/models.py:23`; `app/config/settings/base.py:95`)
- **Comentário da ALLOWLIST agora confere:** a ofensa restante é `_com_batida = {timezone.localtime(b.timestamp).date() ...}`. (`app/ponto/tests/test_contract_no_batida_date.py:72`; `app/ponto/services/fechamento.py:793`)
- **Novo bloqueante identificado: nenhum dentro deste delta**, limitado às três alterações fornecidas; comparação integral do commit/base permanece **[nao verificado]**. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:281`, `:333`; `app/ponto/tests/test_contract_no_batida_date.py:72`)

## 06/11 20:02 · sessao 99c2d369a999 · H12H06-B · sonda · origem=Code

Não executei a sonda: o invólucro cria arquivos e grava log, contrariando a restrição de somente leitura (`bin/sonda_leitura.sh:216`, `bin/sonda_leitura.sh:217`, `bin/sonda_leitura.sh:250`). Segue a transcrição integral do dado fornecido, sem nova execução: [nao verificado]

```text
sonda_leitura: pre.py [f35cf407c2faf784] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

HASH_FM {"05": [615, "e29ff62650eebac5"], "06": [704, "9400c1a360842d79"], "07": [629, "2edced101a39a6c3"], "08": [618, "22459b830139a009"], "09": [607, "6e69a4d396e9f407"], "10": [624, "1cc01147a16e856b"], "11": [579, "d5a3a67bf065738d"], "12": [87, "afb9cb5ee9e2741c"]}
EXP {"pk": 2, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "80f417a64296887d8c1f58d580947e40aae78af2c21945c14485c0a96f9814c7", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 3, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "98edd7f6e5d72d68a427efcd40cf83e08814313ab7d10d8c858acea3711ebff2", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 4, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "98edd7f6e5d72d68a427efcd40cf83e08814313ab7d10d8c858acea3711ebff2", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 5, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "98edd7f6e5d72d68a427efcd40cf83e08814313ab7d10d8c858acea3711ebff2", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 6, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "98edd7f6e5d72d68a427efcd40cf83e08814313ab7d10d8c858acea3711ebff2", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 7, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "98edd7f6e5d72d68a427efcd40cf83e08814313ab7d10d8c858acea3711ebff2", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 8, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "71570435dbbe5246f59b19986230ad265eece33da6015177bf5b806b75d45e6b", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 9, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "1f2b81f241a8f8cd344a4cd211911f20ab4cdd4ae25d65b416072bff7df2ac02", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 10, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "3c51aae49a29ab84f6b2c21885f796beca6038b483394138c44715743fa95a18", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 11, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "3c51aae49a29ab84f6b2c21885f796beca6038b483394138c44715743fa95a18", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 12, "empresa_id": 4, "mes": 7, "ano": 2026, "hash_sha256": "68d299ff806aceb36e414b75fd18e6ea9043d556b351645072448cf024af010c", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 13, "empresa_id": 4, "mes": 7, "ano": 2026, "hash_sha256": "491f6fff352cc5102cd0d0a5f24f004edc433e792d114563a4e77b529a216c2f", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 14, "empresa_id": 4, "mes": 7, "ano": 2026, "hash_sha256": "491f6fff352cc5102cd0d0a5f24f004edc433e792d114563a4e77b529a216c2f", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 15, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "1897e08736e104ef3d9eb81917bb26c2849f60a21e6f3141bfc2b35acdc08f63", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 16, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "1897e08736e104ef3d9eb81917bb26c2849f60a21e6f3141bfc2b35acdc08f63", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 17, "empresa_id": 3, "mes": 7, "ano": 2026, "hash_sha256": "7d54b0e6602bf4ea8bf9615009cb4e470398f96a9e66b8623dfc03ec4874524e", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 18, "empresa_id": 2, "mes": 8, "ano": 2026, "hash_sha256": "790d8cd7ea0c631c1b76cb462bc9ebadc163787ffd7e854b1d5b9ea0b0d73f01", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 19, "empresa_id": 2, "mes": 8, "ano": 2026, "hash_sha256": "2b3ac743a5782a5122fc89fb917405d9e9529d141f8bb6d81e0c61610b043dd0", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 20, "empresa_id": 2, "mes": 9, "ano": 2026, "hash_sha256": "8a63e3baabc501af7d63aa3845c6a9221d7f18ac091b5f7038650a23aaa59dbc", "invalidada_em": "2026-09-30 20:07:06.058663+00:00", "substituida_por_id": 23}
EXP {"pk": 21, "empresa_id": 3, "mes": 9, "ano": 2026, "hash_sha256": "6e0f1048428293078bc82c21aa8756630446c394ef2c2a5b16cbb1efbb6626d8", "invalidada_em": "2026-09-30 20:07:07.142119+00:00", "substituida_por_id": 24}
EXP {"pk": 22, "empresa_id": 4, "mes": 9, "ano": 2026, "hash_sha256": "cb7f00fe59f84e505fce92aab2769242aec6671b688d7c514311518345c29773", "invalidada_em": "2026-09-30 20:07:07.385295+00:00", "substituida_por_id": 25}
EXP {"pk": 23, "empresa_id": 2, "mes": 9, "ano": 2026, "hash_sha256": "0e3e008b49b1ff73a89556a24e495f0e05f566cad644d8354f224300bbc5a703", "invalidada_em": "2026-10-01 14:57:23.407593+00:00", "substituida_por_id": 26}
EXP {"pk": 24, "empresa_id": 3, "mes": 9, "ano": 2026, "hash_sha256": "5c503b95f9f9cd35541db29a45a6447f026e272b9eedf27b346cda26b92810a6", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 25, "empresa_id": 4, "mes": 9, "ano": 2026, "hash_sha256": "84c78cd0871f5f5251db047a6d12054930f91c05a39ec46d6669fddc1d97a73b", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 26, "empresa_id": 2, "mes": 9, "ano": 2026, "hash_sha256": "0ae5da67364cabfb5dcadde9481ec75d63b702ca95956627adbbb37079664d53", "invalidada_em": "2026-10-01 22:44:56.762182+00:00", "substituida_por_id": 27}
EXP {"pk": 27, "empresa_id": 2, "mes": 9, "ano": 2026, "hash_sha256": "361d0f9685f86d3a51d49b8f5194df469db8e4818c9cae0e9bd9f1461f9af0cc", "invalidada_em": null, "substituida_por_id": null}
AUS5198 {"pk": 5198, "data": "2026-10-24", "minutos": 420, "tipo": "falta"}
REG_PEND {"n": 0, "itens": []}
REG_PEND_FROTA 2
DISP_ABERTAS {"n": 23}
HASH_DP 9 11501 54c6419c61520d6a
HASH_DP 10 21418 9d68332a36ef63eb
ERROS {}
```

REG_PEND n=0, itens=[]; DISP_ABERTAS n=23. [nao verificado]  
HASH_FM: 09=[607,"6e69a4d396e9f407"]; 10=[624,"1cc01147a16e856b"]. [nao verificado]  
HASH_DP: 9=11501/54c6419c61520d6a; 10=21418/9d68332a36ef63eb. [nao verificado]  
AUS5198: 420 minutos. [nao verificado]  
ERROS={}; nenhuma exceção aparece no dado fornecido; ocorrência fora dele: [nao verificado].

## 06/11 20:01 · sessao 1988589b9388 · - · - · origem=Ronald (tmux)

(a) Fecha o nascimento indevido de conversa `hora`: a guarda recusa apenas quando as duas fontes finais de dia são `None`; antes, esse caminho caía em `abrir_conversa`, que grava `localdate()`. Referências: `app/chamados/services/disputa_emissao.py:410`, `app/chamados/services/disputa_emissao.py:411`, `app/chamados/services/disputa_emissao.py:417`, `app/chamados/models.py:2426`.

**Caminho legítimo “nasce conversa hora e recebe dia depois”: nao achei.** A conversa já nasce com data preenchida; o escritor posterior `reapontar_resolvedora` só atribui data quando a pergunta está sem data. Referências: `app/chamados/models.py:2426`, `app/chamados/services/fio.py:835`, `app/chamados/services/fio.py:837`.

Os quatro motivos `hora` são `orfao_14h`, `saida_sem_entrada`, `intervalo_saida` e `intervalo_volta`. Os intervalos sem dia já retornam antes da nova guarda; seu fluxo não era o fallback de conversa. Referências: `app/chamados/catalogo/perguntas.py:48`, `app/chamados/catalogo/perguntas.py:56`, `app/chamados/catalogo/perguntas.py:124`, `app/chamados/catalogo/perguntas.py:132`, `app/chamados/services/disputa_emissao.py:387`.

**Ressalva concreta:** o novo caminho pode deixar disputa sem pergunta e enviar push mandando responder: o chamador ignora a lista retornada e envia push após gerar; a mensagem promete “Toque para responder”. Uma disputa sem perguntas também trava o encerramento do chamado. Referências: `app/chamados/services/disputa_emissao.py:640`, `app/chamados/services/disputa_emissao.py:641`, `app/chamados/services/disputa_emissao.py:652`, `app/chamados/services/disputa_emissao.py:656`, `app/chamados/services/disputa_emissao.py:1575`, `app/chamados/juizes.py:2084`.

**Não desaparece todo rastro além do log:** motivos/contexto já foram persistidos na disputa antes da tentativa de geração. O ramo novo, porém, não persiste um ato de recusa da pergunta: apenas `logger.info` e `continue`. Uma chamada posterior com contexto corrigido tenta gerar novamente, mesmo com motivo já existente. Referências: `app/chamados/services/disputa_emissao.py:557`, `app/chamados/services/disputa_emissao.py:511`, `app/chamados/services/disputa_emissao.py:529`, `app/chamados/services/disputa_emissao.py:415`, `app/chamados/services/disputa_emissao.py:652`.

Chamadores diretos encontrados fora dos testes: [nao verificado]

| Chamador | Expectativa observável |
|---|---|
| `perguntar_entradas_pendentes` | Injeta dia obtido por `_T1`, acumula a lista retornada e salva contexto somente se a lista não estiver vazia. `app/chamados/services/disputa_emissao.py:282`, `app/chamados/services/disputa_emissao.py:292`, `app/chamados/services/disputa_emissao.py:294`, `app/chamados/services/disputa_emissao.py:295`. |
| `criar_ou_atualizar_disputa`, nascimento e atualização | Ignora lista, envia push por nascimento/motivo novo e retorna disputa. `app/chamados/services/disputa_emissao.py:640`, `app/chamados/services/disputa_emissao.py:642`, `app/chamados/services/disputa_emissao.py:652`, `app/chamados/services/disputa_emissao.py:656`, `app/chamados/services/disputa_emissao.py:658`. |
| `fabrica_da_celula.perguntar` | Fornece dia explícito, ignora a lista e retorna quantidade de carimbos desfeitos. `app/chamados/services/fabrica_da_celula.py:139`, `app/chamados/services/fabrica_da_celula.py:141`, `app/chamados/services/fabrica_da_celula.py:142`. |
| `smoke_chamados._bloco_a_motivos` | Fornece data válida para templates datados e exige exatamente uma pergunta por motivo. `app/chamados/management/commands/smoke_chamados.py:96`, `app/chamados/management/commands/smoke_chamados.py:102`, `app/chamados/management/commands/smoke_chamados.py:104`. |

`models.py` apenas reexporta a função; a declaração de hora usa sua própria porta e documenta que não chama o gerador. Referências: `app/chamados/models.py:3350`, `app/chamados/services/disputa_emissao.py:585`.

**Testes, por análise contrafactual do diff; execução no pai [nao verificado]:** [nao verificado]

- `PerguntaDeHoraSemDiaDoTurnoTest.test_MORDE_orfao_sem_dia_nao_nasce_como_conversa_de_hoje` **falha no pai**: data malformada produz texto, o parser de dia falha, não há batida nem chamado para fornecer outra data, e o antigo fallback cria conversa em `localdate()`, contrariando `assertFalse`. Referências: `app/chamados/tests/test_plantio_noturno_mais1.py:203`, `app/chamados/tests/test_plantio_noturno_mais1.py:206`, `app/chamados/tests/test_plantio_noturno_mais1.py:207`, `app/chamados/catalogo/query.py:62`, `app/chamados/services/disputa_emissao.py:364`, `app/chamados/services/disputa_emissao.py:238`, `app/chamados/models.py:2426`.
- Seu contraexemplo `escolha` **passa também no pai**: esse tipo não é atingido pela nova guarda. Referências: `app/chamados/tests/test_plantio_noturno_mais1.py:210`, `app/chamados/catalogo/perguntas.py:82`, `app/chamados/services/disputa_emissao.py:411`.
- O caso congelado de `test_a2` **passaria no pai com o mesmo congelamento transplantado**: usa motivo `escolha`, cujo comportamento não mudou. Sem congelamento, passaria nos dias diferentes de 12/08/2026; em 12/08, os dois asserts são incompatíveis. Congelar cura instabilidade do teste, não produz RED de implementação. Referências: `app/chamados/tests/test_a2_data_evento_batida.py:51`, `app/chamados/tests/test_a2_data_evento_batida.py:57`, `app/chamados/tests/test_a2_data_evento_batida.py:59`, `app/chamados/tests/test_a2_data_evento_batida.py:60`, `app/chamados/catalogo/perguntas.py:84`.

O teste novo de `hora` rejeita somente pergunta com data de hoje; não demonstra ausência de pergunta em qualquer outra data. Referência: `app/chamados/tests/test_plantio_noturno_mais1.py:207`.

**Escritores encontrados de `PerguntaDisputa.data_evento`:** nascimento real em `_nascer`; atribuição posterior e desfazimento em memória em `reapontar_resolvedora`; `_Proxy.data_evento` é insumo transitório de validação. Outro escritor real: **nao achei**. Referências: `app/chamados/models.py:2498`, `app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:846`, `app/chamados/models.py:2444`.

**ESMERIL:** permanece a divergência já declarada fora do escopo: conversa grava dia da pergunta, enquanto `do_fato` e fábrica usam a mesma data como identidade de fato/célula, sem discriminador. A guarda nova elimina a conversa `hora` deste emissor, mas não resolve essa colisão geral. Referências: `app/chamados/models.py:2422`, `app/chamados/models.py:2396`, `app/chamados/models.py:2453`, `app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/disputa_emissao.py:411`.

## 06/11 20:01 · sessao 0ca929e8dcf8 · - · - · origem=Ronald (tmux)

**(c) fecha a omissão para batidas novas; não transforma `criado_em` em data da última materialização.** O filtro agora usa nascimento da linha, enquanto a comparação continua pelo juiz `_T1`. Referências: `app/ponto/management/commands/vigia_plantio_overnight.py:76`, `app/ponto/management/commands/vigia_plantio_overnight.py:87`.

- **Escritor normal:** `Batida.criado_em` é `auto_now_add=True`; as criações passam por `Batida.objects.create`. A porta de anotação não permite alterar esse campo. Referências: `app/ponto/models.py:79`, `app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`, `app/ponto/registro_batida.py:203`, `app/ponto/registro_batida.py:241`.
- **Reescritor dedicado do campo em produção: nao achei.** A restauração de tenant usa `pg_restore`; a sombra incremental copia `ponto_batida` com `SELECT *` e repõe as colunas por `COPY`. Por essas operações, o valor é preservado do registro original, não recalculado pela hora da cópia — inferência do SQL. Referências: `scripts/restore_tenant.sh:8`, `scripts/backup_tenant.sh:10`, `bin/sombra_incremental.sh:34`, `bin/sombra_incremental.sh:57`, `bin/sombra_incremental.sh:62`. Conteúdo real dos dumps e restaurações executadas: **[nao verificado]**.
- **Plantio depois de reabertura:** quando cria outra linha, recebe o nascimento novo; quando reaproveita linha existente, conserva o nascimento antigo. O materializador primeiro busca batida no marco e somente cria se não encontrou; depois pode apenas trocar `pergunta_origem`. Referências: `app/chamados/services/materializacao.py:835`, `app/chamados/services/materializacao.py:847`, `app/chamados/services/materializacao.py:903`, `app/chamados/services/materializacao.py:922`, `app/chamados/services/materializacao.py:924`.
- **Outro reaproveitamento:** `registrar_batida` devolve linha existente pelo mesmo tipo na janela ou pela mesma chave colaborador/instante, inclusive retratada; não altera `criado_em`. Referências: `app/ponto/registro_batida.py:113`, `app/ponto/registro_batida.py:120`.
- **Restauração de retratação:** a porta `desfazer` só grava `aprovada=True` e `retratada_em=None`; conserva `criado_em`. Referência: `app/ponto/services/retratar_humano.py:207`.

**Limite semântico:** “plantada recentemente” coincide com `criado_em` para criação nova; não coincide com reaproveitamento/revinculação recente de linha antiga. Assim, uma linha antiga de `origem='disputa_s84_retro'` vinculada hoje a outra pergunta continua fora da janela por criação. Referências: `app/ponto/management/commands/vigia_plantio_overnight.py:67`, `app/ponto/management/commands/vigia_plantio_overnight.py:73`, `app/ponto/management/commands/vigia_plantio_overnight.py:76`, `app/chamados/services/materializacao.py:924`. Isso não demonstra nova regressão: se seu `timestamp` também era antigo, o filtro do pai já a omitia — comparação com o delta fornecido; execução no pai **[nao verificado]**.

**Teste do vigia: morde por análise contrafactual.** Cria a linha em 01/12, com instante em 23/10; na janela de 30 dias, o filtro anterior por `timestamp` exclui a única candidata e o resultado seria `[]`, contrariando `[b.pk]`. O teste cobre criação nova, não reaproveitamento. Referências: `app/chamados/tests/test_plantio_noturno_mais1.py:30`, `app/chamados/tests/test_plantio_noturno_mais1.py:32`, `app/chamados/tests/test_plantio_noturno_mais1.py:192`, `app/chamados/tests/test_plantio_noturno_mais1.py:193`, `app/chamados/tests/test_plantio_noturno_mais1.py:195`. RED executado: **[nao verificado]**.

**Demais leitores de `Batida.criado_em` encontrados:** o pareador usa chegada para distinguir eco de flush; o contador de sequência seleciona batidas criadas desde um marco; o cache de veredito usa o máximo de criação e trata retratação separadamente. Esses usos conservam o significado de criação/chegada. Referências: `app/ponto/turnos.py:355`, `app/ponto/turnos.py:366`, `app/ponto/turnos.py:368`, `app/ponto/turnos.py:2384`, `app/chamados/veredito_lavrado.py:137`, `app/chamados/veredito_lavrado.py:140`.

## 06/11 20:01 · sessao 5275b260cc32 · - · - · origem=Ronald (tmux)

1. **(b) fecha o `None.isoformat()` local do PWA.** A nova guarda só chama a disputa se `_dia_sse` vier preenchido; não altera o registro nem o retorno ao usuário (`app/ponto/services/triagem_batida.py:726`, `app/ponto/services/triagem_batida.py:730`, `app/ponto/services/triagem_batida.py:757`).

   O caminho concreto de `None` existe: `registrar_batida` exclui retratadas no primeiro dedup, mas depois devolve a mesma chave `(colaborador,timestamp)` mesmo retratada, sem gravá-la novamente; `_T1` não inclui retratadas no mapa (`app/ponto/registro_batida.py:107`, `app/ponto/registro_batida.py:120`, `app/ponto/turnos.py:2905`, `app/ponto/turnos.py:2925`).

   **Cura equivalente nos gêmeos API: nao achei.** Ambos continuam chamando `.isoformat()` diretamente no retorno de `_dtb_sse`; é vizinho preexistente, não introduzido pelo delta (`app/api/views.py:780`, `app/api/views.py:782`, `app/api/views_core.py:780`, `app/api/views_core.py:782`).

2. **Emissor posterior que recupere especificamente essa tentativa SSE/reanime automaticamente essa batida retratada: nao achei.** A devolução da mesma chave não executa `save`; os gatilhos de julgamento/reconciliação ligados a `post_save` não são disparados por esse retorno (`app/ponto/registro_batida.py:120`, `app/ponto/signals.py:131`, `app/ponto/signals.py:206`).

   Há emissão posterior **da falta do dia**, condicional: `processar_cartorio` percorre células passadas e chama `julgar_colab`; para células julgadas, este chama `perguntar_pela_celula`, que pode criar disputa e pergunta datadas pela célula (`app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/management/commands/processar_cartorio.py:65`, `app/ponto/services/cartorio.py:995`, `app/chamados/services/fabrica_da_celula.py:139`, `app/chamados/services/fabrica_da_celula.py:174`).

   Isso **não garante pergunta dessa tentativa**: exige célula, colaborador ativo, dia permitido, ata/veredito e marcos cobrados; pode ir à pauta em vez de pergunta. A batida retratada continua fora da apuração (`app/chamados/services/fabrica_da_celula.py:91`, `app/chamados/services/fabrica_da_celula.py:99`, `app/chamados/services/fabrica_da_celula.py:102`, `app/chamados/services/fabrica_da_celula.py:171`, `app/ponto/turnos.py:2905`).

   `disparar_perguntas_competencia` tem caminho escritor para **chamado vivo sem pergunta**; o caminho das células é vigia. Se a guarda não criou disputa/chamado dessa tentativa, esse caminho não a recupera por si (`app/chamados/management/commands/disparar_perguntas_competencia.py:141`, `app/chamados/management/commands/disparar_perguntas_competencia.py:238`, `app/chamados/management/commands/disparar_perguntas_competencia.py:268`).

   A existência/execução efetiva desses crons em produção é **[nao verificado]**. A restituição explícita por humano existe e limpa `retratada_em`, devolvendo a batida à apuração (`app/ponto/services/retratar_humano.py:190`, `app/ponto/services/retratar_humano.py:207`).

3. **Inventário de `contexto['saida_sem_entrada']`:**

   - **Escritor direto do delta:** PWA grava `data` pelo `_T1` da batida e `hora_tentativa` pelo relógio da tentativa (`app/ponto/services/triagem_batida.py:729`, `app/ponto/services/triagem_batida.py:738`).
   - **APIs gêmeas:** montam inicialmente `data` civil, mas a sobrescrevem por `_T1` antes de chamar o persistidor; exceção no `_T1` interrompe esse trajeto (`app/api/views.py:655`, `app/api/views.py:780`, `app/api/views_core.py:652`, `app/api/views_core.py:780`).
   - **Fábrica genérica do dia:** para motivo `saida_sem_entrada`, grava `{motivo: {'data': dia.isoformat()}}`, com dia da célula (`app/chamados/services/fabrica_da_celula.py:139`, `app/chamados/services/fabrica_da_celula.py:176`).
   - **Persistidor:** nascimento copia `contexto_nascimento`; agregação faz merge por motivo, preservando campos anteriores e sobrescrevendo os recebidos (`app/chamados/services/disputa_emissao.py:518`, `app/chamados/services/disputa_emissao.py:525`, `app/chamados/services/disputa_emissao.py:557`, `app/chamados/services/disputa_emissao.py:632`).
   - **Leitor de texto:** usa `contexto[motivo].data`; o texto canônico SSE pergunta pela **entrada** daquele dia (`app/chamados/catalogo/query.py:58`, `app/chamados/catalogo/query.py:64`, `app/chamados/catalogo/perguntas.py:56`).
   - **Leitor de nascimento:** transforma esse `data` em `data_evento` (`app/chamados/services/disputa_emissao.py:364`, `app/chamados/services/disputa_emissao.py:367`, `app/chamados/services/disputa_emissao.py:410`, `app/chamados/services/disputa_emissao.py:423`).
   - **Leitor de destino:** usa a data da pergunta prioritariamente; contexto é fallback para casar o dia do chamado (`app/chamados/services/disputa_emissao.py:191`, `app/chamados/services/disputa_emissao.py:194`, `app/chamados/services/disputa_emissao.py:231`).
   - **Materializador:** prioriza `p.data_evento`, depois contexto por motivo, e trata o valor como dia do marco; SSE materializa tipo `E` (`app/chamados/services/materializacao.py:341`, `app/chamados/services/materializacao.py:343`, `app/chamados/services/materializacao.py:347`).

   **Leitor específico de SSE que ainda trate `data` como dia civil da saída: nao achei.** O par PWA→leitores está alinhado em dia do turno; a leitura de `hora_tentativa` para recuar entrada saiu do derivador, conforme a delegação/comentário atual (`app/ponto/services/triagem_batida.py:739`, `app/chamados/services/materializacao.py:419`, `app/chamados/juizes.py:302`).

   Ressalva preexistente: APIs adicionam motivo `orfao_14h` enquanto escrevem a subchave `saida_sem_entrada`; o leitor genérico por motivo não acessa diretamente essa chave para `orfao_14h`, embora haja fallback pela `ultima_batida.id` (`app/api/views_core.py:651`, `app/api/views_core.py:652`, `app/api/views_core.py:773`, `app/chamados/services/disputa_emissao.py:364`, `app/chamados/services/disputa_emissao.py:379`).

## 06/11 19:55 · sessao 3d97092632e9 · BUG-PLANTIO-NOTURNO-MAIS-1 · verificador · origem=Code

**O aval não está integralmente atendido:** a conversa ainda pode ser confundida com pergunta de turno no dedup e na fábrica da célula; há datas de contexto que chegam à porta sem passar por `_T1`; e o vigia pode omitir plantio retroativo recente. Referências: `app/chamados/models.py:2396`, `app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/disputa_emissao.py:1101`, `app/ponto/management/commands/vigia_plantio_overnight.py:75`.

**1. Quem escreve e quem lê**

Os escritores efetivos encontrados são `_nascer` no nascimento e `reapontar_resolvedora` na costura. A atribuição ao `_Proxy` é transitória, para validação; não grava uma pergunta. Referências: `app/chamados/models.py:2444`, `app/chamados/models.py:2498`, `app/chamados/services/fio.py:835`.

| Emissor que fornece o valor à porta | Origem do valor |
|---|---|
| `gerar_perguntas_disputa` | Dia fornecido pela grade após localizar batida e consultar `_T1`; alternativamente, `contexto[motivo].data`, batida de `ultima_batida.id` via `_T1`, ou dia do chamado. O resultado é embrulhado por `dia_do_turno_da_celula`. `app/chamados/services/disputa_emissao.py:68`, `app/chamados/services/disputa_emissao.py:76`, `app/chamados/services/disputa_emissao.py:364`, `app/chamados/services/disputa_emissao.py:383`, `app/chamados/services/disputa_emissao.py:400`, `app/chamados/services/disputa_emissao.py:424`. |
| `perguntar_entradas_pendentes` | Localiza cada entrada por PK, consulta `_T1` e injeta seu dia no contexto do gerador. `app/chamados/services/disputa_emissao.py:282`, `app/chamados/services/disputa_emissao.py:292`. |
| Agrupador de chamados órfãos | Com batida-origem: `resolver_data_evento` → `_T1`. Sem origem: parser do contexto do chamado. Para intervalo: lê `marcos_faltantes[].data`. `app/chamados/services/disputa_emissao.py:1017`, `app/chamados/services/disputa_emissao.py:1074`, `app/chamados/services/disputa_emissao.py:1101`, `app/chamados/services/disputa_emissao.py:1114`. |
| `_complementar_marcos_faltantes` | Dia da célula devolvido pelo leitor dos marcos faltantes. `app/chamados/services/declaracao_texto.py:123`, `app/chamados/services/disputa_emissao.py:842`. |
| `declarar_hora_colab` e `aplicar_declaracao` | Dia do marco faltante da célula. `app/chamados/services/declaracao_texto.py:299`, `app/chamados/services/declaracao_texto.py:327`, `app/chamados/services/declaracao_texto.py:617`. |
| `abrir_conversa` | Dia civil em que foi feita: `localdate()`. Chamadores: reabertura administrativa, âncora ausente e fallback do gerador. `app/chamados/models.py:2426`, `app/chamados/services/acoes_disputa.py:260`, `app/chamados/services/disputa_emissao.py:412`, `app/chamados/services/disputa_emissao.py:1230`. |
| Reconciliador → `reapontar_resolvedora` | Dia do chamado/contexto, embrulhado como dia de célula. A costura só preenche pergunta sem data. `app/chamados/reconciliador.py:941`, `app/chamados/reconciliador.py:1624`, `app/chamados/reconciliador.py:1641`, `app/chamados/services/fio.py:835`. |
| Declaração → `reapontar_resolvedora` | Passa diretamente o dia do marco faltante, sem o construtor tipado. `app/chamados/services/declaracao_texto.py:320`. |

Detector, API e PWA escrevem **contextos que alimentam esses emissores**: detector via `resolver_data_evento`; kernels e triagem via batida gravada → `_T1`. Referências: `app/chamados/detector_anomalias.py:47`, `app/api/views.py:780`, `app/api/views_core.py:782`, `app/ponto/services/triagem_batida.py:734`.

Inventário dos leitores de produção encontrados, agrupado pelo significado:

| Leitura | Consumidores e significado |
|---|---|
| Instante do plantio | `_ts_tipo_do_plantio` lê como **dia do turno** e combina com a hora; `_ajustar_saida_overnight` escolhe D ou D+1 pelos marcos. `app/chamados/services/materializacao.py:344`, `app/chamados/services/materializacao.py:414`, `app/chamados/juizes.py:312`, `app/chamados/juizes.py:319`. |
| Slot, prova do plantio e lote | `_slot_esperado` delega ao plantio; `_plantio_na_hora_do_slot` usa D/D+1; `classe_do_lote` verifica conflito no instante do slot. `app/chamados/juizes.py:346`, `app/chamados/juizes.py:266`, `app/chamados/juizes.py:271`, `app/chamados/services/validacao.py:825`. |
| Turno, célula e julgamento | `_turno_tem_saida`, `_celulas_da_pergunta`, `_pergunta_materializada_de_fato`, `_pergunta_viva_calc`, `_marco_esperado_da_grade` e `resolvedora_valida` usam a data como **chave do turno/fato**. `app/chamados/juizes.py:363`, `app/chamados/juizes.py:623`, `app/chamados/juizes.py:666`, `app/chamados/juizes.py:1202`, `app/chamados/juizes.py:1524`, `app/chamados/juizes.py:1964`. |
| Dedup e emissão | `do_fato`, `_nascer`, gerador, `_ja_perguntou`, declaração, migração de resolvedoras, fábrica da célula e diagnóstico de pergunta no app usam **identidade por dia/motivo**. `app/chamados/models.py:2396`, `app/chamados/models.py:2452`, `app/chamados/services/disputa_emissao.py:389`, `app/chamados/services/disputa_emissao.py:1272`, `app/chamados/services/declaracao_texto.py:227`, `app/chamados/services/declaracao_texto.py:539`, `app/chamados/services/fio.py:941`, `app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/pergunta_no_app.py:109`. |
| Dia do chamado e competência | Derivação do dia pela disputa, tranca, vias de resolução, competência trancada, passivo validado e expiração usam **dia do fato/turno para competência**. `app/chamados/juizes.py:76`, `app/chamados/juizes.py:209`, `app/chamados/juizes.py:932`, `app/chamados/juizes.py:1476`, `app/chamados/juizes.py:1480`, `app/chamados/models.py:351`, `app/chamados/models.py:377`, `app/chamados/services/competencia_trancada.py:107`, `app/chamados/services/passivo_validado.py:113`, `app/chamados/veredito_lavrado.py:229`. |
| Exceção civil explícita | O ramo de adesão de `via_pelo_fato` monta **o dia civil inteiro** a partir da chave, para perguntar se houve qualquer batida naquele dia. É diferente de consultar batidas pertencentes ao turno. `app/chamados/juizes.py:1450`, `app/chamados/juizes.py:1461`. |
| Ausência e validação | Declaração/materialização de ausência, perguntas absorvidas, aceitação de folga e recálculo usam **dia do fato**. `app/chamados/services/materializacao.py:473`, `app/chamados/services/materializacao.py:517`, `app/chamados/services/materializacao.py:607`, `app/chamados/services/validacao.py:250`, `app/chamados/services/validacao.py:407`. |
| Reconciliação | Julgamento de órfãs, encerramento do dia, semeadura de eventos e reatamento usam **dia do turno/célula**. `app/chamados/reconciliador.py:370`, `app/chamados/reconciliador.py:1040`, `app/chamados/reconciliador.py:1080`, `app/chamados/reconciliador.py:1297`, `app/chamados/reconciliador.py:1543`. |
| Costura e eventos | Comparação com o dia do chamado, guarda de costura e localização da célula pelo par colaborador/data. `app/chamados/services/fio.py:87`, `app/chamados/services/fio.py:773`, `app/chamados/models.py:2690`, `app/chamados/models.py:2706`. |
| Folha, calendário e fechamento do fio | Agrupam a pergunta no **dia da célula**. `app/folha/previa.py:65`, `app/colaboradores/services/calendario.py:473`, `app/ponto/services/dia_do_colab.py:65`, `app/ponto/services/veredito_celula.py:176`, `app/chamados/services/relavra_via.py:152`, `app/chamados/services/limbo_folgas.py:202`. |
| Canal, cobrança e vigias | Associam pergunta ao dia do chamado/célula ou comparam com o turno da batida. `app/chamados/canal.py:72`, `app/chamados/services/cobranca.py:83`, `app/chamados/services/aviso_pendencia.py:20`, `app/chamados/services/vigia_fio.py:816`, `app/ponto/management/commands/vigia_plantio_overnight.py:89`. |
| API e apresentação | Serializam, ordenam ou imprimem a data, sem converter instante. `app/api/views.py:1174`, `app/api/views.py:2654`, `app/api/views_mensageria.py:568`, `app/chamados/views_cobrar.py:176`, `app/chamados/services/explicador.py:70`, `app/chamados/services/explicador.py:126`, `app/chamados/services/historico.py:108`, `app/templates/chamados/partials/_card_atendimento.html:90`, `app/templates/chamados/partials/_lote_preview.html:24`, `app/templates/chamados/meu_atendimento.html:19`. |
| Comandos operacionais | Dedup por fato, seleção por competência, reabertura, backfill, cruzamento de furos e auditorias leem **dia do fato/turno**; alguns apenas o imprimem. `app/chamados/management/commands/dedup_perguntas_fato.py:54`, `app/chamados/management/commands/disparar_perguntas_competencia.py:213`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:68`, `app/chamados/management/commands/backfill_veredito_lavrado.py:111`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:74`, `app/ponto/management/commands/mapa_divergencia.py:272`, `app/chamados/management/commands/auditar_invariantes_chamados.py:73`, `app/chamados/management/commands/auditar_invariantes_chamados.py:892`. |

**Divergências encontradas:** conversa versus identidade de turno, detalhada no item 2; contexto civil versus dia do turno, detalhado no item 7; e apresentação que ainda promete “é aqui que a batida nasce” ao mostrar `data_evento`, embora a saída noturna nasça em D+1. Referências: `app/chamados/models.py:2426`, `app/chamados/models.py:2453`, `app/chamados/data_evento.py:27`, `app/templates/chamados/partials/modal_fio.html:166`, `app/chamados/juizes.py:319`.

**2. Conversa: há leitores que a tratam como turno**

**Sim.** O parâmetro `conversa` só controla o nascimento; não é persistido como discriminador. Referências: `app/chamados/models.py:2450`, `app/chamados/models.py:2498`.

Caminho concreto de colisão:

- A reabertura aceita qualquer motivo válido e cria conversa `escolha` com esse motivo e `localdate()`. Referências: `app/chamados/services/acoes_disputa.py:245`, `app/chamados/services/acoes_disputa.py:247`, `app/chamados/services/acoes_disputa.py:260`, `app/chamados/models.py:2426`.
- Uma abertura datada posterior procura somente colaborador/motivo/data. Pode encontrar essa conversa, atribuir-lhe a célula e devolvê-la, sem criar a pergunta `hora`. Referências: `app/chamados/models.py:2396`, `app/chamados/models.py:2453`, `app/chamados/models.py:2457`, `app/chamados/models.py:2497`.
- A fábrica da célula também pode considerar o marco “já perguntado” por causa dessa conversa. Referências: `app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/fabrica_da_celula.py:123`, `app/chamados/services/fabrica_da_celula.py:126`.
- O C6 passa a data a `classificar_falta`; o comando de dedup e a migração de resolvedoras também não distinguem conversa. Referências: `app/chamados/juizes.py:1202`, `app/chamados/management/commands/dedup_perguntas_fato.py:54`, `app/chamados/services/fio.py:941`.

As conversas normais `escolha` não entram no slot; `data_hora` usa a data declarada na resposta; o vigia novo seleciona apenas `hora`. Referências: `app/chamados/juizes.py:332`, `app/chamados/juizes.py:334`, `app/ponto/management/commands/vigia_plantio_overnight.py:73`.

Entretanto, há caminho para conversa **`hora`**: data malformada ainda produz texto, falha no parser e, sem outra data derivável, chega a `abrir_conversa` mantendo o tipo do catálogo. Depois, plantio e vigia leem seu `localdate()` como turno. Referências: `app/chamados/catalogo/query.py:62`, `app/chamados/services/disputa_emissao.py:365`, `app/chamados/services/disputa_emissao.py:410`, `app/chamados/services/disputa_emissao.py:413`, `app/chamados/services/materializacao.py:344`, `app/ponto/management/commands/vigia_plantio_overnight.py:89`. Ocorrência em produção: **[nao verificado]**.

**3. Escritor fora das portas invisível ao selo**

**Escritor real adicional: nao achei.** O censo AST executado em memória encontrou somente proxy/create de `_nascer` e atribuição/desfazimento de `reapontar_resolvedora`. Referências: `app/chamados/models.py:2444`, `app/chamados/models.py:2498`, `app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:846`.

O selo vê `qs.update(data_evento=d)` mesmo com queryset em variável. Não acompanha, porém, `campos={'data_evento': d}; qs.update(**campos)`, nome variável em `setattr`, SQL ou escrita implícita por formulário. Sua análise procura constantes no próprio argumento e não acompanha fluxo de valores. Referências: `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:54`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:86`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:92`.

Existe uma abertura genérica: `descarimbar_em_lote(**outros)` encaminha campos ao `update`; não achei chamador atual passando `data_evento`. Referências: `app/chamados/models.py:2714`, `app/chamados/models.py:2721`, `app/chamados/models.py:1508`, `app/chamados/services/disputa_emissao.py:1449`.

**4. Consumidores dos contextos do kernel**

| Campo | Quem lê |
|---|---|
| `entrada_sem_saida.entrada_id` | Só achei `_data_furo_da_grade`: busca por PK e consulta `_T1`. `app/chamados/services/disputa_emissao.py:68`, `app/chamados/services/disputa_emissao.py:76`. |
| `ultima_batida.id` | `_data_furo_da_grade`, fallback do gerador via `_T1` e resolvedor de foto como identidade da batida. `app/chamados/services/disputa_emissao.py:72`, `app/chamados/services/disputa_emissao.py:379`, `app/api/foto_recebida.py:67`. |
| `saida_sem_entrada.data` | Texto, data da pergunta, seleção do chamado e fallback do materializador. `app/chamados/catalogo/query.py:58`, `app/chamados/services/disputa_emissao.py:364`, `app/chamados/services/disputa_emissao.py:196`, `app/chamados/services/materializacao.py:343`. |

**Leitor que ainda interprete especificamente `saida_sem_entrada.data` como dia civil da saída: nao achei.** O texto canônico pergunta pela entrada naquele dia. Referência: `app/chamados/catalogo/perguntas.py:56`.

Há duas ressalvas: o kernel acrescenta motivo `orfao_14h`, mas grava contexto sob `saida_sem_entrada`, portanto a leitura genérica pelo motivo não pega diretamente essa chave; e o vizinho `_dia_do_contexto_fraco` ainda transforma `ultima_batida.timestamp` em `.date()` sem conversão de fuso ou `_T1`. Referências: `app/api/views.py:654`, `app/api/views.py:655`, `app/api/views_core.py:651`, `app/api/views_core.py:652`, `app/chamados/juizes.py:155`, `app/chamados/juizes.py:162`.

**5. Triagem: disputa depois de `registrar_batida`**

**Sim, há caminhos em que antes nascia e agora não:** `dupla`, retorno `batida is None`/`ignorada_intervalo` e exceções entre o predicado e o nascimento. O predicado está antes desses caminhos; a disputa agora vem depois deles. Referências: `app/ponto/services/triagem_batida.py:654`, `app/ponto/services/triagem_batida.py:670`, `app/ponto/services/triagem_batida.py:701`, `app/ponto/services/triagem_batida.py:714`, `app/ponto/services/triagem_batida.py:729`; posição anterior: diff fornecido **[nao verificado por comparação Git]**.

**Outro sítio que exija explicitamente a disputa prévia para dupla/ignorada: nao achei.** Contudo, os sinais da gravação já executam cartório e reconciliador antes de a nova disputa nascer; essa passagem não vê o fio futuro. Referências: `app/ponto/signals.py:131`, `app/ponto/signals.py:206`, `app/chamados/reconciliador.py:1418`, `app/ponto/services/triagem_batida.py:701`, `app/ponto/services/triagem_batida.py:729`.

Há ainda um caminho de exceção novo: `registrar_batida` pode devolver uma mesma chave retratada; o wrapper de `_T1` pode retornar `None`, mas a triagem chama `.isoformat()` sem guarda. Para ultrapassar o guard de dupla de saída, a chave retratada pode ser de outro tipo. Referências: `app/ponto/registro_batida.py:116`, `app/ponto/registro_batida.py:120`, `app/ponto/turnos.py:2925`, `app/ponto/services/triagem_batida.py:665`, `app/ponto/services/triagem_batida.py:734`. Ocorrência em produção: **[nao verificado]**.

**6. Classes que já reprovaram obra**

| Classe | Conferência |
|---|---|
| Segundo juiz | **Juiz geométrico novo: nao achei.** O wrapper consulta o juiz existente; a autoridade registrada continua `turnos_do_colab`. `app/ponto/turnos.py:2925`, `app/core/juizes.py:196`. |
| Segundo escritor | **Escritor real extra: nao achei.** Há as lacunas de detecção do item 3. `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:86`. |
| Relógio fora da borda | `abrir_conversa` lê o relógio no modelo, e o contador lê dentro do helper, sem receber instante do chamador. A primeira leitura foi deslocada pelo diff; a segunda é nova. `app/chamados/models.py:2426`, `app/ponto/management/commands/vigia_plantio_overnight.py:75`. |
| Derivação paralela | **Ainda existe entrada por parsers de contexto**, sem `_T1`; o construtor de célula apenas devolve a data recebida. `app/chamados/data_evento.py:27`, `app/chamados/services/disputa_emissao.py:1101`, `app/ponto/turnos.py:2933`. |
| Selo/RED sem mordida | Há asserts que atingem o defeito principal: escritor retorna D, saída planta em D+1, detector grava D e sem marcos não recua. `app/chamados/tests/test_plantio_noturno_mais1.py:78`, `app/chamados/tests/test_plantio_noturno_mais1.py:83`, `app/chamados/tests/test_plantio_noturno_mais1.py:90`, `app/chamados/tests/test_plantio_noturno_mais1.py:112`. Não cobrem a colisão conversa/datada. `app/chamados/tests/test_b2_data_conversa.py:14`, `app/chamados/tests/test_b2_data_conversa.py:26`. Execução Django, RED contra o pai e mutação real: **[nao verificado]**. |
| Relógio dos testes | O teste novo exige simultaneamente data diferente de 12/08/2026 e igual a `localdate()`, sem congelamento: no próprio dia 12/08/2026 as duas condições são incompatíveis. O arquivo já está na lista de exceções. `app/chamados/tests/test_a2_data_evento_batida.py:50`, `app/chamados/tests/test_a2_data_evento_batida.py:57`, `app/chamados/tests/test_a2_data_evento_batida.py:58`, `app/core/tests/testes_sem_relogio.txt:26`. |
| Ordem de trava | **Inversão comprovada: nao achei.** A costura mantém atomic até o save; a emissão trava disputa. A mudança verificável é gravação/sinais antes da nova disputa, conforme item 5. `app/chamados/services/fio.py:840`, `app/chamados/services/fio.py:850`, `app/chamados/services/disputa_emissao.py:494`. Deadlock: **[nao verificado]**. |
| Pouso antes do pré-requisito | Suite, pouso e saneamento do passivo: **[nao verificado]**. A costura não converte datas já preenchidas, pois só data perguntas sem dia. `app/chamados/services/fio.py:835`. |
| Campo com dois significados | **Permanece alcançável:** conversa grava dia da pergunta; leitores de identidade/célula o recebem como dia do turno. `app/chamados/models.py:2426`, `app/chamados/models.py:2396`, `app/chamados/services/fabrica_da_celula.py:118`. |

A porta possui anotação `DiaDoTurno`, mas sua guarda executável verifica apenas `None`; `NewType` não estabelece uma validação semântica nessa porta. Os testes novos verificam ausência/`None`, não proveniência do dia. Referências: `app/core/dia_do_turno.py:13`, `app/chamados/models.py:2402`, `app/chamados/models.py:2413`, `app/chamados/tests/test_b2_data_conversa.py:26`.

**7. Fallback disfarçado restante**

**Sim, nestes caminhos:**

- Sem batida-origem, o agrupador usa `data_evento_do_contexto`, que aceita `data_batida`, prefixo de `entrada_timestamp`, texto `pendentes` e `horario_previsto`. Não consulta `_T1` nem converte fuso antes de extrair esses dias. Referências: `app/chamados/services/disputa_emissao.py:1101`, `app/chamados/services/disputa_emissao.py:1114`, `app/chamados/data_evento.py:27`, `app/chamados/data_evento.py:31`, `app/chamados/data_evento.py:39`.
- O derivador aceita o dia do chamado; esse juiz tem degraus de dia civil da batida e timestamp cru do contexto. Referências: `app/chamados/services/disputa_emissao.py:231`, `app/chamados/services/disputa_emissao.py:247`, `app/chamados/juizes.py:53`, `app/chamados/juizes.py:162`.
- O gerador aceita `contexto[motivo].data` antes de consultar a batida, e depois o declara dia de célula sem conferir a origem. Referências: `app/chamados/services/disputa_emissao.py:364`, `app/chamados/services/disputa_emissao.py:4

[truncado em 20000 de 22829 caracteres]

## 06/11 19:55 · sessao ed5c1cb9db74 · ESMERIL-SEM-INSTANTE · verificador · origem=Code

**Parecer: a troca corrige o referencial UTC → dia civil local no filtro, mas não resolve a atribuição de batidas ao dia do turno.** Há uma restrição nova para timestamps naive e um residual real nas células alternadas; nenhum deles foi demonstrado ocorrendo em produção pelo dado fornecido. (`app/ponto/services/fechamento.py:204`, `:254`, `:261`; `app/ponto/turnos.py:46`)

A correspondência da cópia com o hash informado e o diff integral contra `5a3f8d0e` permanece **[nao verificado]**: não há metadados Git disponíveis. A execução completa da suíte Django também permanece **[nao verificado]**: Django não está instalado neste ambiente.

1. **Correção e contraexemplos**

   - **Caminho identificado de produção:** fechamento, espelho e PDF entregam listas carregadas por `batidas_apuraveis`; essa função consulta batidas não retratadas e ordena por timestamp. O campo não declara `null=True`, e a configuração usa `USE_TZ=True`. Isso sustenta o contrato esperado de instante aware; os objetos efetivamente devolvidos em produção continuam **[nao verificado]**. (`app/ponto/services/fechamento.py:633`; `app/ponto/services/espelho.py:1022`; `app/relatorios/pdf_espelho.py:272`; `app/ponto/turnos.py:2864`, `:2871`; `app/ponto/models.py:23`; `app/config/settings/base.py:95`)
   - **`timestamp=None`:** `data_local` devolve `None`, mas a comparação seguinte com `date` levanta `TypeError`. A porta inteira não se tornou None-safe; o código antigo também quebrava, ao chamar `.date()` sobre `None`. Não encontrei origem comprovada desse valor chegando pelos três carregadores. (`app/ponto/turnos.py:46`; `app/ponto/services/fechamento.py:204`, `:254`; filtro antigo no diff fornecido)
   - **Naive:** existe regressão da interface: antes `.date()` aceitava naive; agora `localtime` o rejeita. A conversão acontece inclusive antes de saber se alguma fatia rodará: uma batida naive com `escalas=[]` passa a falhar. A normalização defensiva do motor acontece depois dessa porta, portanto não protege esse caso. Origem naive de produção chegando aqui: **[nao verificado]**. (`app/ponto/services/fechamento.py:204`, `:205`, `:242`; `app/ponto/turnos.py:44`; `app/ponto/motor_calculo_v2.py:77`; `app/ponto/turnos.py:1528`)
   - **Iterador consumível:** o novo código consome `batidas` uma vez e reutiliza a lista materializada em todas as fatias. O antigo percorria o iterador novamente por fatia, deixando as posteriores sem batidas. Portanto, nesse ponto o diff melhora o comportamento. Os chamadores encontrados já usam listas. (`app/ponto/services/fechamento.py:204`, `:253`, `:633`; `app/ponto/services/espelho.py:1022`; `app/relatorios/pdf_espelho.py:272`)
   - **Ordem e identidade:** as compreensões preservam a ordem relativa e devolvem os mesmos objetos; não há ordenação ou deduplicação nova. Isso importa porque `remover_duplicatas` percorre a ordem recebida. (`app/ponto/services/fechamento.py:204`, `:253`, `:260`, `:262`; `app/ponto/motor_calculo_v2.py:905`)
   - **Leitores do comportamento antigo:** os três consumidores compartilham a porta, portanto recebem a alteração. Não encontrei contrato nesses chamadores exigindo dia UTC; suas buscas são construídas por limites locais. Dependência externa ou dinâmica do comportamento antigo: **[nao verificado]**. (`app/ponto/services/fechamento.py:622`, `:754`; `app/ponto/services/espelho.py:1019`, `:1164`; `app/relatorios/pdf_espelho.py:264`, `:419`)

2. **Quem escreve, quem lê e qual “dia”**

   | Valor | Escritor/origem e leitores |
   |---|---|
   | `b.timestamp` | Persistido por `registrar_batida`; também corrigido diretamente pela regularização externa. O carregador ORM o entrega; a porta lê e passa o objeto ao motor. **Instante**, não data de jornada. (`app/ponto/registro_batida.py:153`, `:158`; `app/chamados/services/regularizacao_ext.py:154`; `app/ponto/turnos.py:2871`; `app/ponto/services/fechamento.py:204`, `:287`) |
   | `fatia_ini`, `fatia_fim` | Escritos pelo encontro entre competência, vigência e piso operacional; depois podem ser reduzidos ao primeiro/último `_meus`. Lidos pelo filtro, cobertura, fábrica do motor, carimbo e cálculo. Limites **inclusivos** de datas nominais. (`app/ponto/services/fechamento.py:206`, `:207`, `:238`, `:261`, `:267`, `:279`, `:283`, `:287`) |
   | `fatia_ini_busca`, `fatia_fim_busca` | Escritos exclusivamente como limites finais −1/+1 dia; lidos no primeiro filtro, com ambas as pontas inclusivas. São envelope civil de contexto, não julgamento da jornada. (`app/ponto/services/fechamento.py:244`, `:245`, `:254`) |
   | `_meus` | Derivado das datas de células cujo vínculo é resolvido por `vinculo_do_dia`; complementado por dias sem dono cobertos exclusivamente pelo vínculo. Lido para pular/reduzir a fatia, filtrar batidas e declarar cobertura. (`app/ponto/services/fechamento.py:189`, `:209`, `:229`, `:233`, `:235`, `:238`, `:259`, `:267`) |
   | `_batidas_do_dia` | Novo cache de pares `(objeto, dia civil local do instante)`, escrito antes do laço e lido por cada fatia. (`app/ponto/services/fechamento.py:204`, `:253`) |
   | `batidas_fatia` | Escrita pelo envelope, restringida por `_meus`, desempacotada para objetos e entregue exclusivamente ao cálculo da fatia. (`app/ponto/services/fechamento.py:253`, `:260`, `:262`, `:287`) |
   | `data_local` | Implementada em `turnos.py`; devolve dia civil local de aware ou `None`. Não calcula `data_turno`. A nova porta reutiliza essa implementação. (`app/ponto/turnos.py:43`, `:46`; `app/ponto/services/fechamento.py:203`) |

   **Não são todos o mesmo significado de dia.** A célula possui uma data nominal D, mas seu marco de saída pode estar em D+1. O motor julga períodos por `data_jornada`, obtida do juiz de turno, com fallback local e possível herança da jornada anterior. Portanto, o comentário novo alinha o calendário/fuso, mas não demonstra equivalência entre **dia civil da batida** e **dia nominal da célula/jornada**. (`app/escala/models.py:1685`; `app/ponto/management/commands/gerar_celulas.py:67`; `app/ponto/services/fechamento.py:247`; `app/ponto/motor_calculo_v2.py:2403`, `:2408`, `:2429`)

   As competências e vigências também são datas cadastrais inclusivas, não dias UTC; o corte da competência é parametrizado, apesar do “21..20” usado na explicação dos testes. (`app/escala/models.py:875`, `:876`; `app/escala/alimentacao.py:214`; `app/ponto/janelas.py:15`, `:21`, `:57`; `app/ponto/tests/test_composicao_de_fatias_e_total.py:252`)

   Há ainda uma distinção de fuso: `data_local` chama `timezone.localtime` sem fuso explícito; o motor fixa `TZ_LOCAL` a partir de `settings.TIME_ZONE`. Igualdade entre fuso ativo e fuso configurado em toda execução de produção: **[nao verificado]**. (`app/ponto/turnos.py:46`; `app/ponto/motor_calculo_v2.py:28`, `:79`)

   Outros leitores diretos de `data_local`: signal, detector de par relâmpago e os auxiliares internos de turno, cruzamento de meia-noite e apresentação. (`app/ponto/signals.py:23`; `app/ponto/management/commands/detectar_par_relampago.py:303`; `app/ponto/turnos.py:284`, `:320`, `:1013`, `:1098`, `:1186`, `:1294`, `:2198`, `:2918`)

3. **Os testes novos mordem, com lacunas** [nao verificado]

   No ensaio em memória, usando os corpos da cópia e substitutos de `localtime/override`, os três passaram no novo e os três falharam com o filtro antigo reconstruído do anexo. Isso confirma a distinção dos cenários; execução na suíte Django real: **[nao verificado]**. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:290`, `:305`, `:315`)

   - O primeiro exige duas inclusões e uma exclusão; o segundo exige inclusão na borda e exclusão antes dela. Uma implementação que entrega tudo vazio falha nesses testes. Os `assertNotIn`, isoladamente, seriam vacuosos, mas estão acompanhados de inclusão positiva. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:301`, `:302`, `:303`, `:312`, `:313`)
   - O terceiro é **individualmente vacuável**: dois resultados vazios — inclusive `{}` — satisfazem a igualdade. Não exige conteúdo esperado. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:327`, `:328`, `:329`)
   - O capturador usa `sorted`, portanto esconde regressões de ordem. Os testes não exigem a distribuição correta em B, cardinalidade exata, identidade dos objetos ou dinheiro calculado pelo motor real. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:274`, `:280`, `:301`, `:303`, `:329`)
   - Não há relógio solto nos três casos: datas e instantes são literais. O `override` envolve toda execução da porta, cobrindo a conversão local necessária; não congela relógio, mas esses casos não precisam disso. Não cobre outros fusos nem a distinção fuso ativo/configurado. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:245`, `:268`, `:299`, `:310`, `:326`; `app/ponto/motor_calculo_v2.py:28`)
   - A cronologia “RED antes do código”, afirmada na docstring, não é demonstrada pelos anexos: **[nao verificado]**. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:254`)

4. **Classes históricas de reprovação**

   | Classe | Conferência |
   |---|---|
   | Segundo juiz | `data_local` é conversor existente; não aparece como juiz registrado. O juiz de atribuição ao turno é `turnos_do_colab`. `dia_das_batidas` responde dia atribuído para apresentação, usando `data_turno` e depois dia civil para não reclamadas. Para o envelope civil, `data_local` é adequado; para preservar uma saída pertencente à jornada anterior, essa conversão é insuficiente. (`app/core/juizes.py:196`; `app/ponto/turnos.py:43`, `:2875`, `:2878`, `:2916`, `:2918`) |
   | Segundo escritor fora da porta | O diff apenas lê timestamps. **Existe escritor externo na cópia:** regularização externa atribui e salva `timestamp` diretamente. Não foi introduzido pelo diff fornecido. (`app/ponto/services/fechamento.py:204`; `app/chamados/services/regularizacao_ext.py:149`, `:154`, `:155`) |
   | Relógio fora da borda | O trecho novo fornece explicitamente o timestamp; `None` não dispara leitura implícita de “agora”, pois `data_local` o intercepta. (`app/ponto/services/fechamento.py:204`; `app/ponto/turnos.py:46`) |
   | Derivação paralela | A conversão civil é delegada à função existente. O julgamento por jornada continua posterior, no motor; a divergência civil/jornada permanece no filtro de pertencimento. (`app/ponto/services/fechamento.py:203`, `:261`; `app/ponto/motor_calculo_v2.py:2403`) |
   | Selo/RED sem mordida | Há mordida para a troca de referencial, mas o terceiro assert aceita vazio e o capturador mascara ordem. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:280`, `:301`, `:312`, `:329`) |
   | Ordem de trava | O trecho alterado não acrescenta trava/transação; continua antes do contexto de alimentação e da chamada ao motor. Ordem global de travas da aplicação: **[nao verificado]**. (`app/ponto/services/fechamento.py:204`, `:278`, `:286`) |
   | Pouso antes do pré-requisito | O diff não demonstra impacto de frota nem atendimento às condições de pouso de cura monetária. A L-113 alterou a L-092 e preserva DIFF antes de ato de frota; não cabe tratar a antiga recusa de exportada como lei intacta. Atendimento operacional: **[nao verificado]**. (`app/docs/LEIS.md:112`, `:150`; `app/ponto/services/fechamento.py:485`, `:495`) |
   | Campo com dois significados | Nenhum campo persistido recebe significado novo neste trecho; permanece a comparação entre dia civil do instante e data nominal da célula, que divergem no cruzamento da meia-noite. (`app/ponto/services/fechamento.py:204`, `:261`; `app/ponto/management/commands/gerar_celulas.py:67`) |

5. **Residual de saída em D+1 pertencente a B**

   Considere as células dos testes: 01→A, 02→B, 03→A; entrada **01/10 às 22h locais** e saída **02/10 às 07h locais**. Ambas têm data UTC 02/10. No filtro antigo, A perde ambas; no novo, A recebe a entrada e continua perdendo a saída. B pode receber ambas, pois a entrada local de 01 está no pad externo de sua fatia reduzida ao dia 02. Portanto, **o defeito já existia; o diff melhora a entrada desse exemplo, mas não recupera o par em A**. (`app/ponto/tests/test_composicao_de_fatias_e_total.py:294`; `app/ponto/services/fechamento.py:238`, `:244`, `:254`, `:261`; filtro antigo no diff fornecido)

   O motor não tem recuperação garantida: pareia as batidas recebidas; o caminho da ata exige encontrar a batida real correspondente à saída. Se B atribuir corretamente a jornada ao dia 01, seu recorte iniciado em 02 descarta esse período. O tratamento de dias órfãos tampouco cobre automaticamente o caso, pois D já foi declarado coberto por A. (`app/ponto/motor_calculo_v2.py:882`, `:2354`, `:640`, `:641`, `:2429`; `app/ponto/services/fechamento.py:267`, `:270`, `:796`)

   Não é correto afirmar que o diff “nunca toca” esse residual: saídas locais entre 21h e 23h59 mudam de chave UTC para local e podem mudar de pertinência. O que permanece é a exclusão por **dia civil de outro vínculo dentro da fatia**, sem consultar antes a propriedade da jornada. Impacto monetário desses casos em produção: **[nao verificado]**. (`app/ponto/services/fechamento.py:254`, `:261`; `app/ponto/turnos.py:2878`)

6. **Selo e ALLOWLIST**

   Os quatro asserts do contrato passaram em execução isolada do visitante/censo original, substituindo somente `SimpleTestCase` por `unittest.TestCase`. Execução completa sob Django: **[nao verificado]**. (`app/ponto/tests/test_contract_no_batida_date.py:236`, `:242`, `:251`, `:270`)

   **A entrada de `fechamento.py` continua não podre**, especificamente por `timezone.localtime(b.timestamp).date()` na linha 793. As extrações de `.entrada` dos períodos não são, sozinhas, justificativa para essa entrada: o visitante procura encadeamento/referência a `timestamp`. (`app/ponto/services/fechamento.py:793`, `:839`, `:935`, `:947`; `app/ponto/tests/test_contract_no_batida_date.py:185`, `:203`, `:208`, `:266`)

   O verde desse selo não certifica o filtro novo: o visitante não reconhece chamadas a `data_local`, e a exceção é por arquivo inteiro. Ele comprova ausência de novos ofensores reconhecidos e ausência de entradas podres, não correção da atribuição de jornada. (`app/ponto/tests/test_contract_no_batida_date.py:183`, `:201`, `:238`, `:266`)

## 06/11 19:54 · sessao b60fce496d2e · H12H06-B · sonda · origem=Code

Transcrição integral do dado fornecido [nao verificado]. Não executei novamente: o lançador grava arquivos em `bin/sonda_leitura.sh:132` e `bin/sonda_leitura.sh:250`, contrariando a regra de somente leitura.

```text
sonda_leitura: pre.py [f35cf407c2faf784] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

HASH_FM {"05": [615, "e29ff62650eebac5"], "06": [704, "9400c1a360842d79"], "07": [629, "2edced101a39a6c3"], "08": [618, "22459b830139a009"], "09": [607, "6e69a4d396e9f407"], "10": [624, "1cc01147a16e856b"], "11": [579, "5f2fd1dd77f3498b"], "12": [87, "afb9cb5ee9e2741c"]}
EXP {"pk": 2, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "80f417a64296887d8c1f58d580947e40aae78af2c21945c14485c0a96f9814c7", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 3, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "98edd7f6e5d72d68a427efcd40cf83e08814313ab7d10d8c858acea3711ebff2", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 4, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "98edd7f6e5d72d68a427efcd40cf83e08814313ab7d10d8c858acea3711ebff2", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 5, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "98edd7f6e5d72d68a427efcd40cf83e08814313ab7d10d8c858acea3711ebff2", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 6, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "98edd7f6e5d72d68a427efcd40cf83e08814313ab7d10d8c858acea3711ebff2", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 7, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "98edd7f6e5d72d68a427efcd40cf83e08814313ab7d10d8c858acea3711ebff2", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 8, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "71570435dbbe5246f59b19986230ad265eece33da6015177bf5b806b75d45e6b", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 9, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "1f2b81f241a8f8cd344a4cd211911f20ab4cdd4ae25d65b416072bff7df2ac02", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 10, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "3c51aae49a29ab84f6b2c21885f796beca6038b483394138c44715743fa95a18", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 11, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "3c51aae49a29ab84f6b2c21885f796beca6038b483394138c44715743fa95a18", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 12, "empresa_id": 4, "mes": 7, "ano": 2026, "hash_sha256": "68d299ff806aceb36e414b75fd18e6ea9043d556b351645072448cf024af010c", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 13, "empresa_id": 4, "mes": 7, "ano": 2026, "hash_sha256": "491f6fff352cc5102cd0d0a5f24f004edc433e792d114563a4e77b529a216c2f", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 14, "empresa_id": 4, "mes": 7, "ano": 2026, "hash_sha256": "491f6fff352cc5102cd0d0a5f24f004edc433e792d114563a4e77b529a216c2f", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 15, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "1897e08736e104ef3d9eb81917bb26c2849f60a21e6f3141bfc2b35acdc08f63", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 16, "empresa_id": 4, "mes": 6, "ano": 2026, "hash_sha256": "1897e08736e104ef3d9eb81917bb26c2849f60a21e6f3141bfc2b35acdc08f63", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 17, "empresa_id": 3, "mes": 7, "ano": 2026, "hash_sha256": "7d54b0e6602bf4ea8bf9615009cb4e470398f96a9e66b8623dfc03ec4874524e", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 18, "empresa_id": 2, "mes": 8, "ano": 2026, "hash_sha256": "790d8cd7ea0c631c1b76cb462bc9ebadc163787ffd7e854b1d5b9ea0b0d73f01", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 19, "empresa_id": 2, "mes": 8, "ano": 2026, "hash_sha256": "2b3ac743a5782a5122fc89fb917405d9e9529d141f8bb6d81e0c61610b043dd0", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 20, "empresa_id": 2, "mes": 9, "ano": 2026, "hash_sha256": "8a63e3baabc501af7d63aa3845c6a9221d7f18ac091b5f7038650a23aaa59dbc", "invalidada_em": "2026-09-30 20:07:06.058663+00:00", "substituida_por_id": 23}
EXP {"pk": 21, "empresa_id": 3, "mes": 9, "ano": 2026, "hash_sha256": "6e0f1048428293078bc82c21aa8756630446c394ef2c2a5b16cbb1efbb6626d8", "invalidada_em": "2026-09-30 20:07:07.142119+00:00", "substituida_por_id": 24}
EXP {"pk": 22, "empresa_id": 4, "mes": 9, "ano": 2026, "hash_sha256": "cb7f00fe59f84e505fce92aab2769242aec6671b688d7c514311518345c29773", "invalidada_em": "2026-09-30 20:07:07.385295+00:00", "substituida_por_id": 25}
EXP {"pk": 23, "empresa_id": 2, "mes": 9, "ano": 2026, "hash_sha256": "0e3e008b49b1ff73a89556a24e495f0e05f566cad644d8354f224300bbc5a703", "invalidada_em": "2026-10-01 14:57:23.407593+00:00", "substituida_por_id": 26}
EXP {"pk": 24, "empresa_id": 3, "mes": 9, "ano": 2026, "hash_sha256": "5c503b95f9f9cd35541db29a45a6447f026e272b9eedf27b346cda26b92810a6", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 25, "empresa_id": 4, "mes": 9, "ano": 2026, "hash_sha256": "84c78cd0871f5f5251db047a6d12054930f91c05a39ec46d6669fddc1d97a73b", "invalidada_em": null, "substituida_por_id": null}
EXP {"pk": 26, "empresa_id": 2, "mes": 9, "ano": 2026, "hash_sha256": "0ae5da67364cabfb5dcadde9481ec75d63b702ca95956627adbbb37079664d53", "invalidada_em": "2026-10-01 22:44:56.762182+00:00", "substituida_por_id": 27}
EXP {"pk": 27, "empresa_id": 2, "mes": 9, "ano": 2026, "hash_sha256": "361d0f9685f86d3a51d49b8f5194df469db8e4818c9cae0e9bd9f1461f9af0cc", "invalidada_em": null, "substituida_por_id": null}
AUS5198 {"pk": 5198, "data": "2026-10-24", "minutos": 720, "tipo": "falta"}
REG_PEND {"n": 0, "itens": []}
REG_PEND_FROTA 2
DISP_ABERTAS {"n": 23}
HASH_DP 9 11501 54c6419c61520d6a
HASH_DP 10 21418 9d68332a36ef63eb
ERROS {}
```

- REG_PEND: n=0; itens=[] [nao verificado].
- DISP_ABERTAS: n=23 [nao verificado].
- HASH_FM: 09=[607, "6e69a4d396e9f407"]; 10=[624, "1cc01147a16e856b"] [nao verificado].
- HASH_DP: 9=11501, 54c6419c61520d6a; 10=21418, 9d68332a36ef63eb [nao verificado].
- AUS5198: 720 minutos; ERROS={}; exceção não indicada no dado fornecido [nao verificado].

## 06/11 19:52 · sessao 1e760cda5396 · - · - · origem=Ronald (tmux)

Achados centrais:

- **Conversa continua sendo lida como dia do turno.** `abrir_conversa` grava `localdate()` (`app/chamados/models.py:2426`), mas `conversa` é só parâmetro de `_nascer`; o `create` não persiste essa distinção (`app/chamados/models.py:2498`). O C6 passa qualquer `p.data_evento`, inclusive de `escolha`, a `classificar_falta` antes do ramo de materialização (`app/chamados/juizes.py:1198`, `app/chamados/juizes.py:1202`).
- **Dedup e célula têm caminho concreto de colisão.** Reabertura administrativa aceita qualquer motivo do catálogo (`app/chamados/services/acoes_disputa.py:245`, `app/chamados/services/acoes_disputa.py:247`) e abre conversa `escolha` com esse motivo (`app/chamados/services/acoes_disputa.py:260`). Depois, abertura datada chama `do_fato`, que filtra apenas colaborador/motivo/data (`app/chamados/models.py:2396`, `app/chamados/models.py:2453`): pode recuperar essa conversa, atribuir-lhe a célula do turno (`app/chamados/models.py:2457`) e devolvê-la sem criar a pergunta `hora` (`app/chamados/models.py:2497`). A fábrica da célula também considera qualquer pergunta do mesmo dia/motivo como já perguntada, sem distinguir conversa (`app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/fabrica_da_celula.py:123`, `app/chamados/services/fabrica_da_celula.py:126`). Ocorrência em produção: [nao verificado].
- **A tela ainda chama o campo de dia civil do plantio.** O modal lê a data pelo filtro (`app/templates/chamados/partials/modal_fio.html:161`; `app/chamados/templatetags/chamados_extras.py:31`; `app/chamados/services/fio.py:87`) e exibe “(e aqui que a batida nasce)” (`app/templates/chamados/partials/modal_fio.html:166`). Isso contradiz o noturno: combina D com a hora e ajusta a saída para D+1 (`app/chamados/services/materializacao.py:414`, `app/chamados/services/materializacao.py:421`; `app/chamados/juizes.py:319`).

Inventário de leitores, agrupado. “Turno” abaixo significa que a leitura usa a data como chave do fato/célula/turno/competência; “exibição” apenas imprime/ordena a data, sem converter instante.

| Família e significado | Leitores de produção encontrados |
|---|---|
| Plantio, slot e turno | `_ts_tipo_do_plantio`, `_tipo_pelo_marco`: `app/chamados/services/materializacao.py:344`, `app/chamados/services/materializacao.py:279`; `_ajustar_saida_overnight`: `app/chamados/juizes.py:312`; `_slot_esperado` delega ao plantio em `app/chamados/juizes.py:346`; `_plantio_na_hora_do_slot`: `app/chamados/juizes.py:266`; `_turno_tem_saida`: `app/chamados/juizes.py:363`; `classe_do_lote` usa slot em `app/chamados/services/validacao.py:825`. |
| Julgamento de pergunta, célula e janela | `_celulas_da_pergunta`: `app/chamados/juizes.py:623`; `_pergunta_materializada_de_fato`: `app/chamados/juizes.py:659`, `app/chamados/juizes.py:666`, `app/chamados/juizes.py:750`, `app/chamados/juizes.py:754`; `_pergunta_viva_calc`: `app/chamados/juizes.py:1198`, `app/chamados/juizes.py:1202`; `dia_da_pergunta_trancado`: `app/chamados/juizes.py:932`; `_marco_esperado_da_grade`: `app/chamados/juizes.py:1524`; `resolvedora_valida`: `app/chamados/juizes.py:1964`; `_chave_memo_pv` somente memoriza o insumo: `app/chamados/juizes.py:815`. |
| Dia do chamado e competência | `_dia_da_disputa`: `app/chamados/juizes.py:76`; `dias_do_fato`: `app/chamados/juizes.py:209`; `_dias_das_perguntas_vivas`: `app/chamados/juizes.py:2030`; `_vivas_entre` / `_plano_pergunta_viva`: `app/chamados/models.py:351`, `app/chamados/models.py:377`; `via_pelo_fato` recebe a data em `app/chamados/services/disputa_emissao.py:1652`, `app/chamados/services/disputa_emissao.py:1724`, `app/chamados/services/relavra_via.py:101`, `app/chamados/services/materializacao.py:1241` e consulta ausência/turno em `app/chamados/juizes.py:1476`, `app/chamados/juizes.py:1480`. O ramo adesão recorta explicitamente o **dia civil correspondente à chave**, por desenho anterior, em `app/chamados/juizes.py:1461`. |
| Identidade e dedup | `do_fato` / `_nascer`: `app/chamados/models.py:2396`, `app/chamados/models.py:2452`; `gerar_perguntas_disputa` / `_ja_perguntou`: `app/chamados/services/disputa_emissao.py:389`, `app/chamados/services/disputa_emissao.py:405`, `app/chamados/services/disputa_emissao.py:1272`; `_candidata_reusavel` / `insumos_em_regularizacao`: `app/chamados/services/declaracao_texto.py:227`, `app/chamados/services/declaracao_texto.py:539`, `app/chamados/services/declaracao_texto.py:542`; `migrar_resolvedoras_vivas`: `app/chamados/services/fio.py:941`; `motivos_a_perguntar` / `perguntar`: `app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/fabrica_da_celula.py:137`; `retencao` / `_marco_ja_perguntado`: `app/chamados/services/pergunta_no_app.py:92`, `app/chamados/services/pergunta_no_app.py:109`. |
| Materialização de ausência e validação | `_declarar_ausencia_de_pergunta` / `_materializar_ausencia_de_resposta` / `perguntas_absorvidas_por`: `app/chamados/services/materializacao.py:473`, `app/chamados/services/materializacao.py:517`, `app/chamados/services/materializacao.py:607`; `_aceitar_a_folga`: `app/chamados/services/validacao.py:250`; `validar_pergunta` manda recalcular a data em `app/chamados/services/validacao.py:407`; `conferir_data_do_fato` usa `data_do_fato_de` e registra o valor em `app/chamados/services/validacao.py:624`. |
| Reconciliador | `lavrar_sem_turno_aberto`: `app/chamados/reconciliador.py:370`; `_dia_fora_do_alcance_c`: `app/chamados/reconciliador.py:726`; `_orfas_abertas`: `app/chamados/reconciliador.py:1040`; `veredito_orfa`: `app/chamados/reconciliador.py:1072`, `app/chamados/reconciliador.py:1080`; `semear_eventos_celula`: `app/chamados/reconciliador.py:1293`, `app/chamados/reconciliador.py:1297`; `reatar_pergunta_orfa`: `app/chamados/reconciliador.py:1543`, `app/chamados/reconciliador.py:1553`; `encerrar_orfas_pela_lampada` imprime data em `app/chamados/reconciliador.py:1142`. |
| Costura, eventos e lavra | `data_do_fato_de`: `app/chamados/services/fio.py:87`; `_costura_nao_mata_o_fio`: `app/chamados/services/fio.py:773`; `reapontar_resolvedora` só escreve quando a leitura detecta data ausente: `app/chamados/services/fio.py:835`; `linhas_do_evento` / `acordar_celulas` ligam por colaborador/data: `app/chamados/models.py:2690`, `app/chamados/models.py:2706`; `lavrar` calcula expiração: `app/chamados/veredito_lavrado.py:229`; `expira_em` lê a data como chave de competência: `app/chamados/veredito_lavrado.py:112`. |
| Outros serviços de dia/competência | `medir` / `reabrir`: `app/chamados/services/competencia_trancada.py:107`, `app/chamados/services/competencia_trancada.py:252`; `_dia_do_plantio` / `_plantar_o_dia`: `app/chamados/services/passivo_validado.py:113`, `app/chamados/services/passivo_validado.py:519`; `filhos_no_defeito`: `app/chamados/services/limbo_folgas.py:202`; `relavrar_dia`: `app/chamados/services/relavra_via.py:152`; `carimbos_sem_lastro_por_dono`: `app/chamados/services/vigia_fio.py:816`. |
| Folha, célula e calendário | `montar_previa` agrupa pergunta por dia da célula: `app/folha/previa.py:65`, `app/folha/previa.py:67`, `app/folha/previa.py:95`; `contexto_calendario`: `app/colaboradores/services/calendario.py:473`, `app/colaboradores/services/calendario.py:477`; `dia_do_colab`: `app/ponto/services/dia_do_colab.py:65`; `fechar_o_fio_do_dia`: `app/ponto/services/veredito_celula.py:176`. |
| Canal e cobrança | `perguntas_mudas_com_resposta` casa data da pergunta com dia do chamado em `app/chamados/canal.py:72`, cujo dia vem de `dia_do_fato` em `app/chamados/canal.py:45`; `checar_cobranca` imprime “sobre o seu ponto nos dias…” a partir do campo em `app/chamados/services/cobranca.py:83`, `app/chamados/services/cobranca.py:133`; `aviso_texto_livre`: `app/chamados/services/aviso_pendencia.py:20`. |
| API, textos, templates: exibição/ordenação | `api_chamados_v2`: `app/api/views.py:1174`; `api_disputa_responder` log: `app/api/views.py:2654`; `_item_validar`: `app/api/views_mensageria.py:568`; `validar_inline`: `app/chamados/views_cobrar.py:176`, `app/chamados/views_cobrar.py:185`; `explicar_pergunta` / `por_que_nao_fecha`: `app/chamados/services/explicador.py:70`, `app/chamados/services/explicador.py:122`, `app/chamados/services/explicador.py:126`; `_eventos_dos_chamados`: `app/chamados/services/historico.py:108`; resposta/log: `app/chamados/services/disputa_emissao.py:1355`, `app/chamados/services/disputa_emissao.py:1376`; duplicata/log: `app/chamados/services/fio.py:897`; templates diretos: `app/templates/chamados/partials/_card_atendimento.html:90`, `app/templates/chamados/partials/_card_atendimento.html:164`, `app/templates/chamados/partials/_lote_preview.html:24`, `app/templates/chamados/meu_atendimento.html:19`; filtros indiretos: `app/chamados/templatetags/chamados_extras.py:31`, `app/chamados/templatetags/chamados_extras.py:44`, `app/chamados/templatetags/chamados_extras.py:58`; modal: `app/templates/chamados/partials/modal_fio.html:161`. |
| Comandos | `dedup_perguntas_fato`: `app/chamados/management/commands/dedup_perguntas_fato.py:54`, `app/chamados/management/commands/dedup_perguntas_fato.py:57`, `app/chamados/management/commands/dedup_perguntas_fato.py:69`; `disparar_perguntas_competencia`: `app/chamados/management/commands/disparar_perguntas_competencia.py:213`; `reabrir_orfas_enterradas`: `app/chamados/management/commands/reabrir_orfas_enterradas.py:68`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:86`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:144`; `backfill_veredito_lavrado`: `app/chamados/management/commands/backfill_veredito_lavrado.py:111`; `encerrar_perguntas_com_fato` só imprime: `app/chamados/management/commands/encerrar_perguntas_com_fato.py:74`; `mapa_divergencia` cruza furo/pergunta por dia: `app/ponto/management/commands/mapa_divergencia.py:272`; vigia compara turno da batida com pergunta: `app/ponto/management/commands/vigia_plantio_overnight.py:85`, `app/ponto/management/commands/vigia_plantio_overnight.py:89`. |
| Auditor de invariantes | `_candidatos_i8`: `app/chamados/management/commands/auditar_invariantes_chamados.py:63`, `app/chamados/management/commands/auditar_invariantes_chamados.py:73`; `_contraprova_i8bx`: `app/chamados/management/commands/auditar_invariantes_chamados.py:148`; `_cobre_ausencia_i21`: `app/chamados/management/commands/auditar_invariantes_chamados.py:162`; `_piso_f3_antes_do_gerador`: `app/chamados/management/commands/auditar_invariantes_chamados.py:282`; `_contraprova_f3g`: `app/chamados/management/commands/auditar_invariantes_chamados.py:298`; `contraprova_piso`: `app/chamados/management/commands/auditar_invariantes_chamados.py:325`, `app/chamados/management/commands/auditar_invariantes_chamados.py:348`, `app/chamados/management/commands/auditar_invariantes_chamados.py:361`, `app/chamados/management/commands/auditar_invariantes_chamados.py:373`; `handle`: `app/chamados/management/commands/auditar_invariantes_chamados.py:892`, `app/chamados/management/commands/auditar_invariantes_chamados.py:914`, `app/chamados/management/commands/auditar_invariantes_chamados.py:926`, `app/chamados/management/commands/auditar_invariantes_chamados.py:930`, `app/chamados/management/commands/auditar_invariantes_chamados.py:957`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1662`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1705`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1775`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1801`. |

Sobre plantio de conversa: as portas normais de reabertura usam `escolha` (`app/chamados/services/acoes_disputa.py:263`) e âncora usa `data_hora` (`app/chamados/catalogo/perguntas.py:118`). `_slot_esperado` de `data_hora` lê a data declarada na resposta, não `data_evento` (`app/chamados/juizes.py:334`). O vigia novo só inclui `hora`, portanto exclui essas conversas normais (`app/ponto/management/commands/vigia_plantio_overnight.py:74`).

Porém `abrir_conversa` não restringe tipo/motivo (`app/chamados/models.py:2419`), e o gerador passa o tipo do catálogo ao fallback de conversa (`app/chamados/services/disputa_emissao.py:412`). Um contexto `data` malformado ainda produz texto (`app/chamados/catalogo/query.py:62`), falha no parse da data (`app/chamados/services/disputa_emissao.py:365`) e pode chegar como conversa `hora` quando não houver data derivável (`app/chamados/services/disputa_emissao.py:410`). Nesse caso slot/plantio/vigia lerão `localdate` como turno (`app/chamados/services/materializacao.py:344`; `app/ponto/management/commands/vigia_plantio_overnight.py:89`). Existência de contexto assim em produção: [nao verificado].

Os testes novos de conversa só verificam nascimentos distintos/localdate e recusa de `abrir` sem data; não cobrem a conversa já gravada sendo encontrada por abertura datada ou fábrica da célula (`app/chamados/tests/test_b2_data_conversa.py:14`, `app/chamados/tests/test_b2_data_conversa.py:26`).

_2508 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
