# AVAIS NA MESA — 11

> Gerado por `bin/gerar_avais.py` a partir de `app/docs/PENDENTES_RONALD.json`.
> **So itens ABERTOS.** Item respondido SOME daqui na proxima geracao -- a historia dele fica
> no JSON e no RELATO, nunca aqui. Ordem: o que trava a fila 1 primeiro, depois o mais antigo.

| # | id | tipo | desde | o numero | frase PRONTA para colar |
|---|---|---|---|---|---|
| 1 | `s5b-troca-do-calculador-tabela-final` | **!** | 2026-10-02T03:10 | DIFF REFEITO na sombra de 02/10, com os portoes LIGADOS e DESLIGADOS no MESMO dado -- a isolacao que o seu aval de 20:4x pedia. **atraso: +59,82 h em 13 colabs -> -0,16 h em 1**. **antecipada: +31,38 -> +29,96 h, os mesm | `! Ronald: SOBE a troca do leitor de rubrica para o calculador (S5b) -- atraso em faixa (-0,16 h em 1 colab), tudo fora de atraso/antecipada identico ao de antes, e a antecipada que sobra (+29,96 h em 10) fica declarada como PAREAMENTO no O111, nao como regra` |
| 2 | `F2-PORTAO-22-22` | **lei** | 2026-10-02T10:35 | O hook aponta a **F2 (VISAO-FALTAS-FERIAS)** como o proximo da fila 1, e o portao dela e literal: *'nao construir antes de 22/22'*. MEDIDO pela funcao real (`core/contratos_estruturais.linha_do_placar`): **8/22 verdes, 1 | `lei Ronald: a F2 (VISAO-FALTAS-FERIAS) SEGUE no portao -- 8/22 -- e o 22/22 espera o `!` da S5b, porque as duas celulas mais proximas dependem do sitio de dinheiro que a troca vai substituir; a esteira segue na fila 1 pelos itens de portao ABERTO` |
| 3 | `CENSO-JUIZ-BATIDA-E-ESCALA` | **lei** | 2026-10-02T10:35 | DUAS celulas da matriz dizem *'NINGUEM COMECOU, e o censo e o que falta primeiro'*: `batida x um juiz por pergunta` e `escala x um juiz por pergunta` -- nao existe `JUIZES['batida']` nem `JUIZES['escala']` em `core/juize | `corte Ronald: monte o censo MEDIDO das familias `batida` e `escala` (pergunta, quem responde hoje, quantos sitios respondem por conta propria) e me traga as frases de corte para eu assinar` |
| 4 | `PARAMETROS-SEM-EFEITO-16-CAMPOS` | **!** | 2026-10-02T10:35 | CINCO das 14 celulas nao-verdes sao `parametro consumido ou sem efeito`, e o contrato (CLAUDE.md 4b) so as deixa verdes com o campo **consumido** -- rotular 'sem efeito' nao fecha a celula, so a torna honesta. MEDIDO pel | `! Ronald: dos 16 campos sem efeito, consuma <lista> e remova <lista>; os 7 da folha/export movem dinheiro, entao eles entram so com DIFF de frota publicado antes` |
| 5 | `O25-DIAS-PREVISTOS-EXIBIDOS` | **lei** | 2026-10-02T10:35 | O corte do O25 pede um SELO que eu nao pude fabricar: *'para TODO colab, `dias_previstos` do fechamento == dias previstos exibidos pelo espelho na mesma janela (contador `espelho_x_fechamento_dias`, esperado 0)'*. MEDI n | `lei Ronald: "dia previsto exibido" e <(a) todo dia com marco previsto na janela / (b) todo dia que a celula diz trabalho>; com isso eu fabrico o contador `espelho_x_fechamento_dias` e fecho o selo do O25` |
| 6 | `O27-COLUNA-NAO-FECHA-COM-O-RODAPE` | **lei** | 2026-10-02T10:50 | O27 JANELA-EXATA: a sua linha dizia *'medir antes de construir'*, e a medicao MUDA o alvo. MEDIDO na SOMBRA, competencia 09, pelas funcoes REAIS do cartao (`relatorios/pdf_espelho::_coletar_dados_espelho_mes`): **cartao_ | `corte Ronald: no cartao, (c) o rodape ganha a linha 'folga trabalhada' e passa a somar os valores EXIBIDOS -- a coluna e o badge ficam como estao; com isso eu fecho o selo `cartao_x_fechamento_total`` |
| 7 | `F2-ART130-LE-LITERAL-NAO-CADASTRO` | **lei** | 2026-10-02T11:05 | 4 tipos descontam e o art.130 conta 1; 7 de 1.046 periodos com direito gravado acima da tabela (todos em_curso); 0 adquirido/concedido divergente | `lei Ronald: para o art.130 contam <so a falta de dia / falta + suspensao>; atraso e saida antecipada sao parciais e NAO entram -- com isso eu troco o literal por um conjunto do catalogo e a F2 passa a mostrar a projecao do direito` |
| 8 | `ui-resposta-diz-o-que-e-smoke` | **smoke** | 2026-10-02T03:45 | NO AR desde 02/10 03:3x (`432058dd` + deploy). As 4 frases medidas nas perguntas REAIS pelo codigo no ar: `perg#37945` col106 'atraso de 1h 00m' + 'vai para a folha' + botao 'Validar com atraso'; `perg#38287` col358 'sai | `smoke Ronald: abri a mesa de disputa, a resposta longe do marco agora diz a direcao e o que validar faz, e o Reabrir avisa quantas respostas apaga -- pode fechar a UI-RESPOSTA-DIZ-O-QUE-E` |
| 9 | `W12X36-HPD-SMOKE` | **smoke** | 2026-10-02T11:47 | 128 tipos 12x36, 0 com hpd, 0 de 896 dia-tipo mudam; serve 338 vinculos e 1.307 plantoes de fim de semana | `smoke Ronald: abri o wizard de um 12x36, marquei o domingo com horario proprio e salvou; um 12x36 sem marcar nada continuou igual -- pode fechar o W12X36-HPD` |
| 10 | `RELAVRATURA-10-APLICADA` | **aplicado, revise** | 2026-10-02T16:00 | 565 colabs; 09 hash igual; 21 exportacoes identicas; 0 erro | `` |
| 11 | `COL954-FALTA-1440` | **!** | 2026-10-02T16:10 | 1440 min lancados contra 480 previstos; excesso 960 min = 16 h; 1 caso em 14 | `` |

---

Total no JSON: **189** · aberto **11** · respondido **14** · sem-motivo **164**.

> `sem-motivo` nao e "resolvido": e *"ninguem julgou este item"*. A ordem de 18:4x proibe
> triar os 163 antigos, entao eles ficam ai, nomeados, em vez de serem chutados para um lado.

