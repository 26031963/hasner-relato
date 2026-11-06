# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 18:26** · publicado em **06/11/2026 18:31** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 18:26 · sessao eeb4262ba705 · DIA-SEM-TRABALHO-NA-FRENTE · sonda · origem=Code

**1) Transcrição literal**

Fonte deste bloco: o dado raspado da mensagem, sem arquivo numerado disponível para conferência [nao verificado]. Preservei os exemplos existentes; os grupos menores não trazem três exemplos. Não há linha `CONCLUSAO` no texto recebido: há `INCOMPLETO: 100 erros` [nao verificado].

```text
U_D_BASE sem_porta classe d n=217 omitidos=0
U_D n=173 (CAUSA-D 11:2x: 165+5=170) fora={'BATIDA': 3, 'CADASTRO': 14, 'ESTRUTURA_mas_dia_de_trabalho': 27}
U_A n=170 (chat 17:4x PROD: 175) | INTERSECAO mesmo colab+dia=0 | so_D=173 so_A=170 | UNIAO=343
LINHAS 343 == UNIAO 343 True
QUADRO (por_que, batidas, porta, status pede decisao?, segura folha?) -- soma=343
GRUPO|n=156|folga_do_calendario ; batERRO ; sem_porta ; nao_pede(folga) ; segura|dono={'ESTRUTURA': 156}|em={'D': 156}
   ex col144 2026-09-21 clausula=trabalha_False fer=False/True/False aus=[] n_bat=ERRO orfas=0 porta=None status=folga resolver=False comp=10/2026
   ex col144 2026-09-23 clausula=trabalha_False fer=False/True/False aus=[] n_bat=ERRO orfas=0 porta=None status=folga resolver=False comp=10/2026
   ex col144 2026-09-25 clausula=trabalha_False fer=False/True/False aus=[] n_bat=ERRO orfas=0 porta=None status=folga resolver=False comp=10/2026
GRUPO|n=126|folga_do_calendario ; batERRO ; sem_porta ; nao_pede(folga_trabalhada) ; nao_segura|dono={'BATIDA': 27, 'CADASTRO': 71, 'ESTRUTURA': 28}|em={'A': 126}
   ex col49 2026-11-04 clausula=trabalha_False fer=False/True/False aus=[] n_bat=ERRO orfas=1 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col60 2026-11-03 clausula=trabalha_False fer=False/True/False aus=[] n_bat=ERRO orfas=3 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col60 2026-11-05 clausula=trabalha_False fer=False/True/False aus=[] n_bat=ERRO orfas=3 porta=None status=folga_trabalhada resolver=False comp=11/2026
GRUPO|n=30|feriado_sem_trabalha_em_feriado ; batERRO ; sem_porta ; nao_pede(folga_trabalhada) ; nao_segura|dono={'CADASTRO': 22, 'ESTRUTURA': 7, 'BATIDA': 1}|em={'A': 30}
   ex col43 2026-11-02 clausula=trabalha_False fer=True/False/True aus=[] n_bat=ERRO orfas=2 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col203 2026-11-02 clausula=trabalha_False fer=True/False/True aus=[] n_bat=ERRO orfas=4 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col207 2026-11-02 clausula=trabalha_False fer=True/False/True aus=[] n_bat=ERRO orfas=2 porta=None status=folga_trabalhada resolver=False comp=11/2026
GRUPO|n=8|feriado_trab_feriado_True_mas_folga ; batERRO ; sem_porta ; nao_pede(folga) ; segura|dono={'ESTRUTURA': 8}|em={'D': 8}
   ex col144 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=ERRO orfas=0 porta=None status=folga resolver=False comp=11/2026
   ex col209 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=ERRO orfas=0 porta=None status=folga resolver=False comp=11/2026
   ex col268 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=ERRO orfas=0 porta=None status=folga resolver=False comp=11/2026
GRUPO|n=5|ausencia_lancada ; batERRO ; sem_porta ; pede ; segura|dono={'ESTRUTURA': 5}|em={'D': 5}
   ex col137 2026-11-03 clausula=ata_tipo_dia=ausencia fer=False/True/False aus=[(4851, 'reciclagem', 'por_outro', 'sem_chamado')] n_bat=ERRO orfas=0 porta=None status=irregular resolver=False comp=11/2026
   ex col137 2026-11-05 clausula=ata_tipo_dia=ausencia fer=False/True/False aus=[(4851, 'reciclagem', 'por_outro', 'sem_chamado')] n_bat=ERRO orfas=0 porta=None status=irregular resolver=False comp=11/2026
   ex col292 2026-10-22 clausula=ata_tipo_dia=ausencia fer=False/True/False aus=[(5110, 'atestado', 'por_outro', 'sem_chamado')] n_bat=ERRO orfas=0 porta=None status=irregular resolver=False comp=11/2026
GRUPO|n=5|feriado_trab_feriado_True_mas_folga ; batERRO ; sem_porta ; nao_pede(folga_trabalhada) ; nao_segura|dono={'ESTRUTURA': 1, 'BATIDA': 1, 'CADASTRO': 3}|em={'A': 5}
   ex col366 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=ERRO orfas=1 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col788 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=ERRO orfas=8 porta=None status=folga_trabalhada resolver=False comp=11/2026
   ex col868 2026-11-02 clausula=trabalha_False fer=True/True/False aus=[] n_bat=ERRO orfas=1 porta=None status=folga_trabalhada resolver=False comp=11/2026
GRUPO|n=3|ata_tipo_dia_folga_sem_ausencia_cobrindo ; batERRO ; sem_porta ; nao_pede(escala_suspeita) ; segura|dono={'CADASTRO': 3}|em={'A': 3}
   ex col97 2026-11-03 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=ERRO orfas=1 porta=None status=escala_suspeita resolver=False comp=11/2026
   ex col97 2026-11-05 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=ERRO orfas=1 porta=None status=escala_suspeita resolver=False comp=11/2026
   ex col741 2026-11-05 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=ERRO orfas=2 porta=None status=escala_suspeita resolver=False comp=11/2026
GRUPO|n=2|folga_do_calendario ; batERRO ; sem_porta ; pede ; segura|dono={'ESTRUTURA': 2}|em={'D': 2}
   ex col348 2026-10-24 clausula=trabalha_False fer=False/True/False aus=[] n_bat=ERRO orfas=0 porta=None status=irregular resolver=False comp=11/2026
   ex col348 2026-10-31 clausula=trabalha_False fer=False/True/False aus=[] n_bat=ERRO orfas=0 porta=None status=irregular resolver=False comp=11/2026
GRUPO|n=2|ata_tipo_dia_folga_sem_ausencia_cobrindo ; batERRO ; porta ; nao_pede(escala_suspeita) ; segura|dono={'CADASTRO': 2}|em={'A': 2}
   ex col741 2026-11-03 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=ERRO orfas=1 porta=porta_pergunta status=escala_suspeita resolver=False comp=11/2026
   ex col866 2026-11-04 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=ERRO orfas=2 porta=porta_pergunta status=escala_suspeita resolver=False comp=11/2026
GRUPO|n=1|ata_tipo_dia_folga_sem_ausencia_cobrindo ; batERRO ; sem_porta ; pede ; segura|dono={'CADASTRO': 1}|em={'A': 1}
   ex col741 2026-11-01 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=ERRO orfas=1 porta=None status=aberto resolver=False comp=11/2026
GRUPO|n=1|ata_tipo_dia_folga_sem_ausencia_cobrindo ; batERRO ; sem_porta ; pede ; nao_segura|dono={'CADASTRO': 1}|em={'A': 1}
   ex col866 2026-10-21 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=ERRO orfas=1 porta=None status=aberto resolver=False comp=11/2026
GRUPO|n=1|ata_tipo_dia_folga_sem_ausencia_cobrindo ; batERRO ; porta ; pede ; segura|dono={'CADASTRO': 1}|em={'A': 1}
   ex col866 2026-11-02 clausula=ata_tipo_dia=folga fer=False/None/None aus=[] n_bat=ERRO orfas=1 porta=porta_pergunta status=aberto resolver=False comp=11/2026
GRUPO|n=1|ausencia_lancada_pelo_colab ; batERRO ; sem_porta ; pede ; segura|dono={'ESTRUTURA': 1}|em={'D': 1}
   ex col889 2026-10-25 clausula=ata_tipo_dia=ausencia fer=False/True/False aus=[(4670, 'atestado', 'pelo_colab', 'sem_chamado')] n_bat=ERRO orfas=0 porta=None status=irregular resolver=False comp=11/2026
GRUPO|n=1|feriado_sem_trabalha_em_feriado ; batERRO ; sem_porta ; nao_pede(folga) ; segura|dono={'ESTRUTURA': 1}|em={'D': 1}
   ex col969 2026-10-12 clausula=trabalha_False fer=True/False/True aus=[] n_bat=ERRO orfas=None porta=None status=folga resolver=False comp=10/2026
GRUPO|n=1|folga_do_calendario ; batERRO ; sem_porta ; nao_pede(ok) ; nao_segura|dono={'BATIDA': 1}|em={'A': 1}
   ex col975 2026-11-01 clausula=trabalha_False fer=False/True/False aus=[] n_bat=ERRO orfas=2 porta=None status=ok resolver=False comp=11/2026
MARGINAL|POR_QUE|{'feriado_sem_trabalha_em_feriado': 31, 'folga_do_calendario': 285, 'ata_tipo_dia_folga_sem_ausencia_cobrindo': 8, 'ausencia_lancada': 5, 'feriado_trab_feriado_True_mas_folga': 13, 'ausencia_lancada_pelo_colab': 1}
MARGINAL|BAT|{'ERRO': 343}
MARGINAL|PORTA|{None: 340, 'porta_pergunta': 3}
MARGINAL|STATUS|{'folga_trabalhada': 161, 'escala_suspeita': 5, 'irregular': 8, 'folga': 165, 'aberto': 3, 'ok': 1}
MARGINAL|PEDE_DECISAO|{False: 332, True: 11}
MARGINAL|RESOLVER_DIA|{False: 343}
MARGINAL|SEGURA|{False: 163, True: 180}
MARGINAL|DONO|{'CADASTRO': 104, 'BATIDA': 30, 'ESTRUTURA': 209}
MARGINAL|EM|{'A': 170, 'D': 173}
MARGINAL|CLAUSULA|{'trabalha_False': 329, 'ata_tipo_dia=folga': 8, 'ata_tipo_dia=ausencia': 6}
D_FOLGA_TRABALHADA n=161 com_bat=0 sem_porta=161 resolver_dia_True=0 (STATUS_QUE_PEDEM_DECISAO inclui? False)
C444 {'col': 'col444', 'dia': '2026-10-25', 'emp': 2, 'em': 'A', 'clausula': 'trabalha_False', 'tipo_dia_ata': 'folga', 'feriado': False, 'trab_feriado': True, 'fer_vira_folga': False, 'aus': [], 'por_que': 'folga_do_calendario', 'n_bat': 'ERRO', 'n_orfas': 4, 'porta': None, 'status': 'folga_trabalhada', 'status_pede_decisao': False, 'resolver_dia': False, 'segura': False, 'comp': '11/2026', 'dono': 'ESTRUTURA'}
ERRO|dia_das_batidas|col43|2026-11-02|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col49|2026-11-04|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col60|2026-11-03|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col69|2026-10-25|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col97|2026-11-02|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col123|2026-11-05|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col130|2026-10-30|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col137|2026-11-03|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col144|2026-09-21|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col203|2026-11-02|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col207|2026-11-02|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col209|2026-09-21|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col211|2026-10-24|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col227|2026-11-03|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col242|2026-11-02|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col268|2026-09-21|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col270|2026-10-22|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col278|2026-11-02|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col282|2026-11-01|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col292|2026-10-22|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col300|2026-10-24|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col301|2026-10-22|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col312|2026-09-21|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col328|2026-10-22|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col348|2026-10-24|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col354|2026-10-30|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col366|2026-10-27|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col369|2026-11-02|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col373|2026-10-21|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERRO|dia_das_batidas|col384|2026-11-02|'<=' not supported between instances of 'datetime.date' and 'datetime.datetime'
ERROS {'dia_das_batidas:TypeError': 100}
INCOMPLETO: 100 erros
rc_sombra=0
```

**2) Funções, contratos e decisões da sonda**

O corpo de `sonda_dst.py` não está disponível no material consultável; sua identificação/hash na saída não permite conferir suas chamadas. Portanto, os argumentos efetivamente passados a **cada uma** das funções abaixo são [nao verificado].

| Função e localização | Contrato documentado/implementado | Sonda cumpre? |
|---|---|---|
| `dia_sem_trabalho(cel)` — `app/chamados/juizes.py:413-428` | Recebe célula; lê `trabalha` e `ata`. Ausência parcial tem tratamento próprio. | [nao verificado] |
| `censo_alcance(empresas=None, hoje=None, limite=50, lista=None)` — `app/chamados/services/vigia_fio.py:542-554` | Censo de dias retidos sem porta; `empresas=None` usa empresas visíveis. `limite` não corta os dias sem porta. | [nao verificado] |
| `InsumosDoDono.dono(self, data)` — `app/ponto/management/commands/e6_oraculo.py:172-183` | Recebe a data; utiliza os insumos da instância. O construtor recebe colaborador, `ini`, `fim`, `hoje` e exige `agora=` — `:113-145`. | [nao verificado] |
| `dia_das_batidas(colab, ini, fim, turnos=None)` — `app/ponto/turnos.py:2874-2897` | Janela apresentada; `turnos=` é alimentação. Retorna **`{batida.pk: date}`**, não contagem. | [nao verificado] |
| `dias_da_ausencia(tipo, data, data_fim, ini, fim)` — `app/ponto/turnos.py:2774-2790` | Retorna datas no intervalo fechado; ausência sem fim estende até `fim` somente para `TIPOS_EM_ABERTO`. | [nao verificado] |
| `porta_alcancavel_do_dia(celula_id, *, alimentacao=None)` — `app/chamados/juizes.py:1341-1370` | Recebe **ID da célula**; alimentação opcional vem de `alimentar_portas(lote)`. Retorna `None` ou dicionário de rota/fila/verbos/fio. | [nao verificado] |
| `status_dos_dias_lavrados(pares, *, agora=None, carga=None, fatos=None)` — `app/ponto/status_dia.py:221-247` | Pares `(colaborador, data)`; carga opcional deve conter pessoas e datas; `fatos` somente com carga. Validações em `:259-267`. | [nao verificado] |
| `resolver_dia_oferecido_nas_datas(colaborador, datas)` — `app/colaboradores/services/calendario.py:117-138` | Datas de um mesmo colaborador; retorna `{date: bool}` lendo o calendário. Dia ausente do calendário responde `False`. | [nao verificado] |
| `o_que_segura(colab, mes, ano)` — `app/folha/fora_da_folha.py:203-231` | Recebe colaborador e competência; retorna dicionário com os dias retidos e seus motivos/fios. | [nao verificado] |
| `competencia_do_fato(colaborador, quando)` — `app/ponto/services/fechamento.py:1586-1588` | Fato de um dia; retorna `(mes, ano)`. Normaliza `date`/`datetime` pelo caminho `:1542-1583`. | [nao verificado] |

O erro impresso comprova apenas a mensagem de comparação entre `date` e `datetime`; sem a chamada e o traceback, atribuí-lo definitivamente a argumentos errados da sonda seria [nao verificado]. O código chama o pareador quando `turnos` não foi alimentado e contém comparação de datas de segmento com `data_turno`. (`app/ponto/turnos.py:2900-2905,1990-1993`)

**`com_bat=0` não comprova “zero batidas”**: a própria saída registra `MARGINAL|BAT|{'ERRO': 343}`; a contagem correta das batidas permanece [nao verificado].

Quanto às decisões próprias:

- **`por_que`:** a implementação e sua ordem de precedência são [nao verificado]. O juiz existente não devolve esses motivos: devolve um booleano por `trabalha is False` ou tipo de ata preenchido diferente de `trabalho`, com exceção para cobertura parcial. Tratar os nomes impressos como causas oficiais extrapolaria esse contrato. (`app/chamados/juizes.py:413-428`)
- **`folga_do_calendario`:** `trabalha=False`, isoladamente, não documenta a causa da folga; o próprio código explica que a célula guarda esse booleano sem guardar o porquê. O rótulo pode servir ao agrupamento, mas sua derivação pela sonda é [nao verificado]. (`app/escala/models.py:1373-1384`)
- **Os dois rótulos de feriado:** `trabalha_em_feriado=True` pode coexistir com folga; essa flag impede a transformação por feriado, mas não transforma uma folga já existente em trabalho. Portanto, o nome `feriado_trab_feriado_True_mas_folga` não demonstra contradição. A regra completa também considera ciclo e lugar do vínculo; um teste apenas da flag seria insuficiente. (`app/escala/models.py:1367-1371,1382-1395`)
- **`ausencia_lancada` e `ata_tipo_dia_folga_sem_ausencia_cobrindo`:** a ata não laboral já participa do juiz; a lista de ausências não é requisito adicional desse predicado. Se a sonda usa essa lista somente para subdividir os resultados, é agrupamento; se redecide o booleano com ela, a equivalência é [nao verificado]. (`app/chamados/juizes.py:422-428`)
- **`pelo_colab`:** já existe teste de identidade entre o colaborador de `registrado_por` e o colaborador da ausência. Se a sonda compara `registrado_por_id` com `colab.usuario_id`, repete essa identidade; a comparação efetivamente escrita é [nao verificado]. Essa identidade não prova origem via disputa nem aprovação. (`app/ponto/services/ausencia.py:627-631`; `app/colaboradores/models.py:351`; `app/chamados/services/materializacao.py:495-528`)

**3) Guarda do emissor, outros escritores e leitores da constante**

`motivos_a_perguntar` executa nesta ordem:

1. Recusa por `dia_sem_trabalho(cel)`; somente se falso, pelo curto-circuito do `or`, verifica colaborador diferente de `ativo`. (`app/chamados/services/fabrica_da_celula.py:89-92`)
2. Recusa data futura. (`app/chamados/services/fabrica_da_celula.py:93-94`)
3. Obtém `agora`, calcula marcos vencidos e recusa veredito preenchido fora de `VEREDITOS_ACUSAM`. (`app/chamados/services/fabrica_da_celula.py:95-100`)
4. Recusa ausência parcial destinada somente à lista administrativa, por `parcial_so_lista`. (`app/chamados/services/fabrica_da_celula.py:101-104`)
5. Carrega perguntas existentes, exclui duplicatas e devolve somente motivos ainda não perguntados. O cálculo dos motivos aplica o conjunto de marcos vencidos e as guardas de ata. (`app/chamados/services/fabrica_da_celula.py:118-126,45-64`)

Para as linhas cuja cláusula impressa é `trabalha_False`, **a primeira guarda basta**, independentemente de o status ser `folga_trabalhada`. (`app/chamados/juizes.py:428`; `app/chamados/services/fabrica_da_celula.py:91-92`; correspondência efetiva das células com a impressão: [nao verificado])

**Sim, existem caminhos fora dessa fábrica que podem criar chamado da célula ou pergunta:**

- **Chamado da célula:** a abertura administrativa aceita tipo catalogado e data do fato; por exemplo, `ausencia_colab` tem escopo DIA e chave `data`. A criação resolve a célula por colaborador/data. As guardas dessa criação não constituem proibição universal de `folga_trabalhada`. (`app/chamados/services/abertura.py:147-153,172-192`; `app/chamados/catalogo/modulos.py:120-122`; `app/chamados/models.py:1026-1039,1073-1076,1097-1106,1193-1201`)
- **Pergunta por batida em folga:** o detector devolve anomalia grave quando a escala diz folga; a API encaminha o motivo à disputa; o helper gera perguntas. A data é extraída do contexto e `PerguntaDisputa.abrir` associa a célula daquele colaborador/data, sem consultar `dia_sem_trabalho` nesse nascimento. (`app/chamados/detector_anomalias.py:164-175`; `app/api/views_core.py:725-730,771-780`; `app/chamados/services/disputa_emissao.py:350-366,389-397,598-618`; `app/chamados/models.py:2428-2430,2476-2477`)

**Nascer não basta para constituir porta alcançável:** o chamado precisa estar na fila; pela pergunta, ela precisa estar viva, no app e sob âncora na fila. Uma pergunta

[truncado em 20000 de 22672 caracteres]

## 06/11 18:25 · sessao 2da3cfdb4a00 · PAUTA-08 · verificador · origem=Code

1. **Fecha o achado da volta 2 sobre congelar o gravado até a gravação da pauta, mas não estabelece essa garantia para toda escrita da reversão.** O comando agora executa o `select_for_update` de `FechamentoMensal`, confere o hash e escreve cabeça, filhas e atualização da reversão dentro do mesmo `atomic` por colaborador. Não identifiquei caminho que persista uma cabeça nova sem essa conferência no mesmo bloco. ([comando:190](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:190), [comando:196](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196), [comando:199](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:199), [comando:207](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:207), [comando:217](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:217))

   **Existe, porém, reversão gravada sem conferência no mesmo `atomic`: o JSON inicial.** Ele recebe o plano e os hashes medidos anteriormente, com `pautas=[]`, e é escrito antes do laço e da transação. O próprio teste de hash divergente abre esse arquivo após a recusa. ([comando:120](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:120), [comando:162](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:162), [comando:167](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:167), [teste:211](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:211))

   O arquivo também continua fora do rollback do banco: falha no commit posterior à sua atualização pode deixar PKs mortos nele; `_reverter` pula PK inexistente. A comparação final ocorre depois das transações por colaborador. ([comando:214](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:214), [comando:228](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:228), [comando:275](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:275))

2. **O teste novo morde presença e ordem do SQL, não toda a garantia anunciada.** Exige algum `FOR UPDATE` contendo `fechamentomensal` antes do primeiro `INSERT` contendo `pauta`. Não verifica o filtro, linhas bloqueadas, lock anterior ao hash, duração do bloqueio ou concorrência. ([teste:225](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:225), [teste:228](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:228), [teste:231](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:231))

   **Mutante que passaria por análise estática:** mover somente o lock da linha 196 para depois do `if/raise` de hash, antes de `_existente`. Continua havendo lock antes do `INSERT`; o teste de hash adulterado continua recusando antes de criar pautas ou acrescentar PKs ao JSON. As demais verificações seriais preservam seus resultados. Execução dos 26 testes com esse mutante: **[nao verificado]**. ([comando:196](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196), [comando:199](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:199), [comando:202](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:202), [teste:204](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:204), [teste:217](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:217))

   Esse mutante reabre uma janela entre conferência e lock: uma alteração concorrente pode entrar ali, seguida da escrita da cabeça sem reconferência; o hash final só reclamaria depois da saída do `atomic`. ([comando:199](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:199), [comando:207](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:207), [comando:218](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:218), [comando:228](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:228))

3. **Sim: a frase nova sobre HE autorizada em exportada corresponde ao código, condicionada a haver autorizações efetivas.** `_ato` chama automaticamente `_linha_para_o_dp` quando `exportada` e `autorizados` são verdadeiros; o emissor chama `pautas.services.escrever`. A confirmação envolve `_ato` em transação e recusa divergência do DIFF mostrado; a prévia desfaz sua transação. ([LEIS.md:177](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/docs/LEIS.md:177), [autorizacao_he_periodo.py:299](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/services/autorizacao_he_periodo.py:299), **[emissor:244](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/services/autorizacao_he_periodo.py:244)**, [prévia:321](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/services/autorizacao_he_periodo.py:321), [confirmação:343](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/services/autorizacao_he_periodo.py:343))

4. **Escritores e leitores no caminho examinado:**

   | Objeto | Quem escreve/produz | Quem lê |
   |---|---|---|
   | Hash do gravado | `hash_do_gravado` calcula MD5 truncado sobre campos de `FechamentoMensal`; o comando guarda o resultado no plano e no JSON. ([regen_exportada.py:62](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/portas/regen_exportada.py:62), [comando:120](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:120), [comando:165](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:165)) | O comando compara por colaborador e no fim. A porta REGEN calcula antes/depois, envia à auditoria e devolve `mudou`. ([comando:199](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:199), [comando:228](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:228), [regen_exportada.py:206](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/portas/regen_exportada.py:206), [regen_exportada.py:218](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/portas/regen_exportada.py:218)) |
   | `FechamentoMensal` | PAUTA-08 pede somente leitura. No ramo escritor relacionado, `recalcular_fechamento_mes` usa `get_or_create`, `save` ou `update`; REGEN chama esse ramo. ([comando:135](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:135), [fechamento.py:889](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/services/fechamento.py:889), [fechamento.py:1128](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/services/fechamento.py:1128), [fechamento.py:1144](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/services/fechamento.py:1144), [regen_exportada.py:179](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/portas/regen_exportada.py:179)) | `hash_do_gravado`, `valores_do_gravado`, recálculo que semeia objeto destacado e novo SELECT de bloqueio. ([regen_exportada.py:71](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/portas/regen_exportada.py:71), [regen_exportada.py:81](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/portas/regen_exportada.py:81), [fechamento.py:881](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/ponto/services/fechamento.py:881), [comando:196](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196)) |
   | Arquivo de reversão | `_escrever` chama `_grava` inicialmente e após acrescentar PKs; `_grava` serializa em temporário e executa `os.replace`. ([comando:167](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:167), [comando:216](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:216), [comando:302](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:302)) | `_reverter` carrega JSON, valida marca/schema e percorre PKs para cancelar pautas; **não reconfere `hash_gravado` do plano**. ([comando:262](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:262), [comando:266](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:266), [comando:273](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:273), [comando:283](/home/ronald/codex_ws/54619807cdfb351178aec93562973eb3f60cad4c/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:283)) |

   Ocorrência desses cenários em produção: **[nao verificado]**.

## 06/11 18:23 · sessao 88a1e5c18480 · - · - · origem=Ronald (tmux)

O teste novo **morde a presença e a ordem textual do SQL**, não a garantia inteira anunciada: procura qualquer `FOR UPDATE` em `fechamentomensal` antes do primeiro `INSERT` contendo `pauta`; não verifica se o lock antecede o hash, quais linhas foram selecionadas ou uma disputa concorrente. (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:225`, `:228`, `:229`, `:231`)

**Mutante preferível:** mover somente o lock de `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196` para depois da comparação/recusa de hash (`:199`–`:201`), antes de `_existente` (`:202`). O SQL continua antes do `INSERT` (`:207`), satisfazendo o novo teste; o teste do hash adulterado continua recusando antes das criações e preservando a lista vazia do JSON. (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:204`, `:213`, `:214`, `:217`, `:231`)

Esse mutante permite alteração concorrente **entre a conferência e a aquisição do lock**: a pauta pode ser criada sem nova conferência depois da alteração; a comparação final só ocorre após sair do `atomic` daquela pessoa. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:190`, `:199`, `:207`, `:218`, `:228`)

Outro mutante explícito: acrescentar `pk__isnull=True` ao filtro do lock, gerando `SELECT … FOR UPDATE` sem travar linha alguma; o teste não inspeciona o filtro nem o resultado. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196`; `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:228`, `:231`)

A previsão de que esses mutantes passam pelos 26 testes decorre da inspeção estática das asserções; execução dos testes e dos mutantes: **[nao verificado]**. [nao verificado]

## 06/11 18:23 · sessao a75e25bc3777 · - · - · origem=Ronald (tmux)

1. `motivos_a_perguntar` recusa na seguinte ordem:

   - Primeiro `dia_sem_trabalho(cel)`; por curto circuito, só consulta situação do colaborador se esse juiz devolver falso. Colaborador diferente de ativo também recusa. **`app/chamados/services/fabrica_da_celula.py:89-92`**.
   - Data futura. **`app/chamados/services/fabrica_da_celula.py:93-94`**.
   - Calcula `agora` e marcos vencidos; depois recusa veredito preenchido que não esteja em `CelulaDia.VEREDITOS_ACUSAM`. **`app/chamados/services/fabrica_da_celula.py:95-100`**.
   - Recusa parcial destinado só à lista administrativa (`parcial_so_lista`). **`app/chamados/services/fabrica_da_celula.py:101-104`**.
   - Consulta perguntas existentes, excetuando duplicatas, e só devolve motivos da ata ainda não perguntados. Teto dos marcos é aplicado dentro de `motivos_pela_ata`. **`app/chamados/services/fabrica_da_celula.py:118-126`**, **`:45-64`**.

   O juiz considera `trabalha is False` ou tipo de ata preenchido diferente de trabalho; ausência parcial deixa o restante cobrável, salvo `trabalha is False`. **`app/chamados/juizes.py:422-428`**. Logo, para os registros cuja cláusula é `trabalha_False`, a primeira guarda já explica a recusa; não depende do status `folga_trabalhada`. **`app/chamados/services/fabrica_da_celula.py:91`**, **`app/chamados/juizes.py:428`**.

2. **Há outros escritores; ausência de porta na medição não prova impossibilidade de emissão.**

   Caminho específico para batida em folga:

   - `detectar_batida_fora_escala` usa `esc.eh_dia_trabalho(data_turno_ref)` e devolve grave / `batida_em_folga` quando falso. **`app/chamados/detector_anomalias.py:164-175`**.
   - API converte grave em motivo `batida_fora_escala_grave` e chama `criar_ou_atualizar_disputa`, preenchendo também `ultima_batida.timestamp`. **`app/api/views_core.py:725-730`**, **`:771-780`**; mesmo código em `app/api/views.py`.
   - O helper abre/agrega disputa e chama o gerador de perguntas. **`app/chamados/services/disputa_emissao.py:598-618`**.
   - O gerador lê `ultima_batida.timestamp`, converte para data local e passa `data_evento` a `PerguntaDisputa.abrir`. **`app/chamados/services/disputa_emissao.py:350-366`**, **`:389-397`**.
   - A porta consulta a célula por colaborador/data e cria a pergunta com essa célula; não tem guarda `dia_sem_trabalho`. **`app/chamados/models.py:2428-2430`**, **`:2476-2477`**.
   - Nascer pergunta não garante porta alcançável: `porta_alcancavel_do_dia` ainda exige pergunta viva, no app e âncora na fila. **`app/chamados/juizes.py:1378-1381`**. Pergunta respondida com texto pode permanecer viva apesar de a célula não acusar, salvo decisão administrativa; pergunta nova sem resposta pode morrer pelo veredito não acusador. **`app/chamados/juizes.py:1150-1162`**, **`:1181-1196`**.

   Outro caminho concreto de **chamado da célula**: abertura administrativa no fio recebe tipo catalogado e data do fato, cria chamado com a chave de data; `criar` resolve a célula do colaborador/data. Exemplo de tipo permitido: `ausencia_colab`, de escopo DIA, chave `data`. **`app/chamados/services/abertura.py:147-153`**, **`:172-192`**; **`app/chamados/catalogo/modulos.py:120-122`**; **`app/chamados/models.py:1026-1028`**, **`:1193-1201`**. As guardas posteriores são absolvição de módulo de turno e veto da régua; não constituem veto universal a `folga_trabalhada`. **`app/chamados/models.py:1031-1039`**, **`:1073-1076`**, **`:1097-1106`**. O chamado só constitui porta se integrar a fila considerada pelo juiz. **`app/chamados/juizes.py:1375-1386`**.

   Outros emissores de cobrança possuem guardas diferentes: detector canônico chama `celula_veta_cobranca`; c2 também, e essa função veta `trabalha is False` sem chamar `dia_sem_trabalho`. **`app/ponto/management/commands/detectar_ausencias.py:630-633`**, **`app/chamados/reconciliador.py:876-880`**, **`app/ponto/marcos.py:154-163`**. Portanto não se deve dizer que “todo escritor passa por dia_sem_trabalho”. **Mesmas referências.**

3. `STATUS_QUE_PEDEM_DECISAO`: a busca em Python encontrou somente definição e uso produtivo em `resolver_dia_oferecido`; fora dele, leitura direta apenas no teste que importa, compara a tupla e a percorre. **`app/colaboradores/services/calendario.py:94`**, **`:103-104`**; **`app/chamados/tests/test_o261_vigia_fio.py:901-903`**. O vigia lê indiretamente a decisão pela porta plural do calendário. **`app/chamados/services/vigia_fio.py:263-271`**.

4. Não encontrei escritores chamados `extra_sem_aviso` ou `batida_orfa`; `batida_orfa` aparece como mapeamento legado de categoria para `orfao_14h`. **`app/chamados/models.py:3025-3026`**. Não usar os nomes como escritores provados. A ocorrência de `BATIDA_ORFA_FORA_TOLERANCIA` é protesto do suprajuiz. **`app/ponto/supra_juiz.py:518-522`**.

## 06/11 18:22 · sessao 4f61a43c87d5 · - · - · origem=Ronald (tmux)

Leitor existente e alcance:

- `app/ponto/services/dia_decidido.py:529` — `decididos_do_periodo(colaborador, data_ini, data_fim, *, ausencias=None)` responde `{date: cartao_do_dia}`. A consulta seleciona ausências do colaborador que tocam a janela, sem filtro por autor, status ou chamado (`:550-555`); expande cada ausência entre `data` e `data_fim or data`, recortada pela janela (`:558-562`). Assim alcança também ausência registrada pelo próprio colaborador, desde que o dia esteja nessa janela.
- `app/ponto/services/dia_decidido.py:512-526` — `cartao_do_dia` entrega veredito, cor, rótulo, minutos, tipo, status, ID, quem e quando. `quem_quando` prefere `aprovado_por`, depois `registrado_por`, e usa `aprovada_em or criado_em` (`:500-509`).
- A grade do admin carrega `decididos_do_periodo` em `app/colaboradores/services/calendario.py:531-534`, fornece a decisão a `do_dia` em `:715-720` e publica palavra/cor/veredito em `:724`. O template mostra a palavra e usa o tooltip “O que foi decidido neste dia” em `app/templates/colaboradores/partials/_calendario_grade.html:98`; se há ausência, o chip usa a palavra no título e abre o registro (`:97`).
- O espelho carrega o mesmo leitor em `app/ponto/services/espelho.py:647-649`, transporta `_decisao_do_dia` em `:871` e fornece a `do_dia` em `:340-345`.
- Atenção: alcance não garante que a palavra mostrada será “decidido/aprovado”. `do_dia` considera cobertura, decisão e pedido pendente (`app/ponto/services/dia_decidido.py:399-435`). Pedido pendente no espelho ganha palavra “aguardando decisão” (`:420-425`). Os dados raspados não trazem `Ausencia.status` da ausência 4670; aprovação e palavra efetiva para col889 são `[nao verificado]`.

Autoria:

- O teste de identidade já existe no gate de aprovação: `app/ponto/services/ausencia.py:627-631` verifica se `ausencia.registrado_por.colaborador.pk == ausencia.colaborador_id`. A relação inversa é sustentada por `Colaborador.usuario`, OneToOne de `auth.User`, `related_name="colaborador"` em `app/colaboradores/models.py:351`.
- Se a sonda compara `registrado_por_id == colab.usuario_id`, repete essa identidade existente; a chamada exata da sonda permanece `[nao verificado]`. Identidade do autor, sozinha, não comprova origem via disputa.
- A autodeclaração via disputa está documentada em `app/chamados/services/materializacao.py:495-508`; a criação passa `registrado_por=user` e `chamado_origem=p.chamado_destino` em `:520-528`. O caminho `declara_falta` passa `request_user` e `ch` em `:484-492`.
- Não encontrei `_quem_da_ausencia`; a função de autoria no módulo é `quem_quando`, `app/ponto/services/dia_decidido.py:500`.

Contratos das funções:

- `app/ponto/turnos.py:2774-2776` — `dias_da_ausencia(tipo, data, data_fim, ini, fim)` devolve lista de datas em intervalo fechado. Sem `data_fim`, estende até `fim` somente para `TIPOS_EM_ABERTO`; nos demais tipos usa um dia (`:2779-2789`).
- `app/ponto/turnos.py:2874-2897` — `dia_das_batidas(colab, ini, fim, turnos=None)` devolve **mapa `{batida.pk: date}`**, não lista/número. `ini/fim` são a janela apresentada; `turnos=` é alimentação. A implementação chama `turnos_do_colab` e `janela_com_pad` (`:2900-2905`).
- `turnos_do_colab` documenta intervalo por `data_turno` em `app/ponto/turnos.py:1848-1857`; o corpo compara `seg_ini <= t.data_turno <= seg_fim` em `:1993`. Um `datetime` misturado com `date` poderia falhar, mas sem a sonda/traceback a origem exata do TypeError é `[nao verificado]`.
- A busca por nome não encontrou `sonda_dst.py` na cópia; argumentos efetivamente utilizados pela sonda são `[nao verificado]`.

## 06/11 18:22 · sessao 5a6a06837842 · DIA-SEM-TRABALHO-PORTA · leitura · origem=Code

HASH LIDO = 7c140052a45f90bc52eb5d68726782428fe9307c [nao verificado]

| id | regra de origem (L-NNN, corte literal do aval ou arquivo:linha da regra) | entrada (model e campos, cada campo com arquivo:linha da definicao) | esperado OBSERVAVEL (o que um teste ve: retorno, linha gravada, status, texto) | mutante que tem de morder (a alteracao de codigo que este caso pega) | tipo golden/borda/adversarial | [nao verificado]
|---|---|---|---|---|---|
| G01 | Aval literal: “(1) batida em FOLGA DO CALENDARIO -> porta de decisao do admin (folga trabalhada)”. | Colaborador `situacao='ativo'` (app/colaboradores/models.py:323). FolgaDia `escala_colaborador=V`, `data=05/10/2026`, `calendario_origem=F` (app/escala/models.py:1585; app/escala/models.py:1588; app/escala/models.py:1589). CelulaDia `colaborador=C`, `data=05/10`, `trabalha=False` (app/escala/models.py:1681; app/escala/models.py:1685; app/escala/models.py:1686). Batida `colaborador=C`, `timestamp=05/10 12h e 16h`, `tipo=E/S` (app/ponto/models.py:22; app/ponto/models.py:23; app/ponto/models.py:24); apresentação UTC−3 (CLAUDE.md:143). | O teste alcança uma ação do admin para decidir a folga trabalhada desse dia. Botão, URL, entidade criada e retorno obrigatório de uma função específica: **regra nao decide**. “Dia decidido” antes da decisão: **regra nao decide**. A distinção entre porta alcançável e “Resolver dia” está escrita em app/chamados/juizes.py:1344 e app/chamados/juizes.py:1367. | Tratar toda célula `trabalha=False` como motivo suficiente para eliminar também a decisão administrativa (guarda em app/chamados/services/fabrica_da_celula.py:91). | golden |
| G02 | Aval literal: “(2) batida em FERIADO sem trabalha_em_feriado -> porta de decisao”. | Colaborador e Batida como G01. Feriado `data=06/10/2026`, `abrangencia='nacional'` (app/core/models.py:12; app/core/models.py:13). EscalaColaborador `trabalha_em_feriado=False` (app/escala/models.py:896). CelulaDia `colaborador=C`, `data=06/10`, `trabalha=False` (app/escala/models.py:1681; app/escala/models.py:1685; app/escala/models.py:1686). Batidas em 06/10, mesmos campos de G01. | O teste alcança uma ação administrativa para decidir o trabalho nesse feriado. Botão, URL, entidade e retorno obrigatório de função específica: **regra nao decide**. “Dia decidido” antes da decisão: **regra nao decide**. Não se exige `fio='admin'` só pelo nome do ator: a escada de fios distingue validar de outros verbos de porta (app/chamados/juizes.py:1323). | Suprimir a decisão porque o feriado foi materializado como `trabalha=False` (app/chamados/juizes.py:413). | golden |
| G03 | Aval literal: “(3) dia com AUSENCIA LANCADA -> NENHUMA porta, mostrado como ‘dia decidido’”; representação da decisão em app/docs/PROMPTS.md:53. | CelulaDia `colaborador=C`, `data=07/10/2026` (app/escala/models.py:1681; app/escala/models.py:1685). Ausencia `colaborador=C`, `tipo='atestado'`, `data=07/10`, `data_fim=07/10`, `status='aprovada'` (app/ponto/models.py:743; app/ponto/models.py:749; app/ponto/models.py:750; app/ponto/models.py:751; app/ponto/models.py:799). Sem Batida para C nesse dia (app/ponto/models.py:22; app/ponto/models.py:23). | Nenhuma porta para decidir novamente esse dia, pelo aval. Grade, cartão e app mostram a decisão com palavra e efeito do catálogo, verde para abono; nunca só tick (app/docs/PROMPTS.md:53; app/ponto/catalogo/ausencias.py:95). Exigir literalmente o texto “dia decidido”, um status com esse nome ou um retorno específico: **regra nao decide**. | Oferecer nova decisão apesar da ausência aprovada; ou representar a decisão apenas por tick (app/ponto/services/dia_decidido.py:3). | golden |
| B01 | Aval literal: “batida cross-meia-noite que cai no dia de folga pela autoridade de dia ponto/turnos.py::dia_das_batidas”; dia reclamado pelo turno em app/ponto/turnos.py:2877. | Entrada G01, com CelulaDia `data=05/10` (app/escala/models.py:1685). Batida `timestamp=05/10 22h e 06/10 06h`, `tipo=E/S`, `colaborador=C` (app/ponto/models.py:23; app/ponto/models.py:24; app/ponto/models.py:22). Pré-condição: a autoridade atribui ambas a 05/10 (app/ponto/turnos.py:2875). | A ação administrativa exigida por G01 pertence a **05/10**, incluindo a saída de 06/10. O mapa de dia associa as duas identidades de batida a 05/10 (app/ponto/turnos.py:2877; app/ponto/turnos.py:2892). | Agrupar por `timestamp.date()` e perder a saída na avaliação da folga (app/ponto/turnos.py:2883). | borda |
| B02 | Batida reclamada pertence ao dia do turno, não ao dia civil da saída (app/ponto/turnos.py:2877). | CelulaDia `data=06/10`, `trabalha=False` (app/escala/models.py:1685; app/escala/models.py:1686). Batida `timestamp=06/10 06h`, `tipo='S'`, `colaborador=C` (app/ponto/models.py:23; app/ponto/models.py:24; app/ponto/models.py:22). Pré-condição: a autoridade reclama a saída para 05/10 (app/ponto/turnos.py:2877). | O mapa atribui a saída a 05/10. Essa saída não comprova trabalho na folga de 06/10; existência de outra causa de porta em 06/10: **regra nao decide** (app/ponto/turnos.py:2877). | Usar a data civil da saída como evidência de trabalho na folga seguinte (app/ponto/turnos.py:2883). | borda |
| B03 | Aval literal do feriado, combinado com `feriado_vira_folga`: ciclos weekday, flag falsa e praça do vínculo (app/escala/models.py:1373; app/escala/models.py:1382). | Entrada G02. TipoEscala `tipo_ciclo='5x2'` (app/escala/models.py:117; app/escala/models.py:29). EscalaColaborador `tipo_escala=T`, `posto=P`, `trabalha_em_feriado=False` (app/escala/models.py:874; app/escala/models.py:873; app/escala/models.py:896). Feriado cadastrado na praça de P, com `data`, `abrangencia`, `uf`, `municipio` (app/core/models.py:12; app/core/models.py:13; app/core/models.py:14; app/core/models.py:15). | `feriado_vira_folga` responde verdadeiro nas condições escritas; a ação de decisão de G02 continua exigida. A conversão para folga não elimina o fato de ser feriado (app/escala/models.py:1377; app/escala/models.py:1382). | Fazer a conversão para folga apagar a necessidade da decisão do feriado trabalhado (app/escala/models.py:1369). | borda |
| B04 | A conversão exige `trabalha_em_feriado=False` (app/escala/models.py:1382); o aval mínimo também contém essa condição. | Entrada B03, alterando apenas EscalaColaborador `trabalha_em_feriado=True` (app/escala/models.py:896). | `feriado_vira_folga=False`, pela condição escrita (app/escala/models.py:1382). Obrigatoriedade ou proibição de porta pelo trabalho nesse feriado: **regra nao decide** no aval desta obra. | Ignorar a flag e converter qualquer feriado em folga (app/escala/models.py:1390). | borda |
| B05 | Ausência autodeclarada via disputa: falta nasce aprovada, “confissao contra interesse: nada a decidir” (app/chamados/services/materializacao.py:495); aval literal do caso 3. | DisputaSupervisao `colaborador=C` (app/chamados/models.py:1666). PerguntaDisputa `disputa=D`, `celula=CD`, `data_evento=08/10`, `resposta_colab='FALTA_DIA'`, `respondida_em` e `validada_em` preenchidos (app/chamados/models.py:2211; app/chamados/models.py:2209; app/chamados/models.py:2218; app/chamados/models.py:2227; app/chamados/models.py:2228; app/chamados/models.py:2229). Ausencia resultante `tipo='falta'`, `status='aprovada'` (app/ponto/models.py:749; app/ponto/models.py:799). | A ausência nasce aprovada; a autoria da declaração não impede o dia decidido (app/chamados/services/materializacao.py:498). Nenhuma nova porta para decidir o mesmo dia, pelo aval. Exibição com palavra de falta, desconto e horas em vermelho (app/docs/PROMPTS.md:53; app/ponto/catalogo/ausencias.py:88). | Exigir aprovação adicional só porque a falta veio da resposta do próprio colaborador (app/chamados/services/materializacao.py:498). | borda |
| B06 | Atestado autodeclarado sem documento não nasce decidido (app/chamados/services/materializacao.py:499); ausência aberta conserva o chamado vivo e silenciado (app/chamados/services/materializacao.py:556). | Mesma disputa de B05, com PerguntaDisputa `resposta_colab='ATESTADO'` (app/chamados/models.py:2227). Ausencia `tipo='atestado'`, `status='aguardando_documento'` (app/ponto/models.py:749; app/ponto/models.py:799). | O lançamento aparece pendente, amarelo, sem antecipar abono aprovado (app/ponto/services/dia_decidido.py:37). O chamado não é fechado como ausência decidida (app/chamados/services/materializacao.py:560). Aplicação da frase genérica “AUSENCIA LANCADA -> NENHUMA porta” a esse estado aberto: **regra nao decide** no aval específico. | Tratar a mera existência de Ausencia como aprovação e fechar o chamado (app/chamados/services/materializacao.py:556). | borda |
| B07 | “rejeitada nunca cobre dia” (app/docs/PROMPTS.md:50); rejeição deve continuar visível (app/ponto/services/dia_decidido.py:11). | Entrada G03, alterando Ausencia `status='rejeitada'` (app/ponto/models.py:799). | A ausência não cobre folha nem suspende cobrança; a exibição informa o atestado rejeitado, em laranja (app/ponto/services/dia_decidido.py:11; app/docs/PROMPTS.md:53). Obrigatoriedade de uma porta de decisão do **dia** só por essa rejeição: **regra nao decide**. A ação “Reverter rejeicao” é prescrita separadamente (app/docs/PROMPTS.md:50). | Aplicar a ausência rejeitada como cobertura aprovada ou ocultar a rejeição (app/ponto/services/dia_decidido.py:11). | borda |
| B08 | Abono parcial cobra o resto; a lâmpada distingue marco dispensado de marco apagado (app/chamados/juizes.py:403). | CelulaDia `trabalha=True`, `ata` com `tipo_dia='ausencia'`, `cobertura_parcial=True` e marco restante apagado (app/escala/models.py:1686; app/escala/models.py:1778; definição dos carimbos em app/chamados/juizes.py:407). Ausencia `tipo='declaracao'`, `status='aprovada'` (app/ponto/models.py:749; app/ponto/models.py:799). | A parte descoberta continua cobrável; não se dispensa o dia inteiro pelo rótulo de ausência (app/chamados/juizes.py:403). Porta exata e momento de sua oferta: **regra nao decide** nesta cláusula. | Fazer toda `ata.tipo_dia='ausencia'` eliminar também a cobrança dos marcos descobertos (app/chamados/juizes.py:423). | borda |
| B09 | Competência padrão 21→20 (CLAUDE.md:81); saída do cross de fim recebe dia mesmo estando em fim+1 (app/ponto/turnos.py:2895). | Empresa `dia_inicio_competencia=21` (app/colaboradores/models.py:46). Entrada G01 parametrizada com CelulaDia `data=20/10` e `21/10` (app/escala/models.py:1685). No primeiro, Batida `timestamp=20/10 22h e 21/10 06h` (app/ponto/models.py:23), ambas atribuídas a 20/10 pela autoridade (app/ponto/turnos.py:2877). | A decisão do dia 20 pertence à competência 10; a do dia 21, à 11. A saída em 21/10 do turno de 20/10 permanece com o dia 20 (CLAUDE.md:81; app/ponto/turnos.py:2895). A exigência de porta dos casos mínimos não ganha exceção de competência no aval literal. | Recortar por mês civil ou descartar a saída porque seu timestamp ultrapassa o dia 20 (app/ponto/turnos.py:2888). | borda |
| B10 | Folga de calendário pertence ao vínculo e só vale nos dias dele; geradora manda dentro da vigência (app/docs/JUIZES.md:94; app/escala/alimentacao.py:272). | EscalaColaborador V antigo `data_inicio=01/09`, `data_fim=10/10`, `ativa=False`; V novo `data_inicio=11/10`, `ativa=True` (app/escala/models.py:875; app/escala/models.py:876; app/escala/models.py:877). FolgaDia `escala_colaborador=V antigo`, `data=05/10` (app/escala/models.py:1585; app/escala/models.py:1588). CelulaDia `escala_geradora=V antigo`, `data=05/10` (app/escala/models.py:1692; app/escala/models.py:1685), com batidas de G01. | A troca posterior não apaga a folga válida de 05/10 nem a decisão exigida por G01. O vínculo antigo responde por esse dia apesar de atualmente inativo (app/escala/alimentacao.py:272). | Consultar apenas o vínculo atualmente ativo e reclassificar o passado (app/escala/alimentacao.py:272). | borda |
| B11 | Geradora fora da vigência devolve `None`, dono CADASTRO, sem escolher outro vínculo (app/escala/alimentacao.py:272); L-099 (CLAUDE.md:176). | Entrada B10, mas CelulaDia `data=15/10`, `escala_geradora=V antigo` (app/escala/models.py:1685; app/escala/models.py:1692), cuja EscalaColaborador `data_fim=10/10` (app/escala/models.py:876). | A autoridade do vínculo devolve `None`; divergência com dono CADASTRO (app/escala/alimentacao.py:272). Classificação do dia como folga trabalhada e porta específica: **regra nao decide** para esse cadastro inválido. | Substituir silenciosamente a geradora inválida pelo vínculo novo (app/escala/alimentacao.py:277). | borda |
| B12 | O aval exige **batida** nos casos 1 e 2; todo dia não trabalhado leva palavra do veredito (app/ponto/services/dia_decidido.py:186). | Entrada G01 sem nenhuma Batida de C no dia (campos de seleção em app/ponto/models.py:22; app/ponto/models.py:23); sem Ausencia para C/data (app/ponto/models.py:743; app/ponto/models.py:750). | A folga é mostrada com palavra, conforme a regra de representação (app/ponto/services/dia_decidido.py:186). Oferecer ou proibir porta administrativa nessa folga sem batida: **regra nao decide** no aval. Não há ausência decidida fornecida na entrada. | Exibir somente ícone/tick para a folga, sem palavra (app/ponto/services/dia_decidido.py:186). | borda |
| B13 | Sem decisão humana, furo não vira falta decidida; dia previsto encerrado sem batida é irregular (app/ponto/status_dia.py:122); falta tem um significado, decidida (app/docs/LEIS.md:87). | CelulaDia `colaborador=C`, `data=09/10`, `trabalha=True` (app/escala/models.py:1681; app/escala/models.py:1685; app/escala/models.py:1686). Sem Batida para C/data (app/ponto/models.py:22; app/ponto/models.py:23), sem Ausencia para C/data (app/ponto/models.py:743; app/ponto/models.py:750). Dia encerrado como pré-condição da regra (app/ponto/status_dia.py:122). | O status não afirma falta decidida: é irregular nas condições escritas (app/ponto/status_dia.py:122). O furo aparece como dia em aberto, separado de falta decidida (app/docs/CORTES.json:77). | Transformar automaticamente ausência de batida em falta humana já decidida (app/ponto/status_dia.py:122). | borda |
| B14 | “ultimo recurso + passado + status que pede decisao” (app/colaboradores/services/calendario.py:89). | Item do calendário `data=05/11, 06/11 ou 07/11`, `status='irregular'`, `pode_ultimo_recurso=True` (app/colaboradores/services/calendario.py:722; app/colaboradores/services/calendario.py:726; app/colaboradores/services/calendario.py:738). Parâmetro `hoje=06/11` (app/colaboradores/services/calendario.py:97). | `resolver_dia_oferecido` é verdadeiro somente para 05/11; falso para hoje e futuro, pela exigência de passado (app/colaboradores/services/calendario.py:100). Existência de outra porta hoje: **regra nao decide** nessa regra do botão. | Trocar o limite de passado por inclusão de hoje (app/colaboradores/services/calendario.py:103). | borda |
| A01 | “chamar 2x com a mesma entrada = mesmo estado, 1 linha de trilha, 0 chamado/pergunta duplicada” (app/docs/ESTRUTURAL.md:22); aval exige “rodar 2x = mesma porta, 0 duplicata”. | Entrada G01 ou G02, repetida duas vezes, conservando os mesmos Colaborador, CelulaDia, FolgaDia/Feriado e Batida com os campos e definições dessas entradas. | Mesma porta lógica e mesmo estado final; uma linha de trilha do ato idempotente, zero chamados/perguntas duplicados (app/docs/ESTRUTURAL.md:22). Qual entidade materializa a porta: **regra nao decide** no aval desta obra. | Recriar a porta ou sua trilha a cada julgamento repetido (app/docs/ESTRUTURAL.md:22). | adversarial |
| A02 | L-099 exige determinismo e idempotência com o cadastro existente (CLAUDE.md:176); contrato de duplicatas em app/docs/ESTRUTURAL.md:22. | Entrada G01 entregue simultaneamente a duas execuções; identidades e campos dos models permanecem os mesmos de G01. | Ao término das duas execuções, a decisão continua alcançável e há zero chamados/perguntas duplicados (aval do caso 1; app/docs/ESTRUTURAL.md:22). Ordem de conclusão, bloqueio, isolamento e retorno transitório de cada execução: **regra nao decide**. | Permitir que duas execuções criem duplicatas para a mesma entrada (app/docs/ESTRUTURAL.md:22). | adversarial |
| A03 | Universo de apuração exclui retratada (CLAUDE.md:155); exibição conserva a marcação com asterisco (app/ponto/models.py:64). | Entrada G01 com as únicas Batida `retratada_em` preenchidas (app/ponto/models.py:67); `colaborador`, `timestamp`, `tipo` mantidos (app/ponto/models.py:22; app/ponto/models.py:23; app/ponto/models.py:24). | As batidas não entram no universo apurável; continuam na exibição de marcações com asterisco (CLAUDE.md:155; app/ponto/models.py:64). Existência ou destino histórico da porta após retratação: **regra nao decide** no aval. | Contar a batida retratada como trabalho apurável para acionar o caso de folga trabalhada (app/ponto/turnos.py:2847). | adversarial |
| A04 | Status e veredito são escritos no mesmo ato/save (app/docs/ADR/0002-celula-e-o-dia.md:44); evento julga, consumidor lê a lâmpada (CLAUDE.md:168). | CelulaDia `veredito`, `status_dia`, `status_rotulo` inicialmente nulos (app/escala/models.py:1770; app/escala/models.py:1816; app/escala/models.py:1817), com insumos de G01. Observar o estado publicado durante e após uma lavra. | A leitura persistida não apresenta veredito novo com status da lavra anterior: ambos pertencem ao mesmo save (app/docs/ADR/0002-celula-e-o-dia.md:44). Porta antes de existir lavra e ordem de atualização de outros artefatos: **regra nao decide**. | Gravar/publicar o veredito e só depois gravar o status em outro ato (app/docs/ADR/0002-celula-e-o-dia.md:44). | adversarial |
| A05 | Alterar ausência rejulga pelo outbox (app/docs/ADR/0002-celula-e-o-dia.md:16); aval literal do caso 3. | Entrada G03, porém legado com CelulaDia `ata={}`, `status_dia=None`, `status_rotulo=None` (app/escala/models.py:1778; app/escala/models.py:1816; app/escala/models.py:1817). Ausencia conserva `status='aprovada'` e período integral (app/ponto/models.py:799; app/ponto/models.py:750; app/ponto/models.py:751). | Depois de processado o evento de rejulgamento, vale G03: nenhuma porta para decidir novamente e palavra/efeito da ausência (aval do caso 3; app/docs/PROMPTS.md:53). Resposta antes de qualquer evento, migração automática e prazo de convergência: **regra nao decide**. | Fazer o caminho legado ignorar a ausência aprovada após o rejulgamento (app/docs/ADR/0002-celula-e-o-dia.md:16). | adversarial |
| A06 | Regeneração denuncia a alteração por campos próprios (CLAUDE.md:146); mudança de DNA rejulga pelo outbox (app/docs/ADR/0002-celula-e-o-dia.md:16); consumidor lê a lâmpada (CLAUDE.md:170). | Entrada G01 após primeira avaliação. CelulaDia muda `trabalha=False→True`, com `dna`, `regenerada_em`, `regeneracoes`, `dna_anterior` atualizados (app/escala/models.py:1686; app/escala/models.py:1697; app/escala/models.py:1718; app/escala/models.py:1723; app/escala/models.py:1726). | A regeneração fica identificável pelos campos de histórico e provoca novo julgamento dos insumos (CLAUDE.md:146; app/docs/ADR/0002-celula-e-o-dia.md:16). Consumidores leem a nova lâmpada (CLAUDE.md:170). Fechar, substituir ou conservar a porta anterior: **regra nao decide**. | Conservar indefinidamente o julgamento anterior apesar de mudar o DNA da célula (app/docs/ADR/0002-celula-e-o-dia.md:16). | adversarial |
| A07 | E-SEM-VIVA (B): desligado que nunca bateu segura a folha e vai à lista do admin com porta (app/docs/RELATO.md:60); recorte da competência em app/docs/BACKLOG.md:1799. | Colaborador `situacao='desligado'`, `data_demissao=31/10` (app/colaboradores/models.py:323; app/colaboradores/models.py:325). CelulaDia `colaborador=C`, `data=15/10`, `veredito='nunca_bateu'` (app/escala/models.py:1681; app/escala/models.py:1685; app/escala/models.py:1770). Zero Batida de C na vida (app/ponto/models.py:22). Empresa `dia_inicio_competencia=21` (app/colaboradores/models.py:46); competência retida como pré-condição do corte (app/docs/BACKLOG.md:1799). | O desligamento não exclui esse dia anterior à demissão: a folha continua retida e o admin encont

[truncado em 20000 de 23185 caracteres]

## 06/11 18:19 · sessao 6911586370aa · - · - · origem=Ronald (tmux)

- Tabuleiro: “EVENTO carimba e julga na hora”; consumidor “le a lampada, nunca re-julga”. `CLAUDE.md:168-170`.
- Idempotência explícita: “chamar 2x com a mesma entrada = mesmo estado, 1 linha de trilha, 0 chamado/pergunta duplicada”. `app/docs/ESTRUTURAL.md:22-25`.
- L-099 exige coerência, determinismo e idempotência com o cadastro existente; divergência tem dono ESTRUTURA/CADASTRO/BATIDA; os dois últimos vão à lista do admin. `CLAUDE.md:176`; `app/docs/ESTRUTURAL.md:29-39`.
- E-SEM-VIVA (B): desligado que nunca bateu segura a folha e vai à lista do admin com porta; recorte “dias antes da demissão em comp que ele segura”. `app/docs/RELATO.md:60`; `app/docs/PROMPTS.md:1900`; `app/docs/BACKLOG.md:1799`.
- Definição escrita de porta: “o dia em que a tela oferece ‘Resolver dia’”, lida de `resolver_dia_oferecido`; fio vivo pode ser pergunta viva **ou** essa ação. Assim, observar a oferta de “Resolver dia” tem lastro; exigir criação de chamado/pergunta em todo caso não decorre dessa regra. `app/docs/CORTES.md:271`.
- Dia decidido: efeito abona=verde; desconta=vermelho com horas; suprime=cinza; rejeitado=laranja; hover quem/quando; “Nunca tick sem palavra”; vale grade/cartão/app. `app/docs/PROMPTS.md:53`. L-039: falta significa ausência lançada cujo tipo desconta, decidida. `app/docs/LEIS.md:87`.
- Ausência própria via disputa: docstring expressa `falta -> aprovada` (“confissão contra interesse: nada a decidir”); atestado sem documento fica aguardando documento; chamada fecha somente se ausência nasce decidida. `app/chamados/services/materializacao.py:495-508`. Comentário de regras S4 explicita ausência aberta mantém chamado vivo e silenciado; FALTA_DIA aprovada nasce decidida. `app/chamados/services/materializacao.py:556-561`.
- Rejeitada nunca cobre o dia. `app/docs/PROMPTS.md:50`. Perguntas absorvidas voltam a pendente quando a ausência é rejeitada. `app/docs/PROMPTS.md:1377`; `app/docs/RELATO.md:1703`.
- Pendente não representa efeito futuro aprovado; é amarelo e “ninguém decidiu ainda”. `app/ponto/services/dia_decidido.py:37-40`.
- Competência padrão 21→20, configurável por empresa; nunca presumir mês civil ou corte 21 invariável. `CLAUDE.md:81-82`; `app/docs/JUIZES.md:75`.
- Folga de calendário pertence ao vínculo e só vale nos dias dele; folga manual pertence à pessoa e sobrevive à troca. Geradora da célula manda; sem ela, vigência. `app/docs/JUIZES.md:94-103`.
- Retratada sai do universo apurável. `CLAUDE.md:155`.
- Estado intermediário: status e veredito devem ser escritos no mesmo ato/save; DNA, vínculo, batidas, ausência e feriado alterados devem provocar rejulgamento por outbox. `app/docs/ADR/0002-celula-e-o-dia.md:12-18`; `app/docs/ADR/0002-celula-e-o-dia.md:44-49`.
- Concorrência explicitamente escrita para consumidor outbox: idempotente e `FOR UPDATE SKIP LOCKED`. `app/docs/CORTES.md:351`. Isolamento/ordenação específica entre duas ofertas simultâneas da porta: [não verificado].
- Aval do instrumento Specification by Example e formato da tabela: `app/docs/RELATO.md:6736-6737`; `app/docs/BACKLOG.md:1812`.

Lacunas procuradas:

- Aval `DIA-SEM-TRABALHO-PORTA` registrado em arquivo: não encontrado nas buscas por nome exato nos documentos/cópia; usar corte literal fornecido pelo usuário. [não verificado]
- Regra específica para dados legados sem ata/status que determine obrigatoriamente oferta ou recusa da porta: [não verificado].
- Regra específica para ordenação observável durante processamento ainda pendente do outbox: [não verificado].
- Manifesto da cópia contendo hash informado: não encontrado; `.git` ausente e busca pelo hash completo sem resultado. O nome do diretório corresponde ao hash fornecido, mas identificação de commit por conteúdo não foi verificável. [não verificado] [nao verificado]

## 06/11 18:18 · sessao cf2c3d5b1b40 · - · - · origem=Ronald (tmux)

Referências adicionais para consolidar:

- `FolgaDia`: vínculo `app/escala/models.py:1585`, data `:1588`, calendário de origem `:1589`, tipo `:1594`; definição declara “foto materializada” de folga rígida de vínculo/data (`app/escala/models.py:1582`).
- `DisputaSupervisao`: colaborador `app/chamados/models.py:1666`, contexto `:1675`, chamado `:1679`, fechamento `:1687`; campo próprio `status` [não verificado].
- `PerguntaDisputa`: célula `app/chamados/models.py:2209`, disputa `:2211`, motivo `:2216`, data `:2218`, tipo de resposta `:2222`, resposta do colaborador `:2227`, respondida em `:2228`, validada em `:2229`, pendência administrativa `:2317`, motivo da recusa `:2321`, via `:2322`.
- Ausência declarada na própria resposta: regra escrita diferencia falta aprovada por confissão e atestado submetido ao juiz de estado; ausência aberta segura o chamado; operação idempotente (`app/chamados/services/materializacao.py:495`).
- Estados de ausência: aguardando documento, aguardando decisão, aprovada, rejeitada (`app/ponto/models.py:701`).
- Item do calendário: data `app/colaboradores/services/calendario.py:722`, status/ausência `:726`, último recurso `:738`; regra literal exige último recurso + passado + status lavrado que pede decisão (`app/colaboradores/services/calendario.py:89`).
- Tipo de ciclo: `app/escala/models.py:117`; ciclos e conjunto de ciclos em que feriado vira folga `app/escala/models.py:26`.
- Situação e datas do colaborador: `app/colaboradores/models.py:323`, `:324`, `:325`.

Cuidados normativos encontrados:

- Vínculo do dia: carimbo da geradora manda, mas somente dentro da vigência; inatividade atual não invalida dia coberto; fora da vigência é `None` e dono CADASTRO, sem escolher outro vínculo (`app/escala/alimentacao.py:272`). Geradora ausente da carga também não autoriza escolher outro (`app/escala/alimentacao.py:264`).
- Feriado: município/posto do vínculo que responde pelo dia, não posto atual do colaborador (`app/escala/models.py:1382`).
- Batida retratada fica fora da geometria/apuração; continua exibida com asterisco (`app/ponto/models.py:64`).
- Estado intermediário: materializa, depois julga dentro de transação; reprovação desfaz tudo, e carimbo sem lastro não sai (`app/chamados/services/materializacao.py:18`).
- Dia decidido deve mostrar o que foi decidido pelo efeito do catálogo; nunca apenas tick (`app/ponto/services/dia_decidido.py:3`). Rejeitada não cobre folha nem suspende cobrança, mas deve aparecer para colaborador e administrador (`app/ponto/services/dia_decidido.py:11`). Pendente é amarelo, sem antecipar efeito favorável do tipo (`app/ponto/services/dia_decidido.py:37`).

## 06/11 18:15 · sessao 0c5a4b131c42 · E-SEM-VIVA-B · verificador · origem=Code

**Não identifiquei defeito funcional novo no delta.** A fronteira inferior ganhou prova, o selo cobre `**{...}` literal e a docstring passou a distinguir ausência de fechamento novo de fechamento preexistente. Permanecem limitações do selo e dois problemas do contador do vigia. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:278`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:334`, `app/escala/services/cadastro_realidade.py:148`, `app/folha/vigia_retido_calado.py:43`.

Execução da suíte Django, sobrevivência à suíte integral e ocorrência dos cenários em produção: **[nao verificado]**. [nao verificado]

1. **CURADO — o teste novo distingue os mutantes inferiores; os superiores sobrevivem a ele isoladamente.** [nao verificado]

   O filtro exige início da anterior inclusive e ontem inclusive. O fixture novo contém exatamente 20/09 e 21/09, exigindo somente 21/09. `app/escala/services/cadastro_realidade.py:176`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:279`.

   | Mutante simples | Teste novo isolado | Como provar | [nao verificado]
   |---|---|---|
   | Remover `data__gte` | **Morre:** devolve 20 e 21/09. | Fixture e expectativa em `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:279`. |
   | Trocar `gte` por `gt` | **Morre:** devolve vazio. | A única data permitida está exatamente no início inclusivo. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:280`. |
   | Usar início da atual | **Morre:** devolve vazio, pois o início passa a 21/10. | `app/escala/services/cadastro_realidade.py:174`, `app/ponto/janelas.py:27`. |
   | Remover `data__lte` | **Sobrevive:** continua devolvendo 21/09. | O fixture não contém hoje ou futuro. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:279`. |
   | Trocar `lte` por `lt` | **Sobrevive:** continua devolvendo 21/09. | O fixture não contém ontem. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:279`. |

   **No conjunto dos testes ativos, esses cinco mutantes simples são distinguíveis:** `_ativo` cria ontem, 24/10, e hoje; `ESPERADO` inclui ontem e exclui hoje. Assim, retirar o limite superior inclui hoje; torná-lo estrito perde ontem. Isso foi conferido pelos predicados em memória, sem executar Django. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:261`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:264`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:269`.

2. **BAIXA — o selo AST continua com pontos cegos plausíveis.**

   A cura reconhece expansão de dicionário **literal**; não resolve expressões, variáveis nem chamadas transitivas. As sondas abaixo executaram `_veredito` extraído do código, em memória. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:304`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:308`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:334`.

   | Forma | Resultado e causa |
   |---|---|
   | `Q(data_demissao__isnull=False)` | **Detecta:** examina kwargs de qualquer chamada. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:332`. |
   | `Q(**{"data_demissao__isnull": False})` | **Detecta:** cura do delta. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:334`. |
   | `Q(("data_demissao__isnull", False))` | **Não detecta:** não examina strings dentro da tupla posicional. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:329`. |
   | `kw={"data_demissao__isnull":False}; filter(**kw)` | **Não detecta:** exige `ast.Dict` diretamente na expansão. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:334`. |
   | `F("data_demissao")` | **Não detecta:** `F` está fora dos nomes examinados. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:296`. |
   | `values(dem=F("data_demissao"))` | **Não detecta:** o kwarg é `dem`; a referência dentro de `F` escapa. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:330`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:332`. |
   | `values("colaborador__data_demissao")` | **Não detecta:** compara a string posicional inteira, sem separar `__`. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:330`. |
   | `from operator import attrgetter as ag; ag("data_demissao")(c)` | **Não detecta campo:** o detector usa o nome sintático `ag`. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:328`. |
   | `from folha import export as e; e.classificar_export(...)` | **Escapa:** registra módulo `folha`, e chamadas por atributo não entram em `chamadas`. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:313`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:322`. |
   | `import folha.export as e` | **Bloqueia pelo módulo.** `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:317`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:343`. |
   | `from folha.export import classificar_export as e; e(...)` | **Detecta o apelido.** `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:315`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:322`. |
   | Helper externo lendo campo ou chamando juiz | **Não acompanha:** analisa somente o corpo selecionado de `colabs_nunca_bateu`. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:304`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:311`. |

   **Relevância:** kwargs em variável, `F()` e helpers são caminhos plausíveis para introduzir futuramente um predicado paralelo na seleção de desligados. Não demonstram recomposição no corpo atual: ele seleciona desligados por `situacao` e cruza dias com o retrato. `app/escala/services/cadastro_realidade.py:179`, `app/escala/services/cadastro_realidade.py:186`, `app/escala/services/cadastro_realidade.py:192`.

   **Como provar:** manter uma chamada descartada a `linhas_do_retrato` e acrescentar qualquer leitura cega acima; o requisito positivo continua satisfeito. O selo não prova fluxo até o retorno. Os testes sem retrato, divergência lavra/vivo e cadastro torto combatem alterações comportamentais específicas, mas não fecham todas essas formas. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:341`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:154`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:160`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:227`.

3. **SEM DIVERGÊNCIA DE CALENDÁRIO — lavrador e leitor concordam para o mesmo `hoje` e corte.**

   | Valor | Quem produz/escreve | Quem lê |
   |---|---|---|
   | Corte da empresa | Formulário valida/clampa; porta persiste. `app/colaboradores/views.py:495`, `app/colaboradores/views.py:523`, `app/colaboradores/services/estrutura.py:17`. | `corte_da_empresa`, com default 21 e clamp 2–28. `app/ponto/janelas.py:50`. |
   | Janela atual | `periodo_apuracao_atual` → `periodo_apuracao`. `app/ponto/janelas.py:24`. | `janela_atual`, usada por C5 e lavrador. `app/ponto/janelas.py:60`, `app/escala/services/cadastro_realidade.py:174`, `app/folha/management/commands/lavrar_retrato_folha.py:60`. |
   | Janela anterior | `janela_anterior`, perguntando pela janela do dia anterior ao início atual. `app/ponto/janelas.py:71`. | C5 lê `[0]` como limite inferior; C5 e lavrador leem `[1]` como competência. `app/escala/services/cadastro_realidade.py:176`, `app/escala/services/cadastro_realidade.py:183`, `app/folha/management/commands/lavrar_retrato_folha.py:61`. |
   | `(empresa, mes, ano)` do retrato | Lavrador usa mês/ano do **fim** de ambas as janelas e persiste. `app/folha/management/commands/lavrar_retrato_folha.py:61`, `app/folha/management/commands/lavrar_retrato_folha.py:74`. | C5 constrói os mesmos pares e consulta a empresa. `app/escala/services/cadastro_realidade.py:184`, `app/escala/services/cadastro_realidade.py:185`. |
   | Janela do conteúdo lavrado | `dias_que_seguram` usa `janela_fechamento`; export delega ao mesmo juiz. `app/folha/fora_da_folha.py:118`, `app/folha/export.py:47`. | Retentor recebe essas bordas; universo do fechamento também usa esse juiz. `app/folha/fora_da_folha.py:137`, `app/ponto/services/fechamento.py:423`. |

   **Como provar:** a sonda das funções reais percorreu todos os dias de 2024–2028 e cortes 2–28: 49.329 combinações, sem divergência de contenção, continuidade ou reconstrução da janela pelo mês/ano do fim. O código trata `dia >= corte` e dezembro→janeiro explicitamente. `app/ponto/janelas.py:15`, `app/ponto/janelas.py:27`, `app/ponto/janelas.py:30`, `app/ponto/janelas.py:71`.

   Exemplos, corte 21: em 20/10/2026 selecionam competências 09 e 10; em 21/10, 10 e 11; em 21/12, 12/2026 e 01/2027; em 01/01/2027 permanece esse último par. `app/ponto/janelas.py:24`, `app/ponto/janelas.py:66`.

   **Limite da conclusão:** isso não certifica contemporaneidade da lavra. O command usa data real ou competência explícita; C5 aceita `hoje` arbitrário e lê o retrato atualmente gravado, sem seleção histórica por `lavrado_em`. No corte, antes da nova lavra, uma competência pode ainda não ter retrato; o desligado fica sem esses dias, conforme a política implementada. `app/folha/management/commands/lavrar_retrato_folha.py:48`, `app/folha/management/commands/lavrar_retrato_folha.py:55`, `app/folha/management/commands/lavrar_retrato_folha.py:74`, `app/escala/services/cadastro_realidade.py:169`, `app/escala/services/cadastro_realidade.py:185`.

4. **MÉDIA — remanescente: o vigia perde competência e dias ao declarar roteamento.**

   **O que está errado:** transforma `{colaborador: datas}` em conjunto de IDs e aplica esse conjunto a todas as competências selecionadas. Uma C5 positiva somente na anterior pode retirar de `calados` a linha da corrente. A unidade pessoa é declarada, mas o resultado também é apresentado por competência. `app/folha/vigia_retido_calado.py:12`, `app/folha/vigia_retido_calado.py:43`, `app/folha/vigia_retido_calado.py:48`, `app/folha/vigia_retido_calado.py:54`.

   **Como provar:** sonda em memória do corpo real de `censo`, com dois retratos, mesmo colaborador e fio `ninguem`, mas C5 contendo somente 05/10: resultado `bloqueados=2`, `calados=0`, `roteados=2`, um roteado em cada competência. Isso comprova o mecanismo do contador, sem executar ORM. `app/folha/vigia_retido_calado.py:43`, `app/folha/vigia_retido_calado.py:48`.

   O cenário pode ser anterior com `nunca_bateu` e corrente retida por `sem_celula`, sem porta: o retentor produz esse segundo tipo de dia, enquanto C5 lê somente células `nunca_bateu`. Ocorrência em produção: **[nao verificado]**. `app/folha/export.py:1231`, `app/folha/fora_da_folha.py:67`, `app/folha/fora_da_folha.py:86`, `app/escala/services/cadastro_realidade.py:175`.

5. **MÉDIA — remanescente: “roteado à lista” pode referir empresa excluída da tela.**

   **O que está errado:** vigia e C5 aceitam retrato/empresa inativa; a tela e as próximas lavras exigem `empresas_visiveis`. Portanto, o contador pode declarar encaminhamento a uma lista que não exibe aquela empresa. `app/folha/vigia_retido_calado.py:33`, `app/escala/services/cadastro_realidade.py:171`, `app/escala/views.py:487`, `app/folha/management/commands/lavrar_retrato_folha.py:49`, `app/colaboradores/queries.py:85`.

   **Como provar:** lavrar empresa visível com C5 positiva, torná-la inativa e consultar `censo([id])`. O retrato permanece selecionável, mas a empresa sai da tela e do lavrador. A sonda do contador real com retrato de empresa inativa e C5 positiva devolveu `roteados=1`, `calados=0`, `cego=0`; o caminho completo em Django e sua ocorrência em produção são **[nao verificado]**. `app/folha/vigia_retido_calado.py:35`, `app/folha/vigia_retido_calado.py:48`, `app/folha/vigia_retido_calado.py:56`, `app/colaboradores/queries.py:85`.

6. **INFORMATIVO — resumo dos demais escritores/leitores e mudança desde a rodada 2.**

   | Campo/valor consumido ou produzido | Quem escreve | Quem lê |
   |---|---|---|
   | `Colaborador`: ID, empresa, situação | Criação; edição/transferência da empresa; desligamento/reativação da situação. `app/colaboradores/services/colaborador.py:91`, `app/colaboradores/views.py:1313`, `app/colaboradores/views.py:1386`, `app/colaboradores/services/desligamento.py:104`, `app/colaboradores/services/desligamento.py:210`. | C5 filtra empresa, identifica desligados e associa IDs. `app/escala/services/cadastro_realidade.py:175`, `app/escala/services/cadastro_realidade.py:179`. |
   | `CelulaDia`: colaborador/data e `veredito='nunca_bateu'` | Gerador cria identidade do dia; cartório decide o veredito pela porta de lavra. `app/ponto/portas/celula.py:633`, `app/ponto/services/cartorio.py:1241`, `app/ponto/portas/celula.py:814`. | C5 lê veredito, empresa, datas e ID. `app/escala/services/cadastro_realidade.py:175`. |
   | Retrato: `empresa`, `mes`, `ano`, `linhas`, `lavrado_em` | `lavrar_retrato_folha --apply`. `app/folha/management/commands/lavrar_retrato_folha.py:74`. | C5 seleciona chave; vigia seleciona último dia de lavra e relata competências; porta lê JSON. `app/escala/services/cadastro_realidade.py:185`, `app/folha/vigia_retido_calado.py:38`, `app/folha/vigia_retido_calado.py:50`, `app/folha/retrato_folha.py:110`. |
   | JSON: `colab`, `status`, `motivo` | Export decide status/motivo; montador copia ID e valores. `app/folha/export.py:962`, `app/folha/retrato_folha.py:1178`. | `_balde` interpreta status/motivo; C5 exige `furo_espelho`; vigia lê bloqueados. `app/folha/retrato_folha.py:89`, `app/escala/services/cadastro_realidade.py:189`, `app/folha/vigia_retido_calado.py:46`. |
   | JSON: `datas` de furo | Retentor → `dias_que_seguram` → conversão ISO → montador. `app/folha/fora_da_folha.py:98`, `app/folha/fora_da_folha.py:128`, `app/folha/fora_da_folha.py:192`, `app/folha/retrato_folha.py:1183`. | C5 une datas e compara com `d.isoformat()`; recorte temporal também as lê. `app/escala/services/cadastro_realidade.py:190`, `app/escala/services/cadastro_realidade.py:192`, `app/folha/retrato_folha.py:1242`. |
   | JSON: `dono`, contendo fio | `_fio_cru`, chamado pelo montador. `app/folha/retrato_folha.py:364`, `app/folha/retrato_folha.py:1186`. | `fio_da_linha`, usado pelo vigia para selecionar `ninguem`. `app/folha/retrato_folha.py:428`, `app/folha/vigia_retido_calado.py:47`. |
   | `{cid: datas}` → assinatura `C5` e URL | C5 monta retorno; formatador monta assinatura; `acao` fornece URL. `app/escala/services/cadastro_realidade.py:194`, `app/escala/services/cadastro_realidade.py:202`, `app/escala/services/cadastro_realidade.py:657`. | `lista` incorpora assinatura; vigia reduz retorno a IDs. `app/escala/services/cadastro_realidade.py:833`, `app/folha/vigia_retido_calado.py:43`. |
   | `hoje`, contadores, `cego` e alarmes | Command fornece data; `censo` calcula resultados em memória. `app/folha/management/commands/vigiar_retido_calado.py:31`, `app/folha/vigia_retido_calado.py:44`, `app/folha/vigia_retido_calado.py:56`. | C5 calcula ontem/janelas; formatador e command imprimem/avaliam alarme. `app/escala/services/cadastro_realidade.py:170`, `app/folha/vigia_retido_calado.py:60`, `app/folha/management/commands/vigiar_retido_calado.py:36`. |

   Nos testes, os escritores são os fixtures de empresa/colaborador/célula, `_fechar`, o command real e os retratos artificiais dos negativos; essas gravações pertencem ao código de teste. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:39`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:53`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:66`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:69`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:196`.

   **Desde a rodada 2:** mudaram a prova inferior, o reconhecimento AST do dicionário literal e a explicação documental. A ressalva sobre desligado sem data agora concorda com o caminho do código: ele não ganha fechamento novo pelo universo, mas fechamento antigo continua sendo lido pelo export; C5 segue a lavra. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:278`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:334`, `app/escala/services/cadastro_realidade.py:148`, `app/ponto/services/fechamento.py:430`, `app/folha/export.py:775`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:227`.

## 06/11 18:14 · sessao 40b4c38a7809 · ESPELHO-DECIDIU · sonda · origem=Code

O ponto central é: **o valor de falta gravado soma as ausências aprovadas que descontam, mesmo havendo abono sobreposto**; já a cobertura usada para abono/supressão escolhe uma ausência por dia. São operações diferentes. (`app/folha/export.py:140`, `app/folha/export.py:142`, `app/ponto/services/fechamento.py:674`)

1) Transcrição do dado fornecido. `[nao verificado]` indica ausência de verificação independente na sombra; os números abaixo são os do texto recebido. [nao verificado]

```text
UNIVERSO dias_colab_com_ausencia_aprovada=5278 dias_com_2+=33 colabs=5 [nao verificado]
PARES_DE_TIPO {('falta', 'folga_abonada'): 1, ('falta', 'folga'): 1, ('ferias', 'ferias'): 29, ('atestado', 'falta'): 1, ('falta', 'folga_compensatoria'): 1} [nao verificado]
POR_MES {'2026-10': 3, '2026-09': 30} [nao verificado]
CLASSE (tipos, fechamento) {('so_abono', 'nao_desconta'): 29, ('abono+falta', 'desconta'): 4} [nao verificado]
ABONO+FALTA_QUE_DESCONTA 4 [nao verificado]
ERROS {} [nao verificado]
CONCLUSAO_LIBERADA [nao verificado]
rc_sombra=0 [nao verificado]
```

Os oito `EX` estão abaixo; as tuplas de `aus` foram mantidas sem atribuir nomes aos seus campos. A coluna `tela` resume os campos completos visíveis no trecho fornecido. [nao verificado]

| Colaborador / dia | `aus` | `horas_falta` / `falta_min` | `tela`: tipo / faixa / rótulo; suprimidos; previsto | Fonte |
|---|---|---|---|---|
| col127 / 2026-09-03 | `(4709, 'ferias', 0, False, None, '2026-10-05 11:55')`; `(2634, 'ferias', 0, False, None, '2026-08-13 11:44')` | `0.0 / None` | `ferias / ferias / Ferias`; `['ferias','folga']`; `False` | [nao verificado] |
| col127 / 2026-09-04 | `(4710, 'ferias', 0, False, None, '2026-10-05 11:55')`; `(2634, 'ferias', 0, False, None, '2026-08-13 11:44')` | `0.0 / None` | `ferias / ferias / Ferias`; `['ferias','trabalho']`; `True` | [nao verificado] |
| col127 / 2026-09-05 | `(4711, 'ferias', 0, False, None, '2026-10-05 11:55')`; `(2634, 'ferias', 0, False, None, '2026-08-13 11:44')` | `0.0 / None` | `ferias / ferias / Ferias`; `['ferias','folga']`; `False` | [nao verificado] |
| col127 / 2026-09-06 | `(4712, 'ferias', 0, False, None, '2026-10-05 11:55')`; `(2634, 'ferias', 0, False, None, '2026-08-13 11:44')` | `0.0 / None` | `ferias / ferias / Ferias`; `['ferias','trabalho']`; `True` | [nao verificado] |
| col824 / 2026-09-07 | `(4519, 'folga_compensatoria', 0, False, None, '2026-09-28 16:34')`; `(4421, 'falta', 660, False, None, '2026-09-24 18:04')` | `11.0 / 660.0` | `falta / atestado / Atestado`; `['folga','trabalho']`; `True` | [nao verificado] |
| col643 / 2026-09-17 | `(4431, 'falta', 720, False, None, '2026-09-25 13:04')`; `(4434, 'atestado', 0, False, None, '2026-09-25 13:07')` | `12.0 / 720.0` | `falta / atestado / Atestado`; `['atestado','trabalho']`; `True` | [nao verificado] |
| col736 / 2026-10-04 | `(5146, 'folga', 0, False, None, '2026-10-23 17:14')`; `(5168, 'falta', 720, False, 28450, '2026-10-23 19:05')` | `12.0 / 720.0` | `falta / atestado / Atestado`; `['atestado','trabalho']`; `True` | [nao verificado] |
| col444 / 2026-10-10 | `(4830, 'falta', 720, False, 30118, '2026-10-12 18:33')`; `(5098, 'folga_abonada', 0, False, None, '2026-10-22 18:02')` | `12.0 / 720.0` | `falta / atestado / Atestado`; `['atestado','trabalho']`; `True` | [nao verificado] |

Nos quatro exemplos de férias, `fatos.ausencias=['ferias','ferias']`; nos demais, respectivamente, `['falta','folga_compensatoria']`, `['falta','atestado']`, `['falta','folga']` e `['falta','folga_abonada']`. O feriado de col824 aparece truncado como `'Independência do Bra…'`; os demais exemplos mostram `feriado=None`. Os textos de `tela` recebidos estão truncados. [nao verificado]

| C444: pk | Dia | Tipo | min | Status | pelo_proprio | chamado_origem | horas_falta / falta_min | Fonte |
|---|---|---|---:|---|---|---:|---|---|
| 5198 | 2026-10-24 | falta | 720 | aprovada | False | 34480 | `12.0 / 720.0` | [nao verificado] |
| 5099 | 2026-10-16 | falta | 420 | aprovada | False | None | `7.0 / 420.0` | [nao verificado] |
| 4830 | 2026-10-10 | falta | 720 | aprovada | False | 30118 | `12.0 / 720.0` | [nao verificado] |
| 5098 | 2026-10-10 | folga_abonada | 0 | aprovada | False | None | `12.0 / 720.0` | [nao verificado] |

2) Pelo código, há três operações que precisam ser distinguidas.

- **Cobertura para abono/supressão:** o fechamento chama `cobertura_ausencia_periodo(..., para='folha')`; só entram aprovadas. A consulta ordena por `data, pk` crescentes e sobrescreve o mapa: vence **maior data inicial; empatando, maior pk**. Não usa o instante de criação ou aprovação. (`app/ponto/services/fechamento.py:674`, `app/ponto/turnos.py:2514`, `app/ponto/turnos.py:2757`, `app/ponto/turnos.py:2769`)
- **Efeito da vencedora:** no fechamento, ABONA acrescenta o dia às justificadas; DESCONTA não o suprime; os demais efeitos entram nas suprimidas. Ausências pagas parciais são ressalvadas antes. `efeitos_da_ausencia` também consulta a **mesma função de cobertura**, com outro laço para aplicar ABONA/SUPRIME. (`app/ponto/services/fechamento.py:685`, `app/ponto/services/fechamento.py:689`, `app/ponto/services/fechamento.py:691`, `app/ponto/services/efeito_ausencia.py:53`, `app/ponto/services/efeito_ausencia.py:59`)
- **Quantidade de falta gravada:** vem de `minutos_de_falta_por_dia`, que percorre **todas** as aprovadas dos tipos `descontam_vigentes()` e acumula os minutos por dia. Não consulta a vencedora da cobertura nem elimina a falta pela presença de abono. Cada registro é rateado pelos seus dias; `DiaPago` recebe minutos/60 em `horas_falta` e os minutos em `detalhe.falta_min`. (`app/ponto/services/fechamento.py:864`, `app/folha/export.py:88`, `app/folha/export.py:118`, `app/folha/export.py:121`, `app/folha/export.py:140`, `app/folha/export.py:142`, `app/ponto/services/dia_pago.py:272`)

A expansão de dias usa intervalo fechado; sem fim, alcança o fim consultado somente para `TIPOS_EM_ABERTO`; nos demais tipos cobre apenas a data inicial, sempre recortada à janela. (`app/ponto/turnos.py:2779`, `app/ponto/turnos.py:2785`)

**A saída `fatos_do_dia` da sonda usa outra escolha:** delega a `fatos_do_periodo`, inclui não rejeitadas e ordena inicialmente apenas por `data` crescente. Depois escolhe a primeira candidata pela faixa: `ferias`, `afastamento`, `atestado`, `feriado`, `folga`, `trabalho`. Na mesma faixa, a data mais antiga chega primeiro; mesma faixa e mesma data não têm desempate explícito por pk. (`app/ponto/precedencia.py:256`, `app/ponto/precedencia.py:273`, `app/ponto/precedencia.py:279`, `app/ponto/precedencia.py:46`, `app/ponto/precedencia.py:396`)

`falta`, `folga` e `folga_abonada` caem na faixa padrão `atestado`; `folga_compensatoria` tem faixa explícita `folga`. O código devolve o tipo vencedor, mas monta o rótulo pela faixa: isso permite `tipo_do_dia='falta'`, `faixa='atestado'`, `rotulo='Atestado'`. (`app/ponto/precedencia.py:52`, `app/ponto/precedencia.py:79`, `app/ponto/precedencia.py:403`, `app/ponto/precedencia.py:405`)

**A palavra final do espelho tem outra etapa:** o espelho carrega a cobertura de folha pela mesma `cobertura_ausencia_periodo` usada pelo fechamento e a passa a `dia_decidido.do_dia`. Quando existe cobertura, ela tem prioridade sobre os fatos A2, e o rótulo vem daquela ausência. (`app/ponto/services/espelho.py:588`, `app/ponto/services/espelho.py:340`, `app/ponto/services/dia_decidido.py:363`, `app/ponto/services/dia_decidido.py:428`)

Portanto:

| Comparação | Resposta pelo código |
|---|---|
| Cobertura do fechamento × cobertura da palavra final do espelho | **Mesma função:** `cobertura_ausencia_periodo`. (`app/ponto/services/fechamento.py:674`, `app/ponto/services/espelho.py:588`) |
| Cobertura × `fatos_do_dia` mostrado na sonda | **Funções e critérios diferentes:** maior `(data,pk)` versus faixa de precedência e ordem por data. (`app/ponto/turnos.py:2757`, `app/ponto/turnos.py:2769`, `app/ponto/precedencia.py:279`, `app/ponto/precedencia.py:396`) |
| Desconto gravado × essas escolhas | **Soma independente** das aprovadas que descontam. (`app/folha/export.py:140`, `app/folha/export.py:142`) |
| `decididos_do_periodo` | Outra função: inclui rejeitadas e sobrescreve por `(data,pk)`; não substitui a cobertura quando esta existe. (`app/ponto/services/dia_decidido.py:550`, `app/ponto/services/dia_decidido.py:555`, `app/ponto/services/dia_decidido.py:561`, `app/ponto/services/dia_decidido.py:363`) |

O recorte não mostra `data/data_fim` de cada registro nem a palavra final de `dia_decidido`; o pk vencedor da cobertura e essa palavra nos exemplos permanecem [nao verificado].

3) Escritores operacionais de `Ausencia.minutos` encontrados na cópia:

| Sítio | O que escreve ou encaminha |
|---|---|
| Campo do modelo | `default=0`. (`app/ponto/models.py:755`) |
| `Ausencia.save` | Deriva **somente falta aprovada com minutos falsy**; soma `minutos_previstos_do_dia` pelo intervalo e atribui o total. Valores positivos são preservados. (`app/ponto/models.py:877`, `app/ponto/models.py:885`, `app/ponto/models.py:887`) |
| `criar_ausencia` | Argumento com default zero → `kwargs['minutos']` → instância → save. (`app/ponto/services/ausencia.py:130`, `app/ponto/services/ausencia.py:343`, `app/ponto/services/ausencia.py:385`, `app/ponto/services/ausencia.py:418`) |
| `editar_ausencia_registro` | Atribui `int(minutos or 0)` e salva. (`app/ponto/services/ausencia.py:508`) |
| `mudar_estado` / `decidir` | Persiste `minutos` em `update_fields`, incluindo a derivação do modelo. (`app/ponto/services/ausencia.py:834`, `app/ponto/services/ausencia.py:911`) |
| `rederivar_fim` / comando de cura | Faz save completo, podendo persistir a derivação. (`app/ponto/services/ausencia.py:1296`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:46`) |
| Lançamento admin | Encaminha minutos recebidos da entrada. (`app/ponto/views.py:2388`, `app/ponto/views.py:2417`) |
| Submissão PWA | Encaminha `_min` recebido da entrada. (`app/ponto/views.py:2497`, `app/ponto/views.py:2539`) |
| API | Converte a entrada e encaminha `_min`. (`app/api/views.py:1815`, `app/api/views.py:1824`) |
| Edição pela tela | Lê POST, converte e passa à edição central. (`app/ponto/views.py:2784`, `app/ponto/views.py:2787`, `app/ponto/views.py:2801`) |
| Copiloto | Omite minutos; recebe default zero. (`app/ponto/services/checar_ausencia.py:46`, `app/ponto/services/ausencia.py:130`) |
| Veredito da célula | Omite minutos; recebe default zero. (`app/ponto/services/veredito_celula.py:315`, `app/ponto/services/ausencia.py:130`) |
| Declaração de pergunta de disputa | Omite minutos; recebe default zero. (`app/chamados/services/materializacao.py:484`, `app/ponto/services/ausencia.py:130`) |
| Materialização da resposta de disputa | Omite minutos; recebe default zero. (`app/chamados/services/materializacao.py:520`, `app/ponto/services/ausencia.py:130`) |
| Sincronização de férias | Passa `minutos=0`. (`app/ponto/services/ausencia.py:1204`) |
| `garantir_ausencia` | Encaminha os argumentos à criação central. (`app/ponto/services/ausencia.py:461`) |
| Regularização | Passa `minutos=0` ao wrapper. (`app/ponto/services/regularizacao.py:119`) |
| Abandono provisório | Passa `minutos=0` ao wrapper. (`app/ponto/management/commands/marcar_abandono_provisorio.py:80`) |
| Comando H06 | Passa `minutos=alvo` à edição: aplicação rederiva; reversão restaura o valor anterior do snapshot. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:127`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169`) |

**Nenhum desses produtores operacionais embute atualmente `720` ou outro literal positivo para minutos.** As duas materializações omitem o argumento; os comentários registram a retirada do valor fixo. Entradas explícitas e valores de reversão continuam podendo persistir números positivos, que `save` preserva. (`app/chamados/services/materializacao.py:484`, `app/chamados/services/materializacao.py:487`, `app/chamados/services/materializacao.py:520`, `app/chamados/services/materializacao.py:522`, `app/ponto/services/ausencia.py:508`, `app/ponto/models.py:877`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:170`)

Há literais como `720` e atualizações diretas em fixtures de testes. (`app/ponto/tests/test_h12h06b_rederivar_minutos.py:71`, `app/ponto/tests/test_h12h06b_rederivar_minutos.py:190`) O formulário é `forms.Form`, e o admin de Ausencia herda de `SoLeitura`. (`app/ponto/forms.py:41`, `app/ponto/admin.py:38`)

Quem escreveu os `720`, `660` e `420` dos registros concretos, e com qual versão do código, permanece [nao verificado].

4) **Existe veto de sobreposição, com uma exceção assimétrica:** `linha_que_veta` percorre as sobrepostas não rejeitadas e retorna a primeira não absolvida. `abona_sobre_falta` absolve somente **tipo novo que abona sobre registro existente de tipo `falta`**. Falta nova sobre abono não recebe essa exceção. (`app/ponto/turnos.py:2815`, `app/ponto/precedencia.py:145`, `app/ponto/precedencia.py:127`, `app/ponto/precedencia.py:131`)

| Caminho de criação | Passagem pelo veto |
|---|---|
| Admin, PWA, copiloto e veredito da célula | Usam o modo padrão, com `linha_que_veta` e veto final de precedência. (`app/ponto/views.py:2415`, `app/ponto/views.py:2536`, `app/ponto/services/checar_ausencia.py:46`, `app/ponto/services/veredito_celula.py:315`, `app/ponto/services/ausencia.py:323`, `app/ponto/services/ausencia.py:377`) |
| API | Mesmo fato vivo é devolvido; para novo fato, muda para modo `veto`. (`app/api/views.py:1826`, `app/ponto/services/ausencia.py:246`, `app/ponto/services/ausencia.py:253`) |
| Sincronização de férias | Usa criação padrão e registra as recusas. (`app/ponto/services/ausencia.py:1202`, `app/ponto/services/ausencia.py:1209`) |
| Regularização e abandono | Usam `retornar`: havendo sobreposição, devolvem a existente sem criar outra; não executam o veto final reservado ao modo `veto`. (`app/ponto/services/regularizacao.py:118`, `app/ponto/management/commands/marcar_abandono_provisorio.py:76`, `app/ponto/services/ausencia.py:461`, `app/ponto/services/ausencia.py:312`, `app/ponto/services/ausencia.py:377`) |
| As duas materializações de disputa | **Não passam pelo veto entre tipos diferentes:** usam `retornar_mesmo_tipo`. (`app/chamados/services/materializacao.py:490`, `app/chamados/services/materializacao.py:526`) |

Nesse último modo, o produtor procura somente mesmo colaborador/tipo/data; se não encontrar, define `conflito=None` e sobrepostas vazias. Também pula o veto final, condicionado a `se_existir=='veto'`. (`app/ponto/services/ausencia.py:294`, `app/ponto/services/ausencia.py:302`, `app/ponto/services/ausencia.py:377`)

Outros escritores que não passam por `linha_que_veta`:

- **Aprovação posterior:** o gate verifica documento/contradição, e `decidir → mudar_estado` verifica a transição e salva; não reconsulta sobreposição nem substitui a falta anterior. (`app/ponto/services/ausencia.py:619`, `app/ponto/services/ausencia.py:628`, `app/ponto/services/ausencia.py:911`, `app/ponto/services/ausencia.py:820`, `app/ponto/services/ausencia.py:835`)
- **Edição:** não usa essa exceção; tem veto próprio mais restrito, recusando qualquer outra sobreposta. (`app/ponto/services/ausencia.py:487`, `app/ponto/services/ausencia.py:491`)
- **Reversão de rejeição:** chama `_reabrir`, que muda o estado sem consultar esse veto. (`app/ponto/services/ausencia.py:1102`, `app/ponto/services/ausencia.py:1141`)
- **Rederivação e modelo:** `rederivar_fim` chama `save`; o `save` deriva carimbo, minutos e intervalo, sem veto de sobreposição. A constraint declarada limita apenas duplicidade viva de **colaborador/tipo/data inicial**, permitindo tipos diferentes e interseções de intervalos com inícios distintos. (`app/ponto/services/ausencia.py:1296`, `app/ponto/models.py:868`, `app/ponto/models.py:877`, `app/ponto/models.py:894`, `app/ponto/models.py:827`)

O dado raspado não identifica qual desses caminhos produziu cada sobreposição; atribuir os casos concretos a um escritor específico permanece [nao verificado].

## 06/11 18:13 · sessao 1875a7bbf072 · - · - · origem=Ronald (tmux)

**MÉDIA — remanescente: vigia perde competência e datas ao decidir roteamento.** `na_lista` conserva somente IDs, e `rot` verifica somente `colab`; assim, C5 positiva na anterior pode retirar da conta de calados a linha da corrente sem nenhum dia corrente na C5. A unidade declarada é pessoa, mas o relatório apresenta o número por competência. `app/folha/vigia_retido_calado.py:12`, `app/folha/vigia_retido_calado.py:43`, `app/folha/vigia_retido_calado.py:48`, `app/folha/vigia_retido_calado.py:54`.

Como provar: sonda executada em memória com o corpo real de `censo` extraído por AST, sem Django/DB: dois retratos, competências 10 e 11, mesmo `colab=7`, `dono='ninguem'`; C5 devolvendo somente `05/10`. Resultado observado: `bloqueados=2`, `calados=0`, `roteados=2`, um roteado por competência. O mecanismo responsável são as linhas 43 e 48; ocorrência em produção **[nao verificado]**. `app/folha/vigia_retido_calado.py:43`, `app/folha/vigia_retido_calado.py:48`.

Cenário real plausível: anterior retida por `nunca_bateu`; corrente retida por `sem_celula`, sem porta. O retentor produz dias sem célula, cuja ausência de porta conduz a grupo D; entretanto, C5 consulta somente células `nunca_bateu`. `app/folha/export.py:1231`, `app/folha/fora_da_folha.py:67`, `app/folha/fora_da_folha.py:86`, `app/escala/services/cadastro_realidade.py:175`.

**MÉDIA — remanescente: “roteado à lista” pode apontar empresa que a tela exclui.** O contador lê retratos históricos independentemente da atividade da empresa; C5 também consulta todas as empresas ou os IDs informados. A tela e o lavrador usam `empresas_visiveis`, que exige `ativa=True` e exclui CNPJ diagnóstico. `app/folha/vigia_retido_calado.py:33`, `app/folha/vigia_retido_calado.py:38`, `app/escala/services/cadastro_realidade.py:171`, `app/escala/views.py:487`, `app/folha/management/commands/lavrar_retrato_folha.py:49`, `app/colaboradores/queries.py:85`.

Como provar: lavrar retrato com C5 positiva, depois tornar a empresa inativa e consultar `censo([id])`; o retrato continua no universo do contador, mas a empresa sai da tela e das próximas lavras. Sonda com o corpo real de `censo` em memória, retrato associado a `empresa.ativa=False` e C5 positiva retornou `bloqueados=1`, `calados=0`, `roteados=1`, `cego=0`. A sonda comprova a ausência de filtro no contador; ocorrência real em produção **[nao verificado]**. `app/folha/vigia_retido_calado.py:35`, `app/folha/vigia_retido_calado.py:48`, `app/escala/services/cadastro_realidade.py:185`, `app/escala/views.py:487`.

**Mapa dos campos da lavra consumidos pelo diff:**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `status='fora'`, `motivo='furo_espelho'` | `classificar_export`, sobre fechamentos da empresa/competência. `app/folha/export.py:775`, `app/folha/export.py:962` | `montar_linhas` copia; `_balde` interpreta; C5 exige motivo; vigia lê balde. `app/folha/retrato_folha.py:1164`, `app/folha/retrato_folha.py:1180`, `app/folha/retrato_folha.py:89`, `app/escala/services/cadastro_realidade.py:189`, `app/folha/vigia_retido_calado.py:46` |
| `datas` de furo | Retentor → `dias_que_seguram`, somente para itens de furo; `fora_da_folha` converte datas ISO. `app/folha/fora_da_folha.py:95`, `app/folha/fora_da_folha.py:128`, `app/folha/fora_da_folha.py:137`, `app/folha/fora_da_folha.py:192` | `montar_linhas` copia; C5 une e compara ISO. `app/folha/retrato_folha.py:1171`, `app/folha/retrato_folha.py:1183`, `app/escala/services/cadastro_realidade.py:190`, `app/escala/services/cadastro_realidade.py:192` |
| `colab` | ID vindo do item do export, copiado no JSON. `app/folha/retrato_folha.py:1163`, `app/folha/retrato_folha.py:1178` | C5 e vigia. `app/escala/services/cadastro_realidade.py:189`, `app/folha/vigia_retido_calado.py:48` |
| `dono` contendo fio | `_fio_cru`, chamado pelo montador. `app/folha/retrato_folha.py:364`, `app/folha/retrato_folha.py:1186` | `fio_da_linha`, usado pelo vigia para selecionar `ninguem`. `app/folha/retrato_folha.py:428`, `app/folha/retrato_folha.py:438`, `app/folha/vigia_retido_calado.py:47` |
| `empresa`, `mes`, `ano`, `linhas`, `lavrado_em` | Command `lavrar_retrato_folha --apply`. `app/folha/management/commands/lavrar_retrato_folha.py:74` | C5 seleciona empresa/competências; vigia seleciona empresa e último dia de lavra, ordena e relata competência; porta lê `linhas`. `app/escala/services/cadastro_realidade.py:185`, `app/folha/vigia_retido_calado.py:35`, `app/folha/vigia_retido_calado.py:38`, `app/folha/vigia_retido_calado.py:45`, `app/folha/retrato_folha.py:110` |
| Retorno C5 `{cid:[datas]}` | Função C5, apenas em memória. `app/escala/services/cadastro_realidade.py:194` | `_nunca_bateu` formata assinatura; `lista` incorpora; `acao` fornece porta; vigia reduz a IDs. `app/escala/services/cadastro_realidade.py:201`, `app/escala/services/cadastro_realidade.py:833`, `app/escala/services/cadastro_realidade.py:657`, `app/folha/vigia_retido_calado.py:43` |
| `hoje` do vigia | Command lê `timezone.localdate()`. `app/folha/management/commands/vigiar_retido_calado.py:31` | `censo` repassa à C5, que calcula ontem e janelas. `app/folha/vigia_retido_calado.py:43`, `app/escala/services/cadastro_realidade.py:170`, `app/escala/services/cadastro_realidade.py:174` |
| Contadores e `cego` | `censo`, em memória. `app/folha/vigia_retido_calado.py:44`, `app/folha/vigia_retido_calado.py:49`, `app/folha/vigia_retido_calado.py:56` | `linhas` imprime; command avalia alarme. `app/folha/vigia_retido_calado.py:60`, `app/folha/management/commands/vigiar_retido_calado.py:32`, `app/folha/management/commands/vigiar_retido_calado.py:36` |

Nenhum defeito adicional de execução identificado nesta revisão do delta; execução da suíte Django **[nao verificado]**. [nao verificado]

## 06/11 18:12 · sessao 0307bae4437f · - · - · origem=Ronald (tmux)

**Sem divergência aritmética entre lavrador e leitor para o mesmo `hoje` e corte.** Ambos derivam `(mes, ano)` do fim das mesmas duas janelas; C5 usa o início da anterior como limite inferior. `app/folha/management/commands/lavrar_retrato_folha.py:60`, `app/escala/services/cadastro_realidade.py:174`, `app/escala/services/cadastro_realidade.py:176`, `app/escala/services/cadastro_realidade.py:183`.

Sonda em memória, com as funções reais, conferiu 49.329 combinações: todos os dias de 2024–2028, cortes 2–28; nenhuma divergência de contenção, continuidade ou reconstrução por competência. A implementação inclui `dia >= corte`, dezembro→janeiro e anterior por dia imediatamente antes do início corrente. `app/ponto/janelas.py:15`, `app/ponto/janelas.py:27`, `app/ponto/janelas.py:30`, `app/ponto/janelas.py:71`.

| Valor | Quem escreve/produz | Quem lê |
|---|---|---|
| `dia_inicio_competencia` | Formulário valida/clampa e chama a porta; porta persiste. `app/colaboradores/views.py:495`, `app/colaboradores/views.py:523`, `app/colaboradores/views.py:538`, `app/colaboradores/services/estrutura.py:12`, `app/colaboradores/services/estrutura.py:17`. | `corte_da_empresa`; default 21 e clamp 2–28. `app/ponto/janelas.py:50`. |
| Janela corrente | `periodo_apuracao_atual` → `periodo_apuracao`. `app/ponto/janelas.py:24`. | `janela_atual`; C5 e lavrador. `app/ponto/janelas.py:60`, `app/escala/services/cadastro_realidade.py:174`, `app/folha/management/commands/lavrar_retrato_folha.py:60`. |
| Janela anterior | `janela_anterior` chama corrente no dia anterior ao início. `app/ponto/janelas.py:71`. | C5 lê início; C5/lavrador leem fim para competência. `app/escala/services/cadastro_realidade.py:176`, `app/escala/services/cadastro_realidade.py:183`, `app/folha/management/commands/lavrar_retrato_folha.py:61`. |
| Competência de `RetratoFolha` | Lavrador grava `(empresa, mes, ano)` do fim. `app/folha/management/commands/lavrar_retrato_folha.py:74`. | C5 consulta mesmo par. `app/escala/services/cadastro_realidade.py:185`. |
| Janela do conteúdo lavrado | `montar_linhas` → `fora_da_folha` → `dias_que_seguram`, que chama `janela_fechamento` e export. `app/folha/retrato_folha.py:1157`, `app/folha/fora_da_folha.py:118`. | Export delega ao mesmo juiz; retentor recebe essas bordas. `app/folha/export.py:47`, `app/folha/export.py:807`, `app/folha/fora_da_folha.py:137`. |
| Janela do fechamento | Juiz `janela_fechamento`. `app/ponto/janelas.py:54`. | Universo e recalculador. `app/ponto/services/fechamento.py:423`, `app/ponto/services/fechamento.py:545`, `app/ponto/services/fechamento.py:612`. |

Exemplos corte 21, calculados pelas funções reais: em 20/10/2026, competências 09/10; em 21/10, 10/11; em 21/12, 12/2026 e 01/2027; em 01/01/2027, mesmo par; em 21/01, 01/02 de 2027. `app/ponto/janelas.py:24`, `app/ponto/janelas.py:66`.

**Limitação — concordância de calendário não certifica contemporaneidade.** Lavrador fixa a data real, aceita competências explícitas e sobrescreve o único retrato da chave; C5 aceita `hoje` arbitrário e consulta o retrato existente sem seleção por `lavrado_em`. Portanto, `hoje` retroativo não recupera uma fotografia histórica. `app/folha/management/commands/lavrar_retrato_folha.py:48`, `app/folha/management/commands/lavrar_retrato_folha.py:55`, `app/folha/management/commands/lavrar_retrato_folha.py:74`, `app/folha/models.py:363`, `app/escala/services/cadastro_realidade.py:169`, `app/escala/services/cadastro_realidade.py:185`.

O vigia seleciona retratos pela última **data de lavra**, enquanto C5 seleciona competências pelo `hoje` recebido; esses universos podem divergir, inclusive antes da nova lavra no dia do corte. A C5 então deixa o desligado sem dias da competência ainda não lavrada. `app/folha/vigia_retido_calado.py:38`, `app/folha/vigia_retido_calado.py:43`, `app/escala/services/cadastro_realidade.py:181`, `app/escala/services/cadastro_realidade.py:190`. Ocorrência em produção: **[nao verificado]**.

## 06/11 18:12 · sessao 800b74a61b49 · - · - · origem=Ronald (tmux)

**Q1 — cura comprovada por sonda em memória; execução Django [nao verificado].** O filtro está em `app/escala/services/cadastro_realidade.py:176`; o teste novo está em `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:278`.

| Mutante simples | Teste novo isolado | Prova | [nao verificado]
|---|---|---|
| Remover `data__gte` | Distingue | Retorna 20/09 e 21/09, exigia só 21/09. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:279` |
| `gte` → `gt` | Distingue | Retorna vazio. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:280` |
| Usar início de `janela_atual` | Distingue | Retorna vazio; início atual 21/10. `app/escala/services/cadastro_realidade.py:174` |
| Remover `data__lte` | Sobrevive | Ambas as datas do fixture são anteriores a ontem. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:279` |
| `lte` → `lt` | Sobrevive | Fixture não contém ontem. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:279` |

Os dois mutantes superiores são distinguidos pelos testes ativos já existentes: `_ativo` cria HOJE e ontem=24/10, e `ESPERADO` inclui 24/10 e exclui HOJE. Logo os testes desse arquivo, em conjunto, distinguem todos os mutantes simples listados; sobrevivência à suíte integral [nao verificado]. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:261`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:264`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:269`.

**Q2 — BAIXA, lacunas remanescentes do selo; nenhum defeito funcional atual demonstrado.** Extraí e executei `_veredito` em memória, sem importar Django; ele percorre somente a função localizada por `_fn` e reconhece strings posicionais apenas para os nomes enumerados. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:304`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:296`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:328`.

| Forma sondada | Resultado do detector |
|---|---|
| `Q(data_demissao__isnull=False)` | Detecta `data_demissao`; kwargs de qualquer chamada são examinados. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:332` |
| `Q(**{"data_demissao__isnull": False})` | Detecta; cura do delta funciona. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:334` |
| `Q(("data_demissao__isnull", False))` | Não detecta; não percorre strings de tuplas posicionais. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:329` |
| `kw={"data_demissao__isnull":False}; Q(**kw)` | Não detecta; exige `ast.Dict` diretamente no kwarg. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:334` |
| `F("data_demissao")` | Não detecta; `F` não pertence à lista. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:296` |
| `values(d=F("data_demissao"))` | Não detecta; inspeciona o nome `d`, não o campo dentro de `F`. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:330`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:332` |
| `values("colaborador__data_demissao")` | Não detecta; string posicional comparada inteira, sem dividir `__`. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:330` |
| `from operator import attrgetter as ag; ag("data_demissao")(c)` | Não detecta campo; usa nome sintático `ag`, sem resolver o apelido para o detector de campos. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:315`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:328` |
| `from folha import export as e; e.classificar_export(...)` | Não detecta proibida nem módulo proibido: registra `folha`, e chamadas por atributo não entram em `chamadas`. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:313`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:322` |
| `import folha.export as e; e.classificar_export(...)` | Bloqueia pelo módulo, mesmo sem reconhecer chamada. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:317`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:343` |
| `from folha.export import classificar_export as e; e(...)` | Detecta pelo mapa de apelidos. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:315`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:322` |
| Helper externo que consulta campo/juiz | Não segue corpo externo: `_fn` entrega somente o AST de `colabs_nunca_bateu`. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:304`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:311` |

**Relevância:** esses pontos cegos permitem introduzir futuramente predicado paralelo na consulta de desligados ou mover a recomposição para helper mantendo o selo verde; não provam que isso ocorre no corpo atual, que consulta somente situação e consome dias do retrato. O teste de cadastro torto combate especificamente a exclusão por demissão nula, e os testes sem retrato/divergência combatem recomposição simples; o selo continua sem provar fluxo dos dados da chamada até o retorno. `app/escala/services/cadastro_realidade.py:179`, `app/escala/services/cadastro_realidade.py:186`, `app/escala/services/cadastro_realidade.py:192`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:227`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:154`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:160`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:341`.

## 06/11 18:12 · sessao 6edb84ac9a89 · - · - · origem=Ronald (tmux)

Inventário operacional de `Ausencia.minutos` na cópia:

| Sítio | Valor / efeito |
|---|---|
| `app/ponto/models.py:755` | Campo com `default=0`. |
| `app/ponto/models.py:877` | `save` deriva **somente** se `status=='aprovada'`, `tipo=='falta'` e minutos falsy; soma `minutos_previstos_do_dia` no range e atribui em `:887`. Minutos positivos permanecem. |
| `app/ponto/services/ausencia.py:130` | `criar_ausencia(..., minutos=0)`. Repassa o argumento ao `kwargs` em `:343`, instancia em `:385` e grava em `:418`. |
| `app/ponto/services/ausencia.py:508` | `editar_ausencia_registro`: `ausencia.minutos=int(minutos or 0)`; `save()` em `:509`. |
| `app/ponto/services/ausencia.py:834` | `mudar_estado` inclui `minutos` em `update_fields`; grava em `:835`, permitindo persistir a derivação do modelo. `decidir` chama essa porta em `:911`. |
| `app/ponto/services/ausencia.py:1296` | `rederivar_fim` faz `save()` completo e pode, portanto, persistir a derivação do modelo. Seu chamador operacional é `app/ponto/management/commands/curar_ausencia_sem_fim.py:46`. |
| `app/ponto/views.py:2415` | Lançamento admin: `minutos=minutos` em `:2417`, obtido do POST pelo parser em `:2388`; inválido degrada a zero em `:2390`. |
| `app/ponto/views.py:2536` | Submissão PWA: `minutos=_min` em `:2539`, obtido do POST em `:2497`. |
| `app/api/views.py:1820` | API: `minutos=_min` em `:1824`, obtido de `request.data` em `:1815`. |
| `app/ponto/views.py:2794` | Edição da tela passa `minutos=minutos` em `:2801`, lendo POST em `:2784` e convertendo em `:2787`. |
| `app/ponto/services/checar_ausencia.py:46` | Copiloto chama criação aprovada sem argumento minutos; recebe default zero de `app/ponto/services/ausencia.py:130`. |
| `app/ponto/services/veredito_celula.py:315` | Veredito cria aprovada sem argumento minutos; recebe default zero de `app/ponto/services/ausencia.py:130`. |
| `app/chamados/services/materializacao.py:484` | Declaração de pergunta: omite minutos; recebe default zero de `app/ponto/services/ausencia.py:130`. |
| `app/chamados/services/materializacao.py:520` | Ausência da resposta: omite minutos; recebe default zero de `app/ponto/services/ausencia.py:130`. |
| `app/ponto/services/ausencia.py:1202` | Sincronização de férias passa `minutos=0` em `:1204`. |
| `app/ponto/services/ausencia.py:453` | Wrapper `garantir_ausencia(**kw)` repassa todos os argumentos a `criar_ausencia` em `:461`. |
| `app/ponto/services/regularizacao.py:118` | Chama esse wrapper com `minutos=0` em `:119`. |
| `app/ponto/management/commands/marcar_abandono_provisorio.py:76` | Chama o wrapper com `minutos=0` em `:80`. |
| `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169` | Chama edição com `minutos=alvo` em `:170`; aplicação usa `alvo=None` no plano em `:244`; reversão usa os minutos anteriores do snapshot em `:241`. |

**Nenhum desses escritores operacionais conserva literal não zero, como `720`.** Os dois sítios de materialização atuais omitem minutos, com a retirada do literal documentada em `app/chamados/services/materializacao.py:487` e `:522`; os demais passam zero, argumento do usuário, valor encaminhado ou snapshot, conforme tabela. A varredura AST de produção desta leitura percorreu a raiz, inclusive arquivos fora de `app`, sem outro construtor/escritor operacional encontrado; essa conclusão de completude é `[nao verificado]` quanto a mecanismos dinâmicos que a busca estática não revela.

**Testes ainda contêm números fixos**, por exemplo `720` em `app/ponto/tests/test_h12h06b_rederivar_minutos.py:71`, `:73`, `:337`, `:349`; também há atualizações diretas de fixture em `:190`, `:204`, `:301`. O selo que procura literais não zero na criação operacional está em `app/chamados/tests/test_h06_selo_minutos_sem_literal.py:34`, com exclusão explícita de testes e migrations em `:55`; não o executei.

**Formulário/admin:** `AusenciaForm` herda de `forms.Form`, em `app/ponto/forms.py:41`, e não grava como `ModelForm`. O admin herda de `SoLeitura`, em `app/ponto/admin.py:38`; o mixin recusa adicionar/alterar/apagar em `app/core/admin.py:59`, `:62`, `:65` e torna os campos somente leitura em `:68`.

**Limite temporal:** os `720` do dado fornecido não identificam quem escreveu cada registro nem demonstram execução do código atual; autoria histórica individual permanece `[nao verificado]`.

## 06/11 18:11 · sessao 6c411e286cf9 · - · - · origem=Ronald (tmux)

A porta existe, mas não é aplicada em todos os caminhos; estar no produtor único não significa passar pelo veto. O construtor está em `app/ponto/services/ausencia.py:385`, e o veto final só roda quando `se_existir == 'veto'`, em `app/ponto/services/ausencia.py:377`.

**Regra exata da porta**

- `ausencias_sobrepostas` consulta ausências **não rejeitadas**, portanto inclui aprovadas e abertas; considera intervalo fechado, início dentro da janela e intervalo aberto somente nos tipos autorizados, ordenando por `data, pk`. `app/ponto/turnos.py:2815`, `app/ponto/turnos.py:2819`.
- `linha_que_veta` percorre todas e devolve a primeira que `abona_sobre_falta` não absolver. `app/ponto/precedencia.py:145`.
- A exceção é assimétrica: só absolve quando a existente tem `tipo == 'falta'` e o tipo novo pertence a `abonam_vigentes()`. Logo, abono novo sobre falta pode entrar; falta nova sobre abono não recebe essa exceção. `app/ponto/precedencia.py:127`, `app/ponto/precedencia.py:131`.
- No criar padrão, `linha_que_veta` é consultada em `app/ponto/services/ausencia.py:323`; havendo veto, levanta `AusenciaInvalida` em `app/ponto/services/ausencia.py:326`. O veto final também chama esse juiz, em `app/ponto/precedencia.py:461`.
- A criação pode nascer diretamente aprovada após essas verificações: `status` vai para os argumentos em `app/ponto/services/ausencia.py:344`; carimbos da aprovação são preenchidos em `app/ponto/services/ausencia.py:347`.

**Criadores e comportamento**

| Caminho | Referência | Passa pelo veto de sobreposição? |
|---|---|---|
| Lançamento admin, já aprovado | `app/ponto/views.py:2415`, `app/ponto/views.py:2420` | Sim, modo padrão |
| Submissão PWA | `app/ponto/views.py:2536` | Sim, modo padrão |
| API app | `app/api/views.py:1820`, `app/api/views.py:1826` | Novo fato: sim; mesmo fato vivo é devolvido antes, sem criar (`app/ponto/services/ausencia.py:246`) |
| Copiloto, já aprovado | `app/ponto/services/checar_ausencia.py:46` | Sim, modo padrão |
| Veredito da célula, já aprovado | `app/ponto/services/veredito_celula.py:315` | Sim, modo padrão |
| Espelhamento de férias, já aprovado | `app/ponto/services/ausencia.py:1202` | Sim, modo padrão; recusas são registradas em `app/ponto/services/ausencia.py:1209` e encaminhadas à pauta em `app/ponto/services/ausencia.py:1215` |
| Regularização | `app/ponto/services/regularizacao.py:118` | Usa `garantir_ausencia` → `retornar`; conflito devolve existente e não cria (`app/ponto/services/ausencia.py:461`, `app/ponto/services/ausencia.py:312`) |
| Abandono provisório | `app/ponto/management/commands/marcar_abandono_provisorio.py:76` | Mesmo comportamento `retornar` |
| Declaração validada de disputa | `app/chamados/services/materializacao.py:484`, `app/chamados/services/materializacao.py:490` | **Não**: `retornar_mesmo_tipo` |
| Materialização da resposta de disputa | `app/chamados/services/materializacao.py:520`, `app/chamados/services/materializacao.py:526` | **Não**: `retornar_mesmo_tipo`; recebe `status` em `app/chamados/services/materializacao.py:525` |

O desvio das duas materializações é executável: o modo `retornar_mesmo_tipo` só procura mesmo colaborador/tipo/data; se não achar, define `conflito = None` e `_sobrepostas = none()`, evitando `linha_que_veta`. Depois evita também o veto final porque o modo não é `veto`. `app/ponto/services/ausencia.py:294`, `app/ponto/services/ausencia.py:302`, `app/ponto/services/ausencia.py:377`.

**Aprovação posterior não consulta essa porta**

- Aprovação em lote usa `gate_aprovacao_ausencia` e chama `_a4_decidir`. `app/ponto/views.py:2283`, `app/ponto/views.py:2290`.
- Aprovação individual também usa esse gate e aprova os irmãos via `_a4_decidir`. `app/ponto/views.py:2607`, `app/ponto/views.py:2643`.
- Esse gate verifica contradição documento/desconto e aprovação sem documento; não verifica sobreposição. `app/ponto/services/ausencia.py:619`, `app/ponto/services/ausencia.py:628`.
- `_a4_decidir` delega a `decidir`, que chama `mudar_estado`; esta verifica a transição, atribui o status e salva, sem `linha_que_veta` ou `veto_de_lancamento`. `app/ponto/views.py:85`, `app/ponto/services/ausencia.py:911`, `app/ponto/services/ausencia.py:820`, `app/ponto/services/ausencia.py:824`, `app/ponto/services/ausencia.py:835`.
- Portanto, o abono que entrou como aberto sobre uma falta pode ser aprovado mantendo ambas as linhas aprovadas: a criação aceita a exceção, e a aprovação não rejeita/substitui a falta nem consulta colisão. `app/ponto/precedencia.py:127`, `app/ponto/services/ausencia.py:911`, `app/ponto/services/ausencia.py:824`.

**Outros escritores sem esse juiz**

- `editar_ausencia_registro` não usa `linha_que_veta`; tem veto próprio mais restrito: qualquer `ausencia_sobrepoe`, excluindo a própria, recusa a edição. `app/ponto/services/ausencia.py:487`, `app/ponto/services/ausencia.py:491`.
- `reverter_rejeicao` chama `_reabrir` diretamente; `_reabrir` muda para estado aberto sem consulta de sobreposição. `app/ponto/services/ausencia.py:1102`, `app/ponto/services/ausencia.py:1141`.
- `rederivar_fim` simplesmente chama o `save`; o comando de cura usa esse escritor. `app/ponto/services/ausencia.py:1296`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:46`.
- `Ausencia.save` deriva carimbo/minutos/range, sem veto de sobreposição. `app/ponto/models.py:868`, `app/ponto/models.py:877`, `app/ponto/models.py:894`.
- A constraint declarada impede apenas duplicidade viva de **colaborador/tipo/data inicial**, não interseção de intervalos nem dois tipos diferentes no mesmo dia. `app/ponto/models.py:827`.
- Admin Django de Ausencia está em modo `SoLeitura`. `app/ponto/admin.py:38`.

O dado raspado não identifica o caminho de escrita que produziu cada registro; atribuir os casos concretos a algum desses escritores é `[nao verificado]`.

## 06/11 18:11 · sessao b8bae2adde14 · - · - · origem=Ronald (tmux)

O fechamento tem dois caminhos distintos:

- **Abono/supressão e motor:** `fechamento.py` chama `cobertura_ausencia_periodo(..., para='folha')` e aplica o efeito da vencedora; ABONA justifica, DESCONTA conserva o previsto, outros efeitos suprimem. Referências: `app/ponto/services/fechamento.py:671-692`. `efeitos_da_ausencia` usa a mesma cobertura para justificadas/suprimidas, embora some créditos parciais num laço sobre todas. Referências: `app/ponto/services/efeito_ausencia.py:47-62`.
- **Horas de falta gravadas:** NÃO usa somente aquela vencedora. `fechamento.py:864-866` chama `folha.export.minutos_de_falta_por_dia`; essa função percorre **todas** as aprovadas dos tipos `descontam_vigentes()` e soma minutos por dia, sem testar se outro abono cobre o mesmo dia. Referências: `app/folha/export.py:87-92`, `app/folha/export.py:138-143`. Rateia cada registro pelo número integral de dias: `app/folha/export.py:113-122`; a expansão delega a `dias_da_ausencia`: `app/folha/export.py:63-70`. O mapa alimenta a lavratura em `app/ponto/services/fechamento.py:1189`, `app/ponto/services/fechamento.py:1265`; `DiaPago.horas_falta = minutos/60` e `detalhe.falta_min = round(minutos,2)` em `app/ponto/services/dia_pago.py:272-275`.

**Critério da cobertura:** para folha só aprovada (`app/ponto/turnos.py:2514-2515`); ordena por `data, pk` crescentes e sobrescreve o mapa, vencendo maior `data`, depois maior `pk`. Referências: `app/ponto/turnos.py:2753-2769`. Isso não é ordem de `criado_em` ou `aprovada_em`; os campos efetivamente usados são `data,pk` (`app/ponto/turnos.py:2757`). A consulta singular `ausencia_cobre` implementa o mesmo critério com `order_by('-data','-pk').first()` (`app/ponto/turnos.py:2552-2557`), mas é outra função.

**Cobertura de dias:** fechado usa `data_fim`; sem fim, tipos `TIPOS_EM_ABERTO` alcançam o fim consultado; demais cobrem apenas `data`, limitado à janela. Referências: `app/ponto/turnos.py:2760-2770`, `app/ponto/turnos.py:2779-2789`.

**A2 / `fatos_do_dia` que aparece em `tela` na sonda:**

- Delega a `fatos_do_periodo`: `app/ponto/precedencia.py:256`.
- Inclui todas não rejeitadas, inclusive pendentes; ordenação da consulta somente por `data` crescente: `app/ponto/precedencia.py:273-279`.
- Monta as candidatas e ordena pela faixa, escolhendo a primeira: `app/ponto/precedencia.py:379-397`. Ordem: férias, afastamento, atestado, feriado, folga, trabalho (`app/ponto/precedencia.py:45-46`).
- Dentro da mesma faixa conserva a ordem recebida; data mais antiga chega antes. Entre candidatas com mesma data e faixa, não há desempate explícito por `pk` (`app/ponto/precedencia.py:279`, `app/ponto/precedencia.py:396-397`); um vencedor exato nesses empates é `[nao verificado]`.
- `falta`, `folga`, `folga_abonada` não estão no mapa especial e caem na faixa padrão `atestado`; `folga_compensatoria` está explicitamente na faixa `folga`. Referências: `app/ponto/precedencia.py:52-64`, `app/ponto/precedencia.py:79`. O `tipo_do_dia` conserva o código, mas `rotulo` vem da faixa; por isso código `falta` pode aparecer com faixa `atestado` e rótulo `Atestado`. Referências: `app/ponto/precedencia.py:66-68`, `app/ponto/precedencia.py:403-405`.

**Espelho completo / `dia_decidido`: cuidado para não igualar à saída A2 da sonda.**

- A montagem efetiva do espelho carrega cobertura de folha pela **mesma função de `turnos`**: `app/ponto/services/espelho.py:588`; carrega A2 e decisões separadamente: `app/ponto/services/espelho.py:646-649`.
- Encaminha a cobertura a `do_dia`: `app/ponto/services/espelho.py:331-342`.
- `dia_decidido.veredito_do_dia` prioriza a cobertura, antes dos fatos A2: `app/ponto/services/dia_decidido.py:363-364`; `do_dia` usa `rotulo_de(cobertura)` quando ela existe: `app/ponto/services/dia_decidido.py:428-429`.
- `rotulo_de` usa o efeito vigente e os minutos daquela ausência: `app/ponto/services/dia_decidido.py:468-481`, `app/ponto/services/dia_decidido.py:491-497`; para desconto monta “... (desconta ...h)” (`app/ponto/services/dia_decidido.py:219-221`).
- `decididos_do_periodo`, outro mapa para histórico/recusas, também ordena `data,pk` e sobrescreve; inclui rejeitadas e usa `data_fim or data`, sem estender tipos abertos até o fim consultado. Referências: `app/ponto/services/dia_decidido.py:547-562`.

Portanto, **A2 e cobertura são funções diferentes com critérios diferentes**; **palavra final do espelho e cobertura usada pelo motor compartilham `cobertura_ausencia_periodo`**; **valor de falta gravado soma todas as aprovadas que descontam**, independentemente das duas escolhas. Referências nos pontos acima. O trecho `tela` fornecido não contém a palavra final de `dia_decidido`; o resultado real dessa palavra nos exemplos é `[nao verificado]`.

As tuplas EX não mostram `data/data_fim` de cada ausência; inferir qual `pk` ganha cobertura usando somente o timestamp exibido seria `[nao verificado]`.

## 06/11 18:11 · sessao 23b0f53d0635 · ESPURIA · sonda · origem=Code

**Não é possível certificar integralmente a sonda nem explicar o cadastro do PAR com este material:** não localizei `sonda_espuria2.py` na cópia, e a saída fornecida contém `PAR_ACHADOS {}`. [nao verificado]

1. **Leituras e eventual repetição de regra**

A correção das chaves está de acordo com a porta real: `rubricas_do_dia` devolve `extras_50` e `extras_100`, lendo os totais do `DiaPago`; o espelho chama essa função. [app/ponto/services/dia_pago.py:912](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/services/dia_pago.py:912), [app/ponto/services/espelho.py:813](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/services/espelho.py:813)

Os pontos que precisam ser conferidos no código da sonda são:

| Número | Fonte real / regra que seria repetida |
|---|---|
| `he` | A porta já fornece `minutos_extra = (extras_50 + extras_100) * 60`. Somar os dois totais novamente repete essa composição, embora não reapure os períodos. [app/ponto/services/dia_pago.py:925](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/services/dia_pago.py:925) |
| `he_fer`, `folga_trab` | São campos próprios do `DiaPago`; lê-los diretamente não exige reconstruir o enquadramento. [app/ponto/models.py:331](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/models.py:331), [app/ponto/models.py:333](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/models.py:333) |
| `pago` | A autoridade é `pago_do_dia`: verifica sinal nas rubricas e soma trabalhadas, folga trabalhada e `folga_feriado_min/60`. Copiar essa expressão ou seu teste de ausência seria repetir regra. [app/ponto/services/dia_pago.py:888](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/services/dia_pago.py:888) |
| `n_bat` | `batidas_apuraveis` aplica o filtro de retratação, colaborador e janela inclusiva. A atribuição ao dia da jornada é outra porta: `dia_das_batidas`. Agrupar tudo apenas pela data civil pode divergir dela. [app/ponto/turnos.py:2863](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/turnos.py:2863), [app/ponto/turnos.py:2908](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/turnos.py:2908) |
| `min_real` | Há duas origens a distinguir: o campo persistido do `DiaPago`, alimentado pelo mapa da grade, e o relógio do espelho, produzido por `realizado_do_dia` com vínculo e intervalo do dia. [app/ponto/services/dia_pago.py:286](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/services/dia_pago.py:286), [app/ponto/services/espelho.py:286](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/services/espelho.py:286), [app/ponto/services/espelho.py:783](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/services/espelho.py:783) |

**Arquivo:linha de repetição dentro da própria sonda: [nao verificado].** O cabeçalho e os resultados fornecidos não permitem confirmar quais dessas expressões ela executou. [nao verificado]

`sem_pago` também não significa necessariamente “sem lavratura”: `pago_do_dia` retorna `None` para linha ausente **ou** sem sinal monetário; `rubricas_do_dia` distingue ausência por `lavrada=False`. [app/ponto/services/dia_pago.py:888](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/services/dia_pago.py:888), [app/ponto/services/dia_pago.py:908](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/services/dia_pago.py:908)

2. **Saída fornecida, em tabelas**

Os registros abaixo são transcrição do dado raspado; não houve confirmação independente em banco ou log. [nao verificado]

`S2_DIN sem_trab`:

| pago | HE | fer | FT | dia | batidas | relógio | n | Evidência |
|---|---|---|---|---|---|---|---:|---|
| sem_pago | he0 | fer0 | ft0 | feriado | bat=2+ | real0 | 16 | [nao verificado] |
| sem_pago | he0 | fer0 | ft0 | util | bat=1 | real0 | 22 | [nao verificado] |
| pago | he0 | fer0 | ft0 | util | bat=1 | real0 | 32 | [nao verificado] |
| pago | he0 | fer0 | ft0 | feriado | bat=2+ | real0 | 15 | [nao verificado] |
| pago | he0 | fer0 | ft>0 | util | bat=2+ | real0 | 1 | [nao verificado] |
| sem_pago | he0 | fer0 | ft0 | util | bat=2+ | real0 | 36 | [nao verificado] |
| pago | he0 | fer0 | ft0 | util | bat=2+ | real0 | 43 | [nao verificado] |
| pago | he0 | fer0 | ft0 | feriado | bat=1 | real0 | 2 | [nao verificado] |
| sem_pago | he0 | fer0 | ft0 | feriado | bat=1 | real0 | 2 | [nao verificado] |
| pago | he0 | fer0 | ft0 | util | bat=2+ | real>0 | 1 | [nao verificado] |

`S2_ESTR_DIN n=72 com_2+_batidas=50 com_relogio>0=0`. [nao verificado]

Nos oito `S2_ESTR_DIN_EX`, são comuns: `pago=None`, `trabalhadas=0.0`, `he=0.0`, `he_fer=0.0`, `folga_trab=0.0`, `fer_min=0.0`. [nao verificado]

| col | dia | status | feriado | fer_vira_folga | trab_feriado | n_bat | n_orfas | min_real | lavrada | dono | Evidência |
|---|---|---|---|---|---|---:|---:|---|---|---|---|
| col43 | 2026-11-02 | folga_trabalhada | True | True | False | 2 | 2 | 0.0 | True | CADASTRO | [nao verificado] |
| col97 | 2026-11-02 | folga_trabalhada | False | None | None | 1 | 1 | 0.0 | True | CADASTRO | [nao verificado] |
| col97 | 2026-11-03 | escala_suspeita | False | None | None | 1 | 1 | None | False | CADASTRO | [nao verificado] |
| col97 | 2026-11-05 | escala_suspeita | False | None | None | 1 | 1 | 0.0 | True | CADASTRO | [nao verificado] |
| col207 | 2026-11-02 | folga_trabalhada | True | True | False | 2 | 2 | 0.0 | True | ESTRUTURA | [nao verificado] |
| col227 | 2026-11-03 | folga_trabalhada | False | False | True | 4 | 1 | None | False | BATIDA | [nao verificado] |
| col242 | 2026-11-02 | folga_trabalhada | True | True | False | 4 | 4 | 0.0 | True | ESTRUTURA | [nao verificado] |
| col270 | 2026-10-22 | folga_trabalhada | False | False | True | 4 | 4 | 0.0 | True | ESTRUTURA | [nao verificado] |

Portanto, `com_relogio>0=0` não deve ser reescrito como “todos têm relógio medido em zero”: os exemplos incluem `min_real=None`. [nao verificado]

Para apresentar todas as `S2_D0211` sem repetir campos, uso estas abreviações da transcrição. [nao verificado]

- Perfil **A**: `feriado=True`, `trab_feriado=False`, `fer_vira_folga=True`; **B**: `True, True, False`; **C**: `False, None, None`. [nao verificado]
- `ST` corresponde a `sem_trabalho`; `FT` a `folga_trabalhada`; donos `CAD/EST/BAT` correspondem a `CADASTRO/ESTRUTURA/BATIDA`. [nao verificado]
- Em todas essas linhas: `folga_trab=0.0`, `fer_min=0.0`; `trabalhadas` coincide com `pago` quando este é numérico, e vale `0.0` quando `pago=None`. Na coluna `he_fer`, **=pago** conserva o número integral da coluna `pago`. [nao verificado]

| col | ST | status | perfil | ec | n_bat | min_real | pago | he | he_fer | dono | Evidência |
|---|---|---|---|---:|---:|---:|---|---:|---|---|---|
| col43 | True | FT | A | 1411 | 2 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col97 | True | FT | C | None | 1 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col203 | True | FT | A | 1409 | 4 | 0.0 | 7.069715555555556 | 0.0 | 0.0 | CAD | [nao verificado] |
| col207 | True | FT | A | 1452 | 2 | 0.0 | None | 0.0 | 0.0 | EST | [nao verificado] |
| col227 | False | irregular | B | 195 | 1 | 533.0 | 8.893322777777776 | 0.0 | 0.0 | BAT | [nao verificado] |
| col242 | True | FT | A | 1398 | 4 | 0.0 | None | 0.0 | 0.0 | EST | [nao verificado] |
| col278 | True | FT | A | 1350 | 4 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col282 | False | ok | B | 247 | 2 | 659.0 | 10.971358055555555 | 0.0 | 0.0 | EST | [nao verificado] |
| col296 | False | irregular | B | 261 | 0 | 345.0 | None | 0.0 | 0.0 | EST | [nao verificado] |
| col300 | False | irregular | B | 1459 | 1 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col366 | True | FT | B | 1165 | 1 | 0.0 | 10.998848333333335 | 0.0 | 0.0 | EST | [nao verificado] |
| col369 | True | FT | A | 1313 | 4 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col371 | False | ok | B | 324 | 4 | 416.0 | 5.010742500000001 | 5.01 | =pago | CAD | [nao verificado] |
| col384 | True | FT | A | 1347 | 4 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col451 | False | irregular | B | 1485 | 1 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col476 | True | FT | A | 1451 | 4 | 0.0 | 7.448265966111111 | 0.0 | 0.0 | EST | [nao verificado] |
| col569 | False | ok | B | 484 | 4 | 406.0 | 6.652301132499999 | 6.65 | =pago | CAD | [nao verificado] |
| col583 | True | FT | A | 1287 | 4 | 0.0 | 8.078643055555556 | 0.0 | 0.0 | CAD | [nao verificado] |
| col617 | True | FT | A | 1313 | 4 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col622 | True | FT | A | 1251 | 4 | 0.0 | 6.974738382777777 | 0.0 | 0.0 | CAD | [nao verificado] |
| col655 | False | irregular | B | 1275 | 2 | 240.0 | 2.305491111111111 | 2.31 | =pago | CAD | [nao verificado] |
| col707 | False | irregular | B | 1465 | 4 | 611.0 | 11.193095 | 0.0 | 0.0 | CAD | [nao verificado] |
| col784 | True | FT | A | 1458 | 2 | 0.0 | 4.133950277777777 | 0.0 | 0.0 | EST | [nao verificado] |
| col788 | True | FT | B | 1301 | 8 | 0.0 | 16.0160475 | 0.0 | 0.0 | BAT | [nao verificado] |
| col789 | True | FT | A | 1472 | 4 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col820 | True | FT | A | 1380 | 1 | 0.0 | None | 0.0 | 0.0 | BAT | [nao verificado] |
| col822 | True | FT | A | 1360 | 4 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col864 | True | FT | A | 1494 | 4 | 0.0 | None | 0.0 | 0.0 | EST | [nao verificado] |
| col866 | True | aberto | C | None | 1 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col877 | False | irregular | B | 1337 | 4 | 660.0 | 10.165167222222221 | 0.0 | 0.0 | CAD | [nao verificado] |
| col932 | False | irregular | B | 1335 | 2 | 716.0 | 11.936174444444445 | 0.0 | 0.0 | CAD | [nao verificado] |
| col944 | True | FT | A | 1348 | 3 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col961 | True | FT | A | 1348 | 4 | 0.0 | 7.146785555555555 | 0.0 | 0.0 | CAD | [nao verificado] |
| col962 | True | FT | A | 1303 | 2 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col967 | True | FT | A | 1316 | 4 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col968 | True | FT | A | 1358 | 4 | 0.0 | 6.9958141666666664 | 0.0 | 0.0 | EST | [nao verificado] |
| col979 | False | irregular | B | 1390 | 1 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col982 | False | irregular | B | 1394 | 1 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col985 | True | FT | A | 1418 | 2 | 0.0 | 4.017282222222223 | 0.0 | 0.0 | CAD | [nao verificado] |
| col986 | True | FT | B | 1419 | 4 | 0.0 | 11.057805930555556 | 0.0 | 0.0 | CAD | [nao verificado] |
| col988 | True | FT | A | 1422 | 4 | 0.0 | 7.342193096111111 | 0.0 | 0.0 | CAD | [nao verificado] |
| col990 | True | FT | A | 1424 | 4 | 0.0 | 7.317387678888889 | 0.0 | 0.0 | CAD | [nao verificado] |
| col991 | True | FT | A | 1425 | 4 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col998 | True | FT | B | 1450 | 4 | 0.0 | 10.450366262777777 | 0.0 | 0.0 | CAD | [nao verificado] |
| col1003 | True | FT | A | 1469 | 2 | 0.0 | None | 0.0 | 0.0 | EST | [nao verificado] |
| col60 | False | irregular | B | 1490 | 4 | 656.0 | 10.932136944444444 | 0.0 | 0.0 | CAD | [nao verificado] |
| col87 | False | ok | B | 802 | 4 | 445.0 | 4.435254722222222 | 4.44 | =pago | CAD | [nao verificado] |
| col114 | False | ok | B | 1036 | 4 | 439.0 | 6.332564722222222 | 6.33 | =pago | CAD | [nao verificado] |
| col129 | False | irregular | B | 853 | 2 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col502 | False | ok | B | 1150 | 4 | 440.0 | 4.812576055277777 | 4.81 | =pago | CAD | [nao verificado] |
| col749 | True | FT | A | 1374 | 4 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col868 | True | FT | B | 1377 | 1 | 0.0 | 11.870235555555555 | 0.0 | 0.0 | CAD | [nao verificado] |
| col900 | True | FT | A | 1341 | 1 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col996 | True | FT | A | 1446 | 4 | 0.0 | 13.019503333333333 | 0.0 | 0.0 | CAD | [nao verificado] |
| col1001 | True | FT | A | 1467 | 4 | 0.0 | 7.38249611111111 | 0.0 | 0.0 | CAD | [nao verificado] |

`PAR_ACHADOS {}`. [nao verificado]  
`ERROS {}`. [nao verificado]

3. **PAR: diferença cadastral que explica HE feriado 0 × 2,31**

O lado com `he=2.31` aparece como **col655**, com `he_fer=2.305491111111111`, `ec=1275`, `trab_feriado=True` e `fer_vira_folga=False`. O integrante com HE zero não foi identificado por `PAR_ACHADOS`; o anexo tampouco fornece escala efetiva/ciclo ou régua do col655. Assim, a diferença concreta de cadastro entre os dois é **[nao verificado]**.

O código comprova estes caminhos de transformação:

| Cadastro/condição | Transformação em rubrica |
|---|---|
| Vínculo do dia | `escala_vigente` delega a escolha a `vinculo_do_dia`, incluindo a geradora da célula quando existente. [app/escala/servico_jornada.py:38](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/escala/servico_jornada.py:38) |
| `trabalha_em_feriado` e `feriado_vira_folga` | Se trabalha em feriado, o predicado retorna falso; caso contrário, exige ciclo permitido e feriado no lugar do vínculo. [app/escala/models.py:1389](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/escala/models.py:1389) |
| Período de folga com escala certa | O fechamento primeiro filtra `periodos_ft` por `escala_certa_no_dia`; depois, nos elegíveis cujo vínculo declara `feriado_vira_folga`, transfere as horas para HE100 e HE100 feriado. [app/ponto/services/fechamento.py:919](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/services/fechamento.py:919), [app/ponto/services/fechamento.py:935](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/services/fechamento.py:935), [app/ponto/services/fechamento.py:945](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/services/fechamento.py:945) |
| Folga sem escala certa | As horas entram em trabalhadas, sem aquele adicional. [app/ponto/services/fechamento.py:964](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/services/fechamento.py:964) |
| Feriado em período normal | O motor exige `feriado_em_dobra` e feriado no dia; um dos escritores coloca a duração em `minutos_extra_100_feriado`. [app/ponto/motor_calculo_v2.py:934](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/motor_calculo_v2.py:934), [app/ponto/motor_calculo_v2.py:2809](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/motor_calculo_v2.py:2809) |
| Ciclo e régua | O padrão de `feriado_em_dobra` vem do ciclo: `12x36` integra o conjunto de feriado simples; a fábrica CCT pode habilitar sua dobra por cláusula cadastrada. [app/ponto/motor_calculo_v2.py:3599](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/motor_calculo_v2.py:3599), [app/ponto/motor_calculo_v2.py:3669](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/motor_calculo_v2.py:3669), [app/core/regua_cct.py:477](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/core/regua_cct.py:477) |

Para col655, o dado imprime `fer_vira_folga=False`; portanto, ele não fornece evidência de enquadramento no ramo de folga-feriado de `fechamento.py:940`. Atribuir o contraste do PAR a “12x36 versus 6x1”, a uma CCT específica ou a outro vínculo seria **[nao verificado]**.

## 06/11 18:09 · sessao e58a772d7f48 · ESPURIA · sonda · origem=Code

O QUADRO fecha: **161 + 136 + 96 + 30 + 27 + 12 + 4 = 466**, igual a `UNIVERSO dias_com_orfa_na_ata=466` e `LINHAS 466`. Os vetores `sem_trab` somam **136 + 30 + 4 = 170**; o bloco DINHEIRO soma **93 + 72 + 3 + 1 + 1 = 170**. [nao verificado]

Não aparece campo literal `ERRO`; aparece `ERROS {}`. Há `CONCLUSAO_LIBERADA`. [nao verificado]

A linha a conferir como possível problema de instrumento é **`HE nos sem_trabalho {(False, False, False): 170}`**, pelos três indicadores zerados em todos os 170 dias; as AMOSTRA também exibem `he`, `he_fer` e `folga_trab` em `0.0`. Isso não comprova erro de instrumento. [nao verificado] O modelo define rubricas separadas `horas_extras_50`, `horas_extras_100`, `horas_extras_100_feriado` e `horas_folga_trabalhada`. [app/ponto/models.py:328](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/models.py:328) O mapeamento dessas rubricas para os rótulos da sonda não foi conferido: não localizei `logs/espuria/sonda_espuria.py` nesta cópia. [nao verificado]

As transcrições abaixo são literais do texto fornecido. **Cada linha dos blocos recebe `[nao verificado]`**, indicado aqui fora dos blocos para preservar o conteúdo literal.

1. Agregados, DINHEIRO, ERROS e conclusão — [nao verificado]

```text
UNIVERSO dias_com_orfa_na_ata=466 (aval citou 477 em PROD 17:3x; aqui SOMBRA dump do dia)
ITEM3 {'celulas': 9845, 'dias_lampada_acesa_tipo_real_desconhecido': 578, 'dias_lampada_tipo_divergente': 200, 'dias_lampada_tipo_divergente_COM_orfa': 111, 'dias_lampada_tipo_divergente_SEM_orfa': 89, 'lampadas_tipo_divergente': 303, 'par_E_aceso_por_S': 166, 'par_S_aceso_por_E': 137}
LINHAS 466
QUADRO por vetor (sem_trabalho, lista CxR do motor, regenerada) -- soma=466
  trab/nao_cxr/nao_regen                   161
  sem_trab/nao_cxr/nao_regen               136
  trab/cxr/nao_regen                       96
  sem_trab/nao_cxr/regen                   30
  trab/nao_cxr/regen                       27
  trab/cxr/regen                           12
  sem_trab/cxr/nao_regen                   4
STATUS {'folga_trabalhada': 161, 'escala_suspeita': 6, 'ok': 107, 'irregular': 189, 'aberto': 3}
DONO {'CADASTRO': 279, 'ESTRUTURA': 109, 'BATIDA': 78}
DONO x vetor {('sem_trab/nao_cxr/nao_regen', 'CADASTRO'): 86, ('trab/nao_cxr/nao_regen', 'CADASTRO'): 66, ('sem_trab/nao_cxr/regen', 'ESTRUTURA'): 7, ('trab/nao_cxr/nao_regen', 'ESTRUTURA'): 64, ('trab/nao_cxr/nao_regen', 'BATIDA'): 31, ('trab/cxr/nao_regen', 'CADASTRO'): 88, ('sem_trab/nao_cxr/nao_regen', 'BATIDA'): 19, ('sem_trab/nao_cxr/nao_regen', 'ESTRUTURA'): 31, ('trab/nao_cxr/regen', 'ESTRUTURA'): 7, ('sem_trab/nao_cxr/regen', 'CADASTRO'): 16, ('trab/nao_cxr/regen', 'CADASTRO'): 9, ('trab/nao_cxr/regen', 'BATIDA'): 11, ('sem_trab/nao_cxr/regen', 'BATIDA'): 7, ('trab/cxr/regen', 'CADASTRO'): 12, ('trab/cxr/nao_regen', 'BATIDA'): 8, ('sem_trab/cxr/nao_regen', 'CADASTRO'): 2, ('sem_trab/cxr/nao_regen', 'BATIDA'): 2}
FERIADO {(True, False, True): 30, (False, None, None): 10, (False, True, False): 325, (False, False, False): 78, (True, True, False): 23}
ESCALA_ERRADA {None: 268, 'LIMBO': 66, 'A2': 122, 'A8': 10}
SUGESTAO_PROPOR_ESCALA (por dia) {'media': 191, 'insuficiente': 159, 'alta': 116}
--- DINHEIRO dos dias sem trabalho n=170
  pago / decisao_he=None / porta=None / sem_verbo_admin                            93
  sem_pago / decisao_he=None / porta=None / sem_verbo_admin                        72
  sem_pago / decisao_he=None / porta=porta_pergunta / sem_verbo_admin              3
  pago / decisao_he=autorizado / porta=None / sem_verbo_admin                      1
  sem_pago / decisao_he=nao / porta=None / sem_verbo_admin                         1
ESTRUTURA_DINHEIRO (sem pago E sem decisao E sem porta/verbo admin): 72
HE nos sem_trabalho {(False, False, False): 170}
ERROS {}
CONCLUSAO_LIBERADA
```

2. Todas as linhas ITEM3_EX_SEM_ORFA — [nao verificado]

```text
ITEM3_EX_SEM_ORFA ('col174', '2026-10-25', [('S', '02:00', '02:27', 'E')])
ITEM3_EX_SEM_ORFA ('col191', '2026-10-22', [('S', '16:00', '17:54', 'E'), ('E', '17:00', '18:59', 'S'), ('S', '23:00', '23:01', 'E')])
ITEM3_EX_SEM_ORFA ('col200', '2026-10-29', [('E', '22:00', '02:00', 'S')])
ITEM3_EX_SEM_ORFA ('col200', '2026-11-02', [('E', '22:00', '02:00', 'S'), ('S', '02:00', '02:00', 'E'), ('E', '03:00', '03:02', 'S')])
ITEM3_EX_SEM_ORFA ('col207', '2026-10-24', [('E', '13:00', '13:38', 'S')])
ITEM3_EX_SEM_ORFA ('col247', '2026-10-24', [('E', '23:00', '00:00', 'S')])
```

Todas as linhas AMOSTRA — [nao verificado]

```text
AMOSTRA sem_trab/nao_cxr/nao_regen {'col': 'col43', 'dia': '2026-11-02', 'emp': 2, 'sug': ("{'tipo_escala_id': 421, 'codigo': '34', 'descricao': '6x1milano', 'ciclo': '6x1', 'horario': '07:30-16:30'}", 'media'), 'sem_trabalho': True, 'status': 'folga_trabalhada', 'tipo_dia': 'folga', 'trabalha': False, 'regen': False, 'feriado': True, 'trab_feriado': False, 'fer_vira_folga': True, 'ec': 1411, 'escala_errada': None, 'cxr': False, 'dono': 'CADASTRO', 'lavrada': True, 'pago': None, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
AMOSTRA sem_trab/nao_cxr/nao_regen {'col': 'col97', 'dia': '2026-11-02', 'emp': 2, 'sug': (None, 'insuficiente'), 'sem_trabalho': True, 'status': 'folga_trabalhada', 'tipo_dia': 'folga', 'trabalha': False, 'regen': False, 'feriado': False, 'trab_feriado': None, 'fer_vira_folga': None, 'ec': None, 'escala_errada': None, 'cxr': False, 'dono': 'CADASTRO', 'lavrada': True, 'pago': None, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
AMOSTRA sem_trab/nao_cxr/nao_regen {'col': 'col97', 'dia': '2026-11-03', 'emp': 2, 'sug': (None, 'insuficiente'), 'sem_trabalho': True, 'status': 'escala_suspeita', 'tipo_dia': 'folga', 'trabalha': True, 'regen': False, 'feriado': False, 'trab_feriado': None, 'fer_vira_folga': None, 'ec': None, 'escala_errada': None, 'cxr': False, 'dono': 'CADASTRO', 'lavrada': False, 'pago': None, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
AMOSTRA trab/nao_cxr/nao_regen {'col': 'col174', 'dia': '2026-10-23', 'emp': 2, 'sug': (None, 'insuficiente'), 'sem_trabalho': False, 'status': 'ok', 'tipo_dia': 'trabalho', 'trabalha': True, 'regen': False, 'feriado': False, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 1156, 'escala_errada': 'LIMBO', 'cxr': False, 'dono': 'CADASTRO', 'lavrada': True, 'pago': 6.782049166666667, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': 'nao', 'porta': None, 'verbo_admin': []}
AMOSTRA trab/nao_cxr/nao_regen {'col': 'col174', 'dia': '2026-10-24', 'emp': 2, 'sug': (None, 'insuficiente'), 'sem_trabalho': False, 'status': 'ok', 'tipo_dia': 'trabalho', 'trabalha': True, 'regen': False, 'feriado': False, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 1156, 'escala_errada': 'LIMBO', 'cxr': False, 'dono': 'CADASTRO', 'lavrada': True, 'pago': None, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
AMOSTRA trab/nao_cxr/nao_regen {'col': 'col174', 'dia': '2026-10-26', 'emp': 2, 'sug': (None, 'insuficiente'), 'sem_trabalho': False, 'status': 'ok', 'tipo_dia': 'trabalho', 'trabalha': True, 'regen': False, 'feriado': False, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 1156, 'escala_errada': 'LIMBO', 'cxr': False, 'dono': 'CADASTRO', 'lavrada': True, 'pago': None, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': 'nao', 'porta': None, 'verbo_admin': []}
AMOSTRA sem_trab/nao_cxr/regen {'col': 'col207', 'dia': '2026-11-02', 'emp': 2, 'sug': ("{'tipo_escala_id': 573, 'codigo': '142', 'descricao': '[nome]', 'ciclo': '6x1', 'horario': '06:30-12:30'}", 'media'), 'sem_trabalho': True, 'status': 'folga_trabalhada', 'tipo_dia': 'folga', 'trabalha': False, 'regen': True, 'feriado': True, 'trab_feriado': False, 'fer_vira_folga': True, 'ec': 1452, 'escala_errada': None, 'cxr': False, 'dono': 'ESTRUTURA', 'lavrada': True, 'pago': None, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
AMOSTRA trab/cxr/nao_regen {'col': 'col214', 'dia': '2026-11-01', 'emp': 2, 'sug': ("{'tipo_escala_id': 175, 'codigo': 'PAI-COMERCIAL.1', 'descricao': '6x1 - 12:40 - 17:00/18:00 - 21:00', 'ciclo': '6x1', 'horario': '12:40-21:00'}", 'alta'), 'sem_trabalho': False, 'status': 'ok', 'tipo_dia': 'trabalho', 'trabalha': True, 'regen': False, 'feriado': False, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 182, 'escala_errada': None, 'cxr': True, 'dono': 'CADASTRO', 'lavrada': True, 'pago': 7.382154238055556, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
AMOSTRA sem_trab/nao_cxr/regen {'col': 'col270', 'dia': '2026-10-22', 'emp': 2, 'sug': ("{'tipo_escala_id': 252, 'codigo': 'PAI-12x36.55', 'descricao': '15:00 19:00 20:00 23:00 FOLGA QUINTA', 'ciclo': '12x36', 'horario': '15:00-23:00'}", 'alta'), 'sem_trabalho': True, 'status': 'folga_trabalhada', 'tipo_dia': 'folga', 'trabalha': False, 'regen': True, 'feriado': False, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 1449, 'escala_errada': None, 'cxr': False, 'dono': 'ESTRUTURA', 'lavrada': True, 'pago': None, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
AMOSTRA sem_trab/nao_cxr/regen {'col': 'col270', 'dia': '2026-10-29', 'emp': 2, 'sug': ("{'tipo_escala_id': 252, 'codigo': 'PAI-12x36.55', 'descricao': '15:00 19:00 20:00 23:00 FOLGA QUINTA', 'ciclo': '12x36', 'horario': '15:00-23:00'}", 'alta'), 'sem_trabalho': True, 'status': 'folga_trabalhada', 'tipo_dia': 'folga', 'trabalha': False, 'regen': True, 'feriado': False, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 1449, 'escala_errada': None, 'cxr': False, 'dono': 'ESTRUTURA', 'lavrada': True, 'pago': None, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
AMOSTRA trab/nao_cxr/regen {'col': 'col278', 'dia': '2026-10-26', 'emp': 2, 'sug': ("{'tipo_escala_id': 457, 'codigo': '44x1', 'descricao': '6x1 - 07:50 - 11:10 - 12:10 - 15:50', 'ciclo': '6x1', 'horario': '07:50-15:50'}", 'alta'), 'sem_trabalho': False, 'status': 'irregular', 'tipo_dia': 'trabalho', 'trabalha': True, 'regen': True, 'feriado': False, 'trab_feriado': False, 'fer_vira_folga': False, 'ec': 1350, 'escala_errada': None, 'cxr': False, 'dono': 'ESTRUTURA', 'lavrada': True, 'pago': 6.836554444444444, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': 'porta_fila', 'verbo_admin': []}
AMOSTRA trab/nao_cxr/regen {'col': 'col278', 'dia': '2026-10-29', 'emp': 2, 'sug': ("{'tipo_escala_id': 457, 'codigo': '44x1', 'descricao': '6x1 - 07:50 - 11:10 - 12:10 - 15:50', 'ciclo': '6x1', 'horario': '07:50-15:50'}", 'alta'), 'sem_trabalho': False, 'status': 'irregular', 'tipo_dia': 'trabalho', 'trabalha': True, 'regen': True, 'feriado': False, 'trab_feriado': False, 'fer_vira_folga': False, 'ec': 1350, 'escala_errada': None, 'cxr': False, 'dono': 'ESTRUTURA', 'lavrada': True, 'pago': 6.844773611111111, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': 'porta_fila', 'verbo_admin': []}
AMOSTRA trab/nao_cxr/regen {'col': 'col278', 'dia': '2026-10-30', 'emp': 2, 'sug': ("{'tipo_escala_id': 457, 'codigo': '44x1', 'descricao': '6x1 - 07:50 - 11:10 - 12:10 - 15:50', 'ciclo': '6x1', 'horario': '07:50-15:50'}", 'alta'), 'sem_trabalho': False, 'status': 'irregular', 'tipo_dia': 'trabalho', 'trabalha': True, 'regen': True, 'feriado': False, 'trab_feriado': False, 'fer_vira_folga': False, 'ec': 1350, 'escala_errada': None, 'cxr': False, 'dono': 'ESTRUTURA', 'lavrada': True, 'pago': 6.834293055555556, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': 'porta_fila', 'verbo_admin': []}
AMOSTRA trab/cxr/nao_regen {'col': 'col371', 'dia': '2026-11-02', 'emp': 2, 'sug': ("{'tipo_escala_id': 277, 'codigo': 'PAI-12x36.74', 'descricao': '12:00 16:00 17:00 20:00 folga segunda', 'ciclo': '12x36', 'horario': '12:00-20:00'}", 'alta'), 'sem_trabalho': False, 'status': 'ok', 'tipo_dia': 'trabalho', 'trabalha': True, 'regen': False, 'feriado': True, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 324, 'escala_errada': None, 'cxr': True, 'dono': 'CADASTRO', 'lavrada': True, 'pago': 5.010742500000001, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': 'porta_fila', 'verbo_admin': []}
AMOSTRA trab/cxr/regen {'col': 'col373', 'dia': '2026-10-29', 'emp': 2, 'sug': ("{'tipo_escala_id': 382, 'codigo': 'PAI-6X1.ALPHAT', 'descricao': '6x1 - 12:50 - 14:40 - 15:50 - 21:20', 'ciclo': '6x1', 'horario': '14:00-22:20'}", 'media'), 'sem_trabalho': False, 'status': 'ok', 'tipo_dia': 'trabalho', 'trabalha': True, 'regen': True, 'feriado': False, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 1497, 'escala_errada': 'LIMBO', 'cxr': True, 'dono': 'CADASTRO', 'lavrada': True, 'pago': 5.77680861111111, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': 'nao', 'porta': 'porta_fila', 'verbo_admin': []}
AMOSTRA trab/cxr/regen {'col': 'col373', 'dia': '2026-10-30', 'emp': 2, 'sug': ("{'tipo_escala_id': 382, 'codigo': 'PAI-6X1.ALPHAT', 'descricao': '6x1 - 12:50 - 14:40 - 15:50 - 21:20', 'ciclo': '6x1', 'horario': '14:00-22:20'}", 'media'), 'sem_trabalho': False, 'status': 'ok', 'tipo_dia': 'trabalho', 'trabalha': True, 'regen': True, 'feriado': False, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 1497, 'escala_errada': 'LIMBO', 'cxr': True, 'dono': 'CADASTRO', 'lavrada': True, 'pago': 6.099447777777778, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': 'nao', 'porta': None, 'verbo_admin': []}
AMOSTRA trab/cxr/nao_regen {'col': 'col382', 'dia': '2026-10-23', 'emp': 2, 'sug': ("{'tipo_escala_id': 547, 'codigo': '120', 'descricao': 'pgconcentrix', 'ciclo': '6x1', 'horario': '15:00-23:30'}", 'media'), 'sem_trabalho': False, 'status': 'irregular', 'tipo_dia': 'trabalho', 'trabalha': True, 'regen': False, 'feriado': False, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 1269, 'escala_errada': None, 'cxr': True, 'dono': 'CADASTRO', 'lavrada': True, 'pago': 7.195496111111111, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
AMOSTRA trab/cxr/regen {'col': 'col707', 'dia': '2026-10-29', 'emp': 2, 'sug': ("{'tipo_escala_id': 381, 'codigo': '12x36_DG.2', 'descricao': '12X36 SUPERVISAO CURITIBA  DAS 11:00 AS 23:00', 'ciclo': '12x36', 'horario': '11:00-23:00'}", 'media'), 'sem_trabalho': False, 'status': 'irregular', 'tipo_dia': 'trabalho', 'trabalha': True, 'regen': True, 'feriado': False, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 1465, 'escala_errada': None, 'cxr': True, 'dono': 'CADASTRO', 'lavrada': True, 'pago': 11.165899238888887, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': 'porta_fila', 'verbo_admin': []}
AMOSTRA sem_trab/cxr/nao_regen {'col': 'col788', 'dia': '2026-10-23', 'emp': 2, 'sug': (None, 'insuficiente'), 'sem_trabalho': True, 'status': 'folga_trabalhada', 'tipo_dia': 'folga', 'trabalha': False, 'regen': False, 'feriado': False, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 1301, 'escala_errada': 'A2', 'cxr': True, 'dono': 'CADASTRO', 'lavrada': True, 'pago': 7.057353611111112, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
AMOSTRA sem_trab/cxr/nao_regen {'col': 'col788', 'dia': '2026-10-29', 'emp': 2, 'sug': (None, 'insuficiente'), 'sem_trabalho': True, 'status': 'folga_trabalhada', 'tipo_dia': 'folga', 'trabalha': False, 'regen': False, 'feriado': False, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 1301, 'escala_errada': 'A2', 'cxr': True, 'dono': 'BATIDA', 'lavrada': True, 'pago': 0.8538388888888889, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
AMOSTRA sem_trab/cxr/nao_regen {'col': 'col788', 'dia': '2026-11-02', 'emp': 2, 'sug': (None, 'insuficiente'), 'sem_trabalho': True, 'status': 'folga_trabalhada', 'tipo_dia': 'folga', 'trabalha': False, 'regen': False, 'feriado': True, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 1301, 'escala_errada': 'A2', 'cxr': True, 'dono': 'BATIDA', 'lavrada': True, 'pago': 16.0160475, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
```

Todas as linhas D0211 — [nao verificado]

```text
D0211 {'col': 'col43', 'dia': '2026-11-02', 'emp': 2, 'sug': ("{'tipo_escala_id': 421, 'codigo': '34', 'descricao': '6x1milano', 'ciclo': '6x1', 'horario': '07:30-16:30'}", 'media'), 'sem_trabalho': True, 'status': 'folga_trabalhada', 'tipo_dia': 'folga', 'trabalha': False, 'regen': False, 'feriado': True, 'trab_feriado': False, 'fer_vira_folga': True, 'ec': 1411, 'escala_errada': None, 'cxr': False, 'dono': 'CADASTRO', 'lavrada': True, 'pago': None, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
D0211 {'col': 'col97', 'dia': '2026-11-02', 'emp': 2, 'sug': (None, 'insuficiente'), 'sem_trabalho': True, 'status': 'folga_trabalhada', 'tipo_dia': 'folga', 'trabalha': False, 'regen': False, 'feriado': False, 'trab_feriado': None, 'fer_vira_folga': None, 'ec': None, 'escala_errada': None, 'cxr': False, 'dono': 'CADASTRO', 'lavrada': True, 'pago': None, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
D0211 {'col': 'col203', 'dia': '2026-11-02', 'emp': 2, 'sug': ("{'tipo_escala_id': 305, 'codigo': 'PAI-12x36.92', 'descricao': '07:00 - 12:00/13:00 - 15:00', 'ciclo': '12x36', 'horario': '07:00-15:00'}", 'alta'), 'sem_trabalho': True, 'status': 'folga_trabalhada', 'tipo_dia': 'folga', 'trabalha': False, 'regen': False, 'feriado': True, 'trab_feriado': False, 'fer_vira_folga': True, 'ec': 1409, 'escala_errada': 'LIMBO', 'cxr': False, 'dono': 'CADASTRO', 'lavrada': True, 'pago': 7.069715555555556, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
D0211 {'col': 'col207', 'dia': '2026-11-02', 'emp': 2, 'sug': ("{'tipo_escala_id': 573, 'codigo': '142', 'descricao': '[nome]', 'ciclo': '6x1', 'horario': '06:30-12:30'}", 'media'), 'sem_trabalho': True, 'status': 'folga_trabalhada', 'tipo_dia': 'folga', 'trabalha': False, 'regen': True, 'feriado': True, 'trab_feriado': False, 'fer_vira_folga': True, 'ec': 1452, 'escala_errada': None, 'cxr': False, 'dono': 'ESTRUTURA', 'lavrada': True, 'pago': None, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
D0211 {'col': 'col227', 'dia': '2026-11-02', 'emp': 2, 'sug': (None, 'insuficiente'), 'sem_trabalho': False, 'status': 'irregular', 'tipo_dia': 'trabalho', 'trabalha': True, 'regen': False, 'feriado': True, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 195, 'escala_errada': None, 'cxr': False, 'dono': 'BATIDA', 'lavrada': True, 'pago': 8.893322777777776, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': 'porta_fila', 'verbo_admin': []}
D0211 {'col': 'col242', 'dia': '2026-11-02', 'emp': 2, 'sug': ("{'tipo_escala_id': 277, 'codigo': 'PAI-12x36.74', 'descricao': '12:00 16:00 17:00 20:00 folga segunda', 'ciclo': '12x36', 'horario': '12:00-20:00'}", 'alta'), 'sem_trabalho': True, 'status': 'folga_trabalhada', 'tipo_dia': 'folga', 'trabalha': False, 'regen': False, 'feriado': True, 'trab_feriado': False, 'fer_vira_folga': True, 'ec': 1398, 'escala_errada': None, 'cxr': False, 'dono': 'ESTRUTURA', 'lavrada': True, 'pago': None, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
D0211 {'col': 'col278', 'dia': '2026-11-02', 'emp': 2, 'sug': ("{'tipo_escala_id': 457, 'codigo': '44x1', 'descricao': '6x1 - 07:50 - 11:10 - 12:10 - 15:50', 'ciclo': '6x1', 'horario': '07:50-15:50'}", 'alta'), 'sem_trabalho': True, 'status': 'folga_trabalhada', 'tipo_dia': 'folga', 'trabalha': False, 'regen': True, 'feriado': True, 'tra

[truncado em 20000 de 47416 caracteres]

## 06/11 18:08 · sessao c956e686389f · - · - · origem=Ronald (tmux)

Não é possível certificar a implementação da sonda: `sonda_espuria2.py` e seus emissores `S2_DIN`/`PAR_ACHADOS` não foram localizados na cópia pesquisada. Linha da própria sonda que replique regra: **[não verificado]**.

As fontes reais comprovadas:

- **Rubricas:** `rubricas_do_dia` lê `DiaPago.horas_extras_50` e `.horas_extras_100`, devolvendo `extras_50`/`extras_100`; `minutos_extra` é a soma desses totais ×60. Não devolve chaves `horas_extras_*`. `app/ponto/services/dia_pago.py:908-926`.
- **Uso em produção pelo código:** espelho importa e chama `linhas_do_periodo`, `pago_do_dia`, `rubricas_do_dia`; calendário também chama `rubricas_do_dia`. `app/ponto/services/espelho.py:406-410`, `app/ponto/services/espelho.py:811-813`, `app/colaboradores/services/calendario.py:655-656`.
- **HE feriado e folga trabalhada:** campos diretos do modelo. `app/ponto/models.py:331-333`.
- **Pago:** a porta `pago_do_dia` primeiro retorna `None` se não houver sinal em nenhuma rubrica monetária; senão soma trabalhadas + folga trabalhada + `detalhe['folga_feriado_min']/60`. Reproduzir esse critério na sonda seria duplicação; se isso ocorre nela é **[não verificado]**. `app/ponto/services/dia_pago.py:888-896`.
- **Batidas apuráveis:** porta aplica `retratada_em IS NULL`, os limites opcionais inclusivos e colaborador, ordenando por timestamp. Não aplica regra de dia da jornada. `app/ponto/turnos.py:2863-2871`.
- **Dia de cada batida:** existe porta própria `dia_das_batidas`; atribui batidas reclamadas ao `data_turno` e as restantes à data local. Agrupar manualmente todas por data civil não reproduziria essa atribuição; implementação da sonda **[não verificado]**. `app/ponto/turnos.py:2874-2905`, `app/ponto/turnos.py:2908-2919`.
- **Atenção a `min_real`:** o campo persistido `DiaPago.minutos_realizados` existe e é alimentado pelo mapa da grade; o relógio publicado no espelho vem de `realizado_do_dia`, passando pelo vínculo e intervalo do dia, e aparece como `minutos_realizados` no dicionário da tela. São origens que precisam ser distinguidas; a origem exata do `min_real` da sonda é **[não verificado]**. `app/ponto/models.py:351`, `app/ponto/services/dia_pago.py:283-290`, `app/ponto/services/espelho.py:286-294`, `app/ponto/services/espelho.py:675-676`, `app/ponto/services/espelho.py:783`, `app/ponto/turnos.py:772-793`.

As ausências são diferentes: `rubricas_do_dia(None)` entrega zeros com `lavrada=False`; `pago_do_dia` entrega `None` tanto para linha ausente quanto para linha com todas as rubricas monetárias zeradas. Portanto `sem_pago` sozinho não prova falta de lavratura. `app/ponto/services/dia_pago.py:888-891`, `app/ponto/services/dia_pago.py:908-914`.

## 06/11 18:08 · sessao 8f98d7cb6b29 · - · - · origem=Ronald (tmux)

Não é possível explicar o cadastro concreto do PAR: o dado termina com `PAR_ACHADOS {}`, não identifica os dois participantes e não fornece ciclo/tipo de escala nem régua de ambos. Atribuir a diferença a 12x36 versus 6x1 seria [não verificado].

O dado mostra col655 em 02/11 com `ec=1275`, `trab_feriado=True`, `fer_vira_folga=False`, `min_real=240`, `pago=2.305491111111111`, `he=2.31`, `he_fer=2.305491111111111`; a identidade do outro participante é [não verificado].

Caminhos comprovados no código:

- Vínculo do dia: `app/escala/servico_jornada.py:38-58` carrega célula e vínculos e delega a `vinculo_do_dia`; havendo geradora na célula, ela alimenta a decisão.
- `feriado_vira_folga`: `app/escala/models.py:1389-1395` retorna falso se `trabalha_em_feriado=True` ou escala ausente; exige ciclo permitido e feriado no lugar do vínculo. Ciclos permitidos são 6x1, 5x2 e semanal, em `app/escala/models.py:32`. Em `app/escala/models.py:1369-1371`, o predicado pode converter trabalho previsto em folga.
- Folga com adicional: `app/ponto/services/fechamento.py:919-926` filtra `resultado.periodos_ft` por `escala_certa_no_dia`. O juiz recusa LIMBO em `app/escala/services/escala_certa.py:31-33` e assinaturas A2/A8 em `app/escala/services/escala_certa.py:38-51`.
- Rubrica de feriado em folga: `app/ponto/services/fechamento.py:935-950` resolve o vínculo do dia, pergunta `feriado_vira_folga`, separa esses períodos e soma suas horas a HE100 e HE100 feriado. O resto elegível vai a folga trabalhada. O mesmo conjunto é entregue à lavratura de `DiaPago` em `app/ponto/services/fechamento.py:1184-1188`; `app/ponto/services/dia_pago.py:244-256` escreve HE100, HE100 feriado e `detalhe['folga_feriado_min']`.
- Folga sem escala certa: fica fora dos adicionais e entra em trabalhadas por `app/ponto/services/fechamento.py:964-969` e `app/ponto/services/dia_pago.py:257-265`.
- Feriado dos períodos normais: `app/ponto/motor_calculo_v2.py:934` exige `feriado_em_dobra` e feriado; um dos escritores da rubrica está em `app/ponto/motor_calculo_v2.py:2809-2812`, outro em `app/ponto/motor_calculo_v2.py:3092-3095`.
- O padrão `feriado_em_dobra` é decidido pelo **tipo_ciclo**, não pelo `tipo_base` ou nome da classe: `app/ponto/motor_calculo_v2.py:3623-3624,3669-3680`. Apenas `'12x36'` pertence ao conjunto padrão de feriado simples (`app/ponto/motor_calculo_v2.py:3599`). CCT pode habilitar dobra do 12x36 em `app/core/regua_cct.py:477-478`.
- Régua do motor: fechamento chama `get_motor_cct` com tipo da escala e início da fatia em `app/ponto/services/fechamento.py:725-732`. `regua_para` lê vínculo e praça do dia em `app/core/regua_cct.py:328-339`, regime da empresa em `app/core/regua_cct.py:352-366` e aplicação/CCT em `app/core/regua_cct.py:366-379`; `get_motor_cct` injeta os parâmetros em `app/core/regua_cct.py:458-478`.

Cuidado ao citar comentários como regra executável: há comentários antigos dizendo “feriado previsto não dobra” em `app/ponto/motor_calculo_v2.py:2807` e `app/ponto/motor_calculo_v2.py:3088-3091`, mas o predicado efetivamente chamado em `app/ponto/motor_calculo_v2.py:934` não consulta previsto; consulta apenas o flag de dobra e o feriado.

_2396 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
