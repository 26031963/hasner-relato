# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 20:34** · publicado em **17/11/2026 20:34** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 20:34 · sessao 088862de7bc0 · CERCA-B2 · verificador · origem=Code

**AVAL-DEVOLVIDO para o item 1:** a direção de usar a leitura da porta está prevista na regra fornecida, mas a função única por pergunta ainda não existe pronta na cópia, e o dry comum não é uma medição comprovadamente sem escrita. Há também um problema no RED proposto: nesta cópia, o “dia do turno” vem do próprio `data_evento` da instância, de modo que o caso descrito exige esclarecer como os dois dias seriam diferentes. ([fio.py:1389](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/services/fio.py:1389), [juizes.py:338](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/juizes.py:338), [saas.py:118](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/config/settings/saas.py:118))

1. **Premissas**

   - **(a) Parcialmente.** O comando calcula a competência a partir de `p.data_evento` e consulta `marcos_da_competencia()['exportada']`; a divisão efetiva está nas linhas 99–103 e 113–119. A janela fixa é a dos **30 dias de carimbo até 28/10/2026 18:32:07**, não uma janela de datas de evento. ([reabrir_orfas_enterradas.py:35](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/reabrir_orfas_enterradas.py:35), [reabrir_orfas_enterradas.py:85](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/reabrir_orfas_enterradas.py:85), [reabrir_orfas_enterradas.py:99](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/reabrir_orfas_enterradas.py:99))
   - **(b) Sim, como chamada que provoca a escrita.** A linha 157 chama `reabrir_fantasma`; ela chama `descarimbar`, cujo `save` grava os campos do carimbo. O mesmo caminho também pode escrever evento, disputa, pai e pauta. ([reabrir_orfas_enterradas.py:153](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/reabrir_orfas_enterradas.py:153), [models.py:3431](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/models.py:3431), [models.py:3324](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/models.py:3324))
   - **(c) Não.** A linha 829 descreve outro comando. A entrada de `reabrir_orfas_enterradas` está em 835–838, no cadastro descritivo de ato de uma vez; a busca na cópia encontrou essa ocorrência, sem entrada agendada para ele na lista `CRONS`. A existência de cron instalado em produção é **[não verificado]**. ([crons.py:281](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/config/crons.py:281), [crons.py:755](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/config/crons.py:755), [crons.py:827](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/config/crons.py:827), [crons.py:835](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/config/crons.py:835))
   - **(d) Não existe pronta a função única por item pressuposta no aval.** `veto_de_competencia(empresa, dias, memo)` é o juiz comum de uma **lista de dias**, não recebe a pergunta; retorna `None` para dia desconhecido e, fora isso, pergunta se algum dia está entregue. A seleção do dia da instância, do dia gravado e a exclusão de conversa estão dentro de `vigiar_escrita_de_perguntas`, que fora da cerca retorna sem julgar. O item `p` fornece os dados para compor essa leitura, mas o dia gravado é buscado novamente no banco; compartilhar o julgamento por item requer extrair essa composição e fazer porta e comando chamarem a mesma função, preservando inclusive conversa e `None`. ([fio.py:1358](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/services/fio.py:1358), [fio.py:1396](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/services/fio.py:1396), [fio.py:1403](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/services/fio.py:1403))
   - **(e) Sim, com condições.** O arquivo padrão de reversão é criado apenas com `--apply` **e** havendo alvo ou pauta; `--reversao` pode substituir o caminho. O resumo final imprime totais separados `reabrir` e `pauta_dp`, inclusive zeros. Linhas individuais `REABRE` e `PAUTA_DP` só aparecem quando há itens. ([reabrir_orfas_enterradas.py:122](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/reabrir_orfas_enterradas.py:122), [reabrir_orfas_enterradas.py:179](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/reabrir_orfas_enterradas.py:179), [reabrir_orfas_enterradas.py:184](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/reabrir_orfas_enterradas.py:184))

2. **Colisão com aval vigente**

   O texto fornecido de **REGRAS-2, item 2(ii)** determina a mesma leitura de dias por porta e laços, julgamento por item fora da cerca e veto de dia indecidível; dentro da cerca, a porta veta o ato inteiro. Assim, a intenção de retirar o leitor próprio é coerente com esse texto, mas o ramo (b) precisa respeitar também o caso indecidível e a distinção dentro/fora da cerca. A redação “ENTREGUE vai à pauta, o resto reabre” não explicita o tratamento de indecidível, nem preserva literalmente as recusas próprias do comando. O texto do anexo foi fornecido na pergunta; sua presença em arquivo da cópia é **[não verificado]**. ([fio.py:1389](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/services/fio.py:1389), [reabrir_orfas_enterradas.py:105](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/reabrir_orfas_enterradas.py:105))

   A L-111 manda remover, com lápide e testes pertinentes, função que perdeu o último chamador de produção; **arquivo de reversão existente mais dry zerado não prova, por si só, ausência de chamadores ou impossibilidade de uso futuro**. O ramo (a) é uma determinação específica do aval fornecido, mas sua justificativa de “escritor morto” precisa dessa distinção. ([LEIS.md:137](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/docs/LEIS.md:137), [reabrir_orfas_enterradas.py:48](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/reabrir_orfas_enterradas.py:48))

   O texto integral do **corte das 18:00**, do **CERCA-B2-REGRAS 17:20** e de **ESPURIAS-ABRE** não está entre os anexos fornecidos nem foi localizado na cópia consultada: colisão com seus literais **[não verificado]**.

3. **Condições que não fecham no literal**

   - O dry normal não está demonstrado como operação sem escrita: a inicialização padrão usa `config.settings.saas`, que executa `os.makedirs('/app/logs', exist_ok=True)` e configura handlers de arquivo. Portanto, “sem `--apply`” não equivale a “nenhuma escrita no processo”. ([manage.py:9](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/manage.py:9), [saas.py:118](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/config/settings/saas.py:118), [saas.py:136](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/config/settings/saas.py:136))
   - Os zeros são observáveis no **resumo**, caso o comando termine; não é necessário esperar linhas individuais com zero. O par (a)/(b) cobre logicamente os resultados binários **se ambas as medições forem obtidas**. Arquivo inacessível, dry com erro ou medição inconclusiva não permitem escolher o ramo com segurança; a existência do arquivo tampouco prova que um apply terminou. ([reabrir_orfas_enterradas.py:124](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/reabrir_orfas_enterradas.py:124), [reabrir_orfas_enterradas.py:184](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/reabrir_orfas_enterradas.py:184))
   - O RED pedido fala em `data_evento` aberto e dia do turno entregue. Na cópia, `_dia_de_turno_da_pergunta(p)` devolve `p.data_evento` para pergunta que não é conversa. A porta compara a instância também com o valor **gravado**, que pode ser diferente da instância; o comando carrega `p` antes do ponto de escrita. É preciso especificar e demonstrar a situação de divergência, inclusive concorrência ou alteração da instância, para o RED morder esta implementação. ([juizes.py:338](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/juizes.py:338), [fio.py:1403](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/services/fio.py:1403), [reabrir_orfas_enterradas.py:67](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/reabrir_orfas_enterradas.py:67))
   - A pauta atual é agrupada pela competência calculada de `data_evento`. Se o veto decorrer de outro dia gravado ou indecidível, qual competência ancora a pauta é **[não verificado]**; não decorre automaticamente do booleano do veto. ([reabrir_orfas_enterradas.py:93](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/reabrir_orfas_enterradas.py:93), [reabrir_orfas_enterradas.py:199](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/reabrir_orfas_enterradas.py:199))

4. **Exportada, dinheiro e `!`**

   No apply atual, a pergunta selecionada para reabertura tem o carimbo alterado e salvo; o comando pode ainda reabrir disputa e chamado e registrar evento. A pauta cria registro próprio e trilha, sem que a chamada mostrada altere a folha gravada. A partição atual pretende evitar reabrir as exportadas segundo **apenas** `data_evento`; ela não prova proteção equivalente à leitura completa da porta em caso de dias divergentes. Efeitos indiretos completos dos sinais e do pai, inclusive sobre dinheiro ou outros objetos, são **[não verificado]**. ([models.py:3324](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/models.py:3324), [reabrir_orfas_enterradas.py:157](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/reabrir_orfas_enterradas.py:157), [pautas/services.py:180](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/pautas/services.py:180))

   O aval fornecido proíbe executar `--apply` em produção; nada nos dados mostra que foi executado ou que houve alteração de dinheiro. O alcance de outros cortes marcados `!`, sem seus textos integrais, é **[não verificado]**.

**Como medir sem escrever em produção.** Verificar a existência dos arquivos de reversão por listagem apenas de leitura; registrar separadamente “existe”, “não existe” ou “não foi possível verificar”. Para as contagens, executar a lógica de leitura em ambiente comprovadamente bloqueado para escrita: conexão de banco com permissão apenas de leitura e processo sem escrita no filesystem, inclusive bytecode e logs, com saída somente no terminal. É preciso validar essa configuração antes de confiar na medida; o comando padrão, por si, não dá essa garantia. A existência de arquivo e as contagens atuais em produção são **[não verificado]**. ([reabrir_orfas_enterradas.py:124](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/reabrir_orfas_enterradas.py:124), [saas.py:118](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/config/settings/saas.py:118))

No caminho sem `--apply`, as escritas identificadas são: **stdout** das linhas e resumos; alterações de **memória** nos contadores, mapa de identidade e memo; criação/verificação do diretório de logs na carga das settings; e abertura/possível escrita ou rotação de `errors.log` e `ponto.log` pelos handlers. O bloco explícito de reversão, `evento`, reabertura e `_pautas` está condicionado a `aplicar`. Escritas adicionais em toda a árvore transitiva de inicialização e leitores são **[não verificado]**, razão para impor a restrição externa de escrita na medição. ([reabrir_orfas_enterradas.py:64](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/reabrir_orfas_enterradas.py:64), [reabrir_orfas_enterradas.py:122](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/reabrir_orfas_enterradas.py:122), [juizes.py:894](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/juizes.py:894), [saas.py:160](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/config/settings/saas.py:160))

**PROCEDE:**

1. **Sim** — premissas (c) e (d) não conferem integralmente; (a) e (e) exigem as qualificações acima.
2. **Sim, parcialmente** — há incompatibilidade literal a resolver quanto a indecidível e ao tratamento por item; os demais cortes citados estão **[não verificado]**.
3. **Sim** — dry sem escrita, RED e resultados inconclusivos exigem correção ou prova.
4. **Não demonstrado** — escrita fora do declarado ou efeito em dinheiro **[não verificado]**; o risco sobre pergunta entregue pela divergência de dias é apontado pelo código.

## 17/11 20:32 · sessao 871eb8601199 · DIA-SEM-TRABALHO-PORTA · leitura · origem=Code

O pré-desenho **não está fechado para construção**: a cópia confirma as três autoridades de decisão e os caminhos que veem a órfã, mas não encontrei uma lista administrativa cujo escritor atual inclua comprovadamente a batida isolada em folga, nem o código das duas sondas que produziram as classes excepcionais. A Gestão de HE já recebe pares com minutos, porém sua porta recusa minutos zero; usar essa porta para uma batida solta sem antes regularizar o fato não está demonstrado. ([he.py:159](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/portas/he.py:159), [he.py:186](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/portas/he.py:186); sonda e destino dos 37 dias: **[não verificado]**.)

O dado raspado informa 238 dias, 582 batidas e 109 pessoas, divididos em 197 pares sem decisão, 37 dias com batida solta, três com HE decidida e um sem turno; dos 197, informa 139 com escala errada, 57 na lista de HE e um fora dela com escala confiável. Esses números e a classificação são do anexo fornecido, sem acesso ao banco ou aos arquivos originais nesta leitura: **[não verificado]**.

### (a) Batida solta em dia sem trabalho

A grade põe as batidas não consumidas como órfãs do dia; `julgar_celula` chega a `julgar_colab`, que conta órfãs entre as batidas do dia e monta a ata. Para folga com `trabalha=False`, não precisa esperar o fim de um turno inexistente. Isso prova que o cartório **vê** o fato, não que emita uma tarefa para ele. ([utils.py:790](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/escala/utils.py:790), [utils.py:1818](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/escala/utils.py:1818), [cartorio.py:1416](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/services/cartorio.py:1416), [cartorio.py:2011](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/services/cartorio.py:2011).)

O detector de turno órfão lê batidas apuráveis e seleciona turnos abertos com entrada, passados mais de 30 minutos; não equivale a todas as órfãs da ata. O caminho `perguntar_entradas_pendentes` pergunta por cada dia somente se `classificar_falta` devolver motivo; esta retorna tudo `None` quando a célula diz `trabalha=False`. O emissor de cluster também exclui o dia sem trabalho antes de exigir duas órfãs em rajada e uma lâmpada apagada. Portanto, esses caminhos específicos não emitem a pergunta para a folga descrita; afirmar que *nenhum outro emissor* cobre qualquer dos 37 casos seria **[não verificado]**. ([utils_turno_orfao.py:28](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/utils_turno_orfao.py:28), [disputa_emissao.py:257](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/chamados/services/disputa_emissao.py:257), [servico_jornada.py:308](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/escala/servico_jornada.py:308), [cluster_espurio.py:15](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/services/cluster_espurio.py:15).)

A **Cadastro x Realidade**, assinatura C9, é uma lista administrativa existente com ação de regularizar batida quando o dono lavrado é BATIDA. Seu escritor `teto_na_lista.lavrar` grava o retrato medido dos pares separados *pelo teto do dia*, que cabiam no envelope de 26 horas; isso exige a assinatura específica do pareador. Não há prova de que uma batida isolada sem par satisfaça esse predicado. Assim, C9 é um candidato de interface, mas **não está demonstrada como destino já alimentado dos 37 dias**; ampliar seu escritor ou leitor exigiria desenho adicional, não um escritor novo presumido. ([cadastro_realidade.py:99](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/escala/services/cadastro_realidade.py:99), [cadastro_realidade.py:731](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/escala/services/cadastro_realidade.py:731), [teto_na_lista.py:12](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/services/teto_na_lista.py:12), [teto_na_lista.py:110](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/services/teto_na_lista.py:110).)

| Estado | Quem grava hoje | O que a tela pode dizer / ação existente |
|---|---|---|
| Batida apurável sem marco, sem decisão | Grade produz a órfã; cartório monta a ata e `lavrar_veredito` a grava. ([cartorio.py:730](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/services/cartorio.py:730), [celula.py:1032](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/portas/celula.py:1032).) | Rótulo e linha administrativa para a batida isolada: **[não verificado]**. A regularização da batida já é uma ação oferecida pela lista C9 em seu universo próprio. ([cadastro_realidade.py:731](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/escala/services/cadastro_realidade.py:731).) |
| Par com HE pendente na lista | O retrato fornece dias e minutos; Gestão de HE sobrepõe a decisão vigente. ([gestao_he.py:63](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/services/gestao_he.py:63), [gestao_he.py:278](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/services/gestao_he.py:278).) | “Sem decisão”; autorizar ou dizer não pela porta HE, conforme permissão e minutos positivos. ([gestao_he.py:370](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/services/gestao_he.py:370), [he.py:175](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/portas/he.py:175).) |
| HE vigente, autorizada ou não | `he._gravar`; revogação usa operação própria. ([he.py:44](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/portas/he.py:44), [he.py:375](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/portas/he.py:375).) | Gestão lê o estado vivo; o rótulo específico da ESPURIA-TELA ainda é proposta. ([gestao_he.py:63](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/services/gestao_he.py:63); **[não verificado]**.) |
| Escala do dia corrigida | `editar_dia` é a operação existente; célula editada é distinta da gerada. ([celula.py:572](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/portas/celula.py:572), [models.py:1635](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/escala/models.py:1635).) | Corrigir o dia pela ação existente; apresentação após o rejulgamento depende dos fatos resultantes: **[não verificado]**. |
| Batida retratada | `retratar_batida` é a operação existente; o detector lê o universo de batidas apuráveis. ([registro_batida.py:287](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/registro_batida.py:287), [utils_turno_orfao.py:19](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/utils_turno_orfao.py:19).) | A condição deve desaparecer quando não restar batida apurável; a convergência completa da tela para este caso: **[não verificado]**. |

**RED proposto:** cadastro com folga confiável, célula `trabalha=False`, uma entrada apurável às 08:00, sem saída e sem decisão nas três autoridades. Esperado: uma linha para o admin identificar e regularizar o fato, sem pergunta ao colaborador; após retratação, a linha deixa de representar pendência. Mata o mutante que só inclui pares completos ou continua silencioso por `classificar_falta`. A lista e a operação concreta que produzem essa linha seguem **[não verificadas]**; por isso o RED especifica o resultado, ainda não uma implementação confirmada. ([servico_jornada.py:310](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/escala/servico_jornada.py:310), [he.py:190](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/portas/he.py:190).)

**Arquivos candidatos**, sujeitos à identificação do destino: leitor da lista em [cadastro_realidade.py](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/escala/services/cadastro_realidade.py:99), possivelmente o escritor existente apropriado e testes. Neste desenho não há necessidade demonstrada de tocar `ponto/services/cartorio.py`, `escala/utils.py` ou `chamados/services/vigia_fio.py`; a lista definitiva de arquivos é **[não verificada]**.

### (b) HE vigente e `ata.orfas`

**Escrita:** as duas montagens de grade geram órfãs; `ata_do_dia` copia suas horas e contagem; o cartório passa a ata a `lavrar_veredito`, cuja porta substitui a ata mesmo quando o veredito humano prevalece. `carimbar_precedencia` também escreve o objeto `ata`, mas preserva suas demais chaves; a atribuição em `celula_como_o_julgamento_deixa` é uma cópia em memória. ([utils.py:790](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/escala/utils.py:790), [utils.py:1818](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/escala/utils.py:1818), [cartorio.py:738](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/services/cartorio.py:738), [cartorio.py:1851](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/services/cartorio.py:1851), [celula.py:1058](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/portas/celula.py:1058), [celula.py:1205](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/portas/celula.py:1205), [celula.py:990](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/portas/celula.py:990).)

**Leitura direta ou através da grade da célula**, nos sítios identificados:

- Juízo e materialização: [juizes.py:514](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/chamados/juizes.py:514), [materializacao.py:437](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/chamados/services/materializacao.py:437), [juiz_batida.py:161](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/juiz_batida.py:161), [cluster_espurio.py:29](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/services/cluster_espurio.py:29), [supra_juiz.py:392](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/supra_juiz.py:392), [status_dia.py:188](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/status_dia.py:188).
- Projeção, relatório e calendário: [leitor_celula.py:181](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/escala/services/leitor_celula.py:181), [calendario.py:409](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/colaboradores/services/calendario.py:409), [cartao_pela_celula.py:288](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/relatorios/cartao_pela_celula.py:288), [pdf_espelho.py:477](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/relatorios/pdf_espelho.py:477), [pdf_espelho.py:876](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/relatorios/pdf_espelho.py:876), [furos_diarios.py:12](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/escala/services/furos_diarios.py:12), [causas.py:43](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/folha/causas.py:43).
- Outros consumidores encontrados: [esmeril_espelho.py:125](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/services/esmeril_espelho.py:125), [vigia_pareador_celula.py:74](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/services/vigia_pareador_celula.py:74), [tripwire_celulas.py:264](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/management/commands/tripwire_celulas.py:264), [orfa_classe.py:53](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/escala/orfa_classe.py:53), [propositor.py:39](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/propositor.py:39), [turnos.py:2679](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/turnos.py:2679), [simular_dia.py:12](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/services/simular_dia.py:12), [bordas_realizado.py:49](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/services/bordas_realizado.py:49).

A exaustividade de leitores indiretos de todas as projeções é **[não verificada]**. Em particular, nem todo uso de `dia['orfas']` prova que aquele objeto veio da ata em todas as chamadas. ([leitor_celula.py:181](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/escala/services/leitor_celula.py:181).)

A autoridade é `DecisaoHE.objects.vigentes()`, exposta em lote por `gestao_he.estado_por_dia`; ela inclui os estados `autorizado` e `nao` e exclui revogadas. O motor já lê autorização no bloqueio L-097, mas isso não remove órfãs da ata. O ponto proposto é **na apresentação e na contagem da pendência**, consultando esse mapa por pessoa e dia e conservando a ata como fato; não apagar a órfã nem criar outra regra de vigência. A linha exata do contador de “carga” a alterar ainda é **[não verificada]**: `FuroDiario`, por exemplo, já exclui dias não trabalho e dias sem marcos previstos, portanto não se pode afirmar que seja a carga dos três casos. ([models.py:1251](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/models.py:1251), [gestao_he.py:63](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/services/gestao_he.py:63), [motor_calculo_v2.py:1455](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/motor_calculo_v2.py:1455), [furos_diarios.py:43](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/escala/services/furos_diarios.py:43).)

**RED proposto:** folga confiável, par 08:00–17:00 ainda em `ata.orfas`, primeiro com decisão vigente `autorizado`, depois `nao`. Nos dois casos, esperado: zero na contagem de **sem decisão**, ata preservada; revogar uma decisão elegível devolve o caso à pendência. Mata os mutantes que olham só `autorizado`, que tratam linha revogada como vigente ou que apagam o fato da ata. Os três dias do anexo são o caso motivador; seus dados individuais são **[não verificados]**. ([models.py:1265](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/models.py:1265), [models.py:1283](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/models.py:1283).)

**Arquivos candidatos:** os consumidores efetivos do contador e da ESPURIA-TELA, ainda não identificados; leitura de [gestao_he.py](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/services/gestao_he.py:63) e testes. Não está demonstrada necessidade de tocar `cartorio.py`, `escala/utils.py` ou `chamados/services/vigia_fio.py`; a lista definitiva é **[não verificada]**.

### (c) Rótulo por dia

- **“Escala a corrigir”**: perguntar `motivo_escala_errada`, que resolve o vínculo do dia e devolve `LIMBO`, `A2`, `A8` ou `None`; as assinaturas A2/A8 vêm de `assinaturas_do_colab`. `None` é a resposta dessa função, não prova independente de que todo cadastro esteja correto. ([escala_certa.py:19](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/escala/services/escala_certa.py:19), [escala_certa.py:31](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/escala/services/escala_certa.py:31).)
- **“Decidir na Gestão de HE”**: pertencer ao universo apresentado por `ler` + `enriquecer` e permanecer `sem_decisao`, com os filtros e dono aplicáveis. Ausência de `DecisaoHE` sozinha **não prova** pertencimento à lista. ([gestao_he.py:370](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/services/gestao_he.py:370), [gestao_he.py:399](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/services/gestao_he.py:399).)
- **Peça (a)**: somente o fato apurável remanescente, depois de ler decisão HE vigente, o efeito da edição do dia e a retratação da batida. A precedência operacional proposta é escala errada → lista HE sem decisão → destino da peça (a); como a identificação concreta deste último destino não foi demonstrada, o rótulo exato e sua fonte final são **[não verificados]**. ([celula.py:572](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/portas/celula.py:572), [registro_batida.py:287](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/registro_batida.py:287), [gestao_he.py:63](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/services/gestao_he.py:63).)

**RED proposto:** três pessoas-dia com batidas 08:00–17:00 em folga: uma com assinatura A8, outra com escala confiável e par presente na Gestão sem decisão, outra com escala confiável e apenas entrada 08:00. Esperar, respectivamente, “escala a corrigir”, “decidir na Gestão de HE” e destino da peça (a). Acrescentar decisão HE vigente, edição para trabalho e retratação, uma por vez, e verificar que não se afirma “sem decisão” com base em uma única fonte. Mata mutantes que usam apenas ausência de `DecisaoHE`, classificam A8 como HE ou tratam uma entrada isolada como par. O comportamento de integração após cada edição é **[não verificado]**. ([escala_certa.py:19](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/escala/services/escala_certa.py:19), [gestao_he.py:370](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/services/gestao_he.py:370).)

**Arquivos candidatos:** o consumidor real da ESPURIA-TELA e seus testes, a identificar; leitura das autoridades já citadas. Não está demonstrada necessidade de editar `ponto/services/cartorio.py`, `escala/utils.py` ou `chamados/services/vigia_fio.py`. O L2 citado no aval depende da B2 e não é incluído neste desenho; o estado atual do pouso da B2 é **[não verificado]**. ([cartorio.py:1458](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/services/cartorio.py:1458).)

### Sonda — texto, não executado

**Não consigo reproduzir fielmente as duas classificações com o material disponível.** Os anexos nomeiam `sonda_dst_par.py` e `sonda_dst_he.py`, mas seu código e os identificadores dos dois casos não foram fornecidos nem localizados nesta cópia: **[não verificado]**. Falta, em particular, a função real da sonda que associa cada órfã a “par completo” ou “sem turno” e o recorte exato usado para comparar com a lista de HE. Abaixo está uma **sonda diagnóstica parcial**, que exige os dois pares `(pk, dia)` depois de recuperados pela sonda original; ela não afirma classificá-los nem reproduzir os totais. Usa somente consultas ORM e operações locais. A data inicial 21/10 é a janela informada no brief fornecido, não uma janela verificada para todas as empresas: **[não verificado]**. Os campos inspecionados existem nos modelos e a leitura de vigência usa a autoridade existente. ([models.py:8](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/models.py:8), [models.py:1635](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/escala/models.py:1635), [models.py:1265](/home/ronald/codex_ws/407dba82b25b5b2d8cc53d76721016011db4c259/app/ponto/models.py:1265).)

```python
# Texto para manage.py shell. Não executado.
# Preencher apenas depois de obter os pares pela sonda original.
from datetime import date, datetime, time, timedelta
from django.utils import timezone
from django_tenants.utils import schema_context
from escala.models import CelulaDia
from escala.servico_jornada import escala_vigente
from ponto.models import Batida, DecisaoHE

CASOS = [
    # ("FORA da lista, escala confiavel", PK_COLAB, date(2026, 11, DD)),
    # ("orfa sem turno no dia", PK_COLAB, date(2026, 11, DD)),
]

with schema_context("juliani"):
    for classe_informada, pk, dia in CASOS:
        if not date(2026, 10, 21) <= dia <= date(2026, 11, 16):
            raise ValueError("Dia fora da janela informada no brief")

        cel = (CelulaDia.objects
               .filter(colab

[truncado em 20000 de 23823 caracteres]

## 17/11 20:32 · sessao 5477278bf9c2 · PORTA-EXPORT · verificador · origem=Code

1. **Sim.** Um mutante pode passar pela regra nova e publicar um número errado. Em [recusa_estado.py:76](/home/ronald/codex_ws/0e1b295059beb7be986b838e398830d109e51b1d/bin/recusa_estado.py:76), substitua apenas o `d['recusas']` usado como argumento de `%d` por:

   ```python
   (d['recusas'],)[0] + ((d['recusas'],)[0] >= 1000001)
   ```

   Para `recusas = 1000001`, a frase publicaria `1000002`. Cada ocorrência de `d['recusas']` tem como **pai imediato** uma tupla, que a regra aceita; ela não verifica o que acontece depois com a tupla. A alteração também não introduz atribuição, proibida em `linha_porta_export`. Os valores testados vão até `1000000`, com lacunas abaixo disso, e a comparação do VERMELHO permanece intacta. A checagem do ESTADO compara a saída com a do próprio medidor mutado. Portanto, pela leitura do selo, esse mutante passa; **execução do mutante e seu rc: [não verificado]**. [test_recusa_estado.sh:95](/home/ronald/codex_ws/0e1b295059beb7be986b838e398830d109e51b1d/bin/tests/test_recusa_estado.sh:95), [test_recusa_estado.sh:109](/home/ronald/codex_ws/0e1b295059beb7be986b838e398830d109e51b1d/bin/tests/test_recusa_estado.sh:109), [test_recusa_estado.sh:117](/home/ronald/codex_ws/0e1b295059beb7be986b838e398830d109e51b1d/bin/tests/test_recusa_estado.sh:117), [test_recusa_estado.sh:126](/home/ronald/codex_ws/0e1b295059beb7be986b838e398830d109e51b1d/bin/tests/test_recusa_estado.sh:126).

   O anexo relata `rc=1` para M8–M11 e `rc=0` para a base; esses resultados não incluem o mutante acima. **Reexecução: [não verificado].**

2. **No código atual, a regra nova aceita os usos presentes:** eles são elementos de tupla ou operandos de comparação. Uma f-string equivalente, porém, colocaria `d['recusas']` sob um nó `FormattedValue`, fazendo a regra rejeitá-la apesar da saída correta. [recusa_estado.py:53](/home/ronald/codex_ws/0e1b295059beb7be986b838e398830d109e51b1d/bin/recusa_estado.py:53), [recusa_estado.py:75](/home/ronald/codex_ws/0e1b295059beb7be986b838e398830d109e51b1d/bin/recusa_estado.py:75), [test_recusa_estado.sh:113](/home/ronald/codex_ws/0e1b295059beb7be986b838e398830d109e51b1d/bin/tests/test_recusa_estado.sh:113).

   É **contrato sintático explícito do selo**, que exige essa forma; é também **defeito se a intenção for aceitar qualquer implementação semanticamente correta**, pois rejeita uma formatação legítima e, como no item 1, não garante a propriedade semântica pretendida. [test_recusa_estado.sh:106](/home/ronald/codex_ws/0e1b295059beb7be986b838e398830d109e51b1d/bin/tests/test_recusa_estado.sh:106).

3. **Valores lidos pelo que mudou no delta:**

   - `recusas`: `contar` incrementa e escreve o total no dicionário; `linha` e `linha_porta_export` o leem para publicar a contagem, e a segunda também o lê para a comparação do VERMELHO. O delta passa a testar `linha` e a inspecionar os usos nas duas funções. [recusa_critica.py:114](/home/ronald/codex_ws/0e1b295059beb7be986b838e398830d109e51b1d/app/core/recusa_critica.py:114), [recusa_critica.py:121](/home/ronald/codex_ws/0e1b295059beb7be986b838e398830d109e51b1d/app/core/recusa_critica.py:121), [recusa_estado.py:53](/home/ronald/codex_ws/0e1b295059beb7be986b838e398830d109e51b1d/bin/recusa_estado.py:53), [recusa_estado.py:75](/home/ronald/codex_ws/0e1b295059beb7be986b838e398830d109e51b1d/bin/recusa_estado.py:75), [test_recusa_estado.sh:101](/home/ronald/codex_ws/0e1b295059beb7be986b838e398830d109e51b1d/bin/tests/test_recusa_estado.sh:101).
   - `dia` e `por_usuario`: `contar` escreve a data-alvo e as contagens por usuário; `linha` lê ambos. No teste novo, são fornecidos diretamente a `linha` como data fixa e dicionário vazio. [recusa_critica.py:113](/home/ronald/codex_ws/0e1b295059beb7be986b838e398830d109e51b1d/app/core/recusa_critica.py:113), [recusa_critica.py:122](/home/ronald/codex_ws/0e1b295059beb7be986b838e398830d109e51b1d/app/core/recusa_critica.py:122), [recusa_estado.py:52](/home/ronald/codex_ws/0e1b295059beb7be986b838e398830d109e51b1d/bin/recusa_estado.py:52), [test_recusa_estado.sh:102](/home/ronald/codex_ws/0e1b295059beb7be986b838e398830d109e51b1d/bin/tests/test_recusa_estado.sh:102).
   - `rotulo`: `resolver_dia_recusado` o escreve e `linha` o lê; no teste novo, o selo fornece `R`. [recusa_critica.py:126](/home/ronald/codex_ws/0e1b295059beb7be986b838e398830d109e51b1d/app/core/recusa_critica.py:126), [recusa_estado.py:53](/home/ronald/codex_ws/0e1b295059beb7be986b838e398830d109e51b1d/bin/recusa_estado.py:53), [test_recusa_estado.sh:102](/home/ronald/codex_ws/0e1b295059beb7be986b838e398830d109e51b1d/bin/tests/test_recusa_estado.sh:102).
   - `esperado`: o leitor da porta do export escreve `0` e `linha_porta_export` o lê; o laço já existia na volta 6 e agora recebe os valores adicionais de `NS`. [recusa_critica.py:133](/home/ronald/codex_ws/0e1b295059beb7be986b838e398830d109e51b1d/app/core/recusa_critica.py:133), [recusa_estado.py:75](/home/ronald/codex_ws/0e1b295059beb7be986b838e398830d109e51b1d/bin/recusa_estado.py:75), [test_recusa_estado.sh:95](/home/ronald/codex_ws/0e1b295059beb7be986b838e398830d109e51b1d/bin/tests/test_recusa_estado.sh:95).

PROCEDE: N

## 17/11 20:25 · sessao 11d80dfb7162 · PORTA-EXPORT · verificador · origem=Code

**O selo ainda tem uma lacuna que bloqueia esta volta:** um mutante da frase da porta do export pode publicar outro número para contagens a partir de 60 e passar pelas verificações mostradas; essa passagem é uma inferência do código, não uma execução observada. [bin/tests/test_recusa_estado.sh:95](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/bin/tests/test_recusa_estado.sh:95) [não verificado]

1. **Mutantes.** O mutante da volta 5, que incrementa `recusas` quando ela é diferente de 2, foi rejeitado no anexo M6 (`rc=1`); o M7 também foi rejeitado (`rc=1`). O teste atual cobre `0..59` e a AST rejeita as atribuições do M6. [bin/tests/test_recusa_estado.sh:95](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/bin/tests/test_recusa_estado.sh:95) [não verificado: anexo fornecido sem numeração de linhas]

   Ainda passa, por inspeção, este mutante em [bin/recusa_estado.py:76](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/bin/recusa_estado.py:76):

   ```python
   # Substituir somente o argumento numérico da frase:
   d['recusas'] + (d['recusas'] >= 60)
   ```

   Para 60, publica 61; não introduz atribuição na AST e se comporta como o original nos valores testados. A comparação com o ESTADO usa a própria função mutada para produzir a frase esperada, portanto não denuncia a alteração. [bin/tests/test_recusa_estado.sh:95](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/bin/tests/test_recusa_estado.sh:95) [bin/tests/test_recusa_estado.sh:110](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/bin/tests/test_recusa_estado.sh:110) [não verificado: mutante não executado]

2. **`linha` tem a mesma lacuna.** Em [bin/recusa_estado.py:54](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/bin/recusa_estado.py:54), substituir `d['recusas']` por `d['recusas'] + 1` publicaria um número diferente; o selo verifica o contador sintético, mas não compara o número da frase de `linha` com esse contador, e usa a própria `linha` para a frase esperada no ESTADO. [bin/tests/test_recusa_estado.sh:76](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/bin/tests/test_recusa_estado.sh:76) [bin/tests/test_recusa_estado.sh:110](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/bin/tests/test_recusa_estado.sh:110) [não verificado: mutante não executado]

   Classifico como **fila desta obra**: é uma lacuna real do selo compartilhado para o contador preexistente do Resolver dia, enquanto o delta desta volta pretende fechar a fidelidade da frase da porta do export. Se o aceite exigido abranger expressamente os dois contadores, ela também deve bloquear esse aceite. [bin/tests/test_recusa_estado.sh:2](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/bin/tests/test_recusa_estado.sh:2) [bin/tests/test_recusa_estado.sh:88](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/bin/tests/test_recusa_estado.sh:88)

3. **Escritores e leitores dos valores.**

   - As recusas do Resolver passam pelos envelopes `resposta_acao` e `resposta_render`, que chamam `registrar`; as recusas da porta do export passam por `_recusa`, chamada nos caminhos de falha ou resultado diferente de zero. `registrar` emite a linha no logger; o settings da cópia configura `pontofile` para `/app/logs/ponto.log`. [app/core/respostas.py:54](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/app/core/respostas.py:54) [app/core/respostas.py:78](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/app/core/respostas.py:78) [app/folha/porta_export.py:1063](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/app/folha/porta_export.py:1063) [app/folha/porta_export.py:1080](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/app/folha/porta_export.py:1080) [app/folha/porta_export.py:1260](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/app/folha/porta_export.py:1260) [app/core/recusa_critica.py:80](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/app/core/recusa_critica.py:80) [app/config/settings/saas.py:160](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/app/config/settings/saas.py:160)
   - `linhas_do_arquivo` lê o log e rotados; `contar` lê data, porta e usuário e produz `dia`, `porta`, `recusas` e `por_usuario`. Os dois leitores acrescentam seus rótulos; o leitor do export escreve `esperado = 0`. [app/core/recusa_critica.py:95](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/app/core/recusa_critica.py:95) [app/core/recusa_critica.py:108](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/app/core/recusa_critica.py:108) [app/core/recusa_critica.py:126](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/app/core/recusa_critica.py:126) [app/core/recusa_critica.py:133](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/app/core/recusa_critica.py:133)
   - `ler` e `ler_porta_export` devolvem esses dados ou constroem `sem_arquivo`, `rotulo`, `dia` e `caminho` quando o arquivo principal falta; `linha` e `linha_porta_export` leem os campos para formatar. O gerador reúne as duas frases em `recusa_txt` e o insere no ESTADO; a API também consome o leitor do Resolver. [bin/recusa_estado.py:37](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/bin/recusa_estado.py:37) [bin/recusa_estado.py:48](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/bin/recusa_estado.py:48) [bin/recusa_estado.py:60](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/bin/recusa_estado.py:60) [bin/recusa_estado.py:71](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/bin/recusa_estado.py:71) [bin/gerar_estado.py:270](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/bin/gerar_estado.py:270) [bin/gerar_estado.py:384](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/bin/gerar_estado.py:384) [app/api/views_mensageria.py:1870](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/app/api/views_mensageria.py:1870)
   - No teste novo, `n` e `esp` são dados sintéticos locais; a frase `f` é lida pela comparação de número e vermelho, e a AST de `recusa_estado.py` é lida para procurar atribuições. [bin/tests/test_recusa_estado.sh:91](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/bin/tests/test_recusa_estado.sh:91)

4. **Efeitos e vermelho alheio ao contrato.** O teste novo em si importa a função e lê o arquivo para a AST; o selo define `PYTHONDONTWRITEBYTECODE=1`, cria seu cenário e ESTADO em diretório temporário e o remove ao sair. Contudo, o gerador também executa `git status` e consulta o tmux na raiz real: não há base suficiente para garantir ausência de todo efeito externo desses processos em qualquer ambiente. [bin/tests/test_recusa_estado.sh:11](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/bin/tests/test_recusa_estado.sh:11) [bin/tests/test_recusa_estado.sh:59](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/bin/tests/test_recusa_estado.sh:59) [bin/tests/test_recusa_estado.sh:91](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/bin/tests/test_recusa_estado.sh:91) [bin/gerar_estado.py:179](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/bin/gerar_estado.py:179) [não verificado: efeitos externos efetivos]

   O selo pode ficar vermelho por uma mudança legítima no log entre a geração e a leitura posterior da frase esperada, ou por atravessar a meia-noite entre esses processos. Também pode falhar por erro em outra seção necessária à geração do ESTADO. [bin/tests/test_recusa_estado.sh:107](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/bin/tests/test_recusa_estado.sh:107) [bin/tests/test_recusa_estado.sh:110](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/bin/tests/test_recusa_estado.sh:110) [bin/gerar_estado.py:198](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/bin/gerar_estado.py:198) [bin/recusa_estado.py:41](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/bin/recusa_estado.py:41)

Não executei o selo nem mutantes, pois eles criam arquivos temporários e a ordem foi somente leitura. O estado do log e as contagens de produção são **[não verificado]**. [bin/tests/test_recusa_estado.sh:59](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/bin/tests/test_recusa_estado.sh:59)

**PROCEDE: 1** — o mutante da porta do export para contagens a partir de 60 permanece sem detecção no selo. [bin/tests/test_recusa_estado.sh:95](/home/ronald/codex_ws/310f14318a399d6116ec2666f384720014c71f2c/bin/tests/test_recusa_estado.sh:95) [não verificado: mutante não executado]

## 17/11 20:21 · sessao 039ca51e6204 · CURA-DE-FASE · censo · origem=Code

A classificação **DATA GARANTIDA** abaixo vale para os caminhos de produção rastreados; quando a origem de um parâmetro não foi fechada, marquei **DUVIDOSO**. As referências são relativas à raiz da cópia. [não verificado: identidade do HEAD por Git, pois a cópia não contém `.git`]

| arquivo:linha | alvo | fonte | veredito |
|---|---|---|---|
| app/api/views.py:588 | escala_vigente | `localtime(ts_efetivo).date()`, app/api/views.py:584 | DATA GARANTIDA |
| app/api/views_core.py:574 | escala_vigente | `localtime(ts_efetivo).date()`, app/api/views_core.py:570 | DATA GARANTIDA |
| app/chamados/detector_anomalias.py:146 | escala_vigente | `localtime(timestamp_batida).date()`, app/chamados/detector_anomalias.py:139–140 | DATA GARANTIDA |
| app/chamados/juizes.py:2203 | escala_vigente | Chamador passa `min(_d_perg, _d_ch)` após subtração; `_d_ch` é convertido a data, app/chamados/juizes.py:2363–2377 | DATA GARANTIDA |
| app/chamados/juizes.py:321 | escala_vigente | Parâmetro recebido de materialização: data da pergunta ou conversão do contexto, app/chamados/services/materializacao.py:613–616, :692; falta fechar a origem de todas as instâncias em memória | DUVIDOSO |
| app/chamados/juizes.py:519 | escala_vigente | `date.fromisoformat(str(data_iso)[:10])`, app/chamados/juizes.py:469–473 | DATA GARANTIDA |
| app/chamados/juizes.py:2009 | vinculo_do_dia | Chamadores encontrados omitem `data`, usando `localdate()`, app/chamados/juizes.py:1996; app/chamados/views.py:929; app/chamados/services/fio.py:509; app/chamados/services/acoes_chamado.py:422 | DATA GARANTIDA |
| app/chamados/management/commands/auditar_disputas.py:112 | escala_vigente | `timezone.localdate(b.timestamp)`, app/chamados/management/commands/auditar_disputas.py:110–111 | DATA GARANTIDA |
| app/chamados/management/commands/auditar_invariantes_chamados.py:391 | vinculo_do_dia | `c.data` de `CelulaDia` consultada pelo ORM, app/chamados/management/commands/auditar_invariantes_chamados.py:331–333; campo em app/escala/models.py:1685 | DATA GARANTIDA |
| app/chamados/management/commands/auditar_invariantes_chamados.py:1538 | vinculo_cobre_a_data | `f.data` de `FolgaDia` consultada pelo ORM, app/chamados/management/commands/auditar_invariantes_chamados.py:1527, :1536–1538; app/escala/models.py:1588 | DATA GARANTIDA |
| app/chamados/management/commands/auditar_invariantes_chamados.py:361 | q_vinculo_cobre_a_data | `p.data_evento` de pergunta consultada pelo ORM, app/chamados/management/commands/auditar_invariantes_chamados.py:347–348; `DateField`, app/chamados/models.py:2528 | DATA GARANTIDA |
| app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:87 | escala_vigente | `t.data_turno`, app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:115, :145; construída de timestamp local e `.date()`, app/ponto/turnos.py:45, :285–308, :318–323 | DATA GARANTIDA |
| app/chamados/services/carga_pergunta_viva.py:162 | escala_vigente | `dia` é parâmetro; também recebe `data_evento` de `classificar_falta`, app/escala/servico_jornada.py:280, :363. Faltou fechar todos os chamadores dessa função | DUVIDOSO |
| app/chamados/services/disputa_emissao.py:833 | escala_vigente | Chamador passa data extraída do JSON por `_ddc`, app/chamados/services/disputa_emissao.py:1246–1249; parser converte, app/chamados/data_evento.py:7–13, :27–39 | DATA GARANTIDA |
| app/chamados/services/disputa_emissao.py:923 | escala_vigente | `data_evento` é parâmetro, app/chamados/services/disputa_emissao.py:901; há caminhos com conversão de JSON, app/chamados/services/disputa_emissao.py:1184–1185, :1246–1254, mas faltou fechar todos os chamadores | DUVIDOSO |
| app/chamados/services/limbo_folgas.py:321 | escala_vigente | `dia_do_fato`, app/chamados/services/limbo_folgas.py:318; célula de `DateField` ou datas convertidas, app/escala/models.py:1685; app/chamados/juizes.py:51–54, :99–122, :162–170 | DATA GARANTIDA |
| app/chamados/services/limbo_folgas.py:200 | vinculo_cobre_a_data | `dia_do_fato`, app/chamados/services/limbo_folgas.py:197; fontes e conversões em app/chamados/juizes.py:51–54, :99–122, :162–170; app/escala/models.py:1685 | DATA GARANTIDA |
| app/colaboradores/queries.py:64 | q_vinculo_cobre_a_data | `timezone.localdate()`, app/colaboradores/queries.py:62 | DATA GARANTIDA |
| app/colaboradores/services/calendario.py:536 | vinculo_cobre_a_data | `data_ini` construída como data ou convertida do GET, app/colaboradores/services/calendario.py:204–241; janela da competência, app/ponto/janelas.py:10–21 | DATA GARANTIDA |
| app/colaboradores/services/calendario.py:537 | vinculo_cobre_a_data | `data_fim` construída como data ou convertida do GET, app/colaboradores/services/calendario.py:204–241; app/ponto/janelas.py:10–21 | DATA GARANTIDA |
| app/colaboradores/services/calendario.py:418 | vinculo_do_dia | `_data` vem da grade, app/colaboradores/services/calendario.py:363, :382–383; grade constrói dias da janela, app/escala/services/leitor_celula.py:375–390; janela em app/colaboradores/services/calendario.py:204–241 | DATA GARANTIDA |
| app/colaboradores/services/feriado_do_posto.py:103 | vinculo_do_dia | `cel.data` de consulta a `CelulaDia`, app/colaboradores/services/feriado_do_posto.py:94–96; `DateField`, app/escala/models.py:1685 | DATA GARANTIDA |
| app/colaboradores/services/ficha.py:129 | vinculo_do_dia | Chamadas produtivas identificadas usam o padrão `localtime(now()).date()`, app/colaboradores/services/ficha.py:117–118, :269–275; app/api/views_mensageria.py:1133–1134 | DATA GARANTIDA |
| app/colaboradores/services/vinculo.py:955 | vinculo_cobre_a_data | Chamador passa `timezone.localdate()`, app/colaboradores/services/estrutura.py:62 | DATA GARANTIDA |
| app/colaboradores/views.py:2652 | q_vinculo_cobre_a_data | `timezone.localdate()`, app/colaboradores/views.py:2650 | DATA GARANTIDA |
| app/core/feriados.py:161 | vinculo_do_dia | Dias construídos por aritmética de datas, app/core/feriados.py:147–150; exemplos de fontes em app/colaboradores/services/calendario.py:204–241, :347 e app/relatorios/pdf_espelho.py:298 | DATA GARANTIDA |
| app/escala/alimentacao.py:358 | vinculo_do_dia | `d` vem dos pares recebidos, app/escala/alimentacao.py:334, :350; outro carregador repassa parâmetro `data`, :320–330. Faltou fechar as fontes de todos os chamadores | DUVIDOSO |
| app/escala/alimentacao.py:285 | vinculo_cobre_a_data | `data` vem de `escala_vigente`, app/escala/servico_jornada.py:66; chamada recebe `x['virada']`, app/ponto/management/commands/censo_fase_12x36.py:413, gerada por `str(virada)`, :224 | TEXTO POSSÍVEL |
| app/escala/folgas.py:487 | vinculo_cobre_a_data | `fd.data`; chamadores identificados carregam `FolgaDia` pelo ORM, app/escala/folgas.py:535–541; app/escala/management/commands/reparentar_folgas.py:28–34; campo em app/escala/models.py:1588 | DATA GARANTIDA |
| app/escala/folgas.py:451 | q_vinculo_cobre_a_data | `data` é parâmetro, app/escala/folgas.py:442; não foi identificado chamador de produção que estabeleça sua fonte | DUVIDOSO |
| app/escala/folgas.py:539 | vinculo_cobre_a_data | `fd.data` de consulta a `FolgaDia`, app/escala/folgas.py:535–538; app/escala/models.py:1588 | DATA GARANTIDA |
| app/escala/folgas.py:624 | vinculo_cobre_a_data | `data` consultada de `CelulaDia`, app/escala/folgas.py:621–623; `DateField`, app/escala/models.py:1685 | DATA GARANTIDA |
| app/escala/management/commands/reparentar_folgas.py:31 | vinculo_cobre_a_data | `fd.data` de consulta a `FolgaDia`, app/escala/management/commands/reparentar_folgas.py:28–30; app/escala/models.py:1588 | DATA GARANTIDA |
| app/escala/models.py:929 | q_vinculo_cobre_a_data | Chamador encontrado omite `hoje`, app/escala/management/commands/vinculos_5x2_6x1_trabalha_feriado.py:15; padrão `date.today()`, app/escala/models.py:920, :926 | DATA GARANTIDA |
| app/escala/services/cadastro_realidade.py:513 | vinculo_do_dia | Dias extraídos de células consultadas pelo ORM, app/escala/services/cadastro_realidade.py:452–456, :497–498; app/escala/models.py:1685 | DATA GARANTIDA |
| app/escala/services/cadastro_realidade.py:765 | vinculo_do_dia | `hoje` é repassado por `lista`, app/escala/services/cadastro_realidade.py:748, :773, :780–781, :877; faltou fechar todas as bordas que fornecem `hoje` | DUVIDOSO |
| app/escala/services/cadastro_realidade.py:761 | q_vinculo_cobre_a_data | Mesmo parâmetro `hoje` de `lista`, app/escala/services/cadastro_realidade.py:748, :773, :780–781, :877; faltou fechar todas as bordas | DUVIDOSO |
| app/escala/services/escala_certa.py:31 | escala_vigente | Caminho identificado: motor passa `data_jornada`, app/ponto/motor_calculo_v2.py:2403–2409, :2461; vem de `.date()` ou `t.data_turno`, :884–895; app/ponto/turnos.py:45–48, :285–308 | DATA GARANTIDA |
| app/escala/services/fase_na_tela.py:161 | q_vinculo_cobre_a_data | Chamador passa `hoje=localdate()` e `contra`, app/colaboradores/views_fase.py:54, :67, :73; `contra` vem dos dias trabalhados, app/escala/services/fase_na_tela.py:40–47, :113–120; app/escala/services/propositor.py:106–118; app/ponto/turnos.py:45–48, :285–308 | DATA GARANTIDA |
| app/escala/services/propositor.py:448 | vinculo_do_dia | `d` é construído com `timedelta` a partir de `hoje`, app/escala/services/propositor.py:415–417 | DATA GARANTIDA |

Nos seis sítios de `q_vinculo_cobre_a_data`, o `Q` monta lookups `lte`/`gte` sobre os campos de vínculo, que são `DateField` ([alimentacao.py:219](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/alimentacao.py:219), [models.py:875](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/models.py:875)). **Conforme o dado fornecido**, o lookup do Django coage texto ISO válido; a coerção não foi verificada independentemente na cópia [não verificado]. A comparação Python da juíza é direta ([alimentacao.py:214](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/alimentacao.py:214)).

**LOTE 0: 40 linhas, DATA GARANTIDA 32, TEXTO POSSÍVEL 1, DUVIDOSO 7.**

## 17/11 20:20 · sessao 3e36c6765b3a · CURA-DE-FASE · censo · origem=Code

O lote tem **39 chamadas**. Nos casos duvidosos, a tabela identifica a parte da cadeia que foi localizada e o que falta comprovar; isso não demonstra, por si só, que texto alcance a chamada.

| arquivo:linha | alvo | fonte | veredito |
|---|---|---|---|
| `app/ponto/portas/regen_exportada.py:152` | `vinculo_cobre_a_data` | `_d=desde` em `app/ponto/portas/regen_exportada.py:150`; a guarda exige `date` em `:118`; chamador filtra datas em `app/chamados/services/passivo_validado.py:450` e passa o intervalo em `:453`. | DATA GARANTIDA |
| `app/ponto/precedencia.py:349` | `vinculo_do_dia` | `data` parte de `data_ini` em `app/ponto/precedencia.py:334`, parâmetro de `:259`; falta fechar as fontes dos chamadores. | DUVIDOSO |
| `app/ponto/precedencia.py:537` | `escala_vigente` | `d` deriva do parâmetro `data` em `app/ponto/precedencia.py:430,507`; um caminho converte ISO em `app/ponto/views.py:2520,2547` e passa por `app/ponto/services/ausencia.py:483,524`; demais fontes não fechadas. | DUVIDOSO |
| `app/ponto/precedencia.py:510` | `escala_vigente` | Mesma cadeia: `data` em `app/ponto/precedencia.py:430,507`; caminho convertido em `app/ponto/views.py:2520,2547` e `app/ponto/services/ausencia.py:483,524`; demais fontes não fechadas. | DUVIDOSO |
| `app/ponto/propositor.py:350` | `escala_vigente` | `dia` vem de `t.data_turno` em `app/chamados/reconciliador.py:96,113`; campo `DateField` em `app/ponto/models.py:1208,1215`. | DATA GARANTIDA |
| `app/ponto/registro_batida.py:29` | `escala_vigente` | `timezone.localtime(timestamp).date()` em `app/ponto/registro_batida.py:28`. | DATA GARANTIDA |
| `app/ponto/registro_batida.py:44` | `escala_vigente` | `timestamp` localizado e `.date()` em `app/ponto/registro_batida.py:40,43`. | DATA GARANTIDA |
| `app/ponto/services/cadastro_futuro.py:70` | `vinculo_do_dia` | `cel.data` das células consultadas em `app/ponto/services/cadastro_futuro.py:64`; `DateField` em `app/escala/models.py:1635,1685`. | DATA GARANTIDA |
| `app/ponto/services/cartorio.py:875` | `escala_vigente` | Fontes rastreadas: `cel.data` em `app/ponto/services/cartorio.py:1353,1364`, `app/ponto/services/vigia_de_hora.py:126` e `app/chamados/services/fabrica_da_celula.py:46`; ISO convertido em `app/ponto/management/commands/recusar_ponta_pequena.py:133,150`; outro caminho faz aritmética de data em `app/ponto/services/cartorio.py:378,395,445`. Campo em `app/escala/models.py:1685`. | DATA GARANTIDA |
| `app/ponto/services/cartorio.py:1903` | `_vinculo_do_dia` | Chamadores passam `data` em `app/ponto/services/cartorio.py:1783,1819,1845`, obtida de `cel.data` em `:1353`; campo em `app/escala/models.py:1685`. | DATA GARANTIDA |
| `app/ponto/services/cartorio.py:2035` | `escala_vigente` | `data` é parâmetro em `app/ponto/services/cartorio.py:2011`; comparação com `localdate()` em `:2016,2025` antecede a chamada, mas as fontes dos chamadores não foram fechadas. | DUVIDOSO |
| `app/ponto/services/cartorio.py:1768` | `_vinculo_do_dia` | `data=cel.data` em `app/ponto/services/cartorio.py:1353`; campo em `app/escala/models.py:1685`. | DATA GARANTIDA |
| `app/ponto/services/cobertura.py:278` | `escala_vigente` | `dia` e `hoje` são parâmetros em `app/ponto/services/cobertura.py:247`; `hoje` pode ser fornecido e há comparação em `:273`; falta comprovar as fontes de ambos. | DUVIDOSO |
| `app/ponto/services/declarar_dia.py:58` | `escala_vigente` | `data` é parâmetro em `app/ponto/services/declarar_dia.py:56`, repassado por chamadas em `:75,109,182`; faltam as fontes de todos os caminhos. | DUVIDOSO |
| `app/ponto/services/diagnostico_escala.py:53` | `vinculo_cobre_a_data` | `d=ini` em `app/ponto/services/diagnostico_escala.py:148,150`; a API constrói `ini` de `localdate()` ou da data convertida em `app/api/views_mensageria.py:1994,2006,2009`; conversão em `:1726,1731,1759`. | DATA GARANTIDA |
| `app/ponto/services/espelho.py:286` | `vinculo_do_dia` | `d` recebe `data` em `app/ponto/services/espelho.py:675`; esta deriva de `data_ini` em `:653`; `montar_dias` recebe o parâmetro em `:365` e é chamada com `vis_ini` em `:1262`; fonte completa de `vis_ini` não fechada. | DUVIDOSO |
| `app/ponto/services/espelho.py:1065` | `vinculo_do_dia` | `fim` é parâmetro em `app/ponto/services/espelho.py:971`; há chamada em `:1227`, mas as fontes de todos os chamadores não foram fechadas. | DUVIDOSO |
| `app/ponto/services/espelho.py:724` | `vinculo_do_dia` | `data` deriva de `data_ini` em `app/ponto/services/espelho.py:653`; `montar_dias` recebe o parâmetro em `:365` e é chamada com `vis_ini` em `:1262`; fonte completa não fechada. | DUVIDOSO |
| `app/ponto/services/fechamento.py:125` | `vinculo_do_dia` | `_d=fim` em `app/ponto/services/fechamento.py:123`; chamador passa `fim.date()` em `:851`. | DATA GARANTIDA |
| `app/ponto/services/fechamento.py:191` | `vinculo_do_dia` | `_d` é chave do mapa em `app/ponto/services/fechamento.py:189`; mapa formado de `c.data` em `:605` ou pelo carregador em `app/escala/alimentacao.py:67,73`; campo em `app/escala/models.py:1685`; outros chamadores em `app/ponto/services/espelho.py:1049,1164` e `app/relatorios/pdf_espelho.py:354,419`. | DATA GARANTIDA |
| `app/ponto/services/fechamento.py:948` | `escala_vigente` | `timezone.localtime(_p_f.entrada).date()` em `app/ponto/services/fechamento.py:947`. | DATA GARANTIDA |
| `app/ponto/services/fechamento.py:229` | `vinculo_cobre_a_data` | `_d=fatia_ini` em `app/ponto/services/fechamento.py:225`; `fatia_ini` inclui parâmetro `ini` em `:132,206`. Um chamador passa `.date()` em `:754`; fontes dos outros, em `app/ponto/services/espelho.py:1164` e `app/relatorios/pdf_espelho.py:419`, não foram fechadas. | DUVIDOSO |
| `app/ponto/services/flip_auto.py:66` | `vinculo_do_dia` | `dia=timezone.localtime(ts_b).date()` em `app/ponto/services/flip_auto.py:62,63`. A chamada efetiva é `vinculo_do_dia`, embora o lote a rotule `escala_vigente` (`:66`). | DATA GARANTIDA |
| `app/ponto/services/furos_vetados.py:24` | `escala_vigente` | `data` é parâmetro em `app/ponto/services/furos_vetados.py:18`; um caminho usa `cel.data` em `:64`, outro usa `dia_do_fato` em `:101,106`; falta fechar esta última fonte. | DUVIDOSO |
| `app/ponto/services/veredito_celula.py:41` | `escala_vigente` | `data` é parâmetro em `app/ponto/services/veredito_celula.py:22`; repasses em `:211,227` e `app/ponto/services/flip_auto.py:201,215,293`; faltam as fontes de todos os chamadores. | DUVIDOSO |
| `app/ponto/services/vigia_cadastro_futuro.py:58` | `vinculo_do_dia` | `cel.data` da consulta em `app/ponto/services/vigia_cadastro_futuro.py:25,31,38,43`; campo em `app/escala/models.py:1685`. | DATA GARANTIDA |
| `app/ponto/services/vigia_de_hora.py:47` | `escala_vigente` | `cel.data` de `CelulaDia` em `app/ponto/services/vigia_de_hora.py:117,118`; campo em `app/escala/models.py:1685`. | DATA GARANTIDA |
| `app/ponto/services/vigia_de_hora.py:74` | `escala_vigente` | `hoje` recebe `cel.data` na chamada em `app/ponto/services/vigia_de_hora.py:130`; campo em `app/escala/models.py:1685`. | DATA GARANTIDA |
| `app/ponto/supra_juiz.py:174` | `vinculo_do_dia` | `data` é parâmetro da closure em `app/ponto/supra_juiz.py:173`; um caminho vem de `cel.data` em `app/ponto/services/cartorio.py:1353,1475`; outro lê `dia.get('data')` em `app/ponto/management/commands/emitir_furo_retroativo.py:90,110`; origem completa da grade não fechada. | DUVIDOSO |
| `app/ponto/turnos.py:2230` | `escala_vigente` | `hoje=timezone.localdate(agora)` em `app/ponto/turnos.py:2227`. | DATA GARANTIDA |
| `app/ponto/turnos.py:788` | `escala_vigente` | `data` é parâmetro em `app/ponto/turnos.py:774`; fontes dos diversos chamadores não foram fechadas. | DUVIDOSO |
| `app/ponto/turnos.py:1965` | `vinculo_do_dia` | `_d=ini` em `app/ponto/turnos.py:1963`; há aritmética prévia sobre a janela em `:1934` e `:1847`; fontes de `ini` dos chamadores em `:1920,3048` não foram fechadas. | DUVIDOSO |
| `app/ponto/turnos.py:2105` | `escala_vigente` | `data` é parâmetro em `app/ponto/turnos.py:2098`; um chamador passa `p.data_evento` em `app/chamados/reconciliador.py:1091`, campo em `app/chamados/models.py:2528`; demais fontes não fechadas. | DUVIDOSO |
| `app/ponto/turnos.py:2373` | `vinculo_do_dia` | `hoje=timezone.localdate(agora)` em `app/ponto/turnos.py:2342`. | DATA GARANTIDA |
| `app/ponto/turnos.py:3070` | `vinculo_do_dia` | `data` é parâmetro em `app/ponto/turnos.py:3065`; exemplos de chamadas em `app/escala/servico_jornada.py:363` e `app/chamados/juizes.py:322`; fontes de todos os caminhos não fechadas. | DUVIDOSO |
| `app/ponto/turnos.py:2178` | `vinculo_do_dia` | `d` parte de `ini` em `app/ponto/turnos.py:2176`; chamadores em `:2161,3057`; fonte de todos os valores de `ini` não fechada. | DUVIDOSO |
| `app/relatorios/dia_da_frota.py:42` | `escala_vigente` | API lê GET em `app/api/views_mensageria.py:1820` e chama o relatório em `:1828`; `_data_pedida` converte ISO ou constrói `date` em `:1726,1731,1748,1759,1764`. | DATA GARANTIDA |
| `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:115` | `vinculo_do_dia` | `d=ini+timedelta` em `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:109`; `ini` vem de `janela_fechamento` em `:94`, construída com `datetime.date` em `app/ponto/janelas.py:16,54`. | DATA GARANTIDA |
| `app/relatorios/pdf_espelho.py:571` | `vinculo_do_dia` | `d` é parâmetro da closure em `app/relatorios/pdf_espelho.py:565`; não foi localizada chamada da closure no arquivo, portanto a fonte de `d` não foi identificada. | DUVIDOSO |

Nenhuma linha deste lote chama `q_vinculo_cobre_a_data`; a função monta lookups `DateField` em `app/escala/alimentacao.py:219,223`. O lookup Django aceita e coage uma string ISO válida **[não verificado na cópia]**. A comparação Python relevante para a classe está em `app/escala/alimentacao.py:214`.

**LOTE 2: 39 linhas, DATA GARANTIDA 20, TEXTO POSSIVEL 0, DUVIDOSO 19.**

## 17/11 20:20 · sessao 549f10d565d5 · CURA-DE-FASE · censo · origem=Code

O sítio confirmado com texto é o da cura; esse valor também pode alcançar as duas chamadas internas de `escala_vigente` quando há vínculos candidatos (`app/ponto/management/commands/censo_fase_12x36.py:224`, `app/ponto/management/commands/censo_fase_12x36.py:413`, `app/escala/servico_jornada.py:63`, `app/escala/servico_jornada.py:66`).

Para encurtar as referências na tabela, **C** significa `app/ponto/management/commands/`. Referências `:linha` na coluna fonte pertencem ao arquivo da própria linha.

| arquivo:linha | alvo | fonte | veredito |
|---|---|---|---|
| app/escala/services/semana_do_posto.py:75 | q_vinculo_cobre_a_data | `seg` vem de `limites_da_semana(inicio)` e recebe `timedelta` antes da chamada (:51, :55, :75, :132, :134). | DATA GARANTIDA |
| app/escala/servico_jornada.py:66 | vinculo_do_dia | Parâmetro `data` (:15); `_curar` passa `x['virada']`, montada com `str(virada)` (C`censo_fase_12x36.py:224`, :413). | TEXTO POSSIVEL |
| app/escala/servico_jornada.py:544 | escala_vigente | Parâmetro `data_evento` de `procedencia_fina` (:513); origem de todos os chamadores não estabelecida [não verificado]. | DUVIDOSO |
| app/escala/servico_jornada.py:63 | vinculo_do_dia | Parâmetro `data` (:15); `_curar` passa `x['virada']`, montada com `str(virada)` (C`censo_fase_12x36.py:224`, :413); este ramo depende de célula geradora. | TEXTO POSSIVEL |
| app/escala/servico_jornada.py:233 | escala_vigente | `data_evento` é parâmetro (:220); um chamador repassa o parâmetro de `classificar_falta` (:280, :364), cuja origem completa falta [não verificado]. | DUVIDOSO |
| app/escala/servico_jornada.py:362 | escala_vigente | `data_evento` é parâmetro (:280); `motivo_da_lei` repassa seu próprio `data` (:558, :566); fontes completas faltam [não verificado]. | DUVIDOSO |
| app/escala/utils.py:2194 | vinculo_do_dia | `data` é parâmetro (:2142), repassado por `intervalo_declarado_do_dia` e `minutos_previstos_do_dia` (:2197, :2212, :2230, :2260); nem todas as fontes foram estabelecidas [não verificado]. | DUVIDOSO |
| app/escala/utils.py:988 | vinculo_do_dia | `d` vem de `ref - timedelta` (:979, :987, :989, :992); `ref` ausente usa `localdate`. | DATA GARANTIDA |
| app/escala/utils.py:1311 | vinculo_do_dia | `d` vem de `ref - timedelta` (:1290, :1306, :1313, :1316); o default de `ref` usa `.date()`. | DATA GARANTIDA |
| app/folha/export.py:1255 | vinculo_cobre_a_data | `dia` ← `d` ← parâmetro `ini` de `_dias_sem_decisao` (:1231, :1254, :1264, :1267); fontes completas de `ini` não estabelecidas [não verificado]. | DUVIDOSO |
| app/folha/previa.py:81 | vinculo_cobre_a_data | `d = cel.data` (:78); `CelulaDia.data` é `DateField` (app/escala/models.py:1635, :1685). | DATA GARANTIDA |
| app/ponto/calculador/alimentacao.py:196 | vinculo_do_dia | `_dia` vem dos valores de `resultado.dia_da_jornada` (:193, :195); o motor mescla mapas (app/ponto/motor_calculo_v2.py:1073), mas todas as origens e todos os resultados aceitos não foram fechados [não verificado]. | DUVIDOSO |
| app/ponto/calculador/alimentacao.py:432 | vinculo_do_dia | `_dia` ← `k` (:492); `k` converte strings com `date.fromisoformat` e os demais valores passam pela comparação com os limites (:473, :474). A origem dos valores não string como datas é [não verificado]. | DUVIDOSO |
| C`aplicar_col900.py:229` | vinculo_do_dia | `dia = date.fromisoformat(it['dia'])` (:226). | DATA GARANTIDA |
| C`aplicar_col900.py:176` | vinculo_do_dia | `dia` vem de `dias`, cujos itens são convertidos com `date.fromisoformat` (:159, :172). | DATA GARANTIDA |
| C`ausencias_sem_efeito_na_celula.py:55` | q_vinculo_cobre_a_data | `d` é chave de `cobertura_ausencia_periodo` (:44, :48); a cobertura constrói as chaves por aritmética de datas (app/ponto/turnos.py:2777, :2790, :2794). | DATA GARANTIDA |
| C`censo_col900.py:111` | vinculo_do_dia | `dia` é chave construída de `CelulaDia.data` (:104, :110; app/escala/models.py:1685). | DATA GARANTIDA |
| C`censo_fase_12x36.py:413` | escala_vigente | `x['virada']` é montada como `str(virada)` (:224), repassada sem conversão (:413). | TEXTO POSSIVEL |
| C`censo_fase_12x36.py:161` | vinculo_do_dia | `_d` vem de `dias`, construída por `ini + timedelta` e `(fim - ini).days` (:129, :134, :160). | DATA GARANTIDA |
| C`curar_residuo_bug115.py:85` | escala_vigente | `d` vem de `dias_do_fato` (:77, :80); os ramos incluem data da célula, JSON convertido com `fromisoformat` e `localdate` (app/chamados/juizes.py:188, :194, :204); demais ramos [não verificado]. | DUVIDOSO |
| C`detectar_ausencias.py:140` | escala_vigente | `data_ref` vem de `hoje` ou `ontem` (:137, :139); `hoje = agora.date()` (:67, :71, :72). | DATA GARANTIDA |
| C`diff_calculador.py:387` | vinculo_do_dia | `_dia` ← `k` na chamada (:479); `k = date.fromisoformat(k_str)` (:428, :429). | DATA GARANTIDA |
| C`diff_col900.py:69` | vinculo_do_dia | `dia = date.fromisoformat(it['dia'])` (:66). | DATA GARANTIDA |
| C`dna_x_batida_real.py:77` | vinculo_do_dia | `d` é chave de `cels`, construída com `CelulaDia.data` (:64, :76; app/escala/models.py:1685). | DATA GARANTIDA |
| C`emitir_furo_retroativo.py:130` | escala_vigente | `data` vem de `grade_da_celula` (:60, :90, :91); o leitor usa `_cur = fim` como `data` e decrementa um dia (app/escala/services/leitor_celula.py:376, :390, :444); `fim` é convertido com `fromisoformat` (:41). | DATA GARANTIDA |
| C`emitir_turno_aberto_retroativo.py:104` | escala_vigente | `data` vem de `t.data_turno` (:88, :89), produzido pelo pareamento (app/ponto/turnos.py:318); `_data_do_turno` declara retorno `date` (app/ponto/turnos.py:248). | DATA GARANTIDA |
| C`gerar_celulas.py:287` | vinculo_cobre_a_data | `d` é parâmetro de `_outro_vinculo_cobre` (:269), chamado com o `d` da regeneração (app/ponto/portas/celula.py:227, :254); origem de `desde` para todos os chamadores falta [não verificado]. | DUVIDOSO |
| C`processar_alertas_avancados.py:37` | escala_vigente | `ontem` deriva de `agora.date()` (:20, :21, :22). | DATA GARANTIDA |
| C`processar_alertas_turno.py:134` | escala_vigente | `tm` vem de `TurnoMaterializado.objects` (:76, :81); `data_turno` é `DateField` (app/ponto/models.py:1208, :1215). | DATA GARANTIDA |
| C`processar_alertas_turno.py:106` | escala_vigente | Mesmo `tm` do queryset (:76, :81); `TurnoMaterializado.data_turno` é `DateField` (app/ponto/models.py:1215). | DATA GARANTIDA |
| C`reconciliar_grade.py:157` | escala_vigente | `dt = self._dt(..., ref)` (:133); `_dt` retorna `datetime.date(...)` ou `ref - timedelta` (:349, :356, :359); `ref` vem de `fromisoformat` ou `.date()` (:78). | DATA GARANTIDA |
| C`reconciliar_grade.py:196` | vinculo_do_dia | `_d_rg = win0` e avança por `timedelta` (:194, :197); `win0` deriva de `ref` convertido ou `.date()` (:78, :79). | DATA GARANTIDA |
| C`rejulgar_ausencia_em_folga.py:65` | escala_vigente | `d` é chave de `folgas_lancadas_do_periodo` (:64); construída de `FolgaDia.data` (app/ponto/precedencia.py:204, :208; app/escala/models.py:1582, :1588). | DATA GARANTIDA |
| C`tripwire_celulas.py:107` | vinculo_do_dia | `cel.data` é `CelulaDia.data`, `DateField` (app/escala/models.py:1635, :1685). | DATA GARANTIDA |
| C`vigia_plantio_overnight.py:42` | escala_vigente | `t` vem de `turnos_do_colab` (:38); o pareador atribui `data_turno` por `_data_do_turno` (app/ponto/turnos.py:318), cujo retorno declarado é `date` (app/ponto/turnos.py:248). | DATA GARANTIDA |
| app/ponto/nucleo.py:26 | vinculo_cobre_a_data | `d` é parâmetro (:18); `dt_fim_previsto_de` repassa `data_turno` (:31, :36), inclusive a partir de turno (:55); todas as origens desse parâmetro não foram fechadas [não verificado]. | DUVIDOSO |
| app/ponto/portas/celula.py:229 | vinculo_cobre_a_data | `d = desde` (:227); `desde` é parâmetro da porta (:112), sem conversão ali; todas as fontes faltam [não verificado]. | DUVIDOSO |
| app/ponto/portas/celula.py:906 | vinculo_cobre_a_data | `d` vem de `dias`, construído com `ini + timedelta` e `(fim - ini).days` (:886, :901). | DATA GARANTIDA |
| app/ponto/portas/celula.py:339 | vinculo_cobre_a_data | `_d` vem de `rejulgar` (:337), mas só alcança a chamada se pertencer ao conjunto de `CelulaDia.data` consultado no banco (:335, :336, :338; app/escala/models.py:1685). | DATA GARANTIDA |
| app/ponto/portas/celula.py:909 | vinculo_do_dia | Mesmo `d` da lista construída por aritmética de datas (:886, :901). | DATA GARANTIDA |

Sobre os dois sítios `q_vinculo_cobre_a_data`: o `Q` monta lookups de `DateField` (`app/escala/alimentacao.py:219`, :223, :224; `app/escala/models.py:875`, :876). O Django normalmente coage uma string ISO válida nesses lookups; a implementação do Django nesta cópia não foi verificada, portanto essa afirmação é **[não verificado]**. Já a juíza Python compara diretamente `ec.data_inicio <= data` (`app/escala/alimentacao.py:214`).

**LOTE 1: 40 linhas, DATA GARANTIDA 26, TEXTO POSSIVEL 3, DUVIDOSO 11.**

## 17/11 20:20 · sessao dc5b573b873d · - · - · origem=Ronald (tmux)

Rastreamento das linhas 28 a 40 enviado ao agente principal, com as cadeias de data e os pontos que permanecem duvidosos.

## 17/11 20:19 · sessao 286e59cd4048 · - · - · origem=Ronald (tmux)

Resultados dos 13 sítios enviados ao agente principal, com as cadeias verificadas e os pontos que permanecem duvidosos.

## 17/11 20:19 · sessao f878465dd084 · - · - · origem=Ronald (tmux)

Investigação das 13 chamadas atribuídas concluída e enviada ao agente principal, com as cadeias verificadas e as fontes que permanecem duvidosas. [não verificado]

## 17/11 20:19 · sessao af38b8160005 · - · - · origem=Ronald (tmux)

Resultados e referências das linhas 15 a 27 foram enviados ao agente principal.

## 17/11 20:19 · sessao 9bbd47c9c14d · - · - · origem=Ronald (tmux)

Rastreamento enviado ao agente principal, com fontes, vereditos e as incertezas identificadas.

## 17/11 20:19 · sessao 89947f98b6af · - · - · origem=Ronald (tmux)

Resultado dos 13 sítios enviado ao agente principal, com as cadeias verificadas e as lacunas identificadas.

## 17/11 20:18 · sessao 623b984d2c9f · PORTA-DO-EXPORT-SEM-BECO · verificador · origem=Code

**PROCEDE: NÃO.** O selo ainda permite que o ESTADO publique um número diferente do devolvido pelo leitor único: ele compara a saída do ESTADO com a frase produzida pelo mesmo medidor, e só testa a frase da porta do export com o valor sintético `2`. [bin/tests/test_recusa_estado.sh:81](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:81), [bin/tests/test_recusa_estado.sh:93](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:93)

1. **BLOQUEIA 1 continua aberto.** Um mutante concreto é inserir, em `linha_porta_export`, depois do retorno para `sem_arquivo` e antes do retorno que formata a frase: [bin/recusa_estado.py:71](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/recusa_estado.py:71)

   ```python
   if d['recusas'] != 2:
       d = dict(d, recusas=d['recusas'] + 1)
   ```

   Para qualquer contagem diferente de `2`, a frase passa a mostrar uma unidade a mais que `porta_export_falhou_hoje` devolveu. A AST nova examina os retornos de `ler` e `ler_porta_export` e proíbe certos nomes, mas não verifica essa alteração em `linha_porta_export`; o caso sintético com `2` permanece igual e a comparação do ESTADO usa a mesma função alterada dos dois lados. [bin/recusa_estado.py:68](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/recusa_estado.py:68), [bin/tests/test_recusa_estado.sh:37](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:37), [bin/tests/test_recusa_estado.sh:83](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:83), [bin/tests/test_recusa_estado.sh:90](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:90)

   A passagem é uma **conclusão estática**, condicionada às demais etapas do selo passarem e às duas leituras da raiz permanecerem coerentes; não executei o mutante nem o selo, pois o próprio selo cria e remove arquivos temporários. Resultado executado: **[não verificado]**. [bin/tests/test_recusa_estado.sh:59](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:59)

2. **BLOQUEIA 2: há registro de vermelho, mas não prova completa das mutações.** O anexo fornecido registra, para M1–M5, nome, arquivo, `diff stat`, mensagem e `rc=1`; isso informa o motivo *reportado* pelo selo para cada execução. Porém, o `diff stat` não mostra as linhas alteradas: não comprova que cada mutação corresponde ao nome atribuído, nem permite avaliar se houve outra alteração com o mesmo tamanho. A autenticidade e a reprodução do log são **[não verificado]**. [anexo `instr_v5_mutantes.log`, fornecido na pergunta: não verificado]

3. **Quem escreve e quem lê os valores:**

   | Valor ou sinal | Quem escreve ou produz | Quem lê |
   |---|---|---|
   | Linhas sintéticas do teste | O próprio selo cria o `ponto.log` temporário e escreve as portas e datas de teste. [selo:70](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:70) | `ler` e `ler_porta_export`; o selo verifica ausência, contagens, esperado e frase. [selo:68](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:68), [selo:81](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:81) |
   | Recusas do Resolver dia | Os envelopes chamam `registrar`, que emite a linha no logger; a configuração SaaS encaminha `ponto` ao arquivo. [respostas.py:54](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/core/respostas.py:54), [respostas.py:78](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/core/respostas.py:78), [recusa_critica.py:70](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/core/recusa_critica.py:70), [saas.py:160](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/config/settings/saas.py:160), [saas.py:181](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/config/settings/saas.py:181) | `contar` lê o log e produz `recusas` e `por_usuario`; `resolver_dia_recusado` acrescenta o rótulo; o medidor e o ESTADO consomem o resultado. [recusa_critica.py:95](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/core/recusa_critica.py:95), [recusa_critica.py:108](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/core/recusa_critica.py:108), [recusa_critica.py:126](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/core/recusa_critica.py:126), [gerar_estado.py:265](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/gerar_estado.py:265) |
   | Falhas ou divergências da porta do export | `_recusa` chama o mesmo `registrar` com a porta do export. [porta_export.py:1063](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/folha/porta_export.py:1063) | `porta_export_falhou_hoje` conta as linhas e define `esperado = 0`; o medidor formata contagem, esperado e vermelho, e o ESTADO usa a frase. [recusa_critica.py:133](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/core/recusa_critica.py:133), [recusa_estado.py:60](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/recusa_estado.py:60), [gerar_estado.py:274](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/gerar_estado.py:274) |
   | `sem_arquivo`, data e frases do ESTADO | Os dois métodos do medidor produzem `sem_arquivo` quando falta o arquivo; o gerador fornece uma data e escreve o ESTADO. [recusa_estado.py:37](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/recusa_estado.py:37), [recusa_estado.py:60](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/recusa_estado.py:60), [gerar_estado.py:270](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/gerar_estado.py:270), [gerar_estado.py:399](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/gerar_estado.py:399) | O selo lê os retornos sintéticos e compara as duas frases do medidor com linhas do ESTADO gerado. [selo:83](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:83), [selo:100](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:100) |

4. **FILAs 4–7:** permanecem como riscos já apontados: duas leituras em momentos distintos; erro do gerador ocultado; igualdade dos fusos sustentada no código por comentário; e AST que procura substrings sem impedir reatribuição de `_hoje_recusa`. [selo:90](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:90), [selo:93](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:93), [gerar_estado.py:267](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/gerar_estado.py:267), [selo:30](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:30)

   A mudança não demonstra, por si, que qualquer uma dessas quatro filas tenha virado um **novo** bloqueio. A falha de integridade demonstrada acima mantém aberto o BLOQUEIA 1; a checagem de `: 2**` reduz o caso da antiga FILA 3 apenas para esse exemplo sintético. [selo:83](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:83)

## 17/11 20:18 · sessao 7e1d803ea754 · E4C-F1-B2 · verificador · origem=Code

A tabela ainda precisa de correção: há **7 linhas procedentes entre os 26 sítios pedidos**, além de 6 itens comuns ou seções procedentes. O descarte de B4 resolve a colisão física, mas deixa duas contradições; os descartes de B9a e B9b não demonstram compatibilidade com o aval posterior.

Os caminhos de código abaixo são relativos a `app/chamados/`, salvo indicação diferente. Os avais são os anexos transcritos na solicitação, identificados por item.

| ID | Revisão |
|---|---|
| Cabeçalho | **PROCEDE** — O hash integral da cópia não pôde ser confirmado por Git: a cópia não contém repositório Git [não verificado]. Além disso, o cabeçalho declara zero indecisos enquanto deixa P21 no topo; o aval já manda usar a mesma leitura nos comandos que escrevem por item (anexo 18:45, item 2; `management/commands/reabrir_orfas_enterradas.py:85`, `:157`). |
| F0 | **CONFERE** — A guarda retorna fora da cerca; há quatro chamadas abridoras no código de produção (`services/fio.py:1396-1398`; `reconciliador.py:553`, `:1701`; `services/becos.py:66`; `services/disputa_emissao.py:1909`). |
| F1 | **CONFERE** — Cerca aninhada executa no contexto externo, sem abrir savepoint próprio (`services/fio.py:1433-1434`). |
| F2 | **CONFERE** — O abridor testa a anotação de veto dentro do atomic e devolve a recusa após o rollback (`services/fio.py:1439-1446`). |
| F3 | **CONFERE** — A guarda antecede o save de pergunta existente e o update em lote (`models.py:3574-3577`, `:3400-3407`). |
| F4 | **PROCEDE** — `evento` protege sua própria gravação, mas a consulta da última retenção ocorre antes dele e fora desse tratamento; falha nessa leitura pode subir, contrariando a garantia proposta de que falha da trilha não derruba o ato (`reconciliador.py:489-493`; `app/core/observ.py:94-105`, `:133-136`; anexo 18:45, item 6). |
| F5 | **CONFERE** — O juiz existente retorna `None` para dia indecidível e usa memo; o resumo e a lista são exigências do aval, ainda não uma implementação demonstrada (`services/fio.py:1358-1368`; anexo 17:20, R3). |
| F6 | **CONFERE** — A extração pode preservar o universo se mantiver exatamente dia do turno, dia **gravado**, exclusão de conversa e o `continue` para conjunto vazio; o juiz e a agregação permanecem separados (`services/fio.py:1399-1422`). |
| F7 | **CONFERE** — A regra da validação existe no caminho comum; o retorno antecipado da folga fica como achado (`services/validacao.py:711-712`, `:740-742`; anexo 18:45, item 2(i)). |
| F8 | **PROCEDE** — A lista descrita como deduplicada por `(pk, motivo)` não especifica como preservar **duas entidades de trilha** para o mesmo pk exigidas por B2a/B2b; as chamadas são sequenciais e `ch_novo` aparece depois (`reconciliador.py:977-1001`, `:1664-1666`). O coletor proposto ainda não existe; `_ESPERAS_163` é somente exemplo de contexto (`services/materializacao.py:1726-1734`). |
| F9 | **CONFERE** — O ponto proposto fica depois dos dois retornos sem escrita e antes da lavra e do save (`models.py:3241-3242`, `:3264-3273`). |
| INDECISOS | **PROCEDE** — A9d está decidido, mas P21 não deve permanecer em “manter ou trocar”: a regra posterior inclui comando de sistema e exige a mesma leitura de dias (`management/commands/reabrir_orfas_enterradas.py:85`, `:94-99`, `:157`; `services/fio.py:1403-1413`; anexo 18:45, item 2(ii)). |
| ACHADOS | **CONFERE** — As seis portas ficam como lista sem obra e sem cerca por decisão expressa; a folga passa antes da regra comum (`services/validacao.py:711-712`, `:740-742`; anexo 19:50, item 3). |
| RED-NOVO-1 | **CONFERE** — O gate chama o reaponte e hoje levanta `ValueError` quando ele recusa; o caso adicional precisa distinguir recusa por veto (`models.py:435-439`, `:689`). |
| RED-NOVO-2 | **CONFERE** — O controle deve chegar a `carimbar` e sair sem escrita para testar a posição do julgamento (`services/materializacao.py:2161-2167`; `models.py:3241-3242`, `:3264-3266`). |
| RED-NOVO-3 | **PROCEDE** — O controle especificado como **não ENTREGUE** não demonstra que `julga_antes_de_carimbar` contaria indevidamente uma saída sem escrita: para morder esse mutante, o controle precisa ser vetável e sair por C6 ou fora do fato (`models.py:3241-3242`, `:3264-3266`; `services/fio.py:1418-1419`). |
| DECISÕES | **PROCEDE** — A decisão B4 repete o descarte apesar das contradições de alvo e de dia; P21 também não deve ficar pendente da escolha manter/trocar (`services/fio.py:1403-1411`, `:969-976`; `management/commands/reabrir_orfas_enterradas.py:85`, `:157`; anexo 18:45, item 2(ii)). |
| A1 | **CONFERE** — O save de justificativa e as escritas de pergunta ficam no ato cercado; o resolver aninhado não abre outra unidade (`reconciliador.py:543-580`; `services/fio.py:1433-1446`). |
| A2 | **CONFERE** — Resolver e fecho ficam sob o abridor do beco; o veto aninhado pertence ao abridor externo (`services/becos.py:42-72`; `services/fio.py:1433-1446`). |
| A3 | **CONFERE** — Direto, resolver abre cerca; aninhado, executa na existente (`services/disputa_emissao.py:1902-1914`; `services/fio.py:1433-1434`). |
| A4 | **CONFERE** — A transição alcança o gate de reata e a relavra; fora de cerca o tratamento deve ser por item (`models.py:435-439`, `:642-656`, `:689`, `:307-312`; anexo 18:45, itens 2-3). |
| A5 | **CONFERE** — O laço da relavra chama a escrita por pergunta; dentro da cerca prevalece o veto do abridor (`models.py:307-312`; `services/fio.py:1439-1446`; anexo 18:45, item 3). |
| A6 | **CONFERE** — Encerrar passa pelo gate e pela gravação do estado; os pontos de pergunta são os identificados na tabela (`models.py:1686-1693`, `:435-439`, `:307-312`). |
| A7 | **CONFERE** — O caminho da tranca passa pelo gate e pela relavra, sem abridor próprio (`models.py:1711`, `:307-312`; `services/competencia_trancada.py:207-209`; anexo 18:45, item 3). |
| A8 | **CONFERE** — Filtrar depois do universo e antes de `alvo` também exclui a vetada das decisões, células e disputas derivadas (`models.py:1601-1609`, `:1621`, `:1638-1649`). |
| A9a | **CONFERE** — O fecho por desligamento continua sem cerca; a cadeia S144 posterior é ato de sistema por item (`models.py:2009-2012`; `signals.py:164-167`; anexo 18:45, itens 1-4). |
| A9c | **CONFERE** — Os `continue` do S144 e as duas saídas de `carimbar` antecedem F9 (`services/materializacao.py:2154-2167`; `models.py:3241-3242`, `:3264-3273`). |
| A9d | **CONFERE** — O aval posterior decide disputa fechada e pergunta vetada sem gravação; F9 permite retorno sem exceção (`services/competencia_trancada.py:222-229`; `signals.py:164-167`; `models.py:3264-3273`; anexo 19:50, item 2). |
| B1a | **CONFERE** — `False` sai pelo `continue`, fora de `n`; a trilha sem chamado deve ter a pergunta como alvo (`reconciliador.py:628-645`; anexo 17:50, item 1). |
| B1b | **CONFERE** — O fecho posterior pode disparar S144; seu julgamento fica em F9 (`reconciliador.py:652-660`; `signals.py:164-167`; `models.py:3264-3273`). |
| B2a | **PROCEDE** — A saída sem fecho confere, mas a especificação de duas trilhas para um pk não fecha com o coletor F8 descrito apenas como lista deduplicada (`reconciliador.py:400-415`, `:1664-1666`, `:977-1001`). |
| B2b | **PROCEDE** — `ch_novo` é o chamado da costura e a recusa retorna `False`, mas falta preservar esse segundo alvo sem duplicar a contagem já feita em B2a (`reconciliador.py:977-1001`, `:1664-1666`). |
| B2c | **CONFERE** — Costura e resolver estão no mesmo abridor HIST; a recusa devolve `False` (`reconciliador.py:1690-1705`; `services/fio.py:1433-1446`). |
| B2d | **PROCEDE** — `(False, mot)` é o retorno de `reapontar_resolvedora`; o chamador de B2d o consome e devolve apenas `False`, não o par afirmado na tabela (`reconciliador.py:1712-1716`). |
| B3a | **CONFERE** — `encerrar_fantasma` delega ao carimbo; `False` pula o evento (`models.py:3421-3429`; `reconciliador.py:1191-1193`). |
| B3b | **CONFERE** — O fecho alcança S144 e, nele, F9 depois das saídas sem escrita (`reconciliador.py:1216-1218`; `services/materializacao.py:2154-2167`; `models.py:3264-3273`). |
| B4 | **PROCEDE** — O risco de índice ao datar a principal com perdedora viva está documentado, mas a célula manda trilha ora ao chamado destino, ora sempre à pergunta. Também afirma que a porta lê `data_evento` da instância: ela lê o **gravado** e o dia do turno (`services/fio.py:939-947`, `:969-976`, `:1403-1411`; `reconciliador.py:489-493`). |
| B5 | **CONFERE** — `lavrar` grava por item; o backfill citado usa o escritor em lote, não essa chamada (`veredito_lavrado.py:240-250`, `:254-275`; `management/commands/backfill_veredito_lavrado.py:143-146`). |
| B6 | **CONFERE** — O `bulk_update` usa a lista recebida e não passa pelo `save` de cada pergunta (`veredito_lavrado.py:240-251`; `models.py:3565-3577`). |
| B7 | **CONFERE** — O ponto antes do save fica depois dos dois `continue` do laço (`services/relavra_via.py:117-127`). |
| B8 | **PROCEDE** — O pré-veto existente usa dia gravado e `novo_dia` e devolve recusa antes da porta; a tabela não demonstra a mesma leitura F6 nem o contrato por item que conta e deixa os demais seguirem, inclusive sob cerca (`services/fio.py:1105-1123`, `:1403-1419`; `models.py:3072-3076`; anexo 18:45, item 2(ii)). |
| B9a | **PROCEDE** — O código confirma o aval anterior por grupo, mas o aval posterior inclui backfill e determina julgamento por item; o descarte não demonstra que a regra antiga prevalece nessa colisão (`management/commands/backfill_decisoes_pergunta.py:244-262`; anexo 18:45, item 2(ii)). |
| B9b | **PROCEDE** — Grupo na ida e reversão tudo ou nada existem, mas sua prevalência sobre o comando posterior por item não foi resolvida pelo descarte (`management/commands/migrar_data_evento_dia_do_turno.py:380-393`, `:476-495`; anexo 18:45, item 2(ii)). |

**Quem escreve e quem lê:** `recusa_motivo` é escrito, entre outros pontos, pelo materializador e pela recusa humana, e lido pelo próprio materializador e pela validação (`services/materializacao.py:1676-1690`; `services/validacao.py:262-266`, `:916-922`). Os carimbos `materializada_em`, `materializada_por` e `via_resolucao` são atribuídos pela porta de carimbo e lidos pelo C6 (`models.py:3218-3222`, `:3260-3273`; `juizes.py:1389-1405`). `evento` cria `LogAuditoria`; `registrar_retencao` lê a última linha para deduplicar (`app/core/observ.py:94-105`, `:133-136`; `reconciliador.py:489-493`). `_CERCA` é escrito e restaurado pelo abridor e lido pela guarda (`services/fio.py:1381`, `:1396`, `:1433-1436`, `:1449`). O coletor F8 é proposto, portanto seus escritores e leitores ainda não existem na cópia; `_ESPERAS_163` apenas demonstra a técnica de `ContextVar` (`services/materializacao.py:1726-1734`).

A extração F6 deve preservar a consulta ao **dia gravado**, inclusive quando B4 já atribuiu outro dia em memória; tratar essa atribuição como leitura existente mudaria o predicado. A ativação exclusiva do coletor por sistema e sua ausência em portas humanas são requisitos do desenho, ainda não comportamento implementado (`services/fio.py:917-928`, `:1403-1413`; `services/materializacao.py:1726-1734`).

**PROCEDE: 13** — 7 dos sítios e 6 itens comuns/seções. **LINHAS RESPONDIDAS: 26 de 26.**

## 17/11 20:17 · sessao 015814b2b862 · - · - · origem=Ronald (tmux)

- **B2d — PROCEDE:** `reapontar_resolvedora` devolve `(False, motivo)`, mas `reatar_pergunta_orfa` trata o par e devolve apenas `False` ao seu chamador ([reconciliador.py:1712](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/reconciliador.py:1712)).

- **B2a/B2b e F8 — PROCEDE:** a tabela descreve um coletor de `(pk, motivo)` deduplicado por pk, mas exige duas trilhas com alvos diferentes para a mesma pergunta. As chamadas são sequenciais, e o chamado novo só aparece no segundo caminho; o desenho precisa preservar os dois alvos sem duplicar a contagem ([reconciliador.py:1664](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/reconciliador.py:1664), [reconciliador.py:998](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/reconciliador.py:998)).

- **P21/INDECISOS — PROCEDE:** o aval transcrito exige a mesma leitura de dias para comando de sistema e nenhum segundo leitor; a tabela deixa indeciso se mantém o leitor próprio de P21 ([reabrir_orfas_enterradas.py:85](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/reabrir_orfas_enterradas.py:85), [fio.py:1403](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/services/fio.py:1403)).

- **A9d/F9:** as duas saídas sem escrita precedem a lavra e o save, como descrito; S144 chama `carimbar` depois dos dois `continue` ([models.py:3241](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/models.py:3241), [models.py:3264](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/models.py:3264), [materializacao.py:2154](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/services/materializacao.py:2154)).

## 17/11 20:17 · sessao 3ae0314e0af1 · E4C-F1-B2 · verificador · origem=Code

| id | Revisão |
|---|---|
| P1 | **PROCEDE** — O ramo de dia diferente mantém `ReusoSemRechave`, que desfaz o ato, enquanto o aval vigente determina recusa por item e continuação dos demais. O ramo sem `upd` não escreve. `models.py:3060`, `models.py:3072`, `models.py:3117`. |
| P3 | **CONFERE** — O julgamento precede a decisão e o carimbo; a retirada do motivo e a tentativa de fecho continuam. `models.py:2145`, `models.py:2147`, `models.py:2163`. |
| P4 | **CONFERE** — Os alvos já excluem as vias de saída sem escrita da porta; o segundo laço decide o fecho. `encerrar_perguntas_com_fato.py:31`, `encerrar_perguntas_com_fato.py:65`, `models.py:3415`. |
| P5 | **CONFERE** — A seleção já restringe à via da competência; a recusa por item precede o descarimbo. `competencia_trancada.py:276`, `models.py:3459`. |
| P6 | **CONFERE** — A validação comum tem regra própria; o retorno antecipado da folga permanece no achado X4. `validacao.py:711`, `validacao.py:740`. |
| P7 | **CONFERE** — A devolução está após a segunda verificação de competência da validação humana. `validacao.py:777`, `validacao.py:781`. |
| P11 | **CONFERE** — Há escrita com `_pend` mesmo se `ts` for `None`; sem `_pend`, essa saída não conta por P11. `materializacao.py:1347`, `materializacao.py:1353`. |
| P12 | **CONFERE** — O ponto é o carimbo de identidade efetivo do ramo; a chamada anterior de recusa tem julgamento próprio em P13. `materializacao.py:1450`, `materializacao.py:1461`. |
| P13 | **CONFERE** — Julgar apenas com `_carimba or _muda_motivo` evita contar a chamada que não escreve nem anota. `materializacao.py:1676`, `materializacao.py:1680`, `materializacao.py:1691`. |
| P14 | **CONFERE** — O `save` ocorre somente no ramo `_muda_motivo`; a chamada isolada não conta. `materializacao.py:1688`, `materializacao.py:1690`. |
| P15 | **PROCEDE** — A saída C6/fora do fato não conta **por P15**, mas a tabela afirma que não conta no ato: a reprovação acrescenta erro e o `finally` pode tentar outra escrita em P13; se essa escrita for vetada, ela conta. `models.py:3241`, `models.py:3264`, `materializacao.py:1968`, `materializacao.py:1976`, `materializacao.py:1990`. |
| P17 | **CONFERE** — A linha fora do fato permanece na captura para acordar célula, mas é excluída da decisão e do update; não conta como veto da escrita de pergunta. `disputa_emissao.py:1625`, `disputa_emissao.py:1645`, `disputa_emissao.py:1648`, `models.py:3405`. |
| P18 | **CONFERE** — Resposta automática é ato de sistema; texto presente sai sem escrita. A validação comum barra, mas a folga retorna antes da guarda. `disputa_emissao.py:1663`, `disputa_emissao.py:1677`, `validacao.py:711`, `validacao.py:740`. |
| P19 | **CONFERE** — O ponto proposto vem após o retorno sem escrita e antes da atribuição e do save. `disputa_emissao.py:1692`, `disputa_emissao.py:1696`, `disputa_emissao.py:1698`. |
| P20 | **CONFERE** — O laço relê, passa pelos `continue` e só então tenta descarimbar; as vias de identidade ficam fora da seleção. `relavra_via.py:181`, `relavra_via.py:236`, `relavra_via.py:242`, `relavra_via.py:246`. |
| P21 | **PROCEDE** — O comando usa dia gravado em leitor próprio; a porta usa turno e dia gravado. A tabela corretamente mantém a decisão sobre o leitor existente no topo. `reabrir_orfas_enterradas.py:86`, `reabrir_orfas_enterradas.py:94`, `fio.py:1403`, `fio.py:1409`. |
| X1 | **CONFERE** — Os `continue` e as duas saídas sem escrita de `carimbar` precedem F9; no caminho da tranca, a disputa fecha e a pergunta vetada conta. `materializacao.py:2161`, `materializacao.py:2167`, `models.py:3241`, `models.py:3264`, `competencia_trancada.py:229`. |
| X2 | **CONFERE** — O K6 existente não substitui a leitura comum; o ponto proposto fica após `campos` vazio e antes do save. `validacao.py:338`, `validacao.py:346`, `validacao.py:351`, `fio.py:1409`. |
| X3 | **CONFERE** — O julgamento após o impedimento e antes do primeiro `registrar` evita decisão sem o update correspondente. `validacao.py:432`, `validacao.py:439`, `validacao.py:440`. |
| X5 | **CONFERE** — O laço é de sistema e pode seguir por item; o laço posterior de chamados permanece. `veredito_celula.py:189`, `veredito_celula.py:202`. |
| X6 | **CONFERE** — A saída idempotente precede o carimbo. O descarte B9a tem fundamento no aval por grupo registrado no código. `fio.py:1483`, `fio.py:1492`, `backfill_decisoes_pergunta.py:245`. |
| X7 | **CONFERE** — O reaponte já trata `False` como recusa e segue; as perdedoras passam pela porta própria. `fio.py:2328`, `fio.py:2329`, `fio.py:2334`. |
| X8 | **CONFERE** — Fora de cerca, deixar a vetada sem reata e permitir o fechamento decorre da classe (ii); não há colisão identificada nos avais dados. Outras recusas conservam o `ValueError`. `models.py:410`, `models.py:435`, `models.py:438`. |
| X9 | **CONFERE** — O laço registra o resultado de cada reaponte e já pula a pergunta apontada ao pai. `limbo_folgas.py:239`, `limbo_folgas.py:242`, `limbo_folgas.py:245`. |
| X10 | **CONFERE** — A vetada permanece **sem reata**; se o furo não fecha e outra reatou, o rollback desfaz as reatas e o gate é tentado novamente. A trilha proposta precisa ser drenada fora do savepoint. `limbo_folgas.py:278`, `limbo_folgas.py:283`, `limbo_folgas.py:285`, `limbo_folgas.py:289`. |
| X12 | **CONFERE** — A saída de recusa do reaponte serve ao julgamento por item; o descarte B4 tem fundamento na colisão e no índice, preservando os demais itens do laço. `reatar_beco_folga_ao_limbo.py:135`, `fio.py:939`, `fio.py:944`. |
| X13 | **CONFERE** — O fecho aciona S144 e herda o ponto F9 de X1, após as saídas sem escrita. `materializacao.py:2030`, `signals.py:164`, `models.py:3264`. |
| X14 | **CONFERE** — A emissão chama o fecho, cuja cadeia S144 herda X1. `disputa_emissao.py:1298`, `signals.py:164`, `materializacao.py:2167`. |
| X15 | **CONFERE** — A ordem proposta libera a cerca antes do callback; o descarte B9b está apoiado no grupo da ida e na reversão integral já declarados no código. `fio.py:1438`, `fio.py:1449`, `migrar_data_evento_dia_do_turno.py:382`, `migrar_data_evento_dia_do_turno.py:482`. |

**PROCEDE: 3, LINHAS RESPONDIDAS: 29 de 29.**

## 17/11 20:17 · sessao a06993d1296e · CURA-DE-FASE · censo · origem=Code

O censo ficou **incompleto**: confirmei a cadeia da virada e alguns outros sítios, mas não rastreei a origem de todas as datas nem validei todos os candidatos da varredura. Portanto, os totais pedidos não estão verificados.

| Chamador | Origem da data e veredito |
|---|---|
| [censo_fase_12x36.py:413](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:413) → `escala_vigente` | **TEXTO POSSÍVEL.** `linhas` recebe `virada` por `str(virada)` em [censo_fase_12x36.py:224](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:224); o filtro forma `_auto` em [censo_fase_12x36.py:257](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:257), entregue a `_curar` em [censo_fase_12x36.py:268](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:268). |
| [servico_jornada.py:66](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/servico_jornada.py:66) → `vinculo_do_dia` | **TEXTO POSSÍVEL.** Repassa o parâmetro `data` recebido em [servico_jornada.py:15](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/servico_jornada.py:15), inclusive pela cadeia do censo acima. |
| [alimentacao.py:285](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/alimentacao.py:285) → `vinculo_cobre_a_data` | **TEXTO POSSÍVEL.** Repassa o parâmetro de [alimentacao.py:228](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/alimentacao.py:228), alcançado pela cadeia anterior; a comparação com `data_inicio` está em [alimentacao.py:214](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/alimentacao.py:214). |
| [aplicar_col900.py:176](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/aplicar_col900.py:176) → `vinculo_do_dia` | **DATA GARANTIDA.** `dias` é construído com `date.fromisoformat` em [aplicar_col900.py:159](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/aplicar_col900.py:159) e percorrido em [aplicar_col900.py:172](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/aplicar_col900.py:172). |
| [aplicar_col900.py:229](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/aplicar_col900.py:229) → `vinculo_do_dia` | **DATA GARANTIDA.** O valor de `it['dia']` passa por `date.fromisoformat` em [aplicar_col900.py:226](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/aplicar_col900.py:226). |
| [diff_col900.py:69](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/diff_col900.py:69) → `vinculo_do_dia` | **DATA GARANTIDA.** `it['dia']` passa por `date.fromisoformat` em [diff_col900.py:66](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/diff_col900.py:66). |
| [alimentacao.py:196](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/calculador/alimentacao.py:196) → `vinculo_do_dia` | **DUVIDOSO.** `_dia` vem dos valores de `resultado.dia_da_jornada` em [alimentacao.py:193](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/calculador/alimentacao.py:193). Falta rastrear quem preenche esse mapa, inclusive no caminho da lavra. [não verificado] |
| [fechamento.py:191](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/fechamento.py:191) → `vinculo_do_dia` | **DUVIDOSO.** `_d` vem da chave do mapa `celulas` em [fechamento.py:189](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/fechamento.py:189). Falta rastrear a construção do mapa em todos os chamadores. [não verificado] |

**TEXTO POSSÍVEL: [não verificado]** — há três sítios confirmados na tabela, contando separadamente o repasse entre funções.

**DUVIDOSO: [não verificado]** — há pelo menos os dois sítios apresentados; faltam os demais candidatos.

A varredura abaixo retornou **119 candidatos fora de testes e 54 em testes**. Ela reconhece imports locais, aliases e chamadas por atributo, mas é uma busca de candidatos: não resolve escopos, sombreamento ou proveniência entre funções. Assim, esses números **não são a contagem final de chamadores nem a contagem validada de testes**. [não verificado] [nao verificado]

```python
import ast
import pathlib
import collections

names = {
    "escala_vigente",
    "vinculo_do_dia",
    "vinculo_cobre_a_data",
    "q_vinculo_cobre_a_data",
}
counts = collections.Counter()

for p in sorted(pathlib.Path("app").rglob("*.py")):
    try:
        t = ast.parse(p.read_bytes())
    except (SyntaxError, ValueError):
        continue

    aliases = {n: n for n in names}
    for x in ast.walk(t):
        if (
            isinstance(x, ast.ImportFrom)
            and (x.module or "").endswith(("servico_jornada", "alimentacao"))
        ):
            for a in x.names:
                if a.name in names:
                    aliases[a.asname or a.name] = a.name

    parent = {
        child: node
        for node in ast.walk(t)
        for child in ast.iter_child_nodes(node)
    }

    for x in ast.walk(t):
        if not isinstance(x, ast.Call):
            continue

        f = x.func
        target = None
        if isinstance(f, ast.Name) and f.id in aliases:
            target = aliases[f.id]
        elif isinstance(f, ast.Attribute) and f.attr in names:
            target = f.attr
        if not target:
            continue

        q = parent.get(x)
        while q and not isinstance(q, (ast.FunctionDef, ast.AsyncFunctionDef)):
            q = parent.get(q)

        test = (
            any(part in ("tests", "test") for part in p.parts)
            or p.name.startswith("test_")
        )
        counts["test" if test else "main"] += 1

        if not test:
            function = q.name if q else "<module>"
            print(f"{p}:{x.lineno} {function} {target} {ast.unparse(x)}")

print("COUNTS", dict(counts))
```

## 17/11 20:17 · sessao 9ce9c6b0f615 · - · - · origem=Ronald (tmux)

Pontos enviados ao agente principal:

- F6/B4: a porta lê `data_evento` gravado, não o valor novo em memória; a proposta de B4 precisa preservar essa leitura literal ([fio.py:1403](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/services/fio.py:1403), [fio.py:916](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/services/fio.py:916)).
- F4: a gravação por `evento` é protegida, mas a consulta anterior de deduplicação não está dentro desse tratamento ([reconciliador.py:489](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/reconciliador.py:489), [observ.py:105](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/core/observ.py:105)).
- F9: as duas saídas sem escrita precedem a lavra e o save ([models.py:3241](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/models.py:3241), [models.py:3264](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/models.py:3264)).
- O coletor é proposto; `_ESPERAS_163` é uma referência de forma, já existente ([materializacao.py:1728](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/services/materializacao.py:1728)).

## 17/11 20:17 · sessao 8f35ea13b72c · - · - · origem=Ronald (tmux)

- **B4 — PROCEDE:** a célula atribui a trilha ao chamado de destino, quando houver, e também manda usar sempre `PerguntaDisputa`; as duas instruções divergem. O código atual registra a retenção no chamado ([reconciliador.py:489](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/reconciliador.py:489)). A célula também diz que a guarda lê `data_evento` da instância, mas ela lê o dia do turno da instância e o dia gravado ([fio.py:1403](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/services/fio.py:1403)). O risco de colisão ao datar a principal com perdedora viva está documentado em [fio.py:939](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/services/fio.py:939).

- **B9a — PROCEDE:** o código confirma o aval anterior por grupo ([backfill_decisoes_pergunta.py:245](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/backfill_decisoes_pergunta.py:245)), mas o aval transcrito das 18:45 inclui backfill e determina julgamento por item, com os demais seguindo. A prevalência do aval anterior, afirmada no descarte, não está demonstrada pelo dado fornecido [não verificado].

- **B9b — PROCEDE:** o código confirma a ida por grupo e a reversão tudo ou nada ([migrar_data_evento_dia_do_turno.py:380](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:380), [migrar_data_evento_dia_do_turno.py:482](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:482)). A compatibilidade dessas unidades com o comando posterior de julgamento por item não está resolvida no dado fornecido [não verificado].

_6720 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
