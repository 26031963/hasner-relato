# CODEX-ESTADO

> Ultima resposta processada: **24/11/2026 02:26** · publicado em **24/11/2026 02:26** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 24/11 02:26 · sessao 7e01b5d91af0 · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

## (1) UNIVERSO e emp_comp

```text
UNIVERSO pares_fechados 16167 invertidos 4 sha256 848979805ef7be0fd6fce6610e41ce9e53c68dcfe3403c902b916763c17bb11d
emp_comp 1 10/2026 fechados 12 invertidos 0
emp_comp 1 11/2026 fechados 14 invertidos 0
emp_comp 2 10/2026 fechados 6038 invertidos 2
emp_comp 2 11/2026 fechados 6337 invertidos 1
emp_comp 2 12/2026 fechados 275 invertidos 0
emp_comp 3 10/2026 fechados 1478 invertidos 1
emp_comp 3 11/2026 fechados 1408 invertidos 0
emp_comp 3 12/2026 fechados 61 invertidos 0
emp_comp 4 10/2026 fechados 276 invertidos 0
emp_comp 4 11/2026 fechados 258 invertidos 0
emp_comp 4 12/2026 fechados 10 invertidos 0
```

## (2) Linhas INV

```text
INV 109 3 10/2026 N 2026-10-05 2026-10-05 22:00:41 2026-10-05 22:10:41 2026-10-05 22:00:41 2026-10-05 22:00:00 0.0 10.7 0.0 649.3 INV
INV 399 2 10/2026 N 2026-09-30 2026-09-30 18:30:29 2026-09-30 18:30:29 2026-09-30 18:30:29 2026-09-30 18:30:00 0.0 0.5 0.0 0.0 INV
INV 456 2 11/2026 N 2026-11-14 2026-11-14 19:00:08 2026-11-14 19:00:08 2026-11-14 19:00:08 2026-11-14 19:00:00 0.0 0.1 0.0 0.0 INV
INV 473 2 10/2026 N 2026-09-21 2026-09-21 19:00:18 2026-09-21 19:23:54 2026-09-21 19:00:18 2026-09-21 19:00:00 0.0 23.9 0.0 0.0 INV
```

## (3) Classificação e contas

**Os quatro casos são B.** Em cada linha do anexo, o dia civil da entrada coincide com o dia de jornada da coluna 5, a entrada efetiva é igual à entrada bruta e a saída foi recortada para antes de ambas as batidas. A cópia ancora os marcos no dia da jornada e grava as pontas resultantes do recorte. O limite de saída pode incorporar extra declarada. O marco cadastrado e a extra declarada, separadamente para cada par desta execução, ficam [nao verificado]. [app/ponto/motor_calculo_v2.py:1549](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/motor_calculo_v2.py:1549) · [app/ponto/motor_calculo_v2.py:1574](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/motor_calculo_v2.py:1574) · [app/ponto/janela_he.py:168](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/janela_he.py:168) · [app/ponto/motor_calculo_v2.py:1873](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/motor_calculo_v2.py:1873)

| INV | Classe | Dia civil da entrada = dia de jornada | Saída efetiva < entrada bruta ≤ saída bruta | Saída efetiva − entrada efetiva |
|---|---|---|---|---|
| 109 | B | `2026-10-05` | `22:00:00 < 22:00:41 < 22:10:41` | `22:00:00 − 22:00:41 = −41 s` |
| 399 | B | `2026-09-30` | `18:30:00 < 18:30:29 = 18:30:29` | `18:30:00 − 18:30:29 = −29 s` |
| 456 | B | `2026-11-14` | `19:00:00 < 19:00:08 = 19:00:08` | `19:00:00 − 19:00:08 = −8 s` |
| 473 | B | `2026-09-21` | `19:00:00 < 19:00:18 < 19:23:54` | `19:00:00 − 19:00:18 = −18 s` |

## 24/11 02:23 · sessao e938a376abf2 · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

A sonda `DEPOIS` parou na inicialização do contêiner, com `rc_sombra=125`. O trecho raspado termina sem contagens de C1 e sem linhas `fech`. (`DADO RASPADO:3–6`, contando a partir da primeira linha `sonda_leitura`.)

### 1. Contagens e soma C1

| Classe | Noturno bruto | Motor agora | Contagem |
|---|---:|---:|---|
| `iguais_so_fechados` | ≤ 5 min | = 0 | [nao verificado] |
| `iguais_so_fechados` | ≤ 5 min | > 0 | [nao verificado] |
| `iguais_so_fechados` | > 5 a 30 min | = 0 | [nao verificado] |
| `iguais_so_fechados` | > 5 a 30 min | > 0 | [nao verificado] |
| `iguais_so_fechados` | > 30 min | = 0 | [nao verificado] |
| `iguais_so_fechados` | > 30 min | > 0 | [nao verificado] |
| Demais classes, inclusive seus nomes | | | [nao verificado] |
| **Soma = C1** | | | **[nao verificado]** |

O trecho contém o erro do Docker abaixo. Traceback Python [nao verificado]. (`DADO RASPADO:3–6`)

```text
docker: Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: error during container init: error mounting "/home/ronald/saas-hasner/app/staticfiles" to rootfs at "/app/staticfiles": create mountpoint for /app/staticfiles mount: make mountpoint "/app/staticfiles": mkdirat /var/lib/docker/rootfs/overlayfs/ff272d7d2278fe95edf6641b5e8d4057eb88fbf061c4bd2fb5824188b871dde0/app/staticfiles: read-only file system

Run 'docker run --help' for more information
rc_sombra=125
```

### 2. Exemplos `fech` e corte da faixa

Nas seis subclasses, os pares a transcrever e o resultado dos testes `efetivo_saida <= 22:00` e `efetivo_entrada >= 05:00` ficam [nao verificado]. O motor registra as pontas efetivas e usa o trecho recortado para calcular o noturno, com desconto das pausas e prorrogação conforme a configuração. [`motor_calculo_v2.py:1867`](/home/ronald/wt-janela-turno-020503/app/ponto/motor_calculo_v2.py:1867), [`motor_calculo_v2.py:2927`](/home/ronald/wt-janela-turno-020503/app/ponto/motor_calculo_v2.py:2927), [`motor_calculo_v2.py:983`](/home/ronald/wt-janela-turno-020503/app/ponto/motor_calculo_v2.py:983) [nao verificado]

### 3. `motor_agora > 0` com `DiaPago = 0`

Se o positivo vier de um período que a lavratura `versao='motor'` incluiria na mesma linha de colaborador, competência e dia da jornada, o zero gravado diverge do que essa lavratura produziria hoje. `lavrar` soma `minutos_noturnos_legais / 60` dos períodos recebidos no dia atribuído e grava o resultado quando é chamada. [`fechamento.py:1200`](/home/ronald/wt-janela-turno-020503/app/ponto/services/fechamento.py:1200), [`fechamento.py:1217`](/home/ronald/wt-janela-turno-020503/app/ponto/services/fechamento.py:1217), [`dia_pago.py:173`](/home/ronald/wt-janela-turno-020503/app/ponto/services/dia_pago.py:173), [`dia_pago.py:359`](/home/ronald/wt-janela-turno-020503/app/ponto/services/dia_pago.py:359)

A ocorrência dessa divergência na sombra, a data e a versão da lavra e a hipótese de que o zero veio de uma lavra anterior à mudança do motor ficam [nao verificado].

## 24/11 02:22 · sessao 42fa281c12fe · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

1. `UNIVERSO` e `emp_comp` — nenhuma dessas linhas aparece no trecho fornecido. [nao verificado]
2. `INV` — nenhuma linha para transcrever. O trecho mostra um erro de montagem do Docker e termina com `rc_sombra=125`. A quantidade de `INV` fica [nao verificado].
3. Classes A, B e C — sem linhas `INV`, faltam os horários de cada par para fazer as contas e atribuir uma classe. [nao verificado]

## 24/11 02:22 · sessao 9d23ce02ad2a · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

### Transcrição verbatim do anexo

```text
CASO 37 2026-10-05 emp 4 janela True 2026-08-21 piso 0 teto 60 saida True 60
  EC 517 2026-05-21 None TE 12x36 12x36 19:00:00 07:00:00 02:00:00 03:00:00 720 he_extra 0 0
    marcos_do_dia 2026-10-04 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha False
    marcos_do_dia 2026-10-05 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
    marcos_do_dia 2026-10-06 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha False
  CEL 2026-10-04 False gerada None
  CEL 2026-10-05 True gerada {'hf': '07:00', 'hi': '19:00', 'hfi': '03:00', 'hii': '02:00'}
  CEL 2026-10-06 False gerada None
  BAT 2026-10-06 02:00:15 S app
  BAT 2026-10-06 03:00:38 E app
  BAT 2026-10-06 07:01:01 S app
CASO 113 2026-10-30 emp 3 janela True 2026-08-21 piso 0 teto 60 saida True 60
  EC 818 2026-06-24 None TE 12x36 12x36 19:00:00 07:00:00 02:00:00 03:00:00 720 he_extra 0 0
    marcos_do_dia 2026-10-29 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha False
    marcos_do_dia 2026-10-30 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
    marcos_do_dia 2026-10-31 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha False
  CEL 2026-10-29 False gerada None
  CEL 2026-10-30 True gerada {'hf': '07:00', 'hi': '19:00', 'hfi': '03:00', 'hii': '02:00'}
  CEL 2026-10-31 False gerada None
  BAT 2026-10-31 02:00:00 S disputa_s84_retro
  BAT 2026-10-31 03:00:00 E disputa_s84_retro
  BAT 2026-10-31 07:02:00 S disputa_s84_retro
CASO 113 2026-11-01 emp 3 janela True 2026-08-21 piso 0 teto 60 saida True 60
  EC 818 2026-06-24 None TE 12x36 12x36 19:00:00 07:00:00 02:00:00 03:00:00 720 he_extra 0 0
    marcos_do_dia 2026-10-31 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha False
    marcos_do_dia 2026-11-01 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
    marcos_do_dia 2026-11-02 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha False
  CEL 2026-10-31 False gerada None
  CEL 2026-11-01 True gerada {'hf': '07:00', 'hi': '19:00', 'hfi': '03:00', 'hii': '02:00'}
  CEL 2026-11-02 False gerada None
  BAT 2026-11-02 02:01:00 S disputa_s84_retro
  BAT 2026-11-02 03:00:00 E disputa_s84_retro
  BAT 2026-11-02 07:01:00 S disputa_s84_retro
CASO 174 2026-09-23 emp 2 janela True 2026-08-21 piso 0 teto 60 saida True 60
  EC 1616 2026-11-16 None TE 12x36 12x36 18:00:00 06:00:00 01:00:00 02:00:00 720 he_extra 0 0
    marcos_do_dia 2026-09-22 (datetime.time(21, 0), datetime.time(5, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
    marcos_do_dia 2026-09-23 (datetime.time(21, 0), datetime.time(5, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
    marcos_do_dia 2026-09-24 (datetime.time(21, 0), datetime.time(5, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
  CEL 2026-09-22 True gerada {'hf': '05:00', 'hi': '21:00', 'hfi': '03:00', 'hii': '02:00'}
  CEL 2026-09-23 True gerada {'hf': '05:00', 'hi': '21:00', 'hfi': '03:00', 'hii': '02:00'}
  CEL 2026-09-24 True gerada {'hf': '05:00', 'hi': '21:00', 'hfi': '03:00', 'hii': '02:00'}
  BAT 2026-09-22 20:58:21 E app
  BAT 2026-09-23 02:46:44 S app
  BAT 2026-09-23 03:46:54 E app
  BAT 2026-09-23 05:02:44 S app
  BAT 2026-09-24 02:32:11 E app
  BAT 2026-09-24 03:24:34 S app
  BAT 2026-09-24 05:00:58 S app
CASO 200 2026-11-02 emp 2 janela True 2026-08-21 piso 0 teto 60 saida True 60
  EC 1548 2026-11-10 None TE 6x1 turno_partido 15:00:00 23:00:00 18:00:00 19:00:00 420 he_extra 10 10
    marcos_do_dia 2026-11-01 (datetime.time(22, 0), datetime.time(6, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
    marcos_do_dia 2026-11-02 (datetime.time(22, 0), datetime.time(6, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
    marcos_do_dia 2026-11-03 (datetime.time(22, 0), datetime.time(6, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
  CEL 2026-11-01 True gerada {'hf': '06:00', 'hi': '22:00', 'hfi': '03:00', 'hii': '02:00'}
  CEL 2026-11-02 True gerada {'hf': '06:00', 'hi': '22:00', 'hfi': '03:00', 'hii': '02:00'}
  CEL 2026-11-03 True gerada {'hf': '06:00', 'hi': '22:00', 'hfi': '03:00', 'hii': '02:00'}
  BAT 2026-11-03 02:00:00 S disputa_s84_retro
  BAT 2026-11-03 02:00:33 E app
  BAT 2026-11-03 03:02:17 S app
  BAT 2026-11-03 06:00:15 S app
  BAT 2026-11-03 22:00:47 E app
  BAT 2026-11-04 02:00:24 S app
  BAT 2026-11-04 03:00:36 E app
  BAT 2026-11-04 06:00:48 S app
CASO 218 2026-10-15 emp 2 janela True 2026-08-21 piso 0 teto 60 saida True 60
  EC 1262 2026-09-20 None TE 6x1 turno_partido 21:00:00 05:00:00 02:00:00 03:00:00 420 he_extra 0 0
    marcos_do_dia 2026-10-14 (datetime.time(21, 0), datetime.time(5, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha False
    marcos_do_dia 2026-10-15 (datetime.time(21, 0), datetime.time(5, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
    marcos_do_dia 2026-10-16 (datetime.time(21, 0), datetime.time(5, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
  CEL 2026-10-14 False gerada None
  CEL 2026-10-15 True gerada {'hf': '05:00', 'hi': '21:00', 'hfi': '03:00', 'hii': '02:00'}
  CEL 2026-10-16 True gerada {'hf': '05:00', 'hi': '21:00', 'hfi': '03:00', 'hii': '02:00'}
  BAT 2026-10-16 02:15:08 S app
  BAT 2026-10-16 03:15:06 E app
  BAT 2026-10-16 05:00:01 S app
  BAT 2026-10-16 20:59:13 E app
  BAT 2026-10-17 02:15:42 S app
  BAT 2026-10-17 03:15:07 E app
  BAT 2026-10-17 05:00:33 S app
CASO 296 2026-11-02 emp 2 janela True 2026-08-21 piso 0 teto 60 saida True 60
  EC 261 2026-07-21 None TE 12x36 12x36 19:00:00 07:00:00 02:00:00 03:00:00 720 he_extra 0 0
    marcos_do_dia 2026-11-01 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha False
    marcos_do_dia 2026-11-02 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
    marcos_do_dia 2026-11-03 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha False
  CEL 2026-11-01 False gerada None
  CEL 2026-11-02 True gerada {'hf': '07:00', 'hi': '19:00', 'hfi': '03:00', 'hii': '02:00'}
  CEL 2026-11-03 False gerada None
  BAT 2026-11-03 00:15:19 E app
  BAT 2026-11-03 01:29:03 E app
  BAT 2026-11-03 07:00:41 S app
```

### Turno de D por caso

`hi` e `hf` vêm da célula de D. Quando o turno previsto cruza a meia-noite, `hf` cai em D+1. [app/escala/models.py:1135] [app/ponto/selecao_periodo.py:138] [app/escala/servico_jornada.py:111]

A lista de batidas resulta do pareador da cópia alimentado com os marcos e os tipos `E`/`S` do anexo. O pareamento efetivo na sombra, que também pode usar o papel dado pela ata, fica [nao verificado]. [app/ponto/turnos.py:1192] [app/ponto/turnos.py:1944] [app/ponto/turnos.py:3125]

- **CASO 37 · 2026-10-05** — `hi` 19:00 em D, `hf` 07:00 em D+1, cruza a meia-noite. Batidas de 2026-10-06: `02:00:15 S`, `03:00:38 E`, `07:01:01 S`. [app/ponto/turnos.py:293]
- **CASO 113 · 2026-10-30** — `hi` 19:00 em D, `hf` 07:00 em D+1, cruza a meia-noite. Batidas de 2026-10-31: `02:00:00 S`, `03:00:00 E`, `07:02:00 S`. [app/ponto/turnos.py:293]
- **CASO 113 · 2026-11-01** — `hi` 19:00 em D, `hf` 07:00 em D+1, cruza a meia-noite. Batidas de 2026-11-02: `02:01:00 S`, `03:00:00 E`, `07:01:00 S`. [app/ponto/turnos.py:293]
- **CASO 174 · 2026-09-23** — `hi` 21:00 em D, `hf` 05:00 em D+1, cruza a meia-noite. Batidas de 2026-09-24: `02:32:11 E`, `03:24:34 S`, `05:00:58 S`. [app/ponto/turnos.py:299] [app/ponto/turnos.py:1371]
- **CASO 200 · 2026-11-02** — `hi` 22:00 em D, `hf` 06:00 em D+1, cruza a meia-noite. Batidas de 2026-11-03: `02:00:00 S`, `02:00:33 E`, `03:02:17 S`, `06:00:15 S`. [app/ponto/turnos.py:293] [app/ponto/turnos.py:1371]
- **CASO 218 · 2026-10-15** — `hi` 21:00 em D, `hf` 05:00 em D+1, cruza a meia-noite. Batidas de 2026-10-16: `02:15:08 S`, `03:15:06 E`, `05:00:01 S`. [app/ponto/turnos.py:293]
- **CASO 296 · 2026-11-02** — `hi` 19:00 em D, `hf` 07:00 em D+1, cruza a meia-noite. Batidas de 2026-11-03: `00:15:19 E`, `01:29:03 E`, `07:00:41 S`. [app/ponto/turnos.py:299] [app/ponto/turnos.py:1371]

## 24/11 02:21 · sessao cebe1e5caa38 · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

## 1. DEPOIS — contagem e saída da sonda

O Docker falhou ao montar `/app/logs` durante a inicialização do contêiner, antes da chamada ao Python. O trecho fornecido contém o erro abaixo. Traceback Python [nao verificado]. [bin/sonda_leitura.sh:203](/home/ronald/wt-janela-turno-020503/bin/sonda_leitura.sh:203) · [bin/sonda_leitura.sh:235](/home/ronald/wt-janela-turno-020503/bin/sonda_leitura.sh:235)

```text
docker: Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: error during container init: error mounting "tmpfs" to rootfs at "/app/logs": create mountpoint for /app/logs mount: make mountpoint "/app/logs": mkdirat /var/lib/docker/rootfs/overlayfs/2f6ba2df7015b1f000d88291ba0c235856952da9e2aea595a35b059ad3fbbb1a/app/logs: read-only file system

Run 'docker run --help' for more information
rc_sombra=125
```

| Classe | Noturno bruto | Motor agora `=0` | Motor agora `>0` |
|---|---:|---|---|
| `iguais_so_fechados` | ≤ 5 min | [nao verificado] | [nao verificado] |
| `iguais_so_fechados` | > 5 a 30 min | [nao verificado] | [nao verificado] |
| `iguais_so_fechados` | > 30 min | [nao verificado] | [nao verificado] |

Demais classes, total C1 e conferência da soma das classes com C1 [nao verificado].

## 2. Exemplos `fech` e corte da faixa noturna

Para cada uma das seis subclasses, os pares `[entrada, saida, efetivo_entrada, efetivo_saida, min_trab, min_fora_janela]` e o resultado do teste por par ficam [nao verificado]. O teste usa as datas completas da faixa em questão, com `efetivo_saida <= 22:00` do início da faixa ou `efetivo_entrada >= 05:00` do fim dela. O motor carimba as pontas efetivas e calcula a interseção em horário local. [app/ponto/motor_calculo_v2.py:89](/home/ronald/wt-janela-turno-020503/app/ponto/motor_calculo_v2.py:89) · [app/ponto/motor_calculo_v2.py:1867](/home/ronald/wt-janela-turno-020503/app/ponto/motor_calculo_v2.py:1867) [nao verificado]

## 3. `motor_agora > 0` com `DiaPago = 0`

**Se a comparação for para o mesmo colaborador, competência, dia de jornada e `DiaPago` de versão `motor`, com os mesmos períodos elegíveis à lavra, o zero gravado está divergente do que o motor da cópia lavraria hoje.** O motor calcula `minutos_noturnos_legais` pelas pontas efetivas, com desconto de intervalo, prorrogação conforme a configuração e aplicação do fator de hora noturna. A lavra da versão `motor` recebe os períodos e o mapa do dia de jornada, soma os minutos legais divididos por 60 e substitui as linhas daquela versão na competência. [app/ponto/motor_calculo_v2.py:983](/home/ronald/wt-janela-turno-020503/app/ponto/motor_calculo_v2.py:983) · [app/ponto/motor_calculo_v2.py:2929](/home/ronald/wt-janela-turno-020503/app/ponto/motor_calculo_v2.py:2929) · [app/ponto/services/fechamento.py:1201](/home/ronald/wt-janela-turno-020503/app/ponto/services/fechamento.py:1201) · [app/ponto/services/fechamento.py:1227](/home/ronald/wt-janela-turno-020503/app/ponto/services/fechamento.py:1227) · [app/ponto/services/dia_pago.py:173](/home/ronald/wt-janela-turno-020503/app/ponto/services/dia_pago.py:173) · [app/ponto/services/dia_pago.py:359](/home/ronald/wt-janela-turno-020503/app/ponto/services/dia_pago.py:359)

Nesta execução, a ocorrência dessa divergência, a equivalência dos insumos, a data da lavra e a causa do eventual zero, inclusive se veio de uma lavra anterior à correção, ficam [nao verificado].

## 24/11 02:20 · sessao 481b8b509dc8 · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

1. `UNIVERSO` e `emp_comp` — nenhuma linha fornecida no trecho da sonda. [nao verificado]
2. `INV` — nenhuma linha fornecida no trecho da sonda. [nao verificado]
3. Classes A, B e C — sem linhas `INV`, não há horários para calcular nem casos para classificar. [nao verificado]

O trecho fornecido termina com um erro do Docker na montagem de `/app/logs` e `rc_sombra=125`. [nao verificado]

## 24/11 02:17 · sessao fc72fded9863 · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

**ANTES — C1 = 402.** As 17 classes somam 402. Entre os 320 casos `iguais_so_fechados`, todos estão em `motor_agora=0`. `anexo-sonda:3–20`

As referências `anexo-sonda:N` numeram a saída fornecida a partir de `sonda_leitura`, incluindo a linha em branco. A correspondência da cópia local com o hash informado e o conteúdo do script `sonda_c1c.py` ficam [nao verificado].

### 1. Contagem por classe

| Classe | Contagem | Fonte |
|---|---:|---|
| `aberto_ou_saida_orfa` | 1 | `anexo-sonda:4` |
| `aberto_ou_saida_orfa+quantidade_de_blocos` | 1 | `anexo-sonda:5` |
| `aberto_ou_saida_orfa+virada_ou_outro_dia` | 1 | `anexo-sonda:6` |
| `batida_orfa_na_celula+aberto_ou_saida_orfa+virada_ou_outro_dia` | 1 | `anexo-sonda:7` |
| `entrada_repetida+aberto_ou_saida_orfa+virada_ou_outro_dia` | 2 | `anexo-sonda:8` |
| `entrada_repetida+batida_orfa_na_celula+aberto_ou_saida_orfa` | 2 | `anexo-sonda:9` |
| `entrada_repetida+saida_apos_marco+aberto_ou_saida_orfa` | 1 | `anexo-sonda:10` |
| `entrada_repetida+saida_apos_marco+aberto_ou_saida_orfa+virada_ou_outro_dia+quantidade_de_blocos` | 1 | `anexo-sonda:11` |
| `entrada_repetida+saida_apos_marco+virada_ou_outro_dia` | 1 | `anexo-sonda:12` |
| `entrada_repetida+virada_ou_outro_dia` | 1 | `anexo-sonda:13` |
| `iguais_com_aberto` | 62 | `anexo-sonda:14` |
| `iguais_so_fechados\|bruto5-30\|motor_agora=0` | 55 | `anexo-sonda:15` |
| `iguais_so_fechados\|bruto<=5\|motor_agora=0` | 208 | `anexo-sonda:16` |
| `iguais_so_fechados\|bruto>30\|motor_agora=0` | 57 | `anexo-sonda:17` |
| `saida_apos_marco+aberto_ou_saida_orfa` | 1 | `anexo-sonda:18` |
| `saida_apos_marco+virada_ou_outro_dia` | 4 | `anexo-sonda:19` |
| `virada_ou_outro_dia` | 3 | `anexo-sonda:20` |
| **Soma = C1** | **402** | `anexo-sonda:3–20` |

O trecho fornecido termina em `rc_sombra=0`, sem traceback para transcrever. `anexo-sonda:1–125`

### 2. Exemplos `fech` e corte da faixa noturna

A comparação usa a data e a hora dos limites da faixa que o par bruto alcançou. Nos pares da tarde, o limite relevante é 22h do próprio dia. Nos pares da madrugada, 05h é do dia civil das batidas. `anexo-sonda:73–84` · [motor_calculo_v2.py:97](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:97)

| Subclasse de `iguais_so_fechados` | `colab` | `dia` | `bruto` | `leg` | `pares` | A ponta efetiva cortou a faixa? | Fonte |
|---|---:|---|---:|---:|---|---|---|
| `bruto5-30\|motor_agora=0` | 108 | 2026-10-19 | 10.5 | 0.0 | `[["2026-10-19 10:00:31", "2026-10-19 22:10:29", "2026-10-19 10:00:31", "2026-10-19 22:00:00", 657.9, 10.5]]` | Sim, saída efetiva = 22:00 | `anexo-sonda:73` |
| `bruto5-30\|motor_agora=0` | 108 | 2026-10-21 | 6.3 | 0.0 | `[["2026-10-21 09:59:32", "2026-10-21 22:06:16", "2026-10-21 10:00:00", "2026-10-21 22:00:00", 659.9, 6.7]]` | Sim, saída efetiva = 22:00 | `anexo-sonda:74` |
| `bruto5-30\|motor_agora=0` | 108 | 2026-10-25 | 17.6 | 0.0 | `[["2026-10-25 10:00:52", "2026-10-25 22:17:37", "2026-10-25 10:00:52", "2026-10-25 22:00:00", 658.1, 17.6]]` | Sim, saída efetiva = 22:00 | `anexo-sonda:75` |
| `bruto5-30\|motor_agora=0` | 108 | 2026-11-16 | 15.6 | 0.0 | `[["2026-11-16 09:59:13", "2026-11-16 22:15:37", "2026-11-16 10:00:00", "2026-11-16 22:00:00", 659.7, 16.4]]` | Sim, saída efetiva = 22:00 | `anexo-sonda:76` |
| `bruto<=5\|motor_agora=0` | 60 | 2026-10-31 | 3.5 | 0.0 | `[["2026-10-31 09:52:32", "2026-10-31 22:03:32", "2026-10-31 09:52:32", "2026-10-31 19:00:00", 486.3, 183.5]]` | Sim, saída efetiva = 19:00 < 22:00 | `anexo-sonda:77` |
| `bruto<=5\|motor_agora=0` | 108 | 2026-10-01 | 0.7 | 0.0 | `[["2026-10-01 09:59:41", "2026-10-01 22:00:39", "2026-10-01 10:00:00", "2026-10-01 22:00:00", 659.3, 1.0]]` | Sim, saída efetiva = 22:00 | `anexo-sonda:78` |
| `bruto<=5\|motor_agora=0` | 108 | 2026-10-03 | 0.5 | 0.0 | `[["2026-10-03 09:56:13", "2026-10-03 22:00:27", "2026-10-03 10:00:00", "2026-10-03 22:00:00", 658.2, 4.2]]` | Sim, saída efetiva = 22:00 | `anexo-sonda:79` |
| `bruto<=5\|motor_agora=0` | 108 | 2026-10-05 | 0.9 | 0.0 | `[["2026-10-05 09:56:07", "2026-10-05 22:00:54", "2026-10-05 10:00:00", "2026-10-05 22:00:00", 660.5, 4.8]]` | Sim, saída efetiva = 22:00 | `anexo-sonda:80` |
| `bruto>30\|motor_agora=0` | 37 | 2026-10-05 | 119.4 | 0.0 | `[["2026-10-06 03:00:38", "2026-10-06 07:01:01", "2026-10-06 19:00:00", "2026-10-06 07:00:00", 0, 960.4]]` | Sim, entrada efetiva = 19:00 > 05:00 e posterior à saída | `anexo-sonda:81` |
| `bruto>30\|motor_agora=0` | 37 | 2026-10-09 | 119.3 | 0.0 | `[["2026-10-10 03:00:40", "2026-10-10 07:01:50", "2026-10-10 19:00:00", "2026-10-10 07:00:00", 0, 961.2]]` | Sim, entrada efetiva = 19:00 > 05:00 e posterior à saída | `anexo-sonda:82` |
| `bruto>30\|motor_agora=0` | 113 | 2026-10-30 | 120.0 | 0.0 | `[["2026-10-31 03:00:00", "2026-10-31 07:02:00", "2026-10-31 19:00:00", "2026-10-31 07:00:00", 0, 962.0]]` | Sim, entrada efetiva = 19:00 > 05:00 e posterior à saída | `anexo-sonda:83` |
| `bruto>30\|motor_agora=0` | 113 | 2026-11-01 | 120.0 | 0.0 | `[["2026-11-02 03:00:00", "2026-11-02 07:01:00", "2026-11-02 19:00:00", "2026-11-02 07:00:00", 0, 961.0]]` | Sim, entrada efetiva = 19:00 > 05:00 e posterior à saída | `anexo-sonda:84` |

Nos quatro exemplos `bruto>30`, o `dia` do registro antecede a data civil da entrada bruta. No código, `_janela_do_dia` busca os marcos por `entrada.date()`, e `entrada_efetiva` ancora o marco na data da batida. Essa lógica é compatível com o par efetivo invertido mostrado no anexo. O motor devolve zero quando a entrada é igual ou posterior à saída e só soma interseções de duração positiva. O corte nos demais casos de cada subclasse fica [nao verificado]. `anexo-sonda:81–84` · [motor_calculo_v2.py:1528](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:1528) · [janela_he.py:20](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/janela_he.py:20) · [janela_he.py:94](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/janela_he.py:94) · [motor_calculo_v2.py:91](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:91) · [motor_calculo_v2.py:112](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:112)

### 3. `motor_agora>0` com `DiaPago=0`

**Para a mesma linha de `DiaPago` e o mesmo conjunto de períodos que entrariam na lavra, sim, no sentido de divergência frente ao cálculo atual.** Na versão `motor`, o fechamento entrega os períodos e o mapa `dia_da_jornada` a `lavrar`. A função usa esse mapa quando há dia para o período, soma `minutos_noturnos_legais / 60` em `horas_noturnas` e grava as linhas da competência e da versão. Se o total comparável agora é positivo, uma lavra com esses períodos atribuiria horas noturnas positivas à linha hoje zerada. [fechamento.py:1201](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/services/fechamento.py:1201) · [fechamento.py:1217](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/services/fechamento.py:1217) · [dia_pago.py:126](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/services/dia_pago.py:126) · [dia_pago.py:173](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/services/dia_pago.py:173) · [dia_pago.py:224](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/services/dia_pago.py:224) · [dia_pago.py:359](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/services/dia_pago.py:359)

O anexo mostra **zero casos `motor_agora>0` entre os 320 `iguais_so_fechados`**. A presença dessa divergência nas outras classes, a data e os insumos da lavra original e o motivo de eventual defasagem ficam [nao verificado]. `anexo-sonda:4–20`

## 24/11 02:15 · sessao d29f00865ac3 · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

## 1. UNIVERSO e emp_comp

Transcrição literal do dado raspado fornecido. Execução da sonda e hashes [nao verificado] de forma independente.

```text
UNIVERSO pares_fechados 16167 invertidos 25 sha256 1247d31fd41f181b3e4f14c331ac9a98cf5d99d830c1833d6ce403daaa6aff5b
emp_comp 1 10/2026 fechados 12 invertidos 0
emp_comp 1 11/2026 fechados 14 invertidos 0
emp_comp 2 10/2026 fechados 6038 invertidos 6
emp_comp 2 11/2026 fechados 6337 invertidos 10
emp_comp 2 12/2026 fechados 275 invertidos 1
emp_comp 3 10/2026 fechados 1478 invertidos 4
emp_comp 3 11/2026 fechados 1408 invertidos 2
emp_comp 3 12/2026 fechados 61 invertidos 0
emp_comp 4 10/2026 fechados 276 invertidos 2
emp_comp 4 11/2026 fechados 258 invertidos 0
emp_comp 4 12/2026 fechados 10 invertidos 0
```

## 2. Linhas INV

Transcrição literal do mesmo dado raspado.

```text
INV 1010 2 11/2026 N 2026-11-05 2026-11-05 01:04:44 2026-11-05 05:59:33 2026-11-05 18:00:00 2026-11-05 05:59:33 0.0 1015.3 0.0 0.0 INV
INV 109 3 10/2026 N 2026-10-05 2026-10-05 22:00:41 2026-10-05 22:10:41 2026-10-05 22:00:41 2026-10-05 22:00:00 0.0 10.7 0.0 649.3 INV
INV 113 3 11/2026 N 2026-10-30 2026-10-31 03:00:00 2026-10-31 07:02:00 2026-10-31 19:00:00 2026-10-31 07:00:00 0.0 962.0 0.0 0.0 INV
INV 113 3 11/2026 N 2026-11-01 2026-11-02 03:00:00 2026-11-02 07:01:00 2026-11-02 19:00:00 2026-11-02 07:00:00 0.0 961.0 0.0 0.0 INV
INV 174 2 10/2026 N 2026-09-23 2026-09-24 02:32:11 2026-09-24 05:00:58 2026-09-24 21:00:00 2026-09-24 05:00:00 0.0 1108.8 0.0 0.0 INV
INV 174 2 11/2026 N 2026-10-25 2026-10-25 02:32:15 2026-10-25 04:57:35 2026-10-25 21:00:00 2026-10-25 04:57:35 0.0 1107.7 0.0 0.0 INV
INV 200 2 11/2026 N 2026-11-02 2026-11-03 02:00:33 2026-11-03 06:00:15 2026-11-03 22:00:00 2026-11-03 06:00:00 0.0 1199.7 0.0 0.0 INV
INV 218 2 10/2026 N 2026-10-15 2026-10-16 03:15:06 2026-10-16 05:00:01 2026-10-16 21:00:00 2026-10-16 05:00:00 0.0 1064.9 0.0 0.0 INV
INV 218 2 11/2026 N 2026-11-14 2026-11-15 03:17:11 2026-11-15 05:00:03 2026-11-15 21:00:00 2026-11-15 05:00:00 0.0 1062.9 0.0 0.0 INV
INV 218 2 12/2026 N 2026-11-21 2026-11-22 03:22:03 2026-11-22 05:00:16 2026-11-22 21:00:00 2026-11-22 05:00:00 0.0 1058.2 0.0 0.0 INV
INV 296 2 11/2026 N 2026-11-02 2026-11-03 00:15:19 2026-11-03 07:00:41 2026-11-03 19:00:00 2026-11-03 07:00:00 0.0 1125.4 0.0 0.0 INV
INV 37 4 10/2026 N 2026-10-05 2026-10-06 03:00:38 2026-10-06 07:01:01 2026-10-06 19:00:00 2026-10-06 07:00:00 0.0 960.4 0.0 0.0 INV
INV 37 4 10/2026 N 2026-10-09 2026-10-10 03:00:40 2026-10-10 07:01:50 2026-10-10 19:00:00 2026-10-10 07:00:00 0.0 961.2 0.0 0.0 INV
INV 382 2 10/2026 N 2026-09-30 2026-10-01 01:01:26 2026-10-01 07:50:17 2026-10-01 23:50:00 2026-10-01 07:50:00 0.0 1368.9 0.0 0.0 INV
INV 382 2 11/2026 N 2026-11-10 2026-11-11 00:02:41 2026-11-11 07:51:20 2026-11-11 23:50:00 2026-11-11 07:50:00 0.0 1428.7 0.0 0.0 INV
INV 399 2 10/2026 N 2026-09-30 2026-09-30 18:30:29 2026-09-30 18:30:29 2026-09-30 18:30:29 2026-09-30 18:30:00 0.0 0.5 0.0 0.0 INV
INV 456 2 11/2026 N 2026-11-14 2026-11-14 19:00:08 2026-11-14 19:00:08 2026-11-14 19:00:08 2026-11-14 19:00:00 0.0 0.1 0.0 0.0 INV
INV 473 2 10/2026 N 2026-09-21 2026-09-21 19:00:18 2026-09-21 19:23:54 2026-09-21 19:00:18 2026-09-21 19:00:00 0.0 23.9 0.0 0.0 INV
INV 515 3 10/2026 N 2026-10-09 2026-10-10 02:59:26 2026-10-10 07:03:44 2026-10-10 19:00:00 2026-10-10 07:00:00 0.0 964.3 0.0 0.0 INV
INV 515 3 10/2026 N 2026-10-11 2026-10-12 03:00:00 2026-10-12 07:05:26 2026-10-12 19:00:00 2026-10-12 07:00:00 0.0 965.4 0.0 0.0 INV
INV 673 2 11/2026 N 2026-10-21 2026-10-22 06:57:21 2026-10-22 07:00:00 2026-10-22 19:00:00 2026-10-22 07:00:00 0.0 722.6 0.0 0.0 INV
INV 841 2 11/2026 N 2026-11-13 2026-11-14 06:53:31 2026-11-14 07:05:54 2026-11-14 19:00:00 2026-11-14 07:00:00 0.0 732.4 0.0 0.0 INV
INV 868 3 10/2026 N 2026-10-10 2026-10-10 02:29:34 2026-10-10 06:58:56 2026-10-10 19:00:00 2026-10-10 06:58:56 0.0 990.4 0.0 0.0 INV
INV 922 2 11/2026 N 2026-10-27 2026-10-29 04:07:39 2026-10-29 07:02:13 2026-10-29 23:00:00 2026-10-29 07:00:00 0.0 1134.6 0.0 0.0 INV
INV 973 2 10/2026 N 2026-10-07 2026-10-08 06:55:11 2026-10-08 07:00:43 2026-10-08 19:00:00 2026-10-08 07:00:00 0.0 725.5 0.0 0.0 INV
```

## 3. Classificação e conta por INV

- **A\* — 18 linhas pelo limite efetivo de entrada.** A data civil da entrada difere do dia da jornada. O motor consulta o marco pela data civil da batida, e o recorte leva a entrada efetiva para depois da saída bruta em todas as 18 linhas. A regra admite `limite efetivo = marco nominal − extra declarada antes`. A igualdade com o marco nominal em cada linha fica **[nao verificado]**. [app/ponto/motor_calculo_v2.py:1528][dia] · [app/ponto/motor_calculo_v2.py:1702][entrada-motor] · [app/ponto/janela_he.py:94][entrada]
- **B — 4 linhas.** Em cada uma, `saída efetiva < entrada bruta ≤ saída bruta`, com a entrada intacta. A saída recortada fica no marco de saída ou depois dele se houver extra declarada. A desigualdade põe o par bruto inteiro depois do marco. [app/ponto/janela_he.py:143][saida] · [app/escala/models.py:83][extra-saida]
- **C — 3 linhas.** A entrada bruta está no próprio dia da jornada. O recorte desloca apenas a entrada, que passa da saída bruta. A saída efetiva permanece igual à saída bruta. [app/ponto/motor_calculo_v2.py:1760][pontas] · [app/ponto/janela_he.py:94][entrada] · [app/ponto/janela_he.py:143][saida]

Cada linha abaixo usa os horários da INV na mesma posição do bloco acima. A última coluna faz `efetivo_saida − efetivo_entrada` em `hh:mm:ss`, antes de limitar a duração trabalhada a zero. [app/ponto/motor_calculo_v2.py:2888][duracao] · [app/ponto/motor_calculo_v2.py:2893][trabalhado]

| INV · colab | Classe | Jornada → dia civil da entrada | Entrada bruta → efetiva | Saída bruta → efetiva | Saída efetiva − entrada efetiva |
|---|---|---|---|---|---:|
| 01 · 1010 | [C][pontas] | 05/11 → 05/11 | 01:04:44 → 18:00:00 | 05:59:33 → 05:59:33 | −12:00:27 |
| 02 · 109 | [B][saida] | 05/10 → 05/10 | 22:00:41 → 22:00:41 | 22:10:41 → 22:00:00 | −00:00:41 |
| 03 · 113 | [A*][entrada] | 30/10 → 31/10 | 03:00:00 → 19:00:00 | 07:02:00 → 07:00:00 | −12:00:00 |
| 04 · 113 | [A*][entrada] | 01/11 → 02/11 | 03:00:00 → 19:00:00 | 07:01:00 → 07:00:00 | −12:00:00 |
| 05 · 174 | [A*][entrada] | 23/09 → 24/09 | 02:32:11 → 21:00:00 | 05:00:58 → 05:00:00 | −16:00:00 |
| 06 · 174 | [C][pontas] | 25/10 → 25/10 | 02:32:15 → 21:00:00 | 04:57:35 → 04:57:35 | −16:02:25 |
| 07 · 200 | [A*][entrada] | 02/11 → 03/11 | 02:00:33 → 22:00:00 | 06:00:15 → 06:00:00 | −16:00:00 |
| 08 · 218 | [A*][entrada] | 15/10 → 16/10 | 03:15:06 → 21:00:00 | 05:00:01 → 05:00:00 | −16:00:00 |
| 09 · 218 | [A*][entrada] | 14/11 → 15/11 | 03:17:11 → 21:00:00 | 05:00:03 → 05:00:00 | −16:00:00 |
| 10 · 218 | [A*][entrada] | 21/11 → 22/11 | 03:22:03 → 21:00:00 | 05:00:16 → 05:00:00 | −16:00:00 |
| 11 · 296 | [A*][entrada] | 02/11 → 03/11 | 00:15:19 → 19:00:00 | 07:00:41 → 07:00:00 | −12:00:00 |
| 12 · 37 | [A*][entrada] | 05/10 → 06/10 | 03:00:38 → 19:00:00 | 07:01:01 → 07:00:00 | −12:00:00 |
| 13 · 37 | [A*][entrada] | 09/10 → 10/10 | 03:00:40 → 19:00:00 | 07:01:50 → 07:00:00 | −12:00:00 |
| 14 · 382 | [A*][entrada] | 30/09 → 01/10 | 01:01:26 → 23:50:00 | 07:50:17 → 07:50:00 | −16:00:00 |
| 15 · 382 | [A*][entrada] | 10/11 → 11/11 | 00:02:41 → 23:50:00 | 07:51:20 → 07:50:00 | −16:00:00 |
| 16 · 399 | [B][saida] | 30/09 → 30/09 | 18:30:29 → 18:30:29 | 18:30:29 → 18:30:00 | −00:00:29 |
| 17 · 456 | [B][saida] | 14/11 → 14/11 | 19:00:08 → 19:00:08 | 19:00:08 → 19:00:00 | −00:00:08 |
| 18 · 473 | [B][saida] | 21/09 → 21/09 | 19:00:18 → 19:00:18 | 19:23:54 → 19:00:00 | −00:00:18 |
| 19 · 515 | [A*][entrada] | 09/10 → 10/10 | 02:59:26 → 19:00:00 | 07:03:44 → 07:00:00 | −12:00:00 |
| 20 · 515 | [A*][entrada] | 11/10 → 12/10 | 03:00:00 → 19:00:00 | 07:05:26 → 07:00:00 | −12:00:00 |
| 21 · 673 | [A*][entrada] | 21/10 → 22/10 | 06:57:21 → 19:00:00 | 07:00:00 → 07:00:00 | −12:00:00 |
| 22 · 841 | [A*][entrada] | 13/11 → 14/11 | 06:53:31 → 19:00:00 | 07:05:54 → 07:00:00 | −12:00:00 |
| 23 · 868 | [C][pontas] | 10/10 → 10/10 | 02:29:34 → 19:00:00 | 06:58:56 → 06:58:56 | −12:01:04 |
| 24 · 922 | [A*][entrada] | 27/10 → 29/10 | 04:07:39 → 23:00:00 | 07:02:13 → 07:00:00 | −16:00:00 |
| 25 · 973 | [A*][entrada] | 07/10 → 08/10 | 06:55:11 → 19:00:00 | 07:00:43 → 07:00:00 | −12:00:00 |

[dia]: /home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:1528
[entrada-motor]: /home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:1702
[entrada]: /home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/janela_he.py:94
[saida]: /home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/janela_he.py:143
[extra-saida]: /home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/escala/models.py:83
[pontas]: /home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:1760
[duracao]: /home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:2888
[trabalhado]: /home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:2893

## 24/11 02:12 · sessao e9d3f1c19b9b · A-TELA-DO-DIA-LE-UM-JUIZ · verificador · origem=Code

**Pré-verificação da parte (1) — bloqueia.** A regra nova pode alcançar uma competência exportada, o mesmo dia pode receber impressões diferentes no cron e no julgamento avulso, e o novo `FURO_PARCIAL` pode reter a folha sem produzir uma pergunta que a cobrança saiba fazer. `app/ponto/services/cartorio.py:333–337,1188–1235,1480–1497,1916–1934,2143–2145`, `app/folha/export.py:1285–1294`

Usei o diff fornecido e o código da cópia, em leitura. Confirmação independente do SHA e do pai, execução do RED e da suíte, volumes de produção e efeito em valores ou rubricas [nao verificado]. [nao verificado]

## Achados

1. **Bloqueia — o horizonte não protege a competência exportada.** `HORIZONTE_SEM_PAR = '2026-11'` habilita toda competência de novembro de 2026 em diante pelo mês do fim da janela da empresa. Uma competência 11, 12 ou posterior com TXT vigente passa pelo mesmo teste. O cartório pode relavrar a célula porque o caminho novo não consulta `competencia_entregue` (K6). A guarda temporária existente seleciona setembro de 2026 e atua no recálculo de `FechamentoMensal`, em outra porta. O selo novo verifica datas anteriores ao piso e declara que a guarda de exportação fica fora do teste. O piso preserva o alcance para frente da **L-124**, mas não cumpre a restrição particular desta obra. A **L-113** altera a **L-092** para recálculos e preserva o TXT anterior. Essa regra geral não substitui o recorte explícito desta parte. `app/ponto/services/cartorio.py:316–337,1223–1235,1953–1977`, `app/ponto/supra_juiz.py:97–105`, `app/folha/export.py:556–589`, `app/ponto/services/fechamento.py:27–42,558–575`, `app/ponto/tests/test_a_tela_sem_par_acusa.py:221–247`, `app/docs/LEIS.md:125,145,155`

2. **Bloqueia — a janela altera o sinal e o pulo do cron.** O cron das 06:27 chama `julgar_colab` com todas as células passadas do colaborador, em ordem, sem `--forcar`. `julgar_celula` passa `[cel]`. A segunda passada do próprio lote pode usar só as células reconciliadas. Cada corpo tira `d0,d1` das pontas da lista. O pareador monta segmentos por vínculo, lê de `ini−2` a `fim+1` e só depois filtra por `data_turno`. O contrato de `dias_encerrados` explicita que o pareamento depende da janela. `app/config/crons.py:616`, `app/ponto/management/commands/processar_cartorio.py:59–65`, `app/ponto/services/cartorio.py:1073–1077,1188–1229,2142–2145`, `app/ponto/turnos.py:1834–1847,1867–1871,1963–2002,2133–2139`

   Um contraexemplo por leitura estática é a E às 23:30 de D e a S às 00:30 de D+2, em vínculo diurno 08–14, sem outras batidas nem papel de pausa ou eco. A janela avulsa exclui a S e deixa a E aberta. A ampla pode fechar o par de 25h no envelope de 26h porque as duas pontas estão longe dos respectivos marcos. Depois de expirar a aberta, o avulso pode gravar `sem_par:E` e o cron omitir a parte. A S em D+2 também fica fora das batidas diretamente hasheadas para D. Com os demais insumos iguais, o cron pode pular um dia que o avulso rejulgaria ou relavrar após o avulso sem fato novo. Isso quebra a coerência e a idempotência exigidas pela **L-099**. Incidência em produção [nao verificado]. `app/ponto/turnos.py:312–340,1377–1422,1430–1454,1491–1502,2005–2035`, `app/ponto/motor_calculo_v2.py:1931`, `app/ponto/services/cartorio.py:1233–1235,601–603,1435–1440,1479–1497`, `app/docs/LEIS.md:122`

3. **Bloqueia — o sinal e o realizado podem vir de pareações diferentes dentro da mesma lavra.** Encerramento e sinal compartilham uma lista, de `turnos_do_colab(c,d0,d1)`, alimentada com a ata persistida. Já a grade usada na comparação de dia cheio pareia com `ref=d1+1` e com uma cópia das lâmpadas e órfãs **desta montagem**. A grade documenta um caso em que a ata anterior mudou os minutos para as mesmas batidas. É o mesmo algoritmo registrado, chamado com duas alimentações. Nos comandos, a mistura é entre a ata lavrada e o sinal calculado agora. O diff não introduz fórmula de minutos nem outro algoritmo de pareamento, mas introduz essa segunda resposta como insumo da decisão. **L-002 e L-099.** `app/ponto/services/cartorio.py:1208–1231,1252–1259`, `app/ponto/turnos.py:1889–1892,1940–1944`, `app/escala/utils.py:1873–1909,2052–2072`, `app/escala/services/leitor_celula.py:397–424`, `app/ponto/management/commands/supra_juiz.py:120–132`, `app/ponto/management/commands/emitir_furo_retroativo.py:60–66`, `app/docs/LEIS.md:60,122`

4. **Bloqueia — a condição diverge do corte em “marcos acesos” e na destinação da espúria.** O ramo novo faz o seguinte. `app/ponto/supra_juiz.py:253–259,290–314,340–341,493–512`

   | Cláusula | Implementação e divergência |
   |---|---|
   | Dia fechado | Usa o veredito de `dias_encerrados`, que consulta turno aberto vivo e compara o instante com o fim previsto. O cartório zera E/S enquanto esse juiz não fecha o dia, inclusive quando o vigia passa `turno_encerrado=True` para julgar só até o marco vencido. O ramo novo atende ao teto da **L-073**, sujeito à alimentação descrita acima. `app/ponto/turnos.py:2121–2130,2166–2182`, `app/ponto/services/cartorio.py:1223–1231,1560–1564`, `app/docs/LEIS.md:100` |
   | E sem par com marcos acesos | Aceita a E inclusive com realizado zero, como pede o literal. Porém `not _n_missing` só conta dicionários com `missing` verdadeiro. Lista vazia ou coluna `None` passa. A montagem da ata pode lavrar `acesa=False` ou `None` para um marco do DNA nessas condições. Há um segundo predicado para a mesma resposta que a lâmpada já expressa. **L-061 e L-002.** `app/ponto/supra_juiz.py:340–341,504–512`, `app/ponto/services/cartorio.py:685–728`, `app/docs/LEIS.md:60,95` |
   | S órfã com realizado positivo | Usa a S do pareador e exige `real > 0`. A condição de zero `missing` também foi aplicada à S, embora o literal associe “marcos acesos” à E. Os ramos antigos ainda podem acusar quando há `missing`. O novo exige trabalho e previsto não zero, exclui isento, sem âncora, justificado e qualquer cobertura aprovada, inclusive parcial. Se passar com `sem_vinculo`, emite código de cadastro. `app/ponto/supra_juiz.py:253–259,290–304,425–485,504–512` |
   | Dia cheio e espúria | `real < prev` exclui igualdade e excedente do ramo novo, em minutos do dicionário da grade. Os ramos antigos de marco apagado continuam aplicáveis. Para a E residual, o ramo apenas suprime o furo novo. A pergunta de espúria está em `SEM_JUIZ` e o fluxo existente de cluster exige ao menos duas órfãs e uma lâmpada apagada. A E única que sobra em dia cheio não é encaminhada por esse fluxo. O exemplo 726/720 consta como relato e fixture, sem confirmação independente da lavra de col63 [nao verificado]. `app/ponto/supra_juiz.py:257–259,451–461,499–512`, `app/core/juizes.py:1223–1225`, `app/ponto/services/cluster_espurio.py:15–36`, `app/ponto/tests/test_a_tela_sem_par_acusa.py:57–64`, `app/docs/RELATO.md:1` |
   | Só competência não exportada | Falta a consulta do estado de entrega, conforme achado 1. `app/ponto/services/cartorio.py:333–337`, `app/folha/export.py:556–589` |

5. **Bloqueia — `FURO_PARCIAL` adquire dois significados que os leitores tratam como um só.** Antes identificava marco apagado com fato. Agora também identifica um resíduo do pareador quando a grade não tem `missing`. Ambos têm precedência para `veredito='furo'`, no mesmo escritor. A **L-088** liga furo a marco não cumprido e pergunta em aberto. O corte novo manda acusar na lavra, mas os seguintes consumidores continuam operando pelo contrato anterior. `app/ponto/supra_juiz.py:451–485,504–512`, `app/escala/models.py:1622–1642`, `app/ponto/portas/celula.py:1096–1105,1126–1134`, `app/docs/LEIS.md:120`

   - **Chamados e cobrança.** O emissor só abre ou espelha cobrança se achar marco apagado nomeável na ata. A fábrica também precisa de motivo de marco. Com as lâmpadas realmente acesas, o novo protesto pode alimentar o contador de furos cobrados e o reprocessamento, que ainda pode devolver `sem_cobranca_depois`. C6 e o reconciliador passam a ler um veredito acusador nos caminhos de perguntas existentes, sujeitos às suas guardas. `app/ponto/services/cartorio.py:1916–1943,2032–2046`, `app/chamados/services/fabrica_da_celula.py:52–74,119–139`, `app/ponto/services/furos_sem_cobranca.py:35,53–63,82–108,117–131`, `app/chamados/juizes.py:1539–1562`, `app/chamados/reconciliador.py:928–963,979–1004`
   - **Folha.** A retenção lê o código e o veredito gravados. Um dia que passe de `concorde` a `furo`, com todos os marcos acesos, pode virar `espelho_furo` e deixar o colaborador fora do TXT. A exceção da **L-033** exige ao menos uma pausa apagada e o conjunto exato `{'FURO_PARCIAL'}`. Com todas acesas, reter demonstra a mudança de elegibilidade, sem demonstrar infração à cláusula *só pausa*. Prévia e lista de fora da folha também leem o resultado. Valores, rubricas e quantidade de retidos em produção [nao verificado]. `app/folha/export.py:968–973,1148–1155,1173,1273–1294`, `app/folha/previa.py:170–171`, `app/folha/fora_da_folha.py:120–121`, `app/docs/LEIS.md:81`
   - **Status, tela e Pauta DP.** O status lavrado passa de `ok` a `irregular` quando o `furo` vence, depois da guarda temporal. Calendário, relatório, painel e app leem o novo estado. A palavra `Em aberto` do calendário ainda vem da lista de E/S do pareador e pode persistir em dia cheio com status `ok`. O novo furo também pode vencer `REALIZADO_ZERO_COM_TURNO` e tirar o dia da Pauta DP dos `discordantes`, embora não haja marco para cobrar. A igualdade entre palavra e status no caso cheio continua pendente pela **L-061/L-099**. `app/ponto/status_dia.py:65–70,154–162,273–289`, `app/colaboradores/services/calendario.py:595–616,748–763`, `app/ponto/services/dia_decidido.py:346–355,419–430`, `app/relatorios/furos.py:119–127`, `app/core/services/painel_op.py:325–336`, `app/api/views.py:1493–1514`, `app/ponto/supra_juiz.py:316–319,504–512`, `app/ponto/services/pautas_dos_discordantes.py:27–31,39–41,76–81,196–213`, `app/docs/LEIS.md:95,122`

6. **Bloqueia — os selos mordem o ramo simples, mas deixam as fronteiras acima sem prova.** Os casos puros de E e S exigem `FURO_PARCIAL`, o de hash distingue `''`, `E`, `S` e `ES`, e o teste avulso morde o pulo com e sem sinal na mesma janela. A comparação entre `sem_par=''` e o default só compara duas chamadas ao código novo. Os casos integrados positivos verificam o sinal bruto e `out['cods']`, sem afirmar a ata persistida com todos os marcos acesos nem o realizado usado. O caso cheio integrado só verifica ausência do código. Faltam ao selo a paridade cron/avulso, a competência **dentro** do horizonte com exportação vigente e o caminho folha/cobrança. A atualização do censo de linhas mantém a catraca de sítios, sem testar essas propriedades. **L-110 e Lei-Akita 5.** RED efetivamente rodado contra o pai e suíte inteira [nao verificado]. `app/ponto/tests/test_a_tela_sem_par_acusa.py:34–102,155–247,255–263`, `app/core/tests/test_medida_turno_do_par.py:291,297,319`, `app/docs/LEIS.md:136`, `CLAUDE.md:28–29,383–392`

7. **Fila — relógio, ordem de trava, pouso e fallbacks.** O comando `supra_juiz` já omitia `agora`, mas a extração muda o instante da leitura interna para **depois** da carga do pareador. O retroativo injeta o instante da borda. Ambos ainda consultam `localdate()` por linha. O cartório injeta `agora` no encerramento, e o avulso conserva as leituras pré-existentes de `localdate()` e `now()` em sua guarda. É desvio do padrão de borda da **L-128**, distinto de usar data civil como teto do ramo novo. `app/ponto/management/commands/supra_juiz.py:120–124,235–237`, `app/ponto/management/commands/emitir_furo_retroativo.py:47,63–66,113–115`, `app/ponto/turnos.py:2148–2163`, `app/ponto/services/cartorio.py:1216–1221,2119–2145`, `app/docs/LEIS.md:150`, `app/docs/PLANO-PADROES.md:97–103`

   - **Escritor, trava e `except`.** O diff acrescenta leituras e código de saída, preserva as portas `lavrar_veredito` e `carimbar` e não introduz `except` nem aquisição nova de trava. O save e seu evento seguem juntos na porta existente. Há sobreposição com o juiz registrado de **marco faltante**, pela aproximação de “acesos” e pelo código reutilizado. O helper em si agrega a saída do pareador já registrado. `app/ponto/services/cartorio.py:1208–1235,1953–1977`, `app/ponto/portas/celula.py:993–1001,1217–1237`, `app/core/juizes.py:193–207,216–248`, `app/escala/servico_jornada.py:452–489`, `app/ponto/supra_juiz.py:86–105,340–341,504–512`
   - **Fallback e estados comprimidos.** Os kwargs são obrigatórios. O mapa ausente e `(False, False)` colapsam ausência de sinal e exclusão pelo horizonte, e o cartório também usa o par falso para dia em curso. `turnos or ()`, `getattr(t, 'aberto', False)` e o `getattr` da empresa ainda convertem carga ou atributo ausente em vazio/falso ou corte padrão. Os caminhos de produção examinados fornecem a lista e o modelo exige empresa. Ocorrência de entrada malformada [nao verificado]. `app/ponto/supra_juiz.py:97–105,239–244`, `app/ponto/services/cartorio.py:1223–1235`, `app/ponto/janelas.py:36–51`, `app/colaboradores/models.py:321`
   - **Pré-requisito.** O corte de 14:16 pôs PAR-PELO-TURNO à frente. O aval de 23:55 autorizou construção em raias paralelas e pousos separados. A cópia ainda usa o teto previsto+4h que o V6 manda retirar do corte do par. O commit em cópia não comprova pouso antecipado. Ordem efetiva, reconstrução das projeções pela **L-121** e DIFF/aval de impacto aplicável pela **L-094** [nao verificado]. `app/docs/CORTES.md:629–630`, `app/docs/PROMPTS.md:2852,2863`, `app/ponto/turnos.py:1423–1454`, `app/docs/LEIS.md:112,142`
   - **L-102/L-103.** O ramo lê minutos e acrescenta protesto. A regra de apresentação para dia inteiro sem turno pareado e a escrita explícita de zero continuam nas respectivas fontes. Uma E ou S residual pode coexistir com pares fechados. `app/docs/LEIS.md:128–129`, `app/escala/utils.py:2052–2072`, `app/ponto/services/dia_decidido.py:410–418`, `app/ponto/supra_juiz.py:257–259,504–512`

## Censo de origem, leitores e chamadores

| Valor | Quem produz e quem lê |
|---|---|
| `entrada_sem_par`, `saida_orfa` | `_mk` grava `Turno.aberto` quando há entrada e falta saída. `datas_saida_orfa` lê `Turno` com saída presente e entrada ausente. `sem_par_do_pareador` agrega por `data_turno` em `{date: (bool E, bool S)}` e filtra pelo horizonte. `classificar_dia` recebe os dois kwargs e os lê no ramo novo. O calendário lê os sinais de origem em sua própria carga. `app/ponto/turnos.py:234–245,312–324`, `app/ponto/supra_juiz.py:86–105,239–244,504–512`, `app/colaboradores/services/calendario.py:602–616` |
| `HORIZONTE_SEM_PAR` | Constante de módulo `2026-11`, validada no import. `sem_par_no_horizonte` lê a competência do dia do turno por `janela_atual`, pelo corte da empresa, e o helper consome esse resultado. Os testes usam também `empresa=None`, com corte padrão 21. `app/ponto/services/cartorio.py:316–337`, `app/ponto/janelas.py:24–33,36–51,60–63`, `app/ponto/supra_juiz.py:97–105`, `app/ponto/tests/test_a_tela_sem_par_acusa.py:108–123,236–243` |
| `sem_par:` e `CelulaDia.impressao` | `_sem_par_da_impressao` serializa `''`, `E`, `S` ou `ES`, já com o filtro de encerramento. `impressao_insumos` inclui o texto quando preenchido e devolve SHA-256 em hexadecimal. A porta `carimbar` persiste o digest. Os dois ramos do cartório comparam o digest para pular, e `celula_veredito_velho` o copia para a fotografia de reversão. O ramo sem adesão omite `sem_par` na impressão e só entrega os kwargs ao classificador depois do pulo. O ramo normal pode trocar a impressão até em dia cheio ou coberto, pois calcula o hash antes da decisão. `app/ponto/services/cartorio.py:513–514,598–603,1223–1235,1283–1292,1337–1355,1480–1500`, `app/ponto/portas/celula.py:1217–1237`, `app/ponto/management/commands/celula_veredito_velho.py:103–110` |
| Previsto, realizado, marcos e encerramento | O cartório usa a grade nova. Ela materializa minutos líquidos do dia pelos marcos e pausas, e usa `realizado_do_dia` sobre os turnos da montagem. Os dois comandos usam `grade_da_celula`, que transporta minutos da ata e consulta o juiz de previsto quando falta o campo. A grade também fornece `celulas`, `tipo_dia` e `justificado`, e o cartório escreve a ata pela porta de lavra. O encerramento vem do juiz registrado `dia_encerrado`, pela carga de janela. `app/ponto/services/cartorio.py:1216–1221,1252–1259,777–815,1953–1977`, `app/escala/utils.py:1620–1650,1818–1822,2052–2081,2241–2245`, `app/escala/services/leitor_celula.py:397–440`, `app/core/juizes.py:193–202,243`, `app/ponto/turnos.py:2121–2182` |

**Chamadores de produção.** `classificar_dia` tem quatro sítios diretos. Cartório sem adesão e normal passam o par filtrado pelo encerramento. `supra_juiz` e `emitir_furo_retroativo` passam E/S após o horizonte, junto do encerramento em outro kwarg. Naqueles quatro sítios a chave é dia do turno, os sinais são booleanos e a referência de tempo é o fuso Django ativo, cujo padrão é `America/Sao_Paulo`. A S de madrugada pode pertencer à véspera. Há uma diferença pré-existente no insumo vizinho `batidas_no_dia`, que os dois comandos contam por data civil e o cartório por células/órfãs do dia-turno. No comando `supra_juiz`, o código novo alimenta placar e exemplos, e a retratação lê a lavra. O retroativo só emite mediante `FURO_SEM_COBRANCA`. `impressao_insumos` tem os dois sítios do cartório, com reexportação pelo comando do cron. `app/ponto/services/cartorio.py:1337–1355,1480–1486,1501–1508,1553–1565`, `app/ponto/management/commands/supra_juiz.py:225–255`, `app/ponto/management/commands/emitir_furo_retroativo.py:103–117`, `app/ponto/management/commands/processar_cartorio.py:20`, `app/ponto/turnos.py:43–48,285–324`, `app/config/settings/base.py:93`

**Chamadores diretos em testes.** As linhas abaixo identificam os sítios. Os testes antigos do classificador injetam `False, False`, inclusive em fixtures com órfãs da grade, que são outro conceito. O teste novo injeta E/S sintéticos e omite os kwargs deliberadamente no teste de `TypeError`. Para a impressão, os testes antigos usam o default e o teste novo exercita os quatro valores. Os proxies e spies transportam os kwargs ou substituem a carga, de modo que os testes sintéticos não comprovam paridade de janela, data ou fuso entre as portas reais. `app/ponto/tests/test_a_tela_sem_par_acusa.py:34–102,255–263`, `app/ponto/tests/test_supra_juiz_orfa_builder.py:15–27`, `app/ponto/tests/test_e3_nb_impressao_memoria.py:145–159,191–212,271,337–382,404–409`, `app/chamados/tests/test_e3_pos_suite_sem_banco.py:135–181`, `app/chamados/tests/test_e4c_f5_voltaC_lotes.py:208–215`

- `classificar_dia` em `app/escala/tests/` — `test_cobertura_parcial_periodo.py:118`, `test_hx_numero_honesto.py:79,88,99,108,124,138,150`, `test_montador_realizado_pela_autoridade.py:173`.
- `classificar_dia` em `app/ponto/tests/` — `test_a_tela_sem_par_acusa.py:43,102`, `test_bo_inss_nunca_bateu_le_a_precedencia.py:273`, `test_bug_lastro_vai_e_volta.py:26,41`, `test_cobparc8_leitor_transporta_justificado.py:80`, `test_cobparc_marco_no_periodo.py:99,170,281,293,489`, `test_concorde_sem_entrada.py:31`, `test_contagem_unica.py:47,51,61,65,74,85,142`, `test_f1c4_3_supra_indefinido_nao_retrata.py:24`, `test_f1c4_3_supra_pergunta_ao_juiz.py:32`, `test_fnd_pergunta_a_juiza.py:86,104,189`, `test_grade_da_celula_cobertura_parcial.py:45`, `test_hx_cobranca_morta.py:23`, `test_supra_juiz.py:26,35,42,55,62,69,77,94,103,110,130,137,144,159,163,169,184,185,215,222,235,245,251`, `test_supra_juiz_cobranca.py:21,70,96`, `test_supra_juiz_fase.py:22`, `test_supra_juiz_indefinido.py:19`, `test_supra_juiz_inflado.py:23,28,33,38,43`, `test_supra_juiz_orfa.py:28,31,34,37,40,43`, `test_supra_juiz_orfa_builder.py:18,20,27`, `test_supra_juiz_teto.py:15,28`.
- `impressao_insumos` em `app/escala/tests/test_cobertura_parcial_periodo.py:205–209` e em `app/ponto/tests/` — `test_a_tela_sem_par_acusa.py:257,261–262`, `test_cartorio.py:16–35`, `test_celula_concorde_meio_turno.py:56–57,86–88`, `test_e3_isento_na_impressao.py:49–55,81–100`, `test_o146_extra_decla

[truncado em 20000 de 20290 caracteres]

## 24/11 02:10 · sessao 5a41b3c84321 · - · - · origem=Ronald (tmux)

**Bloqueia.** O mesmo colaborador-dia recebe sinais de pareamento produzidos com janelas distintas. O cron entrega toda a série de células passadas, o avulso entrega `[cel]`, e o segundo passe do cron entrega somente as reconciliadas. O pareador lê a janela com `ini−2` e `fim+1`, segmenta conforme o vínculo e filtra os resultados por `data_turno`. O sinal entra no hash só quando presente. Quando os sinais divergem, o mesmo dia pode pular no cron ou ser relavrado com outra impressão. Casos e volume em produção [nao verificado]. `app/ponto/management/commands/processar_cartorio.py:49–65`, `app/ponto/services/cartorio.py:1073–1077`, `app/ponto/services/cartorio.py:1188–1235`, `app/ponto/services/cartorio.py:1480–1497`, `app/ponto/services/cartorio.py:2142–2145`, `app/ponto/turnos.py:1834–1847`, `app/ponto/turnos.py:1961–2002`.

**Bloqueia.** O sinal lê geometria alimentada por atas gravadas. O realizado do cartório lê geometria alimentada por lâmpadas e órfãs da montagem corrente, em outra janela. Os dois caminhos usam o mesmo pareador, com entradas diferentes. Os comandos combinam o pareamento atual com minutos da última ata lavrada. `app/ponto/services/cartorio.py:1213`, `app/ponto/services/cartorio.py:1252–1259`, `app/ponto/turnos.py:1889–1892`, `app/ponto/turnos.py:1942–1944`, `app/escala/utils.py:1889–1909`, `app/escala/utils.py:2052–2072`, `app/escala/services/leitor_celula.py:397–424`.

**Bloqueia.** `not _n_missing` aceita ausência de células, posições `None` e dispensas, porque a contagem só considera dicionários com `missing` verdadeiro. O ramo exige essa condição também para S órfã, além de trabalho, previsão positiva na prática, falta de cobertura aprovada, falta de justificativa e âncora. Nas coberturas parciais, o novo ramo fecha a porta que a classificação anterior admite para o resto do dia. `app/ponto/supra_juiz.py:339–347`, `app/ponto/supra_juiz.py:472–485`, `app/ponto/supra_juiz.py:504–512`.

**Censo.** `Turno.aberto` é escrito pelo pareador, a lista de S órfãs lê o `Turno` com entrada ausente e saída presente, e `sem_par_do_pareador` agrega ambos em `date: (bool E, bool S)`. É dia-turno atribuído no fuso local Django, inclusive véspera no noturno, e os minutos são inteiros. As quatro chamadas de produção usam essa semântica, mas o cartório suprime os dois flags antes da chamada se o turno está em curso, enquanto os comandos passam o resultado bruto pós-horizonte com `turno_encerrado` separado. As contagens de `batidas_no_dia` que os acompanham diferem, dia civil nos comandos e dia-turno no cartório. `app/ponto/turnos.py:43–48`, `app/ponto/turnos.py:234–245`, `app/ponto/turnos.py:285–324`, `app/config/settings/base.py:93`, `app/ponto/supra_juiz.py:88–105`, `app/ponto/services/cartorio.py:1223–1235`, `app/ponto/services/cartorio.py:1349–1355`, `app/ponto/services/cartorio.py:1501–1508`, `app/ponto/services/cartorio.py:1553–1565`, `app/ponto/management/commands/supra_juiz.py:225–239`, `app/ponto/management/commands/emitir_furo_retroativo.py:103–116`.

**Censo.** As duas chamadas de produção de `impressao_insumos` estão no cartório. A normal passa `E/S/ES` ou `''`. A sem adesão usa o default `''` no hash, embora entregue flags ao classificador se passar da guarda, e vem de ausência de primeira batida apurável. O token entra no SHA-256 e só o digest é persistido, por `carimbar`. O comando de snapshot lê o digest. `HORIZONTE_SEM_PAR` é constante validada `2026-11`, com único leitor funcional na comparação da competência pelo fim da janela da empresa. Censo de testes com todas as linhas enviado à coordenação. `app/ponto/services/cartorio.py:316–337`, `app/ponto/services/cartorio.py:513–514`, `app/ponto/services/cartorio.py:598–603`, `app/ponto/services/cartorio.py:1283–1285`, `app/ponto/services/cartorio.py:1337–1355`, `app/ponto/services/cartorio.py:1480–1486`, `app/ponto/services/cartorio.py:1976–1977`, `app/ponto/portas/celula.py:1217–1237`, `app/ponto/management/commands/celula_veredito_velho.py:103–110`, `app/ponto/supra_juiz.py:105`, `app/ponto/janelas.py:24–33`, `app/ponto/janelas.py:60–63`.

## 24/11 02:09 · sessao afd447896a19 · - · - · origem=Ronald (tmux)

1. **Bloqueia — a janela muda o sinal e a decisão de pular.** O cron real roda sem `--forcar` e passa todas as células passadas. O avulso passa `[cel]`. A segunda passada do mesmo ato pode usar só as células reconciliadas. Cada corpo busca os turnos entre as pontas da sua lista, alimentando encerramento e sem-par com essa mesma carga. `app/config/crons.py:616`, `app/ponto/management/commands/processar_cartorio.py:59`, `app/ponto/services/cartorio.py:1073`, `app/ponto/services/cartorio.py:1188`, `app/ponto/services/cartorio.py:1208`, `app/ponto/services/cartorio.py:2143`.

   A janela do pareador chega a `fim+1`. Por leitura estática, com vínculo diurno 08–14, uma E em D às 23:30 e uma S em D+2 às 00:30, sem outras batidas nem papel de intervalo ou eco, dão `E` em `[D,D]`. Uma janela ampla do mesmo vínculo inclui a S e admite fechar em 25h pelo envelope de 26h, pois ambas as pontas estão a mais de três horas dos respectivos marcos. Depois da expiração, o avulso pode imprimir `sem_par:E` e o cron imprimir sem esse componente. A S em D+2 também fica de fora das batidas diretamente hasheadas de D. Com os demais insumos iguais, o cron pode pular um dia que o avulso rejulgaria, ou relavrar depois do avulso sem fato novo. Incidência real [nao verificado]. `app/ponto/turnos.py:1834`, `app/ponto/turnos.py:1867`, `app/ponto/turnos.py:1977`, `app/ponto/turnos.py:1430`, `app/ponto/turnos.py:1491`, `app/ponto/turnos.py:2005`, `app/ponto/services/cartorio.py:1439`, `app/ponto/services/cartorio.py:1480`, `app/ponto/services/cartorio.py:1491`, `app/docs/LEIS.md:122` (L-099).

2. **Bloqueia — duas respostas do pareador na mesma lavra.** O sinal/encerramento lê `turnos_do_colab(c,d0,d1)` com a ata persistida. O realizado da grade compara o mesmo dia usando `turnos_do_colab` com janela que estende `ref` a `d1+1` e cópia em memória da ata desta montagem. A própria grade explica por que ler papel velho já produziu minuto diferente para as mesmas batidas. É a mesma autoridade com alimentações distintas. `app/ponto/services/cartorio.py:1213`, `app/ponto/services/cartorio.py:1253`, `app/ponto/turnos.py:1889`, `app/ponto/turnos.py:1944`, `app/escala/utils.py:1873`, `app/escala/utils.py:1889`, `app/escala/utils.py:1909`, `app/escala/utils.py:2056`.

3. **Bloqueia — o horizonte permite a nova regra em exportada.** O piso `2026-11` compara a competência derivada do corte da empresa. Uma 11, 12 ou posterior com TXT vigente passa pelo piso. O juiz de entrega K6 existe. A guarda temporária da 09 age no recálculo de `FechamentoMensal`/`DiaPago`, sem proteger o cartório. O teste antigo da 09 distingue ata relavrável de FM/TXT intactos. O selo novo declara que não prova a guarda da exportada. L-092 e L-113 coexistem com o recorte particular da obra, e a regra para frente está em L-124. `app/ponto/services/cartorio.py:316`, `app/ponto/services/cartorio.py:333`, `app/ponto/supra_juiz.py:105`, `app/folha/export.py:556`, `app/ponto/services/fechamento.py:27`, `app/ponto/services/fechamento.py:558`, `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:113`, `app/ponto/tests/test_a_tela_sem_par_acusa.py:230`, `app/docs/LEIS.md:125`, `app/docs/LEIS.md:145`, `app/docs/LEIS.md:155`.

4. **Bloqueia — o protesto novo pode parar folha e entrar na fila de cobrança sem marco cobrável.** O emissor parcial precisa de marco apagado nomeado. A fila fora do diff inclui `FURO_PARCIAL` no universo cobravel e força relavra avulsa, que pode devolver `sem_cobranca_depois`. A pauta existente para os dias sem fio exige `discordante`, enquanto a nova lavra classifica `furo`. A folha retém `espelho_furo` sob as guardas usuais, pois a exceção L-033 exige pelo menos uma pausa apagada. Incidência real [nao verificado]. `app/ponto/services/cartorio.py:1916`, `app/ponto/services/cartorio.py:2032`, `app/ponto/services/furos_sem_cobranca.py:35`, `app/ponto/services/furos_sem_cobranca.py:82`, `app/ponto/services/furos_sem_cobranca.py:117`, `app/ponto/services/pautas_dos_discordantes.py:27`, `app/ponto/services/pautas_dos_discordantes.py:40`, `app/escala/models.py:1623`, `app/folha/export.py:1150`, `app/folha/export.py:1293`, `app/docs/LEIS.md:81`.

5. **Fila — relógio e ordem.** O comando `supra_juiz` conserva a leitura implícita de `now()` dentro de `dias_encerrados`, agora após a carga explicitada pelo diff. Retroativo passa `agora` da borda, mas ambos leem `localdate()` por linha. No cartório, o novo ramo segue o encerramento por fato/instante conforme L-073. A fila de 14:16 punha PAR-PELO-TURNO à frente. O aval posterior de 23:55 autoriza construção paralela e pousos separados. O V6 prevê mudar o teto previsto+4h, que ainda rege esta cópia. Ordem efetiva de pouso [nao verificado]. `app/ponto/management/commands/supra_juiz.py:122`, `app/ponto/management/commands/supra_juiz.py:235`, `app/ponto/management/commands/emitir_furo_retroativo.py:47`, `app/ponto/management/commands/emitir_furo_retroativo.py:113`, `app/ponto/turnos.py:2121`, `app/ponto/turnos.py:2148`, `app/docs/LEIS.md:100`, `app/docs/LEIS.md:150`, `app/docs/CORTES.md:630`, `app/docs/PROMPTS.md:2852`, `app/docs/PROMPTS.md:2863`.

## 24/11 02:09 · sessao 53ef3fdd98b9 · - · - · origem=Ronald (tmux)

1. **Bloqueia — retenção sem cobrança correspondente.** O novo `FURO_PARCIAL` passa a representar tanto marco apagado quanto despareamento sem `missing`. Ambos lavram `furo` pela mesma porta. A folha retém o segundo por `espelho_furo`, pois sua exceção exige uma pausa apagada. A emissão direta e a fábrica de perguntas, por sua vez, precisam de marco apagado nomeável. O vigia inclui o novo caso entre os furos cobrados e pode apurá-lo como `sem_cobranca_depois` mesmo após rejulgamento. A mudança afeta a elegibilidade do TXT, prévia e lista de retenção. Impacto em produção [nao verificado]. `app/ponto/supra_juiz.py:451–461,504–512`, `app/escala/models.py:1622–1627`, `app/ponto/services/cartorio.py:1916–1934,1953–1977,2032–2041`, `app/ponto/portas/celula.py:1096–1105,1126–1134`, `app/folha/export.py:968–972,1148–1155,1173,1285–1294`, `app/folha/previa.py:170–171`, `app/folha/fora_da_folha.py:120–121`, `app/chamados/services/fabrica_da_celula.py:52–74`, `app/ponto/services/furos_sem_cobranca.py:35,53–62,82–108,117–131`.

2. **Bloqueia — `missing == 0` não equivale à lâmpada acesa.** A condição aceita coleção ausente/vazia e ignora entradas `None`. A própria montagem da ata pode lavrar `acesa=False` ou `None` se falta coluna para o marco do DNA. Esse caminho também permite que o novo código interaja com a L-033 apesar de a condição aparentar excluir todo marco apagado. Para todas as lâmpadas realmente acesas, retenção é comprovável, mas esse caso não satisfaz a premissa da exceção L-033. A mudança abre incompatibilidade com a L-088, que liga furo a marco não cumprido e pergunta em aberto, embora o corte específico mande a lavra acusar o despareamento. `app/ponto/supra_juiz.py:340–341,504–512`, `app/ponto/services/cartorio.py:685–728`, `app/docs/LEIS.md:81,120`.

3. **Bloqueia — os leitores existentes reagem também sem nova pergunta.** O C6 lê um veredito acusador como vida nos degraus indicados, e c2 admite o código persistido como condição de emissão de uma pergunta elegível já viva, submetida aos demais gates. `REALIZADO_ZERO_COM_TURNO` pode coexistir com o novo furo no próprio fixture E com zero, e `furo` tem precedência sobre `discordante`. Assim, o dia pode sair da Pauta DP dos discordantes e a pauta existente pode ser fechada sem que o emissor de marco tenha aberto uma cobrança. `app/chamados/juizes.py:1539–1562`, `app/chamados/reconciliador.py:928–963,979–1004`, `app/ponto/supra_juiz.py:316–319,504–512`, `app/ponto/tests/test_a_tela_sem_par_acusa.py:34–36,51–53`, `app/escala/models.py:1622–1627`, `app/ponto/services/pautas_dos_discordantes.py:27–31,39,76–81,196–213`.

4. **Fila — status, calendários e contadores.** Trabalho com `furo` vira `irregular` no D4 após o corte do relógio. Calendário, relatório de furos, painel e app passam a mostrar o novo estado, e o app traduz `irregular` em `alerta`. A palavra lateral do calendário ainda usa turno aberto/S órfã e pode dizer `Em aberto` no dia cheio mesmo se o D4 lavrou `ok`. L-002/L-061/L-099 amparam a divergência residual quando se pede a mesma resposta. Em dia insuficiente, `irregular` e `Em aberto` podem coexistir coerentemente. O C4 do afastado também lê o novo conjunto de protestos. `app/ponto/status_dia.py:30–32,68,79–91,154–161,273–289`, `app/colaboradores/services/calendario.py:595–616,635–640,748–763`, `app/ponto/services/dia_decidido.py:346–355,419–430`, `app/templates/colaboradores/partials/_calendario_grade.html:68,98–104`, `app/relatorios/furos.py:119–127`, `app/core/services/painel_op.py:325–336`, `app/api/views.py:1493–1514`, `app/escala/services/cadastro_realidade.py:591–623`, `app/docs/LEIS.md:60,95,122`.

5. **Bloqueia — ordem e espúria.** O corte vigente coloca PAR-PELO-TURNO antes da A-TELA. A mesma cópia ainda traz teto fixo de 240 minutos em caminhos do pareador, enquanto o corte PAR-PELO-TURNO prevê cadastro por empresa e permite o teto antigo provisoriamente. Pouso anterior ou autorização posterior [nao verificado]. A exceção do cheio somente cala o novo ramo. A pergunta de espúrias consta em `SEM_JUIZ` e o fluxo de cluster exige ao menos duas órfãs e uma lâmpada apagada, portanto não trata pela regra demonstrada a E única do dia cheio com marcos acesos. `app/docs/CORTES.md:629–630`, `app/ponto/turnos.py:602,731,768`, `app/ponto/supra_juiz.py:499–512`, `app/core/juizes.py:1223–1225`, `app/ponto/services/cluster_espurio.py:15–36`.

6. **Fila — alcance das demais leis e dos comandos.** L-039 preservada no status, que distingue `irregular` de falta. L-102/L-103 tratam de dia sem turno/valor pareado e o diff lê os minutos da grade. A prova de reconstrução de projeções do pouso, L-121, e de impacto em dinheiro pela L-094 e 7b [nao verificado]. `emitir_furo_retroativo` segue emitindo apenas em `FURO_SEM_COBRANCA`; `supra_juiz` usa o código novo para placar/exemplos, não no `--alarme` nem para decidir a escrita da retratação, que lê a lavra. `app/docs/LEIS.md:87,112,128–129,142`, `CLAUDE.md:413–429`, `app/ponto/status_dia.py:139–160`, `app/ponto/supra_juiz.py:257–259,504`, `app/ponto/management/commands/emitir_furo_retroativo.py:104–117`, `app/ponto/management/commands/supra_juiz.py:225–249,251–292,309–315`.

## 24/11 01:35 · sessao 50bc8a92b541 · NOTURNO-NAO-APURADO · sonda · origem=Code

## 1. Universo e empresa × competência

`sonda/anexos` conta as linhas da saída atual a partir de `sonda_leitura`, incluindo a linha em branco. `sonda_inv.r` conta as linhas do anexo anterior a partir do primeiro título.

**UNIVERSO** `pares_fechados 16316 invertidos 25 colabs 18 min_brutos 4370`. `sonda/anexos:3`

As linhas `COLAPSADO` registram os fechados com pontas efetivas iguais. O bloqueio do dia sem previsto produz essa igualdade. `sonda/anexos:4–6` · `app/ponto/motor_calculo_v2.py:1471–1483`

| Empresa | Competência | Fechados | Invertidos | Colabs | Minutos brutos | Fonte |
|:---|:---|---:|---:|---:|---:|:---|
| COLAPSADO | 10/2026 | 57 | 0 | 0 | 0 | `sonda/anexos:4` |
| COLAPSADO | 11/2026 | 77 | 0 | 0 | 0 | `sonda/anexos:5` |
| COLAPSADO | 12/2026 | 15 | 0 | 0 | 0 | `sonda/anexos:6` |
| 1 | 10/2026 | 12 | 0 | 0 | 0 | `sonda/anexos:7` |
| 1 | 11/2026 | 14 | 0 | 0 | 0 | `sonda/anexos:8` |
| 2 | 10/2026 | 6038 | 6 | 6 | 692 | `sonda/anexos:9` |
| 2 | 11/2026 | 6337 | 10 | 10 | 1846 | `sonda/anexos:10` |
| 2 | 12/2026 | 275 | 1 | 1 | 98 | `sonda/anexos:11` |
| 3 | 10/2026 | 1478 | 4 | 3 | 769 | `sonda/anexos:12` |
| 3 | 11/2026 | 1408 | 2 | 1 | 483 | `sonda/anexos:13` |
| 3 | 12/2026 | 61 | 0 | 0 | 0 | `sonda/anexos:14` |
| 4 | 10/2026 | 276 | 2 | 1 | 482 | `sonda/anexos:15` |
| 4 | 11/2026 | 258 | 0 | 0 | 0 | `sonda/anexos:16` |
| 4 | 12/2026 | 10 | 0 | 0 | 0 | `sonda/anexos:17` |

A saída fornecida termina em `rc_sombra=0`, sem traceback no trecho. `sonda/anexos:1–30`

## 2. Exemplos e ramo pelas pontas

A classificação indica quais pontas mudaram pela janela de HE. O motor aplica os recortes de entrada e saída separadamente e grava as pontas resultantes. `app/ponto/motor_calculo_v2.py:1691–1712,1731–1763,1834–1842`

| Colab | Comp. | Entrada bruta | Saída bruta | Entrada efetiva | Saída efetiva | `trab` | `fora` | `not_leg` | Ramo | Fonte |
|---:|:---|:---|:---|:---|:---|---:|---:|---:|:---|:---|
| 37 | 10/2026 | `2026-10-06 03:00:38` | `2026-10-06 07:01:01` | `2026-10-06 19:00:00` | `2026-10-06 07:00:00` | 0 | 960.4 | 0.0 | HE nas duas pontas | `sonda/anexos:18` · `sonda_inv.r:27` |
| 37 | 10/2026 | `2026-10-10 03:00:40` | `2026-10-10 07:01:50` | `2026-10-10 19:00:00` | `2026-10-10 07:00:00` | 0 | 961.2 | 0.0 | HE nas duas pontas | `sonda/anexos:19` · `sonda_inv.r:28` |
| 109 | 10/2026 | `2026-10-05 22:00:41` | `2026-10-05 22:10:41` | `2026-10-05 22:00:41` | `2026-10-05 22:00:00` | 0 | 10.7 | 0.0 | HE só na saída | `sonda/anexos:20` · `sonda_inv.r:34` |
| 113 | 11/2026 | `2026-10-31 03:00:00` | `2026-10-31 07:02:00` | `2026-10-31 19:00:00` | `2026-10-31 07:00:00` | 0 | 962.0 | 0.0 | HE nas duas pontas | `sonda/anexos:21` · `sonda_inv.r:36` |
| 113 | 11/2026 | `2026-11-02 03:00:00` | `2026-11-02 07:01:00` | `2026-11-02 19:00:00` | `2026-11-02 07:00:00` | 0 | 961.0 | 0.0 | HE nas duas pontas | `sonda/anexos:22` · `sonda_inv.r:37` |
| 174 | 10/2026 | `2026-09-24 02:32:11` | `2026-09-24 05:00:58` | `2026-09-24 21:00:00` | `2026-09-24 05:00:00` | 0 | 1108.8 | 0.0 | HE nas duas pontas | `sonda/anexos:23` |
| 174 | 11/2026 | `2026-10-25 02:32:15` | `2026-10-25 04:57:35` | `2026-10-25 21:00:00` | `2026-10-25 04:57:35` | 0 | 1107.7 | 0.0 | HE só na entrada | `sonda/anexos:24` |
| 200 | 11/2026 | `2026-11-03 02:00:33` | `2026-11-03 06:00:15` | `2026-11-03 22:00:00` | `2026-11-03 06:00:00` | 0 | 1199.7 | 0.0 | HE nas duas pontas | `sonda/anexos:25` |
| 218 | 10/2026 | `2026-10-16 03:15:06` | `2026-10-16 05:00:01` | `2026-10-16 21:00:00` | `2026-10-16 05:00:00` | 0 | 1064.9 | 0.0 | HE nas duas pontas | `sonda/anexos:26` |
| 218 | 11/2026 | `2026-11-15 03:17:11` | `2026-11-15 05:00:03` | `2026-11-15 21:00:00` | `2026-11-15 05:00:00` | 0 | 1062.9 | 0.0 | HE nas duas pontas | `sonda/anexos:27` |
| 218 | 12/2026 | `2026-11-22 03:22:03` | `2026-11-22 05:00:16` | `2026-11-22 21:00:00` | `2026-11-22 05:00:00` | 0 | 1058.2 | 0.0 | HE nas duas pontas | `sonda/anexos:28` |
| 296 | 11/2026 | `2026-11-03 00:15:19` | `2026-11-03 07:00:41` | `2026-11-03 19:00:00` | `2026-11-03 07:00:00` | 0 | 1125.4 | 0.0 | HE nas duas pontas | `sonda/anexos:29` |

## 3. Destino dos 4.370 minutos brutos

- **No próprio par invertido, zero em `minutos_trabalhados`.** O motor mede `saída efetiva − entrada efetiva` e grava `max(0, duração)`. `minutos_fora_janela` recebe a soma dos recortes, que pode superar a duração bruta do par. O teto de pontualidade lê esse campo separadamente. `sonda/anexos:3,18–29` · `app/ponto/motor_calculo_v2.py:2759–2787,2886–2895,3224–3258,1170–1196`
- **O crédito do aberto pula candidatos cuja entrada bruta já consta em um fechado de `resultado.periodos`, inclusive se o fechado tem zero trabalhado.** A guarda usa `resultado.periodos`, enquanto os fechados de folga ficam em `resultado.periodos_ft`. Para cada candidato aceito, o motor roda `calcular_periodo` e credita os campos retornados separadamente. Se o candidato também sai invertido, acrescenta zero ao trabalhado e pode acrescentar minutos ao `fora_janela`. `app/ponto/motor_calculo_v2.py:2492–2494,2513–2560,2759–2787,2886–2895,3224–3258`
- **Outro par contribui com seu próprio `minutos_trabalhados`.** A soma mensal e a lavratura leem esse campo dos períodos. `DiaPago.minutos_realizados` recebe o mapa da grade por dia. Se algum dos 4.370 minutos brutos aparece sob outro pareamento, no crédito de um candidato com outras pontas ou no realizado da grade, é [nao verificado]. `app/ponto/motor_calculo_v2.py:294–300,2549–2560` · `app/ponto/services/dia_pago.py:173–179,234–267,285–292`

## 24/11 01:31 · sessao bf4a511fb0a8 · NOTURNO-NAO-APURADO · sonda · origem=Code

## 1. Universo e empresa × competência

As referências `sonda/anexos:N` numeram o trecho fornecido a partir de `sonda_leitura`, contando a linha em branco.

**UNIVERSO** `pares_fechados 16167 invertidos 174 colabs 117 min_brutos 81266`. `sonda/anexos:3`

| Empresa | Competência | Fechados | Invertidos | Colabs | Minutos brutos | Fonte |
|---:|:---|---:|---:|---:|---:|:---|
| 1 | 10/2026 | 12 | 2 | 1 | 968 | `sonda/anexos:4` |
| 1 | 11/2026 | 14 | 4 | 2 | 615 | `sonda/anexos:5` |
| 2 | 10/2026 | 6038 | 54 | 46 | 24762 | `sonda/anexos:6` |
| 2 | 11/2026 | 6337 | 74 | 59 | 34599 | `sonda/anexos:7` |
| 2 | 12/2026 | 275 | 14 | 14 | 7866 | `sonda/anexos:8` |
| 3 | 10/2026 | 1478 | 11 | 10 | 4920 | `sonda/anexos:9` |
| 3 | 11/2026 | 1408 | 9 | 7 | 4567 | `sonda/anexos:10` |
| 3 | 12/2026 | 61 | 1 | 1 | 720 | `sonda/anexos:11` |
| 4 | 10/2026 | 276 | 2 | 1 | 482 | `sonda/anexos:12` |
| 4 | 11/2026 | 258 | 2 | 2 | 1022 | `sonda/anexos:13` |
| 4 | 12/2026 | 10 | 1 | 1 | 746 | `sonda/anexos:14` |

As linhas somam **81267 minutos brutos**, um a mais que os **81266** do `UNIVERSO`. A origem da diferença é [nao verificado]. A saída fornecida termina em `rc_sombra=0`. O trecho da sonda não contém traceback. `sonda/anexos:1–27`

## 2. Exemplos e ramo das pontas efetivas

| Colab | Comp. | Entrada bruta | Saída bruta | Entrada efetiva | Saída efetiva | `trab` | `fora` | `not_leg` | Ramo pela leitura das pontas | Fonte |
|---:|:---|:---|:---|:---|:---|---:|---:|---:|:---|:---|
| 37 | 10/2026 | `2026-10-06 03:00:38` | `2026-10-06 07:01:01` | `2026-10-06 19:00:00` | `2026-10-06 07:00:00` | 0 | 960.4 | 0.0 | HE nas duas pontas | `sonda/anexos:15` |
| 37 | 10/2026 | `2026-10-10 03:00:40` | `2026-10-10 07:01:50` | `2026-10-10 19:00:00` | `2026-10-10 07:00:00` | 0 | 961.2 | 0.0 | HE nas duas pontas | `sonda/anexos:16` |
| 37 | 11/2026 | `2026-11-15 02:00:18` | `2026-11-15 07:01:13` | `2026-11-15 02:00:18` | `2026-11-15 02:00:18` | 0.0 | 300.9 | 0.0 | Bloqueio do dia sem previsto | `sonda/anexos:17` |
| 50 | 12/2026 | `2026-11-22 07:00:48` | `2026-11-22 19:26:47` | `2026-11-22 07:00:48` | `2026-11-22 07:00:48` | 0.0 | 746.0 | 0.0 | Bloqueio do dia sem previsto | `sonda/anexos:18` |
| 55 | 11/2026 | `2026-11-11 14:00:21` | `2026-11-11 15:08:32` | `2026-11-11 14:00:21` | `2026-11-11 14:00:21` | 0.0 | 68.2 | 0.0 | Bloqueio do dia sem previsto | `sonda/anexos:19` |
| 61 | 11/2026 | `2026-11-08 07:02:51` | `2026-11-08 19:00:01` | `2026-11-08 07:02:51` | `2026-11-08 07:02:51` | 0.0 | 717.2 | 0.0 | Bloqueio do dia sem previsto | `sonda/anexos:20` |
| 95 | 10/2026 | `2026-10-10 18:59:36` | `2026-10-11 06:55:15` | `2026-10-10 18:59:36` | `2026-10-10 18:59:36` | 0.0 | 655.6 | 0.0 | Bloqueio do dia sem previsto | `sonda/anexos:21` |
| 109 | 10/2026 | `2026-10-05 22:00:41` | `2026-10-05 22:10:41` | `2026-10-05 22:00:41` | `2026-10-05 22:00:00` | 0 | 10.7 | 0.0 | HE só na saída | `sonda/anexos:22` |
| 110 | 11/2026 | `2026-11-13 01:59:07` | `2026-11-13 07:00:02` | `2026-11-13 01:59:07` | `2026-11-13 01:59:07` | 0.0 | 300.9 | 0.0 | Bloqueio do dia sem previsto | `sonda/anexos:23` |
| 113 | 11/2026 | `2026-10-31 03:00:00` | `2026-10-31 07:02:00` | `2026-10-31 19:00:00` | `2026-10-31 07:00:00` | 0 | 962.0 | 0.0 | HE nas duas pontas | `sonda/anexos:24` |
| 113 | 11/2026 | `2026-11-02 03:00:00` | `2026-11-02 07:01:00` | `2026-11-02 19:00:00` | `2026-11-02 07:00:00` | 0 | 961.0 | 0.0 | HE nas duas pontas | `sonda/anexos:25` |
| 114 | 10/2026 | `2026-10-12 09:58:37` | `2026-10-12 17:10:11` | `2026-10-12 09:58:37` | `2026-10-12 09:58:37` | 0.0 | 431.6 | 0.0 | Bloqueio do dia sem previsto | `sonda/anexos:26` |

**HE nas duas pontas, colabs 37 e 113.** O ponto que ancora a entrada às 19h do mesmo dia civil é `_janela_do_dia`, que faz `dia = entrada.date()` e consulta `_md(dia)`. Em seguida, `entrada_efetiva` monta o marco com `marco_no_dia(entrada, hi)`, que troca a hora e preserva a data da batida. O motor calcula `data_jornada` em separado e o usa para a folga e a atribuição do período. O `data_jornada` efetivamente atribuído a cada exemplo é [nao verificado]. `sonda/anexos:15–16,24–25,47` · `app/ponto/motor_calculo_v2.py:1520–1542,1676–1712,2400–2408,2445–2461` · `app/ponto/janela_he.py:20–30,94–103`

**Condição do recorte.** A janela precisa estar ativa e vigente, o par precisa passar pela guarda de folga e pela L-084, e o dia consultado pela janela precisa estar sem decisão `autorizado`. A entrada é recortada quando a antecedência, descontada a extra declarada, supera o piso. Para a saída, a janela de saída precisa estar ativa, com marco final, e o excesso depois dele, descontada a extra declarada, também precisa superar o piso. O marco final nasce na data civil da saída e só avança um dia se o turno cruza meia-noite e o marco ficaria antes da **entrada bruta**. Com marcos de 19h e 07h, essa comparação mantém 07h no dia da batida de saída nos quatro exemplos. Os valores cadastrais de marcos, piso, extra declarada e decisão são [nao verificado]. `sonda/anexos:15–16,24–25` · `app/ponto/motor_calculo_v2.py:1389–1395,1520–1550,1641–1653,1691–1712,1731–1763` · `app/ponto/janela_he.py:41–51,73,94–103,145–153`

**Guarda de 180 min.** A L-084 dispensa a janela quando **as duas** distâncias absolutas superam 180 min. Com marcos 19h e 07h, as quatro entradas brutas estão de 15h59min20s a 16h do marco, e as saídas brutas estão de 1min01s a 2min do marco. Assim, a guarda **do par** deixa a janela passar. Há uma segunda guarda com as pontas externas do dia do juiz. As pontas e os marcos usados nessa segunda guarda são [nao verificado]. `sonda/anexos:15–16,24–25,47` · `app/ponto/motor_calculo_v2.py:1559–1571,1582–1606,1623–1629,1931–1949`

**HE só na saída, colab 109.** A entrada permanece `22:00:41` e `_saida_efetiva` recorta a saída para `22:00:00`, deixando-a 41 segundos antes da entrada. A condição é excesso de saída acima do piso após a extra declarada, com a janela de saída ativa, marco final disponível e sem autorização. O motivo específico pelo qual a entrada ficou intacta é [nao verificado]. `sonda/anexos:22` · `app/ponto/motor_calculo_v2.py:1691–1712,1731–1750` · `app/ponto/janela_he.py:145–153`

**Bloqueio do dia sem previsto, sete exemplos com pontas iguais.** A saída efetiva vira uma cópia da entrada efetiva, inclusive os segundos. Isso identifica o ramo na cópia, pois o recorte de saída da janela devolve um marco de minuto inteiro deslocado por extra declarada em minutos inteiros. `_dia_sem_previsto_fora_da_janela` exige trabalho previamente apurado, folga ou feriado, janela vigente, previsto igual a zero e ausência de autorização. Em folga com colaborador identificado, exige também escala certa. O ramo move o trabalho apurado para `minutos_fora_janela`, zera trabalho e noturno e iguala as pontas. Os valores das guardas por exemplo, inclusive se era folga ou feriado, são [nao verificado]. `sonda/anexos:17–21,23,26,49` · `app/ponto/motor_calculo_v2.py:1355–1362,1440–1483,2457–2461` · `app/ponto/janela_he.py:20–30,145–153`

## 3. Destino dos minutos brutos

- **No próprio par, a contribuição para `minutos_trabalhados` é zero.** Na inversão pela janela, os três corpos de `calcular_periodo` calculam a duração pelas pontas efetivas e gravam `max(0, duracao)`. No bloqueio do dia sem previsto, o motor zera o campo depois de calcular o par. `minutos_fora_janela` recebe as sobras do recorte ou o trabalho apurado antes do bloqueio. Na inversão estrita, as sobras podem somar mais que a duração bruta, como mostram os quatro recortes para `19:00 → 07:00`. A L-093 usa esse campo no teto de pontualidade. `sonda/anexos:15–16,24–25` · `app/ponto/motor_calculo_v2.py:1170–1196,1471–1483,2759–2787,2886–2895,3224–3258`

- **O crédito do aberto não repassa o bruto do fechado.** Ele monta candidatos a partir dos turnos do juiz. Se a entrada bruta já pertence a um fechado em `resultado.periodos`, pula o candidato mesmo quando o fechado tem `trab=0`. `_pagos_a` é formado só por `resultado.periodos`. Para um candidato que passa, o motor chama `calcular_periodo` e soma separadamente o `minutos_trabalhados` e o `minutos_fora_janela` retornados. Se esse recálculo também inverter as pontas, só pode carregar a parcela de fora da janela. A existência de candidato separado ou sobreposto nos casos da sonda é [nao verificado]. `sonda/anexos:15–26,39` · `app/ponto/motor_calculo_v2.py:2492–2494,2513–2560,2759–2787,2886–2895,3224–3258`

- **Outro par entra na soma pelo próprio valor apurado.** O total mensal soma `p.minutos_trabalhados`, e a lavra faz a soma por dia com esse mesmo campo. `DiaPago.minutos_realizados` vem por um mapa separado da grade. Os **81266** são o bruto publicado pela sonda. Quanto desse tempo aparece em outro par, no realizado da grade ou na folha lavrada é [nao verificado]. `sonda/anexos:3,15–26` · `app/ponto/motor_calculo_v2.py:294–300` · `app/ponto/services/dia_pago.py:173–179,237–267,285–292`

## 24/11 01:29 · sessao 2f08c8a501dd · E4C-F1-B2b-F0 · verificador · origem=Code

**1. NAO PROCEDE para o contrato inteiro.** As três correções pontuais funcionam, mas o rollback ainda deixa exceções descobertas. `app/chamados/services/fio.py:1508`, `app/chamados/models.py:3463`, `app/chamados/models.py:3509`

- **A — PROCEDE.** A lista deixa `StopIteration` subir da guarda. O gerador do diff a convertia em `RuntimeError`. Com campos já carregados, a ordem segue gravadas → empresa → veto. Num lote com dia diferido, a lista lê o dia da segunda pergunta antes de consultar a empresa da primeira, enquanto o gerador intercalava essas leituras. A caracterização registra contagens, e a comparação direta da ordem e da exceção com `327da9db7` fica [nao verificado]. `app/chamados/services/fio.py:1508`, `app/chamados/services/fio.py:1522`, `app/chamados/services/fio.py:1544`, `app/chamados/juizes.py:339`, `app/chamados/models.py:3826`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:179`
- **2 — PROCEDE para a falha do julgador.** O `except` restaura carimbo, resposta, validação e os três campos da lavra. Em `RETIDA`, a porta restaura os campos da tentativa, mantém a lavra anterior e registra `RECUSA_RETIDA`. Ainda escapam do rollback uma falha em `texto_vazio`, no import de `veredito_lavrado` ou em `foto_da_lavra`, todos depois da mutação. Uma falha injetada na foto deixou o carimbo tentado. O rollback também descarta `_pv_memo` e o reset inicial de `recusa_do_carimbo` não restaura o valor anterior em caso de exceção. `app/chamados/models.py:3422`, `app/chamados/models.py:3435`, `app/chamados/models.py:3463`, `app/chamados/models.py:3482`, `app/chamados/veredito_lavrado.py:279`
- **4c — PROCEDE.** A identidade sobrescreve a recusa com `RECUSA_RETIDA` antes de tocar os campos do carimbo. Com o coletor desligado, o julgador continua retornando `False` após uma leitura do `ContextVar`, sem consulta. Em `carimbar`, a foto da lavra agora vem antes dessa leitura. `app/chamados/models.py:3509`, `app/chamados/models.py:3463`, `app/chamados/services/fio.py:1635`

**2. PROCEDE. Há um leitor indireto após `carimbar_identidade`.**

- **Escritores.** O atributo em memória tem default de classe `None`. `carimbar` o zera e escreve `RECUSA_FORA_DO_FATO`, `RECUSA_C6`, `RECUSA_PAR_ATO_COLUNA` ou `RECUSA_RETIDA`. `carimbar_identidade` escreve apenas `RECUSA_RETIDA`, nesse ramo. Um teste injeta `RECUSA_C6`. `app/chamados/models.py:3387`, `app/chamados/models.py:3422`, `app/chamados/models.py:3426`, `app/chamados/models.py:3451`, `app/chamados/models.py:3461`, `app/chamados/models.py:3470`, `app/chamados/models.py:3510`, `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:262`
- **Leitores no código de produção.** Quatro leituras vêm logo após `carimbar`. A quinta está em `_balde_da_recusa`. No caminho de colisão, `_materializar_uma` chama `_carimbar_recusa` no `finally`, e esta chama `carimbar_identidade(VIA_FATO_OCUPADO)`. O passivo então lê a mesma instância se a via não mudou e há erro. Se a identidade devolveu `RETIDA`, o leitor cai em `recusa:COLISAO`, pois só trata especialmente `RECUSA_PAR_ATO_COLUNA`. Ocorrência com o coletor ligado em produção [nao verificado]. `app/chamados/services/materializacao.py:1105`, `app/chamados/services/materializacao.py:2093`, `app/chamados/services/materializacao.py:2350`, `app/chamados/services/materializacao.py:2356`, `app/chamados/services/materializacao.py:1792`, `app/chamados/services/materializacao.py:2116`, `app/chamados/services/passivo_validado.py:151`, `app/chamados/services/passivo_validado.py:585`, `app/chamados/services/passivo_validado.py:593`
- **Leitores nos testes.** F9b lê diretamente após a identidade. As demais leituras estão em `app/chamados/tests/test_e4c_b2b_f0_espinha.py:311`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:402`, `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:455`, `app/chamados/tests/test_e4c_f5_volta3_ausencia.py:223`, `app/chamados/tests/test_e4c_f5_guarda_par_s143.py:59`, `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:134`, `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:78` e `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:149`. F9b: `app/chamados/tests/test_e4c_b2b_f0_espinha.py:323`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:328`

**3. PROCEDE.** Os dois testes mordem os defeitos nomeados. O primeiro abre a cerca, injeta `StopIteration` no auxiliar chamado pela guarda e exige essa classe. O segundo injeta a falha do julgador e compara instante, autor e via. Com o julgador na posição antiga, a exceção subiria com o trio tentado ainda na instância. Esse segundo teste cobre o trio. A cobertura de validação e lavra está nos testes anteriores. F9b também falharia sem a nova atribuição, embora parta de `None` em vez de semear uma recusa anterior. Execução da suíte Django [nao verificado]. `app/chamados/tests/test_e4c_b2b_f0_espinha.py:221`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:234`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:314`, `app/chamados/models.py:3435`, `app/chamados/models.py:3465`, `app/chamados/tests/test_o261e_f2_fila.py:186`

**4. PROCEDE. E e 4b merecem ressalva na triagem.**

- **E.** O helper `engolir` já existia, mas a captura da falha **na leitura** é nova no ramo `alvo`. No legado, `.first()` pode propagar. No item, a falha de leitura vira rastro e `False` após o savepoint. O censo da cópia aponta zero chamadores diretos com `alvo`. `app/chamados/reconciliador.py:505`, `app/chamados/reconciliador.py:525`, `app/core/observ.py:86`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:638`
- **4b.** A fixture herdada lê o relógio real antes do `freeze_time` dos métodos. `setUpTestData` cria o vínculo, cujo `post_save` chama `invalidar_previsto`, que avalia `timezone.now()`. Efeito nas asserções [nao verificado]. `app/chamados/tests/test_e4c_b2_cerca_retratar.py:116`, `app/chamados/tests/_vinculo_do_dia.py:25`, `app/escala/signals.py:137`, `app/ponto/services/fechamento.py:1607`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:223`
- **3, E′, 4a e F — SEM ACHADO adicional.** `A,B,A` segue o contrato de registrar mudanças. A corrida continua possível entre leitura e escrita e já está anotada para a ativação. O ramo aceita `modelo` genérico, com dois modelos exercitados e exigência de lista fechada [nao verificado]. O ponto cego de `getattr` e `**kwargs` é real, com uso efetivo [nao verificado]. As tags de M1 e CERCA são compartilhadas dentro de cada caso, e o teste exige `AssertionError` com a tag esperada. O resultado de **H** consta do anexo para `4346eddae`. Confirmação independente e resultado no delta [nao verificado]. `app/chamados/reconciliador.py:468`, `app/chamados/reconciliador.py:519`, `app/chamados/reconciliador.py:525`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:473`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:521`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:538`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:583`

## 24/11 01:29 · sessao 68876d82a44d · - · - · origem=Ronald (tmux)

**4. PROCEDE em E e 4b, com alcance limitado.**

- **E — PROCEDE.** O ramo de item introduz uma captura na leitura de deduplicação. A leitura sem `alvo` pode propagar a exceção. Com `alvo`, `engolir` captura, registra e devolve `False`. O best-effort de `evento` já cobria a escrita. Autorização literal do corte para essa captura e efeito em produção [nao verificado]. `app/chamados/reconciliador.py:505-532`, `app/core/observ.py:73-105`.
- **4b — PROCEDE.** A fixture herdada cria um vínculo antes do `freeze_time(T0)`. O `post_save` desse vínculo chama `invalidar_previsto`, que avalia `timezone.now()`. Os corpos dos testes novos estão congelados. Dependência das asserções do horário real [nao verificado]. `app/chamados/tests/test_e4c_b2_cerca_retratar.py:116-128`, `app/chamados/tests/_vinculo_do_dia.py:25-26`, `app/escala/signals.py:137-146`, `app/ponto/services/fechamento.py:1607-1613`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:102,217-244`.
- **3 — SEM ACHADO adicional.** `A,B,A` dá três linhas por construção, conforme a regra de gravar as mudanças. A corrida continua possível porque a leitura precede o insert sem trava ou unicidade nessa chave. O censo local mostrou os seis chamadores diretos e nenhum passa `alvo`. Ocorrência concorrente em produção [nao verificado]. `app/chamados/reconciliador.py:466-482,525-531`, `app/core/models.py:285-316`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:620-640`.
- **E′ — SEM ACHADO.** O modelo do item é genérico, e o próprio teste usa `PerguntaDisputa` e `ChamadoColaborador`. Uma lista obrigatória de modelos [nao verificado]. `app/chamados/reconciliador.py:516-531`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:473-488`.
- **4a — SEM ACHADO adicional.** Confirmei em memória os dois pontos cegos. `getattr` com nome literal passa despercebido e o desempacotamento de `alvo` aparece como lista vazia de argumentos nomeados. Uso efetivo dessas formas para ativar a fatia [nao verificado]. `app/chamados/tests/test_e4c_b2b_f0_espinha.py:570-595,614-640`.
- **F — NAO PROCEDE como vacuidade.** Cada tag admite mais de uma asserção, porém o executor exige `AssertionError` com a tag no caso designado. Ligando o coletor em `M1`, a porta devolve uma recusa falsy e a primeira asserção falha. O mutante `CERCA` remove a guarda e provoca o veto esperado pelo caso. `app/chamados/tests/test_e4c_b2b_f0_espinha.py:55-61,91-93,341-354,410-422,521-545`, `app/chamados/reconciliador.py:408-409`.
- **H — SEM ACHADO.** O anexo informa o resultado para `4346eddae`. A execução histórica e o resultado da suíte no delta ficam [nao verificado].

## 24/11 01:28 · sessao fe9f81ea416b · - · - · origem=Ronald (tmux)

**2. PROCEDE.** `recusa_do_carimbo` é atributo em memória com default de classe. `carimbar` escreve `None`, `RECUSA_FORA_DO_FATO`, `RECUSA_C6`, `RECUSA_PAR_ATO_COLUNA` ou `RECUSA_RETIDA`. `carimbar_identidade` só escreve `RECUSA_RETIDA` no ramo retido. `app/chamados/models.py:3380`, `:3387`, `:3422`, `:3426`, `:3451`, `:3461`, `:3470`, `:3509–3521`.

- Leituras de produção após `carimbar`: `app/chamados/services/materializacao.py:1105`, `:2093`, `:2350`, `:2356`.
- Leitura indireta de produção após `carimbar_identidade`: `app/chamados/services/passivo_validado.py:151`. O laço entrega a mesma instância e a mesma lista de erros a `_materializar_uma` e `_balde_da_recusa`. Se houve colisão, `_materializar_uma` chama `_carimbar_recusa` no `finally`, que chama `carimbar_identidade(VIA_FATO_OCUPADO)`. Com `RETIDA` a via fica igual e o erro permanece. `_balde_da_recusa` lê o valor, compara com `RECUSA_PAR_ATO_COLUNA` e devolve a classificação do erro de colisão. Ocorrência com coletor ativo em produção [nao verificado]. `app/chamados/services/passivo_validado.py:147–154`, `:577–593`; `app/chamados/services/materializacao.py:1625–1629`, `:1782–1793`, `:2034–2042`, `:2106–2116`; `app/chamados/models.py:3509–3511`.
- Leitura direta após identidade no teste F9b: `app/chamados/tests/test_e4c_b2b_f0_espinha.py:323`, `:328`. Demais leituras de teste: `app/chamados/tests/test_e4c_b2b_f0_espinha.py:311`, `:402`; `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:455`; `app/chamados/tests/test_e4c_f5_volta3_ausencia.py:223`; `app/chamados/tests/test_e4c_f5_guarda_par_s143.py:59`; `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:134`; `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:78`; `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:149`. A injeção de `RECUSA_C6` em teste fica em `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:262`.

## 24/11 01:27 · sessao 86c298e2de0f · - · - · origem=Ronald (tmux)

**3. PROCEDE.** Os dois testes novos falham com os defeitos que nomeiam, por inspeção. O primeiro liga a cerca, injeta `StopIteration` no próprio auxiliar importado pela guarda e exige essa classe. Com o `yield` do diff anexado, a exceção escaparia como `RuntimeError`. [test_e4c_b2b_f0_espinha.py](/home/ronald/codex_ws/81c096c32aef53d72411b8327c8aace96c6fc429/app/chamados/tests/test_e4c_b2b_f0_espinha.py:221), [fio.py](/home/ronald/codex_ws/81c096c32aef53d72411b8327c8aace96c6fc429/app/chamados/services/fio.py:1506)

O segundo injeta `RuntimeError` em `reter_se_vetada` e exige que instante, autor e via voltem aos valores anteriores. Na posição antiga do julgador, o erro escapava depois da mutação e antes da restauração. O caso cobre o trio do carimbo. A cobertura da lavra e da validação está nos testes anteriores. [test_e4c_b2b_f0_espinha.py](/home/ronald/codex_ws/81c096c32aef53d72411b8327c8aace96c6fc429/app/chamados/tests/test_e4c_b2b_f0_espinha.py:234), [models.py](/home/ronald/codex_ws/81c096c32aef53d72411b8327c8aace96c6fc429/app/chamados/models.py:3432), [models.py](/home/ronald/codex_ws/81c096c32aef53d72411b8327c8aace96c6fc429/app/chamados/models.py:3465), [test_o261e_f2_fila.py](/home/ronald/codex_ws/81c096c32aef53d72411b8327c8aace96c6fc429/app/chamados/tests/test_o261e_f2_fila.py:186)

O F9b também falharia sem a atribuição nova. Ele parte de `None` e exige `RECUSA_RETIDA`. [test_e4c_b2b_f0_espinha.py](/home/ronald/codex_ws/81c096c32aef53d72411b8327c8aace96c6fc429/app/chamados/tests/test_e4c_b2b_f0_espinha.py:314), [models.py](/home/ronald/codex_ws/81c096c32aef53d72411b8327c8aace96c6fc429/app/chamados/models.py:3386) A execução dos testes nesta cópia está [nao verificado].

## 24/11 01:26 · sessao 4c298ef0e2f4 · NOTURNO-NAO-APURADO · sonda · origem=Code

## 1. Contagem por classe

As 17 classes somam **402 = C1**. Os **320** casos de `iguais_so_fechados` aparecem nas três subclasses com `motor_agora=0`. Para as demais classes, a saída não informa esse indicador. `sonda/anexos:3–20`

As referências a `sonda/anexos` contam a partir de `sonda_leitura`, a primeira linha do dado raspado. `sonda/anexos:1`

| Classe | Contagem | Fonte |
|---|---:|---|
| `aberto_ou_saida_orfa` | 1 | `sonda/anexos:4` |
| `aberto_ou_saida_orfa+quantidade_de_blocos` | 1 | `sonda/anexos:5` |
| `aberto_ou_saida_orfa+virada_ou_outro_dia` | 1 | `sonda/anexos:6` |
| `batida_orfa_na_celula+aberto_ou_saida_orfa+virada_ou_outro_dia` | 1 | `sonda/anexos:7` |
| `entrada_repetida+aberto_ou_saida_orfa+virada_ou_outro_dia` | 2 | `sonda/anexos:8` |
| `entrada_repetida+batida_orfa_na_celula+aberto_ou_saida_orfa` | 2 | `sonda/anexos:9` |
| `entrada_repetida+saida_apos_marco+aberto_ou_saida_orfa` | 1 | `sonda/anexos:10` |
| `entrada_repetida+saida_apos_marco+aberto_ou_saida_orfa+virada_ou_outro_dia+quantidade_de_blocos` | 1 | `sonda/anexos:11` |
| `entrada_repetida+saida_apos_marco+virada_ou_outro_dia` | 1 | `sonda/anexos:12` |
| `entrada_repetida+virada_ou_outro_dia` | 1 | `sonda/anexos:13` |
| `iguais_com_aberto` | 62 | `sonda/anexos:14` |
| `iguais_so_fechados\|bruto5-30\|motor_agora=0` | 55 | `sonda/anexos:15` |
| `iguais_so_fechados\|bruto<=5\|motor_agora=0` | 208 | `sonda/anexos:16` |
| `iguais_so_fechados\|bruto>30\|motor_agora=0` | 57 | `sonda/anexos:17` |
| `saida_apos_marco+aberto_ou_saida_orfa` | 1 | `sonda/anexos:18` |
| `saida_apos_marco+virada_ou_outro_dia` | 4 | `sonda/anexos:19` |
| `virada_ou_outro_dia` | 3 | `sonda/anexos:20` |
| **Soma** | **402** | `sonda/anexos:3–20` |

**Traceback.** Nenhum traceback consta no trecho fornecido. A última linha da saída é `rc_sombra=0`. `sonda/anexos:1–125`

## 2. Exemplos `fech` e corte da faixa noturna

A ordem de cada par é `[entrada, saida, efetivo_entrada, efetivo_saida, min_trab, min_fora_janela]`. O corte é avaliado contra a faixa noturna que o par bruto alcançava. O motor calcula o noturno pelas pontas efetivas e exige interseção positiva. `sonda/anexos:73–84` · [motor_calculo_v2.py:89](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:89) · [motor_calculo_v2.py:2792](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:2792)

### `iguais_so_fechados|bruto5-30|motor_agora=0`

| Colab / dia | Bruto / leg | Par | Cortou a faixa? | Fonte |
|---|---:|---|---|---|
| 108 / 2026-10-19 | 10.5 / 0.0 | `["2026-10-19 10:00:31", "2026-10-19 22:10:29", "2026-10-19 10:00:31", "2026-10-19 22:00:00", 657.9, 10.5]` | Sim, saída efetiva = 22:00 | `sonda/anexos:73` |
| 108 / 2026-10-21 | 6.3 / 0.0 | `["2026-10-21 09:59:32", "2026-10-21 22:06:16", "2026-10-21 10:00:00", "2026-10-21 22:00:00", 659.9, 6.7]` | Sim, saída efetiva = 22:00 | `sonda/anexos:74` |
| 108 / 2026-10-25 | 17.6 / 0.0 | `["2026-10-25 10:00:52", "2026-10-25 22:17:37", "2026-10-25 10:00:52", "2026-10-25 22:00:00", 658.1, 17.6]` | Sim, saída efetiva = 22:00 | `sonda/anexos:75` |
| 108 / 2026-11-16 | 15.6 / 0.0 | `["2026-11-16 09:59:13", "2026-11-16 22:15:37", "2026-11-16 10:00:00", "2026-11-16 22:00:00", 659.7, 16.4]` | Sim, saída efetiva = 22:00 | `sonda/anexos:76` |

### `iguais_so_fechados|bruto<=5|motor_agora=0`

| Colab / dia | Bruto / leg | Par | Cortou a faixa? | Fonte |
|---|---:|---|---|---|
| 60 / 2026-10-31 | 3.5 / 0.0 | `["2026-10-31 09:52:32", "2026-10-31 22:03:32", "2026-10-31 09:52:32", "2026-10-31 19:00:00", 486.3, 183.5]` | Sim, saída efetiva 19:00 < 22:00 | `sonda/anexos:77` |
| 108 / 2026-10-01 | 0.7 / 0.0 | `["2026-10-01 09:59:41", "2026-10-01 22:00:39", "2026-10-01 10:00:00", "2026-10-01 22:00:00", 659.3, 1.0]` | Sim, saída efetiva = 22:00 | `sonda/anexos:78` |
| 108 / 2026-10-03 | 0.5 / 0.0 | `["2026-10-03 09:56:13", "2026-10-03 22:00:27", "2026-10-03 10:00:00", "2026-10-03 22:00:00", 658.2, 4.2]` | Sim, saída efetiva = 22:00 | `sonda/anexos:79` |
| 108 / 2026-10-05 | 0.9 / 0.0 | `["2026-10-05 09:56:07", "2026-10-05 22:00:54", "2026-10-05 10:00:00", "2026-10-05 22:00:00", 660.5, 4.8]` | Sim, saída efetiva = 22:00 | `sonda/anexos:80` |

### `iguais_so_fechados|bruto>30|motor_agora=0`

| Colab / dia | Bruto / leg | Par | Cortou a faixa? | Fonte |
|---|---:|---|---|---|
| 37 / 2026-10-05 | 119.4 / 0.0 | `["2026-10-06 03:00:38", "2026-10-06 07:01:01", "2026-10-06 19:00:00", "2026-10-06 07:00:00", 0, 960.4]` | Sim, entrada efetiva 19:00 > 05:00 e posterior à saída efetiva de 07:00 | `sonda/anexos:81` |
| 37 / 2026-10-09 | 119.3 / 0.0 | `["2026-10-10 03:00:40", "2026-10-10 07:01:50", "2026-10-10 19:00:00", "2026-10-10 07:00:00", 0, 961.2]` | Sim, entrada efetiva 19:00 > 05:00 e posterior à saída efetiva de 07:00 | `sonda/anexos:82` |
| 113 / 2026-10-30 | 120.0 / 0.0 | `["2026-10-31 03:00:00", "2026-10-31 07:02:00", "2026-10-31 19:00:00", "2026-10-31 07:00:00", 0, 962.0]` | Sim, entrada efetiva 19:00 > 05:00 e posterior à saída efetiva de 07:00 | `sonda/anexos:83` |
| 113 / 2026-11-01 | 120.0 / 0.0 | `["2026-11-02 03:00:00", "2026-11-02 07:01:00", "2026-11-02 19:00:00", "2026-11-02 07:00:00", 0, 961.0]` | Sim, entrada efetiva 19:00 > 05:00 e posterior à saída efetiva de 07:00 | `sonda/anexos:84` |

Nos oito primeiros exemplos, o trecho efetivo termina às 22h ou antes. Nos quatro de `bruto>30`, o par efetivo está invertido, e `minutos_noturnos` retorna zero quando a entrada é igual ou posterior à saída. A regra ou configuração que produziu cada recorte na sombra fica **[nao verificado]**. `sonda/anexos:73–84` · [motor_calculo_v2.py:89](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:89)

## 3. `motor_agora>0` com `DiaPago.horas_noturnas=0`

**Só caracteriza uma lavra desatualizada frente ao recálculo se o valor positivo for o que o fechamento de hoje entregaria a `lavrar` para a mesma linha.** O fechamento passa os períodos e o mapa `dia_da_jornada` à lavra da versão `motor`. `lavrar` soma `minutos_noturnos_legais / 60` no dia atribuído, tanto nos períodos normais quanto nas partições de folga trabalhada, e grava a rubrica em `DiaPago.horas_noturnas`. Se a soma hoje entregue à mesma linha fosse positiva, o zero persistido divergiriria do que seria lavrado hoje. [fechamento.py:1200](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/services/fechamento.py:1200) · [dia_pago.py:126](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/services/dia_pago.py:126) · [dia_pago.py:173](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/services/dia_pago.py:173) · [dia_pago.py:223](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/services/dia_pago.py:223) · [dia_pago.py:345](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/services/dia_pago.py:345)

Nesta saída, o zero do C1 vem do `DiaPago` lavrado, e os 320 `iguais_so_fechados` também têm zero no recálculo da sonda. A equivalência entre o recálculo da sonda e os períodos que o fechamento entregaria à mesma linha, a data e a régua da lavra original, e a causa de uma eventual divergência ficam **[nao verificado]**. O código de `sonda_c1c.py` e o vínculo da cópia com o hash informado também ficam **[nao verificado]**. `causa_c1.r:3–5` · `sonda/anexos:1,15–17`

## 24/11 01:23 · sessao bc2e125c7f87 · NOTURNO-NAO-APURADO · sonda · origem=Code

## 1. C1 e contagem por classe

**C1 = 402.** As referências a `sonda/anexos` numeram a saída colada a partir de `C1 402` como linha 1. `sonda/anexos:1`

| Classe | Contagem | Fonte |
|---|---:|---|
| `aberto_ou_saida_orfa` | 1 | `sonda/anexos:2` |
| `aberto_ou_saida_orfa+quantidade_de_blocos` | 1 | `sonda/anexos:3` |
| `aberto_ou_saida_orfa+virada_ou_outro_dia` | 1 | `sonda/anexos:4` |
| `batida_orfa_na_celula+aberto_ou_saida_orfa+virada_ou_outro_dia` | 1 | `sonda/anexos:5` |
| `entrada_repetida+aberto_ou_saida_orfa+virada_ou_outro_dia` | 2 | `sonda/anexos:6` |
| `entrada_repetida+batida_orfa_na_celula+aberto_ou_saida_orfa` | 2 | `sonda/anexos:7` |
| `entrada_repetida+saida_apos_marco+aberto_ou_saida_orfa` | 1 | `sonda/anexos:8` |
| `entrada_repetida+saida_apos_marco+aberto_ou_saida_orfa+virada_ou_outro_dia+quantidade_de_blocos` | 1 | `sonda/anexos:9` |
| `entrada_repetida+saida_apos_marco+virada_ou_outro_dia` | 1 | `sonda/anexos:10` |
| `entrada_repetida+virada_ou_outro_dia` | 1 | `sonda/anexos:11` |
| `iguais_com_aberto` | 62 | `sonda/anexos:12` |
| `iguais_so_fechados` | 320 | `sonda/anexos:13` |
| `iguais_sem_par` | 0, por saldo | `sonda/anexos:1–16` |
| `saida_apos_marco+aberto_ou_saida_orfa` | 1 | `sonda/anexos:14` |
| `saida_apos_marco+virada_ou_outro_dia` | 4 | `sonda/anexos:15` |
| `virada_ou_outro_dia` | 3 | `sonda/anexos:16` |

As demais classes somam 20. A soma fecha em **20 + 62 + 320 + 0 = 402**. A linha de `iguais_sem_par` está ausente da listagem, e seu saldo é `402 − (20 + 62 + 320) = 0`. `sonda/anexos:1–16`

A saída fornecida termina em `rc_sombra=0` e não contém traceback para transcrever. `sonda/anexos:1–89`

## 2. Cinco exemplos de `iguais_so_fechados`

Em cada exemplo, usei o único par publicado, igual no motor e no juiz. A conta mede a interseção das pontas brutas com a faixa das 22h do dia indicado às 05h do dia seguinte, sem prorrogação e sem desconto de pausas. Os cinco pares intersectam a faixa. `sonda/anexos:76–80` · `app/ponto/motor_calculo_v2.py:99–118`

### `37` · `2026-10-05`

Batidas transcritas por data. `sonda/anexos:76`

```text
2026-10-03  19:00:08 E
2026-10-04  02:00:42 S  03:00:46 E  07:00:00 S
2026-10-06  02:00:15 S  03:00:38 E  07:01:01 S
2026-10-07  19:21:03 E
```

Motor e juiz `2026-10-06 03:00:38 → 2026-10-06 07:01:01`. **Sim.** Na madrugada de 06/10, `05:00:00 − 03:00:38 = 1h59min22s`. `sonda/anexos:76` · `app/ponto/motor_calculo_v2.py:99–118`

### `37` · `2026-10-09`

Batidas transcritas por data. `sonda/anexos:77`

```text
2026-10-07  19:21:03 E
2026-10-08  02:00:01 S  03:00:25 E  07:00:25 S
2026-10-10  02:00:04 S  03:00:40 E  07:01:50 S
2026-10-11  19:00:04 E
```

Motor e juiz `2026-10-10 03:00:40 → 2026-10-10 07:01:50`. **Sim.** Na madrugada de 10/10, `05:00:00 − 03:00:40 = 1h59min20s`. `sonda/anexos:77` · `app/ponto/motor_calculo_v2.py:99–118`

### `60` · `2026-10-31`

Batidas transcritas por data. `sonda/anexos:78`

```text
2026-10-29  06:56:19 E  11:02:34 S  12:02:20 E  19:00:25 S
2026-10-31  09:52:32 E  12:02:15 S  13:03:23 E  22:03:32 S
2026-11-02  10:05:20 E  15:20:23 S  16:20:32 E  22:01:25 S
```

Motor e juiz `2026-10-31 09:52:32 → 2026-10-31 22:03:32`. **Sim.** Em 31/10, `22:03:32 − 22:00:00 = 3min32s`. `sonda/anexos:78` · `app/ponto/motor_calculo_v2.py:99–118`

### `108` · `2026-10-01`

Batidas transcritas por data. `sonda/anexos:79`

```text
2026-10-01  09:59:41 E  15:00:22 S  16:01:05 E  22:00:39 S
2026-10-03  09:56:13 E  15:00:34 S  16:02:19 E  22:00:27 S
```

Motor e juiz `2026-10-01 09:59:41 → 2026-10-01 22:00:39`. **Sim.** Em 01/10, `22:00:39 − 22:00:00 = 39s`. `sonda/anexos:79` · `app/ponto/motor_calculo_v2.py:99–118`

### `108` · `2026-10-03`

Batidas transcritas por data. `sonda/anexos:80`

```text
2026-10-01  09:59:41 E  15:00:22 S  16:01:05 E  22:00:39 S
2026-10-03  09:56:13 E  15:00:34 S  16:02:19 E  22:00:27 S
2026-10-05  09:56:07 E  15:01:46 S  16:01:15 E  22:00:54 S
```

Motor e juiz `2026-10-03 09:56:13 → 2026-10-03 22:00:27`. **Sim.** Em 03/10, `22:00:27 − 22:00:00 = 27s`. `sonda/anexos:80` · `app/ponto/motor_calculo_v2.py:99–118`

## 3. Sítios candidatos por exemplo

A janela de HE teria de retirar toda a interseção ou produzir `saída efetiva <= entrada efetiva`. Se a janela deixar interseção positiva, `calcular_noturnas` só a zera por desconto de pausas quando elas cobrem todo o noturno efetivo, inclusive a prorrogação quando aplicável. `app/ponto/motor_calculo_v2.py:89–118,162–196,983–1005,1689–1763`

| Colab e dia | Condição que explicaria zero no cálculo do par |
|---|---|
| `37` · `2026-10-05` | **Janela de HE na entrada.** Deslocar a entrada efetiva de 06/10 para `05:00:00` ou depois retiraria a faixa de `1h59min22s`. Um recorte só da saída teria de chegar à entrada ou antes dela para zerar. Com as pontas brutas mantidas, pausas julgadas teriam de cobrir `03:00:38–05:00:00` e, se houver prorrogação, também `05:00:00–07:01:01`. `sonda/anexos:76` · `app/ponto/motor_calculo_v2.py:89–118,983–987,1689–1750` |
| `37` · `2026-10-09` | **Janela de HE na entrada.** Deslocar a entrada efetiva de 10/10 para `05:00:00` ou depois retiraria a faixa de `1h59min20s`. Um recorte só da saída teria de chegar à entrada ou antes dela para zerar. Com as pontas brutas mantidas, pausas julgadas teriam de cobrir `03:00:40–05:00:00` e, se houver prorrogação, também `05:00:00–07:01:50`. `sonda/anexos:77` · `app/ponto/motor_calculo_v2.py:89–118,983–987,1689–1750` |
| `60` · `2026-10-31` | **Janela de HE na saída.** Uma saída efetiva às `22:00:00` ou antes eliminaria os `3min32s`. Com as pontas brutas mantidas, pausas julgadas teriam de cobrir `22:00:00–22:03:32`. `sonda/anexos:78` · `app/ponto/motor_calculo_v2.py:99–118,983–987,1731–1750` |
| `108` · `2026-10-01` | **Janela de HE na saída.** Uma saída efetiva às `22:00:00` ou antes eliminaria os `39s`. Com as pontas brutas mantidas, pausas julgadas teriam de cobrir `22:00:00–22:00:39`. `sonda/anexos:79` · `app/ponto/motor_calculo_v2.py:99–118,983–987,1731–1750` |
| `108` · `2026-10-03` | **Janela de HE na saída.** Uma saída efetiva às `22:00:00` ou antes eliminaria os `27s`. Com as pontas brutas mantidas, pausas julgadas teriam de cobrir `22:00:00–22:00:27`. `sonda/anexos:80` · `app/ponto/motor_calculo_v2.py:99–118,983–987,1731–1750` |

Para cada um dos cinco, a aplicação da janela, os marcos, as pontas efetivas, as pausas julgadas e a prorrogação estão **[nao verificado]**. O cálculo preserva frações de minuto e a lavra soma os minutos legais divididos por 60 em um campo `FloatField`. Os `39s` e `27s`, por si sós, dão valor positivo nesse caminho. `app/ponto/motor_calculo_v2.py:112–118,983–992` · `app/ponto/services/dia_pago.py:173–179` · `app/ponto/models.py:327`

Dois sítios posteriores também são candidatos para cada exemplo. **Bloqueio do dia sem previsto** exigiria trabalho apurado, folga ou feriado, janela vigente, previsto igual a zero, ausência de autorização e, em folga com colaborador identificado, escala certa. O bloqueio zera os dois campos de noturno. As guardas e o antes/depois de cada exemplo estão **[nao verificado]**. `app/ponto/motor_calculo_v2.py:1440–1483,2457–2461`

**Fatia ou atribuição na lavra** exigiria verificar se o par entrou no resultado usado pelo fechamento e em que data foi somado. Nos dois exemplos do `37`, a data civil da entrada é o dia seguinte ao `dia` publicado. Na lavra normal, o mapa da jornada pode atribuir o par ao dia publicado, com a data civil da entrada como fallback. No O85, o fechamento agrupa pela data civil da entrada e encaminha só minutos trabalhados. O mapa efetivo, a aplicação do O85 e a correspondência entre o motor vivo da sonda e o fechamento que gerou o zero estão **[nao verificado]**. `sonda/anexos:76–80` · `app/ponto/services/espelho.py:1198–1227` · `app/ponto/motor_calculo_v2.py:2399–2432` · `app/ponto/services/fechamento.py:792–796,838–843,1201–1217` · `app/ponto/services/dia_pago.py:126–132,269–272`

_8036 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
