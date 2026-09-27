# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 26/09/2026 23:40:41.**

| | |
|---|---|
| `HEAD` local | `4f82c385 [O71+O68] o cadastro que mente: NENHUM dos 37 "turno_partido" e partido, e o maior vao e 70 min` |
| `origin/main` | `d39c0696 [O68 PLANO-B-MEDIDO] o plano B tem tamanho: 758 dia-colab, 11,0% do que o motor julga` |
| commits a subir (`ahead`) | **1** |

## Fabrica

**fabricante: DESLIGADO** -- dono `Ronald`, desde `2026-09-26 10:01:34`.

- por que: 19-26/09: portas 149->149, noites 0-2 fatias, 28 de 44 caidos, O57
- condicao de saida: criterio do estrutural fechado + corte Ronald
- pacotes caidos: **72** (baseline divergiu 42, nunca lancada 25, o processo morreu sem escrever fim 2, vermelho da ARVORE no lote 2, copia falhou 1)

## `git log --oneline -5`

```
4f82c385 [O71+O68] o cadastro que mente: NENHUM dos 37 "turno_partido" e partido, e o maior vao e 70 min
d39c0696 [O68 PLANO-B-MEDIDO] o plano B tem tamanho: 758 dia-colab, 11,0% do que o motor julga
a497ed37 [O69 VINCULO-DO-DIA] o espelho e o cartao perguntam a celula: a regra propria tinha QUATRO copias
068e830b TICKETS: linhas da O69 (aplicada) e da O67 (medida) que a regua cobrou
a92ae9c4 [O69 VINCULO-DO-DIA] APLICADA em 09: 654,74 h, e os sobrepostos cairam de 116h para 7 minutos
```

## `git status --short`

```
 M CLAUDE.md
 M app/config/crons.py
 M app/config/crons_duracao.json
 M app/core/juizes.py
 M app/core/templatetags/hasner_filters.py
 M app/docs/ARQUITETURA.mmd
 M app/docs/BACKLOG.md
 M app/docs/CORTES.md
 M app/docs/PENDENTES_RONALD.json
 M app/docs/PROMPTS.md
 M app/docs/RELATO.md
 M app/escala/services/cadastro_tipo.py
 M app/escala/servico_jornada.py
 M app/escala/tests/test_jm_derivado.py
 M app/ponto/management/commands/diff_reclassificar_partido.py
 M app/ponto/management/commands/plano_b_no_dinheiro.py
 M app/ponto/motor_calculo_v2.py
 M app/relatorios/management/commands/vinculo_do_dia_divergentes.py
 M bin/relato.sh
?? app/ponto/management/commands/aplicar_09_corte_b.py
?? app/ponto/management/commands/dna_x_batida_real.py
?? esteira.pausada
```

## painel da sessao (ultimas 40 linhas)

```
● O apply pousou. Os três números que eu me comprometi a conferir passaram — mas a tabela de 26 campos mostra três grupos que não estavam no seu critério, e isso vem primeiro.

  Ran 1 shell command

❯ RELATO bloqueado desde 23:08: RELATO.md:23 "Item (1) FEITO: a porta nova saiu da arvore" sem linha PROVA -- ponha a PROVA (hora dos workers x hora do arquivo, e o grep da
  chamada comentada em cadastro_tipo.py:324) e republique. E cura o efeito colateral do selo: afirmacao sem prova barra so o RELATO; ESTADO e SESSAO publicam sempre, com um aviso
  no topo do ESTADO dizendo "RELATO retido: <linha>". Nenhuma lei nova.

● RELATO retido, e o selo está certo — eu escrevi "FEITO" sem prova ao lado. Ponho a prova e curo o efeito colateral:

  Ran 3 shell commands

● O mtime é 23:21 e os workers subiram 23:35 (02:35 UTC) — o deploy carregou esta versão. E a função nem existe mais: _recusa_partido_sem_vao tem 0 ocorrências. Ponho a PROVA e
  curo o relato.sh:

  Ran 2 shell commands

● RELATO republicado — 4dc5cc9, e o selo da prova está verde: 0 afirmação sem prova. A raspagem trocou 188 nomes com 0 vazamento.

  A PROVA que eu tinha deixado de escrever, agora ao lado da linha:

  grep -c '_recusa_partido_sem_vao' = 0 — a função que recusava não existe mais na árvore; foi substituída por descreve_turno_partido, que responde pelo vão em vez de barrar.
  Chamada ativa: 0. Arquivo com mtime 23:21:22, workers do saas_ui de pé desde 23:35 — os servidos carregaram esta versão, 14 minutos depois de ela existir. E o caso que morde:
  test_MORDE_a_porta_SALVA_noturno_continuo cadastra um 6x1 noturno contínuo e passa.

  E o efeito colateral está curado. Agora ESTADO e SESSAO publicam sempre, e o topo do ESTADO leva > **RELATO retido:** <linha> com a explicação de que falta uma página.

  O motivo de eu ter escrito isso no código, e não só consertado a linha: um selo que cala o mensageiro inteiro por causa de uma frase deixa a casa muda. O RELATO ficou retido das
  23:08 até agora, e junto com ele pararam o ESTADO e a SESSÃO — que não afirmam ato nenhum e eram exatamente o que permitiria a alguém de fora ver que a esteira estava andando.
  Ficar mudo é o que custou 26 dias em 04/09, e o selo tinha acabado de reproduzir a forma em escala menor.

  Falta, na sua sequência: cartao_x_txt e pdf_x_espelho de 09 = 0, depois o template da O74 e o smoke.

✻ Baked for 3m 12s · done 11:40 PM · 1 shell still running
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ mede cartao_x_txt e pdf_x_espelho de 09
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 1 shell · ← for agents · ↓ to manage
  ⧉  index
```
