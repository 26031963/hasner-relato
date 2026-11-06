# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 14:19** · publicado em **06/11/2026 14:20** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 14:19 · sessao 6af08ca7b704 · - · - · origem=Ronald (tmux)

O limite permanece: `_medido_em` lê somente `fonte_do_medido`, enumera a glob e devolve a maior data, sem consultar `prova`. `bin/gerar_estado.py:47`, `bin/gerar_estado.py:51`, `bin/gerar_estado.py:56`.

O selo atual valida correspondência com a glob e um único arquivo citado entre leitores; não chama `_medido_em`. `app/core/tests/test_r1_termometro_uma_medicao.py:33`, `app/core/tests/test_r1_termometro_uma_medicao.py:50`, `app/core/tests/test_r1_termometro_uma_medicao.py:67`.

Reproduzi em memória, sem arquivos nem bytecode: os cinco testes passam; com glob simulada contendo arquivos de 06/11 e 07/11, a função real retorna `{'R1': '2026-11-07'}`, mantendo provas em 06/11. Bases da reprodução: `app/core/tests/test_r1_termometro_uma_medicao.py:49`, `app/core/placar_estrutural.py:120`, `bin/gerar_estado.py:56`. Existência real do arquivo de 07/11: [nao verificado].

Proposta de selo: chamar o publicador real com glob simulada e provas sintéticas, cobrindo arquivo único coincidente, candidato posterior sem atualização da prova, dois arquivos da mesma data e ausência de arquivo datado. Comparar identidade de arquivo, não apenas data: o contrato atual exige uma única medição, não somente datas iguais. `app/core/tests/test_r1_termometro_uma_medicao.py:11`, `app/core/tests/test_r1_termometro_uma_medicao.py:70`.

Só acrescentar esse teste deixaria o caso posterior vermelho; fechar o limite exige mudar a seleção ou publicar explicitamente a divergência. A função atual escolhe o máximo e seu chamador usa esse resultado para data, cadência e veredito. `bin/gerar_estado.py:56`, `bin/gerar_estado.py:80`, `bin/gerar_estado.py:81`, `bin/gerar_estado.py:84`, `bin/gerar_estado.py:116`.

Custo estimado: baixo para caracterizar com teste isolado; maior para escolher e implementar a política de publicação e verificar seus consumidores. Prazo preciso: [nao verificado]. Riscos inferidos: um selo que consulte arquivos reais pode reprovar a cópia sem logs; simplesmente usar a prova pode esconder medição mais recente; lançar exceção indiscriminada pode substituir todo o placar por “indisponível”. Bases: `bin/gerar_estado.py:51`, `bin/gerar_estado.py:56`, `bin/gerar_estado.py:86`.

Minha recomendação é tratar a cura como extensão do **publicador**, acompanhada de teste de integração R1: a autoridade de I/O é explicitamente do publicador, enquanto o placar recebe data e relógio pelo chamador e permanece puro. `bin/gerar_estado.py:34`, `app/core/placar_estrutural.py:618`, `app/core/placar_estrutural.py:623`. A divisão formal da obra em outro documento: [nao verificado].

## 06/11 14:19 · sessao 9881bdce649f · - · - · origem=Ronald (tmux)

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `SOMA.fonte` | Declaração em `app/core/placar_estrutural.py:108-109` | Copiado por `placar()` em `app/core/placar_estrutural.py:634-643`; selo genérico varre campos string em `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:65-75`. O publicador só menciona o campo no rodapé, sem renderizar seu valor: `bin/gerar_estado.py:121-123`. |
| `R1.numero` | Declaração em `app/core/placar_estrutural.py:118` | Presença usada no estado efetivo em `app/core/placar_estrutural.py:637-639`; cópia em `:635`; renderização integral em `bin/gerar_estado.py:112-117`; selo genérico em `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:65-75`. |
| Seis `ETAPAS[*].prova` | `app/core/espelho_verdade.py:49`, `:82`, `:169`, `:173`, `:177`, `:179` | Presença para validar FEITO em `app/core/espelho_verdade.py:201-203`; publicação limitada a 150 caracteres em `bin/gerar_estado.py:164`; censo e validação de caminhos em `app/core/tests/test_r1_termometro_uma_medicao.py:45-46`, `:64`, `:69-71`. |

Não identifiquei quebra de execução na análise estática: continuam strings nas mesmas chaves; os consumidores usam presença, cópia ou formatação, e o selo R1 examina os caminhos nas provas. `app/core/placar_estrutural.py:635-643`; `app/core/espelho_verdade.py:201-203`; `bin/gerar_estado.py:116-117`, `:164`; `app/core/tests/test_r1_termometro_uma_medicao.py:64-71`.

O recorte de conteúdo permanece imposto pelo publicador, independentemente da redação das provas. `bin/gerar_estado.py:164`.

Handoff e selo R6 leem outros dados: regex da frase dos contratos e `R6.numero`, respectivamente. `bin/handoff_sessao.sh:179-181`; `app/core/tests/test_selo_r6_frase_do_juiz.py:17-24`.

Execução Django e selos host: [nao verificado].

## 06/11 14:17 · sessao 8bb22732cf87 · H12H06-B-PAUTA-08 · verificador · origem=Code

**Não considero a obra pronta:** o novo command deixa vermelho o contrato B6.1, e a idempotência não está protegida contra execuções concorrentes (`app/chamados/tests/test_contract_crons.py:220`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:174`; `app/pautas/models.py:123`).

Analisei a cópia disponível e reproduzi detectores em memória. Identidade do HEAD/parent e exclusividade dos três arquivos no diff: **[nao verificado]**. Suite Django e mutações de código: **[nao verificado]**. [nao verificado]

1. **Quem escreve e quem lê.**

   | Estado/valor | Escritor | Leitores relevantes |
   |---|---|---|
   | Todos os 26 `CAMPOS` de `FechamentoMensal` | Criação com mês/ano; cálculo e persistência pelo serviço de fechamento; restauração pela mesma porta. `app/ponto/management/commands/aplicar_09_corte_b.py:67`; `app/ponto/services/fechamento.py:889`; `app/ponto/services/fechamento.py:893`; `app/ponto/services/fechamento.py:1128`; `app/ponto/services/fechamento.py:1144`; `app/ponto/services/fechamento.py:1376` | PAGO, retorno de leitura, comparação nova e exportação TXT. `app/ponto/portas/regen_exportada.py:79`; `app/ponto/services/fechamento.py:1118`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:133`; `app/folha/export.py:722` |
   | `Pauta.texto`, `ancora_tipo`, `ancora_id`, `pai` | `escrever`, no `create` canônico. `app/pautas/services.py:180` | Deduplicação nova, API e seleção de cabeças. `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:174`; `app/pautas/views.py:40`; `app/pautas/services.py:435` |
   | `Pauta.cancelado_em` | `cancelar`. `app/pautas/services.py:294` | `nao_canceladas`, reversão. `app/pautas/models.py:60`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:214` |
   | `Pauta.feito_em` | `marcar_feita` e `marcar_feita_pelo_sistema`, ambos na porta. `app/pautas/services.py:247`; `app/pautas/services.py:272` | QuerySet, API, deduplicação e reversão. `app/pautas/models.py:57`; `app/pautas/views.py:43`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:178`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:216` |
   | `ExportacaoDominio` | `registrar` cria conteúdo/hash/metadados; `invalidar` escreve invalidação/substituição. `app/folha/models.py:182`; `app/folha/models.py:158` | K6 consulta empresa/competência e vigência; o command lê exclusivamente sua resposta. `app/folha/export.py:581`; `app/folha/models.py:211`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:118` |

   **Não encontrei escritor novo de Pauta fora da porta:** cabeças/filhas passam por `escrever`, reversão por `cancelar` (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:184`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:188`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:219`).

   A chamada de DEVIDO usa `somente_leitura=True`, com **`lavrar_dias_pagos=False` no default**. Ela monta instância destacada, evita `get_or_create`/`save`/`update` e não entra na lavra de `DiaPago` nem na trilha da guarda; células são consultadas (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:129`; `app/ponto/services/fechamento.py:439`; `app/ponto/services/fechamento.py:881`; `app/ponto/services/fechamento.py:1112`; `app/ponto/services/fechamento.py:1175`; `app/ponto/services/fechamento.py:530`; `app/ponto/services/fechamento.py:595`).

   **Entretanto, escrever pauta também escreve `ContadorPauta` e `LogAuditoria`, e pode criar `auth.User`.** `autor_sistema` chama `get_user_sistema`, cujo código faz `get_or_create`; isso ocorre antes do `atomic` do command e não é desfeito pela reversão (`app/pautas/services.py:186`; `app/pautas/services.py:352`; `app/core/observ.py:134`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:182`; `app/core/utils.py:18`). Prova dinâmica de ausência de escrita em todas as tabelas: **[nao verificado]**.

2. **DEVIDO versus REGEN e `fechamento_lido`.**

   Os argumentos que definem a conta coincidem: mês/ano e lista unitária; `empresa_id=None` nos dois caminhos. `motivo_exportada` muda o registro do ato; `coletar_furos` coleta saída; `retornar_objeto` escolhe a representação. `fechamento_lido` chama a mesma função, acrescentando apenas `retornar_objeto=True` (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:129`; `app/ponto/portas/regen_exportada.py:179`; `app/ponto/services/fechamento.py:439`; `app/ponto/services/fechamento.py:538`; `app/ponto/services/fechamento.py:1111`; `app/ponto/services/fechamento.py:1119`; `app/ponto/services/fechamento.py:1743`).

   **Ressalva:** mesma conta não garante o mesmo resultado da retificação completa: REGEN primeiro regenera células, alterando os insumos. Além disso, a guarda da 09 pode bloquear a escrita enquanto permite leitura (`app/ponto/portas/regen_exportada.py:174`; `app/ponto/portas/regen_exportada.py:179`; `app/ponto/services/fechamento.py:530`; `app/ponto/services/fechamento.py:551`).

3. **Chaves e unidades.**

   Ambos projetam exatamente o mesmo `CAMPOS`; não há chave unilateral no código atual. Mês/ano são excluídos da comparação. Horas continuam horas, minutos continuam minutos e contadores continuam contadores, sem conversão pelo command (`app/ponto/portas/regen_exportada.py:84`; `app/ponto/services/fechamento.py:1120`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:42`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:133`; `app/ponto/models.py:412`; `app/ponto/models.py:432`; `app/ponto/models.py:465`).

   **Ausência versus zero pode desaparecer:** `_diferente(None, 0)` considera igualdade por usar `a or 0`. É comportamento da autoridade reutilizada, não divergência de chaves introduzida aqui (`app/ponto/portas/regen_exportada.py:248`).

4. **Derivação paralela e formatação.**

   Não identifiquei regra financeira própria: cálculo e tolerância vêm das autoridades; a soma local conta colaboradores nos baldes (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:104`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:129`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:133`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:139`).

   Para números finitos, `_fmt` não transforma diferença acusada em dois números iguais: arredonda a quatro casas, enquanto `_diferente` exige delta superior a `0.015` — inferência matemática dessas duas implementações (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:45`; `app/ponto/portas/regen_exportada.py:250`).

5. **Universo e ordem das checagens.**

   Em execução concluída, as quatro partes somam os **IDs distintos** pedidos. Inexistentes recusam; ausência de DEVIDO recusa o ato antes de `_escrever`; os demais entram em exatamente um balde (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:91`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:107`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:119`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:125`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:135`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:136`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:139`).

   Entregue antes de gravado é coerente com o escopo: K6 exige TXT vigente não invalidado; competência não entregue fica fora desta pauta, com explicação. Não significa proibir seu recálculo nem fazer Pauta pré-condição, revogada pela L-113 (`app/folha/export.py:583`; `app/core/juizes.py:312`; `CLAUDE.md:398`; `CLAUDE.md:403`; `app/docs/LEIS.md:150`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:121`).

   **A promessa sobre desligado é condicional:** se não entregue ou sem gravado, ele entra nesses baldes; somente quem chega ao cálculo sem retorno provoca recusa. Não é “todo desligado sempre recusa” (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:123`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:128`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:130`).

6. **Idempotência.**

   `col1` não casa com `col12`: a chave contém espaço depois do PK e competência completa; a busca acrescenta outro espaço delimitador. Feitas continuam participando porque a consulta usa `nao_canceladas` (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:53`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:174`; `app/pautas/models.py:60`).

   **Não há unicidade garantida:** consulta e criação estão separadas, a consulta ocorre antes do `atomic`, e o modelo não tem constraint dessa chave. Duas execuções podem observar ausência e criar duas cabeças (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:174`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:183`; `app/pautas/models.py:123`).

   Pelo contrato declarado de chave do fato, uma segunda rodada com DEVIDO diferente **não cria outra pauta**: também não atualiza a antiga. Assim, a pauta persistida pode conservar o DEVIDO anterior; o teste de duas rodadas não muda o motor entre elas (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:29`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:176`; `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:188`).

7. **Reversão e falhas.**

   O arquivo com plano nasce antes da primeira pauta e antes de `autor_sistema`; cabeça e filhas ficam numa transação por colaborador. Rodadas parcialmente concluídas preservam os grupos anteriores e seus IDs no arquivo (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:154`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:158`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:182`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:183`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:191`).

   **Banco e arquivo não são atomicamente coerentes:** o JSON recebe IDs antes do commit; falha no commit pode deixá-lo apontando para registros inexistentes. O reverter ignora IDs ausentes. Há também corrida entre `exists` e a gravação: duas execuções com o mesmo caminho podem ultrapassar a recusa e sobrescrever o arquivo (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:192`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:213`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:87`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:231`).

   `cancelar(p, p.autor_user)` é permitido pela porta: ela exige igualdade com o autor gravado. O command pula feitas, cuja tentativa de cancelamento seria recusada (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:216`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:219`; `app/pautas/services.py:288`).

8. **Tamanho e identificação.**

   O teto é 500. As filhas passam pelo packer, que inclui prefixo e continuação no cálculo e recusa item impossível. A cabeça não passa pelo packer, mas seu molde fixo cabe; medi em memória 334 caracteres com PK de 19 dígitos e ano 9999 (`app/pautas/models.py:89`; `app/pautas/services.py:123`; `app/pautas/services.py:133`; `app/pautas/services.py:140`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:57`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:169`).

   Texto e stdout explícitos usam `colNNN`, sem nome/CPF. **O caminho indireto de falha pode registrar nome no logger do fechamento**; destino efetivo desse logger em stdout/stderr: **[nao verificado]** (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:54`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:167`; `app/ponto/services/fechamento.py:1287`).

   “PAGO” significa gravado de `FechamentoMensal`, não pagamento comprovado nem conteúdo recuperado do TXT; o próprio cabeçalho manda conferir no Domínio (`app/ponto/portas/regen_exportada.py:81`; `CLAUDE.md:410`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:60`).

9. **Os testes mordem, mas não fecham todas as propriedades.** [nao verificado]

   Os mutantes abaixo são candidatos por inspeção; sobrevivência efetivamente executada: **[nao verificado]**.

   | Propriedade | Mutante que a cobertura deixa aberto |
   |---|---|
   | Entregue | Fixar K6 em agosto/2026: todas as chamadas do helper usam essa competência. `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:73` |
   | Gravado intacto | Alterar apenas `previsto_em`, `derivado_em` ou carimbos de aprovação: o hash os exclui e o teste não compara esses campos. `app/ponto/portas/regen_exportada.py:37`; `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:175` |
   | Dry não escreve | Antecipar `autor_sistema` para antes da saída dry, permitindo criar User: o teste conta somente Pauta. `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:156` |
   | Duas rodadas | Deduplicar também pelo valor de DEVIDO: as duas rodadas do teste têm o mesmo DEVIDO. `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:188` |
   | Feita | Remover o salto da feita na reversão: o teste de feita apenas repete a emissão; não reverte. A porta real recusaria a tentativa. `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:205`; `app/pautas/services.py:290` |
   | Reversão antes/coerência | Remover o `atomic`: a falha simulada ocorre já na primeira escrita, sem cabeça prévia para ficar órfã; não cobre falha na filha/commit. `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:216` |
   | Recusa do desligado | Substituir a ausência no retorno por regra própria `situacao == 'desligado'`: o único fixture sem retorno usado nessa prova é desligado. `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:54`; `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:136` |

   Há mordida concreta para os dois números, exclusão da empresa aberta, duplicação sequencial e cancelamento sem apagar; não classifico a bateria inteira como decorativa (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:99`; `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:125`; `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:188`).

10. **Contratos e selos.**

   - **B6.1 vermelho:** reproduzindo a expressão do contrato, sobra `pauta_pago_x_devido_exportada`. Todo command precisa estar nas entradas ou em `FORA_DE_PIPELINE`; o molde está cadastrado, o novo não (`app/chamados/tests/test_contract_crons.py:214`; `app/chamados/tests/test_contract_crons.py:225`; `app/config/crons.py:777`).
   - **S4 não acusa esta chamada:** libera a presença de `somente_leitura`; o novo command passa `True` (`app/ponto/tests/test_s4_ninguem_recalcula_para_ler.py:88`; `app/ponto/tests/test_s4_ninguem_recalcula_para_ler.py:116`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:129`).
   - **Pauta/censo não ganham escrita fora da porta:** o command delega as mutações; os contratos cobram esse limite (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:184`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:219`; `app/pautas/tests/test_contract_um_escritor.py:37`; `app/core/tests/test_censo_escritas.py:297`).
   - **Relógio:** o teste congela o tempo; o command lê `now` uma vez para o metadado da reversão, antes do laço. Os detectores da grade não apontaram ocorrência nova (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:34`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:154`; `app/escala/tests/test_selo_grade_pede_o_instante.py:93`).

11. **Classes de reprovação anteriores.**

   Não identifiquei segundo juiz, segundo escritor de Pauta ou derivação financeira paralela: o command chama K6, fechamento, comparação e portas existentes. O registro vivo é `core/juizes.py`, conforme o próprio documento de consulta (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:103`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:129`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:133`; `app/docs/JUIZES.md:7`; `app/core/juizes.py:312`).

   **Aparecem lacunas de prova e de proteção concorrente:** falha intermediária/commit não exercitada, DEVIDO alterado entre rodadas não exercitado e deduplicação sem trava/constraint. Há ainda o pré-requisito estrutural B6.1 pendente (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:188`; `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:216`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:174`; `app/pautas/models.py:123`; `app/chamados/tests/test_contract_crons.py:220`). Ordem operacional das travas e pouso efetivo antes de pré-requisito: **[nao verificado]**.

## 06/11 14:15 · sessao f7903ca7ae65 · - · - · origem=Ronald (tmux)

Achado bloqueante: o command novo não tem casa no pipeline. Reproduzi em memória a expressão do contrato e obtive `['pauta_pago_x_devido_exportada']`; `test_todo_command_tem_casa` exige que todo command esteja nas entradas ou em `FORA_DE_PIPELINE` (`app/chamados/tests/test_contract_crons.py:214-228`). O molde está declarado; o novo falta (`app/config/crons.py:777-780`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:64`).

Demais verificações estruturais reproduzidas em memória, sem arquivos:

- S4: nenhum ofensor; chamada nova é `(129, True)`. O detector libera pela presença de `somente_leitura`, inclusive sem conferir se seu valor é `True` (`app/ponto/tests/test_s4_ninguem_recalcula_para_ler.py:88`; `app/ponto/tests/test_s4_ninguem_recalcula_para_ler.py:113-118`).
- Censo: nenhuma escrita direta no command; família pauta com sete sítios, sete na porta, zero fora. A asserção exige zero fora (`app/core/censo_escritas.py:451-466`; `app/core/tests/test_censo_escritas.py:297-298`). O command escreve e cancela pela porta (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:184-190`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:219`).
- Relógio do teste: detector retornou zero; arquivo congela o tempo (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:34`; `app/core/tests/test_selo_teste_sem_relogio.py:32-45`).
- Relógio produção: os três detectores da grade retornaram `[]`. O command lê `timezone.now()` uma vez, para `gerado_em`, antes do laço (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:154-161`). O selo da grade só examina alvos enumerados e funções que os chamam (`app/escala/tests/test_selo_grade_pede_o_instante.py:24-38`; `app/escala/tests/test_selo_grade_pede_o_instante.py:109-119`).

Q9 — os testes têm mordida, porém deixam estes mutantes candidatos sobreviverem por inspeção; **não executei mutações** `[nao verificado]`: [nao verificado]

| Propriedade | Mutante que a cobertura deixa aberto |
|---|---|
| Entregue | Fixar a consulta em mês 8/2026: todo `_roda` usa exatamente essa competência (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:73-75`). Os testes mordem empresa aberta e TXT invalidado, mas não outro mês (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:125-130`; `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:239-247`). |
| Gravado intacto | Alterar apenas `derivado_em`, `previsto_em`, `aprovado_em` ou `aprovado_por`: estão excluídos do hash usado na prova (`app/ponto/portas/regen_exportada.py:37-38`; `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:175-186`). |
| Dry não escreve | Inserir escrita em User, DiaPago, CelulaDia ou trilha durante dry: o teste só compara quantidade de Pauta (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:156-162`). |
| 2x | Deduplicar pelo conteúdo de PAGO/DEVIDO em vez de chave do fato: a segunda rodada conserva o mesmo motor/DEVIDO (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:188-196`). |
| Feita | Na reversão, cancelar indevidamente uma feita: o teste de feita verifica somente não gerar outra cabeça; não chama reversão (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:205-214`). A porta verdadeira recusaria a tentativa (`app/pautas/services.py:288-291`). |
| Reversão antes | Remover `transaction.atomic` ou criar inicialmente arquivo sem plano: a falha simulada ocorre na primeira escrita e só exige `pautas == []`, sem exigir `plano` nem falhar na filha/commit (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:216-226`). |
| Recusa desligado | Duplicar a regra com `c.situacao == 'desligado'` em vez de tratar ausência no retorno: o único fixture ausente é desligado (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:54`; `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:136-143`). |

Sobre Q11: há cobertura incompleta para falha intermediária e mudança de DEVIDO entre rodadas, além do vermelho B6.1 comprovado; não sustento “RED sem mordida” em bloco, pois existem asserções de efeito para texto com os dois números, ausência de pauta em aberta, idempotência e cancelamento sem apagar (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:99-118`; `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:125-130`; `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:188-203`).

## 06/11 14:15 · sessao 31ef3b58b827 · - · - · origem=Ronald (tmux)

- **Escritores atribuídos pelo censo:** `FechamentoMensal` permanece em `app/ponto/services/fechamento.py`; `Pauta` em `app/pautas/services.py`; `ExportacaoDominio` em `app/folha/models.py`, conforme portas declaradas em `app/core/portas.py:500`, `app/core/portas.py:505`, `app/core/portas.py:510`. Executei `varrer_arvore` em memória, sem bytecode; ele atribuiu os seguintes sítios:
  - FM: `app/ponto/services/fechamento.py:882`, `:889`, `:1144`, `:1324`, `:1338`, `:1376`, `:1433`, `:1594`, `:1636`, `:1716`.
  - Pauta: `app/pautas/services.py:180`, `:215`, `:247`, `:272`, `:294`.
  - ED: `app/folha/models.py:162`.
  - **Limitação:** o próprio censo não atribuiu `cls.objects.create` de `ExportacaoDominio.registrar`, que efetivamente escreve em `app/folha/models.py:182`; portanto não vender seu resultado como prova universal de ausência de escritor oculto. O alcance declarado do mecanismo está em `app/core/censo_escritas.py:6`.

- **FM CAMPOS:** origem da lista de 26 chaves em `app/ponto/management/commands/aplicar_09_corte_b.py:67`; escritor dos números é o laço de `recalcular_fechamento_mes`, com atribuições em `app/ponto/services/fechamento.py:893`, `:909`, `:949`, `:985`, `:1046`, `:1067`, `:1084`; persistência por `save` em `:1128` ou `update` em `:1144`. Restauração de snapshot também pode escrever CAMPOS, pela mesma porta, em `app/ponto/services/fechamento.py:1342` e `:1376`. `mes/ano` entram na criação em `:889`; a restauração os exclui em `:1364`. Leitores relevantes: PAGO recorta todos os CAMPOS em `app/ponto/portas/regen_exportada.py:79`; somente leitura semeia FM em `app/ponto/services/fechamento.py:881` e entrega o mesmo recorte em `:1118`; TXT lê FM e entrega a `eventos_do_fechamento` em `app/folha/export.py:711` e `:722`; classificação lê em `:777`; outros leitores identificados são relatórios em `app/relatorios/views.py:68` e `:139`, cartão em `app/relatorios/cartao_pela_celula.py:250`, inteligência em `app/inteligencia/resumo.py:35`, detalhe em `app/colaboradores/services/detalhe.py:115`. É um mapa de leitores relevantes, não censo exaustivo.

- **Pauta:** criação de `texto`, `ancora_tipo`, `ancora_id`, `pai` exclusivamente pela chamada nova a `_ps.escrever` em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:184` e `:188`; o `create` efetivo fica em `app/pautas/services.py:180`. `cancelado_em` é escrito por `cancelar` em `app/pautas/services.py:294`; `feito_em` por `marcar_feita` em `:247` e `marcar_feita_pelo_sistema` em `:272`. Novo command lê texto/âncora/pai/cancelamento para deduplicar em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:174`; lê `feito_em` em `:178` e `:216`, cancelamento em `:214`. QuerySets leem os estados em `app/pautas/models.py:57` e `:60`; API lê texto, âncoras e estado em `app/pautas/views.py:40`, `:43`, `:56`. `pai` também seleciona cabeças em `app/pautas/services.py:435`.

- **Outras tabelas realmente escritas no modo escrever:** Pauta escreve trilha e contador por `app/pautas/services.py:186` e `:188`; `_trilha` chama `evento` em `:369`; este cria `LogAuditoria` em `app/core/observ.py:134`; `lavrar` cria/atualiza `ContadorPauta` em `app/pautas/services.py:352` e `:354`. **Há ainda criação possível de `auth.User`:** command chama `autor_sistema` em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:182`, antes do `atomic` de `:183`; `autor_sistema` chama `get_user_sistema` em `app/pautas/services.py:87`, que executa `User.objects.get_or_create(username='sistema')` em `app/core/utils.py:18`. A reversão guarda só IDs de pautas em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:154` e `:191`; não desfaz eventual User. Dry interrompe antes disso em `:180`.

- **ED:** `registrar` escreve empresa, competência, usuário, nome, hash, linhas, reexportação e conteúdo em `app/folha/models.py:182`; `invalidar` altera invalidação/substituição e escreve auditoria em `:158` e `:163`, preservando conteúdo/hash conforme contrato em `:149`. Novo command só lê K6 em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:118`; leitura efetiva de ED em `app/folha/export.py:581`, ordenação/vigência em `app/folha/models.py:211`. Outros leitores: histórico/download em `app/folha/views.py:445`, `:522`, `:554`, `:827`; Haiku em `app/folha/haiku_export.py:67`, `:99`, `:157`; vigia em `app/folha/services/vigia_txt.py:201`.

- **Universo/K6:** IDs são conjunto ordenado em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:91`. Inexistente recusa em `:107`; não entregue entra em `nao_entregue` e termina em `:119`; entregue sem gravado entra em `sem_gravado` em `:125`; restante entra em exatamente um dos dois baldes em `:135`; falta de DEVIDO recusa antes de `_escrever` em `:136`, com sequência medir/escrever em `:96`; soma conferida em `:139`. Desligado não entregue ou sem gravado não alcança a checagem de DEVIDO, pois os `continue` anteriores o classificam (`:123`, `:128`); isso é diferente de “todo desligado sempre recusa”.

- **Ordem entregue antes de gravado é coerente com escopo desta pauta:** K6 significa TXT vigente não invalidado, sem provar pagamento, em `app/folha/export.py:560`, `:583`; registro vivo do juiz em `app/core/juizes.py:312`. A regra de competência exportada determina os dois números em `CLAUDE.md:403` e `:406`; não exportada tem caminho de apply em `CLAUDE.md:398`. L-113 remove Pauta como pré-condição em `app/docs/LEIS.md:150`; command não estabelece tal pré-condição porque apenas classifica e sai em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:119`. Texto não deve equiparar entregue a pago efetivo: `CLAUDE.md:410` declara que TXT é fotografia, não pagamento; o cabeçalho pede conferência no Domínio em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:60`.

## 06/11 14:15 · sessao 80d80ef3e4c8 · R1-PUBLICA · verificador · origem=Code

**Os achados 1 e 3 continuam parcialmente abertos. O selo corrigido morde os três casos pedidos, mas ainda não vincula o arquivo citado à data efetivamente escolhida pelo publicador.** Procedência: `bin/gerar_estado.py:164`, `app/core/placar_estrutural.py:108`, `app/core/tests/test_r1_termometro_uma_medicao.py:67`, `bin/gerar_estado.py:51`.

1. **Achado 1 — parcialmente resolvido: os números chegaram ao início, mas continuam truncados.**

   Reproduzi em memória o corte de 150 caracteres aplicado pelo publicador. Nenhuma das seis provas publica o caminho completo da medição; várias perdem números ou ressalvas. Procedência: `bin/gerar_estado.py:164`.

   | Prova | Final efetivamente publicado | Origem | [nao verificado]
   |---|---|---|
   | E1-sem-previsao | ``(aberta) (medida `logs/`` | `app/core/espelho_verdade.py:49` |
   | E3-turno-aberto | `dos impares que DIVERGEM, 31 e` | `app/core/espelho_verdade.py:82` |
   | E6-sistema-bate | `11/2026 aberta **94,82%** (3680 ` | `app/core/espelho_verdade.py:169` |
   | E6-exportavel | `lista do admin pela L-099; entrada ef` | `app/core/espelho_verdade.py:173` |
   | E6-batida-impar | `198 tem contagem impar no dia e ` | `app/core/espelho_verdade.py:177` |
   | E6-piso-185 | `120 = 201 de 20` | `app/core/espelho_verdade.py:179` |

   Portanto, desapareceu o prefixo repetitivo que escondia todos os resultados, mas a publicação ainda mutila a informação — inclusive a ressalva do achado 6 e o denominador `201`. Procedência: recortes acima e `bin/gerar_estado.py:164`.

2. **Achado 2 — resolvido nos três cenários pedidos; permanece um limite na origem da data.**

   Executei os cinco métodos em memória, substituindo somente `SimpleTestCase` por `unittest.TestCase`. Procedência: `app/core/tests/test_r1_termometro_uma_medicao.py:49`.

   | Cenário | Executados / falhas / erros | Regra responsável |
   |---|---:|---|
   | Texto atual | 5 / 0 / 0 | `app/core/tests/test_r1_termometro_uma_medicao.py:49` |
   | Outro leitor cita arquivo datado diferente, na mesma glob | 5 / 1 / 0 | `app/core/tests/test_r1_termometro_uma_medicao.py:67` |
   | Um leitor cita somente a glob literal | 5 / 1 / 0 | `app/core/tests/test_r1_termometro_uma_medicao.py:33`, `:64` |
   | Menção no meio da prova, com caminho antigo | 5 / 1 / 0 | `app/core/tests/test_r1_termometro_uma_medicao.py:46`, `:64` |
   | Menção no meio, com caminho correto | 5 / 0 / 0 | `app/core/tests/test_r1_termometro_uma_medicao.py:46` |

   **Não copia os valores atuais nem consulta relógio:** compara caminhos derivados das declarações; os exemplos literais usam caminhos fictícios. Procedência: `app/core/tests/test_r1_termometro_uma_medicao.py:32`, `:50`, `:67`, `:75`.

   **Limite persistente:** todos podem citar o mesmo arquivo de 06/11 e o publicador escolher 07/11 se existir outro arquivo correspondente à glob. `_medido_em` escolhe a maior data sem consultar as provas; a simulação desses dois caminhos devolveu 07/11. Existência real desse segundo arquivo: **[nao verificado]**. Procedência: `app/core/placar_estrutural.py:111`, `bin/gerar_estado.py:47`, `:51`, `:56`.

   Retirar completamente a menção ainda remove um item do censo; esse mutante passou, conforme o limite agora declarado. Procedência: `app/core/tests/test_r1_termometro_uma_medicao.py:15`, `:46`.

3. **Achado 3 — parcialmente resolvido: restou a atribuição errada em `SOMA.fonte`.**

   `numero` e `prova` agora atribuem corretamente os 36 `SOMA True` à sonda, compatíveis com as seis linhas de cada uma das seis partes do anexo. Entretanto, `fonte` ainda afirma: **“a soma e cobrada no proprio comando, nao por sonda paralela”**. Procedência: anexo `medida.txt`; `app/core/placar_estrutural.py:106`, `:107`, `:108`, `:109`.

   O comando apenas calcula e imprime os totais dos donos e das classes; não compara os dois para produzir booleano ou reprovar a execução. Também não confere igualdade de horas ou união. Procedência: `app/ponto/management/commands/e6_oraculo.py:415`, `:423`, `:424`, `:426`, `:429`. Implementação da sonda: **[nao verificado]**.

4. **Achado 4 — resolvido no texto integral.**

   A conclusão exclusiva “cobrança, não cura de motor” foi retirada da prova. Procedência: `app/core/espelho_verdade.py:82`.

5. **Achado 5 — resolvido quanto aos subconjuntos e ao alcance da comparação.**

   **31/25 estão corretamente atribuídos a `impar_FORA_da_faixa`, dentro dos ímpares divergentes.** Também ficou explícito que 198+1 e 137+2 repartem os divergentes, enquanto 347/262 incluem ímpares que batem e divergem. Procedência: anexo `medida.txt`; `app/core/espelho_verdade.py:82`, `:177`; `app/ponto/management/commands/e6_oraculo.py:370`, `:381`, `:386`.

   A inclusão passou a valer “entre os dias COMPARADOS”, com ressalva sobre os excluídos; isso acompanha a ordem do código, que exclui antes de classificar paridade. Procedência: `app/core/espelho_verdade.py:177`; `app/ponto/management/commands/e6_oraculo.py:324`, `:327`, `:331`, `:339`, `:342`, `:347`.

6. **Achado 6 — resolvido como declaração normativa no texto integral; ressalva perdida na publicação.**

   “Entrada efetiva na lista NAO medida aqui” distingue a política da execução comprovada. A destinação normativa tem respaldo na L-099; entradas efetivas permanecem **[nao verificado]**. Procedência: `app/core/espelho_verdade.py:173`, `app/docs/LEIS.md:122`. O publicador corta justamente essa ressalva, conforme o achado 1: `bin/gerar_estado.py:164`.

7. **Achado 7 — correções solicitadas realizadas, com pequeno resíduo na docstring.**

   A nota deixou de afirmar cadência cumprida em 09/10; a docstring passou a contar seis itens. Procedência: `app/core/placar_estrutural.py:119`, `app/core/tests/test_r1_termometro_uma_medicao.py:9`.

   Ainda existe desencontro descritivo: a docstring diz “prova comeca por”, enquanto a implementação aceita menção em qualquer posição. Procedência: `app/core/tests/test_r1_termometro_uma_medicao.py:6`, `:14`, `:46`.

   A publicação independente por `logs/e6_placar.json` continua separada; seus valores atuais são **[nao verificado]**, sem conclusão adicional nesta revisão. Procedência: `bin/gerar_estado.py:145`, `:168`; `app/core/espelho_verdade.py:208`.

8. **Texto e números: sem inversão encontrada, mas há duas qualificações pendentes.**

   As somas dos campos fornecidos conferem com os números publicados: BATE/comparados, percentuais, dias e horas por dono, zeros e contadores de ímpares. As horas representam diferenças absolutas, como agora declarado. Procedência: anexo `medida.txt`; `app/core/placar_estrutural.py:106`, `:118`; `app/core/espelho_verdade.py:49`, `:82`, `:177`; `app/ponto/management/commands/e6_oraculo.py:368`, `:423`.

   - **Afirmação nova sem demonstração:** “6 partes disjuntas por pk”. O anexo apresenta contagens, sem composição ou regra de partição; igualdade das somas dos universos não demonstra disjunção. Assim, 144/105 e os colaboradores por dono conferem como somas dos parciais; sua interpretação como união global sem duplicação é **[nao verificado]**. Procedência da afirmação: `app/core/placar_estrutural.py:118`; conjuntos dentro de uma execução: `app/ponto/management/commands/e6_oraculo.py:369`, `:429`.
   - **Alcance contextual remanescente:** 95,95%/94,82% incluem ímpares que batem, mas aparecem sob “com batidas COMPLETAS”. Não são percentuais exclusivos dos completos. Procedência: `app/core/espelho_verdade.py:168`, `:169`; `app/ponto/management/commands/e6_oraculo.py:347`, `:352`, `:353`, `:355`.

9. **Leitores — nenhuma quebra de execução encontrada nos caminhos examinados; permanece a quebra de conteúdo pelo recorte.**

   Os dois publicadores renderizaram em memória sem cair em “indisponível”, com data simulada e ausência simulada do log E6. Os 11 métodos L114 passaram com a mesma substituição por `unittest.TestCase`. Procedência: `bin/gerar_estado.py:60`, `:127`; `app/core/tests/test_l114_placar_papel_e_termometro.py:30`, `:48`, `:69`, `:84`, `:112`.

   Handoff e selo R6 leem a declaração de contratos/R6, fora dos textos alterados. Procedência: `bin/handoff_sessao.sh:179`, `app/core/tests/test_selo_r6_frase_do_juiz.py:17`.

   Execução Django canônica, selo host com ancestralidade Git, publicação sem nomes e identidade Git do commit: **[nao verificado]**. Dependências dos selos: `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:24`, `:75`; `bin/tests/test_publicacao_sem_nome.sh:39`, `:60`.

## 06/11 14:14 · sessao b18c5d4c573a · - · - · origem=Ronald (tmux)

Q1–4, leitura estática:

- **Sem novo escritor de Pauta fora da porta.** O command escreve cabeçalho/filhas exclusivamente por `_ps.escrever` e reverte por `cancelar` (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:184`, `:188`, `:219`). A porta grava texto, âncora e pai em `Pauta.objects.create`, além de trilha e contador (`app/pautas/services.py:180`, `:186`, `:188`). `feito_em` pertence a `marcar_feita`/`marcar_feita_pelo_sistema`, e `cancelado_em` a `cancelar` (`app/pautas/services.py:247`, `:272`, `:294`).

- **Há escrita indireta adicional de `auth.User` possível em `--escrever`.** O command chama `autor_sistema` antes do seu `transaction.atomic` (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:182`). Essa função chama `get_user_sistema` (`app/pautas/services.py:86`), que executa `User.objects.get_or_create(username='sistema')` (`app/core/utils.py:18`). Essa criação pode sobreviver a falha posterior da pauta; dry pula antes dessa chamada (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:180`).

- **O modo invocado não escreve FechamentoMensal, DiaPago nem trilha de exportada.** `lavrar_dias_pagos=False` é o default (`app/ponto/services/fechamento.py:439`). Com `somente_leitura=True`, a instância é destacada, sem PK, enquanto o `get_or_create` fica no `else` (`app/ponto/services/fechamento.py:881`, `:889`). Retorna o dict antes dos ramos `save`/`update` (`app/ponto/services/fechamento.py:1112`, `:1128`, `:1144`). A lavra de DiaPago exige `(not somente_leitura) or lavrar_dias_pagos` (`app/ponto/services/fechamento.py:1175`). O warning/trilha da guarda também depende desse predicado, logo a leitura pura não entra (`app/ponto/services/fechamento.py:530`, `:552`).

- **A leitura de células examinada é consulta.** O fechamento consulta o lote de CelulaDia (`app/ponto/services/fechamento.py:595`), e a grade chama `grade_da_celula` (`app/folha/export.py:335`), cuja carga também é `filter` (`app/escala/services/leitor_celula.py:350`). Não encontrei escrita nessas rotas; prova dinâmica de todas as tabelas: `[nao verificado]`.

- **Escritor e leitores dos CAMPOS.** A lista canônica está em `app/ponto/management/commands/aplicar_09_corte_b.py:67`. O escritor monta horas em `app/ponto/services/fechamento.py:893`, `:909`, `:949`, `:985`; minutos/dias em `:1046`, `:1067`; demais contadores/reflexo/banco em `:1084`. Persiste por `save` ou `update` (`:1128`, `:1144`). PAGO lê essa lista por `getattr` do gravado (`app/ponto/portas/regen_exportada.py:79`); DEVIDO lê a mesma lista da instância destacada (`app/ponto/services/fechamento.py:1118`); o command compara em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:133`.

- **ExportacaoDominio não é escrito pelo command.** A autoridade K6 consulta registros e verifica vigente não invalidado (`app/folha/export.py:580`). Seus escritores existentes são `registrar` para criação e `invalidar` para invalidada_em/motivo/substituída (`app/folha/models.py:174`, `:182`, `:143`, `:158`, `:162`).

- **DEVIDO usa os mesmos argumentos numéricos da chamada de recalcular dentro da REGEN.** Ambos passam mês/ano e lista unitária; empresa_id permanece None (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:129`; `app/ponto/portas/regen_exportada.py:179`; default em `app/ponto/services/fechamento.py:439`). A janela é resolvida pela empresa do colaborador dentro do laço (`app/ponto/services/fechamento.py:612`). `motivo_exportada` afeta o warning (`:538`); `retornar_objeto` escolhe objeto/dict (`:1119`); `coletar_furos` só copia a saída (`:1111`). `fechamento_lido` usa a mesma chamada com `retornar_objeto=True` (`:1743`).

- **Ressalva sobre a frase “o que a REGEN faria”:** a REGEN regenera células antes de recalcular, então seu resultado posterior pode mudar por insumos distintos, mesmo usando o mesmo motor (`app/ponto/portas/regen_exportada.py:174`, `:179`). Na 09/2026, a guarda atual também retira colaboradores exportados do universo da escrita, enquanto leitura pura passa (`app/ponto/services/fechamento.py:530`, `:551`, `:555`).

- **Mesmas chaves e unidades.** PAGO e DEVIDO projetam exatamente CAMPOS; mês/ano são excluídos da comparação (`app/ponto/portas/regen_exportada.py:84`; `app/ponto/services/fechamento.py:1120`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:42`, `:133`). Campos de horas são Decimal com duas casas; minutos e contadores são inteiros; reflexo/banco são Float (`app/ponto/models.py:412`, `:429`, `:432`, `:465`, `:467`). **None↔0 é tratado como igual pela autoridade**, pois `_diferente` usa `float(a or 0)`; isso pode esconder ausência versus zero, mas não é divergência de chave nem regra nova do command (`app/ponto/portas/regen_exportada.py:248`).

- **Sem derivação financeira paralela identificada.** O command usa as autoridades para cálculo e comparação (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:104`, `:129`, `:133`). `_fmt` arredonda a quatro casas (`:45`); a comparação exige diferença maior que 0,015 (`app/ponto/portas/regen_exportada.py:250`). Para valores finitos numéricos, esse arredondamento não transforma diferença acusada em dois números iguais: erro combinado máximo de apresentação é 0,0001, inferior à tolerância (inferência dessas linhas).

- **Nome em caminho indireto de erro:** o cálculo pode registrar `colab.nome` no logger ao capturar exceção (`app/ponto/services/fechamento.py:1287`). O command só detecta a ausência de DEVIDO depois (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:130`). Portanto texto/stdout explícitos usam colNNN, mas log/stderr de falha pode carregar nome; configuração efetiva de destino desse logger `[nao verificado]`.

## 06/11 14:14 · sessao 8f50287db03d · - · - · origem=Ronald (tmux)

1. **Achado 3 só parcialmente resolvido.** `numero` e `prova` atribuem corretamente os 36 `SOMA True` à sonda, mas `fonte` conserva literalmente “a soma e cobrada no proprio comando, nao por sonda paralela”. Procedência: `app/core/placar_estrutural.py:106`, `:107`, `:108`, `:109`; anexo `medida.txt`, seis blocos × seis linhas `SOMA`. O comando calcula/imprime dois números, sem cotejá-los nem emitir booleano; tampouco confere horas ou união: `app/ponto/management/commands/e6_oraculo.py:415`, `:424`, `:426`, `:429`.

2. **Afirmação nova sem sustentação: “6 partes disjuntas por pk”.** O anexo mostra `PARTE`, universos parciais e totais, mas não mostra IDs, predicado de particionamento ou interseções; somar 624/578 não demonstra disjunção. Procedência da afirmação: `app/core/placar_estrutural.py:118`; os seis itens dizem “6 partes por pk”: `app/core/espelho_verdade.py:49`, `:82`, `:169`, `:173`, `:177`, `:179`. O comando desta cópia também não oferece opção de particionamento: `app/ponto/management/commands/e6_oraculo.py:264`. Disjunção e implementação da sonda: **[nao verificado]**. Consequentemente, 144/105 e os colabs por dono conferem como **somas dos parciais**, mas sua interpretação como união global sem duplicações permanece **[nao verificado]**; dentro de uma execução o comando usa conjuntos: `app/ponto/management/commands/e6_oraculo.py:369`, `:429`.

3. **Números/chaves conferidos, sem inversão.** Somei em memória os campos fornecidos nos seis blocos de `medida.txt`:

   - Outubro: 7483 comparados, 7180 BATE = 95,95%; donos dias 26/78/199; horas 88,36/306,53/961,16; colabs somados 16/32/113; total 303/1356,05/144. Publicação: `app/core/placar_estrutural.py:106`, `:118`.
   - Novembro: 3881 comparados, 3680 BATE = 94,82%; donos dias 9/72/120; horas 40,62/347,49/643,06; colabs somados 7/33/78; total 201/1031,17/105. Publicação: `app/core/placar_estrutural.py:106`, `:118`.
   - `esp_zero_e6_trabalho` = 35/28, inversa = 19/14; publicação usa a chave certa: `app/core/espelho_verdade.py:49`; classificação: `app/ponto/management/commands/e6_oraculo.py:353`, `:358`.
   - Ímpares = 347/262; BATE = 148/123; DIVERGE = 199/139; FORA = 31/25; contagem ímpar = 198/137; só corte = 1/2. Publicação: `app/core/espelho_verdade.py:82`, `:177`; alcance dos subcontadores: `app/ponto/management/commands/e6_oraculo.py:370`, `:381`, `:386`.

4. **Tratamentos 4, 5 e 6 resolvidos na prosa alterada.** A atribuição exclusiva a cobrança desapareceu; 31/25 agora explicitamente pertencem aos ímpares divergentes; 198+1/137+2 também têm esse alcance; a inclusão está limitada aos comparados; entrada efetiva na lista do admin foi expressamente retirada do que se afirma medido. Procedência: `app/core/espelho_verdade.py:82`, `:173`, `:177`; exclusões anteriores à paridade: `app/ponto/management/commands/e6_oraculo.py:324`, `:327`, `:331`, `:339`, `:342`. Destino normativo CADASTRO/BATIDA: `app/docs/LEIS.md:122`.

5. **Qualificação contextual remanescente:** 95,95%/94,82% são percentuais de **todos os comparados**, incluindo os ímpares que BATE, e não uma medição exclusiva de batidas completas. A prova conserva esses números sob o título “com batidas COMPLETAS”. Procedência: `app/core/espelho_verdade.py:168`, `:169`; denominador e inclusão dos ímpares: `app/ponto/management/commands/e6_oraculo.py:347`, `:352`, `:353`, `:355`. Não é inversão numérica nova; é associação de alcance potencialmente enganosa.

6. **Achado 7, prosa, corrigido:** a nota deixou de atestar cadência cumprida em 09/10: `app/core/placar_estrutural.py:119`; a docstring passou de cinco para seis itens: `app/core/tests/test_r1_termometro_uma_medicao.py:9`. Resta pequeno desencontro interno da docstring: ainda diz “prova comeca por” na linha 6, embora o censo efetivo seja menção em qualquer posição: `app/core/tests/test_r1_termometro_uma_medicao.py:6`, `:14`, `:44`.

## 06/11 14:13 · sessao 2ac7ccc3795a · - · - · origem=Ronald (tmux)

**Achado 1: resolvido parcialmente.** Os seis textos agora abrem com números, mas o publicador continua cortando em 150 caracteres; ainda elimina informação necessária. Procedência: execução em memória das provas de `app/core/espelho_verdade.py:49`, `:82`, `:169`, `:173`, `:177`, `:179`, usando o recorte de `bin/gerar_estado.py:164`.

| Item | Final efetivamente publicado | [nao verificado]
|---|---|
| E1-sem-previsao | ``(aberta) (medida `logs/`` |
| E3-turno-aberto | `dos impares que DIVERGEM, 31 e` |
| E6-sistema-bate | `11/2026 aberta **94,82%** (3680 ` |
| E6-exportavel | `lista do admin pela L-099; entrada ef` |
| E6-batida-impar | `198 tem contagem impar no dia e ` |
| E6-piso-185 | `120 = 201 de 20` |

Assim, o ESPELHO publicado ainda perde todos os caminhos completos e, especificamente, a ressalva de execução da lista do admin; o texto completo qualifica o achado 6, mas sua publicação interrompe essa qualificação. Referências: `app/core/espelho_verdade.py:173`, `bin/gerar_estado.py:164`.

**Leitores: nenhuma quebra de execução encontrada nos caminhos examinados.** Renderizei em memória `_placar_estrutural`, simulando a data recebida, e `_placar_espelho_verdade`, simulando exclusivamente ausência do log E6; ambos produziram suas tabelas sem cair no ramo de indisponibilidade. Procedência: funções de `bin/gerar_estado.py:60` e `:127`; tratamento de indisponibilidade em `:86` e `:146`.

- `placar_estrutural.placar()` copia registros e exige apenas prova não vazia; os novos textos satisfazem isso. Referências: `app/core/placar_estrutural.py:635`, `:637`.
- `espelho_verdade.placar()` exige prova não vazia para FEITO e mantém o texto integral até o publicador cortá-lo. Referências: `app/core/espelho_verdade.py:201`, `:203`; `bin/gerar_estado.py:164`.
- O termômetro publica `numero` inteiro; SOMA publica `numero` e `prova` inteiros. Referências: `bin/gerar_estado.py:99`, `:100`, `:116`.
- Executei os **11 métodos de L114**, substituindo somente `SimpleTestCase` por `unittest.TestCase`: **11 executados, 0 falhas, 0 erros**, incluindo o censo de datas. Procedência: `app/core/tests/test_l114_placar_papel_e_termometro.py:30`, `:48`, `:69`, `:84`, `:112`; substituição em memória, execução Django canônica **[nao verificado]**.
- O selo host de ancestralidade lê todos os campos textuais; as alterações não introduzem uma nova combinação de frase declarada de “fora do ar” e hash. Sua execução canônica exige repositório Git, indisponível nesta cópia: **[nao verificado]**. Referências: `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:24`, `:47`, `:67`, `:72`, `:75`.
- O selo de publicação sem nomes depende do banco e do repositório público; resultado real **[nao verificado]**. Referências: `bin/tests/test_publicacao_sem_nome.sh:39`, `:47`, `:60`, `:72`.
- Handoff extrai somente a frase de contratos; o selo de R6 lê apenas `R6.numero`. Referências: `bin/handoff_sessao.sh:179`; `app/core/tests/test_selo_r6_frase_do_juiz.py:17`, `:22`.

**Limite persistente da data:** mesmo com todos os leitores citando um único arquivo, `_medido_em` continua escolhendo a maior data de **todos** os caminhos da glob, sem conferir qual foi citado nas provas. Simulei arquivos de 06 e 07/11, e a função devolveu 07/11. Procedência: execução em memória de `bin/gerar_estado.py:47`, `:51`, `:56`; presença real de outro arquivo **[nao verificado]**.

## 06/11 14:11 · sessao 006a124a65eb · V3-IMPACTO-COM-DONO · sonda · origem=Code

Todas as linhas `SOMA` da parte fornecida apresentam `True`. Não há registro `INCOMPLETO` no texto fornecido. [nao verificado]

Transcrição integral da saída fornecida, sem verificação em arquivo ou execução: [nao verificado]

```text
sonda_leitura: script.py [c9475340dadfbee7] no cpuset 4-7, lendo sombra como leitor (LEITURA)

PARTE|1|6
FINALIDADE|IMPACTO_NUNCA_VALIDACAO
BASE_SOMAS|COLABORADORES_MEDIDOS
COBERTURA_SOMA|MEDIDOS_MAIS_EXCLUIDOS_IGUAL_UNIVERSO_PARTE
ATO|hoje=2026-11-06|agora=2026-11-06T14:02:42.101619-03:00
FORA_DA_MEDIDA|horas_falta
FORA_DA_MEDIDA|horas_reflexo_dsr
FORA_DA_MEDIDA|saldo_banco_horas
UNIVERSO|2026-10|colabs_porta=624
UNIVERSO_PARTE 2026-10 110
RECUSA|2026-10|col30|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=30
EXCLUIDO|2026-10|col30|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col42
MEDIDO|2026-10|col60
RECUSA|2026-10|col66|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=0
EXCLUIDO|2026-10|col66|PRODUTO:RECUSA_PORTA_SEM_DONO
HISTORICO_T|2026-10|col72|sem_resultado_do_fechamento|1
EXCLUIDO|2026-10|col72|PRODUTO:SEM_RETORNO_FECHAMENTO
MEDIDO|2026-10|col78
MEDIDO|2026-10|col84
MEDIDO|2026-10|col90
MEDIDO|2026-10|col108
MEDIDO|2026-10|col114
MEDIDO|2026-10|col120
MEDIDO|2026-10|col126
MEDIDO|2026-10|col138
RECUSA|2026-10|col144|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=15
EXCLUIDO|2026-10|col144|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col150
MEDIDO|2026-10|col168
MEDIDO|2026-10|col174
RECUSA|2026-10|col186|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=21
EXCLUIDO|2026-10|col186|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col192
MEDIDO|2026-10|col198
MEDIDO|2026-10|col204
DIA|2026-10|horas_trabalhadas|col204|2026-09-29|CADASTRO|7.3164730555555568|4.7735825000000007|-2.5428905555555561
RECUSA|2026-10|col210|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=30
EXCLUIDO|2026-10|col210|PRODUTO:RECUSA_PORTA_SEM_DONO
RECUSA|2026-10|col216|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=15
EXCLUIDO|2026-10|col216|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col222
MEDIDO|2026-10|col228
RECUSA|2026-10|col234|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=15
EXCLUIDO|2026-10|col234|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col246
MEDIDO|2026-10|col252
MEDIDO|2026-10|col258
DIA|2026-10|horas_atraso|col258|2026-10-19|ESTRUTURA|0|2.3685|2.3685
MEDIDO|2026-10|col270
DIA|2026-10|horas_extras_100_noturna|col270|2026-10-12|ESTRUTURA|0|0.95973594416666663|0.95973594416666663
MEDIDO|2026-10|col276
MEDIDO|2026-10|col282
MEDIDO|2026-10|col288
RECUSA|2026-10|col294|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=0
EXCLUIDO|2026-10|col294|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col300
MEDIDO|2026-10|col306
RECUSA|2026-10|col312|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=15
EXCLUIDO|2026-10|col312|PRODUTO:RECUSA_PORTA_SEM_DONO
RECUSA|2026-10|col318|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=30
EXCLUIDO|2026-10|col318|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col342
MEDIDO|2026-10|col348
MEDIDO|2026-10|col354
MEDIDO|2026-10|col360
MEDIDO|2026-10|col366
MEDIDO|2026-10|col384
MEDIDO|2026-10|col390
DIA|2026-10|horas_extras_100_noturna|col390|2026-10-12|ESTRUTURA|0|0.55330963833333335|0.55330963833333335
MEDIDO|2026-10|col396
MEDIDO|2026-10|col408
MEDIDO|2026-10|col414
MEDIDO|2026-10|col432
MEDIDO|2026-10|col438
MEDIDO|2026-10|col444
MEDIDO|2026-10|col450
DIA|2026-10|horas_intra_indenizada|col450|2026-09-27|BATIDA|1|0|-1
DIA|2026-10|horas_intra_indenizada|col450|2026-10-11|BATIDA|1|0|-1
MEDIDO|2026-10|col456
MEDIDO|2026-10|col468
DIA|2026-10|horas_extras_100_noturna|col468|2026-10-12|ESTRUTURA|0|0.55090891944444442|0.55090891944444442
MEDIDO|2026-10|col474
HISTORICO_T|2026-10|col492|sem_resultado_do_fechamento|1
EXCLUIDO|2026-10|col492|PRODUTO:SEM_RETORNO_FECHAMENTO
MEDIDO|2026-10|col504
MEDIDO|2026-10|col510
MEDIDO|2026-10|col516
MEDIDO|2026-10|col522
RECUSA|2026-10|col534|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=30
EXCLUIDO|2026-10|col534|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col552
MEDIDO|2026-10|col558
MEDIDO|2026-10|col564
MEDIDO|2026-10|col570
MEDIDO|2026-10|col576
MEDIDO|2026-10|col594
MEDIDO|2026-10|col600
MEDIDO|2026-10|col612
MEDIDO|2026-10|col618
MEDIDO|2026-10|col624
RECUSA|2026-10|col642|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=23
EXCLUIDO|2026-10|col642|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col648
MEDIDO|2026-10|col654
MEDIDO|2026-10|col666
RECUSA|2026-10|col678|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=0
EXCLUIDO|2026-10|col678|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col696
MEDIDO|2026-10|col726
MEDIDO|2026-10|col744
MEDIDO|2026-10|col750
RECUSA|2026-10|col786|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=10
EXCLUIDO|2026-10|col786|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col822
MEDIDO|2026-10|col834
RECUSA|2026-10|col840|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=1
EXCLUIDO|2026-10|col840|PRODUTO:RECUSA_PORTA_SEM_DONO
HISTORICO_T|2026-10|col846|sem_resultado_do_fechamento|1
EXCLUIDO|2026-10|col846|PRODUTO:SEM_RETORNO_FECHAMENTO
MEDIDO|2026-10|col852
MEDIDO|2026-10|col864
HISTORICO_T|2026-10|col870|sem_resultado_do_fechamento|1
EXCLUIDO|2026-10|col870|PRODUTO:SEM_RETORNO_FECHAMENTO
MEDIDO|2026-10|col876
MEDIDO|2026-10|col882
MEDIDO|2026-10|col888
HISTORICO_T|2026-10|col894|sem_resultado_do_fechamento|1
EXCLUIDO|2026-10|col894|PRODUTO:SEM_RETORNO_FECHAMENTO
MEDIDO|2026-10|col900
HISTORICO_T|2026-10|col906|sem_resultado_do_fechamento|1
EXCLUIDO|2026-10|col906|PRODUTO:SEM_RETORNO_FECHAMENTO
MEDIDO|2026-10|col912
MEDIDO|2026-10|col918
MEDIDO|2026-10|col924
MEDIDO|2026-10|col930
MEDIDO|2026-10|col936
RECUSA|2026-10|col942|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=0
EXCLUIDO|2026-10|col942|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col948
MEDIDO|2026-10|col954
MEDIDO|2026-10|col960
MEDIDO|2026-10|col966
MEDIDO|2026-10|col972
MEDIDO|2026-10|col978
MEDIDO|2026-10|col984
MEDIDO|2026-10|col990
RECUSA|2026-10|col996|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=2
EXCLUIDO|2026-10|col996|PRODUTO:RECUSA_PORTA_SEM_DONO
RECUSA|2026-10|col1002|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=0
EXCLUIDO|2026-10|col1002|PRODUTO:RECUSA_PORTA_SEM_DONO
COBERTURA_COLAB|2026-10|medidos=86|universo_parte=110|excluidos=24
COBERTURA_INTEGRAL|2026-10|False
PRODUTO|2026-10|DIA_FORA_UNIVERSO_PORTA|56|296
PRODUTO|2026-10|RECUSA_PORTA_SEM_DONO|18|237
PRODUTO|2026-10|SEM_DECISAO_horas_atraso|59|326
PRODUTO|2026-10|SEM_DECISAO_horas_extras_100|59|321
PRODUTO|2026-10|SEM_DECISAO_horas_extras_100_feriado|59|321
PRODUTO|2026-10|SEM_DECISAO_horas_extras_100_noturna|59|321
PRODUTO|2026-10|SEM_DECISAO_horas_extras_50|59|321
PRODUTO|2026-10|SEM_DECISAO_horas_extras_50_noturna|59|321
PRODUTO|2026-10|SEM_DECISAO_horas_folga_trabalhada|56|296
PRODUTO|2026-10|SEM_DECISAO_horas_intra_indenizada|56|296
PRODUTO|2026-10|SEM_DECISAO_horas_noturnas|59|312
PRODUTO|2026-10|SEM_DECISAO_horas_saida_antecipada|59|326
PRODUTO|2026-10|SEM_DECISAO_horas_trabalhadas|56|296
PRODUTO|2026-10|SEM_RETORNO_FECHAMENTO|6|0
DIAS_NAO_LOCALIZADOS|2026-10|PRODUTO|SEM_RETORNO_FECHAMENTO|colabs=6
DONO|2026-10|horas_trabalhadas|CADASTRO|7.3164730555555568|4.7735825000000007|-2.5428905555555561|1|1
RESIDUO|2026-10|horas_trabalhadas|ATE_LIMIAR|11259.129095282779|11259.129095282779|8.8817841970012523e-16|86|1222
RESIDUO|2026-10|horas_trabalhadas|SEM_DECISAO|54.306727913055553|54.306727913055553|0|56|296
PARCIAL|2026-10|horas_trabalhadas|v2_h=11266.445568338333|v3_h=11263.902677782778|autoridade2_h=11320.752296251389|autoridade3_h=11318.209405695834|porta_h=11263.902677782778|emitido_decidido_h=11263.902677782778|fm_dias=11320.72|fm_retorno=11320.72|colabs_div=1|dias_colab_div=1
RUBRICA|2026-10|horas_trabalhadas|11266.445568338333|11263.902677782778|-2.5428905555545498|1|1
INSTRUMENTO_HISTORICO|2026-10|horas_trabalhadas|v2_h=11229.414913473611|v3_h=11263.902677782777|delta_h=34.487764309165868|colabs=86
DONO|2026-10|horas_intra_indenizada|BATIDA|2|0|-2|1|2
RESIDUO|2026-10|horas_intra_indenizada|ATE_LIMIAR|368.75561535805554|368.75561535805554|0|86|1221
RESIDUO|2026-10|horas_intra_indenizada|SEM_DECISAO|0|0|0|56|296
PARCIAL|2026-10|horas_intra_indenizada|v2_h=370.75561535805554|v3_h=368.75561535805554|autoridade2_h=370.75561535805554|autoridade3_h=368.75561535805554|porta_h=368.75561535805554|emitido_decidido_h=368.75561535805554|fm_dias=370.76|fm_retorno=370.76|colabs_div=1|dias_colab_div=2
RUBRICA|2026-10|horas_intra_indenizada|370.75561535805554|368.75561535805554|-2|1|2
INSTRUMENTO_HISTORICO|2026-10|horas_intra_indenizada|v2_h=370.18784535805554|v3_h=368.75561535805554|delta_h=-1.4322300000000041|colabs=86
RESIDUO|2026-10|horas_extras_50|ATE_LIMIAR|0.21736972222222259|0.21736972222222259|0|84|1198
RESIDUO|2026-10|horas_extras_50|SEM_DECISAO|0|0|0|59|321
PARCIAL|2026-10|horas_extras_50|v2_h=0.21736972222222259|v3_h=0.21736972222222259|autoridade2_h=0.21736972222222259|autoridade3_h=0.21736972222222259|porta_h=0.21736972222222259|emitido_decidido_h=0.21736972222222259|fm_dias=0.22|fm_retorno=0.22|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_extras_50|0.21736972222222259|0.21736972222222259|0|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_extras_50|v2_h=0.21736972222222259|v3_h=0.21736972222222259|delta_h=0|colabs=86
RESIDUO|2026-10|horas_extras_100|ATE_LIMIAR|75.71780262916667|75.71780262916667|-6.2172489379008766e-15|84|1198
RESIDUO|2026-10|horas_extras_100|SEM_DECISAO|0|0|0|59|321
PARCIAL|2026-10|horas_extras_100|v2_h=75.71780262916667|v3_h=75.71780262916667|autoridade2_h=75.71780262916667|autoridade3_h=75.71780262916667|porta_h=75.71780262916667|emitido_decidido_h=75.71780262916667|fm_dias=75.72|fm_retorno=75.72|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_extras_100|75.71780262916667|75.71780262916667|0|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100|v2_h=75.71780262916667|v3_h=75.71780262916667|delta_h=0|colabs=86
RESIDUO|2026-10|horas_extras_100_feriado|ATE_LIMIAR|75.71780262916667|75.71780262916667|-6.2172489379008766e-15|84|1198
RESIDUO|2026-10|horas_extras_100_feriado|SEM_DECISAO|0|0|0|59|321
PARCIAL|2026-10|horas_extras_100_feriado|v2_h=75.71780262916667|v3_h=75.71780262916667|autoridade2_h=75.71780262916667|autoridade3_h=75.71780262916667|porta_h=75.71780262916667|emitido_decidido_h=75.71780262916667|fm_dias=75.72|fm_retorno=75.72|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_extras_100_feriado|75.71780262916667|75.71780262916667|0|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100_feriado|v2_h=0|v3_h=75.71780262916667|delta_h=75.71780262916667|colabs=86
RESIDUO|2026-10|horas_extras_50_noturna|ATE_LIMIAR|0|0|0|84|1198
RESIDUO|2026-10|horas_extras_50_noturna|SEM_DECISAO|0|0|0|59|321
PARCIAL|2026-10|horas_extras_50_noturna|v2_h=0|v3_h=0|autoridade2_h=0|autoridade3_h=0|porta_h=0|emitido_decidido_h=0|fm_dias=0.0|fm_retorno=0.0|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_extras_50_noturna|0|0|0|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_extras_50_noturna|v2_h=0|v3_h=0|delta_h=0|colabs=86
DONO|2026-10|horas_extras_100_noturna|ESTRUTURA|0|2.0639545019444445|2.0639545019444445|3|3
RESIDUO|2026-10|horas_extras_100_noturna|ATE_LIMIAR|10.749999178333333|10.747388900555556|-0.0026102777777774477|84|1195
RESIDUO|2026-10|horas_extras_100_noturna|SEM_DECISAO|0|0|0|59|321
PARCIAL|2026-10|horas_extras_100_noturna|v2_h=10.749999178333333|v3_h=12.8113434025|autoridade2_h=10.749999178333333|autoridade3_h=12.8113434025|porta_h=12.8113434025|emitido_decidido_h=12.8113434025|fm_dias=10.75|fm_retorno=10.75|colabs_div=3|dias_colab_div=3
RUBRICA|2026-10|horas_extras_100_noturna|10.749999178333333|12.8113434025|2.0613442241666675|3|3
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100_noturna|v2_h=0|v3_h=12.8113434025|delta_h=12.8113434025|colabs=86
RESIDUO|2026-10|horas_folga_trabalhada|ATE_LIMIAR|0|0|0|86|1223
RESIDUO|2026-10|horas_folga_trabalhada|SEM_DECISAO|0|0|0|56|296
PARCIAL|2026-10|horas_folga_trabalhada|v2_h=0|v3_h=0|autoridade2_h=0|autoridade3_h=0|porta_h=0|emitido_decidido_h=0|fm_dias=0.0|fm_retorno=0.0|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_folga_trabalhada|0|0|0|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_folga_trabalhada|v2_h=91.337382777777776|v3_h=0|delta_h=-91.337382777777776|colabs=86
RESIDUO|2026-10|horas_noturnas|ATE_LIMIAR|2366.7224471582936|2366.7224471582936|-7.9936057773011271e-15|85|1207
RESIDUO|2026-10|horas_noturnas|SEM_DECISAO|0|0|0|59|312
PARCIAL|2026-10|horas_noturnas|v2_h=2366.7224471582936|v3_h=2366.7224471582936|autoridade2_h=2366.7224471582936|autoridade3_h=2366.7224471582936|porta_h=2366.7224471582936|emitido_decidido_h=2366.7224471582936|fm_dias=2366.73|fm_retorno=2366.73|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_noturnas|2366.7224471582936|2366.7224471582936|0|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_noturnas|v2_h=2331.4232703725793|v3_h=2372.086056245595|delta_h=40.662785873015764|colabs=86
DONO|2026-10|horas_atraso|ESTRUTURA|0|2.3685|2.3685|1|1
RESIDUO|2026-10|horas_atraso|ATE_LIMIAR|57.825166666666668|57.825468780000001|0.00030211333333321488|83|1192
RESIDUO|2026-10|horas_atraso|SEM_DECISAO|0|0|0|59|326
PARCIAL|2026-10|horas_atraso|v2_h=57.825166666666668|v3_h=60.193968779999999|autoridade2_h=57.825166666666668|autoridade3_h=60.193968779999999|porta_h=60.193968779999999|emitido_decidido_h=60.193968779999999|fm_dias=57.83|fm_retorno=57.83|colabs_div=1|dias_colab_div=1
RUBRICA|2026-10|horas_atraso|57.825166666666668|60.193968779999999|2.3688021133333308|1|1
INSTRUMENTO_HISTORICO|2026-10|horas_atraso|v2_h=55.44233333333333|v3_h=55.442635626111112|delta_h=0.00030229277778204278|colabs=86
RESIDUO|2026-10|horas_saida_antecipada|ATE_LIMIAR|96.867833333333337|96.868517868055562|0.00068453472222396172|83|1193
RESIDUO|2026-10|horas_saida_antecipada|SEM_DECISAO|0|0|0|59|326
PARCIAL|2026-10|horas_saida_antecipada|v2_h=96.867833333333337|v3_h=96.868517868055562|autoridade2_h=96.867833333333337|autoridade3_h=96.868517868055562|porta_h=96.868517868055562|emitido_decidido_h=96.868517868055562|fm_dias=96.87|fm_retorno=96.87|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_saida_antecipada|96.867833333333337|96.868517868055562|0.00068453472222529399|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_saida_antecipada|v2_h=92.6965|v3_h=92.697184534722226|delta_h=0.00068453472222529399|colabs=86
UNIVERSO|2026-11|colabs_porta=578
UNIVERSO_PARTE 2026-11 96
RECUSA|2026-11|col30|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=22
EXCLUIDO|2026-11|col30|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-11|col42
MEDIDO|2026-11|col60
RECUSA|2026-11|col66|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=0
EXCLUIDO|2026-11|col66|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-11|col78
MEDIDO|2026-11|col84
RECUSA|2026-11|col90|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=0
EXCLUIDO|2026-11|col90|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-11|col108
MEDIDO|2026-11|col114
MEDIDO|2026-11|col120
RECUSA|2026-11|col144|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=15
EXCLUIDO|2026-11|col144|PRODUTO:RECUSA_PORTA_SEM_DONO
RECUSA|2026-11|col150|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=24
EXCLUIDO|2026-11|col150|PRODUTO:RECUSA_PORTA_SEM_DONO
RECUSA|2026-11|col168|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_t

[truncado em 20000 de 43374 caracteres]

## 06/11 14:10 · sessao d172c057eb85 · R1-PUBLICA · verificador · origem=Code

**Os números conferem com a soma do anexo. Há achados procedentes na publicação, na garantia de “uma medição” e na atribuição das verificações ao comando.** Referências: `app/core/placar_estrutural.py:106`, `app/core/placar_estrutural.py:118`, `bin/gerar_estado.py:164`, `app/core/tests/test_r1_termometro_uma_medicao.py:29`.

1. **Procedente — o publicador elimina os números novos dos seis itens.** [nao verificado]

   `bin/gerar_estado.py:164` publica apenas os primeiros **150 caracteres** de cada prova. Nos seis textos novos, esses caracteres são idênticos e terminam em `578 na 11 = soma das partes,`. O caminho começa na posição 152; os números específicos vêm depois. Assim, a tabela do ESPELHO-VERDADE publicada perde BATE, divergências, ímpares, zeros e referência da medição. Procedência: recorte em memória dos textos de `app/core/espelho_verdade.py:49`, `:82`, `:169`, `:173`, `:177`, `:179`. [Publicador](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/bin/gerar_estado.py:164).

2. **Procedente — o selo garante correspondência com uma glob, não identidade da medição.**

   `_cita_a_medicao` aceita **qualquer** caminho que case com `logs/r1remede/r1_dono_*.txt`. Duas provas com datas diferentes passam simultaneamente; até uma prova contendo literalmente essa glob passa. Enquanto isso, `_medido_em` escolhe a maior data entre todos os arquivos encontrados, sem conferir o arquivo citado na prova. Referências: [matcher](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/core/tests/test_r1_termometro_uma_medicao.py:29), `app/core/placar_estrutural.py:111`, [seleção da data](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/bin/gerar_estado.py:51).

   Outro escape: retirar `TERMOMETRO R1` do início de uma prova remove aquele item da fiscalização. A proteção exige apenas que reste algum item, sem detectar a redução do conjunto. Referências: `app/core/tests/test_r1_termometro_uma_medicao.py:37`, `app/core/tests/test_r1_termometro_uma_medicao.py:56`.

   Reproduzi em memória os quatro métodos, substituindo somente `SimpleTestCase` por `unittest.TestCase`: versão atual **4/0/0**; mutante com outra data na mesma glob **4/0/0**; mutante retirando o prefixo de um item **4/0/0**. Procedência: métodos de `app/core/tests/test_r1_termometro_uma_medicao.py:43`. Execução Django/canônica e RED histórico de `logs/r1pub/red.log`: **[nao verificado]**.

   O selo **tem mordida contra a pasta antiga**, demonstrada pelo caso negativo `logs/sombra/...` e pelos positivos fictícios. Não copia os valores atuais, nem consulta relógio. Sua mordida é insuficiente para a propriedade anunciada de “uma medição”. Referências: `app/core/tests/test_r1_termometro_uma_medicao.py:14`, `:60`, `:63`.

3. **Procedente — “o próprio comando cobra” excede o código disponível.**

   A publicação atribui ao comando 36 verificações `SOMA True`, incluindo horas. Porém, nesta cópia, ele apenas imprime a soma dos donos e das classes: não compara para produzir booleano, não exige igualdade e não imprime verificações de horas ou união. Referências: [publicação](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/core/placar_estrutural.py:106), `app/core/placar_estrutural.py:108`, `app/core/placar_estrutural.py:118`, [comando](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/ponto/management/commands/e6_oraculo.py:415), `app/ponto/management/commands/e6_oraculo.py:424`.

   As **36 linhas True** existem no anexo fornecido. A implementação da sonda que emitiu `PARCIAL`, `PARCIAL_TOTAL`, `UNIVERSO_PARTE` e as verificações adicionais não foi fornecida: **[nao verificado]**. Isso impede atribuir tais verificações ao comando desta cópia ou conferir como a sonda particionou os colaboradores; os argumentos do comando não incluem particionamento. Referência: `app/ponto/management/commands/e6_oraculo.py:264`.

A conferência numérica abaixo usa exclusivamente as seis partes fornecidas. A correspondência com arquivos físicos de logs e banco é **[nao verificado]**.

| Campo — soma das seis partes | 10/2026 | 11/2026 | Publicado em | [nao verificado]
|---|---:|---:|---|
| Universo | 624 | 578 | `app/core/placar_estrutural.py:118` |
| BATE / comparados | 7180 / 7483 | 3680 / 3881 | `app/core/placar_estrutural.py:118` |
| Percentual arredondado | 95,95% | 94,82% | `app/core/placar_estrutural.py:118` |
| ESTRUTURA: dias / horas / colabs | 26 / 88,36 / 16 | 9 / 40,62 / 7 | `app/core/placar_estrutural.py:118` |
| CADASTRO: dias / horas / colabs | 78 / 306,53 / 32 | 72 / 347,49 / 33 | `app/core/placar_estrutural.py:118` |
| BATIDA: dias / horas / colabs | 199 / 961,16 / 113 | 120 / 643,06 / 78 | `app/core/placar_estrutural.py:118` |
| Total divergente: dias / horas / colabs | 303 / 1356,05 / 144 | 201 / 1031,17 / 105 | `app/core/placar_estrutural.py:118` |
| `dia_batida_impar` | 347 | 262 | `app/core/espelho_verdade.py:82` |
| `impar_FORA_da_faixa` | 31 | 25 | `app/core/espelho_verdade.py:82` |
| `impar_que_BATE` / `impar_que_DIVERGE` | 148 / 199 | 123 / 139 | `app/core/espelho_verdade.py:177` |
| `impar_no_conjunto_do_dia` | 198 | 137 | `app/core/espelho_verdade.py:177` |
| `impar_SO_POR_CORTE_do_oraculo` | 1 | 2 | `app/core/espelho_verdade.py:177` |
| `esp_zero_e6_trabalho` | 35 | 28 | `app/core/espelho_verdade.py:49` |

**Nenhuma divergência aritmética ou inversão de chave foi encontrada nesses valores.** Os totais de colaboradores reproduzem a soma dos `PARCIAL_TOTAL`; comprovar que essa soma representa união global exige partes disjuntas, cuja composição é **[nao verificado]**. O comando normal calcula conjuntos por dono e união dos divergentes. Referências: `app/ponto/management/commands/e6_oraculo.py:369`, `:429`.

`esp_zero_e6_trabalho` significa espelho zero e diferença superior a dez minutos; a chave inversa, `e6_zero_esp_trabalho`, significa oráculo zero e soma **19/14** no anexo. `None` sem lavratura não é contado como zero. Referências: `app/ponto/management/commands/e6_oraculo.py:251`, `:353`, `:358`. As horas publicadas representam **soma das diferenças absolutas**, dividida por sessenta. Referências: `app/ponto/management/commands/e6_oraculo.py:368`, `:423`.

O censo dos campos alterados ficou assim:

| Campo | Quem escreve a declaração | Quem lê |
|---|---|---|
| `R1.numero/prova` | `app/core/placar_estrutural.py:118`, `:120` | `app/core/placar_estrutural.py:635`, `:637`; número publicado em `bin/gerar_estado.py:116`; prova no selo novo em `app/core/tests/test_r1_termometro_uma_medicao.py:45`; strings no selo host em `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:67` |
| `R1.fonte_do_medido` | `app/core/placar_estrutural.py:111` | `bin/gerar_estado.py:47`; selo novo em `app/core/tests/test_r1_termometro_uma_medicao.py:45`, `:50`, `:54`; presença exigida em `app/core/tests/test_l114_placar_papel_e_termometro.py:98`; selo host em `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:67` |
| `SOMA.numero/prova` | `app/core/placar_estrutural.py:106`, `:107` | `app/core/placar_estrutural.py:635`, `:637`; publicação em `bin/gerar_estado.py:99`, `:100`; prova no selo novo em `app/core/tests/test_r1_termometro_uma_medicao.py:50`; selo host em `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:67` |
| Seis `ETAPAS[…].prova` | `app/core/espelho_verdade.py:49`, `:82`, `:169`, `:173`, `:177`, `:179` | `app/core/espelho_verdade.py:201`, `:203`; publicação em `bin/gerar_estado.py:164`; seleção e conferência no selo novo em `app/core/tests/test_r1_termometro_uma_medicao.py:39`, `:57` |

O escritor final do ESTADO é `bin/gerar_estado.py:376`; a publicação passa pelo raspador em `bin/relato.sh:103`. A prova do R1 participa da verificação de presença, mas sua tabela de termômetro não a exibe. Referências: `app/core/placar_estrutural.py:637`, `bin/gerar_estado.py:110`, `:116`.

Quanto aos demais leitores e selos:

- **Glob/data:** o novo nome é compatível e produz `2026-11-06` quando presente. Sem arquivo datado, o placar fica amarelo. Existência no host: **[nao verificado]**. Referências: `bin/gerar_estado.py:51`, `:52`, `app/core/placar_estrutural.py:680`.
- **Hash citado:** o selo exige frase declarada de “fora do ar” junto de hash conhecido e ancestral; os textos novos não introduzem essa combinação. A ancestralidade real não pôde ser conferida nesta cópia: **[nao verificado]**. Referências: `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:47`, `:72`, `:75`.
- **Data cravada:** o censo real, executado em memória, não apontou ocorrência no placar; `06/11` e o sufixo `20261106` não casam os padrões proibidos. Referências: `app/core/datas_cravadas.py:26`, `app/core/tests/test_l114_placar_papel_e_termometro.py:90`.
- **Nome/CPF:** o selo público depende da lista do banco e do repositório publicado; resultado real **[nao verificado]**. Ele não exige a pasta antiga de logs. Referências: `bin/tests/test_publicacao_sem_nome.sh:39`, `:47`, `:60`, `:72`.
- **Prova obrigatória:** as provas novas continuam não vazias; os verificadores existentes só exigem presença, sem validar seu conteúdo. Referências: `app/core/placar_estrutural.py:637`, `app/core/espelho_verdade.py:201`.
- **Leitores de R6:** `handoff_sessao` e o selo da frase do juiz extraem somente a declaração de contratos do R6, fora dos campos alterados. Referências: `bin/handoff_sessao.sh:179`, `app/core/tests/test_selo_r6_frase_do_juiz.py:17`.

Há ainda estas qualificações de texto e procedência:

4. **Não sustentado pela medida — “a saída que falta é cobrança, não cura de motor”.** A medida não identifica essa causa exclusiva; o próprio oráculo admite artefatos de corte e casos em que a tela está certa. A frase foi mantida no texto atualizado. Referências: `app/core/espelho_verdade.py:82`, `app/ponto/management/commands/e6_oraculo.py:25`, `:372`.

5. **Escopo excessivo — “nenhum fica fora em silêncio”.** A igualdade dos ímpares vale entre os dias comparados. Antes de classificar paridade, o código exclui dias sem linha, isentos/ausentes, sem lavratura, zero em ambos ou com diferença explicada pela janela declarada. A paridade desses excluídos é **[nao verificado]**. Referências: `app/core/espelho_verdade.py:177`, `app/ponto/management/commands/e6_oraculo.py:324`, `:347`.

   Além disso, **198+1 e 137+2 repartem somente os ímpares divergentes**; 31/25 FORA também pertencem a esse subconjunto. Os números estão certos, mas o texto deveria explicitar esse alcance. Referências: `app/ponto/management/commands/e6_oraculo.py:370`, `:381`, `:386`.

6. **Destino é política, não execução comprovada.** “CADASTRO e BATIDA vão à lista do admin” tem respaldo normativo na L-099; o anexo não comprova entradas efetivas para esses colaboradores: **[nao verificado]**. O novo R1 corretamente declara que não mediu causa/destino dos dias ESTRUTURA. Referências: `app/docs/LEIS.md:122`, `app/core/espelho_verdade.py:173`, `app/core/placar_estrutural.py:118`.

7. **Restou texto antigo:** `nota_do_papel` ainda afirma que a medição aberta é de **09/10** e que a cadência “foi CUMPRIDA”, enquanto número/prova passaram a 06/11. A docstring nova também fala em **cinco** itens antigos, embora o diff altere seis. Referências: `app/core/placar_estrutural.py:119`, `app/core/tests/test_r1_termometro_uma_medicao.py:9`, `app/core/espelho_verdade.py:177`.

   Existe ainda publicação independente de números E6 por `logs/e6_placar.json`, fora do selo novo. Se conserva valores antigos é **[nao verificado]**. Referências: `bin/gerar_estado.py:145`, `:168`, `app/core/espelho_verdade.py:208`.

8. **“0 erro” não foi reafirmado — corretamente.** `rc_sombra=0` não demonstra zero exceções capturadas: o comando as acumula separadamente. Essa linha não consta do anexo. Referências: `app/core/espelho_verdade.py:169`, `app/ponto/management/commands/e6_oraculo.py:305`, `:396`. Ausência da palavra `INCOMPLETO` no recorte tampouco certifica completude da execução: **[nao verificado]**.

Das classes de reprovação enumeradas, aparece **selo com mordida insuficiente para a propriedade declarada**, comprovado pelos contraexemplos acima. O diff não introduz segundo juiz, segundo escritor operacional ou nova leitura de relógio; o relógio continua na borda publicadora. Referências: `app/core/tests/test_r1_termometro_uma_medicao.py:29`, `app/core/placar_estrutural.py:106`, `:118`, `bin/gerar_estado.py:79`. Derivação da sonda/agregador, ordem de travas, suíte completa, leitura final e pouso após pré-requisitos: **[nao verificado]**.

## 06/11 14:08 · sessao df46b1c224d2 · V3-IMPACTO-COM-DONO · leitura · origem=Code

**Nos caminhos de produto examinados, o leitor continua em `motor`; a ausência de `oraculo` não altera o número exibido nem cria alerta específico.** O fechamento escreve as versões separadamente, e os leitores usam `motor` por padrão ou filtro explícito (`app/ponto/services/fechamento.py:1211`, `:1278`; `app/ponto/services/dia_pago.py:548`, `:750`, `:831`; `app/folha/porta_export.py:589`). As contagens informadas da sombra e a situação atual do banco: **[não verificado]**.

**1. Quais entradas deixam campos sem dono**

A porta considera dono a **presença da chave**, tanto em `valores` quanto em `nao_decide`; também aceita os mapas externos vazios, desde que diferentes de `None`. A união é feita sobre **todos os dias da chamada**, não individualmente por dia (`app/ponto/services/dia_pago.py:50–75`).

No fechamento, falta e os três mapas de grade nascem como `{}` e são passados à lavratura; `do_motor` não é informado. Portanto, quando `rubricas_por_dia` devolve `{}`, ficam **11 campos sem dono** (`app/ponto/services/fechamento.py:844–849`, `:1261–1278`; `app/ponto/services/dia_pago.py:139–140`, `:34–41`, `:60–75`):

```text
horas_trabalhadas
horas_noturnas
horas_extras_50
horas_extras_50_noturna
horas_extras_100
horas_extras_100_feriado
horas_extras_100_noturna
horas_folga_trabalhada
horas_atraso
horas_saida_antecipada
horas_intra_indenizada
```

O universo visitado por `rubricas_por_dia` é `v.minutos` mais os dias publicados pelo motor como bloqueados, recortado à janela. Se esse universo ficar vazio, retorna `{}` (`app/ponto/calculador/alimentacao.py:449–475`, `:533`).

| Entrada/cenário | Efeito sobre os donos |
|---|---|
| Nenhum par fechado na competência; somente turnos abertos; pares existentes apenas fora da janela | Pode resultar em `{}`: o núcleo não acrescenta minutos para par aberto, e a alimentação filtra os dias pela janela (`app/ponto/calculador/nucleo.py:139–155`; `app/ponto/calculador/alimentacao.py:467–475`). |
| Períodos sem dia no mapa, sem carimbo efetivo, ou invertidos/colapsados sem publicação como bloqueados | Esses períodos podem não fornecer pares ao núcleo; se nenhum dia restar, resulta em `{}` (`app/ponto/calculador/alimentacao.py:73–92`, `:109–123`, `:449–471`). |
| Colaborador desligado | O estado sozinho não determina rubricas vazias. O universo exclui desligado sem data ou com demissão anterior ao início da janela da empresa; quem entra ainda depende dos períodos efetivos (`app/ponto/services/fechamento.py:419–436`; `app/ponto/calculador/alimentacao.py:449–475`). |
| Afastado o mês inteiro, sem pares fechados nem dias bloqueados | Pode resultar em `{}`. Afastamento não exclui automaticamente do universo; ausências que suprimem previsto são tratadas no fechamento, mas não acrescentam dias a `rubricas_por_dia` (`app/ponto/services/fechamento.py:391–401`, `:684–692`; `app/ponto/calculador/alimentacao.py:459–475`). |
| Único dia bloqueado por HE100 | **Já está contemplado nesta árvore**: a alimentação visita o dia com zero. Há teste escrito exigindo lavrar esse zero com donos completos (`app/ponto/calculador/alimentacao.py:459–471`; `app/ponto/tests/test_he100_dia_sem_previsto.py:242–248`, `:277–287`). |
| Somente folgas trabalhadas, com escala certa respondida | **Já está contemplado**: o ramo declara também os dois splits noturnos; há teste exigindo existência do oráculo (`app/ponto/calculador/regras.py:704–739`; `app/ponto/tests/test_he100_dia_sem_previsto.py:233–240`). |
| Dia sem trabalho previsto, não intermitente, com `certa_cache=None` | A alimentação passa `escala_certa=None`; o ramo retorna `Rubricas({}, _nd, ...)`, deixando pelo menos trabalhadas, intra e HE50 sem declaração. É uma entrada possível da função, mas o fechamento mostrado inicializa e passa um cache `{}` (`app/ponto/calculador/alimentacao.py:524`; `app/ponto/calculador/regras.py:643–645`, `:664`, `:740–741`; `app/ponto/services/fechamento.py:923`, `:1257`). |

Falta de CCT, pontas de HE ou pontualidade **não significa automaticamente campo sem dono**: esses campos entram em `nao_decide`, que a porta contabiliza como declaração (`app/ponto/calculador/regras.py:604–644`; `app/ponto/services/dia_pago.py:63`).

**O que deveria aceitar ou recusar pela lei escrita:**

- **Aceitar resposta explicitamente vazia/zero, sustentada pela autoridade.** A casa distingue “perguntei e não havia” de “não perguntei”; exige zero declarado para dia sem turno pareado e prevê uma linha de ajuste mesmo quando zerada (`app/ponto/services/dia_pago.py:54–58`, `:346–356`; `app/docs/LEIS.md:129`).
- **Recusar campo omitido sem valor, causa ou proveniência declarada.** Essa é precisamente a finalidade escrita da porta; regra ausente não pode virar zero (`app/ponto/services/dia_pago.py:303–319`; `app/ponto/calculador/regras.py:9–13`).
- **`rubricas={}` sozinho não distingue essas situações.** Hoje a porta recusa ambos os casos; liberar indiscriminadamente esse dicionário esconderia também falha de alimentação. A distinção precisa vir da autoridade que fornece os dias, preservando a recusa de campos sem declaração (`app/ponto/calculador/alimentacao.py:459–466`; `app/ponto/services/dia_pago.py:310–319`; `CLAUDE.md:17–21`, `:35–36`).

A recusa acontece **antes de apagar as linhas anteriores**. Logo, pode deixar o colaborador sem primeira lavratura ou preservar um oráculo antigo; o `except` apenas registra ERROR e o fechamento incrementa `processados` (`app/ponto/services/dia_pago.py:310–319`, `:357–358`; `app/ponto/services/fechamento.py:1281–1286`).

**2. Quem lê e o que aparece sem oráculo**

Nos caminhos abaixo, **não há seleção de `versao='oraculo'`**; ausência dessa versão é invisível para o leitor indicado, pelas consultas citadas em cada linha.

| Leitor | Fonte e comportamento quando falta apenas `oraculo` |
|---|---|
| Tela do espelho | Continua mostrando linhas `motor`; o helper é chamado sem alterar a versão padrão. O resumo usa fechamento e, sem fechamento, soma da lavratura `motor` (`app/ponto/services/espelho.py:406–410`; `app/ponto/services/dia_pago.py:831–854`; `app/relatorios/cartao_pela_celula.py:323–345`, `:141–143`). |
| Calendário do colaborador | Continua mostrando rubricas `motor`, obtidas por `linhas_do_periodo` (`app/colaboradores/services/calendario.py:290`, `:655–656`; `app/ponto/services/dia_pago.py:831–854`). |
| PDF/cartão | Continua imprimindo `pago_h` e rubricas preparados pelo espelho; os totais seguem a mesma seleção entre fechamento e lavratura `motor` (`app/relatorios/pdf_espelho.py:837–839`, `:901–914`; `app/relatorios/cartao_pela_celula.py:323–345`). |
| Extrato parcial | Continua somando `motor`, pois chama `soma_do_periodo` sem versão alternativa (`app/relatorios/views.py:481–487`; `app/ponto/services/dia_pago.py:750`, `:782–785`). |
| Gestão de HE e informação ao DP na autorização por período | Continuam lendo a soma `motor` e o respectivo rótulo de disponibilidade (`app/ponto/services/gestao_he.py:256–257`, `:297–298`; `app/ponto/services/autorizacao_he_periodo.py:172–175`; `app/ponto/services/dia_pago.py:750`). |
| App/API | Continua lendo `motor`; o payload recebe horas e `dia_pago_lavrada` dessa fonte (`app/api/views.py:1454–1457`, `:1539–1558`; `app/ponto/services/dia_pago.py:831`). |
| TXT/export | As rubricas saem do `FechamentoMensal`, não da versão oráculo. A montagem percorre os fechamentos e seus eventos (`app/folha/export.py:703–724`). |
| Conferência/placar S2 e R4 | Comparam/somam `motor`. A falta apenas de oráculo não entra em `colabs_sem_lavratura`; esse contador nasce quando a soma `motor` retorna `None` (`app/folha/porta_export.py:388–393`, `:588–597`; `app/ponto/services/dia_pago.py:548–559`). O comando publica o contador, que permanece fora de `falhas` (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:67–69`, `:88–107`; `app/folha/porta_export.py:687–701`). |
| Termômetro R1/E6 | O “oráculo” aqui é o cálculo independente sobre batidas apuráveis; não é a linha `DiaPago.versao='oraculo'`. A comparação continua usando esse resultado (`app/ponto/management/commands/e6_oraculo.py:144–146`, `:304–318`; `app/core/placar_estrutural.py:115–116`). |

Se **também faltar `motor`**, há comportamentos distintos: espelho mostra “sem apuração ainda”; extrato distingue “sem lavratura” de “sem apuração ainda”; a conferência registra `colabs_sem_lavratura`; o app entrega horas do período com `dia_pago_lavrada=False` (`app/templates/ponto/espelho.html:268–271`; `app/relatorios/views.py:516–536`; `app/folha/porta_export.py:388–393`; `app/ponto/services/espelho.py:129–136`).

**3. Sonda integral — somente leitura**

O script abaixo emite um `SELECT`, com tabelas qualificadas no schema `juliani`. Conta colaboradores distintos com **qualquer linha da versão**, inclusive ajuste; filtra a competência por `mes`/`ano`, pois `data` é nula no ajuste (`app/ponto/models.py:311–323`; `app/ponto/services/dia_pago.py:355–356`). A empresa é a vinculada atualmente ao colaborador (`app/colaboradores/models.py:321`).

A janela usa `Empresa.dia_inicio_competencia` pela função existente; com corte 21, outubro é **21/09–20/10** e novembro **21/10–20/11** (`app/colaboradores/models.py:46`; `app/ponto/janelas.py:10–21`, `:36–57`). **Script não executado; resultados [não verificado].**

```python
from collections import defaultdict
from types import SimpleNamespace

from django.db import connection

from colaboradores.models import Colaborador, Empresa
from ponto.janelas import janela_fechamento
from ponto.models import DiaPago


SCHEMA = "juliani"
COMPETENCIAS = ((10, 2026), (11, 2026))

# Campos:
# app/ponto/models.py:315-323
# app/colaboradores/models.py:46,321
#
# Janela:
# app/ponto/janelas.py:10-21,36-57
#
# As tabelas são qualificadas pelo schema; o script não troca search_path.
q = connection.ops.quote_name


def tabela(modelo):
    return "%s.%s" % (q(SCHEMA), q(modelo._meta.db_table))


def coluna(modelo, campo):
    if campo == "pk":
        return q(modelo._meta.pk.column)
    return q(modelo._meta.get_field(campo).column)


sql = f"""
SELECT DISTINCT
    e.{coluna(Empresa, "pk")},
    e.{coluna(Empresa, "dia_inicio_competencia")},
    dp.{coluna(DiaPago, "mes")},
    dp.{coluna(DiaPago, "ano")},
    dp.{coluna(DiaPago, "versao")},
    dp.{coluna(DiaPago, "colaborador")}
FROM {tabela(Empresa)} AS e
LEFT JOIN {tabela(Colaborador)} AS c
    ON c.{coluna(Colaborador, "empresa")} = e.{coluna(Empresa, "pk")}
LEFT JOIN {tabela(DiaPago)} AS dp
    ON dp.{coluna(DiaPago, "colaborador")} = c.{coluna(Colaborador, "pk")}
   AND dp.{coluna(DiaPago, "ano")} = %s
   AND dp.{coluna(DiaPago, "mes")} IN (%s, %s)
   AND dp.{coluna(DiaPago, "versao")} IN (%s, %s)
ORDER BY 1, 3, 4, 5, 6
"""

# Única consulta de dados: SELECT.
with connection.cursor() as cursor:
    cursor.execute(sql, (2026, 10, 11, "motor", "oraculo"))
    linhas = cursor.fetchall()

cortes = {}
presenca = defaultdict(set)

for empresa_id, corte, mes, ano, versao, colaborador_id in linhas:
    cortes[empresa_id] = corte
    if colaborador_id is not None:
        presenca[(empresa_id, mes, ano, versao)].add(colaborador_id)

for mes, ano in COMPETENCIAS:
    for empresa_id in sorted(cortes):
        empresa = SimpleNamespace(
            pk=empresa_id,
            dia_inicio_competencia=cortes[empresa_id],
        )
        inicio, fim = janela_fechamento(mes, ano, empresa)

        motor = presenca[(empresa_id, mes, ano, "motor")]
        oraculo = presenca[(empresa_id, mes, ano, "oraculo")]
        faltantes = sorted(motor - oraculo)
        rotulos = ["col%d" % pk for pk in faltantes]

        print(
            "tenant=%s empresa=emp%d competencia=%02d/%d "
            "janela=%s..%s dia_inicio_competencia=%s "
            "n_colabs_motor=%d n_colabs_oraculo=%d "
            "n_motor_sem_oraculo=%d motor_sem_oraculo=%s"
            % (
                SCHEMA,
                empresa_id,
                mes,
                ano,
                inicio.strftime("%d/%m/%Y"),
                fim.strftime("%d/%m/%Y"),
                cortes[empresa_id],
                len(motor),
                len(oraculo),
                len(faltantes),
                "[" + ", ".join(rotulos) + "]",
            )
        )
```

## 06/11 14:08 · sessao 4a8660024d33 · - · - · origem=Ronald (tmux)

Achados para consolidação, todos por leitura estática; não rodei testes nem escrevi arquivos. [nao verificado]

1. **A publicação exagera o que “o próprio comando cobra”.** `SOMA.numero` afirma “36 linhas SOMA True”, “as HORAS também fecham” e “o PROPRIO comando a cobra”; `SOMA.fonte` repete “a soma e cobrada no proprio comando” ([app/core/placar_estrutural.py:106](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/core/placar_estrutural.py:106), :108–109). O comando da cópia apenas imprime a soma dos donos e a soma das classes, sem comparação booleana, `assert`, exceção ou falha; não imprime verificações de horas nem união ([app/ponto/management/commands/e6_oraculo.py:415](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/ponto/management/commands/e6_oraculo.py:415), :424–429). As 36 verificações constam do **anexo**, portanto são sustentadas enquanto saída fornecida; sua atribuição ao próprio comando desta cópia é incorreta. A sonda que acrescentou `UNIVERSO_PARTE`, `PARCIAL`, `PARCIAL_TOTAL`, `SOMA ... HORAS ... True`, `SOMA ... COLABS_UNIAO ... True` não foi fornecida [nao verificado]. Isso também afeta a frase de R1 em :118.

2. **Nenhuma inversão de chave em E1:** `esp_zero_e6_trabalho` soma **35/28**, conforme publicado ([app/core/espelho_verdade.py:49](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/core/espelho_verdade.py:49); anexo p1…p6, existência física [nao verificado]). A classe corresponde a `m_esp == 0`, com diferença absoluta superior a dez minutos; a chave inversa corresponde a `m_e6 == 0` e soma **19/14** ([app/ponto/management/commands/e6_oraculo.py:353](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/ponto/management/commands/e6_oraculo.py:353), :358–360). `None` no espelho não é zero: tenta `pago_h`, e, faltando ambos, retorna ausência de lavratura (:251–257).

3. **Horas publicadas são horas de diferença absoluta, não trabalho.** O acumulador acrescenta `abs(m_e6 - m_esp)` por divergência; o comando divide por sessenta ([app/ponto/management/commands/e6_oraculo.py:350](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/ponto/management/commands/e6_oraculo.py:350), :368, :423). Os números de horas do diff são compatíveis com essa unidade; não se deve interpretá-los como horas trabalhadas ([app/core/placar_estrutural.py:106](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/core/placar_estrutural.py:106), :118; soma do anexo, origem física [nao verificado]).

4. **Subconjunto precisa ficar claro em E6-batida-impar.** `198+1=199` e `137+2=139` repartem apenas os **ímpares divergentes**, não todos os 347/262 comparados; o código só incrementa `impar_no_conjunto_do_dia`/`impar_SO_POR_CORTE_do_oraculo` depois de rejeitar BATE ([app/ponto/management/commands/e6_oraculo.py:353](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/ponto/management/commands/e6_oraculo.py:353), :370–382). O mesmo vale para **31/25 FORA**: pertencem aos 199/139 divergentes (:383–387). Os valores de [app/core/espelho_verdade.py:177](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/core/espelho_verdade.py:177) estão corretos, mas o texto poderia explicitar “dos que divergem”.

5. **“Nenhum fica fora em silencio” exige delimitar o universo comparável.** O comando elimina dias sem linha, com isenção/ausência, sem lavratura, zero dos dois lados ou diferença explicada pelo intervalo declarado **antes** de classificar a paridade ([app/ponto/management/commands/e6_oraculo.py:324](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/ponto/management/commands/e6_oraculo.py:324), :327–349). A medida mostra que `dia_batida_impar = impar_que_BATE + impar_que_DIVERGE` entre comparados; não informa quantos excluídos também eram ímpares [nao verificado]. A frase universal de [app/core/espelho_verdade.py:177](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/core/espelho_verdade.py:177) não prova ausência de todos os tipos de exclusão.

6. **Causa operacional não demonstrada:** “a saída que falta e cobranca, nao cura de motor” permanece em [app/core/espelho_verdade.py:82](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/core/espelho_verdade.py:82). Contagem ímpar e faixa não demonstram essa exclusividade: o próprio comando reconhece artefato do corte como suspeita do oráculo ([app/ponto/management/commands/e6_oraculo.py:372](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/ponto/management/commands/e6_oraculo.py:372), :379–385); a docstring cita casos em que o oráculo corta jornada verdadeira e a tela está certa (:25–31). A medida fornecida inclui **1/2** divergentes ímpares só pelo corte [nao verificado quanto arquivos físicos]. Portanto o dado não identifica “só cobrança” como causa/cura.

7. **Texto velho continua em R1:** a nota ainda diz “a medicao da competencia aberta e de 09/10” e que a cadência “foi CUMPRIDA”, embora o número/prova passem a 06/11 ([app/core/placar_estrutural.py:119](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/core/placar_estrutural.py:119), em contraste com :118/:120). É história possivelmente intencional, mas está no presente e cita a medição antiga. Não localizei os antigos 95,8%, 333, 92/449,2/39 nos seis itens novos; o `194` em placar :150 pertence a R3, outra medida.

8. **Destino declarado como regra versus medido:** “CADASTRO ... BATIDA ... vao a lista do admin” em [app/core/espelho_verdade.py:173](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/core/espelho_verdade.py:173) tem respaldo como política em L-099 ([app/docs/LEIS.md:122](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/docs/LEIS.md:122)); o anexo não demonstra que todos os 32/113 e 33/78 tenham efetivamente entrada ou chamado [nao verificado]. O comando imprime o rótulo “lista do admin”, mas não realiza escrita nela ([app/ponto/management/commands/e6_oraculo.py:418](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/ponto/management/commands/e6_oraculo.py:418), :419–423).

9. **“0 erro” não foi reafirmado no novo E6-sistema-bate.** O comando tem contador específico de exceções de `espelho_do_colab`, imprime “erros no espelho”, mas o anexo não apresenta essa linha ([app/ponto/management/commands/e6_oraculo.py:303](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/ponto/management/commands/e6_oraculo.py:303), :305–307, :396). `rc_sombra=0`/ausência de `INCOMPLETO` no anexo não prova, isoladamente, zero exceções capturadas [nao verificado]. A nova prova corretamente suprimiu “0 erro” ([app/core/espelho_verdade.py:169](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/core/espelho_verdade.py:169)).

10. **União somada depende das partes serem disjuntas:** a medida permite reproduzir 144/105 pela soma dos seis `PARCIAL_TOTAL.colabs`; não permite conferir identidades ou ausência de sobreposição entre partes [nao verificado]. O comando normal mantém conjuntos por dono e calcula união real do seu próprio `div` ([app/ponto/management/commands/e6_oraculo.py:369](/home/ronald/codex_ws/36c90132efac4acdd84dbf3cecb325ccdfe76c41/app/ponto/management/commands/e6_oraculo.py:369), :429). A implementação do particionamento não consta deste comando (:264–270 não têm `--parte`/`--partes`).

## 06/11 14:07 · sessao bc8feadb57e1 · - · - · origem=Ronald (tmux)

Achados procedentes:

1. **O selo novo valida uma família de nomes, não uma única medição.** `_cita_a_medicao` usa `any(fnmatch(...))`; provas com `r1_dono_...20261106.txt` e `r1_dono_...20261024.txt` passam simultaneamente, assim como a prova literal contendo o próprio glob. Além disso, `_medido_em` escolhe a maior data entre os arquivos existentes, sem conferir qual arquivo a prova cita. Logo, o selo pode ficar verde com número/prova antigos e data de outro arquivo. Procedência: execução em memória das funções reais, sem gravação; `app/core/tests/test_r1_termometro_uma_medicao.py:29`, `:45`, `:50`, `:57`; `bin/gerar_estado.py:51`, `:56`.

2. **A publicação dos seis itens elimina os números e a referência completa.** O publicador corta cada prova em 150 caracteres; todos os seis textos novos têm exatamente o mesmo prefixo de 150 caracteres, terminando em `578 na 11 = soma das partes,`. A tabela pública não mostra BATE, divergentes, ímpares, zeros ou caminho da medição nesses itens. Procedência: recorte em memória dos textos reais; `bin/gerar_estado.py:164`; `app/core/espelho_verdade.py:49`, `:82`, `:169`, `:173`, `:177`, `:179`.

3. **Existe outro publicador de números E6 fora da costura nova.** O ESTADO continua carregando `logs/e6_placar.json` e publicando `pct_bate`, `dias_comparados`, `colabs_divergentes` e `quando`, independentemente da fonte R1. O selo novo só seleciona provas começando com `TERMOMETRO R1`. Se esse JSON contém números velhos, eles continuam aparecendo; o conteúdo atual é [nao verificado]. Procedência: `bin/gerar_estado.py:145`, `:168`; `app/core/espelho_verdade.py:208`; `app/core/tests/test_r1_termometro_uma_medicao.py:37`.

Censo dos campos:

| Campo | Escritor canônico | Leitores reais |
|---|---|---|
| SOMA.numero/prova | `app/core/placar_estrutural.py:106`, `:107` | `app/core/placar_estrutural.py:634`, `:637`; `bin/gerar_estado.py:99`, `:100`; selo novo lê prova em `app/core/tests/test_r1_termometro_uma_medicao.py:50`; host varre strings em `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:65` |
| R1.numero/prova | `app/core/placar_estrutural.py:118`, `:120` | `app/core/placar_estrutural.py:634`, `:637`; número publicado em `bin/gerar_estado.py:116`; prova conferida pelo selo novo em `app/core/tests/test_r1_termometro_uma_medicao.py:45`; host em `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:65` |
| R1.fonte_do_medido | `app/core/placar_estrutural.py:111` | `bin/gerar_estado.py:47`; selo novo em `app/core/tests/test_r1_termometro_uma_medicao.py:45`, `:50`, `:54`; presença exigida em `app/core/tests/test_l114_placar_papel_e_termometro.py:98`; host varre strings em `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:65` |
| ETAPAS[…].prova | `app/core/espelho_verdade.py:49`, `:82`, `:169`, `:173`, `:177`, `:179` | `app/core/espelho_verdade.py:201`, `:203`; `bin/gerar_estado.py:164`; selo novo em `app/core/tests/test_r1_termometro_uma_medicao.py:39`, `:57` |

Leitores acessórios e selos:

- O teste L114 lê o registro R1, a separação de papéis e o vencimento, sem fixar a pasta antiga; substitui `RESULTADOS` apenas por fixture sintética e restaura depois. `app/core/tests/test_l114_placar_papel_e_termometro.py:31`, `:65`, `:96`, `:116`.
- O host de hash só acusa frases declaradas de “fora do ar” na mesma sentença de hash reconhecido e ancestral. Os textos novos não introduzem essa combinação. `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:47`, `:65`, `:72`, `:75`; textos em `app/core/placar_estrutural.py:106`, `:118`, `:120`.
- O selo de data percorre código por regex; a conferência real em memória retornou **nenhuma data cravada no placar**. Datas em caminhos `20261106` e prosa `06/11` não casam esses padrões. `app/core/datas_cravadas.py:26`, `:117`; exigência específica em `app/core/tests/test_l114_placar_papel_e_termometro.py:90`.
- `handoff_sessao.sh` lê o arquivo inteiro, mas extrai somente a frase `contratos_estruturais: N/T verdes` do R6, fora dos campos alterados. O respectivo teste também só lê R6.numero. `bin/handoff_sessao.sh:178`; `app/core/tests/test_selo_r6_frase_do_juiz.py:17`.
- A publicação passa pelo raspador e o selo de nomes/CPF examina o repositório público usando nomes do banco; não valida a pasta dos logs. Resultado real de privacidade: [nao verificado], pois exige banco e publicação. `bin/relato.sh:103`, `:105`; `bin/tests/test_publicacao_sem_nome.sh:39`, `:47`, `:60`, `:72`.
- Número sem prova continua rebaixando o placar; FEITO sem prova continua rebaixando o espelho. As novas provas são não vazias. `app/core/placar_estrutural.py:637`; `app/core/espelho_verdade.py:201`; prova nova em `app/core/placar_estrutural.py:120`, `app/core/espelho_verdade.py:177`.

A mudança para `logs/r1remede/r1_dono_*.txt` é compatível com o leitor de glob/data, desde que o arquivo datado exista no host; existência e retorno em produção são [nao verificado]. `bin/gerar_estado.py:47`, `:51`, `:52`.

## 06/11 14:07 · sessao c673e8cb9b39 · - · - · origem=Ronald (tmux)

Achado procedente: o selo novo não garante “uma medição”; garante apenas que alguma referência na prova casa com a glob. `_cita_a_medicao` usa somente `any(fnmatch.fnmatch(...))`, sem identificar o arquivo/data comum entre leitores (`app/core/tests/test_r1_termometro_uma_medicao.py:29`). A fonte permite múltiplos arquivos (`app/core/placar_estrutural.py:111`), enquanto o publicador escolhe a maior data entre **todos** os arquivos existentes nessa glob, independentemente da prova citada (`bin/gerar_estado.py:51`, `bin/gerar_estado.py:56`).

Contraexemplo reproduzido em memória: com o padrão atual, a prova `logs/r1remede/r1_dono_10_e_11_20261024.txt` passa, assim como uma referência sem data e até `logs/r1remede/r1_dono_*.txt`. Portanto, provas de 24/10 e 06/11 podem coexistir com o selo verde e carimbo escolhido pelo máximo do diretório. Origem do defeito: matcher em `app/core/tests/test_r1_termometro_uma_medicao.py:29`; ligação ausente com o arquivo efetivamente selecionado por `bin/gerar_estado.py:51`.

Outro escape reproduzido: retirando o prefixo `TERMOMETRO R1` de uma das seis provas, esse item deixa de ser fiscalizado, pois o censo usa `startswith` (`app/core/tests/test_r1_termometro_uma_medicao.py:37`). A proteção contra vacuidade exige apenas que **algum** item permaneça no conjunto (`app/core/tests/test_r1_termometro_uma_medicao.py:56`).

Validação limitada, somente leitura: carreguei por `compile/exec` os módulos puros e substituí apenas a base `django.test.SimpleTestCase` por `unittest.TestCase`, executando os quatro métodos do selo. Atual: 4 casos, 0 falhas, 0 erros. Mutante mesma glob/outra data em uma prova: 4/0/0. Mutante removendo o prefixo desse item: 4/0/0. Isso não comprova execução da suite Django/canônica [nao verificado]. [nao verificado]

O selo tem mordida real para a mudança **de pasta**: o caso adversarial rejeita `logs/sombra/...` e aceita `logs/x/r1_dono_...`, incluindo pontuação final (`app/core/tests/test_r1_termometro_uma_medicao.py:60`). Não copia valores medidos atuais nem data/caminho atual nas asserções; consulta as estruturas do código e usa um caminho fictício no adversarial (`app/core/tests/test_r1_termometro_uma_medicao.py:43`, `app/core/tests/test_r1_termometro_uma_medicao.py:62`). O literal “39 dia-colab” não é conferido pelo matcher, que só extrai caminhos (`app/core/tests/test_r1_termometro_uma_medicao.py:22`, `app/core/tests/test_r1_termometro_uma_medicao.py:64`).

Não há leitura de relógio no selo novo: suas importações e helpers usam `fnmatch`, regex e os dois módulos de dados (`app/core/tests/test_r1_termometro_uma_medicao.py:14`). O relógio continua na borda publicadora (`bin/gerar_estado.py:79`); o cálculo do vencimento recebe `hoje` e `medido_em` por parâmetro (`app/core/placar_estrutural.py:617`, `app/core/placar_estrutural.py:647`).

O RED histórico alegado no commit, com `FAILED failures=1` e `logs/r1pub/red.log`, não pode ser confirmado com o material disponível [nao verificado]. A ausência de sustentação desse histórico não elimina a mordida estática contra a pasta antiga, mas também não comprova a execução alegada (`app/core/tests/test_r1_termometro_uma_medicao.py:60`).

Pequena inconsistência documental introduzida: a docstring diz “cinco itens” antigos (`app/core/tests/test_r1_termometro_uma_medicao.py:9`), enquanto o diff fornecido troca seis provas — E1-sem-previsao, E3-turno-aberto e quatro itens E6 (`app/core/espelho_verdade.py:49`, `app/core/espelho_verdade.py:82`, `app/core/espelho_verdade.py:169`, `app/core/espelho_verdade.py:173`, `app/core/espelho_verdade.py:177`, `app/core/espelho_verdade.py:179`).

Das classes listadas, a ocorrência comprovável é **selo com mordida insuficiente para a propriedade declarada** (`app/core/tests/test_r1_termometro_uma_medicao.py:29`). O diff de produto altera dados declarados, sem introduzir nova função de julgamento/escrita ou travas (`app/core/placar_estrutural.py:106`, `app/core/placar_estrutural.py:118`, `app/core/espelho_verdade.py:49`). Pouso, ordem efetivamente executada, suite verde e leitura final não estão demonstrados [nao verificado].

## 06/11 13:57 · sessao 0b5c7db967e3 · V3-IMPACTO-COM-DONO · leitura · origem=Code

1. **Os 3191 registros não identificam nenhum `raise LacunaDeMedicao()` literal.** No anexo, `RegistroErros.dados` escreve `sonda` quando não recebe traceback; um `raise` capturado teria origem com linha. Condicionado ao anexo ser o código executado, os registros vieram destas chamadas **sem lançamento** de `falha(LacunaDeMedicao())`. A repartição integral entre elas não foi preservada. [anexo `q3_script_p1.py`, `RegistroErros.dados` e `executar`; nao verificado]

| Sítio do anexo | O que significa | Classificação |
|---|---|---|
| Varredura dos contadores `historico["T"]` | Uma ocorrência por **chave não zero**, não por unidade do contador. Mistura falhas diferentes do comando. | O registro genérico é defeito do instrumento; a causa subjacente exige identificar o contador. [nao verificado] |
| `if not c["emitido"][versao]` | Confunde chamada ausente com chamada bem sucedida sem objetos `TIPO_DIA`. | Defeito do instrumento: `lavrar` sempre pode emitir somente AJUSTE. [app/ponto/services/dia_pago.py:346] |
| `if alvo not in c["fechos"]` | O fechamento não entregou objeto para o alvo. | Pode ser fato de escopo do produto: o fechamento aplica seu próprio universo e restrições. A causa destes casos não está demonstrada. [app/ponto/services/fechamento.py:545] [app/ponto/services/fechamento.py:555] [nao verificado] |
| `if v2 is None or v3 is None` | Uma data pertence à união dos dias emitidos, mas falta em uma versão. | Depois de uma recusa conhecida, é **lacuna secundária fabricada pelo instrumento**; depois de duas lavras bem sucedidas, é diferença real dos conjuntos emitidos, a registrar separadamente. [app/ponto/services/fechamento.py:1281] [nao verificado quanto aos casos individuais] |

O defeito concreto está na captura: o anexo guarda `rubricas`, `cegos` e `colab` **depois** de `REAL_LAVRAR`. A recusa impede essa captura; o fechamento captura a exceção e continua. O instrumento então acusa oráculo vazio e falta de par para os dias do motor. A cura é capturar a entrada **antes**, registrar tentativa e sucesso separadamente e excluir uma vez pela recusa. [anexo `q3_script_p1.py`, `emissor_capturado`; nao verificado] [app/ponto/services/fechamento.py:1272] [app/ponto/services/fechamento.py:1281]

Em P1, `SEM_PAR` permite atribuir **237** lacunas de outubro e **329** de novembro ao teste diário; restam **47** e **24**, respectivamente, sem repartição recuperável entre os demais sítios sintéticos. Nada permite estender essa decomposição às outras partes. [anexos `q3_s_p1.r` e `agrega.r`; nao verificado] [nao verificado]

Portanto, **um DIA vira lacuna nesse teste pela ausência de uma das duas linhas emitidas**, independentemente de valor, batida ou divergência. A data exemplificada não prova falta de decisão da autoridade. Os universos também diferem: a porta visita dias do núcleo mais bloqueados publicados pelo motor; a lavra cria dias adicionais por furos e grade. [anexo `q3_script_p1.py`, laço de `chaves`; nao verificado] [app/ponto/calculador/alimentacao.py:467] [app/ponto/calculador/alimentacao.py:469] [app/ponto/services/dia_pago.py:280] [app/ponto/services/dia_pago.py:286] [nao verificado]

2. **Os 740 não são demonstradamente falhas de `porta_ok`.** O anexo usa a mesma classe na conferência mensal e na conferência porta/emissor; ambas são registradas como `sonda`. O `raise` de `n != len(capturadas)` teria traceback, portanto não corresponde à origem publicada, condicionado ao anexo executado. Além disso, `False` em `SOMA V3_PORTA_EMISSOR` pode resultar apenas de `and cobertura`. [anexo `q3_script_p1.py`, `acumular`/`publicar`; nao verificado] [nao verificado]

Quem escreve os lados:

- **`porta`**: `rubricas_por_dia` escreve cada dia chamando `regras.do_dia`; o fechamento entrega o mapa a `lavrar`. [app/ponto/calculador/alimentacao.py:514] [app/ponto/services/fechamento.py:1255] [app/ponto/services/fechamento.py:1272]
- **`emitido_decidido`**: `lavrar` lê os valores da porta, atribui ao campo do dia e constrói os objetos `DiaPago`. Nesse caminho, o chamador não passa `do_motor`; a atribuição é direta. [app/ponto/services/dia_pago.py:322] [app/ponto/services/dia_pago.py:328] [app/ponto/services/dia_pago.py:343] [app/ponto/services/fechamento.py:1261]

Assim, uma diferença **comprovada na mesma célula decidida** indicaria incoerência do produto, provavelmente **ESTRUTURA**, no campo identificado pela comparação. O dado atual não identifica essas células nem demonstra que essa igualdade falhou. [CLAUDE.md:176] [nao verificado]

Há também defeito instrumental em `fm_ok = bool(e2) ...`: uma lavra somente de AJUSTE, com mensal zero, produz falsamente uma conservação quebrada **por rubrica**. A ocorrência concreta desses casos não pode ser contada pelo dado. [anexo `q3_script_p1.py`, `acumular`; nao verificado] [app/ponto/services/dia_pago.py:346]

P1 mostra uma diferença mensal específica em novembro/trabalhadas: **5478,32 versus 5478,33**. O campo é `horas_trabalhadas`; o mensal tem arredondamento inicial e pode arredondar novamente ao adicionar folga sem escala. Isso oferece uma hipótese de **ESTRUTURA**, mas não prova a causa daquele centésimo. [anexo `q3_s_p1.r`; nao verificado] [app/ponto/services/fechamento.py:893] [app/ponto/services/fechamento.py:968]

3. O caminho é `diff_calculador` → fechamento → `lavrar(..., rubricas=_rub_o, versao="oraculo")`. O comando instala normalmente `_nada`; o captador do anexo reinstala a chamada real durante o fechamento. [app/ponto/management/commands/diff_calculador.py:137] [app/ponto/management/commands/diff_calculador.py:140] [app/ponto/services/fechamento.py:1261] [app/ponto/services/fechamento.py:1272] [anexo `q3_script_p1.py`; nao verificado]

**A recusa é esperada para entrada sem proveniência; “lei do ancoramento pendente” não descreve a lei vigente desta cópia.** A lei já manda ancorar por período, pela mesma `he_noturna`, e isso está implementado. [app/ponto/services/dia_pago.py:311] [app/ponto/services/dia_pago.py:315] [app/ponto/calculador/regras.py:414] [app/ponto/calculador/regras.py:825] [app/ponto/calculador/regras.py:832]

O conjunto de donos depende das declarações encontradas em `rubricas.values()`, incluindo `nao_decide`. Um mapa vazio não declara os onze campos do calculador; a própria alimentação documenta esse mecanismo. Por isso a recusa depende dos insumos de cada colaborador, não ocorre indistintamente. **Não é possível afirmar que todas as 182 recusas sejam mapa vazio**, pois os campos recusados não foram publicados. As frequências fornecidas variam de 11 a 21 por parte/competência. [app/ponto/services/dia_pago.py:61] [app/ponto/services/dia_pago.py:64] [app/ponto/calculador/alimentacao.py:459] [anexo `agrega.r`; nao verificado] [nao verificado]

O guard está correto ao recusar. Se a integração entrega um mês vazio sem declarar sua proveniência, o defeito é do protocolo de alimentação; não se resolve preenchendo zero nem suspendendo o guard. Qual entrada explica cada recusa permanece [nao verificado]. [app/ponto/services/dia_pago.py:308]

4. O mapa escritor/leitor é este; a tabela de juízes aponta cálculo para motor/CCT e janela para `janelas`, sem substituir o código vivo. [app/docs/JUIZES.md:14] [app/docs/JUIZES.md:29] [app/docs/JUIZES.md:35]

| Valor somado pelo anexo | Quem escreve | Quem lê |
|---|---|---|
| `v2_h` publicado | Lavra `motor`: períodos, folgas, órfãos e demais mapas; objetos em `:343`. [app/ponto/services/dia_pago.py:173] [app/ponto/services/dia_pago.py:234] [app/ponto/services/dia_pago.py:267] [app/ponto/services/dia_pago.py:343] | Captador → `acumular` → `comparados` → `publicar`. É somente o subconjunto decidido da comparação. [anexo `q3_script_p1.py`; nao verificado] | [nao verificado]
| `v3_h` publicado | A mesma lavra, substituindo campos decididos pela porta. [app/ponto/services/dia_pago.py:328] | O mesmo percurso do anexo, versão `oraculo`. [nao verificado] | [nao verificado]
| `porta` | `regras.do_dia`, pela alimentação chamada pelo fechamento. [app/ponto/calculador/alimentacao.py:514] [app/ponto/services/fechamento.py:1255] | `lavrar`, o DIFF e o captador do anexo. [app/ponto/services/dia_pago.py:322] [app/ponto/management/commands/diff_calculador.py:469] [nao verificado quanto ao anexo] |
| `emitido_decidido` | Objetos produzidos pelo único emissor. [app/ponto/services/dia_pago.py:343] | O anexo seleciona os campos presentes e não cegos. [nao verificado] |
| `INSTRUMENTO_HISTORICO` | `m_dia` de períodos e `c_dia` da porta; `S` soma essas leituras. [app/ponto/management/commands/diff_calculador.py:253] [app/ponto/management/commands/diff_calculador.py:501] [app/ponto/management/commands/diff_calculador.py:536] | Tracer do anexo. **Não é a soma dos objetos emitidos.** [nao verificado] |

Leitores produtivos concretos: calendário lê linhas e `rubricas_do_dia`; export lê dias da versão `motor`; a conferência da competência agrega os campos lavrados. Isso não constitui censo global dos leitores de `oraculo`. [app/colaboradores/services/calendario.py:290] [app/colaboradores/services/calendario.py:656] [app/folha/porta_export.py:588] [app/ponto/services/dia_pago.py:560] [nao verificado quanto ao censo global]

Segue a proposta completa, **não executada**. Ela admite o colaborador somente depois das conferências, aceita lavra bem sucedida sem DIA e não usa o registro global de exceções para invalidar os medidos. `SOMA COBERTURA` passa a conferir a contabilização **medidos + excluídos**; `COBERTURA_INTEGRAL` informa separadamente se houve exclusões. Os limites de tempo/RSS são cooperativos; cabimento efetivo em 2 GB/15 minutos permanece [nao verificado].

```python
# PROPOSTA NAO EXECUTADA. Todo comportamento/performance desta reescrita:
# [nao verificado].
#
# Medida de IMPACTO, nunca validacao: CLAUDE.md:50,58,415.
# LEI-AKITA: origem=captura antes de lavrar;
# testemunha=lavrar real + retorno real do fechamento;
# RED=[nao verificado]; quem-mais-le=JUIZES.md:29,35 e leitores citados;
# juizes novos=0; padrao=n/a (instrumento).
#
# Nenhum arquivo e aberto para escrita por este script.
# Executar somente pelo envelope de leitura ja autorizado/configurado:
# bin/sonda_leitura.sh:237-240.
#
# PRODUTO.n_dias conta datas observadas associadas a classe, sem multiplicar
# por rubrica. Na recusa: dias preparados pela lavra + chaves da porta.
# Na conservacao mensal: dias que contribuiram ao total daquele campo.
# Ausencia de data observavel aparece adicionalmente como DIAS_NAO_LOCALIZADOS.
# Classes de PRODUTO podem sobrepor: nao somar suas contagens entre classes.

import collections
import datetime as dt
import gc
import linecache
import logging
import math
import resource
import sys
import time
from contextlib import redirect_stdout, redirect_stderr
from decimal import Decimal
from unittest.mock import Mock, patch

from django.conf import settings
from django.db import connection, reset_queries, transaction
from django.utils import timezone

from ponto.calculador import regras
from ponto.catalogo import ausencias as CAT
from ponto.management.commands import diff_calculador as DC
from ponto.management.commands.e6_oraculo import InsumosDoDono
from ponto.models import DiaPago
from ponto.services import dia_pago as DP
from ponto.services import fechamento as F
PARTE = 1
PARTES = 6

sys.dont_write_bytecode = True

COMPETENCIAS = ((2026, 10), (2026, 11))
RUBRICAS = tuple(r for r in regras.RUBRICAS if r not in regras.NAO_DECIDE)
DONOS = {"BATIDA", "CADASTRO", "ESTRUTURA"}
LIMIAR_H = 0.02  # diff_calculador.py:538
INICIO = time.monotonic()
PRAZO = INICIO + 14 * 60       # reserva para diagnostico/publicacao
RSS_LIMITE_KIB = 1900 * 1024  # Linux; controle cooperativo
HOJE, AGORA = timezone.localdate(), timezone.now()

REAL_RECALC = F.recalcular_fechamento_mes
REAL_LAVRAR = DP.lavrar
COD_COMANDO = DC.Command.handle.__code__
COD_LAVRAR = REAL_LAVRAR.__code__
RAIZ_APP = DC.__file__.split("/ponto/management/commands/")[0] + "/"

SOMAS = []
TOTAL_EXCLUIDOS = 0
TOTAL_INSTRUMENTO = 0


class FalhaInstrumento(Exception):
    pass


class Limite(BaseException):
    def __init__(self, causa):
        self.causa = causa


class Descartar:
    def write(self, texto):
        return len(texto)

    def flush(self):
        pass

    def isatty(self):
        return False


DESCARTAR = Descartar()


def rotulo(cid):
    # Mesma convencao do anexo. Nunca imprimir cid isolado, modelos,
    # nomes, CPF, argumentos SQL ou str(exc).
    if not isinstance(cid, int) or isinstance(cid, bool):
        raise FalhaInstrumento("IDENTIFICADOR_INVALIDO")
    return "col%d" % cid


def limites():
    # Nao interrompe, por si so, consulta bloqueada em codigo nativo.
    if time.monotonic() >= PRAZO:
        raise Limite("PRAZO")
    if resource.getrusage(resource.RUSAGE_SELF).ru_maxrss >= RSS_LIMITE_KIB:
        raise Limite("MEMORIA")


def numero(valor):
    try:
        n = float(valor)
    except (TypeError, ValueError, OverflowError):
        raise FalhaInstrumento("NUMERO_INVALIDO") from None
    if not math.isfinite(n):
        raise FalhaInstrumento("NUMERO_NAO_FINITO")
    return n


def soma(xs):
    return math.fsum(xs)


def igual(a, b):
    return math.isclose(a, b, rel_tol=1e-12, abs_tol=1e-8)


def centavos(valor):
    # Mantem o criterio do anexo, por colaborador.
    return Decimal(str(round(numero(valor), 2)))


def datas(xs):
    return {x for x in xs if type(x) is dt.date}


def tipo_seguro(exc):
    nome = type(exc).__name__
    if nome.isascii() and nome.isidentifier() and len(nome) <= 80:
        return nome
    return "Excecao"


def ultimo_tb(exc):
    tb = exc.__traceback__
    while tb is not None and tb.tb_next is not None:
        tb = tb.tb_next
    return tb


def origem(exc):
    tb = ultimo_tb(exc)
    if tb is None:
        return "sem_traceback"
    nome = tb.tb_frame.f_code.co_filename
    if nome.startswith(RAIZ_APP):
        return "app/%s:%d" % (nome[len(RAIZ_APP):], tb.tb_lineno)
    return "fora_do_app:%d" % tb.tb_lineno


def novo_resultado():
    return {
        "ids": set(), "cortes": 0, "completo": False, "historico": None,
        "janela": None, "colab": None, "fecho": None,
        "tentativas": {"motor": 0, "oraculo": 0},
        "sucessos": {"motor": 0, "oraculo": 0},
        "emitido": {"motor": {}, "oraculo": {}},
        "porta": {}, "cegos": {},
        "faltas_porta_dc": set(),
        "produto": {}, "instrumento": {},
        "nao_localizados": set(), "causas": set(),
        "excecoes": [], "recusas": [], "parada": None,
    }


def registrar(c, natureza, classe, dias=(), bloqueia=False, localizado=True):
    conjunto = datas(dias)
    c[natureza].setdefault(classe, set()).update(conjunto)
    if not localizado:
        c["nao_localizados"].add((natureza, classe))
    if bloqueia:
        c["causas"].add(natureza.upper() + ":" + classe)


def instrumental(c, classe, dias=()):
    registrar(c, "instrumento", classe, dias, bloqueia=True)


def produto(c, classe, dias=(), bloqueia=False, localizado=True):
    registrar(c, "produto", classe, dias, bloqueia, localizado)


def falhou(c, exc, operacao, dias=()):
    if isinstance(exc, Limite):
        instrumental(c, exc.causa, dias)
        c["parada"] = exc.causa
    elif isinstance(exc, MemoryError):
        instrumental(c, "MEMORIA", dias)
        c["parada"] = "MEMORIA"
    elif isinstance(exc, FalhaInstrumento):
        instrumental(c, exc.args[0], dias)
    else:
        classe = "FALHA_%s_%s" % (operacao, tipo_seguro(exc))
        produto(c, classe, dias, bloqueia=True,
                localizado=bool(datas(dias)))
        c["excecoes"].append((operacao, tipo_seguro(exc), origem(exc)))


def limpar():
    # Catalogo: cache de modulo; demais referencias sao de um colaborador.
    # app/ponto/catalogo/ausencias.py:257,261.
    CAT.invalidar_cache()
    reset_queries()
    gc.collect()


def executar(ano, mes, alvo=None):
    c = novo_resultado()

    def observar(frame, evento, arg):
        # A sonda precisa de linhas somente no comando.
        # Excecao interna tratada nao e, por si, falha da medicao.
        if frame.f_code is not COD_COMANDO:
            return None
        frame.f_trace_lines = True
        loc = frame.f_locals

        if evento == "line":
            limites()
            if frame.f_lineno == 116:
                # Antes do fechamento e do laco: diff_calculador.py:116.
                lista = loc.get("ids")
                if not isinstance(lista, list):
                    raise FalhaInstrumento("IDS_NAO_LISTA")
                if any(not isinstance(x, int) or isinstance(x, bool)
                       for x in lista):
                    raise FalhaInstrumento("IDS_INVALIDOS")
                if len(lista) != len(set(lista)):
                    raise FalhaInstrumento("IDS_DUPLICADOS")

                c["cortes"] += 1
                if alvo is None:
                    if c["ids"].intersection(lista):
                        raise FalhaInstrumento("IDS_ENTRE_EMPRESAS")
                    c["ids"].update(lista)
                    lista[:] = []
                else:
                    lista[:] = [
                        x for x in lista
                        if x == alvo and x % PARTES == PARTE - 1
                    ]
                    if len(lista) > 1 or c["ids"].intersection(lista):
                        raise FalhaInstrumento("ALVO_DUPLICADO")
                    c["ids"].update(lista)

            if frame.f_lineno == 471:
                # Dia do nucleo sem resposta da porta, nao "dia sem batida".
                # diff_calculador.py:469-472.
                c["faltas_porta_dc"].update(datas([loc.get("k")]))

            if frame.f_lineno == 550:
                c["completo"] = True

        elif evento == "return":
            S, T = loc.get("S"), loc.get("T")
            if isinstance(S, dict) and isinstance(T, collections.Counter):
                c["historico"] = {
                    "S": {r: tuple(v) for r, v in S.items()},
                    "T": dict(T),
                }
        return observar

    def fechamento_capturado(m, a, **kw):
        limites()
        chamador = sys._getframe(1)
        if chamador.f_code is not COD_COMANDO:
            raise FalhaInstrumento("CHAMADOR_FECHAMENTO")
        loc = chamador.f_locals
        ids = set(loc.get("ids", ()))
        if not ids:
            return {}
        if alvo is None or ids != {alvo}:
            raise FalhaInstrumento("ESCOPO_FECHAMENTO")
        if (m, a) != (mes, ano):
            raise FalhaInstrumento("COMPETENCIA_FECHAMENTO")
        if not kw.get("somente_leitura") or not kw.get("lavrar_dias_pagos"):
            raise FalhaInstrumento("MODO_FECHAMENTO")

        ini, fim = loc.get("ini"), loc.get("fim")
        if type(ini) is not dt.date or type(fim) is not dt.date or ini > fim:
            raise FalhaInstrumento("JANELA_INVALIDA")
        c["janela"] = (ini, fim)

        def emissor_capturado(colab, mm, aa, **dados):
            limites()
            versao = dados.get("versao", "motor")
            if (colab.pk != alvo or (mm, aa) != (mes, ano)
                    or versao not in ("motor", "oraculo")):
                raise FalhaInstrumento("ESCOPO_LAVRAR")
            if c["tentativas"][versao]:
                raise FalhaInstrumento("LAVRAR_REPETIDA_" + versao.upper())

            c["tentativas"][versao] += 1
            c["colab"] = colab

            # CURA DA CAPTURA: guardar a entrada ANTES da chamada real.
            # Fonte: fechamento.py:1255,1272.
            if versao == "oraculo":
                rubricas = dados.get("rubricas")
                if not isinstance(rubricas, dict):
                    raise FalhaInstrumento("RUBRICAS_NAO_CAPTURADAS")
                if dados.get("do_motor"):
                    raise FalhaInstrumento("PROVENIENCIA_DO_MOTOR_MUDOU")
                for dia, rub in rubricas.items():
                    if type(dia) is not dt.date:
                        raise FalhaInstrumento("DATA_PORTA_INVAL

[truncado em 20000 de 45545 caracteres]

## 06/11 13:51 · sessao cd0d5a68c7a6 · - · - · origem=Ronald (tmux)

A dedução está correta para o script anexo: `sonda` significa `tb=None`. Um `raise` preservaria traceback e produziria um sítio com linha; portanto os 3191 registros `sonda LacunaDeMedicao` não demonstram disparo de nenhum `raise LacunaDeMedicao()` literal. Eles vêm de `falha(LacunaDeMedicao())` sem lançamento: contadores `T`, emissão DIA vazia, fecho ausente ou par diário ausente. [nao verificado]

Em P1, `SEM_PAR` permite atribuir 237 lacunas de outubro e 329 de novembro ao teste diário; restam 47 e 24 lacunas sintéticas cuja divisão entre os outros testes não é recuperável. [nao verificado] [nao verificado]

Analogamente, os `sonda ConservacaoNaoFecha` vêm das chamadas sintéticas de conferência mensal ou porta/emissor; o `raise` de `n != len(capturadas)` teria sítio com linha. [nao verificado]

## 06/11 13:49 · sessao cd0d5a68c7a6 · - · - · origem=Ronald (tmux)

A falha importante seria excluir por qualquer exceção propagada capturada pelo tracer. A alimentação trata exceções e continua com abstenção/default declarado em `app/ponto/calculador/alimentacao.py:389` e `app/ponto/calculador/alimentacao.py:500`; o tracer deve manter diagnóstico separado. Exclusão exige falha da operação ou violação concreta do contrato.

“Tentativa sem retorno” só deve receber classe PRODUTO quando a falha veio da autoridade real; exceção levantada pelo captador é INSTRUMENTO. [nao verificado]

Também preserve `SOMA` independente do registro global de erros e da totalidade do universo: valide antes de acumular e julgue apenas os colaboradores admitidos. Caso contrário, a exclusão declarada recria o `False` geral. [nao verificado]

## 06/11 13:47 · sessao a01dce60e12a · - · - · origem=Ronald (tmux)

Mapa para a pergunta 4:

| Valor | Quem escreve a fonte | Quem lê |
|---|---|---|
| `v2_h` da `RUBRICA` do anexo | `DP.lavrar`: períodos normais em `app/ponto/services/dia_pago.py:173`; campos em `:178`–`:186`; FT em `:234`, `:244`, `:257`; atraso em `:190`; órfãos em `:267`. Constrói `DiaPago` em `:343` e persiste em `:358`. Chamada real em `app/ponto/services/fechamento.py:1185`, versão motor em `:1211`. | No anexo: `emissor_capturado` → `c["emitido"]["motor"]` → `acumular` → `comparados` → `publicar`; numeração do anexo [nao verificado]. |
| `v3_h` da `RUBRICA` do anexo | A mesma `DP.lavrar`, substituindo valores decididos da porta em `app/ponto/services/dia_pago.py:320`–`:328`, construindo em `:343` e persistindo em `:358`; chamada em `app/ponto/services/fechamento.py:1261`, `rubricas` em `:1272`, versão em `:1278`. | No anexo: `emissor_capturado` → `c["emitido"]["oraculo"]` → `acumular` → `comparados` → `publicar`; numeração [nao verificado]. |
| `porta` | `rubricas_por_dia` é a fronteira, `app/ponto/calculador/alimentacao.py:353`; chamada do fechamento em `app/ponto/services/fechamento.py:1255`; o diff tem sua própria chamada real dessa mesma função em `app/ponto/management/commands/diff_calculador.py:417`. | DP lê os `Rubricas.valores` em `app/ponto/services/dia_pago.py:322`; diff lê em `app/ponto/management/commands/diff_calculador.py:469` e guarda em `:501`; anexo lê kwargs de `lavrar`, atualmente somente após retorno bem-sucedido [nao verificado]. |
| `emitido_decidido` | Objetos `DiaPago` construídos pelo único emissor `app/ponto/services/dia_pago.py:343`; horas de origem do motor e substituições descritas acima. | Anexo seleciona apenas células com `r in valores` e `r not in cegos`, somando `b[r]`; numeração [nao verificado]. |
| `INSTRUMENTO_HISTORICO` | Diferente de `v2_h` emitido: `m_dia` montado de períodos em `app/ponto/management/commands/diff_calculador.py:246`–`:268`; `c_dia` vem da porta em `:501`; `S` acumula em `:535`–`:537`. | Tracer lê `S` no retorno do comando; anexo [nao verificado]. |

Autoridades de referência: `app/docs/JUIZES.md:29` nomeia motor/fábrica CCT; `:35` nomeia janela de folha; `:20` nomeia geometria. O resultado composto nasce em `app/ponto/services/fechamento.py:182`, cada fatia chama o motor em `:277`, e a composição ocorre em `:285`–`:291`. O `setdefault` do tracer nativo do comando em `app/ponto/management/commands/diff_calculador.py:131`–`:135` continua capturando a primeira instância composta.

Leitores produtivos concretos:

- Calendário pede as linhas em `app/colaboradores/services/calendario.py:290` e lê a rubrica em `:655`–`:656`; a porta tem versão padrão motor em `app/ponto/services/dia_pago.py:831`.
- `rubricas_do_dia` lê campos em `app/ponto/services/dia_pago.py:912`–`:924`; não soma variantes noturna/feriado aos totais, conforme `:902`–`:904`.
- Porta export consulta dias motor em `app/folha/porta_export.py:588`–`:589`.
- `soma_da_competencia` agrega campos de `DiaPago` em `app/ponto/services/dia_pago.py:557`–`:561`, com arredondamento final.
- O comentário “nenhum leitor lê oráculo” de `app/ponto/services/fechamento.py:1231` é histórico; não entreguei censo global de todos os leitores [nao verificado].

Defeito de captura confirmado na lógica do anexo: rubricas/cegos são guardados **depois** de `REAL_LAVRAR`. A recusa impede a captura e depois aparece como emissão ausente/SEM_PAR, produzindo falhas secundárias [nao verificado]. A correção deve capturar kwargs antes da chamada real e registrar a recusa à parte; não inventar emissão. O produto captura a exceção e continua em `app/ponto/services/fechamento.py:1281`.

## 06/11 13:46 · sessao cd0d5a68c7a6 · - · - · origem=Ronald (tmux)

- **A saída não permite repartir exatamente as 3191 lacunas por sítio.** O registro sintético usa `tb=None` e agrupa em `sonda`; não conserva a causa nem a linha chamadora. A associação integral das contagens aos sítios é [nao verificado]. P1 informa 237 dias `SEM_PAR` em outubro e 329 em novembro; o loop do anexo registra uma lacuna por chave com um emissor ausente, mas outras lacunas vêm dos testes de vazio, `T`, fechos e donos. A decomposição restante é [nao verificado]. [nao verificado]

- **Defeito concreto da captura:** o anexo registra `colabs`, `rubricas` e `nao_decide` apenas depois de `REAL_LAVRAR` retornar. A recusa em `dia_pago.py:315` impede essa captura, embora o chamador tenha fornecido `_rub_o` em `fechamento.py:1272`. O próprio fechamento captura a recusa em `fechamento.py:1281` e continua. Assim, uma recusa declarada gera artificialmente “oráculo vazio” e lacunas para todos os dias do motor no script anexo. Capture argumentos antes da chamada, registre tentativa/retorno independentemente e classifique a recusa uma única vez.

- **Outro defeito lógico do instrumento:** `if not c["emitido"][versao]` não distingue chamada ausente de lavratura bem sucedida sem linhas DIA. A função sempre emite AJUSTE (`dia_pago.py:346`, `dia_pago.py:355`), enquanto os dias dependem de períodos, furos e mapas (`dia_pago.py:173`, `dia_pago.py:267`, `dia_pago.py:272`, `dia_pago.py:280`, `dia_pago.py:286`). Ocorrência desse segundo caso nos anexos: [nao verificado].

- **O que torna um dia lacuna no anexo:** pertencer à união de chaves DIA motor/oráculo e faltar em um dos mapas; não depende de valor diferente, presença de batida ou rubrica divergente. O exemplo com data é produzido por esse teste ou pelos testes de janela/dono; a saída agregada não guarda o sítio para distinguir exatamente [nao verificado]. A captura proposta deve registrar classe explícita por dia. [nao verificado]

- **Proveniência real do caminho:** o comando chama o fechamento em `diff_calculador.py:140`, após instalar `_nada` em `diff_calculador.py:137`; o wrapper do anexo substitui temporariamente essa função por seu captador durante a chamada real. O fechamento chama `lavrar` motor em `fechamento.py:1185` e oráculo em `fechamento.py:1261`; este recebe `rubricas=_rub_o` em `fechamento.py:1272`. A alimentação escreve `_rub_o` pela chamada `fechamento.py:1255`; a função retorna o mapa em `calculador/alimentacao.py:533` e escreve cada entrada com `regras.do_dia` em `calculador/alimentacao.py:514`.

- **Os universos de dias são distintos:** a porta do calculador visita os dias do núcleo mais os dias bloqueados publicados pelo motor (`calculador/alimentacao.py:467`, `calculador/alimentacao.py:469`, `calculador/alimentacao.py:471`); a lavratura também cria dias por furos e grade (`dia_pago.py:280`, `dia_pago.py:286`). Consequentemente, ausência na porta é abstenção ou diferença de universo a investigar, não automaticamente defeito de captura.

- **Histórico do DIFF não é a soma emitida:** o comando escreve `m_dia` diretamente de períodos em `diff_calculador.py:253`, escreve `c_dia` a partir das rubricas em `diff_calculador.py:501`, exclui cegueiras em `diff_calculador.py:532` e soma `S` em `diff_calculador.py:536`. O script soma objetos DIA capturados de `lavrar`; são grandezas/caminhos distintos.

- Para a reescrita: capture a porta **antes** da chamada; conte chamada e retorno explicitamente; preserve recusa esperada como `RECUSA_PORTA_SEM_DONO`; não transforme recusa em centenas de lacunas derivadas; valide cada colaborador antes de acumular ou imprimir `DIA`, `DONO` e `RESIDUO`; publique classes de produto separadamente; mantenha contagem exata dos dias afetados por conjuntos, evitando multiplicar um dia por rubrica. São recomendações de desenho [nao verificado].

_2317 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
