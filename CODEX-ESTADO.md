# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 11:33** · publicado em **31/10/2026 11:34** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 11:33 · sessao f7baeb4ed248 · CODEX-LICOES-PROBE0 · leitura · origem=Code

1) ZEBRA-07E712A0 [nao verificado]

2) `AGENTS.md`, do diretório `/home/ronald/codex_ws/19d2a06fc144d68abf7cd05b67a4d150c49eb776`, fornecido no contexto pelo usuário. [nao verificado]

## 31/10 11:33 · sessao 4db2ad64e94e · CODEX-LICOES · verificador · origem=Code

**A cópia e o anexo fornecido representam estados diferentes.** Na cópia, o wrapper termina sem os blocos de AGENTS, base/diff e julgamento do brief presentes no anexo; o BACKLOG ainda registra o contexto aguardando pouso. As referências abaixo são da cópia. A origem/versionamento do wrapper anexado é [não verificado]. (`bin/codex_pergunta.sh:118`, `bin/codex_pergunta.sh:177`, `app/docs/BACKLOG.md:871`)

1. **Os dois papéis novos hoje nem chegam ao Codex.** `licao` e `licao-consolidar` caem na recusa com rc 2. Além de ampliar o wrapper, é necessário atualizar os leitores: chamadas e tokens desses papéis seriam agrupados em `outro`. (`bin/codex_pergunta.sh:48`, `bin/codex_limite_estado.py:23`, `bin/codex_limite_estado.py:78`, `bin/codex_limite_estado.py:194`)

2. **A pergunta fixa conflita com o cabeçalho existente.** O wrapper exige `arquivo:linha ou [nao verificado]`; a lição proíbe caminho e `:NNN`. Uma lição sustentada por citação seria recusada; usar `[nao verificado]` satisfaria o cabeçalho sem comprovar a lição. É necessária uma regra específica por papel, preservando a exigência nos demais — o selo atual cobra esse cabeçalho. (`bin/codex_pergunta.sh:124`, `bin/tests/test_codex_pergunta.sh:27`)

3. **O material do gatilho está subespecificado.** A classificação vigente recebe **todas** as perguntas/respostas, as linhas de triagem e devolve trechos por achado. O contrato passa “a pergunta .q + a resposta .r”, sem escolher a volta nem exigir a tabela classificada como anexo. Em obra com várias voltas, isso pode ensinar uma conclusão sem sua história ou evidência. Inferência baseada no fluxo existente. (`CLAUDE.md:598`, `CLAUDE.md:614`, `CLAUDE.md:616`, `CLAUDE.md:617`)

4. **“Arquivo na árvore viva” não significa “próxima chamada recebe”.** O wrapper lê um `git archive` do hash e reutiliza a cópia existente; não acompanha alterações posteriores no vivo. Para cumprir o contrato antes do próximo commit, precisa anexar explicitamente uma fotografia do arquivo em `$VIVO`, identificando sua versão. Usar `$WS` entrega conteúdo antigo; usar `$R` depende de onde o wrapper foi executado. (`bin/codex_pergunta.sh:29`, `bin/codex_pergunta.sh:30`, `bin/codex_pergunta.sh:87`, `bin/codex_pergunta.sh:91`; `app/docs/LEIS.md:132`)

5. **Anexar em qualquer papel acrescenta uma dependência de raspagem a todas as chamadas.** Hoje qualquer `--anexo` exige lista de nomes válida e pode encerrar com rc 4 antes do Codex. Os anexos também compartilham um corte silencioso de 300.000 bytes: dependendo da ordem, lições ou evidências podem desaparecer. O contrato precisa definir indisponibilidade, ordem e tratamento do excesso. (`bin/codex_pergunta.sh:104`, `bin/codex_pergunta.sh:105`, `bin/codex_pergunta.sh:111`, `bin/codex_pergunta.sh:115`; `bin/tests/test_codex_sonda.sh:76`)

6. **Rodar em fundo não garante ficar fora do caminho crítico.** Lições disputariam os mesmos seis slots com verificadores e sondas; o slot permanece ocupado até a saída. O timeout só começa depois da espera pelo slot. Portanto, uma lição pode atrasar a próxima chamada necessária à obra, mesmo sem alguém esperar diretamente por ela. (`bin/codex_pergunta.sh:47`, `bin/codex_pergunta.sh:138`, `bin/codex_pergunta.sh:150`, `bin/codex_pergunta.sh:153`, `bin/codex_pergunta.sh:157`)

7. **A consolidação contradiz o teto absoluto.** Contraexemplo: existem 40 linhas, entra a 41ª e a consolidação retorna 75 ou resposta inválida; “mantém” deixa 41 linhas, contrariando o selo. O retorno 75 já é um caminho explícito do wrapper; preservar o teto nesse cenário não está definido. **[não verificado quanto à implementação futura]** (`bin/codex_pergunta.sh:167`, `bin/codex_pergunta.sh:171`)

8. **O lock do append não resolve a substituição concorrente.** Se a consolidação ler uma versão, outra chamada acrescentar uma lição e depois ocorrer a substituição, o acréscimo pode ser perdido. Falta definir lock estável compartilhado entre append, leitura e publicação da consolidação, ou comparação de versão antes da troca. Os `flock` atuais controlam capacidade de execução, não o documento. **[não verificado quanto à implementação futura]** (`bin/codex_pergunta.sh:138`, `bin/codex_pergunta.sh:144`)

9. **A validação e os erros estão incompletos.** A consolidação exige apenas até 40 linhas com CLASSE: não repete a proibição de caminhos nem estabelece mínimo de uma linha. Também falta dizer que só se publica após rc 0, saída válida e `thread_id` presente; hoje o CLI recebe diretamente o destino da resposta, pode terminar com erro/timeout, e o SID ausente é apenas informado. (`bin/codex_pergunta.sh:157`, `bin/codex_pergunta.sh:159`, `bin/codex_pergunta.sh:163`, `bin/codex_pergunta.sh:175`, `bin/codex_pergunta.sh:176`)

10. **O selo proposto não prova “escrita fora do wrapper”.** Documento e log são dois arquivos: falha entre as gravações pode produzir linha legítima sem par. A consolidação também cria novos textos e precisa registrar novos pares. Falta protocolo de recuperação e identidade do achado para impedir duplicação ao republicar a tabela; a identidade atual das chamadas usa obra/hash/pid, sem identificar o achado. Ausência de par comprovaria inconsistência, não sua causa. **[não verificado quanto à implementação futura]** (`bin/codex_pergunta.sh:53`, `bin/codex_pergunta.sh:59`, `bin/codex_limite_estado.py:63`)

11. **“Qualquer papel” reintroduz opinião no verificador.** Lições derivadas dos achados do opus voltariam como contexto das verificações seguintes, inclusive da mesma obra. Isso exige uma exceção explícita à independência atual: pergunta sem hipótese e proibição de achado do opus no brief do verificador. O risco de eco é uma inferência; há registro documental anterior desse sinal, sem comprovação aqui dos resultados brutos. (`CLAUDE.md:579`, `CLAUDE.md:585`, `app/docs/PROMPTS.md:1684`, `app/docs/RELATO.md:176`)

12. **A/B, desligamento e certificação ainda não têm encaixe definido.** O pouso e o protocolo de `CODEX-CONTEXTO-SEM-CONDUTA` são [não verificado]. É preciso congelar as lições entre os braços, garantir sua ausência no braço “sem”, registrar a versão enviada e definir a chave de desligamento. O precedente documentado exige mesma pergunta/hash e controle de posição; os testes atuais cobrem estrutura, raspagem e limite, sem certificar essas novas invariantes. Uma segunda chamada inline para consolidar também quebraria o selo que exige uma única ocorrência de `codex exec`. (`app/docs/RELATO.md:179`, `app/docs/PROMPTS.md:1686`, `bin/tests/test_codex_pergunta.sh:13`, `bin/tests/test_codex_sonda.sh:57`, `bin/tests/test_codex_sonda.sh:87`, `bin/tests/test_codex_limite.sh:31`)

## 31/10 11:33 · sessao 3f91e974b7bb · V3-RETOMA-FILA1 · verificador · origem=Code

O desenho acerta ao substituir a soma manual pela lavra, mas **não garante ausência de escrita nem comparabilidade completa**: há auditoria fora de `lavrar`, filtros diferentes de colaboradores/dias e critérios diferentes de data. (`app/ponto/services/fechamento.py:523–558`; `app/ponto/management/commands/diff_calculador.py:102–108`; `app/ponto/services/dia_pago.py:173–290`)

**(a) Quem escreve e quem lê**

O encadeamento efetivo é: motor produz resultados das fatias → fechamento compõe o `ResultadoMes` e prepara mapas/partições → `lavrar` distribui os valores e constrói `DiaPago` → o captador proposto substituiria a persistência por linhas em memória. (`app/ponto/services/fechamento.py:270–300`; `app/ponto/services/fechamento.py:1169–1205`; `app/ponto/services/dia_pago.py:343–358`)

| Valor/argumento | Quem produz ou escreve | Quem lê e o que escreve na lavra |
|---|---|---|
| `colab`, `mes`, `ano` | Laço do fechamento e argumentos do recalculo. (`app/ponto/services/fechamento.py:432–435`, `:598`) | Identificação das linhas e filtro da substituição. (`app/ponto/services/dia_pago.py:343`, `:355–357`) |
| `ResultadoMes` composto por colaborador | `apurar_por_fatia` cria o composto **antes** de calcular as fatias; concatena listas, soma acumuladores e mescla mapas. O recalculo recebe `_ap.resultado`. (`app/ponto/services/fechamento.py:182`, `:270–300`, `:729–731`) | O DIFF atual captura a primeira instância por `colaborador_id` com `setdefault`; depois troca apenas `aut.resultado`. (`app/ponto/management/commands/diff_calculador.py:131–135`, `:154–162`) |
| `periodos` | Motor de cada fatia; fechamento concatena mantendo os objetos. (`app/ponto/services/fechamento.py:270–281`) | Trabalhadas, noturnas, HE50, HE50 noturna, HE100, HE100 feriado, HE100 noturna, saída antecipada e intra. (`app/ponto/services/dia_pago.py:173–186`) |
| `ft_certa`, `ft_feriado`, `ft_sem_escala` | Fechamento parte `resultado.periodos_ft`: escala certa; retira feriados; calcula complemento **por identidade**. (`app/ponto/services/fechamento.py:908–928`, `:948–949`) | Respectivamente: folga; HE100 **e** HE100 feriado; trabalhadas. Todos carregam noturno e intra. (`app/ponto/services/dia_pago.py:223–265`) |
| `orfaos_por_dia` | Outra execução de motor, `_r_orfa`; fechamento agrupa seus períodos por data local. **Não incorpora esses períodos ao composto.** (`app/ponto/services/fechamento.py:802–818`) | Acrescenta trabalhadas e detalhe `o85_dia_sem_vinculo_min`. (`app/ponto/services/dia_pago.py:267–270`) |
| `faltas_por_dia` | `minutos_de_falta_por_dia`. (`app/ponto/services/fechamento.py:848–850`) | `horas_falta` e detalhe `falta_min`. (`app/ponto/services/dia_pago.py:272–275`) |
| `atraso_por_dia`, `credito_por_dia` | `credito_parcial.atraso_por_dia(resultado.periodos)` e `creditos_por_dia`. (`app/ponto/services/fechamento.py:962–968`) | Atraso líquido: `max(atraso − min(atraso, crédito), 0)/60`; detalhe do crédito utilizado. (`app/ponto/services/dia_pago.py:190–195`) |
| `abono_por_dia`, `previsto_por_dia`, `realizado_por_dia` | Grade do fechamento → `por_dia_da_grade`; abono cobre o previsto não realizado na ausência, realizado tem teto diário. (`app/ponto/services/fechamento.py:981–982`, `:1028–1032`; `app/ponto/services/dia_pago.py:384–400`) | Os três `minutos_*`, **em minutos**, podendo criar dias. (`app/ponto/services/dia_pago.py:286–290`) |
| `reflexo_dsr`, `banco` | Campos montados no fechamento; banco é zerado quando `sem_movimento`. (`app/ponto/services/fechamento.py:1069–1072`, `:1175–1176`) | Somente `horas_reflexo_dsr` e `saldo_banco_horas` do ajuste. (`app/ponto/services/dia_pago.py:348–356`) |
| `dia_da_jornada` | Motor escreve/mescla `id(periodo) → data`, incluindo FT; fechamento mescla mapas das fatias. (`app/ponto/motor_calculo_v2.py:1011–1013`, `:2878–2897`; `app/ponto/services/fechamento.py:295–300`) | Lavra consulta por `id(p)`; alimentação v3 também. Sem chave, lavra usa data local; alimentação v3 conta `sem_dia_da_jornada` e descarta o período. (`app/ponto/services/dia_pago.py:126–132`; `app/ponto/calculador/alimentacao.py:63–72`) |
| `furos_do_motor` | `resultado.datas_falta` passa por `_furos_encerrados`. (`app/ponto/services/fechamento.py:1083–1085`, `:1194`) | Cria dias mesmo sem horas e escreve `furo_apurado`: `None` significa não perguntado; lista vazia significa nenhum furo. (`app/ponto/services/dia_pago.py:279–281`, `:343–345`) |
| `versao`, `versao_motor`, `regua_por_dia`, `evento_origem` | Chamada motor fornece `'motor'`, impressão da régua, mapa das fatias e `'backfill'` no modo proposto. (`app/ponto/services/fechamento.py:268–269`, `:739–740`, `:1195–1205`) | Metadados comuns; régua diária substitui `versao_motor`, truncado em 32 caracteres; ausência de régua é contada no ajuste. (`app/ponto/services/dia_pago.py:333–356`) |
| `versao_dna` | As duas chamadas omitem; default `''`. (`app/ponto/services/dia_pago.py:139`; `app/ponto/services/fechamento.py:1169–1205`, `:1245–1264`) | Metadado comum. (`app/ponto/services/dia_pago.py:333–334`) |
| `rubricas`, `do_motor` | Chamada motor omite ambos; oráculo fornece `_rub_o`, produzido por `rubricas_por_dia`. (`app/ponto/services/fechamento.py:1169–1205`, `:1235–1256`) | Validação de dono de cada campo; substituição dos valores fornecidos, preservando campos declarados `do_motor`; detalhe `calculador_nao_decide`. (`app/ponto/services/dia_pago.py:310–331`) |

Nos objetos capturados, `lavrar` escreve `data`, `tipo`, identificação, rubricas, `detalhe` e proveniência; todas as rubricas começam em zero. O modelo declara essas rubricas como `FloatField`, além dos três campos de minutos. (`app/ponto/services/dia_pago.py:169–170`, `:333–356`; `app/ponto/models.py:325–351`)

O leitor do instrumento deve usar `data`, `tipo` e os campos de `alvo`: hoje `alvo` exclui falta, reflexo de DSR e banco; também não contém os três campos de grade nem os metadados. (`app/ponto/management/commands/diff_calculador.py:78`; `app/ponto/calculador/regras.py:400–435`)

Capturar kwargs não reproduz integralmente um objeto persistido: `lavrado_em` é automático, a PK não é fornecida pela lavra e o ajuste recebe `furo_apurado=None` pelo default do modelo. Esses campos não são necessários para a comparação das rubricas de `alvo`. (`app/ponto/models.py:281`, `:369–380`; `app/ponto/services/dia_pago.py:355–356`; `app/ponto/management/commands/diff_calculador.py:78`)

**(b) Escrita no banco e dependências do modelo**

**Dentro de `lavrar`, a substituição é viável.** O import de `DiaPago` ocorre dentro da função; as únicas operações de persistência são `filter(...).delete()` e `bulk_create(novas)`. Seus auxiliares usados nesse caminho trabalham em memória. (`app/ponto/services/dia_pago.py:50–76`, `:109–132`, `:163`, `:357–358`)

O substituto precisa também ser **construtor chamável**, aceitando os kwargs das linhas de dia e ajuste. Apenas constantes e manager não bastam. `lavrar` não exige `_meta`, `save`, PK, validação ou métodos próprios do modelo nesses caminhos. (`app/ponto/services/dia_pago.py:343–358`)

**A operação inteira, porém, pode escrever fora desse patch.** A combinação proposta ativa a busca das empresas exportadas; a guarda de setembro/2026 pode excluir colaboradores e chamar `evento_kw` antes de qualquer lavra. Esse caminho chega a `LogAuditoria.objects.create()`, sem passar por `DiaPago`. A ocorrência concreta depende de exportações e colaboradores que o dado fornecido não demonstra: **[não verificado]**. (`app/ponto/services/fechamento.py:36–43`, `:523–558`; `app/core/observ.py:133–148`)

Logo, o `finally` que restaura `DiaPago` resolve a duração do patch; **não prova “nenhuma escrita” do recalculo**. (`app/ponto/services/fechamento.py:551–557`; `app/ponto/services/dia_pago.py:163`, `:357–358`)

**(c) Dia, universo e rubricas comparáveis**

Há cinco desencontros materiais:

1. **Colaboradores:** o comando seleciona empresas ativas e IDs com `FechamentoMensal` existente; o recalculo usa `universo_da_competencia`, com seu predicado de situação/demissão, seguido da guarda. Capturar a empresa inteira não torna esses universos iguais. (`app/ponto/management/commands/diff_calculador.py:97–108`; `app/ponto/services/fechamento.py:423–429`, `:538–548`)

2. **Dias da lavra:** períodos, órfãos, faltas, furos e grade podem criar linhas; `lavrar` não recebe nem aplica `ini/fim`. A porta v3 aplica a janela e visita dias do núcleo mais os dias bloqueados publicados pelo motor. (`app/ponto/services/dia_pago.py:135–141`, `:173–290`; `app/ponto/calculador/alimentacao.py:454–462`)

3. **O DIFF ainda estreita a porta v3:** percorre somente `v.minutos`, embora `rubricas_por_dia` possa devolver também dias bloqueados sem segmento. Depois a união dos dias transforma lado ausente em `{}` e rubrica ausente em zero, sem cegueira registrada para dias nunca visitados. (`app/ponto/management/commands/diff_calculador.py:428–431`, `:469–472`, `:517–535`; `app/ponto/calculador/alimentacao.py:454–458`)

4. **O dia não é uniforme:** a lavra agrupa períodos pelo mapa da jornada, mas o fechamento classifica FT pela **data local da entrada**; atraso e órfãos também chegam agrupados pelo calendário. A v3 pergunta vínculo/escala certa pelo dia da jornada. Portanto, compartilhar o resultado não elimina esse desencontro de critério. Casos efetivamente atingidos: **[não verificado]**. (`app/ponto/services/fechamento.py:814–817`, `:909–924`; `app/ponto/services/credito_parcial.py:29–43`; `app/ponto/services/dia_pago.py:174`, `:190–195`; `app/ponto/calculador/alimentacao.py:460–511`)

5. **Compartilhar resultado não compartilha toda a alimentação:** o comando conserva `aut.esc` e `aut.feriados`; o fechamento usa seu vínculo de referência. O espelho pergunta pelo último dia da janela; o fechamento procura do fim para trás até encontrar vínculo. A porta resolve CCT/família antes do laço diário usando o template recebido. Divergência concreta: **[não verificado]**. (`app/ponto/management/commands/diff_calculador.py:151–163`, `:361`, `:417–423`; `app/ponto/services/espelho.py:1069–1070`; `app/ponto/services/fechamento.py:123–129`, `:826–827`; `app/ponto/calculador/alimentacao.py:377–395`)

Além de HE100 feriado e folga, a troca muda potencialmente estas rubricas do lado v2, comparada à soma manual atual:

| Rubrica | Mudança introduzida pela lavra |
|---|---|
| `horas_trabalhadas` | Acrescenta FT sem escala certa e órfãos. (`app/ponto/services/dia_pago.py:257–270`) |
| `horas_extras_100` | Acrescenta o trabalhado de FT feriado, além de preencher seu campo específico. (`app/ponto/services/dia_pago.py:244–251`) |
| `horas_noturnas` | Passa a carregar o noturno de todos os grupos FT. (`app/ponto/services/dia_pago.py:223–263`) |
| `horas_intra_indenizada` | Passa a carregar a intra de todos os grupos FT. (`app/ponto/services/dia_pago.py:223–263`) |
| `horas_atraso` | Usa mapa diário e abate crédito parcial, em vez da soma bruta dos períodos. (`app/ponto/services/dia_pago.py:190–195`; `app/ponto/management/commands/diff_calculador.py:261`) |
| `horas_extras_50_noturna`, `horas_extras_100_noturna` | Passam a ser preenchidas; a soma manual atual omite ambas. (`app/ponto/services/dia_pago.py:181`, `:184`; `app/ponto/management/commands/diff_calculador.py:253–268`) |

HE50 e saída antecipada dos períodos comuns mantêm a soma aritmética; ainda podem aparecer em uma comparação com universo diferente. (`app/ponto/services/dia_pago.py:180`, `:185`; `app/ponto/management/commands/diff_calculador.py:255`, `:262`, `:527–535`)

**(d) Outras falhas do desenho**

- **Ajuste não é dia:** a lavra sempre cria `tipo='ajuste', data=None`, mesmo sem dias, e seu retorno inclui essa linha. É necessário filtrá-la antes de construir o universo ordenado de datas; captura existente ou retorno positivo não prova dia comparável. (`app/ponto/services/dia_pago.py:346–359`; `app/ponto/management/commands/diff_calculador.py:527`)

- **O erro não chega necessariamente ao comando:** o fechamento absorve exceções da lavra motor, da preparação/lavra oráculo e do bloco externo por colaborador. `erro_lavra_do_fechamento` precisa ser registrado dentro do captador; captura parcial deve ser descartada. Ter `ResultadoMes` capturado não prova lavra concluída. (`app/ponto/services/fechamento.py:1206–1209`, `:1265–1276`; `app/ponto/management/commands/diff_calculador.py:131–135`)

- **Oráculo usa o mesmo nome já importado:** `_lavrar_dia_pago` é importado uma vez e reutilizado na segunda chamada. O wrapper precisa tratar `versao!='motor'` explicitamente; restaurar somente o atributo do módulo não altera esse alias local. A preparação do oráculo continua ocorrendo em outubro em diante, mesmo que sua chamada final vire no-op. (`app/ponto/services/fechamento.py:1168–1169`, `:1233–1264`)

- **É preciso preservar a função real e a identidade dos períodos:** chamar novamente `_dpm.lavrar` após substituí-la causaria recursão; a chamada real deve usar a referência salva. Recriar períodos invalidaria as chaves `id(p)` do mapa. (`app/ponto/management/commands/diff_calculador.py:121`, `:137–142`; `app/ponto/services/fechamento.py:278–300`; `app/ponto/services/dia_pago.py:129`)

- **Exigir apenas `--periodos-do-fechamento` é insuficiente:** sem `--pares-da-autoridade`, `_ins_porta=None`, a porta de rubricas não é chamada e `c_dia` fica vazio; a comparação pode tratar toda a lavra como divergência contra zero. (`app/ponto/management/commands/diff_calculador.py:325–337`, `:417–428`, `:527–535`)

- **A autoridade antiga ainda exclui colaboradores:** mesmo com lavra capturada, o comando chama `autoridade_do_periodo` antes de trocar o resultado; uma falha ali incrementa `erro_motor` e abandona o colaborador. (`app/ponto/management/commands/diff_calculador.py:145–162`)

- **O selo proposto cobre só parte do risco:** períodos sintéticos não exercitam guarda/auditoria, composição por fatias, órfãos, crédito parcial, dias sem segmentos e exceções absorvidas. Esses caminhos existem separadamente no código. (`app/ponto/services/fechamento.py:270–300`, `:545–557`, `:802–818`, `:962–969`, `:1206–1209`; `app/ponto/calculador/alimentacao.py:454–458`)

A indicação “Roda por ATO” está no cadastro do comando; já frota com `sem_lavra=0`, resultados dos re-DIFFs 09/10 e coluna `INSTRUMENTO` próxima de zero não foram demonstrados pelo anexo: **[não verificado]**. A saída atual contém motor, calculador, delta e contagens, sem coluna `INSTRUMENTO`. (`app/config/crons.py:966–970`; `app/ponto/management/commands/diff_calculador.py:550–556`)

## 31/10 11:33 · sessao 1835b36532b4 · - · - · origem=Ronald (tmux)

Achados materiais nas fontes:

1. **As classes declaradas não cobrem o CLAUDE atual.** Ficam sem classe as seções 1, 3, 5, 6b, 7 e 8. O selo de cobertura prometido começaria vermelho; classificar todas como excluídas também não é neutro: 6b contém regras de folha e seleção da CCT. Referências: `CLAUDE.md:90`, `CLAUDE.md:194`, `CLAUDE.md:325`, `CLAUDE.md:430`, `CLAUDE.md:460`, `CLAUDE.md:821`; conteúdo jurídico em `CLAUDE.md:433` e `CLAUDE.md:444`.

2. **“Excluir seções de conduta” não produz “sem conduta”.** As próprias seções incluídas misturam arquitetura com instruções ao executor: seção 2 manda publicar, resolver merge e fazer deploy imediatamente; 4b manda fechar itens, ordena R1/R2 e famílias F1. Referências: `CLAUDE.md:137`, `CLAUDE.md:168`, `CLAUDE.md:174`, `CLAUDE.md:303`, `CLAUDE.md:320`, `CLAUDE.md:323`. A seção 4 também contém aval para editar motor e forma obrigatória de sonda: `CLAUDE.md:242`, `CLAUDE.md:246`. [nao verificado]

3. **LEIS reintroduz a conduta excluída sem reproduzir seus cabeçalhos.** O próprio documento define `CONDUTA` como regra de processo, e suas linhas L-001, L-005, L-117 e L-123 reproduzem mandamentos de construção, fila, propostas e revisão. Um detector de título/hash do recorte original pode ficar verde diante dessas paráfrases. Referências: `app/docs/LEIS.md:22`, `app/docs/LEIS.md:59`, `app/docs/LEIS.md:63`, `app/docs/LEIS.md:138`, `app/docs/LEIS.md:144`.

4. **PLANO-PADROES inteiro viola a premissa de compartilhar somente FATO/LEI.** Há fila, estado de obra e dono; o arquivo identifica expressamente contexto, alternativas e consequências como redação do Code derivada do RELATO. Incluí-lo integralmente compartilha a interpretação do construtor que o desenho quer afastar do verificador. Referências: `app/docs/PLANO-PADROES.md:7`, `app/docs/PLANO-PADROES.md:26`, `app/docs/PLANO-PADROES.md:59`. Também inclui conduta de revisão/commit: `app/docs/PLANO-PADROES.md:53`, `app/docs/PLANO-PADROES.md:56`.

5. **Separar por seção pode remover lei junto da conduta.** Não se pode presumir que 7b só contém comportamento de agente: ali também estão regra do TXT como fotografia e a condição de apply em competência aberta. A correção é decidir recortes de conteúdo e sua autoridade; o título da seção sozinho não define a natureza de tudo que contém. Referências: `CLAUDE.md:683`, `CLAUDE.md:708`. Não afirme perda efetiva sem verificar se a mesma lei completa entra por LEIS.

6. **O vocabulário incluído mantém referências de conduta.** GLOSSARIO liga padrões a construir em cópia, comboio e proibição da LEI-AKITA. É outro caminho de entrada que a blacklist de títulos não mede. Referências: `app/docs/GLOSSARIO.md:18`, `app/docs/GLOSSARIO.md:20`, `app/docs/GLOSSARIO.md:23`.

Medição independente somente em memória: com CLAUDE 2/4/4a/4b da cópia, índice LEIS pelo algoritmo do anexo, MAPA a–h e GLOSSARIO/PLANO/JUIZES inteiros, o subtotal sem moldura/cabeçalho é **42.330 B**. Isso confirma o conflito com 30.000, mas o número não tem arquivo:linha próprio; no texto final, usar o ~42,7k fornecido com `[nao verificado]` ou identificar explicitamente como medição em memória.

## 31/10 11:32 · sessao 3bcf16e165d3 · - · - · origem=Ronald (tmux)

Pontos documentais para a consolidação:

- **A base atual é anterior ao anexo fornecido.** O wrapper da cópia não tem o cabeçalho de contexto nem perguntas fixas: monta somente regras, pergunta e anexos em `bin/codex_pergunta.sh:118-135`. O instrumento CODEX-CONTEXTO-DA-CASA consta como aguardando pouso em `app/docs/BACKLOG.md:871`; o texto anexado pelo usuário corresponde a outro estado [não verificado quanto à origem].
- **Dependência sem evidência nesta cópia.** CODEX-CONTEXTO-SEM-CONDUTA não apareceu na busca em CLAUDE.md/app/docs; seu pouso e protocolo A/B são [não verificado]. O instrumento documentado é CODEX-CONTEXTO-DA-CASA (`app/docs/BACKLOG.md:871`).
- **O gatilho precisa preservar a história inteira.** A classificação existente recebe TODOS os `.q`/`.r` e linhas de triagem e devolve os trechos por achado (`CLAUDE.md:614-618`). O contrato fala em “a pergunta .q + a resposta .r” sem definir qual volta; numa obra com várias voltas, anexar apenas um par pode esconder onde o tema apareceu [inferência: `CLAUDE.md:598-600`, `CLAUDE.md:616-618`].
- **Lições de opus para todo papel ameaçam a independência.** A pista exige pergunta sem hipótese (`CLAUDE.md:579`, `CLAUDE.md:585-589`) e o aval do contexto proíbe achado do opus/hipótese no verificador (`app/docs/PROMPTS.md:1684`). Reenviar a qualquer verificador as lições derivadas dos achados do opus reintroduz esse conteúdo por outra via [inferência: mesmas referências].
- **A/B necessita braços realmente comparáveis e gabarito separado.** O primeiro ensaio documentado variava só AGENTS.md, tinha mesma `.q`/hash e n=2; registrou viés de posição (`app/docs/RELATO.md:179`). O aval posterior exigiu chave invertida e gabarito do opus (`app/docs/PROMPTS.md:1686`). A re-medida registra ganho concentrado nos itens do próprio Codex e “sinal fraco de eco” (`app/docs/RELATO.md:176`); efeitos das novas lições continuam [não verificado].
- **L-106 autoriza levar o documento no próximo marco, sem dar publicação imediata ao commit.** A regra determina docs junto do código do marco (`CLAUDE.md:778-780`, `app/docs/LEIS.md:132`). Como o wrapper atual lê `git archive` e reutiliza a cópia do hash (`bin/codex_pergunta.sh:87`, `bin/codex_pergunta.sh:91-101`), lição recém-escrita no vivo só chega antes do commit se houver canal explícito de anexo do vivo; arquivo dentro da cópia não acompanha o vivo [inferência: mesmas referências].
- **Atualizar só o vocabulário do wrapper deixa a medição incompleta.** Os papéis conhecidos do ESTADO são quatro (`bin/codex_limite_estado.py:23`); papel novo vira `outro` tanto em chamadas (`bin/codex_limite_estado.py:74-78`) quanto em tokens (`bin/codex_limite_estado.py:191-194`).
- **O prompt fixo de “sem caminho” colide com a evidência exigida pelo wrapper.** Toda afirmação exige arquivo:linha ou `[nao verificado]` (`bin/codex_pergunta.sh:123-124`); uma lição verificada com citação de arquivo seria rejeitada pelo próprio contrato [inferência: referências e contrato do usuário].

## 31/10 11:32 · sessao 545919d7e475 · - · - · origem=Ronald (tmux)

Achados de integração e testes, apenas por leitura: [nao verificado]

- **A cópia e o anexo são versões diferentes.** O wrapper local termina em `bin/codex_pergunta.sh:177`; seu cabeçalho é montado diretamente em `bin/codex_pergunta.sh:118`, sem os blocos de AGENTS/base/juiz do brief presentes no dado fornecido. Não se pode atribuir esses blocos ao hash local. `app/docs/BACKLOG.md:871` ainda descreve o contexto como construção em outra cópia e pouso retido.

- **Os dois papéis novos são recusados antes da chamada:** a whitelist aceita apenas `leitura|censo|verificador|sonda` e sai com rc 2 nos demais casos. `bin/codex_pergunta.sh:48`. Há teste para papel inválido, sem caso de `licao` ou `licao-consolidar`: `bin/tests/test_codex_sonda.sh:99`.

- **Há conflito de formato com o cabeçalho atual:** toda afirmação deve carregar `arquivo:linha ou [nao verificado]`, enquanto a lição exige não conter caminhos. `bin/codex_pergunta.sh:124`. Remover essa regra globalmente quebra o selo estrutural que exige o texto: `bin/tests/test_codex_pergunta.sh:27`.

- **Somente ampliar a whitelist não integra as métricas.** O leitor mantém quatro papéis fixos e agrupa os demais em `outro`, inclusive nos tokens. `bin/codex_limite_estado.py:23`, `bin/codex_limite_estado.py:78`, `bin/codex_limite_estado.py:194`. O selo confirma esse agrupamento: `bin/tests/test_codex_papel_estado.sh:31`.

- **Consolidação pode colidir com o selo de uma única chamada.** O teste exige exatamente uma ocorrência executável de `codex … exec` no wrapper; adicionar uma segunda chamada inline para consolidar torna esse teste vermelho. `bin/tests/test_codex_sonda.sh:87`. Reutilizar o caminho comum evita essa colisão, mas a estratégia do contrato não está implementada. `bin/codex_pergunta.sh:157`.

- **Os testes existentes não certificam a transcrição proposta.** O selo estrutural cobre sandbox, diretório da cópia, teto, slot e cabeçalho; o comportamental cobre raspagem, falha fechada, procedência e papéis inválidos. `bin/tests/test_codex_pergunta.sh:13`, `bin/tests/test_codex_sonda.sh:57`, `bin/tests/test_codex_sonda.sh:90`. Validação `CLASSE`, caminhos, multiline, append concorrente, consolidação, integridade do par hash/thread e desligamento por A/B permanecem **[não verificado]**.

- **O tratamento de rc 75 existe, mas seu teste cobre apenas limite genérico.** O wrapper detecta os eventos e sai antes da captura de SID; o SID é extraído somente depois desse ramo. `bin/codex_pergunta.sh:163`, `bin/codex_pergunta.sh:175`. O selo verifica rc e número de linhas em `limite.log`, sem assegurar “nenhuma lição transcrita”. `bin/tests/test_codex_limite.sh:31`.

- **Background não elimina disputa por capacidade.** Todas as chamadas usam o mesmo gate e slots, mantidos até a saída; a geração de lição pode, portanto, ocupar slot que uma chamada posterior precisa. `bin/codex_pergunta.sh:138`, `bin/codex_pergunta.sh:153`. O timeout começa no `codex`, após a espera pelo slot. `bin/codex_pergunta.sh:157`.

## 31/10 11:32 · sessao 3488c57e5209 · - · - · origem=Ronald (tmux)

Achados materiais:

1. **Pasta de selos não garante execução no pre-push.** O loop `tests/test_*.sh` roda em `bin/regua.sh:138-145`. O próprio pre-push declara que esse loop não roda nele e chama alguns selos explicitamente (`bin/pre-push.sh:13-19`). A suite do arsenal é Django (`bin/pre-push.sh:155-184`). Portanto, só adicionar `test_agents_md_gerado.sh` à pasta deixa push direto sem essa guarda; a chamada deve verificar o commit enviado, cujo archive nasce em `bin/pre-push.sh:33-38`.

2. **O contexto consumido precisa estar commitado no hash, não apenas gerado no disco.** O wrapper extrai `git archive "$HASH"` e torna a cópia sem escrita (`bin/codex_pergunta.sh:91-100`); depois executa Codex nessa cópia (`bin/codex_pergunta.sh:157-158`). Gerar após o commit ou deixar `AGENTS.md` fora dele não atualiza o contexto dessa chamada.

3. **Cópia reutilizada não é conferida.** Se o diretório do hash já existe, o wrapper pula a criação inteira, sem comparar conteúdo com o archive (`bin/codex_pergunta.sh:91-101`). Logo, trocar o gerador não atualiza uma cópia existente do mesmo hash. A/B precisa de cópias isoladas e controle do conteúdo; o wrapper admite base alternativa por `CODEX_WS_BASE` (`bin/codex_pergunta.sh:87`).

4. **“Sem conduta” no AGENTS não impede conduta no prompt.** O wrapper concatena literalmente a pergunta (`bin/codex_pergunta.sh:118-135`) e aceita anexos, cuja transformação é raspagem de nomes/CPF, seguida de concatenação (`bin/codex_pergunta.sh:103-115`). Não há nessa montagem filtro de títulos/hash de conduta. Portanto, um selo só sobre AGENTS não prova ausência de conduta no contexto total recebido.

5. **Selos atuais não provam carregamento nem ausência de conduta no AGENTS.** O selo do wrapper verifica strings de forma, sandbox e `-C` (`bin/tests/test_codex_pergunta.sh:10-29`); sua mutação troca o sandbox (`bin/tests/test_codex_pergunta.sh:30-33`). O selo sonda usa Codex falso que só captura stdin (`bin/tests/test_codex_sonda.sh:21-25`), e a prova real existente mede ausência de escrita, não contexto carregado (`bin/tests/prova_codex_pergunta_real.sh:15-28`). É necessário distinguir “gerado conforme” de “conteúdo efetivamente recebido”.

Não apontaria rc 75 como conflito: ele é classificado depois da execução e devolvido sem regeneração de contexto (`bin/codex_pergunta.sh:163-171`). Tokens não entram em `chamadas.log`, que registra thread, rc e duração (`bin/codex_pergunta.sh:53-60`); a captura JSONL existe (`bin/codex_pergunta.sh:156-158`), mas a apuração do A/B prometido permanece [nao verificado].

## 31/10 11:32 · sessao 314427c48401 · - · - · origem=Ronald (tmux)

Achados de comparabilidade e falhas do contrato:

- **A captura muda mais rubricas que HE100 feriado e folga.** Passam a entrar os splits `horas_extras_50_noturna` e `horas_extras_100_noturna`; `horas_extras_100` recebe FT-feriado; `horas_trabalhadas` recebe FT sem escala e órfãos; noturnas e intra recebem todos os FT; atraso passa a usar crédito parcial. O laço atual não faz essas operações. `app/ponto/management/commands/diff_calculador.py:253-268`; `app/ponto/services/dia_pago.py:178-195`, `:223-270`.

- **Mesmo resultado não garante a mesma classificação do dia.** Fechamento classifica FT com `_certa` e `escala_vigente` na **data local da entrada**; lavra depois distribui pelo mapa `id(periodo) → jornada`. V3 decide escala certa, vínculo e feriado pelo **dia da jornada**. Há duas perguntas de data no caminho v2; incidência no dado de produção: [não verificado]. `app/ponto/services/fechamento.py:909-925`; `app/ponto/services/dia_pago.py:109-132`, `:234-258`; `app/ponto/calculador/alimentacao.py:460-511`.

- **Atraso também conserva calendário no v2.** `atraso_por_dia` usa a data local da entrada, e a lavra desconta o crédito nessa chave. V3 agrupa a pontualidade pelo mapa da jornada e não recebe o mapa de crédito parcial. `app/ponto/services/credito_parcial.py:15-44`; `app/ponto/services/dia_pago.py:190-195`; `app/ponto/calculador/alimentacao.py:227-228`, `:496-519`.

- **Os universos de dias diferem.** A lavra cria dias por períodos, atraso, órfãos, faltas, furos e grade; não contém filtro `ini <= dia <= fim`. V3 produz dias com minutos e ainda dias bloqueados pelo motor, com recorte da janela. `app/ponto/services/dia_pago.py:173-290`; `app/ponto/calculador/alimentacao.py:454-462`.

- **O DIFF descarta parte da resposta da porta v3.** Embora `rubricas_por_dia` inclua dias bloqueados sem segmento, o comando monta `c_dia` iterando somente `v.minutos`, não o retorno inteiro da porta. Assim, pode ignorar uma rubrica efetivamente devolvida; na união posterior, a ausência vira `{}` e então zero. A cegueira é registrada apenas para dias que passaram pelo laço. `app/ponto/management/commands/diff_calculador.py:417-431`, `:469-472`, `:501-535`; `app/ponto/calculador/alimentacao.py:454-458`.

- **A linha de ajuste exige tratamento explícito.** Ela nasce sempre, com `data=None`; guardar todas as linhas em `{dia: campos}` pode incluir `None` no `sorted` da união com datas. Filtrar somente rubricas de `alvo` não remove a linha. `app/ponto/services/dia_pago.py:346-356`; `app/ponto/management/commands/diff_calculador.py:527`.

- **Os universos de colaboradores não são iguais por construção.** O DIFF parte de IDs com `FechamentoMensal` já existente, depois aplica `--colab`/limite. A captura recalcula a empresa pelo `universo_da_competencia`. Também continua chamando `autoridade_do_periodo` e excluindo erros antes de consultar a captura. Portanto, `sem_lavra_do_fechamento == 0` não decorre do código. `app/ponto/management/commands/diff_calculador.py:97-108`, `:140-150`; `app/ponto/services/fechamento.py:538-541`.

- **Mesmo resultado não significa mesmos demais insumos.** O comando troca somente `aut.resultado`, conserva `aut.esc` e `aut.feriados`, e passa esses valores à porta. Fechamento usa `vinculo_de_referencia`; espelho usa vínculo no último dia da janela. A porta resolve CCT/família uma vez em `ini` com esse template e só depois ajusta template por dia. Possível efeito concreto na frota: [não verificado]. `app/ponto/management/commands/diff_calculador.py:151-163`, `:361`, `:417-423`; `app/ponto/services/fechamento.py:823-827`; `app/ponto/services/espelho.py:1069-1070`; `app/ponto/calculador/alimentacao.py:377-395`, `:476-486`.

- **Capturar primeira instância é dependência de ordem.** O hook atual guarda a primeira `ResultadoMes` por colaborador com `setdefault`; não identifica semanticamente o composto. O composto hoje nasce em `apurar_por_fatia` antes das chamadas por fatia, o que sustenta esse comportamento atual. `app/ponto/management/commands/diff_calculador.py:120-138`; `app/ponto/services/fechamento.py:182`, `:270`.

- **As duas lavras usam o mesmo nome local.** A chamada motor importa `_lavrar_dia_pago`, e a chamada oráculo reutiliza esse alias; o captador precisa ignorar versões diferentes de `motor`. Buscar a “real” pelo nome já substituído causaria recursão; a referência original precisa ser guardada antes do patch, como `_ol` hoje. `app/ponto/management/commands/diff_calculador.py:121-142`; `app/ponto/services/fechamento.py:1167-1169`, `:1195`, `:1233-1245`.

- **Falha de lavra não alcança automaticamente o comando.** Fechamento captura a exceção da chamada motor e apenas registra erro; o captador precisa contabilizá-la dentro da própria chamada e só publicar linhas completas após sucesso. Ausência, falha e chamada bem sucedida sem dias precisam permanecer distinguíveis; a lavra sempre devolve ao menos a linha de ajuste. `app/ponto/services/fechamento.py:1206-1209`; `app/ponto/services/dia_pago.py:346-359`.

## 31/10 11:31 · sessao b69471ac8b0b · - · - · origem=Ronald (tmux)

Achados em `fechamento.py`:

- **“Nenhuma escrita porque somente_leitura” é falso para a chamada proposta.** `somente_leitura=True, lavrar_dias_pagos=True` ativa consulta às exportadas; para setembro/2026 guardado, exclui colaboradores antes da apuração e chama `evento_kw`, que chega a `LogAuditoria.objects.create`. O patch de `DiaPago` durante `lavrar` não alcança essa escrita anterior. `app/ponto/services/fechamento.py:36`, `app/ponto/services/fechamento.py:523`, `app/ponto/services/fechamento.py:545`, `app/ponto/services/fechamento.py:551`, `app/core/observ.py:142`, `app/core/observ.py:133`.

- **Universo de setembro pode perder colaboradores antes de qualquer captura.** A guarda `(9,2026)` está ativa na cópia e exclui exportadas; se há empresas exportadas no dado real não foi informado. `app/ponto/services/fechamento.py:36`, `app/ponto/services/fechamento.py:545`; presença em produção: [não verificado].

- **Resultado composto tem dono claro.** `apurar_por_fatia` cria `ResultadoMes(colaborador_id=colab.id)` antes dos resultados das fatias; concatena `periodos`/`periodos_ft`, soma campos e mescla `dia_da_jornada`. Os mesmos objetos de período conservam a chave `id(p)` do mapa. `app/ponto/services/fechamento.py:180`, `app/ponto/services/fechamento.py:96`, `app/ponto/services/fechamento.py:270`, `app/ponto/services/fechamento.py:278`, `app/ponto/services/fechamento.py:295`, `app/ponto/services/fechamento.py:729`.

- **Origens kwargs da lavra:** `periodos` vem do composto; `_ft_certa` é filtrado por escala certa na data local da entrada; `_ft_feriado` usa `escala_vigente` e `feriado_vira_folga` nessa mesma data; `_ft_sem_escala` é complemento por identidade das duas listas. `app/ponto/services/fechamento.py:1171`, `app/ponto/services/fechamento.py:908`, `app/ponto/services/fechamento.py:919`, `app/ponto/services/fechamento.py:927`, `app/ponto/services/fechamento.py:948`.

- **Demais origens kwargs:** órfãos saem de outra execução de motor e de outro resultado `_r_orfa`, sem incorporação ao composto; faltas vêm de `minutos_de_falta_por_dia`; atraso/crédito de `credito_parcial`; abono/previsto/realizado de `por_dia_da_grade`; furo de `resultado.datas_falta` com teto temporal; reflexo/banco dos campos montados do fechamento. `app/ponto/services/fechamento.py:802`, `app/ponto/services/fechamento.py:813`, `app/ponto/services/fechamento.py:848`, `app/ponto/services/fechamento.py:962`, `app/ponto/services/fechamento.py:1028`, `app/ponto/services/fechamento.py:1083`, `app/ponto/services/fechamento.py:1069`, `app/ponto/services/fechamento.py:1172`.

- **Mesmo `dia_da_jornada` não uniformiza todos os dias:** FT é classificado pela data local da entrada; órfãos são agrupados por essa data; `atraso_por_dia` também agrupa pela data local, enquanto crédito usa a data da ausência. São mapas distintos entregues junto do mapa da jornada. `app/ponto/services/fechamento.py:910`, `app/ponto/services/fechamento.py:922`, `app/ponto/services/fechamento.py:814`, `app/ponto/services/credito_parcial.py:29`, `app/ponto/services/credito_parcial.py:36`, `app/ponto/services/credito_parcial.py:23`, `app/ponto/services/fechamento.py:1185`.

- **Escritor do mapa de jornada:** motor mescla em `_aplicar_teto_pontualidade` e também expõe os FT; fechamento mescla os mapas das fatias. `app/ponto/motor_calculo_v2.py:1006`, `app/ponto/motor_calculo_v2.py:2889`, `app/ponto/services/fechamento.py:295`.

- **Captura de resultado não comprova lavra.** Resultado nasce antes do `try` do colaborador; erro posterior pode ser engolido; leitura é registrada antes da chamada à lavra. O retorno não informa falha da lavra. `app/ponto/services/fechamento.py:729`, `app/ponto/services/fechamento.py:834`, `app/ponto/services/fechamento.py:1096`, `app/ponto/services/fechamento.py:1206`, `app/ponto/services/fechamento.py:1271`, `app/ponto/services/fechamento.py:1277`.

- **Contador de erro precisa estar dentro do captador.** A exceção da lavra motor é apenas logada pelo fechamento; não chegará ao chamador externo. O oráculo tem outro `except` igualmente absorvente. `app/ponto/services/fechamento.py:1206`, `app/ponto/services/fechamento.py:1265`.

- **Oráculo usa exatamente o mesmo alias.** O import local de `lavrar as _lavrar_dia_pago` serve à primeira chamada motor e depois é reutilizado na segunda chamada oráculo. O wrapper deve interceptar ambas e deixar explícito o tratamento de `versao!='motor'`. Preparar o oráculo (`insumos`/`rubricas`) ocorre mesmo se a chamada seguinte virar no-op. `app/ponto/services/fechamento.py:1168`, `app/ponto/services/fechamento.py:1195`, `app/ponto/services/fechamento.py:1233`, `app/ponto/services/fechamento.py:1245`, `app/ponto/services/fechamento.py:1262`.

- **Comentário de última fatia está desatualizado:** o `esc`/`tipo_escala` entregue ao oráculo é atualmente o vínculo de referência escolhido consultando do último dia da janela para trás; não necessariamente a última fatia executada. `app/ponto/services/fechamento.py:111`, `app/ponto/services/fechamento.py:123`, `app/ponto/services/fechamento.py:826`, `app/ponto/services/fechamento.py:1229`, `app/ponto/services/fechamento.py:1241`.

## 31/10 11:31 · sessao 95438681c14a · - · - · origem=Ronald (tmux)

Achados de `lavrar` e `DiaPago`, todos por inspeção estática:

- `lavrar` importa `DiaPago` **dentro da função**, portanto substituir `ponto.models.DiaPago` antes de chamar a função real alcança essa chamada; além de constantes e manager, o substituto deve aceitar construção `DiaPago(**kwargs)`, usada para linhas de dia e ajuste. Não há outra operação de persistência dentro de `lavrar`: as operações são `objects.filter(...).delete()` e `objects.bulk_create(novas)`. Os auxiliares chamados ali, `_dia_de` e `_DONOS_DECLARADOS`, só leem e processam memória. (`app/ponto/services/dia_pago.py:109`, `app/ponto/services/dia_pago.py:163`, `app/ponto/services/dia_pago.py:343`, `app/ponto/services/dia_pago.py:355`, `app/ponto/services/dia_pago.py:357`, `app/ponto/services/dia_pago.py:358`)

- A promessa de **nenhuma escrita da operação inteira** falha por outro caminho: `somente_leitura=True, lavrar_dias_pagos=True` ativa o cálculo de exportadas; a guarda de setembro pode excluir colaboradores e escrever auditoria por `evento_kw`, independentemente do captador de `DiaPago`. `evento_kw` chama `evento`, que executa `LogAuditoria.objects.create()`. A guarda está configurada para `(9, 2026)`. A ocorrência efetiva depende das empresas exportadas e dos colaboradores: [não verificado]. (`app/ponto/services/fechamento.py:36`, `app/ponto/services/fechamento.py:523`, `app/ponto/services/fechamento.py:544`, `app/ponto/services/fechamento.py:548`, `app/ponto/services/fechamento.py:553`, `app/core/observ.py:133`, `app/core/observ.py:134`, `app/core/observ.py:142`)

Leitura dos argumentos e escrita dos campos:

| Argumento/fonte lido | Campo/efeito escrito pela lavra | Fonte |
|---|---|---|
| `periodos` | `horas_trabalhadas`, `horas_noturnas`, HE50, HE50 noturna, HE100, HE100 feriado, HE100 noturna, saída antecipada e intra | `app/ponto/services/dia_pago.py:173`–`186` |
| `dia_da_jornada` | Determina o dia pelo `mapa[id(p)]`; faltando entrada retorna `None`; mapa ausente ou incompleto cai na data local da entrada | `app/ponto/services/dia_pago.py:126`–`132` |
| `atraso_por_dia`, `credito_por_dia` | `horas_atraso=max(atraso-min(atraso,credito),0)/60`; detalhe `credito_parcial_min` | `app/ponto/services/dia_pago.py:190`–`195` |
| `ft_certa` | Folga trabalhada; também noturno e intra | `app/ponto/services/dia_pago.py:223`–`240` |
| `ft_feriado` | HE100 **e** HE100 feriado pelo total trabalhado; também noturno, intra e detalhe `folga_feriado_min` | `app/ponto/services/dia_pago.py:244`–`256` |
| `ft_sem_escala` | Trabalhadas; também noturno, intra e detalhe `folga_sem_escala_min` | `app/ponto/services/dia_pago.py:257`–`265` |
| `orfaos_por_dia` | Trabalhadas e detalhe `o85_dia_sem_vinculo_min` | `app/ponto/services/dia_pago.py:267`–`270` |
| `faltas_por_dia` | Falta e detalhe `falta_min` | `app/ponto/services/dia_pago.py:272`–`275` |
| `furos_do_motor` | Cria linha mesmo sem horas; grava `furo_apurado` ternário conforme `None`/lista | `app/ponto/services/dia_pago.py:279`–`281`, `app/ponto/services/dia_pago.py:343`–`345` |
| `abono_por_dia`, `previsto_por_dia`, `realizado_por_dia` | Criam linhas e preenchem os três `minutos_*`, sem conversão para horas | `app/ponto/services/dia_pago.py:286`–`290` |
| `rubricas`, `do_motor` | Verifica propriedade de todos os `CAMPOS_DIA`; pode levantar `ValueError`; substitui valores decididos, preservando campos declarados `do_motor`; grava `calculador_nao_decide` | `app/ponto/services/dia_pago.py:310`–`331` |
| `versao`, `versao_motor`, `versao_dna`, `evento_origem` | Metadados comuns | `app/ponto/services/dia_pago.py:333`–`334` |
| `regua_por_dia` | `versao_motor` por dia, truncado em 32; contador de dias sem régua no detalhe do ajuste | `app/ponto/services/dia_pago.py:336`–`356` |
| `reflexo_dsr`, `banco` | Só `horas_reflexo_dsr` e `saldo_banco_horas` da linha de ajuste | `app/ponto/services/dia_pago.py:348`–`356` |
| `colab`, `mes`, `ano`, `versao` | Identificação das novas linhas e filtro do delete | `app/ponto/services/dia_pago.py:343`, `app/ponto/services/dia_pago.py:355`, `app/ponto/services/dia_pago.py:357` |

O chamador motor entrega esses argumentos em `app/ponto/services/fechamento.py:1169`–`1205`; o chamador oráculo usa o **mesmo alias local** `_lavrar_dia_pago` em `app/ponto/services/fechamento.py:1245`–`1264`. Logo, restaurar somente o atributo do módulo antes da segunda chamada não retira o captador do alias já importado; o captador deve tratar `versao!='motor'` explicitamente. (`app/ponto/services/fechamento.py:1168`, `app/ponto/services/fechamento.py:1245`, `app/ponto/services/fechamento.py:1262`)

O modelo define as rubricas como `FloatField(default=0.0)`, identificação, metadados e detalhe, e `lavrado_em=auto_now=True`; `furo_apurado` tem default `None`. Um captador que guarda somente kwargs não reproduz `lavrado_em` nem o default de `furo_apurado` no ajuste, que não recebe esse argumento. Isso não altera as rubricas de `alvo`, mas impede dizer que capturou todos os campos reais do modelo. (`app/ponto/models.py:315`–`339`, `app/ponto/models.py:349`–`351`, `app/ponto/models.py:369`, `app/ponto/models.py:374`–`380`, `app/ponto/services/dia_pago.py:355`)

A lavra **sempre** cria ajuste `data=None`, mesmo sem dias. Portanto, `{dia:campos}` deve distinguir `tipo='dia'` de `tipo='ajuste'`; mera presença de uma captura ou retorno maior que zero não prova presença de dia comparável. O retorno inclui o ajuste. (`app/ponto/services/dia_pago.py:346`–`359`)

Além de HE100 feriado e folga, trocar a soma manual pela lavra pode mudar trabalhadas, HE100 total, noturno, intra e atraso: a lavra redireciona FT sem escala/orfãos para trabalhadas; FT feriado para HE100; carrega noturno/intra de todos os FT; e abate crédito parcial do atraso. Os splits noturnos passam a ser preenchidos pela lavra, embora a soma antiga não os preencha. (`app/ponto/services/dia_pago.py:181`, `app/ponto/services/dia_pago.py:184`, `app/ponto/services/dia_pago.py:190`–`195`, `app/ponto/services/dia_pago.py:223`–`270`; `app/ponto/management/commands/diff_calculador.py:253`–`268`)

As exceções da chamada motor são engolidas pelo fechamento, que só registra erro; exceções do oráculo também são engolidas; há ainda `except` externo por colaborador. O contador específico proposto precisa ser atualizado **no captador antes de propagar**, pois o comando não receberá necessariamente a exceção. (`app/ponto/services/fechamento.py:1206`–`1209`, `app/ponto/services/fechamento.py:1265`–`1269`, `app/ponto/services/fechamento.py:1271`–`1275`)

## 31/10 11:24 · sessao f3d5f1a4d064 · V3-RETOMA · verificador · origem=Code

**Parecer estático: correção parcial.** O template passa a ser o do dia, mas a CCT continua consultada em `ini`, com cache apenas por `template.pk`. Com a régua habilitada, pode combinar **ciclo do dia com praça/CCT do início do período**. Isso impede considerar completa a promessa de “quatro insumos do dia” (`app/ponto/calculador/alimentacao.py:383`, `app/ponto/calculador/alimentacao.py:389`, `app/ponto/calculador/alimentacao.py:392`, `app/core/regua_cct.py:458`).

Execução de testes, RED/GREEN, impacto individual em produção e cumprimento dos pré-requisitos do pouso: **[nao verificado]**. [nao verificado]

**1. Quem escreve, quem lê e quais juízes existem**

| Campo/valor | Escritor identificado na cópia | Leitor no caminho examinado |
|---|---|---|
| Template: `tipo_ciclo`, `tipo_base` | Serviço de cadastro monta o `TipoEscala` com esses campos (`app/escala/services/cadastro_tipo.py:151`, `app/escala/services/cadastro_tipo.py:174`). | Alimentação lê o template do vínculo; fábrica lê ciclo/base (`app/ponto/calculador/alimentacao.py:492`, `app/core/regua_cct.py:440`, `app/ponto/motor_calculo_v2.py:3561`). |
| Vínculo: template, posto, início, fim, ativo | Porta de vínculo atualiza ou cria o vínculo (`app/colaboradores/services/vinculo.py:265`, `app/colaboradores/services/vinculo.py:278`). | Juiz lê geradora/vigência e escolhe o vínculo (`app/escala/alimentacao.py:237`, `app/escala/alimentacao.py:250`, `app/escala/alimentacao.py:253`). |
| Célula: `escala_geradora` e DNA | Porta da célula grava a geradora e o DNA (`app/ponto/portas/celula.py:247`). | `vinculo_do_dia` lê `escala_geradora_id` (`app/escala/alimentacao.py:238`). |
| Empresa: `regime_trabalhista` | Tela recebe o campo e passa à porta de estrutura; a porta persiste (`app/colaboradores/views.py:512`, `app/colaboradores/views.py:521`, `app/colaboradores/services/estrutura.py:17`). | Régua lê a empresa do colaborador e seu regime (`app/core/regua_cct.py:352`). |
| Aplicação da CCT: sindicato, empresa, praça, ativo | Porta de aplicação cadastra e desativa (`app/core/services/aplicacao_convencao.py:101`, `app/core/services/aplicacao_convencao.py:120`, `app/core/services/aplicacao_convencao.py:151`). | `_aplicacao_vigente` lê empresa, praça, ativo e sindicato (`app/core/regua_cct.py:253`, `app/core/regua_cct.py:256`). |
| Vigência e parâmetros cadastrados da CCT | Tela recebe vigência e flags, atribui e salva `Sindicato` (`app/core/views_config.py:268`, `app/core/views_config.py:275`, `app/core/views_config.py:297`, `app/core/views_config.py:307`, `app/core/views_config.py:320`). | Régua verifica vigência e lê parâmetros (`app/core/regua_cct.py:261`, `app/core/regua_cct.py:374`). |
| Dobra efetiva | Fábrica deriva pelo ciclo; CCT pode conceder dobra ao 12x36; motor guarda o resultado (`app/ponto/motor_calculo_v2.py:3607`, `app/core/regua_cct.py:477`, `app/ponto/motor_calculo_v2.py:737`). | Fronteira `cct_do_motor` → alimentação → regra (`app/ponto/calculador/nucleo.py:351`, `app/ponto/calculador/alimentacao.py:510`, `app/ponto/calculador/alimentacao.py:524`). |
| Prorrogação efetiva | Régua legal resolve pelo ciclo; fábrica injeta o parâmetro; motor guarda (`app/core/regua_cct.py:182`, `app/core/regua_cct.py:466`, `app/ponto/motor_calculo_v2.py:712`). | Fronteira → alimentação → regra (`app/ponto/calculador/nucleo.py:348`, `app/ponto/calculador/alimentacao.py:508`, `app/ponto/calculador/alimentacao.py:530`). |
| Hora reduzida afastada | Fábrica restringe a concessão ao 12x36; motor guarda (`app/core/regua_cct.py:470`, `app/ponto/motor_calculo_v2.py:719`). | Fronteira → alimentação → regra (`app/ponto/calculador/nucleo.py:349`, `app/ponto/calculador/alimentacao.py:509`, `app/ponto/calculador/alimentacao.py:530`). |
| Lei da família | Tabela por identidade da classe declara as famílias isentas; fábrica escolhe a classe (`app/ponto/calculador/regras.py:487`, `app/ponto/motor_calculo_v2.py:3554`). | Leitor único consulta a tabela; diff entrega a causa à regra (`app/ponto/calculador/regras.py:502`, `app/ponto/calculador/regras.py:518`, `app/ponto/calculador/alimentacao.py:403`, `app/ponto/calculador/alimentacao.py:531`). |

O escritor novo é somente o cache local `_cct_por_template`; a saída continua passando por `regras.do_dia`. A persistência das rubricas continua na porta `dia_pago.lavrar` (`app/ponto/calculador/alimentacao.py:403`, `app/ponto/calculador/alimentacao.py:516`, `app/ponto/services/dia_pago.py:320`, `app/ponto/services/dia_pago.py:357`).

**Há juiz de vínculo registrado:** `vinculo_do_dia`, e também o predicado `vinculo_cobre_a_data` (`app/core/juizes.py:1242`, `app/core/juizes.py:1250`). **Há autoridade de CCT na tabela documental:** `regua_para` decide parâmetros e `_aplicacao_vigente` decide aplicação (`app/docs/JUIZES.md:30`).

O diff consulta o juiz do vínculo com `k`; consulta a fábrica de CCT com `ini`. A fábrica, quando habilitada, chama `regua_para`, que resolve novamente vínculo/praça nessa outra data. Portanto, **não copia o critério do juiz; fornece datas diferentes para perguntas que deveriam estar coerentes** (`app/ponto/calculador/alimentacao.py:430`, `app/ponto/calculador/alimentacao.py:490`, `app/ponto/calculador/alimentacao.py:392`, `app/core/regua_cct.py:329`, `app/core/regua_cct.py:458`).

**2. Casos de borda**

| Caso | Resultado da leitura estática |
|---|---|
| **Sem vínculo na data** | O juiz pode devolver `None`, mas `_te_dia` conserva o template do chamador. A nova consulta pode afirmar parâmetros de outro dia, em vez de declarar ausência do vínculo (`app/escala/alimentacao.py:253`, `app/ponto/calculador/alimentacao.py:487`, `app/ponto/calculador/alimentacao.py:491`, `app/ponto/calculador/alimentacao.py:507`). Esse reaproveitamento do chamador já estava no caminho; o diff passa a usá-lo também para os quatro insumos. |
| **Dois vínculos cobrindo a data** | Com célula, manda a geradora válida; sem geradora, o juiz escolhe por `(ativa, data_inicio)`. Não há desempate novo no diff. Continua possível consultar praça/CCT de outro vínculo em `ini` (`app/escala/alimentacao.py:237`, `app/escala/alimentacao.py:247`, `app/escala/alimentacao.py:254`, `app/core/regua_cct.py:329`). |
| **Vínculo muda no meio da competência** | O template acompanha `k`; praça/CCT continuam em `ini`. Mesmo template em dois vínculos/postos também compartilha a entrada do cache (`app/ponto/calculador/alimentacao.py:389`, `app/ponto/calculador/alimentacao.py:392`, `app/ponto/calculador/alimentacao.py:490`). |
| **Turno cruza meia-noite** | Não aparece corte civil novo: os segmentos usam `dia_da_jornada`, e a pontualidade ancora os marcos nesse dia, acrescentando o dia seguinte pelo juiz de cruzamento. Turno aberto também chega ao teto como aberto. O teste novo não prova essa borda (`app/ponto/calculador/alimentacao.py:63`, `app/ponto/calculador/alimentacao.py:278`, `app/ponto/calculador/alimentacao.py:287`, `app/ponto/calculador/regras.py:268`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:87`). |
| **Template nulo** | Chamador nulo é corrigido quando existe template no vínculo do dia. Sem nenhum template, a fábrica escolhe geometria `Motor12x36`, mas ciclo desconhecido e dobra verdadeira. `tipo_escala=NULL` no próprio vínculo não é estado regular do modelo (`app/ponto/calculador/alimentacao.py:492`, `app/escala/models.py:874`, `app/ponto/motor_calculo_v2.py:3554`, `app/ponto/motor_calculo_v2.py:3607`). |
| **CCT fora de vigência** | A vigência é verificada, porém em `ini`. Uma CCT válida em `ini` e expirada em `k` pode continuar aplicada; uma que começa depois de `ini` pode ser ignorada. É falha de data, não ausência de validação (`app/core/regua_cct.py:261`, `app/core/regua_cct.py:366`, `app/ponto/calculador/alimentacao.py:392`). |
| **Empresa sem regime** | É ramo explícito de piso legal, não exceção nem CCT desconhecida. O piso mantém prorrogação verdadeira e afastamento da hora reduzida falso; a dobra efetiva continua dependendo do ciclo. Não identifiquei falha nova específica desse caso (`app/core/regua_cct.py:39`, `app/core/regua_cct.py:210`, `app/core/regua_cct.py:363`, `app/ponto/motor_calculo_v2.py:3607`). |

Esses efeitos são condicionais ao cadastro e ao caminho percorrido; sua ocorrência individual no dado de produção fornecido é **[nao verificado]**.

**3. Outros sítios com referência diferente do dia**

- **Na própria porta V3:** CCT em `ini`, cache por template (`app/ponto/calculador/alimentacao.py:389`, `app/ponto/calculador/alimentacao.py:392`).
- **Motores que fornecem períodos à V3:** fechamento e espelho consultam CCT em `fatia_ini`. A fatia é delimitada pela vigência do vínculo; esse recorte não considera fronteiras da CCT dentro do mesmo vínculo (`app/ponto/services/fechamento.py:203`, `app/ponto/services/fechamento.py:711`, `app/ponto/services/espelho.py:1115`).
- **Motor de órfãos:** consulta na primeira data órfã para o conjunto processado (`app/ponto/services/fechamento.py:793`, `app/ponto/services/fechamento.py:803`).
- **Relatório do DIFF:** recria motor em `ini`, com template de `aut.esc`; nesse trecho o resultado serve à identificação da classe, não aos parâmetros monetários da porta (`app/ponto/management/commands/diff_calculador.py:163`, `app/ponto/management/commands/diff_calculador.py:345`, `app/ponto/management/commands/diff_calculador.py:348`).
- **V3 de ensaio:** `periodo.reler` recebe uma única CCT e uma única causa de família para todos os dias. O próprio módulo declara esse caminho como ensaio (`app/ponto/calculador/periodo.py:540`, `app/ponto/calculador/periodo.py:543`, `app/ponto/calculador/periodo.py:787`, `app/ponto/calculador/periodo.py:829`).
- **Referências do chamador:** espelho escolhe vínculo no fim da janela; fechamento escolhe o último dia respondido pelo juiz. A descrição do patch como “última fatia” está imprecisa perante a implementação atual do fechamento (`app/ponto/services/espelho.py:1069`, `app/ponto/services/fechamento.py:123`, `app/ponto/services/fechamento.py:826`).

**4. O teste morde?** [nao verificado]

**Morde a volta ao template do chamador:** os dois primeiros casos exigem zero no dia 12x36 com chamador nulo/outro ciclo; o terceiro exige 12 horas no dia 24x48 com chamador 12x36. Pela fábrica, reverter essa seleção deve contrariar as três asserções; RED efetivamente executado é **[nao verificado]** (`app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:104`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:111`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:116`, `app/ponto/motor_calculo_v2.py:3607`).

**Não passa por ausência da resposta:** `_fer` exige o dia, recusa rubrica declarada e retorna um valor que será comparado a zero ou 12. Os dois casos de zero, isoladamente, aceitariam zeragem indiscriminada; o terceiro a reprova (`app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:98`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:107`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:119`).

**Não sela toda regressão ao “vínculo de hoje”.** Se a regressão ocorrer dentro de `regua_para`, pode permanecer verde: a fixture não declara regime nem cadastra CCT/aplicação/switch. Com switch desligado, a fábrica nem consulta `regua_para`; com regime vazio, a régua retorna piso antes da aplicação (`app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:59`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:83`, `app/core/regua_cct.py:449`, `app/core/regua_cct.py:363`).

Também não afirma prorrogação, hora reduzida ou lei da família. Logo, **o defeito `ini` + cache por template pode passar nesse teste** (`app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:104`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:119`, `app/ponto/calculador/alimentacao.py:389`, `app/ponto/calculador/alimentacao.py:392`).

**5. Checklist das classes**

| Classe | Aparece no diff? |
|---|---|
| **Segundo juiz duplicando registrado** | **Não no helper novo:** ele chama fábrica e leitores existentes. O problema é a data/contexto fornecido (`app/ponto/calculador/alimentacao.py:392`, `app/ponto/calculador/alimentacao.py:403`). |
| **Segundo escritor fora da porta** | **Não identificado no trecho produtivo:** escreve cache local; a persistência continua pela porta existente (`app/ponto/calculador/alimentacao.py:403`, `app/ponto/services/dia_pago.py:357`). |
| **Relógio fora da borda** | **Não aparece nos acréscimos:** usam `ini`, `k` e datas explícitas no teste. Isso não torna `ini` a data correta (`app/ponto/calculador/alimentacao.py:392`, `app/ponto/calculador/alimentacao.py:490`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:74`). |
| **Derivação paralela de fonte única** | **Não há fórmula nova copiada.** Há contexto divergente: vínculo/template em `k`, praça/CCT em `ini`, e reaproveitamento do chamador quando o juiz cala (`app/ponto/calculador/alimentacao.py:392`, `app/ponto/calculador/alimentacao.py:487`, `app/core/regua_cct.py:329`). |
| **Selo/RED sem mordida** | **Cobertura parcial:** morde o ciclo do chamador e ausência de resposta; não morde CCT por data nem os quatro insumos prometidos. Evidência de execução do RED: **[nao verificado]** (`app/ponto/calculador/alimentacao.py:383`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:98`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:104`). |
| **Ordem de trava** | **Nenhuma aquisição nova de trava nos acréscimos.** Ordem efetivamente executada na esteira: **[nao verificado]** (`app/ponto/calculador/alimentacao.py:385`, `app/ponto/calculador/alimentacao.py:507`). |
| **Pouso antes do pré-requisito** | **[nao verificado].** O teste existente não comprova RED, suíte verde ou leitura final. Esses requisitos estão escritos em `CLAUDE.md:28`, `CLAUDE.md:598`, `CLAUDE.md:604`. | [nao verificado]

## 31/10 11:22 · sessao 5687aea26424 · - · - · origem=Ronald (tmux)

Achado impeditivo: a cura troca o **template pelo do dia**, mas continua perguntando a **CCT na data inicial da janela** e armazenando a resposta apenas por `template.pk`. Assim, combina ciclo de `k` com praça/CCT de `ini`; mudança de praça, vigência ou convenção dentro da competência permanece descoberta. (`app/ponto/calculador/alimentacao.py:389`, `:392`, `:405`; `app/core/regua_cct.py:329`, `:330`, `:366`.)

Casos:

- **Sem vínculo no dia:** `_te_dia` conserva o template do chamador; `None` do juiz não declara a CCT desconhecida. Pode aplicar ciclo de outro dia. (`app/ponto/calculador/alimentacao.py:487`, `:490`, `:507`; `app/escala/alimentacao.py:253`.)
- **Dois vínculos:** com célula, geradora válida manda; sem célula, o juiz desempata por `(ativa, data_inicio)`. O diff usa esse juiz; a inconsistência restante é perguntar novamente em `ini` para obter praça/CCT. (`app/escala/alimentacao.py:237`, `:247`, `:250`, `:253`, `:254`; `app/core/regua_cct.py:329`.)
- **Troca durante competência:** template acompanha o dia; CCT não. Mesmo template usado em dois postos compartilha a mesma entrada no cache. (`app/ponto/calculador/alimentacao.py:389`, `:392`, `:490`, `:494`.)
- **Cross-meia-noite:** não identifiquei corte civil novo: o dia vem de `dia_da_jornada`, e esse dia chega ao juiz. A pontualidade também ancora nele e usa o juiz de cruzamento. Teste novo só cobre 07–19 no mesmo dia. (`app/ponto/calculador/alimentacao.py:63`, `:68`, `:278`, `:287`, `:490`; `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:84`.)
- **Template nulo:** parâmetro do chamador nulo é corrigido quando há vínculo do dia. `EscalaColaborador.tipo_escala` não admite `NULL`; ausência real de template no vínculo não é estado regular do modelo. Se nenhum template chega, fábrica usa geometria `Motor12x36`, ciclo desconhecido e dobra verdadeira. (`app/ponto/calculador/alimentacao.py:492`, `:494`; `app/escala/models.py:874`; `app/ponto/motor_calculo_v2.py:3554`, `:3559`, `:3607`.)
- **CCT fora da vigência:** juiz verifica vigência na data recebida; aqui recebe `ini`. CCT válida em `ini` e expirada em `k` continua aplicada; CCT iniciada depois de `ini` pode ser ignorada durante toda janela. (`app/core/regua_cct.py:261`, `:263`, `:366`, `:371`; `app/ponto/calculador/alimentacao.py:392`.)
- **Empresa sem regime:** fluxo explícito retorna piso legal; não gera CCT desconhecida nem exceção. Prorrogação legal específica do 12x36 só entra com CLT declarada. (`app/core/regua_cct.py:352`, `:363`, `:364`, `:210`, `:211`.) Com switch desligado, `regua_para` não é consultada. (`app/core/regua_cct.py:449`, `:457`.)

Outros sítios com referência diferente do dia:

- **V3/porta:** `_cct_de(...ini...)`, já descrito. (`app/ponto/calculador/alimentacao.py:392`.)
- **Relatório DIFF:** fábrica com `ini` e template de `aut.esc`; resultado só identifica a classe no relatório, não alimenta dinheiro. (`app/ponto/management/commands/diff_calculador.py:163`, `:345`, `:348`; porta efetiva `:417`.)
- **V3 de ensaio:** `periodo.reler` recebe um único `cct` e uma única causa de família para toda releitura; dobra lê esse objeto em todos dias. O módulo declara que não é caminho de produção. (`app/ponto/calculador/periodo.py:540`, `:543`, `:787`, `:829`.)
- **Motores que fornecem períodos à V3:** fechamento e espelho resolvem em `fatia_ini`. Fatias são por vigência de vínculo, não por fronteiras da CCT; uma troca de CCT dentro do mesmo vínculo não provoca nova fatia nesse código. (`app/ponto/services/fechamento.py:203`, `:205`, `:711`; `app/ponto/services/espelho.py:1115`.) Motor órfão usa primeiro dia órfão. (`app/ponto/services/fechamento.py:793`.)
- **Referências de janela:** `Autoridade` conserva vínculo do fim; fechamento usa último dia da janela que o juiz responde, não simplesmente última posição da lista. (`app/ponto/services/espelho.py:1069`, `:1070`; `app/ponto/services/fechamento.py:826`, `:123`, `:125`.)
- **Periféricos, fora da V3:** esmeril escolhe vínculo no fim e CCT no início; TXT escolhe ciclo e cláusula no fim; PDF resolve vínculo no fim. (`app/ponto/services/esmeril_espelho.py:154`, `:161`; `app/folha/export.py:348`, `:369`; `app/relatorios/pdf_espelho.py:303`.)

Execução e impacto real desses casos: [nao verificado].

## 31/10 11:21 · sessao 7f970b2f0d86 · - · - · origem=Ronald (tmux)

- **Mordida contra o defeito específico:** voltar a montar a CCT do `te` do chamador contraria os três testes: `None` e 24x48 dobrariam `dia_a`, e 12x36 zeraria `dia_b`. Fundamentação: `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:104-121`, `app/ponto/motor_calculo_v2.py:3537`, `:3554-3559`, `:3607-3608`; `app/ponto/calculador/regras.py:344-347`, `:775-785`. Trata-se de previsão estática; RED/GREEN executado **[nao verificado]**.

- **Não passa vazio no dia afirmado:** `_fer` exige presença do dia, ausência da rubrica em `nao_decide`, e retorna o valor que precisa ser exatamente 0 ou aproximadamente 12. `rub={}`, dia ausente, rubrica DECLARADA ou chave ausente falham. `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:98-102`, `:107`, `:114`, `:119`.

- **Não protege toda regressão “vínculo de hoje”:** a troca do template do chamador acusa; regressão dentro de `regua_para` para buscar vínculo ativo pode passar. A fixture não declara regime, switch, sindicato ou aplicação; `Empresa.regime_trabalhista` tem default vazio. Sem switch a fábrica sequer chama `regua_para`; com switch e regime vazio ela retorna piso legal antes de aplicação. `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:59-83`; `app/colaboradores/models.py:150-151`; `app/core/regua_cct.py:440-458`, `:353-365`.

- **Cobertura faltante:** nenhum caso de CCT que entra/sai de vigência, praça que muda, vínculos sobrepostos, ausência de vínculo na data trabalhada, template realmente nulo no vínculo, turno noturno ou cruzamento de meia-noite. Os dois turnos fabricados são 07–19, as duas escalas não se sobrepõem e possuem template. Não há asserts de prorrogação, hora reduzida ou lei da família. `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:66-96`, `:104-121`. Assim, o uso de `ini` e cache só por template pode continuar verde: `app/ponto/calculador/alimentacao.py:389-405`.

Checklist:

| Classe | Avaliação |
|---|---|
| Segundo juiz | O helper novo chama a fábrica e os leitores existentes, sem copiar critérios de CCT/família; o defeito é alimentar data inadequada e cache incompleto. `app/ponto/calculador/alimentacao.py:387-405`; `app/ponto/calculador/nucleo.py:320-351`; `app/ponto/calculador/regras.py:502-518`. |
| Segundo escritor fora da porta | Não aparece no trecho produtivo novo: grava cache local e valores locais; os `objects.create` são fixture de `TestCase`. `app/ponto/calculador/alimentacao.py:385-405`, `:507-510`; `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:51-83`. |
| Relógio fora da borda | Não aparece: chamada recebe `ini`, loop usa `k`; teste fabrica datas explícitas e apenas aplica timezone, sem `now`/`today`. `app/ponto/calculador/alimentacao.py:392`, `:470-510`; `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:26-27`, `:74-75`, `:91`. |
| Derivação paralela | Não duplica as regras; mantém, porém, seleção híbrida: ciclo do dia com CCT/praça consultadas em `ini`. `app/ponto/calculador/alimentacao.py:392`, `:490-510`; `app/core/regua_cct.py:329-339`, `:366`. |
| Selo/RED sem mordida | Morde o template do chamador e evita vazio, mas não sela a promessa dos quatro insumos nem CCT por dia. `app/ponto/calculador/alimentacao.py:383-384`; `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:98-121`. Execução do RED **[nao verificado]**. |
| Ordem de trava | O diff não acrescenta aquisição de trava nem comandos de suite; ordem efetivamente usada **[nao verificado]**. Trechos novos: `app/ponto/calculador/alimentacao.py:385-405`, `:507-510`; teste `:1-121`. |
| Pouso antes do pré-requisito | Histórico de RED, suite e opus final **[nao verificado]**. São requisitos escritos em `CLAUDE.md:28-29`, `:598-605`; existência do teste/comentário não comprova sua execução. | [nao verificado]

## 31/10 11:21 · sessao 4ae0642668fc · - · - · origem=Ronald (tmux)

Juízes existentes:

- Vínculo: registro vivo `app/core/juizes.py:1242-1251`; consulta documental `app/docs/JUIZES.md:74-79`; implementação `app/escala/alimentacao.py:196-254`. Geradora prevalece dentro da vigência; sem geradora, escolhe máximo por `(ativa, data_inicio)` entre os que cobrem a data (`app/escala/alimentacao.py:237-254`).
- CCT: autoridade explicitamente documentada em `app/docs/JUIZES.md:30-34`: `regua_para` decide parâmetros e `_aplicacao_vigente` decide onde se aplica. Não encontrei entrada nominal dessas funções em `app/core/juizes.py` [nao verificado: ausência demonstrada por busca, sem enumeração integral].
- O diff pergunta aos dois caminhos, mas com datas diferentes: vínculo pelo dia `k` (`app/ponto/calculador/alimentacao.py:490-494`, casca chama juiz em `:430-431`); CCT com **`ini`**, mesmo depois de obter template do dia (`app/ponto/calculador/alimentacao.py:392`). `regua_para` consulta vínculo/praça por essa data recebida (`app/core/regua_cct.py:328-339`), e `_aplicacao_vigente` verifica vigência CCT nela (`app/core/regua_cct.py:261-263`).
- Cache novo separa somente `template.pk` (`app/ponto/calculador/alimentacao.py:389-405`), portanto não separa mudança de praça/vínculo ou vigência CCT com template igual; o código guarda o primeiro resultado por template e o reutiliza (`app/ponto/calculador/alimentacao.py:390-405`).

Mapa de escritores/leitores:

| Valor | Escritor(es) identificados | Leitura que chega ao diff |
|---|---|---|
| Template e `tipo_ciclo` | `salvar_tipo` cria campos canônicos em `app/escala/services/cadastro_tipo.py:151-176` e salva em `:306` | Vínculo do dia fornece `tipo_escala` em `app/ponto/calculador/alimentacao.py:490-496`; fábrica lê ciclo em `app/core/regua_cct.py:440` |
| Vínculo, template e posto | `executar_vinculo` atualiza/cria em `app/colaboradores/services/vinculo.py:265-284`; campos do modelo em `app/escala/models.py:871-877` | Juiz `app/escala/alimentacao.py:237-254`; alimentação chama em `app/ponto/calculador/alimentacao.py:430-431` |
| Geradora da célula | Porta da célula grava em `app/ponto/portas/celula.py:248-260`, `:535`, `:595-613` | Juiz lê `escala_geradora_id` em `app/escala/alimentacao.py:237-247` |
| Empresa/regime | UI em `app/colaboradores/views.py:512-523` e `:531-539`; persistência `app/colaboradores/services/estrutura.py:12-20` | `regua_para`, `app/core/regua_cct.py:352-365`; switch desligado também lê regime em `:452-454` |
| Aplicação CCT empresa/praça/sindicato/ativo | Porta `app/core/services/aplicacao_convencao.py:101-132`, desativação `:138-155` | `_aplicacao_vigente`, `app/core/regua_cct.py:253-274` |
| Vigência CCT e flags cadastrados | UI `app/core/views_config.py:268-297`, atribuição/persistência `:307-335`; campos/defaults `app/core/models.py:38-74` | Vigência em `app/core/regua_cct.py:261-263`; flags em `:374`; fábrica injeta em `:466-478` |
| Dobra efetiva | `get_motor` deriva pelo ciclo em `app/ponto/motor_calculo_v2.py:3607-3608`; CCT pode conceder dobra em `app/core/regua_cct.py:477-478`; `MotorBase` grava em `app/ponto/motor_calculo_v2.py:737` | Leitor único `app/ponto/calculador/nucleo.py:344-351`; diff lê CCT em `app/ponto/calculador/alimentacao.py:510`, entrega em `:524` |
| Prorrogação efetiva | Regra legal `app/core/regua_cct.py:153-182`; regime CLT aplica em `:211-212`; fábrica injeta cadastro em `:466`; motor grava em `app/ponto/motor_calculo_v2.py:708-712` | Leitor único `app/ponto/calculador/nucleo.py:348`; diff lê `app/ponto/calculador/alimentacao.py:508`, entrega em `:530` |
| Hora reduzida afastada | Fábrica limita flag ao 12x36 em `app/core/regua_cct.py:470-472`; motor grava em `app/ponto/motor_calculo_v2.py:719` | Leitor único `app/ponto/calculador/nucleo.py:349`; diff lê `app/ponto/calculador/alimentacao.py:509`, entrega em `:530` |
| Lei da família para pontualidade | Tabela declarada por identidade de classe `app/ponto/calculador/regras.py:487-498`; fábrica escolhe classe em `app/ponto/motor_calculo_v2.py:3554-3572` | Leitor único `app/ponto/calculador/regras.py:502-518`; diff pergunta em `app/ponto/calculador/alimentacao.py:403`, entrega em `:531` |
| Cache novo | Escritor local `app/ponto/calculador/alimentacao.py:403-404` | Leitor local `app/ponto/calculador/alimentacao.py:405`, chamado por dia em `:507` |

CCT fora de vigência é descartada pelo juiz (`app/core/regua_cct.py:261-263`); sem aplicação ou empate retorna piso com fonte explícita (`app/core/regua_cct.py:366-373`). Empresa sem regime retorna piso com fonte explícita (`app/core/regua_cct.py:363-365`); esse piso mantém prorrogação `True`, hora reduzida afastada `False`, dobra cadastrada 12x36 `False` (`app/core/regua_cct.py:39-44`), mas o ciclo da fábrica ainda decide dobra efetiva (`app/ponto/motor_calculo_v2.py:3607-3608`).

O diff não introduz persistência de cadastro: grava dicionário local e lê motor pela fronteira existente (`app/ponto/calculador/alimentacao.py:385-405`). Escritas reais de produção, valores efetivos do switch e cadastros presentes no banco: [nao verificado].

## 31/10 11:10 · sessao 500b0beb443e · V3-RETOMA · verificador · origem=Code

O diff corrige a escolha do **ciclo**, mas ainda pode aplicar a **CCT errada**: passa o template do dia à fábrica mantendo `ini` como data da convenção. Há também uma inconsistência de calendário que essa troca pode ativar. `app/ponto/calculador/alimentacao.py:392`; `app/core/regua_cct.py:329`; `app/core/regua_cct.py:366`; `app/ponto/calculador/alimentacao.py:524`.

1. **Ciclo do dia combinado com praça/vigência de `ini`.**  
   `_te_dia` vem do vínculo de `k`, mas `_cct_de` chama `get_motor_cct(c, ini, ...)`. Dentro da fábrica, `regua_para` resolve a praça pelo vínculo dessa data e verifica a vigência da convenção nessa mesma data. Uma troca de praça/convenção durante a janela pode, portanto, receber os parâmetros do início da janela. A data fixa já existia; o diff mantém esse defeito ao ampliar a resolução para templates diferentes. `app/ponto/calculador/alimentacao.py:490`; `app/ponto/calculador/alimentacao.py:494`; `app/ponto/calculador/alimentacao.py:392`; `app/core/regua_cct.py:329`; `app/core/regua_cct.py:330`; `app/core/regua_cct.py:262`.

   **A fonte efetiva já existe no fechamento:** ele cria o motor com `fatia_ini` e calendário, calcula a fatia e extrai sua régua. A alimentação fabrica outro motor; usar a mesma fábrica evita copiar a política, mas não garante ler a decisão do motor que rodou. Essa reconstrução paralela é herdada e continua presente. `app/ponto/services/fechamento.py:711`; `app/ponto/services/fechamento.py:713`; `app/ponto/services/fechamento.py:263`; `app/ponto/services/fechamento.py:270`; `app/ponto/calculador/alimentacao.py:392`; `app/ponto/calculador/nucleo.py:321`; `app/ponto/calculador/periodo.py:567`.

2. **Calendário do motor descartado: contraexemplo intermitente.**  
   `cct_do_motor` devolve também `CCT.feriados`, mas `rubricas_por_dia` usa o calendário externo. O `MotorIntermitente` limpa seu calendário, enquanto sua flag de dobra fica verdadeira porque apenas `12x36` pertence a `CICLOS_FERIADO_SIMPLES`. Com chamador 12x36 e vínculo do dia intermitente, o diff pode trocar a flag anterior falsa por verdadeira, mantendo o calendário externo. `app/ponto/calculador/nucleo.py:350`; `app/ponto/calculador/alimentacao.py:407`; `app/ponto/calculador/alimentacao.py:524`; `app/ponto/motor_calculo_v2.py:3495`; `app/ponto/motor_calculo_v2.py:3537`; `app/ponto/motor_calculo_v2.py:3608`.

   Para um feriado **somente injetado, ausente no banco**, o caminho passa de zero à jornada inteira: o intermitente segue para o cálculo comum e a dobra usa esse calendário externo. É uma inconsistência herdada ativada nessa combinação pelo diff. Não se deve generalizar o exemplo para feriado cadastrado: o motor consulta o banco quando a data falta no conjunto. `app/ponto/calculador/regras.py:736`; `app/ponto/calculador/regras.py:775`; `app/ponto/calculador/regras.py:785`; `app/ponto/motor_calculo_v2.py:903`; `app/ponto/motor_calculo_v2.py:905`.

3. **Mapa de escritores e leitores.** Os nomes com `_` são locais; os leitores externos relevantes consomem os atributos do motor, o DTO `CCT` ou as rubricas resultantes. `app/ponto/calculador/alimentacao.py:403`; `app/ponto/calculador/alimentacao.py:507`; `app/ponto/calculador/nucleo.py:347`; `app/ponto/services/dia_pago.py:320`.

   | Valor/porta | Quem produz/escreve | Quem lê |
   |---|---|---|
   | `_cct` | `cct_do_motor` constrói o DTO; helper guarda no cache e desempacota por dia. `nucleo.py:347`; `alimentacao.py:403`; `alimentacao.py:507` | Extrai três flags, deixando `CCT.feriados` sem uso nesse caminho. `alimentacao.py:508`; `alimentacao.py:524` |
   | `_cct_pro` | Campo do DTO → local. `alimentacao.py:508` | Noturno e split noturno de HE. `alimentacao.py:530`; `regras.py:609`; `regras.py:815` |
   | `_cct_hr` | Campo do DTO → local. `alimentacao.py:509` | Cadastro necessário ao noturno. `alimentacao.py:530`; `regras.py:609` |
   | `_fer_dobra` | Campo do DTO → local. `alimentacao.py:510` | Disponibilidade e cálculo da dobra. `alimentacao.py:524`; `regras.py:633`; `regras.py:775` |
   | `_fora_por_lei` | Leitor da tabela por identidade da classe → cache/local. `regras.py:518`; `alimentacao.py:403`; `alimentacao.py:507` | Declara atraso/saída antecipada. `alimentacao.py:531`; `regras.py:618` |
   | `get_motor_cct` | Resolve parâmetros e entrega o motor. `core/regua_cct.py:458`; `core/regua_cct.py:466`; `core/regua_cct.py:479` | Além do helper: fechamento, relatório do DIFF e juiz da frase de pontualidade. `services/fechamento.py:711`; `commands/diff_calculador.py:348`; `chamados/juizes.py:1545` |
   | `cct_do_motor` | Lê os quatro atributos, recusando ausência/`None`. `nucleo.py:344` | Alimentação; o DTO também alimenta noturno/dobra na releitura por período. `alimentacao.py:404`; `periodo.py:508`; `periodo.py:830` |
   | `motivo_nao_julga_pontualidade` | Lê `NAO_JULGA_PONTUALIDADE`; `None` devolve `None`. `regras.py:487`; `regras.py:516` | Alimentação e `folha_julga_pontualidade`; a releitura recebe a causa pronta. `alimentacao.py:403`; `chamados/juizes.py:1551`; `periodo.py:463` |

   Os atributos originais são escritos pelo construtor do motor; a impressão da régua também os lê, fora do diff. `app/ponto/motor_calculo_v2.py:712`; `app/ponto/motor_calculo_v2.py:719`; `app/ponto/motor_calculo_v2.py:737`; `app/ponto/motor_calculo_v2.py:740`; `app/core/regua_cct.py:421`.

4. **Segundo juiz, segundo escritor e relógio: não aparecem como infrações novas neste diff.**  
   A escolha do vínculo continua delegada ao juiz registrado; a dobra continua vindo da fábrica e da constante registrada. O helper acrescenta cache, sem copiar essas políticas. `app/core/juizes.py:1250`; `app/core/juizes.py:257`; `app/ponto/calculador/alimentacao.py:430`; `app/ponto/calculador/alimentacao.py:392`; `app/ponto/motor_calculo_v2.py:3608`.

   As novas escritas são no dicionário e em variáveis locais. A persistência continua pela chamada existente a `lavrar`, cuja porta aplica as rubricas e grava `DiaPago`. `app/ponto/calculador/alimentacao.py:403`; `app/ponto/calculador/alimentacao.py:507`; `app/ponto/services/fechamento.py:1245`; `app/ponto/services/dia_pago.py:328`; `app/ponto/services/dia_pago.py:358`.

   A chamada nova usa data explícita. Há um relógio em outro consumidor da lei da família — `data or localdate()` —, mas ele pertence a `folha_julga_pontualidade`, não ao caminho acrescentado. `app/ponto/calculador/alimentacao.py:392`; `app/chamados/juizes.py:1530`; `app/chamados/juizes.py:1551`.

5. **Cache `(pk ou None)`: pode misturar objetos distintos, com limite importante.**  
   Dois templates sem PK, ou `None` e um template sem PK, compartilham a chave; o primeiro resultado vence. Já dois templates persistidos distintos no caminho normal têm PKs diferentes: os vínculos vêm do ORM com FK para `TipoEscala`. Logo, a colisão existe para objetos não salvos/dublês; ocorrência nos chamadores atuais é **[não verificado]**. `app/ponto/calculador/alimentacao.py:389`; `app/ponto/calculador/alimentacao.py:390`; `app/ponto/calculador/alimentacao.py:405`; `app/ponto/calculador/alimentacao.py:419`; `app/escala/models.py:874`; `app/escala/apps.py:5`.

   Mesmo com PKs válidas, template sozinho não identifica praça/convenção/vigência; corrigir apenas a colisão de `None` não resolve o primeiro achado. `app/core/regua_cct.py:330`; `app/core/regua_cct.py:366`; `app/core/regua_cct.py:262`.

6. **O teste morde a dobra, mas deixa os outros efeitos sem cobertura.**   [nao verificado]
   Ele exige dia presente, rubrica decidida, zero nos dois casos 12x36 e 12 horas no sentido inverso. Portanto, não é um teste inteiramente sem mordida. `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:98`; `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:107`; `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:114`; `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:119`.

   Entretanto, só afirma `horas_extras_100_feriado`, usa períodos diurnos e não cadastra convenções. Ambos os templates omitem `tipo_base`, cujo default leva ambos à mesma família com escala; assim, não testa a mudança de lei da família, nem noturno, vigência, colisão ou dia trabalhado sem vínculo. `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:54`; `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:66`; `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:87`; `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:102`; `app/escala/models.py:174`; `app/ponto/motor_calculo_v2.py:3571`.

7. **Selos AST: as formas relevantes são preservadas.**  
   A consulta ao juiz, os argumentos por dia e a ausência de consulta ORM crua no laço permanecem. I4 não proíbe o novo `getattr(..., 'pk', None)`; I7 examina escrita em atributo de motor, enquanto o helper grava no cache local. Esses selos não verificam a coerência entre `k`, `ini` e a convenção escolhida. `app/ponto/tests/test_s5b_porta_unica.py:182`; `app/ponto/tests/test_s5b_porta_unica.py:204`; `app/ponto/tests/test_s5b_porta_unica.py:217`; `app/ponto/tests/test_s5b_porta_unica.py:393`; `app/ponto/tests/test_s5b_porta_unica.py:429`; `app/ponto/tests/test_s5b_porta_unica.py:808`; `app/ponto/calculador/alimentacao.py:403`.

8. **Dias sem vínculo: o comportamento lógico permanece o anterior.**  
   `_te_dia` começa com o template do chamador e só é substituído quando o juiz retorna vínculo com template. Sem vínculo, a fábrica continua recebendo esse mesmo template e `ini`; não passa automaticamente a “sem CCT”. O teste “chamador sem template” não cobre esse caso: seus dois dias trabalhados possuem vínculos. `app/ponto/calculador/alimentacao.py:487`; `app/ponto/calculador/alimentacao.py:491`; `app/ponto/calculador/alimentacao.py:507`; `app/ponto/calculador/alimentacao.py:392`; `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:74`; `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:78`; `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:82`; `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:87`.

Execução da suíte Django completa e reprodução na frota: **[não verificado]**. [nao verificado]

## 31/10 11:08 · sessao 7d440cefcf34 · - · - · origem=Ronald (tmux)

Correção do limite: calendário vazio **não veta feriado cadastrado**, pois `eh_feriado` consulta o banco quando a data não está no conjunto. `app/ponto/motor_calculo_v2.py:903`; `app/ponto/motor_calculo_v2.py:905`.

A divergência foi reproduzida somente em memória, extraindo os métodos reais por AST e simulando `is_feriado=False`: motor com calendário vazio e flag `True` → **0 min**; contexto das regras com feriado injetado e flag `True` → **480 min**; contexto com flag anterior do chamador 12x36 (`False`) → **0 min**. O cenário demonstrado exige feriado exclusivamente injetado, ausente no banco. `app/ponto/motor_calculo_v2.py:859`; `app/ponto/motor_calculo_v2.py:901`; `app/ponto/calculador/regras.py:287`; `app/ponto/calculador/regras.py:344`; `app/ponto/calculador/regras.py:347`.

Logo, o achado deve ser apresentado como **inconsistência herdada entre calendários, ativada pelo diff nessa combinação**, sem generalizar para todo feriado intermitente. `app/ponto/calculador/alimentacao.py:507`; `app/ponto/calculador/alimentacao.py:524`; `app/ponto/motor_calculo_v2.py:3495`.

## 31/10 11:08 · sessao 87cd6c692d4f · - · - · origem=Ronald (tmux)

O teste novo **morde a dobra**, mas não cobre os outros três insumos: exige dia presente, rubrica decidida e resultados 0/12 nos dois sentidos; portanto não é vacuidade total. Ambos os períodos são diurnos e a única rubrica assertada é `horas_extras_100_feriado`. `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:87`, `:98`, `:104`, `:111`, `:116`.

A cobertura de família é particularmente insuficiente: os dois templates omitem `tipo_base`, cujo default é `12x36`; com `hora_inicio`, ambos despacham para `Motor12x36ComEscala`, apesar dos ciclos diferentes. `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:66`; `app/escala/models.py:170`; `app/ponto/motor_calculo_v2.py:3571`.

O fixture não cadastra convenção nem troca de vigência; a empresa omite `regime_trabalhista`, cujo default é vazio. Não demonstra o efeito do cache em CCTs diferentes ao longo da janela. `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:54`; `app/colaboradores/models.py:150`.

Não encontrei violação dos selos AST relevantes: executei em memória, usando AST e `unittest` da stdlib, oito métodos estruturais e dois de I4; os dez passaram. Foram os métodos de `app/ponto/tests/test_s5b_porta_unica.py:60`, `:73`, `:80`, `:103`, `:147`, `:182`, `:204`, `:217`, `:393`, `:429`. Isso não equivale à execução da suíte Django. [não verificado]

Os selos existentes continuam exigindo consulta ao juiz do vínculo, argumentos por dia e ausência de query crua no laço. O diff preserva essas formas. `app/ponto/tests/test_s5b_porta_unica.py:182`, `:204`, `:217`; `app/ponto/calculador/alimentacao.py:430`, `:490`, `:520`.

I4 proíbe `getattr` com default nos nomes da CCT e defaults booleanos nas assinaturas; o novo `getattr` consulta `pk`, fora desse conjunto. I7 examina escrita de atributo em objeto motor; o diff só lê `_m` e grava no dicionário local. Não há violação dessas formas. `app/ponto/tests/test_s5b_porta_unica.py:284`, `:317`, `:343`, `:808`; `app/ponto/calculador/alimentacao.py:389`, `:403`.

## 31/10 11:08 · sessao d3fbd053822d · - · - · origem=Ronald (tmux)

1. **CCT ainda não é inteiramente “do dia”.** A fábrica recebe `ini` fixo, mesmo após selecionar `_te_dia`; `regua_para` consulta o vínculo/praça nessa data, e `_aplicacao_vigente` testa a vigência nessa data. Assim, template de outubro pode receber a convenção/praça de setembro. O cache por template também ignora essas dimensões temporais. Referências: `app/ponto/calculador/alimentacao.py:392`, `app/core/regua_cct.py:328`, `app/core/regua_cct.py:330`, `app/core/regua_cct.py:366`, `app/core/regua_cct.py:262`.

2. **Cache conflui templates sem PK.** `_chave = getattr(_te, 'pk', None)` mistura `None`, templates não salvos e dublês sem PK; o primeiro resultado prevalece. Extraí e executei exatamente `_cct_de` por AST, somente em memória, com fábrica stub: `[NS(pk=None, tipo_ciclo='12x36'), NS(pk=None, tipo_ciclo='24x48')]` devolveu o resultado de `12x36` duas vezes e chamou a fábrica uma vez. `[None, NS(pk=None, tipo_ciclo='12x36')]` também confluiu; PKs diferentes `1,2` ficaram separados. Referências: `app/ponto/calculador/alimentacao.py:389`, `app/ponto/calculador/alimentacao.py:390`, `app/ponto/calculador/alimentacao.py:403`, `app/ponto/calculador/alimentacao.py:405`.

3. **Limite da colisão:** não há evidência de dois templates persistidos distintos compartilharem a chave no caminho normal. A lista vem do ORM com `select_related('tipo_escala')`; `EscalaColaborador.tipo_escala` é FK obrigatório; `TipoEscala` é `models.Model` e a aplicação usa PK automático `BigAutoField`. Objetos com mesmo PK e atributos divergentes também confluirão, mas o caminho atual produz esses snapshots divergentes é **[não verificado]**. Referências: `app/ponto/calculador/alimentacao.py:419`, `app/ponto/calculador/alimentacao.py:420`, `app/escala/models.py:874`, `app/escala/models.py:24`, `app/escala/apps.py:5`.

4. **Dias sem vínculo continuam com o template do chamador.** `_te_dia` começa como `te`; só muda quando o juiz retorna vínculo com template. Quando retorna `None`, `_cct_de` recebe o mesmo `te` usado pela implementação anterior, com o mesmo `c, ini`. Portanto o diff não altera logicamente esses dias sob cadastro estável; continua o fallback declarado, inclusive quando a célula nomeia geradora fora de vigência e o juiz recusa o vínculo. Referências: `app/ponto/calculador/alimentacao.py:487`, `app/ponto/calculador/alimentacao.py:491`, `app/ponto/calculador/alimentacao.py:494`, `app/ponto/calculador/alimentacao.py:507`, `app/escala/alimentacao.py:250`.

5. **O novo teste não cobre dia efetivamente sem vínculo nem colisão.** Os dois dias trabalhados estão cobertos pelos vínculos criados e os templates são salvos com PKs diferentes; a janela inclui dias descobertos, mas o resultado falso só contém os dois períodos cobertos. Referências: `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:66`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:70`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:74`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:78`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:82`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:87`.

6. **Chamadores confirmados:** fechamento passa o template da última fatia; comando de comparação usa `getattr(aut.esc, 'tipo_escala', None)`. Referências: `app/ponto/services/fechamento.py:1229`, `app/ponto/services/fechamento.py:1241`, `app/ponto/management/commands/diff_calculador.py:163`, `app/ponto/management/commands/diff_calculador.py:419`.

## 31/10 11:08 · sessao 7d440cefcf34 · - · - · origem=Ronald (tmux)

Achados confirmados por leitura:

- **Ciclo do dia com CCT da data errada:** `_cct_de` chama `get_motor_cct(c, ini, tipo_escala=_te)`; a praça vem do vínculo de `ini`, e a vigência da convenção também usa `ini`. Portanto pode combinar ciclo do dia com praça/convenção do início da janela. A data fixa já existia, e o diff mantém o defeito ao prometer CCT do dia. `app/ponto/calculador/alimentacao.py:392`; `app/core/regua_cct.py:329`; `app/core/regua_cct.py:330`; `app/core/regua_cct.py:366`; `app/core/regua_cct.py:261`.

- **Regressão concreta possível no intermitente:** para chamador 12x36 e dia de vínculo intermitente, o diff muda `feriado_em_dobra=False` para `True`: a lista de ciclos simples só contém 12x36. Entretanto `MotorIntermitente` zera seu calendário. A alimentação descarta `CCT.feriados` e passa o calendário externo; `do_dia` deixa intermitente seguir e paga a dobra. O resultado pode passar de zero à jornada inteira onde o motor intermitente não reconhece feriados. `app/ponto/calculador/alimentacao.py:507`; `app/ponto/calculador/alimentacao.py:524`; `app/ponto/motor_calculo_v2.py:3537`; `app/ponto/motor_calculo_v2.py:3608`; `app/ponto/motor_calculo_v2.py:3495`; `app/ponto/calculador/regras.py:736`; `app/ponto/calculador/regras.py:775`; `app/ponto/calculador/regras.py:785`.

- **Fonte paralela mantida:** a alimentação fabrica outro motor, enquanto o fechamento já criou o motor efetivo com `fatia_ini` e calendário, calculou a fatia e extraiu seu carimbo. A fronteira documenta leitura do motor que rodou; a releitura por período efetivamente usa `cct.feriados`, diferentemente de `rubricas_por_dia`. `app/ponto/calculador/alimentacao.py:392`; `app/ponto/services/fechamento.py:711`; `app/ponto/services/fechamento.py:713`; `app/ponto/services/fechamento.py:263`; `app/ponto/services/fechamento.py:270`; `app/ponto/calculador/nucleo.py:321`; `app/ponto/calculador/periodo.py:567`; `app/ponto/calculador/periodo.py:830`.

Mapa de autoridade:

- Os atributos fonte são escritos no motor em `app/ponto/motor_calculo_v2.py:712`, `:719`, `:737`, `:740`; os kwargs de CCT são montados em `app/core/regua_cct.py:454`, `:466`, `:470`, `:478`. O leitor `cct_do_motor` os converte em `CCT` em `app/ponto/calculador/nucleo.py:344`.
- `motivo_nao_julga_pontualidade` lê a tabela por identidade da classe; `None` devolve `None`. A tabela nomeia separadamente motor 12x36 cru e intermitente. `app/ponto/calculador/regras.py:487`; `:494`; `:516`; `:518`.
- Outro leitor é `chamados/juizes.py::folha_julga_pontualidade`: fabrica motor e pergunta ao mesmo leitor; tem `data or localdate()` preexistente. Essa função não participa do caminho novo. `app/chamados/juizes.py:1530`; `:1545`; `:1551`; `app/ponto/calculador/alimentacao.py:403`.
- O diff continua delegando vínculo e política às autoridades existentes: vínculo em `app/ponto/calculador/alimentacao.py:430`, dobra na fábrica em `app/ponto/motor_calculo_v2.py:3608`, cuja constante está registrada em `app/core/juizes.py:257`. Não há literal de ciclo novo no helper nem escrita nova de atributo de motor; são cache local e variáveis locais. `app/ponto/calculador/alimentacao.py:387`; `:403`; `:507`.
- O caminho transitivo `regua_para → vinculo_do_dia_carregado → vinculos_do_dia_carregado` carrega pela data explícita, sem consultar relógio. `app/core/regua_cct.py:329`; `app/escala/alimentacao.py:284`; `:298`; `:300`; `:301`.

## 31/10 10:55 · sessao 9839af8cbf61 · V3-RETOMA · leitura · origem=Code

**Não: o `motor_h` do diff não reproduz a partição final do fechamento.** Para folga, soma todo `periodos_ft`; para HE100 feriado, não preenche a rubrica e lê zero na chave ausente. O fechamento filtra escala certa, separa feriado e devolve o restante a trabalhadas. (`app/ponto/management/commands/diff_calculador.py:245–268`, `:535`; `app/ponto/services/fechamento.py:908–953`).

**(a) Origem das duas colunas**

O resultado v2 vem de `autoridade_do_periodo(c, ini, fim)`. A autoridade usa `apurar_por_fatia` quando há escalas e devolve o resultado em `Autoridade`. (`app/ponto/management/commands/diff_calculador.py:147`; `app/ponto/services/espelho.py:1166–1170`, `:1195`).

| Rubrica | Origem de `motor_h` |
|---|---|
| `horas_extras_100_feriado` | **Não há atribuição dessa chave em `m_dia`.** O loop lê `p.minutos_extra_100` para **`horas_extras_100`**, sem separar feriado. Depois, `a = float(mm.get(r) or 0)` transforma a chave ausente em **0**, nas linhas efetivamente comparadas. (`app/ponto/management/commands/diff_calculador.py:245–268`, `:535`) |
| `horas_folga_trabalhada` | Soma `(p.minutos_trabalhados or 0) / 60` de **todos** os `aut.resultado.periodos_ft` com entrada, agrupados por `_dia_do(p.entrada)` e dentro da janela. (`app/ponto/management/commands/diff_calculador.py:263–268`) |

No lado calculador, a trilha é:

1. Com `--pares-da-autoridade`, chama `insumos_do_calculador(c, aut.resultado, ...)`; essa função chama `segmentos_do_motor`, que lê conjuntamente `periodos` e `periodos_ft`, usa `resultado.dia_da_jornada` e as pontas efetivas dos períodos. (`app/ponto/management/commands/diff_calculador.py:287–313`; `app/ponto/calculador/alimentacao.py:302–305`, `:63–73`; `app/ponto/calculador/nucleo.py:393–400`).
2. `rubricas_por_dia` soma os pares pelo núcleo; o núcleo acumula `(saida - entrada).total_seconds() / 60` por dia. A alimentação entrega esses minutos a `regras.do_dia`, junto com escala certa, calendário e `folga_e_feriado`. (`app/ponto/calculador/alimentacao.py:437`, `:501–512`; `app/ponto/calculador/nucleo.py:134–155`).
3. As regras que produzem as duas rubricas são:

| Rubrica | Regra que produz `calculador_h` |
|---|---|
| `horas_extras_100_feriado`, no caminho normal | `do_dia` chama `dobra_de_feriado_do_dia`; havendo dobra, escreve `_dobra / 60`. A função devolve todo o trabalhado quando `_feriado_dobra_no_dia(dia)` é verdadeiro; esse método é emprestado de `MotorBase` e exige `feriado_em_dobra` e feriado. (`app/ponto/calculador/regras.py:287–295`, `:344–347`, `:774–785`; `app/ponto/motor_calculo_v2.py:879`) |
| `horas_extras_100_feriado`, no caminho de folga | Com folga, escala certa e `folga_e_feriado`, escreve `_certa / 60` em HE100 e HE100 feriado. (`app/ponto/calculador/regras.py:646–664`, `:675–683`) |
| `horas_folga_trabalhada` | `folga_trabalhada_do_dia` divide o trabalhado entre certa e sem escala certa. `do_dia` escreve `_certa / 60` em folga somente quando essa parcela não foi destinada ao feriado. (`app/ponto/calculador/regras.py:386–395`, `:660–690`) |

O comando coloca `_r_dia.valores` em `c_dia`; lê `a` de `m_dia` e `b` de `c_dia`; publica `round(a, 2)` e `round(b, 2)` nas colunas `motor_h` e `calculador_h`. Rubricas presentes em `nao_decide` são puladas. (`app/ponto/management/commands/diff_calculador.py:501`, `:517`, `:527–542`, `:655–658`).

**Condição do comando:** sem `--pares-da-autoridade`, `_ins_porta=None`, `_rub_do_dia={}` e os dias sem `_r_dia` são pulados antes de preencher `c_dia`; portanto essa execução não percorre as regras acima, e a consulta posterior ao calculador ausente também usa zero. (`app/ponto/management/commands/diff_calculador.py:325–337`, `:417–424`, `:469–472`, `:535`).

**(b) Passos do fechamento e reprodução no diff**

O fechamento compõe `periodos` e `periodos_ft` das fatias de vínculo antes de calcular os campos mensais. (`app/ponto/services/fechamento.py:96–97`, `:270–281`).

| Passo do fechamento v2 | Lado motor do diff | Lado calculador v3, pela trilha acima |
|---|---|---|
| **1. Base de HE100 feriado:** inicializa `horas_extras_100_feriado` com `round(resultado.horas_extra_100_feriado, 2)`. Essa property soma o subset feriado exclusivamente em `resultado.periodos`. (`app/ponto/services/fechamento.py:898`; `app/ponto/motor_calculo_v2.py:280–282`) | **Não reproduz:** não lê o subset nem preenche a rubrica. (`app/ponto/management/commands/diff_calculador.py:245–268`) | Calcula a dobra do dia e a identifica em campo próprio, em vez de copiar a property v2. (`app/ponto/calculador/regras.py:774–785`) |
| **2. Escala certa:** parte de todo `periodos_ft` e mantém em `_ft_certa` somente quem passa em `_certa(colab.id, data_local_da_entrada, cache)`. (`app/ponto/services/fechamento.py:903–910`) | **Não reproduz esse filtro final:** soma todos os `periodos_ft`. (`app/ponto/management/commands/diff_calculador.py:263–268`) | **Reproduz a decisão certa/sem certa:** recebe `_certa(c.pk, k, cache)` e divide os minutos nos dois retornos de `folga_trabalhada_do_dia`. (`app/ponto/calculador/alimentacao.py:511`; `app/ponto/calculador/regras.py:395`, `:660–664`) |
| **3. `feriado_vira_folga`:** somente dentro de `_ft_certa`, resolve o vínculo; se `feriado_vira_folga` for verdadeiro, move para `_ft_feriado`, retira de `_ft_certa` por identidade e acrescenta seus minutos/60 à base de HE100 feriado. (`app/ponto/services/fechamento.py:919–932`) | **Não reproduz:** esses minutos continuam somados como folga; não são transferidos para HE100 feriado. (`app/ponto/management/commands/diff_calculador.py:263–268`) | **Reproduz a transferência**, com a condição recebida `folga_e_feriado`: mantém folga zero e escreve `_certa/60` em HE100 feriado. (`app/ponto/calculador/alimentacao.py:508`; `app/ponto/calculador/regras.py:675–686`) |
| **4. Folga final:** grava `round(sum(minutos_trabalhados de _ft_certa)/60, 2)`, após retirar os feriados. (`app/ponto/services/fechamento.py:926–934`) | **Não reproduz a seleção:** sua soma continua sendo a totalidade de `periodos_ft`. (`app/ponto/management/commands/diff_calculador.py:263–268`) | Escreve folga apenas no ramo `_certa` que não caiu em `_hora_feriado`. (`app/ponto/calculador/regras.py:680–686`) |
| **5. Sem escala certa:** calcula o complemento por identidade, excluindo **tanto** `_ft_certa` **quanto** `_ft_feriado`; soma os minutos restantes a `horas_trabalhadas`. (`app/ponto/services/fechamento.py:948–953`) | **Não reproduz:** mantém esses minutos em folga; trabalhadas é somada somente do loop de `periodos`. (`app/ponto/management/commands/diff_calculador.py:247–253`, `:263–268`) | **Reproduz o destino em trabalhadas:** `_sem/60` vai para `horas_trabalhadas`, e folga fica zero. Nesse ramo, HE100 feriado é declarada em `nao_decide`, salvo a exceção `_hora_feriado`; não é afirmada como zero. (`app/ponto/calculador/regras.py:675–711`) |

O juiz de escala certa retorna falso para vínculo `LIMBO` ou assinaturas `A2`/`A8` na competência do dia. O predicado `feriado_vira_folga` exige vínculo que não trabalha em feriado, ciclo permitido e feriado conforme o lugar do colaborador. (`app/escala/services/escala_certa.py:16–51`; `app/escala/models.py:1357–1363`).

**A reprodução v3 não é literalmente idêntica em todos os insumos:** o fechamento consulta escala certa e feriado pela **data local de `p.entrada`**; o calculador usa **`k`, dia da jornada**, e sua alimentação exige adicionalmente `k in _feriados` para `folga_e_feriado`. (`app/ponto/services/fechamento.py:910`, `:922–924`; `app/ponto/calculador/alimentacao.py:63–73`, `:459–460`, `:508–512`).

Há também um bloqueio anterior no próprio motor que pode zerar o período e colapsar suas pontas efetivas; ambos os lados recebem esses efeitos. Isso não executa a partição final acima. (`app/ponto/motor_calculo_v2.py:2396–2399`, `:1409–1421`; `app/ponto/management/commands/diff_calculador.py:263–268`; `app/ponto/calculador/alimentacao.py:73`, `:102–111`).

Os valores mensais assim montados são persistidos por `fechamento.save()` no status aberto, ou por `QuerySet.update`, que inclui explicitamente as duas rubricas. (`app/ponto/services/fechamento.py:1096–1112`, `:1128–1138`).

Mesmo `--periodos-do-fechamento` substitui apenas `aut.resultado`; o somatório posterior continua lendo todos os `periodos_ft`, sem consumir `_ft_certa`, `_ft_feriado` ou `_ft_sem_escala`. (`app/ponto/management/commands/diff_calculador.py:154–162`, `:263–268`).

**(c) Campo feriado existente e omitido pelo diff**

**Sim:**

- `PeriodoCalculo.minutos_extra_100_feriado`, declarado como subset de `minutos_extra_100`. (`app/ponto/motor_calculo_v2.py:168–169`).
- `ResultadoMes.horas_extra_100_feriado`, que soma esse campo em `self.periodos` e divide por 60; existe também `horas_extra_100_normal`, calculada pela diferença entre HE100 total e feriado. (`app/ponto/motor_calculo_v2.py:278–286`).

Os escritores da dobra preenchem o subset com toda a duração ou todo o trabalhado do dia. O diff lê apenas `minutos_extra_100` para HE100 total e omite o subset feriado. (`app/ponto/motor_calculo_v2.py:2747–2750`, `:3030–3033`, `:3193–3196`, `:3275–3279`; `app/ponto/management/commands/diff_calculador.py:255–256`).

Ler essa property recuperaria a **base** de feriado dos períodos normais; o acréscimo vindo de `_ft_feriado` é calculado separadamente pelo fechamento. (`app/ponto/motor_calculo_v2.py:280–282`; `app/ponto/services/fechamento.py:929–930`).

_966 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
