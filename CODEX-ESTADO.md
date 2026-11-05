# CODEX-ESTADO

> Ultima resposta processada: **05/11/2026 15:34** · publicado em **05/11/2026 15:34** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 05/11 15:34 · sessao 276c86da9dbb · BATERIA-QUEIXAS-ADMIN · leitura · origem=Code

**Q4 — casos pela regra, antes do código.** Os cadastros e as batidas abaixo são cenários propostos, sem execução nem observação de produção. [não verificado] O gabarito segue a regra aplicada ao cadastro e às batidas, conforme L-110. (`app/docs/LEIS.md:136`)

**Convenções das tabelas**

- Valores em minutos: **P/T/F/A/N/HE** = previsto / trabalhado computado / falta decidida / abono / noturno / hora extra. Noturno é uma parcela do trabalhado, com adicional próprio; não se soma novamente ao tempo trabalhado. (`app/docs/LEIS.md:84`; `app/docs/CORTES.md:418`)
- **D = 03/11/2026**, na competência aberta de uma empresa cadastrada com corte no dia 21; **R = 15/09/2026**, anterior ao horizonte 10. Datas escolhidas para os cenários. [não verificado] A janela deve vir do cadastro da empresa; L-124 preserva a fotografia anterior ao horizonte até fato novo. (`app/docs/LEIS.md:77`; `app/docs/LEIS.md:145`)
- Horários locais UTC−3. No noturno, “+1” identifica o dia seguinte, sem transferir a jornada para ele. (`app/docs/LEIS.md:107`; `app/docs/LEIS.md:117`)
- Os cenários são independentes. Nos casos de troca, o cadastro anterior está comprovadamente errado desde D: a correção usa a exceção formal de L-020, com vigência declarada, denúncia na célula e trilha. D−1 conserva seu cadastro. (`app/docs/LEIS.md:71`; `app/docs/LEIS.md:72`; `app/docs/LEIS.md:150`)

A coluna **leitores** usa estes contratos:

| Código | Espelho, app, célula, DiaPago e chamado |
|---|---|
| **V** | Espelho e app apresentam a tupla indicada; célula mantém o previsto/marcos e a lavratura correspondente; DiaPago representa os mesmos valores. Chamado/pergunta lê o lastro atual da célula, sem cálculo próprio. (`app/docs/LEIS.md:60`; `app/docs/LEIS.md:95`; `app/docs/LEIS.md:113`; `app/docs/GLOSSARIO.md:22`) |
| **VC** | V após a correção. A marcação original permanece no histórico como retratada e sai da apuração; nenhuma cobrança pode continuar sustentada exclusivamente por ela. Isso não determina o nome literal do status do chamado. (`app/docs/LEIS.md:76`; `app/docs/CORTES.md:182`; `app/docs/CORTES.md:316`) |
| **VR** | Antes do fato novo, todos representam a fotografia; depois dele, representam o novo julgamento. TXT anterior permanece preservado. (`app/docs/LEIS.md:145`; `app/docs/LEIS.md:149`) |
| **VA** | V com ausência aprovada cobrindo integralmente o dia; os marcos cobertos não sustentam cobrança de batida faltante. (`app/docs/LEIS.md:95`; `app/docs/LEIS.md:120`; `app/docs/LEIS.md:121`; `app/docs/CORTES.md:319`) |
| **VF** | V com falta explicitamente decidida; deve aparecer como falta, sem continuar apresentada como mero furo indecidido. (`app/docs/LEIS.md:87`; `app/docs/CORTES.md:179`) |

**6x1 — 44h semanais, 7h20 por dia, intervalo de 1h**

Cadastro-base: segunda a sábado, **08:00–12:00 / 13:00–16:20**, domingo de folga; P diário = 440 e semanal = 2.640. O intervalo cadastrado fica fora da jornada. (`app/docs/LEIS.md:75`; `app/docs/LEIS.md:80`)

| id | cadastro | batidas | lançamentos | resposta da regra | regra citada | leitores |
|---|---|---|---|---|---|---|
| **6-1 Cadastro** | Base vigente em D | 08:00; 12:00; 13:00; 16:20 | 1. Cadastrar vínculo e grade; 2. registrar batidas; 3. apurar | **440/440/0/0/0/0**. Trabalho: 240 + 200 = 440. | L-024/L-032: `app/docs/LEIS.md:75`; `app/docs/LEIS.md:80` | **V**; nenhum furo desses quatro marcos. |
| **6-2 Troca na aberta** | Errado: 08–12/13–16, P420. Correto desde D: base, P440 | 08:00; 12:00; 13:00; 16:20 | 1. Declarar correção desde D; 2. encerrar anterior em D−1; 3. regenerar pela exceção; 4. rejulgar competência | **440/440/0/0/0/0**. Os 20 minutos entre 16:00 e 16:20 passam a integrar a jornada prevista; não ficam como HE pendente herdada do cadastro errado. | L-020/L-024/L-097/L-112: `app/docs/LEIS.md:71`; `app/docs/LEIS.md:75`; `app/docs/LEIS.md:115`; `app/docs/LEIS.md:150`; `app/docs/CORTES.md:229` | **V**; célula denuncia correção; chamado usa os novos marcos. |
| **6-3 Correção de batida** | Base | 08:00; 12:00; 13:00; saída errada 15:50 | 1. Retratar 15:50 com motivo; 2. registrar saída correta 16:20; 3. rejulgar | Antes: **440/410/0/0/0/0**, antecipada 30. Depois: **440/440/0/0/0/0**, antecipada 0. Os 30 minutos não eram falta decidida. | L-025/L-039/L-093: `app/docs/LEIS.md:76`; `app/docs/LEIS.md:87`; `app/docs/LEIS.md:111`; `app/docs/CORTES.md:182` | **VC**; 15:50 retratada continua visível no histórico. |
| **6-4 Retroativo L-124** | Base em R; fotografia inicial **440/410/0/0/0/0** | R: 08:00; 12:00; 13:00; 15:50 equivocada | 1. Pousar obra nova sem fato novo; 2. conferir fotografia; 3. corrigir saída para 16:20 pela retratação; 4. rejulgar | Após 1: permanece **440/410/0/0/0/0**. Após 3–4: **440/440/0/0/0/0**. Não reprocessar os demais dias antigos por consequência desse ato. | L-124/L-113: `app/docs/LEIS.md:145`; `app/docs/LEIS.md:149`; `app/docs/CORTES.md:419` | **VR + VC**. |
| **6-5 Lote e folgas** | Pessoas X/Y/Z passam à base desde D. Y tinha folga de calendário do vínculo anterior; Z tem folga avulsa pessoal em D | X: caso 6-3; Y: quatro batidas completas da base; Z: nenhuma | 1. Corrigir vínculos; 2. corrigir saída de X; 3. processar lote; 4. repetir os mesmos atos | X = **440/440/0/0/0/0**; Y = igual; Z = **0/0/0/0/0/0**. Agregado: **880/880/0/0/0/0**, igual na repetição. Folga antiga de Y não acompanha o vínculo; a avulsa de Z sobrevive. | `app/docs/CORTES.md:417`; `app/docs/CORTES.md:229`; L-007/L-110/L-114: `app/docs/LEIS.md:65`; `app/docs/LEIS.md:136`; `app/docs/LEIS.md:148` | **VC** para X; **V** para Y/Z; Z sem cobrança de trabalho previsto. |
| **6-6 Abono integral** | Base; D previsto | Nenhuma | 1. Lançar ausência integral abonadora; 2. aprovar; 3. rejulgar | **440/0/0/440/0/0**. Abono cobre os 440 previstos não trabalhados. | L-088/L-089: `app/docs/LEIS.md:120`; `app/docs/LEIS.md:121`; `app/docs/CORTES.md:88` | **VA**. |
| **6-7 Falta decidida** | Base; D previsto | Nenhuma | 1. Lançar ausência integral descontável; 2. aprovar decisão; 3. rejulgar | **440/0/440/0/0/0**. Antes da decisão, ausência de batidas não basta para fabricar essa falta. | L-039/L-088: `app/docs/LEIS.md:87`; `app/docs/LEIS.md:120`; `app/docs/CORTES.md:179` | **VF**. |

**12x36 diurno — 07:00–19:00, intervalo de 1h**

Cadastro-base: **07:00–12:00 / 13:00–19:00**, com foto/paridade declarando D como trabalho e D+1 como folga. P = 720 − 60 = **660**, sem presumir que o nome “12x36” representa 720 minutos trabalhados. (`app/docs/LEIS.md:73`; `app/docs/LEIS.md:75`; `app/docs/LEIS.md:80`; `app/docs/LEIS.md:124`)

| id | cadastro | batidas | lançamentos | resposta da regra | regra citada | leitores |
|---|---|---|---|---|---|---|
| **D-1 Cadastro** | Base | 07:00; 12:00; 13:00; 19:00 | 1. Cadastrar vínculo, foto e fase; 2. registrar batidas; 3. apurar | **660/660/0/0/0/0**. Trabalho: 300 + 360 = 660. | L-022/L-024/L-032: `app/docs/LEIS.md:73`; `app/docs/LEIS.md:75`; `app/docs/LEIS.md:80` | **V**; quatro marcos cumpridos. |
| **D-2 Troca na aberta** | Errado: 6x1 P440. Correto desde D: base 12x36, P660; descanso entre jornadas preservado | 07:00; 12:00; 13:00; 19:00 | 1. Declarar início/fase D; 2. encerrar anterior D−1; 3. regenerar formalmente; 4. rejulgar | **660/660/0/0/0/0**. A diferença de 220 para o cadastro antigo integra o novo previsto; não vira HE pela diferença entre escalas. | L-020/L-024/L-097/L-112: `app/docs/LEIS.md:71`; `app/docs/LEIS.md:75`; `app/docs/LEIS.md:115`; `app/docs/LEIS.md:150` | **V**; célula denuncia a correção; chamado perde premissas exclusivas da escala anterior. |
| **D-3 Correção de batida** | Base | 07:00; 12:00; 13:00; saída errada 18:30 | 1. Retratar 18:30; 2. registrar 19:00; 3. rejulgar | Antes: **660/630/0/0/0/0**, antecipada 30. Depois: **660/660/0/0/0/0**, antecipada 0. | L-025/L-039/L-093: `app/docs/LEIS.md:76`; `app/docs/LEIS.md:87`; `app/docs/LEIS.md:111`; `app/docs/CORTES.md:182` | **VC**. |
| **D-4 Retroativo L-124** | Base em R; foto **660/630/0/0/0/0** | R: 07:00; 12:00; 13:00; 18:30 equivocada | 1. Pousar sem fato novo; 2. conferir foto; 3. corrigir saída para 19:00; 4. rejulgar R | Após 1: **660/630/0/0/0/0**. Após correção: **660/660/0/0/0/0**. Demais fotografias antigas preservadas. | L-124/L-113: `app/docs/LEIS.md:145`; `app/docs/LEIS.md:149`; `app/docs/CORTES.md:419` | **VR + VC**. |
| **D-5 Lote e folgas** | X/Y/Z na base; Y com folga do calendário antigo; Z com folga avulsa pessoal D | X: caso D-3; Y: 07:00; 12:00; 13:00; 19:00; Z: nenhuma | 1. Corrigir vínculos; 2. corrigir X; 3. processar lote; 4. repetir | X/Y: cada **660/660/0/0/0/0**; Z: **0/0/0/0/0/0**. Agregado **1320/1320/0/0/0/0**, estável na repetição. | `app/docs/CORTES.md:417`; `app/docs/CORTES.md:229`; `app/docs/LEIS.md:65`; `app/docs/LEIS.md:136`; `app/docs/LEIS.md:148` | **VC** para X; **V** para Y/Z; folga pessoal de Z permanece. |
| **D-6 Abono integral** | Base; D de trabalho | Nenhuma | 1. Lançar ausência abonadora integral; 2. aprovar; 3. rejulgar | **660/0/0/660/0/0**. Abono corresponde ao previsto líquido do dia. | `app/docs/LEIS.md:120`; `app/docs/LEIS.md:121`; `app/docs/CORTES.md:88` | **VA**. |
| **D-7 Falta decidida** | Base; D de trabalho | Nenhuma | 1. Lançar ausência descontável integral; 2. aprovar; 3. rejulgar | **660/0/660/0/0/0**. Não descontar 720: o intervalo não integra o previsto trabalhável. | `app/docs/LEIS.md:80`; `app/docs/LEIS.md:87`; `app/docs/LEIS.md:120` | **VF**. |

**12x36 noturno — 19:00–07:00, intervalo de 1h, CCT Londrina**

Cadastro-base: CCT Vigilantes Londrina aplicável e vigente, **19:00–01:00 / 02:00–07:00**, foto/fase explícita; prorrogação pós-05h **desmarcada**. A cl.38-d determina hora de 60 minutos e mantém adicional de 20%; a prorrogação depende de cadastro separado. Assim, P/T completos = **660** e N = 22–05 menos pausa 01–02 = **360**. (`app/core/regua_cct.py:129`; `app/core/regua_cct.py:136`; `app/core/regua_cct.py:166`; `app/docs/LEIS.md:83`; `app/docs/LEIS.md:84`)

| id | cadastro | batidas | lançamentos | resposta da regra | regra citada | leitores |
|---|---|---|---|---|---|---|
| **N-1 Cadastro** | Base; pós-05h desmarcada | D 19:00; +1 01:00; 02:00; 07:00 | 1. Cadastrar vínculo/fase/CCT; 2. registrar batidas; 3. apurar após encerrar turno | **660/660/0/0/360/0**. São 300 minutos diurnos + 360 noturnos; adicional de 20% sobre N, sem conversão por 52min30s. Tudo pertence a D. | L-032/L-035/L-036/L-073/L-085: `app/docs/LEIS.md:80`; `app/docs/LEIS.md:83`; `app/docs/LEIS.md:84`; `app/docs/LEIS.md:100`; `app/docs/LEIS.md:117` | **V**; +1 não ganha outro dia trabalhado. |
| **N-2 Troca na aberta** | Errado: diurno 07–19. Correto desde D: base noturna; último turno anterior suficientemente afastado para preservar descanso | D 19:00; +1 01:00; 02:00; 07:00 | 1. Declarar vigência/fase correta; 2. encerrar vínculo anterior; 3. regenerar formalmente; 4. rejulgar | **660/660/0/0/360/0**. P conserva 660, mas os marcos mudam; corrigir não cria outro turno no dia seguinte nem HE por comparação com os marcos diurnos. | `app/docs/LEIS.md:71`; `app/docs/LEIS.md:75`; `app/docs/LEIS.md:115`; `app/docs/LEIS.md:117`; `app/docs/LEIS.md:150` | **V**; célula denuncia a correção; cobrança acompanha os marcos noturnos. |
| **N-3 Correção de batida** | Base | D 19:00; +1 01:00; 02:00; saída errada 06:30 | 1. Retratar 06:30; 2. registrar 07:00; 3. rejulgar D | Antes: **660/630/0/0/360/0**, antecipada 30. Depois: **660/660/0/0/360/0**. N permanece 360 porque os 30 corrigidos são após 05h e a prorrogação está desmarcada. | `app/docs/LEIS.md:76`; `app/docs/LEIS.md:83`; `app/docs/LEIS.md:87`; `app/docs/LEIS.md:111`; `app/docs/CORTES.md:182` | **VC**; histórico de +1 permanece associado ao turno D. |
| **N-4 Retroativo L-124** | Base em R; foto **660/630/0/0/360/0** | R 19:00; +1 01:00; 02:00; 06:30 equivocada | 1. Pousar sem fato novo; 2. conferir foto; 3. corrigir saída para 07:00; 4. rejulgar R | Após pouso: fotografia **660/630/0/0/360/0**. Após fato novo: **660/660/0/0/360/0**. | `app/docs/LEIS.md:145`; `app/docs/LEIS.md:149`; `app/docs/CORTES.md:419` | **VR + VC**; se exportada, conservar TXT anterior. |
| **N-5 Lote e folgas** | X/Y/Z na base; Y com folga do calendário antigo; Z com folga avulsa pessoal em D | X: caso N-3; Y: 19:00/+1 01:00/02:00/07:00; Z: nenhuma | 1. Corrigir vínculos; 2. corrigir X; 3. processar lote; 4. repetir | X/Y: cada **660/660/0/0/360/0**; Z: zeros. Agregado **1320/1320/0/0/720/0**, sem duplicação na repetição nem no dia +1. | `app/docs/CORTES.md:417`; `app/docs/CORTES.md:229`; `app/docs/LEIS.md:117`; `app/docs/LEIS.md:136`; `app/docs/LEIS.md:148` | **VC** para X; **V** para Y/Z. |
| **N-6 Abono integral** | Base; D de trabalho | Nenhuma | 1. Lançar ausência abonadora cobrindo o turno D19–+1 07; 2. aprovar; 3. rejulgar | **660/0/0/660/0/0**. Abono de 660; nenhum minuto noturno efetivamente trabalhado. Eventual reflexo remuneratório do adicional habitual exige regra específica: **lei:** [não verificado]. | `app/docs/LEIS.md:120`; `app/docs/LEIS.md:121`; `app/docs/CORTES.md:330` | **VA**; cobertura pertence ao turno D. |
| **N-7 Falta decidida** | Base; D de trabalho | Nenhuma | 1. Lançar ausência integral descontável do turno; 2. aprovar; 3. rejulgar | **660/0/660/0/0/0**. Uma falta do turno D; nenhuma segunda falta em +1. | `app/docs/LEIS.md:87`; `app/docs/LEIS.md:117`; `app/docs/LEIS.md:120` | **VF**. |
| **N-8 Prorrogação declarada** | Igual à base, mas pós-05h **marcada**, vigente em D | D 19:00; +1 01:00; 02:00; 07:00 | 1. Declarar aplicação/vigência do cadastro; 2. apurar D | **660/660/0/0/480/0**. N = 22–07, 540 − 60 de pausa = 480; diurno 180. Hora permanece de 60 minutos e adicional de 20%. | `app/docs/LEIS.md:83`; `app/docs/LEIS.md:84`; `app/docs/CORTES.md:418` | **V**; todos apresentam N480, conforme esse cadastro. |

**Personalizado — 6h corridas, sem intervalo cadastrado**

Cadastro-base solicitado: **08:00–14:00**, intervalo zero, P360. A grade decide esse previsto; não se inventa pausa cadastral para reduzir o trabalho observado. (`app/docs/LEIS.md:75`; `app/docs/LEIS.md:80`; `app/docs/LEIS.md:124`)

Há uma ressalva jurídica concreta: para empregado urbano sujeito à regra geral, seis horas de trabalho exigem intervalo de 15 minutos; sua supressão gera indenização do período suprimido com acréscimo de 50%. Portanto, **360 trabalhados não viram 345**, e a indenização é separada de HE. Fonte: [CLT, art.71, §§1º, 2º e 4º — Câmara](https://www2.camara.leg.br/legin/fed/declei/1940-1949/decreto-lei-5452-1-maio-1943-415500-normaatualizada-pe.html). Lastro integral desses parágrafos em arquivo local: [não verificado].

Abaixo, **J15** significa essa indenização de 15 minutos, separada da tupla. **lei:** os textos locais consultados não definem a representação dessa combinação “intervalo cadastrado zero + mínimo legal de 15” em cada leitor. Não considerar o cadastro juridicamente regular apenas porque P/T fecham. (`app/docs/LEIS.md:79`; `app/docs/LEIS.md:80`; `app/docs/CORTES.md:411`; [não verificado])

| id | cadastro | batidas | lançamentos | resposta da regra | regra citada | leitores |
|---|---|---|---|---|---|---|
| **P-1 Cadastro** | Base 08–14; intervalo zero | 08:00; 14:00 | 1. Cadastrar grade; 2. registrar batidas; 3. apurar | **360/360/0/0/0/0**, mais **J15**. Não deduzir pausa fictícia nem classificar a indenização como HE. | `app/docs/LEIS.md:75`; `app/docs/LEIS.md:124`; CLT art.71 §§1º/4º, fonte acima; lastro local [não verificado] | **V** para a tupla; J15 separado, com a lacuna de representação declarada acima. |
| **P-2 Troca na aberta** | Errado: 6x1 P440 com intervalo1h. Correto desde D: 08–14, intervalo0, P360 | 08:00; 14:00 | 1. Declarar correção desde D; 2. encerrar anterior D−1; 3. regenerar formalmente; 4. rejulgar | **360/360/0/0/0/0**, mais J15. Não conservar o desconto de 1h nem cobrar os 80 minutos do previsto anterior. | `app/docs/LEIS.md:71`; `app/docs/LEIS.md:75`; `app/docs/LEIS.md:80`; `app/docs/LEIS.md:150`; CLT art.71, lastro local [não verificado] | **V**; célula denuncia mudança; chamado usa apenas os marcos 08/14. |
| **P-3 Correção de batida** | Base | 08:00; saída errada 13:30 | 1. Retratar 13:30; 2. registrar 14:00; 3. rejulgar | Antes: **360/330/0/0/0/0**, antecipada30. Depois: **360/360/0/0/0/0**, antecipada0. J15 nos dois cenários de trabalho contínuo sem pausa. | `app/docs/LEIS.md:76`; `app/docs/LEIS.md:87`; `app/docs/LEIS.md:111`; `app/docs/CORTES.md:182`; CLT art.71, lastro local [não verificado] | **VC**; não manter 330 em DiaPago após apresentar 360 no espelho. |
| **P-4 Retroativo L-124** | Base em R; foto **360/330/0/0/0/0**, J15 | R 08:00; 13:30 equivocada | 1. Pousar sem fato novo; 2. conferir foto; 3. corrigir saída para14:00; 4. rejulgar | Só pouso mantém **360/330/0/0/0/0**. Correção leva a **360/360/0/0/0/0**; J15 separado. | `app/docs/LEIS.md:145`; `app/docs/LEIS.md:149`; `app/docs/CORTES.md:419`; CLT art.71, lastro local [não verificado] | **VR + VC**. |
| **P-5 Lote e folgas** | X/Y/Z na base; Y com folga do calendário anterior; Z com folga avulsa pessoal D | X: caso P-3; Y:08:00/14:00; Z:nenhuma | 1. Corrigir vínculos; 2. corrigir X; 3. processar lote; 4. repetir | X/Y: cada **360/360/0/0/0/0**, J15; Z: zeros, J0. Agregado **720/720/0/0/0/0**, J30; repetição conserva os valores. | `app/docs/CORTES.md:417`; `app/docs/CORTES.md:229`; `app/docs/LEIS.md:136`; `app/docs/LEIS.md:148`; CLT art.71, lastro local [não verificado] | **VC** para X; **V** para Y/Z; indenização separada, sem duplicação. |
| **P-6 Abono integral** | Base; D previsto | Nenhuma | 1. Lançar ausência integral abonadora; 2. aprovar; 3. rejulgar | **360/0/0/360/0/0**, J0: não houve trabalho com intervalo suprimido. | `app/docs/LEIS.md:120`; `app/docs/LEIS.md:121`; `app/docs/CORTES.md:88`; CLT art.71, lastro local [não verificado] | **VA**. |
| **P-7 Falta decidida** | Base; D previsto | Nenhuma | 1. Lançar ausência integral descontável; 2. aprovar; 3. rejulgar | **360/0/360/0/0/0**, J0. Desconto corresponde aos 360 previstos, sem acrescentar intervalo inexistente na grade. | `app/docs/LEIS.md:75`; `app/docs/LEIS.md:87`; `app/docs/LEIS.md:120` | **VF**. |

**Propriedades fixas exigidas em todos os casos**

| Propriedade | Asserção exigida pela regra |
|---|---|
| **Partes somam o total** | Nos dias completos destas tabelas: 6x1, 240+200=440; diurno, 300+360=660; noturno sem prorrogação, 300 diurnos+360 noturnos=660; com prorrogação, 180+480=660. N não se soma novamente a T; indenização de intervalo também não vira tempo trabalhado. (`app/docs/LEIS.md:80`; `app/docs/LEIS.md:84`; `app/docs/LEIS.md:148`; `app/docs/CORTES.md:418`) |
| **Previsto independente do realizado** | Corrigir, retratar ou remover uma batida não muda P. Só mudança pertinente da grade/cadastro/decisão do dia pode alterá-lo. (`app/docs/LEIS.md:75`; `app/docs/CORTES.md:240`; `app/docs/CORTES.md:333`) |
| **Déficit não fabrica falta** | Nos casos de saída 30 minutos antecipada, F continua0 sem decisão descontável. Antecipada respeita o teto P−T. Dia sem batidas e sem decisão permanece “em aberto”, com o número correspondente. (`app/docs/LEIS.md:87`; `app/docs/LEIS.md:111`; `app/docs/CORTES.md:179`) |
| **Abono não duplica trabalho** | Abono cobre somente o previsto que as batidas não cobriram. Nos casos integrais: T0+A=P; com dia totalmente trabalhado, não acrescentar outro P de abono. (`app/docs/CORTES.md:88`) |
| **Idempotência e determinismo** | Mesmos fatos e cadastro efetivo produzem o mesmo julgamento; repetir o lote não duplica minutos, decisões ou cobrança do mesmo fato. (`app/docs/LEIS.md:122`; `app/docs/LEIS.md:148`; `app/docs/GLOSSARIO.md:27`) |
| **Todo leitor lê o mesmo número** | Comparar os mesmos dias, competência e versão da lavratura entre espelho, app, célula e DiaPago. Chamado lê esse mesmo lastro; seu fechamento isolado não prova pagamento ou correção. (`app/docs/LEIS.md:60`; `app/docs/LEIS.md:95`; `app/docs/LEIS.md:113`; `app/docs/CORTES.md:316`) |
| **Correção conserva história** | Retratada fica fora da apuração, co

[truncado em 20000 de 21317 caracteres]

## 05/11 15:34 · sessao 5528345e03b0 · - · - · origem=Ronald (tmux)

Duas fichas sustentáveis por leitura estática; não executei testes nem consultei produção. [não verificado] [nao verificado]

1. **Justificativa aprovada depois da lavra mantém marco faltante; antes da lavra o dispensa.**

   Entrada sintética: 6x1, 07:00–15:20, pausa 13:00–14:00, domingo de folga; dia passado com `07:00 E / 13:00 S / 15:20 S`, faltando retorno 14:00. Justificativa `esqueci`, sem `saida_informada`, sem `batida_entrada` e sem `chamado_origem`; esses campos evitam plantio e fechamento de chamado como fatos adicionais. A aprovação aceita o ramo sem plantio, grava `aprovada` e apenas invalida o fechamento. `app/ponto/services/justificativa.py:137`, `app/ponto/services/justificativa.py:190`, `app/ponto/services/justificativa.py:199`.

   **Ordem A:** aprovar antes do primeiro julgamento → grade limpa o `missing`, marca `justificado`; cartório dispensa a lâmpada, resultando em zero marcos faltantes. `app/escala/utils.py:1213`, `app/escala/utils.py:1275`, `app/ponto/services/cartorio.py:763`, `app/ponto/services/cartorio.py:772`.

   **Ordem B:** julgar primeiro, aprovar depois → signal chama cartório sem `forcar`; justificativa não integra a impressão; impressão igual faz o cartório pular a célula, preservando a lâmpada antiga faltante. `app/ponto/signals.py:85`, `app/ponto/signals.py:161`, `app/ponto/services/cartorio.py:443`, `app/ponto/services/cartorio.py:1348`, `app/ponto/services/cartorio.py:1358`.

   A própria bateria mantém esse defeito como `expectedFailure`, especificamente “Justificativa aprovada não move a impressão”. `app/ponto/tests/test_d3_impressao_cobre_insumos.py:247`.

   Leitores públicos seguem a ata/status antigos: espelho carrega `grade_da_celula`; app recebe `cel.status_dia` lavrado. Não afirmar divergência numérica entre admin e app neste caso: ambos podem conservar o mesmo resultado velho; a violação comprovável é **dependência da ordem para o mesmo estado final**, contra determinismo L-099. `app/ponto/services/espelho.py:506`, `app/ponto/status_dia.py:287`, `app/docs/LEIS.md:122`.

   **Certeza:** alta para a falha do hash; a adaptação exata do fixture existente de 5x2 para 6x1 não foi executada. `app/ponto/tests/test_d3_impressao_cobre_insumos.py:57`, `app/ponto/tests/test_d3_impressao_cobre_insumos.py:247`. **Dono L-099: ESTRUTURA.** `app/docs/LEIS.md:122`.

2. **Editar template compartilhado atualiza previsão sem atualizar DiaPago.**

   Entrada sintética: 6x1 07:00–15:20, pausa 13:00–14:00; `07:00 E / 13:00 S / 14:00 E / 15:20 S`; fechamento e DiaPago inicialmente lavrados com previsto **440 min**. Editar o template para saída 16:20 dá previsto **500 min**, pelo cálculo bruto menos pausa. `app/escala/services/cadastro_tipo.py:290`.

   Ordem: lavrar → editar template → próximo julgamento do cartório → ler célula e DiaPago antes do recálculo financeiro. A edição propaga regeneração com `recalculo=False`, explicitamente, e apenas invalida fechamento. `app/escala/services/cadastro_tipo.py:307`, `app/escala/services/cadastro_tipo.py:327`, `app/escala/services/cadastro_tipo.py:348`, `app/escala/services/cadastro_tipo.py:357`.

   Regeneração escreve DNA novo; julgamento antecipado e dinheiro estão condicionados a `recalculo`. Depois de julgamento separado, ata recebe o previsto novo; DiaPago permanece com o velho porque invalidar apenas zera `FechamentoMensal.derivado_em`. `app/ponto/portas/celula.py:251`, `app/ponto/portas/celula.py:360`, `app/ponto/portas/celula.py:384`, `app/ponto/services/cartorio.py:704`, `app/ponto/services/fechamento.py:1636`.

   **Código:** célula/ata **500 previstos**, DiaPago **440 previstos** até rederivação. O escritor de DiaPago copia previsão da grade somente quando sua lavratura efetivamente ocorre. `app/ponto/services/dia_pago.py:384`, `app/ponto/services/dia_pago.py:391`, `app/ponto/services/dia_pago.py:395`. **Regra:** dinheiro e leitores devem acompanhar o cadastro de modo coerente; a implementação declara que o recálculo dessa porta continua manual. `app/docs/LEIS.md:122`, `app/escala/services/cadastro_tipo.py:339`.

   **Certeza:** alta para existência do intervalo sem atualização; execução dessa entrada sintética e ocorrência em produção não verificadas. [não verificado] **Dono L-099: ESTRUTURA.** `app/docs/LEIS.md:122`.

| Caso | Divergência | Dono | Prova principal |
|---|---|---|---|
| Justificativa antes/depois | 0 × 1 marco faltante, conforme ordem | ESTRUTURA | `app/ponto/services/cartorio.py:1358`; `app/ponto/tests/test_d3_impressao_cobre_insumos.py:247` |
| Edição de template | célula/ata 500 × DiaPago 440 minutos previstos | ESTRUTURA | `app/escala/services/cadastro_tipo.py:348`; `app/ponto/services/fechamento.py:1636` |

## 05/11 15:33 · sessao 8e0f66c1c2ec · BATERIA-QUEIXAS-ADMIN · leitura · origem=Code

Passo (1): **gabarito pela regra**, com oito casos parametrizados por escala. Cada par roda nas duas ordens; cada trio, nas seis permutações. São 36 sequências por escala, 144 no conjunto — contagem da matriz sintética, sem execução contra o sistema. `[nao verificado]`

Não criei, alterei nem apaguei arquivos. Não consultei as implementações proibidas para decidir resultados. `[nao verificado]` O método adotado é o da L-110: cadastro + batidas + regra; lacuna não recebe resposta emprestada do código. (`app/docs/LEIS.md:136`, `app/docs/CORTES.md:240`)

**Convenções para ler as tabelas.**

- A resposta é o vetor **`(P,T,F,A,N,HE)`**, em minutos: previsto, trabalhado, falta descontável, abono, trabalho noturno efetivo e horas extras. Noturno é uma parte do trabalhado; não se soma novamente à jornada. (`app/docs/LEIS.md:113`, `app/docs/CORTES.md:240`)
- **G** = folga válida; **F** = lançamento de falta integral; **A** = atestado aprovado, com efeito abonador; **V** = férias concedidas e vigentes. Nos casos 04, A é parcial, com período explícito; nos demais, cobre integralmente o plantão. São premissas sintéticas, não cadastros encontrados no banco. `[nao verificado]`
- `X⇄Y` significa duas execuções independentes: `X→Y` e `Y→X`. `Π(X,Y,Z)` significa `XYZ`, `XZY`, `YXZ`, `YZX`, `ZXY`, `ZYX`. Cada execução começa novamente no cadastro inicial. `[nao verificado]`
- **Z** = `(0,0,0,0,0,0)`: folga reconhecida pela grade, sem batidas. Ausência não cria abono, desconto ou consumo de saldo de ausência nessa folga. (`app/docs/LEIS.md:75`, `app/docs/LEIS.md:121`)
- **U(b)** = **`(lei: ?,0,0,lei: ?,0,0)`**, com previsão contratual de referência **b**. Os textos não especificam suficientemente se férias zeram o previsto apurável ou mantêm uma referência coberta, nem como essa cobertura entra no campo abono. Não significa dia sem remuneração. **lei: `[nao verificado]`**; método para conservar a lacuna: `app/docs/CORTES.md:240`.

Férias são contadas em dias corridos. Portanto, “ausência não consome saldo em folga” não autoriza retirar domingos ou descansos do período de férias. A CLT também preserva remuneração de férias e manda considerar adicionais na sua base; **N=0 de trabalho efetivo não significa adicional habitual de férias igual a zero**. ([CLT, arts. 129, 130 e 142, §§5º–6º](https://legis.senado.leg.br/norma/530547/publicacao/37353698); referência local desses dispositivos: `[nao verificado]`)

Os códigos da coluna **leitores** expandem-se assim; os rótulos são descrições do resultado, não afirmações sobre nomes de enums existentes. `[nao verificado]`

| Código | Espelho e app | Célula | DiaPago | Chamado | Regra |
|---|---|---|---|---|---|
| **S** | Folga; vetor Z; sem falta ou abono artificial | Dia sem trabalho, com a folga reconhecida | Mesmos minutos Z; isso não declara salário/DSR igual a zero | Cobrança de batidas desse dia deve calar | `app/docs/LEIS.md:95`, `app/docs/LEIS.md:113`, `app/docs/LEIS.md:121`, `app/docs/CORTES.md:416` |
| **J** | Trabalho parcial e atestado parcial, discriminando T e A | Períodos trabalhado e coberto coerentes com o vetor | Mesmo T, A, F, N e HE | Não cobrar marcos cobertos; batidas fora do período de A não viram fato dentro da ausência | `app/docs/CORTES.md:277`, `app/docs/CORTES.md:312`, `app/docs/CORTES.md:315` |
| **SV** | Minutos Z; **lei:** palavra principal “folga”/“férias” não definida | Mesmo resultado numérico; prioridade de classificação pendente | Minutos Z, sem concluir remuneração de férias zero | Sem cobrança de falta injustificada; tratamento do conflito de classificação: **lei** | `app/docs/LEIS.md:121`, `app/docs/CORTES.md:198`, `app/docs/CORTES.md:240`; prioridade: `[nao verificado]` |
| **V** | Férias justificam a ausência; mostrar os valores decididos de U(b) | F=0; representação de P/A: **lei** | Não inventar P/A nem duplicar cobertura | Não cobrar como falta injustificada; encaminhamento da colisão: **lei** | `app/docs/LEIS.md:120`, `app/docs/LEIS.md:136`; representação: `[nao verificado]` |
| **AV** | Colisão atestado/férias explicitada neste gabarito; U(b) | Precedência e representação de P/A: **lei** | Sem duplicar atestado e férias; número definitivo depende da decisão escrita | Estado/ação de resolução da colisão: **lei** | `app/docs/CORTES.md:240`, `app/docs/CORTES.md:277`; precedência: `[nao verificado]` |

**6x1.** Cadastro sintético **C6**: 44h semanais; segunda a sábado, 07:00–15:20, intervalo 11:00–12:00; domingo de descanso. Um dia de trabalho tem **440 minutos**, descontado o intervalo cadastrado. Datas, vínculo e aplicação da CCT são premissas de teste. `[nao verificado]` A retirada do intervalo da jornada vem da L-032. (`app/docs/LEIS.md:80`)

| id | cadastro | batidas — horário local | lançamentos | resposta da regra | regra citada | leitores |
|---|---|---|---|---|---|---|
| 6-01 | C6; dia originalmente trabalhável; G avulsa válida | Nenhuma | G⇄F | **Z**. F não cobra 440 min em folga | L-089 — `app/docs/LEIS.md:121` | S |
| 6-02 | C6; domingo; G de calendário do vínculo vigente | Nenhuma | G⇄A | **Z**. A não produz abono nem consome saldo de ausência | L-023/L-089 — `app/docs/LEIS.md:74`, `app/docs/LEIS.md:121`; calendário — `app/docs/CORTES.md:417` | S |
| 6-03 | C6; G avulsa válida; V abrangendo o dia | Nenhuma | G⇄V | **Z** nos minutos; **lei:** classificação principal e sua representação | L-089 — `app/docs/LEIS.md:121`; lacuna — `app/docs/CORTES.md:240` | SV |
| 6-04 | C6; A parcial 10:40–15:20 | E 07:00; S 10:40 | F⇄A | **`(440,220,0,220,0,0)`**. A cobre 10:40–11:00 + 12:00–15:20; intervalo não é abonado | L-032/L-088 — `app/docs/LEIS.md:80`, `app/docs/LEIS.md:120`; abono residual — `app/docs/CORTES.md:277`, `app/docs/CORTES.md:315` | J |
| 6-05 | C6; V válida abrangendo o dia; base 440 | Nenhuma | F⇄V | **U(440)**. Não cabe falta injustificada; **lei:** P/A sob férias | L-088/L-110 — `app/docs/LEIS.md:120`, `app/docs/LEIS.md:136` | V |
| 6-06 | C6; A integral e V concorrentes; base 440 | Nenhuma | A⇄V | **U(440)**. **lei:** qual efeito permanece e como registrar P/A | L-110 — `app/docs/LEIS.md:136`; lacuna — `app/docs/CORTES.md:240` | AV |
| 6-07 | C6; G avulsa válida; demais fatos integrais | Nenhuma | Π(G,F,A); Π(G,F,V); Π(G,A,V) | **Z** nas 18 ordens. Sem V: folga. Com V: **lei** para classificação, sem abono/desconto de ausência em folga | L-089/L-099 — `app/docs/LEIS.md:121`, `app/docs/LEIS.md:122`; lacuna — `app/docs/CORTES.md:240` | S sem V; SV com V |
| 6-08 | C6; sem G; A integral, V vigente; base 440 | Nenhuma | Π(F,A,V) | **U(440)** nas seis ordens. F não se torna injustificada por ser lançada por último; **lei:** precedência A/V | L-088/L-110 — `app/docs/LEIS.md:120`, `app/docs/LEIS.md:136` | AV |

**12x36 diurno.** Cadastro sintético **CD**: 07:00–19:00, intervalo 12:00–13:00; fase declarada com o dia-alvo trabalhável e o seguinte de descanso. Previsto do plantão: **660 minutos**. `[nao verificado]` O intervalo cadastrado sai da jornada; a fase não deve ser inventada a partir da ausência de batidas. (`app/docs/LEIS.md:73`, `app/docs/LEIS.md:80`)

| id | cadastro | batidas — horário local | lançamentos | resposta da regra | regra citada | leitores |
|---|---|---|---|---|---|---|
| D-01 | CD; plantão convertido em G avulsa válida | Nenhuma | G⇄F | **Z**. F não cobra 660 min de folga | L-089 — `app/docs/LEIS.md:121` | S |
| D-02 | CD; dia de descanso reconhecido pela fase | Nenhuma | G⇄A | **Z**. Sem abono e sem consumo de saldo de ausência | L-022/L-089 — `app/docs/LEIS.md:73`, `app/docs/LEIS.md:121` | S |
| D-03 | CD; G avulsa válida e V abrangendo o dia | Nenhuma | G⇄V | **Z** nos minutos; **lei:** classificação principal | L-089 — `app/docs/LEIS.md:121`; lacuna — `app/docs/CORTES.md:240` | SV |
| D-04 | CD; A parcial 11:00–19:00 | E 07:00; S 11:00 | F⇄A | **`(660,240,0,420,0,0)`**. A cobre 11:00–12:00 + 13:00–19:00 | L-032/L-088 — `app/docs/LEIS.md:80`, `app/docs/LEIS.md:120`; abono residual — `app/docs/CORTES.md:277`, `app/docs/CORTES.md:315` | J |
| D-05 | CD; V vigente; base 660 | Nenhuma | F⇄V | **U(660)**. F=0; **lei:** representação de P/A | L-088/L-110 — `app/docs/LEIS.md:120`, `app/docs/LEIS.md:136` | V |
| D-06 | CD; A integral e V concorrentes; base 660 | Nenhuma | A⇄V | **U(660)**. **lei:** precedência A/V, sem duplicar cobertura | L-110 — `app/docs/LEIS.md:136`; abono limitado — `app/docs/CORTES.md:277` | AV |
| D-07 | CD; G avulsa válida; demais fatos integrais | Nenhuma | Π(G,F,A); Π(G,F,V); Π(G,A,V) | **Z** nas 18 ordens; com V, classificação principal permanece **lei** | L-089/L-099 — `app/docs/LEIS.md:121`, `app/docs/LEIS.md:122`; lacuna — `app/docs/CORTES.md:240` | S sem V; SV com V |
| D-08 | CD; sem G; A integral, V vigente; base 660 | Nenhuma | Π(F,A,V) | **U(660)** nas seis ordens. Sem falta injustificada; **lei:** efeito prevalente e P/A | L-088/L-110 — `app/docs/LEIS.md:120`, `app/docs/LEIS.md:136` | AV |

**12x36 noturno.** Cadastro sintético **CN**: 19:00 de D–07:00 de D+1, intervalo 01:00–02:00; D trabalhável, D+1 de descanso; CCT Vigilantes Londrina aplicável. Previsto: **660 minutos**. `[nao verificado]` A cl. 38-d determina hora de 60 minutos e preserva o adicional de 20%; o plantão pertence a D. (`app/core/regua_cct.py:132`, `app/core/regua_cct.py:136`, `app/docs/LEIS.md:84`, `app/docs/LEIS.md:117`)

| id | cadastro | batidas — horário local | lançamentos | resposta da regra | regra citada | leitores |
|---|---|---|---|---|---|---|
| N-01 | CN; plantão D convertido em G avulsa válida | Nenhuma | G⇄F | **Z**. A madrugada não cria falta separada em D+1 | L-085/L-089 — `app/docs/LEIS.md:117`, `app/docs/LEIS.md:121` | S |
| N-02 | CN; descanso D+1; A sem alcançar outro plantão previsto | Nenhuma | G⇄A | **Z**. Nem 660 min de abono, nem saldo consumido no descanso | L-089 — `app/docs/LEIS.md:121` | S |
| N-03 | CN; G avulsa válida e V abrangendo o plantão | Nenhuma | G⇄V | **Z** nos minutos; **lei:** classificação principal e alcance documental da virada | L-085/L-089 — `app/docs/LEIS.md:117`, `app/docs/LEIS.md:121`; lacuna — `app/docs/CORTES.md:240` | SV |
| N-04 | CN; A parcial D 23:00–D+1 07:00 | E D 19:00; S D 23:00 | F⇄A | **`(660,240,0,420,60,0)`**. N=22:00–23:00, em minutos de 60s; adicional de 20% sobre essa base. A exclui 01:00–02:00 | Cl.38-d/L-036 — `app/core/regua_cct.py:132`, `app/docs/LEIS.md:84`; L-032 — `app/docs/LEIS.md:80`; parcial noturna — `app/docs/CORTES.md:330`; abono — `app/docs/CORTES.md:277` | J |
| N-05 | CN; V vigente; base 660 | Nenhuma | F⇄V | **U(660)**. N efetivo=0; F=0; **lei:** P/A de férias | L-085/L-088/L-110 — `app/docs/LEIS.md:117`, `app/docs/LEIS.md:120`, `app/docs/LEIS.md:136` | V |
| N-06 | CN; A integral cobrindo o plantão e V concorrente | Nenhuma | A⇄V | **U(660)**. **lei:** precedência A/V; não inventar minutos noturnos trabalhados | L-110 — `app/docs/LEIS.md:136`; cl.38-d — `app/core/regua_cct.py:132` | AV |
| N-07 | CN; G avulsa válida; demais fatos integrais | Nenhuma | Π(G,F,A); Π(G,F,V); Π(G,A,V) | **Z** nas 18 ordens, sem transferir cobrança para D+1; com V, classificação **lei** | L-085/L-089/L-099 — `app/docs/LEIS.md:117`, `app/docs/LEIS.md:121`, `app/docs/LEIS.md:122` | S sem V; SV com V |
| N-08 | CN; sem G; A integral, V vigente; base 660 | Nenhuma | Π(F,A,V) | **U(660)** nas seis ordens. Mesmo plantão D; **lei:** precedência e representação P/A | L-085/L-088/L-110 — `app/docs/LEIS.md:117`, `app/docs/LEIS.md:120`, `app/docs/LEIS.md:136` | AV |

No N-04, não há trabalho depois das 05:00; portanto, seu N=60 não depende de resolver prorrogação noturna. Para outro caso com trabalho pós-05:00, a regra exige conferir o regime e o texto coletivo aplicável; a cl.38-d isolada não fornece toda essa resposta. (`app/docs/LEIS.md:83`, `app/core/regua_cct.py:132`)

**Personalizado sem intervalo.** Cadastro sintético **CP**: CLT, 07:00–13:00, intervalo cadastrado zero, referência de **360 minutos**. `[nao verificado]` **lei:** esse cadastro não demonstra autorização para seis horas efetivamente corridas: trabalho superior a quatro e até seis horas exige 15 minutos de intervalo. Se houver supressão efetiva, ela precisa ser tratada separadamente, sem transformar automaticamente a indenização em HE. ([CLT, art.71, §§1º e 4º](https://legis.senado.leg.br/norma/530547/publicacao/37353698); fundamento local específico para dispensar esse intervalo: `[nao verificado]`)

| id | cadastro | batidas — horário local | lançamentos | resposta da regra | regra citada | leitores |
|---|---|---|---|---|---|---|
| P-01 | CP; dia convertido em G avulsa válida | Nenhuma | G⇄F | **Z**. F não cobra 360 min em folga | L-089 — `app/docs/LEIS.md:121` | S |
| P-02 | CP; descanso previsto no calendário vigente | Nenhuma | G⇄A | **Z**. Sem abono e sem consumo de saldo de ausência | L-089 — `app/docs/LEIS.md:121`; calendário — `app/docs/CORTES.md:417` | S |
| P-03 | CP; G avulsa válida e V abrangendo o dia | Nenhuma | G⇄V | **Z** nos minutos; **lei:** classificação principal | L-089 — `app/docs/LEIS.md:121`; lacuna — `app/docs/CORTES.md:240` | SV |
| P-04 | CP; A parcial 10:00–13:00 | E 07:00; S 10:00 | F⇄A | **`(360,180,0,180,0,0)`**. Foram trabalhadas 3h; não houve seis horas efetivas corridas | L-088 — `app/docs/LEIS.md:120`; abono residual — `app/docs/CORTES.md:277`, `app/docs/CORTES.md:315` | J |
| P-05 | CP; V vigente; base 360 | Nenhuma | F⇄V | **U(360)**. F=0; **lei:** P/A sob férias | L-088/L-110 — `app/docs/LEIS.md:120`, `app/docs/LEIS.md:136` | V |
| P-06 | CP; A integral e V concorrentes; base 360 | Nenhuma | A⇄V | **U(360)**. **lei:** precedência, sem duplicar cobertura | L-110 — `app/docs/LEIS.md:136`; abono limitado — `app/docs/CORTES.md:277` | AV |
| P-07 | CP; G avulsa válida; demais fatos integrais | Nenhuma | Π(G,F,A); Π(G,F,V); Π(G,A,V) | **Z** nas 18 ordens; com V, classificação principal **lei** | L-089/L-099 — `app/docs/LEIS.md:121`, `app/docs/LEIS.md:122`; lacuna — `app/docs/CORTES.md:240` | S sem V; SV com V |
| P-08 | CP; sem G; A integral, V vigente; base 360 | Nenhuma | Π(F,A,V) | **U(360)** nas seis ordens. **lei:** efeito prevalente e representação P/A | L-088/L-110 — `app/docs/LEIS.md:120`, `app/docs/LEIS.md:136` | AV |

**Propriedades fixas para todas as execuções**, inclusive depois de cada lançamento e de cada rejulgamento:

| Propriedade | Exigência da bateria | Regra |
|---|---|---|
| Partes somam o total | Nos casos 04: 440=220+220; 660=240+420; 360=180+180. Em Z, todas as parcelas são zero. Férias precisam de uma representação de cobertura definida antes de fechar a identidade correspondente | `app/docs/CORTES.md:240`, `app/docs/CORTES.md:277`; representação de férias: **lei**, `[nao verificado]` |
| Sem dupla cobertura | Abono cobre somente previsto não coberto pelas batidas. Não pagar o mesmo minuto novamente como trabalho e abono | `app/docs/CORTES.md:277` |
| Noturno não aumenta artificialmente T | No N-04, os 60 min noturnos estão dentro dos 240 trabalhados; o adicional é de 20%, com hora de 60 min | `app/docs/LEIS.md:84`, `app/core/regua_cct.py:132`, `app/docs/CORTES.md:240` |
| Idempotência | Julgar duas vezes produz o mesmo resultado; repetir o mesmo fato não duplica minutos, saldo ou pergunta | `app/docs/CORTES.md:333`, `app/docs/GLOSSARIO.md:27` |
| Previsto independente do realizado | Apagar ou acrescentar batidas não redefine a previsão. Mudança de previsto precisa decorrer do cadastro/fato reconhecido pela grade | `app/docs/LEIS.md:75`, `app/docs/CORTES.md:240` |
| Mesmo número em todos os leitores | Espelho, app, célula, DiaPago e chamado compartilham resultado e cobertura; leitor não refaz a conta | `app/docs/LEIS.md:60`, `app/docs/LEIS.md:95`, `app/docs/LEIS.md:113` |
| Furo não é falta | Antes de decisão válida, batida ausente é pergunta em aberto; não vira desconto automaticamente | `app/docs/LEIS.md:87`, `app/docs/LEIS.md:120`, `app/docs/CORTES.md:179` |
| Folga não vira ausência com efeito | Atestado/falta em folga não criam minutos nem consomem saldo de ausência | `app/docs/LEIS.md:121` |
| Virada preservada | Plantão noturno, cobertura e seus minutos pertencem ao dia de início; não duplicar em D+1 | `app/docs/LEIS.md:117`, `app/docs/CORTES.md:330` |
| Totais conferíveis | Minutos inteiros na fonte; topo e rodapé iguais à soma das linhas, sem ajuste compensatório | `app/docs/CORTES.md:210` |
| Ordem não substitui regra | Nos resultados decididos, todas as permutações do mesmo conjunto de fatos devem chegar ao mesmo resultado. Nas lacunas, não adotar “último lançamento vence” como gabarito | `app/docs/LEIS.md:122`, `app/docs/LEIS.md:136`, `app/docs/CORTES.md:240` |

Permanecem **três decisões de lei** para completar o gabarito numérico: precedência atestado/férias, classificação principal folga/férias e representação de férias em previsto/abono/DiaPago. Também falta fundamento para considerar lícito o cadastro de seis horas efetivamente corridas sem intervalo. Essas lacunas estão marcadas; não foram preenchidas por comportamento da implementação. **lei: `[nao verificado]`** (`app/docs/CORTES.md:240`)

## 05/11 15:33 · sessao d721c186275b · - · - · origem=Ronald (tmux)

Três contraexemplos sintéticos sustentados pelo código atual; ocorrência em produção [não verificado].

1. **12x36 diurno: pausa gozada vira trabalho.** Cadastro 07:00–19:00, pausa 12:00–13:00; lançar `09:30 E → 12:00 S → 13:00 E → 15:00 S`. O caso existente garante um período fechado com essa geometria; a saída foi escolhida fora do envelope da pausa. (`app/ponto/tests/test_o280_bateria_da_lei.py:1735`, `:1752`, `:1786`.)

   O motor calcula envelope de 330 minutos e só desconta a pausa se `duracao > 360`: resultado **330 trabalhados, pausa descontada 0**. (`app/ponto/motor_calculo_v2.py:2761`, `:2767`, `:2774`, `:2785`.) O juiz do realizado desconta a pausa batida e retorna **270**; a regra do Art.71 §2, transcrita na bateria, também pede **270**. (`app/ponto/turnos.py:754`, `:765`, `:769`; `app/ponto/tests/test_o280_bateria_da_lei.py:1741`.) A versão oráculo do DiaPago propaga 330, porque só quebra o envelope se o motor informou desconto. (`app/ponto/calculador/alimentacao.py:124`, `:129`, `:137`.) **Certeza estática alta; dono ESTRUTURA/L-099**, pois o mesmo cadastro e batidas recebem duas derivações. (`app/docs/LEIS.md:122`.)

2. **12x36 noturno em empresa CCT: switch desligado aplica restrição reservada ao CLT declarado.** Cadastro: empresa `regime_trabalhista='cct'`, nenhuma aplicação vigente; ciclo 12x36, 19:00–07:00, pausa 01:00–02:00. Lançar cronologicamente as quatro batidas exatas. A regra interna preserva prorrogação quando falta CLT declarado: `_regua_legal` começa com o padrão `True` e só altera para CLT explicitamente declarado. (`app/core/regua_cct.py:185`, `:210`, `:211`; regra `app/docs/LEIS.md:83`.)

   Com `regua_cct_ativa=0`, a fábrica não injeta a prorrogação para empresa CCT; o motor decide `False` apenas pelo ciclo. (`app/core/regua_cct.py:449`, `:453`, `:457`; `app/ponto/motor_calculo_v2.py:758`; `app/core/regua_cct.py:182`.) Resultado calculável: **360 minutos noturnos de relógio = 411,43 legais**, contra **480 = 548,57 legais** pela régua preservada. As contas usam janela 22–05, extensão até 07 quando ligada, desconto da pausa e fator 60/52,5. (`app/ponto/motor_calculo_v2.py:99`, `:110`, `:983`, `:985`, `:986`, `:992`.) Com switch ligado e a mesma lacuna de aplicação, `regua_para` retorna a régua preservada, mudando o resultado. (`app/core/regua_cct.py:366`, `:371`, `:458`, `:466`.) **Certeza estática alta; leitores entre si [não verificado]; conflito código versus L-035 demonstrável; dono ESTRUTURA/L-099.** (`app/docs/LEIS.md:83`, `:122`.)

3. **Mudança de vigência CCT dentro do mesmo vínculo recebe régua do primeiro dia da fatia.** Entrada concreta: competência 21/09–20/10; único vínculo cobrindo todo o período; células atribuídas ao mesmo vínculo; empresa CCT; switch ligado; uma aplicação empresarial ativa cujo sindicato tem vigência 01/10–31/12 e `prorrogacao_noturna_pos5h=False`. Em plantão previsto de **02/10**, lançar `19:00 E → 03/10 01:00 S → 02:00 E → 07:00 S`. Aplicação empresarial e vigência são decididas pelo cadastro mencionado. (`app/core/regua_cct.py:253`, `:257`, `:261`, `:267`.)

   A apuração cria a fatia pela vigência do vínculo e monta um motor para todos os dias; a consulta CCT recebe **21/09**, não 02/10. (`app/ponto/services/fechamento.py:204`, `:236`, `:270`, `:277`, `:727`.) O espelho repete a consulta pelo início da fatia. (`app/ponto/services/espelho.py:1111`.) Em 21/09 a CCT é descartada por vigência; a lacuna preserva prorrogação `True`. (`app/core/regua_cct.py:261`, `:371`, `:210`.) Portanto o plantão de 02/10 recebe **480 noturnos de relógio / 548,57 legais**; perguntar a própria autoridade pelo dia 02/10 encontra a CCT e fornece `False`, correspondente a **360 / 411,43**. (`app/core/regua_cct.py:297`, `:374`, `:466`; `app/ponto/motor_calculo_v2.py:983`.) **Certeza estática alta sobre a seleção temporal; execução integrada [não verificado]; dono ESTRUTURA/L-099.** (`app/docs/LEIS.md:122`.)

A faixa **4–6 horas sem pausa** também paga **0** em vez do piso **15 minutos**, mas a cópia declara expressamente a pendência L-031 e a compensação indevida entre portão e `default=60`; apresentar como dívida conhecida com decisão de cadastro pendente, não como correção já autorizada. (`app/escala/models.py:17`, `:19`; `app/ponto/motor_calculo_v2.py:3232`; `app/ponto/calculador/regras.py:764`; `app/ponto/calculador/periodo.py:316`, `:320`, `:326`; `app/docs/LEIS.md:79`.)

## 05/11 15:33 · sessao d403ea06b76b · - · - · origem=Ronald (tmux)

Contra-exemplo confirmado por leitura estática: **falta aprovada → atestado aprovado ou férias** mantém desconto de falta e acrescenta abono no mesmo dia; a aprovação altera apenas a ausência alvo, sem rejeitar a falta anterior (`app/ponto/services/ausencia.py:881`, `app/ponto/services/ausencia.py:895`, `app/ponto/services/ausencia.py:930`).

Entrada concreta comum: dia **04/11/2026**, célula `trabalha=True`, geradora correta, DNA com marcos abaixo; batidas `[]`; primeiro falta aprovada com `minutos=0`, depois atestado aprovado com documento, ou férias cobrindo o dia. A falta deriva os minutos previstos; atestado/férias podem atravessar falta porque ambos abonam (`app/ponto/models.py:875`, `app/ponto/catalogo/ausencias.py:95`, `app/ponto/catalogo/ausencias.py:101`, `app/ponto/precedencia.py:127`).

| Cadastro concreto | Motor selecionado | P | Resultado estático após F→A/V |
|---|---|---:|---|
| 6x1, base comercial, 08–17, pausa12–13, domingo folga, sábado08–12 | MotorComercial | 480min | abono480min **e** falta8h, TXT1dia |
| 12x36 diurno, base12x36, 07–19, pausa12–13, âncora de plantão04/11 | Motor12x36ComEscala | 660min | abono660min **e** falta11h, TXT1dia |
| 12x36 noturno, base12x36, 19–07, pausa00–01, âncora04/11 | Motor12x36ComEscala | 660min | abono660min **e** falta11h, TXT1dia |
| Personalizado, basecomercial, JSON `{"2":{"hi":"08:00","hf":"10:00"}}`, sem intervalo | MotorComercial | 120min | abono120min **e** falta2h, TXT1dia |

Seleção de motor: `app/ponto/motor_calculo_v2.py:3572`, `app/ponto/motor_calculo_v2.py:3633`, `app/ponto/motor_calculo_v2.py:3668`. Personalizado responde pela chave weekday do JSON: `app/escala/models.py:862`. Marcos: `app/escala/models.py:337`; célula soberana: `app/escala/models.py:1094`, `app/escala/models.py:1124`. Previsto líquido: `app/escala/utils.py:2290`, `app/escala/utils.py:2293`, `app/escala/utils.py:2326`.

Caminho do abono: grade soma segmentos E→S, grava P e depois aplica ausência mais recente, preservando P; sem batidas, abono=P (`app/escala/utils.py:1656`, `app/escala/utils.py:1828`, `app/escala/utils.py:1869`, `app/ponto/turnos.py:2757`, `app/ponto/turnos.py:2769`, `app/escala/utils.py:1241`, `app/ponto/services/dia_pago.py:385`, `app/ponto/services/dia_pago.py:394`).

Caminho do desconto: busca todas as faltas aprovadas sem eliminar a suprimida; fechamento e DiaPago conservam P/60; TXT conta um dia (`app/folha/export.py:76`, `app/folha/export.py:129`, `app/ponto/services/fechamento.py:865`, `app/ponto/services/fechamento.py:987`, `app/ponto/services/dia_pago.py:272`, `app/folha/export.py:170`, `app/folha/export.py:207`, `app/folha/export.py:721`).

Regra: falta custa o previsto do dia **sem justificativa**; atestado abona; férias não podem virar8792 (`app/docs/LEIS.md:120`, `app/ponto/catalogo/ausencias.py:95`, `app/folha/export.py:202`). Esperado nos quatro casos: **abono=P e falta0h, TXT0dias de falta**; dono **L-099/ESTRUTURA**, pois a contradição decorre do mesmo cadastro, não de cadastro ausente (`app/docs/LEIS.md:113`, `app/ponto/services/fechamento.py:1010`).

Não confirmei divergência numérica **app versus admin**: API chama a mesma fonte e usa os totais do espelho (`app/api/views.py:1358`, `app/api/views.py:1369`, `app/api/views.py:1431`). O demonstrado é contradição **grade/abono versus falta lavrada/TXT**; execução e incidência em produção **[nao verificado]**.

**Descartar meu candidato anterior “range falta abrangendo plantão e folga natural” pela porta normal**: o veto percorre todos os dias e recusa falta em dia sem jornada (`app/ponto/precedencia.py:501`, `app/ponto/precedencia.py:508`, `app/ponto/precedencia.py:538`).

## 05/11 15:33 · sessao b4a525895ad8 · - · - · origem=Ronald (tmux)

Dois contra-exemplos demonstráveis por leitura estática; ocorrência em produção e execução nesta cópia: [nao verificado].

**CE — parcial descontável vira um dia inteiro no TXT.**

Entrada construída: 14/10/2026, escala personalizada sem intervalo, 08:00–12:00; batidas 08:00 E / 11:30 S; `TipoAusencia.codigo=saida_antecipada`, `efeito=desconta`, `parcial=True`, `fim=dia_unico`, ativo; lançamento aprovado de 30 minutos, depois das batidas. O cadastro aceita essas combinações: `app/ponto/views_tipos_ausencia.py:64`, `:69`, `:76`, `:78`, `:80`; grava efetivamente em `app/ponto/services/ausencia.py:1289`. A porta admite minutos quando o tipo é parcial e preserva os 30 no registro: `app/ponto/services/ausencia.py:193`, `:206`, `:341`. O corte expresso manda saída antecipada descontar **em minutos**, com palavra “desconta Xh”: `app/docs/CORTES.md:202`.

Cadeia de dinheiro:

- `descontam_vigentes()` inclui qualquer tipo com efeito DESCONTA: `app/ponto/catalogo/ausencias.py:331`.
- `ausencias_na_janela` seleciona o registro aprovado: `app/folha/export.py:76`.
- `_rateio_por_dia` produz `{14/10:30}`: `app/folha/export.py:103`, `:110`; `minutos_de_falta_por_dia` usa esse mapa: `:129`.
- Fechamento calcula `horas_falta=0,5`: `app/ponto/services/fechamento.py:864`, `:866`, `:987`; DiaPago grava `horas_falta=0,5`: `app/ponto/services/dia_pago.py:272`.
- O leitor de dias do TXT reaproveita os tipos DESCONTA, mas expande a data sem olhar minutos: `app/folha/export.py:149`, `:168`.
- O escritor de 8792 usa **`len(datas)`**: `app/folha/export.py:204`, `:207`. Assim retorna quantidade **1 dia**, com registro `11` para 14/10: `:208`. Esse escritor é efetivamente chamado na montagem do TXT: `:721`–`:725`.

Código versus regra: **DiaPago 0,5 h / TXT 8792 1 dia**, para falta parcial de 30 minutos; regra cadastrada e corte pedem 30 minutos (`app/docs/CORTES.md:202`). Não converter 1 dia em quantidade monetária: valor pago pelo Domínio [nao verificado].

A palavra do admin/app usa os minutos e dirá “Saida antecipada (desconta 30min)”: `app/ponto/services/dia_decidido.py:219`, `:148`; composição comum `:428`; admin aplica em `app/ponto/services/espelho.py:340`; app copia em `app/api/views.py:1395`, `:1609`. Portanto a divergência é **palavra/admin/app e DiaPago contra quantidade do 8792**, não admin contra app.

Há também caminho de 8069 para a saída antecipada apurada das batidas: `app/folha/export.py:438`–`:441`, `app/ponto/services/fechamento.py:986`. Uma dupla emissão de 8069+8792 é consequência possível se a apuração produzir saída antecipada e o colaborador for elegível; emissão real [nao verificado].

**Elegibilidade deve ficar explícita:** `montar_linhas` só emite para `classificar_export` com `status=entra` e fechamento aberto/aprovado: `app/folha/export.py:668`, `:697`, `:707`. O classificador exige código Domínio e exclui rescisão/férias com batida: `:840`, `:846`, `:850`; a retenção consulta o veredito da célula: `:1215`–`:1236`. Assim o número errado **na função emissora** é certo; não afirmar que atravessou a retenção em produção.

Certeza: alta, estática, condicionada ao cadastro explícito; dono **ESTRUTURA**, pois dois leitores divergem com o mesmo cadastro (`app/docs/LEIS.md:122`).

---

**CE — falta inteira ainda não decidida aparece “Faltas 1” e “Em aberto 1” no mesmo PDF.**

Entrada construída: 14/10/2026, personalizado 08:00–12:00 sem intervalo, vínculo/célula de trabalho válidos, sem batidas e sem ausência/justificativa; pedir PDF do período livre **14/10–14/10**, em 05/11/2026. Falta decidida é zero pela L-039: `app/docs/LEIS.md:87`; furo permanece pergunta pela L-088: `:120`.

Cadeia:

- O PDF alimenta os dias previstos e justificadas no motor: `app/relatorios/pdf_espelho.py:372`, `:394`.
- Motor considera o dia previsto sem trabalho/justificativa, grava `dias_falta=1`, `horas_falta=4`: `app/ponto/motor_calculo_v2.py:2595`–`:2619`.
- O PDF copia esses campos para o resumo: `app/relatorios/pdf_espelho.py:461`, `:471`.
- `folha_manda` sobrescreve os totais apenas no ramo de período correspondente a competência: `app/relatorios/cartao_pela_celula.py:321`–`:346`. Período livre passa direto.
- A mesma função calcula corretamente decididas vazias e aberto um: `app/relatorios/cartao_pela_celula.py:373`, `:418`–`:421`; derivador subtrai decididas de furo: `app/ponto/services/dia_decidido.py:315`–`:320`.
- PDF chama essa função: `app/relatorios/pdf_espelho.py:628`, reaplica a palavra “Em aberto” em `:638`, mas conserva `dias_falta=1`.
- Renderização imprime **“Faltas 1 dia(s)”** em `app/relatorios/pdf_espelho.py:787` e **“Em aberto (a decidir) 1 dia(s)”** em `:791`.

Resposta da regra: **0 faltas decididas, 1 dia em aberto** (`app/docs/LEIS.md:87`). Resposta do PDF: **1 falta e 1 em aberto**, referentes ao mesmo único dia (`app/relatorios/pdf_espelho.py:787`, `:791`). Não depende de ausência de fechamento porque o período livre nem entra no ramo que os consulta (`app/relatorios/cartao_pela_celula.py:321`).

O app copia a palavra do espelho em `app/api/views.py:1395`–`:1403` e para dia sem batidas expõe `falta` pelo D4 em `:1621`; sem decisão, o D4 responde `sem_certidao`, não `falta`: `app/ponto/status_dia.py:314`–`:316`; adaptador entrega `neutro`: `:84`. O PDF, portanto, conserva uma contagem de falta que o status do app não afirma.

Certeza: alta, estática; dono **ESTRUTURA** pela L-099 (`app/docs/LEIS.md:122`). A própria dívida documental coincide com o código: `app/docs/LEIS.md:163`.

**Suspeita não promovida a achado principal:** crédito de atestado parcial só abate atraso (`app/ponto/services/credito_parcial.py:47`, `app/ponto/services/fechamento.py:985`), enquanto saída antecipada segue integral (`:986`; DiaPago `app/ponto/services/dia_pago.py:185`). Existe teste preservando desconto em parcial (`app/ponto/tests/test_abono_dia_inteiro_zera_pontualidade.py:98`), e o aval ali transcrito restringe a cura a dia inteiro (`:4`). Pode ser contra-exemplo à regra trabalhista, mas norma externa que determine o resultado preciso não foi verificada; tratar como [nao verificado], evitando afirmar que é regressão já corrigida.

## 05/11 15:33 · sessao 792007f13eef · - · - · origem=Ronald (tmux)

Matriz das portas de criação, com `X=falta`, `A=atestado`, `V=férias`, catálogo canônico (`X=DESCONTA`, `A/V=ABONA`): `app/ponto/catalogo/ausencias.py:88`, `app/ponto/catalogo/ausencias.py:95`, `app/ponto/catalogo/ausencias.py:101`. O cadastro vivo de produção é [nao verificado].

| Ordem tentada, continuando depois de cada veto | Aceitos | Vetados |
|---|---|---|
| XA | X,A | — |
| AX | A | X |
| XV | X,V | — |
| VX | V | X |
| AV | A | V |
| VA | V | A |
| XAV | X,A | V |
| XVA | X,V | A |
| AXV | A | X,V |
| AVX | A | V,X |
| VXA | V | X,A |
| VAX | V | A,X |

A matriz deriva da exceção `abona_sobre_falta`, que libera **todo tipo ABONA sobre falta**, inclusive férias, e do veto por linha contra qualquer outra ausência viva: `app/ponto/precedencia.py:127`, `app/ponto/precedencia.py:145`, `app/ponto/precedencia.py:461`. A criação usa esse veto; aprovação só altera o estado da própria ausência, sem rejeitar a falta anterior: `app/ponto/services/ausencia.py:377`, `app/ponto/services/ausencia.py:881`, `app/ponto/services/ausencia.py:801`.

Contra-exemplos prioritários, todos **ESTRUTURA / L-099**, por divergência com o mesmo cadastro: `app/docs/LEIS.md:122`. Certeza alta por inspeção estática; execução e ocorrência em produção [nao verificado].

1. **X→A aprovado: atestado abona visualmente, mas falta continua no DiaPago/TXT.** Entrada construída: CLT, 6x1, terça-feira 06/10/2026, jornada 08:00–12:00/13:00–17:00, célula `trabalha=True`, batidas `[]`, falta aprovada com 480 minutos; depois atestado do mesmo dia, inteiro (`minutos=0`), com documento e aprovado. Os campos são entrada do cenário, não dado de produção [nao verificado]. A porta admite a justificativa sobre falta: `app/ponto/precedencia.py:127`, `app/ponto/services/ausencia.py:315`. A cobertura escolhe A pelo maior `(data,pk)`: `app/ponto/turnos.py:2552`, `app/ponto/turnos.py:2753`. A grade fica `tipo_dia='ausencia', ausencia_tipo='atestado'`: `app/escala/utils.py:1209`, `app/escala/utils.py:1231`. A palavra da tela usa essa cobertura e mostra atestado abonado: `app/ponto/services/dia_decidido.py:363`, `app/ponto/services/dia_decidido.py:428`. Entretanto o extrator do desconto pega **todas** as faltas aprovadas sem consultar o vencedor: `app/folha/export.py:76`, `app/folha/export.py:127`. Isso chega a `FechamentoMensal.horas_falta=8` e `DiaPago.horas_falta=8`: `app/ponto/services/fechamento.py:864`, `app/ponto/services/fechamento.py:987`, `app/ponto/services/fechamento.py:1184`, `app/ponto/services/dia_pago.py:272`. Havendo elegibilidade e rubrica configurada, TXT inclui 06/10 na falta: `app/folha/export.py:721`. Regra esperada: a justificativa aprovada vence a falta; o próprio contrato promete isso: `app/ponto/precedencia.py:119`. **Não afirmar 8932 simultânea no TXT**: `datas_abonadas` existe em `app/folha/export.py:190`, mas seu chamador produtivo atual é [nao verificado].

2. **X→V: férias cobrem o dia, TXT ainda desconta.** Mesma entrada anterior, substituindo A por férias aprovadas materializadas pelo agendamento; entrada construída [nao verificado]. Férias atravessam falta porque ABONA: `app/ponto/catalogo/ausencias.py:101`, `app/ponto/precedencia.py:127`. O agendamento consulta o veto e materializa férias: `app/ferias/models.py:428`, `app/ferias/models.py:434`, `app/ponto/services/ausencia.py:1186`. Férias vencem os fatos pela faixa e a grade pela cobertura: `app/ponto/precedencia.py:46`, `app/ponto/precedencia.py:396`, `app/ponto/turnos.py:2753`. A falta permanece aprovada porque a nova criação não muda as outras linhas: `app/ponto/services/ausencia.py:341`, `app/ponto/services/ausencia.py:385`. O caminho de desconto é o mesmo do item 1: `app/folha/export.py:76`, `app/ponto/services/fechamento.py:864`, `app/ponto/services/dia_pago.py:272`, `app/folha/export.py:721`. O próprio export declara que seu papel é impedir falta 8792 no dia de férias: `app/folha/export.py:196`. Inversão V→X veta X: `app/ponto/precedencia.py:145`.

3. **X→A: direito de férias conta a falta já justificada.** Entrada construída: seis faltas singelas aprovadas em dias previstos dentro do período aquisitivo; depois atestado inteiro aprovado sobre uma delas, sem rejeitar X [nao verificado]. A cobertura passa a A: `app/ponto/turnos.py:2552`. `PeriodoAquisitivo.faltas_periodo` continua retornando **6**, porque conta linhas de falta aprovadas e ignora a cobertura: `app/ferias/models.py:140`. `aplicar_art130` usa esse número e determina **24 dias**, enquanto cinco faltas remanescentes corresponderiam a **30 dias** na tabela local: `app/ferias/services.py:243`, `app/ferias/models.py:52`. O código que permite X→A e promete a vitória da justificativa é `app/ponto/precedencia.py:119`. O impacto real em saldo é [nao verificado].

4. **X de range anterior→A em dia posterior: ata de precedência diverge deterministicamente da grade.** Entrada construída: X aprovada cobrindo 05–06/10/2026; A aprovada só em 06/10, sem batidas nesse dia [nao verificado]. A porta admite A sobre X: `app/ponto/precedencia.py:127`. `fatos_do_periodo` ordena ausências pela data crescente e põe X e A na mesma faixa; o sort final só considera a faixa: `app/ponto/precedencia.py:273`, `app/ponto/precedencia.py:379`, `app/ponto/precedencia.py:396`, `app/ponto/precedencia.py:77`. Portanto em 06/10 a precedência preserva X, enquanto a cobertura escolhe A pela data mais recente: `app/ponto/turnos.py:2753`. O cartório grava esse `tipo_do_dia='falta'` em `ata['precedencia']`, mas a grade fica A: `app/ponto/services/cartorio.py:1947`, `app/escala/utils.py:1209`. Para **mesma data inicial**, a consulta ordena somente por `data`, sem desempate por `pk`; a ordem retornada pelo banco e o vencedor da precedência são [nao verificado].

## 05/11 15:32 · sessao 14e497c6e2c5 · - · - · origem=Ronald (tmux)

Os valores propostos são coerentes, com estas condições:

- **6h contínuas:** `P360/T360/HE0/I15`. Declare trabalho contínuo comprovado, regime CLT e `intervalo_indenizavel=True`; cadastro sem pausa não autoriza dedução fictícia de 15 minutos. `app/docs/LEIS.md:79`, `app/docs/LEIS.md:80`; CLT art.71 §§1º/4º [nao verificado em arquivo local].
- **Falta integral decidida:** `F360/I0`; **atestado integral aprovado:** `A360/I0`. Furo anterior à decisão continua “em aberto”. `app/docs/LEIS.md:87`, `app/docs/LEIS.md:120`.
- **Parcial paga 08–10, trabalho 10–14:** `T240/A120/I0`, considerando exatamente quatro horas efetivas e nenhuma sobreposição paga. `app/docs/CORTES.md:277`, `app/docs/CORTES.md:315`; CLT art.71 §1º [nao verificado em arquivo local].
- **08–14:30 contínuo comprovado:** antes da autorização, `T computável360/HE0/HP30/I60`; depois, `T390/HE30/HP0/I60`. A obrigação de uma hora de intervalo decorre do trabalho contínuo superior a seis horas, não do clique que autoriza a ponta. `app/docs/LEIS.md:115`; CLT art.71 caput/§4º [nao verificado em arquivo local].
- **Noturno 19–07, pausa 00–01:** `T660/N360` sem prorrogação; `T660/N480` com prorrogação expressamente cadastrada até 07h. Com pausa validada **05–06** e sem prorrogação: `T660/N420`. Hora de 60 minutos; adicional de 20%. `app/docs/LEIS.md:80`, `app/docs/LEIS.md:83`, `app/docs/LEIS.md:84`, `app/docs/CORTES.md:418`.

**lei:** a cl.38-d, isoladamente, não determina prorrogação após 05h; explicite esse cadastro em cada variante. Uma resposta apenas informando a saída também não comprova ausência de pausa: continuidade deve constar como entrada hipotética do caso. `app/docs/LEIS.md:83`; `app/docs/CORTES.md:273`.

## 05/11 15:32 · sessao 5f08f305e748 · - · - · origem=Ronald (tmux)

Contra-exemplo estático confirmado: **falta aprovada → folga de calendário** deixa o mesmo dia como folga na grade e falta no extrator do TXT. A ordem inversa recusa a falta. Ocorrência em produção e execução destes cenários: [nao verificado].

Entrada construída: colaborador ativo, vínculo vigente, empresa CLT, competência aberta; 6x1 08:00–12:00/13:00–17:00, quarta-feira 04/11/2026 inicialmente prevista, batidas `[]`, falta aprovada de 480 minutos. Depois aplicar calendário que declare 04/11 folga. A aplicação real troca o calendário e materializa a foto; o escritor da foto não consulta `veto_de_lancamento`. (`app/escala/views.py:409`, `app/escala/views.py:411`, `app/escala/folgas.py:150`, `app/escala/folgas.py:173`.)

Caminho e respostas:

- Foto ⇒ folga: `eh_dia_trabalho_calculado` responde `False` para data fotografada; a foto provoca regeneração. (`app/escala/models.py:1311`, `app/escala/models.py:1317`, `app/escala/folgas.py:192`.)
- Grade ⇒ folga: `_marcar_ausencias` explicitamente impede falta de substituir folga. O teste existente exige esse resultado. (`app/escala/utils.py:1231`, `app/escala/tests/test_grade_segue_a_precedencia.py:48`.)
- Extrator TXT ⇒ falta de **1 dia**, mantendo **480 minutos**: consulta faltas aprovadas sem filtrar jornada ou selecionar o fato vencedor; expande D e soma seus minutos. (`app/folha/export.py:76`, `app/folha/export.py:129`, `app/folha/export.py:149`, `app/folha/export.py:158`.)
- Regra ⇒ **zero desconto em folga**, pois ausência que abona/desconta só alcança dia previsto. (`app/docs/LEIS.md:121`.)
- Ordem inversa calendário→falta ⇒ recusa por folga existente; falta→folga **manual** também recusa. (`app/ponto/precedencia.py:514`, `app/ponto/precedencia.py:538`, `app/escala/folgas.py:258`, `app/ponto/precedencia.py:461`.)

**Não afirmar `DiaPago.horas_falta=0`:** o extrator de minutos mantém 480; o valor final do DiaPago exige rastrear a ligação completa. [nao verificado]

Extensão numérica ao saldo: período aquisitivo com outras cinco faltas válidas e a falta de D acima. O contador conta **6 registros**, incluindo a falta agora em folga; `aplicar_art130` atribui **24 dias**, enquanto a exclusão da falta em folga deixaria cinco faltas e **30 dias**. (`app/ferias/models.py:144`, `app/ferias/services.py:246`, `app/ferias/models.py:54`, `app/docs/LEIS.md:121`.)

A mesma construção alcança 12x36 diurno/noturno com D inicialmente plantão e falta de 720 minutos, ou personalizado sem intervalo 08:00–16:00 com 480 minutos: o escritor de foto e os extratores não restringem o ciclo. Efeitos adicionais da foto na fase do 12x36 devem ser explicitados, pois a foto vence a âncora. (`app/escala/folgas.py:173`, `app/folha/export.py:76`, `app/escala/models.py:1289`, `app/escala/models.py:1296`.)

Matriz relevante da folga manual — assimetrias abaixo são **intencionais na implementação**, e não defeitos por si:

| Ordem | Resultado da porta | Referência |
|---|---|---|
| Folga→falta | Recusa | `app/ponto/precedencia.py:514` |
| Falta→folga | Recusa | `app/ponto/precedencia.py:461` |
| Folga→atestado | Recusa | `app/ponto/precedencia.py:514` |
| Atestado→folga | Recusa | `app/ponto/precedencia.py:461` |
| Folga→férias | Permite; férias suprimem folga | `app/ponto/precedencia.py:496` |
| Férias→folga | Recusa | `app/ponto/precedencia.py:461` |

A exceção para férias atravessarem folga já lançada está documentada como decisão específica; não exigir cancelamentos individuais. (`app/ponto/precedencia.py:470`, `app/ponto/precedencia.py:485`, `app/ponto/precedencia.py:492`.)

Outro achado, de classificação: **atestado aprovado em folga automática** transforma a grade em ausência e limpa células/orfas; o teste atual exige isso. A própria tabela de leis reconhece o conflito com L-089. Valores monetários divergentes decorrentes deste caso: [nao verificado]. (`app/escala/utils.py:1231`, `app/escala/utils.py:1241`, `app/escala/utils.py:1272`, `app/escala/tests/test_grade_segue_a_precedencia.py:43`, `app/docs/LEIS.md:173`.)

| CE | Código × regra | Leitores/efeitos divergentes | Certeza | Dono |
|---|---|---|---|---|
| Falta→folga calendário | Extrator mantém 480/720 min e 1 dia; regra exige zero | Grade folga × extrator TXT falta | Alta estática; execução [nao verificado] | ESTRUTURA, L-099 (`app/docs/LEIS.md:122`) |
| Mesmo caso + cinco faltas válidas | Direito férias 24 × 30 dias | Grade folga × contador do período | Alta estática; execução [nao verificado] | ESTRUTURA, L-099 (`app/docs/LEIS.md:122`) |
| Atestado sobre folga automática | Grade ausência × L-089 sem efeito em folga | Classificação; dinheiro [nao verificado] | Alta estática | ESTRUTURA, L-099 (`app/docs/LEIS.md:122`) |

## 05/11 15:32 · sessao 5007f5a878cd · - · - · origem=Ronald (tmux)

Achados por leitura estática; ocorrência em produção e execução integrada destes cenários: [nao verificado]. Nenhum arquivo foi criado, editado ou apagado.

1. Personalizado sem intervalo: falta parcial de três horas desaparece.

Entrada construída: segunda-feira passada, não feriado; empresa `regime_trabalhista='clt'`; vínculo vigente; `tipo_ciclo='personalizado'`, `tipo_base='comercial'`, `hora_inicio=08:00`, `hora_fim=12:00`, `horarios_por_dia={'0': {'hi':'08:00','hf':'12:00'}}`, ambos os campos de intervalo `None`; célula `trabalha=True`, DNA com esses marcos. Primeiro cadastro/célula, depois batidas `11:00 E`, `12:00 S`; nenhuma ausência. O personalizado reconhece os dias pelas chaves do mapa e os horários pelo override: `app/escala/models.py:862`, `app/escala/models.py:326`.

Caminho:

- Fábrica escolhe `MotorComercial`: `app/ponto/motor_calculo_v2.py:3572`, `app/ponto/motor_calculo_v2.py:3668`.
- Comercial chama a pontualidade: `app/ponto/motor_calculo_v2.py:3293`.
- Marcos vêm do vínculo/célula ou template: `app/ponto/motor_calculo_v2.py:2048`.
- `diff=180`; o limiar é `>=180`: `app/escala/regua_defesa.py:381`, `app/escala/regua_defesa.py:498`.
- Sem marcos de intervalo, retira `prevista_entrada`, independentemente de a saída ser exata: `app/ponto/motor_calculo_v2.py:2122`, `app/ponto/motor_calculo_v2.py:2142`.
- Sem previsão da entrada, atraso permanece zero; saída exata também dá zero: `app/ponto/motor_calculo_v2.py:2217`, `app/ponto/motor_calculo_v2.py:1970`, `app/ponto/motor_calculo_v2.py:2006`.

Resposta: **60 minutos trabalhados; atraso 0**, com alerta `HORARIO_DESLOCADO`. Pela L-084 escrita, uma ponta distante segue descontando; pela L-093, o teto é `240−60=180`: **180 minutos de atraso**. Referências: `app/docs/LEIS.md:116`, `app/docs/LEIS.md:111`.

Certeza alta para a resposta do caminho. Ressalva indispensável: a exceção para dia sem intervalo está expressamente descrita como recorte aprovado nos comentários, portanto há **colisão entre a regra absoluta de LEIS.md e o recorte implementado**, e não desconhecimento dessa condição: `app/ponto/motor_calculo_v2.py:2123`, `app/ponto/calculador/regras.py:197`. Os dois calculadores repetem o mesmo resultado; divergência concreta entre leitores deste exemplo: [nao verificado]. Dono L-099: **ESTRUTURA**, porque o mesmo cadastro íntegro contém o marco de entrada e a única ponta distante é apagada por regra do programa; separação de donos em `app/docs/LEIS.md:122`.

2. Atestado de oito horas no 12x36 é promovido a dia inteiro; declarar o período é recusado.

Entrada construída: 12x36 diurno previsto `07:00–19:00`, pausa `12:00–13:00`, dia explicitamente de trabalho, previsão líquida **660 minutos**; atestado de **480 minutos**, um único dia, tipo `atestado` habilitado para minutos. Ordem: cadastrar escala/célula, lançar atestado de 480 minutos, tentar aprovar com período `07:00–15:00`; alternativamente, aprovar sem período. Não é preciso supor batidas para demonstrar a recusa; use zero batidas para mostrar a dispensa integral. O previsto deriva dos marcos e pausas: `app/escala/utils.py:2287`, `app/escala/utils.py:2293`, `app/escala/utils.py:2312`.

Caminho e resposta:

- A porta aceita minutos em tipo parcial cadastrado: `app/ponto/services/ausencia.py:206`.
- O juiz fixa `MINUTOS_DIA_INTEIRO=480`; exige `0 < minutos < 480` para considerar parcial, sem perguntar o previsto do dia: `app/ponto/turnos.py:2580`, `app/ponto/turnos.py:2598`.
- Aprovação com período passa por `_validar_periodo`, que recusa **“So ausencia PARCIAL tem periodo.”**: `app/ponto/services/ausencia.py:890`, `app/ponto/services/ausencia.py:863`.
- Sem período, o registro é considerado inteiro, a grade marca `cobertura_parcial=False` e apaga todas as células e órfãs: `app/escala/utils.py:1210`, `app/escala/utils.py:1251`, `app/escala/utils.py:1272`.
- A mesma promoção alcança o conjunto de ausência integral que zera atraso/saída antecipada: `app/ponto/turnos.py:2668`, `app/ponto/turnos.py:2684`, `app/ponto/services/fechamento.py:683`, `app/ponto/services/fechamento.py:742`, `app/ponto/motor_calculo_v2.py:1092`.
- O cartório dispensa as lâmpadas não acesas quando a grade não marcou parcial: `app/ponto/services/cartorio.py:763`.

Regra aplicável: atestado por minutos não abona o dia inteiro; o próprio contrato documentado do efeito da ausência distingue o crédito parcial do dia integral: `app/ponto/services/efeito_ausencia.py:19`. Portanto, **480 não explica 660 minutos**, e não pode dispensar os 180 minutos de jornada restantes por simples limiar de oito horas. Valor líquido exato de cobertura de um documento `07:00–15:00` com pausa dentro: [nao verificado]; a falha de aceitar um período parcial é independente desse cálculo.

Ressalva relevante: os comentários admitem que os 480 nasceram como aval para **“so o contador”**, mas o predicado hoje alimenta portas, grade e desconto: `app/ponto/turnos.py:2575`. Certeza alta na recusa e classificação integral; regra jurídica específica externa: [nao verificado]. Dono L-099: **ESTRUTURA**, pois o programa ignora o previsto conhecido ao interpretar a mesma quantidade de minutos; `app/docs/LEIS.md:122`.

O mesmo limiar produz o mesmo problema de classificação em 12x36 noturno `19:00–07:00` com pausa `02:00–03:00`, previsão líquida 660; o encadeamento atravessa a meia-noite em `app/escala/servico_jornada.py:80`. Uma entrada concreta para aprovação seria atestado480 com período `19:00–03:00`; a mesma recusa ocorre antes de qualquer cálculo noturno: `app/ponto/services/ausencia.py:892`. A porta reconhece período noturno `fim<inicio`; o impedimento é o falso “inteiro”: `app/ponto/services/ausencia.py:863`.

3. Outro elo relevante para a consolidação: parcial120 pode ganhar todo o restante do previsto no DiaPago.

A grade marca qualquer ausência não-falta como `tipo_dia='ausencia'`, mantendo essa classificação também na parcial: `app/escala/utils.py:1241`, `app/escala/utils.py:1245`. O leitor de célula transporta a parcial e os números lavrados: `app/escala/services/leitor_celula.py:389`, `app/escala/services/leitor_celula.py:404`.

`por_dia_da_grade` calcula **abono = previsto − realizado** para todo dia `ausencia`, sem consultar minutos do atestado ou `cobertura_parcial`: `app/ponto/services/dia_pago.py:385`, `app/ponto/services/dia_pago.py:391`. Esse mapa vai para o fechamento: `app/ponto/services/fechamento.py:1044`.

Exemplo aritmético na entrada dessa função: dia previsto660, realizado360, atestado120 → **abono300**, embora o contrato do parcial diga que só os minutos declarados viram crédito: `app/ponto/services/efeito_ausencia.py:19`. Recomendo ao pai verificar a montagem integral desse dia e o leitor concorrente antes de publicar como contraexemplo independente; minha certeza é alta no cálculo local e a integração ponta a ponta permanece [nao verificado].

## 05/11 15:32 · sessao 9e4b4b7477c4 · - · - · origem=Ronald (tmux)

Três contra-exemplos estáticos fortes; ocorrência em produção e execução nesta cópia [nao verificado].

| Caso | Entrada concreta e ordem | Código hoje × regra | Leitores divergentes | Certeza / L-099 |
|---|---|---|---|---|
| E-A — atestado posterior mantém desconto | 6x1, 03/11/2026 previsto 480 min, sem batidas. Primeiro falta aprovada, `minutos=0`; depois atestado do mesmo dia, `minutos=0`, anexado e aprovado. A falta deriva 480 min: `app/ponto/models.py:875`. A porta permite abono sobre falta: `app/ponto/services/ausencia.py:309`, `app/ponto/precedencia.py:106`. | Cobertura escolhe atestado por maior PK no empate de data: `app/ponto/turnos.py:2753`. Grade vira ausência: `app/escala/utils.py:1231`; abono = 480: `app/ponto/services/dia_pago.py:391`. Porém falta aprovada continua somada sem resolver cobertura: `app/folha/export.py:129`; fechamento desconta 8 h: `app/ponto/services/fechamento.py:865`, `:987`; DiaPago também: `app/ponto/services/dia_pago.py:272`. Regra: falta exige ausência de justificativa: `app/docs/LEIS.md:120`; aprovação do atestado vence falta: `app/ponto/precedencia.py:119`. | Grade/abono 480 min contra falta 8 h no próprio DiaPago; TXT inclui 1 dia, se elegível e rubrica configurada: `app/folha/export.py:721`, `:204`. | Alta por leitura; **ESTRUTURA**: `app/docs/LEIS.md:122`. |
| E-B — data inicial vence ordem real do lançamento | Mesma escala, sem batidas. Primeiro falta aprovada em 03/11; depois atestado aprovado de 02–04/11, `dias_corridos=3`, `minutos=0`. Derivação inclusiva: `app/ponto/services/ausencia.py:76`; permissão sobre falta: `:309`. | Em 03/11, cobertura escolhe falta porque sua `data=03/11` supera `02/11`, mesmo com atestado lançado depois: `app/ponto/turnos.py:2753`. Grade vira falta: `app/escala/utils.py:1234`. Já precedência ordena ausências por data crescente: `app/ponto/precedencia.py:273`; falta e atestado ocupam mesma faixa: `:52`, `:76`; ordenação apenas por faixa conserva atestado primeiro: `:380`, `:396`. Regra expressa diz atestado ganha de falta na aprovação: `:119`; falta com justificativa contraria `app/docs/LEIS.md:120`. | `ata.precedencia.tipo_do_dia='atestado'`, gravado em `app/ponto/services/cartorio.py:1947`, contra grade `tipo_dia='falta'` e desconto 480 min: `app/escala/utils.py:1234`, `app/folha/export.py:129`. | Alta por leitura; **ESTRUTURA**: `app/docs/LEIS.md:122`. |
| E-C — falta inteira distribuída no dia errado | 6x1, folga domingo; padrão 08–17, intervalo 12–13; sábado reduzido 08–12 por `horarios_por_dia={"5":{"hi":"08:00","hf":"12:00"}}`, cadastro nativo: `app/escala/models.py:193`, `:198`, `:336`. Sem batidas em sexta 30/10 e sábado 31/10. Lançar uma falta aprovada de dois dias, minutos zero. | Modelo soma previstos: 480+240=720 min: `app/ponto/models.py:875`. Export divide igualmente pelos dois dias: 360+360, `app/folha/export.py:102`; mapa segue à lavratura: `app/ponto/services/fechamento.py:1184`; DiaPago registra 6 h+6 h: `app/ponto/services/dia_pago.py:272`. Regra exige custo previsto **daquele dia**, portanto 8 h+4 h: `app/docs/LEIS.md:120`. | Previsto por dia 480/240 contra falta DiaPago 360/360; total mensal permanece 720: `app/ponto/models.py:883`, `app/folha/export.py:110`, `app/ponto/services/dia_pago.py:274`. | Alta por leitura; **ESTRUTURA**: `app/docs/LEIS.md:122`. |

Ressalvas úteis para consolidação:

- **Atestado sobre folga humana é vetado atualmente**, não usar como bug antigo: `app/ponto/precedencia.py:509`.
- **Atestado sobre folga de template entra**, mas a troca de rótulo para ausência não prova dinheiro indevido: `app/ponto/precedencia.py:435`, `app/escala/utils.py:1231`. O dia de folga recebe previsto zero: `app/escala/utils.py:1822`; abono resulta zero: `app/ponto/services/dia_pago.py:391`.
- `datas_abonadas()` conta datas sem filtrar folga: `app/folha/export.py:190`; chegada dessa função à rubrica 8932 do TXT [nao verificado]. Não encontrei chamador de produção na busca.
- Nenhum desses exemplos depende de inventar pessoa ou registro do dado raspado; sua ocorrência real no hash [nao verificado].

## 05/11 15:30 · sessao be86a22ee392 · - · - · origem=Ronald (tmux)

Bases para as duas tabelas: horários locais UTC−3 (L-080, `app/docs/LEIS.md:107`); noturno atribuído integralmente ao dia de início (L-085, `app/docs/LEIS.md:117`); `P/T/F/A/N/HE` = previsto/trabalhado/falta decidida/abono/noturno/extra, em minutos. O cadastro de prorrogação pós05h precisa constar explicitamente: cl.38-d fixa hora60 e adicional20%, mas deixa a prorrogação para o cadastro (L-035/L-036, `app/docs/LEIS.md:83`, `app/docs/LEIS.md:84`; texto da cláusula em `app/core/regua_cct.py:129`).

**Propostas 12x36 noturno:** adotar cadastro CCT Londrina vigente, 19–07, pausa01–02, prorrogação pós05h=False; foto/paridade declarando o dia como trabalho. Assim P=660 e N=360 (`app/docs/LEIS.md:73`, `app/docs/LEIS.md:80`, `app/docs/LEIS.md:83`, `app/docs/LEIS.md:84`).

| id | cadastro | batidas | lançamentos, na ordem | resposta da regra | regra citada | leitores |
|---|---|---|---|---|---|---|
| N1 | Base acima; competência aberta | D19:00, D+1 01:00/02:00/07:00 | cadastrar vínculo e foto → apurar | `660/660/0/0/360/0`; 300min diurnos+360min noturnos=660; adicional20% sobre N | L-024/032/035/036/085, `LEIS.md:75`, `:80`, `:83`, `:84`, `:117` | Todos os valores coincidem; nenhuma pergunta pelo turno completo: L-002/061/095, `LEIS.md:60`, `:95`, `:113` |
| N2 | Antes diurno07–19; correção administrativa comprovada para noturno19–07 desde D | Mesmas de N1 | declarar vigência D → reescrever célula pela exceção → rejulgar | Resultado N1; dia D−1 conserva seu vínculo e seus marcos; cadastro incorreto não pode zerar trabalho | L-020/024/084/112, `LEIS.md:71`, `:75`, `:116`, `:150`; vínculo do dia, `GLOSSARIO.md:24` | Célula denuncia correção; todos passam a N1; chamado lê o fato atual, `LEIS.md:95` |
| N3 | Base | D19:00, D+1 01:00/02:00; saída equivocada06:00 | retratar06:00 com motivo → lançar saída correta07:00 → rejulgar | Antes T600, saída antecipada60; depois N1. N continua360; falta decidida não nasce de simples déficit | L-025/039/093, `LEIS.md:76`, `:87`, `:111`; porta e trilha, `CORTES.md:182` | Marca06:00 continua no histórico cru como retratada; apuração usa07:00. Pergunta sem fato desaparece, L-025/061 |
| N4 | Dia anterior ao horizonte, já fotografado com T600 | Como N3 | pouso de regra nova, sem fato → depois correção humana06→07 | Só pouso conserva fotografia600; fato novo rejulga e leva a N1; exportação antiga permanece documento da versão anterior | L-124/113, `LEIS.md:145`, `:149`; `CORTES.md:419` | Todos leem fotografia antes do fato; depois todos leem660, preservando TXT anterior | [nao verificado]
| N5 | Base; duas pessoas/dias equivalentes | Em cada dia19:00,01:00/02:00/07:30 | apurar lote → “Não” para ponta30min de A → autorizar30min de B → repetir lote | A=`660/660/0/0/360/0`; B=`660/690/0/0/360/30`. Antes da decisão ambas têm HE0 e30min pendentes. Minuto posterior05h não integra N porque cadastro=False | L-097/035/110, `LEIS.md:115`, `:83`, `:136` | A mostra ponta riscada e ciência; B mostra30 autorizados; pendência resolvida sem duplicar no replay; trilha por dia |
| N6 | Base, alterando explicitamente prorrogação pós05h=True | N1 | declarar cadastro aplicável → rejulgar dia alcançado | `660/660/0/0/480/0`; N=22–07(540)−pausa60=480; 180diurnos+480noturnos=660. Hora continua60 | L-035/036, `LEIS.md:83`, `:84`; intervalo na mesma janela, `CORTES.md:418` | Todos exibem480, não360 nem hora reduzida; cadastro efetivo/vigência precisa estar explícito |

**Propostas personalizado 6h corridas:** definir cadastro08–14, intervalo0, jornada360, dias de trabalho explícitos; o desenho manda, não o rótulo (`app/docs/LEIS.md:75`, `app/docs/LEIS.md:124`). **lei:** a legalidade de seis horas sem os15min da CLT art.71§1 e a indenização correspondente exigem fonte CLT primária; esse artigo integral não está nos trechos normativos consultados [não verificado]. Não apresentar esta escala como juridicamente regular só por existir cadastro; preservar a aritmética do trabalho e separar a rubrica de intervalo [não verificado].

| id | cadastro | batidas | lançamentos | resposta aritmética pela grade; ressalva legal acima | regra citada | leitores |
|---|---|---|---|---|---|---|
| P1 | 08–14, sem intervalo | 08:00/14:00 | cadastrar → apurar | `360/360/0/0/0/0`; nenhuma hora fictícia de pausa subtraída | L-024/091, `LEIS.md:75`, `:124` | Todos360; **lei:** rubrica legal de intervalo, separada de HE |
| P2 | Antes08–14; mudança prospectiva para09–15 desde D | D−1 08–14; D09–15 | encerrar antigo em D−1 → declarar novo D → apurar | Cada dia `360/360/0/0/0/0`; marcos diferentes por vigência; nenhuma alteração no D−1 | L-020/021/112, `LEIS.md:71`, `:72`, `:150`; `GLOSSARIO.md:24` | Célula/espelho/app/DiaPago usam vínculo do dia |
| P3 | Base | 08:00/13:00 equivocado | retratar13:00 → lançar14:00 correto → rejulgar | Antes T300 e antecipada60; depois P1; F0 sem lançamento de falta | L-025/039/093, `LEIS.md:76`, `:87`, `:111`; `CORTES.md:182` | Histórico preserva13 retratada; todos360 depois; nenhum leitor mantém300 |
| P4 | Dia antigo antes do horizonte com T300 | Como P3 | apenas pouso → posteriormente corrigir batida | Pouso mantém300; fato novo leva360, sem backfill geral | L-124/113, `LEIS.md:145`, `:149` | Fotografia antes; valor corrigido depois em todos; TXT anterior intacto | [nao verificado]
| P5 | Base; dois dias equivalentes | 08:00/14:30 em ambos | lote → negar ponta de A → autorizar ponta de B → repetir | A=`360/360/0/0/0/0`; B=`360/390/0/0/0/30`; inicialmente30 pendentes em cada | L-097/110, `LEIS.md:115`, `:136` | Riscar30 em A; mostrar HE30 em B; sem duplicação pelo lote |
| P6 | Base; D previsto e D+1 folga avulsa da pessoa | Nenhuma | lançar abono integralD → trocar vínculo → reapurar lote | D=`360/0/0/360/0/0`; D+1=`0/0/0/0/0/0`; folga avulsa sobrevive à troca; calendário do vínculo antigo não acompanha o novo | L-089, `LEIS.md:121`; abono só residual, `CORTES.md:88`; folga pessoa×vínculo, `CORTES.md:417` | Todos mostram abono360 somenteD; não cobrar trabalho da folga |

Para quaisquer tabelas, o chamado deve seguir a pergunta viva derivada do fato, não “fechar chamado” como prova do pagamento (`app/docs/CORTES.md:316`, `app/docs/CORTES.md:322`). Evitar prometer um rótulo exato de status não definido pela regra; pode dizer “sem cobrança desse fato” e deixar **lei:** para enumeração do status (`app/docs/LEIS.md:95`, `app/docs/LEIS.md:136`).

## 05/11 15:30 · sessao 657665964edf · - · - · origem=Ronald (tmux)

Sugestão: oito casos por escala, todos com `N=0/HE=0`; unidades em minutos. São cenários propostos, sem execução. [não verificado]

| Caso | 6x1: 07–12/13–15:20 | 12x36 diurno: 07–12/13–19 | Fundamento |
|---|---|---|---|
| 1. Cadastro conforme | P440/T440/F0/A0 | P660/T660/F0/A0 | Previsto vem da grade e intervalo cadastrado sai da jornada: `app/docs/LEIS.md:75`, `app/docs/LEIS.md:80`. |
| 2. Troca na aberta | Vínculo anterior termina D−1; novo 6x1 começa D; batidas completas em D → P440/T440/F0/A0; D−1 conserva sua escala | Vínculo anterior termina D−1; novo 12x36 começa D, com foto/fase explicitamente declarada; batidas completas → P660/T660/F0/A0 | Vigência sem fim anterior ao início; início declarado; exceção formal para cadastro errado: `app/docs/LEIS.md:72`, `app/docs/LEIS.md:150`, `app/docs/LEIS.md:71`. |
| 3. Corrigir saída | Retratar 15:00 e lançar 15:20: T420→440; P440/F0/A0 | Retratar 18:00 e lançar 19:00: T600→660; P660/F0/A0 | Retratada fora da apuração; marcações cruas continuam exibidas: `app/docs/LEIS.md:76`; log imutável com compensação: `app/docs/GLOSSARIO.md:25`. |
| 4. Retro sem fato novo | Foto histórica P440/T420/F0/A0 permanece após pouso de obra nova | Foto histórica P660/T600/F0/A0 permanece | Passado fica foto até fato novo, sem backfill a cada pouso: `app/docs/LEIS.md:145`, `app/docs/CORTES.md:419`. |
| 5. Retro com fato novo | Na foto do caso 4, corrigir saída 15:00→15:20 → P440/T440/F0/A0 | Na foto do caso 4, corrigir saída 18:00→19:00 → P660/T660/F0/A0 | Fato novo rejulga normalmente em qualquer competência; cálculo usa o que tem: `app/docs/LEIS.md:145`, `app/docs/LEIS.md:149`. |
| 6. Lote e repetição | Corrigir dois dias iguais ao caso 3 → cada um440; agregado880; repetir não duplica valores | Corrigir dois dias iguais ao caso 3 → cada um660; agregado1320; repetir não duplica valores | Idempotência e partes somam total: `app/docs/LEIS.md:136`, `app/docs/LEIS.md:148`; trilha por ação crítica: `app/docs/LEIS.md:65`. |
| 7. Ausência abonada | Sem batidas; aprovar ausência integral de tipo abonador → P440/T0/F0/A440 | Sem batidas; aprovar ausência integral abonadora → P660/T0/F0/A660 | Ausência tem efeito no dia previsto; justificativa distingue falta; custo do dia vem do previsto: `app/docs/LEIS.md:120`, `app/docs/LEIS.md:121`. |
| 8. Falta decidida | Sem batidas; aprovar ausência integral descontável → P440/T0/F440/A0 | Sem batidas; aprovar ausência integral descontável → P660/T0/F660/A0 | Falta exige ausência lançada de tipo descontável: `app/docs/LEIS.md:87`; custo previsto daquele dia: `app/docs/LEIS.md:120`. |

Nos casos 3 e 4, antes da correção, o déficit de 20/60 não é automaticamente uma falta decidida; a antecipada deve respeitar o teto P−T. `app/docs/LEIS.md:87`, `app/docs/LEIS.md:111`.

Espelho, app, célula e DiaPago devem representar a mesma lavratura; chamado/pergunta lê o estado da célula, sem recálculo próprio. `app/docs/LEIS.md:60`, `app/docs/LEIS.md:95`, `app/docs/LEIS.md:113`; projeções reconstruíveis: `app/docs/GLOSSARIO.md:22`.

**lei:** atomicidade do lote em falha parcial, prazo máximo de atualização das projeções e nome literal do status final do chamado não ficaram definidos pelos textos consultados. [não verificado]

**lei:** impedir emissão durante tentativas intermediárias de troca não pode ser presumido; o próprio corte remete essa condição a pergunta de lei. `app/docs/CORTES.md:310`.

Folga avulsa pertence à pessoa e sobrevive à troca; folga do calendário pertence ao vínculo e vale somente na vigência dele. `app/docs/CORTES.md:417`.

## 05/11 15:29 · sessao 6d501cd6306e · - · - · origem=Ronald (tmux)

- **Fonte e invariantes:** resposta nasce da regra aplicada ao cadastro e às batidas; código atual não é gabarito. Todo caso deve exigir partes somando total, idempotência, previsto independente do realizado e igualdade entre leitores. `app/docs/LEIS.md:136` (L-110); invariantes valem também para dados sujos. `app/docs/LEIS.md:148` (L-114).

- **Troca de escala:** grade é fonte única do previsto; mudança exige vigência válida, sem fim anterior ao início. `app/docs/LEIS.md:75` (L-024), `app/docs/LEIS.md:72` (L-021). Admin declara início e fase inclusive antes da primeira batida. `app/docs/LEIS.md:150` (L-112). Passado errado por cadastro admite regeneração formal denunciada pela célula. `app/docs/LEIS.md:71` (L-020).

- **Folgas durante troca:** folga de calendário pertence ao vínculo e só vale na sua vigência; folga avulsa manual pertence à pessoa e sobrevive à troca. `app/docs/CORTES.md:417`. Calendário com uso muda por versão com vigência, preservando o passado. `app/docs/CORTES.md:291`.

- **Correção de batidas:** retratar nunca apaga: guarda `retratada_em`, autor, instante e motivo; rejulga o dia imediatamente; desfazer disponível por sete dias. `app/docs/CORTES.md:182`. Apuração exclui retratadas; exibição de marcação mantém o cru. `app/docs/LEIS.md:76` (L-025); log imutável com evento compensatório. `app/docs/GLOSSARIO.md:25`.

- **Reatividade e lote:** depois de regenerar células, chamar a porta de dinheiro uma vez por competência tocada, com motivo do vínculo; lote segue por job. `app/docs/CORTES.md:229`. Ação crítica exige usuário, antes/depois e motivo. `app/docs/LEIS.md:65` (L-007). Não inferir atomicidade integral do lote nem política de falha parcial: **lei:** essas condições não estão decididas nesse corte. `app/docs/CORTES.md:229`.

- **Retroativo L-124:** obra nova alcança horizonte em diante; passado permanece fotografia até fato novo; alcance geral retroativo ocorre no backfill único final. Fato novo rejulga qualquer competência; correção provada em exportada segue a porta vigente. `app/docs/LEIS.md:145`, `app/docs/CORTES.md:419`. L-113 permite recalcular exportada, conservando TXT anterior, trilha e DIFF anterior a ato de frota. `app/docs/LEIS.md:149`. **lei:** não escolher arbitrariamente uma versão numérica da regra para exemplo anterior ao horizonte; declarar a fotografia inicial do caso e distinguir pouso sem fato de correção com fato novo. `app/docs/LEIS.md:145`.

- **Leitores:** espelho, app, calendário, fechamento e TXT leem o dinheiro lavrado; não derivam novamente. `app/docs/LEIS.md:113` (L-095). Chamado/pergunta leem célula/ata. `app/docs/LEIS.md:95` (L-061). Espelho e DiaPago são projeções reconstruíveis. `app/docs/GLOSSARIO.md:22`; reconstrução pela porta após cura de juiz em competência não exportada. `app/docs/LEIS.md:142` (L-121).

- **Furo e falta:** falta depende de ausência descontável ou veredito explícito; furo indecidido deve aparecer como “dias em aberto (a decidir)”, com número. `app/docs/CORTES.md:179`. Marcação humana do dia leva identificação de quem/quando/o quê, lida da ata. `app/docs/CORTES.md:194`.

- **Perguntas de lei relevantes:** segurar emissão de cobranças enquanto admin experimenta versões antes de confirmar aparece expressamente como questão de lei, sem decisão fechada. `app/docs/CORTES.md:310`. Não converter estado do chamado em prova de regularidade: pergunta viva segue autoridade própria e pode aparecer em disputa fechada sem reabertura em lote. `app/docs/CORTES.md:316`.

## 05/11 15:29 · sessao ee5632eb0b51 · BATERIA-QUEIXAS-ADMIN · leitura · origem=Code

**BATERIA-QUEIXAS-ADMIN — passo 1, Q1: falta e atestado.** Gabarito proposto pela regra, sem usar o comportamento da implementação como referência. (L-110, `app/docs/LEIS.md:136`)

Todos os cadastros, horários e lançamentos abaixo são **cenários sintéticos**, não registros de produção. `[nao verificado]`

**Convenções para ler as tabelas**

- Valores em minutos: **P** previsto; **T** trabalhado dentro da jornada; **F** ausência decidida que desconta; **A** abono; **U** período ainda em aberto; **N** noturno efetivamente trabalhado; **HE** hora extra. F parcial não significa um dia inteiro de falta. (`app/docs/LEIS.md:120`; `app/docs/CORTES.md:202`; `app/docs/CORTES.md:240`)
- Horários locais, UTC−3; no noturno, `+1` significa o dia seguinte. A jornada pertence ao dia do início. (`app/docs/LEIS.md:107`; `app/docs/LEIS.md:117`)
- “Atestado aprovado” significa documento aceito para abonar **o período explicitamente informado**. Não se presume abono integral de uma declaração de comparecimento. (`app/docs/CORTES.md:315`; `app/docs/CORTES.md:319`; [CFM, art. 4º](https://sistemas.cfm.org.br/normas/arquivos/resolucoes/BR/2024/2381_2024.pdf))
- Os intervalos indicados são gozados ou estão integralmente dentro do afastamento aprovado; não há minutos externos à jornada nestes casos, portanto **HE=0**. (`app/docs/LEIS.md:80`; `app/docs/LEIS.md:115`)

A coluna **leitores** usa estes contratos, aplicáveis a cada linha:

| Código | Espelho, app, célula, DiaPago e chamado devem mostrar |
|---|---|
| **R-F** | Espelho e app: **Falta decidida**, com o desconto da linha. Célula: mesma decisão e minutos. DiaPago: mesmo efeito de falta. Chamado: não continuar perguntando uma ausência já decidida; preservar histórico. (`app/docs/LEIS.md:87`; `app/docs/LEIS.md:95`; `app/docs/LEIS.md:113`; `app/docs/CORTES.md:198`) |
| **R-P** | Espelho e app: **“Saída antecipada — desconta 2h”**, preservando o trabalho realizado. Célula e DiaPago: F=120, sem falta inteira adicional. Chamado: refletir essa decisão. (`app/docs/CORTES.md:202`; `app/docs/LEIS.md:95`; `app/docs/LEIS.md:111`) |
| **R-A** | Espelho e app: **Atestado**, distinguindo trabalho e abono. Célula e DiaPago: mesmos T/A/F e rubricas. Chamado: não cobrar marcos cobertos; cobertura parcial não elimina cobrança fora do período. (`app/docs/CORTES.md:312`; `app/docs/CORTES.md:315`; `app/docs/LEIS.md:95`; `app/docs/GLOSSARIO.md:22`) |
| **R-L** | Todos preservam **Folga**, sem falta, abono ou consumo de saldo. Chamado: nenhuma cobrança de jornada nessa folga. (`app/docs/LEIS.md:121`; `app/docs/CORTES.md:416`) |
| **R-X** | Mostrar falta anterior e atestado posterior, períodos, situação e documento; oferecer resolução ao admin. **lei:** precedência automática e operação de substituição não estão definidas nos textos examinados. Não fabricar resultado definitivo. (`app/docs/CORTES.md:338`; `[nao verificado]`) |
| **R-C** | Mostrar trabalho e atestado conflitantes; célula registra o conflito e os leitores o reproduzem. Não duplicar trabalho com abono. Havendo batida dentro do período declarado, cabe **FATO_EM_AUSENCIA** e decisão do admin. (`app/docs/CORTES.md:277`; `app/docs/CORTES.md:315`; `app/docs/LEIS.md:95`) |
| **R-U** | Espelho e app: **Em aberto — a decidir**, sem chamar de falta. Célula mantém a pendência; DiaPago não apresenta ausência como falta decidida nem como abono. Chamado/pergunta permanece alcançável para regularização. (`app/docs/CORTES.md:179`; `app/docs/LEIS.md:120`; `app/docs/LEIS.md:141`) |

**lei:** esses contratos definem conteúdo e coerência; não inventam um nome de status terminal para todo chamado de atestado. O corte que manda encerrar o chamado de origem fala especificamente de aprovação de **Justificativa**. (`app/docs/CORTES.md:362`; `[nao verificado]`)

**6x1 — 44h semanais; 08:00–16:20, intervalo 12:00–13:00**

Cadastro **S6**: segunda a sábado, domingo de folga; 440 minutos líquidos por dia, total semanal 2.640 minutos. O intervalo não integra o previsto. (Premissa sintética: `[nao verificado]`; exclusão do intervalo: L-032, `app/docs/LEIS.md:80`)

| id | cadastro | batidas | lançamentos — na ordem | resposta da regra | regra citada | leitores |
|---|---|---|---|---|---|---|
| Q1-6-01 | S6; D útil | Nenhuma | 1. Admin decide falta integral de D | **P440 T0 F440 A0 U0 N0 HE0** | L-039/L-088 — `app/docs/LEIS.md:87`; `app/docs/LEIS.md:120` | **R-F**; desconto de 440 min |
| Q1-6-02 | S6; D útil | 08:00, 12:00, 13:00, 14:20 | 1. Admin decide saída antecipada 14:20–16:20, sem abono | **P440 T320 F120 A0 U0 N0 HE0**; desconto parcial, sem somar falta inteira | `app/docs/CORTES.md:202`; L-093 — `app/docs/LEIS.md:111` | **R-P**; trabalho 5h20, desconto 2h |
| Q1-6-03 | S6; D útil | 10:00, 12:00, 13:00, 16:20 | 1. Receber atestado 08:00–10:00; 2. Aprovar abono desse período | **P440 T320 F0 A120 U0 N0 HE0** | `app/docs/CORTES.md:312`; `app/docs/CORTES.md:315` | **R-A**; trabalho 5h20 + abono 2h |
| Q1-6-04 | S6; D e D+1 úteis | Nenhuma nos dois dias | 1. Receber atestado de dois dias; 2. Aprovar ambos | Cada dia: **P440 T0 F0 A440 U0 N0 HE0**. Total: **P880 A880** | L-088/L-089 — `app/docs/LEIS.md:120`; `app/docs/LEIS.md:121`; `app/docs/CORTES.md:277` | **R-A** por dia; sem falta nesses períodos |
| Q1-6-05 | S6; domingo de folga | Nenhuma | 1. Registrar documento cobrindo somente domingo | **P0 T0 F0 A0 U0 N0 HE0**; não consumir saldo nem transportar abono para segunda | L-089 — `app/docs/LEIS.md:121` | **R-L** |
| Q1-6-06 | S6; D útil | Nenhuma | 1. Aprovar falta integral; 2. Depois receber atestado integral de D | Antes: **P440 T0 F440 A0**. **lei:** recepção posterior não define, sozinha, substituição. Se a decisão anterior for regularizada e o atestado aceito como cobertura: **F0 A440** | `app/docs/CORTES.md:338`; `app/docs/LEIS.md:120`; `[nao verificado]` para precedência | **R-X**; após decisão válida, **R-A**, preservando trilha |
| Q1-6-07 | S6; D útil | 08:00, 12:00, 13:00, 16:20 | 1. Registrar atestado parcial 11:30–13:30 sobre jornada já realizada | **P440 T440 F0 A0 U0 N0 HE0** na decomposição temporal; não acrescentar abono sobre trabalho. Batidas 12:00/13:00 estão no período declarado: conflito requer decisão | `app/docs/CORTES.md:277`; `app/docs/CORTES.md:315` | **R-C**; não apresentar pagamento adicional de 2h |
| Q1-6-08 | S6; D encerrado | Nenhuma | Nenhuma decisão ou justificativa | **P440 T0 F0 A0 U440 N0 HE0**; não converter U440 automaticamente em F440 | L-039/L-088 — `app/docs/LEIS.md:87`; `app/docs/LEIS.md:120`; `app/docs/CORTES.md:179` | **R-U** |

**12x36 diurno — 07:00–19:00, intervalo 12:00–13:00**

Cadastro **SD**: plantões D e D+2; D+1 de folga; **P660 por plantão**, resultante de 720 minutos de envelope menos 60 de intervalo. (Premissa sintética: `[nao verificado]`; L-032, `app/docs/LEIS.md:80`)

| id | cadastro | batidas | lançamentos — na ordem | resposta da regra | regra citada | leitores |
|---|---|---|---|---|---|---|
| Q1-D-01 | SD; D de plantão | Nenhuma | 1. Admin decide falta integral | **P660 T0 F660 A0 U0 N0 HE0**; falta custa 11h, não 12h | `app/docs/LEIS.md:80`; `app/docs/LEIS.md:120` | **R-F**; desconto de 660 min |
| Q1-D-02 | SD; D de plantão | 07:00, 12:00, 13:00, 17:00 | 1. Decidir saída antecipada 17:00–19:00, sem abono | **P660 T540 F120 A0 U0 N0 HE0** | `app/docs/CORTES.md:202`; `app/docs/LEIS.md:111` | **R-P**; trabalho 9h, desconto 2h |
| Q1-D-03 | SD; D de plantão | 09:00, 12:00, 13:00, 19:00 | 1. Receber atestado 07:00–09:00; 2. Aprovar período | **P660 T540 F0 A120 U0 N0 HE0** | `app/docs/CORTES.md:312`; `app/docs/CORTES.md:315` | **R-A** |
| Q1-D-04 | SD; D e D+2 plantões; D+1 folga | Nenhuma | 1. Receber atestado de três dias, D até D+2; 2. Aprovar | D e D+2: cada qual **P660 T0 F0 A660 U0 N0 HE0**. D+1: tudo zero. Total **P1320 A1320**, não A1980 | `app/docs/LEIS.md:80`; L-089 — `app/docs/LEIS.md:121`; `app/docs/CORTES.md:277` | **R-A** nos plantões; **R-L** na folga |
| Q1-D-05 | SD; D+1 folga | Nenhuma | 1. Registrar atestado somente de D+1 | **P0 T0 F0 A0 U0 N0 HE0** | L-089 — `app/docs/LEIS.md:121` | **R-L** |
| Q1-D-06 | SD; D de plantão | Nenhuma | 1. Aprovar falta integral; 2. Depois receber atestado integral | Antes: **P660 T0 F660 A0**. **lei:** substituição automática não definida. Após regularização da falta e aceitação do atestado: **F0 A660** | `app/docs/CORTES.md:338`; `app/docs/LEIS.md:120`; `[nao verificado]` para precedência | **R-X** → **R-A**, quando decidida a colisão |
| Q1-D-07 | SD; D de plantão | 07:00, 12:00, 13:00, 19:00 | 1. Registrar atestado 11:30–13:30 sobre trabalho realizado | **P660 T660 F0 A0 U0 N0 HE0** temporalmente; batidas dentro do período geram conflito, sem abono duplicado | `app/docs/CORTES.md:277`; `app/docs/CORTES.md:315` | **R-C** |
| Q1-D-08 | SD; plantão encerrado | Nenhuma | Nenhuma decisão ou justificativa | **P660 T0 F0 A0 U660 N0 HE0**; ainda não é falta decidida | `app/docs/LEIS.md:87`; `app/docs/LEIS.md:120`; `app/docs/CORTES.md:179` | **R-U** |

**12x36 noturno — 19:00–07:00+1, intervalo 01:00–02:00+1**

Cadastro **SN**: CCT Vigilantes Londrina aplicável; hora de **60 minutos**, adicional de **20%**; P660 por plantão. Não converter 60 minutos físicos em 68,57 minutos pela hora reduzida. (Premissa de aplicação: `[nao verificado]`; cláusula 38-d, [`app/core/regua_cct.py:132`](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/core/regua_cct.py:132); L-036, `app/docs/LEIS.md:84`; intervalo, `app/docs/LEIS.md:80`)

**† lei: pós-05h.** A cláusula transcrita não resolve a prorrogação; L-035 remete à CCT cadastrada. Nas linhas com trabalho 05:00–07:00, **δ** representa os 120 minutos cuja inclusão no adicional depende dessa definição: δ=0 ou δ=120. O corte sobre SINEEPRES não prova, sozinho, a correspondência desse sindicato com o cadastro sintético SN. (`app/docs/LEIS.md:83`; `app/docs/CORTES.md:279`; `app/core/regua_cct.py:132`; `[nao verificado]`)

**‡ lei: noturno remunerado no afastamento.** N abaixo mede trabalho noturno efetivo. Não concluo que a rubrica remuneratória de adicional deva zerar durante atestado: a integração salarial do afastamento não está definida nesses textos. (`app/docs/LEIS.md:113`; `app/core/regua_cct.py:132`; `[nao verificado]`)

| id | cadastro | batidas | lançamentos — na ordem | resposta da regra | regra citada | leitores |
|---|---|---|---|---|---|---|
| Q1-N-01 | SN; plantão D | Nenhuma | 1. Admin decide falta integral do plantão | **P660 T0 F660 A0 U0 N0 HE0** | `app/docs/LEIS.md:80`; `app/docs/LEIS.md:120`; `app/docs/LEIS.md:117` | **R-F**; uma falta do plantão D, sem outra em D+1 |
| Q1-N-02 | SN; plantão D | D 19:00; +1 01:00, 02:00, 05:00 | 1. Decidir saída antecipada 05:00–07:00+1, sem abono | **P660 T540 F120 A0 U0 N360 HE0**. N=22:00–05:00 menos intervalo de 60 min | `app/docs/CORTES.md:202`; `app/docs/LEIS.md:84`; `app/docs/LEIS.md:111`; `app/docs/LEIS.md:117` | **R-P**; N360 a 20%, sem redução fictícia da hora |
| Q1-N-03 | SN; plantão D | D 19:00, 23:00; +1 03:00, 07:00 | 1. Receber atestado D 23:00–03:00+1; 2. Aprovar período | **P660 T480 F0 A180 U0 N(180+δ) HE0**. Abono: 240 min de envelope menos intervalo 01:00–02:00. **lei: AN pago do afastamento‡** | `app/docs/CORTES.md:330`; `app/docs/CORTES.md:312`; `app/docs/LEIS.md:80`; `app/docs/LEIS.md:83` | **R-A**; cobertura atravessa meia-noite e pertence a D; †‡ |
| Q1-N-04 | SN; plantões D e D+2 | Nenhuma | 1. Receber atestado de quatro dias civis, D até D+3, cobrindo ambos os plantões inteiros; 2. Aprovar | Cada plantão: **P660 T0 F0 A660 U0 N0 HE0**. Total **P1320 A1320**. Folgas sem abono adicional. **lei: AN pago‡** | `app/docs/LEIS.md:117`; `app/docs/LEIS.md:121`; `app/docs/CORTES.md:277` | **R-A** nos plantões; **R-L** nas folgas; ‡ |
| Q1-N-05 | SN; folga D+1 | Turno anterior: D 19:00; +1 01:00, 02:00, 07:00 | 1. Registrar atestado para a folga D+1 | Folga D+1: **P0 T0 F0 A0 U0 N0 HE0**. Batidas da madrugada continuam no plantão D. **lei:** eventual cobertura do trecho civil do plantão anterior exige decisão explícita; não presumir | `app/docs/LEIS.md:117`; `app/docs/LEIS.md:121`; `app/docs/CORTES.md:330`; `[nao verificado]` para alcance do documento | **R-L** em D+1; preservar o turno D |
| Q1-N-06 | SN; plantão D | Nenhuma | 1. Aprovar falta integral; 2. Depois receber atestado cobrindo D 19:00–07:00+1 | Antes: **P660 T0 F660 A0**. **lei:** substituição automática não definida. Após regularização e aceitação integral: **F0 A660 N efetivo0**; **lei: AN pago‡** | `app/docs/CORTES.md:338`; `app/docs/LEIS.md:120`; `app/docs/LEIS.md:117`; `[nao verificado]` para precedência | **R-X** → **R-A**, quando decidida a colisão; ‡ |
| Q1-N-07 | SN; plantão D | D 19:00; +1 01:00, 02:00, 07:00 | 1. Registrar atestado parcial 00:30–02:30+1 sobre trabalho realizado | **P660 T660 F0 A0 U0 N(360+δ) HE0** temporalmente. As batidas 01:00/02:00 estão no período declarado: conflito, sem duplicar abono | `app/docs/CORTES.md:277`; `app/docs/CORTES.md:315`; `app/docs/LEIS.md:84`; `app/docs/LEIS.md:83` | **R-C**; † |
| Q1-N-08 | SN; julgamento após 07:00+1 | Nenhuma | Nenhuma decisão ou justificativa | **P660 T0 F0 A0 U660 N0 HE0**. Antes do fim previsto, não declarar falta pelo simples encerramento do dia civil D | `app/docs/LEIS.md:100`; `app/docs/LEIS.md:117`; `app/docs/LEIS.md:120` | **R-U**; uma pendência do plantão D |

**Personalizado sem intervalo — exemplo solicitado de 6h corridas, 08:00–14:00**

Cadastro **SP**: P360 e intervalo cadastrado zero. **lei:** esse desenho exige regularização ou fundamento específico: a CLT determina 15 minutos de intervalo quando o trabalho ultrapassa quatro horas até seis. As contas abaixo preservam o cadastro recebido para expor o caso, sem atestar sua conformidade jurídica. Não se deve inventar intervalo gozado nem apagar trabalho efetivo. (`app/docs/LEIS.md:122`; [CLT, art. 71, §§1º–4º](https://www2.camara.leg.br/legin/fed/declei/1940-1949/decreto-lei-5452-1-maio-1943-415500-normaatualizada-pe.html); fundamento de exceção: `[nao verificado]`)

| id | cadastro | batidas | lançamentos — na ordem | resposta da regra | regra citada | leitores |
|---|---|---|---|---|---|---|
| Q1-P-01 | SP; D previsto | Nenhuma | 1. Admin decide falta integral | Com o cadastro recebido: **P360 T0 F360 A0 U0 N0 HE0**; não usar 440 ou 660 por padrão | L-024/L-088 — `app/docs/LEIS.md:75`; `app/docs/LEIS.md:120`; ressalva SP acima | **R-F**; sinalização de cadastro separada |
| Q1-P-02 | SP; D previsto | 08:00, 12:00 | 1. Decidir saída antecipada 12:00–14:00, sem abono | **P360 T240 F120 A0 U0 N0 HE0** | `app/docs/CORTES.md:202`; `app/docs/LEIS.md:111`; ressalva SP | **R-P** |
| Q1-P-03 | SP; D previsto | 10:00, 14:00 | 1. Receber atestado 08:00–10:00; 2. Aprovar período | **P360 T240 F0 A120 U0 N0 HE0** | `app/docs/CORTES.md:312`; `app/docs/CORTES.md:315`; ressalva SP | **R-A**; não cobrar intervalo inexistente no cadastro como batida faltante |
| Q1-P-04 | SP; D e D+1 previstos | Nenhuma | 1. Receber atestado de dois dias; 2. Aprovar | Cada dia: **P360 T0 F0 A360 U0 N0 HE0**. Total **P720 A720** | `app/docs/LEIS.md:75`; `app/docs/LEIS.md:121`; `app/docs/CORTES.md:277`; ressalva SP | **R-A** |
| Q1-P-05 | SP; data declarada de folga | Nenhuma | 1. Registrar atestado apenas para essa folga | **P0 T0 F0 A0 U0 N0 HE0** | L-089 — `app/docs/LEIS.md:121` | **R-L** |
| Q1-P-06 | SP; D previsto | Nenhuma | 1. Aprovar falta integral; 2. Depois receber atestado integral | Antes: **P360 T0 F360 A0**. **lei:** substituição automática não definida. Após regularização e aceitação: **F0 A360** | `app/docs/CORTES.md:338`; `app/docs/LEIS.md:120`; `[nao verificado]` para precedência; ressalva SP | **R-X** → **R-A**, quando decidida a colisão |
| Q1-P-07 | SP; D previsto | 08:00, 14:00 | 1. Registrar atestado integral sobre jornada realizada | **P360 T360 F0 A0 U0 N0 HE0** temporalmente; conflito com o documento, sem abono duplicado. Sem exceção normativa, **15 min de intervalo suprimido são matéria de indenização separada, não HE** | `app/docs/CORTES.md:277`; `app/docs/CORTES.md:315`; `app/docs/LEIS.md:79`; [CLT, art. 71](https://www2.camara.leg.br/legin/fed/declei/1940-1949/decreto-lei-5452-1-maio-1943-415500-normaatualizada-pe.html); ressalva SP | **R-C**; todos mostram também a questão do intervalo |
| Q1-P-08 | SP; D encerrado | Nenhuma | Nenhuma decisão ou justificativa | **P360 T0 F0 A0 U360 N0 HE0**; cadastro problemático não autoriza inventar falta | `app/docs/LEIS.md:87`; `app/docs/LEIS.md:120`; `app/docs/LEIS.md:122`; ressalva SP | **R-U**; questão do cadastro destinada ao admin |

**Propriedades fixas para todos os casos**

1. **Partes somam o previsto:** nestes cenários, sem trabalho externo autorizado, **P=T+F+A+U**. Trabalho e abono não ocupam duas vezes o mesmo minuto; intervalo fica fora da partição. N é uma classificação sobre minutos, não uma parcela a somar novamente. Indenização de intervalo também fica separada. (`app/docs/CORTES.md:240`; `app/docs/CORTES.md:277`; `app/docs/LEIS.md:79`; `app/docs/LEIS.md:80`)
2. **Idempotência e determinismo:** mesmas entradas e decisões produzem o mesmo resultado; julgar duas vezes não duplica abono, desconto, pergunta ou efeito financeiro. (`app/docs/CORTES.md:333`; `app/docs/LEIS.md:148`; `app/docs/GLOSSARIO.md:27`)
3. **Previsto independe do realizado:** falta, atestado ou ausência de batidas não transformam P440/P660/P360 em zero. Folga já prevista tem P0. (`app/docs/LEIS.md:75`; `app/docs/CORTES.md:333`)
4. **Todo leitor lê o mesmo número e a mesma decisão:** espelho, app, célula, DiaPago e chamado não mantêm cálculos concorrentes; DiaPago é projeção reconstruível. (`app/docs/LEIS.md:60`; `app/docs/LEIS.md:95`; `app/docs/LEIS.md:113`; `app/docs/GLOSSARIO.md:22`)
5. **Sem período, sem inferência:** atestado parcial sem início/fim declarado vai para **CADASTRO — parcial sem período**; não recebe abono arbitrário nem acusação automática de batida dentro da ausência. (`app/docs/CORTES.md:319`)
6. **Trilha e totais:** retificação preserva quem, quando, motivo e antes/depois; totais do período somam minutos das linhas, sem ajuste no rodapé. (`app/docs/LEIS.md:65`; `app/docs/CORTES.md:194`; `app/docs/CORTES.md:210`)

## 05/11 15:29 · sessao 2f176748e217 · REVISAO-JUIZ-1-V5 · verificador · origem=Code

**Há achados nos escritores e na leitura temporal.** Os contraexemplos abaixo são cadastros construídos a partir do código; ocorrência, quantidade de afetados e valores em produção: **[nao verificado]**. Execução da suíte Django e dos mutantes completos: **[nao verificado]**. [nao verificado]

**1. Quem escreve e quem lê os campos e valores alterados**

| Campo ou valor | Quem escreve ou produz | Quem lê |
|---|---|---|
| `EscalaColaborador.trabalha_em_feriado` | Nasce pelo padrão do vínculo; a porta alterada grava a flag. O comando de sincronização escolhe o valor pelo cargo atual. (`app/colaboradores/services/vinculo.py:287`; `app/colaboradores/services/vinculo.py:843`; `app/colaboradores/management/commands/sincronizar_feriado_admin.py:47`) | Gerador, precedência e classificação financeira do feriado. (`app/escala/models.py:1390`; `app/ponto/precedencia.py:376`; `app/ponto/services/fechamento.py:940`; `app/ponto/calculador/alimentacao.py:521`) |
| `Posto.municipio` | `criar_posto`, `atualizar_posto` e `definir_municipio_posto`; os dois últimos disparam o evento novo. (`app/colaboradores/services/estrutura.py:42`; `app/colaboradores/services/estrutura.py:64`; `app/colaboradores/services/estrutura.py:86`) | `lugar_do_vinculo` lê a FK atual; alcance e nome do feriado saem desse município. (`app/core/feriados.py:121`; `app/core/feriados.py:57`; `app/core/feriados.py:98`) |
| `EscalaColaborador.posto` | A criação usa o posto do colaborador; o reaproveitamento de vínculo também pode substituir esse posto. (`app/colaboradores/services/vinculo.py:266`; `app/colaboradores/services/vinculo.py:280`) | Novos leitores de lugar, presença e veto de cobrança. (`app/core/feriados.py:121`; `app/colaboradores/services/feriado_do_posto.py:105`; `app/ponto/management/commands/detectar_ausencias.py:628`) |
| `Colaborador.posto` e `empresa` | A transferência escreve ambos pela porta do colaborador. O diff acrescenta recusa de alteração de posto em lote. (`app/colaboradores/views.py:1385`; `app/colaboradores/services/colaborador.py:73`; `app/colaboradores/services/colaborador.py:86`) | A empresa atual ainda decide janela, lavra e régua de competência passada; o posto do cadastro permanece como rótulo na API. (`app/colaboradores/services/vinculo.py:851`; `app/ponto/portas/celula.py:199`; `app/core/regua_cct.py:352`; `app/api/views_mensageria.py:1060`) |
| Vínculo escolhido para cada dia | `vinculo_do_dia` escolhe; `vinculos_dos_dias` e as cascas carregam a alimentação. (`app/escala/alimentacao.py:269`; `app/escala/alimentacao.py:285`; `app/escala/alimentacao.py:350`) | Conjunto de feriados, fila por causa, universo de retratação do posto e localização da API. (`app/core/feriados.py:151`; `app/chamados/services/fila_por_causa.py:173`; `app/colaboradores/management/commands/marcar_feriado_do_posto.py:107`; `app/api/views_mensageria.py:976`) |
| Conjunto `feriados` | `feriados_do_colab` compõe o alcance pelo vínculo de cada data. (`app/core/feriados.py:139`) | Precedência, calendário, espelho, fechamento, PDF e comandos de comparação/diagnóstico. (`app/ponto/precedencia.py:331`; `app/colaboradores/services/calendario.py:318`; `app/ponto/services/espelho.py:1029`; `app/ponto/services/fechamento.py:639`; `app/relatorios/pdf_espelho.py:298`; `app/relatorios/management/commands/diff_extrato_parcial.py:58`; `app/ponto/management/commands/diff_espelho_alimentacao.py:93`; `app/ponto/management/commands/desvio_o68b.py:113`) |
| `feriado_so_no_pagamento`, `faixa`, `tipo_do_dia`, `suprimidos` e nome do feriado | A precedência agora usa `not ec.feriado_vira_folga(data)` e produz a classificação. (`app/ponto/precedencia.py:376`; `app/ponto/precedencia.py:382`; `app/ponto/precedencia.py:402`) | Reavaliação de faltas, calendário, status do dia e verificação de folha sem furo. (`app/chamados/services/feriado_reavaliar.py:33`; `app/colaboradores/services/calendario.py:507`; `app/ponto/status_dia.py:298`; `app/folha/services/sem_furo.py:25`) |
| `CelulaDia.trabalha`, `dna`, `escala_geradora`, `dna_anterior`, `regenerada_em`, `regeneracoes` | A regeneração cria ou substitui os valores; o evento de município passa a acioná-la. (`app/ponto/portas/celula.py:243`; `app/ponto/portas/celula.py:260`; `app/colaboradores/services/vinculo.py:903`) | A escolha do vínculo lê a geradora; a precedência lê `trabalha`; a alteração agenda julgamento, dinheiro e reavaliação dos chamados. (`app/escala/alimentacao.py:269`; `app/ponto/precedencia.py:347`; `app/ponto/portas/celula.py:360`; `app/ponto/portas/celula.py:384`; `app/ponto/portas/celula.py:428`) |
| `ChamadoColaborador.status_local`, nota e trilha de reconciliação | O evento chama `encerrar`, que usa `reconciliar`, acrescenta nota e registra `reconciliar_feriado`. (`app/chamados/services/feriado_reavaliar.py:43`; `app/chamados/models.py:607`) | O universo e o vigia leem chamados vivos. Na frota, a nova chamada de reavaliação ocorre sem a rejunção de `celula_id`. (`app/chamados/services/feriado_reavaliar.py:17`; `app/ponto/management/commands/reavaliar_ausencias_feriado.py:35`; `app/ponto/portas/celula.py:433`) |
| Valores de `FechamentoMensal` e `derivado_em` | A porta da flag agenda recálculo; o fechamento grava as rubricas. O evento de município usa `recalculo=False` e invalida a projeção. (`app/colaboradores/services/vinculo.py:857`; `app/ponto/portas/celula.py:397`; `app/ponto/services/fechamento.py:1144`; `app/colaboradores/services/vinculo.py:904`; `app/ponto/services/fechamento.py:1636`) | A autoridade do período do espelho consulta o fechamento gravado. (`app/ponto/services/espelho.py:1024`) |
| Contadores de presença e sugestão por posto | `presenca_em_feriado` passa a atribuir a observação ao vínculo fornecido à juíza. (`app/colaboradores/services/feriado_do_posto.py:102`; `app/colaboradores/services/feriado_do_posto.py:116`) | Pisos de observação, sugestões e pauta da supervisão. (`app/colaboradores/services/feriado_do_posto.py:121`; `app/colaboradores/services/feriado_do_posto.py:136`; `app/colaboradores/services/feriado_do_posto.py:146`) |
| Argumento e papel do cron | A configuração troca `--apply` por `--alarme`, retira o comando dos juízes e declara `vigia`. (`app/config/crons.py:608`; `app/config/crons.py:2015`; `app/config/crons.py:2130`) | O comando conta e produz `SystemExit(2)` quando encontra espúrios com alarme. (`app/ponto/management/commands/reavaliar_ausencias_feriado.py:35`; `app/ponto/management/commands/reavaliar_ausencias_feriado.py:40`) |

**2. Contraexemplos concretos**

**A — Corrigir município troca indevidamente o dono do dia.** Considere **10/12/2026**, com célula gerada por `ecQ`, posto em Maringá, 5x2, flag `False`, vínculo ativo desde janeiro. Existe também `ecP`, iniciado em fevereiro, inativo com término em 20/12, no posto P sem município. Ambos cobrem a data, mas a juíza responde **ecQ**, porque a geradora válida da célula manda. O cadastro admite inícios distintos e intervalo válido. (`app/escala/alimentacao.py:214`; `app/escala/alimentacao.py:269`; `app/escala/models.py:1004`; `app/escala/models.py:1037`)

Ao corrigir P para Londrina em 10/12, o evento seleciona `ecP` apenas porque cobre hoje. A porta aceita substituir a geradora diferente, grava `trabalha=False` e passa a célula para `ecP`. O município corrigido de P acaba reescrevendo o dia que pertencia a Q. (`app/colaboradores/services/vinculo.py:899`; `app/ponto/portas/celula.py:251`; `app/ponto/portas/celula.py:260`)

Pela autoridade declarada, o vínculo do dia continua sendo a geradora válida Q; a correção do município de P não constitui troca desse vínculo. O resultado contraria essa escolha e altera o chão usado para julgar a falta. (`app/docs/CORTES.md:284`; `app/docs/JUIZES.md:98`; `app/ponto/portas/celula.py:439`; `app/chamados/services/feriado_reavaliar.py:33`)

**B — Empresa atual deixa passar dia exportado por outra empresa.** Considere A e B com corte 21: vínculo 5x2 em Londrina, flag `True`, célula de **10/12/2026** como trabalho; A exportou dezembro, sem holerite publicado. Em **05/01/2027**, o cadastro é transferido para B, que não exportou dezembro; o vínculo permanece no posto de A. O fluxo de transferência escreve posto/empresa e histórico, sem encerrar o vínculo. (`app/colaboradores/views.py:1383`; `app/colaboradores/views.py:1392`)

Ao marcar a flag `False`, a janela anterior alcança 10/12. A guarda consulta os TXT de **B**, pela empresa atual do colaborador, e não encontra a exportação de A; a célula pode ser reescrita para folga. O corte específico da obra exige evento somente em competência aberta. (`app/colaboradores/services/vinculo.py:850`; `app/ponto/janelas.py:66`; `app/ponto/portas/celula.py:199`; `app/ponto/management/commands/gerar_celulas.py:161`; `app/ponto/portas/celula.py:238`; `app/docs/CORTES.md:362`)

Esse caminho também agenda recálculo financeiro usando a empresa atual. Ocorrência real e diferença monetária: **[nao verificado]**. (`app/ponto/portas/celula.py:388`; `app/ponto/portas/celula.py:397`)

**C — Célula sem geradora desaparece da presença por posto.** Considere vínculo cobrindo **10/12/2026** em Londrina e célula com `trabalha=True`, lâmpadas preenchidas e `escala_geradora=None`. A juíza deveria usar o vínculo vigente, mas `presenca_em_feriado` fornece lista vazia e descarta a observação. A FK permite nulo. (`app/escala/models.py:1692`; `app/escala/alimentacao.py:285`; `app/colaboradores/services/feriado_do_posto.py:102`; `app/colaboradores/services/feriado_do_posto.py:106`)

Com duas outras observações de feriado, perder essa terceira impede atingir o piso de três e elimina o posto da sugestão. Isso altera a alimentação da autoridade e o universo contado. (`app/colaboradores/services/feriado_do_posto.py:36`; `app/colaboradores/services/feriado_do_posto.py:121`; `app/docs/JUIZES.md:98`)

**3. Regra de negócio alterada sem lastro demonstrado**

O evento de município introduz a regra prática **“todo vínculo deste posto que cobre hoje pode regenerar a célula”**. Ela permite substituir a geradora de outro posto, como no caso A; o lastro citado estabelece que quem decide o vínculo do dia é a juíza da geradora, não esse conjunto de candidatos. (`app/colaboradores/services/vinculo.py:899`; `app/ponto/portas/celula.py:251`; `app/docs/CORTES.md:284`)

Há também mudança explícita da exceção do feriado: a precedência passa de consultar a flag para consultar `feriado_vira_folga`, deixando o 12x36 com flag `False` como trabalho. O código atribui isso ao aval MANHA-0511; confirmação desse aval nos LEIS/CORTES anexados: **[nao verificado]**. A L-086 trata pagamento, e seu próprio registro distingue as regras de dobra por ciclo. (`app/ponto/precedencia.py:371`; `app/ponto/tests/test_a2_precedencia.py:305`; `app/docs/LEIS.md:118`)

**4. Selo que não morde**

O teste alterado **`test_com_lugar_nao_acusa`** só entrega ao analisador uma string contendo `eh_feriado(d, lugar_do_vinculo(ec))`. Não executa a função de lugar nem verifica a origem do município. O analisador aceita qualquer segundo argumento. Portanto, esse teste permanece verde com a cura de localização revertida. (`app/core/tests/test_contract_feriado_com_praca.py:65`; `app/core/contrato_feriado.py:75`)

Na execução isolada, em memória, o analisador retornou `[]` para a string nova, para `lugar_do_colab(colab)` e para `None`; esse comportamento corresponde à condição que verifica somente a quantidade de argumentos. (`app/core/contrato_feriado.py:73`)

O teste **`test_RED_transferencia_as_20h_nao_reescreve_hoje`** também deixa passar uma reversão parcial: ele verifica célula e chamado após transferir o cadastro, mas não consulta a precedência novamente. Restaurar apenas o lugar antigo na precedência preserva essas asserções, pois `gravar` apenas salva o cadastro. Execução desse mutante completo: **[nao verificado]**. (`app/chamados/tests/test_jv1_volta4_posto_do_dia.py:119`; `app/chamados/tests/test_jv1_volta4_posto_do_dia.py:121`; `app/colaboradores/services/colaborador.py:55`)

**5. Cadastro atual decidindo dia ou competência passada**

| Cadastro atual | Escritor localizado | Decisão sobre passado |
|---|---|---|
| `situacao` e `data_demissao` | Desligamento grava ambos; reativação volta para ativo e apaga a demissão. (`app/colaboradores/services/desligamento.py:104`; `app/colaboradores/services/desligamento.py:210`) | O universo de qualquer competência consulta esses valores atuais. (`app/ponto/services/fechamento.py:422`; `app/ponto/services/fechamento.py:430`) |
| `posto` | Transferência altera o posto do colaborador; o reaproveitamento pode alterar o posto do próprio vínculo. (`app/colaboradores/views.py:1385`; `app/colaboradores/services/vinculo.py:266`) | O feriado migrou para o vínculo do dia, mas lê o posto e o município atuais desse objeto. (`app/core/feriados.py:121`; `app/core/feriados.py:151`) |
| `empresa` | Transferência acompanha a empresa do novo posto. (`app/colaboradores/views.py:1386`) | Define universo, janela histórica, guarda de lavra e empresa da régua. É o mecanismo do caso B. (`app/ponto/services/fechamento.py:435`; `app/ponto/services/fechamento.py:612`; `app/ponto/portas/celula.py:199`; `app/core/regua_cct.py:352`) |
| `regime_trabalhista` | Formulário de empresa → porta da estrutura. (`app/colaboradores/views.py:512`; `app/colaboradores/services/estrutura.py:17`) | A régua recebe uma data histórica, mas escolhe CLT/CCT pelo regime atual da empresa atual. (`app/core/regua_cct.py:329`; `app/core/regua_cct.py:352`; `app/core/regua_cct.py:366`) |
| `isento_ponto` | Passe livre → porta do colaborador. (`app/colaboradores/views.py:173`; `app/colaboradores/services/colaborador.py:55`) | A flag atual elimina faltas afirmáveis da competência e inconsistências dos dias do espelho. (`app/core/constants.py:18`; `app/ponto/services/fechamento.py:357`; `app/ponto/services/espelho.py:600`; `app/ponto/services/espelho.py:669`) |
| Vínculo atual | As portas criam, reaproveitam e encerram vínculos. (`app/colaboradores/services/vinculo.py:266`; `app/colaboradores/services/vinculo.py:280`; `app/colaboradores/services/vinculo.py:780`) | O comando escolhe vínculos ativos e o cargo atual para mudar a flag; a porta nova regenera janela anterior e atual. A seleção de feriado por dia usa a geradora/vigência, mas os demais atributos continuam vivos. (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:42`; `app/colaboradores/management/commands/sincronizar_feriado_admin.py:47`; `app/colaboradores/services/vinculo.py:850`; `app/escala/alimentacao.py:269`) |

O município atual já produz uma divergência expressa no teste novo: em **05/12**, corrigir o posto para Londrina mantém a célula de **02/12** como trabalho, mas faz o vigia contar sua falta como espúria. O leitor financeiro também recebe feriados calculados pelo município atual. (`app/chamados/tests/test_jv1_volta3_municipio_e_quem_chegou.py:59`; `app/chamados/tests/test_jv1_volta3_municipio_e_quem_chegou.py:68`; `app/chamados/tests/test_jv1_volta3_municipio_e_quem_chegou.py:74`; `app/ponto/services/fechamento.py:639`)

As leituras temporais estão localizadas; sua ocorrência em produção e o desacordo de regra isolado para retroatividade de situação, regime ou isenção permanecem **[nao verificado]**.

**VEREDITO: ACHADO**

1. `app/colaboradores/services/vinculo.py:899` — **itens 1, 2 e 3**: evento de município substitui a geradora de outro vínculo; **toca dinheiro: sim**, com alteração do previsto e invalidação do fechamento. (`app/ponto/portas/celula.py:260`; `app/colaboradores/services/vinculo.py:905`)
2. `app/ponto/portas/celula.py:199` — **itens 1, 2 e 5**: empresa atual pode ocultar exportação da empresa anterior; **toca dinheiro: sim**. (`app/ponto/management/commands/gerar_celulas.py:161`; `app/ponto/portas/celula.py:397`)
3. `app/core/feriados.py:121` — **item 5**: município atual classifica dias passados e alimenta o fechamento; **toca dinheiro: sim**. (`app/core/feriados.py:151`; `app/ponto/services/fechamento.py:639`)
4. `app/colaboradores/services/feriado_do_posto.py:103` — **itens 1 e 2**: célula sem geradora perde observação apesar de vínculo vigente; **toca dinheiro: nao**, o resultado é sugestão para decisão humana. (`app/escala/alimentacao.py:285`; `app/colaboradores/services/feriado_do_posto.py:136`)
5. `app/core/tests/test_contract_feriado_com_praca.py:65` — **item 4**: teste sintático permanece verde com a cura revertida; **toca dinheiro: nao**, o teste apenas analisa fonte. (`app/core/contrato_feriado.py:75`)

## 05/11 15:28 · sessao 807cf5343cb9 · REVISAO-CRON-MOTOR · verificador · origem=Code

**ACHADO:** o agrupamento pode mudar a decisão do dia; há também cadastro atual decidindo períodos passados nos leitores alcançados pelo diff. (`app/colaboradores/services/calendario.py:132`; `app/core/feriados.py:113`; `app/core/regua_cct.py:352`)

Os contraexemplos abaixo são sintéticos, conferidos com funções da cópia executadas em memória. Ocorrência em produção, integração completa do cron e resultado da suíte: **[nao verificado]**. [nao verificado]

**1. Quem escreve e quem lê os valores alterados**

| Campo/valor | Escritor/produtor | Leitor |
|---|---|---|
| `status_dia`, `status_rotulo` | `lavrar_veredito`, por `_aplicar_status`: `app/ponto/portas/celula.py:705`, `app/ponto/portas/celula.py:709` | D4 lê a coluna ou decide disponibilidade; calendário recebe o resultado: `app/ponto/status_dia.py:287`, `app/ponto/status_dia.py:292`, `app/colaboradores/services/calendario.py:663` |
| `pode_ultimo_recurso` | Calendário combina janela atual da empresa e pergunta viva: `app/colaboradores/services/calendario.py:463`, `app/colaboradores/services/calendario.py:476`, `app/colaboradores/services/calendario.py:738` | Predicado `resolver_dia_oferecido`: `app/colaboradores/services/calendario.py:103` |
| `resolver_dia_oferecido` | Predicado escreve o campo de cada dia: `app/colaboradores/services/calendario.py:769` | Template e porta plural: `app/templates/colaboradores/partials/_calendario_grade.html:125`, `app/colaboradores/services/calendario.py:135` |
| Mapas `datas`, `inst`, `oferecido` | Agrupamento por colaborador e consulta plural: `app/chamados/services/vigia_fio.py:265`, `app/chamados/services/vigia_fio.py:269` | Dois censos e classificação do dono: `app/chamados/services/vigia_fio.py:428`, `app/chamados/services/vigia_fio.py:635`, `app/chamados/services/vigia_fio.py:305` |
| `portas`, `balde`, `papel`, contadores e listas | Porta antecipada, classificação e acumulação: `app/chamados/services/vigia_fio.py:634`, `app/chamados/services/vigia_fio.py:303`, `app/chamados/services/vigia_fio.py:431`, `app/chamados/services/vigia_fio.py:656` | Command imprime e carimba a rodada: `app/chamados/management/commands/vigiar_fio_morto.py:167`, `app/chamados/management/commands/vigiar_fio_morto.py:206` |
| Cadastro de feriados e conjunto por lugar | Tela grava data/abrangência/UF/município; `feriados_de` produz o conjunto: `app/core/views_config.py:63`, `app/core/views_config.py:81`, `app/core/feriados.py:83` | Espelho, fechamento e PDF: `app/ponto/services/espelho.py:1034`, `app/ponto/services/fechamento.py:634`, `app/relatorios/pdf_espelho.py:297` |
| `feriados_motor`, recorte visual e bordas ampliadas | Espelho produz conjunto e recorte; fechamento amplia envelope; PDF usa bordas das batidas: `app/ponto/services/espelho.py:1034`, `app/ponto/services/espelho.py:1035`, `app/ponto/services/fechamento.py:586`, `app/relatorios/pdf_espelho.py:297` | Injeção nos motores e desenho da tela: `app/ponto/services/espelho.py:1118`, `app/ponto/services/espelho.py:1271`, `app/ponto/services/fechamento.py:725`, `app/relatorios/pdf_espelho.py:395` |
| `self.feriados`, `_feriados_injetados` | Motor cria conjunto e guarda identidade; intermitente substitui conjunto; contexto v3 mantém marcador `None`: `app/ponto/motor_calculo_v2.py:790`, `app/ponto/motor_calculo_v2.py:795`, `app/ponto/motor_calculo_v2.py:3557`, `app/ponto/calculador/regras.py:295` | `eh_feriado`; transferência para CCT da v3: `app/ponto/motor_calculo_v2.py:963`, `app/ponto/motor_calculo_v2.py:965`, `app/ponto/calculador/nucleo.py:350` |

O resultado de `eh_feriado` alcança dobra, HE sem previsto, classificação dos períodos e soma semanal. Os resultados monetários chegam a `FechamentoMensal` e `DiaPago`. (`app/ponto/motor_calculo_v2.py:934`, `app/ponto/motor_calculo_v2.py:1449`, `app/ponto/motor_calculo_v2.py:2796`, `app/ponto/motor_calculo_v2.py:3082`, `app/ponto/motor_calculo_v2.py:3157`, `app/ponto/motor_calculo_v2.py:3251`, `app/ponto/motor_calculo_v2.py:3330`; `app/ponto/services/fechamento.py:1140`; `app/ponto/services/fechamento.py:1181`)

**2. Contraexemplos concretos**

**Agrupar altera a decisão.** Cadastro sintético: colaborador sem vínculo, sem marcos/ata/status lavrado; célula de **04/09/2026** com `trabalha=False`; sem pergunta viva respondida; consulta em **05/11/2026**, empresa com corte 21. Existência desse cadastro em produção: **[nao verificado]**.

Batidas do caso, em horário local:

| Tipo | `timestamp` | `criado_em` |
|---|---|---|
| E | 01/09/2026 08:00 | 04/09/2026 08:00:01 |
| E | 03/09/2026 20:00 | 04/09/2026 08:00:00 |
| E | 04/09/2026 08:00 | 04/09/2026 08:00:02 |
| S | 04/09/2026 17:00 | 04/09/2026 17:00:00 |

Esses campos são justamente os insumos da identificação de eco; ela ordena por `criado_em` e compara cada entrada com a anterior dessa ordem. (`app/ponto/turnos.py:353`, `app/ponto/turnos.py:364`, `app/ponto/turnos.py:372`)

- **Consulta individual de 04/09:** o pad começa em 02/09; a entrada de 01/09 fica fora. A entrada de 04/09 vira eco da entrada de 03/09, deixando **nenhum turno próprio em 04/09**. O D4 retorna `folga`; Resolver dia resulta **False**. (`app/ponto/turnos.py:1845`; `app/ponto/turnos.py:368`; `app/ponto/turnos.py:1396`; `app/ponto/status_dia.py:312`; `app/colaboradores/services/calendario.py:103`)
- **Consulta conjunta de 01/09 e 04/09:** a entrada antiga entra na ordenação de chegada e interrompe aquela comparação. A entrada de 04/09 deixa de ser eco e forma **08:00–17:00**. O D4 retorna `sem_certidao`; Resolver dia resulta **True**. (`app/colaboradores/services/calendario.py:132`; `app/ponto/turnos.py:372`; `app/ponto/turnos.py:1417`; `app/ponto/status_dia.py:292`; `app/colaboradores/services/calendario.py:103`)

O corte anexado vincula o vigia ao mesmo campo oferecido pela tela. Aqui, a tela customizada de um dia e a consulta agrupada podem discordar para o mesmo cadastro e dia; o booleano diferente muda o balde para `decisao_do_dia_pendente`. (`app/docs/CORTES.md:82`; `app/templates/colaboradores/partials/_calendario_grade.html:125`; `app/chamados/services/vigia_fio.py:305`)

**Isenção atual apaga eventos de competência passada.** Outro cadastro sintético: fechamento **09/2026** com **2 horas de HE50**, pessoa não isenta naquela competência e incluída no Passe livre em **05/11/2026**. A função real devolveu o evento de 2 horas com a flag falsa e `[]` após mudar somente a flag atual, preservando o mesmo fechamento. (`app/colaboradores/views.py:176`; `app/folha/export.py:407`; `app/folha/export.py:430`)

Isso permite suprimir dinheiro já apurado pela condição atual do colaborador, enquanto a L-095 manda os leitores consumirem a lavratura do dia. É comportamento preexistente, alcançado pela revisão dos leitores. (`app/docs/LEIS.md:113`; `app/core/constants.py:18`; `app/folha/export.py:408`)

**3. Regra de negócio alterada e lastro**

O agrupamento introduz uma dependência nova: **outros dias pedidos na rodada podem decidir se este dia tem decisão pendente**. O contraexemplo acima contraria a equivalência com a leitura individual que o próprio teste declara preservar. (`app/colaboradores/services/calendario.py:130`; `app/ponto/turnos.py:353`; `app/chamados/tests/test_sombra_c1_resolver_em_lote.py:68`; `app/docs/CORTES.md:82`)

No motor, também mudou o contrato: ausência no conjunto injetado passou a ser resposta negativa definitiva, inclusive para conjunto vazio. O teste exige expressamente ignorar o feriado nacional cadastrado nesse caso. (`app/ponto/motor_calculo_v2.py:965`; `app/ponto/tests/test_sombra_c1_motor_feriado_injetado.py:47`)

O aval F3 item 3 é citado nos comentários; seu teor como autorização independente nos anexos fornecidos: **[nao verificado]**. (`app/ponto/tests/test_sombra_c1_motor_feriado_injetado.py:3`)

**4. Selo que passa com a cura revertida**

O teste de paridade passa se a porta plural voltar a montar **um calendário por dia**, mantendo sua assinatura: ele compara respostas, sem contar calendários. Já os dois testes de desempenho detectam essa reversão pela contagem. (`app/chamados/tests/test_sombra_c1_resolver_em_lote.py:71`; `app/chamados/tests/test_sombra_c1_resolver_em_lote.py:41`; `app/chamados/tests/test_sombra_c1_resolver_em_lote.py:64`)

No motor, `test_none_segue_perguntando_ao_juiz` e `test_intermitente_fica_como_hoje` também passam com o comportamento anterior: são controles de preservação. Os testes de zero consulta e conjunto vazio distinguem a mudança. Resultado de execução da suíte: **[nao verificado]**. (`app/ponto/tests/test_sombra_c1_motor_feriado_injetado.py:34`; `app/ponto/tests/test_sombra_c1_motor_feriado_injetado.py:40`; `app/ponto/tests/test_sombra_c1_motor_feriado_injetado.py:47`; `app/ponto/tests/test_sombra_c1_motor_feriado_injetado.py:51`)

**5. Cadastro de hoje decidindo passado**

| Cadastro atual | Quem escreve | Decisão histórica alcançada |
|---|---|---|
| `situacao`, `data_demissao` | Desligamento grava ambos; reativação apaga a demissão: `app/colaboradores/services/desligamento.py:104`, `app/colaboradores/services/desligamento.py:210` | Universo da competência passada lê esses valores atuais: `app/ponto/services/fechamento.py:423`, `app/ponto/services/fechamento.py:430` |
| `posto` | Transferência substitui o posto: `app/colaboradores/views.py:1385` | Município atual decide feriado de qualquer período solicitado: `app/core/feriados.py:113`; três chamadores alterados: `app/ponto/services/fechamento.py:634`, `app/ponto/services/espelho.py:1034`, `app/relatorios/pdf_espelho.py:297` |
| `empresa` | Transferência substitui a empresa: `app/colaboradores/views.py:1386` | Filtro, janela da competência antiga e habilitação de último recurso: `app/ponto/services/fechamento.py:435`, `app/ponto/services/fechamento.py:612`, `app/colaboradores/services/calendario.py:463` |
| `regime_trabalhista` | Formulário e porta de estrutura: `app/colaboradores/views.py:515`, `app/colaboradores/services/estrutura.py:17` | Régua recebe data histórica, mas lê empresa/regime atuais: `app/core/regua_cct.py:352`, `app/core/regua_cct.py:453` |
| `isento_ponto` | Passe livre: `app/colaboradores/views.py:176` | Suprime furos, inconsistências e eventos do fechamento passado: `app/ponto/services/fechamento.py:357`, `app/ponto/services/espelho.py:676`, `app/folha/export.py:407` |
| Vínculo atual | Criação/edição do vínculo: `app/colaboradores/services/vinculo.py:274`, `app/colaboradores/services/vinculo.py:278` | Os caminhos principais consultam vínculo histórico: `app/ponto/services/espelho.py:1070`, `app/relatorios/pdf_espelho.py:307`, `app/ponto/services/fechamento.py:838`. A consulta inicial ao ativo no calendário possui complemento pela geradora: `app/colaboradores/services/calendario.py:258`, `app/ponto/precedencia.py:327` |

Exemplo temporal adicional: desligado em **10/09/2026**, reativado em **01/11/2026**. Antes da reativação fica fora da competência **10/2026**, corte 21; depois, com `situacao='ativo'` e demissão apagada, entra nessa mesma competência passada. A seleção deixa de representar o desligamento fora daquela janela. (`app/colaboradores/services/desligamento.py:210`; `app/ponto/services/fechamento.py:430`; `app/ponto/janelas.py:10`; `app/docs/CORTES.md:81`)

VEREDITO: **ACHADO**

1. `app/colaboradores/services/calendario.py:132` — itens **2–3**: agrupamento muda a decisão do dia; toca dinheiro **não**.
2. `app/folha/export.py:407` — itens **2 e 5**: isenção atual suprime eventos de competência passada; preexistente; toca dinheiro **sim**.
3. `app/core/feriados.py:113` — item **5**: posto atual decide feriado passado; preexistente; toca dinheiro **sim**.
4. `app/core/regua_cct.py:352`, `app/ponto/services/fechamento.py:612` — item **5**: empresa/regime atuais decidem régua e janela passadas; preexistente; toca dinheiro **sim**.
5. `app/colaboradores/services/desligamento.py:210`, `app/ponto/services/fechamento.py:430` — item **5**: reativação altera elegibilidade de competência passada; preexistente; toca dinheiro **sim**.

## 05/11 15:28 · sessao 02a683dc80c3 · - · - · origem=Ronald (tmux)

Extratos normativos para montar os casos, sem leitura da implementação dos módulos proibidos:

| Tema | Regra e referência |
|---|---|
| Previsto | A grade é a fonte única de previsto; não derivar novamente por leitor. `app/docs/LEIS.md:75` — L-024. |
| Geometria | A definição/desenho da escala decide o regime; o rótulo não manda no cálculo. `app/docs/LEIS.md:124` — L-091. |
| Intervalo cadastrado | Sempre sai da jornada; intervalo deslocado dentro do envelope continua sendo intervalo, sem HE. `app/docs/LEIS.md:80` — L-032. |
| Intervalo ausente | `intervalo_indenizavel=True`: indenizar só o suprimido; `False`: pré-assinalado, sem HE e sem indenização, permanecendo pergunta pela marcação faltante. `app/docs/LEIS.md:79` — texto de L-031. |
| Intervalo ausente não vira HE | Corte confirma indenizar só o suprimido, HE zero. `app/docs/CORTES.md:411`. |
| Pagabilidade | Entrada e saída presentes, faltando apenas intervalo: dia pagável; pergunta continua sem bloquear folha. `app/docs/LEIS.md:81` — L-033. |
| Resposta sobre pausa | Resposta validada pelo admin vira registro do intervalo e retira indenização; sem resposta ou recusada permanece a política de indenização; deduplicação pela chave do fato. `app/docs/CORTES.md:273`. |
| Falta decidida | Falta exige ausência lançada de tipo que desconta. `app/docs/LEIS.md:87` — L-039. Furo é pergunta em aberto, não decisão de falta. `app/docs/LEIS.md:120` — L-088. |
| Abono sem sobreposição | Abono cobre só o previsto não coberto pelas batidas. `app/docs/CORTES.md:277` — B-B21. |
| Justificativa parcial | Cobre somente tipo e período; trabalhou: pagar realizado mais abonado do período. `app/docs/CORTES.md:312` e `:315`. Sem período explícito, parcial vai para CADASTRO, não se infere cobertura. `app/docs/CORTES.md:319`. |
| Parcial noturna | Hora final anterior à inicial significa cruzamento da meia-noite, pertencente ao dia inicial do turno. `app/docs/CORTES.md:330`. |
| Folga | Ausência que abona/desconta só alcança dia previsto de trabalho; folga não gera efeito nem consome saldo. `app/docs/LEIS.md:121` — L-089. |
| HE de pontas | Minutos antes/depois dos marcos ficam riscados e pendentes; só passam a contar após autorização humana com trilha. “Não” mantém exclusão e registra ciência. Exportação bloqueada enquanto houver HE pendente. `app/docs/LEIS.md:115` — L-097. |
| Nova ponta após “Não” | Negativa cobre os minutos da trilha, não todo o dia; minutos novos podem gerar nova pendência. `app/docs/CORTES.md:274`. |
| Pontualidade | Atraso mais saída antecipada não superam previsto menos trabalhado; previsto zero implica teto zero. `app/docs/LEIS.md:111` — L-093. |
| Turno em outro horário | Somente quando ambas as pontas distam mais de três horas dos marcos: não cobrar pontualidade, contar trabalho, indicar CADASTRO × REALIDADE. `app/docs/LEIS.md:116` — L-084. |
| Meia-noite | Toda jornada pertence ao dia inicial; cauda não cria trabalho em D+1. `app/docs/LEIS.md:117` — L-085. |
| Domingo/feriado previsto | Plantão previsto nunca se torna HE100 apenas por domingo/feriado. `app/docs/LEIS.md:118` — L-086. |
| CCT Londrina | Cl.38-d: no 12x36, hora normal de 60 minutos, mantendo adicional noturno de 20%. Texto literal em `app/core/regua_cct.py:129`–`:139`; proibição de embutir adicional no piso em `:146`–`:149`; L-036 em `app/docs/LEIS.md:84`. |
| Prorrogação após 05h | CLT + 12x36: não; CLT comum: sim; CCT: depende do cadastro da convenção. Não presumir que cl.38-d sozinha decide prorrogação. `app/docs/LEIS.md:83` — L-035; texto normativo em `app/core/regua_cct.py:156`–`:168`. |
| Janela do intervalo noturno | Intervalo usa a mesma janela de prorrogação do turno; parte ≤ todo. `app/docs/CORTES.md:418`. |
| Furo com número ainda indefinido | Caso de batida ímpar: “Em aberto”, sem número. `app/docs/CORTES.md:278`. |
| Estado antes de encerramento | “Previsto”/“aberto”; depois, ler status lavrado. `app/docs/CORTES.md:415`. |
| Leitores sincronizados | Consumidor lê célula/ata. `app/docs/LEIS.md:95` — L-061. Valor do dia lavrado uma vez, todos leem sem recalcular. `:113` — L-095. Coerência inclusive com cadastro ruim: `:122` — L-099. |
| Invariantes | Partes somam total, idempotência, todo leitor dá mesmo número: `app/docs/LEIS.md:148` — L-114. Previsto independente do realizado, julgamento repetido igual e jornada no dia inicial: `app/docs/CORTES.md:333`. |

**Atenção ao personalizado de 6h corridas:** CLT art.71 §1 exige 15 minutos de intervalo quando o trabalho ultrapassa quatro e não excede seis horas. Cadastro sem pausa não prova dispensa legal. Para preservar o exemplo de 6h, marcar **“lei: cadastro sem intervalo confronta piso CLT; falta norma autorizadora”**; não inventar intervalo registrado nem desconto automático. Outra opção é personalizado de quatro horas sem intervalo, pois “6h” foi dado como exemplo. [CLT, fonte primária](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm). Essa exigência não foi localizada nos trechos normativos locais examinados. [não verificado]

**Números úteis, condicionados ao cadastro descrito:** 07:00–15:20 menos pausa 11:00–12:00 = 440 min; 07:00–19:00 menos 12:00–13:00 = 660 min; 19:00–07:00 menos 00:00–01:00 = 660 min, dos quais 360 noturnos em janela estrita 22:00–05:00, ou 480 se a CCT cadastrada estender até 07:00. Hora de 60 minutos e adicional de 20%, sem converter 360 em 411,43 nem 480 em 548,57. São cálculos de cenários propostos, sustentados pela dedução do intervalo e regras de janela acima; não são medições do sistema. `app/docs/LEIS.md:80`, `:83`, `:84`; `app/core/regua_cct.py:132`–`:139`.

## 05/11 15:28 · sessao d536100f04c1 · - · - · origem=Ronald (tmux)

Leitura restrita aos três documentos; não consultei implementação.

| Tema | Regra normativa | Referência |
|---|---|---|
| Mesma resposta nos leitores | Tela, PDF, badge e contador leem a mesma autoridade; chamado, pergunta, tela e folha leem célula/ata. | `app/docs/LEIS.md:60` L-002; `app/docs/LEIS.md:95` L-061 |
| Valores do dia | Normais, HE, noturno, intra, pontualidade e falta são lavrados e lidos; leitores não recalculam. | `app/docs/LEIS.md:113` L-095 |
| Coerência com qualquer dado | Com o cadastro disponível, sistema deve ser coerente, determinístico e idempotente; erro ou acerto deve aparecer igual em todo lugar. | `app/docs/LEIS.md:122` L-099 |
| Previsto | Grade é fonte única; não derivar previsto em paralelo. | `app/docs/LEIS.md:75` L-024 |
| Falta versus pergunta | Falta é decidida por ausência cujo tipo desconta; furo é marco não cumprido e pergunta em aberto, não decisão. | `app/docs/LEIS.md:87` L-039; `app/docs/LEIS.md:120` L-088 |
| Intervalo faltante | Indenizável=True indeniza só suprimido; False é pré-assinalado, sem HE/indenização, com pergunta. Intervalo cadastrado sempre sai da jornada. | `app/docs/LEIS.md:79` L-031; `app/docs/LEIS.md:80` L-032 |
| Intervalo não bloqueia folha | Entrada e saída batidas, faltando só intervalo: dia pagável e pergunta segue aberta. | `app/docs/LEIS.md:81` L-033 |
| Resposta sobre intervalo | Validada pelo admin vira registro do intervalo e sai indenização; sem resposta ou recusada, segue política de indenizar; dedup pela chave do fato. | `app/docs/CORTES.md:273` |
| Resposta sem validação | Resposta do colaborador segue viva, “só Validar”; encerrar disputa não decide o dia. | `app/docs/CORTES.md:342` |
| Fechar disputa | Exige fio vivo vazio **e nenhuma resposta sem veredito**. | `app/docs/CORTES.md:272` |
| Justificativa aprovada | Encerra chamado de origem pela porta do chamado na mesma transação da aprovação. | `app/docs/CORTES.md:362` item 2 |
| Parcial | Justificativa cobre só tipo e período; ausência parcial conflita somente com batida dentro do período declarado. | `app/docs/CORTES.md:315` |
| Parcial sem horário | Início/fim são cadastro preenchido pelo admin; sem período, lista CADASTRO “parcial sem período”, sem inferir ponta. | `app/docs/CORTES.md:319` |
| Parcial noturna | Fim menor que início cruza meia-noite e pertence ao dia de início do turno. | `app/docs/CORTES.md:330` item 2 |
| Abono | Cobre somente previsto não coberto pelas batidas; justificativa aprovada produz efeito parcial e período pago deve vir da mesma fonte. | `app/docs/CORTES.md:277`; `app/docs/CORTES.md:336`; `app/docs/CORTES.md:353` |
| Folga | Ausência que abona/desconta não produz efeito nem consome saldo em folga. | `app/docs/LEIS.md:121` L-089 |
| Batida que resolve pergunta | Batida que chega absolve na passada seguinte; veredito não lê seu próprio chamado vivo. | `app/docs/CORTES.md:413` |
| Propriedades fixas | Partes somam total; idempotência; previsto independente de realizado; todos leitores com mesmo número. | `app/docs/CORTES.md:240`; `app/docs/LEIS.md:148` L-114 |
| Intercalações | Responder, validar, recusar, fechar, reabrir e lote devem preservar determinismo e trilha. | `app/docs/CORTES.md:333` |

**lei:** os documentos não fixam convergência instantânea dos leitores após o envio de uma resposta; há consumidor a cada um minuto e absolvição “na passada seguinte”. A bateria deve distinguir resposta enviada, resposta validada e processamento concluído; o prazo exato para igualdade após cada ato permanece não decidido. `app/docs/CORTES.md:351`; `app/docs/CORTES.md:413`; `[não verificado]`

O glossário classifica espelho e DiaPago como projeções reconstruíveis e a deduplicação como idempotência pela chave do fato. `app/docs/GLOSSARIO.md:22`; `app/docs/GLOSSARIO.md:27`

## 05/11 15:28 · sessao 30b205fbfa06 · F3-BECO-1 · verificador · origem=Code

1. **Achado 1 — fechado pelo delta. Gravidade: fila; retiro o bloqueio de atomicidade.** O snapshot do defeito, o delete, o `bulk_create` e a re-lavra agora ficam dentro do mesmo `atomic`; uma exceção propagada pela re-lavra desfaz a foto (`app/escala/folgas.py:162`, `app/escala/folgas.py:179`, `app/escala/folgas.py:180`, `app/escala/folgas.py:184`, `app/escala/folgas.py:203`).

   Não encontrei chamador que capture essa falha para aproveitar a foto parcialmente gravada. A view de vinculação e o comando deixam a exceção propagar; os outros dois caminhos já envolviam a geração em transação externa, portanto já desfaziam suas escritas quando a exceção escapava (`app/colaboradores/views.py:245`, `app/escala/management/commands/lavrar_foto_folgas.py:170`, `app/escala/views.py:407`, `app/escala/views.py:411`, `app/colaboradores/services/vinculo.py:187`, `app/colaboradores/services/vinculo.py:275`, `app/colaboradores/services/vinculo.py:289`).

   **O teste novo morde a retirada do `atomic`, pela leitura.** Ele injeta `RuntimeError` na re-lavra, captura a exceção e consulta a foto imediatamente. Sem o `atomic` próprio, a folga de 30/10 permanece na transação do `TestCase`, fazendo falhar a exigência de zero linhas; `assertRaises` não desfaz essa escrita. A fixture efetivamente fornece a data 30 (`app/escala/tests/test_f3_beco1_cadastro_entrou.py:32`, `app/escala/tests/test_f3_beco1_cadastro_entrou.py:183`, `app/escala/tests/test_f3_beco1_cadastro_entrou.py:187`, `app/escala/tests/test_f3_beco1_cadastro_entrou.py:190`, `app/escala/folgas.py:590`). Execução GREEN/RED do mutante: **[não verificado]**.

2. **Achado 2 — permanece a possibilidade de duas solicitações mensais; duplicação de emissão não decorre disso automaticamente. Gravidade: fila.** O snapshot antecede a escrita, sem trava comum de vínculo/mês, e a seleção mensal depende de ANTES verdadeiro e DEPOIS falso (`app/escala/folgas.py:309`, `app/escala/folgas.py:311`, `app/escala/folgas.py:312`, `app/escala/folgas.py:220`, `app/escala/folgas.py:229`).

   Para enumerar os entrelaçamentos, considero duas inclusões que preservam a folga e confirmam com sucesso. A/B são intercambiáveis; o defeito é a inexistência de FolgaDia naquele vínculo/mês (`app/escala/models.py:1249`, `app/escala/folgas.py:312`).

   | Entrelaçamento sob READ COMMITTED | Solicitações de re-julgamento mensal | [nao verificado]
   |---|---|
   | A confirma antes do snapshot de B: A vê ANTES verdadeiro; B vê falso. | **1 vez** (`app/escala/folgas.py:311`, `app/escala/folgas.py:220`, `app/escala/folgas.py:227`). |
   | Ambos tiram snapshot antes do primeiro commit: ambos veem verdadeiro; cada leitura DEPOIS vê ao menos sua própria folga. As comparações DEPOIS podem ocorrer antes ou depois do commit alheio. | **2 vezes** (`app/escala/models.py:1249`, `app/escala/folgas.py:229`, `app/escala/folgas.py:231`). |
   | O mês já estava fora do defeito antes dos dois snapshots. | **0 vezes** (`app/escala/folgas.py:220`, `app/escala/folgas.py:227`). |

   **Com mês inicialmente no defeito e duas inclusões bem-sucedidas, zero não é resultado do entrelaçamento:** a primeira transação a confirmar necessariamente capturou o defeito e vê sua própria inclusão. Na mesma data, eventual espera no `get_or_create` ocorre depois do snapshot e não apaga o verdadeiro já capturado (`app/escala/folgas.py:311`, `app/escala/folgas.py:312`, `app/escala/models.py:1600`). Essas contagens são de solicitações mensais; guardas e exceções podem reduzir os julgamentos efetivamente concluídos (`app/ponto/portas/celula.py:294`, `app/ponto/portas/celula.py:399`, `app/ponto/portas/celula.py:405`).

   No caso de duas passagens, `forcar=True` remove o pulo pela impressão, mas conserva a fábrica e suas deduplicações (`app/ponto/services/cartorio.py:1362`, `app/ponto/services/cartorio.py:997`):

   - **PerguntaDisputa:** a fábrica exclui motivos já existentes por colaborador/dia; `perguntar` encaminha à emissão, cujo gate final procura colaborador/motivo/data e retorna a pergunta existente (`app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/fabrica_da_celula.py:126`, `app/chamados/services/fabrica_da_celula.py:139`, `app/chamados/models.py:2381`, `app/chamados/models.py:2420`, `app/chamados/models.py:2464`).
   - **Chamado:** `abrir` trava a linha do colaborador antes da consulta e retorna o chamado existente; há proteção concorrente explícita nessa porta (`app/chamados/models.py:863`, `app/chamados/models.py:866`, `app/chamados/models.py:867`, `app/chamados/models.py:885`).
   - **Pauta:** o ramo limbo/competência entregue procura cabeças anteriores da âncora dia/DP/marcador, incluindo feitas e canceladas, e escreve somente motivos ausentes. No re-julgamento mensal, exportadas já são excluídas pela guarda (`app/chamados/services/fabrica_da_celula.py:171`, `app/chamados/services/fabrica_da_celula.py:219`, `app/chamados/services/fabrica_da_celula.py:234`, `app/ponto/portas/celula.py:295`).

   Portanto, **a segunda passagem que enxerga a primeira não duplica essas entidades**. Isso não certifica toda corrida: PerguntaDisputa e Pauta usam consulta seguida de criação sem trava própria nesses gates; duplicação efetiva nesse cenário: **[não verificado]** (`app/chamados/models.py:2420`, `app/chamados/models.py:2465`, `app/chamados/services/fabrica_da_celula.py:234`, `app/pautas/services.py:180`).

3. **Achado 3 — permanece a perda possível da cura automática. Gravidade: bloqueia.** A exceção por dia é capturada e o processamento continua; uma falha Python antes da conclusão pode permitir confirmar a folga, consumindo a transição mensal que acionaria nova tentativa (`app/ponto/portas/celula.py:402`, `app/ponto/portas/celula.py:405`, `app/escala/folgas.py:220`, `app/escala/folgas.py:227`).

   O bloco já aparece no parecer anterior anexado e não é alterado pelo diff2 fornecido; confirmação de ancestralidade, autoria e data por Git/blame: **[não verificado]**. Sua localização atual é `app/ponto/portas/celula.py:405`.

   **Existem outros caminhos de produção no código, mas nenhum encontrado garante repetir esse dia apenas porque a folga corrigiu o mês:**

   - O cron configurado executa `processar_cartorio --apply`, sem força. Ele percorre células passadas, porém impressão igual permite pular `indefinida`; a exceção explícita desse atalho é `nunca_bateu` (`app/config/crons.py:597`, `app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/management/commands/processar_cartorio.py:65`, `app/ponto/services/cartorio.py:1358`, `app/ponto/services/cartorio.py:1362`).
   - Signals podem provocar nova avaliação por outro evento, mas chamam sem força; a vigia de hora limita seu universo a hoje/ontem (`app/ponto/signals.py:93`, `app/chamados/signals.py:169`, `app/chamados/signals.py:180`, `app/ponto/services/vigia_de_hora.py:23`).
   - Outro ato de vínculo pode forçar o período novamente; “Cobrar dia” também força uma data passada. São recuperações condicionadas a outro ato (`app/colaboradores/services/vinculo.py:330`, `app/colaboradores/services/pos_vinculo.py:72`, `app/chamados/services/cobrar_dia.py:26`, `app/chamados/services/cobrar_dia.py:39`).

   A impressão não inclui explicitamente a existência mensal de FolgaDia; portanto, só a correção desse insumo não assegura que o cron ultrapasse o atalho (`app/ponto/services/cartorio.py:443`, `app/ponto/services/cartorio.py:452`, `app/ponto/services/cartorio.py:1348`). Recuperação efetivamente ocorrida em produção: **[não verificado]**.

4. **Achado 4 — permanece. Gravidade: bloqueia.** A seleção em `qs_na_fila` ocorre fora da transação da costura. Depois vêm coleta da reversão, gravação do arquivo e carregamento dos pais/perguntas; o `atomic` da porta começa somente ao reapontar (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:59`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:86`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:91`, `app/chamados/services/fio.py:837`).

   **Para a primeira costura efetiva, há quatro SELECTs explícitos após a seleção da fila e antes do UPDATE:** destinos anteriores, pais, perguntas e pai recarregado sob trava. A lavra anterior ao UPDATE acrescenta consultas dependentes do estado; total SQL exato: **[não verificado]** (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:86`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:92`, `app/chamados/services/limbo_folgas.py:237`, `app/chamados/services/fio.py:763`, `app/chamados/services/fio.py:846`, `app/chamados/services/fio.py:847`, `app/chamados/veredito_lavrado.py:219`).

   A trava recarrega o pai, mas a porta aceita imediatamente status não terminal. Assim, silêncio vigente ou mudança para `registrado` entre seleção e trava não impedem a costura, embora retirem o pai da fila (`app/chamados/services/fio.py:765`, `app/chamados/services/painel.py:174`, `app/chamados/services/painel.py:187`, `app/chamados/services/painel.py:188`).

   **Não encontrei chamador pronto que revalide a fila sob trava:** os caminhos examinados desembocam na mesma porta, inclusive migração e reconciliação (`app/chamados/services/limbo_folgas.py:243`, `app/chamados/services/fio.py:947`, `app/chamados/reconciliador.py:1570`). O predicado existente pode ser reutilizado sem inventar regra: `qs_na_fila` aceita `_base`; falta executar essa validação dentro do ato que mantém a trava até a costura (`app/chamados/services/painel.py:158`, `app/chamados/services/painel.py:186`, `app/chamados/services/fio.py:837`).

_1639 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
