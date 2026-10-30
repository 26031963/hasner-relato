# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 16:43** · publicado em **30/10/2026 16:46** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 16:43 · sessao 01c938693ab5 · CALENDARIO-HOJE · verificador · origem=Code

**REPROVADO.** O caso cego recebe `sem_certidao` e um rótulo com `sem_celula`, mas esse rótulo não chega à tela: falta o ramo correspondente no template, que exibe `—`. O teste valida somente o dicionário. (`app/colaboradores/services/calendario.py:899`; `app/templates/colaboradores/partials/_calendario_grade.html:99`; `app/templates/colaboradores/partials/_calendario_grade.html:107`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:99`)

Análise estática; execução dos testes, RED histórico, GREEN e contagens efetivas de queries: **[nao verificado]**. [nao verificado]

**1. Dia passado, de trabalho, sem batida/veredito, com `sem_previsto`**

Ele sai sempre de `_em_curso`; portanto, nessas premissas, `futuro=False`. A árvore abaixo cobre as combinações: vale a primeira condição aplicável. (`app/colaboradores/services/calendario.py:714`; `app/colaboradores/services/calendario.py:822`)

| Primeira condição aplicável | Status produzido |
|---|---|
| Ausência na precedência A2 | `falta` se desconta; senão `ausencia`. (`app/colaboradores/services/calendario.py:124`) |
| Feriado que suprimiu o trabalho | `folga`. (`app/colaboradores/services/calendario.py:127`) |
| Folga lançada humana | `folga`. (`app/colaboradores/services/calendario.py:131`; `app/colaboradores/services/calendario.py:149`) |
| Isento | `ok`. (`app/colaboradores/services/calendario.py:137`) |
| Régua suspeita | `escala_suspeita`. (`app/colaboradores/services/calendario.py:139`) |
| Folha respondeu `sem_celula` | `sem_certidao`. (`app/colaboradores/services/calendario.py:141`) |
| Sem retenção; cobertura não descontante | `ausencia`. (`app/colaboradores/services/calendario.py:148`) |
| Sem retenção; sem cobertura ou cobertura descontante | `falta`. (`app/colaboradores/services/calendario.py:148`) |

Feriado não suprime trabalho quando a precedência reconhece `trabalha_em_feriado`; nesse caso, seguem os demais ramos. (`app/ponto/precedencia.py:314`; `app/ponto/precedencia.py:319`)

**Sim, existem respostas diferentes:** a tela pode produzir `folga`, `ausencia` ou `escala_suspeita`, enquanto a folha retém `sem_celula`. A folha retém célula passada sem veredito e com `trabalha != None`, sem consultar essas precedências nesse ramo. Isso não demonstra reincidência de A1: o classificador agora preserva expressamente essas prioridades. (`app/folha/export.py:1237`; `app/colaboradores/services/calendario.py:124`; `app/colaboradores/services/calendario.py:139`; `app/colaboradores/services/calendario.py:141`)

Quanto às exceções:

- **Isento:** a folha retorna sem retenção; a tela produz `ok` se nenhuma condição anterior vencer. (`app/folha/export.py:1174`; `app/colaboradores/services/calendario.py:137`)
- **Fora de admissão/demissão:** a folha recorta seu universo; a tela não aplica esse recorte contratual. Sua guarda é de vigência do vínculo e somente quando não há célula. Mantido `eh_trabalho=True`, pode produzir `falta` onde a folha não retém nada. (`app/folha/export.py:1180`; `app/folha/export.py:1182`; `app/colaboradores/services/calendario.py:782`; `app/colaboradores/services/calendario.py:148`)
- **Próprio dia da demissão:** sem veredito, a folha não retém; a tela segue a árvore acima, podendo produzir `falta`. (`app/folha/export.py:1211`; `app/folha/export.py:1237`; `app/colaboradores/services/calendario.py:148`)
- **Motivo diferente de `sem_celula`:** nas premissas consistentes de ausência de veredito, não ocorre no juiz atual. O outro motivo, `espelho_<veredito>`, exige veredito acusatório. O calendário aproveita somente `sem_celula`. (`app/folha/export.py:1215`; `app/folha/export.py:1236`; `app/colaboradores/services/calendario.py:727`)

Cobertura não é uma prioridade independente acima de tudo: a ausência pode vencer já pela A2; no ramo residual, retenção vem antes da cobertura. (`app/ponto/precedencia.py:317`; `app/colaboradores/services/calendario.py:141`; `app/colaboradores/services/calendario.py:148`)

**2. Quem escreve e quem lê**

| Campo/valor | Escritor e leitores internos |
|---|---|
| `status` | `_classificar` decide; montagem chama e publica. (`app/colaboradores/services/calendario.py:189`; `app/colaboradores/services/calendario.py:814`; `app/colaboradores/services/calendario.py:898`) |
| `status_rotulo` | Montagem escreve conforme status e conjuntos auxiliares. (`app/colaboradores/services/calendario.py:899`) |
| `retido_folha` | Montagem alimenta; `_classificar` lê. (`app/colaboradores/services/calendario.py:834`; `app/colaboradores/services/calendario.py:141`) |
| `_cego_folha` | Montagem preenche com a resposta filtrada da folha; alimenta classificação e rótulo. (`app/colaboradores/services/calendario.py:727`; `app/colaboradores/services/calendario.py:834`; `app/colaboradores/services/calendario.py:899`) |
| `_em_curso` | Montagem deriva dos motivos da juíza; alimenta `futuro` e rótulo. (`app/colaboradores/services/calendario.py:714`; `app/colaboradores/services/calendario.py:822`; `app/colaboradores/services/calendario.py:900`) |
| `futuro` | Montagem alimenta; classificador lê. (`app/colaboradores/services/calendario.py:822`; `app/colaboradores/services/calendario.py:135`; `app/colaboradores/services/calendario.py:155`) |

Os quatro últimos são auxiliares locais, não campos publicados no item de `dias`. (`app/colaboradores/services/calendario.py:893`)

As portas e os leitores relevantes são:

| Porta/leitor | Consumo e efeito |
|---|---|
| `status_do_dia` | Retorna o status do contexto; não decide separadamente. (`app/colaboradores/services/calendario.py:225`; `app/colaboradores/services/calendario.py:228`) |
| `contar_por_status` | Conta os status publicados pelo contexto. (`app/colaboradores/services/calendario.py:236`; `app/colaboradores/services/calendario.py:239`) |
| `resolver_dia_oferecido` | Lê status; tanto `falta` quanto `sem_certidao` pedem decisão. A troca entre eles preserva essa condição. (`app/colaboradores/services/calendario.py:196`; `app/colaboradores/services/calendario.py:204`) |
| `vigia_fio` | Consulta a porta curta e registra decisão pendente. Passado que muda de `falta` para `previsto` deixa de satisfazer essa condição. (`app/chamados/services/vigia_fio.py:294`; `app/colaboradores/services/calendario.py:196`) |
| Grade compartilhada | Lê status para cor e ícone; lê rótulo nos ramos conhecidos, mas não possui ramo `sem_certidao`. (`app/templates/colaboradores/partials/_calendario_grade.html:69`; `app/templates/colaboradores/partials/_calendario_grade.html:99`; `app/templates/colaboradores/partials/_calendario_grade.html:107`) |
| Mapa de furos | Monta o mesmo contexto por card. Seu `nfuro` continua vindo de `estado_do_dia`, não do status novo. (`app/relatorios/furos.py:160`; `app/relatorios/furos.py:164`; `app/relatorios/furos.py:184`) |
| PDF do mapa | Extrai status do calendário e converte em cores; `sem_certidao` não tem entrada nos mapas e recebe branco/cinza pelo fallback. (`app/relatorios/views.py:1550`; `app/relatorios/furos_pdf.py:11`; `app/relatorios/furos_pdf.py:21`; `app/relatorios/furos_pdf.py:56`) |

Chamadores de produção externos de `status_do_dia` e `contar_por_status`: não encontrados; exaustividade desse censo negativo **[nao verificado]**.

Há efeitos adicionais à mudança de status: o dia que passa de `falta` para `sem_certidao` sai de `resumo.faltas`, e não existe contador de `sem_certidao` nesse resumo. (`app/colaboradores/services/calendario.py:950`; `app/colaboradores/services/calendario.py:948`)

A perda do rótulo na grade, o fallback visual do PDF e essa saída do resumo não estão explicitados no corpo anexado do commit. **[nao verificado: comparação textual com anexo sem referência numerada]** [nao verificado]

**3. Hoje e passado ainda em curso**

**Hoje:** qualquer motivo não nulo da juíza entra em `_em_curso`, inclusive `sem_previsto`. Sem veredito, isso alimenta `futuro`; em dia de trabalho sem batida, o classificador produz `previsto`, ressalvadas ausência, feriado, folga humana e cobertura. Hoje já encerrado não recebe essa proteção. (`app/colaboradores/services/calendario.py:714`; `app/colaboradores/services/calendario.py:822`; `app/colaboradores/services/calendario.py:124`; `app/colaboradores/services/calendario.py:135`; `app/colaboradores/services/calendario.py:187`)

**Passado:** entram `relogio`, `turno_aberto_relogio` e `turno_aberto`; `sem_previsto` fica excluído. Com o vocabulário atual, não há outro motivo pelo qual um passado não encerrado vire `previsto`. Veredito existente impede usar `_em_curso` como `futuro`. (`app/ponto/turnos.py:2066`; `app/ponto/turnos.py:2070`; `app/colaboradores/services/calendario.py:714`; `app/colaboradores/services/calendario.py:822`)

Dia com batida não passa pelo ramo sem batida; turno aberto segue podendo produzir `aberto`. (`app/colaboradores/services/calendario.py:133`; `app/colaboradores/services/calendario.py:159`)

**4. Os dez testes mordem, mas não fecham todas as costuras** [nao verificado]

As avaliações abaixo são de alcance estático das asserções; execução dos mutantes **[nao verificado]**.

| Teste, abreviado | Mutação que pega | Mutação que deixa passar | [nao verificado]
|---|---|---|
| Hoje antes do turno | Voltar a `futuro = data > hoje`; retirar rótulo especial. | Regra local que imite o caso sem consultar a juíza. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:43`) |
| Encadeado além de ontem | Restringir proteção a hoje/ontem; ignorar os marcos encadeados. | Manter esse DNA previsto também depois de encerrado. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:48`) |
| Cruza meia-noite | Voltar ao teto apenas por data. | Dar `previsto` incondicionalmente a ontem. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:58`) |
| Depois do fim | Proteger também o turno já encerrado. | Erro exatamente na igualdade `agora == fim`. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:62`) |
| Cartório já julgou | Remover `and not _vv` de `futuro`. | Erros nos demais vereditos: só exercita `nunca_bateu`. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:66`) |
| Sem fim, sem veredito | Remover retenção do classificador; recolocar passado `sem_previsto` em curso. | Fabricar localmente `sem_certidao/sem_celula`; continuar sem renderizar o rótulo. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:92`) |
| Sem fim, com veredito | Restaurar o comportamento antigo completo que sobrepunha cegueira ao veredito. | Consultar folha desnecessariamente para dias julgados, preservando resultado. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:103`) |
| Folga lançada | Antepor retenção à folga humana. | Quebrar precedência de ausência ou feriado. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:114`) |
| Isento | Manter passado `sem_previsto` como em curso. | Quebrar isenção na própria folha ou o rótulo apresentado. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:123`) |
| Custo | Acrescentar queries proporcionais aos dias cegos nessa janela. | Acrescentar custo constante, repetir consultas por montagem ou criar N+1 por todos os dias da janela fixa. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:132`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:154`) |

**A3 está corrigido na construção do caso:** agora há vínculo, célula de trabalho, DNA sem fim e asserção explícita de `sem_previsto`; o RED histórico alegado continua **[nao verificado]**. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:29`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:82`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:96`)

**A5 está parcialmente atendido:** `q8-base == q1-base` equivale a `q8 == q1`. O teste verifica crescimento pelo número de dias cegos, mas não fixa custo absoluto por card nem sela “uma chamada à folha”. A chamada atual está efetivamente fora do laço; a nova pergunta a `dias_encerrados` também carrega vínculos em toda montagem, inclusive sem dia cego. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:153`; `app/colaboradores/services/calendario.py:726`; `app/colaboradores/services/calendario.py:729`; `app/ponto/turnos.py:2130`)

**5. Checklist das classes**

| Classe | Resultado |
|---|---|
| Segundo juiz duplicando autoridade | Não demonstrado: calendário consulta encerramento e retenção; o predicado temporal continua no puro compartilhado. (`app/colaboradores/services/calendario.py:712`; `app/colaboradores/services/calendario.py:726`; `app/ponto/turnos.py:2088`) |
| Segundo escritor fora da porta | A1 removido: status sai diretamente de `_classificar`, sem ternário posterior. (`app/colaboradores/services/calendario.py:814`; `app/colaboradores/services/calendario.py:898`) |
| Relógio fora da borda | A nova decisão usa o mesmo instante para hoje e encerramento. Ainda existe outra leitura herdada para desfazer retratação; não é leitura nova deste diff. (`app/colaboradores/services/calendario.py:258`; `app/colaboradores/services/calendario.py:343`; `app/colaboradores/services/calendario.py:712`) |
| Derivação paralela | Não demonstrada no novo status; há, porém, costura assimétrica: batch repassa `celulas` ao puro, porta singular não repassa para escolha de vínculo/fim. Equivalência com alimentação divergente do banco não está demonstrada. (`app/ponto/turnos.py:2079`; `app/ponto/turnos.py:2137`; `app/ponto/tests/test_c1_dias_encerrados.py:28`) |
| Selo/RED sem mordida | Os testes comportamentais mordem os casos indicados, mas deixam passar a falha real de apresentação e custos constantes adicionais. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:99`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:154`) |
| Ordem de trava | Alteração de ordem de locks/travas não demonstrada no diff fornecido; execução da esteira **[nao verificado]**. |

**Achados procedentes:**

- **Bloqueante: o rótulo soberano do dia cego não é apresentado.** O produtor o escreve; grade e PDF não o consomem nesse estado. O teste passa apesar disso. (`app/colaboradores/services/calendario.py:899`; `app/templates/colaboradores/partials/_calendario_grade.html:107`; `app/relatorios/furos_pdf.py:56`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:100`)
- **A5 permanece incompleto como prova:** o selo relativo não demonstra custo absoluto por card nem protege a quantidade de chamadas constantes. Não há aqui prova de lentidão real; essa magnitude é **[nao verificado]**. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:154`; `app/ponto/turnos.py:2130`)

## 30/10 16:41 · sessao 27c1a6390ae4 · - · - · origem=Ronald (tmux)

Leitura estática concluída; execução dos testes, RED histórico e GREEN de 764 testes ficam `[nao verificado]`. [nao verificado]

Os dez testes têm as seguintes mordidas e lacunas: [nao verificado]

| Teste, abreviado | Mutação que detecta | Mutação que deixa passar | [nao verificado]
|---|---|---|
| `hoje_antes_do_turno` | Retirar o “em curso” de `futuro`, voltando a `data > hoje`, ou retirar o rótulo especial: exige `previsto` e rótulo exato. | Derivação própria que imite esse caso, sem consultar `dias_encerrados`; comportamento de hoje depois do encerramento. Referências: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:42`, `app/colaboradores/services/calendario.py:822`. |
| `turno_encadeado_alem_de_ontem` | Limitar a pergunta a hoje/ontem ou ignorar os quatro marcos congelados: o dia 14 ainda deve sair previsto no dia 16. | Rotular arbitrariamente esse dia como previsto; não afirma o retorno da juíza nem o encerramento posterior desse mesmo DNA. Referências: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:47`, `app/escala/servico_jornada.py:75`. |
| `cruza_a_meia_noite` | Voltar ao futuro decidido apenas por data. | Dar previsto incondicionalmente a ontem; não verifica rótulo. Referência: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:57`. |
| `depois_do_fim` | Tornar previsto também o dia noturno já encerrado. | Classificar todo dia passado como falta sem consultar o relógio/juíza; não testa igualdade exata com o instante final. Referências: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:61`, `app/ponto/turnos.py:2105`. |
| `cartorio_ja_julgou` | Retirar `and not _vv` da alimentação de `futuro`: `nunca_bateu` deve vencer o turno ainda não encerrado. | Erros nos demais vereditos e nas precedências de ausência/feriado; verifica só `nunca_bateu`. Referências: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:65`, `app/colaboradores/services/calendario.py:822`. |
| `sem_fim_sem_veredito` | Retirar a retenção do classificador ou restaurar o passado `sem_previsto` em curso: exige trabalho verdadeiro, folha `sem_celula`, status `sem_certidao` e motivo no rótulo. | Substituir a consulta à folha por uma regra local “sem fim ⇒ sem_certidao/sem_celula”; a consulta direta à folha é uma asserção independente, sem prova de ligação entre autoridades. Referências: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:91`, `app/colaboradores/services/calendario.py:726`. |
| `sem_fim_com_veredito` | Restaurar o escritor ternário antigo com inclusão dos dias julgados: exige falta e proíbe rótulo `sem_celula`. | Consultar desnecessariamente a folha também para dias julgados, desde que filtre o motivo e preserve o resultado; não mede queries nesse caso. Referências: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:102`, `app/colaboradores/services/calendario.py:723`. |
| `folga_lancada` | Antepor retenção à folga humana, inclusive recolocar o ternário escritor. | Divergir da folha nesse cenário: o teste exige somente tela `folga`, sem consultar/assertar a retenção da folha. Referência: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:113`. |
| `isento` | Manter passado `sem_previsto` em `_em_curso`: daria previsto em vez de ok. | Erros na própria exclusão de isento pela folha ou rótulo incorreto: só afirma `status == ok`. Referência: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:122`. |
| `custo` | Query adicional por cada dia sem fim: compara oito contra um, na mesma janela, exigindo também as quantidades de `sem_certidao`. | N+1 pago em todos os oito dias da janela, duas ou mais consultas constantes por montagem, aumento fixo enorme ou custo crescente com comprimento da janela; não afirma teto absoluto nem quantidade de chamadas à folha. Referência: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:131`. |

A5 foi parcialmente tratada: `q8 - base == q1 - base` equivale a `q8 == q1`; mede dependência do número de dias cegos numa janela fixa, sem publicar contagem absoluta. A chamada à folha está fora do laço e ocorre uma vez no código atual; `dias_encerrados` ainda faz uma consulta incondicional de vínculos por montagem. Referências: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:148`, `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:153`, `app/colaboradores/services/calendario.py:724`, `app/ponto/turnos.py:2130`.

A3 tem agora fixture pertinente: vínculo criado, célula de trabalho e DNA com início sem fim; há asserção explícita `MOTIVO_SEM_PREVISTO` antes de conferir a tela. O RED histórico alegado continua `[nao verificado]`. Referências: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:29`, `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:82`.

Nenhum dos dez testes cobre ausência, feriado, cobertura, régua suspeita, admissão/demissão, motivos de retenção diferentes de `sem_celula`, turno aberto com batida, nem igualdade do relógio com o fim; a fixture contém apenas os cenários explicitados acima. Referências: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:19`, `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:42`, `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:154`.

Sobre os auxiliares de produção:

- `fim_turno` apenas propaga a alimentação `celulas` a `marcos_turno`; a montagem crescente dos marcos e a exigência de início e fim permanecem no mesmo sítio. Referências: `app/escala/servico_jornada.py:69`, `app/escala/servico_jornada.py:72`, `app/escala/servico_jornada.py:80`, `app/escala/servico_jornada.py:89`.
- O batch passou a propagar `celulas` até o fim previsto. A porta singular `dia_encerrado_motivo`, apesar de receber `celulas`, continua passando o argumento apenas a `turnos_do_colab`, deixando `escala_vigente` e o puro recarregarem do banco. Isso impede assegurar equivalência para alimentação em memória diferente do banco ou para `{}`; o contrato dos modelos atribui significado à chave ausente. Referências: `app/ponto/turnos.py:2079`, `app/ponto/turnos.py:2102`, `app/ponto/turnos.py:2137`, `app/escala/models.py:1082`.
- O selo antigo de equivalência compara apenas chamadas sem alimentação e sem os novos parâmetros `turnos`/`motivos`; não protege essa nova costura. Referências: `app/ponto/tests/test_c1_dias_encerrados.py:26`, `app/ponto/tests/test_c1_dias_encerrados.py:32`.
- `_dia_encerrado_puro` virou wrapper sem chamador: o batch agora chama diretamente `_dia_encerrado_motivo_puro`. Não duplica o predicado, mas deixou função morta cujo comentário ainda diz que o batch a lê. Referências: `app/ponto/turnos.py:2083`, `app/ponto/turnos.py:2137`. A regra de zero chamador está em `CLAUDE.md:341`.
- A borda nova usa um único instante para `hoje` e encerramento, porém o contexto ainda possui outra leitura herdada do relógio em `_agora_retr`; a afirmação “borda é uma” não vale para todo o contexto. Referências: `app/colaboradores/services/calendario.py:258`, `app/colaboradores/services/calendario.py:343`, `app/colaboradores/services/calendario.py:712`.

## 30/10 16:41 · sessao 4ff79538d4b8 · - · - · origem=Ronald (tmux)

Q1 pode ser respondida por esta árvore de precedência, que cobre as combinações sem listar o produto cartesiano. Assuma dia passado, `eh_trabalho=True`, sem batidas e sem veredito; `sem_previsto` nunca entra em `_em_curso`, logo `futuro=False` nesse caso. (`app/colaboradores/services/calendario.py:714`, `app/colaboradores/services/calendario.py:822`)

| Primeiro fato aplicável | Status da tela | Folha |
|---|---|---|
| Faixa de ausência | `falta` se efeito desconta; senão `ausencia` | Pode reter `sem_celula` |
| Feriado que efetivamente suprimiu o trabalho | `folga` | Pode reter `sem_celula` |
| Folga humana lançada | `folga` | Pode reter `sem_celula` |
| Isento | `ok` | Não retém |
| Régua suspeita | `escala_suspeita` | Pode reter `sem_celula` |
| Folha respondeu `sem_celula` | `sem_certidao`, rótulo contendo `sem_celula` | Retém `sem_celula` |
| Sem retenção; cobertura não descontante | `ausencia` | Não retém |
| Sem retenção; sem cobertura ou cobertura descontante | `falta` | Não retém |

Referências da árvore: ausência `calendario.py:124`; feriado `:127`; folga humana `:131`/`:149`; isento `:137`; régua `:139`; retenção `:141`; cobertura `:148`; rótulo `:899`. A folha isenta Art.62 em `app/folha/export.py:1174` e retém célula passada sem veredito e `trabalha != None` em `:1237`. A folha não consulta ausência, feriado, folga, cobertura ou régua nesse ramo. (`app/folha/export.py:1206`)

Assim, **sim**, há tela `folga`/`ausencia`/`escala_suspeita` com folha `sem_celula`. As diferenças de A2 não constituem por si uma nova reprovação: o achado A1 fornecido exigiu preservar essa precedência; a ordem da régua antes da retenção também consta explicitamente do corpo do commit fornecido. (`app/colaboradores/services/calendario.py:124`, `:139`, `:141`; corpo de `diff5_delta.txt` fornecido, sem numeração `[nao verificado]`)

A cobertura fica depois da retenção, mas normalmente uma ausência coberta já aparece na faixa A2: ambos consultam ausências não rejeitadas e expandem o período. Portanto não afirmaria, sem caso concreto, mudança adicional normal causada por essa ordem. (`app/ponto/precedencia.py:211`, `:317`; `app/ponto/turnos.py:2489`, `:2734`, `:2750`)

**Admissão/demissão:** a folha recorta o universo e pode retornar vazio; no próprio dia da demissão, célula sem veredito não retém. A tela não faz esse recorte contratual: sua guarda verifica vigência do vínculo apenas quando não há célula. Mantido `eh_trabalho=True`, esses dias seguem os mesmos ramos — podendo resultar em `falta`, enquanto a folha nada retém. (`app/folha/export.py:1180`, `:1211`, `:1237`; `app/colaboradores/services/calendario.py:782`, `:785`, `:148`)

**Motivo diferente de `sem_celula`:** sob as premissas consistentes de ausência de veredito, não existe esse caso no juiz atual. O outro motivo é `espelho_<veredito>`, que exige `VEREDITOS_ACUSAM`. O calendário filtra expressamente apenas `sem_celula`. (`app/folha/export.py:1215`, `:1236`; `app/colaboradores/services/calendario.py:727`)

**Q3:** os únicos motivos atualmente possíveis são `relogio`, `turno_aberto_relogio`, `turno_aberto`, `sem_previsto` ou `None`. Hoje aceita qualquer motivo não nulo em `_em_curso`; passado aceita todos exceto `sem_previsto`. Portanto não há passado `previsto` por outro motivo atual além de relógio/turno aberto, e a existência de veredito impede usar `_em_curso` como `futuro`. (`app/ponto/turnos.py:2073`, `:2097`; `app/colaboradores/services/calendario.py:714`, `:822`)

Não executei testes nem consultei banco/log; essas conclusões são estáticas. `[nao verificado]` [nao verificado]

## 30/10 16:41 · sessao 4b8fe8905497 · - · - · origem=Ronald (tmux)

- **Escritor de `status`:** `_classificar` retorna o valor em `app/colaboradores/services/calendario.py:189`; a montagem chama exclusivamente essa porta em `:814` e publica em `:898`. **Escritor de `status_rotulo`:** montagem em `:899-901`.
- **`retido_folha`:** montagem alimenta em `calendario.py:834`; `_classificar` lê em `:141`. **`_cego_folha`:** inicialização `:721`, preenchimento `:727`, leituras `:834,:899`. **`_em_curso`:** preenchimento `:714-715`, leituras `:822,:900`. **`futuro`:** alimentação `:822`, leituras `:135,:155`. Esses quatro últimos fatos permanecem locais: o dict publicado de dia não os contém (`calendario.py:893-929`). [nao verificado]
- **Portas curtas:** `status_do_dia` monta contexto e lê o primeiro status (`calendario.py:225-228`); `contar_por_status` monta contexto e conta status (`:236-239`). Não encontrei chamadores de produção fora do módulo; censo negativo não possui linha própria `[nao verificado]`. O registro de `status_do_dia` é `app/core/juizes.py:809`.
- **Resolver/vigia:** `STATUS_QUE_PEDEM_DECISAO` inclui tanto `falta` quanto `sem_certidao` (`calendario.py:196`); o juiz lê status em `:204-205`, o contexto publica sua resposta em `:945`; a porta curta lê esse campo em `:216`; `vigia_fio` pergunta e classifica decisão pendente em `app/chamados/services/vigia_fio.py:294-300`. A troca específica `falta → sem_certidao` conserva elegibilidade; a troca `falta → previsto` para passado ainda em curso elimina elegibilidade (`calendario.py:196-205,:714-715,:822`).
- **Templates compartilhados:** contexto chega pelo perfil (`app/colaboradores/views.py:1946,:1962`), drawer (`app/colaboradores/services/drawer.py:121`; `app/templates/colaboradores/partials/_drawer_diagnostico.html:35`) e mapa (`app/relatorios/furos.py:184-188`; `app/templates/relatorios/furos.html:42`). Cor lê `status` em `app/templates/colaboradores/partials/_calendario_grade.html:69-78`; ícone e títulos leem status/rótulo em `:99-106`; Resolver lê campo soberano em `:125`.

**Achado procedente: o novo rótulo cego não chega à tela.** O produtor escreve `"Dia sem julgamento ... sem_celula"` (`calendario.py:899`), porém o template não possui ramo `sem_certidao`: cai no cinza (`_calendario_grade.html:78`) e no travessão sem título (`:107`). A precedência da palavra também ocorre antes dos ícones (`:97-98`). Assim o teste que procura `sem_celula` no dict (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:99-100`) não garante que a tela leia o rótulo.

- **PDF:** `app/relatorios/views.py:1550-1551` extrai os status do calendário; `app/relatorios/furos.py:194-207` os embala nas semanas; `app/relatorios/furos_pdf.py:53-58` traduz estado em cor. `sem_certidao` não existe nos mapas (`furos_pdf.py:11-20`): aparece branco/cinza pelo fallback (`:21,:56-57`), sem rótulo ou motivo.
- **Mapa de furos:** os cards e contagens continuam saindo de outro dado já existente: `grade_da_celula` (`furos.py:141`), `estado_do_dia` (`:160`), `nfuro` (`:164`) e ordenação (`:190`). O novo status altera o calendário dentro do card, não o universo nem `nfuro` (`:165-188`).
- **Homônimo sem efeito desta montagem:** `app/templates/escala/minha_escala.html:75,:84` lê `status_rotulo`, mas seu produtor é `app/escala/views.py:49-55`, construído diretamente de `ROTULO_STATUS`.
- **Rodapé:** a mudança de `falta` para `sem_certidao` retira o dia de `resumo.faltas` (`calendario.py:950`), sem contador `sem_certidao` no resumo (`:948-966`); o template imprime “Sem registro” de `resumo.faltas` (`_calendario_grade.html:145-147`). Efeito concreto omitido da descrição específica do delta fornecido `[nao verificado: corpo do commit só disponível no anexo]`. [nao verificado]
- **Histórico:** esta cópia não possui repositório Git acessível; comparação base/head por objetos Git não foi possível `[nao verificado]`.

## 30/10 15:47 · sessao 821fe87eef02 · O261-VETO-A2 · sonda · origem=Code

Não executei a sonda: não localizei `sonda_fsc.py` na cópia. Segue a saída integral fornecida no enunciado. [nao verificado] [nao verificado]

```text
sonda_leitura: sonda_fsc.py [fe36a5107d6c7951] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

TOTAL=68
POR_PORTA|pergunta_ao_colab=48
POR_PORTA|supervisao_sem_push=20
POR_PORTA|veto_celula=1
POR_EMPRESA|2=68
ITENS=69
FIM-DA-SONDA
```

## 30/10 15:05 · sessao be63b35eee76 · BENCH-CORRECAO · censo · origem=Code

A aplicabilidade de alguns itens não pode ser determinada: os anexos não trazem as declarações LEI-AKITA de cada hash nem informam quando s1 e s2 foram corrigidos. Nesses casos, usei `[nao verificado]`; usar `n/a` afirmaria uma ausência não demonstrada. As referências abaixo são as citadas pelos revisores; sua correspondência com os hashes históricos é [nao verificado].

Considerei `n/a` para s3 antes da guarda de reabertura e, em `a3bbef39`, para os defeitos corrigidos pelos deltas descritos: 3, 4, 5 e s3. (`app/chamados/models.py:1143`; `app/ponto/tests/test_veto_que_cai_rejulga.py:124`; `app/chamados/tests/_vinculo_do_dia.py:18`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:184`; referências históricas: [nao verificado])

Estes trechos sustentam os reconhecimentos integrais ou parciais; as demais classificações indicam ausência de identificação do defeito nas respostas fornecidas [nao verificado].

| Volta / item | A | B |
|---|---|---|
| 1c565c8f / 2 | **EM PARTE:** identifica o leitor omitido: “leitor de desfazer corroboração filtra pelo vínculo específico”, mas não denuncia a declaração desatualizada. (`app/escala/management/commands/desfazer_corroboracao_regua.py:38`) | **EM PARTE:** “desfazer corroboração filtra pelo vínculo”, sem apontar o defeito da declaração. (`app/escala/management/commands/desfazer_corroboracao_regua.py:38`) |
| 1c565c8f / 3 | **EM PARTE:** “Acrescenta o veto à impressão”, tratando o efeito de `ec=None`, sem apontar a falta do teste próprio. (`app/ponto/services/cartorio.py:1013`, `:1083`) | **EM PARTE:** “Acrescenta `veto:VETO_DEGENERADA` à impressão do ramo normal”, sem identificar a lacuna de teste. (`app/ponto/services/cartorio.py:1013`, `:1083`) |
| 1c565c8f / 5 | **EM PARTE:** “no apply distingue retorno recusado”, sem denunciar a classificação por eliminação. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:174`) | **NÃO VIU:** não identifica o contador por eliminação [nao verificado]. |
| 1c565c8f / 6 | **EM PARTE:** “Reversão anterior, DIFF financeiro, preservação de exportadas por hash [...] [nao verificado]”; não identifica “63 × 1.576” nem exige expressamente `medir` antes/depois. (`app/ponto/services/cartorio.py:1083`; `app/ponto/management/commands/processar_cartorio.py:49`) | **EM PARTE:** “Reversão anterior, preservação por hash e diferença à Pauta DP: [nao verificado]”; faltam os outros componentes do item. (`app/ponto/management/commands/processar_cartorio.py:49`; `app/ponto/portas/regen_exportada.py:62`) |
| 1c565c8f / 7 | **VIU:** “Ato humano passa sem registrar o veto dispensado”; “falta a informação de que o humano atravessou um veto”. (`app/chamados/views_arquivar.py:68`, `:75`, `:79`; `app/ponto/services/furos_vetados.py:90`) | **NÃO VIU:** não aponta a ausência de registro do veto atravessado [nao verificado]. |
| 673ecaf7 / 6 | **EM PARTE:** “Item 9 continua pendente de pouso”; “Quantidade afetada e resultado da medida anunciada: [nao verificado]”. (`app/config/crons.py:597`; `app/ponto/management/commands/processar_cartorio.py:49`; `app/ponto/services/cartorio.py:1083`) | **EM PARTE:** “continua sem resultado da medição exigida no anexo”, sem cobrir todas as condições do gabarito. (`app/ponto/management/commands/processar_cartorio.py:49`; `app/ponto/services/cartorio.py:1083`) |
| a3bbef39 / 1 | **VIU:** “RED do nascimento contra a base — Cura não demonstrada”; existência dos testes não comprova o RED histórico. (`app/chamados/tests/test_o261_veto_na_porta.py:418`, `:522`; histórico: [nao verificado]) | **VIU:** “RED contra a base — Cura não comprovada”; “sua existência não prova aquela execução”. (`app/chamados/tests/test_o261_veto_na_porta.py:418`, `:522`; histórico: [nao verificado]) |
| a3bbef39 / 2 | **VIU:** nomeia a declaração desatualizada, as origens e o consumidor omitido; “A linha corrigida de commit não foi fornecida”. (`app/chamados/models.py:1090`, `:1143`; `app/escala/management/commands/desfazer_corroboracao_regua.py:38`; declaração: [nao verificado]) | **VIU:** “declaração LEI-AKITA — Cura não comprovada”, identificando `recusa_da_reabertura`, `reabrir` e o consumidor. (`app/chamados/models.py:1071`, `:1143`; `app/escala/management/commands/desfazer_corroboracao_regua.py:38`; declaração: [nao verificado]) |

Para s1 e s2, o gabarito comprova a ocorrência em `1c565c8f`, mas a persistência nas outras voltas é [nao verificado]. A análise de calendário de `a3bbef39` não identifica as cinco fixtures omitidas de s1 nem o F841 de s2. (`app/api/tests/test_chamados_v2_destino.py:37`; `app/ponto/tests/test_bug115_lampada_do_marco.py:54`; demais fixtures e linha do F841: [nao verificado])

Os registros NOVO abaixo inventariam os achados alegados nas respostas, incluindo lacunas de teste e contraexemplos; não certificam ocorrência em produção nem sobrevivência executada dos mutantes [nao verificado]. [nao verificado]

NOTA|O261-VETO-A2_1c565c8f|1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_1c565c8f|2|[nao verificado]|A=EM_PARTE|B=EM_PARTE
NOTA|O261-VETO-A2_1c565c8f|3|existia|A=EM_PARTE|B=EM_PARTE
NOTA|O261-VETO-A2_1c565c8f|4|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_1c565c8f|5|existia|A=EM_PARTE|B=NAO_VIU
NOTA|O261-VETO-A2_1c565c8f|6|existia|A=EM_PARTE|B=EM_PARTE
NOTA|O261-VETO-A2_1c565c8f|7|existia|A=VIU|B=NAO_VIU
NOTA|O261-VETO-A2_1c565c8f|s1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_1c565c8f|s2|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_1c565c8f|s3|n/a|A=n/a|B=n/a
NOTA|O261-VETO-A2_673ecaf7|1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_673ecaf7|2|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_673ecaf7|3|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_673ecaf7|4|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_673ecaf7|5|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_673ecaf7|6|existia|A=EM_PARTE|B=EM_PARTE
NOTA|O261-VETO-A2_673ecaf7|7|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_673ecaf7|s1|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_673ecaf7|s2|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_673ecaf7|s3|n/a|A=n/a|B=n/a
NOTA|O261-VETO-A2_3b68a95b|1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_3b68a95b|2|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_3b68a95b|3|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_3b68a95b|4|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_3b68a95b|5|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_3b68a95b|6|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_3b68a95b|7|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_3b68a95b|s1|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_3b68a95b|s2|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_3b68a95b|s3|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_a3bbef39|1|existia|A=VIU|B=VIU
NOTA|O261-VETO-A2_a3bbef39|2|[nao verificado]|A=VIU|B=VIU
NOTA|O261-VETO-A2_a3bbef39|3|n/a|A=n/a|B=n/a
NOTA|O261-VETO-A2_a3bbef39|4|n/a|A=n/a|B=n/a
NOTA|O261-VETO-A2_a3bbef39|5|n/a|A=n/a|B=n/a
NOTA|O261-VETO-A2_a3bbef39|6|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_a3bbef39|7|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_a3bbef39|s1|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_a3bbef39|s2|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_a3bbef39|s3|n/a|A=n/a|B=n/a
NOVO|O261-VETO-A2_1c565c8f|A|app/ponto/services/cartorio.py:907|Ramo nunca_bateu calcula impressão sem veto e pode manter célula sem vínculo pulada.
NOVO|O261-VETO-A2_1c565c8f|A|app/chamados/models.py:921|Deduplicação omite via e suprime a trilha própria de uma segunda operação.
NOVO|O261-VETO-A2_1c565c8f|A|app/chamados/tests/test_emissao_reata_orfa.py:67|Teste do desligado pode passar sem o gate de situação porque falta a célula necessária à emissão. [nao verificado]
NOVO|O261-VETO-A2_1c565c8f|A|app/chamados/tests/test_emissor_nomeia_o_marco.py:88|Teste promete nascimento, mas verifica somente o contexto produzido por _ctx_c2. [nao verificado]
NOVO|O261-VETO-A2_1c565c8f|A|app/chamados/tests/test_bug125_cobranca_cega.py:176|Teste promete nascimento sem marco, mas verifica somente o contexto. [nao verificado]
NOVO|O261-VETO-A2_1c565c8f|A|app/ponto/management/commands/detectar_ausencias.py:139|Ativo sem vínculo pode ser descartado pelos fluxos antes de alcançar o escritor do aviso de cadastro.
NOVO|O261-VETO-A2_1c565c8f|A|app/chamados/services/disputa_emissao.py:1606|Resolver automático reabre o pai diretamente sem consultar o veto novo.
NOVO|O261-VETO-A2_1c565c8f|A|app/ponto/management/commands/reconciliar_grade.py:522|Grade pode reabrir o encerrado devolvido após a porta recusar seu renascimento.
NOVO|O261-VETO-A2_1c565c8f|A|app/chamados/tests/test_o261_veto_na_porta.py:212|Selo não reconhece a reabertura pelo receptor _pai e permanece verde com o escape. [nao verificado]
NOVO|O261-VETO-A2_1c565c8f|B|app/ponto/services/cartorio.py:907|Impressão do ramo nunca_bateu não incorpora o novo veto antes do retorno antecipado.
NOVO|O261-VETO-A2_1c565c8f|B|app/chamados/models.py:921|Deduplicação omite via e consulta toda a história, suprimindo outra operação e o retorno do veto após correção.
NOVO|O261-VETO-A2_1c565c8f|B|app/chamados/tests/test_emissor_nomeia_o_marco.py:88|Teste de nascimento observa apenas o dicionário de contexto. [nao verificado]
NOVO|O261-VETO-A2_1c565c8f|B|app/chamados/tests/test_bug125_cobranca_cega.py:176|Teste de nascimento sem marco não executa a abertura. [nao verificado]
NOVO|O261-VETO-A2_1c565c8f|B|app/ponto/management/commands/detectar_ausencias.py:140|Ativo sem vínculo não alcança automaticamente o escritor de regua_degenerada nos fluxos examinados.
NOVO|O261-VETO-A2_1c565c8f|B|app/chamados/services/disputa_emissao.py:1606|Callback automático reabre o pai sem passar pelo veto do renascer.
NOVO|O261-VETO-A2_1c565c8f|B|app/ponto/management/commands/reconciliar_grade.py:522|Grade trata somente retorno None e pode desfazer a recusa de renascimento reabrindo o encerrado.
NOVO|O261-VETO-A2_1c565c8f|B|app/chamados/tests/test_o261_veto_na_porta.py:207|Varredor não classifica pai.reabrir() como escrita de vitalidade.
NOVO|O261-VETO-A2_1c565c8f|B|bin/placar_code.sh:209|Placar captura somente o contador antigo e não mostra chamados vivos em dia vetado.
NOVO|O261-VETO-A2_673ecaf7|A|app/chamados/tests/test_o261_veto_na_porta.py:256|Inventário AST perde chamadas por alias ou getattr e novas chamadas dentro de função já classificada.
NOVO|O261-VETO-A2_673ecaf7|A|app/chamados/services/acoes_chamado.py:315|Resposta livre pode reabrir por transicionar sem passar por reabrir nem aparecer no censo.
NOVO|O261-VETO-A2_673ecaf7|A|app/chamados/services/acoes_disputa.py:265|Ação administrativa ignora a recusa de reabrir e anuncia sucesso após abrir disputa e pergunta.
NOVO|O261-VETO-A2_673ecaf7|A|app/chamados/models.py:923|Recusa histórica suprime nova trilha quando o mesmo veto retorna após passagem permitida.
NOVO|O261-VETO-A2_673ecaf7|A|bin/placar_code.sh:209|Placar pode mostrar zero enquanto o vigia novo encontra chamado vivo vetado sem célula furo.
NOVO|O261-VETO-A2_673ecaf7|B|app/chamados/tests/test_o261_veto_na_porta.py:256|Selo perde chamadas indiretas e registra funções sem detectar outra chamada dentro de função já enumerada.
NOVO|O261-VETO-A2_673ecaf7|B|app/chamados/services/acoes_chamado.py:315|Reabertura humana por transicionar fica fora do inventário de reabrir.
NOVO|O261-VETO-A2_673ecaf7|B|app/chamados/tests/test_contract_status_nucleo.py:23|Contrato complementar permite chamada externa a _gravar_estado para vivificar sem reconhecer essa escrita.
NOVO|O261-VETO-A2_673ecaf7|B|app/chamados/services/acoes_disputa.py:266|Ação administrativa ignora retorno de reabrir e pode deixar disputa aberta com pai encerrado.
NOVO|O261-VETO-A2_673ecaf7|B|app/chamados/models.py:923|Busca histórica cala a recusa de um novo ciclo após correção, renascimento e retorno do veto.
NOVO|O261-VETO-A2_673ecaf7|B|bin/placar_code.sh:209|Placar extrai apenas o contador antigo e pode ocultar o achado do vigia novo.
NOVO|O261-VETO-A2_673ecaf7|B|app/ponto/management/commands/reconciliar_grade.py:480|Triagem generaliza indevidamente o descarte de ec=None; _emitir_furo aceita None e chama a defesa de cadastro.
NOVO|O261-VETO-A2_673ecaf7|B|app/chamados/tests/test_emissor_nomeia_o_marco.py:88|Teste verifica contexto e não sustenta prova de nascimento. [nao verificado]
NOVO|O261-VETO-A2_673ecaf7|B|app/chamados/tests/test_bug125_cobranca_cega.py:176|Teste verifica contexto e não sustenta prova de nascimento. [nao verificado]
NOVO|O261-VETO-A2_3b68a95b|A|app/chamados/models.py:569|analisar() pode usar instância desatualizada e vivificar chamado encerrado concorrentemente sem consultar veto.
NOVO|O261-VETO-A2_3b68a95b|A|app/chamados/services/competencia_trancada.py:239|Serviço aceita autor=None e usuário sistema; a garantia humana depende dos chamadores encontrados.
NOVO|O261-VETO-A2_3b68a95b|A|app/chamados/models.py:925|Consulta seguida de inserção sem serialização ou unicidade permite duplicar a mesma recusa concorrentemente.
NOVO|O261-VETO-A2_3b68a95b|A|app/chamados/models.py:933|Nascimento permitido em outro dia pode liberar nova trilha para recusa antiga porque _passou não filtra dia ou célula.
NOVO|O261-VETO-A2_3b68a95b|A|app/chamados/models.py:935|Sequência de motivos A→B→A sem passagem permitida pode calar a volta de A.
NOVO|O261-VETO-A2_3b68a95b|A|app/core/models.py:313|Índices declarados não cobrem a chave JSON e a ordenação da consulta de deduplicação; custo real não medido.
NOVO|O261-VETO-A2_3b68a95b|B|app/chamados/models.py:566|analisar() pode reabrir por estado antigo em memória após encerramento concorrente, sem reler banco ou veto.
NOVO|O261-VETO-A2_3b68a95b|B|app/chamados/services/competencia_trancada.py:258|Serviço passa humano=True mesmo quando autor=None foi substituído por usuário sistema.
NOVO|O261-VETO-A2_3b68a95b|B|app/chamados/models.py:925|Duas chamadas podem consultar antes de qualquer inserção e gravar recusas duplicadas.
NOVO|O261-VETO-A2_3b68a95b|B|app/chamados/models.py:931|Nascimento em outro dia satisfaz _passou e permite linha extra sem passagem daquele dia.
NOVO|O261-VETO-A2_3b68a95b|B|app/chamados/models.py:378|Renovar SLA de chamado ainda encerrado altera atualizado_em e permite outra linha de recusa.
NOVO|O261-VETO-A2_3b68a95b|B|app/chamados/models.py:937|Inserção tardia de recusa pode ficar posterior ao nascimento permitido e calar o ciclo seguinte.
NOVO|O261-VETO-A2_3b68a95b|B|app/core/models.py:313|Índices não cobrem a chave JSON com a ordenação da deduplicação; plano e latência não foram verificados.
NOVO|O261-VETO-A2_a3bbef39|A|app/ponto/tests/test_veto_que_cai_rejulga.py:126|Spy observa argumento, mas não detecta hash que ignore especificamente VETO_DEGENERADA.
NOVO|O261-VETO-A2_a3bbef39|A|app/chamados/tests/_vinculo_do_dia.py:18|Não há teste direto dos sete weekdays; omissão de domingo dependeria dos dias exercitados pela suíte. [nao verificado]
NOVO|O261-VETO-A2_a3bbef39|A|app/ponto/tests/test_passe_turno_aberto_retroativo.py:142|MORDE verifica contadores, mas não exige a mensagem RECUSADO sem juiz.
NOVO|O261-VETO-A2_a3bbef39|A|app/ponto/tests/test_passe_turno_aberto_retroativo.py:124|Falta cenário com célula e veto recusando simultaneamente para distinguir a prioridade dos juízes.
NOVO|O261-VETO-A2_a3bbef39|A|app/ponto/tests/test_passe_turno_aberto_retroativo.py:140|MORDE substitui abrir e não exercita seu corpo real nem reproduz a mudança concorrente narrada.
NOVO|O261-VETO-A2_a3bbef39|B|app/ponto/tests/test_veto_que_cai_rejulga.py:126|Teste pode passar se o hash ignorar VETO_DEGENERADA, pois observa somente kwargs do spy. [nao verificado]
NOVO|O261-VETO-A2_a3bbef39|B|app/chamados/tests/_vinculo_do_dia.py:18|Ausência de teste próprio dos sete dias deixa detecção de domingo omitido dependente dos dias exercitados. [nao verificado]
NOVO|O261-VETO-A2_a3bbef39|B|app/ponto/tests/test_passe_turno_aberto_retroativo.py:113|Testes de motivos isolados não distinguem prioridade invertida quando célula e veto recusam juntos. [nao verificado]
NOVO|O261-VETO-A2_a3bbef39|B|app/ponto/tests/test_passe_turno_aberto_retroativo.py:142|Retirar a mensagem RECUSADO sem juiz preservando os contadores não é detectado pelo MORDE.
NOVO|O261-VETO-A2_a3bbef39|B|app/ponto/tests/test_passe_turno_aberto_retroativo.py:140|MORDE alcança o fallback por mock de abrir e não demonstra a concorrência descrita na docstring.

## 30/10 15:04 · sessao 91323e3d3246 · BENCH-CORRECAO · censo · origem=Code

Usei estas definições do gabarito:

- **E — FIO-DO-ADMIN-NO-ESTADO:** o painel omite os contadores do admin (`app/docs/BACKLOG.md:778`).
- **L — FIO-DO-ADMIN-LAVRA-VELHA:** o vigia aceita o dono lavrado sem conferir a idade da lavra (`app/docs/BACKLOG.md:779`).
- **S — SELO-CARIMBO-VIRADA:** o teste não provoca a travessia da meia-noite (`app/docs/BACKLOG.md:780`).

Considerei **L n/a nas três primeiras voltas**: as respostas anteriores à final descrevem o vigia recalculando o dono; a leitura do dono lavrado aparece na final (`app/chamados/services/vigia_fio.py:275`, citado em `8110a931`; `app/chamados/services/vigia_fio.py:266`, citado em `c3b91265`). A continuidade dessa implementação na volta intermediária `3e49c1bc` é inferida; confirmação independente dessa árvore: **[nao verificado]**.

Considerei **E e S existentes nas quatro voltas**. Para S nas duas primeiras, a existência é inferência de continuidade do teste do carimbo, presente na cópia; seu conteúdo exato nesses dois hashes é **[nao verificado]** (`app/chamados/tests/test_o261_vigia_fio.py:755`). As referências históricas abaixo preservam as linhas citadas pelos revisores.

| Volta | Item | A | B |
|---|---|---|---|
| `d75bd454` | E | **VIU:** “O ESTADO não mostra o passivo transferido” (`bin/fio_estado.py:82`). | **VIU:** “o ESTADO perde a visibilidade da transferência” (`bin/fio_estado.py:82`). |
| `d75bd454` | L | **n/a:** o ramo consulta o dono novamente (`app/chamados/services/vigia_fio.py:254`, citado na resposta). | **n/a:** o ramo consulta o dono novamente (`app/chamados/services/vigia_fio.py:254`, citado na resposta). |
| `d75bd454` | S | **EM PARTE:** “Uma rodada atravessando meia-noite pode combinar referências temporais diferentes”; aponta o comportamento do comando, sem identificar a falta desse cenário no selo (`app/chamados/management/commands/vigiar_fio_morto.py:195`). | **EM PARTE:** “Uma execução atravessando meia-noite pode combinar datas diferentes”; aponta o comportamento do comando, sem identificar a lacuna do selo (`app/chamados/management/commands/vigiar_fio_morto.py:195`). |
| `3e49c1bc` | E | **NÃO VIU:** a resposta trata exclusivamente do teste do recorte (`app/chamados/tests/test_o261_vigia_fio.py:600`). | **NÃO VIU:** a resposta trata exclusivamente do teste do recorte (`app/chamados/tests/test_o261_vigia_fio.py:611`). |
| `3e49c1bc` | L | **n/a**, pela continuidade histórica inferida **[nao verificado]**. | **n/a**, pela continuidade histórica inferida **[nao verificado]**. |
| `3e49c1bc` | S | **NÃO VIU:** a única crítica é à ausência de outra empresa visível no teste (`app/chamados/tests/test_o261_vigia_fio.py:600`). | **NÃO VIU:** a única crítica é à ausência de outra empresa visível no teste (`app/chamados/tests/test_o261_vigia_fio.py:611`). |
| `8110a931` | E | **VIU:** “Não mostra os três campos novos” (`bin/fio_estado.py:82`). | **VIU:** “o destino `fio_do_admin` fica fora da projeção e da frase do painel” (`bin/fio_estado.py:82`). |
| `8110a931` | L | **n/a:** “a lista publica dono lavrado e o vigia calcula dono atual” (`app/chamados/services/vigia_fio.py:275`). | **n/a:** “A lista usa `dono` lavrado; o vigia recalcula pelo carregador atual” (`app/chamados/services/vigia_fio.py:275`). |
| `8110a931` | S | **EM PARTE:** “O teste do carimbo […] não prova propagação do instante”; identifica cobertura temporal insuficiente, sem apontar a ausência da virada (`app/chamados/tests/test_o261_vigia_fio.py:787`). | **VIU:** “O teste temporal […] não fabrica travessia da meia-noite” (`app/chamados/tests/test_o261_vigia_fio.py:801`). |
| `c3b91265` | E | **VIU:** “o leitor do ESTADO descarta os três” (`bin/fio_estado.py:82`). | **VIU:** “o ESTADO não seleciona os três campos novos” (`bin/fio_estado.py:82`). |
| `c3b91265` | L | **EM PARTE:** “a leitura do dono lavrado está implementada”; menciona a origem do dono, sem apontar falta de controle de idade ou permanência indefinida (`app/chamados/services/vigia_fio.py:266`). | **EM PARTE:** “a lista lê uma fotografia persistida”; distingue fotografia de cálculo atual, sem apontar a ausência de controle de idade (`app/ponto/services/teto_na_lista.py:123`). |
| `c3b91265` | S | **VIU:** “O teste de carimbo também não força travessia de meia-noite” (`app/chamados/tests/test_o261_vigia_fio.py:795`). | **VIU:** “O selo temporal verifica formato/offset/data, sem provocar a virada” (`app/chamados/tests/test_o261_vigia_fio.py:793`). |

Os registros NOVO abaixo reproduzem os defeitos e lacunas levantados pelos revisores fora desses três itens; sua procedência histórica independente permanece **[nao verificado]**.

NOTA|FIO-MORTO-DONO-NA-LISTA_d75bd454|FIO-DO-ADMIN-NO-ESTADO|existia|A=VIU|B=VIU
NOTA|FIO-MORTO-DONO-NA-LISTA_d75bd454|FIO-DO-ADMIN-LAVRA-VELHA|n/a|A=n/a|B=n/a
NOTA|FIO-MORTO-DONO-NA-LISTA_d75bd454|SELO-CARIMBO-VIRADA|existia|A=EM_PARTE|B=EM_PARTE
NOTA|FIO-MORTO-DONO-NA-LISTA_3e49c1bc|FIO-DO-ADMIN-NO-ESTADO|existia|A=NAO_VIU|B=NAO_VIU
NOTA|FIO-MORTO-DONO-NA-LISTA_3e49c1bc|FIO-DO-ADMIN-LAVRA-VELHA|n/a|A=n/a|B=n/a
NOTA|FIO-MORTO-DONO-NA-LISTA_3e49c1bc|SELO-CARIMBO-VIRADA|existia|A=NAO_VIU|B=NAO_VIU
NOTA|FIO-MORTO-DONO-NA-LISTA_8110a931|FIO-DO-ADMIN-NO-ESTADO|existia|A=VIU|B=VIU
NOTA|FIO-MORTO-DONO-NA-LISTA_8110a931|FIO-DO-ADMIN-LAVRA-VELHA|n/a|A=n/a|B=n/a
NOTA|FIO-MORTO-DONO-NA-LISTA_8110a931|SELO-CARIMBO-VIRADA|existia|A=EM_PARTE|B=VIU
NOTA|FIO-MORTO-DONO-NA-LISTA_c3b91265|FIO-DO-ADMIN-NO-ESTADO|existia|A=VIU|B=VIU
NOTA|FIO-MORTO-DONO-NA-LISTA_c3b91265|FIO-DO-ADMIN-LAVRA-VELHA|existia|A=EM_PARTE|B=EM_PARTE
NOTA|FIO-MORTO-DONO-NA-LISTA_c3b91265|SELO-CARIMBO-VIRADA|existia|A=VIU|B=VIU
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|A|app/chamados/services/vigia_fio.py:253|Frota consulta todas as empresas e pode transferir ao admin um dia de empresa invisível na tela.
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|A|app/escala/services/cadastro_realidade.py:588|Recorte --empresa entrega objetos Empresa onde lista compara IDs inteiros, descartando linhas.
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|A|app/chamados/services/vigia_fio.py:251|Falha da lista não ocupa o cache e provoca nova execução integral por candidato.
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|A|app/chamados/management/commands/vigiar_fio_morto.py:195|Comando, censo e carimbo capturam instantes distintos e podem discordar ao atravessar meia-noite.
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|A|app/escala/services/cadastro_realidade.py:667|Helper aceita data estruturada sem garantir que a tela exiba esse dia; contraprova apresentada é sintética.
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|A|app/escala/services/cadastro_realidade.py:667|Truncar str(data) não normaliza datas como 25/10/2026 para a chave ISO do vigia.
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|A|app/escala/services/cadastro_realidade.py:373|Dias mostrados textualmente por C4/C5 ficam fora da projeção; elegibilidade de candidatos reais é [nao verificado].
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|A|app/ponto/management/commands/e6_oraculo.py:136|Novo caminho inclui lista completa, carregamento de insumos e possível execução do motor; custo atual é [nao verificado].
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|A|app/escala/services/cadastro_realidade.py:561|Falhas internas da lista retornam resultado parcial sem incrementar fio_do_admin_sem_resposta.
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|A|app/chamados/tests/test_o261_vigia_fio.py:560|Mocks da lista e do carregador escondem contratos de universo, argumentos e falha parcial.
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|A|app/chamados/tests/test_o261_vigia_fio.py:628|Teste da soma inclui fio_do_admin sem fabricar explicitamente uma parcela positiva nesse contador. [nao verificado]
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|A|app/folha/fora_da_folha.py:41|Dia transferido conserva orientação genérica de resolver chamado, sem orientação específica para a lista do admin.
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|B|app/chamados/services/vigia_fio.py:253|Frota consulta todas as empresas, permitindo transferência de dias que empresas_visiveis exclui da tela.
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|B|app/escala/services/cadastro_realidade.py:588|Lista compara empresa_id inteiro com objetos Empresa recebidos pelo recorte e descarta as linhas.
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|B|app/ponto/services/espelho.py:1229|Consulta nova pode executar cálculo amplo e sobreposto entre competências, contrariando a promessa de nenhum recálculo.
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|B|app/chamados/services/vigia_fio.py:251|Exceção persistente provoca repetição da lista inteira por candidato porque somente sucesso fica em cache.
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|B|app/escala/services/cadastro_realidade.py:565|Exceção interna de C9 vira lista parcial tratada como sucesso, deixando sem_resposta sem registrar a falha.
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|B|app/chamados/management/commands/vigiar_fio_morto.py:195|Comando, censo e carimbo usam referências temporais independentes, com possível divergência na meia-noite.
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|B|app/escala/services/cadastro_realidade.py:186|C2 ignora o hoje da lista e consulta date.today(), impedindo reprodução integral da rodada.
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|B|app/chamados/tests/test_o261_vigia_fio.py:560|Testes substituem lista e carregador e não verificam os argumentos, deixando passar os erros de universo e recorte. [nao verificado]
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|B|app/chamados/tests/test_o261_vigia_fio.py:645|Teste da soma não exige fio_do_admin positivo e pode passar sem exercitar a nova parcela. [nao verificado]
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|B|app/escala/services/cadastro_realidade.py:667|Presença de data estruturada é aceita sem garantir que a data seja exibida pela tela.
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|B|app/escala/services/cadastro_realidade.py:667|Truncamento da representação da data não valida nem normaliza seu formato.
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|B|app/folha/fora_da_folha.py:83|Orientação permanece resolver o chamado, sem encaminhamento novo para Cadastro x Realidade.
NOVO|FIO-MORTO-DONO-NA-LISTA_3e49c1bc|A|app/chamados/tests/test_o261_vigia_fio.py:600|Teste do universo não inclui outra empresa visível fora do recorte e passa mesmo removendo a interseção. [nao verificado]
NOVO|FIO-MORTO-DONO-NA-LISTA_3e49c1bc|B|app/chamados/tests/test_o261_vigia_fio.py:611|Teste não distingue presença e ausência da interseção porque falta segunda empresa visível fora do recorte. [nao verificado]
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|A|app/chamados/services/vigia_fio.py:181|Fallbacks temporais do censo violam o selo existente de um relógio por ato.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|A|app/chamados/services/vigia_fio.py:275|Vigia recalcula dono atual enquanto a lista apresenta dono lavrado, produzindo duas observações potencialmente distintas.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|A|app/escala/services/cadastro_realidade.py:667|Projeção de presença omite dias que C4/C5 apresentam somente no detalhe.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|A|app/escala/services/cadastro_realidade.py:187|C2 fixa corte de competência em 21, divergindo da autoridade registrada que considera o corte da empresa.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|A|app/colaboradores/services/calendario.py:249|Calendário chamado pelo censo captura outro instante sem receber o instante da rodada.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|A|app/escala/services/cadastro_realidade.py:186|C2 consulta date.today() em vez de usar a referência temporal recebida pela lista.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|A|app/escala/services/cadastro_realidade.py:350|C4 chama afastados_hoje sem transmitir o dia e aciona outro relógio.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|A|app/chamados/tests/test_o261_vigia_fio.py:560|Matriz positiva substitui lista e carregador, deixando seus contratos reais fora do teste. [nao verificado]
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|A|app/chamados/tests/test_o261_vigia_fio.py:675|Teste da soma acrescenta fio_do_admin sem construir uma parcela admin positiva. [nao verificado]
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|A|app/escala/services/cadastro_realidade.py:565|Falha interna de C9 é absorvida pela lista e não chega ao contador externo de falha.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|A|app/chamados/services/vigia_fio.py:259|Uma candidata aciona cálculo da lista completa; o gatilho restrito não restringe todo o trabalho executado.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|B|app/escala/services/cadastro_realidade.py:667|Predicado de presença não abrange dias que C4/C5 mostram textualmente sem data estruturada.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|B|app/chamados/services/vigia_fio.py:275|Dono atual BATIDA/CADASTRO pode transferir dia cuja C9 lavrada não oferece porta humana por ter outro dono ou nenhum dono.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|B|app/escala/services/cadastro_realidade.py:187|C2 escolhe competência com corte literal 21, contrariando janela_atual e o corte por empresa.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|B|app/chamados/services/vigia_fio.py:181|Defaults independentes do censo impedem que o agora do comando congele toda a rodada.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|B|app/escala/services/cadastro_realidade.py:186|C2 lê date.today() sem usar o dia transmitido pelo comando.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|B|app/escala/services/cadastro_realidade.py:350|C4 omite dia ao chamar afastados_hoje, cujo default consulta outro relógio.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|B|app/colaboradores/services/calendario.py:249|Calendário captura outro instante no caminho chamado pelo censo.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|B|app/escala/services/cadastro_realidade.py:565|Falha interna de C9 ou ausência de snapshot pode deixar sem_resposta zerado apesar da indisponibilidade da fonte.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|B|app/chamados/tests/test_o261_vigia_fio.py:674|Fixture da partição não produz fio_do_admin positivo, deixando a nova parcela sem exercício efetivo.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|B|app/chamados/tests/test_o261_vigia_fio.py:576|Teste positivo usa mocks e não cobra a conservação da partição completa. [nao verificado]
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|B|app/ponto/services/espelho.py:1229|Lista completa e carregadores podem ampliar cálculo e repetir intervalos entre competências; duração real é [nao verificado].
NOVO|FIO-MORTO-DONO-NA-LISTA_c3b91265|A|app/escala/services/cadastro_realidade.py:669|dias_na_lista omite datas que C4/C5 exibem somente no detalhe.
NOVO|FIO-MORTO-DONO-NA-LISTA_c3b91265|A|app/escala/services/cadastro_realidade.py:187|C2 mantém corte literal 21 em vez do corte da empresa indicado pela autoridade registrada.
NOVO|FIO-MORTO-DONO-NA-LISTA_c3b91265|A|app/escala/services/cadastro_realidade.py:186|C2 consulta date.today() e escapa do instante único capturado pelo comando.
NOVO|FIO-MORTO-DONO-NA-LISTA_c3b91265|A|app/escala/services/cadastro_realidade.py:350|C4 chama afastados_hoje sem passar o dia e provoca nova leitura temporal.
NOVO|FIO-MORTO-DONO-NA-LISTA_c3b91265|A|app/colaboradores/services/calendario.py:249|Calendário relê seu próprio relógio antes do ramo admin, impedindo congelamento transitivo da rodada.
NOVO|FIO-MORTO-DONO-NA-LISTA_c3b91265|A|app/escala/services/cadastro_realidade.py:565|Exceção interna de C9 e ausência de medição podem desaparecer da lista sem sinal em fio_do_admin_sem_resposta.
NOVO|FIO-MORTO-DONO-NA-LISTA_c3b91265|A|app/chamados/tests/test_o261_vigia_fio.py:666|Teste da partição inclui fio_do_admin, mas a mistura não cria parcela positiva nesse contador. [nao verificado]
NOVO|FIO-MORTO-DONO-NA-LISTA_c3b91265|A|app/chamados/tests/test_o261_vigia_fio.py:565|Teste positivo separado dos dois dias admin não verifica a partição completa. [nao verificado]
NOVO|FIO-MORTO-DONO-NA-LISTA_c3b91265|A|app/escala/services/cadastro_realidade.py:195|Lista acionada por uma candidata percorre células de todo o recorte e outras fontes, ampliando o custo além dos dias mortos.
NOVO|FIO-MORTO-DONO-NA-LISTA_c3b91265|B|app/escala/services/cadastro_realidade.py:669|Helper considera somente data estruturada e ignora dias mencionados textualmente em C4/C5.
NOVO|FIO-MORTO-DONO-NA-LISTA_c3b91265|B|app/escala/services/cadastro_realidade.py:187|C2 fixa corte de competência em 21, contrariando o corte por empresa registrado.
NOVO|FIO-MORTO-DONO-NA-LISTA_c3b91265|B|app/colaboradores/services/calendario.py:249|Calendário chamado pelo censo relê timezone.now() apesar do instante único do comando.
NOVO|FIO-MORTO-DONO-NA-LISTA_c3b91265|B|app/escala/services/cadastro_realidade.py:186|C2 consulta date.today() em vez de usar a referência temporal da rodada.
NOVO|FIO-MORTO-DONO-NA-LISTA_c3b91265|B|app/escala/services/cadastro_realidade.py:350|C4 omite dia ao chamar afastados_hoje e aciona o default que relê localdate().
NOVO|FIO-MORTO-DONO-NA-LISTA_c3b91265|B|app/ponto/management/commands/pautas_discordantes.py:93|Pautas mantém consulta parcial ao juiz antes de chamar novamente o carregador completo do dono.
NOVO|FIO-MORTO-DONO-NA-LISTA_c3b91265|B|app/chamados/tests/test_o261_vigia_fio.py:666|Mistura da partição não cria C9 nem exige fio_do_admin positivo, deixando a nova parcela sem proteção efetiva.
NOVO|FIO-MORTO-DONO-NA-LISTA_c3b91265|B|app/chamados/tests/test_o261_vigia_fio.py:553|Teste positivo separado não verifica a soma da partição. [nao verificado]
NOVO|FIO-MORTO-DONO-NA-LISTA_c3b91265|B|app/escala/services/cadastro_realidade.py:195|Uma candidata dispara montagem de todas as assinaturas e passagem pelas células da competência de todo o recorte.
NOVO|FIO-MORTO-DONO-NA-LISTA_c3b91265|B|app/escala/services/cadastro_realidade.py:561|Falha interna de C9 é absorvida pela lista e pode resultar em fio_do_admin_sem_resposta igual a zero.
NOVO|FIO-MORTO-DONO-NA-LISTA_c3b91265|B|app/chamados/tests/test_o261_vigia_fio.py:573|Testes de falha substituem a lista inteira e não exercitam a falha interna da fonte C9. [nao verificado]

## 30/10 15:04 · sessao 942d4847c398 · BENCH-CORRECAO · censo · origem=Code

Usei os itens **1–5 na ordem do gabarito**, todos atribuídos ao opus final sobre `af2cf55f`. A suíte verde é contexto informado pelo anexo, sem comprovação independente nesta análise. (`gab_O261-V-FASE.txt:1`, `gab_O261-V-FASE.txt:3`; execução **[nao verificado]**.)

A existência nas voltas anteriores é inferida dos caminhos descritos nas respostas. Para o item 4, considerei `d6adf8c0` a primeira versão mostrada e o defeito existente após o fortalecimento dos testes em `519fa2c2`; a correspondência com o RED histórico permanece **[nao verificado]**. As referências históricas abaixo são as citadas pelos revisores, sem confirmação independente das três árvores. **[nao verificado]**

Apliquei **EM_PARTE** também às verificações negativas específicas: abordam o tema, mas não identificam o defeito, conforme a definição solicitada. **[nao verificado]** [nao verificado]

| Volta | Item | Fundamentação de A | Fundamentação de B |
|---|---|---|---|
| `d6adf8c0` | 1 — custo | **VIU:** “Cada HTML/PDF e os commands C7/C8 passam a pagar a leitura de toda a frota”. (`app/escala/services/cadastro_realidade.py:601`) | **VIU:** “Varredura global no request, contrariando a restrição do próprio serviço”. (`app/escala/services/cadastro_realidade.py:330`, `app/escala/services/cadastro_realidade.py:48`) |
| `d6adf8c0` | 2 — duas sugestões | **NAO_VIU:** o achado de seleção paralela trata da identidade do vínculo, sem comparar os dois produtores de sugestão de âncora. (`app/escala/services/fase_12x36.py:114`, `app/escala/services/cadastro_realidade.py:488`) | **EM_PARTE:** “Não encontrei segundo juiz no complemento”. Aborda a unicidade, sem confrontar as sugestões do detalhe e da porta. (`app/escala/services/cadastro_realidade.py:336`) |
| `d6adf8c0` | 3 — dias e vínculo da ação | **VIU:** “cabeçalho do vínculo novo e ação sem identificar qual corrigir”; também registra os quatro dias reais como não verificados. (`app/escala/services/cadastro_realidade.py:488`, `app/escala/services/cadastro_realidade.py:521`; confirmação dos dias **[nao verificado]**) | **EM_PARTE:** “não demonstram a presença dos quatro dias reais de col451”; não aponta o vínculo errado da ação. (`app/escala/tests/test_o261v_fase_na_lista.py:23`, `app/escala/tests/test_o261v_fase_na_lista.py:40`) |
| `d6adf8c0` | 4 — RED da primeira versão | **n/a**, pela ressalva temporal acima. **[nao verificado]** | **n/a**, pela mesma ressalva. **[nao verificado]** |
| `d6adf8c0` | 5 — inventário dos leitores | **EM_PARTE:** “As chaves novas são aditivas para os consumidores encontrados”; enumera leitores, sem denunciar sua ausência na LEI-AKITA. (`app/escala/management/commands/fechar_avisos_regua_na_lista.py:32`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:40`) | **EM_PARTE:** “Os commands também pagam esse custo, embora selecionem exclusivamente C7 e C8”; reconhece leitores, sem apontar a omissão documental. (`app/escala/management/commands/fechar_avisos_regua_na_lista.py:27`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:35`) |
| `519fa2c2` | 1 — custo | **NAO_VIU:** não há denúncia da varredura global nesta resposta. **[nao verificado]** | **NAO_VIU:** não há denúncia da varredura global nesta resposta. **[nao verificado]** |
| `519fa2c2` | 2 — duas sugestões | **EM_PARTE:** “não identifiquei segundo juiz”; verifica o adaptador, sem comparar as duas sugestões. (`app/escala/services/cadastro_realidade.py:330`) | **NAO_VIU:** a discussão sobre juiz trata do pareamento sem ata, outro defeito. (`app/escala/services/fase_12x36.py:175`, `app/core/juizes.py:905`) |
| `519fa2c2` | 3 — dias e vínculo da ação | **EM_PARTE:** “procede a correção de identificação”, com `ec_id` no detalhe; mantém os dias reais como não verificados, sem denunciar a ação restante. (`app/escala/services/cadastro_realidade.py:340`, `app/escala/services/cadastro_realidade.py:344`; dias **[nao verificado]**) | **EM_PARTE:** “procede como correção de apresentação”; aborda a identidade exibida, sem apontar o vínculo errado da ação. (`app/escala/services/cadastro_realidade.py:340`, `app/escala/services/cadastro_realidade.py:344`) |
| `519fa2c2` | 4 — RED da primeira versão | **EM_PARTE:** “RED histórico [...] [nao verificado]”; não identifica que a prova cobre somente a primeira versão. **[nao verificado]** | **EM_PARTE:** “RED histórico [...] [não verificado]”; mesma limitação. **[nao verificado]** |
| `519fa2c2` | 5 — inventário dos leitores | **NAO_VIU:** não aponta os leitores ausentes da LEI-AKITA. **[nao verificado]** | **NAO_VIU:** não aponta os leitores ausentes da LEI-AKITA. **[nao verificado]** |
| `af2cf55f` | 1 — custo | **NAO_VIU:** não denuncia o custo da chamada global. **[nao verificado]** | **NAO_VIU:** não denuncia o custo da chamada global. **[nao verificado]** |
| `af2cf55f` | 2 — duas sugestões | **NAO_VIU:** conferir a “reutilização do leitor” não identifica a duplicidade de sugestões. (`app/escala/services/cadastro_realidade.py:338`) | **EM_PARTE:** “sem cálculo próprio de fase”; aborda a autoridade do complemento, sem confrontá-la com a sugestão da porta. (`app/escala/services/cadastro_realidade.py:331`) |
| `af2cf55f` | 3 — dias e vínculo da ação | **EM_PARTE:** “reprodução dos quatro dias do col451: [nao verificado]”; considera regular a ação pela tela existente. (`app/escala/services/cadastro_realidade.py:491`; reprodução **[nao verificado]**) | **EM_PARTE:** “resultado real do col451: [não verificado]”; também aceita o encaminhamento à tela existente. (`app/escala/services/cadastro_realidade.py:491`; resultado **[nao verificado]**) |
| `af2cf55f` | 4 — RED da primeira versão | **EM_PARTE:** “histórico RED/verde [...] [nao verificado]”; não aponta a cobertura restrita à versão inicial. **[nao verificado]** | **EM_PARTE:** “RED histórico [...] [não verificado]”; mesma limitação. **[nao verificado]** |
| `af2cf55f` | 5 — inventário dos leitores | **NAO_VIU:** não denuncia a omissão documental. **[nao verificado]** | **NAO_VIU:** não denuncia a omissão documental. **[nao verificado]** |

Nos registros `NOVO`, incluí defeitos fora do gabarito explicitamente reconhecidos nas respostas, inclusive dívidas anteriores retomadas nas triagens; as referências são as fornecidas pelos revisores. A procedência histórica desses registros não foi revalidada. **[nao verificado]**

NOTA|O261-V-FASE_d6adf8c0|1|existia|A=VIU|B=VIU
NOTA|O261-V-FASE_d6adf8c0|2|existia|A=NAO_VIU|B=EM_PARTE
NOTA|O261-V-FASE_d6adf8c0|3|existia|A=VIU|B=EM_PARTE
NOTA|O261-V-FASE_d6adf8c0|4|n/a|A=n/a|B=n/a
NOTA|O261-V-FASE_d6adf8c0|5|existia|A=EM_PARTE|B=EM_PARTE
NOTA|O261-V-FASE_519fa2c2|1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-V-FASE_519fa2c2|2|existia|A=EM_PARTE|B=NAO_VIU
NOTA|O261-V-FASE_519fa2c2|3|existia|A=EM_PARTE|B=EM_PARTE
NOTA|O261-V-FASE_519fa2c2|4|existia|A=EM_PARTE|B=EM_PARTE
NOTA|O261-V-FASE_519fa2c2|5|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-V-FASE_af2cf55f|1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-V-FASE_af2cf55f|2|existia|A=NAO_VIU|B=EM_PARTE
NOTA|O261-V-FASE_af2cf55f|3|existia|A=EM_PARTE|B=EM_PARTE
NOTA|O261-V-FASE_af2cf55f|4|existia|A=EM_PARTE|B=EM_PARTE
NOTA|O261-V-FASE_af2cf55f|5|existia|A=NAO_VIU|B=NAO_VIU
NOVO|O261-V-FASE_d6adf8c0|A|app/escala/services/cadastro_realidade.py:599|Falha do leitor de fase é registrada apenas em log e fica indistinguível de ausência de achados C10.
NOVO|O261-V-FASE_d6adf8c0|A|app/escala/tests/test_o261v_fase_na_lista.py:111|Validação das datas aceita valores errados ou repetidos que preservem comprimento e formato ISO.
NOVO|O261-V-FASE_d6adf8c0|A|app/escala/tests/test_o261v_fase_na_lista.py:128|Teste do PDF verifica somente o prefixo %PDF e não protege a presença de C10 e suas datas. [nao verificado]
NOVO|O261-V-FASE_d6adf8c0|A|app/escala/tests/test_o261v_fase_na_lista.py:12|A afirmação de que os dois mocks ficam vermelhos sem a integração é exagerada: o mock vazio continuaria verde. [nao verificado]
NOVO|O261-V-FASE_d6adf8c0|A|app/escala/services/fase_12x36.py:175|Leitor pareia batidas cruas sem alimentar o papel da ata, dívida anterior ampliada pelo consumo em C10.
NOVO|O261-V-FASE_d6adf8c0|B|app/escala/tests/test_o261v_fase_na_lista.py:114|Teste das datas não detecta a troca entre categorias trabalho e folga quando as sequências têm o mesmo tamanho. [nao verificado]
NOVO|O261-V-FASE_d6adf8c0|B|app/escala/tests/test_o261v_fase_na_lista.py:128|Teste do PDF permite omitir C10 e suas datas porque exige apenas o prefixo %PDF. [nao verificado]
NOVO|O261-V-FASE_519fa2c2|A|app/escala/services/cadastro_realidade.py:112|C10 rotula como 12x36 um universo do leitor que também inclui achados de 24x48.
NOVO|O261-V-FASE_519fa2c2|A|app/escala/services/cadastro_realidade.py:609|Falha do leitor de fase continua sem indicador de leitura incompleta na lista.
NOVO|O261-V-FASE_519fa2c2|A|app/escala/services/fase_12x36.py:175|Pareamento direto sem alimentação da ata permanece como dívida do leitor existente.
NOVO|O261-V-FASE_519fa2c2|B|app/escala/services/cadastro_realidade.py:112|C10 apresenta achados alcançáveis de 24x48 com texto fixo de 12x36.
NOVO|O261-V-FASE_519fa2c2|B|app/escala/services/fase_12x36.py:175|A exclusão por sítio não tocado é incorreta: fora_de_fase foi alterada e mantém pareamento sem o papel da ata.
NOVO|O261-V-FASE_519fa2c2|B|app/escala/services/cadastro_realidade.py:550|A guarda compartilhada com os vizinhos mantém falhas do leitor sem aviso na lista.
NOVO|O261-V-FASE_af2cf55f|A|app/escala/services/cadastro_realidade.py:114|Rótulo promete falta onde o leitor identifica somente ausência de turno completo, inclusive em dia justificado.
NOVO|O261-V-FASE_af2cf55f|A|app/escala/services/fase_12x36.py:175|Permanece o pareamento sem alimentação da ata, com migração para turnos_do_colab já inventariada.
NOVO|O261-V-FASE_af2cf55f|B|app/escala/services/fase_12x36.py:175|Reconhece a dívida persistente do pareamento sem ata, embora a considere fora do complemento.

## 30/10 15:03 · sessao 407c616a174f · BENCH-CORRECAO · censo · origem=Code

A numeração 1–6 segue o gabarito reproduzido em `app/docs/RELATO.md:13`. As referências dos achados NOVO são as citadas pelos revisores; não representam confirmação independente nas árvores históricas. [não verificado]

Para temporalidade, considerei os itens 1–5 existentes desde a primeira volta e o item 6 a partir de `24f1cd69`, quando aparece o cabeçalho calculado por `dia_mostrado`. Na primeira volta, o cabeçalho ainda usa diretamente `data_evento`: a divergência apontada ali não depende de regeneração. A correspondência histórica foi inferida dos anexos e do resumo em `app/docs/RELATO.md:24`; não foi confirmada por comparação entre snapshots. [não verificado]

Os créditos VIU e EM_PARTE se apoiam nestes trechos das respostas:

- **469d61ba, item 2 — ambos VIU.** A: “O texto lê `lampadas[].instante`; o plantio continua recompondo marcos [...] dentro de um envelope de 90 minutos”, com contraexemplo 08→07. B: “há fontes paralelas preexistentes”, descrevendo escala ativa/template versus vínculo/DNA congelado. (`app/chamados/catalogo/query.py:65`; `app/chamados/juizes.py:305`; `app/chamados/juizes.py:308`; referências de B: `app/chamados/juizes.py:1251`, `app/escala/models.py:1099`.)
- **469d61ba, item 3 — ambos EM_PARTE.** A: “não exercitam [...] telas” e “RED contra o commit anterior: [não verificado]”. B: “Suíte Django e RED histórico são [não verificado]”. Abordam cobertura/RED, sem identificar o TypeError nem demonstrar a insuficiência histórica específica. (`app/chamados/tests/test_noturno_le_o_dia_do_instante.py:65`; `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:110`.)
- **469d61ba, item 4 — ambos EM_PARTE.** A: “`classe_do_lote` calcula o plantio” e “Esse trabalho acontece por pergunta da fila”. B: “acrescenta consultas”, detalhando leituras repetidas. Identificam o custo, sem fechar o percurso pelas views da UI. (`app/chamados/services/validacao.py:830`; `app/chamados/services/materializacao.py:266`; `app/chamados/services/materializacao.py:355`.)
- **469d61ba, item 5 — ambos EM_PARTE.** A: “não exercitam intervalos”. B: “O DNA de teste deixa os intervalos vazios”. Nenhum aponta a mudança de classe do intervalo diurno. (`app/chamados/tests/test_noturno_le_o_dia_do_instante.py:36`; `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:65`.)
- **24f1cd69, item 6 — A EM_PARTE.** “A propriedade calcula o dia pelo DNA atual”; a pergunta existente retorna “sem atualizar `pergunta`”. O exemplo aborda texto antigo, sem identificar regeneração posterior da célula. (`app/chamados/models.py:2172`; `app/chamados/models.py:2062`; `app/chamados/models.py:2084`.)
- **31b8cec0, item 6 — ambos EM_PARTE.** A: “a igualdade falha e retorna 22/10”. B: “a comparação reconstrói [...] não coincide e o cabeçalho retorna 22/10”. Apontam contradição entre cabeçalho e texto causada por data ISO com hora, sem apontar regeneração. (`app/chamados/catalogo/query.py:99`; `app/chamados/models.py:2176`.)
- **691508dd, item 6 — ambos EM_PARTE.** A: “O ponto final quebra a igualdade com o produtor e aciona o fallback”. B: “acrescentar apenas `.` faz retornar 21/10”. Apontam outra causa de contradição entre cabeçalho e texto. (`app/chamados/models.py:2178`; `app/templates/chamados/partials/_card_atendimento.html:90`.)

NOTA|O261-NOTURNO_469d61ba|1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_469d61ba|2|existia|A=VIU|B=VIU
NOTA|O261-NOTURNO_469d61ba|3|existia|A=EM_PARTE|B=EM_PARTE
NOTA|O261-NOTURNO_469d61ba|4|existia|A=EM_PARTE|B=EM_PARTE
NOTA|O261-NOTURNO_469d61ba|5|existia|A=EM_PARTE|B=EM_PARTE
NOTA|O261-NOTURNO_469d61ba|6|n/a|A=n/a|B=n/a
NOTA|O261-NOTURNO_24f1cd69|1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_24f1cd69|2|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_24f1cd69|3|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_24f1cd69|4|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_24f1cd69|5|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_24f1cd69|6|existia|A=EM_PARTE|B=NAO_VIU
NOTA|O261-NOTURNO_31b8cec0|1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_31b8cec0|2|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_31b8cec0|3|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_31b8cec0|4|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_31b8cec0|5|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_31b8cec0|6|existia|A=EM_PARTE|B=EM_PARTE
NOTA|O261-NOTURNO_691508dd|1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_691508dd|2|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_691508dd|3|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_691508dd|4|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_691508dd|5|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_691508dd|6|existia|A=EM_PARTE|B=EM_PARTE
NOTA|O261-NOTURNO_e8c314e0|1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_e8c314e0|2|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_e8c314e0|3|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_e8c314e0|4|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_e8c314e0|5|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_e8c314e0|6|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_06d7ad06|1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_06d7ad06|2|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_06d7ad06|3|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_06d7ad06|4|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_06d7ad06|5|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_06d7ad06|6|existia|A=NAO_VIU|B=NAO_VIU
NOVO|O261-NOTURNO_469d61ba|A|app/templates/chamados/partials/_card_atendimento.html:90|Cabeçalho e confirmação de escape anunciam D enquanto a pergunta noturna passa a nomear D+1.
NOVO|O261-NOTURNO_469d61ba|A|app/templates/chamados/partials/modal_fio.html:180|Modal anuncia D junto do texto D+1; a verificação de divergência não compara o dia textual.
NOVO|O261-NOTURNO_469d61ba|A|app/api/views.py:1168|API entrega data_evento_label D junto da pergunta D+1; apresentação pelo cliente não verificada.
NOVO|O261-NOTURNO_469d61ba|A|app/chamados/catalogo/query.py:95|Truncar a entrada em dez caracteres altera o tratamento anterior de datas com hora ou sufixo.
NOVO|O261-NOTURNO_469d61ba|A|app/chamados/catalogo/query.py:100|Consulta à célula ocorre antes do dedup; a dupla geração da fábrica pode cobrar duas consultas por alvo.
NOVO|O261-NOTURNO_469d61ba|A|app/chamados/services/materializacao.py:404|Ajuste overnight fora do try alcança parse desprotegido de DNA e pode interromper a classificação.
NOVO|O261-NOTURNO_469d61ba|A|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:128|Selo deixa escapar f-string, porcentagem, format posicional e format_map; perímetro e exclusões limitam a fiscalização.
NOVO|O261-NOTURNO_469d61ba|A|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:65|Testes não cobrem os seis emissores, identidade persistida nem teto de consultas. [nao verificado]
NOVO|O261-NOTURNO_469d61ba|A|app/chamados/management/commands/auditar_disputas.py:191|INV4 verifica existência de texto possível, sem conferir o dia do texto já gravado.
NOVO|O261-NOTURNO_469d61ba|A|app/chamados/services/fio.py:736|Ação confirmar_correcao_pergunta enviada pelo botão não é tratada e cai em Nada a fazer.
NOVO|O261-NOTURNO_469d61ba|B|app/templates/chamados/partials/_card_atendimento.html:90|Cabeçalho anuncia D imediatamente antes da pergunta noturna que nomeia D+1.
NOVO|O261-NOTURNO_469d61ba|B|app/templates/chamados/partials/modal_fio.html:180|Modal anuncia D junto do texto D+1 e não verifica divergência com o dia textual.
NOVO|O261-NOTURNO_469d61ba|B|app/api/views.py:1168|API preserva label D junto da pergunta D+1; apresentação pelo cliente não verificada.
NOVO|O261-NOTURNO_469d61ba|B|app/chamados/catalogo/query.py:95|Corte nos dez primeiros caracteres muda a normalização de datas com hora mesmo sem colaborador.
NOVO|O261-NOTURNO_469d61ba|B|app/chamados/services/fabrica_da_celula.py:124|Dupla geração e formatação anterior ao dedup podem acrescentar duas consultas à célula por alvo.
NOVO|O261-NOTURNO_469d61ba|B|app/chamados/services/materializacao.py:404|Parse inválido de DNA no ajuste overnight pode propagar exceção até a classificação da fila.
NOVO|O261-NOTURNO_469d61ba|B|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:140|Selo deixa escapar f-string, porcentagem, format posicional, str.format indireto e format_map.
NOVO|O261-NOTURNO_469d61ba|B|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:60|Testes não exercitam os seis emissores, identidade persistida, padrao nem data com hora. [nao verificado]
NOVO|O261-NOTURNO_469d61ba|B|app/chamados/management/commands/auditar_disputas.py:74|Auditorias existentes não conferem o dia do DNA no texto gravado.
NOVO|O261-NOTURNO_469d61ba|B|app/chamados/tests/test_corrigir_pergunta_cega.py:54|Teste de correção simula atribuição e save, sem provar a ação real nem o cenário noturno. [nao verificado]
NOVO|O261-NOTURNO_469d61ba|B|app/chamados/management/commands/curar_perguntas_em_limbo.py:31|Casamento pelo texto integral e hash da nota podem ser afetados por correção retrospectiva; impacto não verificado.
NOVO|O261-NOTURNO_24f1cd69|A|app/templates/chamados/partials/_card_atendimento.html:164|Confirmação de escape usa data_evento D enquanto o cabeçalho usa dia_mostrado D+1.
NOVO|O261-NOTURNO_24f1cd69|A|app/chamados/services/cobranca.py:83|Cobrança push continua reunindo dias por data_evento e pode anunciar D em vez de D+1. [nao verificado]
NOVO|O261-NOTURNO_24f1cd69|A|app/chamados/services/aviso_pendencia.py:20|Aviso de questionário usa data_evento e conserva D nos retornos da API.
NOVO|O261-NOTURNO_24f1cd69|A|app/chamados/models.py:2172|Pergunta antiga reutilizada sem atualizar o texto pode receber cabeçalho D+1 calculado pelo DNA atual.
NOVO|O261-NOTURNO_24f1cd69|A|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:154|Detector acusa placeholder em keyword de mensagem de exceção embora seja prosa.
NOVO|O261-NOTURNO_24f1cd69|A|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:147|Detector deixa escapar format com expansão de dicionário contendo data.
NOVO|O261-NOTURNO_24f1cd69|A|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:133|Varredura restrita a chamados não garante exclusividade global do produtor.
NOVO|O261-NOTURNO_24f1cd69|B|app/templates/chamados/partials/_card_atendimento.html:164|Confirmação usa data_evento D e diverge do cabeçalho calculado por dia_mostrado.
NOVO|O261-NOTURNO_24f1cd69|B|app/chamados/services/aviso_pendencia.py:20|Avisos de pendência continuam nomeando o questionário por data_evento D.
NOVO|O261-NOTURNO_24f1cd69|B|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:154|Detector acusa prosa em keyword de exceção e format_map sem campo data.
NOVO|O261-NOTURNO_24f1cd69|B|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:156|Literal com placeholder data dentro de f-string escapa porque o pai JoinedStr é ignorado.
NOVO|O261-NOTURNO_24f1cd69|B|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:134|Varredura restrita a chamados e isenção integral de models.py deixam código sem fiscalização.
NOVO|O261-NOTURNO_31b8cec0|A|app/chamados/models.py:2176|Contexto ISO com hora produz texto bruto, mas dia_mostrado retorna D+1 e contradiz esse texto.
NOVO|O261-NOTURNO_31b8cec0|A|app/api/views.py:1043|Queryset carrega chamado_destino__celula, mas dia_mostrado acessa p.celula e gera N+1 na API.
NOVO|O261-NOTURNO_31b8cec0|A|app/chamados/services/aviso_pendencia.py:9|Cobrança e aviso carregam célula individualmente em pergunta_viva porque o queryset não seleciona a relação.
NOVO|O261-NOTURNO_31b8cec0|A|app/chamados/models.py:2162|Propriedade sem cache repete o cálculo no cabeçalho e nas opções do card.
NOVO|O261-NOTURNO_31b8cec0|A|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:155|Selo deixa escapar replace por variável e template passado em pergunta; exclusões ampliam os falsos negativos.
NOVO|O261-NOTURNO_31b8cec0|A|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:150|Selo acusa format com dicionário sem data, format_map alheio e prosa concatenada em exceção.
NOVO|O261-NOTURNO_31b8cec0|B|app/chamados/models.py:2176|Texto com data ISO e hora não coincide com a reconstrução canônica e recebe cabeçalho D+1 divergente.
NOVO|O261-NOTURNO_31b8cec0|B|app/api/views.py:1045|API não carrega p.celula antecipadamente e acrescenta uma consulta por pergunta ao serializar o dia.
NOVO|O261-NOTURNO_31b8cec0|B|app/chamados/services/aviso_pendencia.py:12|Cobrança e aviso também omitem celula do carregamento inicial e a buscam individualmente em pergunta_viva.
NOVO|O261-NOTURNO_31b8cec0|B|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:151|AST acusa format e format_map que substituem apenas nome e não formatam data.
NOVO|O261-NOTURNO_31b8cec0|B|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:150|AST deixa escapar replace por variável, alias de format e template em pergunta; models e código externo ficam dispensados.
NOVO|O261-NOTURNO_691508dd|A|app/chamados/models.py:2178|Ponto final ou data ISO no texto quebra a igualdade canônica e faz o cabeçalho retornar D apesar do texto D+1.
NOVO|O261-NOTURNO_691508dd|A|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:168|Detector ignora format com dicionário expandido ou chave dinâmica que pode fornecer data.
NOVO|O261-NOTURNO_691508dd|B|app/chamados/models.py:2178|Acrescentar ponto final ou usar texto ISO faz o cabeçalho voltar a D embora o texto continue nomeando D+1.
NOVO|O261-NOTURNO_691508dd|B|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:168|Expansão interna de dicionário pode fornecer data e escapar do detector.
NOVO|O261-NOTURNO_691508dd|B|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:178|Cópia do espelho em outro dicionário escapa porque literal com pai ast.Dict não é reconhecido.
NOVO|O261-NOTURNO_e8c314e0|A|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:172|Detector acusa format com expansão interna contendo apenas nome, embora não exista formatação de data.
NOVO|O261-NOTURNO_e8c314e0|B|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:170|Novo all rejeita expansão interna de dicionário mesmo quando receptor e argumentos usam somente nome.
NOVO|O261-NOTURNO_06d7ad06|A|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:166|Filtro por substring exata deixa escapar campos válidos data!s e data: na formatação.
NOVO|O261-NOTURNO_06d7ad06|B|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:166|Filtro deixa escapar data!s e data com especificador de formato, permitindo outro produtor de data.

## 30/10 15:03 · sessao ae6de421cf17 · BENCH-CORRECAO · censo · origem=Code

Numeração: **1** LEI-AKITA nos commits; **2** Pauta DP; **3** custo da cerca; **4** guarda morta; **5** R1 sem mordida; **6** composição duplicada; **7** ordem da docstring; **8** veto garantido pela porta de nascimento. Os seis primeiros e sua cura estão registrados em `app/docs/RELATO.md:83–84`; os dois últimos, em `app/docs/RELATO.md:85–87`.

Há uma limitação temporal: o material não permite determinar quando a guarda morta foi acrescentada nem quando a lista da docstring ficou fora de ordem. Nas linhas abaixo, **n/a nesses dois casos significa não imputação conservadora**; inexistência nas árvores anteriores: **[nao verificado]**. A relavratura descrita por A e B em `0ed50cc2` sugere que a guarda veio depois, mas seu hash de introdução é **[nao verificado]**.

Para o item 8, considero a invariante estrutural completa. A consulta acrescentada ao c2 corrige o caminho sequencial, mas a porta continua sem garantir o veto no nascimento/renascimento (`app/docs/RELATO.md:45–49`; `app/chamados/models.py:666–688`).

As classificações positivas têm estes fundamentos, usando as referências históricas citadas nas respostas:

- **4da2943d, item 3:** A, **EM PARTE**: “consulta de exportação duplicada por intervalo vivo” identifica custo adicional, sem localizar toda a cerca antecipada (`app/chamados/reconciliador.py:703,709,749,1436`). B, **VIU**: “C6 também passou a anteceder as recusas” e “esse cenário não mede o custo novo” apontam antecipação e ausência de cobertura do universo de órfãs (`app/chamados/reconciliador.py:394`; `app/ponto/tests/test_selo_chokepoint_escrita.py:239`).
- **4da2943d, item 5:** B, **EM PARTE**: “R1/R2/R3 […] possuem asserções da mudança” examina o teste, mas não identifica a falta de exigência do renascimento (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:117`).
- **4da2943d, item 6:** A, **EM PARTE**: “Derivação paralela […] nada na implementação. A lacuna está no selo da janela”; B, **EM PARTE**: “Os helpers delegam a C6, K6 e `janela_atual`”. Ambos abordam a composição, sem apontar suas duas versões nem a divergência sem data (`app/chamados/reconciliador.py:700–703`; `app/chamados/reconciliador.py:711`).
- **68a38548, item 6:** A, **EM PARTE**: “Nada de fórmula duplicada; há resultado antigo substituindo a consulta atual”; B, **EM PARTE**: “Nenhuma fórmula nova; permanece o defeito de atualização do memo”. Discutem a consulta de competência, mas o defeito apontado é envelhecimento do memo, não duplicação entre consumidores (`app/chamados/reconciliador.py:703`; `app/chamados/reconciliador.py:706`).
- **13963efb, item 8:** A, **VIU**: “a dispensa do veto não acompanha a decisão de renascer”; identifica o renascimento após mudança de estado entre leitura e trava, sem nova consulta ao veto (`app/chamados/reconciliador.py:798–799,843–848`; `app/chamados/models.py:666–687`). B, **EM PARTE**: “compartilhar o leitor da impressão não garante paridade integral com a emissão”; identifica outro furo de veto, por vínculo ausente, mas não a proteção dentro da porta (`app/ponto/services/furos_vetados.py:26`; `app/chamados/reconciliador.py:845`).

As demais respostas não apontam o defeito específico dos itens ainda imputados; afirmações de correção nas voltas posteriores não recebem crédito retroativo. Os achados novos abaixo são alegações dos revisores, com as linhas que eles citaram; sua procedência individual permanece **[nao verificado]**.

NOTA|O261-C_4da2943d|1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_4da2943d|2|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_4da2943d|3|existia|A=EM_PARTE|B=VIU
NOTA|O261-C_4da2943d|4|n/a|A=n/a|B=n/a
NOTA|O261-C_4da2943d|5|existia|A=NAO_VIU|B=EM_PARTE
NOTA|O261-C_4da2943d|6|existia|A=EM_PARTE|B=EM_PARTE
NOTA|O261-C_4da2943d|7|n/a|A=n/a|B=n/a
NOTA|O261-C_4da2943d|8|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_68a38548|1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_68a38548|2|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_68a38548|3|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_68a38548|4|n/a|A=n/a|B=n/a
NOTA|O261-C_68a38548|5|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_68a38548|6|existia|A=EM_PARTE|B=EM_PARTE
NOTA|O261-C_68a38548|7|n/a|A=n/a|B=n/a
NOTA|O261-C_68a38548|8|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_0ed50cc2|1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_0ed50cc2|2|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_0ed50cc2|3|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_0ed50cc2|4|n/a|A=n/a|B=n/a
NOTA|O261-C_0ed50cc2|5|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_0ed50cc2|6|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_0ed50cc2|7|n/a|A=n/a|B=n/a
NOTA|O261-C_0ed50cc2|8|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_22c08b8e|1|n/a|A=n/a|B=n/a
NOTA|O261-C_22c08b8e|2|n/a|A=n/a|B=n/a
NOTA|O261-C_22c08b8e|3|n/a|A=n/a|B=n/a
NOTA|O261-C_22c08b8e|4|n/a|A=n/a|B=n/a
NOTA|O261-C_22c08b8e|5|n/a|A=n/a|B=n/a
NOTA|O261-C_22c08b8e|6|n/a|A=n/a|B=n/a
NOTA|O261-C_22c08b8e|7|n/a|A=n/a|B=n/a
NOTA|O261-C_22c08b8e|8|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_13963efb|1|n/a|A=n/a|B=n/a
NOTA|O261-C_13963efb|2|n/a|A=n/a|B=n/a
NOTA|O261-C_13963efb|3|n/a|A=n/a|B=n/a
NOTA|O261-C_13963efb|4|n/a|A=n/a|B=n/a
NOTA|O261-C_13963efb|5|n/a|A=n/a|B=n/a
NOTA|O261-C_13963efb|6|n/a|A=n/a|B=n/a
NOTA|O261-C_13963efb|7|n/a|A=n/a|B=n/a
NOTA|O261-C_13963efb|8|existia|A=VIU|B=EM_PARTE
NOVO|O261-C_4da2943d|A|app/chamados/services/disputa_emissao.py:1634|Segundo laço do S128 contorna a guarda de tranca e carimba pergunta com via que a reabertura por competência não desfaz.
NOVO|O261-C_4da2943d|A|app/chamados/services/disputa_emissao.py:1659|Segundo laço carimba resposta sem testemunha e a retira do universo do materializador.
NOVO|O261-C_4da2943d|A|app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:43|Fixture sem corte personalizado não distingue competência da janela de mês civil.
NOVO|O261-C_4da2943d|A|app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:205|Idempotência compara poucos campos e permite relavratura com alteração de julgado_em.
NOVO|O261-C_4da2943d|A|app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:92|Casos MORTOS usam somente validada_em e não exercitam tranca nem resposta do colaborador.
NOVO|O261-C_4da2943d|A|app/chamados/reconciliador.py:1436|Reatar e c2 repetem o SELECT de entrega para a mesma competência.
NOVO|O261-C_4da2943d|B|app/chamados/services/disputa_emissao.py:1622|S128 alcança disputas de outros dias e competências, podendo carimbar uma irmã exportada apesar da cerca na pergunta de entrada.
NOVO|O261-C_4da2943d|B|app/chamados/services/disputa_emissao.py:1634|Segundo laço do S128 ignora a proteção da resposta sem testemunha e impede sua materialização posterior.
NOVO|O261-C_4da2943d|B|app/chamados/services/disputa_emissao.py:1647|Segundo laço vence a guarda de tranca e grava carimbo não abrangido pela reversão de competencia_trancada.
NOVO|O261-C_4da2943d|B|app/chamados/reconciliador.py:1457|Dois históricos que passam a casar pelo juiz impedem o despacho ao c1 e deixam a pergunta sem tratamento.
NOVO|O261-C_4da2943d|B|app/chamados/reconciliador.py:1436|Consulta de K6 no reatar é repetida pelo helper do c2 para o mesmo intervalo vivo.
NOVO|O261-C_4da2943d|B|app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:92|Fixture só mata perguntas por validada_em e não distingue C6 de um segundo juiz baseado nesse campo.
NOVO|O261-C_4da2943d|B|app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:95|Bateria não chama c2 diretamente e permite implementar o alargamento somente no wrapper.
NOVO|O261-C_4da2943d|B|app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:66|DNA usa ii/fi enquanto o juiz procura hii/hfi, sem comprovar leitura da lâmpada específica.
NOVO|O261-C_4da2943d|B|app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:133|R4 exportada, R4b, R5 e R6 preservam comportamento anterior e não constituem RED isolado contra o ancestral.
NOVO|O261-C_68a38548|A|app/chamados/reconciliador.py:702|Memo de K6 na instância do colaborador atravessa perguntas e pode conservar entrega antiga após exportação ou invalidação.
NOVO|O261-C_68a38548|A|app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:234|Idempotência integral da chamada direta contradiz a relavratura que renova julgado_em.
NOVO|O261-C_68a38548|B|app/chamados/reconciliador.py:1451|C7 é consultado antes de datar a pergunta e deixa o HIST escrever em dia trancado quando data_evento é nula.
NOVO|O261-C_68a38548|B|app/chamados/reconciliador.py:702|Memo de K6 persiste entre perguntas e não acompanha exportação ou invalidação concorrente.
NOVO|O261-C_68a38548|B|app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:183|R8 sempre fornece data_evento e não cobre a brecha de tranca com data nula.
NOVO|O261-C_68a38548|B|app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:197|Testes não exercitam mudança de entrega entre consultas na mesma instância. [nao verificado]
NOVO|O261-C_0ed50cc2|A|app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:264|Comparação integral falha com avanço do relógio; congelamento pode mascarar a relavratura de julgado_em.
NOVO|O261-C_0ed50cc2|B|app/chamados/reconciliador.py:1452|HIST filtra exclusivamente pelo dia no JSON e descarta chamado cuja célula informa o dia reconhecido por dia_do_fato.
NOVO|O261-C_0ed50cc2|B|app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:264|Teste exige igualdade integral embora a segunda costura renove julgado_em. [nao verificado]
NOVO|O261-C_22c08b8e|A|app/chamados/reconciliador.py:765|Reordenamento permite log de veto antes da cerca, contrariando a justificativa de ausência de efeitos colaterais.
NOVO|O261-C_22c08b8e|B|app/chamados/reconciliador.py:830|Veto de célula passa a chamar o logger em caminho exportado antes encerrado pela cerca antecipada.
NOVO|O261-C_13963efb|B|app/ponto/services/furos_vetados.py:26|Leitor retorna ausência de veto para vínculo nulo, divergindo da degeneração considerada pela emissão.
NOVO|O261-C_13963efb|B|app/chamados/reconciliador.py:843|Novo veto também altera nascimento e renascimento dos motivos C1 fora do alcance novo antes declarado.
NOVO|O261-C_13963efb|B|app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:186|R2b cobre apenas régua suspeita, intervalo e nascimento; não caracteriza C1, degeneração ou renascimento vetado.

## 30/10 15:02 · sessao 661803796d2c · BENCH-CORRECAO · censo · origem=Code

Considerei a existência em `61ae629c` conforme o gabarito fornecido, sem equiparar esse hash à cópia local: nela, `ec=None` ainda retorna `None` (`app/ponto/services/furos_vetados.py:26`). Nas referências ao anexo abaixo, a primeira linha é a que começa com “GABARITO”.

- **s1 — existia; A e B EM PARTE.** A identifica “lacuna de integração de `ec=None`”; B afirma “a fixture mantém vínculo válido; o teste chama somente o juiz”. Ambos discutem cobertura de testes sem vínculo, mas não identificam a quebra das fixtures dos 45 módulos registrada pelo gabarito (`app/chamados/tests/test_o261_veto_na_porta.py:302`, conforme respostas; `gab_O261-VETO-A.txt:2`).
- **s2 — existia; A e B EM PARTE.** A escreve “Resposta que reabria pode virar nota tardia”; B descreve “veto impede renascimento → resposta vira nota tardia”. Ambos reconhecem a mudança funcional, sem apontar os dois testes que continuam exigindo o comportamento anterior (`app/chamados/services/resposta_tardia.py:45`, conforme respostas; `gab_O261-VETO-A.txt:2`). As expectativas antigas aparecem na cópia em `app/chamados/tests/test_resposta_tardia.py:72` e `app/chamados/tests/test_c2_le_o_protesto_que_cobra.py:71`.
- **s3 — existia; A e B NÃO VIU.** Nenhuma resposta aponta a falta de inscrição `@bolha`; suas observações sobre juiz novo não identificam esse defeito de registro (`gab_O261-VETO-A.txt:2`; requisito do selo em `app/core/tests/test_selo_mapa_contrato.py:41`).
- **m1 — existia; A e B VIU.** A diz que a impressão “passa a conter `veto:VETO_DEGENERADA`” e “provoca rejulgamento”; B diz que ela “deixa de satisfazer o salto por impressão igual”. Ambos descrevem também a mudança no passivo e no c2. Isso identifica o mecanismo, mesmo sem reproduzir a quantidade medida (`app/ponto/services/cartorio.py:1013`; `app/ponto/services/cartorio.py:1083`; `app/ponto/services/cartorio.py:1092`; `app/ponto/services/furos_sem_cobranca.py:96`; `gab_O261-VETO-A.txt:3`).

**A correção permanece incompleta para A2-2, A2-3, A2-5 e A2-7:** o anexo apenas os referencia, sem descrever seu conteúdo. Sua correspondência com A/B está [nao verificado]. Não atribuí `n/a`, pois falta do anexo não demonstra inexistência (`gab_O261-VETO-A.txt:4`).

Os registros `NOVO` abaixo identificam achados adicionais aos quatro itens descritos. A exclusão de sobreposição com os itens ausentes de A2 está **[nao verificado]**; os endereços são os citados pelos revisores.

NOTA|O261-VETO-A_61ae629c|s1|existia|A=EM_PARTE|B=EM_PARTE
NOTA|O261-VETO-A_61ae629c|s2|existia|A=EM_PARTE|B=EM_PARTE
NOTA|O261-VETO-A_61ae629c|s3|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A_61ae629c|m1|existia|A=VIU|B=VIU
NOVO|O261-VETO-A_61ae629c|A|app/ponto/management/commands/reconciliar_grade.py:522|Grade reabre diretamente o chamado encerrado devolvido após recusa de renascimento.
NOVO|O261-VETO-A_61ae629c|A|app/chamados/services/disputa_emissao.py:1606|Callback de resolução reabre automaticamente o pai sem consultar o veto.
NOVO|O261-VETO-A_61ae629c|A|app/ponto/views.py:291|Entrada bloqueada nasce com contexto sem dia e dispensa a consulta ao juiz do veto.
NOVO|O261-VETO-A_61ae629c|A|app/ponto/management/commands/processar_alertas_avancados.py:121|Veto novo altera emissão N5 também com vínculo existente e régua suspeita.
NOVO|O261-VETO-A_61ae629c|A|app/ponto/management/commands/processar_alertas_turno.py:317|Marco crítico continua com alerta e justificativa quando recebe chamado encerrado cujo renascimento foi recusado.
NOVO|O261-VETO-A_61ae629c|A|app/chamados/views_arquivar.py:75|Reabertura humana não apresenta nem registra autorização específica sobre o veto.
NOVO|O261-VETO-A_61ae629c|A|app/ponto/services/furos_vetados.py:98|Vigia conta reaberturas humanas permitidas como violações da invariante automática.
NOVO|O261-VETO-A_61ae629c|A|app/ponto/management/commands/furos_vetados_por_regua.py:32|Agregação inclui None no conjunto e publica um vínculo quando não existe vínculo real.
NOVO|O261-VETO-A_61ae629c|A|app/chamados/tests/test_o261_veto_na_porta.py:100|Selo AST não acompanha alias transitivo da classe e deixa passar criação fora da porta.
NOVO|O261-VETO-A_61ae629c|A|app/chamados/tests/test_o261_veto_na_porta.py:168|Selo AST não reconhece criação através de módulo importado por from . import models as m.
NOVO|O261-VETO-A_61ae629c|A|app/chamados/tests/test_o261_veto_na_porta.py:170|Selo AST não detecta nascimento por construtor seguido de save.
NOVO|O261-VETO-A_61ae629c|A|app/chamados/tests/test_o261_veto_na_porta.py:168|Selo AST não detecta bulk_create através de alias do manager.
NOVO|O261-VETO-A_61ae629c|A|app/chamados/tests/test_o261_veto_na_porta.py:143|Selo AST não detecta reabertura por update com status recebido em kwargs.
NOVO|O261-VETO-A_61ae629c|A|app/chamados/tests/test_o261_veto_na_porta.py:163|Alias de abrir esconde do selo a chamada à porta junto de uma cópia externa do veto.
NOVO|O261-VETO-A_61ae629c|A|app/chamados/tests/test_o261_veto_na_porta.py:170|Selo não reconhece os reabridores automáticos por instância como escritores.
NOVO|O261-VETO-A_61ae629c|A|app/chamados/tests/test_o261_veto_na_porta.py:302|Teste sem vínculo exercita apenas o juiz e permite mutante permissivo na integração da porta. [nao verificado]
NOVO|O261-VETO-A_61ae629c|A|app/chamados/tests/test_o261_veto_na_porta.py:269|Testes não distinguem a exclusão de batida_ausente e intervalo_pendente do universo protegido. [nao verificado]
NOVO|O261-VETO-A_61ae629c|B|app/ponto/management/commands/reconciliar_grade.py:519|Grade reabre diretamente o chamado encerrado devolvido após recusa de renascimento.
NOVO|O261-VETO-A_61ae629c|B|app/chamados/services/disputa_emissao.py:1606|Receiver de fechamento aciona callback que reabre automaticamente o pai sem veto.
NOVO|O261-VETO-A_61ae629c|B|app/ponto/views.py:291|Entrada bloqueada nasce sem dia forte e escapa do veto; contador a separa em sem_dia.
NOVO|O261-VETO-A_61ae629c|B|app/ponto/management/commands/processar_alertas_turno.py:316|Marco crítico cobra por alerta e justificativa apesar de o chamado permanecer encerrado após o veto.
NOVO|O261-VETO-A_61ae629c|B|app/chamados/views_arquivar.py:75|Desarquivamento humano reabre sem aviso nem trilha específica do veto.
NOVO|O261-VETO-A_61ae629c|B|app/ponto/services/furos_vetados.py:98|Contador acusa como violação a exceção humana autorizada.
NOVO|O261-VETO-A_61ae629c|B|app/ponto/management/commands/processar_alertas_avancados.py:121|Veto novo altera emissão N5 com vínculo válido e régua suspeita.
NOVO|O261-VETO-A_61ae629c|B|app/ponto/management/commands/furos_vetados_por_regua.py:32|Conjunto de vínculos contendo apenas None produz contagem incorreta de um vínculo.
NOVO|O261-VETO-A_61ae629c|B|app/chamados/tests/test_o261_veto_na_porta.py:97|Selo AST deixa passar criação através de alias transitivo da classe.
NOVO|O261-VETO-A_61ae629c|B|app/chamados/tests/test_o261_veto_na_porta.py:168|Selo AST deixa passar criação através de módulo importado por from . import models as m.
NOVO|O261-VETO-A_61ae629c|B|app/chamados/tests/test_o261_veto_na_porta.py:170|Selo AST deixa passar nascimento por construtor seguido de save.
NOVO|O261-VETO-A_61ae629c|B|app/chamados/tests/test_o261_veto_na_porta.py:168|Selo AST deixa passar bulk_create através de alias do manager.
NOVO|O261-VETO-A_61ae629c|B|app/chamados/tests/test_o261_veto_na_porta.py:137|Selo AST deixa passar reabertura por update com status recebido em kwargs.
NOVO|O261-VETO-A_61ae629c|B|app/chamados/tests/test_o261_veto_na_porta.py:194|Catraca por quantidade permite substituir nome autorizado sem reduzir o passivo.
NOVO|O261-VETO-A_61ae629c|B|app/chamados/tests/test_o261_veto_na_porta.py:209|Uso de set esconde uma escrita adicional dentro de função já autorizada.
NOVO|O261-VETO-A_61ae629c|B|app/chamados/tests/test_o261_veto_na_porta.py:302|Teste sem vínculo não exercita integração com a porta e aceita mutante permissivo nessa integração. [nao verificado]
NOVO|O261-VETO-A_61ae629c|B|app/chamados/tests/test_o261_veto_na_porta.py:295|Teste negativo de criar não tem controle positivo correspondente e aceita recusa permanente de turno_aberto_24h. [nao verificado]

## 30/10 15:02 · sessao fdc65b427f91 · - · - · origem=Ronald (tmux)

Recomendo esta leitura estrita: reconhecer o sintoma, sem identificar o mecanismo específico do gabarito, vale **EM_PARTE**. [não verificado]

| Volta | Item 1 | Item 2 | Item 3 | Item 4 | Item 5 | Item 6 |
|---|---|---|---|---|---|---|
| 469d61ba | A/B NAO_VIU | A/B VIU | A/B EM_PARTE | A/B EM_PARTE | A/B EM_PARTE | n/a |
| 24f1cd69 | A/B NAO_VIU | A/B NAO_VIU | A/B NAO_VIU | A/B NAO_VIU | A/B NAO_VIU | A EM_PARTE; B NAO_VIU |
| 31b8cec0 | A/B NAO_VIU | A/B NAO_VIU | A/B NAO_VIU | A/B NAO_VIU | A/B NAO_VIU | A/B EM_PARTE |
| 691508dd | A/B NAO_VIU | A/B NAO_VIU | A/B NAO_VIU | A/B NAO_VIU | A/B NAO_VIU | A/B EM_PARTE |
| e8c314e0 | A/B NAO_VIU | A/B NAO_VIU | A/B NAO_VIU | A/B NAO_VIU | A/B NAO_VIU | A/B NAO_VIU |
| 06d7ad06 | A/B NAO_VIU | A/B NAO_VIU | A/B NAO_VIU | A/B NAO_VIU | A/B NAO_VIU | A/B NAO_VIU |

A tabela é julgamento sobre as respostas anexadas; as justificativas positivas seguem abaixo. A existência histórica dos itens depende da consolidação da cronologia. [não verificado]

- **469d61ba, item 2/A — VIU:** “O texto lê `lampadas[].instante`; o plantio continua recompondo marcos e escolhendo o primeiro candidato D/D+1 dentro de um envelope de 90 minutos”, acompanhado do contraexemplo 08:00→07:00. A descreve os dois critérios e uma divergência concreta. (`app/chamados/catalogo/query.py:65`; `app/chamados/juizes.py:305`; `app/chamados/juizes.py:308`, referências da resposta A.)

- **469d61ba, item 2/B — VIU:** “há fontes paralelas preexistentes”; o envelope depende de fallback que “escolhe escala ativa e lê o template”, enquanto o outro caminho usa “vínculo do dia e DNA congelado”. Negar um segundo juiz **novo** não equivale a negar as fontes divergentes existentes. (`app/chamados/catalogo/query.py:59`; `app/chamados/catalogo/query.py:67`; `app/chamados/juizes.py:301`; `app/chamados/juizes.py:1251`; `app/chamados/juizes.py:1256`, referências da resposta B.)

- **469d61ba, item 3/A — EM_PARTE:** “não exercitam intervalos, os seis emissores, telas, identidade persistida ou teto de consultas” e “RED contra o commit anterior: [não verificado]”. Identifica lacunas dos testes, mas não identifica o RED do texto por TypeError nem demonstra o RED histórico incompleto. (`app/chamados/tests/test_noturno_le_o_dia_do_instante.py:65`; `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:96`; `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:110`, referências da resposta A.)

- **469d61ba, item 3/B — EM_PARTE:** “não exercitam os seis emissores, a identidade persistida, os intervalos, `padrao` ou data com hora”; “Suíte Django e RED histórico são [não verificado]”. A ressalva sobre falta de verificação não denuncia, por si, a falha concreta do RED. (`app/chamados/tests/test_noturno_le_o_dia_do_instante.py:36`; `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:60`; `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:110`, referências da resposta B.)

- **469d61ba, item 4/A — EM_PARTE:** “Antes de procurar qualquer conflito, `classe_do_lote` calcula o plantio” e “Esse trabalho acontece por pergunta da fila”. Reconhece o custo por pergunta, sem localizar sua execução nas views da UI nem quantificá-lo. (`app/chamados/services/validacao.py:830`; `app/chamados/services/materializacao.py:266`; `app/chamados/services/materializacao.py:354`, referências da resposta A.)

- **469d61ba, item 4/B — EM_PARTE:** “acrescenta consultas” e “a derivação pode consultar a grade no crivo e novamente na escolha do tipo”. Também não fecha o percurso até as views da UI. (`app/chamados/services/materializacao.py:355`; `app/chamados/services/materializacao.py:266`; `app/chamados/services/validacao.py:830`, referências da resposta B.)

- **469d61ba, item 5/A e B — EM_PARTE:** ambos apontam intervalos sem testes; nenhum descreve o intervalo **diurno mudando de classe no lote**. A: “não exercitam intervalos”. B: “O DNA de teste deixa os intervalos vazios”. (`app/chamados/tests/test_noturno_le_o_dia_do_instante.py:36`; `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:65`, referências das respostas.)

- **24f1cd69, item 6/A — EM_PARTE:** “A propriedade calcula o dia pelo DNA atual”; pergunta existente retorna “sem atualizar `pergunta`”; “uma pergunta antiga cujo texto nomeia D pode ganhar cabeçalho D+1”. Identifica a combinação DNA atual/texto congelado, mas o exemplo é texto antigo, sem identificar regeneração posterior da célula. (`app/chamados/models.py:2172`; `app/chamados/models.py:2062`; `app/chamados/models.py:2084`, referências da resposta A.)

- **31b8cec0, item 6/A e B — EM_PARTE:** ambos reproduzem cabeçalho divergente do texto por contexto ISO com hora. É o mesmo tema de coerência cabeçalho/texto, com mecanismo diferente da regeneração. A: “a igualdade falha e retorna 22/10”. B: “a comparação reconstrói ... não coincide e o cabeçalho retorna 22/10”. (`app/chamados/catalogo/query.py:99`; `app/chamados/models.py:2176`, referências das respostas.)

- **691508dd, item 6/A e B — EM_PARTE:** ambos mostram que adicionar ponto final ao texto D+1 faz o cabeçalho voltar a D. Identificam a incoerência, sem apontar a célula regenerada como causa. A: “O ponto final quebra a igualdade com o produtor e aciona o fallback”. B: “acrescentar apenas `.` faz retornar 21/10”. (`app/chamados/models.py:2178`; `app/templates/chamados/partials/_card_atendimento.html:90`, referências das respostas.)

O N+1 de `p.celula` apontado em 31b8cec0 é achado novo, distinto do custo de `_ts_tipo_do_plantio` no item 4. Não o usaria para dar crédito nesse item. (`app/api/views.py:1043`; `app/api/views.py:1045`; `app/chamados/models.py:2172`, referências das respostas A/B.)

## 30/10 15:01 · sessao c148d26f9db9 · - · - · origem=Ronald (tmux)

Sugestão de notas enviada ao pai; os pontos que exigem cautela são:

- Item 4: `d6adf8c0=n/a`; `519fa2c2` e `af2cf55f=existia`, por inferência da ampliação dos testes entre versões. A cronologia efetiva do RED está **[nao verificado]**.
- Item 2: checagens negativas específicas como “não encontrei segundo juiz” sustentam **EM_PARTE** pela definição literal; mera menção a reutilizar leitor não identifica a divergência entre sugestões. (`app/escala/services/cadastro_realidade.py:336`; `app/escala/services/fase_na_tela.py:50`)
- Item 3: A/d6 aponta a ação sem identidade do vínculo e registra ausência de confirmação do caso real: **VIU**. B/d6 identifica somente a ausência de prova dos dias: **EM_PARTE**. (`app/escala/services/cadastro_realidade.py:488`; `app/escala/tests/test_o261v_fase_na_lista.py:23`)
- Item 5: A e B/d6 inventariam leitores de `lista()`, mas não denunciam a omissão na LEI-AKITA: **EM_PARTE**. (`app/escala/management/commands/fechar_avisos_regua_na_lista.py:32`; `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:40`)

## 30/10 15:01 · sessao ee86488cf02c · - · - · origem=Ronald (tmux)

A cópia local contém o registro final dos seis itens em `app/docs/RELATO.md:13`; o mesmo registro situa o achado final em `44e6a8a1`. A cronologia resumida relaciona `469d61ba`, `31b8cec0`, `691508dd` e `44e6a8a1`, mas não conserva as seis árvores para comparação direta (`app/docs/RELATO.md:24`).

Temporalidade recomendada, por inferência dos anexos:

| Item | 469d61ba | Demais voltas |
|---|---|---|
| 1 — contexto usa dia do marco | existia | existia |
| 2 — DNA versus escala/envelope | existia | existia |
| 3 — RED parcial | existia | existia |
| 4 — custo do plantio na UI | existia | existia |
| 5 — intervalo diurno muda de classe | existia | existia |
| 6 — regeneração descasa cabeçalho/texto | n/a | existia |

A primeira volta já descreve produtor lendo o DNA, plantio dentro da classificação e ausência de cobertura/RED; a segunda já descreve `dia_mostrado` consultando DNA atual enquanto o texto permanece gravado. São evidências dos próprios anexos; a introdução exata de cada implementação por commit não foi verificada. [não verificado] [nao verificado]

Há duas ressalvas para a consolidação:

- Item 6 em `469d61ba`: o cabeçalho ainda usa `data_evento`, portanto a contradição D/D+1 apontada naquela volta é outro defeito, anterior à propriedade que passa a acompanhar a célula. Isso sustenta `n/a` para o mecanismo específico de regeneração. [não verificado]
- Item 3: as respostas da primeira volta mencionam RED histórico não verificado e lacunas de cobertura, sem identificar o TypeError; a classificação entre `VIU` e `EM_PARTE` depende de tratar “RED parcial” como achado composto ou exigir o mecanismo específico. `app/docs/RELATO.md:13` registra que a triagem posterior considerou o item 3 visto.

O registro posterior atribui item 2 e item 3 a “viu e a triagem descartou”, item 4 a parcial e item 6 a parcial, mas isso é relato de classificação anterior, não substitui a leitura cega de A e B (`app/docs/RELATO.md:13`).

O código local confirma as premissas anteriores à cura: fábrica grava o dia recebido (`app/chamados/services/fabrica_da_celula.py:122`); detector grava a saída esperada (`app/chamados/detector_anomalias.py:45`, `app/chamados/detector_anomalias.py:49`); emissão deriva identidade do contexto (`app/chamados/services/disputa_emissao.py:347`); plantio usa escala vigente e envelope (`app/chamados/juizes.py:300`, `app/chamados/juizes.py:305`, `app/chamados/juizes.py:308`). A cópia não contém a implementação NOTURNO descrita nos anexos: produtor tem assinatura antiga (`app/chamados/catalogo/query.py:45`) e lote ainda verifica somente o instante construído de `data_evento` (`app/chamados/services/validacao.py:817`).

## 30/10 15:01 · sessao c7e3163faa7a · - · - · origem=Ronald (tmux)

Evidências temporais disponíveis:

- Os seis achados de `df863705` estão registrados em `app/docs/RELATO.md:83`; `22c08b8e` é identificado como a “cura dos 6” em `app/docs/RELATO.md:84`. Para esses seis defeitos, as três primeiras voltas são anteriores à cura; nas duas últimas, o registro informa correção. Ancestralidade exata dos hashes antigos: [nao verificado].
- O veto ausente no c2 precedia a suíte: o relato identifica expressamente um “bug no caminho escondido enquanto o c2 recusava intervalo”, corrigido em `13963efb`. Portanto, sua descoberta posterior não justifica automaticamente `n/a` nas primeiras voltas. (`app/docs/RELATO.md:85`, `app/docs/RELATO.md:86`)
- A reprovação da ordem da docstring aconteceu sobre `3ab94852`; `e281d2f1` corrigiu apenas a lista, mantendo o código executável. O registro não informa em qual hash a ordem ficou errada; existência desse defeito específico nas voltas anteriores: [nao verificado]. (`app/docs/RELATO.md:87`)
- O relato distingue a alteração do comentário da cerca em `3ab94852` da correção posterior da docstring; não permite concluir que o erro da docstring nasceu em `3ab94852`. (`app/docs/RELATO.md:87`)

Item 6, composição duplicada:

- Nenhuma resposta anterior ao opus nomeia a duplicação **entre vigia e reconciliador**, acompanhada da divergência sem data; atribuir `VIU` a elas não encontra suporte no texto fornecido. [nao verificado por arquivo da resposta]
- As observações sobre janela nos testes, K6 consultado duas vezes e memo envelhecido tratam problemas próximos; são candidatos a `EM_PARTE` pela definição literal “toca o tema sem o defeito”. [nao verificado por arquivo da resposta] [nao verificado]
- A composição atual documenta expressamente que antes existiam duas versões e que o `None` cabe a cada consumidor. (`app/chamados/services/vigia_fio.py:458`, `app/chamados/services/vigia_fio.py:464`)
- A cerca atual delega à composição única. (`app/chamados/reconciliador.py:711`)

Ambiguidade importante do item 8:

- Se o item significa estritamente **“c2 não consulta veto”**, ele está corrigido em `13963efb`; nesse recorte, a última volta recebe `n/a`. (`app/docs/RELATO.md:86`, `app/chamados/reconciliador.py:852`)
- Se significa **“a porta de nascimento/renascimento não garante o veto”**, permanece após essa cura: o censo identifica ausência na porta e no renascimento. Nesse recorte, a corrida descrita por A em `13963efb` demonstra precisamente a brecha estrutural. (`app/docs/RELATO.md:48`, `app/docs/RELATO.md:49`, `app/chamados/models.py:666`, `app/chamados/models.py:685`)
- O defeito de B em `13963efb`, vínculo ausente liberado, possui causa própria: `motivo_do_veto` retorna `None` quando `ec is None`. Pode ser listado como novo; sua ocorrência real não foi demonstrada. (`app/ponto/services/furos_vetados.py:26`; ocorrência [nao verificado])

## 30/10 15:01 · sessao f018b9d0bbb4 · - · - · origem=Ronald (tmux)

- **Custo (item 3):** 4da A=EM_PARTE: “consulta de exportação duplicada por intervalo vivo” (`app/chamados/reconciliador.py:703,709,749,1436`, referências da resposta); B=VIU: “C6 também passou a anteceder as recusas do c1” e “esse cenário não mede o custo novo” (`app/chamados/reconciliador.py:394`; `app/ponto/tests/test_selo_chokepoint_escrita.py:239`, referências da resposta). Nas voltas 68 e 0ed, A/B=NAO_VIU. Em 22 e 139, n/a: cerca reordenada e selo acrescentado, conforme `app/docs/RELATO.md:84`; fonte atual confirma cerca depois dos vetos em `app/chamados/reconciliador.py:858`.
- **Guarda morta (item 4):** nenhuma resposta das primeiras três aponta o defeito. Contudo, **não há prova de que a guarda existisse nesses hashes**: 68 A e 0ed A/B descrevem relavratura incondicional ao reapontar ao mesmo destino, sugerindo que a guarda foi introduzida depois para curar a idempotência; hash de introdução [nao verificado]. Recomendo n/a nas primeiras três, explicitando a inferência e a limitação. Em 22/139, n/a: guarda apagada, `app/docs/RELATO.md:84`.
- **R1 sem mordida (item 5):** primeiras três A/B=NAO_VIU. A única aproximação literal é 4da B: “R1/R2/R3 [...] possuem asserções da mudança”; isso afirma cobertura, sem revelar a lacuna do renascimento. Se “toca o tema” incluir afirmação favorável, B=EM_PARTE nesse único caso; recomendo NAO_VIU. Em 22/139, n/a: cura registrada em `app/docs/RELATO.md:84`; asserções atuais exigem chamado renascido e recusa nomeada em `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:151` e `:163`.

A cronologia consolidada está em `app/docs/RELATO.md:81`–`:87`; ela confirma o achado no df863705, mas não demonstra retrospectivamente que a guarda morta existia nas três primeiras árvores.

## 30/10 15:01 · sessao be60c8cf67a9 · CALENDARIO-HOJE · verificador · origem=Code

**Parecer: reprova; cura parcial.** O delta resolve o caso específico de `nunca_bateu` até hoje, mas ainda permite divergências entre calendário e retenção. ([calendario.py:802](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/colaboradores/services/calendario.py:802), [calendario.py:170](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/colaboradores/services/calendario.py:170), [export.py:1237](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/folha/export.py:1237))

**1. Divergência curada?**

Para trabalho sem batida, sem cobertura abonadora, data até hoje e `nunca_bateu` lavrado: **sem achado**. `_vv` impede `previsto`; o veredito resulta em `falta`, compatível com a retenção `espelho_nunca_bateu`. ([calendario.py:69](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/colaboradores/services/calendario.py:69), [calendario.py:177](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/colaboradores/services/calendario.py:177), [calendario.py:802](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/colaboradores/services/calendario.py:802), [export.py:1235](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/folha/export.py:1235))

Persistem estes contraexemplos condicionais:

| Estado | Calendário × folha |
|---|---|
| Juíza não encerrou; `concorde` lavrado; trabalho sem batida, sem cobertura, não isento | `_vv` desliga `previsto`, mas a tradução para `ok` é descartada: fica **`falta`**. Esse veredito **não retém** pelo leitor de células. O teste antigo exige expressamente essa `falta`. ([calendario.py:170](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/colaboradores/services/calendario.py:170), [test_calendario_ata_decide.py:38](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/colaboradores/tests/test_calendario_ata_decide.py:38), [export.py:1215](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/folha/export.py:1215)) |
| Mesmo estado, com `trabalhou` pela via humana | Também fica **`falta`**, pois sofre o mesmo veto ao `ok`; esse veredito não integra os acusadores que retêm. A via humana real chama-se **`admin`**. ([calendario.py:71](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/colaboradores/services/calendario.py:71), [calendario.py:170](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/colaboradores/services/calendario.py:170), [models.py:1735](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/escala/models.py:1735), [veredito_celula.py:76](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/ponto/services/veredito_celula.py:76)) |
| Dia passado, trabalho, juíza não encerrou, `veredito=None`, mesmo com ata | Continua **`previsto`**, mas a folha retém **`sem_celula`**, sem consultar a ata nesse ramo. Ata sem veredito é permitida: `carimbar_precedencia` grava somente `ata`. ([calendario.py:521](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/colaboradores/services/calendario.py:521), [calendario.py:802](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/colaboradores/services/calendario.py:802), [export.py:1237](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/folha/export.py:1237), [celula.py:732](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/ponto/portas/celula.py:732)) |
| Data futura com `nunca_bateu` lavrado | Continua **`previsto`** por `data > hoje`; o veredito não vence esse status. A retenção acusadora não possui guarda temporal e pode reter o dia. ([calendario.py:802](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/colaboradores/services/calendario.py:802), [calendario.py:177](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/colaboradores/services/calendario.py:177), [export.py:1215](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/folha/export.py:1215)) |

No caso com ata positiva e nenhum turno, a palavra pode ser “Sem turno pareado”; isso não corrige o `status` usado na cor e nos contadores. ([dia_decidido.py:375](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/ponto/services/dia_decidido.py:375), [_calendario_grade.html:75](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/templates/colaboradores/partials/_calendario_grade.html:75), [calendario.py:943](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/colaboradores/services/calendario.py:943))

**2. Aval e MORDE**

**Sem achado nas premissas dos testes:** hoje antes do turno, sem veredito, segue `previsto`; depois do fim, sem batida, segue `falta`. A juíza compara o instante com o fim previsto, e o calendário preserva esses dois ramos. ([turnos.py:2102](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/ponto/turnos.py:2102), [calendario.py:132](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/colaboradores/services/calendario.py:132), [test_calendario_hoje_nao_e_falta.py:42](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:42), [test_calendario_hoje_nao_e_falta.py:61](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:61))

**3. Demais pendências**

**Chaves incompatíveis: persiste.** O cartório entrega `(pk,data)`; o lote repassa ao carregador, que recompõe `(pk,(pk,data))`. A busca da geradora espera `(pk,data)`. ([cartorio.py:853](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/ponto/services/cartorio.py:853), [turnos.py:2131](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/ponto/turnos.py:2131), [turnos.py:1928](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/ponto/turnos.py:1928), [alimentacao.py:237](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/escala/alimentacao.py:237))

**Dependência da janela: persiste.** DNA vazio vira `None`; DNA não vazio sem a véspera impede o retorno dos marcos do template nesse ramo. ([turnos.py:1949](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/ponto/turnos.py:1949), [turnos.py:1297](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/ponto/turnos.py:1297))

**Tooltip: persiste.** “Turno em curso” continua aplicado antes do início e em `sem_previsto`; a nova guarda `data <= hoje` repete uma condição já presente em `_em_curso`. ([calendario.py:62](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/colaboradores/services/calendario.py:62), [calendario.py:707](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/colaboradores/services/calendario.py:707), [calendario.py:878](/home/ronald/codex_ws/cfa186cea5de736feb8874c928dfd675945c9601/app/colaboradores/services/calendario.py:878))

Execução integrada Django, ocorrência e quantidade dos estados em produção, pagamento ou desconto efetivo: **[nao verificado]**.

## 30/10 15:00 · sessao c7f1e7d087f8 · - · - · origem=Ronald (tmux)

**A cura é parcial.** Para trabalho sem batida, sem cobertura abonadora e data até hoje, `nunca_bateu` lavrado retira a condição `futuro`, produz `falta` e continua retendo como `espelho_nunca_bateu`. (`app/colaboradores/services/calendario.py:69,132-142,177-178,802`; `app/folha/export.py:1215-1236`)

Persistem estes contraexemplos de código:

| Estado condicionado | Calendário | Retenção da folha | Referências |
|---|---|---|---|
| Juíza não encerrou; `concorde` lavrado; trabalho sem batida; não isento; sem cobertura | `falta`: `_vv` desliga `futuro`, mas a tradução `ok` é descartada | Esse dia não retém | `calendario.py:132-142,170-171,802`; `models.py:1735-1736`; `export.py:1215-1238` |
| Mesmo estado com `trabalhou`, via humana | Mesma falsa `falta` | Esse dia não retém | `calendario.py:70-71,170-171,802,810-811`; `export.py:1215-1238` |
| Dia passado, `trabalha=True`, juíza não encerrou, `veredito=None`, mesmo possuindo ata | `previsto` | Retém `sem_celula` | `calendario.py:521-524,707,802`; `export.py:1237-1238` |
| Data futura com veredito acusador lavrado | Continua `previsto` | Pode reter, porque o ramo acusador não tem guarda temporal | `calendario.py:133-134,177,802`; `export.py:1215-1236` |

O primeiro contraexemplo já está explicitamente esperado pelo teste antigo: `concorde` sem batida produz `falta`; o teste da folha exige que `concorde` não retenha. (`app/colaboradores/tests/test_calendario_ata_decide.py:38-41`; `app/folha/tests/test_borda_celula.py:40-42`)

A via humana real chama-se **`admin`**. O classificador recebe `veredito_via`, mas não usa esse campo; a porta aceita `trabalhou` sem verificar existência de batida. O despachante normalmente materializa batidas, portanto o contraexemplo deve ser apresentado como estado condicionado, sem afirmar que ocorre em produção. (`app/ponto/services/veredito_celula.py:75-76,299-308`; `app/colaboradores/services/calendario.py:106-183,811`; `app/ponto/portas/celula.py:682-697`)

A ata sem veredito é um estado permitido pelo código: `carimbar_precedencia` pode gravar somente `ata`, sem lavrar veredito. (`app/ponto/portas/celula.py:732-744`)

Para o exemplo `concorde`, sem cobertura e sem ata positiva, o template não esconde o problema: a palavra do dia fica vazia e o status pinta fundo vermelho/ícone de falta. (`app/ponto/services/dia_decidido.py:370-396,426-427`; `app/templates/colaboradores/partials/_calendario_grade.html:72,98-102`)

**Limite:** ausência de motivo de retenção nesse leitor não comprova pagamento ou desconto; execução e ocorrência dos contraexemplos em produção: **[nao verificado]**.

## 30/10 14:59 · sessao 46c312b8da3f · - · - · origem=Ronald (tmux)

1. **Aval e MORDE: sem achado no código.** Sem veredito, antes do fim previsto a juíza retorna falso; o calendário marca `futuro=True` e classifica trabalho sem batida como `previsto`. Depois do fim, sem turno aberto, retorna verdadeiro e alcança `falta` nas mesmas premissas dos testes. Referências: `app/ponto/turnos.py:2097–2105`; `app/colaboradores/services/calendario.py:707,802,132–142`. Os testes existentes cobrem hoje às 13h antes do turno 18h–06h e o dia seguinte às 11h: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:23–26,42–45,61–63`. Execução: **[nao verificado]**.

2. **Chaves incompatíveis: pendência herdada persiste.** Cartório fornece `{(pk,data): célula}` a `dias_encerrados`, que repassa sem adaptação para `turnos_do_colab`; este trabalha com `{data: célula}` e recompõe `(_cid,_d)`, produzindo `(pk,(pk,data))`. A consulta de geradora espera `(pk,data)` e perde a célula nesse trecho. Referências: `app/ponto/services/cartorio.py:853`; `app/ponto/turnos.py:2131,1852,1880–1882,1928–1932`; `app/escala/alimentacao.py:237–254`. Ocorrência/impacto em produção: **[nao verificado]**.

3. **Dependência da janela: pendência herdada persiste.** Mapa DNA vazio vira `None`; mapa não vazio sem véspera retorna `None` em vez dos marcos do template. Logo, o alinhamento local de janelas não cura a diferença de alimentação entre chamadas largas e unitárias. Referências: `app/ponto/turnos.py:1949,1297–1300,1973–1976`; calendário alinhado em `app/colaboradores/services/calendario.py:310,705–706`. Contraexemplo executado e incidência: **[nao verificado]**.

4. **Tooltip: permanece impreciso.** “Turno em curso, ainda sem batida” continua tanto antes do início quanto em `sem_previsto`; o delta só protege o futuro por data, já excluído de `_em_curso`. Referências: `app/colaboradores/services/calendario.py:62,707,878–879`; `app/ponto/turnos.py:2102–2105`; o teste exige essa frase às 13h para turno que começa às 18h em `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:42–45`.

## 30/10 14:57 · sessao e70fd78a26cb · BENCH-CORRECAO · censo · origem=Code

Itens: **1** custo no request; **2** dois juízes de âncora; **3** dias reais/ação sobre vínculo errado; **4** RED restrito à primeira versão; **5** censo LEI-AKITA incompleto. (`app/docs/RELATO.md:19`)

Considerei 1, 2, 3 e 5 existentes nas três voltas, pela integração descrita desde a primeira e pelas autoridades já presentes na cópia. A retroação da declaração documental do item 5 não tem comprovação independente por hash. (`app/escala/services/cadastro_realidade.py:48`, `app/escala/services/fase_12x36.py:45`, `app/escala/services/fase_na_tela.py:50`, `app/docs/RELATO.md:23`; cronologia exata: [não verificado])

Para **4**, adoto **n/a na primeira volta**: a insuficiência específica surge diante das versões ampliadas, descritas na segunda volta. Nas duas seguintes, considero existente conforme o gabarito final; a ausência de outro RED intermediário permanece [não verificado]. (`app/docs/RELATO.md:19`, `app/docs/RELATO.md:22`)

As referências nas tabelas são as citadas pelas respostas dos anexos; sua correspondência às árvores históricas está [não verificado].

**Volta d6adf8c0**

| Item | A | B | Trecho que sustenta o crédito |
|---|---|---|---|
| 1 | VIU | VIU | A: “cada chamada da tela/PDF e dos dois comandos agora executa o detector global”. B: “Cada HTML/PDF e os commands C7/C8 passam a pagar a leitura de toda a frota”. Ambos descrevem o defeito. (`app/escala/services/cadastro_realidade.py:336`) |
| 2 | NAO_VIU | NAO_VIU | Nenhuma resposta identifica os dois juízes de âncora. [não verificado] |
| 3 | NAO_VIU | EM_PARTE | B: “C10 pode acusar um vínculo sob o cabeçalho de outro” e “ação sem identificar qual corrigir”. Identifica divergência entre vínculos ativos, sem diagnosticar o vínculo histórico dos dias e a regeneração do passado. (`app/escala/services/fase_12x36.py:114`, `app/escala/services/cadastro_realidade.py:488`; distinção: `app/docs/RELATO.md:18`) |
| 4 | n/a | n/a | Aplicabilidade temporal ressalvada acima. (`app/docs/RELATO.md:19`, `app/docs/RELATO.md:22`; cronologia: [não verificado]) |
| 5 | EM_PARTE | EM_PARTE | A: “ambos continuam selecionando explicitamente C7/C8”. B: “Os commands continuam exigindo especificamente C7/C8 para fechar”. Conferem consumidores, mas não apontam sua ausência na declaração LEI-AKITA. (`app/escala/management/commands/fechar_avisos_regua_na_lista.py:32`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:40`, `app/docs/RELATO.md:23`) |

**Volta 519fa2c2**

| Item | A | B | Trecho que sustenta o crédito |
|---|---|---|---|
| 1 | NAO_VIU | NAO_VIU | Não apontam o custo da varredura global no request. [não verificado] |
| 2 | NAO_VIU | EM_PARTE | B: “não identifiquei segundo juiz”. Toca a unicidade, mas não compara os dois calculadores de âncora. (`app/escala/services/cadastro_realidade.py:330`; autoridades: `app/escala/services/fase_12x36.py:45`, `app/escala/services/fase_na_tela.py:50`) |
| 3 | NAO_VIU | EM_PARTE | B: “presença dos quatro dias reais do col451: **[nao verificado]**”. Reconhece a prova pendente, sem identificar a ação necessária sobre o vínculo do passado. [não verificado] |
| 4 | EM_PARTE | EM_PARTE | A: “execução verde e RED histórico não foram verificados”. B: “Execução verde, RED histórico [...] **[nao verificado]**”. Não afirmam que o RED cobre somente a primeira versão. [não verificado] | [nao verificado]
| 5 | NAO_VIU | NAO_VIU | Não apontam a omissão dos leitores de `lista()` na declaração LEI-AKITA. [não verificado] |

**Volta af2cf55f**

| Item | A | B | Trecho que sustenta o crédito |
|---|---|---|---|
| 1 | NAO_VIU | NAO_VIU | Não apontam o custo no request. [não verificado] |
| 2 | EM_PARTE | NAO_VIU | A: “sem cálculo próprio de fase”. Examina a autoridade do adaptador, mas não confronta a sugestão do leitor com a da porta. (`app/escala/services/cadastro_realidade.py:331`; autoridades: `app/escala/services/fase_12x36.py:45`, `app/escala/services/fase_na_tela.py:50`) |
| 3 | EM_PARTE | NAO_VIU | A: “resultado real do col451: **[não verificado]**”. B discute serialização e vínculo iterado, sem examinar os dias reais ou o vínculo histórico. [não verificado] |
| 4 | EM_PARTE | NAO_VIU | A: “Execução da suíte, RED histórico [...] **[não verificado]**”. B registra somente “Execução dos testes: [não verificado]”, sem tratar a cobertura histórica do RED. [não verificado] | [nao verificado]
| 5 | NAO_VIU | EM_PARTE | B: “Trocar o pareamento alcançaria pauta, fila por causa, propositor e mensageria”. Faz censo do leitor de fase, mas não alcança os consumidores omitidos de `lista()`. (`app/escala/services/fase_12x36.py:223`, `app/chamados/services/fila_por_causa.py:173`, `app/docs/RELATO.md:23`) |

Não conto como achado novo o candidato de A/d6 sobre falha silenciosa, pois A declara “**Não fechei como achado**”. O conflito de vínculos de B/d6 recebeu crédito parcial no item 3, sem duplicação abaixo. (`app/escala/services/cadastro_realidade.py:603`, `app/escala/services/fase_12x36.py:114`)

Os registros NOVO reproduzem alegações fora do gabarito, inclusive dívidas anteriores; procedência histórica independente: [não verificado].

NOTA|O261-V-FASE_d6adf8c0|1|existia|A=VIU|B=VIU
NOTA|O261-V-FASE_d6adf8c0|2|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-V-FASE_d6adf8c0|3|existia|A=NAO_VIU|B=EM_PARTE
NOTA|O261-V-FASE_d6adf8c0|4|n/a|A=n/a|B=n/a
NOTA|O261-V-FASE_d6adf8c0|5|existia|A=EM_PARTE|B=EM_PARTE
NOTA|O261-V-FASE_519fa2c2|1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-V-FASE_519fa2c2|2|existia|A=NAO_VIU|B=EM_PARTE
NOTA|O261-V-FASE_519fa2c2|3|existia|A=NAO_VIU|B=EM_PARTE
NOTA|O261-V-FASE_519fa2c2|4|existia|A=EM_PARTE|B=EM_PARTE
NOTA|O261-V-FASE_519fa2c2|5|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-V-FASE_af2cf55f|1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-V-FASE_af2cf55f|2|existia|A=EM_PARTE|B=NAO_VIU
NOTA|O261-V-FASE_af2cf55f|3|existia|A=EM_PARTE|B=NAO_VIU
NOTA|O261-V-FASE_af2cf55f|4|existia|A=EM_PARTE|B=NAO_VIU
NOTA|O261-V-FASE_af2cf55f|5|existia|A=NAO_VIU|B=EM_PARTE
NOVO|O261-V-FASE_d6adf8c0|B|app/escala/services/cadastro_realidade.py:599|Falha do leitor C10 apenas gera log, sem indicador que distinga leitura incompleta de ausência de achados.
NOVO|O261-V-FASE_d6adf8c0|B|app/escala/tests/test_o261v_fase_na_lista.py:111|Teste aceita datas ISO erradas ou repetidas porque verifica formato e comprimento sem expectativa independente. [nao verificado]
NOVO|O261-V-FASE_d6adf8c0|B|app/escala/tests/test_o261v_fase_na_lista.py:128|Teste do PDF verifica apenas o prefixo PDF e permite omitir C10 e suas datas. [nao verificado]
NOVO|O261-V-FASE_d6adf8c0|B|app/escala/tests/test_o261v_fase_na_lista.py:170|Mock vazio continuaria verde sem a integração, contrariando a afirmação de que ambos os casos ficariam vermelhos. [nao verificado]
NOVO|O261-V-FASE_d6adf8c0|B|app/escala/services/fase_12x36.py:146|Dívida anterior: leitor pareia batidas cruas sem o papel da ata, em vez de consumir turnos_do_colab.
NOVO|O261-V-FASE_519fa2c2|A|app/escala/services/cadastro_realidade.py:112|C10 anuncia apenas 12x36, embora incorpore também achados 24x48.
NOVO|O261-V-FASE_519fa2c2|A|app/escala/services/fase_12x36.py:175|Dívida anterior: parear_turnos é chamado sem papel_por_minuto e não incorpora a autoridade da ata.
NOVO|O261-V-FASE_519fa2c2|B|app/escala/services/cadastro_realidade.py:112|Descrição e contador C10 anunciam 12x36 para um universo que também inclui 24x48.
NOVO|O261-V-FASE_519fa2c2|B|app/escala/services/cadastro_realidade.py:602|Permanece a falha silenciosa de C10, compartilhada com outros leitores, sem indicador de leitura incompleta.
NOVO|O261-V-FASE_519fa2c2|B|app/escala/services/fase_12x36.py:175|Dívida anterior: leitor consulta batidas e chama diretamente o pareador, em vez de turnos_do_colab.
NOVO|O261-V-FASE_af2cf55f|A|app/escala/services/fase_12x36.py:175|Dívida anterior da ata permanece no pareamento do leitor; o inventário aponta migração para turnos_do_colab.
NOVO|O261-V-FASE_af2cf55f|B|app/escala/services/fase_12x36.py:146|Dívida anterior: parear_turnos sem papel_por_minuto não recebe o papel da ata carregado por turnos_do_colab.

_611 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
