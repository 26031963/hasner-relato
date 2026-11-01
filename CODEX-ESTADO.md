# CODEX-ESTADO

> Ultima resposta processada: **01/11/2026 16:16** · publicado em **01/11/2026 16:16** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 01/11 16:16 · sessao 0144ff325e9e · - · - · origem=Ronald (tmux)

- **Ordem no cartório:** `_turno_encerrado` monta e guarda o mapa uma vez, sob demanda, em `app/ponto/services/cartorio.py:939-943`. No ramo normal ele pode ser chamado em `:1180`, antes da lavra da célula atual em `:1596` (`save=False`) e do `carimbar` em `:1616`. `carimbar` chama a gravação em `app/ponto/portas/celula.py:807`; esta faz `save` e evento no mesmo `atomic`, em `:639-642`. A ressalva é o caráter preguiçoso: não afirmar que o mapa necessariamente foi calculado antes de **todas** as lavras da passagem. (`app/ponto/services/cartorio.py:937-943`, `:1136`, `:1180`)

- **Nunca bateu / dry-run:** a pergunta de encerramento em `app/ponto/services/cartorio.py:1063` está dentro do `if apply_` de `:1040`; vem antes da lavra em `:1075` e do `carimbar` em `:1097-1099`. Nesse ramo o dry-run não executa essa pergunta; é assimetria preexistente, não criada pela linha alterada. (`app/ponto/services/cartorio.py:1040`, `:1063`, `:1075`, `:1097`)

- **Fornecedores de `cels`:** todos os fornecedores externos encontrados carregam do banco: cron em `app/ponto/management/commands/processar_cartorio.py:61-65`; sombra do veredito velho dentro de `atomic` em `app/ponto/management/commands/celula_veredito_velho.py:133-145`; preenchimento de status em `app/ponto/management/commands/lavrar_status_competencia.py:48-56`; régua, restrita às células `veredito='furo'`, em `app/escala/regua_defesa.py:685-693`; unidade em `app/ponto/services/cartorio.py:1739-1754`. Internamente há segunda passada com lista ordenada em `:833` e releitura da lista original em `:898`. Nenhum desses trechos modifica célula em memória antes de passá-la ao julgamento. (mesmas referências)

- **Célula fresca ainda sem save:** há esse contrato na **grade**, que copia células, substitui sua ata em memória e alimenta diretamente `turnos_do_colab`, em `app/escala/utils.py:1829-1847`. Esse caminho não passa por `dias_encerrados`; portanto a alteração não descarta sua alimentação. No cartório, a montagem da grade ocorre em `app/ponto/services/cartorio.py:963`, mas a alimentação de encerramento continua sendo a lista original `cels` em `:942`, não `_cels_juiz` da grade. Uma divergência real dry-run/apply decorrente da nova leitura está [não verificado].

- **(a) segundo juiz: NÃO introduzido.** Os registrados são `turnos_do_colab` em `app/core/juizes.py:196` e `dia_encerrado` em `:223`; o carregador mantém `_dias_encerrados_puro`, que pergunta à mesma função pura por dia. (`app/ponto/turnos.py:2153-2155`, `:2169-2172`)

- **(b) segundo escritor: NÃO introduzido.** A alteração termina nas mesmas funções de leitura e retorna um mapa; a lavra segue pela porta existente. (`app/ponto/turnos.py:2145-2155`, `:2167-2174`; `app/ponto/services/cartorio.py:1596`, `:1616`; `app/ponto/portas/celula.py:709-713`)

- **(c) relógio fora da borda: NÃO introduzido, mas existe no sítio.** `dias_encerrados` já usa `timezone.now()` quando `agora=None`, em `app/ponto/turnos.py:2140-2141`; `dia_encerrado_motivo` também, em `:2094-2095`. O corte exige injeção de instante em juiz/grade/cartório. (`app/docs/CORTES.md:369`) O cartório injeta `agora` na chamada alterada. (`app/ponto/services/cartorio.py:942`)

- **(d) dois carregadores: SIM, preexistentes; regra paralela nova: NÃO.** Casca e lote são declarados como fornecedores do mesmo corpo em `app/ponto/turnos.py:1919-1924` e `:2159-2161`; a alteração passa a deixar o pareador carregar sua alimentação própria em `:2154`. A diferença de janela/estado entre a alimentação externa de encerramento e a carga do pareador permanece possível por desenho; ocorrência material em produção [não verificado].

- **(f) ordem de trava: NÃO alterada pelo diff.** A linha alterada apenas remove um argumento da chamada de leitura. (`app/ponto/turnos.py:2153-2155`) Há uma hipótese preexistente de composição Colaborador → Disputa via cartório, registrada expressamente como não reproduzida; não confundir isso com prova de regressão deste diff. (`app/docs/BACKLOG.md:767`)

- **(g) pouso antes do pré-requisito:** o pré-requisito específico existe: a ficha exige “RED + impacto na sombra” e determina tocar `turnos.py` somente depois do pouso E4-7. (`app/docs/BACKLOG.md:861`) A ficha do E4-7 declara pouso em 31/10, `a62862d4`, suíte 13124 e aprovação. (`app/docs/BACKLOG.md:789`) A execução efetiva dessa sequência e do impacto da cura está [não verificado].

- **Leis:** o grep em `LEIS.md` encontra L-061 (`:95`), L-073 (`:100`), L-085, que nomeia/protege `turnos_do_colab` (`:117`), L-115 (`:146`) e L-113 (`:148`), além das auditorias `:166/:190`. A mudança preserva a pergunta por turno/instante e o dia de início, em vez de acrescentar predicado por data. (`app/ponto/turnos.py:2114-2122`, `:2153-2155`) L-002 proíbe derivação própria (`app/docs/LEIS.md:60`); o corpo segue único (`app/ponto/turnos.py:2161`, `:2170`). L-110 exige casos pela regra antes do código (`app/docs/LEIS.md:136`); L-114 distingue certificado, impacto e termômetro (`:147`). O fonte dos testes declara essa intenção, mas a cronologia real e os resultados executados são [não verificado].

## 01/11 16:15 · sessao 545be1432c25 · - · - · origem=Ronald (tmux)

Censo de produção com `celulas` explícito, incluindo aliases e argumento posicional:

| Chamada | Escritor/forma/janela | Efeito do diff |
|---|---|---|
| `dias_encerrados` no cartório | `{(c.pk, x.data): x}` da lista `cels`; janela entre primeira e última célula, podendo haver lacunas. `app/ponto/services/cartorio.py:919`, `app/ponto/services/cartorio.py:942` | Muda: sem `turnos`, agora o pareador recarrega células com pad. `app/ponto/turnos.py:2153` |
| `dias_encerrados` no calendário | `_cels_mes = celulas_do_periodo([pk], data_ini, data_fim)`, chave composta; fornece também os turnos da mesma janela. `app/colaboradores/services/calendario.py:395`, `app/colaboradores/services/calendario.py:699` | Não muda: `turnos is not None` evita chamada ao pareador nos dois códigos. `app/ponto/turnos.py:2154` |
| `turnos_do_colab` em `dia_encerrado_motivo` | Repasse posicional do parâmetro `celulas`; formato e alcance dependem do chamador, cujo contrato é `{data: CelulaDia}` com pad da janela `[data,data]`. `app/ponto/turnos.py:2090`, `app/ponto/turnos.py:2096`, `app/ponto/turnos.py:1852`, `app/ponto/turnos.py:1845` | Não muda: esta chamada não foi alterada. `app/ponto/turnos.py:2096` |
| `_tdc_pre` na montagem da grade | Converte `_cels_gr` composta para `{data: célula}`; `_cels_gr` vem de `janela_com_pad(_ini_al, ref)`. `app/escala/utils.py:1258`, `app/escala/utils.py:1260`, `app/escala/utils.py:1532` | Não muda: chamada direta ao pareador permanece. `app/escala/utils.py:1532` |
| `turnos_do_colab` após overlays da grade | `{data: célula}`; mesmo pad; fora da montagem usa célula do banco, dentro usa cópia com ata recém-montada, não salva. `app/escala/utils.py:1830`, `app/escala/utils.py:1833`, `app/escala/utils.py:1835`, `app/escala/utils.py:1846`, `app/escala/utils.py:1847` | Não muda: alimentação direta continua preservada. `app/escala/utils.py:1847` |
| Emissor de turno aberto retroativo | `{x.data: x}`, banco cobrindo `[ini−2, fim+2]`, portanto superset do pad `[ini−2, fim+1]`. `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:86`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88`, `app/ponto/turnos.py:1845` | Não muda: chamada direta permanece. `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88` |

Os seis são todos os sítios explícitos encontrados no censo AST da cópia, incluindo aliases de imports e passagem posicional; referências dinâmicas externas à cópia são `[não verificado]`.

`dias_encerrados` escreve seu resultado exclusivamente no corpo compartilhado: percorre dias, pergunta `_dia_encerrado_motivo_puro`, produz motivo quando `motivos=True`, senão `_m is None`. A alimentação composta é lida por `vinculo_do_dia` e pelo julgamento/fim previsto. `app/ponto/turnos.py:2169`, `app/ponto/turnos.py:2170`, `app/ponto/turnos.py:2172`; consulta composta em `app/escala/alimentacao.py:237`; DNA composto em `app/escala/models.py:1095`.

Leitores de produção do retorno da função de módulo:

- Cartório lê bool com default `False`; passa ao classificador e calcula `_em_curso`. `app/ponto/services/cartorio.py:943`, `app/ponto/services/cartorio.py:1180`, `app/ponto/services/cartorio.py:1253`.
- Calendário lê motivos para `_em_curso` e trata `sem_previsto`. `app/colaboradores/services/calendario.py:701`, `app/colaboradores/services/calendario.py:709`.
- `supra_juiz` recebe bool e passa ao classificador. `app/ponto/management/commands/supra_juiz.py:121`, `app/ponto/management/commands/supra_juiz.py:232`.
- `emitir_furo_retroativo` recebe bool e passa ao classificador. `app/ponto/management/commands/emitir_furo_retroativo.py:62`, `app/ponto/management/commands/emitir_furo_retroativo.py:108`.
- `par_completo_da_janela` recebe bool e exclui turnos de dias não encerrados. `app/ponto/turnos.py:2183`, `app/ponto/turnos.py:2186`.

Leitores do método homônimo de lote: status usa motivos; painel usa bool. Esse método chama diretamente o corpo compartilhado, portanto não é afetado pela alteração da casca. `app/ponto/status_dia.py:196`, `app/ponto/status_dia.py:197`, `app/core/services/painel_op.py:334`, `app/ponto/turnos.py:3018`.

Custo: o cartório passa a pagar uma consulta adicional de células por chamada efetiva de `dias_encerrados`, porque agora o pareador recebe `None` e executa `celulas_do_periodo`; essa função é um único SELECT. O cache `_enc` limita a uma carga por execução do corpo, não uma por dia. `app/ponto/turnos.py:2154`, `app/ponto/turnos.py:1882`, `app/escala/alimentacao.py:41`, `app/ponto/services/cartorio.py:939`. Células do pad podem ainda introduzir geradoras adicionais e disparar o SELECT condicional de `mais`; custo efetivo em produção `[não verificado]`. `app/ponto/turnos.py:1885`, `app/escala/alimentacao.py:123`. O lote mantém cargas próprias e corpo compartilhado, sem passar pela casca alterada. `app/ponto/turnos.py:2957`, `app/ponto/turnos.py:2963`, `app/ponto/turnos.py:3018`.

Ordem e memória no cartório:

1. Monta a grade, cujo pareamento final usa cópias com ata fresca em memória. `app/ponto/services/cartorio.py:963`, `app/escala/utils.py:1835`, `app/escala/utils.py:1847`.
2. Consulta encerramento lazy, normalmente em `_em_curso`; este novo pareamento lê células/atas do banco, anteriores à lavra corrente. `app/ponto/services/cartorio.py:1180`, `app/ponto/services/cartorio.py:942`, `app/ponto/turnos.py:1884`, `app/ponto/turnos.py:1937`.
3. Calcula/classifica; somente depois, no apply, lavra com `save=False` e salva via `carimbar`. `app/ponto/services/cartorio.py:1243`, `app/ponto/services/cartorio.py:1593`, `app/ponto/services/cartorio.py:1596`, `app/ponto/services/cartorio.py:1616`, `app/ponto/portas/celula.py:807`.

Portanto existe diferença estrutural entre a ata fresca usada pela grade e a ata persistida usada por esse pareador; resultado incorreto adicional no dado fornecido `[não verificado]`. A célula corrente ainda não foi lavrada quando encerramento é consultado, e a ata fresca da grade tampouco era repassada pelo cartório antes do diff: sua alimentação era a lista original `cels`. `app/ponto/services/cartorio.py:942`, `app/ponto/services/cartorio.py:963`, `app/ponto/services/cartorio.py:1596`.

No ramo `nunca_bateu`, a consulta de encerramento ocorre somente dentro de `apply_`, antes da lavra; dry-run desse ramo não passa por essa consulta. `app/ponto/services/cartorio.py:1040`, `app/ponto/services/cartorio.py:1063`, `app/ponto/services/cartorio.py:1075`.

## 01/11 16:15 · sessao b7e648826626 · - · - · origem=Ronald (tmux)

Análise estática; não executei testes nem mutantes: execução e resultado efetivo [não verificado]. [nao verificado]

| Teste novo | Mutante que detecta pela estrutura/asserts | Mutante que deixa passar | [nao verificado]
|---|---|---|
| `test_regra` | Remover `mais=self._geradoras` só do lote; ignorar geradora e usar vínculo ativo; erro compartilhado que fecha Y na saída de intervalo ou mantém X aberto, pois exige aberto X=False/Y=True e motivos literais nos três instantes. `app/ponto/tests/test_e4_carga_paridade.py:129-143`; `app/ponto/turnos.py:2963-2964`. | Retornar bool invertido quando `motivos=False`: só usa `motivos=True`. Também erro só em dia diferente de D ou ramo sem previsto. `app/ponto/tests/test_e4_carga_paridade.py:107`, `:115`, `:143`. |
| `test_paridade_carga_x_casca_em_fixture_mista` | Divergência de carregamento só do lote, como retirar `mais`, mudar a identidade da saída ou perder uma batida: compara todos os seis campos do turno, mapas de motivos e dia por pk nas duas janelas. `app/ponto/tests/test_e4_carga_paridade.py:51-56`, `:106-124`, `:145-148`. | Erro comum aos corpos compartilhados que mantém ambos iguais. Casca e lote chamam `_turnos_do_colab_puro` e `_dias_encerrados_puro`. `app/ponto/turnos.py:1913`, `:2153-2155`, `:3009-3019`. |
| `test_MORDE_carga_sem_mais` | É controle positivo: injeta retirada de `mais`, exige que o patch tenha sido chamado e que Y difira em turnos. `app/ponto/tests/test_e4_carga_paridade.py:155-165`. | **Sozinho, não mata uma retirada permanente de `mais` no código:** com lote já defeituoso e casca correta, a divergência exigida continua existindo. Quem mata essa retirada permanente são `test_regra` e `test_paridade`. `app/ponto/tests/test_e4_carga_paridade.py:139-148`, `:158-165`. |
| `test_RED_alimentado_igual_a_juiza_e_ao_lote` | Restaurar a linha antiga que repassa o dict composto ao pareador: constrói exatamente esse dict, compara todos os dias contra lote e D contra singular nos três instantes e duas janelas. `app/ponto/tests/test_bug_enc_celulas_forma.py:32-44`; a composição adicional da chave ocorre em `app/ponto/turnos.py:1954-1958`, enquanto o juiz busca `(colaborador_id,data)` em `app/escala/alimentacao.py:237`. | Erro compartilhado no julgamento puro, igualdade preservada; bool errado no modo sem motivos; erro apenas no ramo de `turnos=` fornecido. Todas as chamadas do teste usam `motivos=True` e nenhuma fornece `turnos`. `app/ponto/tests/test_bug_enc_celulas_forma.py:33`, `:43-44`; `app/ponto/turnos.py:2154`, `:2170-2172`. |
| `test_RED_regra_do_veredito_do_cartorio` | Restaurar repasse antigo; usar vínculo ativo no pareador; ignorar a pausa de Y: fixa X encerrado e Y `'turno_aberto_relogio'` às 08h. `app/ponto/tests/test_bug_enc_celulas_forma.py:46-50`. | Erro só às 03h/12h, só em Z, nos outros dias ou na conversão bool. O teste consulta apenas X/Y, D e `INSTANTES[1]`, através da helper com motivos. `app/ponto/tests/test_bug_enc_celulas_forma.py:33`, `:49-50`. |

O patch **alcança a carga do lote**. `_lote()` instancia `CargaDaJanela` dentro do `with`; `CargaDaJanela.__init__` faz import local de `escalas_do_periodo_por_colab` e chama a referência importada com `mais=self._geradoras`. Portanto busca o atributo já patchado de `escala.alimentacao`. `app/ponto/tests/test_e4_carga_paridade.py:111-113`, `:159-161`; `app/ponto/turnos.py:2950-2951`, `:2963-2964`.

A casca medida antes do patch fica materializada e intacta: assinatura contém datas, pks, tuplas e bools; motivos são calculados imediatamente; mapa de dias contém pk→date. Não há queryset pendente nesses resultados. `app/ponto/tests/test_e4_carga_paridade.py:51-56`, `:103-109`, `:158`; `app/ponto/turnos.py:2172-2174`, `:2925-2933`. Medir a casca *dentro* do patch também a mutaria, pois `escalas_do_periodo` resolve a função plural pelo global do módulo. `app/escala/alimentacao.py:153-154`.

Os testes novos não cobrem célula fresca ainda não salva, execução real de `cartorio`, orçamento de consultas ou ramo sem motivos: a fixture salva células, atualiza a geradora por queryset e a helper relê do banco; nenhum dos asserts mede consultas. `app/ponto/tests/test_e4_carga_paridade.py:92-96`, `:127-165`; `app/ponto/tests/test_bug_enc_celulas_forma.py:32-50`. Também não há célula no pad fora de D: todas as geradoras/células da fixture são criadas para D; assim remover só alimentação das células do pad pode passar nesta fixture, embora as batidas de D+1 existam. `app/ponto/tests/test_e4_carga_paridade.py:77-79`, `:92-94`.

Não há leitura direta do relógio real nos testes novos: D e INSTANTES são construídos de constantes; ambas as classes são congeladas; chamadas de encerramento recebem `agora` explícito. `app/ponto/tests/test_e4_carga_paridade.py:39-48`, `:100`, `:107`, `:115`; `app/ponto/tests/test_bug_enc_celulas_forma.py:24`, `:33`, `:44`. Os defaults `timezone.now()` dessas portas só rodam se `agora is None`, condição não usada nesses testes. `app/ponto/turnos.py:2094-2095`, `:2140-2141`.

## 01/11 16:14 · sessao 79df0d6c3572 · - · - · origem=Ronald (tmux)

| Achado | Classe proposta | Pergunta e resposta/triagem | Evidência da cópia |
|---|---|---|---|
| 6 | **(b)** | `codex_v1.q`, item 7: “Algum chamador [...] (codigo, template, command, string em getattr) sobrou?”; `codex_v1.r`, §7 identifica exatamente as três referências. `triagem_v1.md`, item 7: “INCOMPLETO declarado. E instrumento (`bin/`) e doc, com pouso proprio (RAIAS 5).” | `bin/esmeril_chamado_censo.py:135`; `bin/esmeril_chamado_censo_de_hoje.txt:38`; `app/docs/DOSSIES.md:46`. |
| 7 | **(a)** | `codex_v1.q`, item 6: “selo/RED sem mordida”; `codex_v3.q`, item 4 repete para o “diff total”. Resposta ou triagem sobre a perda da testemunha viva da classe (f): **nenhum**. É inspeção estrutural de teste, acessível na cópia, não depende de execução histórica de RED. | `app/core/tests/test_censo_escritas.py:202` explica a troca; `:210`–`:213` exige chamada AST; `:214`–`:217` procura genericamente `update()` censado sem `[relacionado]`. |
| 8 | **(e)**, pela leitura estrita da pergunta | Nenhuma pergunta pede custo ou repetição do C6. A mais próxima, `codex_v1.q`, item 3, menciona “memo `_pv_memo`”, mas pergunta se a releitura resulta **VIVA**, não se há duas avaliações. `codex_v1.r`, §2 descreve “antes salvava depois do pré-C6”, e §8 diz “Já exigia C6 morto antes”; não aponta o custo nem há triagem desse custo. | Primeira consulta: `app/chamados/reconciliador.py:398`; chamada da porta: `:405`; segunda consulta: `app/chamados/models.py:2565`; instante/via mudam a chave: `app/chamados/juizes.py:790`–`:791`. |
| 9 | **(e)** | Nenhuma `.q` pede auditoria de vocabulário do commit ou palavra proibida em comentário. Menções a `fallback` em `codex_v1.r`, §6, e `codex_v2.r`, §5, tratam o **relógio**, não esta ocorrência lexical. Resposta/triagem sobre este achado: **nenhum**. | Comentário: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:404` e `app/chamados/tests/test_porta_desfecho_pergunta.py:204`. Corpo do commit não disponível nesta cópia sem `.git`: **[nao verificado]**, além do relato fornecido pelo opus. |

As referências aos anexos acima usam seções/itens porque eles só foram entregues inline; o agente principal precisa aplicar sua convenção de numeração às linhas dos anexos. **[nao verificado]**

## 01/11 16:14 · sessao b71e20460e38 · - · - · origem=Ronald (tmux)

Proposta para 1–5: **a, b, b, b, b**; o #5 exige ressalva sobre ausência de motivo específico da triagem. (`CLAUDE.md:565`.)

1. **(a), tema presente, bypass não apontado.**  
   Pergunta: `codex_v1.q`, nº 6: “selo/RED sem mordida (o selo AST de test_o261e_f2_porta_carimbo.py morde mutante real?)”; nº 2 também pede “Para cada chamador de `carimbar` / `carimbar_identidade`...”. Respostas: **nenhum** trecho aponta o bypass mediante chamada de `_escrever_carimbo` ou a ausência de restrição dos chamadores de identidade; os achados sobre o selo tratam formas de escrita que escapam. O selo permite `_escrever_carimbo` e verifica atribuições; o método escreve sem C6, enquanto o C6 está em `carimbar`. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:44`; `app/chamados/models.py:2527`; `app/chamados/models.py:2565`.)  
   Atenção: a cura literal proposta pelo opus restringindo `_escrever_carimbo` apenas a dois chamadores excluiria também as chamadas legítimas atuais de reversão e descarimbo. Não é necessário julgar a cura para classificar o achado. (`app/chamados/models.py:2582`; `app/chamados/models.py:2611`.)

2. **(b), visto e reduzido a nota.**  
   Pergunta: `codex_v1.q`, nº 2: comportamento observável “quando a porta RECUSA”; nº 8 inclui “ausencia/declaracao”. Resposta: `codex_v1.r`, §2, linha da tabela `_materializar_ausencia_de_resposta`: **“Ignora o booleano. A ausência já criada/reaberta continua”**; `codex_v3.r`, §2: **“recusas individuais podem reduzir o conjunto carimbado”**. Triagem: `triagem_v3.md`, item 2c: **“Nota. O Codex mesmo diz que o conjunto segue restrito ao efetivamente carimbado.”** Esse é o motivo dado para não promover o tema a correção. O retorno é ignorado e a consulta das absorvidas exige carimbo não nulo. (`app/chamados/services/materializacao.py:523`; `app/chamados/services/materializacao.py:571`.) A expressão específica “sem trilha/log” não é formulada nas respostas fornecidas: **[nao verificado]**.

3. **(b), os dois componentes foram vistos.**  
   Pergunta: `codex_v1.q`, nº 3: gravação seguida de releitura VIVA; nº 4: “instancia em memoria diferente do banco”; nº 6: ordem de trava. Resposta: `codex_v1.r`, §3: **“sem reler ou travar a pergunta”**; §4: **“exceção da lavra ou do `save`, depois da aprovação, fica fora do `try` de reversão.”** Triagem: `triagem_v1.md`, item 3a, e `triagem_v2.md`, 5b: janela anterior, travar mudaria ordem de sete chamadores; `triagem_v3.md`, 3b: **“Nao e regressao; vai a fila”**, reconhecendo que o banco volta e a memória não. A guarda encerra antes de lavra/save. (`app/chamados/models.py:2564`; `app/chamados/models.py:2572`; `app/chamados/models.py:2576`.)

4. **(b), relógio visto e remetido a complemento.**  
   Pergunta: `codex_v1.q`, nº 6: **“relogio lido fora da borda”**; repetida nas voltas seguintes. Resposta: `codex_v1.r`, §6: **“Os novos métodos têm `now()` de fallback”**; `codex_v3.r`, §4: **“Presente, embora encaminhado”**. Triagem: `triagem_v2.md`, 5a: **“a leitura do relogio MUDOU DE SITIO, nao nasceu”**, remetendo ao complemento RELOGIO-INJETADO; `triagem_v3.md`, 3c, confirma o encaminhamento. Os fallbacks existem; `encerrar_fantasma` chama a porta sem `quando`. (`app/chamados/models.py:2550`; `app/chamados/models.py:2602`; `app/chamados/models.py:2648`.) O subdetalhe “ignora o agora do chamador” não foi explicitamente nomeado nas respostas: **[nao verificado]**.

5. **(b), com ressalva: descarte agregado, motivo específico ausente.**  
   Pergunta: `codex_v1.q`, nº 6: **“derivacao paralela de fonte unica”**. Resposta: `codex_v1.r`, §6, derivação paralela: **“Permanece a separação já existente entre identidade no C6, no queryset e na lista da relavra.”** O próprio trecho cita os dois sítios exatos do opus. Triagem: `triagem_v2.md`, item 6: **“2o juiz, derivacao paralela e 2o escritor explicito: nenhum.”** Isso registra o encerramento agregado do tema, mas **motivo específico da triagem para manter as listas 5×4: [nao verificado]**. Não atribuir à triagem o argumento de preexistência que aparece na resposta Codex. A divergência está confirmada: o C6 inclui `VIA_COMPETENCIA_TRANCADA`; a lista da relavra tem quatro vias. (`app/chamados/juizes.py:1066`; `app/chamados/services/relavra_via.py:166`.)

## 01/11 15:38 · sessao 8e933ad4f1a9 · O261-E-F2 · verificador · origem=Code

**Não aprovo o selo como prova de escritor único:** a árvore passa, mas formas válidas de escrita ainda escapam, e o resolvedor produz `RecursionError` em programas finitos. A falha está na resolução de chaves e no reconhecimento das chamadas. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:96, app/chamados/tests/test_o261e_f2_porta_carimbo.py:107, app/chamados/tests/test_o261e_f2_porta_carimbo.py:142)

1. **Detector executado em memória**

Executei os 12 testes da classe estrutural: zero falhas e zero erros. Na varredura real, obtive seis ocorrências para o carimbo e quatro para a via, com `fora=[]` e nenhuma porta declarada ausente; duas das seis ocorrências do carimbo pertencem à regularização externa. Isso confirma a passagem dos testes atuais, mas os contraexemplos abaixo invalidam sua suficiência. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:188, app/chamados/tests/test_o261e_f2_porta_carimbo.py:44, app/chamados/services/regularizacao_ext.py:198)

Nos mutantes, `F` abaixo representa tanto `materializada_em` quanto `via_resolucao`; ambos produziram os mesmos resultados. As escritas por chave local e `dict` posicional também foram executadas contra um receptor em memória, confirmando que entregam o campo ao método enquanto o detector retorna `[]`. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:107, app/chamados/tests/test_o261e_f2_porta_carimbo.py:149)

| Forma que escapou | Resultado | Causa no detector |
|---|---|---|
| `campo='F'; qs.update(**{campo: None})` | `[]` | A chave do literal precisa ser constante; `_resolve` não é aplicado à chave. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:114) |
| `qs.update(**dict([('F', None)]))` | `[]` | A análise de `dict` só examina argumentos nomeados, ignorando o argumento posicional. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:116) |
| `PD=PerguntaDisputa; p=PD(F=None); p.save()` | `[]` | O nome do construtor não é resolvido por alias. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:144) |
| `for campo in ['F']: setattr(p, campo, None)` | `[]` | `_locais` não coleta os valores atribuídos pelo `for`. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:70) |
| `p.__setattr__('F', None); p.save()` | `[]` | Só a chamada denominada `setattr` recebe esse tratamento. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:145) |
| `d={}; d \|= {'F': None}; qs.update(**d)` | `[]` | `_locais` não incorpora a união por `AugAssign`. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:70) |
| Dicionário definido na função externa e usado por uma função interna | `[]` | A entrada na função interna substitui o ambiente local, perdendo a variável capturada. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:127) |

Essas lacunas excedem as exclusões declaradas de `obj.__dict__` e `SET_NULL`; não encontrei com isso um escritor clandestino **existente** na produção da cópia. A existência dessas formas no acervo de produção é [não verificado]. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:85, app/chamados/tests/test_o261e_f2_porta_carimbo.py:169)

Este programa executa normalmente, mas o detector levantou `RecursionError`, tanto com `{**d}` quanto com `dict(**d)` na terceira linha. `_resolve` protege ciclos entre nomes; `_chaves_de` reinicia essa proteção ao seguir o espalhamento. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:99, app/chamados/tests/test_o261e_f2_porta_carimbo.py:113, app/chamados/tests/test_o261e_f2_porta_carimbo.py:120)

```python
def mutante(qs):
    d = {'materializada_em': None}
    d = {**d}
    qs.update(**d)
```

Também medi expansão exponencial: cadeias com duas atribuições iguais por nível produziram 256, 4.096 e 65.536 achados para 8, 12 e 16 níveis. O resolvedor percorre todas as definições, sem deduplicação ou memorização; portanto pode consumir tempo e memória desproporcionais mesmo sem ciclo. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:73, app/chamados/tests/test_o261e_f2_porta_carimbo.py:101)

2. **Escritores e leitores dos sete campos**

Na tabela, “escritor” inclui atribuição em memória e persistência delegada; os chamadores que escolhem a operação continuam nos serviços, mas a atribuição do carimbo foi centralizada no modelo. (app/chamados/models.py:2527, app/chamados/models.py:2576, app/chamados/models.py:2626)

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `materializada_em` | `_escrever_carimbo`; limpeza em lote. (app/chamados/models.py:2529, app/chamados/models.py:2627) | C6/lastro; estado, fechamento e ressuscitação; materialização, emissão, reconciliação e validação; mesa, cache e procedência. (app/chamados/juizes.py:624, app/chamados/juizes.py:1077, app/chamados/models.py:1421, app/chamados/models.py:1768, app/chamados/models.py:2477, app/chamados/services/materializacao.py:510, app/chamados/services/disputa_emissao.py:1539, app/chamados/reconciliador.py:583, app/chamados/services/validacao.py:182, app/chamados/services/raiox.py:133, app/chamados/veredito_lavrado.py:163, app/ponto/services/procedencia_batida.py:74) |
| `materializada_por` | `_escrever_carimbo`, pelo `_id`; limpeza em lote; exclusão do usuário por `SET_NULL`. (app/chamados/models.py:2530, app/chamados/models.py:2627, app/chamados/models.py:2762) | Preservação/reversão da porta; fotos de auditoria; passivo; procedência. (app/chamados/models.py:2552, app/chamados/models.py:2600, app/chamados/services/materializacao.py:618, app/chamados/reconciliador.py:1091, app/chamados/services/passivo_validado.py:83, app/ponto/services/procedencia_batida.py:72) |
| `via_resolucao` | `_escrever_carimbo`; limpeza em lote; `encerrar_por_fato_no_espelho`; `relavrar`. (app/chamados/models.py:2531, app/chamados/models.py:2627, app/chamados/models.py:2636, app/chamados/services/relavra_via.py:123) | C6/lastro, reaberturas e idempotência; emissão, absorção, deduplicação e relavra; explicação, tranca, fábrica, cartório, flip e passivo. (app/chamados/juizes.py:635, app/chamados/juizes.py:1071, app/chamados/models.py:2674, app/chamados/models.py:2681, app/chamados/services/disputa_emissao.py:1628, app/chamados/services/materializacao.py:572, app/chamados/services/fio.py:882, app/chamados/services/relavra_via.py:116, app/chamados/services/explicador.py:95, app/chamados/services/competencia_trancada.py:244, app/chamados/services/fabrica_da_celula.py:104, app/ponto/services/cartorio.py:1429, app/ponto/services/flip_auto.py:129, app/chamados/services/passivo_validado.py:141) |
| `respondida_em` | Resposta normal, automática e recuperação pela trilha; reabertura; porta do carimbo; limpeza em lote. Há também restauração e cópia em memória. (app/chamados/services/disputa_emissao.py:1248, app/chamados/services/disputa_emissao.py:1394, app/chamados/services/disputa_emissao.py:1410, app/chamados/services/disputa_emissao.py:1358, app/chamados/models.py:2558, app/chamados/models.py:1488, app/chamados/models.py:2583, app/chamados/models.py:1441) | C6, fila e corrigíveis; estado e bloqueio de fechamento; validação, materialização, emissão, fio e cache. Também API, cobrança, vigias, telas e relatórios. (app/chamados/juizes.py:1047, app/chamados/juizes.py:1277, app/chamados/juizes.py:1284, app/chamados/models.py:1769, app/chamados/models.py:2479, app/chamados/services/validacao.py:363, app/chamados/services/materializacao.py:1259, app/chamados/services/disputa_emissao.py:1559, app/chamados/services/fio.py:336, app/chamados/veredito_lavrado.py:167, app/api/views.py:1009, app/chamados/services/cobranca.py:229, app/chamados/services/vigia_fio.py:630, app/chamados/views.py:1280, app/chamados/relatorios_disputa.py:20) |
| `validada_em` | Validação/devolução; invalidação por resposta nova; reabertura; autovalidação da porta; limpeza em lote; cópia/restauração em memória. (app/chamados/services/validacao.py:131, app/chamados/services/validacao.py:188, app/chamados/services/disputa_emissao.py:1260, app/chamados/services/disputa_emissao.py:1358, app/chamados/models.py:2561, app/chamados/models.py:1488, app/chamados/models.py:1440, app/chamados/models.py:2584) | Primeiro degrau do C6; estado e fechamento; validação, materialização, emissão e reconciliação; declaração, verbos, painel, vigia, flip e procedência. (app/chamados/juizes.py:1033, app/chamados/models.py:2475, app/chamados/models.py:1768, app/chamados/services/validacao.py:366, app/chamados/services/materializacao.py:1260, app/chamados/services/disputa_emissao.py:1620, app/chamados/reconciliador.py:584, app/chamados/services/declaracao_texto.py:208, app/chamados/services/verbos.py:246, app/chamados/services/painel.py:268, app/chamados/services/vigia_carimbo.py:98, app/ponto/services/flip_auto.py:130, app/ponto/services/procedencia_batida.py:74) |
| `validada_por` | Mesmos atos de validação, invalidação, reabertura e autovalidação; restauração em memória; `SET_NULL`. (app/chamados/services/validacao.py:132, app/chamados/services/validacao.py:189, app/chamados/services/disputa_emissao.py:1261, app/chamados/services/disputa_emissao.py:1358, app/chamados/models.py:2562, app/chamados/models.py:1488, app/chamados/models.py:2585, app/chamados/models.py:2742) | Fotos de validação, resposta e reabertura; porta; auditoria da absorção; verbos, painel, passivo e procedência. (app/chamados/services/validacao.py:139, app/chamados/services/disputa_emissao.py:1250, app/chamados/services/disputa_emissao.py:1373, app/chamados/models.py:2553, app/chamados/services/materializacao.py:616, app/chamados/services/verbos.py:240, app/chamados/services/painel.py:260, app/chamados/services/passivo_validado.py:83, app/ponto/services/procedencia_batida.py:72) |
| `veredito_lavrado` | `lavrar`, persistência individual/lote e atribuição do backfill. A nova porta solicita a lavra e inclui o campo no mesmo `save`. (app/chamados/veredito_lavrado.py:227, app/chamados/veredito_lavrado.py:231, app/chamados/veredito_lavrado.py:207, app/chamados/management/commands/backfill_veredito_lavrado.py:138, app/chamados/models.py:2572) | Validade do cache, mesa, comparação da relavra, passivo, backfill e auditoria. (app/chamados/veredito_lavrado.py:179, app/chamados/services/raiox.py:134, app/chamados/services/raiox.py:182, app/chamados/services/relavra_via.py:122, app/chamados/services/passivo_validado.py:83, app/chamados/management/commands/backfill_veredito_lavrado.py:120, app/chamados/management/commands/auditar_invariantes_chamados.py:1169) |

Outros consumidores diretos de `respondida_em` incluem declaração textual, becos, furo sem canal, aviso, verbos, ações, cobrança manual, mensageria, suprajuiz e templates. (app/chamados/services/declaracao_texto.py:363, app/chamados/services/becos.py:30, app/chamados/services/furo_sem_canal.py:81, app/chamados/services/aviso_pendencia.py:13, app/chamados/services/verbos.py:311, app/chamados/acoes_disputa.py:95, app/chamados/views_cobrar.py:169, app/api/views_mensageria.py:569, app/ponto/supra_juiz.py:515, app/templates/chamados/partials/_pergunta_card.html:24)

**Sim, leitores intactos passam a receber valores diferentes:**

- **Fecho sem credor:** a recusa preserva texto sem `respondida_em`; a fila exclui a pergunta, a validação devolve `sem_resposta`, `.estado` permanece pendente e o texto bloqueia fechar a disputa. O teste foi invertido para aceitar essa preservação; os leitores não foram migrados. Incidência em produção: [não verificado]. (app/chamados/services/disputa_emissao.py:1638, app/chamados/juizes.py:1284, app/chamados/services/validacao.py:363, app/chamados/models.py:2479, app/chamados/models.py:1768, app/chamados/tests/test_o261_resposta_vazia.py:107)
- **Autor após ressuscitação:** agora `materializada_por` também fica nulo. `procedencia` passa a devolver outro `quem` quando dependia desse autor; não há migração desse leitor. A frase já podia indicar ausência de validação porque a data já era apagada. Impacto visível adicional em produção: [não verificado]. (app/chamados/models.py:1488, app/ponto/services/procedencia_batida.py:72, app/ponto/services/procedencia_batida.py:90)
- **Absorção por ausência:** recusas individuais podem reduzir o conjunto carimbado; `perguntas_absorvidas_por` continua selecionando pelo grupo de instante e via. Isso altera o conjunto devolvido, mas não demonstra erro: ele continua restrito às perguntas efetivamente carimbadas. (app/chamados/services/materializacao.py:523, app/chamados/services/materializacao.py:569, app/chamados/services/materializacao.py:573)

A lavra granular passou para depois da autovalidação, mas não demonstrei diferença de `veredito_lavrado` causada por essa ordem: o juiz da lavra não consulta `respondida_em`, `validada_em` ou `validada_por`. (app/chamados/models.py:2556, app/chamados/models.py:2574, app/chamados/veredito_lavrado.py:219, app/chamados/juizes.py:610)

3. **Justificativas das triagens**

Há duas justificativas que precisam de correção e uma conclusão de cura que os mutantes contradizem:

- **V2, item 3 — “vias derivadas só existem depois do carimbo”: não é garantia da porta.** `relavrar` aceita a via sem verificar `materializada_em`; o modelo admite data nula e não declara restrição conjunta. O padrão dos escritores atuais não prova a premissa para qualquer entrada/acervo. Acervo contrário em produção: [não verificado]. (app/chamados/services/relavra_via.py:116, app/chamados/services/relavra_via.py:123, app/chamados/models.py:2298, app/chamados/models.py:2756, app/chamados/models.py:2776)
- **V1, nota 4 — “o atomic do chamador cobre”: excessivo para reversão em memória.** A restauração só cobre exceção/recusa do C6; lavra e `save` ficam depois dessa guarda. O rollback não executa `_desfazer_em_memoria`, e o refresh posterior da recusa só recarrega três campos. Não demonstrei uma regressão nova desse problema. (app/chamados/models.py:2564, app/chamados/models.py:2572, app/chamados/models.py:2579, app/chamados/services/materializacao.py:1024)
- **V2, itens 1/6 — selo “curado”: conclusão insuficiente.** As quatro formas acrescentadas estão cobertas, mas permanecem os escapes e a recursão demonstrados no item 1 desta resposta. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:246, app/chamados/tests/test_o261e_f2_porta_carimbo.py:107)

Os demais encaminhamentos principais têm apoio, com estes limites:

- **Limbo:** “não inventa hora” está explícito; completar o instante exige testemunha da trilha. Isso sustenta encaminhar a decisão, mas não elimina a mudança funcional dos leitores descrita acima. (app/chamados/services/disputa_emissao.py:1400, app/chamados/services/disputa_emissao.py:1403, app/chamados/services/validacao.py:363)
- **Relógio para complemento:** o corte autoriza complemento da obra dona; as leituras internas continuam presentes. Encaminhado não significa resolvido. (app/docs/CORTES.md:369, app/docs/CORTES.md:380, app/chamados/models.py:2550, app/chamados/models.py:2602)
- **Concorrência para fila:** a porta continua julgando e salvando a instância recebida, sem trava própria. Existem sete funções chamadoras da porta não identidade; afirmar que qualquer solução necessariamente mudaria a ordem de trava de todas elas é [não verificado]. (app/chamados/models.py:2552, app/chamados/models.py:2576, app/chamados/models.py:2648, app/chamados/reconciliador.py:405, app/chamados/reconciliador.py:618, app/chamados/services/disputa_emissao.py:1566, app/chamados/services/materializacao.py:523, app/chamados/services/materializacao.py:1166, app/chamados/services/materializacao.py:1313)
- **Referências obsoletas:** o instrumento ainda declara a função removida; portanto o item permanece incompleto, como reconhecido na triagem. (bin/esmeril_chamado_censo.py:135, app/chamados/models.py:2501)

4. **Classes de reprovação no diff total**

| Classe | Resultado |
|---|---|
| Segundo juiz | Não identificado nos caminhos alterados: a porta chama C6 e a lavra chama o juiz de lastro existente. (app/chamados/models.py:2565, app/chamados/veredito_lavrado.py:219) |
| Segundo escritor fora da porta | Nenhum escritor explícito encontrado pela varredura atual; a exclusividade não está provada porque os mutantes escapam. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:169, app/chamados/tests/test_o261e_f2_porta_carimbo.py:149) |
| Relógio fora da borda | **Presente**, embora encaminhado: fallback nas duas portas e relógio próprio da lavra. (app/chamados/models.py:2550, app/chamados/models.py:2602, app/chamados/veredito_lavrado.py:228) |
| Derivação paralela nova | Não identificada: a lista de identidade vem da autoridade existente, e o julgamento continua delegado. (app/chamados/models.py:2524, app/chamados/models.py:2565) |
| Selo/RED sem mordida suficiente | **Presente:** os testes passam apesar dos escapes e do resolvedor vulnerável à recursão. (app/chamados/tests/test_o261e_f2_porta_carimbo.py:188, app/chamados/tests/test_o261e_f2_porta_carimbo.py:107) |
| Ordem de trava | A porta não acrescenta trava; inversão nova ou deadlock concreto: [não verificado]. (app/chamados/models.py:2533) |
| Pouso antes do pré-requisito | Não demonstrado no plantio granular: a recusa levanta `_Reprovada` dentro do `atomic`. Pouso operacional: [não verificado]. (app/chamados/services/materializacao.py:1109, app/chamados/services/materializacao.py:1166, app/chamados/services/materializacao.py:1172) |

Há ainda uma limitação importante da aprovação: com texto e `resposta='validar'`, a porta preenche `validada_em` **antes** do C6; seu primeiro degrau então devolve morta. Nesse caminho, a nova consulta não demonstra lastro independente nem pode reprovar pela lei do dia. Isso segue a aplicação do estado completo anunciada pela porta, portanto não o classifico isoladamente como segundo juiz ou pouso indevido. (app/chamados/models.py:2541, app/chamados/models.py:2560, app/chamados/models.py:2565, app/chamados/juizes.py:1033)

## 01/11 15:36 · sessao 6f098fe6ed12 · - · - · origem=Ronald (tmux)

Achado principal: há leitor não migrado com consequência funcional. No fecho sem credor, a recusa mantém texto e `respondida_em=None` (`app/chamados/services/disputa_emissao.py:1638`); a fila exige instante (`app/chamados/juizes.py:1284`), a validação retorna `sem_resposta` (`app/chamados/services/validacao.py:363`) e o fecho da disputa continua bloqueado pelo texto (`app/chamados/models.py:1768`). Incidência em produção: [não verificado].

A porta autoescreve `validada_em` antes de consultar C6 (`app/chamados/models.py:2561`), cujo primeiro ramo devolve morta quando esse campo existe (`app/chamados/juizes.py:1033`). Portanto `resposta='validar'` impede a nova guarda de reprovar por vitalidade, apesar da alegação do comentário de `app/chamados/services/materializacao.py:520`.

Procedência tem consumidores reais no calendário e espelho (`app/colaboradores/services/calendario.py:725`, `app/ponto/services/espelho.py:767`). Zerar `materializada_por` na ressuscitação muda o `quem` lido (`app/ponto/services/procedencia_batida.py:72`); porém `quando` já era nulo antes, e a frase já caía em “sem carimbo de validação” (`app/ponto/services/procedencia_batida.py:90`). Impacto visível adicional: [não verificado].

Mover a lavra para depois de completar resposta/validação não altera diretamente seu julgamento: `lavrar` chama o juiz de lastro (`app/chamados/veredito_lavrado.py:219`), que não consulta esses três campos (`app/chamados/juizes.py:610`). O C6 acrescentado pode carregar caches relacionais antes da lavra (`app/chamados/juizes.py:812`); não encontrei alteração adicional demonstrável do resultado da lavra por essa ordem. [não verificado]

## 01/11 15:36 · sessao bf24d52218f0 · - · - · origem=Ronald (tmux)

Triagens:

- **V2, item 3: motivo não comprovado.** `relavrar` só exige via em `VIAS_DERIVADAS`, sem exigir carimbo (`app/chamados/services/relavra_via.py:116`), e grava nova via em `:123`. O modelo permite via preenchida (`app/chamados/models.py:2298`) e `materializada_em=NULL` (`:2756`); sua `Meta` não declara restrição entre eles (`:2776`). Portanto, “essas vias só existem depois do carimbo” é premissa dos escritores atuais, não garantia da porta. Existência de acervo contrário: **[não verificado]**.

- **Limbo: encaminhamento tem apoio.** A recuperação exige instante testemunhado pela trilha, explicitamente “não inventa hora” (`app/chamados/services/disputa_emissao.py:1400`). A validação recusa ausência de instante (`app/chamados/services/validacao.py:363`), enquanto texto cru bloqueia o fechamento (`app/chamados/models.py:1767`).

- **Relógio: encaminhamento tem apoio, mas a classe continua presente.** O corte define injeção e borda (`app/docs/CORTES.md:369`) e permite complemento da obra dona (`:380`). As portas leem relógio internamente (`app/chamados/models.py:2550`, `:2602`); C6 e lavra também (`app/chamados/juizes.py:539`; `app/chamados/veredito_lavrado.py:228`). Selo executável específico RELOGIO-INJETADO: **[não verificado]**.

- **Concorrência: sete funções chamadoras confirmadas**, com oito sítios (`app/chamados/models.py:2648`; `app/chamados/reconciliador.py:405`, `:618`; `app/chamados/services/disputa_emissao.py:1566`, `:1638`; `app/chamados/services/materializacao.py:523`, `:1166`, `:1313`). A porta não trava nem relê (`app/chamados/models.py:2552`). O diff fornecido substitui escritas das mesmas instâncias; ocorrência de corrida/deadlock: **[não verificado]**.

- **V1, nota 4: justificativa excessiva.** A restauração cobre exceção/recusa do C6 (`app/chamados/models.py:2564`); lavra e `save` ficam depois da guarda (`:2572`). Transação não restaura memória Python. Entretanto, o item foi tratado como nota, e novidade concreta dessa falha no diff permanece **[não verificado]**.

Classes:

- **Relógio fora da borda:** presente nos pontos acima (`app/chamados/models.py:2550`, `:2602`).
- **Gate C6 sem discriminação nas autovalidações:** `resposta='validar'` preenche `validada_em` antes do julgamento (`app/chamados/models.py:2560`); C6 retorna morta imediatamente por esse campo (`app/chamados/juizes.py:1033`). É limitação concreta da aprovação; classificá-la como violação do mandato exige considerar que o mandato também autoriza aplicar a validação antes do C6 (`app/chamados/models.py:2541`).
- **Rollback do plantio antes da recusa:** sustentado; a nova `_Reprovada` ocorre dentro do `atomic` (`app/chamados/services/materializacao.py:1109`, `:1166`), com tratamento fora (`:1172`) e recusa posterior (`:1186`).
- **Segundo juiz ou derivação paralela novos:** não identifiquei nesses caminhos; a porta chama C6 (`app/chamados/models.py:2565`), e a lista de identidade é importada da autoridade existente (`:2524`).
- **Pouso antes de pré-requisito:** ocorrência operacional **[não verificado]**. [nao verificado]

## 01/11 15:35 · sessao fcc8e849bb7b · - · - · origem=Ronald (tmux)

- Não encontrei segundo juiz introduzido: a porta chama `pergunta_viva`, cuja lei permanece em `_pergunta_viva_calc`; a lavra continua chamando `_pergunta_materializada_de_fato`. (`app/chamados/models.py:2565`; `app/chamados/juizes.py:1032`; `app/chamados/veredito_lavrado.py:219`.)

- Não encontrei pouso da batida antes da aprovação no ramo alterado: `_materializar_uma` executa o plantio e `carimbar` dentro de `atomic`; recusa levanta `_Reprovada` dentro desse bloco e só é capturada depois. (`app/chamados/services/materializacao.py:1109`; `app/chamados/services/materializacao.py:1166`; `app/chamados/services/materializacao.py:1172`.)

- A checagem do C6 fica automaticamente satisfeita quando `resposta='validar'` encontra texto e validação nula: a porta preenche `validada_em` antes de chamar o juiz, e o primeiro degrau retorna morta por esse campo. Isso limita o significado da nova checagem nesses chamadores; o fluxo corresponde à aplicação do estado completo descrita na porta. (`app/chamados/models.py:2556`; `app/chamados/models.py:2561`; `app/chamados/models.py:2565`; `app/chamados/juizes.py:1033`.)

- O ramo granular mudou a ordem da lavra em relação à autovalidação: agora responde/valida antes de lavrar. Não identifiquei diferença de `veredito_lavrado` causada por essa ordem: o juiz da lavra lê carimbo, via, resposta e testemunhas, mas não `respondida_em`/`validada_em`. (`app/chamados/models.py:2556`; `app/chamados/models.py:2572`; `app/chamados/juizes.py:609`; `app/chamados/juizes.py:755`.) A ordem antiga está somente no anexo do usuário. [não verificado: arquivo:linha do anexo]

- Um leitor indireto sensível é `perguntas_absorvidas_por`: identifica a absorção pelo grupo de carimbo que contém o escape. A nova recusa individual pode deixar irmãs mudas sem carimbo enquanto aceita a pergunta com texto autovalidada; o leitor passa a devolver somente o subconjunto carimbado. Não demonstrou inconsistência: a devolução continua restrita às perguntas efetivamente absorvidas. (`app/chamados/services/materializacao.py:509`; `app/chamados/services/materializacao.py:523`; `app/chamados/services/materializacao.py:569`; `app/chamados/services/materializacao.py:573`; `app/chamados/services/materializacao.py:579`.)

- A porta não acrescenta `select_for_update`; a janela entre julgamento e `save` permanece. Não sustento, apenas pelo código, a quantificação “travar muda a ordem de sete chamadores”. (`app/chamados/models.py:2552`; `app/chamados/models.py:2565`; `app/chamados/models.py:2576`; quantificação: [não verificado].)

- Há relógio interno nas duas portas novas, e a lavra também lê seu próprio relógio. A ausência de injeção integral é demonstrável; a execução em produção ou impacto temporal específico não foi verificado. (`app/chamados/models.py:2550`; `app/chamados/models.py:2602`; `app/chamados/veredito_lavrado.py:228`; produção: [não verificado].)

- A restauração só envolve exceção do C6 ou recusa: falha posterior de lavra/`save` deixa a instância alterada em memória; não cabe afirmar que **todo** chamador tem transação que resolve isso, e rollback de banco tampouco restaura a instância Python. (`app/chamados/models.py:2564`; `app/chamados/models.py:2572`; `app/chamados/models.py:2576`; `app/chamados/models.py:2579`.)

## 01/11 15:32 · sessao 2cc3769c9f76 · O261-E-F2 · verificador · origem=Code

**Os selos passam na árvore atual, mas ainda não provam escritor único:** há escritas plausíveis que escapam e leituras que são contadas como escrita. A resolução é superficial e não distingue o objeto receptor das chamadas (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:88`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:123`).

1. **Escritas que escapam**

   Extraí o detector por AST e executei em memória, sem importar Django. As 11 provas da classe de selo passaram; a árvore retornou seis ocorrências para o carimbo e quatro para a via, com `fora=[]` e nenhuma porta faltante. Resultados efêmeros, sem arquivo de evidência: [nao verificado]. [nao verificado]

   Acrescentei virtualmente uma função escritora a `chamados/reconciliador.py`, sem alterar arquivo. Estas cinco formas escaparam **para cada um dos quatro campos**: `materializada_em`, `materializada_por`, `materializada_por_id` e `via_resolucao`; os dois selos continuaram passando nos 20 mutantes [nao verificado].

   | Forma do mutante — `CAMPO` representa cada campo | Por que escapa |
   |---|---|
   | `d = {'CAMPO': None}; alias = d; qs.update(**alias)` | `_resolve` não resolve nomes recursivamente: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:88`. |
   | `d: dict = {'CAMPO': None}; qs.update(**d)` | `_locais` só coleta `Assign`, não `AnnAssign`: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:70`. |
   | `d = {'CAMPO': None}; qs.update(**{**d})` | `_chaves_de` ignora expansões internas do dicionário: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:96`. |
   | `campo = 'CAMPO'; setattr(p, campo, None); p.save()` | `setattr` exige nome literal diretamente no argumento: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:124`. |
   | `p.__dict__.update({'CAMPO': None}); p.save()` | A chamada é examinada por keywords; o dicionário posicional não entra: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:126`. |

   Há também um mecanismo **declarado na produção** fora desse censo: `materializada_por` usa `on_delete=SET_NULL`, permitindo zerar o autor pela exclusão do usuário. A declaração está em `app/chamados/models.py:2759`; ocorrência ou caminho efetivamente exercido de exclusão de usuário: [nao verificado].

2. **Leituras contadas como escrita**

   Sim, em formas plausíveis. Por exemplo:

   ```python
   trilha = {'materializada_em': p.materializada_em}
   evento(antes=dict(**trilha))
   ```

   O detector acusa `dict(**trilha)` porque `dict` integra `METODOS_DE_ESCRITA`, independentemente do destino do resultado (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:40`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:130`).

   Também acusa construir `dict(materializada_em=p.materializada_em)`, preencher `trilha.update(materializada_em=...)` ou atribuir `trilha['materializada_em']=...`; nesses exemplos há somente leitura do modelo e montagem de trilha (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:119`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:128`). Os mutantes confirmaram essas acusações também para autor e via [nao verificado].

   **Não identifiquei falso positivo nas ocorrências retornadas da árvore atual:** elas correspondem às escritas listadas abaixo (`app/chamados/models.py:2529`, `app/chamados/models.py:2625`, `app/chamados/models.py:2635`, `app/chamados/services/relavra_via.py:123`, `app/chamados/services/regularizacao_ext.py:198`).

3. **Exatidão das listas; escritores e leitores da via**

   As listas correspondem aos **sítios explícitos encontrados nesta cópia**. A afirmação universal “nenhum outro escreve” não fica demonstrada pelo selo, pelos escapes da pergunta 1 (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:88`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:124`).

   | Lista | Escrita efetiva |
   |---|---|
   | `PORTA` | `_escrever_carimbo`: instante e autor, `app/chamados/models.py:2529`; `descarimbar_em_lote`: update, `app/chamados/models.py:2625`; `materializar_regularizacao_externa`: homônimo de **RegularizacaoExterna**, `app/chamados/services/regularizacao_ext.py:198`. |
   | `PORTA_VIA` | `_escrever_carimbo`, `app/chamados/models.py:2531`; `descarimbar_em_lote`, `app/chamados/models.py:2626`; `encerrar_por_fato_no_espelho`, `app/chamados/models.py:2635`; `relavrar`, `app/chamados/services/relavra_via.py:123`. |

   Nenhum escritor **explícito** adicional da via foi identificado [nao verificado]. Os leitores encontrados fora dos quatro sítios de `PORTA_VIA` são:

   | Grupo | Arquivo:linhas |
   |---|---|
   | Juízo e memo | `app/chamados/juizes.py:624`, `:635`, `:790`, `:1042`, `:1071`. |
   | Modelo | `app/chamados/models.py:1838`, `:2489`, `:2552`, `:2644`, `:2656`, `:2673`, `:2680`, `:2712`, `:2714`. |
   | Serviços de chamados | `app/chamados/services/fio.py:882`; `competencia_trancada.py:244`; `explicador.py:95`; `disputa_emissao.py:1628`; `materializacao.py:423`, `:572`, `:614`, `:1023`; `passivo_validado.py:64`, `:83`, `:140`, `:515`; `fabrica_da_celula.py:104`; `relavra_via.py:153`, `:179`, `:230`; `app/chamados/reconciliador.py:1089`. |
   | Ponto e tela | `app/ponto/services/cartorio.py:1429`; `app/ponto/services/flip_auto.py:129`; `app/templates/chamados/partials/_card_atendimento.html:186`. |
   | Commands de chamados | `app/chamados/management/commands/relavrar_via_i6.py:58`; `reabrir_orfas_enterradas.py:67`; `dedup_perguntas_fato.py:74`; `disparar_perguntas_competencia.py:127`; `encerrar_perguntas_com_fato.py:33`; `auditar_invariantes_chamados.py:201`, `:432`, `:541`, `:954`, `:1706`. |

   A descrição “só trocam a via de pergunta já carimbada” depende de premissas dos chamadores: `encerrar_por_fato_no_espelho` não verifica o instante; seu command seleciona materializadas. `relavrar` verifica a via derivada, sem exigir `materializada_em` (`app/chamados/models.py:2633`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:32`, `app/chamados/services/relavra_via.py:116`).

4. **Docstring de `carimbar_identidade`**

   **Parcialmente correta.** Acertou que resposta por validar pode vencer a via de identidade e que `retratar_duplicata` aceita esse estado (`app/chamados/juizes.py:1047`, `app/chamados/services/fio.py:882`, `app/chamados/services/fio.py:891`).

   Continua incorreta como regra absoluta em “segue viva […] até o admin decidir”: antes da identidade, o C6 devolve **morta** quando a resposta concorda com a luz e o dia não acusa, mesmo sem decisão do admin. Validação e tranca também encerram antes (`app/chamados/models.py:2593`, `app/chamados/juizes.py:1033`, `app/chamados/juizes.py:1037`, `app/chamados/juizes.py:1055`).

   Portanto, a formulação correta seria **“pode continuar viva”**, condicionada aos degraus anteriores; a fila global ainda aplica `pergunta_viva` (`app/chamados/juizes.py:1284`, `app/chamados/juizes.py:1288`).

5. **Justificativas da triagem**

   Os números abaixo identificam itens do anexo fornecido; `triagem_v1.md` não foi localizado na cópia [nao verificado].

   - **#6, relógio — descarte insuficiente.** “Os escritores antigos já faziam” não torna fora do escopo os fallbacks das portas novas. O escopo inclui sítios criados/tocados e leitores/escritores envolvidos (`app/chamados/models.py:2550`, `app/chamados/models.py:2601`, `app/docs/CORTES.md:385`, `CLAUDE.md:559`).
   - **#3a, concorrência — descarte insuficiente pelo mesmo critério.** Ser anterior explica a origem, mas não exclui automaticamente a porta tocada, que julga e salva a instância recebida sem adquirir trava (`app/chamados/models.py:2552`, `app/chamados/models.py:2576`, `CLAUDE.md:559`). Uma corrida nova introduzida nesta volta: [nao verificado].
   - **#6, detector “curado” — cura parcial.** Os exemplos específicos foram cobertos, mas aliases, anotações e composição de dicionários continuam fora da resolução (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:70`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:88`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:96`).
   - **#3b, prosa — cura parcial**, pela exceção de concordância descrita na pergunta 4 (`app/chamados/models.py:2593`, `app/chamados/juizes.py:1056`).
   - **#1/8/9, PERGUNTA — o motivo “não inventa hora” tem apoio.** Existe recuperação com instante testemunhado pela trilha, não com `agora` arbitrário; existência dessa trilha nos casos de produção fornecidos: [nao verificado] (`app/chamados/services/disputa_emissao.py:1399`, `app/chamados/services/disputa_emissao.py:1403`).

6. **Classes que reaparecem**

   | Classe | Resultado |
   |---|---|
   | Selo sem mordida | **Sim, cobertura incompleta no delta v2**, apesar das provas positivas adicionadas: resolução superficial e ausência de análise do receptor (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:88`, `:123`). |
   | Relógio fora da borda | **Presente no diff total**, nos fallbacks novos; C6 e lavra também conservam relógios internos. O corte exige injeção e borda nominal (`app/chamados/models.py:2550`, `:2601`; `app/chamados/juizes.py:539`; `app/chamados/veredito_lavrado.py:228`; `app/docs/CORTES.md:369`). |
   | Segundo juiz / derivação paralela | Não identificados no caminho novo: a porta chama C6 e reutiliza a lista de identidade (`app/chamados/models.py:2524`, `:2565`). |
   | Segundo escritor explícito atual fora da porta | Não identificado; os mutantes mostram que o selo não garante sua ausência (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:126`). |
   | Ordem de trava | A porta nova não adquire trava; mudança de ordem não demonstrada (`app/chamados/models.py:2552`, `:2576`). |
   | Pouso antes do pré-requisito / RED histórico / suíte inteira | [nao verificado]. | [nao verificado]

   A recusa granular mantém `_Reprovada` dentro do `atomic`, preservando a estrutura de rollback da batida (`app/chamados/services/materializacao.py:1109`, `app/chamados/services/materializacao.py:1166`, `app/chamados/services/materializacao.py:1172`).

## 01/11 15:30 · sessao ed37d9048a68 · - · - · origem=Ronald (tmux)

As listas correspondem aos sítios de atribuição explícita encontrados, com uma ressalva de escopo: `PORTA` inclui o homônimo `RegularizacaoExterna`, não só `PerguntaDisputa`. Os campos desse outro modelo estão em `app/chamados/models.py:3112`, `:3134`, `:3135`; sua escrita ocorre em `app/chamados/services/regularizacao_ext.py:198`, `:199`, `:200`.

| Lista | Sítio que efetivamente escreve |
|---|---|
| PORTA | `_escrever_carimbo`: atribui instante e autor em `app/chamados/models.py:2529`, `:2530`; `descarimbar_em_lote`: update em `:2625`, `:2626`; homônimo acima. |
| PORTA_VIA | `_escrever_carimbo`, `app/chamados/models.py:2531`; `descarimbar_em_lote`, `:2626`; `encerrar_por_fato_no_espelho`, `:2635`, persistência em `:2636`; `relavrar`, `app/chamados/services/relavra_via.py:123`, persistência em `:125`. |

Não identifiquei atribuição explícita adicional pelos nomes dos campos ou pelos setters dinâmicos examinados; ausência universal de escrita indireta permanece [nao verificado]. `passivo_validado.CAMPOS_REVERSAO` é leitura: lista os campos em `app/chamados/services/passivo_validado.py:64`, e `_reversao_de` usa `getattr` em `:83`, armazenando o snapshot em `:367`.

Há uma escrita indireta condicional que merece separar do censo de atribuições: `materializada_por` usa `on_delete=models.SET_NULL` em `app/chamados/models.py:2759`, `:2761`. Uma exclusão de `User` pode, portanto, zerar o autor pelo coletor do ORM; ocorrência e caminho executável de exclusão neste ambiente são [nao verificado]. O detector não examina essa política: só reconhece alvos de atribuição, `setattr` literal e chamadas selecionadas em `app/chamados/tests/test_o261e_f2_porta_carimbo.py:109`, `:124`, `:126`.

Leitores de `via_resolucao` fora dos quatro sítios de `PORTA_VIA`:

- Juízo e memo: `app/chamados/juizes.py:624`, `:635`, `:790`, `:1042`, `:1071`.
- Modelo: `app/chamados/models.py:1838`, `:2489`, `:2552`, `:2644`, `:2656`, `:2673`, `:2680`, `:2712`, `:2714`.
- Serviços: `app/chamados/services/fio.py:882`; `competencia_trancada.py:244`; `explicador.py:95`; `disputa_emissao.py:1628`, `:1643`, `:1646`; `materializacao.py:423`, `:572`, `:614`, `:1023`; `passivo_validado.py:83`, `:140`, `:141`, `:142`, `:515`; `fabrica_da_celula.py:104`; `relavra_via.py:153`, `:179`, `:230`; `app/chamados/reconciliador.py:1089`, `:1100`.
- Ponto: `app/ponto/services/cartorio.py:1429`; `app/ponto/services/flip_auto.py:129`.
- Tela: `app/templates/chamados/partials/_card_atendimento.html:186`.
- Commands, todos sob `app/chamados/management/commands/`: `relavrar_via_i6.py:58`, `:59`, `:62`, `:66`, `:68`, `:88`; `reabrir_orfas_enterradas.py:67`, `:128`, `:141`, `:149`; `dedup_perguntas_fato.py:74`; `disparar_perguntas_competencia.py:127`; `encerrar_perguntas_com_fato.py:33`, `:42`, `:49`, `:66`, `:76`; `auditar_invariantes_chamados.py:201`, `:205`, `:432`, `:541`, `:954`, `:1706`.

A descrição “só trocam a via de pergunta já carimbada” depende dos chamadores: `encerrar_por_fato_no_espelho` não verifica `materializada_em` antes de gravar (`app/chamados/models.py:2633`, `:2635`), mas seu command seleciona materializadas (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:32`). `relavrar` verifica a via derivada, sem verificar instante (`app/chamados/services/relavra_via.py:116`); `relavrar_dia` também filtra pela via, usando a premissa documentada de que ela implica materialização (`:147`, `:153`).

## 01/11 15:30 · sessao 39357f1b88dd · - · - · origem=Ronald (tmux)

4. A docstring corrigiu “sempre morta”, mas ainda exagera em “segue viva [...] até o admin decidir”: resposta com `respondida_em` e texto retorna **morta sem validação do admin** quando `_resposta_concorda_com_a_luz(p) is True`; essa precedência vem antes da identidade. Validação e tranca também matam antes. `app/chamados/models.py:2591-2593`; `app/chamados/juizes.py:1033-1059`.

`retratar_duplicata` realmente aceita resposta por validar: só recusa quando a via já é duplicata, e chama `carimbar_identidade` sem examinar resposta. `app/chamados/services/fio.py:878-895`. A fila global aceita identidade respondida e depois consulta C6: `app/chamados/models.py:2710-2723`; `app/chamados/juizes.py:1282-1288`.

Executei `_pergunta_viva_calc` extraída por AST, sem Django/banco: duplicata materializada, respondida e não validada, tranca=False; concordância stubada True → False; concordância None → True. Resultado efêmero [nao verificado]; ramo real: `app/chamados/juizes.py:1055-1059`.

5. Justificativas contestáveis na triagem fornecida:

- **#6 relógio:** descartar porque os antigos escritores já tinham fallback não sustenta “fora do F2”: as portas novas têm `now()` em `models.py:2550` e `models.py:2601`. O corte define escopo como todo sítio criado/tocado, inclusive novo, e substitui o critério anterior: `app/docs/CORTES.md:385`. O corte do relógio exige instante vindo do chamador e borda nominal: `app/docs/CORTES.md:369`. O C6 e a lavra ainda leem relógio internamente, mas esses arquivos não aparecem alterados no delta fornecido: `app/chamados/juizes.py:539`; `app/chamados/veredito_lavrado.py:228`.
- **#3a concorrência:** “a janela é anterior” pode explicar origem, mas não basta para excluir um sítio tocado, pelo mesmo corte: `app/docs/CORTES.md:385`. A porta julga/salva a instância sem trava: `app/chamados/models.py:2552-2576`. Não provei janela concorrente nova nesta volta [nao verificado].
- **#4 reversão em memória:** “atomic do chamador cobre” não cobre a instância Python. O try/except restaura só erro do juiz; `lavrar` e `save` ficam fora e podem deixar o carimbo/resposta aplicados após exceção: `app/chamados/models.py:2555-2576`. Isso é insuficiência da justificativa; não prova corrupção persistida [nao verificado].
- **#1/8/9 PERGUNTA:** o motivo “não inventa hora” tem apoio explícito no código da recuperação pela trilha: `app/chamados/services/disputa_emissao.py:1399-1414`. A recusa conserva o insumo porque a porta restaura os campos e o chamador segue: `app/chamados/models.py:2569-2571`; `app/chamados/services/disputa_emissao.py:1638-1639`. Sem instante a validação recusa e a fila global exclui: `app/chamados/services/validacao.py:363-365`; `app/chamados/juizes.py:1284`. Não encontrei fundamento para invalidar esse encaminhamento.
- **#3b prosa:** a cura é parcial; a exceção de concordância continua omitida na nova frase “até admin”: `app/chamados/models.py:2593`; `app/chamados/juizes.py:1056-1059`.

O arquivo `triagem_v1.md` não foi encontrado na cópia; referências aos números são ao anexo fornecido [nao verificado].

## 01/11 15:30 · sessao 6446b6cf9ca6 · - · - · origem=Ronald (tmux)

Para a pergunta 6:

- **Delta v2 estrito:** há fragilidade na classe **selo sem mordida**: o detector resolve apenas um nível de variável (`_resolve`) e só chaves diretas de `Dict`/`dict(...)`; nomes encadeados, composição de dicionário e aliases ficam fora dessa análise. A comprovação por mutantes está com o agente principal. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:88-100`)

- **Diff total:** existem leituras internas de relógio nos métodos novos: `quando or _tz.now()` em `carimbar` e `carimbar_identidade`. O C6 e a lavra também continuam lendo relógio próprio. Portanto, a justificativa “fora do F2” não se sustenta como exclusão automática de escopo: a regra inclui sítios criados/tocados e seus leitores/escritores. A exigência específica de injeção nomeia juiz/grade/cartório/reconciliador/fábrica; classificar o método do modelo como “fábrica” exige interpretação. (`app/chamados/models.py:2550`, `app/chamados/models.py:2601`, `app/chamados/juizes.py:539`, `app/chamados/veredito_lavrado.py:228`, `app/docs/CORTES.md:369`, `app/docs/CORTES.md:385`, `CLAUDE.md:559-560`)

- **Não identifiquei segundo juiz no caminho novo:** `carimbar` chama `pergunta_viva`; `vias_de_identidade` devolve a lista existente. (`app/chamados/models.py:2524-2525`, `app/chamados/models.py:2545`, `app/chamados/models.py:2565`)

- **Memo:** a alteração em memória dos campos do carimbo muda a chave do C6; a recusa também remove explicitamente `_pv_memo`. Não há prova de cache antigo sendo aceito apenas por faltar um `pop` antes da consulta. (`app/chamados/juizes.py:788-795`, `app/chamados/juizes.py:812-824`, `app/chamados/models.py:2586`)

- **Rollback granular preservado:** a recusa da porta levanta `_Reprovada` dentro do `atomic`; o tratamento fica fora. (`app/chamados/services/materializacao.py:1109`, `app/chamados/services/materializacao.py:1166-1175`)

- **Ordem de trava:** o novo método não adquire trava; não há nova ordem demonstrável nele. A corrida com instância desatualizada permanece possível, mas sua novidade no diff é [nao verificado]. (`app/chamados/models.py:2552-2576`)

- **Pouso antes do pré-requisito, suite e RED histórico:** [nao verificado]. [nao verificado]

## 01/11 15:23 · sessao 6e1ef93ef2da · O261-E-F2 · verificador · origem=Code

**Há bloqueio no fluxo do “fecho sem credor”: a recusa preserva texto sem `respondida_em`, estado que impede fechar a disputa e também impede validar a resposta pela porta normal.** O teste alterado passou a exigir esse estado. (`app/chamados/services/disputa_emissao.py:1638`; `app/chamados/models.py:1767`; `app/chamados/services/validacao.py:363`; `app/chamados/tests/test_o261_resposta_vazia.py:104`.)

Ocorrência, quantidade e duração desse problema em produção: **[nao verificado]**. Execução da suíte Django e RED contra a árvore de `6f583cbc`: **[nao verificado]**. As comparações abaixo com a base usam o diff fornecido.

**1. Censo de escritores e leitores — nota; ressalva sobre a via.**

Os escritores de `materializada_em` e `materializada_por/_id` convergiram para a porta. **`via_resolucao` conserva dois escritores independentes dos novos métodos.** (`app/chamados/models.py:2529`; `app/chamados/models.py:2624`; `app/chamados/models.py:2633`; `app/chamados/services/relavra_via.py:123`.)

| Campo | Quem escreve | Na porta? |
|---|---|---|
| `materializada_em` | `_escrever_carimbo`, atribuição; `descarimbar_em_lote`, `update()` | Sim. `app/chamados/models.py:2529`, `app/chamados/models.py:2624`. |
| `materializada_por/_id` | Os mesmos, inclusive atribuição por `_id` | Sim. `app/chamados/models.py:2530`, `app/chamados/models.py:2624`. |
| `via_resolucao` | Os anteriores; `encerrar_por_fato_no_espelho`; `relavrar` | Os dois últimos ficam fora da nova porta do carimbo, embora pertençam às portas declaradas da entidade. `app/chamados/models.py:2531`, `app/chamados/models.py:2624`, `app/chamados/models.py:2633`, `app/chamados/services/relavra_via.py:123`. |
| `respondida_em` | `carimbar`; reversão em memória; ressuscitação pelo lote | Nova porta. `app/chamados/models.py:2558`, `app/chamados/models.py:2583`, `app/chamados/models.py:1488`. |
| `respondida_em` | `responder_pergunta_disputa_core`; `reabrir_para_correcao`, por dicionário/`update(**campos)`; `responder_automaticamente`; `carimbar_resposta_pela_trilha` | Fora dos novos métodos, dentro da porta de resposta. `app/chamados/services/disputa_emissao.py:1248`, `app/chamados/services/disputa_emissao.py:1358`, `app/chamados/services/disputa_emissao.py:1378`, `app/chamados/services/disputa_emissao.py:1394`, `app/chamados/services/disputa_emissao.py:1410`. |
| `validada_em` | `carimbar`; reversão em memória; ressuscitação pelo lote | Nova porta. `app/chamados/models.py:2561`, `app/chamados/models.py:2584`, `app/chamados/models.py:1488`. |
| `validada_em` | Resposta nova; reabertura; `_carimbar_validacao`; `devolver_validacao_recusada` | Fora da porta do carimbo, dentro das portas de resposta/validação. `app/chamados/services/disputa_emissao.py:1260`, `app/chamados/services/disputa_emissao.py:1358`, `app/chamados/services/validacao.py:131`, `app/chamados/services/validacao.py:188`. |
| `validada_por/_id` | Mesmos atos de aplicação/reversão/lote, resposta nova, reabertura e validação | Mesma classificação. `app/chamados/models.py:2562`, `app/chamados/models.py:2585`, `app/chamados/models.py:1488`, `app/chamados/services/disputa_emissao.py:1261`, `app/chamados/services/disputa_emissao.py:1358`, `app/chamados/services/validacao.py:132`, `app/chamados/services/validacao.py:189`. |
| `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `lavrar`, atribuição e `save`; `gravar_lavras_em_lote`, `bulk_update` | Porta da lavra. `app/chamados/veredito_lavrado.py:227`, `app/chamados/veredito_lavrado.py:228`, `app/chamados/veredito_lavrado.py:229`, `app/chamados/veredito_lavrado.py:231`, `app/chamados/veredito_lavrado.py:207`. |
| Trio da lavra | Backfill atribui os três em memória e entrega à porta de persistência | Atribuição fora do módulo canônico; persistência delegada. `app/chamados/management/commands/backfill_veredito_lavrado.py:138`, `app/chamados/management/commands/backfill_veredito_lavrado.py:143`, `app/chamados/management/commands/backfill_veredito_lavrado.py:146`. |

Há ainda atribuições de `validada_em` e `respondida_em` **somente na cópia** usada para julgar a ressuscitação. O autor do carimbo agora é limpo nessa cópia por `descarimbar(salvar=False)`. (`app/chamados/models.py:1439`; `app/chamados/models.py:1440`; `app/chamados/models.py:1441`.)

Os saves que persistem lavra calculada com `salvar=False` aparecem também no nascimento/reapontamento, na porta nova e na relavra. (`app/chamados/models.py:2449`; `app/chamados/models.py:2575`; `app/chamados/services/fio.py:847`; `app/chamados/services/relavra_via.py:125`.)

O homônimo escrito por `materializar_regularizacao_externa` pertence a `ItemRegularizacao`; não é escritor de `PerguntaDisputa`. (`app/chamados/services/regularizacao_ext.py:198`; `app/chamados/models.py:3132`.)

**Leitores.** A tabela inclui acessos diretos, consultas ORM, leitura genérica por nome de campo e templates. Legenda: **M**=`materializada_em`; **A**=`materializada_por/_id`; **V**=`via_resolucao`; **R**=`respondida_em`; **D**=`validada_em`; **U**=`validada_por/_id`; **L**=`veredito_lavrado`; **J**=`julgado_em`; **X**=`janela_expira_em`. Nas células, as linhas pertencem ao arquivo da primeira coluna.

| Arquivo | Campos lidos e linhas |
|---|---|
| `app/chamados/juizes.py` | M:275,624,650,791,1042,1071,1077,1107,1125,1633; V:624,635,790,1042,1071; R:789,1047,1157,1277,1284,1287,1634; D:789,1033,1634. |
| `app/chamados/models.py` | M:1421,1768,2477,2480,2490,2552,2596,2597,2642,2654,2671,2710,2712; A:2552,2597; V:1838,2489,2552,2631,2642,2654,2671,2678,2710,2712; R:1722,1769,2479,2482,2553,2557,2708,2780; D:293,327,1768,2475,2553,2560,2719; U:2553. |
| `app/chamados/reconciliador.py` | M:366,583,998,1090,1100,1405; A:1091,1101; V:1089,1100; D:584,998. |
| `app/chamados/veredito_lavrado.py` | M:163,164; R:167,168; L:179; J:179,185,189; X:182,185. |
| `app/chamados/services/disputa_emissao.py` | M:211,695,874,894,1117,1539,1620; V:1628,1643,1646; R:1372,1408,1446,1559,1630,1647; D:1250,1251,1373,1620; U:1250,1373. |
| `app/chamados/services/materializacao.py` | M:419,510,571,573,578,617,1023,1027,1261,1308; A:618; V:423,572,614,1023; R:615,1259; D/U:616; D:1260. |
| `app/chamados/services/validacao.py` | M:180,182,422,433,451; R:363,537,542,558,877; D:139,180,187,315,366,433,534,685,706; U:139,180,315. |
| `app/chamados/services/fio.py` | V:882; R:336,656. |
| `app/chamados/services/fabrica_da_celula.py` | M:123; V:104. |
| `app/chamados/services/raiox.py` | M:133,173; R:136; L:32,134,182,183; J:31; X:32. |
| `app/chamados/services/relavra_via.py` | M:179,229; V:116,119,121,153,179,230; L:122,128,135. |
| `app/chamados/services/passivo_validado.py` | Todos os campos:64,65,66,83, por `getattr`; V também:140,142,515. |
| `app/chamados/services/historico.py` | R:99; D:101; M:103; consumo genérico:105. |
| `app/chamados/services/painel.py` | M/R/D:67, agregação genérica; U:260; D:268,271. |
| `app/chamados/services/acoes_disputa.py` | R:95,130; D:341. |
| `app/chamados/services/declaracao_texto.py` | R:363,365,632; D:208. |
| `app/chamados/services/verbos.py` | R:311,312,318,321; D:240,246; U:240,244. |
| `app/chamados/services/vigia_fio.py` | R:630,737. |
| `app/chamados/services/vigia_carimbo.py` | D:98. |
| `app/chamados/services/explicador.py` | V:95. |
| `app/chamados/services/competencia_trancada.py` | V:244,250. |
| `app/chamados/services/aviso_pendencia.py` | R:13. |
| `app/chamados/services/becos.py` | R:30. |
| `app/chamados/services/cobranca.py` | R:229. |
| `app/chamados/services/furo_sem_canal.py` | R:81. |
| `app/chamados/views.py` | M:1257; R:1280,1416. |
| `app/chamados/views_cobrar.py` | R:169,176. |
| `app/chamados/relatorios_disputa.py` | R:20. |
| `app/api/views.py` | R:1009,1175,1187,2634,2688,2705. |
| `app/api/views_mensageria.py` | R:569,570. |
| `app/ponto/services/procedencia_batida.py` | A/U:71,72; M/D:74. |
| `app/ponto/services/cartorio.py` | V:1429. |
| `app/ponto/services/flip_auto.py` | V:129; D:130. |
| `app/ponto/supra_juiz.py` | R:515,518. |
| `app/ponto/management/commands/reconciliar_grade.py` | R:461. |
| `app/ponto/management/commands/reconciliar_fantasmas.py` | M:212. |
| `bin/censo_folha.py` | R:28,29; D:29. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py` | M/D:32; V:33,42,49,66,76. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py` | M:469,537,587,904,954,1072,1153,1570,1598,1763; V:201,205,432,541,954,1706; R:177,591,1102,1568,1599; D:204,1072,1569,1598,1762,1774; U:1073; L:1169. |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py` | M:67,87,90,128,141; A:129,142; V:67,128,141,149; D:68. |
| `app/chamados/management/commands/auditar_disputas.py` | M:71,150. |
| `app/chamados/management/commands/disparar_perguntas_competencia.py` | V:127. |
| `app/chamados/management/commands/relavrar_via_i6.py` | V:58,59,62,66,68,88. |
| `app/chamados/management/commands/dedup_perguntas_fato.py` | V:74. |
| `app/chamados/management/commands/curar_perguntas_em_limbo.py` | R:46,69. |
| `app/chamados/management/commands/backfill_veredito_lavrado.py` | L:120,124,134; X:121,135. |
| `app/templates/chamados/partials/modal_fio.html` | R:184,212,215; D:223. |
| `app/templates/chamados/partials/_card_atendimento.html` | V:186; R:94,100,137,155,384,385. |
| `app/templates/chamados/partials/_pergunta_card.html` | R:24,29,32; D:20,23,42. |

**2. Comportamento de cada chamador quando há recusa — nota.**

A recusa restaura os seis campos capturados e retorna `False`; o save e a lavra posterior não acontecem. (`app/chamados/models.py:2552`; `app/chamados/models.py:2569`; `app/chamados/models.py:2572`; `app/chamados/models.py:2579`.)

| Chamador de `carimbar` | Resultado observável da recusa, comparado ao escritor anterior do diff |
|---|---|
| `lavrar_sem_turno_aberto` | Retorna `False`, sem carimbo e sem alcançar o fechamento da disputa; antes salvava depois do pré-C6. `app/chamados/reconciliador.py:398`, `app/chamados/reconciliador.py:405`, `app/chamados/reconciliador.py:411`. |
| `retratar_perguntas_absolvidas_por_celula` | Pula a pergunta; não incrementa `n` nem inclui essa disputa no fechamento. `app/chamados/reconciliador.py:618`, `app/chamados/reconciliador.py:620`, `app/chamados/reconciliador.py:622`. |
| `encerrar_fantasma` | Devolve `False`; seu consumidor pula evento, contador e inclusão da disputa. `app/chamados/models.py:2645`; `app/chamados/reconciliador.py:1093`, `app/chamados/reconciliador.py:1095`, `app/chamados/reconciliador.py:1102`. |
| S128, primeiro ramo | Pula carimbo, lavra e incremento de perguntas resolvidas; os laços seguintes de reabertura e fechamento continuam. `app/chamados/services/disputa_emissao.py:1566`, `app/chamados/services/disputa_emissao.py:1569`, `app/chamados/services/disputa_emissao.py:1583`, `app/chamados/services/disputa_emissao.py:1652`. |
| S128, fecho sem credor | Preserva estado anterior; não emite `fecho_sem_credor`; ainda consulta `pode_fechar` no final. `app/chamados/services/disputa_emissao.py:1638`, `app/chamados/services/disputa_emissao.py:1641`, `app/chamados/services/disputa_emissao.py:1658`. |
| `_materializar_ausencia_de_resposta` | Ignora o booleano. A ausência já criada/reaberta continua; reconciliação do destino e tentativa de fechar a disputa seguem. A resposta original também já foi gravada. `app/chamados/services/materializacao.py:501`, `app/chamados/services/materializacao.py:523`, `app/chamados/services/materializacao.py:532`, `app/chamados/services/materializacao.py:536`; `app/chamados/services/disputa_emissao.py:1266`, `app/chamados/services/disputa_emissao.py:1292`. |
| `_materializar_uma` | Levanta `_Reprovada`, desfaz a tentativa no atomic interno, registra erro e depois motivo/possível nota; retorna `False`. `app/chamados/services/materializacao.py:1109`, `app/chamados/services/materializacao.py:1168`, `app/chamados/services/materializacao.py:1172`, `app/chamados/services/materializacao.py:1186`. |
| `materializar_pendentes_aprovadas` | Omite o pk da lista devolvida. S144 ignora essa lista; a disputa que disparou o receiver já está fechada. `app/chamados/services/materializacao.py:1313`; `app/chamados/signals.py:130`, `app/chamados/signals.py:140`. |

**Nos caminhos com texto e `resposta='validar'`, a própria porta preenche `validada_em` antes do julgamento; o C6 retorna MORTA imediatamente no degrau zero.** Assim, a recusa nominal pelo C6 não ocorre nesses casos. (`app/chamados/models.py:2560`; `app/chamados/models.py:2565`; `app/chamados/juizes.py:1033`.)

Os demais chamadores não têm recusa C6:

| Porta | Chamadores e comportamento |
|---|---|
| `carimbar_identidade` | Admin; duplicata; paridade recusada; colisão. Salvam diretamente após validar a via; duplicata preserva instante/autor quando já carimbada, e colisão leva motivo no mesmo save. `app/chamados/models.py:2498`; `app/chamados/services/fio.py:891`; `app/chamados/services/materializacao.py:858`; `app/chamados/services/materializacao.py:1033`; `app/chamados/models.py:2594`. |
| `descarimbar` | Cópia da ressuscitação; reabertura de fantasma, por resposta e por competência; desfazer lastro. Mantêm o comportamento anterior dos respectivos atos, passando o trio pela porta. `app/chamados/models.py:1439`, `app/chamados/models.py:2656`, `app/chamados/models.py:2673`, `app/chamados/models.py:2680`; `app/chamados/services/relavra_via.py:231`. |
| `descarimbar_em_lote` | Ressuscitação e reabertura com absorção desfeita. A mudança material da ressuscitação é limpar também `materializada_por`. `app/chamados/models.py:1488`; `app/chamados/services/disputa_emissao.py:1377`; `app/chamados/models.py:2624`. |

**3. Gravar e reler VIVA — bloqueia a garantia universal sob concorrência; nota para identidade.**

A porta comum **julga a instância recebida**, sem reler ou travar a pergunta, e salva somente os campos escolhidos. Há esta intercalação possível:

1. S128 carrega uma pergunta com validação preenchida. (`app/chamados/services/disputa_emissao.py:1537`.)
2. Uma resposta concorrente grava texto/instante novos e limpa a validação. (`app/chamados/services/disputa_emissao.py:1247`; `app/chamados/services/disputa_emissao.py:1260`; `app/chamados/services/disputa_emissao.py:1266`.)
3. A instância antiga entra na porta; seu `validada_em` antigo dá MORTA no C6. Como ele já estava preenchido nessa instância, não entra nos campos do save. (`app/chamados/models.py:2560`; `app/chamados/models.py:2565`; `app/chamados/juizes.py:1033`.)
4. O trio é gravado sobre uma linha com validação nula; relida, a pergunta pode dar VIVA pela resposta nova. (`app/chamados/models.py:2576`; `app/chamados/juizes.py:1047`.)

A janela de concorrência não nasceu necessariamente neste diff; a nova garantia da porta continua sem cobri-la. Ocorrência real: **[nao verificado]**.

A identidade também pode gravar e ser relida VIVA: `retratar_duplicata` aceita resposta pendente de validação, e o C6 examina a resposta **antes** da identidade. Isso pertence à exceção autorizada, mas torna inexata a afirmação incondicional da docstring de que essas vias são sempre mortas no degrau dois. (`app/chamados/services/fio.py:878`; `app/chamados/models.py:2589`; `app/chamados/juizes.py:1047`; `app/chamados/juizes.py:1071`.)

Nos caminhos sequenciais examinados, não encontrei alteração escalar posterior ao carimbo que demonstrasse esse retorno imediato à vida. O memo considera carimbo, resposta e validação; porém não representa todos os insumos relacionados, como `ata`/`dna`, e a porta não força relações frescas. A equivalência geral entre instância cacheada e releitura do banco fica **[nao verificado]**. (`app/chamados/juizes.py:778`; `app/chamados/juizes.py:788`; `app/chamados/models.py:2565`.)

**4. Recusa deixando campos aplicados em memória — nota.** [nao verificado]

Na recusa booleana do C6, os seis escalares aplicados são restaurados: instante, autor e via do carimbo; instante da resposta; instante e autor da validação. `_pv_memo` é removido. Não encontrei campo escalar aplicado pela porta e esquecido nessa reversão. (`app/chamados/models.py:2552`; `app/chamados/models.py:2579`.)

Isso não restaura a identidade dos objetos previamente presentes no cache de relações: restaura os ids. Além disso, exceção da lavra ou do `save`, **depois** da aprovação, fica fora do `try` de reversão. Essa última situação é falha posterior, não recusa C6. (`app/chamados/models.py:2564`; `app/chamados/models.py:2572`; `app/chamados/models.py:2585`.)

**5. `_Reprovada`, atomic e efeitos sobreviventes — nota.**

A exceção nova é levantada dentro do `with _tx.atomic()` de `_materializar_uma`; a captura fica depois da saída desse bloco. Portanto a batida criada nessa tentativa e as demais escritas SQL internas são revertidas. Uma batida preexistente reutilizada continua existindo. (`app/chamados/services/materializacao.py:1109`; `app/chamados/services/materializacao.py:1111`; `app/chamados/services/materializacao.py:1168`; `app/chamados/services/materializacao.py:1172`; `app/chamados/services/materializacao.py:793`.)

**Há efeitos deliberadamente posteriores ao rollback:** a mensagem entra em `erros`; avisos podem ser transferidos para `reusos`; o `finally` chama `_carimbar_recusa`, que grava motivo e pode anexar ocorrência ao chamado. (`app/chamados/services/materializacao.py:1173`; `app/chamados/services/materializacao.py:1179`; `app/chamados/services/materializacao.py:1186`; `app/chamados/services/materializacao.py:1037`; `app/chamados/services/materializacao.py:1041`.)

A validação pode envolver tudo num atomic externo e ainda emitir trilha da falha; o passivo conta a tentativa como recusada. Esses efeitos sobrevivem se a transação externa terminar normalmente. O contador de materializadas não aumenta quando a unidade devolve `False`. (`app/chamados/services/validacao.py:395`; `app/chamados/services/validacao.py:451`; `app/chamados/services/passivo_validado.py:511`; `app/chamados/services/passivo_validado.py:524`; `app/chamados/services/materializacao.py:1265`.)

Nos três universos normais da unidade, a validação já está preenchida; esse novo ramo de recusa C6 não ganhou uma testemunha normal nesses chamadores. Efeitos externos síncronos de toda a cadeia de signals, bem como RED executado desse ramo: **[nao verificado]**. (`app/chamados/services/materializacao.py:1258`; `app/chamados/services/validacao.py:395`; `app/chamados/services/passivo_validado.py:514`; `app/chamados/juizes.py:1033`.)

**6. Classes de reprovação — bloqueia no selo; fila nas demais ressalvas.**

| Classe | Verificação |
|---|---|
| Segundo juiz | A porta chama o C6 registrado, sem implementar outro predicado de vitalidade. `vias_de_identidade` importa a lista existente. **Nota.** `app/core/juizes.py:266`; `app/chamados/models.py:2524`; `app/chamados/models.py:2565`. |
| Segundo escritor | Não encontrei escritor adicional de M/A fora dos dois sítios canônicos; a via conserva os dois escritores apontados no item 1. **Fila**, se o despacho pretende centralizar também toda alteração isolada da via. `app/chamados/models.py:2529`; `app/chamados/models.py:2624`; `app/chamados/models.py:2633`; `app/chamados/services/relavra_via.py:123`. |
| Relógio fora da borda | Os novos métodos têm `now()` de fallback; o C6 e a lavra continuam lendo relógio internamente, sem receber o instante da porta. A injeção integral não está concluída nesta árvore. **Fila.** `app/chamados/models.py:2550`; `app/chamados/models.py:2599`; `app/chamados/juizes.py:539`; `app/chamados/veredito_lavrado.py:112`; `app/chamados/veredito_lavrado.py:228`; corte em `app/docs/CORTES.md:369`. |
| Derivação paralela | O diff reutiliza `via_pelo_fato`, `declarou_hora` e a lista de identidade. Permanece a separação já existente entre identidade no C6, no queryset e na lista da relavra. **Nota.** `app/chamados/services/disputa_emissao.py:1566`; `app/chamados/services/materializacao.py:523`; `app/chamados/juizes.py:1066`; `app/chamados/models.py:2705`; `app/chamados/services/relavra_via.py:166`. |
| Selo/RED sem m

[truncado em 20000 de 29009 caracteres]

## 01/11 15:21 · sessao 571682035d43 · - · - · origem=Ronald (tmux)

1. **Chamadores de `carimbar`, na recusa C6, versus o diff fornecido:**

| Chamador | Efeito atual da recusa | Referências |
|---|---|---|
| `lavrar_sem_turno_aberto` | Retorna `False`; não carimba nem chega ao fechamento da disputa. O pré-C6 continua existindo. | `app/chamados/reconciliador.py:398-414` |
| `retratar_perguntas_absolvidas_por_celula` | `continue`; não incrementa `n` nem inscreve essa disputa para fechamento. | `app/chamados/reconciliador.py:608-639` |
| `PerguntaDisputa.encerrar_fantasma` | Devolve o `False` da porta. Consumidor não grava evento, não incrementa encerradas nem inclui disputa para fechar. | `app/chamados/models.py:2641-2645`; `app/chamados/reconciliador.py:1092-1103` |
| S128, ramo destino | `continue`; não incrementa `perguntas_resolvidas`, não relavra veredito. O restante da função ainda examina reabertura de pais e candidatos a fechamento. | `app/chamados/services/disputa_emissao.py:1566-1583`; `:1591`; `:1606`; `:1652-1661` |
| S128, ramo sem credor | `continue`; não grava trio/resposta nem evento `fecho_sem_credor`; continua fechamento final consultando `pode_fechar`. | `app/chamados/services/disputa_emissao.py:1638-1659` |
| `_materializar_ausencia_de_resposta` | Ignora o retorno: ausência já foi criada/reaberta; continua reconciliando destino se ausência decidida e depois consulta fechamento. A resposta original continua gravada pelo escritor da resposta. | `app/chamados/services/materializacao.py:501-537`; `app/chamados/services/disputa_emissao.py:1247-1266`; `:1292-1295` |
| `_materializar_uma` | Levanta `_Reprovada` dentro do atomic interno; desfaz plantio/transação dessa tentativa; captura fora dele, acrescenta erro, finalmente grava motivo/nota de recusa e retorna `False`. | `app/chamados/services/materializacao.py:1109-1115`; `:1166-1188`; `:1023-1041` |
| `materializar_pendentes_aprovadas` | Omite pk de `feitas`; não fabrica batida; retorna lista reduzida. Receiver S144 ignora lista e fechamento da disputa que acionou o receiver já ocorreu. | `app/chamados/services/materializacao.py:1307-1315`; `app/chamados/signals.py:130-142` |

Nos ramos com `resposta='validar'` e texto, a recusa nominal por C6 não ocorre: a porta coloca `validada_em` antes de consultar o juiz, cujo primeiro degrau retorna morta para qualquer `validada_em` preenchida. Portanto a nova guarda `_Reprovada` por recusa C6 não tem testemunha normal nos três chamadores existentes da unidade, cujos universos trazem validação preenchida. `app/chamados/models.py:2556-2565`; `app/chamados/juizes.py:1033-1034`; `app/chamados/services/materializacao.py:1258-1265`; `app/chamados/services/validacao.py:395-424`; `app/chamados/services/passivo_validado.py:514-516`. Prova de mutante/runtime desse ramo [nao verificado].

2. **Identidade e descarimbo não têm a recusa C6.** Identidade valida a lista e salva; descarimbo salva/atualiza diretamente. Assim não existe comportamento “quando C6 recusa” para esses chamadores: admin `models.py:2498`; duplicata `fio.py:891-895`; paridade `materializacao.py:858-859`; colisão `materializacao.py:1033-1041`; retorno individual `models.py:2656`, `:2673`, `:2680`; ressuscitação em cópia `models.py:1439-1442`; ressuscitação em lote `models.py:1488`; reabertura `disputa_emissao.py:1377`; desfazer lastro `relavra_via.py:231-246`. Implementações: `app/chamados/models.py:2588-2624`. A mudança observável de ressuscitação é limpar também autor (diff fornecido; `app/chamados/models.py:1488`, `:2623-2624`).

3. **Q3 — grava e relê VIVA: há via de identidade e há janela concorrente na porta comum. Gravidade: bloqueia para garantia universal, nota para exceção de identidade.**

- Identidade aceita pergunta com resposta existente não validada: salva sem C6 (`app/chamados/models.py:2588-2604`). C6 verifica resposta antes das vias de identidade (`app/chamados/juizes.py:1047-1072`). Por exemplo, `retratar_duplicata` não exige ausência de resposta (`app/chamados/services/fio.py:878-895`). Com `respondida_em`, texto, validação nula, sem decisão admin-dia e sem concordância comprovada com luz, o C6 relido retorna VIVA. Isso é compatível com a exceção solicitada, mas invalida a docstring “C6 mata essas vias” como afirmação incondicional.
- A porta comum julga instância recebida e não relê/trava (`app/chamados/models.py:2552-2576`). Caminho concorrente: S128 lê p com validação preenchida (`app/chamados/services/disputa_emissao.py:1537-1541`); outra resposta limpa validação no banco (`:1247-1266`); p antigo chega à porta e morre pelo primeiro degrau (`app/chamados/juizes.py:1033-1034`); como sua validação antiga não é nula, o save não a inclui (`app/chamados/models.py:2560-2563`). Grava trio sobre DB cuja validação foi limpa. C6 relido pode retornar VIVA pela resposta nova (`app/chamados/juizes.py:1047-1059`). Ocorrência real [nao verificado].
- Em execução sequencial dos chamadores examinados, não identifiquei alteração posterior de campo escalar C6 que prove imediatamente esse retorno à vida. O C6 pode usar relações cacheadas; a porta não substitui essas relações por leitura atual. `app/chamados/models.py:2565`; `app/chamados/juizes.py:778-796`.

4. **Q4 — recusa nominal restaura os seis campos aplicados, mas não oferece rollback geral da instância. Gravidade: nota.** Snapshot inclui trio e respondida/validada/autor; recusa e exceção do juiz restauram esses seis e removem `_pv_memo`. `app/chamados/models.py:2552-2553`; `:2566-2586`. Nenhum outro escalar é aplicado antes do C6. A relação `validada_por` em cache não é restaurada como objeto anterior, somente seu id; diferenças em objetos relacionados previamente modificados fora da porta [nao verificado]. Falha de `_lavrar`/`save` posterior à aprovação não está coberta por esse desfazer (`:2572-2576`), mas é exceção, não recusa C6.

5. **Q5 — limites de atomic e sobreviventes. Gravidade: nota.**

- Atomic específico da unidade começa em `app/chamados/services/materializacao.py:1109`; criação/reuso/anotação/auditoria/reconciliação da batida ocorrem no corpo chamado (`:860-909`); `_Reprovada` nova é lançada em `:1168` e capturada em `:1172`, após saída do atomic.
- Batida criada nessa tentativa e gravações SQL internas são revertidas; batida preexistente reusada continua existindo, mas anotação feita nessa tentativa cai. `:793-804`; `:880-909`; `:1109-1173`.
- Sobrevivem em Python `erros`, `reusos` e `_batida_ok`; `finally` converte avisos de reuso e chama `_carimbar_recusa` fora do atomic interno (`:1176-1186`). A mensagem nova não contém padrão COLISAO do catálogo (`app/chamados/catalogo/recusa.py:66`); grava motivo e pode anexar ocorrência (`app/chamados/services/materializacao.py:1023-1041`).
- Chamador administrativo pode envolver a unidade no atomic externo da validação (`app/chamados/services/validacao.py:395-427`), depois produzir trilha de erro (`:451-455`), e o lote da disputa não incrementa materializadas em `False` (`app/chamados/services/materializacao.py:1264-1275`).
- Reparador envolve atomic de dia e conta recusa quando unidade retorna `False` (`app/chamados/services/passivo_validado.py:511-524`), transfere esse contador se dia termina (`:548-554`); rollback superior de geometria pode desfazer motivo/nota internos, regravando recusa do ato fora do savepoint (`:543-547`).
- Callbacks `on_commit` registrados dentro do atomic revertido não chegam ao commit; efeitos externos síncronos de todos os signals chamados durante registro da batida [nao verificado]. [nao verificado]

6. **Q8 — rejeição legítima versus perda eterna.**

- A diferença comprovada é pergunta com texto sem `respondida_em` no fecho sem credor: a aplicação de `resposta='responder'` ativa o degrau de resposta C6, que pode devolver viva e causar restauração (`app/chamados/services/disputa_emissao.py:1635-1639`; `app/chamados/models.py:2556-2571`; `app/chamados/juizes.py:1047-1059`). O próprio teste foi invertido para fixar esse comportamento (`app/chamados/tests/test_o261_resposta_vazia.py:97-116`). Se era correto gravar no sentido da lei atual, ou se ficará para sempre pendente, depende do rastreio de leitores solicitado na Q9.
- Via derivada de fato não é, por si, evidência de falso negativo: `via_pelo_fato` escolhe adesão/cadastro/ausência/completude/encerrado (`app/chamados/juizes.py:1327-1371`), mas C6 pode acusar dia antes do último degrau (`:1081-1114`). O gate cumpre literalmente a exigência C6; ocorrência produtiva de rejeição indevida [nao verificado].
- Ausência: resposta e ausência continuam, mesmo quando irmã muda recusa (`app/chamados/services/materializacao.py:501-537`). Com texto, `resposta='validar'` dá morte no degrau 0 (`app/chamados/models.py:2560-2565`; `app/chamados/juizes.py:1033`); irmã muda pode permanecer sem carimbo enquanto C6 acusa célula antiga. Forever [nao verificado].
- `sem_turno_aberto` e `celula_absolveu` já exigiam C6 morto antes; nova guarda somente reavalia com carimbo (`app/chamados/reconciliador.py:398-405`; `:608-618`). Não identifiquei contraexemplo sequencial válido nessas duas.
- Fantasma produtivo exige texto vazio, marco aceso e C6 inicialmente vivo (`app/chamados/reconciliador.py:1031-1036`); ao aplicar carimbo, marco aceso mata no BUG117 (`app/chamados/juizes.py:1077-1079`). Portanto o caminho produtivo normal não sofre a recusa demonstrada pelo teste direto de `encerrar_fantasma` com texto (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:234-244`).
- `juiz_aprovou` pode recusar resposta esperando admin, mas esse comportamento não prova “deveria sair”; C6 tem justamente essa regra (`app/chamados/services/materializacao.py:1307-1315`; `app/chamados/juizes.py:1047-1059`). O fechamento que disparou S144 já ocorreu no receiver; se esse caminho deve reabrir pergunta/disputa é questão de leitores/Q9 (`app/chamados/signals.py:130-140`).

## 01/11 15:21 · sessao e640b463b2d7 · - · - · origem=Ronald (tmux)

1. **Bloqueia — limbo no fecho sem credor (Q9).** A recusa deixa `materializada_em`, `respondida_em`, `validada_em` nulos, conservando o texto (`app/chamados/models.py:2552-2571`, restauração `:2579-2586`); o chamador segue sem evento (`app/chamados/services/disputa_emissao.py:1638-1641`). Sem o instante, a resposta não entra no degrau de resposta do C6 (`app/chamados/juizes.py:1047`), podendo permanecer MORTA pelo degrau da célula (`:1095-1096`), enquanto a disputa permanece bloqueada pelo texto cru em `respostas_sem_veredito` (`app/chamados/models.py:1767-1778`). É precisamente a combinação que o teste alterado conserva (`app/chamados/tests/test_o261_resposta_vazia.py:104-113`).

2. **Bloqueia — sem rota normal para concluir esse texto.** App e badge exigem C6 VIVA (`app/chamados/services/pergunta_no_app.py:42-44`; `app/chamados/models.py:1720-1723`); fila de validação exige `respondida_em` não nulo (`app/chamados/juizes.py:1284-1288`); validação direta devolve `sem_resposta` (`app/chamados/services/validacao.py:363-365`). Crédito do painel exige pai/destino VIVO (`app/chamados/services/raiox.py:54-55`), que o ramo sem credor excluiu (`app/chamados/services/disputa_emissao.py:1608-1615`). A guarda que reabre o pai também exige instante de resposta (`app/chamados/juizes.py:1632-1638`), logo não resgata esse caso.

3. **Nota — há leitura e intervenção manual, não desaparecimento físico.** O fio manual do admin carrega todos os chamados e perguntas de disputas abertas (`app/chamados/services/fio.py:174-176`, `:218-239`), mas o template só mostra resposta/ações de validação dentro de `if perg.respondida_em` (`app/templates/chamados/partials/modal_fio.html:212-215`); sem instante mostra “O colaborador ainda não respondeu” (`:329-336`). A correção pelo colaborador também exige instante (`app/chamados/juizes.py:1277-1279`; `app/chamados/models.py:2126-2129`).

4. **Fila — vigia existe; cura geral não foi encontrada.** I20 detecta texto com instante nulo (`app/chamados/management/commands/auditar_invariantes_chamados.py:1101-1103`), com cron configurado (`app/config/crons.py:523`). A porta `carimbar_resposta_pela_trilha` pode restaurar instante mediante testemunha (`app/chamados/services/disputa_emissao.py:1399-1415`), mas o único chamador de produção encontrado é a cura nominal de 21 perguntas, com duas carimbáveis e recusa se o universo mudar (`app/chamados/management/commands/curar_perguntas_em_limbo.py:21-23`, `:45-53`, `:67-69`); ela está declarada como ação única, sem cron (`app/config/crons.py:1396-1397`).

5. **Nota — re-tentativas são condicionais.** Novo `save` de chamado terminal chama novamente S128 (`app/chamados/signals.py:75-92`), mas sem mudança de insumos repete a recusa (`app/chamados/services/disputa_emissao.py:1638-1640`). Reatamento pode salvar a pergunta sem destino de motivo reatável (`app/chamados/reconciliador.py:1401-1406`): achando histórico único, reaponta e chama S128 (`:1539-1545`), podendo alcançar o primeiro ramo com `resposta='validar'` (`app/chamados/services/disputa_emissao.py:1566-1568`). Isso não cobre todos os motivos/estados (`app/chamados/reconciliador.py:1402-1406`, `:1521-1522`). “Pendente para sempre” em produção permanece [nao verificado]; persistência sob mesmos insumos está demonstrada nesses caminhos.

6. **Fila — cron de becos não cura esse estado automaticamente.** Configurado com `--alarme` (`app/config/crons.py:644`), portanto não aplica (`app/chamados/management/commands/fechar_becos_disputa.py:30`, `:54-56`). Sem destino e sem instante nem classifica como beco, pois `_resolv(None)` é falso e entra em `vis` (`app/chamados/services/becos.py:21-32`); com destino terminal, o texto barra `pode_fechar`, tornando BARRADA (`:33`; `app/chamados/models.py:1767-1778`). O cron só alarma FECHÁVEIS (`app/chamados/management/commands/fechar_becos_disputa.py:67-70`).

7. **Fila — selo AST incompleto (Q6).** `CAMPOS` omite `via_resolucao` (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:35`); reconhecimento de `**` só cobre dict literal diretamente na chamada, sem alias e sem `defaults` de `update_or_create` (`:82-89`). Executei apenas o detector AST isolado, sem Django/DB/escrita: estes mutantes válidos produziram `[]`:

   ```python
   p.via_resolucao = 'fantasma'
   p.save(update_fields=['via_resolucao'])

   campos = {'materializada_em': None}
   qs.update(**campos)

   qs.update_or_create(pk=1, defaults={'materializada_em': None})
   ```

   Controle `p.materializada_em = agora()` foi detectado. O MORDE existente usa string sintética e nome de arquivo fabricado, sem inserir mutante na varredura da árvore (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:163-170`). Ele morde a forma direta, mas não prova integração real da mutação com o gate da árvore (`:123-128`, `:163-170`).

8. **Nota — L-111 (Q7).** Não encontrei chamada executável, template ou `getattr` remanescente de `fechar_por_competencia_trancada`; a definição foi substituída por lápide (`app/chamados/models.py:2501-2506`). Sobrou uma declaração obsoleta no esmeril, afirmando que a porta ainda existe (`bin/esmeril_chamado_censo.py:135-137`), e no retrato textual (`bin/esmeril_chamado_censo_de_hoje.txt:38`), além de prosa histórica (`app/docs/DOSSIES.md:46`).

## 01/11 15:20 · sessao 35954cc1eb48 · - · - · origem=Ronald (tmux)

Censo estático, sem banco/log. “Escritor” abaixo separa atribuição em memória de persistência; comentários/documentação não contam como leitores executáveis.

1. **Escritores dos campos do carimbo**
   
   | Campo | Escritores encontrados | Situação |
   |---|---|---|
   | `materializada_em` | `app/chamados/models.py:2529` `_escrever_carimbo`; `:2624` `descarimbar_em_lote` | Dentro da nova porta. |
   | `materializada_por/_id` | `app/chamados/models.py:2530`; `:2624` | Dentro da nova porta. |
   | `via_resolucao` | Os anteriores, `app/chamados/models.py:2531`, `:2624`; **também** `app/chamados/models.py:2633` `encerrar_por_fato_no_espelho`, save `:2634`; **também** `app/chamados/services/relavra_via.py:123` `relavrar`, save `:125` | Há dois escritores da via fora dos novos métodos. São portas de entidade declaradas, mas não passam por `carimbar`/`carimbar_identidade`. |

   O homônimo `ItemRegularizacao.materializada_em/materializada_por` é escrito em `app/chamados/services/regularizacao_ext.py:198`–`:200`; não é `PerguntaDisputa` (`app/chamados/models.py:3132`–`:3133`).

2. **Escritores dos demais campos**

   - `respondida_em`: `app/chamados/models.py:1441` apenas na cópia usada para julgar ressuscitação; `:2558` aplica pela porta; `:2583` desfaz em memória; `:2624` recebe `**outros` de `ressuscitar_perguntas` (`:1488`). `app/chamados/services/disputa_emissao.py:1248` resposta do colaborador; `:1358` dicionário da reabertura aplicado por `:1378` ou enviado à porta de lote em `:1377`; `:1394` resposta automática; `:1410` cura pela trilha. São portas declaradas de resposta/identidade, embora não sejam todos métodos da nova porta do carimbo.
   - `validada_em`: `app/chamados/models.py:1440` só na cópia; `:2561` aplica; `:2584` desfaz; lote `:2624` recebe `:1488`. `app/chamados/services/disputa_emissao.py:1260` desfaz validação quando chega resposta nova; `:1358`/`:1378` reabre. `app/chamados/services/validacao.py:131` valida; `:188` devolve validação recusada.
   - `validada_por/_id`: `app/chamados/models.py:2562`, `:2585`, lote `:2624` com `:1488`; `app/chamados/services/disputa_emissao.py:1261`, `:1358`/`:1378`; `app/chamados/services/validacao.py:132`, `:189`.
   - Trio `veredito_lavrado/julgado_em/janela_expira_em`: atribuições canônicas em `app/chamados/veredito_lavrado.py:227`–`:229`, save `:231`; persistência em lote `:207`, lista em `:194`. O command **atribui em memória fora desse módulo** (`app/chamados/management/commands/backfill_veredito_lavrado.py:138`–`:140`) e grava pela porta em `:143`/`:146`. Saves de lavra calculada com `salvar=False` existem em `app/chamados/models.py:2449`, `:2575`, `app/chamados/services/fio.py:847`–`:848`, `app/chamados/services/relavra_via.py:125`–`:126`.

3. **Leitores diretos/ORM do carimbo, por campo**

   **`materializada_em`:**

   - `app/chamados/juizes.py:275`, `:624`, `:650`, `:791`, `:1042`, `:1071`, `:1077`, `:1107`, `:1125`, `:1633`.
   - `app/chamados/models.py:1421`, `:1768`, `:2477`, `:2480`, `:2490`, `:2552`, `:2596`, `:2597`, `:2642`, `:2654`, `:2671`, `:2710`, `:2712`.
   - `app/chamados/reconciliador.py:366`, `:583`, `:998`, `:1090`, `:1100`, `:1405`.
   - `app/chamados/views.py:1257`.
   - `app/chamados/veredito_lavrado.py:163`–`:164`.
   - `app/chamados/services/disputa_emissao.py:211`, `:695`, `:874`, `:894`, `:1117`, `:1539`, `:1620`.
   - `app/chamados/services/materializacao.py:419`, `:510`, `:571`, `:573`, `:578`, `:617`, `:1023`, `:1027`, `:1261`, `:1308`.
   - `app/chamados/services/validacao.py:180`, `:182`, `:422`, `:433`, `:451`.
   - `app/chamados/services/fabrica_da_celula.py:123`.
   - `app/chamados/services/raiox.py:133`, `:173`.
   - `app/chamados/services/relavra_via.py:179`, `:229`.
   - `app/chamados/services/historico.py:103`.
   - `app/chamados/services/painel.py:67`.
   - `app/ponto/services/procedencia_batida.py:74`.
   - Commands: `app/ponto/management/commands/reconciliar_fantasmas.py:212`; `app/chamados/management/commands/encerrar_perguntas_com_fato.py:32`; `auditar_invariantes_chamados.py:469`, `:537`, `:587`, `:904`, `:954`, `:1072`, `:1153`, `:1570`, `:1598`, `:1763`; `reabrir_orfas_enterradas.py:67`, `:87`, `:90`, `:128`, `:141`; `auditar_disputas.py:71`, `:150`.

   **`materializada_por/_id`:**

   - `app/chamados/models.py:2552`, `:2597`.
   - `app/chamados/reconciliador.py:1091`, `:1101`.
   - `app/chamados/services/materializacao.py:618`.
   - `app/ponto/services/procedencia_batida.py:71`–`:72`.
   - `app/chamados/management/commands/reabrir_orfas_enterradas.py:129`, `:142`.
   - Leitura genérica de todos os campos da reversão em `app/chamados/services/passivo_validado.py:64`–`:66`, `:83`.

   **`via_resolucao`:**

   - `app/chamados/juizes.py:624`, `:635`, `:790`, `:1042`, `:1071`.
   - `app/chamados/models.py:1838`, `:2489`, `:2552`, `:2631`, `:2642`, `:2654`, `:2671`, `:2678`, `:2710`, `:2712`.
   - `app/chamados/reconciliador.py:1089`, `:1100`.
   - `app/chamados/services/materializacao.py:423`, `:572`, `:614`, `:1023`.
   - `app/chamados/services/disputa_emissao.py:1628`, `:1643`, `:1646`.
   - `app/chamados/services/fio.py:882`; `fabrica_da_celula.py:104`; `explicador.py:95`; `competencia_trancada.py:244`, `:250`; `passivo_validado.py:140`–`:142`, `:515`; `relavra_via.py:116`, `:119`, `:121`, `:153`, `:179`, `:230`.
   - `app/ponto/services/cartorio.py:1429`; `flip_auto.py:129`.
   - Commands: `app/chamados/management/commands/disparar_perguntas_competencia.py:127`; `encerrar_perguntas_com_fato.py:33`, `:42`, `:49`, `:66`, `:76`; `auditar_invariantes_chamados.py:201`, `:205`, `:432`, `:541`, `:954`, `:1706`; `reabrir_orfas_enterradas.py:67`, `:128`, `:141`, `:149`; `relavrar_via_i6.py:58`, `:59`, `:62`, `:66`, `:68`, `:88`; `dedup_perguntas_fato.py:74`.
   - Template: `app/templates/chamados/partials/_card_atendimento.html:186`.

4. **Leitores de resposta e validação**

   **`respondida_em`:**

   - Juiz/modelo: `app/chamados/juizes.py:789`, `:1047`, `:1157`, `:1277`, `:1284`, `:1287`, `:1634`; `app/chamados/models.py:1722`, `:1769`, `:2479`, `:2482`, `:2553`, `:2557`, `:2708`, `:2780`.
   - API/telas: `app/api/views.py:1009`, `:1175`, `:1187`, `:2634`, `:2688`, `:2705`; `app/api/views_mensageria.py:569`–`:570`; `app/chamados/views.py:1280`, `:1416`; `views_cobrar.py:169`, `:176`; `relatorios_disputa.py:20`.
   - Serviços: `app/chamados/services/aviso_pendencia.py:13`; `becos.py:30`; `acoes_disputa.py:95`, `:130`; `cobranca.py:229`; `declaracao_texto.py:363`, `:365`, `:632`; `disputa_emissao.py:1372`, `:1408`, `:1446`, `:1559`, `:1630`, `:1647`; `fio.py:336`, `:656`; `furo_sem_canal.py:81`; `historico.py:99`; `materializacao.py:615`, `:1259`; `painel.py:67`; `validacao.py:363`, `:537`, `:542`, `:558`, `:877`; `verbos.py:311`, `:312`, `:318`, `:321`; `vigia_fio.py:630`, `:737`; `raiox.py:136`; `veredito_lavrado.py:167`–`:168`.
   - Outros: `app/ponto/supra_juiz.py:515`, `:518`; `app/ponto/management/commands/reconciliar_grade.py:461`; `bin/censo_folha.py:28`–`:29`.
   - Commands: `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`, `:69`; `auditar_invariantes_chamados.py:177`, `:591`, `:1102`, `:1568`, `:1599`.
   - Templates: `app/templates/chamados/partials/modal_fio.html:184`, `:212`, `:215`; `_card_atendimento.html:94`, `:100`, `:137`, `:155`, `:384`–`:385`; `_pergunta_card.html:24`, `:29`, `:32`.

   **`validada_em`:**

   - `app/chamados/juizes.py:789`, `:1033`, `:1634`.
   - `app/chamados/models.py:293`, `:327`, `:1768`, `:2475`, `:2553`, `:2560`, `:2719`.
   - `app/chamados/reconciliador.py:584`, `:998`.
   - `app/chamados/services/disputa_emissao.py:1250`, `:1251`, `:1373`, `:1620`; `materializacao.py:616`, `:1260`; `validacao.py:139`, `:180`, `:187`, `:315`, `:366`, `:433`, `:534`, `:685`, `:706`; `declaracao_texto.py:208`; `acoes_disputa.py:341`; `verbos.py:240`, `:246`; `painel.py:67`, `:268`–`:271`; `historico.py:101`; `vigia_carimbo.py:98`.
   - `app/ponto/services/procedencia_batida.py:74`; `flip_auto.py:130`; `bin/censo_folha.py:29`.
   - Commands: `app/chamados/management/commands/encerrar_perguntas_com_fato.py:32`; `auditar_invariantes_chamados.py:204`, `:1072`, `:1569`, `:1598`, `:1762`, `:1774`; `reabrir_orfas_enterradas.py:68`.
   - Templates: `app/templates/chamados/partials/modal_fio.html:223`; `_pergunta_card.html:20`, `:23`, `:42`.

   **`validada_por/_id`:**

   - `app/chamados/models.py:2553`.
   - `app/chamados/services/disputa_emissao.py:1250`, `:1373`; `materializacao.py:616`; `validacao.py:139`, `:180`, `:315`; `verbos.py:240`–`:244`; `painel.py:260`.
   - `app/ponto/services/procedencia_batida.py:71`–`:72`.
   - `app/chamados/management/commands/auditar_invariantes_chamados.py:1073`.

5. **Leitores do trio de lavra**

   - `veredito_lavrado`: `app/chamados/veredito_lavrado.py:179`; `app/chamados/services/raiox.py:32`, `:134`, `:182`–`:183`; `relavra_via.py:122`, `:128`, `:135`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1169`; `backfill_veredito_lavrado.py:120`, `:124`, `:134`.
   - `julgado_em`: `app/chamados/veredito_lavrado.py:179`, `:185`, `:189`; `app/chamados/services/raiox.py:31`.
   - `janela_expira_em`: `app/chamados/veredito_lavrado.py:182`, `:185`; `app/chamados/services/raiox.py:32`; `app/chamados/management/commands/backfill_veredito_lavrado.py:121`, `:135`.
   - Todos também são lidos genericamente pelo snapshot `app/chamados/services/passivo_validado.py:64`–`:66`, `:83`.

6. **Falhas concretas do selo novo — fila ou bloqueia conforme exigência do despacho**

   - O selo não protege `via_resolucao`: `CAMPOS` contém apenas `materializada_em`, `materializada_por`, `_id` (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:35`). Um mutante que só escreve a via passa; dois escritores reais continuam fora da porta do carimbo (`app/chamados/models.py:2633`, `app/chamados/services/relavra_via.py:123`).
   - `update_or_create(defaults={'materializada_em': ...})` não é detectado: o teste só examina nome direto do kwarg ou `**` cujo valor seja um `ast.Dict`; `defaults` não casa (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:83`–`:93`).
   - `d = {'materializada_em': ...}; qs.update(**d)` também escapa: dicionário literal atribuído a variável não recebe tratamento de escrita, e o `**d` não é `ast.Dict` (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:63`–`:93`). O teste cobre `dict(materializada_em=...)` e subscript, formas diferentes (`:145`–`:155`).
   - `bulk_update` com lista em variável escapa à detecção desse selo, pois só percorre a expressão do argumento recebido, sem resolver o nome (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:94`–`:98`). O trio da lavra usa exatamente lista em constante no código real (`app/chamados/veredito_lavrado.py:194`, `:207`), embora sejam outros campos.

## 01/11 14:40 · sessao 67798c11cab9 · COBERTURA-PARCIAL-UM-JUIZ · censo · origem=Code

**O dado fornecido não distingue “a cura mudou a decisão” de “a versão fez o dia chegar novamente à decisão”.** A fábrica só recebe datas julgadas; impressão igual pode impedir essa chegada. Para os três casos da comp09, falta saber a impressão anterior e a declaração efetivamente usada em B do controle. (`app/ponto/services/cartorio.py:1214`, `app/ponto/services/cartorio.py:1282`, `app/ponto/services/cartorio.py:878`; estado desses casos: `[nao verificado]`)

**1. Caminho do cartório até o chamado**

O caminho comum aos dois subtipos é:

1. A passada seleciona as células anteriores a hoje e chama `julgar_colab`. O corpo monta a grade, compara a impressão e, se não pular, classifica o dia, registra sua data em `out['cods']`, lavra ata/veredito e carimba a impressão. (`app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/management/commands/processar_cartorio.py:65`, `app/ponto/services/cartorio.py:987`, `app/ponto/services/cartorio.py:1218`, `app/ponto/services/cartorio.py:1269`, `app/ponto/services/cartorio.py:1282`, `app/ponto/services/cartorio.py:1653`, `app/ponto/services/cartorio.py:1675`)

2. Depois do corpo, com `apply_`, `julgar_colab` chama `perguntar_pela_celula` para as datas presentes em `out['cods']`. (`app/ponto/services/cartorio.py:864`, `app/ponto/services/cartorio.py:868`, `app/ponto/services/cartorio.py:878`)

3. `motivos_a_perguntar` exige: colaborador ativo; dia não calado por `dia_sem_trabalho`; data não futura; veredito preenchido pertencente a `VEREDITOS_ACUSAM`; e motivo ainda não perguntado naquele dia, desconsiderando duplicatas. Também calcula quais marcos venceram no instante. (`app/chamados/services/fabrica_da_celula.py:80`, `app/chamados/services/fabrica_da_celula.py:82`, `app/chamados/services/fabrica_da_celula.py:87`, `app/chamados/services/fabrica_da_celula.py:88`, `app/chamados/services/fabrica_da_celula.py:103`, `app/chamados/services/fabrica_da_celula.py:111`)

4. `motivos_pela_ata` → `_ata_do_dia` → `_marcos_faltantes` → `grade_da_celula`. O leitor converte lâmpada apagada em `missing`; `_marcos_faltantes` seleciona esses marcos e atribui sua posição pela ordem da ata. (`app/chamados/services/fabrica_da_celula.py:49`, `app/chamados/services/disputa_emissao.py:827`, `app/chamados/services/declaracao_texto.py:82`, `app/escala/services/leitor_celula.py:105`, `app/escala/services/leitor_celula.py:119`, `app/chamados/services/declaracao_texto.py:110`)

| Marco faltante na ata | Motivo produzido |
|---|---|
| Último par, tipo `S`: posição `fim` | `orfao_14h` |
| Par intermediário, tipo `S`: posição `int_ida` | `intervalo_saida` |

A posição e o mapa estão em `app/chamados/services/declaracao_texto.py:113`, `app/chamados/services/declaracao_texto.py:118` e `app/chamados/services/declaracao_texto.py:42`. **Nesse ramo, `orfao_14h` é o nome associado à saída final; não há teste literal de 14 horas.** O filtro temporal usa os marcos vencidos. (`app/chamados/services/fabrica_da_celula.py:27`, `app/chamados/services/fabrica_da_celula.py:59`)

5. A fábrica chama `perguntar`, um motivo por vez, com contexto `{motivo: {'data': dia}}`, dentro de `push_disputa_silenciado`. (`app/chamados/services/fabrica_da_celula.py:120`, `app/chamados/services/fabrica_da_celula.py:124`, `app/chamados/services/fabrica_da_celula.py:159`)

6. `criar_ou_atualizar_disputa` passa `subcategoria=motivos_lista[0]` ao núcleo. Havendo disputa aberta, agrega motivos; sem ela, chama `ChamadoColaborador.abrir` com categoria/módulo `disputa_supervisao`, subcategoria e contexto recebidos. O nascimento efetivo fica no `get_or_create`. (`app/chamados/services/disputa_emissao.py:598`, `app/chamados/services/disputa_emissao.py:601`, `app/chamados/services/disputa_emissao.py:467`, `app/chamados/services/disputa_emissao.py:471`, `app/chamados/services/disputa_emissao.py:504`, `app/chamados/services/disputa_emissao.py:514`, `app/chamados/models.py:930`)

**O pai de disputa nasce antes das perguntas e antes do eventual c2.** Depois, `gerar_perguntas_disputa` abre a pergunta e tenta reatá-la quando nasceu sem destino. (`app/chamados/services/disputa_emissao.py:605`, `app/chamados/services/disputa_emissao.py:389`, `app/chamados/services/disputa_emissao.py:404`)

**2. Alterações do diff que alcançam esse caminho**

Abaixo, identifico as linhas alteradas pela sua localização **na árvore da cura**, correspondente aos hunks fornecidos.

| Linhas alteradas | Mudança e condição de efeito |
|---|---|
| `app/ponto/services/cartorio.py:111` | Acrescenta `(3,'2026-09')`. Pode fazer chegar ao julgamento uma célula cuja impressão anterior continha outra versão; não muda sozinho o motivo emitido. A versão entra no hash em `app/ponto/services/cartorio.py:393`; o pulo ocorre em `app/ponto/services/cartorio.py:1218`. |
| `app/ponto/services/cartorio.py:349`, `app/ponto/services/cartorio.py:1054`, `app/ponto/services/cartorio.py:1210` | A impressão passa a incluir os índices dos marcos `dispensada`. Se o período muda quais marcos são dispensados, a impressão muda mesmo quando o agregado continua, por exemplo, `fora`. Sem marco dispensado, o helper devolve o insumo anterior. (`app/ponto/services/cartorio.py:358`) |
| `app/escala/utils.py:353`, `app/escala/utils.py:365`, `app/escala/utils.py:1233`, `app/escala/utils.py:1243` | A grade usa os instantes da cadeia do turno e dispensa somente `missing` dentro do período declarado. Um `S` fora continua apagado e pode gerar um dos dois motivos; dentro deixa de ser faltante. Sem período ou sem deslocamento conhecido, esse bloco não dispensa. (`app/escala/utils.py:1247`) |
| `app/ponto/turnos.py:2656`, `app/escala/services/leitor_celula.py:416` | As horas reais passam a usar deslocamentos da cadeia do turno, inclusive madrugada em D+1. Isso pode mudar o agregado `dentro/fora/nao_sei` e, portanto, a classificação de uma parcial noturna. (`app/ponto/turnos.py:2662`, `app/escala/services/leitor_celula.py:420`) |
| `app/ponto/services/cartorio.py:470`, `app/ponto/services/cartorio.py:641` | A ata conserva `dispensada` por marco e passa a carregar `cobertura_parcial=True`. Esse segundo carimbo permite à fábrica reconhecer que `tipo_dia='ausencia'` ainda tem resto cobrável. (`app/chamados/juizes.py:424`) |
| `app/ponto/services/cartorio.py:665` | A dispensa integral passa a depender do carimbo da grade, substituindo o teste cru de minutos. Na parcial, esse bloco não acende todos os marcos; preserva o resultado por período vindo da grade. No dia inteiro, pode retirar faltantes e impedir perguntas. (`app/ponto/services/cartorio.py:667`, `app/ponto/services/cartorio.py:672`) |
| `app/chamados/juizes.py:398`, `app/chamados/juizes.py:424` | **Abre o portão principal:** ata `ausencia` com `cobertura_parcial` deixa de significar dia sem trabalho, desde que `trabalha is not False`. No controle, o retorno anterior calava qualquer ata preenchida com tipo diferente de `trabalho`. (`app/chamados/juizes.py:426`, linha anterior no diff fornecido) |
| `app/chamados/services/declaracao_texto.py:97` | **Abre o segundo portão principal:** `_marcos_faltantes` passa a percorrer o dia de ausência parcial; antes, descartava todo `tipo_dia != 'trabalho'`. Só retorna os marcos que continuam `missing`. (`app/chamados/services/declaracao_texto.py:98`, `app/chamados/services/declaracao_texto.py:110`) |
| `app/ponto/services/cartorio.py:1229`, `app/escala/services/leitor_celula.py:91`, `app/escala/services/leitor_celula.py:127`, `app/escala/services/leitor_celula.py:343` | Marco dispensado deixa de contar como batida/casamento e conserva seu marcador na leitura. Isso evita alterar artificialmente o número de batidas e o enquadramento do dia. |
| `app/ponto/supra_juiz.py:311`, `app/ponto/supra_juiz.py:390`, `app/ponto/supra_juiz.py:437`, `app/ponto/supra_juiz.py:530` | Modifica a decisão consumida pelo portão de veredito: parcial sem resto não acusa furo sem batida; parcial com fato, período `fora` e marco faltante pode produzir `FURO_PARCIAL`; parcial com batida sem par passa a ser examinada; cobrança viva do resto deixa de produzir `COBRANCA_EM_DIA_COBERTO`. (`app/ponto/supra_juiz.py:313`, `app/ponto/supra_juiz.py:396`, `app/ponto/supra_juiz.py:439`, `app/ponto/supra_juiz.py:531`) |

O critério de parcial não é somente o nome do tipo: exige tipo no catálogo de parciais, um único dia e `0 < minutos < 480`. O período tem bordas inclusivas; `hora_fim < hora_ini` atravessa a meia-noite. (`app/ponto/turnos.py:2595`, `app/ponto/turnos.py:2632`, `app/ponto/turnos.py:2636`, `app/ponto/turnos.py:2639`)

Há mudanças próximas que precisam ser separadas do nascimento desses pais:

- `_coberto_por_ausencia` passa de “existe alguma não-falta cobrindo” para a ausência **vencedora**, integral ou sem período. Isso altera o agrupador/complementador e o c2; **não é veto direto de `gerar_perguntas_disputa` neste caminho**, que chama `PerguntaDisputa.abrir` diretamente. (`app/chamados/services/materializacao.py:78`, `app/chamados/services/materializacao.py:81`, `app/chamados/services/disputa_emissao.py:763`, `app/chamados/services/disputa_emissao.py:389`, `app/chamados/reconciliador.py:828`)

- `_celula_furada` deixa de considerar qualquer ausência suficiente para regularizar o dia; passa a exigir ausência que explique o dia inteiro. É mudança de premissa/regularização, não o teste que escolhe o subtipo do pai acima. (`app/chamados/juizes.py:446`, `app/chamados/juizes.py:447`, `app/chamados/models.py:1247`)

- `parcial_so_lista`, as novas âncoras e o espelhamento dos marcos alteram os portões do **chamado do dia**. A fábrica do pai `disputa_supervisao` não aplica `parcial_so_lista`. Portanto, ausência de fato novo pode calar `batida_ausente` e ainda permitir o pai de disputa. (`app/ponto/services/cartorio.py:1560`, `app/ponto/services/cartorio.py:1578`, `app/ponto/services/cartorio.py:1630`, `app/ponto/services/cartorio.py:1643`, `app/chamados/reconciliador.py:848`, `app/chamados/services/fabrica_da_celula.py:146`, `app/chamados/services/fabrica_da_celula.py:158`)

- Pela mesma razão, **“parcial sem período” não é veto universal desse pai**: `_marcos_faltantes` aceita a parcial sem testar o período, e a fábrica admite também o veredito `indefinida`. (`app/chamados/services/declaracao_texto.py:98`, `app/chamados/services/fabrica_da_celula.py:88`, `app/escala/models.py:1735`)

**3. Os três `orfao_14h` e a subida de versão**

**O controle nativo não contém a subida v3.** A linha removida do diff declara `((1,'2026-09'), (2,'2026-10'))`; a cura acrescenta `(3,'2026-09')`. Para comp09, `versao_para` retorna respectivamente 1 e 3, pois escolhe a maior versão cujo horizonte alcance a competência. (`app/ponto/services/cartorio.py:111`, comparação fornecida no diff; `app/ponto/services/cartorio.py:205`, `app/ponto/services/cartorio.py:208`)

Assim:

- **B do controle não rejulga esses dias “por v3” apenas por executar o código do controle.** Se a impressão continua igual, pula antes da fábrica. Pode rejulgar por outro insumo diferente ou por `forcar`. (`app/ponto/services/cartorio.py:1214`, `app/ponto/services/cartorio.py:1218`)
- **Se a sonda injetou explicitamente v3 também no controle**, uma célula antes impressa com `regra:1` teria a igualdade rompida também ali. Essa injeção não é demonstrada pelo rótulo “B (bump v3)” nem pelos contadores fornecidos: `[nao verificado]`. O mecanismo é o mesmo hash e ramo de pulo. (`app/ponto/services/cartorio.py:393`, `app/ponto/services/cartorio.py:1208`, `app/ponto/services/cartorio.py:1218`)

| Hipótese | Compatibilidade pelo código |
|---|---|
| **(i) Só ganhou nova passagem; o controle já emitiria se julgasse o mesmo estado** | Compatível. Um dia de trabalho com saída final apagada, veredito acusador e sem pergunta anterior já satisfaz o caminho; a versão pode apenas fazê-lo chegar à fábrica. (`app/chamados/services/fabrica_da_celula.py:80`, `app/chamados/services/fabrica_da_celula.py:111`, `app/chamados/services/declaracao_texto.py:44`) |
| **(ii) Precisa da mudança funcional da cura** | Também compatível. Se a grade/ata representa ausência parcial com saída final ainda apagada, os antigos descartes por `ausencia` impediam esse caminho, mesmo forçando o julgamento. (`app/chamados/juizes.py:424`, `app/chamados/juizes.py:426`, `app/chamados/services/declaracao_texto.py:98`, comparação com linhas removidas do diff) |

**Qual hipótese vale para col150, col304 e col476: `[nao verificado]`.** O DET não mostra ausência, período, ata anterior, impressão anterior nem a declaração efetiva da sonda B. Os totais agregados não permitem reconstruir esses valores por célula. (`app/ponto/services/cartorio.py:1205`, `app/ponto/services/cartorio.py:1214`)

A competência é calculada pelo corte da empresa, não pelo mês civil. Com corte 21, os três dias informados pertencem à comp09; o corte efetivo da empresa 3 permanece `[nao verificado]`. (`app/ponto/janelas.py:24`, `app/ponto/janelas.py:50`, `app/ponto/janelas.py:60`)

**4. Dados que decidem e consultas mínimas, sem executar**

| Caso fornecido | Dado decisivo a confrontar no estado anterior à passada |
|---|---|
| col222 — 2026-10-28 — `intervalo_saida` `[nao verificado]` | Ata/grade com `S` intermediário apagado; ausência aprovada vencedora, parcialidade e posição desse marco perante o período; veredito e pergunta anterior. (`app/chamados/services/declaracao_texto.py:118`, `app/escala/utils.py:1187`, `app/escala/utils.py:1247`, `app/chamados/services/fabrica_da_celula.py:88`, `app/chamados/services/fabrica_da_celula.py:103`) |
| col150 — 2026-09-11 — `orfao_14h` `[nao verificado]` | Os mesmos dados, agora para o `S` final, mais impressão anterior e versão efetivamente usada em B. (`app/chamados/services/declaracao_texto.py:115`, `app/chamados/services/declaracao_texto.py:44`, `app/ponto/services/cartorio.py:1208`, `app/ponto/services/cartorio.py:1214`) |
| col304 — 2026-08-31 — `orfao_14h` `[nao verificado]` | Mesmo recorte de col150; confirmar competência pelo corte da empresa. (`app/ponto/janelas.py:60`, `app/ponto/services/cartorio.py:205`) |
| col476 — 2026-08-21 — `orfao_14h` `[nao verificado]` | Mesmo recorte de col150; confirmar especialmente a borda da competência pelo corte. (`app/ponto/janelas.py:27`, `app/ponto/janelas.py:60`) |

As consultas ORM abaixo leem as tabelas dos modelos indicados. Devem ser aplicadas **ao snapshot anterior às respectivas passadas** para decidir o contrafactual histórico; o cartório sobrescreve ata e impressão ao julgar. (`app/ponto/services/cartorio.py:1655`, `app/ponto/services/cartorio.py:1675`)

```python
# Proposta de consulta; não executada.
from datetime import date, datetime, time, timedelta
from django.db.models import Q
from django.utils import timezone
from colaboradores.models import Empresa, Colaborador
from escala.models import CelulaDia
from ponto.models import Ausencia, Batida, TipoAusencia
from chamados.models import PerguntaDisputa, DisputaSupervisao

casos = [
    (222, date(2026, 10, 28)),
    (150, date(2026, 9, 11)),
    (304, date(2026, 8, 31)),
    (476, date(2026, 8, 21)),
]

Empresa.objects.filter(pk=3).values("id", "dia_inicio_competencia")
TipoAusencia.objects.filter(ativo=True).values("codigo", "parcial")

for cid, d in casos:
    Colaborador.objects.filter(pk=cid, empresa_id=3).values(
        "id", "situacao", "isento_ponto",
        "data_admissao", "data_demissao"
    )

    CelulaDia.objects.filter(
        colaborador_id=cid, data__range=(d - timedelta(days=1), d)
    ).values(
        "id", "data", "trabalha", "escala_geradora_id", "dna",
        "ata", "veredito", "protestos", "julgada_em", "impressao",
        "editada_em", "regenerada_em"
    )

    Ausencia.objects.filter(colaborador_id=cid).exclude(
        status="rejeitada"
    ).filter(
        Q(data=d)
        | Q(data__lte=d, data_fim__gte=d)
        | Q(data__lte=d, data_fim__isnull=True,
            tipo__in=Ausencia.TIPOS_EM_ABERTO)
    ).order_by("-data", "-pk").values(
        "id", "data", "data_fim", "tipo", "status",
        "minutos", "hora_ini", "hora_fim",
        "criado_em", "atualizado_em", "aprovada_em"
    )

    t0 = timezone.make_aware(
        datetime.combine(d - timedelta(days=1), time.min))
    t1 = timezone.make_aware(
        datetime.combine(d + timedelta(days=2), time.min))
    Batida.objects.filter(
        colaborador_id=cid, timestamp__gte=t0, timestamp__lt=t1
    ).values(
        "id", "timestamp", "tipo", "is_intervalo",
        "criado_em", "retratada_em"
    )

    PerguntaDisputa.objects.filter(
        disputa__colaborador_id=cid, data_evento=d
    ).values(
        "id", "motivo", "via_resolucao", "disputa_id",
        "celula_id", "chamado_destino_id",
        "respondida_em", "validada_em", "materializada_em"
    )

    # A disputa aberta é procurada por colaborador, sem filtro de data.
    DisputaSupervisao.objects.filter(colaborador_id=cid).values(
        "id", "motivos", "contexto", "chamado_id",
        "aberta_em", "fechada_em"
    )
```

O filtro e a ordem das ausências reproduzem a autoridade de cobertura. **Para a grade, tomar a primeira aprovada; para cobrança, a primeira não rejeitada** — podem ser registros distintos. (`app/ponto/turnos.py:2514`, `app/ponto/turnos.py:2516`, `app/ponto/turnos.py:2552`, `app/ponto/turnos.py:2557`, `app/escala/utils.py:1187`)

O recorte de batidas preserva também as retratadas para examinar o fato novo; a retração pode ser justamente o evento ocorrido depois do julgamento. A véspera da célula atende ao leitor de faltantes. (`app/ponto/services/cartorio.py:292`, `app/ponto/services/cartorio.py:294`, `app/chamados/services/declaracao_texto.py:81`)

Se houver parcial noturna, consultar ainda a geradora e os vínculos candidatos, sem substituí-los por “escala ativa”:

```python
from escala.models import EscalaColaborador, TipoEscala
from escala.alimentacao import q_vinculo_cobre_a_data

# gid vem de CelulaDia.escala_geradora_id.
EscalaColaborador.objects.filter(colaborador_id=cid).filter(
    Q(pk=gid) | q_vinculo_cobre_a_data(d)
).values()

# tids vêm dos vínculos retornados; incluir template_pai quando houver.
TipoEscala.objects.filter(pk__in=tids).values()
```

A geradora tem precedência; os marcos/pausas e a cadeia determinam se a madrugada pertence a D+1 e se o marco está dentro do período. (`app/escala/alimentacao.py:196`, `app/escala/alimentacao.py:200`, `app/escala/utils.py:353`, `app/escala/utils.py:365`; herança do template: `app/escala/models.py:806`)

Para testar especificamente o veto de fato novo dos chamados do dia, acrescentar:

```python
from ponto.models import Justificativa

Justificativa.objects.filter(
    colaborador_id=cid, data_turno=d
).values("id", "status", "criado_em", "atualizado_em", "aprovada_em")
```

Esses carimbos, junto com ausência, batida, edição/regeneração e marcos vencidos, são comparados com o julgamento anterior por `houve_fato_novo`. **Eles não substituem o exame do portão próprio do pai de disputa.** (`app/ponto/services/cartorio.py:297`, `app/ponto/services/cartorio.py:301`, `app/ponto/services/cartorio.py:307`, `app/ponto/services/cartorio.py:339`, `app/chamados/services/fabrica_da_celula.py:146`)

Nenhuma consulta ao banco resolve qual constante a **sonda** instalou em B: isso depende do instrumento executado, ausente do dado fornecido. `[nao verificado]`

## 01/11 14:39 · sessao e24157d16709 · COBPARC-ATA-DEFASADA-09 · censo · origem=Code

O dado separa **código atual × ata gravada** de **efeito adicional da subida de versão**; o registro local reproduz os cinco deltas e relata controle sem a cura COBPARC igual à cura, campo a campo. Isso não identifica, sozinho, o commit causador nem o dono de cada dia. (`app/docs/BACKLOG.md:759–760`; `app/ponto/management/commands/e6_oraculo.py:98–110`)

A correspondência histórica exata **hash → diff**, assim como um censo exaustivo dos commits, fica **[nao verificado]**: esta cópia não disponibiliza o histórico Git. Abaixo, os hashes vêm do anexo e as linhas apontam o comportamento presente na cópia; não atribuo os deltas individuais por coincidência numérica. [nao verificado]

**1. Escritores e leitores**

O caminho se divide: **FechamentoMensal não é calculado somando DiaPago**. O fechamento calcula os valores e entrega os mesmos períodos e mapas à lavratura de DiaPago; a soma de DiaPago existe como leitor separado de conferência. (`app/ponto/services/fechamento.py:877–878, 1028–1032, 1168–1185`; `app/ponto/services/dia_pago.py:548–561`)

| Campo | Quem produz e escreve | Quem lê e onde chega ao fechamento |
|---|---|---|
| `dias_previstos` | Não é rubrica de DiaPago: é contagem dos dias da grade cujo `tipo_dia` seja `trabalho` ou `ausencia`. O fechamento escreve `len(_dt_trab)`. (`app/ponto/services/dia_pago.py:99`; `app/ponto/services/fechamento.py:1014–1017, 1052`) | A grade do fechamento lê a célula/ata; `tipo_dia` vem da ata, com alternativa em `cel.trabalha`. (`app/folha/export.py:318–327`; `app/escala/services/leitor_celula.py:345–346`) |
| `minutos_previstos` | A grade soma segmentos previstos E→S; o cartório copia o resultado para a ata; a porta persiste a ata. (`app/escala/utils.py:1569–1590`; `app/ponto/services/cartorio.py:599–600`; `app/ponto/portas/celula.py:767–775`) | O leitor prefere `ata.minutos_previstos`; sem essa chave consulta `minutos_previstos_do_dia`. O fechamento soma trabalho/ausência; DiaPago recebe o mapa diário. (`app/escala/services/leitor_celula.py:347–354`; `app/ponto/services/fechamento.py:1014–1016, 1051, 1180`; `app/ponto/services/dia_pago.py:286–290`) |
| `minutos_realizados` | `realizado_dos_turnos` mede os turnos, descontando pausa efetiva ou janela fixa aplicável; a grade publica o resultado e o cartório o copia para a ata. (`app/ponto/turnos.py:705–769`; `app/escala/utils.py:1994–2007`; `app/ponto/services/cartorio.py:601`) | O leitor lê a ata; `por_dia_da_grade` aplica **cap por dia**: `min(realizado, previsto)`, no universo trabalho/ausência. Esse mapa alimenta tanto DiaPago quanto o total mensal. (`app/escala/services/leitor_celula.py:365`; `app/ponto/services/dia_pago.py:385–399`; `app/ponto/services/fechamento.py:1028–1032, 1181`) |
| `horas_trabalhadas` | O motor escreve `periodo.minutos_trabalhados`; o resultado soma períodos/60. O fechamento acrescenta os minutos sem vínculo e a folga sem escala certa. DiaPago escreve dos mesmos períodos e complementos. (`app/ponto/motor_calculo_v2.py:2773, 295–296`; `app/ponto/services/fechamento.py:877–878, 948–953`; `app/ponto/services/dia_pago.py:178, 262–269`) | A lavra da **ata não fornece esse campo**: a origem monetária é o motor, enquanto os minutos da grade seguem a ata. Os dois chegam separadamente à lavratura. (`app/ponto/services/cartorio.py:599–601`; `app/ponto/services/fechamento.py:1169–1181`) |
| Intrajornada: `horas_intra_indenizada` | O motor calcula o suprimido e escreve `minutos_intrajornada_indenizada`; o resultado soma períodos normais e de folga/60. (`app/ponto/motor_calculo_v2.py:2755–2772, 363–370`) | O fechamento escreve a propriedade do resultado; DiaPago soma a intra dos períodos normais e dos ramos de folga. (`app/ponto/services/fechamento.py:972`; `app/ponto/services/dia_pago.py:186, 223–225, 234–263`) |

A lavratura de DiaPago materializa as linhas com `bulk_create`; o fechamento persiste por `save()` ou pelo ramo de `update()` dos campos calculados. (`app/ponto/services/dia_pago.py:343–358`; `app/ponto/services/fechamento.py:1105–1112, 1128–1153`)

**2. Mudanças identificáveis e respectivas condições**

Na coluna “dono”, **D** significa aplicar a decisão ordenada do item 4; a condição técnica descrita, sozinha, não fornece os argumentos necessários para escolher um dono. (`app/ponto/management/commands/e6_oraculo.py:53–54, 98–110`)

| Hash(es) do anexo | Condição em que o valor pode mudar com os mesmos dados | Campos e trecho atual | Dono |
|---|---|---|---|
| `212971ad` | Folga lançada em parte do mês: fora da foto, o ciclo continua respondendo pela folga semanal. Células já existentes prevalecem; não se pode pressupor regeneração. | Dias/minutos previstos: `app/escala/models.py:1071–1073, 1270–1292`; limite documentado em `app/docs/TICKETS.md:319`. | D |
| `a5cd3d39` | Foto parcial em 12x36/24x48: fora da foto, usa a paridade da folga mais próxima, em vez de considerar trabalho indiscriminadamente. | Dias/minutos previstos: `app/escala/models.py:1294–1335`. | D |
| `10832b45` / `a2e903e1`, `96beb803` | Dia com segunda pausa: DNA e sequência da grade passam a carregar todas as pausas, alterando os segmentos previstos e o casamento das batidas. | Previstos e geometria do realizado: `app/escala/models.py:1141–1181`; `app/escala/utils.py:1560–1590`. | D |
| `fecb9a67` | O juiz de previsto descontava somente a primeira pausa; passa a descontar todas. O comentário documenta 635→550. É um produtor distinto da soma de segmentos da grade. | Previsto consultado pelo motor e pela alternativa do leitor: `app/escala/utils.py:2224–2268`; `app/escala/services/leitor_celula.py:347–354`; associação documental em `app/docs/TICKETS.md:243`. | D |
| `801253ed` | Ata com várias pausas legíveis: o motor passa a receber todas, preservando os instantes reais das batidas. | Trabalhadas/intra: `app/ponto/motor_calculo_v2.py:694–721`. | D |
| `107e3bb5` | O papel das batidas muda na própria montagem: o realizado passa a usar a ata dessa montagem, em vez da anterior. Em intermitente, previsto também acompanha o realizado. | Realizados e previsto intermitente: `app/escala/utils.py:1802–1847, 1994–2019`. | D |
| `14ec97c2` / `825bb46a` | Vínculos concorrentes: a geradora da célula decide o vínculo do dia; geradora ausente ou fora da vigência devolve `None`. | Dias/previstos e cálculo dependente do vínculo: `app/escala/utils.py:1250–1263`; `app/escala/alimentacao.py:237–254`. | **CADASTRO quando sem vínculo**, ramo `e6_oraculo.py:98–99`; demais casos D. |
| `8ec526f2` | Folga de calendário pertence a outro vínculo: deixa de valer naquele dia; folga manual continua valendo para a pessoa. A alteração de `tipo_dia` repercute no universo contado/somado. | Dias/previstos/realizados: `app/ponto/precedencia.py:222–239`; `app/escala/utils.py:1103–1128`; `app/ponto/services/fechamento.py:1014–1017`; `app/ponto/services/dia_pago.py:385–399`. | D |
| `879ff834` | Ausência com batidas: passa a incluir realizado coberto, limitado ao previsto; o abono cobre somente o saldo. | Realizados: `app/ponto/services/dia_pago.py:373–399`. | D |
| `7b8329a1` | Pausa declarada pelas duas luzes da ata: a declaração tem prioridade sobre o casamento guloso, inclusive acima do teto da pausa deduzida. | Geometria que alimenta trabalhadas/realizados/intra: `app/ponto/turnos.py:802–814, 1777–1784, 1805–1824`. | D |
| `912ab728` | Uma perna da pausa acendeu e a outra sobrou: o ramo passa a aceitar essa forma dentro do teto. | Geometria dos mesmos campos: `app/ponto/turnos.py:1755–1765`. | D |
| `3d037f1b` | Duas células reivindicam a mesma batida: a reivindicação posterior é recusada. | Geometria dos mesmos campos: `app/ponto/turnos.py:1786–1801`. | D |
| `ce88575c` | Batida de outro dia próxima ao horário do intervalo: o envelope impede absorvê-la como pausa ou saída do turno anterior. | Geometria dos mesmos campos: `app/ponto/turnos.py:1326–1350`. | D |
| `c7b8bfdb` | Todas as pausas marcadas estão abaixo do piso: a guarda passa a consultar os intervalos efetivos, permitindo descontar a janela fixa quando aplicável. | Realizados: `app/ponto/turnos.py:742–762`. | D |
| `185b9af0` / `5d9ec893` / `a5ae5013` | Forma posicional com saída em outro turno e um único vão interior elegível: mede primeira entrada→última saída, menos a pausa. | Realizados: `app/ponto/turnos.py:671–730`. | D |
| `ed68dfd7` | Entradas recebidas juntas por flush, mas pertencentes a turnos distintos: o teto de 14h impede apagar a entrada seguinte como eco. | Geometria dos mesmos campos: `app/ponto/turnos.py:342–370`. | D |
| `dbd21fae` | Saída além do teto da jornada: passa a usar o teto do dia; a forma L-084 nas duas pontas conserva o envelope de 26h. | Geometria dos mesmos campos: `app/ponto/turnos.py:1419–1443`. | D |
| `0728ce98` | Cauda após meia-noite: substituição de períodos passa a remover pelo dia da jornada, preservando a cauda da véspera. | Trabalhadas/intra: `app/ponto/motor_calculo_v2.py:679–688`. | D |
| `de5b1a61`, `596064b7`, `cfd4ff83`, `8c851906` | Empresa/dia sujeitos à janela HE, pontas fora dos limites e extra declarada: o recorte muda os instantes efetivos; folga trabalhada sai desse recorte. A L-084 é avaliada antes dele. | Trabalhadas/intra: `app/ponto/motor_calculo_v2.py:1478–1544, 1664–1725, 2744–2773`. | D; se o dia estiver em CxR, aplica-se o ramo `e6_oraculo.py:110`, depois das prioridades. |
| `aad64415` | Pausa total ou parcialmente fora da janela efetiva: somente a interseção é descontada. | Trabalhadas/intra: `app/ponto/motor_calculo_v2.py:1869–1877, 2750–2763`. | D |
| `871ccd5b` / `2190b3da` / `156aa1a6` | Folga/feriado com previsto zero, sujeito à janela e sem autorização; para folga, exige escala certa. O ramo zera trabalho e intra. | Trabalhadas/intra: `app/ponto/motor_calculo_v2.py:1428–1464`. | D |
| `39668e22` | O bloqueio anterior também publica o par efetivo colapsado, para o calculador não pagar o segmento bloqueado. | Trabalhadas/intra do calculador: `app/ponto/motor_calculo_v2.py:1465–1471`; `app/ponto/calculador/alimentacao.py:114–123`. | D |
| `c6c3785b` | Folga trabalhada sem escala certa: a hora entra em trabalhadas, sem o adicional de 100%. | Trabalhadas mensal e diária: `app/ponto/services/fechamento.py:935–953`; `app/ponto/services/dia_pago.py:257–265`. | D |
| `9592fc83` / `a6b653ad` | Folga trabalhada deixa de perder a intra na lavratura. | Intra diária: `app/ponto/services/dia_pago.py:213–225, 234–263`. | D |
| `438ad986` | Feriado já separado da folga certa não pode voltar a entrar em “sem escala”; elimina a duplicação de trabalhadas e de intra diária. Também passa a perguntar escala certa na competência do dia. | Trabalhadas; intra de DiaPago: `app/ponto/services/fechamento.py:903–910, 943–953`; `app/ponto/services/dia_pago.py:244–263`. | D |

Há ainda mudanças de **materialização**, sem uma nova fórmula geométrica: `67024c7e` passa a gravar os mapas de previstos/realizados em DiaPago; `38f18bbe` cria o retrato que substitui esses minutos a partir da grade, inclusive zerando dias retirados dela. Atualmente, a guarda interrompe esse retrato para a 09 exportada. (`app/ponto/services/dia_pago.py:88–91, 286–290, 449–462, 479–494`)

Alguns commits do anexo precisam ficar fora da atribuição direta aos cinco deltas: [nao verificado]

| Classe | Limite demonstrável |
|---|---|
| O116 / `070c1345` | Muda a distribuição diária pelo mapa `dia_da_jornada`; o comentário registra total mensal invariável no universo medido. (`app/ponto/services/dia_pago.py:109–132, 174, 235–258`) |
| S5b: `ec22b2f1`, `a80bf914`, `abea49b1`, `5b4e5bb8` / `e8e67d9b` | A substituição por rubricas existe, mas a chamada grava versão `oraculo` somente a partir de 10/2026; não demonstra causa da 09 em versão `motor`. (`app/ponto/services/dia_pago.py:310–328`; `app/ponto/services/fechamento.py:1233–1262`) |
| O280 exclusivo de `calculador/periodo.py`, incluindo `f6b16a7e` / `ad299e5c` | O módulo declara ausência de leitores de produção; o motor presente ainda conserva o ramo `duracao > 360`. (`app/ponto/calculador/periodo.py:8–20`; `app/ponto/motor_calculo_v2.py:2755–2773`) |
| `917b8afa` | Altera o pago exibido da folga-feriado pelo detalhe; isso não equivale a escrever `horas_trabalhadas`. (`app/ponto/services/dia_pago.py:252–256, 892–896`) |
| `32ab503c`, `bcf58258`, `861100a1`, `e15125d2` | Nos trechos correspondentes, escrevem furo ou carimbos, não novas fórmulas dos cinco campos. (`app/ponto/services/dia_pago.py:668–671, 523–541, 340–345, 333–334`) |

A causa nominal de **col211, col638, col749, col900 e col926** continua **[nao verificado]**. O registro contém os totais, mas não a decomposição diária necessária para ligar cada diferença a uma das condições acima. (`app/docs/BACKLOG.md:760`)

**3. Ramos de rejulgamento**

| Situação | Decisão |
|---|---|
| Dia com adesão/batidas, impressão igual, julgamento e veredito presentes, diferente de `nunca_bateu` | `_em_dia=True`; sem `forcar`, incrementa `pulados` e não relavra. (`app/ponto/services/cartorio.py:1190–1196`) |
| Mesmo ramo, mas impressão diferente, carimbo/veredito ausente ou veredito `nunca_bateu` | Não satisfaz o pulo; busca o dia da grade e prossegue no julgamento. (`app/ponto/services/cartorio.py:1187–1199`) |
| Ramo sem adesão, `nunca_bateu` | Tem teste próprio: julgamento + impressão igual + veredito; sem `forcar`, também pula. (`app/ponto/services/cartorio.py:1030–1038`) |
| `forcar=True` | Ignora o pulo por impressão, inclusive sem subida de versão. (`app/ponto/services/cartorio.py:774–779, 1194`) |

**Sem subida**, alterações em qualquer parte efetivamente codificada no hash podem provocar rejulgamento: batidas, cobertura, chamados, DNA, veto, teto, fio, cobertura parcial e encerramento do turno. Os minutos calculados e `tipo_dia` da grade não aparecem nesse hash. (`app/ponto/services/cartorio.py:365–421`)

**Só pela subida**, o mecanismo é: `versao_para` escolhe a maior versão cujo horizonte alcance a competência; a versão entra como `regra:N` no hash; o cartório passa esse valor ao teste de impressão. Isso alcança atas que continuariam com hash igual pelos demais insumos. (`app/ponto/services/cartorio.py:223–229, 384–385, 977–980, 1181–1186`)

Na cópia, a declaração é `((1,'2026-09'), (2,'2026-10'), (3,'2026-10'))`: a 09 recebe versão 1. Alterar o horizonte da versão 3 para `'2026-09'` faz a 09 receber 3; **o primeiro número é versão, não empresa**. (`app/ponto/services/cartorio.py:111–113, 223–229`)

Rejulgar a ata não implica escrever o fechamento exportado: a guarda atual da 09 exclui essas empresas da escrita de FechamentoMensal/DiaPago, enquanto a leitura pura permanece permitida. (`app/ponto/services/fechamento.py:27–43, 523–524, 542–559`)

**4. Dono de cada condição**

Para todas as linhas marcadas **D**, esta é a decisão exata, na ordem em que o código executa:

| Primeira condição satisfeita | Dono e ramo |
|---|---|
| `folga_nao_definida` ou `sem_vinculo_no_dia` ou `sem_vinculo_em_dia_de_trabalho` | **CADASTRO** — `app/ponto/management/commands/e6_oraculo.py:98–99` |
| `batida_fora_de_turno` | **BATIDA** — `app/ponto/management/commands/e6_oraculo.py:104–105` |
| Quantidade ímpar de `horas_do_dia` | **BATIDA** — `app/ponto/management/commands/e6_oraculo.py:106–107` |
| `fase_deslocada` | **CADASTRO** — `app/ponto/management/commands/e6_oraculo.py:108–109` |
| `em_cadastro_x_realidade` | **CADASTRO** — `app/ponto/management/commands/e6_oraculo.py:110` |
| Nenhuma das anteriores | **ESTRUTURA** — `app/ponto/management/commands/e6_oraculo.py:110` |

Portanto, **“mudou por correção de código” não basta para classificar como ESTRUTURA**: o nome do commit e o motivo técnico não são argumentos da função; os insumos são carregados por dia. (`app/ponto/management/commands/e6_oraculo.py:53–54, 168–178`)

O E6 compara minutos realizados — ou, na falta deles, `pago_h×60` —, não classifica diretamente um delta mensal de dias previstos ou de intra. Aplicar seu juiz aos cinco colaboradores exige os dias e os respectivos insumos; os donos nominais permanecem **[nao verificado]**. (`app/ponto/management/commands/e6_oraculo.py:246–252, 361–364`; `app/docs/BACKLOG.md:760`)

## 01/11 14:38 · sessao 9c1230f1a8a5 · - · - · origem=Ronald (tmux)

Consulta proposta, sem executar, para cada par `(cid, d)` = `(222, 2026-10-28)`, `(150, 2026-09-11)`, `(304, 2026-08-31)`, `(476, 2026-08-21)`; valores de produção e snapshots anteriores a A/B continuam `[nao verificado]`.

```python
from datetime import date, datetime, time, timedelta
from django.db.models import Q
from django.utils import timezone
from colaboradores.models import Colaborador, Empresa
from escala.models import CelulaDia, EscalaColaborador
from ponto.models import Ausencia, Batida, Justificativa, TipoAusencia
from chamados.models import PerguntaDisputa, DisputaSupervisao, ChamadoColaborador

casos = [(222, date(2026,10,28)), (150, date(2026,9,11)),
         (304, date(2026,8,31)), (476, date(2026,8,21))]

Empresa.objects.filter(pk=3).values("id", "dia_inicio_competencia")
TipoAusencia.objects.filter(ativo=True).values(
    "codigo", "parcial", "fim", "bloqueia_ponto", "efeito")

# Para cada cid,d:
Colaborador.objects.filter(pk=cid, empresa_id=3).values(
    "id", "empresa_id", "situacao", "isento_ponto")

CelulaDia.objects.filter(colaborador_id=cid, data__range=(d-timedelta(days=1), d)).values(
    "id", "data", "trabalha", "escala_geradora_id", "dna", "dna_anterior",
    "julgada_em", "impressao", "veredito", "protestos", "ata",
    "editada_em", "regenerada_em")

Ausencia.objects.filter(colaborador_id=cid).exclude(status="rejeitada").filter(
    Q(data=d) | Q(data__lte=d, data_fim__gte=d)
    | Q(data__lte=d, data_fim__isnull=True, tipo__in=Ausencia.TIPOS_EM_ABERTO)
).order_by("-data", "-pk").values(
    "id", "data", "data_fim", "tipo", "status", "minutos", "hora_ini", "hora_fim",
    "criado_em", "atualizado_em", "aprovada_em")

t0 = timezone.make_aware(datetime.combine(d-timedelta(days=1), time.min))
t1 = timezone.make_aware(datetime.combine(d+timedelta(days=2), time.min))
Batida.objects.filter(colaborador_id=cid, timestamp__gte=t0, timestamp__lt=t1).values(
    "id", "timestamp", "tipo", "is_intervalo", "origem", "aprovada",
    "retratada_em", "criado_em", "pergunta_origem_id")

Justificativa.objects.filter(colaborador_id=cid, data_turno=d).values(
    "id", "status", "criado_em", "atualizado_em", "aprovada_em")

PerguntaDisputa.objects.filter(disputa__colaborador_id=cid, data_evento=d).values(
    "id", "motivo", "celula_id", "disputa_id", "chamado_destino_id",
    "respondida_em", "validada_em", "materializada_em", "via_resolucao",
    "disputa__chamado_id", "disputa__fechada_em", "disputa__motivos",
    "disputa__contexto")

# Sem filtrar data/status: disputa é agregada por colaborador.
DisputaSupervisao.objects.filter(colaborador_id=cid).values(
    "id", "chamado_id", "motivos", "contexto", "aberta_em", "fechada_em")
ChamadoColaborador.objects.filter(colaborador_id=cid).values(
    "id", "celula_id", "categoria", "subcategoria", "modulo_origem",
    "status_local", "contexto_json", "criado_em", "atualizado_em")
```

Referências para o recorte e os campos:

- **Empresa/cadastro:** corte em `app/colaboradores/models.py:46`; situação/isento/empresa em `app/colaboradores/models.py:294` e `:321`. Catálogo lê apenas `TipoAusencia.ativo=True` em `app/ponto/catalogo/ausencias.py:249`; flag parcial integra o catálogo em `:251`, sendo consumida em `:357`.
- **Ausência vencedora:** filtro e ordenação exatos em `app/ponto/turnos.py:2552`; desempate `-data,-pk` em `:2557`. A grade toma a primeira **aprovada**, enquanto a cobrança toma a primeira **não rejeitada**, pelo filtro de efeito em `:2514` e `:2516`; grade chama `para='folha'` em `app/escala/utils.py:1187`.
- **Parcial/período:** tipo vigente + um dia + `0 < minutos < 480` em `app/ponto/turnos.py:2595`; período ausente em `:2632`, virada noturna em `:2636`, inclusão das bordas em `:2639`.
- **Ata/célula:** `trabalha`, geradora e DNA em `app/escala/models.py:1653`, `:1660`, `:1665`; `regenerada_em`, `dna_anterior`, `editada_em` em `:1686`, `:1694`, `:1700`; impressão/julgamento em `:1713`; veredito/protestos/ata em `:1738`, `:1741`, `:1746`. A véspera é relevante ao leitor de faltantes em `app/chamados/services/declaracao_texto.py:81`, que a limita a fim de turno em `:119`.
- **Batida/fato novo:** os campos estão em `app/ponto/models.py:22`, `:26`, `:40`, `:63`, `:67`, `:79`. O recorte `[d−1, d+2)` e as marcas de criação/retração são exatamente os de `app/ponto/services/cartorio.py:292`; ausência, justificativa e edição/regeneração são verificadas em `:297`, `:301`, `:307`. Não eliminar retratadas desta consulta: o próprio ato de retração pode ser fato novo, conforme `:295`.
- **Pergunta/disputa/chamado:** campos de pergunta em `app/chamados/models.py:2194`, `:2201`, `:2203`, `:2213`, `:2300`, `:2651`, `:2659`; disputa em `:1651`, `:1656`, `:1660`, `:1664`, `:1671`; chamado em `:32`, `:38`, `:41`, `:44`, `:46`, `:61`.

Para a cadeia do turno, acrescentar a geradora apontada pela célula e os demais vínculos candidatos, sem substituir a geradora por “escala ativa”: a autoridade da geradora está em `app/escala/alimentacao.py:196`; a inclusão explícita de geradoras fora da janela está em `:123`.

```python
# gid = escala_geradora_id extraído acima.
EscalaColaborador.objects.filter(colaborador_id=cid).filter(
    Q(pk=gid)
    | (Q(data_inicio__lte=d)
       & (Q(data_fim__isnull=True) | Q(data_fim__gte=d))
       & (Q(ativa=True) | Q(data_fim__isnull=False)))
).values()  # Preservar configuração integral do vínculo.
```

Campos de vigência e template: `app/escala/models.py:874`; filtro de candidatura em `app/escala/alimentacao.py:187`. Para template, consultar `TipoEscala` dos `tipo_escala_id` retornados, incluindo sua cadeia de `template_pai`: a herança é lida em `app/escala/models.py:806`.

A impressão é hash dos insumos, sem guardar a lista reversível deles; o carimbo pode ser sobrescrito ao rejulgar (`app/escala/models.py:1706`). Portanto, afirmar quais dias o **controle B efetivamente pulou** requer os mesmos campos em snapshot anterior a B ou instrumentação por célula; o estado atual sozinho não demonstra esse histórico `[nao verificado]`.

_1425 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
