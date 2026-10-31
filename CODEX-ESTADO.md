# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 08:47** · publicado em **31/10/2026 08:47** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 08:47 · sessao ac2386711bba · E3-STATUS · verificador · origem=Code

O delta ainda deixa uma regressão demonstrável: **ausência parcial de isento que nunca bateu passa a receber `ok`**, embora a regra preserve o rótulo de ausência acima da isenção. Também permanecem perdas de precedência entre grade, lavra e calendário. (`app/ponto/services/cartorio.py:1040`; `app/ponto/status_dia.py:80`; `app/escala/utils.py:799`.)

**1. Combinações alcançáveis e incoerências**

As cinco coordenadas pedidas não bastam para determinar a resposta: falta `previsto_min`. A mesma tupla `(ausencia, False, True, True, nunca_bateu)` recebe `ok` com previsto positivo e `folga` com previsto zero. Além disso, comparar com o calendário exige conhecer a faixa A2 e o subtipo da ausência, informações que `fatos_do_status` não transporta. (`app/ponto/status_dia.py:80`; `app/ponto/status_dia.py:82`; `app/ponto/status_dia.py:84`; `app/ponto/status_dia.py:112`; `app/colaboradores/services/calendario.py:107`.)

Admin não integra o domínio de `status_da_lavra`: `_status_alvo` desvia tanto a decisão direta quanto a célula anteriormente decidida pelo admin para `status_do_admin`. As três chamadas do cartório passam fatos, mas esse desvio permanece. (`app/ponto/portas/celula.py:694`; `app/ponto/services/cartorio.py:733`; `app/ponto/services/cartorio.py:1041`; `app/ponto/services/cartorio.py:1556`.)

Na tabela, a tupla segue a ordem solicitada; `B` e `I` significam os dois valores booleanos quando indicados.

| Combinação e condição adicional | Status lavrado | Alcance e avaliação |
|---|---|---|
| `(ausencia, False, True, True, nunca_bateu)`, previsto positivo | `ok` | **Regressão do delta.** A grade conserva ausência parcial; o juiz isento retorna vazio, mas o ramo sem adesão força `nunca_bateu`. A nova exceção pula `ausencia` e alcança o isento. A regra escrita preserva ausência/folga no rótulo. (`app/escala/utils.py:1180`; `app/escala/utils.py:1190`; `app/ponto/supra_juiz.py:195`; `app/ponto/services/cartorio.py:1031`; `app/ponto/services/cartorio.py:1040`; `app/ponto/status_dia.py:80`; `app/escala/utils.py:799`.) |
| `(ausencia, False, True, I, nunca_bateu)`, previsto zero | `folga` | O ramo sem adesão também força essa palavra na cobertura parcial sem previsto. A lavra perde a ausência porque cai no teste de zero. Há **conflito textual a explicitar**: A2 mantém ausência acima de folga, enquanto L-089 restringe seu efeito ao dia previsto de trabalho; a própria auditoria das leis registra essa divergência de classificação. Não há palavra normativa única demonstrada apenas pela tupla. (`app/escala/utils.py:1743`; `app/escala/utils.py:1170`; `app/ponto/services/cartorio.py:996`; `app/ponto/services/cartorio.py:1040`; `app/ponto/status_dia.py:82`; `app/ponto/precedencia.py:46`; `app/docs/LEIS.md:121`; `app/docs/LEIS.md:172`.) |
| `(folga, B, False, False, indefinida)`, **com feriado ou folga humana prevalecente** | `escala_suspeita` | **Regressão de precedência do delta.** Célula `trabalha=True` com geradora inválida produz grade folga/zero e acusação de cadastro. Feriado ou folga manual ainda podem vencer pela A2. A lavra agora ultrapassa essa natureza, contrariando a exceção expressa do ADR para `folga`/`folga_trabalhada`. (`app/ponto/supra_juiz.py:168`; `app/ponto/supra_juiz.py:171`; `app/escala/utils.py:1543`; `app/escala/utils.py:1743`; `app/ponto/supra_juiz.py:367`; `app/ponto/supra_juiz.py:405`; `app/ponto/precedencia.py:296`; `app/ponto/precedencia.py:319`; `app/ponto/status_dia.py:77`; `app/docs/ADR/0002-celula-e-o-dia.md:51`.) |
| `(ausencia, False, False, I, concorde)`, subtipo `folga_compensatoria` ou `feriado_folga` | `ausencia` | **Defeito anterior ao delta.** A2 classifica esses subtipos como folga; `_marcar_ausencias` os achata para ausência. A lavra não recebe o subtipo necessário para conservar essa distinção. (`app/ponto/precedencia.py:60`; `app/escala/utils.py:1179`; `app/ponto/status_dia.py:80`; `app/ponto/status_dia.py:112`.) |
| `(ausencia, True, False, False, fato_em_ausencia)`, mesmos subtipos de folga | `irregular` | **Defeito anterior ao delta.** O realizado pode reaparecer depois de o overlay limpar células/órfãs; o juiz acusa fato em ausência e a lavra segue essa acusação, embora A2 diga que era folga. (`app/escala/utils.py:1195`; `app/escala/utils.py:1985`; `app/escala/utils.py:1998`; `app/ponto/supra_juiz.py:361`; `app/escala/models.py:1587`; `app/ponto/status_dia.py:93`; `app/ponto/precedencia.py:60`.) |
| `(ausencia, True, False, True, concorde)`, mesmos subtipos de folga | `ok` | A isenção faz o juiz retornar vazio, derivando `concorde`; a lavra dá `ok`, também perdendo a natureza de folga humana. (`app/ponto/supra_juiz.py:195`; `app/escala/models.py:1595`; `app/ponto/status_dia.py:95`; `app/ponto/precedencia.py:60`.) |

Há ainda uma **lacuna alcançável**, sem gabarito de palavra suficientemente demonstrado: `(trabalho, False, False, False, concorde)`, previsto positivo, após justificativa aprovada. A grade mantém trabalho, marca `justificado` e limpa o furo; o juiz remove `FURO_*`; a lavra retorna `sem_certidao` apesar do julgamento existente. O calendário pode retornar `falta`, que não se sustenta sem ausência descontante pela L-039. A palavra correta dessa justificativa específica é `[nao verificado]`. (`app/escala/utils.py:1198`; `app/escala/utils.py:1203`; `app/ponto/supra_juiz.py:338`; `app/escala/models.py:1595`; `app/ponto/status_dia.py:90`; `app/colaboradores/services/calendario.py:133`; `app/colaboradores/services/calendario.py:161`; `app/docs/LEIS.md:87`.)

**Não é correto reprovar toda ocorrência de `folga + indefinida`.** Sem feriado/folga humana, o calendário pode consultar a célula `trabalha=True` e também mostrar `escala_suspeita`: a “folga” da grade veio da ausência de vínculo válido, não necessariamente da natureza A2 do dia. (`app/ponto/supra_juiz.py:168`; `app/escala/models.py:1071`; `app/colaboradores/services/calendario.py:770`; `app/colaboradores/services/calendario.py:158`.)

Existe também o caminho de rollback: `GRADE_POR_TURNO=False` remapeia a grade descartando `minutos_previstos` e `cobertura_parcial`; a lavra normaliza o previsto ausente para zero. Isso permite, por exemplo, trabalho sem batida virar `folga`. O default da cópia é verdadeiro; valor efetivo de produção e ocorrência desse caminho são `[nao verificado]`. (`app/escala/utils.py:1016`; `app/escala/utils.py:2026`; `app/escala/utils.py:2029`; `app/ponto/status_dia.py:112`; `app/ponto/status_dia.py:82`; `app/config/settings/base.py:165`.)

Exaustividade sobre **estados históricos entregues pelo D5** é `[nao verificado]`: ele reutiliza o veredito armazenado se a impressão coincide, e a própria ADR reconhece que isenção não está nessa impressão. Portanto “mesmo hash” não demonstra que o veredito corresponde a todos os fatos atuais. (`app/ponto/services/cartorio.py:999`; `app/ponto/services/cartorio.py:1155`; `app/ponto/services/cartorio.py:733`; `app/docs/ADR/0002-celula-e-o-dia.md:58`.)

**2. Diferenças em relação ao calendário**

| Situação | Lavra × calendário | Qual regra sustenta |
|---|---|---|
| Trabalho sem batida, `nunca_bateu`, não isento, previsto positivo | `irregular` × `falta` | **Lavra:** falta exige decisão humana. (`app/ponto/status_dia.py:88`; `app/colaboradores/services/calendario.py:56`; `app/docs/LEIS.md:87`.) |
| Mesmo caso, isento | `ok`, rótulo próprio × calendário pode sobrepor `falta` | **Lavra:** ADR determina `ok` para isento; o mapa antigo do calendário ainda sobrepõe `nunca_bateu`. (`app/ponto/status_dia.py:84`; `app/colaboradores/services/calendario.py:122`; `app/colaboradores/services/calendario.py:168`; `app/docs/ADR/0002-celula-e-o-dia.md:42`.) |
| Ausência parcial, sem batida, não isento, previsto positivo, `furo` | `irregular` × `ausencia` | **Lavra:** BO-INSS mantém cobrança do restante do dia. O calendário retorna antes de ler o veredito. (`app/ponto/supra_juiz.py:274`; `app/ponto/supra_juiz.py:279`; `app/ponto/status_dia.py:80`; `app/colaboradores/services/calendario.py:109`.) |
| Ausência parcial de isento sem adesão | `ok` × `ausencia` | **Calendário:** ausência conserva o rótulo acima da isenção; é a regressão descrita acima. (`app/ponto/status_dia.py:80`; `app/colaboradores/services/calendario.py:109`; `app/escala/utils.py:799`.) |
| `indefinida` em feriado/folga humana | `escala_suspeita` × `folga`/`folga_trabalhada` | **Calendário:** exceção de precedência expressa no ADR. L-099 exige coerência, mas não escreve uma prioridade geral de `indefinida` sobre a natureza do dia. (`app/ponto/status_dia.py:77`; `app/colaboradores/services/calendario.py:112`; `app/colaboradores/services/calendario.py:116`; `app/colaboradores/services/calendario.py:168`; `app/docs/ADR/0002-celula-e-o-dia.md:51`; `app/docs/LEIS.md:122`.) |
| Folga lançada pelos subtipos de ausência | `ausencia`, `irregular` ou `ok` × `folga`/`folga_trabalhada` | **Calendário:** A2 reconhece explicitamente esses subtipos como folga. (`app/ponto/precedencia.py:60`; `app/ponto/status_dia.py:80`; `app/ponto/status_dia.py:93`; `app/ponto/status_dia.py:95`; `app/colaboradores/services/calendario.py:116`.) |
| Trabalho coberto, encerrado, `fato_em_ausencia`, batidas regulares | `irregular` × pode ser `ok` ou `sem_certidao` | **Lavra:** o juiz acusa o trabalho em cobertura. O calendário não possui esse veredito no mapa e depende das pontas/desvio. A ressalva são os subtipos de folga da linha anterior. (`app/ponto/supra_juiz.py:361`; `app/ponto/status_dia.py:42`; `app/ponto/status_dia.py:93`; `app/colaboradores/services/calendario.py:54`; `app/colaboradores/services/calendario.py:150`; `app/colaboradores/services/calendario.py:153`.) |
| `fato_sem_previsao` com batida | `folga_trabalhada` × pode permanecer `ok`, `irregular`, `sem_certidao` ou `aberto` | Não há mapa desse veredito no calendário. Se a natureza efetiva for folga, ADR/A2 sustentam `folga_trabalhada`; para outras causas de previsto zero, gabarito universal de status é `[nao verificado]`. (`app/ponto/status_dia.py:91`; `app/colaboradores/services/calendario.py:54`; `app/colaboradores/services/calendario.py:144`; `app/colaboradores/services/calendario.py:148`; `app/docs/ADR/0002-celula-e-o-dia.md:53`.) |
| Admin: `trabalhou`, `faltou`, `justificado` | Lavra sempre `ok`, `falta`, `ausencia`; calendário pode preservar natureza/relógio, suprimir `ok` sem batida ou mapear `justificado→folga` | **Lavra:** decisão humana prevalece expressamente. Esses casos passam por `status_do_admin`, não por `status_da_lavra`. (`app/ponto/status_dia.py:39`; `app/ponto/portas/celula.py:694`; `app/colaboradores/services/calendario.py:58`; `app/colaboradores/services/calendario.py:161`; `app/colaboradores/services/calendario.py:168`; `app/docs/ADR/0002-celula-e-o-dia.md:31`.) |
| Trabalho sem batida, `concorde`, previsto positivo, após justificativa | `sem_certidao` × pode ser `falta` | **Nenhum dos dois demonstrado integralmente:** calendário contraria L-039; palavra correta da lavra é `[nao verificado]`. (`app/ponto/status_dia.py:90`; `app/colaboradores/services/calendario.py:133`; `app/docs/LEIS.md:87`.) |

A coluna lavrada e o estado temporal são comparações diferentes: D4 deve devolver `previsto`/`aberto` antes do encerramento. O calendário, por sua vez, deixa de considerar o turno em curso como “futuro” quando já existe veredito; isso pode expor acusação antes do encerramento. Nesse ponto, **D4 sustenta o leitor novo**, não a coluna isolada nem essa exceção do calendário. (`app/docs/ADR/0002-celula-e-o-dia.md:21`; `app/ponto/status_dia.py:136`; `app/ponto/status_dia.py:138`; `app/colaboradores/services/calendario.py:807`.)

**3. Quem escreve e quem lê**

O inventário abaixo distingue a chave da grade, sua materialização na ata e campos homônimos de dinheiro; estes últimos são entidades distintas segundo o ADR. (`app/ponto/services/cartorio.py:567`; `app/ponto/services/cartorio.py:568`; `app/docs/ADR/0002-celula-e-o-dia.md:8`.)

| Dado | Escritores de produção | Leitores diretos |
|---|---|---|
| `cobertura_parcial` | Grade: `app/escala/utils.py:1190`, consultando o juiz em `app/escala/utils.py:1150`. Reconstrução pela célula: `app/escala/services/leitor_celula.py:394`. Transporte dos fatos: `app/ponto/status_dia.py:115`. | `app/ponto/supra_juiz.py:243`, `app/ponto/supra_juiz.py:279`, `app/ponto/supra_juiz.py:350`; `app/ponto/services/cartorio.py:990`, `app/ponto/services/cartorio.py:998`, `app/ponto/services/cartorio.py:1142`, `app/ponto/services/cartorio.py:1151`; `app/ponto/status_dia.py:80`, `app/ponto/status_dia.py:115`; `app/escala/services/leitor_celula.py:395`. |
| `tipo_dia` | Grade: `app/escala/utils.py:1743`, `app/escala/utils.py:1749`; overlays: `app/escala/utils.py:1118`, `app/escala/utils.py:1174`, `app/escala/utils.py:1180`. Ata: `app/ponto/services/cartorio.py:567`, atribuída/persistida pela porta em `app/ponto/portas/celula.py:768`, `app/ponto/portas/celula.py:640`. Reconstrução: `app/escala/services/leitor_celula.py:340`, `app/escala/services/leitor_celula.py:342`, `app/escala/services/leitor_celula.py:359`. Rollback: `app/escala/utils.py:727`, `app/escala/utils.py:752`, `app/escala/utils.py:987`, `app/escala/utils.py:1011`, `app/escala/utils.py:1016`. | Núcleo: `app/ponto/status_dia.py:112`, `app/ponto/supra_juiz.py:197`, `app/ponto/services/cartorio.py:567`, `app/ponto/services/cartorio.py:996`, `app/escala/utils.py:850`, `app/escala/utils.py:1170`, `app/escala/utils.py:1198`, `app/escala/services/leitor_celula.py:344`, `app/escala/services/leitor_celula.py:384`. Demais consumidores abaixo. |
| `minutos_previstos` da grade | `app/escala/utils.py:1743`, `app/escala/utils.py:1751`; exceção intermitente: `app/escala/utils.py:2010`. Ata: `app/ponto/services/cartorio.py:568`. Reconstrução: `app/escala/services/leitor_celula.py:338`, `app/escala/services/leitor_celula.py:363`, com fallback ao juiz em `app/escala/services/leitor_celula.py:353`. | `app/ponto/status_dia.py:112`; `app/ponto/supra_juiz.py:198`; `app/ponto/services/cartorio.py:568`; `app/escala/services/leitor_celula.py:346`; `app/folha/causas.py:58`, `app/folha/causas.py:84`; `app/folha/export.py:1072`, `app/folha/export.py:1074`, `app/folha/export.py:1079`; `app/ponto/services/fechamento.py:1016`; `app/ponto/services/dia_pago.py:391`; `app/ponto/services/bordas_realizado.py:76`. |
| Veredito `indefinida` | Palavra derivada pelo mapa em `app/escala/models.py:1583`, aplicado em `app/escala/models.py:1597`. Fontes dos códigos: `app/ponto/supra_juiz.py:305`, `app/ponto/supra_juiz.py:312`, `app/ponto/supra_juiz.py:324`, `app/ponto/supra_juiz.py:357`, `app/ponto/supra_juiz.py:370`, `app/ponto/supra_juiz.py:383`, `app/ponto/supra_juiz.py:397`, `app/ponto/supra_juiz.py:407`. Escrita/persistência: `app/ponto/portas/celula.py:770`, `app/ponto/portas/celula.py:775`; o modelo delega em `app/escala/models.py:1794`. | Leitores literais: `app/ponto/status_dia.py:77`; `app/colaboradores/services/calendario.py:56`; `app/relatorios/furos.py:26`; `app/relatorios/dia_da_frota.py:12`. Também integra `VEREDITOS_ACUSAM`, cujos consumidores seguem abaixo. (`app/escala/models.py:1735`.) |

Outros leitores de **`tipo_dia`**, agrupados por consumidor:

- Folha/dinheiro: `app/folha/causas.py:40`, `app/folha/causas.py:41`, `app/folha/causas.py:100`; `app/folha/export.py:1071`, `app/folha/export.py:1075`, `app/folha/export.py:1080`; `app/ponto/services/fechamento.py:1011`, `app/ponto/services/fechamento.py:1012`, `app/ponto/services/fechamento.py:1015`; `app/ponto/services/dia_pago.py:386`, `app/ponto/services/dia_pago.py:393`, `app/ponto/services/dia_pago.py:398`; `app/ponto/services/bordas_realizado.py:74`.
- Ponto/escala: `app/ponto/propositor.py:36`, `app/ponto/propositor.py:210`; `app/ponto/worklist.py:19`; `app/ponto/services/veredito_celula.py:221`; `app/ponto/services/espelho.py:690`; `app/ponto/services/simular_dia.py:12`; `app/escala/orfa_classe.py:65`; `app/escala/services/furos_diarios.py:39`.
- Chamados/relatórios: `app/chamados/services/materializacao.py:223`; `app/chamados/services/declaracao_texto.py:90`; `app/chamados/services/disputa_emissao.py:68`; `app/chamados/juizes.py:401`; `app/relatorios/furos.py:147`, `app/relatorios/furos.py:151`, `app/relatorios/furos.py:218`, `app/relatorios/furos.py:237`; `app/inteligencia/calculadores.py:46`, `app/inteligencia/calculadores.py:141`.

O calendário usa **`tipo_do_dia` da precedência**, não a chave `tipo_dia` da grade; confundi-las esconderia justamente as diferenças acima. (`app/colaboradores/services/calendario.py:794`; `app/ponto/status_dia.py:112`.)

Os homônimos persistidos **`minutos_previstos` em DiaPago/FechamentoMensal** têm escritores em `app/ponto/services/dia_pago.py:287`, `app/ponto/services/dia_pago.py:474`, `app/ponto/services/dia_pago.py:480`, `app/ponto/services/dia_pago.py:493`, `app/ponto/services/fechamento.py:1051`, `app/ponto/services/fechamento.py:1062`; leitores em `app/folha/export.py:1019`, `app/folha/export.py:1024`, `app/folha/porta_export.py:135`, `app/folha/porta_export.py:347`, `app/ponto/services/dia_pago.py:491`, `app/ponto/services/fechamento.py:1149`, `app/relatorios/cartao_pela_celula.py:117`, `app/relatorios/cartao_pela_celula.py:120`, `app/relatorios/cartao_pela_celula.py:172`. O histórico do modelo consulta/acumula o juiz em `app/ponto/models.py:879`, `app/ponto/models.py:883`.

Leitores de **`indefinida` via `VEREDITOS_ACUSAM`**: `app/folha/export.py:1215`; `app/chamados/juizes.py:1020`, `app/chamados/juizes.py:1074`; `app/chamados/models.py:2623`, `app/chamados/models.py:2624`; `app/chamados/services/disputa_emissao.py:705`; `app/chamados/services/limbo_folgas.py:194`; `app/chamados/services/resposta_tardia.py:45`; `app/chamados/services/vigia_fio.py:314`, `app/chamados/services/vigia_fio.py:362`; `app/chamados/services/silencio_ausencia.py:41`; `app/chamados/services/fabrica_da_celula.py:86`; `app/chamados/reconciliador.py:598`, `app/chamados/reconciliador.py:982`; `app/folha/services/vigia_pausa.py:155`, `app/folha/services/vigia_pausa.py:157`; `app/ponto/services/vigia_ausencia_veredito.py:41`, `app/ponto/services/vigia_ausencia_veredito.py:42`; `app/relatorios/dia_da_frota.py:55`.

**Efeito do delta nos leitores:** muda o status calculado pela porta e, depois de persistido, a resposta de `status_do_dia_lavrado`; não muda a palavra do veredito nem migra calendário, folha ou chamados para outra leitura. O calendário permanece congelado até E4. (`app/ponto/portas/celula.py:698`; `app/ponto/status_dia.py:142`; `app/docs/ADR/0002-celula-e-o-dia.md:71`.)

**A correção não alcança automaticamente status existentes:** D5 retorna para qualquer status não nulo; o julgamento normal também pode pular pela impressão. Quantos registros continuam com a resposta anterior é `[nao verificado]`. (`app/ponto/services/cartorio.py:726`; `app/ponto/services/cartorio.py:1155`; `app/ponto/services/cartorio.py:1159`.)

**4. Checklist**

- **Segundo juiz:** `status_da_lavra` e `status_do_dia_lavrado` são as duas metades declaradas da autoridade, com registro e corte. O calendário continua sendo derivação antiga expressamente mantida até E4; essa coexistência não constitui criação clandestina pelo delta. (`app/core/juizes.py:169`; `app/docs/JUIZES.md:74`; `app/docs/CORTES.md:414`; `app/core/medida_leitor_do_dia.py:269`; `app/core/medida_leitor_do_dia.py:284`; `app/docs/ADR/0002-celula-e-o-dia.md:71`.)
- **Segundo escritor fora da porta:** a atribuição de status encontrada está na porta; persistência passa pelo save central, inclusive após `save=False` via `carimbar`. Há dois escritores **em memória** de `cobertura_parcial`, ambos consultando a autoridade existente; isso não é outro escritor da coluna de status. (`app/ponto/portas/celula.py:705`; `app/ponto/portas/celula.py:640`; `app/ponto/portas/celula.py:807`; `app/escala/utils.py:1150`; `app/escala/utils.py:1190`; `app/escala/services/leitor_celula.py:394`.)
- **Relógio fora da borda:** o delta corrige o comando para uma leitura e deriva `hoje` dela. Permanecem leituras preexistentes no leitor 

[truncado em 20000 de 22791 caracteres]

## 31/10 08:44 · sessao dd7a74435bc0 · - · - · origem=Ronald (tmux)

Leitura complementar concluída, sem escrita de arquivo. Os três testes novos executados por AST em memória passam no código atual; no mutante que restaura o comportamento anterior, mantendo a assinatura nova, há **2 falhas e 1 erro**: ausência parcial volta a `ausencia` em vez de `irregular`, folga indefinida volta a `folga`, e desaparece a chave `cobertura_parcial`. Isso comprova mordida contra reversão, sem comprovar RED histórico nem integração Django completa. Referências: `app/ponto/tests/test_e3_status_lavrado.py:78`, `:88`, `:101`; comportamento revertido: `app/ponto/status_dia.py:75`, `:115`.

Diferenças confirmadas para a consolidação:

- **Parcial sem batida acusada**: lavra `irregular`, calendário retorna `ausencia` antes de consultar veredito. A cobrança parcial tem regra explícita BO-INSS; portanto sustenta a lavra. Caminho: `app/escala/utils.py:1184` → `app/ponto/supra_juiz.py:274` e `:279` → `app/ponto/status_dia.py:80` e `:88`; calendário: `app/colaboradores/services/calendario.py:109`.
- **`fato_em_ausencia` com batidas completas regulares em cobertura parcial**: lavra `irregular`, calendário pode mostrar `ok`, pois `_VER_STATUS` não contém esse veredito. Pode mostrar também `sem_certidao`, `aberto` ou `folga_trabalhada` conforme os outros fatos. O ramo do juiz diz expressamente que acusa e retém: `app/ponto/supra_juiz.py:348`, `:361`; mapa: `app/escala/models.py:1587`; lavra: `app/ponto/status_dia.py:42`, `:93`; calendário: `app/colaboradores/services/calendario.py:54`, `:144`, `:150`, `:153`, `:158`. Para dia encerrado de trabalho coberto, a regra sustenta a acusação da lavra; ADR exige coerência tela/folha: `app/docs/ADR/0002-celula-e-o-dia.md:29`.
- **`nunca_bateu` em trabalho sem batida**: lavra `irregular`, calendário `falta`; para isento, lavra `ok` e calendário ainda pode sobrepor `falta`. Sustentação da lavra: falta exige decisão, `app/docs/LEIS.md:87`; isento `ok`, `app/docs/ADR/0002-celula-e-o-dia.md:42`, e validação do rótulo em `app/docs/CORTES.md:411`. Código: `app/ponto/status_dia.py:84`, `:88`; calendário `app/colaboradores/services/calendario.py:56`, `:122`, `:168`.
- **Admin**: a porta sempre aplica `trabalhou→ok`, `faltou→falta`, `justificado→ausencia`; calendário pode contrariar qualquer um pelos retornos de ausência/feriado, status protegidos, supressão de `ok` sem batida, mapa `justificado→folga` ou overlay final de cobertura. A regra sustenta a lavra: decisão humana prevalece. Referências: `app/ponto/portas/celula.py:694`; `app/ponto/status_dia.py:39`; `app/colaboradores/services/calendario.py:58`, `:109`, `:112`, `:161`, `:168`, `:172`; `app/docs/ADR/0002-celula-e-o-dia.md:31`, `:44`.
- **Relógio é comparação separada**: a coluna não pretende mostrar `previsto/aberto`; o leitor D4 cobre a coluna antes do encerramento. O calendário também consulta encerramento, mas desativa seu `futuro` de turno em curso quando já há veredito. Referências: `app/ponto/status_dia.py:136`, `:138`; `app/colaboradores/services/calendario.py:697`, `:807`; regra D4: `app/docs/ADR/0002-celula-e-o-dia.md:21`.
- **Não concluir divergência automaticamente de `tipo_dia='folga'` + `indefinida`**: o caminho sem vínculo exige célula `trabalha=True`, que o calendário também consulta; pode resultar `escala_suspeita` nos dois. Referências: `app/ponto/supra_juiz.py:168`, `:405`; `app/colaboradores/services/calendario.py:760`, `:770`, `:158`.

Checklist:

- As duas funções novas são metades declaradas da autoridade: `app/core/medida_leitor_do_dia.py:269`; juiz registrado em `app/core/juizes.py:169` e `app/docs/JUIZES.md:74`. O calendário permanece derivador antigo declarado, congelado até E4: `app/core/medida_leitor_do_dia.py:284`; `app/docs/ADR/0002-celula-e-o-dia.md:70`.
- O comando corrigiu para uma leitura na borda e passa ambos os valores: `app/ponto/management/commands/lavrar_status_competencia.py:43`, `:55`. A função pura de lavra não lê relógio: `app/ponto/status_dia.py:55`.
- Escrita de status acontece na porta: `app/ponto/portas/celula.py:705`, `:752`, `:773`; selo inclui os campos e exige fatos: `app/ponto/tests/test_selo_veredito_so_pela_porta.py:16`, `:156`.
- Ordem de pousos escrita: `app/docs/CORTES.md:413`; E2 antes de E3: `app/docs/BACKLOG.md:773`. Execução efetiva dessa ordem e pouso: `[nao verificado]`.
- E2 não está demonstrada apenas por selos verdes: lacuna de isenção reconhecida no ADR e `trabalha` ainda fora da impressão na catraca. Referências: `app/docs/ADR/0002-celula-e-o-dia.md:58`; `app/escala/tests/test_d3_guarda_da_linha_celula.py:68`.
- O comentário do teste associa sem batida a `PARCIAL_SEM_PERIODO`, mas esse código real exige realizado ou órfãs, que tornam `tem_batida=True`. O caso sem batida com `indefinida` pode vir dos outros códigos de cadastro. Referências: `app/ponto/tests/test_e3_status_lavrado.py:82`; `app/ponto/supra_juiz.py:280`, `:348`; `app/ponto/status_dia.py:109`.

## 31/10 08:44 · sessao 8cc0e7d5deae · - · - · origem=Ronald (tmux)

Achados de alcance confirmados:

1. **Regressão nova: ausência parcial do isento sem adesão.** Tupla `(ausencia, False, True, True, nunca_bateu)`: o cartório força `nunca_bateu` apesar do juiz retornar vazio para isento (`app/ponto/services/cartorio.py:963`, `app/ponto/services/cartorio.py:996`, `app/ponto/services/cartorio.py:1031`, `app/ponto/services/cartorio.py:1040`; `app/ponto/supra_juiz.py:195`). A lavra nova salta `ausencia` e retorna `ok` com previsto positivo; retorna `folga` com previsto zero (`app/ponto/status_dia.py:80`, `app/ponto/status_dia.py:82`, `app/ponto/status_dia.py:84`). O calendário retorna `ausencia` antes de consultar isenção (`app/colaboradores/services/calendario.py:109`). A regra escrita mantém a natureza/rotulagem de ausência apesar da isenção (`app/escala/utils.py:795`, `app/escala/utils.py:799`).

2. **`indefinida` sobre feriado ou folga humana.** Alcance: célula `trabalha=True`, geradora inválida, cadastro retorna `sem_vinculo_em_dia_de_trabalho=True`, grade entrega folga/zero, juiz produz `indefinida` (`app/ponto/supra_juiz.py:168`, `app/ponto/supra_juiz.py:171`, `app/escala/utils.py:1543`, `app/escala/utils.py:1743`, `app/ponto/supra_juiz.py:367`, `app/ponto/supra_juiz.py:405`, `app/escala/models.py:1583`). Havendo feriado ou folga manual, A2 ainda reconhece essa precedência (`app/ponto/precedencia.py:291`, `app/ponto/precedencia.py:296`, `app/ponto/precedencia.py:314`, `app/ponto/precedencia.py:319`, `app/ponto/precedencia.py:321`). A lavra dá `escala_suspeita`; calendário dá `folga`/`folga_trabalhada`, protegidas contra o veredito (`app/ponto/status_dia.py:77`; `app/colaboradores/services/calendario.py:112`, `app/colaboradores/services/calendario.py:116`, `app/colaboradores/services/calendario.py:146`, `app/colaboradores/services/calendario.py:168`). ADR sustenta a precedência do calendário (`app/docs/ADR/0002-celula-e-o-dia.md:51`). **Sem feriado/folga humana, não inferir diferença apenas de `tipo_dia='folga'`: o calendário lê a célula soberana `trabalha=True` e pode também resultar `escala_suspeita`** (`app/escala/models.py:1071`, `app/colaboradores/services/calendario.py:770`).

3. **Folga lançada como ausência perde sua natureza na grade.** `folga_compensatoria`/`feriado_folga` pertencem à faixa folga de A2, mas `_marcar_ausencias` escreve `tipo_dia='ausencia'` para todo não-falta (`app/ponto/precedencia.py:60`, `app/escala/utils.py:1179`). Sem batida, lavra `ausencia` versus calendário `folga`; com realizado e veredito `fato_em_ausencia`, lavra `irregular` versus calendário `folga_trabalhada` (`app/ponto/status_dia.py:80`, `app/ponto/status_dia.py:93`; `app/colaboradores/services/calendario.py:116`, `app/colaboradores/services/calendario.py:134`, `app/colaboradores/services/calendario.py:146`). O segundo caso é alcançável porque o realizado é calculado depois de o overlay limpar células/órfãs (`app/escala/utils.py:1195`, `app/escala/utils.py:1985`, `app/escala/utils.py:1998`; `app/ponto/supra_juiz.py:361`). ADR/A2 sustentam o calendário (`app/docs/ADR/0002-celula-e-o-dia.md:54`; `app/ponto/precedencia.py:60`).

4. **Justificativa aprovada pode produzir trabalho sem batida e `concorde`, lavrado `sem_certidao`.** A grade limpa missing/órfãs e marca `justificado`, mantendo trabalho; o juiz remove o código de furo; vazio deriva `concorde`; a lavra cai no retorno residual (`app/escala/utils.py:1198`, `app/escala/utils.py:1203`; `app/ponto/supra_juiz.py:338`; `app/escala/models.py:1595`; `app/ponto/status_dia.py:90`). Calendário pode mostrar `falta`, pois suprime o `ok` sem batida (`app/colaboradores/services/calendario.py:133`, `app/colaboradores/services/calendario.py:161`). A palavra correta dessa justificativa específica não está explicitamente fixada nos trechos normativos que li `[nao verificado]`; `falta` sem ausência descontante contraria L-039 (`app/docs/LEIS.md:87`).

Cuidados para a matriz:

- As cinco coordenadas solicitadas não determinam a resposta: é necessário incluir `previsto_min`; ausência parcial sem batida e `nunca_bateu` retorna `folga` se zero, `irregular` se positivo e não isento, `ok` se positivo e isento (`app/ponto/status_dia.py:80`, `app/ponto/status_dia.py:82`, `app/ponto/status_dia.py:84`, `app/ponto/status_dia.py:88`).
- Admin não chega a `status_da_lavra`: `_status_alvo` desvia para `status_do_admin` (`app/ponto/portas/celula.py:694`).
- `PARCIAL_SEM_PERIODO` exige realizado ou órfã; o teste novo com `tem_batida=False` e essa justificativa no comentário não demonstra esse caminho (`app/ponto/supra_juiz.py:348`, `app/ponto/supra_juiz.py:351`; `app/ponto/tests/test_e3_status_lavrado.py:84`).
- O rollback legacy descarta previsto e cobertura parcial no remapeamento; também é caminho do código, embora a flag de produção seja `[nao verificado]` (`app/escala/utils.py:1016`, `app/escala/utils.py:2026`; `app/config/settings/base.py:165`).
- Frequências, colaboradores atingidos e ocorrência efetiva em produção: `[nao verificado]`.

## 31/10 08:43 · sessao ea0196bb5f24 · - · - · origem=Ronald (tmux)

Censo somente leitura, com escopo dos nomes usados pelo fluxo do dia. Não executei testes ou acesso a banco; prevalência em produção e configuração de ambiente são `[nao verificado]`. [nao verificado]

**`cobertura_parcial`**

- Escritor da grade do cartório: `app/escala/utils.py:1190`, alimentado pelo juiz `cobre_parte_do_dia`, importado em `app/escala/utils.py:1138` e aplicado em `app/escala/utils.py:1150`.
- Existe também escritor na reconstrução da grade pela célula: `app/escala/services/leitor_celula.py:394`; usa `ausencia_explica_o_dia_inteiro`, importado em `app/escala/services/leitor_celula.py:388`. A finalidade e os consumidores supra-juiz/retroativo estão declarados em `app/escala/services/leitor_celula.py:375`.
- Leitores: `app/ponto/supra_juiz.py:243`, aplicado à cobertura integral em `app/ponto/supra_juiz.py:244`, à cobrança em `app/ponto/supra_juiz.py:279` e ao período em `app/ponto/supra_juiz.py:350`; `app/ponto/services/cartorio.py:990`, `app/ponto/services/cartorio.py:1142`, assinatura dos insumos em `app/ponto/services/cartorio.py:998` e `app/ponto/services/cartorio.py:1151`; transporte para a regra nova em `app/ponto/status_dia.py:115`, decisão em `app/ponto/status_dia.py:80`; o próprio escritor reconstruído também lê em `app/escala/services/leitor_celula.py:395`.

**`tipo_dia`**

- Escritores da grade principal: folga `app/escala/utils.py:1743`, trabalho `app/escala/utils.py:1749`, overlay da foto de folga `app/escala/utils.py:1118`, falta aprovada `app/escala/utils.py:1174`, ausência `app/escala/utils.py:1180`.
- Materialização na ata: `app/ponto/services/cartorio.py:567`; a atribuição da ata à célula é da porta `app/ponto/portas/celula.py:768`, persistida em `app/ponto/portas/celula.py:640`.
- Reconstrução pelo leitor: base fora de vínculo `app/escala/services/leitor_celula.py:340`, sem célula `app/escala/services/leitor_celula.py:342`, leitura da ata/fallback `app/escala/services/leitor_celula.py:344`, escrita do dia reconstruído `app/escala/services/leitor_celula.py:359`.
- Leitores diretos de produção: `app/ponto/status_dia.py:112`, regra `app/ponto/status_dia.py:75`, `app/ponto/status_dia.py:80`, `app/ponto/status_dia.py:82`, `app/ponto/status_dia.py:91`; classificador `app/ponto/supra_juiz.py:197`; cartório `app/ponto/services/cartorio.py:567`, `app/ponto/services/cartorio.py:996`; isenção da grade `app/escala/utils.py:850`, overlays `app/escala/utils.py:1170`, `app/escala/utils.py:1198`; reconstrução `app/escala/services/leitor_celula.py:344`, `app/escala/services/leitor_celula.py:384`; folha `app/folha/causas.py:40`, `app/folha/causas.py:41`, `app/folha/causas.py:100`, `app/folha/export.py:1071`, `app/folha/export.py:1075`, `app/folha/export.py:1080`; fechamento `app/ponto/services/fechamento.py:1011`, `app/ponto/services/fechamento.py:1012`, `app/ponto/services/fechamento.py:1015`; DiaPago `app/ponto/services/dia_pago.py:386`, `app/ponto/services/dia_pago.py:393`, `app/ponto/services/dia_pago.py:398`; bordas do realizado `app/ponto/services/bordas_realizado.py:74`; propositor `app/ponto/propositor.py:36`, `app/ponto/propositor.py:210`; worklist `app/ponto/worklist.py:19`; decisão humana `app/ponto/services/veredito_celula.py:221`; espelho `app/ponto/services/espelho.py:690`; simulação `app/ponto/services/simular_dia.py:12`; relatórios `app/relatorios/furos.py:147`, `app/relatorios/furos.py:151`, `app/relatorios/furos.py:218`, `app/relatorios/furos.py:237`; materialização `app/chamados/services/materializacao.py:223`; inteligência `app/inteligencia/calculadores.py:46`, `app/inteligencia/calculadores.py:141`; declaração `app/chamados/services/declaracao_texto.py:90`; classificação de órfã `app/escala/orfa_classe.py:65`; furos diários `app/escala/services/furos_diarios.py:39`; emissão `app/chamados/services/disputa_emissao.py:68`; leitura do tipo da ata por juiz de chamado `app/chamados/juizes.py:401`.
- `calendario.py` usa outra chave, `tipo_do_dia`, da precedência; não confundir com `tipo_dia` da grade: `app/colaboradores/services/calendario.py:794`.

**`minutos_previstos` do dia**

- Escritores da grade principal: zero de folga `app/escala/utils.py:1743`, previsto de trabalho `app/escala/utils.py:1751`, exceção do intermitente igualando ao realizado `app/escala/utils.py:2010`.
- Ata: `app/ponto/services/cartorio.py:568`, persistida pela mesma porta referida acima.
- Reconstrução: base zero `app/escala/services/leitor_celula.py:338`, leitura da ata `app/escala/services/leitor_celula.py:346`, fallback consultando autoridade `app/escala/services/leitor_celula.py:353`, escrita normalizada `app/escala/services/leitor_celula.py:363`.
- Leitores diretos do campo do dia: `app/ponto/status_dia.py:112`; `app/ponto/supra_juiz.py:198`; `app/ponto/services/cartorio.py:568`; `app/escala/services/leitor_celula.py:346`; `app/folha/causas.py:58`, `app/folha/causas.py:84`; `app/folha/export.py:1072`, `app/folha/export.py:1074`, `app/folha/export.py:1079`; `app/ponto/services/fechamento.py:1016`; `app/ponto/services/dia_pago.py:391`; `app/ponto/services/bordas_realizado.py:76`.
- Há campos homônimos persistidos em outros modelos: `DiaPago` em `app/ponto/models.py:350` e `FechamentoMensal` em `app/ponto/models.py:432`. Escritores desses campos: `app/ponto/services/dia_pago.py:287`, `app/ponto/services/dia_pago.py:474`, `app/ponto/services/dia_pago.py:480`, `app/ponto/services/dia_pago.py:493`; `app/ponto/services/fechamento.py:1051`, `app/ponto/services/fechamento.py:1062`. Não tratá-los como escritores paralelos da chave da grade; são destinos distintos do dado, conforme `app/ponto/services/dia_pago.py:423` e `app/docs/ADR/0002-celula-e-o-dia.md:8`.
- Leitores escalares desses homônimos: `app/folha/export.py:1019`, `app/folha/export.py:1024`; `app/folha/porta_export.py:135`, `app/folha/porta_export.py:347`; `app/ponto/services/dia_pago.py:491`; `app/ponto/services/fechamento.py:1149`; `app/relatorios/cartao_pela_celula.py:117`, `app/relatorios/cartao_pela_celula.py:120`, `app/relatorios/cartao_pela_celula.py:172`; a reconstrução de histórico do próprio modelo consulta o juiz em `app/ponto/models.py:879`, acumulando em `app/ponto/models.py:883`.

**Atenção ao rollback da grade**

- O default da cópia é `GRADE_POR_TURNO=True`, mas é sobrescrevível por ambiente: `app/config/settings/base.py:165`; valor efetivo de produção `[nao verificado]`.
- A alternativa continua alcançável pelo código em `app/escala/utils.py:2029`. Seus escritores de `tipo_dia` são `app/escala/utils.py:727`, `app/escala/utils.py:752`, `app/escala/utils.py:987`, `app/escala/utils.py:1011`, `app/escala/utils.py:1016`.
- Esse remapeamento não leva `minutos_previstos` nem `cobertura_parcial`: `app/escala/utils.py:1016`; o status normaliza minutos ausentes para zero em `app/ponto/status_dia.py:112`. Portanto uma resposta exaustiva do código precisa citar essa alternativa ou declarar que considera o builder por turno default, nunca assumir configuração de produção.

**Veredito `indefinida`**

- Gerador da palavra: mapa código→veredito `app/escala/models.py:1583`, aplicado por `app/escala/models.py:1597`. Escrita no objeto/persistência é da porta em `app/ponto/portas/celula.py:770` e `app/ponto/portas/celula.py:775`; o método do modelo só delega em `app/escala/models.py:1794`.
- O ramo normal classifica e lavra pela terceira chamada de fatos em `app/ponto/services/cartorio.py:1556`; D5 pode repetir veredito já existente em `app/ponto/services/cartorio.py:733`. No nunca-bateu, sem cobertura integral a palavra é explicitamente substituída por `nunca_bateu`: `app/ponto/services/cartorio.py:1031`, `app/ponto/services/cartorio.py:1040`.
- Leitor literal novo afetado pelo delta: `app/ponto/status_dia.py:77`. Outros leitores literais: mapa do calendário `app/colaboradores/services/calendario.py:56`, mapa do relatório de furos `app/relatorios/furos.py:26`, rótulo da frota `app/relatorios/dia_da_frota.py:12`. A constante `INDEFINIDA` em `app/escala/orfa_classe.py:24` é outro domínio, não veredito da célula.
- Leitores da palavra por `VEREDITOS_ACUSAM`, que a inclui em `app/escala/models.py:1735`: `app/folha/export.py:1215`; `app/chamados/juizes.py:1020`, `app/chamados/juizes.py:1074`; `app/chamados/models.py:2623`, `app/chamados/models.py:2624`; `app/chamados/services/disputa_emissao.py:705`; `app/chamados/services/limbo_folgas.py:194`; `app/chamados/services/resposta_tardia.py:45`; `app/chamados/services/vigia_fio.py:314`, `app/chamados/services/vigia_fio.py:362`; `app/chamados/services/silencio_ausencia.py:41`; `app/chamados/services/fabrica_da_celula.py:86`; `app/chamados/reconciliador.py:598`, `app/chamados/reconciliador.py:982`; `app/folha/services/vigia_pausa.py:155`, `app/folha/services/vigia_pausa.py:157`; `app/ponto/services/vigia_ausencia_veredito.py:41`, `app/ponto/services/vigia_ausencia_veredito.py:42`; `app/relatorios/dia_da_frota.py:55`.
- O relatório de furos pode descartar o veredito antes do mapa em dias não trabalho: `app/relatorios/furos.py:151`; a frota também trata `not cel.trabalha` antes de contar veredito em `app/relatorios/dia_da_frota.py:47`. Delta não migra esses consumidores.

**Leitores que mudam / não mudam com o delta**

- Muda a saída de `status_da_lavra`, chamada pela porta em `app/ponto/portas/celula.py:698`. Isso se torna observável pelo leitor registrado, `app/ponto/status_dia.py:142`, depois de lavrar a nova palavra; o calendário segue leitura própria em `app/colaboradores/services/calendario.py:512`, conforme congelamento até E4 em `app/docs/ADR/0002-celula-e-o-dia.md:71`.
- O D5 **não corrige status já lavrado**: retorna imediatamente se não nulo em `app/ponto/services/cartorio.py:726`. A correção depende de relavra normal; o delta não altera o veredito irmão, pois as mudanças fornecidas estão na regra do status, no transporte da parcial e no relógio do comando.
- Relavra normal também pode pular a célula em dia pela impressão: `app/ponto/services/cartorio.py:1155`, `app/ponto/services/cartorio.py:1159`. Sem mudança de insumo/forçar, não se pode afirmar que status incorretos existentes foram corrigidos `[nao verificado]`.

**Checklist**

- Juiz novo não registrado: não; registro explícito `app/core/juizes.py:169`, `app/core/juizes.py:173`, documentação `app/docs/JUIZES.md:74`.
- Segundo escritor do status fora da porta: não encontrei; atribuição do status em `app/ponto/portas/celula.py:705`, save central `app/ponto/portas/celula.py:640`. Dois escritores em memória da parcial não significam dois juízes, mas contrariam a leitura literal de “um escritor” se alguém a estender indiscriminadamente a todos os builders: `app/escala/utils.py:1190`, `app/escala/services/leitor_celula.py:394`.
- Relógio do delta: comando agora lê uma vez e deriva hoje em `app/ponto/management/commands/lavrar_status_competencia.py:43`, `app/ponto/management/commands/lavrar_status_competencia.py:44`. Permanecem fallbacks preexistentes fora de comando: `app/ponto/status_dia.py:131`, `app/ponto/portas/celula.py:772`, e unidade `app/ponto/services/cartorio.py:1684`, `app/ponto/services/cartorio.py:1705`, `app/ponto/services/cartorio.py:1708`. O caminho D5 não passa pelo relógio da porta, porque retorna no ramo status-only em `app/ponto/portas/celula.py:753`–`app/ponto/portas/celula.py:766`.
- Ordem de trava: delta não acrescenta trava. Save e evento estão no mesmo atomic em `app/ponto/portas/celula.py:639`; ausência global de inversão/deadlock em produção `[nao verificado]`.
- Pré-requisito: a própria ADR reconhece isenção fora da impressão em `app/docs/ADR/0002-celula-e-o-dia.md:58`, deixando para E2 em `app/docs/ADR/0002-celula-e-o-dia.md:59`; não declarar que cobertura completa dos insumos foi provada. Migração de telas fica E4 em `app/docs/ADR/0002-celula-e-o-dia.md:71`.

## 31/10 08:26 · sessao 7e4502ad0cab · F1C4-3 · verificador · origem=Code

**A correção local atende ao aval, mas a obra ainda deixa um leitor que transforma indeterminado em “dia sem trabalho” e um caminho do vigia que usa suspensão de cobrança como motivo de retratação.** (`app/chamados/juizes.py:408`; `app/ponto/supra_juiz.py:441`; `app/ponto/management/commands/reconciliar_grade.py:401`)

**1. Onde falha — e onde está correto**

O novo `dia_sem_trabalho` preserva as duas cláusulas autorizadas: `trabalha is False` ou `ata.tipo_dia` presente e diferente de `trabalho`; `None` sozinho deixou de significar folga, conforme o próprio campo. (`app/chamados/juizes.py:407`; `app/chamados/juizes.py:408`; `app/escala/models.py:1656`)

O ramo novo dos indefinidos **usa corretamente a chave do mapa**: o produtor entrega `(colaborador_id, data)` e o consumidor consulta `(c.pk, _d)`; não há defeito de chave nessa versão. (`app/escala/alimentacao.py:41`; `app/escala/alimentacao.py:42`; `app/ponto/management/commands/reconciliar_grade.py:282`)

A propagação de `para=para` na recursão também está correta: a ausência da véspera passa a ser consultada pelo mesmo efeito solicitado para D. Folha aceita somente aprovada; cobrança aceita não rejeitada. (`app/chamados/juizes.py:426`; `app/chamados/juizes.py:448`; `app/ponto/turnos.py:2487`; `app/ponto/turnos.py:2489`)

**Falha remanescente no vigia:** considere célula `None`, ata de trabalho com lâmpada apagada, ausência pendente no próprio D, vínculo válido e chamado único, sem outras causas de absolvição. O filtro novo deixa `_indef` vazio, mas `_coberto`, usando `para='cobranca'`, ainda põe D em `mortos`; `_lei_cala_no_dia` também cala pela ausência pendente. Esse é um contraexemplo deduzido do fluxo, não uma ocorrência comprovada em produção. (`app/ponto/management/commands/reconciliar_grade.py:266`; `app/ponto/management/commands/reconciliar_grade.py:281`; `app/ponto/management/commands/reconciliar_grade.py:401`; `app/ponto/marcos.py:171`; `app/escala/servico_jornada.py:342`)

Nesse caso, o comando registra `RETRACT` e acrescenta `retracts` antes de chamar a porta; a porta consulta a premissa para **folha**, ignora a pendente e encontra o furo, podendo recusar o fechamento. O comando ignora essa recusa e o vigia continua contando a intenção em `len(retracts)`. Portanto, o filtro novo não elimina esse motivo de divergência persistente. (`app/ponto/management/commands/reconciliar_grade.py:303`; `app/ponto/management/commands/reconciliar_grade.py:304`; `app/ponto/management/commands/reconciliar_grade.py:309`; `app/chamados/models.py:699`; `app/chamados/models.py:1247`; `app/chamados/reconciliador.py:525`; `app/ponto/management/commands/reconciliar_grade.py:337`)

Há ainda uma limitação anterior na recursão: qualquer dia considerado sem trabalho consulta D−1, sem verificar naquele trecho se existe relação com turno noturno; resultado desconhecido da véspera vira `False`. O diff corrige o efeito transmitido, mas não modifica essa seleção de dia. Para `batida_ausente`, o coletor possui absolvição direta por dia sem trabalho antes dessa chamada; chamados `turno_aberto*` são excluídos dessa absolvição direta. (`app/chamados/juizes.py:443`; `app/chamados/juizes.py:448`; `app/chamados/juizes.py:449`; `app/chamados/models.py:1232`; `app/chamados/models.py:1247`)

**2. Quem escreve e quem lê**

| Campo/valor | Escritor ou produtor | Leitor e efeito |
|---|---|---|
| `CelulaDia.trabalha` | A porta calcula pela autoridade do vínculo e cria/altera na geração e regeneração; também escreve na reversão. (`app/ponto/portas/celula.py:243`; `app/ponto/portas/celula.py:260`; `app/ponto/portas/celula.py:532`; `app/ponto/portas/celula.py:590`; `app/ponto/portas/celula.py:612`) | `eh_dia_trabalho` devolve o triestado intacto; o predicado alterado compara estritamente com `False`; o reconciliador passa a consultá-lo. (`app/escala/models.py:1072`; `app/chamados/juizes.py:408`; `app/ponto/management/commands/reconciliar_grade.py:282`) |
| `CelulaDia.ata` | `ata_do_dia` produz `tipo_dia`, lâmpadas e demais fatos; `lavrar_veredito` persiste pela porta. A chave `precedencia` tem operação própria na mesma porta. (`app/ponto/services/cartorio.py:558`; `app/ponto/services/cartorio.py:566`; `app/ponto/portas/celula.py:725`; `app/ponto/portas/celula.py:732`; `app/ponto/portas/celula.py:775`) | `dia_sem_trabalho` lê `tipo_dia`; `_celula_furada` lê lâmpadas e órfãs; `premissa_regularizada` consulta esses julgamentos. (`app/chamados/juizes.py:407`; `app/chamados/juizes.py:454`; `app/chamados/juizes.py:457`; `app/chamados/juizes.py:459`; `app/chamados/models.py:1232`; `app/chamados/models.py:1247`) |
| Chave de `celulas_do_periodo` | O carregador constrói `(c.colaborador_id, c.data)`. (`app/escala/alimentacao.py:42`) | O comando carrega o mapa, alimenta `vinculo_do_dia` e consulta a mesma tupla no novo filtro. (`app/ponto/management/commands/reconciliar_grade.py:189`; `app/ponto/management/commands/reconciliar_grade.py:196`; `app/ponto/management/commands/reconciliar_grade.py:282`) |
| `plan` / `RETRACT_INDEFINIDO` | `handle` cria o `Counter`; incrementa `RETRACT_INDEFINIDO` quando todas as datas estão em `mortos` e `_indef` não está vazio. O incremento antecede o gate do catálogo. (`app/ponto/management/commands/reconciliar_grade.py:113`; `app/ponto/management/commands/reconciliar_grade.py:292`; `app/ponto/management/commands/reconciliar_grade.py:294`; `app/ponto/management/commands/reconciliar_grade.py:298`) | A saída imprime o plano; os testes novos procuram a presença/ausência do rótulo. (`app/ponto/management/commands/reconciliar_grade.py:318`; `app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:64`; `app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:74`) |
| `retracts` | Lista criada no comando; recebe candidatos após `premissa_morta`, antes da execução de `retratar`. (`app/ponto/management/commands/reconciliar_grade.py:113`; `app/ponto/management/commands/reconciliar_grade.py:298`; `app/ponto/management/commands/reconciliar_grade.py:304`) | Alimenta a listagem e `divergencia_grade_cartorio`. O script de placar extrai posteriormente esse contador da saída registrada. (`app/ponto/management/commands/reconciliar_grade.py:324`; `app/ponto/management/commands/reconciliar_grade.py:337`; `bin/placar_code.sh:528`; `bin/placar_code.sh:530`) |
| `para` | É argumento da consulta: o coletor e o helper têm default `cobranca`; o fechamento exige `folha`; a recursão agora preserva a escolha. (`app/chamados/models.py:1200`; `app/chamados/juizes.py:411`; `app/chamados/models.py:699`; `app/chamados/juizes.py:448`) | `ausencia_cobre` aplica `_pelo_efeito`, separando aprovada de não rejeitada. (`app/ponto/turnos.py:2525`; `app/ponto/turnos.py:2487`; `app/ponto/turnos.py:2489`) |

A projeção da ata do julgamento em curso é uma cópia em memória, distinta da persistência: `celula_do_dia_agora` passa pela projeção e a porta copia a instância antes de sobrepor a ata. As alterações diretas de `trabalha`/`ata` nos testes são alimentação de fixture. (`app/chamados/juizes.py:378`; `app/chamados/juizes.py:394`; `app/ponto/portas/celula.py:678`; `app/ponto/portas/celula.py:680`; `app/chamados/tests/test_f1c4_3_none_cobra.py:83`)

**3. Mordida dos testes** [nao verificado]

| Teste/grupo | Avaliação | [nao verificado]
|---|---|
| Dois RED unitários de `None` | **Mordem** voltar a `is not True`: o predicado antigo responde `True`, contrariando ambos. (`app/chamados/tests/test_f1c4_3_none_cobra.py:39`; `app/chamados/tests/test_f1c4_3_none_cobra.py:42`; `app/chamados/juizes.py:405`) |
| Unitários da cláusula da ata | **Mordem** retirar a cláusula: usam `trabalha=True` com ata de folga/ausência. Não há caso explícito de `tipo_dia='falta'`. (`app/chamados/tests/test_f1c4_3_none_cobra.py:45`) |
| Unitário de `False` | **Morde isoladamente** retirar `trabalha is False`: usa ata de trabalho e ata sem tipo. (`app/chamados/tests/test_f1c4_3_none_cobra.py:50`) |
| Unitário de `True` | Controla respostas indevidamente constantes ou que calem um dia de trabalho sem causa. (`app/chamados/tests/test_f1c4_3_none_cobra.py:54`) |
| RED `_celula_furada(None)` | **Morde** o predicado antigo: ele desviaria para véspera inexistente e retornaria `False`; o novo encontra a lâmpada apagada e retorna `True`. (`app/chamados/tests/test_f1c4_3_none_cobra.py:93`; `app/chamados/juizes.py:437`; `app/chamados/juizes.py:448`; `app/chamados/juizes.py:457`) |
| Controle `_celula_furada(False, 'folga')` | **Não isola a cláusula `False`**: mesmo removendo-a, a ata de folga mantém o resultado. Essa mordida está nos unitários acima. (`app/chamados/tests/test_f1c4_3_none_cobra.py:97`; `app/chamados/juizes.py:408`) |
| Controle `_celula_furada(True, 'folga')` | **Morde** retirar a cláusula da ata: sem ela, a lâmpada apagada acusaria furo. (`app/chamados/tests/test_f1c4_3_none_cobra.py:101`; `app/chamados/juizes.py:457`) |
| RED da premissa em `None` | **Morde** o predicado antigo já na absolvição direta; com o novo, cai na leitura do furo. (`app/chamados/tests/test_f1c4_3_none_cobra.py:105`; `app/chamados/models.py:1232`; `app/chamados/models.py:1247`) |
| Controle da premissa com ata de folga | **Morde** retirar a cláusula da ata, pois o dia contém lâmpada apagada. (`app/chamados/tests/test_f1c4_3_none_cobra.py:111`; `app/chamados/models.py:1232`; `app/chamados/juizes.py:457`) |
| RED da véspera para `folha` | **Morde retirar `para=para`**: no ensaio das funções em memória, a mutação troca a resposta de `True` para `False`, pela cobertura da pendente no default `cobranca`. (`app/chamados/tests/test_f1c4_3_none_cobra.py:117`; `app/chamados/tests/test_f1c4_3_none_cobra.py:131`; `app/chamados/juizes.py:448`; `app/ponto/turnos.py:2489`) |
| Controle da véspera para `cobranca` | Confirma a suspensão pela pendente, mas **sozinho não morde** retirar o argumento: o default já é cobrança. (`app/chamados/tests/test_f1c4_3_none_cobra.py:133`; `app/chamados/juizes.py:411`) |
| Dois RED do reconciliador | **Pelo fluxo anterior fornecido no diff, ficam vermelhos**: `_indef` incluiria ONTEM, gerando `RETRACT_INDEFINIDO` e divergência 1. Eles exigem ausência desse rótulo e divergência 0. (`app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:61`; `app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:67`; `app/ponto/management/commands/reconciliar_grade.py:294`; `app/ponto/management/commands/reconciliar_grade.py:304`; `app/ponto/management/commands/reconciliar_grade.py:337`) |
| MORDE do reconciliador com ata de folga | **Morde** zerar todos os indefinidos ou consultar chave errada. **Não prova retratação efetiva**: roda sem apply e verifica contador anterior à porta. (`app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:58`; `app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:72`; `app/ponto/management/commands/reconciliar_grade.py:294`; `app/ponto/management/commands/reconciliar_grade.py:309`) |
| A2 alterado | **Morde a tentativa indevida**, mesmo se o gate impedir fechar: exige zero chamadas ao `retratar` real envolvido por `wraps`. Verificar somente status vivo seria insuficiente. (`app/ponto/tests/test_a2_marco_morto.py:47`; `app/ponto/tests/test_a2_marco_morto.py:52`; `app/ponto/tests/test_a2_marco_morto.py:54`) |

Os RED do reconciliador não estão vazios por falta de batidas: o chamado põe o colaborador no universo, a fixture fornece vínculo semanal de sete dias e a grade marca o dia como indefinido. (`app/ponto/management/commands/reconciliar_grade.py:95`; `app/ponto/management/commands/reconciliar_grade.py:106`; `app/chamados/tests/_vinculo_do_dia.py:18`; `app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:33`)

A fixture de ausência merece atenção: criar `Ausencia` dispara julgamento síncrono, podendo substituir a ata preparada antes. Neste caso, pendente não dispensa lâmpadas e o overlay usa o efeito folha; faltam asserts após essa criação para explicitar as precondições, mas o código não demonstra um RED falso por ausência de sinal. (`app/chamados/tests/test_f1c4_3_none_cobra.py:129`; `app/ponto/signals.py:143`; `app/ponto/signals.py:93`; `app/ponto/services/cartorio.py:609`; `app/escala/utils.py:1148`)

**Execução integral da suíte Django e execução efetiva na árvore Git antiga: `[nao verificado]`.** [nao verificado]

**4. Leitores de produção restantes fora da fila informada**

**Sim: o caminho mais forte é leitor da célula → supra → retratação.** Sem `tipo_dia` na ata, `trabalha=None` produz `indefinido`; sem previsto lavrado, produz zero. O supra aceita esse tipo como “dia sem trabalho”, porque `SEM_JURISDICAO` exclui apenas `fora_vinculo` e `sem_celula`. (`app/escala/services/leitor_celula.py:344`; `app/escala/services/leitor_celula.py:353`; `app/ponto/supra_juiz.py:46`; `app/ponto/supra_juiz.py:441`)

Esse resultado chega ao ramo que tenta `retratar` com `--executar`, declarado no cron. O gate final pode impedir o fechamento, mas a classificação e a tentativa indevida permanecem. (`app/ponto/management/commands/supra_juiz.py:206`; `app/ponto/management/commands/supra_juiz.py:243`; `app/ponto/management/commands/supra_juiz.py:248`; `app/config/crons.py:580`; `app/chamados/models.py:699`)

Também restam silenciamentos de emissão por `None`, embora nem todos o rotulem explicitamente como folga:

- `processar_alertas_avancados` usa `not eh_dia_trabalho`, pulando `None` antes do caminho de criação/escalonamento de `turno_aberto_24h`. (`app/ponto/management/commands/processar_alertas_avancados.py:49`; `app/ponto/management/commands/processar_alertas_avancados.py:91`; `app/ponto/management/commands/processar_alertas_avancados.py:121`)
- O cartório exige `is True` para emitir furo total e rejeita `is not True` no leitor de furo parcial, antes de consultar os marcos apagados. (`app/ponto/services/cartorio.py:1459`; `app/ponto/services/cartorio.py:1469`; `app/ponto/services/cartorio.py:1558`; `app/ponto/services/cartorio.py:1574`)
- `emitir_furo_retroativo` conta `None` como limbo e pula o caminho de emissão. (`app/ponto/management/commands/emitir_furo_retroativo.py:127`; `app/ponto/management/commands/emitir_furo_retroativo.py:142`)

Esses consumidores recebem o triestado preservado pela autoridade; o colapso acontece depois dela. (`app/escala/models.py:1072`)

**Ocorrências, quantidades e fechamentos efetivos desses casos em produção: `[nao verificado]`.**

## 31/10 08:25 · sessao 8fad8251d37b · - · - · origem=Ronald (tmux)

Correção: o controle `_celula_furada(False,'folga')` **não** morde remover a cláusula `trabalha is False`, pois `ata.tipo_dia='folga'` continua tornando o predicado verdadeiro; os unitários com `False/'trabalho'` e `False/sem tipo` é que isolam essa cláusula. `app/chamados/tests/test_f1c4_3_none_cobra.py:50–52,97–99`; `app/chamados/juizes.py:408`.

Contraexemplo restante do vigia, deduzido do código: célula `None`, ata trabalho apagada, ausência pendente no próprio D, vínculo válido e chamado único sem outras causas de absolvição. `_indef` fica vazio, porém `_coberto(..., para='cobranca')` inclui D em `mortos`; `_lei_cala_no_dia` também cala pela pendente. `app/ponto/management/commands/reconciliar_grade.py:266–267,281–291,401–403`; `app/ponto/marcos.py:166–171`; `app/escala/servico_jornada.py:342–345`.

O plano incrementa `RETRACT` e acrescenta `retracts` antes da porta. A porta pergunta `premissa_regularizada(para='folha')`, que ignora a pendente e vê a lâmpada apagada; sem exceção de pai/irmão, recusa fechar. O retorno `False` de `retratar` é ignorado e o vigia continua contando `len(retracts)=1`. `app/ponto/management/commands/reconciliar_grade.py:303–309,337–342`; `app/chamados/models.py:699,709–724,1232–1249`; `app/chamados/juizes.py:426–427,457–458`; `app/chamados/reconciliador.py:524–526`.

Os novos testes de reconciliar não criam ausência, portanto deixam esse caminho descoberto. `app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:48–58`. Ocorrência e quantidade em produção: `[nao verificado]`.

## 31/10 08:24 · sessao 8fad8251d37b · - · - · origem=Ronald (tmux)

Auditoria estática + AST puro em memória; execução da suíte Django e da árvore Git antiga: `[nao verificado]`. [nao verificado]

| Teste/grupo | Mordida demonstrável pelo código | [nao verificado]
|---|---|
| `JuizDiaSemTrabalhoTest` | Os dois RED falham com `trabalha is not True`: `None` retorna `True` em vez de `False`. Os controles de cláusula 2, `False` e `True` distinguem apagar cada cláusula e predicados constantes. `app/chamados/tests/test_f1c4_3_none_cobra.py:38–56`; predicado atual `app/chamados/juizes.py:407–408`. |
| `_celula_furada(None, ata trabalho apagada)` | Morde a volta do predicado antigo: este recurse para véspera inexistente e termina `False`, quando o teste exige `True`. Atual encontra a lâmpada apagada. `app/chamados/tests/test_f1c4_3_none_cobra.py:93–95`; `app/chamados/juizes.py:443–458`. |
| Controles `_celula_furada` folga / ata folga | Morderiam remover respectivamente a cláusula `False` e a cláusula da ata: a APAGADA do próprio dia devolveria `True`. Entretanto a véspera não existe: esses dois controles não demonstram seleção correta do dia noturno. `app/chamados/tests/test_f1c4_3_none_cobra.py:97–103`; `app/chamados/juizes.py:443–449`. |
| `premissa_regularizada(None)` | Morde o predicado antigo já na cláusula de dia sem trabalho; com o novo, sem marcos nomeados, cai em `_celula_furada`, que retorna `True`, e a premissa não regulariza. `app/chamados/tests/test_f1c4_3_none_cobra.py:85–90,105–109`; `app/chamados/models.py:1232–1249`; `app/ponto/services/lastro.py:110–114,170–172`. |
| Controle premissa ata folga | Morde retirar cláusula 2: com ela regulariza diretamente; sem ela chega à lâmpada apagada e não regulariza. `app/chamados/tests/test_f1c4_3_none_cobra.py:111–115`; `app/chamados/models.py:1232–1249`. |
| RED `para='folha'` | **Sim, morde a retirada de `para=para`.** AST das funções reais, com alimentação em memória e ausência no contrato de `_pelo_efeito`, produziu atual `folha=True/cobranca=False`; mutação sem keyword `folha=False/cobranca=False`. Teste e recursão: `app/chamados/tests/test_f1c4_3_none_cobra.py:117–131`; `app/chamados/juizes.py:426–427,448`; diferença de efeito: `app/ponto/turnos.py:2487–2490`. |
| Controle véspera cobrança | Confirma suspensão por pendente em cobrança, mas sozinho não morde retirar `para=para`, pois o default já é cobrança. `app/chamados/tests/test_f1c4_3_none_cobra.py:133–143`; `app/chamados/juizes.py:411,448`. |
| Dois RED reconciliar (`ata trabalho` e `sem ata`) | **Na árvore anterior descrita pelo diff, deveriam ficar vermelhos**: `_indef=dts&indefinidos` inclui ONTEM, produz `RETRACT_INDEFINIDO=1` e um elemento em `retracts`, portanto divergência 1. Ambos exigem ausência da primeira string e divergência 0. `app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:61–70`; leitores do resultado `app/ponto/management/commands/reconciliar_grade.py:292–304,337–342`. |
| MORDE ata folga reconciliar | Morde zerar todos os indefinidos, errar a chave para `_d` em vez de `(c.pk,_d)` ou não perguntar ao juiz: desapareceria `RETRACT_INDEFINIDO=1`. Não prova que a porta efetivamente retrataria: roda `apply=False`, e o contador antecede `premissa_morta` e `retratar`. `app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:58,72–74`; `app/ponto/management/commands/reconciliar_grade.py:281–309`. |
| A2 alterado | O `wraps` e a exigência `call_count=0` mordem retornar ao ramo antigo mesmo se a porta barrar o fechamento; verificar só status vivo não morderia essa tentativa. `app/ponto/tests/test_a2_marco_morto.py:47–55`; `app/ponto/management/commands/reconciliar_grade.py:305–309`. |

Não encontrei ausência de sinal que torne os RED de reconciliar triviais: a fixture dá vínculo semanal de sete dias, cria o chamado antes da célula e põe ONTEM na janela congelada. `app/chamados/tests/_vinculo_do_dia.py:15–26`; `app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:25,37,46,52–58`. `com_chamado` põe o colaborador no universo mesmo sem batidas, e `sem_ancora` preenche `indefinidos`. `app/ponto/management/commands/reconciliar_grade.py:95–106,139–150`. `_lei_cala_no_dia` não cala só por `None`: `classificar_falta` testa `is False`, preserva marcos do vínculo e zero batidas retorna falta. `app/ponto/marcos.py:166–176`; `app/escala/servico_jornada.py:303–304,427–435,457–480`.

**Cuidado real da fixture de ausência:** `Ausencia.objects.create` dispara julgamento síncrono e pode sobrescrever a APAGADA montada antes. `app/chamados/tests/test_f1c4_3_none_cobra.py:126–130`; `app/ponto/signals.py:143–147,85–93`; `app/ponto/services/cartorio.py:938–981`. Neste caso o código conserva o sinal relevante: a grade só aplica cobertura `para='folha'`, pendente não cobre nesse efeito e `dispensar_por_ausencia` não acende lâmpadas para pendente. `app/escala/utils.py:1148`; `app/ponto/turnos.py:2487–2488`; `app/ponto/services/cartorio.py:609–612`. A precedência adicional escreve somente `ata['precedencia']`, não substitui `tipo_dia`. `app/ponto/services/cartorio.py:1629–1638`; `app/ponto/portas/celula.py:771–775`. Ainda faltam asserts explícitos após criar ausência para certificar a lâmpada e as duas respostas de cobertura; ausência desses asserts é lacuna de diagnóstico, não prova de RED falso. `app/chamados/tests/test_f1c4_3_none_cobra.py:129–131`.

## 31/10 08:23 · sessao f94190de7f99 · - · - · origem=Ronald (tmux)

Sim: sobra um caminho de retratação que converte **indeterminado em dia sem trabalho**:

- `app/escala/services/leitor_celula.py:344` usa a ata; sem `tipo_dia`, `trabalha=None` produz `'indefinido'` (:345). Sem minutos na ata, produz previsto `0` (:353).
- `app/ponto/supra_juiz.py:441` considera qualquer tipo diferente de `'trabalho'` e fora de `SEM_JURISDICAO` como dia sem trabalho, havendo previsto zero e chamado vivo. `'indefinido'` não pertence às duas exclusões definidas em :46.
- `app/ponto/management/commands/supra_juiz.py:116` consome essa grade, :206 chama o classificador e :243-250 tenta `retratar()` com `--executar`; a trava está desligada (:41). O cron declara precisamente essa execução em `app/config/crons.py:580`.
- A tentativa ainda recebe texto incorreto sobre “dia não de trabalho/fase redeclarada”, de `app/ponto/supra_juiz.py:100`. O fechamento efetivo pode ser recusado pelo gate que consulta `premissa_regularizada(para='folha')`, em `app/chamados/models.py:699`; ocorrência efetiva em produção `[nao verificado]`.

Executei somente em memória o módulo puro `ponto/supra_juiz.py` com dia `'indefinido'`, previsto `0`, um chamado vivo e turno encerrado: retornou `COBRANCA_EM_DIA_SEM_TRABALHO`, exatamente pelo ramo de `app/ponto/supra_juiz.py:441`.

Há também emissores adicionais que calam `None`, fora da lista fornecida:

| Leitor | Efeito demonstrável |
|---|---|
| `app/ponto/management/commands/processar_alertas_avancados.py:49` | `if not ec.eh_dia_trabalho(ontem): continue` colapsa `None` e `False`, antes do caminho que cria/escalona `turno_aberto_24h` (:91-123). Cron declarado em `app/config/crons.py:344`. |
| `app/ponto/services/cartorio.py:1459` | Emissão de furo total exige `is True`; `None` não chega a `_abrir_chamado` (:1469). |
| `app/ponto/services/cartorio.py:1558` | Furo parcial devolve `None` quando o dia não é `True`; o leitor emissor está em :1505. |
| `app/ponto/management/commands/emitir_furo_retroativo.py:127` | `is not True` incrementa `limbo` e pula emissão; chamada ao emissor em :142. É comando sob ordem, conforme `app/config/crons.py:1339`. |

Esses três últimos comprovam silêncio de emissão por previsão desconhecida; não rotulam explicitamente `None` como folga. O colapso booleano direto está no primeiro, e a conversão explícita em **“dia sem trabalho”**, com caminho de retratação, está no supra. Todos recebem o valor original da célula pelo retorno de `app/escala/models.py:1072`.

Não incluir como novo defeito direto de folga:

- `app/chamados/services/feriado_reavaliar.py:34` preserva a incerteza com `continue` quando `era_dia_previsto is None`.
- `app/chamados/detector_anomalias.py:165` distingue `None` de `False` (:168).
- `app/ponto/services/flip_auto.py:212` chama a véspera `None` de “folga real” (:213), mas sua ação imediata é decisão de flip de batida; pertencimento direto ao caminho de cobrança/retratação pedido `[nao verificado]`.

## 31/10 08:23 · sessao b2842f1b8926 · - · - · origem=Ronald (tmux)

Mapeamento dos campos e leitores, somente pela cópia:

| Valor | Escritor/produtor | Leitor relevante |
|---|---|---|
| `CelulaDia.trabalha` | Porta `ponto/portas/celula.py`: regeneração calcula `valor = ec.eh_dia_trabalho_calculado(d)` e cria/altera em `:243`, `:247`, `:260`; geração em `:590`, `:593`, `:612`; reversão em `:532`. | `EscalaColaborador.eh_dia_trabalho` devolve o triestado sem converter em `escala/models.py:1066`, `:1072`; `dia_sem_trabalho` compara estritamente com `False` em `chamados/juizes.py:408`. |
| `CelulaDia.ata` | Conteúdo produzido por `ata_do_dia`: `ponto/services/cartorio.py:558`, `:566`; persistência por `lavrar_veredito`: `ponto/portas/celula.py:697`, `:711`, `:725`, `:732`. A chave `precedencia` tem operação própria: `:767`, `:775`, `:778`. | `dia_sem_trabalho` lê `tipo_dia`: `chamados/juizes.py:407`; `_celula_furada` lê `lampadas`/órfãs em `:454`, `:457`, `:459`; `premissa_regularizada` lê o mesmo juiz em `chamados/models.py:1227`, `:1232`, `:1247`. |
| chave do mapa | `celulas_do_periodo` produz **`(colaborador_id, datetime.date)`**, não data isolada: `escala/alimentacao.py:41`, `:42`. | Novo ramo `_indef` usa corretamente `(c.pk, _d)`: `ponto/management/commands/reconciliar_grade.py:281`, `:282`. |

A ata tem também cópias estritamente em memória: `celula_como_o_julgamento_deixa` copia a instância e sobrepõe a ata em `ponto/portas/celula.py:667`, `:678`, `:680`; a montagem da grade cria cópia para calcular realizado em `escala/utils.py:1826`, `:1827`, `:1837`. Não confundir essas atribuições com persistência fora da porta: ambas declaram/implementam cópias, sem `save` nesses trechos (`ponto/portas/celula.py:668`; `escala/utils.py:1826`).

Leitores adicionais à fila nominal que impedem cobrar `None`:

- `ponto/services/cartorio.py::_marcos_do_furo_parcial` exige `ec.eh_dia_trabalho(data) is True` antes de ler os marcos apagados da ata: `:1558`, `:1559`, `:1574`. Como a autoridade preserva `None`, o cartório descarta a possibilidade de cobrar esse furo parcial (`escala/models.py:1072`).
- `ponto/management/commands/emitir_furo_retroativo.py:127` também exige `is True`, soma `limbo` e pula em `:128`, `:129`; o caminho que emitiria fica em `:142`.
- `ponto/management/commands/processar_alertas_avancados.py:49` usa `not ec.eh_dia_trabalho(ontem)`, portanto `None` pula a análise do turno antes de consultar `TurnoMaterializado` (`:50`, `:58`). A ligação específica com retratação de chamado de furo nesse comando é `[nao verificado]`.
- `ponto/services/flip_auto.py:212` chama a véspera `None` de `'folga real (vespera tambem folga)'` em `:213`; porém essa função propõe etiquetas, e seu contrato diz que não emite chamado (`:165`, `:176`). É derivação errada de folga, mas não provei que pertence ao mesmo caminho direto de cobrança/retratação.

Leitores conferidos que preservam a distinção:

- `classificar_falta` cala apenas `trabalha is False`: `escala/servico_jornada.py:303`.
- Detector de anomalias distingue `None` de `False`: `chamados/detector_anomalias.py:164`, `:165`, `:168`.
- Julgamento de procedência devolve `ambiguo` para `None`: `escala/servico_jornada.py:539`, `:542`, `:544`, `:546`.

O contador do vigia continua baseado nas **intenções** acumuladas, `len(creates) + len(retracts)`, não no resultado da porta: `ponto/management/commands/reconciliar_grade.py:337`. `retracts.append` ocorre antes de `retratar` em `:304`, `:309`; logo eventual recusa da porta não remove a intenção do contador (`:337`).

## 31/10 07:48 · sessao 81e59f67ca4c · E3-STATUS · verificador · origem=Code

**Reprovaria a obra por incoerência entre status e veredito, atualização incompleta por evento e garantia insuficiente dos selos.** Há contraexemplos no código, independentemente de dados de produção: cobertura parcial pode resultar em `furo/ausencia`; cadastro indefinido pode resultar em `indefinida/folga`; uma execução concorrente do lote pode sobrescrever o status de uma decisão humana. (`app/ponto/status_dia.py:71`, `app/ponto/status_dia.py:73`, `app/ponto/services/cartorio.py:733`, `app/ponto/portas/celula.py:753`.)

Ocorrência e quantidade desses casos em produção: **[não verificado]**. Suíte Django, RED histórico e migrations efetivamente aplicadas: **[não verificado]**. As verificações executadas limitaram-se à leitura e à avaliação de funções AST em memória, sem banco e sem criar arquivos. **[não verificado: não há artefato persistido dessa execução]** [nao verificado]

**1. Achados, causa e gravidade**

| Gravidade | Achado e caminho demonstrável |
|---|---|
| **Alta** | **Ausência parcial vira ausência integral no status.** A grade mantém `tipo_dia='ausencia'` e informa separadamente `cobertura_parcial`; o juiz pode acusar o restante como furo. O novo dict descarta essa distinção, e `status_da_lavra` retorna `ausencia` antes de consultar o veredito. Entrada possível: `tipo_dia='ausencia', previsto_min=480, tem_batida=False, veredito='furo'` → status `ausencia`. Contradiz D2 e a própria distinção já existente no juiz. (`app/escala/utils.py:1180`, `app/escala/utils.py:1190`, `app/ponto/supra_juiz.py:279`, `app/escala/models.py:1580`, `app/ponto/status_dia.py:71`, `app/ponto/status_dia.py:103`, `app/docs/ADR/0002-celula-e-o-dia.md:44`.) |
| **Alta** | **Cadastro indefinido pode virar folga.** O caminho de célula de trabalho sem vínculo válido chega à grade sem previsto e acusa `PREVISTO_COM_DIA_INDEFINIDO`, convertido em `indefinida`. O novo status interpreta `tipo_dia='folga'` ou previsto zero como folga antes de testar `indefinida`. Resultado: `indefinida/folga` no mesmo ato. (`app/ponto/supra_juiz.py:168`, `app/ponto/supra_juiz.py:222`, `app/ponto/supra_juiz.py:405`, `app/escala/models.py:1583`, `app/ponto/status_dia.py:73`, `app/ponto/status_dia.py:77`.) |
| **Alta** | **FolgaDia pode apagar a evidência de batida.** O overlay limpa `celulas` e `orfas`. Com uma entrada sem saída, o realizado pode ser zero; o novo extrator então conclui `tem_batida=False` e lavra `folga`, embora exista batida. Isso impede `folga_trabalhada` justamente no caso incompleto. (`app/escala/utils.py:1118`, `app/ponto/turnos.py:731`, `app/ponto/turnos.py:736`, `app/escala/utils.py:1998`, `app/ponto/status_dia.py:101`, `app/ponto/status_dia.py:73`.) |
| **Alta** | **Corrida lote × admin quebra a coerência persistida.** O lote carrega uma instância sem trava. Depois dessa leitura, o admin pode gravar `faltou/admin/falta`; a instância antiga do lote continua com `concorde/cartorio`, calcula `ok` e salva somente status. Resultado possível no banco: `faltou/admin/ok`. O `atomic` protege save+outbox, mas não a leitura e a decisão. (`app/ponto/management/commands/lavrar_status_competencia.py:53`, `app/ponto/services/cartorio.py:733`, `app/ponto/services/veredito_celula.py:330`, `app/ponto/portas/celula.py:753`, `app/ponto/portas/celula.py:760`, `app/ponto/portas/celula.py:639`.) |
| **Alta** | **Evento de feriado não cobre todas as células afetadas.** O receiver chama a reavaliação de chamados; seu universo contém somente `batida_ausente` vivos. Sem chamado elegível, não há rejulgamento indireto. A remoção do feriado não tem receiver. Status já lavrado pode permanecer velho. (`app/chamados/signals.py:265`, `app/chamados/services/feriado_reavaliar.py:17`, `app/chamados/services/feriado_reavaliar.py:76`, `app/chamados/signals.py:261`.) |
| **Alta** | **Retirar a ausência não revoga a decisão humana da célula.** O serviço cria ausência aprovada e lavra `admin`; cancelar a ausência a torna rejeitada e dispara o cartório, mas a porta preserva o veredito humano e recompõe o status a partir dele. Assim, `justificado/ausencia` ou `faltou/falta` sobrevivem à retirada da cobertura. Isso demonstra que cancelar a ausência não implementa desfazer a decisão da célula; uma porta explícita de revogação desse veredito é **[não verificado]**. (`app/ponto/services/veredito_celula.py:315`, `app/ponto/services/veredito_celula.py:330`, `app/ponto/views.py:2724`, `app/ponto/signals.py:147`, `app/ponto/portas/celula.py:738`, `app/ponto/portas/celula.py:694`.) |
| **Média** | **Regeneração não garante relavratura.** O rejulgamento depende de `recalculo`, exclui decisões humanas e absorve falhas. A reversão restaura DNA/trabalho sem relavrar status. O campo novo herda esses caminhos de envelhecimento. (`app/ponto/portas/celula.py:360`, `app/ponto/portas/celula.py:369`, `app/ponto/portas/celula.py:375`, `app/ponto/portas/celula.py:530`, `app/ponto/portas/celula.py:538`.) |
| **Média** | **A impressão não protege todos os fatos novos do status.** `isento_ponto` é lido na nova lavra, mas não integra a impressão; a própria declaração admite a lacuna. Uma mudança de isenção pode encontrar impressão igual e ser pulada, mantendo status/rótulo antigos. A falha é declarada como dívida da E2, portanto não deve ser apresentada como cobertura completa do hash nesta E3. (`app/ponto/services/cartorio.py:734`, `app/ponto/services/cartorio.py:324`, `app/ponto/services/cartorio.py:1159`, `app/docs/ADR/0002-celula-e-o-dia.md:57`.) |

A proteção contra falta automática existe no caso comum: os vereditos acusadores são mapeados para `irregular`, não `falta`; isso acompanha L-039. Os problemas acima são de perda de fatos ou precedência da regra, não desse mapeamento comum. (`app/ponto/status_dia.py:42`, `app/ponto/status_dia.py:81`, `app/docs/LEIS.md:87`.)

**2. Quem escreve**

Nas referências seguintes, os campos são os de `CelulaDia`; não incluí como escritores persistentes os homônimos de outros objetos nem as cópias usadas durante julgamento. A cópia externa de `ata`, por exemplo, é explicitamente uma cópia em memória. (`app/escala/utils.py:1826`.)

| Campo/valor | Escritores e persistência encontrados |
|---|---|
| `status_dia`, `status_rotulo` | `_aplicar_status`, chamado pela lavra; persistência por `salvar_e_emitir`, diretamente ou junto de `carimbar`. (`app/ponto/portas/celula.py:705`, `app/ponto/portas/celula.py:739`, `app/ponto/portas/celula.py:754`, `app/ponto/portas/celula.py:773`, `app/ponto/portas/celula.py:640`, `app/ponto/portas/celula.py:807`.) |
| `veredito`, `veredito_via`, `veredito_em` | Lavra completa; o método do model apenas delega. (`app/ponto/portas/celula.py:770`, `app/ponto/portas/celula.py:771`, `app/ponto/portas/celula.py:772`, `app/escala/models.py:1795`.) |
| `protestos` | Lavra completa; cartório substitui pelos códigos recebidos, admin conserva os anteriores. (`app/ponto/portas/celula.py:746`, `app/ponto/portas/celula.py:751`, `app/ponto/portas/celula.py:769`.) |
| `ata` | `lavrar_veredito` e **também** `carimbar_precedencia`, ambas dentro da porta. Portanto, “só lavrar_veredito escreve ata” seria uma descrição incorreta. (`app/ponto/portas/celula.py:741`, `app/ponto/portas/celula.py:768`, `app/ponto/portas/celula.py:820`, `app/ponto/portas/celula.py:822`.) |
| `fatos_do_status` | Nasce em `status_dia.fatos_do_status`; cartório o produz nas três chamadas e a porta o expande em `status_da_lavra`. O wrapper do model o repassa. (`app/ponto/status_dia.py:95`, `app/ponto/status_dia.py:103`, `app/ponto/services/cartorio.py:734`, `app/ponto/services/cartorio.py:1045`, `app/ponto/services/cartorio.py:1557`, `app/escala/models.py:1796`, `app/ponto/portas/celula.py:698`.) |

Os chamadores de produção da lavra são o complemento de status, os dois ramos de julgamento do cartório e a decisão humana. (`app/ponto/services/cartorio.py:733`, `app/ponto/services/cartorio.py:1041`, `app/ponto/services/cartorio.py:1556`, `app/ponto/services/veredito_celula.py:330`.)

Os caminhos especiais pedidos no censo ficam assim:

- **Regeneração:** escreve DNA, trabalho, geradora e denúncia; não limpa nem recalcula status nesses saves. O eventual rejulgamento é uma etapa posterior e condicional. (`app/ponto/portas/celula.py:233`, `app/ponto/portas/celula.py:264`, `app/ponto/portas/celula.py:360`.)
- **Geração comum:** cria células sem preencher os campos julgados; enriquecimento e atualização usam `update_fields` de DNA/trabalho/geradora. (`app/ponto/portas/celula.py:593`, `app/ponto/portas/celula.py:605`, `app/ponto/portas/celula.py:615`.)
- **`bulk_update` da porta:** grava somente ausência, entrada, saída e instante dos insumos, não os campos examinados aqui. (`app/ponto/portas/celula.py:843`.)
- **Migrations:** 0036 adiciona protestos/veredito/via/instante; 0037 adiciona ata; 0044 adiciona status/rótulo. Esses `AddField` deixam os valores nulos; não fazem a relavratura de dados. (`app/escala/migrations/0036_b5_celula_veredito.py:13`, `app/escala/migrations/0037_hx_ata_celula.py:13`, `app/escala/migrations/0044_celuladia_status_dia_celuladia_status_rotulo.py:13`.)
- **Escrita direta nos testes novos:** há `.update(status_dia=None, status_rotulo=None)` nas duas fixtures de backfill. (`app/ponto/tests/test_e3_status_lavrado.py:197`, `app/ponto/tests/test_e3_status_lavrado.py:210`.)
- **Segundo escritor persistente de produção fora da porta, `save()` integral de CelulaDia, fixture de command ou migration de dados escrevendo esses valores:** nenhum caso concreto foi identificado; completude diante de aliases, SQL construído e dicionários dinâmicos é **[não verificado]**. O selo existente também não consegue certificar essa completude. (`app/ponto/tests/test_selo_veredito_so_pela_porta.py:28`, `app/ponto/tests/test_selo_veredito_so_pela_porta.py:114`, `app/ponto/tests/test_selo_veredito_so_pela_porta.py:173`.)

**3. Quem lê**

O índice abaixo cobre os acessos de produção identificados, incluindo consultas ORM e comandos. **Não certifico como completo o inventário literal de todos os leitores/assertions de testes da árvore: [não verificado].** [nao verificado]

| Campo | Leitores identificados — prefixo `app/` |
|---|---|
| `status_dia` | `ponto/portas/celula.py:697`, `:703`, `:723`, `:780`; `ponto/services/cartorio.py:726`; `ponto/status_dia.py:131`, `:132`. |
| `status_rotulo` | `ponto/portas/celula.py:697`, `:703`; `ponto/status_dia.py:132`. |
| `veredito_em` | `colaboradores/services/calendario.py:510`; `ponto/services/veredito_celula.py:107`; `chamados/veredito_lavrado.py:153`; `chamados/services/vigia_fio.py:361`; `ponto/management/commands/celula_veredito_velho.py:107`. |
| `protestos` | `folha/export.py:1147`, `:1228`; `chamados/reconciliador.py:838`; `ponto/services/cartorio.py:733`, `:863`, `:868`; `ponto/portas/celula.py:722`, `:751`, `:753`, `:779`; `ponto/management/commands/celula_veredito_velho.py:108`; `ponto/services/furos_sem_cobranca.py:57`; `escala/services/cadastro_realidade.py:391`. |
| Dict `fatos_do_status` | Argumento recebido/repassado pelo model e pela porta; `_status_alvo` decide entre admin, ausência de fatos e expansão dos fatos; `status_da_lavra` consome `tipo_dia`, `tem_batida`, `previsto_min`, `isento` e veredito. (`escala/models.py:1788`, `:1796`; `ponto/portas/celula.py:686`, `:694`, `:696`, `:698`, `:739`, `:752`; `ponto/status_dia.py:55`.) |

Leitores de **`veredito`**, agrupados por área:

- **Folha/relatórios/pautas:** `app/folha/export.py:1204`, `:1215`, `:1236`, `:1237`; `app/folha/previa.py:94`; `app/folha/services/vigia_pausa.py:157`; `app/relatorios/dia_da_frota.py:51`, `:54`, `:60`; `app/relatorios/furos.py:160`; `app/pautas/services.py:415`.
- **Colaboradores:** `app/colaboradores/services/calendario.py:510`; `app/colaboradores/services/pos_vinculo.py:61`; `app/colaboradores/management/commands/corrigir_escala_retroativa.py:123`.
- **Escala:** `app/escala/services/leitor_celula.py:359`, `:365`; `app/escala/services/propositor.py:472`; `app/escala/services/cadastro_realidade.py:142`, `:313`, `:396`.
- **Ponto:** `app/ponto/marcos.py:155`; `app/ponto/services/cartorio.py:733`, `:999`, `:1155`; `app/ponto/services/veredito_celula.py:107`; `app/ponto/services/vigia_ausencia_veredito.py:42`, `:56`; `app/ponto/services/diagnostico_escala.py:177`; `app/ponto/services/furos_sem_cobranca.py:61`; `app/ponto/services/furos_vetados.py:61`; `app/ponto/services/pautas_dos_discordantes.py:69`; `app/ponto/portas/celula.py:365`, `:657`, `:697`, `:722`, `:739`, `:753`.
- **Comandos de ponto:** `app/ponto/management/commands/recusar_ponta_pequena.py:145`; `app/ponto/management/commands/celula_veredito_velho.py:88`, `:106`, `:125`; `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:58`, `:62`.
- **Chamados, núcleo:** `app/chamados/reconciliador.py:584`, `:963`, `:982`; `app/chamados/models.py:1148`, `:1152`, `:2622`, `:2623`, `:2624`; `app/chamados/juizes.py:784`, `:1020`, `:1036`, `:1072`, `:1074`.
- **Serviços de chamados:** `app/chamados/services/fabrica_da_celula.py:86`; `app/chamados/services/silencio_ausencia.py:40`; `app/chamados/services/furo_sem_canal.py:71`; `app/chamados/services/vigia_fio.py:361`; `app/chamados/services/resposta_tardia.py:45`, `:56`; `app/chamados/services/limbo_folgas.py:194`; `app/chamados/services/disputa_emissao.py:705`; `app/chamados/services/adesao.py:32`.
- **Comandos de chamados:** `app/chamados/management/commands/disparar_perguntas_competencia.py:120`; `app/chamados/management/commands/auditar_invariantes_chamados.py:197`, `:376`, `:1434`, `:1443`, `:1591`, `:1598`, `:1603`, `:1710`, `:1798`.
- **Fora de `app/`:** `bin/r5_idempotencia_frota.py:56`; projeção SQL de `bin/sombra_incremental.sh:35`.

Leitores de **`ata`**:

- **Folha/relatórios/pautas:** `app/folha/validacao_pdf.py:44`, `:47`; `app/folha/previa.py:83`; `app/folha/services/sem_furo.py:20`, `:22`; `app/relatorios/cartao_pela_celula.py:286`, `:287`; `app/relatorios/dia_da_frota.py:61`; `app/pautas/services.py:414`.
- **Ponto, núcleo/serviços:** `app/ponto/juiz_batida.py:157`; `app/ponto/worklist.py:22`; `app/ponto/services/lastro.py:89`, `:113`, `:128`, `:149`; `app/ponto/services/vigia_entrada.py:78`, `:80`, `:123`; `app/ponto/services/cartorio.py:320`, `:1184`; `app/ponto/services/dia_do_colab.py:29`; `app/ponto/services/cluster_espurio.py:23`; `app/ponto/services/vigia_pareador_celula.py:69`; `app/ponto/services/esmeril_espelho.py:125`, `:129`; `app/ponto/services/diagnostico_escala.py:156`; `app/ponto/portas/celula.py:731`, `:735`, `:816`, `:818`.
- **Comandos de ponto:** `app/ponto/management/commands/vigia_b5_ausencia.py:79`; `app/ponto/management/commands/tripwire_celulas.py:258`, `:338`, `:341`; `app/ponto/management/commands/backfill_b5_insumos.py:73`; `app/ponto/management/commands/curar_residuo_bug115.py:45`, `:145`; `app/ponto/management/commands/celula_veredito_velho.py:108`; `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:61`; `app/ponto/management/commands/detectar_intervalo_ausente.py:46`.
- **Demais áreas:** `app/escala/services/leitor_celula.py:331`; `app/chamados/reconciliador.py:673`; `app/chamados/models.py:2621`; `app/chamados/juizes.py:400`, `:435`, `:879`, `:999`; `app/chamados/services/materializacao.py:156`; `app/colaboradores/services/feriado_do_posto.py:96`.
- **Fora de `app/`:** `bin/r5_idempotencia_frota.py:56`; `bin/snapshot_o118_col221.py:18`.

Os templates que usam `dia.status_rotulo` continuam recebendo o dict produzido pelos leitores antigos; isso não constitui migração para a coluna nova. O calendário constrói novamente esse rótulo, e o ADR congela os leitores até E4. (`app/colaboradores/services/calendario.py:884`, `app/templates/colaboradores/partials/_calendario_grade.html:99`, `app/templates/escala/minha_escala.html:75`, `app/docs/ADR/0002-celula-e-o-dia.md:71`.)

**4. Idempotência, trilha e outbox**

| Situação | Resultado do código |
|---|---|
| Mesma lavra sequencial, com ata, veredito, via, protestos e status iguais | Retorna `[]`; não salva nem acrescenta evento da porta. (`app/ponto/portas/celula.py:735`, `app/ponto/portas/celula.py:753`, `app/ponto/portas/celula.py:755`.) |
| Somente status/rótulo muda | Salva esses campos, não move `veredito_em`, registra auditoria no ramo comum e não emite `EventoCelula`. (`app/ponto/portas/celula.py:754`, `:760`, `:762`, `:766`; `app/ponto/portas/celula.py:623`.) |
| Ata muda, mesmo com mesmo veredito | Entra na lavra completa e move `veredito_em`. (`app/ponto/portas/celula.py:735`, `:753`, `:772`.) |
| Cartório sobre célula decidida pelo admin | Pode mudar ata/status e retorna antes da auditoria da lavra. **Falha de trilha, média.** (`app/ponto/portas/celula.py:738`, `:743`, `:744`.) |
| Serviço humano | A porta registra a ação e o serviço registra novamente `veredito_celula`. Portanto, “uma linha total” não vale nesse caminho. **Falha de trilha, média.** (`app/ponto/services/veredito_celula.py:330`, `:334`; `app/ponto/portas/celula.py:777`; `app/core/auditoria.py:22`.) |
| `save=False` | A auditoria ocorre mesmo sem persistência naquele momento; o save vem depois por `carimbar`. Uma falha entre os atos pode deixar trilha de lavra não persistida. **Média; mecanismo anterior ampliado para status.** (`app/ponto/portas/celula.py:774`, `:777`, `:807`.) |
| Só rótulo muda | A comparação detecta a mudança, mas a trilha não contém rótulo anterior/novo; pode registrar `ok → ok`. **Média.** (`app/ponto/portas/celula.py:703`, `:722`, `:764`.) |

Além disso, `evento` é best effort: uma escrita concluída não garante sequer uma linha de `LogAuditoria`. (`app/core/observ.py:105`, `app/core/observ.py:133`, `app/core/observ.py:139`.)

O save e o `EventoCelula` são atômicos entre si. Quem acorda o consumidor é a escrita de `veredito` ou `ata`; o consumidor retrata chamados absolvidos e encerra perguntas órfãs pela lâmpada. **Esse outbox é de consumo da célula, não o gatilho que rejulga todos os seus insumos.** Batidas e ausências chegam ao cartório pelos signals. (`app/ponto/portas/celula.py:623`, `:638`, `:639`; `app/chamados/reconciliador.py:1304`, `:1307`; `app/ponto/signals.py:135`, `:147`.)

O teste de idempotência novo verifica que uma segunda execução do cartório não aumenta a contagem existente; não exige “uma linha no primeiro ato”, não cobre o serviço humano e não conta `EventoCelula`. (`app/ponto/tests/test_e3_status_lavrado.py:172`, `:177`, `:182`.)

**5. Command e encerramento do dia**

- **Entregue:** se `competencia_entregue` responde verdadeiro na consulta inicial, o command retorna antes de escrever. Porém não há nova conferência nem serialização com uma entrega concorrente durante o lote; a garantia absoluta “nunca escreve após entrega” não está implementada nesse fluxo. (`app/ponto/management/commands/lavrar_status_competencia.py:40`, `:42`, `:53`, `:55`.)
- **L-092/L-113:** o bloqueio atende à declaração específica do D5. A justificativa genérica “exportada é intocável pela L-092” precisa reconhecer que L-113 alterou essa lei; isso não transforma, por si só, o backfill desta obra em uma autorização de correção de exportada. (`app/docs/ADR/0002-celula-e-o-dia.md:64`, `app/docs/LEIS.md:148`.)
- **Rejulgar/emitir/fechar:** no caminho normal de complemento, o corpo desvia antes do julgamento e a função retorna antes da segunda passada/fábrica. Não abre nem reconcilia chamados diretamente. (`app/ponto/services/cartorio.py:1000`, `:1156`, `:776`.)
- **Condição não checada:** a elegibilidade exige veredito, mas não uma `veredito_via` coerente. Se existir célula com veredito e via nula, a chamada com `via='cartorio'` pode entrar na lavra completa, mover `veredito_em` e emitir outbox. Existência dessas linhas em produção: **[não verificado]**. (`app/ponto/services/cartorio.py:999`, `:1155`, `:733`; `app/ponto/portas/celula.py:753`, `:772`, `:773`.)
- **Idempotência:** depois de preencher status, o complemento pula a célula. Isso torna a segunda passada estável, mas também faz o lote ignorar status já preenchido e errado ou rótulo faltante; não é um

[truncado em 20000 de 25252 caracteres]

## 31/10 07:45 · sessao 6507aa010fd3 · - · - · origem=Ronald (tmux)

Achados de selo confirmados por execução em memória, sem Django/banco:

- **Alta — `fatos_do_status=None` passa no selo.** O guard verifica apenas a presença do keyword; não prova o requisito de lavrar status junto do veredito (`app/ponto/tests/test_selo_veredito_so_pela_porta.py:166`). A porta aceita `None` e pode conservar status anterior ou deixar status nulo (`app/ponto/portas/celula.py:695`).
- **Alta — formas inteiras de segundo escritor passam verdes:** `create`, `update_or_create(defaults=...)`, `bulk_create`, queryset em alias e atribuição em tupla seguida de `save()`. O detector limita operações/receptores e só reconhece atribuição cujo alvo imediato é `Attribute` (`app/ponto/tests/test_selo_veredito_so_pela_porta.py:85`, `app/ponto/tests/test_selo_veredito_so_pela_porta.py:117`).
- **Média — escrita isolada de `veredito_em`/`protestos` por `save(update_fields=...)` passa.** Esses campos estão em `CAMPOS`, porém ausentes de `CAMPOS_DO_SAVE` (`app/ponto/tests/test_selo_veredito_so_pela_porta.py:16`, `app/ponto/tests/test_selo_veredito_so_pela_porta.py:17`).
- **Média — migrations e testes não participam da garantia; dentro da porta só se fiscaliza `save()` sem `update_fields`.** Os exemplos novos mordem formas literais específicas, sem garantir escritor único universal (`app/ponto/tests/test_selo_veredito_so_pela_porta.py:112`, `app/ponto/tests/test_selo_veredito_so_pela_porta.py:173`, `app/ponto/tests/test_selo_veredito_so_pela_porta.py:259`).

Censo de produção e índice de 114 arquivos candidatos a fixtures de testes enviados nas mensagens anteriores. O índice de fixtures usa heurísticas AST e não deve ser apresentado como prova exaustiva de tipos; inventário completo das leituras/assertions de testes [não verificado]. Nenhuma escrita direta persistida desses campos fora da porta foi encontrada no censo estático; completude diante de aliases/dicionários dinâmicos [não verificado]. [nao verificado]

Correção ao meu primeiro levantamento: `protestos` nasce **NULL**, como os demais campos antigos; não nasce `[]` (`app/escala/models.py:1741`, `app/escala/migrations/0036_b5_celula_veredito.py:16`).

## 31/10 07:43 · sessao 51914975b57e · - · - · origem=Ronald (tmux)

Achados demonstráveis na cópia:

- **Alta — D5 pode deixar status incompatível com decisão admin por concorrência.** O command materializa células sem trava em `app/ponto/management/commands/lavrar_status_competencia.py:53`; `_completar_status` usa o veredito daquela instância em `app/ponto/services/cartorio.py:733`. Se o admin gravar `faltou/admin/falta` depois da leitura, a instância antiga ainda pode gerar `ok` e salvar somente status em `app/ponto/portas/celula.py:753` e `app/ponto/portas/celula.py:760`. O `atomic` envolve o save, sem releitura ou comparação do estado persistido, em `app/ponto/portas/celula.py:639`. Resultado possível: `faltou/admin` junto de `ok`.

- **Alta — retirar a ausência da decisão admin não desfaz a palavra da célula.** O service cria ausência aprovada em `app/ponto/services/veredito_celula.py:315` e lavra admin em `app/ponto/services/veredito_celula.py:330`. Cancelar a ausência muda seu estado para rejeitada em `app/ponto/views.py:2724`; o signal rejulga em `app/ponto/signals.py:147`, mas a porta preserva a decisão admin em `app/ponto/portas/celula.py:738` e deriva novamente o status dela em `app/ponto/portas/celula.py:694`. Assim, `justificado/ausencia` ou `faltou/falta` sobrevivem à retirada da cobertura. Porta explícita de revogação desse veredito: **[não verificado]**.

- **Alta — feriado não invalida todas as células afetadas.** O evento de feriado chama somente reavaliação de chamados em `app/chamados/signals.py:265`; o universo contém apenas `batida_ausente` vivos em `app/chamados/services/feriado_reavaliar.py:17`. Sem chamado elegível, não ocorre o rejulgamento indireto. Remoção do feriado não tem receiver, explicitamente documentado em `app/chamados/signals.py:261`. Status já lavrado pode ficar velho.

- **Média — regeneração não garante atualização do status.** O rejulgamento depende de `recalculo=True`, exclui decisões humanas e dias futuros em `app/ponto/portas/celula.py:360` e `app/ponto/portas/celula.py:369`; suas falhas são absorvidas em `app/ponto/portas/celula.py:375`. `reverter_regeneracao_dia` restaura DNA/trabalho em `app/ponto/portas/celula.py:530` e salva esses campos em `app/ponto/portas/celula.py:538`, sem relavrar o status nesse caminho.

- **Média — “uma linha de trilha” não vale para o service admin.** `app/ponto/services/veredito_celula.py:330` chama a porta, que registra `veredito_celula` em `app/ponto/portas/celula.py:777`; depois, o service registra novamente a mesma ação em `app/ponto/services/veredito_celula.py:334`. O adaptador efetivamente chama `evento` em `app/core/auditoria.py:22`. A repetição sem alteração ainda alcança o segundo registro. O teste E3 repete somente o cartório e compara delta de contagem em `app/ponto/tests/test_e3_status_lavrado.py:172`.

- **Média — ramo protegido pelo admin escreve sem LogAuditoria da lavra.** Pode atualizar ata/status e retornar em `app/ponto/portas/celula.py:738` a `app/ponto/portas/celula.py:744`, antes dos registros de auditoria dos demais ramos.

- **Média — trilha incompleta e sem garantia absoluta.** Mudança exclusiva de `status_rotulo` é detectada em `app/ponto/portas/celula.py:703`, mas os payloads registram somente `status_dia` em `app/ponto/portas/celula.py:722` e `app/ponto/portas/celula.py:764`. O registro é best effort em `app/core/observ.py:105`, podendo terminar sem linha em `app/core/observ.py:139`. Com `save=False`, a auditoria também ocorre antes de `carimbar` persistir: `app/ponto/portas/celula.py:774`, `app/ponto/portas/celula.py:777` e `app/ponto/portas/celula.py:807`.

Comportamentos confirmados:

- Mesma entrada sequencial na mesma instância não escreve novamente quando ata, veredito, via, protestos e status coincidem: `app/ponto/portas/celula.py:735`, `app/ponto/portas/celula.py:753` e `app/ponto/portas/celula.py:755`.
- `veredito_em` anda no ramo completo em `app/ponto/portas/celula.py:772`; alteração exclusiva de status retorna antes disso em `app/ponto/portas/celula.py:766`.
- Outbox acorda por `veredito` ou `ata`, não por status: `app/ponto/portas/celula.py:623` e `app/ponto/portas/celula.py:638`. O consumidor chama `retratar_chamados_absolvidos_por_celula` e `encerrar_orfas_pela_lampada`: `app/chamados/reconciliador.py:1304` e `app/chamados/reconciliador.py:1307`.
- Batidas e ausências via save/delete acionam cartório: `app/ponto/signals.py:131`, `app/ponto/signals.py:138`, `app/ponto/signals.py:143` e `app/ponto/signals.py:150`. Raw/bulk estão excluídos desse mecanismo: `app/ponto/signals.py:83`.
- Folga manual atravessa regeneração: `app/escala/folgas.py:257`, `app/escala/folgas.py:261` e `app/escala/folgas.py:220`.

Censo de escrita: os escritores persistentes encontrados são `_aplicar_status`, `lavrar_veredito`, `carimbar_precedencia` e a persistência por `salvar_e_emitir`: `app/ponto/portas/celula.py:705`, `app/ponto/portas/celula.py:768`, `app/ponto/portas/celula.py:769`, `app/ponto/portas/celula.py:820` e `app/ponto/portas/celula.py:640`. A sombra de ata fora da porta é cópia em memória: `app/escala/utils.py:1826`. Os testes E3 fazem escrita direta de status em `app/ponto/tests/test_e3_status_lavrado.py:197` e `app/ponto/tests/test_e3_status_lavrado.py:210`.

Segundo escritor persistente fora da porta, fixture de command escrevendo esses campos, migration de dados escrevendo seus valores ou `CelulaDia.save()` sem `update_fields`: **[não verificado]**; as buscas textuais e AST efetuadas não encontraram caso concreto.

## 31/10 07:42 · sessao c89ee4469920 · - · - · origem=Ronald (tmux)

Achados confirmados em leitura e testemunhas puras por AST, sem Django/banco:

1. **Alta — o status pode absolver a cobertura parcial enquanto o veredito acusa.** A grade conserva `tipo_dia='ausencia'` mesmo quando a cobertura é parcial e conserva as lâmpadas que faltam (`app/escala/utils.py:1180`, `app/escala/utils.py:1184`, `app/escala/utils.py:1190`). O juiz distingue cobertura parcial de integral e pode emitir `FURO_SEM_COBRANCA` pelo restante (`app/ponto/supra_juiz.py:243`, `app/ponto/supra_juiz.py:279`, `app/ponto/supra_juiz.py:334`), convertido em `furo` (`app/escala/models.py:1580`). Entretanto, `fatos_do_status` perde `cobertura_parcial`/`periodo_parcial` (`app/ponto/status_dia.py:103`), e `status_da_lavra` retorna `ausencia` antes de consultar o veredito (`app/ponto/status_dia.py:71`). Testemunha pura: `tipo_dia='ausencia', previsto_min=480, tem_batida=False, isento=False, veredito='furo'` → `ausencia` (`app/ponto/status_dia.py:68`).

2. **Alta — dia sem vínculo válido, com célula de trabalho, pode gravar `indefinida` e `folga` no mesmo ato.** A alimentação detecta célula de trabalho sem vínculo válido (`app/ponto/supra_juiz.py:168`), a grade chega como `folga`, previsto zero (`app/ponto/supra_juiz.py:222`), e o juiz emite `PREVISTO_COM_DIA_INDEFINIDO` mesmo sem previsto (`app/ponto/supra_juiz.py:405`), convertido em `indefinida` (`app/escala/models.py:1583`). O status retorna `folga` antecipadamente (`app/ponto/status_dia.py:73`), tornando inalcançável o tratamento `escala_suspeita` (`app/ponto/status_dia.py:77`). Testemunha pura reproduziu `(indefinida, folga)` (`app/ponto/status_dia.py:73`).

3. **Alta — FolgaDia com batida única pode perder o fato e virar `folga`.** O overlay elimina todas as células e órfãs (`app/escala/utils.py:1118`). A autoridade do realizado soma zero quando há apenas entrada, sem fim fechado (`app/ponto/turnos.py:731`, `app/ponto/turnos.py:736`, `app/ponto/turnos.py:769`); a grade materializa esse realizado (`app/escala/utils.py:1985`, `app/escala/utils.py:1998`). `fatos_do_status` só reconhece batida pelas células/órfãs remanescentes ou por realizado positivo (`app/ponto/status_dia.py:101`). Assim, após apagar o fato visual, retorna `tem_batida=False`, e a lavra grava `folga` (`app/ponto/status_dia.py:73`). Depois que o aberto expirar, o leitor exibirá esse status persistido (`app/ponto/status_dia.py:125`, `app/ponto/status_dia.py:131`).

Quanto ao juiz do encerramento, **não há segundo juiz entre `dia_encerrado` e `dias_encerrados`**: o primeiro delega a `dia_encerrado_motivo` (`app/ponto/turnos.py:2058`), que usa `_dia_encerrado_motivo_puro` (`app/ponto/turnos.py:2079`); o carregador `dias_encerrados` chama exatamente a mesma função (`app/ponto/turnos.py:2137`). `status_do_dia_lavrado` consulta esse carregador corretamente (`app/ponto/status_dia.py:126`).

Existe, porém, **exceção nova no leitor**: `MOTIVO_SEM_PREVISTO` significa que o juiz não afirma encerramento (`app/ponto/turnos.py:2103`), mas o leitor o ignora para devolver a coluna lavrada (`app/ponto/status_dia.py:127`). O texto original D4 manda vocabulário de relógio antes do encerramento (`app/docs/ADR/0002-celula-e-o-dia.md:21`); a exceção está apenas na documentação do novo leitor (`app/ponto/status_dia.py:113`). Registrar como desvio contratual, observando que isso também torna visível o erro do item 2 (`app/ponto/status_dia.py:131`).

Não demonstrado defeito na dependência da migration: 0044 depende de 0043 (`app/escala/migrations/0044_celuladia_status_dia_celuladia_status_rotulo.py:8`), que cria `EventoCelula` (`app/escala/migrations/0043_eventocelula.py:11`). Aplicação efetiva da migration antes do código: **[não verificado]**. A regra exige migrar antes e bloqueia deploy com migration pendente (`CLAUDE.md:138`, `CLAUDE.md:740`).

A tabela `JUIZES.md` não recebeu a nova autoridade: seu catálogo de célula termina no furo (`app/docs/JUIZES.md:72`), enquanto o registro vivo contém o status (`app/core/juizes.py:173`); a própria tabela exige correção no mesmo commit quando diverge (`app/docs/JUIZES.md:14`). Gravidade baixa, documentação.

## 31/10 07:42 · sessao f877b3d9d8f5 · F1C4-3 · verificador · origem=Code

**1. Sim, o teste afirma a cláusula (1) e, pela leitura do caminho, morde a regressão.** Ele cria cobrança aberta, cria célula com `trabalha=None`, executa o comando com `apply=True` e exige status final `aberto` ou `pendente`. Apenas a montagem da grade está mockada; a premissa permanece real. [app/ponto/tests/test_a2_marco_morto.py:33](app/ponto/tests/test_a2_marco_morto.py#L33) [app/ponto/tests/test_a2_marco_morto.py:46](app/ponto/tests/test_a2_marco_morto.py#L46)

O caminho é:

- A grade simulada entrega `sem_ancora=True`; o comando inclui essas datas em `indefinidos`, depois em `mortos`, e solicita `retratar`. Portanto, o teste efetivamente chega à tentativa de fechamento. `app/ponto/tests/test_a2_marco_morto.py:30`; `app/ponto/management/commands/reconciliar_grade.py:139`, `:274`, `:284`, `:301`.
- `retratar → _retratar → ch.reconciliar → _decisao_do_fechamento → _gate_premissa_do_fechamento → premissa_regularizada(para='folha')`. `app/chamados/reconciliador.py:548`, `:525`; `app/chamados/models.py:615`, `:646`, `:699`.
- **Código atual:** `dia_sem_trabalho` retorna `False` para `None` sem ata. A premissa não regulariza por esse atalho; `_celula_furada` retorna `None` porque não há lâmpadas lavradas. O coletor então consulta os fatos, incluindo ausência de batidas. `app/chamados/juizes.py:408`, `:452`; `app/chamados/models.py:1232`, `:1247`, `:1257`.
- **Mutante `trabalha is not True`:** retorna `True` para essa célula; `premissa_regularizada` retorna imediatamente `True`; o gate permite fechar; `RECONCILIAR` leva `aberto → resolvido`. A asserção final rejeita `resolvido`. `app/chamados/models.py:1232`, `:699`, `:618`; `app/chamados/catalogo/motor.py:111`; `app/ponto/tests/test_a2_marco_morto.py:50`.

A cláusula (2), sobre ata declarando dia diferente de trabalho, **não é exercitada pelo teste alterado**: sua célula nasce sem ata fornecida, e `ata` é campo anulável. `app/ponto/tests/test_a2_marco_morto.py:46`; `app/escala/models.py:1746`.

Execução do teste completo e comprovação empírica verde/vermelho: **[não verificado]**. Comparação histórica dos dois commits: **[não verificado]**. [nao verificado]

**2. Existe um positivo com `None`, mas sustentado pela ata de folga.** O teste exige literalmente `dia_sem_trabalho(_cel(None, 'folga')) is True`, em conformidade com a cláusula (2). [app/chamados/tests/test_f1c4_3_none_cobra.py:47](app/chamados/tests/test_f1c4_3_none_cobra.py#L47)

Não encontrei outro teste que faça **`None` sozinho** calar cobrança ou fechar chamado por esses leitores; garantia absoluta de ausência de qualquer caso indireto: **[não verificado]**. Os testes explícitos encontrados exigem o contrário: [nao verificado]

| Caso | Asserção | Local |
|---|---|---|
| `None` + ata de trabalho | `dia_sem_trabalho=False` | `app/chamados/tests/test_f1c4_3_none_cobra.py:40` |
| `None` + ata sem `tipo_dia` | `dia_sem_trabalho=False` | `app/chamados/tests/test_f1c4_3_none_cobra.py:43` |
| `None` + lâmpada apagada | `_celula_furada=True` | `app/chamados/tests/test_f1c4_3_none_cobra.py:94`, `:95` |
| `None` + ata de trabalho | `premissa_regularizada=False` | `app/chamados/tests/test_f1c4_3_none_cobra.py:107`, `:109` |
| `None`, outro gate de emissão | `celula_veta_cobranca=None`, sem veto | `app/ponto/tests/test_t31_gate_celula.py:25`, `:34` |

Dois semelhantes merecem distinção:

- `test_tripwire_ata_fantasma` usa `None + ata folga`, mas afirma contagem pelo detector de ata fantasma, sem afirmar encerramento pelos leitores perguntados. `app/ponto/tests/test_tripwire_ata_fantasma.py:60`, `:62`, `:78`, `:81`.
- `test_selo_limbo_cala_o_dia` afirma silêncio por `limbo_folgas.pode_calar` sobre vínculo sem folga definida; não prepara célula `trabalha=None` para os três leitores perguntados. `app/ponto/tests/test_selo_limbo_cala_o_dia.py:37`, `:46`, `:73`, `:77`.

**3. Escritores encontrados e leitores centrais dos campos:**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| **`CelulaDia.trabalha`** | No teste, a fixture grava `None`. Na produção, a porta da célula gera, regenera, neutraliza célula fora da vigência e recompõe o valor na reversão. `app/ponto/tests/test_a2_marco_morto.py:46`; `app/ponto/portas/celula.py:231`, `:247`, `:260`, `:532`, `:595`, `:612`. | No caminho auditado, `dia_sem_trabalho`, chamado pela premissa e por `_celula_furada`. Outros leitores: `eh_dia_trabalho`, precedência, guarda dos marcos e veto de emissão. `app/chamados/juizes.py:408`, `:443`; `app/chamados/models.py:1232`; `app/escala/models.py:1072`; `app/ponto/precedencia.py:290`; `app/escala/servico_jornada.py:303`; `app/ponto/marcos.py:155`. |
| **`CelulaDia.ata`** | O cartório produz o conteúdo por `ata_do_dia` e entrega à lavra; a porta atribui e persiste. `carimbar_precedencia` também escreve a chave `precedencia`. `app/ponto/services/cartorio.py:558`, `:566`, `:1520`, `:1535`; `app/ponto/portas/celula.py:711`, `:725`, `:640`, `:775`. | `dia_sem_trabalho` lê `tipo_dia`; `_celula_furada` lê lâmpadas e órfãs; a premissa também consulta lastro dos marcos. Outros consumidores incluem prévia e validação PDF. `app/chamados/juizes.py:407`, `:452`, `:455`, `:457`; `app/chamados/models.py:1234`; `app/ponto/services/lastro.py:89`; `app/folha/previa.py:83`; `app/folha/validacao_pdf.py:47`. |
| **`ChamadoColaborador.status_local`** | Nasce pelos gates `abrir`/`criar`; transições atribuem em `_gravar_estado` e persistem no seu `save`. Neste caminho, `reconciliar` chama esse escritor. `app/chamados/models.py:897`, `:1046`, `:222`, `:279`, `:618`. | O teste relê do banco e verifica o status; `reconciliar_grade` seleciona vivos; o gate consulta o motor; C6, C8 e cartório também consomem o estado. `app/ponto/tests/test_a2_marco_morto.py:49`; `app/ponto/management/commands/reconciliar_grade.py:96`, `:207`; `app/chamados/models.py:637`; `app/chamados/juizes.py:719`, `:1780`; `app/ponto/services/cartorio.py:335`. |

`ata` é campo persistido, mas há projeções somente em memória: a célula pode receber a ata do julgamento corrente para leitura, e a montagem da grade também produz uma projeção. Assim, atribuição a `.ata` não implica, sozinha, escrita no banco. `app/escala/models.py:1746`; `app/chamados/juizes.py:394`; `app/ponto/portas/celula.py:678`; `app/escala/utils.py:1827`.

Exaustividade de todos os consumidores desses campos no repositório: **[não verificado]**.

## 31/10 07:41 · sessao 3a0fd74c031b · - · - · origem=Ronald (tmux)

Por leitura estática, **sim: o teste morde a volta de `trabalha is not True`**. Execução da suíte e mutante: [não verificado]. [nao verificado]

Caminho:

1. O teste cria chamado `batida_ausente` aberto, célula `trabalha=None` sem ata e mocka somente a montagem da grade; ao final exige `status_local` em `('aberto', 'pendente')`. (app/ponto/tests/test_a2_marco_morto.py:37–50; app/ponto/management/commands/reconciliar_grade.py:54)
2. `sem_ancora` coloca a data em `indefinidos`; ela entra em `mortos` e o command chama `retratar`. (app/ponto/management/commands/reconciliar_grade.py:139–150,274–301)
3. `retratar → _retratar → ch.reconciliar → _decisao_do_fechamento → _gate_premissa_do_fechamento`. (app/chamados/reconciliador.py:525,546–548; app/chamados/models.py:615,646)
4. O gate pergunta `premissa_regularizada(para='folha')`. No código atual, `dia_sem_trabalho` responde `False` para `None` sem ata; `_celula_furada` responde `None` porque faltam `lampadas`; o coletor recorre aos fatos e encontra falta descoberta, pois a fixture não cria batidas. Assim a premissa segue viva e o gate recusa o fechamento. (app/chamados/models.py:699,1232–1264,728–732; app/chamados/juizes.py:408,452–453; app/ponto/tests/test_a2_marco_morto.py:19–48)
5. No mutante, `dia_sem_trabalho=True` faz `premissa_regularizada` retornar `True` diretamente em `models.py:1232–1233`; o gate permite, `reconciliar` grava `resolvido`, e a asserção da linha 50 falha. (app/chamados/models.py:699–700,618; app/chamados/catalogo/motor.py:111; app/ponto/tests/test_a2_marco_morto.py:50)

Sinais não invalidam essa conclusão: o nascimento do chamado aciona o cartório, porém o cartório retorna sem criar célula quando ela ainda inexiste; a célula é criada depois, expressamente pelo teste. (app/chamados/signals.py:164–180; app/ponto/services/cartorio.py:1658–1660; app/ponto/tests/test_a2_marco_morto.py:37–46)

A fixture usa escala semanal com trabalho nos sete dias e o campo `trabalha_em_feriado` tem default `True`; portanto não encontrei dependência de fim de semana ou feriado que torne a premissa regularizada nessa fixture. (app/chamados/tests/_vinculo_do_dia.py:18–26; app/escala/models.py:896–898; app/ponto/precedencia.py:314–324)

Limite de cobertura: esse teste afirma sobrevivência de **cobrança já existente** com `None` e ata ausente; não testa emissão nova nem a cláusula de ata com `tipo_dia != 'trabalho'`. (app/ponto/tests/test_a2_marco_morto.py:37–50)

## 31/10 07:41 · sessao f1b5c3353445 · - · - · origem=Ronald (tmux)

Mapa confirmado dos campos, com escritores reais e leitores relevantes ao delta:

| Campo | Natureza | Quem escreve | Quem lê |
|---|---|---|---|
| `CelulaDia.trabalha` | `BooleanField(null=True)`, sem `default` explícito; `None=indeterminado (limbo cobra)`. `app/escala/models.py:1654` | Porta `ponto/portas/celula.py`: regeneração fora da vigência põe `False` (:231); regeneração cria (:247) ou atualiza (:260, salva :264); reversão recompõe pelo DNA (:532, salva :538); geração cria (:595) ou atualiza (:612, salva :615). | Juiz `dia_sem_trabalho`: `is False` (:408 de `app/chamados/juizes.py`); `EscalaColaborador.eh_dia_trabalho` devolve o valor preservando `None` (`app/escala/models.py:1072`); precedência (`app/ponto/precedencia.py:290`, :300, :306); guarda dos marcos (`app/escala/servico_jornada.py:303`); veto da emissão (`app/ponto/marcos.py:155`, :159). |
| `CelulaDia.ata` | Campo persistido `JSONField(null=True, blank=True)`, não property. `app/escala/models.py:1746` | O produtor do conteúdo é `cartorio.ata_do_dia`, incluindo `tipo_dia`, `lampadas`, `sem_ancora` etc. (`app/ponto/services/cartorio.py:558`, :566). Cartório envia à lavra (:1518, :1520); método do modelo delega (`app/escala/models.py:1781`). Porta `lavrar_veredito` atribui (`app/ponto/portas/celula.py:711`, :725); preserva chave `precedencia` (:703); `carimbar_precedencia` grava essa chave (:775, :776). Persistência em `salvar_e_emitir` (:640); cartório pode lavrar com `save=False` e persistir junto com carimbo (:1535 de `cartorio.py`, :762 de `portas/celula.py`). | `dia_sem_trabalho` lê `tipo_dia` (`app/chamados/juizes.py:407`); `_celula_furada` lê a ata e `lampadas`/órfãs (:442, :452, :455, :457); premissa chama esses leitores (`app/chamados/models.py:1232`, :1247). Lastro lê lâmpadas (`app/ponto/services/lastro.py:89`, :113, :128, :149); apresentação por célula (`app/folha/previa.py:83`); validação PDF (`app/folha/validacao_pdf.py:47`). |
| `ChamadoColaborador.status_local` | `CharField(..., default='aberto')`. `app/chamados/models.py:46` | Nascimento por `abrir`: monta status vindo do motor (:897) e cria pelo gate; `criar`: status definido em :1022, grava em :1046. Mudança de estado em `_gravar_estado`: atribui :222, persiste :279. `reconciliar` passa por decisão :615 e chama escritor :618. Há simulação temporária somente em memória para julgamento, restaurada em `finally` (:339, :352). | O teste atualiza instância do banco e verifica aberto/pendente (`app/ponto/tests/test_a2_marco_morto.py:49`, :50). Reconciliador seleciona chamados abertos (`app/ponto/management/commands/reconciliar_grade.py:96`, :207), lê encerrados para reabrir (:521). Gate consulta motor pelo estado (`app/chamados/models.py:637`); C6 usa estado do destino (`app/chamados/juizes.py:719`); C8 filtra vivos (:1780). Cartório usa o estado na impressão (`app/ponto/services/cartorio.py:335`) e filtra vivos (:1100). |

A geração é chamada pelo command `gerar_celulas` (`app/ponto/management/commands/gerar_celulas.py:311`); o command reexporta a porta (:13). Signals de escala também acionam regeneração (`app/escala/signals.py:71`, :117).

No teste, o escritor de `trabalha=None` é a própria fixture (`app/ponto/tests/test_a2_marco_morto.py:46`); ela não fornece `ata`, e a grade mockada não é uma gravação de ata (`app/ponto/tests/test_a2_marco_morto.py:26`, :30, :47`). Existe também substituição de ata apenas em memória no fluxo normal: `celula_do_dia_agora → celula_deste_julgamento → celula_como_o_julgamento_deixa` (`app/chamados/juizes.py:378`, :394; `app/ponto/portas/celula.py:678`, :680); e uma projeção em memória na montagem da grade (`app/escala/utils.py:1827`). Portanto, “só lavrar_veredito escreve ata” precisa distinguir persistência, chave `precedencia` e projeções em memória (`app/ponto/portas/celula.py:640`, :775, :680).

Este é o mapa dos escritores encontrados e dos leitores centrais/relevantes, não um censo exaustivo de todos os consumidores do repositório. [não verificado]

## 31/10 07:41 · sessao 1a64a44db582 · - · - · origem=Ronald (tmux)

Na busca global e inspeção dos testes, não encontrei outro teste que faça **`trabalha=None`, por si só**, calar cobrança ou regularizar premissa nesses leitores; ausência absoluta de caso indireto: [não verificado]. [nao verificado]

O positivo explícito é **`None` com ATA de folga**, autorizado pela cláusula 2: `dia_sem_trabalho(_cel(None, 'folga')) is True` em `app/chamados/tests/test_f1c4_3_none_cobra.py:47`.

As proteções contrárias são:

- `None` + ATA de trabalho → `False`: `app/chamados/tests/test_f1c4_3_none_cobra.py:40`.
- `None` + ATA sem `tipo_dia` → `False`: `app/chamados/tests/test_f1c4_3_none_cobra.py:43`.
- `None` + lâmpada apagada → `_celula_furada=True`: preparação em `app/chamados/tests/test_f1c4_3_none_cobra.py:94`, asserção em `:95`.
- `None` + ATA de trabalho → `premissa_regularizada=False`: preparação em `app/chamados/tests/test_f1c4_3_none_cobra.py:107`, asserção em `:109`.
- Outro gate também exige que `None` não vete cobrança: célula em `app/ponto/tests/test_t31_gate_celula.py:25`, asserção em `:34`.

Casos parecidos que **não exercitam esses leitores**:

- `None` + ATA folga é contabilizado pelo detector de ATA fantasma, sem asserção de encerramento: `app/ponto/tests/test_tripwire_ata_fantasma.py:60`, `:62`, `:78`, `:81`.
- Dia duvidoso isolado pode ser calado por `limbo_folgas.pode_calar`; o teste usa vínculo 6x1 sem folga, sem criar `CelulaDia`: `app/ponto/tests/test_selo_limbo_cala_o_dia.py:37`, `:46`, `:73`, `:77`.
- Testes próximos de fechamento por folga usam **`False`**, não `None`: `app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:197`, `:201`, `:214`, `:215`; `app/ponto/tests/test_t32_folga_sem_turno.py:24`, `:31`.

## 31/10 07:35 · sessao 45b4164c7701 · F1C4-3 · verificador · origem=Code

**Não aprovaria a obra como “predicado único”.** A troca atende ao aval dentro de `dia_sem_trabalho`, mas altera a escolha entre dia e véspera, mantém decisões paralelas em produção e deixa um teste legado afirmando a regra revogada. (`app/chamados/juizes.py:408`, `app/chamados/juizes.py:443`, `app/ponto/services/cartorio.py:1167`, `app/ponto/tests/test_a2_marco_morto.py:38`)

**(a) Escritores, leitores e alcance da mudança**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `CelulaDia.trabalha` | A porta da célula: geração e regeneração recebem `eh_dia_trabalho_calculado`; também existem saneamento fora da vigência e reversão de regeneração. (`app/ponto/portas/celula.py:590`, `:595`, `:243`, `:260`, `:231`, `:532`) | O juiz registrado do vínculo devolve o valor ternário; `dia_sem_trabalho` interpreta esse valor junto da ata; `_protesta` faz sua própria seleção. (`app/escala/models.py:1072`, `app/chamados/juizes.py:408`, `app/ponto/services/cartorio.py:1167`) |
| `ata['tipo_dia']` | `cartorio.ata_do_dia` copia o tipo produzido pela grade; a porta persiste a ata. (`app/ponto/services/cartorio.py:566`, `app/ponto/portas/celula.py:725`, `:732`) | `dia_sem_trabalho`, leitor da célula e consumidores da grade, inclusive folha. (`app/chamados/juizes.py:408`, `app/escala/services/leitor_celula.py:344`, `app/folha/export.py:1071`) |
| `ata['lampadas']` | `lampadas_do_dia`, incorporado por `ata_do_dia`; dispensa acende lâmpadas e teto temporal marca pendentes com `acesa=None`. A persistência passa pela mesma porta. (`app/ponto/services/cartorio.py:427`, `:564`, `:617`, `:713`; `app/ponto/portas/celula.py:732`) | `_celula_furada`, lastro por marco, apresentação e folha. (`app/chamados/juizes.py:455`, `app/ponto/services/lastro.py:175`, `app/escala/services/leitor_celula.py:93`, `app/folha/previa.py:84`) |

Há atas projetadas **em memória**, sem constituir escritor persistente adicional: a grade reutiliza `lampadas_do_dia`, e a leitura do julgamento atual recebe uma cópia pela porta. (`app/escala/utils.py:1818`, `:1826`, `:1832`; `app/chamados/juizes.py:394`; `app/ponto/portas/celula.py:678`)

O efeito nas três funções é:

- **`dia_sem_trabalho`:** somente `None` com tipo ausente/vazio ou `'trabalho'` muda de `True` para `False`; a cláusula da ata permanece. Isso corresponde às duas cláusulas literais do aval. (`app/chamados/juizes.py:407`, `:408`)
- **`_celula_furada`:** nesses casos, deixa de recursar à véspera e passa a consultar as lâmpadas do próprio dia. Portanto, a mudança **não é monotônica no sentido de cobrar mais**. (`app/chamados/juizes.py:443`, `:446`, `:452`, `:455`)
- **`premissa_regularizada`:** perde o retorno antecipado de dia sem trabalho e passa pelos marcos, pelo furo e eventualmente pelos fallbacks. Justificativa aprovada e decisão administrativa continuam podendo regularizar antes disso. (`app/chamados/models.py:1214`, `:1230`, `:1232`, `:1236`, `:1245`, `:1247`, `:1253`)

**Contraexemplo relevante:** D tem `trabalha=None`, ata de trabalho e todas as lâmpadas acesas; D−1 tem lâmpada apagada, sem cobertura de ausência. Antes, `_celula_furada(D)` consultava D−1 e retornava `True`; depois retorna `False` pelas lâmpadas de D. Para `turno_aberto*`, o coletor deliberadamente não usa o atalho dos demais módulos: chega a essa mudança e pode considerar regularizado um chamado cuja véspera continua furada. É uma consequência estática do código; ocorrência em produção **[nao verificado]**. (`app/chamados/juizes.py:443`, `:446`, `:455`, `:465`; `app/chamados/models.py:1232`, `:1247`)

O aval não explicita retirar a consulta à véspera nesse cenário, e o RED novo não o cobre; já existe teste exigindo que um `turno_aberto` datado na folga continue vivo pela véspera furada, mas usando `trabalha=False`, não `None`. (`app/chamados/tests/test_f1c4_3_none_cobra.py:93`, `:105`; `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:65`, `:66`, `:75`, `:88`)

Também há um defeito **preexistente** nessa recursão: ela não transmite `para=para`; uma consulta para folha volta ao default cobrança na véspera, onde ausência aberta pode cobrir. Ao evitar a recursão em D=None, o diff também muda qual política de cobertura participa. (`app/chamados/juizes.py:411`, `:424`, `:426`, `:446`)

Os leitores indiretos afetados incluem:

| Porta/leitor | Consequência possível, se os demais requisitos permitirem |
|---|---|
| S122 humano | Premissa antes regularizada pode passar a impedir resolução com `ValueError`. (`app/chamados/models.py:175`, `:177`, `:185`) |
| Fechamento pelo sistema | Pode recusar reconciliação; permanecem as exceções de agrupamento e deduplicação. (`app/chamados/models.py:646`, `:699`, `:709`) |
| Reabertura | Pode deixar de recusar por premissa regularizada; veto do dia e fechamento administrativo sem fato novo continuam anteriores. (`app/chamados/models.py:1285`, `:1290`, `:1296`) |
| Emissor que encontra encerrado | `abrir` pede renascimento com os marcos do **novo pedido**; passa por `reabrir`, podendo gravar estado e ressuscitar perguntas. (`app/chamados/models.py:880`, `:882`, `:978`, `:1235`, `:1380`, `:1389`) |
| Defesa 1 | O fechamento do pai passa pelo mesmo `reconciliar` afetado. (`app/chamados/signals.py:42`) |

Esses efeitos são alcançados pelo coletor comum; não são automaticamente violações do aval. A lacuna é certificá-los apenas com chamadas diretas ao juiz/coletor, sem testar os portões e o caso da véspera. (`app/chamados/tests/test_f1c4_3_none_cobra.py:40`, `:95`, `:109`)

**(b) Mesma pergunta ou perguntas distintas**

**`_protesta` inteiro responde outra pergunta:** “este chamado deve participar como protesto neste julgamento?”. Inclui a satisfação dos marcos específicos, cujo juiz é `marcos_acesos`. Essa parte não equivale a “dia sem trabalho”. (`app/ponto/services/cartorio.py:1167`; `app/ponto/services/lastro.py:153`, `:170`)

**Mas contém uma subpergunta duplicada e divergente:**

```text
dia_sem_trabalho: trabalha é False OU ata.tipo_dia não trabalho
_protesta:       trabalha não é False E marcos não comprovadamente acesos
```

Para `trabalha=True` ou `None`, ata `'folga'` e marco apagado/sem marco: `dia_sem_trabalho=True`, enquanto `_protesta=True`. A dispensa pelo tipo lavrado é reconhecida num sítio e ignorada no outro. (`app/chamados/juizes.py:408`; `app/ponto/services/cartorio.py:1167`; `app/ponto/services/lastro.py:172`, `:179`)

Isso interfere na entrada de `classificar_dia`, na lâmpada `'cobrado'`, no adiamento e na seleção para reconciliação. A divergência **já existia**; o diff alinha o caso None/ata trabalho, mas não realiza a promessa de predicado único. (`app/ponto/services/cartorio.py:1168`, `:1170`, `:1206`, `:1236`, `:1237`, `:1242`)

Outros sítios ainda classificam None como folga/sem trabalho:

- Relatório soma em `folga` e deixa de examinar a acusação. (`app/relatorios/dia_da_frota.py:47`)
- Frase para admin/IA afirma “Dia sem trabalho previsto: nenhuma marca era esperada”. (`app/ponto/services/dia_do_colab.py:121`)
- Passivo classifica como `FOLGA`, classe selecionada por `a_retratar`. (`app/ponto/services/passivo_s84.py:96`, `:116`)
- `reconciliar_grade` mantém a derivação “previsto não sabido = premissa morta”, inclui indefinidos entre mortos e fornece fatos `False` literais antes de tentar retratar; o gate posterior pode recusar. (`app/ponto/management/commands/reconciliar_grade.py:272`, `:275`, `:292`, `:300`; `app/chamados/models.py:699`)

Há também **perguntas legitimamente distintas**: o juiz registrado do vínculo responde o valor ternário da previsão; `dia_sem_trabalho` acrescenta a decisão lavrada na ata; fabricar pergunta ou autorizar plantio exige evidência positiva e não constitui, por si, afirmação de folga. (`app/core/juizes.py:158`; `app/escala/models.py:1072`; `app/chamados/juizes.py:408`; `app/chamados/services/fabrica_da_celula.py:78`; `app/chamados/services/materializacao.py:158`)

Uma tensão adicional é preexistente: a grade pode lavrar `'ausencia'` **parcial**, preservando as células, enquanto `classificar_falta` não cala por ausência parcial. O aval manda manter a cláusula ampla da ata; não atribuo essa divergência à alteração proposta. (`app/escala/utils.py:1180`, `:1190`, `:1195`; `app/escala/servico_jornada.py:313`, `:344`; `app/chamados/juizes.py:408`)

**(c) Teste existente que afirma o comportamento antigo** [nao verificado]

**Sim:** `MarcoMortoTest.test_cobranca_em_dia_indefinido_e_retratada`. O comentário declara literalmente None como “dia sem furo”; a fixture cria `trabalha=None`, e a expectativa exige retratação. Essa especificação precisa ser invertida diante do aval. (`app/ponto/tests/test_a2_marco_morto.py:28`, `:38`, `:40`, `:44`)

Não afirmo que o teste necessariamente falhe agora: ele cria célula sem ata; depois do diff, esse caso alcança o retorno desconhecido e os fallbacks do coletor. Resultado executado **[nao verificado]**. (`app/ponto/tests/test_a2_marco_morto.py:40`; `app/chamados/juizes.py:452`; `app/chamados/models.py:1250`)

**(d) Checklist**

| Classe | Verificação |
|---|---|
| Segundo juiz duplicando registrado | Não há função nova no diff. Não basta chamar `dia_sem_trabalho` de clone do juiz ternário do vínculo; a duplicação comprovada está na interpretação paralela usada por `_protesta`. (`app/core/juizes.py:158`; `app/escala/models.py:1072`; `app/chamados/juizes.py:408`; `app/ponto/services/cartorio.py:1167`) |
| Segundo escritor fora da porta | Não aparece escritor novo de produção; a atualização direta no arquivo novo é fixture. (`app/chamados/juizes.py:407`; `app/chamados/tests/test_f1c4_3_none_cobra.py:83`) |
| Relógio fora da borda | O predicado alterado não lê relógio; o teste usa tempo congelado. (`app/chamados/juizes.py:407`; `app/chamados/tests/test_f1c4_3_none_cobra.py:59`) |
| Derivação paralela | Permanece em `_protesta`, nas classificações de None e no retratador de indefinidos descritos acima. (`app/ponto/services/cartorio.py:1167`; `app/ponto/services/passivo_s84.py:96`; `app/ponto/management/commands/reconciliar_grade.py:275`) |
| RED sem mordida | **O RED novo morde a volta de `is not True`:** os dois asserts diretos exigem `False` onde o predicado antigo retorna `True`. Falta cobertura da véspera e dos portões, não mordida dessa mutação. Execução **[nao verificado]**. (`app/chamados/tests/test_f1c4_3_none_cobra.py:40`, `:43`, `:95`, `:109`; `app/chamados/juizes.py:405`) |
| Ordem de trava | O diff não altera aquisição de locks; o fechamento atual trava a linha dentro do atomic. Prova concorrente desta obra **[nao verificado]**. (`app/chamados/juizes.py:407`; `app/chamados/models.py:666`, `:672`) |
| Pouso antes do pré-requisito | Há exigência documental de sombra antes do pouso e vai-e-volta zero. Seu cumprimento não está demonstrado pelo material fornecido **[nao verificado]**. (`app/docs/CORTES.md:409`, `:412`) |

## 31/10 07:33 · sessao d1ff92a88f33 · - · - · origem=Ronald (tmux)

Escritores e fontes:

- `trabalha`: a porta declara dono único e operações em `app/ponto/portas/celula.py:3`, `:66`; geração recebe `eh_dia_trabalho_calculado` e cria/atualiza em `:590`, `:595`, `:612`; regeneração em `:243`, `:247`, `:260`; fora do vínculo vira `False` em `:231`; reversão usa existência de `dna.marcos` em `:532`.
- `ata.tipo_dia` e `ata.lampadas`: `ata_do_dia` monta ambos em `app/ponto/services/cartorio.py:558`, `:564`, `:566`; produtor comum das lâmpadas em `:427`; ausência/justificativa dispensam acendendo lâmpadas em `:593`, `:611`, `:617`; teto modifica pendentes em `:706`.
- Persistência da ata: fachada `app/escala/models.py:1775` delega à porta; atribuições e salvamento em `app/ponto/portas/celula.py:697`, `:711`, `:725`, `:732`. `carimbar_precedencia` acrescenta somente chave `precedencia`, `:774`, `:775`, preservada no merge `:703`.
- Projeções em memória: `app/escala/utils.py:1826` copia célula e põe somente lâmpadas/orfas em `:1827`; `app/ponto/portas/celula.py:667` cria cópia do julgamento atual, ata em `:680`. Não equivalem a novo escritor persistente.

Comparação de perguntas:

- Autoridade registrada “dia de trabalho para o vínculo” é `eh_dia_trabalho`, `app/core/juizes.py:158`; devolve o valor ternário bruto da célula em `app/escala/models.py:1072`. `dia_sem_trabalho` adiciona uma pergunta composta: escala explicitamente folga **ou** ata não trabalho (`app/chamados/juizes.py:408`). Portanto não é simplesmente outra implementação da negação booleana do juiz do vínculo.
- `_protesta` responde se um chamado contribui para o julgamento atual, incluindo seus **marcos**, `app/ponto/services/cartorio.py:1166`, `:1167`. Função inteira difere de `dia_sem_trabalho`, mas o subpredicado “dia sem trabalho” diverge: `trabalha=True/None`, `ata.tipo_dia='folga'/'ausencia'`, marco apagado → `_protesta=True`, `dia_sem_trabalho=True`. A divergência da cláusula ata é preexistente; o diff alinha o caso `None + ata trabalho`, não elimina as duas definições (`app/chamados/juizes.py:408`; `app/ponto/services/cartorio.py:1167`).
- Atenção: `'ausencia'` na ata pode indicar cobertura **parcial**, preservando batidas e lâmpadas, `app/escala/utils.py:1180`, `:1190`, `:1195`. `classificar_falta` declara que parcial não cala cobrança em `app/escala/servico_jornada.py:313` e somente retorna sem furo quando ausência não parcial em `:342`, `:344`. A cláusula 2 do aval mantém a supressão ampla; não é falha introduzida por este diff.

Outros consumidores ainda equiparam `None` a folga:

- Relatório soma `folga` e exclui acusações em `app/relatorios/dia_da_frota.py:47`.
- Frase para admin/IA afirma “Dia sem trabalho previsto: nenhuma marca era esperada” em `app/ponto/services/dia_do_colab.py:121`.
- Passivo classifica `None=FOLGA` em `app/ponto/services/passivo_s84.py:96`; `a_retratar` seleciona essa classe em `:116`. São classificações do mesmo fato, com efeitos diferentes.
- Já exigir `True` para fabricar pergunta (`app/chamados/services/fabrica_da_celula.py:78`) ou dar lastro a plantio (`app/chamados/services/materializacao.py:158`) responde elegibilidade/autorização, não prova que `None` seja folga.
- Leitor da célula preserva indeterminação sem tipo lavrado: `'indefinido'`, `app/escala/services/leitor_celula.py:344`.

Classes leitoras de ata/lâmpadas que não usam necessariamente o juiz alterado:

- Grade/apresentação: `app/escala/services/leitor_celula.py:93`, `:98`, `:344`; worklist: `app/ponto/worklist.py:19`, `:24`.
- Lastro por marco: `app/ponto/services/lastro.py:89`, `:128`, `:175`; nomeação de cobrança: `app/chamados/reconciliador.py:673`, `:677`.
- Pergunta/resposta: `app/chamados/juizes.py:885`, `:888`, `:1006`.
- Geometria/realizado: entrada e contrato ternário em `app/ponto/juiz_batida.py:4`, `:15`.
- Folha/contadores: `app/folha/previa.py:84`, `:86`; `app/folha/services/sem_furo.py:20`, `:26`; soma por tipo em `app/folha/export.py:1071`, `:1075`, `:1080`.
- Detectores: `app/ponto/services/cluster_espurio.py:24`, `:27`.

Nenhuma medição de quantidade ou impacto em produção foi demonstrada por essas leituras [nao verificado].

## 31/10 07:33 · sessao c95fcf9aafe2 · - · - · origem=Ronald (tmux)

Achados para consolidação:

1. **A mudança não apenas acrescenta cobrança: pode trocar a célula julgada e até deixar de cobrar.** Em `None` com `ata.tipo_dia='trabalho'` ou ausente, o código antigo entrava no ramo de véspera; o novo consulta as lâmpadas de D. Contraprova estática: D=`None` com lâmpadas acesas; D−1 com lâmpada apagada. Antes `_celula_furada(D)=True`; depois `False`. Para `turno_aberto*`, o coletor não usa o atalho direto de dia sem trabalho e chega justamente nessa mudança; pode regularizar/fechar turno cuja véspera continua furada. Referências: `app/chamados/juizes.py:408`, `:443`, `:446`, `:452`, `:455`, `:465`; `app/chamados/models.py:1232`, `:1247`. Ocorrência em produção: [não verificado].

2. **Há perda preexistente de `para` na recursão.** `_celula_furada(..., para='folha')` recursa à véspera sem transmitir `para`, voltando a `'cobranca'`; nesse modo ausência aberta pode cobrir, segundo comentário e chamada a `ausencia_cobre`. O diff passa a evitar esse caminho em D=None, alterando também qual política de cobertura participa. Referências: `app/chamados/juizes.py:411`, `:424`, `:426`, `:446`. Não é defeito introduzido pela linha, mas precisa entrar no censo do efeito.

3. **Os efeitos alcançam todos estes portões, sem testes novos deles:** [nao verificado]
   - S122 humano pode passar de fechamento permitido a `ValueError`: `app/chamados/models.py:175`, `:177`, `:185`.
   - `reconciliar()` e seu dry `reconciliaria()` leem o mesmo gate; premissa viva pode impedir fechamento, com exceções de agrupado/irmão: `app/chamados/models.py:605`, `:615`, `:625`, `:646`, `:699`, `:709`.
   - `recusa_da_reabertura` deixa de recusar por premissa regularizada; continuam anteriores os vetos e admin sem fato novo: `app/chamados/models.py:1285`, `:1290`, `:1296`.
   - Emissor encontra encerrado em `abrir`, pede renascimento com **marcos do novo pedido**, passa por `reabrir`; se permitido grava estado e ressuscita perguntas: `app/chamados/models.py:880`, `:881`, `:978`, `:1235`, `:1338`, `:1380`, `:1389`.
   - Defesa 1 também passa pelo fechamento afetado: `app/chamados/signals.py:42`.
   - Os testes novos só chamam juiz, `_celula_furada` e coletor diretamente: `app/chamados/tests/test_f1c4_3_none_cobra.py:39`, `:93`, `:105`. Não demonstram efeito nos portões acima nem no turno noturno.

4. **`_protesta`: pergunta composta distinta, mas subpergunta duplicada.** Decide se uma cobrança protesta no julgamento: `_cel.trabalha is not False and _acesos(x, _ata) is not True`. Seu componente de “dia sem trabalho” não lê `ata.tipo_dia`, embora `dia_sem_trabalho` o leia. Exemplo: trabalha=True ou None, ata folga, marco apagado/sem marco → `dia_sem_trabalho=True` e `_protesta=True`. Referências: `app/ponto/services/cartorio.py:1166`, `:1167`; `app/chamados/juizes.py:408`; sem marco dá `marcos_acesos=None`: `app/ponto/services/lastro.py:170`, `:172`.
   
   O protesto decide entrada em `classificar_dia`, lâmpada `cobrado`, adiamento e seleção para reconciliação: `app/ponto/services/cartorio.py:1168`, `:1170`, `:1206`, `:1236`, `:1237`, `:1242`. Divergência é preexistente; o diff não realiza a promessa de predicado único. Não confundir a outra metade — “os marcos desta cobrança acenderam?” — com “dia sem trabalho”: aquela é pergunta própria e tem juiz `marcos_acesos`: `app/ponto/services/lastro.py:153`, `:156`.

5. **Teste antigo contraditório encontrado fora do arquivo novo:** `MarcoMortoTest.test_cobranca_em_dia_indefinido_e_retratada` documenta “previsto não sabido = premissa morta” e exige retratação de chamado sobre célula trabalha=None. Referências: `app/ponto/tests/test_a2_marco_morto.py:8`, `:12`, `:28`, `:38`, `:40`, `:44`. Deve mudar especificação/expectativa diante do aval. **Não afirmar que já falha em execução:** a fixture tem ata ausente; agora `_celula_furada` retorna None e o coletor usa os fallbacks. Referências: `app/chamados/juizes.py:452`, `app/chamados/models.py:1250`, `:1253`, `:1258`. Execução: [não verificado].

6. **Teste existente prova a relevância da véspera:** turno aberto de D+1 folga deve continuar vivo cobrando D furada, e a pergunta deve ressuscitar lendo a célula dela; o fixture exige `_celula_furada(D+1)=True`. Referências: `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:63`, `:65`, `:66`, `:75`, `:88`, `:90`. O novo RED não cobre variante D+1=None.

_911 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
