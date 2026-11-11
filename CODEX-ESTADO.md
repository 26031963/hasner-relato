# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 04:37** · publicado em **11/11/2026 04:39** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 04:37 · sessao e7d57b8fb5e2 · - · - · origem=Ronald (tmux)

Causa S2/S3: o commit `6c1fae937` acrescentou `sem_adesao:1` ao hash de todo o ramo sem adesão (`ponto/services/cartorio.py:522–526`, `:1212–1214`). O B5-u constrói deliberadamente a impressão anterior sem esse marcador (`ponto/tests/test_selo_b5u_nunca_bateu_ve_ausencia.py:129`): isso rompe a idempotência em `:142` e faz o mutante cego relavrar em `:185`.

Cura sugerida: calcular `_ata_nb`, `_cods_nb` e `_vd_nb` antes da impressão; acrescentar o marcador somente quando:

```python
sem_adesao=not _coberto_inteiro_nb and _vd_nb not in cel.VEREDITOS_ACUSAM
```

Assim a seleção usa `derivar_veredito`, já chamado em `ponto/services/cartorio.py:1248`, sem predicado próprio de folga. Trabalho acusador e cobertura integral conservam a impressão anterior; folga absolvida sem cobertura conserva o marcador necessário ao C3 (`ponto/tests/test_x1_folga_sem_adesao.py:116–146`).

Não basta selecionar todo veredito não acusador: ausência aprovada também ficaria marcada, e o B5-u cego continuaria divergindo. Usar o veredito persistido também falha: a primeira lavra mudaria vazio para `concorde`, rompendo C5 (`ponto/tests/test_x1_folga_sem_adesao.py:172–190`).

Atualizar somente a âncora textual de `sem_fonte_no_gancho` no executor de mutantes para a expressão nova; conservar o teste C3 (`ponto/tests/x1_mutantes.py:23–26`).

Nenhum arquivo alterado; execução ORM e resultado da sugestão [não verificado].

## 11/11 04:37 · sessao 6804993530b2 · - · - · origem=Ronald (tmux)

S1 nasceu antes do c8c: o histórico do teste aponta `ddf5980ac`/`dc0ecbdeb`; o c8c `6b8898fdc` ampliou o universo para todas as perguntas elegíveis (`chamados/services/tarefa_do_dia.py:492–506`). O selo exige zero chamadas ao C6 e uma leitura do painel naquele cenário (`chamados/tests/test_e3_portas_tarefa.py:118–126`).

A cura mínima é decidir **por dia** se existe consumidor de `viva_c6`:

- Dia sem trabalho: precisa, porque `expor_admin` é chamado pelos marcos dispensados (`tarefa_do_dia.py:260–268`) e A/S consulta todas as perguntas (`:327–328`).
- Motivo fora do mapa ou posição ausente da grade: precisa (`:314–319`).
- Trabalho com todas as perguntas dentro da grade: dispensa C6; esse é o cenário S1 (`chamados/tests/e3_fixtures.py:41–48`, `:65–72`).

Quando precisar, preservar **todas as perguntas daquele dia**, evitando restaurar o filtro individual que o c8c curou. A posição ausente precisa continuar recebendo vida (`chamados/tests/test_e3_c8c_sem_banco.py:40–64`; `test_e3_c8c_orm.py:35–49`).

Não usar `q_vivas` como C6 exato: seu contrato é continência, explicitamente não igualdade (`chamados/models.py:3336–3345`). O painel só entrega verbo/fila (`chamados/juizes.py:1475–1493`); seu crédito usa esse pré-filtro generoso (`chamados/services/raiox.py:74`).

Para S4–S6, o recorte deve devolver valores dentro de `juizes.py`, consumidos como locais por `_pergunta_viva_calc`, sem copiar nem atribuir campos de PerguntaDisputa. Os pontos são `juizes.py:1346`, `:1357`, `:1362`, `:1383`, `:1389`, `:1421`. Materialização futura deve resultar em ausência de carimbo efetivo antes de alcançar o juiz do lastro (`:1421–1429`). Atenção: `_resposta_concorda_com_a_luz` lê resposta (`:1323`) e a carga pré-calcula slots usando a pergunta (`chamados/services/carga_pergunta_viva.py:119`); resposta futura não pode vazar por esses caminhos.

## 11/11 04:18 · sessao a480d6252eb2 · FIO-E0-V4-E4B · construtor · origem=Code

Entreguei o [diff](</home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v2.diff>) e o [recibo com causas F1–F8 e comandos individuais](</home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v2.txt:6>).

- **F1–F2:** fixture reescrita pelas portas reais, com pré-condições explícitas. [Código:56](</home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:56>)
- **F3:** caracterização declarada da recusa de nascimento sem dia. [Código:206](</home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:206>)
- **F4:** `1 != 0` significava reversão ausente; o teste agora exige foto integral e reversão antes do primeiro ato. [Código:145](</home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:145>) [nao verificado]
- **F5–F8:** comparação estática encontrou consultas substituídas; tetos ajustados com C8 e sonda BASE/HEAD preparada. [Análise:48](</home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v2.txt:48>)

Passaram **52 testes em memória, 40 selos e 5 testes do executor**; dois mutantes do executor caíram por assertion. [Resultados e comandos:104](</home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v2.txt:104>) [nao verificado]

**GREEN ORM, números da base e cinco mortes ORM: [nao verificado]. Ruff limpo: [nao verificado] — ferramenta ausente.** [Pendências e comandos pela trava:130](</home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v2.txt:130>)

## 11/11 04:06 · sessao 044058884828 · - · - · origem=Ronald (tmux)

F1–F4 compartilham uma fixture que interfere no próprio dia-alvo: Ch12 nasce ligado a `self.cel` de setembro, e o `post_save` julga esse dia imediatamente, antes de montar P90/D90 de julho. A chamada extra de ressuscitação só ocorre dentro de `_executar_dia`; o teste não demonstra que esse hook foi alcançado. [chamados/tests/test_e4b_c4_orm.py:58](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:58), [chamados/signals.py:187](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/signals.py:187), [chamados/tests/test_e4b_c4_orm.py:90](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:90). [nao verificado]

O `1 != 0` de F4 corresponde a **uma reversão esperada, nenhuma capturada**, pela asserção `len(fotos)`, não à sobra de uma linha. A reversão só é gravada havendo itens executáveis. [chamados/tests/test_e4b_c4_orm.py:148](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:148), [backfill_fio_morto_e4b.py:258](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/management/commands/backfill_fio_morto_e4b.py:258).

Proposta concreta para a fixture:

- Montar primeiro o dia de julho, com **batida S real sem E**, usando `registrar_batida`, e pedir julgamento/fábrica; selecionar P90, D90 e seu Ch12 efetivamente produzidos. Isso evita transformar setembro durante a montagem. A fábrica se abstém para `nunca_bateu`, portanto a batida real fornece a adesão necessária. [fabrica_da_celula.py:183](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/fabrica_da_celula.py:183).
- Fechar D90 pela porta `d90.fechar(user, motivo=FECHOU_ADMIN_ENCERRA, resolucao=...)`, que carimba a muda e grava o fechamento. Não fabricar carimbo/fechamento por `.update`. [chamados/models.py:1918](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/models.py:1918).
- Antes do ensaio, exigir que `ch12.perguntas_a_ressuscitar()` contenha P90. A porta exclui apenas as vias fora do fato; testa C6 na cópia descarimbada e reabre a disputa após ressuscitar. [chamados/models.py:1483](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/models.py:1483), [chamados/models.py:1506](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/models.py:1506), [chamados/models.py:1573](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/models.py:1573).
- Exigir separadamente `call_count` do hook e quantidade ressuscitada; assim falha de alcance não se confunde com falha da porta. O teste atual só verifica `any(nascimentos)`. [chamados/tests/test_e4b_c4_orm.py:99](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:99). [nao verificado]

Há duas caracterizações necessárias: o elo datado setembro→julho é recusado pela porta de nascimento via `resolvedora_valida`, e `PerguntaDisputa.abrir(data_evento=None)` levanta. Portanto a indeterminada atual, criada por `.update(data_evento=None)`, não representa saída dessas portas. [chamados/models.py:2879](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/models.py:2879), [chamados/models.py:2787](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/models.py:2787), [chamados/tests/test_e4b_c4_orm.py:83](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:83).

Nenhum arquivo alterado; a proposta e os cinco mutantes ORM permanecem **[nao verificado]**.

## 11/11 04:04 · sessao 945ef0509c6f · - · - · origem=Ronald (tmux)

F5–F8: a c4 **substituiu consultas**, sem acrescentar uma consulta nos dois sítios apontados:

- HEAD `disputa_emissao.py:1799`: `_ancoradas()` substitui a consulta por destino não validado da base `b20a37f3e:app/chamados/services/disputa_emissao.py:1789`.
- HEAD `disputa_emissao.py:1915`: `_ancoradas()` substitui a consulta a todas as perguntas de disputas abertas do colaborador da base `b20a37f3e:app/chamados/services/disputa_emissao.py:1907`.
- Os quatro tetos são idênticos na base e em HEAD: `ponto/tests/test_selo_chokepoint_escrita.py:116`, `:118`, `:161`, `:162`.

O caminho é fechamento → S143 (`chamados/signals.py:96`, `:114`) → resolver (`chamados/services/disputa_emissao.py:1878`) → consulta canônica (`:1915`). `_ancoradas()` executa uma consulta por chamada, inclusive vazia: `chamados/models.py:336–347`. Os números efetivos na base permanecem **[nao verificado]**, pois não executei ORM/banco.

Não recomendo simplesmente usar `_ancoradas()` também como `perguntas`: a primeira exclui validadas (`chamados/models.py:344`); S128 inclui perguntas validadas com `materializada_em=NULL` em disputa aberta (`chamados/services/disputa_emissao.py:1904–1908`). Fundir ingenuamente enfraquece o universo.

Além disso, o RED atual exige uma chamada à origem `_ancoradas()` (`chamados/tests/test_e4b_classe_e_memoria.py:384–388`). A alternativa conservadora é manter a consulta canônica necessária e justificar C8 ao lado dos tetos, deixando os números da base explicitamente pendentes da execução pela trava.

## 11/11 04:00 · sessao 9323309d2863 · FIO-E0-V4-E3 · construtor · origem=Code

Aplicadas as duas correções de teste: [X1:129](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_x1_folga_sem_adesao.py:129) e [c8c:40](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/test_e3_c8c_orm.py:40). [nao verificado]

```diff
--- a/app/ponto/tests/test_x1_folga_sem_adesao.py
+++ b/app/ponto/tests/test_x1_folga_sem_adesao.py
@@ -128,2 +128,3 @@
             out = self._julgar()  # proximo julgamento, SEM forcar
+        self.assertIn('cods', out, 'C3: a folga foi pulada em vez de rejulgada apos a adesao')
         self.assertIn(self.d, out['cods'], 'C3: perdeu rejulgamento ao perder a palavra NB')

--- a/app/chamados/tests/test_e3_c8c_orm.py
+++ b/app/chamados/tests/test_e3_c8c_orm.py
@@ -39,3 +39,8 @@
         agora = _ts(D, 21)
+        carga = carga_da_tarefa([self.c], D, D, agora)
+        carga._ler_vida()
+        self.assertIn(p.pk, [q.pk for q in carga._perg.get((self.c.pk, D), ())
+                             if hasattr(q, 'viva_c6')],
+                      'a pergunta fora da grade tambem precisa receber a vida do C6')
         avulso = tarefa_do_dia(self.c, D, agora)
-        lote = carga_da_tarefa([self.c], D, D, agora).tarefa(self.c, D)
+        lote = carga.tarefa(self.c, D)
```

`_julgar()` devolve o `out` de `julgar_celula`, após atualizar a célula (`ponto/tests/test_x1_folga_sem_adesao.py:59`). No rejulgamento normal, esse dicionário recebe `cods[data]`; quando a célula é pulada, incrementa `pulados` sem produzir essa chave (`ponto/services/cartorio.py:1436`, `:1372`, `:1914`).

Escolhi **(a)** no c8c. Sim: o mutante exclui a pergunta de `candidatas`, mas ela permanece em `_perg`; sem passar pelo escritor de `viva_c6`, chega ao leitor de `:245`. O leitor de `:328` depende do ramo `fato_em_ausencia`. A nova assertion verifica a alimentação antes desses consumidores, preservando as assertions finais (`chamados/tests/e3_c8c_mutantes.py:17`; `chamados/services/tarefa_do_dia.py:487`, `:506`, `:536`, `:245`, `:328`; `chamados/tests/test_e3_c8c_orm.py:42`).

| Mutante abrangido | Teste que deve cair por FAIL | [nao verificado]
|---|---|
| `sem_fonte_no_gancho` | `FolgaSemAdesaoTest.test_c3_primeira_batida_viva_distante_rejulga_folga` — assertion em `ponto/tests/test_x1_folga_sem_adesao.py:129` |
| `universo_incompleto` | `C8cCargaReal.test_RED_posicao_sumiu_avulso_e_lote_reais` — assertion em `chamados/tests/test_e3_c8c_orm.py:42` |

Os harnesses permanecem sem prefixo `test_`; X1 continua exigindo GREEN com 11 testes (`ponto/tests/x1_mutantes.py:79`; `chamados/tests/e3_c8c_mutantes.py:66`).

Comandos individuais, **somente no ambiente isolado da sessão**:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.x1_mutantes --modo mutante --mutante sem_fonte_no_gancho
PYTHONDONTWRITEBYTECODE=1 E3_C8C_MUTANTE_ORM=universo_incompleto python3 manage.py test chamados.tests.test_e3_c8c_orm.C8cCargaReal --noinput -v 2
```

Execuções pela trava, conforme os caminhos fornecidos; conteúdo dos scripts [nao verificado]:

```bash
PYTHONDONTWRITEBYTECODE=1 bash logs/FIO-E0-V4-E3/x1_rodar_mutantes.sh
PYTHONDONTWRITEBYTECODE=1 bash logs/FIO-E0-V4-E3/c8c_rodar_mutantes_orm.sh
```

Ruff limpo **[nao verificado]**: o comando abaixo retornou `No module named ruff`. A execução dos testes ORM alterados fica pendente da sessão **[nao verificado]**. [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m ruff check --no-cache ponto/tests/test_x1_folga_sem_adesao.py chamados/tests/test_e3_c8c_orm.py
```

## 11/11 03:57 · sessao 4d4630f2ce92 · FIO-E0-V4-E4B · construtor · origem=Code

C4 escrita. Execuções locais: **52 testes em memória, 40 selos e 43 ensaios individuais de mutação, rc=0**. ORM, tetos e produção continuam **[nao verificado]**; certificação fica com Code e opus. [Recibo e resultados](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4.txt:13). [nao verificado]

| Item | Arquivo e entrega | RED / mutante / rc |
|---|---|---|
| 1 | [Harness:135](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_classe_e_memoria.py:135): atributo do pacote pai, módulos reais com sentinela, restauração e coletor | RED=1; GREEN=0; mutantes exigem assertion, rc=0 |
| 2 | [disputa_emissao.py:1799](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/disputa_emissao.py:1799): `_ancoradas()` por destino unida à ressuscitação | RED na base=1; `D3_volta_segunda_copia`=0 |
| 3 | [Backfill:183](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/management/commands/backfill_fio_morto_e4b.py:183): protocolo, foto integral sob C e cerca antes do commit | REDs e cinco mutantes ORM preparados; rc **[nao verificado]** | [nao verificado]
| 4 | [foto_e4b.py:18](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/foto_e4b.py:18): fora do alvo recusa mesmo com K6=False; BANG identifica competência/linha | Mutantes de alvo, exportada e indeterminada: rc=0; ORM **[nao verificado]** |
| 5 | [disputa_emissao.py:1915](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/disputa_emissao.py:1915): evento lê `_ancoradas()` | RED de seleção=1; mutante=0; **número de queries [nao verificado]** |
| 6 | [Terminal:211](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_o261e_f1_porta_encerramento.py:211): fechado gravado, reabertura com nota e contraexemplo administrativo | REDs/mutante ORM: **[nao verificado]** |
| 7 | [foto_e4b.py:60](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/foto_e4b.py:60): DIFF por colaborador, nascimentos, push e dinheiro da autoridade | REDs=1; mutantes DIFF/push/dinheiro=0; ORM **[nao verificado]** | [nao verificado]
| 8 | [Testemunhas:688](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_classe_e_memoria.py:688): mesma recusa, motivo/instante novos e limbo `31,39` | Três mutantes mortos por assertion; rc=0 |
| 9 | [Passivo:61](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4_passivo.py:61): caracterização invertida; universo e soma preservados | REDs/mutante ORM: **[nao verificado]** |
| 10 | [Registros dos selos](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4.txt:96): relógio, B2 e tratamento explícito do rollback | REDs=1; selos=0; selos mutados=1 |
| 11 | [BACKLOG.md:1875](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/BACKLOG.md:1875): `Parallel Change / Expand-Contract`, fase BACKFILL | RED=1; positivo=0; mutante=1 |

Comandos executados:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4b_classe_e_memoria chamados.tests.test_e4b_foto_memoria
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4b_c4_verificar --selos
```

Os **43 comandos individuais**, com respectivos rc, estão no [recibo:198](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4.txt:198). Os quatro comandos de selos mutados, cada qual com rc=1 por assertion, estão no [recibo:105](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4.txt:105).

TestCase ORM entregues para o Code, com execução **[nao verificado]**:

- [CercaC4ORMTest](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:19): Ch12/P90/D90 fora do alvo, exportada, indeterminada, foto sob C e DIFF monetário.
- [F1HumanoETripwireTest](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_o261e_f1_porta_encerramento.py:192): terminal, admin sem fato novo e mutante.
- [PassivoE4bTest](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4_passivo.py:24).
- [BackfillE4bTest](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4_backfill.py:109).
- [RespostaVisivelTest](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_b2d_resposta_visivel.py:26).
- [SeloChokepointEscritaTest](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/ponto/tests/test_selo_chokepoint_escrita.py:218).

Nomes completos dos métodos e comandos ORM, incluindo mutantes e os quatro tetos: [recibo:111](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4.txt:111).

**QUEM ESCREVE / QUEM LÊ:** [censo por campo — 249 campos declarados mais PKs](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-campos.txt:6), complementado pelo [censo de efeitos e campos temporários](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4.txt:143). Escritores/leitores não identificados estão marcados **[nao verificado]**.

## 11/11 03:12 · sessao 2771d9db9c73 · - · - · origem=Ronald (tmux)

Exploração somente leitura; nenhuma execução ORM.

- Dinheiro online: [`ponto/services/fechamento.py:1735`](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/ponto/services/fechamento.py:1735), `fechamento_lido(colaborador, mes, ano)`, chama o escritor existente em `somente_leitura=True`, sem salvar (`:1759`). `None` significa ausência de resposta e deve permanecer explícito (`:1747`).
- Leitores reais: [`ponto/views.py:993`](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/ponto/views.py:993), `:1480`; [`relatorios/cartao_pela_celula.py:69`](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/relatorios/cartao_pela_celula.py:69).
- DSR e faltas: fechamento recebe DSR em [`ponto/services/fechamento.py:1101`](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/ponto/services/fechamento.py:1101), grava em `:1173–1179`. Autoridade das faltas por dia: [`folha/export.py:125`](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/folha/export.py:125), já lida pelo escritor em `fechamento.py:873`.
- DiaPago: campos monetários declarados em [`ponto/services/dia_pago.py:34`](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/ponto/services/dia_pago.py:34); escritor `lavrar`, grava dias e ajuste em `:345–360`. Fotografar linhas completas por colaborador, incluindo ajuste/versões; não somar novamente no command. O ajuste contém DSR (`:350–358`).
- Eventos de fechamento ficam em `on_commit`: [`ponto/services/fechamento.py:1798`](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/ponto/services/fechamento.py:1798), `:1824`. Logo só comparar raw DiaPago/fechamento gravado dentro do DRY pode ocultar o dinheiro que mudaria após commit; incluir `fechamento_lido` antes/depois.
- Push silenciado: [`chamados/services/disputa_emissao.py:1742`](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/disputa_emissao.py:1742), emissor retorna em `:1753–1756`. Um coletor ContextVar no próprio ramo silenciado consegue contar tentativas por colaborador/disputa, inclusive agregações, sem executar push.
- Relógio: entrada a retirar é [`core/tests/testes_sem_relogio.txt:32`](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/core/tests/testes_sem_relogio.txt:32), `test_b2d_resposta_visivel.py`. Só um método tem freeze hoje (`test_b2d_resposta_visivel.py:79`); `setUp` e demais métodos continuam sem congelamento (`:40`, `:67`, `:114`).
- B2: registrar `chamados/management/commands/backfill_fio_morto_e4b.py::Command.handle` na lista informativa [`core/tests/test_medida_turno_do_par.py:353`](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/core/tests/test_medida_turno_do_par.py:353), em ordem. Não alterar números/critérios em `:100–101`; lista é informativa, conforme `:12–15`. Há receita sem Django/banco em `:20–69`.
- Padrão: corrigir a linha da obra em [`docs/BACKLOG.md:1875`](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/BACKLOG.md:1875) para `padrao=Parallel Change / Expand-Contract, fase BACKFILL`, conforme [`docs/GLOSSARIO.md:18`](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/GLOSSARIO.md:18).

## 11/11 03:12 · sessao 92fe8513932e · - · - · origem=Ronald (tmux)

Leitura concluída, sem alterações nem execução ORM.

- **Trava D1:** `PerguntaDisputa.trava_na_ordem` aceita a coleção de colaboradores primeiro; depois, por colaborador, `abertas_do_colaborador=True` cobre disputas abertas e chamados vivos. A disputa fechada ressuscitada fica coberta pela trava C já adquirida. (`chamados/models.py:2629`, `:2652`, `:2664`, `:1578`.)
- **Fixture Ch12/P90/D90:** reutilizar `BaseE4b`; Ch12 vivo associado ao dia tratado, P90 ancorada nele mas com `data_evento`/célula de outro dia, D90 fechada. A cascata real chama `ressuscitar_perguntas` sem recorte de data; E2 também seleciona pelo colaborador inteiro. (`chamados/tests/test_e4_backfill.py:39`; `ponto/services/cartorio.py:1633`, `:970`; `chamados/reconciliador.py:627`.)
- **Terminal→terminal:** teste está em `F1HumanoETripwireTest.test_i_terminal_para_terminal_passa`. Inverter o resultado final para `em_analise`; exigir a sequência de auditoria `resolvido→fechado→em_analise`, nota E4B única e pergunta sem carimbo. A auditoria anterior ao save permite comprovar que `fechado` foi gravado antes da reentrada. (`chamados/tests/test_o261e_f1_porta_encerramento.py:211`; `chamados/models.py:292`.)
- **Contraexemplo admin:** utilizar `VIA_ADMIN`, dia explícito e `resolvido_em` posterior ao último fato; exigir `recusa:admin_sem_fato_novo`, credor terminal e nota de recusa. Fixtures existentes demonstram essa montagem. (`chamados/tests/test_reabrir_pergunta_a_premissa.py:45`, `:73`; `chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:117`; `ponto/services/cartorio.py:329`.)
- **Tetos existentes:** reutilizar `SeloChokepointEscritaTest`, principalmente `test_gate_da_premissa_fecha_com_dia_e_celula`, `test_registrar_batida_com_1_furo_vivo_que_ela_fecha` e `test_registrar_batida_com_3_furos_vivos_que_ela_fecha`; não alterar constantes. (`ponto/tests/test_selo_chokepoint_escrita.py:218`, `:345`, `:388`, `:393`.)

Resultados atuais e redução de queries: **[nao verificado]**.

## 11/11 03:09 · sessao 11b3832e7d26 · VERIFICADOR-EM-FABLE · censo · origem=Code

As referências aos anexos numeram o conteúdo colado, incluindo linhas vazias.

**O #3 exige classificação mista:** a falsa negativa estava perguntada e não foi identificada; chave desligada e filtro de tipo foram vistos e aceitos pela triagem. (`preverif1.q:4`; `preverif1.r:9`; `preverif1.r:11`; `preverif1.r:19`; `triagem.txt:3`)

**No #4, a atribuição à triagem é parcial:** pv1 apontou acúmulo sem janela e foi considerado PROCEDENTE. Depois, pv2 registrou a ressalva temporal, classificada como descritiva. A consequência específica “primeira ocorrência nova deixa vermelho para sempre” não aparece literalmente nas respostas; o código atual acrescenta observações e conta seus IDs sem filtro temporal. (`preverif1.r:3`; `triagem.txt:2`; `preverif2.r:5`; `triagem.txt:6`; `bin/disjuntor.py:2182`; `bin/disjuntor.py:2201`)

**O #5 não é (c):** a ausência de `linha()` e a primeira falha por `KeyError` foram identificadas por leitura/reprodução em memória, independentemente da execução integral da suíte. (`preverif3.r:33`; `preverif3.r:35`) [nao verificado]

**Há sobreposição entre (e) e (d) no pedido para o #6:** nenhum trecho pergunta ou responde sobre a adequação do padrão escolhido. Pela regra da cópia, seria **(e)**: MODO-COMUM exige que Codex **e opus** tenham deixado passar, enquanto o opus registrou esse achado. Pela definição ampliada de (d) no pedido, também caberia (d); não há desempate fornecido. (`preverif1.q:8`; `preverif2.q:9`; `preverif3.q:9`; `opus_final_v3.txt:7`; `CLAUDE.md:390`)

| Achado | Classe | Trecho q | Trecho r |
|---|---|---|---|
| **1 — prefixo confunde negativa sem obra com B3 de obra parada** | **(a), falha do Codex.** O tema dos efeitos da negativa estava presente. O código usa o prefixo ambíguo, extrai `B3` como regra e reconhece tokens de obra na mensagem. (`bin/disjuntor.py:1646`; `bin/disjuntor.py:262`; `bin/disjuntor.py:638`) | “A negativa nova pula algum efeito que antes acontecia nessa chamada (trilha, dispara, oks do CENSO, b4)? Isso importa?” (`preverif1.q:5`) | **nenhum.** A resposta apenas afirmou: “O registro externo `NEGOU`, resposta `deny` e tentativa de registro no RELATO continuam acontecendo.” Isso não identifica a colisão semântica. (`preverif1.r:23`) |
| **2 — mesmo nome para lista persistida e número retornado** | **(b), falha da triagem.** Motivo registrado: “FILA (nome), consumidores usam o numero.” (`triagem.txt:9`) | “Cada campo persistido (`b3_sem_obra`, `b3_sem_obra_obs`) tem agora UM significado, em todo escritor e leitor?” (`preverif3.q:4`) | “Atenção ao nome na saída: `ler()['b3_sem_obra']` continua sendo o **número derivado de observações**, não a lista histórica persistida.” (`preverif3.r:5`) |
| **3 — reconhecimento de final; chave; tipo do agente** | **(a)** para falsa negativa por mencionar leitura final; **(b)** para chave desligada/outra grafia e cobertura restrita por tipo. Motivo da triagem: “saida declarada do instrumento, fila.” (`triagem.txt:3`) | “E existe pedido NAO-final ou final COM obra legivel que agora passa a ser negado indevidamente?”; “chave desligada”; “subagent_type com outra grafia”. (`preverif1.q:4`) | **Falsa negativa: nenhum.** A resposta disse “não aparece falsa negativa nova dentro do contrato dos parsers.” (`preverif1.r:19`) **Demais partes:** “Com a chave desligada, `main_pretool` pula a decisão”; “Outra grafia de `subagent_type` e ausência de `tool_input` também não entram na negativa nova”; “com metadata `agentType == 'verificador'`”. (`preverif1.r:11`; `preverif1.r:9`) |
| **4 — observações acumulam sem janela nem baixa** | **(b), parcial**, pela ressalva temporal vista e tratada como descritiva; o BLOQUEIA original de pv1 foi aceito, não descartado. (`triagem.txt:2`; `triagem.txt:6`) | “O contador ‘esperado 0’ passa a contar so entradas novas, e o historico aparece separado sem sumir?” (`preverif2.q:4`) | “O escritor acrescenta IDs à lista sem remover os anteriores; o leitor retorna seu comprimento, sem janela temporal ou marco da alteração.” (`preverif1.r:3`) Depois: “Não existe comparação temporal com um marco da cura nesse caminho.” (`preverif2.r:5`) A consequência literal sobre a primeira ocorrência nova: **nenhum**. |
| **5 — caso não chama `linha()`; mutante cai primeiro por exceção** | **(b), falha da triagem.** Motivo registrado: “DESCARTADO para esta obra, linha de fila; conferido em memoria pelo Codex.” (`triagem.txt:7`) | “O caso b3_sem_obra_historico e os 3 mutantes (historico-reconta, forma-invalida-vira-zero, duplicata-reconta) MORDEM de verdade?” (`preverif3.q:8`) | “`forma-invalida-vira-zero`: cai primeiro por `KeyError` em `{}`”; “apesar do comentário, esse caso não chama `linha()`”. (`preverif3.r:33`; `preverif3.r:35`) |
| **6 — padrão Architecture Conformance Test inadequado à guarda de runtime** | **(e) pela regra da cópia; também (d) pela definição sobreposta do pedido**, conforme ressalva acima. (`CLAUDE.md:383`; `CLAUDE.md:390`; `opus_final_v3.txt:7`) | **nenhum** sobre adequação do padrão à guarda de runtime. As listas de classes perguntadas não incluem esse tema. (`preverif1.q:8`; `preverif2.q:9`; `preverif3.q:9`) | **nenhum** sobre esse tema, inclusive lateralmente. (`preverif1.r:29`; `preverif2.r:33`; `preverif2.r:37`; `preverif3.r:37`) |

## 11/11 03:09 · sessao 291b543431b5 · VERIFICADOR-EM-FABLE · verificador · origem=Code

APROVA

1. **A negativa passa a ter regra distinta de `B3`.** `relato_negacao` captura hífens e escreve `NEGOU B3-SEM-OBRA`; a guarda retorna essa negativa antes de consultar a parada da obra (`bin/disjuntor.py:262`, `bin/disjuntor.py:279`, `bin/disjuntor.py:1845`).

   Os demais leitores não precisam interpretar essa regra: `ler()` separa eventos pelo terceiro token, contando `NEGOU` como negativa e somente `DISJUNTOR` como disparo. O Stop recebe o mapa de obras paradas; `gerar_estado.py` delega a leitura e apresentação ao disjuntor. Não encontrei falsa negativa ou contagem indevida de parada introduzida pela troca nesses caminhos (`bin/disjuntor.py:1985`, `bin/disjuntor.py:1995`, `bin/disjuntor.py:2003`, `bin/hook_stop_fila1.py:1204`, `bin/gerar_estado.py:280`).

2. **Existe sobreposição lexical com IDs legítimos, mas ambas as negativas são inertes no consumidor encontrado.** `B3-SEM-OBRA` e `B3-NAO-SEI` são valores diferentes, ambos aceitos por `_OBRA_ID`/`_OBRA_HIF`; portanto `obras_da_linha` pode devolver esses nomes como obras (`bin/disjuntor.py:157`, `bin/disjuntor.py:163`, `bin/disjuntor.py:619`, `bin/disjuntor.py:637`).

   Isso não transforma nenhuma delas em obra parada ou liberação: o consumidor decisório exige também que a linha comece por `DESENHO ` ou `ERRO-BOBO `. Nem a mensagem `DISJUNTOR…` nem a linha datada `NEGOU…` satisfazem essa condição, inclusive quando a obra consultada tem exatamente um desses IDs (`bin/disjuntor.py:279`, `bin/disjuntor.py:735`, `bin/disjuntor.py:737`, `bin/disjuntor.py:748`).

3. **Os retornos 5 e 6 verificam o RELATO produzido pela porta real.** `_arvore` inicializa o RELATO de mentira; `_pre` executa `disjuntor.py pretool` com essa raiz; `main_pretool` chama `relato_negacao`, que encaminha a linha a `no_relato`. Os dois `grep` verificam a presença da regra nova e a ausência da regra `B3` (`bin/tests/test_disjuntor.sh:79`, `bin/tests/test_disjuntor.sh:85`, `bin/disjuntor.py:1907`, `bin/disjuntor.py:294`, `bin/tests/test_disjuntor.sh:1474`, `bin/tests/test_disjuntor.sh:1475`).

   **Ressalva precisa sobre a mordida:** o mutante `prefixo-do-disparo` já cai no retorno **1**, pela mudança do stdout; ele não chega aos retornos 5 e 6. Portanto mata a regressão, mas sozinho não demonstra a mordida dessas duas verificações (`bin/tests/test_disjuntor.sh:1471`, `bin/tests/test_disjuntor.sh:1548`). Execução integral da suíte shell: [nao verificado].

4. **Não encontrei reaparecimento das classes citadas no delta.** A negativa conserva a porta única de escrita; a regra ganha prefixo próprio, enquanto o disparo conserva `DISJUNTOR B3 <obra>`; o selo consulta o RELATO resultante. Não há novo escritor nem novo reconhecedor paralelo nesse trajeto (`bin/disjuntor.py:294`, `bin/disjuntor.py:1149`, `bin/disjuntor.py:1647`, `bin/tests/test_disjuntor.sh:1474`).

5. **Nenhum FILA fornecido demonstra bloqueio adicional da cura literal do item (1).**

   - O número publicado deriva explicitamente de `b3_sem_obra_obs`; o histórico é contado separadamente. Não são dois significados no mesmo campo persistido (`bin/disjuntor.py:2045`, `bin/disjuntor.py:2201`, `bin/disjuntor.py:2207`).
   - O contador cumulativo preserva ocorrências posteriores; não aplicar janela é compatível com acompanhar a meta “zero a partir daqui” do aval fornecido [nao verificado em arquivo] (`bin/disjuntor.py:2183`, `bin/disjuntor.py:2202`).
   - A identificação ampla de “final”, o filtro pelo tipo `verificador` e a chave desligada continuam limitando a cobertura. Não cabe afirmar garantia universal para qualquer agente ou configuração (`bin/disjuntor.py:177`, `bin/disjuntor.py:840`, `bin/disjuntor.py:1842`, `bin/disjuntor.py:1897`).
   - A falta de chamada a `linha()` no selo histórico e a morte incidental do mutante continuam fraquezas de cobertura; não demonstram apresentação incorreta no código atual (`bin/tests/test_disjuntor.sh:1485`, `bin/tests/test_disjuntor.sh:1542`, `bin/disjuntor.py:2202`, `bin/disjuntor.py:2275`).

O zero efetivo em produção e a identificação individual das seis obras históricas permanecem [nao verificado].

## 11/11 03:08 · sessao 12066ae415d0 · FIO-E0-V4-E3 · construtor · origem=Code

Ajustado apenas C6(i), mantendo as três guardas anteriores. O teste compara todas as colunas dos chamados e exige o carimbo da pergunta; se o pai mudar, falha com seu número. [teste:252–264](ponto/tests/test_x1_folga_sem_adesao.py:252)

Carimbo existente: `via_resolucao='celula_absolveu'`, pela chamada em [reconciliador.py:595](chamados/reconciliador.py:595); campos persistidos pela porta em [models.py:3128](chamados/models.py:3128).

```diff
-        # C6(i): conferir tambem o escritor que o consumidor chama apos a lavra.
-        # So comparar o retorno de julgar deixaria esse caminho fora da prova.
+        # DECIDIDO (volta 3, secao 4a): celula soberana; consumidor le, nao re-julga.
+        # Estoque 0 "desses dias": carimbo preexistente fica fora desta cura.
         from chamados.reconciliador import reconciliar_chamados_colab
         reconciliar_chamados_colab(self.colab)
-        self.assertEqual(self._estoque(), antes,
-                         'C6(i): reconciliador alterou pergunta/chamado ja gravado')
+        self.assertEqual(self._estoque()[0], antes[0],
+                         f'C6(i): PARE chamado-pai #{pai.pk} alterado pelo reconciliador')
+        pergunta = PerguntaDisputa.objects.get(pk=antes[1][0]['id'])
+        self.assertEqual(pergunta.via_resolucao, PerguntaDisputa.VIA_CELULA_ABSOLVEU)
+        self.assertIsNotNone(pergunta.materializada_em)
+        self.assertIsNotNone(pergunta.materializada_por_id)
```

Continuam definidos **11 testes**; GREEN mantém essa exigência em [x1_mutantes.py:79](ponto/tests/x1_mutantes.py:79). ORM, RED e morte dos mutantes **[não verificado]**: banco proibido nesta volta. Comandos para a sessão autorizada:

```bash
export PYTHONDONTWRITEBYTECODE=1
export DJANGO_SETTINGS_MODULE=config.settings.ci

# GREEN: arquivo inteiro, 11 testes; esperado rc 0 [nao verificado]
python3 -m ponto.tests.x1_mutantes --modo green

# RED da base; esperado rc 1 por assertion R1
python3 -m ponto.tests.x1_mutantes --modo base

# Mutantes; esperado rc 1 por assertion
python3 -m ponto.tests.x1_mutantes --modo mutante --mutante literal_incondicional
python3 -m ponto.tests.x1_mutantes --modo mutante --mutante sem_fonte_no_gancho
```

Expectativas do executor: [x1_mutantes.py:78](ponto/tests/x1_mutantes.py:78).

Ruff nos dois arquivos: **`All checks passed!`, rc 0**, com o comando abaixo; certificação Code/opus **[não verificado]**.

```bash
PYTHONDONTWRITEBYTECODE=1 /tmp/claude-1001/-home-ronald-saas-hasner-app/707e509b-29e4-4438-9018-16733980c39a/scratchpad/ruffpkg/bin/ruff check --no-cache ponto/tests/test_x1_folga_sem_adesao.py ponto/tests/x1_mutantes.py
```

## 11/11 03:07 · sessao 3e4f7f5604cf · - · - · origem=Ronald (tmux)

1. `relato_negacao` extrai a regra inteira por `[A-Z0-9-]+`; portanto grava `NEGOU B3-SEM-OBRA`, distinto de `NEGOU B3` e `NEGOU B3-NAO-SEI` (`bin/disjuntor.py:262`, `bin/disjuntor.py:279`, `bin/disjuntor.py:1094`, `bin/disjuntor.py:1647`).

2. **Há colisão lexical com a gramática de ID**, porque ambos os nomes satisfazem `_OBRA_ID` e `_OBRA_HIF`, sem reserva desses nomes; eles continuam distintos entre si (`bin/disjuntor.py:157`, `bin/disjuntor.py:163`, `bin/disjuntor.py:619`). `obras_da_linha` aceita ambos como tokens de obra (`bin/disjuntor.py:637`, `bin/disjuntor.py:640`).

3. **Ambos são inertes no consumidor decisório existente:** `linhas_libera_b3` consulta o conjunto, mas só acrescenta uma liberação se a linha começar por `DESENHO ` ou `ERRO-BOBO `; nenhuma negativa `DISJUNTOR ...` nem linha `**... disjuntor (NEGOU ...)` satisfaz isso (`bin/disjuntor.py:735`, `bin/disjuntor.py:737`, `bin/disjuntor.py:747`, `bin/disjuntor.py:748`, `bin/disjuntor.py:751`). A referência decisória a `obras_da_linha` encontrada é essa chamada; os demais usos localizados são documentação e testes (`bin/disjuntor.py:735`, `bin/tests/test_disjuntor.sh:746`, `bin/tests/test_disjuntor.sh:1668`, `bin/tests/test_disjuntor.sh:1669`).

4. Reproduzi somente em memória, compilando as constantes e funções do AST da cópia: as negativas dão respectivamente `{'B3-SEM-OBRA'}` e `{'B3-NAO-SEI', 'OBRA-X'}`; `linhas_libera_b3` devolve `[]` para ambas, inclusive quando a obra consultada é literalmente um desses dois nomes. O resultado decorre dos reconhecedores e das exigências de prefixo acima (`bin/disjuntor.py:157`, `bin/disjuntor.py:174`, `bin/disjuntor.py:619`, `bin/disjuntor.py:631`, `bin/disjuntor.py:735`, `bin/disjuntor.py:737`, `bin/disjuntor.py:748`).

5. O delta mantém a porta única de escrita: a negativa chama `relato_negacao`, que monta a linha e chama `ev.no_relato`; não introduz segundo escritor ou derivação paralela nesse trajeto (`bin/disjuntor.py:1907`, `bin/disjuntor.py:279`, `bin/disjuntor.py:292`, `bin/disjuntor.py:294`). A ambiguidade entre negativa sem obra e disparo `DISJUNTOR B3 <obra>` é removida na constante, enquanto a mensagem efetiva de parada conserva o formato anterior (`bin/disjuntor.py:1647`, `bin/disjuntor.py:1149`).

## 11/11 03:07 · sessao b01916492971 · - · - · origem=Ronald (tmux)

Nenhum FILA fornecido demonstra bloqueio novo do item (1) neste delta; a porta do agente `verificador` final exige `_obra_de(prompt)` e nega ausência em `bin/disjuntor.py:1842-1845`.

- **FILA 2:** há tradução explícita entre lista persistida e número publicado: `_b3_sem_obra` lê `b3_sem_obra_obs` e deduplica `aid` (`bin/disjuntor.py:2198-2202`); `ler()` publica esse resultado em `b3_sem_obra` (`bin/disjuntor.py:2045`); histórico é contado separadamente (`bin/disjuntor.py:2205-2208`). Não demonstra campo com dois significados na mesma camada.
- **FILA 3:** os limites permanecem: `_FINAL_RE` admite leitura/final na mesma linha sem identificar intenção (`bin/disjuntor.py:177-179`); a porta restringe-se a `subagent_type == 'verificador'` (`bin/disjuntor.py:1842`); o coletor restringe-se a `agentType == 'verificador'` (`bin/disjuntor.py:840`). Portanto uma garantia irrestrita sobre qualquer agente/modelo seria falsa; são limitações existentes fora do delta fornecido [nao verificado quanto ao histórico Git].
- **Chave:** a dispensa é expressa na arquitetura (`bin/disjuntor.py:35`); `main_pretool()` deixa de chamar `pretool()` quando desligado (`bin/disjuntor.py:1896-1900`). Não constitui nova fuga criada pelo delta.
- **FILA 4:** o contador é cumulativo: só acrescenta IDs novos (`bin/disjuntor.py:2178-2183`), e o leitor não aplica janela (`bin/disjuntor.py:2201-2202`). Isso é compatível com “esperado 0 a partir daqui” do aval fornecido [nao verificado em arquivo]; uma ocorrência posterior deixa corretamente registrado que a meta foi violada.
- **FILA 5:** o selo histórico não chama `linha()` (`bin/tests/test_disjuntor.sh:1484-1498`), e o mutante remove validação de elementos (`bin/tests/test_disjuntor.sh:1542-1544`), permitindo morte incidental por `e['aid']` (`bin/disjuntor.py:2202`). É fraqueza do selo existente; a apresentação atual seleciona contador e histórico separadamente (`bin/disjuntor.py:2271-2275`).
- **FILA 6:** o rótulo mencionado só consta do anexo fornecido [nao verificado]; não demonstra falha funcional da guarda, localizada em `bin/disjuntor.py:1842-1845`.

O zero efetivo de produção e a identificação individual das seis obras históricas não estão demonstrados pelos dados fornecidos [nao verificado]. Não executei a suite, que cria e remove arquivos (`bin/tests/test_disjuntor.sh:39-41`, `bin/tests/test_disjuntor.sh:50-55`).

## 11/11 03:07 · sessao 63b7724082f8 · - · - · origem=Ronald (tmux)

1. A regra muda corretamente para `B3-SEM-OBRA`: o extrator aceita hífens e publica a regra integral no RELATO (`bin/disjuntor.py:262`, `bin/disjuntor.py:279`). A negativa continua passando por `trilha('NEGOU', neg)` e `relato_negacao`, sem chamar disparo (`bin/disjuntor.py:1904`, `bin/disjuntor.py:1907`).

2. `ler()` distingue o evento pelo terceiro token da trilha; `NEGOU` incrementa negativas e somente `DISJUNTOR` incrementa disparos (`bin/disjuntor.py:1984`, `bin/disjuntor.py:1995`, `bin/disjuntor.py:2003`). Portanto a negativa nova não entra como disparo `B3`; o disparo verdadeiro permanece anunciado pela transição do estado com detalhe `B3 <obra>` (`bin/disjuntor.py:1012`, `bin/disjuntor.py:1043`).

3. Há colisão **lexical** com IDs legítimos: `B3-SEM-OBRA` e `B3-NAO-SEI` satisfazem `_OBRA_ID`/`_OBRA_HIF`, pois não há reserva desses nomes (`bin/disjuntor.py:157`, `bin/disjuntor.py:163`, `bin/disjuntor.py:619`). `obras_da_linha` consequentemente aceita os tokens das duas regras (`bin/disjuntor.py:637`). Entretanto, seu consumidor encontrado exige além da obra o prefixo `DESENHO ` ou `ERRO-BOBO `; nenhuma negativa satisfaz isso (`bin/disjuntor.py:735`, `bin/disjuntor.py:737`, `bin/disjuntor.py:748`). **Ambas são inertes nesse consumidor**, mesmo que a obra real tenha exatamente um desses IDs (`bin/disjuntor.py:735`, `bin/disjuntor.py:737`, `bin/disjuntor.py:748`).

4. O Stop usa diretamente o mapa retornado por `julga_b3`: só produz `mensagem_b3(obra)` para as obras desse mapa; a mensagem de incerteza é separada (`bin/hook_stop_fila1.py:1204`, `bin/hook_stop_fila1.py:1206`, `bin/hook_stop_fila1.py:1208`). `gerar_estado.py` delega a leitura e a composição a `disjuntor.ler`/`linha`, sem extrator paralelo (`bin/gerar_estado.py:280`).

5. O delta não introduz outro escritor: `relato_negacao` segue chamando `_no_relato().no_relato` (`bin/disjuntor.py:292`, `bin/disjuntor.py:294`). A guarda continua devolvendo a negativa antes de consultar o estado B3 quando `_obra_de` não reconhece o ID (`bin/disjuntor.py:1842`, `bin/disjuntor.py:1845`, `bin/disjuntor.py:1847`).

Falsa negativa adicional decorrente da troca de prefixo nos leitores examinados: não encontrada pelos caminhos citados; consumidores externos à cópia e efeitos em produção: [nao verificado].

## 11/11 03:03 · sessao a8910a847d4e · FIO-E0-V4-E3 · construtor · origem=Code

Diff c8c escrito para revisão. ORM, suite inteira e certificação pelo opus: **[nao verificado]**. Em `juizes.py`, a alteração apenas transporta `agora` nas três chamadas alimentadas do CF; a comparação AST preservou o restante do predicado. (chamados/juizes.py:692,783,1416)

Por item:

| Item | Cura | RED e mutante |
|---|---|---|
| 1 | Alimenta todas as perguntas dos dias elegíveis, incluindo posições desaparecidas. Sem default para `viva_c6`. (chamados/services/tarefa_do_dia.py:492) | ORM: `test_RED_posicao_sumiu_avulso_e_lote_reais`. Sem banco: `test_RED_posicao_ausente_e_alimentada`. Mutante `universo_incompleto`, **rc=1**. (chamados/tests/test_e3_c8c_orm.py:35; chamados/tests/test_e3_c8c_sem_banco.py:40) |
| 2 | Foto privada recorta resposta, validação e materialização posteriores ao horizonte; o construtor entrega essa foto aos leitores. (chamados/services/tarefa_do_dia.py:338,402) | ORM: `test_RED_T1_inteiro_antes_depois_validacao_real_T2`. Mutantes `validacao_futura`, `materializacao_futura`, `resposta_futura`: **rc=1 cada**. (chamados/tests/test_e3_c8c_orm.py:46; chamados/tests/test_e3_c8c_sem_banco.py:121) |
| 3 | CF exige `agora` quando recebe carga. Avulso conserva a leitura do relógio no teto. (escala/servico_jornada.py:292,387) | ORM: `test_RED_CF_agora_12_independe_do_relogio_ambiente`. Sem banco também verifica o comportamento avulso às 12h/20h. Mutante `relogio_ambiente`, **rc=1**. (chamados/tests/test_e3_c8c_orm.py:76; chamados/tests/test_e3_c8c_sem_banco.py:66) |
| 4 | A carga chama o mesmo `escala_vigente`, alimentado pelo índice de vínculos próprios **e geradoras carregadas**. (chamados/services/carga_pergunta_viva.py:89,162; escala/servico_jornada.py:55) | ORM: `test_RED_geradora_de_outro_colaborador_carga_e_avulso`. Mutante `geradora_so_proprio`, **rc=1**. (chamados/tests/test_e3_c8c_orm.py:63; chamados/tests/test_e3_c8c_sem_banco.py:15) |
| 5 | B5-j segue helpers `self/cls` da mesma classe; comentário corrigido. Teto permanece 8. (ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:132,191; ponto/turnos.py:3015) | `test_MORDE_carregador_extraido_da_classe`. Mutantes `selo_nao_segue_helper` e `batidas_cruas_no_helper`: **rc=1 cada**. (ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:195; chamados/tests/e3_c8c_mutantes.py:39) |
| 6 | K6 levanta `ValueError` para linha de outra tripla. (folha/export.py:584) | `test_RED_K6_recusa_outra_tripla`, cobrindo empresa/mês/ano e `registro`. Mutante `tripla_alheia`, **rc=1**, seis assertions. (chamados/tests/test_e3_c8c_sem_banco.py:91) |
| 8a | Exposição administrativa no ramo dispensado fica restrita ao dia sem trabalho; lâmpada acesa de trabalho responde primeiro. Teste E2 preservado. (chamados/services/tarefa_do_dia.py:263; chamados/tests/test_e2_tarefa_do_dia.py:873) | `test_RED_lampada_acesa_antes_da_pergunta`. Mutante `acesa_expoe_admin`, **rc=1**. (chamados/tests/test_e3_c8c_sem_banco.py:32) |
| 8b | `local.date()` saiu do slot; usa `dia_do_turno_da_celula`. Calendário da alimentação reutiliza `_hoje`. Cat raca e registros preservados. (chamados/services/carga_pergunta_viva.py:135; chamados/services/tarefa_do_dia.py:504) | Cat raca existente: **35 testes, rc=0**. Mutante `dia_derivado_do_slot`, **rc=1**. (core/tests/test_medida_turno_do_par.py:478; chamados/tests/test_e3_c8c_sem_banco.py:151) |
| 8c | Removidos `datetime/timedelta` e `JUIZES` mortos. (chamados/tests/e3_c8b_apoio.py:8; chamados/tests/test_e3_c8b_sem_banco.py:14) | Ruff: **rc=1**, módulo ausente; lint **[nao verificado]**. |

A impossibilidade da geradora estrangeira por todos os escritores de cadastro permanece **[nao verificado]**. A reversão copia `dna_anterior['vinculo_id']`; a paridade foi corrigida independentemente disso. (ponto/portas/celula.py:789)

Execuções locais, todas sem banco:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -v chamados.tests.test_e3_tarefa_admin_sem_banco chamados.tests.test_e3_c8b_sem_banco chamados.tests.test_e3_c8c_sem_banco
# rc=0 — 41 testes [nao verificado]

PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_c8b_mutantes
# rc=0 — agregado dos 14 mutantes anteriores

PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_c8c_mutantes
# rc=0 — agregado dos 11 novos mutantes
```

Os REDs iniciais sem banco deram **rc=1 por assertion**, antes das curas; a catraca inicial deu **rc=1, seis falhas**. Os selos B5-j e turno passaram posteriormente com **7 e 35 testes**, usando adaptação em memória apenas de `SimpleTestCase` para `unittest.TestCase`. (chamados/tests/test_e3_c8c_sem_banco.py:15; ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:194; core/tests/test_medida_turno_do_par.py:478)

Comando individual de cada mutante abaixo: todos foram executados e retornaram **rc=1, FAIL>0, ERROR=0**. Definições e executor: chamados/tests/e3_c8c_mutantes.py:16,79.

```bash
mutar() {
  PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_c8c_mutantes --mutante "$1"
}
mutar universo_incompleto
mutar validacao_futura
mutar materializacao_futura
mutar resposta_futura
mutar relogio_ambiente
mutar geradora_so_proprio
mutar selo_nao_segue_helper
mutar batidas_cruas_no_helper
mutar tripla_alheia
mutar acesa_expoe_admin
mutar dia_derivado_do_slot
```

Os quatro TestCase ORM estão em [test_e3_c8c_orm.py:9](/home/ronald/wt-codex-fio-e0-v4-e3-024128/app/chamados/tests/test_e3_c8c_orm.py:9), herdando o cadastro ORM existente, sem `__new__`. Para a sessão executar:

```bash
PYTHONDONTWRITEBYTECODE=1 bin/suite.sh --only chamados.tests.test_e3_c8c_orm.C8cCargaReal
```

Mutantes ORM preparados, com **rc ainda [nao verificado]**; ativação somente em memória. (chamados/tests/e3_c8c_mutantes.py:54)

```bash
E3_C8C_MUTANTE_ORM=universo_incompleto PYTHONDONTWRITEBYTECODE=1 bin/suite.sh --only chamados.tests.test_e3_c8c_orm.C8cCargaReal.test_RED_posicao_sumiu_avulso_e_lote_reais

E3_C8C_MUTANTE_ORM=sem_foto_na_carga PYTHONDONTWRITEBYTECODE=1 bin/suite.sh --only chamados.tests.test_e3_c8c_orm.C8cCargaReal.test_RED_T1_inteiro_antes_depois_validacao_real_T2

E3_C8C_MUTANTE_ORM=relogio_ambiente PYTHONDONTWRITEBYTECODE=1 bin/suite.sh --only chamados.tests.test_e3_c8c_orm.C8cCargaReal.test_RED_CF_agora_12_independe_do_relogio_ambiente

E3_C8C_MUTANTE_ORM=geradora_so_proprio PYTHONDONTWRITEBYTECODE=1 bin/suite.sh --only chamados.tests.test_e3_c8c_orm.C8cCargaReal.test_RED_geradora_de_outro_colaborador_carga_e_avulso
```

Censo produtivo dos chamadores CF, incluindo aliases:

- chamados/juizes.py:690,781,1414.
- chamados/services/disputa_emissao.py:288,806; wrapper lido em :1200,1237 e reexportado por chamados/models.py:4093.
- chamados/management/commands/retratar_chamados_lei_cala.py:45.
- chamados/management/commands/auditar_invariantes_chamados.py:1683,1709,1804.
- ponto/management/commands/reconciliar_grade.py:243.
- ponto/marcos.py:171.
- escala/servico_jornada.py:566.

Chamadas dinâmicas sem identificador literal: **[nao verificado]**.

Escritores e leitores dos valores transitórios tocados:

| Valor | Quem escreve/alimenta | Quem lê |
|---|---|---|
| Foto da pergunta | `_pergunta_no_instante`. (chamados/services/tarefa_do_dia.py:338) | Construtor, passivo, lavra e tarefa. (mesmo arquivo:402,421,426,538) |
| `viva_c6` | Retorno do C6 na carga. (chamados/services/tarefa_do_dia.py:506) | Exposição e residual. (mesmo arquivo:245,328) |
| `CargaDaVida.agora` / kwarg CF `agora` | Carga recebe `tarefa.agora`; C6 encaminha nas três chamadas. (chamados/services/carga_pergunta_viva.py:88; chamados/juizes.py:692,783,1416) | Guarda obrigatória e teto CF. (escala/servico_jornada.py:292,387) |
| `_escalas_c6` / `escalas_por_id` | Índice dos vínculos e candidatos carregados. (chamados/services/carga_pergunta_viva.py:89,162) | Carregador comum `escala_vigente`. (escala/servico_jornada.py:55,58,64) |

Além do C6, os carimbos são lidos pelo passivo E1, pela validade da lavra, pela identidade da tarefa, pelo slot e pela concordância com a luz. (chamados/models.py:3924,3931,3936; chamados/veredito_lavrado.py:224,228; chamados/services/tarefa_do_dia.py:230; chamados/services/materializacao.py:279,358,411; chamados/juizes.py:1323)

Os escritores persistentes continuam nas portas existentes de resposta, validação e materialização. (chamados/services/disputa_emissao.py:1467,1652; chamados/services/validacao.py:134; chamados/models.py:3097)

AST dos 12 arquivos tocados e `git diff --check`: **rc=0**. Schema da tarefa preservado pelo selo existente. (chamados/tests/test_e3_c8b_sem_banco.py:81) Suite inteira, resultados ORM, impacto produtivo e aprovação final: **[nao verificado]**.

## 11/11 02:52 · sessao d7f2c3787aa4 · FIO-E0-V4-E3 · sonda · origem=Code

1. **174 pares, 15 colaboradores e soma geral de candidatas = 0**, conforme o anexo fornecido. [nao verificado]

| Colaborador | Soma de candidatas |
|---|---:|
| col144 | 0 [nao verificado] |
| col209 | 0 [nao verificado] |
| col237 | 0 [nao verificado] |
| col268 | 0 [nao verificado] |
| col312 | 0 [nao verificado] |
| col348 | 0 [nao verificado] |
| col427 | 0 [nao verificado] |
| col574 | 0 [nao verificado] |
| col650 | 0 [nao verificado] |
| col969 | 0 [nao verificado] |
| col1005 | 0 [nao verificado] |
| col1009 | 0 [nao verificado] |
| col1018 | 0 [nao verificado] |
| col1019 | 0 [nao verificado] |
| col1027 | 0 [nao verificado] |

O único `perg_total=1` está em col237, com zero candidatas; o anexo não identifica qual condição a exclui. [nao verificado]

2. **Não há caminho alternativo de seleção para carimbar essas perguntas sem FK de célula.** O escritor percorre `perguntas_absolvidas_pela_celula` e carimba somente as perguntas recebidas desse gerador (`app/chamados/reconciliador.py:584`, `:595`). O gerador parte de `qs_perguntas_absolviveis` (`app/chamados/reconciliador.py:651`), que exige colaborador da disputa, ambos os carimbos nulos e **`celula__isnull=False`** (`app/chamados/reconciliador.py:627-631`). Compartilhar disputa não substitui esse vínculo (`app/chamados/reconciliador.py:627-642`).

3. **Sim, o universo descrito pela sonda é suficiente para essa pergunta:** ela conta perguntas cuja própria célula pertence ao colab/dia e aplica os dois filtros de nulidade; o resultado informado é zero. A implementação da sonda não foi aberta. [nao verificado]

Essa contagem é um **superconjunto** do queryset efetivo: além desses requisitos, o código exige veredito da célula não nulo, não vazio e fora de `VEREDITOS_ACUSAM`; depois, o gerador ainda consulta o juiz e a tranca (`app/chamados/reconciliador.py:637-641`, `:661-668`). Portanto, zero nesse superconjunto implica zero selecionáveis nesses pares por essa via (`app/chamados/reconciliador.py:627-642`).

Uma pergunta ligada à célula de **outro dia** não integra o recorte desses 174 pares só por compartilhar disputa. [nao verificado] O escritor roda por colaborador e pode selecionar outros dias, mas julga a célula vinculada à própria pergunta (`app/chamados/reconciliador.py:628`, `:637-641`, `:651`).

## 11/11 02:52 · sessao 62e1af2f76c7 · - · - · origem=Ronald (tmux)

Censo produtivo do CF na base:

- `chamados/juizes.py:690,781,1414`.
- `chamados/services/disputa_emissao.py:288,806`; wrapper de `:806` chamado em `:1200,1237` e reexportado em `chamados/models.py:4093`.
- `chamados/management/commands/retratar_chamados_lei_cala.py:45`.
- `chamados/management/commands/auditar_invariantes_chamados.py:1683,1709,1804`.
- `ponto/management/commands/reconciliar_grade.py:243`.
- `ponto/marcos.py:171`.
- `escala/servico_jornada.py:554`.

Hits em API e reconciliador são comentários, sem chamadas diretas. Chamadas dinâmicas sem identificador literal: [nao verificado].

A divergência da geradora estrangeira vem da alimentação: avulso entrega diretamente `celula.escala_geradora` ao juiz (`escala/servico_jornada.py:55-58`); lote mantém vínculos somente no balde do proprietário (`ponto/turnos.py:2990-2991`). `_cand` já inclui geradoras pedidas por `mais` (`ponto/turnos.py:2986-2987`), porém o carregador plural coloca a estrangeira no balde `e.colaborador_id` (`escala/alimentacao.py:155-159`). Assim, existe linha carregada suficiente para corrigir sem N+1.

Proposta: fazer `CargaDaVida.vinculo` chamar `escala_vigente` com alimentação opcional dos vínculos já lidos; o corpo comum resolve a geradora por PK, usando índice composto de `_cand` e `_vinculos`, e usa os candidatos do próprio colaborador quando não existe geradora. A seleção continua exclusivamente em `vinculo_do_dia` (`escala/alimentacao.py:267-289`).

Impossibilidade pelo cadastro não está demonstrada. Geração usual vincula a célula ao proprietário do vínculo (`ponto/portas/celula.py:259-260,855-856`), mas reversão copia diretamente `dna_anterior['vinculo_id']` para a FK (`ponto/portas/celula.py:789-791`). Ocorrência produtiva: [nao verificado].

Leitores adicionais dos carimbos pela carga:

- Passivo: `DecisaoPergunta.linhas_da_projecao_sem_linha` lê `respondida_em`, texto, `validada_em`, `validada_por_id` (`chamados/models.py:3924,3931-3938`), chamado em `chamados/services/tarefa_do_dia.py:394`.
- Lavra: `esta_stale` lê `julgado_em` e `janela_expira_em` (`chamados/veredito_lavrado.py:224,228`), chamado em `tarefa_do_dia.py:399`.
- Identidade da tarefa lê `via_resolucao` (`tarefa_do_dia.py:230-231`).
- C6 lê validação, resposta, materialização e via (`chamados/juizes.py:1346,1357,1362,1383,1389,1421`).
- Auxiliares de materialização leem materialização/via (`chamados/juizes.py:288,671,683,699`).
- Slot e concordância leem texto (`chamados/juizes.py:354,1323`; `chamados/services/materializacao.py:279,358,411`).

Escritores: validação real (`chamados/services/validacao.py:134-141`); resposta (`chamados/services/disputa_emissao.py:1462-1468`), resposta automática (`:1652-1657`); carimbo de materialização/via (`chamados/models.py:3099-3101`).

Nenhuma escrita ou execução ORM foi feita nesta exploração.

## 11/11 02:52 · sessao 62dfbb574916 · - · - · origem=Ronald (tmux)

Use `_Base`, que já é `TestCase` ORM real e fornece cadastro, vínculo, colaborador, admin, célula, pergunta e decisão; os construtores reais são `carga_da_tarefa` e `tarefa_do_dia`. (`chamados/tests/test_e2_tarefa_do_dia.py:75`, `:102`, `:124`, `:133`; `chamados/services/tarefa_do_dia.py:546`, `:551`)

- **RED 1:** célula com DNA `{'marcos': {'hi': '08:00', 'hf': '18:00'}}`, lâmpadas somente E08/S18 e pergunta antiga `intervalo_saida`. Faça chamadas independentes pelo avulso e por `carga_da_tarefa([self.c], D, D, agora).tarefa(self.c,D)`. A pergunta continua no universo `_perg`, mas sua posição sumiu da grade; compare o dict inteiro e a exposição administrativa após decisão RESPOSTA. (`chamados/tests/test_e2_tarefa_do_dia.py:102`, `:124`, `:133`; `chamados/services/tarefa_do_dia.py:373`, `:465`)
- **RED 2:** pergunta textual, célula `trabalha=False`/`tipo_dia='folga'`; resposta real em T1 por `responder_pergunta_disputa_core`; dict inteiro em T1; validação real em T2 por `validar_pergunta`; nova carga e mesmo `agora=T1`; dict deve permanecer inteiro igual. (`chamados/services/disputa_emissao.py:1425`, `:1467`, `:1512`; `chamados/services/validacao.py:373`, `:435`)
- A validação textual completa pode retornar `nao_gravou`: texto não tem ramo materializador; o carimbo e a decisão VALIDACAO permanecem, pois a devolução é específica de resposta HORA. Exija no RED que `validada_em == T2` e que a linha VALIDACAO tenha T2; não exija `res['ok']`. (`chamados/services/materializacao.py:1226`, `:1244`; `chamados/services/validacao.py:457`, `:469`, `:496`)
- O escritor menor `_carimbar_validacao` também grava carimbo+linha reais, já usado diretamente por teste existente; a porta completa acima cobre mais do contrato pedido. (`chamados/services/validacao.py:127`; `chamados/tests/test_e1_entradas.py:95`)
- **RED 4:** depois de criar a célula de `self.c`, troque sua FK para `self.ec2` por ORM e compare `escala_vigente(self.c,D)` com `carga_da_vida(carga_real, perguntas).vinculo(self.c,D)`, sem substituir `escala_vigente`. O avulso entrega a geradora nomeada diretamente. (`escala/servico_jornada.py:55`; `chamados/services/carga_pergunta_viva.py:157`)
- A geração normal usa `colaborador_id=ec.colaborador_id`; edição usa o vínculo resolvido para `_cid`. Isso não prova impossibilidade por todos os escritores; classifique essa impossibilidade como `[nao verificado]`. (`ponto/portas/celula.py:259`, `:603`)

Nenhum arquivo escrito e nenhum ORM executado. [nao verificado]

## 11/11 02:50 · sessao 3ce02936cad3 · VERIFICADOR-EM-FABLE · verificador · origem=Code

APROVA

1. **#6 fechado no delta.** O campo persistido `b3_sem_obra` é histórico de IDs string; `b3_sem_obra_obs` contém observações com `aid` e `ts_observado`. O campo determina o significado; o tipo serve para validar. Não encontrei escritor operacional remanescente que acrescente histórico. (`bin/disjuntor.py:2173`, `bin/disjuntor.py:2174`, `bin/disjuntor.py:2182`, `bin/disjuntor.py:2200`, `bin/disjuntor.py:2206`)

   Atenção ao nome na saída: `ler()['b3_sem_obra']` continua sendo o **número derivado de observações**, não a lista histórica persistida. Os consumidores usam esse número. (`bin/disjuntor.py:2044`, `bin/tests/test_disjuntor.sh:430`)

2. **#4 fechado no delta.** Cada contador valida integralmente seu respectivo campo: não lista, elemento de outro tipo, dict sem `aid` string e `aid` vazio resultam em `None`. JSON quebrado ou raiz não objeto também chegam como `None`; a apresentação mostra “nao sei” nos dois lugares. Histórico só desaparece quando vale zero. (`bin/disjuntor.py:396`, `bin/disjuntor.py:398`, `bin/disjuntor.py:2190`, `bin/disjuntor.py:2192`, `bin/disjuntor.py:2200`, `bin/disjuntor.py:2206`, `bin/disjuntor.py:2274`)

   Reproduzi essas formas em memória, inclusive a apresentação. O zero restante é contratual para arquivo/campo ausente ou lista vazia; não encontrei caminho desses leitores que converta conteúdo ilegível em zero. A validação é independente: histórico inválido não torna uma observação ausente inválida. (`bin/disjuntor.py:394`, `bin/disjuntor.py:2189`, `bin/disjuntor.py:2201`, `bin/disjuntor.py:2207`)

3. **Escritores e leitores encontrados em `bin/`:**

   | Sítio | Papel |
   |---|---|
   | `_anota_sem_obra` | Lê histórico e observações para dedup; cria/acrescenta somente observações. (`bin/disjuntor.py:2173`, `bin/disjuntor.py:2178`, `bin/disjuntor.py:2182`) |
   | `decide_b3` / `_com_estado` | Chama o escritor e persiste o estado inteiro sob trava, preservando o histórico. (`bin/disjuntor.py:990`, `bin/disjuntor.py:1054`, `bin/disjuntor.py:351`, `bin/disjuntor.py:371`) |
   | `_b3_sem_obra` / `_b3_sem_obra_hist` | Validam e contam IDs distintos dos respectivos campos. (`bin/disjuntor.py:2200`, `bin/disjuntor.py:2206`) |
   | `ler` / `linha` / CLI | Propagam os dois números e apresentam o resultado. (`bin/disjuntor.py:2044`, `bin/disjuntor.py:2274`, `bin/disjuntor.py:2345`) |
   | `gerar_estado.py` | Usa `ler` e `linha`, sem derivação própria desses campos. (`bin/gerar_estado.py:280`, `bin/gerar_estado.py:281`) |
   | `hook_stop_fila1.py` | Aciona `julga_b3`, que delega a `decide_b3`; não manipula os campos diretamente. (`bin/hook_stop_fila1.py:1204`, `bin/disjuntor.py:1075`) |
   | Testes | Leem número/apresentação; escrevem fixtures dos dois campos; exercitam o escritor e verificam histórico, IDs e tempos. Há também regravação genérica do estado inteiro. (`bin/tests/test_disjuntor.sh:430`, `bin/tests/test_disjuntor.sh:1368`, `bin/tests/test_disjuntor.sh:1483`, `bin/tests/test_disjuntor.sh:1487`, `bin/tests/test_disjuntor.sh:1490`, `bin/tests/test_disjuntor.sh:1493`, `bin/tests/test_disjuntor.sh:1098`) |

   **B3 antigo segue coerente por inspeção:** cria `s1`, aciona Stop e espera `1`; o scanner extrai esse ID, o escritor registra em observações e o contador conta um ID distinto. Execução integral desse caso: **[nao verificado]**. (`bin/tests/test_disjuntor.sh:428`, `bin/tests/test_disjuntor.sh:429`, `bin/tests/test_disjuntor.sh:431`, `bin/disjuntor.py:843`, `bin/disjuntor.py:845`, `bin/disjuntor.py:2182`, `bin/disjuntor.py:2201`)

4. **Pode perder a anotação, mas há sinal no ESTADO.** Com observações não lista, o helper retorna sem registrar a ocorrência e sem aviso próprio; o campo permanece inválido, seu contador retorna `None` e a linha mostra “nao sei”. Falta aviso imediato dessa ocorrência; não há silêncio global nem zero falso. (`bin/disjuntor.py:2175`, `bin/disjuntor.py:2176`, `bin/disjuntor.py:2192`, `bin/disjuntor.py:2274`)

   O dedup suprime igualdade exata de ID: um ID diferente entra; um ID presente no histórico é intencionalmente suprimido. Reuso/colisão real de IDs: **[nao verificado]**. (`bin/disjuntor.py:2177`, `bin/disjuntor.py:2178`, `bin/disjuntor.py:2180`)

5. **Os três mutantes mordem nas reproduções em memória; as âncoras são únicas.** O controle reproduziu as expectativas do caso. (`bin/tests/test_disjuntor.sh:1483`, `bin/tests/test_disjuntor.sh:1494`, `bin/tests/test_disjuntor.sh:1515`, `bin/tests/test_disjuntor.sh:1538`, `bin/tests/test_disjuntor.sh:1542`)

   - `historico-reconta`: acrescenta `aa6` às observações e derruba a expectativa do escritor. (`bin/tests/test_disjuntor.sh:1491`, `bin/tests/test_disjuntor.sh:1494`, `bin/tests/test_disjuntor.sh:1538`)
   - `duplicata-reconta`: produz `2 1`, contrariando `1 1`. (`bin/tests/test_disjuntor.sh:1484`, `bin/tests/test_disjuntor.sh:1485`, `bin/tests/test_disjuntor.sh:1542`)
   - `forma-invalida-vira-zero`: cai primeiro por `KeyError` em `{}`; isolando a fixture seguinte, produz `0 0` em vez de `None None`. Portanto morde, embora a primeira mordida seja por exceção. (`bin/tests/test_disjuntor.sh:1486`, `bin/tests/test_disjuntor.sh:1487`, `bin/tests/test_disjuntor.sh:1539`, `bin/disjuntor.py:2201`)

   **FILA — lacuna do selo:** apesar do comentário, esse caso não chama `linha()`; também não contém fixtures de `aid` vazio/não string nem retorno antecipado do escritor. A correção desses caminhos foi conferida separadamente em memória. (`bin/tests/test_disjuntor.sh:1480`, `bin/tests/test_disjuntor.sh:1482`, `bin/tests/test_disjuntor.sh:1491`, `bin/disjuntor.py:2175`, `bin/disjuntor.py:2200`, `bin/disjuntor.py:2274`)

6. **Nenhuma das classes citadas reaparece como defeito novo demonstrado.** A escrita permanece na mesma porta sob trava; o helper copia o relógio recebido; os consumidores reutilizam os contadores; os mutantes derrubam expectativas; época deixou de depender do tipo. O fallback interno `time.time()` continua presente e não foi introduzido neste delta. (`bin/disjuntor.py:351`, `bin/disjuntor.py:366`, `bin/disjuntor.py:975`, `bin/disjuntor.py:1054`, `bin/disjuntor.py:2182`, `bin/disjuntor.py:2200`, `bin/disjuntor.py:2206`, `bin/gerar_estado.py:281`, `bin/tests/test_disjuntor.sh:1538`)

Suíte shell integral: **[nao verificado]**, pois cria, altera e remove arquivos. Estado efetivo de produção e identificação das obras históricas no RELATO: **[nao verificado]**. (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:41`, `bin/tests/test_disjuntor.sh:1517`)

_3652 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
