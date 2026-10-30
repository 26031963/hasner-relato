# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 14:23** · publicado em **30/10/2026 14:27** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 14:23 · sessao 3943ba67c44f · CALENDARIO-HOJE · verificador · origem=Code

**Parecer: reprova.** O teto HOJE/ONTEM foi removido, mas permanecem divergências por `sem_previsto`, incompatibilidade de chaves no cartório e reutilização de turnos de outra janela. O custo novo caiu para **+1 query por calendário elegível**, sem zerar. (`app/colaboradores/services/calendario.py:707-712`; `app/ponto/turnos.py:1852,1928,2130-2139`)

1. **Teto de data**

   **Antigo corte HOJE/ONTEM: sem achado.** Agora a pergunta cobre `data_ini` até `min(data_fim, hoje)`, incluindo datas anteriores a ontem. (`app/colaboradores/services/calendario.py:707-710`)

   **Ainda há “não encerrado” que o calendário desconsidera:** `sem_previsto` produz `False` na juíza, mas fica fora de `_em_curso`. Em trabalho sem batida, isso permite `falta`. Há também a divergência de janela descrita abaixo. (`app/ponto/turnos.py:2102-2105,2140`; `app/colaboradores/services/calendario.py:711-712,132-142,805`)

2. **Equivalência da alimentação: não vale universalmente**

   | Chamador | Alimentação efetiva |
   |---|---|
   | Cartório | Passa `celulas={(colaborador_id, data): célula}`; não fornece `turnos` nem `escalas`. (`app/ponto/services/cartorio.py:853`) |
   | `supra_juiz` | Não fornece alimentação; o lote carrega. (`app/ponto/management/commands/supra_juiz.py:108`) |
   | Emissor retroativo | Fornece apenas `agora`; o lote carrega. (`app/ponto/management/commands/emitir_furo_retroativo.py:62`) |
   | `par_completo_da_janela` | Não fornece alimentação e depois carrega turnos novamente. (`app/ponto/turnos.py:2151-2153`) |
   | Calendário | Fornece células com chave composta e turnos previamente carregados; omite `escalas`. (`app/escala/alimentacao.py:41-46`; `app/colaboradores/services/calendario.py:709-710`) |

   **Chaves incompatíveis no cartório — achado herdado.** O lote espera chave `(colaborador_id, data)`, mas repassa o mesmo argumento a `turnos_do_colab`, cujo contrato é `{data: célula}`. Este recompõe a chave como `(colaborador_id, chave_recebida)`, produzindo `(colaborador_id, (colaborador_id, data))`; `vinculo_do_dia` procura `(colaborador_id, data)` e perde a geradora. A passagem sem adaptação já constava da versão anterior mostrada no anexo. (`app/ponto/turnos.py:2114,2133,1852,1928`; `app/escala/alimentacao.py:237-254`; anexo `diff2.txt`, hunk de `dias_encerrados`)

   **Células com `dna=NULL`, vazio ou sem marcos: sem achado nessa regra.** Com mapa ou consultando sozinho, `marcos_do_dia` recorre ao template. O carregador normal traz o campo `dna`; campo diferido é outra situação, advertida pelo próprio contrato como possível N+1. (`app/escala/models.py:1089-1108`; `app/escala/alimentacao.py:41-46`)

   **Turnos de janela diferente — achado no reaproveitamento novo.** O calendário forma turnos para `[data_ini, data_fim]`, mas os entrega ao lote perguntado somente até hoje, contrariando a exigência documentada de “MESMA janela”. O lote aceita a lista sem reconstruí-la. (`app/colaboradores/services/calendario.py:310-311,707-710`; `app/ponto/turnos.py:2113-2114,2133`)

   Contraexemplo derivado do código: vínculo único 21h–05h, sem intervalos; única entrada em D às 01h; nenhuma célula em D−1/D; único DNA com marcos em D+2; agora D às 06h; calendário `[D−1,D+3]`. Na carga curta, o mapa DNA fica vazio e vira `None`: a entrada ancora em D−1, cuja aberta vive até D às 09h. Na carga larga, o DNA de D+2 torna o mapa não vazio; a falta de D−1 nesse mapa elimina as bordas da véspera, e a entrada ancora em D. Assim, D−1 fica sem aberta e passa a encerrado às 06h. **A resposta muda com a janela.** (`app/escala/utils.py:100-107`; `app/ponto/turnos.py:1845,1949,1297-1300,283-306,1495-1496,1992,2008-2011,2102-2105`)

   Portanto, nem os chamadores sem alimentação têm equivalência universal com a consulta unitária: o pareamento conserva essa dependência da janela. Execução do contraexemplo e ocorrência em produção: **[nao verificado]**. (`app/ponto/turnos.py:2079-2080,2133`; traço acima)

3. **Os três motivos e `sem_previsto`**

   **É regra própria do leitor.** A autoridade distingue quatro motivos de não encerramento; para ela, `sem_previsto` continua sendo não encerrado. O calendário seleciona três e deixa o quarto seguir pelo caminho dos encerrados. Isso não equivale a ler a resposta booleana da juíza e conflita com a LEI-AKITA 2. (`app/ponto/turnos.py:2073-2075,2102-2105,2140`; `app/colaboradores/services/calendario.py:704-712`; `CLAUDE.md:20-22`)

   A exceção está declarada no comentário local, mas o teste ainda promete genericamente “turno NAO encerrou → previsto”. Declaração local não modifica o julgamento da autoridade. (`app/colaboradores/services/calendario.py:702-703`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:1-5`)

4. **Custo adicional**

   **Conta estática da cadeia nova: +1 query por calendário/card** quando `data_ini <= min(data_fim, hoje)`; **zero** para janela inteiramente futura. Células e turnos são reaproveitados, mas `escalas=` não é passado: o lote consulta todos os vínculos com `select_related('tipo_escala')`. Não há consulta adicional por dia para ler o DNA nesse caminho. (`app/colaboradores/services/calendario.py:707-710`; `app/ponto/turnos.py:2129-2139`; `app/escala/models.py:1094-1108`)

   No mapa HTML, são **+N queries para N cards efetivamente montados**, nessa condição. No PDF, são os mesmos **+N**, pagos antes da bifurcação de renderização; a geração reutiliza os cards. (`app/relatorios/furos.py:164-188`; `app/relatorios/views.py:1541,1548-1551,1560-1566`)

   Esse é o incremento estático atribuível à obra identificável na cópia. Medição efetiva e comparação integral com a árvore de `d2c37edd`: **[nao verificado]**.

5. **Leitores e efeitos além de “falta → previsto”**

   A declaração local concentra-se em “previsto, não falta”, mas o classificador também permite `ok → previsto`, `escala_suspeita → previsto`, `escala_suspeita → sem_dados` no limbo e bloqueio da sobreposição de veredito. Para HOJE/ONTEM com `sem_previsto`, a exclusão nova permite os sentidos inversos em relação à volta anterior. (`app/colaboradores/services/calendario.py:132-152,177-178,697-703,805`; anexo `diff2.txt`, hunk de `_em_curso`)

   | Leitor | Efeito |
   |---|---|
   | Perfil e gaveta | Herdam status, ícone e tooltip. **Tooltip corrigido: sem achado.** (`app/colaboradores/views.py:1946`; `app/colaboradores/services/drawer.py:121`; `app/colaboradores/services/calendario.py:881-882`; `app/templates/colaboradores/partials/_calendario_grade.html:99-105`) |
   | Furos HTML/PDF | Pintura muda; seleção, `nfuro`, marcos faltantes e ordenação seguem a cadeia independente da grade. (`app/relatorios/furos.py:141-190`; `app/relatorios/views.py:1549-1551`; `app/relatorios/furos_pdf.py:56-69`) |
   | `resolver_dia_oferecido` | `previsto` não pede decisão: datas passadas podem perder a ação, conforme os demais filtros. (`app/colaboradores/services/calendario.py:190,198-199,895-896,925-926`) |
   | `vigia_fio` | Pode trocar `decisao_do_dia_pendente` por um dos ramos seguintes, inclusive `fio_morto`. Ocorrência real: **[nao verificado]**. (`app/chamados/services/vigia_fio.py:294-324`) |
   | Rodapé | Totais acompanham o status; `previstos` são contados, mas não exibidos. (`app/colaboradores/services/calendario.py:930-946`; `app/templates/colaboradores/partials/_calendario_grade.html:137-173`) |
   | `status_do_dia` e contagem | Herdam a alteração ao remontarem o calendário; a diferença de janela acima também impede garantir igualdade entre consulta unitária e grade larga. (`app/colaboradores/services/calendario.py:213-233`; `app/ponto/turnos.py:1297-1300`) |

Execução dos testes Django, contagem medida de queries e ocorrência desses cenários em produção: **[nao verificado]**. [nao verificado]

## 30/10 14:22 · sessao d3f052a57a8b · - · - · origem=Ronald (tmux)

**P2: não há equivalência universal.** O calendário reaproveita turnos de janela maior; o cartório continua passando chaves incompatíveis ao pareador. (`app/colaboradores/services/calendario.py:310,707-710`; `app/ponto/services/cartorio.py:853`; `app/ponto/turnos.py:1852,1928,2133`)

| Chamador | Alimentação e resultado da inspeção |
|---|---|
| Cartório | Passa `{(cid,data): célula}`; `dias_encerrados` repassa sem conversão a `turnos_do_colab`, que espera `{data:célula}` e transforma as chaves em `(cid,(cid,data))`; a geradora deixa de ser encontrada pelo pareador. É defeito herdado: o anexo mantém essa chamada antiga. (`app/ponto/services/cartorio.py:853`; `app/ponto/turnos.py:1852,1928-1932,2133`; `app/escala/alimentacao.py:237-247`) |
| Supra_juiz | Não fornece alimentação: lote carrega mapa composto para vínculo/fim e `turnos_do_colab` carrega separadamente seu mapa por data. Sem achado novo nesse contrato. (`app/ponto/management/commands/supra_juiz.py:108`; `app/ponto/turnos.py:2129-2139,1880-1882`) |
| Emissor retroativo | Mesmo caminho sem alimentação; sem achado novo nesse contrato. (`app/ponto/management/commands/emitir_furo_retroativo.py:62`; `app/ponto/turnos.py:2129-2139`) |
| `par_completo_da_janela` | Mesmo caminho sem alimentação; depois carrega turnos novamente. Sem achado novo nas chaves. (`app/ponto/turnos.py:2151-2154`) |
| Calendário | Mapa composto correto para fim/vínculo; evita repassar esse mapa ao pareador porque fornece `turnos=`. Porém os turnos vêm de `[data_ini,data_fim]`, enquanto o julgamento é solicitado até `min(data_fim,hoje)`. (`app/colaboradores/services/calendario.py:310,404,707-710`) |

**Contraexemplo lógico da janela diferente:** EC único noturno 21–05, sem intervalos; única batida E em D às 01h; agora D às 06h; nenhuma célula em D−1/D; uma célula com DNA em D+2; calendário `[D−1,D+3]`. São entradas hipotéticas, ocorrência em produção **[nao verificado]**.

- Carga curta `[D−1,D]`: seu pad termina D+1; não encontra DNA, então o mapa vira `None`. `_vespera` usa o template e ancora E01 em D−1. A aberta permanece viva até D09; juiz responde não encerrado. (`app/ponto/turnos.py:1845,1949,1297-1300,297-306,1992,2008-2011,2097-2100`)
- Carga larga do calendário: encontra DNA D+2, deixando o mapa não vazio; como D−1 não consta, `_vespera` devolve `None`, e E01 fica em D. D−1 fica sem aberta; às D06, seu fim D05 passou, e o lote alimentado responde encerrado. (`app/escala/utils.py:100-107`; `app/ponto/turnos.py:1297-1299,283-286,1495-1496,1973-1976,2102-2105`)

Isso demonstra logicamente que turnos carregados em outra janela podem alterar o encerramento do dia; execução do contraexemplo **[nao verificado]**. (`app/ponto/turnos.py:1297-1300,1949`)

**Célula sem DNA: sem achado.** Tanto alimentação quanto carga individual chegam à mesma queda para `TipoEscala.marcos_do_dia`; `None`, `{}` ou ausência de `marcos` não inventam fim. (`app/escala/models.py:1094-1108`) A carga comum traz todos os campos, evitando DNA diferido. (`app/escala/alimentacao.py:41-46`)

**`escalas=`: sem achado nos chamadores atuais**, que não o fornecem; o lote carrega todos os vínculos com tipo relacionado. O contrato exige lista completa, pois geradora ausente devolve `None`. (`app/ponto/turnos.py:2114,2130-2131`; `app/escala/alimentacao.py:232-250`; chamadas citadas acima)

O teste de propriedade atual só compara lote sem alimentação com consulta individual; não cobre `turnos=`, `escalas=`, `motivos=True`, chaves do cartório nem o contraexemplo de janela. (`app/ponto/tests/test_c1_dias_encerrados.py:26-51`)

## 30/10 14:21 · sessao 9e672f61d1e5 · - · - · origem=Ronald (tmux)

1. **Sem achado no antigo teto HOJE/ONTEM:** consulta `data_ini..min(data_fim, hoje)`, incluindo D−2; o teste novo contempla encadeamento até D+2. Existe limite de 366 dias para a janela customizada, separado do julgamento de encerramento. (`app/colaboradores/services/calendario.py:296-298,707-712`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:47-55`)

2. **Ainda há “não encerrado” ignorado:** `sem_previsto` significa que a juíza não afirma encerramento; `dias_encerrados` retorna `False` para esse motivo. O calendário exclui explicitamente esse motivo de `_em_curso`, podendo chegar a `falta` em trabalho sem batida. Não é equivalência com a resposta booleana da juíza. (`app/ponto/turnos.py:2047-2050,2102-2105,2140`; `app/colaboradores/services/calendario.py:702-712,132-142,805`)

3. **A seleção dos três motivos é regra própria do leitor:** o ramo `sem_previsto` não virou encerrado na autoridade; o calendário lhe aplica o mesmo caminho aplicado a encerrados. Isso conflita com a exigência de leitor sem regra própria de classe e com a promessa do teste de que trabalho cujo turno “NAO encerrou” recebe `previsto`. A exceção está declarada no comentário local, mas declaração não altera a autoridade. (`CLAUDE.md:20-22`; `app/ponto/turnos.py:2102-2105`; `app/colaboradores/services/calendario.py:702-712`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:1-5`)

4. **Efeitos nos leitores:**

| Leitor | Efeito |
|---|---|
| Perfil/gaveta | Herdam mudança de status, ícone e tooltip; o tooltip em curso foi corrigido. (`app/colaboradores/views.py:1946`; `app/colaboradores/services/drawer.py:121`; `app/colaboradores/services/calendario.py:881-882`; `app/templates/colaboradores/partials/_calendario_grade.html:99-105`) |
| Furos HTML/PDF | Calendário/pintura mudam; seleção dos cards, `nfuro`, lista de marcos faltantes e ordenação continuam independentes, lendo `grade_da_celula → estado_do_dia`. (`app/relatorios/furos.py:141-190`; `app/relatorios/views.py:1549-1551`; `app/relatorios/furos_pdf.py:56-69`) |
| Resolver dia/DP | `previsto` não pede decisão. Datas passadas em curso podem perder ação; `sem_previsto` hoje/ontem pode percorrer o sentido inverso em relação ao delta anterior, se os demais filtros permitirem. (`app/colaboradores/services/calendario.py:190,198-199,895-896,925-926`; `app/templates/colaboradores/partials/_calendario_grade.html:125-126`) |
| Vigia | Perder a ação faz deixar `decisao_do_dia_pendente`; pode terminar em `fio_morto` ou outro balde subsequente. Ocorrência real **[nao verificado]**. (`app/chamados/services/vigia_fio.py:294-324`) |
| Rodapé | Totais `ok`, `faltas`, `escala_suspeita`, `previstos` acompanham status; previstos são contados, mas não aparecem no rodapé. (`app/colaboradores/services/calendario.py:930-946`; `app/templates/colaboradores/partials/_calendario_grade.html:137-173`) |
| `status_do_dia` / `contar_por_status` | Herdam integralmente a alteração, pois remontam o calendário. (`app/colaboradores/services/calendario.py:213-233`) |

5. **Alcance além de falta:** para datas antigas agora reconhecidas em curso, pode ocorrer `ok → previsto`, `escala_suspeita → previsto`, `escala_suspeita → sem_dados` no limbo, bloqueio de veredito e retirada do rótulo de escala suspeita. Para HOJE/ONTEM com `sem_previsto`, a exclusão nova permite os sentidos inversos em relação à volta anterior. A exclusão está declarada localmente; os demais efeitos não estão descritos na declaração da obra, que promete “previsto, nao falta”. (`app/colaboradores/services/calendario.py:132-152,177-178,697-703,805,894`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:1-5`)

Execução dos testes e ocorrência dos cenários em produção: **[nao verificado]**. [nao verificado]

## 30/10 14:20 · sessao 77b51e13bbae · - · - · origem=Ronald (tmux)

Custo novo do julgamento no calendário, por inspeção estática:

- **+1 query por calendário/card** quando `data_ini <= min(data_fim, hoje)`; **zero** quando a janela é inteiramente futura. A chamada passa `celulas` e `turnos`, mas omite `escalas`; `dias_encerrados` executa uma consulta com `select_related('tipo_escala')` para todos os vínculos. (`app/colaboradores/services/calendario.py:707-710`; `app/ponto/turnos.py:2129-2133`)
- Esse `+1` poderia alimentar dados já obtidos no calendário: `_esc_cal` foi carregado por `escalas_do_periodo(..., mais=_ger_cal)`, mas não é encaminhado à nova chamada. O parâmetro novo exige todos os vínculos; equivalência de reutilizar a lista filtrada precisa avaliação própria. (`app/colaboradores/services/calendario.py:404-406,432-434,709-710`; `app/ponto/turnos.py:2113-2114`)
- **Não há query por dia de `fim_turno` no caminho alimentado do calendário**, inclusive célula cujo `dna` é nulo: o carregador de células não usa `.only()`, o mapa fornece a instância completa e o método lê `cel.dna`; sem marcos, consulta o `tipo_escala` já trazido pelo `select_related`, em memória. (`app/escala/alimentacao.py:41-46`; `app/escala/models.py:1094-1108`; `app/ponto/turnos.py:2131,2138-2139`; `app/escala/servico_jornada.py:68-71,89-93`)
- **HTML: +N queries para N cards efetivamente montados**, sob a condição de janela acima. O calendário é chamado somente depois de `nfuro or not so_com_furo`. (`app/relatorios/furos.py:164-188`; `app/colaboradores/services/calendario.py:707-710`; `app/ponto/turnos.py:2130-2131`)
- **PDF: os mesmos +N no request, sem segunda cobrança do julgamento na geração do PDF**: os cards são montados antes da bifurcação HTML/PDF; o PDF recebe os cards e lê suas semanas. (`app/relatorios/views.py:1541,1548-1551,1560-1566`; `app/relatorios/furos_pdf.py:47-56,73-84`)
- A conta anterior `6 + E + M + U + K` não descreve a chamada atual do calendário: seus dois carregamentos caros estão bypassados pelos argumentos fornecidos e o DNA chega até o predicado. (`app/colaboradores/services/calendario.py:709-710`; `app/ponto/turnos.py:2129-2139`)

**Medição efetiva e comparação integral com a árvore de `d2c37edd`: [nao verificado].** O número `+1` descreve a nova cadeia identificável na cópia; a base completa não está acessível nesta árvore nem nos anexos fornecidos. **[nao verificado]**

## 30/10 14:12 · sessao ef4ba8f661a1 · CODEX-ASTRA-AB · verificador · origem=Code

**Veredito da cópia: REPROVADO.** Há reabertura automática que contorna o veto, e o selo não detecta esse caminho; a garantia de abrir cadastro para ativo sem vínculo também não está cumprida (`app/chamados/services/disputa_emissao.py:1606`; `app/chamados/tests/test_o261_veto_na_porta.py:212`; `app/ponto/management/commands/detectar_ausencias.py:139`).

Identidade do commit, comparação Git com `d2c37edd`, suíte Django, ocorrência dos contraexemplos em produção e execução da relavratura: **[nao verificado]**. A análise abaixo é da cópia disponível e dos anexos.

**1. Quem escreve, quem lê e efeito de `ec=None`**

O registro vivo nomeia `motivo_do_veto` como juiz; o corte contém a autorização exigida. O próprio `JUIZES.md` determina que o registro vivo prevalece (`app/core/juizes.py:248`; `app/docs/CORTES.md:401`; `app/docs/JUIZES.md:10`).

Os leitores de produção encontrados para o resultado de `motivo_do_veto` são:

| Leitor | Efeito quando `ec=None` passa a retornar `VETO_DEGENERADA` |
|---|---|
| Porta `_veto_da_regua` | Recusa nascimento por `abrir`/`criar` e renascimento por `_renascer_por_premissa_viva`, havendo dia forte. Sem dia, continua permitindo (`app/chamados/models.py:731`, `:778`, `:838`, `:891`, `:896`). |
| Cartório `_veto_do_dia` | Acrescenta o veto à impressão; pode desfazer a igualdade que dispensava rejulgamento. O resultado não é passado diretamente como veredito ao classificador (`app/ponto/services/cartorio.py:1013`, `:1083`, `:1092`, `:1108`). |
| `furos_sem_cobranca.medir` | Classifica o item como `porta='veto'`, retirando-o do total cobrável, mas mantendo-o em `itens`/`por_porta`. As exclusões anteriores — chamado vivo, tranca e afastamento — continuam prevalecendo (`app/ponto/services/furos_sem_cobranca.py:30`, `:83`, `:96`, `:109`). |
| C2 `reemitir_chamado_do_dia` | Recusa emissão/renascimento. Quando já há chamado vivo, deliberadamente não consulta esse veto e pode costurar nele (`app/chamados/reconciliador.py:807`, `:852`, `:854`, `:855`). |
| `furos_vetados_por_regua` | Inclui furo sem vínculo, com `vinculo=None` e `corroborada_em=None`; continua excluindo desligado (`app/ponto/services/furos_vetados.py:61`, `:65`, `:69`). |
| `chamados_vivos_em_dia_vetado` | Conta chamado vivo do universo com dia forte e vínculo ausente; não fecha nem modifica o chamado. Sem dia vai para contador separado (`app/ponto/services/furos_vetados.py:95`, `:101`, `:106`, `:110`). |

As cadeias de produção e consumo dos demais valores afetados são:

| Campo/valor | Quem fornece/escreve → quem lê |
|---|---|
| `VETO_DEGENERADA`, `VETO_REGUA`, `None` | `motivo_do_veto` compõe `regua_degenerada` e o veto de suspeita; não grava banco → leitores da tabela anterior (`app/ponto/services/furos_vetados.py:33`). |
| Universo de módulos | `FURO_MODULES` e `MODULOS_DE_TURNO` → união em `modulos_do_veto` → porta, vigia e teste dos cinco módulos (`app/chamados/catalogo/furo_modulos.py:15`; `app/chamados/reconciliador.py:336`; `app/ponto/services/furos_vetados.py:79`; `app/chamados/models.py:889`; `app/chamados/tests/test_o261_veto_na_porta.py:383`). |
| Dia, `contexto_json`, `celula` | Emissores fornecem contexto; porta resolve a célula; `dia_do_fato(fortes=True)` prioriza célula e chaves do catálogo/contexto → porta e vigia (`app/chamados/models.py:718`, `:725`, `:830`; `app/chamados/juizes.py:89`, `:104`; `app/ponto/services/furos_vetados.py:101`). |
| Vínculo do dia, `escala_geradora`, vigência | Porta de célula grava geradora; carregador busca célula/vínculos e delega a `alimentacao.vinculo_do_dia` → `vinculo_do_emissor` → juiz. Não encontrei nova escolha paralela de vínculo nessa cadeia (`app/ponto/portas/celula.py:248`, `:260`; `app/escala/servico_jornada.py:46`, `:57`; `app/escala/alimentacao.py:196`; `app/ponto/services/furos_vetados.py:23`). |
| Template e avaliação da régua | `TipoEscala.marcos_do_dia` alimenta a degeneração; avaliação/corrobor ação gravam estado da régua → veto de suspeita lê avaliação, faixa e avaliações consecutivas (`app/escala/models.py:326`; `app/escala/regua_defesa.py:23`, `:26`, `:560`, `:644`, `:666`, `:765`; `app/colaboradores/services/vinculo.py:889`). |
| `status_local`, retorno de nascimento | Nascimentos continuam nos dois gates; transições continuam em `_gravar_estado` → dedup, emissores, C2 e vigia leem estado/retorno (`app/chamados/models.py:192`, `:670`, `:735`, `:817`; `app/chamados/reconciliador.py:885`, `:888`; `app/ponto/services/furos_vetados.py:95`). |
| Trilha: ação, modelo, objeto, `modulo_origem`, `dia`, `veto`, `via`, colaborador | `_trilha_do_veto` monta e consulta a chave; `core.observ.evento` escreve `LogAuditoria` → deduplicação e teste leem a trilha (`app/chamados/models.py:915`, `:921`, `:927`; `app/core/observ.py:134`; `app/chamados/tests/test_o261_veto_na_porta.py:359`). |
| Impressão, ata e veredito | `impressao_insumos` incorpora veto; cartório classifica e pede lavra; porta de célula grava → próxima passagem compara impressão (`app/ponto/services/cartorio.py:329`, `:1092`, `:1409`, `:1424`; `app/ponto/portas/celula.py:647`, `:708`). |
| Achados e contadores de veto | Serviços produzem dicionários → comando imprime; leitor de desfazer corroboração filtra pelo vínculo específico, descartando naturalmente `None` (`app/ponto/services/furos_vetados.py:69`, `:108`; `app/ponto/management/commands/furos_vetados_por_regua.py:29`, `:34`, `:44`; `app/escala/management/commands/desfazer_corroboracao_regua.py:38`). |
| `VETO_NA_PORTA` | Detector/grade devolvem texto → detector conta por chave, grade conta no plano e cartório conta veto, sem anunciar emissão (`app/ponto/management/commands/detectar_ausencias.py:733`, `:440`; `app/ponto/management/commands/reconciliar_grade.py:520`, `:254`; `app/ponto/services/cartorio.py:1497`). |
| `recusado_veto` | Retroativo consulta `veto_no_nascimento` no dry; no apply distingue retorno recusado; imprime total local (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:138`, `:174`, `:186`). |
| Fixtures: tipo, posto e vínculo | `dar_vinculo` cria/reutiliza template e posto e cria `EscalaColaborador` ativa desde 2020 → resolução normal de vínculo e juiz (`app/chamados/tests/_vinculo_do_dia.py:13`, `:19`, `:23`; `app/escala/servico_jornada.py:58`). |

Detector e grade ainda consultam os predicados anteriores antes da porta; C2 ainda consulta o novo juiz por fora. Isso está explicitamente permitido pela allowlist do pouso A, portanto **não classifico essas três permanências, isoladamente, como achado contra esse pouso** (`app/chamados/tests/test_o261_veto_na_porta.py:46`).

Há três achados nessa cadeia:

- **Impressão incompleta no ramo “nunca bateu”.** Esse ramo chama `impressao_insumos` sem `veto`, compara o hash e retorna antes do leitor do juiz. **Contraexemplo:** ativo sem adesão, célula carimbada, `ec=None` e demais insumos iguais: continua sendo pulado; o novo veto não entra em sua impressão. Esse ramo não emite cobrança por desenho; a falha é de cobertura da impressão, não prova de cobrança indevida (`app/ponto/services/cartorio.py:903`, `:907`, `:941`, `:943`, `:1003`, `:1013`).

- **Ato humano passa sem registrar o veto dispensado.** Desarquivar registra resgate, mas não consulta nem registra `VETO_DEGENERADA`; a trilha nova registra recusas. **Contraexemplo:** desarquivar chamado encerrado pela competência, com dia conhecido sem vínculo: há trilha de resgate, mas falta a informação de que o humano atravessou um veto. O próprio vigia documenta essa pendência (`app/chamados/views_arquivar.py:68`, `:75`, `:79`; `app/chamados/models.py:915`; `app/ponto/services/furos_vetados.py:90`).

- **Deduplicação da trilha não distingue a operação prometida.** A documentação promete uma linha por operação e grava `via`, mas o filtro não consulta `via`. **Contraexemplo:** `abrir` recusado e depois `criar` recusado para o mesmo colaborador/célula, módulo, dia e veto: a segunda operação fica sem linha própria (`app/chamados/models.py:908`, `:921`, `:927`).

**2. Fixtures migradas**

**Intenção “sem vínculo/sem escala” mascarada pelo helper: sem achado.** O caso explicitamente sem escala permanece sem ela: não chama o helper de chamado agrupado que passou a conceder vínculo. “Tranca sem cadastro” trata avisos de cadastro fora da tranca, não colaborador sem vínculo (`app/chamados/tests/test_selo_f4_fatia4.py:45`, `:69`, `:131`; `app/chamados/tests/test_tranca_sem_cadastro.py:9`).

Há vacuidades em testes tocados, **já presentes no corpo anterior mostrado pelo diff**, sem evidência de que tenham sido introduzidas pelo helper: [nao verificado]

| Teste | O que não prova e contraexemplo | [nao verificado]
|---|---|
| `test_MORDE_desligado_segue_o_gate_do_c2` | O desligado recebe vínculo, mas não recebe a célula criada para o outro colaborador. Remover o gate de situação ainda permite que a ausência de protesto impeça emissão; as asserções continuam satisfeitas (`app/chamados/tests/test_emissao_reata_orfa.py:31`, `:67`, `:71`, `:73`; `app/chamados/reconciliador.py:789`, `:834`, `:836`). |
| `test_sem_celula_o_chamado_ainda_nasce` | Só chama `_ctx_c2` e examina o dicionário. Uma porta que recusasse todos os nascimentos não seria observada pelo teste (`app/chamados/tests/test_emissor_nomeia_o_marco.py:88`, `:92`; `app/chamados/reconciliador.py:862`). |
| `test_sem_ata_possivel_o_chamado_nasce_sem_marco_e_nao_deixa_de_nascer` | Também só verifica o contexto. Recusar a abertura depois da montagem desse contexto preservaria suas asserções (`app/chamados/tests/test_bug125_cobranca_cega.py:176`, `:182`; `app/chamados/reconciliador.py:862`). |

O teste novo sem vínculo efetivamente pede abertura, espera `(None, False)` e compara com nascimento com vínculo; os testes de deduplicação também exigem chamado efetivo. Nesses controles: **sem achado** (`app/chamados/tests/test_o261_veto_na_porta.py:395`, `:404`, `:406`; `app/chamados/tests/test_3c_abrir_ausencia.py:52`; `app/chamados/tests/test_f3_dedup_atraso.py:57`).

Estado verde da suíte: **[nao verificado]**. [nao verificado]

**3. Quem abre `regua_degenerada` para ativo sem vínculo**

O escritor existe: `vetar_cobranca_se_degenerada` transforma `ec=None` em template ausente e abre `regua_degenerada`, com código `SEM_ESCALA`. Os chamadores são detector e grade (`app/escala/regua_defesa.py:23`, `:45`, `:50`, `:51`; `app/ponto/management/commands/detectar_ausencias.py:599`; `app/ponto/management/commands/reconciliar_grade.py:486`).

**Achado: existir o escritor não garante que o ativo sem vínculo o alcance.**

- Detector pula vínculo ausente antes do emissor (`app/ponto/management/commands/detectar_ausencias.py:139`).
- Cartório integral exige vínculo; parcial retorna sem ele (`app/ponto/services/cartorio.py:1347`, `:1446`).
- Retroativo de furo pula vínculo ausente (`app/ponto/management/commands/emitir_furo_retroativo.py:118`).
- Grade exclui template ausente da fila de emissão (`app/ponto/management/commands/reconciliar_grade.py:157`; `app/ponto/marcos.py:107`).
- Emissores de turno também saem sem vínculo (`app/ponto/management/commands/processar_alertas_avancados.py:37`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:104`).
- Juiz e nova porta somente recusam; não produzem o cadastro (`app/ponto/services/furos_vetados.py:40`; `app/chamados/models.py:896`).

**Contraexemplo:** ativo em operação, sem qualquer vínculo nem geradora de célula, sem aviso de cadastro anterior. Os fluxos acima saltam a emissão; chamar a nova porta com dia conhecido apenas veta. Não nasce `regua_degenerada` por esses caminhos. A garantia do aval não está cumprida na cópia; não atribuo a origem dessa lacuna ao diff (`app/ponto/management/commands/detectar_ausencias.py:139`; `app/ponto/services/cartorio.py:1348`; `app/chamados/models.py:896`).

**4. Há nascimento/reabertura automática que escapa?**

**Sim: duas rotas de reabertura.**

**A. Resposta pendente reabre diretamente.** O resolver chama `_pai.reabrir()` ao encontrar resposta aguardando validação. Ele é acionado automaticamente pelo signal e pelo reconciliador; não passa por `_renascer_por_premissa_viva` (`app/chamados/services/disputa_emissao.py:1598`, `:1604`, `:1606`; `app/chamados/signals.py:92`; `app/chamados/reconciliador.py:538`).

**Contraexemplo:** pai histórico `entrada_bloqueada_turno_aberto`, resolvido, dia conhecido sem vínculo, sem agrupamento/irmão vivo/encerramento administrativo, categoria sem classificação de furo; disputa aberta com resposta não validada nem materializada creditada ao pai. O juiz de respostas aceita o caso; `reabrir` grava estado vivo sem perguntar o veto. O módulo pertence ao universo, embora possa ficar fora da guarda específica de premissa de furo (`app/chamados/juizes.py:1471`; `app/chamados/catalogo/furo_modulos.py:15`; `app/chamados/models.py:984`, `:1078`, `:1158`).

**B. A grade pode desfazer a recusa da porta.** `abrir` devolve o existente encerrado mesmo quando `_renascer_por_premissa_viva` recusa. A grade só trata `None`; recebendo encerrado, chama `reabrir()` diretamente (`app/chamados/models.py:686`, `:688`; `app/ponto/management/commands/reconciliar_grade.py:519`, `:521`, `:522`).

**Contraexemplo:** `batida_ausente` resolvido com marco ainda faltante; template pré-carregado era 08–17 e muda para 08–08 antes da abertura. O veto externo lê o objeto antigo; a porta recarrega o vínculo e veta; a grade recebe o encerrado e o reabre sem veto. A trava de `abrir` é no colaborador, não no template (`app/ponto/management/commands/reconciliar_grade.py:189`, `:196`, `:487`; `app/chamados/models.py:669`, `:778`, `:896`).

**O selo fica verde apesar dessas fugas.** A varredura pura em memória encontrou exatamente as duas escritas e três cópias permitidas, mas a função real do caminho A não apareceu. O selo reconhece operações de gerente/construtor; não acompanha a reabertura pelo receptor `_pai`. Portanto a prova atual não sustenta “toda reabertura automática passa pelo veto” (`app/chamados/tests/test_o261_veto_na_porta.py:40`, `:46`, `:207`, `:212`, `:216`; `app/chamados/services/disputa_emissao.py:1606`).

Nos demais critérios solicitados:

| Classe | Resultado |
|---|---|
| Segundo juiz novo / derivação paralela nova de vínculo ou universo | **Sem achado** na composição nova; usa autoridades existentes e união declarada (`app/ponto/services/furos_vetados.py:23`, `:33`, `:79`). |
| Segundo escritor novo de estado | **Sem achado**; as fugas usam o escritor existente por uma entrada que contorna a guarda (`app/chamados/models.py:192`, `:1158`). |
| Ordem de trava | **Sem achado de inversão introduzida**; permanece a trava de colaborador antes da leitura/dedup. Ensaio concorrente: **[nao verificado]** (`app/chamados/models.py:666`, `:669`). |
| Relógio novo fora da borda | **Sem achado no novo juiz/gate**: recebem ou derivam o dia do fato, sem consultar “agora” (`app/ponto/services/furos_vetados.py:27`; `app/chamados/models.py:891`). |
| Custo de frota no request | **Sem achado dessa classe**; há leitura adicional por dia na porta, pelo carregador de vínculo. Custo medido/latência: **[nao verificado]** (`app/chamados/models.py:896`; `app/escala/servico_jornada.py:46`, `:58`). |
| Pouso antes dos pré-requisitos financeiros | O novo hash pode provocar relavra, e o cron seleciona células passadas sem recorte de competência nessa seleção. Reversão anterior, DIFF financeiro, preservação de exportadas por hash e encaminhamento à Pauta DP: **[nao verificado]** (`app/ponto/services/cartorio.py:1083`, `:1092`, `:1409`; `app/ponto/management/commands/processar_cartorio.py:49`, `:61`). |

## 30/10 14:10 · sessao efaf950742da · - · - · origem=Ronald (tmux)

Varredura completa do selo executada em memória, sem Django, banco ou escrita. Foram usados o AST real de `varrer` e o valor real de `VIVOS` (`app/chamados/tests/test_o261_veto_na_porta.py:225`; `app/chamados/catalogo/motor.py:24`).

Resultado:

- Escritas: `0038_a3_registrado_backfill.py::desfazer` e `::registrar`.
- Cópias: `reemitir_chamado_do_dia`, `detectar_ausencias.Command._abrir_chamado`, `reconciliar_grade.Command._emitir_furo`.
- Ambas as listas coincidem exatamente com suas allowlists; catraca `5 <= 5`; as duas portas foram encontradas (`app/chamados/tests/test_o261_veto_na_porta.py:40`, `:46`, `:54`, `:244`).

Isso confirma que o selo passa suas comparações atuais, embora não detecte a reabertura automática por `_pai.reabrir()` do HX: o corpo real do resolver e o mutante mínimo retornaram `([], [])`, coerentemente com o descarte do receptor não reconhecido (`app/chamados/tests/test_o261_veto_na_porta.py:207`, `:212`; `app/chamados/services/disputa_emissao.py:1606`).

Não foi executada a suíte Django; comportamento de integração e ocorrências em produção: [nao verificado]. [nao verificado]

## 30/10 14:09 · sessao efaf950742da · - · - · origem=Ronald (tmux)

Achados confirmados por leitura:

1. **Reabertura automática foge do veto pelo HX.** `resolver_perguntas_por_chamado_resolvido` chama `_pai.reabrir()` diretamente quando há resposta aguardando validação, sem consultar `_renascer_por_premissa_viva` nem `motivo_do_veto` (`app/chamados/services/disputa_emissao.py:1598`, `:1604`, `:1606`). O caminho é automático: receiver de `post_save` chama o resolver (`app/chamados/signals.py:75`, `:89`, `:92`); o reconciliador também o chama (`app/chamados/reconciliador.py:538`).

   Contraexemplo concreto construído por leitura: pai histórico `entrada_bloqueada_turno_aberto`, `resolvido`, categoria/subcategoria sem classificação de furo, dia conhecido sem vínculo, sem agrupamento/irmão vivo/encerramento admin; disputa aberta com resposta não validada e não materializada creditada ao pai. O juiz das irmãs só exige carimbos/resposta/crédito (`app/chamados/juizes.py:1471`). Esse módulo está no universo do veto (`app/chamados/catalogo/furo_modulos.py:15`), mas fora de `CALADOS_POR_ISENCAO` (`app/chamados/catalogo/modulos.py:235`), permitindo evitar a guarda de premissa (`app/chamados/models.py:984`, `:1078`). `reabrir` grava estado vivo (`app/chamados/models.py:1158`) sem perguntar vínculo/veto. Ocorrência concreta em produção e execução de integração: [nao verificado].

2. **Grade pode desfazer a recusa da própria porta.** O vínculo/template é pré-carregado (`app/ponto/management/commands/reconciliar_grade.py:189`, `:196`), e o veto externo usa esse objeto (`:487`, `:491`). `abrir` chama `_renascer_por_premissa_viva`, mas devolve o existente mesmo quando renascer recusou (`app/chamados/models.py:686`, `:688`). A guarda nova só trata `ch is None`; se veio encerrado, a grade chama `ch.reabrir()` diretamente (`app/ponto/management/commands/reconciliar_grade.py:519`, `:521`, `:522`).

   Contraexemplo: chamado `batida_ausente` resolvido, célula ainda acusa marco faltante; template carregado 08–17 muda para 08–08 antes do pedido à porta. A cópia externa aprova o objeto antigo, a porta consulta vínculo/template atual e veta (`app/chamados/models.py:778`, `:896`), mas o fallback reabre sem veto. A trava de `abrir` é no colaborador (`app/chamados/models.py:669`), não no template. Ocorrência em produção/ensaio concorrente: [nao verificado].

3. **O selo AST não cobre a fuga real de reabertura por instância.** Só reconhece pedido de porta quando o receptor cita o modelo (`app/chamados/tests/test_o261_veto_na_porta.py:207`); descarta receptores como `_pai` (`:212`); as escritas reconhecidas são construtor/save ou operações create/update de gerente (`:209`, `:216`, `:218`). Executei extração das funções AST em memória com `python3 -B`, sem Django/imports locais/DB/escrita: tanto `def automatica(pai): pai.reabrir()` quanto o corpo real de `resolver_perguntas_por_chamado_resolvido` retornaram `([], [])`. Evidência de execução só nesta sessão, sem artefato: [nao verificado]. A limitação é verificável nas linhas acima.

4. **Ato humano tem trilha genérica, mas não mostra/registra o veto dispensado.** Desarquivar chama `reabrir` e grava resgate (`app/chamados/views_arquivar.py:68`, `:75`, `:79`); reabertura administrativa grava motivo/justificativa humana (`app/chamados/services/acoes_disputa.py:266`, `:270`); reabrir competência grava competência/status (`app/chamados/services/competencia_trancada.py:258`, `:261`). Nenhum desses caminhos pergunta o veto. A trilha nova só registra **recusas** (`app/chamados/models.py:915`, `:924`); o próprio contador admite que ato humano ainda não deixa trilha própria (`app/ponto/services/furos_vetados.py:89`). Contraexemplo: desarquivar chamado de competência trancada com dia sem vínculo; registra resgate, sem `VETO_DEGENERADA` ou dispensa humana desse veto.

Hipótese descartada:

- **Abertura manual via `abrir_chamado_para_colaborador` produzir `None.pk` para módulo do universo: sem achado no catálogo atual.** O serviço rejeita módulo fora de `tipos_para_colaborador` (`app/chamados/services/abertura.py:149`), que só inclui `abrivel_por_humano` (`app/chamados/catalogo/modulos.py:281`, `:306`). Os módulos do universo não declaram essa abertura (`:105`, `:107`, `:109`, `:111`, `:182`); não alcançarão `criar` por essa interface.

## 30/10 14:07 · sessao ab87115a333a · - · - · origem=Ronald (tmux)

Achado confirmado: o efeito “ativo sem vínculo segue abrindo `regua_degenerada`” não tem caminho alcançável para o caso sem vínculo em todo o período.

- O escritor existe: `vetar_cobranca_se_degenerada` transforma `ec=None` em template `None`, considera degenerada e chama `ChamadoColaborador.abrir(modulo_origem='regua_degenerada')`, com `tipo_escala_id=None` e código `SEM_ESCALA` (`app/escala/regua_defesa.py:23`, `:45`, `:51`, `:73`).
- Seus dois chamadores são `_abrir_chamado` do detector (`app/ponto/management/commands/detectar_ausencias.py:599`) e `_emitir_furo` da grade (`app/ponto/management/commands/reconciliar_grade.py:486`).
- O detector exclui `ec=None` antes de chegar ali (`app/ponto/management/commands/detectar_ausencias.py:139`).
- O cartório integral exige vínculo não nulo (`app/ponto/services/cartorio.py:1347`); seu ramo parcial retorna sem vínculo (`app/ponto/services/cartorio.py:1446`).
- O emissor retroativo de furo pula vínculo ausente (`app/ponto/management/commands/emitir_furo_retroativo.py:118`).
- A grade não coloca o dia na fila de emissão quando o template é ausente: `escala_ok(None)==False` (`app/ponto/management/commands/reconciliar_grade.py:157`, `app/ponto/marcos.py:107`).
- Os emissores de turno tampouco abrem cadastro: saem sem vínculo (`app/ponto/management/commands/processar_alertas_avancados.py:37`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:104`).
- O novo juiz é somente leitura e só retorna `VETO_DEGENERADA`; a porta também não chama o escritor de cadastro (`app/ponto/services/furos_vetados.py:40`, `app/chamados/models.py:896`).

Contraexemplo concreto por fluxo: colaborador ativo, em operação, sem qualquer `EscalaColaborador` nem geradora de célula, sem chamado prévio de cadastro; detector e retroativos saltam o dia, cartório não chama emissor, grade não enfileira emissão, porta veta o chamado de turno sem abrir `regua_degenerada`. Chamado de cadastro não nasce pelos caminhos acima; ocorrência real em produção [nao verificado]. A lacuna já pode anteceder o diff; o que fica reprovado é a garantia do aval nesta cópia.

Achado adicional de impressão: existe um ramo do cartório que não incorpora o novo veto.

- `adesao is None` entra no ramo “nunca bateu” (`app/ponto/services/cartorio.py:907`).
- Esse ramo chama `impressao_insumos` sem argumento `veto` (`app/ponto/services/cartorio.py:941`), compara o hash e pula a célula se igual (`:943`), encerrando antes do leitor de `motivo_do_veto` (`:1003`, `:1007`).
- O ramo normal consulta o juiz em `:1013`, passa seu resultado em `:1083`; `impressao_insumos` acrescenta `veto:VETO_DEGENERADA` ao hash em `:329`.

Contraexemplo: ativo sem adesão, célula já carimbada, `ec=None`, demais insumos iguais. O novo resultado do juiz não muda `imp0`; `:943` continua pulando. Se “ec=None também para impressão” abrange toda célula, o censo deixou esse leitor indireto de fora. Não há cobrança de furo nesse ramo por desenho (`app/ponto/services/cartorio.py:903`); portanto não alegar cobrança indevida como efeito.

Relavratura/dinheiro/exportada: há caminho de rejulgamento automático que percorre todas as células passadas sem recorte de competência (`app/ponto/management/commands/processar_cartorio.py:49`, `:61`, `:65`), e o hash novo do ramo normal pode provocar lavra (`app/ponto/services/cartorio.py:1083`, `:1092`, `:1409`, `:1424`). Escritores efetivos são `lavrar_veredito` (`app/ponto/portas/celula.py:689`) e `carimbar` (`:721`). Reversão prévia, aplicação financeira autorizada, hashes preservados de exportação e Pauta DP da mudança específica: [nao verificado]. Não confundir rejulgamento de célula com alteração comprovada do TXT exportado.

## 30/10 14:07 · sessao 95faa6137e78 · - · - · origem=Ronald (tmux)

**Migração “sem vínculo/sem escala”: sem achado.** O caso explicitamente “sem escala” preserva a condição: `_colab(..., com_escala=False)` só cria vínculo quando solicitado; `test_MORDE_procedencia_ignora_batida_retratada` não chama o helper `_chamado_agrupado` que ganhou `dar_vinculo` (`app/chamados/tests/test_selo_f4_fatia4.py:45`, `:50`, `:69`, `:131`, `:136`, `:139`). `TrancaSemCadastro` testa a exclusão de **avisos de cadastro** da tranca, e não ausência de vínculo (`app/chamados/tests/test_tranca_sem_cadastro.py:9`, `:39`, `:50`).

**Vacuidades encontradas em testes tocados, mas não demonstradas como introduzidas pela migração:** [nao verificado]

1. `test_MORDE_desligado_segue_o_gate_do_c2` ganha vínculo, mas continua sem célula para o desligado; a célula da fixture pertence ao outro colaborador (`app/chamados/tests/test_emissao_reata_orfa.py:30`, `:31`, `:67`, `:71`). As asserções exigem apenas destino ausente/zero chamados (`:73`, `:74`). **Contraexemplo:** remover o gate `situacao != 'ativo'` ainda deixa o caso barrado pela ausência de protesto da célula (`app/chamados/reconciliador.py:789`, `:834`, `:836`). O teste não prova o gate que nomeia.

2. `test_sem_celula_o_chamado_ainda_nasce` somente chama `_ctx_c2` e inspeciona o dicionário; não tenta abrir chamado (`app/chamados/tests/test_emissor_nomeia_o_marco.py:88`, `:92`, `:93`). **Contraexemplo:** impedir todo nascimento na porta não é observado por essas asserções; a porta é chamada separadamente pelo c2 (`app/chamados/reconciliador.py:862`).

3. A mesma limitação ocorre em `test_sem_ata_possivel_o_chamado_nasce_sem_marco_e_nao_deixa_de_nascer`: somente `_ctx_c2` e três campos do resultado são exercitados (`app/chamados/tests/test_bug125_cobranca_cega.py:176`, `:182`, `:183`). **Contraexemplo:** recusar o nascimento depois de construir contexto não altera o resultado que esse teste verifica (`app/chamados/reconciliador.py:862`, `:872`).

**Controles úteis preservados:** os testes de deduplicação exigem um chamado efetivo, evitando verde por veto total (`app/chamados/tests/test_3c_abrir_ausencia.py:52`, `app/chamados/tests/test_f3_dedup_atraso.py:57`, `:64`); o teste novo sem vínculo espera `(None, False)` e compara com nascimento com vínculo (`app/chamados/tests/test_o261_veto_na_porta.py:395`, `:404`, `:406`).

Execução da suíte e estado verde efetivo: **[nao verificado]**. [nao verificado]

## 30/10 14:04 · sessao 4573a6843b71 · CALENDARIO-HOJE · verificador · origem=Code

**Parecer: reprova pelo teto civil que dispensa o juiz para dias anteriores a ontem e pelo custo adicional por card no request.** (`app/colaboradores/services/calendario.py:702-704`; `app/relatorios/furos.py:184-188`). Execução dos testes, medição de queries e ocorrência dos contraexemplos em produção: **[nao verificado]**.

1. **Quem escreve e quem lê**

   `contexto_calendario` fornece `futuro`; `_classificar` o lê nos ramos de trabalho e de escala suspeita. O campo não é exportado: os consumidores recebem `status` e seus derivados. (`app/colaboradores/services/calendario.py:131,147,797,867-901`)

   | Leitor | Efeito da mudança |
   |---|---|
   | Perfil e gaveta | Mudam cor, ícone e apresentação pela mesma grade. (`app/colaboradores/views.py:1946`; `app/colaboradores/services/drawer.py:121`; `app/templates/colaboradores/partials/_calendario_grade.html:102-105`) |
   | Furos HTML/PDF | O calendário muda; seleção, `nfuro`, marcos faltantes e ordenação continuam na cadeia independente `grade_da_celula → estado_do_dia`. O PDF recebe o novo status para pintar. (`app/relatorios/furos.py:141,160-190`; `app/relatorios/views.py:1550-1551`; `app/relatorios/furos_pdf.py:56,66`) |
   | Resolver dia / DP | HOJE já era excluído. ONTEM em curso pode perder a ação, pois `previsto` não pede decisão, caso também satisfaça “último recurso”. (`app/colaboradores/services/calendario.py:188,196-197,886-887`) |
   | Vigia | Nesse cenário condicionado de ONTEM, deixa de contar `decisao_do_dia_pendente` e pode alcançar `fio_morto`, dependendo dos demais filtros. Ocorrência real **[nao verificado]**. (`app/chamados/services/vigia_fio.py:289-324`) |
   | Resumo/rodapé | Reduz `faltas` ou `ok` e aumenta `previstos`; previstos são contados, mas não exibidos no rodapé. (`app/colaboradores/services/calendario.py:921-937`; `app/templates/colaboradores/partials/_calendario_grade.html:141-173`) |
   | `status_do_dia` / `contar_por_status` | Herdam a mudança integralmente por montarem o calendário. Chamadas dinâmicas externas: **[nao verificado]**. (`app/colaboradores/services/calendario.py:217-231`) |

   O alcance excede `falta → previsto`: inclui `ok` do isento, `escala_suspeita`, possível `escala_suspeita → sem_dados` no limbo e bloqueio da sobreposição de veredito. O rótulo de escala suspeita também desaparece quando o status muda. (`app/colaboradores/services/calendario.py:131-150,175,885`)

   **Achado de apresentação:** HOJE/ONTEM em curso passam a receber o tooltip **“Dia futuro, ainda sem batida”**. (`app/colaboradores/services/calendario.py:55,873`; `app/templates/colaboradores/partials/_calendario_grade.html:105`)

2. **HOJE/ONTEM é teto por data: sim**

   Para datas anteriores a ontem, o calendário não pergunta ao juiz e fornece `futuro=False`. A justificativa de que todo turno anterior já encerrou não é garantida pelo juiz. (`app/colaboradores/services/calendario.py:698-704,797`; `app/ponto/turnos.py:2102-2105`)

   Há três contraexemplos derivados do código:

   - **24 horas com aberta:** início/fim às 23h representa jornada de 24h; sua aberta pode permanecer viva até D+2 às 03h, pelo fim+4h. Em D+2 às 02h, D é anterior a ontem e ainda não encerrou. Isso refuta a justificativa, embora, com batida, o status já siga `aberto` independentemente de `futuro`. (`app/escala/servico_jornada.py:95-102`; `app/ponto/turnos.py:1992,2007-2011`; `app/colaboradores/services/calendario.py:129,151`)
   - **Turno longo sem batida:** o encadeamento `E18 → S06 → E18 → S06` alcança D+2 às 06h. Consultado às 02h de D+2, o juiz ainda responde não encerrado; o corte exclui D e permite `falta`. É contraexemplo do algoritmo; presença desse DNA/template em produção **[nao verificado]**. (`app/escala/servico_jornada.py:74-85,89-92`; `app/ponto/turnos.py:2105`; `app/colaboradores/services/calendario.py:702,797,140`)
   - **Trabalho sem fim previsto:** o juiz devolve `sem_previsto`, sem prazo civil para virar encerrado. O calendário pode dar `previsto` hoje/ontem e `falta` antes disso apenas pela idade da data. Existência real **[nao verificado]**. (`app/ponto/turnos.py:2102-2105`; `app/escala/models.py:1099-1108`; `app/colaboradores/services/calendario.py:131-140,702,797`)

   Sem célula, o juiz pode recorrer ao template; ausência de célula não garante encerramento. Folga, feriado e ausência têm precedências próprias no classificador, portanto não viram necessariamente `previsto` por estarem no conjunto `_em_curso`. (`app/escala/models.py:1097-1108`; `app/colaboradores/services/calendario.py:120-128,141-146`)

3. **Relógios**

   **Sem achado na cadeia adicionada:** `_agora` produz `hoje` e é passado a `dias_encerrados`; este só lê outro relógio se receber `None`, e o julgamento usa o argumento recebido. Também não nasce outro juiz: o lote chama o predicado puro existente. (`app/colaboradores/services/calendario.py:250-251,704`; `app/ponto/turnos.py:2083-2105,2118-2132`)

   **Há relógios independentes no restante da montagem:** `folha_manda` consulta `turno_aberto_de` sem `agora` e `ate_ontem` sem `hoje`. Eles podem decidir a palavra/lista “Em aberto” com instante diferente daquele usado pelo status. Divergência efetivamente observada **[nao verificado]**. (`app/colaboradores/services/calendario.py:681`; `app/relatorios/cartao_pela_celula.py:398,412,40`; `app/ponto/turnos.py:2254-2255`)

   Há ainda relógio próprio de `pergunta_viva` para janela de competência e outro para `pode_desfazer`; são finalidades distintas do encerramento novo. (`app/colaboradores/services/calendario.py:335-339,580`; `app/chamados/juizes.py:795,1066,518`)

4. **Custo adicional e request**

   Sem interseção com hoje/ontem: **zero consultas adicionais**. Com interseção, o calendário chama o lote sem alimentar células ou turnos já carregados. (`app/colaboradores/services/calendario.py:308,402,702-704`)

   A conta estática é **`6 + E + M + U + K`**: seis consultas base; `E` para geradoras extras, `M` para mapa DNA, `U` para última batida e `K` para os dias que consultam DNA por `fim_turno`, até dois. Medição efetiva **[nao verificado]**. (`app/ponto/turnos.py:1858,1880-1886,1903-1911,1556,1949,2102,2123-2126`; `app/escala/alimentacao.py:123-127`; `app/escala/models.py:1097`)

   No cenário `E=U=0`, `M=1`, com todos os dias alcançando o fim previsto: **+8 queries para um dia; +9 para dois; +9N para N cards nessa condição**. É derivação estática, não medição. (`app/ponto/turnos.py:2123-2132`; `app/escala/utils.py:100-102`; `app/escala/models.py:1097`)

   A duplicação inclui duas cargas de células: o lote cria `cels`, mas passa o argumento original `celulas=None` a `turnos_do_colab`. HTML e PDF pagam o acréscimo durante o request, antes da bifurcação de renderização; o PDF reutiliza os cards. (`app/ponto/turnos.py:2123-2126`; `app/relatorios/furos.py:184-188`; `app/relatorios/views.py:1541,1560-1566`)

5. **Premissa do isento: sem achado funcional**

   Para HOJE às 10h, turno 08–17 sem batida, `ok → previsto` acompanha o aval: o turno não encerrou e `futuro` precede a isenção. O teste discriminante do Art. 62 continua cobrando dias encerrados separadamente. (`app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:94,298-302,272-286`; `app/colaboradores/services/calendario.py:131-134`; `app/ponto/turnos.py:2102-2105`)

   Ficaram obsoletos o nome, a descrição e a mensagem que ainda prometem verificar o caso literal `status=ok`. (`app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:288-304`)

6. **Mordida dos testes e adequação de `previsto`** [nao verificado]

   Os dois RED distinguem a expressão antiga; o MORDE impede manter ONTEM previsto depois do fim no cenário 18–06. **Execução Django: [nao verificado]**. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:23-47`)

   **A cobertura é insuficiente:** o mutante `data >= hoje or data in _em_curso` satisfaz as três expectativas e continua errado para HOJE após o fim. Faltam também turno longo, limite exato, aberta viva e fatos de ausência/cobertura/veredito. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:37-47`; `app/colaboradores/services/calendario.py:797`)

   `previsto` é adequado para trabalho sem batida e sem fato prioritário, mas não é resposta universal:

   | Fato presente | Resultado atual |
   |---|---|
   | Ausência na faixa humana | `falta` ou `ausencia`, antes de `futuro`. (`app/colaboradores/services/calendario.py:120-121`) |
   | Cobertura abonadora | Substitui `previsto` por `ausencia`. (`app/colaboradores/services/calendario.py:179-180`) |
   | Veredito isolado | `previsto` bloqueia sua sobreposição, inclusive `faltou`, `nunca_bateu` e `justificado`. (`app/colaboradores/services/calendario.py:65-69,175`) |

   Esse último é um **risco não testado**, sem regressão de produção demonstrada: a porta normal de `faltou/justificado` cria ausência aprovada antes de lavrar o veredito, permitindo a precedência da ausência. Existência de ata isolada no dado fornecido **[nao verificado]**. (`app/ponto/services/veredito_celula.py:310-330`; `app/ponto/precedencia.py:211-217,318`; `app/colaboradores/services/calendario.py:120-121`)

## 30/10 14:02 · sessao c6cb00e95f9a · - · - · origem=Ronald (tmux)

**Pergunta 2 — há teto por data e a justificativa não é válida em geral.**

- O calendário pergunta somente sobre `[max(início, hoje−1), min(fim, hoje)]`; dias anteriores não entram em `_em_curso`, e seu `futuro` será falso. Isso decide implicitamente encerramento por idade civil nesses dias. (`app/colaboradores/services/calendario.py:702`, `app/colaboradores/services/calendario.py:704`, `app/colaboradores/services/calendario.py:797`)
- Contraexemplo do juiz: turno de 24 horas, `hi=hf=23:00`, iniciado em D, termina em D+1 às 23:00 e aberta expira em D+2 às 03:00; consultado em D+2 às 02:00, D é anterior a ontem e permanece não encerrado. Igualdade das bordas representa 24 horas; expiração é fim+4h, com comparação estrita `agora > expiração`. (`app/escala/servico_jornada.py:95`, `app/escala/servico_jornada.py:102`, `app/ponto/turnos.py:1992`, `app/ponto/turnos.py:2007`, `app/ponto/turnos.py:2011`, `app/ponto/turnos.py:2097`)
- Esse contraexemplo com aberta refuta o comentário do corte, mas sozinho não prova status incorreto: com batida, o classificador segue `aberto`, independentemente de `futuro`. (`app/colaboradores/services/calendario.py:129`, `app/colaboradores/services/calendario.py:151`)
- Há contraexemplo de status derivável: dia de trabalho sem batida e sem marcos/fim previsto pode continuar não encerrado indefinidamente; o juiz devolve `sem_previsto`, porém o calendário passa `futuro=False` quando anterior a ontem e pode produzir `falta`. Célula sem DNA de marcos recorre ao tipo, que pode fornecer horários ausentes. Existência desse caso nos dados de produção **[nao verificado]**. (`app/ponto/turnos.py:2102`, `app/ponto/turnos.py:2104`, `app/escala/models.py:1099`, `app/escala/models.py:1108`, `app/escala/servico_jornada.py:72`, `app/colaboradores/services/calendario.py:130`, `app/colaboradores/services/calendario.py:140`, `app/colaboradores/services/calendario.py:797`)
- O encadeamento dos quatro marcos admite avançar mais de uma virada; portanto o corpo de `fim_turno` não impõe duração máxima de um dia. A presença de DNA/template assim em produção **[nao verificado]**. (`app/escala/servico_jornada.py:74`, `app/escala/servico_jornada.py:82`, `app/escala/servico_jornada.py:89`)

**Pergunta 3 — juiz novo recebe o mesmo relógio; calendário inteiro ainda tem outros relógios.**

- **Sem achado** na cadeia adicionada: calendário captura `_agora` e deriva `hoje` dele; repassa `_agora`; `dias_encerrados` só captura `timezone.now()` quando argumento ausente; julgamento usa o argumento recebido. (`app/colaboradores/services/calendario.py:250`, `app/colaboradores/services/calendario.py:251`, `app/colaboradores/services/calendario.py:704`, `app/ponto/turnos.py:2118`, `app/ponto/turnos.py:2097`, `app/ponto/turnos.py:2105`)
- O calendário chama `folha_manda`, que consulta `turno_aberto_de` sem `agora` e `ate_ontem` sem `hoje`; ambos capturam relógio novamente. Isso afeta a palavra/lista “Em aberto”, enquanto `_em_curso` e status usam a borda anterior. (`app/colaboradores/services/calendario.py:681`, `app/relatorios/cartao_pela_celula.py:398`, `app/ponto/turnos.py:2254`, `app/relatorios/cartao_pela_celula.py:412`, `app/relatorios/cartao_pela_celula.py:40`)
- Existe outro `timezone.now()` para `pode_desfazer`, de finalidade distinta. (`app/colaboradores/services/calendario.py:335`, `app/colaboradores/services/calendario.py:339`)
- `_pv` também pode chegar a `date.today()`: calendário consulta pergunta viva; seu cálculo consulta janela viva, que usa data do sistema. Isso influencia `_perg_viva`, não o encerramento consultado no diff. (`app/colaboradores/services/calendario.py:580`, `app/chamados/juizes.py:795`, `app/chamados/juizes.py:1066`, `app/chamados/juizes.py:518`)
- `grade_da_celula` não captura relógio no seu corpo principal. (`app/escala/services/leitor_celula.py:321`, `app/escala/services/leitor_celula.py:371`)

**Pergunta 4 — custo adicional em request, por card.**

- Sem interseção com hoje/ontem: **zero** consultas adicionadas. Havendo interseção: uma chamada de lote por montagem, sem alimentação das células/turnos já carregados. (`app/colaboradores/services/calendario.py:308`, `app/colaboradores/services/calendario.py:402`, `app/colaboradores/services/calendario.py:703`, `app/colaboradores/services/calendario.py:704`)
- Contagem estática adicional: **`6 + E + M + U + K`**. Seis fixas: duas cargas de células, dois carregamentos de vínculos, batidas e vínculo da ata; `E`=0/1 geradoras extras, `M`=0/1 mapa DNA, `U`=0/1 última batida, `K`=0..2 consultas unitárias de DNA quando os dias alcançam `fim_turno`. Medição efetiva **[nao verificado]**. (`app/ponto/turnos.py:2123`, `app/ponto/turnos.py:2124`, `app/ponto/turnos.py:2126`, `app/ponto/turnos.py:1880`, `app/ponto/turnos.py:1883`, `app/ponto/turnos.py:1858`, `app/ponto/turnos.py:1591`, `app/ponto/turnos.py:1556`, `app/escala/alimentacao.py:123`, `app/ponto/turnos.py:1949`, `app/ponto/turnos.py:1906`, `app/ponto/turnos.py:2102`, `app/escala/models.py:1097`)
- No caso comum `E=U=0`, `M=1`, todos os dias alcançando fim previsto: **+8 queries para um dia; +9 para dois dias**; para N cards com os dois dias, **+9N**. Derivação estática; execução **[nao verificado]**. (Mesmas consultas acima.)
- A duplicação de células existe porque o lote cria `cels`, mas passa o argumento original `celulas=None` a `turnos_do_colab`; além disso, não passa células ao `fim_turno → EC.marcos_do_dia`. (`app/ponto/turnos.py:2123`, `app/ponto/turnos.py:2126`, `app/ponto/turnos.py:2102`, `app/escala/servico_jornada.py:69`, `app/escala/models.py:1097`)
- HTML e PDF pagam isso no request: calendário é construído no laço de cards, dataset é chamado pela view antes da bifurcação HTML/PDF; o PDF reutiliza os cards, sem segunda montagem nessa bifurcação. (`app/relatorios/furos.py:139`, `app/relatorios/furos.py:184`, `app/relatorios/views.py:1541`, `app/relatorios/views.py:1560`, `app/relatorios/views.py:1566`)

## 30/10 14:02 · sessao d1a89dce4e93 · - · - · origem=Ronald (tmux)

5. **Sem achado na troca `ok → previsto` para essa premissa.** O fixture fixa HOJE às 10h, turno 08–17, isento sem batida; no classificador, `futuro` precede isenção e o juiz só encerra após o fim previsto. A mudança acompanha a lei do aval; a proteção específica do Art.62 continua discriminada pelos dias encerrados em `test_05`. (`app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:94`, `app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:116`, `app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:127`, `app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:272`, `app/colaboradores/services/calendario.py:131`, `app/ponto/turnos.py:2102`)

O nome/docstring/mensagem do `test_05b` ficaram obsoletos: ainda descrevem status `ok` e o “CASO 5 literal”, mas a asserção agora verifica `previsto`. Isso não demonstra regressão funcional; enfraquece a descrição do que o teste garante. (`app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:288`, `app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:301`, `app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:304`)

6. **Os três casos especificados têm mordida por inspeção, mas execução Django [nao verificado].** O fixture define jornada diária 18–06; os dois RED verificam D às 13h e D+1 às 02h como `previsto`; o MORDE exige `falta` em D+1 às 11h. Com a expressão antiga `data > hoje`, D não seria futuro em nenhum dos três momentos: os RED distinguem a correção e o MORDE impede mantê-lo previsto indefinidamente. Os testes não cobrem instante exato do fim, pós-fim no próprio dia, jornadas longas, aberta viva, ausência/cobertura/veredito nem unicidade de relógio. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:23`, `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:32`, `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:37`, `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:41`, `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:45`; expressão antiga no diff fornecido, `calendario.py:785`)

**`previsto` não é resposta universal quando há outros fatos.** A precedência atual permanece:

| Fatos além de trabalho/sem batida/não encerrado | Resultado |
|---|---|
| Nenhum; ou isento | `previsto`. (`app/colaboradores/services/calendario.py:131`) |
| Faixa humana `ferias/afastamento/atestado` | `falta` se desconta; senão `ausencia`, antes de consultar `futuro`. (`app/colaboradores/services/calendario.py:120`) |
| Feriado; ou folga humana | `folga`. (`app/colaboradores/services/calendario.py:123`, `app/colaboradores/services/calendario.py:127`) |
| Cobertura que abona, sem faixa humana | `ausencia`: o ajuste final sobrescreve `previsto`. (`app/colaboradores/services/calendario.py:179`) |
| Cobertura que desconta, sem faixa humana | `previsto`, pois ela só determina `falta` no ramo passado. (`app/colaboradores/services/calendario.py:131`, `app/colaboradores/services/calendario.py:140`) |
| Veredito isolado `faltou`, `nunca_bateu`, `justificado` ou `concorde` | `previsto`: esse status bloqueia a sobreposição de qualquer veredito. (`app/colaboradores/services/calendario.py:65`, `app/colaboradores/services/calendario.py:175`) |

Exercitei essa tabela extraindo apenas `_classificar` e suas constantes por AST em memória, com `python3 -B`, sem importar Django nem acessar banco; os resultados correspondem aos ramos acima. Evidência persistida dessa execução [nao verificado].

**Ressalva para não produzir falso achado:** a porta normal de veredito humano cria ausência aprovada antes de lavrar `faltou/justificado`; essa ausência entra na faixa humana e retorna antes de `previsto`. Portanto, “o diff apaga toda falta declarada pelo admin” seria uma conclusão incorreta. A regressão é demonstrável para o classificador com **ata isolada**, mas existência desse estado real no dado fornecido [nao verificado]. (`app/ponto/services/veredito_celula.py:310`, `app/ponto/services/veredito_celula.py:329`, `app/ponto/precedencia.py:211`, `app/ponto/precedencia.py:318`, `app/colaboradores/services/calendario.py:120`)

Achado adicional para item 1: HOJE/ONTEM em curso passam a carregar `status_rotulo='Dia futuro, ainda sem batida'`, e o template imprime essa frase no tooltip do ícone `previsto`. O rótulo não acompanha a ampliação semântica do status. (`app/colaboradores/services/calendario.py:55`, `app/colaboradores/services/calendario.py:797`, `app/colaboradores/services/calendario.py:873`, `app/templates/colaboradores/partials/_calendario_grade.html:105`)

## 30/10 14:01 · sessao 24d67b3f80aa · - · - · origem=Ronald (tmux)

Censo de leitores confirmado; não encontrei outro chamador direto de produção além das cadeias abaixo. Chamadas dinâmicas ou externas permanecem [nao verificado].

| Escritor/leitor | Efeito do diff |
|---|---|
| Escritor de `futuro` | `contexto_calendario` escreve o fato em `calendario.py:797`; somente `_classificar` lê esse fato (`app/colaboradores/services/calendario.py:131`, `app/colaboradores/services/calendario.py:147`). O dicionário exportado contém `status` e derivados, sem `futuro` (`app/colaboradores/services/calendario.py:867`). |
| Escritor de `status` | `_classificar` decide e `contexto_calendario` publica `status`, `status_rotulo`, `regua_rotulo`, `resolver_dia_oferecido` e contagens (`app/colaboradores/services/calendario.py:104`, `app/colaboradores/services/calendario.py:872`, `app/colaboradores/services/calendario.py:885`, `app/colaboradores/services/calendario.py:917`, `app/colaboradores/services/calendario.py:920`). |
| Perfil | A view monta o calendário; o partial inclui a grade; falta muda de fundo/ícone para previsto (`app/colaboradores/views.py:1946`, `app/templates/colaboradores/calendario_partial.html:62`, `app/templates/colaboradores/partials/_calendario_grade.html:72`, `app/templates/colaboradores/partials/_calendario_grade.html:75`, `app/templates/colaboradores/partials/_calendario_grade.html:102`, `app/templates/colaboradores/partials/_calendario_grade.html:105`). |
| Gaveta | Monta o mesmo calendário e inclui a mesma grade e resumo (`app/colaboradores/services/drawer.py:121`, `app/templates/colaboradores/partials/_drawer_diagnostico.html:35`). |
| Furos HTML | Monta o calendário por card e inclui a grade/resumo. Contagem, seleção, lista de marcos e ordenação continuam vindo da outra cadeia `grade_da_celula → estado_do_dia` (`app/relatorios/furos.py:141`, `app/relatorios/furos.py:160`, `app/relatorios/furos.py:164`, `app/relatorios/furos.py:184`, `app/relatorios/furos.py:190`, `app/templates/relatorios/furos.html:42`). |
| Furos PDF | A view extrai `data → status`; o PDF pinta `falta` e `previsto` com cores distintas, mas imprime `nfuro` e a lista de furos independentes (`app/relatorios/views.py:1550`, `app/relatorios/furos_pdf.py:12`, `app/relatorios/furos_pdf.py:13`, `app/relatorios/furos_pdf.py:56`, `app/relatorios/furos_pdf.py:66`). |
| Resolver dia / DP | `previsto` não pertence aos status que oferecem decisão. HOJE já era excluído por passado estrito; ONTEM em curso pode perder o botão se também satisfizer último recurso (`app/colaboradores/services/calendario.py:188`, `app/colaboradores/services/calendario.py:196`, `app/colaboradores/services/calendario.py:886`, `app/templates/colaboradores/partials/_calendario_grade.html:125`). |
| Vigia | Pula hoje/futuro e consulta a porta curta do calendário para passado. Se ONTEM perder Resolver dia, deixa de entrar em `decisao_do_dia_pendente` e segue pelos outros filtros; pode alcançar `fio_morto` (`app/chamados/services/vigia_fio.py:289`, `app/chamados/services/vigia_fio.py:294`, `app/chamados/services/vigia_fio.py:298`, `app/chamados/services/vigia_fio.py:301`, `app/chamados/services/vigia_fio.py:324`). Ocorrência real [nao verificado]. |
| Rodapé/resumo | Conta literalmente os status; diminui `faltas` ou `ok`, aumenta `previstos`. O rodapé mostra “Sem registro” e “Ok”, mas não exibe previstos (`app/colaboradores/services/calendario.py:921`, `app/colaboradores/services/calendario.py:922`, `app/colaboradores/services/calendario.py:937`, `app/templates/colaboradores/partials/_calendario_grade.html:142`, `app/templates/colaboradores/partials/_calendario_grade.html:146`, `app/templates/colaboradores/partials/_calendario_grade.html:173`). |
| Portas curtas | `status_do_dia` e `contar_por_status` montam calendário e devolvem o status/contagem; herdam integralmente a mudança (`app/colaboradores/services/calendario.py:217`, `app/colaboradores/services/calendario.py:220`, `app/colaboradores/services/calendario.py:228`, `app/colaboradores/services/calendario.py:231`). Chamadores diretos encontrados são testes; inexistência de chamadas dinâmicas [nao verificado]. |

**Alcance maior que “falta → previsto”.** O novo `futuro=True` também desloca isento de `ok` para `previsto`, escala suspeita de `escala_suspeita` para `previsto`, e pode deslocar limbo suspeito para `sem_dados`; remove o `regua_rotulo` quando deixa de ser escala suspeita (`app/colaboradores/services/calendario.py:131`, `app/colaboradores/services/calendario.py:133`, `app/colaboradores/services/calendario.py:135`, `app/colaboradores/services/calendario.py:147`, `app/colaboradores/services/calendario.py:150`, `app/colaboradores/services/calendario.py:885`).

**Ata passa a ser ignorada nesse ramo.** Antes, uma falta/ok/escala suspeita podia receber o status do veredito; após virar `previsto`, a guarda impede a substituição. Cobertura abonadora ainda substitui `previsto` por `ausencia`, e ausência/feriado continuam precedendo a pergunta de futuro (`app/colaboradores/services/calendario.py:120`, `app/colaboradores/services/calendario.py:123`, `app/colaboradores/services/calendario.py:175`, `app/colaboradores/services/calendario.py:179`).

**Achado condicionado para o vigia:** na virada da competência, ONTEM em curso pode satisfazer último recurso e antes oferecer Resolver dia; com `previsto`, perde a ação e pode aparecer como fio morto, embora o turno ainda esteja vivo. A cadeia permite isso; caso real no dado fornecido [nao verificado] (`app/colaboradores/services/calendario.py:571`, `app/colaboradores/services/calendario.py:886`, `app/colaboradores/services/calendario.py:196`, `app/chamados/services/vigia_fio.py:294`, `app/chamados/services/vigia_fio.py:324`).

## 30/10 13:55 · sessao 3ded292a25d3 · O261-VETO-A2 · verificador · origem=Code

**O delta não comprova a cura dos achados 1 e 2; cura os alvos de 3, 4 e 5 no código, com lacunas de teste descritas abaixo.** Execução dos testes, RED contra a base e resultados em produção: **[nao verificado]**. [nao verificado]

**1. Achados 1..5 do opus**

| Achado | Veredito e mudança | Mutante/lacuna que permanece |
|---|---|---|
| **1 — RED do nascimento contra a base** | **Cura não demonstrada.** O diff fornecido não acrescenta essa prova **[nao verificado]**. Há testes de nascimento, `ec=None` e reabertura na cópia; existência de teste não comprova RED contra a base (`app/chamados/tests/test_o261_veto_na_porta.py:418`, `:476`, `:500`, `:522`). | Não há teste novo desse item no delta. Quais mutantes a execução contra a base eliminaria: **[nao verificado]**. |
| **2 — linha LEI-AKITA desatualizada** | **Cura não demonstrada.** A linha corrigida de commit não foi fornecida **[nao verificado]**. As origens que precisam constar existem: `recusa_da_reabertura` consulta o veto e `reabrir` consulta essa guarda (`app/chamados/models.py:1090`, `:1143`). O leitor omitido importa e consome `furos_vetados_por_regua` (`app/escala/management/commands/desfazer_corroboracao_regua.py:25`, `:38`). | Nenhum teste novo verifica esse metadado **[nao verificado]**. No consumidor citado, `vinculo=None` é excluído pelo filtro `== ec.pk`: **sem achado funcional nesse caminho** (`app/ponto/services/furos_vetados.py:69`; `app/escala/management/commands/desfazer_corroboracao_regua.py:39`). |
| **3 — impressão com `ec=None` sem teste próprio** | **Curado no alvo: sem achado.** O teste novo observa a impressão real, controla `{None}`, encerra a vigência e exige `{'VETO_DEGENERADA'}` (`app/ponto/tests/test_veto_que_cai_rejulga.py:124`, `:129`, `:130`, `:131`). O cartório consulta o juiz mesmo sem vínculo (`app/ponto/services/cartorio.py:1013`). | Mutante plausível: `impressao_insumos` receber `VETO_DEGENERADA`, mas ignorar especificamente esse valor no hash. O spy continuaria vendo o argumento; o teste direto do hash só compara `VETO_REGUA` (`app/ponto/services/cartorio.py:329`; `app/ponto/tests/test_veto_que_cai_rejulga.py:126`, `:138`). Sobrevivência executada: **[nao verificado]**. |
| **4 — helper 5x2 dependente do fim de semana** | **Curado no calendário declarado:** agora é semanal com os sete dias (`app/chamados/tests/_vinculo_do_dia.py:18`). Semanal usa a lista; 5x2 sem configuração folga sábado/domingo (`app/escala/models.py:830`, `:852`, `:861`). Efeitos nas fixtures estão discriminados abaixo. | Não foi acrescentado teste direto dos sete weekdays. Um mutante omitindo domingo não seria detectado pelos novos testes de cartório/comando, que criam seus próprios tipos 5x2 e 6x1 (`app/ponto/tests/test_veto_que_cai_rejulga.py:37`; `app/ponto/tests/test_passe_turno_aberto_retroativo.py:41`). Resultado na suíte inteira: **[nao verificado]**. |
| **5 — veto contado por eliminação** | **Curado no código: sem achado no alvo.** Agora pergunta célula, pergunta veto e, sem resposta dos dois, incrementa `recusado_sem_juiz` (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:174`, `:179`, `:184`, `:193`). | Mutante plausível: imprimir `[RECUSADO veto]` no fallback, preservando o contador correto. O MORDE novo só exige os contadores, sem exigir `[RECUSADO sem juiz]` (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:188`; `app/ponto/tests/test_passe_turno_aberto_retroativo.py:142`). Sobrevivência executada: **[nao verificado]**. |

**2. Helper semanal: mudança de comportamento e módulos dependentes**

A mudança **pode alterar a previsão calculada dos fins de semana**, mas não sobrescreve uma célula existente: `EscalaColaborador.eh_dia_trabalho` devolve primeiro `cel.trabalha`; só sem célula consulta o cálculo (`app/escala/models.py:1066`, `:1071`, `:1073`).

Também não se pode deduzir “novo chamado por remoção do veto”: `regua_degenerada` consulta os marcos, e `TipoEscala.marcos_do_dia` fornece horários independentemente de trabalho/folga (`app/escala/regua_defesa.py:26`; `app/escala/models.py:326`). Portanto, folga → trabalho não implica automaticamente veto → nascimento (`app/ponto/services/furos_vetados.py:40`).

Casos fixos relevantes:

| Módulo | Efeito identificável |
|---|---|
| `app/chamados/tests/test_agrupado_em.py:17` | O delta também troca 03/08 por **01/08**. A fixture efetiva era segunda de trabalho e passa a sábado de trabalho; não é o mesmo dia passando de folga a trabalho. Exercita abrir/reabrir (`app/chamados/tests/test_agrupado_em.py:32`, `:42`; comparação anterior: diff fornecido **[nao verificado]**). |
| `app/chamados/tests/test_selo_i4b_um_chamado_por_turno.py:52` | O sábado **05/09** permanece fixo e passa a trabalho no template do helper; a premissa regularizada é mockada como falsa, enquanto reabrir continua real (`app/chamados/tests/test_selo_i4b_um_chamado_por_turno.py:42`, `:69`, `:126`). |
| `app/chamados/tests/test_trilha_x_banco.py:28` | O primeiro caso monta o domingo **11/10**; muda o calendário disponível à abertura (`app/chamados/tests/test_trilha_x_banco.py:39`). |
| `app/ponto/tests/test_justificativa_encerra_origem.py:101` | O segundo caso do lote cria chamado no domingo **25/10**; muda o calendário disponível nesse cenário (`app/ponto/tests/test_justificativa_encerra_origem.py:86`, `:102`). |

**As dependências explícitas de folga/trabalho continuam presentes:**

- **Folga:** `test_reconciliar_pergunta_a_premissa` força `trabalha=False` antes e depois da abertura, mantém folga com ata velha e compara folga/trabalho na reabertura. O helper não apagou esses cenários (`app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:196`, `:199`, `:212`, `:239`, `:250`).
- **Indefinição:** `test_a2_marco_morto` força `trabalha=None`, com contraprova de grade furada (`app/ponto/tests/test_a2_marco_morto.py:38`, `:48`).
- **Trabalho fixado na própria célula:** os módulos abaixo preservam esse booleano pela precedência da célula (`app/escala/models.py:1071`):

| Módulo | Fixture |
|---|---|
| `app/chamados/tests/test_b2d_resposta_visivel.py` | `:86` |
| `app/chamados/tests/test_c2_le_o_protesto_que_cobra.py` | `:40` |
| `app/chamados/tests/test_emissao_reata_orfa.py` | `:37` |
| `app/chamados/tests/test_emissao_reemite_c2.py` | `:48`, `:61` |
| `app/chamados/tests/test_fechar3_retorno_do_gate.py` | `:62` |
| `app/chamados/tests/test_reabrir_decide_pelo_banco.py` | `:42` |
| `app/chamados/tests/test_reabrir_pergunta_a_premissa.py` | `:47` |
| `app/chamados/tests/test_reconciliar_pergunta_a_premissa.py` | `:43` |
| `app/chamados/tests/test_tranca_de_verdade.py` | `:60` |

Os usuários com **dia do fato relativo**, que exigem atenção ao calendário de execução, são estes; essa lista identifica datas sensíveis, não comprova mudança de resultado:

| Módulo | Referência |
|---|---|
| `app/api/tests/test_chamados_v2_destino.py` | `:37` |
| `app/chamados/tests/test_3c_abrir_ausencia.py` | `:36` |
| `app/chamados/tests/test_b8_smoke_reabertura.py` | `:36` |
| `app/chamados/tests/test_bug125_cobranca_cega.py` | `:56` |
| `app/chamados/tests/test_e5b_cobrar_massa.py` | `:24` |
| `app/chamados/tests/test_emissor_nomeia_o_marco.py` | `:44`, `:126` |
| `app/chamados/tests/test_f3_dedup_atraso.py` | `:55`, `:62` |
| `app/chamados/tests/test_fila_por_causa.py` | `:111` |
| `app/chamados/tests/test_orfa_sem_candidato.py` | `:37` |
| `app/chamados/tests/test_reconciliar_pergunta_orfa.py` | `:27` |
| `app/chamados/tests/test_regressao_etapa7.py` | `:79`, `:107`, `:328`, `:475` |
| `app/chamados/tests/test_resposta_tardia.py` | `:20` |
| `app/chamados/tests/test_selo_b5h_followup_motivo.py` | `:68`, `:82` |
| `app/chamados/tests/test_selo_dedup_premissa_viva.py` | `:90` |
| `app/chamados/tests/test_selo_f4_fatia4.py` | `:65` |
| `app/chamados/tests/test_selo_i6_ouve_a_lei.py` | `:63` |
| `app/chamados/tests/test_selo_porta_recusa.py` | `:63`, `:186` |
| `app/chamados/tests/test_validar_classe_a.py` | `:45` |
| `app/ponto/tests/test_a2_marco_morto.py` | `:31`, `:59` |
| `app/ponto/tests/test_bug114_fechador_de_dois_dias.py` | `:73` |
| `app/ponto/tests/test_bug115_lampada_do_marco.py` | `:54` |

Não há demonstração de folga explícita escondida pelo helper: **sem achado nesses cenários** (`app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:196`, `:212`). Novo furo, chamado ou total previsto efetivamente produzido pela troca, e resultado da suíte: **[nao verificado]**.

**3. O spy intercepta a chamada real?**

**Sim — sem achado.** `impressao_insumos` é definida no próprio módulo; `_julgar_colab_corpo` a chama pelo nome global desse módulo. O patch substitui justamente esse atributo. Não há alias local/importado nessa chamada (`app/ponto/services/cartorio.py:314`, `:829`, `:1083`; `app/ponto/tests/test_veto_que_cai_rejulga.py:124`).

O percurso observado é `_julgar()` → `julgar_colab()` → `_julgar_colab_corpo()` → `impressao_insumos(..., veto=_veto_do_dia(data))` (`app/ponto/tests/test_veto_que_cai_rejulga.py:85`; `app/ponto/services/cartorio.py:744`, `:1083`).

**4. Ordem e ramo exercitado pelo MORDE**

**A ordem dos dois juízes coincide — sem achado:** `abrir` pergunta primeiro à célula e depois ao veto. `sem_juiz` é fallback diagnóstico do comando, não uma terceira guarda de `abrir` (`app/chamados/models.py:725`, `:731`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:184`).

**O MORDE exercita o fallback pretendido por injeção:** a fixture lavra `furo`, que não absolve; mantém vínculo 6x1 válido; o patch entrega `(None, False)` ao comando (`app/ponto/tests/test_passe_turno_aberto_retroativo.py:41`, `:50`, `:69`, `:140`; `app/chamados/reconciliador.py:977`).

Ele não exercita o corpo real de `abrir` nem reproduz mudança concorrente do dia (`app/ponto/tests/test_passe_turno_aberto_retroativo.py:140`). Também falta cenário com **ambos** os juízes recusando: inverter a prioridade célula/veto não seria distinguido pelos novos casos de veto isolado e nenhum juiz (`app/ponto/tests/test_passe_turno_aberto_retroativo.py:124`, `:135`). Mutação executada: **[nao verificado]**.

**5. Leitores de `fim:` e campo novo**

**Sem achado na cópia.** O comando está declarado fora do pipeline, para execução por ato (`app/config/crons.py:743`, `:875`, `:884`). Os testes capturam `stdout` e verificam substrings, aceitando o campo acrescentado (`app/ponto/tests/test_passe_turno_aberto_retroativo.py:77`, `:118`, `:142`).

O envelope de cron registra tempo e código de saída em JSON próprio; o placar lê esse JSON, sem interpretar o `fim:` textual (`bin/cron_run.sh:11`, `:13`; `app/core/management/commands/pipeline_placar.py:69`, `:72`, `:76`, `:80`).

Regex ou consumidor externo à cópia, e eventual quebra em produção: **[nao verificado]**.

## 30/10 13:54 · sessao 152a623de8b1 · - · - · origem=Ronald (tmux)

Achado 4: o helper realmente passou a `semanal`, com `dias_trabalho_semana='0,1,2,3,4,5,6'` (`app/chamados/tests/_vinculo_do_dia.py:18`). O template anterior `5x2` folgava sábado/domingo pelo fallback; o atual trabalha nesses dias (`app/escala/models.py:830-834`, `app/escala/models.py:852-861`). Isso muda o calendário da fixture, mas não demonstra automaticamente novo furo/chamado: a célula existente prevalece sobre o template (`app/escala/models.py:1066-1073`).

Casos fixos cujo calendário muda:

- `test_agrupado_em`: sábado `2026-08-01`, exercitando abrir/reabrir (`app/chamados/tests/test_agrupado_em.py:17`, `:32-45`).
- `test_selo_i4b_um_chamado_por_turno`: sábado `2026-09-05`, helper condicional; a premissa é explicitamente mockada como viva (`app/chamados/tests/test_selo_i4b_um_chamado_por_turno.py:37-44`, `:52-54`, `:66-77`).
- `test_trilha_x_banco`: o primeiro caso monta domingo `2026-10-11` por formatação (`app/chamados/tests/test_trilha_x_banco.py:28-30`, `:39`).
- `test_justificativa_encerra_origem`: o segundo caso do lote cria chamado de domingo `2026-10-25` (`app/ponto/tests/test_justificativa_encerra_origem.py:86-94`, `:97-109`).

Folga e indefinição próprias continuam explícitas:

- `test_reconciliar_pergunta_a_premissa` força `trabalha=False` antes e depois de abrir, e também mantém cenários de folga/trabalho (`app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:196-201`, `:212-219`, `:239-259`).
- `test_a2_marco_morto` monta `trabalha=None` e contraprova com grade definida e furada (`app/ponto/tests/test_a2_marco_morto.py:38-42`, `:48-68`).

Módulos que fixam trabalho na célula, além dos relativos: `test_b2d_resposta_visivel.py:86`, `test_c2_le_o_protesto_que_cobra.py:40`, `test_emissao_reata_orfa.py:37`, `test_emissao_reemite_c2.py:48,61`, `test_fechar3_retorno_do_gate.py:62`, `test_reabrir_decide_pelo_banco.py:42`, `test_reabrir_pergunta_a_premissa.py:47`, `test_reconciliar_pergunta_a_premissa.py:43`, `test_tranca_de_verdade.py:60`, todos sob `app/chamados/tests/`. O helper não sobrescreve esses booleanos (`app/escala/models.py:1071-1073`).

Os 21 módulos com dia do fato relativo são:

- `app/api/tests/test_chamados_v2_destino.py:37`.
- Sob `app/chamados/tests/`: `test_3c_abrir_ausencia.py:36`; `test_b8_smoke_reabertura.py:36`; `test_bug125_cobranca_cega.py:56`; `test_e5b_cobrar_massa.py:24`; `test_emissor_nomeia_o_marco.py:44,126`; `test_f3_dedup_atraso.py:55,62`; `test_fila_por_causa.py:111`; `test_orfa_sem_candidato.py:37`; `test_reconciliar_pergunta_orfa.py:27`; `test_regressao_etapa7.py:79,107,328,475`; `test_resposta_tardia.py:20`; `test_selo_b5h_followup_motivo.py:68,82`; `test_selo_dedup_premissa_viva.py:90`; `test_selo_f4_fatia4.py:65`; `test_selo_i6_ouve_a_lei.py:63`; `test_selo_porta_recusa.py:63,186`; `test_validar_classe_a.py:45`.
- Sob `app/ponto/tests/`: `test_a2_marco_morto.py:31,59`; `test_bug114_fechador_de_dois_dias.py:73`; `test_bug115_lampada_do_marco.py:54`.

Os outros seis módulos com relógio relativo têm fato fixo e usam `now()` para carimbos: `test_b2d_resposta_visivel.py:16,53`, `test_competencia_trancada.py:20-21,46`, `test_laco_hx_x_bug31.py:37,70-75`, `test_reabrir_pergunta_a_premissa.py:24,95-97`, `test_tranca_de_verdade.py:22,38`, `test_validacao_respeita_tranca.py:30,51`, sob `app/chamados/tests/`. Portanto “27 módulos relativos” não significa 27 dias do fato relativos nesses usuários.

Feriados: não há mudança demonstrada pelo helper; ele usa `objects.create` sem preencher `trabalha_em_feriado`, cujo default é `True` nos dois casos (`app/chamados/tests/_vinculo_do_dia.py:25-26`; `app/escala/models.py:896-898`). A política estática que diferencia `5x2` de `semanal` não é chamada pelo helper (`app/escala/models.py:900-917`).

Resultado da suíte ou novo furo/chamado/previsto efetivamente produzido: `[nao verificado]`. Sem achado demonstrado de asserção sobre folga apagada pela mudança; os cenários explícitos preservam a folga e a indefinição (`app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:196-215`; `app/ponto/tests/test_a2_marco_morto.py:38-42`).

## 30/10 13:53 · sessao e52407c8c32d · - · - · origem=Ronald (tmux)

Achado 5 curado no código: recusa por célula, depois veto consultado explicitamente, depois `recusado_sem_juiz`; o contador deixou de atribuir veto por eliminação. A sequência coincide com os dois gates de nascimento de `abrir`. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:174-188`; `app/chamados/models.py:725-734`.)

O MORDE exercita o fallback pretendido por injeção: `_entrada_sem_saida` usa `acusa=True` e lavra `furo`; célula acusadora não absolve, e o patch faz `abrir` devolver `(None, False)`. Ele verifica `recusado_veto=0` e `recusado_sem_juiz=1`. Não comprova uma corrida real nem executa o corpo de `abrir`. (`app/ponto/tests/test_passe_turno_aberto_retroativo.py:57-74`, `:135-143`; `app/chamados/reconciliador.py:977-979`.)

Mutantes plausíveis ainda permitidos pelos novos testes: [nao verificado]

- Trocar apenas o texto do fallback para `[RECUSADO veto]`, mantendo o contador `recusado_outro`: o MORDE só confere contadores, sem exigir `[RECUSADO sem juiz]`. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:184-188`; `app/ponto/tests/test_passe_turno_aberto_retroativo.py:140-143`.)
- Inverter a prioridade célula/veto quando ambos recusam: os novos casos cobrem veto isolado e ausência dos dois juízes, sem forçar dupla recusa. (`app/ponto/tests/test_passe_turno_aberto_retroativo.py:124-143`.)

Pergunta 5: **sem achado na cópia**. O comando está declarado fora do pipeline, executado por ato; os consumidores diretos encontrados são os testes, que capturam `stdout` e usam `assertIn`, tolerando o campo acrescentado. (`app/config/crons.py:743`, `:875-885`; `app/ponto/tests/test_passe_turno_aberto_retroativo.py:77-84`, `:118-143`.)

O envelope de cron usa código de saída e tempos para produzir seu próprio JSON; o placar lê desse JSON `nome`, `emp`, `exit` e `dur`, sem interpretar a linha textual `fim:`. (`bin/cron_run.sh:11-13`; `app/core/management/commands/pipeline_placar.py:60-80`.) Leitores externos/regex fora da cópia e execução dos testes: `[nao verificado]`.

## 30/10 13:52 · sessao 3f59d5d879cc · - · - · origem=Ronald (tmux)

1. **RED contra a base: cura não demonstrada pelo delta.** Os testes de nascimento e reabertura existem na cópia, mas sua existência não prova execução vermelha contra a base: `app/chamados/tests/test_o261_veto_na_porta.py:418`, `:476`, `:500`, `:522`. Não encontrei o log `red_reabrir.log`, árvore Git ou mensagem de commit que permitam conferir aquisição e hash; execução contra a base é `[nao verificado]`. O delta fornecido não altera esses testes nem acrescenta essa prova `[nao verificado]`.

2. **Linha LEI-AKITA: cura não demonstrada pelo delta.** A origem atual realmente inclui `recusa_da_reabertura`, que pergunta o veto em `app/chamados/models.py:1090`, e `reabrir`, que consulta essa função e grava a recusa em `app/chamados/models.py:1143`. O leitor omitido também existe: `app/escala/management/commands/desfazer_corroboracao_regua.py:25` importa `furos_vetados_por_regua`; `:38` consome a lista e `:39` filtra por vínculo. A linha de commit corrigida não está disponível `[nao verificado]`.

   Nesse consumidor específico, `vinculo=None` não quebra a leitura: o produtor retorna essa chave com `None` em `app/ponto/services/furos_vetados.py:69`, e o filtro por `ec.pk` o exclui em `app/escala/management/commands/desfazer_corroboracao_regua.py:39`. **Sem achado funcional nesse caminho.**

Para 1/2, não há teste novo dedicado no delta fornecido; eventual “mutante sobrevivente” desses itens não foi executado `[nao verificado]`. [nao verificado]

## 30/10 13:50 · sessao 13957253312a · - · - · origem=Ronald (tmux)

OK

## 30/10 13:40 · sessao bdc58fb8ddda · O261-VETO-A2 · censo · origem=Code

As referências aos anexos contam a primeira linha após cada cabeçalho `== anexo: ... ==` como linha 1.

1. **RED contra a base — classe (c), fora do alcance.**  
   **Pergunta:** incluía “selo/RED sem mordida”. (`pergunta_final.q:8`)  
   **Resposta:** delimitou “Execução da bateria Django [...] [nao verificado]”. Não há trecho que identifique o RED executado na cópia intermediária. (`codex_final.txt:3`)  
   O achado depende da execução histórica e da base efetivamente usada, informadas somente no relato do Opus; sua reprodução independente é **[nao verificado]**. (`opus_achados.txt:2`)

2. **Linha LEI-AKITA desatualizada — classe (a), não apontado com o tema presente.**  
   **Pergunta:** exigia “censo quem-mais-le incompleto” e “QUEM ESCREVE e QUEM LE cada campo/valor”. (`pergunta_final.q:8`; `pergunta_final.q:11`)  
   **Resposta:** identificou expressamente o leitor: “desfazer corroboracão filtra pelo vínculo”, citando `desfazer_corroboracao_regua.py:38`; também examinou a reabertura direta. Portanto, viu esses caminhos, mas não apontou sua ausência na declaração LEI-AKITA. (`codex_final.txt:21`; `codex_final.txt:50`)  
   A omissão documental específica não consta da triagem fornecida; não há sustentação para classificá-la como descarte pela triagem. (`triagem_final.txt:2–16`)

3. **Impressão com `ec=None` sem teste próprio — classe (a), não apontado com o tema presente.**   [nao verificado]
   **Pergunta:** nomeava explicitamente “ec=None vale TAMBEM para a impressao”, os leitores de cartório/impressão e a classe “selo/RED sem mordida”. (`pergunta_final.q:6`; `pergunta_final.q:8`; `pergunta_final.q:11`)  
   **Resposta:** descreveu que o cartório “Acrescenta `veto:VETO_DEGENERADA` à impressão do ramo normal”, mas não denunciou a falta de teste desse contrato nem o mutante indicado pelo Opus. (`codex_final.txt:28`; `opus_achados.txt:4`)   [nao verificado]
   O achado colocado em FILA foi outro: impressão do ramo `nunca_bateu` sem veto. Isso não demonstra descarte do achado sobre cobertura de `ec=None` no ramo normal. (`codex_final.txt:58`; `triagem_final.txt:8`)

4. **Helper com folga no fim de semana — classe (a), não apontado com o tema presente.**  
   **Pergunta:** nomeava o helper e perguntava se algum teste “ficou verde pelo motivo errado” ou “ficou vacuo”. (`pergunta_final.q:12`)   [nao verificado]
   **Resposta:** declarou “sem achado de intenção ‘sem vínculo/sem escala’ mascarada pela migração” e apontou dois testes que não provavam nascimento; não apontou a dependência do calendário descrita pelo Opus. (`codex_final.txt:36`; `codex_final.txt:38`; `opus_achados.txt:5`)   [nao verificado]
   Classifico a configuração do helper como inspecionável pelo código. A contagem histórica de 27 módulos e o resultado efetivo da suíte permanecem **[nao verificado]**. A cópia atual já usa sete dias de trabalho e atribui essa alteração ao Opus final. (`app/chamados/tests/_vinculo_do_dia.py:5–6`; `app/chamados/tests/_vinculo_do_dia.py:18`)

5. **`recusado_veto` contado por eliminação — classe (a), não apontado com o tema presente.**  
   **Pergunta:** exigia examinar quem determina e lê cada campo/valor, incluindo os “emissores”, além de procurar derivações paralelas de fonte única. (`pergunta_final.q:8`; `pergunta_final.q:11`)  
   **Resposta:** examinou nominalmente o retroativo: “lê indiretamente o juiz pelo dry `veto_no_nascimento`”. Não apontou a classificação da recusa no caminho de aplicação. (`codex_final.txt:34`)  
   A cópia atual contém uma consulta explícita ao juiz antes de incrementar `recusado_veto`, com comentário atribuindo a correção ao Opus; o defeito anterior é o registrado no anexo. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:179–187`; `opus_achados.txt:6`)

6. **Condições operacionais do pouso e medidas — classe (c), fora do alcance, com risco já visto.**   [nao verificado]
   **Pergunta:** exigia “reversao antes, exportada intacta por hash” e procurar “pouso antes do pre-requisito”; também nomeava `furos_sem_cobranca`. (`pergunta_final.q:6`; `pergunta_final.q:8`; `pergunta_final.q:11`)   [nao verificado]
   **Resposta:** registrou “Reversão anterior, preservação por hash e diferença à Pauta DP: [nao verificado]”. Na DELTA, reiterou: “Item 9 continua pendente de pouso, embora esteja como MEDINDO”. (`codex_final.txt:64`; `codex_delta.txt:45`)   [nao verificado]
   A triagem manteve a medida “antes de qualquer pouso”; portanto, há evidência de reconhecimento da pendência, sem evidência de descarte. A execução das condições, a divergência “63 × 1.576” e os resultados antes/depois não são demonstrados pelo material fornecido: **[nao verificado]**. (`triagem_final.txt:15–16`; `opus_achados.txt:7`) [nao verificado]

7. **Ato humano sem mostrar/gravar o veto ultrapassado — classe (a), não apontado com o tema presente.**  
   **Pergunta:** trazia literalmente “ato humano passa com trilha”; a DELTA 2 voltou a pedir conferência das três cadeias `humano=True`. (`pergunta_final.q:5`; `pergunta_delta2.q:4`)  
   **Resposta:** declarou “sem achado nas três cadeias humanas atuais”, verificando permissões e origem humana, mas não apontou a ausência do veto ultrapassado na apresentação/auditoria. (`codex_delta2.txt:7–9`)  
   Na cópia, `humano=True` dispensa a consulta ao veto nessa guarda; as notas e auditorias examinadas registram o ato administrativo sem nomear esse veto. Isso sustenta a distinção entre registrar a ação humana e registrar qual veto ela ultrapassou. (`app/chamados/models.py:1090–1093`; `app/chamados/services/acoes_disputa.py:266–270`; `app/chamados/services/competencia_trancada.py:258–264`)

| Achado | Classe | Referência principal |
|---|---|---|
| 1. RED contra a base | **(c)** | `codex_final.txt:3`; `opus_achados.txt:2` |
| 2. LEI-AKITA desatualizada | **(a)** | `pergunta_final.q:8`; `codex_final.txt:21` |
| 3. Teste da impressão com `ec=None` | **(a)** | `pergunta_final.q:6`; `codex_final.txt:28` | [nao verificado]
| 4. Helper dependente do calendário | **(a)** | `pergunta_final.q:12`; `codex_final.txt:36–38` |
| 5. Contador por eliminação | **(a)** | `pergunta_final.q:11`; `codex_final.txt:34` |
| 6. Condições e medidas do pouso | **(c)**, com pendência reconhecida | `codex_delta.txt:45`; `triagem_final.txt:15–16` | [nao verificado]
| 7. Trilha do veto ultrapassado pelo humano | **(a)** | `pergunta_final.q:5`; `codex_delta2.txt:7–9` |

## 30/10 13:36 · sessao ff42a8bee5de · O261-VETO-A2 · sonda · origem=Code

`sonda_fsc.py` não foi localizado na cópia; não houve nova execução. Segue integralmente a saída fornecida. [nao verificado]

```text
sonda_leitura: sonda_fsc.py [fe36a5107d6c7951] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

TOTAL=61
POR_PORTA|pergunta_ao_colab=45
POR_PORTA|supervisao_sem_push=16
POR_PORTA|veto_celula=1
POR_EMPRESA|2=61
ITENS=62
FIM-DA-SONDA
```

_496 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
