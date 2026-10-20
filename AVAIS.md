# AVAIS NA MESA — 4

> Gerado por `bin/gerar_avais.py` a partir de `app/docs/PENDENTES_RONALD.json`.
> **So itens ABERTOS.** Item respondido SOME daqui na proxima geracao -- a historia dele fica
> no JSON e no RELATO, nunca aqui. Ordem: o que trava a fila 1 primeiro, depois o mais antigo.

| # | id | tipo | desde | o numero | frase PRONTA para colar |
|---|---|---|---|---|---|
| 1 | `APPLY-DO-LOTE-DA-10-COM-O-DIFF-RE-MEDIDO` | **!** | 2026-10-20 | O DIFF de frota do lote da O261 na competencia **10** esta RE-MEDIDO na base de HOJE e PUBLICADO (`logs/o261/DIFF_FROTA_10_20-10.md`; sombra `dia=20261020 completa diverge=0`, arvore `affa98a7` que e a que PROD roda, `BA | `! aplica o lote como esta -- entra +43,83 h de `0200` (feriado/folga 100%), sai -14,33 h de `0025` em 2 colabs, 8 dias a zero, recorte `--colaboradores` com os 50 ids do ensaio e reversao em disco   OU   ! a (B) se cura ANTES (o par de 1490 min nao nasce) e o lote espera a cura   OU   ! planta so os` |
| 2 | `APP-DESENHA-A-PALAVRA-DO-DIA` | **!** | 02/10 10:2x | O SERVIDOR JA ENTREGA, o app nativo ainda nao desenha (O21 ROTULO-DO-DIA-DECIDIDO, seu corte de 24/09 10:xx). `api_espelho_v2` passou a levar em cada dia `veredito_dia`, `palavra_dia`, `cor_dia`, `decidido_por` e `decidi | `smoke Ronald: o app nativo passa a desenhar `palavra_dia`/`cor_dia` no release (as chaves ja saem de `api_espelho_v2`, aditivas -- app velho ignora) -- sao 615 dia-colab com decisao humana na competencia e 117 deles NAO-abonados, que hoje chegam ao celular iguais a um abono.   OU   fica com o tick g` |
| 3 | `REGUA10-DUAS-CCT-NA-MESMA-COMPETENCIA` | **lei** | 2026-10-19 | A competencia 10 esta lavrada com DUAS convencoes dentro da MESMA empresa, e quem as partiu foi uma troca de cadastro feita HOJE, dois dias antes do export de 21/10. Medido na sombra carimbada (`dia=20261019 tipo=complet | `! o sind=3 fica COMO ESTA (`p5h=True`) e eu re-lavro a 10 pelo O317 -- 16 colabs, efeito **-326,13 h**, seis dos sete INVERTEM o sinal   OU   ! vale a cl.10: **voce desliga o `p5h` na tela** e eu re-lavro -- **-1.408,73 h em 61 linhas**, 446 colabs, ate 21/10` |
| 4 | `FURO-APURADO-DA-09-FICA-NULL-OU-SE-CARIMBA` | **!** | 2026-10-19 | A coluna `DiaPago.furo_apurado` esta **True em 0 de 33.886 linhas** na sombra carimbada de hoje (`logs/regua10/sonda_furo_apurado_19-10.out`), porque `ponto/services/fechamento.py::apurar_por_fatia` compunha 8 dos 14 cam | `! re-carimba a 09 depois que a cura pousar -- `apurar_furo_da_exportada --mes 9 --ano 2026 --apply --motivo "..."`, so a coluna `furo_apurado`, dinheiro e hash intactos e provados, reversao em disco   OU   a 09 fica como esta (NULL nas 11.367) e so a 10 em diante recebe o carimbo certo` |

---

Total no JSON: **261** · aberto **4** · respondido **96** · sem-motivo **161**.

> `sem-motivo` nao e "resolvido": e *"ninguem julgou este item"*. A ordem de 18:4x proibe
> triar os 163 antigos, entao eles ficam ai, nomeados, em vez de serem chutados para um lado.

