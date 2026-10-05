# AVAIS NA MESA — 8

> Gerado por `bin/gerar_avais.py` a partir de `app/docs/PENDENTES_RONALD.json`.
> **So itens ABERTOS.** Item respondido SOME daqui na proxima geracao -- a historia dele fica
> no JSON e no RELATO, nunca aqui. Ordem: o que trava a fila 1 primeiro, depois o mais antigo.

| # | id | tipo | desde | o numero | frase PRONTA para colar |
|---|---|---|---|---|---|
| 1 | `APP-DESENHA-A-PALAVRA-DO-DIA` | **!** | 02/10 10:2x | O SERVIDOR JA ENTREGA, o app nativo ainda nao desenha (O21 ROTULO-DO-DIA-DECIDIDO, seu corte de 24/09 10:xx). `api_espelho_v2` passou a levar em cada dia `veredito_dia`, `palavra_dia`, `cor_dia`, `decidido_por` e `decidi | `smoke Ronald: o app nativo passa a desenhar `palavra_dia`/`cor_dia` no release (as chaves ja saem de `api_espelho_v2`, aditivas -- app velho ignora) -- sao 615 dia-colab com decisao humana na competencia e 117 deles NAO-abonados, que hoje chegam ao celular iguais a um abono.   OU   fica com o tick g` |
| 2 | `LASTRO-MEDE-DUAS-VEZES` | **!** | 04/10 00:5x | ACHADO MEDIDO, NAO CURADO -- o arquivo esta na sua lista "ESPERAM LEI MINHA, nao tocar" (aval JUIZ-DE-CHAMADO, secao E), entao eu medi e PAREI de tocar. `ponto/management/commands/fechar_cobranca_com_lastro.py::handle` c | `! curar `fechar_cobranca_com_lastro` na origem: `fechar()` devolve o quadro que usou e o comando imprime ESSE -- hoje ele mede duas vezes por hora e o log diz `(a)=0` em dia que fechou 6 (soma da serie: 5 contra 10 fechados).   OU   fica como esta e o log segue descrevendo outra medicao.` |
| 3 | `ui-resposta-diz-o-que-e-smoke` | **smoke** | 2026-10-02T03:45 | NO AR desde 02/10 03:3x (`432058dd` + deploy). As 4 frases medidas nas perguntas REAIS pelo codigo no ar: `perg#37945` col106 'atraso de 1h 00m' + 'vai para a folha' + botao 'Validar com atraso'; `perg#38287` col358 'sai | `smoke Ronald: abri a mesa de disputa, a resposta longe do marco agora diz a direcao e o que validar faz, e o Reabrir avisa quantas respostas apaga -- pode fechar a UI-RESPOSTA-DIZ-O-QUE-E` |
| 4 | `W12X36-HPD-SMOKE` | **smoke** | 2026-10-02T11:47 | 128 tipos 12x36, 0 com hpd, 0 de 896 dia-tipo mudam; serve 338 vinculos e 1.307 plantoes de fim de semana | `smoke Ronald: abri o wizard de um 12x36, marquei o domingo com horario proprio e salvou; um 12x36 sem marcar nada continuou igual -- pode fechar o W12X36-HPD` |
| 5 | `PAUTA-DP-09-COL954` | **!** | 2026-10-02T17:30 | rubrica 8792, 2 dias (18/09 e 19/09), matricula 2103; emp2 213 linhas contra 210 | `col954: a falta de 18/09 e 19/09 (2 dias, rubrica 8792) entra na 09 do Dominio por correcao LA.   OU   gera TXT novo da 09 com `--usuario` e `--motivo` meus.   OU   fica fora da 09 e entra na 10.` |
| 6 | `PAUTA-DP-09-COL900` | **!** | 2026-10-02T17:30 | rubricas 0200 = 7,37 e 0243 = 4,50; matricula 657; emp3 88 linhas contra 86 | `col900: as rubricas 0200 (7,37) e 0243 (4,50) entram na 09 do Dominio por correcao LA.   OU   gera TXT novo da 09 com `--usuario` e `--motivo` meus.   OU   ficam fora da 09.` |
| 7 | `RELAVRATURA-10-PAROU-DIFF-SURPREENDE` | **!** | 2026-10-05T03:32 | 36 de 572 fechamentos mexidos; minutos_abonados +20.860 min (+347,67 h) em 25 colabs; horas_trabalhadas +32,39 h em 6; 30 dos 36 mexem campo FORA do alvo; previsto/falta em 4 colabs; DiaPago motor +39 linhas, oraculo +1. | `relavratura 10: aplica mesmo com a DERIVA junto -- os 25 colabs de `minutos_abonados` (+347,67 h) e os 4 de previsto/falta entram no mesmo ato, e a condicao 2 sai por `bin/snapshot_relavratura_10_2026.py`.   OU   aplica SO o realizado dos dia-colab da cura, e a deriva vira fatia propria com o seu nu` |
| 8 | `PAUTA-DP-09-RELAVRATURA` | **!** | 2026-10-05T03:32 | 291 dia-colab / 94 colabs / +142.778 min = +2.379,63 h a MAIS do que o Dominio recebeu na 09; 155 dia-colab / 43 colabs de deriva preexistente ficam de fora | `a 09 recebe as +2.379,63 h de 94 colabs por TXT de retificacao pela porta da REGEN-EM-EXPORTADA, com `--usuario` e `--motivo` meus.   OU   entra por correcao LA no Dominio, e o sistema so guarda a pauta.   OU   fica na 09 como esta e a diferenca aparece na 10.` |

---

Total no JSON: **210** · aberto **8** · respondido **41** · sem-motivo **161**.

> `sem-motivo` nao e "resolvido": e *"ninguem julgou este item"*. A ordem de 18:4x proibe
> triar os 163 antigos, entao eles ficam ai, nomeados, em vez de serem chutados para um lado.

