# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 11:21** · publicado em **12/11/2026 11:21** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 11:21 · sessao 94df418df845 · LASTRO-FAIXA-SEM-DEFAULT · construtor · origem=Code

Curas, REDs, mutantes e comando para a sombra estão no [LASTRO_C14.txt](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/LASTRO_C14.txt:7).

**81 testes sem banco e ruff passaram**, com RC=0 ([saídas](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/evidencia_c14_validacao.txt:403)). [nao verificado]

A entrega permanece parcial: parei o ponto do selo de marca espúria porque sua premissa depende da grade própria, conforme sua condição literal ([pendência](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/LASTRO_C14.txt:36)). Os vizinhos Django não carregaram por falta da dependência ([saída](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/evidencia_c14_validacao.txt:109)).

Sem execução da sombra ou commit ([escopo](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/evidencia_c14_escopo.txt:35)). [nao verificado]

## 12/11 11:19 · sessao 49c101e1bfcf · AUDITORIA-DE-TRAVAS-CORTE · censo · origem=Code

Regra adotada, antes dos números:

1. Uma linha do TSV = uma run, incluindo cópias em `opus_final`. A numeração abaixo começa na primeira linha de cada anexo.
2. Associação pela pasta; separação por nome quando a pasta reúne obras diferentes, como E1/E2/E1-FILA em `fio_e0_v4`. Associações e agrupamentos por nome são **[nao verificado]**.
3. FINAL candidata = última linha com horário **estritamente anterior** ao pouso associado. Empate entre runs: última linha do TSV. Empate com o minuto do commit, ausência de pouso identificável ou execução posterior ao pouso: classificação temporal indeterminada **[nao verificado]**. [nao verificado]
4. FAILED intermediária: primeiro procurar alteração posterior do arquivo/módulo em commit da mesma obra; depois verificar se **todos os IDs completos** dos testes vermelhos aparecem em outras obras com distância absoluta ≤24 h; restante indeterminado. Coincidência não prova causalidade **[nao verificado]**. [nao verificado]

**O censo fecha por linha; o número de “mudaram decisão” não é verificável com o material fornecido.** O gitlog não informa arquivos alterados por commit, e o código atual não permite reconstruir essas alterações históricas **[nao verificado]**. [nao verificado]

| Classificação nominal | Total | FAILED | OK |
|---|---:|---:|---:|
| FINAL candidatas | 88 | 3 | 85 |
| INTERMEDIARIAS | 344 | 109 | 235 |
| Classificação temporal indeterminada | 94 | 28 | 66 |
| **Total** | **526** | **140** | **386** |

Fonte: `suites_14d.tsv:1–526`; classificação por associação de nomes **[nao verificado]**.

São **525 linhas com rótulo de pasta/obra e 1 SEM obra**, `integrador.log`. Rótulo de pasta não comprova identidade da obra **[nao verificado]**. Fontes: `suites_14d.tsv:1–526`, especialmente `suites_14d.tsv:526`.

As **94 temporalmente indeterminadas** são: 84 sem pouso identificável, oito empatadas no minuto do pouso, uma posterior ao pouso conhecido (`lastro2/construcao_green.log`) e uma cujo commit associado diz “cura”, sem pouso explícito (`o261/suite_bd0aef09.log`) **[nao verificado]**.

Para classificar cada linha sem reproduzir os 526 registros: estas são as **88 FINAL candidatas**; a lista seguinte contém as 94 temporalmente indeterminadas; **todas as demais linhas são as 344 INTERMEDIARIAS**, segundo a regra acima **[nao verificado]**.

| Data | Linhas FINAL candidatas em `suites_14d.tsv` |
|---|---|
| 29/10 | 2, 19, 21, 29, 32 |
| 30/10 | 39, 50, 56, 60, 71 |
| 31/10 | 83, 84, 88, 92, 93, 97, 99, 108, 112, 114, 119, 123, 138 |
| 01/11 | 141, 149, 159 |
| 02/11 | 170, 173, 181, 183, 185, 194 |
| 03/11 | 199 |
| 04/11 | 204, 207, 208, 210–212, 215, 218, 222, 224–225 |
| 05/11 | 235, 238, 248, 251–252, 260, 263–264, 273, 275, 277, 280, 283, 289 |
| 06/11 | 303, 312–313, 319–320, 322, 330–331, 335, 343 |
| 07/11 | 353, 357, 378, 389 |
| 08/11 | 407, 437 |
| 09/11 | 443, 446, 448, 456, 465, 469, 471 |
| 10/11 | 474, 477, 487 |
| 11/11 | 502–503, 509, 516 |

As três FINAL candidatas FAILED são ORFAS, O261-V e LASTRO-3 (`suites_14d.tsv:19`, `suites_14d.tsv:71`, `suites_14d.tsv:313`); associação aos pousos **[nao verificado]**.

As 94 linhas temporalmente indeterminadas, em `suites_14d.tsv`, são **[nao verificado: associação ao pouso]**: [nao verificado]

```text
33–34, 36–38, 57, 61–62, 73, 96, 100, 102, 150, 171,
174–180, 191, 193, 198, 200–201, 209, 226–227, 243, 253,
284, 287, 290, 292–296, 298–300, 305–308, 315, 317,
323–326, 332–334, 336, 339, 366, 409, 449–450, 453–454,
460, 463–464, 480, 488–495, 504–505, 508, 510–515, 517–526
```

**Das 109 INTERMEDIARIAS FAILED, nenhuma pode ser atribuída com comprovação a “mudou decisão” ou a “ruído exclusivo”.** Todas permanecem indeterminadas quanto a essa divisão **[nao verificado]**.

Dentro delas, **17 atendem ao critério observável de coincidência integral em outras obras em até 24 h**: linhas 6, 30, 65, 79, 107, 130, 291, 365, 368, 439, 447, 451, 466, 468, 470, 478 e 506 de `suites_14d.tsv`. Os IDs constam de `suites_14d_falhas.tsv:3`, `:11`, `:20`, `:26`, `:31`, `:35`, `:63`, `:82–83`, `:103`, `:106`, `:108`, `:112–114`, `:116` e `:132`; distinção entre obras **[nao verificado]**.

Essas 17 são **candidatas a ruído**, pois falta excluir alteração posterior na própria obra. As outras **92** não preencheram esse critério integral; isso também não demonstra cura **[nao verificado]**.

Das OK, são **235 intermediárias**, sem mudança de decisão pela definição solicitada. As **85 OK escolhidas como últimas antes do pouso** estão entre as FINAL candidatas; outras **66 OK** têm classificação temporal indeterminada (`suites_14d.tsv:1–526`; associação **[nao verificado]**). [nao verificado]

| Tabela final solicitada | Resultado |
|---|---:|
| Total | **526** |
| Intermediárias na reconstrução nominal | **344** **[nao verificado]** |
| Intermediárias que mudaram decisão | **[nao verificado] — 0 comprovadas** |
| Ruído exclusivo | **[nao verificado] — 0 comprovadas** |
| FAILED intermediárias indeterminadas | **109**, incluindo 17 candidatas a ruído |
| Runs sem classificação FINAL/INTERMEDIARIA resolvida | **94** **[nao verificado]** |

Fonte das contagens: `suites_14d.tsv:1–526` e `suites_14d_falhas.tsv:1–140`. “0 comprovadas” expressa ausência de comprovação, **não zero ocorrências reais [nao verificado]**.

As dez obras com mais intermediárias **na reconstrução nominal**, excluídas as 94 linhas temporalmente indeterminadas, são **[nao verificado: associações por nome]**:

| Obra/agrupamento adotado | Intermediárias | Fonte em `suites_14d.tsv` |
|---|---:|---|
| FIO-E0-V4-E1 | 61 | `:346–437` |
| FIO-E0-V4-E2 | 14 | `:358–448` |
| COBPARC | 13 | `:142–170` |
| GRADE-FOLGA-JUIZ (`grade_folga` + `grade_juiz`) | 13 | `:106–149` |
| O261-E-F5C (`F5` + `F5C`) | 13 | `:63–88` |
| JUIZ-VARREDURA-1 | 10 | `:228–264` |
| O261-VETO-A | 10 | `:40–50` |
| ORFAS | 10 | `:3–19` |
| V3-P5 (`v3per` + `v3p5`) | 10 | `:124–141` |
| V2-BUGS-DO-MOTOR, item 2 | 9 | `:338–357` |

O ranking definitivo e a quantidade causal pedida no item 8 permanecem **[nao verificado]**, pelas associações nominais, pelas 94 posições temporais não resolvidas e pela ausência dos arquivos alterados por commit. [nao verificado]

## 12/11 11:15 · sessao 78f2590fba58 · - · - · origem=Ronald (tmux)

Mapa consolidado: **88 FINAL estritas: 85 OK e 3 FAILED**; **94 com classificação temporal indeterminada**; sobre 526 linhas, restam **344 INTERMEDIÁRIAS**. Todas as associações obra↔pasta/pouso são por nome **[nao verificado]**. [nao verificado]

Os três FAILED finais são `orfas/suite_inteira_6.log` (10-29 08:53), `o261v/suite3_9c9e030e.log` (10-30 23:07) e `lastro3/suite_v3.log` (11-06 09:00). **[nao verificado: associação por nome]**

As 94 linhas temporalmente indeterminadas compreendem:

- 84 linhas em 25 grupos sem pouso identificável posterior. [nao verificado]
- 8 linhas empatadas no minuto do pouso: `o261v`, `medida_programa`, `o261e/d3`, `disjuntor`, `e3solo`, `mc`, `jv2p`, `h12h06b/pauta08`. [nao verificado]
- `lastro2/construcao_green.log`, posterior ao pouso conhecido.
- `o261/suite_bd0aef09.log`, cuja associação depende de tratar um commit “cura” como pouso. **[nao verificado]**

Para as duas linhas `lastro2/.../suite_v2.log` de 11-06 05:55, a FINAL única usa a última linha do TSV, `lastro2/suite_v2.log`; a ordem real entre ambas é **[nao verificado]**.

A lista completa das 88 FINAL e os hashes dos pousos foi enviada nas mensagens anteriores.

## 12/11 11:15 · sessao 628c0cf32473 · - · - · origem=Ronald (tmux)

Não há `.git` nem anexos locais nesta cópia; não foi possível verificar arquivos alterados por commit. O gitlog fornecido contém assunto, data e hash, sem diff. Portanto, “mudou decisão” e “ruído exclusivo” não são quantificáveis com comprovação. [nao verificado] [nao verificado]

Coincidências integrais candidatas a ruído, contando IDs completos de teste e distância absoluta ≤24 h: [nao verificado]

| Linhas em `suites_14d_falhas.tsv` | Contrapartes |
|---|---|
| 3, 5, 11 | Entre si: diagrama |
| 20 | 18: import resolve |
| 26 | 18: diagrama |
| 31 | 26 ou 35: diagrama |
| 35 | 31 ou 37: diagrama |
| 63 | 57 ou 73: cron |
| 69 | 77: chokepoint |
| 82 | 83: signal âncora |
| 83 | 80 ou 82: signal âncora |
| 103 | 105; alternativamente 112 para `classificar` e 104 para `linhas` |
| 106, 107, 108 | 103–105 e entre si: catraca linhas |
| 112, 113 | 103 ou 105: classificar |
| 114 | 109: diagrama e mapa |
| 116 | 121, 124, 127 ou 132: catraca linhas |
| 132 | 116, 121, 124, 127 ou 135: catraca linhas |

São **20 candidatas**, separando E1/E2/FILA dentro de `fio_e0_v4`; **18** se usar somente pasta raiz. Todas as separações de obra por nome são [nao verificado]. Referências: linhas indicadas de `suites_14d_falhas.tsv`.

Duas candidatas parecem FINAL FAILED, devendo sair das intermediárias: linha 5, `orfas/suite_inteira_6.log`, antes de `ac7216078` em 29/10 09:24; linha 69, `lastro3/suite_v3.log`, antes de `24946096d` em 06/11 09:43. Associação por nome [nao verificado]. Isso deixa **18 candidatas intermediárias** com divisão E1/E2/FILA, ou **16** com pasta raiz.

A precedência “cura antes de ruído” continua impossível de verificar: as candidatas podem também ter recebido alterações posteriores na própria obra. A classificação estrita deve manter todo FAILED intermediário como indeterminado e apresentar as coincidências como subtotal observável. [nao verificado]

O código atual permite resolver módulos, por exemplo `app/core/tests/test_medida_turno_do_par.py:544`, `app/core/tests/test_medida_turno_do_par.py:550` e `app/core/tests/test_selo_diagrama_do_codigo.py:42`; ele não fornece o histórico de alterações. [nao verificado]

Há inconsistência na linha 140 do anexo de falhas: veredito `failures=5`, mas lista extensa de FAIL e dois ERROR. Conta como uma linha/run; composição histórica dessa lista [nao verificado].

## 12/11 11:14 · sessao ae97398abee9 · - · - · origem=Ronald (tmux)

Censo por linha do anexo: **526 runs = 140 FAILED + 386 OK; 111 pastas/raízes distintas**, incluindo `integrador.log` como raiz sem pasta. Fontes: `suites_14d.tsv:1–526`, `suites_14d_falhas.tsv:1–140` (numeração do anexo textual).

Formato abaixo: `pasta total/FAILED`; OK = total − FAILED. Fonte de todas as contagens: `suites_14d.tsv:1–526`.

```text
fio_e0_v4 83/27
o261e 40/13
cobparc 14/3
o261 12/3
orfas 11/3
e4cal 11/2
jv1 11/2
c20_fiomudo 11/2
v2bugs 10/2
grade_juiz 9/1
BO-0611-05 9/2
col129 8/2
v3p5 8/0
f3_beco1 8/3
dme 7/0
otimiza3 7/0
FIO-E0-V4-E3 7/4
calendario_hoje 6/1
cobparc_relogio 6/1
jv3 6/2
jv4 6/0
h12h06 6/1
h12h06b 6/1
fio_e3 6/3
BO-DISPUTA-SEM-PERGUNTA 6/6
E4B-ALCANCE 6/4
grupo1 5/3
v3retoma 5/0
grade_folga 5/1
f1c4emis 5/1
lastro2 5/2
esemvivab 5/0
LASTRO-FAIXA 5/5
justif 4/2
o261n 4/1
o261v 4/2
vfase_asof 4/0
mc 4/1
n1im 4/1
f346 4/0
plantio_mais1 4/3
motor_turno 4/0
par_nao_adjacente 4/1
selos_canon 4/1
FIO-E0-V4-E4-ESCRITORES 4/2
FIO-E0-V4-E4A-2 4/1
disjuntor 3/1
e4_7iii 3/1
v3per 3/1
lenvivos 3/0
toast 3/0
e4app 3/2
juizvarredura2 3/0
n1alim 3/1
snc2 3/1
pep3 3/1
o10c 3/0
codexneg 3/0
cxr_tela 3/1
UI-PAINEL 3/1
DATA-EVENTO-CRU 3/0
TRIAGEM-OFFLINE-SEM-CAMERA 3/0
outbox 2/1
grupo1p1 2/0
suite_impacto 2/1
medida_programa 2/0
e3status 2/1
comboio 2/0
e3isento 2/0
cobparc_rotulo 2/0
isento_h10 2/0
noreverse 2/0
n1pna 2/2
he 2/0
cronsmarca 2/0
c20_o169 2/0
pep 2/2
akmed 2/0
ferias2x 2/1
e1fila_v22 2/1
CODEX-2-RAIAS 2/0
MODO-COMUM-PASSADO-SEM-TURNO 2/1
O169-FECHADA 2/0
COMBOIO-1556 2/1
fioadmin 1/0
e3solo 1/0
reprova_desenho 1/0
instr 1/0
comboio_instr 1/0
e4_comboio 1/0
contencao 1/0
cods_ato 1/0
e4_8 1/0
e4esp 1/0
cron15 1/0
sw 1/0
jv2p 1/0
sombra_c1 1/0
lastro3 1/1
fiomudob 1/0
pep5 1/0
o10b 1/0
r1pub 1/0
comboio_0611 1/0
esminst 1/0
ata_pausa_0811 1/1
auditoria_0911 1/0
REVERTER-DIA-SEM-RECALCULO 1/0
FIO-E0-V4-E4B 1/1
BO-ROTULO-ENCERRADO 1/0
integrador.log 1/1
```

Separação solicitada por nome de arquivo — identidade de obra [nao verificado]:

| Pasta | Stream inferido | Runs | FAILED | OK |
|---|---|---:|---:|---:|
| fio_e0_v4 | E1 | 62 | 20 | 42 |
| fio_e0_v4 | E2 | 15 | 6 | 9 |
| fio_e0_v4 | E1FILA | 6 | 1 | 5 |
| o261e | F1 | 4 | 4 | 0 |
| o261e | D3 | 1 | 0 | 1 |
| o261e | F5 | 8 | 2 | 6 |
| o261e | F5C | 6 | 1 | 5 |
| o261e | F1C4 | 5 | 2 | 3 |
| o261e | F1C4-3 | 3 | 1 | 2 |
| o261e | F2 | 9 | 2 | 7 |
| o261e | F2FILA | 3 | 1 | 2 |
| o261e | dj → DISJUNTOR | 1 | 0 | 1 |

E2 inclui 11 `green_e2*` em 07/11, `e2_grade_recorte` em 08/11 e três `e2_rebase` em 09/11. E1FILA inclui quatro `e1_fila*` em 08/11 e dois em 09/11. Fonte: `suites_14d.tsv:347–448`. Associações [nao verificado].

Há **10 linhas em `opus_final`** com Ran/tempo/veredito repetidos fora da subpasta: 3 FAILED e 7 OK; mantidas na contagem. Distribuição: jv3 1, c20_fiomudo 4, pep 1, otimiza3 2, lastro2 2. Fonte: `suites_14d.tsv:282–312`; identidade física da execução [nao verificado].

Faixas do anexo úteis para citações:

```text
29/10: 1–33
30/10: 34–74
31/10: 75–139
01/11: 140–168
02/11: 169–198
03/11: 199
04/11: 200–228
05/11: 229–289
06/11: 290–346
07/11: 347–397
08/11: 398–441
09/11: 442–473
10/11: 474–490
11/11: 491–520
12/11: 521–526
```

## 12/11 11:06 · sessao e7500f65d2e9 · AUDITORIA-DE-TRAVAS-CORTE · censo · origem=Code

Numerei cada anexo desde a primeira linha de dados. Usei **p50 = mediana** e **p95 = valor na posição `ceil(0,95 × n)`**, após ordenar.

**Item 6 — chamadas do wrapper**

“Total” abaixo significa últimas 20 chamadas globais; leitura/censo/sonda formam um grupo.

| Janela | n | p50 (s) | p95 (s) | Máximo (s) | rc=124 | >900 s | Fonte |
|---|---:|---:|---:|---:|---:|---:|---|
| Construtor | 20 | 715,5 | 1.630 | 1.928 | 0 | 7 | `chamadas_fim_vivo.tsv:316–395`, filtrando papel |
| Verificador | 20 | 325,5 | 594 | 656 | 0 | 0 | `chamadas_fim_vivo.tsv:335–400`, filtrando papel |
| Leitura/censo/sonda | 20 | 204,5 | 891 | 1.178 | 0 | 1 | `chamadas_fim_vivo.tsv:351–399`, filtrando grupo |
| Total global | 20 | 332 | 1.068 | 1.928 | 0 | 2 | `chamadas_fim_vivo.tsv:381–400` |
| União das três janelas | 60 | 386,5 | 1.206 | 1.928 | 0 | 8 | Mesmas seleções acima |

No anexo inteiro, os **três rc=124** são: construtor/904 s, construtor/905 s e censo/905 s; estão fora dessas janelas. (`chamadas_fim_vivo.tsv:66,130,317`)

| Suites | n | p50 (s) | p95 (s) | Máximo (s) | Fonte |
|---|---:|---:|---:|---:|---|
| Últimas 20 linhas `Ran` | 20 | 1.306,5525 | 1.638,001 | 1.788,073 | `suites_14d.tsv:507–526` |
| Últimas 20 aparentemente completas¹ | 20 | 1.316,237 | 1.638,001 | 1.788,073 | `suites_14d.tsv:500–525`, seleção por nome/tamanho |

¹ A classificação como completa usa o nome do log e o número de testes; a fase efetiva da execução não consta do anexo. [não verificado] [nao verificado]

O **timeout de 900 s do wrapper deve ser avaliado pelas chamadas Codex**. Ele envolve `codex exec`, tanto no construtor quanto nos demais papéis; o construtor recebe `-k 30`. Nesta cópia, os locais são `bin/codex_pergunta.sh:401` e `:408`, e o default permanece em `:83`. O próprio cabeçalho atribui a certificação por suite ao Code. (`bin/codex_pergunta.sh:331`)

Há uma ressalva na medida: `duracao_s` começa antes da preparação e da espera por vagas/locks. Portanto, **passar de 900 s não demonstra que o `codex exec` excedeu seu timeout**, nem revela o `CODEX_TIMEOUT` efetivamente usado naquela chamada. (`bin/codex_pergunta.sh:252,260,348–376`; valor efetivo por chamada: [não verificado])

Outro teto de **900 s** existe em **`bin/encadeado.sh:71`**: espera carimbos de conclusão de cron e sai com rc=2 quando vence; não é timeout de suite nem de Codex. (`bin/encadeado.sh:84–96`)

As contas das margens são:

| Cobertura pretendida | Conta `p95 + margem` | Teto empírico | Fonte |
|---|---|---:|---|
| Últimas 20 suites | `1.638,001 + (1.788,073 − 1.638,001)` | **1.788,073 s → 1.789 s inteiros** | `suites_14d.tsv:514,524` |
| Últimos 20 construtores | `1.630 + (1.928 − 1.630)` | **1.928 s** | `chamadas_fim_vivo.tsv:365,386` |
| Wrapper cobrindo também o maior rc=0 de todo o anexo | `1.630 + (2.806 − 1.630)` | **2.806 s** | `chamadas_fim_vivo.tsv:214,365` |

**Proposta empírica para o wrapper: 2.807 s**, isto é, `1.630 + 1.177`; a margem é `(2.806 − 1.630) + 1 s`, ficando estritamente acima do máximo registrado, com resolução inteira. Usar apenas a janela recente daria 1.929 s pelo mesmo critério. (`chamadas_fim_vivo.tsv:214,365,386`)

Isso cobre os máximos de **duração registrada**; a afirmação de que algum dos três rc=124 foi falso, ou de que esse teto evitaria falsos rc=124 futuros, permanece **[não verificado]**.

**Item 8 — “mudaram decisão”: não medido; não significa zero.**

Há **526 registros `Ran`, 140 deles FAILED**. O anexo informa totais de falhas, mas não identifica os testes; também não marca sistematicamente intermediária/final. Há possíveis cópias em diretórios `opus_final`, impedindo equiparar linhas a execuções independentes. (`suites_14d.tsv:1–526`; exemplos de cópias: `281–282,302–304,309–312`) [nao verificado]

O caso E3 mostra o limite: a suite tem sete falhas, um commit posterior diz “pós-suite: S1–S7 curados”, depois aparecem três falhas e finalmente verde. O título truncado não identifica os casos da execução nem fecha a prova pedida até a final. (`suites_14d.tsv:497–500`; `gitlog_14d.txt:59,30`) [nao verificado]

A tabela abaixo cobre **todos os agrupamentos com algum FAILED**. **NM = não medido** em ambas as colunas solicitadas. Os números entre parênteses são o censo verificável: **registros / FAILED**. Minutos são a soma bruta de `Ran ... in Xs ÷ 60`, **não minutos comprovados de intermediárias ou de economia**. Não tratei a última observação como final. (`suites_14d.tsv:1–526`)

| Obra/prefixo do anexo | Intermediárias (registros/FAILED) | Mudaram decisão | Minutos brutos | Linhas em `suites_14d.tsv` |
|---|---:|---|---:|---|
| outbox | NM (2/1) | NM | 18,640 | 1–2 |
| orfas | NM (11/3) | NM | 119,192 | 3–19, prefixo |
| col129 | NM (8/2) | NM | 93,913 | 6–21, prefixo |
| grupo1 | NM (5/3) | NM | 72,541 | 22–26 |
| justif | NM (4/2) | NM | 59,898 | 27,30–32 |
| o261n | NM (4/1) | NM | 33,198 | 34,36–38 |
| o261v | NM (4/2) | NM | 63,096 | 35,59,71,73 |
| o261/veto_a2 | NM (11/3) | NM | 166,253 | 40–50 |
| calendario_hoje | NM (6/1) | NM | 85,975 | 51–56 |
| suite_impacto — gerações distintas | NM (2/1) | NM | 49,138 | 57,226 |
| o261e, exceto f2/f2fila | NM (28/10) | NM | 327,878 | 58–97, prefixo |
| disjuntor | NM (3/1) | NM | 44,711 | 78,84,96 |
| e3status | NM (2/1) | NM | 30,745 | 98–99 |
| grade_folga | NM (5/1) | NM | 69,002 | 106,111,116,120,122 |
| f1c4emis | NM (5/1) | NM | 84,507 | 107,109,113,117,119 |
| e4_7iii | NM (3/1) | NM | 43,106 | 118,121,123 |
| v3per | NM (3/1) | NM | 39,316 | 124–126 |
| grade_juiz | NM (9/1) | NM | 150,402 | 127–149, prefixo |
| cobparc | NM (14/3) | NM | 208,564 | 142–170, prefixo |
| o261e/f2 | NM (9/2) | NM | 108,488 | 151–159 |
| cobparc_relogio | NM (6/1) | NM | 79,354 | 174–179 |
| e4cal | NM (11/2) | NM | 168,778 | 186–199, prefixo |
| mc | NM (4/1) | NM | 69,594 | 203,205,207,209 |
| e4app | NM (3/2) | NM | 36,395 | 213–215 |
| n1pna | NM (2/2) | NM | 41,197 | 216–217 |
| n1im | NM (4/1) | NM | 40,556 | 219–222 |
| jv1 | NM (11/2) | NM | 169,889 | 228–264, prefixo |
| n1alim | NM (3/1) | NM | 28,680 | 232–233,235 |
| jv3 | NM (6/2) | NM | 93,137 | 239,241,279,281–283 |
| o261e/f2fila | NM (3/1) | NM | 41,743 | 244,246,248 |
| f3_beco1 | NM (8/3) | NM | 152,549 | 256–275, prefixo |
| h12h06 | NM (6/1) | NM | 99,348 | 265–280, prefixo |
| snc2 | NM (3/1) | NM | 37,106 | 268,271,273 |
| c20_fiomudo | NM (11/2) | NM | 119,175 | 284–300, prefixo |
| h12h06b, incluindo pauta08 | NM (6/1) | NM | 103,853 | 291,297,301,303,331–332 |
| pep | NM (2/2) | NM | 34,120 | 302,304 |
| lastro2 | NM (5/2) | NM | 76,021 | 309–312,508 |
| lastro3 | NM (1/1) | NM | 17,170 | 313 |
| pep3 | NM (3/1) | NM | 26,640 | 314,316,318 |
| plantio_mais1 | NM (4/3) | NM | 63,081 | 333–334,336,339 |
| v2bugs/i2 | NM (10/2) | NM | 82,524 | 338–357, prefixo |
| ferias2x | NM (2/1) | NM | 36,728 | 342–343 |
| fio_e0_v4 — mistura E1/E2/fila/rebase | NM (83/27) | NM | 1.055,536 | 346–448, prefixo |
| cxr_tela | NM (3/1) | NM | 64,876 | 405–407 |
| ata_pausa_0811 | NM (1/1) | NM | 19,673 | 409 |
| e1fila_v22 | NM (2/1) | NM | 38,836 | 444,446 |
| par_nao_adjacente | NM (4/1) | NM | 87,109 | 449–450,453–454 |
| selos_canon | NM (4/1) | NM | 82,189 | 451–452,455–456 |
| fio_e3 — 09/11 | NM (6/3) | NM | 124,964 | 457–465, prefixo |
| FIO-E0-V4-E4-ESCRITORES | NM (4/2) | NM | 78,317 | 466–469 |
| MODO-COMUM-PASSADO-SEM-TURNO | NM (2/1) | NM | 39,387 | 470–471 |
| FIO-E0-V4-E4A-2 | NM (4/1) | NM | 79,135 | 472–473,476–477 |
| BO-0611-05 | NM (9/2) | NM | 132,060 | 478–487, prefixo |
| BO-DISPUTA-SEM-PERGUNTA | NM (6/6) | NM | 112,739 | 480,488,490–491,493,495 |
| FIO-E0-V4-E4B | NM (1/1) | NM | 3,754 | 489 |
| E4B-ALCANCE | NM (6/4) | NM | 128,479 | 492,494,504–505,515,519 |
| FIO-E0-V4-E3 — 11/11 | NM (7/4) | NM | 161,441 | 496–502 |
| UI-PAINEL | NM (3/1) | NM | 69,216 | 506–507,516 |
| COMBOIO-1556 | NM (2/1) | NM | 49,174 | 513–514 |
| LASTRO-FAIXA | NM (5/5) | NM | 98,717 | 521–525 |
| integrador | NM (1/1) | NM | 6,606 | 526 |

**Falta, para cada linha NM:** identificar a execução e sua fase, os testes que ficaram vermelhos, o commit que curou esses mesmos testes e a execução final que confirma a cura. A correspondência entre prefixos e obras também precisa fechar onde há agrupamentos mistos. [não verificado] [nao verificado]

Além disso:

- **BO-DISPUTA e LASTRO-FAIXA:** a última observação ainda é FAILED; falta a final. (`suites_14d.tsv:495,525`)
- **FIO-E4B ↔ E4B-ALCANCE:** falta comprovar a associação entre esses nomes. [não verificado]
- **jv3, c20_fiomudo, pep e lastro2:** falta distinguir cópia de log de execução independente. (`suites_14d.tsv:281–282,287,292–295,298–299,302,304,309–312`)
- **Demais prefixos:** seus registros não contêm FAILED; isso não identifica quantas execuções foram intermediárias nem comprova a final. (`suites_14d.tsv:1–526`, excluindo os agrupamentos da tabela)

Assim, **140 linhas FAILED não podem ser convertidas em 140 suites intermediárias que mudaram decisão**. O número solicitado no item 8 permanece **não medido** pelos anexos fornecidos. (`suites_14d.tsv:1–526`; vínculo causal: [não verificado])

## 12/11 11:05 · sessao f9aecd196290 · - · - · origem=Ronald (tmux)

Proposta concreta, sem escrita:

1. Use builder completo, sem reaproveitar `julgar_sem_adesao`: esse harness força ausência de batidas e testa outro caminho (`ponto/tests/test_lastro_c5_sem_banco.py:566-567`).

2. O builder completo mais curto é `montar_grade_prevista`: aceita escala/horários e batidas em memória; executa pareamento, cobertura e classificação para folga (`escala/utils.py:668`, `escala/utils.py:805-814`). Limite: esse builder não publica minutos previstos/realizados; não atribuir a ele uma prova financeira positiva (`escala/utils.py:791`). Para provar esses valores, usar o builder por turno (`escala/utils.py:1301`).

3. Crie célula em memória com ata antiga `tipo_dia='ausencia'`; rode builder com ausência `folga` integral e batida. A cobertura deve produzir `tipo_dia='folga'`, transportar ausência e limpar órfãs/células, preservando a batida de entrada externa (`escala/utils.py:1243-1254`, `escala/utils.py:1275-1277`).

4. Execute `ata_do_dia`, depois a porta real por AST: `lavrar_veredito`, `veredito_que_a_lavra_escreve`, `_status_alvo`, `_aplicar_status`; substitua somente emissão/persistência por duplos. Assert obrigatório: retorno contém `ata`, ata mudou de ausência para folga e metadados sobreviveram (`ponto/services/cartorio.py:721-768`, `ponto/portas/celula.py:912`, `ponto/portas/celula.py:951-1054`). Ler esse arquivo não exige editá-lo.

5. Para provar leitores: `grade_do_fechamento` real → `grade_da_celula` real → `por_dia_da_grade` real → bloco do fechamento que publica mapas e causa. Execute `_retidos` real de `simular_folha` com esse fechamento e `prontidao` real; isso demonstra o leitor do simulador, com integração completa/motor/TXT ainda `[nao verificado]` (`folha/export.py:329-338`, `escala/services/leitor_celula.py:397-438`, `ponto/services/fechamento.py:1014-1085`, `folha/management/commands/simular_folha.py:68-93`).

Dois pontos que precisam constar do relato:

- Batida isolada não implica `dias_trocados`: esse ramo exige órfã e condições anteriores; cobertura integral limpa órfãs (`folha/causas.py:92-106`, `escala/utils.py:1275-1277`).
- Célula `trabalha=True`, mesmo com ata relavrada para folga, fica fora do tripwire de ata fantasma (`ponto/management/commands/tripwire_celulas.py:338-342`).

## 12/11 11:05 · sessao 8f90fa80b83a · - · - · origem=Ronald (tmux)

Mapeamento somente leitura; nenhum arquivo escrito.

- Curas c13 mantidas: transporte `recusas` nas seis costuras dos builders e tratamento no overlay, preservando geometria e removendo conclusão de faixa (`escala/utils.py:807`, `:1009`, `:1018`, `:1205`, `:1212`, `:1218`, `:1872`, `:2112`, `:2364`). Harness de pacotes previamente carregados (`ponto/tests/test_lastro_c12_sem_banco.py:87`).
- RED histórico executado: `--red-c12` com os três testes `LastroC13.test_grade_recusa_nomeada_preserva_orfa_e_vizinhos_sem_inventar_faixa`, `test_recusa_nao_depende_de_cobertura_aprovada_nem_de_quem_a_venceu` e `test_canal_atravessa_dispatcher_e_costuras_dos_dois_builders`: rc 1, `FAILED (errors=6)`, por ausência do argumento `recusas` na c12 (`ponto/tests/test_lastro_c13_sem_banco.py:39`).
- Nove mutantes executados, todos rc 1: `recusa_apagada`, `recusa_gate`, `recusa_faixa`, `canal_grade`, `canal_dispatcher`, `canal_turno`, `canal_legacy`, `canal_single`, `canal_overlay` (`ponto/tests/test_lastro_c13_sem_banco.py:49`).
- `--mutante harness_c12` executa o `setUp` histórico exato. Dois testes API falharam: `busca real proibida` e `grade real proibida`, rc 1 (`ponto/tests/test_lastro_c13_sem_banco.py:26`, `:89`).

Comando comum:

```sh
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.test_lastro_c13_sem_banco --mutante NOME TESTE -v [nao verificado]
```

Selos vizinhos podem exigir delegação a `espelho_do_colab`, consumo de `orfas_grade` e proibição da grade própria no PDF, preservando as exigências financeiras (`escala/tests/test_f1c_contrato.py:42`; `relatorios/tests/test_cartao_pela_celula.py:170`).

Há incompatibilidade concreta na fixture de marca espúria: cria vínculo e batidas, sem `CelulaDia`; a tela entrega `sem_celula`, e o adaptador retorna órfãs vazias sem marcos (`relatorios/tests/test_cartao_marca_espuria.py:33`; `escala/services/leitor_celula.py:394`; `escala/utils.py:1087`). Comparar apenas vazios perderia a mordida positiva. Para preservá-la, a fixture precisa entregar tela com célula/ata/lâmpadas/órfã e manter tanto a marca quanto a supressão do turno aberto e a prova textual do PDF (`relatorios/tests/test_cartao_marca_espuria.py:55`, `:70`).

## 12/11 11:04 · sessao fdafdbeb8bfa · - · - · origem=Ronald (tmux)

Recorte recente: **298 registros, 89 FAILED, 4.691,280 minutos brutos** (`suites_14d.tsv:229–526`). As últimas 20 linhas são `507–526`.

Tabela completa enviada nas mensagens anteriores com n bruto, FAILED, soma dos tempos e referências. Pontos principais:

| Prefixo | n | FAILED | Minutos brutos | Linhas |
|---|---:|---:|---:|---|
| BO-0611-05 | 9 | 2 | 132,060 | 478–479, 481–487 |
| BO-DISPUTA-SEM-PERGUNTA | 6 | 6 | 112,739 | 480, 488, 490–491, 493, 495 |
| FIO-E0-V4-E3 | 7 | 4 | 161,441 | 496–502 |
| E4B-ALCANCE | 6 | 4 | 128,479 | 492, 494, 504–505, 515, 519 |
| LASTRO-FAIXA | 5 | 5 | 98,717 | 521–525 |
| DATA-EVENTO-CRU | 3 | 0 | 35,784 | 510, 512, 518 |
| TRIAGEM-OFFLINE-SEM-CAMERA | 3 | 0 | 74,051 | 511, 517, 520 |
| O169-FECHADA | 2 | 0 | 42,975 | 475, 503 |

**Intermediárias que mudaram decisão: não medido.** Não há fechamento demonstrado do vínculo entre execução vermelha específica, cura dos respectivos casos e final da obra. BO-DISPUTA e LASTRO terminam vermelhos no anexo (`suites_14d.tsv:495,525`).

Há registros potencialmente copiados para `opus_final`, portanto estes números contam linhas, sem demonstrar execuções independentes: `jv3:281–282`; `c20_fiomudo:287,292;290,293;294–295;298–299`; `pep:302,304`; `otimiza3:305,308;306–307`; `lastro2:309–312`.

## 12/11 11:04 · sessao 0c048614a4ba · - · - · origem=Ronald (tmux)

Recalculei e enviei a tabela dos 27 grupos com FAILED: **169 registros, 51 FAILED e 2.317,742 minutos brutos**. `suites_14d.tsv:1-228`

O trecho completo tem **228 registros e 3.181,774 minutos brutos**; os outros **59 registros** são OK e somam **864,032 minutos**. `suites_14d.tsv:1-228`

“Intermediárias” e “mudaram decisão” permanecem **não medido**; nenhuma contagem usa `n−1`. [não verificado]

## 12/11 11:03 · sessao 0c048614a4ba · - · - · origem=Ronald (tmux)

No trecho `suites_14d.tsv:1-228`:

- **228 registros**, **51 FAILED**, **56 prefixos**.
- Proxy “todas menos a última observada por prefixo”: **172 candidatas**, **2.278,789 minutos**.
- Nos **27 prefixos com FAILED**: **169 registros**, **142 candidatas**, **1.883,350 minutos**.
- **Mudaram decisão: não medido**. Não fechei um vínculo entre falha identificada na suite intermediária, commit que atribua a cura àquela falha e final comprovada; os títulos truncados não sustentam essa causalidade. [não verificado] [nao verificado]

Tabelas completas, referências de linhas e grupos sem FAILED foram enviadas nas mensagens anteriores. Os minutos são soma de `Ran ... in Xs / 60`, sem o último registro de cada agrupamento; não representam tempo causal de cura. `suites_14d.tsv:1-228`

Cuidados para consolidar:

- `comboio`, `suite_impacto`, `otimiza3` e `jv1` continuam no trecho recente; agregar antes de subtrair a última observação. `suites_14d.tsv:100,227-229`
- `suite_impacto:57,226` contém duas gerações distintas; a associação a uma mesma obra não está demonstrada. [não verificado]
- A última observação é vermelha em `orfas`, `n1pna` e `suite_impacto`; não a chamar de final comprovada. `suites_14d.tsv:19,217,226`
- Exemplos antigos de cura que pareciam promissores citam reprovação do **opus**, ou vermelhos anteriores à suite, e não fecham a prova solicitada. [não verificado] [nao verificado]

## 12/11 11:03 · sessao 6fd90b5b6a65 · - · - · origem=Ronald (tmux)

Método: linhas numeradas a partir da primeira linha de dados; p50 = mediana; p95 = ordem `ceil(0,95 × n)`.

| Recorte | n | p50 s | p95 s | max s | rc124 | >900 s | Fonte |
|---|---:|---:|---:|---:|---:|---:|---|
| Últimas20 construtor |20|715,5|1630|1928|0|7|chamadas_fim_vivo.tsv:316–395, filtrando construtor |
| Últimas20 verificador |20|325,5|594|656|0|0|chamadas_fim_vivo.tsv:335–400, filtrando verificador |
| Últimas20 leitura/censo/sonda |20|204,5|891|1178|0|1|chamadas_fim_vivo.tsv:351–399, filtrando o grupo |
| Últimas20 globais |20|332|1068|1928|0|2|chamadas_fim_vivo.tsv:381–400 |
| União das três janelas |60|386,5|1206|1928|0|8|mesmas seleções |

Linhas exatas selecionadas:

- Construtor:316,333,342,350,352,353,356,359,360,365,366,368,371,374,375,378,386,391,393,395.
- Verificador:335,336,337,338,339,341,354,361,367,369,373,376,379,380,388,389,390,392,394,400.
- Grupo leitura/censo/sonda:351,355,357,358,362,363,364,370,372,377,381,382,383,384,385,387,396,397,398,399.

O anexo inteiro tem400 chamadas; seus3 `rc=124` são construtor904s, construtor905s e censo905s, todos fora das janelas acima. (chamadas_fim_vivo.tsv:66,130,317)

Últimas20 linhas `Ran`: p50=1306,5525s; p95=1638,001s; max=1788,073s;15 passaram de900s. (suites_14d.tsv:507–526)

Se considerar apenas completas, últimas20 linhas são500–507,509,511,513–520,524–525: p50=1316,237s; p95/max permanecem1638,001/1788,073s. A classificação como completa usa nomes e tamanho observado dos testes. (suites_14d.tsv:500–525) [nao verificado]

Margens mínimas empíricas:

- Suite: `1788,073 − 1638,001 =150,072s`; teto `1638,001+150,072=1788,073s`, arredondável para1789s inteiros. (suites_14d.tsv:514,524) [nao verificado]
- Wrapper/construtor, janela20: `1928−1630=298s`; teto1928s. (chamadas_fim_vivo.tsv:365,386)
- Wrapper/construtor, cobrindo todo o histórico fornecido: max comrc0=2806s; `2806−1630=1176s`; teto2806s. (chamadas_fim_vivo.tsv:214,365)

O fato de um124 ter sido “falso” e qualquer garantia para chamadas futuras são [não verificado].

## 12/11 11:01 · sessao e15566ed2acf · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO · construtor · origem=Code

Entrega parcial em [resposta_c3.txt](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-090745/app/docs/resposta_c3.txt:1), com correções, comandos RED/GREEN, mutantes e inventário.

[66 testes passaram; dois REDs permanecem](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-090745/app/docs/resposta_c3.txt:5), dependentes da porta proibida. Ruff indisponível. Sem commit, respeitando a proibição inicial; limitações registradas no arquivo. [nao verificado]

## 12/11 11:00 · sessao 383f5335fd91 · LASTRO-FAIXA-SEM-DEFAULT · leitura · origem=Code

A divergência das órfãs está demonstrada: o selo exige `orfas_grade=[]` na tela e `batida.orfa=True` no PDF. Já a afirmação de que o rótulo nunca chega pelos dois primeiros sítios do espelho precisa de ressalva: a grade falha, mas a composição posterior ainda pode produzir `palavra_dia` (`app/ponto/tests/test_lastro_c11_sem_banco.py:380-389`; `app/ponto/services/espelho.py:572-574`, `:653-654`, `:880`).

Identidade da cópia como HEAD c13/hash informado: **[nao verificado]**. Os testes abaixo foram lidos; execução e resultado nesta análise: **[nao verificado]**. Ocorrências e quantidades em produção além dos anexos: **[nao verificado]**. [nao verificado]

“Sem faixa” significa código ausente de `_FAIXA_DE_TIPO`, não um campo vazio de `TipoAusencia`. A porta atual recusa criar esse código, mesmo inativo, e recusa ativá-lo; permite desativar um legado. As linhas abaixo tratam de ausência existente que alcança a leitura (`app/ponto/precedencia.py:52-110`; `app/ponto/services/ausencia.py:1309-1322`).

TELA é a apresentação HTML: o handler chama `espelho_do_colab`, depois `montar_semanas`. ESPELHO, na terceira matriz, é esse serviço e seus caminhos internos; os sítios do laudo estão em `services/espelho.py` (`app/ponto/views.py:157-158`, `:183-197`; `app/ponto/services/espelho.py:492-574`).

**Matriz TELA — apresentação do dia na UI**

| Estado de entrada — cadastro + ata/célula | Juiz consultado — função:linha | Operação | Persistido | Retorno na UI | Prova existente |
|---|---|---|---|---|---|
| Dia válido, faixa reconhecida, célula/ata legível e status lavrado | `grade_da_celula`, chamada em `app/ponto/services/espelho.py:522`; A2 `fatos_do_periodo`, `:653`; D4 `status_dos_dias_lavrados`, `:170` | Monta realizado; depois transporta status/rótulo do D4 (`app/ponto/services/espelho.py:693-699`, `:170-174`) | Nada; altera os dicionários apresentados (`app/ponto/services/espelho.py:174`) | Faixa usa `status_rotulo` no `title`; cartão mostra previsto e marcações (`app/templates/ponto/espelho.html:151`, `:218-246`) | `app/ponto/tests/test_e4_esp_espelho_le_o_d4.py::test_RED_testemunha_le_status_e_rotulo_do_D4_em_todo_dia`, `:64-79` |
| Ausência sem faixa; dia passado, sem batida e sem status lavrado; montagem alcança A2 | A2 com `recusas`, `app/ponto/services/espelho.py:653-654`; D4, `app/ponto/status_dia.py:311-318` | Transporta recusa; escreve palavra vermelha, veredito vazio; D4 devolve `sem_certidao` com o rótulo (`app/ponto/services/espelho.py:880`, `:339-341`; `app/ponto/status_dia.py:318`) | Nada; campos de apresentação em memória (`app/ponto/services/espelho.py:340-341`) | `"Tipo sem faixa: <codigo>"` chega ao **title da faixa**, por `status_rotulo`. O template usa esse campo; não imprime `palavra_dia` nesse sítio (`app/templates/ponto/espelho.html:151`) | `app/ponto/tests/test_lastro_c11_sem_banco.py::test_RED_espelho_montar_semanas_dia_passado_sem_batida_sem_lavra`, `:123-136`. Não cobre handler HTML inteiro |
| Mesma ausência sem faixa, com batida ou status antigo lavrado | A2 da palavra, `app/ponto/services/espelho.py:653`; D4 retorna antes da consulta às recusas (`app/ponto/status_dia.py:283-298`) | Pode preencher `palavra_dia`; a faixa mantém andamento, status lavrado ou `sem_certidao` genérico (`app/ponto/status_dia.py:283-298`) | Nada nesses leitores (`app/ponto/services/espelho.py:340`; `app/ponto/status_dia.py:284-297`) | **Rótulo sem-faixa não garantido na UI**: o template lê o `status_rotulo` escolhido pelo D4 (`app/templates/ponto/espelho.html:151`) | Nenhuma específica sem-faixa + batida entre as provas abertas **[nao verificado]** |
| Dia com órfã; célula/ata e marcos disponíveis | `_orfas_da_ata`, `app/escala/services/leitor_celula.py:180`; `montar_realizado_grade`, chamada em `app/ponto/services/espelho.py:698` | Lê `ata.orfas`, ou `?` pela contagem antiga; publica `orfas_grade` (`app/escala/services/leitor_celula.py:187-190`, `:420`; `app/ponto/services/espelho.py:853`) | Nada; lista em memória (`app/ponto/services/espelho.py:853`) | Com `realizado_grade`, imprime órfãs como `• HH:MM`; sem realizado, imprime batidas cruas (`app/templates/ponto/espelho.html:231-246`) | `app/escala/tests/test_t41_leitor_celula.py::test_orfas_chegam_com_hora_real_nao_interrogacao`, `:47-71`; cobre transporte, não HTML completo |
| Dia com órfã, sem célula/marcos | `montar_realizado_grade`, `app/escala/utils.py:1081` | Guarda retorna `[], []`; tela recebe `orfas_grade=[]` (`app/escala/utils.py:1087-1088`; `app/ponto/services/espelho.py:698-701`) | Nada (`app/ponto/services/espelho.py:853`) | Batida crua, sem identificação de órfã nesse ramo (`app/templates/ponto/espelho.html:243-246`) | `app/ponto/tests/test_lastro_c11_sem_banco.py::test_PDF_igual_tela_orfa_sem_celula_dia_valido_coletor_inteiro`, `:380-389`; montagem da tela recortada por AST, `:306-326` |
| Dia relavrado ausência → folga, com batida | `grade_da_celula` prefere `ata.tipo_dia`, `app/escala/services/leitor_celula.py:397-398`; D4 em `app/ponto/services/espelho.py:170` | Define `dia.folga` pela grade e conserva batidas; palavra usa A2/cobertura separadamente (`app/ponto/services/espelho.py:688`, `:775-779`, `:879-882`) | Nada no leitor | Pode mostrar badge `Folga` e batidas. Palavra não se deduz só de `ata.tipo_dia`: cobertura tem prioridade; faixa lê D4 (`app/templates/ponto/espelho.html:171`, `:231-246`; `app/ponto/services/dia_decidido.py:363-372`) | Nenhuma da sequência relavra → UI entre as provas abertas **[nao verificado]** |

**Matriz PDF — `_coletar_dados_espelho`**

| Estado de entrada — cadastro + ata/célula | Juiz consultado — função:linha | Operação | Persistido | Retorno no PDF | Prova existente |
|---|---|---|---|---|---|
| Dia válido com faixa; exemplo de ausência aprovada `reciclagem` | `faixa_de`, `app/ponto/precedencia.py:105`; tela `espelho_do_colab`, chamada em `app/relatorios/pdf_espelho.py:479` | Usa dias da tela e reaplica compositor comum (`app/relatorios/pdf_espelho.py:563`, `:598-600`) | Nada de cadastro/ata pelo coletor; mutações em memória (`app/relatorios/pdf_espelho.py:567-600`) | `palavra_dia` na coluna Motivo (`app/relatorios/pdf_espelho.py:885`) | `app/relatorios/tests/test_rotulo_ausencia_pdf.py::test_05_tipo_novo_criado_pela_porta_rotula_sozinho`, `:52` |
| Ausência sem faixa; chamada da tela consegue terminar | Grade própria com `recusas={}`, `app/relatorios/pdf_espelho.py:474`; A2, `app/ponto/precedencia.py:419-427` | Grade entrega recusa e `tipo_dia=None`; PDF **sobrescreve** `_recusa_do_dia` com a recusa dessa grade e aplica palavra (`app/escala/utils.py:1218-1221`; `app/relatorios/pdf_espelho.py:567`, `:598`) | Nada de domínio; alteração do dia em memória (`app/relatorios/pdf_espelho.py:567`) | `"Tipo sem faixa: <codigo>"`, veredito vazio; pode abortar antes se a chamada da tela propagar exceção, conforme terceira matriz (`app/ponto/services/espelho.py:339-341`, `:563`; `app/relatorios/pdf_espelho.py:479`) | `app/ponto/tests/test_lastro_c13_sem_banco.py::test_PDF_le_rotulo_recusado_do_contrato_sem_concluir_precedencia`, `:154-162`; tela é substituída na fixture (`app/ponto/tests/test_lastro_c11_sem_banco.py:325-326`) |
| Dia com órfã, inclusive sem célula na tela | **Grade própria** `grade_espelho_janela`, `app/relatorios/pdf_espelho.py:474` | Indexa `grade.dias[].orfas` por data; une HH:MM; marca `batida.orfa`; usa conjunto para suprimir turno aberto espúrio (`app/relatorios/pdf_espelho.py:475-481`, `:517`, `:571`) | Nada de domínio; `batida.orfa` transitório (`app/relatorios/pdf_espelho.py:571`) | Marcação cinza com asterisco; alertas correspondentes omitidos (`app/relatorios/pdf_espelho.py:838-847`, `:926`) | `app/ponto/tests/test_lastro_c11_sem_banco.py::test_PDF_igual_tela_orfa_sem_celula_dia_valido_coletor_inteiro`, `:380-389`, exige explicitamente a divergência em `:383-384` |
| Dia relavrado ausência → folga, com batida | Dias da tela, `app/relatorios/pdf_espelho.py:563`; órfãs ainda pela grade própria, `:480` | Desenha o dia recebido, mas classifica órfãs pela segunda fonte (`app/relatorios/pdf_espelho.py:567-571`) | Nada de relavra pelo coletor | Palavra segue compositor/cobertura, não apenas `ata.tipo_dia`; resultado concreto **[nao verificado]** (`app/ponto/services/dia_decidido.py:363-372`) | Nenhuma da sequência relavra → PDF entre as provas abertas **[nao verificado]** |

“Nada” acima é do coletor e do estado do dia. A emissão mensal auditável chama `gerar_pdf_auditavel`, que cria `RegistroPDFAuditavel`; a emissão informacional retorna bytes em memória (`app/relatorios/pdf_espelho.py:1065`, `:1018-1033`; `app/relatorios/pdf_base.py:152-165`).

**Matriz ESPELHO — serviço e os três sítios do laudo**

| Estado de entrada — cadastro + ata/célula | Juiz consultado — função:linha | Operação | Persistido | Retorno ou exceção | Prova existente |
|---|---|---|---|---|---|
| Dia válido com faixa; ata legível | `grade_da_celula` + `remapear_na_regua`, `app/ponto/services/espelho.py:522-523`; A2, `:653` | Lê célula, monta dias, aplica palavra e retorna contrato (`app/ponto/services/espelho.py:693-699`, `:1301-1306`) | Nada nesses leitores | `dias`, `resumo` e demais campos do contrato (`app/ponto/services/espelho.py:1304-1306`) | `app/ponto/tests/test_caracterizacao_espelho.py::test_contrato_context`, `:53-90`; cobre contexto da rota |
| Ausência sem faixa; chega à composição posterior da palavra | `fatos_do_periodo(...recusas=...)`, `app/ponto/services/espelho.py:653-654` | Recebe recusa por data, sem concluir precedência; transporta rótulo (`app/ponto/precedencia.py:420-427`; `app/ponto/services/espelho.py:880`) | Nada | Palavra sem-faixa, veredito vazio; exceção fora da resposta por data é relançada (`app/ponto/services/espelho.py:339-341`, `:656-658`) | `app/ponto/tests/test_lastro_c10_sem_banco.py::test_C_RED_rotulo_vem_da_recusa_por_dia_sem_concluir_precedencia`, `:269-279`; executa recorte AST posterior à grade (`:236-267`) |
| **Sítio 1: `_pela_grade`**, ausência sem faixa que faça o builder consultar A2 | `_contrato` = `grade_espelho_janela`, **sem `recusas`**, `app/ponto/services/espelho.py:488`, `:492-498` | Não oferece destinatário da recusa. A2 relança quando `recusas is None` (`app/ponto/precedencia.py:420-422`) | Nada | Helper propaga `TipoSemFaixa`; destino depende do chamador, nas duas linhas seguintes | Nenhuma específica desse helper/caminho entre as provas abertas **[nao verificado]** |
| **Sítio 2: pré-apuração** chama `_pela_grade`; builder levanta `TipoSemFaixa` | `_pela_grade`, `app/ponto/services/espelho.py:533-534` | `except Exception` captura e zera `_grade_por_data` (`app/ponto/services/espelho.py:572-574`) | Nada | Segue sem grade. **Pode ainda produzir palavra sem-faixa depois**, pela chamada A2 com recusas (`app/ponto/services/espelho.py:653-654`, `:880`, `:1301`) | Nenhuma da execução completa desse ramo entre as provas abertas **[nao verificado]** |
| **Sítio 3: handler `AtaAgregadaSemMarcos`** chama builder, que levanta `TipoSemFaixa` | `_pela_grade`, dentro do handler, `app/ponto/services/espelho.py:540-563` | Exceção nasce dentro do handler; o `except Exception` irmão não a captura (`app/ponto/services/espelho.py:540-574`) | Nada nesses leitores | Propaga antes da composição posterior. A rota não envolve `espelho_do_colab` em captura: caminho de erro/500; ocorrência real **[nao verificado]** (`app/ponto/views.py:157-158`) | Nenhuma desse caminho completo entre as provas abertas **[nao verificado]** |
| Dia com órfã | `grade_da_celula` na competência; builder antes dela ou na degradação (`app/ponto/services/espelho.py:521-565`) | Adaptador lê a fonte selecionada e publica `orfas_grade` (`app/ponto/services/espelho.py:698-701`, `:853`) | Nada | Órfãs dessa fonte; sem dia/marcos, lista vazia (`app/escala/utils.py:1087-1088`, `:1113`) | `app/ponto/tests/test_lastro_c11_sem_banco.py::test_PDF_igual_tela_orfa_sem_celula_dia_valido_coletor_inteiro`, `:380-389`; alcance limitado já indicado |
| Dia relavrado ausência → folga, com batida | `grade_da_celula`, `app/escala/services/leitor_celula.py:397-398` | Lê tipo da ata, define `folga`, mantém batidas; não relavra (`app/ponto/services/espelho.py:688`, `:775-779`) | Nada | Contrato com `folga`; palavra continua dependente de A2/cobertura (`app/ponto/services/espelho.py:879-882`; `app/ponto/services/dia_decidido.py:363-372`) | `app/ponto/tests/test_lastro_c5_sem_banco.py::test_B1_grade_ata_leitor_folha_preserva_tipo_link_e_dinheiro`, `:342-360`, cobre cadeia sintética; não serviço completo |

Depois das matrizes:

1. **PDF ler a tela.** A tela chama `montar_realizado_grade`, que publica o resultado em `orfas_grade`; o PDF já chama `espelho_do_colab`. Portanto, a fonte a ler é `d['orfas_grade']` desse retorno (`app/ponto/services/espelho.py:698-699`, `:853`; `app/relatorios/pdf_espelho.py:479`). Em `pdf_espelho.py:474-481`, desaparecem chamada/índice da grade própria e o mapa passa a vir dos dias da tela. Também é necessário retirar a dependência de `_dias_grade` para a recusa em `:567`, preservando `_recusa_do_dia` que a tela entrega em `app/ponto/services/espelho.py:880`. A união HH:MM em `:481` e seus leitores `:517`/`:571` também precisam consumir essa fonte; colisão real entre datas **[nao verificado]**. O selo deve inverter a exigência de `batida.orfa=True` e deixar de exigir chamada da grade própria; a igualdade em `:382` compara objetos que o coletor muta (`app/ponto/tests/test_lastro_c11_sem_banco.py:382-389`; `app/relatorios/pdf_espelho.py:563-571`). Isso segue L-037/L-038 já existentes (`app/docs/LEIS.md:85-86`).

2. **Onde o rótulo não chega.** Nos três sítios falta o canal `recusas` do builder. No helper ele propaga; na pré-apuração vira falha genérica/grade vazia; no handler agregado escapa e interrompe a leitura (`app/ponto/services/espelho.py:492-498`, `:533-574`). Porém, **“não chega ao resultado final” não se sustenta como afirmação geral para os dois primeiros**: a composição posterior pode recuperar a recusa e escrever a palavra (`app/ponto/services/espelho.py:653-654`, `:880`, `:1301`). Visibilidade na UI continua condicionada ao D4 e ao template, conforme matriz TELA (`app/ponto/status_dia.py:283-318`; `app/templates/ponto/espelho.html:151`).

3. **`simular_folha` e item (d).**
   - Comando normal, `--lido`, `--recalcular` e `--comparar` **não relavram `CelulaDia`**: leem atas existentes; `--recalcular` grava fechamento/DiaPago (`app/folha/management/commands/simular_folha.py:120-134`, `:243-251`; `app/ponto/services/fechamento.py:1014-1015`, `:1142-1149`, `:1196-1206`; `app/folha/export.py:329-338`).
   - Shell `foto`/`par` sem `--antes` também não acrescenta relavra. Com `--antes`, pode passar por ela conforme o comando fornecido; em `par`, somente na foto “depois” (`bin/simular_folha.sh:65-74`, `:94-99`).
   - Exemplo de caminho disponível: `--antes "processar_cartorio --empresa N --apply --forcar"` → `julgar_colab` → builder → ata → lavra. Execução desse caminho na c13 **[nao verificado]** (`app/ponto/management/commands/processar_cartorio.py:31-34`, `:54-71`; `app/ponto/services/cartorio.py:1163-1172`, `:1846-1870`).
   - Logo, recalcular sobre atas prontas não demonstra o item (d). Há teste de preservação da ata antiga até fato novo, mas não prova de `simular_folha` atravessando essa transição (`app/ponto/tests/test_lastro_c5_sem_banco.py::test_P5_ata_antiga_e_FOTO_sem_bump_ate_fato_novo`, `:692-712`; `app/folha/tests/test_o48_passo4_simular_folha_lido.py:87-112`, `:159-245`).

Escritores e leitores dos campos citados:

| Campo | Quem escreve | Quem lê |
|---|---|---|
| Cadastro `codigo`, `efeito`, `ativo` | UI chama `gravar_tipo_ausencia`; serviço valida faixa e salva/cria (`app/ponto/views_tipos_ausencia.py:64-98`; `app/ponto/services/ausencia.py:1301-1322`) | Catálogo lê tabela; `efeito_vigente` inclui inativos. Faixa vem do mapa de código, não do efeito (`app/ponto/catalogo/ausencias.py:249-252`, `:298-300`; `app/ponto/precedencia.py:105-110`) |
| `recusas[data]`, `erro.codigo/rotulo` | `TipoSemFaixa.__init__` e A2 (`app/ponto/precedencia.py:99-102`, `:420-423`) | Overlay, montador da palavra e D4 (`app/escala/utils.py:1218-1220`; `app/ponto/services/espelho.py:880`; `app/ponto/status_dia.py:317-318`) |
| Grade `recusa`, `tipo_dia`, `ausencia_tipo/id/label`, `cobertura_parcial` | `_marcar_ausencias`; **a classificação para folga está aqui** (`app/escala/utils.py:1218-1220`, `:1243-1254`) | Cartório compõe ata; leitores posteriores recebem cópia (`app/ponto/services/cartorio.py:729`, `:757-768`; `app/escala/services/leitor_celula.py:397-438`) |
| `CelulaDia.ata`, incluindo tipo, lâmpadas, contagens, órfãs e metadados da ausência | `ata_do_dia` compõe; porta `lavrar_veredito` atribui; `carimbar` persiste o conjunto (`app/ponto/services/cartorio.py:727-768`, `:1847-1870`; `app/ponto/portas/celula.py:1032-1038`, `:1065-1072`) | `grade_da_celula` e `_orfas_da_ata` (`app/escala/services/leitor_celula.py:379-438`, `:187-190`) |
| `CelulaDia.trabalha` | Portas de geração/edição; a lavra citada altera ata/veredito/status (`app/ponto/portas/celula.py:851-857`, `:609`, `:1032-1038`) | Leitor usa como alternativa ao tipo da ata; tripwire pré-filtra por ele (`app/escala/services/leitor_celula.py:397-398`; `app/ponto/management/commands/tripwire_celulas.py:338-340`) |
| `orfas_grade`, `realizado_grade`, `folga` | Montador do espelho (`app/ponto/services/espelho.py:779-781`, `:853`) | Template HTML (`app/templates/ponto/espelho.html:171`, `:231-246`). PDF hoje lê outra fonte para órfãs (`app/relatorios/pdf_espelho.py:480`) |
| `_recusa_do_dia` → `palavra_dia`, `veredito_dia`, `cor_dia` | Espelho transporta recusa; PDF hoje sobrescreve pela grade própria; compositor escreve apresentação (`app/ponto/services/espelho.py:880`, `:339-341`; `app/relatorios/pdf_espelho.py:567`) | PDF desenha palavra; faixa HTML lê `status_rotulo` (`app/relatorios/pdf_espelho.py:885`; `app/templates/ponto/espelho.html:151`) |
| `status_dia/status_rotulo` persistentes → `status/status_rotulo` apresentados | Porta aplica status na lavra; `montar_semanas` copia resposta D4 (`app/ponto/portas/celula.py:968-978`, `:1038`; `app/ponto/services/espelho.py:170-174`) | D4 lê lavrado; template lê apresentação (`app/ponto/status_dia.py:290-293`; `app/templates/ponto/espelho.html:151`) |
| `batida.orfa` | PDF atribui em memória (`app/relatorios/pdf_espelho.py:571`) | Desenho das marcações e alertas (`app/relatorios/pdf_espelho.py:838-847`, `:926`) |
| `impressao` | `carimbar` (`app/ponto/portas/celula.py:1065-1072`) | Cartório pode pular julgamento sem fato novo e sem força (`app/ponto/services/cartorio.py:1256-1262`, `:1405-1411`) |

Os dois leitores de `tipo_dia=='folga'` com batida não fecham o caso da relavra:

- `folha/causas.py:100` exige **órfãs**, além das condições anteriores de previsão/furo; pode produzir `dias_trocados` sem consultar `ausencia_tipo`. Ter batida, sozinho, não satisfaz esse predicado. O teste existente usa folga sintética, sem relavra (`app/folha/causas.py:58-106`; `app/folha/tests/test_causas.py::test_dias_trocados_nao_vira_falta`, `:81-85`).
- `tripwire_celulas.py:342` procura ata folga sem lâmpadas e com contagem positiva, após filtrar `trabalha=False/NULL`; não pergunta por batida/órfã. Uma célula `trabalha=True` com ata relavrada para folga fica fora desse queryset. As provas abertas têm `n_orfas=0` (`app/ponto/management/commands/tripwire_celulas.py:338-349`; `app/ponto/tests/test_tripwire_ata_fantasma.py:47-71`).

Por fim, a referência do aval precisa ser precisa: `cartorio.py:763-768` **transporta metadados**, enquanto `escala/utils.py:1243-1244` classifica o dia. Na cobertura integral, o overlay também limpa células e órfãs; portanto, “relavra para folga com batida” não implica conservar `orfas` para acionar `dias_trocados` (`app/ponto/services/cartorio.py:763-768`; `app/escala/utils.py:1243-1247`, `:1275-1277`).

## 12/11 10:59 · sessao 327b56f5872e · PUSH-SEM-REGRAVAR · verificador · origem=Code

1. **ok — a regra (c) saiu inteira do julgamento.** Cada commit é julgado pela própria mensagem e arquivos; depois disso, só a exceção nominal pode apagar sua falha. Não restaram `primeira_linha`, `pouso_de`, `julgar_pouso` ou `POR_POUSO` no selo. O tratamento de **hunk próprio do merge** permanece, mas não dispensa os commits dos pais ([test_lei_akita.sh:438–467](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:438)).

2. **ok — exceção restrita ao SHA inteiro solicitado.** A lista contém somente `c2162edda843b4adea87885762cdfc451c01167a`, com o motivo literal do aval ([test_lei_akita.sh:369–372](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:369)). A condição exige simultaneamente falha e pertinência exata: `if d and h in EXCECAO_NOMINAL`. Não compara prefixos; o abreviamento acontece apenas no registro da saída. Outro SHA, inclusive merge, não entra. Duplicatas saem antes desse ramo, como duplicatas ([test_lei_akita.sh:442–464](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:442)).

3. **ok funcional; FILA na cobertura da integração.** A catraca rejeita dois itens, outro SHA, zero itens e motivo vazio ou composto de espaços ([test_lei_akita.sh:373–378](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:373)). Roda **antes de chamar o enumerador**, embora depois da leitura do vocabulário ([test_lei_akita.sh:129](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:129), [test_lei_akita.sh:932–935](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:932)).

   O erro interrompe o Python sem `PADRAO_FIM`; o shell chama `_falha` e termina com **rc 1**, não apenas impressão. Conferi essa ligação isoladamente, em memória: os cinco casos inválidos deram rc 1; a lista real deu rc 0 ([test_lei_akita.sh:26](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:26), [test_lei_akita.sh:943–961](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:943)).

   **FILA:** o MORDE prova os quatro retornos de rejeição, mas não testa retirar a chamada produtiva da catraca. Esse mutante não seria acusado pelos casos novos ([test_lei_akita.sh:913–918](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:913), [test_lei_akita.sh:932–934](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:932)).

4. **ok por inspeção para restaurar integralmente (c); FILA para volta+ff literal.** O caso `juiz unico` cria passo inválido na raia e merge válido, mas exige que o passo continue vermelho: restaurar (c) retiraria esse passo das falhas e dispararia `MORDE_FALHOU` ([test_lei_akita.sh:896–911](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:896)).

   Os casos de merge inventado e `n/a` são **vácuos para matar essa restauração**, pois o ramo antigo já não dispensava nesses cenários; a condição antiga está apenas no anexo fornecido **[nao verificado]**. O teste novo constrói pouso `raia→base` com `--no-ff`, não `main→raia` seguido de ff ([test_lei_akita.sh:904–907](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:904)). Execução integral do mutante: **[nao verificado]**.

5. **ok — escritores e leitores identificados.**

   | Valor | Quem escreve | Quem lê |
   |---|---|---|
   | `EXCECAO_NOMINAL` | `bin/tests/test_lei_akita.sh:369–371` | `:462–463`, `:914–915`, `:932` |
   | `EXCECAO_CATRACA` | `bin/tests/test_lei_akita.sh:372` | `:373–375`, `:913` |
   | `POR_EXCECAO` | `bin/tests/test_lei_akita.sh:399`, `:463` | `:909–911`, `:940–941` |
   | `PADRAO_EXCECAO` | `bin/tests/test_lei_akita.sh:941` | apresentação `:949`; contagem no OK `:958` |
   | Frase final de OK | `bin/tests/test_lei_akita.sh:958` | chamadas externas abertas usam rc: `bin/pre-push.sh:38`, `bin/regua.sh:140–143` |

   No censo literal da cópia, não apareceu leitor órfão de `PADRAO_POUSO`; “pelo pouso” fora do selo aparece como comentário de outra obra ([test_esmeril_celula_censo.sh:10](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_esmeril_celula_censo.sh:10)). O pre-push chama o selo antes do atalho do arsenal ([pre-push.sh:38–43](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/pre-push.sh:38)). Hook efetivamente instalado e leitores dinâmicos: **[nao verificado]**.

6. **FILA — mordida incompleta da ligação produtiva da catraca**, conforme item 3 ([test_lei_akita.sh:913–918](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:913)). **ok no delta** quanto a derivação paralela: produção e MORDE chamam a mesma função; não há segundo predicado ([test_lei_akita.sh:914–918](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:914), [test_lei_akita.sh:932](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:932)). Não identifiquei campo com dois significados nem leitura de relógio nos trechos novos ([test_lei_akita.sh:369–378](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:369), [test_lei_akita.sh:940–958](/home/ronald/codex_ws/397202a2b4114c74ab846ea5f66265eba6d34b93/bin/tests/test_lei_akita.sh:940)).

**Pouso: [nao verificado].** Resultado v6 com **0 FALHA no main real**, selo completo, MORDE completo e identidade Git do hash informado: **[nao verificado]**. [nao verificado]

## 12/11 10:57 · sessao 395d7244505f · - · - · origem=Ronald (tmux)

TELA é `app/ponto/views.py::espelho` → `services.espelho.espelho_do_colab` (`app/ponto/views.py:157-159`), depois `montar_semanas` (`app/ponto/views.py:183-184`), renderizando `ponto/espelho.html` (`app/ponto/views.py:192-197`).

| Entrada | Juiz consultado | Operação | Persistido | Retorno UI | Prova existente |
|---|---|---|---|---|---|
| Dia normal de trabalho, faixa reconhecida, ata/status lavrado | `grade_da_celula` (`app/ponto/services/espelho.py:522`); `fatos_do_periodo` (`app/ponto/services/espelho.py:653`); D4 `status_dos_dias_lavrados` (`app/ponto/services/espelho.py:170`) | Transporta ata/marcos; monta previsto e realizado (`app/ponto/services/espelho.py:693-699`); aplica palavra (`app/ponto/services/espelho.py:1301`); lê status/rotulo (`app/ponto/services/espelho.py:174`) | Nada nessas operações; muta dicionários em memória (`app/ponto/services/espelho.py:773`, `app/ponto/services/espelho.py:340`, `app/ponto/services/espelho.py:174`) | Faixa usa status e `status_rotulo` no title (`app/templates/ponto/espelho.html:151`); cartões mostram previsto/marcações (`app/templates/ponto/espelho.html:218-246`) | `app/ponto/tests/test_caracterizacao_espelho.py::test_contrato_context` confere HTTP 200, D4, turnos e batidas (`:53-55`, `:84-90`); `app/ponto/tests/test_e4_esp_espelho_le_o_d4.py::test_RED_testemunha_le_status_e_rotulo_do_D4_em_todo_dia` (`:64-79`). |
| Ausência cujo tipo não tem faixa; dia passado sem batida e sem status lavrado | A2 `fatos_do_periodo(...recusas=...)` (`app/ponto/services/espelho.py:653-654`); D4 (`app/ponto/status_dia.py:311-318`) | Transporta `erro.rotulo` para `_recusa_do_dia` (`app/ponto/services/espelho.py:880`); aplica `veredito_dia=''`, `palavra_dia=rotulo`, cor vermelha (`app/ponto/services/espelho.py:339-341`); D4 devolve `sem_certidao` com rótulo da recusa (`app/ponto/status_dia.py:317-318`) | Nada; alterações de dicionário (`app/ponto/services/espelho.py:340`, `app/ponto/status_dia.py:318`) | O texto **não é impresso como `palavra_dia` pelo template ponto/espelho**; chega no `title="{{ d.status_rotulo }}"` da faixa (`app/templates/ponto/espelho.html:151`). Com ausência, card mostra chip próprio de ausência (`app/templates/ponto/espelho.html:191`) | `app/ponto/tests/test_lastro_c10_sem_banco.py::test_C_RED_rotulo_vem_da_recusa_por_dia_sem_concluir_precedencia` (`:269-279`), recorte AST (`:236-267`); `app/ponto/tests/test_lastro_c11_sem_banco.py::test_RED_espelho_montar_semanas_dia_passado_sem_batida_sem_lavra` (`:123-136`). Não provam o handler UI completo. |
| Mesmo tipo sem faixa, **com batida**, ou com status antigo lavrado | Mesmo A2 da palavra (`app/ponto/services/espelho.py:653-654`), mas D4 passa por retornos anteriores (`app/ponto/status_dia.py:283-298`) | `palavra_dia` pode receber recusa; D4 **não consulta recusas** se turno vivo, andamento, status lavrado ou batida sem lavra (`app/ponto/status_dia.py:283-298`) | Nada nessas operações (`app/ponto/services/espelho.py:340`, `app/ponto/status_dia.py:284-297`) | Turno vivo: `aberto`; status lavrado: preserva status/rotulo; batida encerrada sem lavra: `sem_certidao` genérico (`app/ponto/status_dia.py:283-298`). Template lê esse `status_rotulo`, não `palavra_dia` (`app/templates/ponto/espelho.html:151`) | Nenhuma prova específica sem-faixa+batida aberta. Teste geral status lavrado com batida: `app/ponto/tests/test_e4_esp_espelho_le_o_d4.py::test_RED_celula_lavrada_irregular_com_batida_na_tela_e_irregular` (`:46-54`). |
| Batida órfã na competência, com célula/ata | `grade_da_celula` (`app/ponto/services/espelho.py:522`), `_orfas_da_ata` (`app/escala/services/leitor_celula.py:180`), `montar_realizado_grade` (`app/ponto/services/espelho.py:698`) | Leitor transporta `ata.orfas` ou `['?']*n_orfas` (`app/escala/services/leitor_celula.py:187-190`, `:420`); adaptador devolve cópia de `gd.orfas` (`app/escala/utils.py:1113`); escreve `orfas_grade` no dict (`app/ponto/services/espelho.py:853`) | Nada; `orfas_grade` é memória (`app/ponto/services/espelho.py:853`) | Se `realizado_grade` tiver conteúdo, órfãs aparecem como `• HH:MM` (`app/templates/ponto/espelho.html:231-242`); sem realizado, imprime batidas cruas (`:243-246`) | Transporte da ata: `app/escala/tests/test_t41_leitor_celula.py::test_orfas_chegam_com_hora_real_nao_interrogacao` (`:47-71`). Prova ponta a ponta UI órfã: nenhuma entre os arquivos abertos. |
| Batida órfã sem célula/marcos | `montar_realizado_grade` (`app/escala/utils.py:1081`) | Guarda `if not gd or not marcos` devolve `[],[]` (`app/escala/utils.py:1087-1088`); TELA grava `orfas_grade=[]` em memória (`app/ponto/services/espelho.py:698`, `:853`) | Nada | Batidas cruas, sem marcação de órfã nesse template (`app/templates/ponto/espelho.html:243-246`) | `app/ponto/tests/test_lastro_c11_sem_banco.py::test_PDF_igual_tela_orfa_sem_celula_dia_valido_coletor_inteiro` (`:380-389`): construção chama atribuição real da tela por AST (`:306-319`), mas `espelho_do_colab` é mock (`:325-326`). |
| Dia relavrado `ausencia→folga`, com batida | `grade_da_celula` lê `ata.tipo_dia` (`app/escala/services/leitor_celula.py:397-398`); A2 separado (`app/ponto/services/espelho.py:653`); D4 separado (`app/ponto/services/espelho.py:170`) | Define `dia.folga` pelo `gd.tipo_dia=='folga'` (`app/ponto/services/espelho.py:688`, `:779`); mantém batidas (`:775`); transporta cobertura atual para palavra (`:882`), que tem precedência sobre folga no tradutor (`app/ponto/services/dia_decidido.py:363-372`) | Nada no leitor; lavra é anterior | Card pode mostrar badge `Folga` (`app/templates/ponto/espelho.html:171`) e batidas (`:231-246`); faixa lê D4, podendo conservar status lavrado (`app/ponto/status_dia.py:290-293`). Não deduzir “Folga” na palavra só por `ata.tipo_dia`: ela usa A2/cobertura (`app/ponto/services/espelho.py:879-882`, `app/ponto/services/dia_decidido.py:363-372`) | Nenhuma prova aberta da sequência real relavra+UI; teste D4 geral inclui folga na janela, mas não relavra (`app/ponto/tests/test_e4_esp_espelho_le_o_d4.py:72-79`). |

Fontes e escritores dos campos:

- `ata.tipo_dia`, `ata.orfas`, `ata.n_orfas`: compositor `ata_do_dia` escreve no dict (`app/ponto/services/cartorio.py:729-736`); leitor transporta tipo e órfãs (`app/escala/services/leitor_celula.py:397-420`).
- `ata.ausencia_tipo/id/label`: compositor copia da grade (`app/ponto/services/cartorio.py:763-768`); leitor copia para grade lida (`app/escala/services/leitor_celula.py:436-438`).
- `orfas_grade`, `realizado_grade`, `folga`: montador TELA escreve (`app/ponto/services/espelho.py:779-781`, `:853`); template lê (`app/templates/ponto/espelho.html:171`, `:231-242`).
- `_recusa_do_dia`: montador escreve de A2 (`app/ponto/services/espelho.py:880`); `aplicar_palavra_do_dia` lê e escreve palavra/veredito/cor (`app/ponto/services/espelho.py:339-341`).
- `status/status_rotulo`: `montar_semanas` escreve da saída D4 (`app/ponto/services/espelho.py:170-174`); faixa UI lê (`app/templates/ponto/espelho.html:151`). Escritor persistente de `CelulaDia.status_dia/status_rotulo` não aberto por este explorer [nao verificado].

Nuance essencial dos três sítios: `TipoSemFaixa` no caminho normal/pre-apuração dentro do `try` pode cair em `except Exception`, zerar grade e seguir (`app/ponto/services/espelho.py:533-574`), alcançando posteriormente A2+recusa (`:653-654`) e `palavra_dia` (`:880`, `:339-341`, `:1301`). Portanto “rótulo não chega” nesses dois sítios é demonstrável para **a grade daquele caminho**, mas não autoriza afirmar que o dicionário final jamais recebe palavra. No fallback `AtaAgregadaSemMarcos`, a chamada a `_pela_grade` dentro do handler (`:563`) pode propagar, sem alcançar a montagem posterior; handlers irmãos não cobrem exceção levantada dentro desse handler (`:540-574`).

Nos testes c10-c13 abertos, não existe execução desses três caminhos inteiros: o teste de palavra executa somente bloco AST posterior à grade (`app/ponto/tests/test_lastro_c10_sem_banco.py:236-267`); o teste c13 de recusa chama `_marcar_ausencias` diretamente (`app/ponto/tests/test_lastro_c13_sem_banco.py:110-128`).

## 12/11 10:57 · sessao 3f3e9341170f · - · - · origem=Ronald (tmux)

**ok funcional; FILA de cobertura.**

- **Catraca efetiva:** `catraca_excecao` rejeita 2 itens, outro SHA, zero itens e motivo vazio (`bin/tests/test_lei_akita.sh:373-378`). Confirmei esses retornos executando apenas a função extraída em memória. O fluxo real chama a catraca **antes de enumerar** (`:932-935`; enumeração em `:422`). O erro interrompe Python antes de `PADRAO_FIM` (`:934`, `:943`); o shell registra `_falha` por marcador ausente (`:950`), incrementa `falhas` (`:26`) e termina com **rc 1** (`:957-961`).

- **MORDE da catraca:** cobre os quatro argumentos inválidos e a lista real, chamando a mesma função (`:913-918`). **FILA:** testa somente o retorno da função; remover a chamada produtiva de `:932-934` não é detectado por esses casos. Portanto não há mordida específica da integração catraca→rc do selo (`:913-918`, `:932-935`).

- **Mutante que devolve (c):** por leitura, o caso com `padrao_merge='juiz unico'` mata a restauração literal da regra antiga: cria `passo` inválido fora da primeira linha (`:901-903`), cria `fp` inválido no tronco (`:904-906`), pousa com merge válido (`:907`) e exige ambos vermelhos (`:908-911`). O antigo ramo dispensaria `passo`; isso imprime `MORDE_FALHOU`, rejeitado pelo shell e convertido em rc 1 (`:954-955`, `:961`). Execução do mutante real: **[nao verificado]**, pois o MORDE cria arquivos (`:714-725`).

- **Vacuidade:** os casos de merge com termo inventado e `n/a` (`:896`) **não matam o mutante (c)**: a regra antiga já recusava dispensar o passo nesses cenários — conforme delta v5 fornecido **[nao verificado]**. Continuam controles úteis da permanência das falhas (`:909`).

- **Volta+ff literal:** **FILA:** o novo teste chamado `volta/pouso` constrói pouso `raia→base` com `--no-ff`, após voltar ao tronco (`:904-907`); não constrói `main→raia` seguido de ff do main. A mordida da restauração integral de (c) vem do pouso válido (`:896-911`); regressão específica de volta+ff não está demonstrada nesse bloco. [nao verificado]

## 12/11 10:57 · sessao 926af4d1aeec · - · - · origem=Ronald (tmux)

- **ok — leitores externos por rc.** `bin/pre-push.sh:38` chama o selo com `LEI_AKITA_HEAD="$PUSH_SHA"` e aborta com rc 1; essa chamada antecede o atalho de arsenal (`bin/pre-push.sh:43`). `bin/regua.sh:138` enumera os selos, captura saída e aborta pelo rc (`bin/regua.sh:140–143`). A régua tem atalho anterior que pode pular a pasta (`bin/regua.sh:84–94`); o pre-push continua chamando diretamente o selo (`bin/pre-push.sh:38`).

- **ok — escritores/leitores internos.** `EXCECAO_NOMINAL` nasce em `bin/tests/test_lei_akita.sh:369–371`; é lida na pertinência e no motivo (`:462–463`), no MORDE (`:914–915`) e na catraca real (`:932`). `EXCECAO_CATRACA` nasce em `:372`, alimenta o argumento padrão da catraca (`:373`) e o SHA dos casos MORDE (`:913`). `POR_EXCECAO` é declarada global e zerada em `:398–399`, recebe apenas falha nominal em `:462–464`, é lida pelo MORDE em `:909–911` e pela emissão em `:940–941`.

- **ok — emissão e apresentação.** O Python escreve `PADRAO_EXCECAO` com SHA abreviado apenas na saída e motivo em `bin/tests/test_lei_akita.sh:941`; o Bash lê e apresenta em `:949`, e conta essas linhas na frase final em `:958`. A frase final termina com quantidade de exceções e `catraca 1` (`:958`); nenhum leitor de frase foi identificado nas chamadas abertas (`bin/pre-push.sh:38`; `bin/regua.sh:140–143`).

- **ok no censo literal da cópia — sem leitor órfão identificado.** A busca por `PADRAO_POUSO`, `POR_POUSO`, `pouso_de`, `julgar_pouso` e `primeira_linha` em `bin`, hooks e pastas ocultas não encontrou referências. Não há referência externa aos novos nomes no censo literal. A ocorrência externa de “pelo pouso” é comentário sobre outra obra (`bin/tests/test_esmeril_celula_censo.sh:10`). Leitores construídos dinamicamente ou instalação real: **[nao verificado]**.

- **FILA/limite de instalação.** O instalador define fonte/instalação em `bin/hooks.sh:9–11` e gera wrapper que faz `exec bash .../bin/pre-push.sh` em `:49–56`; o hook instalado não foi aberto: **[nao verificado]**. Existe uma cópia histórica separada em `app/core/hooks/pre-push.sh`, cujo fluxo não chama AKITA (`app/core/hooks/pre-push.sh:5–28`); não há evidência de ela ser o hook instalado **[nao verificado]**.

## 12/11 10:57 · sessao 1662bd412cf1 · - · - · origem=Ronald (tmux)

Rastreio somente leitura; testes abaixo foram abertos, não executados [nao verificado]. [nao verificado]

**A correção da referência do aval:** `cartorio.py:763-768` não faz `ausencia→folga`: transporta `ausencia_tipo`, `ausencia_id`, `ausencia_label` para a ata. A atribuição de `tipo_dia='folga'` ocorre em `_marcar_ausencias`: faixa A2 `folga`/`feriado` e cobertura integral; parcial continua `ausencia` (`app/escala/utils.py:1238-1254`; `app/ponto/services/cartorio.py:763-768`).

**Caminhos de `simular_folha`:**

| Caminho | Passa pela relavra? | Referência |
|---|---|---|
| Comando sem opções | Não: lê export/fechamentos já gravados | `app/folha/management/commands/simular_folha.py:120-134`; `_retidos` lê gravados em `:76-80` |
| `--lido` | Não: chama fechamento em leitura; a grade usada pelo fechamento lê atas existentes | `app/folha/management/commands/simular_folha.py:124-134`; `app/ponto/services/fechamento.py:1014-1015`; `app/folha/export.py:329-338`; `app/escala/services/leitor_celula.py:378-398` |
| `--recalcular` | Não relavra CelulaDia: recalcula fechamento, lê a mesma grade lavrada; persiste FechamentoMensal e DiaPago | `app/folha/management/commands/simular_folha.py:121-123`; `app/ponto/services/fechamento.py:1014-1015`, `:1142-1149`, `:1196-1206`; `app/ponto/services/dia_pago.py:359-360` |
| `--comparar` | Não: lê JSON e compara | `app/folha/management/commands/simular_folha.py:243-251` |
| Shell `foto`/`par`, sem `--antes` | Não: shell chama `--recalcular`; `par` faz as duas fotos | `bin/simular_folha.sh:73`, `:97-99` |
| Shell `foto --antes ...` | **Pode**, conforme comando fornecido: executa `tenant_command $ANTES` antes do recalculo | `bin/simular_folha.sh:65-74` |
| Shell `par --antes ...` | **Pode**, somente na foto **depois**, conforme comando fornecido | `bin/simular_folha.sh:94-99` |

Um caminho concreto de relavra é `--antes "processar_cartorio --empresa N --apply --forcar"`: flags abertas em `app/ponto/management/commands/processar_cartorio.py:31-34`, dias passados da empresa selecionados em `:54-67`, chamada a `julgar_colab` em `:71`. O cartório monta grade prevista viva em `app/ponto/services/cartorio.py:1163-1172`, consulta/transporta a ata nova em `:1432-1436` e lavra em `:1846-1870`. Isso descreve alcance do código; execução concreta desse caminho no c13 [nao verificado]. `--antes` genérico sozinho não prova que relavra ocorreu (`bin/simular_folha.sh:72-74`).

Mesmo invocar o cartório sem forçar pode conservar a ata velha se impressão estiver em dia: ramo normal `app/ponto/services/cartorio.py:1405-1411`, ramo sem adesão `:1256-1262`. O teste existente demonstra preservação de `{'tipo_dia':'ausencia'}` sem fato novo e mudança para `folga` com fato novo: `app/ponto/tests/test_lastro_c5_sem_banco.py::LastroC5::test_P5_ata_antiga_e_FOTO_sem_bump_ate_fato_novo` (classe exata `LastroC5` [nao verificado]; método/linhas verificados em `:692-712`).

**Escritores e leitores:**

- Grade escreve `dia.tipo_dia`, `ausencia_tipo/id/label`, `cobertura_parcial`: `app/escala/utils.py:1243-1254`. Cartório copia tipo e métricas/orfas para `_ata`: `app/ponto/services/cartorio.py:729-753`; copia metadados de ausência em `:766-768`.
- Persistência passa por `CelulaDia.lavrar_veredito` → porta: `app/escala/models.py:1819-1828`; porta atribui `cel.ata` e devolve campos em `app/ponto/portas/celula.py:1032-1047`; cartório usa `save=False` e `carimbar` em `app/ponto/services/cartorio.py:1847-1870`; `carimbar` salva campos lavrados em `app/ponto/portas/celula.py:1065-1072`.
- Leitor `grade_da_celula` prefere `ata.tipo_dia`; só sem esse valor usa `cel.trabalha`: `app/escala/services/leitor_celula.py:397-398`. Transporta orfas/métricas em `:418-428`, ausência em `:433-442`.
- Dinheiro conserva ausência quando existe `ausencia_tipo` mesmo se rótulo é folga: `app/ponto/services/dia_pago.py:392-406`. Teste sintético da cadeia grade→ata→leitor→mapas: `app/ponto/tests/test_lastro_c5_sem_banco.py::test_B1_grade_ata_leitor_folha_preserva_tipo_link_e_dinheiro`, `:342-360`; cobre realizados 0/180/600, não `simular_folha`.
- `trabalha` tem escritores gerar/editar na porta: criação `app/ponto/portas/celula.py:851-857`, edição em `:609`; a lavra acima altera ata/veredito/status e não atribui `trabalha` (`:1032-1038`).

**Os dois leitores de folga com batida:**

1. `folha.causas.classificar_causa` usa `tipo_dia=='folga' and orfas` em `app/folha/causas.py:100`. Só chega ali depois das saídas antecipadas para falta de previsão, classe da régua, ausência de furo, adesão zero e sem âncora (`:58-99`); retorna `dias_trocados` em `:104-106`, sem conferir `ausencia_tipo` (`:100-106`). Prova existente: `app/folha/tests/test_causas.py::CausasTest::test_dias_trocados_nao_vira_falta`, `:81-85`, com folga sintética/orfas e trabalho com missing; nenhuma relavra nesse teste (`:81-85`).

2. `tripwire_celulas.medir_ata_fantasma` pré-filtra `trabalha=False` ou NULL (`app/ponto/management/commands/tripwire_celulas.py:338-340`), então exige ata `folga`, sem lâmpadas (`:342`) e conta `n_celulas>0` (`:344-349`). Não pergunta se houve batida/orfas nesse julgamento (`:341-349`). **Célula `trabalha=True` relavrada com ata folga fica fora do queryset**, embora o leitor `grade_da_celula` reconheça o tipo da ata (`:338-342`; `app/escala/services/leitor_celula.py:397-398`). Provas existentes: `app/ponto/tests/test_tripwire_ata_fantasma.py::TripwireEnxergaFolgaIndeterminadaTest::test_GOLDEN_folga_com_trabalha_NONE_e_contada`, `:56-65`, e `::test_folga_com_trabalha_FALSE_segue_contada`, `:67-71`; as fixtures têm `n_orfas=0` (`:47-54`), não fecham relavra com batida.

Testes de simulação abertos não demonstram esse caso: `app/folha/tests/test_o48_passo4_simular_folha_lido.py` prepara jornada/folga e vereditos em `:87-112`, testa escolha da autoridade em `::test_MORDE_a_foto_gravada_nao_manda_na_linha_do_TXT_quando_lido` (`:159-189`), gravado em `::test_MORDE_sem_lido_a_foto_segue_o_gravado` (`:191-215`) e ausência de escrita de fechamento/DiaPago em `::test_MORDE_lido_nao_escreve_nada` (`:231-245`). Ausência→folga dentro de `simular_folha` nessas provas: nenhuma (`:87-112`, `:159-245`).

## 12/11 10:57 · sessao bd44bcd67409 · - · - · origem=Ronald (tmux)

PDF — achados para consolidação, apenas leitura; testes não executados [nao verificado]. [nao verificado]

| Entrada | Juiz consultado | Operação | Persistido | Retorno | Prova existente |
|---|---|---|---|---|---|
| Ausência conhecida com faixa, aprovada | `faixa_de`, `app/ponto/precedencia.py:105`; A2 em `app/escala/utils.py:1212`; composição comum em `app/ponto/services/espelho.py:343` | Coletor chama tela e recorta seus dias (`app/relatorios/pdf_espelho.py:479`, `:563`); reaplica palavra comum (`:598`) | Nenhuma escrita de ata/cadastro pelo coletor; muta os objetos dos dias (`:567`, `:571`, `:600`). Emissão auditável grava registro, conforme ressalva abaixo | `palavra_dia` na coluna Motivo (`app/relatorios/pdf_espelho.py:885`) | `app/relatorios/tests/test_rotulo_ausencia_pdf.py::test_05_tipo_novo_criado_pela_porta_rotula_sozinho` (`:52`), com código `reciclagem`, cadastro pela porta e ausência aprovada |
| Ausência de tipo sem faixa | A2 captura recusa por data quando recebe `recusas`, `app/ponto/precedencia.py:419`; grade recebe recipiente em `app/relatorios/pdf_espelho.py:474` | Grade escreve `recusa`, `tipo_dia=None` e preserva geometria (`app/escala/utils.py:1218`); PDF transporta recusa da **grade própria**, `app/relatorios/pdf_espelho.py:567`; compositor a transforma em palavra vermelha (`app/ponto/services/espelho.py:339`) | Nada de domínio; campos transitórios em memória (`app/relatorios/pdf_espelho.py:567`, `:600`) | `"Tipo sem faixa: <codigo>"`, veredito vazio (`app/ponto/services/espelho.py:340`), desenhado em `app/relatorios/pdf_espelho.py:885` | `app/ponto/tests/test_lastro_c13_sem_banco.py::test_PDF_le_rotulo_recusado_do_contrato_sem_concluir_precedencia` (`:154`); teste DB existente `app/relatorios/tests/test_rotulo_ausencia_pdf.py::test_09_legado_sem_faixa_rotula_so_o_dia_recusado` (`:85`) |
| Batida órfã; caso sem célula na tela | PDF consulta `grade_espelho_janela`, `app/relatorios/pdf_espelho.py:474`; tela chama `montar_realizado_grade`, `app/ponto/services/espelho.py:698` | PDF lê `grade_pdf.dias[].orfas`, indexados por data (`app/relatorios/pdf_espelho.py:475`, `:480`), une horários (`:481`), escreve `batida.orfa` (`:571`) e exclui entrada espúria de turno aberto (`:517`) | Nada de domínio; atributo transitório `batida.orfa` (`app/relatorios/pdf_espelho.py:571`) | Órfã continua impressa, cinza com asterisco (`app/relatorios/pdf_espelho.py:838`, `:847`); seus alertas de turno são omitidos (`:926`) | `app/ponto/tests/test_lastro_c11_sem_banco.py::test_PDF_igual_tela_orfa_sem_celula_dia_valido_coletor_inteiro` (`:380`) exige `orfas_grade=[]` na tela (`:383`) e `batida.orfa=True` (`:384`); o nome do teste não demonstra igualdade |
| Dia relavrado de ausência para folga, com batida | Faixa A2 traduzida em `tipo_dia='folga'` para cobertura integral (`app/escala/utils.py:1243`); leitura dos dias da tela em `app/relatorios/pdf_espelho.py:563` | PDF usa dias da tela, mas recalcula classificação transitória da órfã pela própria grade (`app/relatorios/pdf_espelho.py:567`); mostra palavra e pago da linha (`:864`, `:885`) | Coletor não relavra; cartório transporta `ausencia_tipo/id/label` à ata (`app/ponto/services/cartorio.py:766`) | Não é possível afirmar só de `ata.tipo_dia='folga'` que a palavra será “Folga”: compositor prioriza cobertura (`app/ponto/services/dia_decidido.py:363`), depois reconhece fatos A2 `folga/folga_da_escala` (`:371`). Resultado concreto sem mais entradas [nao verificado] | **Nenhuma prova aberta da sequência relavra → PDF**. Teste próximo `app/relatorios/tests/test_pdf_linha_do_dia_e_o_pago.py::test_MORDE_folga_trabalhada_NAO_sai_como_zero_no_papel` (`:100`) fabrica `DiaPago` (`:113`); ele mesmo declara que testa derivação, não lavratura (`:109`) |

A função que a tela chama é `montar_realizado_grade` (`app/ponto/services/espelho.py:698`), definida em `app/escala/utils.py:1081`: sem dia ou marcos retorna duas listas vazias (`:1087`); caso contrário, devolve `gd['orfas']` (`:1113`). O resultado é publicado em `dia['orfas_grade']` (`app/ponto/services/espelho.py:853`).

O PDF **já chama** `espelho_do_colab` (`app/relatorios/pdf_espelho.py:479`). Portanto, poderia ler desse retorno `{d['data']: set(d.get('orfas_grade') or []) ...}`, exatamente a troca introduzida pelo mutante `orfas_tela` (`app/ponto/tests/test_lastro_c13_sem_banco.py:45`). Desapareceriam chamada e índice da grade própria (`app/relatorios/pdf_espelho.py:473`); mudaria a fonte do mapa (`:480`). Porém, `:567` também depende de `_dias_grade` para a recusa: ao remover a grade, esse transporte precisa ler `_recusa_do_dia` que a tela publica (`app/ponto/services/espelho.py:880`), preservando a palavra comum (`:339`). Apagar apenas `:474-476` deixa referência pendente em `:567`.

A igualdade do selo é fraca: coletor seleciona os mesmos objetos de `_tela['dias']` (`app/relatorios/pdf_espelho.py:563`), depois os muta (`:567`, `:571`, `:600`); a fixture devolve `dias` ao chamador (`app/ponto/tests/test_lastro_c11_sem_banco.py:378`) e compara essa lista ao resultado (`:382`). Já a divergência explícita de fontes é demonstrada pelas asserções `:383-384`.

“Persistido = nada” deve ser limitado ao domínio/coletor: a emissão mensal chama `gerar_pdf_auditavel` (`app/relatorios/pdf_espelho.py:1065`), que cria `RegistroPDFAuditavel` (`app/relatorios/pdf_base.py:152`) com hash, filtros, autor, identificação do colaborador e dados de origem (`:153-165`). A emissão informacional usa buffer em memória e retorna bytes (`app/relatorios/pdf_espelho.py:1018`, `:1033`).

A união só por HH:MM existe (`app/relatorios/pdf_espelho.py:481`); `_entrada_espuria` compara apenas horário (`:178`), e esse predicado controla a contagem de turnos abertos (`:517`). Assim, a estrutura permite colisão entre datas; incidência real no dado raspado [nao verificado].

_4188 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
