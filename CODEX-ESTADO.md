# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 10:06** · publicado em **12/11/2026 10:06** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 10:06 · sessao 0ab2e5092d0c · - · - · origem=Ronald (tmux)

1–3 H: corretas. O defeito é o atributo do pacote já carregado: API importa pelos pais (`app/api/views_mensageria.py:2077`, `:2100`); o harness agora substitui ambos e restaura no cleanup (`app/ponto/tests/test_lastro_c12_sem_banco.py:89`). Asserções continuam exigindo 1 buraco, zero explicados e vizinhos preservados (`:103`, `:109`, `:141`). Reverti somente esse bloco em AST: três falhas de busca/grade proibidas, zero erros; a fixture adversarial está em `app/ponto/tests/test_lastro_c13_sem_banco.py:92`. Achado: nada.

4 C: correta por inspeção; a inversão exige recusa nomeada, `sem_certidao` e veredito vazio (`app/colaboradores/tests/test_calendario_ausencia_pela_autoridade.py:59`). O calendário já recebe a recusa A2 e transporta seu rótulo (`app/colaboradores/services/calendario.py:510`, `:607`, `:714`). Não apagou o caso nem substituiu por sucesso genérico (`app/colaboradores/tests/test_calendario_ausencia_pela_autoridade.py:52`). Execução Django/ORM: [nao verificado]. Achado: nada.

5 C: correta. `licenca especial` vira `licenca_especial` pela view (`app/ponto/views_tipos_ausencia.py:92`); esse código não consta do mapa (`app/ponto/precedencia.py:52`). O novo sucesso cria `reciclagem`, cuja faixa já existe (`app/ponto/tests/test_tela_tipos.py:31`; `app/ponto/precedencia.py:70`), verifica crescimento e palavra pelo compositor (`app/ponto/tests/test_tela_tipos.py:37`, `:47`). O par exige não gravar e erro nomeado (`:51`). A porta consulta o juiz antes de gravar (`app/ponto/services/ausencia.py:1312`). Execução HTTP/ORM: [nao verificado]. Achado: nada.

6–7 R: corretas quanto à perda/restauração do contrato; os selos originais exigem o contrato (`app/escala/tests/test_f1c_contrato.py:70`; `app/relatorios/tests/test_cartao_pela_celula.py:176`) e o coletor voltou a chamá-lo (`app/relatorios/pdf_espelho.py:474`). A nova prova executa as mesmas funções dos selos (`app/ponto/tests/test_lastro_c13_sem_banco.py:130`). Achado dos testes: nada; isso sozinho não certifica ausência de regras próprias no restante do PDF (`app/escala/tests/test_f1c_contrato.py:68`).

8–9 R: corretas quanto às expectativas preservadas: espúria deve ser marcada e não produzir turno aberto (`app/relatorios/tests/test_cartao_marca_espuria.py:59`, `:75`). O substituto sem banco agora exige `orfa=True`/zero aberto (`app/ponto/tests/test_lastro_c11_sem_banco.py:384`); suprimir as órfãs restauradas é o mutante (`:68`) e ficou vermelho nesta reexecução. O teste nativo também exige o texto e a batida no PDF binário (`app/relatorios/tests/test_cartao_marca_espuria.py:84`); essa execução está [nao verificado]. Achado dos testes: nada.

10 H: atualização do registro preserva a igualdade estrita (`app/core/tests/test_medida_turno_do_par.py:542`); a ocorrência atual é a consulta de feriados em `app/ponto/services/espelho.py:1039`. Rodei os 35 testes existentes: verdes; recolocar somente `[1032]` em memória causa uma falha de igualdade, zero erros (`app/core/tests/test_medida_turno_do_par.py:541`). A identidade AST histórica c9/c12 está [nao verificado], pois esta cópia não contém `.git`; o delta fornecido não altera o produto `espelho.py` [nao verificado]. Achado: nada.

Verificação própria: os 72 testes C5/C7/C10/C11/C12/C13 passaram; os 11 mutantes C13 e os oito vizinhos executados ficaram vermelhos nas asserções/canais correspondentes (`app/ponto/tests/test_lastro_c13_sem_banco.py:45`, `:73`; `app/ponto/tests/test_lastro_c12_sem_banco.py:34`; `app/ponto/tests/test_lastro_c10_sem_banco.py:296`; `app/ponto/tests/test_lastro_c11_sem_banco.py:68`). São costuras: o teste dos builders reduz seus corpos às chamadas extraídas, portanto não executa a geometria/ORM completa (`app/ponto/tests/test_lastro_c13_sem_banco.py:196`, `:200`).

FILA — portabilidade do replay: `--red-c12` e `--mutante harness_c12` dependem de `git show`, indisponível nesta cópia sem repositório (`app/ponto/tests/test_lastro_c13_sem_banco.py:27`, `:40`). Contra-exemplo: invocar o replay nesta cópia falha antes de exercer a cura; substituí a reversão por AST em memória para verificar a mordida [nao verificado quanto à execução do replay original].

## 12/11 10:06 · sessao 38bc72b0ad37 · - · - · origem=Ronald (tmux)

- **BLOQUEIA — PDF mantém regra própria de órfã/turno aberto, preexistente e não curada.** `_entrada_espuria` compara apenas HH:MM (app/relatorios/pdf_espelho.py:174–178), depois da união das órfãs de todas as datas (:480–481), para excluir turnos abertos (:517–519). Contraexemplo executado em memória: órfã em 10/11 às 07:02; turno aberto legítimo em 11/11 às 07:02, sem órfã nesse dia → função retorna `True` e exclui o turno. Tela conta `p.turno_aberto` sem essa guarda (app/ponto/services/espelho.py:436–437). Viola testemunha sem regra própria (CLAUDE.md:20–22).

- **BLOQUEIA — c13 restaura o contrato no PDF, mas conserva duas respostas para a mesma órfã.** PDF usa builder `grade_espelho_janela` (app/relatorios/pdf_espelho.py:474); tela usa `grade_da_celula`/`remapear_na_regua` (app/ponto/services/espelho.py:522–530). O novo teste fixa concretamente `orfas_grade=[]` para tela e `batida.orfa=True`, `turnos_abertos=0` para PDF (app/ponto/tests/test_lastro_c11_sem_banco.py:383–385). O teste chama tela por `Mock` (:326) e fornece órfã pronta ao builder (:356–357); não certifica convergência dos leitores. Classificações R de 6–9 fazem sentido, mas a cura dos sintomas não fecha a classe.

- **BLOQUEIA — rótulo não está garantido no ramo de ata agregada, defeito anterior não alcançado pelo canal c13.** Tela chama contrato sem `recusas` em `_pela_grade` (app/ponto/services/espelho.py:495–496). Se `grade_da_celula` levanta `AtaAgregadaSemMarcos`, seu handler chama `_pela_grade` (:563); uma `TipoSemFaixa` levantada ali escapa ao `except Exception` irmão (:572). PDF passa pelo contrato com `recusas={}` (app/relatorios/pdf_espelho.py:474), mas depois chama essa tela (:479). Contraexemplo: ata agregada + ausência aprovada sem faixa → leitura pode levantar em vez de apresentar o rótulo. O comportamento sem destinatário está explicitamente preservado e cobrado (app/ponto/tests/test_lastro_c13_sem_banco.py:126–127). Ocorrência produtiva: [nao verificado].

- **Rótulo no canal coberto:** nasce de `TipoSemFaixa.rotulo` (app/ponto/precedencia.py:102), chega ao PDF por `dia['recusa']` (app/escala/utils.py:1219; app/relatorios/pdf_espelho.py:567) e ao compositor único (app/ponto/services/espelho.py:339–341), que escreve palavra e veredito vazio. Tela transporta o mesmo atributo (app/ponto/services/espelho.py:880). Não encontrei contraexemplo novo de apagamento do rótulo por sobreposição em :567 com mesmas entradas/janela; universalidade fora desses canais: [nao verificado].

## 12/11 10:06 · sessao e928c079842b · - · - · origem=Ronald (tmux)

- BLOQUEIA — leitura nomeada incompleta no espelho/PDF: `_pela_grade()` chama o contrato **sem `recusas`** em `app/ponto/services/espelho.py:495`; esse caminho roda no trecho anterior à competência (`app/ponto/services/espelho.py:534`) ou na ata agregada (`app/ponto/services/espelho.py:563`), antes da carga nomeada (`app/ponto/services/espelho.py:653`). Contra-exemplo reproduzido em memória: ausência aprovada `fantasma_xyz` faz `_pela_grade()` levantar `TipoSemFaixa`; o PDF também chama esse espelho (`app/relatorios/pdf_espelho.py:479`). Execução completa com ORM: [nao verificado].

- Escritor do dicionário `recusas[data]`: A2, `app/ponto/precedencia.py:423`; sem destinatário, levanta em `app/ponto/precedencia.py:422`. A recusa impede produzir fato nesse dia (`app/ponto/precedencia.py:425`).

- Escritor de `dia['recusa']` e `dia['tipo_dia']=None`: `app/escala/utils.py:1219` e `app/escala/utils.py:1220`. Leitor produtivo encontrado de **essa recusa da grade**: `app/relatorios/pdf_espelho.py:567`; o compositor imprime a palavra e deixa veredito vazio em `app/ponto/services/espelho.py:339`.

- `tipo_dia=None` também é lido internamente pelo overlay de isenção: não ganha a flag de dia de trabalho (`app/escala/utils.py:889`), chamado depois da ausência (`app/escala/utils.py:1875`); o legacy preserva `None`, pois o default de `.get()` só cobre chave ausente (`app/escala/utils.py:1046`).

- Nenhum caminho financeiro demonstrado recebe o novo `None`: o único chamador produtivo encontrado que abre `recusas` na grade é o PDF (`app/relatorios/pdf_espelho.py:474`); `grade_do_fechamento()` lê `grade_da_celula` (`app/folha/export.py:335`, `app/folha/export.py:338`). Fechamento lê essa grade em `app/ponto/services/fechamento.py:1015` e seus mapas em `app/ponto/services/fechamento.py:1048`; `simular_folha --recalcular/--lido` passa por esse fechamento (`app/folha/management/commands/simular_folha.py:122`, `app/folha/management/commands/simular_folha.py:130`).

- Leitores potencialmente incompatíveis existem, porém alcance pelo canal novo não foi demonstrado: `por_dia_da_grade` descarta `None` sem `ausencia_tipo` (`app/ponto/services/dia_pago.py:392`); `classificar_orfas` traduz `None` para folga (`app/escala/orfa_classe.py:65`). Nenhum chamador produtivo de `classificar_orfas` encontrado no censo textual: [nao verificado]. Não classificar isso como regressão alcançável da c13.

- Sem `recusas`, o delta conserva os guards e chamadas anteriores: retorno cedo em `app/escala/utils.py:1205`, condição de consulta A2 em `app/escala/utils.py:1213`, ramo novo desativado em `app/escala/utils.py:1218`; os transportes omitem o argumento quando `None` (`app/escala/utils.py:808`, `app/escala/utils.py:1010`, `app/escala/utils.py:1019`, `app/escala/utils.py:1873`, `app/escala/utils.py:2114`, `app/escala/utils.py:2117`, `app/escala/utils.py:2365`). Equivalência dinâmica de todos os chamadores: [nao verificado].

- BLOQUEIA pouso financeiro — par `simular_folha` da competência 11 e efeito `abona=0`: [nao verificado], conforme dado fornecido. O comparador mede TXT e retidos (`app/folha/management/commands/simular_folha.py:160`, `app/folha/management/commands/simular_folha.py:218`); inspeção de código não substitui esse par: [nao verificado].

## 12/11 09:57 · sessao 12930e22770f · LASTRO-FAIXA-SEM-DEFAULT · construtor · origem=Code

Cura deixada na cópia. Classificação dos dez vermelhos, alterações, RED, mutantes e comandos estão em [LASTRO_C13.txt](LASTRO_C13.txt:1).

Passaram Ruff, 72 testes sem banco, 35 da catraca e 8 estáticos; 22 mutantes foram rejeitados. [Evidências](LASTRO_C13.txt:156) [nao verificado]

Suite com Django, PDF real e par financeiro da competência 11: **[não verificado]**. [Limites](LASTRO_C13.txt:274) [nao verificado]

## 12/11 09:57 · sessao 28365f5c4971 · PUSH-SEM-REGRAVAR · verificador · origem=Code

**O v3 fecha as perdas do comparador anterior, mas não fecha toda a auditoria:** comparar stdout em bytes ainda depende do que o Git apresenta; a mordida de `lugar` depende do contexto; e “histórico já julgado” continua sem comprovação. (`bin/tests/test_lei_akita.sh:390`, `bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:416`, `bin/tests/test_lei_akita.sh:888`)

**1) Fechamento dos achados do v2**

| Achado | Veredito |
|---|---|
| Falso positivo de lugar | **Fecha o defeito do comparador:** agora conserva cabeçalhos, contexto e posição. A fixture distingue as duas alterações, com a ressalva de não vacuidade detalhada abaixo. (`bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:416`, `bin/tests/test_lei_akita.sh:845`, `bin/tests/test_lei_akita.sh:852`) |
| LF final/binário | **Fecha a perda anterior por normalização e filtragem:** retorna `p.stdout` em bytes e acrescenta `--binary`. A comparação deixou de usar `text=True`, `splitlines()` e seleção de linhas `+/-`. Isso garante igualdade da saída capturada, condicionada à apresentação feita pelo Git. (`bin/tests/test_lei_akita.sh:395`, `bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:404`) |
| Mordida C — prosa | **Fecha a ausência da prova específica:** planta a célula com `armadilha:` e reprova qualquer termo que conserve essa prosa. (`bin/tests/test_lei_akita.sh:457`, `bin/tests/test_lei_akita.sh:470`) |
| Mordida D — saída malformada | **Fecha parcialmente:** injeta `lixo` e exige `PADRAO_ERRO`; também protege os dois SHAs do mesmo pid. Porém retirar somente a validação hexadecimal, conservando `len(cs) != 2`, sobrevive aos casos novos. (`bin/tests/test_lei_akita.sh:362`, `bin/tests/test_lei_akita.sh:474`, `bin/tests/test_lei_akita.sh:479`) |
| Frase final | **Fecha a explicação das populações N/M e das dispensas. Não comprova julgamento anterior do publicado.** (`bin/tests/test_lei_akita.sh:407`, `bin/tests/test_lei_akita.sh:412`, `bin/tests/test_lei_akita.sh:426`, `bin/tests/test_lei_akita.sh:888`) |

As bordas anteriores de vocabulário permanecem: o inteiro com vírgula continua incompatível com o parser do campo; o literal completo `ADR (Architecture Decision Record)` continua sendo separado, em vez de acrescentado inteiro. (`app/docs/GLOSSARIO.md:13`, `app/docs/GLOSSARIO.md:28`, `bin/tests/test_lei_akita.sh:114`, `bin/tests/test_lei_akita.sh:125`, `bin/tests/test_lei_akita.sh:319`)

**2) Commit novo com diff diferente pode sair como DUPLICATA?** [nao verificado]

**Sim, há um contraexemplo construído com apresentação não injetiva por `textconv`:** os dois produtores de diff permitem essa conversão; nenhum passa `--no-textconv`. A segunda comparação verifica a saída apresentada, sem comparar diretamente árvores ou blobs. (`bin/tests/test_lei_akita.sh:390`, `bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:416`)

Considere esta configuração e estes commits hipotéticos: [nao verificado]

```text
diff.default.textconv = sed '/^IGNORADO=/d'

Publicado P: [nao verificado]
  A: valor=0 → valor=1

Local L, sobre a mesma base:
  A: valor=0 → valor=1
  F: IGNORADO=0 → IGNORADO=1
```

A conversão conserva o patch de `A` e torna iguais as imagens de `F`, ocultando inteiramente sua alteração. Assim, `log -p` fornece o mesmo patch-id e `show --binary` apresenta os mesmos bytes; `L` encontra `P` e entra em `DUPLICATAS`, embora seu diff bruto contenha uma alteração adicional. Esse é o caminho permitido pelos dois comandos e pela dispensa. (`bin/tests/test_lei_akita.sh:390`, `bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:416`, `bin/tests/test_lei_akita.sh:424`)

Execução ponta a ponta desse cenário com commits reais e ocorrência em produção: **[nao verificado]**. [nao verificado]

Quanto às configurações citadas:

| Configuração | O que se pode concluir |
|---|---|
| `diff.noprefix`, `mnemonicPrefix`, cor | Os dois lados usam o mesmo comando e repositório. Mudança de representação, isoladamente, não demonstra falsa igualdade; se resultar em bytes diferentes, a dispensa falha. (`bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:415`, `bin/tests/test_lei_akita.sh:416`) |
| `textconv` | Pode eliminar informação antes da comparação, como no contraexemplo acima; não exige configuração diferente entre os lados. (`bin/tests/test_lei_akita.sh:390`, `bin/tests/test_lei_akita.sh:400`) |
| `ext-diff` | Não há `--ext-diff` nos comandos; a simples ausência de `--no-ext-diff` não demonstra execução de um externo. Habilitação efetiva nesse ambiente: **[nao verificado]**. (`bin/tests/test_lei_akita.sh:390`, `bin/tests/test_lei_akita.sh:400`) |

O código também não congela configuração entre subprocessos. Uma mudança efetiva de configuração entre as chamadas que tenha causado dispensa indevida nos dados fornecidos: **[nao verificado]**. (`bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:415`, `bin/tests/test_lei_akita.sh:416`)

**3) `lugar` tem realmente o mesmo patch-id?**

**Sim, com contexto padrão de três linhas.** A base tem dois blocos iguais; o publicado troca o primeiro `V`, e o local troca o segundo. Reconstruindo esses patches em memória e passando-os a `git patch-id --stable`, ambos resultam em `eef41b6dc7f223182f6b375487979f6d30ec0357`. (`bin/tests/test_lei_akita.sh:820`, `bin/tests/test_lei_akita.sh:822`, `bin/tests/test_lei_akita.sh:828`, `bin/tests/test_lei_akita.sh:845`)

Os hunks têm o mesmo corpo — contexto `a,b,c`, remoção `V`, adição `W`, contexto `d,e,f` — mas cabeçalhos diferentes: `@@ -1,7 +1,7 @@` e `@@ -9,7 +9,7 @@`. Portanto, o comparador anterior os igualava; o novo distingue seus bytes. (`bin/tests/test_lei_akita.sh:820`, `bin/tests/test_lei_akita.sh:828`, `bin/tests/test_lei_akita.sh:845`, `bin/tests/test_lei_akita.sh:416`)

**A prova não está protegida contra vacuidade por configuração.** Com contexto quatro, `sep` passa a integrar os contextos em posições diferentes, e os pids reconstruídos divergem: `d05d6ed6fc9a394fc975b679e1d2ff95dfe49b95` e `dc2bd81c57071c4a778df9ca541ed480b4a55ecb`. O comando não fixa `-U3`, e a asserção não exige explicitamente igualdade dos pids; nesse caso, retirar a comparação adicional ainda deixaria `lugar` julgado. (`bin/tests/test_lei_akita.sh:390`, `bin/tests/test_lei_akita.sh:820`, `bin/tests/test_lei_akita.sh:852`)

**4) Escritores, leitores e verdade da frase**

| Informação | Escritores e leitores |
|---|---|
| `DUPLICATAS` | Zerada por chamada; recebe local8/publicado12 antes do `continue`. MORDE confere o mapa; emissão normal produz `PADRAO_DUPLICATA`; shell escreve `ja julgado:` e conta os registros na frase final. (`bin/tests/test_lei_akita.sh:386`, `bin/tests/test_lei_akita.sh:424`, `bin/tests/test_lei_akita.sh:850`, `bin/tests/test_lei_akita.sh:852`, `bin/tests/test_lei_akita.sh:871`, `bin/tests/test_lei_akita.sh:879`, `bin/tests/test_lei_akita.sh:888`) |
| `JULGADOS` | Zerado com `ENUMERADOS=len(hs)`; incrementado após excluir ausência de linha e duplicatas. MORDE exige quatro; emissão produz `PADRAO_JULGADOS N de M`; shell interpola esse registro. (`bin/tests/test_lei_akita.sh:407`, `bin/tests/test_lei_akita.sh:419`, `bin/tests/test_lei_akita.sh:426`, `bin/tests/test_lei_akita.sh:852`, `bin/tests/test_lei_akita.sh:873`, `bin/tests/test_lei_akita.sh:888`) |
| Frase final | Escrita pelo `echo` quando `falhas=0`. O pre-push consome o código de saída; a régua captura a saída e a exibe na falha, sem interpretar os contadores. (`bin/tests/test_lei_akita.sh:887`, `bin/tests/test_lei_akita.sh:888`, `bin/pre-push.sh:38`, `bin/regua.sh:140`, `bin/regua.sh:142`) |

**As contagens estão descritas corretamente; “histórico já julgado” não é garantido.** Um publicado sem linha `LEI-AKITA:` pode fornecer o patch usado para dispensar um local com linha inválida, embora o próprio publicado não fosse julgado por esse instrumento. Publicação e julgamento anterior são condições diferentes no código. (`bin/tests/test_lei_akita.sh:412`, `bin/tests/test_lei_akita.sh:416`, `bin/tests/test_lei_akita.sh:419`, `bin/tests/test_lei_akita.sh:424`)

A alegação das **14 duplicatas reais iguais byte a byte** consta somente do comentário; comprovação no dado fornecido: **[nao verificado]**. (`bin/tests/test_lei_akita.sh:399`)

## 12/11 09:54 · sessao ec9bb461cbab · - · - · origem=Ronald (tmux)

Confirmação adicional: `git hash-object --stdin`, sem `-w`, confirmou os blobs usados na reconstrução da fixture (`bin/tests/test_lei_akita.sh:820`, `bin/tests/test_lei_akita.sh:828`, `bin/tests/test_lei_akita.sh:845`).

| Contexto reconstruído | Patch-ids | Comparador antigo | Bytes completos |
|---|---|---|---|
| 3 linhas | ambos `eef41b6dc7f223182f6b375487979f6d30ec0357` | iguais | diferentes |
| 4 linhas | `d05d6ed6fc9a394fc975b679e1d2ff95dfe49b95` / `dc2bd81c57071c4a778df9ca541ed480b4a55ecb` | iguais | diferentes |

A condição de pid igual está demonstrada para contexto três; a fixture não fixa esse contexto nem o verifica expressamente (`bin/tests/test_lei_akita.sh:390`, `bin/tests/test_lei_akita.sh:852`). A configuração efetiva e a execução integral do MORDE permanecem **[nao verificado]**.

## 12/11 09:54 · sessao c049468680ad · - · - · origem=Ronald (tmux)

Contraexemplo condicional mais forte: publicado altera `A`; local altera `A` identicamente e também um gitlink `S`. Com configuração que oculte completamente a alteração de submódulo, `log -p` e `show --binary` enxergam somente `A`; os pids e os bytes comparados coincidem, entrando em `DUPLICATAS`. Os comandos não fixam `--ignore-submodules=none`. Ocorrência/configuração efetiva e execução desse cenário: **[nao verificado]**. (`bin/tests/test_lei_akita.sh:390`, `bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:416`, `bin/tests/test_lei_akita.sh:424`)

Outro contraexemplo condicional usa `textconv`: além do patch comum `A`, o local altera `F`, mas a conversão de `F` devolve o mesmo conteúdo antes/depois e elimina inteiramente seu diff. Ambos os comandos deixam de desabilitar textconv; se `F` desaparecer nos dois, a igualdade dos bytes não detectará a alteração. A omissão efetiva e a configuração são **[nao verificado]**. (`bin/tests/test_lei_akita.sh:390`, `bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:416`)

Para as cinco configurações pedidas:

- `diff.noprefix`, `color` e `mnemonicPrefix`: os comandos não fixam esses valores; mudança de representação não demonstra, sozinha, falsa igualdade. Não construí contraexemplo de dispensa provocado apenas por esses três. **[nao verificado]**. (`bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:416`)
- `textconv`: candidato de perda de informação descrito acima; `--binary` não é explicitamente acompanhado de `--no-textconv`. (`bin/tests/test_lei_akita.sh:400`)
- `ext-diff`: não há `--ext-diff` nem `--no-ext-diff`; o comando, isoladamente, não prova execução de filtro externo. A habilitação padrão de externos em `show/log` seria uma afirmação sobre Git que exige qualificação **[nao verificado]** sob a restrição de fontes do usuário. (`bin/tests/test_lei_akita.sh:390`, `bin/tests/test_lei_akita.sh:400`)

Os dois lados de `diff_inteiro` passam pelo mesmo comando, no mesmo `repo`; o código não fornece configurações diferentes por lado nem congela a configuração entre subprocessos. Drift real entre chamadas: **[nao verificado]**. (`bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:415`, `bin/tests/test_lei_akita.sh:416`)

A cópia não contém repositório Git utilizável; testar `git show HEAD` falhou por ausência de `.git`. Não executei fixtures nem criei arquivos. **[nao verificado]**. [nao verificado]

## 12/11 09:53 · sessao ec9bb461cbab · - · - · origem=Ronald (tmux)

Referências abaixo em `bin/tests/test_lei_akita.sh`, salvo indicação contrária.

- **`lugar` não é mordida vazia com contexto padrão de três linhas.** A base contém dois blocos iguais (`:820`, `:822`); o publicado muda o primeiro `V` (`:828`) e o local muda o segundo (`:845`). Reconstruí os dois patches em memória e executei somente `git patch-id --stable`, sem criar arquivos: ambos deram `eef41b6dc7f223182f6b375487979f6d30ec0357`. Os hunks são `@@ -1,7 +1,7 @@` e `@@ -9,7 +9,7 @@`, ambos com ` a / b / c / -V / +W / d / e / f`. O cabeçalho `index` também difere: `7c92588..aa0c6d0` versus `7c92588..f53da47`; logo `diff_inteiro` distingue ambos (`:400`, `:416`).
- **Ressalva de configuração:** o código não fixa `-U3` no cálculo de pid (`:390`) nem afirma explicitamente a igualdade dos pids no MORDE (`:852`). Reconstruindo com contexto quatro, os pids passam a `d05d6ed6fc9a394fc975b679e1d2ff95dfe49b95` e `dc2bd81c57071c4a778df9ca541ed480b4a55ecb`: `sep` entra em posições diferentes do contexto. Nesse ambiente a mordida fica vazia. O MORDE apaga variáveis `GIT_`, mas não isola configuração global (`:69`). Configuração e execução em produção: **[nao verificado]**.
- **Lugar, LF final e binário:** a comparação conserva agora stdout em bytes e usa `--binary`, sem `text=True`, `splitlines()` ou filtro `+/-` (`:395`, `:400`, `:404`). Isso fecha os contraexemplos de perda por filtragem/normalização. A nova fixture cobre lugar (`:845`, `:852`); fixtures específicas de LF final e binário no delta: **não há** (`:813`, `:855`).
- **C ganhou mordida específica de prosa:** a fixture inclui `Ggg Hhh (armadilha: iii jjj)` (`:457`) e exige `ggg hhh`, proibindo qualquer termo contendo `armadilha` (`:470`). Reintroduzir a célula inteira com prosa faz essa condição falhar; a implementação remove parênteses (`:125`). A borda da célula com vírgula permanece: `app/docs/GLOSSARIO.md:13`, parser `:319`; o literal inteiro com `ADR (...)` continua não acrescentado por esse caminho (`:125`; `app/docs/GLOSSARIO.md:28`).
- **D ganhou mordida, mas não de cada requisito:** o parser usado pelo instrumento é o helper (`:394`); o MORDE injeta linha válida seguida de `lixo` e exige `PADRAO_ERRO` (`:474`, `:477`). Também exige conservar ambos os SHAs (`:479`). Retirar somente o teste hexadecimal e manter `len(cs) != 2` **sobrevive**: não há caso com dois tokens inválidos (`:362`, `:474`, `:479`). Completude, pertencimento à seleção e stdout vazio continuam sem verificação no helper (`:360`, `:365`). [nao verificado]
- **Frase final:** agora explica corretamente numerador, denominador e número de dispensas (`:888`): `ENUMERADOS=len(hs)` (`:406`, `:407`), pula ausência de linha (`:419`), pula duplicatas (`:423`) e só depois incrementa `JULGADOS` (`:426`). Porém **“histórico já julgado” continua uma premissa**, não uma prova de julgamento anterior: candidatos vêm de `origin/main` e piso (`:412`); não há consulta a resultado anterior (`:416`, `:424`).
- **Escritores/leitores:** `DUPLICATAS` é zerada (`:386`), acrescentada (`:424`), lida pelo MORDE (`:850`), emitida (`:871`), traduzida pelo shell para `ja julgado:` (`:879`) e contada na frase (`:888`). `JULGADOS` é zerado (`:407`), incrementado (`:426`), conferido pelo MORDE (`:852`), emitido com `ENUMERADOS` (`:873`) e interpolado na frase (`:888`). O MORDE termina antes da emissão normal/final (`:866`, `:867`); portanto não testa a veracidade da redação humana (`:888`).

## 12/11 09:53 · sessao f284376b3ac7 · - · - · origem=Ronald (tmux)

- `DUPLICATAS`: declarada global e zerada por chamada; recebe `(local[:8], publicado[:12])` antes do `continue`. (`bin/tests/test_lei_akita.sh:385`, `bin/tests/test_lei_akita.sh:386`, `bin/tests/test_lei_akita.sh:424`, `bin/tests/test_lei_akita.sh:425`)
- Leitores: MORDE converte em dicionário e exige mapa exato; execução normal emite `PADRAO_DUPLICATA`; shell transforma em `ja julgado:` e conta os registros na frase final. (`bin/tests/test_lei_akita.sh:850`, `bin/tests/test_lei_akita.sh:852`, `bin/tests/test_lei_akita.sh:871`, `bin/tests/test_lei_akita.sh:872`, `bin/tests/test_lei_akita.sh:879`, `bin/tests/test_lei_akita.sh:888`)
- `JULGADOS`: zerado juntamente com `ENUMERADOS=len(hs)`; incrementado depois de excluir commits sem linha e duplicatas. MORDE exige quatro; execução normal emite `PADRAO_JULGADOS N de M`; shell interpola na frase final. (`bin/tests/test_lei_akita.sh:407`, `bin/tests/test_lei_akita.sh:419`, `bin/tests/test_lei_akita.sh:423`, `bin/tests/test_lei_akita.sh:426`, `bin/tests/test_lei_akita.sh:852`, `bin/tests/test_lei_akita.sh:873`, `bin/tests/test_lei_akita.sh:888`)
- A frase final é escrita pelo `echo` do ramo `falhas=0`. O pre-push usa o código de saída; a régua captura a saída e a exibe apenas na falha; nenhum desses leitores interpreta os contadores. (`bin/tests/test_lei_akita.sh:887`, `bin/tests/test_lei_akita.sh:888`, `bin/pre-push.sh:38`, `bin/regua.sh:140`, `bin/regua.sh:142`)

**Contagem/texto:** a descrição das populações está corrigida: N conta commits com linha que não foram dispensados; M conta todos os enumerados em `head ^origin/main`, respeitado o piso; duplicatas são contadas pelos registros emitidos. (`bin/tests/test_lei_akita.sh:383`, `bin/tests/test_lei_akita.sh:406`, `bin/tests/test_lei_akita.sh:407`, `bin/tests/test_lei_akita.sh:419`, `bin/tests/test_lei_akita.sh:424`, `bin/tests/test_lei_akita.sh:426`, `bin/tests/test_lei_akita.sh:872`, `bin/tests/test_lei_akita.sh:888`)

**“Histórico já julgado” continua sem prova e não é universalmente verdadeiro:** candidatos publicados são todos os patches de `origin/main` desde o piso, sem checagem de mensagem ou julgamento anterior. Um publicado sem linha `LEI-AKITA:` não seria julgado por esse instrumento, mas ainda pode dispensar um local com o mesmo patch e linha inválida; logo, “publicado” não implica “já julgado”. (`bin/tests/test_lei_akita.sh:390`, `bin/tests/test_lei_akita.sh:412`, `bin/tests/test_lei_akita.sh:416`, `bin/tests/test_lei_akita.sh:419`, `bin/tests/test_lei_akita.sh:422`, `bin/tests/test_lei_akita.sh:424`)

A afirmação de “14 duplicatas reais [...] iguais byte a byte” está apenas no comentário; comprovação pelos anexos fornecidos: **[nao verificado]**. (`bin/tests/test_lei_akita.sh:399`)

## 12/11 09:49 · sessao 06c5cff16f72 · PUSH-SEM-REGRAVAR · verificador · origem=Code

**O delta fecha a perda de associações e o diagnóstico de registros malformados; fecha parcialmente whitespace, vocabulário e contagem.** A dispensa continua sem provar equivalência de negócio, e o texto final não explicita a população contada. (`bin/tests/test_lei_akita.sh:389`, `bin/tests/test_lei_akita.sh:406`, `bin/tests/test_lei_akita.sh:388`, `bin/tests/test_lei_akita.sh:856`)

O anexo não identifica os achados por letras; adoto abaixo A=mapa, B=whitespace, C=vocabulário, D=saída malformada, E=contagem. Correspondência com uma rotulação original A–E: **[nao verificado]**. Execução do MORDE, RED/GREEN histórico e comportamento em produção: **[nao verificado]**.

| Achado | A cura fecha? |
|---|---|
| **A — dois locais com mesmo patch-id** | **Sim, para esse defeito.** Cada pid conserva todos os SHAs; a inversão inclui cada local. A busca percorre também todos os publicados daquele pid até encontrar linhas iguais. (`bin/tests/test_lei_akita.sh:389`, `bin/tests/test_lei_akita.sh:403`, `bin/tests/test_lei_akita.sh:406`) |
| **B — whitespace semântico dispensado** | **Fecha o exemplo `ab` versus `a b`; não fecha equivalência de negócio nem igualdade byte a byte.** Agora exige igualdade das listas filtradas, mas descarta contexto e delimitadores de linha. (`bin/tests/test_lei_akita.sh:358`, `bin/tests/test_lei_akita.sh:394`, `bin/tests/test_lei_akita.sh:406`) |
| **C — prosa/nome inteiro** | **Parcial.** Retira a prosa parentética da célula inteira; nomes internos curtos continuam entrando separadamente. Porém a célula de `sinal derivado…` mantém vírgula e continua impossível no campo. O inteiro literal `ADR (Architecture Decision Record)` também deixa de ser acrescentado por esse caminho. (`bin/tests/test_lei_akita.sh:114`, `bin/tests/test_lei_akita.sh:125`, `bin/tests/test_lei_akita.sh:319`; `app/docs/GLOSSARIO.md:13`, `app/docs/GLOSSARIO.md:28`) |
| **D — `split()`/saída malformada** | **Sim, para quantidade e formato dos campos:** exige dois hexadecimais minúsculos de 40 caracteres e emite `PADRAO_ERRO`. Não verifica completude, pertencimento dos SHAs à seleção ou stdout vazio com sucesso. (`bin/tests/test_lei_akita.sh:385`, `bin/tests/test_lei_akita.sh:387`, `bin/tests/test_lei_akita.sh:390`) |
| **E — contagem/texto final** | **Parcial.** O MORDE passa a proteger a exclusão de duplicatas em `JULGADOS`; a frase final continua sem explicar essa exclusão nem que o denominador inclui commits sem linha. (`bin/tests/test_lei_akita.sh:397`, `bin/tests/test_lei_akita.sh:409`, `bin/tests/test_lei_akita.sh:415`, `bin/tests/test_lei_akita.sh:820`, `bin/tests/test_lei_akita.sh:856`) |

**1) Existe local dispensado sem publicado de linhas iguais?** [nao verificado]

**Pelo caminho de duplicata, não.** O único `continue` dessa dispensa depende de `pub`, retornado apenas entre candidatos publicados do mesmo pid cuja lista de linhas seja igual. Local sem associação não encontra candidato e segue ao julgamento. Erro estrito de `git show` interrompe a execução; a ausência de `PADRAO_FIM` reprova no shell. (`bin/tests/test_lei_akita.sh:361`, `bin/tests/test_lei_akita.sh:406`, `bin/tests/test_lei_akita.sh:413`, `bin/tests/test_lei_akita.sh:848`)

**Literalmente, existe outro caminho:** commit enumerado sem linha `LEI-AKITA:` é ignorado antes de consultar publicado. Isso permanece explícito no contrato de `julgar`. (`bin/tests/test_lei_akita.sh:338`, `bin/tests/test_lei_akita.sh:409`)

“Publicado” significa alcançável de `origin/main`, após a exclusão pelo piso e sem merges; não há verificação de julgamento anterior daquele SHA. Portanto, `ja julgado` expressa uma premissa do instrumento. (`bin/tests/test_lei_akita.sh:373`, `bin/tests/test_lei_akita.sh:380`, `bin/tests/test_lei_akita.sh:402`, `bin/tests/test_lei_akita.sh:847`)

**2) Há contraexemplos e bordas restantes?**

**Sim, há falso positivo de negócio.** Contraexemplo construído: duas funções do mesmo arquivo contêm blocos suficientemente longos com contexto idêntico; o publicado troca `return 1` por `return 0` na função de pagamento, enquanto o local faz a mesma troca na função de simulação. Diferindo somente posição e identificação da função no cabeçalho do hunk, podem coincidir o patch-id e todas as linhas filtradas. O comparador não conserva a localização da alteração. Ocorrência nos commits reais: **[nao verificado]**. (`bin/tests/test_lei_akita.sh:380`, `bin/tests/test_lei_akita.sh:394`, `bin/tests/test_lei_akita.sh:406`)

**Também há falso negativo para cherry-pick sobre outra base.** Se a base alterar uma linha de contexto, mantendo exatamente as mesmas remoções e adições, o patch-id pode mudar. Nesse caso, o comparador tolerante a contexto nunca recebe o publicado como candidato. A promessa do comentário de tolerar contexto diferente fica condicionada à igualdade prévia do pid. (`bin/tests/test_lei_akita.sh:381`, `bin/tests/test_lei_akita.sh:393`, `bin/tests/test_lei_akita.sh:406`)

**“Byte a byte” é excessivo.** `text=True` e `splitlines()` normalizam/removem delimitadores; a marca `\ No newline at end of file` também é descartada. Assim, `old\n → new\n` e `old\n → new` podem produzir a mesma lista e o mesmo pid, apesar dos bytes finais diferentes. Consequência de negócio em produção: **[nao verificado]**. (`bin/tests/test_lei_akita.sh:358`, `bin/tests/test_lei_akita.sh:392`, `bin/tests/test_lei_akita.sh:394`)

| Borda | Comportamento |
|---|---|
| **Merge** | Excluído do cálculo de pid e impedido de usar a dispensa; com linha, incrementa `JULGADOS` e passa pelo julgamento dos hunks próprios. (`bin/tests/test_lei_akita.sh:380`, `bin/tests/test_lei_akita.sh:412`, `bin/tests/test_lei_akita.sh:416`, `bin/tests/test_lei_akita.sh:422`) |
| **Vazio** | Sem registro de pid, não encontra publicado. Com linha, é contado e julgado com lista vazia de caminhos: termo inventado reprova; `n/a (instrumento)` pode passar. Saída efetiva do Git de produção: **[nao verificado]**. (`bin/tests/test_lei_akita.sh:319`, `bin/tests/test_lei_akita.sh:325`, `bin/tests/test_lei_akita.sh:333`, `bin/tests/test_lei_akita.sh:406`, `bin/tests/test_lei_akita.sh:430`) |
| **Binário** | Quando o diff só contém cabeçalhos e indicação de binário, a lista filtrada fica vazia; a distinção fica a cargo do patch-id. Não foi acrescentada comparação própria de blobs. Não se segue disso que binários diferentes sejam automaticamente duplicatas. Configuração/saída em produção: **[nao verificado]**. (`bin/tests/test_lei_akita.sh:380`, `bin/tests/test_lei_akita.sh:394`, `bin/tests/test_lei_akita.sh:406`) |

**3) O MORDE morde cada cura?**

As mordidas abaixo são deduzidas das asserções; execução efetiva dos mutantes: **[nao verificado]**.

| Cura | Mutante que pega | Mutante que não pega |
|---|---|---|
| **A — mapa** | Voltar a guardar um único local por pid: exige `dup2` **e** `dup3` no mapa exato. (`bin/tests/test_lei_akita.sh:811`, `bin/tests/test_lei_akita.sh:813`, `bin/tests/test_lei_akita.sh:819`, `bin/tests/test_lei_akita.sh:820`) | Consultar somente o primeiro publicado do pid: não planta dois candidatos publicados, sendo apenas o segundo exato. (`bin/tests/test_lei_akita.sh:797`, `bin/tests/test_lei_akita.sh:799`) |
| **B — linhas** | Retirar a comparação: `a b` local seria dispensado contra `ab` publicado, contrariando as falhas esperadas. (`bin/tests/test_lei_akita.sh:799`, `bin/tests/test_lei_akita.sh:814`, `bin/tests/test_lei_akita.sh:820`) | Comparar o diff inteiro, incluindo contexto: não planta cherry-pick com contexto diferente; tampouco cobre posição semântica ou LF final. (`bin/tests/test_lei_akita.sh:797`, `bin/tests/test_lei_akita.sh:811`, `bin/tests/test_lei_akita.sh:813`) |
| **C — prosa** | As provas existentes pegam retirar o inteiro sem parênteses e acrescentar o inteiro da segunda coluna. **Não há mordida específica da cura de prosa.** (`bin/tests/test_lei_akita.sh:445`, `bin/tests/test_lei_akita.sh:456`) | Reintroduzir a inclusão da célula com prosa parentética: a fixture não contém esse caso. Vírgulas também não são exercitadas. (`bin/tests/test_lei_akita.sh:445`, `bin/tests/test_lei_akita.sh:446`, `bin/tests/test_lei_akita.sh:459`) |
| **D — saída** | **Não há mordida específica nova:** não injeta stdout malformado de `patch-id`. (`bin/tests/test_lei_akita.sh:381`, `bin/tests/test_lei_akita.sh:795`) | Remover a validação nova mantendo o processamento da saída saudável. (`bin/tests/test_lei_akita.sh:387`, `bin/tests/test_lei_akita.sh:389`) |
| **E — contagem** | Contar duplicatas em `JULGADOS`: exige exatamente três, correspondentes à remoção, ao whitespace novo e ao outro patch. (`bin/tests/test_lei_akita.sh:812`, `bin/tests/test_lei_akita.sh:814`, `bin/tests/test_lei_akita.sh:815`, `bin/tests/test_lei_akita.sh:820`) | Alterar ou manter a descrição humana enganosa: o MORDE termina antes da emissão final. (`bin/tests/test_lei_akita.sh:835`, `bin/tests/test_lei_akita.sh:856`) |

O cenário também pega retirada completa da dispensa, dispensa indiscriminada e associação publicada errada, pois exige simultaneamente falhas e mapa de duplicatas exatos. (`bin/tests/test_lei_akita.sh:817`, `bin/tests/test_lei_akita.sh:819`, `bin/tests/test_lei_akita.sh:820`)

**4) Quem escreve/lê e o texto continua verdadeiro?**

| Informação | Escritores e leitores |
|---|---|
| **`DUPLICATAS`** | Zerada por chamada; recebe local8/publicado12; lida pelo MORDE e pela emissão normal; o shell transforma o registro em `ja julgado:`. (`bin/tests/test_lei_akita.sh:376`, `bin/tests/test_lei_akita.sh:414`, `bin/tests/test_lei_akita.sh:818`, `bin/tests/test_lei_akita.sh:839`, `bin/tests/test_lei_akita.sh:847`) |
| **`JULGADOS`** | Zerado com a enumeração; incrementado após os dois filtros — sem linha e duplicata; lido pelo MORDE, emitido em `PADRAO_JULGADOS` e interpolado na frase final. (`bin/tests/test_lei_akita.sh:397`, `bin/tests/test_lei_akita.sh:409`, `bin/tests/test_lei_akita.sh:416`, `bin/tests/test_lei_akita.sh:820`, `bin/tests/test_lei_akita.sh:841`, `bin/tests/test_lei_akita.sh:856`) |

O `pre-push` usa o código de saída; a régua também usa o código e exibe a saída quando há falha. Esses leitores não interpretam as contagens. Leitores externos: **[nao verificado]**. (`bin/pre-push.sh:38`; `bin/regua.sh:140`, `bin/regua.sh:142`)

**O número de julgados é coerente; a descrição final continua incompleta.** Em `N de M`, **N** conta commits com linha que chegaram ao julgamento, excluindo duplicatas; **M** conta todos os enumerados, inclusive sem linha e duplicatas. A frase seria precisa como: “padrão julgado em N commits; M enumerados; D duplicatas dispensadas”. (`bin/tests/test_lei_akita.sh:397`, `bin/tests/test_lei_akita.sh:409`, `bin/tests/test_lei_akita.sh:414`, `bin/tests/test_lei_akita.sh:416`, `bin/tests/test_lei_akita.sh:856`)

## 12/11 09:47 · sessao 017bd36f7dcb · - · - · origem=Ronald (tmux)

- **Nenhuma dispensa por duplicata sem comparação igual:** o único `continue` de duplicata exige `pub`, obtido entre publicados do mesmo patch-id com lista `+/-` igual. A associação local preserva todos os SHAs. (`bin/tests/test_lei_akita.sh:389`, `bin/tests/test_lei_akita.sh:403`, `bin/tests/test_lei_akita.sh:406`, `bin/tests/test_lei_akita.sh:413`) **Literalmente**, local sem `LEI-AKITA:` continua não julgado sem exigir publicado. (`bin/tests/test_lei_akita.sh:409`)

- **Falso negativo por base diferente:** a comparação tolera contexto/posição diferentes, mas só é alcançada após igualdade de patch-id. Portanto, alteração de contexto que mude o patch-id impede reconhecer um cherry-pick com as mesmas linhas adicionadas/removidas; reprodução nessa configuração Git é **[nao verificado]**. (`bin/tests/test_lei_akita.sh:380`, `bin/tests/test_lei_akita.sh:394`, `bin/tests/test_lei_akita.sh:406`)

- **“Byte a byte” é excessivo:** o subprocesso usa `text=True`, e o comparador usa `splitlines()`; também descarta a marca `\ No newline at end of file`. (`bin/tests/test_lei_akita.sh:358`, `bin/tests/test_lei_akita.sh:392`, `bin/tests/test_lei_akita.sh:394`) Contraexemplo candidato: publicado altera `old\n` para `new\n`; local altera `old\n` para `new` sem LF final. As listas são iguais; se o patch-id ignorar a marca, haverá dispensa apesar dos bytes diferentes. Igualdade dos patch-ids sem execução: **[nao verificado]**.

- **Merge:** excluído do cálculo por `--no-merges`, bloqueado explicitamente no bypass e contado/julgado pelos hunks próprios. (`bin/tests/test_lei_akita.sh:380`, `bin/tests/test_lei_akita.sh:412`, `bin/tests/test_lei_akita.sh:416`, `bin/tests/test_lei_akita.sh:422`)

- **Vazio:** sem registro de patch-id, não há candidato publicado; com linha, segue para julgamento e incrementa `JULGADOS`. A ausência efetiva de registro depende da saída do Git **[nao verificado]**. (`bin/tests/test_lei_akita.sh:403`, `bin/tests/test_lei_akita.sh:406`, `bin/tests/test_lei_akita.sh:416`, `bin/tests/test_lei_akita.sh:430`)

- **Binário:** se `show` emitir apenas cabeçalhos/`Binary files...`, a lista será vazia; a proteção contra binários diferentes permanece no patch-id, sem comparação própria de blobs. A saída/configuração efetiva de produção é **[nao verificado]**. (`bin/tests/test_lei_akita.sh:380`, `bin/tests/test_lei_akita.sh:394`, `bin/tests/test_lei_akita.sh:406`)

- **Lacuna do MORDE:** o gerador pesquisa todos os publicados do mesmo pid, corrigindo também “primeiro publicado tem whitespace diferente; segundo é exato”; o caso plantado tem apenas um publicado por pid e não prova essa busca além do primeiro. (`bin/tests/test_lei_akita.sh:406`, `bin/tests/test_lei_akita.sh:797`, `bin/tests/test_lei_akita.sh:799`, `bin/tests/test_lei_akita.sh:820`)

## 12/11 09:47 · sessao 022365898ec8 · - · - · origem=Ronald (tmux)

Leitura estática, sem execução do MORDE; RED/GREEN efetivo permanece **[nao verificado]**.

| Cura | Mutante mordido pelo MORDE | Mutante/lacuna que escapa |
|---|---|---|
| Dois locais por patch-id | Voltar a conservar apenas um SHA: `dup2` e `dup3` usam o mesmo conteúdo/caminho, e a asserção exige ambos no mapa exato. (`bin/tests/test_lei_akita.sh:811`, `bin/tests/test_lei_akita.sh:813`, `bin/tests/test_lei_akita.sh:819`, `bin/tests/test_lei_akita.sh:820`) | Procurar somente o primeiro publicado do mesmo pid: o cenário contém apenas um publicado equivalente a `dup`, sem candidato anterior de linhas diferentes. (`bin/tests/test_lei_akita.sh:797`, `bin/tests/test_lei_akita.sh:799`, `bin/tests/test_lei_akita.sh:819`) |
| Whitespace semântico | Retirar comparação de linhas: publicado contém `ab`, local contém `a b`, e local deve permanecer entre falhas. (`bin/tests/test_lei_akita.sh:799`, `bin/tests/test_lei_akita.sh:814`, `bin/tests/test_lei_akita.sh:820`) | Comparar diff inteiro, incluindo contexto: a prova nova só recria arquivo `dup.py` com conteúdo idêntico; não planta cherry-pick com contexto alterado. (`bin/tests/test_lei_akita.sh:797`, `bin/tests/test_lei_akita.sh:811`, `bin/tests/test_lei_akita.sh:813`) |
| Prosa em parênteses | **Não há mordida específica nova.** A fixture de vocabulário usa célula sem parênteses; casos de `julgar` tampouco usam prosa do glossário real. (`bin/tests/test_lei_akita.sh:445`, `bin/tests/test_lei_akita.sh:446`, `bin/tests/test_lei_akita.sh:459`, `bin/tests/test_lei_akita.sh:482`) | Reintroduzir o loop v1 que também adicionava a célula inteira com prosa preservaria as asserções existentes de inteiro/pedaços/segunda coluna. (`bin/tests/test_lei_akita.sh:456`, `bin/tests/test_lei_akita.sh:457`) |
| Saída malformada patch-id | **Não há injeção específica de stdout malformado.** Os erros plantados são repositório raso, falta de origin e falta de corte. (`bin/tests/test_lei_akita.sh:715`, `bin/tests/test_lei_akita.sh:720`, `bin/tests/test_lei_akita.sh:829`) | Retirar a validação nova de quantidade/formato de campos não é exercitado por esses preparos saudáveis de patch-id. (`bin/tests/test_lei_akita.sh:385`, `bin/tests/test_lei_akita.sh:387`, `bin/tests/test_lei_akita.sh:795`) |
| Contagem | Contar duplicatas em `JULGADOS`: esperado é exatamente 3, correspondentes a remoção, whitespace novo e outro patch; as duas duplicatas ficam de fora. (`bin/tests/test_lei_akita.sh:812`, `bin/tests/test_lei_akita.sh:814`, `bin/tests/test_lei_akita.sh:815`, `bin/tests/test_lei_akita.sh:820`) | Alterar somente a frase final para continuar sem explicar a exclusão não muda o valor validado pelo MORDE; a frase é emitida fora da execução MORDE. (`bin/tests/test_lei_akita.sh:834`, `bin/tests/test_lei_akita.sh:835`, `bin/tests/test_lei_akita.sh:856`) |

O cenário também morde dispensa indiscriminada, retirada completa da dispensa e associação publicada errada, pois exige simultaneamente conjunto exato de falhas e mapa exato de duplicatas. (`bin/tests/test_lei_akita.sh:817`, `bin/tests/test_lei_akita.sh:819`, `bin/tests/test_lei_akita.sh:820`)

A cura de prosa retira os parênteses antes de adicionar a célula inteira; `Architecture Conformance Test (armadilha: ...)` e `Offline Fallback (network-first: ...)` deixam de entrar com prosa. O termo `Architecture Decision Record` ainda entra isoladamente pelo filtro de nome entre parênteses. (`bin/tests/test_lei_akita.sh:84`, `bin/tests/test_lei_akita.sh:114`, `bin/tests/test_lei_akita.sh:125`; `app/docs/GLOSSARIO.md:15`, `app/docs/GLOSSARIO.md:28`, `app/docs/GLOSSARIO.md:31`)

Vírgula continua aberta: a célula `sinal derivado do estado, reconciliacao level-triggered` ainda entra inteira, mas o parser de `padrao=` não admite vírgula no valor. Não existe teste dessa célula na fixture nova. (`app/docs/GLOSSARIO.md:13`; `bin/tests/test_lei_akita.sh:125`, `bin/tests/test_lei_akita.sh:127`, `bin/tests/test_lei_akita.sh:319`, `bin/tests/test_lei_akita.sh:445`)

## 12/11 09:42 · sessao 1644ac242a95 · PUSH-SEM-REGRAVAR · verificador · origem=Code

A pré-verificação encontra um defeito concreto em (a): **dois locais com o mesmo patch-id perdem uma das associações**, podendo deixar uma duplicata sujeita a reprovação. Em (b), há ampliação para prosa e nomes inteiros que entram no vocabulário, mas continuam impossíveis de usar em `padrao=` por conterem vírgulas. (`bin/tests/test_lei_akita.sh:385`, `bin/tests/test_lei_akita.sh:395`, `bin/tests/test_lei_akita.sh:123`, `bin/tests/test_lei_akita.sh:317`)

Não executei o MORDE completo, pois ele cria arquivos e repositórios temporários; os resultados históricos RED/GREEN e “4/4 mutantes” permanecem **[não verificado]**. (`bin/tests/test_lei_akita.sh:429`, `bin/tests/test_lei_akita.sh:432`, `bin/tests/test_lei_akita.sh:642`)

1. **Aval versus implementação, por item.**

   **(a)** O aval transcrito pede dispensar o commit cujo patch-id coincide com um publicado e emitir `DUPLICATA de <sha publicado>`; a transcrição não está disponível como arquivo local **[não verificado em arquivo]**. O código implementa essa dispensa antes de `julgar`, mas somente para commits com linha `LEI-AKITA:`, com até um pai e presentes no mapa local; merges continuam pelo hunk próprio. Portanto, não cobre literalmente todo commit duplicado, e a perda de associações no mapa impede reconhecer todas as duplicatas elegíveis. (`bin/tests/test_lei_akita.sh:398`, `bin/tests/test_lei_akita.sh:401`, `bin/tests/test_lei_akita.sh:410`, `bin/tests/test_lei_akita.sh:385`)

   A saída humana fica `ja julgado: <local8> DUPLICATA de <publicado12>`, e a duplicata deixa de incrementar `JULGADOS`. (`bin/tests/test_lei_akita.sh:402`, `bin/tests/test_lei_akita.sh:404`, `bin/tests/test_lei_akita.sh:815`, `bin/tests/test_lei_akita.sh:822`)

   **(b)** O aval transcrito pede o nome inteiro da primeira coluna, além dos pedaços **[não verificado em arquivo]**. O código acrescenta tanto a célula inteira quanto sua versão sem parênteses, preservando a extração anterior das duas primeiras colunas; não acrescenta a segunda coluna inteira. Entretanto, o parser do campo ainda impede os nomes com vírgulas. (`bin/tests/test_lei_akita.sh:98`, `bin/tests/test_lei_akita.sh:117`, `bin/tests/test_lei_akita.sh:123`, `bin/tests/test_lei_akita.sh:317`)

   Os anexos também divergem no testemunho de `7313a031c`: o aval cita `072556ec8`, enquanto a mensagem do commit cita `4aa0a00fb`. Se ambos são publicados equivalentes, **[não verificado]**; o código escolhe o primeiro publicado encontrado para cada patch-id. (`bin/tests/test_lei_akita.sh:385`)

2. **Quando conteúdo diferente pode receber o mesmo patch-id.**

   A tabela descreve a semântica do comando escolhido; sua reprodução na versão/configuração Git de produção é **[não verificado]**. A chamada efetiva usa `log -p --no-merges --no-renames` seguido de `patch-id --stable`. (`bin/tests/test_lei_akita.sh:378`, `bin/tests/test_lei_akita.sh:379`)

   | Caso | Consequência |
   |---|---|
   | Whitespace | É ignorado no cálculo: diferenças de indentação ou espaços dentro de strings podem preservar o patch-id e mudar comportamento. O bypass não faz comparação adicional de árvore, blob ou AST. (`bin/tests/test_lei_akita.sh:379`, `bin/tests/test_lei_akita.sh:401`) |
   | Base/contexto | Patch-id identifica o patch normalizado, não a árvore resultante nem sua equivalência semântica em outra base; o código aceita somente essa identidade. (`bin/tests/test_lei_akita.sh:379`, `bin/tests/test_lei_akita.sh:401`) |
   | Modo de arquivo | Cabeçalhos de mudança de modo participam do patch; “só modo” pode produzir patch-id e ser duplicata. Não é automaticamente vazio. (`bin/tests/test_lei_akita.sh:378`, `bin/tests/test_lei_akita.sh:401`) |
   | Binário | Não se pode afirmar que todo binário recebe o mesmo ID: a representação binária incorpora identificação dos blobs. Não há prova dessa borda no MORDE novo. (`bin/tests/test_lei_akita.sh:378`, `bin/tests/test_lei_akita.sh:778`; comportamento em produção **[não verificado]**) |
   | Rename | `--no-renames` apresenta remoção e adição, incluindo caminhos e conteúdo; o bypass não se baseia apenas no rótulo “rename”. (`bin/tests/test_lei_akita.sh:378`) |
   | Commit vazio | Não produz registro de patch-id; portanto, não recebe um “ID vazio comum” que o torne duplicata. Sem associação local, segue o julgamento normal se tiver linha. (`bin/tests/test_lei_akita.sh:383`, `bin/tests/test_lei_akita.sh:398`, `bin/tests/test_lei_akita.sh:401`) |

   **Há possibilidade de dispensar código de negócio novo sem julgá-lo:** por exemplo, patches que diferem apenas entre `return 'a b'` e `return 'ab'`. Isso decorre do critério de equivalência escolhido, além do defeito do mapa; sua ocorrência nos commits fornecidos é **[não verificado]**. (`bin/tests/test_lei_akita.sh:379`, `bin/tests/test_lei_akita.sh:403`)

   Um vazio com linha e termo inexistente continua vermelho; vazio com `n/a (instrumento)` pode passar por não tocar negócio. Um commit só de modo, sem correspondência publicada, continua sujeito ao julgamento dos caminhos alterados e pode reprovar `n/a` em `app/`. (`bin/tests/test_lei_akita.sh:323`, `bin/tests/test_lei_akita.sh:331`, `bin/tests/test_lei_akita.sh:415`, `bin/tests/test_lei_akita.sh:418`)

3. **Publicados, erros e `split()`.** [nao verificado]

   Os publicados são os commits não merge alcançáveis de `origin/main`, excluída a ancestralidade do pai do corte; é um limite de grafo, não uma data. Não há restrição a primeiro pai nem checagem de que o publicado teve efetivamente julgamento anterior. (`bin/tests/test_lei_akita.sh:371`, `bin/tests/test_lei_akita.sh:378`, `bin/tests/test_lei_akita.sh:394`)

   `origin/main` ausente provoca erro estrito na enumeração; repositório raso é recusado explicitamente; retorno não zero de `patch-id` produz `PADRAO_ERRO`. Esses caminhos não viram conjunto vazio silencioso. (`bin/tests/test_lei_akita.sh:359`, `bin/tests/test_lei_akita.sh:363`, `bin/tests/test_lei_akita.sh:380`, `bin/tests/test_lei_akita.sh:388`)

   **Há conjuntos vazios legítimos:** nenhum local enumerado; nenhum publicado restante após o piso; ou nenhum patch emitido, como numa seleção só de vazios. O código também aceita stdout vazio com retorno zero sem validar sua completude. (`bin/tests/test_lei_akita.sh:383`, `bin/tests/test_lei_akita.sh:394`, `bin/tests/test_lei_akita.sh:395`)

   Na saída normal, cada registro tem dois campos; commit vazio simplesmente não gera registro. Mas `pid, sha = ln.split()` lança `ValueError` diante de linha com quantidade diferente de campos, sem convertê-lo em `PADRAO_ERRO`; dois campos inválidos também não são validados. A ocorrência dessa saída anormal é **[não verificado]**. (`bin/tests/test_lei_akita.sh:383`, `bin/tests/test_lei_akita.sh:384`)

   Exceções de execução ou decodificação dos subprocessos também não têm tratamento geral. Ainda assim, uma interrupção antes do fim deixa faltar `PADRAO_FIM`, e o shell reprova: diagnóstico fora do padrão, mas não verde por esse caminho. (`bin/tests/test_lei_akita.sh:356`, `bin/tests/test_lei_akita.sh:379`, `bin/tests/test_lei_akita.sh:817`, `bin/tests/test_lei_akita.sh:823`)

4. **Locais, merges, raízes e mapa.**

   `--no-walk=unsorted` seleciona os SHAs fornecidos sem percorrer seus ancestrais; `--no-merges` exclui merges do cálculo. Os merges permanecem em `hs` e seguem o julgamento de hunks próprios. (`bin/tests/test_lei_akita.sh:378`, `bin/tests/test_lei_akita.sh:388`, `bin/tests/test_lei_akita.sh:395`, `bin/tests/test_lei_akita.sh:405`)

   Raízes são elegíveis porque a condição é `len(pais) <= 1`. Na configuração usual, `log -p` mostra sua criação contra a árvore vazia; a configuração efetiva, inclusive `log.showRoot`, é **[não verificado]**. O julgamento posterior possui tratamento explícito de raiz. (`bin/tests/test_lei_akita.sh:401`, `bin/tests/test_lei_akita.sh:416`)

   **O mapa fica errado para dois locais com o mesmo patch-id:** `setdefault(pid, sha)` conserva apenas o primeiro SHA; inverter esse dicionário não recupera o outro. Um local omitido será julgado mesmo sendo duplicata publicada. (`bin/tests/test_lei_akita.sh:385`, `bin/tests/test_lei_akita.sh:395`, `bin/tests/test_lei_akita.sh:401`)

5. **Termos novos do GLOSSÁRIO real.**

   São **dez entradas novas no conjunto**, provenientes das nove células abaixo; somente sete são utilizáveis pelo parser atual de `padrao=`. A normalização remove marcação, converte para minúsculas e compacta espaços. (`bin/tests/test_lei_akita.sh:83`, `bin/tests/test_lei_akita.sh:123`, `bin/tests/test_lei_akita.sh:317`)

   | Célula que ganha aceite integral | Uso no campo |
   |---|---|
   | `Single Source of Truth / Aggregate Root` | Aceito. (`app/docs/GLOSSARIO.md:11`) |
   | `sinal derivado do estado, reconciliacao level-triggered (o oposto, edge-triggered, e o fio morto)` | Entra inteiro **e sem o parêntese**, mas ambas as entradas contêm vírgula e são recusadas pelo parser. (`app/docs/GLOSSARIO.md:13`; `bin/tests/test_lei_akita.sh:123`, `bin/tests/test_lei_akita.sh:317`) |
   | `Architecture Conformance Test (armadilha: selar a FORMA e nao o SIGNIFICADO)` | Aceito, incluindo a prosa. (`app/docs/GLOSSARIO.md:15`; `bin/tests/test_lei_akita.sh:331`) |
   | `Parallel Change / Expand-Contract` | Aceito. (`app/docs/GLOSSARIO.md:18`) |
   | `Shadow mode / Parallel run` | Aceito. (`app/docs/GLOSSARIO.md:21`) |
   | `Backfill / Reprocessing` | Aceito. (`app/docs/GLOSSARIO.md:23`) |
   | `Duplicated business logic / logic drift` | Aceito. (`app/docs/GLOSSARIO.md:25`) |
   | `ADR (Architecture Decision Record)` | Aceito; o inteiro é novo, embora seus componentes já fossem extraídos. (`app/docs/GLOSSARIO.md:28`; `bin/tests/test_lei_akita.sh:114`, `bin/tests/test_lei_akita.sh:123`) |
   | `Offline Fallback (network-first: sem rede, a copia do cache; sem copia, o erro de rede de sempre -- nunca undefined no respondWith)` | Entra no conjunto após normalização, mas suas vírgulas impedem uso no campo. (`app/docs/GLOSSARIO.md:31`; `bin/tests/test_lei_akita.sh:83`, `bin/tests/test_lei_akita.sh:317`) |

   Portanto, **sim, prosa passa a ser aceita integralmente** no caso de `Architecture Conformance Test`. O filtro antigo para parênteses que parecem nomes não é aplicado pelo loop novo à célula inteira. (`app/docs/GLOSSARIO.md:15`; `bin/tests/test_lei_akita.sh:88`, `bin/tests/test_lei_akita.sh:123`)

6. **Quem escreve e quem lê.**

   | Informação | Escrita e leitura |
   |---|---|
   | `DUPLICATAS` | Zerada por chamada, preenchida com local8/publicado12; lida pelo MORDE novo e pela emissão normal. (`bin/tests/test_lei_akita.sh:374`, `bin/tests/test_lei_akita.sh:402`, `bin/tests/test_lei_akita.sh:794`, `bin/tests/test_lei_akita.sh:814`) |
   | `JULGADOS` | Inicializada na enumeração e incrementada depois da exclusão de duplicatas; emitida em `PADRAO_JULGADOS`. (`bin/tests/test_lei_akita.sh:389`, `bin/tests/test_lei_akita.sh:404`, `bin/tests/test_lei_akita.sh:816`) |
   | `ENUMERADOS` | Total de `hs`, incluindo sem linha e duplicatas; lida na saída e nas asserções de sincronização, outra ref e piso. (`bin/tests/test_lei_akita.sh:389`, `bin/tests/test_lei_akita.sh:676`, `bin/tests/test_lei_akita.sh:701`, `bin/tests/test_lei_akita.sh:802`, `bin/tests/test_lei_akita.sh:816`) |
   | `PADRAO_FALHA` | Python emite; shell conta e transforma em `FALHA: PADRAO-NA-CURA`. (`bin/tests/test_lei_akita.sh:813`, `bin/tests/test_lei_akita.sh:820`, `bin/tests/test_lei_akita.sh:821`) |
   | `PADRAO_DUPLICATA` | Python emite; shell transforma em `ja julgado:`. (`bin/tests/test_lei_akita.sh:815`, `bin/tests/test_lei_akita.sh:822`) |
   | `PADRAO_JULGADOS` | Consumida pelo próprio selo para compor a frase final de sucesso. (`bin/tests/test_lei_akita.sh:816`, `bin/tests/test_lei_akita.sh:831`) |
   | `PADRAO_FIM` / `PADRAO_ERRO` | O sentinela é exigido; não há parser dedicado de `PADRAO_ERRO`, cuja interrupção provoca ausência do sentinela. (`bin/tests/test_lei_akita.sh:819`, `bin/tests/test_lei_akita.sh:823`) |

   `pre-push.sh` passa o SHA e decide pelo código de saída; `regua.sh` também decide pelo código de saída e apenas exibe a saída quando há falha. A adição da linha de duplicata não quebra esses leitores. (`bin/pre-push.sh:38`; `bin/regua.sh:140`, `bin/regua.sh:142`)

   Outro selo que grepe essa saída não foi encontrado na varredura da cópia; leitor externo, ignorado ou ausente da cópia é **[não verificado]**.

7. **MORDE e mutantes.**

   O caso sintético de vocabulário morde retirar o nome inteiro e acrescentar também o inteiro da segunda coluna; os dois casos de `julgar` mordem negar `Parallel Change / Expand-Contract` e aceitar uma composição inventada só porque contém um pedaço válido. (`bin/tests/test_lei_akita.sh:444`, `bin/tests/test_lei_akita.sh:449`, `bin/tests/test_lei_akita.sh:450`, `bin/tests/test_lei_akita.sh:471`)

   O caso de duplicata morde retirar a dispensa, dispensar indiscriminadamente o outro patch ou emitir associação publicada errada naquele cenário. Contudo, o patch diferente está em **outro arquivo**, e existe apenas um local com o patch-id publicado. (`bin/tests/test_lei_akita.sh:783`, `bin/tests/test_lei_akita.sh:789`, `bin/tests/test_lei_akita.sh:791`, `bin/tests/test_lei_akita.sh:795`)

   Permanecem lacunas concretas: dois locais com mesmo patch-id — inclusive o defeito atual —; mudança diferente no mesmo caminho; whitespace semântico; vazio/modo/binário/raiz; erro ou saída malformada de `patch-id`; prosa e vírgulas nas células. Esses casos não são plantados pela prova nova. (`bin/tests/test_lei_akita.sh:433`, `bin/tests/test_lei_akita.sh:778`)

   Também sobreviveria à asserção nova um mutante que continuasse contando duplicatas em `JULGADOS`, porque ela verifica somente falhas e associação da duplicata. Um mutante que devolvesse `{}` apenas quando `patch-id` falha não seria exercitado pelo cenário saudável; sua execução efetiva é **[não verificado]**. (`bin/tests/test_lei_akita.sh:380`, `bin/tests/test_lei_akita.sh:795`)

8. **As três classes de reprovação citadas.**

   **Selo/RED sem mordida:** as asserções novas têm ligação com os comportamentos pretendidos, mas não provam todas as bordas; o defeito de múltiplos locais passa pela prova nova. O RED histórico alegado continua **[não verificado]**. (`bin/tests/test_lei_akita.sh:444`, `bin/tests/test_lei_akita.sh:795`, `bin/tests/test_lei_akita.sh:385`)

   **Erro operacional virando verde:** não encontrei esse caminho nas falhas explícitas examinadas; ausência de `PADRAO_FIM` reprova, e `MORDE_OK` sozinho não basta quando existe `MORDE_FALHOU`. Há, porém, exceções fora de `PADRAO_ERRO` e stdout vazio com sucesso sem validação adicional. (`bin/tests/test_lei_akita.sh:383`, `bin/tests/test_lei_akita.sh:823`, `bin/tests/test_lei_akita.sh:827`)

   **Campo com dois significados:** o formato `N de M` permanece, mas `JULGADOS` agora exclui duplicatas com linha. Nenhum dos dois leitores operacionais identificados depende da contagem; o próprio texto final continua dizendo “commit(s) com linha LEI-AKITA”, sem explicar a exclusão, e o MORDE novo não protege essa semântica. (`bin/tests/test_lei_akita.sh:401`, `bin/tests/test_lei_akita.sh:795`, `bin/tests/test_lei_akita.sh:831`; `bin/pre-push.sh:38`; `bin/regua.sh:140`)

## 12/11 09:42 · sessao 42642e1bff0d · - · - · origem=Ronald (tmux)

6–9 são **R**, com origem no c10 `3fc096198`: ele retirou `grade_espelho_janela` do PDF e substituiu suas órfãs por `orfas_grade` da tela ([pdf_espelho.py:471](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/relatorios/pdf_espelho.py:471)). A tela prioriza `grade_da_celula`, que pode responder sem célula e sem órfãs ([espelho.py:522](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/ponto/services/espelho.py:522)); isso explica a marca perdida e o aberto contado.

Cura mínima sugerida: restaurar a leitura real do contrato no PDF e passar `recusas=None` opcional pelo caminho:

- `grade_espelho_janela` → dispatcher → builder por turno → `_marcar_ausencias` ([utils.py:2338](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/escala/utils.py:2338), [utils.py:2099](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/escala/utils.py:2099), [utils.py:1292](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/escala/utils.py:1292)).
- Preservar também os ramos legacy: chamada direta sem vínculo e `montar_grade_prevista` ([utils.py:1008](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/escala/utils.py:1008), [utils.py:1016](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/escala/utils.py:1016)).
- `_marcar_ausencias` entrega o destinatário à A2 e, antes de escolher faixa, transporta `erro.rotulo` no dia recusado; conserva células/órfãs e não conclui precedência ([utils.py:1209](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/escala/utils.py:1209)). Sem destinatário, a A2 continua levantando ([precedencia.py:299](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/ponto/precedencia.py:299)).

Há dois testes novos que selam a regressão e precisam caracterização curada: C10 proíbe explicitamente importar/chamar o contrato ([test_lastro_c10_sem_banco.py:324](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/ponto/tests/test_lastro_c10_sem_banco.py:324)); C11 exige órfã falsa/aberto 1 quando a tela recebe `sem_celula` ([test_lastro_c11_sem_banco.py:360](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/ponto/tests/test_lastro_c11_sem_banco.py:360)). Nenhum arquivo editado ou teste executado por este explorer. [nao verificado]

## 12/11 09:42 · sessao 6c6e0198f7a5 · FIO-E0-V4-E3-LEITORES · leitura · origem=Code

ACHADO 1 | CODIGO: confere em parte -- antes de descarimbar, `registrar(linhas_da_projecao_sem_linha(...))` persiste RESPOSTA e VALIDACAO históricas faltantes, identificadas por pergunta/ato/instante, conservando timestamps, texto e autor da validação (`app/chamados/models.py:1564`, `app/chamados/models.py:3932`, `app/chamados/models.py:3938`, `app/chamados/models.py:3954`). Depois limpa os campos sem acrescentar REABERTURA à sequência (`app/chamados/models.py:1567`); reabre a disputa e registra auditoria separada (`app/chamados/models.py:1576`, `app/chamados/models.py:1590`). Se VALIDACAO era o último ato e não existe posterior, **VALIDACAO continua por último**, pela ordenação ato_em/pk; isso não vale universalmente para toda ressuscitação (`app/chamados/models.py:3890`). | CENSO: NOVO; “0 escritores persistentes” trata do estado da tarefa, não dos seus insumos (`e3_censo_anexo.r:39`). | EFEITO: sim, condicional no código: a tarefa continua no ramo VALIDACAO, enquanto C6 pode passar a viva após a limpeza; as demais guardas continuam valendo (`app/chamados/services/tarefa_do_dia.py:141`, `app/chamados/juizes.py:1391`, `app/chamados/juizes.py:1466`). Ocorrência em produção [nao verificado].

ACHADO 2 | CODIGO: confere em parte -- o campo aceita NULL e o filtro por intervalo exclui essas perguntas da carga (`app/chamados/models.py:2424`, `app/chamados/services/tarefa_do_dia.py:374`). C6 pode considerá-las vivas, mas NULL não basta: validação, tranca, vias e célula têm precedência (`app/chamados/juizes.py:1391`, `app/chamados/juizes.py:1395`, `app/chamados/juizes.py:1438`, `app/chamados/juizes.py:1455`, `app/chamados/juizes.py:1466`). A abertura atual recusa data nula (`app/chamados/models.py:2788`). | CENSO: NOVO; a decisão 8 cobre conversas, não fatos sem data (`e3_censo_anexo.r:125`). | EFEITO: sim, condicional: C17 pode considerar a pergunta pela célula enquanto a tarefa não a carrega (`app/chamados/juizes.py:1571`, `app/chamados/juizes.py:1651`, `app/chamados/services/tarefa_do_dia.py:376`). Existência desse estado e desaparecimento após migração [nao verificado].

ACHADO 3 | CODIGO: confere em parte -- C17 agrupa pela célula; a tarefa, por colaborador/data_evento (`app/chamados/juizes.py:1574`, `app/chamados/services/tarefa_do_dia.py:380`). Porém `rechavear_data_evento` atualiza conjuntamente data e célula; o caminho citado não demonstra que o noturno produza desencontro (`app/chamados/services/fio.py:1035`, `app/chamados/services/fio.py:1038`, `app/chamados/services/fio.py:1044`). | CENSO: NOVO; não é um dos quatro conflitos enumerados (`e3_censo_anexo.r:113`). | EFEITO: sim, se houver desencontro e forem atendidas as demais condições: a pergunta participa de dias diferentes nos dois universos (`app/chamados/services/tarefa_do_dia.py:236`, `app/chamados/juizes.py:1649`). Incidência e invariante na frota [nao verificado].

ACHADO 4 | CODIGO: confere em parte -- `alimentar_portas` passa agora à fila, mas chama C6 sem instante; `fio_vivo` também, e a janela usa `date.today()` (`app/chamados/juizes.py:1566`, `app/chamados/juizes.py:1573`, `app/chamados/juizes.py:1503`, `app/chamados/juizes.py:592`). O DIFF avisa expressamente esse limite (`app/chamados/management/commands/diff_e3_sombra.py:105`). A alegação absoluta de irreprodutibilidade desconsidera que a sonda fornecida envolve a comparação em `freeze_time(agora)`; execução dessa sonda [nao verificado] (`e3_censo_anexo.r:392`). | CENSO: NOVO quanto ao defeito de propagação do relógio; o congelamento já consta da sonda (`e3_censo_anexo.r:392`). | EFEITO: sim, potencialmente na vitalidade e nas portas; a janela participa da decisão, enquanto a carga da tarefa fornece hoje/agora (`app/chamados/juizes.py:1455`, `app/chamados/juizes.py:1473`, `app/chamados/services/tarefa_do_dia.py:489`). Divergência efetiva na virada [nao verificado].

ACHADO 5 | CODIGO: confere em parte -- importa os privados e **exclui** dias com fio diferente de SEM_PORTA; os restantes viram candidatos à Pauta (`app/ponto/services/pautas_dos_discordantes.py:61`, `app/ponto/services/pautas_dos_discordantes.py:77`, `app/ponto/services/pautas_dos_discordantes.py:79`). A quebra ou permanência no juiz antigo não é necessária: `_fio_do_dia` chama `_porta`, cuja implementação pode mudar preservando o contrato (`app/folha/fora_da_folha.py:77`, `app/folha/fora_da_folha.py:67`). | CENSO: NOVO consumidor; o wrapper já estava listado (`e3_censo_anexo.r:64`). | EFEITO: nao sobre tarefa/C6/C17; esse caminho consome a resposta e escreve Pautas quando aplicado (`app/ponto/services/pautas_dos_discordantes.py:75`, `app/ponto/services/pautas_dos_discordantes.py:177`, `app/ponto/services/pautas_dos_discordantes.py:183`).

ACHADO 6 | CODIGO: confere -- `do_dia` seleciona órfãos com causa `fabrica_nao_rodou`; a porta chama a fábrica com notificação e relê o resultado (`app/chamados/services/cobrar_dia.py:45`, `app/chamados/services/cobrar_dia.py:46`, `app/chamados/services/cobrar_dia.py:49`, `app/chamados/services/cobrar_dia.py:53`). Mensagem/ok usam perguntas e chamados retornados; `do_dia` mantém sua seleção e retenção (`app/chamados/services/cobrar_dia.py:55`, `app/chamados/services/cobrar_dia.py:58`, `app/chamados/services/pergunta_no_app.py:217`, `app/chamados/services/pergunta_no_app.py:223`). | CENSO: NOVO chamador com escrita; a referência anterior cobre o copiloto (`e3_censo_anexo.r:68`). | EFEITO: sim, potencialmente: a fábrica abre perguntas persistidas que entram nas cargas da tarefa/C17 (`app/chamados/services/disputa_emissao.py:837`, `app/chamados/models.py:3018`, `app/chamados/services/tarefa_do_dia.py:374`, `app/chamados/juizes.py:1571`). Ocorrência em produção [nao verificado].

ACHADO 7 | CODIGO: confere -- `_material` prioriza texto não vazio, depois consulta `fio_vivo`, senão retorna “nada” (`app/folha/previa.py:142`, `app/folha/previa.py:144`, `app/folha/previa.py:146`); C15 delega a C6 sem instante explícito (`app/chamados/juizes.py:1503`). | CENSO: NOVO consumidor; a descrição geral de C15 não enumera esse leitor (`e3_censo_anexo.r:35`). | EFEITO: nao sobre as respostas dos três juízes; o caminho recebe perguntas e produz o rótulo material da prévia (`app/folha/previa.py:95`, `app/folha/previa.py:139`).

ACHADO 8 | CODIGO: confere -- o vigia considera canal pela existência de pergunta sem respondida_em ou chamado em VIVOS, sem filtro de silêncio (`app/chamados/services/furo_sem_canal.py:80`, `app/chamados/services/furo_sem_canal.py:82`, `app/chamados/services/furo_sem_canal.py:85`). VIVOS inclui registrado; COBRANCA não (`app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:29`). | CENSO: NOVO contador; o vigia enumerado é outro serviço (`e3_censo_anexo.r:66`). | EFEITO: nao sobre tarefa/C6/C17; altera a medição de “sem canal”, retornando contagens e amostra (`app/chamados/services/furo_sem_canal.py:89`). Divergência concreta em algum dia de produção [nao verificado].

ACHADO 9 | CODIGO: confere -- a fila exclui silêncio futuro; a lista de trabalho exige NULL, portanto também exclui silêncio vencido (`app/chamados/services/painel.py:190`, `app/chamados/services/janela_colab.py:73`). `q_vivas` amplia a janela para chamados com perguntas; não é o único critério de visibilidade (`app/chamados/services/janela_colab.py:63`, `app/chamados/services/janela_colab.py:69`, `app/chamados/services/janela_colab.py:74`). | CENSO: NOVO quanto à assimetria; a fonte compartilhada do app listada não cobre esse filtro (`e3_censo_anexo.r:62`). | EFEITO: sim, condicional: vencido o silêncio, a âncora pode voltar à fila e fornecer porta à tarefa/C17, embora continue fora dessa lista (`app/chamados/services/tarefa_do_dia.py:415`, `app/chamados/services/tarefa_do_dia.py:127`, `app/chamados/juizes.py:1651`). Ocorrência concreta [nao verificado].

ACHADO 10 | CODIGO: confere -- somente aberto/em_analise sem silêncio vigente entram na fila; `_pela_decisao` usa porta None para âncora fora dela (`app/chamados/catalogo/motor.py:29`, `app/chamados/services/painel.py:189`, `app/chamados/services/painel.py:190`, `app/chamados/services/tarefa_do_dia.py:127`). A âncora retorna destino ou chamado da disputa, sem seguir uma cadeia de superação (`app/chamados/services/fio.py:117`). | CENSO: REPETIDO -- a decisão 4 já exige preservar ações humanas sem porta (`e3_censo_anexo.r:117`). | EFEITO: sim na porta publicada, sem necessariamente eliminar a ação exigível; C17 também exige âncora na fila (`app/chamados/services/tarefa_do_dia.py:131`, `app/chamados/services/tarefa_do_dia.py:546`, `app/chamados/juizes.py:1651`). Incidência desses estados [nao verificado].

ACHADO 11 | CODIGO: confere em parte -- os receivers acionam beco após commit e outbox; o beco consulta C7 (`app/chamados/signals.py:240`, `app/chamados/signals.py:257`, `app/chamados/services/becos.py:33`). `lastro.py:239` pertence à leitura `julgar`; o fechador real também consulta fila sem agora e tenta reconciliar (`app/ponto/services/lastro.py:196`, `app/ponto/services/lastro.py:239`, `app/ponto/services/lastro.py:388`, `app/ponto/services/lastro.py:401`). Ausência de post_save no lote não implica ausência de evento: a ressuscitação acorda células explicitamente (`app/chamados/models.py:1569`). | CENSO: NOVO mecanismo transitivo; a cobertura completa desses efeitos já estava declarada não verificada (`e3_censo_anexo.r:25`). | EFEITO: sim, se os gates aprovarem a mudança de status: ela altera a fila e as portas da tarefa/C17 (`app/chamados/models.py:652`, `app/chamados/services/painel.py:189`, `app/chamados/services/tarefa_do_dia.py:127`, `app/chamados/juizes.py:1651`). Ocorrência em produção [nao verificado].

## 12/11 09:39 · sessao 1ff43799f416 · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO · construtor · origem=Code

Entrega parcial no [relatório, com tabela E/N, LEI-AKITA, leitores/escritores e comandos](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-090745/app/docs/BUG-CADASTRO-NAO-ALCANCA-O-FUTURO-c1.txt:1).

- **73 testes sem banco passaram; 24 mutantes ficaram vermelhos.** [Resultados](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-090745/app/docs/BUG-CADASTRO-NAO-ALCANCA-O-FUTURO-c1.txt:116) [nao verificado]
- **O cron mantém um RED:** ainda reescreve futuras. [Teste](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-090745/app/ponto/tests/test_cadastro_futuro_sem_banco.py:742) [nao verificado]
- Suíte Django: **[nao verificado]**, dependência ausente. Commit não realizado pela proibição inicial. [Registro](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-090745/app/docs/BUG-CADASTRO-NAO-ALCANCA-O-FUTURO-c1.txt:195) [nao verificado]

Para concluir o cron, é necessária exceção explícita à proibição de editar [celula.py:870](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-090745/app/ponto/portas/celula.py:870), onde está o ramo exigido pelo próprio brief.

## 12/11 09:39 · sessao 872bd20c1a5c · - · - · origem=Ronald (tmux)

ACHADO 1: **confere em parte**. `ressuscitar_perguntas` realmente limpa `validada_em`, `validada_por`, `respondida_em`, além dos três campos do carimbo, sem registrar um novo ato de ressuscitação/reabertura (`app/chamados/models.py:1564`, `app/chamados/models.py:1567`, `app/chamados/models.py:3265`). Porém não apaga a projeção sem antes preservar os atos históricos: `linhas_da_projecao_sem_linha` produz **RESPOSTA** e **VALIDACAO** ausentes por correspondência `(pergunta, ato, instante)`, usando os instantes originais, texto e autor da validação (`app/chamados/models.py:3925`, `app/chamados/models.py:3932`, `app/chamados/models.py:3938`); `registrar` persiste essas linhas e acorda células (`app/chamados/models.py:3953`). Depois não acrescenta REABERTURA; reabre apenas a disputa e registra auditoria própria (`app/chamados/models.py:1575`, `app/chamados/models.py:1590`, `app/chamados/models.py:2174`). A sequência é ordenada por `ato_em, pk`; **no caso descrito, em que VALIDACAO era o último ato e não há posterior, VALIDACAO permanece por último** (`app/chamados/models.py:3890`, `app/chamados/services/tarefa_do_dia.py:390`). Não é correto universalizar que toda ressuscitação termina com VALIDACAO: pode não haver projeção assinada ou existir ato posterior (`app/chamados/models.py:3925`, `app/chamados/services/tarefa_do_dia.py:390`). EFEITO: **sim, condicional demonstrado pelo código**: a tarefa continua interpretando o último ato como validação (`app/chamados/services/tarefa_do_dia.py:141`), enquanto C6 lê os campos atuais e pode passar a viva após limpeza, observadas suas demais guardas (`app/chamados/juizes.py:1375`, `app/chamados/juizes.py:1391`, `app/chamados/juizes.py:1466`). No caso de validação, tarefa pode responder desconhecido, materializar, rejulgar ou encerrada, conforme lavra/efeitos; não há uma única resposta garantida (`app/chamados/services/tarefa_do_dia.py:142`). O contraste com `reabrir_para_correcao` procede: ela registra novo ato atual com foto antes do update (`app/chamados/services/disputa_emissao.py:1615`, `app/chamados/services/disputa_emissao.py:1630`). Os três chamadores citados foram abertos e conferem (`app/chamados/models.py:1452`, `app/ponto/services/cartorio.py:1673`, `app/chamados/views_arquivar.py:86`). Incidência concreta em produção: [nao verificado].

ACHADO 11: **confere em parte**. Os dois receivers existem: o de beco agenda `fechar_beco_da_disputa` em `on_commit`; o de outbox ignora criação/raw e acorda células quando os campos relevantes são salvos (`app/chamados/signals.py:229`, `app/chamados/signals.py:240`, `app/chamados/signals.py:252`, `app/chamados/signals.py:257`). O beco tem recorte próprio, mas delega fechamento a C7, resolve perguntas e fecha a disputa pela porta; o signal da disputa pode então reconciliar o pai (`app/chamados/services/becos.py:25`, `app/chamados/services/becos.py:33`, `app/chamados/services/becos.py:54`, `app/chamados/services/becos.py:57`, `app/chamados/signals.py:43`). O consumidor realmente chama `lastro.fechar_pelo_marco_colab`, que filtra `qs_na_fila` sem `agora`, verifica marco aceso e tenta `reconciliar`; esta grava o novo estado só se seus gates aprovam (`app/chamados/reconciliador.py:1383`, `app/ponto/services/lastro.py:388`, `app/ponto/services/lastro.py:395`, `app/ponto/services/lastro.py:401`, `app/chamados/models.py:652`). A linha `lastro.py:239` citada pertence a `julgar`, leitura usada na medição, não ao fechador; o mesmo default temporal aparece no fechador real (`app/ponto/services/lastro.py:196`, `app/ponto/services/lastro.py:239`, `app/ponto/services/lastro.py:388`). **Lote não disparar `post_save` não significa perder outbox neste caminho:** a ressuscitação emite explicitamente após o lote; `registrar` também emite e `reabrir_para_correcao` emite explicitamente (`app/chamados/models.py:1566`, `app/chamados/models.py:1569`, `app/chamados/models.py:3955`, `app/chamados/services/disputa_emissao.py:1633`). EFEITO: **sim, se o gate fecha**: mudança de status altera a fila e, portanto, a porta que tarefa/C17 publicam (`app/chamados/services/painel.py:189`, `app/chamados/services/tarefa_do_dia.py:126`, `app/chamados/juizes.py:1647`, `app/chamados/juizes.py:1653`). A alegação de que sejam necessariamente regras concorrentes está superdimensionada: C7 e `reconciliar` são autoridades compartilhadas nesses corpos (`app/chamados/services/becos.py:33`, `app/ponto/services/lastro.py:401`). Incidência concreta em produção: [nao verificado].

CENSO: os dois mecanismos específicos não constam das tabelas/decisões do censo transcrito; classificar como **NOVO**, sem confundir “0 escritores persistentes de tarefa” com “nenhum escritor de seus insumos”. O próprio censo limita sua cobertura transitiva — essa não é prova de ausência. Linha física do anexo transcrito: [nao verificado].

## 12/11 09:39 · sessao 1bad645ac0f9 · - · - · origem=Ronald (tmux)

ACHADO 2 | CODIGO: confere em parte — `data_evento` aceita NULL (`app/chamados/models.py:2424`), e a carga filtra intervalo, excluindo essas linhas (`app/chamados/services/tarefa_do_dia.py:374`); sem data, C6 pula a janela e pode devolver viva se não materializada (`app/chamados/juizes.py:338`, `app/chamados/juizes.py:1455`, `app/chamados/juizes.py:1466`), mas antes aplica validação, tranca, vias e veredito da célula (`app/chamados/juizes.py:1391`, `app/chamados/juizes.py:1395`, `app/chamados/juizes.py:1428`, `app/chamados/juizes.py:1438`); a porta atual de abertura recusa NULL (`app/chamados/models.py:2788`). | CENSO: NOVO — DECISÃO 8 do anexo cobre conversas, não fato com data nula; numeração física do anexo [nao verificado]. | EFEITO: sim, como possibilidade condicional: C17 pode considerar pergunta ligada à célula enquanto C19 não a carrega (`app/chamados/juizes.py:1571`, `app/chamados/juizes.py:1651`, `app/chamados/services/tarefa_do_dia.py:376`); existência dessas linhas na frota e desaparecimento nos consumidores após migração [nao verificado].

ACHADO 3 | CODIGO: confere em parte — C17 agrupa pela FK de célula (`app/chamados/juizes.py:1571`, `app/chamados/juizes.py:1574`), e C19 pelo par colaborador/data_evento e igualdade com o dia (`app/chamados/services/tarefa_do_dia.py:380`, `app/chamados/services/tarefa_do_dia.py:236`); porém a porta citada de rechaveamento atualiza **ambos** data e célula para o novo dia, no mesmo save (`app/chamados/services/fio.py:1035`, `app/chamados/services/fio.py:1038`, `app/chamados/services/fio.py:1044`), e o nascimento seleciona a célula do dia do fato (`app/chamados/models.py:2897`); turno noturno, sozinho, não demonstra desencontro. | CENSO: NOVO — DECISÃO 2 do anexo enumera outros quatro conflitos; numeração física [nao verificado]. | EFEITO: sim **se** existir o desencontro e as demais condições das portas/tarefas forem atendidas (`app/chamados/juizes.py:1649`, `app/chamados/juizes.py:1651`, `app/chamados/services/tarefa_do_dia.py:236`); incidência/invariante na frota [nao verificado].

ACHADO 4 | CODIGO: confere em parte — `agora` chega somente à alimentação da fila (`app/chamados/juizes.py:1566`), enquanto perguntas são julgadas sem hoje/agora (`app/chamados/juizes.py:1573`); `fio_vivo` faz o mesmo (`app/chamados/juizes.py:1503`), e a janela usa `date.today()` quando hoje falta (`app/chamados/juizes.py:592`); `_alimentar` não oferece instante (`app/folha/fora_da_folha.py:55`, `app/folha/fora_da_folha.py:62`); os defaults vizinhos conferem (`app/chamados/services/verbos.py:35`, `app/chamados/services/painel.py:190`, `app/chamados/services/vigia_fio.py:566`). O próprio DIFF declara esse limite (`app/chamados/management/commands/diff_e3_sombra.py:105`); a sonda transcrita no censo usa `freeze_time(agora)`, portanto a irreprodutibilidade absoluta alegada não está demonstrada para ela [nao verificado]. | CENSO: NOVO — não há decisão específica sobre relógios no anexo; numeração física [nao verificado]. | EFEITO: sim, como possibilidade de mudar C6/C17: janela pode alterar a conclusão sobre carimbo sem testemunha (`app/chamados/juizes.py:693`, `app/chamados/juizes.py:700`, `app/chamados/juizes.py:1473`), enquanto C19 passa hoje e agora explicitamente (`app/chamados/services/tarefa_do_dia.py:489`, `app/chamados/services/tarefa_do_dia.py:491`); divergência efetiva de paridade perto da virada [nao verificado].

Observação para consolidação: se “EFEITO” significar ocorrência efetiva em algum dia **do dado de produção fornecido**, marque `[nao verificado]` nos três; não há instâncias/linhas de produção no material que demonstrem esses estados. Os “sim” acima são possibilidades condicionais demonstradas pelo código, não medições de frota.

## 12/11 09:39 · sessao e7a5c82407d6 · - · - · origem=Ronald (tmux)

ACHADO 5 | CODIGO: confere em parte — importa os privados em `app/ponto/services/pautas_dos_discordantes.py:61`; alimenta em `:75`, exclui dias cujo fio é diferente de `FIO_SEM_PORTA` em `:77` e inclui os restantes em `:79`; pode escrever Pautas quando `apply=True` (`:173`, `:177`, `:183`). A quebra ou permanência obrigatória no juiz antigo não decorre disso: `_fio_do_dia` chama `_porta` (`app/folha/fora_da_folha.py:77`), que chama C17 (`:67`); alterar internamente `_porta` preservando o contrato alcança esse consumidor. | CENSO: NOVO consumidor concreto; o censo fornecido lista o wrapper, mas não este chamador; linha numérica do anexo [nao verificado]. | EFEITO: nao — este caminho consulta a porta e escreve Pautas, sem alterar entradas dos três juízes (`app/ponto/services/pautas_dos_discordantes.py:75`, `:177`, `:183`).

ACHADO 6 | CODIGO: confere — o resultado de `do_dia` escolhe órfãos com causa `fabrica_nao_rodou`, dispara a fábrica com `notificar=True` e relê o resultado (`app/chamados/services/cobrar_dia.py:45`, `:46`, `:49`, `:53`); mensagem/`ok` usam pergunta/chamado do resultado (`:55`, `:58`, `:70`). `do_dia` usa `VIVOS`, módulos, dia do chamado, perguntas C6 e retenção (`app/chamados/services/pergunta_no_app.py:217`, `:218`, `:220`, `:221`, `:223`). | CENSO: NOVO chamador com efeito de escrita; o serviço já listado pelo copiloto não cobre este uso; linha numérica do anexo [nao verificado]. | EFEITO: sim, potencial estático — a fábrica abre perguntas (`app/chamados/services/disputa_emissao.py:837`), cujo nascimento persiste (`app/chamados/models.py:3018`); a tarefa e C17 carregam essas entradas (`app/chamados/services/tarefa_do_dia.py:374`, `app/chamados/juizes.py:1571`); ocorrência em produção [nao verificado].

ACHADO 7 | CODIGO: confere — qualquer texto não vazio ganha precedência sobre a vitalidade; depois consulta `fio_vivo` sem informar relógio e senão devolve `nada` (`app/folha/previa.py:142`, `:144`, `:146`); essa saída preenche o campo `material` (`:95`). | CENSO: NOVO leitor concreto; ausente da relação de leitores fornecida; linha numérica do anexo [nao verificado]. | EFEITO: nao — muda a apresentação `material` da prévia, não a resposta de tarefa/C6/C17; o caminho mostrado apenas recebe perguntas e devolve rótulo (`app/folha/previa.py:139`, `:142`, `:144`, `:146`).

ACHADO 8 | CODIGO: confere — o contador considera canal a pergunta com `respondida_em` nulo ou chamado em `VIVOS`, sem filtro de silêncio (`app/chamados/services/furo_sem_canal.py:80`, `:82`, `:85`); `VIVOS` inclui `registrado`, enquanto `COBRANCA` não (`app/chamados/catalogo/motor.py:24`, `:29`). Pode excluir do balde sem canal um dia cujo único chamado é registrado/silenciado; ocorrência real [nao verificado]. | CENSO: NOVO leitor concreto; ausente da relação fornecida; linha numérica do anexo [nao verificado]. | EFEITO: nao — o caminho calcula conjuntos e retorna contagens/amostra; não altera entradas ou respostas dos juízes (`app/chamados/services/furo_sem_canal.py:80`, `:85`, `:89`).

## 12/11 09:38 · sessao 4d2389e8adb8 · TRIAGEM-OFFLINE-SEM-CAMERA · leitura · origem=Code

**Sim: a entrega de páginas offline já era defeituosa no pré-v6.** O SW não grava documentos nas navegações normais; abrir as páginas com internet não executa o ramo que guardaria o HTML. Depois, sem rede e sem entrada correspondente no CacheStorage, ele devolve `Response.error()`. Essa cadeia permanece na v6, pois o patch fornecido não altera o SW. (`app/static/sw.js:3–12`, `29–30`, `45–50`, `67–79`.)

**A reprodução literal de “todos os toques inertes” e “tela branca” no iPhone é indeterminada pela leitura.** O código permite demonstrar a falha de entrega do documento; não demonstra a aparência apresentada pelo Safari, os eventos de toque nem o conteúdo efetivo do cache daquele aparelho. [nao verificado]

Nas referências seguintes, **`Δ arquivo:+N` identifica a linha do arquivo novo numerada pelos hunks do patch**, não a posição física na mensagem. **`=` significa pré-v6 e v6 iguais**: SW, views, URLs, home e bases não aparecem como arquivos alterados no diff completo fornecido. Para esses arquivos, não existe linha adicionada no patch para citar.

Para o SW — itens 1, 4 e C1 — a comparação é:

| Questão | Pré-v6 | V6 |
|---|---|---|
| Nome do cache | `juliani-__BUILD_ID__`. (`app/static/sw.js:1`.) | = |
| Precache | `/static/pwa.js`, `/static/manifest.json`, `/static/icons/icon-192.png`, `/static/icons/icon-512.png`. (`app/static/sw.js:3–8`.) | = |
| Instalação | `cache.addAll(STATIC_CACHE)`. (`app/static/sw.js:10–13`.) | = |
| Navegação online | `fetch(event.request)`, **sem gravar a resposta**. (`app/static/sw.js:45–50`.) | = |
| Navegação offline | `caches.match(request)`; ausente, `Response.error()`. (`app/static/sw.js:29–30`, `48`.) | = |
| Fallback de documento | Nenhuma página alternativa: só a resposta correspondente à própria requisição, ou erro de rede. (`app/static/sw.js:29–30`.) | = |
| Gravação dinâmica de HTML | Existe para GET de `/` e `/ponto/bater/…`, **depois do retorno antecipado das navegações**. (`app/static/sw.js:46–50`, `67–79`.) | = |

Portanto, o preparo relatado foi feito, mas **esse preparo por navegação não povoa o cache de documentos neste código**. Isso decorre do retorno em `app/static/sw.js:50`, anterior ao `cache.put` de `app/static/sw.js:74`; não depende de supor que o usuário esqueceu de abrir alguma tela.

Há também uma distinção de rota: a home efetiva do colaborador renderiza `dashboard_colaborador_v2.html`, cujo “Bater ponto” aponta para **`/ponto/bater-v2/`**, não `/ponto/bater/`. (`app/core/views.py:270–271`, `app/templates/core/dashboard_colaborador_v2.html:31`, `app/ponto/urls.py:12`; v6 =.)

| Página/documento | O que uma navegação ou reload offline recebe, nos dois lados |
|---|---|
| Home `/` | Resposta encontrada para a requisição; ausente, `Response.error()`. (`app/static/sw.js:29–30`, `45–50`.) |
| `/ponto/bater/` | O mesmo tratamento. (`app/static/sw.js:29–30`, `45–50`.) |
| Apto da triagem | Não tem URL própria nesses caminhos: é o template escolhido pela view de bater. O cache é consultado pela URL navegada, não pelo nome `apto.html`. (`app/ponto/views.py:225–235`, `358`; `app/static/sw.js:29–30`.) |
| `/ponto/bater-v2/`, destino da home | O mesmo tratamento de navegação; essa rota tampouco está na lista posterior `comCache`. (`app/static/sw.js:45–50`, `68`; `app/ponto/urls.py:12`.) |

**Não há mudança de fallback entre pré-v6 e v6.** O retorno sem documento é erro de rede; não é uma resposta HTML 404/500 fabricada pelo SW. A tela branca como apresentação desse erro no iOS é [nao verificado]. (`app/static/sw.js:29–30`.)

O build id é derivado assim, em ambos os lados:

- `GOVERNADOS_PELO_SW` contém `sw.js`, `pwa.js`, `manifest.json` e os dois ícones. Templates não entram. (`app/core/views.py:524–525`.)
- A raiz é `/app/staticfiles` quando ali existe `sw.js`; caso contrário, `/app/static`. (`app/core/views.py:528–530`.)
- O cálculo concatena os conteúdos no SHA-1, na ordem da tupla, usando `ausente:<nome>` em falha de leitura, e conserva os oito primeiros hexadecimais. Há memoização por processo durante 30 segundos. (`app/core/views.py:520–556`.)
- A view substitui `__BUILD_ID__` pelo resultado e usa `ETag: "sw-<build>"`. (`app/core/views.py:560–576`.)

**A v6 muda uma entrada desse hash: `pwa.js`.** Se o nome resultante mudar, o `activate` apaga os outros caches; o `controllerchange` já podia recarregar páginas sem formulário marcado como sujo. Esses mecanismos são anteriores à v6. Sua ocorrência durante o smoke e os nomes concretos dos caches implantados são [nao verificado]. (`app/core/views.py:548–555`; `Δ app/static/pwa.js:+58–305`; `app/static/sw.js:17–23`; `app/static/pwa.js:34–45`, trecho preservado.)

Sobre os cliques — item 2, C4 e C5 — **os cartões da home não precisam de handlers registrados por `pwa.js`**. Bater, Espelho, Escala, Confirmar Batidas, Chamados, Justificar, Ausência, Holerites e Férias são links `<a href>`, sem `await`, pré-checagem de rede ou teste de `navigator.onLine` antes de navegar. (`app/templates/core/dashboard_colaborador_v2.html:31`, `56`, `60`, `66`, `78`, `82`, `88`, `100`, `110`; v6 =.)

O relógio consulta `/hora/` separadamente; offline, usa seu fallback. Ele não envolve o clique dos cartões. (`app/templates/core/dashboard_colaborador_v2.html:152–159`; v6 =.)

| Controle | Quem fornece o comportamento, nos dois lados |
|---|---|
| Cartões de navegação | O próprio `<a href>`. (`app/templates/core/dashboard_colaborador_v2.html:31–110`.) |
| Menus da casca | Script inline independente do PWA. (`app/templates/base.html:340–382`; `app/templates/base_app.html:98–139`.) |
| Fechar/adiar modal de onboarding | `onclick` inline com `remove()`. Presença desse modal no smoke: [nao verificado]. (`app/templates/core/dashboard_colaborador_v2.html:121–141`.) |
| “Ligar avisos” e fechar o convite | Criados e ligados por `pwa.js`. (`app/static/pwa.js:351–374`; v6: linhas herdadas `+536–559`.) |

As duas cascas carregam `pwa.js` com `defer`; a ausência desse script prejudica a fila e os recursos que ele exporta, mas não remove os `href` dos cartões. (`app/templates/base.html:20`; `app/templates/base_app.html:20`; `app/static/pwa.js:305–318`; v6: `Δ app/static/pwa.js:+489–502`.)

A auditoria de sintaxe/APIs encontrou:

| Construção ou API | Pré-v6 | V6 e alcance da falha |
|---|---|---|
| Optional chaining | `event.data?.type`, `req.result?.valor` e acessos ao cookie. (`app/static/pwa.js:16`, `78`, `140`, `245`, `261`, `403`.) | Já existia; configuração passa a `linha?.valor`. (`Δ app/static/pwa.js:+124`; demais usos preservados.) Parser incompatível pode rejeitar o script inteiro; isso no aparelho relatado é [nao verificado]. |
| Nullish coalescing `??` | Já em `lerConfig`. (`app/static/pwa.js:78`.) | Preservado em `Δ app/static/pwa.js:+124`. |
| `async/await`, arrows, `let/const`, spread, destructuring e `catch` sem binding | Já presentes. (`app/static/pwa.js:11`, `50`, `95`, `209`, `240`, `286`.) | Continuam presentes; não constituem novidade exclusiva da v6. (`Δ app/static/pwa.js:+61–128`, `188–231`.) |
| Chave computada e métodos de objeto | — | Novos no helper de selfie/aviso. São sintaxe analisada junto com o script. (`Δ app/static/pwa.js:+199`, `244`, `253`.) |
| `AbortController` | — no `pwa.js` pré; já usado pelos relógios das telas. (`app/templates/core/dashboard_colaborador_v2.html:154`; `app/templates/ponto/triagem/base_triagem.html:41`.) | Novos usos no PWA verificam `typeof AbortController === 'function'`. (`Δ app/static/pwa.js:+146`, `297`.) |
| `navigator.mediaDevices.getUserMedia` | Chamado nas telas. (`app/templates/ponto/bater_ponto_v2.html:238`; `app/templates/ponto/triagem/apto.html:149`, `248`.) | Chamado dentro do trabalho assíncrono de `_noTeto`, que captura exceções/rejeições; não durante a avaliação inicial. (`Δ app/static/pwa.js:+188–195`, `211–217`.) |
| `indexedDB` | Acesso ao chamar `abrirDB`. (`app/static/pwa.js:48–60`.) | Continua dentro da função, agora com captura explícita. (`Δ app/static/pwa.js:+66–86`.) |
| `serviceWorker`, storage, geolocation e push | Guardas/`try` já existentes. (`app/static/pwa.js:12–30`, `35–46`, `291`, `333–344`, `436–443`.) | Preservados; não há nova chamada desses recursos identificada como fatal na avaliação inicial. (`Δ app/static/pwa.js`, trechos preservados.) |

**Primeiro iOS suportado para cada construção/API acima: [nao verificado].** O código e o anexo não fornecem uma matriz de compatibilidade.

Não há `structuredClone`, `AbortSignal.timeout`, `crypto.randomUUID`, `Promise.any` nem `await` de nível superior no PWA auditado; os `await` estão dentro de funções assíncronas. (`app/static/pwa.js:1–443`; `Δ app/static/pwa.js:+45–305`, `489–511`.) Não identifiquei no acréscimo da v6 uma nova API sem guarda chamada sincronamente antes dos exports; erro real de avaliação no Safari permanece [nao verificado].

A espera de `/hora/` na inicialização também ocorre **depois** da definição de `window.JulianiPWA`; sua pendência não impede, por essa cadeia, a criação dos exports. (`app/static/pwa.js:305–326`; v6: `Δ app/static/pwa.js:+489–511`.)

Para C2, o SW **não contém tratamento de `response.redirected`**: não rejeita a resposta, não a reconstrói e não normaliza seu URL antes de servir. Os ramos que gravam cache clonam a resposta e a guardam pela requisição original. (`app/static/sw.js:29–30`, `54–60`, `67–77`; v6 =.)

Entretanto, **o código chama `fetch(event.request)`**, sem estabelecer `redirect: 'follow'`. Não basta aplicar o padrão de `fetch(url)` e afirmar qual era o modo de redirect da requisição de navegação concreta. Esse valor e uma resposta `redirected=true` efetivamente guardada no aparelho são [nao verificado]. (`app/static/sw.js:42`, `48`, `57`, `71`.)

Há estes caminhos condicionais de redirect, iguais nos dois lados:

- Home: login, troca de senha e ciência de comunicado. (`app/core/views.py:117`, `122–123`, `136–140`; `app/config/settings/base.py:106`.)
- Bater e bater-v2: login, onboarding e ciência de comunicado. (`app/ponto/views.py:241–255`.)
- Ciência pode produzir URL com `?next=…` e `app=1`. (`app/comunicados/services.py:36–50`.)

Não existe redirect obrigatório nos caminhos liberados: eles renderizam os templates. Qual condição ocorreu no smoke é [nao verificado]. (`app/core/views.py:270–271`; `app/ponto/views.py:356–358`.) O `CommonMiddleware` está configurado, mas o redirect concreto de URL sem barra não foi demonstrado. (`app/config/settings/base.py:57`; comportamento efetivo [nao verificado].)

**Um GET não navegacional de `/` ou `/ponto/bater/…` pode guardar a resposta recebida sem verificar se ela veio de redirect**, inclusive porque a exclusão de login é decidida pelo pathname original. Depois, o fallback não verifica `redirected` ao servi-la. Isso é possibilidade do código; não prova que aconteceu no iPhone. (`app/static/sw.js:39–43`, `67–77`, `29–30`; v6 =.) A recusa específica pelo Safari/iOS e sua manifestação como branco são [nao verificado].

Para C3, o SW usa a requisição completa como chave em `cache.put` e em `caches.match`, sem `ignoreSearch`, `ignoreVary` ou normalização de barra. Portanto não programa equivalência entre URLs com queries diferentes ou com/sem barra; também não pede para ignorar `Vary`. (`app/static/sw.js:30`, `56`, `59`, `74`; v6 =.) Os headers `Vary`, URLs finais e pares de chaves realmente presentes no dispositivo são [nao verificado].

Assim, **C1, C2, C3 e C4 já tinham essas características antes da v6**: mesmo ramo de navegação, mesmo tratamento de respostas, mesmas chaves e mesmos links da home. (`app/static/sw.js:29–79`; `app/templates/core/dashboard_colaborador_v2.html:31–110`; arquivos ausentes do diff completo fornecido.)

Sobre `bater_ponto.html`, item 3:

| Referência pré-v6 | O que ela significa | V6 |
|---|---|---|
| Views e URLs ativas | `/bater/` seleciona triagem; `/bater-v2/` renderiza v2. Nenhum desses renders aponta para o legado. (`app/ponto/urls.py:11–12`; `app/ponto/views.py:225–235`, `356–358`.) | = |
| Verificador da lei | Lê o arquivo para inspeção textual. (`app/scripts/check_a_lei_batida.py:20–23`.) | Referências removidas no hunk `Δ app/scripts/check_a_lei_batida.py:@@ -17,10 +17,7 @@`. |
| Verificador de URLs | Inclui o arquivo na lista inspecionada. (`app/tools/check_frontend_urls.py:29`.) | Entrada removida no hunk `Δ app/tools/check_frontend_urls.py:@@ -26,7 +26,6 @@`. |
| Links dentro do legado | Usam o nome da rota `ponto:bater_ponto`, não o arquivo como destino. (`app/templates/ponto/bater_ponto.html:27`, `201`.) | Arquivo inteiro eliminado: `Δ app/templates/ponto/bater_ponto.html:@@ -1,614 +0,0 @@`. |
| SW/PWA e retorno offline | SW não precacheia esse documento; PWA envia à API; apto retorna à home. (`app/static/sw.js:3–8`; `app/static/pwa.js:153`; `app/templates/ponto/triagem/apto.html:115–118`, `298`.) | Nenhum desses caminhos passa a depender do arquivo apagado. (`Δ app/static/pwa.js`, sync preservado; `Δ app/templates/ponto/triagem/apto.html:+307`.) |

**A exclusão não retirou página alguma do precache nem rompeu os renders ativos identificados.** Atribuir a ela 404, 500 ou branco não é sustentado por esses caminhos; ocorrência real por outro caminho é [nao verificado]. (`app/static/sw.js:3–8`; `app/ponto/views.py:356–358`; hunk de exclusão acima.)

As mudanças da v6 que efetivamente alteram o caminho **depois de chegar à tela de batida** são:

| Pré-v6 | Alteração da v6 |
|---|---|
| V2 recusa continuar sem foto. (`app/templates/ponto/bater_ponto_v2.html:285–290`, `312`.) | Passa a consultar o helper de selfie e seguir com aviso. (`Δ app/templates/ponto/bater_ponto_v2.html:+269–296`.) |
| Apto offline esconde confirmar após rejeição da câmera; a gravação fica dentro do callback de `toBlob`. (`app/templates/ponto/triagem/apto.html:248–282`.) | Mantém confirmar e consulta captura com teto. (`Δ app/templates/ponto/triagem/apto.html:+245–293`.) |
| Escritor espera `obterGap`, que pode esperar `/hora/` e seu corpo sem teto. (`app/static/pwa.js:86–108`.) | Limita o gap antes da fila. (`Δ app/static/pwa.js:+135–162`, `269–276`.) |
| Transação da fila trata `complete` e `error`. (`app/static/pwa.js:111–121`.) | Helper trata também `abort`; demora mostra aviso e continua esperando o evento definitivo. (`Δ app/static/pwa.js:+90–103`, `239–286`.) |

O RED headless adequado deve exercitar **navegação HTTP real com SW**, porque o teste novo da v6 abre templates em `file://` e substitui rede/armazenamento; ele não cobre o trajeto home → documento offline, e declara não provar Safari. (`Δ app/ponto/tests/test_selfie_nao_barra_chromium.py:+9–18`, `30–34`.)

Proposta de RED, **não executada** [nao verificado]:

1. Usar contexto novo, usuário de teste liberado dos redirects e SW controlando a página; abrir com rede home, bater-v2 e bater/confirmar, depois voltar à home. As rotas e condições estão em `app/ponto/urls.py:11–12`, `app/ponto/views.py:241–255`, `356–358`.
2. Cortar a rede e clicar no `<a>` “Bater ponto”; exigir que apareça o documento de bater-v2. (`app/templates/core/dashboard_colaborador_v2.html:31`.)
3. Registrar a batida e exigir uma linha persistida em `batidas_pendentes`; testar câmera funcional e, separadamente, câmera ausente. (`app/static/pwa.js:112–120`; v6: `Δ app/static/pwa.js:+275–282`.)
4. Recarregar offline home e cada tela; exigir documento utilizável. O ramo sob teste é `app/static/sw.js:45–50`.

**No contexto novo preparado apenas por navegações, o primeiro passo que viola o esperado já é o clique da home:** o SW não guardou o documento durante o preparo e entrega `Response.error()` quando a consulta não encontra entrada. O reload tem a mesma falha. Isso vale para pré-v6 e v6. (`app/static/sw.js:3–12`, `29–30`, `45–50`, `67–79`.) Se a tela for alcançada por outro estado de cache, o pré-v6 ainda possui as barreiras de foto e espera da hora descritas acima; qual delas ocorreu no campo é [nao verificado].

Por fim, estes são os escritores e leitores dos caminhos de execução do front:

| Dado | Quem escreve/define | Quem lê — pré-v6 e v6 |
|---|---|---|
| Build id | Calcula SHA-1 e grava `_CACHE_DO_BUILD['v']`. (`app/core/views.py:547–556`; v6 =.) | Memoização e view `/sw.js`. (`app/core/views.py:543–545`, `562`, `572`; v6 =.) |
| `GOVERNADOS_PELO_SW` | Tupla declarada em `app/core/views.py:524–525`; v6 =. | Laço do hash em `app/core/views.py:548`; v6 =. |
| `CACHE_NAME` | View injeta o build; SW define a constante. (`app/core/views.py:572`; `app/static/sw.js:1`; v6 =.) | Instalação, limpeza de caches e gravações dinâmicas. (`app/static/sw.js:12`, `20`, `59`, `74`; v6 =.) |
| Lista de precache | Array `STATIC_CACHE`. (`app/static/sw.js:3–8`; v6 =.) | `cache.addAll`. (`app/static/sw.js:12`; v6 =.) |
| Conteúdo do CacheStorage | `addAll` e `cache.put`. (`app/static/sw.js:12`, `59`, `74`; v6 =.) | `caches.match`, consultando caches sem restringir por `CACHE_NAME`. (`app/static/sw.js:30`, `56`; v6 =.) |
| Banco e chave da fila | IndexedDB **`juliani-offline`, versão 2**, store **`batidas_pendentes`**, chave **`id` autoIncrement**. Pré: `app/static/pwa.js:50–54`; v6: `Δ app/static/pwa.js:+73–77`. | Operações abaixo; não é uma chave de `localStorage`. (Mesmas referências.) |
| Inclusão de batida | Pré: `registrarBatidaOffline` → `add`, `app/static/pwa.js:107–120`. V6: `Δ app/static/pwa.js:+269–282`. | Sync usa `getAll`: pré `app/static/pwa.js:132`; v6 linha herdada `+313`. |
| Contagem da fila | As inclusões/exclusões alteram a quantidade. (Referências desta tabela.) | Indicador usa `count`: pré `app/static/pwa.js:190`; v6 herdada `+371`. |
| Exclusão da fila | Sync usa `delete(b.id)` após sucesso ou HTTP 400: pré `app/static/pwa.js:158–174`; v6 herdadas `+339–355`, deletes em `+343`, `+353`. | Próximos `getAll`/`count` acima. |
| Configuração do relógio, separada da fila | Store `config`, chave `chave`; `put` de `ntp_gap`/`ntp_calculado_em`. Pré: `app/static/pwa.js:56`, `67`, `92–93`; v6: `Δ app/static/pwa.js:+79`, `114`, `158–159`. | Pré `get` em `app/static/pwa.js:77`; v6 `Δ app/static/pwa.js:+123`. |

As linhas v6 marcadas **herdadas** resultam do deslocamento `+181` nos trechos de sync/indicador preservados pelo patch; não estão impressas individualmente nele. (`Δ app/static/pwa.js:@@ -104,23 +170,138 @@`, `@@ -308,6 +489,9 @@`.)

O SW não abre essa fila: envia `SYNC_BATIDAS` às páginas, e o receptor de `pwa.js` chama a sincronização. A limpeza de caches usa CacheStorage, enquanto a fila permanece no IndexedDB. (`app/static/sw.js:19–20`, `112–125`; `app/static/pwa.js:15–16`, `50–54`; v6 = nesses trechos, banco em `Δ app/static/pwa.js:+73–77`.)

_4147 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
