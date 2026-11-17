# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 08:04** · publicado em **17/11/2026 08:04** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 08:04 · sessao eac78c1f358b · - · - · origem=Ronald (tmux)

Para os seis dias desta exploração, o agregado R3 não contém entradas suficientes para escolher um ramo causal do pareador; permanecem **sem causa**, com sonda **S**. [não verificado]

| Caso | Conclusão e dado faltante | Referências da regra |
|---|---|---|
| col876 2026-09-21 | Sem causa: “05:36 fora do turno” não informa tipo, papel, carimbo, borda ou `data_turno`. BATIDA é hipótese de triagem, sem comprovação. [não verificado] | `app/ponto/turnos.py:1310-1326` carimba papel; `:1377-1382` papel vence tipo; `:1402-1419` E pode abrir outro turno; `:285-309` atribuição à véspera. |
| col130 2026-10-29 | Sem causa: o segmento noturno pode ser perdido ou atribuído a outro turno/dia por decisões diferentes. Faltam batidas completas e papéis. BATIDA é hipótese, sem comprovação. [não verificado] | `app/ponto/turnos.py:1377-1382`; `:1417-1419`; `:285-309`; `:719-721` realizado filtra por `data_turno`. |
| col220 2026-11-10 | Sem causa: “06:00 fora do turno” não identifica por que ficou fora; faltam papel/ata e turno anterior/posterior. BATIDA é hipótese, sem comprovação. [não verificado] | `app/ponto/turnos.py:1310-1326`; `:1377-1382`; `:1430-1457` teto pode separar S; `:285-309` dono do dia. |
| col868 2026-09-22 | Sem causa: 6x1 diurno diante de quatro batidas noturnas sustenta **candidata CADASTRO**, não cadastro errado provado; faltam vínculo vigente, célula geradora/DNA e tipos/papéis. [não verificado] | `app/docs/LEIS.md:122` L-099; `app/ponto/turnos.py:1695-1700` duas pontas muito longe desabilitam papel; `:1965-2000` vínculo do dia governa segmento e marcos. |
| col516 2026-10-23 | Sem causa: folga com realizado e zero turno E6 não identifica defeito na tela nem no pareador. Dono provável não é dedutível; qualquer ESTRUTURA/BATIDA/CADASTRO seria hipótese. [não verificado] | `app/ponto/turnos.py:719-721`, `:735-771` realizado vem dos turnos do dia; `app/ponto/services/espelho.py:450-460` cards vêm de períodos do motor mapeados por entrada; `:783` minutos vêm de `_real_dia`. |
| col884 2026-11-08 | Sem causa: “tipos invertidos” **não demonstra** recusa da pausa, porque pares declarados/ata/mistos podem passar mesmo assim. BATIDA é hipótese; falta verificar os carimbos das duas pontas e intervalo declarado. Atestado rejeitado é excluído dos efeitos; não explica sozinho a divergência de realizado. [não verificado] | `app/ponto/turnos.py:811-815` declarado passa primeiro; `:875-883` aceita ata, misto ou S→E; `:756-766` fallback para janela declarada; `:2528-2531` folha só aprovada/cobrança exclui rejeitada. |

Duas correções relevantes nas referências do relato:

- Nesta cópia, `app/ponto/turnos.py:2798-2814` implementa expansão de datas da ausência. O filtro do atestado rejeitado é `app/ponto/turnos.py:2523-2531`.
- `app/ponto/turnos.py:502-567` implementa leitores de intervalo/piso, não o corpo de `parear_turnos`; o corpo começa em `app/ponto/turnos.py:1192`.

Caminho real útil para a consolidação:

- `turnos_do_colab` carrega batidas vivas com pad de dois dias atrás e um à frente, carrega células e vínculos, e chama o corpo puro: `app/ponto/turnos.py:1847`, `:1867-1893`, `:1920`.
- O corpo obtém papéis da ata e segmenta pelo vínculo do dia; faz cópias rasas das batidas, chama o pareador e filtra por `data_turno`: `app/ponto/turnos.py:1944`, `:1965-1971`, `:1988-2000`.
- `parear_turnos` usa papel antes do tipo; `X`/`Xi` viram internas, com marca declarada para `Xi`: `app/ponto/turnos.py:1310-1326`, `:1377-1397`.
- A atribuição do turno consulta a véspera noturna e seu envelope; S órfã também participa: `app/ponto/turnos.py:285-309`.
- Realizado escolhe turnos por `data_turno`; desconta pausa reconhecida ou, na ausência dela e modo fixo, interseção da janela declarada: `app/ponto/turnos.py:719`, `:756-771`.
- Tela guarda cards de períodos do motor e número do realizado por caminhos distintos: `app/ponto/services/espelho.py:450-460`, `:767`, `:783`. Isso é distinção de fontes, não prova de incoerência nestes seis casos. [não verificado]

Na sonda, imprimir por `colNNN`: batidas com `pk,timestamp,tipo,is_intervalo,retratada_em,origem,escala_id`; todos os turnos e seus `data_turno,entrada,saida,aberto`; carimbos `_papel_ata,_intra_dur,_intra_ata,_intra_declarado`; intervalos aceitos/abaixo do piso; vínculo vigente e gerador, DNA/ata por whitelist; `RealizadoDoDia`; linha da função real `espelho_do_colab`; ausência somente por campos tipados, sem texto livre. Os campos existem em `app/ponto/models.py:22-29`, `:45`, `:67`; marcas em `app/ponto/turnos.py:1315-1325`; API real em `app/ponto/services/espelho.py:1194-1202`. Evitar `str(model)` porque Batida expõe nome em `app/ponto/models.py:111-112`.

## 17/11 08:04 · sessao 80b57feb2634 · - · - · origem=Ronald (tmux)

Achados estáticos prioritários, execução [nao verificado]:

| Achado | Classe | Evidência | Gravidade | Ação |
|---|---|---|---|---|
| RESPOSTA corrigida mantém validação antiga na nova leitura. A porta limpa `validada_em`, mas o redutor não considera RESPOSTA entre atos limpadores. | 2/extra | Diff `app/chamados/models.py:4186`, `:4213-4218`; cópia `app/chamados/services/disputa_emissao.py:1484-1496`, `:1523-1526`. | Alta | Acrescentar RED real VALIDACAO→nova resposta pelo colaborador. Preservar distinção com resposta automática: ela grava resposta sem limpar validação (`disputa_emissao.py:1674-1679`). Simplesmente incluir toda RESPOSTA em `_LIMPAM_VALIDACAO` altera essa outra porta. |
| Ressuscitação apaga projeção sem ato limpador; leitura recupera a VALIDACAO que a própria porta acaba de preservar. | 1/2 | Diff `models.py:4203-4221`; cópia `models.py:1621-1622`, `:1632-1640`. | Alta | Resolver representação desse efeito respeitando decisão existente que proíbe usar REABERTURA indiscriminadamente. O controle novo exige leitura vazia depois de iniciar com `validada_em=T2`, mas a porta preserva VALIDACAO e o redutor devolve T2. Falha prevista por leitura, run [nao verificado]. |
| Contador do selo diz seis, mas os 11 casos codificados produzem sete validadas pela própria regra. | 3/4 | Diff `test_e4c_f2_leitura_do_ato.py`, método `test_face_casa_leitura_em_todas_as_perguntas_do_caso`, lista `casos` e `assertEqual(len(face), 6)`. | Média | Conferir casos 1, 2, 4, 5, 7, 8 e 9; todos conservam validação. Não certificar GREEN desse selo sem corrigir expectativa e executar. |

A docstring do redutor afirma reproduzir “o efeito que o ESCRITOR de cada ato grava”, mas RESPOSTA tem efeitos diferentes sobre validação conforme a porta: a humana limpa (`app/chamados/services/disputa_emissao.py:1484-1496`), a automática preserva (`app/chamados/services/disputa_emissao.py:1674-1679`). Portanto, a afirmação universal do comentário novo é falsa; o registro já reconhece escritores distintos de `validada_em` (`app/docs/JUIZES.md:38-39`).

Absorção: a porta grava carimbo com `resposta='validar'` e registra apenas ABSORCAO (`app/chamados/services/materializacao.py:1046-1051`); o fallback novo consegue ler os carimbos projetados sem linha correspondente, como descrito no diff. Incoerência adicional demonstrada nesse caminho: nada. Escrita nova fora de `DecisaoPergunta.registrar`: nada no diff, que só extrai `_sem_correspondencia` e adiciona leitura; porta canônica na cópia `app/chamados/models.py:4123-4137`. Relógio novo fora da borda: nada no redutor, que ordena os instantes recebidos; cortes temporais existentes continuam no C6 (`app/chamados/juizes.py:1382-1404`).

## 17/11 08:04 · sessao aa5a69c70063 · - · - · origem=Ronald (tmux)

Os seis casos permanecem **“sem causa”**, pois o relato agrupado não identifica qual guarda falhou em cada batida; atribuir a todos a causa “folga/feriado sem marcos” seria conclusão sem prova. [não verificado]

| Caso | Conclusão | Dono provável |
|---|---|---|
| col369 2026-10-12 | Sem causa: faltam papéis/marcas e intervalo efetivo. [não verificado] | ESTRUTURA, hipótese [não verificado] |
| col864 2026-10-12 | Sem causa: faltam papéis/marcas e intervalo efetivo. [não verificado] | ESTRUTURA, hipótese [não verificado] |
| col114 2026-11-08 | Sem causa: o vão 14:50–16:00 relatado não determina se suas pontas foram marcadas/interiores. [não verificado] | ESTRUTURA, hipótese [não verificado] |
| col114 2026-11-15 | Sem causa: faltam papéis/marcas e intervalo efetivo. [não verificado] | ESTRUTURA, hipótese [não verificado] |
| col723 2026-10-08 | Sem causa: o vão 10:44–13:20 relatado não determina se suas pontas foram marcadas/interiores. [não verificado] | ESTRUTURA, hipótese [não verificado] |
| col746 2026-09-24 | Sem causa: o vão 13:02–15:39 relatado não determina se suas pontas foram marcadas/interiores. [não verificado] | ESTRUTURA, hipótese [não verificado] |

Regras que a sonda deve discriminar:

- **Sem bordas não significa sem pausa:** quando não há marcos de intervalo, o pareador chama `_marca_intra_duracao`; sem bordas, essa função reconhece S→E até 3 h quando há E anterior e S imediatamente posterior dentro de `cont_max_s`. `app/ponto/turnos.py:392-418`, `:1217-1221`.
- Com bordas, exige S longe delas por mais de 90 minutos; a E de volta também deve estar longe, salvo prova de continuidade por S posterior. `app/ponto/turnos.py:431-457`.
- A ata marca X/Xi como internas; Xi também recebe `_intra_declarado`. `app/ponto/turnos.py:1310-1325`.
- `_pares_marcados` admite primeiro pares declarados interiores; para os demais exige marca na primeira ponta, ambas fora das bordas e autoridade da ata/mista ou tipos gravados S→E. `app/ponto/turnos.py:811-883`.
- `intervalos_do_turno` aplica piso de 15 minutos; os vãos relatados de 39–157 minutos não explicam falha por esse piso. Regra: `app/ponto/turnos.py:499`, `:542-546`; aplicação aos seis vãos: [não verificado].
- O realizado seleciona apenas turnos com `data_turno` igual ao dia, soma bordas fechadas e desconta intervalo reconhecido; na ausência dele, tenta janela declarada se não for livre, senão desconta zero. `app/ponto/turnos.py:719-771`.
- O dia do turno passa por `_mk` e `_data_do_turno`, inclusive regras de jornada noturna da véspera. `app/ponto/turnos.py:285-309`, `:312-318`.
- A tela escolhe vínculo e intervalo efetivo, pede o realizado à autoridade e publica os minutos e a janela descontada. `app/ponto/services/espelho.py:286-294`, `:675`, `:783`, `:819`.

Campos indispensáveis: batidas cruas com PK, timestamp, tipo, `is_intervalo`, `retratada_em`; **batidas das cópias devolvidas nos turnos** com `_papel_ata`, `_intra_dur`, `_intra_ata`, `_intra_declarado`; turnos com `data_turno`, entrada/saída, aberto, batidas, `_fim_fechado`, `_pares_marcados`, `intervalos_do_turno`; célula/DNA/ata, vínculo escolhido, modo livre, intervalo efetivo e resultado `_realizado_do_dia_tela`. As cópias são relevantes porque os carimbos são aplicados por segmento: `app/ponto/turnos.py:1944`, `:1984-2000`.

A lei L-099 separa donos ESTRUTURA/CADASTRO/BATIDA; o código lido não permite transformar a hipótese ESTRUTURA dos seis em dono comprovado. `app/docs/LEIS.md:122`; aplicação aos seis: [não verificado].

## 17/11 08:04 · sessao 6d845629636e · - · - · origem=Ronald (tmux)

| Caso | Causa demonstrável no código; limite do diagnóstico | Obra registrada | Menor escopo que fecharia |
|---|---|---|---|
| HJ-005-U1 — col42, 01/10 | O caminho compatível com o sintoma é: fato real/órfão em dia encerrado + cobertura aprovada gera `FATO_EM_AUSENCIA` (`app/ponto/supra_juiz.py:392`, `:413`, `:417`); esse veredito integra os irregulares (`app/ponto/status_dia.py:68`), e a retenção da célula acusadora vira `espelho_` + veredito (`app/folha/export.py:1273`, `:1293`). **Por que a cobertura desse dia foi considerada aprovada, se parcial ou inteira, e se o fato caiu dentro do período: [nao verificado].** O complemento exige precisamente reconciliar dono/destino e diz que COBERTURA-PARCIAL não foi demonstrada para 01/10 (`app/docs/BACKLOG.md:865`). | **BO-ADMIN-2710**, complemento de censo já registrado (`app/docs/BACKLOG.md:865`; GRADE `app/docs/GRADE-MESTRA.md:137`). **Cura de código específica: sem obra demonstrada**; a obra COBERTURA-PARCIAL trata nominalmente 08/15/22-10 (`app/docs/BACKLOG.md:874`). | Censo nominal de 01/10: comparar célula/protestos/status/retentor, ausência efetivamente usada, período e horas reais; ler o mesmo `InsumosDoDono.dono` (`app/ponto/management/commands/e6_oraculo.py:182`). Se a cobrança for legítima, corrigir dono/destino; se houver contradição, registrar a menor cura no produtor/leitor responsável. **Necessidade de alteração funcional: [nao verificado]**; escopo de reconciliação pedido em `app/docs/BACKLOG.md:865`. |
| HJ-012-U2 — col239, 12/10 | `espelho_cobrado` nasce quando a célula tem veredito acusador e não recebe as exceções de protesto/pausa (`app/folha/export.py:1273`, `:1293`). O juiz do dono devolve ESTRUTURA como resíduo quando não encontra as condições de CADASTRO/BATIDA (`app/ponto/management/commands/e6_oraculo.py:98`, `:104`, `:106`, `:110`); isso **não demonstra que a cobrança seja defeituosa**. O complemento registra conflito entre ingestão “ESTRUTURA” e medição anterior “legítimo → DP” (`app/docs/BACKLOG.md:1589`). **Causa específica de defeito: [nao verificado].** | **BUG-RETRATO-VELHO-NA-TELA**, complemento de reconciliação já registrado (`app/docs/BACKLOG.md:1589`; GRADE `app/docs/GRADE-MESTRA.md:845`). **Cura específica para HJ-012-U2: sem obra demonstrada.** | Reconciliar, no mesmo instante/competência, retenção real, célula/protestos, juiz do dono e a testemunha anterior do feriado; concluir DP ou identificar contradição de código. Não presumir que aviso/invalidação de retrato cure esse dia: frescor mede `lavrado_em` contra atualização do fechamento (`app/folha/retrato_folha.py:1413`, `:1420`), enquanto o conflito nominal está registrado em `app/docs/BACKLOG.md:1589`. **Alteração funcional necessária: [nao verificado].** |

**Total:** os “2 sem obra” do quadro ESTRUTURA são estes mesmos dois casos, não outros dois (`app/docs/RELATO.md:666`). Há **2 complementos de investigação registrados**, mas **0 curas específicas demonstradas** para esses dois dias (`app/docs/BACKLOG.md:865`, `:1589`). O quadro recente mantém os dois no balde “sem obra” e registra falta de individualização de 4 CADASTRO + 1 BATIDA (`app/docs/RELATO.md:240`).

## 17/11 08:03 · sessao 0280a0e823b0 · CODEX-NAO-FICA-OCIOSO · verificador · origem=Code

**AVAL-DEVOLVIDO — somente item 5:** faltam fechar a definição da medida e o contrato antes da construção. (`CLAUDE.md:35-38,394`)

As referências aos anexos abaixo contam linhas desde a primeira linha de cada literal fornecido.

1. **As premissas conferem parcialmente.** `bin/relato.sh` gera `ESTADO.md` chamando `gerar_estado.py`; a linha atual do Codex vem de `codex_limite_estado.ler/linha`. Portanto, existe caminho para acrescentar a medida ao ESTADO. (`bin/relato.sh:37-38`; `bin/gerar_estado.py:267-270,337-339`)

   O formato declarado confere com o escritor: timestamp, obra, papel, hash, pid e `ini|fim`, separados por TAB; `fim` acrescenta sessão, rc e duração. Isso permite contar **inícios do wrapper** por hora e calcular distâncias entre eles. Existência e conteúdo efetivos de `logs/codex/chamadas.log` em produção: **[nao verificado]**. (`bin/codex_pergunta.sh:266-273,308-309`)

   Ressalva decisiva: `ini` é gravado **antes da espera pelo slot**, da trava da resposta e da execução do Codex; pode haver `ini` seguido de recusa sem execução. O próprio código o reconhece como tentativa, sem prova de execução. (`bin/codex_pergunta.sh:308-309,364-394,418-426`; `bin/disjuntor.py:2072-2075`)

   Consumidor implementado da fila específica não foi localizado na cópia: **[nao verificado]** quem a lê operacionalmente. O literal fornecido tem **seis ABERTOS**, R2–R7; os cinco pertencem ao caso de teste proposto. (`fila.md:5-10`, literal; `complemento1_queima_util_1711.txt:13`, literal) [nao verificado]

2. **Não há colisão necessária, mas há instrumentos e requisitos a preservar.** Já existe leitor de `chamadas.log` contando `ini` por papel nas 24 h e compondo a mesma linha Codex; a extensão deve aproveitar esse produtor, preservando suas medidas. (`bin/codex_limite_estado.py:9-13,49-79,235-249`)

   `CODEX-ESTADO.md` é outra saída: respostas finais e procedência; seu debounce regula publicação, não ociosidade. O placar exigido no item 6 de CODEX também mede outro universo: itens encaminhados ao Codex versus opus/sonnet, motivos dos desvios e descartes. Chamadas por hora não o substituem. (`bin/gerar_codex_estado.py:138-156`; `bin/codex_estado.sh:6-10`; `CLAUDE.md:389-391`)

   Nos produtores conferidos, não há o maior intervalo solicitado: `ociosa_h` mede espera entre negativa de censo e aprovação; o outro alarme mede a sessão **Code** pelo transcript Claude e painel. Ausência absoluta de outro produtor: **[nao verificado]**. (`bin/disjuntor.py:2083-2084,2145-2150`; `bin/alarme_sessao_ociosa.py:18-22,151-152`)

   O contrato exige cinco linhas antes de construir. O anexo não explicita falha/limite nem quem desliga; contrato separado existente: **[nao verificado]**. (`app/docs/PISTA.md:170-176`; `complemento1_queima_util_1711.txt:13-14`, literal)

3. **O literal não fecha estas condições:**

   - **Qual intervalo:** distância entre `ini` consecutivos ou tempo sem chamada em curso? São medidas diferentes, e `ini/fim` delimitam o wrapper, incluindo espera. Nenhuma delas, isoladamente, comprova preenchimento de toda vaga livre. (`bin/codex_pergunta.sh:308-309,364-392`; `complemento1_queima_util_1711.txt:2,13`, literal)
   - **Qual janela:** faltam definir recorte nas últimas 24 h, bordas até “agora”, ausência de inícios e chamadas atravessando a borda. “Por hora” também não define os agrupamentos horários. (`complemento1_queima_util_1711.txt:13`, literal; `CLAUDE.md:35-38`)
   - **Reserva em qual instante:** seis ABERTOS no retrato fornecido não demonstram reserva disponível durante um buraco anterior. O literal não escolhe entre reserva atual e reserva existente durante o intervalo. (`fila.md:5-10`, literal; `complemento1_queima_util_1711.txt:13`, literal)
   - **Mordida e limites:** buraco de 2 h com cinco ABERTOS não distingue as duas interpretações sem definir chamadas em curso e momento da reserva. Há ainda cortes vigentes de orçamento que podem impedir chamadas; o contador não pode revogá-los implicitamente. (`complemento1_queima_util_1711.txt:13,15`, literal; `bin/disjuntor.py:1468-1485`)

4. **O escopo declarado não toca dinheiro nem gravado exportado:** pede leitura de log/fila e publicação de contador. Implementação futura: **[nao verificado]**. Publicar ESTADO no repositório de relato já integra o fluxo; isso é distinto de alterar competência exportada. Permanecem os limites de `!` e o pouso próprio de instrumento. (`complemento1_queima_util_1711.txt:13`, literal; `bin/relato.sh:114-117,145-147`; `CLAUDE.md:265-268,327,411-413`)

**Pergunta exata a devolver:** “No item 5, ‘maior intervalo sem chamada’ mede distância entre `ini` do wrapper ou tempo sem chamada em curso? Como tratar esperas/recusas, bordas das 24 h, ausência de registros e reserva: no instante da publicação ou durante o intervalo? Complete o contrato com falha/limite e quem desliga, preservando os cortes vigentes.” (`bin/codex_pergunta.sh:308-309,364-394`; `app/docs/PISTA.md:170-176`; `bin/disjuntor.py:1468-1485`)

VEREDITO: item 5 PROCEDE — medida ambígua e contrato incompleto no literal; definir antes de construir. (`CLAUDE.md:35-38`; `app/docs/PISTA.md:170-176`; `complemento1_queima_util_1711.txt:13`, literal)

## 17/11 08:03 · sessao 4247ffb62d5e · QUEIMA-UTIL-1711 · leitura · origem=Code

**SIM: os dois caminhos podem consultar K6 no escritor, sem juiz novo.** O corte DESLIGADO-NAO-TRAVA já dispensa a retenção da folha por leitura; não exige carimbar a pergunta para liberar a folha. A guarda de entrega existe, mas os dois escritores examinados não a aplicam antes desses efeitos. (`app/core/juizes.py:343`; `app/chamados/services/fio.py:1357`; `app/folha/export.py:1207`; `app/chamados/reconciliador.py:520`; `app/chamados/models.py:2009`.)

**Ressalva de alcance:** K6 responde “existe exportação vigente?”, não constitui sozinho uma proibição de toda escrita. L-092 protege o gravado; L-113 altera L-092 e revoga a recusa genérica de recálculo em exportada. Portanto, a conclusão acima aplica a cerca de escrita em ENTREGUE determinada neste item, sem transformar L-092 em veto universal a qualquer entidade. (`app/folha/export.py:556`; `app/docs/LEIS.md:125`; `app/docs/LEIS.md:155`.)

| Caminho / estado de partida | Juiz consultado | Operação | O que persiste — campo:arquivo:linha | Retorno | Prova existente |
|---|---|---|---|---|---|
| **(i)** Evento de célula leva à retratação; meia-resposta ENTREGUE deixou de reter na B2. O consumidor alcança `_retratar` por absolvição da célula ou premissa morta. (`app/chamados/reconciliador.py:1371`, `app/chamados/reconciliador.py:1375`, `app/chamados/reconciliador.py:1006`, `app/chamados/reconciliador.py:1467`.) | Retenção → `sem_veredito_entre` → C21; depois motor/premissa/conversa do pai/C6. **Sem veto K6 nesta unidade.** A guarda de vivas do chamado exclui entregues do bloqueio. (`app/chamados/juizes.py:2117`; `app/chamados/models.py:1933`; `app/chamados/models.py:664`; `app/chamados/models.py:364`.) | `_retratar` transiciona o chamado, rejeita justificativa pendente, resolve perguntas e acrescenta ocorrência. (`app/chamados/reconciliador.py:525`; `app/chamados/reconciliador.py:531`; `app/chamados/reconciliador.py:538`.) | Chamado: `status_local/resolvido_em/via_encerramento/atualizado_em/urgencias`:`app/chamados/models.py:228`. Justificativa: `status/descricao`:`app/chamados/reconciliador.py:534`. Perguntas elegíveis: `materializada_em/por/via_resolucao`:`app/chamados/models.py:3220`; lavra:`app/chamados/models.py:3270`. Ocorrência:`app/chamados/reconciliador.py:540`. | `False` se retido ou transição recusada; `True` ao completar. (`app/chamados/reconciliador.py:523`; `app/chamados/reconciliador.py:526`; `app/chamados/reconciliador.py:543`.) | Consumidor/idempotência: `app/chamados/tests/test_c20_juiz3_outbox.py:270` e `app/chamados/tests/test_c20_juiz3_outbox.py:299`. Saída da meia-resposta da retenção: `app/chamados/tests/test_f1nova_b2_leitores.py:61`. **Barreira ENTREGUE nesse caminho: sem teste identificado [nao verificado].** |
| **(ii)** Disputa aberta; meia-resposta não integra mais `respostas_sem_veredito`; existem perguntas vivas, inclusive ENTREGUE. (`app/chamados/models.py:1933`; `app/chamados/models.py:1986`.) | `respostas_sem_veredito` e C6 para selecionar vivas. **Sem K6.** `fechar_por_admin` delega ao carimbo de identidade, cujo julgamento pertence ao chamador. (`app/chamados/models.py:1977`; `app/chamados/models.py:1987`; `app/chamados/models.py:3185`; `app/chamados/models.py:3289`.) | `fechar(FECHOU_DESLIGAMENTO)` carimba cada viva e fecha a disputa. (`app/chamados/models.py:2009`; `app/chamados/models.py:2023`.) | Pergunta: `materializada_em/por/via_resolucao=admin_decidiu`:`app/chamados/models.py:3306`. Decisão: `SILENCIO`, instante, autor, texto e motivo:`app/chamados/models.py:2015`. Auditoria:`app/chamados/models.py:2018`. Disputa: `fechada_em/por/resolucao/motivo_fechamento`:`app/chamados/models.py:2023`. | `True` ao completar; repetição idêntica retorna `False`; resposta completa sem veredito causa `ValueError`. (`app/chamados/models.py:2032`; `app/chamados/models.py:1970`; `app/chamados/models.py:1977`.) | Desligamento normal, carimbo/trilha e resposta completa segurando: `app/chamados/tests/test_c7_red_minimos.py:209`, `app/chamados/tests/test_c7_red_minimos.py:221`, `app/chamados/tests/test_c7_red_minimos.py:234`. **ENTREGUE + meia-resposta: sem teste identificado [nao verificado].** |

No caminho **(i)**, “retratar o chamado” não significa necessariamente carimbar aquela pergunta: o resolver ainda consulta C6, tranca e C21. Porém a transição já dispara S143 e pode relavrar perguntas antes da chamada explícita ao resolver; por isso a cerca precisa anteceder a transição. (`app/chamados/services/disputa_emissao.py:1988`; `app/chamados/services/disputa_emissao.py:1992`; `app/chamados/services/disputa_emissao.py:2002`; `app/chamados/signals.py:117`; `app/chamados/models.py:309`.)

**Os outros escritores não consultam todos a mesma guarda.** Estes são os pontos de consulta existentes e seus recortes. (`app/chamados/services/materializacao.py:792`; `app/chamados/services/validacao.py:400`; `app/chamados/services/fio.py:1357`.)

| Escritor de `PerguntaDisputa` | Guarda de competência consultada |
|---|---|
| `validar_pergunta` | `entrega_que_barra_a_validacao`, antes da validação e novamente depois do plantio; delega a `_k6_que_barra`, **restrita às metades de intervalo fora163**. (`app/chamados/services/validacao.py:740`; `app/chamados/services/validacao.py:777`; `app/chamados/services/validacao.py:225`; `app/chamados/services/materializacao.py:792`.) |
| `_materializar_uma` | `_competencia_que_barra(..., somente_entregue=True)` antes dos efeitos; compõe K6 daquele recorte com a guarda dos instantes de plantio. (`app/chamados/services/materializacao.py:1854`; `app/chamados/services/materializacao.py:818`.) |
| `devolver_paridade_bug_par_103` | `competencia_do_fato`, sob trava; recusa entregue ou ausência do dia do fato. (`app/chamados/services/validacao.py:332`; `app/chamados/services/validacao.py:338`.) |
| `devolver_instante_sem_texto_l130` | `impedimento_estoque_l130` → `competencia_do_fato`; relê sob trava e recusa entregue. (`app/chamados/services/validacao.py:400`; `app/chamados/services/validacao.py:432`.) |
| Backfill de decisões | `fio.veto_de_competencia`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:194`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:256`.) |
| Migração do dia da pergunta | O mesmo veto, inclusive na reversão. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:305`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:488`.) |
| `rechavear_no_reuso` | `veto_de_competencia` sobre dia gravado e dia novo; competência desconhecida também recusa. (`app/chamados/services/fio.py:1116`.) |
| `responder_pergunta_disputa_core` | Sem K6 próprio no corpo da escrita. (`app/chamados/services/disputa_emissao.py:1481`; `app/chamados/services/disputa_emissao.py:1522`.) |
| `responder_automaticamente` | Sem K6 próprio; verifica texto anterior e integridade do par. (`app/chamados/services/disputa_emissao.py:1663`; `app/chamados/services/disputa_emissao.py:1672`.) |
| `carimbar_resposta_pela_trilha` | Sem K6 próprio; exige texto e instante ausente. (`app/chamados/services/disputa_emissao.py:1692`.) |
| `reabrir_para_correcao` | Sem K6 próprio; limpa resposta/validação pela porta de lote. (`app/chamados/services/disputa_emissao.py:1609`; `app/chamados/services/disputa_emissao.py:1648`.) |
| `ressuscitar_perguntas` | Sem K6 próprio; universo por C6/marco e escrita pelo lote. (`app/chamados/models.py:1527`; `app/chamados/models.py:1564`; `app/chamados/models.py:1639`.) |
| `remover_motivo` | Sem K6 próprio; carimba identidade retratada. (`app/chamados/models.py:2122`; `app/chamados/models.py:2152`.) |
| `carimbar` | Sem K6 próprio; C6 julga o estado após aplicação do carimbo. (`app/chamados/models.py:3260`; `app/chamados/models.py:3273`.) |
| `carimbar_identidade` / `fechar_por_admin` | Sem K6 próprio; a identidade é julgada pelo chamador. (`app/chamados/models.py:3289`; `app/chamados/models.py:3185`.) |
| `descarimbar` / `descarimbar_em_lote` | Sem K6 próprio; proteções de fora do fato, integridade do par e trava. (`app/chamados/models.py:3324`; `app/chamados/models.py:3381`; `app/chamados/models.py:3402`.) |
| `recusar_pergunta` | Sem K6 próprio; consulta C21 e delega à reabertura. (`app/chamados/services/validacao.py:901`; `app/chamados/services/validacao.py:925`.) |
| `devolver_validacao_recusada` | Sem K6 próprio; o chamador `validar_pergunta` faz a consulta de entrega. (`app/chamados/services/validacao.py:264`; `app/chamados/services/validacao.py:777`.) |
| `encerrar_por_fato_no_espelho` / `reabrir_por_resposta` / `reabrir_por_competencia` | Sem K6 próprio nessas portas. (`app/chamados/models.py:3406`; `app/chamados/models.py:3439`; `app/chamados/models.py:3455`.) |
| `relavra_via.relavrar` | Sem K6 próprio; altera via e relavra. (`app/chamados/services/relavra_via.py:106`; `app/chamados/services/relavra_via.py:127`.) |

O motivo verificável de `_retratar` passar é este: sua retenção pergunta por resposta sem veredito; C21 exige instante **e** texto, então meia-resposta não segura. A consulta posterior às vivas não substitui K6 e ainda exclui entregues do bloqueio do chamado. Uma justificativa histórica para a ausência dessa cerca é **[nao verificado]**. (`app/chamados/reconciliador.py:520`; `app/chamados/models.py:1933`; `app/chamados/juizes.py:1552`; `app/chamados/models.py:364`.)

**O menor conserto proposto**, ainda sem implementação ou execução de teste **[nao verificado]**, é: [nao verificado]

| Sítio | Guarda existente a consultar | O que deixa de gravar quando houver ENTREGUE |
|---|---|---|
| `_retratar`, dentro da unidade e antes de `transicionar_com_pauta` | `fio.veto_de_competencia`, alimentado pelos dias do fato das perguntas alcançadas, incluindo os efeitos dos receivers e da relavra. A guarda já compõe dia → janela da empresa → K6; não copiar `_k6_que_barra`, cujo recorte é fora163. (`app/chamados/reconciliador.py:524`; `app/chamados/services/fio.py:1357`; `app/chamados/services/materializacao.py:792`.) | Retorna `False` antes de alterar estado do chamado, justificativa, carimbos, lavras e ocorrência. Barrar só o carimbo seria tarde: o save do chamado já dispara S143 e relavra. (`app/chamados/reconciliador.py:525`; `app/chamados/reconciliador.py:531`; `app/chamados/signals.py:117`; `app/chamados/models.py:309`.) |
| `fechar`, sob a trava já existente e antes do bloco `FECHOU_DESLIGAMENTO` | O mesmo veto, sobre os dias do fato das vivas que seriam carimbadas; recusa a unidade antes da primeira escrita. (`app/chamados/models.py:2001`; `app/chamados/models.py:2009`; `app/chamados/services/fio.py:1357`.) | Preserva perguntas e disputa: sem trio do carimbo, `SILENCIO`, auditoria de encerramento e fechamento da disputa. Fazer apenas `fechar_por_admin` retornar `False` não basta: o laço continuaria até salvar a disputa. (`app/chamados/models.py:2012`; `app/chamados/models.py:2015`; `app/chamados/models.py:2023`.) |

A suficiência da seleção de todas as perguntas indiretas, especialmente no primeiro caminho, ainda precisa de prova específica **[nao verificado]**. Os universos atuais do resolver e da relavra não são idênticos: o primeiro seleciona destinos não materializados em disputa aberta; a segunda alcança perguntas do destino não validadas. (`app/chamados/services/disputa_emissao.py:1928`; `app/chamados/models.py:309`.)

**DESLIGADO-NAO-TRAVA já cobre a liberação da folha.** O corte registra a unidade literal “dia-colab de colaborador com situacao desligado” e declara `esta_desligado` como autoridade. (`app/docs/CORTES.md:440`.) O texto transcrito no retentor é:

> “colaborador desligado NAO retem a folha em NENHUM dia da competencia -- nem antes nem depois da saida. Dias sem batida antes da saida nao travam: aparecem como INFORMATIVO para o DP (rescisao), nunca como trava nem na lista do supervisor”. (`app/folha/export.py:1197`.)

Isso é implementado pelo retorno vazio do retentor e pelo informativo separado, que lê os mesmos dias. **Esse corte não autoriza o carimbo da pergunta ENTREGUE**: libera a folha por leitura e declara o informativo sem alteração de célula/batida. (`app/folha/export.py:1207`; `app/folha/export.py:1216`; `app/folha/export.py:1223`.)

**Toca `FechamentoMensal` exportado? NÃO diretamente no fechamento/carimbo.** Os saves desse bloco escrevem Pergunta e Disputa. **Não é possível afirmar “não” para todos os efeitos indiretos:** o fechamento agenda retomada de regularizações confirmadas; elas podem registrar batidas e acionar recálculo, inclusive em exportada. Existência dessas regularizações nas disputas do anexo e alteração efetiva de `FechamentoMensal`: **[nao verificado]**. (`app/chamados/models.py:3307`; `app/chamados/models.py:2027`; `app/chamados/signals.py:122`; `app/chamados/services/regularizacao_ext.py:219`; `app/chamados/services/regularizacao_ext.py:77`; `app/ponto/registro_batida.py:195`; `app/ponto/services/fechamento.py:1790`; `app/ponto/services/fechamento.py:1809`.)

**Cabe tecnicamente como COMPLEMENTO da B2**, conservando C21 e acrescentando a consulta de competência nos escritores, sem declarar autoridade nova. `models.py` é arquivo comum demonstrável pelas marcações explícitas da B2 e pelo sítio de `fechar`. A interseção **formal com o diff histórico completo**, inclusive se `reconciliador.py` fazia parte dele, é **[nao verificado]**: não foi disponibilizado um diff B2 verificável nesta cópia. (`app/chamados/models.py:1928`; `app/chamados/models.py:3677`; `app/chamados/models.py:2009`; `app/core/juizes.py:343`.)

**Quem escreve e quem lê os campos atingidos:**

| Campos | Escritor neste caminho | Leitor identificado |
|---|---|---|
| Pergunta: `materializada_em`, `via_resolucao` | `_escrever_carimbo`; persistência por `carimbar` ou `carimbar_identidade`. (`app/chamados/models.py:3218`; `app/chamados/models.py:3273`; `app/chamados/models.py:3311`.) | C6 para vitalidade; lista sem veredito; histórico. (`app/chamados/juizes.py:1443`; `app/chamados/models.py:1934`; `app/chamados/services/historico.py:103`.) |
| Pergunta: `materializada_por` | Mesmo escritor do carimbo. (`app/chamados/models.py:3221`.) | Preservação/restauração da autoria ao manter carimbo de identidade. Outros leitores de apresentação: **[nao verificado]**. (`app/chamados/models.py:3304`.) |
| Pergunta: `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `veredito_lavrado.lavrar`, acionado pelo carimbo ou pela transição do destino. (`app/chamados/models.py:3270`; `app/chamados/models.py:312`; `app/chamados/veredito_lavrado.py:270`.) | `esta_stale` lê lavra, instante e expiração. (`app/chamados/veredito_lavrado.py:224`; `app/chamados/veredito_lavrado.py:228`.) |
| Pergunta: `respondida_em/resposta_colab`, eventual validação | No resolver, preenchimento de instante/validação exige texto; a meia-resposta não ganha esses preenchimentos. (`app/chamados/models.py:3251`.) | C21, lista sem veredito e histórico. (`app/chamados/juizes.py:1552`; `app/chamados/models.py:1933`; `app/chamados/services/historico.py:99`.) |
| Decisão: pergunta, `ato=SILENCIO`, `ato_em/por/texto/motivo` | `DecisaoPergunta.registrar`, chamado por `fechar`. (`app/chamados/models.py:2015`; `app/chamados/models.py:4124`.) | Sequência/histórico da pergunta e projeção temporal. (`app/chamados/models.py:4027`; `app/chamados/juizes.py:1389`.) |
| Disputa: `fechada_em/por/resolucao/motivo_fechamento` | `fechar`. (`app/chamados/models.py:2023`.) | `aberta`; idempotência do próprio fechamento; signal do pai; S144 e retomada de regularizações. (`app/chamados/models.py:1898`; `app/chamados/models.py:1971`; `app/chamados/signals.py:29`; `app/chamados/signals.py:165`; `app/chamados/signals.py:138`.) |
| Chamado: `status_local/resolvido_em/via_encerramento/atualizado_em` | `_gravar_estado`. (`app/chamados/models.py:228`; `app/chamados/models.py:296`.) | Crédito/retenção por estado; painel por resolução; resolver por via. Leitor específico de `atualizado_em` neste recorte: **[nao verificado]**. (`app/chamados/juizes.py:2115`; `app/chamados/services/painel.py:245`; `app/chamados/services/disputa_emissao.py:1923`.) |
| Chamado: `urgencia/urgencia_original` | Rebaixamento da transição terminal. (`app/chamados/models.py:246`; `app/chamados/models.py:471`.) | Restauração na reabertura. (`app/chamados/models.py:250`.) |
| Justificativa: `status/descricao` | `_retratar`. (`app/chamados/reconciliador.py:534`.) | Portas DP leem status; o save acorda cartório. Leitor específico da descrição acrescentada: **[nao verificado]**. (`app/ponto/services/justificativa.py:143`; `app/ponto/services/justificativa.py:278`; `app/ponto/signals.py:161`.) |
| Ocorrência: chamado, autor, conteúdo, anexo | `nota.anexar_ocorrencia`. (`app/chamados/services/nota.py:88`; `app/chamados/services/nota.py:103`.) | Fio e histórico. (`app/chamados/views.py:372`; `app/chamados/services/historico.py:80`.) |
| Auditoria: ator, descrição, antes/depois, instante | `evento` → `LogAuditoria`; gravação é best effort. (`app/core/observ.py:94`; `app/core/observ.py:134`; `app/core/models.py:307`.) | Histórico; retenção lê sua última trilha para deduplicar. (`app/chamados/services/historico.py:86`; `app/chamados/reconciliador.py:480`.) |
| Outbox da célula | Save da pergunta e registro da decisão emitem evento. (`app/chamados/signals.py:252`; `app/chamados/models.py:4134`.) | `consumir_eventos_celula`. (`app/chamados/reconciliador.py:1371`.) |

A retenção é lista calculada, não campo a limpar. A trilha `perguntas_pendentes` guarda apenas os primeiros oito PKs e não substitui a lista integral. (`app/chamados/juizes.py:2115`; `app/chamados/reconciliador.py:470`.)

As **111 abaixo são as meia-respostas que seguravam**, não as 172 perguntas ENTREGUE que o fechamento alcançaria. O anexo informa 12 colaboradores, 14 chamados e competência 09/2026; na B2 deixam de segurar a/b/c. (`anexo lista.md:10`; `anexo lista.md:12`; `anexo lista.md:15`.) Nas referências seguintes, a linha conta o texto do anexo fornecido, incluindo linhas vazias; não representa remedição de produção **[nao verificado]**.

```text
pk, colNNN, competencia, chamado retido

28357, col592, 09/2026, 19023 — anexo lista.md:21
28443, col869, 09/2026, 18009 — anexo lista.md:22
29114, col607, 09/2026, 19013 — anexo lista.md:23
29167, col191, 09/2026, 18856 — anexo lista.md:24
29168, col191, 09/2026, 18856 — anexo lista.md:25
29169, col191, 09/2026, 18856 — anexo lista.md:26
29270, col869, 09/2026, 18009 — anexo lista.md:27
29502, col349, 09/2026, 18513 — anexo lista.md:28
29503, col349, 09/2026, 18513 — anexo lista.md:29
29872, col446, 09/2026, 13504 — anexo lista.md:30
31334, col607, 09/2026, 19013 — anexo lista.md:31
31335, col607, 09/2026, 19013 — anexo lista.md:32
31472, col903, 09/2026, 18606 — anexo lista.md:33
31473, col903, 09/2026, 18606 — anexo lista.md:34
31476, col903, 09/2026, 23072 — anexo lista.md:35
31477, col903, 09/2026, 23072 — anexo lista.md:36
31584, col204, 09/2026, 9535 — anexo lista.md:37
31585, col204, 09/2026, 9535 — anexo lista.md:38
31586, col204, 09/2026, 33525 — anexo lista.md:39
31587, col204, 09/2026, 33525 — anexo lista.md:40
31708, col592, 09/2026, 19023 — anexo lista.md:41
31709, col592, 09/2026, 19023 — anexo lista.md:42
31710, col592, 09/2026, 19023 — anexo lista.md:43
31711, col592, 09/2026, 19023 — anexo lista.md:44
31712, col592, 09/2026, 19023 — anexo lista.md:45
31713, col592, 09/2026, 19023 — anexo lista.md:46
31714, col592, 09/2026, 19023 — anexo lista.md:47
31715, col592, 09/2026, 19023 — anexo lista.md:48
31716, col592, 09/2026, 19023 — anexo lista.md:49
31717, col592, 09/2026, 19023 — anexo lista.md:50
31724, col191, 09/2026, 18856 — anexo lista.md:51
31725, col191, 09/2026, 18856 — anexo lista.md:52
31726, col191, 09/2026, 18856 — anexo lista.md:53
31727, col191, 09/2026, 18856 — anexo lista.md:54
31728, col191, 09/2026, 18856 — anexo lista.md:55
31729, col191, 09/2026, 18856 — anexo lista.md:56
31730, col191, 09/2026, 18856 — anexo lista.md:57
31731, col191, 09/2026, 18856 — anexo lista.md:58
31839, col789, 09/2026, 18472 — anexo lis

[truncado em 20000 de 23638 caracteres]

## 17/11 08:03 · sessao 081ccfce41cb · QUEIMA-UTIL-1711 · leitura · origem=Code

| Obra | GRADE: linha e estado atual | RELATO: registro divergente | Estado que deveria constar |
|---|---|---|---|
| BUG-ENC-CELULAS-FORMA | app/docs/GRADE-MESTRA.md:50 — INTERROMPIDA | app/docs/RELATO.md:256 — “NO AR (7825fec5b)” | NO AR 7825fec5b |
| FIO-E0-V4-E1-ENTRADAS | app/docs/GRADE-MESTRA.md:106 — PARADA | app/docs/RELATO.md:284 — “FECHADA”, núcleo a8a692f55; complementos separados | FECHADA; complementos em linhas próprias |
| REABRIR-NAO-DESTROI | app/docs/GRADE-MESTRA.md:176 — REGISTRADA, falta smoke | app/docs/RELATO.md:129 — “REABRIR-NAO-DESTROI FECHADA” | FECHADA pelo CORTE A de 16/11 |
| BUG-ANCORA-NAO-REGENERA | app/docs/GRADE-MESTRA.md:178 — PARADA | app/docs/RELATO.md:133 — “O99 e BUG-ANCORA-NAO-REGENERA FECHADAS” | FECHADA; pendências de cadastro encaminhadas ao administrador |
| O99 | app/docs/GRADE-MESTRA.md:190 — PARADA | app/docs/RELATO.md:133 — “O99 e BUG-ANCORA-NAO-REGENERA FECHADAS” | FECHADA pelo CORTE B de 16/11 |
| BO-HE-RECUSAR-LOTE-500 | app/docs/GRADE-MESTRA.md:242 — REGISTRADA, smoke pendente | app/docs/RELATO.md:129 — “BO-HE-RECUSAR-LOTE-500 FECHADA” | FECHADA pelo CORTE A de 16/11 |
| BO-0611-05 | app/docs/GRADE-MESTRA.md:270 — PARADA | app/docs/RELATO.md:280 — “BO-0611-05 esta NO AR (fb9269ea9)” | NO AR fb9269ea9 |
| CONTRATO-DE-ENTRADA-DO-FIO | app/docs/GRADE-MESTRA.md:689 — BRIEF PRONTO | app/docs/RELATO.md:475 — “NO AR … CEF 77dfa94fa”, deploy dos dois registrado | NO AR 77dfa94fa |
| BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS | app/docs/GRADE-MESTRA.md:1024 — EM VOO | app/docs/RELATO.md:32 — c4+c4c POUSO e deploy; relato_delta_vivo.diff: `+ESTADO 07:5x SMOKES-[nome]-1711 item 2: BUG-PAR c4 FECHADA` | NO AR; c4+c4c pousada e smoke c4 fechado |
| DISPUTA-ZUMBI-1110 | app/docs/GRADE-MESTRA.md:1029 — REGISTRADA | app/docs/RELATO.md:285 — “ESTACIONADA 20:1x”; :476 — cura das 18 aplicada, contador incompleto | PARADA; cura das 18 aplicada, contador estacionado até esgotar a lista ou novo aval |
| O10-D-PERMUTA-RECIPROCA | app/docs/GRADE-MESTRA.md:1422 — REGISTRADA | relato_delta_vivo.diff: `+ESTADO 08:2x O10-D 2a leitura opus APROVA a arvore bdbff7c4a; pouso so com corte… e o ! de escala` | APROVADA bdbff7c4a; pouso aguarda cortes pendentes e ! de escala |
| FATO-DE-AUSENCIA-ACORDA-A-PROJECAO / LOTE7 | app/docs/GRADE-MESTRA.md:1765 — PARADA, c6 anunciada | app/docs/RELATO.md:162 — “LOTE7 c7 NO AR” | NO AR c7 |
| BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS | app/docs/GRADE-MESTRA.md:1780 — PARADA, leis do predesenho | app/docs/RELATO.md:32 — c4+c4c POUSO e deploy; relato_delta_vivo.diff: `+ESTADO 07:5x SMOKES-[nome]-1711 item 2: BUG-PAR c4 FECHADA` | NO AR; c4+c4c pousada e smoke c4 fechado |
| E4C-F2 | app/docs/GRADE-MESTRA.md:1789 — PARADA, matriz e leis | relato_delta_vivo.diff: `+DESPACHO 07:5x CORTE-B2-MEIA-RESPOSTA-ENTREGUE item 5: F2 construtor -> opus` | EM CURSO; construtor opus em cópia de 708162b15 |
| BUG-ENC-CELULAS-FORMA | app/docs/GRADE-MESTRA.md:1799 — EM VOO, opus final | app/docs/RELATO.md:256 — “NO AR (7825fec5b)” | NO AR 7825fec5b |
| E3-RESTO-R3 | app/docs/GRADE-MESTRA.md:1800 — EM VOO, c17 retida | app/docs/RELATO.md:235 — pouso c17; :80 — “E3-R3 c17 POUSADA” | POUSADA c17 |
| E4C-F1-NOVA-JUIZ-RESPONDIDA | app/docs/GRADE-MESTRA.md:1801 — EM VOO, B1 no anexo | app/docs/RELATO.md:174 — B1 NO AR; relato_delta_vivo.diff: `+ESTADO 04:1x E4C-F1-B2 … APROVA … 708162b15; o pouso segue no ! das 2 PAREI` | B1 NO AR; B2 APROVADA 708162b15, pouso retido pelas duas PAREI |
| HE-REVOGADA-NA-PORTA | app/docs/GRADE-MESTRA.md:1803 — EM VOO, pre-opus | app/docs/RELATO.md:259 — “NO AR 271ecba2e” | NO AR 271ecba2e |
| SITUACAO-DESLIGADO / LOTE 3 desligado | app/docs/GRADE-MESTRA.md:1804 — EM VOO, c6 na fila | app/docs/RELATO.md:184 — “NO AR 10:58 LOTE3 c7” | NO AR c7 be4a6219e |

**Total: 19 linhas divergentes, correspondentes a 17 obras.**

## 17/11 08:03 · sessao c67ac0790712 · - · - · origem=Ronald (tmux)

- `relato.sh` já gera `ESTADO.md` via `gerar_estado.py` (`bin/relato.sh:37-38`); a linha Codex vem de `codex_limite_estado.ler/linha` (`bin/gerar_estado.py:267-270`), na seção “Codex (limite)” (`bin/gerar_estado.py:337-339`).
- O leitor atual conta `ini` nas últimas 24 h por papel (`bin/codex_limite_estado.py:71-79`), mostra últimas cinco chamadas e “em curso” sem `fim` (`bin/codex_limite_estado.py:82-89`), além de limite e uso semanal (`bin/codex_limite_estado.py:235-243`). Não calcula maior intervalo ou reserva nessa composição (`bin/codex_limite_estado.py:235-249`).
- O próprio instrumento declara acrescentar leituras à **mesma linha**, sem contador paralelo (`bin/codex_limite_estado.py:9-13`); `gerar_codex_estado.py` identifica esse módulo como leitor único de `chamadas.log` (`bin/gerar_codex_estado.py:41`).
- `CODEX-ESTADO.md` é outra saída: respostas finais, datas e procedência (`bin/gerar_codex_estado.py:138-156`); seu produtor único é `codex_estado.sh`, compartilhado entre evento e rede do relato (`bin/relato.sh:89-97`). O debounce de 30 s regula pushes (`bin/codex_estado.sh:6-10`), não mede ociosidade.
- Já existe `ociosa_h`, mas mede união de esperas da primeira negativa por censo até aprovação da mesma obra, ou agora, recortadas em 24 h (`bin/disjuntor.py:2083-2084`, `bin/disjuntor.py:2114-2122`, `bin/disjuntor.py:2145-2150`). Seu escopo inclui aberturas de Agent e wrapper construtor (`bin/disjuntor.py:2077-2082`), não maior intervalo sem chamadas Codex.
- Outro alarme mede ociosidade da sessão Code por transcript Claude e painel tmux (`bin/alarme_sessao_ociosa.py:18-22`, `bin/alarme_sessao_ociosa.py:144-152`).
- Consumidor implementado de `logs/CODEX-RESERVA/fila.md` não localizado na busca literal pela cópia [nao verificado]. As referências encontradas com “CODEX-RESERVA” são o corte histórico “ATE-O-RESET”, distinto da nova fila (`app/docs/PROMPTS.md:1781`, `app/docs/RELATO.md:2744`).
- Ausência absoluta de qualquer outro medidor de intervalo/ociosidade Codex permanece [nao verificado]; nos produtores identificados, a composição publicada está delimitada pelas funções citadas (`bin/codex_limite_estado.py:235-249`, `bin/gerar_codex_estado.py:132-159`).

## 17/11 08:03 · sessao 7feec77990fb · QUEIMA-UTIL-1711 · leitura · origem=Code

**P1 — Ressuscitar uma pergunta deveria devolvê-la ao colaborador?**

**Hoje:** o sistema limpa a resposta inteira, retira os carimbos de validação e encerramento, conserva o anterior na trilha e reabre a disputa. Mas **não registra uma devolução ao colaborador**. Por isso, se o último ato continua sendo SILENCIO, a tarefa continua sem pedir ação. (`app/chamados/models.py:1613`; `app/chamados/models.py:1623`; `app/chamados/models.py:1629`; `app/chamados/models.py:1638`; `app/chamados/services/tarefa_do_dia.py:176`.)

Há uma diferença importante na apresentação: a pergunta viva, sem instante de resposta e com disputa aberta, entra nas pendentes do app. Portanto, **aparecer como pendente e ter uma cobrança atribuída ao colaborador não são hoje a mesma decisão**. (`app/chamados/services/pergunta_no_app.py:61`; `app/chamados/services/pergunta_no_app.py:81`; `app/chamados/services/tarefa_do_dia.py:176`.)

| Opção | O que significa para a empresa | Lei existente | Quantas perguntas |
|---|---|---|---|
| **A — Ressuscitar sem criar nova cobrança ao colaborador** | A pergunta volta a poder ser tratada; ressuscitar, sozinho, não muda quem deve agir. | É a conservação determinada para a B1. A L-130 manda limpar a resposta, mas declara a devolução ainda **não decidida**. (`app/docs/RELATO.md:188`; `app/docs/LEIS.md:151`.) | Candidatas atuais à ressuscitação, conservadas sem nova devolução: **[a medir]**. |
| **B — Toda ressuscitação passa a pedir resposta ao colaborador** | Ressuscitar significa também dizer: “precisamos que você responda novamente”. | REABERTURA já produz essa cobrança na tarefa; aplicá-la à ressuscitação depende da decisão pendente. Limpar a resposta, pela L-130, não autoriza sozinho mudar o responsável. (`app/chamados/services/tarefa_do_dia.py:163`; `app/docs/LEIS.md:151`; `app/docs/RELATO.md:191`.) | As mesmas candidatas, agora com devolução explícita: **[a medir]**. |

**O caso ligado à F5 tem uma decisão vigente própria:** resposta vazia segue fora do app, com ação do administrador. Devolver também esse estoque ao colaborador exigiria dizer expressamente que o corte mudou. Há ainda um desacordo no código: último ato RESPOSTA vazia continua atribuindo “responder” ao colaborador. A F6 deve herdar o responsável da tarefa, sem criar outra tabela de responsáveis. (`app/docs/CORTES.md:488`, itens 5–6; `app/chamados/services/tarefa_do_dia.py:159`.)

**P2 — O último ato com texto diferente deveria substituir o texto atual?**

**Hoje:** há um texto corrente na pergunta e textos guardados no histórico. Os leitores da F8 ainda consultam o texto corrente: por exemplo, calendário, prévia da folha e resumo do dia. Ao registrar SILENCIO porque o administrador decidiu o dia, o sistema também copia esse texto corrente. O juiz de “respondida” exige instante e texto corrente não vazio; ele não busca o texto no histórico. (`app/colaboradores/services/calendario.py:504`; `app/folha/previa.py:142`; `app/ponto/services/dia_do_colab.py:70`; `app/ponto/services/veredito_celula.py:197`; `app/chamados/juizes.py:1552`.)

| Opção | O que significa para a empresa | Lei existente | Quantas perguntas |
|---|---|---|---|
| **A — Conservar o texto corrente** | Uma diferença no histórico não troca automaticamente o texto usado pelos leitores e pelo próximo SILENCIO. | É o comportamento atual, compatível com a limpeza exigida pela L-130. Isso **não prova que toda diferença esteja correta**: o código atual não é o gabarito da regra. (`app/ponto/services/veredito_celula.py:197`; `app/docs/LEIS.md:151`; `app/docs/LEIS.md:136`.) | Perguntas divergentes cujo texto seria conservado: **[a medir]**. |
| **B — O texto do último ato vence, inclusive quando vazio** | Os leitores e o próximo SILENCIO passam a usar o texto do último registro. | A L-002 exige uma autoridade comum para os leitores, mas não escolhe qual texto vence. A L-130 continua proibindo resposta pela metade e exige apagar a resposta ao reabrir/ressuscitar. A substituição universal não está autorizada por esses dispositivos. (`app/docs/LEIS.md:60`; `app/docs/LEIS.md:151`; `CLAUDE.md:20`.) | As mesmas perguntas divergentes, candidatas à substituição, abertas por tipo de ato: **[a medir]**. |

**O limite da opção B:** um ato pode guardar a resposta que foi **retirada**, e não uma nova resposta. Copiar o texto de RECUSA ou REABERTURA pode recolocar aquilo que a L-130 manda apagar. Portanto, “último ato sempre vence” precisa distinguir o significado dos atos antes de virar implementação. (`app/chamados/models.py:4018`; `app/docs/LEIS.md:151`.)

As alternativas acima são perguntas para decisão, não novas leis aprovadas; a L-117 exige corte literal para criar lei. (`app/docs/LEIS.md:138`.)

A sonda abaixo está **não executada; resultados [a medir] [nao verificado]**. Para P1, mede o que a porta **poderia ressuscitar agora**, sem reconstruir ressuscitações passadas. Para P2, mede diferenças entre texto corrente e último ato, ordenado por instante e desempate pelo identificador. Esses são alcances de perguntas candidatas, não contagens de telas efetivamente alteradas. (`app/chamados/models.py:1501`; `app/chamados/models.py:4056`; `app/chamados/models.py:4020`.)

O estoque físico de vivas com instante e sem texto aparece separado e por último ato: ele não é automaticamente o conjunto “última RESPOSTA vazia no painel”. (`app/chamados/juizes.py:1531`; `app/chamados/services/tarefa_do_dia.py:159`.)

```python
from collections import Counter, defaultdict
from copy import copy
from django.db import connection, transaction
from django.utils import timezone
from django_tenants.utils import schema_context

from chamados.models import (
    PerguntaDisputa as P, DecisaoPergunta as D, ChamadoColaborador as C,
)
from chamados.juizes import (
    pergunta_viva, pergunta_respondida, respondida_sem_texto,
    texto_sem_carimbo,
)

# Django shell da SOMBRA, schema conforme o anexo.
# Fonte P1: app/chamados/models.py:1501; relacionamento: :3522.
# Fonte P2: app/chamados/models.py:4020; ordem: :4056.
# Juizes: app/chamados/juizes.py:1531,1537,1544.
# Somente leitura: nenhuma porta escritora e chamada abaixo.

n = Counter()
linhas = []
por_col = defaultdict(Counter)

with schema_context("juliani"), transaction.atomic():
    with connection.cursor() as cur:
        cur.execute(
            "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ READ ONLY"
        )

    agora = timezone.now()
    ps = list(P.objects.select_related(
        "disputa__colaborador__empresa", "disputa__chamado",
        "chamado_destino", "celula",
    ).order_by("pk"))
    por_pk = {p.pk: p for p in ps}

    ultimos = {}
    for d in D.objects.filter(ato_em__lte=agora).order_by(
        "pergunta_id", "ato_em", "pk"
    ).iterator():
        ultimos[d.pergunta_id] = d

    def tipo(pid):
        d = ultimos.get(pid)
        if d is None:
            return "sem_ato_ate_agora"
        return d.ato if d.ato in dict(D.ATOS) else "ato_desconhecido"

    u1 = set()
    destinos = {
        p.chamado_destino_id for p in ps
        if p.chamado_destino_id is not None
    }
    for c in C.objects.filter(pk__in=destinos).select_related(
        "colaborador__empresa"
    ).order_by("pk"):
        u1.update(c.perguntas_a_ressuscitar())

    vazias_vivas = {
        p.pk for p in ps
        if respondida_sem_texto(p) and pergunta_viva(p, agora=agora)
    }
    u2 = {
        p.pk for p in ps if p.pk in ultimos
        and (p.resposta_colab or "") !=
            (ultimos[p.pk].ato_texto or "")
    }

    alcances = {
        "P1_A_candidatas_sem_nova_devolucao": u1,
        "P1_B_candidatas_com_devolucao": u1,
        "P2_A_textos_divergentes_conservados": u2,
        "P2_B_textos_divergentes_substituidos": u2,
    }
    for rotulo, ids in alcances.items():
        n[rotulo] = len(ids)
        for pid in ids:
            por_col[por_pk[pid].colaborador_id][rotulo] += 1

    n["perguntas_total"] = len(ps)
    n["P2_sem_ato_ate_agora_conservar_nas_duas"] = sum(
        p.pk not in ultimos for p in ps
    )
    n["P1_anexo_F5_vivas_instante_sem_texto"] = len(vazias_vivas)
    n["P1_intersecao_ressuscitaveis_F5"] = len(u1 & vazias_vivas)

    for pid in u1:
        n["P1_ultimo_" + tipo(pid)] += 1
    for pid in vazias_vivas:
        n["F5_fisico_ultimo_" + tipo(pid)] += 1

    extras_p2 = (
        "P2_divergentes_apos_trim",
        "P2_divergentes_disputa_aberta",
        "P2_divergentes_com_validacao",
        "P2_divergentes_com_materializacao",
        "P2_B_resultaria_par_ilegal_se_trocar_so_texto",
        "P2_B_mudaria_juiz_respondida_se_trocar_so_texto",
        "P2_B_texto_nao_vazio_em_REABERTURA_ou_RECUSA",
    )
    n.update({k: 0 for k in extras_p2})

    for pid in u2:
        p, d = por_pk[pid], ultimos[pid]
        n["P2_ultimo_" + tipo(pid)] += 1
        n["P2_divergentes_apos_trim"] += int(
            (p.resposta_colab or "").strip() !=
            (d.ato_texto or "").strip()
        )
        n["P2_divergentes_disputa_aberta"] += int(
            p.disputa.fechada_em is None
        )
        n["P2_divergentes_com_validacao"] += int(
            p.validada_em is not None
        )
        n["P2_divergentes_com_materializacao"] += int(
            p.materializada_em is not None
        )

        # Hipotese B apenas em memoria; nao e um novo juiz aprovado.
        # Restricoes existentes: app/docs/LEIS.md:151.
        q = copy(p)
        q.resposta_colab = d.ato_texto or ""
        n["P2_B_resultaria_par_ilegal_se_trocar_so_texto"] += int(
            respondida_sem_texto(q) or texto_sem_carimbo(q)
        )
        n["P2_B_mudaria_juiz_respondida_se_trocar_so_texto"] += int(
            pergunta_respondida(p) != pergunta_respondida(q)
        )
        n["P2_B_texto_nao_vazio_em_REABERTURA_ou_RECUSA"] += int(
            d.ato in (D.REABERTURA, D.RECUSA)
            and bool((d.ato_texto or "").strip())
        )

    linhas.append("INSTANTE " + timezone.localtime(agora).isoformat())
    for rotulo in sorted(n):
        linhas.append(f"NUMERO {rotulo}: {n[rotulo]}")
    for cid in sorted(por_col):
        for rotulo in sorted(alcances):
            linhas.append(
                f"COL col{cid:03d} {rotulo}: {por_col[cid][rotulo]}"
            )

for linha in linhas:
    print(linha)
print("FIM_OK")
```

lei: RESSUSCITAR-DEVOLVE-AO-COLABORADOR -- hoje limpa sem registrar devolução; opcoes A conservar cobrança/B devolver ao colaborador; lei existente L-130 decide limpeza e mantém devolução pendente (`app/docs/LEIS.md:151`); alcance [a medir].

lei: ULTIMO-ATO-TEXTO-F8 -- hoje leitores/SILENCIO usam texto corrente; opcoes A conservar/B substituir pelo último ato; lei existente L-002 exige autoridade comum e L-130 limita substituição, sem decidir precedência (`app/docs/LEIS.md:60`; `app/docs/LEIS.md:151`; `app/ponto/services/veredito_celula.py:197`); alcance [a medir].

## 17/11 08:02 · sessao 33e051dae175 · - · - · origem=Ronald (tmux)

- O contrato continua obrigatório antes da construção: cinco linhas no RELATO, com quem chama, quem espera/como, falha/limite, medida/esperado e quem desliga; revisão uma vez pelo Codex verificador. O item 5 fornecido não explicita falha/limite nem desligamento; eventual contrato separado no RELATO é [nao verificado]. (app/docs/PISTA.md:170-176; CLAUDE.md:394)

- O contador novo não substitui o placar vigente: este exige leitura/sonda/investigação no Codex versus opus/sonnet nas 24 h, motivos dos desvios e alerta dos descartes; número de chamadas por hora mede outro universo. (CLAUDE.md:382-391; app/docs/PISTA.md:165-167)

- A extensão deve preservar o produtor existente: `codex_limite_estado.py` já concentra a leitura de chamadas, contando `ini` por papel nas 24 h e compondo a linha do Codex; `codex_estado.sh` gera outra página, `CODEX-ESTADO.md`, com respostas finais e raspagem. (bin/codex_limite_estado.py:49-79; bin/codex_limite_estado.py:235-243; bin/codex_estado.sh:24-36)

- Existe alarme de ociosidade da sessão **Code**, baseado em transcript Claude e painel; não mede intervalos entre chamadas Codex. Não encontrei nesse percurso produtor equivalente ao solicitado para o Codex. (bin/alarme_sessao_ociosa.py:18-22; bin/alarme_sessao_ociosa.py:144-152; ausência global: [nao verificado])

- Limites vigentes podem gerar ausência de chamadas com reserva aberta: B4-CODEX nega chamadas com uso semanal acima de 85% fora da escada e, acima do orçamento diário, admite somente volta final; o contador não recebe autorização para remover essas guardas. (bin/disjuntor.py:1322-1325; bin/disjuntor.py:1468-1485)

- Pelo escopo literal fornecido, medir log e fila não implica dinheiro nem gravado exportado [nao verificado quanto à implementação futura]. As fronteiras de `!` permanecem as de CLAUDE.md:265-268; instrumento exige pouso próprio, separado de produto, em CLAUDE.md:325-328. [nao verificado]

- A ambiguidade merece devolução porque “rótulo diz o que a conta faz” e “contador == universo”; condição que não fecha não pode ser ampliada por interpretação. (CLAUDE.md:35-38)

## 17/11 08:02 · sessao a1231bb007b1 · - · - · origem=Ronald (tmux)

Divergências confirmadas nas faixas auditadas:

| GRADE | Obra e estado atual | RELATO contraditório | Estado sugerido |
|---|---|---|---|
| app/docs/GRADE-MESTRA.md:1422 | O10-D-PERMUTA-RECIPROCA — REGISTRADA | relato_delta_vivo.diff: `+ESTADO 08:2x O10-D 2a leitura opus APROVA a arvore bdbff7c4a; pouso so com corte... e o ! de escala` | APROVADA em cópia bdbff7c4a; pouso aguarda cortes pendentes e ! de escala |
| app/docs/GRADE-MESTRA.md:1765 | FATO-DE-AUSENCIA-ACORDA-A-PROJECAO / LOTE7 — PARADA, c6 anunciada | app/docs/RELATO.md:162 — LOTE7 c7 NO AR | NO AR, c7 |
| app/docs/GRADE-MESTRA.md:1780 | BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS — PARADA, leis do predesenho | app/docs/RELATO.md:32 — c4+c4c POUSO, opus APROVA, deploy registrado | POUSADA, c4+c4c, com deploy registrado |
| app/docs/GRADE-MESTRA.md:1789 | E4C-F2 — PARADA, matriz e leis | relato_delta_vivo.diff: `+DESPACHO 07:5x ... item 5: F2 construtor -> opus ...` | EM CURSO, construtor opus sobre 708162b15 |
| app/docs/GRADE-MESTRA.md:1799 | BUG-ENC-CELULAS-FORMA — EM VOO, opus final | app/docs/RELATO.md:256 — NO AR | NO AR |
| app/docs/GRADE-MESTRA.md:1800 | E3-RESTO-R3 — EM VOO, c17 retida | app/docs/RELATO.md:243 — R3 NO AR 01:36; app/docs/RELATO.md:80 — c17 POUSADA | POUSADA, c17 NO AR |
| app/docs/GRADE-MESTRA.md:1801 | E4C-F1-NOVA-JUIZ-RESPONDIDA — EM VOO, B1 no anexo | app/docs/RELATO.md:174 — B1 NO AR; relato_delta_vivo.diff: `+ESTADO 04:1x ... APROVA ...708162b15; ... pouso ... ! das 2 PAREI` | B1 NO AR; B2 APROVADA 708162b15, pouso em PAREI |
| app/docs/GRADE-MESTRA.md:1803 | HE-REVOGADA-NA-PORTA — EM VOO, pre-opus | app/docs/RELATO.md:259 — NO AR 271ecba2e | NO AR |
| app/docs/GRADE-MESTRA.md:1804 | SITUACAO-DESLIGADO / LOTE3 — EM VOO, c6 na fila | app/docs/RELATO.md:184 — c7 NO AR | NO AR, c7 |

Exclusões relevantes: FERIADO-G:1802 já registra PAREI 3(ii), coerente com RELATO:288-290; F1:1527 pertence à O30, conforme GRADE:1524-1527, e não à espinha. GRADE:1565 e :1566 já refletem os fechamentos de A2 e O19, confirmados por RELATO:122 e :108.

Total desta auditoria: **9 linhas**.

## 17/11 08:02 · sessao 3dc7ee60b585 · QUEIMA-UTIL-1711 · leitura · origem=Code

Leitura somente; não alterei arquivos nem executei banco, logs ou testes. A aprovação da árvore e os resultados dos anexos são dados fornecidos, sem revalidação nesta sessão. [não verificado] [nao verificado]

1. **`d1=d2`: trocar turnos na mesma data.**

   Hoje a porta recusa imediatamente datas iguais. O dia assumido recebe os horários do próprio vínculo, produzidos por `montar_dna`; não recebe os horários da pessoa substituída. [app/ponto/portas/celula.py:1002; app/ponto/portas/celula.py:636; app/ponto/management/commands/gerar_celulas.py:85]

   As opções e seus alcances são:

   - **Aceitar, transportando o turno cedido:** alcança os pedidos na mesma data; se essa mudança também for aplicada às datas diferentes, muda igualmente o horário dos dias assumidos nessas permutas. O alcance passa pelo produtor do DNA e pelos leitores dos horários congelados, como `EscalaColaborador.marcos_do_dia`. [app/ponto/portas/celula.py:828; app/ponto/management/commands/gerar_celulas.py:104; app/escala/models.py:1124]
   - **Manter datas iguais fora da porta:** mantém recusado todo esse conjunto de pedidos, contrariando o aval registrado. [app/ponto/portas/celula.py:1002; app/docs/PROMPTS.md:2015]

   Aceitar exige mais que apagar o primeiro `if`: na mesma data existem apenas duas combinações pessoa/data, enquanto a validação exige quatro cartas associadas a quatro combinações distintas; a barreira de sobreposição também precisa representar a troca de horários corretamente. [app/ponto/portas/celula.py:1022; app/ponto/portas/celula.py:1026; app/ponto/portas/celula.py:916]

   **Linha final — DECIDIDO PELA LEI LEI-O10-D-PERMUTA: aceitar `d1=d2`; texto: “d1=d2 permitido, nao exportada”. P13 explica a limitação técnica, mas não revoga esse aceite; a representação do turno transportado permanece pendente.** [app/docs/PROMPTS.md:2015; app/ponto/management/commands/gerar_celulas.py:73; app/ponto/portas/celula.py:976]

2. **Tipo e aprovação da carta.**

   Hoje a permuta exige quatro cartas das pessoas/datas certas, cobertura do dia exato e documento. Exclui somente cartas rejeitadas: não exige tipo `permuta` nem aprovação. [app/ponto/portas/celula.py:1022; app/ponto/portas/celula.py:1034; app/ponto/portas/celula.py:1042]

   As opções e seus alcances são:

   - **Exigir tipo permuta aprovado:** recusa qualquer pedido com pelo menos uma carta de outro tipo ou ainda não aprovada. É a condição já registrada na lei. [app/docs/PROMPTS.md:2015]
   - **Exigir apenas aprovação, aceitando tipos atuais:** retira as pendentes, mas continua usando atestado, declaração ou outro tipo como carta; não cumpre a exigência do tipo específico. A declaração, por exemplo, tem efeito de abono na semente. [app/docs/PROMPTS.md:2015; app/ponto/catalogo/ausencias.py:96]
   - **Manter como está:** conserva a aceitação de cartas documentadas não rejeitadas, independentemente do tipo. [app/ponto/portas/celula.py:1042]
   - **Implementar a carta específica sem efeito de ausência:** envolve a validação da permuta e os consumidores de ausência. Apenas adicionar o nome `permuta` não basta: o cadastro oferece somente efeitos `abona`, `desconta` e `suprime`; a cobrança considera ausências não rejeitadas, enquanto a folha considera aprovadas. [app/ponto/models.py:571; app/ponto/turnos.py:2523]

   O código não traz `permuta` na semente, mas permite cadastrar tipos pela UI. Portanto, ausência desse código no cadastro de produção não está demonstrada; tampouco um código cadastrado provaria neutralidade. [app/ponto/catalogo/ausencias.py:87; app/ponto/models.py:562; app/ponto/models.py:571]

   A troca isolada também aceita documentos não rejeitados sem verificar tipo; estender a ela o contrato específico das quatro cartas seria alcance adicional, pois o contrato antigo trata um par confirmado pelo administrador. [app/ponto/portas/celula.py:759; app/docs/CORTES.md:254]

   **Linha final — DECIDIDO PELA LEI LEI-O10-D-PERMUTA: exigir carta de tipo permuta aprovada; texto: “carta = ato tipo permuta aprovado”. Recusar cartas inadequadas é compatível com L-081, “aplica-se a mais RESTRITIVA”; a representação neutra continua exigindo desenho.** [app/docs/PROMPTS.md:2015; app/docs/LEIS.md:108; app/ponto/models.py:571]

3. **`IntegrityError`: duas operações tentam criar a mesma célula.**

   Hoje a porta trava as células existentes. Se a célula não existe, `editar_dia` tenta criá-la; o banco exige unicidade por pessoa/data. A permuta e a troca capturam somente suas exceções próprias de recusa, deixando `IntegrityError` escapar. O ato está dentro de uma transação e os efeitos posteriores ficam para o commit. [app/ponto/portas/celula.py:1016; app/ponto/portas/celula.py:644; app/escala/models.py:1833; app/ponto/portas/celula.py:1091; app/ponto/portas/celula.py:798; app/ponto/portas/celula.py:1089]

   As opções e seus alcances são:

   - **Travar a chave pessoa/data antes da criação:** pretende impedir a disputa. Precisa coordenar os caminhos que criam essa célula, incluindo edição, regeneração e geração; adotá-la somente na permuta deixa os demais criadores fora desse acordo. [app/ponto/portas/celula.py:645; app/ponto/portas/celula.py:294; app/ponto/portas/celula.py:1273]
   - **Após desfazer o ato, devolver recusa para repetir:** alcança as colisões que efetivamente ocorrerem. O tratamento precisa considerar permuta → troca → edição e distinguir a unicidade esperada de outros erros de integridade. Essa classificação não está implementada nesses caminhos. [app/ponto/portas/celula.py:1074; app/ponto/portas/celula.py:775; app/ponto/portas/celula.py:1091; app/escala/models.py:1833]
   - **Manter a exceção escapando:** conserva o desfazimento transacional, mas essa ocorrência não recebe o retorno normal `recusada`. [app/ponto/portas/celula.py:999; app/ponto/portas/celula.py:1091]

   Célula ausente mede exposição à criação; não demonstra colisão ocorrida nem sua frequência. A frequência em produção permanece [não verificado].

   **Linha final — DECIDIDO PELA LEI O10-D-PERMUTA-RECIPROCA: desfazimento integral e recusa falante; textos: “rollback integral” e “recusa falante”. A escolha entre trava de chave e tratamento para repetir permanece pendente; H12H06-B-SIMPLES já decide “(b) sem ordem global”.** [app/docs/PROMPTS.md:2013; app/docs/BACKLOG.md:2198; app/docs/PROMPTS.md:1890]

4. **“Cede só plantão”: pode oferecer um dia que já era folga?**

   Hoje a guarda consulta o previsto dos dois cedentes. Se algum não tem plantão no dia oferecido, recusa a permuta antes das duas trocas; cadastro sem resposta também impede o ato. O teste fixa essa recusa. [app/ponto/portas/celula.py:905; app/ponto/portas/celula.py:909; app/ponto/portas/celula.py:959; app/ponto/portas/celula.py:1065; app/ponto/tests/test_o10d_permuta_reciproca.py:320]

   As opções e seus alcances são:

   - **Manter:** continua recusando pedidos com pelo menos um dia cedido sem plantão previsto. [app/ponto/portas/celula.py:909]
   - **Retirar somente essa guarda:** deixa passar dessa etapa apenas os pedidos cujo único impedimento era ceder folga; os demais continuam barrados. Para quem já folgava no dia cedido, manter esse dia como folga e assumir outro acrescenta um dia previsto — consequência inferida do par escrito. [app/ponto/portas/celula.py:1065; app/ponto/portas/celula.py:773]

   A lei fala em “compensacao”, e TROCA-DE-PLANTAO determina “dia cedido = folga trocada sem desconto”. Isso sustenta a justificativa do comentário, mas não enuncia literalmente que o dia precisava ser plantão antes. L-081 não fornece esse predicado de negócio ausente; L-004 exige respeitar o escopo literal do aval. [app/docs/PROMPTS.md:2015; app/docs/CORTES.md:254; app/ponto/portas/celula.py:907; app/docs/LEIS.md:108; app/docs/LEIS.md:62]

   **Linha final — PENDENTE NO LITERAL: manter ou retirar “cede só plantão”. Não cabe carimbar “DECIDIDO PELA LEI” apenas pela interpretação de “compensação”; o anexo T5(ii) também apresenta essa exigência como regra nova.** [app/docs/PROMPTS.md:2015; app/docs/CORTES.md:254; não verificado: anexo entrega.md, T5(ii)]

5. **Interjornada e DSR: descanso entre turnos e na semana.**

   Hoje a permuta verifica os dois colaboradores. Barra intervalo inferior a 11 horas antes ou depois do turno assumido, procurando turnos nos dois dias anteriores e posteriores. Para DSR, barra trabalho em todos os dias da semana segunda–domingo que contém a data assumida. São cálculos locais da porta. [app/ponto/portas/celula.py:822; app/ponto/portas/celula.py:927; app/ponto/portas/celula.py:940; app/ponto/portas/celula.py:947; app/ponto/portas/celula.py:952]

   A troca isolada escreve seu par sem chamar essas barreiras; a permuta as chama antes de compor as duas trocas. [app/ponto/portas/celula.py:775; app/ponto/portas/celula.py:1065]

   As opções e seus alcances são:

   - **Conservar a obrigação e regularizar o juiz:** mantém as recusas da permuta e coloca a decisão sob a autoridade exigida pela arquitetura. Registrar predicado novo exige o corte previsto em L-011; declarar um predicado já único exige as medidas de identidade e universo previstas em CLAUDE. [app/docs/LEIS.md:69; CLAUDE.md:278; CLAUDE.md:314]
   - **Estender também à troca isolada O10-B:** acrescenta barreiras a uma segunda porta; os casos adicionais são os pares isolados que hoje passam sem essa avaliação. Essa extensão não aparece decidida no aval da permuta localizado. [app/ponto/portas/celula.py:703; app/ponto/portas/celula.py:775; app/docs/PROMPTS.md:2015]
   - **Retirar as barreiras da permuta:** elimina esses impedimentos, mas contraria a lei existente. [app/docs/PROMPTS.md:2015]

   O código testa semana fixa e dias com trabalho; não implementa nesse trecho uma janela móvel nem um teste de duração contínua do descanso semanal. A homologação jurídica dessa fórmula não foi demonstrada pelo dado fornecido. [app/ponto/portas/celula.py:952; não verificado] [nao verificado]

   **Linha final — DECIDIDO PELA LEI LEI-O10-D-PERMUTA: exigir descanso na permuta; textos: “11h art.66 previsto nos dois colabs” e “compensacao, sem DSR na semana barra”. Permanecem pendentes o juiz autorizado e a extensão à O10-B; JUIZ-ANTES-DO-SÍTIO manda “predicado NOVO = pergunta no topo ANTES de curar qualquer sitio”.** [app/docs/PROMPTS.md:2015; CLAUDE.md:314]

**Sonda textual para quantificar os alcances [a medir].** O manifesto deve conter candidatos reais, com pessoas, datas e cartas; a assinatura da porta recebe esses elementos explicitamente. Não há esse manifesto no dado fornecido, portanto não atribuo quantidade de pedidos à frota. [app/ponto/portas/celula.py:965; não verificado]

O texto abaixo consulta leitores existentes e mede barreiras dessa etapa, cartas e exposição à criação. Não chama escritores nem certifica aceite integral, idempotência, colisões ocorridas ou comportamento de uma implementação futura. [app/ponto/portas/celula.py:869; app/ponto/portas/celula.py:1034; app/ponto/portas/celula.py:1058; não verificado: sonda não executada]

```python
# [a medir] Somente texto; não executado nem salvo.
# Executar em processo novo, no tenant selecionado pelo envelope de leitura.
# CASOS: lista explícita do DP, sem nome/CPF.
# Item: (caso_id, col_a_id, d1_date, col_b_id, d2_date, cartas_ids)

import datetime as dt
from collections import Counter
from django.db import connection, transaction
from colaboradores.models import Colaborador
from escala.models import CelulaDia
from ponto.models import Ausencia, TipoAusencia
from ponto.turnos import dias_da_ausencia
from ponto.portas.celula import _barreiras_da_permuta, _empresa_do_colab

def medir(CASOS):
    if CASOS is None:
        raise ValueError("Falta manifesto; ausência de dados não significa zero.")

    n = Counter()
    cartas_vistas = set()
    cartas_por_tipo_status = Counter()

    with transaction.atomic():
        with connection.cursor() as cur:
            cur.execute(
                "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
            )

        print("[a medir] cadastro_permuta", list(
            TipoAusencia.objects.filter(codigo="permuta").values(
                "codigo", "ativo", "efeito", "parcial", "fim"
            )
        ))

        for caso, aid, d1, bid, d2, ids in CASOS:
            n["candidatos_do_manifesto"] += 1
            if aid == bid:
                n["entrada_mesma_pessoa"] += 1
                continue

            cartas = list(Ausencia.objects.filter(pk__in=ids))
            for carta in cartas:
                if carta.pk not in cartas_vistas:
                    cartas_vistas.add(carta.pk)
                    cartas_por_tipo_status[(carta.tipo, carta.status)] += 1

            if any(c.tipo != "permuta" or c.status != "aprovada"
                   for c in cartas):
                n["candidatos_com_carta_fora_tipo_aprovacao_da_lei"] += 1

            if d1 == d2:
                n["d1_igual_d2_recusado_antes_das_demais_guardas"] += 1
                continue

            esperadas = {(aid, d1), (bid, d1), (bid, d2), (aid, d2)}
            recebidas = {(c.colaborador_id, c.data) for c in cartas}
            if len(cartas) != 4 or recebidas != esperadas:
                n["fora_da_etapa_cartas_incompletas"] += 1
                continue
            if any(not c.documento or c.status == "rejeitada"
                   for c in cartas):
                n["fora_da_etapa_documento_ou_rejeicao"] += 1
                continue
            if any(c.data == dt.date.max for c in cartas):
                n["limite_date_max_a_examinar"] += 1
                continue
            if any(dias_da_ausencia(
                c.tipo, c.data, c.data_fim,
                c.data, c.data + dt.timedelta(days=1)
            ) != [c.data] for c in cartas):
                n["fora_da_etapa_carta_nao_e_do_dia_exato"] += 1
                continue

            emp = _empresa_do_colab(aid)
            if emp is None or emp != _empresa_do_colab(bid):
                n["fora_da_etapa_empresa"] += 1
                continue

            a = Colaborador.objects.get(pk=aid)
            b = Colaborador.objects.get(pk=bid)
            textos = _barreiras_da_permuta(
                [(a, d1, d2), (b, d2, d1)], emp,
                [c.pk for c in cartas]
            )

            classes = set()
            for texto in textos:
                if "nao tem plantao previsto" in texto:
                    classes.add("cede_folga")
                elif "INTERJORNADA" in texto:
                    classes.add("interjornada")
                elif "DSR --" in texto:
                    classes.add("dsr")
                else:
                    classes.add("outro")

            n["candidatos_avaliados_pela_barreira_real"] += 1
            for classe in classes:
                n["incidencia_" + classe] += 1
            if classes == {"cede_folga"}:
                n["unico_impedimento_nesta_etapa_cede_folga"] += 1
            if classes and classes <= {"interjornada", "dsr"}:
                n["unicos_impedimentos_nesta_etapa_descanso"] += 1

            if not textos:
                ausentes = sum(
                    not CelulaDia.objects.filter(
                        colaborador_id=cid, data=dia
                    ).exists()
                    for cid, dia in esperadas
                )
                n["celulas_ausentes_em_candidatos_sem_barreira"] += ausentes
                if ausentes:
                    n["candidatos_sem_barreira_expostos_a_criacao"] += 1

        print("[a medir] alcance_no_manifesto", dict(n))
        print("[a medir] cartas_distintas_por_tipo_status",
              sorted(cartas_por_tipo_status.items()))
```

Para a **troca isolada**, a medição precisa de manifesto próprio de pares: aplicar a simulação da permuta a esses pares produziria uma resposta sobre outra operação. O alcance quantitativo dessa extensão permanece [a medir] [não verificado]. [app/ponto/portas/celula.py:703; app/ponto/portas/celula.py:965]

## 17/11 08:02 · sessao e60642852241 · - · - · origem=Ronald (tmux)

Divergências firmes da faixa, com referências da cópia:

| GRADE | Estado atual | RELATO contrário | Estado proposto |
|---|---|---|---|
| app/docs/GRADE-MESTRA.md:50 — BUG-ENC-CELULAS-FORMA | INTERROMPIDA | app/docs/RELATO.md:256: “NO AR (7825fec5b)” | NO AR 7825fec5b |
| app/docs/GRADE-MESTRA.md:106 — FIO-E0-V4-E1-ENTRADAS | PARADA | app/docs/RELATO.md:284: “FECHADA ... nucleo a8a692f55, 16 complementos em linhas proprias” | FECHADA; complementos em linhas próprias |
| app/docs/GRADE-MESTRA.md:176 — REABRIR-NAO-DESTROI | REGISTRADA; falta smoke | app/docs/RELATO.md:129: “REABRIR-NAO-DESTROI FECHADA” | FECHADA pelo CORTE A de 16/11 |
| app/docs/GRADE-MESTRA.md:178 — BUG-ANCORA-NAO-REGENERA | PARADA | app/docs/RELATO.md:133: “O99 e BUG-ANCORA-NAO-REGENERA FECHADAS” | FECHADA; cadastro encaminhado ao administrador |
| app/docs/GRADE-MESTRA.md:190 — O99 | PARADA | app/docs/RELATO.md:133: “O99 e BUG-ANCORA-NAO-REGENERA FECHADAS” | FECHADA pelo CORTE B de 16/11 |
| app/docs/GRADE-MESTRA.md:242 — BO-HE-RECUSAR-LOTE-500 | REGISTRADA; smoke pendente | app/docs/RELATO.md:129: “BO-HE-RECUSAR-LOTE-500 FECHADA” | FECHADA pelo CORTE A de 16/11 |
| app/docs/GRADE-MESTRA.md:270 — BO-0611-05 | PARADA | app/docs/RELATO.md:280 e :478: “BO-0611-05 ... NO AR (fb9269ea9)” | NO AR fb9269ea9 |
| app/docs/GRADE-MESTRA.md:689 — CONTRATO-DE-ENTRADA-DO-FIO | BRIEF PRONTO | app/docs/RELATO.md:475: “NO AR ... CEF 77dfa94fa” | NO AR 77dfa94fa |
| app/docs/GRADE-MESTRA.md:1024 — BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS | EM VOO | app/docs/RELATO.md:32: pouso e deploy c4+c4c; diff `+ESTADO 07:5x SMOKES... BUG-PAR c4 FECHADA` | FECHADA c4; pouso e smoke concluídos |
| app/docs/GRADE-MESTRA.md:1422 — O10-D-PERMUTA-RECIPROCA | REGISTRADA | diff `+ESTADO 08:2x O10-D 2a leitura opus APROVA a arvore bdbff7c4a; pouso so com corte...` | APROVADA bdbff7c4a; pouso aguarda cortes e ! de escala |
| app/docs/GRADE-MESTRA.md:1765 — FATO-DE-AUSENCIA-ACORDA-A-PROJECAO / LOTE7 | PARADA — c6 anunciada | app/docs/RELATO.md:162: “LOTE7 c7 NO AR” | NO AR c7; smoke concluído |
| app/docs/GRADE-MESTRA.md:1780 — BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS | PARADA — leis do predesenho | app/docs/RELATO.md:32 e diff smoke acima | FECHADA c4; pouso e smoke concluídos |
| app/docs/GRADE-MESTRA.md:1789 — E4C-F2 | PARADA — matriz e leis | diff `+DESPACHO 07:5x ... item 5: F2 construtor -> opus` | EM CURSO; construtor opus em cópia de 708162b15 |
| app/docs/GRADE-MESTRA.md:1799 — BUG-ENC-CELULAS-FORMA | EM VOO — opus final | app/docs/RELATO.md:256 | NO AR 7825fec5b |
| app/docs/GRADE-MESTRA.md:1800 — E3-RESTO-R3 | EM VOO — c17 retida | app/docs/RELATO.md:235: merge c17; :80: “E3-R3 c17 POUSADA” | POUSADA c17 |
| app/docs/GRADE-MESTRA.md:1801 — E4C-F1-NOVA-JUIZ-RESPONDIDA | EM VOO — B1 no anexo | app/docs/RELATO.md:174: “F1 B1 ... NO AR”; diff `+ESTADO 04:1x ... B2 ... APROVA ... pouso ... ! ... 2 PAREI` | B1 NO AR; B2 APROVADA 708162b15, pouso retido pelas duas PAREI |
| app/docs/GRADE-MESTRA.md:1803 — HE-REVOGADA-NA-PORTA | EM VOO — pre-opus | app/docs/RELATO.md:259: “NO AR 271ecba2e” | NO AR 271ecba2e |
| app/docs/GRADE-MESTRA.md:1804 — SITUACAO-DESLIGADO / LOTE 3 desligado | EM VOO — c6 na fila | app/docs/RELATO.md:184: “NO AR 10:58 LOTE3 c7 ... be4a6219e” | NO AR c7 be4a6219e |

Três casos adicionais dependem do critério sobre estado operacional:

- **O-JANELA-HE**, GRADE:180, “REGISTRADA (no ar ... falta smoke)” versus RELATO:129, “ABERTA com o numero”; proposta: **ABERTA; medição de uso registrada**.
- **SEM-CELULA-11-DEVE-TER-CELULA**, GRADE:243, “PARADA ... col305 espera !” versus RELATO:130, caso correto, dono CADASTRO, nada a aplicar; proposta: **Código publicado; col305 é CADASTRO, sem apply**. O RELATO não escreve FECHADA. [nao verificado]
- **COMPETENCIA-PAGA-UM-JUIZ**, GRADE:1786, “PARADA — corte da 10 paga” versus RELATO:171, corte registrado e obra atrás da espinha, e :104, sem construtor agora; proposta: **AGUARDA A ESPINHA; corte LU2-06 registrado; tela e medida como lateral**.

Excluiria FIO-E0-V4-E3-LEITORES:119: o pouso de R3 está comprovado, mas não achei fecho explícito da mãe com seus nove complementos. FRAC5-TEXTO pousou em 14/11, fora de 15–17/11. AUSENCIA-EFEITO, UI-EDITOR-CALENDARIO e UI-VINCULO não possuem linha própria na GRADE fornecida. [nao verificado]

Total da faixa: **18 linhas firmes; 3 casos operacionais adicionais para consolidação**.

## 17/11 08:02 · sessao 3c6c5cdeb9a6 · QUEIMA-UTIL-1711 · leitura · origem=Code

**Nenhuma candidata pode ser aberta com cruzamento zero demonstrado.** Nos alvos nominais das remedidas, a lista aproximada da B2 já cruza **3 arquivos da F3 e 5 da F9**. O cruzamento integral das edições futuras permanece **[nao verificado]**, porque os anexos distinguem alvos de alteração de consumidores e testes apenas alcançados. (F3/remedida.md, §6; F9/remedida.md, §6; numeração dos arquivos externos **[nao verificado]**.)

O comando `git diff --name-only fbc5c76ce 708162b15` falhou com “not a git repository”. Portanto, **se o diff exato diverge de `b2_arquivos_vs_520d.txt`: [nao verificado]**. Os números abaixo usam essa aproximação.

Nas tabelas: **sim** = arquivo comum identificado nos escopos fornecidos; **—** = ausente da lista explícita da F2 ou da aproximação B2; **?** = edição pela F4/F7 **[nao verificado]**. As âncoras comprovam o arquivo na cópia; o planejamento vem dos anexos. (F2: seção “ARQUIVOS QUE A F2 VAI TOCAR”; F3/remedida.md, §6; F9/remedida.md, §6; correspondência externa **[nao verificado]**.)

**F3 — cinco arquivos do núcleo nominal**, conforme a ressalva expressa da própria remedida. Lista final, incluindo consumidores e testes a editar: **[nao verificado]**. (F3/remedida.md, §6.)

| Arquivo e âncora na cópia | F2 | F4 | F7 | B2 aproximada |
|---|:---:|:---:|:---:|:---:|
| `app/chamados/services/validacao.py:594` | — | sim¹ | ? | sim |
| `app/chamados/services/acoes_disputa.py:97` | — | ? | ? | sim |
| `app/chamados/services/verbos.py:263` | — | ? | ? | sim |
| `app/chamados/services/painel.py:50` | — | ? | ? | — |
| `app/chamados/veredito_lavrado.py:151` | — | ? | ? | — |

¹ A F4 declara alteração da composição que procura a irmã; esse leitor está em `app/chamados/services/validacao.py:224`. É o cruzamento identificado **F3∩F4 = `{app/chamados/services/validacao.py}`** no núcleo considerado. (f4.r, seção U163-NO-K6; localização externa **[nao verificado]**.)

**F3∩F2 = vazio no núcleo nominal. F3∩F7: nenhum toque comum demonstrado; ausência integral de colisão [nao verificado].** A união dos arquivos comuns identificados nesse núcleo contém os três arquivos marcados na B2; `validacao.py` conta uma vez. (Anexos F2, F3/remedida.md §6, f4.r e f7.r; numeração externa **[nao verificado]**.)

A F3 também enumera autoridades, consumidores e provas, condicionando sua edição ao contrato final. Entre esses arquivos adicionais, os seguintes constam da B2 aproximada; **não os conto como edição confirmada da F3**. Testes entram nessa mesma verificação. (F3/remedida.md, §6; toque futuro **[nao verificado]**.)

```text
app/chamados/juizes.py
app/chamados/models.py
app/chamados/services/vigia_fio.py
app/chamados/services/fio.py
app/chamados/management/commands/auditar_invariantes_chamados.py
app/chamados/views.py
app/chamados/views_cobrar.py
app/chamados/tests/test_contract_leitores_vitalidade.py
app/chamados/tests/test_e3_catraca_respondedores.py
```

Se `models.py` ou `juizes.py` entrarem efetivamente na F3, haverá também cruzamento com a F2 — `juizes.py` é condicional na lista da F2. Essa necessidade de edição pela F3 está **[nao verificado]**. (F2, seção de arquivos; F3/remedida.md, §6.)

**F9 — doze comandos e um arquivo de selo declarados como alvos.** Os outros testes da remedida são revalidação, com necessidade de alteração expressamente **[nao verificado]**. (F9/remedida.md, §6.)

| Arquivo e âncora na cópia | F2 | F4 | F7 | B2 aproximada |
|---|:---:|:---:|:---:|:---:|
| `app/chamados/management/commands/auditar_invariantes_chamados.py:177` | — | ? | ? | sim |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:44` | — | ? | ? | sim |
| `app/chamados/management/commands/dedup_perguntas_fato.py:75` | — | ? | ? | sim |
| `app/ponto/management/commands/reconciliar_grade.py:461` | — | ? | ? | sim |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py:67` | — | ? | ? | — |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:32` | — | ? | ? | — |
| `app/chamados/management/commands/curar_perguntas_em_limbo.py:46` | — | ? | ? | — |
| `app/chamados/management/commands/auditar_disputas.py:71` | — | ? | ? | — |
| `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:147` | — | ? | ? | — |
| `app/chamados/management/commands/validar_classe_a.py:80` | — | ? | ? | — |
| `app/chamados/management/commands/relavrar_via_i6.py:49` | — | ? | ? | — |
| `app/ponto/management/commands/reconciliar_fantasmas.py:213` | — | ? | ? | — |
| `app/chamados/tests/test_contract_leitores_vitalidade.py:310` | — | ? | ? | sim |

**F9∩F2 = vazio nos treze alvos declarados. F9∩F4 e F9∩F7: nenhum toque comum demonstrado; ausência integral de colisão [nao verificado].** Censo, pré-backfill, dependência e prova relacionada não demonstram que F4/F7 editarão esses arquivos. (F9/remedida.md, §6; f4.r; f7.r; numeração externa **[nao verificado]**.)

**L-126:** nenhuma candidata atingiu zero demonstrado para receber construtor. Entre os alvos nominais das duas tabelas, proibidos pelos padrões de caminho: **nenhum**; eventual inclusão de `app/chamados/models.py` na F3 seria proibida. (`bin/codex_pergunta.sh:122`, `bin/codex_pergunta.sh:126`, `bin/codex_pergunta.sh:127`, `bin/codex_pergunta.sh:134`.)

A última linha registra **3 e 5 como arquivos distintos comuns dos alvos nominais contra a B2 aproximada**, não como totais integrais confirmados pelo diff solicitado **[nao verificado]**.

CRUZAMENTO: F3=3, F9=5; abre=nenhuma; construtor=n/a

## 17/11 08:02 · sessao 699b836feb5b · - · - · origem=Ronald (tmux)

GRADE 527–1100: duas divergências diretas e uma com ressalva de escopo.

| Obra | GRADE: estado | RELATO contrário | Estado proposto |
|---|---|---|---|
| CONTRATO-DE-ENTRADA-DO-FIO | app/docs/GRADE-MESTRA.md:689 — BRIEF PRONTO | app/docs/RELATO.md:475 — “NO AR dedup b6d268dfd + CEF 77dfa94fa”, deploy rc 0 para ambos; a associação CEF com esta obra consta também em app/docs/RELATO.md:546 | NO AR; CEF pousada em 77dfa94fa. Smoke col677 solicitado. |
| DISPUTA-ZUMBI-1110 | app/docs/GRADE-MESTRA.md:1029 — REGISTRADA | app/docs/RELATO.md:285 — “ESTACIONADA 20:1x”, cópia guardada, volta com a lista esgotada ou por aval. app/docs/RELATO.md:476 distingue a cura das 18 já aplicada do contador incompleto. | PARADA; cura das 18 aplicada, contador estacionado até esgotar a lista ou novo aval. |
| BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS | app/docs/GRADE-MESTRA.md:1024 — EM VOO | app/docs/RELATO.md:32 — BUG-PAR-O-PAR c4+c4c POUSO e deploy; anexo relato_delta_vivo.diff, linha + “ESTADO 07:5x SMOKES-[nome]-1711 item 2: BUG-PAR c4 FECHADA”. O apply do passivo é ato separado em app/docs/RELATO.md:366. | NO AR; c4+c4c pousada e smoke c4 fechado. Apply do passivo em ato separado. |

Ressalva do terceiro: o título literal antigo não reaparece no trecho recente; a continuidade é sustentada pela mesma cópia 6e2fc9133 em app/docs/GRADE-MESTRA.md:1780 e app/docs/RELATO.md:163. Não promover PARIDADE-ABSOLVIDAS-168 (:1032) a FECHADA: app/docs/RELATO.md:299 preserva lote/apply separado.

Não contei E3-FORMA-SQL-DO-JUIZ (:537) nem E3-IMPACTO-E-CONTRATOS-DE-ENTRADA (:538): o recorte recente registra genericamente “E3 ... NO AR” em app/docs/RELATO.md:484, sem fecho nominal desses complementos. Identidade e encerramento específico em 15–17/11: [não verificado].

O segmento originalmente delegado, RELATO 1001–2000, não contém referências a 15/11, 16/11 ou 17/11; não usei seus registros históricos como prova de mudança nesses dias.

Total recomendado no segmento: 3 linhas, com a ressalva explicitada para BUG-PAR.

## 17/11 08:01 · sessao 8df2ec6160e3 · QUEIMA-UTIL-1711 · leitura · origem=Code

**A matriz fecha: 17 + 11 + 3 + 12 = 43; diferença = 0.** São as quatro classes registradas na obra, ainda REGISTRADA, com dependência de CONTRATO-DE-INSTRUMENTO quando chegar sua vez (`app/docs/GRADE-MESTRA.md:286`).

Os casos abaixo são transcrição do R3 de `causas.md` fornecido; a confirmação individual em produção permanece **[nao verificado]**. O código sustenta os mecanismos descritos; `s1.r` não fornece uma reprodução completa dos 43 **[nao verificado]**.

| Classe | O que o E6 faz | O que a casa faz e qual lei manda | Conserto mínimo proposto no oráculo |
|---|---|---|---|
| **Madrugada — 17** | Chaveia o grupo pela data da primeira batida: `k = str(tu[0].date())`. A montagem E6 fornece somente timestamps (`app/ponto/calculador/nucleo.py:237`; `app/ponto/management/commands/e6_oraculo.py:153`). | Decide `data_turno` pela jornada noturna da véspera, inclusive na saída órfã; turno novo fora do envelope fica no dia próprio. **L-085**, jornada pertence ao dia de início (`app/ponto/turnos.py:285`; `:293`; `:301`; `app/docs/LEIS.md:117`). | Alimentar a comparação com o **dia decidido pelo juiz**, por turno/segmento. O núcleo já aceita o terceiro elemento `dia_do_par`; deslocar indiscriminadamente toda madrugada violaria a guarda do envelope (`app/ponto/calculador/nucleo.py:134`; `app/ponto/turnos.py:302`). |
| **Pareamento posicional — 11** | Soma posições 0→1, 2→3 etc.; havendo quantidade ímpar, abandona a última. Não recebe os papéis da ata nesse caminho (`app/ponto/calculador/nucleo.py:245`; `app/ponto/management/commands/e6_oraculo.py:153`). | Usa o papel da ata; `X` é interno. Existe fallback para tipo quando a ata não responde. **L-087**, instante contra marco/DNA (`app/ponto/turnos.py:1377`; `:1382`; `app/docs/LEIS.md:119`). | Entregar os pares decididos pela autoridade ao ramo `pares=`, preservando órfãs. Esse contrato já existe para evitar outro juiz de marcos no núcleo (`app/ponto/calculador/nucleo.py:65`; `:110`). Preservar também a posição **legítima** da L-115, após pausa fechada (`app/docs/LEIS.md:153`; `app/ponto/turnos.py:725`). |
| **Corte por vão — 3** | Corta por gap ≥8h com contagem par, gap >14h ou vão desde o início >14h. Pode transformar um par legítimo em fragmentos sem minutos; o próprio código reconhece esse erro (`app/ponto/calculador/nucleo.py:215`; `:218`). | O fecho consulta o teto da jornada: previsto +4h, com envelope de 26h nesse ramo e exceção quando as duas pontas caracterizam **L-084**. Dia/papel seguem **L-085/L-087** (`app/ponto/turnos.py:78`; `:1433`; `:1440`; `app/docs/LEIS.md:116`; `:117`; `:119`). | Fazer o E6 respeitar as fronteiras dos turnos decididas pela autoridade, evitando reaplicar o corte cego. Preservar a rejeição de colagem de dias; retirar todo teto não satisfaz essa guarda (`app/ponto/calculador/nucleo.py:110`; `app/ponto/tests/test_e6_turno_nao_cresce_sem_fim.py:61`). |
| **Pausa abaixo do piso — 12** | Subtrai a pausa posicional efetivamente batida. Depois, só dispensa a divergência se `abs(diferença − janela_descontada) ≤ 10`; falta considerar a pausa curta já abatida (`app/ponto/calculador/nucleo.py:247`; `:253`; `app/ponto/management/commands/e6_oraculo.py:236`). | Pausa marcada inferior a 15min não conta como descanso; sem pausa válida, no modo fixo desconta a janela declarada dentro do período. **L-032**, pré-assinalação; piso fundamentado no Art. 71 §1 pelo código (`app/ponto/turnos.py:537`; `:545`; `:756`; `app/docs/LEIS.md:80`). | Conciliar a diferença com **janela descontada menos pausa curta já abatida**, exigindo lastro da mesma pausa e período. Preservar o cru e os negativos sem carimbo/diferença inexplicada; não ampliar simplesmente a tolerância (`app/ponto/management/commands/e6_oraculo.py:236`; `app/ponto/tests/test_e6_janela_declarada_nao_e_divergencia.py:96`; `:108`). |

A alimentação por pares/dia é uma proposta sustentada pelo contrato existente; seu fechamento como desenho permanece **[nao verificado]**. Ela limita a independência do E6 nessa parte: compartilhar a geometria permite conferir sua aritmética, mas não constitui certificação independente do pareador. A certificação por cenários pertence à BATERIA, pela **L-110/L-114** (`app/ponto/calculador/nucleo.py:65`; `app/docs/LEIS.md:136`; `:154`).

**Casos por classe**, todos em 2026, conforme R3 do anexo **[nao verificado]**:

| Classe | Casos do anexo |
|---|---|
| **Madrugada — 17** | col218 **15/10, 14/11**; col37 **05/10, 09/10, 14/11**; col382 **30/09, 10/11**; col515 **09/10, 11/10**; col110 **12/11**; col113 **30/10, 01/11**; col200 **29/10, 02/11**; col296 **02/11**; col516 **28/10**; col727 **10/11**. **[nao verificado]** |
| **Pareamento — 11** | col129 **12/10**; col451 **23/09, 25/09**; col478 **25/09**; col841 **20/10**; col112 **21/10, 22/10**; col874 **22/10**; col989 **23/10**; col996 **28/10, 10/11**. **[nao verificado]** |
| **Corte — 3** | col854 **04/10**; col1021 **06/11**; col909 **11/11**. **[nao verificado]** |
| **Pausa — 12** | col252 **23/09, 24/09, 25/09, 28/09, 01/10, 02/10, 06/10, 09/10, 09/11, 10/11, 11/11, 13/11**. **[nao verificado]** |

Os dois dias de col112 classificados em R2 como “borda mensal” já integram os 11 de pareamento em R3; somá-los novamente duplicaria casos **[nao verificado — anexo causas.md, R2/R3]**.

**Leitores compartilhados pelas classes:** corrigir a montagem E6 alcança também consumidores de horas/paridade/dono; conciliar somente a pausa no comparador não altera automaticamente esses consumidores (`app/ponto/management/commands/e6_oraculo.py:182`; `:195`; `:354`).

| Leitor | Consumo comprovado |
|---|---|
| Comando E6 | Obtém o veredito e compara minutos com o espelho (`app/ponto/management/commands/e6_oraculo.py:328`; `:342`; `:350`). |
| Pautas discordantes | Lê horas e dono (`app/ponto/management/commands/pautas_discordantes.py:86`; `:102`; `:118`). |
| Teto na lista | Monta insumos e lê dono por dia (`app/ponto/services/teto_na_lista.py:72`; `:86`). |
| CXR na lista | Monta insumos com o espelho e lê dono (`app/ponto/services/cxr_na_lista.py:99`; `:103`). |
| Censo de fase 12×36 | Lê dono pelos insumos E6 (`app/ponto/management/commands/censo_fase_12x36.py:287`; `:289`). |
| Férias com batida | Lê dono (`app/ferias/management/commands/listar_ferias_com_batida.py:53`; `:57`). |
| Cadastro × realidade/becos | Consulta dono compartilhado (`app/escala/services/cadastro_realidade.py:510`; `:516`; `:1010`; `:1012`). |
| Calculador e DIFF | Compartilham o núcleo: alimentação e período passam pares; DIFF também possui caminho com `_pares=None` (`app/ponto/calculador/alimentacao.py:449`; `app/ponto/calculador/periodo.py:356`; `app/ponto/management/commands/diff_calculador.py:327`; `:414`). |
| Placar estrutural | Publica o R1 como termômetro a partir de medição declarada (`app/core/placar_estrutural.py:129`; `:131`). |

O CSV é exportado para consumo externo, mas a identidade desse leitor não está demonstrada **[nao verificado]**; o ponto de saída está em `app/ponto/management/commands/e6_oraculo.py:450`.

**RED que morde:** executei quatro microcasos sintéticos em memória sobre as funções da cópia; eles demonstram o caminho cru, sem reproduzir os casos individuais de produção **[nao verificado quanto à produção]**. [nao verificado]

| Classe | Falha observável e teste necessário | [nao verificado]
|---|---|
| Madrugada | **01/10 01:01:26→07:50:17** fica em **01/10** no cru; com a jornada noturna da fixture, deve pertencer a **30/09**. O RED deve entrar pela montagem E6, incluindo o gêmeo fora do envelope que permanece no dia próprio (`app/ponto/calculador/nucleo.py:237`; `app/ponto/tests/test_o195_dia_do_turno_por_envelope.py:59`; `:65`). |
| Pareamento | **07:00 / 12:00 / 17:00** produz **300min**; com papéis **E/X/S**, sem intervalo, a resposta exigida é **600min**. Testar ata real na montagem E6, incluindo órfã e tipos invertidos; fornecer manualmente `pares=` só testa o ramo já existente (`app/ponto/calculador/nucleo.py:245`; `app/ponto/turnos.py:1382`; `app/ponto/management/commands/e6_oraculo.py:153`). |
| Corte | **05:55→19:56** produz **0**, com carimbo de corte; um par legítimo deve conservar **841min**. Acrescentar cenário com pausa e manter o negativo de colagem de dias (`app/ponto/calculador/nucleo.py:216`; `:222`; `app/ponto/tests/test_e6_turno_nao_cresce_sem_fim.py:61`). |
| Pausa | **07/12/12:12/17**, janela fixa **12→13**: cru **588**, casa **540**, diferença **48**, carimbo **60**; o filtro atual devolve **False**. Exigir conciliação e preservar negativos sem carimbo, diferença inexplicada e pausa reconhecida de 15min (`app/ponto/calculador/nucleo.py:247`; `app/ponto/turnos.py:545`; `:760`; `app/ponto/management/commands/e6_oraculo.py:239`). |

Há um teste que **congela o defeito do corte**: apesar do nome “jornada longa de verdade”, exige `minutos={}` para 06:52→21:34. Deve ser revisto junto com a cura (`app/ponto/tests/test_e6_turno_nao_cresce_sem_fim.py:101`; `:114`). Já o teste de dia fornecido manualmente por `pares=` não protege a montagem crua do E6 (`app/ponto/tests/test_e6_oraculo_julga_impar.py:160`; `app/ponto/management/commands/e6_oraculo.py:153`). RED de integração das quatro classes: **[nao verificado]**.

**Dono ESTRUTURA da casa:** segundo R3, nenhum dos **43** foi atribuído à casa; os **dois col922 — 01/11 e 04/11 — estão fora deles**, classificados como SISTEMA porque o realizado soma turno que a tela não exibe. A cópia registra essa separação e mantém E3-geometria “a confirmar” (`app/docs/RELATO.md:127`; `:128`).

Em col922, o anexo informa **1382 = 901 + 481** e **676 = 218 + 458**; `s1.r` preserva as diferenças **901** e **218**, mas não identifica independentemente os turnos **[nao verificado]**. O código mostra caminhos distintos para montar os cards e publicar o realizado (`app/ponto/services/espelho.py:450`; `:460`; `:783`). Isso sustenta investigar coerência da casa pela **L-099**, sem transferir esses dois dias à obra do oráculo (`app/docs/LEIS.md:122`). A suficiência de E3-geometria para curá-los permanece **[nao verificado]**.

Os **12 sem nome** também permanecem fora dos 43; não há dado suficiente para promovê-los a defeito do E6 ou da casa (`app/docs/RELATO.md:128`; **[nao verificado quanto ao dono individual]**).

## 17/11 08:01 · sessao 21059f5d3749 · - · - · origem=Ronald (tmux)

Pelo texto dos anexos:

- **F3: 5 alvos nominais**: `app/chamados/services/validacao.py`, `acoes_disputa.py`, `verbos.py`, `painel.py`; `app/chamados/veredito_lavrado.py`. Os outros 14 arquivos da tabela são autoridades/consumidores; a própria remedida condiciona a necessidade de edição ao contrato final. Todos os 17 arquivos de testes são contratos/fixtures alcançados; a destinação dos novos T1–T9 está **[nao verificado]**.
- **F9: 13 alvos declarados**: os 12 comandos da seção 6 mais `app/chamados/tests/test_contract_leitores_vitalidade.py`. Os outros 17 testes estão explicitamente sob “necessidade de alterar cada um […] [nao verificado]”.

**Interseção do núcleo F3 com B2 aproximada: 3 arquivos**:

1. `app/chamados/services/validacao.py`
2. `app/chamados/services/acoes_disputa.py`
3. `app/chamados/services/verbos.py`

**Interseção dos alvos F9 com B2 aproximada: 5 arquivos**:

1. `app/chamados/management/commands/auditar_invariantes_chamados.py`
2. `app/chamados/management/commands/backfill_decisoes_pergunta.py`
3. `app/chamados/management/commands/dedup_perguntas_fato.py`
4. `app/ponto/management/commands/reconciliar_grade.py`
5. `app/chamados/tests/test_contract_leitores_vitalidade.py`

**F2 declarada:** nenhum dos 5 alvos nominais F3 ou dos 13 alvos F9 coincide com models, juizes opcional ou os testes F2 novos. F3 cita models/juizes no grafo, mas isso não demonstra futura edição. [nao verificado]

**F4:** `validacao.py` aparece como ponto de alteração da composição que procura a irmã no complemento U163; portanto há colisão nominal F3/F4 em `validacao.py`. Os comandos da F9 são citados na F4 como censo, dependência, pré-backfill ou consumidores; nenhuma dessas citações prova edição pela F4.

**F7:** os arquivos centrais da migração de apresentação não coincidem com os 5 nominais F3 ou 13 F9. `validacao.py`, `acoes_disputa.py`, `verbos.py` e comandos F9 aparecem no censo, sem prova de edição. A F7 também cita o contrato de vitalidade como prova relacionada, sem declarar que o alterará.

**Conclusão:** nenhuma tem zero contra **todos**, já pela B2 aproximada. A conclusão sobre o diff exato permanece dependente de verificação Git; o anexo não prova equivalência a `fbc5c76ce..708162b15`. Não há candidata elegível que demande decidir Codex versus Opus.

Evitar afirmar listas finais exaustivas de edições ou ausência absoluta de colisão com F4/F7: os anexos deixam contratos e nomes de testes futuros indefinidos. Os números 3 e 5 são verificáveis apenas para os alvos nominais declarados contra a lista B2 aproximada. [nao verificado]

## 17/11 08:01 · sessao 3bc6ff2ff83b · - · - · origem=Ronald (tmux)

O escopo está correto, com estes rótulos:

- U1 mede **perguntas que a porta poderia ressuscitar agora**; não mede ressuscitações passadas nem garante que serão chamadas. A/B alcançam o mesmo U1; diferem na regra de cobrança. (`app/chamados/models.py:1501`; `app/chamados/models.py:1593`.)
- Obter chamados pelos destinos distintos está correto: `chamado_destino` tem relação reversa `perguntas_que_resolvem`. (`app/chamados/models.py:3522`.)
- Estratificar pelo último ato é inventário; não deve ser apresentado como dono após a escrita, pois a porta também conserva linhas faltantes antes de limpar a projeção. (`app/chamados/models.py:1613`; `app/chamados/models.py:1629`.)
- F5 separado mede **perguntas vivas com instante sem texto**; isso não equivale automaticamente a “última RESPOSTA vazia no painel”. O predicado físico é exato, mas esses conjuntos precisam de estratificação para serem equiparados. (`app/chamados/juizes.py:1531`; `app/chamados/services/tarefa_do_dia.py:159`.)
- Deduplicar a interseção U1/F5 e não aplicar B automaticamente ao F5 preserva os escopos distintos: o corte atual mantém o caso F5 fora do app e com admin, enquanto a devolução geral continua pendente. (`app/docs/CORTES.md:488`.)

## 17/11 08:00 · sessao f2bbc4600591 · - · - · origem=Ronald (tmux)

**Caminho i, provas de código**

| Estado → juiz → operação | Persistência | Retorno / teste | [nao verificado]
|---|---|---|
| EventoCelula pendente; chamado vivo absolvido pela célula ou premissa morta → `_absolvido_pela_celula` / `_veredito_premissa` → `_retratar`. O consumidor chama as duas rotas em `app/chamados/reconciliador.py:1371` e `:1375`; elas chegam à unidade em `:1006` e `:1467`. | `_retratar` consulta retenção em `app/chamados/reconciliador.py:520`; ela usa `sem_veredito_entre(..., ignorar_tranca=True)` em `app/chamados/juizes.py:2117`. Com retenção, só registra trilha e retorna False (`reconciliador.py:521`). Sem retenção, executa a transição antes dos demais efeitos (`:525`). | False se retido ou transição recusada; True ao final (`reconciliador.py:523`, `:526`, `:543`). Teste consumidor existente: `app/chamados/tests/test_c20_juiz3_outbox.py:270`; idempotência em `:299`. Teste da meia-resposta que deixa de segurar: `app/chamados/tests/test_f1nova_b2_leitores.py:61`. Teste específico de barreira K6 neste escritor: **sem teste identificado**. |

`_retratar` não consulta K6: seu corpo consulta a retenção, chama `reconciliar`, rejeita justificativa pendente, resolve perguntas e anexa nota (`app/chamados/reconciliador.py:505`). `reconciliar` consulta motor, premissa, conversa do pai e pergunta viva (`app/chamados/models.py:664`); a guarda de vivas **exclui** as entregues do bloqueio (`app/chamados/models.py:364`).

**Campos escritos e leitores**

- Chamado: `status_local`, `resolvido_em`, `via_encerramento`, `atualizado_em`, `urgencia`, `urgencia_original`; escritor `_gravar_estado` em `app/chamados/models.py:228`, `:244`, `:246`, `:296`. Leitores: crédito/retenção em `app/chamados/juizes.py:2115`; painel por status/data em `app/chamados/services/painel.py:245`; restauração da urgência em `app/chamados/models.py:250`.
- Justificativa: `status='rejeitada'`, `descricao` acrescida; escritor em `app/chamados/reconciliador.py:534`. Portas DP leem `status='pendente'` em `app/ponto/services/justificativa.py:143` e `:278`; o save rejulga célula em `app/ponto/signals.py:161`. Teste do efeito: `app/chamados/tests/test_retratar_rejeita_justificativa.py:48`.
- Pergunta: o S143 já roda no save do chamado (`app/chamados/signals.py:91`, `:117`), antes da chamada explícita do resolver em `app/chamados/reconciliador.py:538`. Resolver consulta C6, tranca e C21 em `app/chamados/services/disputa_emissao.py:1988`, `:1992`, `:2002`; carimba em `:2008`. Escrita efetiva do trio `materializada_em/por/via_resolucao`: `app/chamados/models.py:3218`; persistência e lavra `veredito_lavrado/julgado_em/janela_expira_em`: `:3270`. Meia-resposta conserva instante/texto: o ramo que escreve resposta/validação exige texto (`:3251`). O status do chamado também relavra perguntas não validadas em `app/chamados/models.py:309`.
- Nota: `chamado_id`, `autor_tipo`, `autor_user`, `conteudo`, `anexo_hash` escritos em `app/chamados/services/nota.py:88`, `:103`; leitura do fio em `app/chamados/views.py:372`.
- Auditoria do estado: escrita em `app/chamados/models.py:292`. Retenção: escreve `valor_depois.perguntas_pendentes` somente com os primeiros oito PKs (`app/chamados/reconciliador.py:470`); lê última trilha para deduplicar em `:480`.

**Outros escritores e respectivas guardas existentes**

| Escritor | Guarda |
|---|---|
| `validar_pergunta` | `entrega_que_barra_a_validacao` antes da validação (`app/chamados/services/validacao.py:740`) e novamente depois da materialização (`:777`). Delega a `_k6_que_barra` (`:225`), **somente metades de intervalo fora163**, conforme `app/chamados/services/materializacao.py:792`. |
| `_materializar_uma` | `_competencia_que_barra(..., perguntas=(p,), somente_entregue=True)` antes de qualquer escrita (`app/chamados/services/materializacao.py:1854`); essa composição consulta `_k6_que_barra` em `:825`, com o mesmo recorte fora163. |
| `devolver_paridade_bug_par_103` | `competencia_do_fato` sob trava; sem dia ou entregue retorna sem escrever (`app/chamados/services/validacao.py:338`). |
| `devolver_instante_sem_texto_l130` | `impedimento_estoque_l130` sob trava (`app/chamados/services/validacao.py:432`); consulta `competencia_do_fato` e recusa entregue em `:400` e `:403`. |
| Backfill de decisões | `fio.veto_de_competencia`, em `app/chamados/management/commands/backfill_decisoes_pergunta.py:194`, aplicado em `:256`. |
| Migração do dia da pergunta | Mesmo veto em `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:305`, `:310`; reversão também consulta em `:488`. |
| Rechaveamento condicionado do fio | Veto dos dias gravado e novo em `app/chamados/services/fio.py:1116`. |

A composição reutilizável é dia → janela → K6, já em `app/chamados/services/materializacao.py:776` ou `app/chamados/services/vigia_fio.py:786`; nenhuma exige juiz novo. A guarda `_k6_que_barra` **não serve como veto universal**, porque pula perguntas fora do recorte (`materializacao.py:805`).

**Menor conserto proposto**

Na entrada de `_retratar`, antes da transição e dos receivers, consultar K6 pela composição existente para as perguntas que a unidade alcançará; havendo competência entregue, retornar False e preservar chamado, justificativa, perguntas e notas. É insuficiente barrar apenas `carimbar`: a transição já disparou S143 e relavra (`app/chamados/reconciliador.py:525`; `app/chamados/models.py:296`, `:309`; `app/chamados/signals.py:117`). Proposta, ainda sem implementação/teste **[nao verificado]**.

**Risco de dizer “não toca FechamentoMensal” sem qualificar**

O fechamento das disputas pelo resolver existe (`app/chamados/services/disputa_emissao.py:2113`). O post_save agenda retomada de regularizações confirmadas (`app/chamados/signals.py:122`, `:146`), que pode criar batidas (`app/chamados/services/regularizacao_ext.py:225`, `:77`). `registrar_batida` chama recalculo por evento (`app/ponto/registro_batida.py:180`, `:197`), chegando ao escritor de FechamentoMensal (`app/ponto/services/fechamento.py:1810`, `:898`, `:1160`). Portanto, não há escrita **direta** de FechamentoMensal em `_retratar`, mas existe caminho indireto condicional. Existência de regularizações desse tipo no dado fornecido: **[nao verificado]**.

L-113 altera L-092 e revoga trava geral de recalculo exportado (`app/docs/LEIS.md:155`); não apresentar L-092 isolada como prova de veto universal do código.

## 17/11 08:00 · sessao aa3fa4aafc7c · QUEIMA-UTIL-1711 · sonda · origem=Code

**O raspado contém 19 linhas: 16 `sem_acao_disponivel` e 3 `sem_leitura_sistema` [nao verificado].** O código permite explicar esses baldes; a causa interna específica de cada dia não veio no dado [nao verificado].

As referências **P** e **L** usadas na tabela significam:

- **P — SEM_PORTA:** o leitor não recebeu porta com rota alcançável. `_fio_da_porta` devolve `nada`; todos os dias assim formam grupo D, gravado como `ninguem`. `fio_da_linha` lê esse valor e `DONO_DO_FIO` o apresenta como `sem_acao_disponivel`. Fontes: [fora_da_folha.py:86](app/folha/fora_da_folha.py:86), [fora_da_folha.py:102](app/folha/fora_da_folha.py:102), [retrato_folha.py:376](app/folha/retrato_folha.py:376), [retrato_folha.py:446](app/folha/retrato_folha.py:446), [retrato_folha.py:194](app/folha/retrato_folha.py:194).
- **L — SEM_LEITURA:** a porta publicou desconhecimento, com `nao_sei=True` e sem rota. O leitor devolve `nao_sei`, apresentado como `sem_leitura_sistema`. Fontes: [tarefa_do_dia.py:722](app/chamados/services/tarefa_do_dia.py:722), [fora_da_folha.py:89](app/folha/fora_da_folha.py:89), [retrato_folha.py:195](app/folha/retrato_folha.py:195).

**Obra D1 = `CLASSE-D-NA-FRENTE-DA-E4D`:** seu escopo explícito é “dia-colab sem porta”, com cura por classe pela porta existente; consta registrada, não construída. Isso cobre a classe observada, sem comprovar o reparo específico destas pessoas: [BACKLOG.md:1990](app/docs/BACKLOG.md:1990). O guarda-chuva também aparece como `O261-FIO-MORTO-PELO-RETRATO`, com residual absorvido pela `O261-E-CELULA-20-UM-DONO`: [BACKLOG.md:1088](app/docs/BACKLOG.md:1088), [BACKLOG.md:981](app/docs/BACKLOG.md:981), [GRADE-MESTRA.md:979](app/docs/GRADE-MESTRA.md:979), [GRADE-MESTRA.md:971](app/docs/GRADE-MESTRA.md:971).

Os campos de produção abaixo reproduzem exclusivamente o raspado fornecido [nao verificado].

| n | emp | colNNN | dono | causa | classe | obra |
|---:|---:|---|---|---|---|---|
| 1 | 2 | col1009 | sem_acao_disponivel | P: 3/3 dias sem porta; tipo `falta` [nao verificado] | SEM_PORTA | D1 |
| 2 | 2 | col639 | sem_acao_disponivel | P: 5/5 dias sem porta; tipo `sem_causa` [nao verificado] | SEM_PORTA | D1 |
| 3 | 2 | col496 | sem_acao_disponivel | P: 1/1 dia sem porta; tipo `falta` [nao verificado] | SEM_PORTA | D1 |
| 4 | 2 | col265 | sem_leitura_sistema | L: 1/1 dia sem leitura; tipo `sem_causa`; origem do desconhecimento [nao verificado] | SEM_LEITURA | sem obra específica comprovada [nao verificado] |
| 5 | 2 | col994 | sem_acao_disponivel | P: 1/1 dia sem porta; tipo `folga` [nao verificado] | SEM_PORTA | D1 |
| 6 | 2 | col1043 | sem_acao_disponivel | P: 2/2 dias sem porta; tipo `nao_bate` [nao verificado] | SEM_PORTA | D1 |
| 7 | 2 | col838 | sem_leitura_sistema | L: 1/1 dia sem leitura; tipo `falta`; origem do desconhecimento [nao verificado] | SEM_LEITURA | sem obra específica comprovada [nao verificado] |
| 8 | 2 | col327 | sem_acao_disponivel | P: 1/1 dia sem porta; tipo `sem_causa` [nao verificado] | SEM_PORTA | D1 |
| 9 | 2 | col558 | sem_acao_disponivel | P: 1/1 dia sem porta; tipo `falta` [nao verificado] | SEM_PORTA | D1 |
| 10 | 2 | col97 | sem_acao_disponivel | P: 8/8 dias sem porta; tipo `sem_causa` [nao verificado] | SEM_PORTA | D1 |
| 11 | 2 | col347 | sem_acao_disponivel | P: 1/1 dia sem porta; tipo `sem_causa` [nao verificado] | SEM_PORTA | D1 |
| 12 | 2 | col407 | sem_leitura_sistema | L: 3/3 dias sem leitura; tipo `falta`; origem do desconhecimento [nao verificado] | SEM_LEITURA | sem obra específica comprovada [nao verificado] |
| 13 | 2 | col504 | sem_acao_disponivel | P: 2/2 dias sem porta; tipo `sem_causa` [nao verificado] | SEM_PORTA | D1 |
| 14 | 2 | col719 | sem_acao_disponivel | P: 3/3 dias sem porta; tipo `sem_causa` [nao verificado] | SEM_PORTA | D1 |
| 15 | 2 | col476 | sem_acao_disponivel | P: 1/1 dia sem porta; tipo `folga` [nao verificado] | SEM_PORTA | D1 |
| 16 | 2 | col348 | sem_acao_disponivel | P: 5/5 dias sem porta; tipo `nao_bate` [nao verificado] | SEM_PORTA | D1 |
| 17 | 2 | col246 | sem_acao_disponivel | P: 1/1 dia sem porta; tipo `sem_causa` [nao verificado] | SEM_PORTA | D1 |
| 18 | 3 | col61 | sem_acao_disponivel | P: 1/1 dia sem porta; tipo `folga` [nao verificado] | SEM_PORTA | D1 |
| 19 | 3 | col177 | sem_acao_disponivel | P: 1/1 dia sem porta; tipo `falta` [nao verificado] | SEM_PORTA | D1 |

**QUADRO por classe — unidade: linha/pessoa; contagem do raspado [nao verificado].**

| Classe | emp 2 | emp 3 | emp 4 | Total |
|---|---:|---:|---:|---:|
| SEM_PORTA | 14 | 2 | 0 | **16** |
| SEM_LEITURA | 3 | 0 | 0 | **3** |
| **Total** | **17** | **2** | **0** | **19** |

O limite da conclusão é o seguinte: **`tipo` não explica a ausência de porta**. O lavrador grava dono, ações e tipo separadamente; `sem_causa` também pode resultar de `indeterminado` ou `sem_furo`. Portanto, não há base para transformar os tipos em subclasses causais destes becos: [retrato_folha.py:1248](app/folha/retrato_folha.py:1248), [retrato_folha.py:253](app/folha/retrato_folha.py:253).

Também não se pode traduzir `acoes=[]` em “não existe chamado ou tarefa”: a folha copia apenas a ação publicada pela porta, e a tarefa pode ter ação de sistema sem rota. Fontes: [fora_da_folha.py:223](app/folha/fora_da_folha.py:223), [tarefa_do_dia.py:660](app/chamados/services/tarefa_do_dia.py:660), [tarefa_do_dia.py:683](app/chamados/services/tarefa_do_dia.py:683). Nas linhas `folga`, pode ainda existir o link complementar para o vínculo da escala, embora o fio esteja sem ação: [retrato_folha.py:1005](app/folha/retrato_folha.py:1005), [retrato_folha.py:1066](app/folha/retrato_folha.py:1066).

Para as três linhas SEM_LEITURA, o código admite diversas origens — célula ausente, ata sem lâmpadas, veredito sem lavra vigente ou falha da oferta de Resolver dia —, mas escolher uma delas para cada pessoa seria [nao verificado]. Fontes dos caminhos possíveis: [tarefa_do_dia.py:230](app/chamados/services/tarefa_do_dia.py:230), [tarefa_do_dia.py:240](app/chamados/services/tarefa_do_dia.py:240), [tarefa_do_dia.py:168](app/chamados/services/tarefa_do_dia.py:168), [tarefa_do_dia.py:635](app/chamados/services/tarefa_do_dia.py:635).

_6364 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
