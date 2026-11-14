# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 01:17** · publicado em **14/11/2026 01:19** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 14/11 01:17 · sessao df4d76a780ab · - · - · origem=Ronald (tmux)

Os ajustes corrigem os pontos de revisão R8, R4 ORM e a declaração de R11. (`chamados/juizes.py:1572`, `chamados/tests/test_e3_portas_tarefa.py:235`, `chamados/management/commands/diff_e3_sombra.py:107`)

R13 ganhou cobertura do caminho administrativo dispensado. (`chamados/tests/test_e3_portas_tarefa.py:585`)

Persiste uma falha no executor: `absorcao_admin_sem_rotulo` tem alvo repetido e encerrou com prova inválida. É necessário tornar o contexto da substituição exclusivo de `expor_admin`. (`chamados/tests/e3_v8_mutantes.py:62`, `e3-v8-mutantes.out:1464`)

R2 continua pendente de alcance ORM e cura; o teste puro agora identifica corretamente a hipótese administrativa isolada. (`chamados/tests/test_e3_portas_tarefa.py:220`, `:440`)

Execução ORM e certificação: **[nao verificado]**.

## 14/11 01:14 · sessao df4d76a780ab · - · - · origem=Ronald (tmux)

Pré-verificação encontrou:

- **R8 reprovado:** `.only(...)` conserva os `select_related` herdados da fila, incluindo relações omitidas, podendo gerar `FieldError`. Limpar relações antes da seleção ou fornecer `_base` adequado. (`chamados/juizes.py:1571`, `chamados/services/painel.py:98`)
- **R2 ainda não curado:** ações simultâneas `validar/admin` e `responder/colaborador` sobre a mesma porta produzem `fio='colab'`. O teste puro contém somente a ação administrativa; não comprova o caso simultâneo do teste ORM. (`chamados/services/tarefa_do_dia.py:588`, `chamados/tests/test_e3_portas_tarefa.py:212`, `:439`)
- **R4 ORM não representa Q11:** registra `SILENCIO` sem projetar o encerramento da pergunta e espera `cobrar/colab`. O puro cobre validação com lavra; a fixture ORM precisa corresponder. (`chamados/tests/test_e3_portas_tarefa.py:235`, `:452`)
- **R11 deixou declaração obsoleta:** o comando ainda anuncia leitura do relógio da máquina pelo app. (`chamados/management/commands/diff_e3_sombra.py:107`)
- **R13:** falta mutante do segundo caminho alterado, `expor_admin`. (`chamados/services/tarefa_do_dia.py:258`, `chamados/tests/e3_v8_mutantes.py:55`)

A adaptação posicional preserva os oito índices anteriores. (`chamados/services/vigia_fio.py:635`, `chamados/management/commands/backfill_fio_morto_e4b.py:253`)

Comando executado sem banco:

```bash
env -u DJANGO_SETTINGS_MODULE PYTHONDONTWRITEBYTECODE=1 \
python3 -m unittest chamados.tests.test_e3_pendentes_app.CatracaC17Test
```

Resultado: **Ran 7; OK**. A catraca cobra um único chamador permitido de C17. (`chamados/tests/test_e3_pendentes_app.py:146`)

Alcance e execução ORM: **[nao verificado]**. Nenhum arquivo editado por este revisor.

## 14/11 01:08 · sessao bb855a61a1c6 · CORTE-DAS-TRAVAS · verificador · origem=Code

**REPROVADO para pouso.** Há caminhos que classificam dinheiro/universo como “so leitor”, certificação que ignora arquivos executáveis não rastreados e catraca que ainda reprova sem aumento do total (`bin/classe_da_obra.py:151`; `bin/classe_da_obra.py:237`; `app/core/tests/test_medida_turno_do_par.py:563`).

Identidade do commit/base, arquivo original do aval, inteira final e retomada efetiva em produção: **[nao verificado]**. [nao verificado]

1. **[bloqueia] Classificador: dinheiro pode sair “so leitor”.**  
   O reconhecimento de dinheiro depende do caminho; qualquer `.py` restante sem os tokens enumerados recebe `LEITOR` (`bin/classe_da_obra.py:50`; `bin/classe_da_obra.py:145`). Contraprovas em memória: mudar `adicional_noturno_pct: 20.0` para `40.0`, ou a conversão de horas extras de `/ 60` para `/ 30`, devolve **“so leitor”** (`app/core/regua_cct.py:42`; `app/ponto/services/dia_pago.py:180`; `bin/classe_da_obra.py:151`). Isso viola diretamente a proibição de usar essa classe em dinheiro.

2. **[bloqueia] Classificador: mudar universo julgado também pode sair “so leitor”.**  
   O trecho `j[0] <= dia <= j[1]` seleciona os dias da janela de cada empresa. Trocar somente os limites por `<` altera o universo e devolve **“so leitor”**, confirmado em memória (`app/chamados/services/furo_sem_canal.py:75`; `bin/classe_da_obra.py:151`). O classificador examina somente linhas alteradas e procura uma enumeração de tokens, sem reconhecer essa restrição de universo (`bin/classe_da_obra.py:117`; `bin/classe_da_obra.py:55`).

3. **[bloqueia] Inteira final: os arquivos executados podem divergir da tree certificada.**  
   Trecho: `status --porcelain --untracked-files=no`. Arquivos não rastreados ficam fora da limpeza, mas a suite monta o diretório físico `copia/app` (`bin/classe_da_obra.py:237`; `bin/suite.sh:204`; `bin/suite.sh:230`). Portanto, um módulo não rastreado pode participar da execução sem integrar a tree registrada; comparar `HEAD^{tree}` antes/depois não detecta isso (`bin/classe_da_obra.py:247`; `bin/classe_da_obra.py:255`). Ocorrência em produção: **[nao verificado]**.

4. **[bloqueia] Catraca: o corte “reprova só pelo total” não alcançou o teste completo.**   [nao verificado]
   `_catraca()` aceita redução, mas logo depois permanece `assertEqual(CRITERIOS_REGISTRADOS, ...)`, congelando A1/B1 em 6/82 (`app/core/tests/test_medida_turno_do_par.py:508`; `app/core/tests/test_medida_turno_do_par.py:108`; `app/core/tests/test_medida_turno_do_par.py:563`). Esses campos são **contagens**, apesar do nome “critérios” (`app/core/medida_turno_do_par.py:441`).  
   Contraprova em memória: retirar o corpo pareador de `_alinhar_por_papel` produz pareadores **9→8**, derivações **85→85**, A1 **6→5**. `_catraca()` apenas avisa; o teste completo reprova pela igualdade restante (`app/core/tests/test_medida_turno_do_par.py:158`; `app/core/tests/test_medida_turno_do_par.py:563`).

5. **[bloqueia] Triagem: o destino implementado diverge do destino literal pedido.**  
   O aval fornecido pede `logs/AUDITORIA-TRAVAS/triagem.log` **[nao verificado no arquivo original]**. O código escreve e vigia **`logs/codex/triagem.log`** (`bin/disjuntor.py:148`; `bin/disjuntor.py:936`). O acréscimo no wrapper apenas imprime uma instrução; não chama o escritor de triagem (`bin/codex_pergunta.sh:469`). A retomada da escrita no destino pedido permanece **[nao verificado]**.

6. **[bloqueia] Vigia: há falsos “zero pendentes” por integridade e por identidade.**  
   `chamadas.log` malformado tem linhas descartadas silenciosamente; um `fim` cortado também desaparece da leitura. O vigia interpreta a ausência resultante como nenhuma conclusão a triar e retorna rc 0 (`bin/codex_limite_estado.py:57`; `bin/codex_limite_estado.py:66`; `bin/disjuntor.py:1015`; `bin/disjuntor.py:1038`). Ambos foram reproduzidos em memória.  
   Além disso, a triagem identifica a volta por `hash12@pid@fim@sha12`, mas o vigia reduz isso a **obra/hash12/pid**. Uma chamada posterior no mesmo hash, após reutilização do PID, pode herdar a triagem antiga e sumir das pendências (`bin/disjuntor.py:928`; `bin/disjuntor.py:1003`). Essa contraprova também retornou zero em memória. O vigia ainda passa `{}` ao leitor, perdendo a conferência histórica de truncamento usada pelo B3 (`bin/disjuntor.py:997`; `bin/disjuntor.py:955`).

7. **[fila — brecha herdada] B3: “linha do Ronald” é reserva textual, sem autoria conferida.**  
   `ERRO-BOBO` é aceito pelo conteúdo; a cota só se aplica a `tipo == 'auto'` (`bin/disjuntor.py:772`; `bin/disjuntor.py:1115`). Uma linha nova `ERRO-BOBO OBRA-X: texto do Code` liberou, em simulação em memória, uma obra com AUTO já esgotada (`bin/disjuntor.py:1124`). A guarda protege `--rearma-b3`, mas não autentica quem escreveu essa linha no RELATO (`bin/disjuntor.py:249`; `bin/disjuntor.py:264`). **Não é afrouxamento introduzido pelo diff**: o reconhecedor antigo já aparece como contexto no anexo fornecido **[nao verificado na base Git]**.

8. **[fila] Os selos ainda têm mutantes plausíveis que passam.**
   
   - **GATE:** o selo fabrica certificados diretamente no livro e testa o portão, sem executar `pouso_inteira`. Suprimir a condição de recusa do emissor não é exercitado por esse selo (`bin/tests/test_gate_pela_classe.sh:121`; `bin/tests/test_gate_pela_classe.sh:135`; `bin/classe_da_obra.py:256`).
   - **B3/caso 9:** adicionar o literal `B3: o Code libera desenho pela AUTOLIBERA, do Ronald` passou o **caso 9 completo em memória**. `DESENHO` é procurado distinguindo maiúsculas/minúsculas; sujeito/verbo não (`bin/tests/test_disjuntor.sh:1332`; `bin/tests/test_disjuntor.sh:1356`).
   - **Triagem:** o selo corrompe `triagem.log`, mas não cobre `chamadas.log` malformado nem reutilização de PID/hash, os falsos zeros do achado 6 (`bin/tests/test_triagem_pendente.sh:17`; `bin/tests/test_triagem_pendente.sh:50`).

9. **[fila] Catraca: aviso ao RELATO não tem entrega comprovada.**  
   `_aviso()` somente escreve stderr; os novos MORDE conferem listas retornadas, sem cobrar a publicação (`app/core/tests/test_medida_turno_do_par.py:467`; `app/core/tests/test_medida_turno_do_par.py:593`; `app/core/tests/test_medida_turno_do_par.py:602`). O filtro do pre-push descarta `CATRACA-AVISO` da saída apresentada (`bin/pre-push.sh:146`). Escrita desses avisos no RELATO: **[nao verificado]**.

10. **[fila] Leitores humanos externos continuam recebendo o significado antigo de `arquivo=`.**  
    O wrapper ainda diz “jsonl cresce = prova de vida”, e LEIS ainda descreve prova por “pid/arquivo” (`bin/codex_pergunta.sh:51`; `app/docs/LEIS.md:66`). Agora arquivo sozinho falha e, junto do PID, é inerte (`bin/hook_stop_fila1.py:748`).

O mapa de escritores, leitores e significados ficou assim:

| Dado | Quem escreve | Quem lê / significado |
|---|---|---|
| `chamadas.log` | Wrapper: `ini` e `fim` (`bin/codex_pergunta.sh:305`, `:272`). | Leitor comum `_linhas_chamadas`; vigia o reutiliza (`bin/codex_limite_estado.py:49`; `bin/disjuntor.py:1007`). TAB; chave obra/hash12/PID; duração em segundos desde `T0C`, incluindo etapas do wrapper (`bin/codex_pergunta.sh:265`, `:273`). |
| Sidecar `.id` | Wrapper remove o anterior e grava obra/papel/hash/PID/fim/SHA da resposta desta chamada (`bin/codex_pergunta.sh:401`, `:464`). | `_le_id` e `_triagem_sob_trava`, com conferência do SHA (`bin/disjuntor.py:879`, `:926`). Seu `fim` é carimbado antes do `fim` escrito pelo EXIT; não são necessariamente o mesmo instante (`bin/codex_pergunta.sh:464`, `:272`). |
| `triagem.log` | `--triagem`, sob trava da resposta e depois do estado (`bin/disjuntor.py:912`, `:930`, `:938`). | B3/rearme e vigia usam `_triagens`; última linha por volta prevalece (`bin/disjuntor.py:976`, `:1062`, `:1195`, `:997`). Volta completa contém fim/SHA; vigia descarta essas parcelas — achado 6. |
| Estado do disjuntor | `_com_estado`: flock exclusivo, temporário e replace (`bin/disjuntor.py:374`, `:392`). | Decisão, rearme e mensagens (`bin/disjuntor.py:1147`, `:1220`, `:1260`). `auto_usada` conta liberações históricas por obra; `auto_sobe` deduplica anúncios por SHA (`bin/disjuntor.py:1116`, `:1123`, `:1243`). |
| Fotografia B3 | `_fotografa` grava SHA das linhas e instante Unix em segundos (`bin/disjuntor.py:847`, `:856`). | Liberação exige linha nova/não consumida e **mtime ≥ disparado_em**; igualdade é aceita (`bin/disjuntor.py:1111`, `:1113`). |
| `disjuntor.log` | `trilha`; anúncio B3 depois da gravação do estado, ainda sob trava (`bin/disjuntor.py:220`, `:406`, `:1139`). | Painel conta eventos `LIBERA (auto)` desde `agora−86400`, limite inferior inclusivo (`bin/disjuntor.py:2087`, `:2107`, `:2114`). Isso não é a cota histórica `auto_usada` (`bin/disjuntor.py:158`). |
| `REGISTRO` / `LINHAS_REGISTRADAS` | Literais no teste (`app/core/tests/test_medida_turno_do_par.py:158`, `:254`). | `_identidade`: chave `(arquivo::função, sítio ou critério)`; linhas são ocorrências AST (`app/core/tests/test_medida_turno_do_par.py:518`, `:523`). Unidade da medida: função/critério mais parcelas nomeadas; uma função pode contar duas vezes (`app/core/medida_turno_do_par.py:10`). |
| Topo do RELATO | Porta automática `no_topo`; wrapper apenas emite a sugestão ESPERANDO (`app/core/esteira_vigia.py:865`; `bin/codex_pergunta.sh:397`). Autoria material de AUTOLIBERA/DESENHO: **[nao verificado]**. | Stop usa `_topo`; B3 usa o mesmo recorte, com dupla leitura estável (`bin/hook_stop_fila1.py:717`; `bin/disjuntor.py:805`, `:812`). |
| `hook_stop_espera.json` | Rotina de escrita retirada no diff fornecido **[nao verificado na base Git]**. | Restam lápide e teste de ausência; consumidor executável restante não localizado na cópia (`bin/hook_stop_fila1.py:538`; `bin/tests/test_hook_esperando_com_prova.sh:114`). Remoção do arquivo legado no host: **[nao verificado]**. |
| Livro/log da inteira | `pouso_inteira` grava log e seis campos no livro (`bin/classe_da_obra.py:250`, `:258`). | Parser usa último veredito/último `Ran N`; portão usa a **tree**, não o commit (`bin/classe_da_obra.py:224`, `:207`, `:216`). O leitor do livro não valida commit, obra, hora ou conteúdo do log (`bin/classe_da_obra.py:207`). |

Os carimbos de chamadas, triagem, trilha e livro usam hora local sem fuso gravado; igualdade dos fusos dos processos em produção: **[nao verificado]** (`bin/codex_pergunta.sh:272`; `bin/disjuntor.py:939`, `:220`; `bin/classe_da_obra.py:259`).

Por peça, os limites preservados que consegui conferir são:

- **ESPERANDO:** não encontrei afrouxamento além de tornar `arquivo=` inerte. Permanecem PID vivo, identidade do processo, ancestralidade e rejeição de campos repetidos; a prova do RELATO vem antes da liberação (`bin/hook_stop_fila1.py:605`, `:739`, `:1261`, `:1294`). PID vivo com arquivo parado/ausente está explicitamente coberto (`bin/tests/test_hook_esperando_com_prova.sh:117`).
- **AUTOLIBERA:** aceita somente FORMA/ERRO-BOBO, uma vez por obra; rearme preserva a cota. Não aceita `classe=DESENHO` (`bin/disjuntor.py:151`, `:778`, `:1115`, `:1206`).
- **Juízes/ordem/escritor:** classificador consulta a união base/cabeça de JUIZES; B3 mantém decisão/gravação/anúncio na mesma transição; catraca e ESTADO reutilizam o mesmo medidor (`bin/classe_da_obra.py:166`; `bin/disjuntor.py:1147`; `bin/turno_do_par_estado.py:36`). Nenhum segundo juiz de negócio foi identificado nesses caminhos; abrangência fora da cópia: **[nao verificado]**.
- **Consumidores externos:** o selo do censo mantém a tupla de ESPERANDO; ESTADO lê o medidor, sem depender de REGISTRO; novos selos entram pelo glob da régua (`bin/tests/test_censo_antes_da_vaga.sh:329`; `bin/gerar_estado.py:299`; `bin/regua.sh:138`). O classificador declara estar sem ligação à conduta; pouso real protegido por ele: **[nao verificado]** (`bin/classe_da_obra.py:35`).
- **p95/andável:** a cópia contém tetos em segundos por papel e exige ausência de outro item andável para PAREI devolver o turno (`bin/codex_pergunta.sh:95`; `bin/hook_stop_fila1.py:1151`). Medição que sustenta os percentis, execução dos selos completos e inteira final: **[nao verificado]**.

## 14/11 01:03 · sessao 8bdaa99be379 · - · - · origem=Ronald (tmux)

1. **BLOQUEIA — a catraca completa ainda reprova com total menor.** `_catraca()` permite queda e retorna aviso (`app/core/tests/test_medida_turno_do_par.py:503`), mas o mesmo teste continua exigindo igualdade de A1/B1 (`app/core/tests/test_medida_turno_do_par.py:563`), congelados em 6/82 (`app/core/tests/test_medida_turno_do_par.py:108`). Esses campos são contagens, não definições dos critérios (`app/core/medida_turno_do_par.py:441`). Contraprova em memória: substituir o corpo de `_alinhar_por_papel` por `return []` retirou o pareador de `app/escala/utils.py:520`: pareadores 9→8, derivações 85→85; `_catraca()` apenas avisou, porém o teste completo falhou por A1 6→5 (`app/core/tests/test_medida_turno_do_par.py:563`).

2. **FILA — aviso prometido ao RELATO termina em stderr; selo não cobra sua publicação.** `_aviso()` só escreve stderr (`app/core/tests/test_medida_turno_do_par.py:465`), chamado pelo teste principal (`app/core/tests/test_medida_turno_do_par.py:560`). Os novos MORDE conferem a lista devolvida por `_identidade()`, sem conferir stderr (`app/core/tests/test_medida_turno_do_par.py:587`, `app/core/tests/test_medida_turno_do_par.py:597`); o caso de redução testa apenas `_catraca()` com um número artificialmente decrementado (`app/core/tests/test_medida_turno_do_par.py:906`). Mutante em memória `_aviso=lambda msg:None` passou os seis casos relevantes. O filtro existente do pre-push elimina `CATRACA-AVISO` (`bin/pre-push.sh:146`). Escrita efetiva desses avisos no RELATO: **[nao verificado]**.

3. **FILA — documentação externa mantém o significado antigo de `arquivo=`.** O wrapper ainda descreve “jsonl cresce = prova de vida” (`bin/codex_pergunta.sh:51`) e LEIS mantém “pid/arquivo” (`app/docs/LEIS.md:66`); agora arquivo sem PID reprova e arquivo junto do PID é inerte (`bin/hook_stop_fila1.py:738`, `bin/hook_stop_fila1.py:748`).

**ESPERANDO:** não identifiquei afrouxamento adicional ao descarte da prova por arquivo: continuam a checagem de identidade/processo e ancestralidade (`bin/hook_stop_fila1.py:605`), rejeição de campos repetidos/ilegíveis (`bin/hook_stop_fila1.py:739`) e prova do RELATO antes da espera (`bin/hook_stop_fila1.py:1261`, `bin/hook_stop_fila1.py:1294`). Os casos PID vivo+arquivo parado/ausente estão explicitamente cobertos (`bin/tests/test_hook_esperando_com_prova.sh:117`).

**Mapa escritores/leitores:**

- **REGISTRO/LINHAS:** literais mantidos no código (`app/core/tests/test_medida_turno_do_par.py:158`, `app/core/tests/test_medida_turno_do_par.py:254`), lidos por `_identidade()` (`app/core/tests/test_medida_turno_do_par.py:518`). Chave: `(arquivo::funcao, sitio|criterio)`; valores de linhas: ocorrências AST (`app/core/tests/test_medida_turno_do_par.py:523`). Unidade: função por critério mais parcelas nomeadas, permitindo multiplicidade da função (`app/core/medida_turno_do_par.py:10`).
- **Saída da medida:** escritor único do resultado é `censo_fontes()` (`app/core/medida_turno_do_par.py:336`); consumidores externos importam o mesmo medidor (`bin/turno_do_par_estado.py:33`, `bin/gerar_estado.py:299`). Não dependem de REGISTRO nem do stderr da catraca (`bin/turno_do_par_estado.py:36`).
- **Topo RELATO:** wrapper emite a sugestão ESPERANDO em stderr (`bin/codex_pergunta.sh:397`); inserção material é manual ou pela porta `no_topo()` (`app/core/esteira_vigia.py:865`), cujo chamador de censo é `disjuntor.py:347`. Leitura central em `_topo()` (`bin/hook_stop_fila1.py:467`); ORÇAMENTO e Stop compartilham a fotografia por memo (`bin/hook_stop_fila1.py:758`, `bin/hook_stop_fila1.py:1052`, `bin/hook_stop_fila1.py:1294`).
- **Memória removida:** `hook_stop_espera.json` aparece apenas como lápide e verificação de ausência no recorte examinado (`bin/hook_stop_fila1.py:538`, `bin/tests/test_hook_esperando_com_prova.sh:114`). Nenhum consumidor executável restante localizado **[nao verificado fora da cópia pesquisada]**.
- **Chamador externo preservado:** o selo do censo continua consumindo a mesma tupla de três posições de `_esperando_com_prova()` (`bin/tests/test_censo_antes_da_vaga.sh:329`). Handoff e alarme usam o reconhecedor de PAREI-censo preservado (`bin/handoff_sessao.sh:129`, `bin/alarme_sessao_ociosa.py:137`).

## 14/11 01:03 · sessao 8aee5e252b0d · - · - · origem=Ronald (tmux)

Achados para consolidação, todos por leitura e probes em memória:

1. **BLOQUEIA — dinheiro sai `so leitor`.** O dinheiro é reconhecido pelo nome do caminho; o fallback para `.py` examina só tokens nas linhas alteradas (`bin/classe_da_obra.py:50`, `:145–151`). Confirmei que mudar somente `'adicional_noturno_pct': 20.0` para `40.0` em `app/core/regua_cct.py:42`, ou `/ 60` para `/ 30` em `app/ponto/services/dia_pago.py:180`, resulta em `so leitor`. Esses arquivos não estão no conjunto extraído do `JUIZES` atual pelo extrator (`bin/classe_da_obra.py:81–114`); a função do dinheiro está explícita em `app/ponto/services/dia_pago.py:4`, `:34–43`, `:178–186`. O selo não contém fixture desses casos (`bin/tests/test_gate_pela_classe.sh:104–117`).

2. **BLOQUEIA — mudança de universo sai `so leitor`.** `app/chamados/services/furo_sem_canal.py:70–76` restringe o universo à janela inclusiva de cada empresa. Alterar apenas os `<=` de `:75` para `<` muda o universo e resulta em `so leitor`, confirmado em memória. O instrumento perde o contexto ao extrair só linhas alteradas (`bin/classe_da_obra.py:117–125`); `UNIVERSO` enumera tokens e não reconhece essa fronteira (`:55–58`, `:145–151`). Também não há fixture desse caso no selo (`bin/tests/test_gate_pela_classe.sh:104–117`).

3. **BLOQUEIA — a árvore certificada pode divergir dos arquivos executados.** `_limpa` deliberadamente ignora não rastreados (`bin/classe_da_obra.py:236–237`), mas a suite monta o diretório do disco (`bin/suite.sh:203–204`, `:230–233`). A certificação só compara árvores de `HEAD` e esse estado parcial de limpeza (`bin/classe_da_obra.py:246–256`). Logo um módulo/teste não rastreado pode participar da execução sem pertencer à tree certificada. Ocorrência concreta em produção: **[nao verificado]**.

4. **FILA — selo não morde a emissão de certificados.** O caso de pouso escreve o livro sintético diretamente e chama apenas `--portao-pouso`; os últimos testes chamam somente o parser de log (`bin/tests/test_gate_pela_classe.sh:119–145`). Um mutante que troque a condição de recusa em `bin/classe_da_obra.py:256` por `if False:` passa esse selo sem testar a supressão de `rc`, limpeza ou igualdade de árvores. Remover a detecção de dinheiro em `:131` também não encontra fixture de dinheiro no selo (`bin/tests/test_gate_pela_classe.sh:104–117`). Não executei esses mutantes em disco: conclusão por alcance das chamadas citadas.

Mapa do certificado:

| Dado | Escritor | Leitor/significado |
|---|---|---|
| `logs/INTEIRA-FINAL.tsv` | `pouso_inteira`, append de seis campos (`bin/classe_da_obra.py:258–259`) | `trees_certificadas` aceita `>=6` campos, tree hex40 e N inteiro positivo; não verifica commit, obra, hora ou log (`:200–208`); `portao_pouso` compara tree de head (`:214–217`). |
| Tree/commit | `git rev-parse` em `:196–197`, `:246–247` | Certificado é por tree, não commit (`:255–259`); coerente com a comparação do portão (`:216–217`). | [nao verificado]
| N/veredito | Saída de `suite.sh`, processo em `:250–254` | Último `Ran N` e último `OK/FAILED`, independentes entre si (`:220–233`); exige rc0/OK/N>0 (`:256`). | [nao verificado]
| Hora | `time.strftime` sem timezone explícito (`:259`) | Leitor do livro não interpreta hora (`:206–208`); fuso real **[nao verificado]**. |
| Log | Arquivo por obra/tree12, truncado na nova execução (`:249–250`) | Parser lê ao terminar (`:253–254`); livro conserva só caminho (`:259`), sem hash do conteúdo. |

A busca incluindo arquivos ocultos encontrou referências à nova classe/livro somente no próprio instrumento e no selo; ausência de outros consumidores é limitada à cópia examinada. O próprio arquivo declara não estar conectado à conduta (`bin/classe_da_obra.py:35`); portanto não afirmaria que o pouso real já esteja protegido. Os selos novos entram no glob existente da régua (`bin/regua.sh:138–145`).

## 14/11 01:03 · sessao c15daa2ec8c3 · - · - · origem=Ronald (tmux)

1. **[bloqueia, fragilidade herdada] A exclusividade “linha do Ronald” depende de conduta.** `ERRO-BOBO` é aceito somente pelo conteúdo, sem autoria (`bin/disjuntor.py:772-775`); a cota verifica exclusivamente `tipo == 'auto'`, enquanto a linha antiga ganha precedência e libera sem gastar cota (`bin/disjuntor.py:1108-1130`). `guarda_chave` protege o comando de rearme, mas não a edição da linha no RELATO (`bin/disjuntor.py:239-265`); chamadas comuns sem `agent_id` saem do PreToolUse antes de qualquer outra verificação (`bin/disjuntor.py:1931-1932`). A reserva das linhas ao Ronald aparece na nova mensagem (`bin/disjuntor.py:1264-1267`), sem mecanismo correspondente nesses caminhos. O reconhecedor antigo já existia no diff fornecido; portanto, **não caracterizo como afrouxamento novo**. Em simulação exclusivamente em memória, estado parado com `auto_usada=['auto-antiga']` foi liberado por `ERRO-BOBO OBRA-X: texto do Code`, mantendo a cota esgotada — resultado previsto pelos ramos citados (`bin/disjuntor.py:1115-1130`).

2. **[fila] Caso 9 ainda admite mutante plausível de orientação proibida.** `_NOMEIA` e a condição específica de `DESENHO` distinguem caixa, enquanto sujeito e verbo usam `re.I` (`bin/tests/test_disjuntor.sh:1332-1335`, `bin/tests/test_disjuntor.sh:1356-1361`). Troquei somente em memória o trecho de `_recado_auto` para `FORMA, ERRO-BOBO ou desenho: o Code libera sozinho`; `b3_quem_libera` continuou retornando universo 4 e zero violações. O trecho alterável está em `bin/disjuntor.py:1252`; a combinação que deixa passar está em `bin/tests/test_disjuntor.sh:1351-1361`. Esse mutante ensina a liberar desenho apesar da proibição descrita pelo próprio selo (`bin/tests/test_disjuntor.sh:1327-1330`).

3. **AUTOLIBERA, isoladamente, preserva as restrições verificáveis.** Classes válidas são somente FORMA e ERRO-BOBO (`bin/disjuntor.py:151`, `bin/disjuntor.py:776-779`); a cota é uma por obra (`bin/disjuntor.py:152`, `bin/disjuntor.py:1240-1243`); o rearme não limpa `auto_usada` (`bin/disjuntor.py:1204-1208`); a segunda AUTO permanece parada, grava SOBE uma vez por SHA e deixa tentar outra candidata (`bin/disjuntor.py:1115-1121`). `classe=DESENHO` retornou lista vazia na execução do reconhecedor em memória, conforme o ramo citado (`bin/disjuntor.py:776-779`).

4. **Mapa dos campos B3 e seus sentidos:**

   | Campo/arquivo | Escritor | Leitores e significado |
   |---|---|---|
   | `logs/sessao/disjuntor_estado.json` | `_com_estado`: flock exclusivo, JSON temporário, replace (`bin/disjuntor.py:368-406`) | `ler_estado` rejeita objeto ilegível (`bin/disjuntor.py:412-423`); decisão B3 roda dentro de `_com_estado` (`bin/disjuntor.py:1146-1153`). |
   | `b3[obra].auto_usada` | Apenas a AUTO efetivamente aceita acrescenta SHA (`bin/disjuntor.py:1122-1123`) | `auto_esgotada`: quantidade de liberações durante a vida do estado da obra, ausência vale zero (`bin/disjuntor.py:1240-1243`); mensagens consultam a mesma fonte (`bin/disjuntor.py:1256-1262`). |
   | `auto_sobe` | SHA da AUTO recusada (`bin/disjuntor.py:1115-1120`) | A mesma condição evita repetir o anúncio por SHA (`bin/disjuntor.py:1116`). |
   | `consumidas`, `liberado`, `disparado` | Transição de liberação (`bin/disjuntor.py:1124-1127`) | `consumidas` impede reutilizar linha (`bin/disjuntor.py:1110-1112`); `liberado` impede recontar reprovação antiga (`bin/disjuntor.py:1073-1083`). |
   | `topo_no_disparo`, `disparado_em` | `_fotografa`, sob a transição (`bin/disjuntor.py:839-856`) | SHA normalizado da linha, instante Unix em segundos; liberação exige linha fora da fotografia e `mtime >= disparado_em` (`bin/disjuntor.py:692-694`, `bin/disjuntor.py:1110-1114`). Igualdade é aceita. |
   | Topo do RELATO | Escritor de máquina comum usa `_trava_relato` e temporário/replace (`app/core/esteira_vigia.py:847-875`, `app/core/esteira_vigia.py:916-927`); autoria humana das linhas liberadoras: **[nao verificado]** | B3 usa dois conteúdos integrais iguais, inode/mtime/tamanho estáveis e o recorte do Stop (`bin/disjuntor.py:788-815`). |
   | `logs/sessao/disjuntor.log` | `trilha`: data/hora local sem fuso explícito, detalhe normalizado; B3 anuncia após gravar estado e ainda sob flock (`bin/disjuntor.py:213-223`, `bin/disjuntor.py:1136-1147`, `bin/disjuntor.py:403-406`) | `ler` conta LIBERA `(auto)` desde `agora-86400`, com limite inferior inclusivo e sem limite superior (`bin/disjuntor.py:2085-2116`); `linha` apresenta a contagem (`bin/disjuntor.py:2376-2381`). |

   `auto_usada` é cota histórica por obra; `b3_libera.auto` é número de eventos na janela de 24 horas. O código distingue expressamente os dois sentidos (`bin/disjuntor.py:158-159`). Escritor e leitor da trilha usam respectivamente `datetime.now()` e `strptime(...).timestamp()`, ambos dependentes do fuso local do processo; igualdade dos fusos em produção é **[nao verificado]** (`bin/disjuntor.py:220`, `bin/disjuntor.py:2102`).

5. **Ordem e consumidores externos preservados nos caminhos examinados.** A decisão completa, gravação e anúncio ficam sob `_com_estado`; falha de gravação devolve como parada uma obra que teria sido liberada (`bin/disjuntor.py:358-409`, `bin/disjuntor.py:1136-1153`). Stop consulta `julga_b3` e usa `mensagem_b3` (`bin/hook_stop_fila1.py:1111-1116`); PreToolUse consulta `estado_b3` pela mesma decisão (`bin/disjuntor.py:1890-1896`, `bin/disjuntor.py:1174-1182`); wrapper consulta a porta `b3_parada` (`bin/disjuntor.py:1224-1236`). Não encontrei nova decisão B3 paralela nesses consumidores; execução real dos hooks e resultado dos selos completos são **[nao verificado]**.

As verificações executadas acima limitaram-se a importação com `python3 -B`, reconhecedores puros e transição com I/O substituído por mocks; não executei os selos que criam arquivos. Referências dos selos com escrita: `bin/tests/test_disjuntor.sh:1148-1164`.

## 14/11 01:00 · sessao 2e75ba60ed53 · - · - · origem=Ronald (tmux)

Achados de leitura, sem edições ou execução ORM:

- **R5 exige cuidado posicional:** Vigia publica oito colunas em `chamados/services/vigia_fio.py:633`. `backfill_fio_morto_e4b.py:252` desempacota exatamente oito campos. Uma nona coluna exige adaptar esse leitor, preservando os índices usados em `:79`, `:242` e `:250`. Nenhum consumidor CSV/template/JS dessa lista foi encontrado nas buscas realizadas; consumidores externos são [nao verificado].
- **Folha:** dict de cada dia nasce em `folha/fora_da_folha.py:241-242`, sem `nao_sei`. Não encontrei chamador de produção de `o_que_segura` nos arquivos Python/HTML pesquisados; há testes. O Vigia usa `dias_que_seguram`, em `vigia_fio.py:589`.
- **Universos diferem na entrada:** Folha parte de `classificar_export` e retidos (`fora_da_folha.py:130-149`); Vigia deduplica `(colab,data)` desses dias (`vigia_fio.py:585-592`); furo parte das células `furo/cobrado` nas empresas ativas (`furo_sem_canal.py:61`, `:79-85`). Paridade global sem alinhar recortes seria [nao verificado]. `dias_por_fio` da Folha conta dias (`fora_da_folha.py:190-192`), enquanto grupos contam colaboradores (`:195`).
- **R8 compatível com escritor real:** `ChamadoColaborador.criar` resolve célula somente quando o campo não foi passado (`chamados/models.py:1066-1068`) e grava em `:1096`. `_celula_de` retorna `.first()` do dia (`:1241`), podendo retornar `None`. O formulário entrega a data do catálogo em `services/abertura.py:191`. Alcance ORM efetivo segue [nao verificado].
- **Dia forte existente:** `dia_do_fato` prefere célula (`juizes.py:98-99`), catálogo (`:113-114`) e fallback `data_turno` (`:118`); `fortes=True` termina antes dos degraus fracos (`:127-128`). Catálogo lê a chave declarada em `catalogo/modulos.py:219-225`.
- **R11 alcançável por leitura:** `perguntas_do_app` chama `pergunta_viva` sem instante (`pergunta_no_app.py:62`); o cálculo chega à janela (`juizes.py:1455`), cujo default lê `date.today()` (`:592`). Com `agora`, `pergunta_viva` deriva o dia civil local (`:885-887`). Portanto a injeção opcional deve atravessar a cadeia inteira.
- **Escritores de campos persistentes:** `status_local`: `_gravar_estado`, `models.py:201-204`, atribuição `:228`; nascimento `:1096`. `silenciado_ate`: `arquivo.py:166/:171` e limpeza `:202/:210`. `celula_id`: nascimento acima e `services/vinculos.py:93-101`.
- **Borda do silêncio:** fila exclui somente `silenciado_ate > agora` (`painel.py:188-190`); igualdade já pertence à fila. Partição usa arquivado quando `> agora` e histórico quando `<= agora` (`verbos.py:51`), diferença que permanece na obra R6.
- **R2/R3:** natureza prioriza validar (`catalogo/natureza.py:46-49`); crédito separa respondidas/vivas (`raiox.py:143-144`) e destino vivo vence pai (`:150-153`). O filtro anterior exclui materializada com lavra sim (`:135-138`). Prova ORM com consulta e valor: [nao verificado].

## 14/11 00:56 · sessao eee42a348fa5 · E3-FORMA-SQL · construtor · origem=Code

Preciso resolver uma contradição antes de escrever: o brief v8 proíbe reutilizar `wt-codex-e3-forma-sql-195919`, mas esse é o único diretório autorizado pelo ambiente. A conformidade da cópia com essa exigência permanece **[nao verificado]**.

A leitura confirmou os pontos de R7 e R11:

- R7 descarta membros da fila sem verbo: [juizes.py:1562](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/juizes.py:1562).
- R11 chama C6 sem `agora`: [pergunta_no_app.py:60](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/services/pergunta_no_app.py:60); a janela pode consultar `date.today()`: [juizes.py:592](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/juizes.py:592).

**A sessão fornecerá a cópia nova ou autoriza expressamente aplicar a v8 nesta cópia, dispensando essa proibição?** A confirmação é necessária pela restrição explícita do próprio brief v8.

## 14/11 00:34 · sessao 64afa9b3793f · BUG-PAR · verificador · origem=Code

**A criação divergente está bloqueada no ponto do TOCTOU, mas resta um achado procedente: a espera da irmã pode virar recusa escrita da metade externa.** (`app/chamados/services/materializacao.py:1355`, `app/chamados/services/materializacao.py:1599`, `app/chamados/services/materializacao.py:1814`).

1. **Outro caminho de criação — descartável como bypass demonstrado.**

   O ramo `hora` compara os mesmos `ts/_tipo` que entrega a `registrar_batida`; irmã e passivo passam por `_materializar_uma`. O chokepoint cria com os valores recebidos. (`app/chamados/services/materializacao.py:1253`, `app/chamados/services/materializacao.py:1355`, `app/chamados/services/materializacao.py:1359`, `app/chamados/services/materializacao.py:1596`, `app/chamados/services/passivo_validado.py:537`, `app/ponto/registro_batida.py:175`).

   Existe outra criação em `data_hora`, sem portão 163, fixando tipo `E`. Contudo, as fábricas consultadas dão `hora` às metades de intervalo e `data_hora` à âncora. Uma metade efetivamente chegando por esse ramo é **[nao verificado]**; promovê-la a bypass seria **duvidoso**. (`app/chamados/services/materializacao.py:1460`, `app/chamados/models.py:3709`, `app/chamados/models.py:3714`, `app/chamados/models.py:3721`, `app/chamados/services/disputa_emissao.py:1371`).

   `escolha` não cria batida; o command do passivo delega ao reparador, e BUG-PAR-103 chama a devolução. (`app/chamados/services/materializacao.py:1529`, `app/chamados/management/commands/materializar_passivo_validado.py:113`, `app/chamados/management/commands/bug_par_103.py:85`).

2. **Rollback e engolimento — descartáveis no trajeto direto.**

   A premissa sobre `pendencia_admin/recusa_motivo` precisa de correção: o `save` deles ocorre **dentro** do `atomic` de `_materializar_uma`. A espera tardia atravessa esse bloco; portanto, desfaz essas escritas e as da irmã já materializada dentro dele. O carimbo final da própria metade vem depois do ponto que levanta. (`app/chamados/services/materializacao.py:1735`, `app/chamados/services/materializacao.py:1737`, `app/chamados/services/materializacao.py:1257`, `app/chamados/services/materializacao.py:1593`, `app/chamados/services/materializacao.py:1357`, `app/chamados/services/materializacao.py:1792`).

   A captura específica antecede `except Exception`; a mensagem da espera só entra em `erros` depois do `finally`, evitando a recusa nesse retorno direto. (`app/chamados/services/materializacao.py:1798`, `app/chamados/services/materializacao.py:1802`, `app/chamados/services/materializacao.py:1813`, `app/chamados/services/materializacao.py:1815`).

   Escritas realmente anteriores não pertencem a esse rollback. Por exemplo, a recusa `DATA_EM_DISPUTA` ocorre antes do bloco, mas retorna imediatamente e não alcança a espera tardia. (`app/chamados/services/materializacao.py:1720`, `app/chamados/services/materializacao.py:1721`, `app/chamados/services/materializacao.py:1735`).

3. **Irmã esperando — procedente, residual.**

   A irmã devolve `False` com a mensagem de espera. O wrapper transforma **qualquer** retorno falso em `_ParNaoFechou`, desfaz o savepoint da irmã e devolve um impedimento textual. A categoria “espera” se perde nessa fronteira. (`app/chamados/services/materializacao.py:1726`, `app/chamados/services/materializacao.py:1817`, `app/chamados/services/materializacao.py:1819`, `app/chamados/services/materializacao.py:1596`, `app/chamados/services/materializacao.py:1599`, `app/chamados/services/materializacao.py:1602`).

   A externa conserva `_viz`, acrescenta erro de **guarda de paridade** e segue pelas portas de recusa/carimbo. O `finally` grava o erro contendo o impedimento da irmã. Isso impede a batida divergente, mas **não respeita “espera não carimba recusa” para a externa**. (`app/chamados/services/materializacao.py:1290`, `app/chamados/services/materializacao.py:1299`, `app/chamados/services/materializacao.py:1303`, `app/chamados/services/materializacao.py:1338`, `app/chamados/services/materializacao.py:1349`, `app/chamados/services/materializacao.py:1814`, `app/chamados/services/materializacao.py:1555`).

   O caminho cabe justamente na corrida examinada: a externa passa inicialmente por reuso; na reconsulta precisa criar e chama a irmã. Na avaliação aninhada, a externa em voo fica excluída da seleção de irmã, enquanto a escrita considera sua batida projetada; assim, o portão tardio da irmã pode ser alcançado. Ocorrência em produção: **[nao verificado]**. (`app/chamados/services/materializacao.py:824`, `app/chamados/services/materializacao.py:1266`, `app/chamados/services/materializacao.py:1290`, `app/chamados/services/materializacao.py:146`, `app/chamados/services/materializacao.py:1594`, `app/chamados/services/materializacao.py:1282`, `app/chamados/services/materializacao.py:1355`).

4. **RED novo — procedente por leitura; execução do mutante [nao verificado].**

   Ele desliga somente o portão antecipado e exige retorno falso, mensagem de espera, nenhuma criação e foto preservada. Retirar o portão tardio deixa aberto o caminho para criar S `01:00` diante da declaração `01:20`; o controle vizinho confirma a criação nesse cenário quando a declaração coincide. (`app/chamados/tests/test_bug_par_projetado.py:1158`, `app/chamados/tests/test_bug_par_projetado.py:1190`, `app/chamados/tests/test_bug_par_projetado.py:1193`, `app/chamados/tests/test_bug_par_projetado.py:1206`, `app/chamados/tests/test_bug_par_projetado.py:1207`, `app/chamados/tests/test_bug_par_projetado.py:1210`).

   A cobertura não alcança o residual aninhado: a fixture carimba ambas as perguntas, enquanto selecionar a irmã exige `materializada_em` nulo. A foto também omite `pendencia_admin` e `materializada_por`. (`app/chamados/tests/test_bug_par_projetado.py:1163`, `app/chamados/services/materializacao.py:145`, `app/chamados/tests/test_bug_par_projetado.py:990`).

| Classe no delta d2 | Classificação e evidência |
|---|---|
| 2º juiz | **Descartável:** reutiliza `_espera_do_aval_163`. `app/chamados/services/materializacao.py:847`, `app/chamados/services/materializacao.py:1355`. |
| 2º escritor | **Descartável:** mantém `registrar_batida`; irmã usa a mesma unidade. `app/chamados/services/materializacao.py:1359`, `app/chamados/services/materializacao.py:1596`. |
| Derivação paralela | **Descartável:** compara os valores já derivados. `app/chamados/services/materializacao.py:1253`, `app/chamados/services/materializacao.py:1355`. |
| Ordem de trava | **Descartável neste delta:** comparação sem trava nova; porta trava antes de reler/materializar. `app/chamados/services/materializacao.py:725`, `app/chamados/services/materializacao.py:1921`. |
| Campo com dois significados | **Descartável como novidade persistida; procedente na perda de significado do retorno:** espera e falha comum chegam como `False` e recebem o mesmo tratamento. `app/chamados/services/materializacao.py:1687`, `app/chamados/services/materializacao.py:1819`, `app/chamados/services/materializacao.py:1599`. |

## 14/11 00:33 · sessao d00fa833a560 · - · - · origem=Ronald (tmux)

Q1 — **Descartável como bypass concreto demonstrado.**

- A criação de resposta `hora` passa pelo portão imediatamente antes de `registrar_batida`: `app/chamados/services/materializacao.py:1355-1360`. A irmã usa `_materializar_uma`, portanto alcança o mesmo escritor: `app/chamados/services/materializacao.py:1596`.
- O passivo também usa `_materializar_uma`: `app/chamados/services/passivo_validado.py:537`. Seu command delega a `reparar`: `app/chamados/management/commands/materializar_passivo_validado.py:113`.
- BUG-PAR-103 apenas chama a porta de devolução, sem criar batida: `app/chamados/management/commands/bug_par_103.py:85`.
- `escolha` pode ter motivo intervalo por reabertura administrativa, mas seu ramo não cria batida: `app/chamados/services/acoes_disputa.py:252-268`; `app/chamados/services/materializacao.py:1529-1533`.
- Existe outra criação em `data_hora`, sem portão 163 e com tipo fixo `E`: `app/chamados/services/materializacao.py:1448-1461`. Entretanto, a fábrica de intervalo usa `hora`, enquanto `data_hora` nasce para `ancora_ausente`: `app/chamados/models.py:3709-3723`; `app/chamados/services/disputa_emissao.py:1371-1375`. Uma metade de intervalo efetivamente chegando como `data_hora` permanece **[nao verificado]**; seria **duvidoso** promover essa hipótese a bypass.
- O chokepoint cria com o timestamp/tipo recebidos, sem derivação alternativa: `app/ponto/registro_batida.py:170-176`. A criação por bulk em `semear` exige banco de ensaio: `app/ponto/registro_batida.py:349-353`.

Q3 — **Procedente a perda da natureza “espera” na propagação; sem criação divergente nesse caminho.**

A irmã que retorna `False` vira `_ParNaoFechou`; seu relato é convertido em impedimento textual: `app/chamados/services/materializacao.py:1599-1603`. A metade externa conserva a vizinha ofensora e entra na recusa de paridade: `app/chamados/services/materializacao.py:1299-1307`. Pode gravar `_carimbar_recusa` diretamente e o `finally` também carimba havendo erro: `app/chamados/services/materializacao.py:1338-1341`; `app/chamados/services/materializacao.py:1813-1814`. Isso preserva o impedimento de criar a batida, mas não propaga a espera como espera à metade externa: `app/chamados/services/materializacao.py:1341`.

## 14/11 00:33 · sessao 3f970a8ac939 · - · - · origem=Ronald (tmux)

**PROCEDENTE — a espera da irmã pode virar recusa escrita da externa.** `_materializar_uma(q)` devolve `False` com a mensagem de espera; `_gravar_irma_do_par` converte esse resultado em `_ParNaoFechou` e devolve apenas um impedimento textual (`app/chamados/services/materializacao.py:1726`, `:1798`, `:1815`, `:1596`, `:1599`, `:1601`).

A externa mantém `_viz`, acrescenta erro de “guarda de paridade” e escreve recusa; o `finally` ainda grava o erro que contém o impedimento da irmã. Portanto, a irmã permanece preservada pelo rollback, mas a externa pode ter `recusa_motivo` alterado (`app/chamados/services/materializacao.py:1290`, `:1299`, `:1303`, `:1338`, `:1813`, `:1554`).

Caminho concreto dedutível do código, sem afirmar ocorrência em produção:

- Externa S declarou `01:00`; irmã E declarou `02:20`, marco `02:00`. A primeira leitura da externa encontra S `01:00` aprovada e retorna `ja_satisfeita`; essa batida deixa de existir antes da reconsulta do escritor (`app/chamados/services/materializacao.py:824`, `:1266`).
- Com E `18:30` e S `06:30`, o par projetado resolve a paridade, conforme a geometria da fixture (`app/chamados/tests/test_bug_par_projetado.py:61`, `:69`, `:112`, `:114`).
- A externa chama a irmã; no derivador da irmã, a externa em voo não pode ser selecionada como irmã, produzindo `bloqueada_paridade`, que não aciona a espera antecipada (`app/chamados/services/materializacao.py:146`, `:1659`, `:832`, `:853`, `:756`).
- O escritor da irmã considera a externa projetada e alcança o portão tardio, que levanta pela divergência E `02:00` × E `02:20`; o resultado segue a conversão em recusa externa descrita acima (`app/chamados/services/materializacao.py:1594`, `:1282`, `:1355`, `:1357`, `:1602`).

**DESCARTÁVEL — engolimento por `except Exception` nesse trajeto.** A captura específica de `_EsperaNaCriacao` antecede a genérica; o problema é a perda da categoria “espera” ao retornar `False` e convertê-lo em `_ParNaoFechou` (`app/chamados/services/materializacao.py:1798`, `:1802`, `:1599`).

**Rollback:** `pendencia_admin`/`recusa_motivo` de `_materializar_resposta_hora` são escritos dentro do atomic do chamador, portanto entram no rollback provocado pela espera; não estão antes desse atomic. Escritas realmente anteriores, como a recusa `DATA_EM_DISPUTA`, pertencem a outro ramo que retorna antes de alcançar o raise tardio (`app/chamados/services/materializacao.py:1735`, `:1737`, `:1257`, `:1720`, `:1721`).

## 14/11 00:33 · sessao 2491ebd33a06 · - · - · origem=Ronald (tmux)

- **RED novo: procedente, por análise estática.** Ele desliga apenas o portão antecipado e exige retorno falso, mensagem de espera, nenhuma batida e foto preservada (`app/chamados/tests/test_bug_par_projetado.py:1206-1211`). O mutante sem o portão tardio alcançaria `registrar_batida` após derivar S 01:00 para resposta 01:20 (`app/chamados/services/materializacao.py:659-662,1253,1355-1359`); o controle com declaração 01:00 confirma a criação nesse caminho (`app/chamados/tests/test_bug_par_projetado.py:1189-1194`). Execução do mutante nesta cópia: **[nao verificado]**.

- **Cobertura limitada:** o teste não produz uma corrida real, substitui `espera_na_escrita` por `''` (`app/chamados/tests/test_bug_par_projetado.py:1206`). A foto não cobre `pendencia_admin` nem `materializada_por` (`app/chamados/tests/test_bug_par_projetado.py:990-992`). A fixture carimba ambas as perguntas, enquanto a seleção da irmã exige `materializada_em` nulo; portanto esse RED não exercita a irmã esperando (`app/chamados/tests/test_bug_par_projetado.py:1163-1166`; `app/chamados/services/materializacao.py:145`).

- **Segundo juiz, segundo escritor e derivação paralela: descartáveis neste delta.** A nova consulta usa o mesmo `_espera_do_aval_163`, com `ts` e `_tipo` já derivados, e mantém `registrar_batida` como criação (`app/chamados/services/materializacao.py:725,847,861,1253,1355-1359`).

- **Ordem de trava: descartável neste delta.** O portão só compara valores; a porta existente trava antes de reler e materializar (`app/chamados/services/materializacao.py:725-731,1921-1931`).

- **Campo com dois significados: descartável como novidade.** `_espera_na_criacao` é variável local e vira mensagem após o `finally`; não há campo persistido novo (`app/chamados/services/materializacao.py:1687,1798-1799,1815-1817`).

- **Irmã esperando: candidato procedente.** O retorno falso da irmã vira `_ParNaoFechou` e impedimento textual; a metade externa segue a recusa de paridade, podendo reescrever sua recusa ou carimbar identidade (`app/chamados/services/materializacao.py:1596-1603,1290,1299-1307,1338-1349,1813-1814`).

## 14/11 00:27 · sessao 5cc64f975557 · CORTE-DAS-TRAVAS · leitura · origem=Code

1. **GATE — linha 1: classificador e chamadores.** **Furo se a semente for reaproveitada literalmente:** `classifica.py` usa repositório fixo, agrega commits por token da obra, reconhece escrita somente nas linhas adicionadas e chama `git show` sem conferir o retorno. Uma falha pode produzir “nada”, depois convertido em “so leitor”. O novo chamador precisa tratar erro/saída inválida como escritor, sem aproveitar uma classificação parcial (`/home/ronald/saas-hasner/logs/AUDITORIA-TRAVAS/medida_1311/classifica.py:3`, `:25`, `:27`, `:34`, `:39`, `:45`, `:51`). Os caminhos atuais são distintos: volta chama `suite.sh`; régua e pre-push executam suas próprias suites. Integrar somente `suite.sh` deixa dois caminhos fora do instrumento (`bin/volta_intermediaria.sh:32`, `bin/regua.sh:165`, `bin/pre-push.sh:186`).

2. **GATE — linha 2: pré-opus e pouso.** **Afrouxa uma trava existente:** hoje a inteira verde precede o opus; o seletor intermediário admite escapar dependências transitivas, URLs, signals e templates, declarando justamente a inteira pré-opus como defesa. Adiá-la para o pouso remove essa defesa da revisão final (`CLAUDE.md:377`, `bin/vizinhos_da_volta.py:12`). **Falha aberta adicional, por análise estática:** se não conseguir abrir `$OUT.log`, `volta_intermediaria.sh` pode não executar nenhuma etapa e sair zero: inicializa `rc_final=0`, ignora a falha do bloco redirecionado e altera o resultado somente por marcadores encontrados no log (`bin/volta_intermediaria.sh:16`, `:42`, `:43`, `:47`). Nem motivo nem rc zero bastam como prova de inteira: `--seco` sai zero sem executá-la, e são permitidos filtros `--tag`, `--exclude-tag` e `-k` (`bin/suite.sh:116`, `:183`, `:189`).

3. **GATE — linha 3: limite, JUIZES e universo.** **Nada contra a precedência declarada; o risco está na descoberta dos sítios.** `JUIZES` inclui atribuição inicial e acréscimos por subscrito; copiar o leitor do selo do mapa perde esses acréscimos (`app/core/juizes.py:182`, `:727`, `:1044`, `:1192`, `:1310`; `bin/tests/test_mapa_juizes.sh:27`). Outros consumidores: gerador do mapa, gerador do diagrama e medidor de turno; este último depende do formato AST da declaração e lança `LookupError` se não reconhecer a autoridade (`bin/gerar_mapa_juizes.py:19`, `:40`, `:66`; `app/core/management/commands/gerar_diagrama.py:344`; `app/core/medida_turno_do_par.py:246`). Há escritor do **arquivo** pelo integrador, baixando `PENDENTES`, sem ser escritor de novos juízes (`app/core/integrador_lote.py:428`, `:436`, `:442`). Mudar a representação pode quebrar o medidor; mudar referências sem atualizar o mapa pode deixar seu selo vermelho (`app/core/medida_turno_do_par.py:250`, `:257`; `bin/tests/test_mapa_juizes.sh:45`). O contraexemplo de queryset está no achado 11.

4. **GATE — linha 4: medida e selo.** **Furo intrínseco da medição: nada.** Porém o “FAILED só por forma = zero” não demonstra preservação do universo: o próprio RELATO registra E3 como “só leitor”, LEITOR sem diferenças e universo aumentado em 79 (`app/docs/RELATO.md:14`, `:127`). O selo precisa morder falha de classificação, seleção parcial, prova de hash antigo e execução simulada; conferir apenas classe textual ou nome do motivo não cobre os caminhos dos achados 1–2 (`bin/suite.sh:116`, `:183`; `/home/ronald/saas-hasner/logs/AUDITORIA-TRAVAS/medida_1311/classifica.py:27`). RED e mutantes do novo selo: **[nao verificado]**.

5. **GATE — linha 5: bloqueio do push e desligamento.** **Criar o selo em `bin/tests/` sozinho não garante bloqueio.** A régua pode retornar “JA VERDE” antes do loop de selos; o pre-push não executa esse loop e chama nominalmente seus selos de push. O novo selo precisa estar nesse caminho antes do atalho (`bin/regua.sh:84`, `:93`, `:138`; `bin/pre-push.sh:12`, `:16`, `:66`). Existe ainda a saída explícita `SKIP_TESTS=1` antes das guardas; portanto “selo vermelho bloqueia o push” não é absoluto no código atual (`bin/pre-push.sh:5`). Autenticação de “só corte Ronald” pelo novo instrumento: **[nao verificado]**.

6. **COMBOIO — linha 1: peças, leitores e escritores.** **Há dois endereços de triagem diferentes:** o escritor existente `--triagem` grava `logs/codex/triagem.log`, e B3 lê esse mesmo endereço. Escritor automático do endereço `logs/AUDITORIA-TRAVAS/triagem.log`: **nada localizado [nao verificado]** (`bin/disjuntor.py:138`, `:908`, `:932`). Redirecionar somente a escrita deixa B3 lendo a trilha antiga; trocar também a leitura exige preservar o histórico, pois trilha ausente/truncada depois de consumida vira `NAO_SEI` (`bin/disjuntor.py:924`, `:934`, `:941`). **Outro consumidor importante:** `_itens_andaveis` alimenta tanto o Stop quanto a exigência de censo do disjuntor. Encolher indevidamente seu resultado pode transformar obra obrigada a censo em obra fora dessa obrigação (`bin/hook_stop_fila1.py:267`, `:273`; `bin/disjuntor.py:1623`, `:1634`, `:1701`).

7. **COMBOIO — linha 2: espera e liberação única de B3.** **O código deste hash ainda libera DESENHO automaticamente:** o reconhecedor aceita `desenho` e `erro-bobo`, e a transição consome ambos. Não basta acrescentar FORMA deixando a porta antiga (`bin/disjuntor.py:726`, `:737`, `:747`, `:1025`). Hoje a proteção é contra reutilização do **SHA da linha**, não contra segunda liberação da obra: outra linha pode liberar outra reprovação (`bin/disjuntor.py:1027`, `:1034`). Os consumidores a preservar são wrapper, PreToolUse de Codex/verificador final e recados do Stop (`bin/codex_pergunta.sh:390`; `bin/disjuntor.py:1826`, `:1842`; `bin/hook_stop_fila1.py:1455`). A tentativa anterior está registrada como estacionada, sem pouso, por selo enganado por tokens; o RELATO não prova que aquela mudança esteja neste hash (`app/docs/RELATO.md:328`, `:334`).

8. **COMBOIO — linha 3: falhas e limites.**
   
   - **ESPERANDO afrouxa:** retirar a prova do arquivo faz “PID vivo + arquivo parado” passar onde hoje bloqueia. Se “PID vivo” significar mera existência, perde também identificação do produtor, exclusão de ancestral/zumbi e proteção contra troca de processo (`bin/hook_stop_fila1.py:604`, `:613`, `:620`, `:627`, `:831`; `bin/tests/test_hook_esperando_com_prova.sh:141`). A infraestrutura também serve ao censo, handoff e alarme; mudar sua semântica sem preservar esses consumidores pode fazê-los discordar (`bin/hook_stop_fila1.py:803`; `bin/handoff_sessao.sh:129`; `bin/alarme_sessao_ociosa.py:137`).
   - **Erro pode liberar:** a prova local de espera captura exceções como ausência de prova, mas o `main` do Stop falha aberto. Falha ao gravar o contador, por exemplo, alcança essa saída (`bin/hook_stop_fila1.py:837`, `:1433`, `:1460`). B3 preserva erro como `NAO_SEI`, recusado pelo wrapper (`bin/disjuntor.py:1072`, `:1081`; `bin/codex_pergunta.sh:391`).
   - **Catraca só total afrouxa:** remove identidade, composição e linhas congeladas. Já existe mutante cuja composição muda com os totais passando, mas que a trava de composição barra (`app/core/tests/test_medida_turno_do_par.py:499`, `:539`, `:545`, `:560`). O publicador do ESTADO também lê o medidor e suas listas; reduzir o teste não exige apagar esses dados (`bin/turno_do_par_estado.py:36`, `:59`, `:71`).
   - **Timeout:** hoje é p95 **mais margem medida**, equivalente ao máximo da janela +1, com tetos 1929/657/1179 por papel; p95 puro mudaria essa regra e os selos que fixam os tetos (`bin/codex_pergunta.sh:85`, `:95`; `bin/tests/test_codex_pergunta.sh:464`, `:475`). **Liberação por timeout: nada nesse fechamento:** o wrapper devolve o rc e somente cria sidecar de sucesso com rc zero e resposta desta chamada (`bin/codex_pergunta.sh:463`, `:469`).

9. **COMBOIO — linha 4: antes/depois e mutantes.** **Nada de furo na exigência em si.** Há mordidas existentes que a mudança precisa deliberadamente substituir, não apenas apagar: PID vivo com arquivo parado, composição alterada sem aumento dos totais e timeout cobrindo efetivamente o processo Codex (`bin/tests/test_hook_esperando_com_prova.sh:141`; `app/core/tests/test_medida_turno_do_par.py:560`; `bin/tests/test_codex_pergunta.sh:467`). Para B3, selo por presença de palavras já falhou em tentativa real registrada; o mutante precisa exercitar a decisão de liberação (`app/docs/RELATO.md:334`). Antes/depois completo das novas peças: **[nao verificado]**.

10. **COMBOIO — linha 5: pausa e corte.** **`pausar.sh` não pausa as peças por si:** escreve o arquivo indicado e a trilha. É necessário nomear a sentinela e seus consumidores (`bin/pausar.sh:31`, `:45`, `:47`). Hoje o disjuntor lê `DISJUNTOR_DESLIGADO`; o wrapper lê `logs/pausas/codex-construtor` somente no ramo construtor. Uma pausa nova, sem leitores, não interrompe esses caminhos (`bin/disjuntor.py:209`; `bin/codex_pergunta.sh:204`, `:407`). O script tampouco autentica Ronald: `--soltar` aceita o nome informado, inclusive `?` por omissão (`bin/pausar.sh:24`). Pausar sem afrouxar as guardas exige definir esse efeito explicitamente; desligar o disjuntor tem efeito de desativação (`bin/disjuntor.py:210`).

11. **(c), comum aos contratos — contraexemplo real: SIM.** Commit **`5319676ee87bc86705b95d9a53a8a4c7de5f1d7c`**, E3-FORMA-SQL: `q_vivas` passa a excluir `veredito=''` do predicado que absolvia a célula; isso alarga o universo de perguntas retornadas, sem escrita de negócio nessa alteração (`5319676ee:app/chamados/models.py:3404`, `:3408`, `:3411`). A classificação da medida registra esse commit como **“so leitor”**, e sua regra identifica dinheiro por caminhos e escritor por operações adicionadas (`/home/ronald/saas-hasner/logs/AUDITORIA-TRAVAS/medida_1311.md:11`, `:80`). Os arquivos do commit são models e dois testes de chamados; folha/motor/holerite não entram na lista retornada pelo classificador (`/home/ronald/saas-hasner/logs/AUDITORIA-TRAVAS/medida_1311/classifica.py:27`, `:30`, `:35`, `:37`). O **+79 é medida da obra E3**, não atribuição isolada a esse commit (`app/docs/RELATO.md:127`). O limite novo “queryset/universo = escritor” cobre precisamente esse contraexemplo; o classificador antigo não cobre (`5319676ee:app/chamados/models.py:3411`; `/home/ronald/saas-hasner/logs/AUDITORIA-TRAVAS/medida_1311.md:80`).

12. **(d), comum aos contratos — recibo da inteira final.** **Existe escritor de prova por árvore:** `bin/regua.sh` grava `.regua_stamp` com `REGUA_ARVORE`, status, horário e resumo; parcial `--only` não carimba. Isso não contém obra, motivo nem SHA do commit pousado (`bin/regua.sh:181`, `:196`). `suite.sh` termina em `exec`, e o pre-push termina a execução bem-sucedida sem gravar recibo próprio daquele SHA (`bin/suite.sh:226`; `bin/pre-push.sh:186`, `:198`, `:200`). **Escritor do recibo exato pedido: nada localizado [nao verificado].**

    **Pouso proposto:** registro externo ao commit, por exemplo `logs/suites-inteiras/<SHA>.json`, escrito pela porta executora após a inteira final, contendo obra, classe, SHA/tree testados, seleção efetiva, resultado e log; é proposta, ainda sem implementação **[nao verificado]**. O selo deve confrontá-lo com `PUSH_SHA` e barrar prova ausente, parcial ou de outro hash; o pre-push já obtém esse SHA (`bin/pre-push.sh:33`).

    **Não basta reutilizar o atalho atual:** ele compara o carimbo à árvore viva e depois compara ao commit enviado apenas `.py/.html`. Por análise estática, outra ref com diferença somente em JS/CSS/dados pode satisfazer essa igualdade e pular o arsenal sem inteira daquele conteúdo (`bin/pre-push.sh:65`, `:68`, `:70`; `bin/arvore_do_push.sh:89`). O novo selo precisa conferir a identidade integral do conteúdo enviado, além de rejeitar `--seco` e filtros de seleção (`bin/suite.sh:116`, `:183`).

## 14/11 00:25 · sessao d5a4f5238c05 · - · - · origem=Ronald (tmux)

1. **GATE linha 1 — implementação/caller:** o mecanismo atual de suite intermediária é `bin/volta_intermediaria.sh:32`; a suite inteira aceita apenas motivo `pre-opus` ou `pouso`, com recusa rc 2 (`bin/suite.sh:173-180`). A presença de `bin/classe_da_obra.py` e de `logs/AUDITORIA-TRAVAS/medida_1311/classifica.py` neste snapshot não foi confirmada por arquivo acessível `[nao verificado]`.

2. **GATE linha 2 — afrouxamento real de cobertura:** hoje a regra exige inteira verde antes do opus (`CLAUDE.md:377-378`), e o seletor intermediário declara que **dependências transitivas, URLs, signals, templates e consumidores de migrations de outro app escapam; a defesa é a inteira pré-opus** (`bin/vizinhos_da_volta.py:12-17`). Substituí-la por intermediária remove essa defesa até o pouso; o alcance da classificação precisa absorver esses caminhos (`bin/vizinhos_da_volta.py:12-17`).

   **Falha aberta no wrapper reutilizado:** `rc_final=0` nasce antes do bloco (`bin/volta_intermediaria.sh:16`); o bloco inteiro depende de abrir `$OUT.log`, sem conferir o resultado dessa abertura (`bin/volta_intermediaria.sh:42`); só marcadores alteram o rc (`bin/volta_intermediaria.sh:43-44`), e erros de `echo`/`touch` não mudam a saída (`bin/volta_intermediaria.sh:45-47`). Portanto, saída em diretório inexistente/sem permissão pode impedir todos os testes e terminar rc 0 — inferência estática, sem execução (`bin/volta_intermediaria.sh:16`, `bin/volta_intermediaria.sh:42-47`).

3. **GATE linha 3 — JUIZES e leitores/escritores:** não basta ler o primeiro literal: há `JUIZES =` (`app/core/juizes.py:182`) e famílias acrescentadas por subscript (`app/core/juizes.py:727`, `app/core/juizes.py:1044`, `app/core/juizes.py:1192`, `app/core/juizes.py:1310`). O selo atual do mapa reconhece somente atribuição cujo alvo é o nome `JUIZES`; copiá-lo perde as famílias acrescentadas (`bin/tests/test_mapa_juizes.sh:27-40`).

   Outros consumidores são o gerador do MAPA, que lê o dicionário e escreve `MAPA-JUIZES.md` (`bin/gerar_mapa_juizes.py:19`, `bin/gerar_mapa_juizes.py:40`, `bin/gerar_mapa_juizes.py:66`), o gerador do diagrama (`app/core/management/commands/gerar_diagrama.py:341-348`) e a medida de turno, cujo leitor AST lança `LookupError` se a autoridade deixar de ser identificável (`app/core/medida_turno_do_par.py:246-258`). O integrador também escreve o arquivo `juizes.py`, para baixar `PENDENTES` (`app/core/integrador_lote.py:428-442`). Alterar o formato da declaração pode quebrar a medida; alterar referências sem regenerar o mapa deixa o selo vermelho (`app/core/medida_turno_do_par.py:250-257`, `bin/tests/test_mapa_juizes.sh:45-53`).

4. **GATE linha 4 — prova de execução e contraexemplo documentado:** o RELATO já registra um “só leitor” E3-FORMA-SQL cujo universo mudou **+79**, e memória **440→1233 MB** (`app/docs/RELATO.md:14`). Isso é prova documental do caso, mas não é confirmação independente do diff/commit; o hash real deve ser encontrado no histórico pelo agente principal `[nao verificado]`.

5. **GATE linha 5 — selo vermelho pode não bloquear push:** o loop de selos `bin/tests/test_*.sh` está na régua (`bin/regua.sh:138-144`), **depois** da saída antecipada “JA VERDE” (`bin/regua.sh:84-94`). O pre-push explica que esse loop não roda nele e chama nominalmente os selos de push antes do atalho (`bin/pre-push.sh:12-19`). Logo, criar apenas `bin/tests/test_gate_pela_classe.sh` não garante a exigência; precisa estar no caminho de push antes do atalho (`bin/pre-push.sh:16`, `bin/pre-push.sh:66-70`). Há ainda o bypass explícito atual `SKIP_TESTS=1`, antes de todas essas guardas (`bin/pre-push.sh:5`).

6. **Item d — escritor atual e lugar proposto:**
   - Existe escritor de prova **de árvore**, `.regua_stamp`: `REGUA_ARVORE`, status, tempo e resumo são gravados pela régua (`bin/regua.sh:196-203`); a árvore é calculada antes da corrida (`bin/regua.sh:45`). O arquivo é ignorado pelo Git (`.gitignore:60`).
   - `bin/suite.sh` termina em `exec` da suite e não grava prova final própria nesse caminho (`bin/suite.sh:226-233`). O pre-push testa o `PUSH_SHA` (`bin/pre-push.sh:33`, `bin/pre-push.sh:39`, `bin/pre-push.sh:186-188`), mas chega à mensagem de liberação sem escrever um registro de inteira daquele hash (`bin/pre-push.sh:198-200`).
   - Uma proposta verificável é um registro externo ao commit, indexado pelo **SHA pousado**, com obra, classe, árvore, seleção inteira, resultado e caminho do log; escritor único na porta que executa a inteira, gravado somente após sucesso e conferência da identidade final. Esse formato/escritor ainda é proposta `[nao verificado]`. [nao verificado]
   - Não reutilizar cegamente o atalho atual: ele compara o stamp com a árvore **viva** (`bin/pre-push.sh:65-67`), enquanto a igualdade com `PUSH_SHA` verifica somente `.py/.html` (`bin/arvore_do_push.sh:85-91`). Push de outra ref com diferença somente em JS/CSS/dado pode satisfazer esse teste e pular o arsenal do SHA enviado — inferência estática (`bin/pre-push.sh:33`, `bin/pre-push.sh:68-70`, `bin/arvore_do_push.sh:89-90`).

## 14/11 00:25 · sessao 34f969fbf415 · - · - · origem=Ronald (tmux)

1. **COMBOIO linha 1 — chamadores e trilhas.** O escritor atual de triagem é `disjuntor.py --triagem`: exige sidecar do wrapper, papel `verificador`, SHA correspondente e trava; grava em **`logs/codex/triagem.log`**, não em `logs/AUDITORIA-TRAVAS/triagem.log` (`bin/disjuntor.py:138`, `bin/disjuntor.py:869`, `bin/disjuntor.py:892`, `bin/disjuntor.py:908`). O consumidor de B3 lê aquela mesma constante (`bin/disjuntor.py:932`, `bin/disjuntor.py:981`). **Se a mudança simplesmente redirecionar a escrita para o endereço do contrato**, B3 continuará lendo a trilha antiga; é necessário esclarecer se o novo arquivo é medição adicional ou substituição [inferência dos endereços em `bin/disjuntor.py:138`, `bin/disjuntor.py:908`, `bin/disjuntor.py:932`]. O wrapper fornece o lastro com `<resposta>.id` somente após rc=0 e resposta desta chamada (`bin/codex_pergunta.sh:461`, `bin/codex_pergunta.sh:463`).

2. **COMBOIO linha 2 — quem espera e autoridade de B3.** Hoje o Code consegue liberar **DESENHO também**: o reconhecedor aceita `desenho` e `erro-bobo`; a transição consome qualquer dos dois (`bin/disjuntor.py:724`, `bin/disjuntor.py:737`, `bin/disjuntor.py:1025`, `bin/disjuntor.py:1031`). A mensagem de proteção da chave ensina expressamente “o Code libera por DESENHO ou ERRO-BOBO” (`bin/disjuntor.py:227`). Logo, implementar “DESENHO só Ronald” apenas na nova autoliberação deixa aberta a porta antiga [inferência de `bin/disjuntor.py:737`, `bin/disjuntor.py:1025`]. Os consumidores são: wrapper → `b3_parada` (`bin/codex_pergunta.sh:390`, `bin/disjuntor.py:1131`); PreToolUse → `_nega_b3` para Codex e verificador final (`bin/disjuntor.py:1776`, `bin/disjuntor.py:1826`, `bin/disjuntor.py:1842`); Stop → `julga_b3`, acrescentando o recado (`bin/hook_stop_fila1.py:1204`, `bin/hook_stop_fila1.py:1455`). Hoje o consumo é **por SHA de linha**, permitindo outra linha distinta depois de nova reprovação; não há limite geral de uma liberação por obra nessa transição (`bin/disjuntor.py:1027`, `bin/disjuntor.py:1034`). A tentativa do item 2 está documentada como estacionada na raia `8773091a3`, sem pouso/sem v6; a conduta declarada é DESENHO fora da porta do Code (`app/docs/RELATO.md:328`, `app/docs/RELATO.md:334`).

3. **COMBOIO linha 3 — furos concretos.**
   - **ESPERANDO:** “só PID vivo” afrouxa **se significar substituir** a prova atual por mera existência do PID. Hoje há exclusão de ancestral, zumbi, PID reciclado, produtor genérico e executável/script divergente (`bin/hook_stop_fila1.py:604`, `bin/hook_stop_fila1.py:613`, `bin/hook_stop_fila1.py:620`, `bin/hook_stop_fila1.py:625`). Quando PID e arquivo são declarados, **ambos** precisam valer; remover a prova do arquivo faria vivo+arquivo parado passar, caso hoje explicitamente bloqueado (`bin/hook_stop_fila1.py:831`, `bin/tests/test_hook_esperando_com_prova.sh:141`). Retirar somente a modalidade `arquivo=` restringiria os caminhos hoje aceitos, sem esse afrouxamento (`bin/hook_stop_fila1.py:829`, `bin/hook_stop_fila1.py:834`). Censo também passa por esta infraestrutura: procura wrapper vivo da obra/papel corretos, e sua prova alimenta Stop, alarme e handoff (`bin/hook_stop_fila1.py:735`, `bin/hook_stop_fila1.py:763`, `bin/hook_stop_fila1.py:803`, `bin/alarme_sessao_ociosa.py:127`, `bin/tests/test_hook_esperando_com_prova.sh:190`).
   - **Falha libera:** exceção local da prova de espera vira “sem prova” (`bin/hook_stop_fila1.py:837`), porém o `main` do Stop termina com **falha aberta** (`bin/hook_stop_fila1.py:1460`). Por exemplo, falha na gravação do contador chega a essa fronteira; alterações novas precisam permanecer contidas para não liberar por erro (`bin/hook_stop_fila1.py:1433`, `bin/hook_stop_fila1.py:1460`). B3, ao contrário, preserva `NAO_SEI` como recusa nos consumidores principais (`bin/disjuntor.py:1072`, `bin/disjuntor.py:1081`, `bin/disjuntor.py:1141`, `bin/disjuntor.py:1781`).
   - **Catraca só total:** remove travas existentes de composição, identidade e linhas, permitindo trocar os sítios mantendo os números (`app/core/tests/test_medida_turno_do_par.py:499`, `app/core/tests/test_medida_turno_do_par.py:539`, `app/core/tests/test_medida_turno_do_par.py:545`). Já existe mutante demonstrando composição alterada com `_catraca` passando, mas `_composicao` vermelha (`app/core/tests/test_medida_turno_do_par.py:560`). Essa catraca entra na volta por `bin/selos_globais_da_volta.txt:1`; o ESTADO utiliza o mesmo medidor, mas publica as listas e ilegíveis independentemente do congelamento do teste (`bin/turno_do_par_estado.py:9`, `bin/turno_do_par_estado.py:36`, `bin/turno_do_par_estado.py:59`, `bin/turno_do_par_estado.py:71`).
   - **Timeout:** o atual é **p95 + margem medida = máximo +1**, separado por papel: 1929/657/1179 segundos; p95 puro seria mudança da regra existente (`bin/codex_pergunta.sh:85`, `bin/codex_pergunta.sh:95`). O mesmo valor rege espera da trava e execução, em sequência (`bin/codex_pergunta.sh:92`, `bin/codex_pergunta.sh:389`). Timeout não ganha sidecar de sucesso, pois este exige rc=0 (`bin/codex_pergunta.sh:463`). Furo de liberar por timeout: **nada nesse fechamento** (`bin/codex_pergunta.sh:463`, `bin/codex_pergunta.sh:469`). Os selos atuais fixam os tetos e a recusa de papel sem medida (`bin/tests/test_codex_pergunta.sh:459`, `bin/tests/test_codex_pergunta.sh:473`, `bin/tests/test_codex_pergunta.sh:485`).

4. **COMBOIO linha 4 — medidas/selos.** Há selos atuais que perderiam o significado se a alteração só apagasse as asserções: PID+arquivo parado precisa bloquear; composição alterada sem alteração do total precisa ficar vermelha; timeout precisa preservar teto e relação com o processo Codex (`bin/tests/test_hook_esperando_com_prova.sh:141`, `app/core/tests/test_medida_turno_do_par.py:560`, `bin/tests/test_codex_pergunta.sh:467`). O histórico registra como o selo da tentativa B3 item 2 foi enganado por literal que continha os tokens esperados, motivo do estacionamento (`app/docs/RELATO.md:334`). Antes/depois e mutantes novos do COMBOIO: **[nao verificado]**.

5. **COMBOIO linha 5 — pausar.** `pausar.sh` grava o **arquivo passado pelo chamador** e apenda trilha; ele não implementa consumidores da pausa (`bin/pausar.sh:31`, `bin/pausar.sh:39`, `bin/pausar.sh:47`). Hoje o disjuntor consulta `DISJUNTOR_DESLIGADO`, e o wrapper consulta `logs/pausas/codex-construtor` somente no papel construtor (`bin/disjuntor.py:209`, `bin/codex_pergunta.sh:204`, `bin/codex_pergunta.sh:407`). Assim, “pausar por `pausar.sh`” precisa nomear a sentinela e os leitores de cada peça; criar um arquivo de pausa novo, sozinho, não interrompe esses caminhos [inferência de `bin/pausar.sh:31`, `bin/disjuntor.py:209`, `bin/codex_pergunta.sh:204`]. O comando `--soltar` aceita qualquer `quem`, com default `?`; o script não autentica Ronald (`bin/pausar.sh:24`).

## 13/11 23:33 · sessao c8de3c12712d · BUG-PAR · verificador · origem=Code

**Há uma lacuna procedente na proteção da escrita concorrente:** `ja_satisfeita` libera a porta, mas o escritor consulta novamente a batida e pode passar de reuso para criação sem repetir declarado × gravado. No fluxo sem mudança dos insumos, não encontrei esse escape. (`app/chamados/services/materializacao.py:757`, `app/chamados/services/materializacao.py:823`, `app/chamados/services/materializacao.py:1266`, `app/chamados/services/materializacao.py:1352`.) Reprodução concorrente, configuração efetivamente carregada e ocorrência em produção: **[nao verificado]**.

**1. Escritores, leitores e semântica**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| Item: `dono` | `explicar_pergunta` em `app/chamados/services/explicador.py:97`, `app/chamados/services/explicador.py:101`, `app/chamados/services/explicador.py:109`, `app/chamados/services/explicador.py:117`; os três donos de não gravação passam por `_sem_gravar`, em `app/chamados/services/explicador.py:43`. | Agrupamento do resumo em `app/chamados/services/explicador.py:184`; testes em `app/chamados/tests/test_explicador_disputa.py:54`, `app/chamados/tests/test_explicador_disputa.py:63`, `app/chamados/tests/test_explicador_disputa.py:77`, `app/chamados/tests/test_explicador_disputa.py:101`. |
| Item: `quem_resolve` | `_sem_gravar`, lendo a tabela: `app/chamados/services/explicador.py:43`. | Modal em `app/templates/chamados/partials/modal_fio.html:468`; teste em `app/chamados/tests/test_bug_par_projetado.py:1229`. |
| Item: `gravou` | Emite `False` no não gravado e na resolução por recusa: `app/chamados/services/explicador.py:43`, `app/chamados/services/explicador.py:126`. | Desenlace testa `is False`: `app/chamados/services/desenlace.py:67`; testes em `app/chamados/tests/test_bug_par_projetado.py:267`, `app/chamados/tests/test_bug_par_projetado.py:283`, `app/chamados/tests/test_bug_par_projetado.py:1207`. |
| Item: `estado` | `app/chamados/services/explicador.py:43`, `app/chamados/services/explicador.py:97`, `app/chamados/services/explicador.py:101`, `app/chamados/services/explicador.py:109`, `app/chamados/services/explicador.py:117`. | Resumo em `app/chamados/services/explicador.py:178`; desenlace em `app/chamados/services/desenlace.py:70`; modal em `app/templates/chamados/partials/modal_fio.html:463`, `app/templates/chamados/partials/modal_fio.html:468`. |
| `SEM_GRAVAR_POR_DONO` | Definição em `app/chamados/services/explicador.py:32`. | Item lê o resolvedor em `app/chamados/services/explicador.py:43`; resumo lê a frase em `app/chamados/services/explicador.py:200`. `cadastro`, cuja frase é `None`, recebe tratamento próprio em `app/chamados/services/explicador.py:190`. |
| `por_que_nao_fecha.resumo` | `app/chamados/services/explicador.py:180`, `app/chamados/services/explicador.py:203`. | Fio transporta em `app/chamados/services/fio.py:348`; desenlace lê em `app/chamados/services/desenlace.py:83`; modal apresenta em `app/templates/chamados/partials/modal_fio.html:462`. |
| Retorno de `espera_na_escrita` | Produz detalhe apenas para `ESPERA_DECLARADO_X_GRAVADO`; demais estados produzem `''`: `app/chamados/services/materializacao.py:753`, `app/chamados/services/materializacao.py:757`. | `_materializar_uma`, que interrompe a escrita quando recebe texto: `app/chamados/services/materializacao.py:1713`. |
| Retorno de `_estado_pelo_derivador` | Produz `estado` e `detalhe`: `app/chamados/services/materializacao.py:818`, `app/chamados/services/materializacao.py:822`, `app/chamados/services/materializacao.py:825`, `app/chamados/services/materializacao.py:847`, `app/chamados/services/materializacao.py:864`. | Leitores diretos: `espera_na_escrita` e `estado_aplicacao_pergunta`, em `app/chamados/services/materializacao.py:756`, `app/chamados/services/materializacao.py:806`. Pela testemunha, chegam ao contador, explicador e fio: `app/chamados/services/materializacao.py:338`, `app/chamados/services/explicador.py:124`, `app/chamados/services/fio.py:374`. |

**Descartável — tradução antiga de `dono` para TI/DP.** O modal lê `quem_resolve` pronto; item e resumo consultam a mesma tabela, que distingue a espera pela obra. O desenlace não interpreta `dono`: usa `gravou`, `estado` e `resumo`. (`app/templates/chamados/partials/modal_fio.html:468`, `app/chamados/services/explicador.py:35`, `app/chamados/services/explicador.py:43`, `app/chamados/services/explicador.py:200`, `app/chamados/services/desenlace.py:67`, `app/chamados/services/desenlace.py:83`.)

**Duvidoso — ESMERIL residual na orientação da Central.** Quando C7 permite fechar e existe item sem gravação, a ação geral ainda sugere conferir/corrigir marcação e fechar; o item específico manda esperar a obra. Não é uma tradução errada de `dono`, mas permanece uma orientação genérica pouco adequada à espera. O teste novo cobre C7 bloqueando, não esse ramo. (`app/chamados/services/desenlace.py:63`, `app/chamados/services/desenlace.py:79`, `app/chamados/services/explicador.py:135`, `app/chamados/tests/test_bug_par_projetado.py:1205`, `app/chamados/tests/test_bug_par_projetado.py:1224`.)

**2. Escrita, reuso e passivo**

**Procedente estrutural — o reuso não fica assegurado entre julgamento e execução.** Contraexemplo construído pela leitura do código, condicionado à visibilidade da alteração concorrente:

1. Declarado `S 01:20`, marco `S 01:00`, existente aprovada `S 01:10`: o derivador responde `ja_satisfeita` e a porta libera sem comparar a declaração com o plantio. (`app/chamados/services/materializacao.py:659`, `app/chamados/services/materializacao.py:757`, `app/chamados/services/materializacao.py:823`.)
2. Entre essa consulta e a do escritor, outro POST pode inverter a existente para `E 01:10`. A porta de flip chama `anotar(tipo)`, cujo `save` não adquire previamente a trava do colaborador ou da batida. (`app/ponto/views.py:3266`, `app/ponto/services/flip_batida.py:30`, `app/ponto/registro_batida.py:272`.)
3. A trava do materializador cobre Colaborador, Disputa, Chamado e Pergunta; não fixa a Batida consultada. (`app/chamados/models.py:2687`, `app/chamados/models.py:2696`, `app/chamados/models.py:2709`, `app/chamados/models.py:2713`.)
4. O escritor consulta novamente, já não encontra `S` e pode criar `S 01:00`. Com vizinhas `E`, a paridade não impede esse `S`; dez minutos também excedem a janela máxima de colisão de cinco minutos. Não há nova comparação com `S 01:20` nesse ramo. (`app/chamados/services/materializacao.py:121`, `app/chamados/services/materializacao.py:1266`, `app/chamados/services/materializacao.py:1278`, `app/chamados/services/materializacao.py:1352`, `app/ponto/registro_batida.py:31`.)
5. O juiz posterior pode aceitar a batida no slot derivado do marco, portanto não garante rollback dessa divergência. (`app/chamados/juizes.py:752`, `app/chamados/juizes.py:760`.)

Autocommit/isolamento efetivos, reprodução desse cronograma e ocorrência real: **[nao verificado]**.

Para insumos estáveis, as demais suspeitas ficam assim:

| Estado | Classificação e fundamento |
|---|---|
| `ja_satisfeita` | **Descartável como criação sequencial:** mesma busca, mesmo tipo e ±20 minutos; existente pula a criação. O reuso pode anotar `pergunta_origem`, sem mudar hora/tipo. (`app/chamados/juizes.py:235`, `app/chamados/juizes.py:244`, `app/chamados/services/materializacao.py:1266`, `app/chamados/services/materializacao.py:1278`, `app/chamados/services/materializacao.py:1394`.) |
| `invalida` | **Descartável:** derivador e escritor usam `_ts_tipo_do_plantio`; ambos param com `ts=None`. (`app/chamados/services/materializacao.py:820`, `app/chamados/services/materializacao.py:822`, `app/chamados/services/materializacao.py:1253`, `app/chamados/services/materializacao.py:1259`.) |
| `bloqueada_data` | **Descartável:** a unidade já recusa antes do portão 163. (`app/chamados/services/materializacao.py:1687`, `app/chamados/services/materializacao.py:1707`.) |
| `bloqueada_competencia` | **Descartável para criação:** o chokepoint reaplica `competencia_fechada`, com `fato_de_chao=False`. (`app/chamados/services/materializacao.py:1355`, `app/ponto/registro_batida.py:156`, `app/ponto/registro_batida.py:159`.) |
| `bloqueada_paridade` | **Descartável no caminho estável examinado:** escritor reaplica vizinhança/par; sem liberação, retorna sem plantar. (`app/chamados/services/materializacao.py:1282`, `app/chamados/services/materializacao.py:1290`, `app/chamados/services/materializacao.py:1341`, `app/chamados/services/materializacao.py:1350`.) |
| `aplicavel` | **Descartável:** só aparece depois da comparação da metade ou das duas metades. (`app/chamados/services/materializacao.py:847`, `app/chamados/services/materializacao.py:861`.) |

**Procedente a diferença de critérios, sem violação literal demonstrada:** o derivador exige existente `aprovada`; o escritor reutiliza qualquer existente retornada. A busca de janela/tipo é a mesma, mas “satisfeita” não tem exatamente a mesma exigência. (`app/chamados/services/materializacao.py:824`, `app/chamados/services/materializacao.py:1272`.)

**Descartável — escape do passivo pelo carimbo.** A escrita chama diretamente `_estado_pelo_derivador`, evitando o atalho `aplicada`. Passivo, laço, irmã e validação chegam à mesma unidade. (`app/chamados/services/materializacao.py:756`, `app/chamados/services/materializacao.py:1713`, `app/chamados/services/passivo_validado.py:537`, `app/chamados/services/materializacao.py:1913`, `app/chamados/services/materializacao.py:1584`, `app/chamados/services/validacao.py:587`.) Os commands delegam ao reparador/laço; o reconciliador examinado trata chamados e perguntas. (`app/chamados/management/commands/materializar_passivo_validado.py:68`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:147`, `app/chamados/reconciliador.py:1433`.)

**3. Rótulo e prefetch**

**Descartável — escritor conhecido criando BUG-PAR-103 fora de intervalo.** A porta dedicada exige o predicado que restringe `_PAR_DO_INTERVALO`, antes de registrar a devolução. Existe escritor genérico de devolução copiando `recusa_motivo`; ele não fabrica esse prefixo. Linha histórica fora do universo: **[nao verificado]**. (`app/chamados/services/validacao.py:264`, `app/chamados/services/materializacao.py:274`, `app/chamados/services/validacao.py:279`, `app/chamados/services/validacao.py:213`.)

**Descartável como caminho comprovado de cache stale no mesmo request.** O fio carrega decisões antes dos leitores; o POST processa a ação antes de reconstruir o fio. A devolução BUG-PAR-103 localizada é chamada pelo command, que passa o ID à porta e recarrega a pergunta. (`app/chamados/services/fio.py:261`, `app/chamados/services/fio.py:346`, `app/chamados/services/fio.py:374`, `app/chamados/views.py:954`, `app/chamados/views.py:889`, `app/chamados/management/commands/bug_par_103.py:85`, `app/chamados/services/validacao.py:263`.)

**Duvidoso como fragilidade:** `registrar` usa `bulk_create` sem invalidar instâncias antigas, enquanto o rótulo agora aceita o cache. Chamador concreto retendo a instância prefetched através dessa escrita: **[nao verificado]**. (`app/chamados/models.py:4039`, `app/chamados/services/materializacao.py:291`.)

**4. Classes de reprovação**

- **Segundo juiz/derivação paralela: descartável nos hunks.** A extração compartilha o corpo; fechamento continua delegado a C7. (`app/chamados/services/materializacao.py:756`, `app/chamados/services/materializacao.py:806`, `app/chamados/services/explicador.py:177`, `app/core/juizes.py:283`.)
- **Segundo escritor fora da porta: descartável nos caminhos examinados.** Plantio continua por `_materializar_uma` e `registrar_batida`. (`app/chamados/services/materializacao.py:1584`, `app/chamados/services/materializacao.py:1724`, `app/chamados/services/materializacao.py:1352`.)
- **Relógio novo fora da borda: descartável no corpo extraído.** Ele deriva dos insumos e consulta as autoridades existentes. (`app/chamados/services/materializacao.py:809`, `app/chamados/services/materializacao.py:820`, `app/chamados/services/materializacao.py:858`.)
- **Trava: procedente a lacuna de estabilidade descrita acima; inversão nova de ordem não demonstrada.** (`app/chamados/models.py:2687`, `app/chamados/models.py:2713`, `app/ponto/registro_batida.py:272`.)
- **Selo sem mordida: descartável por inspeção das assertivas.** Reuso exige sucesso, estado exato e nenhuma batida nova; o teste semântico verifica resolvedor, resumo e HTML. Execução RED/GREEN: **[nao verificado]**. (`app/chamados/tests/test_bug_par_projetado.py:1100`, `app/chamados/tests/test_bug_par_projetado.py:1101`, `app/chamados/tests/test_bug_par_projetado.py:1102`, `app/chamados/tests/test_bug_par_projetado.py:1224`, `app/chamados/tests/test_bug_par_projetado.py:1229`, `app/chamados/tests/test_bug_par_projetado.py:1232`.)

## 13/11 23:31 · sessao e025fea219d0 · - · - · origem=Ronald (tmux)

**O candidato concorrente é procedente como janela de falha no código; reprodução e ocorrência em produção: [nao verificado].** A conclusão depende de o POST do flip executar com autocommit e isolamento que permita enxergar o UPDATE entre as duas leituras; configuração efetivamente carregada: [nao verificado].

O interleaving proposto passa pelas guardas examinadas:

1. Declarado `S 01:20`, plantio derivado `S 01:00`, existente aprovada `S 01:10`: o derivador retorna `ja_satisfeita`, liberando o portão sem comparar declarado × plantio — `app/chamados/services/materializacao.py:823`, `:824`, `:825`, `:757`.
2. O POST autorizado chama `flip_tipo`; este altera `S→E` pela porta `anotar`, sem adquirir previamente trava de Colaborador ou Batida — `app/ponto/views.py:3263`, `:3266`; `app/ponto/services/flip_batida.py:15`, `:30`; `app/ponto/registro_batida.py:270`, `:272`.
3. A trava do materializador cobre Colaborador/Disputa/Chamado/Pergunta, sem travar a Batida consultada — `app/chamados/models.py:2687`, `:2696`, `:2709`, `:2713`.
4. O escritor consulta novamente e agora não encontra a `S`; tenta criar `S 01:00`, sem repetir declarado × plantio — `app/chamados/services/materializacao.py:1266`, `:1278`, `:1352`. Vizinhas `E` não impedem esse plantio `S` — `:121`, `:123`.
5. A `E 01:10` está a dez minutos do plantio, fora da janela máxima de colisão de cinco minutos — `app/ponto/registro_batida.py:21`, `:31`, `:109`, `:110`. A criação mantém o `timestamp` e o `tipo` recebidos — `:175`.
6. O juiz posterior não assegura rollback por divergência da declaração: ele deriva o slot e pode aceitar a batida plantada no marco — `app/chamados/juizes.py:752`, `:760`. O C6 aceita a pergunta já validada — `:1401`, `:1402`.

Portanto, **“`ja_satisfeita` não grava” é verdadeiro apenas enquanto a batida usada para essa decisão continuar reutilizável**; o código decide isso em uma leitura e executa com outra, sem preservar a primeira testemunha — `app/chamados/services/materializacao.py:823`, `:1266`. O delta retirou a comparação direta que barrava o plantio divergente mesmo nessa mudança de cenário — [nao verificado quanto ao histórico Git; consta no delta fornecido].

## 13/11 23:29 · sessao e025fea219d0 · - · - · origem=Ronald (tmux)

Revisão somente leitura da escrita concluída; não encontrei contraexemplo demonstrado pelo código em que esta alteração, sozinha, deixe uma metade gravar hora/tipo divergente. Execução e ocorrência em produção: [nao verificado].

| Suspeita | Classificação | Evidência |
|---|---|---|
| `ja_satisfeita` passa e grava a hora do marco | **Descartável no fluxo estável** | Derivador e escritor chamam a mesma `_batida_existente_na_janela`: `app/chamados/services/materializacao.py:823`, `:1266`; existente faz reuso e pula `registrar_batida`: `:1272`, `:1278`, `:1352`. A consulta usa mesmo tipo e ±20 minutos: `app/chamados/juizes.py:235`, `:244`. |
| Derivador e escritor usam critérios idênticos de “satisfeita” | **Procedente a diferença, sem demonstrar violação literal** | Derivador exige `aprovada`: `app/chamados/services/materializacao.py:824`; escritor reutiliza qualquer resultado não nulo: `:1272`. Isso pode mudar o estado anunciado, mas esse ramo não grava hora/tipo: `:1278`. |
| `invalida` passa pelo portão e grava | **Descartável no fluxo estável** | Ambos derivam pela mesma função; `ts is None` encerra o derivador e o escritor: `app/chamados/services/materializacao.py:820`, `:822`, `:1253`, `:1259`. |
| `bloqueada_data` passa e grava | **Descartável** | A unidade recusa DATA_EM_DISPUTA antes do novo portão: `app/chamados/services/materializacao.py:1686`, `:1687`, `:1707`, `:1708`; derivador aplica a mesma condição: `:816`. |
| `bloqueada_competencia` passa e grava divergente | **Descartável para criação normal** | Derivador consulta `competencia_fechada`: `app/chamados/services/materializacao.py:764`; `registrar_batida(... fato_de_chao=False)` aplica novamente essa autoridade antes de criar: `:1355`; `app/ponto/registro_batida.py:156`, `:161`, `:175`. Dedup anterior ao guard somente retorna linha existente: `:138`, `:145`. |
| `bloqueada_paridade` passa e grava divergente | **Descartável no fluxo normal examinado** | Escritor reaplica vizinhança e tenta o mesmo `par_do_intervalo`: `app/chamados/services/materializacao.py:1282`, `:1290`; sem par permitido, retorna sem plantio: `:1299`, `:1341`, `:1350`. Colisão entre irmãs desfaz o par: `:1361`, `:1375`, `:1379`. |
| `aplicavel` grava diferente | **Descartável no fluxo estável** | Antes de retornar `aplicavel`, o derivador compara ambas as metades ou a metade avulsa: `app/chamados/services/materializacao.py:847`, `:861`; comparação confere hora local e tipo: `:708`, `:714`. |
| Passivo carimbado escapa pelo atalho `aplicada` | **Descartável** | `espera_na_escrita` chama diretamente `_estado_pelo_derivador`, ignorando o atalho do carimbo: `app/chamados/services/materializacao.py:756`; a unidade recusa antes do plantio: `:1713`, `:1716`; passivo chama essa unidade: `app/chamados/services/passivo_validado.py:537`. |

Há uma diferença real de contexto na projeção: o escritor lê `_PAR_PROJETADO` em `app/chamados/services/materializacao.py:1282`, enquanto `_estado_pelo_derivador` consulta a vizinhança sem essa projeção em `:828`. **Duvidoso como vulnerabilidade isolada**: a metade inicial compara o par inteiro antes da recursão (`:847`), e `_gravar_irma_do_par` volta ao mesmo juiz, exigindo irmã validada (`:1575`). O contexto é instalado apenas na chamada recursiva e restaurado no `finally` (`:1582`, `:1584`, `:1586`). Não encontrei caminho concreto que ultrapasse essa proteção no estado estável; concorrência capaz de alterar os insumos entre essas leituras: [nao verificado].

O censo de chamadas de produção para escrita de resposta leva à unidade: validação em `app/chamados/services/validacao.py:466`, `:587`; laço da disputa em `app/chamados/services/materializacao.py:1913`; irmã em `:1584`; passivo em `app/chamados/services/passivo_validado.py:537`. O backfill delega ao laço (`app/chamados/management/commands/backfill_fio_morto_e4b.py:147`), e o command de passivo importa o reparador (`app/chamados/management/commands/materializar_passivo_validado.py:68`). O command BUG-PAR-103 devolve perguntas, pela porta de validação, sem plantar diretamente (`app/chamados/management/commands/bug_par_103.py:81`, `:85`).

Não localizei flip de tipo nos efeitos imediatos examinados: a gravação recebe `ts, _tipo` (`app/chamados/services/materializacao.py:1352`), e o chokepoint os usa diretamente no `create` (`app/ponto/registro_batida.py:175`). Os signals acionam núcleo, cartório e reconciliador (`app/ponto/signals.py:35`, `:132`, `:207`); o corpo de `reconciliar_chamados_colab` retrata/reatta chamados e perguntas (`app/chamados/reconciliador.py:1433`, `:1434`, `:1442`).

Os testes existentes têm assertivas pertinentes para reuso e passivo: sucesso `ja_no_espelho` e nenhuma batida nova em `app/chamados/tests/test_bug_par_projetado.py:1100`, `:1101`, `:1102`; passivo divergente recusado sem alterar a foto em `:1180`, `:1183`, `:1184`; controle positivo do passivo igual ao declarado em `:1193`, `:1194`. Resultado RED/GREEN e execução desses testes: [nao verificado].

## 13/11 23:28 · sessao 8d1c142b9b8f · - · - · origem=Ronald (tmux)

**Pergunta 3 — descartável como regressão comprovada.**

- O escritor dedicado só cria `DEVOLUCAO/BUG-PAR-103` depois de `paridade_absolvida_bug_par_103` aprovar; esse predicado exige `p.motivo in _PAR_DO_INTERVALO`. Portanto a saída antecipada do rótulo preserva o universo desse escritor. Referências: `app/chamados/services/validacao.py:263`, `app/chamados/services/validacao.py:264`, `app/chamados/services/validacao.py:279`, `app/chamados/services/validacao.py:282`, `app/chamados/services/materializacao.py:274`.

- Existe outro escritor genérico de `DEVOLUCAO`, que copia `recusa_motivo`; ele não fabrica o identificador BUG-PAR-103. Uma linha histórica com esse prefixo fora de intervalo permanece **[nao verificado]**. Referências: `app/chamados/services/validacao.py:210`, `app/chamados/services/validacao.py:213`.

- O único chamador produtivo localizado da devolução BUG-PAR-103 é o comando, passando identificador à porta, que carrega uma instância nova. Não há nesse caminho instância prefetched sendo reutilizada para desenhar o rótulo. Referências: `app/chamados/management/commands/bug_par_103.py:81`, `app/chamados/management/commands/bug_par_103.py:85`, `app/chamados/services/validacao.py:263`.

- O fio faz `Prefetch('decisoes')` antes de chamar o explicador e calcular `estado_aplicacao`; o rótulo é chamado por `_estado_pelo_derivador`. Referências: `app/chamados/services/fio.py:255`, `app/chamados/services/fio.py:261`, `app/chamados/services/fio.py:346`, `app/chamados/services/fio.py:373`, `app/chamados/services/materializacao.py:813`.

- No POST do modal, a ação é processada antes de reconstruir o fio. A reconstrução consulta novamente perguntas e decisões; o render parcial também chama novamente esse construtor. Não localizei caminho produtivo com decisão BUG-PAR-103 escrita depois do prefetch e leitura posterior na mesma instância. Referências: `app/chamados/views.py:951`, `app/chamados/views.py:954`, `app/chamados/views.py:889`, `app/chamados/services/fio.py:535`, `app/chamados/services/fio.py:538`, `app/chamados/services/fio.py:487`.

**Duvidoso como fragilidade genérica:** `DecisaoPergunta.registrar` usa `bulk_create` e não invalida caches de instâncias previamente carregadas; o rótulo agora pode consumir esses caches. Falta um chamador concreto retendo a instância através da escrita para converter isso em achado procedente. Referências: `app/chamados/models.py:4037`, `app/chamados/models.py:4039`, `app/chamados/services/materializacao.py:291`.

O teste existente verifica o rótulo após a devolução e numa montagem posterior do modal; não reproduz retenção de instância prefetched através da escrita. Referências: `app/chamados/tests/test_bug_par_projetado.py:794`, `app/chamados/tests/test_bug_par_projetado.py:797`, `app/chamados/tests/test_bug_par_projetado.py:802`. Execução desses testes: **[nao verificado]**.

## 13/11 23:28 · sessao 79cd380c31aa · - · - · origem=Ronald (tmux)

Censo do explicador e avaliação semântica:

| Valor | Escritor | Leitor |
|---|---|---|
| `dono` | `explicar_pergunta`: `colaborador`, `admin`, `ninguem` em `app/chamados/services/explicador.py:97`, `:101`, `:109`, `:117`; os três donos de não gravação passam por `_sem_gravar`, `:43`, chamados em `:134`, `:139`, `:142`. | Agrupamento do resumo em `app/chamados/services/explicador.py:184`; testes de contrato anteriores em `app/chamados/tests/test_explicador_disputa.py:54`, `:63`, `:77`, `:101`; teste impede falso `ninguem` em `app/chamados/tests/test_o261_paridade_respeita_a_lampada.py:207`. |
| `quem_resolve` | Somente `_sem_gravar`, consultando tabela em `app/chamados/services/explicador.py:43`. | Modal lê texto pronto em `app/templates/chamados/partials/modal_fio.html:468`; teste novo em `app/chamados/tests/test_bug_par_projetado.py:1229`. |
| `gravou` | `False` no não gravado em `app/chamados/services/explicador.py:43`; também no resolvido pela recusa em `:126`. Não há emissão explícita de `True` nesses caminhos (`:94`, `:117`, `:127`). | Desenlace considera exclusivamente `is False`, `app/chamados/services/desenlace.py:67`, e separa causas pelo `estado`, `:70`; testes em `app/chamados/tests/test_bug_par_projetado.py:267`, `:283`, `:1207`. O teste de caminho positivo explicita item sem `gravou` em `:291`. |
| `estado` do item | `app/chamados/services/explicador.py:43`, `:97`, `:101`, `:109`, `:117`. | Resumo exclui resolvida em `:178`; desenlace classifica `validada_sem_gravar` em `app/chamados/services/desenlace.py:70`; modal filtra e escolhe mensagens em `app/templates/chamados/partials/modal_fio.html:463`, `:466`, `:467`, `:468`. |
| `SEM_GRAVAR_POR_DONO` | Definição única em `app/chamados/services/explicador.py:32`. | Item lê índice `[1]` em `:43`; resumo percorre frases em `:200` e aplica contagem em `:202`. `cadastro` tem frase `None`, sendo tratado especificamente em `:190` a `:199`. |
| `resumo` | `app/chamados/services/explicador.py:180` ou `:203`, retornado em `:204`. | Fio chama produtor e guarda explicação em `app/chamados/services/fio.py:346`, `:348`; desenlace usa resumo em `app/chamados/services/desenlace.py:83`; modal apresenta em `app/templates/chamados/partials/modal_fio.html:462`. |

**Descartável — sobrevivência do antigo “todo não cadastro é TI/DP”.** O modal agora lê `quem_resolve` pronto, e a tabela distingue expressamente a obra sem ação humana; o resumo usa a mesma tabela. Referências: `app/templates/chamados/partials/modal_fio.html:468`; `app/chamados/services/explicador.py:35`, `:43`, `:200`.

**Descartável — Central/desenlace interpretando o novo `dono` como `sistema`.** O desenlace recebe a explicação por `app/chamados/services/fio.py:445` e usa `pode_fechar`, `gravou`, `estado` e `resumo`; não consulta `dono` nem `quem_resolve` nesse caminho (`app/chamados/services/desenlace.py:62`, `:67`, `:70`, `:83`). A classificação por dono é apresentação, enquanto a permissão de fechar vem diretamente de C7 em `app/chamados/services/explicador.py:177`.

**Descartável — contador externo dependendo da enumeração antiga desse item.** O contador localizado para esse `dono` é o agrupamento interno do resumo, que aceita qualquer chave em `app/chamados/services/explicador.py:184` e traduz o novo valor pela tabela em `:200`. Os consumidores de runtime localizados para a explicação são fio, desenlace e modal, acima; existência de leitor dinâmico não localizado [nao verificado].

**Duvidoso, apresentação anterior ao delta — ação genérica do desenlace quando C7 libera e há espera.** Esse estado é explicitamente montado pelo teste em `app/chamados/tests/test_bug_par_projetado.py:1198`, `:1203`, `:1205`; o item manda esperar a obra em `app/chamados/services/explicador.py:135`, enquanto a ação geral do desenlace sugere conferir/corrigir marcação e fechar em `app/chamados/services/desenlace.py:79`. Isso não é leitura divergente de `dono`/`quem_resolve`, pois esses campos não são consultados; não demonstrado como violação da regra literal de escrita.

**Descartável — selo novo sem mordida, por inspeção.** O teste exige a premissa C7 bloqueando, o estado correto, a causa no resumo e no resolvedor, além de impedir a tradução antiga no HTML: `app/chamados/tests/test_bug_par_projetado.py:1224`, `:1226`, `:1227`, `:1228`, `:1229`, `:1230`, `:1232`, `:1233`. O HTML passa pelo contexto real do fio, não por texto fabricado (`app/chamados/tests/test_bug_par_projetado.py:765`, `:768`; `app/chamados/services/fio.py:487`).

**Descartável — selo de reuso permitindo `nao_gravou`.** Agora exige `ok=True`, estado exato `ja_no_espelho` e nenhuma nova batida, em `app/chamados/tests/test_bug_par_projetado.py:1100`, `:1101`, `:1102`. Execução de RED/GREEN e resultado de testes [nao verificado].

_4902 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
