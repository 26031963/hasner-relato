# CODEX-ESTADO

> Ultima resposta processada: **18/11/2026 10:46** · publicado em **18/11/2026 10:49** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 18/11 10:46 · sessao 671f115eebf8 · SONDA-DIA-IMPAR-1811 · sonda · origem=Code

```text
SONDA achadoA_casos agora=18/11 10:38:15 ultima_batida_sombra=18/11 03:55:21
============================================================
CASO C004 col281 08/11  (1.4s)
 ESPELHO n_marcacoes=2: 18:55 E orfa=True origem=app proc=None just= ; 06:58 S orfa=False origem=app proc=None just=
 APURAVEIS d-1 12h..d+1 12h n=4: 07/11 18:59:04 E pk=148830 orig=app itv=False apr=True tr= canal=apk perg=None ; 08/11 07:00:09 S pk=149145 orig=app itv=False apr=True tr= canal=apk perg=None ; 08/11 18:55:42 E pk=149472 orig=app itv=False apr=True tr= canal=apk perg=None ; 09/11 06:58:35 S pk=149781 orig=app itv=False apr=True tr= canal=apk perg=None
 RETRATADAS na janela n=0
 TURNO data_turno=2026-11-08 entrada=08/11 18:55:42 saida=09/11 06:58:35 aberto=False fim_fechado=09/11 06:58:35 batidas=18:55E 06:58S
   intervalos_do_turno=[]
 turnos de data_turno=2026-11-08 no pareamento da janela inteira: 1 (total na janela 14)
 REALIZADO(todos turnos)=(723, False, False, False, 0, False)
 REALIZADO(so turnos com saida)=(723, False, False, False, 0, False)
 borda_posicional=False
 CELULA: {"id": "137834", "colaborador_id": "281", "data": "2026-11-08", "origem": "gerada", "escala_geradora_id": "246", "dna": "{\"marcos\": null, \"lampadas\": null, \"dna_versao\": 2, \"tipo_ciclo\": \"12x36\", \"vinculo_id\": 246, \"tipo_escala_id\": 179, \"intervalo_livre\": true, \"minutos_jornada\": 720}", "gerada_em": "2026-10-21 08:50:22.986233+00:00", "julgada_em": "2026-11-09 21:57:11.865832+00:00", "impressao": "efd0e9f5a5cc829b1ea09876e6c6d9a33293dd2bc1b08c0ce8092a3058a96355", "veredito": "fato_sem_previsao", "veredito_via": "cartorio", "veredito_em": "2026-11-09 21:57:11.865832+00:00", "protestos": "[\"FATO_SEM_PREVISAO\"]", "ata": "{\"orfas\": [\"18:55\"], \"n_orfas\": 1, \"lampadas\": [], \"tipo_dia\": \"folga\", \"n_celulas\": 0, \"n_missing\": 0, \"sem_ancora\": false, \"precedencia\": {\"faixa\": \"folga\", \"texto\": \"\", \"rotulo\": \"Folga\", \"suprimidos\": [], \"tipo_do_dia\": \"folga\", \"era_dia_previsto\": false}, \"minutos_previstos\": 0, \"minutos_realizados\": 723}", "insumos_em": "2026-11-09 21:57:11.865832+00:00", "status_dia": "folga_trabalhada", "status_rotulo": "Trabalhou em dia sem jornada prevista"}
 EC.marcos_do_dia=(datetime.time(19, 0), datetime.time(7, 0), None, None) tipo_escala=12x36 · 19:00-07:00
 JUSTIFICATIVAS data_turno=2026-11-08 n=0 []
 DIAPAGO versao=motor pago_do_dia_h=None min=None | {"id": "2368660", "colaborador_id": "281", "mes": "11", "ano": "2026", "data": "2026-11-08", "tipo": "dia", "versao": "motor", "versao_motor": "legais|p5h=0|hr=1|fd=0", "evento_origem": "recalcular_fechamento_mes", "lavrado_em": "2026-11-18 10:01:18.646726+00:00"}
 DIAPAGO versao=oraculo pago_do_dia_h=None min=None | {"id": "2368677", "colaborador_id": "281", "mes": "11", "ano": "2026", "data": "2026-11-08", "tipo": "dia", "versao": "oraculo", "evento_origem": "recalcular_fechamento_mes", "detalhe": "{\"calculador_nao_decide\": [\"horas_atraso\", \"horas_extras_100\", \"horas_extras_100_feriado\", \"horas_extras_100_noturna\", \"horas_extras_50\", \"horas_extras_50_noturna\", \"horas_falta\", \"horas_noturnas\", \"horas_reflexo_dsr\", \"horas_saida_antecipada\", \"saldo_banco_horas\"]}", "lavrado_em": "2026-11-18 10:01:18.771705+00:00"}
 TELA(dia dict): pago_h=None veredito_dia='folga' palavra_dia='Folga' cor='#94a3b8' realizado_aberto=False realizado_h=12.1 minutos_realizados=723 previsto=[] ausencia=None falta_frase='' falta_marcos=[] rubricas={'lavrada': True, 'extras_50': 0.0, 'extras_100': 0.0, 'atraso': 0.0, 'antecipada': 0.0, 'noturnas': 0.0, 'intra': 0.0, 'trabalhadas': 0.0, 'minutos_extra': 0.0}
 TELA(dia dict) turnos_motor=[('08/11 18:55:42', '09/11 06:58:35', False, ['Intrajornada suprimida — 60min pagos como hora extra (Art.71 §4 CLT / Sumula 437 TST). Intra real: 0min.', 'DIA SEM JORNADA PREVISTA -- 723 min fora da janela de HE (L-097, bloqueio total): pendencia na Gestao de HE ate Autorizar.', 'FOLGA TRABALHADA — 08/11/2026 18:55 (dia de folga no ciclo 12x36 (ancora 26/07/2026)). Registro mantido conforme Portaria MTE 671/2021. Horas apuradas: 0.0h. Requer validacao do responsavel antes do fechamento.'])]
 PDF_LINHA: 08/11 dom | 19:00 07:00 | MARCACOES:S06:58 + *18:55 | Folga | HE fora da janela: 723 min em dia sem jornada prevista — nao conta sem decisao |  |  | —
============================================================
CASO C009 col902 28/10  (1.3s)
 ESPELHO n_marcacoes=3: 07:58 E orfa=False origem=app proc=None just= ; 12:06 S orfa=False origem=app proc=None just= ; 18:07 S orfa=False origem=app proc=None just=
 APURAVEIS d-1 12h..d+1 12h n=8: 27/10 12:10:13 S pk=138670 orig=app itv=False apr=True tr= canal=apk perg=None ; 27/10 13:10:05 E pk=138758 orig=app itv=False apr=True tr= canal=apk perg=None ; 27/10 18:00:44 S pk=139051 orig=app itv=False apr=True tr= canal=apk perg=None ; 28/10 07:58:46 E pk=139547 orig=app itv=False apr=True tr= canal=apk perg=None ; 28/10 12:06:03 S pk=139697 orig=app itv=False apr=True tr= canal=apk perg=None ; 28/10 18:07:37 S pk=140070 orig=app itv=False apr=True tr= canal=apk perg=None ; 29/10 07:58:05 E pk=140537 orig=app itv=False apr=True tr= canal=apk perg=None ; 29/10 11:58:37 S pk=140667 orig=app itv=False apr=True tr= canal=apk perg=None
 RETRATADAS na janela n=0
 TURNO data_turno=2026-10-28 entrada=28/10 07:58:46 saida=28/10 18:07:37 aberto=False fim_fechado=28/10 18:07:37 batidas=07:58E 12:06S 18:07S
   intervalos_do_turno=[]
 turnos de data_turno=2026-10-28 no pareamento da janela inteira: 1 (total na janela 19)
 REALIZADO(todos turnos)=(549, False, False, False, 60, False)
 REALIZADO(so turnos com saida)=(549, False, False, False, 60, False)
 borda_posicional=False
 CELULA: {"id": "132139", "colaborador_id": "902", "data": "2026-10-28", "trabalha": "True", "origem": "gerada", "escala_geradora_id": "1101", "dna": "{\"marcos\": {\"hf\": \"18:00\", \"hi\": \"08:00\", \"hfi\": \"13:00\", \"hii\": \"12:00\"}, \"lampadas\": [{\"hora\": \"08:00\", \"tipo\": \"E\", \"chave\": \"hi\", \"instante\": \"2026-10-28T08:00\"}, {\"hora\": \"12:00\", \"tipo\": \"S\", \"chave\": \"hii\", \"instante\": \"2026-10-28T12:00\"}, {\"hora\": \"13:00\", \"tipo\": \"E\", \"chave\": \"hfi\", \"instante\": \"2026-10-28T13:00\"}, {\"hora\": \"18:00\", \"tipo\": \"S\", \"chave\": \"hf\", \"instante\": \"2026-10-28T18:", "gerada_em": "2026-10-21 08:50:15.435733+00:00", "julgada_em": "2026-11-02 07:25:34.015313+00:00", "impressao": "96f692673e51b232b853c86945e57e6ef725ca1cab3957f800b4d2315f088d12", "veredito": "furo", "veredito_via": "cartorio", "veredito_em": "2026-10-28 21:07:38.775318+00:00", "protestos": "[\"FURO_PARCIAL\"]", "ata": "{\"orfas\": [], \"n_orfas\": 0, \"lampadas\": [{\"luz\": \"07:58\", \"hora\": \"08:00\", \"tipo\": \"E\", \"acesa\": true, \"tipo_real\": \"E\"}, {\"luz\": \"12:06\", \"hora\": \"12:00\", \"tipo\": \"S\", \"acesa\": true, \"tipo_real\": \"S\"}, {\"luz\": null, \"hora\": \"13:00\", \"tipo\": \"E\", \"acesa\": false, \"tipo_real\": null}, {\"luz\": \"18:07\", \"hora\": \"18:00\", \"tipo\": \"S\", \"acesa\": true, \"tipo_real\": \"S\"}], \"tipo_dia\": \"trabalho\", \"n_celulas\"", "tem_entrada": "True", "tem_saida": "True", "insumos_em": "2026-11-02 07:25:34.015313+00:00", "status_dia": "irregular", "status_rotulo": "Batida faltando, fora do horario previsto ou em ordem inesperada"}
 EC.marcos_do_dia=(datetime.time(8, 0), datetime.time(18, 0), datetime.time(12, 0), datetime.time(13, 0)) tipo_escala=5x2 · Seg/Ter/Qua/Qui/Sex 08:00-18:00
 JUSTIFICATIVAS data_turno=2026-10-28 n=0 []
 DIAPAGO versao=motor pago_do_dia_h=10.0 min=600 | {"id": "2378566", "colaborador_id": "902", "mes": "11", "ano": "2026", "data": "2026-10-28", "tipo": "dia", "versao": "motor", "horas_trabalhadas": "10.0", "horas_intra_indenizada": "1.0", "minutos_previstos": "540.0", "minutos_realizados": "540.0", "versao_motor": "legais|p5h=0|hr=0|fd=1", "evento_origem": "recalcular_fechamento_mes", "lavrado_em": "2026-11-18 10:02:49.144170+00:00"}
 DIAPAGO versao=oraculo pago_do_dia_h=10.0 min=600 | {"id": "2378590", "colaborador_id": "902", "mes": "11", "ano": "2026", "data": "2026-10-28", "tipo": "dia", "versao": "oraculo", "horas_trabalhadas": "10.0", "horas_intra_indenizada": "1.0", "minutos_previstos": "540.0", "minutos_realizados": "540.0", "evento_origem": "recalcular_fechamento_mes", "detalhe": "{\"calculador_nao_decide\": [\"horas_falta\", \"horas_reflexo_dsr\", \"saldo_banco_horas\"]}", "lavrado_em": "2026-11-18 10:02:49.362282+00:00"}
 TELA(dia dict): pago_h=10.0 veredito_dia='trabalhou' palavra_dia='' cor='' realizado_aberto=False realizado_h=9.2 minutos_realizados=549 previsto=[] ausencia=None falta_frase='falta entrada 13:00' falta_marcos=[{'tipo': 'E', 'hora': '13:00'}] rubricas={'lavrada': True, 'extras_50': 0.0, 'extras_100': 0.0, 'atraso': 0.0, 'antecipada': 0.0, 'noturnas': 0.0, 'intra': 1.0, 'trabalhadas': 10.0, 'minutos_extra': 0.0}
 TELA(dia dict) turnos_motor=[('28/10 07:58:46', '28/10 18:07:37', False, ['Intrajornada suprimida — 60min pagos como hora extra (Art.71 §4 CLT / Sumula 437 TST). Intra real: 0min.'])]
 PDF_LINHA: 28/10 qua | 08:00 18:00 | MARCACOES:E07:58 S12:06 S18:07 | HE fora da janela: 2 min antes do marco 08:00 — nao conta (Art. 4º §2º) | HE fora da janela: 7 min depois do marco 18:00 | 1h | 10h
============================================================
CASO C015 col89 08/11  (1.7s)
 ESPELHO n_marcacoes=4: 06:55 E orfa=True origem=app proc=None just= ; 12:32 S orfa=False origem=app proc=None just= ; 13:02 E orfa=False origem=app proc=None just= ; 14:00 S orfa=False origem=app proc=None just=
 APURAVEIS d-1 12h..d+1 12h n=5: 08/11 06:55:32 E pk=149114 orig=app itv=False apr=True tr= canal=apk perg=None ; 08/11 12:32:52 S pk=149266 orig=app itv=False apr=True tr= canal=apk perg=None ; 08/11 13:02:55 E pk=149285 orig=app itv=False apr=True tr= canal=apk perg=None ; 08/11 14:00:04 S pk=149307 orig=app itv=False apr=True tr= canal=apk perg=None ; 09/11 09:55:13 E pk=149917 orig=app itv=False apr=True tr= canal=apk perg=None
 RETRATADAS na janela n=0
 TURNO data_turno=2026-11-08 entrada=08/11 06:55:32 saida=08/11 14:00:04 aberto=False fim_fechado=08/11 14:00:04 batidas=06:55E 12:32S 13:02E 14:00S
   intervalos_do_turno=[('08/11 12:32:52', '08/11 13:02:55')]
 turnos de data_turno=2026-11-08 no pareamento da janela inteira: 1 (total na janela 24)
 REALIZADO(todos turnos)=(395, False, False, False, 0, False)
 REALIZADO(so turnos com saida)=(395, False, False, False, 0, False)
 borda_posicional=False
 CELULA: {"id": "123501", "colaborador_id": "89", "data": "2026-11-08", "trabalha": "True", "origem": "gerada", "escala_geradora_id": "1039", "dna": "{\"marcos\": {\"hf\": \"18:20\", \"hi\": \"10:00\", \"hfi\": \"13:00\", \"hii\": \"12:00\"}, \"lampadas\": [{\"hora\": \"10:00\", \"tipo\": \"E\", \"chave\": \"hi\", \"instante\": \"2026-11-08T10:00\"}, {\"hora\": \"12:00\", \"tipo\": \"S\", \"chave\": \"hii\", \"instante\": \"2026-11-08T12:00\"}, {\"hora\": \"13:00\", \"tipo\": \"E\", \"chave\": \"hfi\", \"instante\": \"2026-11-08T13:00\"}, {\"hora\": \"18:20\", \"tipo\": \"S\", \"chave\": \"hf\", \"instante\": \"2026-11-08T18:", "gerada_em": "2026-10-06 10:20:42.792209+00:00", "julgada_em": "2026-11-09 21:15:15.737185+00:00", "impressao": "8cd0ee03c41e7ad6fdf2fe26a5053e3235298638c77c8314f577ce6eb04eab5f", "veredito": "concorde", "veredito_via": "cartorio", "veredito_em": "2026-11-08 21:50:06.197830+00:00", "ata": "{\"orfas\": [\"06:55\"], \"n_orfas\": 1, \"lampadas\": [{\"luz\": \"12:32\", \"hora\": \"10:00\", \"tipo\": \"E\", \"acesa\": true, \"tipo_real\": \"S\"}, {\"luz\": \"13:02\", \"hora\": \"12:00\", \"tipo\": \"S\", \"acesa\": true, \"tipo_real\": \"E\"}, {\"luz\": \"14:00\", \"hora\": \"13:00\", \"tipo\": \"E\", \"acesa\": true, \"tipo_real\": \"S\"}, {\"luz\": null, \"hora\": \"18:20\", \"tipo\": \"S\", \"acesa\": false, \"tipo_real\": null}], \"tipo_dia\": \"trabalho\", \"n_c", "tem_entrada": "True", "insumos_em": "2026-11-09 21:15:15.737185+00:00", "status_dia": "ok", "status_rotulo": "Marco cumprido com batida — todas as batidas previstas registradas"}
 EC.marcos_do_dia=(datetime.time(10, 0), datetime.time(18, 20), datetime.time(12, 0), datetime.time(13, 0)) tipo_escala=ESCALA ARTES · 6x1 · 10:00-18:20
 JUSTIFICATIVAS data_turno=2026-11-08 n=0 []
 DIAPAGO versao=motor pago_do_dia_h=6.574583888888889 min=394 | {"id": "2383042", "colaborador_id": "89", "mes": "11", "ano": "2026", "data": "2026-11-08", "tipo": "dia", "versao": "motor", "horas_trabalhadas": "6.574583888888889", "horas_intra_indenizada": "0.49908749999999996", "minutos_previstos": "440.0", "minutos_realizados": "395.0", "versao_motor": "relogio|p5h=1|hr=0|fd=1", "evento_origem": "recalcular_fechamento_mes", "lavrado_em": "2026-11-18 10:03:36.467889+00:00"}
 DIAPAGO versao=oraculo pago_do_dia_h=6.5745838888888875 min=394 | {"id": "2383073", "colaborador_id": "89", "mes": "11", "ano": "2026", "data": "2026-11-08", "tipo": "dia", "versao": "oraculo", "horas_trabalhadas": "6.5745838888888875", "horas_intra_indenizada": "0.49908749999999996", "minutos_previstos": "440.0", "minutos_realizados": "395.0", "evento_origem": "recalcular_fechamento_mes", "detalhe": "{\"calculador_nao_decide\": [\"horas_falta\", \"horas_reflexo_dsr\", \"saldo_banco_horas\"]}", "lavrado_em": "2026-11-18 10:03:36.699900+00:00"}
 TELA(dia dict): pago_h=6.57 veredito_dia='trabalhou' palavra_dia='' cor='' realizado_aberto=False realizado_h=6.6 minutos_realizados=395 previsto=[] ausencia=None falta_frase='falta saida 18:20' falta_marcos=[{'tipo': 'S', 'hora': '18:20'}] rubricas={'lavrada': True, 'extras_50': 0.0, 'extras_100': 0.0, 'atraso': 0.0, 'antecipada': 0.0, 'noturnas': 0.0, 'intra': 0.49908749999999996, 'trabalhadas': 6.574583888888889, 'minutos_extra': 0.0}
 TELA(dia dict) turnos_motor=[('08/11 06:55:32', '08/11 14:00:04', False, ['Intrajornada suprimida — 29min pagos como hora extra (Art.71 §4 CLT / Sumula 437 TST). Intra real: 30min.'])]
 PDF_LINHA: 08/11 dom | 10:00 18:20 | MARCACOES:S12:32 E13:02 S14:00 + *06:55 |  |  | 0h30 | 6h34
============================================================
CASO C015 col89 15/11  (0.0s)
 ESPELHO n_marcacoes=4: 06:55 E orfa=True origem=app proc=None just= ; 12:00 S orfa=False origem=app proc=None just= ; 12:32 E orfa=False origem=app proc=None just= ; 14:03 S orfa=False origem=app proc=None just=
 APURAVEIS d-1 12h..d+1 12h n=8: 14/11 12:01:43 S pk=154912 orig=app itv=False apr=True tr= canal=apk perg=None ; 14/11 13:00:09 E pk=154943 orig=app itv=False apr=True tr= canal=apk perg=None ; 14/11 18:14:16 S pk=155150 orig=app itv=False apr=True tr= canal=apk perg=None ; 15/11 06:55:08 E pk=155519 orig=app itv=False apr=True tr= canal=apk perg=None ; 15/11 12:00:14 S pk=155653 orig=app itv=False apr=True tr= canal=apk perg=None ; 15/11 12:32:30 E pk=155680 orig=app itv=False apr=True tr= canal=apk perg=None ; 15/11 14:03:44 S pk=155731 orig=app itv=False apr=True tr= canal=apk perg=None ; 16/11 09:55:05 E pk=156388 orig=app itv=False apr=True tr= canal=apk perg=None
 RETRATADAS na janela n=0
 TURNO data_turno=2026-11-15 entrada=15/11 06:55:08 saida=15/11 14:03:44 aberto=False fim_fechado=15/11 14:03:44 batidas=06:55E 12:00S 12:32E 14:03S
   intervalos_do_turno=[('15/11 12:00:14', '15/11 12:32:30')]
 turnos de data_turno=2026-11-15 no pareamento da janela inteira: 1 (total na janela 24)
 REALIZADO(todos turnos)=(396, False, False, False, 0, False)
 REALIZADO(so turnos com saida)=(396, False, False, False, 0, False)
 borda_posicional=False
 CELULA: {"id": "123508", "colaborador_id": "89", "data": "2026-11-15", "trabalha": "True", "origem": "gerada", "escala_geradora_id": "1039", "dna": "{\"marcos\": {\"hf\": \"18:20\", \"hi\": \"10:00\", \"hfi\": \"13:00\", \"hii\": \"12:00\"}, \"lampadas\": [{\"hora\": \"10:00\", \"tipo\": \"E\", \"chave\": \"hi\", \"instante\": \"2026-11-15T10:00\"}, {\"hora\": \"12:00\", \"tipo\": \"S\", \"chave\": \"hii\", \"instante\": \"2026-11-15T12:00\"}, {\"hora\": \"13:00\", \"tipo\": \"E\", \"chave\": \"hfi\", \"instante\": \"2026-11-15T13:00\"}, {\"hora\": \"18:20\", \"tipo\": \"S\", \"chave\": \"hf\", \"instante\": \"2026-11-15T18:", "gerada_em": "2026-10-06 10:20:42.801509+00:00", "julgada_em": "2026-11-16 21:15:34.708287+00:00", "impressao": "36a7df7a96d4fce7b201973b1c5796436395ec251b17330b70c43b4453ed6889", "veredito": "concorde", "veredito_via": "cartorio", "veredito_em": "2026-11-15 21:50:05.107581+00:00", "ata": "{\"orfas\": [\"06:55\"], \"n_orfas\": 1, \"lampadas\": [{\"luz\": \"12:00\", \"hora\": \"10:00\", \"tipo\": \"E\", \"acesa\": true, \"tipo_real\": \"S\"}, {\"luz\": \"12:32\", \"hora\": \"12:00\", \"tipo\": \"S\", \"acesa\": true, \"tipo_real\": \"E\"}, {\"luz\": \"14:03\", \"hora\": \"13:00\", \"tipo\": \"E\", \"acesa\": true, \"tipo_real\": \"S\"}, {\"luz\": null, \"hora\": \"18:20\", \"tipo\": \"S\", \"acesa\": false, \"tipo_real\": null}], \"tipo_dia\": \"trabalho\", \"n_c", "tem_entrada": "True", "insumos_em": "2026-11-16 21:15:34.708287+00:00", "status_dia": "ok", "status_rotulo": "Marco cumprido com batida — todas as batidas previstas registradas"}
 EC.marcos_do_dia=(datetime.time(10, 0), datetime.time(18, 20), datetime.time(12, 0), datetime.time(13, 0)) tipo_escala=ESCALA ARTES · 6x1 · 10:00-18:20
 JUSTIFICATIVAS data_turno=2026-11-15 n=0 []
 DIAPAGO versao=motor pago_do_dia_h=6.605381666666667 min=396 | {"id": "2383049", "colaborador_id": "89", "mes": "11", "ano": "2026", "data": "2026-11-15", "tipo": "dia", "versao": "motor", "horas_trabalhadas": "6.605381666666667", "horas_extras_100": "6.605381666666667", "horas_extras_100_feriado": "6.605381666666667", "horas_intra_indenizada": "0.4620066666666666", "minutos_previstos": "440.0", "minutos_realizados": "396.0", "versao_motor": "relogio|p5h=1|hr=0|fd=1", "evento_origem": "recalcular_fechamento_mes", "lavrado_em": "2026-11-18 10:03:36.468043+00:00"}
 DIAPAGO versao=oraculo pago_do_dia_h=6.605381666666667 min=396 | {"id": "2383080", "colaborador_id": "89", "mes": "11", "ano": "2026", "data": "2026-11-15", "tipo": "dia", "versao": "oraculo", "horas_trabalhadas": "6.605381666666667", "horas_extras_100": "6.605381666666667", "horas_extras_100_feriado": "6.605381666666667", "horas_intra_indenizada": "0.4620066666666666", "minutos_previstos": "440.0", "minutos_realizados": "396.0", "evento_origem": "recalcular_fechamento_mes", "detalhe": "{\"calculador_nao_decide\": [\"horas_falta\", \"horas_reflexo_dsr\", \"saldo_banco_horas\"]}", "lavrado_em": "2026-11-18 10:03:36.700054+00:00"}
 TELA(dia dict): pago_h=6.61 veredito_dia='trabalhou' palavra_dia='' cor='' realizado_aberto=False realizado_h=6.6 minutos_realizados=396 previsto=[] ausencia=None falta_frase='falta saida 18:20' falta_marcos=[{'tipo': 'S', 'hora': '18:20'}] rubricas={'lavrada': True, 'extras_50': 0.0, 'extras_100': 6.605381666666667, 'atraso': 0.0, 'antecipada': 0.0, 'noturnas': 0.0, 'intra': 0.4620066666666666, 'trabalhadas': 6.605381666666667, 'minutos_extra': 396.3229}
 TELA(dia dict) turnos_motor=[('15/11 06:55:08', '15/11 14:03:44', False, ['Intrajornada suprimida — 27min pagos como hora extra (Art.71 §4 CLT / Sumula 437 TST). Intra real: 32min.', 'Feriado em 15/11/2026 — todas as 6.6h pagas em dobro (Sumula 146 TST).', 'Feriado em 15/11/2026 — todas as 6.6h pagas em dobro (Sumula 146 TST).'])]
 PDF_LINHA: 15/11 dom ★ | 10:00 18:20 | MARCACOES:S12:00 E12:32 S14:03 + *06:55 |  | 6h36 | 0h28 | 6h37
============================================================
CASO C017 col32 22/10  (1.2s)
 ESPELHO n_marcacoes=3: 07:00 E orfa=False origem=app proc=None just= ; 12:26 S orfa=False origem=app proc=None just= ; 19:00 S orfa=False origem=app proc=None just=
 APURAVEIS d-1 12h..d+1 12h n=3: 22/10 07:00:23 E pk=133877 orig=app itv=False apr=True tr= canal=None perg=None ; 22/10 12:26:07 S pk=134164 orig=app itv=False apr=True tr= canal=None perg=None ; 22/10 19:00:23 S pk=134711 orig=app itv=False apr=True tr= canal=None perg=None
 RETRATAD

[truncado em 20000 de 68679 caracteres]

## 18/11 10:39 · sessao 5eee9c2e3d72 · E4C-F4 · verificador · origem=Code

**1. A nova asserção é verdadeira por análise do código; execução do teste [não verificado].** A pauta prevista no segundo comando contém: [nao verificado]

- `(self.saida.pk, 'sem dia do fato')`
- `(self.volta.pk, '10/2026')`

Os valores numéricos dos PKs são atribuídos pelo banco de teste e estão **[não verificado]**. A fixture cria saída e volta para o mesmo colaborador e com data inicial de 25/09/2026 ([test_bug_par_projetado.py:28](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/test_bug_par_projetado.py:28), [test_bug_par_projetado.py:75](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/test_bug_par_projetado.py:75), [test_bug_par_projetado.py:87](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/test_bug_par_projetado.py:87)).

O caminho é:

1. `_par_163` coloca ambas em U60 e U163; U103 permanece vazio desde a inicialização ([test_bug_par_volta.py:25](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/test_bug_par_volta.py:25), [test_bug_par_volta.py:501](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/test_bug_par_volta.py:501)).
2. O teste entrega 10/2026, confirma ambas nesse balde e depois altera somente a data da saída para `None` ([test_bug_par_volta.py:496](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/test_bug_par_volta.py:496), [test_bug_par_volta.py:605](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/test_bug_par_volta.py:605), [test_bug_par_volta.py:616](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/test_bug_par_volta.py:616)).
3. A testemunha consulta a própria pergunta; nesse caminho, não procura a irmã. O contador põe a saída sem dia em `bug_par_103_sem_dia` e a volta entregue em `bug_par_103_pauta_dp` ([validacao.py:209](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/services/validacao.py:209), [materializacao.py:341](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/services/materializacao.py:341)).
4. Os laços anteriores do comando não incluem essas perguntas: U103 está vazio e o outro laço exclui U163. O comando acrescenta os dois baldes, associa `col<id>` a cada PK e imprime a pauta; a asserção compara apenas PK e rótulo, ordenados ([bug_par_103.py:54](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/management/commands/bug_par_103.py:54), [bug_par_103.py:73](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/management/commands/bug_par_103.py:73), [bug_par_103.py:81](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/management/commands/bug_par_103.py:81), [bug_par_103.py:143](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/management/commands/bug_par_103.py:143), [test_bug_par_volta.py:622](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/test_bug_par_volta.py:622)).

**2. Os três alvos são únicos na respectiva fonte:** contei uma ocorrência textual de cada alvo, nas linhas [bug_par_103.py:148](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/management/commands/bug_par_103.py:148), [bug_par_103.py:86](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/management/commands/bug_par_103.py:86) e [materializacao.py:343](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/services/materializacao.py:343). Os três apontam para o mesmo teste ([bug_par_mutantes.py:113](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/bug_par_mutantes.py:113)).

Por análise estática:

- `u163_l5_fora_da_pauta` deixa a segunda pauta só com `(self.volta.pk, '10/2026')`; a comparação da linha 626 deve falhar por asserção ([bug_par_mutantes.py:120](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/bug_par_mutantes.py:120), [test_bug_par_volta.py:626](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/test_bug_par_volta.py:626)).
- `u163_fora_da_pauta` elimina a inclusão dos baldes e deve falhar na primeira comparação da pauta, antes da alteração da data ([bug_par_mutantes.py:116](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/bug_par_mutantes.py:116), [test_bug_par_volta.py:611](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/test_bug_par_volta.py:611)).
- `u163_l1_no_pendente` impede a classificação L1/L5 nesse ramo; após a entrega, as duas passam para `outras`, compondo pendentes, e a asserção de zero deve falhar ([bug_par_mutantes.py:113](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/bug_par_mutantes.py:113), [materializacao.py:352](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/services/materializacao.py:352), [test_bug_par_volta.py:605](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/test_bug_par_volta.py:605)).

A execução efetiva e a confirmação de `falhas > 0`, `erros = 0` para cada mutante ficam **[não verificado]**: são mutantes de integração, cujo executor usa o runner Django de testes ([bug_par_mutantes.py:163](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/bug_par_mutantes.py:163), [bug_par_mutantes.py:170](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/bug_par_mutantes.py:170), [bug_par_mutantes.py:237](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/bug_par_mutantes.py:237)).

## 18/11 10:37 · sessao 9a9ccfe2565c · SONDA-DIA-IMPAR-1811 · sonda · origem=Code

Há traceback em `/app/relatorios/pdf_espelho.py:722`; faltam `FIM_SONDA_CASOS` e `rc_sombra=0` [nao verificado].

```text
SONDA achadoA_casos agora=18/11 10:37:10 ultima_batida_sombra=18/11 03:55:21
Traceback (most recent call last):
  File "/app/manage.py", line 22, in <module>
    main()
    ~~~~^^
  File "/app/manage.py", line 18, in main
    execute_from_command_line(sys.argv)
    ~~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^
  File "/usr/local/lib/python3.13/site-packages/django/core/management/__init__.py", line 442, in execute_from_command_line
    utility.execute()
    ~~~~~~~~~~~~~~~^^
  File "/usr/local/lib/python3.13/site-packages/django/core/management/__init__.py", line 436, in execute
    self.fetch_command(subcommand).run_from_argv(self.argv)
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^
  File "/usr/local/lib/python3.13/site-packages/django_tenants/management/commands/tenant_command.py", line 48, in run_from_argv
    klass.run_from_argv(args)
    ~~~~~~~~~~~~~~~~~~~^^^^^^
  File "/usr/local/lib/python3.13/site-packages/django/core/management/base.py", line 420, in run_from_argv
    self.execute(*args, **cmd_options)
    ~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.13/site-packages/django/core/management/base.py", line 464, in execute
    output = self.handle(*args, **options)
  File "/usr/local/lib/python3.13/site-packages/django/core/management/commands/shell.py", line 247, in handle
    exec(options["command"], {**globals(), **self.get_namespace(**options)})
    ~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "<string>", line 1, in <module>
  File "<string>", line 72, in <module>
  File "/app/relatorios/pdf_espelho.py", line 722, in _build_story
    _dir = Paragraph('Emitido %s%s' % (dados['gerado_em_str'], ('<br/><font color="#9a3412">' + _anom_txt + '</font>') if _anom_txt else ''), style_meta)
                                       ~~~~~^^^^^^^^^^^^^^^^^
KeyError: 'gerado_em_str'
rc_sombra=1
```

## 18/11 10:34 · sessao f9447a60588b · RESPOSTAS-DE-LEI-1811-1010 · sonda · origem=Code

**Passo 1 — cópia do dado raspado**

```text
=== SOMBRA (sombra_db, leitor) 18/11 10:33:03 ===
sonda_leitura: sonda_i4.py [dbc76cca2877ec2c] no cpuset 4-7, lendo sombra como leitor (LEITURA)

(1, 1, 5, 1367)
(1, 2, 5, 1758)
(1, 3, 5, 1264)
(1, 4, 5, 17)
(1, 5, 5, 500)
(1, 6, 5, 737)
(1, 8, 5, 65)
(2, 1, 0, 0)
(2, 2, 0, 0)
(2, 3, 0, 0)
(2, 4, 0, 0)
(2, 5, 5708, 244)
(3, 5, '1_ate_1min', 1)
(3, 5, '3_ate_1dia', 4)
(3, 5, '4_mais_1dia', 239)
(5, 'classe2_sem_log_auditoria', 0)
(6, 'lavras_total', 47318)
(7, 'candidata_A_foto_historica_move', 0, 'marcos_do_lote nao le DP (veredito_lavrado.py:151-208), lavras que mudam = 0 por construcao')
(8, 'candidata_B_provada_classe2', 0)
(9, 'cota_classe1', 0)
(9, 'cota_classe2', 0)
(9, 'cota_classe4', 0)
(9, 'cota_qualquer_1_2_4', 0)
(10, 'total_dp', 5708)
rc_sombra=0
```

Linhas copiadas: **28**, incluindo a linha em branco. [não verificado]

**Passo 2 — leitura da sonda**

A classe 2 foi definida nesta sonda como ato `resposta` com log `carimbar_resposta_pela_trilha` na pergunta; o código registra esse evento após criar a decisão. ([disputa_emissao.py:1685](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1685), [disputa_emissao.py:1701](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1701))

| Classe 2 | Resultado informado |
|---|---:|
| Total | 0 [não verificado] |
| Inversões por pk | 0 [não verificado] |
| Faixas de lag inserção − `ato_em` do log | Nenhuma linha de faixa para a classe 2 [não verificado] |
| Sem log de auditoria | 0 [não verificado] |

A classe 5 contém o restante, distribuído pelos **códigos de ato tal como aparecem na sonda**; a correspondência entre esses números e os nomes dos atos não consta do dado fornecido. [não verificado]

| Ato | Classe 5: quantidade |
|---:|---:|
| 1 | 1.367 [não verificado] |
| 2 | 1.758 [não verificado] |
| 3 | 1.264 [não verificado] |
| 4 | 17 [não verificado] |
| 5 | 500 [não verificado] |
| 6 | 737 [não verificado] |
| 8 | 65 [não verificado] |
| **Total** | **5.708** [não verificado] |

A linha 2 informa 244 inversões para a classe 5; as linhas 3 informam 1 na faixa `1_ate_1min`, 4 em `3_ate_1dia` e 239 em `4_mais_1dia`. A definição executada dessas faixas não está disponível na cópia apresentada. [não verificado]

As classes 1, 3 e 4 por prefixo de motivo **não são mensuráveis nesta sombra**, pois `ato_motivo` é mascarado; sua medição por motivo fica para produção. O código contém escritores com motivos relevantes para essa distinção. ([sombra_regras_pessoais.py:142](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/bin/sombra_regras_pessoais.py:142), [disputa_emissao.py:1679](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1679), [models.py:4119](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4119), [backfill_decisoes_pergunta.py:323](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/backfill_decisoes_pergunta.py:323))

**Passo 3 — candidatas e limite**

- **A — foto histórica:** a sonda informa **0 lavras movidas**, por construção; `marcos_do_lote` lê marcos de batidas, células e perguntas, sem ler `DecisaoPergunta`. Isso descreve essa candidata, não uma medição de inserções posteriores. ([veredito_lavrado.py:151](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/veredito_lavrado.py:151))
- **B — provadas pela classe 2:** a sonda informa **0 lavras movidas provadas**, no critério de inserção posterior a `julgado_em`; a classe 2 também tem total 0 na saída. Isso não mede as demais classes. [não verificado]
- **Cota superior, linha 9:** a saída informa **0** para cada uma das classes 1, 2 e 4 e **0** para a união `qualquer_1_2_4`. É o limite reportado pela sonda, não uma contagem de movimentos provados; sua definição de cálculo não está no dado fornecido e as classes por motivo estão mascaradas na sombra. [não verificado]

Falta a coluna de **instante de inserção** em `DecisaoPergunta` para comparar diretamente esse instante com `julgado_em`; o modelo declara `ato_em` como relógio do ato. ([models.py:4019](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4019), [models.py:4051](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4051))

**Passo 4 — pergunta ao Ronald**

Ronald, a sombra informa 0 lavras movidas para A, 0 provadas para B e cota 0 na linha 9; as classes 1, 3 e 4 por motivo estão mascaradas. [não verificado]  
Como deseja tratar esses números na decisão do item 4, considerando a medição por motivo em produção e a ausência de instante de inserção em `DecisaoPergunta`? ([sombra_regras_pessoais.py:142](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/bin/sombra_regras_pessoais.py:142), [models.py:4051](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4051))

## 18/11 10:32 · sessao d60dbd8c2c65 · RESPOSTAS-DE-LEI-1811-1010 · sonda · origem=Code

```text
=== SOMBRA (sombra_db, leitor) 18/11 10:28:42 ===
sonda_leitura: sonda_i5.py [9504050b83aadff2] no cpuset 4-7, lendo sombra como leitor (LEITURA)

agora 2026-11-18T10:28:45.391321-03:00 SILENCIO 11 REABERTURA 29
SILENCIO pk16360 col42 dia=2026-08-25 no_app_hoje=True razao=[] acoes=[] | HOJE rota=porta_acao fio=admin nao_sei=False fila=0 | APOS_F5 rota=porta_acao fio=admin nao_sei=False fila=0 | porta_da_tarefa_hoje=ok
SILENCIO pk24694 col925 dia=2026-09-05 no_app_hoje=True razao=['silencio_decidido'] acoes=[('resolver_dia', 'admin')] | HOJE rota=porta_acao fio=admin nao_sei=False fila=0 | APOS_F5 rota=porta_acao fio=admin nao_sei=False fila=0 | porta_da_tarefa_hoje=ok
SILENCIO pk29676 col925 dia=2026-09-05 no_app_hoje=True razao=['silencio_decidido'] acoes=[] | HOJE rota=porta_acao fio=admin nao_sei=False fila=0 | APOS_F5 rota=porta_acao fio=admin nao_sei=False fila=0 | porta_da_tarefa_hoje=ok
SILENCIO pk31420 col925 dia=2026-09-05 no_app_hoje=True razao=['silencio_decidido'] acoes=[] | HOJE rota=porta_acao fio=admin nao_sei=False fila=0 | APOS_F5 rota=porta_acao fio=admin nao_sei=False fila=0 | porta_da_tarefa_hoje=ok
SILENCIO pk31421 col925 dia=2026-09-05 no_app_hoje=True razao=['silencio_decidido'] acoes=[] | HOJE rota=porta_acao fio=admin nao_sei=False fila=0 | APOS_F5 rota=porta_acao fio=admin nao_sei=False fila=0 | porta_da_tarefa_hoje=ok
SILENCIO pk43761 col510 dia=2026-10-14 no_app_hoje=True razao=[] acoes=[] | HOJE rota=None fio=nada nao_sei=False fila=0 | APOS_F5 rota=None fio=nada nao_sei=False fila=0 | porta_da_tarefa_hoje=None
SILENCIO pk46070 col510 dia=2026-10-19 no_app_hoje=True razao=[] acoes=[] | HOJE rota=None fio=nada nao_sei=False fila=0 | APOS_F5 rota=None fio=nada nao_sei=False fila=0 | porta_da_tarefa_hoje=None
SILENCIO pk46440 col510 dia=2026-10-20 no_app_hoje=True razao=[] acoes=[] | HOJE rota=None fio=nada nao_sei=False fila=0 | APOS_F5 rota=None fio=nada nao_sei=False fila=0 | porta_da_tarefa_hoje=None
SILENCIO pk53787 col42 dia=2026-11-04 no_app_hoje=True razao=['silencio_decidido'] acoes=[] | HOJE rota=None fio=nada nao_sei=False fila=0 | APOS_F5 rota=None fio=nada nao_sei=False fila=0 | porta_da_tarefa_hoje=None
SILENCIO pk53837 col42 dia=2026-11-04 no_app_hoje=True razao=['silencio_decidido'] acoes=[] | HOJE rota=None fio=nada nao_sei=False fila=0 | APOS_F5 rota=None fio=nada nao_sei=False fila=0 | porta_da_tarefa_hoje=None
SILENCIO pk53875 col42 dia=2026-11-04 no_app_hoje=True razao=['silencio_decidido'] acoes=[] | HOJE rota=None fio=nada nao_sei=False fila=0 | APOS_F5 rota=None fio=nada nao_sei=False fila=0 | porta_da_tarefa_hoje=None
SILENCIO TOTAL 11
SILENCIO    ('apos', 'porta_acao', 'admin') 5
SILENCIO    ('apos', None, 'nada') 6
SILENCIO    ('hoje', 'porta_acao', 'admin') 5
SILENCIO    ('hoje', None, 'nada') 6
SILENCIO SEM_PORTA hoje (rota None): 6 | apos F5 (rota None): 6
REABERTURA pk21482 col822 dia=2026-08-28 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=None fio=nao_sei nao_sei=True fila=0 | APOS_F5 rota=None fio=nao_sei nao_sei=True fila=0 | porta_da_tarefa_hoje=ok
REABERTURA pk21483 col822 dia=2026-08-28 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=None fio=nao_sei nao_sei=True fila=0 | APOS_F5 rota=None fio=nao_sei nao_sei=True fila=0 | porta_da_tarefa_hoje=ok
REABERTURA pk21484 col822 dia=2026-08-28 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=None fio=nao_sei nao_sei=True fila=0 | APOS_F5 rota=None fio=nao_sei nao_sei=True fila=0 | porta_da_tarefa_hoje=ok
REABERTURA pk32973 col822 dia=2026-09-18 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_app fio=colab nao_sei=True fila=0 | APOS_F5 rota=porta_app fio=colab nao_sei=True fila=0 | porta_da_tarefa_hoje=ok
REABERTURA pk36273 col446 dia=2026-09-25 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_pergunta fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_pergunta fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk36383 col169 dia=2026-09-25 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk39989 col836 dia=2026-10-04 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_app fio=colab nao_sei=True fila=0 | APOS_F5 rota=porta_app fio=colab nao_sei=True fila=0 | porta_da_tarefa_hoje=ok
REABERTURA pk42735 col836 dia=2026-10-11 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_app fio=colab nao_sei=True fila=0 | APOS_F5 rota=porta_app fio=colab nao_sei=True fila=0 | porta_da_tarefa_hoje=ok
REABERTURA pk45597 col836 dia=2026-10-18 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_app fio=colab nao_sei=True fila=0 | APOS_F5 rota=porta_app fio=colab nao_sei=True fila=0 | porta_da_tarefa_hoje=ok
REABERTURA pk45691 col848 dia=2026-10-18 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk45776 col363 dia=2026-10-19 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk47212 col908 dia=2026-10-22 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_pergunta fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_pergunta fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk49033 col822 dia=2026-10-22 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk49034 col822 dia=2026-10-22 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk49035 col822 dia=2026-10-22 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk49036 col822 dia=2026-10-22 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk49668 col247 dia=2026-10-24 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk50478 col148 dia=2026-10-26 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk51099 col902 dia=2026-10-28 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk51329 col363 dia=2026-10-23 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk51667 col822 dia=2026-10-29 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk51685 col822 dia=2026-10-29 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk51763 col822 dia=2026-10-29 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk53093 col902 dia=2026-11-02 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk53139 col902 dia=2026-11-02 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk53172 col902 dia=2026-11-02 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk53250 col902 dia=2026-11-02 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk53554 col848 dia=2026-11-03 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk55305 col1020 dia=2026-11-08 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA TOTAL 29
REABERTURA    ('apos', 'porta_app', 'colab') 4
REABERTURA    ('apos', 'porta_fila', 'admin') 20
REABERTURA    ('apos', 'porta_pergunta', 'admin') 2
REABERTURA    ('apos', None, 'nao_sei') 3
REABERTURA    ('hoje', 'porta_app', 'colab') 4
REABERTURA    ('hoje', 'porta_fila', 'admin') 20
REABERTURA    ('hoje', 'porta_pergunta', 'admin') 2
REABERTURA    ('hoje', None, 'nao_sei') 3
REABERTURA SEM_PORTA hoje (rota None): 3 | apos F5 (rota None): 3
rc_sombra=0
```

linhas copiadas: 61, incluindo a linha em branco.

Os números abaixo são contagens do **dado fornecido acima**; a execução e a origem da sonda não foram verificadas de forma independente. [não verificado]

| Classe | Cenário | Com dono e rota | Sem porta | Total |
|---|---|---:|---:|---:|
| SILENCIO | HOJE | 5 admin, `porta_acao` | 6 `nada` | 11 |
| SILENCIO | APOS_F5 | 5 admin, `porta_acao` | 6 `nada` | 11 |
| REABERTURA | HOJE | 4 colab, `porta_app`; 22 admin, 20 `porta_fila` e 2 `porta_pergunta` | 3 `nao_sei` | 29 |
| REABERTURA | APOS_F5 | 4 colab, `porta_app`; 22 admin, 20 `porta_fila` e 2 `porta_pergunta` | 3 `nao_sei` | 29 |

A tabela conta a **rota da tarefa do dia**, conforme impressa pela sonda, inclusive quando `nao_sei=True`; esse indicador pode coexistir com uma rota. Um envelope de `porta_da_tarefa` com rota `None` também não é uma rota publicada: por isso os três casos `nao_sei` contam como sem porta, embora a sonda imprima `porta_da_tarefa_hoje=ok`. [app/chamados/services/tarefa_do_dia.py:718](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:718); [não verificado]

**Sem porta nos dois cenários**, segundo o dado fornecido:

- SILENCIO: pk43761, pk46070, pk46440, pk53787, pk53837, pk53875.
- REABERTURA: pk21482, pk21483, pk21484. [não verificado]

Nenhuma das 40 perguntas passa a ter dono que não tinha, nem deixa de ter: a rota e o fio impressos são iguais nos dois cenários para cada pk. A razão exata de cada uma das cinco portas administrativas de SILENCIO não é toda demonstrada pelas ações da instância impressa; a tarefa agrega ações de todas as instâncias e também pode receber `resolver_dia` da autoridade do calendário. Para pk24694 essa ação aparece na própria saída; para as demais, sua origem específica é [não verificado]. [app/chamados/services/tarefa_do_dia.py:611](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:611); [app/chamados/services/tarefa_do_dia.py:643](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:643).

**As 29 REABERTURA não mostram ação `responder` na instância da pergunta.** Todas imprimem `passivo_sem_linha` e `acoes=[]`: em `_pela_decisao`, a verificação de passivo vem antes da regra que, para uma REABERTURA não interceptada, retornaria `responder`. O código exige tanto a pergunta em `_pendentes_app` quanto uma ação `responder` do colaborador naquela instância para `colab_tem_porta`. Assim, colocar essas perguntas no conjunto não cria essa ação. As quatro rotas `porta_app` publicadas pertencem à tarefa do dia; qual outra instância as origina não aparece no recorte. [app/chamados/services/tarefa_do_dia.py:154](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:154); [app/chamados/services/tarefa_do_dia.py:163](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:163); [app/chamados/services/tarefa_do_dia.py:657](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:657); [não verificado]

Para SILENCIO, `_pela_decisao` retorna `silencio_decidido` com ações vazias, mas isso **não fecha necessariamente a porta da tarefa inteira**: o código exclui a pergunta encerrada de `portas_pergunta`, enquanto a âncora ainda pode entrar em `portas` se estiver na fila, sujeita à seleção por verbo; outras ações também podem dar dono ao dia. No dado, os 11 imprimem `fila=0`, portanto ele não demonstra uma âncora de fila alcançando esses casos. A ação `resolver_dia` impressa em pk24694 é compatível com o acréscimo posterior pela tarefa. [app/chamados/services/tarefa_do_dia.py:176](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:176); [app/chamados/services/tarefa_do_dia.py:643](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:643); [app/chamados/services/tarefa_do_dia.py:690](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:690); [não verificado]

O contrafactual é **fiel ao efeito informado sobre a pertinência dessas 40 perguntas em `_pendentes_app`**, condicionado às premissas fornecidas de que estão vivas e de que a F5 mudaria suas partições como descrito: `_ler_vida` toma os pendentes da partição e ainda filtra por `pergunta_viva`. Ele não executa a F5 real nem comprova seus demais efeitos, a partição de outras perguntas, mudanças em outras entradas da carga ou o comportamento de outras telas. O brief F5 e o código da sonda não estavam disponíveis na cópia; a implementação efetiva e a fidelidade integral são [não verificado]. [app/chamados/services/tarefa_do_dia.py:530](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:530); [app/chamados/services/pergunta_no_app.py:76](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/pergunta_no_app.py:76).

Pergunta para Ronald: **“Avaliamos a F5 sabendo que, nesta sonda, os 11 SILENCIO permanecem em 5 com dono e 6 sem porta e as 29 REABERTURA em 26 com dono e 3 sem porta, sem mudança de dono em nenhum dos 40 casos?”** [não verificado]

## 18/11 10:31 · sessao 6596f28a6d0e · RESPOSTAS-DE-LEI-1811-1010 · sonda · origem=Code

```text
---- DADO RASPADO (sonda/anexos) ----
=== SOMBRA (sombra_db, leitor) 18/11 10:30:44 ===
sonda_leitura: sonda_i4.py [cae9871ac1d5b64e] no cpuset 4-7, lendo sombra como leitor (LEITURA)

(1, 1, 5, 1367)
(1, 2, 5, 1758)
(1, 3, 5, 1264)
(1, 4, 5, 17)
(1, 5, 5, 500)
(1, 6, 5, 737)
(1, 8, 5, 65)
(2, 1, 0, 0)
(2, 2, 0, 0)
(2, 3, 0, 0)
(2, 4, 0, 0)
(2, 5, 5708, 244)
(3, 5, '1_ate_1min', 1)
(3, 5, '3_ate_1dia', 4)
(3, 5, '4_mais_1dia', 239)
(5, 'classe2_sem_log_auditoria', 0)
(6, 'lavras_total', 47318)
(7, 'candidata_A_foto_historica_move', 0, 'marcos_do_lote nao le DP (veredito_lavrado.py:151-208), lavras que mudam = 0 por construcao')
(8, 'candidata_B_provada_classe2', 0)
(9, 'cota_classe1', 0)
(9, 'cota_classe2', 0)
(9, 'cota_classe4', 0)
(9, 'cota_qualquer_1_2_4', 0)
(10, 'total_dp', 5708)
rc_sombra=0
---- FIM DO DADO ----
```

linhas copiadas: 30, incluindo delimitadores e linha vazia.

Os números abaixo são a transcrição e operações aritméticas sobre o dado fornecido; a execução e o conteúdo da sonda não são verificáveis nesta cópia: **[não verificado]**.

| Escritor / classe de motivo | Total DP | `ato_em` anterior à inserção provada pelo log | Inversões por pk — **proxy** | O que não se mede |
|---|---:|---:|---:|---|
| 1 — projeção sem linha | 0 | Não medido | 0 | Inserção real e atraso; a projeção fornece `ato_em` histórico ([models.py:4122](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4122)). |
| 2 — pela trilha | 0 | **0** | 0 | Não há linhas para medir atraso ou faixas; sem log: 0. O escritor grava a DP e depois emite o evento ([disputa_emissao.py:1685](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1685)). |
| 3 — resposta automática | 0 | Não medido | 0 | Inserção real e atraso ([disputa_emissao.py:1655](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1655)). |
| 4 — backfill E1 de silêncio | 0 | Não medido | 0 | Inserção real e atraso; o backfill usa `materializada_em` ([backfill_decisoes_pergunta.py:323](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/backfill_decisoes_pergunta.py:323)). |
| 5 — demais | 5.708 | Não medido | 244 | Inserção real e atraso por log não foram medidos para esta classe; ela reúne vários escritores, entre eles [resposta](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1525), [reabertura/recusa](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1632), [validação](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/validacao.py:173), [devolução](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/validacao.py:279), [silêncio](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:2013), [retratação](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:2145) e [absorção](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1048). |

A soma é **5.708 = `total_dp`**. Das 244 inversões por pk, 1 está em até 1 minuto, 4 na faixa `3_ate_1dia` e 239 em mais de 1 dia; inversão é comparação entre `ato_em` e atos de pk menor, não prova do instante de inserção. **[não verificado: saída da sonda fornecida]**

Das **47.318 lavras** do recorte informado **[não verificado: saída da sonda fornecida]**:

- **A — manter a foto histórica:** **0 lavras movidas por DP, por construção**; `marcos_do_lote` não consulta DP ([veredito_lavrado.py:151](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/veredito_lavrado.py:151)). O efeito de colocar na F3 somente o marcador do placar é **[não verificado]**.
- **B — marco por `Max(pk)`/`Count` de DP:** **0 lavras provadas** pela classe 2 e **cota do recorte 1/2/4 = 0**; o número total de lavras que B moveria, seja com `ato_em <= ate`, seja sem filtro, é **[não verificado]**. O marco atual e sua comparação com `julgado_em` estão em [veredito_lavrado.py:201](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/veredito_lavrado.py:201) e [veredito_lavrado.py:230](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/veredito_lavrado.py:230).

A cota **0** conta lavras com DP do mesmo colaborador nas classes 1, 2 ou 4 e `ato_em <= julgado_em`, independentemente de se provar inserção posterior; ela não cobre a classe 5 nem estabelece o efeito total de B; para verificar a ordem de inserção faltaria uma coluna de criação em `DecisaoPergunta`, junto aos campos em [models.py:4051](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4051), ou um snapshot histórico comparável **[não verificado para os registros existentes]**.

Ronald, você escolhe **A**, manter a foto histórica e usar na F3 só o marcador do placar — **0 lavras movidas por DP por construção** ([veredito_lavrado.py:151](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/veredito_lavrado.py:151)) — ou **B**, acrescentar marco `Max(pk)`/`Count` das DP, com `ato_em <= ate` ou sem filtro — **0 provadas pela classe 2, cota 0 nas classes 1/2/4, total movido [não verificado]** ([models.py:4051](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4051))? **[não verificado: números da sonda fornecida]**

## 18/11 10:31 · sessao c02f51090e3f · RESPOSTAS-DE-LEI-1811-1010 · leitura · origem=Code

A cópia examinada não tem metadados Git; portanto, sua identidade como HEAD `dd489c627` está **[não verificado]**. As referências a arquivos abaixo são da cópia aberta. Nos anexos, a numeração começa na primeira linha de conteúdo de cada bloco.

A conclusão técnica é que **nenhum dos três escritores existentes registra qualquer uma das duas marcas pedidas com seu significado e universo**. Um escritor novo é a proposta para ambas, mas a lei ainda precisa decidir se “nenhuma” é um ato persistido ou a leitura da ausência de decisão, e quais efeitos e transições pertencem a “trabalho reconhecido”. O brief exige uma porta única de decisão e condiciona a abertura aos cortes L1 e L2. (brief_dst.md:L2–L3, L24, L28–L30, L32–L34, L48–L49; [he.py:159](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/he.py:159), [celula.py:572](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:572), [retratar_humano.py:150](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/retratar_humano.py:150))

### 1. Matriz de transições

`RED` abaixo significa **caso a especificar**, não teste existente conferido. Para todas as linhas DST, o juiz e a operação completos estão **[não verificado]**; a tabela distingue o comportamento pedido do que o código já faz. `decisao_do_dia` lê veredictos humanos de Resolver dia, cujo vocabulário é outro. ([veredito_celula.py:18](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:18), [veredito_celula.py:85](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:85))

| Estado / transição | Juiz | Operação | O que persiste | Retorno / recusa | Prova |
|---|---|---|---|---|---|
| **A: sem marca → nenhuma** | Juiz DST **[não verificado]**. `decisao_do_dia` devolve `{}` quando não encontra veredito humano; isso não demonstra uma marca persistida “nenhuma”. | Escritor DST **[não verificado]**; depende da lei sobre ausência versus ato. | Marca, modelo e trilha DST **[não verificado]**. | Retorno e recusa **[não verificado]**. | [veredito_celula.py:104](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:104); **RED A1:** distinguir ausência de linha e marca, se a lei exigir marca. |
| **A: nenhuma → outra decisão** | Juiz DST **[não verificado]**. | Transição DST **[não verificado]**; HE, correção de grade e retratação são atos distintos. | Antes/depois da decisão DST **[não verificado]**. | Estados permitidos e recusa **[não verificado]**. | brief_dst.md:L30; **RED A2:** trocar para cada saída aprovada e conservar a trilha. |
| **A: repetida 2×** | Juiz DST **[não verificado]**. | Operação DST **[não verificado]**. | Uma trilha e nenhuma duplicata são o contrato de desenho do brief, ainda não demonstrado para A. | Retorno específico **[não verificado]**. | brief_dst.md:L28; **RED A3:** repetir a mesma entrada e verificar escrita e trilha. |
| **A: revogada** | Juiz de vigência DST **[não verificado]**. | Revogação DST **[não verificado]**. | O brief exige conservar decisão tomada na trilha quando o fato muda; campos e significado de revogar “nenhuma” **[não verificado]**. | Retorno e estado posterior **[não verificado]**. | brief_dst.md:L29; **RED A4:** conservar história e definir leitura após revogação. |
| **B: sem decisão → trabalho reconhecido** | Juiz DST **[não verificado]**. O admin é o decisor previsto no brief. | Escritor DST **[não verificado]**. Resolver dia com `trabalhou` recusa quando a grade calculada tem `tipo_dia != trabalho`; HE é decisão separada. | Registro DST **[não verificado]**; `DecisaoHE` guarda decisão sobre minutos fora da janela, não reconhecimento do trabalho. | Resolver dia retorna `ok=False` nesse ramo; HE recusa minutos não positivos. | brief_dst.md:L30; [veredito_celula.py:243](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:243), [he.py:186](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/he.py:186); **RED B1**. |
| **B: reconhecido → escala corrigida** | Juiz da troca DST **[não verificado]**; vínculo da edição é obtido por `vinculo_do_dia_carregado`. | `editar_dia(..., trabalha=True)` já faz a correção da célula; não demonstra troca de decisão DST. | `CelulaDia`: trabalho, DNA, vínculo, origem, autor e instante; trilha da edição com antes/depois e motivo. | `editada`, `igual`, `barrada` ou `sem_vinculo`; transição DST **[não verificado]**. | [celula.py:572](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:572); **RED B2**. |
| **B: reconhecido → batida invalidada** | Juiz da troca DST **[não verificado]**. | `retratar` existe por batida, mas a troca da decisão por pessoa × dia **[não verificado]**. | A batida recebe `aprovada=False` e `retratada_em`; trilha da retratação. Persistência da troca DST **[não verificado]**. | `retratar` devolve `(ok, mensagem)`; exige permissão e motivo ativo. | [retratar_humano.py:150](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/retratar_humano.py:150), [registro_batida.py:287](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/registro_batida.py:287); **RED B3**. |
| **B: repetida 2×** | Juiz DST **[não verificado]**. | Operação DST **[não verificado]**. | Uma trilha e zero duplicata são requisitos do desenho, não comportamento demonstrado para B. | Retorno específico **[não verificado]**. | brief_dst.md:L28; **RED B4**. |
| **B: revogada** | Juiz de vigência DST **[não verificado]**. `vigentes()` é juiz da decisão **HE**, não da DST. | Revogação DST **[não verificado]**. | Trilha da decisão DST e seus campos de revogação **[não verificado]**; o brief pede retenção da decisão quando o fato muda. | Efeito sobre HE, grade e batidas **[não verificado]**. | brief_dst.md:L29; [models.py:1265](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/models.py:1265); **RED B5**. |

### 2. Encaixe nos três escritores

As respostas sobre **universo** significam se a operação aceita *todos* os dias da unidade pedida, não se ela pode ser chamada em algum exemplo desse universo. (brief_dst.md:L3)

| Escritor | Vocabulário contém A / B? | Aceita o universo DST? | Trilha e repetição |
|---|---|---|---|
| `decidir_he` | **Não / não.** Apenas `autorizado` e `nao`, relativos à HE fora da janela. | **Não integralmente.** Exige minutos positivos fornecidos pelo chamador, além de permissão e, para autorizar, motivo; não valida a unidade DST como tal. | Guarda estado, minutos, motivo, autor e datas; tenta trilha antes/depois. A trilha é **best effort**, portanto não garantida. Mesma decisão vigente retorna `no_op`, sem nova trilha. ([models.py:1283](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/models.py:1283), [he.py:175](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/he.py:175), [he.py:89](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/he.py:89), [he.py:195](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/he.py:195)) |
| `editar_dia` | **Não / não.** Aceita `trabalha=True/False`, que altera a grade. | **Não integralmente.** Pode editar dia sem trabalho, mas exige vínculo, autor e motivo e tem guarda de competência; não exige batida apurável. | Guarda autor, instante, motivo e antes/depois; falha da trilha desfaz a edição. Edição já equivalente retorna `igual` sem escrever. ([celula.py:572](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:572), [celula.py:597](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:597), [celula.py:632](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:632)) |
| `retratar` | **Não / não.** Registra que uma marcação não foi trabalho. | **Não como decisão de dia.** Recebe uma batida individual; pode atuar numa batida do universo, mas não representa a decisão sobre pessoa × dia. | Exige ator autorizado e motivo ativo; passa motivo e antes/depois à auditoria, que pode falhar sem barrar a gravação. Repetir sobre batida retratada retorna sucesso sem nova trilha. ([retratar_humano.py:131](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/retratar_humano.py:131), [retratar_humano.py:150](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/retratar_humano.py:150), [registro_batida.py:255](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/registro_batida.py:255), [auditoria.py:17](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/core/auditoria.py:17)) |

- **A — EXIGE escritor novo se “nenhuma” tiver de ser uma marca persistida.** Nenhum dos três escreve essa marca; se for apenas a leitura da ausência de decisão, a necessidade de escritor para A depende da lei. ([models.py:1287](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/models.py:1287), [celula.py:575](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:575), [retratar_humano.py:150](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/retratar_humano.py:150))
- **B — EXIGE escritor novo para a decisão própria**, sujeito ao corte da lei: HE decide minutos, a edição muda a grade e a retratação invalida uma batida. O escritor novo proposto pode coordenar os efeitos existentes que a lei escolher, dentro da porta única prevista no brief. (brief_dst.md:L24, L30; [he.py:159](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/he.py:159), [celula.py:572](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:572), [retratar_humano.py:150](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/retratar_humano.py:150))

### 3. Frota demonstrada pelos anexos

- **Medida da sombra, 17/11:** dos 170 dias `sem_trabalho` com órfã, o relato fecha **93 pagos sem decisão + 72 `ESTRUTURA_DINHEIRO` + 3 com porta/pergunta + 1 HE autorizada + 1 HE negada = 170**. Dos 72, 50 tinham duas ou mais batidas; o relato informa zero com relógio positivo. Essa é uma medida distinta do retrato gravado da folha. (relato_72.txt:L7–L8)
- **Recorte detalhado do retrato:** as 55 linhas pessoa × dia de `retrato_a2` correspondem às datas das linhas selecionadas de `retrato_a`; **5** têm simultaneamente `trabalha=False` e batidas apuráveis positivas: os cinco dias de `col348`. As outras **50** não satisfazem as duas condições *nesses campos impressos*. (retrato_a.saida.txt:L4–L24; retrato_a2.compacto.txt:L1–L55, em especial L47–L51)
- **Limite da contagem pelo juiz:** das outras 50, **25** têm zero batidas apuráveis e **25** têm `trabalha=True` e batidas positivas. Para estas últimas, o anexo não imprime `ata.tipo_dia` nem o carimbo necessário da ausência parcial: `dia_sem_trabalho` também pode considerar a ata. Logo, **5 são confirmados pelo `trabalha=False`; o total segundo o juiz completo e a classificação definitiva dos outros 25 com batidas estão [não verificado]**. Não se pode extrapolar as 55 linhas selecionadas para todas as linhas do retrato. (retrato_a2.compacto.txt:L1–L55; [juizes.py:445](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:445); [retrato_folha.py:98](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/retrato_folha.py:98))
- **Leitor e rótulo:** no retrato gravado, `col348` aparece como `furo_espelho`, fio `ninguem`, balde `sem_acao_disponivel`, sem ações; o rótulo desse balde é **“Sem ação disponível”**. `tarefa_do_dia` informa, para esses cinco dias, `fio=None`; o retrato é uma leitura por pessoa e não comprova um rótulo DST específico para cada dia. (retrato_a.saida.txt:L20; retrato_a2.compacto.txt:L47–L51; [retrato_folha.py:191](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/retrato_folha.py:191), [tarefa_do_dia.py:718](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:718))

**Perguntas de lei para Ronald:** “nenhuma” deve ser uma marca persistida, com ato e revogação, ou o juiz deve devolvê-la quando não houver decisão vigente? “Trabalho reconhecido” é decisão própria independente de autorização de HE; que efeitos, trocas e revogação ela produz, inclusive quando não há minutos positivos de HE? Permanecem também os cortes L1 e L2 exigidos para abertura. (brief_dst.md:L30, L32–L34, L48–L49; [he.py:186](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/he.py:186))

FRASE DE CORTE DST-L1: (A) a marca 'nenhuma' [não verificado: depende da pergunta de lei acima] ; (B) 'trabalho reconhecido' nasce como escritor novo decidir_dstl1 [proposta sujeita ao corte de lei]; juiz novo decisao_admin_dia_sem_trabalho [proposta]; trilha quem, quando, motivo, antes/depois e revogação [campos propostos]; universo pessoa × dia do turno cuja célula diz sem trabalho e há batida apurável; frota 72 de 170 na sombra, retrato detalhado 5 confirmados, 25 fora por zero batidas e 25 dependentes da ata.

## 18/11 10:28 · sessao a2164c7661b8 · RESPOSTAS-DE-LEI-1811-1010 · leitura · origem=Code

## (a) O que a F3 propõe e o que existe hoje

O brief propõe que **os dois marcadores incluam um marco das linhas de `DecisaoPergunta` por `Max(pk)` ou `Count`**, além do marco de efeito; rejeita `max(ato_em)` porque uma linha inserida com `ato_em` antigo pode deixar esse máximo inalterado. T3 exige mudança com ato sem alteração da projeção; T3b exige mudança com a conservação retroativa. (`brief.md:64`, `brief.md:65`, `brief.md:79`, `brief.md:82`, `matriz.md:84`, `matriz.md:111`, `matriz.md:132`; linhas dos anexos fornecidos, correspondência com arquivos externos **[nao verificado]**)

Para `marcos_do_lote`, a F3 acrescenta que o marco deve respeitar `ate`, mas **não especifica o filtro que acompanha `Max(pk)` ou `Count`**. Se filtrados por `ato_em <= ate`, ambos mudam quando entra uma linha retroativa elegível; `max(ato_em)` pode não mudar. Sem filtro, `Max(pk)` e `Count` também veem linhas cujo ato é posterior a `ate`. Nenhuma dessas formas, isoladamente, identifica quais linhas já estavam inseridas em `ate`. (`brief.md:65`, `brief.md:82`, `matriz.md:111`, `app/chamados/models.py:4051`, `app/chamados/models.py:4077`)

Hoje, `marcos_do_lote` **não lê `DecisaoPergunta`**: agrega tempos de Batida, CelulaDia e os campos projetados `materializada_em` e `respondida_em` de PerguntaDisputa, aplicando `ate` aos tempos. Seu retorno é um `datetime` por colaborador; `esta_stale` compara esse tempo com `julgado_em`. O marcador do placar agrega campos de ChamadoColaborador e os três tempos projetados de PerguntaDisputa. (`app/chamados/veredito_lavrado.py:151`, `app/chamados/veredito_lavrado.py:163`, `app/chamados/veredito_lavrado.py:177`, `app/chamados/veredito_lavrado.py:191`, `app/chamados/veredito_lavrado.py:202`, `app/chamados/veredito_lavrado.py:230`, `app/chamados/services/painel.py:62`)

`Max(pk)` e `Count` são inteiros, portanto não podem simplesmente ser acrescentados ao máximo temporal atual e comparados com `julgado_em`; precisariam de outro contrato de comparação e de um valor correspondente conservado na lavra. Essa mudança não está especificada nos anexos. (`app/chamados/veredito_lavrado.py:151`, `app/chamados/veredito_lavrado.py:230`, `matriz.md:132`)

## (b) Censo de escritores e limite do instante de inserção

**Há escritor retroativo fora do backfill:** `carimbar_resposta_pela_trilha` usa o instante da nota anterior, recebido como argumento; o comando de manutenção o chama para curar perguntas em limbo. Portanto, a condição “só backfill” do corte retirado não está demonstrada e é contrariada por esse caminho de código. (`app/chamados/services/disputa_emissao.py:1685`, `app/chamados/services/disputa_emissao.py:1701`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`, `corte_retirado.txt:1`)

“Anterior à inserção”, literalmente, também inclui o intervalo entre colher `timezone.now()` e executar o INSERT. A distinção útil abaixo é entre **instante histórico recebido/conservado**, que pode ser arbitrariamente anterior, e **agora capturado durante a operação**, que necessariamente antecede ou coincide com a execução posterior, sem medir sua duração. (`app/chamados/services/disputa_emissao.py:1476`, `app/chamados/services/disputa_emissao.py:1525`, `app/chamados/models.py:4162`)

| Escritor ou fonte de linhas | Origem de `ato_em` e retroatividade |
|---|---|
| `linhas_da_projecao_sem_linha` | Copia `respondida_em` ou `validada_em`; pode ser histórico. É chamada pelos pontos de ressuscitação e nascimento, resposta humana/automática, validação, devoluções, cura L-130 e backfill E4b. (`app/chamados/models.py:4122`, `app/chamados/models.py:4141`, `app/chamados/models.py:4146`, `app/chamados/models.py:1619`, `app/chamados/models.py:3137`, `app/chamados/services/disputa_emissao.py:1480`, `app/chamados/services/disputa_emissao.py:1674`, `app/chamados/services/validacao.py:167`, `app/chamados/services/validacao.py:272`, `app/chamados/services/validacao.py:345`, `app/chamados/services/validacao.py:436`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`) |
| Resposta humana | `_agora = timezone.now()` antes da transação e inserção. A mesma chamada também insere as linhas históricas de conservação. (`app/chamados/services/disputa_emissao.py:1476`, `app/chamados/services/disputa_emissao.py:1525`) |
| Resposta automática | Recebe `instante`; o chamador de produção indicado passa o agora do processamento. A assinatura aceita instante anterior; também conserva projeção histórica. (`app/chamados/services/disputa_emissao.py:1655`, `app/chamados/services/disputa_emissao.py:1671`, `app/chamados/services/disputa_emissao.py:1679`, `app/api/foto_recebida.py:48`) |
| Resposta pela trilha | Recebe instante da nota da trilha, possivelmente histórico; chamada pelo comando de cura. (`app/chamados/services/disputa_emissao.py:1685`, `app/chamados/services/disputa_emissao.py:1701`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`) |
| Reabertura e recusa em lote | `timezone.now()` no início da transação, antes da inserção das linhas. (`app/chamados/services/disputa_emissao.py:1622`, `app/chamados/services/disputa_emissao.py:1632`) |
| Validação | `perg.validada_em`, atribuído a `timezone.now()` na operação; acompanha eventuais linhas históricas da projeção. (`app/chamados/services/validacao.py:167`, `app/chamados/services/validacao.py:168`, `app/chamados/services/validacao.py:173`) |
| Devolução normal e BUG-PAR | `timezone.now()` na montagem da linha; ambas podem inserir também conservação histórica. (`app/chamados/services/validacao.py:272`, `app/chamados/services/validacao.py:279`, `app/chamados/services/validacao.py:345`, `app/chamados/services/validacao.py:354`) |
| Silêncio no fechamento de disputa e no dia decidido | `materializada_em` do carimbo recém-feito pela porta; capturado antes da inserção. (`app/chamados/models.py:2010`, `app/chamados/models.py:2013`, `app/ponto/services/veredito_celula.py:190`, `app/ponto/services/veredito_celula.py:195`) |
| Retratação | `_agora_ret` da operação; inserção usa esse valor. (`app/chamados/models.py:2131`, `app/chamados/models.py:2145`) |
| Absorção | `agora` recebido pela operação e usado também no carimbo; a assinatura não garante que seja contemporâneo à inserção. (`app/chamados/services/materializacao.py:1046`, `app/chamados/services/materializacao.py:1048`) |
| Backfill de silêncio | Copia `p.materializada_em` histórico. O mesmo comando usa `linhas_da_projecao_sem_linha` para **medir** linhas candidatas; essa leitura não é, por si, inserção. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:44`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:67`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`) |
| Porta física | `registrar` recebe dicionários e faz `bulk_create`; não gera timestamp de inserção. O `save` do modelo também permite inserir uma instância sem pk, embora a busca estática por chamadas identificadas tenha encontrado a porta acima. Chamadas dinâmicas ou SQL externo: **[nao verificado]**. (`app/chamados/models.py:4111`, `app/chamados/models.py:4152`, `app/chamados/models.py:4162`) |
| Migração | A 0051 renomeia `RespostaAnterior` e seu instante para `DecisaoPergunta`/`ato_em`, preservando linhas antigas; sua função de dados mostrada atualiza PerguntaDisputa. Não é uma inserção nova de DP. O histórico completo de escritores anteriores à cópia: **[nao verificado]**. (`app/chamados/migrations/0051_decisao_pergunta.py:22`, `app/chamados/migrations/0051_decisao_pergunta.py:41`, `app/chamados/migrations/0051_decisao_pergunta.py:49`) |

O modelo declara `ato_em`, mas não declara coluna de instante de inserção; `registrar` também não registra uma trilha por DP que forneça esse instante. Eventos de operações sobre PerguntaDisputa existem, mas não constituem, pelo código citado, um registro universal e inequívoco da inserção de cada DP. Assim, **não é possível contar exatamente DP com `ato_em` anterior à inserção nem atribuí-las todas ao escritor**. (`app/chamados/models.py:4051`, `app/chamados/models.py:4077`, `app/chamados/models.py:4152`, `app/chamados/services/disputa_emissao.py:1701`, `app/chamados/services/disputa_emissao.py:1704`)

O substituto disponível é **ordem por pk**, como aproximação da ordem de alocação; comparar `ato_em` de uma linha com o maior `ato_em` de pks menores detecta inversões na sequência, não a idade no INSERT. Concorrência, transações e importações podem separar ordem de pk, execução e commit; mesmo sem inversão, uma linha pode ter instante histórico. (`app/chamados/models.py:4081`, `app/chamados/models.py:4162`; garantias efetivas da sequência e do banco em produção **[nao verificado]**)

## (c) Candidatas e script de leitura

Para um colaborador \(c\), conjunto atual de linhas \(D_c\) e corte \(t\):

- **A — por instante do ato:** \(A(D_c,t)=\max\{d.ato\_em : d\in D_c,\ d.ato\_em\le t\}\), ou vazio. É a leitura por instante descrita no corte retirado. Ela **inclui hoje** uma inserção posterior com ato anterior ao corte: o esquema não permite recuperar a foto das linhas efetivamente presentes naquele instante. (`corte_retirado.txt:1`, `app/chamados/models.py:4053`)
- **B — sensível à inserção posterior:** conceitualmente, exige comparar o conjunto atual de IDs com o conjunto de IDs presentes na lavra, ou com um marco de inserção conservado nela. O material pergunta por essa alternativa, mas **não define sua função exata**; o esquema mostrado não guarda esse conjunto nem um instante de inserção por DP. Assim, uma função B executável e a contagem pedida para ela são **[nao verificado]**. (`F3.r`, seção 4, pergunta aberta; `app/chamados/models.py:4051`, `app/chamados/veredito_lavrado.py:237`)

Há também uma consequência matemática para a medida solicitada: com `ate=julgado_em`, o valor de A nunca supera `julgado_em`. Logo, acrescentado como `datetime` aos marcos reais, ele **não pode acionar o terceiro ramo de `esta_stale`**. Comparar o *valor do marco* com e sem linhas históricas ainda pode revelar diferenças; isso não é o mesmo que contar lavras tornadas stale. O relógio ou os marcos reais podem tornar uma lavra stale independentemente de A. (`app/chamados/veredito_lavrado.py:213`, `app/chamados/veredito_lavrado.py:224`, `app/chamados/veredito_lavrado.py:228`, `app/chamados/veredito_lavrado.py:230`)

O script abaixo é uma **sonda parcial honesta**, pois as contagens exatas por escritor e B não são recuperáveis desses campos. Execute seu conteúdo em `manage.py shell` na sombra; ele só consulta e imprime tuplas numéricas. Não foi executado aqui. (`app/chamados/models.py:4051`, `app/chamados/veredito_lavrado.py:151`)

Rótulos numéricos das saídas:

- `(1, ato, classe, n)`: universo completo de DP por ato e classe de motivo; a soma de `n` é o total de DP. Ato: 1 resposta, 2 validação, 3 devolução, 4 recusa, 5 reabertura, 6 silêncio, 7 retratação, 8 absorção, 9 outro. Classe: 1 projeção sem linha, 2 pela trilha, 3 resposta automática, 4 backfill E1 de silêncio, 5 demais. Os prefixos vêm dos escritores citados. (`app/chamados/models.py:4037`, `app/chamados/models.py:4119`, `app/chamados/services/disputa_emissao.py:1680`, `app/chamados/services/disputa_emissao.py:1702`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:327`)
- `(2, classe, n, inversoes)`: total de DP da classe e quantas têm `ato_em` menor que o maior `ato_em` observado em **qualquer DP de pk menor**. A soma de `n` é o total de DP; inversão é somente aproximação, não contagem de inserções retroativas por escritor. (`app/chamados/models.py:4053`, `app/chamados/models.py:4081`)
- `(3, universo, marcos_diferentes, stale_com, stale_sem, mudou_stale)`: lavras com veredito não vazio e `julgado_em`, comparando A com todas as DP contra A sem classes 1 e 4; ambos combinados aos marcos reais calculados em `ate=julgado_em`. `esta_stale` é avaliada no mesmo `agora` capturado para os dois lados. `marcos_diferentes` conta diferença de **marco combinado**, não necessariamente de veredito. (`app/chamados/veredito_lavrado.py:151`, `app/chamados/veredito_lavrado.py:213`)
- `(4, total_dp, total_lavras)`: totais de controle. B e retroatividade real por escritor não recebem número substituto. (`app/chamados/models.py:4051`, `app/chamados/veredito_lavrado.py:237`)

```python
from collections import Counter
from django.db.models import Max
from django.utils import timezone

from chamados.models import DecisaoPergunta as DP, PerguntaDisputa as PD
from chamados.veredito_lavrado import marcos_do_lote, esta_stale

atos = {
    "resposta": 1,
    "validacao": 2,
    "devolucao": 3,
    "recusa": 4,
    "reabertura": 5,
    "silencio": 6,
    "retratacao": 7,
    "absorcao": 8,
}

def classe(motivo):
    motivo = motivo or ""
    if motivo.startswith("projecao sem linha"):
        return 1
    if motivo.startswith("pela trilha"):
        return 2
    if motivo.startswith("resposta automatica"):
        return 3
    if motivo.startswith("backfill E1 v5: admin_decidiu sem ato"):
        return 4
    return 5

universo = Counter()
classes = Counter()
inversoes = Counter()
maior_ato_anterior = None
total_dp = 0

linhas = (
    DP.objects.order_by("pk")
    .values_list("pk", "ato", "ato_em", "ato_motivo")
    .iterator(chunk_size=2000)
)
for pk, ato, instante, motivo in linhas:
    c = classe(motivo)
    universo[(atos.get(ato, 9), c)] += 1
    classes[c] += 1
    total_dp += 1
    if maior_ato_anterior is not None and instante < maior_ato_anterior:
        inversoes[c] += 1
    if maior_ato_anterior is None or instante > maior_ato_anterior:
        maior_ato_anterior = instante

for (ato, c), n in sorted(universo.items()):
    print((1, ato, c, n))

for c in range(1, 6):
    print((2, c, classes[c], inversoes[c]))

agora = timezone.now()
total_lavras = 0
marcos_diferentes = 0
stale_com = 0
stale_sem = 0
mudou_stale = 0

lavras = (
    PD.objects.exclude(veredito_lavrado="")
    .filter(veredito_lavrado__isnull=False, julgado_em__isnull=False)
    .select_related("disputa")
    .order_by("pk")
    .iterator(chunk_size=500)
)

for p in lavras:
    total_lavras += 1
    cid = p.disputa.colaborador_id
    corte = p.julgado_em

    reais = marcos_do_lote([cid], ate=corte)
    base = reais.get(cid)

    decisoes = DP.objects.filter(
        pergunta__disputa__colaborador_id=cid,
        ato_em__lte=corte,
    )
    com = decisoes.aggregate(m=Max("ato_em"))["m"]
    sem = (
        decisoes
        .exclude(ato_motivo__startswith="projecao sem linha")
        .exclude(ato_motivo__startswith="backfill E1 v5: admin_decidiu sem ato")
        .aggregate(m=Max("ato_em"))["m"]
    )

    def combinar(a, b):
        valores = [v for v in (a, b) if v is not None]
        return max(valores) if valores else None

    marco_com = combinar(base, com)
    marco_sem = combinar(base, sem)
    marcos_diferentes += int(marco_com != marco_sem)

    mapa_com = {} if marco_com is None else {cid: marco_com}
    mapa_sem = {} if marco_sem is None else {cid: marco_sem}
    s_com = esta_stale(p, mapa_com, agora)
    s_sem = esta_stale(p, mapa_sem, agora)

    stale_com += int(s_com)
    stale_sem += int(s_sem)
    mudou_stale += int(s_com != s_sem)

print((3, total_lavras, marcos_diferentes, stale_com, stale_sem, mudou_stale))
print((4, total_dp, total_lavras))
```

Pontos aproximados: as classes de motivo não identificam univocamente o escritor nem todas as possíveis linhas de backfill; exclusão das classes 1 e 4 é um recorte observável, não prova do conjunto completo de backfills. A ordem de pk é apenas proxy; a comparação dos marcos usa o estado atual das projeções e demais tabelas, não uma cópia histórica do banco. A consulta por lavra pode ser custosa na sombra. (`app/chamados/models.py:4119`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`, `app/chamados/veredito_lavrado.py:177`, `app/chamados/veredito_lavrado.py:202`)

**Não verificado**

- Identidade Git independente da cópia com `dd489c627`; os anexos físicos `logs/ESPINHA-BRIEFS/F3/` não estavam disponíveis, e as linhas dos anexos citadas são as do texto fornecido. **[nao verificado]**
- Dados e contagens atuais da sombra, execução do script, ordem efetiva de alocação/commit e existência de trilhas externas ou SQL fora da cópia. **[nao verificado]** [nao verificado]
- Definição operacional de B, snapshot de IDs na lavra e contagem exata de retroatividade por escritor: os dados fornecidos e os campos citados não bastam para calculá-los. **[nao verificado]**

## 18/11 10:28 · sessao c12f5ee63016 · E4C-F4 · verificador · origem=Code

**Veredito:** o delta corrige o filtro de carimbo cru no segundo laço e acrescenta à pauta as U60 não carimbadas que o contador classificou em L1/L5. A pauta ainda inclui toda U103 entregue ou sem dia, inclusive uma U103 já resolvida; isso não satisfaz literalmente “as 163 que a L1 barrar”. A comparação é uma análise do código por classe: os pks e estados efetivos de produção são **[não verificado]**. [app/chamados/management/commands/bug_par_103.py:54](app/chamados/management/commands/bug_par_103.py:54), [app/chamados/services/materializacao.py:328](app/chamados/services/materializacao.py:328), [app/chamados/tests/test_bug_par_volta.py:622](app/chamados/tests/test_bug_par_volta.py:622)

A cópia não contém `.git`; a revisão anterior foi reconstruída pelo diff fornecido, e as linhas da revisão atual vêm dos arquivos abertos. A identidade efetiva da cópia com o hash informado é **[não verificado]**.

### 1. Escritores, leitores e significado

O delta altera a seleção e a montagem da pauta; não altera os escritores persistentes dos campos de pergunta. Abaixo, “escreve” identifica os produtores relevantes abertos, sem afirmar que sejam todos os escritores existentes.

| Dado ou função | Quem escreve ou produz | Quem lê no caminho examinado |
|---|---|---|
| `U60`, `U103` | O JSON contém listas de pks; `_carregar_universo_bug_par` valida hash, chaves, cardinalidade, inteiros, ausência de repetição e interseção, e cria `frozenset`s. [app/chamados/data/bug_par_163_2026_11_13.json:1](app/chamados/data/bug_par_163_2026_11_13.json:1), [app/chamados/services/universo_bug_par.py:12](app/chamados/services/universo_bug_par.py:12) | O comando seleciona `U103`; o contador seleciona `U163`. [app/chamados/management/commands/bug_par_103.py:50](app/chamados/management/commands/bug_par_103.py:50), [app/chamados/services/materializacao.py:323](app/chamados/services/materializacao.py:323) |
| `U163` | O carregador produz a união `U60 | U103`; não a lê como terceira chave do JSON. [app/chamados/services/universo_bug_par.py:25](app/chamados/services/universo_bug_par.py:25) | O comando exclui seus pks do segundo laço; o contador os inclui. Ambos usam pks, não objetos de pergunta. [app/chamados/management/commands/bug_par_103.py:73](app/chamados/management/commands/bug_par_103.py:73), [app/chamados/services/materializacao.py:324](app/chamados/services/materializacao.py:324) |
| `bug_par_103_pauta_dp` | O contador produz lista de tuplas `(pk, 'MM/AAAA')`, tanto para U103 elegível carimbada entregue quanto para não carimbada com testemunha L1. [app/chamados/services/materializacao.py:329](app/chamados/services/materializacao.py:329), [app/chamados/services/materializacao.py:341](app/chamados/services/materializacao.py:341) | `_pauta_das_163` converte a lista em dict e usa o valor como rótulo. O formato coincide com `_competencia_falante`, mas o **significado da inclusão** não coincide sempre com o primeiro laço U103, que não consulta o estado da testemunha. [app/chamados/management/commands/bug_par_103.py:58](app/chamados/management/commands/bug_par_103.py:58), [app/chamados/management/commands/bug_par_103.py:143](app/chamados/management/commands/bug_par_103.py:143) |
| `bug_par_103_sem_dia` | O contador produz lista de pks, não pares nem objetos. [app/chamados/services/materializacao.py:333](app/chamados/services/materializacao.py:333), [app/chamados/services/materializacao.py:346](app/chamados/services/materializacao.py:346) | `_pauta_das_163` associa a cada pk a string literal `sem dia do fato`; se um pk aparecesse nas duas chaves, `update` daria precedência a esse rótulo. A partição do contador percorre cada pergunta uma vez e usa `continue`. [app/chamados/management/commands/bug_par_103.py:140](app/chamados/management/commands/bug_par_103.py:140), [app/chamados/services/materializacao.py:328](app/chamados/services/materializacao.py:328) |
| `disputa__colaborador_id` | A pergunta tem FK `disputa`; o campo acessado é o id do colaborador da disputa. A identidade da pergunta também tem `colaborador`, cuja igualdade com o da disputa é descrita no modelo; a prova desse selo não foi aberta **[não verificado]**. [app/chamados/models.py:2529](app/chamados/models.py:2529), [app/chamados/models.py:2540](app/chamados/models.py:2540) | Os laços usam `p.disputa.colaborador_id`; o auxiliar obtém o mesmo id por `values_list`, não o objeto, e ambos formatam `col%s`. [app/chamados/management/commands/bug_par_103.py:64](app/chamados/management/commands/bug_par_103.py:64), [app/chamados/management/commands/bug_par_103.py:149](app/chamados/management/commands/bug_par_103.py:149) |
| `data_evento`, `motivo`, `tipo_resposta`, `respondida_em`, `resposta_colab` | O modelo declara data anulável, motivo e tipo textuais, texto e instante de resposta; os locais exatos de todos os escritores desses campos são **[não verificado]**. [app/chamados/models.py:2534](app/chamados/models.py:2534), [app/chamados/models.py:2555](app/chamados/models.py:2555) | O segundo laço usa a face SQL do juiz de resposta, motivos de intervalo e tipo `hora`; `competencia_do_fato` usa a data. Respondida requer instante **e** texto após trim. [app/chamados/management/commands/bug_par_103.py:73](app/chamados/management/commands/bug_par_103.py:73), [app/chamados/juizes.py:1544](app/chamados/juizes.py:1544), [app/chamados/services/materializacao.py:788](app/chamados/services/materializacao.py:788) |
| `materializada_em`, `materializada_por_id`, `via_resolucao`, `validada_em`, `validada_por_id`, `recusa_motivo` | `_escrever_carimbo` atribui os três campos do carimbo; as portas que o chamam persistem. `carimbar` também pode atribuir validação. Outros escritores de validação e recusa, exaustivamente: **[não verificado]**. [app/chamados/models.py:3216](app/chamados/models.py:3216), [app/chamados/models.py:3245](app/chamados/models.py:3245) | O contador distingue carimbadas e usa o predicado da devolução; o comando fotografa esses campos na reversão, sem usá-los para filtrar o segundo laço atual. [app/chamados/services/materializacao.py:328](app/chamados/services/materializacao.py:328), [app/chamados/management/commands/bug_par_103.py:65](app/chamados/management/commands/bug_par_103.py:65) |
| `competencia_do_fato`, `estado_aplicacao_pergunta`, contador | A primeira compõe janela e juiz K6 e retorna `None` ou `(mês, ano, bool)`; a testemunha retorna dict com `estado`/`detalhe` ou `None`; o contador produz os baldes. [app/chamados/services/materializacao.py:788](app/chamados/services/materializacao.py:788), [app/chamados/services/materializacao.py:865](app/chamados/services/materializacao.py:865), [app/chamados/services/materializacao.py:364](app/chamados/services/materializacao.py:364) | Comando e contador leem `competencia_do_fato`; contador lê a testemunha; comando lê os baldes. K6 recebe **pk da empresa**, enquanto `competencia_do_fato` recebe o **objeto empresa**. [app/chamados/services/materializacao.py:798](app/chamados/services/materializacao.py:798), [app/chamados/management/commands/bug_par_103.py:81](app/chamados/management/commands/bug_par_103.py:81) |
| `pauta`, `ja`, `rot`, `col` e `_pauta_das_163` | Os laços e o auxiliar produzem tuplas `(pk, 'colNNN', rótulo)`; `ja` é conjunto de pks; `rot` mapeia pk a rótulo; `col` mapeia pk a id do colaborador. [app/chamados/management/commands/bug_par_103.py:57](app/chamados/management/commands/bug_par_103.py:57), [app/chamados/management/commands/bug_par_103.py:143](app/chamados/management/commands/bug_par_103.py:143) | O comando imprime `pauta` em JSON e volta a alterá-la após as portas em `--apply`. [app/chamados/management/commands/bug_par_103.py:90](app/chamados/management/commands/bug_par_103.py:90), [app/chamados/management/commands/bug_par_103.py:119](app/chamados/management/commands/bug_par_103.py:119) |

Os tipos e formatos dos dois produtores de rótulo são compatíveis; **a semântica da seleção não é única**: o primeiro laço pergunta K6 diretamente para toda U103, enquanto o auxiliar aceita a partição do contador, que distingue resolvidas, respondidas e estado de aplicação. [app/chamados/management/commands/bug_par_103.py:58](app/chamados/management/commands/bug_par_103.py:58), [app/chamados/services/materializacao.py:328](app/chamados/services/materializacao.py:328)

### 2. Comparação dos conjuntos de pks

Defina **E** como competência própria entregue e **D** como ausência de `data_evento`. A coluna “antes” usa o segundo laço removido no diff fornecido: excluía U103 e excluía U163 carimbada. O primeiro laço U103 permaneceu. [app/chamados/management/commands/bug_par_103.py:54](app/chamados/management/commands/bug_par_103.py:54), [app/chamados/management/commands/bug_par_103.py:73](app/chamados/management/commands/bug_par_103.py:73)

| Classe | `35039881d` | `5d6dd907f` e avaliação |
|---|---|---|
| **U103 carimbada**, E ou D | Lista pelo primeiro laço, qualquer que seja a via ou a resposta. | Igual. Para a candidata à devolução barrada, é coerente com a pauta específica dessa devolução; para uma já resolvida, é inclusão excedente frente à regra das bloqueadas. [app/chamados/management/commands/bug_par_103.py:58](app/chamados/management/commands/bug_par_103.py:58), [app/chamados/services/materializacao.py:329](app/chamados/services/materializacao.py:329) |
| **U103 não carimbada**, E ou D | Lista pelo primeiro laço, mesmo sem resposta. | Igual. Se respondida e de tipo hora de intervalo, a testemunha alcança L1/L5; se não respondida, a testemunha retorna `None`, mas o primeiro laço ainda lista. [app/chamados/management/commands/bug_par_103.py:58](app/chamados/management/commands/bug_par_103.py:58), [app/chamados/services/materializacao.py:882](app/chamados/services/materializacao.py:882) |
| **U60 carimbada**, E ou D | Não lista: o segundo laço a excluía pelo carimbo. | Não lista: contador a classifica como resolvida, pois o predicado da devolução exige U103. Para uma já gravada, as duas versões seguem a interpretação do teste de que ela não é barrada. [app/chamados/services/materializacao.py:250](app/chamados/services/materializacao.py:250), [app/chamados/services/materializacao.py:329](app/chamados/services/materializacao.py:329), [app/chamados/tests/test_bug_par_volta.py:622](app/chamados/tests/test_bug_par_volta.py:622) |
| **U60 não carimbada**, respondida, hora de intervalo, E ou D | Lista pelo segundo laço. | Lista pelo balde L1/L5 do contador. Mesmo conjunto nessas condições, agora sem filtro próprio de carimbo no comando. [app/chamados/services/materializacao.py:341](app/chamados/services/materializacao.py:341), [app/chamados/management/commands/bug_par_103.py:86](app/chamados/management/commands/bug_par_103.py:86) |
| **Não respondida**, não carimbada | U103 entra em E/D pelo primeiro laço; U60 e fora163 não entram no segundo. | Igual para esses casos: a testemunha não classifica a U60 não respondida como L1/L5. A inclusão incondicional da U103 é o excesso indicado acima. [app/chamados/management/commands/bug_par_103.py:58](app/chamados/management/commands/bug_par_103.py:58), [app/chamados/services/materializacao.py:882](app/chamados/services/materializacao.py:882) |
| **Respondida e validada sem carimbo**, hora de intervalo | U103 entra em E/D; U60 entra pelo segundo laço em E/D. | Também entra: a testemunha consulta a guarda antes da derivação, independentemente de `validada_em`. A gravação é barrada em L1/L5. [app/chamados/services/materializacao.py:892](app/chamados/services/materializacao.py:892), [app/chamados/tests/test_bug_par_volta.py:578](app/chamados/tests/test_bug_par_volta.py:578) |
| **“Irmã” com competência diferente** | Cada linha do comando usa sua própria data. | Igual. A irmã que a porta reconhece exige mesma disputa **e mesma data**; portanto uma pergunta de outra competência não é essa irmã. A guarda normal dispensa a busca com base nessa identidade. [app/chamados/services/materializacao.py:139](app/chamados/services/materializacao.py:139), [app/chamados/services/validacao.py:219](app/chamados/services/validacao.py:219) |
| **Fora163**, respondida, hora de intervalo, E ou D | Lista, com ou sem carimbo. | Igual: o novo laço exclui U163 e consulta K6 da própria pergunta; D recebe rótulo explícito. [app/chamados/management/commands/bug_par_103.py:73](app/chamados/management/commands/bug_par_103.py:73), [app/chamados/management/commands/bug_par_103.py:154](app/chamados/management/commands/bug_par_103.py:154) |

Para uma competência própria aberta e com data, nenhum dos laços diretos a lista; o auxiliar só a acrescenta se o contador a classificou L1/L5. A resposta sobre quais pks concretos caem em cada classe, e se há casos reais de excesso ou omissão, é **[não verificado]**. [app/chamados/management/commands/bug_par_103.py:58](app/chamados/management/commands/bug_par_103.py:58), [app/chamados/services/materializacao.py:341](app/chamados/services/materializacao.py:341)

Na **primeira lista atual**, os conjuntos dos dois laços são disjuntos por U163, e o auxiliar exclui pks já presentes. Logo não há duplicata por construção sob leitura estável. O auxiliar, porém, faz nova consulta por pk e só retorna os encontrados: se uma pergunta desaparecer entre o contador e essa consulta, pode sumir sem rótulo; ocorrência real é **[não verificado]**. [app/chamados/management/commands/bug_par_103.py:73](app/chamados/management/commands/bug_par_103.py:73), [app/chamados/management/commands/bug_par_103.py:143](app/chamados/management/commands/bug_par_103.py:143)

### 3. Recorte e `--apply`

Com `--empresa`, o queryset do comando filtra perguntas pela empresa da disputa; o contador recebe disputas filtradas pela mesma empresa e restringe suas perguntas a U163. Sem `--empresa`, o comando considera todas as perguntas e o contador todas as perguntas U163. Assim, **o recorte de empresa coincide para U163**, mas os universos completos não são iguais: a lista do comando também contempla fora163, enquanto o contador só conta U163. [app/chamados/management/commands/bug_par_103.py:45](app/chamados/management/commands/bug_par_103.py:45), [app/chamados/management/commands/bug_par_103.py:73](app/chamados/management/commands/bug_par_103.py:73), [app/chamados/services/materializacao.py:323](app/chamados/services/materializacao.py:323)

Em `--apply`, a segunda lista **não é necessariamente coerente por pk com a primeira**. Após uma recusa da porta, o comando relê K6 e acrescenta a tupla se a **tupla inteira** não estiver na pauta. Se o mesmo pk já estiver com outro rótulo e a leitura posterior produzir um rótulo diferente, pode aparecer duas vezes; a lista também preserva rótulos anteriores, sem reclassificação geral. Mudanças concorrentes e duplicatas efetivas em produção: **[não verificado]**. [app/chamados/management/commands/bug_par_103.py:110](app/chamados/management/commands/bug_par_103.py:110), [app/chamados/management/commands/bug_par_103.py:119](app/chamados/management/commands/bug_par_103.py:119)

### 4. Contratos e mutante

O predicado `materializada_em__isnull` removido do segundo laço deixa de ser um ofensor AST naquele ponto. O comando continua fazendo uma classificação própria de **inclusão na pauta U103** por `exp or k6 is None`; consulta o juiz de competência via composição, mas não consulta a testemunha de aplicação para decidir se a U103 foi barrada. O contador ainda possui um ramo próprio por carimbo; esse ramo e a testemunha estão na allowlist do contrato de vitalidade. Isso não demonstra, por si só, que julguem *vitalidade*, mas impede afirmar que todos os leitores delegam qualquer decisão envolvendo carimbo. [app/chamados/management/commands/bug_par_103.py:58](app/chamados/management/commands/bug_par_103.py:58), [app/chamados/services/materializacao.py:328](app/chamados/services/materializacao.py:328), [app/chamados/tests/test_contract_leitores_vitalidade.py:107](app/chamados/tests/test_contract_leitores_vitalidade.py:107)

O contrato aberto procura formas AST específicas de comparação com `None`, `__isnull` e teste direto dos dois campos, com allowlist por função; passar nele não prova ausência de todo predicado equivalente ou de decisão própria de competência. Resultado da suíte nesta cópia: **[não verificado]**. [app/chamados/tests/test_contract_leitores_vitalidade.py:313](app/chamados/tests/test_contract_leitores_vitalidade.py:313)

O alvo textual novo do mutante aparece uma vez na fonte aberta. Por raciocínio, ele **morde o teste alvo**: a fixture põe as duas perguntas somente em U60/U163; com a chamada ao auxiliar substituída por `pauta.extend([])`, o primeiro laço U103 e o segundo laço fora163 ficam vazios, mas o teste exige os dois pks. O executor também exige ocorrência textual única e classifica o mutante como integração; execução e resultado real: **[não verificado]**. [app/chamados/tests/bug_par_mutantes.py:116](app/chamados/tests/bug_par_mutantes.py:116), [app/chamados/tests/test_bug_par_volta.py:501](app/chamados/tests/test_bug_par_volta.py:501), [app/chamados/tests/test_bug_par_volta.py:611](app/chamados/tests/test_bug_par_volta.py:611), [app/chamados/tests/bug_par_mutantes.py:213](app/chamados/tests/bug_par_mutantes.py:213)

Há uma lacuna no teste de “já gravada não entra”: ele usa a mesma fixture U60 e, por isso, não exercita a U103 já resolvida que o primeiro laço ainda lista. Além disso, o teste alvo verifica L5 no contador após remover a data, mas não chama novamente o comando para conferir a pauta L5. [app/chamados/tests/test_bug_par_volta.py:501](app/chamados/tests/test_bug_par_volta.py:501), [app/chamados/tests/test_bug_par_volta.py:616](app/chamados/tests/test_bug_par_volta.py:616), [app/chamados/tests/test_bug_par_volta.py:622](app/chamados/tests/test_bug_par_volta.py:622)

## 18/11 10:27 · sessao eeb00102cb29 · RESPOSTAS-DE-LEI-1811-1010 · sonda · origem=Code

```text
---- DADO RASPADO (sonda/anexos) ----
=== SOMBRA (sombra_db, leitor) 18/11 10:25:11 ===
sonda_leitura: sonda_i8.py [4cdf8da477f4472e] no cpuset 4-7, lendo sombra como leitor (LEITURA)

=== col198 dia 09/11
 celula vespera 08/11: trabalha=False origem=gerada regen=0 marcos=None
 celula dia     09/11: trabalha=True origem=gerada regen=0 marcos={'hf': '07:00', 'hi': '19:00', 'hfi': None, 'hii': None}
 batidas apuraveis D-2..D+1: [('07/11 18:57E', 'app'), ('08/11 06:57S', 'app'), ('08/11 18:57E', 'app'), ('09/11 06:58S', 'app')]
  turno data_turno=2026-11-08 E=08/11 18:57E S=09/11 06:58S aberto=False cross=True nbat=2
=== col366 dia 09/11
 celula vespera 08/11: trabalha=False origem=gerada regen=0 marcos=None
 celula dia     09/11: trabalha=True origem=gerada regen=0 marcos={'hf': '07:00', 'hi': '19:00', 'hfi': '03:00', 'hii': '02:00'}
 batidas apuraveis D-2..D+1: [('08/11 19:00E', 'app'), ('09/11 02:00S', 'app'), ('09/11 03:00E', 'app'), ('09/11 07:00S', 'app')]
  turno data_turno=2026-11-08 E=08/11 19:00E S=09/11 07:00S aberto=False cross=True nbat=4
=== col366 dia 05/11
 celula vespera 04/11: trabalha=False origem=gerada regen=0 marcos=None
 celula dia     05/11: trabalha=True origem=gerada regen=0 marcos={'hf': '07:00', 'hi': '19:00', 'hfi': '03:00', 'hii': '02:00'}
 batidas apuraveis D-2..D+1: [('03/11 02:00S', 'app'), ('03/11 03:00E', 'app'), ('03/11 07:00S', 'app'), ('04/11 19:00E', 'app'), ('05/11 02:00S', 'app'), ('05/11 03:00E', 'app'), ('05/11 07:00S', 'app')]
  turno data_turno=2026-11-04 E=04/11 19:00E S=05/11 07:00S aberto=False cross=True nbat=4
=== col366 dia 03/11
 celula vespera 02/11: trabalha=False origem=gerada regen=0 marcos=None
 celula dia     03/11: trabalha=True origem=gerada regen=0 marcos={'hf': '07:00', 'hi': '19:00', 'hfi': '03:00', 'hii': '02:00'}
 batidas apuraveis D-2..D+1: [('02/11 19:00E', 'app'), ('03/11 02:00S', 'app'), ('03/11 03:00E', 'app'), ('03/11 07:00S', 'app'), ('04/11 19:00E', 'app')]
  turno data_turno=2026-11-02 E=02/11 19:00E S=03/11 07:00S aberto=False cross=True nbat=4
  turno data_turno=2026-11-04 E=04/11 19:00E S=05/11 07:00S aberto=False cross=True nbat=4
=== col366 dia 30/10
 celula vespera 29/10: trabalha=False origem=gerada regen=0 marcos=None
 celula dia     30/10: trabalha=True origem=gerada regen=0 marcos={'hf': '07:00', 'hi': '19:00', 'hfi': '03:00', 'hii': '02:00'}
 batidas apuraveis D-2..D+1: [('28/10 02:00S', 'app'), ('28/10 03:00E', 'app'), ('28/10 07:00S', 'app'), ('29/10 19:00E', 'app'), ('30/10 02:00S', 'app'), ('30/10 03:00E', 'app'), ('30/10 07:00S', 'app')]
  turno data_turno=2026-10-29 E=29/10 19:00E S=30/10 07:00S aberto=False cross=True nbat=4
=== col366 dia 28/10
 celula vespera 27/10: trabalha=False origem=gerada regen=0 marcos=None
 celula dia     28/10: trabalha=True origem=gerada regen=0 marcos={'hf': '07:00', 'hi': '19:00', 'hfi': '03:00', 'hii': '02:00'}
 batidas apuraveis D-2..D+1: [('27/10 19:00E', 'app'), ('28/10 02:00S', 'app'), ('28/10 03:00E', 'app'), ('28/10 07:00S', 'app'), ('29/10 19:00E', 'app')]
  turno data_turno=2026-10-27 E=27/10 19:00E S=28/10 07:00S aberto=False cross=True nbat=4
  turno data_turno=2026-10-29 E=29/10 19:00E S=30/10 07:00S aberto=False cross=True nbat=4
=== col489 dia 10/11
 celula vespera 09/11: trabalha=False origem=gerada regen=2 marcos=None
 celula dia     10/11: trabalha=True origem=gerada regen=2 marcos={'hf': '00:00', 'hi': '16:00', 'hfi': '21:00', 'hii': '20:00'}
 batidas apuraveis D-2..D+1: [('08/11 14:59E', 'app'), ('08/11 19:00S', 'app'), ('08/11 20:00E', 'app'), ('08/11 23:00S', 'app'), ('09/11 16:01E', 'app'), ('09/11 20:00S', 'app'), ('09/11 21:00E', 'app'), ('10/11 00:00S', 'app'), ('11/11 19:00E', 'app'), ('11/11 23:00S', 'app')]
  turno data_turno=2026-11-09 E=09/11 16:01E S=10/11 00:00S aberto=False cross=True nbat=4
  turno data_turno=2026-11-11 E=11/11 19:00E S=12/11 07:00S aberto=False cross=True nbat=4
=== col566 dia 17/11
 celula vespera 16/11: trabalha=False origem=gerada regen=0 marcos=None
 celula dia     17/11: trabalha=True origem=gerada regen=0 marcos={'hf': '06:00', 'hi': '18:00', 'hfi': None, 'hii': None}
 batidas apuraveis D-2..D+1: [('15/11 06:13S', 'app'), ('16/11 18:09E', 'app'), ('17/11 06:32S', 'app')]
  turno data_turno=2026-11-16 E=16/11 18:09E S=17/11 06:32S aberto=False cross=True nbat=2
=== col868 dia 17/11
 celula vespera 16/11: trabalha=False origem=gerada regen=0 marcos=None
 celula dia     17/11: trabalha=True origem=gerada regen=0 marcos={'hf': '07:00', 'hi': '19:00', 'hfi': '02:00', 'hii': '01:00'}
 batidas apuraveis D-2..D+1: [('15/11 07:03S', 'app'), ('16/11 19:16E', 'app'), ('17/11 03:39S', 'app'), ('17/11 04:35E', 'app'), ('17/11 06:49S', 'app')]
  turno data_turno=2026-11-16 E=16/11 19:16E S=17/11 06:49S aberto=False cross=True nbat=4
=== col1017 dia 17/11
 celula vespera 16/11: trabalha=False origem=gerada regen=0 marcos=None
 celula dia     17/11: trabalha=True origem=gerada regen=0 marcos={'hf': '06:00', 'hi': '18:00', 'hfi': None, 'hii': None}
 batidas apuraveis D-2..D+1: [('15/11 05:58S', 'app'), ('16/11 17:59E', 'app'), ('17/11 05:57S', 'app')]
  turno data_turno=2026-11-16 E=16/11 17:59E S=17/11 05:57S aberto=False cross=True nbat=2
=== col1031 dia 17/11
 celula vespera 16/11: trabalha=False origem=gerada regen=0 marcos=None
 celula dia     17/11: trabalha=True origem=gerada regen=0 marcos={'hf': '07:00', 'hi': '19:00', 'hfi': '00:00', 'hii': '23:00'}
 batidas apuraveis D-2..D+1: [('15/11 00:01E', 'app'), ('15/11 06:55S', 'app'), ('16/11 18:56E', 'app'), ('17/11 06:55S', 'app')]
  turno data_turno=2026-11-16 E=16/11 18:56E S=17/11 06:55S aberto=False cross=True nbat=2
=== col1031 dia 15/11
 celula vespera 14/11: trabalha=False origem=gerada regen=0 marcos=None
 celula dia     15/11: trabalha=True origem=gerada regen=0 marcos={'hf': '07:00', 'hi': '19:00', 'hfi': '00:00', 'hii': '23:00'}
 batidas apuraveis D-2..D+1: [('13/11 06:52S', 'app'), ('13/11 19:02E', 'app'), ('13/11 23:00S', 'app'), ('14/11 00:03E', 'app'), ('14/11 18:54E', 'app'), ('14/11 23:00S', 'app'), ('15/11 00:01E', 'app'), ('15/11 06:55S', 'app'), ('16/11 18:56E', 'app')]
  turno data_turno=2026-11-14 E=14/11 18:54E S=15/11 06:55S aberto=False cross=True nbat=4
  turno data_turno=2026-11-16 E=16/11 18:56E S=17/11 06:55S aberto=False cross=True nbat=2
=== col1031 dia 11/11
 celula vespera 10/11: trabalha=False origem=gerada regen=0 marcos=None
 celula dia     11/11: trabalha=True origem=gerada regen=0 marcos={'hf': '07:00', 'hi': '19:00', 'hfi': '00:00', 'hii': '23:00'}
 batidas apuraveis D-2..D+1: [('10/11 18:55E', 'app'), ('10/11 23:01S', 'app'), ('11/11 00:07E', 'app'), ('11/11 06:52S', 'app'), ('12/11 18:56E', 'app')]
  turno data_turno=2026-11-10 E=10/11 18:55E S=11/11 06:52S aberto=False cross=True nbat=4
  turno data_turno=2026-11-12 E=12/11 18:56E S=13/11 06:52S aberto=False cross=True nbat=2
rc_sombra=0
---- FIM DO DADO ----
```

linhas copiadas: 71.

Na tabela, as células e os turnos são transcritos do **DADO RASPADO** acima [não verificado em arquivo]. `T` e `F` representam `trabalha=True` e `False`; os marcos estão na ordem `hi/hii/hfi/hf`. As datas dos turnos são de 2026. O parecer “não” compara a célula que contém o marco `hf` com a `data_turno` impressa: não afirma que o cadastro esteja correto nem que um consumidor tenha julgado a saída de determinada forma. O código associa as batidas contidas em um turno à sua `data_turno` em [app/ponto/turnos.py:2957](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/turnos.py:2957).

A sonda imprime as pontas `E` e `S` e `nbat`, mas não imprime a lista `turno.batidas`. Portanto, para batidas intermediárias, a pertença individual está marcada **[não verificado]**: falta essa lista por turno. A saída impressa como `S` está identificada diretamente.

| ID | Dia | Célula da véspera | Célula do dia | Turnos montados: data, E, S, aberto | Batidas da madrugada do dia → data_turno | Concordam sobre a saída? |
|---|---|---|---|---|---|---|
| col198 | 09/11 | F; None | T; 19:00/None/None/07:00 | 08/11, 08/11 18:57, 09/11 06:58, False | 06:58S → 08/11 | Não: `hf` na célula 09/11; `data_turno=08/11`. |
| col366 | 09/11 | F; None | T; 19:00/02:00/03:00/07:00 | 08/11, 08/11 19:00, 09/11 07:00, False | 02:00S e 03:00E → [não verificado]; 07:00S → 08/11 | Não: `hf` na célula 09/11; `data_turno=08/11`. |
| col366 | 05/11 | F; None | T; 19:00/02:00/03:00/07:00 | 04/11, 04/11 19:00, 05/11 07:00, False | 02:00S e 03:00E → [não verificado]; 07:00S → 04/11 | Não: `hf` na célula 05/11; `data_turno=04/11`. |
| col366 | 03/11 | F; None | T; 19:00/02:00/03:00/07:00 | 02/11, 02/11 19:00, 03/11 07:00, False; **04/11**, 04/11 19:00, 05/11 07:00, False | 02:00S e 03:00E → [não verificado]; 07:00S → 02/11 | Não: `hf` na célula 03/11; `data_turno=02/11`. |
| col366 | 30/10 | F; None | T; 19:00/02:00/03:00/07:00 | 29/10, 29/10 19:00, 30/10 07:00, False | 02:00S e 03:00E → [não verificado]; 07:00S → 29/10 | Não: `hf` na célula 30/10; `data_turno=29/10`. |
| col366 | 28/10 | F; None | T; 19:00/02:00/03:00/07:00 | 27/10, 27/10 19:00, 28/10 07:00, False; **29/10**, 29/10 19:00, 30/10 07:00, False | 02:00S e 03:00E → [não verificado]; 07:00S → 27/10 | Não: `hf` na célula 28/10; `data_turno=27/10`. |
| col489 | 10/11 | F; None | T; 16:00/20:00/21:00/00:00 | 09/11, 09/11 16:01, 10/11 00:00, False; **11/11**, 11/11 19:00, 12/11 07:00, False | 00:00S → 09/11 | Não: `hf` na célula 10/11; `data_turno=09/11`. |
| col566 | 17/11 | F; None | T; 18:00/None/None/06:00 | 16/11, 16/11 18:09, 17/11 06:32, False | 06:32S → 16/11 | Não: `hf` na célula 17/11; `data_turno=16/11`. |
| col868 | 17/11 | F; None | T; 19:00/01:00/02:00/07:00 | 16/11, 16/11 19:16, 17/11 06:49, False | 03:39S e 04:35E → [não verificado]; 06:49S → 16/11 | Não: `hf` na célula 17/11; `data_turno=16/11`. |
| col1017 | 17/11 | F; None | T; 18:00/None/None/06:00 | 16/11, 16/11 17:59, 17/11 05:57, False | 05:57S → 16/11 | Não: `hf` na célula 17/11; `data_turno=16/11`. |
| col1031 | 17/11 | F; None | T; 19:00/23:00/00:00/07:00 | 16/11, 16/11 18:56, 17/11 06:55, False | 06:55S → 16/11 | Não: `hf` na célula 17/11; `data_turno=16/11`. |
| col1031 | 15/11 | F; None | T; 19:00/23:00/00:00/07:00 | 14/11, 14/11 18:54, 15/11 06:55, False; **16/11**, 16/11 18:56, 17/11 06:55, False | 00:01E → [não verificado]; 06:55S → 14/11 | Não: `hf` na célula 15/11; `data_turno=14/11`. |
| col1031 | 11/11 | F; None | T; 19:00/23:00/00:00/07:00 | 10/11, 10/11 18:55, 11/11 06:52, False; **12/11**, 12/11 18:56, 13/11 06:52, False | 00:07E → [não verificado]; 06:52S → 10/11 | Não: `hf` na célula 11/11; `data_turno=10/11`. |

O mesmo limite vale para atribuir uma **obra causal** a cada um dos 13 dias: [não verificado]. O dado mostra a disposição das células e o resultado do pareador; faltam as leituras ou decisões dos demais componentes, e, quando pertinente, o histórico de lavratura e rejulgamento. As linhas abaixo distinguem o que cada obra registrada cobre do que a sonda permite concluir.

| Obra registrada | Trecho do backlog que cobre seu mecanismo | Aplicação aos 13 dias |
|---|---|---|
| MOTOR-LE-O-TURNO | “motor e DiaPago leem par e dia SO de core/juizes.py::turnos_do_colab.” [app/docs/BACKLOG.md:2043](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/BACKLOG.md:2043) | [não verificado] em todos: faltam o par e o dia efetivamente usados pelo motor e pelo DiaPago. |
| DIA-DA-BATIDA-JUIZ-UNICO | “JUIZO PARALELO (migra para data_turno do juiz do par)” e “RED turno 19-07 + DIFF da 10 por migracao”. [app/docs/BACKLOG.md:1846](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/BACKLOG.md:1846) | [não verificado] em todos: falta identificar um juízo consumidor que use o dia civil da batida e sua decisão. A divergência de campos, por si só, não demonstra esse juízo. |
| DIA-SEM-TRABALHO-PORTA | “batida em folga do calendario e em feriado sem trabalha_em_feriado -> porta de decisao do admin”. [app/docs/BACKLOG.md:1968](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/BACKLOG.md:1968) | A véspera aparece com `trabalha=False` em todos [não verificado em arquivo]. A aplicação da porta é [não verificado]: faltam a classificação completa do dia e a decisão ou passagem pela porta. |
| CELULA-CONCORDE-NO-MEIO-DO-TURNO | “celula concorde no meio do turno e rejulgada no fim”. [app/docs/BACKLOG.md:1821](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/BACKLOG.md:1821) | [não verificado] em todos: faltam estado da célula, horários de lavratura e eventual rejulgamento. |

Nos totais seguintes, “mecanismo” significa **padrão observado de célula e turno**, sem atribuição causal de obra [não verificado em arquivo]:

| Padrão observado | Dias | Contagem |
|---|---|---:|
| Véspera F/None; dia T sem marcos de intervalo; turno da véspera fechado com 2 batidas | col198 09/11; col566 17/11; col1017 17/11 | 3 |
| Véspera F/None; dia T com marcos de intervalo; turno da véspera fechado com 4 batidas | col366 09/11, 05/11, 03/11, 30/10 e 28/10; col489 10/11; col868 17/11; col1031 15/11 e 11/11 | 9 |
| Véspera F/None; dia T com marcos de intervalo; turno da véspera fechado com 2 batidas | col1031 17/11 | 1 |
| **Total** | | **13** |

Por obra causal comprovada: **0 dias atribuíveis com o dado fornecido; 13 com atribuição [não verificado]**. Isso não é contagem de dias que as obras necessariamente corrigiriam: faltam as evidências indicadas na tabela por obra. [app/docs/BACKLOG.md:1846](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/BACKLOG.md:1846), [app/docs/BACKLOG.md:1968](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/BACKLOG.md:1968), [app/docs/BACKLOG.md:2043](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/BACKLOG.md:2043), [app/docs/BACKLOG.md:1821](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/BACKLOG.md:1821).

## 18/11 10:26 · sessao d9f6a5ad5bea · DST-L2 · sonda · origem=Code

```text
furo_vivos 3002
com_dia 3002 com_celula 2998 trabalha_false(ja_fechava_no_base) 11
dia_sem_trabalho_so_pela_ata_gravada(DST-L2_alarga) 35
  destes_com_justif_pendente_chamado_origem 0 com_ponteiro_pendente 0
```

## 18/11 10:25 · sessao cd955b9d3c25 · RESPOSTAS-DE-LEI-1811-1010 · verificador · origem=Code

**Resultado: item 1 devolvido; item 3 não devolvido pelos achados verificáveis.** No item 1, falta informação na saída do casador para executar literalmente o recorte pedido por órfã e comprovar o RED do dia vizinho. No item 3, a solução é possível na porta existente, mas ainda exige alteração do universo e da escrita. [app/escala/utils.py:621](app/escala/utils.py:621) · [app/chamados/models.py:1546](app/chamados/models.py:1546) · [app/chamados/models.py:1613](app/chamados/models.py:1613)

Os anexos raspados foram fornecidos sem numeração própria de linhas; afirmações que dependem exclusivamente deles estão marcadas **[não verificado]**. A identidade Git do HEAD não pôde ser conferida: esta cópia não apresentou metadados Git. **[não verificado]**

| Item | Pergunta | Veredito e prova |
|---|---|---|
| **1** | **1. Executável no literal e na ordem?** | **PROCEDE.** `_match_marcos` devolve células e órfãs como listas separadas; cada órfã é apenas `HH:MM`, sem data civil, identidade da batida ou associação ao marco `missing`. A grade reúne as listas por dia, mas também acrescenta sobras pela data civil e pode acrescentar órfãs por falta de coluna. Isso permite testar se **o dia** tem algum `missing`, mas não prova, por órfã, a relação com um marco nem distingue todos os casos necessários ao RED “dia vizinho não entra” sem informação adicional. [app/escala/utils.py:615](app/escala/utils.py:615) · [app/escala/utils.py:662](app/escala/utils.py:662) · [app/escala/utils.py:1715](app/escala/utils.py:1715) · [app/escala/utils.py:1807](app/escala/utils.py:1807) · [app/escala/utils.py:1813](app/escala/utils.py:1813) |
| **1** | **2. Falta “senão”?** | **PROCEDE.** Para a situação identificada acima — a saída não distinguir a órfã necessária ao recorte — o complemento não determina o que o vigia deve fazer; contá-la ou descartá-la escolheria uma regra que o dado não fornece. **[não verificado]** A classificação CADASTRO, por sua vez, é expressável pelo juiz existente e pode alimentar o RED de não alarmar. [app/ponto/management/commands/e6_oraculo.py:98](app/ponto/management/commands/e6_oraculo.py:98) |
| **1** | **3. Colisão com lei, aval ou obra?** | **NÃO PROCEDE**, quanto às colisões demonstráveis nos materiais disponíveis: abrir por dono e reservar o alarme a ESTRUTURA acompanha a L-099; o complemento posterior declara substituir o anterior onde colidir. [app/docs/LEIS.md:122](app/docs/LEIS.md:122) · **[não verificado: precedência entre anexos raspados]** Compatibilidade com toda obra em voo fora do material fornecido: **[não verificado]**. |
| **1** | **4. Referências, unidade e universo corretos?** | **PROCEDE.** `utils.py:631` monta tipos das batidas candidatas, e `:673` é documentação; as emissões efetivas de horários estão em `:621` e `:662`. A unidade do complemento é **órfã/batida**, enquanto os 106 fornecidos são **dias com órfã**: não se pode comparar as contagens como se fossem a mesma unidade. [app/escala/utils.py:631](app/escala/utils.py:631) · [app/escala/utils.py:673](app/escala/utils.py:673) · [app/escala/utils.py:621](app/escala/utils.py:621) · **[não verificado: números e unidades dos anexos raspados]** A justificativa sobre `perto_do_marco` está correta: a distância usa minuto do dia e é circular, descartando a data. [app/escala/regua_defesa.py:412](app/escala/regua_defesa.py:412) · [app/escala/regua_defesa.py:442](app/escala/regua_defesa.py:442) |
| **3** | **1. Executável no literal e na ordem?** | **NÃO PROCEDE**, como impossibilidade: é possível alterar a porta existente para incluir o estado hoje excluído e registrar a foto marcada antes do lote zerar o par, sob a transação e a trava existentes. Isso **não está implementado hoje**: o filtro pula texto sem instante e sem validação, e `linhas_da_projecao_sem_linha` não o fotografa. A restrição do modelo permite a marca apenas em `REABERTURA`. [app/chamados/models.py:1541](app/chamados/models.py:1541) · [app/chamados/models.py:1613](app/chamados/models.py:1613) · [app/chamados/models.py:1636](app/chamados/models.py:1636) · [app/chamados/models.py:4094](app/chamados/models.py:4094) · [app/chamados/models.py:4132](app/chamados/models.py:4132) |
| **3** | **2. Falta “senão”?** | **NÃO PROCEDE**, para a ordem de gravação: a porta retorna sem escrever quando não há IDs e o registro e a limpeza estão no mesmo `atomic`; falha da operação não autoriza concluir que o par foi limpo com sucesso. [app/chamados/models.py:1599](app/chamados/models.py:1599) · [app/chamados/models.py:1613](app/chamados/models.py:1613) · [app/chamados/models.py:1637](app/chamados/models.py:1637) Tratamento operacional específico de uma falha futura: **[não verificado]**. |
| **3** | **3. Colisão com lei, aval ou obra?** | **NÃO PROCEDE**, nas fontes disponíveis: a L-130 exige limpar instante e texto e conservar o texto na trilha; a marca já foi prevista no expand, e a carga já a ignora antes de escolher o último ato. [app/docs/LEIS.md:151](app/docs/LEIS.md:151) · [app/chamados/models.py:4065](app/chamados/models.py:4065) · [app/chamados/services/tarefa_do_dia.py:421](app/chamados/services/tarefa_do_dia.py:421) O estado e a coordenação efetiva da obra P2 em outra cópia são **[não verificado]**. |
| **3** | **4. Referências, unidade e universo corretos?** | **NÃO PROCEDE**, com a ressalva de implementação: `models.py:1541`, `:1619`, `:4069` e `:4132` sustentam, respectivamente, a exclusão, a fotografia atual antes da limpeza, a marca prevista e a limitação da fotografia. A unidade é pergunta. A porta atual **ainda pode criar** linhas `RESPOSTA`/`VALIDACAO` de projeção; cumprir “sem RESPOSTA sintética” exige tratar essa chamada ao construir, e não presumir que ela já satisfaz o aval. [app/chamados/models.py:1541](app/chamados/models.py:1541) · [app/chamados/models.py:1619](app/chamados/models.py:1619) · [app/chamados/models.py:4069](app/chamados/models.py:4069) · [app/chamados/models.py:4139](app/chamados/models.py:4139) |

**Quem escreve e quem lê — item 1.** O casador escreve em memória as células `missing` e as listas de horários; o builder agrupa essas listas pelo dia de entrada e acrescenta sobras pelo dia civil. O classificador lê `orfas` e verifica se **alguma célula do dia** está `missing`; não recebe uma ligação órfã→marco. O juiz de dono lê insumos do dia e devolve ESTRUTURA, CADASTRO ou BATIDA. O vigia proposto seria leitor dessas saídas; seu código e seu placar nesta obra são **[não verificado]**. [app/escala/utils.py:615](app/escala/utils.py:615) · [app/escala/utils.py:1434](app/escala/utils.py:1434) · [app/escala/utils.py:1813](app/escala/utils.py:1813) · [app/escala/orfa_classe.py:35](app/escala/orfa_classe.py:35) · [app/ponto/management/commands/e6_oraculo.py:53](app/ponto/management/commands/e6_oraculo.py:53) · [app/docs/JUIZES.md:27](app/docs/JUIZES.md:27)

**Quem escreve e quem lê — item 3.**

- **`ato` e `ato_texto`:** `DecisaoPergunta.registrar` é a porta de escrita; entre os chamadores atuais, `reabrir_para_correcao` fornece a reabertura e o texto anterior, e a fotografia da projeção fornece respostas ou validações. O escritor proposto seria a ressuscitação dentro da porta existente. [app/chamados/models.py:4151](app/chamados/models.py:4151) · [app/chamados/services/disputa_emissao.py:1632](app/chamados/services/disputa_emissao.py:1632) · [app/chamados/models.py:4139](app/chamados/models.py:4139)
- **Leitores:** a tarefa lê `ato` e lê `ato_texto` para distinguir resposta vazia; o histórico seleciona atos com foto e o modal mostra `ato_texto`. Assim, a marca protege a seleção de tarefa, mas o histórico também verá a reabertura marcada como ato com foto. [app/chamados/services/tarefa_do_dia.py:158](app/chamados/services/tarefa_do_dia.py:158) · [app/chamados/models.py:3680](app/chamados/models.py:3680) · [app/templates/chamados/partials/modal_fio.html:210](app/templates/chamados/partials/modal_fio.html:210)
- **`da_ressuscitacao`:** o modelo declara o escritor P2 futuro e o predicado que lê a marca; a carga da tarefa usa esse predicado para excluir o ato **antes** da escolha do último. Uma reabertura comum passa e é lida como devolução ao colaborador. [app/chamados/models.py:4065](app/chamados/models.py:4065) · [app/chamados/models.py:4102](app/chamados/models.py:4102) · [app/chamados/services/tarefa_do_dia.py:421](app/chamados/services/tarefa_do_dia.py:421) · [app/chamados/services/tarefa_do_dia.py:163](app/chamados/services/tarefa_do_dia.py:163) O juiz de resposta exige instante e texto não vazio. [app/docs/JUIZES.md:102](app/docs/JUIZES.md:102)

**Item 1 — AVAL-DEVOLVIDO: sim**: perguntas 1, 2 e 4 procedem.

**Item 3 — AVAL-DEVOLVIDO: não**: nenhuma das quatro perguntas procedeu nos limites verificados; a implementação ainda precisa cumprir as ressalvas acima.

## 18/11 10:24 · sessao 8841839b2e3b · DST-L2 · sonda · origem=Code

Transcrição literal das linhas numéricas fornecidas [não verificado]:

```text
pendentes_90d 68
pendente_com_ponteiro_de_chamado_encerrado 38
pendente_com_chamado_origem_encerrado 38
chamados_encerrados_universo 38 sem_followup_sistema 3
justif_orfas_com_celula 38 dia_sem_trabalho_pelo_juiz 4
```

Não há traceback no trecho fornecido [não verificado].

**Limite da análise:** o conteúdo de `sonda_justif_orfa.py` não foi disponibilizado no texto e não o localizei na cópia; portanto, não é possível verificar os filtros, o universo, as exclusões ou a forma da consulta de cada contador [não verificado]. A cópia também não contém metadados Git acessíveis para confirmar o hash informado [não verificado].

Pelos **nomes impressos**, as linhas parecem tratar dos seguintes conjuntos; a definição exata de cada um continua [não verificada]:

- `pendentes_90d`: pendentes em uma janela de 90 dias. Qual data define a janela e seus limites: [não verificado].
- `pendente_com_ponteiro_de_chamado_encerrado` e `pendente_com_chamado_origem_encerrado`: pendentes associadas a chamado encerrado por dois critérios. O modelo possui tanto `Justificativa.chamado_origem` quanto `ChamadoColaborador.justificativa_relacionada`, que são relações distintas; não é possível confirmar qual consulta produz o primeiro contador nem que os dois grupos de 38 sejam os mesmos. ([ponto/models.py:173](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/models.py:173), [chamados/models.py:69](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:69)) [não verificado].
- `chamados_encerrados_universo` e `sem_followup_sistema`: chamados do universo da sonda e os classificados sem follow-up de sistema. Como esse universo é formado e o que a consulta considera ausência de follow-up: [não verificado]. O modelo permite follow-up com `autor_tipo='sistema'`. ([chamados/models.py:1733](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:1733))
- `justif_orfas_com_celula` e `dia_sem_trabalho_pelo_juiz`: justificativas classificadas pela sonda como órfãs e com célula, e casos classificados pelo juiz como dia sem trabalho. Como a sonda define “órfã”, encontra a célula e trata célula ausente: [não verificado]. O juiz retorna verdadeiro para `trabalha is False` ou para `ata.tipo_dia` presente e diferente de `trabalho`; numa ausência parcial, retorna apenas `trabalha is False`. ([chamados/juizes.py:432](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/juizes.py:432))

Quanto à possível distorção, `Justificativa`, `ChamadoColaborador` e `FollowupChamado` têm ordenações padrão, mas **não é possível dizer se elas inflam algum contador sem ver as consultas**. ([ponto/models.py:182](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/models.py:182), [chamados/models.py:103](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:103), [chamados/models.py:1765](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:1765)) O uso e o efeito de `Q` com subconsulta, de `distinct`, e de células ausentes também permanecem [não verificados]. O código admite chamado sem célula, e o próprio supra-juiz distingue `sem_celula` de um juízo de folga. ([chamados/models.py:48](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:48), [ponto/supra_juiz.py:51](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/supra_juiz.py:51))

Para concluir a análise dos seis contadores e de suas possíveis distorções, preciso do conteúdo de `sonda_justif_orfa.py` mencionado no envelope [não verificado].

## 18/11 10:24 · sessao 443a793a78f8 · DST-L2 · leitura · origem=Code

O fechamento antecipado acontece durante o `create()` da justificativa, **antes de `_justif` gravar o ponteiro**. A alteração DST-L2 faz o cartório considerar a ata de ausência ao decidir se o chamado protesta; o fechamento direto pelo cartório passa ao largo da unidade S128, que é onde estão o Fix A e o followup de retratação ([teste](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/tests/test_jz1_autoridade_unica.py:41), [cartório](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1457), [S128](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:563)). Os resultados do diagnóstico informados na pergunta não foram reproduzidos nesta leitura: **[não verificado]**.

### 1. Sequência e diferença para a base

1. `_justif` cria uma `Justificativa` pendente; somente depois atribui `ch.justificativa_relacionada` e salva o chamado ([teste](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/tests/test_jz1_autoridade_unica.py:41)).
2. O `post_save` da criação chama `_julgar_dias`, que chama `cartorio.julgar_celula(colaborador, data_turno)` ([signals de ponto](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/signals.py:161), [signals de ponto](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/signals.py:85)). `julgar_celula` chama `julgar_colab` para a célula encontrada ([cartório](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:2019)).
3. A ata do julgamento é construída antes de `_protesta`; no commit analisado, este pergunta a `dia_sem_trabalho` usando `trabalha` **e a ata atual** ([cartório](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1435), [cartório](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1462)). O juiz considera dia sem trabalho uma ata com `tipo_dia` preenchido diferente de `trabalho`, ressalvada a ausência parcial ([juiz](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/juizes.py:445)).
4. Para os chamados que não protestam, o cartório chama `transicionar_com_pauta(..., ato=x.reconciliar)` imediatamente em **`_porm`, linha 1546**, ou os adia quando há fio vivo e os fecha na **linha 1011**, depois da lavra e do E2 ([cartório](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1540), [cartório](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1000)). A fixture afirma que a célula absolve a pergunta, mas o dado fornecido não identifica qual das duas chamadas executou no diagnóstico; **linha efetivamente executada: [não verificado]** ([fixture](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/tests/_celula_absolve.py:2), [juiz do fio](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/juizes.py:1510)).
5. `reconciliar` passa pelo gate e chama `_gravar_estado`; é este último que atribui `self.status_local = novo` ([modelo](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:642), [modelo](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:228)). O `post_save` do status tenta resolver perguntas pelo receiver S143, em modo *best effort* ([signals de chamados](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/signals.py:91)).

Na base `dd489c627`, `_protesta` só exclui o dia quando `cel.trabalha is False` ou os marcos estão acesos; não consulta o tipo de dia da ata ([cartório da base](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1458)). No cenário indicado, a mudança relevante é que o dia previsto de trabalho coberto por ausência passa a não protestar pela segunda cláusula do juiz ([teste](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/tests/test_jz1_autoridade_unica.py:96), [cartório](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1457)). A execução exata dos gates na base é **[não verificado]**.

Depois do fechamento, o `save` do ponteiro não muda `status_local`; o receiver do cartório retorna quando o status não mudou, e o S143 também retorna quando o save não escreveu o status ([signals de chamados](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/signals.py:189), [signals de chamados](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/signals.py:91)). A grade coleta apenas chamados em `OPEN`, por isso um já resolvido deixa de ser candidato à retratação por essa coleta ([reconciliar_grade](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/management/commands/reconciliar_grade.py:199)).

### 2. Todos os sítios diretos do cartório

Todos os seis sítios abaixo chamam `reconciliar` pelo mesmo wrapper; se a transição efetivamente grava o status, o S143 **tenta** resolver perguntas, mas pode falhar sem desfazer o fechamento. Nenhum deles chama diretamente a unidade S128 ou seu Fix A ([modelo](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:642), [S143](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/signals.py:91), [S128](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:563)).

| Sítio e condição | Followup nesse ramo | Fix A | Resolver de perguntas |
|---|---|---|---|
| Adiados após lavra e E2 ([linha 1011](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1011)) | Não | Não | S143; E2 também é chamado antes ([linha 1006](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1006)) |
| `_porm`, não protestam e não adiados ([linha 1546](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1546)) | Não | Não | S143 |
| Marcos inexistentes, `_velhos` ([linha 1559](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1559)) | **Sim**, `marco_inexistente` ([linha 1563](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1563)) | Não | S143 |
| Cobrança em dia sem trabalho ou sem furo, `_rec` ([linha 1580](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1580)) | Não | Não | S143 |
| Fio composto de perguntas duplicatas, `x` ([linha 1659](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1659)) | Não | Não | S143 |
| Deduplicação dos outros chamados, `y` ([linha 1674](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1674)) | Não | Não | S143 |

“Não” na coluna de followup refere-se à chamada de `anexar_ocorrencia` no ramo do cartório; efeitos indiretos específicos de outras rotinas ou dados do chamado são **[não verificado]**.

### 3. Escritores, leitores e ordem do ponteiro

O escritor de produção do ponteiro é `vinculos.apontar_justificativa`: ele só escreve se o ponteiro estiver vazio e salva os campos do vínculo ([vínculos](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/services/vinculos.py:81)). Os três chamadores encontrados são:

- API: busca e salva uma pendente existente ou cria uma nova, **antes** de apontar ([API](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/api/views.py:1988)).
- View de ponto: também salva a existente ou cria a nova, **antes** de apontar ([view](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1621)).
- `processar_alertas_turno`: faz `get_or_create` e aponta quando `created` é verdadeiro, portanto depois da criação ([comando](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/management/commands/processar_alertas_turno.py:392)).

Em todos, o `save()` ou a criação da justificativa pode disparar o julgamento do dia **entre** a escrita da justificativa e a do ponteiro, sujeito às guardas do cartório ([signal](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/signals.py:161), [cartório](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:2019)).

Leitores de produção encontrados para o ponteiro são: a guarda de transição humana e o coletor de premissa no modelo ([modelo](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:135), [modelo](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:1313)); a retenção e o Fix A do reconciliador ([reconciliador](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:440), [reconciliador](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:573)); e os dois fluxos de batida que podem rejeitar a pendente ([triagem](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/triagem_batida.py:598), [entrada sincronizada](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/entrada_sincronizada.py:29)).

Por retratação, o Fix A escreve `rejeitada`, acrescenta a descrição e salva ([reconciliador](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:569)). Os dois fluxos de batida também escrevem esse status após uma transição bem-sucedida ([triagem](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/triagem_batida.py:600), [entrada sincronizada](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/entrada_sincronizada.py:32)). Há rejeição humana individual e em lote ([serviço](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:269), [view](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1863)); a fila lê o status para filtrar rejeitadas ([view](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1707)) e a API o apresenta ([API](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/api/views.py:2106)).

### 4. Ponteiro escrito depois do fechamento

**Não encontrei um caminho automático de reparo desse estado no código examinado.** A porta do ponteiro só grava o vínculo; a S128 executa o Fix A apenas depois de uma transição aceita, enquanto `reconciliar` declara no-op para chamado já encerrado; os fluxos de batida também exigem fechamento bem-sucedido antes de rejeitar ([vínculos](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/services/vinculos.py:81), [S128](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:563), [modelo](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:642), [triagem](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/triagem_batida.py:598), [entrada sincronizada](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/entrada_sincronizada.py:29)). Uma rejeição posterior por decisão humana é possível ([serviço](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:269)). Existência de outro caminho não localizado ou execução em produção: **[não verificado]**.

### 5. Contratos e conflito

A S128 declara autoridade única de retratação: transição, Fix A, resolução de perguntas e ocorrência compõem o ato cercado; o teste JZ1 exige chamado encerrado, perguntas resolvidas, disputa fechada, followup de sistema e justificativa rejeitada ([reconciliador](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:522), [ato](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:563), [selo](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/tests/test_jz1_autoridade_unica.py:50)).

O cartório declara que orquestra o juiz e a lavra, e que a ata lavrada prevalece para seus leitores ([docstring](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:7)). A L-061 registra que o consumidor lê a lâmpada ([LEIS.md](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/docs/LEIS.md:95)). O O261 descreve a ordem **lavrar → E2 → reconciliar** para o lote adiado com fio, enquanto o código fecha imediatamente o sem fio ([cartório](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1528), [cartório](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1540)). O corte DST-L2 determina que o cartório pergunte a `dia_sem_trabalho` ([CORTES.md](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/docs/CORTES.md:533)).

**Há conflito na implementação observada, não uma impossibilidade lógica entre as ordens:** o cartório usa `reconciliar` diretamente para quem não protesta, inclusive após E2, e assim não executa os demais componentes obrigatórios da S128; além disso, quando a justificativa nasce depois do fechamento, o Fix A não tinha ponteiro para ler no instante do fechamento ([cartório](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1011), [cartório](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1546), [ato S128](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:563), [teste](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/tests/test_jz1_autoridade_unica.py:41)). O registro antigo que diz que `_protesta` ainda usa só a primeira cláusula está desatualizado perante o corte DST-L2 e o código desta cópia ([CORTES.md](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/docs/CORTES.md:448), [CORTES.md](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/docs/CORTES.md:533), [cartório](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1464)).

## 18/11 10:16 · sessao fec0b6249760 · RESPOSTAS-DE-LEI-1811-1010 · censo · origem=Code

**O censo ainda está ABERTO nos quatro itens.** A saída recebida permitiu confirmar escritores, leitores e caminhos que faltavam, mas não sustenta uma lista exaustiva nem a classificação individual de todos os acessos dinâmicos. **[nao verificado: completude do censo]**

Há duas correções relevantes para o construtor:

- A escrita de `validada_em` não se limita ao ato de validar: há limpeza pela resposta humana, devoluções e preenchimentos associados ao carimbo. A distinção é entre o dono do **ato de validar** e todos os escritores do campo. [app/chamados/services/validacao.py:1] [app/chamados/models.py:3249] [app/chamados/services/disputa_emissao.py:1494]
- A carga remove as decisões marcadas como ressuscitação antes de escolher o último ato, mas `respostas_anteriores` percorre atos com fotografia sem esse filtro. Um novo ato marcado precisa ter os dois leitores avaliados. [app/chamados/services/tarefa_do_dia.py:421] [app/chamados/models.py:3688]

As listas abaixo registram os sítios confirmados nesta volta; **não são a lista completa pedida**. **[nao verificado: sítios restantes]**

### 1. Escritores e leitores R/V/C e porta `registrar` — STATUS ABERTO

**Escrita da projeção:**

- Resposta humana e invalidação da validação anterior: [app/chamados/services/disputa_emissao.py:1481].
- Resposta automática e carimbo pela trilha: [app/chamados/services/disputa_emissao.py:1675] [app/chamados/services/disputa_emissao.py:1698].
- Validação e devoluções: [app/chamados/services/validacao.py:168] [app/chamados/services/validacao.py:273] [app/chamados/services/validacao.py:349].
- Carimbo com preenchimento condicional de resposta e validação: [app/chamados/models.py:3249].
- Escrita em lote dinâmica por `**outros`: [app/chamados/models.py:3371] [app/chamados/models.py:3404]. Os chamadores confirmados limpam R/V/C na ressuscitação, R/V/C na reabertura e o par de resposta na devolução L-130: [app/chamados/models.py:1637] [app/chamados/services/disputa_emissao.py:1610] [app/chamados/services/disputa_emissao.py:1649] [app/chamados/services/validacao.py:440].
- A entrada por foto delega à resposta automática e à validação; o cabeçalho antigo do módulo que a descreve como escrita local não corresponde ao corpo atual. [app/api/foto_recebida.py:2] [app/api/foto_recebida.py:39]

**Escrita da sequência pela porta `DecisaoPergunta.registrar`:**

- Modelo e portas de resposta/reabertura: [app/chamados/models.py:1619] [app/chamados/models.py:2013] [app/chamados/models.py:2145] [app/chamados/models.py:3137] [app/chamados/services/disputa_emissao.py:1525] [app/chamados/services/disputa_emissao.py:1632] [app/chamados/services/disputa_emissao.py:1679] [app/chamados/services/disputa_emissao.py:1701].
- Validação, devoluções e absorção: [app/chamados/services/validacao.py:173] [app/chamados/services/validacao.py:279] [app/chamados/services/validacao.py:354] [app/chamados/services/validacao.py:439] [app/chamados/services/materializacao.py:1048].
- Cartório e comandos de acervo: [app/ponto/services/veredito_celula.py:195] [app/chamados/management/commands/backfill_decisoes_pergunta.py:324] [app/chamados/management/commands/backfill_fio_morto_e4b.py:141].
- Falso positivo: `registrar` em [app/chamados/views.py:91] é da recusa crítica, não a porta de `DecisaoPergunta`. [app/chamados/views.py:90]

**Leituras confirmadas que o desenho precisa considerar:**

- Juízes, faces SQL e ordenação da fila: [app/chamados/juizes.py:352] [app/chamados/juizes.py:856] [app/chamados/juizes.py:1389] [app/chamados/juizes.py:1534] [app/chamados/juizes.py:1552] [app/chamados/juizes.py:1572] [app/chamados/juizes.py:1580] [app/chamados/juizes.py:1761].
- Estado e apresentação da pergunta; histórico de decisões: [app/chamados/models.py:3159] [app/chamados/models.py:3681] [app/chamados/models.py:3701].
- Fio e materialização: [app/chamados/services/fio.py:188] [app/chamados/services/fio.py:263] [app/chamados/services/materializacao.py:546] [app/chamados/services/materializacao.py:1153] [app/chamados/services/materializacao.py:2091].
- API e apresentação: [app/api/views.py:1163] [app/api/views.py:2561] [app/api/views.py:2639] [app/api/views_mensageria.py:567] [app/chamados/templatetags/chamados_tags.py:139] [app/chamados/templatetags/chamados_tags.py:166].
- Histórico cronológico lê dinamicamente os três instantes e o texto; a reversão do passivo também fotografa campos da pergunta: [app/chamados/services/historico.py:96] [app/chamados/services/passivo_validado.py:64] [app/chamados/services/passivo_validado.py:83].
- Auditoria, placares e procedência têm leituras próprias: [app/chamados/management/commands/auditar_invariantes_chamados.py:1077] [app/chamados/management/commands/auditar_invariantes_chamados.py:1107] [app/chamados/services/painel.py:261] [app/chamados/services/verbos.py:263] [app/ponto/services/procedencia_batida.py:71].

**Por que permanece aberto:** faltam a classificação de todos os leitores listados no inventário, o rastreamento completo dos chamadores e a resolução individual dos acessos genéricos que podem receber objetos ou nomes em execução. **[nao verificado]**

### 2. Importadores, selo e medida da sombra — STATUS ABERTO

Importadores confirmados: `relavrar_via_i6` importa `universo_i6`; `dedup_perguntas_fato` importa `aplicar` e `CONTADORES_DO_APPLY`; `worklist` importa `_parse_pendentes` e `MODULOS_ALVO`, e tem consumidores na view e no comando. [app/chamados/management/commands/relavrar_via_i6.py:46] [app/chamados/management/commands/dedup_perguntas_fato.py:105] [app/ponto/worklist.py:37] [app/colaboradores/views.py:3061] [app/ponto/management/commands/worklist_dp.py:12]

O selo global de vitalidade declarado examina `materializada_em` e `validada_em` e formas sintáticas específicas; ele não demonstra, sozinho, cobertura de todos os campos ou acessos dinâmicos. [app/chamados/tests/test_contract_leitores_vitalidade.py:14] [app/chamados/tests/test_contract_leitores_vitalidade.py:329] O selo específico lista os quatro escritores esperados de `resposta_mantem_validacao`. [app/chamados/tests/test_f2_resposta_mantem_validacao.py:19]

A medida raspada da **sombra** informa:

| Projeção atual nula com ato histórico correspondente | Último físico / efetivo | Total |
|---|---|---:|
| Resposta | reabertura / reabertura | 52 |
| Resposta | recusa / recusa | 7 |
| Validação | devolução / devolução | 60 |
| Validação | reabertura / reabertura | 9 |
| Validação | resposta / resposta | 3 |

Esses valores são os fornecidos no anexo da conversa; produção e interseção dos grupos são **[nao verificado]**. A função confirmada mede o sentido inverso, projeção sem linha correspondente por ato e instante; os números acima não demonstram erros nem uma correspondência geral ato → projeção. [app/chamados/models.py:4122] [app/chamados/services/disputa_emissao.py:1632]

A exclusividade dos importadores e a classificação completa dos selos permanecem **[nao verificado]**.

### 3. Campos novos e leituras de REABERTURA — STATUS ABERTO

- `da_ressuscitacao` e `resposta_mantem_validacao` têm defaults e restrições por tipo de ato. [app/chamados/models.py:4071] [app/chamados/models.py:4077] [app/chamados/models.py:4095] [app/chamados/models.py:4098]
- A marca de ressuscitação é lida pelo predicado em [app/chamados/models.py:4103] e pela carga em [app/chamados/services/tarefa_do_dia.py:421]. A porta de ressuscitação nesta cópia registra a projeção faltante e limpa os campos; não se pode presumir que já grave a nova marca. [app/chamados/models.py:1619] [app/chamados/models.py:1637]
- A reabertura fotografa resposta, instantes e autor antes de limpar a projeção; `respostas_anteriores` é leitor da sequência e a carga do fio busca também o autor original. [app/chamados/services/disputa_emissao.py:1632] [app/chamados/models.py:3688] [app/chamados/services/fio.py:263]
- Os valores declarados para `resposta_mantem_validacao` são `True` na fotografia da projeção, resposta automática e carimbo pela trilha; `False` na resposta humana. [app/chamados/models.py:4141] [app/chamados/services/disputa_emissao.py:1525] [app/chamados/services/disputa_emissao.py:1679] [app/chamados/services/disputa_emissao.py:1701]

**Contradição a resolver no desenho:** o comentário do modelo associa a regra atual ao `ato_motivo`, mas `_pela_decisao` aqui trata toda última `RESPOSTA` com texto como `aguarda_validacao`, sem consultar esse motivo ou `resposta_mantem_validacao`. [app/chamados/models.py:4072] [app/chamados/services/tarefa_do_dia.py:158] Não está demonstrado que trocar esse leitor pela nova marca preserve o comportamento atual. **[nao verificado]**

A lista completa de leitores indiretos e o efeito de todos os caminhos de `registrar` seguem **[nao verificado]**.

### 4. Cascas, templates e PDF — STATUS ABERTO

- `base_tpl` é escolhido pelo contexto entre `base_app.html` e `base.html`; `meu_atendimento.html` usa essa escolha e inclui o cartão nas duas abas. [app/core/context_processors.py:194] [app/core/context_processors.py:205] [app/templates/chamados/meu_atendimento.html:1] [app/templates/chamados/meu_atendimento.html:127] [app/templates/chamados/meu_atendimento.html:144]
- O cartão do colaborador lê resposta e instante. [app/templates/chamados/partials/_card_atendimento.html:106] [app/templates/chamados/partials/_card_atendimento.html:143] [app/templates/chamados/partials/_card_atendimento.html:183] [app/templates/chamados/partials/_card_atendimento.html:391]
- A casca administrativa `detalhe_local` estende `base.html` e inclui `_pergunta_card`, que também pode ser renderizado isoladamente por HTMX. [app/templates/chamados/detalhe_local.html:1] [app/templates/chamados/detalhe_local.html:310] [app/chamados/views.py:398]
- `modal_fio` lê histórico, resposta e validação e também é renderizado isoladamente; portanto o grafo que parte apenas das bases não o enumera como alcançado. [app/templates/chamados/partials/modal_fio.html:205] [app/templates/chamados/partials/modal_fio.html:219] [app/templates/chamados/partials/modal_fio.html:228] [app/chamados/views.py:927] [app/chamados/views.py:1399]
- A prévia de lote lê a resposta e é renderizada pela view de cobrança. [app/templates/chamados/partials/_lote_preview.html:24] [app/chamados/views_cobrar.py:360]
- No caminho de PDF inspecionado, a view alimenta o gerador de histórico e o bloco examinado lê followups. Relações indiretas adicionais entre PDF e R/V/C permanecem **[nao verificado]**. [app/relatorios/views.py:871] [app/relatorios/pdf_chamados.py:135]

### Classificação dos dinâmicos examinados

- **Escrita real de R/V/C:** `models.py:3404`, por `**outros`, com os três chamadores identificados no item 1. [app/chamados/models.py:3379] [app/chamados/models.py:3404]
- **Leitura real:** `historico.py:105`; `passivo_validado.py:83`; e `_PD` em `veredito_lavrado.py:202` e `:206` é alias de `PerguntaDisputa`, com filtros sobre materialização e resposta. [app/chamados/services/historico.py:96] [app/chamados/services/passivo_validado.py:64] [app/chamados/veredito_lavrado.py:163] [app/chamados/veredito_lavrado.py:202]
- **Não é acesso R/V/C nesses sítios:** os dinâmicos de `ChamadoColaborador` em `models.py:637`, `:953`, `:976`, `:1018` e `:1142`; os filtros de chave de família em `fio.py:2110`, `:2112`, `:2122`, `:2134`, `:2164`, `:2166` e `:2187`; e os filtros dinâmicos por identidade nas duas portas de validação. [app/chamados/models.py:637] [app/chamados/models.py:953] [app/chamados/models.py:976] [app/chamados/models.py:1018] [app/chamados/models.py:1142] [app/chamados/services/fio.py:1515] [app/chamados/services/fio.py:2110] [app/chamados/services/fio.py:2112] [app/chamados/services/fio.py:2122] [app/chamados/services/fio.py:2134] [app/chamados/services/fio.py:2164] [app/chamados/services/fio.py:2166] [app/chamados/services/fio.py:2187] [app/chamados/services/validacao.py:673] [app/chamados/services/validacao.py:890]
- **Ainda indeterminados:** os demais candidatos genéricos do inventário, inclusive o alcance completo dos wrappers de `carga_pergunta_viva` e dos resolvedores genéricos de `core`, não foram classificados um a um. [app/chamados/services/carga_pergunta_viva.py:30] [app/chamados/services/carga_pergunta_viva.py:59] [app/core/intake.py:487] [app/core/reexibir.py:49] **[nao verificado]**

Para terminar a classificação dos **selos** sem acesso a logs, seria necessária a parte da saída bruta filtrada referente a `tests/`; para o censo de produção ainda falta completar a leitura dos candidatos e caminhos indiretos acima. **[nao verificado]**

alcance nao verificado restante: 4

## 18/11 10:15 · sessao 6e3da0f0e6be · RESPOSTAS-DE-LEI-1811-1010 · censo · origem=Code

O censo **ainda não autoriza abrir o construtor**: os itens 1, 3 e 5 permanecem abertos. O inventário amplia os sítios conhecidos, mas não demonstra que todos os acessos dinâmicos, aliases indiretos, carregamentos de templates e leitores foram classificados. [não verificado]

Há uma contradição concreta com a formulação da volta 1 sobre Q4: o card de pergunta exibe “validada” diretamente quando `validada_em` existe, mas o “Validada” do modal está dentro do ramo `perg.respondida` e depois do ramo prioritário `pendencia_admin`. Assim, validação projetada sem linha de decisão, por si só, **não demonstra** que o modal mostrará “Validada”; isso também depende do par de resposta e da ausência de pendência prioritária. [app/templates/chamados/partials/_pergunta_card.html:23](app/templates/chamados/partials/_pergunta_card.html:23) [app/templates/chamados/partials/modal_fio.html:217](app/templates/chamados/partials/modal_fio.html:217) [app/templates/chamados/partials/modal_fio.html:222](app/templates/chamados/partials/modal_fio.html:222) [app/templates/chamados/partials/modal_fio.html:228](app/templates/chamados/partials/modal_fio.html:228)

### 1. Cascas do card — STATUS: ABERTO

Os caminhos de produção identificados para a F7 são:

- **Card do colaborador, leitura:** `meu_atendimento` inclui `_card_atendimento` nos dois loops; a casca pode ser `base.html` ou `base_app.html`, conforme `base_tpl`. O card lê o par de resposta e a fase. [app/templates/chamados/meu_atendimento.html:1](app/templates/chamados/meu_atendimento.html:1) [app/templates/chamados/meu_atendimento.html:127](app/templates/chamados/meu_atendimento.html:127) [app/templates/chamados/meu_atendimento.html:144](app/templates/chamados/meu_atendimento.html:144) [app/core/context_processors.py:210](app/core/context_processors.py:210) [app/templates/chamados/partials/_card_atendimento.html:106](app/templates/chamados/partials/_card_atendimento.html:106) [app/templates/chamados/partials/_card_atendimento.html:189](app/templates/chamados/partials/_card_atendimento.html:189)
- **Card da pergunta, leitura:** a página de detalhe usa `base.html` e inclui `_pergunta_card`; o rerender parcial também chega ao mesmo template por `_render_card`, alias de `render`. [app/templates/chamados/detalhe_local.html:1](app/templates/chamados/detalhe_local.html:1) [app/templates/chamados/detalhe_local.html:310](app/templates/chamados/detalhe_local.html:310) [app/chamados/views.py:397](app/chamados/views.py:397) [app/chamados/views.py:404](app/chamados/views.py:404)
- **Modal do fio, leitura:** é um fragmento renderizado diretamente, sem casca própria; uma hospedeira comprovada é o painel em `base.html`, que o busca pela rota. Há também chamada da fila de benefícios com `modo=beneficio`; a lista integral de hospedeiras e suas cascas permanece [não verificado]. [app/chamados/views.py:927](app/chamados/views.py:927) [app/chamados/views.py:1400](app/chamados/views.py:1400) [app/chamados/services/fio.py:543](app/chamados/services/fio.py:543) [app/templates/chamados/painel_gestao.html:1](app/templates/chamados/painel_gestao.html:1) [app/templates/chamados/painel_gestao.html:528](app/templates/chamados/painel_gestao.html:528) [app/templates/colaboradores/fila_solicitacoes_beneficio.html:119](app/templates/colaboradores/fila_solicitacoes_beneficio.html:119)
- **Prévia de lote, leitura:** é fragmento renderizado diretamente; o painel em `base.html` abre-o no drawer. Outras hospedeiras não foram excluídas. [app/templates/chamados/painel_gestao.html:14](app/templates/chamados/painel_gestao.html:14) [app/chamados/views_cobrar.py:360](app/chamados/views_cobrar.py:360) [app/chamados/views_cobrar.py:364](app/chamados/views_cobrar.py:364) [app/chamados/views_cobrar.py:380](app/chamados/views_cobrar.py:380) [app/chamados/views_cobrar.py:388](app/chamados/views_cobrar.py:388) [app/templates/chamados/partials/_lote_preview.html:24](app/templates/chamados/partials/_lote_preview.html:24)

O grafo fornecido identifica includes compartilhados pelas duas bases, mas não prova, sozinho, todas as hospedeiras dos fragmentos nem renderização e cliques em execução. Essa revisão transitiva continua aberta. [app/templates/base.html:93](app/templates/base.html:93) [app/templates/base_app.html:85](app/templates/base_app.html:85) [app/templates/base.html:399](app/templates/base.html:399) [app/templates/base_app.html:154](app/templates/base_app.html:154) [não verificado]

### 2. Escritor de reabertura — STATUS: FECHADO para a dúvida específica

`reabrir_para_correcao` prepara a limpeza do par e da validação, registra uma decisão com a fotografia anterior e chama `descarimbar_em_lote`; o corpo dessa função não chama `linhas_da_projecao_sem_linha`. Isso fecha a dúvida sobre **essa chamada**, sem fechar o censo de escritores. [app/chamados/services/disputa_emissao.py:1577](app/chamados/services/disputa_emissao.py:1577) [app/chamados/services/disputa_emissao.py:1610](app/chamados/services/disputa_emissao.py:1610) [app/chamados/services/disputa_emissao.py:1632](app/chamados/services/disputa_emissao.py:1632) [app/chamados/services/disputa_emissao.py:1649](app/chamados/services/disputa_emissao.py:1649)

### 3. Leitores cruzados e escritores — STATUS: ABERTO

A lista seguinte é de **sítios comprovados a considerar**, não uma lista integral nem uma determinação de que todos devam ser alterados pela F7. O brief completo e o efeito exato pretendido em cada consumidor não estão demonstrados no material fornecido. [não verificado]

- **Escrita persistida:** resposta e validação em `disputa_emissao`; validações em `validacao`; carimbo com possível preenchimento de resposta e validação em `models`; e limpeza em lote, inclusive campos passados por `**outros`. O último ponto é relevante: a porta verifica expressamente se `outros` contém campos do par e os passa ao `update`. [app/chamados/services/disputa_emissao.py:1481](app/chamados/services/disputa_emissao.py:1481) [app/chamados/services/disputa_emissao.py:1494](app/chamados/services/disputa_emissao.py:1494) [app/chamados/services/disputa_emissao.py:1675](app/chamados/services/disputa_emissao.py:1675) [app/chamados/services/disputa_emissao.py:1698](app/chamados/services/disputa_emissao.py:1698) [app/chamados/services/validacao.py:168](app/chamados/services/validacao.py:168) [app/chamados/services/validacao.py:273](app/chamados/services/validacao.py:273) [app/chamados/services/validacao.py:349](app/chamados/services/validacao.py:349) [app/chamados/models.py:3247](app/chamados/models.py:3247) [app/chamados/models.py:3251](app/chamados/models.py:3251) [app/chamados/models.py:3254](app/chamados/models.py:3254) [app/chamados/models.py:3371](app/chamados/models.py:3371) [app/chamados/models.py:3404](app/chamados/models.py:3404)
- **Atribuições que não são, isoladamente, escrita persistida:** as de `models.py:1561–1563` alteram uma cópia usada no julgamento; as de `models.py:3282–3284` restauram a instância em memória após recusa. Classificá-las simplesmente como escritoras de banco seria incorreto. [app/chamados/models.py:1548](app/chamados/models.py:1548) [app/chamados/models.py:1561](app/chamados/models.py:1561) [app/chamados/models.py:3278](app/chamados/models.py:3278)
- **Juízes e fase, leitura:** projeção temporal, juiz do par, propriedade `respondida` e `estado` são pontos centrais; `estado` dá precedência à validação projetada. [app/chamados/juizes.py:1382](app/chamados/juizes.py:1382) [app/chamados/juizes.py:1544](app/chamados/juizes.py:1544) [app/chamados/models.py:3152](app/chamados/models.py:3152) [app/chamados/models.py:3674](app/chamados/models.py:3674)
- **Interfaces e API, leitura:** além dos quatro templates do item 1, há apresentação de resposta por filtros e saídas da API e mensageria. [app/chamados/templatetags/chamados_tags.py:139](app/chamados/templatetags/chamados_tags.py:139) [app/chamados/templatetags/chamados_tags.py:166](app/chamados/templatetags/chamados_tags.py:166) [app/api/views.py:1163](app/api/views.py:1163) [app/api/views.py:2561](app/api/views.py:2561) [app/api/views_mensageria.py:567](app/api/views_mensageria.py:567)
- **Outros consumidores de produção, leitura:** materialização, reconciliação, histórico, painel, folha e cartório têm leituras diretas ou via juiz que precisam ser avaliadas conforme a mudança final. [app/chamados/services/materializacao.py:191](app/chamados/services/materializacao.py:191) [app/chamados/services/materializacao.py:2091](app/chamados/services/materializacao.py:2091) [app/chamados/reconciliador.py:365](app/chamados/reconciliador.py:365) [app/chamados/services/historico.py:98](app/chamados/services/historico.py:98) [app/chamados/services/painel.py:270](app/chamados/services/painel.py:270) [app/folha/previa.py:141](app/folha/previa.py:141) [app/ponto/services/cartorio.py:1641](app/ponto/services/cartorio.py:1641)
- **Auditoria e reconstrução, leitura e eventual escrita de decisões:** os comandos consultam as projeções, e o backfill chama a construção de linhas ausentes. Seu comportamento completo e a necessidade de mudança pela F7 permanecem [não verificado]. [app/chamados/management/commands/auditar_invariantes_chamados.py:1077](app/chamados/management/commands/auditar_invariantes_chamados.py:1077) [app/chamados/management/commands/backfill_decisoes_pergunta.py:56](app/chamados/management/commands/backfill_decisoes_pergunta.py:56) [app/chamados/management/commands/backfill_decisoes_pergunta.py:67](app/chamados/management/commands/backfill_decisoes_pergunta.py:67)

**Indeterminados examinados:**

- `models.py:637–638`: acesso dinâmico aos campos de urgência de `ChamadoColaborador`, não uma escrita da projeção de `PerguntaDisputa`. [app/chamados/models.py:620](app/chamados/models.py:620) [app/chamados/models.py:637](app/chamados/models.py:637)
- `models.py:953`, `:976`, `:1018`, `:1142`: filtros e criação da porta de `ChamadoColaborador`; não são, nesses sítios, escritas de `PerguntaDisputa`. [app/chamados/models.py:953](app/chamados/models.py:953) [app/chamados/models.py:976](app/chamados/models.py:976) [app/chamados/models.py:1018](app/chamados/models.py:1018) [app/chamados/models.py:1142](app/chamados/models.py:1142)
- `models.py:3404`: **escrita relevante**, inclusive dos campos dinâmicos `outros`, além dos carimbos explícitos. [app/chamados/models.py:3371](app/chamados/models.py:3371) [app/chamados/models.py:3404](app/chamados/models.py:3404)
- `historico.py:105`: **leitura relevante**; o laço percorre `respondida_em`, `validada_em` e `materializada_em`, e também lê o texto. [app/chamados/services/historico.py:98](app/chamados/services/historico.py:98) [app/chamados/services/historico.py:105](app/chamados/services/historico.py:105) [app/chamados/services/historico.py:109](app/chamados/services/historico.py:109)
- `passivo_validado.py:83`: **leitura relevante** para fotografia de reversão; a lista inclui materialização, resposta, validação e autor da validação. [app/chamados/services/passivo_validado.py:64](app/chamados/services/passivo_validado.py:64) [app/chamados/services/passivo_validado.py:83](app/chamados/services/passivo_validado.py:83)
- `validacao.py:677` e `:894`: filtros dinâmicos por `pk` e, opcionalmente, disputa; selecionam a pergunta, mas essas chaves não são campos da projeção. Os corpos seguintes ainda são consumidores relevantes. [app/chamados/services/validacao.py:673](app/chamados/services/validacao.py:673) [app/chamados/services/validacao.py:677](app/chamados/services/validacao.py:677) [app/chamados/services/validacao.py:890](app/chamados/services/validacao.py:890) [app/chamados/services/validacao.py:894](app/chamados/services/validacao.py:894)
- `veredito_lavrado.py:202` e `:206`: **leituras relevantes** dos instantes de materialização e resposta, com limite temporal dinâmico; `:177` e `:180` consultam `Batida`. [app/chamados/veredito_lavrado.py:163](app/chamados/veredito_lavrado.py:163) [app/chamados/veredito_lavrado.py:177](app/chamados/veredito_lavrado.py:177) [app/chamados/veredito_lavrado.py:180](app/chamados/veredito_lavrado.py:180) [app/chamados/veredito_lavrado.py:202](app/chamados/veredito_lavrado.py:202) [app/chamados/veredito_lavrado.py:206](app/chamados/veredito_lavrado.py:206)
- `fio.py:2112`, `:2122`, `:2134`, `:2164`, `:2166`, `:2187`: consultas dinâmicas às chaves de família e aos campos de exclusão do índice; os nomes são definidos nas tabelas do próprio arquivo. A classificação de seu impacto indireto completo continua [não verificado]. [app/chamados/services/fio.py:1515](app/chamados/services/fio.py:1515) [app/chamados/services/fio.py:1521](app/chamados/services/fio.py:1521) [app/chamados/services/fio.py:2109](app/chamados/services/fio.py:2109) [app/chamados/services/fio.py:2164](app/chamados/services/fio.py:2164)
- `explicador.py:43`: `dict.update` em resultado de apresentação, não `QuerySet.update` nem escrita de modelo. [app/chamados/services/explicador.py:41](app/chamados/services/explicador.py:41)
- `pdf_espelho.py:856`: o `getattr` apontado lê campos de rubricas dos turnos; não é, nesse ponto, leitura direta da projeção da pergunta. Outros caminhos indiretos até PDF permanecem [não verificado]. [app/relatorios/pdf_espelho.py:844](app/relatorios/pdf_espelho.py:844) [app/relatorios/pdf_espelho.py:855](app/relatorios/pdf_espelho.py:855)
- `core/reexibir.py:45` e `:49`: atribuições genéricas em memória; os chamadores encontrados são em `colaboradores/views.py`. Não demonstram, por si, escrita persistida da pergunta. [app/core/reexibir.py:21](app/core/reexibir.py:21) [app/core/reexibir.py:45](app/core/reexibir.py:45) [app/core/reexibir.py:49](app/core/reexibir.py:49) [app/colaboradores/views.py:1116](app/colaboradores/views.py:1116) [app/colaboradores/views.py:1281](app/colaboradores/views.py:1281)

**Não concluído:** não classifiquei individualmente todos os demais indeterminados e aliases da saída, nem demonstrei a lista integral de consumidores indiretos, SQL bruto ou caminhos de PDF. O item 3, portanto, não está fechado. [não verificado]

### 4. Contrato F1 antigo — STATUS: FECHADO quanto à implicação para a F7

A F7 deve censar os leitores efetivos desta cópia; não há demonstração de equivalência geral entre eles e o fold histórico descrito no anexo. Aqui, `respondida` delega ao juiz do par projetado, enquanto a projeção temporal e `estado` consideram outros campos. A ancestralidade e os bytes do objeto histórico informados no anexo permanecem [não verificado]. [app/chamados/models.py:3674](app/chamados/models.py:3674) [app/chamados/juizes.py:1544](app/chamados/juizes.py:1544) [app/chamados/juizes.py:1382](app/chamados/juizes.py:1382) [app/chamados/models.py:3152](app/chamados/models.py:3152)

### 5. Frota U1/U2 — STATUS: ABERTO

O anexo declara resultado de sombra com **U1 global = 29.931** e **15.118 validações projetadas sem linha**, com `rc_sombra=0`; não há acesso independente ao banco ou log nesta sessão. [não verificado]

O segundo contador usa correspondência de projeção por pergunta, ato e instante; ele não mede o último ato exigido para U2. Portanto, a sonda relatada fecha o resultado declarado para U1 e para o contador auxiliar, mas **U2 continua não medida**. [app/chamados/models.py:4122](app/chamados/models.py:4122) [app/chamados/models.py:4135](app/chamados/models.py:4135) [não verificado]

alcance nao verificado restante: 3

## 18/11 10:15 · sessao a2b0196b566b · - · - · origem=Ronald (tmux)

Evidências e limitações enviadas ao agente principal, incluindo os escritores de decisão, os leitores de REABERTURA, os importadores, os limites dos selos e a divergência entre o comentário sobre `ato_motivo` e o comportamento de `_pela_decisao` ([models.py:4075](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4075), [tarefa_do_dia.py:158](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:158)).

## 18/11 10:15 · sessao 7bb0d685aa36 · - · - · origem=Ronald (tmux)

Classificação dos candidatos dinâmicos relevantes enviada ao agente principal, com os sítios de leitura e escrita de R/V/C, os falsos positivos identificados e as lacunas que permanecem [nao verificado].

## 18/11 10:14 · sessao e888750e899c · - · - · origem=Ronald (tmux)

Rastreamento de templates, cascas dinâmicas e PDF enviado ao agente principal, com os respectivos arquivos, linhas e limites de verificação.

## 18/11 10:13 · sessao b997298c28f7 · RESPOSTAS-DE-LEI-1811-1010 · sonda · origem=Code

```text
=== SOMBRA (sombra_db, leitor) 18/11 10:12:49 ===
sonda_leitura: cen_F9_s1.py [3b68d969da5dc8d8] no cpuset 4-7, lendo sombra como leitor (LEITURA)

R_projecao_nula_com_ato_resposta	ultimo_fisico=reabertura	ultimo_efetivo=reabertura	total=52
R_projecao_nula_com_ato_resposta	ultimo_fisico=recusa	ultimo_efetivo=recusa	total=7
V_projecao_nula_com_ato_validacao	ultimo_fisico=devolucao	ultimo_efetivo=devolucao	total=60
V_projecao_nula_com_ato_validacao	ultimo_fisico=reabertura	ultimo_efetivo=reabertura	total=9
V_projecao_nula_com_ato_validacao	ultimo_fisico=resposta	ultimo_efetivo=resposta	total=3
rc_sombra=0
```
linhas copiadas: 9

_6900 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
