# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 09:19** · publicado em **17/11/2026 09:19** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 09:19 · sessao c5a0e7256b78 · ENSAIO-DA-FOLHA-11 · verificador · origem=Code

**AVAL-DEVOLVIDO — itens 1 e 4:** o caminho do lote existe e o defeito do export confere, mas faltam fechar a execução completa de B1 e as condições de abertura da obra lateral. (`app/relatorios/lote_espelho.py:132`; `bin/sombra.sh:490`; `app/folha/export.py:872`; `app/docs/BACKLOG.md:2088`.)

*Referências a `respostas_1711_0915.txt` são ao anexo colado, contando “aval Ronald” como linha 1.*

**Item 1 — B1 na sombra**

1. **As premissas conferem parcialmente.** A view chama `pedir` e `disparar`; este lança o command, que chama `executar` e o mesmo gerador de PDF do lote. O command exige lote previamente criado; não cria o pedido sozinho. (`app/relatorios/views.py:1227`; `app/relatorios/views.py:1246`; `app/relatorios/lote_espelho.py:127`; `app/relatorios/management/commands/gerar_cartoes_em_lote.py:44`; `app/relatorios/management/commands/gerar_cartoes_em_lote.py:57`; `app/relatorios/lote_espelho.py:205`.)

   **O filho herda as settings da sombra, por inferência do código:** `Popen` não fornece `env`; o container define `DJANGO_SETTINGS_MODULE=config.settings.sombra`; `manage.py` preserva esse valor com `setdefault`. Essas settings fixam banco `sombra`, host `sombra_db`; o subprocesso recebe o schema da conexão do pai. Configuração efetivamente usada numa execução: **[nao verificado]**. (`app/relatorios/lote_espelho.py:126`; `app/relatorios/lote_espelho.py:132`; `bin/sombra.sh:184`; `app/manage.py:9`; `app/config/settings/sombra.py:29`; `app/config/settings/sombra.py:47`; `app/config/settings/sombra.py:49`.)

2. **O novo aval resolve os pontos de autorização anteriores no escopo declarado.** Autoriza função/command sem POST, trava inteira, saída por `/sombra` e as duas operações da lavra. A exigência específica de smoke HTTP em banco `juliani_ref_*` não se aplica à chamada direta proposta; continuam valendo banco da sombra, CPUSET de teste e escopo literal. (`respostas_1711_0915.txt:2`, anexo; `CLAUDE.md:205`; `CLAUDE.md:206`; `CLAUDE.md:87`; `CLAUDE.md:37`.) [nao verificado]

3. **A espera e a retirada do PDF ainda não fecham automaticamente.**

   - **`--rodar` não espera especificamente o filho do `Popen`.** `disparar` abre uma nova sessão e devolve apenas o PID, sem `wait`; `--rodar` executa o script principal dentro de `sh -c`. Não há nesse caminho uma espera adicional pelo job destacado. `start_new_session=True` não demonstra sobrevivência ao encerramento do container `--rm`. (`app/relatorios/lote_espelho.py:132`; `app/relatorios/lote_espelho.py:146`; `bin/sombra.sh:183`; `bin/sombra.sh:191`; `bin/sombra.sh:490`.)
   - **Como segurar a trava inteira:** usar o próprio `--rodar`, que adquire o `flock` antes da lavra, e manter o script em primeiro plano até terminar geração, conferência e retirada do PDF. O descritor 8 permanece aberto no processo do host durante essa chamada. O caminho concreto mais direto é `pedir` seguido de chamada síncrona ao command, com `schema='juliani'`, em vez de destacar o job. Isso é uma receita possível pelo código; script B1 que a implemente: **[nao verificado]**. (`bin/sombra.sh:487`; `bin/sombra.sh:489`; `bin/sombra.sh:490`; `bin/sombra_trava.sh:26`; `bin/sombra_trava.sh:28`; `app/relatorios/management/commands/gerar_cartoes_em_lote.py:37`; `app/relatorios/management/commands/gerar_cartoes_em_lote.py:57`.)
   - **O PDF não sai automaticamente por `/sombra`.** O gerador grava em `MEDIA_ROOT/cartoes_lote`, dentro do tmpfs. É preciso copiar o arquivo para `/sombra` antes de encerrar o container e depois encaminhá-lo à pasta do ensaio; `/sombra` corresponde a `logs/sombra` do host. (`app/config/settings/base.py:102`; `app/relatorios/models.py:125`; `app/relatorios/lote_espelho.py:250`; `bin/sombra.sh:81`; `bin/sombra.sh:189`; `bin/sombra.sh:190`.)
   - **Conclusão do lote não prova B1 auditável:** o informacional também incrementa `n_ok`, e o lote pode terminar `CONCLUIDO`. Além disso, `--rodar` termina com um `echo` depois da pipeline, sem propagar explicitamente seu resultado; o retorno da porta sozinho não certifica sucesso. (`app/relatorios/services.py:244`; `app/relatorios/services.py:252`; `app/relatorios/lote_espelho.py:251`; `bin/sombra.sh:45`; `bin/sombra.sh:492`; `bin/sombra.sh:493`.)

4. **Há escritas, agora autorizadas no banco sombra, e artefatos a declarar.** A lavra faz dois `update_or_create`, podendo alterar snapshots existentes, e escreve `lavra_prod.json` e `repor_lavra.py`; a porta também escreve `logs/<rotulo>.out`. Não envolver `--rodar` em `--com-a-sombra`: ambas chamam a lavra, duplicando as operações aceitas. (`bin/sombra.sh:144`; `bin/sombra.sh:151`; `bin/sombra.sh:157`; `bin/sombra.sh:160`; `bin/sombra.sh:460`; `bin/sombra.sh:489`; `bin/sombra.sh:492`; `respostas_1711_0915.txt:2`, anexo.)

   O caminho registra lote e auditoria de PDF; não demonstra alteração de dinheiro ou de exportada em produção. Efeito real de uma execução: **[nao verificado]**. A raspagem continua necessária porque o PDF imprime nome e CPF; exportar dado pessoal segue proibido. (`app/relatorios/lote_espelho.py:103`; `app/relatorios/pdf_base.py:152`; `app/relatorios/pdf_espelho.py:739`; `CLAUDE.md:206`; `CLAUDE.md:265`.)

**Item 4 — EXPORT-UM-JUIZ-DO-CODIGO**

1. **O defeito confere como possibilidade do código.** O classificador começa com `entra`, normaliza com `strip` e, nesse crivo, exclui apenas código vazio. A montagem preenche com zeros até dez posições; o validador exige linha de 43 caracteres e campo `matric` numérico. Se os demais crivos permitirem entrada e houver evento gerado, um código não numérico pode provocar recusa do TXT inteiro. Caso real de col1041 e dimensão na frota: **[nao verificado]**. (`app/folha/export.py:840`; `app/folha/export.py:841`; `app/folha/export.py:872`; `app/folha/export.py:728`; `app/folha/export.py:273`; `app/folha/export.py:277`; `app/folha/views.py:275`; `app/folha/views.py:279`.)

   **Precisão para o censo:** esse validador não verifica duplicidade; código numérico curto é preenchido com zeros; vazio já sai pelo classificador. Portanto essas classes não podem ser todas rotuladas como “recusadas pelo layout”. (`app/folha/export.py:266`; `app/folha/export.py:307`; `app/folha/export.py:728`; `app/folha/export.py:872`.)

2. **Registrar e preparar são compatíveis; “juiz único” não autoriza automaticamente registrar outro juiz.** Não encontrei autoridade específica registrada para “código do Domínio válido”; existem `classificar_export` para entrada no TXT e aprovação, também apontado em `JUIZES.md`. Declarar autoridade existente exige medir unicidade, mesmo predicado e universo igual; autoridade nova exige corte. (`app/core/juizes.py:181`; `app/core/juizes.py:182`; `app/core/juizes.py:329`; `app/core/juizes.py:330`; `app/docs/JUIZES.md:118`; `CLAUDE.md:74`; `CLAUDE.md:278`; `CLAUDE.md:314`.)

3. **A abertura futura não fecha apenas com “censo fechado e lei 0”.** O anexo menciona “4 condições” sem enumerá-las. A cópia reserva vaga 2 para obras da espinha, exige cruzamento por arquivo/juiz/vaga e condiciona laterais à disponibilidade; a B2 exemplifica que censo fechado e lei zero não bastam quando há arquivos comuns. Cumprimento das quatro condições e vaga disponível: **[nao verificado]**. (`respostas_1711_0915.txt:5`, anexo; `app/docs/BACKLOG.md:1875`; `app/docs/BACKLOG.md:2088`; `app/docs/BACKLOG.md:824`.)

   A CLASSE-D estava posicionada após F1 e antes de F2; há registro posterior de que F2 pode abrir sem esperá-la. O novo aval pode estabelecer prioridade relativa desta lateral sobre CLASSE-D, mas isso não comprova vaga livre nem permite deslocar a espinha, que o próprio anexo preserva. (`app/docs/BACKLOG.md:1990`; `app/docs/PROMPTS.md:2448`; `respostas_1711_0915.txt:5`, anexo.)

4. **Somente registrar/preparar não autoriza gerar TXT, corrigir cadastro ou aplicar dinheiro.** Usar o gerador como sonda teria escrita: ele pode aprovar fechamentos antes da validação final do layout. Uma futura cura precisa respeitar exportada e os demais `!`; neste aval a construção está expressamente excluída. (`app/folha/views.py:250`; `app/folha/views.py:252`; `app/folha/views.py:275`; `CLAUDE.md:265`; `CLAUDE.md:411`; `respostas_1711_0915.txt:5`, anexo.)

VEREDITO: item 1 PROCEDE; item 4 PROCEDE -- B1 precisa fechar espera e retirada do PDF antes do container sair; o item 4 permite preparar, mas não demonstra as quatro condições nem a disponibilidade da segunda vaga. (`app/relatorios/lote_espelho.py:132`; `bin/sombra.sh:189`; `bin/sombra.sh:490`; `respostas_1711_0915.txt:5`, anexo; `app/docs/BACKLOG.md:2088`.)

## 17/11 09:18 · sessao 0ef5ca9a8648 · - · - · origem=Ronald (tmux)

Item 4 — evidências para consolidação:

1. **Defeito confere como possibilidade do código.** `classificar_export` inicia em `entra`, aplica `.strip()` e só exclui código vazio; não exige número nem limite de dez posições. A montagem usa `.zfill(10)`, que preserva texto e códigos longos; o layout exige registro de 43 caracteres e `matric` numérico. Havendo linha gerada, qualquer erro recusa o TXT inteiro da empresa. (`app/folha/export.py:840`; `app/folha/export.py:841`; `app/folha/export.py:872`; `app/folha/export.py:728`; `app/folha/export.py:36`; `app/folha/export.py:273`; `app/folha/export.py:277`; `app/folha/views.py:275`; `app/folha/views.py:279`.) Ocorrência real do caso col1041 e dimensão na frota: **[nao verificado]**.

   Precisão importante: o layout **não verifica duplicidade**; código numérico com menos de dez posições é preenchido com zeros. Portanto “o validar_layout recusaria vazio, repetido, fora do tamanho” não descreve literalmente todos esses casos: vazio já é excluído pelo classificador; repetido não é recusado por esse validador; tamanho menor é admitido por preenchimento. (`app/folha/export.py:266`; `app/folha/export.py:307`; `app/folha/export.py:728`; `app/folha/export.py:872`.)

2. **Não encontrei juiz registrado para “código do Domínio válido”.** O registro declara `classificar_export` para aprovação e entrada no TXT; a tabela documental também aponta esse juiz, sem autoridade específica para validade do código. Isso não transforma automaticamente o validador de linhas em juiz já registrado desse novo predicado. (`app/core/juizes.py:319`; `app/core/juizes.py:329`; `app/core/juizes.py:330`; `app/docs/JUIZES.md:118`.) Registro de autoridade nova exige corte; a autonomia para declarar autoridade existente exige função já existente, única autoridade medida, mesmo predicado e universo igual. (`CLAUDE.md:74`; `CLAUDE.md:278`; `CLAUDE.md:280`.)

3. **Preparar censo cabe; abertura automática ainda não está demonstrada.** A segunda vaga tem fila: E4C-F1-B2 ocupa a vaga 2 e espera BUG-PAR por cruzar oito arquivos; a ordem anterior também protege R3/F1. Laterais exigem conferir cruzamento por arquivo/juiz/vaga e só andam com a suíte livre. (`app/docs/BACKLOG.md:824`; `app/docs/BACKLOG.md:1875`; `app/docs/BACKLOG.md:2088`.) CLASSE-D estava explicitamente após E4c F1 e antes de F2; o novo anexo pode priorizar esta lateral sobre CLASSE-D, mas não prova disponibilidade nem autoriza deslocar a espinha. (`app/docs/BACKLOG.md:1990`; `app/docs/BACKLOG.md:2265`; anexo `respostas_1711_0915.txt`, item 4 **[nao verificado: arquivo ausente na cópia]**.) As “4 condições” não são enumeradas no anexo; identificá-las e demonstrá-las permanece **[nao verificado]**. Há ainda portão documentado da segunda vaga por carga, horário e arquivos comuns. (`app/docs/BACKLOG.md:2187`.)

4. **Somente registrar/preparar não contém apply de exportada ou dinheiro.** Uma futura execução do gerador não seria leitura: aprova fechamentos antes da validação final do layout; o defeito não autoriza usar esse caminho para medir. (`app/folha/views.py:250`; `app/folha/views.py:252`; `app/folha/views.py:275`.) Exportada, escala/vínculo e regras fora do pedido continuam sob `!`. (`CLAUDE.md:265`; `CLAUDE.md:268`.)

Complemento útil ao censo: o modelo aceita texto de até vinte caracteres, sem validator numérico no campo; a admissão e a edição recebem `codigo_folha` com `.strip()` e gravam o valor não vazio. A porta de persistência confere operação/dono, sem validar o código nessas funções. (`app/colaboradores/models.py:311`; `app/colaboradores/models.py:312`; `app/colaboradores/views.py:1131`; `app/colaboradores/views.py:1231`; `app/colaboradores/views.py:1237`; `app/colaboradores/views.py:1353`; `app/colaboradores/views.py:1365`; `app/colaboradores/services/colaborador.py:140`; `app/colaboradores/services/colaborador.py:148`; `app/colaboradores/services/colaborador.py:152`; `app/colaboradores/services/colaborador.py:160`.)

## 17/11 09:17 · sessao bf379d6c6987 · ENSAIO-DA-FOLHA-11 · leitura · origem=Code

1. **Quem grava:** `folha.porta_export.lavrar()` escreve em **`MetricaSnapshot`**, chave **`porta_export_leitores`**, com escopo `empresa`, `escopo_id=empresa.pk` e `data_ref=hoje`; guarda competência em `detalhe.mes/ano` e falhas em `valor`. (`app/folha/porta_export.py:18`, `app/folha/porta_export.py:768-796`) Quem chama essa gravação é **`selo_leitores_no_mesmo_numero`**, após medir cada empresa ativa, com mês/ano obrigatórios. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:27-35`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86-92`)

2. **Cron:** o selo está em **`FORA_DE_PIPELINE`**, explicitamente destinado à execução manual antes do export; portanto **não tem horário nem cobertura automática da aberta 11/2026**. (`app/config/crons.py:769`, `app/config/crons.py:887-897`) O universo geral do cron é empresas `(2, 3, 4)`, schema `juliani`; o comando do selo percorre todas as empresas ativas do tenant e mede exatamente a competência passada em `--mes/--ano`. (`app/config/crons.py:17-18`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:35`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86-92`) Apesar do comentário dizer “roda no ATO do export”, o POST **somente confere**. (`app/config/crons.py:893-894`, `app/folha/views.py:128-133`) Crontab instalado e execução efetiva de hoje: [nao verificado].

3. **O que o admin vê:** sem carimbo, `gerar_dominio_txt` interrompe o fluxo, passa a recusa como erro e redireciona para a tela de exportação, que exibe a mensagem em alerta. (`app/folha/views.py:128-133`, `app/core/respostas.py:50-51`, `app/core/respostas.py:68-71`, `app/templates/folha/exportar.html:10`) A mensagem manda literalmente **`Rode: docker exec saas_core python manage.py tenant_command selo_leitores_no_mesmo_numero --schema=juliani --mes 11 --ano 2026`**; não orienta uma ação disponível na tela. (`app/folha/porta_export.py:809-813`) **Não há botão/rota para produzir esse carimbo** no fluxo: há seleção, Raio-X, PDFs e geração do TXT; o botão de gerar aparece por haver elegíveis, sem exigir previamente o selo. (`app/folha/urls.py:6-29`, `app/templates/folha/exportar.html:25-54`, `app/folha/views.py:356`, `app/folha/views.py:406-428`) Que esse admin específico não tenha shell: [nao verificado].

4. **Escopo e validade:** a porta exige carimbo da **mesma empresa, mesma competência, data de hoje e valor zero**. (`app/folha/porta_export.py:807-821`, `app/folha/porta_export.py:865-867`) Porém, a chave persistida não inclui competência: medir outra competência da mesma empresa no mesmo dia **sobrescreve** o carimbo anterior. (`app/folha/porta_export.py:772-776`, `app/inteligencia/models.py:102-105`) “Hoje” é **data civil local**, via `timezone.localdate()`, com fuso padrão `America/Sao_Paulo`: vale até a virada da data, não por 24 horas desde a medição. (`app/folha/porta_export.py:21-23`, `app/folha/porta_export.py:807-808`, `app/config/settings/base.py:93-95`)

BECO: sim -- a recusa exige comando de shell e o fluxo da tela não oferece ação para gerar o carimbo. (`app/folha/porta_export.py:809-813`, `app/folha/urls.py:6-29`, `app/folha/views.py:406-428`)

## 17/11 09:17 · sessao cb7662e893ce · - · - · origem=Ronald (tmux)

Item 1 tem caminho viável, mas o `--rodar` não garante sozinho a espera do filho nem a retirada do PDF; essas condições dependem do script B1, cujo conteúdo não foi fornecido. (`bin/sombra.sh:490-493`; `app/relatorios/lote_espelho.py:132-146`; [nao verificado].)

1. **Premissas.** A view usa `pedir` e `disparar`; o filho executa o command que chama o mesmo gerador de lote. (`app/relatorios/views.py:1227-1246`; `app/relatorios/lote_espelho.py:127-133`; `app/relatorios/management/commands/gerar_cartoes_em_lote.py:44-64`; `app/relatorios/lote_espelho.py:205-207`.)

   Por inferência do código, o filho herda a configuração da sombra: `Popen` não passa `env`, o container fixa `DJANGO_SETTINGS_MODULE=config.settings.sombra`, e `manage.py` usa `setdefault`, preservando esse valor; o settings fixa banco `sombra`, host `sombra_db`, e o comando recebe o schema do pai. Execução efetiva: [nao verificado]. (`app/relatorios/lote_espelho.py:126-133`; `bin/sombra.sh:184`; `app/manage.py:9`; `app/config/settings/sombra.py:29`; `app/config/settings/sombra.py:47-51`.)

2. **Compatibilidade.** Sem POST HTTP, a restrição específica de smoke de porta não se aplica ao caminho proposto; o aval fornecido aceita função/command e as duas escritas da lavra. Permanecem o escopo literal, banco da sombra e CPUSET de teste. (`CLAUDE.md:37-38`; `CLAUDE.md:87`; `CLAUDE.md:205-206`; `bin/sombra.sh:183-184`; `respostas_1711_0915.txt:2`, anexo, contando linhas desde “aval Ronald”.)

3. **Condições literais.**
   
   - **Espera:** `disparar` cria processo com `start_new_session=True` e devolve somente PID; não chama `wait`. `--rodar` espera o comando principal do container, sem supervisão adicional desse filho. O script precisa permanecer executando até o job terminar. (`app/relatorios/lote_espelho.py:132-146`; `bin/sombra.sh:490-493`.)
   - **Trava inteira:** `--rodar` já adquire `flock`; a shell do host mantém o descritor 8 enquanto aguarda o container. Assim, basta que o script não termine antes de concluir geração, conferência e retirada do artefato; não é necessário um segundo `--com-a-sombra`. (`bin/sombra.sh:487-492`; `bin/sombra_trava.sh:26-39`.)
   - **Alternativa síncrona:** dentro do script B1, criar/reutilizar o lote pela porta e chamar o command de forma síncrona evita o `Popen`; o command assume e executa o lote antes de retornar. O command precisa de lote já existente e schema correto. (`app/relatorios/lote_espelho.py:83-114`; `app/relatorios/management/commands/gerar_cartoes_em_lote.py:27-38`; `app/relatorios/management/commands/gerar_cartoes_em_lote.py:44-64`.)
   - **PDF:** o lote grava em `cartoes_lote/`, dentro de media; `/app/media` é tmpfs, e `/sombra` corresponde a `logs/sombra`. O script precisa copiar o PDF para `/sombra` antes do container encerrar e depois encaminhá-lo à pasta do ensaio; essa cópia não é automática. (`app/relatorios/models.py:125`; `app/relatorios/lote_espelho.py:250-253`; `bin/sombra.sh:81`; `bin/sombra.sh:183`; `bin/sombra.sh:189-190`.)
   - **Resultado:** `CONCLUIDO` e quantidade gerada não provam ausência de fallback: o informacional também incrementa `n_ok`, e o lote termina concluído. (`app/relatorios/services.py:235-255`; `app/relatorios/lote_espelho.py:251-253`.)

4. **Efeitos adicionais.** As duas operações de `MetricaSnapshot` podem atualizar registros existentes, e ocorrem antes do script B1, em outro container. Também são escritos `lavra_prod.json`, `repor_lavra.py` e `logs/<rotulo>.out`; esses artefatos devem constar da descrição do ensaio. (`bin/sombra.sh:144`; `bin/sombra.sh:151`; `bin/sombra.sh:157-167`; `bin/sombra.sh:489-492`.)

   Não há, nesses trechos, demonstração de alteração de competência exportada ou pagamento; a lavra deixou de decidir dinheiro segundo o comentário vigente. Efeitos reais em produção: [nao verificado]. (`app/ponto/services/esmeril_espelho.py:351-352`; `CLAUDE.md:265-268`.)

   **Cuidado concreto:** envolver `--rodar` em `--com-a-sombra` executaria a lavra nas duas portas, excedendo as duas operações declaradas. (`bin/sombra.sh:460`; `bin/sombra.sh:489`; `bin/sombra.sh:157-163`.)

## 17/11 09:17 · sessao 59845f30321f · - · - · origem=Ronald (tmux)

- O comando **não tem horário em `CRONS` nem elo em `ENCADEADOS`**: está em `FORA_DE_PIPELINE`, mapa iniciado em `app/config/crons.py:769`, com justificativa explícita de execução sob ato/manual em `app/config/crons.py:887` e `app/config/crons.py:891`.
- A declaração “roda no ATO do export” em `app/config/crons.py:894` é comentário/descritivo: o próprio comando explica que ele mede e lavra, enquanto o TXT apenas confere, em `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:36`.
- **Competência é fornecida explicitamente**: `--mes` e `--ano` são obrigatórios, em `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:28`. O comando usa esses valores em `:35`, percorre todas as empresas ativas em `:86`, mede em `:88` e lavra em `:92`. Logo `--mes 11 --ano 2026` mede essa competência, mas não há cobertura automática de 11/2026 por esse selo; evidências: `app/config/crons.py:891` e `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:28`.
- O universo padrão do cron é empresas `(2, 3, 4)`, tenant `juliani`, em `app/config/crons.py:17`; isso **não restringe** o selo manual, que percorre `Empresa.objects.filter(ativa=True)` em `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86`.
- O render percorre `CRONS` em `app/config/crons.py:1889`, expande empresas em `:1894`, acrescenta schema em `:1899` e conecta correntes em `:1913`. `bin/crons.sh` somente renderiza/check/instala esse resultado em `bin/crons.sh:24` e `bin/crons.sh:27`.
- Há cron parecido, **`vigiar_txt_dominio`**, declarado como elo em `app/config/crons.py:1615`, mas ele compara o acervo de exportações e não grava o selo, conforme `app/folha/management/commands/vigiar_txt_dominio.py:8` e chamada de `censo` em `:59`. O `lavrar_retrato_folha`, outro elo em `app/config/crons.py:1655`, é retrato para a tela, descrito em `app/config/crons.py:1619`; não substitui esse selo.
- `cartao_x_txt_divergentes` é chamado separadamente pelo placar em `bin/placar_code.sh:330`, sem chamada a `lavrar`; os contadores PDF/cartão são explicitamente tratados como manuais por competência em `app/config/crons.py:1139`, e o cartão está em `FORA_DE_PIPELINE` em `app/config/crons.py:1282`.
- Cron instalado e execução real em produção hoje: **[não verificado]**.

## 17/11 09:16 · sessao b9ab0ef52323 · - · - · origem=Ronald (tmux)

(3) O clique usa formulário POST comum para `folha:gerar_txt`, com empresa/mês/ano ocultos e botão “Gerar arquivo dos N elegiveis”; a prévia decide mostrar esse botão apenas por `n_entra > 0`, sem consultar carimbo (`app/folha/views.py:356`, `app/folha/views.py:416-428`).

`gerar_dominio_txt` chama `conferir(empresa, mes, ano)`; se houver recusa, devolve a mensagem integral por `resposta_acao(ok=False, ..., redirect_url=folha:exportar)` (`app/folha/views.py:128-133`). Esse helper classifica `ok=False` como erro, grava Django message e retorna redirect 302 para a tela (`app/core/respostas.py:50-51`, `app/core/respostas.py:68-71`). O template exibe a mensagem em um `alert`; a base também a passa ao toast, cuja variante erro é vermelha e desaparece após dez segundos (`app/templates/folha/exportar.html:10`, `app/templates/base.html:334`, `app/templates/core/_dialogo.html:26-43`, `app/templates/core/_dialogo.html:46-54`).

Sem carimbo, o texto é: “A PORTA DO EXPORT NAO FOI MEDIDA HOJE para %s. O TXT nao sai sem a prova de que a tela, o PDF, o fechamento e o TXT estao no mesmo numero. Rode: %s”; o `%s` final contém `docker exec saas_core python manage.py tenant_command selo_leitores_no_mesmo_numero --schema=juliani --mes %02d --ano %s` (`app/folha/porta_export.py:809-813`).

A mensagem oferece uma instrução de shell, sem ação correspondente na tela: o inventário de rotas da folha não contém medir/lavrar o carimbo; o template oferece seleção de empresa/competência e Raio-X; a prévia oferece PDFs e gerar TXT (`app/folha/urls.py:6-29`, `app/templates/folha/exportar.html:25-54`, `app/folha/views.py:406-430`). O próprio POST declara que só confere, pois medir custa aproximadamente cem segundos para 205 colaboradores e quem mede é o comando (`app/folha/views.py:124-126`). Que o administrador específico não tenha acesso ao shell: [nao verificado].

Conclusão para consolidar: **BECO: sim**, no sentido definido pelo usuário, pois a recusa manda executar comando operacional e não oferece pela tela uma ação para produzir o carimbo (`app/folha/porta_export.py:809-813`, `app/folha/urls.py:6-29`, `app/folha/views.py:406-430`).

## 17/11 09:16 · sessao 786cf13e0f0a · - · - · origem=Ronald (tmux)

- **Gravador real:** `folha.porta_export.lavrar(empresa, mes, ano, r)` faz `MetricaSnapshot.objects.update_or_create`; chave literal **`porta_export_leitores`**, `escopo='empresa'`, `escopo_id=empresa.pk`, `data_ref=hoje`; guarda falhas em `valor` (`-1` quando não mediu), competência em `detalhe['mes'/'ano']` e `data_ini=hoje` (`app/folha/porta_export.py:18`, `app/folha/porta_export.py:768-776`).
- **Comando escritor:** `selo_leitores_no_mesmo_numero` exige `--mes` e `--ano`, importa `medir, lavrar`, percorre `Empresa.objects.filter(ativa=True).order_by('id')`, mede e lavra para cada empresa (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:27-35`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86-92`). Pode interromper a sequência se medição levantar exceção ou `cartao_x_txt is None` (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:89-95`).
- **Leitura/recusa:** `conferir` consulta chave+empresa+`data_ref=_hoje()`; ausência produz “A PORTA DO EXPORT NAO FOI MEDIDA HOJE...” com shell `docker exec saas_core python manage.py tenant_command selo_leitores_no_mesmo_numero --schema=juliani --mes %02d --ano %s`; em seguida compara a competência armazenada com a pedida, rejeita `valor is None`, negativo ou positivo; só zero retorna vazio/libera (`app/folha/porta_export.py:800-821`, `app/folha/porta_export.py:865-867`).
- **Nuance importante:** a validação é por empresa **e competência**, mas a identidade persistida é somente **chave+escopo+empresa+data**, sem mês/ano. Logo medir outra competência da mesma empresa no mesmo dia **sobrescreve** o carimbo anterior; não coexistem carimbos independentes de 10 e 11 no mesmo dia (`app/folha/porta_export.py:772-776`; constraint confirma `app/inteligencia/models.py:102-105`).
- **Validade:** “hoje” é `timezone.localdate()`, isto é, data civil local, e não janela móvel de 24 h; o carimbo deixa de atender assim que a data local muda. Configuração padrão: `TIME_ZONE='America/Sao_Paulo'`, `USE_TZ=True` (`app/folha/porta_export.py:21-23`, `app/folha/porta_export.py:807-808`, `app/config/settings/base.py:92-95`).
- **Comentário enganoso no modelo:** docstring de `MetricaSnapshot` diz “Escrito so por inteligencia.metricas”, porém a escrita efetiva aqui é `folha.porta_export.lavrar` (`app/inteligencia/models.py:81-82`; `app/folha/porta_export.py:770-776`).

## 17/11 09:15 · sessao a362d0d29557 · QUEIMA-UTIL-1711 · sonda · origem=Code

Não executei a sonda: `j6_sonda.py` não foi localizado na cópia. Abaixo, reprodução literal da saída fornecida. [nao verificado] [nao verificado]

```text
sonda_leitura: j6_sonda.py [2865480a2c411b4d] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

INSTANTE 2026-11-17T09:04:55.320910-03:00
NUMERO F5_fisico_ultimo_sem_ato_ate_agora: 143
NUMERO P1_A_candidatas_sem_nova_devolucao: 2174
NUMERO P1_B_candidatas_com_devolucao: 2174
NUMERO P1_anexo_F5_vivas_instante_sem_texto: 143
NUMERO P1_intersecao_ressuscitaveis_F5: 0
NUMERO P1_ultimo_sem_ato_ate_agora: 1709
NUMERO P1_ultimo_silencio: 465
NUMERO P2_A_textos_divergentes_conservados: 226
NUMERO P2_B_mudaria_juiz_respondida_se_trocar_so_texto: 2
NUMERO P2_B_resultaria_par_ilegal_se_trocar_so_texto: 213
NUMERO P2_B_texto_nao_vazio_em_REABERTURA_ou_RECUSA: 224
NUMERO P2_B_textos_divergentes_substituidos: 226
NUMERO P2_divergentes_apos_trim: 226
NUMERO P2_divergentes_com_materializacao: 90
NUMERO P2_divergentes_com_validacao: 7
NUMERO P2_divergentes_disputa_aberta: 204
NUMERO P2_sem_ato_ate_agora_conservar_nas_duas: 48822
NUMERO P2_ultimo_reabertura: 214
NUMERO P2_ultimo_recusa: 12
NUMERO perguntas_total: 50895
COL col039 P1_A_candidatas_sem_nova_devolucao: 0
COL col039 P1_B_candidatas_com_devolucao: 0
COL col039 P2_A_textos_divergentes_conservados: 1
COL col039 P2_B_textos_divergentes_substituidos: 1
COL col042 P1_A_candidatas_sem_nova_devolucao: 17
COL col042 P1_B_candidatas_com_devolucao: 17
COL col042 P2_A_textos_divergentes_conservados: 0
COL col042 P2_B_textos_divergentes_substituidos: 0
COL col043 P1_A_candidatas_sem_nova_devolucao: 0
COL col043 P1_B_candidatas_com_devolucao: 0
COL col043 P2_A_textos_divergentes_conservados: 1
COL col043 P2_B_textos_divergentes_substituidos: 1
COL col050 P1_A_candidatas_sem_nova_devolucao: 0
COL col050 P1_B_candidatas_com_devolucao: 0
COL col050 P2_A_textos_divergentes_conservados: 2
COL col050 P2_B_textos_divergentes_substituidos: 2
COL col062 P1_A_candidatas_sem_nova_devolucao: 0
COL col062 P1_B_candidatas_com_devolucao: 0
COL col062 P2_A_textos_divergentes_conservados: 1
COL col062 P2_B_textos_divergentes_substituidos: 1
COL col072 P1_A_candidatas_sem_nova_devolucao: 5
COL col072 P1_B_candidatas_com_devolucao: 5
COL col072 P2_A_textos_divergentes_conservados: 0
COL col072 P2_B_textos_divergentes_substituidos: 0
COL col082 P1_A_candidatas_sem_nova_devolucao: 1
COL col082 P1_B_candidatas_com_devolucao: 1
COL col082 P2_A_textos_divergentes_conservados: 0
COL col082 P2_B_textos_divergentes_substituidos: 0
COL col087 P1_A_candidatas_sem_nova_devolucao: 6
COL col087 P1_B_candidatas_com_devolucao: 6
COL col087 P2_A_textos_divergentes_conservados: 0
COL col087 P2_B_textos_divergentes_substituidos: 0
COL col089 P1_A_candidatas_sem_nova_devolucao: 4
COL col089 P1_B_candidatas_com_devolucao: 4
COL col089 P2_A_textos_divergentes_conservados: 0
COL col089 P2_B_textos_divergentes_substituidos: 0
COL col090 P1_A_candidatas_sem_nova_devolucao: 14
COL col090 P1_B_candidatas_com_devolucao: 14
COL col090 P2_A_textos_divergentes_conservados: 0
COL col090 P2_B_textos_divergentes_substituidos: 0
COL col091 P1_A_candidatas_sem_nova_devolucao: 8
COL col091 P1_B_candidatas_com_devolucao: 8
COL col091 P2_A_textos_divergentes_conservados: 0
COL col091 P2_B_textos_divergentes_substituidos: zombies 0
COL col101 P1_A_candidatas_sem_nova_devolucao: 0
COL col101 P1_B_candidatas_com_devolucao: 0
COL col101 P2_A_textos_divergentes_conservados: 2
COL col101 P2_B_textos_divergentes_substituidos: 2
COL col104 P1_A_candidatas_sem_nova_devolucao: 8
COL col104 P1_B_candidatas_com_devolucao: 8
COL col104 P2_A_textos_divergentes_conservados: 0
COL col104 P2_B_textos_divergentes_substituidos: 0
COL col108 P1_A_candidatas_sem_nova_devolucao: 1
COL col108 P1_B_candidatas_com_devolucao: 1
COL col108 P2_A_textos_divergentes_conservados: 5
COL col108 P2_B_textos_divergentes_substituidos: 5
COL col120 P1_A_candidatas_sem_nova_devolucao: 2
COL col120 P1_B_candidatas_com_devolucao: 2
COL col120 P2_A_textos_divergentes_conservados: 0
COL col120 P2_B_textos_divergentes_substituidos: 0
COL col122 P1_A_candidatas_sem_nova_devolucao: 6
COL col122 P1_B_candidatas_com_devolucao: 6
COL col122 P2_A_textos_divergentes_conservados: 0
COL col122 P2_B_textos_divergentes_substituidos: 0
COL col126 P1_A_candidatas_sem_nova_devolucao: 8
COL col126 P1_B_candidatas_com_devolucao: 8
COL col126 P2_A_textos_divergentes_conservados: 0
COL col126 P2_B_textos_divergentes_substituidos: 0
COL col129 P1_A_candidatas_sem_nova_devolucao: 0
COL col129 P1_B_candidatas_com_devolucao: 0
COL col129 P2_A_textos_divergentes_conservados: 1
COL col129 P2_B_textos_divergentes_substituidos: 1
COL col135 P1_A_candidatas_sem_nova_devolucao: 12
COL col135 P1_B_candidatas_com_devolucao: 12
COL col135 P2_A_textos_divergentes_conservados: 0
COL col135 P2_B_textos_divergentes_substituidos: 0
COL col138 P1_A_candidatas_sem_nova_devolucao: 6
COL col138 P1_B_candidatas_com_devolucao: 6
COL col138 P2_A_textos_divergentes_conservados: 1
COL col138 P2_B_textos_divergentes_substituidos: 1
COL col145 P1_A_candidatas_sem_nova_devolucao: 0
COL col145 P1_B_candidatas_com_devolucao: 0
COL col145 P2_A_textos_divergentes_conservados: 1
COL col145 P2_B_textos_divergentes_substituidos: 1
COL col147 P1_A_candidatas_sem_nova_devolucao: 28
COL col147 P1_B_candidatas_com_devolucao: 28
COL col147 P2_A_textos_divergentes_conservados: 0
COL col147 P2_B_textos_divergentes_substituidos: 0
COL col148 P1_A_candidatas_sem_nova_devolucao: 0
COL col148 P1_B_candidatas_com_devolucao: 0
COL col148 P2_A_textos_divergentes_conservados: 1
COL col148 P2_B_textos_divergentes_substituidos: 1
COL col155 P1_A_candidatas_sem_nova_devolucao: 2
COL col155 P1_B_candidatas_com_devolucao: 2
COL col155 P2_A_textos_divergentes_conservados: 0
COL col155 P2_B_textos_divergentes_substituidos: 0
COL col161 P1_A_candidatas_sem_nova_devolucao: 1
COL col161 P1_B_candidatas_com_devolucao: 1
COL col161 P2_A_textos_divergentes_conservados: 0
COL col161 P2_B_textos_divergentes_substituidos: 0
COL col169 P1_A_candidatas_sem_nova_devolucao: 0
COL col169 P1_B_candidatas_com_devolucao: 0
COL col169 P2_A_textos_divergentes_conservados: 1
COL col169 P2_B_textos_divergentes_substituidos: 1
COL col200 P1_A_candidatas_sem_nova_devolucao: 0
COL col200 P1_B_candidatas_com_devolucao: 0
COL col200 P2_A_textos_divergentes_conservados: 10
COL col200 P2_B_textos_divergentes_substituidos: 10
COL col207 P1_A_candidatas_sem_nova_devolucao: 2
COL col207 P1_B_candidatas_com_devolucao: 2
COL col207 P2_A_textos_divergentes_conservados: 0
COL col207 P2_B_textos_divergentes_substituidos: 0
COL col209 P1_A_candidatas_sem_nova_devolucao: 8
COL col209 P1_B_candidatas_com_devolucao: 8
COL col209 P2_A_textos_divergentes_conservados: 0
COL col209 P2_B_textos_divergentes_substituidos: 0
COL col228 P1_A_candidatas_sem_nova_devolucao: 10
COL col228 P1_B_candidatas_com_devolucao: 10
COL col228 P2_A_textos_divergentes_conservados: 0
COL col228 P2_B_textos_divergentes_substituidos: 0
COL col239 P1_A_candidatas_sem_nova_devolucao: 0
COL col239 P1_B_candidatas_com_devolucao: 0
COL col239 P2_A_textos_divergentes_conservados: 2
COL col239 P2_B_textos_divergentes_substituidos: 2
COL col247 P1_A_candidatas_sem_nova_devolucao: 0
COL col247 P1_B_candidatas_com_devolucao: 0
COL col247 P2_A_textos_divergentes_conservados: 4
COL col247 P2_B_textos_divergentes_substituidos: 4
COL col250 P1_A_candidatas_sem_nova_devolucao: 1
COL col250 P1_B_candidatas_com_devolucao: 1
COL col250 P2_A_textos_divergentes_conservados: 0
COL col250 P2_B_textos_divergentes_substituidos: 0
COL col258 P1_A_candidatas_sem_nova_devolucao: 1
COL col258 P1_B_candidatas_com_devolucao: 1
COL col258 P2_A_textos_divergentes_conservados: 0
COL col258 P2_B_textos_divergentes_substituidos: 0
COL col268 P1_A_candidatas_sem_nova_devolucao: 8
COL col268 P1_B_candidatas_com_devolucao: 8
COL col268 P2_A_textos_divergentes_conservados: 0
COL col268 P2_B_textos_divergentes_substituidos: 0
COL col270 P1_A_candidatas_sem_nova_devolucao: 63
COL col270 P1_B_candidatas_com_devolucao: 63
COL col270 P2_A_textos_divergentes_conservados: 0
COL col270 P2_B_textos_divergentes_substituidos: 0
COL col281 P1_A_candidatas_sem_nova_devolucao: 0
COL col281 P1_B_candidatas_com_devolucao: 0
COL col281 P2_A_textos_divergentes_conservados: 1
COL col281 P2_B_textos_divergentes_substituidos: 1
COL col288 P1_A_candidatas_sem_nova_devolucao: 0
COL col288 P1_B_candidatas_com_devolucao: 0
COL col288 P2_A_textos_divergentes_conservados: 1
COL col288 P2_B_textos_divergentes_substituidos: 1
COL col297 P1_A_candidatas_sem_nova_devolucao: 0
COL col297 P1_B_candidatas_com_devolucao: 0
COL col297 P2_A_textos_divergentes_conservados: 3
COL col297 P2_B_textos_divergentes_substituidos: 3
COL col304 P1_A_candidatas_sem_nova_devolucao: 0
COL col304 P1_B_candidatas_com_devolucao: 0
COL col304 P2_A_textos_divergentes_conservados: 8
COL col304 P2_B_textos_divergentes_substituidos: 8
COL col305 P1_A_candidatas_sem_nova_devolucao: 36
COL col305 P1_B_candidatas_com_devolucao: 36
COL col305 P2_A_textos_divergentes_conservados: 0
COL col305 P2_B_textos_divergentes_substituidos: 0
COL col325 P1_A_candidatas_sem_nova_devolucao: 26
COL col325 P1_B_candidatas_com_devolucao: 26
COL col325 P2_A_textos_divergentes_conservados: 0
COL col325 P2_B_textos_divergentes_substituidos: 0
COL col338 P1_A_candidatas_sem_nova_devolucao: 39
COL col338 P1_B_candidatas_com_devolucao: 39
COL col338 P2_A_textos_divergentes_conservados: 0
COL col338 P2_B_textos_divergentes_substituidos: 0
COL col343 P1_A_candidatas_sem_nova_devolucao: 27
COL col343 P1_B_candidatas_com_devolucao: 27
COL col343 P2_A_textos_divergentes_conservados: 0
COL col343 P2_B_textos_divergentes_substituidos: 0
COL col346 P1_A_candidatas_sem_nova_devolucao: 2
COL col346 P1_B_candidatas_com_devolucao: 2
COL col346 P2_A_textos_divergentes_conservados: 0
COL col346 P2_B_textos_divergentes_substituidos: 0
COL col354 P1_A_candidatas_sem_nova_devolucao: 2
COL col354 P1_B_candidatas_com_devolucao: 2
COL col354 P2_A_textos_divergentes_conservados: 12
COL col354 P2_B_textos_divergentes_substituidos: 12
COL col362 P1_A_candidatas_sem_nova_devolucao: 0
COL col362 P1_B_candidatas_com_devolucao: 0
COL col362 P2_A_textos_divergentes_conservados: 1
COL col362 P2_B_textos_divergentes_substituidos: 1
COL col363 P1_A_candidatas_sem_nova_devolucao: 0
COL col363 P1_B_candidatas_com_devolucao: 0
COL col363 P2_A_textos_divergentes_conservados: 3
COL col363 P2_B_textos_divergentes_substituidos: 3
COL col367 P1_A_candidatas_sem_nova_devolucao: 69
COL col367 P1_B_candidatas_com_devolucao: 69
COL col367 P2_A_textos_divergentes_conservados: 0
COL col367 P2_B_textos_divergentes_substituidos: 0
COL col370 P1_A_candidatas_sem_nova_devolucao: 2
COL col370 P1_B_candidatas_com_devolucao: 2
COL col370 P2_A_textos_divergentes_conservados: 0
COL col370 P2_B_textos_divergentes_substituidos: 0
COL col371 P1_A_candidatas_sem_nova_devolucao: 0
COL col371 P1_B_candidatas_com_devolucao: 0
COL col371 P2_A_textos_divergentes_conservados: 1
COL col371 P2_B_textos_divergentes_substituidos: 1
COL col373 P1_A_candidatas_sem_nova_devolucao: 0
COL col373 P1_B_candidatas_com_devolucao: 0
COL col373 P2_A_textos_divergentes_conservados: 3
COL col373 P2_B_textos_divergentes_substituidos: 3
COL col374 P1_A_candidatas_sem_nova_devolucao: 0
COL col374 P1_B_candidatas_com_devolucao: 0
COL col374 P2_A_textos_divergentes_conservados: 1
COL col374 P2_B_textos_divergentes_substituidos: 1
COL col385 P1_A_candidatas_sem_nova_devolucao: 6
COL col385 P1_B_candidatas_com_devolucao: 6
COL col385 P2_A_textos_divergentes_conservados: 0
COL col385 P2_B_textos_divergentes_substituidos: 0
COL col388 P1_A_candidatas_sem_nova_devolucao: 6
COL col388 P1_B_candidatas_com_devolucao: 6
COL col388 P2_A_textos_divergentes_conservados: 0
COL col388 P2_B_textos_divergentes_substituidos: 0
COL col393 P1_A_candidatas_sem_nova_devolucao: 54
COL col393 P1_B_candidatas_com_devolucao: 54
COL col393 P2_A_textos_divergentes_conservados: 0
COL col393 P2_B_textos_divergentes_substituidos: 0
COL col400 P1_A_candidatas_sem_nova_devolucao: 4
COL col400 P1_B_candidatas_com_devolucao: 4
COL col400 P2_A_textos_divergentes_conservados: 0
COL col400 P2_B_textos_divergentes_substituidos: 0
COL col403 P1_A_candidatas_sem_nova_devolucao: 23
COL col403 P1_B_candidatas_com_devolucao: 23
COL col403 P2_A_textos_divergentes_conservados: 0
COL col403 P2_B_textos_divergentes_substituidos: 0
COL col417 P1_A_candidatas_sem_nova_devolucao: 67
COL col417 P1_B_candidatas_com_devolucao: 67
COL col417 P2_A_textos_divergentes_conservados: 0
COL col417 P2_B_textos_divergentes_substituidos: 0
COL col422 P1_A_candidatas_sem_nova_devolucao: 104
COL col422 P1_B_candidatas_com_devolucao: 104
COL col422 P2_A_textos_divergentes_conservados: 0
COL col422 P2_B_textos_divergentes_substituidos: 0
COL col430 P1_A_candidatas_sem_nova_devolucao: 16
COL col430 P1_B_candidatas_com_devolucao: 16
COL col430 P2_A_textos_divergentes_conservados: 0
COL col430 P2_B_textos_divergentes_substituidos: 0
COL col432 P1_A_candidatas_sem_nova_devolucao: 0
COL col432 P1_B_candidatas_com_devolucao: 0
COL col432 P2_A_textos_divergentes_conservados: 1
COL col432 P2_B_textos_divergentes_substituidos: 1
COL col437 P1_A_candidatas_sem_nova_devolucao: 9
COL col437 P1_B_candidatas_com_devolucao: 9
COL col437 P2_A_textos_divergentes_conservados: 0
COL col437 P2_B_textos_divergentes_substituidos: 0
COL col441 P1_A_candidatas_sem_nova_devolucao: 5
COL col441 P1_B_candidatas_com_devolucao: 5
COL col441 P2_A_textos_divergentes_conservados: 0
COL col441 P2_B_textos_divergentes_substituidos: 0
COL col443 P1_A_candidatas_sem_nova_devolucao: 2
COL col443 P1_B_candidatas_com_devolucao: 2
COL col443 P2_A_textos_divergentes_conservados: 0
COL col443 P2_B_textos_divergentes_substituidos: 0
COL col444 P1_A_candidatas_sem_nova_devolucao: 0
COL col444 P1_B_candidatas_com_devolucao: 0
COL col444 P2_A_textos_divergentes_conservados: 1
COL col444 P2_B_textos_divergentes_substituidos: 1
COL col446 P1_A_candidatas_sem_nova_devolucao: 0
COL col446 P1_B_candidatas_com_devolucao: 0
COL col446 P2_A_textos_divergentes_conservados: 2
COL col446 P2_B_textos_divergentes_substituidos: 2
COL col455 P1_A_candidatas_sem_nova_devolucao: 0
COL col455 P1_B_candidatas_com_devolucao: 0
COL col455 P2_A_textos_divergentes_conservados: 1
COL col455 P2_B_textos_divergentes_substituidos: 1
COL col456 P1_A_candidatas_sem_nova_devolucao: 0
COL col456 P1_B_candidatas_com_devolucao: 0
COL col456 P2_A_textos_divergentes_conservados: 2
COL col456 P2_B_textos_divergentes_substituidos: 2
COL col460 P1_A_candidatas_sem_nova_devolucao: 20
COL col460 P1_B_candidatas_com_devolucao: 20
COL col460 P2_A_textos_divergentes_conservados: 0
COL col460 P2_B_textos_divergentes_substituidos: 0
COL col466 P1_A_candidatas_sem_nova_devolucao: 0
COL col466 P1_B_candidatas_com_devolucao: 0
COL col466 P2_A_textos_divergentes_conservados: 5
COL col466 P2_B_textos_divergentes_substituidos: 5
COL col476 P1_A_candidatas_sem_nova_devolucao: 17
COL col476 P1_B_candidatas_com_devolucao: 17
COL col476 P2_A_textos_divergentes_conservados: 0
COL col476 P2_B_textos_divergentes_substituidos: 0
COL col490 P1_A_candidatas_sem_nova_devolucao: 1
COL col490 P1_B_candidatas_com_devolucao: 1
COL col490 P2_A_textos_divergentes_conservados: 0
COL col490 P2_B_textos_divergentes_substituidos: 0
COL col492 P1_A_candidatas_sem_nova_devolucao: 22
COL col492 P1_B_candidatas_com_devolucao: 22
COL col492 P2_A_textos_divergentes_conservados: 0
COL col492 P2_B_textos_divergentes_substituidos: 0
COL col493 P1_A_candidatas_sem_nova_devolucao: 0
COL col493 P1_B_candidatas_com_devolucao: 0
COL col493 P2_A_textos_divergentes_conservados: 1
COL col493 P2_B_textos_divergentes_substituidos: 1
COL col496 P1_A_candidatas_sem_nova_devolucao: 1
COL col496 P1_B_candidatas_com_devolucao: 1
COL col496 P2_A_textos_divergentes_conservados: 0
COL col496 P2_B_textos_divergentes_substituidos: 0
COL col502 P1_A_candidatas_sem_nova_devolucao: 0
COL col502 P1_B_candidatas_com_devolucao: 0
COL col502 P2_A_textos_divergentes_conservados: 7
COL col502 P2_B_textos_divergentes_substituidos: 7
COL col503 P1_A_candidatas_sem_nova_devolucao: 0
COL col503 P1_B_candidatas_com_devolucao: 0
COL col503 P2_A_textos_divergentes_conservados: 2
COL col503 P2_B_textos_divergentes_substituidos: 2
COL col504 P1_A_candidatas_sem_nova_devolucao: 0
COL col504 P1_B_candidatas_com_devolucao: 0
COL col504 P2_A_textos_divergentes_conservados: 7
COL col504 P2_B_textos_divergentes_substituidos: 7
COL col506 P1_A_candidatas_sem_nova_devolucao: 21
COL col506 P1_B_candidatas_com_devolucao: 21
COL col506 P2_A_textos_divergentes_conservados: 0
COL col506 P2_B_textos_divergentes_substituidos: 0
COL col507 P1_A_candidatas_sem_nova_devolucao: 0
COL col507 P1_B_candidatas_com_devolucao: 0
COL col507 P2_A_textos_divergentes_conservados: 5
COL col507 P2_B_textos_divergentes_substituidos: 5
COL col510 P1_A_candidatas_sem_nova_devolucao: 67
COL col510 P1_B_candidatas_com_devolucao: 67
COL col510 P2_A_textos_divergentes_conservados: 0
COL col510 P2_B_textos_divergentes_substituidos: 0
COL col515 P1_A_candidatas_sem_nova_devolucao: 17
COL col515 P1_B_candidatas_com_devolucao: 17
COL col515 P2_A_textos_divergentes_conservados: 0
COL col515 P2_B_textos_divergentes_substituidos: 0
COL col518 P1_A_candidatas_sem_nova_devolucao: 9
COL col518 P1_B_candidatas_com_devolucao: 9
COL col518 P2_A_textos_divergentes_conservados: 0
COL col518 P2_B_textos_divergentes_substituidos: 0
COL col523 P1_A_candidatas_sem_nova_devolucao: 0
COL col523 P1_B_candidatas_com_devolucao: 0
COL col523 P2_A_textos_divergentes_conservados: 1
COL col523 P2_B_textos_divergentes_substituidos: 1
COL col529 P1_A_candidatas_sem_nova_devolucao: 4
COL col529 P1_B_candidatas_com_devolucao: 4
COL col529 P2_A_textos_divergentes_conservados: 0
COL col529 P2_B_textos_divergentes_substituidos: 0
COL col555 P1_A_candidatas_sem_nova_devolucao: 24
COL col555 P1_B_candidatas_com_devolucao: 24
COL col555 P2_A_textos_divergentes_conservados: 0
COL col555 P2_B_textos_divergentes_substituidos: 0
COL col559 P1_A_candidatas_sem_nova_devolucao: 13
COL col559 P1_B_candidatas_com_devolucao: 13
COL col559 P2_A_textos_divergentes_conservados: 0
COL col559 P2_B_textos_divergentes_substituidos: 0
COL col569 P1_A_candidatas_sem_nova_devolucao: 2
COL col569 P1_B_candidatas_com_devolucao: 2
COL col569 P2_A_textos_divergentes_conservados: 0
COL col569 P2_B_textos_divergentes_substituidos: 0
COL col576 P1_A_candidatas_sem_nova_devolucao: 12
COL col576 P1_B_candidatas_com_devolucao: 12
COL col576 P2_A_textos_divergentes_conservados: 0
COL col576 P2_B_textos_divergentes_substituidos: 0
COL col608 P1_A_candidatas_sem_nova_devolucao: 3
COL col608 P1_B_candidatas_com_devolucao: 3
COL col608 P2_A_textos_divergentes_conservados: 0
COL col608 P2_B_textos_divergentes_substituidos: 0
COL col612 P1_A_candidatas_sem_nova_devolucao: 0
COL col612 P1_B_candidatas_com_devolucao: 0
COL col612 P2_A_textos_divergentes_conservados: 1
COL col612 P2_B_textos_divergentes_substituidos: 1
COL col631 P1_A_candidatas_sem_nova_devolucao: 2
COL col631 P1_B_candidatas_com_devolucao: 2
COL col631 P2_A_textos_divergentes_conservados: 0
COL col631 P2_B_textos_divergentes_substituidos: 0
COL col648 P1_A_candidatas_sem_nova_devolucao: 0
COL col648 P1_B_candidatas_com_devolucao: 0
COL col648 P2_A_textos_divergentes_conservados: 4
COL col648 P2_B_textos_divergentes_substituidos: 4
COL col649 P1_A_candidatas_sem_nova_devolucao: 4
COL col649 P1_B_candidatas_com_devolucao: 4
COL col649 P2_A_textos_divergentes_conservados: 0
COL col649 P2_B_textos_divergentes_substituidos: 0
COL col651 P1_A_candidatas_sem_nova_devolucao: 36
COL col651 P1_B_candidatas_com_devolucao: 36
COL col651 P2_A_textos_divergentes_conservados: 0
COL col651 P2_B_textos_divergentes_substituidos: 0
COL col659 P1_A_candidatas_sem_nova_devolucao: 17
COL col659 P1_B_candidatas_com_devolucao: 17
COL col659 P2_A_textos_divergentes_co

[truncado em 20000 de 36040 caracteres]

## 17/11 09:10 · sessao ebb638ddcccf · - · - · origem=Ronald (tmux)

Contagem dos rótulos `COL` positivos fornecidos: [nao verificado]

| Rótulo | COL positivos | Soma das perguntas | `NUMERO` declarado |
|---|---:|---:|---:|
| `P1_A_candidatas_sem_nova_devolucao` / `P1_B_candidatas_com_devolucao` | 118 | 2145 | 2174 |
| `P2_A_textos_divergentes_conservados` / `P2_B_textos_divergentes_substituidos` | 64 | 191 | 226 |

As somas dos `COL` diferem dos `NUMERO` declarados em 29 para P1 e 35 para P2; a causa não aparece no dado. [nao verificado]

## 17/11 09:10 · sessao af563760d743 · - · - · origem=Ronald (tmux)

P1: **118 col** com valor positivo; soma dos `COL`: **2145**. O `NUMERO P1_A_candidatas_sem_nova_devolucao` fornecido é **2174**. [nao verificado]

P2: **64 col** com valor positivo; soma dos `COL`: **191**. O `NUMERO P2_A_textos_divergentes_conservados` fornecido é **226**. [nao verificado]

As somas por `COL` não coincidem com os respectivos `NUMERO` no dado transcrito. [nao verificado]

## 17/11 09:04 · sessao 29e3e1889e9d · E4C-F2 · verificador · origem=Code

**[bloqueia] A obra conserva uma validação que a ressuscitação acabou de zerar.** O próprio teste exige projeção sem validação, leitura validada, `estado == VALIDADA` e exclusão de `q_vivas` após ressuscitar. Isso contraria a conservação da fila indicada na porta. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:413-428`; `app/chamados/models.py:1640-1648`)

1. **Escritores de RESPOSTA e significado de `ato_motivo`.**

| Emissor | Motivo gravado | Tratamento de `validada_em` |
|---|---|---|
| `responder_pergunta_disputa_core` | Omitido → `''` | Zera a validação existente; salva projeção e registra RESPOSTA no mesmo atomic. (`app/chamados/models.py:4111`; `app/chamados/services/disputa_emissao.py:1494-1496`, `:1517-1526`) |
| `responder_automaticamente` | `resposta automatica (fato entregue por outro caminho)` | Preserva; salva somente texto e instante. (`app/chamados/services/disputa_emissao.py:1674-1679`) |
| `carimbar_resposta_pela_trilha` | `pela trilha: %s` | Preserva; salva somente instante. (`app/chamados/services/disputa_emissao.py:1696-1700`) |
| `linhas_da_projecao_sem_linha` | `projecao sem linha na sequencia (anterior a E1 ou carimbo sem ato)` | A geração da foto não altera a projeção; o efeito depende do chamador. (`app/chamados/models.py:4146`, `:4171-4179`) |

Os caminhos adicionais pedidos se distribuem assim:

- **Nascimento:** registra as fotos das assinaturas recebidas, conservando os valores inseridos. (`app/chamados/models.py:3151-3156`)
- **Backfill E4b:** o ramo `registrar_decisao` grava as fotos pelo helper; esse ramo não limpa a validação. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:138-141`)
- **`backfill_decisoes_pergunta`:** `linhas_a_criar` apenas consulta o helper; a escrita explícita de decisões encontrada no apply é SILENCIO. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:44-48`, `:67`, `:324-327`)
- **`curar_perguntas_em_limbo`:** chama a trilha com `corte 13/09: sem marca de correcao no fio (nota #%d)`; portanto a RESPOSTA recebe esse texto precedido de `pela trilha:`. A limpeza das demais perguntas usa reabertura. (`app/chamados/management/commands/curar_perguntas_em_limbo.py:67-69`; `app/chamados/services/disputa_emissao.py:1700`)
- **`foto_e4b`:** fotografa decisões em um dicionário; não cria RESPOSTA nesse caminho. (`app/chamados/services/foto_e4b.py:247-252`, `:280-281`)
- **Migração 0051:** transforma `RespostaAnterior` em `DecisaoPergunta`, classificando o acervo como REABERTURA. (`app/chamados/migrations/0051_decisao_pergunta.py:41-61`)
- **Admin:** nenhum escritor adicional localizado; ausência absoluta de caminho administrativo indireto [nao verificado].

Entre os três emissores diretos, não encontrei a combinação inversa à regra nova. Entretanto, **há RESPOSTA COM motivo em ato que zera a validação**: as fotos históricas acompanham tanto a resposta humana quanto devoluções e ressuscitações. Logo, “com motivo preserva V no mesmo ato” não vale como afirmação geral. (`app/chamados/services/disputa_emissao.py:1480`, `:1494`, `:1525`; `app/chamados/services/validacao.py:272-282`; `app/chamados/models.py:1629-1648`)

**[bloqueia] A ressuscitação é a exceção funcional:** grava fotos RESPOSTA/VALIDACAO e limpa a projeção sem registrar ato limpador. O replay reaplica a VALIDACAO; `estado` retorna VALIDADA e os filtros deixam de trazer a pergunta. O C6 continua lendo a projeção, criando desacordo entre a pergunta julgada viva e seus coletores SQL. (`app/chamados/models.py:1629-1648`, `:4226-4234`, `:3180-3183`, `:352-355`, `:3557-3560`; `app/chamados/juizes.py:1389`, `:1403-1406`)

**[fila] `ato_motivo` acumula explicação e controle de efeito.** O replay usa sua ausência como comando para desfazer V; a face SQL reproduz essa convenção. O campo permanece texto livre e `registrar` não valida esse protocolo. (`app/chamados/models.py:4201`, `:3496`, `:4111`, `:4248`)

Os demais leitores encontrados usam motivo como explicação no histórico **não-RESPOSTA**, ou reconhecem BUG-PAR-103 em DEVOLUCAO. Não demonstram interpretação conflitante de uma mesma RESPOSTA. (`app/templates/chamados/partials/modal_fio.html:209-212`; `app/chamados/services/materializacao.py:275`)

2. **Cache: a correção cobre a mesma instância, não toda identidade.**

**[fila] Outra instância da mesma pk, ou um queryset com prefetch já avaliado, pode continuar lendo a sequência velha:** `registrar` invalida somente `ln['pergunta']`; `leitura` aceita as linhas de `.decisoes.all()` sem verificar atualização. Linhas somente com `pergunta_id` não invalidam instância alguma. (`app/chamados/models.py:4251-4254`, `:4217`)

Quem passa IDs inclui:

| Caminho | Evidência |
|---|---|
| Todas as fotos produzidas pelo helper | `app/chamados/models.py:4173`, `:4177` |
| Nascimento e ressuscitação, por esse helper | `app/chamados/models.py:3156`, `:1629-1630` |
| Backfill E4b | `app/chamados/management/commands/backfill_fio_morto_e4b.py:141` |
| Reparo L130 | `app/chamados/services/validacao.py:436-440` |
| `reabrir_para_correcao`, diretamente | `app/chamados/services/disputa_emissao.py:1631-1648` |

Nos fluxos examinados, não encontrei uma leitura subsequente **comprovadamente cacheada e velha**: recusa carrega a pergunta sem prefetch; rejeição de ausência segue pelas disputas; limbo termina no relatório; o modal reconstrói o contexto, e a leitura posterior de estado na validação vem após `refresh_from_db()`. (`app/chamados/services/validacao.py:894-895`, `:925-953`; `app/chamados/services/materializacao.py:1118`, `:1153-1175`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:69-70`; `app/chamados/services/fio.py:540-544`, `:659-660`)

Portanto: a possibilidade estrutural existe; ocorrência concreta no mesmo fluxo produtivo fica **[nao verificado]**. O teste novo cobre somente a mesma instância recebida pela porta. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:248-264`)

3. **Correspondência entre leitura e porta.**

**[nota] Sim, usam o mesmo corpo e a mesma chave:** `(pergunta_id, ato, ato_em)`. Além disso, o `ja` da memória também filtra exclusivamente RESPOSTA/VALIDACAO; não há a diferença sugerida de incluir todos os atos. Texto e motivo não participam da correspondência. (`app/chamados/models.py:4165-4179`, `:4218-4222`)

**[fila] A equivalência depende de memória completa e atual.** Cache velho ou parcial muda o conjunto `ja` e omite atos do replay; o corpo único não resolve a atualização dos insumos. (`app/chamados/models.py:4217-4222`, `:4251-4254`)

4. **Mordida dos testes e justificativa do teto.** [nao verificado]

- **[nota] C6 mantém a mordida pedida:** a face entra na varredura; retirar sua participação elimina `validada_em` do conjunto e é cobrado pela asserção. A sentinela também mantém a ligação do scanner ao selo. (`app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:248-266`, `:288-293`)
- **[nota] Respondedores:** `PAROU_EM=0`, quatro declarados e contagens exatas `1/1/2/1`; continuam os mutantes de respondedor novo, leitura adicional em declarado e declaração morta. (`app/chamados/tests/test_e3_catraca_respondedores.py:53-85`, `:577-602`)
- **[nota] Delegação e rótulo:** o sentinela do juiz foi conservado; a fixture do rótulo passou a fornecer um par legal e mantém as asserções valor→rótulo. (`app/chamados/tests/test_f1nova_b2_leitores.py:87-97`; `app/chamados/tests/test_valor_rotulo.py:8-21`)
- **[nota] Falsos sem banco:** acrescentam relação vazia e autor nulo para alimentar a leitura real, conservando o passivo como insumo. (`app/chamados/tests/test_e4b_c4v14_memoria.py:47-53`; `app/chamados/tests/test_e4b_classe_e_memoria.py:282-287`; `app/ponto/tests/test_supra_juiz_executor.py:25-40`)
- **[nota] C12:** permanecem nove identidades nominais e exceções vazias. (`app/chamados/tests/e4b_c12_ast.py:18-29`; `app/chamados/tests/test_e4b_c12_selo.py:34-36`)
- **[bloqueia] Há selo que aceita a divergência:** a caracterização da ressuscitação exige justamente o resultado incompatível com o reset. O controle vizinho usa fixture sem validação e não cobre essa classe. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:388-428`)

**[nota] O custo adicional é por carga de conjunto, não por pergunta:** existe uma chamada de Prefetch para a lista inteira de modelos com pk. Lista vazia ou cache já preenchido pode custar zero. (`app/chamados/models.py:1943-1947`)

A atribuição a `pode_fechar`, `fechar` e CSF é compatível com os caminhos, mas o CSF chama o corpo para **dois universos**. Assim, “+3” é específico do cenário medido, não uma identidade geral por chamada externa. Confirmação dos valores 223/283: **[nao verificado]**. (`app/chamados/reconciliador.py:613-614`; `app/chamados/models.py:1960`, `:1993`; `app/ponto/supra_juiz.py:573-574`)

5. **Checklist das classes de reprovação.**

| Classe | Resultado |
|---|---|
| Segundo juiz | **[nota]** A face SQL está expressamente autorizada como face, e `respondida` delega a C21. O desacordo C6/SQL aparece na ressuscitação descrita acima. (`app/docs/CORTES.md:487`; `app/chamados/models.py:3731`; `app/chamados/juizes.py:1404`) |
| Segundo escritor fora da porta | **[nota]** Nenhum identificado; a persistência permanece em `registrar`, com selo de escritor único. (`app/chamados/models.py:4248`; `app/chamados/tests/test_e1_entradas.py:217-225`) |
| Relógio fora da borda | **[nota]** Os corpos novos de correspondência/replay usam instantes recebidos, sem ler relógio. (`app/chamados/models.py:4149-4180`, `:4204-4235`) |
| Derivação paralela | **[bloqueia]** Correspondência foi unificada, mas projeção/C6 e replay/SQL discordam após reset real. (`app/chamados/models.py:1647`, `:3558`; `app/chamados/juizes.py:1389`) |
| Selo/RED sem mordida | **[bloqueia]** As adaptações examinadas conservam suas cobranças; a caracterização aceita a regressão da ressuscitação. (`app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:262-293`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:413-428`) |
| Ordem de trava | **[nota]** Nenhuma inversão identificada no delta; ressuscitação e reabertura adquirem a ordem antes de registrar. (`app/chamados/models.py:1628-1630`; `app/chamados/services/disputa_emissao.py:1629-1631`) |
| Campo com dois significados | **[fila]** Motivo livre também decide se a RESPOSTA revoga validação, sem protocolo imposto na porta. (`app/chamados/models.py:4111`, `:4201`, `:4248`) |

Verificação executada: 29 testes AST dos selos de respondedores/C12 passaram; os dois testes do escritor único também passaram por extração em memória. Suíte com banco, comparação integral `708162b15..b67ebb3d3` e números de produção ficam **[nao verificado]**. (`app/chamados/tests/test_e3_catraca_respondedores.py:577`; `app/chamados/tests/test_e4b_c12_selo.py:21`; `app/chamados/tests/test_e1_entradas.py:217-225`)

## 17/11 09:03 · sessao 50f0e05fe967 · ENSAIO-DA-FOLHA-11 · verificador · origem=Code

AVAL-DEVOLVIDO — somente item 4. (`respostas_1711_0900.txt:9`, anexo fornecido.)

*Nas referências aos anexos, conto as linhas do conteúdo colado, começando em “aval Ronald”.*

1. **As premissas conferem parcialmente; aplica-se o ramo assíncrono.** O POST chama `pedir`, registra o clique e chama `disparar`; este executa outro `manage.py` por `subprocess.Popen`, que consulta o lote, assume e gera o caderno. (`app/relatorios/views.py:1227`; `app/relatorios/views.py:1238`; `app/relatorios/views.py:1246`; `app/relatorios/lote_espelho.py:127`; `app/relatorios/lote_espelho.py:132`; `app/relatorios/management/commands/gerar_cartoes_em_lote.py:44`; `app/relatorios/management/commands/gerar_cartoes_em_lote.py:57`.)

   As escritas identificadas são:

   | Destino | Efeito |
   |---|---|
   | `LoteEspelho` | Criação, PID/início, estado, progresso, resultado ou erro. Até o polling pode atualizar um lote cujo processo morreu. (`app/relatorios/lote_espelho.py:103`; `app/relatorios/lote_espelho.py:138`; `app/relatorios/lote_espelho.py:161`; `app/relatorios/lote_espelho.py:203`; `app/relatorios/lote_espelho.py:251`; `app/relatorios/lote_espelho.py:264`; `app/relatorios/views.py:1272`; `app/relatorios/lote_espelho.py:75`.) |
   | `LogAuditoria`, via `observ` | Clique, disparo, resultado/falha e deduplicação. A persistência é *best-effort*: pode falhar sem interromper a geração. (`app/relatorios/views.py:1238`; `app/relatorios/views.py:1251`; `app/relatorios/lote_espelho.py:140`; `app/relatorios/lote_espelho.py:254`; `app/relatorios/lote_espelho.py:267`; `app/core/observ.py:105`; `app/core/observ.py:134`; `app/core/observ.py:139`.) |
   | `RegistroPDFAuditavel` | Registro por espelho que percorre a geração auditável. (`app/relatorios/services.py:237`; `app/relatorios/services.py:70`; `app/relatorios/pdf_espelho.py:1103`; `app/relatorios/pdf_base.py:152`.) |
   | Arquivos | Log `MEDIA_ROOT/cartoes_lote/<id>.log`, com stdout/stderr do filho; PDF em `cartoes_lote/`. Há também configuração de logs em `/app/logs/errors.log` e `/app/logs/ponto.log`. (`app/relatorios/lote_espelho.py:124`; `app/relatorios/lote_espelho.py:130`; `app/relatorios/lote_espelho.py:132`; `app/relatorios/lote_espelho.py:250`; `app/relatorios/models.py:125`; `app/config/settings/saas.py:139`; `app/config/settings/saas.py:163`.) |

   **Savepoint + raise no pai não desfaz tudo.** Por inferência da execução de outro Django em outro processo, a transação do pai não abrange os commits da conexão do filho; tampouco desfaz os arquivos. Se o pedido ainda estiver sem commit, o filho pode consultar e não encontrar o lote. (`app/relatorios/lote_espelho.py:132`; `app/relatorios/lote_espelho.py:250`; `app/relatorios/management/commands/gerar_cartoes_em_lote.py:44`.)

   **Existe caminho com escrita pela sombra, mas não pela sonda de leitura:** `sombra.sh --rodar` executa o script com as credenciais da casa e settings que fixam banco `sombra` em `sombra_db`; `sonda_leitura --sombra` mantém a role `leitor`, transações inicialmente em leitura e GRANTs de SELECT. Credenciais e permissões efetivas atuais: **[nao verificado]**. (`bin/sombra.sh:183`; `bin/sombra.sh:487`; `bin/sombra.sh:490`; `app/config/settings/saas.py:87`; `app/config/settings/sombra.py:47`; `app/config/settings/sombra.py:49`; `bin/sonda_leitura.sh:42`; `bin/sonda_leitura.sh:142`; `bin/sonda_leitura.sh:178`.)

   **O PDF não chega automaticamente à pasta do ensaio.** O download apenas devolve `FileResponse`; na porta da sombra, `/app/media` é tmpfs e o container usa `--rm`. O destino persistente disponível é `/sombra`, correspondente a `logs/sombra`. Falta explicitar a espera pelo filho e a transferência do PDF antes de encerrar o container; o mecanismo concreto está **[nao verificado]**. (`app/relatorios/views.py:1311`; `bin/sombra.sh:81`; `bin/sombra.sh:183`; `bin/sombra.sh:189`; `bin/sombra.sh:190`.)

   **Há leitores na esteira.** A completude conta todas as tabelas do tenant; o incremental executa máscara e selo que consultam campos das três tabelas. O cron da sombra executa `--refazer && --bloco`, cujo bloco deriva dos comandos diários. (`bin/sombra.sh:313`; `bin/sombra.sh:204`; `bin/sombra.sh:206`; `bin/sombra_regras_pessoais.py:72`; `bin/sombra_regras_pessoais.py:73`; `bin/sombra_regras_pessoais.py:93`; `bin/sombra_regras_pessoais.py:131`; `bin/sombra_regras_pessoais.py:147`; `bin/sombra_regras_pessoais.py:157`; `bin/sombra_selo_pessoal.py:74`; `app/config/crons.py:350`; `app/config/crons.py:1919`.)

   Há também DIFF que alcança `LogAuditoria` pela carga da tarefa, filtrando ações de `PerguntaDisputa`, e cron que consulta ações de `ChamadoColaborador`. Isso prova leitura da tabela, sem provar dependência das novas linhas `cartao_lote*`. Outros leitores são deduplicação, painel/download e verificação pública do PDF. Scripts concretos usados em `--rodar` e interferência real durante o ensaio: **[nao verificado]**. (`app/chamados/management/commands/diff_e3_sombra.py:134`; `app/chamados/services/tarefa_do_dia.py:534`; `app/chamados/services/carga_pergunta_viva.py:110`; `app/chamados/management/commands/auditar_invariantes_chamados.py:122`; `app/relatorios/lote_espelho.py:96`; `app/relatorios/views.py:1308`; `app/chamados/views.py:1210`; `bin/sombra.sh:490`.)

2. **A permissão posterior resolve a proibição original apenas no escopo declarado de B1.** O ENSAIO original proibia tocar a sombra única além de leitura; o item 4 posterior permite expressamente pedido, auditoria e PDF. Isso não amplia a autorização para produção ou outras alterações. (`ensaio_folha_11_1711.txt:16`; `respostas_1711_0900.txt:5`; `respostas_1711_0900.txt:8`; `CLAUDE.md:37`.)

   **Continua pendente a compatibilização com a ZONA INVIOLÁVEL:** a regra vigente exige smoke em banco lateral `juliani_ref_*` dentro de `sombra_db`, `atomic()+raise` e contagem antes/depois. O item 4 propõe a sombra compartilhada e limpeza posterior para geração assíncrona; não explicita a exceção a essas condições. POST real em produção continua sendo escrita. (`CLAUDE.md:205`; `respostas_1711_0900.txt:5`.)

   BANCO SOMBRA, máscara e CPUSET continuam aplicáveis: a porta usa settings da sombra e CPUSET de teste; `sombra_db.sh` também aplica/confere o CPUSET do banco. Apenas `nice 10` não demonstra isolamento de CPU. Configuração efetiva de uma execução futura: **[nao verificado]**. (`CLAUDE.md:87`; `CLAUDE.md:206`; `bin/sombra.sh:183`; `bin/sombra_db.sh:64`; `bin/sombra_db.sh:92`; `app/relatorios/lote_espelho.py:127`.)

   A máscara da sombra tampouco substitui a raspagem exigida por B1: o gerador imprime os campos nome e CPF; B1 exige `colNNN` e CPF removido. (`app/relatorios/pdf_espelho.py:739`; `ensaio_folha_11_1711.txt:8`.)

3. **Três condições não fecham como garantia literal.**

   **Limpeza:** apagar o lote não apaga suas auditorias nem seus registros auditáveis: `LogAuditoria` usa modelo textual/ID inteiro; `RegistroPDFAuditavel` não tem FK para lote. O CASCADE existente é da empresa para seus lotes. A limpeza precisa identificar separadamente registros e arquivos; a exclusão do lote, por si, não demonstra remoção do PDF/log. (`app/core/models.py:297`; `app/core/models.py:298`; `app/relatorios/models.py:43`; `app/relatorios/models.py:50`; `app/relatorios/models.py:106`; `app/relatorios/models.py:125`; `app/relatorios/lote_espelho.py:130`.)

   “Linhas criadas” também não cobre alterações anteriores: `pedir` pode reutilizar lote existente e o vigia pode atualizar lote antigo. Falta um procedimento demonstrado que distinga esses casos e espere o subprocesso terminar antes da limpeza. (`app/relatorios/lote_espelho.py:96`; `app/relatorios/lote_espelho.py:98`; `app/relatorios/lote_espelho.py:75`; `app/relatorios/lote_espelho.py:132`.)

   **Sem DIFF em curso:** o juiz existente consulta a ocupação de `logs/sombra.lock`; erro de consulta conta como ocupado. A exclusão efetiva exige manter a trava durante geração, espera, retirada do PDF e limpeza. Uma foto anterior não reserva a sombra; além de DIFF, há refazimento/incremental e bloco usando o recurso. Estado atual: **[nao verificado]**. (`bin/disjuntor.py:405`; `bin/disjuntor.py:416`; `bin/disjuntor.py:419`; `bin/sombra_trava.sh:28`; `bin/sombra.sh:438`; `bin/sombra.sh:451`; `bin/sombra.sh:487`.)

   **Auditável:** qualquer exceção nesse ramo aciona o informacional; datas explícitas pulam diretamente para ele. O informacional incrementa `n_ok`, e o lote pode terminar `CONCLUIDO`. Portanto “gerados/concluído” não comprova B1; deve-se conferir o fallback, que o aval mantém como falha. Pode ainda sobrar registro auditável de tentativa que falhou depois da criação, pois a segunda renderização ocorre posteriormente. (`app/relatorios/services.py:235`; `app/relatorios/services.py:244`; `app/relatorios/services.py:247`; `app/relatorios/services.py:252`; `app/relatorios/lote_espelho.py:251`; `respostas_1711_0900.txt:5`; `app/relatorios/pdf_base.py:152`; `app/relatorios/pdf_base.py:178`.)

4. **Há efeitos fora da enumeração declarada no caminho existente de escrita.** Antes do script B1, `--rodar` chama `lavra_de_prod`: grava `lavra_prod.json` e `repor_lavra.py` e, em outra execução, faz dois `update_or_create` em `MetricaSnapshot`. Pode modificar registros preexistentes; apagar apenas linhas criadas pelo lote não restaura isso, e o savepoint do script começa depois. (`bin/sombra.sh:489`; `bin/sombra.sh:144`; `bin/sombra.sh:151`; `bin/sombra.sh:157`; `bin/sombra.sh:160`; `bin/sombra.sh:166`; `bin/sombra.sh:490`.)

   Isso não prova alteração de dinheiro ou competência exportada. A lavra continua sendo lida para pautas; o código atual registra que dinheiro deixou de depender dela e o fechamento consulta `escala_certa_no_dia`. Alteração real de `FechamentoMensal`, exportação ou pagamento neste ensaio: **[nao verificado]**. A permissão de B1 não libera os `!` de exportada, escala/vínculo, arquivos usados por produção ou atalhos. (`app/ponto/services/esmeril_espelho.py:345`; `app/ponto/services/esmeril_espelho.py:358`; `app/ponto/services/esmeril_espelho.py:351`; `app/ponto/services/fechamento.py:928`; `CLAUDE.md:265`; `CLAUDE.md:411`.)

VEREDITO: item 4 PROCEDE -- faltam fechar a exceção do POST, a exclusão durante todo o job, a transferência/limpeza dos artefatos e as escritas adicionais de MetricaSnapshot. (`CLAUDE.md:205`; `bin/sombra_trava.sh:28`; `bin/sombra.sh:189`; `bin/sombra.sh:157`; `bin/sombra.sh:160`.)

## 17/11 09:02 · sessao 5bee4a00eef2 · - · - · origem=Ronald (tmux)

**O caminho `--rodar` tem efeitos adicionais que o item 4 não declara:** antes do script B1, chama `lavra_de_prod`; essa função lê o snapshot mais recente de produção, grava `logs/sombra/lavra_prod.json` e `repor_lavra.py`, e executa outro container que faz dois `update_or_create` em `MetricaSnapshot`. (`bin/sombra.sh:489`; `bin/sombra.sh:141`; `bin/sombra.sh:144`; `bin/sombra.sh:151`; `bin/sombra.sh:157`; `bin/sombra.sh:160`; `bin/sombra.sh:166`)

**“Registrar linhas criadas e apagar” não cobre esse efeito:** `update_or_create` pode atualizar snapshots existentes; o savepoint dentro do script B1 começa depois da chamada e não inclui a execução anterior do importador. Para retornar ao estado anterior seria necessário identificar/restaurar também registros modificados, além dos criados e arquivos. (`bin/sombra.sh:157`; `bin/sombra.sh:160`; `bin/sombra.sh:489`; `bin/sombra.sh:490`)

**Não atribuir mudança de dinheiro atual a essa lavra:** os comentários antigos de `sombra.sh` ainda dizem que ela decide HE100, mas o serviço atual registra que, desde 24/10, dinheiro deixou de ler a lavra; o fechamento consulta `escala_certa_no_dia` pela competência do dia. A lavra continua sendo autoridade de leitura para pautas/lista e sua guarda da sombra. (`bin/sombra.sh:127`; `app/ponto/services/esmeril_espelho.py:250`; `app/ponto/services/esmeril_espelho.py:289`; `app/ponto/services/esmeril_espelho.py:345`; `app/ponto/services/fechamento.py:928`; `app/ponto/services/fechamento.py:934`)

**A interação compartilhada é concreta:** `pautas_do_esmeril.sincronizar` lê a lavra e, com `apply`, abre/fecha pautas; `linhas_para_a_pauta` também lê o snapshot. Logo atualizar a lavra excede os registros administrativos do lote e afeta uma fonte compartilhada, embora a execução desses leitores durante o ensaio esteja [nao verificado]. (`app/ponto/services/pautas_do_esmeril.py:57`; `app/ponto/services/pautas_do_esmeril.py:69`; `app/ponto/services/pautas_do_esmeril.py:75`; `app/ponto/services/esmeril_espelho.py:358`)

**A cópia da lavra não passa novamente pela máscara:** copia/importa `detalhe` integral, sem chamada de máscara/selo nessa função. O gerador atual da lavra inclui PKs, contagens, frases de assinatura e destinos de propostas; isso não prova presença de nome/CPF no snapshot real fornecido ao script. Conteúdo real e eventual dado pessoal nessa cópia estão [nao verificado]. (`bin/sombra.sh:141`; `bin/sombra.sh:144`; `bin/sombra.sh:155`; `bin/sombra.sh:163`; `bin/sombra.sh:166`; `app/ponto/services/esmeril_espelho.py:222`; `app/ponto/services/esmeril_espelho.py:227`; `app/ponto/services/esmeril_espelho.py:239`)

**Limite da máscara:** a lista JSON inventariada contempla `LogAuditoria` e `LoteEspelho`, mas não `MetricaSnapshot`; o detector nominal de colunas pessoais não inclui `detalhe`. Portanto selo pessoal verde não demonstra raspagem desse JSON da lavra. (`bin/sombra_regras_pessoais.py:144`; `bin/sombra_regras_pessoais.py:158`; `bin/sombra_regras_pessoais.py:161`; `bin/sombra_selo_pessoal.py:30`; `bin/sombra_selo_pessoal.py:82`)

## 17/11 09:01 · sessao 0b97f3872cfe · - · - · origem=Ronald (tmux)

- **A própria rodada da sombra lê essas tabelas.** `--refazer --incremental` copia somente quatro tabelas de ponto/escala, mas depois roda máscara e selo pessoais; o inventário inclui `LoteEspelho.erro/placar`, `LogAuditoria.ip/descricao/valor_antes/valor_depois` e `RegistroPDFAuditavel.ip_origem`. A máscara percorre linhas de texto/JSON e o selo consulta esses campos. (`bin/sombra_incremental.sh:33`; `bin/sombra.sh:204`; `bin/sombra_regras_pessoais.py:72`; `bin/sombra_regras_pessoais.py:93`; `bin/sombra_regras_pessoais.py:131`; `bin/sombra_regras_pessoais.py:147`; `bin/sombra_regras_pessoais.py:157`; `bin/sombra_mascara_pessoal.py:51`; `bin/sombra_selo_pessoal.py:55`; `bin/sombra_selo_pessoal.py:74`)

- **Há leitura genérica de todas as tabelas:** a completude faz `count(*)` em cada `BASE TABLE` de `juliani`, portanto alcança essas três quando presentes no schema; `refazer()` chama essa conferência. O refazimento restaura dumps completos de `public` e `juliani`, sem excluir essas tabelas. (`bin/sombra.sh:298`; `bin/sombra.sh:308`; `bin/sombra.sh:313`; `bin/sombra.sh:244`; `bin/sombra.sh:257`)

- **O cron da sombra é `--refazer && --bloco`.** O bloco deriva os comandos diários de `config/crons.py`, exceto estágios infra/contínuo, e inclui encadeados; portanto não basta procurar o nome das três tabelas no shell. (`app/config/crons.py:350`; `app/config/crons.py:1919`; `app/config/crons.py:1934`; `app/config/crons.py:1947`)

- **Um cron alcançado pelo bloco lê `LogAuditoria`:** `auditar_invariantes_chamados`, declarado às 07:18, consulta `modelo='ChamadoColaborador'` e `acao='retratacao_retida_irma_orfa'`. Isso prova leitura da tabela, não consumo das novas ações `cartao_lote*`. (`app/config/crons.py:542`; `app/chamados/management/commands/auditar_invariantes_chamados.py:121`)

- **Há DIFF que pode ler `LogAuditoria` transitivamente:** `diff_e3_sombra` chama alimentação/tarefa; a tarefa alcança `carga_da_vida`, que consulta ações `retratar_pergunta_obsoleta` de `PerguntaDisputa`. Não há nessa consulta dependência das ações `cartao_lote*`. (`app/chamados/management/commands/diff_e3_sombra.py:133`; `app/chamados/management/commands/diff_e3_sombra.py:145`; `app/chamados/services/tarefa_do_dia.py:485`; `app/chamados/services/tarefa_do_dia.py:490`; `app/chamados/services/carga_pergunta_viva.py:110`)

- **`--rodar` não tem conjunto fixo de leitores:** executa o script fornecido em `/sombra`, depois de importar a lavra de produção. Os leitores dependem desse script; scripts efetivamente usados pela esteira não foram fornecidos. (`bin/sombra.sh:483`; `bin/sombra.sh:489`; `bin/sombra.sh:490`; [nao verificado])

- **Outros leitores reais:** pedido/deduplicação lê `LoteEspelho`; progresso e download leem o lote; verificação pública por SHA lê `RegistroPDFAuditavel`. (`app/relatorios/lote_espelho.py:96`; `app/relatorios/lote_espelho.py:98`; `app/relatorios/views.py:1288`; `app/relatorios/views.py:1308`; `app/chamados/views.py:1209`)

- **“Sem DIFF em curso” não está demonstrado pelo dado.** A exclusão oficial é obter e manter `flock` em `logs/sombra.lock`, com herança pelos filhos; o código proíbe adivinhar disponibilidade por `pgrep`. O DIFF de folha segura a mesma trava durante as duas fotos e comparação. A trava precisa abranger geração, espera pelo job, retirada do PDF e limpeza para impedir intercalação; observar ausência num instante não reserva o recurso. (`bin/sombra_trava.sh:2`; `bin/sombra_trava.sh:5`; `bin/sombra_trava.sh:21`; `bin/sombra_trava.sh:28`; `bin/sombra_trava.sh:37`; `bin/simular_folha.sh:25`; `bin/simular_folha.sh:96`)

- **O status genérico da esteira não prova ausência de DIFF:** calcula pacotes vivos/trabalhando, sem distinguir estágio DIFF. Estado atual de processos, locks, sondas e DIFF: **[nao verificado]**. (`bin/esteira_status.sh:14`; `bin/esteira_status.sh:63`; `bin/esteira_status.sh:112`)

## 17/11 09:01 · sessao 5bee4a00eef2 · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 17/11 09:01 · sessao 3f68a35b23bb · - · - · origem=Ronald (tmux)

- **Assíncrono.** O POST chama `pedir`, grava auditoria e dispara subprocesso; `Popen` executa outro `manage.py`, com `nice 10` e `--schema`. O comando consulta o lote e assume/executa a geração. (`app/relatorios/views.py:1227`, `app/relatorios/views.py:1238`, `app/relatorios/views.py:1246`, `app/relatorios/lote_espelho.py:127`, `app/relatorios/lote_espelho.py:132`, `app/relatorios/management/commands/gerar_cartoes_em_lote.py:44`, `app/relatorios/management/commands/gerar_cartoes_em_lote.py:52`)

- **Escritas observadas:** `LoteEspelho` no pedido, PID, execução, progresso, resultado e falha; `LogAuditoria` no clique, disparo, resultado ou deduplicação; `RegistroPDFAuditavel` por PDF auditável. O polling também pode atualizar lote morto. (`app/relatorios/lote_espelho.py:103`, `app/relatorios/lote_espelho.py:138`, `app/relatorios/lote_espelho.py:161`, `app/relatorios/lote_espelho.py:203`, `app/relatorios/lote_espelho.py:251`, `app/relatorios/lote_espelho.py:264`, `app/relatorios/views.py:1238`, `app/relatorios/views.py:1251`, `app/core/observ.py:134`, `app/relatorios/pdf_base.py:152`, `app/relatorios/views.py:1272`, `app/relatorios/lote_espelho.py:75`)

- **Arquivos:** cria/abre em append `MEDIA_ROOT/cartoes_lote/<id>.log`, redireciona stdout/stderr do subprocesso e salva PDF no `FileField(upload_to='cartoes_lote/')`. O download é `FileResponse`, sem mecanismo automático de colocação na pasta do ensaio. (`app/relatorios/lote_espelho.py:124`, `app/relatorios/lote_espelho.py:130`, `app/relatorios/lote_espelho.py:132`, `app/relatorios/lote_espelho.py:250`, `app/relatorios/models.py:125`, `app/relatorios/views.py:1311`)

- **Rollback do pai não engloba tudo:** o subprocesso executa outro Django e as escritas de arquivos são explícitas; inferência: savepoint da conexão pai não desfaz os commits do filho nem arquivos. Se o lote ainda não estiver visível quando o filho consultar, o comando pode simplesmente dizer que não existe. (`app/relatorios/lote_espelho.py:127`, `app/relatorios/lote_espelho.py:132`, `app/relatorios/lote_espelho.py:250`, `app/relatorios/management/commands/gerar_cartoes_em_lote.py:44`)

- **Apagar somente o lote não limpa trilhas:** `LogAuditoria.modelo`/`objeto_id` são texto/inteiro; `RegistroPDFAuditavel` não possui FK para lote e tem `colaborador_id` inteiro. A FK CASCADE do lote aponta para empresa: a exclusão da empresa elimina lotes, não o contrário. Registro auditável protege a exclusão de seu usuário; lote usa usuário SET_NULL. (`app/core/models.py:297`, `app/core/models.py:298`, `app/relatorios/models.py:43`, `app/relatorios/models.py:50`, `app/relatorios/models.py:106`, `app/relatorios/models.py:37`, `app/relatorios/models.py:128`)

- **“Linhas criadas” é insuficiente:** `pedir` pode reutilizar lote preexistente e pode atualizar lote antigo pelo vigia. A identificação dos PDFs auditáveis não tem vínculo estruturado com o lote; limpeza precisa distinguir registros próprios, alterações anteriores, arquivos e log. (`app/relatorios/lote_espelho.py:96`, `app/relatorios/lote_espelho.py:98`, `app/relatorios/lote_espelho.py:75`, `app/relatorios/models.py:30`)

- **B1 pode falhar com lote concluído:** qualquer exceção do auditável aciona informacional; datas explícitas pulam diretamente o auditável. Informacional incrementa `n_ok`; resultado pode ser `CONCLUIDO`, com tipo de erro apenas quando existem `pulados`. Logo, “concluído/gerados” não comprova B1 auditável. (`app/relatorios/services.py:235`, `app/relatorios/services.py:244`, `app/relatorios/services.py:247`, `app/relatorios/services.py:250`, `app/relatorios/services.py:252`, `app/relatorios/lote_espelho.py:247`, `app/relatorios/lote_espelho.py:251`)

- **Falha tardia pode deixar registro:** o registro auditável é criado antes da segunda renderização do PDF; inferência: falha nessa renderização pode deixar registro e provocar fallback no serviço. A auditoria `observ` também é best-effort e pode devolver `None` sem interromper o fluxo. (`app/relatorios/pdf_base.py:152`, `app/relatorios/pdf_base.py:178`, `app/relatorios/services.py:244`, `app/core/observ.py:105`, `app/core/observ.py:139`)

- **Outros leitores concretos:** painel/polling/download leem `LoteEspelho`; verificação pública consulta `RegistroPDFAuditavel` por hash. (`app/relatorios/views.py:1291`, `app/relatorios/views.py:1308`, `app/chamados/views.py:1210`, `app/chamados/views.py:1214`)

## 17/11 09:00 · sessao c5918a93429a · SERIE-BLOQUEIO-FOLHA · sonda · origem=Code

Não executei `smk_lavras_11.py`: não localizei o arquivo na cópia. Execução e acesso a produção: [nao verificado]. Reproduzo literalmente o dado fornecido, sem validação independente [nao verificado]: [nao verificado]

```text
sonda_leitura: smk_lavras_11.py [04d2babc4a78503d] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

BANCO saas_hasner AGORA 2026-11-17 08:59
TOTAL_RETRATOS_11_2026 4
EMP 1 lavrado_em=2026-11-17 06:46:09 linhas=3
EMP 2 lavrado_em=2026-11-17 06:46:09 linhas=453
EMP 3 lavrado_em=2026-11-17 06:46:09 linhas=115
EMP 4 lavrado_em=2026-11-17 06:46:09 linhas=24
POR_DATA_DE_LAVRA {'2026-11-17': 4}
RESTRICAO_UNICA ['retrato_folha_um_por_empresa_competencia']
FIM_OK
```

LAVRAS ANTERIORES A 17/11: nao -- POR_DATA_DE_LAVRA {'2026-11-17': 4}; somente nos retratos atuais de 11/2026 do dado fornecido [nao verificado].

`RetratoFolha` guarda **somente a última lavra por empresa/competência**: a chave única é `(empresa, mes, ano)` em [app/folha/models.py:366](app/folha/models.py:366), e o lavrador substitui `linhas` e `lavrado_em` via `update_or_create` em [app/folha/management/commands/lavrar_retrato_folha.py:77](app/folha/management/commands/lavrar_retrato_folha.py:77).

Não identifiquei outra tabela de histórico de lavras do retrato na cópia; o código documenta que preservar duas fotos exige novo modelo e migration em [app/folha/retrato_folha.py:725](app/folha/retrato_folha.py:725). `ExportacaoDominio` preserva gerações de TXT, não versões de `RetratoFolha`: [app/folha/models.py:174](app/folha/models.py:174).

Portanto, a saída não permite concluir que **nunca houve** lavras anteriores a 17/11; sua existência histórica é [nao verificado].

## 17/11 08:59 · sessao 9853300ba392 · - · - · origem=Ronald (tmux)

[nota] Não identifiquei enfraquecimento nas adaptações examinadas: C6 continua cobrando campos lidos, delegação continua exigindo o sentinela do juiz, e a fixture do rótulo mantém o objetivo de conferir a tradução valor→rótulo com par legal. (`app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:248-293`; `app/chamados/tests/test_f1nova_b2_leitores.py:87-97`; `app/chamados/tests/test_valor_rotulo.py:8-21`)

- **C6:** retirar `q_validada_pelo_ato` da varredura é mordido por `test_a_costura_MORDE`, que exige `validada_em` no conjunto; a sentinela também exige que o scanner de filtros entre no resultado. (`app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:253-266`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:288-293`)
- Demonstração em memória: extração AST com metadados simulados passou nos seis testes; mutação retirando somente a face da chamada em `_lidos_pelo_c6` produziu uma falha: `validada_em not found`. Execução da suíte Django real: [nao verificado]. [nao verificado]
- **Respondedores:** `PAROU_EM=0`, quatro declarados e contagem exata por declarado permanecem; os mutantes de respondedor novo, leitura adicional em declarado e declaração morta continuam cobrados. (`app/chamados/tests/test_e3_catraca_respondedores.py:53-85`; `app/chamados/tests/test_e3_catraca_respondedores.py:544-602`)
- Medição AST sem banco: zero respondedores, declarados com contagens 1/1/2/1, divergências `{}`. Os 29 testes de `test_e3_catraca_respondedores` e `test_e4b_c12_selo` passaram, com bytecode desativado. Resultado de execução sem arquivo de prova: [nao verificado]. [nao verificado]
- **C12:** as nove identidades atualizadas apontam para as chamadas reais; o selo mantém corpo com nove membros e exceções vazias. (`app/chamados/tests/e4b_c12_ast.py:18-29`; `app/chamados/models.py:205`; `app/chamados/models.py:302`; `app/chamados/models.py:458`; `app/chamados/models.py:663`; `app/chamados/models.py:843`; `app/chamados/models.py:861`; `app/chamados/models.py:1497`; `app/chamados/models.py:1701`; `app/chamados/models.py:1721`; `app/chamados/tests/test_e4b_c12_selo.py:34-36`)

[nota] A justificativa do teto corresponde a **consultas por chamada com conjunto ainda não carregado**, sem consulta por pergunta: o Prefetch recebe uma lista inteira; vazio ou cache previamente preenchido pode custar zero. A alegação literal “1 SELECT por chamada” exige essa ressalva. (`app/chamados/models.py:1941-1947`)

[nota] O CSF chama o corpo duas vezes, para universos diferentes; portanto não se deve interpretar sua chamada externa como exatamente um SELECT adicional em qualquer cenário. Os caminhos `pode_fechar` e `fechar` também consultam o corpo separadamente. (`app/ponto/supra_juiz.py:573-574`; `app/chamados/models.py:1960`; `app/chamados/models.py:1993`; `app/chamados/reconciliador.py:613-614`)

[nota] O **+3 e os valores 223/283 efetivamente medidos** permanecem [nao verificado], pois não houve leitura de logs nem execução com banco.

Comando utilizado, sem escrita de bytecode:

```bash
PYTHONDONTWRITEBYTECODE=1 PYTHONPATH=app python3 -B -m unittest chamados.tests.test_e3_catraca_respondedores chamados.tests.test_e4b_c12_selo
```

## 17/11 08:58 · sessao 73f8d70f028f · - · - · origem=Ronald (tmux)

(b) **[fila] Invalidação limitada à instância recebida.** `registrar` limpa somente `ln.get('pergunta')._prefetched_objects_cache['decisoes']`; não alcança outra instância da mesma pk nem linhas passadas apenas por ID. `leitura` confia integralmente em `p.decisoes.all()`, portanto outra instância previamente carregada pode conservar sequência velha. Isso é possibilidade demonstrável pela implementação; ocorrência em fluxo produtivo concreto permanece **[nao verificado]**. (`app/chamados/models.py:4217`, `app/chamados/models.py:4251`)

Quem produz/passsa `pergunta_id`:

- `linhas_da_projecao_sem_linha` produz sempre ID, tanto RESPOSTA quanto VALIDACAO; seus consumidores escritores incluem nascimento, ressuscitação, validação/devolução, resposta humana/automática e E4b. Os escritores que acrescentam uma linha `pergunta=p` invalidam essa mesma instância; os que registram somente o passivo não recebem essa proteção. (`app/chamados/models.py:4173`, `app/chamados/models.py:4177`, `app/chamados/models.py:3156`, `app/chamados/models.py:1629`, `app/chamados/services/validacao.py:173`, `app/chamados/services/validacao.py:279`, `app/chamados/services/validacao.py:354`, `app/chamados/services/validacao.py:439`, `app/chamados/services/disputa_emissao.py:1525`, `app/chamados/services/disputa_emissao.py:1678`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`)
- `reabrir_para_correcao` registra diretamente por ID e executa o UPDATE por outro queryset. (`app/chamados/services/disputa_emissao.py:1631`, `app/chamados/services/disputa_emissao.py:1648`)

Não encontrei leitura subsequente comprovadamente velha nos chamadores produtivos examinados:

- `recusar_pergunta` carrega instância sem prefetch; depois da reabertura usa foto, nota, evento e push, sem consultar novamente `decisoes`/`estado`. (`app/chamados/services/validacao.py:894`, `app/chamados/services/validacao.py:925`, `app/chamados/services/validacao.py:927`)
- Rejeição de ausência carrega perguntas sem prefetch; depois da reabertura usa suas disputas e a foto anterior. (`app/chamados/services/materializacao.py:1118`, `app/chamados/services/materializacao.py:1153`, `app/chamados/services/materializacao.py:1167`, `app/chamados/services/materializacao.py:1170`)
- `curar_perguntas_em_limbo` termina com o relatório após reabrir por queryset. (`app/chamados/management/commands/curar_perguntas_em_limbo.py:69`)
- O modal reconstrói o contexto após a ação; a leitura posterior de `_p.estado` no caminho de validação de disputa fechada vem depois de `refresh_from_db()`. (`app/chamados/services/fio.py:540`, `app/chamados/services/fio.py:659`)
- Upload de foto valida por ID, usando outra instância internamente, mas o fluxo seguinte atualiza a disputa e consulta novamente suas perguntas. (`app/api/foto_recebida.py:43`, `app/api/foto_recebida.py:51`, `app/api/foto_recebida.py:74`, `app/api/foto_recebida.py:81`)

(c) **[nota] A correspondência é a mesma, desde que as linhas em memória estejam completas e atuais.** Banco e memória restringem `ja` aos mesmos atos RESPOSTA/VALIDACAO; o único corpo compara exatamente `(pk, ato, ato_em)`, sem texto ou motivo. Outros atos presentes na sequência não alteram a correspondência. (`app/chamados/models.py:4165`, `app/chamados/models.py:4171`, `app/chamados/models.py:4175`, `app/chamados/models.py:4218`, `app/chamados/models.py:4222`)

**[fila] `ja` velho/parcial pode diferir do banco.** A leitura não verifica se o cache está atualizado: pode acrescentar uma foto sintética que o banco já possui, além de omitir atos novos. O delta protege a mesma instância recebida; proteção de outras instâncias permanece ausente. (`app/chamados/models.py:4217`, `app/chamados/models.py:4221`, `app/chamados/models.py:4251`)

Candidato de regressão independente do cache: ressuscitação preserva linhas antigas e zera a projeção sem novo ato que limpe a sequência; L130 também mantém a RESPOSTA e zera somente a projeção. A nova leitura reaplica as linhas antigas. O pai está examinando esse ponto. (`app/chamados/models.py:1629`, `app/chamados/models.py:1647`, `app/chamados/services/validacao.py:436`, `app/chamados/services/validacao.py:439`, `app/chamados/models.py:4226`)

## 17/11 08:58 · sessao 35b4f208b605 · - · - · origem=Ronald (tmux)

Inventário de RESPOSTA encontrado em produção:

| Escritor | Motivo | `validada_em` no ato |
|---|---|---|
| `responder_pergunta_disputa_core` | Omitido, portanto `''` | Zera quando preenchida; grava projeção e linha no mesmo `atomic`. `app/chamados/services/disputa_emissao.py:1484`, `:1494`, `:1517`, `:1522`, `:1525`; default em `app/chamados/models.py:4111`. |
| `responder_automaticamente` | `resposta automatica (fato entregue por outro caminho)` | Preserva; salva somente texto/instante. `app/chamados/services/disputa_emissao.py:1674`, `:1677`, `:1679`. |
| `carimbar_resposta_pela_trilha` | `pela trilha: %s` | Preserva; salva somente instante. `app/chamados/services/disputa_emissao.py:1696`, `:1698`, `:1700`. |
| Foto da projeção | `MOTIVO_PROJECAO_SEM_LINHA` | A foto isolada não altera projeção; gera RESPOSTA por correspondência. `app/chamados/models.py:4146`, `:4171`, `:4173`. |

Chamadores que merecem distinção:

- Nascimento com assinaturas recebidas usa a foto e conserva os valores: `app/chamados/models.py:3151`, `:3154`, `:3156`.
- `backfill_fio_morto_e4b` grava fotos sob trava sem mudar resposta/validação nesse ramo: `app/chamados/management/commands/backfill_fio_morto_e4b.py:138`.
- `backfill_decisoes_pergunta.linhas_a_criar` é DRY e consulta o helper; o apply explícito encontrado escreve SILENCIO: `app/chamados/management/commands/backfill_decisoes_pergunta.py:44`, `:67`, `:324`.
- `curar_perguntas_em_limbo` delega à trilha com motivo de corte e nota; a limpeza delega à reabertura: `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`.
- `foto_e4b` fotografa campos, inclusive decisões; sua função `guardar` apenas monta dicionário: `app/chamados/services/foto_e4b.py:247`, `:280`.
- A migração 0051 transforma o acervo de `RespostaAnterior` em REABERTURA, não RESPOSTA: `app/chamados/migrations/0051_decisao_pergunta.py:41`, `:49`, `:61`. A migração 0053 apenas verifica colisões: `app/chamados/migrations/0053_pergunta_unica_por_familia.py:36`.
- Nenhum cadastro administrativo de `DecisaoPergunta` foi encontrado; ausência em todos os caminhos possíveis [nao verificado].

**Exceção literal à frase “RESPOSTA com motivo preserva V no mesmo ato”:** a foto com motivo pode acompanhar um ato que limpa V. O core humano gera `_sem_linha` antes de zerar V e grava as fotos junto à resposta nova; devoluções fazem o mesmo junto à DEVOLUCAO. Não são respostas novas com semântica contraditória, mas fotos históricas no mesmo atomic: `app/chamados/services/disputa_emissao.py:1480`, `:1494`, `:1525`; `app/chamados/services/validacao.py:272`, `:273`, `:279`.

**[bloqueia] Ressuscitação sem ato que represente a limpeza.** A porta grava fotos RESPOSTA/VALIDACAO e depois zera instante/texto/validação, explicitamente sem REABERTURA: `app/chamados/models.py:1629`, `:1640`, `:1647`. A nova leitura só limpa V por DEVOLUCAO/RECUSA/REABERTURA ou RESPOSTA sem motivo; portanto conserva a VALIDACAO fotografada: `app/chamados/models.py:4187`, `:4201`, `:4227`. `estado` devolve VALIDADA após esse reset: `app/chamados/models.py:3180`. Há cenário concreto já na suíte: fixture com validação e instante sem texto, reset esperado, somente fotos R/V e nenhum ato limpador: `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:263`, `:270`, `:280`. A mordida antiga verifica projeção e trilha, mas não `estado`/face SQL após o reset: `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:275`. Impacto em produção [nao verificado].

**[fila] `ato_motivo` passou a acumular descrição e controle de efeito.** O template exibe o motivo como explicação, enquanto o replay e SQL usam sua presença para decidir se a RESPOSTA desfaz V: `app/templates/chamados/partials/modal_fio.html:212`; `app/chamados/models.py:4201`, `:3496`. Os escritores diretos atuais concordam com essa convenção, mas `registrar` aceita qualquer motivo e as constraints não impõem o protocolo: `app/chamados/models.py:4248`, `:4125`. O outro leitor de conteúdo usa prefixo BUG-PAR-103 apenas para DEVOLUCAO: `app/chamados/services/materializacao.py:275`.

**[nota] Comentário residual contradiz a regra nova.** Diz “salvo a foto da projeção”, mas automática e trilha também preservam V: `app/chamados/models.py:4183`, `:4197`.

_6419 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
