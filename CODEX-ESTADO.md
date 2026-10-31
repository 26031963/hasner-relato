# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 12:36** · publicado em **31/10/2026 12:39** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 12:36 · sessao 0819a0bb99c3 · INTERMITENTE-EM-FERIADO · leitura · origem=Code

**1. Fontes existentes e limites da leitura**

**Texto integral da CLT, do art. 452-A/§6 e da Lei 605/49/art. 9:** sem fonte no repo. Foram encontradas referências a esses dispositivos, sem sua transcrição integral. [nao verificado]

**Conteúdo cadastrado de `Sindicato.obs`:** sem fonte no repo. [nao verificado] O modelo contém apenas a declaração `obs = models.TextField(blank=True)`. (`app/core/models.py:40`)

O repositório aponta onde estaria esse conteúdo:

> “A fundamentacao INTEIRA (clausulas literais + parecer) esta em Sindicato.obs e em core/regua_cct.py”  
> “A CCT em PDF **nao esta no sistema** […] Enquanto nao tiver, o lastro e o texto em `obs`.”

(`CLAUDE.md:442-443`, `CLAUDE.md:455-456`)

A CCT identificada é “Vigilantes de Londrina/PR”, registro “MTE PR000251/2026”, vigência “01/02/2026-31/01/2028”. (`CLAUDE.md:430-432`)

Existe esta transcrição literal da cláusula 38-d:

> “no regime de 12x36 - ainda que cumprido em horario noturno -, a hora sera considerada normal de 60 (sessenta) minutos, garantido, sempre, o adicional noturno respectivo.”

O trecho trata de hora noturna no 12x36; não menciona feriado ou intermitente. (`app/core/regua_cct.py:127-138`)

**2. O que está escrito sobre intermitente, dobra e §6**

| Fonte | Trecho literal e alcance |
|---|---|
| Docstring de `MotorIntermitente` | “Intermitente-horista (Art. 452-A) — sem escala, sem marcos, sem jornada prevista. Apura SOMENTE horas cumpridas do par E/S (+ noturno legal). **Sem HE por estouro de jornada, sem feriado-100 automatico, sem atraso.**” (`app/ponto/motor_calculo_v2.py:3485-3488`) |
| Comentário do cálculo geral de feriado | “Feriado: Sumula 146 TST paga **TODAS as horas em dobro (nao so excedente)**.” (`app/ponto/motor_calculo_v2.py:2731-2733`) |
| Docstring da regra compartilhada | “o dia de feriado que dobra poe a jornada **INTEIRA em HE 100** […] e nao so o excedente.” (`app/ponto/calculador/regras.py:330-332`) |
| Régua CCT: piso declarado para 12x36 | `'feriado_12x36_em_dobra': False, # BUG 91: piso legal = hora simples (Art.59-A par.2)` (`app/core/regua_cct.py:44`) |
| Régua CCT: exceção cadastrada | “a dobra do feriado no 12x36 so volta por **CLAUSULA CADASTRADA**”; “Fora do 12x36 […] a Sumula 146 e o piso”. (`app/core/regua_cct.py:473-476`) |
| Comentário do modelo `Sindicato` | “o piso LEGAL do feriado trabalhado em 12x36 e HORA SIMPLES”; “a convencao coletiva PODE ser mais benefica e devolver a dobra; nenhuma das nossas diz isso hoje”. (`app/core/models.py:49-52`) |
| Comentário do espelho | “100% = descanso trabalhado (Lei 605/49 art.9 + Sum.146 TST) + excedente por CCT.” Em seguida: “Quantidade CRUA: majoracao e' da folha, nunca do ponto.” (`app/relatorios/pdf_espelho.py:764-766`) |

Há **cortes internos em sentidos diferentes**, com a inversão expressamente registrada:

- **13/10, aproximadamente 17h:** “intermitente que trabalha em feriado recebe a dobra (Lei 605 art.9 / Sumula 146 / art.452-A par.6) -- a docstring do MotorIntermitente se corrige para o codigo”. (`app/docs/PROMPTS.md:1204`)
- **13/10, aproximadamente 18h:** “intermitente em feriado recebe dobra (452-A §6, Lei 605 art.9, Sum.146)”; acrescenta: “**leitura passa pelo DP antes de virar lei**”. (`app/docs/PROMPTS.md:1213`)
- **14/10, 21:4x:** “**SDF/intermitente em feriado NAO recebe dobra (DP 14/10); o cartao nao afirma pagamento em dobro**”. A própria linha identifica esse corte como inversão da O264. (`app/docs/PROMPTS.md:1231`)
- **15/10, 11:5x:** “**intermitente segue pela docstring do MotorIntermitente (sem feriado-100 automatico)**”. O registro complementar diz “sem entrada nova no frozenset”. (`app/docs/PROMPTS.md:1239`)

O BACKLOG também registra a inversão:

> “A LEITURA DO DP CHEGOU E INVERTEU ESTA OBRA”; “a docstring do `MotorIntermitente` **nao se corrige**”; a cura dos dois zeros de 12/10 está “**CANCELADA** -- aqueles zeros sao o numero CERTO”.

(`app/docs/BACKLOG.md:1377`; orientação complementar em `app/docs/BACKLOG.md:1390`)

**Art. 452-A, §6:** os registros de 13/10 acima citam o dispositivo para sustentar a dobra, mas não transcrevem seu conteúdo. (`app/docs/PROMPTS.md:1204`, `app/docs/PROMPTS.md:1213`) Para a enumeração literal solicitada — remuneração, férias + 1/3, 13º, RSR e adicionais legais, pagos ao fim de cada período — **sem fonte no repo**. [nao verificado]

**3. Tratamento atual no código**

**Motor v2**

A limpeza está em [app/ponto/motor_calculo_v2.py:3491](/home/ronald/codex_ws/c7ea207a3f1e8ea911a0265b8e88991b4d91c1e4/app/ponto/motor_calculo_v2.py:3491), com estas atribuições:

```python
super().__init__(*args, **kwargs)
# neutraliza gatilhos de extra: jornada "infinita" e sem feriados
self.jornada_minutos = 24 * 60 * 365
self.feriados = set()
```

(`app/ponto/motor_calculo_v2.py:3491-3495`)

**As linhas atuais 903–905 não limpam o calendário:** elas consultam o conjunto e depois chamam o fallback. (`app/ponto/motor_calculo_v2.py:901-905`)

| Decisão | Código literal / valor produzido |
|---|---|
| Escolha do motor | `if ciclo == 'intermitente' or tipo_base == 'intermitente': cls = MotorIntermitente`. (`app/ponto/motor_calculo_v2.py:3563-3564`) |
| Flag de dobra pelo ciclo | `CICLOS_FERIADO_SIMPLES = frozenset({'12x36'})`; se o argumento não veio: `feriado_em_dobra = ciclo not in CICLOS_FERIADO_SIMPLES`. Para o ciclo literal `intermitente`, esse ramo produz **`True`**. (`app/ponto/motor_calculo_v2.py:3537`, `:3607-3608`) |
| Limpeza no intermitente | `self.feriados = set()`: conjunto vazio. (`app/ponto/motor_calculo_v2.py:3495`) |
| Reconhecimento do feriado | `if data in self.feriados: return True`; caso contrário: `return is_feriado(data)`. O conjunto vazio mantém essa segunda chamada. (`app/ponto/motor_calculo_v2.py:901-905`) |
| Fallback | `return eh_feriado(data, praca)`; em exceção, `return False`. O argumento `praca` tem padrão `None`. (`app/ponto/motor_calculo_v2.py:30-43`) |
| Alcance sem município | “SEM MUNICIPIO […] nem municipal nem estadual valem, so o nacional.” (`app/core/feriados.py:91-95`) |
| Decisão de dobra | `return bool(self.feriado_em_dobra) and self.eh_feriado(dia)`. (`app/ponto/motor_calculo_v2.py:879`) |

`MotorIntermitente` herda de `Motor12x36` e seu corpo contém somente o `__init__` mostrado acima. (`app/ponto/motor_calculo_v2.py:3483-3495`) O `calcular_periodo` herdado decide assim:

- **Sem saída:** marca `turno_aberto = True` e retorna; os campos de extra nascem em zero. (`app/ponto/motor_calculo_v2.py:2689-2693`, `:166-169`)
- **Com duração positiva e dobra reconhecida:** atribui a duração inteira a `minutos_extra`, `minutos_extra_100` e `minutos_extra_100_feriado`, e marca `hora_extra_100 = True`. (`app/ponto/motor_calculo_v2.py:2747-2754`)
- **Ramo seguinte:** `elif feriado and duracao > 0 and self.feriado_em_dobra` apenas acrescenta o alerta “PREVISTO na escala — horas SIMPLES”. O helper anterior já usa o mesmo flag e a consulta de feriado. (`app/ponto/motor_calculo_v2.py:2756-2760`, `:879`)
- **Demais situações:** calcula excedente contra `_jornada_do_dia(e)`; sem previsto, subtrai a própria duração, produzindo **extra zero**. Havendo extra, aplica a tolerância e reparte em até 120 minutos a 50%, com o restante a 100%. (`app/ponto/motor_calculo_v2.py:2773-2792`)

Assim, **o corpo não estabelece “HE100 feriado sempre zero para intermitente”**: o calendário injetado é apagado, mas permanecem o fallback e o flag derivado do ciclo. (`app/ponto/motor_calculo_v2.py:3495`, `:901-905`, `:3537`, `:3607-3608`)

Há teste que injeta `12/06/2026` como feriado e exige `minutos_extra_100 == 0` e `horas_extra == 0`, com as mensagens “Horista nao tem feriado-100 automatico” e “Horista nao tem HE por estouro de jornada”. (`app/ponto/tests/test_intermitente.py:54-68`)

**Calculador em `ponto/calculador/`**

Na alimentação de `regras.do_dia`, o calendário vem diretamente do argumento recebido:

```python
_feriados = set(feriados or ())
```

O flag de dobra vem do motor CCT; a identificação de intermitente é atualizada pelo vínculo do dia e enviada separadamente à regra. (`app/ponto/calculador/alimentacao.py:378-396`, `:479-485`, `:509-510`)

| Ramo | Valor produzido |
|---|---|
| Folga trabalhada do intermitente | `if _trab <= 0 or intermitente: return 0.0, 0.0`. (`app/ponto/calculador/regras.py:386-388`) |
| Dia sem trabalho previsto, mas intermitente | Não retorna pelo ramo dos demais regimes; grava `intermitente_segue_como_trabalho = True` e `horas_trabalhadas = trab / 60`. (`app/ponto/calculador/regras.py:736-740`) |
| Excedente sem previsto | `extra = trab - _limite if previsto_min else 0.0`: **zero** quando não há previsto. (`app/ponto/calculador/regras.py:767-771`) |
| Regra de feriado | Usa os mesmos métodos `MotorBase._feriado_dobra_no_dia` e `MotorBase.eh_feriado`. Retorna o trabalhado inteiro quando o juiz reconhece dobra; caso contrário, **`0.0`**. (`app/ponto/calculador/regras.py:287-295`, `:344-347`) |
| Dobra positiva | `horas_extras_100 = _dobra / 60`, `horas_extras_100_feriado = _dobra / 60`, `horas_extras_50 = 0.0`; retorna sem somar o excedente comum. (`app/ponto/calculador/regras.py:774-789`) |
| Calendário ou flag ausentes | Declara `horas_extras_100` e `horas_extras_100_feriado` em `SEM_ENTRADA`. (`app/ponto/calculador/regras.py:633-638`) |

Na releitura v3 de `periodo.py`, `reler` é descrito como “**LEITURA DE ENSAIO, nao caminho de producao**”. (`app/ponto/calculador/periodo.py:540-545`) Sem previsto, o excesso retorna com os campos numéricos zerados; a dobra é julgada depois, separadamente. (`app/ponto/calculador/periodo.py:367-371`, `:410-413`, `:819-831`) Quando positiva, grava toda a dobra em `minutos_extra`, `minutos_extra_100` e `minutos_extra_100_feriado`, com `minutos_extra_50 = 0.0`; caso contrário, grava os valores do excesso. (`app/ponto/calculador/periodo.py:833-847`)

A integração do fechamento chama `rubricas_por_dia` e grava `versao='oraculo'`; o comentário registra que essas linhas nascem ao lado de `versao='motor'` e que “Nenhum leitor le 'oraculo' hoje”. (`app/ponto/services/fechamento.py:1214-1217`, `:1233-1262`)

O relatório documenta um caso específico como “v2 paga 0 de HE100 feriado […] v3 paga em dobra”, mas o mantém como **pergunta**. Sua referência à limpeza em `903-905` não corresponde às linhas atuais do código. (`app/docs/RELATO.md:12`; `app/ponto/motor_calculo_v2.py:901-905`, `:3493-3495`)

**4. Leis L-NNN, cortes e casos E1**

| Registro | Trecho literal |
|---|---|
| **L-085** | “A JORNADA PERTENCE AO DIA DE INICIO, inclusive para domingo, feriado, contagem de semana e DSR”. (`app/docs/LEIS.md:117`) |
| **L-086** | Texto original: “domingo ou feriado so pagam dobra em dia NAO previsto”. A mesma linha registra: “a clausula de FERIADO do texto esta **REVOGADA** -- feriado previsto DOBRA conforme feriado_em_dobra do ciclo (12x36 nao dobra)”. (`app/docs/LEIS.md:118`) |
| Corte corretivo de 27/09 | “FERIADO NAO entra na L-086: segue a regra de feriado que ja existe no cadastro do ciclo”. (`app/ponto/motor_calculo_v2.py:862-865`) |
| **L-095** | Inclui “HE100 feriado” entre os valores lavrados uma vez pelo motor v2. (`app/docs/LEIS.md:113`) |
| Menção a intermitente na auditoria da **L-091** | “tipo_base ainda escolhe o motor […] 12x36 e intermitente pelo rotulo”. (`app/docs/LEIS.md:174`) |
| **C05** | Descrito como “lote 1 contra a v2, rubrica extra”, “v3 verde, v2 vermelha”. (`app/docs/RELATO.md:86`) |
| **C06** | “CCT Vigilantes x prorrogacao noturna”. (`app/docs/BACKLOG.md:782`) |
| **C16** | “intervalo 30 min batido: suprimido=30 pelo art.71 par.4 pos-2017?”. (`app/docs/BACKLOG.md:782`) |
| **C17** | “comercial 08-17 sem intervalo”; resposta registrada: “C17 HE 0 intra 60”. (`app/docs/RELATO.md:91`) |
| Corte **V3-E1-C17** | “L-031+L-032: sem intervalo batido indeniza so o suprimido, HE 0; C17/C16/C06 PENDENTE ate o DP/contador”. (`app/docs/CORTES.md:410`) |
| Corte **E1-VALIDACAO** | “lote1 todos menos C05/C17 (DP)”. (`app/docs/CORTES.md:411`) |
| Estado posteriormente relatado | “PENDENTE_DP C05/C06/C16/C17 em skip”. (`app/docs/RELATO.md:82`) |

**Arquivos `docs/E1-*` e texto integral desses quatro casos:** sem fonte no repo. [nao verificado] O relatório referencia `docs/E1-PERGUNTA-AO-DP-CONTADOR.md` expressamente como documento “da copia” e identifica outra cópia de trabalho para o E1. (`app/docs/RELATO.md:88`, `:92`)

**C05, C06, C16 ou C17 como caso específico de intermitente trabalhando em feriado:** sem fonte no repo. [nao verificado]

## 31/10 12:35 · sessao d4425bae0753 · - · - · origem=Ronald (tmux)

Fontes encontradas, todas dentro do snapshot:

- **`app/docs/LEIS.md:117` — L-085:** “**A JORNADA PERTENCE AO DIA DE INICIO**, inclusive para domingo, feriado, contagem de semana e DSR: hora depois da meia-noite herda o dia em que o plantao COMECOU, e nunca cria trabalho no dia seguinte”.
- **`app/docs/LEIS.md:118` — L-086**, texto original: “**PLANTAO PREVISTO NA ESCALA NUNCA E HE 100%**: domingo ou feriado so pagam dobra em dia NAO previsto (folga, ou feriado trabalhado fora da escala)”. **A mesma linha registra a revogação parcial:** “a clausula de FERIADO do texto esta REVOGADA -- feriado previsto DOBRA conforme feriado_em_dobra do ciclo (12x36 nao dobra), e ha selo que exige isso.”
- **`app/docs/LEIS.md:113` — L-095:** “o valor de um dia (normais, HE50, HE100, HE100 feriado, noturno, intra indenizada, atraso, saida antecipada, folga trabalhada, falta) e lavrado **uma vez**, pelo motor v2, no evento”.
- **`app/docs/LEIS.md:124` — L-091:** “o motor e a geometria saem do DESENHO […] `TipoEscala.tipo_base` e rotulo de tela e nao manda no calculo”. A auditoria **`app/docs/LEIS.md:174`** registra literalmente: “12x36 e intermitente pelo rotulo”. Não é regra específica de remuneração de feriado.

**Histórico explícito sobre INTERMITENTE-EM-FERIADO — indispensável preservar as mudanças:**

1. **`app/docs/PROMPTS.md:1204`**, corte 13/10 14:5x, ainda alternativa:  
   “corte Ronald: intermitente que trabalha em feriado (recebe dobra pela Lei 605 art.9 OU nao recebe, como o MotorIntermitente declara). Segue a fila; PAREI so em lei ou !”

   A mesma linha contém DELTA 13/10 16:0x:  
   “intermitente que trabalha em feriado **RECEBE** a dobra (Lei 605 art.9); o override morto do `MotorIntermitente` sai pela **L-111**.”

   E DELTA ~17:0x:  
   “intermitente que trabalha em feriado recebe a dobra (Lei 605 art.9 / Sumula 146 / art.452-A par.6) -- a docstring do MotorIntermitente se corrige para o codigo, e os 2 zeros de 12/10 (23,74 h, mesma empresa, mesmo ciclo, trabalha_em_feriado=True) sao bug de ESTRUTURA a curar na comp 10 com DIFF antes”.

2. **`app/docs/PROMPTS.md:1213`**, aval 13/10 ~18:1x:  
   “aval Ronald: pergunta 2 — intermitente em feriado recebe dobra (452-A §6, Lei 605 art.9, Sum.146); docstring errada, os 2 zeros de 12/10 sao bug; **leitura passa pelo DP antes de virar lei**.”

3. **`app/docs/PROMPTS.md:1231`**, corte 14/10 21:4x, rótulo do próprio documento “lei dele, e INVERTE a da O264”:  
   “corte Ronald: **SDF/intermitente em feriado NAO recebe dobra (DP 14/10); o cartao nao afirma pagamento em dobro**. Segue a fila; PAREI so em lei ou !”

   A mesma linha registra: “a O264 ganha DELTA (docstring NAO se corrige; a cura dos 2 zeros de 12/10 esta CANCELADA) e o resto migra para a O277”.

4. **`app/docs/PROMPTS.md:1239`**, corte 15/10 11:5x:  
   “corte Ronald O277: quem carrega o lado SDF e o cadastro da O275 ('feriado e dia de trabalho' no TipoEscala) -- tudo tem cadastro, nada de lista paralela; **intermitente segue pela docstring do MotorIntermitente (sem feriado-100 automatico)**. Ordem: O275 pousa, depois O277 le o flag. DIFF da dobra ja paga na 10 publicado antes; 09 exportada nao se toca.” [nao verificado]

   O comentário da mesma linha explicita: “o intermitente ficando pela docstring, **sem entrada nova no frozenset**”.

5. **`app/docs/BACKLOG.md:227-229`**, repetição do corte de 15/10:  
   “O277: quem carrega o lado SDF e o CADASTRO DA O275 […] ‘O275 pousa, depois O277 le o flag’ -- intermitente segue pela docstring do MotorIntermitente, **sem feriado-100 automatico**.”

6. **`app/docs/RELATO-ARQUIVO.md:878-880`**, histórico confirma a inversão:  
   “A SUA PERGUNTA DE 13/10 14:5x FOI RESPONDIDA PELO DP, E NA DIRECAO CONTRARIA […]”  
   “a leitura chegou e diz **NAO recebe** […] a cura dos **2 zeros de 12/10** […] esta **CANCELADA**: aqueles zeros eram o numero certo.”

7. **`app/docs/RELATO.md:12`**, pergunta atual, ainda pergunta:  
   “**lei: INTERMITENTE-EM-FERIADO (V3-RETOMA, 11:1x).** 1 colab intermitente convocado em feriado, 2 dias: 12,16 h (09) e 12,15 h (10). v2 paga 0 de HE100 feriado nesses dias (o motor intermitente limpa o calendario injetado, motor_calculo_v2.py:903-905), v3 paga em dobra. **PERGUNTA:** intermitente convocado em feriado recebe dobra (HE100) ou hora simples (CLT 452-A / CCT)? Ate a resposta: o caso fica de fora da cura a7088de8 (que nao o toca); a fila segue.”

**CORTES.md e casos E1:**

- **`app/docs/CORTES.md:410`**, 31/10 00:0x, V3-E1-C17:  
  “(b) L-031+L-032: **sem intervalo batido indeniza so o suprimido, HE 0; C17/C16/C06 PENDENTE ate o DP/contador**; DEGRAU_HE_100_MIN vira cadastro por convencao (fila)”.
- **`app/docs/CORTES.md:411`**, 31/10 00:4x, E1-VALIDACAO:  
  “**lote1 todos menos C05/C17 (DP)**; lote2 C02 RETEM (L-039), C12 RETEM por furo pos-encerramento (ADR D4, L-033), C13 nao retem (L-084), C14 -> L3-C01, C18 ausencia nao retem, C19 nao retem rotulo 'isento de ponto'; lote3 C02 escala_suspeita NAO retem (L-084, L-099/R2); 24x48 (L3 C11/C12) fora sem norma; sem tabela de traducao de vocabulario”.
- **`app/docs/BACKLOG.md:782`** descreve os assuntos: “C17 (sem intervalo: HE?), **C16 (intervalo 30 min batido: suprimido=30 pelo art.71 par.4 pos-2017?) e C06 (CCT Vigilantes x prorrogacao noturna)**; os 3 ficam PENDENTE no E1 ate a resposta”.
- **`app/docs/RELATO.md:82`** registra: “PENDENTE_DP **C05/C06/C16/C17** em skip com valores conferidos”.
- **`app/docs/RELATO.md:86`** descreve C05 apenas como “lote 1 contra a v2, **rubrica extra**”, “v3 verde, v2 vermelha”.
- **`app/docs/RELATO.md:91`** descreve C17 como “**comercial 08-17 sem intervalo**”, e registra resposta “C17 HE 0 intra 60”.
- **`app/docs/RELATO.md:88`** cita `docs/E1-PERGUNTA-AO-DP-CONTADOR.md` **“da copia”**.
- **`app/docs/RELATO.md:92`** cita a cópia `wt-v3e1-3010-2336` e o harness `ponto/tests/test_v3_e1_valor.py`.

**Limite importante:** não existem arquivos `docs/E1-*` nem `app/docs/E1-*` neste snapshot; `find .`, `rg --files --hidden` e listagens de `docs`/`app/docs` não os localizaram. Não existe aqui o texto integral dos casos C05/C06/C16/C17. Portanto **C05 ser feriado ou intermitente: sem fonte no repo**; os textos acima são as referências efetivamente disponíveis. Não extrapolar “rubrica extra” para feriado.

## 31/10 12:35 · sessao 15f636befbee · - · - · origem=Ronald (tmux)

Levantamento somente leitura.

**V3: `regras.do_dia` mantém o intermitente no caminho de trabalho comum e aplica a regra geral de feriado.**

- Identificação do regime: `app/ponto/calculador/alimentacao.py:406`:
  > `_interm = getattr(te, 'tipo_ciclo', None) == 'intermitente'`

  O vínculo do dia atualiza essa identificação em `app/ponto/calculador/alimentacao.py:479-485`:
  > `_interm_dia = getattr(_te_d, 'tipo_ciclo', None) == 'intermitente'`

  A chamada entrega calendário, flag e regime em `app/ponto/calculador/alimentacao.py:509-510`:
  > `feriados=_feriados, feriado_em_dobra=_fer_dobra,`
  > `intermitente=_interm_dia, dias_folga=_folga_dia,`

- O calendário passado à regra é a lista recebida pelo alimentador, `app/ponto/calculador/alimentacao.py:396`:
  > `_feriados = set(feriados or ())`

  O flag vem do motor obtido pela fábrica CCT, `app/ponto/calculador/alimentacao.py:378,391-394`:
  > `_mcct = get_motor_cct(c, ini, tipo_escala=te)`
  > `_cct = _cct_do_motor(_mcct) if _mcct is not None else None`
  > `_fer_dobra = None if _cct is None else _cct.feriado_em_dobra`

- Intermitente não recebe classificação de folga trabalhada. `app/ponto/calculador/regras.py:369-372`:
  > `**INTERMITENTE NUNCA E FOLGA TRABALHADA**`
  > `No intermitente a celula diz trabalha=False em TODO dia -- e o normal do regime, nao uma folga violada.`

  Retorno literal da função de folga, `app/ponto/calculador/regras.py:386-388`:
  > `_trab = float(trabalhado_min or 0)`
  > `if _trab <= 0 or intermitente:`
  > `    return 0.0, 0.0`

- Mesmo com `dia_de_trabalho=False`, o intermitente segue para o trabalho comum. `app/ponto/calculador/regras.py:657-660`:
  > `O INTERMITENTE CAI PARA O CAMINHO NORMAL [...] Entao aqui ele NAO retorna -- segue adiante.`
  > `if not intermitente and escala_certa is not None and dia is not None:`

  `app/ponto/calculador/regras.py:736-740`:
  > `if not intermitente:`
  > `    return Rubricas({}, _nd, det)`
  > `det['intermitente_segue_como_trabalho'] = True`
  > `val['horas_trabalhadas'] = trab / 60`

- Sem previsto, o excedente comum é zero, `app/ponto/calculador/regras.py:767-771`:
  > `_limite = float(previsto_min or 0) + min(suprimido, float(intervalo_declarado_min or 0))`
  > `extra = trab - _limite if previsto_min else 0.0`
  > `if extra <= TOLERANCIA_HE_MIN_DIA:`
  > `    extra = 0.0`

- **A dobra de feriado vem depois disso e não contém exceção `intermitente`.** `app/ponto/calculador/regras.py:774-789`:
  > `_dobra = 0.0`
  > `if feriados is not None and feriado_em_dobra is not None and dia is not None:`
  > `    _dobra = dobra_de_feriado_do_dia(dia, trab, feriados=feriados,`
  > `                                     feriado_em_dobra=feriado_em_dobra)`
  > `if _dobra > 0:`
  > `    val['horas_extras_100'] = _dobra / 60`
  > `    if 'horas_extras_100_feriado' not in _nd:`
  > `        val['horas_extras_100_feriado'] = _dobra / 60`
  > `    val['horas_extras_50'] = 0.0`
  > `    [...]`
  > `    return Rubricas(val, _nd, det)`

  Portanto as atribuições desse ramo são `horas_trabalhadas=trab/60`, `horas_extras_100=trab/60`, `horas_extras_100_feriado=trab/60`, `horas_extras_50=0`, quando o juiz devolve dobra; são rubricas/horas, não um valor monetário final. As atribuições estão em `app/ponto/calculador/regras.py:740,779-789`.

- Juiz compartilhado com v2: `app/ponto/calculador/regras.py:287-295`:
  > `_feriado_dobra_no_dia = MotorBase._feriado_dobra_no_dia`
  > `eh_feriado = MotorBase.eh_feriado`
  > `self.feriados = set(feriados or ())`
  > `self.feriado_em_dobra = bool(feriado_em_dobra)`

  `app/ponto/calculador/regras.py:344-347`:
  > `_ctx = _ContextoFeriado(feriados, feriado_em_dobra)`
  > `if float(trabalhado_min or 0) <= 0:`
  > `    return 0.0`
  > `return float(trabalhado_min) if _ctx._feriado_dobra_no_dia(dia) else 0.0`

  O juiz v2, `app/ponto/motor_calculo_v2.py:879`:
  > `return bool(self.feriado_em_dobra) and self.eh_feriado(dia)`

- Se faltarem calendário ou flag, HE100 e HE100 feriado ficam declaradas sem entrada, `app/ponto/calculador/regras.py:633-638`:
  > `if feriados is None or feriado_em_dobra is None:`
  > `    _nd['horas_extras_100'] = SEM_ENTRADA['horas_extras_100']`
  > `    [...]`
  > `    _nd['horas_extras_100_feriado'] = SEM_ENTRADA['horas_extras_100_feriado']`

  A causa literal está em `app/ponto/calculador/regras.py:529-531`:
  > `a regra existe (dobra_de_feriado_do_dia, o juiz do motor) e esta chamada nao recebeu o calendario de feriados nem o cadastro do ciclo -- passe feriados=/feriado_em_dobra=`

**Ponto crítico do v2 que repercute no v3: esvaziar `self.feriados` não desativa a consulta de feriado no banco.**

- Neste hash a classe fica em `app/ponto/motor_calculo_v2.py:3483-3495`; as linhas 903-905 são o corpo de `eh_feriado`, não a limpeza do calendário. Docstring, `app/ponto/motor_calculo_v2.py:3485-3488`:
  > `Intermitente-horista (Art. 452-A) — sem escala, sem marcos, sem jornada prevista. Apura SOMENTE horas cumpridas do par E/S (+ noturno legal).`
  > `Sem HE por estouro de jornada, sem feriado-100 automatico, sem atraso.`
  > `Herda de Motor12x36 apenas o pareamento E/S cruza-meia-noite.`

  Implementação, `app/ponto/motor_calculo_v2.py:3491-3495`:
  > `super().__init__(*args, **kwargs)`
  > `# neutraliza gatilhos de extra: jornada "infinita" e sem feriados`
  > `self.jornada_minutos = 24 * 60 * 365`
  > `self.feriados = set()`

  Consulta, `app/ponto/motor_calculo_v2.py:901-905`:
  > `"""Consulta feriado por contexto injetado; fallback mantém compatibilidade com banco."""`
  > `if data in self.feriados:`
  > `    return True`
  > `return is_feriado(data)`

- O único ciclo da lista simples é 12x36, `app/ponto/motor_calculo_v2.py:3537`:
  > `CICLOS_FERIADO_SIMPLES = frozenset({'12x36'})`

  Fábrica, `app/ponto/motor_calculo_v2.py:3607-3608`:
  > `if feriado_em_dobra is None:`
  > `    feriado_em_dobra = ciclo not in CICLOS_FERIADO_SIMPLES`

  Assim o ciclo literal `'intermitente'` recebe `True` nesse ramo, e a limpeza do conjunto não remove o fallback do banco; essas duas decisões estão em `app/ponto/motor_calculo_v2.py:3537,3607-3608,901-905`.

**V3 possui também uma releitura de ensaio por período.**

- `app/ponto/calculador/periodo.py:540-545`:
  > `def reler(resultado, regua_de, marcos=None, fora_por_lei=None, cct=None):`
  > `LEITURA DE ENSAIO, nao caminho de producao: serve a bateria e ao DIFF do passo 4.`

- Sem previsto, `app/ponto/calculador/periodo.py:410-413`:
  > `_prev = float(getattr(regua, 'previsto_min', 0) or 0)`
  > `if not _prev:`
  > `    return ExcessoV3(sem_previsto=True)`

  Os campos de excesso nascem zero, `app/ponto/calculador/periodo.py:367-371`:
  > `extra_min: float = 0.0`
  > `extra_50_min: float = 0.0`
  > `extra_100_min: float = 0.0`

- A dobra é julgada independentemente desse zero, `app/ponto/calculador/periodo.py:828-843`:
  > `_dobra = 0.0`
  > `if cct is not None:`
  > `    _dobra = dobra_de_feriado_do_dia(dia, _trab, feriados=cct.feriados,`
  > `                                     feriado_em_dobra=cct.feriado_em_dobra)`
  > `if _dobra > 0:`
  > `    _ult.minutos_extra = _dobra`
  > `    _ult.minutos_extra_50 = 0.0`
  > `    _ult.minutos_extra_100 = _dobra`
  > `    _ult.minutos_extra_100_feriado = _dobra`
  > `    _ult.hora_extra_100 = True`

**Integração atual: oráculo paralelo.**

- `app/ponto/services/fechamento.py:1233-1243` chama `rubricas_por_dia` a partir da competência `(2026, 10)` e passa `feriados=feriados`; `app/ponto/services/fechamento.py:1245-1263` lavra com `rubricas=_rub_o` e `versao='oraculo'`.
- Comentário literal, `app/ponto/services/fechamento.py:1214-1217`:
  > `a chave do DiaPago inclui versao, entao estas linhas NASCEM AO LADO das de versao='motor' [...] Nenhum leitor le 'oraculo' hoje -- colaboradores/services/calendario.py e folha/porta_export.py filtram versao='motor'`

**Teste localizado: intermitente comum, sem prova específica de feriado.** [nao verificado]

`app/ponto/tests/test_s5b_regra_pontualidade.py:347-354` passa 480 minutos, sem previsto, `dia_de_trabalho=False`, `intermitente=True`, e exige `horas_trabalhadas=8`, folga trabalhada zero e `intermitente_segue_como_trabalho=True`. A chamada não passa `feriados`/`feriado_em_dobra`; portanto esse teste não verifica a dobra do intermitente em feriado (`app/ponto/tests/test_s5b_regra_pontualidade.py:348-350`).

## 31/10 12:35 · sessao 25c8697c4180 · - · - · origem=Ronald (tmux)

Fontes encontradas, todas somente lidas:

- **Texto primário integral CLT art.452-A/§6 ou Lei605/49 art.9: sem fonte no repo.** As buscas por `adicionais legais`, `pagamento imediato`, `Ao final de cada`, `452-A`, `605` não localizaram a transcrição legal nem a lista de parcelas do §6. Há citações aos artigos em documentação interna, abaixo.
- **Conteúdo cadastrado de Sindicato.obs: sem fonte no repo.** Existe o campo `obs = models.TextField(blank=True)` em `app/core/models.py:40`, mas nenhum dump/fixture com seu valor foi localizado.
- `CLAUDE.md:442-443`: “A fundamentacao INTEIRA (clausulas literais + parecer) esta em Sindicato.obs e / em core/regua_cct.py; aqui fica so o que evita redescobrir do zero:”
- `CLAUDE.md:455-456`: “A CCT em PDF **nao esta no sistema** e o modelo Sindicato nao tem campo de / anexo. Enquanto nao tiver, o lastro e o texto em `obs`.”
- `CLAUDE.md:430-432` identifica a CCT: “CCT VIGILANTES LONDRINA”; “Sindicato #2 (Vigilantes de Londrina/PR), MTE PR000251/2026, vigencia / 01/02/2026-31/01/2028”. As cláusulas resumidas em `444-454` tratam hora noturna, adicional e janela 22h–05h; não mencionam intermitente/feriado.

Regra CCT e modelos:

- `app/core/regua_cct.py:44`: `'feriado_12x36_em_dobra': False,   # BUG 91: piso legal = hora simples (Art.59-A par.2)`
- `app/core/regua_cct.py:473-478`: “a dobra do feriado no 12x36 so volta por CLAUSULA CADASTRADA”; “Fora do 12x36 nao ha o que decidir aqui -- a Sumula 146 e o piso e o / `get_motor` ja resolve pelo ciclo; passar False para um 6x1 seria tirar / dele um direito que nenhuma CCT precisou conceder.” Ramo: `if _ciclo == '12x36' and r.get('feriado_12x36_em_dobra'):` → `kwargs.setdefault('feriado_em_dobra', True)`.
- `app/core/models.py:49-58`: “o piso LEGAL do feriado trabalhado / em 12x36 e HORA SIMPLES -- Art.59-A par.2 CLT, Sumula 444 cancelada”; “a / convencao coletiva PODE ser mais benefica e devolver a dobra; nenhuma das / nossas diz isso hoje”. Help text: “Fora isso, o 12x36 paga hora simples e a dobra vale so para 6x1/5x2/comercial.”
- `app/relatorios/pdf_espelho.py:764-766`: “50% = CLT art.59 §1 / CF art.7 XVI. 100% = descanso trabalhado / (Lei 605/49 art.9 + Sum.146 TST) + excedente por CCT. / Quantidade CRUA: majoracao e' da folha, nunca do ponto.”

Docstrings atuais:

- `app/ponto/motor_calculo_v2.py:3485-3488`: “Intermitente-horista (Art. 452-A) — sem escala, sem marcos, sem jornada / prevista. Apura SOMENTE horas cumpridas do par E/S (+ noturno legal). / Sem HE por estouro de jornada, sem feriado-100 automatico, sem atraso. / Herda de Motor12x36 apenas o pareamento E/S cruza-meia-noite.”
- Limpeza real em `app/ponto/motor_calculo_v2.py:3493-3495`: “neutraliza gatilhos de extra: jornada "infinita" e sem feriados”; `self.jornada_minutos = 24 * 60 * 365`; `self.feriados = set()`.
- **Atenção ao desencontro de linhas:** `app/ponto/motor_calculo_v2.py:901-905` contém `eh_feriado`, com `if data in self.feriados: return True; return is_feriado(data)`. A referência à limpeza nas linhas903–905 em `app/docs/RELATO.md:12` está desatualizada em relação ao arquivo atual.
- `app/ponto/motor_calculo_v2.py:862-865` corte literal: “FERIADO NAO entra / na L-086: segue a regra de feriado que ja existe no cadastro do ciclo (`feriado_em_dobra`; / 12x36 nao dobra pelo parecer do Art.59-A), com o dia do feriado sendo o dia de INICIO do / plantao.”
- `app/ponto/motor_calculo_v2.py:873-875`: “Plantao que comeca no feriado dobra; plantao que comeca na vespera e ENTRA pelo / feriado nao dobra, porque a hora pertence ao dia em que ele comecou.”

Histórico indispensável de cortes, embora fora de CORTES.md:

1. `app/docs/PROMPTS.md:1204`, delta13/10~17:0x: “intermitente que trabalha em feriado recebe a dobra (Lei 605 art.9 / Sumula 146 / art.452-A par.6) -- a docstring do MotorIntermitente se corrige para o codigo, e os 2 zeros de 12/10 (23,74 h, mesma empresa, mesmo ciclo, trabalha_em_feriado=True) sao bug de ESTRUTURA a curar na comp 10 com DIFF antes”.
2. `app/docs/PROMPTS.md:1213`, aval13/10~18:1x: “intermitente em feriado recebe dobra (452-A §6, Lei 605 art.9, Sum.146); docstring errada, os 2 zeros de 12/10 sao bug; leitura passa pelo DP antes de virar lei.”
3. **Corte posterior contrário**, `app/docs/PROMPTS.md:1231`,14/10~21:4x: “corte Ronald: SDF/intermitente em feriado NAO recebe dobra (DP 14/10); o cartao nao afirma pagamento em dobro.”
4. `app/docs/PROMPTS.md:1239`,15/10~11:5x: “intermitente segue pela docstring do MotorIntermitente (sem feriado-100 automatico). Ordem: O275 pousa, depois O277 le o flag. DIFF da dobra ja paga na 10 publicado antes; 09 exportada nao se toca.”
5. `app/docs/BACKLOG.md:1377`, O264 registra a inversão: “A LEITURA DO DP CHEGOU E INVERTEU ESTA OBRA”; “a docstring do `MotorIntermitente` **nao se corrige**”; “a cura dos **2 zeros de 12/10** [...] esta **CANCELADA** -- aqueles zeros sao o numero CERTO.”
6. `app/docs/BACKLOG.md:1390`, O277 registra a mesma inversão e orientação “intermitente pela docstring, **sem entrada nova no frozenset**”.
7. `app/docs/RELATO.md:12` reabre pergunta sob V3-RETOMA: “1 colab intermitente convocado em feriado, 2 dias: 12,16 h (09) e 12,15 h (10). v2 paga 0 de HE100 feriado nesses dias [...] v3 paga em dobra. PERGUNTA: intermitente convocado em feriado recebe dobra (HE100) ou hora simples (CLT 452-A / CCT)?”

As referências ao §6 acima **não transcrevem** as parcelas remuneração, férias+1/3,13º,RSR,adicionais legais. Para a enumeração literal pedida: **sem fonte no repo**.

## 31/10 12:35 · sessao 175c89d8e41c · GRADE-FOLGA · censo · origem=Code

1. **Chamadores no código de produção**

| Função chamada | Todos os chamadores encontrados fora dos testes | [nao verificado]
|---|---|
| `_aplicar_folgas_foto` | `montar_grade_prevista` — `app/escala/utils.py:768`; `montar_grade_prevista_periodo_por_turno` — `app/escala/utils.py:1787`. |
| `montar_grade_prevista` | `_montar_grade_prevista_periodo_legacy` — `app/escala/utils.py:993`. |
| `_montar_grade_prevista_periodo_legacy` | `montar_grade_prevista_periodo` — `app/escala/utils.py:2029`. |
| `montar_grade_prevista_periodo_por_turno` | `montar_grade_prevista_periodo` — `app/escala/utils.py:2027`; `_score_geometria` — `app/ponto/management/commands/detectar_par_relampago.py:136`. |
| `montar_grade_prevista_periodo` | `_julgar_colab_corpo` — `app/ponto/services/cartorio.py:931`; `Command.handle` — `app/ponto/management/commands/reconciliar_grade.py:118`; `grade_espelho_janela` — `app/escala/utils.py:2277`. |

Duas referências executáveis **inspecionam a assinatura, sem executar o builder**: `censo_col900` lê o default de `tol_min` do single; `tripwire_celulas` lê o mesmo parâmetro do dispatcher. — `app/ponto/management/commands/censo_col900.py:53`; `app/ponto/management/commands/tripwire_celulas.py:124`.

O dispatcher escolhe o builder por turno quando `GRADE_POR_TURNO` é verdadeiro; caso contrário, chama o legacy. — `app/escala/utils.py:2026`; `app/escala/utils.py:2029`.

2. **Quem lê o resultado de `montar_grade_prevista_periodo`**

O overlay escreve `tipo_dia='folga'`, substitui `celulas` por uma lista de `None` e apaga `orfas`. **Ele não zera os campos de minutos nesse trecho.** — `app/escala/utils.py:1117`; `app/escala/utils.py:1118`; `app/escala/utils.py:1119`; `app/escala/utils.py:1120`.

| Chamador direto | Lê `tipo_dia`? | Lê as células? Para decidir o quê? |
|---|---|---|
| Cartório | **Sim.** No ramo nunca-bateu, distingue cobertura integral de ausência/falta. O classificador também recebe o dia e lê seu tipo. — `app/ponto/services/cartorio.py:997`; `app/ponto/services/cartorio.py:1207`; `app/ponto/supra_juiz.py:199`. | **Sim.** Conta batidas casadas, calcula lâmpadas/missing, classifica o dia e lavra ata/veredito/status. — `app/ponto/services/cartorio.py:1170`; `app/ponto/services/cartorio.py:564`; `app/ponto/services/cartorio.py:575`; `app/ponto/services/cartorio.py:1557`. |
| `reconciliar_grade` | **Não lê diretamente o `tipo_dia` dos dias retornados.** O processamento extrai missing das células; sua leitura de “dia sem trabalho” vem separadamente da `CelulaDia`, pelo juiz. — `app/ponto/management/commands/reconciliar_grade.py:151`; `app/ponto/management/commands/reconciliar_grade.py:280`. | **Sim.** Missing vencidos compõem os furos; nenhum missing põe o dia em `avaliado_limpo`. Isso alimenta planos de criação e retratação de chamados. — `app/ponto/management/commands/reconciliar_grade.py:154`; `app/ponto/management/commands/reconciliar_grade.py:169`; `app/ponto/management/commands/reconciliar_grade.py:179`; `app/ponto/management/commands/reconciliar_grade.py:258`; `app/ponto/management/commands/reconciliar_grade.py:266`. |
| `grade_espelho_janela` | **Repassa o campo.** Chama o dispatcher, aplica clipping e retorna a grade. — `app/escala/utils.py:2277`; `app/escala/utils.py:2278`; `app/escala/utils.py:2279`. | O clipping substitui células por `None` e apaga órfãs em dias futuros ou anteriores ao piso; os consumidores abaixo fazem as demais leituras. — `app/escala/utils.py:942`; `app/escala/utils.py:944`; `app/escala/utils.py:946`; `app/escala/utils.py:948`. |

No cartório, essas leituras têm consequências além da apresentação:

- **Julgamento:** tipo e células participam das condições de cobrança, furo parcial e cobrança em dia sem trabalho. — `app/ponto/supra_juiz.py:281`; `app/ponto/supra_juiz.py:348`; `app/ponto/supra_juiz.py:379`; `app/ponto/supra_juiz.py:443`.
- **Ata:** `ata_do_dia` grava o tipo, os minutos e as lâmpadas derivadas das células; a lavra entrega essa ata à `CelulaDia`. — `app/ponto/services/cartorio.py:565`; `app/ponto/services/cartorio.py:567`; `app/ponto/services/cartorio.py:568`; `app/ponto/services/cartorio.py:1559`.
- **Chamados:** os códigos controlam emissão de furo inteiro/parcial; a ata deste julgamento também participa da reconciliação por marcos. — `app/ponto/services/cartorio.py:1489`; `app/ponto/services/cartorio.py:1508`; `app/ponto/services/cartorio.py:1531`; `app/ponto/services/cartorio.py:1544`; `app/ponto/services/cartorio.py:1182`; `app/ponto/services/cartorio.py:1281`.
- **Status:** `fatos_do_status` lê tipo e células da grade julgada, e esses fatos são passados à lavra. — `app/ponto/status_dia.py:143`; `app/ponto/status_dia.py:146`; `app/ponto/services/cartorio.py:1563`.

Os consumidores de `grade_espelho_janela` são:

| Consumidor | Leitura e destino |
|---|---|
| Espelho | Chama o builder em `_pela_grade`; usa esse caminho para o trecho anterior à competência ou como fallback de ata agregada. Lê tipo para `folga`, células não nulas para `previsto_canon` e células para realizado/missing. — `app/ponto/services/espelho.py:503`; `app/ponto/services/espelho.py:541`; `app/ponto/services/espelho.py:571`; `app/ponto/services/espelho.py:690`; `app/ponto/services/espelho.py:695`; `app/ponto/services/espelho.py:700`. |
| Tela do espelho | Recebe `folga`, `previsto_canon` e `realizado_grade`; o template usa esses campos para desenhar o dia. — `app/ponto/services/espelho.py:781`; `app/ponto/services/espelho.py:782`; `app/ponto/services/espelho.py:783`; `app/templates/ponto/espelho.html:166`; `app/templates/ponto/espelho.html:213`; `app/templates/ponto/espelho.html:226`. |
| Simulação | `grade_dia` retorna o dia do builder. `dia_perfeito` exige trabalho, ausência de órfãs/missing e compatibilidade dos tipos das células. Isso participa da recusa de materialização e da decisão de flip automático/humano. — `app/ponto/services/simular_dia.py:7`; `app/ponto/services/simular_dia.py:12`; `app/ponto/services/simular_dia.py:18`; `app/ponto/services/simular_dia.py:21`; `app/ponto/services/veredito_celula.py:221`; `app/ponto/services/flip_auto.py:303`; `app/ponto/services/flip_auto.py:305`. |
| `mapa_divergencia` | Faz duas chamadas, inclui tipo e status/missing das células nas assinaturas e compara divergências. — `app/ponto/management/commands/mapa_divergencia.py:104`; `app/ponto/management/commands/mapa_divergencia.py:105`; `app/ponto/management/commands/mapa_divergencia.py:120`; `app/ponto/management/commands/mapa_divergencia.py:131`; `app/ponto/management/commands/mapa_divergencia.py:208`. |
| PDF do espelho | Nas chamadas próprias, lê órfãs e células marcadas como isenção; não decide diretamente pelo `tipo_dia`. As órfãs afetam a identificação de entrada espúria/turno aberto e a marca de batida órfã. Os dias desenhados vêm do serviço de espelho. — `app/relatorios/pdf_espelho.py:471`; `app/relatorios/pdf_espelho.py:509`; `app/relatorios/pdf_espelho.py:539`; `app/relatorios/pdf_espelho.py:548`; `app/relatorios/pdf_espelho.py:551`; `app/relatorios/pdf_espelho.py:594`; `app/relatorios/pdf_espelho.py:598`; `app/relatorios/pdf_espelho.py:605`. |

**Folha/dinheiro:** `grade_do_fechamento` consulta `grade_da_celula`, que lê a ata gravada. A ligação com o dispatcher passa pela ata lavrada pelo cartório; o caminho financeiro está detalhado no item 4. — `app/folha/export.py:324`; `app/folha/export.py:327`; `app/escala/services/leitor_celula.py:331`; `app/ponto/services/cartorio.py:1559`.

Há também leitura das células **dentro do builder por turno, depois do overlay**: ele monta atas em memória com suas lâmpadas, entrega essas cópias ao juiz de turnos e obtém o realizado final desse juiz. — `app/escala/utils.py:1832`; `app/escala/utils.py:1838`; `app/escala/utils.py:1985`; `app/escala/utils.py:1998`.

3. **Onde se decide se uma `FolgaDia` vale**

Na tabela, “outro vínculo” significa diferente do vínculo escolhido para aquele dia; o resultado indicado é o tratamento da foto naquele caminho, sujeito às demais condições do próprio código.

| Sítio | Calendário em outro vínculo | Manual em outro vínculo do mesmo colaborador |
|---|---|---|
| `_aplicar_folgas_foto` | Busca pelo `esc` recebido, sem verificar origem ou vigência. Se receber o vínculo da foto, aplica-a mesmo sem conferir se ele é o vínculo daquele dia. — `app/escala/utils.py:1109`; `app/escala/utils.py:1116`. | Mesmo comportamento: não distingue origem. — `app/escala/utils.py:1109`; `app/escala/utils.py:1118`. |
| Integração do overlay no builder por turno | **Pode aplicar a foto ao dia de outro vínculo:** basta o vínculo da foto responder por algum dia da janela; o overlay recebe todo `dias_out`. — `app/escala/utils.py:1260`; `app/escala/utils.py:1261`; `app/escala/utils.py:1785`; `app/escala/utils.py:1787`. | Exatamente o mesmo alcance. — `app/escala/utils.py:1109`; `app/escala/utils.py:1787`. |
| Builder legacy | Escolhe vínculos por dia, constrói segmentos, chama o single por segmento e conserva apenas os dias daquele segmento. — `app/escala/utils.py:970`; `app/escala/utils.py:975`; `app/escala/utils.py:993`; `app/escala/utils.py:997`. | Mesmo tratamento; o overlay do single filtra pelo vínculo recebido. — `app/escala/utils.py:768`; `app/escala/utils.py:1110`. |
| `EscalaColaborador.eh_dia_trabalho_calculado` | Consulta somente a foto do `self`, por `folgas[self.pk]` ou filtro equivalente; foto de outro vínculo não entra. Não distingue origem nem testa vigência do `self` nesse cálculo. — `app/escala/models.py:1252`; `app/escala/models.py:1257`; `app/escala/models.py:1281`. | Também não entra enquanto estiver presa ao outro vínculo. — `app/escala/models.py:1252`; `app/escala/models.py:1281`. |
| `EscalaColaborador.eh_dia_trabalho` | Havendo célula do colaborador/data, retorna `cel.trabalha`, sem consultar a foto; só sem célula cai no cálculo acima. — `app/escala/models.py:1067`; `app/escala/models.py:1071`; `app/escala/models.py:1073`. | Mesmo comportamento. — `app/escala/models.py:1072`; `app/escala/models.py:1073`. |
| `fatos_do_periodo` / `fatos_do_dia` | Carrega fotos por pessoa/data, mas **descarta a selecionada** se tiver origem de calendário e seu vínculo diferir do vínculo diário, inclusive quando não há vínculo diário. — `app/ponto/precedencia.py:236`; `app/ponto/precedencia.py:307`; `app/ponto/precedencia.py:313`; `app/ponto/precedencia.py:315`. | **Conserva a foto**, pois a guarda exige origem preenchida; ela entra como fato de faixa `folga`, sujeito à precedência das demais faixas. — `app/ponto/precedencia.py:313`; `app/ponto/precedencia.py:337`; `app/ponto/precedencia.py:338`. |
| Alimentação especial do calendário | Quando `_cobre=True`, carrega apenas fotos do `ec` fornecido e passa `{data: True}` à precedência. Foto de outro vínculo não entra nesse mapa; origem e id não são preservados nele. Quando `_cobre=False`, deixa a precedência carregar normalmente. — `app/colaboradores/services/calendario.py:593`; `app/colaboradores/services/calendario.py:602`; `app/colaboradores/services/calendario.py:606`; `app/colaboradores/services/calendario.py:607`. | Também fica fora no caminho `_cobre=True`; no outro caminho vale a carga por pessoa descrita acima. — `app/colaboradores/services/calendario.py:607`; `app/colaboradores/services/calendario.py:608`. |
| `_folga_avulsa` → `veto_de_lancamento` | **Conta por colaborador/data**, sem filtro de vínculo, origem ou vigência. Pode vetar nova folga e outros lançamentos; férias atravessa a folga. — `app/ponto/precedencia.py:165`; `app/ponto/precedencia.py:449`; `app/ponto/precedencia.py:458`; `app/ponto/precedencia.py:462`. | Mesmo tratamento: conta e pode vetar. — `app/ponto/precedencia.py:166`; `app/ponto/precedencia.py:459`; `app/ponto/precedencia.py:462`. |
| `declarar_dia`: `_folga_do_dia`, `o_dia_ja_e_folga`, `_recusa_do_dia` | A consulta local usa o vínculo diário e não encontra foto de outro vínculo. Se própria, calendário recusa com “edite o calendário”. Depois existe o veto por pessoa, que pode encontrar a foto do outro vínculo. — `app/ponto/services/declarar_dia.py:58`; `app/ponto/services/declarar_dia.py:70`; `app/ponto/services/declarar_dia.py:91`; `app/ponto/services/declarar_dia.py:109`; `app/ponto/services/declarar_dia.py:114`. | Não conta localmente como “já é folga” se estiver em outro vínculo; ainda pode ser encontrada pelo veto subsequente por pessoa. — `app/ponto/services/declarar_dia.py:70`; `app/ponto/services/declarar_dia.py:111`; `app/ponto/services/declarar_dia.py:114`. |

Em `fatos_do_periodo`, havendo várias fotos na mesma data, fica a primeira por `pk`. Se essa primeira for calendário de outro vínculo e for descartada, o código não procura a próxima foto nessa seleção. — `app/ponto/precedencia.py:237`; `app/ponto/precedencia.py:238`; `app/ponto/precedencia.py:298`; `app/ponto/precedencia.py:313`.

No cálculo do vínculo, uma foto no mês impede o ramo de âncora; foto no próprio dia produz `False`. Fora da foto, usa o ciclo ou, no ramo de ciclo desconhecido, a paridade da foto mais próxima daquele vínculo. — `app/escala/models.py:1260`; `app/escala/models.py:1267`; `app/escala/models.py:1288`; `app/escala/models.py:1290`; `app/escala/models.py:1327`; `app/escala/models.py:1335`.

Os sítios relacionados à vigência e ao dono da foto são:

| Sítio | Regra |
|---|---|
| `folgas_do_periodo` | Carrega por ids de vínculo, amplia o intervalo para meses inteiros e devolve datas separadas por vínculo; não conserva origem nem aplica vigência. Ambas as origens permanecem no conjunto do vínculo original. — `app/escala/alimentacao.py:60`; `app/escala/alimentacao.py:66`; `app/escala/alimentacao.py:70`. |
| `vinculo_cobre_a_data` | Exige início até o dia, fim nulo ou posterior/igual e vigência declarada: ativo ou fim preenchido. — `app/escala/alimentacao.py:164`; `app/escala/alimentacao.py:182`; `app/escala/alimentacao.py:184`. |
| `vinculo_do_dia` | Com geradora na célula, procura esse vínculo e confere limites de início/fim; sem geradora, escolhe entre os que cobrem o dia por máximo `(ativa, data_inicio)`. — `app/escala/alimentacao.py:237`; `app/escala/alimentacao.py:247`; `app/escala/alimentacao.py:250`; `app/escala/alimentacao.py:253`; `app/escala/alimentacao.py:254`. |
| `reparentar_folga` | Calendário retorna `'calendario'`, sem migrar. Manual permanece se o vínculo original ainda cobre a data, mesmo que outro seja escolhido para o dia. Se não cobre, procura o vínculo diário; sem alvo ou com foto no alvo, não migra. Com alvo disponível e aplicação habilitada, muda o vínculo. — `app/escala/folgas.py:405`; `app/escala/folgas.py:408`; `app/escala/folgas.py:414`; `app/escala/folgas.py:418`; `app/escala/folgas.py:420`; `app/escala/folgas.py:435`. |
| Reparentamento em lote/comando/signal | O lote e o comando selecionam somente manuais cujo vínculo não cobre o dia. O signal aciona esse processo na criação/mudança de vigência. — `app/escala/folgas.py:453`; `app/escala/folgas.py:457`; `app/escala/management/commands/reparentar_folgas.py:28`; `app/escala/management/commands/reparentar_folgas.py:34`; `app/escala/signals.py:185`; `app/escala/signals.py:191`. |

Outros leitores diretos, de tela, edição ou diagnóstico:

| Sítio | Tratamento das duas origens e de outro vínculo |
|---|---|
| Plano de folgas, `_grid_ctx` | Indexa por vínculo/dia; foto gravada aparece antes do cálculo. Ambas as origens presas em outro vínculo ficam na linha desse outro vínculo, se ele estiver entre os ativos carregados. — `app/escala/views_plano_folgas.py:55`; `app/escala/views_plano_folgas.py:63`; `app/escala/views_plano_folgas.py:70`; `app/escala/views_plano_folgas.py:72`. |
| Toggle do plano | Consulta vínculo ativo + data; foto de outro vínculo não é encontrada localmente. Calendário próprio bloqueia edição. — `app/escala/views_plano_folgas.py:109`; `app/escala/views_plano_folgas.py:112`; `app/escala/views_plano_folgas.py:113`. |
| `lancar_folga` / `_foto_do_dia` | Consultam vínculo + data; calendário próprio bloqueia alteração. Outra foto fica fora da consulta local, mas a criação chama o veto por pessoa. — `app/escala/folgas.py:254`; `app/escala/folgas.py:255`; `app/escala/folgas.py:258`; `app/escala/folgas.py:274`; `app/escala/folgas.py:275`; `app/escala/folgas.py:283`. |
| `defeito_folga_nao_definida` | Em 6x1 sem folga semanal, qualquer foto do próprio vínculo no mês afasta o defeito; ambas as origens em outro vínculo não contam. — `app/escala/models.py:1218`; `app/escala/models.py:1220`. |
| Diagnóstico de escala / relatório de furos | Contam ou verificam foto do vínculo no mês, sem distinguir origem; foto de outro vínculo não satisfaz a consulta. — `app/ponto/services/diagnostico_escala.py:102`; `app/relatorios/furos.py:112`. |
| `fase_conflitante` | Lê fotos de cada vínculo ativo, sem filtro de origem/vigência por data; compara foto exata ou paridade da mais próxima com a âncora. Fotos de outro vínculo não entram no conjunto daquele vínculo. — `app/escala/services/fase_conflitante.py:55`; `app/escala/services/fase_conflitante.py:57`; `app/escala/services/fase_conflitante.py:71`; `app/escala/services/fase_conflitante.py:74`; `app/escala/services/fase_conflitante.py:80`. |
| `rejulgar_ausencia_em_folga` | Seleciona pares pessoa/data por foto da empresa/janela, sem distinguir origem ou vínculo diário. Portanto ambas as fotos em outro vínculo podem selecionar o dia para o cruzamento com ausência e eventual rejulgamento. — `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:55`; `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:60`; `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:63`; `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:72`. |
| Auditoria C5 | Considera a foto se o vínculo original cobre a data **ou** outro candidato não resulta em `'nenhum'`; não distingue origem. Assim ambas em vínculo diferente podem contar como contradição se a célula trabalha e a competência não foi exportada. — `app/chamados/management/commands/auditar_invariantes_chamados.py:1532`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1535`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1542`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1546`. |
| `vinculo_que_cobre`, usado pelo C5 | Filtra vigência, exclui o vínculo indicado e pode responder único, nenhum ou ambíguo. O C5 só exclui `'nenhum'`, aceitando também `'ambiguo'`. — `app/escala/folgas.py:369`; `app/escala/folgas.py:383`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1536`. |
| `divergencia_foto_x_calendario` | Compara fotos do vínculo/mês e separa calendário/manual para calcular falta/sobra; fotos em outro vínculo não entram. — `app/escala/folgas.py:101`; `app/escala/folgas.py:104`; `app/escala/folgas.py:107`; `app/escala/folgas.py:108`. |
| `gerar_foto_folgas` | No vínculo/mês recebido, preserva manuais, remove fotos de calendário e recria as datas do calendário sem sobrepor manuais. Fotos em outro vínculo ficam fora desses filtros. — `app/escala/folgas.py:165`; `app/escala/folgas.py:169`; `app/escala/folgas.py:174`; `app/escala/folgas.py:175`. |

4. **Quem lê `ata['tipo_dia']` da `CelulaDia`**

As leituras diretas da chave são:

| Leitor direto | Para quê |
|---|---|
| `grade_da_celula` | Usa `ata.get('tipo_dia')` como tipo do dia; somente se ausente/falso usa `cel.trabalha` como fallback. — `app/escala/services/leitor_celula.py:331`; `app/escala/services/leitor_celula.py:344`; `app/escala/services/leitor_celula.py:345`; `app/escala/services/leitor_celula.py:359`. |
| `dia_sem_trabalho` | Retorna verdadeiro se `cel.trabalha is False` **ou** se a ata tem tipo preenchido diferente de `'trabalho'`. — `app/chamados/juizes.py:407`; `app/chamados/juizes.py:408`. |
| `medir_ata_fantasma` | Seleciona tipo folga sem lâmpadas; conta atas com `n_celulas` preenchido e separa as exportadas. — `app/ponto/management/commands/tripwire_celulas.py:342`; `app/ponto/management/commands/tripwire_celulas.py:344`; `app/ponto/management/commands/

[truncado em 20000 de 26092 caracteres]

## 31/10 12:32 · sessao 82a08d920812 · - · - · origem=Ronald (tmux)

Censo do item 4 — leituras diretas da chave na ATA:

| Leitor | Uso |
|---|---|
| `app/escala/services/leitor_celula.py:344` | `grade_da_celula` usa `ata.get('tipo_dia')` como `dias[].tipo_dia`; só quando ausente/falso usa `cel.trabalha` como fallback (`:345`). Consulta `CelulaDia` em `:325`, obtém ATA em `:331`, devolve tipo em `:359`. |
| `app/chamados/juizes.py:407` | `dia_sem_trabalho`: verdadeiro se `cel.trabalha is False` **ou** tipo presente na ATA diferente de `trabalho` (`:408`). |
| `app/ponto/management/commands/tripwire_celulas.py:341` | `medir_ata_fantasma` seleciona ATA `tipo_dia == folga` sem lâmpadas (`:342`), conta `n_celulas > 0` como fantasma e separa competências exportadas (`:344-349`). |

Consumidores indiretos do tipo devolvido por `grade_da_celula`:

| Consumidor | Decisão sustentada pelo código |
|---|---|
| `app/escala/services/leitor_celula.py:384` | Só dias de `ausencia` recebem apuração de cobertura parcial (`:389-397`). |
| `app/escala/services/leitor_celula.py:366` → `:188` | Isenção chama `aplicar_isencao_no_dia`; este usa tipo `trabalho` para marcar `dia['isencao']` (`app/escala/utils.py:850-851`). |
| `app/folha/export.py:324-327` | `grade_do_fechamento` lê a ATA via `grade_da_celula`, para janela da competência. |
| `app/ponto/services/fechamento.py:981-982` | Obtém a grade da ATA pelo leitor acima. Distingue ausência de célula dentro do vínculo (`:1011-1013`), conta previsto somente de `trabalho`/`ausencia` (`:1014-1018`), deriva mapas de abono/previsto/realizado (`:1028-1032`), grava previsto/dias (`:1051-1054`). |
| `app/ponto/services/dia_pago.py:385-399` | `por_dia_da_grade`: universo `trabalho`/`ausencia`; ausência abona `previsto - min(realizado, previsto)`; previsto e realizado são mapas por dia. É chamado pelo fechamento (`app/ponto/services/fechamento.py:1028-1032`) e por `retratar_exportada` (`app/ponto/services/dia_pago.py:462`), que escreve os três campos de `DiaPago` (`:474-481`). |
| `app/folha/export.py:1069-1086` | Fallback de prontidão: filtra `trabalho`/`ausencia`, soma previsto, abona ausência e soma realizado com teto diário somente de trabalho. Caminho de snapshot lê os campos materializados em `:1019-1024`, `:1050-1053`. |
| `app/folha/causas.py:40-42` | `classificar_causa` conta trabalho e furos; folga com órfãs pode classificar `dias_trocados` (`:100-106`). Chamado pelo fechamento com a grade da ATA (`app/ponto/services/fechamento.py:1045-1048`). |
| `app/folha/porta_export.py:133-145` | Auditoria `_gravado_x_grade`: deriva os mesmos mapas da ATA e compara a soma com lavratura e escalar do fechamento. |
| `app/ponto/services/espelho.py:530` | Lê e remapeia ATA; tipo `folga` vira booleano em `:690`, devolvido em `:781`. Template imprime “Folga” (`app/templates/ponto/espelho.html:166`). |
| `app/relatorios/furos.py:141-153` | Relatório ignora `fora_vinculo`/`sem_celula`; qualquer tipo diferente de trabalho vira estado `folga`, sem procurar furos. |
| `app/relatorios/furos.py:217-225` | `dias_100_em_branco` só examina trabalho sem `sem_ancora`, para retornar dias totalmente furados. |
| `app/relatorios/furos.py:236-250` | `furos_por_dia` só examina trabalho sem `sem_ancora`, retorna horários faltantes. |
| `app/inteligencia/calculadores.py:39-67` | Métrica de preenchimento só conta trabalho, com julgamento das células. |
| `app/inteligencia/calculadores.py:136-153` | Métrica de marcas previstas/batidas só conta trabalho. |
| `app/inteligencia/calculadores.py:324-325` | Passa dias da ATA a `template_suspeito`, que só usa trabalho para calcular desvio (`app/ponto/propositor.py:209-216`). |
| `app/ponto/management/commands/propor_reorganizacao.py:65-71` | Passa ATA a `template_suspeito` e `dia_degenerado`; este recusa tipos diferentes de trabalho (`app/ponto/propositor.py:36-37`). |
| `app/escala/services/furos_diarios.py:36-47` | Só trabalho entra na apuração de previstos, furos e órfãs. |
| `app/ponto/services/bordas_realizado.py:68-88` | Só trabalho entra nos contadores de divergência ATA × juiz do realizado e bordas. |
| `app/ponto/worklist.py:18-26` | Tipo diferente de trabalho retorna gap desconhecido com motivo “dia de … pela celula”; trabalho examina lâmpadas apagadas. |
| `app/chamados/services/disputa_emissao.py:66-70` | `_data_furo_da_grade` só confirma data do turno se o tipo for trabalho. |
| `app/chamados/services/declaracao_texto.py:82-98` | `_marcos_faltantes` distingue sem célula/fora do vínculo e só cobra marcos em trabalho. |
| `app/chamados/services/materializacao.py:217-225` | `_marcos_da_regua` retorna vazio em sem célula/fora do vínculo; demais tipos devolvem horas dos marcos. |
| `app/ponto/management/commands/supra_juiz.py:116` e `app/ponto/management/commands/emitir_furo_retroativo.py:59,99-109` | Leem ATA e a passam ao classificador. `app/ponto/supra_juiz.py:199` lê o tipo; usa trabalho para realizado zero (`:258`), trabalho ou ausência parcial para cobrança (`:281`), tipo sem trabalho para cobrança indevida (`:443-447`), trabalho para realizado inflado (`:448-451`) e órfãs (`:463-472`). |
| `app/ponto/management/commands/mapa_divergencia.py:98,108` | Usa grade da ATA como lado folha; inclui tipo nas assinaturas (`:131,143,155`) e conta divergência por tipo (`:228-229`). |

Consumidores do juiz direto `dia_sem_trabalho`:

- `_celula_furada` consulta esse juiz e, em dia sem trabalho, consulta a véspera antes de retornar falso: `app/chamados/juizes.py:443-449`.
- A premissa do chamado considera o dia regularizado quando esse juiz diz sem trabalho, exceto módulos `turno_aberto*`: `app/chamados/models.py:1227-1233`.
- `reconciliar_grade` usa esse juiz para pôr dias indefinidos no conjunto `mortos`: `app/ponto/management/commands/reconciliar_grade.py:280-283`; retratação exige todas as datas mortas (`:292`).

Separação importante para a consolidação: `status_dia.fatos_do_status` lê o dict da **grade do julgamento**, explicitamente `montar_grade_prevista_periodo`, não o leitor da ATA (`app/ponto/status_dia.py:124-149`). `simular_dia.grade_dia` usa `grade_espelho_janela` (`app/ponto/services/simular_dia.py:6-8`), portanto `simular_dia:12` e `veredito_celula:221` não devem ser atribuídos automaticamente à ATA.

Não encontrei leituras adicionais de `tipo_dia` por SQL/`ata__tipo_dia`, JavaScript ou templates literais; busca estática, sem prova de ausência dinâmica. [não verificado]

## 31/10 12:31 · sessao 42ef32b09afc · - · - · origem=Ronald (tmux)

Inventário do item 3, sem alterações.

**Decisão operacional sobre o dia**

| Sítio | Critério e efeito | Calendário em outro vínculo | Manual em outro vínculo |
|---|---|---|---|
| `app/escala/utils.py:1101` `_aplicar_folgas_foto` | Busca `FolgaDia` por **vínculo passado + intervalo**, sem testar origem/vigência; muda `tipo_dia='folga'`, apaga células e órfãs (`:1109-1120`). | Ignorada se outro que o `esc` passado; aceita se esse vínculo for passado, mesmo fora da vigência naquele dia (`:1109-1119`). | Exatamente igual: nenhuma distinção por origem (`:1109-1119`). |
| `app/escala/utils.py:1785` integração do overlay no builder por turno | Aplica o overlay sobre **todo `dias_out`** para cada vínculo que responde por **algum dia** da janela; o conjunto vem do mapa diário (`:1254-1261`, `:1785-1788`). | Pode virar folga em dia de outro vínculo se o vínculo da foto responder por qualquer outro dia da janela; não existe guarda por vínculo daquele dia dentro do overlay (`:1261`, `:1785-1787`, `:1114-1119`). | Mesmo comportamento (`:1109-1119`, `:1785-1787`). |
| `app/escala/models.py:1223` `EscalaColaborador.eh_dia_trabalho_calculado` | Foto pertence ao **self**: pré-carga usa `folgas[self.pk]`; consultas usam `escala_colaborador=self`. Data na foto faz `base=False` (`:1251-1259`, `:1280-1288`). Foto do mês vence a âncora; fora da foto usa ciclo ou, se ciclo desconhecido, paridade da foto mais próxima desse mesmo vínculo (`:1260-1267`, `:1290-1292`, `:1322-1335`). | Ignora foto de outro vínculo; não testa origem nem vigência do `self` dentro desta função (`:1251-1259`, `:1280-1281`, `:1327-1332`). | Ignora também enquanto presa em outro vínculo (`mesmas linhas`). |
| `app/escala/models.py:1048` `EscalaColaborador.eh_dia_trabalho` | Se há célula do colaborador/dia, retorna diretamente `cel.trabalha`; só sem célula consulta cálculo acima (`:1066-1073`). | Não consulta foto quando há célula; sem célula vale a restrição ao `self` do cálculo (`:1071-1073`, `:1257-1259`). | Igual (`mesmas linhas`). |
| `app/ponto/precedencia.py:212` `fatos_do_periodo` / `fatos_do_dia` | Por padrão carrega fotos de **todos os vínculos do colaborador**, por data; em colisão fica a primeira por pk (`:234-238`). Escolhe vínculo diário via `vinculo_do_dia` (`:307`), descarta foto de calendário cujo vínculo diverge (`:313-315`), mantém demais e acrescenta faixa folga (`:337-338`). `fatos_do_dia` delega ao período (`:209`). | Descarta **a foto selecionada** quando `calendario_origem_id != None` e `escala_colaborador_id != ec.pk`; também descarta se `ec=None` (`:313-315`). Como só a primeira por pk foi selecionada, não procura segunda foto depois do descarte (`:236-238`, `:298`, `:313-315`). | Mantém mesmo presa a outro vínculo ou sem vínculo diário, pois a guarda exige calendário preenchido; entra entre fatos presentes como folga, sujeita às outras faixas de precedência (`:313-315`, `:332-350`). |
| `app/colaboradores/services/calendario.py:592` alimentação especial de `fatos_do_periodo` | Carrega fotos apenas do `ec` já carregado. Se ele cobre ambas pontas da janela, passa `vinculos=[ec]`, foto desse ec e `folgas_lancadas={data: True}`; caso contrário passa `None` e deixa a precedência carregar (`:592-608`). | Foto de outro ec não entra no mapa quando `_cobre=True`; o mapa de `True` também não conserva origem/id para a guarda da precedência (`:593-608`, `app/ponto/precedencia.py:313`). | Também não entra enquanto presa em outro ec no caminho `_cobre=True`; no outro caminho vale carga por pessoa da precedência (`:607-608`, `app/ponto/precedencia.py:234-238`). |
| `app/ponto/precedencia.py:161` `_folga_avulsa`, usada por `veto_de_lancamento` | Busca por **colaborador + data**, sem vínculo/origem/vigência (`:165-166`). O veto recusa nova folga e outros lançamentos perante foto existente; férias atravessa a folga (`:449-466`). | Conta e pode vetar: nenhuma guarda para calendário de outro vínculo neste caminho (`:165-166`, `:458-466`). | Conta e pode vetar, igualmente (`mesmas linhas`). |
| `app/ponto/services/declarar_dia.py:61` `_folga_do_dia`, `o_dia_ja_e_folga`, `_recusa_do_dia` | Resolve vínculo diário por `escala_vigente`, depois busca foto **desse ec e data** (`:56-70`, `:90-91`, `:105-108`). Foto de calendário recusa com “edite o calendário”; manual recusa com “já é folga” (`:109-112`). | Foto de outro ec não é encontrada localmente (`:70`). Ainda pode ser apanhada pelo veto subsequente, que consulta por pessoa (`:113-116`, `app/ponto/precedencia.py:458-466`). | Mesma diferença: não conta como “já é folga” local se presa a outro ec, mas veto subsequente consulta pessoa (`mesmas linhas`). |

**Vigência e mudança de dono da foto**

- `app/escala/alimentacao.py:49-70` `folgas_do_periodo` carrega datas por ids dos vínculos pedidos, arredondando a janela para meses inteiros; conserva a separação por vínculo e **não conserva origem**, nem aplica vigência da foto (`:60-70`). Foto manual e calendário em outro vínculo ficam no balde desse outro vínculo (`:66-70`).
- `app/escala/alimentacao.py:173-184` `vinculo_cobre_a_data` exige início não nulo e anterior/igual ao dia, fim nulo ou posterior/igual e vigência declarada; esta significa ativo ou fim preenchido (`:157-164`). A forma SQL está em `:187-193`.
- `app/escala/alimentacao.py:196-254` `vinculo_do_dia` escolhe a geradora da célula se encontrada e dentro dos limites de início/fim; fora retorna `None` (`:237-252`). Sem geradora escolhe entre vínculos que cobrem o dia pelo máximo `(ativa, data_inicio)` (`:253-254`). O ramo da geradora **não chama** `tem_vigencia_declarada`, ao contrário do ramo sem geradora (`:250-254`).
- `app/escala/folgas.py:392-444` `reparentar_folga`: calendário retorna `'calendario'` e não muda de dono (`:405-406`); manual cujo vínculo ainda cobre o dia retorna `'ja_cobre'` **mesmo se outro for escolhido pela juíza para o dia** (`:407-409`). Só quando o vínculo não cobre busca o alvo via `vinculo_do_dia_carregado` (`:414-416`). Alvo com foto já existente resulta `'ocupado'`; sem alvo resulta `'sem_vinculo'`; com alvo migra e relavra a partir da competência corrente (`:418-443`).
- `app/escala/folgas.py:447-459` `reparentar_folgas_do_colaborador` seleciona só fotos manuais e só chama reparentamento quando o vínculo original não cobre a data; calendário nunca entra (`:453-459`). A mesma seleção está no comando `app/escala/management/commands/reparentar_folgas.py:28-34`.
- `app/escala/signals.py:172-191` aciona reparentamento ao criar vínculo ou mudar a vigência; delega à função acima (`:185-191`).

**Outros leitores diretos que classificam/exibem/censam a foto**

| Sítio | O que faz e tratamento de fotos em outro vínculo |
|---|---|
| `app/escala/views_plano_folgas.py:51-82` `_grid_ctx` | Carrega fotos dos vínculos ativos e indexa por **ec + dia**; foto do próprio ec é exibida como gravada antes de qualquer cálculo (`:55-73`). Foto presa a outro ec não aparece na linha do ec diário; não distingue origem ou testa vigência da data na leitura (`:60-73`). |
| `app/escala/views_plano_folgas.py:109-114` toggle | Busca ec ativo e foto desse ec/data; calendário recusa edição. Foto em outro ec não é encontrada, seja calendário/manual (`:109-114`); a escrita delega a `lancar_folga`/`alterar_folga` (`:124-137`). |
| `app/escala/folgas.py:254-260` `lancar_folga` e `:274-289` `_foto_do_dia` | Consultam por ec/data; foto própria de calendário impede edição, manual pode ser criada/alterada/removida. Foto em outro ec não é encontrada localmente; veto por pessoa é chamado para criação (`:258-260`, `:282-284`). |
| `app/escala/models.py:1212-1221` `defeito_folga_nao_definida` | Para 6x1 sem folga semanal, basta qualquer foto **do self no mês** para afastar defeito. Outro vínculo não conta; origem não diferencia (`:1217-1221`). |
| `app/ponto/services/diagnostico_escala.py:102-104` | Conta fotos do ec/mês e lê defeito acima; calendário/manual de outro ec não contam (`:102-104`). |
| `app/relatorios/furos.py:112-115` | Diagnóstico acrescenta “sem foto FolgaDia do mês” se ec não tem foto; outro vínculo não conta e origem não diferencia (`:112-115`). |
| `app/escala/services/fase_conflitante.py:52-58`, `:71-83` | Para ec ativo, lê suas fotos de qualquer data, calcula folga exata/paridade pela foto mais próxima e compara com âncora. Foto de outro ec não entra; origem/vigência de cada data não são filtradas (`:71-80`). |
| `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:55-60` | Monta `(colaborador_id, data)` de fotos da empresa na janela, **sem vínculo diário, origem ou vigência**. Tanto calendário errado quanto manual errado podem selecionar dias para rejulgamento; cruza com ausência não-falta (`:24-35`, `:55-60`) e, com `--apply` e universo esperado, chama cartório (`:63-72`). |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:1532-1549` C5 | Foto entra se o vínculo original cobre o dia **ou** `vinculo_que_cobre(..., exceto=original)` não devolve `'nenhum'`; origem não distingue (`:1532-1536`). Assim calendário/manual em outro vínculo podem contar; cobra conflito se célula trabalha e competência não foi exportada (`:1540-1549`). |
| `app/escala/folgas.py:363-383` `vinculo_que_cobre` | Usado pelo C5: filtra candidatos pela vigência SQL, exclui vínculo opcional; um candidato vence, vários com único ativo vence, senão `'ambiguo'` (`:369-383`). C5 aceita inclusive `'ambiguo'`, pois só exclui `'nenhum'` (`app/chamados/management/commands/auditar_invariantes_chamados.py:1535-1536`). |
| `app/escala/folgas.py:83-108` `divergencia_foto_x_calendario` | Compara fotos **do ec/mês** com calendário atual; separa calendário/manual pela origem e calcula falta/sobra (`:99-108`). Fotos em outro ec não entram, qualquer que seja origem (`:101-104`). |
| `app/escala/folgas.py:150-175` `gerar_foto_folgas` | No ec/mês, preserva fotos manuais, apaga fotos de calendário e recria datas mandadas, pulando dias com manual (`:165-175`). Outro vínculo não é consultado/tocado por esses filtros (`:165-174`). |

A distinção material comprovada é: **a precedência padrão exclui calendário de outro vínculo e conserva manual de outro vínculo; o veto consulta ambos por pessoa; cálculo/foto e portas locais consultam por vínculo; o overlay por turno percorre vínculos de toda a janela e aplica cada foto à janela inteira** (`app/ponto/precedencia.py:234-238`, `:313-315`, `:458-466`; `app/escala/models.py:1251-1288`; `app/escala/utils.py:1261`, `:1785-1787`, `:1109-1119`).

## 31/10 12:31 · sessao 4943d5eb6b04 · - · - · origem=Ronald (tmux)

Censo confirmado de chamadas executadas:

| Alvo | Chamadores de produção |
|---|---|
| `_aplicar_folgas_foto` | `montar_grade_prevista`, `app/escala/utils.py:768`; `montar_grade_prevista_periodo_por_turno`, `app/escala/utils.py:1787`. |
| `montar_grade_prevista` | `_montar_grade_prevista_periodo_legacy`, `app/escala/utils.py:993`. |
| `_montar_grade_prevista_periodo_legacy` | Dispatcher `montar_grade_prevista_periodo`, `app/escala/utils.py:2029`. |
| `montar_grade_prevista_periodo_por_turno` | Dispatcher, `app/escala/utils.py:2027`; `_score_geometria`, `app/ponto/management/commands/detectar_par_relampago.py:136`. |
| `montar_grade_prevista_periodo` | `_julgar_colab_corpo`, `app/ponto/services/cartorio.py:931`; `Command.handle`, `app/ponto/management/commands/reconciliar_grade.py:118`; `grade_espelho_janela`, `app/escala/utils.py:2277`. |

Há referências executáveis que **inspecionam assinatura**, sem chamar o builder: `app/ponto/management/commands/censo_col900.py:53` lê default `tol_min` de `montar_grade_prevista`; `app/ponto/management/commands/tripwire_celulas.py:124` lê default `tol_min` do dispatcher.

O comentário que enumera cinco consumidores e diz que `detectar_intervalo_ausente` chama o single builder está em `app/escala/utils.py:2017`, mas não descreve o censo atual acima; chamadas reais estão nos pontos da tabela.

Por chamador do dispatcher:

1. **Cartório: lê ambos; decide e grava.**
   - Indexa `g['dias']` por data: `app/ponto/services/cartorio.py:934`.
   - Ramo nunca bateu lê `tipo_dia in ('ausencia', 'falta')` para cobertura integral: `app/ponto/services/cartorio.py:997`; produz ata: `app/ponto/services/cartorio.py:1011`.
   - Ramo normal conta células não missing + órfãs para `batidas_no_dia`: `app/ponto/services/cartorio.py:1170`.
   - Passa dia e marcos ao classificador: `app/ponto/services/cartorio.py:1207`. O classificador lê `tipo_dia`, previsto, realizado e células: `app/ponto/supra_juiz.py:199`, `app/ponto/supra_juiz.py:207`; define dia de cobrança por tipo: `app/ponto/supra_juiz.py:281`; missing participa do furo parcial: `app/ponto/supra_juiz.py:348`, `app/ponto/supra_juiz.py:379`; tipo diferente de trabalho participa de `COBRANCA_EM_DIA_SEM_TRABALHO`: `app/ponto/supra_juiz.py:443`.
   - `ata_do_dia` transforma células em lâmpadas e grava `tipo_dia`, minutos, contagens: `app/ponto/services/cartorio.py:564`, `app/ponto/services/cartorio.py:567`, `app/ponto/services/cartorio.py:575`.
   - Lavra ata, veredito e fatos do status: `app/ponto/services/cartorio.py:1557`.
   - Códigos resultantes controlam emissão de chamado inteiro: `app/ponto/services/cartorio.py:1489`, `app/ponto/services/cartorio.py:1508`; parcial: `app/ponto/services/cartorio.py:1531`, `app/ponto/services/cartorio.py:1544`.
   - Ata deste julgamento também alimenta `marcos_acesos` para avaliar protesto e reconciliação: `app/ponto/services/cartorio.py:1182`, `app/ponto/services/cartorio.py:1204`, `app/ponto/services/cartorio.py:1275`.

2. **`reconciliar_grade`: não lê `dias[].tipo_dia`; lê células e decide chamados.**
   - Itera dias, pula sem âncora, extrai células missing e marca como limpo quando não há missing: `app/ponto/management/commands/reconciliar_grade.py:130`, `app/ponto/management/commands/reconciliar_grade.py:139`, `app/ponto/management/commands/reconciliar_grade.py:151`, `app/ponto/management/commands/reconciliar_grade.py:154`.
   - Missing vencidos geram `furo[dt]`: `app/ponto/management/commands/reconciliar_grade.py:169`, `app/ponto/management/commands/reconciliar_grade.py:179`.
   - Planeja/emite chamados, sujeito a outros vetos: `app/ponto/management/commands/reconciliar_grade.py:215`, `app/ponto/management/commands/reconciliar_grade.py:243`, `app/ponto/management/commands/reconciliar_grade.py:250`.
   - Dias avaliados limpos entram em `mortos`: `app/ponto/management/commands/reconciliar_grade.py:266`; todos os dias mortos permitem retratar chamado: `app/ponto/management/commands/reconciliar_grade.py:292`, `app/ponto/management/commands/reconciliar_grade.py:305`.

3. **`grade_espelho_janela`: não decide por tipo; clippa e repassa.**
   - Chama dispatcher, clippa, retorna: `app/escala/utils.py:2277`.
   - Clipping troca células por `None` e apaga órfãs fora da janela: `app/escala/utils.py:944`, `app/escala/utils.py:948`.

Consumidores vivos de `grade_espelho_janela`:

| Consumidor | Leitura/decisão |
|---|---|
| Espelho | `_pela_grade` chama em `app/ponto/services/espelho.py:503`; usado para trecho anterior à competência em `app/ponto/services/espelho.py:541` ou fallback de ata agregada em `app/ponto/services/espelho.py:548`, `app/ponto/services/espelho.py:571`. Lê tipo para `folga`: `app/ponto/services/espelho.py:690`; células não nulas para `previsto_canon`: `app/ponto/services/espelho.py:695`; células para apresentação de realizado/missing: `app/ponto/services/espelho.py:700`, `app/escala/utils.py:1059`. Transporta `folga`, `previsto_canon`, `realizado_grade`: `app/ponto/services/espelho.py:781`. Template usa esses campos: `app/templates/ponto/espelho.html:160`, `app/templates/ponto/espelho.html:166`, `app/templates/ponto/espelho.html:213`, `app/templates/ponto/espelho.html:226`. |
| Simulação | Chama em `app/ponto/services/simular_dia.py:7`; `dia_perfeito` exige tipo trabalho e ausência de órfãs e missing/divergência: `app/ponto/services/simular_dia.py:12`, `app/ponto/services/simular_dia.py:14`. `veredito_celula` recusa materializar trabalho se tipo não trabalho: `app/ponto/services/veredito_celula.py:220`. Flip automático usa dia perfeito antes/depois para auto/humano: `app/ponto/services/flip_auto.py:303`. |
| Mapa diagnóstico | Duas chamadas em `app/ponto/management/commands/mapa_divergencia.py:104`; assina tipo e células/status/missing em `app/ponto/management/commands/mapa_divergencia.py:120`, `app/ponto/management/commands/mapa_divergencia.py:131`; compara divergências em `app/ponto/management/commands/mapa_divergencia.py:158`, `app/ponto/management/commands/mapa_divergencia.py:208`. |
| PDF | Chamada em `app/relatorios/pdf_espelho.py:471` lê só órfãs; elas excluem entrada espúria da contagem de turnos abertos em `app/relatorios/pdf_espelho.py:509`. Segunda chamada em `app/relatorios/pdf_espelho.py:539` lê órfãs e células de isenção em `app/relatorios/pdf_espelho.py:548`, `app/relatorios/pdf_espelho.py:551`; não lê `tipo_dia`. Órfãs marcam batidas no papel em `app/relatorios/pdf_espelho.py:603`. Os dias desenhados vêm do espelho em `app/relatorios/pdf_espelho.py:592`. |

**Dinheiro/folha/export:** não há chamada direta do dispatcher em `folha/export.py`; o ponto de entrada `grade_do_fechamento` lê `grade_da_celula`: `app/folha/export.py:318`, `app/folha/export.py:324`, `app/folha/export.py:327`. `por_dia_da_grade` usa o tipo dessa grade para incluir somente trabalho/ausência no previsto, calcular abono e limitar realizado pelo previsto: `app/ponto/services/dia_pago.py:385`, `app/ponto/services/dia_pago.py:392`, `app/ponto/services/dia_pago.py:393`, `app/ponto/services/dia_pago.py:398`. A ponte do dispatcher até esse dinheiro passa pela ata que o cartório lavra, nos pontos acima.

**Leitura interna adicional das células do overlay:** depois dos overlays (`app/escala/utils.py:1787`), o builder por turno monta cópia em memória da ata com lâmpadas das células atuais (`app/escala/utils.py:1826`, `app/escala/utils.py:1832`) e passa essas cópias ao juiz de turnos (`app/escala/utils.py:1838`); realizado final vem desse juiz (`app/escala/utils.py:1985`, `app/escala/utils.py:1998`). Portanto o apagamento de células também alcança a montagem do realizado dentro do próprio builder.

## 31/10 12:21 · sessao 46797b835cdf · E4-1 · leitura · origem=Code

**Não existe um conjunto `S` que torne a coluna equivalente a P-hoje para todo dia julgado.** A precedência consulta fatos atuais; a lavra combina grade, batidas, isenção e veredito. Inclusive `folga` pode resultar com P-hoje verdadeiro ou falso. `app/ponto/precedencia.py:305`, `app/ponto/status_dia.py:96`, `app/chamados/services/feriado_reavaliar.py:34`.

**1(a) — Todos os retornos de `status_da_lavra`**

Na tabela, aplica-se **a primeira linha satisfeita**. Notação: `B=tem_batida`, `G=tipo_dia`, `M=previsto_min`, `I=isento`, `C=cobertura_parcial`, `V=veredito`, `H=folga_humana`. `R={furo, discordante, cobrado, nunca_bateu, fato_em_ausencia}`; `A=R∪{indefinida}`. `app/ponto/status_dia.py:55`, `app/ponto/status_dia.py:70`.

| Ordem | Combinação de entrada | Status | Ramo |
|---|---|---|---|
| 1 | `!B && G=falta` | `falta` | `app/ponto/status_dia.py:96` |
| 2 | `!B && G=ausencia`, salvo `C && !I && V∈A` | `ausencia` | `app/ponto/status_dia.py:98` |
| 3 | `V=indefinida && H && B` | `folga_trabalhada` | `app/ponto/status_dia.py:101` |
| 4 | `V=indefinida && H && !B` | `folga` | `app/ponto/status_dia.py:103` |
| 5 | `V=indefinida && !H` | `escala_suspeita` | `app/ponto/status_dia.py:104` |
| 6 | `!B && (G=folga || !M)` | `folga` | `app/ponto/status_dia.py:105` |
| 7 | `!B && I` | `ok`, rótulo `isento de ponto` | `app/ponto/status_dia.py:108` |
| 8 | `!B && V=coberta` | `ausencia` | `app/ponto/status_dia.py:110` |
| 9 | `!B && V∈R` | `irregular` | `app/ponto/status_dia.py:112` |
| 10 | Demais entradas sem batida | `sem_certidao` | `app/ponto/status_dia.py:114` |
| 11 | `B && (G=folga || V=fato_sem_previsao)` | `folga_trabalhada` | `app/ponto/status_dia.py:115` |
| 12 | `B && V∈R` | `irregular` | `app/ponto/status_dia.py:117` |
| 13 | `B && V∈{concorde,coberta,cobranca_indevida}` | `ok`, rótulo de isento quando `I` | `app/ponto/status_dia.py:119` |
| 14 | Demais entradas com batida | `sem_certidao` | `app/ponto/status_dia.py:121` |

`aberto`, `previsto` e `sem_dados` pertencem a `ROTULO_STATUS`, mas **não são devolvidos por `status_da_lavra`**. No leitor, `sem_dados` vem da ausência de célula; `aberto/previsto` vêm do relógio; célula sem status pode devolver `previsto` ou `sem_certidao`. `app/ponto/status_dia.py:28`, `app/ponto/status_dia.py:173`, `app/ponto/status_dia.py:177`, `app/ponto/status_dia.py:184`.

A coluna também tem uma segunda regra de entrada: decisão administrativa usa `status_do_admin`: `trabalhou→ok`, `faltou→falta`, `justificado→ausencia`; `_status_alvo` preserva essa autoridade mesmo numa passagem do cartório. `app/ponto/status_dia.py:52`, `app/ponto/portas/celula.py:694`.

**1(b) — Composição dos fatos, da grade e do status**

As tabelas seguintes especificam as combinações por etapas: primeiro `faixa/era_dia_previsto`; depois `tipo_dia/minutos_previstos`; finalmente a primeira linha aplicável de 1(a). Essa é também a composição implementada: precedência monta seus candidatos, a grade aplica overlays, e `fatos_do_status` extrai a entrada da lavra. `app/ponto/precedencia.py:332`, `app/escala/utils.py:1785`, `app/ponto/status_dia.py:146`.

A grade analisada abaixo é a implementação **por turno**, padrão da configuração. A flag efetiva do ambiente é [não verificado]. O dispatcher também admite o legacy; esse merge não propaga `minutos_previstos`, que `fatos_do_status` converte em zero quando ausente. `app/config/settings/base.py:165`, `app/escala/utils.py:2024`, `app/escala/utils.py:1016`, `app/ponto/status_dia.py:146`.

**Como a precedência calcula `era_dia_previsto`:**

| Célula/vínculo | `era_dia_previsto` |
|---|---|
| `celula.trabalha=True` | `True`, inclusive sem vínculo válido. `app/ponto/precedencia.py:305`, `app/ponto/precedencia.py:316` |
| `celula.trabalha=False` | `False`, inclusive sem vínculo válido. `app/ponto/precedencia.py:316` |
| Célula inexistente ou `trabalha=None`, com vínculo | Resultado de `eh_dia_trabalho_calculado`; exceção resulta em `None`. `app/ponto/precedencia.py:324` |
| Célula inexistente ou `trabalha=None`, sem vínculo | `None`. `app/ponto/precedencia.py:325` |

A geradora declarada pela célula só vale dentro de sua vigência; fora dela a seleção devolve `None`, sem escolher outro vínculo como reserva. `app/escala/alimentacao.py:239`, `app/escala/alimentacao.py:247`.

**Sem ausência vencedora**, sejam `F=feriado no calendário aplicável`, `T=trabalha_em_feriado do vínculo selecionado`, `L=FolgaDia considerada pela precedência`, `p=era_dia_previsto`:

| Combinação | `faixa` | `era_dia_previsto` | Fonte |
|---|---|---|---|
| `F && p=True && T=True`, sem `L` | `trabalho` | `True` | `app/ponto/precedencia.py:330`, `app/ponto/precedencia.py:339` |
| `F && p=True && T=True`, com `L` | `folga` | `True` | `app/ponto/precedencia.py:330`, `app/ponto/precedencia.py:337` |
| `F` e qualquer outra combinação de `p/T` | `feriado` | Mantém `p`, inclusive `None` | `app/ponto/precedencia.py:330`, `app/ponto/precedencia.py:335` |
| Sem feriado candidato; `L` ou `p=False` | `folga` | Mantém `p` | `app/ponto/precedencia.py:337` |
| Sem feriado candidato nem `L`; `p=True` | `trabalho` | `True` | `app/ponto/precedencia.py:339` |
| Sem feriado candidato nem `L`; `p=None` | `None` | `None` | `app/ponto/precedencia.py:342` |

Para `L`, a precedência pega a primeira FolgaDia por `pk` de cada data. Se ela tem `calendario_origem` e pertence a outro vínculo, descarta essa linha; não procura uma segunda FolgaDia. A manual não passa por esse descarte de vínculo. `app/ponto/precedencia.py:234`, `app/ponto/precedencia.py:313`.

**Ausências na precedência:**

| Fato | Efeito em `faixa/era_dia_previsto` |
|---|---|
| Ausência inteira ou parcial, não rejeitada | Entra pela faixa do tipo, sem testar cobertura parcial; não modifica `era_dia_previsto`. `app/ponto/precedencia.py:227`, `app/ponto/precedencia.py:332`, `app/ponto/precedencia.py:360` |
| `ferias` | Faixa `ferias`. `app/ponto/precedencia.py:53` |
| Tipos mapeados como afastamento | Faixa `afastamento`. `app/ponto/precedencia.py:54` |
| `atestado/atestado_inss` | Faixa `atestado`. `app/ponto/precedencia.py:58` |
| `folga_compensatoria/feriado_folga` | Faixa `folga`; feriado candidato ainda vence. `app/ponto/precedencia.py:62`, `app/ponto/precedencia.py:46` |
| `falta` ou outro tipo fora do mapa | Faixa `atestado`, portanto vence feriado/folga. `app/ponto/precedencia.py:76`, `app/ponto/precedencia.py:349` |
| Mais de uma ausência | Considera todas e escolhe pela precedência `ferias > afastamento > atestado > feriado > folga > trabalho`. `app/ponto/precedencia.py:46`, `app/ponto/precedencia.py:349` |

**Base da grade por turno, antes dos overlays:**

| Combinação | `tipo_dia` | `minutos_previstos` | Fonte |
|---|---|---|---|
| Sem vínculo selecionado ou sem tipo de escala | `folga` | `0` | `app/escala/utils.py:1543`, `app/escala/utils.py:1743` |
| Vínculo não intermitente; `eh_dia_trabalho=False` | `folga` | `0` | `app/escala/utils.py:1548`, `app/escala/utils.py:1743` |
| Vínculo não intermitente; `eh_dia_trabalho=True` | `trabalho` | Soma dos segmentos previstos `E→S` | `app/escala/utils.py:1550`, `app/escala/utils.py:1578`, `app/escala/utils.py:1749` |
| Vínculo não intermitente; `eh_dia_trabalho=None` | `trabalho`, `sem_ancora=True` | Mesma soma dos marcos | `app/escala/utils.py:1548`, `app/escala/utils.py:1701`, `app/escala/utils.py:1749` |
| Intermitente sem batidas disponíveis para convocação | Não monta turno; cai em `folga/0` | `0` | `app/escala/utils.py:1476`, `app/escala/utils.py:1743` |
| Intermitente com convocação por batidas | Monta o dia na pré-passagem própria | Previsto próprio desse ramo | `app/escala/utils.py:1480`, `app/escala/utils.py:1720` |

Logo, os booleanos enumerados na pergunta não fixam um número de minutos: o ramo comum soma a duração dos segmentos dos marcos encadeados. `app/escala/utils.py:1578`.

**Onde entram feriado e `trabalha_em_feriado` na grade:**

A grade chama:

```python
if e.eh_dia_trabalho(d, celulas=_cels_gr, folgas=_folgas_gr) is False:
    continue
```

`app/escala/utils.py:1548`.

Esse método devolve a célula existente diretamente:

```python
if cel is not None:
    return cel.trabalha
return self.eh_dia_trabalho_calculado(data, folgas=folgas)
```

`app/escala/models.py:1071`.

Portanto, **havendo célula, a grade não reaplica o feriado nesse gate**. O tratamento está no fallback calculado:

```python
if base is True and self.feriado_vira_folga(data, escala=escala):
    return False
```

`app/escala/models.py:1338`.

O helper recusa a transformação quando `trabalha_em_feriado=True` ou quando o ciclo não pertence a `6x1/5x2/semanal`; depois consulta o feriado pelo lugar do colaborador. `app/escala/models.py:1357`, `app/escala/models.py:32`.

Isso produz, entre outras, estas combinações diretas, antes de overlays:

| Fatos | Precedência | Grade | Status resultante |
|---|---|---|---|
| Célula `True`, vínculo válido, feriado, `T=False` | `feriado/True` | `trabalho/M` | Pela tabela 1(a): sem batida e `V∈R` dá `irregular`; isento dá `ok`; com batida, `concorde` dá `ok`. `app/ponto/precedencia.py:330`, `app/escala/models.py:1071`, `app/ponto/status_dia.py:108`, `app/ponto/status_dia.py:112`, `app/ponto/status_dia.py:119` |
| Mesmos fatos, `T=True` | `trabalho/True` | `trabalho/M` | Mesmos ramos de status; muda P-hoje. `app/ponto/precedencia.py:330`, `app/ponto/status_dia.py:105` |
| Célula `False`, feriado, qualquer `T` | `feriado/False` | `folga/0` | Sem batida: `folga`; com batida: `folga_trabalhada`; `indefinida` também segue `H=True`. `app/ponto/precedencia.py:330`, `app/escala/utils.py:1548`, `app/ponto/status_dia.py:101`, `app/ponto/status_dia.py:105`, `app/ponto/status_dia.py:115` |
| Célula `False`, sem feriado/ausência/folga humana | `folga/False` | `folga/0` | Normalmente `folga/folga_trabalhada`; com `indefinida`, `H=False` dá `escala_suspeita`. `app/ponto/precedencia.py:337`, `app/ponto/precedencia.py:191`, `app/ponto/status_dia.py:104` |
| Sem vínculo, célula `True`, sem feriado | `trabalho/True` | `folga/0` | Sem batida e `nunca_bateu`: `folga`; `indefinida`, sem natureza humana: `escala_suspeita`. `app/ponto/precedencia.py:316`, `app/escala/utils.py:1543`, `app/ponto/status_dia.py:104`, `app/ponto/status_dia.py:105` |
| Sem vínculo, célula `True`, com feriado | `feriado/True` | `folga/0` | Sem batida: `folga`, inclusive com `indefinida`, pois `H=True`. `app/ponto/precedencia.py:330`, `app/ponto/status_dia.py:103`, `app/ponto/status_dia.py:105` |
| Sem vínculo, célula inexistente ou `None`, com feriado | `feriado/None` | `folga/0` | Pode resultar `folga`, mas P-hoje pula por previsão `None`. `app/ponto/precedencia.py:325`, `app/ponto/precedencia.py:335`, `app/ponto/status_dia.py:103`, `app/chamados/services/feriado_reavaliar.py:34` |

**Overlays que completam todas essas combinações:**

| Fato adicional | Grade após o overlay | Consequência para o status |
|---|---|---|
| FolgaDia manual, cujo vínculo participa da janela | `folga`, apaga células/órfãs; **mantém os minutos anteriormente calculados** | Sem batida: `folga`; com batida: `folga_trabalhada`; `indefinida` consulta a natureza humana. `app/escala/utils.py:1109`, `app/escala/utils.py:1118`, `app/ponto/status_dia.py:101`, `app/ponto/status_dia.py:105`, `app/ponto/status_dia.py:115` |
| FolgaDia de calendário de **outro vínculo** | Só aplica se esse outro vínculo estiver em `escs_janela_ids`; quando aplica, muda todos os dias correspondentes da grade, sem conferir o vínculo de cada data ou `calendario_origem` | Pode produzir `folga/folga_trabalhada` enquanto a precedência descartou essa FolgaDia. `app/escala/utils.py:1260`, `app/escala/utils.py:1109`, `app/escala/utils.py:1785`, `app/ponto/precedencia.py:313` |
| Ausência pendente/não aprovada | Não entra no overlay de ausência da grade; a precedência a considera se não rejeitada | Mantém o status derivado da grade anterior, apesar de poder mudar `faixa`. `app/escala/utils.py:1148`, `app/ponto/turnos.py:2487`, `app/ponto/precedencia.py:227` |
| Ausência aprovada inteira, não `falta` | `ausencia`, mantém minutos, limpa células/órfãs | Sem batida: `ausencia`, inclusive com `indefinida`. Com batida: segue os ramos de veredito de 1(a). `app/escala/utils.py:1180`, `app/escala/utils.py:1195`, `app/ponto/status_dia.py:98`, `app/ponto/status_dia.py:115` |
| Ausência aprovada parcial | `ausencia`, mantém minutos e células, grava `cobertura_parcial=True` | Sem batida, não isento, `V∈R`: `irregular` se há previsto; `indefinida`: `escala_suspeita` salvo `H`; demais: `ausencia`. Se previsto zero, o ramo 6 pode devolver `folga`. `app/escala/utils.py:1190`, `app/ponto/status_dia.py:98`, `app/ponto/status_dia.py:101`, `app/ponto/status_dia.py:106`, `app/ponto/status_dia.py:112` |
| Parcial + isento Art.62, sem batida | Continua `ausencia` | `ausencia`, inclusive `nunca_bateu/indefinida`. `app/ponto/status_dia.py:98` |
| Falta aprovada sobre dia que não está em `folga` | `falta`, mantém minutos, limpa células/órfãs | Sem batida: `falta`, antes de qualquer veredito. `app/escala/utils.py:1170`, `app/escala/utils.py:1173`, `app/ponto/status_dia.py:96` |
| Falta aprovada sobre `folga` | Mantém `folga` | Pode dar `folga/folga_trabalhada`; a precedência, contudo, considera a falta na faixa `atestado`. `app/escala/utils.py:1170`, `app/ponto/precedencia.py:76`, `app/ponto/status_dia.py:105`, `app/ponto/status_dia.py:115` |
| Ausência sem nenhum vínculo selecionado na janela | O loop não chama `_marcar_ausencias` | Permanece a classificação base da grade. `app/escala/utils.py:1785` |
| Isento Art.62 | Overlay posterior a folga/ausência; não muda tipo nem minutos, marca células presumidas como `isencao` | Trabalho sem batida, previsto positivo e sem ramo anterior: `ok`; folga/ausência conservam seus ramos anteriores. `app/escala/utils.py:1790`, `app/escala/utils.py:819`, `app/ponto/status_dia.py:96`, `app/ponto/status_dia.py:108` |

A cobertura parcial é determinada pelo registro: tipo admite horas, período de um dia, `0<minutos<480`. A grade escolhe a ausência aprovada mais recente por `(data,pk)`, enquanto a precedência considera todas as não rejeitadas e ordena por faixa. `app/ponto/turnos.py:2571`, `app/ponto/turnos.py:2734`, `app/ponto/turnos.py:2750`, `app/ponto/precedencia.py:349`.

`B` não significa apenas “existe linha de Batida”: `fatos_do_status` conta células não missing/não isenção, órfãs **ou realizado positivo**. Assim, limpar células por folga/ausência inteira não garante `B=False`. `app/ponto/status_dia.py:143`.

`H` só é consultado quando `V=indefinida`: é verdadeiro para feriado vencedor, FolgaDia vencedora ou subtipo de ausência na faixa folga; mera folga da escala não basta. `app/ponto/status_dia.py:135`, `app/ponto/precedencia.py:187`.

**1(c) — Equivalência e diferenças nos dois sentidos**

Para `batida_ausente` com dia encontrado, P-hoje equivale a:

```text
era_dia_previsto != None
e faixa ∈ {feriado, folga}
```

O catálogo associa qualquer subcategoria desse módulo a `falta_descoberta`; passando esse fato como `False`, a premissa está morta. `app/chamados/services/feriado_reavaliar.py:30`, `app/chamados/services/feriado_reavaliar.py:34`, `app/chamados/catalogo/premissa.py:58`, `app/chamados/catalogo/premissa.py:134`.

**Prova de que não existe `S`:** considere o caminho implementado com célula `trabalha=True`, geradora fora da vigência, sem ausência e sem adesão. A seleção não encontra vínculo, a grade produz `folga/0`, e o ramo sem adesão lavra `nunca_bateu`; o status é `folga`. Sem feriado, a precedência devolve `trabalho/True`, e P-hoje rejeita; com feriado, devolve `feriado/True`, e P-hoje aceita. O mesmo valor `folga` exigiria pertencer e não pertencer a `S`. `app/escala/alimentacao.py:250`, `app/escala/utils.py:1543`, `app/escala/utils.py:1743`, `app/ponto/services/cartorio.py:964`, `app/ponto/services/cartorio.py:1041`, `app/ponto/status_dia.py:105`, `app/ponto/precedencia.py:316`, `app/ponto/precedencia.py:335`.

Como `S` não existe, as diferenças dependem do conjunto escolhido. Para o conjunto de rótulos `S₀={folga,folga_trabalhada}`, esta é a **partição completa dos ramos divergentes**, sempre respeitando os retornos anteriores da tabela 1(a):

| Sentido | Combinação | Resultado |
|---|---|---|
| P-hoje aceita, coluna fora de `S₀` | Sem batida, grade `ausencia`, sem a exceção da parcial | `ausencia`. Inclui os subtipos `folga_compensatoria/feriado_folga`, que A2 põe em folga e a grade em ausência. `app/ponto/precedencia.py:62`, `app/escala/utils.py:1180`, `app/ponto/status_dia.py:98` |
| P-hoje aceita, coluna fora de `S₀` | `indefinida && !H`, sem retorno anterior | `escala_suspeita`. O ramo puro cobre folga de escala; sua produção pelo classificador num julgamento consistente é [não verificado]. `app/ponto/status_dia.py:104`, `app/ponto/tests/test_e3_status_lavrado.py:105` |
| P-hoje aceita, coluna fora de `S₀` | Sem batida, grade diferente de folga, previsto positivo, veredito diferente de `indefinida`, sem retorno anterior | Isento: `ok`; `coberta`: `ausencia`; `V∈R`: `irregular`; demais: `sem_certidao`. É a divergência do feriado vivo sobre célula `True`, entre outras entradas. `app/ponto/precedencia.py:330`, `app/ponto/status_dia.py:108` |
| P-hoje aceita, coluna fora de `S₀` | Com batida, grade diferente de folga, `V` diferente de `indefinida/fato_sem_previsao` | `R→irregular`; `concorde/coberta/cobranca_indevida→ok`; demais→`sem_certidao`. `app/ponto/status_dia.py:115` |
| Coluna em `S₀`, P-hoje rejeita | `indefinida && H`, sem retorno anterior, mas `era_dia_previsto=None` | `folga` ou `folga_trabalhada`; P-hoje pula o dia desconhecido. `app/ponto/status_dia.py:101`, `app/chamados/services/feriado_reavaliar.py:34` |
| Coluna em `S₀`, P-hoje rejeita | Sem batida, `V!=indefinida`, `G=folga || !M`, sem retorno anterior, e P-hoje falso | `folga`. Abrange falta sobre folga, ausência pendente que vence A2, overlay de outro vínculo, grade sem vínculo e trabalho com previsto zero. `app/ponto/status_dia.py:105`, `app/escala/utils.py:1170`, `app/ponto/precedencia.py:227`, `app/escala/utils.py:1785`, `app/escala/utils.py:1743` |
| Coluna em `S₀`, P-hoje rejeita | Com batida, `V!=indefinida`, `G=folga || V=fato_sem_previsao`, e P-hoje falso | `folga_trabalhada`. `app/ponto/status_dia.py:115` |

No mesmo estado das fontes, P-hoje verdadeiro com `status_da_lavra=falta` não nasce do pipeline descrito: a grade exige ausência aprovada `tipo=falta`, e essa linha também entra em A2 na faixa `atestado`, que vence feriado/folga. A coluna administrativa ou antiga tem caminhos adicionais, já separados acima. `app/escala/utils.py:1173`, `app/ponto/precedencia.py:227`, `app/ponto/precedencia.py:76`, `app/ponto/portas/celula.py:694`.

**1(d) — Quando a coluna pode ficar desatualizada**

`status_do_dia_lavrado` não compara a coluna com os fatos atuais nem relavra. Quando o relógio permite ler a lavra, devolve `cel.status_dia/status_rotulo` existentes. `app/ponto/status_dia.py:171`, `app/ponto/status_dia.py:181`.

| Escrita | Ordem e condição de desatualização |
|---|---|
| **Feriado criado/alterado** | `save/create` agenda somente `reavaliar_dia` em `on_commit`; não relavra antes. Se mudou de data, captura somente a **nova data**. `app/core/views_config.py:58`, `app/core/views_config.py:81`, `app/chamados/signals.py:265` |
| **Feriado apagado** | A tela executa `delete`; não há `post_delete` de reavaliação, explicitamente porque esse serviço apenas fecha. `app/core/views_config.py:198`, `app/chamados/signals.py:260` |
| **FolgaDia manual pelas portas** | Lançar/alterar/lote escreve a foto e chama regeneração. A cura depende de o dia ser efetivamente tocado e elegível para julgamento; não é atualização incondicional do status. `app/escala/folgas.py:257`, `app/escala/folgas.py:301`, `app/escala/folgas.py:328`, `app/ponto/portas/celula.py:360` |
| **Foto de calendário** | Apaga a foto anterior não manual, faz `bulk_create` e chama `relavrar_celulas_da_foto→regenerar_celulas_vinculo`; mesmas condições da re

[truncado em 20000 de 32859 caracteres]

## 31/10 12:17 · sessao 10cecc7608df · - · - · origem=Ronald (tmux)

Leitura somente; não encontrei `AGENTS.md` no resultado de `rg --files -g AGENTS.md`. [não verificado]

**P1(a): tabela exata da função pura**, aplicando os ramos em ordem, isto é, cada linha pressupõe que nenhuma anterior retornou. Vocabulário em `app/ponto/status_dia.py:28`; conjuntos `R={furo,discordante,cobrado,nunca_bateu,fato_em_ausencia}` e `A=R∪{indefinida}` em `app/ponto/status_dia.py:55`.

| Condição | Resultado | Ramo |
|---|---|---|
| Sem batida, `tipo_dia=falta` | `falta` | `app/ponto/status_dia.py:96` |
| Sem batida, `tipo_dia=ausencia`, exceto `cobertura_parcial && !isento && veredito∈A` | `ausencia` | `app/ponto/status_dia.py:98` |
| `veredito=indefinida`, `folga_humana`, com batida | `folga_trabalhada` | `app/ponto/status_dia.py:101` |
| Idem, sem batida | `folga` | `app/ponto/status_dia.py:103` |
| `veredito=indefinida`, sem `folga_humana` | `escala_suspeita` | `app/ponto/status_dia.py:104` |
| Sem batida e (`tipo_dia=folga` ou previsto zero) | `folga` | `app/ponto/status_dia.py:105` |
| Sem batida, previsto positivo, isento | `ok`, rótulo `isento de ponto` | `app/ponto/status_dia.py:108` |
| Sem batida, previsto positivo, não isento, `coberta` | `ausencia` | `app/ponto/status_dia.py:110` |
| Sem batida, previsto positivo, não isento, `veredito∈R` | `irregular` | `app/ponto/status_dia.py:112` |
| Outros sem batida | `sem_certidao` | `app/ponto/status_dia.py:114` |
| Com batida e (`tipo_dia=folga` ou `fato_sem_previsao`) | `folga_trabalhada` | `app/ponto/status_dia.py:115` |
| Com batida, `veredito∈R` | `irregular` | `app/ponto/status_dia.py:117` |
| Com batida, `veredito∈{concorde,coberta,cobranca_indevida}` | `ok`, rótulo de isento se aplicável | `app/ponto/status_dia.py:119` |
| Demais com batida | `sem_certidao` | `app/ponto/status_dia.py:121` |

`aberto`, `previsto` e `sem_dados` estão no vocabulário, mas não em retorno de `status_da_lavra`; são produzidos pelo leitor nos caminhos relógio/sem célula/sem lavra. `app/ponto/status_dia.py:70`, `app/ponto/status_dia.py:173`, `app/ponto/status_dia.py:177`, `app/ponto/status_dia.py:184`.

**P1(b): composição exata das fontes.**

- `fatos_do_dia` delega ao período de um dia. `app/ponto/precedencia.py:195`, `app/ponto/precedencia.py:209`.
- Ausências: todas não rejeitadas que cobrem a data, sem distinguir inteira/parcial; são ordenadas pela faixa `ferias > afastamento > atestado > feriado > folga > trabalho`. Os tipos `folga_compensatoria` e `feriado_folga` têm faixa `folga`; `falta` e tipos fora do mapa têm faixa `atestado`. `app/ponto/precedencia.py:46`, `app/ponto/precedencia.py:52`, `app/ponto/precedencia.py:76`, `app/ponto/precedencia.py:227`, `app/ponto/precedencia.py:294`, `app/ponto/precedencia.py:332`, `app/ponto/precedencia.py:349`.
- `era_dia_previsto`: se célula tem `trabalha!=None`, é `bool(cel.trabalha)`, inclusive se não existe vínculo válido; quando célula ausente ou `trabalha=None`, calcula pelo vínculo, ou `None` sem vínculo/exceção. `app/ponto/precedencia.py:305`, `app/ponto/precedencia.py:316`.
- Feriado só é suprimido da precedência quando `feriado && previsto is True && ec.trabalha_em_feriado`; com `previsto=False/None`, feriado continua faixa candidata. `app/ponto/precedencia.py:330`, `app/ponto/precedencia.py:335`.
- Folga manual permanece válida mesmo pertencendo a outro vínculo. Folga de calendário de outro vínculo é descartada comparando seu vínculo ao retornado por `vinculo_do_dia`. `app/ponto/precedencia.py:313`.
- Sem ausências: feriado candidato vence folga manual ou `previsto=False`; sem feriado candidato, folga manual/`False` dá faixa `folga`; `True` sem folga dá `trabalho`; `None` sem fatos dá faixa `None`. `app/ponto/precedencia.py:335`, `app/ponto/precedencia.py:342`, `app/ponto/precedencia.py:349`.
- A grade por turno seleciona vínculo com a mesma juíza; sem vínculo/tipo de escala, não monta turno. `app/escala/utils.py:1254`, `app/escala/utils.py:1541`.
- **Grade não aplica feriado diretamente.** Chama `e.eh_dia_trabalho`; esta lê `cel.trabalha`, inclusive `None`, sem recalcular quando célula existe. A grade só pula o turno quando o retorno é exatamente `False`; `None` passa pelo gate. `app/escala/utils.py:1548`, `app/escala/models.py:1066`.
- Feriado e `trabalha_em_feriado` são tratados no **fallback aritmético**: apenas `base=True` e `feriado_vira_folga` levam a `False`; o helper exige vínculo sem `trabalha_em_feriado` e ciclo em `CICLOS_FERIADO_VIRA_FOLGA`. Retornos antecipados da âncora 12x36/24x48 precedem essa etapa. `app/escala/models.py:1260`, `app/escala/models.py:1338`, `app/escala/models.py:1357`.
- Grade sem turno: `tipo_dia=folga`, previsto `0`; grade com turno: `trabalho`, previsto soma minutos dos segmentos `E→S` dos marcos, excluindo pausas. `app/escala/utils.py:1578`, `app/escala/utils.py:1743`, `app/escala/utils.py:1749`.
- **Overlay FolgaDia não zera minutos:** muda `tipo_dia=folga`, apaga células e órfãs, preserva o previsto já calculado. `app/escala/utils.py:1109`, `app/escala/utils.py:1118`.
- **Folga de outro vínculo na grade depende da janela:** consulta apenas FolgaDia do vínculo passado ao overlay; porém o builder chama o overlay de **cada vínculo que possui algum dia na janela**, sobre **todos os dias da grade**, sem filtrar pelo vínculo de cada data nem `calendario_origem`. Logo outro vínculo ausente de `escs_janela_ids` não aplica; outro vínculo presente no conjunto aplica a FolgaDia sobre essa data. `app/escala/utils.py:1260`, `app/escala/utils.py:1109`, `app/escala/utils.py:1785`.
- Ausências na grade usam cobertura para folha, portanto só aprovadas; a autoridade escolhe a mais recente por `(data,pk)`, enquanto A2 considera todas e desempata por faixa. `app/escala/utils.py:1148`, `app/ponto/turnos.py:2487`, `app/ponto/turnos.py:2734`, `app/ponto/turnos.py:2750`.
- Falta aprovada: só transforma em `tipo_dia=falta` se dia não estiver em `folga`; limpa células/órfãs, sem alterar minutos. Ausência não falta aprovada: transforma em `ausencia` mesmo sobre folga, mantém minutos, grava `cobertura_parcial`, limpa células/órfãs só quando inteira. `app/escala/utils.py:1170`, `app/escala/utils.py:1190`, `app/escala/utils.py:1195`.
- Ausência pode deixar de ser aplicada se nenhum vínculo figura em `escs_janela_ids`, pois seu overlay está dentro desse loop. `app/escala/utils.py:1785`.
- Art.62 roda depois de folga/ausência e não muda `tipo_dia` ou previsto; preenche as células faltantes com marca `isencao`. `app/escala/utils.py:1787`, `app/escala/utils.py:1790`, `app/escala/utils.py:819`.
- `fatos_do_status` calcula batida com células não missing e não `isencao`, mais órfãs, **ou** realizado positivo; portanto ausência inteira/FolgaDia podem limpar células e ainda `tem_batida=True` pelo realizado. `app/ponto/status_dia.py:143`.
- Apenas `indefinida` faz `fatos_do_status` consultar A2 para `folga_humana`; A2 devolve verdadeiro para feriado vencedor ou folga lançada/subtipo folga vencedor, e falso para mera folga da escala. `app/ponto/status_dia.py:135`, `app/ponto/precedencia.py:187`.
- Flag da implementação da grade: escolhe por turno se `GRADE_POR_TURNO`; default configurado é verdadeiro, variável de ambiente pode alterar. O legacy também existe e, no merge final, não propaga `minutos_previstos`. `app/escala/utils.py:2024`, `app/config/settings/base.py:165`, `app/escala/utils.py:1016`.

**P1(c): não existe S.**

Prova por mesmo status `folga` em lados opostos: célula `trabalha=True` com geradora fora de vigência dá vínculo `None`; a grade devolve `folga/0`; A2 continua `era_dia_previsto=True`, faixa `trabalho` sem feriado/folga e faixa `feriado` com feriado. No ramo real sem adesão, o cartório grava `nunca_bateu` quando não há cobertura inteira; em ambas as entradas o status é `folga`, mas `espurios` só aceita a segunda. `app/escala/alimentacao.py:239`, `app/escala/alimentacao.py:250`, `app/escala/utils.py:1543`, `app/escala/utils.py:1743`, `app/ponto/precedencia.py:316`, `app/ponto/precedencia.py:330`, `app/ponto/precedencia.py:335`, `app/ponto/services/cartorio.py:1031`, `app/ponto/services/cartorio.py:1041`, `app/ponto/status_dia.py:105`, `app/chamados/services/feriado_reavaliar.py:34`.

Para comparar com `S0={folga,folga_trabalhada}`, a partição por ramos de `status_da_lavra` é mais segura que afirmar alcançabilidade de todas as combinações da função pura. A função pura explicitamente aceita `indefinida` + folga de escala => `escala_suspeita`, com teste; não confirmei execução real do classificador produzindo esse par em um snapshot consistente. `app/ponto/status_dia.py:101`, `app/ponto/tests/test_e3_status_lavrado.py:105`; alcançabilidade desse par em julgamento real [não verificado].

P verdadeiro e `status=falta` **não nasce desse pipeline no mesmo snapshot**: grade falta exige Ausencia aprovada tipo falta; essa mesma ausência está na A2, recebe faixa atestado e vence feriado/folga. `app/escala/utils.py:1148`, `app/escala/utils.py:1173`, `app/ponto/precedencia.py:227`, `app/ponto/precedencia.py:76`, `app/ponto/precedencia.py:46`, `app/ponto/precedencia.py:349`.

P verdadeiro e `status=ausencia` pode vir dos subtipos `folga_compensatoria/feriado_folga`: A2 os classifica em folga e a grade rotula todos os não falta em ausência. `app/ponto/precedencia.py:62`, `app/escala/utils.py:1179`, `app/ponto/status_dia.py:98`.

P falso e status folga/folga trabalhada também ocorre nas entradas de: `era_dia_previsto=None` com natureza humana; falta aprovada sobre folga da grade; ausência pendente em folga/feriado (A2 a considera, grade não); overlay de calendário de outro vínculo presente na janela; trabalho com previsto zero; batida com `fato_sem_previsao`. São os ramos/filtros acima, não evidência de ocorrência em dados. `app/chamados/services/feriado_reavaliar.py:34`, `app/ponto/status_dia.py:101`, `app/escala/utils.py:1170`, `app/ponto/turnos.py:2487`, `app/ponto/precedencia.py:227`, `app/escala/utils.py:1785`, `app/ponto/status_dia.py:106`, `app/ponto/status_dia.py:115`.

Testes particularmente úteis para costura da lavra: `app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:57` julga de verdade; controle sem natureza humana => `indefinida/escala_suspeita` em `:69`; feriado => `indefinida/folga` em `:74`; FolgaDia manual => `indefinida/folga` em `:82`. Testes puros cobrem parcial, isento parcial e folga humana em `app/ponto/tests/test_e3_status_lavrado.py:78`, `:91`, `:100`, `:118`.

## 31/10 12:16 · sessao f3473186e3be · - · - · origem=Ronald (tmux)

P1(d), leitura estática somente:

- **A leitura da coluna não valida frescor nem relavra**: `status_do_dia_lavrado` lê `status_dia/status_rotulo`, consulta o relógio e, quando cabe a lavra, devolve os campos existentes. Não compara com `fatos_do_dia`. `app/ponto/status_dia.py:171-185`.
- **Escrever desenho/DNA não escreve status automaticamente**: status é aplicado em `lavrar_veredito`, pela combinação `_status_alvo` → `_aplicar_status`; os saves de regeneração atualizam outros campos. `app/ponto/portas/celula.py:686-706`, `app/ponto/portas/celula.py:752-775`, `app/ponto/portas/celula.py:260-265`.

| Escrita | Caminho e possibilidade de coluna antiga |
|---|---|
| **Feriado criado/alterado** | A tela usa `save/create`; isso dispara `_feriado_post_save`, cujo corpo só agenda `reavaliar_dia(instance.data)` em `on_commit`: **não relavra antes de reavaliar**. Se a data mudou, o receiver capturou somente a data nova. `app/core/views_config.py:58-84`; `app/chamados/signals.py:265-276`. |
| **Feriado apagado** | A tela faz `feriado.delete()` sem relavra. O comentário do evento explicita que não há `post_delete`, pois a reavaliação só fecha e não reabre o retratado. `app/core/views_config.py:182-201`; `app/chamados/signals.py:260-263`. |
| **FolgaDia pelas portas** | `lancar_folga`, `alterar_folga` e lote escrevem a foto e chamam `relavrar_celulas_da_foto` dentro de `atomic`; esta delega a `regenerar_celulas_vinculo`. Não é lavra incondicional: só haverá rejulgamento dos dias efetivamente tocados pelo desenho, sujeito às guardas da regeneração. `app/escala/folgas.py:251-264`, `app/escala/folgas.py:293-305`, `app/escala/folgas.py:328-343`, `app/escala/folgas.py:218-225`; `app/ponto/portas/celula.py:243-268`, `app/ponto/portas/celula.py:360-383`. |
| **FolgaDia de calendário** | `gerar_foto_folgas` apaga a foto antiga não manual, usa `bulk_create`, depois chama a mesma porta de regeneração. As mesmas ressalvas valem. `app/escala/folgas.py:165-194`. |
| **FolgaDia reparentada** | Só manual é migrada; foto de calendário retorna imediatamente. Após mudar a FK, só chama relavra se a data está na competência corrente em diante. `app/escala/folgas.py:405-406`, `app/escala/folgas.py:434-444`. |
| **Ausencia save/delete normal** | Os receivers chamam `_julgar_dias` sincronamente; este chama `cartorio.julgar_celula` antes dos receivers consumidores, que agendam a reconciliação em `on_commit`. Assim, não cabe afirmar que todo lançamento/decisão de Ausencia deixa coluna antiga. `app/ponto/signals.py:85-93`, `app/ponto/signals.py:143-152`, `app/ponto/signals.py:250-284`. |
| **Ausencia: exceções concretas** | `raw` pula o receiver; julgamento em andamento retorna sem julgar; erros ficam dentro de `engolir`; `_dias_ausencia` deriva apenas os dias do estado atual da instância, sem captura do intervalo anterior. `app/ponto/signals.py:85-93`, `app/ponto/signals.py:115-128`, `app/ponto/signals.py:143-147`. Há também escritor real sem signal: `rejeitar_ausencias_de_ferias` usa `QuerySet.update(status='rejeitada')`; a precedência passa a excluir tais linhas. `app/ponto/services/ausencia.py:1257-1267`; `app/ponto/precedencia.py:227-232`. |
| **Regeneração** | Primeiro salva `trabalha/DNA/geradora`; depois força julgamento dos tocados até hoje, exceto vereditos humanos; depois registra o callback de reavaliação. Portanto o callback **normalmente já encontra a lavra feita**, mas não por uma chamada dentro dele. `app/ponto/portas/celula.py:231-268`, `app/ponto/portas/celula.py:360-383`, `app/ponto/portas/celula.py:421-433`. |
| **Regeneração: exclusões** | Sem vigência declarada retorna 0; célula humana editada e janela exportada ficam fora da reescrita; `recalculo=False` pula julgamento e callback; dias futuros e humanos pulam o julgamento; as exceções são logadas e o fluxo continua. `app/ponto/portas/celula.py:130-139`, `app/ponto/portas/celula.py:226-251`, `app/ponto/portas/celula.py:360-383`, `app/ponto/portas/celula.py:421-433`. |
| **Reversão e geração ordinária** | `reverter_regeneracao_dia` restaura desenho e retorna sem julgamento/reavaliação. `gerar_celulas_periodo` também salva desenho e retorna sem relavrar status. `app/ponto/portas/celula.py:529-552`, `app/ponto/portas/celula.py:590-617`. |
| **Troca de vínculo pela porta completa** | `executar_vinculo` grava o vínculo dentro do primeiro `atomic`; depois recalibra, regenera e chama `reconciliar_apos_vinculo`, reaproveitando os dias já julgados. O segundo serviço julga o restante, exceto humanos. Portanto há cura explícita, mas ela vem **depois** da gravação do vínculo. `app/colaboradores/services/vinculo.py:187-288`, `app/colaboradores/services/vinculo.py:298-331`; `app/colaboradores/services/pos_vinculo.py:50-77`. |
| **Save avulso de vínculo** | Os signals regeneram quando muda vigência ou quando muda âncora para não nula; `created` é pulado nesses dois receivers. O receiver geral de invalidação só invalida previsto, não status. Assim esses signals não constituem relavra incondicional de toda alteração de vínculo, como `trabalha_em_feriado` isolado. `app/escala/signals.py:34-73`, `app/escala/signals.py:79-119`, `app/escala/signals.py:124-138`. |

**Ordem exata nos dois eventos perguntados**

1. **Feriado**: `post_save` → `on_commit(reavaliar_dia)` → `reavaliar_dias` → `espurios` → `encerrar` → `ch.reconciliar()`. A consulta de `fatos_do_dia` acontece antes de qualquer relavra disparada pelo fechamento. `app/chamados/signals.py:265-276`; `app/chamados/services/feriado_reavaliar.py:23-39`, `app/chamados/services/feriado_reavaliar.py:43-48`, `app/chamados/services/feriado_reavaliar.py:59-84`.

   Se `reconciliar` mudar `status_local`, o receiver do chamado chama `julgar_celula`; é esse efeito posterior que pode relavrar. Se não houver fechamento/mudança, esse caminho não provê a relavra. `app/chamados/signals.py:164-180`.

2. **Regenerar vínculo**: salva desenho → `julgar_celula(..., forcar=True)` nos tocados elegíveis → cálculo financeiro → registra `on_commit` → callback faz rejunção e `reavaliar_dias`. Dentro do callback não há chamada direta a relavrar. `app/ponto/portas/celula.py:243-268`, `app/ponto/portas/celula.py:360-399`, `app/ponto/portas/celula.py:421-433`.

**Onde a lavra efetivamente acontece**

- `julgar_celula` chama `julgar_colab`; célula inexistente/futura retorna sem julgamento e hoje com turno ainda vivo pode retornar mesmo com `forcar=True`. `app/ponto/services/cartorio.py:1686-1717`.
- Ramo normal: `cel.lavrar_veredito(..., save=False, fatos_do_status=...)`, depois `carimbar(..., lavrados=_cv)`. `app/ponto/services/cartorio.py:1556-1580`.
- Ramo “nunca bateu”: mesma lavra com os fatos da grade e posterior `carimbar`. `app/ponto/services/cartorio.py:1042-1066`.
- A porta calcula/aplica status; `carimbar` inclui os campos lavrados no save. `app/ponto/portas/celula.py:752-775`, `app/ponto/portas/celula.py:795-807`.
- Mesmo sendo chamado, o cartório pode pular impressão em dia quando não recebeu `forcar`; o evento indireto do fechamento do chamado chama sem `forcar`, enquanto regeneração chama explicitamente com `forcar=True`. `app/ponto/services/cartorio.py:1157-1163`; `app/chamados/signals.py:180`; `app/ponto/portas/celula.py:372`.

Os insumos lidos ao vivo por `fatos_do_dia` são Ausencias não rejeitadas, FolgaDia, feriados, vínculo e `celula.trabalha`; por isso alterações nesses dados podem mudar sua resposta antes de outra lavra atualizar a coluna. `app/ponto/precedencia.py:227-245`, `app/ponto/precedencia.py:305-340`.

## 31/10 12:16 · sessao 49f6f9e4f191 · - · - · origem=Ronald (tmux)

**P2 — consumidores e efeitos**

`espurios(qs)` é somente leitura: obtém `dia_do_fato`, ignora dia inexistente e `era_dia_previsto is None`, exige `faixa in ('feriado','folga')` e exige `premissa_morta(... falta_descoberta=False)`; devolve lista de `(chamado, dia)`. [`app/chamados/services/feriado_reavaliar.py:23-40`]

Os consumidores diretos encontrados no código Python são:

| Consumidor | Uso do resultado |
|---|---|
| `reavaliar_dias` | Para cada par, confirma `d in datas`, chama `encerrar` e conta somente retornos verdadeiros. [`app/chamados/services/feriado_reavaliar.py:82-85`] |
| Command `reavaliar_ausencias_feriado` | Chama `universo(empresa)`, lista quantidade e chamados; sem `--apply`, encerra em dry-run; com `--apply`, chama `encerrar` e conta sucessos. Empresa default `4`. [`app/ponto/management/commands/reavaliar_ausencias_feriado.py:9-32`] |
| `ChamadoColaborador.premissa_regularizada` | Chama `espurios([self])` exclusivamente após `_celula_furada` devolver `None`; lista não vazia devolve `True`, lista vazia prossegue ao catálogo/batidas/turnos. [`app/chamados/models.py:1247-1264`] |

`universo` seleciona apenas `modulo_origem='batida_ausente'` e status em `VIVOS`, opcionalmente por empresa. [`app/chamados/services/feriado_reavaliar.py:14-20`]

`encerrar` chama `ch.reconciliar()`; se recusar ou já encerrado, retorna `False` sem nota/trilha específica; em sucesso anexa ocorrência `falta_espuria_feriado`, registra auditoria `reconciliar_feriado` e retorna `True`. [`app/chamados/services/feriado_reavaliar.py:43-56`]

`reavaliar_dia` apenas delega para `reavaliar_dias([data], colaborador_id=...)`. O plural deduplica datas com `set`, devolve zero para conjunto vazio, recorta `universo()` por `celula.data` **ou** `contexto_json.data_turno`, filtra colaborador quando passado, obtém usuário sistema e usa `espurios`/`encerrar`. [`app/chamados/services/feriado_reavaliar.py:59-85`]

Chamadores externos desses serviços:

- `_feriado_post_save` agenda `reavaliar_dia(instance.data)` por `transaction.on_commit`; ignora `raw`/sem data e envolve o callback em `engolir`. [`app/chamados/signals.py:265-276`]
- `regenerar_celulas_vinculo`, somente com `recalculo and _tocadas`, agenda callback que primeiro faz `rejuntar_celula_dos_vivos(colaborador)` e depois `reavaliar_dias(dias_tocados, colaborador_id=...)`; ambos envolvidos separadamente em `engolir`. [`app/ponto/portas/celula.py:421-433`]
- O command usa diretamente `universo`, `espurios`, `encerrar`; o cron o agenda `2 7 * * *`, `--apply`, por empresa, dependente de `processar_cartorio`, e mantém o nome em `JUIZES_POR_VARREDURA`. [`app/ponto/management/commands/reavaliar_ausencias_feriado.py:23-32`; `app/config/crons.py:589`; `app/config/crons.py:1983`]

**Ordem exata de `premissa_regularizada`**

1. Justificativa relacionada aprovada → `True`. [`app/chamados/models.py:1214-1215`]
2. `dia_do_fato(self, fortes=True)`; nenhum dia → `False`. [`app/chamados/models.py:1216-1220`]
3. Lê `celula_do_dia_agora`: célula do banco projetada com ata/veredito do julgamento em curso. [`app/chamados/models.py:1227-1228`; `app/chamados/juizes.py:373-395`]
4. Célula com `veredito_via='admin'` → `True`. [`app/chamados/models.py:1229-1231`]
5. Módulo não começa por `turno_aberto` e `dia_sem_trabalho(cel)` → `True`. Esse juiz considera `trabalha is False` **ou** `ata.tipo_dia` presente diferente de `'trabalho'`. [`app/chamados/models.py:1232-1233`; `app/chamados/juizes.py:398-408`]
6. `marcos_acesos(alvo, cel) is True` → `True`; o alvo é o próprio chamado ou `_MarcosDoPedido(marcos)` quando informado. [`app/chamados/models.py:1234-1237`]
7. `marcos_inexistentes(alvo, cel) is True` → `True`. [`app/chamados/models.py:1245-1246`]
8. `_celula_furada(colaborador, dia, para=para)`; valor diferente de `None` → retorna sua negação imediatamente. **Uma célula julgada furada impede chegar a `espurios`.** [`app/chamados/models.py:1247-1249`]
9. Somente se o passo anterior devolveu `None`, `espurios([self])` não vazio → `True`. [`app/chamados/models.py:1250-1254`]
10. Senão coleta `falta_descoberta = not batidas_do_dia_turno(...)` e `turno_aberto = any(...)`; pergunta `catalogo.premissa.avaliar`; em escopo retorna `not viva`, fora do escopo retorna existência de batidas. [`app/chamados/models.py:1255-1264`]

`_celula_furada` pergunta primeiro `ausencia_cobre(para=...)` → `False`; sem célula → `None`; em dia sem trabalho consulta recursivamente a véspera com o mesmo `para`, usando `False` se a véspera não sabe; ata sem chave `'lampadas'` → `None`; lâmpada explicitamente `False` → `True`; órfãs com escala confiável → `True`; restante → `False`; erro exterior → `None`. [`app/chamados/juizes.py:423-469`]

**Todos os chamadores diretos de `premissa_regularizada` em produção**

| Chamador | `True` | `False` |
|---|---|---|
| `transicionar`, guarda S122, apenas destino `'resolvido'` e `e_chamado_de_furo()`; usa default `para='cobranca'`. [`app/chamados/models.py:175-177`] | Passa S122 e chega a `_aplicar`. [`app/chamados/models.py:185-193`] | Sem dia forte, ainda tenta existência de disputa fechada do colaborador; se continuar falso, lança `ValueError`, sem aplicar status. [`app/chamados/models.py:178-193`] |
| `_gate_premissa_do_fechamento`, usa `para='folha'`. [`app/chamados/models.py:699`] | Permite fechamento pela premissa. [`app/chamados/models.py:699-700`] | Ainda permite agrupado sob pai vivo ou dedup quando C8 aponta outro sobrevivente; fora dessas condições registra recusa e retorna `False` (`registrar=False`: nem registra). [`app/chamados/models.py:699-732`] |
| `recusa_da_reabertura`, somente encerrado e chamado de furo, usa `para='folha', marcos=marcos`. [`app/chamados/models.py:1296-1298`] | Retorna motivo `'premissa_regularizada'`; `reabrir` registra `reabertura_recusada` e retorna `False`. [`app/chamados/models.py:1296-1298`; `app/chamados/models.py:1348-1353`] | Prossegue às guardas agrupado, irmão vivo e transição; só sem outras recusas reabre/grava estado/ressuscita perguntas/retorna `True`. Antes do coletor podem recusar veto do dia e admin sem fato novo. [`app/chamados/models.py:1285-1308`; `app/chamados/models.py:1354-1390`] |

O universo `e_chamado_de_furo` inclui módulo em `CALADOS_POR_ISENCAO` ou categoria/subcategoria legada nos mapas. [`app/chamados/models.py:1192-1198`]

`_gate_premissa_do_fechamento` é chamado por `_decisao_do_fechamento`; resultado falso produz `None`. `reconciliar` usa essa decisão, retorna `False` para `None`, e grava estado/retorna `True` em sucesso. `reconciliaria` usa a mesma decisão com `registrar=False` e devolve bool, sem escrita. [`app/chamados/models.py:605-657`]

A Defesa 1 não chama o coletor diretamente: ao fechar disputa, chama `ch.reconciliar()` e só anexa nota se ele retornar verdadeiro. [`app/chamados/signals.py:18-45`]

Outro consumidor indireto é `reabrir_orfas_enterradas`: chama `pai.recusa_da_reabertura()` antes de selecionar a pergunta para reabertura; qualquer motivo (incluindo premissa regularizada), ou irmãs que ressuscitariam junto, faz registrar `PAI_RECUSA` e pular o alvo. [`app/chamados/management/commands/reabrir_orfas_enterradas.py:103-118`]

**Testes localizados que fixam esse comportamento — apenas leitura dos testes, não execução** [nao verificado]

- `test_reavaliar_feriado_pelo_juiz.py`: vínculo **do dia** encerrado que folga em feriado fecha; mesmo vínculo com `trabalha_em_feriado=True` permanece aberto; célula `trabalha=True` que não leu feriado não decide contra o juiz; dia útil sem feriado permanece aberto. [`app/ponto/tests/test_reavaliar_feriado_pelo_juiz.py:79-116`]
- `FeriadoObedeceCatalogoTest`: `faixa='folga'`, previsão falsa e `premissa_morta=False` não fecha; catálogo real e célula regularizada sem trabalho fecham. [`app/ponto/tests/test_reavaliar_catalogo_premissa.py:63-84`]
- `test_varredura_feriado_evento.py`: cadastrar feriado encerra e gera uma trilha; outro dia não é tocado; salvar novamente + cron não duplicam trilha; command deve usar a mesma porta e não conter `premissa_morta`; cron permanece em `JUIZES_POR_VARREDURA`. **O helper regulariza a célula com `trabalha=False` antes de criar Feriado, portanto esse teste não prova relavra causada pelo Feriado.** [`app/chamados/tests/test_varredura_feriado_evento.py:54-96`]
- `test_e2_celula_regenerada_reavalia.py`: regeneração após corrigir trabalho em feriado encerra falta; dia de trabalho e outro colaborador não são tocados; duas portas produzem uma trilha e não emitem novo chamado; criação de célula religa FK; encerramento provoca novo julgamento e permanece fechado sem emissão/ressurreição; chamada direta duas vezes de `reavaliar_dias` resulta `(1,0)` e uma trilha; há ainda teste de custo do callback. [`app/chamados/tests/test_e2_celula_regenerada_reavalia.py:78-179`; `app/chamados/tests/test_e2_celula_regenerada_reavalia.py:193-228`]
- Teste D3 `test_porta_6_feriado_RED` é `@unittest.expectedFailure`: sem chamado vivo no dia, o save de Feriado não acorda o juiz. [`app/ponto/tests/test_d3_impressao_cobre_insumos.py:339-345`]
- Testes auxiliares da fonte: folga de calendário de outro vínculo deixa `faixa='trabalho'`; folga manual do vínculo velho deixa `'folga'`. [`app/ponto/tests/test_precedencia_folga_de_calendario_e_do_vinculo.py:45-52`]
- Testes do coletor/gates: premissa não regularizada sem irmão recusa, regularizada fecha, agrupado permite, marco próprio aceso permite mesmo com outro apagado, folga com ata velha permite, admin permite/cartório furado não; cenários testam que fechamento permitido corresponde à reabertura recusada. [`app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:70-102`; `app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:143-167`; `app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:185-229`; `app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:233-259`]
- `trabalha=None` com ata trabalho/lâmpada apagada continua furado e `premissa_regularizada=False`; ata folga regulariza mesmo com `trabalha=True`. [`app/chamados/tests/test_f1c4_3_none_cobra.py:93-115`]

## 31/10 12:07 · sessao 0e5c54ab034f · E4-INCLUI-CONTRATO-20 · censo · origem=Code

**A autoridade lavrada existe. A troca exige preservar também as perguntas sobre marcos, cobrança e costura:** elas têm respostas próprias, além do status agregado do dia (`app/core/juizes.py:158–173`; `app/chamados/juizes.py:1073–1096`; `app/ponto/services/lastro.py:153–181`). Os quatro nomes constam da tupla indicada (`app/config/crons.py:1955–1984`). Incidência atual em produção e contadores efetivamente zerados: **[nao verificado]**.

A base comum para os quatro:

- **Status:** `CelulaDia.status_dia/status_rotulo`, anuláveis, lavrados pela porta junto ao veredito. `status_do_dia_lavrado` trata `aberto/previsto` pelo relógio antes do encerramento, lê a coluna depois e distingue ausência de célula/lavra (`app/escala/models.py:1775–1796`; `app/ponto/portas/celula.py:686–705`; `app/ponto/status_dia.py:157–185`).
- **Natureza/previsão:** `eh_dia_trabalho` lê `CelulaDia.trabalha`, com cálculo do vínculo quando falta célula; `fatos_do_dia` responde precedência; `dia_sem_trabalho(cel)` lê `trabalha is False` ou `ata.tipo_dia` presente e diferente de trabalho. `trabalha=None` permanece indeterminado (`app/escala/models.py:1048–1073`; `app/ponto/precedencia.py:195–209`; `app/chamados/juizes.py:398–408`). O lookup fornecido separa essas autoridades do status (`app/docs/JUIZES.md:64–75`).
- **Limite:** o status agrega situações: folga com batida pode ser `folga_trabalhada`; ausência parcial pode continuar irregular. Portanto, comparar apenas uma palavra não reproduz automaticamente precedência ou satisfação dos marcos cobrados (`app/ponto/status_dia.py:96–120`; `app/ponto/services/lastro.py:153–181`).

| `disparar_perguntas_competencia` | Censo |
|---|---|
| **(1) Decide status/“tem trabalho”?** | **No comando, lê autoridades:** seleciona `trabalha=True`, consulta `VEREDITOS_ACUSAM` e pede motivos pela ata; não monta um status completo (`app/chamados/management/commands/disparar_perguntas_competencia.py:76–123`). **Há derivação transitiva de natureza:** `grade_da_celula` combina admissão/demissão com minutos realizados ou `n_celulas−n_missing` para produzir `fora_vinculo`; sem `ata.tipo_dia`, traduz `trabalha` em `trabalho/folga/indefinido` (`app/escala/services/leitor_celula.py:332–345`). O consumidor descarta dias cujo tipo seja diferente de `trabalho` (`app/chamados/services/declaracao_texto.py:90–96`). |
| **(1) Outros recuos relevantes** | Sem ata, no passado sem marcos, pede **à autoridade do furo** `classificar_falta`; falha de leitura também pode habilitar esse recuo (`app/chamados/services/fabrica_da_celula.py:28–57`; `app/chamados/services/disputa_emissao.py:825–844`). Essa autoridade silencia folga/cobertura integral, aplica teto e deriva entrada/saída faltante pela presença e contagem de E/S e pelos turnos (`app/escala/servico_jornada.py:302–345`; `app/escala/servico_jornada.py:375–480`). O fio mudo consulta `pergunta_viva`, cujos recuos também chamam essa autoridade; não são uma regra de status criada no comando (`app/chamados/management/commands/disparar_perguntas_competencia.py:149–184`; `app/chamados/juizes.py:640–644`; `app/chamados/juizes.py:726–736`; `app/chamados/juizes.py:1097–1103`). |
| **(2) Já existe resposta lavrada?** | Para **status**, sim: leitor e coluna da base comum (`app/ponto/status_dia.py:157–185`; `app/escala/models.py:1784`). Para **qual marco perguntar**, a resposta é a ata: `_marcos_faltantes` lê lâmpadas, posição, tipo e hora; o enum de status não fornece esses dados (`app/chamados/services/declaracao_texto.py:97–124`). |
| **(3) O que mais decide/escreve?** | Define janela por `janela_atual`, vencimento por marco e identidade já perguntada; trata separadamente célula e chamado vivo sem pergunta viva (`app/chamados/management/commands/disparar_perguntas_competencia.py:58–62`; `app/chamados/management/commands/disparar_perguntas_competencia.py:108–129`; `app/chamados/management/commands/disparar_perguntas_competencia.py:155–226`). O escritor desfaz carimbo sem lastro, abre/agrega disputa e cria perguntas (`app/chamados/services/fabrica_da_celula.py:112–124`). A emissão agrega contexto/contadores, abre chamado-pai e disputa; o SLA do pai delega ao prazo de cobrança de **20 minutos** (`app/chamados/services/disputa_emissao.py:476–535`; `app/chamados/catalogo/motor.py:45–51`). Pergunta nova sem destino pode ainda reatar e abrir/renascer o chamado do dia pelo c2 (`app/chamados/services/disputa_emissao.py:398–410`; `app/chamados/reconciliador.py:866–909`). |
| **(4) Quem escreveria como evento?** | **Já existe para a célula julgada:** o cartório chama `perguntar_pela_celula`, que usa o mesmo escritor `perguntar` (`app/ponto/services/cartorio.py:809–828`; `app/chamados/services/fabrica_da_celula.py:143–149`). **Falta cobertura demonstrada do fio mudo sem novo julgamento:** o cron percorre chamados independentemente das células julgadas naquela passada; a lacuna está declarada no próprio comando (`app/chamados/management/commands/disparar_perguntas_competencia.py:9–13`; `app/chamados/management/commands/disparar_perguntas_competencia.py:131–168`). Há ainda diferença de escrita: o julgamento desfaz carimbos apenas em disputa aberta, enquanto `perguntar` usa o default que permite reabrir disputa fechada (`app/chamados/services/fabrica_da_celula.py:119–124`; `app/chamados/services/fabrica_da_celula.py:143–146`; `app/chamados/services/relavra_via.py:185`; `app/chamados/services/relavra_via.py:218–249`). |
| **Vigia atual** | `--alarme` já retorna sem executar o escritor e sai com código 2 havendo alvos; a configuração continua `--comp-corrente --apply` (`app/chamados/management/commands/disparar_perguntas_competencia.py:261–268`; `app/config/crons.py:1464`). Equivalência integral evento/lote: **[nao verificado]**. |

| `fechar_cobranca_com_lastro` | Censo |
|---|---|
| **(1) Decide status/“tem trabalho”?** | Não calcula o status completo nem a previsão de trabalho; classifica **lastro dos marcos cobrados**. Com lâmpada, lê `acesa`: acesa fornece lastro; apagada produz `furo_real` (`app/ponto/services/lastro.py:250–273`). **Sem lâmpada daquele marco, deriva presença/lastro novamente:** sem turno pareado → `sem_batida`; com turno, exige marca do mesmo tipo dentro da tolerância, padrão **90 minutos**; sem candidata → `outro_horario`; com candidata escolhe a mais próxima. Todos os marcos devem passar (`app/ponto/services/lastro.py:59`; `app/ponto/services/lastro.py:245–247`; `app/ponto/services/lastro.py:274–296`). |
| **(2) Já existe resposta lavrada?** | Status: base comum (`app/ponto/status_dia.py:157–185`). Para a pergunta efetiva, **`marcos_acesos(ch, cel)` já responde `True/False/None`**, lendo todos os marcos nomeados com hora e tipo; desconhecimento não concede lastro (`app/ponto/services/lastro.py:153–181`). O próprio marco pode estar resolvido enquanto outro furo do dia permanece; status agregado não substitui essa resposta (`app/ponto/services/lastro.py:160–168`). |
| **(3) O que mais decide/escreve?** | Seleciona a fila `batida_ausente`, obtém o dia por `dia_do_fato`, pareia turnos e separa ausência de marco/tipo, furo real e outro horário (`app/ponto/services/lastro.py:209–218`; `app/ponto/services/lastro.py:243–296`). Não limita a hoje/ontem: busca desde a menor data da fila (`app/ponto/services/lastro.py:232–240`). Recarrega o estado, exige chamado ainda vivo e só após `reconciliar()` aceitar acrescenta ocorrência **`lastro_encontrado`** (`app/ponto/services/lastro.py:300–323`). |
| **(4) Quem escreveria como evento?** | **O fechamento por marco já existe no cartório**, acionado pela batida: consulta `marcos_acesos` na ata deste julgamento e reconcilia os chamados satisfeitos (`app/ponto/signals.py:85–112`; `app/ponto/signals.py:131–135`; `app/ponto/services/cartorio.py:1182–1205`; `app/ponto/services/cartorio.py:1275–1287`). Quando há fio, a sequência é lavrar → E2 carimbar perguntas absolvidas → reconciliar → segunda lavra (`app/ponto/services/cartorio.py:776–801`). **A nota específica ainda precisa de destino:** ela é escrita pelo serviço do cron; o fechamento por marco mostrado no cartório reconcilia/conta/rejulga, sem essa ocorrência (`app/ponto/services/lastro.py:319–322`; `app/ponto/services/cartorio.py:1281–1287`). Substituição integral do recuo sem lâmpada: **[nao verificado]**. |
| **Vigia atual** | `contador()` já declara esperado 0, mas conta **`com_lastro`**, enquanto a escrita depende também da aceitação de `reconciliar()` (`app/ponto/services/lastro.py:334–344`; `app/ponto/services/lastro.py:317`). Para contar fechamento realmente autorizado existe `reconciliaria()`, sem escrita nem trilha (`app/chamados/models.py:621–625`). |

| `reavaliar_ausencias_feriado` | Censo |
|---|---|
| **(1) Decide status/“tem trabalho”?** | **Delega a uma autoridade.** O comando chama `feriado_reavaliar.espurios`; o serviço obtém o dia por `dia_do_fato`, lê `fatos_do_dia`, ignora previsão desconhecida e aceita somente faixa `feriado/folga`. Traduz esse resultado em `falta_descoberta=False` para o catálogo decidir a premissa morta (`app/ponto/management/commands/reavaliar_ausencias_feriado.py:23–32`; `app/chamados/services/feriado_reavaliar.py:23–39`). |
| **(1) Onde a regra do dia é derivada?** | **Dentro da autoridade:** precedência lê `trabalha` da célula; sem resposta, chama o cálculo do vínculo. Feriado não suprime previsão quando `trabalha_em_feriado=True`; os fatos presentes são ordenados pela precedência declarada (`app/ponto/precedencia.py:316–350`). Não há aritmética de escala duplicada no comando/serviço de reavaliação (`app/chamados/services/feriado_reavaliar.py:30–38`). |
| **(2) Já existe resposta lavrada?** | Sim, status e leitor comuns; para “sem trabalho” existe `dia_sem_trabalho` (`app/escala/models.py:1784`; `app/ponto/status_dia.py:157–185`; `app/chamados/juizes.py:398–408`). **A equivalência exige contrato:** a seleção atual pede faixa feriado/folga **e previsão conhecida**; status pode responder `folga_trabalhada`, enquanto `dia_sem_trabalho` também reconhece outros tipos da ata (`app/chamados/services/feriado_reavaliar.py:34–38`; `app/ponto/status_dia.py:115–120`; `app/chamados/juizes.py:407–408`). |
| **(3) O que mais decide/escreve?** | Universo de `batida_ausente` vivos, opcionalmente por empresa; grava somente com `--apply` (`app/chamados/services/feriado_reavaliar.py:14–20`; `app/ponto/management/commands/reavaliar_ausencias_feriado.py:28–32`). A porta chama `reconciliar()` e, se aceito, acrescenta ocorrência **`falta_espuria_feriado`** e auditoria **`reconciliar_feriado`** (`app/chamados/services/feriado_reavaliar.py:43–55`). |
| **(4) Quem escreveria como evento?** | **Os mesmos escritores já estão ligados a dois eventos:** `core.Feriado.post_save → on_commit → reavaliar_dia`; regeneração com `recalculo` e dias tocados → `reavaliar_dias`. Ambos chegam ao mesmo `espurios → encerrar`, preservando ocorrência e auditoria (`app/chamados/signals.py:265–276`; `app/ponto/portas/celula.py:421–433`; `app/chamados/services/feriado_reavaliar.py:76–84`). |
| **O que impede aposentar a escrita?** | Permanecem fora do segundo evento: regeneração sem recálculo, ausência de dias tocados e reversão; o código também declara alteração de natureza sem mudança de célula e acervo como cobertura residual (`app/ponto/portas/celula.py:421`; `app/ponto/portas/celula.py:529–552`; `app/chamados/signals.py:259–263`). **Inferência de dependência:** antes de trocar `fatos_do_dia` por coluna, o evento de Feriado precisa garantir lavra atualizada: hoje ele chama diretamente a reavaliação; o julgamento provocado pela mudança do chamado ocorre depois (`app/chamados/signals.py:271–276`; `app/chamados/signals.py:165–180`). |

| `reconciliar_chamados` | Censo |
|---|---|
| **(1) Decide status/“tem trabalho”?** | Não monta o status agregado. **Deriva “turno aberto” no coletor:** consulta entrada presente/saída ausente na janela dia−1/dia e veta o resultado se todos esses dias estiverem posicionalmente completos; sem dia usa existência global (`app/chamados/reconciliador.py:95–114`; `app/chamados/reconciliador.py:161–178`). O recorte do turno denunciado usa entrada declarada ou `data_turno` (`app/chamados/reconciliador.py:139–158`). Isso sustenta a premissa do chamado; não equivale ao status agregado `aberto` do leitor oficial (`app/chamados/reconciliador.py:286–301`; `app/ponto/status_dia.py:175–183`). |
| **(1) Outros sítios de decisão do dia** | A absolvição lê veredito lavrado fora de `VEREDITOS_ACUSAM`, exigindo ausência de marcos pendentes (`app/chamados/reconciliador.py:973–986`). O c2 lê ausência que suspende cobrança, protestos cobrados, veto da célula e veto da régua (`app/chamados/reconciliador.py:825–860`). O juiz compartilhado `pergunta_viva` prioriza resposta/admin, lâmpada e veredito e conserva recuos para `classificar_falta` (`app/chamados/juizes.py:1032–1103`). São leituras/composição dessas autoridades, além do coletor de turno acima (`app/chamados/reconciliador.py:247–301`). |
| **(2) Já existe resposta lavrada?** | Status: base comum (`app/ponto/status_dia.py:157–185`). Premissa do chamado: catálogo `avaliar`; vitalidade da pergunta: `pergunta_viva`; satisfação da cobrança: lâmpadas e `marcos_acesos`. São perguntas diferentes, já separadas no código (`app/chamados/catalogo/premissa.py:115–150`; `app/chamados/juizes.py:1032–1118`; `app/ponto/services/lastro.py:153–181`). |
| **(3) O que mais decide/escreve?** | Retrata premissa morta; na unidade atômica resolve perguntas, rejeita justificativa automática ainda pendente e acrescenta ocorrência, respeitando retenções (`app/chamados/reconciliador.py:439–455`; `app/chamados/reconciliador.py:505–542`; `app/chamados/reconciliador.py:1362–1374`). Carimba perguntas absolvidas e pode fechar disputas (`app/chamados/reconciliador.py:611–642`). Reata órfãs a candidato vivo/histórico, pode carimbar `sem_turno_aberto` ou reemitir chamado do dia; consulta horizonte, competência entregue/trancada e motor (`app/chamados/reconciliador.py:689–731`; `app/chamados/reconciliador.py:793–913`; `app/chamados/reconciliador.py:1460–1561`). Também religa chamado vivo sem célula à célula existente e drena o outbox (`app/chamados/rejuncao.py:59–81`; `app/chamados/management/commands/reconciliar_chamados.py:50–71`). |
| **(4) Eventos já existentes** | **Batida:** chama o mesmo `reconciliar_chamados_colab`, cobrindo carimbos, reatações e retratação por premissa (`app/ponto/signals.py:206–215`; `app/chamados/reconciliador.py:1354–1374`). **Nascimento da pergunta órfã:** chama `reatar_pergunta_orfa`, inclusive c2 (`app/chamados/services/disputa_emissao.py:398–410`). **Regeneração:** já chama a mesma rejunção de células (`app/ponto/portas/celula.py:421–433`). **Lavra/nascimento de chamado de turno:** `EventoCelula` alimenta consumidor que retrata chamados absolvidos e encerra órfãs pela lâmpada (`app/escala/models.py:1813–1821`; `app/chamados/reconciliador.py:1304–1310`). |
| **O que ainda exige destino/cobertura?** | O consumidor de `EventoCelula` chama essas duas operações, sem executar toda a reconciliação, reatação ou rejunção do cron (`app/chamados/reconciliador.py:1304–1310`; `app/chamados/management/commands/reconciliar_chamados.py:50–62`). Evento de nascimento de órfã também não demonstra reavaliação quando a ambiguidade desaparece posteriormente (`app/chamados/services/disputa_emissao.py:404–410`; `app/chamados/reconciliador.py:1416–1424`). Cobertura integral dos fatos de cadastro, filhos e pares sem nova batida: **[nao verificado]**. Os vigias existentes cobrem apenas absolvidos sem evento e eventos mortos (`app/chamados/reconciliador.py:1201–1224`). |

**Ordem sugerida, por dependência de código:**

1. **Primeiro, acertar o contrato comum de leitura e evento.** Distinguir status, natureza, marco e premissa; definir comportamento de célula/status ausentes. Se consumidores passarem a depender de `status_dia`, revisar o despertar: hoje apenas `veredito/ata` acordam o outbox, e alteração isolada de status deliberadamente não o acorda (`app/ponto/status_dia.py:173–185`; `app/ponto/portas/celula.py:622–642`; `app/ponto/portas/celula.py:753–760`).

2. **`reavaliar_ausencias_feriado`.** Resolver primeiro atualização prévia da lavra e eventos residuais. Seu `espurios` também é fallback de `premissa_regularizada`, usado pelos gates de fechamento/reabertura; alterá-lo repercute nos demais fechadores (`app/chamados/models.py:1200–1208`; `app/chamados/models.py:1247–1264`; `app/chamados/signals.py:271–276`).

3. **`fechar_cobranca_com_lastro`.** Com a premissa estabilizada, eliminar ou dar destino explícito ao recuo sem lâmpada e à nota; preservar fechamento por marco e a sequência lavrar → E2 → reconciliar. Essa sequência antecede a fábrica de perguntas no cartório (`app/ponto/services/lastro.py:274–288`; `app/ponto/services/lastro.py:317–322`; `app/ponto/services/cartorio.py:776–825`).

4. **`disparar_perguntas_competencia`.** Cobrir fio mudo e resolver a diferença sobre disputa fechada antes de ativar o alarme já pronto. Essa fábrica cria perguntas e pode alimentar o c2 que o reconciliador mantém (`app/chamados/management/commands/disparar_perguntas_competencia.py:131–168`; `app/chamados/services/fabrica_da_celula.py:119–149`; `app/chamados/services/disputa_emissao.py:398–410`).

5. **`reconciliar_chamados`, por último.** Consolidar os destinos das escritas restantes depois de estabilizar os emissores e fechadores que ele reconcilia. Hoje o comando acumula reconciliação, rejunção e consumo do outbox; zerar apenas um desses subconjuntos não demonstra aposentadoria do conjunto (`app/chamados/management/commands/reconciliar_chamados.py:50–71`). O zero necessário para concluir qualquer retirada continua **[nao verificado]**.

## 31/10 12:07 · sessao 56f34006715a · F1C4-3-EMISSORES · verificador · origem=Code

**Reprovo a volta 3 pelo aval literal:** o código preserva perguntas novas para `trabalha=True` em competência entregue, e outros emissores contornam o portão novo. O teste de trabalho exportado protege justamente essa exceção. ([fabrica_da_celula.py:153](/home/ronald/codex_ws/c5c433608d2ebd3062b25cf5cfa7a1782a80cc20/app/chamados/services/fabrica_da_celula.py:153); `app/chamados/management/commands/disparar_perguntas_competencia.py:277–289`; `app/chamados/tests/test_f1c4_3_emissores.py:211–214`)

Execução dos testes, execução das mutações e efeitos reais sobre os números de produção: **[nao verificado]**. [nao verificado]

1. **Chamadores e tratamento de `None`.**

   O único chamador de produção de `perguntar_pela_celula` é `cartorio.julgar_colab`: chama para células julgadas naquela passagem e incrementa `perguntas_no_julgamento` somente quando o retorno contém motivos. (`app/ponto/services/cartorio.py:784–803`)

   Os demais chamadores executáveis estão nos testes: [nao verificado]

   | Arquivo | Linhas das chamadas |
   |---|---|
   | `app/chamados/tests/test_fabrica_por_evento.py` | 36, 39, 46, 54 |
   | `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py` | 83 |
   | `app/chamados/tests/test_pergunta_do_dia_em_curso.py` | 74, 82 |
   | `app/chamados/tests/test_f1c4_3_emissores.py` | 187, 222 |

   `competencia_do_dia_entregue` tem **cinco chamadores de produção**, com tratamentos diferentes:

   | Chamador | Tratamento de `None` |
   |---|---|
   | `carimbos_sem_lastro_por_dono` | O ternário trata como falso e põe no balde `aberta`. (`app/chamados/services/vigia_fio.py:768–771`) |
   | `_dia_fora_do_alcance_c` | Dia ausente fica **fora** antes da chamada. Com dia válido, a expressão usa `bool(resultado)`; um `None` artificial seria falso e seguiria para a consulta de tranca. (`app/chamados/reconciliador.py:709–720`) |
   | `ChamadoColaborador._vivas_entre` | Usa `is not False`: `None` fica fora das perguntas que seguram o encerramento. (`app/chamados/models.py:341–350`) |
   | `_costura_nao_mata_o_fio` | Usa `is not False`: `None` libera a costura sem aplicar a proteção da pergunta viva em competência aberta. (`app/chamados/services/fio.py:767–779`) |
   | `perguntar_pela_celula`, novo | Usa verdade booleana: `None` **não manda à Pauta**, prossegue para `perguntar` e retorna os motivos. (`app/chamados/services/fabrica_da_celula.py:153–161`) |

   O chamador direto de teste da composição é `test_MORDE_a_funcao_e_os_dois_chamadores_concordam`, com chamadas nas linhas 382, 383 e 388. (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:382–388`)

   O helper só produz `None` quando `dia is None`. Isso não é uma ocorrência normal demonstrada para a fábrica: `CelulaDia.data` é obrigatório e `motivos_a_perguntar` já compara essa data com `hoje` antes do portão. Portanto há tratamento permissivo de `None`, mas sua ocorrência nesse fluxo real é **[nao verificado]**. (`app/chamados/services/vigia_fio.py:787–797`; `app/escala/models.py:1653`; `app/chamados/services/fabrica_da_celula.py:82–83`)

2. **Combinações que mudam e caminhos que contornam o portão.**

   Considerando `ms` como a lista **já filtrada** por `motivos_a_perguntar`, a única combinação que muda no delta fornecido é **`trabalha=None`, competência entregue, `ms` não vazia**. (`app/chamados/services/fabrica_da_celula.py:146–161`)

   | `trabalha` | Entregue? | `ms` | Antes → depois |
   |---|---|---|---|
   | `True` | não | vazia | `[]` → `[]` |
   | `True` | não | não vazia | pergunta → pergunta |
   | `True` | sim | vazia | `[]` → `[]` |
   | `True` | sim | não vazia | pergunta → pergunta |
   | `False` | não | vazia | `[]` → `[]` |
   | `False` | não | não vazia | Inalcançável pelo filtro anterior |
   | `False` | sim | vazia | `[]` → `[]` |
   | `False` | sim | não vazia | Inalcançável pelo filtro anterior |
   | `None` | não | vazia | `[]` → `[]` |
   | `None` | não | não vazia | pergunta → pergunta |
   | `None` | sim | vazia | `[]` → `[]`, sem Pauta |
   | `None` | sim | não vazia | **pergunta → Pauta DP, retorno `[]`** |

   A tabela decorre do novo ramo nas linhas 153–157; os casos `False` com motivos são excluídos por `dia_sem_trabalho`. Mesmo `True` ou `None` podem resultar em lista vazia por outras condições, inclusive `ata.tipo_dia` diferente de trabalho. (`app/chamados/services/fabrica_da_celula.py:78–111,153–161`; `app/chamados/juizes.py:398–408`)

   **Há caminhos sobre o mesmo dia que continuam cobrando sem passar por essa função:**

   - **Lote `disparar_perguntas_competencia`:** o primeiro universo exige `trabalha=True`; o universo de fio mudo aceita chamado vivo com célula na janela **sem filtro de `trabalha`**. Ambos terminam chamando `fabrica_da_celula.perguntar` diretamente, sem consultar entrega. Assim, um limbo exportado com chamado vivo, motivo elegível e identidade ainda inexistente pode ganhar pergunta por esse segundo universo. (`app/chamados/management/commands/disparar_perguntas_competencia.py:76,155–168,220–224,276–301`)
   - **Cartório → detector → fábrica de órfãos:** o cartório pode abrir chamado pelo detector; este chama `criar_disputa_a_partir_de_chamados_orfaos`, que cria/complementa perguntas pela ata. `_criar_pergunta_se_couber` verifica ausência e dedup, sem consulta de entrega. Esses atos podem acontecer antes de `perguntar_pela_celula`. (`app/ponto/services/cartorio.py:753,800,1450–1507`; `app/ponto/management/commands/detectar_ausencias.py:688–707,782–796`; `app/chamados/services/disputa_emissao.py:756–779,966–981`)
   - Esse segundo caminho **não prova que qualquer `trabalha=None` o percorra**: os ramos do cartório exigem `ec.eh_dia_trabalho(data) is True`, e o supra-juiz trata `sem_ancora` separadamente. O bypass para dia exportado existe; sua incidência em cada limbo depende dessas condições. (`app/ponto/services/cartorio.py:1459,1557–1559`; `app/ponto/supra_juiz.py:325–328,379–403`)
   - **Pergunta existente → chamado `(c2)`:** `reemitir_chamado_do_dia` aplica a cerca de competência aberta somente aos motivos fora de `MOTIVOS_C1`. Os motivos C1 continuam podendo reemitir chamado em competência entregue, sujeitos aos demais gates. (`app/chamados/reconciliador.py:781–783,793–865,866–877`)

3. **Escritores, leitores e deduplicação de Pauta de dia.**

   A criação efetiva continua pela porta única `pautas.services.escrever`, que faz `Pauta.objects.create`. O delta usa essa porta e `autor_sistema('ti')`; não introduz escritor direto do modelo. (`app/pautas/services.py:152–180`; `app/chamados/services/fabrica_da_celula.py:173–187`)

   Os caminhos de escrita de âncora de dia são:

   | Caminho | Escrita |
   |---|---|
   | Novo limbo exportado | TI → DP, `colaborador_id:data ISO`. (`app/chamados/services/fabrica_da_celula.py:175–187`) |
   | Cobertura passada sem nome | Para Supervisão, mesma composição de âncora. (`app/ponto/services/cobertura.py:234–235,329–337`) |
   | View genérica de escrita | Recebe tipo/id e chama a porta. (`app/pautas/views.py:72–82`) |
   | Resposta | Cria filha herdando a âncora original. (`app/pautas/services.py:184–192`) |

   **O formato novo está correto:** o modelo declara `<colab_id>:<AAAA-MM-DD>`, e o parser divide no primeiro `:` e converte para inteiro/data. A cobertura, a busca e o compositor usam o mesmo contrato. (`app/pautas/models.py:33,84–87`; `app/pautas/services.py:373–379`; `app/ponto/services/cobertura.py:234–243`; `app/core/busca.py:179–186`; `app/pautas/views.py:129–136`)

   Os leitores são a própria dedup nova; `pautas_a_confirmar`; parser/rótulo/contexto; `pautas_do_colab`; listagem/payload genéricos; chips por âncora; e apresentação no detalhe do colaborador. O leitor de cobertura exige Supervisão e seu próprio marcador, portanto não confunde a nova pauta DP com cobertura. (`app/chamados/services/fabrica_da_celula.py:177–179`; `app/ponto/services/cobertura.py:238–243`; `app/pautas/services.py:303–330,373–427`; `app/pautas/views.py:33–48`; `app/templates/colaboradores/detalhe.html:113–119`)

   A alimentação efetiva do template por `pautas_do_colab` é **[nao verificado]**: existe a função e existe o uso da variável no template, mas isso não demonstra a ligação entre ambos. (`app/pautas/services.py:423–427`; `app/templates/colaboradores/detalhe.html:113–119`)

   **A deduplicação tem estes limites:**

   - **Resposta simples:** mantém a raiz viva, pois responder apenas cria filha. A dedup continua enquanto a raiz conservar prefixo e tokens. (`app/pautas/services.py:184–192`; `app/pautas/models.py:57–64`; `app/chamados/services/fabrica_da_celula.py:177–180`)
   - **Feita/cancelada:** sai de `vivas()`; persistindo o motivo, a próxima execução pode criar outra raiz. O modelo não possui estado `arquivada`: possui `feito_em` e `cancelado_em`. A garantia implementada é “enquanto houver pauta viva reconhecível”, não “uma vez no histórico por dia/motivo”. (`app/pautas/models.py:57–64,89–102`; `app/chamados/services/fabrica_da_celula.py:170–187`)
   - **Truncamento:** pôr os tokens no início não basta. `escrever` reserva `500-len(contexto)-4` caracteres para a mensagem e depois corta tudo em 500; o contexto inclui todas as batidas, sem limite de comprimento. Com contexto de 480 caracteres, por exemplo, sobram apenas 16 para a mensagem, perdendo o prefixo completo e permitindo reemissão. A ocorrência desse tamanho em produção é **[nao verificado]**. (`app/pautas/services.py:168–177,410–420`; `app/chamados/services/fabrica_da_celula.py:164,177–187`)
   - **Concorrência:** consulta e criação não compartilham trava nem unicidade dia/motivo/marcador. Duas execuções podem consultar ausência antes de qualquer criação; o `atomic` interno de `escrever` começa depois da consulta. (`app/chamados/services/fabrica_da_celula.py:177–183`; `app/pautas/services.py:152–172`; `app/pautas/models.py:112–132`)

   Outros escritores automáticos com marcador mostram que a casa usa tanto dedup por pauta viva quanto dedup histórica:

   | Escritor | Como deduplica |
   |---|---|
   | Esmeril | Vivas, posto/Supervisão/prefixo; chave `(posto, ação)`, abre/fecha por diferença de conjuntos. (`app/ponto/services/pautas_do_esmeril.py:63–79`) |
   | Deslogados | Vivas por prefixo, chave colaborador e diferença de conjuntos. (`app/ponto/services/pautas_dos_deslogados.py:63–83`) |
   | Discordantes | Vivas por prefixo e chave extraída do texto. (`app/ponto/services/pautas_dos_discordantes.py:154–185`) |
   | Passivo validado | Competência+cabeçalho+raiz, incluindo histórico. (`app/chamados/services/passivo_validado.py:158–166,253–258`) |
   | HE no período | Cabeça viva por competência/DP/marcador; novos atos como filhas. (`app/ponto/services/autorizacao_he_periodo.py:236–246`) |
   | Ausência sem documento/férias recusadas | Colaborador/DP/marcador, não feita nem cancelada. (`app/ponto/services/ausencia.py:1022–1028,1211–1218`) |
   | Afastado avisa | Colaborador/raiz/Supervisão/viva, primeiros 40 caracteres. (`app/ponto/services/afastado_avisa.py:62–78`) |
   | Tranca | Competência, `[TRANCA]`, viva. (`app/chamados/services/competencia_trancada.py:209–224`) |
   | Órfãs enterradas | Competência, marcador, viva. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:182–198`) |
   | Dia em aberto | Competência/marcador/viva; complementação condicionada às filhas. (`app/pautas/management/commands/pauta_dia_em_aberto.py:104–129`) |
   | Pautas de férias | Competência/DP/marcador/viva. (`app/ferias/management/commands/pautas_de_ferias.py:149–170`) |
   | Semanal | Competência/departamento/semana, incluindo feitas e canceladas. (`app/pautas/semanal.py:195–199,238–240`) |
   | Pré-fechamento | Competência/DP/prefixo com data/viva; fecha anteriores. (`app/folha/management/commands/pre_fechamento.py:89–111`) |
   | Crons quebrados | Viva/TI/marcador+cron+empresa. (`app/core/services/crons_quebrados.py:51–72`) |
   | Alarmes árvore/esteira | Uma viva por prefixo ou chave. (`app/core/management/commands/alarme_arvore.py:11–16,29–37`; `app/core/management/commands/alarme_esteira.py:10–15,28–40`) |
   | Aparelhos compartilhados | Escreve texto marcado sem consulta prévia de dedup. (`app/colaboradores/management/commands/pauta_aparelhos_compartilhados.py:95–110`) |

4. **Autoridade de competência entregue.**

   **A composição escolhida é a autoridade adequada para “o dia pertence a uma competência entregue?”** Ela usa F5, `janela_atual`, e K6, `competencia_entregue`, ambos registrados; não cria predicado paralelo. K6 exige exportação vigente não invalidada, e F5 respeita o corte da empresa. (`app/chamados/services/vigia_fio.py:775–797`; `app/core/juizes.py:258,278–287`; `app/folha/export.py:569–575`; `app/ponto/janelas.py:60–63`)

   Existem respostas diferentes para perguntas diferentes:

   | Autoridade | Predicado |
   |---|---|
   | F6, `competencia_fechada` | Fechamento do colaborador aprovado/exportado; guarda de escrita. (`app/ponto/janelas.py:75–89`) |
   | K4, `competencia_trancada` | `PeriodoFechado` sem reabertura. (`app/core/juizes.py:276`; `app/ponto/services/fechamento.py:1441–1448`) |
   | K9, `competencia_fechada_para_celula` | K6 **ou** holerite publicado do próprio colaborador. (`app/core/juizes.py:290–297`; `app/folha/export.py:578–609`) |
   | `marco_da_competencia` | Rótulo de tela derivado dos marcos; `aberta` quando não há marco. (`app/ponto/services/fechamento.py:1486–1523`) |

   Essas funções não são segundos juízes de **entrega**: o registro separa explicitamente K6 de holerite/tranca. O antigo `dia_em_competencia_exportada` está removido, com lápides. (`app/core/juizes.py:278–297,589–592`; `app/chamados/juizes.py:1665–1682`)

   Neste contexto de cobrança, já existe uso de **aberta = não entregue** no vigia e na janela da lista. Portanto não é necessário inventar predicado novo para aplicar essa distinção; o problema do delta é aplicá-la somente a `None`. (`app/chamados/services/vigia_fio.py:473–475,764–771`; `app/ponto/services/esmeril_espelho.py:38–59`; `app/chamados/services/fabrica_da_celula.py:153–160`)

5. **Os quatro testes e suas mutações.** [nao verificado]

   As mutações abaixo são consequências da leitura estática; execução efetiva: **[nao verificado]**.

   | Teste | Mutação de produção que o torna vermelho | [nao verificado]
   |---|---|
   | `test_RED_limbo_em_exportada_nao_pergunta_e_vai_a_pauta_dp` | Remover o portão faz retornar motivos e seguir perguntando; remover só a escrita faz faltar a pauta exigida. (`app/chamados/tests/test_f1c4_3_emissores.py:199–205`; `app/chamados/services/fabrica_da_celula.py:153–160`) |
   | `test_limbo_em_aberta_pergunta_sem_pauta` | Substituir a consulta de entrega por `True`: devolve `[]` e escreve pauta. (`app/chamados/tests/test_f1c4_3_emissores.py:207–209`; `app/chamados/services/fabrica_da_celula.py:155–157`) |
   | `test_MORDE_trabalho_em_exportada_segue_perguntando_sem_pauta` | Alargar `is None` para `is not False`: trabalho exportado cai no desvio. **Esse teste protege comportamento contrário ao aval literal fornecido.** (`app/chamados/tests/test_f1c4_3_emissores.py:211–214`; `app/chamados/services/fabrica_da_celula.py:153–157`) |
   | `test_RED_limbo_em_exportada_duas_vezes_uma_pauta` | Trocar `novos` por `list(motivos)`: a segunda passagem escreve outra pauta. (`app/chamados/tests/test_f1c4_3_emissores.py:216–223`; `app/chamados/services/fabrica_da_celula.py:177–187`) |

   **Nenhum dos quatro é meramente fixture vazia:** há entrada acesa, saída apagada, ata de trabalho, veredito `furo`, data passada e vínculo vigente. Os casos exportados exigem pauta; os outros exigem retorno não vazio. (`app/chamados/tests/test_f1c4_3_emissores.py:42–46,63–72,199–223`; `app/chamados/tests/_vinculo_do_dia.py:15–26`)

   O teste de duas passagens alcança a dedup: o primeiro desvio não cria pergunta, logo a consulta de perguntas existentes não elimina o motivo da segunda passagem. (`app/chamados/services/fabrica_da_celula.py:103–111,155–157`; `app/chamados/tests/test_f1c4_3_emissores.py:218–223`)

   **Há um mutante importante sobrevivente:** retirar a chamada a `perguntar`, preservando `return ms`, deixa verdes os testes de aberta e trabalho exportado, pois eles não contam `PerguntaDisputa`. Também faltam casos de `None` da composição, pauta feita/cancelada, truncamento, concorrência e emissores alternativos. (`app/chamados/services/fabrica_da_celula.py:158–161`; `app/chamados/tests/test_f1c4_3_emissores.py:199–223`)

6. **Demais leitores de limbo e mudanças nos contadores.**

   O desvio não transforma a célula em dia sem trabalho: `dia_sem_trabalho` continua aceitando `None` como indeterminado, e `_celula_furada` continua podendo acusar seus marcos apagados. `celula_veta_cobranca` também não veta apenas por `None`. (`app/chamados/juizes.py:398–408,443–458`; `app/ponto/marcos.py:151–163`)

   | Leitor | Consequência relevante |
   |---|---|
   | Cartório | `None` passa em `_protesta`; cobrança viva vem do fio de perguntas, não de Pauta. (`app/ponto/services/cartorio.py:1100–1105,1166–1172`) |
   | Supra-juiz/protestos | Sem fio, pode acusar falta de cobrança; porém `sem_ancora` recebe antes `PREVISTO_COM_DIA_INDEFINIDO`. Não é correto afirmar que todo limbo virará `FURO_SEM_COBRANCA`. (`app/ponto/supra_juiz.py:281–339`) |
   | Contador do julgamento | O novo `[]` deixa de incrementar `perguntas_no_julgamento`; o retorno não registra quantas pautas nasceram. (`app/ponto/services/cartorio.py:800–803`; `app/chamados/services/fabrica_da_celula.py:155–157`) |
   | Vigia de fio | Seu universo aceita célula acusadora com chamado vivo sem exigir `trabalha=True`. Pauta não integra os donos reconhecidos. (`app/chamados/services/vigia_fio.py:289–298,317–362`) |
   | Vigia de alcance | Usa competências não entregues e portas existentes; o desvio novo não acrescenta uma porta de Pauta. (`app/chamados/services/vigia_fio.py:473–481,523–567`) |
   | `furos_sem_cobranca` | Lê `furo` e protestos cobráveis na janela atual, sem filtro `trabalha=True`; não possui tratamento Pauta/exportada. (`app/ponto/services/furos_sem_cobranca.py:55–63,82–114`) |
   | `furos_vetados` | Lê células `furo` e veto de régua, sem exigir `True`; Pauta não participa. (`app/ponto/services/furos_vetados.py:61–70`) |
   | Materialização, cluster e intervalo | Consultam o juiz de dia sem trabalho; o desvio da pergunta não altera suas condições de elegibilidade. (`app/chamados/services/materializacao.py:158`; `app/ponto/services/cluster_espurio.py:24–28`; `app/ponto/management/commands/detectar_intervalo_ausente.py:47–51`) |
   | Vigias de hora/entrada e previstos da fila | Exigem `trabalha=True`, portanto continuam excluindo `None`. (`app/ponto/services/vigia_de_hora.py:21–23`; `app/ponto/services/vigia_entrada.py:78–85,120`; `app/chamados/services/fila_por_causa.py:192–194`) |
   | Reconciliador da grade | Consulta `dia_sem_trabalho`; não mata somente pelo `None`. (`app/ponto/management/commands/reconciliar_grade.py:280–291`) |
   | Tripwire de folga | Inclui explicitamente `False`/NULL e julga a ata; independe de pergunta/Pauta. (`app/ponto/management/commands/tripwire_celulas.py:337–350`) |

   **Efeito possível demonstrado:** se restar chamado vivo, célula acusadora e nenhuma pergunta viva, sem os outros donos reconhecidos, o vigia contará `fio_morto`/`sem_pergunta_nenhuma`, mesmo existindo a nova Pauta DP. A ocorrência concreta após o delta é **[nao verificado]**. (`app/chamados/services/vigia_fio.py:289–298,392–421`)

   Há leitores anteriores que ainda reduzem `None` a falso: `dia_do_colab` descreve-o como dia sem trabalho previsto; `passivo_s84` classifica-o como `FOLGA`; e alguns invariantes pulam `None` por verdade booleana. São problemas existentes nesses leitores, sem alteração direta pelo desvio para Pauta. (`app/ponto/services/dia_do_colab.py:121–123`; `app/ponto/services/passivo_s84.py:94–96`; `app/chamados/management/commands/auditar_invariantes_chamados.py:375–377,1433–1445,1540–1547`)

   Outros leitores independentes são a precedência, que trata o va

[truncado em 20000 de 23024 caracteres]

## 31/10 12:05 · sessao d17f03a5a5fe · - · - · origem=Ronald (tmux)

Auditoria de `disparar_perguntas_competencia` concluída, só por leitura do código; valores atuais e cobertura efetiva em produção [nao verificado].

| Item | Resultado e referências |
|---|---|
| Derivação no comando | Não monta um status completo. Seleciona a coluna `trabalha=True`, lê `VEREDITOS_ACUSAM`, obtém motivos da ata e filtra desligados: `app/chamados/management/commands/disparar_perguntas_competencia.py:76`, `:86`, `:105`, `:111`, `:120`. Fio mudo: chamado vivo sem pergunta viva, motivos da ata, colaborador ativo e fato ainda sem pergunta: `:155`, `:163`, `:167`, `:178`, `:220`. |
| Derivação transitiva de natureza do dia | `grade_da_celula` deriva `fora_vinculo` combinando admissão/demissão com minutos realizados ou `n_celulas-n_missing`; depois lê `ata.tipo_dia` e, se ausente, traduz `trabalha` em `trabalho/folga/indefinido`: `app/escala/services/leitor_celula.py:332`, `:336`, `:339`, `:344`. `_marcos_faltantes` considera apenas `tipo_dia == 'trabalho'`: `app/chamados/services/declaracao_texto.py:90`, `:95`. |
| Fallback de furo | Passado sem marcos: `vencidos_no_instante` retorna `None`; sem ata, `motivos_pela_ata` pede `classificar_falta`: `app/chamados/services/fabrica_da_celula.py:28`, `:30`, `:52`, `:55`. Falha de leitura da ata também habilita esse ramo: `app/chamados/services/disputa_emissao.py:825`, `:832`. |
| Regra de `classificar_falta` | Lê `trabalha=False` e silencia; consulta cobertura da ausência; pede vínculo vigente, turno completo e fim previsto; lê turnos e marcos; compara presença/contagem de E/S para escolher entrada ou saída faltante. Isso é a autoridade de **furo**, não um clone local do status completo: `app/escala/servico_jornada.py:302`, `:340`, `:354`, `:375`, `:391`, `:427`, `:440`, `:447`, `:453`, `:466`, `:469`, `:478`. |
| Fallbacks de `pergunta_viva` | O fio mudo usa `pergunta_viva` e `_ja_perguntou`; este delega `fio_vivo`: `app/chamados/management/commands/disparar_perguntas_competencia.py:149`, `:163`, `:182`; `app/chamados/services/disputa_emissao.py:1202`; `app/chamados/juizes.py:1142`. C6 prioriza validação, tranca, resposta/admin, marco e veredito; sem conclusão pede `classificar_falta`: `app/chamados/juizes.py:1032`, `:1044`, `:1077`, `:1081`, `:1097`. Também consulta essa autoridade para carimbo sem testemunha e destino encerrado sem slot: `:640`, `:726`. |
| Autoridade lavrada disponível | A coluna existe: `app/escala/models.py:1784`. O leitor oficial é `status_do_dia_lavrado`, com relógio antes do encerramento e coluna depois: `app/ponto/status_dia.py:157`, `:175`, `:181`. O escritor aplica `status_da_lavra` na porta: `app/ponto/portas/celula.py:686`, `:693`, `:698`, `:705`. Registro vivo: `app/core/juizes.py:173`. |
| Outras decisões/escritas | Janela via `janela_atual`: comando `:58`. Teto marco + 30min via autoridade: `app/ponto/services/cartorio.py:675`, `:689`. Dedup dos dois universos: comando `:124`, `:181`, `:220`. Desfaz carimbo, abre/agrega disputa e emite perguntas: `app/chamados/services/fabrica_da_celula.py:118`. Agrega motivos/contexto/qtd/ultima_em e abre pai/disputa/trilha: `app/chamados/services/disputa_emissao.py:476`, `:484`, `:504`, `:523`, `:532`. Prazo do pai = agora +20min: `:517`; `app/chamados/catalogo/motor.py:45`, `:48`. Pergunta recém-nascida sem destino pode reatar e reemitir chamado do dia: `app/chamados/services/disputa_emissao.py:404`; `app/chamados/reconciliador.py:734`, `:866`, `:909`. Push silenciado por padrão no comando: `:295`, `:300`. |
| Escritor/evento existente | Batida e ausência acionam julgamento: `app/ponto/signals.py:131`, `:143`; `_julgar_dias`: `:85`, `:93`; julgamento unitário chama `julgar_colab`: `app/ponto/services/cartorio.py:1715`; este pede perguntas para as células **julgadas nesta passada**: `:809`, `:814`, `:823`, `:825`; fábrica executa o mesmo escritor: `app/chamados/services/fabrica_da_celula.py:143`, `:149`. Vigia de hora também chama esse julgamento: `app/ponto/services/vigia_de_hora.py:122`, `:130`. |
| Lacuna para virar vigia | O universo célula tem escritor/evento acima. O universo fio mudo percorre chamados independentemente de novo julgamento: comando `:155`. A reabertura ressuscita perguntas existentes, sem chamada ao emissor novo nesse método: `app/chamados/models.py:1380`, `:1389`. Um evento cobrindo integralmente nascimento de pergunta de fio mudo sem novo julgamento [nao verificado]; o próprio comando declara a lacuna em `:9` e `:131`. |
| Estado atual da troca | `--alarme` já existe, conta alvos e sai 2 se houver: comando `:261`. A configuração ainda usa `--comp-corrente --apply`: `app/config/crons.py:1464`. Portanto, só trocar a configuração ainda depende de cobrir o fio mudo e separar a pergunta “qual status?” da pergunta “qual marco falta?”: comando `:155`; `app/chamados/juizes.py:1073`; `app/ponto/status_dia.py:181`. |

Detalhes importantes para a consolidação:

- `CelulaDia.status_dia` não fornece os motivos ou horários dos marcos; estes vêm das lâmpadas da ata e do DNA: `app/chamados/services/declaracao_texto.py:97`, `:109`; `app/chamados/juizes.py:844`, `:870`, `:887`.
- Ler status lavrado não elimina decisões de identidade do fato, pergunta viva, prazo e costura: comando `:124`, `:163`, `:220`; `app/chamados/services/disputa_emissao.py:389`, `:404`, `:517`.
- O caminho de evento já desfaz carimbo sem lastro no julgamento, inclusive de motivos cuja pergunta já existe, limitado a disputa aberta: `app/chamados/services/fabrica_da_celula.py:143`, `:146`; `app/chamados/services/relavra_via.py:223`, `:227`, `:231`. O comando usa `perguntar`, cujo desfazimento mantém o default de reabrir disputa fechada: `app/chamados/services/fabrica_da_celula.py:119`; `app/chamados/services/relavra_via.py:185`, `:248`. Essa diferença impede declarar equivalência total dos dois escritores sem análise adicional dessas populações [nao verificado].

## 31/10 12:05 · sessao c59631f33060 · - · - · origem=Ronald (tmux)

| Pergunta — `reavaliar_ausencias_feriado` | Resultado e evidência |
|---|---|
| (1) Deriva status/“tem trabalho” por conta própria? | **O comando não tem regra própria de dia:** chama `universo`, `espurios` e, com `--apply`, `encerrar` (`app/ponto/management/commands/reavaliar_ausencias_feriado.py:23`, `:24`, `:28`, `:32`). **O serviço lê uma autoridade, mas ainda não a coluna lavrada:** `espurios` obtém a data por `dia_do_fato`, pergunta `fatos_do_dia`, ignora `era_dia_previsto is None`, seleciona `faixa in ('feriado','folga')` e entrega `falta_descoberta=False` ao catálogo (`app/chamados/services/feriado_reavaliar.py:30`, `:33`, `:34`, `:36`, `:38`). Essa última tradução é uma decisão sobre a premissa da cobrança, apoiada no resultado do juiz, não uma nova aritmética de escala (`app/chamados/services/feriado_reavaliar.py:37`; `app/chamados/catalogo/premissa.py:58`, `:144`). |
| Regra efetivamente chamada | `fatos_do_dia` delega ao período de um dia (`app/ponto/precedencia.py:195`, `:209`). O período lê `CelulaDia.trabalha` quando disponível; sem célula, pede `eh_dia_trabalho_calculado` ao vínculo e conserva desconhecimento como `None` (`app/ponto/precedencia.py:316`, `:322`, `:325`, `:327`). Feriado não suprime trabalho quando há previsão e `trabalha_em_feriado=True`; os fatos presentes são ordenados por férias > afastamento > atestado > feriado > folga > trabalho (`app/ponto/precedencia.py:46`, `:330`, `:335`, `:349`). |
| (2) Autoridade/coluna existente | `CelulaDia.status_dia` e `status_rotulo` existem, são anuláveis e recebem a lavra junto do veredito (`app/escala/models.py:1775`, `:1784`, `:1785`; `app/ponto/portas/celula.py:709`). `status_do_dia_lavrado` lê essas colunas; antes do encerramento responde pelo relógio e explicita ausência de célula/lavra (`app/ponto/status_dia.py:157`, `:171`, `:176`, `:181`, `:184`). Para a pergunta específica “dia sem trabalho”, já há `dia_sem_trabalho(cel)`: `trabalha is False` ou `ata.tipo_dia` presente e diferente de trabalho; `None` não significa folga (`app/chamados/juizes.py:398`, `:403`, `:408`). O lookup identifica precedência e status lavrado separadamente (`app/docs/JUIZES.md:68`, `:74`). |
| Limite da equivalência | **`status_dia` não é uma codificação bijetiva da `faixa` atualmente consultada:** a regra pode devolver `folga`, `folga_trabalhada`, `ausencia` etc.; `folga_trabalhada` decorre de batida em tipo folga, enquanto a seleção atual exige especificamente faixa feriado/folga e previsão conhecida (`app/ponto/status_dia.py:96`, `:101`, `:106`, `:115`; `app/chamados/services/feriado_reavaliar.py:34`, `:36`). Portanto migrar exige definir o predicado do consumidor; “qualquer status diferente de trabalho” não reproduz a regra demonstrada (`app/ponto/status_dia.py:28`; `app/chamados/services/feriado_reavaliar.py:34`). |
| (3) Outras decisões e escritas | Universo: somente `batida_ausente` nos estados `VIVOS`, opcionalmente por empresa (`app/chamados/services/feriado_reavaliar.py:14`, `:17`, `:18`). Não há prazo adicional no corpo auditado; o comando é dry-run por padrão e só grava com `--apply` (`app/ponto/management/commands/reavaliar_ausencias_feriado.py:10`, `:28`). Ao fechar, chama `ch.reconciliar`; apenas se a porta aceitar anexa ocorrência `falta_espuria_feriado` e auditoria `reconciliar_feriado` (`app/chamados/services/feriado_reavaliar.py:43`, `:47`, `:49`, `:52`). A porta de reconciliação consulta o motor, a premissa e a guarda de pergunta viva antes de gravar o estado (`app/chamados/models.py:605`, `:615`, `:618`, `:635`, `:646`, `:649`). |
| Efeitos do fechamento | O estado terminal rebaixa urgência e resolve perguntas pelo mesmo conjunto de receivers usado por outras vias (`app/chamados/signals.py:54`, `:70`, `:72`, `:76`, `:81`, `:92`). Mudança de `status_local` também dispara julgamento da célula do dia, exceto durante julgamento já em curso (`app/chamados/signals.py:165`, `:169`, `:174`, `:180`). |
| (4) Quem escreveria no lugar do cron? Eventos existentes | **O mesmo `feriado_reavaliar.encerrar` já é escritor pelo evento:** `core.Feriado.post_save` agenda `reavaliar_dia(data)` em `on_commit`; esse caminho percorre `espurios` e `encerrar` (`app/chamados/signals.py:265`, `:274`, `:276`; `app/chamados/services/feriado_reavaliar.py:59`, `:76`, `:82`, `:83`). A regeneração de vínculo também agenda `reavaliar_dias(_tocadas, colaborador_id=...)`, somente se `recalculo` e houver dias tocados (`app/ponto/portas/celula.py:421`, `:431`, `:432`, `:433`). Assim, as mesmas escritas de estado, ocorrência e auditoria já têm essas duas entradas de evento (`app/chamados/services/feriado_reavaliar.py:43`). |
| Cobertura incompleta para retirar escrita do cron | O evento de regeneração exige `_tocadas` e `recalculo=True`; a frota chama a porta com `recalculo=False` (`app/ponto/portas/celula.py:421`; `app/escala/services/cadastro_tipo.py:348`, `:350`). A reversão grava DNA/trabalha/vínculo e auditoria, sem chamar a reavaliação neste corpo (`app/ponto/portas/celula.py:529`, `:538`, `:541`, `:552`). O comentário documenta ainda dia que passa a ser feriado/folga sem célula mudar, reversão e acervo como alcance remanescente do cron (`app/chamados/signals.py:259`). A suficiência em produção e o contador esperado zero são **[nao verificado]**. |

Dependências para a ordem de troca:

- **Não tratar a troca em `espurios` como mudança isolada do cron:** o coletor `premissa_regularizada`, usado pelos gates de fechamento e reabertura, chama `espurios([self])` quando célula/ata não responde (`app/chamados/models.py:1200`, `:1208`, `:1250`, `:1252`). Retirar o fallback para fatos sem decidir o comportamento de ausência de lavra muda esses gates (`app/chamados/models.py:1253`; `app/ponto/status_dia.py:173`, `:184`).
- **Garantir lavra anterior à leitura no evento de Feriado:** o receiver atual somente agenda reavaliação; o julgamento provocado pelo fechamento ocorre depois da alteração do chamado (`app/chamados/signals.py:271`, `:275`, `:165`, `:180`). **Inferência de código:** uma futura leitura de coluna nesse receiver pode encontrar a lavra anterior, pois não há ali passo de atualização prévia (`app/chamados/signals.py:266`). Já a regeneração com recálculo julga os dias tocados passados antes de agendar a reavaliação (`app/ponto/portas/celula.py:360`, `:368`, `:372`, `:421`).
- **Cobrir os caminhos restantes antes do vigia:** regeneração de frota, reversão, alteração sem `_tocadas` e acervo têm as lacunas acima; zero efetivo não é fornecido no dado do usuário **[nao verificado]**. A própria declaração mantém os crons até haver `a_fechar=0` medido (`app/ponto/portas/celula.py:419`).
- **Um futuro vigia deve contar fechamentos realmente autorizados:** existe `reconciliaria()`, sem escrita nem trilha, que consulta a mesma decisão de `reconciliar()`; contar apenas `len(espurios)` não considera os gates finais (`app/chamados/models.py:621`, `:625`, `:646`, `:649`; `app/ponto/management/commands/reavaliar_ausencias_feriado.py:25`).

## 31/10 12:04 · sessao f9d6ec28df81 · - · - · origem=Ronald (tmux)

Auditoria de `fechar_cobranca_com_lastro` — somente código, sem banco/log; execução em produção [nao verificado].

| Pergunta | Resultado com referências |
|---|---|
| Entrada e escrita | O cron está configurado às `:40` com `--apply` (`app/config/crons.py:710`). O command delega para `lastro.fechar(aplicar, tolerancia)` (`app/ponto/management/commands/fechar_cobranca_com_lastro.py:52`). `fechar` mede uma vez, recarrega o status do chamado, exige que continue vivo e chama `reconciliar()`; somente após sucesso grava nota `lastro_encontrado` (`app/ponto/services/lastro.py:300`, `app/ponto/services/lastro.py:310`, `app/ponto/services/lastro.py:317`). |
| (1) Deriva status geral do dia? | Não há atribuição de `status_dia` nesse caminho; a pergunta principal é se **todos os marcos nomeados pela cobrança têm lastro**, com a autoridade da lâmpada primeiro (`app/ponto/services/lastro.py:250`, `app/ponto/services/lastro.py:264`). Para lâmpada existente, `acesa` fornece lastro; apagada classifica `furo_real` (`app/ponto/services/lastro.py:265`). |
| (1) Recuo que deriva fato em paralelo | **Sem a lâmpada daquele marco**, o serviço refaz uma avaliação: nenhum turno pareado no dia → `sem_batida`; havendo turnos, exige batida do mesmo tipo a até `tolerancia_min`, padrão 90 minutos; sem candidata → `outro_horario`; com candidata escolhe a mais próxima e concede lastro. Todos os marcos precisam passar (`app/ponto/services/lastro.py:59`, `app/ponto/services/lastro.py:245`, `app/ponto/services/lastro.py:274`, `app/ponto/services/lastro.py:282`, `app/ponto/services/lastro.py:289`). Isso não é status geral, mas é uma segunda derivação de lastro sem certidão. |
| (2) Autoridade existente para o dia | Existem `CelulaDia.status_dia/status_rotulo`, nulos significando ainda não lavrado (`app/escala/models.py:1775`, `app/escala/models.py:1784`). O leitor `status_do_dia_lavrado` consulta a coluna após o encerramento e responde `aberto/previsto` pelo relógio antes dele (`app/ponto/status_dia.py:157`, `app/ponto/status_dia.py:175`, `app/ponto/status_dia.py:181`). A porta lavra status junto ao veredito (`app/ponto/portas/celula.py:686`, `app/ponto/portas/celula.py:701`, `app/ponto/portas/celula.py:709`). |
| (2) Autoridade para **esta cobrança**, distinta do status geral | `lastro.marcos_acesos(ch, cel)` já responde `True/False/None` para todos os marcos nomeados, exigindo hora e tipo e lendo a ata; falta de marco/tipo/lâmpada produz `None` (`app/ponto/services/lastro.py:153`, `app/ponto/services/lastro.py:170`). O próprio `julgar` lê as mesmas lâmpadas (`app/ponto/services/lastro.py:82`, `app/ponto/services/lastro.py:265`). **Substituir essa pergunta apenas por `status_dia` perde a identidade dos marcos cobrados:** a cobrança pode ter seu próprio marco resolvido enquanto outro furo do dia permanece (`app/ponto/services/lastro.py:160`). |
| (2) Autoridades transitivas do gate | `reconciliar()` consulta motor e guarda da premissa, além da guarda de pergunta viva (`app/chamados/models.py:627`, `app/chamados/models.py:637`, `app/chamados/models.py:646`, `app/chamados/models.py:649`). A premissa lê célula atual, decisão admin, `dia_sem_trabalho`, marcos acesos/inexistentes e `_celula_furada` (`app/chamados/models.py:1227`, `app/chamados/models.py:1230`, `app/chamados/models.py:1234`, `app/chamados/models.py:1247`). `dia_sem_trabalho` lê `trabalha is False` ou `ata.tipo_dia != trabalho`, tratando `None` como indeterminado (`app/chamados/juizes.py:398`, `app/chamados/juizes.py:407`). |
| (3) O que mais decide | Seleciona fila `batida_ausente`, obtém dia pelo juiz `dia_do_fato`, pareia turnos, recusa cobrança sem marcos ou sem tipo, determina se todos os marcos foram satisfeitos e tenta fechamento (`app/ponto/services/lastro.py:209`, `app/ponto/services/lastro.py:216`, `app/ponto/services/lastro.py:245`, `app/ponto/services/lastro.py:250`, `app/ponto/services/lastro.py:291`, `app/ponto/services/lastro.py:317`). Não há prazo de idade nessa decisão: busca desde a menor data da fila, com margem de um dia para batidas (`app/ponto/services/lastro.py:232`). |
| (3) Efeitos adicionais | O fechamento por `reconciliar` é transação e passa pelo escritor `_gravar_estado` (`app/chamados/models.py:612`, `app/chamados/models.py:618`). Esse escritor altera `status_local`, carimba resolução/via, rebaixa urgência e registra `estado_chamado` (`app/chamados/models.py:222`, `app/chamados/models.py:227`, `app/chamados/models.py:274`). O cron acrescenta nota do fio por `nota.anexar_ocorrencia`, porta que cria `FollowupChamado` (`app/ponto/services/lastro.py:319`, `app/chamados/services/nota.py:75`, `app/chamados/services/nota.py:102`). |
| (4) Quem já fecha no evento | `Batida.post_save` chama `_julgar_dias`, que chama `cartorio.julgar_celula` para data da batida e véspera (`app/ponto/signals.py:85`, `app/ponto/signals.py:108`, `app/ponto/signals.py:131`). O cartório calcula a ata atual e lê `marcos_acesos`; cobranças sem protesto são reconciliadas (`app/ponto/services/cartorio.py:1182`, `app/ponto/services/cartorio.py:1189`, `app/ponto/services/cartorio.py:1204`, `app/ponto/services/cartorio.py:1275`, `app/ponto/services/cartorio.py:1281`). Com fio vivo, a ordem existente é **lavrar → E2 carimba perguntas absolvidas → reconciliar → segunda lavra** (`app/ponto/services/cartorio.py:776`, `app/ponto/services/cartorio.py:786`, `app/ponto/services/cartorio.py:791`, `app/ponto/services/cartorio.py:797`). |
| (4) Existe evento persistente? | Sim: `salvar_e_emitir` salva célula e insere `EventoCelula` no mesmo atomic quando ata/veredito entram nos campos lavrados (`app/ponto/portas/celula.py:622`, `app/ponto/portas/celula.py:626`, `app/ponto/portas/celula.py:634`). O consumidor chama a retratação de chamados absolvidos por célula (`app/chamados/reconciliador.py:1261`, `app/chamados/reconciliador.py:1304`). Equivalência de cobertura em produção [nao verificado]. |
| (4) O que ainda precisa de destino explícito | A nota específica `lastro_encontrado` nasce no cron (`app/ponto/services/lastro.py:319`); o fechamento por marco do cartório não cria essa nota, apenas reconcilia, conta, registra saída e agenda nova lavra (`app/ponto/services/cartorio.py:1281`). Para conservar o efeito, a nota deve acompanhar o evento que efetivamente fecha; implementação correspondente atual [nao verificado]. O recuo sem lâmpada não tem substituição demonstrada apenas por leitura de status (`app/ponto/services/lastro.py:274`). |
| Vigia já disponível e ressalva | `contador()` já se declara “esperado 0”, mas conta `len(com_lastro)`, **sem perguntar se o gate fecharia** (`app/ponto/services/lastro.py:334`, `app/ponto/services/lastro.py:340`). `fechar` só contabiliza fechamento se `reconciliar()` permitir (`app/ponto/services/lastro.py:317`). Há teste mostrando `com_lastro=1` e `fechados=0` (`app/ponto/tests/test_bug114_fechador_de_dois_dias.py:195`). Para vigia de obrigação de fechamento, há leitor sem escrita `reconciliaria()` (`app/chamados/models.py:621`). |

Dependência para a ordem de troca: o caminho de lastro ainda pode cair em `premissa_regularizada → _celula_furada → feriado_reavaliar.espurios` quando a célula não responde (`app/chamados/models.py:1247`, `app/chamados/models.py:1252`). Portanto, a cura da autoridade de feriado/folga e dos casos sem certidão deve preceder a remoção desse recuo; a ordem global entre os quatro crons depende das demais auditorias [nao verificado].

## 31/10 12:04 · sessao 691e7e577dd7 · - · - · origem=Ronald (tmux)

Leitura estática somente; não executei banco nem testes [nao verificado]. [nao verificado]

**7 — Por que chamado por dia**

- O escritor comum `fabrica_da_celula.perguntar` cria/atualiza a disputa e gera perguntas **por motivo e data**; chama `criar_ou_atualizar_disputa` com `{'data': dia.isoformat()}` e depois `gerar_perguntas_disputa` (`app/chamados/services/fabrica_da_celula.py:120-126`).
- A disputa agrupa pelo **colaborador com `fechada_em IS NULL`**, agregando motivos e atualizando o contexto; não agrupa por competência (`app/chamados/services/disputa_emissao.py:467-502`).
- A pergunta nova sem destino, para motivo do grupo DIA, chama `reatar_pergunta_orfa` imediatamente (`app/chamados/services/disputa_emissao.py:398-410`).
- `reatar_pergunta_orfa` tenta derivação, histórico, lavra `(c1)` e depois `reemitir_chamado_do_dia` `(c2)` (`app/chamados/reconciliador.py:1460-1526`).
- **A função que abre chamado a partir da pergunta é `reemitir_chamado_do_dia`**. A chave é colaborador + `modulo_origem='batida_ausente'` + `contexto_json__data_turno=str(data_ctx)`; não contém disputa, motivo ou competência (`app/chamados/reconciliador.py:807-811`, `app/chamados/reconciliador.py:866-877`). Vários motivos/perguntas daquele dia convergem nesse chamado; outro dia muda a chave (`app/chamados/reconciliador.py:808`).
- O outro emissor canônico, `detectar_ausencias.Command._abrir_chamado`, usa a mesma chave de colaborador, módulo e data_turno (`app/ponto/management/commands/detectar_ausencias.py:688-707`). `ChamadoColaborador.abrir` serializa por colaborador e consulta o existente usando o lookup recebido (`app/chamados/models.py:861-865`).
- Isso explica o mecanismo de um chamado por dia; não prova quais funções produziram os 23 chamados/~320 perguntas da medida anterior [nao verificado].

**2 — Bypasses da fábrica**

1. **Lote de perguntas:** `disparar_perguntas_competencia` seleciona células `trabalha=True` no primeiro caminho (`app/chamados/management/commands/disparar_perguntas_competencia.py:76`); no caminho de **fio mudo**, seleciona chamado vivo com célula na janela, sem filtro de trabalha, e deriva o motivo da ata (`app/chamados/management/commands/disparar_perguntas_competencia.py:155-168`). O escritor chama `fabrica_da_celula.perguntar` **diretamente**, sem `perguntar_pela_celula` nem gate de competência entregue (`app/chamados/management/commands/disparar_perguntas_competencia.py:276-289`). Portanto o fio mudo pode produzir pergunta em limbo exportado se já houver chamado vivo, motivo elegível e nenhuma pergunta com a identidade do fato; existência anterior é barrada em `:220-224` (`app/chamados/management/commands/disparar_perguntas_competencia.py:155-168`, `app/chamados/management/commands/disparar_perguntas_competencia.py:220-224`, `app/chamados/management/commands/disparar_perguntas_competencia.py:288`).

2. **Cartório → detector → pergunta:** o cartório emite diretamente pelo detector, tanto no furo sem cobrança/morta/limbo de hoje quanto no parcial (`app/ponto/services/cartorio.py:1450-1472`, `app/ponto/services/cartorio.py:1492-1507`). O silêncio considera chamado vivo e piso, sem competência entregue (`app/ponto/services/cartorio.py:1430-1438`). O detector abre chamado (`app/ponto/management/commands/detectar_ausencias.py:688-707`) e chama `criar_disputa_a_partir_de_chamados_orfaos` para criar o fio (`app/ponto/management/commands/detectar_ausencias.py:782-796`). Este agrupador cria/complementa perguntas pela ata (`app/chamados/services/disputa_emissao.py:966-981`), passando por `_criar_pergunta_se_couber`, cujos gates são ausência e `_ja_perguntou`, sem competência entregue (`app/chamados/services/disputa_emissao.py:756-779`). O próprio `_ja_perguntou` documenta retirada da guarda exportada e efetivamente só consulta existência/colaborador/fio vivo (`app/chamados/services/disputa_emissao.py:1192-1209`).

   **Cuidado:** não concluir que qualquer `trabalha=None` consegue esse caminho; o cartório exige `ec.eh_dia_trabalho(data) is True` no furo e no auxiliar do parcial (`app/ponto/services/cartorio.py:1459`, `app/ponto/services/cartorio.py:1557-1559`). O supra-juiz costuma classificar `sem_ancora` como `PREVISTO_COM_DIA_INDEFINIDO` (`app/ponto/supra_juiz.py:325-328`) e exclui `sem_ancora` dos ramos `FURO_PARCIAL` (`app/ponto/supra_juiz.py:379-387`, `app/ponto/supra_juiz.py:396-403`).

3. **Pergunta existente → reemissão `(c2)`:** apenas os motivos fora de `MOTIVOS_C1` — os intervalos — recebem o gate `_viva_em_competencia_aberta`; os motivos C1 seguem o caminho anterior, alcançando exportada (`app/chamados/reconciliador.py:781-783`, `app/chamados/reconciliador.py:815-822`, `app/chamados/reconciliador.py:864-877`). Continua condicionado a ativo, piso, isenção, ausência, protesto cobrável, veto da célula/régua e motor (`app/chamados/reconciliador.py:793-865`).

**6 — Leitores e efeitos**

- **Juiz compartilhado:** `dia_sem_trabalho` aceita `None` como indeterminado; só cala por `trabalha is False` ou `ata.tipo_dia` presente diferente de trabalho (`app/chamados/juizes.py:398-408`). `_celula_furada` o lê, e marco apagado ainda acusa `True` (`app/chamados/juizes.py:443-458`). O delta não muda esse juiz (`app/chamados/services/fabrica_da_celula.py:153-160`).
- **Gate de emissão:** `celula_veta_cobranca` lê trabalha/veredito; `None` não veta, `False` veta, e há lista de vereditos que vetam (`app/ponto/marcos.py:119`, `app/ponto/marcos.py:151-163`). Assim desviar a pergunta para Pauta não cria veto de chamado nesse gate (`app/ponto/marcos.py:155-163`, `app/chamados/services/fabrica_da_celula.py:153-157`).
- **Cartório:** `None` passa no teste `_protesta` se os marcos não estão todos acesos (`app/ponto/services/cartorio.py:1166-1167`); também entra na espera do fim de turno para julgamento de hoje (`app/ponto/services/cartorio.py:1664-1669`). O cartório mede fio com `fio_vivo` sobre perguntas e entrega `cobrancas_vivas=len(_com_fio)` ao supra-juiz (`app/ponto/services/cartorio.py:1100-1105`, `app/ponto/services/cartorio.py:1169-1172`).
- **Protestos/veredito:** a falta de cobrança viva alimenta `FURO_SEM_COBRANCA`/`FURO_COM_COBRANCA_MORTA`, mas `sem_ancora` ganha antes o protesto de CADASTRO `PREVISTO_COM_DIA_INDEFINIDO` (`app/ponto/supra_juiz.py:281-339`). Portanto não se pode afirmar que todo limbo desviado virará `FURO_SEM_COBRANCA`; pode continuar em CADASTRO (`app/ponto/supra_juiz.py:325-328`). Na ausência de protestos, o veredito especial `cobrado` depende de `_com_fio`, não de Pauta (`app/ponto/services/cartorio.py:1183-1207`).
- **Contadores cartório:** a fábrica devolvendo `[]` impede incremento de `perguntas_no_julgamento` (`app/ponto/services/cartorio.py:800-803`, `app/chamados/services/fabrica_da_celula.py:155-157`). Os contadores de emissão/protestos são separados; releitura pós-emissão mede o delta de protestos e usa `_conv` (`app/ponto/services/cartorio.py:839-847`). Pauta não incrementa emissão por esse retorno (`app/ponto/services/cartorio.py:800-803`).
- **Vigia de fio:** aceita células acusadoras independentemente de trabalha, partindo de chamado vivo (`app/chamados/services/vigia_fio.py:317-362`). Sem pergunta viva, tenta os donos decisão do calendário, ausência aberta, folga validada e ListaDoAdmin; **Pauta não entra nessa cadeia** (`app/chamados/services/vigia_fio.py:289-298`). Se nenhum desses donos responder, incrementa `fio_morto`, `sem_pergunta_nenhuma` e eventualmente `fio_morto_cobrando` (`app/chamados/services/vigia_fio.py:392-421`). Logo Pauta nova pode deixar um dia contado como fio morto se houver chamado vivo/célula acusando e ele não estiver coberto pelos donos existentes; ocorrência real [nao verificado].
- **Vigia de alcance:** universo vem de `competencias_a_lavrar`/`fora_da_folha`; a porta é `porta_alcancavel_do_dia`, depois os mesmos donos (`app/chamados/services/vigia_fio.py:523-552`, `app/chamados/services/vigia_fio.py:565-567`). Não há integração explícita da Pauta nova nessas fontes (`app/chamados/services/vigia_fio.py:523-552`, `app/chamados/services/vigia_fio.py:289-298`).
- **Passivo `furos_sem_cobranca`:** filtra janela atual até ontem, `veredito='furo'` e protestos cobráveis, sem exigir trabalha=True (`app/ponto/services/furos_sem_cobranca.py:55-63`). Pula chamado vivo e competência trancada, distribui entre afastado/veto/veto_célula/superação/decisão/pergunta ou supervisão; **não tem porta Pauta/exportada** (`app/ponto/services/furos_sem_cobranca.py:82-108`). A soma exclui apenas as portas declaradas (`app/ponto/services/furos_sem_cobranca.py:30-31`, `app/ponto/services/furos_sem_cobranca.py:109-114`). Dado o recorte de janela atual, não afirmar que conta os dias antigos exportados da medida [nao verificado].
- **`furos_vetados`:** lê qualquer célula com `veredito='furo'`, sem trabalha=True, e pergunta ao veto de régua; Pauta não participa (`app/ponto/services/furos_vetados.py:61-70`).
- **Vigias que excluem None:** vigia de hora (`app/ponto/services/vigia_de_hora.py:21-23`), vigia de entrada e seu contador (`app/ponto/services/vigia_entrada.py:78-85`, `app/ponto/services/vigia_entrada.py:120`), previstos de `fila_por_causa` (`app/chamados/services/fila_por_causa.py:192-194`) usam trabalha=True; desviar pergunta de célula None não muda a inclusão nesses universos (`app/ponto/services/vigia_de_hora.py:21`, `app/ponto/services/vigia_entrada.py:79`, `app/chamados/services/fila_por_causa.py:192`).
- **Reconciliador da grade:** indefinido só entra em mortos se `dia_sem_trabalho` disser que é sem trabalho; não mata pelo None (`app/ponto/management/commands/reconciliar_grade.py:280-283`). Há outro gate de lei para retratação (`app/ponto/management/commands/reconciliar_grade.py:284-291`).
- **Tripwire de ata folga:** inclui explicitamente False **ou NULL**, e só acusa ata folga com agregação incompatível; não depende de pergunta/Pauta (`app/ponto/management/commands/tripwire_celulas.py:337-350`).
- **Auditor de invariantes:** há leitores ainda binários: `not c.trabalha` pula None (`app/chamados/management/commands/auditar_invariantes_chamados.py:375-377`); outro caso envia `not _cel4.trabalha` ao contador de célula que não acusa (`app/chamados/management/commands/auditar_invariantes_chamados.py:1433-1445`); o contador de contradição com FolgaDia exige `trabalha is True` (`app/chamados/management/commands/auditar_invariantes_chamados.py:1540-1547`). Não mudam apenas porque nasce Pauta em lugar da pergunta, mas conservam cegueira/semântica binária para None nesses ramos (`app/chamados/management/commands/auditar_invariantes_chamados.py:376`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1443`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1542`).
- **Outros leitores com semântica problemática anterior:** `dia_do_colab` descreve `not cel.trabalha` como “Dia sem trabalho previsto: nenhuma marca era esperada” (`app/ponto/services/dia_do_colab.py:121-123`); `passivo_s84` classifica `None` como `FOLGA` (`app/ponto/services/passivo_s84.py:94-96`). São defeitos preexistentes, não introduzidos pelo delta (`app/ponto/services/dia_do_colab.py:121`, `app/ponto/services/passivo_s84.py:96`).
- **Flip/esmeril:** flip só trata False como folga mas exige True na véspera noturna (`app/ponto/services/flip_auto.py:208-213`); esmeril A8 só False e A2/A? de marcos usa truthiness de trabalha, excluindo None (`app/ponto/services/esmeril_espelho.py:71`, `app/ponto/services/esmeril_espelho.py:121-123`). São leitores de célula independentes da nova Pauta (`app/ponto/services/flip_auto.py:208-213`, `app/ponto/services/esmeril_espelho.py:71`).

**Outro defeito relevante**

O desvio é tardio para garantir “sem pergunta no app” no fluxo completo: o detector já pode abrir o chamado e criar as perguntas antes de a fábrica ser chamada, e o lote de fio mudo usa o escritor de baixo diretamente (`app/ponto/services/cartorio.py:1469-1472`, `app/ponto/management/commands/detectar_ausencias.py:782-796`, `app/ponto/services/cartorio.py:800`, `app/chamados/management/commands/disparar_perguntas_competencia.py:288`). A revisão precisa distinguir as condições em que esse caminho é possível para `None` das condições comprovadas para True (`app/ponto/services/cartorio.py:1459`, `app/ponto/services/cartorio.py:1558`, `app/chamados/management/commands/disparar_perguntas_competencia.py:155-168`).

## 31/10 12:03 · sessao 7fde4e301c26 · V3-RETOMA-FILA1 · verificador · origem=Code

**A cura elimina a soma paralela atual do lado v2, mas o teste novo não protege completamente essa propriedade.** A captura reproduz os valores numéricos que a lavratura receberia para persistir, sob os mesmos insumos; isso não prova igualdade com o histórico de produção nem correção pela lei. (`diff_calculador.py:71–83`, DEPOIS; `dia_pago.py:343–358`; `app/docs/LEIS.md:136`.)

Referências abreviadas: `diff_calculador.py` é `app/ponto/management/commands/diff_calculador.py`; `dia_pago.py`, `fechamento.py` e `credito_parcial.py` estão em `app/ponto/services/`; `models.py` é `app/ponto/models.py`. **DEPOIS** indica numeração calculada pelo anexo; as demais referências apontam à cópia ANTES. Execução integral da suíte e números de produção: **[não verificado]**.

**1. Quem escreve e quem lê**

O fechamento monta a chamada motor; `_captar` recebe os argumentos, separa `versao` e encaminha os demais, preservando os objetos, à função original salva em `_ol`. Não chama recursivamente o atributo substituído. (`fechamento.py:1168–1205`; `diff_calculador.py:188–207`, DEPOIS.)

| Argumento recebido | Quem escreve/origina | Quem lê e qual valor produz |
|---|---|---|
| `colab`, `mes`, `ano` | Parâmetros e universo do fechamento: `fechamento.py:432`, `:598` | Identificação das linhas e filtro de substituição: `dia_pago.py:343`, `:355`, `:357`; `_captar` usa `colab.pk` como chave: `diff_calculador.py:199–203`, DEPOIS. |
| `periodos` | Motor por fatia e composição: `fechamento.py:270–281`, `:729–731` | Escreve trabalhadas, noturnas, HE50, HE50 noturna, HE100, HE100 feriado, HE100 noturna, antecipada e intra: `dia_pago.py:173–186`. |
| `ft_certa` | Classificação por escala certa, retirando os feriados: `fechamento.py:909–910`, `:926–928` | Folga trabalhada, noturnas e intra: `dia_pago.py:223–240`. |
| `ft_feriado` | Partição pelo vínculo e `feriado_vira_folga`: `fechamento.py:920–928` | HE100, HE100 feriado, noturnas e intra: `dia_pago.py:244–256`. |
| `ft_sem_escala` | Complemento por identidade das outras partições: `fechamento.py:948–949` | Trabalhadas, noturnas e intra: `dia_pago.py:257–265`. |
| `orfaos_por_dia` | Apuração dos dias descobertos e soma por calendário: `fechamento.py:766–771`, `:813–817` | Acrescenta trabalhadas: `dia_pago.py:267–270`. |
| `faltas_por_dia` | Autoridade `minutos_de_falta_por_dia`: `fechamento.py:848–850` | Escreve falta em horas: `dia_pago.py:272–275`. |
| `atraso_por_dia` | `fechamento.py:966`; agregador em `credito_parcial.py:36–44` | Base do atraso líquido: `dia_pago.py:190–195`. |
| `credito_por_dia` | `fechamento.py:967`; agregação das ausências parciais pagas em `credito_parcial.py:15–26` | Abate até o atraso daquele dia: `dia_pago.py:191–195`. |
| `reflexo_dsr`, `banco` | Campos mensais: `fechamento.py:1071–1072` | Linha de ajuste, posteriormente descartada pela captura: `dia_pago.py:348–356`; `diff_calculador.py:80–82`, DEPOIS. |
| `abono_por_dia`, `previsto_por_dia`, `realizado_por_dia` | Grade e conversor canônico: `fechamento.py:981–982`, `:1028–1029`; `dia_pago.py:384–400` | Escreve os três `minutos_*`, sem converter para horas: `dia_pago.py:286–290`. |
| `dia_da_jornada` | Mapa composto por mescla: `fechamento.py:295–300`, entregue em `:1185` | `_dia_de` escolhe a data nos quatro laços de períodos: `dia_pago.py:126–132`, `:174`, `:235`, `:245`, `:258`. |
| `furos_do_motor` | Resultado do motor com teto de encerramento: `fechamento.py:1083–1085`, entregue em `:1194` | Cria inclusive dias sem horas e escreve `furo_apurado`: `dia_pago.py:279–281`, `:344`. O booleano não integra a projeção: `dia_pago.py:34–42`. |
| `versao='motor'` | Literal do fechamento: `fechamento.py:1195` | `_captar` aceita apenas motor; lavratura identifica e substitui essa versão: `diff_calculador.py:196–198`, DEPOIS; `dia_pago.py:333`, `:357`. |
| `versao_motor`, `regua_por_dia` | Impressão por fatia e mapa por dia: `fechamento.py:265–269`, `:739–740`, `:1198` | Metadados dos dias e diagnóstico no ajuste: `dia_pago.py:333–345`, `:353–354`; não entram no retorno capturado: `diff_calculador.py:82`, DEPOIS. |
| `evento_origem` | Neste modo recebe **`backfill`**: `fechamento.py:1205` | Metadado das linhas: `dia_pago.py:333–356`; descartado na projeção: `diff_calculador.py:82`, DEPOIS. |

`versao_dna`, `rubricas` e `do_motor` não são enviados na chamada motor e ficam nos defaults. A chamada oráculo envia `rubricas`, mas `_captar` a transforma em no-op. (`dia_pago.py:139–141`; `fechamento.py:1169–1205`, `:1245–1264`; `diff_calculador.py:197–198`, DEPOIS.)

| Estado intermediário | Quem escreve | Quem lê |
|---|---|---|
| Atributo global `ponto.models.DiaPago` | Classe original definida em `models.py:281`; helper salva, substitui e restaura em `finally`: `diff_calculador.py:46`, `:71–75`, DEPOIS | Importação **dentro** da função real: `dia_pago.py:163`; constantes, construção e operações ORM em `:343–358`. |
| Constantes `TIPO_DIA`/`TIPO_AJUSTE` | Modelo: `models.py:311–312`; copiadas ao substituto: `diff_calculador.py:64–65`, DEPOIS | Lavratura identifica as linhas; extração conserva somente dia com data: `dia_pago.py:343`, `:355`; `diff_calculador.py:80`, DEPOIS. |
| `self.campos`, `_linhas` | Construtor guarda kwargs; `bulk_create` acrescenta objetos: `diff_calculador.py:59–69`, DEPOIS | Extração em `diff_calculador.py:78–82`, DEPOIS. |
| Dicionário devolvido por `lavra_sem_gravar` | `{data: {campo: float}}`, limitado a `CAMPOS_DIA`: `diff_calculador.py:76–83`, DEPOIS; lista canônica em `dia_pago.py:34–42` | `_captar` guarda em `_lav_fech[colab.pk]`; o comando recupera `_lv`: `diff_calculador.py:203`, `:246–249`, DEPOIS. |
| `m_dia` | Copia os dias de `_lv` dentro da janela: `diff_calculador.py:329–334`, DEPOIS | Comparação, totais, divergências e CSV: `diff_calculador.py:527–548`, ANTES. |
| Atributo global `dia_pago.lavrar` | Salvo, substituído por `_captar` e restaurado: `diff_calculador.py:188`, `:220–225`, DEPOIS | Importação local do fechamento e reutilização do alias para oráculo: `fechamento.py:1168`, `:1245`. |
| `_res_fech` | Interceptação preexistente de `ResultadoMes.__init__`, guardando a primeira instância por colaborador: `diff_calculador.py:131–135`, ANTES | Pré-requisito para prosseguir à leitura da lavra: `diff_calculador.py:154–162`, ANTES. |

Os novos contadores têm os seguintes escritores e consumidores:

| Contador | Quando incrementa | Quem lê / alcance |
|---|---|---|
| `lavra_repetida_no_fechamento` | Outra chamada motor para um `pk` já capturado com sucesso: `diff_calculador.py:199–201`, DEPOIS | Impressão de `dict(T)`: `diff_calculador.py:550`, ANTES. Mede a captura da empresa inteira. |
| `erro_lavra_do_fechamento` | Exceção dentro da captura; incrementa antes de relançar: `diff_calculador.py:202–206`, DEPOIS | Mesma impressão; o fechamento absorve a exceção: `fechamento.py:1206–1209`. |
| `sem_lavra_do_fechamento` | Colaborador chegou com resultado, mas não existe entrada em `_lav_fech`: `diff_calculador.py:246–249`, DEPOIS | Mesma impressão; alcança somente os colaboradores selecionados que passaram pelas verificações anteriores: `diff_calculador.py:102–108`, `:145–162`, ANTES. |
| `dia_da_lavra_fora_da_janela` | Uma chave de dia capturada ficou fora de `ini..fim`: `diff_calculador.py:330–334`, DEPOIS | Mesma impressão. Conta **dias**, não horas, rubricas ou períodos. |

**Outros leitores durante as substituições.** Na pilha síncrona atual da lavratura motor, os auxiliares usados não consultam novamente o modelo; o acesso relevante é o import em `dia_pago.py:163`. O oráculo reutiliza o alias capturado e é ignorado, depois da restauração do modelo. (`dia_pago.py:167–290`; `fechamento.py:1245–1264`; `diff_calculador.py:75`, `:197–198`, DEPOIS.)

Existem outros importadores locais que **poderiam observar o substituto se executassem simultaneamente no mesmo processo**:

- Serviços de retrato, somas, furo, abonos e linhas: `dia_pago.py:442`, `:556`, `:585`, `:637`, `:695`, `:777`, `:852`.
- Porta de exportação: `app/folha/porta_export.py:71`, `:283`, `:330`.
- Comandos: `app/ponto/management/commands/lavrar_dias_pagos.py:82`; `app/ponto/management/commands/apurar_furo_da_exportada.py:58`.

A troca não tem isolamento por chamada. Uma chamada concorrente à função real poderia inserir linhas no mesmo `_linhas`; a extração não confere colaborador, competência ou versão. Outro fechamento poderia importar `_captar` e deixar de persistir. **Ocorrência concreta dessa concorrência: [não verificado].** (`diff_calculador.py:60`, `:71–82`, `:220–225`, DEPOIS; `dia_pago.py:163`; `fechamento.py:1168`.)

**2. Classes de reprovação**

| Classe | Resultado da revisão |
|---|---|
| **Segundo juiz que duplica juiz registrado** | **Não identificado na cura atual.** O helper delega à lavratura existente; não reimplementa classificação ou agrupamento. A pergunta de data continua em `_dia_de`. (`diff_calculador.py:73`, DEPOIS; `dia_pago.py:109–132`.) |
| **Segundo escritor fora da porta** | **Não cria outro escritor persistente de `DiaPago`.** Intercepta as operações da própria porta. Entretanto, “comando inteiro somente leitura” continua falso no ramo da auditoria preexistente. (`dia_pago.py:357–358`; `diff_calculador.py:49–69`, DEPOIS; `fechamento.py:551–557`; `app/core/observ.py:133–148`.) |
| **Relógio lido fora da borda** | **Não acrescenta leitura de relógio.** O instante do teto de furo já é capturado pelo fechamento e repassado ao julgamento. (`diff_calculador.py:25–83`, DEPOIS; `fechamento.py:595`, `:1083–1085`.) |
| **Derivação paralela de fonte única** | **Remove a existente**, substituindo os laços de soma pela projeção da lavra. Falta proteção suficiente contra sua volta dentro do helper. (`diff_calculador.py:246–268`, ANTES; `:73–83`, `:329–334`, DEPOIS; teste novo `:99–117`.) | [nao verificado]
| **Selo/RED sem mordida** | **Parcialmente cometido.** Morde reversão integral; não garante ausência de derivação paralela no helper nem exercita a integração de `_captar`. Detalhado abaixo. (`test_diff_calculador_lado_v2_e_a_lavra.py:41–117`, anexo.) |
| **Ordem de trava** | **Nenhuma inversão nova identificada.** As substituições são restauradas em `finally`; o problema demonstrável é ausência de isolamento do estado global. (`diff_calculador.py:71–75`, `:220–225`, DEPOIS.) |
| **Pouso antes do pré-requisito** | Os símbolos necessários existem na cópia: `dia_pago.py:34–42`, `:135–163`; `models.py:311–351`. Ordem efetiva de pouso, RED anterior e prova de frota concluída: **[não verificado]**. | [nao verificado]

L-095 sustenta ler a autoridade existente. L-110 permite medir impacto do caminho velho, mas impede transformar essa igualdade em certificação legal do cálculo. (`app/docs/LEIS.md:113`, `:136`.)

**3. Comparabilidade com a lavra motor de produção**

**Sim, para os mesmos insumos, nos campos numéricos das linhas de dia.** A função real monta essas linhas e só depois executa `delete`/`bulk_create`; o substituto captura precisamente esses objetos. Os campos de `DiaPago` são `FloatField`, portanto não há aqui perda por ignorar uma quantização decimal do modelo. (`dia_pago.py:343–358`; `models.py:326–351`; `diff_calculador.py:59–82`, DEPOIS.)

A referência nova passa a incorporar diferenças que o laço anterior omitia ou classificava de outra forma:

| Movimento frente ao ANTES | Origem |
|---|---|
| HE100 feriado e os dois splits noturnos | Não eram somados no ANTES; são lidos pela lavra. (`diff_calculador.py:253–268`, ANTES; `dia_pago.py:181–184`.) |
| FT repartida entre folga, HE100/HE100 feriado e trabalhadas | Partição do fechamento e distribuição da lavra. (`fechamento.py:909–949`; `dia_pago.py:234–265`.) |
| Noturnas e intra das três partições de FT | `dia_pago.py:223–225`, `:240`, `:251`, `:263`. |
| Trabalhadas dos órfãos | `fechamento.py:813–817`; `dia_pago.py:267–270`. |
| Atraso líquido após crédito no próprio dia | `dia_pago.py:190–195`. |
| Dias adicionais de grade, falta e furo, inclusive sem horas trabalhadas | `dia_pago.py:272–290`. |

Há limites importantes:

- **Não compara com o histórico já persistido.** Reexecuta o fechamento atual; igualdade com linhas reais de produção é **[não verificado]**. O modo `--contra-gravado` consulta `FechamentoMensal`, não o histórico diário de `DiaPago`. (`diff_calculador.py:140`, `:561–585`, ANTES.)
- **Não devolve a linha inteira:** exclui ajuste, detalhes, furo e metadados. O evento capturado é `backfill`; a escrita normal usa `recalcular_fechamento_mes`. (`diff_calculador.py:80–82`, DEPOIS; `dia_pago.py:333–356`; `fechamento.py:1205`.)
- **Preserva assimetrias do produto:** a partição FT usa calendário da entrada, enquanto a lavra distribui pelo mapa da jornada; atraso e órfãos também usam calendário. Não são divergências introduzidas pelo helper. (`fechamento.py:909–924`, `:813–817`; `credito_parcial.py:29–43`; `dia_pago.py:128–132`.)
- **Filtra dias fora da janela**, contabilizando-os separadamente. Logo, não representa a totalidade das linhas que `lavrar` produziria. (`diff_calculador.py:330–334`, DEPOIS.)

Na tabela final, divergência significa **uma combinação colaborador/dia/rubrica com diferença absoluta maior que 0,02 hora**; totais somam os valores, e o CSV arredonda a apresentação para duas casas. Não é contagem de erros legais confirmados. (`diff_calculador.py:527–542`, ANTES; `app/docs/LEIS.md:136`.)

**4. Os testes mantêm a pergunta?** [nao verificado]

| Teste | Resultado | [nao verificado]
|---|---|
| `app/ponto/tests/test_dia_da_jornada_e_legivel.py:98–118` | **Perde alcance sobre o agrupamento efetivo do v2.** Continua proibindo `k = algo.date()` dentro de `handle`, mas quem agora escolhe a data está em `dia_pago._dia_de`, fora desse escopo. (`dia_pago.py:126–132`.) |
| Mesmo arquivo, `:120–132` | Continua encontrando a leitura textual do mapa e os contadores, agora mantidos para diagnóstico. **Não prova que esse mapa determina `m_dia`.** (`diff_calculador.py:211–233`, ANTES; `:329–334`, DEPOIS.) |
| `app/ponto/tests/test_s5b_porta_unica.py:60–101` | **Mantém a pergunta do lado calculador:** as chamadas reais de `insumos_do_calculador` e `rubricas_por_dia` permanecem; o diff não introduz os pareadores proibidos. (`diff_calculador.py:309–324`, `:417–424`, ANTES.) |
| `app/ponto/tests/test_chamador_do_calculador_aplica_a_janela.py:61–111` | **Mantém seu alvo:** continua havendo `minutos_do_oraculo(..., pares=_pares)` e chamada da porta aceita pelo detector. A remoção do laço motor não esvazia essa verificação. (`diff_calculador.py:312`, `:414`, ANTES.) |

Isso **não deixa toda a regra de data sem proteção**: há casos específicos da autoridade em `app/ponto/tests/test_o116_lavratura_le_o_dia_do_motor.py:55–101`, incluindo mapa que discorda do calendário. O enfraquecimento apontado é dos selos do comando. 

Quanto ao **teste novo**: [nao verificado]

- **Reversão integral:** seria detectada pela ausência do helper e pela volta dos atributos proibidos no `handle`. (`test_diff_calculador_lado_v2_e_a_lavra.py:42`, `:86`, `:107–117`, anexo.)
- **Helper passando a somar por conta própria:** **não há garantia de detecção.** A AST só inspeciona `handle`; a outra asserção exige apenas um `ast.Name`, sem exigir chamada ou ligação do retorno com `m_dia`. (`test_diff_calculador_lado_v2_e_a_lavra.py:99–117`, anexo.)
- Um helper poderia chamar a lavra, preservar a propagação de exceções, descartar a captura e recomputar corretamente os exemplos fixos. As verificações de valores e restauração continuariam satisfeitas. (`test_diff_calculador_lado_v2_e_a_lavra.py:53–93`, anexo.)
- Nenhum caso comportamental novo executa `Command.handle`, a obtenção dos kwargs reais, as guardas de versão/repetição ou os contadores. (`test_diff_calculador_lado_v2_e_a_lavra.py:41–117`, anexo.)

Para proteger a delegação, falta um caso em que a lavra forneça um valor sentinela diferente de qualquer soma dos períodos, verificando que esse valor chega à comparação; os casos atuais só conferem valores fixos derivados das mesmas entradas. (`test_diff_calculador_lado_v2_e_a_lavra.py:53–76`, anexo.)

**5. Outras falhas e limites**

**A triagem atribui o contador errado aos barrados pela guarda.** A guarda exclui colaboradores antes do laço que cria o `ResultadoMes`; o comando consulta `_res_fech` e dá `continue` antes de consultar `_lav_fech`. Esses colaboradores caem em **`sem_resultado_do_fechamento`**, não em `sem_lavra_do_fechamento`. (`fechamento.py:545–548`, `:598`, `:729`; `diff_calculador.py:154–162`, ANTES; `:246–249`, DEPOIS.)

**A prova da 09 pode ficar sem o universo relevante.** Existe guarda para setembro/2026, e a combinação usada pelo comando — leitura com `lavrar_dias_pagos=True` — ativa o caminho classificado como escrita. Se a empresa estiver exportada, os colaboradores guardados são excluídos. Quais empresas satisfazem isso: **[não verificado]**. (`fechamento.py:36–43`, `:523–548`; `diff_calculador.py:140`, ANTES.)

**Ausência de resposta do calculador ainda pode virar zero.** O comando visita apenas `v.minutos`, mas compara a união dos dias e usa `{}`/zero quando falta `c_dia`. Os novos dias vindos da lavra ampliam esse domínio. Sem `--pares-da-autoridade`, `_rub_do_dia` fica vazio; o comando ainda pode publicar motor contra zero. É preexistente, porém limita a interpretação deste instrumento. (`diff_calculador.py:325–337`, `:417–431`, `:469–472`, `:527–535`, ANTES.)

**A promessa de falhar diante de qualquer novidade no modelo é excessiva.** O construtor aceita quaisquer campos e `filter` aceita quaisquer filtros sem validá-los. Algumas mudanças futuras passariam silenciosamente; nem toda novidade produziria `AttributeError`. (`diff_calculador.py:38–40`, `:55–69`, DEPOIS.)

**Os contadores não tornam falha o resultado do comando.** A exceção da lavra é absorvida pelo fechamento; o comando imprime os contadores e segue produzindo a tabela. Portanto, execução concluída pode conter comparação incompleta. (`fechamento.py:1206–1209`; `diff_calculador.py:550–556`, ANTES.)

## 31/10 12:03 · sessao 64a35c2d3817 · - · - · origem=Ronald (tmux)

4. A composição escolhida reutiliza autoridades registradas: F5=`janela_atual` e K6=`competencia_entregue`; o helper apenas encadeia dia → competência → entrega, sem predicado novo. K6 exige TXT vigente não invalidado. (`app/core/juizes.py:258`, `app/core/juizes.py:287`, `app/chamados/services/vigia_fio.py:775`, `app/folha/export.py:569`)

Existem predicados distintos, explicitamente separados: F6 considera fechamento aprovado/exportado por colaborador; K4 considera tranca viva; K9 considera TXT ou holerite publicado do próprio colaborador. Eles respondem outras perguntas, portanto não justificam substituir K6 neste gate. (`app/core/juizes.py:76`, `app/core/juizes.py:290`, `app/ponto/janelas.py:75`, `app/ponto/services/fechamento.py:1441`, `app/folha/export.py:578`)

A casa já usa “aberta” como “não entregue” nesse contexto: o vigia assim nomeia a composição existente, e o esmeril pergunta “o que ainda está aberto?” lendo K6. Não cabe inventar um novo predicado pela combinação de aprovação, tranca e pagamento. (`app/chamados/services/vigia_fio.py:473`, `app/chamados/services/vigia_fio.py:764`, `app/ponto/services/esmeril_espelho.py:38`, `app/ponto/tests/test_janela_da_lista_le_o_k6.py:6`)

Há também `marco_da_competencia`, cujo rótulo `aberta` significa ausência de todos os marcos; sua docstring o identifica como o que a tela mostra, não como autorização de emissão. (`app/ponto/services/fechamento.py:1486`, `app/ponto/services/fechamento.py:1511`, `app/ponto/services/fechamento.py:1521`)

O antigo `dia_em_competencia_exportada` foi removido; restam lápides, não outra implementação viva desse juiz. (`app/chamados/juizes.py:1665`, `app/core/juizes.py:589`)

5. Mutações previstas pela leitura estática — execução efetiva das mutações: [nao verificado].

| Teste | Mutação de produção que o torna vermelho | [nao verificado]
|---|---|
| `test_RED_limbo_em_exportada_nao_pergunta_e_vai_a_pauta_dp` | Remover o gate: a função retorna motivos e segue perguntando; remover só a escrita da pauta faz falhar `len(p)==1`. (`app/chamados/tests/test_f1c4_3_emissores.py:199`, `app/chamados/services/fabrica_da_celula.py:153`) |
| `test_limbo_em_aberta_pergunta_sem_pauta` | Substituir o resultado da consulta por `True`: retorna `[]` e escreve pauta, contrariando ambas as asserções. (`app/chamados/tests/test_f1c4_3_emissores.py:207`, `app/chamados/services/fabrica_da_celula.py:155`) |
| `test_MORDE_trabalho_em_exportada_segue_perguntando_sem_pauta` | Alargar `cel.trabalha is None` para `cel.trabalha is not False`: trabalho exportado passa a retornar `[]` e escrever pauta. O teste protege precisamente a exceção que o comentário de produção atribui à O44. (`app/chamados/tests/test_f1c4_3_emissores.py:211`, `app/chamados/services/fabrica_da_celula.py:151`) |
| `test_RED_limbo_em_exportada_duas_vezes_uma_pauta` | Trocar o cálculo de `novos` por `list(motivos)`: a segunda execução escreve outra pauta, fazendo o total subir para dois. (`app/chamados/tests/test_f1c4_3_emissores.py:216`, `app/chamados/services/fabrica_da_celula.py:177`) |

A fixture tem saída efetivamente apagada, ata de trabalho e veredito `furo`, além de vínculo real vigente; portanto não é simplesmente um cenário vazio. O marco final `S` corresponde a `orfao_14h`. (`app/chamados/tests/test_f1c4_3_emissores.py:45`, `app/chamados/tests/test_f1c4_3_emissores.py:63`, `app/chamados/tests/test_f1c4_3_emissores.py:65`, `app/chamados/tests/_vinculo_do_dia.py:15`, `app/chamados/services/declaracao_texto.py:42`)

A deduplicação é alcançada na segunda execução: o ramo exportado escreve pauta e retorna antes de criar pergunta; assim a existência de uma pergunta nova não esvazia os motivos da segunda passagem. (`app/chamados/services/fabrica_da_celula.py:103`, `app/chamados/services/fabrica_da_celula.py:155`, `app/chamados/tests/test_f1c4_3_emissores.py:218`)

Os testes de aberta e trabalho exportado verificam retorno truthy, mas não verificam nascimento de `PerguntaDisputa`: retirar a chamada a `perguntar`, preservando `return ms`, sobrevive a essas duas verificações. (`app/chamados/tests/test_f1c4_3_emissores.py:207`, `app/chamados/tests/test_f1c4_3_emissores.py:211`, `app/chamados/services/fabrica_da_celula.py:158`)

Os testes também consultam pautas usando o mesmo formato `colab:data` escrito pelo helper, de modo que não testam a compatibilidade com os leitores existentes. Os quatro corpos não exercitam resposta, arquivo, concorrência, TXT invalidado ou retorno `None` do helper de competência. (`app/chamados/tests/test_f1c4_3_emissores.py:192`, `app/chamados/tests/test_f1c4_3_emissores.py:199`, `app/chamados/services/fabrica_da_celula.py:175`)

_999 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
