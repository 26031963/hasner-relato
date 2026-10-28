# AVAIS NA MESA — 5

> Gerado por `bin/gerar_avais.py` a partir de `app/docs/PENDENTES_RONALD.json`.
> **So itens ABERTOS.** Item respondido SOME daqui na proxima geracao -- a historia dele fica
> no JSON e no RELATO, nunca aqui. Ordem: o que trava a fila 1 primeiro, depois o mais antigo.

| # | id | tipo | desde | o numero | frase PRONTA para colar |
|---|---|---|---|---|---|
| 1 | `APP-DESENHA-A-PALAVRA-DO-DIA` | **!** | 02/10 10:2x | O SERVIDOR JA ENTREGA, o app nativo ainda nao desenha (O21 ROTULO-DO-DIA-DECIDIDO, seu corte de 24/09 10:xx). `api_espelho_v2` passou a levar em cada dia `veredito_dia`, `palavra_dia`, `cor_dia`, `decidido_por` e `decidi | `smoke Ronald: o app nativo passa a desenhar `palavra_dia`/`cor_dia` no release (as chaves ja saem de `api_espelho_v2`, aditivas -- app velho ignora) -- sao 615 dia-colab com decisao humana na competencia e 117 deles NAO-abonados, que hoje chegam ao celular iguais a um abono.   OU   fica com o tick g` |
| 2 | `RE-CARIMBO-DA-09-RECUSADO-EM-28-DIAS-SEM-LINHA` | **!** | 2026-10-20 | DRY na sombra carimbada de hoje (`logs/o140c/dry_09_20-10.out`; base `completa diverge=0`, `--cobre 2026-08-21 2026-09-20 = sim`, `lavra_de_prod=OK`), com a cura de `apurar_por_fatia` provada DENTRO da arvore (`9632cdbd` | `! carimba a 09 RECORTADA -- `apurar_furo_da_exportada --mes 9 --ano 2026 --apply --motivo "..." --colabs <todos menos os 24>`, ~10.500 linhas respondidas, os 24 colabs (268 linhas) ficam NULL e NOMEADOS no RELATO, ciente de que o furo COM linha errado pelo mesmo desvio nao foi medido   OU   ! a 09 e` |
| 3 | `SMOKE-DO-REENVIO-DO-ATESTADO-DEPOIS-DA-CURA` | **smoke** | 2026-10-20 | 4 POSTs de 18/10 07:06 deram 500 por `IntegrityError` na `uniq_ausencia_viva_por_tipo_e_dia`, 0 Ausencia criada, 4 anexos orfaos. Depois do deploy das 15:36: 0 requisicao em `POST /api/ausencias/` na janela medida, entao | `smoke do reenvio OK -- o atestado entrou (ou apareceu a recusa falada dizendo que ja existe atestado no dia), nenhum "erro ao enviar"   OU   AINDA FALHA: <o que a tela disse>, e ai o BO reabre com a mensagem nova como evidencia` |
| 4 | `SMOKE-DA-DECISAO-DE-HE-DEPOIS-DA-CURA` | **smoke** | 2026-10-22 | 2 ocorrencias de 500 medidas no errors.log em `/ponto/gestao-he/recusar-lote/` (as duas em 22/10 07:37:57) e 0 tentativa em `/decidir/` na janela medida; 11.550 testes verdes nao viram o defeito porque `TestCase` roda de | `smoke da HE OK -- autorizei (ou recusei) 1 dia pela tela e o lote tambem respondeu, sem 500 e sem "erro inesperado"   OU   AINDA FALHA: <o que a tela disse>, e ai o BO reabre com a mensagem nova como evidencia` |
| 5 | `SMOKE-FIO-LE-O-JUIZ-MODAL` | **smoke** | 2026-10-27T22:45 | **FIO-LE-O-JUIZ (BO-[nome] C) pronto na copia `wt-fio-2710-1835` @ 5d86c33b** (suite inteira 1fcc61e0 OK Ran 12403; prova do POST 17 OK). FRONT SEM SMOKE NAO SOBE: `templates/chamados/partials/modal_fio.html` mudou. Cl | `aval Ronald: SMOKE-FIO-LE-O-JUIZ-MODAL ok` |

---

Total no JSON: **270** · aberto **5** · respondido **104** · sem-motivo **161**.

> `sem-motivo` nao e "resolvido": e *"ninguem julgou este item"*. A ordem de 18:4x proibe
> triar os 163 antigos, entao eles ficam ai, nomeados, em vez de serem chutados para um lado.

