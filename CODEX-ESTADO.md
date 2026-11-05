# CODEX-ESTADO

> Ultima resposta processada: **05/11/2026 16:10** · publicado em **05/11/2026 16:10** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 05/11 16:10 · sessao e3563cda1ed4 · - · - · origem=Ronald (tmux)

1. **Competência 1–31 não é representável. Certeza alta; ESTRUTURA.** O modelo e formulário anunciam faixa 1–28, mas o POST converte 1 em 2; o juiz também limita a `[2,28]`. Contra-exemplo: empresa com `dia_inicio_competencia=1`, competência outubro/2026: `janela_fechamento(10,2026,empresa)` resulta **02/09–01/10**, não 01/10–31/10. Em 31/10, `janela_atual` dá **02/10–01/11**, identificando novembro. Referências: `app/colaboradores/models.py:46`; `app/templates/colaboradores/empresa_form.html:54`; `app/colaboradores/views.py:495-499`; `app/ponto/janelas.py:10-20`, `24-33`, `36-57`. O export delega ao juiz afetado em `app/folha/export.py:47-51`; fechamento usa a janela por colaborador em `app/ponto/services/fechamento.py:612`. Os testes de invariantes só exercitam cortes 2,15,21,28, sem 1: `app/ponto/tests/test_janelas_invariantes.py:7,17`. **Especulação: nenhuma sobre o resultado estático; ocorrência real não verificada.**

2. **Cron não incorpora automaticamente outro tenant nem empresa fora de 2/3/4. Certeza alta; ESTRUTURA.** Universo e schema são constantes: `EMPRESAS=(2,3,4)`, `SCHEMA='juliani'`; o render injeta esses valores. Contra-exemplo: tenant `cliente2`, empresa pk1: o crontab gerado continua `--schema=juliani`; mesmo mudando só o schema, entradas por empresa continuam 2,3,4. Referências: `app/config/crons.py:17-21`; `app/config/crons.py:1821-1826`; encadeados herdam universo em `app/config/crons.py:1795-1801`. Há ainda literal próprio no reconciliador: `app/config/crons.py:462`. Exemplo de rotina afetada por empresa: `reavaliar_ausencias_lancadas`, `app/config/crons.py:396`. **Especulação rotulada:** o crontab efetivamente instalado e os efeitos operacionais no segundo tenant são [não verificado].

3. **Piso global de operação Juliani corta histórico de outro cliente. Certeza alta, condicionada a importar histórico anterior; ESTRUTURA.** Data fixa: 21/05/2026, explicitamente descrita como go-live Grupo Juliani. Fechamento e previsto fazem `max(início solicitado, início vínculo, DATA_INICIO_OPERACAO)`. Contra-exemplo: novo tenant importa vínculo 5x2 desde 01/04/2026 e batidas 08/12/13/17 em 04/05/2026; a fatia do motor e o previsto excluem esse dia. Referências: `app/core/constants.py:3-5`; `app/ponto/services/fechamento.py:203-205`; `app/escala/alimentacao.py:386-392`. O piso de tela também nasce desse marco: `app/ponto/services/espelho.py:39-48`. **Especulação rotulada:** necessidade de importar histórico anterior e impacto monetário concreto são [não verificado].

4. **Cadastro incompleto de rubricas pode omitir eventos sem reter o colaborador por isso. Certeza alta; ESTRUTURA para o silêncio, CADASTRO para o mapeamento ausente.** `montar_linhas_export` ignora evento com valor quando não acha rubrica; o verificador de homologação também ignora o evento sem rubrica. Contra-exemplo: integração configurada, colaborador com código Domínio, espelho íntegro, fechamento com HE50=2h e atraso=1h, rubrica HE existente e rubrica atraso ausente: a montagem inclui só HE e não denuncia a hora de atraso ausente. Referências: `app/folha/export.py:428-441`; `app/folha/export.py:693-720`; `app/folha/export.py:958-965`; `app/folha/export.py:968-983`. Se catálogo de homologação está vazio, o gate inteiro fica desligado: `app/folha/export.py:972-974`. A view pode aprovar o fechamento antes de descobrir que nenhuma linha foi montada: `app/folha/views.py:244-252`, `268-273`. **Especulação rotulada:** inexistência dessas rubricas no novo tenant é [não verificado].

5. **Integração e código do empregado são pré-requisitos de cadastro, com bloqueio explícito. Certeza alta; CADASTRO.** Contra-exemplo A: empresa nova sem `EmpresaIntegracao(sistema='dominio', ativo=True)` → export gera `ValueError` instrutivo, tratado na view. Contra-exemplo B: colaborador novo sem `codigo_dominio` → classificado `fora/sem_codigo_dominio`. Referências: `app/folha/export.py:23-33`; `app/folha/views.py:134-142`; `app/folha/export.py:846-849`. Isso não demonstra defeito estrutural por si. Referências de cadastro por empresa: `app/folha/models.py:232-242`. **Especulação rotulada:** existência desses registros no tenant atual ou novo é [não verificado].

**Contrapesos e limites:** corte 21 é default, não literal universal nos principais leitores: export e fechamento delegam ao juiz por empresa (`app/folha/export.py:47-51`; `app/ponto/services/fechamento.py:612`). Sem 12x36, a trava de âncora é filtrada explicitamente por esse ciclo (`app/folha/export.py:786-793`), e supressão de feriado depende de `tipo_ciclo=='12x36'` (`app/folha/export.py:409-412`); estes trechos não sustentam dizer que todo cliente exige 12x36. Não executei testes nem acessei banco/log; qualquer contagem, ocorrência ou cadastro efetivo de produção é [não verificado].

## 05/11 16:10 · sessao 6fa29ea13aa0 · - · - · origem=Ronald (tmux)

Achados por inspeção estática; todos os contra-exemplos abaixo são **cenários construídos**, não ocorrências observadas em produção. O anexo fornecido não mostra cadastro, batidas nem estado de um segundo tenant. [não verificado]

1. **CCT com intervalo mínimo de 30 minutos não governa a validação da escala.** O modelo aceita `Sindicato.intrajornada_minima_min=30`, mas `TipoEscala.clean()` exige o piso fixo de 60 minutos quando a jornada supera seis horas, sem consultar a CCT. **Contra-exemplo:** empresa `cct`, aplicação vigente, sindicato com mínimo 30, escala 5x2/comercial 08:00–16:30, pausa 12:00–12:30: a validação recusa a escala por intervalo inferior a 60. **Certeza alta; dono ESTRUTURA.** Fontes: `app/core/models.py:63`, `app/core/models.py:85`, `app/escala/models.py:352`, `app/escala/models.py:593`, `app/escala/models.py:607`, `app/escala/models.py:17`. **Especulação:** a CCT real do segundo cliente conter essa cláusula é [não verificado].

2. **Sete parâmetros de CCT são armazenados, porém declaradamente não consumidos pelo cálculo.** Incluem tolerâncias, intrajornada, compensação e teto de prorrogação; a fábrica injeta quatro parâmetros, sem esses sete. **Contra-exemplo concreto:** sindicato com `tolerancia_marcacao_min=15`, escala 5x2 com entrada prevista 08:00, batida 08:12 e saída no marco: o julgador considera os 12 minutos atraso porque compara com a constante 10. O status da grade também usa essa constante. **Certeza alta; dono ESTRUTURA.** Fontes: `app/core/regua_cct.py:72`, `app/core/regua_cct.py:84`, `app/core/regua_cct.py:458`, `app/ponto/motor_calculo_v2.py:60`, `app/ponto/motor_calculo_v2.py:2006`, `app/escala/utils.py:306`. **Especulação:** frequência e impacto financeiro no segundo cliente são [não verificado].

3. **A CCT cadastrada depende de um switch adicional; sua ausência desliga a leitura da convenção.** `get_motor_cct()` somente ativa a régua quando encontra `ParametroSistema(chave='regua_cct_ativa', empresa=NULL, valor='1')`; ausente, retorna o motor puro. **Contra-exemplo:** segundo schema com empresa `cct`, aplicação/sindicato vigentes, `prorrogacao_noturna_pos5h=False`, escala 5x2 noturna e nenhum switch: a fábrica não consulta `regua_para`; o motor resolve a prorrogação pelo piso do ciclo, cujo resultado para ciclo diferente de 12x36 é `True`. **Certeza alta quanto ao comportamento condicional; dono CADASTRO para a linha faltante.** Fontes: `app/core/regua_cct.py:440`, `app/core/regua_cct.py:444`, `app/core/regua_cct.py:449`, `app/core/regua_cct.py:457`, `app/ponto/motor_calculo_v2.py:758`, `app/core/regua_cct.py:182`. **Não verificado:** existência desse switch no tenant atual ou num segundo tenant; não achei seu semeio no código inspecionado, o que não prova ausência no banco. [não verificado]

4. **Cadastrar sindicato e vínculo sindicato/praça não basta: a aplicação da convenção precisa existir, ter vigência e não empatar.** A seleção consulta `AplicacaoConvencao` da empresa; sem aplicação vigente, regime vazio ou empate, devolve régua legal identificando o motivo. **Contra-exemplo:** empresa 10 em `cct`, sindicato vigente vinculado à praça, colaborador e escala corretamente cadastrados, mas nenhuma `AplicacaoConvencao(empresa=10, ...)`: a CCT não é aplicada. Duas aplicações vigentes no mesmo nível também descartam ambas. **Certeza alta; dono CADASTRO.** Fontes: `app/core/regua_cct.py:253`, `app/core/regua_cct.py:261`, `app/core/regua_cct.py:265`, `app/core/regua_cct.py:363`, `app/core/regua_cct.py:366`, `app/core/regua_cct.py:371`. Há comando parametrizado para cadastrar a aplicação: `app/core/management/commands/semear_aplicacao_convencao.py:38`, `app/core/management/commands/semear_aplicacao_convencao.py:81`, `app/core/management/commands/semear_aplicacao_convencao.py:109`.

**Não sustentar “sem 12x36 quebra tudo”.** Existem motores explícitos para comercial, turno partido, plantão e intermitente; o feriado usa o ciclo, com exceção simples somente para `12x36`. Base desconhecida levanta `CicloSemMotor`, e ausência completa de escala usa geometria `Motor12x36`. Fontes: `app/ponto/motor_calculo_v2.py:3569`, `app/ponto/motor_calculo_v2.py:3599`, `app/ponto/motor_calculo_v2.py:3616`, `app/ponto/motor_calculo_v2.py:3627`, `app/ponto/motor_calculo_v2.py:3669`.

**Limites:** não verifiquei população de sindicatos, aplicações, escalas, switches, feriados ou perfis em qualquer banco; nem execução de testes, valores de folha ou falhas reais em outro cliente. [não verificado] [nao verificado]

## 05/11 16:10 · sessao c65824d4078e · - · - · origem=Ronald (tmux)

Achados de **ESTRUTURA**, com certeza **alta na leitura estática**; ocorrência em produção e quantidade de atingidos **[não verificado]**.

1. **PWA anuncia “Avisos ligados” mesmo quando o servidor recusou a assinatura.** O cliente aguarda `fetch` sem conferir status ou JSON e retorna `true` (`app/static/pwa.js:246-251`); o botão transforma isso em sucesso (`app/static/pwa.js:367-372`). A API pode devolver 400 ou 500 (`app/ponto/views.py:1981`, `app/ponto/views.py:1988`, `app/ponto/views.py:2000`). **Contra-exemplo concreto:** navegador consegue criar a assinatura, mas a gravação no servidor lança exceção: POST retorna 500, tela informa sucesso; como já existe assinatura local, o próximo carregamento pula a reinscrição (`app/static/pwa.js:335-337`). **Especulação rotulada:** uma falha transitória de gravação pode prolongar a ausência de canal; ocorrência real **[não verificado]**.

2. **Troca de usuário no mesmo PWA preserva a associação WebPush do usuário anterior.** Logout web apenas encerra a sessão (`app/core/views.py:79-84`); o cliente só registra no servidor quando `getSubscription()` devolve ausência (`app/static/pwa.js:335-337`). A associação com o colaborador muda apenas no POST de inscrição (`app/ponto/views.py:1989-1995`), e os envios selecionam assinaturas pelo colaborador gravado (`app/chamados/push_utils.py:24`). **Contra-exemplo concreto:** A inscreve o navegador, sai e B entra; a assinatura existente impede o POST de B, enquanto os avisos de A continuam tendo aquele endpoint como destinatário. Exibição efetiva de conteúdo de A durante sessão de B **[não verificado]**.

3. **Servidor envia destino e interação persistente; SW ignora os campos correspondentes.** O emissor envia `url` e `requireInteraction` (`app/chamados/push_utils.py:28-33`); o SW lê `data.urgente` para `requireInteraction` (`app/static/sw.js:97`) e o clique foca a primeira janela ou abre `/`, sem usar `data.url` (`app/static/sw.js:102-108`). **Contra-exemplo concreto:** aviso de holerite destinado a `/holerite/meus/`, emitido por `app/ponto/webpush.py:44-45`, abre `/` quando não há janela; aviso de turno `pos_20`, que pede `requireInteraction: true` e destino `/ponto/bater/`, perde ambos no consumidor (`app/ponto/management/commands/processar_alertas_turno.py:279-284`). Comportamento visual específico do navegador **[não verificado]**.

4. **Navegação offline não alimenta o cache prometido para dashboard e batida.** Requisições `mode === 'navigate'` retornam antes do trecho que grava `/ponto/bater/` e `/` (`app/static/sw.js:45-50`, `app/static/sw.js:67-78`). O cache de instalação contém apenas JS, manifesto e ícones (`app/static/sw.js:3-8`), e cache ausente retorna `Response.error()` (`app/static/sw.js:29-30`). **Contra-exemplo concreto:** instalação limpa, visita normal online à tela de ponto e depois tentativa de abri-la offline: aquela navegação não gravou o HTML. Execução em navegador **[não verificado]**.

**FCM:** a rota exige `fcm_token`, valida formato, transfere token repetido por colaborador e devolve `{status: 'ok'}` (`app/api/views_core.py:1014-1031`). O PWA mostrado utiliza WebPush (`app/static/pwa.js:239-249`). Não consegui confrontar registro, refresh, logout ou consumo de payload FCM com um cliente nativo: implementação nativa **[não verificado]**; entrega Firebase/APNs, credenciais e tokens efetivamente existentes **[não verificado]**. Não tratar ausência de FCM no PWA como defeito: o próprio fluxo implementado usa outra inscrição (`app/static/pwa.js:239-249`).

| achado | arquivo:linha | contra-exemplo | certeza | dono L-099 |
|---|---|---|---|---|
| Sucesso falso na inscrição push | `app/static/pwa.js:246`; `app/ponto/views.py:2000` | Assinatura local criada, POST retorna 500, tela anuncia sucesso | alta | ESTRUTURA |
| Associação push sobrevive à troca de conta | `app/static/pwa.js:335`; `app/core/views.py:79`; `app/ponto/views.py:1989` | A sai; B entra no mesmo navegador com assinatura de A | alta | ESTRUTURA |
| Destino e persistência do push divergentes | `app/chamados/push_utils.py:28`; `app/static/sw.js:97`; `app/static/sw.js:105` | Aviso pede tela específica e persistência; SW abre `/` e usa outro campo | alta | ESTRUTURA |
| HTML da tela de ponto não entra no cache de navegação | `app/static/sw.js:45`; `app/static/sw.js:67` | Instalação limpa, visita online, reabertura offline | alta | ESTRUTURA |

## 05/11 16:09 · sessao 2f123315a889 · CODEX-SUFOCA-14-portaria-671 · leitura · origem=Code

**Há portas que sobrescrevem ou apagam a batida original.** A regularização externa permite alterar `timestamp`/`tipo` da mesma linha ou executar `delete()`; portanto, ler a tabela atual não garante recuperar a marcação original. (`app/chamados/services/regularizacao_ext.py:136`, `app/chamados/services/regularizacao_ext.py:153`)

**A exibição lê o cru apenas em alguns caminhos.** “Últimas batidas” consulta diretamente a tabela, incluindo retratadas, limitada a seis registros; os espelhos de período usam uma fonte que exclui retratadas. (`app/ponto/views.py:191`, `app/ponto/services/espelho.py:1022`, `app/ponto/turnos.py:2864`)

Os contra-exemplos abaixo são **cenários sintéticos deduzidos do código**, não casos observados. O anexo não demonstra batidas, arquivos exportados ou comprovantes reais; incidência e quantidade afetada em produção são **[nao verificado]**.

As divergências são comparadas aos requisitos indicados no tema e aos contratos locais. O texto oficial e os anexos normativos não foram fornecidos; artigo aplicável, versão normativa e conclusão jurídica de infração são **[nao verificado]**. Não adotei hipóteses sobre quem pergunta.

Há proteções parciais: a retratação comum preserva a linha e anota `retratada_em`; `timestamp` não pertence aos campos permitidos por `anotar`. Essas proteções não abrangem as alterações diretas da regularização externa. (`app/ponto/registro_batida.py:203`, `app/ponto/registro_batida.py:267`, `app/chamados/services/regularizacao_ext.py:149`)

Não consegui verificar: completude de AFD/AEJ efetivamente emitidos, sequência global de NSR, comprovantes entregues pelo APK, certificação/registro do REP-P, atestado técnico, assinaturas digitais, sincronização do relógio e guarda efetiva dos registros. **[nao verificado]**

Na tabela, **alta** significa comportamento explícito no código; **média** identifica principalmente ausência de implementação localizada. Os donos indicam o problema de implementação: não atribuí defeito a cadastro ou batida de nenhum colaborador real. A classificação concreta por dia depende dos insumos do juiz L-099, ausentes nesta análise. (`CLAUDE.md:176`, `app/ponto/management/commands/e6_oraculo.py:53`)

| Achado | arquivo:linha | Contra-exemplo sintético | Certeza | Dono L-099 |
|---|---|---|---|---|
| **1. Regularização sobrescreve horário/tipo ou apaga a original.** A seleção do alvo não restringe sua origem. | `app/chamados/views_regularizacao.py:109`; `app/chamados/services/regularizacao_ext.py:51`; `app/chamados/services/regularizacao_ext.py:131`; `app/chamados/services/regularizacao_ext.py:149` | Batida `app` E08:00; supervisão seleciona `corrigir` para 08:30 e colaborador confirma, sem disputa aberta. A mesma linha passa a 08:30. Com `remover`, ela é excluída. | **Alta** | **ESTRUTURA** |
| **2. Rejeição de offline pendente executa DELETE físico.** Não é mera desconsideração anotada. | `app/ponto/views.py:456`; `app/ponto/views.py:477`; `app/ponto/registro_batida.py:285` | Existe batida `offline_pendente`, `aprovada=False`, às 08:00. Operador autorizado rejeita: a linha desaparece. Existência atual dessas linhas em produção **[nao verificado]**. | **Alta** | **ESTRUTURA** |
| **3. Flip altera E/S no próprio registro.** Preserva horário, mas não preserva o tipo original nessa linha. | `app/ponto/views.py:3253`; `app/ponto/services/flip_batida.py:15`; `app/ponto/services/flip_batida.py:25`; `app/ponto/registro_batida.py:254` | Batida original E18:00, não retratada, competência aberta; operador autorizado aciona flip. Mesma PK e horário passam a S. Se E/S integra o registro legal original ou apenas o tratamento: **[nao verificado]**. | **Alta** sobre alteração | **ESTRUTURA** |
| **4. Espelhos de período omitem retratadas**, contrariando a promessa local de continuar mostrando com asterisco. O feed cru mostra as seis últimas, mas sem sinalizar retratação. | `app/ponto/models.py:64`; `app/ponto/services/espelho.py:1022`; `app/ponto/turnos.py:2903`; `app/api/views.py:1369`; `app/relatorios/pdf_espelho.py:596`; `app/templates/ponto/espelho.html:76` | E08:00/S17:00; retratar E08:00. A entrada permanece armazenada, porém sai dos conjuntos usados pelo espelho de período, API auditada e PDF. | **Alta** | **ESTRUTURA** |
| **5. A área “Batidas” da tela pode apresentar o tipo do marco da grade.** O aviso de divergência não substitui a apresentação do tipo registrado. | `app/escala/utils.py:1123`; `app/templates/ponto/espelho.html:231`; `app/templates/ponto/espelho.html:237`; `app/api/views.py:1471`; `app/relatorios/pdf_espelho.py:881` | Marco E08:00, realizado às 08:00 com `tipo_real='S'`. A tela mostra seta E com aviso; API/PDF usam o tipo S da batida. Enquadramento como infração normativa isolada **[nao verificado]**. | **Alta** sobre apresentação | **ESTRUTURA** |
| **6. Tentativas distintas podem ser suprimidas por proximidade temporal.** O dedup da API também encontra retratadas. A porta comum recusa opostas próximas conforme escala/marcos. | `app/api/views.py:557`; `app/ponto/services/triagem_batida.py:668`; `app/ponto/registro_batida.py:21`; `app/ponto/registro_batida.py:138` | E08:00 retratada; nova tentativa E08:03 retorna a PK antiga com sucesso, sem novo registro. Outro cenário: E08:00 e S08:00:30, sem marco próprio que excepcione a colisão, não criam a segunda batida. **Especulação:** tratar uma tentativa real distinta como reenvio perde esse novo fato; sua natureza em produção **[nao verificado]**. | **Alta** sobre supressão | **ESTRUTURA** |
| **7. Existem bloqueios antes da gravação por GPS e situação cadastral.** | `app/api/views.py:415`; `app/api/views.py:449`; `app/api/views.py:462` | Colaborador autenticado envia E válida sem latitude/longitude: recebe 403 `gps_obrigatorio`. Com `situacao='ferias'`, recebe 403 antes da gravação. Incidência e enquadramento jurídico específicos **[nao verificado]**. | **Alta** | **ESTRUTURA** |
| **8. Horário offline fora de 48 horas é substituído pelo horário de chegada.** O horário lido do aparelho fica em campo separado. | `app/api/hora_do_aparelho.py:63`; `app/api/hora_do_aparelho.py:141`; `app/api/hora_do_aparelho.py:148`; `app/api/views.py:684`; `app/api/views.py:695` | Batida feita segunda às 08:00 sincroniza quinta às 08:00: `timestamp` recebe quinta; `timestamp_dispositivo` conserva segunda. Autenticidade do relógio do aparelho e conformidade desse tratamento **[nao verificado]**. | **Alta** sobre transformação | **ESTRUTURA** |
| **9. AFD completo não demonstrado; gerador AEJ não localizado.** O glossário declara AFD pendente. A rota AEJ encontrada é de importação e responde 410; o TXT da folha é Domínio. | `app/templates/core/glossario.html:155`; `app/ponto/views.py:57`; `app/ponto/views.py:70`; `app/folha/urls.py:13` | Empresa com jornada E08:00/S17:00 e ajuste manual solicita AFD e AEJ do período: nenhum gerador correspondente foi localizado nesta cópia. Completude, leiaute, assinatura e exportador externo **[nao verificado]**. | **Alta** sobre declaração/410; **média** sobre ausência global | **ESTRUTURA** |
| **10. `sequencia` reinicia por jornada; não demonstra NSR sequencial.** | `app/ponto/models.py:25`; `app/ponto/turnos.py:2365`; `app/ponto/turnos.py:2373`; `app/api/views.py:688` | Após encerrar uma jornada, a primeira batida de outra recebe novamente 1. Dois colaboradores sem turno aberto também recebem 1. Outro mecanismo de NSR global **[nao verificado]**. | **Alta** sobre reinício | **ESTRUTURA** |
| **11. Retorno de sucesso auditado não contém hash, NSR ou comprovante assinado.** | `app/api/views.py:823`; `app/api/views.py:834` | Batida online válida com GPS recebe ID, tipo, horário, sequência, mensagem e avisos; esse retorno não entrega hash nem documento verificável. Emissão posterior pelo APK ou serviço externo **[nao verificado]**. | **Alta** no retorno auditado | **ESTRUTURA** |
| **12. A trilha não garante reconstrução das alterações/exclusões.** A escrita precede a auditoria em algumas portas; falhas da auditoria são capturadas. | `app/ponto/registro_batida.py:254`; `app/ponto/registro_batida.py:263`; `app/ponto/registro_batida.py:291`; `app/core/auditoria.py:17`; `app/chamados/services/regularizacao_ext.py:107` | Alteração/exclusão ocorre e a auditoria falha antes de persistir, por exemplo ao montar o evento. O tratamento da exceção permite continuar sem essa linha de trilha. Falha concreta em produção **[nao verificado]**. | **Alta** sobre tratamento da falha | **ESTRUTURA** |
| **13. Documentação identifica registrador por programa como REP-C.** Não sustenta a classificação REP-P solicitada no tema. | `app/templates/core/glossario.html:144`; `app/templates/core/glossario.html:145`; `app/templates/core/glossario.html:149` | Usuário consulta a classificação do aplicativo e recebe “REP-C — Registrador Eletrônico de Ponto por Programa”. Classificação jurídica efetiva, registro e certificação do produto **[nao verificado]**. | **Alta** sobre texto; conformidade **[nao verificado]** | **ESTRUTURA** |

## 05/11 16:09 · sessao e96b2974cdbc · C20-O169 · sonda · origem=Code

**O banco não recebe a distinção entre janela, ausência de ata e lastro medido nesse caminho.** O coletor reduz os três casos ao mesmo booleano; o ato grava apenas transição de estado e via. Portanto, o total histórico atribuído exatamente à rotina e sua divisão causal devem sair como **“nao medido”**; registros compatíveis podem ser contados como candidatos (`app/ponto/services/cluster_espurio.py:50`, `app/chamados/models.py:275-278`). Números de PROD: [nao verificado].

1. O caminho de fechamento é este; o aval manda remover o fechamento por janela (`app/docs/BACKLOG.md:424-426`).

| Etapa | Comportamento na cópia | Fonte |
|---|---|---|
| Cron direto do detector | `06:24`, `--alarme`: somente vigia; não fecha. | `app/config/crons.py:598`; `app/ponto/management/commands/detectar_cluster_espurio.py:34-56` |
| Comando manual | `detectar_cluster_espurio --apply` chama `reconciliar_colab`; `--alarme` impede aplicação mesmo acompanhado de `--apply`. `--dias` pode alterar a janela. | `app/ponto/management/commands/detectar_cluster_espurio.py:25-46` |
| Cron do cartório | `processar_cartorio --apply`, início às `06:27` por empresa, chama `julgar_colab`. | `app/config/crons.py:616`; `app/ponto/management/commands/processar_cartorio.py:38-65` |
| Signals | Salvar/apagar batida ou ausência e salvar justificativa passam por `_julgar_dias → julgar_celula`. Criar chamado ou mudar seu estado também pode chamar `julgar_celula`. | `app/ponto/signals.py:85-93`, `:131-165`; `app/chamados/signals.py:164-180` |
| Entrada comum | `julgar_celula → julgar_colab`; depois do julgamento, havendo aplicação e células julgadas, chama `_cluster(c, hoje)`. | `app/ponto/services/cartorio.py:1988-1991`, `:981-994` |
| Coleta | `reconciliar_colab → pendencias → dias_com_cluster → _vivos_que_a_premissa_matou`. Exige colaborador ativo e escala ativa sem fim. | `app/ponto/services/cluster_espurio.py:73-82`, `:91-95` |
| Ato | Para cada `a_fechar`, chama `ch.reconciliar()`; conta somente retorno `True`. | `app/ponto/services/cluster_espurio.py:97-99` |

Também chegam ao julgamento: pós-vínculo, regeneração, defesa do vínculo, reconciliador C2, cobrança manual, cobrança do passivo e vigia de hora; todos ficam sujeitos à mesma condição final do cartório (`app/colaboradores/services/pos_vinculo.py:72`; `app/ponto/portas/celula.py:372`; `app/escala/regua_defesa.py:692`; `app/chamados/reconciliador.py:671`; `app/chamados/services/cobrar_dia.py:39`; `app/ponto/services/furos_sem_cobranca.py:128`; `app/ponto/services/vigia_de_hora.py:131`; `app/ponto/services/cartorio.py:981-994`). Há ainda comandos de rejulgamento/cura que entram por essa porta (`app/ponto/management/commands/rejulgar_ausencia_em_folga.py:78`; `app/ponto/management/commands/curar_residuo_bug115.py:143`; `app/ponto/management/commands/celula_veredito_velho.py:144`).

A janela padrão contém **hoje e os 13 dias anteriores**. A seleção dos chamados vivos, porém, não tem esse filtro de data: qualquer dia forte ausente de `datas` recebe `FATO=False`. Isso abrange dia antigo, futuro, célula inexistente, ata ausente e dia efetivamente observado sem assinatura; chamado sem dia forte é pulado (`app/ponto/services/cluster_espurio.py:12`, `:21-35`, `:45-51`, `:73-82`). Assim, julgar outra célula do colaborador pode disparar a tentativa de fechamento do chamado antigo (`app/ponto/services/cartorio.py:994`; `app/ponto/services/cluster_espurio.py:45-50`).

O ato passa pelo motor e pelas guardas; quando permitido, `RECONCILIAR` leva a `resolvido`, pela via `sistema` (`app/chamados/models.py:607-621`, `:637-659`; `app/chamados/catalogo/motor.py:95-96`, `:111`). Grava `status_local`, `resolvido_em`, `via_encerramento`, `atualizado_em` e eventual rebaixamento de urgência; esse chamador não altera `respondido_por` (`app/chamados/models.py:222-240`, `:620`).

A trilha é `LogAuditoria`, ação `estado_chamado`, objeto igual ao ID do chamado, antes com estado anterior e depois com estado novo e via. **Não carrega fato, motivo, janela, ata ou identificador da rotina** (`app/chamados/models.py:275-278`). O autor vem da request ou de `sistema:<comando>`: identifica o contexto do processo, não a rotina interna (`app/core/observ.py:22-32`, `:108-123`).

O corpo do cluster **não anexa Followup/nota de fechamento** (`app/ponto/services/cluster_espurio.py:95-99`; `app/chamados/models.py:607-621`). Outro caminho, fechamento pela disputa, também chama `reconciliar()` e acrescenta nota própria; logo, `via=sistema` não comprova origem cluster (`app/chamados/signals.py:41-45`). Os campos de autoria/conteúdo/data de Followup não acrescentam um motivo que esse caminho deixou de escrever (`app/chamados/models.py:1587-1610`).

**Não há distinção histórica segura entre aqueles motivos nessa trilha.** A ata atual é substituível, e o evento `veredito_celula` tampouco guarda seu conteúdo completo (`app/ponto/portas/celula.py:743-749`, `:775-788`). Além disso, a auditoria é best-effort e a reabertura limpa `resolvido_em`/via; nem ausência de trilha nem campos atuais provam ausência de fechamento (`app/core/observ.py:94-105`, `:124-139`; `app/chamados/models.py:242-243`).

2. A sonda abaixo conta **registros candidatos**, mantendo os totais exatos do caminho como “nao medido”, conforme a trilha disponível (`app/chamados/models.py:275-278`). Usa 30 datas civis incluindo hoje: **07/10/2026 a 05/11/2026**, em São Paulo — convenção explícita da sonda.

Para a simulação de hoje, mantém a coleta de 14 dias e conserva como `None` aquilo que ficou fora dela ou não possui os insumos da ata. A ata produzida pelo cartório contém `orfas` e `lampadas`; uma ata incompleta também é tratada conservadoramente (`app/ponto/services/cluster_espurio.py:12`, `:21-34`; `app/ponto/services/cartorio.py:704-711`). Aqui, `None` é o **valor do fato não coletado**; o catálogo responde conservando a premissa viva (`app/chamados/catalogo/premissa.py:127-130`).

As idades históricas abaixo usam o **dia atualmente resolvido pelo juiz**, pois o evento de fechamento não grava o dia original; não constituem prova desse vínculo no passado (`app/chamados/juizes.py:78-119`; `app/chamados/models.py:275-278`). A aplicação efetiva da regra nova em PROD permanece [nao verificado].

```python
import json
import logging
from collections import defaultdict
from datetime import date, datetime, time, timedelta
from zoneinfo import ZoneInfo

from django.db import connection, transaction
from django.utils import timezone
from chamados.catalogo.motor import VIVOS
from chamados.juizes import dia_do_fato
from chamados.models import ChamadoColaborador
from core.models import LogAuditoria
from escala.models import CelulaDia
from ponto.services.cluster_espurio import MODULO_ORIGEM, JANELA_DIAS

TZ = ZoneInfo("America/Sao_Paulo")
HOJE = date(2026, 11, 5)
# Convencao: 30 datas civis, incluindo 05/11.
FIM = datetime.combine(HOJE + timedelta(days=1), time.min, tzinfo=TZ)
INI = FIM - timedelta(days=30)
INI_COLETA = HOJE - timedelta(days=JANELA_DIAS - 1)
NM = "nao medido"

# Evita que avisos sobre contexto legado imprimam dados ou gravem logs.
logging.disable(logging.CRITICAL)


def motivos(n):
    return {
        "fora_da_janela": NM,
        "sem_ata": NM,
        "lastro_medido": NM,
        "indistinguivel": n,
    }


def medir():
    with timezone.override(TZ), transaction.atomic():
        with connection.cursor() as cur:
            cur.execute(
                "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
            )
            cur.execute("SELECT current_user")
            if cur.fetchone()[0] != "leitor":
                raise RuntimeError("role inesperada")

        # Trilha generica: app/chamados/models.py:275-278.
        # Universo de CANDIDATOS; origem da rotina nao comprovada.
        universo = ChamadoColaborador.objects.filter(
            modulo_origem=MODULO_ORIGEM
        ).values("pk")

        eventos = list(LogAuditoria.objects.filter(
            modelo="ChamadoColaborador",
            acao="estado_chamado",
            criado_em__gte=INI,
            criado_em__lt=FIM,
            objeto_id__in=universo,
            valor_antes__status_local__in=VIVOS,
            valor_depois__status_local="resolvido",
            valor_depois__via="sistema",
        ).values("id", "objeto_id", "criado_em").order_by("id"))

        chamados = {
            ch.pk: ch
            for ch in ChamadoColaborador.objects.filter(
                pk__in={e["objeto_id"] for e in eventos}
            ).select_related("celula").only(
                "id", "colaborador_id", "modulo_origem",
                "contexto_json", "celula",
            )
        }

        # Mesmo juiz/escada curta do coletor:
        # app/ponto/services/cluster_espurio.py:47-49.
        dias = {
            pk: dia_do_fato(ch, fortes=True)
            for pk, ch in chamados.items()
        }

        celulas = {
            (c["colaborador_id"], c["data"]): c["ata"]
            for c in CelulaDia.objects.filter(
                colaborador_id__in={
                    ch.colaborador_id for ch in chamados.values()
                },
                data__range=(INI_COLETA, HOJE),
            ).values("colaborador_id", "data", "ata")
        }

        def nao_coletado_hoje(ch):
            d = dias[ch.pk]
            if d is None:
                return "sem_dia"

            # Conservacao simulada, mantendo a janela de coleta atual:
            # app/ponto/services/cluster_espurio.py:12,21-24.
            if not INI_COLETA <= d <= HOJE:
                return "fora_da_janela"

            ata = celulas.get((ch.colaborador_id, d))
            if not ata:
                return "sem_ata"

            # Insumos do detector e formato produzido pelo cartorio:
            # app/ponto/services/cluster_espurio.py:30-34;
            # app/ponto/services/cartorio.py:704-711.
            if (
                not isinstance(ata, dict)
                or not isinstance(ata.get("orfas"), list)
                or not isinstance(ata.get("lampadas"), list)
            ):
                return "ata_incompleta"
            return None

        conservados = {
            pk for pk, ch in chamados.items()
            if nao_coletado_hoje(ch) is not None
        }

        grupos = defaultdict(lambda: {
            "eventos": set(),
            "chamados": set(),
            "conservados": set(),
        })

        for e in eventos:
            pk = e["objeto_id"]
            ch = chamados[pk]
            d = dias[pk]
            idade_hoje = (HOJE - d).days if d is not None else NM
            idade_evento = (
                (timezone.localdate(e["criado_em"], TZ) - d).days
                if d is not None else NM
            )
            g = grupos[(ch.modulo_origem, idade_evento, idade_hoje)]
            g["eventos"].add(e["id"])
            g["chamados"].add(pk)
            if pk in conservados:
                g["conservados"].add(pk)

        saida = [{
            "inicio_inclusivo": INI.isoformat(),
            "fim_exclusivo": FIM.isoformat(),
            "hoje": HOJE.isoformat(),
            "fechamentos_exatos_deste_caminho": NM,
            "chamados_deste_caminho_com_fato_None_hoje": NM,
            "registros_candidatos": len(eventos),
            "motivos_historicos_dos_candidatos": motivos(len(eventos)),
            "chamados_candidatos": len(chamados),
            "chamados_candidatos_com_fato_None_hoje": len(conservados),
        }]

        for (modulo, idade_evento, idade_hoje), g in sorted(
            grupos.items(), key=lambda x: str(x[0])
        ):
            saida.append({
                "modulo_origem": modulo,
                "idade_do_dia_hoje_dias": idade_hoje,
                "idade_na_data_do_registro_com_dia_atual_dias": idade_evento,
                "fechamentos_exatos_deste_caminho": NM,
                "motivos_historicos_dos_candidatos": motivos(
                    len(g["eventos"])
                ),
                "chamados_candidatos": len(g["chamados"]),
                "chamados_candidatos_com_fato_None_hoje": len(
                    g["conservados"]
                ),
            })

        # Zero de registros observados nao prova zero de fechamentos:
        # app/core/observ.py:94-105,124-139.
        return saida


try:
    resultado = medir()
except Exception:
    # Nao imprime excecao/contexto potencialmente identificavel.
    resultado = [{"medicao": NM}]

for linha in resultado:
    print(json.dumps(linha, ensure_ascii=False))
```

O agrupamento por módulo terá somente `cluster_batida_espuria`, porque a própria seleção desse caminho restringe esse módulo (`app/ponto/services/cluster_espurio.py:11`, `:45-46`). Os totais de chamados são distintos dentro de cada grupo; reaberturas podem produzir registros em grupos históricos diferentes (`app/chamados/catalogo/motor.py:111-113`).

O Python fornecido usa consultas e controle de transação. O **wrapper**, contudo, escreve credencial, temporário e trilha operacional; executá-lo não satisfaz literalmente uma proibição de qualquer escrita em arquivo (`bin/sonda_leitura.sh:126-134`, `:216-218`, `:248-250`). Execução desta sonda em PROD: [nao verificado].

3. **Quem mais lê o retorno e quais testes existem:** [nao verificado]

| Leitor | Uso | Fonte |
|---|---|---|
| `pendencias` | Publica o retorno como `a_fechar`. | `app/ponto/services/cluster_espurio.py:79-82` |
| `reconciliar_colab` | Consome `a_fechar` e tenta reconciliar. | `app/ponto/services/cluster_espurio.py:95-99` |
| Comando DRY/alarme | Consome `a_fechar` como pendência `premissa_morta`; alarme sai com código 2. | `app/ponto/management/commands/detectar_cluster_espurio.py:50-64` |

Os testes existentes encontrados cobrem: [nao verificado]

- Fechamento depois de **retratar batidas**, fazendo cair a assinatura: `app/ponto/tests/test_varredura_cluster_evento.py:50-62`.
- Vigia sem escrita, alarme e reconciliação da pendência: `app/ponto/tests/test_varredura_cluster_evento.py:64-84`.
- Cron configurado como vigia, sem `--apply`: `app/ponto/tests/test_varredura_cluster_evento.py:87-95`.
- Contador respeitando o retorno do gate, com `pendencias` e `reconciliar` mockados: `app/chamados/tests/test_fechar3_retorno_do_gate.py:205-216`.
- Detecção da rajada e exclusão de turno normal: `app/ponto/tests/test_detectar_cluster_espurio.py:47-74`.
- Uso do juiz do dia e preservação da assinatura na parcial enviada à lista: `app/chamados/tests/test_c_miudos_1.py:38-44`; `app/ponto/tests/test_cobparc7_cluster_le_a_lista.py:31-43`.

**Teste específico que fixe fechamento por idade/fora da janela/sem ata: [nao verificado].** Os testes de fechamento e contador acima não demonstram essa cobertura: um usa retratação real; o outro substitui a coleta por mock (`app/ponto/tests/test_varredura_cluster_evento.py:54-62`; `app/chamados/tests/test_fechar3_retorno_do_gate.py:214-216`).

## 05/11 16:09 · sessao d4d8f156ded4 · - · - · origem=Ronald (tmux)

Quatro achados fortes para consolidar, todos de **ESTRUTURA de custo**; os cadastros abaixo são contraexemplos sintéticos, sem ocorrência em produção verificada. [não verificado]

1. **Cartório consulta ausência por célula e percorre todo o histórico mesmo quando nada mudou.** O comando seleciona todas as células `data<hoje`, sem limite inferior, e faz `Colaborador.get` + consulta de células por pessoa: `app/ponto/management/commands/processar_cartorio.py:49`, `:59`, `:60`, `:61`. No serviço, o laço chama `ausencia_cobre` antes da guarda que pula impressão inalterada: `app/ponto/services/cartorio.py:1303`, `:1306`, `:1358`, `:1362`; essa autoridade executa `.first()` por chamada: `app/ponto/turnos.py:2552`.
   - **Certeza alta:** pelo menos `N×D` chamadas SQL de ausência no ramo comum, além das consultas por colaborador; ausência inexistente também consulta. `app/ponto/services/cartorio.py:1306`; `app/ponto/turnos.py:2557`.
   - **Contraexemplo sintético:** colaboradores com primeira batida existente, 365 células passadas já julgadas, impressão inalterada e nenhuma ausência: a execução seguinte continua buscando ausência para cada célula. `app/ponto/services/cartorio.py:1154`, `:1303`, `:1306`, `:1362`.
   - **Estimativa:** D=30 → 90.000/150.000 consultas de ausência para 3.000/5.000 pessoas; D=365 → 1.095.000/1.825.000. Cálculo derivado dos laços citados; tempos reais [não verificado].
   - **CPU adicional confirmado:** `d0/d1` abrangem a primeira/última célula; `bs` carrega batidas dessa janela e cada célula refaz uma compreensão sobre **todo `bs`**: `app/ponto/services/cartorio.py:1082`, `:1107`, `:1108`, `:1123`, `:1307`. Complexidade `O(N×D×B)`; **inferência condicional**, com B proporcional a D, `O(N×D²)`. Multiplicar D de 30 para 365 representa aproximadamente 148× comparações, mantendo densidade de batidas.
   - **Mitigações existentes:** impressão evita rejulgamento/escritas, mas tarde para esses custos; cadastro e turnos têm caches por colaborador: `app/ponto/services/cartorio.py:1091`, `:1102`, `:1362`. Existe autoridade de cobertura por período já disponível: `app/ponto/turnos.py:2733`, `:2753`.
   - **Agenda:** diário 06:27, `--apply`, por empresa: `app/config/crons.py:616`.

2. **Gerador possui leitura em lote, mas mantém consulta de folga por vínculo/dia e escrita individual.** O lote de 500 existe para `CelulaDia`: `app/ponto/portas/celula.py:582`, `:586`, `:587`. Porém o laço chama `eh_dia_trabalho_calculado(d)` **sem `folgas`**, antes das guardas de célula existente/editada/passada: `app/ponto/portas/celula.py:589`, `:591`, `:598`, `:599`, `:606`.
   - **Certeza alta:** sem alimentação, a função faz `FolgaDia.exists()` mensal por dia; havendo foto do mês, faz outra consulta para a data: `app/escala/models.py:1280`, `:1286`, `:1309`.
   - **Contraexemplo sintético:** 3.000/5.000 vínculos 12×36 com âncora válida, sem foto de folgas e células já completas: cada dia ainda consulta `FolgaDia` antes de retornar pela âncora. `app/escala/models.py:1286`, `:1289`; `app/ponto/portas/celula.py:598`.
   - **Estimativa D=30:** sem foto → 90.000/150.000 consultas de folga; com foto no mês → pelo menos 180.000/300.000. Se faltam todas as células, acrescenta 90.000/150.000 chamadas individuais a `create`; alterações usam `save` individual. `app/ponto/portas/celula.py:601`, `:623`. Outros ramos podem adicionar consultas: fotografia 12×36 fora das folgas consulta referências anterior/posterior, e feriados têm leitura própria. `app/escala/models.py:1356`, `:1359`, `:1369`, `:1395`; `app/core/feriados.py:82`, `:95`.
   - **Horizonte real:** não é fixamente 30 dias. O padrão une `min(início)`/`max(fim)` das competências atuais de todas as empresas ativas; cortes diferentes alargam a janela. `app/ponto/management/commands/gerar_celulas.py:291`, `:292`, `:309`. Cortes cadastrados atuais [não verificado].
   - **Agenda:** diário 05:50: `app/config/crons.py:426`. A transação envolve toda a geração: `app/ponto/management/commands/gerar_celulas.py:310`.

3. **Apuração de furos refaz `update_or_create` por colaborador/dia, inclusive sem mudança.** Percorre colaboradores × janelas e chama a porta de gravação: `app/escala/services/furos_diarios.py:96`, `:98`, `:99`, `:108`. A porta faz `update_or_create` para cada linha, renovando `avaliada_em`, dentro de transação por pessoa: `app/ponto/portas/celula.py:870`, `:871`, `:872`, `:875`.
   - **Certeza alta:** quantidade de operações ORM cresce com dias de trabalho retornados, sem escrita em lote nesse trecho. `app/ponto/portas/celula.py:871`.
   - **Contraexemplo sintético:** colaborador não isento, escala válida, 30 atas de trabalho com marcos previstos positivos, `FuroDiario` já existente para todas e nenhuma mudança nas batidas/atas: as 30 linhas seguem para atualização. `app/escala/services/furos_diarios.py:33`, `:35`, `:43`, `:49`, `:55`, `:57`; `app/ponto/portas/celula.py:872`.
   - **Estimativa:** 30 linhas por pessoa → 90.000/150.000 chamadas `update_or_create`; 60 → 180.000/300.000. São chamadas ORM, não contagem SQL medida; número exato de statements/savepoints [não verificado].
   - **Multiplicador adicional:** inclui competência anterior quando não trancada: `app/escala/services/furos_diarios.py:73`, `:77`, `:78`.
   - **Mitigação existente:** leitura usa ata materializada, um carregamento de células por colaborador/janela, evitando reconstrução por batida: `app/escala/services/furos_diarios.py:43`; `app/escala/services/leitor_celula.py:345`, `:350`.
   - **Agenda:** encadeado após cartório, `teto_s=900`: `app/config/crons.py:1471`.

4. **Lavrador de HE recalcula espelho completo para cada ativo, potencialmente duas competências.** O laço chama `espelho_do_colab` para cada ativo: `app/ponto/services/he_pendente_lavrado.py:92`, `:94`; este monta autoridade/motor e dias: `app/ponto/services/espelho.py:1225`, `:1227`, `:1262`.
   - **Certeza alta para escala de chamadas; tempo baixa/não medido.** São 3.000/5.000 espelhos por competência; se anterior ainda não entregue, 6.000/10.000. `app/ponto/services/he_pendente_lavrado.py:220`, `:223`, `:224`.
   - **Contraexemplo sintético:** todos ativos com batidas regulares, sem qualquer HE pendente, competência anterior não entregue: o cálculo integral ocorre antes de descobrir ausência de pontas. `app/ponto/services/he_pendente_lavrado.py:94`, `:132`, `:133`.
   - **Fator:** mantendo dias e distribuição, 5.000/3.000=1,67× chamadas; contra N₀ da execução atual, 3.000/N₀ e 5.000/N₀. N₀ atual [não verificado]. Cada autoridade busca suas próprias batidas e vínculos: `app/ponto/services/espelho.py:1022`, `:1096`.
   - **Amplificação histórica:** início visual pode anteceder a competência (`min(piso_visual,apur_ini)`), e o filtro da competência acontece depois do espelho montado: `app/ponto/services/espelho.py:1225`; `app/ponto/services/he_pendente_lavrado.py:114`, `:118`.
   - **Mitigações existentes:** decisões de HE carregadas num mapa por empresa/competência: `app/ponto/management/commands/lavrar_he_pendente.py:48`; tela lê retrato segundo o propósito explícito do comando: `app/ponto/management/commands/lavrar_he_pendente.py:4`. O comentário relata sete minutos/532 colaboradores, mas não é medição atual verificável. `app/ponto/management/commands/lavrar_he_pendente.py:5`.
   - **Agenda:** encadeado após `lavrar_deslogados`, dependente da apuração de furos, teto 1.800s: `app/config/crons.py:1516`.

**Complemento opcional:** o lavrador da folha chama duas competências por padrão (`app/folha/management/commands/lavrar_retrato_folha.py:60`, `:64`, `:65`). `classificar_export` tem lote para férias/âncoras, mas ramo `horas_trabalhadas=0` + duas batidas faz previsto por dia passando apenas `escalas` (`app/folha/export.py:876`, `:888`, `:898`, `:912`, `:915`, `:916`); sem `celulas`, cada dia busca uma célula (`app/escala/utils.py:2279`; `app/escala/utils.py:2159`). Contraexemplo sintético: fechamento zero, código válido, vínculo existente na competência mas nenhum dia com jornada prevista; percorre todos os dias. Isso acrescenta até `N_zero×D` buscas de célula; certeza alta sobre o caminho, proporção real desse estado [não verificado].

## 05/11 16:09 · sessao d2f20499b30b · CODEX-SUFOCA-12-cct-clt · leitura · origem=Code

A cópia sustenta parâmetros CCT sem consumidor e divergências de intervalo, HE e DSR; porém a seleção atual da convenção usa regime da empresa e aplicações por FK, sem comparação com “Juliani” ou “Londrina” nesses ramos (`app/core/regua_cct.py:72`, `app/core/regua_cct.py:253`, `app/core/regua_cct.py:352`). Ocorrência, frequência e impacto em produção: **[nao verificado]**.

Os contraexemplos abaixo são construídos a partir do código, não registros produtivos. Sua execução integrada: **[nao verificado]**.

1. **Sete parâmetros CCT são cadastráveis, mas não chegam ao cálculo — certeza alta.** A declaração inclui adicional noturno, duas tolerâncias, intervalo mínimo, autorização/janela de compensação e teto de prorrogação; a fábrica injeta outros quatro parâmetros (`app/core/regua_cct.py:72`, `app/core/regua_cct.py:458`).  
   **Contraexemplo:** empresa CCT com aplicação vigente, escala com intervalo de 60 minutos e batidas 08:00/12:00/12:30/17:00: alterar somente `Sindicato.intrajornada_minima_min` de 60 para 30 mantém o mínimo obtido da escala/perfil e os 30 minutos suprimidos (`app/ponto/motor_calculo_v2.py:1824`, `app/ponto/motor_calculo_v2.py:2771`). A tela já apresenta “sem efeito no cálculo”; portanto não há evidência de omissão atual desse aviso (`app/templates/core/config/sindicato_form.html:74`).

2. **`Sindicato.obs` documenta; não configura o motor — certeza alta.** É um campo textual, enquanto a régua extrai somente os campos enumerados a partir de `REGUA_LEGAL`, sem `obs` (`app/core/models.py:40`, `app/core/regua_cct.py:39`, `app/core/regua_cct.py:54`, `app/core/regua_cct.py:374`).  
   **Contraexemplo:** escrever apenas “intervalo autorizado de 30 minutos” em `obs`, preservando os campos estruturados e as batidas do item anterior, não altera os argumentos fornecidos ao motor (`app/core/regua_cct.py:458`). Isso não significa que toda cláusula seja inerte: o afastamento da hora reduzida no 12x36 tem parâmetro consumido (`app/core/regua_cct.py:470`, `app/ponto/motor_calculo_v2.py:992`).

3. **Excedente CCT recebe política global fixa, e o calculador paralelo não reproduz essa política — certeza alta dos ramos.** Toda CCT vigente recebe `regua_excedente='legais'`; esse atributo não é campo persistido de `Sindicato` (`app/core/regua_cct.py:378`, `app/core/models.py:34`).  
   **Contraexemplo:** turno partido 08–12/15–20, quatro batidas exatas, previsto líquido de 540 minutos, dia normal, sem ausência e switch ligado. O motor partido limita a referência a 480 e produz 60 minutos de HE50; `regras.do_dia`, com trabalhado=previsto=540 e suprimido=0, produz HE zero (`app/ponto/motor_calculo_v2.py:3119`, `app/ponto/motor_calculo_v2.py:3131`, `app/ponto/motor_calculo_v2.py:3141`, `app/ponto/calculador/regras.py:771`). O vão de três horas evita o desvio de certos turnos partidos para o motor comercial (`app/ponto/motor_calculo_v2.py:3635`).  
   A versão `oraculo` é lavrada separadamente; essa divergência não demonstra dois pagamentos ao trabalhador (`app/ponto/services/fechamento.py:1249`, `app/ponto/services/fechamento.py:1278`, `app/folha/porta_export.py:284`).

4. **Intervalo: um sítio testa duração bruta; outros testam líquida — certeza alta.** O motor 12x36 verifica `>360` antes de descontar a pausa; o comercial verifica depois, e o calculador também usa trabalho líquido (`app/ponto/motor_calculo_v2.py:2767`, `app/ponto/motor_calculo_v2.py:2774`, `app/ponto/motor_calculo_v2.py:3230`, `app/ponto/motor_calculo_v2.py:3232`, `app/ponto/calculador/regras.py:764`).  
   **Contraexemplo:** mínimo cadastrado=60, entrada efetiva 08:00, saída 14:30, pausa 12:00–12:30: bruto=390, pausa=30, líquido=360. O primeiro ramo indeniza 30 minutos; comercial e calculador indenizam zero (`app/ponto/motor_calculo_v2.py:2773`, `app/ponto/motor_calculo_v2.py:2784`, `app/ponto/motor_calculo_v2.py:3232`, `app/ponto/calculador/regras.py:764`).

5. **A faixa de quatro a seis horas tem piso reconhecido, mas indenização bloqueada — certeza alta; omissão compartilhada.** O juiz do piso retorna 15 minutos nessa faixa, enquanto os portões de indenização exigem mais de 360 minutos (`app/escala/models.py:17`, `app/ponto/calculador/regras.py:764`, `app/ponto/calculador/periodo.py:334`).  
   **Contraexemplo:** E21:00/S03:00, 360 minutos sem pausa: indenização zero. Há um teste marcado `expectedFailure` para essa diferença entre piso e resultado (`app/ponto/tests/test_o280_bateria_da_lei.py:1307`, `app/ponto/tests/test_o280_bateria_da_lei.py:1337`).

6. **Noturno: o switch muda o tratamento da mesma lacuna cadastral — certeza alta.** Empresa `cct`, ciclo 12x36 e nenhuma aplicação vigente: com switch ligado, o piso preserva prorrogação pós-05h; desligado, a fábrica deixa o default do motor resolver pelo ciclo, retirando-a (`app/core/regua_cct.py:210`, `app/core/regua_cct.py:371`, `app/core/regua_cct.py:449`, `app/ponto/motor_calculo_v2.py:758`, `app/core/regua_cct.py:182`).  
   **Contraexemplo:** E19:00/S07:00, sem pausa, hora reduzida mantida: ligado conta 540 minutos noturnos de relógio; desligado, 420 — respectivamente 10,2857 e 8 horas reduzidas (`app/ponto/motor_calculo_v2.py:110`, `app/ponto/motor_calculo_v2.py:983`). A falta de aplicação é CADASTRO; a diferença introduzida pelo switch é ESTRUTURA (`app/core/regua_cct.py:368`, `CLAUDE.md:176`).  
   A antiga divergência no desconto noturno dos intervalos não pode ser apresentada como atual: motor e calculador compartilham função e desconto (`app/ponto/motor_calculo_v2.py:983`, `app/ponto/calculador/regras.py:63`, `app/ponto/calculador/regras.py:85`).

7. **DSR comercial abandona o dia atribuído à jornada — certeza alta do mecanismo, média da integração.** O base usa o mapa da jornada; o comercial zera os contadores e recompõe o trabalho pela data civil da entrada (`app/ponto/motor_calculo_v2.py:2578`, `app/ponto/motor_calculo_v2.py:3437`, `app/ponto/motor_calculo_v2.py:3441`).  
   **Contraexemplo de estado:** janela 20–21/09/2026, único dia obrigatório=20, nenhuma justificativa; período normal E21/09 00:00–07:50 atribuído pelo juiz ao plantão de 20/09, cujo marco começa 23:50. O base reconhece trabalho no dia 20; o comercial o considera faltoso, alterando o direito daquela semana (`app/ponto/motor_calculo_v2.py:2600`, `app/ponto/motor_calculo_v2.py:2605`, `app/ponto/motor_calculo_v2.py:3454`). A atribuição à véspera tem exemplo no teste existente (`app/ponto/tests/test_o93_dia_da_jornada.py:61`). O reflexo semanal também continua agrupando extras pela data civil, inclusive no base (`app/ponto/motor_calculo_v2.py:2639`).

8. **HE100 após duas horas é convenção fixa, não lastro jurídico demonstrado — certeza alta do comportamento.** Ambos os cálculos separam as primeiras 120 minutos como HE50 e o restante como HE100; o próprio módulo registra que esse degrau não tem lastro escrito (`app/ponto/calculador/regras.py:800`, `app/ponto/calculador/regras.py:815`, `app/ponto/calculador/periodo.py:166`).  
   **Contraexemplo:** previsto=480, trabalhado=660, suprimido=0, dia normal: HE50=120 e HE100=60 minutos (`app/ponto/calculador/regras.py:771`, `app/ponto/calculador/regras.py:800`). É regra compartilhada, não divergência entre sítios; fundamento em CCT efetiva: **[nao verificado]**.

9. **Interjornada não é validada na seleção de cobertura — certeza alta nesse fluxo.** A função declara a ausência da validação de 11 horas e exclui por última entrada aberta, férias e substituição concorrente (`app/ponto/utils.py:152`, `app/ponto/utils.py:183`, `app/ponto/utils.py:202`).  
   **Contraexemplo:** pessoa ativa, última batida S22:00, cobertura seguinte às 06:00, sem férias ou concorrência: a saída não a exclui, apesar do descanso de oito horas (`app/ponto/utils.py:191`, `app/ponto/utils.py:211`). Bloqueio em outro fluxo ou cálculo de verba correspondente: **[nao verificado]**.

10. **Há identidade de cliente fixa na automação e na apresentação — certeza alta, com alcance delimitado.** Os crons expandem somente empresas `(2,3,4)` no schema `juliani`; o filtro de empresas visíveis exclui CNPJ iniciado em `11222333` (`app/config/crons.py:17`, `app/config/crons.py:1821`, `app/colaboradores/queries.py:86`).  
    **Contraexemplos:** empresa ID 5 não recebe a expansão `processar_cartorio --empresa 5`; empresa ativa com CNPJ `11222333000181` fica fora daquele filtro (`app/config/crons.py:616`, `app/config/crons.py:1825`, `app/colaboradores/queries.py:85`). Isso não comprova rubrica calculada por identidade: a régua examinada seleciona por regime/FKs; renomear empresa ou praça preservando essas relações não muda os predicados (`app/core/regua_cct.py:253`, `app/core/regua_cct.py:354`).

Não consegui verificar cadastros, batidas, switch, versões lavradas, crontab instalado, resultados de testes, valores pagos ou validade jurídica das cláusulas/pareceres: **[nao verificado]**. Tampouco ficou demonstrado seletor monetário atual por nome Juliani/Londrina, CNPJ ou praça por nome; os ramos examinados usam relações cadastrais (`app/core/regua_cct.py:253`, `app/core/regua_cct.py:328`, `app/core/regua_cct.py:352`).

Os donos abaixo classificam os mecanismos demonstrados; atribuir dono a um caso produtivo exige os insumos do juiz L-099 (`app/ponto/management/commands/e6_oraculo.py:98`). **Especulação:** nenhuma ocorrência produtiva ou intenção do usuário foi inferida; ambas permanecem **[nao verificado]**.

| Achado | arquivo:linha | Contraexemplo | Certeza | Dono L-099 |
|---|---|---|---|---|
| Sete parâmetros CCT inertes | `app/core/regua_cct.py:72` | Mínimo sindical 60→30 mantém cálculo | Alta | ESTRUTURA |
| `obs` não configura cálculo | `app/core/regua_cct.py:374` | Cláusula escrita apenas em `obs` | Alta | ESTRUTURA |
| Excedente CCT fixo; calculador diverge | `app/core/regua_cct.py:378`; `app/ponto/calculador/regras.py:771` | Partido de 540 min: HE60 versus HE0 | Alta dos ramos | ESTRUTURA |
| Intervalo por bruto versus líquido | `app/ponto/motor_calculo_v2.py:2767`; `app/ponto/calculador/regras.py:764` | Bruto390/pausa30/líquido360: intra30 versus zero | Alta | ESTRUTURA |
| Piso de 15 min bloqueado | `app/escala/models.py:17`; `app/ponto/calculador/periodo.py:334` | Seis horas sem pausa: intra zero | Alta | ESTRUTURA |
| Switch altera noturno na lacuna | `app/core/regua_cct.py:449`; `app/ponto/motor_calculo_v2.py:758` | CCT sem aplicação, 19–07: 540 versus420 min | Alta | ESTRUTURA; lacuna CADASTRO |
| DSR usa dois dias distintos | `app/ponto/motor_calculo_v2.py:2578`; `app/ponto/motor_calculo_v2.py:3441` | Entrada segunda atribuída ao domingo | Alta mecanismo; média integração | ESTRUTURA |
| Degrau HE100 fixo | `app/ponto/calculador/regras.py:800`; `app/ponto/calculador/periodo.py:166` | Excesso180: HE50=120/HE100=60 | Alta comportamento | ESTRUTURA |
| Cobertura sem validar interjornada | `app/ponto/utils.py:152`; `app/ponto/utils.py:191` | S22:00; cobertura06:00 | Alta nesse fluxo | ESTRUTURA |
| Universo fixo dos crons | `app/config/crons.py:17`; `app/config/crons.py:1821` | Empresa5 fora da expansão | Alta | ESTRUTURA |
| Exclusão por prefixo CNPJ | `app/colaboradores/queries.py:86` | Empresa ativa `11222333000181` invisível | Alta | ESTRUTURA |

## 05/11 16:08 · sessao 3c551803faab · CODEX-SUFOCA-13-seguranca · leitura · origem=Code

O achado de maior impacto é a **possibilidade de redefinir a senha de um superusuário com apenas `gerir_usuarios`**, recebendo a nova senha na resposta. O serviço altera a senha e preserva os poderes do alvo (`app/core/views_usuarios.py:331`, `app/core/views_usuarios.py:341`, `app/core/credenciais.py:185`).

Auditoria estática, somente leitura. Os contraexemplos abaixo são **cenários sintéticos**, não ocorrências atribuídas a `colNN`. Não executei testes ou requisições, nem consultei banco ou logs. Correspondência Git da cópia com o hash informado, incidência e impacto em produção: **[não verificado]**. [nao verificado]

Há controles que limitam os achados: `/me` e `/ponto/bater` derivam o colaborador do usuário autenticado; upload de foto exige batida própria; resposta de regularização filtra ID, dono e estado `aguardando`. Portanto, esses caminhos não demonstram IDOR por simples troca de `colab_id` (`app/api/views_core.py:275`, `app/api/views_core.py:388`, `app/api/views_core.py:846`, `app/api/views_core.py:1319`). O CSRF também permanece habilitado globalmente; as dispensas identificadas são locais (`app/config/settings/base.py:58`).

Os IDOR entre empresas abaixo dizem respeito ao **mesmo tenant**. O SaaS configura seleção de schema por middleware; atravessar schemas exige as condições adicionais descritas no achado JWT (`app/config/settings/saas.py:67`, `app/config/settings/saas.py:85`).

Não demonstrei SQL injection explorável. O SSE interpola identificadores em SQL, mas o schema vem de consulta parametrizada ao catálogo de tenants; controlar esses identificadores pela requisição não foi demonstrado (`app/sse_server.py:23`, `app/sse_server.py:36`, `app/sse_server.py:77`). Também não verifiquei variáveis efetivas, versões instaladas, contas, tokens legados, restrições do navegador, arquivos persistidos ou permissões reais **[não verificado]**.

A classificação proposta é **ESTRUTURA**: os cenários expõem controles de código, independentemente de cadastro ou batidas incorretos; a separação de donos consta em `CLAUDE.md:176`.

| achado | arquivo:linha | contra-exemplo | certeza | dono L-099 (ESTRUTURA/CADASTRO/BATIDA) |
|---|---|---|---|---|
| **1. Reset de senha permite alcançar superusuário** | `app/core/views_usuarios.py:16`, `app/core/views_usuarios.py:331`, `app/core/views_usuarios.py:341`, `app/core/credenciais.py:185` | U não superusuário possui `gerir_usuarios`; S é staff, superusuário e ativo. U faz POST com CSRF válido em `/configuracoes/usuarios-ad/ficha/<S>/acao/`, `acao=reset_senha`. Recebe `senha_temp` de S. **Inferência:** a credencial permite assumir S; login efetivo **[não verificado]**. | **Alta**, fluxo estático | ESTRUTURA |
| **2. Agenda altera colaborador fora da empresa autorizada** | `app/colaboradores/views.py:2271`, `app/colaboradores/views.py:2274`, `app/colaboradores/views.py:2303`, `app/escala/services/agenda.py:11`; escopo esperado em `app/colaboradores/mixins.py:251` | Supervisor somente da empresa A, com `editar_agenda`, envia POST válido para `/colaboradores/<B>/agenda/2026-11-06/bloco/`, sendo B da empresa B. Com dia sem agenda ou em rascunho e horas `08:00–12:00`, o fluxo cria o bloco sem comparar empresa do ator. | **Alta**, fluxo estático | ESTRUTURA |
| **3. Cobertura usa IDs sem conferir alcance do solicitante** | `app/ponto/views.py:3032`, `app/ponto/services/cobertura.py:70`, `app/ponto/services/cobertura.py:92`, `app/ponto/services/cobertura.py:100` | Supervisor da empresa A com `editar_cobertura` fornece titular, substituto disponível, posto e escala da empresa B, com janela válida. O executor cria a ordem; `user` serve como `designada_por`. `chamado_origem_id` também é buscado sem vínculo obrigatório ao titular ou empresa. | **Alta**, fluxo estático | ESTRUTURA |
| **4. Permissão de visualizar painel aprova ou apaga batida** | `app/ponto/views.py:436`, `app/ponto/views.py:456`, `app/ponto/views.py:471`, `app/ponto/views.py:477`, `app/ponto/registro_batida.py:291` | U possui apenas `ver_painel_situacional`. X pertence a outro colaborador/empresa, com `origem='offline_pendente'` e `aprovada=False`. POST válido em `/ponto/offline-pendentes/`, `batida_id=X&acao=rejeitar`, chega à exclusão; `aprovar` altera aprovação e origem para `app`. | **Alta**, fluxo estático | ESTRUTURA |
| **5. Leitura de benefícios autoriza escrita de VA** | `app/colaboradores/views.py:2153`, `app/colaboradores/views.py:2165`, `app/colaboradores/services/beneficios.py:47` | U tem `ver_beneficios`, sem `editar_beneficios`. POST válido em `/colaboradores/configuracao-va/`, com empresa existente, `mes=11`, `ano=2026`, `valor_mensal=999,00`, alcança `update_or_create`. Efeito financeiro posterior **[não verificado]**. | **Alta**, fluxo estático | ESTRUTURA |
| **6. Detalhe de batida expõe dados entre empresas** | `app/core/views.py:974`, `app/core/views.py:978`, `app/core/views.py:1007`; rota em `app/core/urls.py:31` | Supervisor somente de A, com `ver_dashboard_gestao`, consulta `/api/batida/<X>/detalhe/`, sendo X de B. A busca usa somente o ID e devolve identificação, GPS, justificativa, histórico e URL de foto. Download da foto **[não verificado]**. | **Alta**, fluxo estático | ESTRUTURA |
| **7. Escritas por sessão dispensam CSRF explicitamente** | `app/ponto/views.py:1937`, `app/ponto/views.py:1960`, `app/ponto/views.py:3024`, `app/ponto/views.py:3038` | POST com sessão válida e sem token CSRF alcança o carimbo de recusa de notificações; com `editar_cobertura` e payload válido, alcança a criação de cobertura. **Especulação:** disparo por página externa depende do envio do cookie pelo navegador **[não verificado]**. | **Alta** na dispensa; exploração externa **[não verificado]** | ESTRUTURA |
| **8. Upload de “foto” sem validar imagem, seguido de resolução de disputa** | `app/api/views_core.py:839`, `app/api/views_core.py:851`, `app/ponto/registro_batida.py:296`, `app/api/foto_recebida.py:61`, `app/api/foto_recebida.py:79` | Dono de batida envia `x.jpg` contendo texto, com disputa cuja pendência é `foto_ausente`. O caminho persiste o arquivo sem verificar conteúdo de imagem e chama a resolução automática. Fechamento efetivo **[não verificado]**. **Especulação:** HTML armazenado poderia causar XSS; execução no navegador **[não verificado]**. | **Alta** na ausência de validação do fluxo | ESTRUTURA |
| **9. JWT sem vínculo explícito ao tenant** | `app/api/views_core.py:19`, `app/api/credencial.py:186`, `app/config/settings/base.py:47`, `app/config/settings/saas.py:10` | **Cenário condicional:** B copia usuários de A preservando ID e hash de senha; ambos aceitam a mesma chave de assinatura. O token de A encontra em B o mesmo `user_id` e `cred`, sem comparação de tenant. **Especulação:** atravessar hosts depende dessa configuração e de hosts alcançáveis **[não verificado]**. Apenas coincidir o ID não basta para token carimbado (`app/api/credencial.py:113`). | **Média**, condicionada | ESTRUTURA |
| **10. Revogação de JWT legado aceita em ausência de informação ou erro** | `app/api/credencial.py:119`, `app/api/credencial.py:124`, `app/api/credencial.py:138`, `app/api/credencial.py:150` | Token válido pela biblioteca, sem `cred` nem `iat`, continua aceito mesmo com `desde` preenchido após troca de senha. Outra condição: erro ao consultar/atualizar estado retorna aceitação do legado. Existência desses tokens e falhas reais **[não verificado]**. | **Alta** na regra de aceitação | ESTRUTURA |
| **11. Segredo previsível como fallback de configuração** | `app/config/settings/base.py:6`, `app/config/settings/saas.py:10`, `mensageria/config/settings.py:3` | Iniciar sem `DJANGO_SECRET_KEY` ou sem `MSG_SECRET_KEY` seleciona segredo literal publicado no código. **Especulação:** comprometimento de assinaturas depende do uso efetivo do fallback; ambiente de produção **[não verificado]**. | **Alta** no fallback; uso real **[não verificado]** | ESTRUTURA |
| **12. IA perde a autorização por domínio depois da entrada** | `app/core/ia_acoes.py:24`, `app/core/ia_acoes.py:47`, `app/core/ia_acoes.py:63`, `app/core/views_ia.py:864`, `app/core/views_ia.py:908`, `mensageria/nucleo/ferramentas.py:394` | U possui somente `ver_cadastro`, sem `ver_folha`. Passa pela união de permissões da IA; a chamada transmite cliente, pergunta e histórico, sem permissões do usuário. O contexto inclui prontidão da folha. **Inferência:** há caminho de leitura sem a autorização específica; resposta efetivamente exposta **[não verificado]**. | **Alta** na perda de contexto de autorização | ESTRUTURA |
| **13. Console e chat internos da mensageria sem identidade obrigatória** | `mensageria/config/urls.py:12`, `mensageria/nucleo/views.py:26`, `mensageria/nucleo/views.py:55`, `mensageria/nucleo/views.py:83` | Cliente com acesso ao serviço interno, sem login, consulta console de todos os clientes ou escolhe `cliente=<slug>` no chat. Tratar alerta também não exige identidade, embora permaneça sujeito ao CSRF. Compose declara somente `expose:8010`; exposição externa **[não verificado]** (`docker-compose.mensageria.yml:15`, `Caddyfile:8`). | **Alta** no serviço; alcance externo **[não verificado]** | ESTRUTURA |
| **14. SSE confunde sessão existente com autorização staff** | `app/sse_server.py:31`, `app/sse_server.py:41`, `app/sse_server.py:73`, `app/sse_server.py:93`, `app/ponto/realtime.py:32` | Sessão não expirada de colaborador comum solicita `/sse/`. O predicado não decodifica a sessão nem verifica usuário/staff e transmite o canal agregado, com IDs e horários de batidas alheias. **Especulação:** exploração pública depende de encaminhamento ao processo; o Caddy fornecido não declara essa rota (`Caddyfile:8`). | **Alta** no código; disponibilidade pública **[não verificado]** | ESTRUTURA |
| **15. Telemetria anônima escreve entrada livre e CPF em log** | `app/api/urls.py:102`, `app/api/views.py:2658`, `app/api/views.py:2674`, `app/api/views.py:2677` | Pedido autenticado com falha de câmera inclui CPF no emissor de log. Pedido anônimo pode fornecer `error` e `mode` sem limite/escape explícito nesse ponto. **Especulação:** quebras de linha podem adulterar a apresentação do log; formato final, retenção, acesso e conteúdo real **[não verificado]**. | **Alta** no emissor; exploração **[não verificado]** | ESTRUTURA |

## 05/11 16:08 · sessao d9b4555d2289 · - · - · origem=Ronald (tmux)

Achados estáticos, todos com dono **ESTRUTURA**. Casos abaixo são sintéticos; ocorrência em produção e duração acima de 30 s estão **[não verificado]**.

| Sítio | Evidência | Contraexemplo sintético | Custo defensável em 3.000 / 5.000 |
|---|---|---|---|
| `turnos_abertos` | `app/ponto/views.py:376` chama `turnos_do_colab` por ativo; `:392` consulta chamado por turno aberto. Helper consulta batidas em `app/ponto/turnos.py:1861`, células em `:1884`, escalas em `:1885`; carregadores executam queries em `app/escala/alimentacao.py:75` e `:148`. | N colaboradores ativos, escala vigente, uma entrada recente sem saída para cada um, turnos abertos nas últimas 26 h. | **≥1+4N consultas: 12.001 / 20.001**, excluindo consultas opcionais do pareador. Alta certeza. |
| Grade Plano de Folgas | `app/escala/views_plano_folgas.py:42` exige empresa; `:59` carrega vínculos sem `tipo_escala`; `:66` acessa a relação; `:69` varre dias e `:76` chama juiz sem alimentação. `app/escala/models.py:1092` consulta `CelulaDia` por dia. | N vínculos ativos da empresa, mês com 31 dias, células existentes para todos os dias e nenhuma `FolgaDia`. Cadastro correto e batidas completas também disparam. | **≥2+32N consultas: 96.002 / 160.002**, antes de consultas adicionais de apresentação. Certeza alta. |
| Aplicar calendário ao tipo | Universo completo em `app/escala/views.py:392`, transação única em `:407`, duas fotos em `:411` e `:412`, mais re-lavra em `:416`. Cada foto já re-lavra em `app/escala/folgas.py:192`. Regeneração varre dias em `app/ponto/portas/celula.py:209`; chama cálculo sem `folgas=` em `:243`. Cálculo executa dois `exists` quando há foto mensal: `app/escala/models.py:1286` e `:1310`. | N vínculos vigentes integralmente em novembro/dezembro, mesmo tipo, sem competência exportada, calendário com pelo menos uma folga por mês. | **3N chamadas explícitas de regeneração: 9.000 / 15.000**; para D=61, **≥2ND visitas** e **≥4ND consultas só a `FolgaDia`: 732.000 / 1.220.000**. Certeza alta. |

No terceiro caso, **não multiplicar escritas por três**: criação/alteração depende do estado e do DNA da célula (`app/ponto/portas/celula.py:245`). O horizonte pode ampliar o período (`app/ponto/portas/celula.py:172`). As contagens acima excluem sinais disparados pelo `ec.save` em `app/colaboradores/services/vinculo.py:445`.

Mais um N+1 concreto: filtro por empresa no painel situacional enumera colaboradores sem vínculo vigente e executa um `.exists()` por excluído (`app/colaboradores/views.py:2601`, `:2612`, `:2613`). Para C excluídos: **1+C consultas**, ou 3.001 / 5.001 quando todos estão excluídos. Exemplo: empresa em rollout, colaboradores ativos com vínculos encerrados ou nunca cadastrados. Certeza alta; dado que dispara pode ser CADASTRO, mas o custo repetitivo é ESTRUTURA.

Evitar diagnóstico abrangente de N+1 no painel situacional: o caminho com placar limita a entrada a 30 (`app/colaboradores/views.py:2664`), e o construtor usa `turnos_abertos_de` em lote (`app/colaboradores/services/situacional.py:167`). O caminho filtrado ainda monta todo o recorte antes de cortar a saída (`app/colaboradores/views.py:2679`), mas isso, isoladamente, não prova consulta por colaborador.

Fatores: para os três sítios principais, a parcela dependente de N aumenta **3× / 5× comparada a um caso sintético de 1.000 pessoas**, mantendo dias e estado; de 3.000 para 5.000, **5/3≈1,67×**. São fatores de operações, não duração; segundos e população atual **[não verificado]**.

## 05/11 16:08 · sessao b7cab4b86a40 · - · - · origem=Ronald (tmux)

Achados de leitura estática; cenários abaixo são sintéticos, não ocorrências comprovadas em produção [não verificado].

1. **Classificação da folha tem N+1 compartilhado por várias telas.** `classificar_export` carrega fechamentos com apenas `select_related('colaborador')`, percorre todos e chama retenção por pessoa; retenção busca células e vínculos separadamente. Referências: `app/folha/export.py:768`, `:795`, `:942`, `:1248`, `:1188`, `:1194`; consulta efetiva dos vínculos em `app/escala/alimentacao.py:148`, casca singular em `:185`.
   - Contraexemplo: N colaboradores não isentos, admitidos antes da janela, sem demissão, com código Domínio, horas trabalhadas positivas e sem férias. Cada um alcança a retenção; não depende de cadastro ou batida incorreta. Referências dos gates: `app/folha/export.py:840`, `:846`, `:850`, `:876`, `:942`.
   - Custo demonstrável: **2N consultas** só para células/vínculos, portanto **6.000/10.000**, mais a carga inicial. Há ainda acesso lazy à empresa por pessoa em `:1248`, pois `:768` não inclui `colaborador__empresa`: nesse caminho, **3N = 9.000/15.000** consultas antes dos demais auxiliares. Certeza alta para N+1; contagem total e duração real [não verificado].
   - Consumidores síncronos: prévia `app/folha/views.py:331`; raio-X `:589`; ranking `app/relatorios/views.py:1658`; pendências `app/relatorios/views_pend.py:58`. Esses sítios compartilham o custo; não somar como algoritmos independentes.
   - Dono: **ESTRUTURA** (`CLAUDE.md:176`).

2. **Prévia da proposta pagina depois do N+1; página de 50 não limita a carga.** `previa_proposta` chama `montar_previa` antes do `Paginator(linhas, 50)`: `app/folha/views.py:723`, `:764`. O montador chama a classificação inteira e faz `.exists()` para cada colaborador: `app/folha/previa.py:43`, `:57`, `:58`.
   - Contraexemplo: N fechamentos na competência, mesmo que a resposta exibida seja só a primeira página; a passagem pelos N ids ocorre antes da paginação. Referências: `app/folha/previa.py:47`, `:57`; `app/folha/views.py:764`.
   - Acréscimo certo ao achado 1: **N consultas**, isto é, **3.000/5.000**, mesmo para página de 50. Para R retidos com dias propostos, `_cobre_a_retencao` repete o carregamento de células e vínculos: **+2R consultas**. Referências: `app/folha/previa.py:120`, `:170`; `app/folha/export.py:1188`, `:1194`.
   - Contraexemplo da repetição: pessoa com alguma batida, vínculo vigente, dia passado com lâmpada `acesa=False` e hora preenchida, retida por esse dia. Gates em `app/folha/previa.py:79`, `:81`, `:86`, `:107`, `:119`.
   - Na frota inteira nesse estado: adicional ao classificador de **3N = 9.000/15.000** consultas; processamento em memória também percorre células/dias em `app/folha/previa.py:77`. Certeza alta; duração >30s é especulação [não verificado].
   - Dono: **ESTRUTURA** (`CLAUDE.md:176`).

3. **Auditoria de espelhos recria o calendário individual por colaborador, apesar da primeira carga coletiva.** `dataset_furos` começa com `CargaDaJanela(colabs, ...)`, mas no loop chama `contexto_calendario` individual para cada card: `app/relatorios/furos.py:123`, `:126`, `:160`. O calendário cria outra `CargaDaJanela([colaborador], ...)`: `app/colaboradores/services/calendario.py:221`.
   - Contraexemplo: `todos=1` para empresa com N ativos; cada pessoa gera card independentemente de ter furo (`app/relatorios/views.py:1540`, `:1541`; `app/relatorios/furos.py:141`). Alternativamente, um dia irregular por pessoa gera os N cards no modo padrão (`app/relatorios/furos.py:128`, `:141`).
   - Piso conservador de consultas diretas repetidas no calendário: batidas retratadas, escala ativa, células/vereditos, três consultas de chamados, perguntas com resposta = **7N**, portanto **21.000/35.000**, além da nova `CargaDaJanela` e outros auxiliares. Referências: `app/colaboradores/services/calendario.py:239`, `:258`, `:411`, `:437`, `:442`, `:447`, `:473`.
   - Para cada pessoa com dia irregular há mais uma leitura da grade: `app/relatorios/furos.py:132` → `app/escala/services/leitor_celula.py:350`. O calendário também pode ler grade em `app/colaboradores/services/calendario.py:334`.
   - Janela aceita até **93 datas inclusivas**, porque só rejeita diferença `>92`: `app/relatorios/views.py:1538`; expansão cartesiana em `app/relatorios/furos.py:122`, `:123`. Isso admite **279.000/465.000 pares pessoa-dia**, contra **90.000/150.000** numa janela de 30 dias. Certeza alta; memória e duração real [não verificado].
   - Dono: **ESTRUTURA** (`CLAUDE.md:176`).

4. **Raio-X da folha acrescenta consulta por retido e leitura individual da grade.** Depois de classificar toda a empresa, percorre todos os `furo_espelho`, busca novamente o colaborador e chama `furos_por_dia`: `app/folha/views.py:589`, `:605`, `:606`, `:616`; helper em `app/relatorios/furos.py:194`; consulta da grade em `app/escala/services/leitor_celula.py:350`.
   - Contraexemplo: N colaboradores com cadastro regular e células passadas com marcos apagados/retentivos. Cada um entra em `furo_espelho` pela classificação: `app/folha/export.py:942`, `:944`.
   - Acréscimo ao achado 1: **2F consultas**, sendo F retidos por furo; se F=N, **6.000/10.000**. Também percorre F×D em memória. Referências: `app/folha/views.py:605`, `:616`; `app/escala/services/leitor_celula.py:354`.
   - Agravante condicional: célula de trabalho cuja ata não contém `minutos_previstos` chama o juiz do previsto por dia, sem alimentação: `app/escala/services/leitor_celula.py:376`, `:383`. Coeficiente exato desse ramo [não verificado].
   - Certeza alta para +2F; média para dimensão do agravante sem rastrear todos os auxiliares. Dono **ESTRUTURA** (`CLAUDE.md:176`).

Proteções já presentes, para não reportar falsos positivos:

- Tela de retrato da folha lê `RetratoFolha` e faz carregamento de colaboradores/vínculos em lote: `app/folha/views_retrato.py:190`, `:197`, `:104`, `:107`. Não generalizar o N+1 do classificador para essa tela.
- Lista de férias limita linhas a 300 e inclui relações necessárias: `app/ferias/views.py:53`, `:66`, `:68`, `:90`. KPI ainda percorre períodos em Python (`:27`), mas `status_de_periodo` é cálculo de campos, sem consulta (`app/ferias/models.py:93`); não achei N+1 comprovado nesse KPI.
- Caderno de cartões dispara processo próprio com `nice 10`: `app/relatorios/lote_espelho.py:127`, `:132`. O processamento pode escalar, mas não é a antiga geração inteira dentro do request.
- Envio de holerites usa job quando pendentes excedem 50: `app/holerite/views.py:600`, `:635`, `:636`, `:643`, `:669`; parsing do lote também vai a processo próprio (`:1704`, `:1710`). Comentários de 37s e outros tempos são documentação histórica, não medição atual (`:1689`).

Não verifiquei tempos atuais, plano SQL, latências, número real de colaboradores por empresa, distribuição dos estados, concorrência, índices efetivamente implantados ou incidência de atas incompletas [não verificado]. Não executei banco, logs ou testes; contagens acima são consultas identificáveis no caminho descrito, não captura de uma execução [não verificado]. [nao verificado]

## 05/11 16:08 · sessao 6b8fd4ff218a · - · - · origem=Ronald (tmux)

Achados por leitura estática; nenhum caso foi observado em produção, pois o anexo fornecido não contém batidas nem estado de colaboradores. [nao verificado]

1. **Espelhos de período omitem marcações retratadas. Certeza alta; dono ESTRUTURA.** O próprio modelo determina que marcação retratada continue aparecendo com asterisco (`app/ponto/models.py:64-67`). Contudo, a fonte do espelho usa `batidas_apuraveis`, que exclui `retratada_em != NULL` (`app/ponto/turnos.py:2864`; `app/ponto/services/espelho.py:1022`). O mapa denominado “dia de cada batida para quem apresenta marcação” também exclui retratadas (`app/ponto/turnos.py:2874-2875`, `2903-2905`). Essa fonte alimenta o espelho web (`app/ponto/services/espelho.py:1227-1228`, `1262-1266`), o app (`app/api/views.py:1369-1370`, `1461-1475`) e o PDF (`app/relatorios/pdf_espelho.py:596-602`). **Contra-exemplo sintético:** E às 08:00, S às 17:00; a E recebe `retratada_em`. A E permanece armazenada, mas sai desses conjuntos apresentados. As linhas citadas sustentam o efeito; ocorrência real e enquadramento jurídico exato são [nao verificado].

2. **Na tela web, “Batidas” pode representar o papel da grade, em vez do tipo original registrado. Certeza alta sobre comportamento; dono ESTRUTURA.** O template prefere `dia.realizado_grade` e mostra `c.tipo`/`c.hora`; só mostra `b.tipo`/`b.timestamp` quando não há grade (`app/templates/ponto/espelho.html:231-249`). O adaptador usa o tipo do marco e apenas marca divergência contra `tipo_real` (`app/escala/utils.py:1123-1130`); sua documentação explicita “template parar de pintar batida CRUA” (`app/escala/utils.py:1099-1103`). A grade pode vir da ata de `CelulaDia` (`app/ponto/services/espelho.py:519`; `app/escala/services/leitor_celula.py:350`, `384-391`), cujo horário é `lampada.luz` (`app/escala/services/leitor_celula.py:122-128`). **Contra-exemplo sintético:** marco E às 08:00, lâmpada acesa às 08:00 com `tipo_real='S'`, batida armazenada como S. A tela apresenta seta de E e aviso de divergência (`app/templates/ponto/espelho.html:237`); app e PDF apresentam o tipo S do registro (`app/api/views.py:1471-1474`; `app/relatorios/pdf_espelho.py:881-882`). Trata-se de divergência comprovável de apresentação; classificá-la isoladamente como infração normativa seria especulação [nao verificado].

**Positivos e limites relevantes:**

- Existem leituras do cru **atualmente armazenado**: “Últimas batidas” busca todas, inclusive retratadas, limitado às seis últimas (`app/ponto/views.py:191`); `/api/me/` busca as do dia sem filtro de retratação (`app/api/views.py:297-299`); dashboard busca as 30 últimas do dia (`app/core/views.py:193-196`). Isso não prova preservação do original anterior a alterações. [nao verificado]
- O feed das seis últimas não sinaliza a retratação: imprime somente data, hora e entrada/saída (`app/templates/ponto/espelho.html:74-79`), apesar da anotação prometida pelo modelo (`app/ponto/models.py:64-67`).
- O PDF preserva horário e tipo das batidas que chegam ao seu conjunto, inclusive mostrando órfãs em cinza com asterisco (`app/relatorios/pdf_espelho.py:876-897`). Esse asterisco identifica órfã, não resolve a exclusão de retratadas (`app/relatorios/pdf_espelho.py:876-885`; `app/ponto/turnos.py:2864`).
- Não encontrei evidência fornecida de incidência, quantidade afetada ou colaboradores concretos desses dois problemas. [nao verificado]
- Não executei testes nem acessei banco/log; resultados descritos são deduções diretas dos filtros e templates citados. [nao verificado] [nao verificado]

## 05/11 16:07 · sessao 1088965d25ce · - · - · origem=Ronald (tmux)

Achados estáticos para consolidação:

1. **Intervalo: bruto e líquido produzem decisões diferentes — certeza alta, ESTRUTURA.** `Motor12x36` verifica `duracao > 360` antes de descontar a pausa; `MotorComercial` desconta primeiro e verifica o líquido; `regras.do_dia` também verifica o líquido. Contraprova: mínimo cadastrado 60, bruto 390, pausa 30, trabalho líquido 360: primeiro ramo indeniza 30; demais indenizam zero. Exemplo noturno: marcos 21:00–04:00, intervalo cadastrado 01:00–02:00; batidas E21:00/S01:00/E01:30/S03:30. O cadastro mantém previsto 360 e mínimo 60. Fontes: `app/ponto/motor_calculo_v2.py:2761`, `:2767`, `:2771`, `:2784`, `:3230`, `:3232`, `:3242`; `app/ponto/calculador/regras.py:753`, `:760`, `:764`; `app/ponto/calculador/periodo.py:334`.

2. **DSR: o override comercial abandona o dia da jornada — certeza alta do mecanismo, média do cenário integrado, ESTRUTURA.** Base usa `_dia_jornada_por_periodo`; comercial zera os contadores e recompõe pelas datas civis das entradas. Contraprova: apuração 20–21/09/2026, único dia obrigatório 20/09, marco domingo23:50–segunda07:50, batidas segunda00:10–07:50, período normal atribuído pelo juiz a domingo, nenhuma justificativa. Base: duas semanas OK, nenhuma perdida; comercial: uma OK e uma perdida. A atribuição à véspera é apoiada pelo teste existente com entrada00:00 e marco23:50; não executei o cenário integrado. Fontes: `app/ponto/motor_calculo_v2.py:2576`, `:2595`, `:2605`, `:3437`, `:3441`, `:3452`, `:3455`; `app/ponto/tests/test_o93_dia_da_jornada.py:61`; dispatch comercial em `app/ponto/motor_calculo_v2.py:3572`.

3. **Art.71, faixa de mais de quatro até seis horas: piso calculado e indenização bloqueada — certeza alta, ESTRUTURA.** `piso_intervalo_art71(360)` devolve15, mas os portões `>360` devolvem indenização zero para jornada de360 sem pausa. Contraprova já está num `expectedFailure`: E21:00/S03:00, previsto360, pausa0; implementação0 contra piso15 documentado. Fontes: `app/escala/models.py:17`, `:19`; `app/ponto/calculador/regras.py:764`; `app/ponto/calculador/periodo.py:334`; `app/ponto/tests/test_o280_bateria_da_lei.py:1307`, `:1337`. Isso é omissão compartilhada, não divergência entre os dois sítios.

4. **Art.59: degrau automático de HE100 após120 minutos é convenção explicitamente sem lastro — certeza alta do código, lastro jurídico [nao verificado], ESTRUTURA.** Contraprova: previsto480, trabalhado660, pausa e suprimido0, dia normal sem feriado: HE50=120 e HE100=60. Ambos os sítios compartilham essa divisão; não classificá-la como divergência. Fontes: `app/ponto/motor_calculo_v2.py:3392`, `:3396`, `:3397`; `app/ponto/calculador/regras.py:800`, `:815`; reconhecimento explícito em `app/ponto/calculador/periodo.py:166`, `:173`.

Limites relevantes:

- **Noturno atual foi compartilhado:** motor e calculador usam a mesma função `minutos_noturnos`, o mesmo desconto de intervalos e o fator60/52,5 ou1. A divergência histórica descrita em comentários não prova divergência atual. Fontes: `app/ponto/motor_calculo_v2.py:983`, `:992`, `:997`; `app/ponto/calculador/regras.py:63`, `:85`, `:88`.
- **Interjornada:** a seleção de substitutos declara expressamente que não valida11h e seus filtros não verificam descanso após saída anterior. Contraprova operacional: pessoa ativa, última batida S22:00, cobertura seguinte06:00, sem férias nem substituição concorrente pode passar pela seleção com8h de descanso. Fonte: `app/ponto/utils.py:152`, `:183`, `:191`, `:202`, `:211`. Verba indenizatória, bloqueio em outro fluxo e ocorrência em produção [nao verificado].
- **Calculador paralelo não implica segundo pagamento:** o pacote descreve escrita da versão `oraculo` ao lado de `motor` e informa ausência de leitura em tela. Fonte: `app/ponto/calculador/__init__.py:12`.
- **L-099:** divergência com o mesmo cadastro pertence à estrutura; não transformar cenário ilustrativo em erro de cadastro ou batida sem evidência. Fonte: `CLAUDE.md:176`.
- Não acessei banco/logs e não executei testes ou cenários integrados; ocorrência e impacto em produção [nao verificado]. [nao verificado]

## 05/11 16:07 · sessao 2e051fe08391 · - · - · origem=Ronald (tmux)

Resultado estático, sem execução e sem dado de produção. Todos os contraexemplos abaixo são sintéticos; ocorrência em produção [nao verificado]. O texto oficial da norma não integra o material permitido: artigo aplicável, versão vigente e conclusão jurídica [nao verificado].

1. **AFD declarado pendente; geração completa não demonstrada.** O glossário informa literalmente que o sistema ainda não gera AFD automaticamente e que a implementação está pendente (`app/templates/core/glossario.html:155-156`). Não encontrei gerador AFD no código executável pesquisado. Contraexemplo: empresa com E08:00/S17:00 solicitando AFD do período não dispõe de gerador identificado nesta cópia. **Certeza alta sobre a declaração; média sobre ausência global. Dono ESTRUTURA.** Existência de exportador externo [nao verificado].

2. **AEJ: importação desativada não constitui geração.** A rota `app/ponto/urls.py:9` aponta para a função que responde HTTP 410 (`app/ponto/views.py:57-70`). O modelo existente é `ImportacaoAEJ`, com arquivo recebido, status e contagens (`app/ponto/models.py:200-216`); o importador antigo está em `.disabled` (`app/ponto/legacy/README.md:8-17`). Não encontrei gerador AEJ no código executável pesquisado; o TXT exposto em folha é exportação Domínio (`app/folha/urls.py:13-18`). Contraexemplo: jornada tratada E08:00/S17:00 e ajuste manual, solicitando AEJ dos eventos: nenhum gerador identificado. **Certeza média para ausência do gerador; alta para HTTP 410. Dono ESTRUTURA.** Integridade/completude de AEJ efetivamente emitido [nao verificado].

3. **`sequencia` não é prova de NSR sequencial.** O campo é inteiro com `default=1` (`app/ponto/models.py:25`). Seu escritor consulta `sequencia_na_jornada` (`app/api/views_core.py:574-577,685`), que retorna `len(t.batidas)+1` quando existe turno aberto e `1` quando não existe (`app/ponto/turnos.py:2365-2373`). Contraexemplo: após E08:00/S17:00 fechar uma jornada, a primeira batida da jornada seguinte recebe novamente 1; dois colaboradores sem jornada aberta também recebem 1. **Certeza alta sobre semântica local; ausência de outro NSR externo [nao verificado]. Dono ESTRUTURA.** O modelo `Batida` não define campo NSR nem hash (`app/ponto/models.py:8-112`).

4. **Confirmação de batida sem hash/NSR/comprovante assinado no retorno auditado.** A resposta de sucesso devolve `id`, `tipo`, `timestamp`, `sequencia`, mensagem, disputa e aviso (`app/api/views.py:823-834`; também `app/api/views_core.py:823-834`). A confirmação web mostra tipo, hora e nome (`app/templates/ponto/bater_ponto.html:11-30`), enquanto a v2 cria uma mensagem de sucesso e redireciona após três segundos (`app/templates/ponto/bater_ponto_v2.html:150-157`). Contraexemplo: batida online válida com GPS produz esse retorno sem hash ou download de comprovante nesses caminhos. **Certeza alta no escopo desses caminhos. Dono ESTRUTURA.** Comprovante produzido pelo APK ou outro serviço [nao verificado].

   A assinatura Keystore valida um payload recebido do aparelho (`app/api/views_core.py:487-500`); PWA sem assinatura é aceito (`app/api/views_core.py:65-68`). Isso não evidencia emissão de comprovante pelo registrador. Há SHA-256 de PDF de relatórios (`app/relatorios/pdf_base.py:137-170`), identificado como hash do PDF sem rodapé (`app/relatorios/models.py:30-35`); não demonstra hash de comprovante individual.

5. **Restrição efetiva de marcação sem GPS.** A API recusa antes de registrar quando latitude ou longitude são ausentes/falsas e retorna 403 `gps_obrigatorio` (`app/api/views.py:449-466`; `app/api/views_core.py:440-458`). Contraexemplo: colaborador autenticado, tipo E válido, sem permissão GPS: não chega ao `registrar_batida`, que só aparece adiante (`app/api/views_core.py:679-694`). **Certeza alta. Dono ESTRUTURA.** Enquadramento jurídico específico dessa restrição [nao verificado].

6. **Deduplicação por proximidade suprime tentativa distinta, inclusive após retratação.** A API procura mesmo colaborador/tipo em ±5 minutos, sem filtrar `retratada_em`, e devolve a antiga com `sucesso=True` (`app/api/views.py:557-576`; `app/api/views_core.py:549-568`). Contraexemplo: E08:00 retratada; nova tentativa E08:03 com GPS recebe o PK e horário de E08:00, sem criar uma nova batida. Mesmo sem retratação, duas tentativas distintas E08:00/E08:03 colidem. **Certeza alta. Dono ESTRUTURA.** Especulação rotulada: se a segunda tentativa for efetivamente um novo fato, haverá perda do novo registro; se for mero reenvio, a deduplicação é intencional. A natureza real da tentativa [nao verificado].

7. **Vocabulário documental incompatível com o tema REP-P.** O glossário chama software de “REP-C (Registrador Eletrônico de Ponto por Programa)” e afirma validade do produto nessa categoria (`app/templates/core/glossario.html:144-150`). Contraexemplo: usuário consultando a classificação do próprio aplicativo recebe essa denominação. **Certeza alta sobre o texto; correção normativa da categoria [nao verificado] no material permitido. Dono ESTRUTURA.**

Não verificados: registro/certificação INPI, atestado técnico e termo de responsabilidade, assinatura ICP-Brasil/PAdES/CAdES, política de guarda efetiva, sequência global de registros, leiautes emitidos, integridade de arquivos reais, distribuição de comprovantes, APK fora desta cópia e sincronização externa. O anexo fornecido contém apenas lookup de juízes, sem amostra desses artefatos [nao verificado].

## 05/11 16:07 · sessao f5e04c2cbef8 · - · - · origem=Ronald (tmux)

Achados estáticos; cenários abaixo são sintéticos, não registros do anexo. Ocorrência em produção, contas e exploração: [não verificado].

1. **Escalada por reset de superusuário — certeza alta.** `_requer_admin` exige somente `gerir_usuarios`; a ação busca qualquer usuário `is_staff=True`, inclusive superusuário, redefine a senha e devolve `senha_temp` ao solicitante. O serviço preserva `is_superuser`. Evidência: `app/core/views_usuarios.py:16`, `app/core/views_usuarios.py:324`, `app/core/views_usuarios.py:331`, `app/core/views_usuarios.py:341`, `app/core/credenciais.py:185`. Rota: `app/core/urls.py:71`. **Contraexemplo:** U ativo, não superusuário, com `rbac_gerir_usuarios`; S ativo, staff e superusuário. U envia POST com CSRF válido para `/configuracoes/usuarios-ad/ficha/<S>/acao/`, `acao=reset_senha`; recebe a nova senha de S. Há restrição explícita somente-super para conceder superusuário e criar [nome], mas ela não aparece no reset: `app/core/views_usuarios.py:73`, `app/core/views_usuarios.py:279`. **Inferência:** entrar com a credencial recebida permite assumir os poderes de S; superusuário passa em `tem_acao`: `app/colaboradores/mixins.py:328`. Login real: [não verificado]. Dono proposto: **ESTRUTURA**.

2. **Agenda permite alterar colaborador de outra empresa no mesmo tenant — certeza alta.** A criação exige `editar_agenda`, mas resolve `cid` globalmente; a porta grava sem verificar alcance do usuário. Evidência: `app/colaboradores/views.py:2271`, `app/colaboradores/views.py:2274`, `app/colaboradores/views.py:2296`, `app/colaboradores/views.py:2303`, `app/escala/services/agenda.py:11`. A semeadura concede `editar_agenda` à supervisão, cujo universo declarado é restrito às empresas dos respectivos setores: `app/colaboradores/mixins.py:355`, `app/colaboradores/mixins.py:251`. Rota: `app/colaboradores/urls.py:20`. **Contraexemplo:** supervisor U vinculado apenas à empresa A, colab B na empresa B, sem agenda ou com rascunho em 2026-11-06; POST com CSRF válido em `/colaboradores/<B>/agenda/2026-11-06/bloco/`, JSON `{"hora_inicio":"08:00","hora_fim":"12:00"}`. O fluxo cria o bloco para B. DELETE e envio para aceite também não verificam empresa do ator: `app/colaboradores/views.py:2319`, `app/colaboradores/views.py:2322`, `app/colaboradores/views.py:2349`, `app/colaboradores/views.py:2352`. Dono proposto: **ESTRUTURA**.

3. **Permissão de visualizar painel aprova ou exclui batidas — certeza alta.** A única autorização é `ver_painel_situacional`; depois o POST busca a batida pelo ID, origem e aprovação, sem escopo de empresa/posto e sem ação de edição. Evidência: `app/ponto/views.py:434`, `app/ponto/views.py:436`, `app/ponto/views.py:456`, `app/ponto/views.py:471`, `app/ponto/views.py:477`. Os serviços efetivamente salvam ou apagam sem outra autorização: `app/ponto/registro_batida.py:235`, `app/ponto/registro_batida.py:254`, `app/ponto/registro_batida.py:285`, `app/ponto/registro_batida.py:291`. Rota: `app/ponto/urls.py:77`. **Contraexemplo:** U autenticado com apenas `rbac_ver_painel_situacional`; batida X de outro colaborador/empresa com `origem='offline_pendente'` e `aprovada=False`. POST com CSRF válido em `/ponto/offline-pendentes/`, `batida_id=X&acao=rejeitar`, apaga X; `acao=aprovar` muda aprovação e origem para `app`. Dono proposto: **ESTRUTURA**.

4. **Permissão de leitura dos benefícios escreve valor de VA — certeza alta.** A view exige apenas `ver_beneficios`, aceita `empresa` do POST e chama `update_or_create`; não exige `editar_beneficios`. Evidência: `app/colaboradores/views.py:2153`, `app/colaboradores/views.py:2155`, `app/colaboradores/views.py:2164`, `app/colaboradores/services/beneficios.py:47`. Rota: `app/colaboradores/urls.py:80`. **Contraexemplo:** U com `rbac_ver_beneficios`, sem `rbac_editar_beneficios`; empresa E existente; POST com CSRF válido em `/colaboradores/configuracao-va/`, `empresa=E&mes=11&ano=2026&valor_mensal=999,00`. O serviço grava `valor_mensal=999`, `ativo=True`, `criado_por=U`: `app/colaboradores/services/beneficios.py:50`. Efeito financeiro posterior: [não verificado]. Dono proposto: **ESTRUTURA**.

5. **Drawer JSON de batida vaza dados entre empresas — certeza alta.** Checa `ver_dashboard_gestao`, carrega batida só pelo ID e devolve nome, matrícula, empresa, GPS, URL de selfie e histórico. Evidência: `app/core/views.py:974`, `app/core/views.py:978`, `app/core/views.py:989`, `app/core/views.py:1005`, `app/core/views.py:1007`. Rota: `app/core/urls.py:31`. **Contraexemplo:** supervisor U só da empresa A, com `ver_dashboard_gestao`, consulta `/api/batida/<X>/detalhe/`, onde X é batida de B na empresa B do mesmo schema. Não há comparação com o universo de `get_colaboradores_visiveis`, que excluiria B: `app/colaboradores/mixins.py:251`. Download efetivo da selfie: [não verificado]. Dono proposto: **ESTRUTURA**.

6. **CSRF explicitamente dispensado em criação de cobertura — certeza alta sobre a dispensa.** A rota usa sessão (`login_required`), exige `editar_cobertura`, mas tem `csrf_exempt` e interpreta `request.body` como JSON sem conferir origem ou Content-Type. Evidência: `app/ponto/views.py:3024`, `app/ponto/views.py:3032`, `app/ponto/views.py:3035`. Rota: `app/ponto/urls.py:50`. **Contraexemplo estático:** sessão autorizada envia POST sem token CSRF, com titular/substituto/posto/tipo existentes, substituto disponível e `dt_fim>dt_inicio`; o serviço grava a ordem: `app/ponto/services/cobertura.py:51`, `app/ponto/services/cobertura.py:67`, `app/ponto/services/cobertura.py:70`, `app/ponto/services/cobertura.py:82`, `app/ponto/services/cobertura.py:99`. **Especulação rotulada:** disparo de um navegador de outra origem depende do envio do cookie e de SameSite/proxy; exploração real [não verificado]. Dono proposto: **ESTRUTURA**.

Limites e controles relevantes:

- O middleware base mantém CSRF, mas não contém um gate global de escopo por empresa ou login: `app/config/settings/base.py:53`. No SaaS há isolamento por schema com `TenantMainMiddleware`; portanto os IDOR acima demonstram cruzamento **entre empresas dentro do mesmo tenant**, não entre schemas: `app/config/settings/saas.py:67`, `app/config/settings/saas.py:85`. Configuração efetivamente implantada: [não verificado].
- `batida_offline` tem `csrf_exempt`, porém retorna apenas HTTP 410; não tratá-la como escrita explorável: `app/ponto/views.py:417`, `app/ponto/views.py:425`.
- `salvar_foto_batida` verifica explicitamente o dono da batida: `app/ponto/views.py:2888`; ausência de IDOR nesse caminho específico.
- Views sem `login_required` não são automaticamente anônimas: `tem_acao` recusa usuário não autenticado em `app/colaboradores/mixins.py:326`. Exemplos como `fechamento_mensal` e `raiox_lente` têm guardas internas: `app/ponto/views.py:1097`, `app/chamados/views.py:1467`.
- Nenhum teste foi executado; não houve acesso a banco/log nem modificação de arquivo. Quantidade de usuários afetados e vínculo de qualquer `colNN` aos cenários: [não verificado]. [nao verificado]

## 05/11 16:07 · sessao c3191843e38b · - · - · origem=Ronald (tmux)

Achados de leitura estática; os contra-exemplos abaixo são sintéticos, sem ocorrência em produção demonstrada. [nao verificado]

1. **Regularização externa altera horário/tipo da batida original e também a apaga fisicamente — certeza alta.** A tela aceita `corrigir` e `remover`, encontra a batida por `pk` e colaborador, sem restringir sua origem, e vincula esse alvo ao item: `app/chamados/views_regularizacao.py:102-130`. O alvo é FK para `ponto.Batida`: `app/chamados/models.py:3214-3222`. Após confirmação do colaborador (`app/chamados/views_regularizacao.py:197-217`), o serviço exige lote `confirmada` e ausência de disputa aberta (`app/chamados/services/regularizacao_ext.py:51-65`), mas executa `alvo.delete()` para remover (`:131-140`) ou sobrescreve `alvo.tipo`, `alvo.timestamp` e salva a mesma linha (`:149-161`).
   
   **Contra-exemplo:** colaborador com batida `origem='app'`, entrada às 08:00; supervisão cria item `corrigir`, alvo nessa batida, entrada 08:30, e colaborador confirma. O horário original é substituído por 08:30. Alternativa: item `remover` elimina a linha. O ramo não consulta tranca de competência, inclusive por decisão documentada no próprio serviço (`app/chamados/services/regularizacao_ext.py:114-119`). **Dono estrutural proposto:** ESTRUTURA, pelo comportamento do escritor; classificação de caso real pelo juiz L-099 não executada [nao verificado].

2. **Existe segunda porta de DELETE para batida offline pendente — certeza alta.** Usuário com `ver_painel_situacional` pode postar `rejeitar`; a busca restringe a `origem='offline_pendente'`, `aprovada=False` (`app/ponto/views.py:433-459`), e chama `apagar` (`:477-481`), que executa `batida.delete()` (`app/ponto/registro_batida.py:285-292`).
   
   **Contra-exemplo:** linha offline pendente com horário 08:00, ainda não aprovada; DP rejeita e a linha deixa de existir. O comentário da porta considera isso “fato [...] antes de virar marcação” (`app/ponto/registro_batida.py:286-287`), mas a própria view o chama “DELETE de fato de chão” (`app/ponto/views.py:478`). Se há linhas assim na produção, ou qual porta atual ainda as cria, [nao verificado]; buscas localizaram principalmente leitores e este tratador.

3. **Flip humano e automático sobrescreve o tipo E/S da mesma batida — certeza alta.** O endpoint aceita POST com `regularizar_batida` e busca qualquer `Batida` por pk (`app/ponto/views.py:3250-3256`). `flip_tipo` recusa retratada, ausência de autor e competência fechada; depois chama `anotar` com o novo tipo (`app/ponto/services/flip_batida.py:15-33`). `anotar` admite `tipo` e salva a mesma linha (`app/ponto/registro_batida.py:203-206,252-263`). Existe comando automático que também chama essa função (`app/ponto/management/commands/flip_automatico.py:62-68`), declarado com `--competencia --apply` no cron (`app/config/crons.py:707`); execução real do cron [nao verificado].
   
   **Contra-exemplo:** marcação original de aparelho E às 18:00, não retratada, competência aberta; operador autorizado aciona flip. Continua a mesma pk e horário, agora `tipo='S'`. **Limite normativo:** se tipo E/S é apenas classificação de tratamento ou integra o registro original legal não foi verificado na norma; alteração de timestamp não ocorre neste flip (`app/ponto/services/flip_batida.py:25-33`).

4. **Trilha é best effort e não garante reconstrução da alteração/exclusão — certeza alta no comportamento de exceção.** `anotar` salva antes de registrar a trilha (`app/ponto/registro_batida.py:252-263`); `apagar` apaga antes da trilha (`:291-292`). `registrar_log` captura qualquer exceção e apenas registra erro (`app/core/auditoria.py:17-29`). A regularização externa também captura falha de auditoria e continua (`app/chamados/services/regularizacao_ext.py:107-112`).
   
   **Contra-exemplo:** batida alterada/removida por uma dessas portas e falha de `evento` antes da persistência da auditoria. A operação do domínio já aconteceu e a função de auditoria não propaga a falha. Falhas concretas e persistência efetiva na produção [nao verificado].

Proteções observadas: retratação comum preserva a linha, anotando `aprovada=False` e `retratada_em`, sem alterar seu horário (`app/ponto/registro_batida.py:267-282`); `timestamp` não pertence a `CAMPOS_ANOTAVEIS` (`:203-206`). O Django admin de Batida usa `SoLeitura` (`app/ponto/admin.py:11-15`), cujo mixin nega adicionar/alterar/excluir (`app/core/admin.py:59-66`). Isso não cobre os ramos diretos de regularização acima.

Não encontrei campo NSR no modelo Batida; `sequencia` é `IntegerField(default=1)`, sem unicidade própria, e a restrição única é colaborador+timestamp (`app/ponto/models.py:22-29,102-106`). Não prova ausência em toda a aplicação ou garantia do banco externo; [nao verificado].

## 05/11 16:07 · sessao 8238e4b9d56a · - · - · origem=Ronald (tmux)

1. **Upload de selfie sem validação de imagem — certeza alta no fluxo estático; dono ESTRUTURA.** `/api/ponto/foto/` aponta para `api_upload_foto`, que autentica e limita a batida ao próprio colaborador, mas recebe o arquivo diretamente e o entrega a `anexar_foto`; o executor persiste sem validar conteúdo, extensão ou tamanho: `app/api/urls_core.py:8`, `app/api/views_core.py:837`, `app/api/views_core.py:846`, `app/api/views_core.py:851`, `app/ponto/registro_batida.py:296`, `app/ponto/registro_batida.py:307`. **Contraexemplo sintético:** usuário com batida própria envia `prova.html` contendo texto em `foto`; esse fluxo não o distingue de uma selfie e depois chama a resolução de `foto_ausente`: `app/api/views_core.py:860`, `app/api/foto_recebida.py:61`, `app/api/foto_recebida.py:79`. Fechamento efetivo de disputa e resposta HTTP são [não verificado]. **Especulação rotulada:** HTML armazenado poderia executar script quando aberto; a mídia autorizada é entregue ao Caddy sem `Content-Type` definido pelo Django, mas execução no navegador é [não verificado]: `app/core/media_protegida.py:45`, `Caddyfile:11`.

2. **CSRF explicitamente desativado em operações de sessão — certeza alta; dono ESTRUTURA.** `notificacao_negada` e `criar_cobertura` exigem login, porém têm `csrf_exempt`; a primeira grava a recusa de notificações e a segunda despacha criação de cobertura: `app/ponto/views.py:1937`, `app/ponto/views.py:1962`, `app/ponto/views.py:3024`, `app/ponto/views.py:3038`. **Contraexemplo sintético:** POST com sessão válida, sem token CSRF, pode carimbar `notificacao_negada_em`; POST JSON com sessão de usuário que possui `editar_cobertura`, IDs existentes e substituto disponível segue para criação: `app/ponto/views.py:1954`, `app/ponto/views.py:1960`, `app/ponto/views.py:3032`, `app/ponto/services/cobertura.py:81`, `app/ponto/services/cobertura.py:100`. **Especulação rotulada:** exploração por página externa depende do envio do cookie pelo navegador; isso é [não verificado]. A existência do bypass no servidor independe dessa hipótese: `app/ponto/views.py:1938`, `app/ponto/views.py:3024`.

3. **Chave Django previsível quando variável não existe — certeza alta na configuração, média no risco efetivo; dono ESTRUTURA.** `SECRET_KEY` possui fallback literal, herdado pelo perfil SaaS sem redefinição; mensageria também possui fallback literal: `app/config/settings/base.py:6`, `app/config/settings/saas.py:10`, `mensageria/config/settings.py:3`. **Contraexemplo sintético:** iniciar com `DJANGO_SECRET_KEY` ausente usa a chave publicada no código; iniciar mensageria sem `MSG_SECRET_KEY` faz o mesmo: mesmas linhas. Uso dessas chaves em produção e comprometimento de credenciais são [não verificado]. Não reproduzir os valores na resposta.

4. **Mensageria tem console e chat sem controle de identidade — certeza alta no código; dono ESTRUTURA.** Rotas `/console/`, `/chat/` e `/console/tratar/<pk>/` chamam views sem login/token; o middleware configura autenticação de sessão, mas não impõe login: `mensageria/config/urls.py:12`, `mensageria/config/urls.py:16`, `mensageria/nucleo/views.py:26`, `mensageria/nucleo/views.py:55`, `mensageria/nucleo/views.py:83`, `mensageria/config/settings.py:11`. **Contraexemplo sintético:** cliente interno sem sessão lê o console com todos os clientes; POST ao chat escolhe o cliente por `slug`, sem associação ao solicitante: `mensageria/nucleo/views.py:28`, `mensageria/nucleo/views.py:89`, `mensageria/nucleo/views.py:95`. Para tratar alerta, ainda há middleware CSRF; não afirmar POST sem token. Mesmo com CSRF válido, não há identidade/autorização e `por` vem do próprio POST: `mensageria/nucleo/views.py:57`, `mensageria/nucleo/acoes.py:18`. **Alcance:** compose só declara `expose: 8010`; Caddy da cópia não publica esse serviço: `docker-compose.mensageria.yml:15`, `Caddyfile:8`, `Caddyfile:10`. Exposição externa é [não verificado].

5. **Servidor SSE não verifica autenticação nem staff dentro da sessão — certeza alta no código; dono ESTRUTURA.** `_sessao_valida` apenas procura sessão e compara expiração; não decodifica `session_data`, não consulta usuário/permissão e transmite o canal agregado: `app/sse_server.py:31`, `app/sse_server.py:38`, `app/sse_server.py:45`, `app/sse_server.py:73`, `app/sse_server.py:93`. **Contraexemplo sintético:** sessão não expirada de colaborador comum do tenant solicita `/sse/`; o predicado aceita, e uma batida de outra pessoa publicada no canal agregado chega com `colab_id`, `batida_id` e timestamp: `app/sse_server.py:60`, `app/sse_server.py:70`, `app/ponto/realtime.py:32`, `app/ponto/realtime.py:39`, `app/ponto/realtime.py:42`. **Alcance:** o Caddy fornecido não encaminha `/sse/` ao processo; exploração no endereço público é [não verificado]: `Caddyfile:8`, `Caddyfile:10`.

**SQL:** há SQL interpolado em `SET search_path` e `LISTEN`, mas os identificadores derivam do schema retornado por consulta parametrizada ao catálogo de tenants; não demonstrei entrada HTTP controlando diretamente esses identificadores: `app/sse_server.py:18`, `app/sse_server.py:23`, `app/sse_server.py:36`, `app/sse_server.py:77`. SQL injection explorável é [não verificado].

**Limites:** nenhum banco, log ou execução HTTP foi usado [não verificado]. Os contraexemplos são sintéticos, sem atribuição a colNN real [não verificado]. Não há evidência fornecida de incidente, impacto em produção, variáveis efetivas ou sessão explorada [não verificado].

## 05/11 16:06 · sessao 049233b51ce2 · - · - · origem=Ronald (tmux)

[resposta retida: senha_atribuida -- nao se publica credencial nem caminho de segredo]

## 05/11 16:06 · sessao 23147f058813 · - · - · origem=Ronald (tmux)

Leitura concluída; nenhum arquivo foi criado, editado ou apagado. Execução e incidência em produção: [nao verificado].

1. **Política global de excedente embutida na régua:** qualquer sindicato encontrado por aplicação vigente recebe `r['regua_excedente']='legais'`, independentemente da identidade e dos parâmetros do sindicato; o piso usa `'relogio'`. `Sindicato` não possui campo persistido `regua_excedente`, portanto não cabe dizer que essa atribuição sobrescreve um campo cadastrável desse modelo. Fontes: `app/core/regua_cct.py:39-40`, `app/core/regua_cct.py:374-378`, `app/core/models.py:34-98`.
   - Contraexemplo construído por leitura: empresa `cct`, aplicação única vigente, switch global `regua_cct_ativa=1`, escala realmente partida 08–12 / 15–20, previsto líquido 540 minutos, quatro batidas exatas, dia normal e sem ausência. A régua injeta `'legais'`; o motor limita a jornada diária a 480 e apura 60 minutos de HE50. Com piso `'relogio'`, o limite permanece 540 e não há essa HE diária. Fontes: `app/core/regua_cct.py:444-459`, `app/ponto/motor_calculo_v2.py:3112-3142`. O vão de três horas evita o desvio das escalas rotuladas partidas com vão de até duas horas para `MotorComercial`: `app/ponto/motor_calculo_v2.py:3635-3666`.
   - Certeza **alta** da política fixa e dos ramos; cálculo numérico inferido do código, não executado [nao verificado]. Validade jurídica dessa diferenciação [nao verificado]. Dono **ESTRUTURA** para esse caso de batidas pares e cadastro coerente: `app/ponto/management/commands/e6_oraculo.py:98-110`.

2. **Identidade de cliente hardcoded existe na automação, sem prova de seleção de rubrica por identidade:** `EMPRESAS=(2,3,4)` e `SCHEMA='juliani'`; entradas `por_empresa=True` expandem somente esses IDs e sempre usam esse schema. Fontes: `app/config/crons.py:17-18`, `app/config/crons.py:1821-1826`.
   - Contraexemplo: nova empresa ID 5 com cadastro operacional e batidas completas não recebe a linha `processar_cartorio --empresa 5` gerada por essa expansão; a entrada está declarada `por_empresa=True`. Fontes: `app/config/crons.py:616`, `app/config/crons.py:1821-1826`.
   - Certeza **alta** da lista fechada; uso efetivo desse crontab e consequência financeira em produção [nao verificado]. Dono **ESTRUTURA** pela cobertura do mecanismo, sem atribuir caso produtivo [nao verificado].

3. **CNPJ hardcoded existe em filtro de apresentação:** empresas ativas cujo CNPJ começa com `11222333` são excluídas de `empresas_visiveis()`. O próprio contrato limita esse helper a dropdowns/filtros e exclui exportação e lógica interna. Fonte: `app/colaboradores/queries.py:78-87`.
   - Contraexemplo: empresa ativa legítima com CNPJ `11222333000181` fica fora desse queryset, mesmo com todos os demais dados preenchidos. Fonte: `app/colaboradores/queries.py:85-86`.
   - Certeza **alta** da exclusão; existência de empresa legítima com esse prefixo em produção [nao verificado]. Dono **ESTRUTURA** da regra literal; não demonstra cálculo CCT por CNPJ.

4. **Não encontrei seletor vivo CCT por nome Juliani/Londrina, CNPJ ou empresa 2/3/4 nos caminhos de cálculo examinados.** O caminho encontrado usa `Empresa.regime_trabalhista`, `AplicacaoConvencao` por FK e praça obtida do vínculo do dia; aplicação de praça vence a da empresa, empate retorna piso. Fontes: `app/core/regua_cct.py:253-275`, `app/core/regua_cct.py:328-330`, `app/core/regua_cct.py:352-374`.
   - Contraexemplo à tese de nome: renomear empresa/praça preservando FKs, regime, vigência e aplicações não altera essas decisões; nomes entram na descrição da fonte, não na comparação desses ramos. Fontes: `app/core/regua_cct.py:253-275`, `app/core/regua_cct.py:354-379`.
   - Certeza **alta** para esses caminhos; ausência absoluta em todo código e execução produtiva [nao verificado].

O comando de semeio também recebe IDs por argumentos; os exemplos com empresa/sindicato específicos estão na documentação, não como critério executável. Fontes: `app/core/management/commands/semear_aplicacao_convencao.py:37-41`, `app/core/management/commands/semear_aplicacao_convencao.py:80-83`.

L-099 vivo: ausência de vínculo/folga indefinida → CADASTRO; batida fora de turno ou quantidade ímpar → BATIDA; fase deslocada/cadastro versus realidade → CADASTRO; resíduo → ESTRUTURA. Fonte: `app/ponto/management/commands/e6_oraculo.py:98-110`.

## 05/11 16:06 · sessao b0033745a695 · - · - · origem=Ronald (tmux)

Auditoria CCT, somente leitura; nenhum teste executado, nenhuma consulta a banco/log. Ocorrência em produção e valores efetivos de cadastro: **[nao verificado]**. [nao verificado]

1. **Sete parâmetros estruturados de CCT ficam sem consumidor de cálculo — certeza alta, dono ESTRUTURA.** São `adicional_noturno_pct`, tolerâncias por marcação/dia, `intrajornada_minima_min`, autorização/janela de compensação e teto de prorrogação: declaração em `app/core/regua_cct.py:72`; fábrica efetivamente injeta apenas quatro parâmetros em `app/core/regua_cct.py:458`. O modelo aceita e valida, por exemplo, intervalo mínimo de 30 minutos (`app/core/models.py:63`, `app/core/models.py:85`), mas `_intra_minima` usa cadastro da escala/perfil e piso pelo previsto, sem consultar esse campo sindical (`app/ponto/motor_calculo_v2.py:1824`). **Contraexemplo construído:** empresa CCT com aplicação vigente, jornada prevista superior a seis horas, escala com intervalo de 60 minutos, batidas 08:00–12:00 / 12:30–17:00; mudar somente o sindicato de intervalo mínimo 60→30 não altera a conta, que continua calculando 30 minutos suprimidos pela fórmula em `app/ponto/motor_calculo_v2.py:2771`. A tela atual **já avisa** “sem efeito no cálculo”; não denunciar promessa silenciosa atual (`app/templates/core/config/sindicato_form.html:74`). Espécie jurídica válida para esse exemplo: **[nao verificado]**.

2. **Cláusula digitada em `Sindicato.obs` não vira regra — certeza alta, dono ESTRUTURA para o limite do sistema.** Campo é texto livre (`app/core/models.py:40`, `app/core/configuracao_efeito.py:155`), salvo pelo formulário (`app/core/views_config.py:267`, `app/core/views_config.py:306`); régua monta seu dicionário exclusivamente a partir de `_CAMPOS` (`app/core/regua_cct.py:54`, `app/core/regua_cct.py:374`), cuja base não contém `obs` (`app/core/regua_cct.py:39`). **Contraexemplo construído:** mesma empresa/aplicação/batidas, alterar apenas observação para “intervalo autorizado de 30 minutos” mantém todos os parâmetros fornecidos ao motor, portanto não muda o cálculo (`app/core/regua_cct.py:458`). Existência desse texto ou expectativa real do usuário em produção: **[nao verificado]**. **Não classificar automaticamente como cadastro errado:** sem dado de instância, isso não foi medido; L‑099 define separar estrutura/dado (`CLAUDE.md:176`).

3. **O switch CCT produz duas respostas noturnas para empresa CCT sem aplicação — certeza alta por encadeamento estático, dono ESTRUTURA.** Com `regua_cct_ativa=1`, empresa `cct` sem aplicação vigente retorna `_regua_legal` sem `regime_clt_declarado`, mantendo `prorrogacao_noturna_pos5h=True` (`app/core/regua_cct.py:39`, `app/core/regua_cct.py:210`, `app/core/regua_cct.py:366`). Com switch desligado, a fábrica só injeta prorrogação se empresa declarada `clt`; para empresa CCT passa direto ao motor (`app/core/regua_cct.py:449`). O valor ausente é resolvido por `prorrogacao_pos5h_legal('12x36')`, que retorna `False` (`app/ponto/motor_calculo_v2.py:758`, `app/core/regua_cct.py:182`), preservado pelo repasse da fábrica (`app/ponto/motor_calculo_v2.py:3678`). **Contraexemplo construído:** empresa CCT, nenhuma aplicação, escala 12x36, batidas 19:00–07:00; ON permite extensão noturna pós‑05h; OFF restringe a janela. A divergência contradiz o comentário de “mantém valor histórico” no ramo OFF (`app/core/regua_cct.py:450`). Switch/casos reais: **[nao verificado]**.

4. **Política de excedente CCT está cravada e só uma família lê o parâmetro — certeza alta, dono ESTRUTURA.** `regua_para` força `regua_excedente='legais'` para toda CCT encontrada (`app/core/regua_cct.py:378`); isso não é campo no modelo sindical (`app/core/models.py:34`) nem input do dicionário de edição (`app/core/views_config.py:275`). Leitores operacionais estão em `MotorTurnoPartido`, que diferencia teto diário, ganho noturno e fechamento semanal (`app/ponto/motor_calculo_v2.py:3107`, `app/ponto/motor_calculo_v2.py:3119`, `app/ponto/motor_calculo_v2.py:3148`); a declaração reconhece efeito somente nessa família (`app/core/regua_cct.py:106`). **Contraexemplo de estado:** desejar declarar uma CCT com excedente semanal não tem caminho por cadastro sindical; mesmo `getattr` devolvendo outro valor, ele seria sobrescrito em `app/core/regua_cct.py:378`. **Contraexemplo de família:** trocar `regua_excedente` em um motor 12x36 não encontra leitor desse atributo nessa classe; a análise AST do contrato descreve e fiscaliza esse limite (`app/core/tests/test_contract_parametro_nao_mente.py:139`). CCT real que demande opção diferente: **[nao verificado]**.

Limites: anexo fornecido contém tabela de autoridades, sem cadastro ou batidas. Não extraí ocorrência, frequência, impacto monetário nem validade jurídica das cláusulas. As referências jurídicas acima descrevem o que o código diz/faz; não houve consulta externa porque o escopo é exclusivamente dado fornecido + cópia.

## 05/11 16:06 · sessao bf259c8846ee · - · - · origem=Ronald (tmux)

A captura recomendada é envolver `dias_que_seguram` para guardar seu retorno e `_fio_do_dia` para registrar `(celula_id, fio)`, ambas chamando a função original. Depois de `fora_da_folha`, percorra `_u['furo']` e `_u['dias'][cid]` na ordem original e associe por posição, verificando `celula_id`; isso preserva também dias com `celula_id=None` (`app/folha/fora_da_folha.py:160`, `app/folha/fora_da_folha.py:171`, `app/folha/fora_da_folha.py:178`).

Verifique que a quantidade capturada de `nada` coincide com `dias_por_fio['nada']` e que não existem chaves repetidas antes de converter em conjunto; a função soma ocorrências no laço (`app/folha/fora_da_folha.py:180`).

`limite=float('inf')` entrega todas as rotas; `sem_porta` já entra independentemente do limite. Verifique `alcance_dias_omitidos == 0` e `len(alcance_dias) == alcance_universo` (`app/chamados/services/vigia_fio.py:624`). O vigia deduplica primeiro por `(colab_id, data)` e conserva a primeira competência com `setdefault`; convém um índice auxiliar nessa dupla para reconhecer competência substituída na comparação tripla (`app/chamados/services/vigia_fio.py:580`, `app/chamados/services/vigia_fio.py:586`).

As classes a..e são somente dos dias que chegam a `sem_porta`; dias excluídos por empresas/competências ou encaminhados a outras rotas devem ter classe `nao medido`, sem inferir causa (`app/chamados/services/vigia_fio.py:647`, `app/chamados/services/vigia_fio.py:656`, `app/chamados/services/vigia_fio.py:666`).

`InsumosDoDono(c, ini, fim, hoje, agora=agora).dono(data)` é a chamada adequada; passa pelo juiz real e carrega seus insumos (`app/ponto/management/commands/e6_oraculo.py:128`, `app/ponto/management/commands/e6_oraculo.py:168`). O dono lavrado de `ListaDoAdmin` é outra informação: presença e dono que a lista já apresenta; não substitui a chamada atual ao juiz (`app/chamados/services/vigia_fio.py:221`, `app/chamados/services/vigia_fio.py:229`).

Há leituras diretas de `date.today()` que um patch apenas em `timezone.now` não alcança: vitalidade e lista do admin (`app/chamados/juizes.py:560`, `app/escala/services/cadastro_realidade.py:198`). `freezegun` já está declarado precisamente para congelamento uniforme (`app/requirements.txt:39`, `app/requirements.txt:43`).

Existem saídas indiretas de logging, inclusive em falha da lista; impedir log e não imprimir texto de exceção evita que erros escapem do formato solicitado (`app/chamados/services/vigia_fio.py:218`, `app/escala/services/cadastro_realidade.py:573`, `app/core/observ.py:79`). Ausência de toda escrita indireta no restante da árvore transitiva: [nao verificado].

_1792 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
