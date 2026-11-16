# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 01:15** · publicado em **16/11/2026 01:16** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 16/11 01:15 · sessao bf44a775e492 · RESPOSTAS-BO-1611 · censo · origem=Code

**A partição v1 soma 131, mas não está demonstrada como partição exclusiva de pessoa+dia.** Os anexos individualizam 126 IDs canônicos; há sobreposições entre raízes e casos classificados como “sem dia-colab” que já têm ambos. (app/docs/RELATO.md:429; anexo estrutura18.tsv:1–18; anexo final67.tsv:2–38; anexo mapa_hj_sem_nome.tsv:1–80)

Uso a numeração dos anexos contando sua primeira linha de conteúdo como linha 1; no código, cito a numeração desta cópia. Identidade Git do HEAD: **[nao verificado]**.

**0 — Conferência da partição**

| Conferência | Resultado e prova |
|---|---|
| ESTRUTURA histórica | Os 18 estão discriminados: nove NO AR, sete com obra pendente e dois sem cobertura comprovada. (anexo resolve.tsv:2–19) |
| Triagem dos 37 | 27 SEM DIA-COLAB, quatro BATIDA, quatro CADASTRO, dois ESTRUTURA. (anexo final67.tsv:2–38) |
| Ingestão | 80 unidades = seis ESTRUTURA + três CADASTRO + cinco BATIDA + 33 sem divergência histórica + 33 indeterminadas. (app/docs/RELATO.md:8916–8921) |
| IDs individualizados | `80 + 18 + 37 − 6 − 3 = 126`: seis ESTRUTURA já pertencem à ingestão; três indeterminados reaparecem em final67. (anexo estrutura18.tsv:1–6; anexo final67.tsv:12–14; anexo mapa_hj_sem_nome.tsv:1–80) |
| Diferença para 131 | O p0 declara sete CADASTRO e seis BATIDA; o mapa individualiza três e cinco. Faltam, portanto, individualizar **quatro CADASTRO e um BATIDA** para demonstrar o total declarado. Identidades desses cinco no p0: **[nao verificado]**. (app/docs/RELATO.md:429; app/docs/RELATO.md:8917–8918) |

As sobreposições encontradas são estas:

| Pessoa+dia | Raízes/baldes envolvidos | Prova |
|---|---|---|
| col42, 08/10 e 15/10 | BO-ADMIN-2710/1 e HJ-005-U2/U4: duas raízes dentro de “não reconferido”. | app/docs/RELATO.md:3625; anexo mapa_hj_sem_nome.tsv:8,10 |
| col42, 22/10 | BO-ADMIN-2710/1 em “não reconferido”; HJ-005-U5 em BATIDA. | app/docs/RELATO.md:3625; anexo mapa_hj_sem_nome.tsv:11 |
| col247, 24/10 | BO-ADMIN-2710/5 em “não reconferido”; HJ-031-U1 em “falta informação”. | app/docs/RELATO.md:3905; anexo final67.tsv:13; anexo mapa_hj_sem_nome.tsv:51 |
| col130, 05/10 | BO-ADMIN-2710/7 em “não reconferido”; HJ-018-U1 nos 33 históricos sem divergência. | app/docs/BACKLOG.md:835; anexo mapa_hj_sem_nome.tsv:33 |
| col789, 11/10 | Nominal RT-04 da BO-ADMIN-2710/3, com obra; HJ-004-U1 nos 33 históricos sem divergência. | app/docs/BACKLOG.md:1956; anexo mapa_hj_sem_nome.tsv:5 |

Essas raízes podem denunciar sintomas diferentes, mas **não formam unidades pessoa+dia exclusivas**. Não as fundi automaticamente. (app/docs/CORTES.md:373; app/docs/RELATO.md:3905; app/docs/BACKLOG.md:1956)

Também há três classificações “SEM DIA-COLAB” materialmente incorretas:

- HJ-012-U1: col239, 2026-10-07; discordância das testemunhas. (anexo final67.tsv:12; anexo mapa_hj_sem_nome.tsv:18)
- HJ-031-U1: col247, 2026-10-24; discordância das testemunhas. (anexo final67.tsv:13; anexo mapa_hj_sem_nome.tsv:51)
- HJ-050-U2: col523, 2026-11-07; era futuro na leitura de 05/11. (anexo final67.tsv:14; anexo mapa_hj_sem_nome.tsv:72; app/docs/RELATO.md:8920)

Portanto, não certifico um quadro corrigido exclusivo de 131: faltam os cinco IDs do p0 e a definição de tratamento das sobreposições. Os 33 permanecem como medida histórica, sem reconferência nova nesta resposta. (app/docs/RELATO.md:429; app/docs/RELATO.md:8919; app/docs/RELATO.md:8924)

**1 — Os nove “não reconferidos”**

A referência nominal privada da ingestão está em `logs/hj_0511/fonte` e `logs/hj_0511/mapa.md`; esses destinos são apontados no RELATO. Linhas internas contendo o casamento por CPF/nome: **[nao verificado]**. (app/docs/RELATO.md:8894; app/docs/RELATO.md:8924)

O lookup não oferece um juiz genérico de “caso fechado”: declara essa pergunta SEM juiz. Abaixo, FECHADO refere-se ao defeito indicado, com os limites escritos. (app/docs/JUIZES.md:109)

| Caso | Pessoa e dia escritos | Juiz real e esperado para FECHADO |
|---|---|---|
| HJ-005-U2 | col42, 2026-10-08. (anexo mapa_hj_sem_nome.tsv:8) | `ponto.turnos.periodo_da_parcial_no_dia`: esperado `fora`; `sem_periodo`/`nao_sei` não conferem. Para afirmar o que a tela mostra, consultar também `ponto.status_dia.status_do_dia_lavrado`. Cobertura no lookup: JUIZES:55–56; implementação: turnos.py:2628–2655; status: JUIZES:120; status_dia.py:201. |
| HJ-005-U4 | col42, 2026-10-15. (anexo mapa_hj_sem_nome.tsv:10) | Mesmo juiz e esperado do anterior. (app/ponto/turnos.py:2628–2655; app/docs/JUIZES.md:55–56,120) |
| HJ-030-U1 | col94, 2026-10-16; pergunta 44764, disputa 6984. (anexo mapa_hj_sem_nome.tsv:50; app/docs/RELATO.md:3964) | `chamados.juizes.pergunta_viva` + `chamados.services.fio.montar_chamados_fio`: se viva, deve aparecer no fio do admin. Viva e invisível = ABERTO; morta, isoladamente, não prova desfecho. O lookup só aponta o localizador de disputa; o registro vivo nomeia C6. (app/docs/JUIZES.md:40; app/core/juizes.py:286; app/chamados/juizes.py:865; app/chamados/services/fio.py:242–275) |
| BO-ADMIN-2710/1 | col42, 2026-10-08, 15 e 22. (app/docs/RELATO.md:3625) | Mesmo juiz de cobertura parcial; esperado `fora`, sem certificar outros furos ou dinheiro. (app/docs/JUIZES.md:55–56; app/ponto/turnos.py:2632–2637) |
| BO-ADMIN-2710/2 | col789; sete dias, somente 08/10 e 26/10 explicitados. **Mover para falta informação: lista exata dos sete dias.** (app/docs/RELATO.md:3625) | Geometria: `ponto.turnos.parear_turnos`/`turnos_do_colab`, lookup JUIZES:20. O nominal exige entrada reconhecida, pontualidade e HE correspondentes; um valor escalar de FECHADO para a raiz: **[nao verificado]**. (app/docs/CORTES.md:364; app/docs/JUIZES.md:20) |
| BO-ADMIN-2710/5 | col247, 2026-10-24. (app/docs/RELATO.md:3905) | Leitor real `_marcos_faltantes`: esperado nominal `int_volta`, tipo `E`, em vez de `int_ida/S`. Isso ainda exige conferir emissão e efeito; não basta fechar chamado. Não há linha específica desse leitor no lookup. (app/chamados/services/declaracao_texto.py:86–143; app/docs/CORTES.md:373; lookup específico **[nao verificado]**) |
| BO-ADMIN-2710/6 | col42/129/789, alvos sem todos os dias. Subcaso escrito: col129, 2026-10-25, chamado 35101. **Mover a raiz para falta informação:** dias/perguntas restantes e tela/perfil do supervisor. (app/docs/RELATO.md:3906; app/docs/CORTES.md:373–374) | C6 `pergunta_viva` e fio para invisibilidade; `qs_na_fila` para a fila. O subcaso depende expressamente da identificação da tela/perfil. (app/core/juizes.py:286; app/chamados/services/fio.py:242; app/chamados/services/painel.py:158; app/docs/CORTES.md:374) |
| BO-ADMIN-2710/7 | col130, 2026-10-05, chamado 28883. (app/docs/BACKLOG.md:835) | `chamados.services.painel.qs_na_fila(...).exists()`: esperado **False** para a pendência nominal. `registrado` não pede ação. Lookup: JUIZES:59–60,82; implementação painel.py:158–190. |
| BO-ADMIN-2710/8.1 | col789; ato de edição em 2026-10-27, 14:33–15:56; vínculos 1221/1472/1473. (app/docs/CORTES.md:362) | `colaboradores.services.vinculo.executar_vinculo` é escritor: **NAO_CONFERIDO, não chamar**. Juiz leitor que certifique fidelidade histórica de ação/antes/depois: **[nao verificado]**. A cura não reescreve a trilha histórica. (app/colaboradores/services/vinculo.py:170; app/docs/RELATO.md:3579) |

**2 — Os 57 rótulos originais “falta informação”**

“De quem” abaixo identifica a **fonte do relato**; não transfere ao relator medições internas. A origem HJ/Fernando está escrita no BACKLOG; BO-0611/Mauricio no PROMPTS; BO-1311/Mauricio na própria obra. (app/docs/BACKLOG.md:2233; app/docs/PROMPTS.md:1914; app/docs/BACKLOG.md:795)

| Caso | Dado que falta ou correção do rótulo | De quem — prova da origem |
|---|---|---|
| HJ-058 | Individualizar pessoa/dia/anexo entre quatro episódios HEIC; há datas textuais, não ausência absoluta de dia. (app/docs/BACKLOG.md:2633) | Fernando — app/docs/BACKLOG.md:2233 |
| HJ-066 | Dado externo necessário **[nao verificado]**; as duas dashboards já estão identificadas. (app/docs/BACKLOG.md:2626) | Fernando — app/docs/BACKLOG.md:2233 |
| HJ-068 | Dado externo necessário **[nao verificado]**; fio lateral, Histórico e contador já estão especificados. (app/docs/BACKLOG.md:2627) | Fernando — app/docs/BACKLOG.md:2233 |
| HJ-071 | Medir internamente se `hora_ini/hora_fim` estão gravadas; telas espelho/PDF conhecidas. (app/docs/BACKLOG.md:2628) | Fernando — app/docs/BACKLOG.md:2233 |
| HJ-073 | Censo interno por causa × permissão do supervisor. (app/docs/BACKLOG.md:2629) | Fernando — app/docs/BACKLOG.md:2233 |
| HJ-075 | Print do app mostrando escala antiga; col1032 conhecido. (app/docs/BACKLOG.md:2631) | Fernando — app/docs/BACKLOG.md:2233 |
| HJ-090 | Classificação interna L-099 dos 40 dias de férias com batida; individualização nessa linha **[nao verificado]**. (app/docs/BACKLOG.md:1932) | Fernando — app/docs/BACKLOG.md:2233 |
| HJ-INGESTAO-0511/HJ-012-U1 | Pessoa/dia presentes: col239, 07/10; falta resolver discordância das testemunhas. (anexo mapa_hj_sem_nome.tsv:18) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-INGESTAO-0511/HJ-031-U1 | Pessoa/dia presentes: col247, 24/10; falta resolver discordância das testemunhas. (anexo mapa_hj_sem_nome.tsv:51) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-INGESTAO-0511/HJ-050-U2 | Pessoa/dia presentes: col523, 07/11; precisa reconferência após a leitura que o considerava futuro. (anexo mapa_hj_sem_nome.tsv:72) | Fernando — app/docs/BACKLOG.md:1883 |
| BO-0611/02 | Qual é a queixa concreta; col78 já corrigido. Dia necessário **[nao verificado]**. (app/docs/RELATO.md:1934; app/docs/PROMPTS.md:1925) | Mauricio — app/docs/PROMPTS.md:1914 |
| BO-0611/03 | Motivos das 40 ocorrências do lote. (app/docs/RELATO.md:1934) | Mauricio — app/docs/PROMPTS.md:1914 |
| BO-0611/05 | Dia do episódio; col456 conhecido. (app/docs/RELATO.md:4; app/docs/RELATO.md:1934) | Mauricio — app/docs/PROMPTS.md:1914 |
| BO-0611/06 | Lacuna atual **[nao verificado]**; quadro antigo dizia conta de teste ativa, mas foi substituído. (app/docs/RELATO.md:1996; app/docs/PROMPTS.md:1925) | Mauricio — app/docs/PROMPTS.md:1914 |
| BO-0611/08 | Dia/prova atual **[nao verificado]**; registro substitutivo já diz col617, atestado aprovado, sem divergência. (app/docs/RELATO.md:1934) | Mauricio — app/docs/PROMPTS.md:1914 |
| BO-0611/09 | Dia/chamado individual **[nao verificado]**; col1000 e falta de porta antes da resposta conhecidos. (app/docs/RELATO.md:1934) | Mauricio — app/docs/PROMPTS.md:1914 |
| BO-0611/10 | Dia e troca individual **[nao verificado]**; col399 e ausência de UI conhecidos. (app/docs/RELATO.md:1934) | Mauricio — app/docs/PROMPTS.md:1914 |
| BO-0611/14 | Dia próprio do item 14 **[nao verificado]**; o turno 21/10 foi explicitamente destinado ao item 13. (app/docs/RELATO.md:1997) | Mauricio — app/docs/PROMPTS.md:1914 |
| BO-1311-ROTEIRO/01 | Confirmar ficha↔número; lacuna individual de dia/declaração/retentor **[nao verificado]**. (app/docs/RELATO.md:685,689) | Mauricio — app/docs/BACKLOG.md:795 |
| BO-1311-ROTEIRO/02 | Confirmar ficha↔número; lacuna individual de dia/declaração/retentor **[nao verificado]**. (app/docs/RELATO.md:685,689) | Mauricio — app/docs/BACKLOG.md:795 |
| BO-1311-ROTEIRO/03 | Confirmar ficha↔número; lacuna individual de dia/declaração/retentor **[nao verificado]**. (app/docs/RELATO.md:685,689) | Mauricio — app/docs/BACKLOG.md:795 |
| BO-1311-ROTEIRO/05 | Confirmar ficha↔número; lacuna individual de dia/declaração/retentor **[nao verificado]**. (app/docs/RELATO.md:685,689) | Mauricio — app/docs/BACKLOG.md:795 |
| BO-1311-ROTEIRO/06 | Confirmar ficha↔número; lacuna individual de dia/declaração/retentor **[nao verificado]**. (app/docs/RELATO.md:685,689) | Mauricio — app/docs/BACKLOG.md:795 |
| BO-1311-ROTEIRO/07 | Confirmar ficha↔número; lacuna individual de dia/declaração/retentor **[nao verificado]**. (app/docs/RELATO.md:685,689) | Mauricio — app/docs/BACKLOG.md:795 |
| BO-1311-ROTEIRO/08 | Confirmar ficha↔número; lacuna individual de dia/declaração/retentor **[nao verificado]**. (app/docs/RELATO.md:685,689) | Mauricio — app/docs/BACKLOG.md:795 |
| BO-1311-ROTEIRO/09 | Confirmar ficha↔número; lacuna individual de dia/declaração/retentor **[nao verificado]**. (app/docs/RELATO.md:685,689) | Mauricio — app/docs/BACKLOG.md:795 |
| BO-ADMIN-2710/8.3 | Recorte dos dias afetados; pessoa/vínculo conhecidos, mecanismo já descrito. (app/docs/RELATO.md:3908) | Mauricio — app/docs/RELATO.md:3894,3898 |
| HJ-004-U2 | Dia; col789 presente. (anexo mapa_hj_sem_nome.tsv:6) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-007-U1 | Dia; col253 presente. (anexo mapa_hj_sem_nome.tsv:13) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-008-U1 | Dia; col283 presente. (anexo mapa_hj_sem_nome.tsv:14) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-017-U2 | Dia; col923 presente. (anexo mapa_hj_sem_nome.tsv:32) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-025-U1 | Dia; col270 presente. (anexo mapa_hj_sem_nome.tsv:42) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-026-U1 | Dia; col129 presente. (anexo mapa_hj_sem_nome.tsv:43) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-032-U1 | Dia; col163 presente. (anexo mapa_hj_sem_nome.tsv:52) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-033-U1 | Dia; col960 presente. (anexo mapa_hj_sem_nome.tsv:53) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-035-U1 | Dia; col949 presente. (anexo mapa_hj_sem_nome.tsv:56) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-036-U1 | Dia; col207 presente. (anexo mapa_hj_sem_nome.tsv:57) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-037-U1 | Dia; col332 presente. (anexo mapa_hj_sem_nome.tsv:58) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-038-U1 | Dia; col446 presente. (anexo mapa_hj_sem_nome.tsv:59) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-039-U1 | Dia; col89 presente. (anexo mapa_hj_sem_nome.tsv:60) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-040-U1 | Dia; col92 presente. (anexo mapa_hj_sem_nome.tsv:61) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-041-U1 | Dia; col231 presente. (anexo mapa_hj_sem_nome.tsv:62) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-042-U1 | Dia; col214 presente. (anexo mapa_hj_sem_nome.tsv:63) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-043-U1 | Dia; col235 presente. (anexo mapa_hj_sem_nome.tsv:64) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-044-U1 | Dia; col246 presente. (anexo mapa_hj_sem_nome.tsv:65) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-045-U1 | Dia; col250 presente. (anexo mapa_hj_sem_nome.tsv:66) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-046-U1 | Dia; col673 presente. (anexo mapa_hj_sem_nome.tsv:67) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-047-U1 | Dia; col194 presente. (anexo mapa_hj_sem_nome.tsv:68) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-048-U1 | Dia; col219 presente. (anexo mapa_hj_sem_nome.tsv:69) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-051-U1 | Dia; col123 presente. (anexo mapa_hj_sem_nome.tsv:73) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-052-U1 | Dia; col1023 presente. (anexo mapa_hj_sem_nome.tsv:74) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-052-U2 | Dia; col78 presente. (anexo mapa_hj_sem_nome.tsv:75) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-053-U1 | Pessoa e dia. (anexo mapa_hj_sem_nome.tsv:76) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-054-U1 | Pessoa e dia. (anexo mapa_hj_sem_nome.tsv:77) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-055-U1 | Pessoa e dia. (anexo mapa_hj_sem_nome.tsv:78) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-056-U1 | Dia; col270 presente. (anexo mapa_hj_sem_nome.tsv:79) | Fernando — app/docs/BACKLOG.md:1883 |
| HJ-056-U2 | Pessoa e dia. (anexo mapa_hj_sem_nome.tsv:80) | Fernando — app/docs/BACKLOG.md:1883 |

Agrupamento dos **57 rótulos originais**, sem fingir que todos são pedidos externos:

| Tipo | Quantidade | Lastro |
|---|---:|---|
| Dia ausente, pessoa presente | 26 | anexo mapa_hj_sem_nome.tsv:6–80 |
| Pessoa e dia ausentes | 4 | anexo mapa_hj_sem_nome.tsv:76–78,80 |
| Individualizar episódio/anexo HEIC | 1 | app/docs/BACKLOG.md:2633 |
| Demanda de tela já descrita; lacuna externa não demonstrada | 2 | app/docs/BACKLOG.md:2626–2627 |
| Medição/classificação interna | 3 | app/docs/BACKLOG.md:2628–2629,1932 |
| Print | 1 | app/docs/BACKLOG.md:2631 |
| Pessoa+dia presentes; discordância/reconferência | 3 | anexo mapa_hj_sem_nome.tsv:18,51,72 |
| Queixa/motivos do lote | 2 | app/docs/RELATO.md:1934 |
| Episódio/dia/prova individual não demonstrados | 5 | app/docs/RELATO.md:4,1934,1997 |
| Lacuna atual não determinada após substituição do quadro | 1 | app/docs/RELATO.md:1996; app/docs/PROMPTS.md:1925 |
| Correspondência e lacuna individual do roteiro não demonstradas | 8 | app/docs/RELATO.md:685,689 |
| Dias afetados pela desativação | 1 | app/docs/RELATO.md:3908 |
| **Soma** | **57** | anexo final67.tsv:2–38; anexo mapa_hj_sem_nome.tsv:1–80 |

BO-ADMIN-2710/2 e /6 acrescentam duas raízes incompletas aos pendentes; não foram inseridas artificialmente nas 57 linhas acima. (app/docs/RELATO.md:3625,3906; app/docs/CORTES.md:374)

**3 — Oito com obra e dois sem obra**

“Hoje” nesta tabela significa o estado documental da cópia; execução ou conclusão atual em produção: **[nao verificado]**.

| Caso | Obra | GRADE / BACKLOG | O que destrava |
|---|---|---|---|
| BO-ADMIN-2710/3 | JUSTIFICATIVA-PAGA-O-PERIODO | GRADE principal INTERROMPIDA; lista posterior PARADA, aguarda juiz. BACKLOG preserva dependência. (app/docs/GRADE-MESTRA.md:270,1758; app/docs/BACKLOG.md:1956) | Pouso de COMPETENCIA-PAGA-UM-JUIZ; certificar árvore/condições do opus, consumo do parcial e DIFF novo. (app/docs/GRADE-MESTRA.md:1758; app/docs/BACKLOG.md:1956) |
| HJ-017-U1 | O232 | REGISTRADA; A/B no ar, complemento da ata pendente. (app/docs/GRADE-MESTRA.md:854; app/docs/BACKLOG.md:1253) | Migrar terceiro leitor, RED das quatro batidas e impacto de frota; colisão documental com JUSTIFICATIVA. (app/docs/BACKLOG.md:1253; app/docs/GRADE-MESTRA.md:854) |
| BO2910-ESTRUTURA/01 | BO2910-ESTRUTURA | REGISTRADA, censo não fechado. (app/docs/GRADE-MESTRA.md:170,7; app/docs/BACKLOG.md:1116) | Censo da classe de duas portas de posto/empresa e exceção engolida. (app/docs/BACKLOG.md:1116) |
| BO2910-ESTRUTURA/02 | BO2910-ESTRUTURA | REGISTRADA, censo não fechado. (app/docs/GRADE-MESTRA.md:170,7; app/docs/BACKLOG.md:1116) | Censo da classe de chamado de texto livre sem leitor da célula. (app/docs/BACKLOG.md:1116) |
| BO2910-ESTRUTURA/05 | BO2910-ESTRUTURA | REGISTRADA, censo não fechado. (app/docs/GRADE-MESTRA.md:170,7; app/docs/BACKLOG.md:1116) | Censo; tela precisa exigir período na aprovação/edição da parcial. (app/docs/BACKLOG.md:1116) |
| BO2910-ESTRUTURA/06 | BO2910-ESTRUTURA | REGISTRADA, censo não fechado. (app/docs/GRADE-MESTRA.md:170,7; app/docs/BACKLOG.md:1116) | Censo da vigência no desligamento e reabertura na reativação. (app/docs/BACKLOG.md:1116) |
| BO2910-ESTRUTURA/08 | BO2910-ESTRUTURA | REGISTRADA, censo não fechado. (app/docs/GRADE-MESTRA.md:170,7; app/docs/BACKLOG.md:1116) | Censo do veredito admin que sobrevive à regeneração com marcos novos. (app/docs/BACKLOG.md:1116) |
| HJ-074 | FERIADO-ESCALA → FERIADO-USA-A-LEI-DE-1609 | GRADE EM VOO, escopo anterior superado; lista posterior PAREI 3(ii). BACKLOG PARADA no desenho G. (app/docs/GRADE-MESTRA.md:127,1798; app/docs/BACKLOG.md:729) | Corte `! FERIADO-G item 3(ii)`; construção permanece parada pela condição medida. (app/docs/GRADE-MESTRA.md:1798; app/docs/RELATO.md:48) |

| Sem obra | Descrição e complemento cabível |
|---|---|
| HJ-005-U1 | col42, 01/10: ingestão acusa irregularidade e retenção por fato em ausência/furo/indefinição. (anexo mapa_hj_sem_nome.tsv:7) Cabe **complemento de censo em BO-ADMIN-2710**, aberta “medindo”, para reconciliar dono e destino; cobertura causal por COBERTURA-PARCIAL não está demonstrada para 01/10. Há ainda destino histórico “decisão do admin” para o recorte 01/09-10, que precisa ser reconciliado. (app/docs/BACKLOG.md:832,841; app/docs/CORTES.md:373) |
| HJ-012-U2 | col239, 12/10: ingestão acusa `espelho_cobrado`. (anexo mapa_hj_sem_nome.tsv:19) Cabe **complemento de

[truncado em 20000 de 31904 caracteres]

## 16/11 01:11 · sessao ced55a2548b4 · MIUDOS-NA-FILA-DE-LOTES · verificador · origem=Code

**AVAL-DEVOLVIDO: item (3). Itens (1) e (2) seguem separados**, conforme a cláusula de devolução do próprio aval. Motivos: premissa da fila sem regra localizada e condições de pouso incompatíveis com as regras escritas. Referências detalhadas abaixo. — `anexo:miudos_na_fila_de_lotes.txt:1–2`; `CLAUDE.md:320–329`. [nao verificado]

Nos anexos, conto as linhas a partir de “aval Ronald”, sem contar quebras visuais da interface.

**1. As premissas conferem?**

- **(1) NAO PROCEDE quanto à existência dos insumos previstos nos avais.** Existe o item (3) do COMPLEMENTO-1: relacionar os oito casos com obra e caracterizar/registrar os dois sem obra. Existe também a determinação de produzir `V0/causas.md`, por causa com `arquivo:linha` e destino. — `anexo:complemento1_respostas_bo_1611.txt:1`; `anexo:espelho_verdade_fecho_v0.txt:1`.

  **A entrega desses insumos permanece [nao verificado].** O registro da cópia apresenta os baldes 8/2/9, mas ainda põe `causas.md` depois da medição R1. Os nove só entram no universo pretendido quando reconferidos e classificados como nossos; não são nove causas nossas já demonstradas. — `app/docs/RELATO.md:4`; `app/docs/RELATO.md:9`; `anexo:miudos_na_fila_de_lotes.txt:1`.

- **(2) NAO PROCEDE.** O quadro por causa é uma entrega solicitada, não uma premissa de arquivo já existente. A correspondência concreta entre casos BO/HJ, dias ESTRUTURA e uma mesma causa está **[nao verificado]**. — `anexo:miudos_na_fila_de_lotes.txt:1`.

- **(3) PROCEDE quanto à premissa da fila.** Não localizei na cópia uma regra escrita da **“fila de LOTES das laterais de 15/11”** que determine quem entra, quando anda e como pousa: **[nao verificado]**. A evidência positiva encontrada são obras numeradas: **LOTE 7 = FATO-DE-AUSENCIA-ACORDA-A-PROJECAO** e **LOTE 3 = SITUACAO-DESLIGADO**, com condições próprias. Isso não demonstra uma fila geral aberta a novos miúdos. — `app/docs/RELATO.md:223–224`.

  **A vaga do Codex construtor existe por escrito e no código:** `--construtor`, uma vaga por repositório em `logs/construtor.vaga`, protegida por `flock`. Sua ocupação atual está **[nao verificado]**. — `CLAUDE.md:353–356`; `bin/codex_pergunta.sh:72–76`; `bin/codex_pergunta.sh:248–253`.

  Portanto, aplica-se a própria salvaguarda do aval: sem regra escrita de entrada identificada, **o item (3) para e registra a pergunta**. Isso não permite concluir que a fila inexiste fora da cópia. — `anexo:miudos_na_fila_de_lotes.txt:1`.

**2. Colide com aval vigente?**

- **(1) NAO PROCEDE.** Acrescentar classificação às leituras não exige construtor nem cura. É compatível com a leitura paralela sem vaga da CAUDA e com as proibições dos dois insumos. — `anexo:miudos_na_fila_de_lotes.txt:1`; `anexo:cauda_da_madrugada_1611.txt:1`; `anexo:complemento1_respostas_bo_1611.txt:1`; `anexo:espelho_verdade_fecho_v0.txt:1`.

- **(2) NAO PROCEDE.** Consolidar o quadro é registro; não autoriza curar dentro de RESPOSTAS-BO ou da V0. Permanecem as proibições de cura dessas obras. — `anexo:miudos_na_fila_de_lotes.txt:1`; `anexo:respostas_bo_1611.txt:1`; `anexo:espelho_verdade_fecho_v0.txt:1`.

- **(3) PROCEDE quanto ao encaixe na ordem vigente.** “Vaga ociosa e nenhuma volta esperando” não define a posição dos miúdos diante de F1 B2, LOTE 7 e LOTE 3. A CAUDA enumera essa ordem e determina somente leitura quando a lista se esgotar; o PREPARA também fixa prioridades. O próprio MIUDOS declara que não altera a ordem vigente. Falta a regra de fila que compatibilize essas cláusulas; um furo de fila efetivamente ocorrido está **[nao verificado]**. — `anexo:miudos_na_fila_de_lotes.txt:1,3`; `anexo:cauda_da_madrugada_1611.txt:1`; `anexo:prepara_volta_bug_par.txt:4`.

  **NAO PROCEDE quanto ao teto:** um Codex construtor e um miúdo aberto não aumentam o teto de um Codex mais até cinco opus. — `anexo:miudos_na_fila_de_lotes.txt:1,3`; `anexo:producao_maxima.txt:1`.

  **NAO PROCEDE como colisão automática com as proibições dos insumos.** MIUDOS contém uma autorização própria, condicional, de construção; isso não transforma RESPOSTAS-BO nem V0 em obras de cura. Uma cura concreta invadindo esses escopos está **[nao verificado]**. — `anexo:miudos_na_fila_de_lotes.txt:1,3`; `anexo:respostas_bo_1611.txt:1`; `anexo:espelho_verdade_fecho_v0.txt:1`.

**3. Alguma condição não fecha no literal?**

- **(1) NAO PROCEDE como contradição demonstrada.** “Um arquivo de produção + teste” não diz “somente dois caminhos no commit”. Docs obrigatórios podem coexistir com uma classificação por tamanho do código; a inclusão ou exclusão deles nessa contagem não está definida. Docs devem entrar no mesmo commit do marco. — `anexo:miudos_na_fila_de_lotes.txt:1`; `CLAUDE.md:332–336`. Uma exigência de **outro arquivo separado de selo em toda cura**, além do teste, está **[nao verificado]**. [nao verificado]

- **(2) NAO PROCEDE.** Consolidar por causa é compatível com a regra de curar a classe, em vez do caso. A classificação concreta continua dependente dos insumos. — `anexo:miudos_na_fila_de_lotes.txt:1`; `CLAUDE.md:65–70`.

- **(3) PROCEDE.** O gatilho **“cinco itens ou fim do dia”** permite espera além dos 30 minutos admitidos para formar comboio. A regra manda rodar sozinho quando não houver outra raia pronta nessa janela; raia verde pousa antes de abrir nova fatia da fila 1, e cura commitada com suíte verde vai ao ar imediatamente. — `anexo:miudos_na_fila_de_lotes.txt:1`; `CLAUDE.md:320–329`; `CLAUDE.md:425–427`. [nao verificado]

  **A retirada de reprovado também não fecha com a promessa irrestrita de “uma suíte inteira”.** A árvore restante precisa ser certificada: a suíte anterior não certifica uma árvore modificada. O COMBOIO prevê expressamente nova suíte de pouso após retirar obra reprovada. — `CLAUDE.md:320–324`; `anexo:comboio_pre_opus.txt:5`; `anexo:miudos_na_fila_de_lotes.txt:1`. [nao verificado]

  **“Um opus final” não dispensa opus por obra.** O COMBOIO exige final por obra, e toda obra construída pelo Codex passa por ele. O aval não define se os cinco itens constituem uma obra única ou várias; essa equivalência está **[nao verificado]**. — `anexo:comboio_pre_opus.txt:5`; `app/docs/LEIS.md:147`; `anexo:miudos_na_fila_de_lotes.txt:1`.

  **NAO PROCEDE quanto a “um commit por item com RED”, isoladamente.** RED evidenciado é obrigatório; não foi localizada exigência de commit separado para o RED **[nao verificado]**. Continuam obrigatórios docs do marco, mensagem LEI-AKITA completa e caminhos declarados. — `CLAUDE.md:28–29`; `CLAUDE.md:193`; `CLAUDE.md:332–336`; `bin/molde_fatia/LEIAME:42–50`; `bin/commit_so_o_declarado.sh:9–18`.

**4. Toca exportada, dinheiro ou `!` fora do declarado?**

- **(1) NAO PROCEDE como toque demonstrado.** O item pede leitura e sinalização desses alcances, sem cura ou apply. — `anexo:miudos_na_fila_de_lotes.txt:1,3`.

- **(2) NAO PROCEDE como toque demonstrado.** A entrega declarada é um quadro Markdown; não há alteração de produção descrita nesse item. — `anexo:miudos_na_fila_de_lotes.txt:1`.

- **(3) NAO PROCEDE como toque efetivamente demonstrado; alcance real [nao verificado].** O literal exclui dinheiro/exportada/API, proíbe apply e preserva parada por `!`, mas não há causas individualizadas nem diff para conferir o alcance. O filtro de tamanho não substitui PARADA item 2: escala/vínculo, apagar ou voltar arquivo usado em produção, regra fora do pedido, atalhos e motor v2 continuam exigindo `!`; a L-126 também proíbe ao Codex construtor item que peça `!`. — `anexo:miudos_na_fila_de_lotes.txt:1,3`; `CLAUDE.md:265–268`; `app/docs/LEIS.md:147`.

## 16/11 01:09 · sessao ee60cda9b763 · - · - · origem=Ronald (tmux)

Achados para consolidar, todos por leitura:

- **Pouso “5 itens ou fim do dia”: PROCEDE no item (3).** A regra escrita manda perguntar por outra raia pronta em até 30 minutos; sem isso roda sozinha, ninguém espera mais de 30 minutos, árvore certificada deve ser a que pousa e nenhum `.py` muda entre veredito e merge (`CLAUDE.md:320–324`; `app/docs/PISTA.md:29–40`). Raia verde pousa antes de abrir fatia nova na fila 1, com merge → commit → deploy no mesmo ato (`CLAUDE.md:325–329`; `app/docs/LEIS.md:131`). Cura commitada com suite verde vai ao ar imediatamente (`CLAUDE.md:425–427`). O anexo propõe outro gatilho de espera, sem nomear alteração dessas regras (`anexo:miudos_na_fila_de_lotes.txt:1`).

- **“Um opus final” para o lote: PROCEDE se substituir as leituras finais por obra.** Toda obra Codex exige opus final sem exceção (`app/docs/LEIS.md:147`), com pré-verificador e suite inteira verde antes dele (`CLAUDE.md:372–378`). O COMBOIO fornecido manda opus final **por obra** e, retirando uma reprovada, nova suite de pouso na árvore restante (`anexo:comboio_pre_opus.txt:5`, contando o cabeçalho como linha 1). O item (3) não explica como seu único opus preserva esse requisito (`anexo:miudos_na_fila_de_lotes.txt:1`).

- **“MIUDO = 1 arquivo de produção + teste” versus docs: ambiguidade, não impossibilidade demonstrada.** Docs entram no mesmo commit do marco; commit só de docs fora de marco não existe, exceto derivado do pós-push (`CLAUDE.md:332–336`; `app/docs/LEIS.md:132`). Se o limite contar todos os caminhos, exclui docs obrigatórios; se medir só produção, pode coexistir. O anexo não declara essa distinção (`anexo:miudos_na_fila_de_lotes.txt:1`). Também não achei regra dizendo que toda cura precisa de **um segundo arquivo separado** de selo: o próprio teste pode cumprir esse papel `[nao verificado]`.

- **“Um commit por item com RED”: NAO PROCEDE como conflito isolado.** RED evidenciado é exigido (`CLAUDE.md:28–29`, `CLAUDE.md:193`), e não achei exigência de commit separado para RED `[nao verificado]`. Permanecem obrigatórios linha LEI-AKITA completa e padrão existente (`CLAUDE.md:62–70`; `bin/molde_fatia/LEIAME:42–50`), citação das leis aplicáveis (`app/docs/LEIS.md:31`) e commit por caminhos declarados (`bin/commit_so_o_declarado.sh:9–18`). O hook julga a mensagem no commit (`bin/commit_msg.sh:2–11`, `bin/commit_msg.sh:29–31`).

- **Retirar reprovado sem segurar outros:** a árvore mudou; precisa nova certificação antes de pousar, conforme regra de árvore (`CLAUDE.md:320–324`) e COMBOIO item 4 (`anexo:comboio_pre_opus.txt:5`). O literal “uma suite inteira” não fecha se significar exatamente uma execução mesmo após retirada (`anexo:miudos_na_fila_de_lotes.txt:1`). [nao verificado]

- **Itens (1) e (2):** estas incompatibilidades de construção/pouso não os bloqueiam; o próprio aval separa a leitura/registro do item (3) (`anexo:miudos_na_fila_de_lotes.txt:1–2`). [nao verificado]

## 16/11 01:09 · sessao 82102c00022d · - · - · origem=Ronald (tmux)

- **Fila de LOTES: regra de entrada não localizada na cópia [nao verificado].** A evidência positiva localizada é de **obras numeradas**: LOTE 7 = FATO-DE-AUSENCIA-ACORDA-A-PROJECAO e LOTE 3 = SITUACAO-DESLIGADO, cada uma com cópia, RED, suíte, opus e condições próprias de pouso (`app/docs/RELATO.md:223`, `app/docs/RELATO.md:224`). O despacho de laterais fala em LOTE 1 e “lotes 2+ aguardam defeitos_por_classe.md final”, sem estabelecer uma fila aberta para novos miúdos (`app/docs/RELATO.md:230`, `app/docs/RELATO.md:232`). A GRADE registra LOTE7 e LOTE3 como obras próprias (`app/docs/GRADE-MESTRA.md:1762`, `app/docs/GRADE-MESTRA.md:1800`).
- **Não confundir comboio com entrada nessa fila.** A regra geral existente manda perguntar se outra raia ficará pronta em até 30 minutos, juntar em uma árvore certificada ou rodar sozinha, sem esperar mais de 30 minutos (`app/docs/PISTA.md:29`, `app/docs/PISTA.md:31`, `app/docs/PISTA.md:32`, `app/docs/PISTA.md:39`). Ela não define entrada de miúdos em “fila de LOTES” [nao verificado].
- **Vaga do Codex construtor confirmada.** O contrato escrito nomeia `--construtor` e uma vaga por repositório em `logs/construtor.vaga`, com `flock` (`CLAUDE.md:353`, `CLAUDE.md:355`). O código aceita o argumento (`bin/codex_pergunta.sh:62`), deriva a vaga do `git-common-dir` do repositório vivo (`bin/codex_pergunta.sh:72`, `bin/codex_pergunta.sh:76`) e recusa ocupação concorrente com `exec 6>>"$VAGA"` / `flock -n 6` (`bin/codex_pergunta.sh:252`, `bin/codex_pergunta.sh:253`). A disponibilidade atual dessa vaga é **[nao verificado]**.
- **Busca realizada:** padrões relativos a fila/LOTES/entrada/laterais/miúdos em `CLAUDE.md`, `app/docs`, `bin`, `docs`, `.claude`, e busca ampliada em `.md`/`.txt` da cópia. A inexistência universal da fila não pode ser afirmada; seu contrato escrito de entrada permanece **[nao verificado]**.

## 16/11 01:07 · sessao 922017003b6d · - · - · origem=Ronald (tmux)

Q1 — tabela dos nove, com referências atuais do archive:

| Caso | Pessoa/dia escritos | Consequência |
|---|---|---|
| HJ-005-U2 | col42, 2026-10-08 — `mapa_hj_sem_nome.tsv:8` | Completo para sonda. |
| HJ-005-U4 | col42, 2026-10-15 — `mapa_hj_sem_nome.tsv:10` | Completo para sonda. |
| HJ-030-U1 | col94, 2026-10-16 — `mapa_hj_sem_nome.tsv:50`; `app/docs/CORTES.md:349` | Completo; pergunta nominal #44764, disputa #6984. |
| BO-ADMIN-2710/1 | col42, 08/15/22-10 — `app/docs/RELATO.md:3625`; `app/docs/CORTES.md:373` | Completo, mas agrega três dias; sobrepõe U2/U4/U5 no nível pessoa+dia. |
| BO-ADMIN-2710/2 | col789, sete dias; apenas 08/10 e 26/10 explicitados no relato — `app/docs/RELATO.md:3625` | **Falta informação:** lista exata dos sete dias; não inferir dos dias de RT-03. |
| BO-ADMIN-2710/5 | col247, 2026-10-24 — `app/docs/RELATO.md:3905`; `app/docs/CORTES.md:373` | Completo; pergunta esperada intervalo_volta, não intervalo_saida. |
| BO-ADMIN-2710/6 | col42/129/789, vários alvos sem dias — `app/docs/RELATO.md:3906`; subcaso col129 25/10 #35101 em `app/docs/CORTES.md:373` | **Falta informação:** dia e pergunta-alvo de cada pessoa para completar a raiz. |
| BO-ADMIN-2710/7 | col130, 2026-10-05, #28883 — `app/docs/CORTES.md:373`; `app/docs/BACKLOG.md:835` | Completo. |
| BO-ADMIN-2710/8.1 | col789, **ato de edição** em 2026-10-27 14:33–15:56; vínculos 1221/1472/1473 — `app/docs/CORTES.md:362` | Data do ato escrita; não é julgamento do dia de ponto. **NAO_CONFERIDO:** escritor não deve ser chamado. |

A referência nominal privada da ingestão mora em `logs/hj_0511/fonte` e `logs/hj_0511/mapa.md`, apontados por `app/docs/RELATO.md:8894` e `:8924`; linhas internas desses arquivos **[nao verificado]**.

Juízes e limites para Q4:

- **Cobertura parcial:** `ponto.turnos.periodo_da_parcial_no_dia`, `app/ponto/turnos.py:2628`, retorna `sem_periodo|dentro|fora|nao_sei`. `fora` é a resposta de ausência sem batida dentro do período (`:2632`); não certifica sozinho ausência de outros furos. `grade_da_celula`, `app/escala/services/leitor_celula.py:350`, transporta o julgamento lavrado `periodo_parcial` (`:429`). Horas noturnas exigem deslocamentos da cadeia (`app/ponto/turnos.py:2639`), nunca hora civil improvisada.
- **HJ-030:** `chamados.juizes.pergunta_viva`, `app/chamados/juizes.py:865`, registrado em `app/core/juizes.py:286`; comparar com `montar_chamados_fio`, `app/chamados/services/fio.py:190`. O fio deve incluir a viva em disputa fechada (`:242`, `:267`). Viva e visível fecha **o defeito de invisibilidade**; viva invisível = ABERTO; morta sozinha não prova resolução.
- **BO/5:** `_marcos_faltantes`, `app/chamados/services/declaracao_texto.py:86`, devolve `(estado, [(dia,pos,tipo,hora)])`. O esperado nominal é `int_volta`, tipo `E`, conforme `app/docs/CORTES.md:373`. Encontrar isso não prova emissão correta; conservar NAO_CONFERIDO sem conferir a pergunta emitida.
- **BO/7:** `chamados.services.painel.qs_na_fila(_base=ChamadoColaborador.objects.filter(pk=28883), agora=agora).exists()`; assinatura `app/chamados/services/painel.py:158`, corpo `:188`. Esperado `False` para o sintoma de pendência, pois `registrado` não pede ação (`:174`; catálogo `app/chamados/catalogo/motor.py:24`).
- **BO/8.1:** `executar_vinculo` é explicitamente executor de escrita (`app/colaboradores/services/vinculo.py:170`). Não chamar. Histórico antigo não é reescrito (`app/docs/RELATO.md:3579`). `vigencia_sem_trilha.medir()` não confere fidelidade do antes/depois; apenas existência de algum registro (`app/escala/services/vigencia_sem_trilha.py:43`).

O lookup não define um fechamento geral: `app/docs/JUIZES.md:109` declara essa pergunta “SEM juiz”. `status_do_dia_lavrado` devolve `status/rotulo/fonte`, sem certificar reparo de fio, geometria ou trilha (`app/ponto/status_dia.py:201`).

Para evitar falso sucesso da sonda, conferir incremento de `core.observ.engolidos()['total']` antes/depois de cada caso (`app/core/observ.py:60`): os leitores podem engolir exceções (`app/chamados/juizes.py:1476`; `app/chamados/services/fio.py:417`). O wrapper SQL também deve contar comandos recusados, além de lançar, para detectar recusa engolida.

## 16/11 01:07 · sessao 3d70cc65d2ed · - · - · origem=Ronald (tmux)

Fontes de origem escritas:

- HJ → Fernando: `app/docs/BACKLOG.md:2233` diz literalmente “casos BO Mauricio / HJ Fernando”.
- BO-0611 → Mauricio: `app/docs/PROMPTS.md:1914`; versão substitutiva em `app/docs/PROMPTS.md:1925`.
- BO-1311-ROTEIRO → Mauricio: `app/docs/BACKLOG.md:795` (`BO-[nome]-1311-ROTEIRO`). A correspondência dos números 01–10 segue a ordem do universo, declarada **contestável** em `app/docs/RELATO.md:689`.
- BO-ADMIN-2710/8.3 → Mauricio: encadeamento explícito do cabeçalho `BO-[nome]-FECHA` em `app/docs/RELATO.md:3894`, item 8.3 em `:3898` e resultado BO-ADMIN em `:3900`/`:3908`.

As referências dos anexos estão defasadas: por exemplo, a descrição BO-0611-V2 está hoje em `RELATO.md:1934`, e os itens BO-ADMIN em `RELATO.md:3901`–`:3908`.

| Caso | Dado faltante comprovável / limite |
|---|---|
| HJ-058 | Falta individualizar pessoa, dia e anexo concreto dos quatro casos; a linha já identifica HEIC e datas textuais 30/09 e 06/11, portanto “sem dia” absoluto é excessivo. `BACKLOG.md:2633`; individualização [nao verificado]. |
| HJ-066 | As telas já são conhecidas: duas dashboards do colaborador. Pessoa/dia não constituem requisito explicitado para essa demanda UI. `BACKLOG.md:2626`; dado externo necessário [nao verificado]. |
| HJ-068 | Tela já conhecida: fio lateral do admin, encerrados no Histórico e contador acionável. Dado externo faltante [nao verificado]. `BACKLOG.md:2627`. |
| HJ-071 | Falta **medir se hora_ini/hora_fim está gravada**, não presumir falta de informação do Fernando. Tela: espelho/PDF. `BACKLOG.md:2628`. Pessoa/dia concreto [nao verificado]. |
| HJ-073 | Falta censo por causa e permissão do supervisor; a linha não pede dado externo específico. `BACKLOG.md:2629`. |
| HJ-075 | Falta print do app mostrando escala antiga; col1032 já conhecido. `BACKLOG.md:2631`. |
| HJ-090 | Falta classificação por dono L-099 dos 40 dias de férias com batida; lista individual pessoa/dia nessa linha [nao verificado]. `BACKLOG.md:1932`. |
| HJ-012-U1 | Pessoa+dia **já fornecidos**: col239, 2026-10-07. Falta resolver discordância das testemunhas, não pedir identidade/dia. `mapa_hj_sem_nome.tsv:18` (anexo). |
| HJ-031-U1 | Pessoa+dia **já fornecidos**: col247, 2026-10-24. Falta resolver discordância das testemunhas. `mapa_hj_sem_nome.tsv:51` (anexo). |
| HJ-050-U2 | Pessoa+dia **já fornecidos**: col523, 2026-11-07; o obstáculo era dia futuro na leitura de 05/11. `mapa_hj_sem_nome.tsv:71` (anexo). |
| BO-0611/02 | Queixa concreta não identificada; col78 já conhecido na versão corrigida. `RELATO.md:1934`; `PROMPTS.md:1925`. Dia [nao verificado]. |
| BO-0611/03 | Faltam motivos das 40 ocorrências do lote; há problema já identificado no validar-lote. `RELATO.md:1934`; `BACKLOG.md:1924`. |
| BO-0611/05 | Falta dia do episódio; col456 já corrigido. `RELATO.md:4`; `RELATO.md:1934`; `PROMPTS.md:1925`. |
| BO-0611/06 | Registro antigo já identifica col950 e cadastro de conta de teste ativa; falta dado atual específico [nao verificado]. `RELATO.md:1996` (quadro depois substituído por `PROMPTS.md:1925`). | [nao verificado]
| BO-0611/08 | Registro substitutivo identifica col617, atestado aprovado e sem divergência. Dia e reconferência atual [nao verificado]. `RELATO.md:1934`. |
| BO-0611/09 | col1000 já conhecido; descrito sem porta antes da resposta. Dia/chamado concreto [nao verificado]. `RELATO.md:1934`. |
| BO-0611/10 | col399 já conhecido; troca sem UI identificada. Dia e troca concreta [nao verificado]. `RELATO.md:1934`. |
| BO-0611/14 | Grupo 13+14 já associado a col407; texto de turno 21/10 está explicitamente destinado ao item 13. Dia próprio do 14 [nao verificado]. `RELATO.md:1995`; `RELATO.md:1997`. |
| BO-1311-ROTEIRO/01 | Pela ordem contestável, col1019; falta localizar ficha/dias e declarações ou projeção do retentor correspondentes. `BACKLOG.md:795`; `RELATO.md:689`; necessidade coletiva `RELATO.md:685`; vínculo individual da lacuna [nao verificado]. |
| BO-1311-ROTEIRO/02 | Mesmo limite, col996 pela ordem contestável. Mesmas fontes. |
| BO-1311-ROTEIRO/03 | Mesmo limite, col1032 pela ordem contestável. Mesmas fontes. |
| BO-1311-ROTEIRO/05 | Mesmo limite, col265 pela ordem contestável. Mesmas fontes. |
| BO-1311-ROTEIRO/06 | Mesmo limite, col982 pela ordem contestável. Mesmas fontes. |
| BO-1311-ROTEIRO/07 | Mesmo limite, col1004 pela ordem contestável. Mesmas fontes. |
| BO-1311-ROTEIRO/08 | Mesmo limite, col523 pela ordem contestável. Mesmas fontes. |
| BO-1311-ROTEIRO/09 | Mesmo limite, col866 pela ordem contestável. Mesmas fontes. |
| BO-ADMIN-2710/8.3 | col789 e vínculo1473 conhecidos; falta recorte pessoa/dia da consequência denunciada, não identidade. Mecanismo e complemento E4 já registrados. `RELATO.md:3908`; `RELATO.md:3898`. |

Não é sustentável chamar todos esses 27 de “sem pessoa/dia”: três HJ possuem ambos no próprio mapa, diversos BO têm colPK corrigido e vários HJ são demandas de tela ou censo interno. `mapa_hj_sem_nome.tsv:18`, `:51`, `:71`; `RELATO.md:1934`; `BACKLOG.md:2626`–`:2629`.

## 16/11 01:06 · sessao b388a9727363 · - · - · origem=Ronald (tmux)

Q3 — linhas do HEAD da cópia, sem confirmação em produção:

| Caso | Obra | Estado na GRADE / BACKLOG | O que destrava |
|---|---|---|---|
| BO-ADMIN-2710/3 | JUSTIFICATIVA-PAGA-O-PERIODO | GRADE registra INTERROMPIDA; quadro posterior registra PARADA aguardando juiz. BACKLOG registra retorno à ordem viva, preservando a dependência do juiz. `app/docs/GRADE-MESTRA.md:270`, `:1758`; `app/docs/BACKLOG.md:1956`. | Pouso de COMPETENCIA-PAGA-UM-JUIZ; depois certificar árvore e condições do opus, consumo do parcial por motor/folha e DIFF novo. `app/docs/GRADE-MESTRA.md:1758`; `app/docs/BACKLOG.md:1956`. |
| HJ-017-U1 | O232 | REGISTRADA; A/B no ar, complemento da ata pendente. `app/docs/GRADE-MESTRA.md:854`; `app/docs/BACKLOG.md:1253`. | Migrar o terceiro leitor, a ata; RED com quatro batidas e impacto de frota antes. Colisão documental com JUSTIFICATIVA consta da GRADE. `app/docs/BACKLOG.md:1253`; `app/docs/GRADE-MESTRA.md:854`. |
| BO2910-ESTRUTURA/01 | BO2910-ESTRUTURA | REGISTRADA; `.+.......`, portanto censo falta conforme legenda. `app/docs/GRADE-MESTRA.md:170`, `:7`; `app/docs/BACKLOG.md:1116`. | Censo; classe: duas portas de vínculo/posto, exceção engolida e posto alterado sem empresa. `app/docs/GRADE-MESTRA.md:170`, `:7`; `app/docs/BACKLOG.md:1116`. |
| BO2910-ESTRUTURA/02 | BO2910-ESTRUTURA | REGISTRADA; censo falta. `app/docs/GRADE-MESTRA.md:170`, `:7`; `app/docs/BACKLOG.md:1116`. | Censo; classe: chamado de texto livre sem leitor da célula. `app/docs/BACKLOG.md:1116`; `app/docs/GRADE-MESTRA.md:170`, `:7`. |
| BO2910-ESTRUTURA/05 | BO2910-ESTRUTURA | REGISTRADA; censo falta. `app/docs/GRADE-MESTRA.md:170`, `:7`; `app/docs/BACKLOG.md:1116`. | Censo; classe: parcial aprovada pela tela sem período; período obrigatório na aprovação/edição. `app/docs/BACKLOG.md:1116`; `app/docs/GRADE-MESTRA.md:170`, `:7`. |
| BO2910-ESTRUTURA/06 | BO2910-ESTRUTURA | REGISTRADA; censo falta. `app/docs/GRADE-MESTRA.md:170`, `:7`; `app/docs/BACKLOG.md:1116`. | Censo; classe: desligamento escreve fim sem conferir vigência e reativação não reabre vínculo. `app/docs/BACKLOG.md:1116`; `app/docs/GRADE-MESTRA.md:170`, `:7`. |
| BO2910-ESTRUTURA/08 | BO2910-ESTRUTURA | REGISTRADA; censo falta. `app/docs/GRADE-MESTRA.md:170`, `:7`; `app/docs/BACKLOG.md:1116`. | Censo; classe: veredito admin sobrevive à regeneração com novos marcos. `app/docs/BACKLOG.md:1116`; `app/docs/GRADE-MESTRA.md:170`, `:7`. |
| HJ-074 | FERIADO-ESCALA, escopo substituído por FERIADO-USA-A-LEI-DE-1609 | GRADE principal EM VOO, escopo anterior superado; quadro posterior EM VOO—PAREI 3(ii). BACKLOG SUPERADA e PARADA no desenho G. `app/docs/GRADE-MESTRA.md:127`, `:1798`; `app/docs/BACKLOG.md:729`. | Corte `! FERIADO-G item 3(ii)`; RELATO diz ENTREGUE FORA DO LOTE=1, col945, e construção (a) não abre. `app/docs/GRADE-MESTRA.md:1798`; `app/docs/RELATO.md:48`. |

Os dois sem obra:

- **HJ-005-U1:** col42, 01/10, irregular, retendo por `espelho_fato_em_ausencia`, `espelho_furo`, `espelho_indefinida` — anexo `mapa_hj_sem_nome.tsv:7`. O HEAD conserva “candidata sem prova” (`app/docs/RELATO.md:429`). Cabe **complemento de censo** em **BO-ADMIN-2710**, aberta “medindo”, que exige número+dono+destino por item (`app/docs/BACKLOG.md:832`). Vínculo causal específico do dia 01/10 com seus achados: **[nao verificado]**. Não afirmar cobertura da COBERTURA-PARCIAL-UM-JUIZ: sua linha só nomeia col42 em 08/15/22-10 (`app/docs/BACKLOG.md:841`).
- **HJ-012-U2:** col239, 12/10, irregular e retendo por `espelho_cobrado` — anexo `mapa_hj_sem_nome.tsv:19`. Cabe **complemento de reconciliação do censo** em **BUG-RETRATO-VELHO-NA-TELA**, obra aberta com partes “em cópia” e “depois do 20/20”; seu item (d) trata especificamente feriado 12/10 e registra col239 como cobrança legítima, destino DP (`app/docs/BACKLOG.md:1555`). O relato confirma `trabalha=True` e destino DP (`app/docs/RELATO.md:5524`). Isso conflita com ESTRUTURA na ingestão histórica (`app/docs/RELATO.md:8916`); causa e classificação atuais **[nao verificado]**.

Observação para consolidação: linhas antigas anexadas deslocaram-se no HEAD: JUSTIFICATIVA `1918→1956`; BO2910 `1102→1116`; O232 `1239→1253`; BO-ADMIN `820→832`. Conteúdo verificado nas linhas atuais citadas.

## 16/11 00:54 · sessao bf1dd93cfcb9 · RESPOSTAS-BO-1611 · censo · origem=Code

**(A) Obra → casos ligados**

**Há 2 IDs do p0 com ligação recuperável; fechamento atual em prod: [nao verificado].** A ligação de BO-0611/05 alcança um complemento da classe, sem certificar o encerramento da queixa de GPS. (app/docs/RELATO.md:343; app/docs/BACKLOG.md:2074; app/docs/BACKLOG.md:2083; app/docs/RELATO.md:1929)

| Obra | Caso e linha que liga |
|---|---|
| E3 — `90f3fa66b` | **Não ligado: [nao verificado].** A grade publica “2” sem individualizar os casos. BO-1311/04 está expressamente destinado à E4B, com “NAO se cura na E3”. (app/docs/GRADE-MESTRA.md:119; app/docs/BACKLOG.md:2010) |
| HJ-091 — `8376da7aa` | **HJ-091** → HJ-091-LISTA, explicitamente escrito como NO AR. O registro do pouso documenta preservação das rejeitadas e uma lista de 60 para o DP. (app/docs/RELATO.md:343; app/docs/BACKLOG.md:750; app/docs/RELATO.md:483) |
| BATIDA-DEDUP-RETRATADA — `b6d268dfd` | **BO-0611/05**, pela ponte exata de sítio: o complemento nomeia a janela de duplicidade em `triagem_batida.py:662`; a obra nomeia esse mesmo sítio. É ligação ao complemento, não prova de fechamento integral do item. (app/docs/BACKLOG.md:2074; app/docs/BACKLOG.md:2083) |
| CONTRATO-DE-ENTRADA-DO-FIO — `77dfa94fa` | **Não ligado ao p0: [nao verificado].** O caso nomeado é HJ-095, ausente do rol fornecido; a indicação “1” do roteiro não individualiza uma ficha. (app/docs/BACKLOG.md:2224; app/docs/GRADE-MESTRA.md:688; p0_universo.r, anexo, seção B) |
| TELA-DIZ-O-QUE-O-SERVIDOR-DISSE — `fac43f42a` | **Não ligado: [nao verificado].** O registro identifica um laudo de 15/11. A possível ponte de BO05 por `pwa.js:158` fica fora do aval dessa obra. (app/docs/BACKLOG.md:1939; app/docs/BACKLOG.md:2074; app/docs/BACKLOG.md:2029) |
| HE-REVOGADA-NA-PORTA — `271ecba2e` | **Não ligado: [nao verificado].** Os registros descrevem portas e competência, sem individualizar caso do p0. (app/docs/RELATO.md:225; app/docs/BACKLOG.md:1951) |
| BUG-ENC-CELULAS-FORMA — `7825fec5b` | **Não ligado: [nao verificado].** A linha descreve a forma de `celulas`; o nominal do impacto é col174, 25/10, ligado a outra obra. (app/docs/BACKLOG.md:988; app/docs/RELATO.md:33; app/docs/BACKLOG.md:1759) |

A atribuição escrita das fontes é esta:

| Fonte | Atribuição comprovada |
|---|---|
| BO-0611 | Mauricio como destinatário: “texto ao Mauricio por item”. (app/docs/PROMPTS.md:1914) |
| BO-1311-ROTEIRO | Denominação **BO-[nome]-1311-ROTEIRO**; autoria do relato, além desse rótulo: **[nao verificado]**. (app/docs/BACKLOG.md:795) |
| HJ | Fernando: “56 HJ do Fernando → unidades pessoa+dia”. (app/docs/BACKLOG.md:1883) |
| BO-ADMIN-2710 | Há associação explícita de itens a BO-[nome]; atribuição escrita da fonte inteira: **[nao verificado]**. (app/docs/BACKLOG.md:833; app/docs/BACKLOG.md:835; app/docs/BACKLOG.md:841) |
| BO2910-ESTRUTURA | Identificada como triagem BO-ADMIN-2910; atribuição a Mauricio ou Fernando: **[nao verificado]**. (app/docs/BACKLOG.md:1116) |

**R3 (se pousar)**

Casos individualizados do p0 que a c17 fecharia: **[nao verificado]**. O escopo publicado reúne LU-01..07 e achados retirados do pouso da E3; o predesenho trata do universo dos retratos, alarme `cego`, C5 e relógio. Isso não fornece uma ligação individual de fechamento aos casos do p0. **Fora do total.** (app/docs/BACKLOG.md:770; app/docs/RELATO.md:61)

**(B) Conferência dos casos ligados**

| Caso; pessoa registrada; dia/competência | Perguntar a quem e o quê | Juiz real; esperado para FECHADO | O que o admin lê |
|---|---|---|---|
| **BO-0611/05 — col456**. Identificação: app/docs/RELATO.md:1929. Dia/competência: **[nao verificado]**. | Ao admin responsável pelo relato: identificar o dia e instante da tentativa, distinguindo a queixa de GPS do complemento de duplicidade. Responsável nominal: **[nao verificado]**. A V2 registra barreira GPS; o complemento registra deduplicação. (app/docs/RELATO.md:1929; app/docs/BACKLOG.md:2074) | A deduplicação consulta **`ponto.turnos.batidas_apuraveis`**, leitura em app/ponto/turnos.py:2868 e app/ponto/turnos.py:2888. Registro dessa função em JUIZES/core: **[nao verificado]**. Valor suficiente para FECHADO: **[nao verificado]**. **Não chamar `registrar_batida`**: cria batida e dispara fechamento. (app/ponto/registro_batida.py:152; app/ponto/registro_batida.py:156; app/ponto/registro_batida.py:158) | O DP lê pendências offline na view e o template apresenta o resultado de `tem_gps`, inclusive “Sem GPS”. Isso mostra a situação das marcações existentes, sem comprovar uma tentativa sem dia identificado. (app/ponto/views.py:437; app/ponto/views.py:444; app/templates/ponto/batidas_offline_pendentes.html:96) |
| **HJ-091 — col127 — 02/10/2026**, agendamento 114. Identificação e dia: app/docs/RELATO.md:749. Competência: **[nao verificado]**. | Ao **DP**: conferir o dia de agendamento sem ausência aprovada e decidir o passivo pela tela. O passivo é destinado à lista do DP. (app/docs/RELATO.md:749; app/ferias/services.py:136) | **`ponto.turnos.ausencia_cobre(..., para='folha')`**, registrado em app/docs/JUIZES.md:52; implementação em app/ponto/turnos.py:2550. Retorna ausência aprovada ou `None`. Valor suficiente para fechar este passivo: **[nao verificado]**; cobertura isolada não certifica a origem. O contador específico trata separadamente origem e dia sem ausência. (app/ponto/turnos.py:2528; app/ferias/services.py:135) | A view lê **`origem_de_ferias`**, que entrega agendamento, período e dia consultado; o template mostra esses dados e o caminho da Gestão de Férias. (app/ponto/views.py:2093; app/ponto/services/ausencia.py:101; app/templates/ponto/partials/painel_ausencia.html:135) |
| **HJ-091 — col344 — 05, 06 e 07/11/2026**, agendamento 103. Identificação/dias: app/docs/RELATO.md:489. Competência 11 aberta naquele registro. (app/docs/RELATO.md:489) | Ao **DP**: conferir a origem 103 das três rejeitadas, a preservação do abono 5592 e a ausência de novas férias aprovadas que contornem os vetos. Ao **Fernando**, homologar o caso; o registro reserva a ele o encerramento. (app/docs/RELATO.md:483; app/docs/RELATO.md:489; app/docs/RELATO.md:8919) | Mesmo juiz de cobertura: **`ponto.turnos.ausencia_cobre`**. Esperado estrutural documentado: rejeitadas preservadas com origem 103, abono 5592 intacto e sem novas férias aprovadas contornando os vetos. **Um juiz registrado cuja resposta certifique esse conjunto: [nao verificado].** (app/docs/JUIZES.md:52; app/docs/RELATO.md:483; app/docs/RELATO.md:489) | Mesmo painel de origem. A edição genérica some para férias derivadas; o painel encaminha à Gestão de Férias. (app/templates/ponto/partials/painel_ausencia.html:131; app/templates/ponto/partials/painel_ausencia.html:140; app/templates/ponto/partials/painel_ausencia.html:145) |

O contador **`ferias.services.ferias_espelho_x_agendamento`** é explicitamente somente leitura, com esperado zero nos dois sentidos; seu registro em JUIZES/core é **[nao verificado]**. Além disso, o pouso mantém passivo para decisão do DP. Portanto, não o substituo por um juiz suficiente para declarar o caso inteiro FECHADO. (app/ferias/services.py:135; app/ferias/services.py:143; app/docs/RELATO.md:483)

**(C) Universo não ligado às sete obras — contagem por fonte**

Contagem sobre as **68 linhas individualizadas do p0 fornecido**, descontando HJ-091 e BO-0611/05. O universo integral continua **[nao verificado]**: a ingestão declara 80 unidades e remete o mapa individual ao arquivo privado. (p0_universo.r, anexo, seções B–C; app/docs/RELATO.md:8907; app/docs/RELATO.md:8919)

| Fonte | Não ligados | Referência |
|---|---:|---|
| BACKLOG-HJ, por raiz | 10 | p0_universo.r, seção B; ligação descontada em app/docs/RELATO.md:343 |
| HJ-INGESTAO, IDs recuperáveis | 17 | p0_universo.r, seção B; app/docs/RELATO.md:8911; app/docs/RELATO.md:8912; app/docs/RELATO.md:8913; app/docs/RELATO.md:8915 |
| BO-0611 | 13 | app/docs/BACKLOG.md:1926; ligação descontada em app/docs/BACKLOG.md:2074 e app/docs/BACKLOG.md:2083 |
| BO-1311-ROTEIRO | 10 | app/docs/BACKLOG.md:795 |
| BO2910-ESTRUTURA | 5 | app/docs/BACKLOG.md:1116 |
| BO-ADMIN-2710 | 11 | p0_universo.r, seção C; app/docs/RELATO.md:3895; app/docs/RELATO.md:3903 |
| **Total documental** | **66** | Soma das linhas acima; completude integral **[nao verificado]** |

**(D) Script**

A sonda preserva os IDs do p0 e emite uma linha por pessoa+dia recuperável; HJ-091, contado como uma raiz no p0, possui os quatro pares abaixo documentados. Ela informa a cobertura, mas mantém **NAO_CONFERIDO** onde falta um juiz suficiente ou a identificação do fato. (p0_universo.r, seção B; app/docs/RELATO.md:749; app/docs/RELATO.md:489; app/docs/RELATO.md:1929)

```python
# SONDA RESPOSTAS-BO-1611
import sys
sys.dont_write_bytecode = True

import json
import re
from datetime import date

from django.db import connection
from django.utils import timezone
from django_tenants.utils import schema_context

# Identidades/dias:
# app/docs/RELATO.md:1929, :749, :489.
CASOS = [
    ("BO-0611/05", 456, None, "ponto.turnos.batidas_apuraveis"),
    ("HJ-091", 127, date(2026, 10, 2), "ponto.turnos.ausencia_cobre"),
    ("HJ-091", 344, date(2026, 11, 5), "ponto.turnos.ausencia_cobre"),
    ("HJ-091", 344, date(2026, 11, 6), "ponto.turnos.ausencia_cobre"),
    ("HJ-091", 344, date(2026, 11, 7), "ponto.turnos.ausencia_cobre"),
]


def somente_select(execute, sql, params, many, context):
    texto = sql.strip()
    if (
        not re.match(r"(?is)^SELECT\b", texto)
        or re.search(r"(?is)\bFOR\s+(UPDATE|SHARE)\b|\bINTO\b", texto)
        or ";" in texto
    ):
        raise RuntimeError("SQL bloqueado pela guarda de leitura")
    return execute(sql, params, many, context)


total_erros = 0
total_nao_conferidos = 0

for caso_id, pessoa_pk, dia, juiz in CASOS:
    resposta = None
    estado = "NAO_CONFERIDO"
    motivo = ""
    dia_saida = dia.isoformat() if dia else "[nao_verificado]"

    try:
        with schema_context("juliani"):
            with connection.execute_wrapper(somente_select):
                if connection.schema_name != "juliani":
                    raise RuntimeError("Tenant diferente do solicitado")

                from colaboradores.models import Colaborador

                # Busca exclusivamente por PK; não serializar o objeto.
                pessoa = Colaborador.objects.only("pk").get(pk=pessoa_pk)

                if dia is None:
                    # app/docs/RELATO.md:1929; app/docs/BACKLOG.md:2074.
                    # Não chamar registrar_batida: grava em
                    # app/ponto/registro_batida.py:152 e :158.
                    motivo = (
                        "dia_instante_e_batidas_do_fato=[nao_verificado];"
                        "registro_do_juiz_da_dedup=[nao_verificado];"
                        "escritor_nao_chamado;"
                        "fonte=app/docs/RELATO.md:1929"
                    )
                else:
                    from ponto.turnos import ausencia_cobre
                    from ponto.services.ausencia import origem_de_ferias

                    # Data civil do fato é literal; instante da consulta é local.
                    agora_local = timezone.localtime()
                    hoje_local = timezone.localdate(agora_local)

                    # Juiz registrado: app/docs/JUIZES.md:52.
                    # Leitura: app/ponto/turnos.py:2550 e :2566.
                    ausencia = ausencia_cobre(pessoa, dia, para="folha")

                    if ausencia is None:
                        cobertura = None
                        origem = None
                    else:
                        cobertura = {
                            "ausencia_pk": ausencia.pk,
                            "tipo": ausencia.tipo,
                            "status": ausencia.status,
                        }
                        # Mesmo montador do painel:
                        # app/ponto/views.py:2093;
                        # app/ponto/services/ausencia.py:101.
                        painel = origem_de_ferias(ausencia)
                        origem = None if painel is None else {
                            "agendamento_pk": painel["agendamento"],
                            "dia": painel["dia"].isoformat(),
                        }

                    resposta = {
                        "cobertura_folha": cobertura,
                        "origem_no_painel": origem,
                        "consulta_em": agora_local.isoformat(),
                        "hoje_local": hoje_local.isoformat(),
                    }

                    # Não promover cobertura a fechamento do conjunto:
                    # origem/vetos/passivo: app/docs/RELATO.md:483 e :489.
                    motivo = (
                        "cobertura_consultada;"
                        "juiz_registrado_suficiente_para_origem_vetos"
                        "_e_passivo=[nao_verificado];"
                        "conferencia_DP_e_homologacao_pendentes;"
                        "fontes=app/docs/RELATO.md:483,"
                        "app/docs/RELATO.md:8919"
                    )

    except Exception as erro:
        total_erros += 1
        causa = getattr(erro, "__cause__", None)
        sqlstate = (
            getattr(causa, "sqlstate", None)
            or getattr(causa, "pgcode", None)
        )
        # Imprime o erro por classe/SQLSTATE; a mensagem arbitrária pode
        # conter dados pessoais ou parâmetros SQL.
        resposta = {
            "erro": type(erro).__name__,
            "sqlstate": sqlstate,
        }
        estado = "NAO_CONFERIDO"
        motivo = "erro_impresso_por_classe_e_sqlstate;nao_convertido_em_zero"

    if estado == "NAO_CONFERIDO":
        total_nao_conferidos += 1

    valor = json.dumps(
        resposta, ensure_ascii=True, separators=(",", ":")
    )
    print(
        f"CASO {caso_id} col{pessoa_pk} {dia_saida} "
        f"juiz={juiz} resposta={valor} -> {estado} motivo={motivo}"
    )

print(f"total_erros={total_erros}")
print(f"total_nao_conferidos={total_nao_conferidos}")
```

## 16/11 00:50 · sessao 86940fe41689 · - · - · origem=Ronald (tmux)

**E3 e R3:** não achei ligação documental suficiente para afirmar fechamento de um caso do p0 por E3 `90f3fa66b` ou R3 c17. **[nao verificado]**

- `app/docs/RELATO.md:477` mede `col996 30/10` e `col399 30/09`, mas pertence à **E3-FIO-PELA-ESCADA/RESTO-R2 expressamente não pousada**. É mudança de fio, sem certificação de fechamento.
- `app/docs/BACKLOG.md:2010` liga o caso BO-1311/04 (`col246 01/11`) à E4B e escreve **“NAO se cura na E3”**.
- `app/docs/RELATO.md:875` e `:928` mencionam perguntas de `col789` na E3-10, mas não identificam dias/IDs que permitam casar com BO-ADMIN-2710/6. Fechamento desse caso por essa passagem: **[nao verificado]**.
- R3 separada: escopo LU-01..07 e achados em `app/docs/BACKLOG.md:770`; pendências de origem da medição, relógio e universo em `app/docs/RELATO.md:51`; contrato c17 de nove itens em `:61`. Casos p0 que fecharia nominalmente: **[nao verificado]**. Não contar no total.

**HJ-091 tem ligação explícita:**

| Evidência | Referência |
|---|---|
| HJ-091 → HJ-091-LISTA NO AR | `app/docs/RELATO.md:343` |
| Merge `8376da7aa`; origem preservada nas rejeitadas; 60 casos na lista DP | `app/docs/BACKLOG.md:750`; `app/docs/RELATO.md:483` |
| Subcasos documentados: `col127 02/10`, agendamento 114; `col344 07/11`, agendamento 103 | `app/docs/RELATO.md:749` |
| `col344` nos três dias 05–07/11; rejeitadas 3977/3995/3985; abono 5592 preservado; erro observado era nascerem férias aprovadas novas em 06–07/11 | `app/docs/RELATO.md:489`; preservação após pouso em `:483` |
| Tela do admin lê `origem_de_ferias(a)` | `app/ponto/views.py:2091–2093` |
| Serviço retorna agendamento, período, dias, status, dia consultado | `app/ponto/services/ausencia.py:101–112` |
| Template mostra origem/período/dia; não oferece edição genérica para derivada | `app/templates/ponto/partials/painel_ausencia.html:131–145` |

**Juízes reais e leitura segura HJ-091:**

- `ponto.turnos.cobertura_ausencia_periodo(colab, ini, fim, para='folha')`, registrado em `app/core/juizes.py:257`, corpo somente consulta em `app/ponto/turnos.py:2747–2766`; retorna `{date: Ausencia}`.
- `ponto.turnos.ausencia_cobre(colab, dia, para='folha')`, registrado em `app/docs/JUIZES.md:52–54`, corpo somente consulta em `app/ponto/turnos.py:2550–2571`; retorna uma ausência aprovada ou `None`, por `_pelo_efeito` em `:2523–2532`.
- `ferias.services.em_gozo_hoje(hoje=dia, colaborador=colab)` está registrado em `app/core/juizes.py:266`; somente consulta em `app/ferias/services.py:59–70`. Responde se o agendamento cobre a data, **não** se as ausências derivadas têm origem correta.
- O contador específico **`ferias.services.ferias_espelho_x_agendamento()`**, explicitamente **SO LEITURA e esperado zero nos dois sentidos**, está em `app/ferias/services.py:135–165`. Retorna PKs de ausências sem origem válida e pares `(agendamento_pk, data)` sem ausência aprovada própria. **Não encontrei seu registro em JUIZES/core: [nao verificado].**
- Para o caso inteiro HJ-091, a resposta de um juiz de cobertura isoladamente não certifica a origem FK nem preservação das rejeitadas. O script deve consultar a cobertura e os objetos específicos sem chamar espelhamento/backfill; se exigir um único juiz registrado que certifique tudo, resultado **NAO_CONFERIDO**, motivo “juiz suficiente para origem e preservação dos vetos [nao verificado]”. As funções que espelham/gravam ficam fora.

**Proveniência escrita:**

- HJ: **“56 HJ do Fernando”** em `app/docs/BACKLOG.md:1883`; **“56 TXT do Fernando”** em `app/docs/PROMPTS.md:1888`.
- BO-0611: **“BO-[nome]-0611 (14 itens)”** em `app/docs/PROMPTS.md:1914`; denominação também em `app/docs/BACKLOG.md:1926–1927`.
- BO-1311: **BO-[nome]-1311-ROTEIRO**, dez casos e ordem das pessoas (`col1019`, `col996`, `col1032`, `col246`, `col265`, `col982`, `col1004`, `col523`, `col866`, `col859`) em `app/docs/BACKLOG.md:795`.
- BO-ADMIN-2710: associação a **BO-[nome]** está nas linhas de suas obras: `app/docs/BACKLOG.md:833`, `:835`, `:841`; a nomeação formal A/B/C é explicada em `app/docs/CORTES.md:366`.
- BO2910-ESTRUTURA: `app/docs/BACKLOG.md:1116` a chama “triagem BO-ADMIN-2910”; autoria Mauricio/Fernando nessa passagem: **[nao verificado]**.

## 16/11 00:49 · sessao 80b7cbd5c09c · - · - · origem=Ronald (tmux)

**BATIDA-DEDUP-RETRATADA → BO-0611/05, pela ponte de sítio explicitamente permitida pelo usuário.** A ficha complementar desse caso nomeia `triagem_batida.py:662` e a janela de duplicidade; a obra nomeia exatamente esse sítio. Provas: `app/docs/BACKLOG.md:2074` + `app/docs/BACKLOG.md:2083`. Essa ligação é ao complemento da classe; não prova o encerramento da queixa original de GPS. A queixa original aparece como barreira GPS em `app/docs/RELATO.md:1929`.

- Identidade: **col456**, `app/docs/RELATO.md:1929` e `app/docs/BACKLOG.md:1927`. Dia do fato: **[nao verificado]**.
- A função que a obra efetivamente consulta é `ponto.turnos.batidas_apuraveis(colab, ini=None, fim=None)`, somente queryset: `app/ponto/turnos.py:2868`, filtros em `:2887–2895`. A casca atual a chama em `app/ponto/services/triagem_batida.py:664–668` e `app/api/views_core.py:514–517`.
- Não encontrei essa função literalmente em `JUIZES.md` ou `core/juizes.py`: registro exigido pelo pedido **[nao verificado]**. Não apresentar `tem_gps` como juiz da dedup.
- O efetivo escritor `ponto.registro_batida.registrar_batida` está em `app/ponto/registro_batida.py:94` e executa `objects.create` em `:152` e `:158`, mais efeitos de fechamento em `:156`/`:162`: **não chamar na sonda**.
- Para comprovar FECHADO nesse caso faltam o instante do toque e batidas concretas antes/depois **[nao verificado]**. Sonda deve imprimir **NAO_CONFERIDO**, com motivo dia/instante não individualizado e impossibilidade de exercitar escritor.
- Leitor admin comprovado para a queixa GPS do mesmo caso: view `app/ponto/views.py:437`, queryset de pendentes em `:444–449`, contador pelo `tem_gps` em `:488`; template `app/templates/ponto/batidas_offline_pendentes.html:95–100`. Feed também chama o filtro em `app/templates/core/_d2_feed.html:16–18`. O espelho aceita gestor `?colab=<pk>` em `app/ponto/views.py:117–123`, exibindo timestamp cru em `app/templates/ponto/espelho.html:246`.

**CONTRATO-DE-ENTRADA-DO-FIO: nenhum caso do p0 ligado com segurança.** A ligação nominal é **HJ-095**, ausente do universo p0 fornecido: `app/docs/BACKLOG.md:2224` e `app/docs/GRADE-MESTRA.md:688`. A grade traz “1 (roteiro … / mapa HJ)” mas não individualiza ficha, pessoa ou dia: não usar esse 1 como caso identificado (`app/docs/GRADE-MESTRA.md:688`). O smoke col677 é da obra, mas não identifica caso p0 (`app/docs/RELATO.md:233`).

**TELA-DIZ-O-QUE-O-SERVIDOR-DISSE: nenhum caso p0 ligado com segurança.** A obra nomeia `bater_ponto_v2.html:327` e laudo novo de 15/11 (`app/docs/BACKLOG.md:1939`); o complementar BO05 cita a fila `pwa.js:158` (`app/docs/BACKLOG.md:2074`), porém a própria fila registra que **pwa.js está fora do aval C** (`app/docs/BACKLOG.md:2029`). Compartilhar o arquivo `bater_ponto_v2.html` com a barreira GPS de BO05 (`app/docs/BACKLOG.md:2011`) não comprova que esta obra fecha aquela queixa. A tela atual lê `data.status`, distinguindo ok/ja_registrada/ignorada_intervalo em `app/templates/ponto/bater_ponto_v2.html:168–174`, acionada no 2xx em `:365`; isso documenta comportamento, não ligação ao caso p0.

**Fontes/donos escritos:** BO-0611 é explicitamente **BO-[nome]-0611** (`app/docs/BACKLOG.md:1926–1927`); BO1311 é **BO-[nome]-1311-ROTEIRO**, com ordem col1019, col996, col1032, col246, col265, col982, col1004, col523, col866, col859 (`app/docs/BACKLOG.md:795`). Isso confirma os rótulos sem precisar escrever nome ou CPF de colaborador.

## 16/11 00:49 · sessao 552b54e251f3 · - · - · origem=Ronald (tmux)

HE-REVOGADA-NA-PORTA: **nenhum caso do universo documentalmente ligado [nao verificado]**. As linhas da obra tratam das quatro portas e da competência 11 aberta, sem nomear caso BO/HJ, pessoa ou dia do universo: app/docs/RELATO.md:225; app/docs/BACKLOG.md:1951. O pouso consta em app/docs/RELATO.md:17.

BUG-ENC-CELULAS-FORMA: **nenhum caso do universo documentalmente ligado [nao verificado]**. A linha da obra nomeia `dias_encerrados`, `turnos_do_colab`, cartório e geradora, sem caso do universo: app/docs/BACKLOG.md:988. Os sítios atingidos constam em app/docs/RELATO.md:8292. O impacto nomeia col174, 25/10, em app/docs/RELATO.md:33; esse nominal pertence a `CELULA-CONCORDE-NO-MEIO-DO-TURNO`, obra fora do universo dado, em app/docs/BACKLOG.md:1759. Não transportar por semelhança. O pouso consta em app/docs/RELATO.md:14.

Para eventual referência técnica, sem adicionar casos:

- `ponto.models.DecisaoHEQuerySet.vigentes`: leitura pura, retorna `filter(revogada_em__isnull=True)` — app/ponto/models.py:1265–1267. Não achei registro em `core/juizes.py` ou `JUIZES.md` **[nao verificado]**; declaração escrita existente em app/docs/CORTES.md:461.
- `ponto.portas.he.decidir_he` é escritor, proibido para sonda — app/ponto/portas/he.py:158–160.
- `ponto.turnos.dia_encerrado` é juiz registrado em app/core/juizes.py:239; assinatura/corpo em app/ponto/turnos.py:2050 e :2083. `dias_encerrados` é carregador, expressamente “não juiz” — app/ponto/turnos.py:2142.

Fontes e destinatários:

- **BO-0611 → Mauricio**, frase “quadro único soma=14 + texto ao Mauricio por item” — app/docs/PROMPTS.md:1914.
- **BO-1311-ROTEIRO → BO-[nome]**, identificador e rol dos dez colabs — app/docs/BACKLOG.md:795; aval — app/docs/PROMPTS.md:2247. Frase separada dizendo autor do relato **[nao verificado]**.
- **HJ → Fernando**, frase “56 HJ do Fernando → unidades pessoa+dia” — app/docs/BACKLOG.md:1883.
- **BO-ADMIN-2710**: associação escrita a BO-[nome] para 8.1, 7, 1/3 e C — app/docs/BACKLOG.md:833, :835, :841, :849. Fonte inteira explicitamente atribuída ao Mauricio **[nao verificado]**.
- **BO2910-ESTRUTURA**: remete à triagem BO-ADMIN-2910 — app/docs/BACKLOG.md:1116; aval diz somente “8 BOs do admin” — app/docs/PROMPTS.md:1620. Atribuição ao Mauricio **[nao verificado]**.
- Cuidado com inferência pelo rótulo: `LAUDO-[nome]-2610` está escrito como “laudo do Fernando” — app/docs/PROMPTS.md:1490; `BO-[nome]-0511-CENSO` também usa relatório do Fernando — app/docs/PROMPTS.md:1880.

Linhas atualizadas do universo úteis para consolidação:

- BO2910 02/05/06/08 — app/docs/RELATO.md:3320, :3322, :3323, :3325; inventário das cinco classes — app/docs/BACKLOG.md:1116.
- BO-ADMIN-2710 1–8.3 — app/docs/RELATO.md:3895–3903.
- BO-0611 V2 — app/docs/RELATO.md:1929; rol antigo substituído — app/docs/RELATO.md:1991.
- BO-1311 ficha→col, ordem contestável — app/docs/BACKLOG.md:795; app/docs/RELATO.md:684. Dados recuperáveis 06–10 e dias col859 07/11 e 09/11 — app/docs/RELATO.md:680.

## 16/11 00:08 · sessao 50ce300f7dae · ESPELHO-VERDADE-FECHO-V0 · sonda · origem=Code

Transcrição integral do dado fornecido, sem verificação independente. [nao verificado]

```text
sonda_leitura: script.py [4e9a5641d912b65d] no cpuset 4-7, lendo sombra como leitor (LEITURA)
sonda_leitura: arvore=/home/ronald/v0-arvore-3580b35e0 (copia), banco segue sombra como leitor

PARTE 1 PARTES 6
[0] UNIVERSO 10/2026 624
UNIVERSO_PARTE 10/2026 110
JANELA 10/2026 2026-09-21 2026-10-20
[0] UNIVERSO 11/2026 595
UNIVERSO_PARTE 11/2026 99
JANELA 11/2026 2026-10-21 2026-11-20
ESTIMATIVA processados 25 universo 209 minutos_total 2.6 ate_40_min True ate_15_min True
ESTIMATIVA processados 50 universo 209 minutos_total 2.9 ate_40_min True ate_15_min True
ESTIMATIVA processados 75 universo 209 minutos_total 2.8 ate_40_min True ate_15_min True
ESTIMATIVA processados 100 universo 209 minutos_total 2.7 ate_40_min True ate_15_min True
COMPETENCIA 10/2026
[1] {'dias_comparados': 1180, 'bate_ate_10min': 1140, 'esp_zero_e6_trabalho': 1, 'e6_zero_esp_trabalho': 3, 'diverge_10_60': 11, 'diverge_acima_60': 25, 'dia_batida_impar': 33, 'impar_que_BATE': 15, 'impar_que_DIVERGE': 18, 'impar_dentro_da_faixa': 15, 'impar_FORA_da_faixa': 3, 'impar_no_conjunto_do_dia': 18, 'impar_SO_POR_CORTE_do_oraculo': 0, 'dia_sem_linha_no_espelho': 0, 'dia_sem_lavratura_no_espelho': 1727, 'dia_isento_ou_ausencia': 316, 'dia_sem_trabalho_ambos': 34, 'esp_desconta_janela_declarada': 43, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 96.6% de 1180 dias comparados
[3] ESTRUTURA    10 dia-colab       9.1 h    3 colabs
[3] CADASTRO     10 dia-colab      38.4 h    3 colabs
[3] BATIDA       20 dia-colab     110.2 h   13 colabs
[4] TOTAL divergentes 40
[5] colabs com divergencia: 17
ESTRUTURA_DIA|10/2026|col252|2026-09-23|esp_min=548|e6_min=594|teto_min=608|dif_min=46|classe=diverge_10_60|paridade=par|batidas=06:56 12:35 12:49 17:04|veredito=trabalhou
ESTRUTURA_DIA|10/2026|col252|2026-09-24|esp_min=552|e6_min=598|teto_min=611|dif_min=46|classe=diverge_10_60|paridade=par|batidas=06:51 12:37 12:51 17:03|veredito=trabalhou
ESTRUTURA_DIA|10/2026|col252|2026-09-25|esp_min=556|e6_min=604|teto_min=616|dif_min=48|classe=diverge_10_60|paridade=par|batidas=06:51 12:37 12:50 17:07|veredito=trabalhou
ESTRUTURA_DIA|10/2026|col252|2026-09-28|esp_min=535|e6_min=581|teto_min=595|dif_min=46|classe=diverge_10_60|paridade=par|batidas=07:12 12:38 12:53 17:07|veredito=trabalhou
ESTRUTURA_DIA|10/2026|col252|2026-10-01|esp_min=545|e6_min=590|teto_min=605|dif_min=45|classe=diverge_10_60|paridade=par|batidas=07:00 12:37 12:51 17:05|veredito=trabalhou
ESTRUTURA_DIA|10/2026|col252|2026-10-02|esp_min=545|e6_min=593|teto_min=606|dif_min=48|classe=diverge_10_60|paridade=par|batidas=06:59 12:40 12:53 17:04|veredito=trabalhou
ESTRUTURA_DIA|10/2026|col252|2026-10-06|esp_min=551|e6_min=597|teto_min=611|dif_min=46|classe=diverge_10_60|paridade=par|batidas=06:58 12:38 12:52 17:09|veredito=trabalhou
ESTRUTURA_DIA|10/2026|col252|2026-10-09|esp_min=544|e6_min=592|teto_min=604|dif_min=48|classe=diverge_10_60|paridade=par|batidas=07:06 12:48 13:00 17:10|veredito=trabalhou
ESTRUTURA_DIA|10/2026|col864|2026-10-12|esp_min=509|e6_min=449|teto_min=509|dif_min=-60|classe=diverge_10_60|paridade=par|batidas=15:01 18:10 19:10 23:30|veredito=feriado
ESTRUTURA_DIA|10/2026|col876|2026-09-21|esp_min=550|e6_min=665|teto_min=722|dif_min=115|classe=diverge_acima_60|paridade=par|batidas=18:29 04:39 05:36 06:31|veredito=em_aberto
ESTRUTURA_DIAS_LINHAS 10/2026 10 True
erros_no_espelho 0
PARCIAL|10/2026|ESTRUTURA|dias=10|horas=9.116283817777775|colabs=3
PARCIAL|10/2026|CADASTRO|dias=10|horas=38.355213611111104|colabs=3
PARCIAL|10/2026|BATIDA|dias=20|horas=110.21011656222223|colabs=13
PARCIAL_TOTAL|10/2026|dias=40|horas=157.681613991111109|colabs=17
ESTIMATIVA processados 125 universo 209 minutos_total 2.9 ate_40_min True ate_15_min True
ESTIMATIVA processados 150 universo 209 minutos_total 3.1 ate_40_min True ate_15_min True
ESTIMATIVA processados 175 universo 209 minutos_total 3.2 ate_40_min True ate_15_min True
ESTIMATIVA processados 200 universo 209 minutos_total 3.2 ate_40_min True ate_15_min True
COMPETENCIA 11/2026
[1] {'dias_comparados': 979, 'bate_ate_10min': 922, 'esp_zero_e6_trabalho': 13, 'e6_zero_esp_trabalho': 6, 'diverge_10_60': 7, 'diverge_acima_60': 31, 'dia_batida_impar': 47, 'impar_que_BATE': 17, 'impar_que_DIVERGE': 30, 'impar_dentro_da_faixa': 20, 'impar_FORA_da_faixa': 10, 'impar_no_conjunto_do_dia': 30, 'impar_SO_POR_CORTE_do_oraculo': 0, 'dia_sem_linha_no_espelho': 396, 'dia_sem_lavratura_no_espelho': 1391, 'dia_isento_ou_ausencia': 232, 'dia_sem_trabalho_ambos': 38, 'esp_desconta_janela_declarada': 33, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 94.2% de 979 dias comparados
[3] ESTRUTURA     9 dia-colab      34.9 h    4 colabs
[3] CADASTRO     20 dia-colab     115.4 h    6 colabs
[3] BATIDA       28 dia-colab     157.3 h   15 colabs
[4] TOTAL divergentes 57
[5] colabs com divergencia: 20
ESTRUTURA_DIA|11/2026|col114|2026-11-08|esp_min=439|e6_min=429|teto_min=499|dif_min=-10|classe=diverge_10_60|paridade=par|batidas=09:59 14:50 16:00 18:18|veredito=trabalhou
ESTRUTURA_DIA|11/2026|col252|2026-11-09|esp_min=551|e6_min=597|teto_min=610|dif_min=46|classe=diverge_10_60|paridade=par|batidas=06:53 12:38 12:52 17:04|veredito=trabalhou
ESTRUTURA_DIA|11/2026|col252|2026-11-10|esp_min=551|e6_min=598|teto_min=611|dif_min=47|classe=diverge_10_60|paridade=par|batidas=06:56 12:39 12:52 17:07|veredito=trabalhou
ESTRUTURA_DIA|11/2026|col252|2026-11-11|esp_min=550|e6_min=598|teto_min=611|dif_min=48|classe=diverge_10_60|paridade=par|batidas=06:54 12:39 12:52 17:04|veredito=trabalhou
ESTRUTURA_DIA|11/2026|col252|2026-11-13|esp_min=551|e6_min=597|teto_min=611|dif_min=46|classe=diverge_10_60|paridade=par|batidas=06:52 12:35 12:49 17:03|veredito=trabalhou
ESTRUTURA_DIA|11/2026|col516|2026-10-23|esp_min=361|e6_min=0|teto_min=0|dif_min=-361|classe=e6_zero_esp_trabalho|paridade=par|batidas=|veredito=folga
ESTRUTURA_DIA|11/2026|col516|2026-10-28|esp_min=372|e6_min=0|teto_min=0|dif_min=-372|classe=e6_zero_esp_trabalho|paridade=par|batidas=|veredito=trabalhou
ESTRUTURA_DIA|11/2026|col996|2026-10-28|esp_min=0|e6_min=580|teto_min=580|dif_min=580|classe=esp_zero_e6_trabalho|paridade=par|batidas=21:18 06:57|veredito=folga
ESTRUTURA_DIA|11/2026|col996|2026-11-10|esp_min=443|e6_min=1026|teto_min=1116|dif_min=583|classe=diverge_acima_60|paridade=par|batidas=06:57 13:00 14:30 15:50 21:16 06:59|veredito=em_aberto
ESTRUTURA_DIAS_LINHAS 11/2026 9 True
erros_no_espelho 0
PARCIAL|11/2026|ESTRUTURA|dias=9|horas=34.87566822027778|colabs=4
PARCIAL|11/2026|CADASTRO|dias=20|horas=115.41531537222224|colabs=6
PARCIAL|11/2026|BATIDA|dias=28|horas=157.27115423694445|colabs=15
PARCIAL_TOTAL|11/2026|dias=57|horas=307.56213782944447|colabs=20
TEMPO_SEGUNDOS 192.2
TRATADA_LOCAL app/escala/alimentacao.py:279 GeneratorExit 252870
TRATADA_LOCAL app/escala/models.py:1282 GeneratorExit 98
TRATADA_LOCAL app/escala/services/fase_12x36.py:114 GeneratorExit 1130
TRATADA_LOCAL app/escala/services/leitor_celula.py:25 ValueError 6152
TRATADA_LOCAL app/escala/services/leitor_celula.py:81 GeneratorExit 52
TRATADA_LOCAL app/escala/utils.py:1500 GeneratorExit 2
TRATADA_LOCAL app/escala/utils.py:1687 GeneratorExit 2
TRATADA_LOCAL app/escala/utils.py:1991 GeneratorExit 6181
TRATADA_LOCAL app/escala/utils.py:2033 GeneratorExit 3
TRATADA_LOCAL app/escala/utils.py:452 GeneratorExit 4
TRATADA_LOCAL app/escala/utils.py:524 GeneratorExit 3658
TRATADA_LOCAL app/escala/utils.py:525 GeneratorExit 3658
TRATADA_LOCAL app/escala/utils.py:888 GeneratorExit 591
TRATADA_LOCAL app/folha/causas.py:58 GeneratorExit 181
TRATADA_LOCAL app/ponto/juiz_batida.py:75 ValueError 466420
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:1131 GeneratorExit 596
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:2604 GeneratorExit 2826
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:677 GeneratorExit 172
TRATADA_LOCAL app/ponto/services/dia_pago.py:892 GeneratorExit 12084
TRATADA_LOCAL app/ponto/services/espelho.py:473 GeneratorExit 121
TRATADA_LOCAL app/ponto/services/espelho.py:657 GeneratorExit 3994
TRATADA_LOCAL app/ponto/services/fechamento.py:1044 GeneratorExit 192
TRATADA_LOCAL app/ponto/services/fechamento.py:1045 _GradeSemCelula 5
TRATADA_LOCAL app/ponto/turnos.py:1295 GeneratorExit 23094
TRATADA_LOCAL app/ponto/turnos.py:1656 GeneratorExit 4574
TRATADA_LOCAL app/ponto/turnos.py:1699 GeneratorExit 37388
TRATADA_LOCAL app/ponto/turnos.py:2653 GeneratorExit 16
TRATADA_LOCAL app/ponto/turnos.py:679 GeneratorExit 16723
SOMA 10/2026 40   <- tem de ser igual a soma das classes (40) True
SOMA 10/2026 HORAS 157.681613991111109 True
SOMA 10/2026 COLABS_UNIAO 17 True
SOMA 11/2026 57   <- tem de ser igual a soma das classes (57) True
SOMA 11/2026 HORAS 307.56213782944447 True
SOMA 11/2026 COLABS_UNIAO 20 True
rc_sombra=0
```

- `SOMA`: as seis linhas apresentam `True` no dado fornecido. [nao verificado]
- `ESTRUTURA_DIAS_LINHAS`: as duas linhas apresentam `True` — `10/2026 10 True` e `11/2026 9 True`. [nao verificado]
- `INCOMPLETO`: não aparece no texto fornecido desta parte 1 de 6. [nao verificado]

## 15/11 23:52 · sessao 323261a980c3 · ESPELHO-VERDADE-FECHO-V0 · leitura · origem=Code

Estados existentes: `FEITO`, `EM_CURSO`, `FALTA` e `ESPERA='ESPERA_RONALD'` no espelho; `FECHADO`, `PARCIAL` e `PENDENTE` no estrutural. Contei os estados declarados: `EM_CURSO`/`ESPERA_RONALD` e todo resultado diferente de `FECHADO`. (app/core/espelho_verdade.py:35; app/core/placar_estrutural.py:45)

Na coluna de obra, **“sem obra” significa sem vínculo comprovado de fechamento da pendência atual**. Registrei também obras relacionadas, sem afirmar que encerram os casos atuais quando isso está [nao verificado]. O54 é o programa geral; R1-REMEDE fechou a medição, distinguindo cinco itens medidos e quatro fora de alcance. (app/docs/BACKLOG.md:1395; app/docs/BACKLOG.md:1854)

| fonte arquivo:linha | id do item | estado literal | obra que fecha ou “sem obra” | estado da obra | o que destrava | dono — origem |
|---|---|---|---|---|---|---|
| app/core/espelho_verdade.py:49 | E1-passivo-53 | `EM_CURSO` | **O50** — app/docs/GRADE-MESTRA.md:862 | `PARADA (passivo espera !)` — app/docs/GRADE-MESTRA.md:862 | O passivo depende do `!` caso a caso: a prova distingue 43 congelados e 10 livres ao propositor. (app/core/espelho_verdade.py:57–58) | **Ronald**, para o `!`; executor: **nao nomeado**. (app/docs/BACKLOG.md:1388) |
| app/core/espelho_verdade.py:61 | E1-sem-previsao | `EM_CURSO` | **sem obra de fecho específico comprovada**; O54 inclui expressamente censo e tratamento das células sem previsão, mas vínculo de fechamento dos 35/28 atuais [nao verificado]. (app/docs/BACKLOG.md:1395; app/core/espelho_verdade.py:61) | **nao nomeado** para fecho específico; O54: `E0 em curso`. (app/docs/BACKLOG.md:1395) | Zerar dia de colaborador ativo sem previsão ou com previsão de vínculo inválido é a porta declarada. (app/core/espelho_verdade.py:40–41) | **nao nomeado** na medição atual. (app/core/espelho_verdade.py:61) |
| app/core/espelho_verdade.py:94 | E3-turno-aberto | `EM_CURSO` | **sem obra de fecho comprovada**; O86 investiga turnos abertos, mas sua correspondência com os atuais 347/262 dias [nao verificado]. (app/docs/BACKLOG.md:1427; app/core/espelho_verdade.py:94) | **nao nomeado** para fecho atual; O86: `REGISTRADA (guarda com DIFF zero; investigação residual)`. (app/docs/GRADE-MESTRA.md:881) | **nao nomeado**: a prova atual mede dias ímpares e casos fora da faixa, sem declarar qual ato encerra a pendência. (app/core/espelho_verdade.py:94) | **nao nomeado** para os casos atuais. (app/core/espelho_verdade.py:94) |
| app/core/espelho_verdade.py:96 | E3-separados-30 | `ESPERA_RONALD` — constante `ESPERA` em app/core/espelho_verdade.py:35 | **CORTE-B-30** — app/docs/GRADE-MESTRA.md:845 | `PARADA (congelada)` — app/docs/GRADE-MESTRA.md:845 | Descongelamento e corte do lote B são as dependências registradas. (app/docs/GRADE-MESTRA.md:845) | **outro — Gabriel**, segundo a prova; Ronald aparece no estado literal de espera. (app/core/espelho_verdade.py:96) |
| app/core/espelho_verdade.py:143 | E4-rotulo-do-desenho | `EM_CURSO` | **sem obra** de execução nomeada; ESPELHO-VERDADE-REMEDIDO registra a tarefa, mas sua entrega é atualizar provas. (app/docs/BACKLOG.md:1579; app/docs/GRADE-MESTRA.md:89) | **nao nomeado** para execução. (app/docs/BACKLOG.md:1579) | Falta a pílula de `templates/escala/tipos_lista.html:42` ler `desenho_do_turno`, na fila 2. (app/core/espelho_verdade.py:143) | **nao nomeado**. (app/core/espelho_verdade.py:143) |
| app/core/espelho_verdade.py:151 | E5-le-da-celula | `EM_CURSO` | **O48** — app/docs/GRADE-MESTRA.md:860; complemento **STATUS-ENCERRADO-DA-COMPETENCIA-NAO-EXISTE** — app/docs/BACKLOG.md:2578 | O48: `REGISTRADA (passos no ar; status e exportação pendentes)` — app/docs/GRADE-MESTRA.md:860 | Falta o status `encerrado`; escrever esse estado nas linhas da 10 exige DIFF e `!`, enquanto migration e censo não pedem aval. (app/core/espelho_verdade.py:151; app/docs/BACKLOG.md:2578) | **Ronald**, para autorizar a escrita; execução: **nao nomeado**. (app/docs/BACKLOG.md:2578) |
| app/core/espelho_verdade.py:181 | E6-sistema-bate | `EM_CURSO` | **sem obra de fecho atual comprovada**; R1-ESTRUTURA-91 existe, mas causa/destino dos atuais E26/E9 não foram medidos. (app/docs/GRADE-MESTRA.md:102; app/core/placar_estrutural.py:130) | **nao nomeado** para fecho atual; R1-ESTRUTURA-91: `REGISTRADA (restante pendente)`. (app/docs/GRADE-MESTRA.md:102) | Com batidas completas, espelho e oráculo precisam bater; a divergência é declarada “bug nosso”. (app/core/espelho_verdade.py:180) | **ESTRUTURA**, explicitamente medida na prova. (app/core/espelho_verdade.py:181) |
| app/core/espelho_verdade.py:185 | E6-exportavel | `EM_CURSO` | **sem obra de fecho atual comprovada**; O54 contém a certificação geral e O125 trata destino administrativo, mas fechamento dos casos atuais [nao verificado]. (app/docs/BACKLOG.md:1395; app/docs/GRADE-MESTRA.md:894) | **nao nomeado** para fecho atual; O125: `REGISTRADA`. (app/docs/GRADE-MESTRA.md:894) | Certificação pelo oráculo é a condição do item; a entrada efetiva dos divergentes na lista do admin não foi medida. (app/core/espelho_verdade.py:184–185) | **ESTRUTURA / CADASTRO / BATIDA**: fila 1 para ESTRUTURA e lista do admin para os demais. (app/core/espelho_verdade.py:185) |
| app/core/espelho_verdade.py:191 | E6-piso-185 | `EM_CURSO` | **sem obra de fecho atual comprovada**; O83 registra classes anteriores, mas causa/destino dos atuais E26/E9 não estão nomeados. (app/docs/BACKLOG.md:1438; app/core/espelho_verdade.py:191; app/core/placar_estrutural.py:130) | **nao nomeado** para fecho atual; O83: `REGISTRADA (curso atual deste id não demonstrado)`. (app/docs/GRADE-MESTRA.md:879) | Falta nomear a causa por colaborador, incluindo os 26+9 dias de ESTRUTURA cuja causa a prova não nomeia. (app/core/espelho_verdade.py:190–191) | **ESTRUTURA / CADASTRO / BATIDA**, na decomposição dos divergentes; causa dos casos ESTRUTURA: **nao nomeado**. (app/core/placar_estrutural.py:118; app/core/espelho_verdade.py:191) |
| app/core/placar_estrutural.py:74 | BATERIA | `PARCIAL` | **sem obra** comprovada para a meta integral; BATERIA-QUEIXAS-ADMIN cobre quatro queixas, sem declarar fechamento de todas as famílias. (app/docs/BACKLOG.md:1867; app/core/placar_estrutural.py:79–100) | **nao nomeado** para meta integral; BATERIA-QUEIXAS-ADMIN: `REGISTRADA`. (app/docs/GRADE-MESTRA.md:103) | Todas as famílias precisam ter bateria declarada e verde, sem allowlist, e nenhum dia ESTRUTURA novo pode ficar sem destino. (app/core/placar_estrutural.py:79–80) | **nao nomeado** para execução integral. (app/core/placar_estrutural.py:74–111) |
| app/core/placar_estrutural.py:146 | R3 | `PARCIAL` | **PLACAR-ESTRUTURAL** — app/docs/GRADE-MESTRA.md:62; O130 já consta no ar. (app/docs/BACKLOG.md:1280) | `PARADA (resultados parciais; espera autorização)` — app/docs/GRADE-MESTRA.md:62 | A grade precisa alcançar os mesmos 48/48 dias de cartão/tela/API; o corte manda manter R3 aberto por divergência de leitor. (app/docs/BACKLOG.md:338–342) | **ESTRUTURA**, nomeado no corte. (app/docs/BACKLOG.md:341) |
| app/core/placar_estrutural.py:211 | R4 | `PARCIAL` | **PLACAR-ESTRUTURAL** — app/docs/GRADE-MESTRA.md:62; **O269** cobre a relavra da 09, sem comprovação de fechamento integral do R4. (app/docs/BACKLOG.md:1611) | PLACAR-ESTRUTURAL: `PARADA (resultados parciais; espera autorização)`; O269: `PORTAO DE DATA: apply so apos 14/10 18h`. (app/docs/GRADE-MESTRA.md:62; app/docs/BACKLOG.md:1611) | A meta é ZERO em cada par; há registros distintos de Pauta DP sem tocar a 09 e de relavra posterior condicionada na O269. (app/core/placar_estrutural.py:215; app/docs/BACKLOG.md:334–337; app/docs/BACKLOG.md:1611) | **ESTRUTURA** para os seis casos novos; **Ronald** para os cortes sobre a 09. (app/core/placar_estrutural.py:291–292; app/docs/BACKLOG.md:334–337,1611) |
| app/core/placar_estrutural.py:433 | R6 | `PARCIAL` | **PLACAR-ESTRUTURAL**, incluindo lote do R6 — app/docs/GRADE-MESTRA.md:62; **O35**, para contratos — app/docs/GRADE-MESTRA.md:896 | PLACAR-ESTRUTURAL: `PARADA (resultados parciais; espera autorização)`; O35: `REGISTRADA (partes construídas; contratos incompletos)`. (app/docs/GRADE-MESTRA.md:62,896) | Chegar a 20/20 contratos e zero passo manual; o registro declara que resta o lote. (app/core/placar_estrutural.py:434–440) | **nao nomeado** como responsável global. (app/docs/GRADE-MESTRA.md:62,896) |

**Contagem do recorte:** 8 `EM_CURSO`, 1 `ESPERA_RONALD` e 4 `PARCIAL`: **13 linhas**. Pela definição conservadora acima, **7 ficaram “sem obra” de fechamento comprovado**: E1-sem-previsao, E3-turno-aberto, E4-rotulo-do-desenho, E6-sistema-bate, E6-exportavel, E6-piso-185 e BATERIA. (app/core/espelho_verdade.py:49,61,94,96,143,151,181,185,191; app/core/placar_estrutural.py:74,146,211,433; vínculos documentais na tabela)

**Espera Ronald:**

- **E1-passivo-53:** `!` caso a caso para os dez livres; O50 mantém o passivo esperando autorização. (app/core/espelho_verdade.py:58; app/docs/BACKLOG.md:1388)
- **E5-le-da-celula:** `!` para escrever `encerrado` nas linhas da 10 após export; migration e censo estão dispensados desse aval. (app/docs/BACKLOG.md:2578)
- **R4:** a linha do programa ainda registra espera de `!`, e as portas de demissão/encerramento de vínculo também o exigem; há cortes separados para Pauta DP e para O269, sem prova aqui de satisfação integral dessas condições. (app/docs/BACKLOG.md:1230; app/core/placar_estrutural.py:389–390; app/docs/BACKLOG.md:334–337,1611)
- **E3-separados-30 — ressalva:** consta literalmente `ESPERA_RONALD`, mas a prova diz “09 está com o Gabriel”; a decisão concreta ainda exigida de Ronald é **nao nomeado** nessa prova. (app/core/espelho_verdade.py:96; app/docs/BACKLOG.md:1579)

R3 tem resposta registrada: permanece aberto pela divergência da grade, com dono ESTRUTURA; a redação antiga sobre não carimbar a própria meta não demonstra uma espera atual de Ronald. (app/docs/BACKLOG.md:338–342; app/core/placar_estrutural.py:185–188)

**perguntas de lei: 0**, no recorte acima. As antigas perguntas referidas pelo R6 têm respostas posteriores registradas; a pergunta sobre dia aberto bloquear a folha consta fechada pela L-118, e “SEM juiz” na tabela de consulta não constitui, por si, lei faltante. (app/core/placar_estrutural.py:593; app/docs/BACKLOG.md:1538,1546,1742; app/docs/JUIZES.md:7–12,105,109)

## 15/11 23:50 · sessao fc196c01b64b · - · - · origem=Ronald (tmux)

Constantes em `app/core/espelho_verdade.py:35`: `FEITO='FEITO'`, `EM_CURSO='EM_CURSO'`, `FALTA='FALTA'`, `ESPERA='ESPERA_RONALD'`. Seleção: **8 EM_CURSO + 1 ESPERA_RONALD = 9 itens**, confirmada também pelo balanço de `app/docs/BACKLOG.md:1854`.

| item / fonte | vínculo documental comprovado | destrava / dono |
|---|---|---|
| E1-passivo-53 — `app/core/espelho_verdade.py:48`, estado em `:49` | **O50**, `app/docs/GRADE-MESTRA.md:862`, estado literal `PARADA (passivo espera !)`; registro detalhado em `app/docs/BACKLOG.md:1388`, estado `! para o passivo dos 62 (DRY por colab no RELATO); a fatia de codigo espera vez` | Passivo: 43 congelados e 10 livres ao propositor com `!` caso a caso, `app/core/espelho_verdade.py:57` e `:58`; dono **Ronald para o !**, pelo corte nominal em `:49`; execução de cadastro pelo propositor, `:58`. |
| E1-sem-previsao — `app/core/espelho_verdade.py:60`, estado em `:61` | O programa **O54** contém literalmente “censo e cura das celulas de trabalho sem previsao valida”, `app/docs/BACKLOG.md:1395`; existe em `app/docs/GRADE-MESTRA.md:72`, estado `REGISTRADA (programa parcial pelas fontes vivas...)`. Obra específica para os **35/28 atuais**: não nomeada em `app/core/espelho_verdade.py:61`. | Zerar dias sem previsão válida é porta E1, `app/core/espelho_verdade.py:40`; causa/dono atuais **nao nomeado**, `:61`. |
| E3-turno-aberto — `app/core/espelho_verdade.py:93`, estado em `:94` | **O86**, `app/docs/GRADE-MESTRA.md:881`, estado `REGISTRADA (guarda com DIFF zero; investigação residual)`, trata diretamente turnos abertos, `app/docs/BACKLOG.md:1427`. Entretanto o registro não liga seus casos antigos aos **347/262 atuais**; fechamento integral [nao verificado]. | A fonte atual mede ímpares e fora da faixa, sem nomear execução/dono, `app/core/espelho_verdade.py:94`. O diagnóstico antigo da O86 separa saída não registrada de geometria e cadastro, `app/docs/BACKLOG.md:1427`; não transportar essa classificação para a medida atual. |
| E3-separados-30 — `app/core/espelho_verdade.py:95`, estado em `:96` | **CORTE-B-30**, `app/docs/GRADE-MESTRA.md:845`, estado `PARADA (congelada)`; `app/docs/BACKLOG.md:1224`, estado `CONGELADA (L-096) -- idem: lista colab a colab, e a obra corre`. | Descongelamento/corte do lote B, `app/docs/GRADE-MESTRA.md:845`; o item diz **ESPERA_RONALD**, mas sua prova diz “09 esta com o **Gabriel**”, `app/core/espelho_verdade.py:96`; mesmo registro em `app/docs/BACKLOG.md:1579`. |
| E4-rotulo-do-desenho — `app/core/espelho_verdade.py:142`, estado em `:143` | **sem obra específica de execução identificada**. A tarefa consta em **ESPELHO-VERDADE-REMEDIDO**, `app/docs/BACKLOG.md:1579`, mas essa obra atualiza provas, conforme `app/docs/GRADE-MESTRA.md:89`; não demonstra executar a mudança da pílula. | Pílula de `templates/escala/tipos_lista.html:42` ler `desenho_do_turno`, fila 2/tela, `app/core/espelho_verdade.py:143`; dono **nao nomeado**, mesma fonte. |
| E5-le-da-celula — `app/core/espelho_verdade.py:150`, estado em `:151` | **O48**, `app/docs/GRADE-MESTRA.md:860`, estado `REGISTRADA (passos no ar; status e exportação pendentes)`; registro detalhado em `app/docs/BACKLOG.md:1386`. | Falta status `encerrado`, `app/core/espelho_verdade.py:151`; detalhado como migration+censo e pós-export, `app/docs/BACKLOG.md:1386`. Dono atual **nao nomeado**; a mesma linha preserva espera de `!` para passo 5 e leitura do DIFF pelo Ronald. |
| E6-sistema-bate — `app/core/espelho_verdade.py:180`, estado em `:181` | **R1-ESTRUTURA-91** é relacionada: `app/docs/GRADE-MESTRA.md:102`, estado `REGISTRADA (restante pendente)`; `app/docs/BACKLOG.md:1822` descreve famílias antigas. Causa/destino dos **E26+E9 atuais não medidos**, explicitamente `app/core/placar_estrutural.py:130`; obra que fecha esses casos integralmente **nao nomeada**. | Com batidas completas, espelho e oráculo baterem; divergência é “bug nosso”, `app/core/espelho_verdade.py:180`. Dono **ESTRUTURA**, explicitamente `:181`. |
| E6-exportavel — `app/core/espelho_verdade.py:184`, estado em `:185` | Programa **O54** contém certificação E6 e portas, `app/docs/BACKLOG.md:1395`, existe em `app/docs/GRADE-MESTRA.md:72`. **O125** trata alcance da lista, `app/docs/GRADE-MESTRA.md:894` / `app/docs/BACKLOG.md:1289`, mas de gap antigo de sete colaboradores; vínculo aos atuais [nao verificado]. | Certificação e destino conforme dono; entrada efetiva na lista administrativa não medida, `app/core/espelho_verdade.py:185`. Donos **ESTRUTURA / CADASTRO / BATIDA**, literais na mesma linha. |
| E6-piso-185 — `app/core/espelho_verdade.py:190`, estado em `:191` | **O83** trata classes de piso anteriores: `app/docs/GRADE-MESTRA.md:879`, estado `REGISTRADA (curso atual deste id não demonstrado)`; `app/docs/BACKLOG.md:1438`, estado `em curso`. Fechamento dos **E26+E9 atuais não nomeado**, `app/core/espelho_verdade.py:191` e `app/core/placar_estrutural.py:130`. | Nomear causa por colaborador, `app/core/espelho_verdade.py:190`; causas ESTRUTURA atuais não nomeadas, `:191`. Donos **ESTRUTURA / CADASTRO / BATIDA** na decomposição atual de `app/core/placar_estrutural.py:130`. |

Cuidados para consolidação:

- **R1-REMEDE não fecha nenhum desses itens**: está `FECHADA` como medição e registra cinco com número e quatro fora de alcance, `app/docs/BACKLOG.md:1854`.
- **O91 não fecha o passivo**: trata contador/instalação, `app/docs/BACKLOG.md:1433`; o espelho registra que instalação já ocorreu e a prova estava velha, `app/core/espelho_verdade.py:49`–`:59`.
- A contagem de “sem obra” depende de aceitar o programa geral **O54** e obras históricas apenas relacionadas como “obra que fecha”. Para vínculo integral comprovado aos números atuais, faltam vínculos específicos em E1-sem-previsao, E3-turno-aberto, E4-rotulo-do-desenho, E6-sistema-bate, E6-exportavel e E6-piso-185; não converter relação temática em prova de fechamento. As lacunas estão expressas nas respectivas fontes acima.
- Espera Ronald inequívoca: **E1-passivo-53**, `app/core/espelho_verdade.py:58` / `app/docs/GRADE-MESTRA.md:862`; **E3-separados-30** pelo estado literal, com ressalva Gabriel, `app/core/espelho_verdade.py:96`. **E5/O48** preserva espera de `!` na obra, mas o impedimento específico do item é status encerrado, `app/core/espelho_verdade.py:151` / `app/docs/BACKLOG.md:1386`.

## 15/11 23:50 · sessao 601f0f0373d8 · - · - · origem=Ronald (tmux)

Para o recorte dos 9 itens do espelho e dos 4 resultados parciais, recomendo **“perguntas de lei: 0”**: nenhuma lei ainda faltante foi confirmada nas pendências selecionadas; as duas antigas referidas em `app/core/placar_estrutural.py:593` têm respostas posteriores registradas em `app/docs/BACKLOG.md:1538`, `app/docs/BACKLOG.md:1546` e `app/docs/BACKLOG.md:1742`. A família escala também consta fechada em `app/docs/JUIZES.md:129`.

**Espera Ronald — registros confirmados:**

| Item | Espera escrita |
|---|---|
| E1-passivo-53 | Dez vínculos livres para propositor com `!` caso a caso; 43 congelados pela L-092 (`app/core/espelho_verdade.py:57`). O50: `PARADA (passivo espera !)` (`app/docs/GRADE-MESTRA.md:862`); BACKLOG exige `!` para o passivo (`app/docs/BACKLOG.md:1388`). |
| E5-le-da-celula | Falta status encerrado (`app/core/espelho_verdade.py:151`). O `!` dele é exigido para escrever o novo estado nas linhas da 10 após export; migration e censo não pedem aval (`app/docs/BACKLOG.md:2578`). |
| R4 | A linha PLACAR-ESTRUTURAL registra espera do `!` para os próximos casos da 09 exportada e paga (`app/docs/BACKLOG.md:1230`); portas de demissão/encerrar vínculo esperam `!` L-009 (`app/core/placar_estrutural.py:389`). Há corte para Pauta DP, sem apply, mantendo 09 intocada (`app/docs/BACKLOG.md:334`). |

**Cuidados para consolidar:**

- **E3-separados-30:** estado literal `ESPERA_RONALD`, mas a prova atribui a 09 ao **Gabriel**, não nomeia decisão pendente de Ronald (`app/core/espelho_verdade.py:95`; `app/docs/BACKLOG.md:1579`). A obra CORTE-B-30 está congelada (`app/docs/BACKLOG.md:1224`; `app/docs/GRADE-MESTRA.md:845`). Pode aparecer na lista como estado literal de espera, explicitando a divergência de responsável.
- **R3:** não classificar como espera atual de Ronald apenas pela redação antiga “placar não carimba a própria meta” (`app/core/placar_estrutural.py:185`). O corte já respondeu: grade 25/48 versus demais leitores 48/48 é divergência de leitor, **dono ESTRUTURA**, e R3 não fecha antes dessa concordância (`app/docs/BACKLOG.md:338`).
- **R6:** “dia em aberto bloqueia a folha?” já tinha lei a executar (`app/docs/BACKLOG.md:435`) e os dois pendentes de batida foram encerrados pela L-118 (`app/docs/BACKLOG.md:1546`). “SEM juiz” na tabela de lookup não basta para contar lei faltante (`app/docs/JUIZES.md:105`; `app/docs/JUIZES.md:109`).
- **BATERIA:** a pendência descrita é seis famílias sem bateria declarada, não lei faltante (`app/core/placar_estrutural.py:99`).
- **E6-exportavel:** donos CADASTRO/BATIDA seguem lista do admin e ESTRUTURA fila 1; entrada efetiva na lista não foi medida (`app/core/espelho_verdade.py:185`). Isso não documenta espera de Ronald nem lei faltante.

## 15/11 23:49 · sessao c9d31d83fb60 · - · - · origem=Ronald (tmux)

Constantes do placar: `FECHADO`, `PARCIAL`, `PENDENTE` (`app/core/placar_estrutural.py:45`). Há **4 resultados selecionados, todos PARCIAL**: BATERIA, R3, R4, R6 (`app/core/placar_estrutural.py:74`, `:146`, `:211`, `:433`). `PENDENTE` também pode ser atribuído por prova ausente (`app/core/placar_estrutural.py:649–651`); os quatro selecionados têm `prova` declarada (`:101`, `:197`, `:266`, `:591`).

| fonte | id | estado | obra existente e estado | destrava e dono |
|---|---|---|---|---|
| `app/core/placar_estrutural.py:74` | BATERIA | PARCIAL | **Sem obra específica comprovada para fechar a meta inteira.** A meta exige todas as famílias; faltam 6 de 7 (`app/core/placar_estrutural.py:79`, `:99–100`). `BATERIA-QUEIXAS-ADMIN` existe, mas abrange Q1–Q4, não declara fechar todas as famílias (`app/docs/BACKLOG.md:1867`; `app/docs/GRADE-MESTRA.md:103`, estado `REGISTRADA`). | Todas as famílias com bateria declarada e verde, sem allowlist, e nenhum dia ESTRUTURA novo sem destino (`app/core/placar_estrutural.py:79–80`). Dono da execução integral: **nao nomeado**; a origem dos casos ESTRUTURA é explícita (`:77–80`). A validação **Ronald+DP** só está registrada para `BATERIA-QUEIXAS-ADMIN` (`app/docs/BACKLOG.md:1867`). |
| `app/core/placar_estrutural.py:146` | R3 | PARCIAL | `PLACAR-ESTRUTURAL` — `PARADA (resultados parciais; espera autorização)` (`app/docs/GRADE-MESTRA.md:62`). `O130` existe e está `NO AR 04/10 10:02`, mas não fecha o restante (`app/docs/BACKLOG.md:1280`, `:338–342`). | Grade alcançar 48/48, concordando com cartão/tela/API; o complemento manda manter R3 aberto e classifica como divergência do leitor, **dono ESTRUTURA** (`app/docs/BACKLOG.md:338–342`). |
| `app/core/placar_estrutural.py:211` | R4 | PARCIAL | `PLACAR-ESTRUTURAL` — `PARADA (resultados parciais; espera autorização)` (`app/docs/GRADE-MESTRA.md:62`); a linha própria ainda diz `espera o !` (`app/docs/BACKLOG.md:1230`). | Meta ZERO em cada par (`app/core/placar_estrutural.py:215`); o corte posterior manda **Pauta DP com os dois números e não tocar a 09**, recusando REGEN para esses quatro colaboradores (`app/docs/BACKLOG.md:334–337`). Dono **ESTRUTURA** para os seis casos novos (`app/core/placar_estrutural.py:291–292`); decisões sobre a 09 são de **Ronald**, pelo corte referido como dele (`app/docs/BACKLOG.md:324–325`, `:334–337`). |
| `app/core/placar_estrutural.py:433` | R6 | PARCIAL | `PLACAR-ESTRUTURAL` — `PARADA (resultados parciais; espera autorização)`, explicitando lote do R6 (`app/docs/GRADE-MESTRA.md:62`). `O35` cobre contratos, estado `REGISTRADA (partes construídas; contratos incompletos)` (`app/docs/GRADE-MESTRA.md:896`). | Chegar a **20/20 e zero passo manual**, restando lote no registro do resultado (`app/core/placar_estrutural.py:434–440`); o critério dos contratos é exceções ZERO, selo da porta e `verde=True` no mesmo commit (`app/docs/BACKLOG.md:1444`). Dono global: **nao nomeado** nessas fontes; não converter `ESPINHA-E` em pessoa responsável sem fonte (`app/docs/GRADE-MESTRA.md:62`, `:896`). |

**Divergências que afetam a consolidação:**

- R3 não deve aparecer esperando apenas o carimbo de Ronald por causa de `app/core/placar_estrutural.py:185–188`: o complemento de 11/10 diz expressamente **“R3 — NÃO FECHA”**, pela divergência da grade, dono ESTRUTURA (`app/docs/BACKLOG.md:324`, `:338–342`).
- R4 conserva espera `!` em prosa antiga (`app/core/placar_estrutural.py:313–315`; `app/docs/BACKLOG.md:1230`), mas há corte posterior **R4-PAR6 = Pauta DP e 09 intocada** (`app/docs/BACKLOG.md:334–337`). Relatar ambos; não transformar o corte em autorização de apply.
- `O130` é id existente, não ausente dos docs (`app/docs/BACKLOG.md:1280`).
- `O35` existe e cobre contratos, mas não demonstra sozinho fechar a outra metade do R6, o lote de reatividade (`app/docs/GRADE-MESTRA.md:896`; `app/core/placar_estrutural.py:438–440`).
- `JUIZES.md` dizer **SEM juiz** não é registro de lei faltante; o arquivo declara ser lookup (`app/docs/JUIZES.md:7–12`, `:105`, `:109`).
- A obra `BATERIA-QUEIXAS-ADMIN` tem espera de validação Ronald+DP, porém não há vínculo explícito afirmando que ela fecha integralmente BATERIA (`app/docs/BACKLOG.md:1867`; `app/core/placar_estrutural.py:79–100`).
- Para perguntas de lei, o R6 cita historicamente duas em prova antiga (`app/core/placar_estrutural.py:591–593`), mas não basta para contá-las como **ainda faltantes**: a própria nota narra cortes posteriores (`:541–548`).

## 15/11 23:49 · sessao f29fd6469b4f · E4C-F1NOVA-B1 · verificador · origem=Code

**O delta fecha os três contraexemplos antigos de cache, mas não garante o par sob concorrência; o contrato também conserva uma falsa absolvição e introduz um falso positivo.** (`app/chamados/models.py:3572`; `app/chamados/models.py:3552`; `app/chamados/tests/test_contract_par_resposta.py:75`; `app/chamados/tests/test_contract_par_resposta.py:93`)

Identidade independente de `HEAD`, comparação Git com `1fcb5d0b6` e execução com Django/banco: **[não verificado]**. A análise abaixo confronta a cópia com o delta fornecido.

1. **PROCEDE o fechamento de 2a, 2b, 2c e 3; o item 5 fecha somente o exemplo anterior.**

   | Item da volta 2 | Execução mental no código atual |
   |---|---|
   | **2a — outra instância zerou o par** | O último `p.save(update_fields=['respondida_em'])` relê `(None, '')`, monta `(I, '')` e recusa. Há teste explícito desse caso. (`app/chamados/models.py:3572`; `app/chamados/models.py:3574`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:146`) |
   | **2b — rollback e repetição do save parcial** | Depois do rollback, relê `(None, '')`; repetir somente o instante monta `(I, '')` e recusa. A comparação não usa estado memorizado após o save anterior. (`app/chamados/models.py:3572`; `app/chamados/models.py:3576`; `app/chamados/services/fio.py:1606`) |
   | **2c — legado intocado após rollback** | Relê `(None, 'legado')`; memória e banco coincidem, portanto retorna sem recusar. O teste novo reproduz essa sequência. (`app/chamados/models.py:3576`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:159`) |
   | **3 — fotografia tratada como banco atual** | A guarda usa leitura local do banco, sem consultar os atributos antigos. (`app/chamados/models.py:3568`; `app/chamados/models.py:3572`) |
   | **5 — saída antecipada aninhada que não governa a escrita** | O exemplo antigo agora é acusado: a saída antecipada precisa estar diretamente em `fn.body`. Isso fecha aquele exemplo, mas rejeita também uma forma legítima, demonstrada no item 5 abaixo. (`app/chamados/tests/test_contract_par_resposta.py:93`; `app/chamados/tests/test_contract_par_resposta.py:162`) |

2. **PROCEDE existir contraexemplo concorrente; NÃO PROCEDE considerar a leitura dentro do `atomic` suficiente.**

   Intercalação entre dois saves, partindo de banco e instância `p` com `(I, 'bati')`: a leitura é simples, a igualdade dispensa a recusa e a escrita acontece depois. (`app/chamados/models.py:3570`; `app/chamados/models.py:3572`; `app/chamados/models.py:3576`; `app/chamados/models.py:3552`)

   ```python
   # A inicia:
   p.save(update_fields=['respondida_em'])
   # A lê (I, 'bati') e passa pela igualdade; pausa antes do UPDATE.

   # B executa e confirma:
   q.respondida_em, q.resposta_colab = None, ''
   q.save(update_fields=['respondida_em', 'resposta_colab'])

   # A continua: escreve apenas respondida_em=I.
   # Resultado: (I, ''), ilegal, sem recusa de A.
   ```

   B pode obedecer à `trava_na_ordem`; isso não serializa A, cujo `save` não adquiriu essa trava antes da leitura. Se ambos os atos já seguram a trava regulamentar antes de entrar no save, essa intercalação fica excluída. Ocorrência em produção, isolamento efetivo e reprodução SQL: **[não verificado]**. (`app/chamados/models.py:2727`; `app/chamados/models.py:2734`; `app/chamados/models.py:3548`)

   Nos demais caminhos pedidos, com valores concretos e `update_fields` por keyword:

   | Caminho | Resultado da guarda |
   |---|---|
   | `create` | Com `adding=True`, valida ambas as metades da memória. (`app/chamados/models.py:3569`; `app/chamados/models.py:3574`) |
   | `only/defer`, refresh integral ou parcial | Não há cache próprio a invalidar; a guarda relê o banco. Sem `update_fields`, acessa ambas as metades antes do `super`, podendo carregar campos diferidos. (`app/chamados/models.py:3572`; `app/chamados/models.py:3574`; `app/chamados/models.py:3552`) |
   | Save parcial tocando o par | A metade excluída vem do banco. Save excluindo ambas retorna imediatamente. (`app/chamados/models.py:3566`; `app/chamados/models.py:3574`) |
   | PK explícito, `adding=False` | Consulta a linha pelo PK; não depende de ter passado por `from_db`. (`app/chamados/models.py:3569`) |
   | Linha apagada antes da guarda | `_banco=None`: valida ambas as metades, sem absolvição por igualdade com a linha antiga. O comportamento subsequente de UPDATE/INSERT do Django: **[não verificado]**. (`app/chamados/models.py:3573`; `app/chamados/models.py:3574`; `app/chamados/models.py:3576`) |
   | Rollback externo | O save seguinte relê o banco revertido; não herda cache do save desfeito. (`app/chamados/models.py:3572`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:159`) |
   | `bulk_update`/`QuerySet.update` fora do save | Não demonstram falha **deste save**. A própria fixture introduz legado por `update` direto. (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:62`) |

   Não encontrei nessas sequências, sem concorrência, recusa nova de legado cujo par efetivamente permanece igual: a igualdade explícita o libera. A implementação interna do Django para saves diferidos permanece **[não verificado]**. (`app/chamados/models.py:3576`)

   Há ainda uma ressalva já presente na forma anterior, portanto sem atribuição ao delta: a guarda lê `update_fields` somente de `kwargs`, enquanto encaminha `*args`. Se o Django aceitar `p.save(False, False, None, ['respondida_em'])`, banco `(None, '')` e memória `(I, 'bati')` passam pela guarda completa, mas gravam somente o instante. Aceitação posicional nessa instalação: **[não verificado]**. (`app/chamados/models.py:3549`; `app/chamados/models.py:3552`; `app/chamados/models.py:3574`)

3. **NÃO PROCEDE haver leitor executável localizado de `_par_carregado` ou `_par_lido`.**

   O grep encontrou somente menção histórica em `app/docs/RELATO.md:9163`. `from_db` aparece no catálogo genérico de métodos, sem dependência específica de PerguntaDisputa; a fixture continua usando o refresh normal. Não localizei dependência dos overrides removidos em código ou testes; dependência dinâmica: **[não verificado]**. (`app/core/management/commands/describe.py:68`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:66`; `app/chamados/models.py:3561`)

4. **PROCEDE o respeito à lei de trava e à catraca A; NÃO PROCEDE inferir disso segurança concorrente.**

   A consulta nova não adquire `select_for_update`; respeita a exclusividade de `trava_na_ordem`. Executei o varredor AST isoladamente: `varrer_travas=[]`. A catraca exige essa lista vazia, mas não prova que todo chamador do save já adquiriu a trava. (`app/chamados/models.py:2729`; `app/chamados/models.py:3572`; `app/chamados/tests/test_e1_v7_catracas.py:158`; `app/chamados/tests/test_e1_v7_catracas.py:390`)

   **NÃO PROCEDE haver teste localizado com `assertNumQueries` envolvendo save de PerguntaDisputa tocando o par.** Os dois candidatos de resposta automática medem o segundo chamado, que retorna antes do save. Contagem executada com banco: **[não verificado]**. (`app/chamados/tests/test_porta_chamado_e3.py:47`; `app/chamados/tests/test_e3_10_cura_no_escritor.py:84`; `app/chamados/services/disputa_emissao.py:1663`; `app/chamados/services/disputa_emissao.py:1677`)

   Custo explícito da guarda: uma leitura para instância existente quando o par é tocado ou `update_fields=None`; zero quando o par é excluído ou `adding=True`. Carregamentos diferidos podem somar consultas. (`app/chamados/models.py:3566`; `app/chamados/models.py:3569`; `app/chamados/models.py:3574`)

5. **PROCEDE falsa absolvição e PROCEDE acusação de escritor legítimo.**

   **Falsa absolvição persistente:** executei o helper real em memória; este exemplo retorna `None`. A guarda pode ler `p`, enquanto o update escreve outra pergunta `q`: com texto em `p` e par vazio em `q`, carimba `q` sem texto. `_le` verifica somente o nome do atributo, sem vincular a linha lida à escrita. (`app/chamados/tests/test_contract_par_resposta.py:75`; `app/chamados/tests/test_contract_par_resposta.py:94`; `app/chamados/tests/test_contract_par_resposta.py:111`)

   ```python
   def f(p, q, agora):
       if not (p.resposta_colab or '').strip():
           return
       PD.objects.filter(pk=q.pk).update(respondida_em=agora)
   ```

   **Falso positivo introduzido pela restrição a `fn.body`:** este exemplo retorna `'respondida_em'`, embora toda execução do save passe pela guarda de texto da própria instância. O `if` interno governa a escrita, mas não está diretamente no corpo da função. (`app/chamados/tests/test_contract_par_resposta.py:93`; `app/chamados/tests/test_contract_par_resposta.py:97`; `app/chamados/tests/test_contract_par_resposta.py:113`)

   ```python
   def f(p, habilitado, agora):
       if habilitado:
           if not (p.resposta_colab or '').strip():
               return
           p.respondida_em = agora
           p.save(update_fields=['respondida_em'])
   ```

   A varredura atual retornou `_ofensores() == []`: o falso positivo demonstra uma forma legítima rejeitada, sem escritor atual localizado nessa situação. Os dois escritores examinados anteriormente continuam corretamente absolvidos pela forma. (`app/chamados/tests/test_contract_par_resposta.py:116`; `app/chamados/models.py:3231`; `app/chamados/models.py:3253`; `app/chamados/services/disputa_emissao.py:1692`; `app/chamados/services/disputa_emissao.py:1698`)

## 15/11 23:49 · sessao 2d5da68b7f6c · ESPELHO-VERDADE-FECHO-V0 · sonda · origem=Code

Saída integral fornecida: [nao verificado]

```text
sonda_leitura: script.py [fe38ad6dc95a1ace] no cpuset 4-7, lendo sombra como leitor (LEITURA)
sonda_leitura: arvore=/home/ronald/v0-arvore-3580b35e0 (copia), banco segue sombra como leitor
docker: Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: error during container init: error mounting "tmpfs" to rootfs at "/app/logs": create mountpoint for /app/logs mount: make mountpoint "/app/logs": mkdirat /var/lib/docker/rootfs/overlayfs/d3b43d2121a485aa3f2f6c0439523bd7423882de7644711b73b9080567ea8b89/app/logs: read-only file system

Run 'docker run --help' for more information
rc_sombra=125
```

O dado fornecido contém a falha de execução acima, sem a saída da parte 4 de 6. [nao verificado]

- `SOMA=True`: [nao verificado].
- `ESTRUTURA_DIAS_LINHAS=True`: [nao verificado].
- Há `INCOMPLETO` na parte 4: [nao verificado].

## 15/11 23:49 · sessao 2f8340c4a6ad · ESPELHO-VERDADE-FECHO-V0 · sonda · origem=Code

[nao verificado] Saída integral fornecida; o anexo não contém a parte 3/6 solicitada:

```text
sonda_leitura: script.py [82c88c2988af3846] no cpuset 4-7, lendo sombra como leitor (LEITURA)
sonda_leitura: arvore=/home/ronald/v0-arvore-3580b35e0 (copia), banco segue sombra como leitor
docker: Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: error during container init: error mounting "tmpfs" to rootfs at "/app/logs": create mountpoint for /app/logs mount: make mountpoint "/app/logs": mkdirat /var/lib/docker/rootfs/overlayfs/c1fc8c1b796422444d13121295ed0346a8470b12b8787743a7783a2d97d043fa/app/logs: read-only file system

Run 'docker run --help' for more information
rc_sombra=125
```

- SOMA é True: [nao verificado]; linha ausente no dado fornecido.
- ESTRUTURA_DIAS_LINHAS é True: [nao verificado]; linha ausente no dado fornecido.
- Há INCOMPLETO na parte: [nao verificado]; o marcador não aparece no dado fornecido, mas a parte solicitada não foi apresentada.

## 15/11 23:48 · sessao 1d41892ae2fa · ESPELHO-VERDADE-FECHO-V0 · sonda · origem=Code

Transcrição integral do dado fornecido; identificação como “parte 2 de 6 (R1 remedido)” [nao verificado]:

```text
sonda_leitura: script.py [23c7f47877c9a3fd] no cpuset 4-7, lendo sombra como leitor (LEITURA)
sonda_leitura: arvore=/home/ronald/v0-arvore-3580b35e0 (copia), banco segue sombra como leitor
docker: Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: error during container init: error mounting "tmpfs" to rootfs at "/app/logs": create mountpoint for /app/logs mount: make mountpoint "/app/logs": mkdirat /var/lib/docker/rootfs/overlayfs/520bfee192a3646b1980c3d4742cc404e62946397531ee22586a173a28f97e78/app/logs: read-only file system

Run 'docker run --help' for more information
rc_sombra=125
```

- **SOMA é True:** [nao verificado]; a linha não consta do dado fornecido.
- **ESTRUTURA_DIAS_LINHAS é True:** [nao verificado]; a linha não consta do dado fornecido.
- **Há INCOMPLETO na parte:** [nao verificado]; o marcador não aparece no trecho fornecido.

_5870 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
