# AVAIS NA MESA — 4

> Gerado por `bin/gerar_avais.py` a partir de `app/docs/PENDENTES_RONALD.json`.
> **So itens ABERTOS.** Item respondido SOME daqui na proxima geracao -- a historia dele fica
> no JSON e no RELATO, nunca aqui. Ordem: o que trava a fila 1 primeiro, depois o mais antigo.

| # | id | tipo | desde | o numero | frase PRONTA para colar |
|---|---|---|---|---|---|
| 1 | `APP-DESENHA-A-PALAVRA-DO-DIA` | **!** | 02/10 10:2x | O SERVIDOR JA ENTREGA, o app nativo ainda nao desenha (O21 ROTULO-DO-DIA-DECIDIDO, seu corte de 24/09 10:xx). `api_espelho_v2` passou a levar em cada dia `veredito_dia`, `palavra_dia`, `cor_dia`, `decidido_por` e `decidi | `smoke Ronald: o app nativo passa a desenhar `palavra_dia`/`cor_dia` no release (as chaves ja saem de `api_espelho_v2`, aditivas -- app velho ignora) -- sao 615 dia-colab com decisao humana na competencia e 117 deles NAO-abonados, que hoje chegam ao celular iguais a um abono.   OU   fica com o tick g` |
| 2 | `RE-CARIMBO-DA-09-RECUSADO-EM-28-DIAS-SEM-LINHA` | **!** | 2026-10-20 | DRY na sombra carimbada de hoje (`logs/o140c/dry_09_20-10.out`; base `completa diverge=0`, `--cobre 2026-08-21 2026-09-20 = sim`, `lavra_de_prod=OK`), com a cura de `apurar_por_fatia` provada DENTRO da arvore (`9632cdbd` | `! carimba a 09 RECORTADA -- `apurar_furo_da_exportada --mes 9 --ano 2026 --apply --motivo "..." --colabs <todos menos os 24>`, ~10.500 linhas respondidas, os 24 colabs (268 linhas) ficam NULL e NOMEADOS no RELATO, ciente de que o furo COM linha errado pelo mesmo desvio nao foi medido   OU   ! a 09 e` |
| 3 | `SMOKE-DO-REENVIO-DO-ATESTADO-DEPOIS-DA-CURA` | **smoke** | 2026-10-20 | 4 POSTs de 18/10 07:06 deram 500 por `IntegrityError` na `uniq_ausencia_viva_por_tipo_e_dia`, 0 Ausencia criada, 4 anexos orfaos. Depois do deploy das 15:36: 0 requisicao em `POST /api/ausencias/` na janela medida, entao | `smoke do reenvio OK -- o atestado entrou (ou apareceu a recusa falada dizendo que ja existe atestado no dia), nenhum "erro ao enviar"   OU   AINDA FALHA: <o que a tela disse>, e ai o BO reabre com a mensagem nova como evidencia` |
| 4 | `EXPORT-DA-10-COM-RESIDUO-DE-8-COLABS` | **!** | 2026-10-22 | O lote do `!` eram 14 plantadas sobre 158 casos da base das 04:00; prod as 07:55 tinha 107 casos. Aplicado: 32 colabs / 67 casos -> 2 plantadas, 0 campo para baixo. RESIDUO: 8 colabs / 40 casos fora do export, entre eles | `! exporta a 10 assim -- o residuo de 8 colabs / 40 casos vira retificacao pela porta depois, ciente de que col723 vai ao Dominio com um dia_falta que o sistema tira depois   OU   segura o export: refaz o DIFF sobre base de prod-de-agora (sombra --refazer --incremental da janela), aplica o lote compl` |

---

Total no JSON: **266** · aberto **4** · respondido **101** · sem-motivo **161**.

> `sem-motivo` nao e "resolvido": e *"ninguem julgou este item"*. A ordem de 18:4x proibe
> triar os 163 antigos, entao eles ficam ai, nomeados, em vez de serem chutados para um lado.

