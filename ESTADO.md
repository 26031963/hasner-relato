# ESTADO -- o repo e a sessao agora

> **RELATO retido:** /home/ronald/saas-hasner/app/docs/RELATO.md:119 AFIRMA ato sem PROVA ao lado -- divergencias**. APLICADOS **148**, SEPARADOS **31**.
>
> O RELATO nao entrou neste ciclo porque uma afirmacao de ato
> (APLICADO / no ar / feito) esta sem a linha `PROVA:` ao lado. O ESTADO e a SESSAO abaixo
> seguem valendo -- eles descrevem, nao afirmam ato.


_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 27/09/2026 13:54:01.**

| | |
|---|---|
| `HEAD` local | `b93b05b0 [O84] a ata NOMEIA a inversao e o juiz devolvia "nao sei" -- a forma posicional do intermitente` |
| `origin/main` | `73322ebd [O68b-PAPEL] no ar com smoke no worker servido, e onde o Ronald clica` |
| commits a subir (`ahead`) | **1** |

## Fabrica

**fabricante: DESLIGADO** -- dono `Ronald`, desde `2026-09-26 10:01:34`.

- por que: 19-26/09: portas 149->149, noites 0-2 fatias, 28 de 44 caidos, O57
- condicao de saida: criterio do estrutural fechado + corte Ronald
- pacotes caidos: **72** (baseline divergiu 42, nunca lancada 25, o processo morreu sem escrever fim 2, vermelho da ARVORE no lote 2, copia falhou 1)

## `git log --oneline -5`

```
b93b05b0 [O84] a ata NOMEIA a inversao e o juiz devolvia "nao sei" -- a forma posicional do intermitente
73322ebd [O68b-PAPEL] no ar com smoke no worker servido, e onde o Ronald clica
5215187e [TICKETS] linha da O93 -- a regua cobrou no push e estava certa
5e7b56ef [O93] o item vai para a fila como `!`, que e o que ele e -- e o selo das DUAS FILAS estava certo
2c0f8fd4 [O93] PAREI o apply de 09: ha folga trabalhada do 2o tipo, e o maior caso e de 11,13 h a 100%
```

## `git status --short`

```
 M app/docs/BACKLOG.md
 M app/docs/RELATO.md
 M app/docs/TICKETS.md
?? esteira.pausada
```

## PLACAR ESPELHO-VERDADE

| etapa | feitos | itens do programa |
|---|---:|---|
| **E1** PREVISAO INTEGRA | **2/4 = 50%** | selos de frota = 0: vinculo com fim<inicio; 12x36 com 3+ trabalha seguidos; dia de colab ativo sem previsao ou |
| **E3** MOTOR PELO JUIZ | **4/8 = 50%** | o motor le periodos do juiz da batida e jornada do juiz do previsto; DIFF no RELATO + `!` |
| **E4** LEITORES NO MESMO NUMERO | **2/5 = 40%** | selo tela == PDF == fechamento == TXT na frota, 0 divergencia |
| **E5** FECHAMENTO ONLINE | **0/2 = 0%** | fechamento = LEITURA; `recalcular` deixa de existir; so atos persistem |
| **E6** CERTIFICACAO POR ORACULO INDEPENDENTE | **2/6 = 33%** | 0 divergencia nao explicada + 0 dia sem previsao + 0 periodo fora do juiz |

| etapa | item | estado | prova |
|---|---|---|---|
| E1 | nascer com data_fim < data_inicio e RECUSADO pelo banco (nao so pelo servico | FEITO | corte 27/09 02:0x: CHECK `ec_vigencia_fim_nunca_antes_do_inicio` NOT VALID (escala/0041); selo `escala.tests.test_vigencia_constraint_e_o_juiz` (15 ca |
| E1 | o passivo de vigencia impossivel que ja existe -- lista que so encolhe | em curso | MEDIDO 27/09 02:2x: 53 vinculos; 43 CONGELADOS por cruzarem competencia exportada (L-092) e 10 livres para o propositor com `!` caso a caso. logs/e1_v |
| E1 | celula de trabalho sem previsao valida | em curso | medido: 4 colabs com zero vinculo E zero celula (col924/391/43/942, ~221 h) |
| E1 | qual vinculo vale no dia tem UM juiz (CelulaDia.escala_geradora) | FEITO | O69 aplicada em 09: 654,74 h; `escala/alimentacao.py::vinculo_do_dia`; selo `ponto.tests.test_vinculo_do_dia_pela_celula` (8 casos) |
| E3 | o MARCO manda, nunca o tipo gravado | FEITO | selo `ponto.tests.test_e3_completa_o_marco_manda` + `test_selo_motor_nao_pareia_pelo_tipo_gravado`; aplicada em 09 |
| E3 | jornada do dia vem de minutos_previstos_do_dia, nunca de minutos_jornada | FEITO | HAIKU `jornada_de_fonte_lixo` (lavrar_jornada_lixo, 11 acessos em 4 arquivos) |
| E3 | os leitores de turno tambem perguntam ao juiz da batida (O65) | FEITO | O65 aplicada: dinheiro ZERO, turnos abertos 806 -> 744; selo `SeloLeitorDeTurnoTambemLeAAtaTest` |
| E3 | a jornada pertence ao dia de INICIO, nas tres derivacoes (O76) | FEITO | O76: `MotorBase._dia_do_turno` passa a servir o MotorBase; RED col382 DSR perdido=1 -> ok=1; aplicada 27/09 |
| E3 | o dia que a ata nao explica nao e pago pelo plano B em silencio (O68b) | em curso | medido na frota de 09: 1.484 dia-colab (19,4% do que o motor julga), 723 por dia sem ata + 761 dos 28 colabs de cadastro partido falso |
| E3 | colab com turno aberto em massa (o motor nao fecha o par, a tela tambem nao  | **FALTA** | medido: col788 63 turnos abertos, col923 46, col880 11 |
| E3 | os 30 separados do corte (b) seguem no motor VELHO no gravado de 09 | em curso | medido: 30 colabs restaurados (logs/apply_modo24h_antes.json); duas causas -- familia 100%/dobra de feriado (17, pergunta de LEI aberta) e DSR/reflexo |
| E3 | no turno partido sem intervalo declarado, a volta da pausa nao e atraso (O73 | **FALTA** | RED col81 (te#189 16:00-00:00): "Atraso: entrada as 18:29 (previsto 16:00)" na volta do intervalo; 6 templates `turno_partido` com intervalo_modo=dura |
| E4 | o cartao PDF desenha o mesmo que a tela | FEITO | `pdf_x_espelho_divergentes` = 0 em 199 colabs (sombra, pos-O69) |
| E4 | o cartao e o TXT no mesmo numero | FEITO | `cartao_x_txt_divergentes` = 0 na competencia 09 |
| E4 | o topo do cartao e a SOMA das linhas | **FALTA** | MEDIDO: topo x coluna diverge (col515 11,13 x 92,30 h). 27/09 02:1x: e o MESMO defeito que a classe C da O83 -- o TOPO concorda com a FOLHA (11,00 h)  |
| E4 | colunas Atraso e Saida antecipada lendo a folha (O51b) | **FALTA** | (sem prova) |
| E4 | o tipo de escala exibido sai da DEFINICAO, nao do rotulo gravado | em curso | O74: `rotulo_do_desenho` + filtro `desenho_do_turno`; ficha no ar, lista de tipos espera o deploy |
| E5 | a 09 lida da celula, sem gravado envelhecendo | **FALTA** | (sem prova) |
| E5 | competencia exportada nao muda o gravado (L-092) | **FALTA** | SEM SELO -- obra O80. MEDIDO o risco: 18 exports em prod, incluindo emp2 08/2026 com 352 linhas entregues |
| E6 | calculador independente do motor, dia a dia | FEITO | `/tmp/e6b.py` roda e publica CSV; metodo VALIDADO em 27/09 (erro real no campo comparado, medido em 1,0 h contra vao de 87 h) |
| E6 | ZERO divergencia nao explicada entre oraculo e espelho | **FALTA** | MEDIDO o contrario em 27/09 ~01:10: 644 de 7.536 dias (8,5%) fora de 10 min -- 257 acima de 60 min, 233 entre 10 e 60, 77 com espelho zero e trabalho  |
| E6 | o oraculo compara contra espelho INTEGRO, nao contra o builder | **FALTA** | MEDIDO: 235 dos colabs caem no builder (espelho.py:352-361). Causa = O81 (3.986 dias de ata agregada em 06/07/08 arrastando 09 via espelho.py:585) |
| E6 | dia de batida impar nao fica fora da certificacao em silencio | **FALTA** | MEDIDO: 473 dias saem da comparacao por batida impar -- o oraculo nao os julga e ninguem mais responde por eles |
| E6 | o piso de horas que o oraculo acusa tem causa por colab | em curso | CLASSIFICADO em 27/09 02:1x (O83), com a regua trabalhadas+folga: 84 colabs / 1.602,2 h em 4 classes (B 27/733,7 · C 32/376,4 · D 21/274,3 · A 4/217,7 |
| E6 | hora de folga trabalhada sem escala certa entra em horas_trabalhadas | FEITO | aplicada em 09: +431,90 h em 11 colabs, HE em zero, 0 saem do TXT; reversao logs/apply_folga_antes.json |

**E6 -- calculador independente (`/tmp/e6b.py`), ultima rodada 27/09/2026 ~01:10:** **91.5% dos dias BATEM** (ate 10 min) em 7536 dias comparados; 160 colabs com divergencia.

## painel da sessao (ultimas 40 linhas)

```

  O68b-PAPEL — a órfã do col904 fechou (18:50 → 07:00 = 12,16 h), mas a causa era maior: a guarda contava batidas por data de calendário, e num turno que cruza a meia-noite o hf
  mora em D+1 — a alimentação da O65 nunca alcançava um 12x36 noturno. Minha 1ª versão foi reprovada pela própria PROVA (turnos_abertos 620 → 665) e ganhou três guardas do seu
  critério: duas pontas, L-084 lida de onde mora, e minuto com duas batidas não se endereça. No ar, com smoke no worker servido (workers 13:22, arquivos 12:52).

  O84 — o juiz aprendeu a forma posicional do intermitente. O col85 21/08 fecha 18:56 → 07:02 = 12,09 h e vai a 0 abertos no mês; o col382 também fechou, porque o leitor passou a
  ler em_aberto/entrada/saida do juiz em vez de procurar chaves hi/hf — juiz paralelo a menos. A metade que não muda tem selo: sem marco é impossível medir desvio, e sobre desvio
  o juiz segue calado. Isso é impossibilidade x cobertura parcial, não conveniência.

  Parei duas vezes para medir, e nas duas o número me derrubou:

  1. PAREI o apply de 09 pelo seu critério da folga trabalhada. Há caso do 2º tipo e o maior não é de 1 h: col616 14/09, célula FOLGA, 05:53 → 18:00 = 11,13 h em hora normal, sem
     o 100% — enquanto 18/09 e 20/09 do mesmo colab, forma idêntica, são folga trabalhada. Espionei eh_dia_trabalho durante o calcular_mes: nos dias que caem em hora normal o
     motor pergunta pelo dia (False) e depois pelo dia anterior (True), e em 14/09 não existe período nenhum em 13/09 para a herança vir de. Virou O93, ! no topo do PENDENTES. Os
     3 casos do col297 são do 1º tipo e estão corretos.
  2. horas_noturnas de +24,55 para −2,87 na O84 — não sabia explicar, então não deploiei. Isolado nas duas árvores contra o mesmo banco: 4 colabs, todos intermitentes, e o noturno
     era inventado. O col830 faz turno de DIA (06:48 → 18:50) e tinha 8,00 h de adicional 22h-05h vindas de um turno que ficava aberto e vazava pela noite; fechado, o certo é
     zero.

  Também cai um número que eu havia publicado: as −279 h de folga trabalhada da sombra são artefato do ensaio — o campo depende da lavra do esmeril em logs/, e o container da
  sombra monta --tmpfs /app/logs. O comentário do próprio sítio já media isso (8 colabs, até 134,71 h).

  Falta fechar: veredito da suíte da O84 → push → deploy → smoke. Depois, classe C residual → E5 fatia 1 → E6 rodada 3. O apply de dinheiro espera seu ! na O93; as curas de código
  sobem.

✻ Worked for 42m 38s · done 1:52 PM · 3 shells still running

❯ aval Ronald: O93 e cura: a heranca de dia (periodo que comeca ate 6 h depois do anterior herda o dia dele) so vale quando EXISTE periodo anterior terminando nesse intervalo;
  sem periodo anterior, o dia e o do inicio (L-085). RED: col616 14/09 (05:53-18:00, celula folga) = folga trabalhada; selos: 18/09 e 20/09 continuam iguais e a volta do almoco
  do plantao noturno continua herdando. DIFF pelo criterio do MODO 24H. !
  E a sombra passa a montar a lavra do esmeril (logs/esmeril_espelho.json) em todo ensaio de dinheiro: sem ela o ensaio sai infiel (-279 h e 134,71 h ja foram artefato). Selo:
  ensaio sem lavra = recusa com aviso.

✢ Lollygagging…
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 3 shells · esc to interrupt · ← for agents · ↓ to manage
  ⧉  index
```
