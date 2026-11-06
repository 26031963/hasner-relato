# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 13:51** · publicado em **06/11/2026 13:54** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 13:51 · sessao cd0d5a68c7a6 · - · - · origem=Ronald (tmux)

A dedução está correta para o script anexo: `sonda` significa `tb=None`. Um `raise` preservaria traceback e produziria um sítio com linha; portanto os 3191 registros `sonda LacunaDeMedicao` não demonstram disparo de nenhum `raise LacunaDeMedicao()` literal. Eles vêm de `falha(LacunaDeMedicao())` sem lançamento: contadores `T`, emissão DIA vazia, fecho ausente ou par diário ausente. [nao verificado]

Em P1, `SEM_PAR` permite atribuir 237 lacunas de outubro e 329 de novembro ao teste diário; restam 47 e 24 lacunas sintéticas cuja divisão entre os outros testes não é recuperável. [nao verificado] [nao verificado]

Analogamente, os `sonda ConservacaoNaoFecha` vêm das chamadas sintéticas de conferência mensal ou porta/emissor; o `raise` de `n != len(capturadas)` teria sítio com linha. [nao verificado]

## 06/11 13:49 · sessao cd0d5a68c7a6 · - · - · origem=Ronald (tmux)

A falha importante seria excluir por qualquer exceção propagada capturada pelo tracer. A alimentação trata exceções e continua com abstenção/default declarado em `app/ponto/calculador/alimentacao.py:389` e `app/ponto/calculador/alimentacao.py:500`; o tracer deve manter diagnóstico separado. Exclusão exige falha da operação ou violação concreta do contrato.

“Tentativa sem retorno” só deve receber classe PRODUTO quando a falha veio da autoridade real; exceção levantada pelo captador é INSTRUMENTO. [nao verificado]

Também preserve `SOMA` independente do registro global de erros e da totalidade do universo: valide antes de acumular e julgue apenas os colaboradores admitidos. Caso contrário, a exclusão declarada recria o `False` geral. [nao verificado]

## 06/11 13:47 · sessao a01dce60e12a · - · - · origem=Ronald (tmux)

Mapa para a pergunta 4:

| Valor | Quem escreve a fonte | Quem lê |
|---|---|---|
| `v2_h` da `RUBRICA` do anexo | `DP.lavrar`: períodos normais em `app/ponto/services/dia_pago.py:173`; campos em `:178`–`:186`; FT em `:234`, `:244`, `:257`; atraso em `:190`; órfãos em `:267`. Constrói `DiaPago` em `:343` e persiste em `:358`. Chamada real em `app/ponto/services/fechamento.py:1185`, versão motor em `:1211`. | No anexo: `emissor_capturado` → `c["emitido"]["motor"]` → `acumular` → `comparados` → `publicar`; numeração do anexo [nao verificado]. |
| `v3_h` da `RUBRICA` do anexo | A mesma `DP.lavrar`, substituindo valores decididos da porta em `app/ponto/services/dia_pago.py:320`–`:328`, construindo em `:343` e persistindo em `:358`; chamada em `app/ponto/services/fechamento.py:1261`, `rubricas` em `:1272`, versão em `:1278`. | No anexo: `emissor_capturado` → `c["emitido"]["oraculo"]` → `acumular` → `comparados` → `publicar`; numeração [nao verificado]. |
| `porta` | `rubricas_por_dia` é a fronteira, `app/ponto/calculador/alimentacao.py:353`; chamada do fechamento em `app/ponto/services/fechamento.py:1255`; o diff tem sua própria chamada real dessa mesma função em `app/ponto/management/commands/diff_calculador.py:417`. | DP lê os `Rubricas.valores` em `app/ponto/services/dia_pago.py:322`; diff lê em `app/ponto/management/commands/diff_calculador.py:469` e guarda em `:501`; anexo lê kwargs de `lavrar`, atualmente somente após retorno bem-sucedido [nao verificado]. |
| `emitido_decidido` | Objetos `DiaPago` construídos pelo único emissor `app/ponto/services/dia_pago.py:343`; horas de origem do motor e substituições descritas acima. | Anexo seleciona apenas células com `r in valores` e `r not in cegos`, somando `b[r]`; numeração [nao verificado]. |
| `INSTRUMENTO_HISTORICO` | Diferente de `v2_h` emitido: `m_dia` montado de períodos em `app/ponto/management/commands/diff_calculador.py:246`–`:268`; `c_dia` vem da porta em `:501`; `S` acumula em `:535`–`:537`. | Tracer lê `S` no retorno do comando; anexo [nao verificado]. |

Autoridades de referência: `app/docs/JUIZES.md:29` nomeia motor/fábrica CCT; `:35` nomeia janela de folha; `:20` nomeia geometria. O resultado composto nasce em `app/ponto/services/fechamento.py:182`, cada fatia chama o motor em `:277`, e a composição ocorre em `:285`–`:291`. O `setdefault` do tracer nativo do comando em `app/ponto/management/commands/diff_calculador.py:131`–`:135` continua capturando a primeira instância composta.

Leitores produtivos concretos:

- Calendário pede as linhas em `app/colaboradores/services/calendario.py:290` e lê a rubrica em `:655`–`:656`; a porta tem versão padrão motor em `app/ponto/services/dia_pago.py:831`.
- `rubricas_do_dia` lê campos em `app/ponto/services/dia_pago.py:912`–`:924`; não soma variantes noturna/feriado aos totais, conforme `:902`–`:904`.
- Porta export consulta dias motor em `app/folha/porta_export.py:588`–`:589`.
- `soma_da_competencia` agrega campos de `DiaPago` em `app/ponto/services/dia_pago.py:557`–`:561`, com arredondamento final.
- O comentário “nenhum leitor lê oráculo” de `app/ponto/services/fechamento.py:1231` é histórico; não entreguei censo global de todos os leitores [nao verificado].

Defeito de captura confirmado na lógica do anexo: rubricas/cegos são guardados **depois** de `REAL_LAVRAR`. A recusa impede a captura e depois aparece como emissão ausente/SEM_PAR, produzindo falhas secundárias [nao verificado]. A correção deve capturar kwargs antes da chamada real e registrar a recusa à parte; não inventar emissão. O produto captura a exceção e continua em `app/ponto/services/fechamento.py:1281`.

## 06/11 13:46 · sessao cd0d5a68c7a6 · - · - · origem=Ronald (tmux)

- **A saída não permite repartir exatamente as 3191 lacunas por sítio.** O registro sintético usa `tb=None` e agrupa em `sonda`; não conserva a causa nem a linha chamadora. A associação integral das contagens aos sítios é [nao verificado]. P1 informa 237 dias `SEM_PAR` em outubro e 329 em novembro; o loop do anexo registra uma lacuna por chave com um emissor ausente, mas outras lacunas vêm dos testes de vazio, `T`, fechos e donos. A decomposição restante é [nao verificado]. [nao verificado]

- **Defeito concreto da captura:** o anexo registra `colabs`, `rubricas` e `nao_decide` apenas depois de `REAL_LAVRAR` retornar. A recusa em `dia_pago.py:315` impede essa captura, embora o chamador tenha fornecido `_rub_o` em `fechamento.py:1272`. O próprio fechamento captura a recusa em `fechamento.py:1281` e continua. Assim, uma recusa declarada gera artificialmente “oráculo vazio” e lacunas para todos os dias do motor no script anexo. Capture argumentos antes da chamada, registre tentativa/retorno independentemente e classifique a recusa uma única vez.

- **Outro defeito lógico do instrumento:** `if not c["emitido"][versao]` não distingue chamada ausente de lavratura bem sucedida sem linhas DIA. A função sempre emite AJUSTE (`dia_pago.py:346`, `dia_pago.py:355`), enquanto os dias dependem de períodos, furos e mapas (`dia_pago.py:173`, `dia_pago.py:267`, `dia_pago.py:272`, `dia_pago.py:280`, `dia_pago.py:286`). Ocorrência desse segundo caso nos anexos: [nao verificado].

- **O que torna um dia lacuna no anexo:** pertencer à união de chaves DIA motor/oráculo e faltar em um dos mapas; não depende de valor diferente, presença de batida ou rubrica divergente. O exemplo com data é produzido por esse teste ou pelos testes de janela/dono; a saída agregada não guarda o sítio para distinguir exatamente [nao verificado]. A captura proposta deve registrar classe explícita por dia. [nao verificado]

- **Proveniência real do caminho:** o comando chama o fechamento em `diff_calculador.py:140`, após instalar `_nada` em `diff_calculador.py:137`; o wrapper do anexo substitui temporariamente essa função por seu captador durante a chamada real. O fechamento chama `lavrar` motor em `fechamento.py:1185` e oráculo em `fechamento.py:1261`; este recebe `rubricas=_rub_o` em `fechamento.py:1272`. A alimentação escreve `_rub_o` pela chamada `fechamento.py:1255`; a função retorna o mapa em `calculador/alimentacao.py:533` e escreve cada entrada com `regras.do_dia` em `calculador/alimentacao.py:514`.

- **Os universos de dias são distintos:** a porta do calculador visita os dias do núcleo mais os dias bloqueados publicados pelo motor (`calculador/alimentacao.py:467`, `calculador/alimentacao.py:469`, `calculador/alimentacao.py:471`); a lavratura também cria dias por furos e grade (`dia_pago.py:280`, `dia_pago.py:286`). Consequentemente, ausência na porta é abstenção ou diferença de universo a investigar, não automaticamente defeito de captura.

- **Histórico do DIFF não é a soma emitida:** o comando escreve `m_dia` diretamente de períodos em `diff_calculador.py:253`, escreve `c_dia` a partir das rubricas em `diff_calculador.py:501`, exclui cegueiras em `diff_calculador.py:532` e soma `S` em `diff_calculador.py:536`. O script soma objetos DIA capturados de `lavrar`; são grandezas/caminhos distintos.

- Para a reescrita: capture a porta **antes** da chamada; conte chamada e retorno explicitamente; preserve recusa esperada como `RECUSA_PORTA_SEM_DONO`; não transforme recusa em centenas de lacunas derivadas; valide cada colaborador antes de acumular ou imprimir `DIA`, `DONO` e `RESIDUO`; publique classes de produto separadamente; mantenha contagem exata dos dias afetados por conjuntos, evitando multiplicar um dia por rubrica. São recomendações de desenho [nao verificado].

## 06/11 13:46 · sessao 435a81aabac3 · - · - · origem=Ronald (tmux)

Referências confirmadas, sem executar sonda ou alterar arquivos:

- `porta`: escritor primário é `regras.do_dia`, chamado por `rubricas_por_dia` em `app/ponto/calculador/alimentacao.py:514`; retorno em `app/ponto/calculador/regras.py:874`. O fechamento chama essa porta em `app/ponto/services/fechamento.py:1255` e passa `rubricas=_rub_o` a `lavrar` em `app/ponto/services/fechamento.py:1272`.
- `emitido`: `lavrar` primeiro distribui motor em `app/ponto/services/dia_pago.py:178`, extras em `:180`, noturnas em `:181`/`:184`, folgas em `:239`, feriado em `:249`, ligação/grade em `:267`/`:286`; substitui cada campo decidido pela porta em `:328`; constrói `DiaPago` em `:343`; persiste em `:357`/`:358`.
- Neste caminho, o fechamento não passa `do_motor` (`app/ponto/services/fechamento.py:1261`). Portanto cada célula decidida capturada da porta deve reaparecer idêntica no objeto emitido pela atribuição `float(_valor or 0)` (`app/ponto/services/dia_pago.py:328`). Uma diferença atribuída especificamente a essa igualdade exigiria identificar células, campos e conjuntos; a saída fornecida não identifica isso. [nao verificado]
- Os 740 `ConservacaoNaoFecha` não provam 740 falhas porta/emissor: o anexo usa a mesma classe em três lugares (`n != capturadas`, motor/dias versus fechamento, porta versus emissor) e publica todos como `sonda`, sem sítio. Distribuição entre os três locais: [nao verificado].
- P1 publica diferença real do seu próprio instrumento em novembro/trabalhadas: `dias=5478.32`, `retorno=5478.33`; causa exata e colaboradores responsáveis não constam. [nao verificado] O mensal arredonda os totais em `app/ponto/services/fechamento.py:909` e seguintes; trabalhadas também pode receber segunda soma/arredondamento em `:968`. Comparar `round(fsum(DiaPago),2)` com mensal que já arredondou parcela antes de somar outra pode revelar diferença de produto, mas não autoriza nomear todas as 740 assim. [nao verificado]
- Se mesmos insumos e mesmo campo produzido pela ligação interna divergem, o dono provável é `ESTRUTURA`, conforme coerência exigida pela L-099 em `CLAUDE.md:176`; campo concreto de cada ocorrência permanece [nao verificado].

A recusa:

- `diff_calculador` chama `_recalc` em `app/ponto/management/commands/diff_calculador.py:140`; ele normalmente substitui `lavrar` por `_nada` em `:137`, mas o wrapper anexo reinstala seu captador dentro da chamada real. [nao verificado quanto à execução]
- O chamador real com `rubricas` é `app/ponto/services/fechamento.py:1261`/`:1272`. O próprio fechamento captura a exceção em `:1281`, registra e segue; assim `rc=0` não prova lavratura completa (`:1293`).
- A recusa em `app/ponto/services/dia_pago.py:315` protege campos sem proveniência; `_DONOS_DECLARADOS` reúne chaves de `valores` **e** `nao_decide` em `:61`–`:75`. Recusa é comportamento esperado do guard.
- A mensagem da recusa ainda fala em lei do ancoramento pendente (`app/ponto/services/dia_pago.py:317`), mas é texto histórico: a lei vigente foi respondida, com ancoramento por período, em `app/ponto/calculador/regras.py:414`–`:423`; a implementação chama a mesma `he_noturna` em `:825`/`:832`.
- Só parte dos colaboradores recusa porque o conjunto de declarações depende de `rubricas.values()`. Se o mapa é vazio, não há declaração de nenhum dos onze campos do calculador, embora grade/falta possam estar declaradas (`app/ponto/services/dia_pago.py:61`, `:64`, `:66`). A própria porta documenta o caso `rubricas={}` e onze campos sem dono em `app/ponto/calculador/alimentacao.py:459`–`:466`; seu universo é núcleo com pares válidos mais dias bloqueados publicados pelo motor (`:467`–`:475`).
- Não se pode assegurar que todas as 182 recusas sejam mapa vazio: nomes dos campos recusados não aparecem no dado raspado. [nao verificado] A nova captura deve calcular `_sem_dono` **antes** de `REAL_LAVRAR`, guardar classe própria e quantidade de dias do universo emitido/motor, e excluir o colaborador inteiro.
- FOLGA com splits sem valor/causa já tem declaração explícita em `app/ponto/calculador/regras.py:704`–`:715`; não atribuir automaticamente recusas atuais a esse defeito histórico.
- As frequências “16–21” valem para exemplos de algumas partes; no agregado fornecido variam 11–21 ocorrências por parte/competência. [nao verificado]

JUIZES aponta cálculo para motor/CCT em `app/docs/JUIZES.md:29`, régua em `:30`, janela em `:35`. A medida deve permanecer IMPACTO, nunca validação, por `CLAUDE.md:58` e `CLAUDE.md:415`.

## 06/11 13:37 · sessao 6a744852f01e · R1-CAUSAD-V3 · leitura · origem=Code

A consolidação abaixo mede **impacto**, conforme a distinção da L-110; não certifica o v3. [CLAUDE.md:49](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/CLAUDE.md:49)

Os números e suas contas derivam exclusivamente dos seis anexos, sem verificação independente. Nas tabelas, cada célula `Δ/C/D` significa **delta em horas / colaboradores divergentes / dias-colaborador divergentes**. Os nomes das rubricas omitem somente o prefixo `horas_`. [nao verificado]

A coluna **Σ emitido** soma os `delta_h` de `RUBRICA`, preservando todas as casas publicadas. A última coluna calcula separadamente `Σ(PARCIAL.v3_h − PARCIAL.v2_h)`, usando integralmente os decimais transcritos, sem arredondamento intermediário. [nao verificado] [nao verificado]

Para **10/2026**, a conta parte a parte é: [nao verificado]

| Rubrica | P1 Δ/C/D | P2 Δ/C/D | P3 Δ/C/D | P4 Δ/C/D | P5 Δ/C/D | P6 Δ/C/D | Σ emitido Δ/C/D | Δ pelos PARCIAL integrais | Evidência |
|---|---|---|---|---|---|---|---|---|---|
| trabalhadas | −2.542890556/1/1 | −2.842305278/1/1 | −3.035321667/2/2 | 0/0/0 | −9.540302056/3/3 | −4.528065278/1/1 | **−22.488884835/8/8** | indisponível integralmente¹ | [nao verificado] |
| intra_indenizada | −2/1/2 | 0/0/0 | −4.056666944/4/9 | −0.635273333/2/3 | −8.166666667/1/7 | −8/2/8 | **−22.858606944/10/29** | −22.85860694444437 | [nao verificado] |
| extras_50 | 0/0/0 | 0/0/0 | 0/0/0 | −0.502948333/1/2 | +1.473515854/2/5 | +0.224154213/1/1 | **+1.194721734/4/8** | +1.1947217330555591 | [nao verificado] |
| extras_100 | 0/0/0 | +10.985971389/1/1 | +12.149131944/1/1 | 0/0/0 | 0/0/0 | 0/0/0 | **+23.135103333/2/2** | +23.135103333333337 | [nao verificado] |
| extras_100_feriado | 0/0/0 | +10.985971389/1/1 | +12.149131944/1/1 | 0/0/0 | 0/0/0 | 0/0/0 | **+23.135103333/2/2** | +23.135103333333337 | [nao verificado] |
| extras_50_noturna | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | **0/0/0** | 0 | [nao verificado] |
| extras_100_noturna | +2.061344224/3/3 | +7.021076119/3/3 | +0.014159722/0/0 | +1.404236338/2/2 | +0.911578611/1/1 | +1/1/1 | **+12.412395014/10/10** | +12.4123950152777756 | [nao verificado] |
| folga_trabalhada | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | **0/0/0** | 0 | [nao verificado] |
| noturnas | 0/0/0 | −2.724547857/3/11 | 0/0/0 | +2.571669984/1/3 | +0.561815714/1/4 | +2.164548292/1/8 | **+2.573486133/6/26** | +2.5734861329370 | [nao verificado] |
| atraso | +2.368802113/1/1 | +4.579204284/3/4 | +1.106468535/1/1 | +0.186039155/1/1 | +2.329948153/1/1 | −0.000374092/0/0 | **+10.570088148/7/8** | +10.570088148888894 | [nao verificado] |
| saida_antecipada | +0.000684535/0/0 | +0.000106703/0/0 | −0.557056843/1/1 | −0.186263275/1/1 | +0.000085790/0/0 | +0.000069986/0/0 | **−0.742373104/2/2** | −0.742373103611115 | [nao verificado] |

¹ O `PARCIAL` de P2/trabalhadas traz `v2_h=11749.[cpf]`. Sem reconstruir esse valor, a soma integral das seis partes fica indisponível. A soma dos cinco deltas integrais disponíveis é `−19.646579556389`; acrescentar o delta explícito de `RUBRICA` de P2, `−2.842305278`, produz **−22.488884834389**, com precisão mista. [nao verificado]

Para **11/2026**, a conta parte a parte é: [nao verificado]

| Rubrica | P1 Δ/C/D | P2 Δ/C/D | P3 Δ/C/D | P4 Δ/C/D | P5 Δ/C/D | P6 Δ/C/D | Σ emitido Δ/C/D | Δ pelos PARCIAL integrais | Evidência |
|---|---|---|---|---|---|---|---|---|---|
| trabalhadas | −0.000000000/0/0 | −2.842521667/1/1 | −3.993019444/1/1 | 0/0/0 | −17.080034312/3/5 | 0/0/0 | **−23.915575423/5/7** | −23.9155754230560 | [nao verificado] |
| intra_indenizada | −1.166666667/1/1 | 0/0/0 | −1/1/1 | +0.403343056/1/2 | −9.628472778/3/10 | −6/2/6 | **−17.391796389/8/20** | −17.39179638888888 | [nao verificado] |
| extras_50 | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | −3.336169674/3/3 | +0.380845000/1/2 | **−2.955324674/4/5** | −2.9553246744444423 | [nao verificado] |
| extras_100 | 0/0/0 | −7.052801863/1/1 | +12.066969812/1/1 | +3.958275278/1/1 | +20.045843333/1/3 | 0/0/0 | **+29.018286560/4/6** | +29.018286560000008 | [nao verificado] |
| extras_100_feriado | 0/0/0 | −7.052801863/1/1 | +12.066969812/1/1 | +3.958275278/1/1 | +16.043546944/1/1 | 0/0/0 | **+25.015990171/4/4** | +25.015990171111121 | [nao verificado] |
| extras_50_noturna | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | **0/0/0** | 0 | [nao verificado] |
| extras_100_noturna | +1.137680830/2/2 | +7.819367573/3/3 | +1/1/1 | +1.499958300/2/2 | +0.002409167/0/0 | +1.861278889/1/1 | **+13.320694759/9/9** | +13.3206947588888876 | [nao verificado] |
| folga_trabalhada | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | **0/0/0** | 0 | [nao verificado] |
| noturnas | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | 0/0/0 | **0/0/0** | 0 | [nao verificado] |
| atraso | −0.000158524/0/0 | +0.622596847/1/1 | +0.000051118/0/0 | +1.128749770/2/4 | +0.076214444/1/1 | −0.000105974/0/0 | **+1.827347681/4/6** | +1.827347681944451 | [nao verificado] |
| saida_antecipada | −0.000158889/0/0 | +0.000201111/0/0 | +0.000008889/0/0 | −0.755222121/1/3 | −0.076319289/1/1 | +4.331691892/1/1 | **+3.500201593/3/5** | +3.500201593055542 | [nao verificado] |

As duas colunas de soma preservam a diferença de precisão existente nos anexos. Por exemplo, outubro/extras_50 soma **+1.194721734** pelos deltas impressos; pelos `PARCIAL`, soma **+1.1947217330555591**, que somente ao final, em nove casas, resulta em **+1.194721733**. [nao verificado]

Os **donos de 10/2026** seguem abaixo. Cada parcela mantém `Δ/C/D`; partes omitidas não publicaram uma linha `DONO` para aquela combinação. A linha conjunta de extras_100/feriado vale separadamente para cada uma das duas rubricas. [nao verificado]

| Rubrica | Dono | Conta por parte — Δ/C/D | Total Δ/C/D | Evidência |
|---|---|---|---|---|
| trabalhadas | CADASTRO | P1(−2.542890556/1/1) + P2(−2.842305278/1/1) + P5(−9.540302056/3/3) + P6(−4.528065278/1/1) | **−19.453563168/6/6** | [nao verificado] |
| trabalhadas | BATIDA | P3(−8/1/1) | **−8/1/1** | [nao verificado] |
| trabalhadas | ESTRUTURA | P3(+4.964678333/1/1) | **+4.964678333/1/1** | [nao verificado] |
| intra_indenizada | BATIDA | P1(−2/1/2) + P3(−1.550810000/2/2) | **−3.550810000/3/4** | [nao verificado] |
| intra_indenizada | CADASTRO | P3(−3.505856944/3/6) + P4(−0.635273333/2/3) + P5(−8.166666667/1/7) + P6(−8/2/8) | **−20.307796944/8/24** | [nao verificado] |
| intra_indenizada | ESTRUTURA | P3(+1/1/1) | **+1/1/1** | [nao verificado] |
| extras_50 | CADASTRO | P4(−0.502948333/1/2) + P5(+1.473515854/2/5) + P6(+0.224154212/1/1) | **+1.194721733/4/8** | [nao verificado] |
| extras_100 e extras_100_feriado | ESTRUTURA | P2(+10.985971389/1/1) + P3(+12.149131944/1/1) | **+23.135103333/2/2**, cada rubrica | [nao verificado] |
| extras_100_noturna | CADASTRO | P2(+6.164677786/2/2) + P6(+1/1/1) | **+7.164677786/3/3** | [nao verificado] |
| extras_100_noturna | ESTRUTURA | P1(+2.063954502/3/3) + P2(+0.856398333/1/1) + P4(+1.404236338/2/2) + P5(+0.909870556/1/1) | **+5.234459729/7/7** | [nao verificado] |
| noturnas | BATIDA | P5(+0.145124127/1/1) | **+0.145124127/1/1** | [nao verificado] |
| noturnas | CADASTRO | P5(+0.134280516/1/1) + P6(+1.069427260/1/4) | **+1.203707776/2/5** | [nao verificado] |
| noturnas | ESTRUTURA | P2(−2.714647817/3/11) + P4(+2.571669984/1/3) + P5(+0.282411071/1/2) + P6(+1.095121032/1/4) | **+1.234554270/6/20** | [nao verificado] |
| atraso | CADASTRO | P2(+1/1/1) + P4(+0.186000000/1/1) | **+1.186000000/2/2** | [nao verificado] |
| atraso | ESTRUTURA | P1(+2.368500000/1/1) + P2(+3.578715833/2/3) + P3(+1.106833333/1/1) + P5(+2.330333333/1/1) | **+9.384382499/5/6** | [nao verificado] |
| saida_antecipada | CADASTRO | P4(−0.186166667/1/1) | **−0.186166667/1/1** | [nao verificado] |
| saida_antecipada | ESTRUTURA | P3(−0.557166667/1/1) | **−0.557166667/1/1** | [nao verificado] |

Os **donos de 11/2026**, com a mesma notação, são: [nao verificado]

| Rubrica | Dono | Conta por parte — Δ/C/D | Total Δ/C/D | Evidência |
|---|---|---|---|---|
| trabalhadas | CADASTRO | P2(−2.842521667/1/1) + P5(−17.080034312/3/5) | **−19.922555979/4/6** | [nao verificado] |
| trabalhadas | BATIDA | P3(−3.993019444/1/1) | **−3.993019444/1/1** | [nao verificado] |
| intra_indenizada | CADASTRO | P1(−1.166666667/1/1) + P3(−1/1/1) + P4(+0.403343056/1/2) + P5(−7.628472778/3/8) + P6(−6/2/6) | **−15.391796389/8/18** | [nao verificado] |
| intra_indenizada | BATIDA | P5(−2/1/2) | **−2/1/2** | [nao verificado] |
| extras_50 | CADASTRO | P5(−3.336169674/3/3) + P6(+0.380845000/1/2) | **−2.955324674/4/5** | [nao verificado] |
| extras_100 e extras_100_feriado | ESTRUTURA | P2(−7.052801863/1/1) + P3(+12.066969812/1/1) + P4(+3.958275278/1/1) | **+8.972443227/3/3**, cada rubrica | [nao verificado] |
| extras_100 | BATIDA | P5(+10.045843333/1/2) | **+10.045843333/1/2** | [nao verificado] |
| extras_100 | CADASTRO | P5(+10/1/1) | **+10/1/1** | [nao verificado] |
| extras_100_feriado | CADASTRO | P5(+16.043546944/1/1) | **+16.043546944/1/1** | [nao verificado] |
| extras_100_noturna | ESTRUTURA | P1(+1.138725830/2/2) + P2(+6.809822454/2/2) + P4(+1.485842467/2/2) | **+9.434390751/6/6** | [nao verificado] |
| extras_100_noturna | CADASTRO | P2(+1/1/1) + P3(+1/1/1) + P6(+1.861278889/1/1) | **+3.861278889/3/3** | [nao verificado] |
| atraso | ESTRUTURA | P2(+0.622824167/1/1) + P4(+0.373195000/1/1) | **+0.996019167/2/2** | [nao verificado] |
| atraso | CADASTRO | P4(+0.755333333/1/3) + P5(+0.076333333/1/1) | **+0.831666666/2/4** | [nao verificado] |
| saida_antecipada | CADASTRO | P4(−0.755333333/1/3) + P5(−0.076333333/1/1) | **−0.831666666/2/4** | [nao verificado] |
| saida_antecipada | ESTRUTURA | P6(+4.331833611/1/1) | **+4.331833611/1/1** | [nao verificado] |

Não foram publicadas linhas `DONO` para extras_50_noturna ou folga_trabalhada nas duas competências, nem para noturnas em novembro. [nao verificado] [nao verificado]

As contagens somam entre partes disjuntas, mas **colaboradores podem aparecer em mais de um dono da mesma rubrica**: outubro/intra soma 12 incidências por dono para 10 colaboradores da rubrica; outubro/noturnas, 9 para 6; novembro/intra, 9 para 8; novembro/extras_100, 5 para 4. [nao verificado]

Também há delta fora das linhas `DONO`, em `RESIDUO|ATE_LIMIAR`. Exemplo de outubro/P1/extras_100_noturna: `+2.063954502 − 0.002610278 = +2.061344224`. Outubro/P3 dessa rubrica apresenta `+0.014159722` com `0/0` divergentes; novembro/P5 apresenta `+0.002409167` com `0/0`. Esses valores permanecem na medida de impacto. [nao verificado]

Por parte, os registros `ERRO`, `INCOMPLETO`, retorno e linhas `SOMA` ficam assim. Nas contas de erro, **V = ValueError**, **C = ConservacaoNaoFecha**, **L = LacunaDeMedicao**; V aparece em `dia_pago.py:315`, enquanto C e L aparecem com origem `sonda`. São contagens de ocorrências reportadas, sem identificação de colaboradores únicos. [nao verificado]

| Parte | rc_sombra | Outubro: V + C + L | Novembro: V + C + L | INCOMPLETO: outubro + novembro | Linhas SOMA | Evidência |
|---|---:|---|---|---|---|---|
| P1 | 0 | 18 + 121 + 284 = **423** | 21 + 34 + 353 = **408** | 423 + 408 = **831** | 113 `False`; 0 `True` | [nao verificado] |
| P2 | 0 | 19 + 102 + 341 = **462** | 16 + 36 + 291 = **343** | 462 + 343 = **805** | 113 `False`; 0 `True` | [nao verificado] |
| P3 | 0 | 13 + 57 + 158 = **228** | 17 + 34 + 295 = **346** | 228 + 346 = **574** | 113 `False`; 0 `True` | [nao verificado] |
| P4 | 0 | 11 + 89 + 225 = **325** | 16 + 34 + 338 = **388** | 325 + 388 = **713** | 113 `False`; 0 `True` | [nao verificado] |
| P5 | 0 | 11 + 99 + 196 = **306** | 12 + 22 + 256 = **290** | 306 + 290 = **596** | 113 `False`; 0 `True` | [nao verificado] |
| P6 | 0 | 13 + 78 + 210 = **301** | 15 + 34 + 244 = **293** | 301 + 293 = **594** | 113 `False`; 0 `True` | [nao verificado] |
| Total | 0 nas seis partes | **85 + 546 + 1414 = 2045** | **97 + 194 + 1777 = 2068** | **2045 + 2068 = 4113** | **678 `False`; 0 `True`** | [nao verificado] |

A soma explícita dos `INCOMPLETO` é `831 + 805 + 574 + 713 + 596 + 594 = 4113`, também igual a `182 V + 740 C + 3191 L`. [nao verificado]

Cada parte publica, por competência, `1 COBERTURA + 11 rubricas × 5 famílias = 56` linhas `SOMA`. As cinco famílias são `PARTES_EMISSOR`, `DIVERGENCIA`, `COLABS_UNIAO_DIAS`, `V3_PORTA_EMISSOR` e `MOTOR_FECHAMENTO`. Portanto: `2 × 56 + 1 ERROS_ZERO = 113` por parte; `6 × 113 = 678` no conjunto. Todos os booleanos publicados são `False`. [nao verificado] [nao verificado]

O wrapper retorna o código da execução Docker e termina com `exit "$rc"`; isso explica por que o retorno do processo deve ser lido separadamente dos resultados publicados pela sonda. [bin/sonda_leitura.sh:241](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/bin/sonda_leitura.sh:241) [bin/sonda_leitura.sh:251](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/bin/sonda_leitura.sh:251)

O código do `script.py` específico não foi localizado na cópia; assim, não atribuo aos `False` um significado adicional de aprovação ou reprovação. [nao verificado]

As ocorrências **TRATADA_LOCAL** estão publicadas para a parte inteira, sem separação por competência: [nao verificado] [nao verificado]

| Sítio / tipo | P1 | P2 | P3 | P4 | P5 | P6 | Soma | Evidência |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| leitor_celula.py:25 / ValueError | 2552 | 2962 | 2290 | 2228 | 3014 | 1881 | **14927** | [nao verificado] |
| juiz_batida.py:75 / ValueError | 154382 | 172335 | 162681 | 145226 | 154656 | 146215 | **935495** | [nao verificado] |
| motor_calculo_v2.py:1075 / AttributeError | 3483 | 3816 | 3625 | 3602 | 4016 | 3819 | **22361** | [nao verificado] |
| fechamento.py:1029 / _GradeSemCelula | 5 | 2 | 4 | não publicada | não publicada | 4 | **15 reportadas** | [nao verificado] | [nao verificado]

P4 e P5 não publicam a linha `_GradeSemCelula`; essa ausência não comprova zero. As contagens locais não foram acrescentadas aos `INCOMPLETO`. [nao verificado]

O confronto **UNIVERSO_PARTE × UNIVERSO** é: [nao verificado]

| Competência | Conta P1 + P2 + P3 + P4 + P5 + P6 | UNIVERSO informado | Diferença | Evidência |
|---|---|---:|---:|---|
| 10/2026 | 110 + 109 + 102 + 96 + 107 + 100 = **624** | 624 | **0** | [nao verificado] |
| 11/2026 | 96 + 99 + 97 + 91 + 99 + 96 = **578** | 578 | **0** | [nao verificado] |

Essa igualdade numérica coexiste com `SOMA COBERTURA=False` em todas as partes; não elimina os `INCOMPLETO`. [nao verificado]

Quanto ao **`!`**, as saídas **não trazem a lista fechada exigida para a troca**, nem o estado de exportação por empresa/competência, nem valores monetários. Logo, não permitem identificar quais divergências desta medida moveriam dinheiro em competência exportada. [nao verificado] A lista fechada — “resto dos 38, ! do v2, ESTRUTURA do v3” — consta como entrega da obra no backlog. [app/docs/BACKLOG.md:1750](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/docs/BACKLOG.md:1750)

O código da cópia permite nomear estes pontos, sem atribuir-lhes alcance atual de produção:

- **Alterar o gravado de competência exportada** ou **editar `motor_calculo_v2.py`** exige `!` pela regra local. [CLAUDE.md:262](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/CLAUDE.md:262)
- O calculador depende do v2 para funções/classes/fator noturno, métodos de pontualidade e `he_noturna`. A dependência existe; eventual alteração no arquivo v2 está sujeita à regra acima. [app/ponto/calculador/regras.py:35](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/calculador/regras.py:35) [app/ponto/calculador/regras.py:112](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/calculador/regras.py:112) [app/ponto/calculador/regras.py:825](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/calculador/regras.py:825)
- A cópia documenta a **reancoragem da janela** e a **remoção da cópia de par na origem v2** como correções destinadas ao `!`. Seus números históricos não foram incorporados à presente medida. [app/ponto/calculador/periodo.py:659](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/calculador/periodo.py:659) [app/ponto/calculador/periodo.py:726](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/calculador/periodo.py:726)

`horas_falta`, `horas_reflexo_dsr` e `saldo_banco_horas` estão expressamente fora da medida nos seis anexos; seu impacto permanece indisponível. [nao verificado] Também não cabe somar as rubricas apresentadas como um total financeiro: o código declara que as variantes noturna e de feriado são subconjuntos das extras totais. [app/ponto/services/dia_pago.py:902](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/services/dia_pago.py:902)

## 06/11 13:35 · sessao 95d3b2f91445 · R1-CAUSAD-V3 · leitura · origem=Code

O consolidado resulta em **303 dias divergentes em 10/2026** e **201 em 11/2026**, com BATE de **95,950821862889%** e **94,820922442669%**, respectivamente. [nao verificado]

Todas as contas abaixo derivam exclusivamente do texto fornecido, sem arquivo:linha disponível. Os termos das somas seguem a ordem **P1 + P2 + P3 + P4 + P5 + P6**; as horas usam os decimais de `PARCIAL` e `PARCIAL_TOTAL`, sem arredondamento intermediário. [nao verificado]

Para **10/2026**, a soma de cada chave de `[1]` é: [nao verificado]

| Chave | Conta por parte = total | Referência |
|---|---|---|
| dias_comparados | 1175 + 1267 + 1232 + 1235 + 1282 + 1292 = **7483** | [nao verificado] |
| bate_ate_10min | 1136 + 1226 + 1165 + 1187 + 1213 + 1253 = **7180** | [nao verificado] |
| esp_zero_e6_trabalho | 1 + 2 + 12 + 6 + 9 + 5 = **35** | [nao verificado] |
| e6_zero_esp_trabalho | 3 + 4 + 6 + 1 + 3 + 2 = **19** | [nao verificado] |
| diverge_10_60 | 11 + 7 + 2 + 5 + 2 + 1 = **28** | [nao verificado] |
| diverge_acima_60 | 24 + 28 + 47 + 36 + 55 + 31 = **221** | [nao verificado] |
| dia_batida_impar | 32 + 64 + 66 + 56 + 76 + 53 = **347** | [nao verificado] |
| impar_que_BATE | 15 + 34 + 24 + 22 + 28 + 25 = **148** | [nao verificado] |
| impar_que_DIVERGE | 17 + 30 + 42 + 34 + 48 + 28 = **199** | [nao verificado] |
| impar_dentro_da_faixa | 14 + 24 + 33 + 30 + 42 + 25 = **168** | [nao verificado] |
| impar_FORA_da_faixa | 3 + 6 + 9 + 4 + 6 + 3 = **31** | [nao verificado] |
| impar_no_conjunto_do_dia | 17 + 30 + 42 + 33 + 48 + 28 = **198** | [nao verificado] |
| impar_SO_POR_CORTE_do_oraculo | 0 + 0 + 0 + 1 + 0 + 0 = **1** | [nao verificado] |
| dia_sem_linha_no_espelho | 0 + 0 + 0 + 0 + 0 + 0 = **0** | [nao verificado] |
| dia_sem_lavratura_no_espelho | 1719 + 1568 + 1422 + 1285 + 1539 + 1396 = **8929** | [nao verificado] |
| dia_isento_ou_ausencia | 329 + 349 + 345 + 305 + 271 + 279 = **1878** | [nao verificado] |
| dia_sem_trabalho_ambos | 34 + 24 + 22 + 18 + 45 + 12 = **155** | [nao verificado] |
| esp_desconta_janela_declarada | 43 + 62 + 39 + 37 + 73 + 21 = **275** | [nao verificado] |
| esp_lido_pelo_pago_h | 0 + 0 + 0 + 0 + 0 + 0 = **0** | [nao verificado] |

Para **11/2026**, a soma de cada chave de `[1]` é: [nao verificado]

| Chave | Conta por parte = total | Referência |
|---|---|---|
| dias_comparados | 603 + 675 + 629 + 610 + 690 + 674 = **3881** | [nao verificado] |
| bate_ate_10min | 568 + 641 + 599 + 596 + 629 + 647 = **3680** | [nao verificado] |
| esp_zero_e6_trabalho | 11 + 3 + 4 + 4 + 4 + 2 = **28** | [nao verificado] |
| e6_zero_esp_trabalho | 5 + 0 + 5 + 0 + 2 + 2 = **14** | [nao verificado] |
| diverge_10_60 | 1 + 2 + 0 + 0 + 6 + 1 = **10** | [nao verificado] |
| diverge_acima_60 | 18 + 29 + 21 + 10 + 49 + 22 = **149** | [nao verificado] |
| dia_batida_impar | 34 + 52 + 42 + 32 + 63 + 39 = **262** | [nao verificado] |
| impar_que_BATE | 14 + 24 + 22 + 22 + 23 + 18 = **123** | [nao verificado] |
| impar_que_DIVERGE | 20 + 28 + 20 + 10 + 40 + 21 = **139** | [nao verificado] |
| impar_dentro_da_faixa | 13 + 27 + 17 + 9 + 32 + 16 = **114** | [nao verificado] |
| impar_FORA_da_faixa | 7 + 1 + 3 + 1 + 8 + 5 = **25** | [nao verificado] |
| impar_no_conjunto_do_dia | 20 + 28 + 19 + 10 + 40 + 20 = **137** | [nao verificado] |
| impar_SO_POR_CORTE_do_oraculo | 0 + 0 + 1 + 0 + 0 + 1 = **2** | [nao verificado] |
| dia_sem_linha_no_espelho | 1344 + 1386 + 1358 + 1274 + 1386 + 1344 = **8092** | [nao verificado] |
| dia_sem_lavratura_no_espelho | 805 + 834 + 814 + 756 + 785 + 767 = **4761** | [nao verificado] |
| dia_isento_ou_ausencia | 174 + 120 + 147 + 148 + 130 + 155 = **874** | [nao verificado] |
| dia_sem_trabalho_ambos | 33 + 17 + 21 + 19 + 24 + 20 = **134** | [nao verificado] |
| esp_desconta_janela_declarada | 17 + 37 + 38 + 14 + 54 + 16 = **176** | [nao verificado] |
| esp_lido_pelo_pago_h | 0 + 0 + 0 + 0 + 0 + 0 = **0** | [nao verificado] |

As contas por dono de **10/2026**, incluindo TOTAL, são: [nao verificado]

| Dono | Medida | Conta por parte = total | Referência |
|---|---|---|---|
| ESTRUTURA | dia-colab | 10 + 5 + 3 + 3 + 3 + 2 = **26** | [nao verificado] |
| ESTRUTURA | horas | 9.116283817777775 + 11.084600277777778 + 15.209666666666665 + 15.566319135833336 + 29.23270916666667 + 8.15 = **88.359579064722224** | [nao verificado] |
| ESTRUTURA | colabs | 3 + 3 + 3 + 3 + 3 + 1 = **16** | [nao verificado] |
| CADASTRO | dia-colab | 10 + 8 + 7 + 23 + 21 + 9 = **78** | [nao verificado] |
| CADASTRO | horas | 38.355213611111104 + 20.397520712222224 + 35.88209174 + 107.60888681222221 + 60.14375256333333 + 44.14612722222221 = **306.533592661111078** | [nao verificado] |
| CADASTRO | colabs | 3 + 6 + 4 + 8 + 8 + 3 = **32** | [nao verificado] |
| BATIDA | dia-colab | 19 + 28 + 57 + 22 + 45 + 28 = **199** | [nao verificado] |
| BATIDA | horas | 108.81011656222222 + 136.0465618775 + 302.3993460441667 + 101.15787729194444 + 190.17304430305552 + 122.57225449861109 = **961.15920057749997** | [nao verificado] |
| BATIDA | colabs | 12 + 18 + 25 + 17 + 25 + 16 = **113** | [nao verificado] |
| TOTAL | dia-colab | 39 + 41 + 67 + 48 + 69 + 39 = **303** | [nao verificado] |
| TOTAL | horas | 156.281613991111099 + 167.528682867500002 + 353.491104450833365 + 224.333083239999986 + 279.54950603305552 + 174.86838172083330 = **1356.052372303333272** | [nao verificado] |
| TOTAL | colabs | 16 + 23 + 30 + 26 + 29 + 20 = **144** | [nao verificado] |

As contas por dono de **11/2026**, incluindo TOTAL, são: [nao verificado]

| Dono | Medida | Conta por parte = total | Referência |
|---|---|---|---|
| ESTRUTURA | dia-colab | 1 + 0 + 3 + 0 + 4 + 1 = **9** | [nao verificado] |
| ESTRUTURA | horas | 6.2 + 0.0 + 11.75 + 0.0 + 20.109676988055558 + 2.5604991666666668 = **40.6201761547222248** | [nao verificado] |
| ESTRUTURA | colabs | 1 + 0 + 2 + 0 + 3 + 1 = **7** | [nao verificado] |
| CADASTRO | dia-colab | 20 + 10 + 5 + 5 + 27 + 5 = **72** | [nao verificado] |
| CADASTRO | horas | 106.69305642305555 + 28.19539852888889 + 15.826795833333335 + 39.726189220555554 + 106.31719708361109 + 50.7295725 = **347.488209589444419** | [nao verificado] |
| CADASTRO | colabs | 7 + 7 + 5 + 3 + 7 + 4 = **33** | [nao verificado] |
| BATIDA | dia-colab | 14 + 24 + 22 + 9 + 30 + 21 = **120** | [nao verificado] |
| BATIDA | horas | 81.76095535388889 + 122.76838809305558 + 113.35641307083331 + 50.69357468888889 + 164.14298169666668 + 110.33564643499999 = **643.05795933833334** | [nao verificado] |
| BATIDA | colabs | 10 + 16 + 10 + 8 + 19 + 15 = **78** | [nao verificado] |
| TOTAL | dia-colab | 35 + 34 + 30 + 14 + 61 + 27 = **201** | [nao verificado] |
| TOTAL | horas | 194.65401177694444 + 150.96378662194447 + 140.933208904166645 + 90.419763909444444 + 290.569855768333328 + 163.6257181016666568 = **1031.1663450824999838** | [nao verificado] |
| TOTAL | colabs | 15 + 21 + 15 + 10 + 26 + 18 = **105** | [nao verificado] |

Para os colabs de TOTAL, somei as uniões informadas por parte, considerando a disjunção entre partes declarada no pedido. [nao verificado]

As conferências agregadas e os percentuais são: [nao verificado]

| Conferência | 10/2026 | 11/2026 | Referência |
|---|---|---|---|
| UNIVERSO_PARTE × UNIVERSO | 110 + 109 + 102 + 96 + 107 + 100 = **624**, igual a 624 | 96 + 99 + 97 + 91 + 99 + 96 = **578**, igual a 578 | [nao verificado] |
| BATE % | 7180 ÷ 7483 × 100 ≈ **95,950821862889%** | 3680 ÷ 3881 × 100 ≈ **94,820922442669%** | [nao verificado] |
| Comparados − BATE | 7483 − 7180 = **303** | 3881 − 3680 = **201** | [nao verificado] |
| Categorias divergentes de `[1]` | 35 + 19 + 28 + 221 = **303** | 28 + 14 + 10 + 149 = **201** | [nao verificado] |
| Dias dos donos | 26 + 78 + 199 = **303** | 9 + 72 + 120 = **201** | [nao verificado] |
| Horas dos donos | 88.359579064722224 + 306.533592661111078 + 961.15920057749997 = **1356.052372303333272** | 40.6201761547222248 + 347.488209589444419 + 643.05795933833334 = **1031.1663450824999838** | [nao verificado] |

Os diagnósticos finais aparecem por execução de parte, abrangendo as duas competências, sem separação mensal de `TRATADA_LOCAL`. A coluna “atravessaram frame” abaixo conta somente linhas explícitas desse diagnóstico exibidas nas saídas; não comprova ocorrências fora do texto fornecido. [nao verificado]

| Parte | rc_sombra | Marcador INCOMPLETO na saída | Linhas TRATADA_LOCAL | Ocorrências somadas de TRATADA_LOCAL | Linhas explícitas de erros que atravessaram frame | Referência |
|---|---:|---|---:|---:|---:|---|
| P1 | 0 | Ausente | 27 | 777169 | 0 | [nao verificado] |
| P2 | 0 | Ausente | 27 | 835848 | 0 | [nao verificado] |
| P3 | 0 | Ausente | 28 | 813047 | 0 | [nao verificado] |
| P4 | 0 | Ausente | 27 | 752749 | 0 | [nao verificado] |
| P5 | 0 | Ausente | 26 | 869997 | 0 | [nao verificado] |
| P6 | 0 | Ausente | 26 | 746948 | 0 | [nao verificado] |

As contas das ocorrências `TRATADA_LOCAL`, na ordem em que aparecem em cada saída, são: [nao verificado]

- P1: 232873 + 214 + 945 + 5201 + 57 + 2 + 5766 + 3 + 4 + 3444 + 3444 + 649 + 180 + 433065 + 505 + 2567 + 161 + 10520 + 104 + 3607 + 191 + 5 + 20459 + 4354 + 33979 + 8 + 14862 = **777169**. [nao verificado]
- P2: 249865 + 13 + 1341 + 5749 + 66 + 4 + 8 + 6433 + 2 + 6 + 4777 + 4777 + 498 + 185 + 459090 + 579 + 2775 + 192 + 11736 + 97 + 3584 + 197 + 2 + 23442 + 5013 + 37362 + 18055 = **835848**. [nao verificado]
- P3: 240100 + 250 + 1365 + 4623 + 41 + 4 + 2 + 6434 + 4 + 6 + 4324 + 4324 + 512 + 179 + 16 + 452220 + 554 + 2847 + 157 + 11514 + 101 + 3979 + 190 + 4 + 22023 + 4399 + 36499 + 16376 = **813047**. [nao verificado]
- P4: 220720 + 551 + 1090 + 4668 + 49 + 1 + 34 + 6712 + 8 + 3 + 3792 + 3792 + 284 + 168 + 1126 + 409650 + 558 + 2635 + 165 + 11428 + 89 + 3666 + 177 + 23759 + 3165 + 37032 + 17427 = **752749**. [nao verificado]
- P5: 257916 + 434 + 1676 + 6042 + 79 + 2 + 20 + 7771 + 6 + 5601 + 5601 + 310 + 190 + 471429 + 788 + 3044 + 194 + 12714 + 143 + 4139 + 195 + 27833 + 3152 + 41156 + 2 + 19560 = **869997**. [nao verificado]
- P6: 219208 + 16 + 1158 + 3928 + 56 + 2 + 8 + 6308 + 6 + 3771 + 3771 + 520 + 177 + 414465 + 470 + 2680 + 185 + 11236 + 73 + 3903 + 186 + 4 + 20152 + 3840 + 35418 + 15407 = **746948**. [nao verificado]

Total de linhas `TRATADA_LOCAL`: 27 + 27 + 28 + 27 + 26 + 26 = **161**; total de ocorrências: 777169 + 835848 + 813047 + 752749 + 869997 + 746948 = **4795758**. [nao verificado]

As linhas `SOMA` fornecidas ficam transcritas em colunas abaixo; cada célula preserva o valor e o booleano da respectiva linha. [nao verificado]

| Parte | Competência | SOMA dias | SOMA HORAS | SOMA COLABS_UNIAO | Referência |
|---|---|---|---|---|---|
| P1 | 10/2026 | 39 — True | 156.281613991111099 — True | 16 — True | [nao verificado] |
| P1 | 11/2026 | 35 — True | 194.65401177694444 — True | 15 — True | [nao verificado] |
| P2 | 10/2026 | 41 — True | 167.528682867500002 — True | 23 — True | [nao verificado] |
| P2 | 11/2026 | 34 — True | 150.96378662194447 — True | 21 — True | [nao verificado] |
| P3 | 10/2026 | 67 — True | 353.491104450833365 — True | 30 — True | [nao verificado] |
| P3 | 11/2026 | 30 — True | 140.933208904166645 — True | 15 — True | [nao verificado] |
| P4 | 10/2026 | 48 — True | 224.333083239999986 — True | 26 — True | [nao verificado] |
| P4 | 11/2026 | 14 — True | 90.419763909444444 — True | 10 — True | [nao verificado] |
| P5 | 10/2026 | 69 — True | 279.54950603305552 — True | 29 — True | [nao verificado] |
| P5 | 11/2026 | 61 — True | 290.569855768333328 — True | 26 — True | [nao verificado] |
| P6 | 10/2026 | 39 — True | 174.86838172083330 — True | 20 — True | [nao verificado] |
| P6 | 11/2026 | 27 — True | 163.6257181016666568 — True | 18 — True | [nao verificado] |

Contagem de `SOMA True`: 6 + 6 + 6 + 6 + 6 + 6 = **36**; de `SOMA False`: 0 + 0 + 0 + 0 + 0 + 0 = **0**. [nao verificado]

Não há linhas de **listas de dias** nem itens **ESPELHO-VERDADE** nos seis blocos de saída fornecidos; portanto, não há conteúdo dessas categorias para transcrever ou agregar. [nao verificado]

A tabela final por competência é: [nao verificado]

| Medida | 10/2026 | 11/2026 | Referência |
|---|---:|---:|---|
| Janela informada | 2026-09-21 a 2026-10-20 | 2026-10-21 a 2026-11-20 | [nao verificado] |
| UNIVERSO_PARTE somado / UNIVERSO | 624 / 624 — confere | 578 / 578 — confere | [nao verificado] |
| Dias comparados | 7483 | 3881 | [nao verificado] |
| Bate até 10 min | 7180 | 3680 | [nao verificado] |
| BATE % | 95,950821862889% | 94,820922442669% | [nao verificado] |
| ESTRUTURA — dia-colab / horas / colabs | 26 / 88.359579064722224 / 16 | 9 / 40.6201761547222248 / 7 | [nao verificado] |
| CADASTRO — dia-colab / horas / colabs | 78 / 306.533592661111078 / 32 | 72 / 347.488209589444419 / 33 | [nao verificado] |
| BATIDA — dia-colab / horas / colabs | 199 / 961.15920057749997 / 113 | 120 / 643.05795933833334 / 78 | [nao verificado] |
| **TOTAL — dia-colab / horas / colabs** | **303 / 1356.052372303333272 / 144** | **201 / 1031.1663450824999838 / 105** | [nao verificado] |
| SOMA, por competência | 3 + 3 + 3 + 3 + 3 + 3 = 18 True; nenhum False | 3 + 3 + 3 + 3 + 3 + 3 = 18 True; nenhum False | [nao verificado] |

## 06/11 13:34 · sessao 8ee384f1b5ff · - · - · origem=Ronald (tmux)

Todas as contas abaixo são derivadas dos anexos, em memória, com `Decimal` dos valores textuais de `PARCIAL`, sem arredondamento intermediário. `[nao verificado]`

Ordem dos vetores: **P1, P2, P3, P4, P5, P6**. `[nao verificado]`

| Competência/rubrica | Deltas integrais por parte | Total integral | Arredondamento somente final, 9 casas |
|---|---|---:|---:|
| 10 trabalhadas | −2.542890555555; **−2.842305278***; −3.035321666668; 0; −9.540302056388; −4.528065277778 | **−22.488884834389*** | −22.488884834* |
| 10 intra | −2; 0; −4.05666694444443; −0.63527333333331; −8.16666666666663; −8 | −22.85860694444437 | −22.858606944 |
| 10 HE50 | 0; 0; 0; −0.502948333333335; +1.4735158538888901; +0.224154212500004 | +1.1947217330555591 | +1.194721733 |
| 10 HE100 | 0; +10.985971388888896; +12.149131944444441; 0; 0; 0 | +23.135103333333337 | +23.135103333 |
| 10 HE100 feriado | mesmo vetor de HE100 | +23.135103333333337 | +23.135103333 |
| 10 HE100 noturna | +2.061344224166667; +7.021076119444445; +0.014159722222222; +1.4042363383333330; +0.9115786111111086; +1 | +12.4123950152777756 | +12.412395015 |
| 10 noturnas | 0; −2.7245478571431; 0; +2.5716699836512; +0.5618157142858; +2.1645482921431 | +2.5734861329370 | +2.573486133 |
| 10 atraso | +2.368802113333331; +4.579204284166672; +1.106468535277777; +0.186039155000003; +2.329948153055554; −0.000374091944443 | +10.570088148888894 | +10.570088149 |
| 10 saída | +0.000684534722225; +0.000106703055551; −0.557056842500004; −0.186263274722222; +0.000085790277780; +0.000069985555555 | −0.742373103611115 | −0.742373104 |
| 11 trabalhadas | −0.0000000000009; −2.8425216666665; −3.9930194444441; 0; −17.0800343119445; 0 | −23.9155754230560 | −23.915575423 |
| 11 intra | −1.16666666666666; 0; −1; +0.40334305555555; −9.62847277777777; −6 | −17.39179638888888 | −17.391796389 |
| 11 HE50 | 0; 0; 0; 0; −3.3361696744444433; +0.380845000000001 | −2.9553246744444423 | −2.955324674 |
| 11 HE100 | +0.000000000000015; −7.052801863055549; +12.066969811944446; +3.958275277777773; +20.045843333333323; 0 | +29.018286560000008 | +29.018286560 |
| 11 HE100 feriado | +0.000000000000015; −7.052801863055549; +12.066969811944446; +3.958275277777773; +16.043546944444436; 0 | +25.015990171111121 | +25.015990171 |
| 11 HE100 noturna | +1.137680829722223; +7.819367573333334; +1; +1.4999583002777781; +0.002409166666664; +1.8612788888888885 | +13.3206947588888876 | +13.320694759 |
| 11 atraso | −0.000158523888888; +0.622596846944447; +0.000051118333336; +1.128749770277779; +0.076214444444446; −0.000105974166669 | +1.827347681944451 | +1.827347682 |
| 11 saída | −0.000158888888890; +0.000201111111110; +0.000008888888885; −0.755222120833338; −0.076319288888886; +4.331691891666661 | +3.500201593055542 | +3.500201593 |

Fonte de todas as linhas da tabela: anexos `PARCIAL` fornecidos. `[nao verificado]`

\* **Outubro/trabalhadas tem precisão mista:** o `v2_h` do `PARCIAL` P2 foi raspado como `11749.[cpf]`; utilizei exclusivamente o delta explícito `RUBRICA`, −2.842305278. O total de precisão integral das seis partes é indisponível; −22.488884834389 é apenas o total misto declarado. `[nao verificado]`

As rubricas `horas_extras_50_noturna` e `horas_folga_trabalhada` têm delta integral zero nas duas competências; `horas_noturnas` tem delta integral zero em novembro. `[nao verificado]`

Comparação dos totais finais em 9 casas com a soma dos `delta_h` já emitidos por `RUBRICA`: `[nao verificado]`

| Competência/rubrica | Soma de `RUBRICA` | Soma `PARCIAL`, arredondada só ao final |
|---|---:|---:|
| 10 trabalhadas | −22.488884835 | −22.488884834* |
| 10 HE50 | +1.194721734 | +1.194721733 |
| 10 HE100 noturna | +12.412395014 | +12.412395015 |
| 10 atraso | +10.570088148 | +10.570088149 |
| 11 atraso | +1.827347681 | +1.827347682 |

Fonte: contas em memória dos anexos; demais rubricas coincidem em nove casas entre as duas formas. `[nao verificado]`

## 06/11 13:33 · sessao 6fec615fd82c · - · - · origem=Ronald (tmux)

Consolidação dos anexos, sem verificação independente. [nao verificado]

| Item | P1 | P2 | P3 | P4 | P5 | P6 | Total |
|---|---:|---:|---:|---:|---:|---:|---:|
| Erros outubro | 423 | 462 | 228 | 325 | 306 | 301 | 2045 |
| Erros novembro | 408 | 343 | 346 | 388 | 290 | 293 | 2068 |
| INCOMPLETO | 831 | 805 | 574 | 713 | 596 | 594 | 4113 |
| TRATADA_LOCAL leitor | 2552 | 2962 | 2290 | 2228 | 3014 | 1881 | 14927 |
| TRATADA_LOCAL juiz | 154382 | 172335 | 162681 | 145226 | 154656 | 146215 | 935495 |
| TRATADA_LOCAL motor | 3483 | 3816 | 3625 | 3602 | 4016 | 3819 | 22361 |
| TRATADA_LOCAL fechamento | 5 | 2 | 4 | ausente | ausente | 4 | 15 reportados |
| Linhas SOMA | 113 | 113 | 113 | 113 | 113 | 113 | 678 |

Toda a tabela deriva dos anexos. Ausência da linha de fechamento em P4/P5 não comprova zero. As 678 linhas `SOMA` exibem `False`; nenhuma exibe `True`. Cada parte contém `2 × (1 COBERTURA + 11 rubricas × 5 famílias) + 1 ERROS_ZERO = 113` linhas. [nao verificado]

Erros por competência: outubro `85 ValueError + 546 ConservacaoNaoFecha + 1414 LacunaDeMedicao = 2045`; novembro `97 + 194 + 1777 = 2068`; total `182 + 740 + 3191 = 4113`, igual à soma dos `INCOMPLETO`. [nao verificado]

Todos os vetores DONO, deltas exatos e contagens por parte foram enviados nas mensagens anteriores. Há sobreposição de colaboradores entre donos: outubro intra `12 incidências DONO / 10 colaboradores RUBRICA`; outubro noturnas `9/6`; novembro intra `9/8`; novembro extras100 `5/4`. Não somar esses donos para obter colaboradores únicos da rubrica. [nao verificado]

## 06/11 13:32 · sessao f98b37141a96 · - · - · origem=Ronald (tmux)

Achados de código, sem banco/log e sem escrita:

- **DIFF é impacto, não validação**; a regra exige dono L-099 para divergências. [CLAUDE.md:49](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/CLAUDE.md:49) [CLAUDE.md:52](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/CLAUDE.md:52)
- `rc=0` é retorno da execução Docker; o wrapper não avalia as linhas `SOMA` nem `INCOMPLETO`. O pipeline captura o retorno e termina com `exit "$rc"`. [bin/sonda_leitura.sh:241](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/bin/sonda_leitura.sh:241) [bin/sonda_leitura.sh:251](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/bin/sonda_leitura.sh:251)
- Não localizei na cópia o `script.py` específico nem os emissores de `SOMA COBERTURA`, `SOMA PARTES_EMISSOR`, `SOMA ERROS_ZERO`, `TRATADA_LOCAL` ou `ERROS_POR_TIPO`; portanto o significado booleano de `False` além do literal fornecido permanece `[nao verificado]`.
- O backlog registra expressamente a entrega pedida: DIFF 10/11 por rubrica/dono e “lista fechada do que a troca exige (resto dos 38, ! do v2, ESTRUTURA do v3)”; ali está em curso, sem lista preenchida. [app/docs/BACKLOG.md:1750](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/docs/BACKLOG.md:1750)

Os sítios de erro fornecidos correspondem a:

| Sítio | Comportamento no código |
|---|---|
| `dia_pago.py:315` | Levanta `ValueError` quando falta dono declarado de campo do dia. [app/ponto/services/dia_pago.py:310](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/services/dia_pago.py:310) |
| `leitor_celula.py:25` | Faz parse `HH:MM`; captura `Exception` e retorna `None`. [app/escala/services/leitor_celula.py:23](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/escala/services/leitor_celula.py:23) |
| `juiz_batida.py:75` | Faz parse `HH:MM`; captura `ValueError/AttributeError` e retorna `None`. [app/ponto/juiz_batida.py:72](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/juiz_batida.py:72) |
| `motor_calculo_v2.py:1075` | Atribui mapa `dia_da_jornada`; captura `Exception` e faz `pass`. [app/ponto/motor_calculo_v2.py:1068](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/motor_calculo_v2.py:1068) |
| `fechamento.py:1029` | Levanta `_GradeSemCelula` se todos os dias dentro do vínculo são `sem_celula`. [app/ponto/services/fechamento.py:1027](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/services/fechamento.py:1027) |

Para a lista `!`:

- Tocar o **gravado de competência exportada** e editar `motor_calculo_v2.py` exige `!` pela regra local. [CLAUDE.md:262](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/CLAUDE.md:262)
- As saídas não informam quais empresas/competências estão exportadas; não permitem atribuir `!` financeiro a colaboradores específicos. `[nao verificado]` O código pergunta isso por empresa e competência, via `empresas_exportadas_no_escopo`. [app/ponto/services/fechamento.py:1480](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/services/fechamento.py:1480)
- Há dependência concreta de v2: funções/classes/fator noturno importados, métodos de pontualidade emprestados, constantes de tolerância e função `he_noturna`. [app/ponto/calculador/regras.py:35](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/calculador/regras.py:35) [app/ponto/calculador/regras.py:112](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/calculador/regras.py:112) [app/ponto/calculador/periodo.py:163](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/calculador/periodo.py:163) [app/ponto/calculador/regras.py:825](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/calculador/regras.py:825)
- O releitor também recebe períodos do resultado atual, nos campos `periodos/periodos_ft`; a porta recusa campo ausente. [app/ponto/calculador/periodo.py:609](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/calculador/periodo.py:609) [app/ponto/calculador/nucleo.py:393](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/calculador/nucleo.py:393)
- O código documenta pendências v2 de reancoragem de janela e remoção de par duplicado na origem, explicitamente destinadas ao `!`; seus números históricos não devem ser trazidos como medida atual. [app/ponto/calculador/periodo.py:659](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/calculador/periodo.py:659) [app/ponto/calculador/periodo.py:726](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/calculador/periodo.py:726)
- Não somar rubricas como um total de dinheiro: extras noturnas/feriado são subconjuntos das extras totais; somá-las ao total duplica horas. [app/ponto/services/dia_pago.py:902](/home/ronald/codex_ws/f23cccec43b642ec2da05318e67a63bdf6a7d1f0/app/ponto/services/dia_pago.py:902)

## 06/11 13:23 · sessao c1ab63920e77 · R1-CAUSAD-V3 · sonda · origem=Code

Saída integral fornecida da parte 1 de 6: [nao verificado]

```text
sonda_leitura: script.py [27c01d2784902123] no cpuset 4-7, lendo sombra como leitor (LEITURA)

PARTE 1 PARTES 6
[0] UNIVERSO 10/2026 624
UNIVERSO_PARTE 10/2026 110
JANELA 10/2026 2026-09-21 2026-10-20
[0] UNIVERSO 11/2026 578
UNIVERSO_PARTE 11/2026 96
JANELA 11/2026 2026-10-21 2026-11-20
ESTIMATIVA processados 25 universo 206 minutos_total 2.7 ate_40_min True ate_15_min True
ESTIMATIVA processados 50 universo 206 minutos_total 3.0 ate_40_min True ate_15_min True
ESTIMATIVA processados 75 universo 206 minutos_total 2.9 ate_40_min True ate_15_min True
ESTIMATIVA processados 100 universo 206 minutos_total 2.7 ate_40_min True ate_15_min True
COMPETENCIA 10/2026
[1] {'dias_comparados': 1175, 'bate_ate_10min': 1136, 'esp_zero_e6_trabalho': 1, 'e6_zero_esp_trabalho': 3, 'diverge_10_60': 11, 'diverge_acima_60': 24, 'dia_batida_impar': 32, 'impar_que_BATE': 15, 'impar_que_DIVERGE': 17, 'impar_dentro_da_faixa': 14, 'impar_FORA_da_faixa': 3, 'impar_no_conjunto_do_dia': 17, 'impar_SO_POR_CORTE_do_oraculo': 0, 'dia_sem_linha_no_espelho': 0, 'dia_sem_lavratura_no_espelho': 1719, 'dia_isento_ou_ausencia': 329, 'dia_sem_trabalho_ambos': 34, 'esp_desconta_janela_declarada': 43, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 96.7% de 1175 dias comparados
[3] ESTRUTURA    10 dia-colab       9.1 h    3 colabs
[3] CADASTRO     10 dia-colab      38.4 h    3 colabs
[3] BATIDA       19 dia-colab     108.8 h   12 colabs
[4] TOTAL divergentes 39
[5] colabs com divergencia: 16
erros_no_espelho 0
PARCIAL|10/2026|ESTRUTURA|dias=10|horas=9.116283817777775|colabs=3
PARCIAL|10/2026|CADASTRO|dias=10|horas=38.355213611111104|colabs=3
PARCIAL|10/2026|BATIDA|dias=19|horas=108.81011656222222|colabs=12
PARCIAL_TOTAL|10/2026|dias=39|horas=156.281613991111099|colabs=16
ESTIMATIVA processados 125 universo 206 minutos_total 2.8 ate_40_min True ate_15_min True
ESTIMATIVA processados 150 universo 206 minutos_total 2.9 ate_40_min True ate_15_min True
ESTIMATIVA processados 175 universo 206 minutos_total 3.0 ate_40_min True ate_15_min True
ESTIMATIVA processados 200 universo 206 minutos_total 3.0 ate_40_min True ate_15_min True
COMPETENCIA 11/2026
[1] {'dias_comparados': 603, 'bate_ate_10min': 568, 'esp_zero_e6_trabalho': 11, 'e6_zero_esp_trabalho': 5, 'diverge_10_60': 1, 'diverge_acima_60': 18, 'dia_batida_impar': 34, 'impar_que_BATE': 14, 'impar_que_DIVERGE': 20, 'impar_dentro_da_faixa': 13, 'impar_FORA_da_faixa': 7, 'impar_no_conjunto_do_dia': 20, 'impar_SO_POR_CORTE_do_oraculo': 0, 'dia_sem_linha_no_espelho': 1344, 'dia_sem_lavratura_no_espelho': 805, 'dia_isento_ou_ausencia': 174, 'dia_sem_trabalho_ambos': 33, 'esp_desconta_janela_declarada': 17, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 94.2% de 603 dias comparados
[3] ESTRUTURA     1 dia-colab       6.2 h    1 colabs
[3] CADASTRO     20 dia-colab     106.7 h    7 colabs
[3] BATIDA       14 dia-colab      81.8 h   10 colabs
[4] TOTAL divergentes 35
[5] colabs com divergencia: 15
erros_no_espelho 0
PARCIAL|11/2026|ESTRUTURA|dias=1|horas=6.2|colabs=1
PARCIAL|11/2026|CADASTRO|dias=20|horas=106.69305642305555|colabs=7
PARCIAL|11/2026|BATIDA|dias=14|horas=81.76095535388889|colabs=10
PARCIAL_TOTAL|11/2026|dias=35|horas=194.65401177694444|colabs=15
TEMPO_SEGUNDOS 178.0
TRATADA_LOCAL app/escala/alimentacao.py:279 GeneratorExit 232873
TRATADA_LOCAL app/escala/models.py:1282 GeneratorExit 214
TRATADA_LOCAL app/escala/services/fase_12x36.py:114 GeneratorExit 945
TRATADA_LOCAL app/escala/services/leitor_celula.py:25 ValueError 5201
TRATADA_LOCAL app/escala/services/leitor_celula.py:81 GeneratorExit 57
TRATADA_LOCAL app/escala/utils.py:1521 GeneratorExit 2
TRATADA_LOCAL app/escala/utils.py:2012 GeneratorExit 5766
TRATADA_LOCAL app/escala/utils.py:2054 GeneratorExit 3
TRATADA_LOCAL app/escala/utils.py:452 GeneratorExit 4
TRATADA_LOCAL app/escala/utils.py:524 GeneratorExit 3444
TRATADA_LOCAL app/escala/utils.py:525 GeneratorExit 3444
TRATADA_LOCAL app/escala/utils.py:888 GeneratorExit 649
TRATADA_LOCAL app/folha/causas.py:58 GeneratorExit 180
TRATADA_LOCAL app/ponto/juiz_batida.py:75 ValueError 433065
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:1131 GeneratorExit 505
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:2604 GeneratorExit 2567
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:677 GeneratorExit 161
TRATADA_LOCAL app/ponto/services/dia_pago.py:890 GeneratorExit 10520
TRATADA_LOCAL app/ponto/services/espelho.py:473 GeneratorExit 104
TRATADA_LOCAL app/ponto/services/espelho.py:657 GeneratorExit 3607
TRATADA_LOCAL app/ponto/services/fechamento.py:1028 GeneratorExit 191
TRATADA_LOCAL app/ponto/services/fechamento.py:1029 _GradeSemCelula 5
TRATADA_LOCAL app/ponto/turnos.py:1293 GeneratorExit 20459
TRATADA_LOCAL app/ponto/turnos.py:1654 GeneratorExit 4354
TRATADA_LOCAL app/ponto/turnos.py:1697 GeneratorExit 33979
TRATADA_LOCAL app/ponto/turnos.py:2639 GeneratorExit 8
TRATADA_LOCAL app/ponto/turnos.py:677 GeneratorExit 14862
SOMA 10/2026 39   <- tem de ser igual a soma das classes (39) True
SOMA 10/2026 HORAS 156.281613991111099 True
SOMA 10/2026 COLABS_UNIAO 16 True
SOMA 11/2026 35   <- tem de ser igual a soma das classes (35) True
SOMA 11/2026 HORAS 194.65401177694444 True
SOMA 11/2026 COLABS_UNIAO 15 True
rc_sombra=0
```

As seis linhas `SOMA` da parte estão `True`. [nao verificado]

Não há ocorrência de `INCOMPLETO` na saída fornecida. [nao verificado]

## 06/11 13:23 · sessao 8e833b06368f · E-SEM-VIVA-B · verificador · origem=Code

**5a está curado; 5b cura o M8 específico, mas mantém sobreviventes e introduz falsos VERMELHOS.** (`app/escala/services/cadastro_realidade.py:135–137`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:190–207`.)

1. **5a — informativa, procedente: docstring corrigida.** Agora distingue uma consulta **de células por empresa** das consultas adicionais do retentor: duas competências, cada uma carregando células e vínculos, até quatro adicionais por desligado elegível. Isso não representa o total geral: existe também a seleção dos desligados. (`app/escala/services/cadastro_realidade.py:135–137,165–172`; `app/folha/export.py:1206–1212`; `app/escala/alimentacao.py:148–154,185–186`.)

2. **5b — informativa, procedente: cura específica confirmada.** O selo agora detecta os quatro campos quando escritos como literais completos: `getattr(c, 'data_demissao')`, subscrição e `.only('data_demissao')`, por exemplo. Também deixa de aceitar como testemunha única uma chamada sob `if False`, `if 0` ou outra constante falsa. A nova costura exige justamente a rejeição dessa combinação. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:165,190–198,218–228`.)

   As duas costuras passaram executando os métodos reais em memória, com base `unittest.TestCase`. Suíte Django completa e reprodução dos logs M8 anexados: **[não verificado]**. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:209–228`.)

3. **Média — procedente: derivação paralela relevante ainda passa pelo selo.** Na fonte produtiva, substituir a chamada de rescisão por este predicado e acrescentar a chamada morta abaixo mantém o selo **VERDE**, confirmado em memória: [nao verificado]

   ```python
   if 0 == 1:
       sai_pela_rescisao(None, None)

   # No lugar de sai_pela_rescisao(colab, fim):
   getattr(colab, "data_" + "demissao") is not None and \
       getattr(colab, "data_" + "demissao") <= fim
   ```

   O campo completo não aparece como `Constant`; a condição morta é `Compare`, não `Constant`. A cópia reproduz a autoridade atual, portanto equivalência comportamental hoje não demonstra ausência de derivação paralela. Sobrevivência na suíte completa: **[não verificado]**. (`app/escala/services/cadastro_realidade.py:171`; `app/folha/export.py:60`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:187–207`.)

   Também continua possível executar a autoridade, descartar seu retorno e decidir por uma cópia: o selo verifica presença da chamada, sem verificar seu uso. Isso vale para rescisão e retentor. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:187–207`.)

4. **Média — procedente: falsos VERMELHOS introduzidos pelo endurecimento.**

   - **Texto legítimo:** `logger.debug("campo: %s", "data_demissao")` reprova sem ler campo algum. Uma docstring cujo conteúdo inteiro seja `data_demissao` também reprova. A comparação usa igualdade do literal inteiro: uma frase maior contendo esse nome passa. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:192,206`.)
   - **Chamada viva apagada do conjunto:** acrescentar `if False: sai_pela_rescisao(...)`, mantendo a chamada produtiva, reprova. O `discard` remove a identidade da função globalmente, sem distinguir ocorrências. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:186–198`.)
   - **`else` vivo tratado como morto:** `ast.walk(n)` percorre também o `else`; assim, `if False: pass; else: autoridade(...)` perde uma chamada executável. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:195–198`.)

   **A fonte produtiva atual passa**, confirmado em memória. Sua docstring é uma constante longa, diferente dos quatro nomes proibidos; comentários não entram nessa coleta AST. (`app/escala/services/cadastro_realidade.py:132–151,171–172`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:190–207`.)

5. **Item 3 — média, mecanismo procedente; justificativa do descarte insuficiente.** Uma exceção no retentor interrompe o mapa antes do retorno; `_nunca_bateu()` não entrega resultado e `lista()` captura antes de incorporar qualquer C5, inclusive dos ativos. A lista continua após registrar a exceção. (`app/escala/services/cadastro_realidade.py:171–183,813–819`.)

   O comentário “falha ALTA” citado trata especificamente de valores inesperadamente nulos nos campos do fechamento, não da disponibilidade da C5. Portanto, **descartar como regressão deste delta documental/teste procede; descartar a ressalva operacional por essa justificativa não procede**. Ocorrência concreta: **[não verificado]**. (`app/folha/export.py:816–825`; `app/escala/services/cadastro_realidade.py:813–819`; anexo `v3b.diff`.)

6. **Item 2 — informativa, descartes procedem dentro do recorte declarado.** “Até ontem” está no contrato e no filtro da C5; o ramo acusador do retentor não impõe esse teto. A equivalência exige essa interseção temporal, sem configurar regressão nova. (`app/escala/services/cadastro_realidade.py:132,160–167`; `app/folha/export.py:1225–1255`.)

   O classificador inteiro responde uma pergunta mais ampla: usa universo de fechamentos e pode sair por código ausente, férias com batida ou cadastro zero antes do retentor. A C5 declara consultar especificamente rescisão + retentor. **O descarte de escopo procede; igualdade com todo `classificar_export` continua sem demonstração.** (`app/escala/services/cadastro_realidade.py:142–147,171–172`; `app/folha/export.py:775–780,858–876,951–960`; `app/core/juizes.py:288–299`.)

7. **Escritores, leitores e classes — informativa, procedente.** O delta não acrescenta escritor nem leitor de estado produtivo: altera documentação e a inspeção de fonte nos testes. O novo leitor é `_veredito`, que passa a examinar constantes e ramos; ele modifica apenas os conjuntos locais `campos` e `chamadas`, consumidos pelas asserções. (`app/escala/services/cadastro_realidade.py:135–137`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:179–207,218–228`; anexo `v3b.diff`.)

   **Segundo juiz, segundo escritor e derivação paralela produtiva: ausentes no delta.** A decisão produtiva continua usando as duas autoridades; o fallback de relógio permanece anterior ao delta. **Selo totalmente sem mordida: não; garantia incompleta e falsos VERMELHOS: sim**, conforme os achados acima. (`app/escala/services/cadastro_realidade.py:160,171–172`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:190–228`; anexo `v3b.diff`.)

## 06/11 13:22 · sessao 386f7d06277f · - · - · origem=Ronald (tmux)

Achados confirmados por execução, em memória, da classe real com `unittest.TestCase` no lugar da base Django; suíte Django e produção: [não verificado]. [nao verificado]

- **Informativa — procedente: M8 específico está morto.** A leitura `getattr(c, "data_demissao")` agora aparece em `campos`, e `if False: sai_pela_rescisao(...)` deixa de satisfazer a chamada exigida. As duas costuras atuais passaram na sonda. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:190–207,218–228`.)

- **Média — procedente: sobrevivente de derivação paralela.** Substituí, apenas na string em memória, `not sai_pela_rescisao(colab, fim)` por `not (getattr(colab, 'data_' + 'demissao') is not None and getattr(colab, 'data_' + 'demissao') <= fim)` e acrescentei `if 1 == 0: sai_pela_rescisao(None, None)`. O selo ficou **VERDE**: o campo completo não é um `Constant`, e `Compare` não é o teste `Constant` eliminado. A cópia reproduz o predicado atual da autoridade; sobrevivência na suíte completa: [não verificado]. (`app/escala/services/cadastro_realidade.py:171`; `app/folha/export.py:60`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:192,195–207`.)

- **Média — procedente: uso do retorno continua desprotegido.** Outro mutante verde executa a autoridade, descarta seu resultado e usa a cópia: `not (sai_pela_rescisao(colab, fim), (getattr(colab, 'data_' + 'demissao') is not None and getattr(colab, 'data_' + 'demissao') <= fim))[1]`. A coleta exige presença de chamadas, sem verificar o valor usado. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:187–207`; `app/folha/export.py:60`.)

- **Média — procedente: falso VERMELHO para texto legítimo.** `logger.debug('data_demissao')` ficou vermelho sem ler campo algum; docstring cujo conteúdo inteiro seja `'data_demissao'` também. Comentário `# data_demissao`, docstring `'campo data_demissao'` e log com essa frase maior ficaram verdes: a comparação usa igualdade do valor inteiro de cada `Constant`, sem contexto de leitura. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:192,206`.)

- **Média — procedente: remoção global e varredura do `else` dão falso VERMELHO.** Acrescentar uma chamada morta homônima, mantendo intacta a chamada produtiva, remove ambas do conjunto; `if False: pass; else: sai_pela_rescisao(...)` também perde a chamada viva. O algoritmo percorre o `If` inteiro, incluindo `orelse`, e faz `discard` pela identidade da função, sem contar ocorrências. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:186–198`.)

- **Informativa — procedente: a fonte produtiva atual passa.** A sonda retornou ambas as autoridades presentes, `campos=set()` e `nomes=set()`. Sua docstring é um único texto maior, não uma constante igual aos quatro campos proibidos; as chamadas estão no fluxo produtivo da compreensão. (`app/escala/services/cadastro_realidade.py:132–151,171–172`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:165–166,192,202–207`.)

## 06/11 13:21 · sessao 1916fbd0fdff · - · - · origem=Ronald (tmux)

- **5a — informativa, curado com precisão de escopo.** A docstring agora declara uma consulta **de células** por empresa e até quatro adicionais **do retentor por desligado**. São duas competências, cada uma com consulta de células e vínculos; há ainda consulta de empresas e lote dos desligados, portanto não representa o total geral. (`app/escala/services/cadastro_realidade.py:135–137,162–172`; `app/folha/export.py:1206–1212`; `app/escala/alimentacao.py:148–154,163–186`.)

- **Item 3 — média, mecanismo procedente; justificativa do descarte insuficiente.** Exceção em um retentor interrompe a construção do mapa; `_nunca_bateu` não retorna e `lista()` captura antes de incorporar qualquer C5, inclusive dos ativos. O código registra a exceção e continua a lista. (`app/escala/services/cadastro_realidade.py:171–183,813–819`.) A “falha ALTA” citada refere-se especificamente a `None` em campos de fechamento considerados não nulos e à comparação na trava de aprovação; não estabelece política geral para C5. (`app/folha/export.py:816–825`.) O descarte procede **como ausência de regressão neste delta**, restrito a docstring/teste segundo o anexo; não elimina a ressalva factual. Ocorrência concreta: **[não verificado]**.

- **Item 2 — informativa, descarte como defeito novo procede.** “Até ontem” consta no contrato e no filtro SQL; o ramo acusador do retentor não possui essa restrição, logo equivalência exige o recorte temporal declarado. (`app/escala/services/cadastro_realidade.py:132,160–167`; `app/folha/export.py:1225–1255`.) A C5 consulta efetivamente rescisão e retentor, sem copiar seus predicados. (`app/escala/services/cadastro_realidade.py:171–172`.)

- **Classificador inteiro — informativa, fora do recorte declarado, sem igualdade demonstrada.** O contrato explicita as duas perguntas consultadas. (`app/escala/services/cadastro_realidade.py:142–147`.) `classificar_export` tem universo de fechamentos e saídas anteriores ao retentor por código ausente, férias com batida e cadastro zero; portanto não pode ser tratado como sinônimo da composição restrita. (`app/folha/export.py:775–780,858–876,951–960`.) JUIZES confirma `classificar_export` como autoridade para **quem entra no TXT**, pergunta mais ampla. (`app/docs/JUIZES.md:74–77`.)

## 06/11 13:19 · sessao 02aea6a7e4ff · R1-CAUSAD-V3 · sonda · origem=Code

As linhas `SOMA` desta parte não são `True`: todas indicam `False`. Há `INCOMPLETO: 594`. [nao verificado]

Transcrição integral do dado fornecido, sem nova execução da sonda. [nao verificado]

```text
sonda_leitura: script.py [fda4c3378d0e47ce] no cpuset 4-7, lendo sombra como leitor (LEITURA)

PARTE|6|6
ATO|hoje=2026-11-06|agora=2026-11-06T13:12:10.739545-03:00
FORA_DA_MEDIDA|horas_falta
FORA_DA_MEDIDA|horas_reflexo_dsr
FORA_DA_MEDIDA|saldo_banco_horas
UNIVERSO|2026-10|colabs_porta=624|segundos=0.1
UNIVERSO_PARTE 2026-10 100
RUBRICA|competencia|rubrica|v2_h|v3_h|delta_h|colabs_div|dias_colab_div
DONO|competencia|rubrica|dono|v2_h|v3_h|delta_h|colabs|dias_colab
DIA|2026-10|horas_extras_100_noturna|col191|2026-10-12|CADASTRO|0.000000000|1.000000000|+1.000000000
DIA|2026-10|horas_noturnas|col245|2026-09-21|CADASTRO|1.867681376|2.134493001|+0.266811625
DIA|2026-10|horas_noturnas|col245|2026-09-23|CADASTRO|1.869965836|2.137103813|+0.267137977
DIA|2026-10|horas_noturnas|col245|2026-09-24|ESTRUTURA|1.851872500|2.116425714|+0.264553214
DIA|2026-10|horas_noturnas|col245|2026-09-25|CADASTRO|1.863168056|2.129334921|+0.266166865
DIA|2026-10|horas_noturnas|col245|2026-09-26|ESTRUTURA|1.915446667|2.189081905|+0.273635238
DIA|2026-10|horas_noturnas|col245|2026-09-27|CADASTRO|1.885175556|2.154486349|+0.269310794
DIA|2026-10|horas_noturnas|col245|2026-09-28|ESTRUTURA|1.942230278|2.219691746|+0.277461468
DIA|2026-10|horas_noturnas|col245|2026-09-30|ESTRUTURA|1.956297778|2.235768889|+0.279471111
DIA|2026-10|horas_trabalhadas|col599|2026-10-20|CADASTRO|10.100982500|5.572917222|-4.528065278
DIA|2026-10|horas_intra_indenizada|col599|2026-09-22|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-10|horas_intra_indenizada|col599|2026-09-24|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-10|horas_intra_indenizada|col599|2026-09-26|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-10|horas_intra_indenizada|col599|2026-09-28|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-10|horas_intra_indenizada|col599|2026-09-30|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-10|horas_intra_indenizada|col599|2026-10-02|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-10|horas_intra_indenizada|col599|2026-10-06|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-10|horas_extras_50|col599|2026-10-16|CADASTRO|0.000000000|0.224154212|+0.224154212
DIA|2026-10|horas_intra_indenizada|col941|2026-10-01|CADASTRO|1.000000000|0.000000000|-1.000000000
PARCIAL|2026-10|horas_trabalhadas|v2_h=12120.982897860556|v3_h=12116.454832582778|colabs_div=1|dias_colab_div=1
RUBRICA|2026-10|horas_trabalhadas|12120.982897861|12116.454832583|-4.528065278|1|1
DONO|2026-10|horas_trabalhadas|CADASTRO|10.100982500|5.572917222|-4.528065278|1|1
RESIDUO|2026-10|horas_trabalhadas|ATE_LIMIAR|12110.881915361|12110.881915361|+0.000000000|84|1295
RESIDUO|2026-10|horas_trabalhadas|SEM_DECISAO|55.446929781|55.446929781|+0.000000000|53|231
RESIDUO|2026-10|horas_trabalhadas|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|181
INSTRUMENTO_HISTORICO|2026-10|horas_trabalhadas|v2_h=12092.499011373|v3_h=12116.454832583|delta_h=+23.955821209
PARCIAL|2026-10|horas_intra_indenizada|v2_h=445.77514605361114|v3_h=437.77514605361114|colabs_div=2|dias_colab_div=8
RUBRICA|2026-10|horas_intra_indenizada|445.775146054|437.775146054|-8.000000000|2|8
DONO|2026-10|horas_intra_indenizada|CADASTRO|8.000000000|0.000000000|-8.000000000|2|8
RESIDUO|2026-10|horas_intra_indenizada|ATE_LIMIAR|437.775146054|437.775146054|+0.000000000|84|1288
RESIDUO|2026-10|horas_intra_indenizada|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|53|231
RESIDUO|2026-10|horas_intra_indenizada|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|181
INSTRUMENTO_HISTORICO|2026-10|horas_intra_indenizada|v2_h=445.775146054|v3_h=437.775146054|delta_h=-8.000000000
PARCIAL|2026-10|horas_extras_50|v2_h=50.156459266388886|v3_h=50.38061347888889|colabs_div=1|dias_colab_div=1
RUBRICA|2026-10|horas_extras_50|50.156459266|50.380613479|+0.224154213|1|1
DONO|2026-10|horas_extras_50|CADASTRO|0.000000000|0.224154212|+0.224154212|1|1
RESIDUO|2026-10|horas_extras_50|ATE_LIMIAR|50.156459266|50.156459266|+0.000000000|84|1271
RESIDUO|2026-10|horas_extras_50|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|55|255
RESIDUO|2026-10|horas_extras_50|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|181
INSTRUMENTO_HISTORICO|2026-10|horas_extras_50|v2_h=50.156459266|v3_h=50.380613479|delta_h=+0.224154213
PARCIAL|2026-10|horas_extras_100|v2_h=85.194816959722218|v3_h=85.194816959722218|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_extras_100|85.194816960|85.194816960|+0.000000000|0|0
RESIDUO|2026-10|horas_extras_100|ATE_LIMIAR|85.194816960|85.194816960|+0.000000000|84|1272
RESIDUO|2026-10|horas_extras_100|SEM_DECISAO|3.430878889|3.430878889|+0.000000000|55|255
RESIDUO|2026-10|horas_extras_100|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|181
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100|v2_h=88.625695849|v3_h=95.626922793|delta_h=+7.001226944
PARCIAL|2026-10|horas_extras_100_feriado|v2_h=82.29819010833333|v3_h=82.29819010833333|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_extras_100_feriado|82.298190108|82.298190108|+0.000000000|0|0
RESIDUO|2026-10|horas_extras_100_feriado|ATE_LIMIAR|82.298190108|82.298190108|-0.000000000|84|1272
RESIDUO|2026-10|horas_extras_100_feriado|SEM_DECISAO|3.430878889|3.430878889|+0.000000000|55|255
RESIDUO|2026-10|horas_extras_100_feriado|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|181
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100_feriado|v2_h=0.000000000|v3_h=92.730295942|delta_h=+92.730295942
PARCIAL|2026-10|horas_extras_50_noturna|v2_h=0|v3_h=0|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_extras_50_noturna|0.000000000|0.000000000|+0.000000000|0|0
RESIDUO|2026-10|horas_extras_50_noturna|ATE_LIMIAR|0.000000000|0.000000000|+0.000000000|84|1272
RESIDUO|2026-10|horas_extras_50_noturna|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|55|255
RESIDUO|2026-10|horas_extras_50_noturna|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|181
INSTRUMENTO_HISTORICO|2026-10|horas_extras_50_noturna|v2_h=0.000000000|v3_h=0.000000000|delta_h=+0.000000000
PARCIAL|2026-10|horas_extras_100_noturna|v2_h=11.499323055555555|v3_h=12.499323055555555|colabs_div=1|dias_colab_div=1
RUBRICA|2026-10|horas_extras_100_noturna|11.499323056|12.499323056|+1.000000000|1|1
DONO|2026-10|horas_extras_100_noturna|CADASTRO|0.000000000|1.000000000|+1.000000000|1|1
RESIDUO|2026-10|horas_extras_100_noturna|ATE_LIMIAR|11.499323056|11.499323056|+0.000000000|84|1271
RESIDUO|2026-10|horas_extras_100_noturna|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|55|255
RESIDUO|2026-10|horas_extras_100_noturna|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|181
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100_noturna|v2_h=0.000000000|v3_h=12.499323056|delta_h=+12.499323056
PARCIAL|2026-10|horas_folga_trabalhada|v2_h=0|v3_h=0|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_folga_trabalhada|0.000000000|0.000000000|+0.000000000|0|0
RESIDUO|2026-10|horas_folga_trabalhada|ATE_LIMIAR|0.000000000|0.000000000|+0.000000000|84|1296
RESIDUO|2026-10|horas_folga_trabalhada|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|53|231
RESIDUO|2026-10|horas_folga_trabalhada|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|181
INSTRUMENTO_HISTORICO|2026-10|horas_folga_trabalhada|v2_h=83.930816268|v3_h=0.000000000|delta_h=-83.930816268
PARCIAL|2026-10|horas_noturnas|v2_h=2938.3726663505554|v3_h=2940.5372146426985|colabs_div=1|dias_colab_div=8
RUBRICA|2026-10|horas_noturnas|2938.372666351|2940.537214643|+2.164548292|1|8
DONO|2026-10|horas_noturnas|CADASTRO|7.485990823|8.555418083|+1.069427260|1|4
DONO|2026-10|horas_noturnas|ESTRUTURA|7.665847222|8.760968254|+1.095121032|1|4
RESIDUO|2026-10|horas_noturnas|ATE_LIMIAR|2923.220828306|2923.220828306|-0.000000000|84|1275
RESIDUO|2026-10|horas_noturnas|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|56|244
RESIDUO|2026-10|horas_noturnas|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|181
INSTRUMENTO_HISTORICO|2026-10|horas_noturnas|v2_h=2926.418086596|v3_h=2948.309629008|delta_h=+21.891542412
PARCIAL|2026-10|horas_atraso|v2_h=53.192499999999995|v3_h=53.192125908055552|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_atraso|53.192500000|53.192125908|-0.000374092|0|0
RESIDUO|2026-10|horas_atraso|ATE_LIMIAR|53.192500000|53.192125908|-0.000374092|83|1263
RESIDUO|2026-10|horas_atraso|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|56|264
RESIDUO|2026-10|horas_atraso|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|181
INSTRUMENTO_HISTORICO|2026-10|horas_atraso|v2_h=52.163666667|v3_h=52.163459241|delta_h=-0.000207425
PARCIAL|2026-10|horas_saida_antecipada|v2_h=56.545666666666669|v3_h=56.545736652222224|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_saida_antecipada|56.545666667|56.545736652|+0.000069986|0|0
RESIDUO|2026-10|horas_saida_antecipada|ATE_LIMIAR|56.545666667|56.545736652|+0.000069986|83|1263
RESIDUO|2026-10|horas_saida_antecipada|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|56|264
RESIDUO|2026-10|horas_saida_antecipada|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|181
INSTRUMENTO_HISTORICO|2026-10|horas_saida_antecipada|v2_h=53.068166667|v3_h=53.068318597|delta_h=+0.000151930
UNIVERSO|2026-11|colabs_porta=578|segundos=97.7
UNIVERSO_PARTE 2026-11 96
RUBRICA|competencia|rubrica|v2_h|v3_h|delta_h|colabs_div|dias_colab_div
DONO|competencia|rubrica|dono|v2_h|v3_h|delta_h|colabs|dias_colab
DIA|2026-11|horas_intra_indenizada|col599|2026-10-22|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-11|horas_intra_indenizada|col599|2026-10-28|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-11|horas_intra_indenizada|col707|2026-10-29|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-11|horas_intra_indenizada|col707|2026-10-31|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-11|horas_intra_indenizada|col707|2026-11-02|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-11|horas_intra_indenizada|col707|2026-11-04|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-11|horas_extras_50|col707|2026-10-31|CADASTRO|0.000000000|0.187750000|+0.187750000
DIA|2026-11|horas_extras_50|col707|2026-11-02|CADASTRO|0.000000000|0.193095000|+0.193095000
DIA|2026-11|horas_extras_100_noturna|col923|2026-11-02|CADASTRO|2.916764722|4.778043611|+1.861278889
DIA|2026-11|horas_saida_antecipada|col1013|2026-11-05|ESTRUTURA|0.000000000|4.331833611|+4.331833611
PARCIAL|2026-11|horas_trabalhadas|v2_h=6235.4115330605555|v3_h=6235.4115330605555|colabs_div=0|dias_colab_div=0
RUBRICA|2026-11|horas_trabalhadas|6235.411533061|6235.411533061|+0.000000000|0|0
RESIDUO|2026-11|horas_trabalhadas|ATE_LIMIAR|6235.411533061|6235.411533061|+0.000000000|80|675
RESIDUO|2026-11|horas_trabalhadas|SEM_DECISAO|57.838073125|57.838073125|+0.000000000|79|889
RESIDUO|2026-11|horas_trabalhadas|SEM_PAR|0.000000000|0.000000000|+0.000000000|13|223
INSTRUMENTO_HISTORICO|2026-11|horas_trabalhadas|v2_h=6219.295985835|v3_h=6235.411533061|delta_h=+16.115547226
PARCIAL|2026-11|horas_intra_indenizada|v2_h=224.49000823722221|v3_h=218.49000823722221|colabs_div=2|dias_colab_div=6
RUBRICA|2026-11|horas_intra_indenizada|224.490008237|218.490008237|-6.000000000|2|6
DONO|2026-11|horas_intra_indenizada|CADASTRO|6.000000000|0.000000000|-6.000000000|2|6
RESIDUO|2026-11|horas_intra_indenizada|ATE_LIMIAR|218.490008237|218.490008237|+0.000000000|80|669
RESIDUO|2026-11|horas_intra_indenizada|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|79|889
RESIDUO|2026-11|horas_intra_indenizada|SEM_PAR|0.000000000|0.000000000|+0.000000000|13|223
INSTRUMENTO_HISTORICO|2026-11|horas_intra_indenizada|v2_h=224.311379904|v3_h=218.490008237|delta_h=-5.821371667
PARCIAL|2026-11|horas_extras_50|v2_h=13.622823055555555|v3_h=14.003668055555556|colabs_div=1|dias_colab_div=2
RUBRICA|2026-11|horas_extras_50|13.622823056|14.003668056|+0.380845000|1|2
DONO|2026-11|horas_extras_50|CADASTRO|0.000000000|0.380845000|+0.380845000|1|2
RESIDUO|2026-11|horas_extras_50|ATE_LIMIAR|13.622823056|13.622823056|+0.000000000|80|656
RESIDUO|2026-11|horas_extras_50|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|79|906
RESIDUO|2026-11|horas_extras_50|SEM_PAR|0.000000000|0.000000000|+0.000000000|13|223
INSTRUMENTO_HISTORICO|2026-11|horas_extras_50|v2_h=13.622823056|v3_h=14.003668056|delta_h=+0.380845000
PARCIAL|2026-11|horas_extras_100|v2_h=66.021159506111104|v3_h=66.021159506111104|colabs_div=0|dias_colab_div=0
RUBRICA|2026-11|horas_extras_100|66.021159506|66.021159506|+0.000000000|0|0
RESIDUO|2026-11|horas_extras_100|ATE_LIMIAR|66.021159506|66.021159506|+0.000000000|80|658
RESIDUO|2026-11|horas_extras_100|SEM_DECISAO|3.152894444|3.152894444|+0.000000000|79|906
RESIDUO|2026-11|horas_extras_100|SEM_PAR|0.000000000|0.000000000|+0.000000000|13|223
INSTRUMENTO_HISTORICO|2026-11|horas_extras_100|v2_h=69.174053951|v3_h=66.021159506|delta_h=-3.152894444
PARCIAL|2026-11|horas_extras_100_feriado|v2_h=65.215916172777781|v3_h=65.215916172777781|colabs_div=0|dias_colab_div=0
RUBRICA|2026-11|horas_extras_100_feriado|65.215916173|65.215916173|+0.000000000|0|0
RESIDUO|2026-11|horas_extras_100_feriado|ATE_LIMIAR|65.215916173|65.215916173|+0.000000000|80|658
RESIDUO|2026-11|horas_extras_100_feriado|SEM_DECISAO|3.152894444|3.152894444|+0.000000000|79|906
RESIDUO|2026-11|horas_extras_100_feriado|SEM_PAR|0.000000000|0.000000000|+0.000000000|13|223
INSTRUMENTO_HISTORICO|2026-11|horas_extras_100_feriado|v2_h=0.000000000|v3_h=65.215916173|delta_h=+65.215916173
PARCIAL|2026-11|horas_extras_50_noturna|v2_h=0|v3_h=0|colabs_div=0|dias_colab_div=0
RUBRICA|2026-11|horas_extras_50_noturna|0.000000000|0.000000000|+0.000000000|0|0
RESIDUO|2026-11|horas_extras_50_noturna|ATE_LIMIAR|0.000000000|0.000000000|+0.000000000|80|658
RESIDUO|2026-11|horas_extras_50_noturna|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|79|906
RESIDUO|2026-11|horas_extras_50_noturna|SEM_PAR|0.000000000|0.000000000|+0.000000000|13|223
INSTRUMENTO_HISTORICO|2026-11|horas_extras_50_noturna|v2_h=0.000000000|v3_h=0.000000000|delta_h=+0.000000000
PARCIAL|2026-11|horas_extras_100_noturna|v2_h=9.8086850000000005|v3_h=11.669963888888889|colabs_div=1|dias_colab_div=1
RUBRICA|2026-11|horas_extras_100_noturna|9.808685000|11.669963889|+1.861278889|1|1
DONO|2026-11|horas_extras_100_noturna|CADASTRO|2.916764722|4.778043611|+1.861278889|1|1
RESIDUO|2026-11|horas_extras_100_noturna|ATE_LIMIAR|6.891920278|6.891920278|+0.000000000|80|657
RESIDUO|2026-11|horas_extras_100_noturna|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|79|906
RESIDUO|2026-11|horas_extras_100_noturna|SEM_PAR|0.000000000|0.000000000|+0.000000000|13|223
INSTRUMENTO_HISTORICO|2026-11|horas_extras_100_noturna|v2_h=0.000000000|v3_h=11.669963889|delta_h=+11.669963889
PARCIAL|2026-11|horas_folga_trabalhada|v2_h=0|v3_h=0|colabs_div=0|dias_colab_div=0
RUBRICA|2026-11|horas_folga_trabalhada|0.000000000|0.000000000|+0.000000000|0|0
RESIDUO|2026-11|horas_folga_trabalhada|ATE_LIMIAR|0.000000000|0.000000000|+0.000000000|80|675
RESIDUO|2026-11|horas_folga_trabalhada|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|79|889
RESIDUO|2026-11|horas_folga_trabalhada|SEM_PAR|0.000000000|0.000000000|+0.000000000|13|223
INSTRUMENTO_HISTORICO|2026-11|horas_folga_trabalhada|v2_h=73.953620351|v3_h=0.000000000|delta_h=-73.953620351
PARCIAL|2026-11|horas_noturnas|v2_h=1424.117478798373|v3_h=1424.117478798373|colabs_div=0|dias_colab_div=0
RUBRICA|2026-11|horas_noturnas|1424.117478798|1424.117478798|+0.000000000|0|0
RESIDUO|2026-11|horas_noturnas|ATE_LIMIAR|1424.117478798|1424.117478798|-0.000000000|80|667
RESIDUO|2026-11|horas_noturnas|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|80|897
RESIDUO|2026-11|horas_noturnas|SEM_PAR|0.000000000|0.000000000|+0.000000000|13|223
INSTRUMENTO_HISTORICO|2026-11|horas_noturnas|v2_h=1424.117478798|v3_h=1424.117478798|delta_h=+0.000000000
PARCIAL|2026-11|horas_atraso|v2_h=26.881499999999999|v3_h=26.88139402583333|colabs_div=0|dias_colab_div=0
RUBRICA|2026-11|horas_atraso|26.881500000|26.881394026|-0.000105974|0|0
RESIDUO|2026-11|horas_atraso|ATE_LIMIAR|26.881500000|26.881394026|-0.000105974|79|655
RESIDUO|2026-11|horas_atraso|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|80|909
RESIDUO|2026-11|horas_atraso|SEM_PAR|0.000000000|0.000000000|+0.000000000|13|223
INSTRUMENTO_HISTORICO|2026-11|horas_atraso|v2_h=26.881500000|v3_h=26.881394026|delta_h=-0.000105974
PARCIAL|2026-11|horas_saida_antecipada|v2_h=31.359500000000001|v3_h=35.691191891666662|colabs_div=1|dias_colab_div=1
RUBRICA|2026-11|horas_saida_antecipada|31.359500000|35.691191892|+4.331691892|1|1
DONO|2026-11|horas_saida_antecipada|ESTRUTURA|0.000000000|4.331833611|+4.331833611|1|1
RESIDUO|2026-11|horas_saida_antecipada|ATE_LIMIAR|31.359500000|31.359358281|-0.000141719|78|654
RESIDUO|2026-11|horas_saida_antecipada|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|80|909
RESIDUO|2026-11|horas_saida_antecipada|SEM_PAR|0.000000000|0.000000000|+0.000000000|13|223
INSTRUMENTO_HISTORICO|2026-11|horas_saida_antecipada|v2_h=31.359500000|v3_h=35.691191892|delta_h=+4.331691892
ERRO 2026-10 app/ponto/services/dia_pago.py:315 ValueError 13 exemplos=[('col41', '-'), ('col53', '-'), ('col83', '-'), ('col209', '-'), ('col257', '-')]
ERRO 2026-10 sonda ConservacaoNaoFecha 78 exemplos=[('col143', '-'), ('col245', '-'), ('col437', '-'), ('col641', '-'), ('col659', '-')]
ERRO 2026-10 sonda LacunaDeMedicao 210 exemplos=[('col41', '-'), ('col41', '2026-09-21'), ('col41', '2026-09-22'), ('col41', '2026-09-23'), ('col41', '2026-09-24')]
ERRO 2026-11 app/ponto/services/dia_pago.py:315 ValueError 15 exemplos=[('col41', '-'), ('col53', '-'), ('col83', '-'), ('col119', '-'), ('col209', '-')]
ERRO 2026-11 sonda ConservacaoNaoFecha 34 exemplos=[('col641', '-'), ('col677', '-'), ('col899', '-'), ('col929', '-')]
ERRO 2026-11 sonda LacunaDeMedicao 244 exemplos=[('col41', '-'), ('col41', '2026-10-21'), ('col41', '2026-10-22'), ('col41', '2026-10-23'), ('col41', '2026-10-24')]
TRATADA_LOCAL app/escala/services/leitor_celula.py:25 ValueError 1881
TRATADA_LOCAL app/ponto/juiz_batida.py:75 ValueError 146215
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:1075 AttributeError 3819
TRATADA_LOCAL app/ponto/services/fechamento.py:1029 _GradeSemCelula 4
ERROS_POR_TIPO ConservacaoNaoFecha 112
ERROS_POR_TIPO LacunaDeMedicao 454
ERROS_POR_TIPO ValueError 28
SOMA COBERTURA|2026-10|False
SOMA PARTES_EMISSOR|2026-10|horas_trabalhadas|v2=False|v3=False
SOMA DIVERGENCIA|2026-10|horas_trabalhadas|partes_h=-4.528065278|autoridade_menos_residuo_h=-4.528065278|False
SOMA COLABS_UNIAO_DIAS|2026-10|horas_trabalhadas|colabs=False|dias=False
SOMA V3_PORTA_EMISSOR|2026-10|horas_trabalhadas|False
SOMA MOTOR_FECHAMENTO|2026-10|horas_trabalhadas|dias=12176.46|retorno=12176.47|False
SOMA PARTES_EMISSOR|2026-10|horas_intra_indenizada|v2=False|v3=False
SOMA DIVERGENCIA|2026-10|horas_intra_indenizada|partes_h=-8.000000000|autoridade_menos_residuo_h=-8.000000000|False
SOMA COLABS_UNIAO_DIAS|2026-10|horas_intra_indenizada|colabs=False|dias=False
SOMA V3_PORTA_EMISSOR|2026-10|horas_intra_indenizada|False
SOMA MOTOR_FECHAMENTO|2026-10|horas_intra_indenizada|dias=445.77|retorno=445.77|False
SOMA PARTES_EMISSOR|2026-10|horas_extras_50|v2=False|v3=False
SOMA DIVERGENCIA|2026-10|horas_extras_50|partes_h=0.224154212|autoridade_menos_residuo_h=0.224154213|False
SOMA COLABS_UNIAO_DIAS|2026-10|horas_extras_50|colabs=False|dias=False
SOMA V3_PORTA_EMISSOR|2026-10|horas_extras_50|False
SOMA MOTOR_FECHAMENTO|2026-10|horas_extras_50|dias=50.15|retorno=50.15|False
SOMA PARTES_EMISSOR|2026-10|horas_extras_100|v2=False|v3=False
SOMA DIVERGENCIA|2026-10|horas_extras_100|partes_h=0.000000000|autoridade_menos_residuo_h=-0.000000000|False
SOMA COLABS_UNIAO_DIAS|2026-10|horas_extras_100|colabs=False|dias=False
SOMA V3_PORTA_EMISSOR|2026-10|horas_extras_100|False
SOMA MOTOR_FECHAMENTO|2026-10|horas_extras_100|dias=88.64|retorno=88.64|False
SOMA PARTES_EMISSOR|2026-10|horas_extras_100_feriado|v2=False|v3=False
SOMA DIVERGENCIA|2026-10|horas_extras_100_feriado

[truncado em 20000 de 27017 caracteres]

## 06/11 13:18 · sessao ced52b90b319 · E-SEM-VIVA-B · verificador · origem=Code

**A volta 3 cura os cortes do retentor reconhecidos na triagem. A equivalência fica demonstrável para células `nunca_bateu` até ontem, no mesmo estado cadastral; não para toda a classificação efetiva do TXT. Restam um custo novo por desligado e uma garantia incompleta do selo AST.** (`app/escala/services/cadastro_realidade.py:163–174`; `app/folha/export.py:775–780,858–876,960,1192–1258`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:179–199`.)

**1. Informativa — procedente: escritores e leitores.**

| Campo/função | Quem escreve ou produz | Quem lê |
|---|---|---|
| `CelulaDia.veredito` | Porta atribui e persiste; método do modelo delega. Cartório e decisão humana são chamadores. (`app/ponto/portas/celula.py:789–819`; `app/escala/models.py:1819–1828`; `app/ponto/services/cartorio.py:1242–1247`; `app/ponto/services/veredito_celula.py:330`.) | C5, retentor, adesão, vigia do fio e Dia da Frota. (`app/escala/services/cadastro_realidade.py:164`; `app/folha/export.py:1233–1255`; `app/chamados/services/adesao.py:32–34`; `app/chamados/services/vigia_fio.py:379`; `app/relatorios/dia_da_frota.py:51–60`.) |
| `CelulaDia.protestos` | Mesma porta: cartório fornece códigos; decisão administrativa preserva os anteriores. (`app/ponto/portas/celula.py:789–795,813–819`.) | Retentor, juiz da pausa, outras assinaturas da lista e vigia de furos. **C5 não lê diretamente.** (`app/folha/export.py:1165,1246–1253`; `app/escala/services/cadastro_realidade.py:170,430,559`; `app/ponto/services/furos_sem_cobranca.py:57`.) |
| `Colaborador.situacao` | Operação `situacao` da porta; desligamento, reativação, INSS e reversor a chamam. (`app/colaboradores/services/colaborador.py:32,55–93`; `app/colaboradores/services/desligamento.py:104–108,210–214`; `app/ponto/views.py:2433–2435`; `app/ponto/management/commands/reverter_situacao_afastado.py:65–80`.) | Seleção dos desligados na C5, C4, emissão, adesão, cobrança e efetivação dos agendados. (`app/escala/services/cadastro_realidade.py:168,550`; `app/ponto/services/cartorio.py:1697,1762`; `app/chamados/services/adesao.py:33,66`; `app/escala/services/furos_diarios.py:259`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38–45`.) |
| `Colaborador.data_demissao` | Mesma operação; agendamento preenche sem desligar, desligamento preenche, reativação limpa. (`app/colaboradores/services/colaborador.py:32,55–76`; `app/colaboradores/services/desligamento.py:85–86,104–108,210–214`.) | `sai_pela_rescisao`, retentor, cartório, cadastro e cron dos agendados. **C5 recebe a resposta das funções.** (`app/folha/export.py:60,1200–1201,1229`; `app/ponto/services/cartorio.py:1096`; `app/escala/services/cadastro_realidade.py:169–170,463`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38–45`.) |
| `Colaborador.data_admissao` | Operação `cadastro`; criação e edição cadastral fornecem o valor. (`app/colaboradores/services/colaborador.py:34–35,55–93`; `app/colaboradores/views.py:1188,1314,1340–1342`.) | Retentor corta o início; C5 herda esse corte. (`app/folha/export.py:1198–1206`; `app/escala/services/cadastro_realidade.py:170`.) |
| `Colaborador.isento_ponto` | Operação `cadastro`, chamada pela tela de passe livre. (`app/colaboradores/services/colaborador.py:34–35,55–61`; `app/colaboradores/views.py:164–176`.) | Autoridade `core.constants.isento_ponto`; retentor, adesão e furos diários a consultam. (`app/core/constants.py:13–18`; `app/folha/export.py:1192`; `app/chamados/services/adesao.py:64`; `app/escala/services/furos_diarios.py:33,298`.) |
| `colabs_nunca_bateu` | Produz mapa em memória, sem persistência. (`app/escala/services/cadastro_realidade.py:160–175`.) | Chamadores produtivos diretos encontrados: `_nunca_bateu` e `censo`. (`app/escala/services/cadastro_realidade.py:181`; `app/folha/vigia_retido_calado.py:43`.) |
| `sai_pela_rescisao` | Produz booleano; compara demissão com fim, **sem ler situação**. (`app/folha/export.py:54–60`.) | Classificador, C5 e resumo do fechamento. (`app/folha/export.py:858`; `app/escala/services/cadastro_realidade.py:169`; `app/ponto/services/fechamento_lista.py:51,109`.) |
| `motivos_retencao_celula` | Produz motivos e, opcionalmente, dias retidos; sem persistência. (`app/folha/export.py:1168–1258`.) | C5, compatibilidade do classificador, fora da folha, prévia e vigia da pausa. (`app/escala/services/cadastro_realidade.py:170`; `app/folha/export.py:960,1267`; `app/folha/fora_da_folha.py:98`; `app/folha/previa.py:170`; `app/folha/services/vigia_pausa.py:125`.) |
| `janela_atual/anterior` | Motor produz intervalos pelo corte da empresa; não persiste. (`app/ponto/janelas.py:10–72`.) | C5, cartório, furos, cadastro de tipo, pré-fechamento, adesão e lavrador do Retrato; exportação usa `janela_fechamento` do mesmo motor. (`app/escala/services/cadastro_realidade.py:163`; `app/ponto/services/cartorio.py:264,325`; `app/escala/services/furos_diarios.py:68,76`; `app/escala/services/cadastro_tipo.py:336`; `app/folha/management/commands/pre_fechamento.py:73,89`; `app/chamados/services/adesao.py:56`; `app/folha/management/commands/lavrar_retrato_folha.py:46,60`; `app/folha/export.py:47–51`.) |
| `vigia_retido_calado.censo` | Lê Retratos e C5, produz contadores em memória. (`app/folha/vigia_retido_calado.py:33–57`.) | Command chama; `linhas()` formata. (`app/folha/management/commands/vigiar_retido_calado.py:14,31–33`; `app/folha/vigia_retido_calado.py:60–66`.) |
| Command `vigiar_retido_calado` | Lê relógio, chama censo, imprime e pode encerrar com código 2; não persiste. (`app/folha/management/commands/vigiar_retido_calado.py:28–40`.) | Cron registrado após a lavra do Retrato. Execução efetiva: **[não verificado]**. (`app/config/crons.py:1641–1643`.) |

Exaustividade de acessos dinâmicos, SQL externo e consumidores fora desta cópia: **[não verificado]**.

**2. Informativa — procedente: equivalência com rescisão + retentor, no recorte da C5.**

Para desligado, a implementação calcula precisamente:

`NB existente nas duas janelas, até ontem ∩ dias do retentor nas competências que não saem pela rescisão`.

Não filtrar `x['motivo']` não produz divergência nesse estado: existe uma única célula por colaborador/data, e uma célula NB retida recebe `espelho_nunca_bateu`. (`app/escala/services/cadastro_realidade.py:163–174`; `app/escala/models.py:1833`; `app/folha/export.py:1233–1254`.)

Casos sintéticos abaixo: hoje **25/10/2026**, corte **21**, anterior **21/09–20/10**, corrente **21/10–20/11**; NB já lavrado, sem outras isenções além das indicadas. (`app/ponto/janelas.py:10–33,60–72`; `app/escala/services/cadastro_realidade.py:163–170`.)

| Caso | C5 versus rescisão + retentor |
|---|---|
| Admissão 10/10; NB 05/10 e 20/10; demissão 22/10 | **Coincidem: só 20/10.** Admissão corta 05/10; corrente sai pela rescisão. (`app/folha/export.py:60,1198–1206`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:120–123`.) |
| Isento | **Coincidem: vazio.** (`app/folha/export.py:1192–1196`; `app/escala/services/cadastro_realidade.py:170`.) |
| Protestos somente `FURO_PRE_ADOCAO` | **Coincidem: dia dispensado.** (`app/ponto/supra_juiz.py:34–36`; `app/folha/export.py:1245–1254`.) |
| Sem protestos ou mistura com código cobrável | **Coincidem: dia retido.** (`app/folha/export.py:1242–1254`.) |
| Somente `FURO_PARCIAL`, entrada/saída completas e apenas pausa apagada | **Coincidem: dia dispensado.** A resposta vem do juiz de marcos pelo retentor. (`app/folha/export.py:1140–1147,1165,1253`; `app/escala/services/cadastro_realidade.py:170`.) |
| `FURO_PARCIAL` misto, ou ata desconhecida/desalinhada/turno aberto | **Coincidem: dia retido.** A exceção exige protesto exclusivo e confirmação da pausa. (`app/folha/export.py:1142–1147,1165,1253–1254`.) |
| Demissão 21/09 ou 20/10 | **Coincidem: nenhuma competência passa.** (`app/folha/export.py:60`; `app/escala/services/cadastro_realidade.py:169`.) |
| Demissão 21/10 ou 20/11 | **Coincidem: só anterior.** O primeiro e o último dia da corrente já eliminam a corrente inteira. (`app/folha/export.py:60`; `app/escala/services/cadastro_realidade.py:169`.) |
| Demissão futura 30/10 | **Coincidem: só anterior**, embora a demissão ainda não tenha ocorrido. (`app/folha/export.py:60`; `app/escala/services/cadastro_realidade.py:169`.) |
| Demissão nula ou 21/11 | Ambas passam pela rescisão; **coincidem para NB até ontem**. (`app/folha/export.py:60`; `app/escala/services/cadastro_realidade.py:165,169–173`.) |
| Corte 10; demissão 15/10; NB 05/10 e 12/10 | **Coincidem: só 05/10.** Anterior termina 09/10; corrente termina 09/11. (`app/ponto/janelas.py:10–33,60–72`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:147–152`.) |
| Hoje 21/10, primeiro dia da competência | C5 só alcança a anterior. O retentor da corrente pode reter NB em 21/10 ou depois, caso passe pela rescisão. **Divergência pelo limite temporal da C5.** (`app/escala/services/cadastro_realidade.py:159,163–165`; `app/folha/export.py:1233–1254`.) |
| Sem demissão; NB em 25/10 e 26/10 | **Retentor inclui; C5 exclui ambos.** O ramo acusador do retentor não exige `d < hoje`. (`app/folha/export.py:1225–1254`; `app/escala/services/cadastro_realidade.py:165`.) |
| Troca A/corte21 → B/corte10; demissão 15/10; NB 05/10 | Antes nada; depois 05/10 entra. **Ambos usam a empresa atual**, portanto não há divergência entre as funções no mesmo estado. Retrato histórico pode discordar da lista atual. (`app/escala/services/cadastro_realidade.py:160–170`; `app/folha/export.py:778,807,1266`; `app/folha/vigia_retido_calado.py:38–48`.) |

**Média — procedente como limite da alegação:** igualdade irrestrita continua falsa por hoje/futuro. Esse limite já integra o contrato declarado da C5; não é regressão introduzida nesta volta. (`app/escala/services/cadastro_realidade.py:132,159,165`; `app/folha/export.py:1233–1254`.)

**Informativa — procedente:** igualdade com o resultado efetivo de `classificar_export` também não está provada: fechamento ausente não entra no universo padrão; código vazio, férias com batida e cadastro zero podem sair antes do retentor. Isso limita a alegação, sem invalidar a cura restrita determinada pela triagem. (`app/folha/export.py:775–780,864–876,951–960`; `app/escala/services/cadastro_realidade.py:169–170`.)

**3. Informativa — procedente: ativo conserva os valores; identidade operacional tem ressalva.**

Comparando os anexos `v3_base.diff` e `v3.diff`, o ativo mantém o mesmo universo e ordenação de dias de **24946096** e **661c6190**: o novo filtro só alcança IDs selecionados como desligados. Admissão, isenção, protestos e demissão preenchida do ativo continuam sem cortar sua C5. Os testes exigem esses casos. (`app/escala/services/cadastro_realidade.py:164–174`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:93–98,153–160`; versões anteriores nos anexos fornecidos.)

**Média — dúvida operacional:** uma exceção no novo retentor de qualquer desligado agora aborta a função inteira; `lista()` captura a exceção e perde toda a contribuição C5, inclusive de ativos. Portanto a preservação é de valores numa leitura concluída, não de disponibilidade sob falha. Ocorrência concreta: **[não verificado]**. (`app/escala/services/cadastro_realidade.py:168–175,811–817`.)

**4. Informativa — procedente: situação da triagem da v2.**

| Item | Veredito |
|---|---|
| Achado 2: admissão, isenção e protestos | **Curado no contrato rescisão + retentor.** Agora consulta a função real; os três contraexemplos viraram asserções. (`app/escala/services/cadastro_realidade.py:169–170`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:119–136`.) |
| Achado 7: corte da empresa, demissão no fim, command sem hoje, predicado direto | **Casos acrescentados e selo presente.** A garantia arquitetural do selo tem a limitação do achado 5 abaixo. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:139–152,163–208`; `app/folha/tests/test_vigia_retido_calado.py:122–130`.) |
| Item 4: remoção do corte posterior à demissão | **Descarte procede.** Para sobreviver, demissão precisa ser posterior ao fim da competência; todo dia dessa competência é anterior à demissão. (`app/folha/export.py:60`; `app/ponto/janelas.py:60–72`; `app/escala/services/cadastro_realidade.py:169`.) |
| Item 3: preservação do ativo | **Procede**, com a ressalva operacional acima. (`app/escala/services/cadastro_realidade.py:168–174,811–817`.) |
| Item 13: relógio | **Curado no caminho command → censo → C5.** Fallback continua executável fora desse caminho. (`app/folha/management/commands/vigiar_retido_calado.py:31`; `app/folha/vigia_retido_calado.py:43`; `app/escala/services/cadastro_realidade.py:158`.) |
| Item 16: citação do corte | **Curado no teste:** aponta ao trecho efetivo de admissão/demissão. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:5–6`; `app/folha/export.py:1198–1201`.) |
| Roteamento sem `data` | **Permanece tecnicamente procedente**, conforme fila expressamente informada no pedido. C5 não fornece a chave; vigia reduz o mapa a IDs. (`app/escala/services/cadastro_realidade.py:182–183,986–989`; `app/folha/vigia_retido_calado.py:43–48`.) |
| Item 9: porta, sem push, HX intocado | **Descarte de defeito novo procede no diff fornecido.** Porta C5 preservada; emissão continua vetando desligado; HX continua lavrando pela porta com códigos vazios. (`app/escala/services/cadastro_realidade.py:637–639`; `app/ponto/services/cartorio.py:1241–1247,1696–1697,1762`.) |

**5. Média — procedente: custo novo; selo com mordida limitada.**

O comentário **“UMA query por empresa” está falso**. Além da consulta NB e do lote de desligados por empresa, cada janela elegível de um desligado não isento e com intervalo não colapsado consulta células e vínculos: **até quatro consultas adicionais por desligado**, duas quando só uma janela passa. Rescisão, isenção ou intervalo colapsado podem evitar essas consultas internas. Latência e quantidade real de desligados: **[não verificado]**. (`app/escala/services/cadastro_realidade.py:135,160–170`; `app/folha/export.py:1192–1212`; `app/escala/alimentacao.py:148–154,163–186`.)

O selo verifica presença sintática de chamadas e ausência de quatro atributos diretos; não verifica execução das chamadas, uso dos retornos ou leitura por strings. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:179–199`.)

**Mutante relevante:** substituir a chamada produtiva de rescisão pelo mesmo predicado usando `getattr(colab, 'data_demissao')` e preservar `if False: sai_pela_rescisao(None, None)`. Confirmei em memória que as asserções AST aceitam essa fonte; o predicado continua equivalente à autoridade atual, mas passa a existir uma cópia independente. Sobrevivência na suíte completa: **[não verificado]**. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:183–199`; `app/folha/export.py:60`.)

O anexo relata **M0 verde e M1–M7 mortos**; isso sustenta mordida contra os mutantes nomeados, não garantia geral de fonte única. Código integral dos mutantes e reprodução dessas execuções: **[não verificado]**. (`v3_mutantes.tab:1–8`, anexo fornecido; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:119–208`; `app/folha/tests/test_vigia_retido_calado.py:122–130`.)

| Classe de reprovação | Presença nesta árvore |
|---|---|
| Segundo juiz duplicando registrado | **Ausente para rescisão + retenção:** usa efetivamente as respostas reais. O classificador completo continua sendo a autoridade para entrada no TXT. (`app/escala/services/cadastro_realidade.py:169–173`; `app/core/juizes.py:288–299`.) |
| Segundo escritor fora da porta | **Ausente no diff produtivo:** leituras e resultados em memória; nenhum escritor novo nesses trechos. (`app/escala/services/cadastro_realidade.py:160–175`; `app/folha/vigia_retido_calado.py:33–57`; `app/folha/management/commands/vigiar_retido_calado.py:28–40`.) |
| Relógio fora da borda | **Ausente no caminho corrigido; fallback presente.** (`app/folha/management/commands/vigiar_retido_calado.py:31`; `app/folha/vigia_retido_calado.py:43`; `app/escala/services/cadastro_realidade.py:158`.) |
| Derivação paralela | **Ausente na implementação da pergunta restrita.** O selo não impede todas as futuras formas de reintroduzi-la. (`app/escala/services/cadastro_realidade.py:169–170`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:183–199`.) |
| Selo/RED sem mordida | **Ausência total: não. Garantia incompleta: sim**, conforme mutante acima. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:119–208`.) |
| Ordem de trava | **Ausente nova aquisição/inversão explícita** nos caminhos alterados e carregadores consultados. (`app/escala/services/cadastro_realidade.py:160–175`; `app/folha/export.py:1192–1258`; `app/escala/alimentacao.py:148–154`.) |
| Pouso antes do pré-requisito | **Dúvida — [não verificado].** Os 25 testes do anexo não demonstram suíte inteira verde seguida de opus final nem sequência de pouso. (`CLAUDE.md:364–373`; `v3_mutantes.tab:1`, anexo fornecido.) | [nao verificado]

L-026 é atendida pelo corte da empresa; L-033 chega à C5 pelo retentor; L-040 conserva lista e PDF compartilhados. (`app/docs/LEIS.md:77,81,88`; `app/escala/services/cadastro_realidade.py:163,170,992`; `app/escala/views.py:484–492`.)

**6. Informativa — procedente: outros leitores mudam frente a 661c6190.**

Para mesmos dados, mesmo hoje e leitura concluída, a v3 acrescenta uma interseção com o retentor: **só retira dias/IDs de desligados relativamente à v2**. (`app/escala/services/cadastro_realidade.py:169–174`; versão anterior no `v3.diff` fornecido.)

| Leitor | Quanto muda |
|---|---|
| `_nunca_bateu` / `lista()` | Menos dias no detalhe; ao perder o último, perde uma C5. C5 conta uma ocorrência por assinatura, não por dia. A pessoa desaparece se ficar sem nenhuma assinatura. (`app/escala/services/cadastro_realidade.py:181–184,813–814,858–866,875–904`.) |
| Tela/PDF | Exibem a lista alterada. (`app/escala/views.py:484–492`; `app/escala/services/cadastro_realidade.py:992–993`.) |
| `vigia_retido_calado` | Por ID que perde a última data: **roteados −k; calados +k**, sendo k suas linhas bloqueadas/`ninguem` nos Retratos lidos. Bloqueados e cego ficam iguais para os mesmos Retratos. (`app/folha/vigia_retido_calado.py:43–56`.) |
| Vigia do fio | C5 continua acrescentando **zero dias** ao mapa. Perdendo a última assinatura da pessoa, pode mudar causa **d → e**, satisfeitas as demais condições. (`app/escala/services/cadastro_realidade.py:182–183,986–989`; `app/chamados/services/vigia_fio.py:216–218,677–686`.) |
| `vigiar_fio_morto` | Contadores derivados podem mudar no carimbo JSON de frota; com `--empresa`, não carimba. (`app/chamados/management/commands/vigiar_fio_morto.py:210–230`.) |
| Fechadores de avisos/vínculos | **Variação de seleção por C5 = zero:** selecionam exclusivamente C7/C8. (`app/escala/management/commands/fechar_avisos_regua_na_lista.py:27–37`; `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:35–45`.) |
| `placar_estrutural` | **Variação automática = zero:** referência à lista é textual; o placar percorre `RESULTADOS`. (`app/core/placar_estrutural.py:149,650–660`.) |

**Roteamento — média, procedente e já encaminhado:** a falta de `data` permanece; o vigia ainda pode rotear outra competência/motivo pelo simples ID. **Esta v3 não amplia esse defeito frente a 661c6190:** reduz o conjunto de IDs elegíveis, podendo desfazer roteamentos anteriores. (`app/escala/services/cadastro_realidade.py:169–174,182–183,986–989`; `app/folha/vigia_retido_calado.py:43–53`.)

Contagens afetadas em produção, permissões efetivas dos administradores, suíte inteira e sequência de pouso: **[não verificado]**. [nao verificado]

## 06/11 13:16 · sessao c0068debe084 · - · - · origem=Ronald (tmux)

1. **Informativa — procedente: ativo conserva o valor, em execução concluída e cadastro estável.** O filtro inicial conserva o mesmo universo NB até ontem; o filtro novo só é aplicado a IDs que a segunda consulta identifica como `situacao='desligado'`. Isenção, admissão, protestos e demissão preenchida do ativo não cortam sua C5. (`app/escala/services/cadastro_realidade.py:164–174`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:93–98,153–160`.) Isso corresponde ao código antigo dos dois diffs fornecidos; comparação dos objetos Git antigos nesta cópia: **[não verificado]**.

   **Limitação operacional:** agora uma falha do retentor de qualquer desligado aborta `colabs_nunca_bateu`; `lista()` captura a falha e perde toda a contribuição C5, inclusive dos ativos daquela chamada. O fato de ocorrer uma falha real é **[não verificado]**; o caminho de propagação existe. (`app/escala/services/cadastro_realidade.py:168–175,811–817`.)

2. **Informativa — procedente: cura dos filtros reconhecidos na triagem.** A C5 agora pergunta ao retentor real, que corta admissão, isenção e protestos exclusivamente não cobraveis; as três novas fixtures cobrem precisamente esses contraexemplos. (`app/escala/services/cadastro_realidade.py:169–170`; `app/folha/export.py:1192–1206,1245–1254`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:119–136`.) A equivalência com a classificação efetiva continua limitada: existência do fechamento, código Domínio, férias com batida e cadastro zero antecedem o retentor no classificador. (`app/folha/export.py:775–780,864–876,951–960`.) A triagem procede ao curar **rescisão + retentor**, mas não prova equivalência irrestrita com `classificar_export`. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:49–60`.)

3. **Média — procedente: selo AST morde predicado direto, mas não garante dependência real da autoridade.** O selo registra a presença sintática das chamadas importadas e a ausência de quatro atributos diretos; não analisa execução, uso do retorno ou acesso por strings/keywords. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:183–199`.) O teste da própria costura detecta precisamente `c.data_demissao`, e resolve alias de importação. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:201–208`.)

   **Mutante relevante sobrevivente por inspeção:** manter uma chamada morta `if False: sai_pela_rescisao(colab, fim)` e substituir a chamada produtiva por helper com `d = getattr(colab, 'data_demissao'); return d is not None and d <= fim`. O selo continua vendo a chamada e não encontra atributo proibido; os resultados dos testes de comportamento permanecem iguais à autoridade atual. Esse é precisamente um segundo juiz que voltaria a divergir quando a autoridade mudasse. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:183–199`; `app/folha/export.py:54–60`.) Sobrevivência executada: **[não verificado]**. O anexo M5 comprova somente a morte do mutante executado, cujo código completo não foi fornecido: **[não verificado]**.

4. **Informativa — procedente: quem mais muda frente a 661c6190.**

   A v3 somente retira dias de desligados que a v2 mantinha: a interseção com dias do retentor acrescenta um filtro após a mesma rescisão. Para mesmos dados/hoje, não acrescenta IDs ou dias à v2. (`app/escala/services/cadastro_realidade.py:163–174`; v3.diff fornecido, versão anterior sem numeração própria: **[não verificado]**.)

   | Consumidor | Alteração |
   |---|---|
   | `_nunca_bateu` e `lista()` | Detalhe tem menos dias; perdendo o último, perde uma C5. Cada C5 conta uma ocorrência, independentemente do número de dias; pode desaparecer a pessoa se era sua única assinatura. (`app/escala/services/cadastro_realidade.py:181–184,813–814,857–866,875–904`.) |
   | Tela/PDF | Recebem e exibem a mesma lista alterada. (`app/escala/views.py:484–492`; `app/templates/escala/cadastro_x_realidade.html:21–28`; `app/escala/services/cadastro_realidade.py:1022–1033`.) |
   | `vigia_retido_calado` | Para cada ID que perde o último dia, `roteados −k`, `calados +k`, onde k são suas linhas bloqueadas com fio `ninguem` nos Retratos lidos. `bloqueados` e `cego` permanecem iguais com mesmos Retratos. (`app/folha/vigia_retido_calado.py:43–56`.) |
   | `ListaDoAdmin` / vigia do fio | C5 não altera mapa de dias, mas perder sua última assinatura pode retirar a presença do colaborador. Sob as demais condições, a causa muda de `d` para `e`, detalhe `sem_pergunta_viva`. (`app/chamados/services/vigia_fio.py:216–218,677–686`.) |
   | `vigiar_fio_morto` | Os contadores e alcance derivados podem mudar no JSON persistido em rodada de frota; com recorte `--empresa`, não carimba. (`app/chamados/management/commands/vigiar_fio_morto.py:210–230`.) |
   | Fechador de avisos e de vínculos | Seleção muda **zero por C5**, pois consomem somente C7 e C8, respectivamente. (`app/escala/management/commands/fechar_avisos_regua_na_lista.py:27–37`; `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:35–45`.) |
   | `placar_estrutural` | Mudança automática zero: a referência a `lista` é texto; `placar()` itera `RESULTADOS`. (`app/core/placar_estrutural.py:149,650–660`.) |

   Quantidades efetivas em produção: **[não verificado]**.

5. **Média — procedente, pendência já autorizada em fila: roteamento continua sem data, mas não há agravamento por ampliação de universo nesta v3.** C5 segue transportando datas somente no detalhe; `dias_na_lista()` exige chave `data`. (`app/escala/services/cadastro_realidade.py:182–183,986–989`.) O vigia de retidos segue reduzindo C5 a IDs, e com um dia em qualquer janela roteia todas as linhas bloqueadas/`ninguem` desse ID, sem cruzar competência ou motivo. (`app/folha/vigia_retido_calado.py:43–48`.) A fixture usa linhas genéricas `CALADA` para demonstrar esse roteamento por pessoa. (`app/folha/tests/test_vigia_retido_calado.py:79–89`.) Como a v3 reduz o conjunto de IDs, não cria novos falsos roteados nesse mecanismo; pode desfazer roteamentos existentes. (`app/escala/services/cadastro_realidade.py:169–174`; `app/folha/vigia_retido_calado.py:43–53`.) A ocorrência histórica concreta de falso roteamento é **[não verificado]**.

6. **Classes do checklist:**

   - **Segundo juiz / derivação paralela: ausente na implementação para a pergunta restrita rescisão + retenção.** Chama ambas autoridades e usa efetivamente seus retornos; não copia cortes de admissão/isenção/protesto. (`app/escala/services/cadastro_realidade.py:156–170`.) Para “quem entra no TXT”, a autoridade registrada continua sendo o classificador completo; não chamar seus demais portões impede alegar equivalência geral. (`app/core/juizes.py:288–299`; `app/folha/export.py:864–876,951–960`.)
   - **Segundo escritor e inversão de trava: ausentes nos trechos produtivos alterados.** C5, censo e command consultam/montam/imprimem, sem gravação ou aquisição explícita de trava. (`app/escala/services/cadastro_realidade.py:160–175`; `app/folha/vigia_retido_calado.py:33–57`; `app/folha/management/commands/vigiar_retido_calado.py:28–40`.)
   - **Relógio fora da borda: ausente no caminho command corrigido; fallback interno continua.** O command injeta `hoje`, censo o repassa e C5 só lê relógio se receber valor falso/nulo. (`app/folha/management/commands/vigiar_retido_calado.py:31`; `app/folha/vigia_retido_calado.py:43`; `app/escala/services/cadastro_realidade.py:158`.) Teste novo intercepta a função real e confere o argumento. (`app/folha/tests/test_vigia_retido_calado.py:122–130`.)
   - **Selo/RED sem mordida: ausente como ausência total; cobertura limitada conforme achado 3.** Casos de corte10, demissão no fim, ativo e filtros novos têm asserções que distinguem os defeitos nomeados. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:119–160`.) Execução pessoal dos testes/mutantes: **[não verificado]**; o anexo relata M0 verde e M1–M7 mortos.
   - **Pouso antes do pré-requisito: dúvida, [não verificado].** O documento exige suíte inteira verde antes de um opus final, e anexa descartes à decisão final; 25 testes relatados não comprovam suíte inteira nem ordem de pouso. (`CLAUDE.md:364–373`.) [nao verificado]

7. **Sem defeito novo de push — procedente.** C5 conserva porta para detalhe do colaborador; `_vinculos` não exclui desligados. (`app/escala/services/cadastro_realidade.py:637–639,710–713`.) O cartório continua excluindo desligados dos dois portões de emissão, e adesão também os exclui. (`app/ponto/services/cartorio.py:1696–1697,1762`; `app/chamados/services/adesao.py:66–67`.) O ramo HX conserva a lavra `nunca_bateu` pela porta com `codigos=[]`; o diff fornecido não altera esse arquivo. (`app/ponto/services/cartorio.py:1241–1247`; v3_base.diff fornecido.)

8. **Leis:** L-040 conserva lista única e mesmo PDF; L-033 mora no retentor e agora é alcançada por delegação; L-026 conserva corte da empresa nas duas janelas. (`app/docs/LEIS.md:88,81,77`; `app/escala/services/cadastro_realidade.py:163,169–170`; `app/folha/export.py:1245–1254`.)

## 06/11 13:15 · sessao e5bcf41be2b8 · - · - · origem=Ronald (tmux)

**Equivalência — informativa, procedente com recorte:** para o mesmo estado cadastral, desligado, células `nunca_bateu` e datas `< hoje`, a C5 coincide com **rescisão + retentor**. A consulta inicial seleciona NB até ontem; depois intersecta as datas devolvidas pelo retentor nas duas janelas que não saem pela rescisão. A unicidade `(colaborador,data)` evita outra célula do mesmo dia dar motivo diferente. (`app/escala/services/cadastro_realidade.py:163–174`; `app/escala/models.py:1833`; `app/folha/export.py:1233–1254`.)

**Equivalência irrestrita — média, procedente:** hoje/futuro continuam diferentes: C5 exclui por `data__lte=ontem`; retentor aceita célula acusadora até `fim`, sem condição `d < hoje` nesse ramo. Exemplo sintético: hoje 25/10, desligado sem demissão, NB em 25/10 e 26/10; retentor devolve ambos, C5 nenhum. Ocorrência em produção: [não verificado]. (`app/escala/services/cadastro_realidade.py:159,165`; `app/folha/export.py:1225–1255`.)

**Classificação efetiva — informativa, procedente como limite:** a cura é do recorte acordado com o retentor, não igualdade geral com `classificar_export`. Fechamento ausente não integra o universo padrão; código vazio ou férias com batida saem antes de consultar o espelho. As fixtures `_cria` não fornecem `codigo_dominio`, cujo default é vazio; assim continuam inadequadas para provar a classificação efetiva, embora adequadas ao contrato restrito do retentor. (`app/folha/export.py:775–780,858–876,960`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:31–34`; `app/colaboradores/models.py:311–313`.)

Casos sintéticos, todos para NB já lavrado, sem outras isenções, hoje 25/10/2026, corte21, anterior 21/09–20/10 e corrente21/10–20/11. Janelas e veto derivam das autoridades existentes. (`app/ponto/janelas.py:10–33,60–72`; `app/folha/export.py:60`; `app/escala/services/cadastro_realidade.py:163–170`.)

| Caso | Resultado C5 e rescisão+retentor |
|---|---|
| Admissão10/10; NB05/10,20/10; demissão22/10 | Ambos só20/10: anterior passa rescisão, admissão corta05; corrente sai. (`app/folha/export.py:60,1198–1206`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:120–123`.) |
| Isento | Ambos vazios. (`app/folha/export.py:1192–1196`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:126–129`.) |
| Protestos somente `FURO_PRE_ADOCAO` | Ambos excluem o dia. (`app/ponto/supra_juiz.py:34–36`; `app/folha/export.py:1245–1254`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:132–136`.) |
| Sem protestos ou protestos mistos, incluindo código cobrável | Ambos retêm. (`app/folha/export.py:1242–1254`.) |
| Somente `FURO_PARCIAL` e juiz confirma somente pausa apagada | Ambos excluem; não há novo filtro paralelo na C5. (`app/folha/export.py:1140–1147,1165,1253`; `app/escala/services/cadastro_realidade.py:169–170`.) |
| `FURO_PARCIAL` misto com outro código, ou ata desconhecida/desalinhada/turno aberto | Ambos retêm. (`app/folha/export.py:1142–1147,1165,1253–1254`.) |
| Demissão21/09 ou20/10 | Ambas as competências saem; nada entra. (`app/folha/export.py:60`; `app/escala/services/cadastro_realidade.py:169`.) |
| Demissão21/10 ou20/11 | Só anterior; inclusive demissão no primeiro/último dia da corrente já elimina a corrente inteira. (`app/folha/export.py:60`; `app/escala/services/cadastro_realidade.py:169`.) |
| Demissão futura30/10 | Só anterior, pois30/10≤20/11. (`app/folha/export.py:60`; `app/escala/services/cadastro_realidade.py:169`.) |
| Demissão21/11 ou nula | As duas competências passam; C5 ainda limita até ontem. (`app/folha/export.py:60`; `app/escala/services/cadastro_realidade.py:165,169`.) |
| Corte10, demissão15/10, NB05/10 e12/10 | Anterior10/09–09/10 passa; corrente10/10–09/11 sai; só05/10 em ambos. (`app/ponto/janelas.py:10–33,60–72`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:147–152`.) |
| Hoje21/10, primeiro dia da competência | C5 só pode devolver anterior, até20/10; retentor da corrente pode devolver NB21/10 ou futura se rescisão não a eliminar. (`app/escala/services/cadastro_realidade.py:159,163–165`; `app/folha/export.py:1233–1254`.) |
| Mudança A/corte21→B/corte10; demissão15/10; NB05/10 | Antes nada; depois05/10 entra. C5 e export usam empresa atual; isso não prova divergência entre ambos no mesmo estado. Fotografia histórica do Retrato pode divergir da lista atual. (`app/escala/services/cadastro_realidade.py:160–170`; `app/folha/export.py:778,807,1266`; `app/folha/vigia_retido_calado.py:38–48`.) |

**Custo — média, procedente:** o comentário “UMA query por empresa” ficou falso. Além da consulta NB por empresa e do lote de colaboradores desligados, cada janela elegível de cada desligado não isento/não colapsado faz uma consulta de células e outra de vínculos. Até **quatro SQL adicionais por desligado** quando ambas passam a rescisão; duas quando só uma passa; zero consultas internas quando rescisão elimina ambas, isento retorna antes ou janela colapsa. Uma consulta global carrega empresas. Não há consulta nova por dia no caminho de pausa. Contagens executadas/latência real: [não verificado]. (`app/escala/services/cadastro_realidade.py:135,160–170`; `app/folha/export.py:1192–1212`; `app/escala/alimentacao.py:148–154,163–186`; `app/folha/export.py:1140–1147`.)

**RED — informativa, procedente:** admissão, isenção, pré-adoção, demissão no fim e corte diferente foram adicionados como casos. Não há caso novo cobrindo `FURO_PARCIAL`, hoje no corte ou troca de empresa; isso é lacuna de cobertura, não contraexemplo à implementação delegada. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:119–160`; `app/escala/services/cadastro_realidade.py:169–170`.)

## 06/11 13:15 · sessao 0235c4f257d9 · - · - · origem=Ronald (tmux)

Mapa enviado ao agente principal, com referências atuais de escritores, leitores e produtores em memória. A inspeção não executou testes nem comandos de escrita; exaustividade de acessos dinâmicos e execução efetiva em produção permanecem [não verificado]. [nao verificado]

## 06/11 13:12 · sessao aa768c718c00 · R1-CAUSAD-V3 · sonda · origem=Code

No dado fornecido, nenhuma linha `SOMA` está `True`: todas indicam `False`. Há `INCOMPLETO: 596`. [nao verificado]

Transcrição integral da saída fornecida, parte 5 de 6. [nao verificado]

```text
sonda_leitura: script.py [3027607d50f632b4] no cpuset 4-7, lendo sombra como leitor (LEITURA)

PARTE|5|6
ATO|hoje=2026-11-06|agora=2026-11-06T13:04:13.903213-03:00
FORA_DA_MEDIDA|horas_falta
FORA_DA_MEDIDA|horas_reflexo_dsr
FORA_DA_MEDIDA|saldo_banco_horas
UNIVERSO|2026-10|colabs_porta=624|segundos=0.1
UNIVERSO_PARTE 2026-10 107
RUBRICA|competencia|rubrica|v2_h|v3_h|delta_h|colabs_div|dias_colab_div
DONO|competencia|rubrica|dono|v2_h|v3_h|delta_h|colabs|dias_colab
DIA|2026-10|horas_intra_indenizada|col82|2026-10-06|CADASTRO|1.166666667|0.000000000|-1.166666667
DIA|2026-10|horas_intra_indenizada|col82|2026-10-08|CADASTRO|1.166666667|0.000000000|-1.166666667
DIA|2026-10|horas_intra_indenizada|col82|2026-10-12|CADASTRO|1.166666667|0.000000000|-1.166666667
DIA|2026-10|horas_intra_indenizada|col82|2026-10-14|CADASTRO|1.166666667|0.000000000|-1.166666667
DIA|2026-10|horas_intra_indenizada|col82|2026-10-15|CADASTRO|1.166666667|0.000000000|-1.166666667
DIA|2026-10|horas_intra_indenizada|col82|2026-10-16|CADASTRO|1.166666667|0.000000000|-1.166666667
DIA|2026-10|horas_intra_indenizada|col82|2026-10-20|CADASTRO|1.166666667|0.000000000|-1.166666667
DIA|2026-10|horas_extras_50|col82|2026-10-06|CADASTRO|0.000000000|0.500647778|+0.500647778
DIA|2026-10|horas_extras_50|col82|2026-10-08|CADASTRO|0.000000000|0.471973333|+0.471973333
DIA|2026-10|horas_extras_50|col82|2026-10-14|CADASTRO|0.000000000|0.179223056|+0.179223056
DIA|2026-10|horas_extras_50|col82|2026-10-15|CADASTRO|0.000000000|0.557518333|+0.557518333
DIA|2026-10|horas_atraso|col274|2026-10-04|ESTRUTURA|0.000000000|2.330333333|+2.330333333
DIA|2026-10|horas_extras_100_noturna|col292|2026-10-12|ESTRUTURA|0.000000000|0.909870556|+0.909870556
DIA|2026-10|horas_trabalhadas|col502|2026-10-10|CADASTRO|7.569179979|2.843692404|-4.725487575
DIA|2026-10|horas_extras_50|col502|2026-10-10|CADASTRO|0.235846646|0.000000000|-0.235846646
DIA|2026-10|horas_trabalhadas|col592|2026-10-20|CADASTRO|7.314860278|5.421271667|-1.893588611
DIA|2026-10|horas_noturnas|col964|2026-09-25|ESTRUTURA|1.000000000|1.142857143|+0.142857143
DIA|2026-10|horas_noturnas|col964|2026-09-26|BATIDA|1.015868889|1.160993016|+0.145124127
DIA|2026-10|horas_noturnas|col964|2026-09-27|ESTRUTURA|0.976877500|1.116431429|+0.139553929
DIA|2026-10|horas_noturnas|col964|2026-09-28|CADASTRO|0.939963611|1.074244127|+0.134280516
DIA|2026-10|horas_trabalhadas|col988|2026-10-17|CADASTRO|6.190200326|3.268974456|-2.921225870
PARCIAL|2026-10|horas_trabalhadas|v2_h=12435.675896993611|v3_h=12426.135594937223|colabs_div=3|dias_colab_div=3
RUBRICA|2026-10|horas_trabalhadas|12435.675896994|12426.135594937|-9.540302056|3|3
DONO|2026-10|horas_trabalhadas|CADASTRO|21.074240583|11.533938527|-9.540302056|3|3
RESIDUO|2026-10|horas_trabalhadas|ATE_LIMIAR|12414.601656410|12414.601656410|+0.000000000|89|1333
RESIDUO|2026-10|horas_trabalhadas|SEM_DECISAO|73.246107319|73.246107319|+0.000000000|59|346
RESIDUO|2026-10|horas_trabalhadas|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|155
INSTRUMENTO_HISTORICO|2026-10|horas_trabalhadas|v2_h=12416.013102646|v3_h=12426.135594937|delta_h=+10.122492291
PARCIAL|2026-10|horas_intra_indenizada|v2_h=369.55118945583331|v3_h=361.38452278916668|colabs_div=1|dias_colab_div=7
RUBRICA|2026-10|horas_intra_indenizada|369.551189456|361.384522789|-8.166666667|1|7
DONO|2026-10|horas_intra_indenizada|CADASTRO|8.166666667|0.000000000|-8.166666667|1|7
RESIDUO|2026-10|horas_intra_indenizada|ATE_LIMIAR|361.384522789|361.384522789|+0.000000000|89|1329
RESIDUO|2026-10|horas_intra_indenizada|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|59|346
RESIDUO|2026-10|horas_intra_indenizada|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|155
INSTRUMENTO_HISTORICO|2026-10|horas_intra_indenizada|v2_h=366.551189456|v3_h=361.384522789|delta_h=-5.166666667
PARCIAL|2026-10|horas_extras_50|v2_h=3.1360523825|v3_h=4.6095682363888901|colabs_div=2|dias_colab_div=5
RUBRICA|2026-10|horas_extras_50|3.136052382|4.609568236|+1.473515854|2|5
DONO|2026-10|horas_extras_50|CADASTRO|0.235846646|1.709362500|+1.473515854|2|5
RESIDUO|2026-10|horas_extras_50|ATE_LIMIAR|2.900205736|2.900205736|+0.000000000|89|1312
RESIDUO|2026-10|horas_extras_50|SEM_DECISAO|2.000000000|2.000000000|+0.000000000|60|365
RESIDUO|2026-10|horas_extras_50|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|155
INSTRUMENTO_HISTORICO|2026-10|horas_extras_50|v2_h=5.136052382|v3_h=4.609568236|delta_h=-0.526484146
PARCIAL|2026-10|horas_extras_100|v2_h=68.607693309444443|v3_h=68.607693309444443|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_extras_100|68.607693309|68.607693309|+0.000000000|0|0
RESIDUO|2026-10|horas_extras_100|ATE_LIMIAR|68.607693309|68.607693309|+0.000000000|89|1317
RESIDUO|2026-10|horas_extras_100|SEM_DECISAO|3.141441776|3.141441776|+0.000000000|60|365
RESIDUO|2026-10|horas_extras_100|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|155
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100|v2_h=71.749135085|v3_h=69.024955254|delta_h=-2.724179831
PARCIAL|2026-10|horas_extras_100_feriado|v2_h=68.607693309444443|v3_h=68.607693309444443|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_extras_100_feriado|68.607693309|68.607693309|+0.000000000|0|0
RESIDUO|2026-10|horas_extras_100_feriado|ATE_LIMIAR|68.607693309|68.607693309|+0.000000000|89|1317
RESIDUO|2026-10|horas_extras_100_feriado|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|60|365
RESIDUO|2026-10|horas_extras_100_feriado|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|155
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100_feriado|v2_h=0.000000000|v3_h=69.024955254|delta_h=+69.024955254
PARCIAL|2026-10|horas_extras_50_noturna|v2_h=0.3541233333333334|v3_h=0.3541233333333334|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_extras_50_noturna|0.354123333|0.354123333|+0.000000000|0|0
RESIDUO|2026-10|horas_extras_50_noturna|ATE_LIMIAR|0.354123333|0.354123333|+0.000000000|89|1317
RESIDUO|2026-10|horas_extras_50_noturna|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|60|365
RESIDUO|2026-10|horas_extras_50_noturna|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|155
INSTRUMENTO_HISTORICO|2026-10|horas_extras_50_noturna|v2_h=0.000000000|v3_h=0.354123333|delta_h=+0.354123333
PARCIAL|2026-10|horas_extras_100_noturna|v2_h=9.6304372222222234|v3_h=10.542015833333332|colabs_div=1|dias_colab_div=1
RUBRICA|2026-10|horas_extras_100_noturna|9.630437222|10.542015833|+0.911578611|1|1
DONO|2026-10|horas_extras_100_noturna|ESTRUTURA|0.000000000|0.909870556|+0.909870556|1|1
RESIDUO|2026-10|horas_extras_100_noturna|ATE_LIMIAR|9.630437222|9.632145278|+0.001708056|89|1316
RESIDUO|2026-10|horas_extras_100_noturna|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|60|365
RESIDUO|2026-10|horas_extras_100_noturna|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|155
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100_noturna|v2_h=0.000000000|v3_h=10.542015833|delta_h=+10.542015833
PARCIAL|2026-10|horas_folga_trabalhada|v2_h=0|v3_h=0|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_folga_trabalhada|0.000000000|0.000000000|+0.000000000|0|0
RESIDUO|2026-10|horas_folga_trabalhada|ATE_LIMIAR|0.000000000|0.000000000|+0.000000000|89|1336
RESIDUO|2026-10|horas_folga_trabalhada|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|59|346
RESIDUO|2026-10|horas_folga_trabalhada|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|155
INSTRUMENTO_HISTORICO|2026-10|horas_folga_trabalhada|v2_h=92.908901667|v3_h=0.000000000|delta_h=-92.908901667
PARCIAL|2026-10|horas_noturnas|v2_h=2347.5012535976189|v3_h=2348.0630693119047|colabs_div=1|dias_colab_div=4
RUBRICA|2026-10|horas_noturnas|2347.501253598|2348.063069312|+0.561815714|1|4
DONO|2026-10|horas_noturnas|BATIDA|1.015868889|1.160993016|+0.145124127|1|1
DONO|2026-10|horas_noturnas|CADASTRO|0.939963611|1.074244127|+0.134280516|1|1
DONO|2026-10|horas_noturnas|ESTRUTURA|1.976877500|2.259288571|+0.282411071|1|2
RESIDUO|2026-10|horas_noturnas|ATE_LIMIAR|2343.568543598|2343.568543598|-0.000000000|89|1322
RESIDUO|2026-10|horas_noturnas|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|61|356
RESIDUO|2026-10|horas_noturnas|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|155
INSTRUMENTO_HISTORICO|2026-10|horas_noturnas|v2_h=2300.755510939|v3_h=2348.063069312|delta_h=+47.307558373
PARCIAL|2026-10|horas_atraso|v2_h=74.335833333333326|v3_h=76.66578148638888|colabs_div=1|dias_colab_div=1
RUBRICA|2026-10|horas_atraso|74.335833333|76.665781486|+2.329948153|1|1
DONO|2026-10|horas_atraso|ESTRUTURA|0.000000000|2.330333333|+2.330333333|1|1
RESIDUO|2026-10|horas_atraso|ATE_LIMIAR|74.335833333|74.335448153|-0.000385180|88|1312
RESIDUO|2026-10|horas_atraso|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|61|369
RESIDUO|2026-10|horas_atraso|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|155
INSTRUMENTO_HISTORICO|2026-10|horas_atraso|v2_h=74.350166667|v3_h=74.349781486|delta_h=-0.000385180
PARCIAL|2026-10|horas_saida_antecipada|v2_h=46.246166666666667|v3_h=46.246252456944447|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_saida_antecipada|46.246166667|46.246252457|+0.000085790|0|0
RESIDUO|2026-10|horas_saida_antecipada|ATE_LIMIAR|46.246166667|46.246252457|+0.000085790|88|1313
RESIDUO|2026-10|horas_saida_antecipada|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|61|369
RESIDUO|2026-10|horas_saida_antecipada|SEM_PAR|0.000000000|0.000000000|+0.000000000|9|155
INSTRUMENTO_HISTORICO|2026-10|horas_saida_antecipada|v2_h=32.592833333|v3_h=32.592919124|delta_h=+0.000085790
UNIVERSO|2026-11|colabs_porta=578|segundos=102.2
UNIVERSO_PARTE 2026-11 99
RUBRICA|competencia|rubrica|v2_h|v3_h|delta_h|colabs_div|dias_colab_div
DONO|competencia|rubrica|dono|v2_h|v3_h|delta_h|colabs|dias_colab
DIA|2026-11|horas_trabalhadas|col82|2026-11-05|CADASTRO|7.463548611|6.085836944|-1.377711667
DIA|2026-11|horas_intra_indenizada|col82|2026-10-25|CADASTRO|0.000000000|0.204860556|+0.204860556
DIA|2026-11|horas_intra_indenizada|col82|2026-10-27|CADASTRO|1.166666667|0.000000000|-1.166666667
DIA|2026-11|horas_intra_indenizada|col82|2026-10-28|CADASTRO|1.166666667|0.000000000|-1.166666667
DIA|2026-11|horas_intra_indenizada|col82|2026-10-29|CADASTRO|1.166666667|0.000000000|-1.166666667
DIA|2026-11|horas_intra_indenizada|col82|2026-11-03|CADASTRO|1.166666667|0.000000000|-1.166666667
DIA|2026-11|horas_intra_indenizada|col82|2026-11-04|CADASTRO|1.166666667|0.000000000|-1.166666667
DIA|2026-11|horas_atraso|col214|2026-11-01|CADASTRO|0.000000000|0.076333333|+0.076333333
DIA|2026-11|horas_saida_antecipada|col214|2026-11-01|CADASTRO|0.951166667|0.874833333|-0.076333333
DIA|2026-11|horas_trabalhadas|col592|2026-10-29|CADASTRO|7.452715833|4.403602222|-3.049113611
DIA|2026-11|horas_intra_indenizada|col592|2026-10-31|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-11|horas_extras_50|col592|2026-10-31|CADASTRO|0.000000000|0.272015833|+0.272015833
DIA|2026-11|horas_intra_indenizada|col922|2026-10-25|BATIDA|1.000000000|0.000000000|-1.000000000
DIA|2026-11|horas_intra_indenizada|col922|2026-10-27|BATIDA|1.000000000|0.000000000|-1.000000000
DIA|2026-11|horas_intra_indenizada|col922|2026-11-02|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-11|horas_extras_50|col922|2026-11-02|CADASTRO|2.000000000|0.000000000|-2.000000000
DIA|2026-11|horas_extras_100|col922|2026-10-25|BATIDA|6.169508333|11.494480833|+5.324972500
DIA|2026-11|horas_extras_100|col922|2026-10-27|BATIDA|6.070659167|10.791530000|+4.720870833
DIA|2026-11|horas_extras_100|col922|2026-11-02|CADASTRO|10.745034167|20.745034167|+10.000000000
DIA|2026-11|horas_extras_100_feriado|col922|2026-11-02|CADASTRO|4.701487222|20.745034167|+16.043546944
DIA|2026-11|horas_trabalhadas|col988|2026-10-22|CADASTRO|7.043500000|3.347891111|-3.695608889
DIA|2026-11|horas_trabalhadas|col988|2026-10-23|CADASTRO|6.947509637|2.004634167|-4.942875471
DIA|2026-11|horas_trabalhadas|col988|2026-10-29|CADASTRO|8.608185508|4.593460833|-4.014724674
DIA|2026-11|horas_extras_50|col988|2026-10-29|CADASTRO|1.608185508|0.000000000|-1.608185508
PARCIAL|2026-11|horas_trabalhadas|v2_h=6877.6199801902776|v3_h=6860.5399458783331|colabs_div=3|dias_colab_div=5
RUBRICA|2026-11|horas_trabalhadas|6877.619980190|6860.539945878|-17.080034312|3|5
DONO|2026-11|horas_trabalhadas|CADASTRO|37.515459590|20.435425278|-17.080034312|3|5
RESIDUO|2026-11|horas_trabalhadas|ATE_LIMIAR|6840.104520601|6840.104520601|-0.000000000|86|726
RESIDUO|2026-11|horas_trabalhadas|SEM_DECISAO|49.509477702|49.509477702|+0.000000000|86|967
RESIDUO|2026-11|horas_trabalhadas|SEM_PAR|0.000000000|0.000000000|+0.000000000|11|239
INSTRUMENTO_HISTORICO|2026-11|horas_trabalhadas|v2_h=6715.239535224|v3_h=6860.539945878|delta_h=+145.300410654
PARCIAL|2026-11|horas_intra_indenizada|v2_h=234.28587616805555|v3_h=224.65740339027778|colabs_div=3|dias_colab_div=10
RUBRICA|2026-11|horas_intra_indenizada|234.285876168|224.657403390|-9.628472778|3|10
DONO|2026-11|horas_intra_indenizada|BATIDA|2.000000000|0.000000000|-2.000000000|1|2
DONO|2026-11|horas_intra_indenizada|CADASTRO|7.833333333|0.204860556|-7.628472778|3|8
RESIDUO|2026-11|horas_intra_indenizada|ATE_LIMIAR|224.452542835|224.452542835|+0.000000000|86|721
RESIDUO|2026-11|horas_intra_indenizada|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|86|967
RESIDUO|2026-11|horas_intra_indenizada|SEM_PAR|0.000000000|0.000000000|+0.000000000|11|239
INSTRUMENTO_HISTORICO|2026-11|horas_intra_indenizada|v2_h=217.285876168|v3_h=224.657403390|delta_h=+7.371527222
PARCIAL|2026-11|horas_extras_50|v2_h=9.5188135633333317|v3_h=6.1826438888888884|colabs_div=3|dias_colab_div=3
RUBRICA|2026-11|horas_extras_50|9.518813563|6.182643889|-3.336169674|3|3
DONO|2026-11|horas_extras_50|CADASTRO|3.608185508|0.272015833|-3.336169674|3|3
RESIDUO|2026-11|horas_extras_50|ATE_LIMIAR|5.910628056|5.910628056|-0.000000000|84|705
RESIDUO|2026-11|horas_extras_50|SEM_DECISAO|0.714590833|0.714590833|+0.000000000|86|990
RESIDUO|2026-11|horas_extras_50|SEM_PAR|0.000000000|0.000000000|+0.000000000|11|239
INSTRUMENTO_HISTORICO|2026-11|horas_extras_50|v2_h=10.233404397|v3_h=6.182643889|delta_h=-4.050760508
PARCIAL|2026-11|horas_extras_100|v2_h=80.016508748333337|v3_h=100.06235208166666|colabs_div=1|dias_colab_div=3
RUBRICA|2026-11|horas_extras_100|80.016508748|100.062352082|+20.045843333|1|3
DONO|2026-11|horas_extras_100|BATIDA|12.240167500|22.286010833|+10.045843333|1|2
DONO|2026-11|horas_extras_100|CADASTRO|10.745034167|20.745034167|+10.000000000|1|1
RESIDUO|2026-11|horas_extras_100|ATE_LIMIAR|57.031307082|57.031307082|+0.000000000|84|705
RESIDUO|2026-11|horas_extras_100|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|86|990
RESIDUO|2026-11|horas_extras_100|SEM_PAR|0.000000000|0.000000000|+0.000000000|11|239
INSTRUMENTO_HISTORICO|2026-11|horas_extras_100|v2_h=80.016508748|v3_h=100.062352082|delta_h=+20.045843333
PARCIAL|2026-11|horas_extras_100_feriado|v2_h=61.732794303888888|v3_h=77.776341248333324|colabs_div=1|dias_colab_div=1
RUBRICA|2026-11|horas_extras_100_feriado|61.732794304|77.776341248|+16.043546944|1|1
DONO|2026-11|horas_extras_100_feriado|CADASTRO|4.701487222|20.745034167|+16.043546944|1|1
RESIDUO|2026-11|horas_extras_100_feriado|ATE_LIMIAR|57.031307082|57.031307082|+0.000000000|84|707
RESIDUO|2026-11|horas_extras_100_feriado|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|86|990
RESIDUO|2026-11|horas_extras_100_feriado|SEM_PAR|0.000000000|0.000000000|+0.000000000|11|239
INSTRUMENTO_HISTORICO|2026-11|horas_extras_100_feriado|v2_h=0.000000000|v3_h=77.776341248|delta_h=+77.776341248
PARCIAL|2026-11|horas_extras_50_noturna|v2_h=1.3930904940507469|v3_h=1.3930904940507469|colabs_div=0|dias_colab_div=0
RUBRICA|2026-11|horas_extras_50_noturna|1.393090494|1.393090494|+0.000000000|0|0
RESIDUO|2026-11|horas_extras_50_noturna|ATE_LIMIAR|1.393090494|1.393090494|+0.000000000|84|708
RESIDUO|2026-11|horas_extras_50_noturna|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|86|990
RESIDUO|2026-11|horas_extras_50_noturna|SEM_PAR|0.000000000|0.000000000|+0.000000000|11|239
INSTRUMENTO_HISTORICO|2026-11|horas_extras_50_noturna|v2_h=0.000000000|v3_h=1.393090494|delta_h=+1.393090494
PARCIAL|2026-11|horas_extras_100_noturna|v2_h=11.701480339282588|v3_h=11.703889505949252|colabs_div=0|dias_colab_div=0
RUBRICA|2026-11|horas_extras_100_noturna|11.701480339|11.703889506|+0.002409167|0|0
RESIDUO|2026-11|horas_extras_100_noturna|ATE_LIMIAR|11.701480339|11.703889506|+0.002409167|84|708
RESIDUO|2026-11|horas_extras_100_noturna|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|86|990
RESIDUO|2026-11|horas_extras_100_noturna|SEM_PAR|0.000000000|0.000000000|+0.000000000|11|239
INSTRUMENTO_HISTORICO|2026-11|horas_extras_100_noturna|v2_h=0.000000000|v3_h=11.703889506|delta_h=+11.703889506
PARCIAL|2026-11|horas_folga_trabalhada|v2_h=0|v3_h=0|colabs_div=0|dias_colab_div=0
RUBRICA|2026-11|horas_folga_trabalhada|0.000000000|0.000000000|+0.000000000|0|0
RESIDUO|2026-11|horas_folga_trabalhada|ATE_LIMIAR|0.000000000|0.000000000|+0.000000000|86|731
RESIDUO|2026-11|horas_folga_trabalhada|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|86|967
RESIDUO|2026-11|horas_folga_trabalhada|SEM_PAR|0.000000000|0.000000000|+0.000000000|11|239
INSTRUMENTO_HISTORICO|2026-11|horas_folga_trabalhada|v2_h=211.889922669|v3_h=0.000000000|delta_h=-211.889922669
PARCIAL|2026-11|horas_noturnas|v2_h=1293.2316784055952|v3_h=1293.2316784055952|colabs_div=0|dias_colab_div=0
RUBRICA|2026-11|horas_noturnas|1293.231678406|1293.231678406|+0.000000000|0|0
RESIDUO|2026-11|horas_noturnas|ATE_LIMIAR|1293.231678406|1293.231678406|+0.000000000|86|728
RESIDUO|2026-11|horas_noturnas|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|86|970
RESIDUO|2026-11|horas_noturnas|SEM_PAR|0.000000000|0.000000000|+0.000000000|11|239
INSTRUMENTO_HISTORICO|2026-11|horas_noturnas|v2_h=1180.231678406|v3_h=1293.231678406|delta_h=+113.000000000
PARCIAL|2026-11|horas_atraso|v2_h=38.779833333333336|v3_h=38.856047777777782|colabs_div=1|dias_colab_div=1
RUBRICA|2026-11|horas_atraso|38.779833333|38.856047778|+0.076214444|1|1
DONO|2026-11|horas_atraso|CADASTRO|0.000000000|0.076333333|+0.076333333|1|1
RESIDUO|2026-11|horas_atraso|ATE_LIMIAR|38.779833333|38.779714444|-0.000118889|84|707
RESIDUO|2026-11|horas_atraso|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|86|990
RESIDUO|2026-11|horas_atraso|SEM_PAR|0.000000000|0.000000000|+0.000000000|11|239
INSTRUMENTO_HISTORICO|2026-11|horas_atraso|v2_h=38.779833333|v3_h=38.856047778|delta_h=+0.076214444
PARCIAL|2026-11|horas_saida_antecipada|v2_h=21.576166666666666|v3_h=21.49984737777778|colabs_div=1|dias_colab_div=1
RUBRICA|2026-11|horas_saida_antecipada|21.576166667|21.499847378|-0.076319289|1|1
DONO|2026-11|horas_saida_antecipada|CADASTRO|0.951166667|0.874833333|-0.076333333|1|1
RESIDUO|2026-11|horas_saida_antecipada|ATE_LIMIAR|20.625000000|20.625014044|+0.000014044|84|707
RESIDUO|2026-11|horas_saida_antecipada|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|86|990
RESIDUO|2026-11|horas_saida_antecipada|SEM_PAR|0.000000000|0.000000000|+0.000000000|11|239
INSTRUMENTO_HISTORICO|2026-11|horas_saida_antecipada|v2_h=21.576166667|v3_h=21.499847378|delta_h=-0.076319289
ERRO 2026-10 app/ponto/services/dia_pago.py:315 ValueError 11 exemplos=[('col268', '-'), ('col298', '-'), ('col370', '-'), ('col400', '-'), ('col574', '-')]
ERRO 2026-10 sonda ConservacaoNaoFecha 99 exemplos=[('col316', '-'), ('col430', '-'), ('col460', '-'), ('col760', '-'), ('col862', '-')]
ERRO 2026-10 sonda LacunaDeMedicao 196 exemplos=[('col268', '-'), ('col268', '2026-09-22'), ('col268', '2026-09-24'), ('col268', '2026-09-26'), ('col268', '2026-09-28')]
ERRO 2026-11 app/ponto/services/dia_pago.py:315 ValueError 12 exemplos=[('col76', '-'), ('col106', '-'), ('col142', '-'), ('col220', '-'), ('col268', '-')]
ERRO 2026-11 sonda ConservacaoNaoFecha 22 exemplos=[('col76', '-'), ('col946', '-')]
ERRO 2026-11 sonda LacunaDeMedicao 256 exemplos=[('col76', '-'), ('col106', '-'), ('col106', '2026-10-21'), ('col106', '2026-10-22'), ('co

[truncado em 20000 de 29038 caracteres]

_2301 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
