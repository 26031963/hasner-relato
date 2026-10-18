# AVAIS NA MESA — 4

> Gerado por `bin/gerar_avais.py` a partir de `app/docs/PENDENTES_RONALD.json`.
> **So itens ABERTOS.** Item respondido SOME daqui na proxima geracao -- a historia dele fica
> no JSON e no RELATO, nunca aqui. Ordem: o que trava a fila 1 primeiro, depois o mais antigo.

| # | id | tipo | desde | o numero | frase PRONTA para colar |
|---|---|---|---|---|---|
| 1 | `APP-DESENHA-A-PALAVRA-DO-DIA` | **!** | 02/10 10:2x | O SERVIDOR JA ENTREGA, o app nativo ainda nao desenha (O21 ROTULO-DO-DIA-DECIDIDO, seu corte de 24/09 10:xx). `api_espelho_v2` passou a levar em cada dia `veredito_dia`, `palavra_dia`, `cor_dia`, `decidido_por` e `decidi | `smoke Ronald: o app nativo passa a desenhar `palavra_dia`/`cor_dia` no release (as chaves ja saem de `api_espelho_v2`, aditivas -- app velho ignora) -- sao 615 dia-colab com decisao humana na competencia e 117 deles NAO-abonados, que hoje chegam ao celular iguais a um abono.   OU   fica com o tick g` |
| 2 | `O306-RESPOSTA-SEM-GRAVACAO-O-QUE-O-SISTEMA-FAZ` | **lei** | 18/10 03:1x | 1.500 perguntas de hora JA validadas sem o horario no espelho (sombra 18/10, universo 13.281, pela funcao REAL `chamados/juizes.py::_pergunta_materializada_de_fato`) | `corte Ronald: as 1.500 respostas que ficaram sem gravacao (1.019 sem plantio, 75 em outro marco, 406 sem slot derivavel) -- (a) VOLTAM a ser pedidas pela porta do colaborador, perguntando de novo o horario;   OU   (b) viram PAUTA DP com o dia e o marco, e o colaborador nao e incomodado de novo;   OU` |
| 3 | `O306-SMOKE-DO-COLAB-AO-SUPERVISOR` | **smoke** | 2026-10-18T05:24 | 4 curas no ar; o PRONTO do seu aval de 17/10 e LITERAL: 7 respostas gravadas e visiveis | `smoke Ronald: abri pelo app como colaborador e declarei a hora nas perguntas de marco faltante -- as 7 voltaram GRAVADAS e aparecem no espelho (nenhuma disse 'enviado' sem registro); no supervisor apliquei a declaracao de uma delas e a batida nasceu no horario que o colaborador declarou, nao no marc` |
| 4 | `O306-BACKFILL-APPLY-EM-PROD` | **!** | 2026-10-18T05:25 | 10.578 de 37.131 perguntas mudam de `veredito_lavrado` na sombra de 18/10 (28,49%), e SO **197** delas (0,53%) sao da cura do O306 -- as outras 10.381 sao acervo que a lavra gravada nunca teve. Zero centavo se move. | `! Ronald: roda o `backfill_veredito_lavrado --apply` em prod -- as 10.578 linhas passam a ter o veredito que o juiz REAL ja da hoje (197 pela cura da O306, 10.381 de acervo), tres campos de cache e trilha, nenhum centavo.   OU   so o DRY em prod agora, e o apply depois do smoke da O306.   OU   nao r` |

---

Total no JSON: **249** · aberto **4** · respondido **84** · sem-motivo **161**.

> `sem-motivo` nao e "resolvido": e *"ninguem julgou este item"*. A ordem de 18:4x proibe
> triar os 163 antigos, entao eles ficam ai, nomeados, em vez de serem chutados para um lado.

