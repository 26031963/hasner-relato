# CODEX-ESTADO

> Ultima resposta processada: **07/11/2026 15:24** · publicado em **07/11/2026 15:25** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 07/11 15:24 · sessao c9a5bc337859 · - · - · origem=Ronald (tmux)

[prova/1_viva.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/1_viva.out:1)
```text
/bin/bash: line 3: /home/ronald/saas-hasner/INTRUSO_VIVA_C.txt: Read-only file system
rc=1
```

[prova/2_tmp.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/2_tmp.out:1)
```text
/bin/bash: line 4: /tmp/INTRUSO_TMP_C.txt: Read-only file system
rc=1
```

[prova/2b_tmpdir.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/2b_tmpdir.out:1)
```text
/bin/bash: line 5: /tmp/INTRUSO_TMPDIR_C.txt: Read-only file system
rc=1 TMPDIR=
```

[prova/3_git.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/3_git.out:1)
```text
/bin/bash: line 6: /home/ronald/saas-hasner/.git/INTRUSO_GIT_C.txt: Read-only file system
rc=1
```

[prova/3b_commit.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/3b_commit.out:1)
```text
fatal: Unable to create '/home/ronald/saas-hasner/.git/worktrees/wt-codex-provaefeito-152358/index.lock': Read-only file system
rc=128
```

[prova/4_raiz.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/4_raiz.out:1)
```text
/bin/bash: line 8: /home/ronald/wt-codex-provaefeito-152358/INTRUSO_RAIZ_WT.txt: Read-only file system
rc=1
```

[prova/5_home.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/5_home.out:1)
```text
/bin/bash: line 9: /home/ronald/INTRUSO_HOME_C.txt: Read-only file system
rc=1
```

[prova/6_docker.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/6_docker.out:1)
```text
permission denied while trying to connect to the docker API at unix:///var/run/docker.sock
rc=1
```

[prova/6b_sock.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/6b_sock.out:1)
```text
rc=7
```

[prova/7_tcp_local.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/7_tcp_local.out:1)
```text
bash: socket: Operation not permitted
bash: line 1: /dev/tcp/127.0.0.1/46597: Operation not permitted
rc=1
```

[prova/7c_unix.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/7c_unix.out:1)
```text
Traceback (most recent call last):
  File "<string>", line 1, in <module>
PermissionError: [Errno 1] Operation not permitted
rc=1
```

[prova/7b_tcp_ext.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/7b_tcp_ext.out:1)
```text
bash: socket: Operation not permitted
bash: line 1: /dev/tcp/1.1.1.1/443: Operation not permitted
rc=1
```

[prova/8_sobrevive.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/8_sobrevive.out:1)
```text
lancado
rc=0
```

## 07/11 14:57 · sessao 512712426e5e · FIO-E0-V4-E1 · verificador · origem=Code

**Veredito: a cura continua parcial. Permanecem PROCEDENTES as classes (1), (2) e (4); a classe (3) melhorou, mas não cumpre integralmente a conferência por item sob trava. A classe (5) foi corrigida.** (`app/chamados/management/commands/backfill_decisoes_pergunta.py:183`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:215`; `app/chamados/models.py:291`; `app/chamados/models.py:2665`; `app/chamados/services/fio.py:1120`)

Confirmação independente do hash/pai, RED→GREEN histórico, suíte inteira `rc=0`, bloqueios executados no banco e ocorrência dos contraexemplos em produção: **[nao verificado]**. [nao verificado]

**1. Classe (1) — PROCEDENTE: ainda há recorte anterior à canônica.**

`aplicar` escolhe sobre `_no_fato(fam)`, que já filtrou o horizonte. Entretanto, a identidade BATIDA é `(batida_origem_id, motivo)`, sem dia, e `do_fato` consulta o episódio inteiro sem esse corte. Portanto as portas continuam podendo entregar canônicas diferentes. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:215`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:218`; `app/chamados/services/fio.py:1055`; `app/chamados/services/fio.py:1017`; `app/chamados/models.py:2562`)

Contraexemplo de código, no acervo anterior à implantação do índice: mesmo episódio/motivo, `p1` respondida abaixo do horizonte e `p2/p3` mudas acima dele. `abrir` e a migração escolhem `p1`; backfill/dedup escolhem `p2` e retratam `p3`. A regra de preferência é a mesma; o universo entregue a ela diverge. Reprodução em banco: **[nao verificado]**. (`app/chamados/models.py:2683`; `app/chamados/models.py:2684`; `app/chamados/services/fio.py:984`; `app/chamados/services/fio.py:1196`; `app/chamados/services/fio.py:1203`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:251`)

Existe ainda diferença sem atravessar o horizonte: `do_fato` incorpora, pelo OR, a pergunta legada **sem episódio** do mesmo dia; o agrupamento BATIDA usado pelo backfill restringe-se à chave do episódio. Essa linha legada também pode ser a canônica excluída. (`app/chamados/models.py:2560`; `app/chamados/models.py:2563`; `app/chamados/services/fio.py:1017`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:215`)

**NAO PROCEDE a permanência do contraexemplo antigo das reutilizáveis:** agora a declaração escolhe primeiro sobre o fato inteiro e somente depois confere destino/reutilização. (`app/chamados/services/declaracao_texto.py:242`; `app/chamados/services/declaracao_texto.py:245`; `app/chamados/services/declaracao_texto.py:246`; `app/chamados/services/declaracao_texto.py:267`)

**2. Classe (2) — PROCEDENTE pelas travas implícitas e pela ordem posterior.**

**NAO PROCEDE aquisição explícita adicional localizada** das quatro entidades por `select_for_update`: as quatro consultas estão em `trava_na_ordem`, na ordem declarada. A busca fora de `app` encontrou detector e fixtures sintéticas, não outra aquisição de produção; construção dinâmica sem os nomes pesquisados permanece **[nao verificado]**. (`app/chamados/models.py:2508`; `app/chamados/models.py:2515`; `app/chamados/models.py:2528`; `app/chamados/models.py:2532`; `bin/atomicidade_da_porta.py:63`; `bin/tests/test_atomicidade_da_porta.sh:53`)

Persistem dois caminhos concretos:

- **Chamado → Disputa por S143.** `fechar → transicionar → _aplicar → _gravar_estado` pode entrar sem `_recarregar_estado_se_mudou`. O escritor trava somente Colaborador, salva Chamado e dispara S143; o resolver adquire depois as Disputas abertas. Um chamado sem impedimento próprio, mas com outra Disputa aberta da mesma pessoa, alcança essa sequência. (`app/chamados/models.py:447`; `app/chamados/models.py:199`; `app/chamados/models.py:442`; `app/chamados/models.py:286`; `app/chamados/models.py:291`; `app/chamados/signals.py:92`; `app/chamados/services/disputa_emissao.py:1683`; `app/chamados/juizes.py:2083`)
- **Pergunta → Disputa na ausência rejeitada.** O atomic chama `reabrir_para_correcao`, que faz UPDATE de Pergunta; depois chama `reabrir_sistema`, que salva Disputa. Não há antecipação dessas travas nesse envelope. (`app/chamados/services/materializacao.py:672`; `app/chamados/services/materializacao.py:673`; `app/chamados/services/disputa_emissao.py:1516`; `app/chamados/models.py:2973`; `app/chamados/services/materializacao.py:677`; `app/chamados/models.py:2130`)

O sítio antigo de `_recarregar_estado_se_mudou` foi corrigido, mas essa correção não cobre todas as entradas no escritor de estado. S88/S143/S144 continuam executando ações síncronas durante os saves. (`app/chamados/models.py:689`; `app/chamados/models.py:442`; `app/chamados/signals.py:42`; `app/chamados/signals.py:92`; `app/chamados/signals.py:140`)

**3. Classe (3) — PROCEDENTE, parcial; NAO PROCEDE mistura de escrita e recusa no retorno normal.**

Mérito é relido dentro do atomic, depois da trava. Exportação também é consultada nesse envelope; contudo `_ent` fica fora do laço e reaproveita a resposta entre colaboradores da mesma competência, sem nova consulta após a trava de cada um. O congelamento contra entrega concorrente não está demonstrado por essas travas. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:194`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:200`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:205`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:209`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:219`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:238`; `app/folha/export.py:581`)

A divergência do horizonte por empresa é conferida **antes** do atomic. Para `sem_ato`, há veto explícito sob trava; para grupos, o horizonte funciona como filtro que pode esvaziar o universo e provocar `continue`, sem registrar `fora_do_horizonte`. Isso não implementa integralmente o veto anunciado por item. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:183`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:205`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:215`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:216`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:225`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:229`)

**NAO PROCEDE o defeito anterior de escrita persistida acompanhada de recusa:** há atomic externo e a exceção é levantada dentro dele; o retorno de recusa ocorre após rollback e zera os contadores. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:205`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:264`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:266`)

`handle` faz mais que chamar `aplicar`: prepara/imprime o DRY e traduz recusas em saída. **NAO PROCEDE outra escrita do acervo nele ou no dedup:** os dois encaminham o apply à mesma função; as alterações de carimbo continuam delegadas a `retratar_duplicata`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:296`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:371`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:384`; `app/chamados/management/commands/dedup_perguntas_fato.py:87`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:251`)

**4. Classe (4) — PROCEDENTE: a declaração mudou, mas leitores permanecem incompatíveis.**

O significado declarado é **dia do turno com `conversa=False`**; conversa conserva o **dia civil da emissão**. O novo `q_do_horizonte` incorpora o discriminador. (`app/chamados/services/fio.py:1031`; `app/chamados/services/fio.py:1036`; `app/chamados/models.py:2637`)

O próprio nascimento ainda entrega o dia civil da conversa a `resolvedora_valida`, usando proxy sem `conversa`. Esse juiz presume conversa apenas quando a data é nula e compara a data recebida com o turno do chamado. Conversa emitida hoje para chamado de dois dias antes pode ser recusada como costura cruzada. (`app/chamados/models.py:2661`; `app/chamados/models.py:2665`; `app/chamados/models.py:2666`; `app/chamados/models.py:2668`; `app/chamados/juizes.py:1966`; `app/chamados/juizes.py:1968`; `app/chamados/juizes.py:1985`; `app/chamados/juizes.py:2001`)

Outros leitores sem `conversa=False`, `do_fato` ou `q_do_horizonte`:

| Leitor | Uso incompatível |
|---|---|
| Juiz do dia do chamado | Incorpora datas de todas as perguntas ao dia inferido da Disputa. (`app/chamados/juizes.py:75`; `app/chamados/juizes.py:80`; `app/chamados/juizes.py:209`) |
| Prévia da folha | Agrupa pela data civil e apresenta a resposta como material do dia da célula. (`app/folha/previa.py:64`; `app/folha/previa.py:67`; `app/folha/previa.py:95`) |
| Copiloto do dia | Inclui conversas civis nas perguntas daquele dia. (`app/ponto/services/dia_do_colab.py:64`; `app/ponto/services/dia_do_colab.py:71`; `app/ponto/services/dia_do_colab.py:99`) |
| Diagnóstico do fio | Compara `data_evento` ao dia do fato sem distinguir conversa. (`app/chamados/services/fio.py:86`; `app/chamados/services/fio.py:88`) |

**5. Classe (5) — NAO PROCEDE no contador atual.**

“Nova” passou a usar `data_evento` da Pergunta contra o dia local da aplicação da 0052. Registro ausente ou dia nulo alarma; o escape de pergunta posterior sobre turno anterior está declarado. O RED fixa a aplicação da migração e exige que pergunta nova em Disputa velha conte como nova. (`app/chamados/services/fio.py:1108`; `app/chamados/services/fio.py:1115`; `app/chamados/services/fio.py:1119`; `app/chamados/services/fio.py:1120`; `app/chamados/tests/test_e1_v6_horizonte.py:228`)

**6. Selos — PROCEDENTE insuficiência de alcance; NAO PROCEDE ausência generalizada de mordida.**

A/B varrem Python sob `app`, com exclusões; C examina somente os dois commands. Os esperados são zero, sem allowlist explícita. Isso não equivale a cobrir semanticamente toda a classe no repositório. (`app/chamados/tests/test_e1_v7_catracas.py:26`; `app/chamados/tests/test_e1_v7_catracas.py:30`; `app/chamados/tests/test_e1_v7_catracas.py:144`; `app/chamados/tests/test_e1_v7_catracas.py:176`; `app/chamados/tests/test_e1_v7_catracas.py:218`; `app/chamados/tests/test_e1_v7_catracas.py:268`)

| Catraca | Formas que escapam ao detector |
|---|---|
| A — trava | `getattr(..., "select_for_update")`, `get_for_update`, SQL cru; importação do módulo como `from chamados import models as m`. Alias direto do modelo e queryset em variável são detectados nos casos cobertos. (`app/chamados/tests/test_e1_v7_catracas.py:65`; `app/chamados/tests/test_e1_v7_catracas.py:79`; `app/chamados/tests/test_e1_v7_catracas.py:191`) |
| B — canônica | Alias importado da função; `.filter/.exclude` no QuerySet; `qs = do_fato(...); qs.first()`; `min`/`sorted()[0]`. (`app/chamados/tests/test_e1_v7_catracas.py:19`; `app/chamados/tests/test_e1_v7_catracas.py:100`; `app/chamados/tests/test_e1_v7_catracas.py:113`; `app/chamados/tests/test_e1_v7_catracas.py:135`) |
| C — escrita | Alias de escritor, `getattr(p, "save")()` e helper chamado fora dos dois arquivos examinados. (`app/chamados/tests/test_e1_v7_catracas.py:144`; `app/chamados/tests/test_e1_v7_catracas.py:157`) |

As dispensas também usam apenas o nome da função: outra função chamada `trava_na_ordem`, `canonica_do_grupo` ou `aplicar` pode ocultar infração, sem comprovar arquivo/classe da autoridade. Logo zero allowlist nominal não garante sítio único. (`app/chamados/tests/test_e1_v7_catracas.py:83`; `app/chamados/tests/test_e1_v7_catracas.py:121`; `app/chamados/tests/test_e1_v7_catracas.py:157`)

Os mutantes mordem as formas antigas escolhidas. Os REDs também corrigem as vacuidades específicas: p1 validada/materializada vence p2 muda; D exige `count()==1`; o `TransactionTestCase` tenta NOWAIT em segunda conexão, com seis linhas bloqueadas e dois controles livres, mais variante sem flag. Execução desses casos no banco: **[nao verificado]**. (`app/chamados/tests/test_e1_v7_catracas.py:182`; `app/chamados/tests/test_e1_v7_catracas.py:235`; `app/chamados/tests/test_e1_v7_catracas.py:276`; `app/chamados/tests/test_e1_v6_horizonte.py:405`; `app/chamados/tests/test_e1_v6_horizonte.py:454`; `app/chamados/tests/test_e1_v5_ordem_trava.py:481`; `app/chamados/tests/test_e1_v5_ordem_trava.py:504`; `app/chamados/tests/test_e1_v5_ordem_trava.py:515`)

Permanece uma limitação distinta: o observador de ordem registra somente a primeira aquisição de cada **tabela**, ignorando outra PK da mesma tabela adquirida posteriormente. (`app/chamados/tests/test_e1_v5_ordem_trava.py:36`; `app/chamados/tests/test_e1_v5_ordem_trava.py:46`)

**7. Escritores, leitores e significados — PROCEDENTE a incompatibilidade de `data_evento`; NAO PROCEDE uniformidade total.**

| Campo/valor | Quem escreve | Quem lê e significado |
|---|---|---|
| `via_resolucao` | Carimbo por `_escrever_carimbo`, pelas portas normal/identidade/descarimbo; lote tem UPDATE próprio. Exceções declaradas: `relavrar` e `encerrar_por_fato_no_espelho`. (`app/chamados/models.py:2810`; `app/chamados/models.py:2865`; `app/chamados/models.py:2902`; `app/chamados/models.py:2973`; `app/chamados/services/relavra_via.py:123`; `app/chamados/models.py:2983`) | C6, `q_vivas`, identidade do fato, backfill e materializador. Representa via de encerramento/identidade, não necessariamente plantio. (`app/chamados/juizes.py:1148`; `app/chamados/models.py:3058`; `app/chamados/models.py:2564`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:129`; `app/chamados/services/materializacao.py:438`) |
| `materializada_em` | Mesmas portas do carimbo. Duplicata preserva instante/autor anteriores quando já existem. (`app/chamados/models.py:2812`; `app/chamados/models.py:2894`; `app/chamados/models.py:2974`) | Vitalidade, elegibilidade do materializador, identificação da absorção e instante do SILENCIO legado. Não significa sempre “batida criada”, nem sempre instante da retratação; o help do campo continua mais estreito. (`app/chamados/juizes.py:1210`; `app/chamados/services/materializacao.py:1363`; `app/chamados/services/materializacao.py:624`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:243`; `app/chamados/models.py:3103`) |
| `duplicata` / motivo retratado | `retratar_duplicata` e `remover_motivo`, pela porta de identidade. (`app/chamados/services/fio.py:1002`; `app/chamados/models.py:2014`) | `VIAS_FORA_DO_FATO`, C6, índices e guardas de descarimbo: terminalidade da identidade; resposta permanece histórica. (`app/chamados/models.py:2364`; `app/chamados/models.py:2564`; `app/chamados/juizes.py:1148`; `app/chamados/models.py:2918`; `app/chamados/models.py:3193`) |
| `admin_decidiu` | `fechar_por_admin`, pela porta de identidade. Backfill acrescenta SILENCIO à sequência; não grava novamente essa via. (`app/chamados/models.py:2777`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:242`) | C6 e backfill distinguem decisão humana; mérito perdedor é essa via com resposta. (`app/chamados/juizes.py:1173`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:129`) |
| `data_evento` | Nascimento; preenchimento de órfã no reapontamento; rechaveamento. (`app/chamados/models.py:2731`; `app/chamados/services/fio.py:840`; `app/chamados/services/fio.py:948`) | Identidade, horizonte, competência, materialização e leitores do dia. A incompatibilidade turno/conversa permanece nos consumidores do item 4. (`app/chamados/models.py:2562`; `app/chamados/services/fio.py:1036`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:118`; `app/chamados/services/materializacao.py:345`; `app/chamados/juizes.py:1966`) |
| `status_local` | `_gravar_estado` atribui/grava; nascimento define estado inicial. (`app/chamados/models.py:228`; `app/chamados/models.py:291`; `app/chamados/models.py:1048`) | `_recarregar_estado_se_mudou`, motor e juízes leem estado persistido do Chamado. **NAO PROCEDE mudança de significado causada pelo diff**; o problema restante é ordem de aquisição. (`app/chamados/models.py:690`; `app/chamados/models.py:692`; `app/chamados/catalogo/motor.py:24`; `app/chamados/models.py:257`) |

Há ainda documentação contraditória: `carimbar_identidade` diz que duplicata respondida pode continuar viva, mas o C6 atual mata `VIAS_QUE_VENCEM_A_RESPOSTA` antes de examinar a resposta. (`app/chamados/models.py:2885`; `app/chamados/models.py:2887`; `app/chamados/juizes.py:1148`; `app/chamados/juizes.py:1153`)

**8. Reincidências e sete TETOS.**

**PROCEDENTES:** seleção sobre universo paralelo, ordem de trava, campo com dois domínios e relógio fora da borda nos testes. As fixtures ainda criam Disputa com `auto_now_add`, chamam fechamento que usa `now()` sem injeção e usam `localdate()/now()` no selo de desempenho. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:215`; `app/chamados/models.py:291`; `app/chamados/models.py:2637`; `app/chamados/tests/test_e1_v6_horizonte.py:74`; `app/chamados/models.py:1710`; `app/chamados/tests/test_e1_v6_horizonte.py:174`; `app/chamados/models.py:2777`; `app/ponto/tests/test_selo_chokepoint_escrita.py:229`; `app/ponto/tests/test_selo_chokepoint_escrita.py:262`)

**NAO PROCEDE segundo escritor do backfill demonstrado nesta volta; NAO PROCEDE selo inteiramente sem mordida.** Persistem insuficiências de cobertura descritas acima. (`app/chamados/management/commands/dedup_perguntas_fato.py:87`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:251`; `app/chamados/tests/test_e1_v7_catracas.py:182`; `app/chamados/tests/test_e1_v7_catracas.py:235`; `app/chamados/tests/test_e1_v7_catracas.py:276`)

**NAO PROCEDE subida sem frase C8:** os sete números têm justificativa ao lado. (`PLANO_PISCADA.md:10`; `app/ponto/tests/test_selo_chokepoint_escrita.py:116`; `app/ponto/tests/test_selo_chokepoint_escrita.py:187`)

| Cenário | Teto / aumento | Fonte |
|---|---:|---|
| `_gravar_estado` sem pergunta | 25 / +2 | `app/ponto/tests/test_selo_chokepoint_escrita.py:116` |
| Reconciliar fecha | 57 / +2 | `app/ponto/tests/test_selo_chokepoint_escrita.py:118` |
| Reconciliar recusa | 14 / +2 | `app/ponto/tests/test_selo_chokepoint_escrita.py:119` |
| Uma cobrança fecha | 204 / +4 | `app/ponto/tests/test_selo_chokepoint_escrita.py:161` |
| Três cobranças fecham | 254 / +8 | `app/ponto/tests/test_selo_chokepoint_escrita.py:162` |
| Uma cobrança fica | 142 / +2 | `app/ponto/tests/test_selo_chokepoint_escrita.py:180` |
| Três cobranças ficam | 211 / +3 | `app/ponto/tests/test_selo_chokepoint_escrita.py:187` |

A decomposição é compatível com quantidade fixa de consultas **por entrada** na trava, não com tempo/contenção constantes: cresce o universo de linhas bloqueadas. O último +3 admite decomposição não medida, apesar da justificativa geral de +2 por entrada. Medições citadas nos logs e causalidade exata desse delta: **[nao verificado]**. (`app/chamados/models.py:2513`; `app/chamados/models.py:2515`; `app/chamados/models.py:2523`; `app/chamados/models.py:2528`; `app/ponto/tests/test_selo_chokepoint_escrita.py:187`)

**9. Efeito colateral — NAO PROCEDE apagamento físico; PROCEDENTE perda de efetividade sem veto de mérito.**

A retratação não apaga resposta, validação ou `DecisaoPergunta`: altera via/carimbo/autor/recusa. Porém tira a linha da identidade e pode matar sua resposta pendente antes da validação. (`app/chamados/services/fio.py:1002`; `app/chamados/models.py:2894`; `app/chamados/models.py:2898`; `app/chamados/models.py:2902`; `app/chamados/models.py:2564`; `app/chamados/juizes.py:1148`)

Contraexemplo abaixo do horizonte: `p1` respondida/validada fora do destino; `x=p2` respondida com `admin_decidiu`, sem validação nem célula, no destino; `p3` muda viva no chamado velho. `x` pode estar viva pela resposta. A migração escolhe `p1`, retrata `x` e `p3` e não migra nenhuma delas para o destino. A resposta de `x` perde sua pendência efetiva; a decisão fica histórica. (`app/chamados/juizes.py:1153`; `app/chamados/juizes.py:1165`; `app/chamados/services/fio.py:1181`; `

[truncado em 20000 de 20638 caracteres]

## 07/11 14:55 · sessao c4396d29c20e · - · - · origem=Ronald (tmux)

Busca complementada fora de `app`, incluindo raiz, `bin` e scripts: **NAO PROCEDE aquisição explícita adicional localizada**; o resultado de produção encontrado é um detector AST, não uma trava. (`bin/atomicidade_da_porta.py:63`)

Os `select_for_update` restantes fora de `app` aparecem em fontes sintéticas de teste sobre `Linha`. (`bin/tests/test_atomicidade_da_porta.sh:53`; `bin/tests/test_atomicidade_da_porta.sh:70`; `bin/tests/test_atomicidade_da_porta.sh:75`; `bin/tests/test_atomicidade_da_porta.sh:80`)

Não surgiram candidatos adicionais por `get_for_update`, `getattr`, SQL `FOR UPDATE`/`FOR SHARE` ou `LOCK TABLE`; ausência por construção dinâmica que não contenha esses nomes: **[nao verificado]**.

## 07/11 14:54 · sessao 675064e115a1 · - · - · origem=Ronald (tmux)

Itens 6/8, somente leitura:

- **PROCEDENTE — catracas incompletas.** A/B varrem somente Python sob `app`, excluindo diretórios e arquivos; C limita-se aos dois commands. Não cobrem literalmente todo o repositório. (`app/chamados/tests/test_e1_v7_catracas.py:26`; `app/chamados/tests/test_e1_v7_catracas.py:30`; `app/chamados/tests/test_e1_v7_catracas.py:144`)
- **NAO PROCEDE — ausência de mordida nas formas antigas.** Executei os 16 métodos dessas três classes em memória, substituindo somente `SimpleTestCase` por `unittest.TestCase`: 16 passaram; os detectores retornaram A/B/C vazios para 749 fontes. Essa execução não constitui suite Django, banco nem RED→GREEN histórico. (`app/chamados/tests/test_e1_v7_catracas.py:165`; `app/chamados/tests/test_e1_v7_catracas.py:209`; `app/chamados/tests/test_e1_v7_catracas.py:261`)
- **PROCEDENTE — burlas demonstradas em memória:** A deixa passar `getattr(...,"select_for_update")`, `get_for_update`, SQL cru e `from chamados import models as m; m.PerguntaDisputa.objects.select_for_update()`. B deixa passar alias importado da canônica, `canonica_do_grupo(do_fato(...).filter(...))` e `qs=do_fato(...); qs.first()`. C deixa passar alias de `retratar_duplicata`, `getattr(p,"save")()` e helper externo. (`app/chamados/tests/test_e1_v7_catracas.py:79`; `app/chamados/tests/test_e1_v7_catracas.py:100`; `app/chamados/tests/test_e1_v7_catracas.py:113`; `app/chamados/tests/test_e1_v7_catracas.py:135`; `app/chamados/tests/test_e1_v7_catracas.py:157`)
- **PROCEDENTE — exceções apenas por nome.** Qualquer função homônima `trava_na_ordem`, `canonica_do_grupo` ou `aplicar` recebe a dispensa, sem provar arquivo/classe da autoridade. Não há allowlist explícita, mas isso não certifica sítio único. (`app/chamados/tests/test_e1_v7_catracas.py:83`; `app/chamados/tests/test_e1_v7_catracas.py:121`; `app/chamados/tests/test_e1_v7_catracas.py:157`)

Os três REDs solicitados têm asserções concretas: p1 validada/materializada versus p2 muda e ordem reversa; D exige exatamente uma linha; teste real usa segunda thread/conexão, NOWAIT, seis linhas bloqueadas e dois controles livres, com variante sem flag. Resultado no banco: **[nao verificado]**. (`app/chamados/tests/test_e1_v6_horizonte.py:405`; `app/chamados/tests/test_e1_v6_horizonte.py:445`; `app/chamados/tests/test_e1_v5_ordem_trava.py:448`; `app/chamados/tests/test_e1_v5_ordem_trava.py:481`; `app/chamados/tests/test_e1_v5_ordem_trava.py:504`; `app/chamados/tests/test_e1_v5_ordem_trava.py:515`)

**PROCEDENTE — ordem ainda observada por tabela:** o rastreador ignora aquisições posteriores de outras PKs da mesma tabela; o novo teste de bloqueio não elimina essa limitação geral. (`app/chamados/tests/test_e1_v5_ordem_trava.py:36`; `app/chamados/tests/test_e1_v5_ordem_trava.py:46`)

**PROCEDENTE — relógio fora da borda permanece.** Fixtures criam Disputa com `auto_now_add`; casos chamam `fechar_por_admin`, que usa `now()` sem injeção. A fixture dos tetos também usa `localdate()` e `now()`. (`app/chamados/tests/test_e1_v6_horizonte.py:74`; `app/chamados/models.py:1710`; `app/chamados/tests/test_e1_v6_horizonte.py:174`; `app/chamados/models.py:2777`; `app/ponto/tests/test_selo_chokepoint_escrita.py:229`; `app/ponto/tests/test_selo_chokepoint_escrita.py:262`)

**NAO PROCEDE — subida calada dos sete tetos:** todos têm justificativa ao lado, como exige C8. Valores/deltas: 25/+2, 57/+2, 14/+2, 204/+4, 254/+8, 142/+2, 211/+3. (`PLANO_PISCADA.md:10`; `app/ponto/tests/test_selo_chokepoint_escrita.py:116`; `app/ponto/tests/test_selo_chokepoint_escrita.py:118`; `app/ponto/tests/test_selo_chokepoint_escrita.py:119`; `app/ponto/tests/test_selo_chokepoint_escrita.py:161`; `app/ponto/tests/test_selo_chokepoint_escrita.py:162`; `app/ponto/tests/test_selo_chokepoint_escrita.py:180`; `app/ponto/tests/test_selo_chokepoint_escrita.py:187`)

A implementação sustenta quantidade fixa de consultas por entrada, não tempo/contenção constantes: o universo de linhas bloqueadas cresce. O último +3 admite decomposição não medida, apesar da justificativa geral +2/entrada; causalidade exata desse delta e medições dos logs: **[nao verificado]**. (`app/chamados/models.py:2508`; `app/chamados/models.py:2513`; `app/chamados/models.py:2523`; `app/chamados/models.py:2528`; `app/ponto/tests/test_selo_chokepoint_escrita.py:187`)

## 07/11 14:54 · sessao ec76314e5c3c · - · - · origem=Ronald (tmux)

**1. PROCEDENTE — universo recortado antes da canônica permanece no backfill.** `aplicar` lê `_no_fato(fam)` e só então chama `canonica_do_grupo`; `_no_fato` já excluiu o passado pelo horizonte. A identidade BATIDA não inclui data, enquanto `do_fato` busca o episódio inteiro sem horizonte. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:215`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:218`; `app/chamados/services/fio.py:1055`; `app/chamados/services/fio.py:1017`; `app/chamados/models.py:2562`)

Contraexemplo deduzido do código: mesmo episódio/motivo BATIDA, `p1` respondida abaixo H, `p2/p3` mudas acima H; `abrir` e migração escolhem `p1`, mas backfill/dedup escolhem `p2` e retratam `p3`. O índice parcial não impede a linha abaixo H; a colisão acima H é justamente o acervo tratado antes do índice. Ocorrência e reprodução no banco: **[nao verificado]**. (`app/chamados/models.py:2683`; `app/chamados/models.py:2684`; `app/chamados/services/fio.py:1196`; `app/chamados/services/fio.py:1203`; `app/chamados/services/fio.py:984`; `app/chamados/models.py:3200`)

Outro universo diferente: `do_fato` inclui a linha antiga **sem episódio** do mesmo dia pelo OR; agrupamento/backfill BATIDA usa exclusivamente `(batida_origem_id,motivo)`, podendo excluir essa canônica mesmo sem atravessar H. (`app/chamados/models.py:2560`; `app/chamados/models.py:2563`; `app/chamados/services/fio.py:1017`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:215`)

**NAO PROCEDE o contraexemplo antigo das reutilizáveis:** seleção passou a consultar o fato inteiro, escolher e depois conferir reutilização/destino. (`app/chamados/services/declaracao_texto.py:242`; `app/chamados/services/declaracao_texto.py:245`; `app/chamados/services/declaracao_texto.py:246`; `app/chamados/services/declaracao_texto.py:267`)

**NAO PROCEDE novo juiz em `reapontar_resolvedora`:** as irmãs retratadas vêm de `_colisao_ao_datar`, que recompõe `[p]+outras`, chama a canônica e recusa quando `p` perde ou existe outra encerrada. (`app/chamados/services/fio.py:849`; `app/chamados/services/fio.py:850`; `app/chamados/services/fio.py:889`; `app/chamados/services/fio.py:891`; `app/chamados/services/fio.py:892`; `app/chamados/services/fio.py:895`)

**4. PROCEDENTE — leitores ainda tratam conversa civil como turno.** A declaração atual limita dia do turno a `conversa=False`, mantendo dia civil em `abrir_conversa`. (`app/chamados/services/fio.py:1031`; `app/chamados/models.py:2632`; `app/chamados/models.py:2637`)

O caso mais forte é o próprio nascimento: `_nascer` passa também o dia civil da conversa a `resolvedora_valida`, num proxy sem discriminador; o juiz presume conversa apenas quando a data é nula e compara ao turno do chamado. Conversa criada hoje para chamado D−2 pode levantar `ResolvedoraCruzada`. (`app/chamados/models.py:2661`; `app/chamados/models.py:2665`; `app/chamados/models.py:2666`; `app/chamados/models.py:2668`; `app/chamados/juizes.py:1966`; `app/chamados/juizes.py:1968`; `app/chamados/juizes.py:1985`; `app/chamados/juizes.py:2001`)

Outros leitores sem discriminação:

- Fio marca divergência comparando dia civil ao turno. (`app/chamados/services/fio.py:86`; `app/chamados/services/fio.py:88`)
- Juiz do dia do chamado incorpora datas de todas as perguntas, inclusive conversas, tanto singular quanto lote. (`app/chamados/juizes.py:75`; `app/chamados/juizes.py:80`; `app/chamados/juizes.py:209`; `app/chamados/juizes.py:214`)
- Prévia da folha agrupa perguntas por `data_evento` e apresenta resposta de conversa como material do dia da célula. (`app/folha/previa.py:64`; `app/folha/previa.py:67`; `app/folha/previa.py:95`; `app/folha/previa.py:142`)
- Copiloto do dia reúne conversas civis em `perguntas_deste_dia`. (`app/ponto/services/dia_do_colab.py:64`; `app/ponto/services/dia_do_colab.py:71`; `app/ponto/services/dia_do_colab.py:99`)

**5. NAO PROCEDE no contador atual:** usa `data_evento` da Pergunta e dia local de aplicação da 0052; ausência da migração ou dia nulo alarma. O escape de turno antigo contado como acervo está declarado. (`app/chamados/services/fio.py:1108`; `app/chamados/services/fio.py:1115`; `app/chamados/services/fio.py:1119`; `app/chamados/services/fio.py:1120`)

**9. NAO PROCEDE apagamento físico; PROCEDENTE retirada de efetividade sem veto de mérito.** A retratação preserva texto da resposta, validação e decisões: só salva via/carimbo/autor/recusa. Porém `duplicata` sai da identidade e mata a pergunta antes da resposta. (`app/chamados/services/fio.py:1002`; `app/chamados/models.py:2894`; `app/chamados/models.py:2898`; `app/chamados/models.py:2902`; `app/chamados/models.py:2564`; `app/chamados/juizes.py:1148`)

O próprio backfill define `admin_decidiu` com resposta como mérito e explica que torná-la duplicata retira a decisão do fato; `_migrar_sob_trava` faz essa troca sem chamar o veto. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:124`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:126`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:129`; `app/chamados/services/fio.py:1212`; `app/chamados/services/fio.py:1213`)

Contraexemplo abaixo H: canônica `p1` respondida/validada fora do destino; `x=p2` respondida, `admin_decidiu`, sem validação nem célula no destino; `p3` muda viva no velho. `x` pode continuar viva antes da migração, pois resposta sem concordância comprovada com luz mantém vitalidade; migração escolhe `p1`, retrata `x` e `p3`, e nada migra para o destino. A decisão de `x` fica histórica, sem a proteção exigida no backfill. Reprodução executada e ocorrência em produção: **[nao verificado]**. (`app/chamados/juizes.py:1100`; `app/chamados/juizes.py:1150`; `app/chamados/juizes.py:1153`; `app/chamados/juizes.py:1165`; `app/chamados/services/fio.py:1181`; `app/chamados/services/fio.py:1203`; `app/chamados/services/fio.py:1204`; `app/chamados/services/fio.py:1212`)

## 07/11 14:54 · sessao c4396d29c20e · - · - · origem=Ronald (tmux)

**Classe (2): PROCEDENTE.** A centralização de `select_for_update` não eliminou aquisições posteriores por `save`/`UPDATE`; há dois caminhos concretos restantes. (`app/chamados/models.py:291`; `app/chamados/services/materializacao.py:672`)

1. **Chamado → Disputa via S143.** `Chamado.fechar()` chama `transicionar()` e `_aplicar()`, sem passar por `_recarregar_estado_se_mudou`. `_gravar_estado` trava somente o Colaborador e salva o Chamado; seu `post_save` chama S143, cujo resolver só então trava todas as Disputas abertas do colaborador. (`app/chamados/models.py:447`; `app/chamados/models.py:128`; `app/chamados/models.py:442`; `app/chamados/models.py:286`; `app/chamados/models.py:291`; `app/chamados/signals.py:91`; `app/chamados/services/disputa_emissao.py:1683`)

   Contraexemplo de código: chamado administrativo sem perguntas próprias, sem justificativa pendente e sem Disputa própria impeditiva, com **outra** Disputa aberta da mesma pessoa. A guarda S127 consulta somente `chamado=chamado`; essa outra Disputa não impede a transição, mas será travada pelo resolver depois do UPDATE do Chamado. Reprodução em banco: **[nao verificado]**. (`app/chamados/juizes.py:2083`; `app/chamados/models.py:136`; `app/chamados/models.py:148`; `app/chamados/services/disputa_emissao.py:1683`)

2. **Pergunta → Disputa na devolução por ausência rejeitada.** `devolver_absorvidas_da_ausencia_rejeitada` abre atomic e chama `reabrir_para_correcao(..., desfazer_absorcao=True)`; este chega ao UPDATE de Pergunta em `descarimbar_em_lote`. Depois, no mesmo atomic externo, chama `d.reabrir_sistema`, que salva a Disputa. Esses envelopes não antecipam a trava do Colaborador/Disputa. (`app/chamados/services/materializacao.py:672`; `app/chamados/services/materializacao.py:673`; `app/chamados/services/disputa_emissao.py:1516`; `app/chamados/models.py:2973`; `app/chamados/services/materializacao.py:676`; `app/chamados/models.py:2130`)

**NAO PROCEDE a permanência dos quatro `select_for_update` explícitos fora do sítio único nos arquivos de produção examinados:** as quatro aquisições localizadas estão em `trava_na_ordem`, na sequência Colaborador, Disputa, Chamado, Pergunta. Isso não certifica a ordem das escritas acima. (`app/chamados/models.py:2508`; `app/chamados/models.py:2515`; `app/chamados/models.py:2528`; `app/chamados/models.py:2532`)

**O sítio citado na v7 foi corrigido:** `_recarregar_estado_se_mudou` agora antecipa Disputas abertas e Chamados vivos antes de ler `status_local`; a insuficiência é a existência de outras entradas no escritor de estado. (`app/chamados/models.py:689`; `app/chamados/models.py:690`; `app/chamados/models.py:442`)

**Parte do item 7 — `status_local`: NAO PROCEDE divergência de significado demonstrada.** O campo armazena o estado do Chamado; `_gravar_estado` atribui o destino, a leitura recarrega a instância quando o estado persistido difere e os conjuntos oficiais distinguem vivo, encerrado e terminal. A migração da trava mantém esse significado. (`app/chamados/models.py:52`; `app/chamados/models.py:228`; `app/chamados/models.py:690`; `app/chamados/catalogo/motor.py:24`; `app/chamados/catalogo/motor.py:30`)

**Limite do selo de ordem:** o observador registra apenas a primeira aquisição de cada **tabela**, portanto não demonstra que uma segunda Disputa/Chamado da mesma pessoa, ainda não travada, não foi adquirida depois da Pergunta. (`app/chamados/tests/test_e1_v5_ordem_trava.py:36`; `app/chamados/tests/test_e1_v5_ordem_trava.py:45`)

**Sinais continuam síncronos:** S88 reconcilia o pai, S143 resolve perguntas e S144 chama o materializador durante `post_save`; a correção da entrada por `_recarregar` não transforma esses sinais em garantia global de ordem. (`app/chamados/signals.py:41`; `app/chamados/signals.py:92`; `app/chamados/signals.py:140`)

## 07/11 14:52 · sessao cc914c55e5a6 · CODEX-CONSTRUTOR-NEGOCIO · verificador · origem=Code

1. **Baixa — v5.r #5: FECHA os dois escapes apontados.** O detector agora reconhece `/usr/bin/python3`; o laço remove `-I` da ocorrência `k`, individualmente. Na reprodução em memória, o wrapper atual ficou limpo, as sete retiradas foram detectadas e ambas as ocorrências de uma linha sintética foram mordidas. O MORDE absoluto detectou `/python3 -c`, pelo motivo declarado. Não identifiquei regressão nesse trecho; isso continua sendo reconhecimento textual, sem cobrir comandos montados por variável (`bin/tests/test_codex_pergunta.sh:56-70`).

2. **Informativo — selo vizinho marcado CURADO: FECHA a quebra de âncora.** As duas âncoras do mutante existem exatamente uma vez; a substituição em memória produz shell sintaticamente válido, desativa a porta posterior e insere a consulta antes do slot (`bin/tests/test_disjuntor.sh:1495-1502`, `:1780-1783`; `bin/codex_pergunta.sh:342`, `:370-371`). O cenário cobra a consequência correta: três triagens durante a espera devem impedir a chamada; consultar somente antes permite perder essa mudança (`bin/tests/test_disjuntor.sh:1419-1432`). A chamada antecipada usa `python3` sem `-I`, mas o caso executa o wrapper mutado diretamente, sem aplicar FORMA-PY-ISOLADO: não há morte incidental por esse selo (`bin/tests/test_disjuntor.sh:1504`, `:1782`). Mordida efetiva do cenário completo: **[nao verificado]**.

3. **Informativo — v5.r #6: FECHA as correções documentais marcadas CURADO.**

   - **Retornos:** L-126 agora distingue pré-check/revalidação com rc 2 do inventário final proibido ou falho com rc 3, conforme o código (`app/docs/LEIS.md:147`; `bin/codex_pergunta.sh:183`, `:237-239`, `:386-392`, `:413-426`).
   - **Vaga por repositório:** corresponde à derivação pelo `git-common-dir` de `CODEX_VIVO` e ao `flock`; clones distintos continuam com vagas distintas (`app/docs/LEIS.md:147`; `CLAUDE.md:352`; `bin/codex_pergunta.sh:56`, `:71-76`, `:232-233`).
   - **“Não remove a wt”:** descreve corretamente o wrapper, que imprime a remoção recomendada; continua removendo temporários e sidecars (`CLAUDE.md:352`; `bin/codex_pergunta.sh:259`, `:376-383`, `:417`).
   - **Proibições:** a exceção do gitfile raiz e a classificação de autoload/symlink no conjunto tocado correspondem ao inventário. Hardlinks são acrescentados explicitamente a esse conjunto (`app/docs/LEIS.md:147`; `bin/codex_pergunta.sh:152`, `:160-177`). O hash do gitfile é uma fotografia tomada no pré-check e comparada depois (`bin/codex_pergunta.sh:242`, `:388`, `:414`).
   - **Opus:** a distinção entre guarda mecânica e conduta permanece correta: o fluxo executa Codex, inventaria e termina sem consultar aprovação opus (`CLAUDE.md:351-353`; `bin/codex_pergunta.sh:394-448`).

4. **Baixa, condicional — DESCARTADO #3: o motivo sustenta apenas um descarte limitado.** A ausência declarada de `PYTHONDONTWRITEBYTECODE` naquela sessão não refuta a diferença ambiental apontada: o wrapper usa `-I`, sem `-B`, e os auxiliares importam módulos locais (`bin/codex_pergunta.sh:96-102`, `:278`, `:371`; `bin/raspar_publico.py:15-16`; `bin/disjuntor.py:123-124`, `:671-672`). Estar no `.gitignore` não impede a escrita de bytecode (`.gitignore:2-3`). Portanto, sustenta “nenhum efeito concreto demonstrado”, mas não “efeitos ambientais equivalentes”; geração efetiva no host e ambiente de produção: **[nao verificado]**.

5. **Informativo — DESCARTADOS #2c e #7 se sustentam como ausência de defeito demonstrado.** Pergunta, anexo e sonda relativos são entradas deliberadamente resolvidas pelo cwd, conforme o próprio contrato do wrapper (`bin/codex_pergunta.sh:68`, `:86-87`, `:98`, `:275`, `:306-308`). `_IRC` torna-se agregado após conferir o gitfile, mas seu consumidor pergunta apenas se houve falha; `_INV1` e `.inventario` também são consumidos como vazio/status/diagnóstico, sem exigir TSV puro nesses caminhos (`bin/codex_pergunta.sh:386-392`, `:413-418`).

6. **Média condicional / baixa de cobertura — as FILAS continuam procedentes; o delta não as fecha.**

   - **PATH — média condicional:** executáveis continuam resolvidos por nome. “Sem `.`” sozinho não exclui componentes relativos ou vazios; PATH efetivo de produção: **[nao verificado]** (`bin/codex_pergunta.sh:125`, `:268`, `:394`).
   - **HOOK_STOP_RAIZ — média condicional:** continua aceitando raiz relativa e usando-a para caminhos de estado. Ser variável do chamador delimita o vetor, sem eliminá-lo (`bin/disjuntor.py:60`, `:119`, `:247-249`; `bin/hook_stop_fila1.py:30-35`).
   - **`py_iso_nomes` — baixa de cobertura:** a triagem caracteriza corretamente a limitação. A fixture instala wrapper e B3 falso; o caso exige a mensagem específica da guarda. Sem ela, a ausência do raspador oferece outra saída rc 4. Isso distingue a guarda, mas não demonstra vazamento nem envio cru ao Codex (`bin/tests/test_codex_pergunta.sh:74-76`, `:117-120`, `:404-407`; `bin/codex_pergunta.sh:302`, `:311`).
   - **Duas listas — média preexistente:** `_nomes` roda no subshell da pergunta e novamente no pai para anexos/sonda; sem override, pode extrair duas fotografias. O cleanup do pai também não conhece o primeiro temporário (`bin/codex_pergunta.sh:259`, `:265-268`, `:285-286`, `:302`).
   - **Relógio B3 — preexistente:** a chamada continua sem fornecer `agora`, chegando ao fallback `time.time()` (`bin/disjuntor.py:1710`, `:1042`, `:989`, `:980`, `:877`).

7. **Informativo — limite da verificação.** Executei somente verificações em memória e análise sintática; as suítes integrais criam/removem arquivos e não foram executadas sob a regra de leitura (`bin/tests/test_codex_pergunta.sh:18-19`, `:74-81`; `bin/tests/test_disjuntor.sh:39-41`, `:1492-1502`). Resultado integral dos selos, prova real e efeitos em produção: **[nao verificado]**.

## 07/11 14:45 · sessao dc04abcadd48 · CODEX-CONSTRUTOR-NEGOCIO · verificador · origem=Code

1. **Informativo — 1b fechado para o vetor declarado.** As sete chamadas diretas usam `python3 -I`: inventário inicial, nomes, duas raspagens, B3, revalidação e inventário final. Não há outro lançamento Python direto escondido por variável ou continuação no wrapper atual (`bin/codex_pergunta.sh:237`, `:270`, `:278`, `:311`, `:371`, `:386`, `:413`). Isso fecha a importação de `app/hashlib.py`, `app/subprocess.py` ou `app/json.py` pelo cwd nessas chamadas; não demonstra isolamento geral dos processos do host (`bin/codex_pergunta.sh:96`).

2. **Média, condicional e preexistente — ainda existem dependências do ambiente/cwd.**

   - O inventário executa `git` pelo `PATH` herdado; o shell também resolve executáveis como `bash`, `timeout` e `codex` pelo nome. Com componente relativo ou `.` no `PATH`, arquivo executável plantado em `<wt>/app` pode rodar no host. O `PATH` de produção é **[nao verificado]** (`bin/codex_pergunta.sh:125`, `:268`, `:394`).
   - O B3 aceita `HOOK_STOP_RAIZ`, inclusive relativo, e usa essa raiz para ler/gravar estado; o fornecedor do topo também aceita a variável. Portanto, esse caminho pode depender do cwd mesmo sob `-I` (`bin/disjuntor.py:60`, `:119`, `:247`, `:274`; `bin/hook_stop_fila1.py:30`).
   - Pergunta, anexos e sonda relativos dependem deliberadamente do cwd do chamador (`bin/codex_pergunta.sh:68`, `:86`, `:87`, `:275`, `:306`, `:308`). Comportamento interno do executável externo `codex`, além dos argumentos fornecidos: **[nao verificado]** (`bin/codex_pergunta.sh:394`).

3. **Baixa — “rodam iguais sob `-I`” precisa de qualificação.** Os imports locais necessários continuam disponíveis: o raspador insere seu próprio `bin` antes de importar `nome_no_texto`; o B3 faz isso antes de importar `orcamento_contexto` e `hook_stop_fila1`. Não aparece import de `<wt>/app` nesses caminhos (`bin/raspar_publico.py:15`; `bin/disjuntor.py:123`, `:671`; `bin/nome_no_texto.py:26`).

   Entretanto, `-I` muda o tratamento das variáveis de inicialização, inclusive `PYTHONDONTWRITEBYTECODE`; sem `-B`, os imports locais podem passar a gerar bytecode. Criação efetiva no host é **[nao verificado]**. A equivalência funcional dos imports está sustentada pelo código; igualdade de todos os efeitos ambientais, não (`bin/codex_pergunta.sh:96`, `:102`, `:278`, `:371`). O Python dos auxiliares citados realmente roda em containers (`bin/relato_nomes.sh:18`; `bin/sonda_leitura.sh:235`, `:239`).

4. **Média — os testes têm mordida, mas `py_iso_nomes` tem uma causa incidental.** [nao verificado]

   - **`pre`:** o mutante deixa passar a primeira varredura; a revalidação ainda isolada recusa depois. Morre por deixar de apresentar o motivo exigido para o pré-check, o que distingue corretamente as fases (`bin/tests/test_codex_pergunta.sh:379`, `:382`, `:113`; `bin/codex_pergunta.sh:386`).
   - **`reval`:** planta durante a espera da trava e exige recusa antes do Codex; retirar `-I` dessa fase permite ultrapassá-la (`bin/tests/test_codex_pergunta.sh:197`, `:204`, `:206`, `:385`; `bin/codex_pergunta.sh:386`, `:394`).
   - **`fim`:** cobra rc 3, motivo “motor v2”, ausência de sidecar e presença do módulo plantado no inventário. A expectativa corresponde ao defeito declarado (`bin/tests/test_codex_pergunta.sh:117`, `:389`, `:394`).
   - **`nomes`:** a fixture copia somente o wrapper e cria um B3 falso; não instala o raspador. Sem `-I`, `json.py` permite passar `_nomes`, mas a chamada seguinte continua terminando rc 4 porque falta `raspar_publico.py`. O mutante morre pela mensagem “sem a lista de nomes” ausente, sem demonstrar envio de dado cru ou chamada ao Codex (`bin/tests/test_codex_pergunta.sh:69`, `:70`, `:378`, `:398`, `:401`, `:499`; `bin/codex_pergunta.sh:311`). Execução integral desses casos: **[nao verificado]**.

5. **Baixa — FORMA cobre a escrita atual, não toda grafia possível.** O detector reconhece as sete chamadas atuais e acusa a retirada individual de `-I`; porém exclui `/` como delimitador anterior, deixando escapar `/usr/bin/python3`, e não interpreta comandos montados por variável (`bin/tests/test_codex_pergunta.sh:56`, `:59`, `:63`). Também promete mordida por ocorrência, mas percorre linhas e remove apenas a primeira ocorrência de cada linha (`bin/tests/test_codex_pergunta.sh:54`, `:61`, `:62`). Essas formas escapáveis não estão nos sete lançamentos atuais (`bin/codex_pergunta.sh:237`, `:270`, `:278`, `:311`, `:371`, `:386`, `:413`).

6. **Média — documentação corrigida em parte, com novas generalizações excessivas.**

   - **L-126:** junta pré-check, revalidação e fim sob “proibido ou falha = rc 3”; os dois primeiros recusam com **rc 2**, e somente o fim converte para **rc 3** (`app/docs/LEIS.md:147`; `bin/codex_pergunta.sh:183`, `:239`, `:392`, `:426`).
   - **CLAUDE:** limita rc 3 ao inventário final e atribui corretamente o descarte ao Code; o comando de remoção é apenas impresso (`CLAUDE.md:351`; `bin/codex_pergunta.sh:417`).
   - **Teto:** “1 Codex construtor por vez” precisa dizer **por repositório selecionado**. A vaga deriva do `git-common-dir` de `CODEX_VIVO`; clones distintos têm vagas distintas (`CLAUDE.md:352`; `app/docs/LEIS.md:147`; `bin/codex_pergunta.sh:56`, `:72`, `:76`, `:233`).
   - **“Não apaga nada”:** correto se significar “não remove a wt”; literalmente, o wrapper apaga temporários e sidecars (`CLAUDE.md:352`; `bin/codex_pergunta.sh:259`, `:376`, `:381`).
   - **Proibições:** o gitfile raiz é exceção explícita; symlink/autoload são classificados no conjunto de alterações, não incondicionalmente em toda entrada da base (`app/docs/LEIS.md:147`; `bin/codex_pergunta.sh:152`, `:160`, `:170`, `:176`, `:414`).
   - A ausência de guarda de opus e do “1 opus” está descrita corretamente como conduta (`CLAUDE.md:353`; `bin/codex_pergunta.sh:393`, `:442`, `:448`).

7. **Informativo — escritores, leitores e significados.**

   | Campo/arquivo | Quem escreve → quem lê |
   |---|---|
   | `_INV0` | stdout **e stderr** do inventário → teste de vazio e diagnóstico da recusa (`bin/codex_pergunta.sh:237`, `:238`, `:239`). |
   | `_IRC0` | status da atribuição anterior → teste explícito de sucesso; separado do conteúdo (`bin/codex_pergunta.sh:237`, `:238`). |
   | `_INV1` | stdout/stderr do inventário, acrescido de `FALHOU` se necessário → teste de vazio (`bin/codex_pergunta.sh:386`, `:389`). |
   | `.inventario` | inventário final; wrapper acrescenta alteração do gitfile → filtro da `.recusa` e asserções dos testes (`bin/codex_pergunta.sh:413`, `:414`, `:418`; `bin/tests/test_codex_pergunta.sh:255`, `:394`). |
   | `_IRC` | inicialmente status do Python; depois status **agregado**, podendo virar 3 pelo gitfile → decisão de recusa e diagnóstico (`bin/codex_pergunta.sh:413`, `:414`, `:415`, `:417`). |
   | Lista de nomes | extração/escrita pelo auxiliar, ou caminho fornecido por `CODEX_NOMES_JSON` → validação de tamanho e leitura pelo raspador (`bin/relato_nomes.sh:11`, `:13`, `:21`; `bin/codex_pergunta.sh:266`, `:270`; `bin/raspar_publico.py:20`). |

   `_INV1` e `.inventario` carregam registros **e diagnósticos**, portanto não são listas TSV puras; os consumidores mostrados usam vazio/status ou marcadores explícitos (`bin/codex_pergunta.sh:386`, `:389`, `:413`, `:418`). `_IRC` tem a transição de significado indicada acima; não deve ser tratado como status bruto do Python após a conferência do gitfile (`bin/codex_pergunta.sh:414`).

8. **Média, preexistente — “lista UMA vez” não descreve o fluxo default.** A primeira `_nomes` roda dentro de `$(_pergunta_raspada)`; suas atribuições não retornam ao pai. Com anexo/sonda, o pai chama `_nomes` novamente e, sem override, pode extrair outra lista: duas fotografias temporais, embora ambas passem pela mesma porta (`bin/codex_pergunta.sh:245`, `:263`, `:265`, `:268`, `:285`, `:302`). O primeiro temporário também não fica identificado em `DADOS` para o cleanup do pai (`bin/codex_pergunta.sh:259`).

9. **Checklist das classes — sem aprovação ampla implícita.**

   | Classe | Resultado |
   |---|---|
   | Segundo juiz | Não identifiquei duplicação: inventário único com três consumidores; raspagem usa `nome_no_texto`; B3 consulta a decisão existente (`bin/codex_pergunta.sh:105`, `:237`, `:386`, `:413`; `bin/raspar_publico.py:16`, `:31`, `:36`; `bin/disjuntor.py:1042`). |
   | Segundo escritor fora da porta | Não identifiquei novo escritor desses artefatos na v5; produção mais ampla **[nao verificado]** (`bin/codex_pergunta.sh:237`, `:413`, `:414`; `bin/relato_nomes.sh:21`). |
   | Relógio fora da borda | Permanece no B3: `--b3-parada` não fornece `agora`, e a decisão lê `time.time()` como fallback. Preexistente; v5 não altera isso (`bin/disjuntor.py:1710`, `:1042`, `:989`, `:980`, `:877`). |
   | Derivação paralela | Inventário e predicado de nomes permanecem únicos; as duas capturas de nomes são a ressalva do item 8 (`bin/codex_pergunta.sh:105`, `:268`, `:302`; `bin/raspar_publico.py:16`). |
   | Selo/RED sem mordida | Ressalvas dos itens 4–5; execução completa **[nao verificado]** (`bin/tests/test_codex_pergunta.sh:56`, `:69`, `:401`). |
   | Ordem de trava | Vaga → gate/slot → resposta → estado B3; triagem também toma resposta antes de estado. Não identifiquei inversão nova nesses caminhos (`bin/codex_pergunta.sh:232`, `:342`, `:348`, `:368`, `:371`; `bin/disjuntor.py:786`, `:818`). |
   | Pouso antes do pré-requisito | Suite no hash, prova real, primeiro uso de negócio e pouso **[nao verificado]**; a exigência de suite antes do opus permanece documental (`CLAUDE.md:375`). | [nao verificado]
   | Campo com dois significados | Ressalvas do item 7; continua também a separação entre raiz de pausa/LOGD e raiz da vaga (`bin/codex_pergunta.sh:76`, `:88`, `:191`). |

## 07/11 14:42 · sessao 27743e1766f0 · - · - · origem=Ronald (tmux)

1. **Média — L-126 amplia indevidamente o `rc 3`.** O texto novo junta “pre-check, revalidação e fim” e depois afirma “proibido ou falha = rc 3” (`app/docs/LEIS.md:147`). No pre-check, qualquer saída não vazia ou código não zero termina por `_recusa2`, portanto **rc 2** (`bin/codex_pergunta.sh:183`, `:237`, `:238`, `:239`); revalidação também termina **rc 2** (`:386`, `:389`, `:392`). Só a recusa final converte em **rc 3** (`:413`, `:415`, `:426`). CLAUDE limita essa promessa ao inventário **no fim** e está correto (`CLAUDE.md:351`).

2. **Média — “1 Codex construtor por vez” precisa dizer “por repositório”.** A vaga deriva de `git-common-dir` do `CODEX_VIVO` selecionado (`bin/codex_pergunta.sh:56`, `:72`, `:76`), e o `flock` prende esse arquivo (`:232`, `:233`). Assim, as frases de `CLAUDE.md:352` e `app/docs/LEIS.md:147` descrevem corretamente worktrees do mesmo repo, mas não um teto global entre clones distintos. É a **fila antiga 3a** do anexo, não defeito introduzido pela v5. A pausa/LOGD continuam na raiz do script, enquanto a vaga vem do repositório vivo: duas fronteiras diferentes (`bin/codex_pergunta.sh:88`, `:191`, `:193`, `:76`), correspondentes à fila antiga 3b.

3. **Baixa — proibição documental é mais ampla que a implementação.** A classificação de symlink/autoload/`.git*` percorre apenas `tocados` (`bin/codex_pergunta.sh:170`, `:171`, `:176`); symlink idêntico ao blob/modo da base não entra por esse motivo (`:160`). O arquivo `.git` da raiz é podado da varredura (`:151`, `:152`) e conferido separadamente (`:242`, `:414`). A formulação irrestrita “.git em qualquer nível [...] symlink [...] = PROIBIDO” merece explicitar o universo de alterações e a exceção do gitfile raiz (`app/docs/LEIS.md:147`). Existência de symlink rastreado nessa base: **[nao verificado]**.

4. **Média, preexistente — lista de nomes não é obtida “UMA vez”.** Essa promessa está em `bin/codex_pergunta.sh:263`, mas a primeira `_nomes` roda dentro de `$(_pergunta_raspada)` (`:276`, `:285`, `:286`); suas atribuições `DADOS`/`NOMES` ficam no subshell (`:265`, `:266`). Com anexo/sonda, `_nomes` roda novamente no pai (`:302`), que inicialmente tinha `DADOS=""` (`:245`). Sem `CODEX_NOMES_JSON`, isso produz duas extrações por `relato_nomes.sh` (`:267`, `:268`), podendo haver duas fotografias temporais. O temporário inicial também não fica disponível ao cleanup do pai (`:259`). Não é um novo escritor: ambas passam pela mesma porta, cuja extração/escrita está em `bin/relato_nomes.sh:8`, `:11`, `:13`, `:18`, `:21`.

5. **Informativo — correções documentais centrais procedem.** A remoção da wt é apenas instruída no texto da recusa; o comando não é executado (`bin/codex_pergunta.sh:417`). “Descartar a wt é conduta do Code” está correto (`CLAUDE.md:351`, `:352`). O fluxo não exige certificado opus nem controla construtor opus antes/depois da chamada (`bin/codex_pergunta.sh:393`, `:404`, `:442`, `:448`), coerente com `CLAUDE.md:353`. A frase “o wrapper não apaga nada” deve ser entendida como **a wt**: há remoção de temporários e sidecars (`bin/codex_pergunta.sh:259`, `:376`, `:381`).

6. **Classes restantes, por inspeção:** inventário permanece em uma única definição com três consumidores (`bin/codex_pergunta.sh:105`, `:237`, `:386`, `:413`); raspagem continua perguntando ao mesmo `nome_no_texto` para mascarar/conferir (`bin/raspar_publico.py:16`, `:31`, `:36`); B3 continua perguntando à decisão existente (`bin/disjuntor.py:1039`, `:1042`). Não identifiquei juiz de negócio duplicado nem novo escritor fora dessas portas nos trechos auditados; o registro de autoridades permanece em `app/core/juizes.py:170`, com papel documental explicado em `app/docs/JUIZES.md:10`. Ordem: vaga → gate/slot → resposta → estado B3 (`bin/codex_pergunta.sh:232`, `:342`, `:348`, `:368`, `:371`); triagem também toma resposta antes de estado (`bin/disjuntor.py:779`, `:786`, `:789`, `:804`, `:818`). Não encontrei inversão nova nesses caminhos. Execução da prova real, suite no hash, pouso e atendimento dos pré-requisitos: **[nao verificado]**.

## 07/11 14:42 · sessao 0e16194bbd08 · - · - · origem=Ronald (tmux)

1. **1b: fechado para o vetor declarado de import Python; isolamento geral do host não demonstrado.** Os sete lançamentos diretos usam `python3 -I`: inventários em `bin/codex_pergunta.sh:237`, `:386`, `:413`; validação de nomes em `:270`; raspagem em `:278`, `:311`; B3 em `:371`. O inventário ainda resolve `git` pelo `PATH` herdado (`bin/codex_pergunta.sh:124-125`); outros executáveis também são chamados pelo nome, inclusive `bash` (`:268`) e `timeout`/`codex` (`:394`, `:401`). Se o `PATH` contiver componente relativo/`.` e o cwd for `<wt>/app`, executável plantado ali permanece vetor possível; o `PATH` efetivo de produção é **[nao verificado]**.

2. **Dependências de cwd deliberadas e restantes.** Pergunta, anexos e sonda podem ser argumentos relativos, usados sem mudança global de cwd (`bin/codex_pergunta.sh:67-68`, `:86-87`, `:275`, `:306`, `:308`). Além disso, `HOOK_STOP_RAIZ` continua sendo lido pelo B3, inclusive se relativo (`bin/disjuntor.py:60`, `:118-119`), e pelo módulo que fornece o topo (`bin/hook_stop_fila1.py:30-34`). O ramo pode então ler/gravar estado relativo ao cwd (`bin/disjuntor.py:247-274`). `-I` não elimina leituras explícitas de `os.environ`; isso não é variável `PYTHON*` (`bin/disjuntor.py:60`).

3. **`raspar_publico.py`: imports locais continuam disponíveis sob `-I`.** Importa `json`, `os`, `sys` antes de inserir explicitamente a própria pasta; importa depois o juiz `nome_no_texto` (`bin/raspar_publico.py:11-16`). O juiz importa somente `re` (`bin/nome_no_texto.py:26`). Os nomes e textos entram por argumentos/arquivos com UTF-8 explícito (`bin/raspar_publico.py:19-28`), e a saída raspada também (`:40`). Nenhuma leitura explícita de `PYTHON*` nesse caminho foi encontrada nesses módulos; a equivalência executada com/sem `-I` é **[nao verificado]**.

4. **B3: a decisão segue o juiz existente, com imports locais explicitamente restaurados.** `main` encaminha `--b3-parada` para `b3_parada` (`bin/disjuntor.py:1709-1710`), que passa por `estado_b3` → `julga_b3` → `decide_b3` (`:1042`, `:989`, `:980`). `_abre` insere a própria pasta antes de importar `orcamento_contexto` (`:122-125`); `_topo_consistente` faz o mesmo para `hook_stop_fila1` (`:671-672`). Ambos têm somente imports de biblioteca padrão no topo (`bin/orcamento_contexto.py:26-31`; `bin/hook_stop_fila1.py:20-34`). Não foi localizado import de código em `<wt>/app` nesse ramo; `_no_relato`, que carregaria `app/core/esteira_vigia.py`, pertence a outro caminho (`bin/disjuntor.py:179-188`, `:227`).

5. **Python dos auxiliares fica no container, conforme o comentário novo.** `relato_nomes.sh` executa Python por `docker exec saas_core` (`bin/relato_nomes.sh:17-19`); `sonda_leitura.sh` por `docker run ... python manage.py` (`bin/sonda_leitura.sh:235-240`). A árvore escolhida por `SONDA_ARVORE` influencia o Python do container, não um interpretador Python do host (`bin/sonda_leitura.sh:232-239`). A ausência de efeitos específicos do executável externo `codex` antes do sandbox é **[nao verificado]**: o wrapper só fornece sua chamada/configuração (`bin/codex_pergunta.sh:394-402`).

6. **Caveat de precisão do comentário “rodam iguais”.** `-I` pretende mudar a resolução inicial de imports e o tratamento das variáveis de inicialização Python (`bin/codex_pergunta.sh:96-102`); portanto “iguais” deve significar o funcionamento esperado com os imports locais restaurados, não igualdade de todo efeito ambiental. Em particular, ignorar `PYTHONDONTWRITEBYTECODE` pode permitir criação de bytecode dos módulos locais importados — inferência, efeito executado **[nao verificado]** (`bin/raspar_publico.py:15-16`; `bin/disjuntor.py:123-124`, `:671-672`).

7. **Lista de nomes: comentário “UMA vez” já não descreve o fluxo default.** `_pergunta_raspada` roda em substituição de comando (`bin/codex_pergunta.sh:285-286`); sua `_nomes` cria `DADOS`/`NOMES` nesse subshell (`:265-268`). As atribuições não atualizam o `DADOS` externo, inicializado vazio (`:245`). Havendo anexo/sonda, `_nomes` é chamado novamente no processo externo (`:301-302`), podendo extrair uma segunda lista do banco. A promessa “UMA vez” (`:263`) não vale nesse cenário sem `CODEX_NOMES_JSON`; não é novidade introduzida pelo diff v5.

## 07/11 14:41 · sessao c0b04a894150 · - · - · origem=Ronald (tmux)

1. **Média — mutante `py_iso_nomes` morre com causa incidental.** O cenário monta somente o wrapper e um B3 falso; não instala `raspar_publico.py` nem `nome_no_texto.py` (`bin/tests/test_codex_pergunta.sh:68-70`). Retirado `-I` pelo mutante (`bin/tests/test_codex_pergunta.sh:499`), `app/json.py` encerra o Python com sucesso (`bin/tests/test_codex_pergunta.sh:378`, `:399`), permitindo passar `_nomes` (`bin/codex_pergunta.sh:270`). A execução então termina **rc 4 porque falta o raspador** (`bin/codex_pergunta.sh:311`); o caso morre pela mensagem “sem a lista de nomes” ausente, verificada em `bin/tests/test_codex_pergunta.sh:113`, `:401`. Não comprova vazamento nem chamada ao Codex; a execução completa do teste permanece [nao verificado].

2. **Informativa — os outros três casos correspondem à classe declarada.** `pre` planta antes da primeira varredura; sem isolamento, o pre-check passa silenciosamente, mas a revalidação isolada ainda rejeita, mudando o motivo exigido (`bin/tests/test_codex_pergunta.sh:379-382`; `bin/codex_pergunta.sh:237-239`, `:386-392`). `reval` planta durante a espera e, sem isolamento nesse sítio, permite chamar o Codex (`bin/tests/test_codex_pergunta.sh:197-206`, `:385-388`; `bin/codex_pergunta.sh:386-398`). `fim` planta o módulo junto com alteração do motor e cobra rc 3, motivo e presença do módulo no inventário (`bin/tests/test_codex_pergunta.sh:389-395`, `:117-122`; `bin/codex_pergunta.sh:413-426`). Resultado integral da suíte [nao verificado].

3. **Informativa — FORMA cobre as sete chamadas atuais.** Sonda somente em memória confirmou sete ocorrências e detecção ao remover isoladamente `-I` de cada uma: `bin/codex_pergunta.sh:237`, `:270`, `:278`, `:311`, `:371`, `:386`, `:413`; implementação do detector em `bin/tests/test_codex_pergunta.sh:56-63`. Não há Python host adicional direto, variável ou continuação escondida nesses lançamentos (`bin/codex_pergunta.sh:237`, `:270`, `:278`, `:311`, `:371`, `:386`, `:413`).

4. **Baixa — promessa genérica da FORMA excede sua implementação.** A regex não detecta `/usr/bin/python3` nem comando montado por variável; ela exclui `/` como delimitador anterior (`bin/tests/test_codex_pergunta.sh:56`). Além disso, promete mordida por ocorrência (`:54-55`), mas o laço percorre linhas e o `sed` troca somente a primeira ocorrência por linha (`:61-63`). Essas grafias não estão entre os sete lançamentos atuais, mas o selo não assegura a promessa para futuras formas de escrita (`bin/codex_pergunta.sh:237`, `:270`, `:278`, `:311`, `:371`, `:386`, `:413`).

## 07/11 14:28 · sessao 1cbdb9d63fe3 · FIO-E0-V4-E2 · leitura · origem=Code

1. **TRANCA-DE-VERDADE / `competencia_trancada`:** arquivo por chamado, mediante `status_local` + `via_encerramento`; não grava célula: app/ponto/services/fechamento.py:1415. Disparador: app/ponto/services/fechamento.py:1421. Escritores: app/chamados/services/competencia_trancada.py:184 → app/chamados/models.py:1580 → app/chamados/models.py:226, app/chamados/models.py:243, app/chamados/models.py:283. Pergunta sem novo carimbo: app/chamados/services/competencia_trancada.py:178. **O44 desarquivar:** app/chamados/views_arquivar.py:70 → app/chamados/models.py:1399 → app/chamados/models.py:247; levantamento do véu e trilha: app/chamados/views_arquivar.py:77 → app/chamados/services/arquivo.py:201, app/chamados/services/arquivo.py:208, app/chamados/services/arquivo.py:209. Toque por colab+dia: app/chamados/services/cobrar_dia.py:37; leitura: app/chamados/juizes.py:955.

2. **NOT READABLE** pelo `leitor_celula`: app/escala/services/leitor_celula.py:418, app/escala/services/leitor_celula.py:428, app/escala/services/leitor_celula.py:445. Leitor mais próximo do arquivo: app/chamados/views_arquivar.py:165. Leitor mais próximo da condição colab+dia: app/chamados/juizes.py:920, app/chamados/juizes.py:948, app/chamados/juizes.py:955.

3. **Nenhuma leitura direta ou transitiva encontrada:** app/chamados/services/tarefa_do_dia.py:294, app/chamados/services/tarefa_do_dia.py:332, app/chamados/services/tarefa_do_dia.py:345, app/chamados/services/tarefa_do_dia.py:355, app/chamados/services/tarefa_do_dia.py:378, app/chamados/services/tarefa_do_dia.py:400. Leitores da tranca fora desse caminho: app/chamados/juizes.py:948; app/chamados/models.py:1011.

4. **Leitores de `via_encerramento='competencia_trancada'`:** app/chamados/views_arquivar.py:64, app/chamados/views_arquivar.py:165; app/chamados/services/disputa_emissao.py:1662; app/chamados/services/competencia_trancada.py:259. **Outras leituras de `via_encerramento`:** app/chamados/models.py:681, app/chamados/models.py:974, app/chamados/models.py:1309, app/chamados/models.py:1342; app/chamados/services/adesao.py:90; app/ponto/services/furos_sem_cobranca.py:69, app/ponto/services/furos_sem_cobranca.py:70; app/chamados/management/commands/reabrir_orfas_enterradas.py:135; app/chamados/management/commands/auditar_invariantes_chamados.py:1068.

## 07/11 14:28 · sessao 119a950000f2 · - · - · origem=Ronald (tmux)

Não existe campo `arquivado` em `CelulaDia`; o arquivo da tranca é por chamado (`via_encerramento`), e o véu é `silenciado_ate`. `app/escala/models.py:1635`, `app/chamados/models.py:52`, `app/chamados/models.py:88`

Escritores da tranca: `app/ponto/services/fechamento.py:1421` → `app/chamados/services/competencia_trancada.py:282` → `app/chamados/services/competencia_trancada.py:184` → `app/chamados/models.py:1580` → `app/chamados/models.py:226`, `app/chamados/models.py:243`, `app/chamados/models.py:283`

Desarquivar: `app/chamados/views_arquivar.py:70` → `app/chamados/models.py:1399` → `app/chamados/models.py:247`; véu levantado em `app/chamados/views_arquivar.py:77` → `app/chamados/services/arquivo.py:201`, `app/chamados/services/arquivo.py:205`, `app/chamados/services/arquivo.py:208`

Véu escrito exclusivamente em `app/chamados/services/arquivo.py:166`, persistido em `app/chamados/services/arquivo.py:170`; a tranca não chama essa porta. `app/chamados/services/competencia_trancada.py:184`

Pergunta não recebe mais carimbo da tranca; o legado é desfeito por `app/chamados/services/competencia_trancada.py:251`, `app/chamados/services/competencia_trancada.py:254` → `app/chamados/models.py:2856`, `app/chamados/models.py:2858`. `app/chamados/services/competencia_trancada.py:125`, `app/chamados/models.py:2317`

Leitores específicos de `via_encerramento=competencia_trancada`: `app/chamados/views_arquivar.py:64`, `app/chamados/views_arquivar.py:165`, `app/chamados/services/disputa_emissao.py:1662`, `app/chamados/services/competencia_trancada.py:259`

Demais leitores produtivos de `via_encerramento`: `app/chamados/models.py:974`, `app/chamados/models.py:1309`, `app/chamados/services/adesao.py:90`, `app/ponto/services/furos_sem_cobranca.py:69`, `app/ponto/services/furos_sem_cobranca.py:70`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:135`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1068`

Leitores produtivos de `silenciado_ate`:

- `app/chamados/models.py:484`
- `app/chamados/signals.py:256`, `app/chamados/signals.py:261`
- `app/chamados/views_arquivar.py:136`, `app/chamados/views_arquivar.py:165`, `app/chamados/views_arquivar.py:166`
- `app/chamados/services/verbos.py:45`, `app/chamados/services/verbos.py:48`, `app/chamados/services/verbos.py:51`
- `app/chamados/services/janela_colab.py:73`
- `app/chamados/services/painel.py:190`, `app/chamados/services/painel.py:216`
- `app/chamados/services/fila.py:122`, `app/chamados/services/fila.py:127`
- `app/chamados/services/materializacao.py:703`
- `app/chamados/services/silencio_ausencia.py:80`
- `app/chamados/services/silencio_isentos.py:30`
- `app/chamados/services/arquivo.py:123`, `app/chamados/services/arquivo.py:161`, `app/chamados/services/arquivo.py:193`
- `app/chamados/management/commands/arquivar_sem_dia_irrecuperavel.py:59`
- `app/chamados/management/commands/arquivar_competencia_encerrada.py:100`
- `app/chamados/management/commands/arquivar_prescritos.py:53`, `app/chamados/management/commands/arquivar_prescritos.py:93`
- `app/chamados/management/commands/auditar_invariantes_chamados.py:645`
- `app/ponto/management/commands/escalonar_chamados_supervisao.py:46`
- `app/ponto/management/commands/processar_alertas_avancados.py:112`
- `app/templates/chamados/partials/_busca_arquivados.html:46`, `app/templates/chamados/partials/_busca_arquivados.html:47`
- `app/templates/chamados/partials/_meus_arquivados.html:33`
- `app/templates/chamados/partials/_lista_chamados.html:85`

Leitor mais próximo do véu: `app/chamados/models.py:480`, classificação `ARQUIVADO` em `app/chamados/services/verbos.py:51`; da tranca por colab+dia: `app/chamados/juizes.py:920`, `app/chamados/juizes.py:948`, `app/chamados/juizes.py:955`

## 07/11 14:27 · sessao b2298409d2ee · - · - · origem=Ronald (tmux)

Nenhuma leitura direta ou transitiva encontrada nos corpos chamados por `tarefa_do_dia`: app/chamados/services/tarefa_do_dia.py:397, app/chamados/services/tarefa_do_dia.py:400, app/chamados/services/tarefa_do_dia.py:332, app/chamados/services/tarefa_do_dia.py:335, app/chamados/services/tarefa_do_dia.py:346, app/chamados/services/tarefa_do_dia.py:377, app/ponto/turnos.py:3061, app/ponto/turnos.py:3072.

O teste exige ausência de SQL em `ponto_fechamentomensal`: app/chamados/tests/test_e2_tarefa_do_dia.py:513, app/chamados/tests/test_e2_tarefa_do_dia.py:524.

Leitores existentes fora desses caminhos: app/chamados/juizes.py:948, app/chamados/models.py:1011.

## 07/11 13:43 · sessao 584b3be3dc77 · CODEX-CONSTRUTOR-NEGOCIO · verificador · origem=Code

1. **Média — v4b.r #2: fecha o contraexemplo por recusa, com incompatibilidade nos cinco papéis.** Common-dirs como `/meta/r1` deixam de produzir uma vaga numa pasta compartilhada: agora a chamada termina com rc 2. Porém, a guarda antecede a seleção do papel e também recusa `leitura`, `censo`, `verificador` e `sonda`, embora estes não abram a vaga. É uma restrição nova de admissão, inclusive para leitura de repositórios com metadados separados. (`bin/codex_pergunta.sh:72`, `bin/codex_pergunta.sh:75`, `bin/codex_pergunta.sh:84`, `bin/codex_pergunta.sh:174`, `bin/codex_pergunta.sh:221`)

   A justificativa da triagem sustenta **rejeitar common-dir cujo basename seja diferente de `.git`**. Não sustenta dizer que todo bare ou `--separate-git-dir` foi rejeitado: o teste verifica somente o basename; metadados separados chamados `.git` passam por essa guarda. Isso também não demonstra que seu pai seja a raiz da árvore viva. Ocorrência dessas topologias nesta obra: **[nao verificado]**. (`bin/codex_pergunta.sh:73`, `bin/codex_pergunta.sh:75`, `bin/codex_pergunta.sh:76`)

2. **Média — v4b.r #3: fecha em parte; o diagnóstico foi corrigido, mas a espera não constitui sincronização.** O controle posterior agora exige tokens por transporte; uma linha tardia qualquer já não substitui automaticamente um dos dois controles. O `sed` também limita o diagnóstico aos registros incluídos em `N_DEPOIS`, eliminando a mistura com os controles posteriores apontada anteriormente. (`bin/tests/prova_codex_pergunta_real.sh:108`, `bin/tests/prova_codex_pergunta_real.sh:112`, `bin/tests/prova_codex_pergunta_real.sh:115`)

   O `sleep 3` cobre o timeout de dois segundos de um `recv` em andamento, **desde que o ouvinte volte a executar e registre nesse intervalo**. O timeout não limita o tempo até a escrita da linha. Um escalonamento possível continua sendo: conexão aceita; ouvinte suspenso antes do registro; snapshot ainda sem a linha; ouvinte retomado, registra a conexão e atende os dois controles posteriores. O julgamento usa apenas o snapshot antigo e pode imprimir zero. Reprodução desse escalonamento e ocorrência em produção: **[nao verificado]**. Portanto, a triagem sustenta a correção do caso usual e do diagnóstico, mas não um fechamento absoluto da janela. (`bin/tests/prova_codex_pergunta_real.sh:38`, `bin/tests/prova_codex_pergunta_real.sh:41`, `bin/tests/prova_codex_pergunta_real.sh:103`, `bin/tests/prova_codex_pergunta_real.sh:104`, `bin/tests/prova_codex_pergunta_real.sh:112`, `bin/tests/prova_codex_pergunta_real.sh:115`)

3. **Informativo — v4b.r #4: não fecha a execução comprovada; o descarte documental se sustenta.** Continua possível satisfazer os critérios textuais produzindo `.out` com `rc=` e as assinaturas esperadas. O controle positivo demonstra uma escrita permitida; não demonstra que cada comando proibido foi executado. A própria prova reconhece que zero conexões não demonstra tentativa. (`bin/tests/prova_codex_pergunta_real.sh:60`, `bin/tests/prova_codex_pergunta_real.sh:90`, `bin/tests/prova_codex_pergunta_real.sh:93`, `bin/tests/prova_codex_pergunta_real.sh:106`, `bin/tests/prova_codex_pergunta_real.sh:118`)

   Para o critério **“mede de fora OU rotula” informado na triagem**, o descarte se sustenta: Docker, docker.sock e TCP externo estão explicitamente rotulados como autodeclarados, inclusive junto ao veredito. Não se sustenta ampliar esse descarte para “cada tentativa foi comprovadamente executada”; essa afirmação continua excedendo o juiz implementado. Não é regressão deste delta. (`bin/tests/prova_codex_pergunta_real.sh:83`, `bin/tests/prova_codex_pergunta_real.sh:128`, `bin/tests/prova_codex_pergunta_real.sh:131`, `bin/tests/prova_codex_pergunta_real.sh:135`, `bin/tests/prova_codex_pergunta_real.sh:145`)

4. **Média — v4b.r #5: os descartes são locais ou condicionais; “surrogate sempre gera exceção” não se sustenta como regra geral.**

   | Ponto | Aval |
   |---|---|
   | Permissões e diretórios vazios | Mantém-se o alcance de identidade Git: compara o bit executável do proprietário; diretórios são confrontados pela presença, sem comparação de permissões. Diretório novo permitido pode terminar como `TOCADO`, rc 0. (`bin/codex_pergunta.sh:121`, `bin/codex_pergunta.sh:146`, `bin/codex_pergunta.sh:168`, `bin/codex_pergunta.sh:170`) |
   | Nomes não UTF-8 | Git é decodificado com `replace`, enquanto o percurso físico não passa por essa conversão. Reproduzi **somente em memória**, com o inventário extraído da cópia: stdout `surrogateescape` resulta em dois `TOCADO`, rc 0; stdout estrito resulta em `UnicodeEncodeError`. O resultado acompanha a divergência e o `print` existentes. Reprodução em filesystem real e configuração do stdout de produção: **[nao verificado]**. (`bin/codex_pergunta.sh:129`, `bin/codex_pergunta.sh:135`, `bin/codex_pergunta.sh:144`, `bin/codex_pergunta.sh:168`) |
   | Zero nomes não ASCII e zero filtros/configurações, informados no dado | Sustentam o descarte **para o universo medido informado**, sem demonstrar robustez geral. O inventário permanece comparando bytes físicos com o blob Git. Reprodução dessas medições: **[nao verificado]**. (`bin/codex_pergunta.sh:116`, `bin/codex_pergunta.sh:118`, `bin/codex_pergunta.sh:150`) |
   | Symlink rastreado e inalterado | Permanece fora da classificação se conteúdo e modo coincidirem. O zero informado sustenta ausência desse caso na base medida; não constitui guarda contra ele. (`bin/codex_pergunta.sh:150`, `bin/codex_pergunta.sh:160`, `bin/codex_pergunta.sh:166`) |
   | `.git` da raiz | Continua excluído da varredura e conferido separadamente por hash; no fim não se repete a exigência inicial de arquivo sem symlink. A configuração declara escrita somente em `<wt>/app`; isolamento efetivo e substituição real: **[nao verificado]**. (`bin/codex_pergunta.sh:142`, `bin/codex_pergunta.sh:202`, `bin/codex_pergunta.sh:382`, `bin/codex_pergunta.sh:386`, `bin/codex_pergunta.sh:402`) |

   Assim, o dado de produção permite descartar exposições ausentes naquele retrato, mas não transforma essas limitações do inventário em correções introduzidas pelo delta. (`bin/codex_pergunta.sh:129`, `bin/codex_pergunta.sh:150`, `bin/codex_pergunta.sh:166`; medições independentes: **[nao verificado]**)

5. **Informativo — v4b.r #6: o caso e o mutante novos mordem pelo motivo declarado, por inspeção; a cobertura é estreita.** O caso cria um repositório com common-dir terminado em `r1`, fornece seu próprio HEAD e chama o papel `leitura`. Exige rc 2, mensagem `vaga sem sitio` e ausência de chamada ao Codex falso. Isso isola a guarda nova, em vez de depender de um hash pertencente a outro repositório. (`bin/tests/test_codex_pergunta.sh:92`, `bin/tests/test_codex_pergunta.sh:94`, `bin/tests/test_codex_pergunta.sh:98`, `bin/tests/test_codex_pergunta.sh:338`)

   O mutante troca apenas `exit 2` por `true` na guarda marcada. A mensagem permanece, mas a execução pode prosseguir; portanto, são os critérios de rc e de Codex não chamado que devem acusá-lo. O mecanismo é coerente com a causalidade declarada. Execução efetiva desse caso e desse mutante: **[nao verificado]**. (`bin/codex_pergunta.sh:75`, `bin/tests/test_codex_pergunta.sh:99`, `bin/tests/test_codex_pergunta.sh:101`, `bin/tests/test_codex_pergunta.sh:440`, `bin/tests/test_codex_pergunta.sh:451`, `bin/tests/test_codex_pergunta.sh:458`)

   O caso não exercita bare, metadados separados chamados `.git` nem os outros quatro papéis nessa topologia. Os testes dos quatro papéis continuam usando o repositório convencional. (`bin/tests/test_codex_pergunta.sh:58`, `bin/tests/test_codex_pergunta.sh:338`, `bin/tests/test_codex_pergunta.sh:358`)

   Quanto aos limites anteriores: o dado informado sobre usuário sem privilégio sustenta o descarte operacional do caso `chmod 000`; repetição dessa medição: **[nao verificado]**. FIFO continua sendo o tipo especial exercitado. A sentinela FSMON continua demonstrando recusa e ausência de efeito, sem demonstrar execução de um hook válido: a fixture escreve somente a configuração e não executa Git naquele diretório. (`bin/tests/test_codex_pergunta.sh:289`, `bin/tests/test_codex_pergunta.sh:291`, `bin/tests/test_codex_pergunta.sh:292`, `bin/tests/test_codex_pergunta.sh:323`, `bin/tests/test_codex_pergunta.sh:330`)

6. **Informativo — v4b.r #1: os fechamentos anteriores permanecem; os limites também.** A descoberta de hardlink idêntico permanece no inventário comum às três fases; tipos especiais, erro de percurso, ignorados, autoload e `.git` aninhado conservam suas guardas. O delta não altera sua implementação. (`bin/codex_pergunta.sh:108`, `bin/codex_pergunta.sh:112`, `bin/codex_pergunta.sh:139`, `bin/codex_pergunta.sh:152`, `bin/codex_pergunta.sh:162`, `bin/codex_pergunta.sh:165`, `bin/codex_pergunta.sh:226`, `bin/codex_pergunta.sh:374`, `bin/codex_pergunta.sh:401`)

   A vaga por common-dir e os mutantes corrigidos de `fs_walk`/`p_git` continuam presentes. A distinção entre permissões do wrapper e regras de conduta também permanece documentada. Para topologia e prova do ouvinte, valem os fechamentos parciais dos itens 1–2 desta resposta. (`bin/codex_pergunta.sh:76`, `bin/tests/test_codex_pergunta.sh:428`, `bin/tests/test_codex_pergunta.sh:432`, `CLAUDE.md:350`, `CLAUDE.md:352`)

7. **Informativo — v4b.r #7: escritores, leitores e significados dos caminhos afetados.**

   | Campo/arquivo | Escritor → leitor |
   |---|---|
   | `_CDIR` → `VAGA` | Git fornece common-dir; shell captura → guarda de basename, `dirname`, reserva do caminho da resposta e abertura/flock. **Common-dir é usado como identidade do repositório e seu pai como sítio da vaga.** (`bin/codex_pergunta.sh:72`, `bin/codex_pergunta.sh:75`, `bin/codex_pergunta.sh:76`, `bin/codex_pergunta.sh:77`, `bin/codex_pergunta.sh:220`) |
   | Arquivo `construtor.vaga` | Wrapper cria/abre append no fd 6 → `flock` arbitra ocupação. Fixtures também criam e prendem esse arquivo. Seu conteúdo não é o estado da vaga; o lock é. (`bin/codex_pergunta.sh:221`, `bin/codex_pergunta.sh:222`, `bin/tests/test_codex_pergunta.sh:334`, `bin/tests/test_codex_pergunta.sh:346`) |
   | `$EV`, `porta.novo`, `porta`, `u.sock` | Shell cria EV exclusivo; ouvinte publica a porta e cria o endpoint Unix → shell lê a porta; controles e bloco solicitado usam os endpoints. (`bin/tests/prova_codex_pergunta_real.sh:21`, `bin/tests/prova_codex_pergunta_real.sh:34`, `bin/tests/prova_codex_pergunta_real.sh:35`, `bin/tests/prova_codex_pergunta_real.sh:46`, `bin/tests/prova_codex_pergunta_real.sh:70`, `bin/tests/prova_codex_pergunta_real.sh:71`) |
   | `escuta/conexoes` | Somente o ouvinte acrescenta registros → shell lê contagens, tokens e intervalo diagnóstico. **Mistura controles externos e conexões candidatas a intrusão; a atribuição depende da fase do snapshot, não de origem registrada.** (`bin/tests/prova_codex_pergunta_real.sh:41`, `bin/tests/prova_codex_pergunta_real.sh:52`, `bin/tests/prova_codex_pergunta_real.sh:104`, `bin/tests/prova_codex_pergunta_real.sh:112`, `bin/tests/prova_codex_pergunta_real.sh:115`) |
   | `N_CONTROLE`, `N_DEPOIS`, `OUV_PID`, `rc` | Shell captura contagens/PID → verifica controles e diferença. O juiz escreve `rc`; `_inc` preserva vermelho e sinaliza inconclusivo. (`bin/tests/prova_codex_pergunta_real.sh:44`, `bin/tests/prova_codex_pergunta_real.sh:52`, `bin/tests/prova_codex_pergunta_real.sh:82`, `bin/tests/prova_codex_pergunta_real.sh:85`, `bin/tests/prova_codex_pergunta_real.sh:104`, `bin/tests/prova_codex_pergunta_real.sh:107`, `bin/tests/prova_codex_pergunta_real.sh:115`) |
   | `$EV/p`, `prova/*.out`, `$EV/prova` | Shell escreve pergunta → wrapper a incorpora; Codex é instruído a escrever `.out` → prova copia evidência, mas julga os originais na wt. **O `.out` serve como registro da suposta tentativa e, em parte dos casos, como relato da negação.** (`bin/tests/prova_codex_pergunta_real.sh:53`, `bin/codex_pergunta.sh:323`, `bin/tests/prova_codex_pergunta_real.sh:61`, `bin/tests/prova_codex_pergunta_real.sh:81`, `bin/tests/prova_codex_pergunta_real.sh:89`, `bin/tests/prova_codex_pergunta_real.sh:128`) |
   | `_INV0`, `_INV1`, `.inventario`, `.recusa` | Python produz inventário → shell exige vazio no início/revalidação; no fim aceita `TOCADO` com rc 0 e gera recusa nos erros. Testes leem os artefatos; prova os copia. **Saída não vazia significa recusa na admissão, mas pode significar alteração permitida no fim.** (`bin/codex_pergunta.sh:226`, `bin/codex_pergunta.sh:374`, `bin/codex_pergunta.sh:401`, `bin/codex_pergunta.sh:403`, `bin/codex_pergunta.sh:406`, `bin/tests/test_codex_pergunta.sh:106`, `bin/tests/test_codex_pergunta.sh:242`, `bin/tests/prova_codex_pergunta_real.sh:80`) |
   | `sep_meta/r1`, `sep_wt/s.txt`, `HQ`, variáveis `*_X`, `r_topologia` | Caso novo cria os metadados e o arquivo, Git registra o commit; caso fornece HEAD e parâmetros → `_roda` traduz para argumentos/env. `r_topologia` é destino de resposta; a recusa original deve ocorrer antes da chamada ao falso Codex. (`bin/tests/test_codex_pergunta.sh:92`, `bin/tests/test_codex_pergunta.sh:94`, `bin/tests/test_codex_pergunta.sh:339`, `bin/tests/test_codex_pergunta.sh:341`) |
   | `MUTANTES`, wrapper temporário, `mut_saida`, `mutantes.txt` | Tabela declara substituição; Python lê original e escreve mutante → caso produz saída; juiz procura `FALHA`, escreve tabela e eventualmente a copia para o destino configurado. (`bin/tests/test_codex_pergunta.sh:440`, `bin/tests/test_codex_pergunta.sh:446`, `bin/tests/test_codex_pergunta.sh:454`, `bin/tests/test_codex_pergunta.sh:458`, `bin/tests/test_codex_pergunta.sh:460`, `bin/tests/test_codex_pergunta.sh:468`) |

8. **Informativo — v4b.r #8: limite desta verificação.** Conferi a sintaxe dos três scripts com `bash -n` e fiz a simulação do inventário exclusivamente em memória. Suíte, mutantes, prova real, isolamento efetivo e medições de produção não foram executados: **[nao verificado]**. Os testes criam fixtures e a prova chama o wrapper, incompatíveis com executar esses fluxos sob a regra de não criar arquivos. (`bin/tests/test_codex_pergunta.sh:18`, `bin/tests/test_codex_pergunta.sh:55`, `bin/tests/prova_codex_pergunta_real.sh:21`, `bin/tests/prova_codex_pergunta_real.sh:77`)

   Correspondência criptográfica da cópia com `9d1d8147` e o delta histórico completo desde `b7e90708`: **[nao verificado]**.

## 07/11 13:38 · sessao b83d84586d6e · FIO-E0-V4-E1 · verificador · origem=Code

**Veredito: a cura é parcial. Permanecem achados PROCEDENTES em `aplicar()`, seleção da canônica, ordem de travas e alcance dos selos.** (`app/chamados/management/commands/backfill_decisoes_pergunta.py:164`; `app/chamados/services/declaracao_texto.py:261`; `app/chamados/models.py:688`; `app/chamados/tests/test_e1_v5_ordem_trava.py:358`)

Execução dos testes, resultados dos logs anexados, estado das migrações e ocorrência em produção: **[nao verificado]**. Confirmação independente do hash/pai: **[nao verificado]**. [nao verificado]

**1. Achados de `codex_e1_v6.r`: cura na origem, parcial ou intocado**

| Achado | Julgamento |
|---|---|
| **A — significado dos campos** | **PROCEDENTE, parcial.** Corrige o discriminador `conversa` nos filtros alterados, mas conserva dois domínios de `data_evento` e a classificação “nova” pelo nascimento da Disputa. (`app/chamados/services/declaracao_texto.py:241`; `app/chamados/models.py:2620`; `app/chamados/services/fio.py:1103`) |
| **B — NULL no horizonte** | **PROCEDENTE, curado na seleção.** `q_do_horizonte` ficou estrito; NULL fica no contador/DRY e separado em `admin_decidiu_sem_ato(sem_dia=True)`. A garantia não foi incorporada ao escritor `aplicar()`. (`app/chamados/services/fio.py:1033`; `app/chamados/services/fio.py:1040`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:97`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:168`) |
| **B — DIVERGE por empresa** | **PROCEDENTE, curado no CLI; parcial na porta.** `handle` agora recusa, mas a chamada direta de `aplicar()` não verifica essa condição. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:315`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:147`) |
| **C — universo das travas** | **PROCEDENTE, parcial.** A antecipação cobre outras disputas abertas nos sete sítios alterados; permanece Chamado → Disputa na entrada por S143 e há caminhos Pergunta → Disputa → Chamado intocados. (`app/chamados/models.py:2504`; `app/chamados/models.py:688`; `app/chamados/services/disputa_emissao.py:1680`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:151`) |
| **D — conversa repetida de episódio retratado** | **PROCEDENTE, curado no ramo sequencial.** Há consulta antes de `abrir_conversa`; concorrência continua sem serialização nessa porta. O RED novo aceita zero perguntas. (`app/chamados/services/disputa_emissao.py:436`; `app/chamados/models.py:2634`; `app/chamados/tests/test_e1_v6_horizonte.py:421`) |
| **E1 — primeira reutilizável versus canônica** | **PROCEDENTE, parcial.** Corrige o contraexemplo com duas reutilizáveis, mas continua escolhendo a canônica somente depois de filtrar as reutilizáveis. (`app/chamados/services/declaracao_texto.py:240`; `app/chamados/services/declaracao_texto.py:261`; `app/chamados/tests/test_e1_v6_horizonte.py:374`) |
| **E2/E3/E4 — conversa nos filtros de fato** | **PROCEDENTE, curado quanto à conversa nesses sítios.** Os filtros passaram a exigir `conversa=False`; continuam filtros próprios, sem equivalência integral demonstrada com a identidade da porta. (`app/chamados/services/declaracao_texto.py:241`; `app/chamados/services/disputa_emissao.py:399`; `app/chamados/services/disputa_emissao.py:418`; `app/chamados/services/disputa_emissao.py:1313`; `app/chamados/models.py:2576`) |
| **F — recusa antes de escrever** | **PROCEDENTE, parcial.** Cura mérito/exportação já presentes na pré-checagem; não fecha o intervalo entre conferência e escrita nem garante rollback do universo. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:147`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:164`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:186`) |
| **G — ausência da 0052** | **PROCEDENTE, curado.** Registro ausente ou instante ausente agora alarma como “nova”. O uso de `disputa.aberta_em` e a impressão truncada permanecem. (`app/chamados/services/fio.py:1113`; `app/chamados/services/fio.py:1103`; `bin/placar_code.sh:198`) |
| **H — cobertura dos selos** | **PROCEDENTE, parcial.** Acrescenta observação do marcador de universo e o contraexemplo antigo da canônica; mantém rastreador por tabela, relógios das fixtures e cobertura insuficiente da fronteira BATIDA no banco. (`app/chamados/tests/test_e1_v5_ordem_trava.py:358`; `app/chamados/tests/test_e1_v5_ordem_trava.py:45`; `app/chamados/tests/test_e1_v6_horizonte.py:210`; `app/chamados/tests/test_e1_v6_horizonte.py:94`) |
| **I — classes reincidentes** | **PROCEDENTE, parcial.** Permanecem seleção anterior à canônica do universo completo, inversões de trava e sinais síncronos anteriores ao julgamento da célula. (`app/chamados/services/declaracao_texto.py:261`; `app/chamados/models.py:688`; `app/chamados/signals.py:92`; `app/chamados/signals.py:180`) |

**Há caminho de escrita sem horizonte estrito:** chamada direta `aplicar([], [pk_antigo])`, com pergunta `admin_decidiu`, sem decisão e competência não exportada, passa pela conferência e registra SILENCIO. A leitura final é por PK, sem `q_do_horizonte`; também não exige novamente carimbo preenchido ou ausência de validação. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:150`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:168`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:169`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:172`)

**Há caminho de recusa depois da escrita:** a pré-checagem passa; o laço de SILENCIO encerra seus atomics; mérito aparece antes da leitura do grupo; a segunda leitura acrescenta `merito_recusado` e continua. O retorno pode conter simultaneamente escrita e recusa, e `handle` imprime “nada escrito”. Reprodução concorrente: **[nao verificado]**. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:164`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:186`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:197`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:329`)

A competência exportada também não é relida nos laços sob trava. `dedup_perguntas_fato`, intocado, continua outro caminho sem essas recusas e sem repetir o horizonte na releitura por PK. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:158`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:183`; `app/chamados/management/commands/dedup_perguntas_fato.py:94`; `app/chamados/management/commands/dedup_perguntas_fato.py:101`)

**O contraexemplo remanescente de E1:** abaixo do horizonte, `p1` respondida, validada e materializada; `p2` muda sem carimbo. A canônica do grupo completo é `p1`; a seleção entre reutilizáveis escolhe `p2`, e a declaração dispensa `abrir`. Portanto a mudança de função não eliminou a divergência de universo. (`app/chamados/services/fio.py:984`; `app/chamados/juizes.py:1137`; `app/chamados/juizes.py:1228`; `app/chamados/services/declaracao_texto.py:240`; `app/chamados/services/declaracao_texto.py:328`; `app/chamados/services/declaracao_texto.py:338`; `app/chamados/models.py:2666`)

**NAO PROCEDE** divergência de operador: helper, modelo e migration usam início inclusivo `>= H`; `--ate` usa limite superior exclusivo deliberado. Tampouco procede exigir horizonte nas portas normais, que continuam vendo o fato antigo. (`app/chamados/services/fio.py:1033`; `app/chamados/models.py:3177`; `app/chamados/migrations/0053_pergunta_unica_por_familia.py:33`; `app/chamados/services/fio.py:1076`; `app/chamados/models.py:2666`)

**2. `trava_na_ordem`: chamadores, cobertura e inversões**

Sem flag, trava Colaborador, Disputas explícitas e Chamados explícitos. Com flag, acrescenta todas as Disputas abertas do colaborador, os pais dessas abertas e todos os Chamados `VIVOS`; cada tabela é ordenada por PK. Não trava Pergunta e não acrescenta automaticamente o pai de uma Disputa explícita fechada. (`app/chamados/models.py:2500`; `app/chamados/models.py:2506`; `app/chamados/models.py:2508`; `app/chamados/models.py:2510`; `app/chamados/models.py:2516`)

| Chamador com flag | O que acrescenta explicitamente antes da Pergunta |
|---|---|
| `DisputaSupervisao.fechar` | Própria disputa, pai e destinos das vivas. (`app/chamados/models.py:1874`) |
| `fechar_beco` | Própria disputa, pai e destinos resolvidos. (`app/chamados/services/becos.py:51`) |
| Resolver público | Chamado recebido; cria atomic somente se estiver fora de transação. A via de competência trancada retorna sem escrita. (`app/chamados/services/disputa_emissao.py:1674`; `app/chamados/services/disputa_emissao.py:1679`; `app/chamados/services/disputa_emissao.py:1701`) |
| Folga já declarada | Disputa, pai e destino da pergunta. (`app/chamados/services/validacao.py:285`) |
| Declaração de folga | Mesmas listas. (`app/chamados/services/validacao.py:317`) |
| Validação normal | Disputa, pai, destino e clones da pergunta original. (`app/chamados/services/validacao.py:431`) |
| `fechar_o_fio_do_dia` | Disputas das perguntas vivas, Chamados vivos do dia e destinos. (`app/ponto/services/veredito_celula.py:182`) |

**PROCEDENTE — existe Pergunta antes de Disputa/Chamado.** `reabrir_orfas_enterradas` trava apenas Colaborador, descarimba/salva Pergunta, depois reabre Disputa e pai. É caminho intocado pelo diff. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:150`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:151`; `app/chamados/models.py:2989`; `app/chamados/models.py:2907`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:159`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:166`)

**PROCEDENTE — S143 ainda permite Chamado antes de Disputa.** O gate trava Chamado, o escritor salva Chamado, S143 chama o resolver e somente então o envelope adquire Disputas. A trava ampliada não retroage às aquisições anteriores. (`app/chamados/models.py:688`; `app/chamados/models.py:291`; `app/chamados/signals.py:92`; `app/chamados/services/disputa_emissao.py:1680`)

A validação ainda chama o materializador geral depois de sair de seu atomic local. Esse materializador percorre irmãs, reconcilia destinos/clones e pode fechar Disputa; a antecipação local não basta para certificar o caminho inteiro. (`app/chamados/services/validacao.py:469`; `app/chamados/services/materializacao.py:1366`; `app/chamados/services/materializacao.py:762`; `app/chamados/services/materializacao.py:772`; `app/chamados/services/materializacao.py:1324`)

**PROCEDENTE — ordem dos sinais intocada:** S88 reconcilia pai sincronamente; S143 resolve perguntas antes do receiver que julga a célula; S144 materializa sincronamente. O savepoint de S143 permanece, portanto **NAO PROCEDE** regressão demonstrada do BUG120. (`app/chamados/signals.py:42`; `app/chamados/signals.py:91`; `app/chamados/signals.py:140`; `app/chamados/signals.py:180`)

**3. Quem escreve, quem lê e qual significado**

| Campo/valor | Escritor/fonte | Leitores e significado |
|---|---|---|
| `via_resolucao` e `materializada_em` | `_escrever_carimbo` atribui o trio; portas salvam; `descarimbar_em_lote` tem UPDATE próprio. (`app/chamados/models.py:2793`; `app/chamados/models.py:2848`; `app/chamados/models.py:2885`; `app/chamados/models.py:2955`) | C6, SQL de vivas, dedups, ressuscitação e backfill. Carimbo representa processamento/encerramento; não prova isoladamente batida materializada. (`app/chamados/juizes.py:1210`; `app/chamados/models.py:3043`; `app/chamados/models.py:1450`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:95`) |
| Troca apenas de `via_resolucao` | Também existem `encerrar_por_fato_no_espelho` e `relavrar`, portas declaradas; não surgiram neste diff. (`app/chamados/models.py:2964`; `app/chamados/services/relavra_via.py:115`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:51`) | Logo “toda via passa exclusivamente por `_escrever_carimbo`” é literalmente falso. (`app/chamados/models.py:2966`; `app/chamados/services/relavra_via.py:123`) |
| `materializada_em` da duplicata | A retratação preserva carimbo anterior quando existente. (`app/chamados/services/fio.py:1002`; `app/chamados/models.py:2877`) | Não significa necessariamente instante da retratação. O backfill usa o instante como SILENCIO somente no ramo `admin_decidiu`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:169`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:173`) |
| `data_evento` | Nasce em `_nascer`; reapontamento pode datar órfã; rechaveamento muda o dia. (`app/chamados/models.py:2714`; `app/chamados/services/fio.py:841`; `app/chamados/services/fio.py:948`) | `abrir`: dia do turno. `abrir_conversa`: dia civil da emissão. O discriminador continua obrigatório para distinguir os domínios. (`app/chamados/models.py:2603`; `app/chamados/models.py:2615`; `app/chamados/models.py:2620`) |
| `HORIZONTE_DO_FATO` | Literal do módulo, reexportado pela classe; migration mantém cópia congelada. (`app/chamados/models.py:2254`; `app/chamados/models.py:2366`; `app/chamados/migrations/0053_pergunta_unica_por_familia.py:28`) | Índices e agrupamento: início inclusivo por `data_evento`. Competência do backfill vem da autoridade `competencia_do_fato`. (`app/chamados/models.py:3177`; `app/chamados/services/fio.py:1033`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:113`) |
| `VIAS_FORA_DO_FATO` | Tupla única do módulo, reexportada. (`app/chamados/models.py:2244`; `app/chamados/models.py:2365`) | Identidade, índices, agrupamento e guardas de descarimbo excluem essas vias independentemente da resposta. (`app/chamados/models.py:2547`; `app/chamados/models.py:3176`; `app/chamados/services/fio.py:1053`; `app/chamados/models.py:2956`) |
| `VIAS_QUE_VENCEM_A_RESPOSTA` | Tranca concatenada à tupla anterior. (`app/chamados/models.py:2375`) | C6 e SQL de vivas exigem via **e carimbo** antes de considerar resposta. (`app/chamados/juizes.py:1148`; `app/chamados/models.py:3043`) |

**NAO PROCEDE** divergência inclusivo/exclusivo nesses predicados; **PROCEDENTE** afirmar que nem todos os usos distinguem turno de dia civil: o backfill seleciona `admin_decidiu` sem filtrar `conversa` e calcula competência diretamente de `data_evento`. Isso não demonstra dano em produção. (`app/chamados/services/fio.py:1033`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:94`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:113`)

`SILENCIO` é ato de decisão, não valor de `via_resolucao`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:173`)

**4. Ressuscitação e mordida de `RetratadaNaoRessuscitaTest`**

**NAO PROCEDE**, nos caminhos examinados, devolver a retratada ao fato: o universo exclui `VIAS_FORA_DO_FATO`; o UPDATE do lote repete a exclusão; descarimbo individual e carimbo normal também recusam a retirada dessa terminalidade. (`app/chamados/models.py:1455`; `app/chamados/models.py:2955`; `app/chamados/models.py:2901`; `app/chamados/models.py:2816`)

**O selo tem mordida comportamental:** exige retratação pela porta, preservação de via/carimbo, pergunta morta, nascimento de outra PK e exatamente uma viva. O mutante remove conjuntamente as guardas baseadas na tupla; não prova cada guarda isoladamente. (`app/chamados/tests/test_e1_v6_horizonte.py:487`; `app/chamados/tests/test_e1_v6_horizonte.py:497`; `app/chamados/tests/test_e1_v6_horizonte.py:506`; `app/chamados/tests/test_e1_v6_horizonte.py:522`)

**PROCEDENTE, porém, o limite da alegação sobre o índice:** o caso usa `posto_desativado`, família BATIDA, sem fornecer batida nem `ultima_batida` no contexto. A origem permanece nula e o índice de episódio exclui justamente esse caso. Estar acima do horizonte não faz esse teste exercitar o índice. (`app/chamados/catalogo/motivos.py:38`; `app/chamados/tests/test_e1_v6_horizonte.py:478`; `app/chamados/tests/test_e1_v6_horizonte.py:483`; `app/chamados/models.py:3154`; `app/chamados/models.py:3183`)

**5. Classes que já reprovaram a obra**

| Classe | Julgamento |
|---|---|
| Segundo juiz/seleção paralela | **PROCEDENTE:** filtro de reutilizáveis ainda precede a canônica do universo completo. (`app/chamados/services/declaracao_texto.py:261`; `app/chamados/models.py:2666`) |
| Segundo escritor novo fora da porta | **NAO PROCEDE como novidade demonstrada deste diff:** as trocas de via adicionais já são portas declaradas. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:51`) |
| Relógio fora da borda | **PROCEDENTE nos selos remanescentes:** abertura real da Disputa e fechamento com `now()` continuam nas fixtures apesar da promessa de instantes literais. (`app/chamados/models.py:1709`; `app/chamados/models.py:2760`; `app/chamados/tests/test_e1_v6_horizonte.py:14`; `app/chamados/tests/test_e1_v6_horizonte.py:210`) |
| Derivação substituta da fonte | **PROCEDENTE:** “nova pergunta” continua medida por abertura da Disputa, com subcontagem explicitamente admitida. (`app/chamados/services/fio.py:1103`) |
| Selo/RED insuficiente | **PROCEDENTE:** D aceita zero; universo observa a flag, não todas as linhas bloqueadas; teste do “pai” afirma apenas lista devolvida. (`app/chamados/tests/test_e1_v6_horizonte.py:421`; `app/chamados/tests/test_e1_v5_ordem_trava.py:358`; `app/chamados/tests/test_e1_v5_ordem_trava.py:422`) |
| Ordem de trava | **PROCEDENTE:** persistem os caminhos descritos no item 2. (`app/chamados/models.py:688`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:151`) |
| Pouso antes do pré-requisito | **[nao verificado]** na instalação. A migration declara dependência da 0052 e guarda de colisões antes dos índices. (`app/chamados/migrations/0053_pergunta_unica_por_familia.py:51`; `app/chamados/migrations/0053_pergunta_unica_por_familia.py:56`) |
| Campo com dois significados | **PROCEDENTE:** `data_evento` continua turno/conversa civil; as curas corrigem alguns consumidores. (`app/chamados/models.py:2603`; `app/chamados/models.py:2615`; `app/chamados/services/disputa_emissao.py:1313`) |

**NAO PROCEDE vacuidade generalizada:** há testes com resultados concretos; as insuficiências apontadas são específicas. (`app/chamados/tests/test_e1_v6_horizonte.py:311`; `app/chamados/tests/test_e1_v6_horizonte.py:374`; `app/chamados/tests/test_e1_v6_horizonte.py:497`)

**6. Três selos e quatro TETOS**

- **Fonte única — NAO PROCEDE afrouxamento:** a concatenação conserva os mesmos três valores e o selo continua exigindo exatamente as duas listas nomeadas. (`app/chamados/models.py:2375`; `app/chamados/tests/test_o261e_f2_fila.py:118`; `app/chamados/tests/test_o261e_f2_fila.py:124`)
- **Allowlist de vitalidade — NAO PROCEDE ampliação demonstrada:** acompanha o corpo renomeado; o envelope contém a trava e o desvio sem escrita. (`app/chamados/tests/test_contract_leitores_vitalidade.py:90`; `app/chamados/services/disputa_emissao.py:1674`; `app/chamados/services/disputa_emissao.py:1680`)
- **MARCO — NAO PROCEDE anistia geral:** a exceção é nominal por arquivo/nome e corresponde à fronteira histórica declarada. Ela não prova o valor nem a equivalência por empresa. (`app/core/datas_cravadas.py:37`; `app/core/datas_cravadas.py:126`; `app/chamados/models.py:2250`)
- **TETOS — NAO PROCEDE excesso comprovado além dos aumentos declarados:** ficaram 23, 55, 200 e 246; a cobrança continua por total de queries. Os deltas medidos e a ausência de folga efetiva são **[nao verificado]**. (`app/ponto/tests/test_selo_chokepoint_escrita.py:116`; `app/ponto/tests/test_selo_chokepoint_escrita.py:118`; `app/ponto/tests/test_selo_chokepoint_escrita.py:161`; `app/ponto/tests/test_selo_chokepoint_escrita.py:162`; `app/ponto/tests/test_selo_chokepoint_escrita.py:240`)

A construção sustenta quantidade fixa de consultas **por entrada** na trava; não sustenta tempo ou contenção constantes: o número de linhas bloqueadas cresce com as disputas abertas e Chamados vivos do colaborador. Os tetos de queries não medem esse crescimento. (`app/chamados/models.py:2504`; `app/chamados/models.py:2514`; `app/ponto/tests/test_selo_chokepoi

[truncado em 20000 de 20019 caracteres]

## 07/11 13:36 · sessao 8bf8acf0802b · - · - · origem=Ronald (tmux)

- **E — PROCEDENTE, cura parcial.** As funções agora delegam a escolha à `canonica_do_grupo`, mas somente **depois** de excluir as não reutilizáveis. Cenário legítimo abaixo do horizonte: `p1` respondida, validada e materializada; `p2` muda sem carimbo. A canônica global é `p1`, mas ambos os seletores escolhem `p2`: a validação mata `p1` no C6, tornando-a não reutilizável. A declaração reutiliza `p2` e dispensa `abrir`. Portanto permanece seleção anterior à autoridade do universo completo. (`app/chamados/services/declaracao_texto.py:240`; `app/chamados/services/declaracao_texto.py:261`; `app/chamados/services/declaracao_texto.py:328`; `app/chamados/services/declaracao_texto.py:338`; `app/chamados/services/declaracao_texto.py:625`; `app/chamados/services/fio.py:984`; `app/chamados/juizes.py:1137`; `app/chamados/juizes.py:1228`; `app/chamados/models.py:2666`)

- **E2/E3/E4 — NAO PROCEDE repetir o defeito específico de conversa nesses filtros corrigidos.** `_candidata_reusavel`, `_ja_perguntou` e os dois dedups datados agora exigem `conversa=False`. Os filtros continuam próprios, sem passar integralmente por `q_dos_fatos`; a cura não demonstra equivalência completa de identidade/instância. (`app/chamados/services/declaracao_texto.py:241`; `app/chamados/services/disputa_emissao.py:399`; `app/chamados/services/disputa_emissao.py:418`; `app/chamados/services/disputa_emissao.py:1313`; `app/chamados/models.py:2576`)

- **D — PROCEDENTE, cura sequencial; selo insuficiente.** O ramo sem dia verifica existência de conversa antes de `abrir_conversa`. Entretanto, o RED aceita `count()==0` e não exige `conversa=True`: pode passar se nenhuma pergunta nascer. A sobrescrita com `pass` não executa o teste herdado naquela classe, embora a classe mãe continue tendo seu próprio teste. (`app/chamados/services/disputa_emissao.py:436`; `app/chamados/services/disputa_emissao.py:439`; `app/chamados/tests/test_e1_v6_horizonte.py:409`; `app/chamados/tests/test_e1_v6_horizonte.py:421`)

- **4 — NAO PROCEDE ressuscitação de `VIAS_FORA_DO_FATO` pelos dois mecanismos examinados.** O universo exclui essas vias e o `UPDATE` do lote repete a exclusão sobre o estado consultado pelo banco. O descarimbo individual, o carimbo normal e a troca para identidade interna também recusam retirar a linha do conjunto terminal. (`app/chamados/models.py:1455`; `app/chamados/models.py:2955`; `app/chamados/models.py:2901`; `app/chamados/models.py:2816`; `app/chamados/models.py:2875`)

- **RetratadaNaoRessuscitaTest morde o comportamento anunciado, com limite.** Exige retratação efetiva pela porta, conservação da via/carimbo, morte pelo juiz, nascimento de outra PK e exatamente uma viva. O mutante remove `VIAS_FORA_DO_FATO` de **todas** as guardas simultaneamente: prova proteção conjunta, não a necessidade individual do filtro do lote ou do universo. Execução do teste: **[nao verificado]**. (`app/chamados/tests/test_e1_v6_horizonte.py:487`; `app/chamados/tests/test_e1_v6_horizonte.py:497`; `app/chamados/tests/test_e1_v6_horizonte.py:506`; `app/chamados/tests/test_e1_v6_horizonte.py:522`)

Mapa dos campos:

| Campo/valor | Escritor | Leitor e significado |
|---|---|---|
| `via_resolucao` + `materializada_em` | `_escrever_carimbo` atribui o trio; carimbar/descarimbar o salvam; lote tem `UPDATE` próprio. (`app/chamados/models.py:2793`; `app/chamados/models.py:2848`; `app/chamados/models.py:2885`; `app/chamados/models.py:2955`) | Carimbo de processamento/encerramento, **não prova isolada de batida**: admin não planta batida e C6 consulta lastro. (`app/chamados/models.py:2746`; `app/chamados/juizes.py:1210`) |
| Troca somente de via | Existem duas portas declaradas: `encerrar_por_fato_no_espelho` e `relavrar`; esta só aceita vias derivadas. Não são escritores novos deste diff. (`app/chamados/models.py:2964`; `app/chamados/services/relavra_via.py:115`; `app/chamados/services/relavra_via.py:77`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:51`) | A frase “toda via é escrita exclusivamente em `_escrever_carimbo`” seria inexata; o selo distingue esses escritores. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:35`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:51`) |
| `materializada_em` na duplicata | Preserva instante/autor anteriores quando já havia carimbo. Portanto não é necessariamente instante da retratação. (`app/chamados/services/fio.py:1002`; `app/chamados/models.py:2877`) | Backfill toma esse instante como `ato_em` de SILENCIO somente quando via é `admin_decidiu`; não interpreta todo carimbo como SILENCIO. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:169`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:173`) |
| `data_evento` | `_nascer`; reapontamento data órfã e rechaveamento muda o dia. (`app/chamados/models.py:2714`; `app/chamados/services/fio.py:841`; `app/chamados/services/fio.py:948`) | Dia do turno em `abrir`; dia civil da emissão em `abrir_conversa`. O discriminador é `conversa`. (`app/chamados/models.py:2603`; `app/chamados/models.py:2613`; `app/chamados/models.py:2620`) |
| `HORIZONTE_DO_FATO` | Literal e reexportação da classe. (`app/chamados/models.py:2254`; `app/chamados/models.py:2366`) | Índices/agrupamento usam início inclusivo `>=`; `NULL` só entra na consulta ampliada de contadores. Competência usa autoridade da janela, não mês civil. (`app/chamados/models.py:3177`; `app/chamados/models.py:3183`; `app/chamados/services/fio.py:1033`; `app/chamados/services/fio.py:1040`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:113`) |
| `VIAS_FORA_DO_FATO` | Tupla fonte do módulo, reexportada pela classe. (`app/chamados/models.py:2244`; `app/chamados/models.py:2365`) | Índices, identidade, universo da ressuscitação e descarimbo leem o conjunto terminal. (`app/chamados/models.py:3176`; `app/chamados/models.py:2547`; `app/chamados/models.py:1455`; `app/chamados/models.py:2956`) |
| `VIAS_QUE_VENCEM_A_RESPOSTA` | Soma da tranca com a fonte anterior. (`app/chamados/models.py:2375`) | C6 e pré-filtro SQL usam via **com carimbo** antes da resposta. (`app/chamados/juizes.py:1148`; `app/chamados/models.py:3043`) |

**6 — NAO PROCEDE afrouxamento demonstrado dos três selos além das exceções declaradas.** A soma preserva os mesmos três valores e o teste continua exigindo exatamente duas listas literais nomeadas. A allowlist acompanha o corpo renomeado. A declaração MARCO é nominal por caminho/nome; não comprova o valor nem a equivalência da janela por empresa. (`app/chamados/models.py:2375`; `app/chamados/tests/test_o261e_f2_fila.py:118`; `app/chamados/tests/test_contract_leitores_vitalidade.py:90`; `app/core/datas_cravadas.py:37`; `app/core/datas_cravadas.py:126`)

**Tetos: NAO PROCEDE extrapolação estática comprovada; medição permanece [nao verificado].** Os quatro valores são 23/55/200/246; continuam cobrados por `assertLessEqual`, e os cenários da batida exigem fechamento real. A trava usa quantidade fixa de consultas por entrada, mas a quantidade de linhas bloqueadas cresce com o universo do colaborador: custo constante de consultas não prova tempo ou contenção constantes. (`app/ponto/tests/test_selo_chokepoint_escrita.py:116`; `app/ponto/tests/test_selo_chokepoint_escrita.py:118`; `app/ponto/tests/test_selo_chokepoint_escrita.py:161`; `app/ponto/tests/test_selo_chokepoint_escrita.py:162`; `app/ponto/tests/test_selo_chokepoint_escrita.py:240`; `app/ponto/tests/test_selo_chokepoint_escrita.py:385`; `app/ponto/tests/test_selo_chokepoint_escrita.py:390`; `app/chamados/models.py:2500`; `app/chamados/models.py:2516`)

## 07/11 13:35 · sessao 36a63810e562 · - · - · origem=Ronald (tmux)

**C / ordem global: PROCEDENTE, cura parcial.** A flag amplia corretamente os sete chamadores alterados, mas não assegura `Colaborador → Disputa → Chamado → Pergunta` em todo caminho. (`app/chamados/models.py:2500`; `app/chamados/models.py:2506`; `app/chamados/models.py:2516`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:146`)

`trava_na_ordem` trava primeiro o Colaborador; depois, por PK, as Disputas explícitas e, com a flag, todas as abertas do colaborador; por fim, por PK, os Chamados explícitos, pais das disputas abertas encontradas e todos os Chamados em `VIVOS`. Não trava Pergunta. Sem flag, trava apenas as listas explícitas e **não acrescenta automaticamente seus pais**. (`app/chamados/models.py:2500`; `app/chamados/models.py:2502`; `app/chamados/models.py:2508`; `app/chamados/models.py:2510`; `app/chamados/models.py:2514`)

| Chamador com flag | Listas adicionais e efeito |
|---|---|
| `DisputaSupervisao.fechar` | Própria disputa, pai e destinos das vivas; universo antes de `fechar_por_admin`, save da Disputa e S88/S143/S144. (`app/chamados/models.py:1874`; `app/chamados/models.py:1885`; `app/chamados/models.py:1900`) |
| `fechar_beco` | Própria disputa, pai e destinos resolvidos; universo antes do primeiro resolver. (`app/chamados/services/becos.py:41`; `app/chamados/services/becos.py:51`; `app/chamados/services/becos.py:54`) |
| Resolver público | Chamado recebido mais universo; atomic próprio somente quando ainda não há transação. Via competência trancada desvia para retorno sem escrita. (`app/chamados/services/disputa_emissao.py:1674`; `app/chamados/services/disputa_emissao.py:1679`; `app/chamados/services/disputa_emissao.py:1701`) |
| Folga já decidida | Disputa, pai e destino da pergunta mais universo antes da validação e fechamento do fio. (`app/chamados/services/validacao.py:285`; `app/chamados/services/validacao.py:288`; `app/chamados/services/validacao.py:296`) |
| Declarar folga | Mesmas listas antes da validação e de `declarar_dia`. (`app/chamados/services/validacao.py:317`; `app/chamados/services/validacao.py:320`) |
| Validação normal | Disputa, pai, destino e clones da pergunta mais universo, antes do carimbo. (`app/chamados/services/validacao.py:431`; `app/chamados/services/validacao.py:435`) |
| `fechar_o_fio_do_dia` | Disputas das perguntas vivas, Chamados vivos do dia e destinos mais universo, antes das perguntas e do reconciliar. (`app/ponto/services/veredito_celula.py:182`; `app/ponto/services/veredito_celula.py:187`; `app/ponto/services/veredito_celula.py:200`) |

**Contraexemplo incontornável Pergunta antes de Disputa/Chamado, intocado pelo diff:** `reabrir_orfas_enterradas` abre atomic, trava somente Colaborador, chama `p.reabrir_fantasma()`, depois reabre Disputa e pai. `reabrir_fantasma → descarimbar → save` grava Pergunta; `reabrir_sistema` só depois grava Disputa. Portanto, segura a trava implícita de Pergunta antes das demais. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:146`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:150`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:159`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:166`; `app/chamados/models.py:2989`; `app/chamados/models.py:2907`; `app/chamados/models.py:2129`)

**S143 ainda permite Chamado antes de Disputa:** o gate `_recarregar_estado_se_mudou` trava Colaborador e Chamado; `_gravar_estado` apenas retrava Colaborador e salva Chamado; S143 chama o wrapper, que então adquire as Disputas. A cura do envelope não retroage à aquisição anterior do Chamado. (`app/chamados/models.py:683`; `app/chamados/models.py:688`; `app/chamados/models.py:280`; `app/chamados/models.py:291`; `app/chamados/signals.py:91`; `app/chamados/services/disputa_emissao.py:1680`)

**Validação continua com trecho fora do atomic local:** após o bloco que antecipa o universo, chama o materializador geral, que percorre todas as irmãs. Não há antecipação de universo no próprio materializador; seus ramos reconciliam destinos/clones e podem fechar Disputa. A presença de transação externa que retenha as travas nesse caminho é `[nao verificado]`. (`app/chamados/services/validacao.py:467`; `app/chamados/services/validacao.py:469`; `app/chamados/services/materializacao.py:1360`; `app/chamados/services/materializacao.py:1367`; `app/chamados/services/materializacao.py:762`; `app/chamados/services/materializacao.py:1324`)

**Ordem dos sinais contra a lâmpada: PROCEDENTE, não tocada.** S88 reconcilia pai sincronamente; o `post_save` de Chamado executa S143 antes do receiver que julga a célula; S144 materializa sincronamente. Só retomada de regularizações e beco ficam em `on_commit`. (`app/chamados/signals.py:42`; `app/chamados/signals.py:75`; `app/chamados/signals.py:92`; `app/chamados/signals.py:124`; `app/chamados/signals.py:140`; `app/chamados/signals.py:164`; `app/chamados/signals.py:180`; `app/chamados/signals.py:121`; `app/chamados/signals.py:202`)

**Selo de universo: parcial.** O novo observador registra `'UNIVERSO'` quando a chamada real retorna com a flag; não verifica quais linhas foram efetivamente travadas. O teste da “outra disputa e pai” afirma apenas a lista de disputas devolvida, sem observar trava do pai. Seu MORDE comprova ausência do marcador sem flag, não reprovação de um helper mutante que conserve a flag e omita uma linha. O rastreador antigo ainda elimina aquisições seguintes da mesma tabela. (`app/chamados/tests/test_e1_v5_ordem_trava.py:358`; `app/chamados/tests/test_e1_v5_ordem_trava.py:361`; `app/chamados/tests/test_e1_v5_ordem_trava.py:417`; `app/chamados/tests/test_e1_v5_ordem_trava.py:422`; `app/chamados/tests/test_e1_v5_ordem_trava.py:424`; `app/chamados/tests/test_e1_v5_ordem_trava.py:45`)

Execução de testes, deadlock reproduzido e ocorrência em produção: `[nao verificado]`. [nao verificado]

## 07/11 13:34 · sessao 0364b6caa4b0 · CODEX-CONSTRUTOR-NEGOCIO · verificador · origem=Code

1. **Informativo — o delta fecha as correções centrais, mas não todos os limites de v4.r.**

   | Achado de v4.r | Aval desta volta |
   |---|---|
   | **#1 — fechamentos anteriores** | Permanecem a proibição de `.git` aninhado, ignorados/autoload e a distinção entre permissões do wrapper e regras de conduta. A demonstração do hook FSMON continua limitada, conforme item 6 abaixo. (`bin/codex_pergunta.sh:105`, `bin/codex_pergunta.sh:109`, `bin/codex_pergunta.sh:159`, `CLAUDE.md:341`, `CLAUDE.md:346`, `CLAUDE.md:351`) |
   | **#2 — hardlink idêntico** | **Fecha:** `st_nlink>1` agora força entrada em `tocados`, antes da classificação. A mesma implementação roda no pre-check, revalidação e fim. (`bin/codex_pergunta.sh:149`, `bin/codex_pergunta.sh:164`, `bin/codex_pergunta.sh:223`, `bin/codex_pergunta.sh:371`, `bin/codex_pergunta.sh:398`) |
   | **#3 — cobertura do inventário** | **Fecha em parte:** fecha tipo especial e erro de percurso; permanecem diferenças entre identidade Git e árvore física, detalhadas no item 5. (`bin/codex_pergunta.sh:116`, `bin/codex_pergunta.sh:136`, `bin/codex_pergunta.sh:161`) |
   | **#4 — vaga por worktree** | **Fecha o contraexemplo:** worktrees com o mesmo common-dir passam a compartilhar a vaga. A fórmula acrescenta uma hipótese sobre a localização dos metadados, conforme item 2. (`bin/codex_pergunta.sh:72`, `bin/codex_pergunta.sh:73`, `bin/tests/test_codex_pergunta.sh:332`) |
   | **#5 — prova sem executar tentativas** | **Não fecha esse contraexemplo; fecha parte da rotulagem.** `.out` fabricado continua suficiente para algumas verificações. (`bin/tests/prova_codex_pergunta_real.sh:90`, `bin/tests/prova_codex_pergunta_real.sh:92`, `bin/tests/prova_codex_pergunta_real.sh:144`) |
   | **#6 — ouvinte morto/EV compartilhado** | **Fecha os casos apresentados:** criação exclusiva de EV e controle posterior. Permanece uma janela de contagem, conforme item 3. (`bin/tests/prova_codex_pergunta_real.sh:21`, `bin/tests/prova_codex_pergunta_real.sh:106`, `bin/tests/prova_codex_pergunta_real.sh:111`) |
   | **#7 — mortes espúrias dos mutantes** | **Fecha os dois mecanismos apontados:** `fs_walk` conserva os rastreados; `p_git` retira a regra efetiva. Execução completa: **[nao verificado]**. (`bin/tests/test_codex_pergunta.sh:422`, `bin/tests/test_codex_pergunta.sh:426`) |

   Os itens informativos #8–#10 precisam ser atualizados pelo mapa e pelos limites desta resposta; não representam correções independentes. (`bin/codex_pergunta.sh:72`, `bin/codex_pergunta.sh:398`, `bin/tests/prova_codex_pergunta_real.sh:111`)

2. **Média — regressão condicional: `dirname(common-dir)` não identifica necessariamente a raiz do repositório.**

   A fórmula funciona para `<principal>/.git`, mas common-dirs distintos como `/srv/r1.git` e `/srv/r2.git` produzem a mesma `/srv/logs/construtor.vaga`. Com metadados separados em `/meta/r1`, a vaga passa para `/meta/logs`, independentemente da árvore escolhida. Não há validação exigindo que o common-dir termine em `.git`. (`bin/codex_pergunta.sh:72`, `bin/codex_pergunta.sh:73`, `bin/codex_pergunta.sh:206`, `bin/codex_pergunta.sh:208`)

   Isso pode serializar repositórios independentes ou recusar um construtor porque a nova pasta não permite escrita. Nos papéis de leitura, não se abre a vaga, mas a reserva desse novo caminho também vale: uma resposta antes legítima nesse endereço passa a ser recusada. Ocorrência dessas topologias nesta obra: **[nao verificado]**. (`bin/codex_pergunta.sh:74`, `bin/codex_pergunta.sh:171`, `bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:218`)

   A consulta ao common-dir agora antecede a separação dos papéis: uma falha nela encerra qualquer papel com rc 2. Os quatro testes de leitura usam apenas a fixture de repositório convencional; não cobrem essas topologias. (`bin/codex_pergunta.sh:72`, `bin/codex_pergunta.sh:81`, `bin/tests/test_codex_pergunta.sh:58`, `bin/tests/test_codex_pergunta.sh:352`)

3. **Média — o controle posterior não fecha a janela entre aceitar e registrar uma conexão.**

   O ouvinte executa `accept`, pode esperar até dois segundos em `recv` e somente depois escreve `conexoes`. Se `N_DEPOIS` for capturado nessa janela, a conexão fica fora da diferença julgada; seu registro tardio ainda pode contribuir para o mínimo de duas linhas exigido pelo controle posterior. O teste não confere individualmente os tokens `controle_pos` nem um sucesso por transporte. Reprodução desse escalonamento: **[nao verificado]**. (`bin/tests/prova_codex_pergunta_real.sh:38`, `bin/tests/prova_codex_pergunta_real.sh:41`, `bin/tests/prova_codex_pergunta_real.sh:103`, `bin/tests/prova_codex_pergunta_real.sh:109`, `bin/tests/prova_codex_pergunta_real.sh:111`, `bin/tests/prova_codex_pergunta_real.sh:114`)

   **Baixa — regressão no diagnóstico:** depois de acrescentar os controles posteriores, o `tail` imprime até EOF e atribui também essas linhas a conexões “de dentro do sandbox”. A quantidade usa o snapshot anterior, mas a lista mistura origens. (`bin/tests/prova_codex_pergunta_real.sh:107`, `bin/tests/prova_codex_pergunta_real.sh:108`, `bin/tests/prova_codex_pergunta_real.sh:114`)

4. **Alta — continua possível verde sem demonstrar cada tentativa.** [nao verificado]

   O controle positivo exige apenas `INTRUSO_WT.txt`. Para as tentativas individuais, o juiz aceita arquivos produzidos pelo próprio Codex contendo `rc=` e, em parte deles, texto de negação. TCP local/Unix dispensam essa assinatura. Fabricar os relatos continua satisfazendo essas condições sem executar cada acesso; o controle posterior não resolve essa lacuna. Contraexemplo completo executado: **[nao verificado]**. (`bin/tests/prova_codex_pergunta_real.sh:60`, `bin/tests/prova_codex_pergunta_real.sh:90`, `bin/tests/prova_codex_pergunta_real.sh:92`, `bin/tests/prova_codex_pergunta_real.sh:117`)

   A triagem sustenta o fechamento **documental** de Docker/socket/TCP externo: os rótulos estão explícitos. Não sustenta fechamento da execução comprovada; essas negativas ainda podem terminar em rc 0. (`bin/tests/prova_codex_pergunta_real.sh:127`, `bin/tests/prova_codex_pergunta_real.sh:130`, `bin/tests/prova_codex_pergunta_real.sh:135`, `bin/tests/prova_codex_pergunta_real.sh:144`, `bin/tests/prova_codex_pergunta_real.sh:147`)

5. **Média — os descartes de v4.r #3 têm alcances diferentes.**

   | Descarte | O motivo se sustenta? |
   |---|---|
   | Permissões além do bit executável e permissões de diretórios | **Sim, para identidade transportada pelo Git.** O inventário compara somente o bit executável do proprietário e não compara permissões dos diretórios. Isso não demonstra integridade completa do filesystem. (`bin/codex_pergunta.sh:118`, `bin/codex_pergunta.sh:129`, `bin/codex_pergunta.sh:143`) |
   | Diretório vazio | **Sim, para o conteúdo representado pela base Git.** Continua podendo sair `TOCADO`, rc 0 no fim. (`bin/codex_pergunta.sh:129`, `bin/codex_pergunta.sh:144`, `bin/codex_pergunta.sh:167`) |
   | Bytes não UTF-8 — “falha fechado” | **Só parcialmente.** Git usa `replace`; o percurso preserva outra representação. Uma simulação exclusivamente em memória com rastreado limpo `app/folha/a\xff.py` produziu dois `TOCADO`, rc 0: pre-check/revalidação recusam a árvore limpa; o fim aceita o rc 0. Execução em filesystem real: **[nao verificado]**. (`bin/codex_pergunta.sh:126`, `bin/codex_pergunta.sh:132`, `bin/codex_pergunta.sh:141`, `bin/codex_pergunta.sh:223`, `bin/codex_pergunta.sh:374`, `bin/codex_pergunta.sh:400`) |
   | Case/ext4 | A comparação literal permanece. Filesystem efetivo da produção: **[nao verificado]**. (`bin/codex_pergunta.sh:132`, `bin/codex_pergunta.sh:147`) |
   | CRLF/smudge | A comparação continua usando bytes físicos contra hash bruto. Ausência de filtros na configuração Git ativa: **[nao verificado]**. (`bin/codex_pergunta.sh:114`, `bin/codex_pergunta.sh:115`, `bin/codex_pergunta.sh:147`) |
   | `.git` da raiz fora da área gravável | **Sustenta o argumento de configuração:** a chamada limita a escrita a `<wt>/app`. A guarda final ainda compara conteúdo, sem repetir a exigência inicial de arquivo sem symlink. Efeito real de uma substituição: **[nao verificado]**. (`bin/codex_pergunta.sh:199`, `bin/codex_pergunta.sh:381`, `bin/codex_pergunta.sh:383`, `bin/codex_pergunta.sh:399`) |

   O symlink rastreado e inalterado continua fora de `tocados`, portanto não recebe a proibição; esse ponto não foi fechado nem explicitamente descartado na triagem fornecida. (`bin/codex_pergunta.sh:147`, `bin/codex_pergunta.sh:157`, `bin/codex_pergunta.sh:163`)

   **Não identifiquei nova recusa de admissão causada apenas por `onerror` numa wt limpa convencional:** antes, rastreados omitidos já viravam remoções, e diretórios extras já eram tocados. No fim, erro de permissão/I/O externo também provoca descarte; a mensagem não demonstra culpa do Codex. Uma wt real desta obra nessa condição: **[nao verificado]**. (`bin/codex_pergunta.sh:144`, `bin/codex_pergunta.sh:150`, `bin/codex_pergunta.sh:152`, `bin/codex_pergunta.sh:402`)

6. **Informativo — os novos mutantes têm mordida coerente por leitura; permanecem limites de cobertura.**

   Hardlink idêntico cobre início, espera e fim; neutralizar `inv_nlink` retira precisamente sua descoberta. O mutante de `fs_walk` agora conserva os rastreados e omite os caminhos plantados, evitando a antiga morte no pre-check. Retirar `p_git` remove a proteção efetiva, não apenas sua mensagem. (`bin/tests/test_codex_pergunta.sh:207`, `bin/tests/test_codex_pergunta.sh:315`, `bin/tests/test_codex_pergunta.sh:318`, `bin/tests/test_codex_pergunta.sh:383`, `bin/tests/test_codex_pergunta.sh:422`, `bin/tests/test_codex_pergunta.sh:426`)

   O caso de ilegibilidade depende de `chmod 000` realmente impedir leitura. Sob privilégio que contorne essa permissão, o original pode falhar no teste antes de avaliar o mutante. FIFO é exercitado; socket/dispositivo não são. Execução desses cenários: **[nao verificado]**. (`bin/tests/test_codex_pergunta.sh:323`, `bin/tests/test_codex_pergunta.sh:325`, `bin/tests/test_codex_pergunta.sh:330`, `bin/tests/test_codex_pergunta.sh:386`)

   O descarte da sentinela FSMON sustenta apenas a interpretação estreita: a fixture verifica recusa e ausência de efeito. Ela cria somente `.git/config`, não executa Git ali nem demonstra o hook válido anunciado pelo comentário. Essa causalidade permanece **[nao verificado]**; o Git do inventário usa `-C wt`. (`bin/tests/test_codex_pergunta.sh:289`, `bin/tests/test_codex_pergunta.sh:291`, `bin/tests/test_codex_pergunta.sh:292`, `bin/codex_pergunta.sh:112`)

7. **Informativo — escritores, leitores e significados.**

   | Campo/artefato | Escritor → leitor |
   |---|---|
   | `_CDIR` | Shell captura o common-dir absoluto do Git → `dirname` calcula `VAGA`. **Metadados do repositório são interpretados como filho da raiz desejada.** (`bin/codex_pergunta.sh:72`, `bin/codex_pergunta.sh:73`) |
   | `VAGA` / arquivo da vaga | Shell define → reserva da resposta, criação da pasta, abertura append no fd 6 e `flock`. Testes também criam, prendem e movem suas fixtures. (`bin/codex_pergunta.sh:74`, `bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:219`, `bin/tests/test_codex_pergunta.sh:175`, `bin/tests/test_codex_pergunta.sh:217`) |
   | `_INV0` / `_INV1` | Python produz stdout/stderr → shell exige saída vazia na admissão; revalidação acrescenta `FALHOU` em erro. (`bin/codex_pergunta.sh:223`, `bin/codex_pergunta.sh:371`, `bin/codex_pergunta.sh:374`) |
   | `.inventario` / `.recusa` | Python escreve inventário; shell acrescenta guarda de `.git`, lê rc e gera recusa → testes verificam; prova copia evidências. **No fim, saída não vazia é permitida se rc 0.** (`bin/codex_pergunta.sh:398`, `bin/codex_pergunta.sh:403`, `bin/tests/test_codex_pergunta.sh:106`, `bin/tests/test_codex_pergunta.sh:242`, `bin/tests/prova_codex_pergunta_real.sh:80`) |
   | `$EV` | Shell define/cria → redirecionamentos e cópias. A resposta original continua em caminho fixo `logs/codex/r/provaefeito`. (`bin/tests/prova_codex_pergunta_real.sh:21`, `bin/tests/prova_codex_pergunta_real.sh:77`, `bin/tests/prova_codex_pergunta_real.sh:80`) |
   | `escuta/porta`, `u.sock`, `conexoes` | Ouvinte publica porta, faz bind e acrescenta registros → shell lê porta/contagens; controles e tentativa usam os endpoints. **`conexoes` mistura controles externos e candidatos a intrusão.** (`bin/tests/prova_codex_pergunta_real.sh:34`, `bin/tests/prova_codex_pergunta_real.sh:35`, `bin/tests/prova_codex_pergunta_real.sh:41`, `bin/tests/prova_codex_pergunta_real.sh:46`, `bin/tests/prova_codex_pergunta_real.sh:103`) |
   | `$EV/p`, `prova/*.out`, `$EV/prova` | Shell escreve pergunta → wrapper lê; Codex é instruído a escrever `.out` → prova copia, mas julga os originais na wt. (`bin/tests/prova_codex_pergunta_real.sh:53`, `bin/tests/prova_codex_pergunta_real.sh:61`, `bin/tests/prova_codex_pergunta_real.sh:77`, `bin/tests/prova_codex_pergunta_real.sh:81`, `bin/tests/prova_codex_pergunta_real.sh:89`) |

   Não encontrei troca relativa/absoluta em `_CDIR` no comando declarado. A ambiguidade relevante é **repositório × worktree**: `VIVO` determina tanto identidade do repositório quanto o pai exigido para o nome da wt; mudar `VIVO` para outra pasta pode conservar a vaga e mudar os destinos admissíveis. (`bin/codex_pergunta.sh:72`, `bin/codex_pergunta.sh:191`, `bin/codex_pergunta.sh:194`)

8. **Informativo — limite do aval.** Suite completa, prova real, isolamento efetivo, configuração Git ativa e correspondência criptográfica da cópia com o commit solicitado: **[nao verificado]**. Os scripts de teste criam fixtures e a prova chama o Codex; não foram executados nesta revisão somente leitura. (`bin/tests/test_codex_pergunta.sh:18`, `bin/tests/test_codex_pergunta.sh:55`, `bin/tests/prova_codex_pergunta_real.sh:21`, `bin/tests/prova_codex_pergunta_real.sh:77`)

## 07/11 13:34 · sessao 68cb2a1e2ac4 · - · - · origem=Ronald (tmux)

B/F/G/H: **curas parciais; persistem caminhos sem a garantia anunciada**. Execução de testes e ocorrências em produção: **[nao verificado]**. [nao verificado]

| Achado anterior | Veredito na cópia |
|---|---|
| B-NULL no agrupamento/seleção do CLI | **Curado na origem desses leitores:** `q_do_horizonte` agora é somente `>= H`; agrupamento o usa; `admin_decidiu_sem_ato` separa nulos para contagem. (`app/chamados/services/fio.py:1033`; `app/chamados/services/fio.py:1052`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:97`) |
| B-recorte estrito em toda escrita | **PROCEDENTE, parcial:** `aplicar` não aplica `q_do_horizonte`, recebe PKs e escreve por PK. Chamada direta com pergunta anterior a H, `admin_decidiu`, sem decisão e competência não exportada pode registrar SILENCIO. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:150`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:168`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:172`) |
| B-DIVERGE no CLI | **Curado para o comando:** registra divergências e recusa antes de `aplicar`. **Não cobre o helper direto:** a verificação mora em `handle`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:228`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:315`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:128`) |
| F-recusa antes de qualquer escrita | **PROCEDENTE, parcial:** pré-checagem cura mérito/exportada já presentes, mas está fora das travas; SILENCIO continua em atomics individuais antes da segunda leitura do mérito. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:147`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:164`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:185`) |
| G-ausência do registro 0052 | **Curado:** ausência de registro ou instante agora classifica como nova. (`app/chamados/services/fio.py:1113`) |
| G-nova medida pelo nascimento da disputa | **PROCEDENTE, não toca:** mantém o proxy e admite explicitamente subcontagem da pergunta nova em disputa velha. (`app/chamados/services/fio.py:1103`; `app/chamados/services/fio.py:1105`) |
| G-placar truncado | **Não toca:** continua exibindo somente dez PKs novas e quantidade do acervo. (`bin/placar_code.sh:198`) |
| H-rastreador por tabela | **PROCEDENTE, parcial:** original continua ignorando aquisições subsequentes da mesma tabela. (`app/chamados/tests/test_e1_v5_ordem_trava.py:45`) |
| H-relógios soltos | **PROCEDENTE, não toca integralmente:** fixture cria disputa com `auto_now_add`, pergunta é fechada lendo `now`, e teste de catraca ainda compara abertura real com migration real. (`app/chamados/tests/test_e1_v6_horizonte.py:74`; `app/chamados/models.py:1709`; `app/chamados/models.py:2760`; `app/chamados/tests/test_e1_v6_horizonte.py:210`) |
| H-canônica da tela | **Curado no contraexemplo antigo:** novo caso põe muda e respondida ambas reutilizáveis e exige a respondida. (`app/chamados/tests/test_e1_v6_horizonte.py:381`) |
| H-fronteira BATIDA no banco | **Não toca:** testes concretos de fronteira criam motivo MARCO padrão; BATIDA é examinada por folhas da condição. (`app/chamados/tests/test_e1_v6_horizonte.py:76`; `app/chamados/tests/test_e1_v6_horizonte.py:94`; `app/chamados/tests/test_e1_v6_horizonte.py:58`) |

Contraexemplo F por interleaving: passar na pré-checagem; gravar SILENCIO e encerrar seu atomic; mérito aparecer no grupo; segunda leitura detectar e acrescentar `merito_recusado`; função devolver recusa após a escrita anterior. Não há atomic externo cobrindo o universo nem rollback nesse ramo. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:161`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:164`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:186`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:197`) Reprodução concorrente: **[nao verificado]**.

O CLI então imprime **“nada escrito”**, embora esse ramo possa devolver `linhas_silencio > 0`; o próprio retorno mantém a contagem. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:197`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:329`)

Exportada também só é consultada antes dos locks; nem loop SILENCIO nem loop retratação a relê sob trava. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:158`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:168`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:183`) Mudança concorrente efetiva em produção: **[nao verificado]**.

`dedup_perguntas_fato` recebe grupos estritos, mas relê somente PKs/exclusão das vias, sem repetir horizonte; também continua sem recusa de mérito e competência exportada. A chamada ao escritor fica em `:101`. (`app/chamados/management/commands/dedup_perguntas_fato.py:59`; `app/chamados/management/commands/dedup_perguntas_fato.py:94`; `app/chamados/management/commands/dedup_perguntas_fato.py:101`) É caminho preexistente sem cura neste diff; execução em produção: **[nao verificado]**.

A cobertura nova de F prova recusa com mérito **já existente** e exportada mockada **desde a primeira leitura**; não prova interleaving nem helper direto com data anterior ao horizonte. (`app/chamados/tests/test_e1_v6_horizonte.py:311`; `app/chamados/tests/test_e1_v6_horizonte.py:323`)

Há novo selo fraco em D: `assertLessEqual(ps.count(), 1)` aceita zero perguntas; portanto não prova nascimento e dedup de uma conversa. A subclasse ainda sobrescreve o teste nominal herdado com `pass`. (`app/chamados/tests/test_e1_v6_horizonte.py:421`; `app/chamados/tests/test_e1_v6_horizonte.py:409`) Execução/mutação concreta: **[nao verificado]**.

**NAO PROCEDE divergência de operador inclusivo/exclusivo:** helper, modelo e migration usam `>= H`; `--ate` é exclusão superior deliberada (`d < dia_antes_de`). (`app/chamados/services/fio.py:1033`; `app/chamados/models.py:3177`; `app/chamados/models.py:3183`; `app/chamados/migrations/0053_pergunta_unica_por_familia.py:33`; `app/chamados/services/fio.py:1076`)

A cópia não disponibiliza repositório Git: `git rev-parse HEAD` retornou “not a git repository”; confirmação independente do hash/pai: **[nao verificado]**.

_2748 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
