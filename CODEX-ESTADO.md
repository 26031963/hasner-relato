# CODEX-ESTADO

> Ultima resposta processada: **05/11/2026 17:46** · publicado em **05/11/2026 17:46** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 05/11 17:46 · sessao a9e58e35a93d · - · - · origem=Ronald (tmux)

Referências conferidas na cópia: as citadas abaixo permanecem nas linhas originais; não encontrei caso “movida” ou “ausente no HEAD” nesta fatia. A verificação foi estática e somente leitura.

Para evitar sobreposição, o item 11.5 foi repartido entre: tolerâncias fixas incorporadas aos parâmetros sem consumo; degrau HE120; política CCT480. O item 14.6 foi repartido entre dedup de retratada e supressão de batidas opostas próximas.

| chave | classe | arquivo:linha confirmado | afirmação consolidada | dinheiro | competência | BACKLOG conferido | origens |
|---|---|---|---|---|---|---|---|
| A01 | defeito | app/folha/export.py:348; app/folha/export.py:359; app/folha/export.py:409; app/folha/export.py:411 | A regra do último dia da competência pode suprimir feriado apurado numa fatia anterior. | sim | n/a | FERI-06 — app/docs/BACKLOG.md:2094 | 11 |
| A02 | divergência | app/ponto/services/fechamento.py:910; app/ponto/services/fechamento.py:911; app/ponto/services/fechamento.py:913; app/folha/export.py:42 | O arredondamento separado das parcelas permite diferença entre HE total da tela e soma das rubricas TXT. | sim | n/a | nenhuma | 11 |
| A03 | defeito | app/folha/export.py:269; app/folha/services/vigia_txt.py:91 | Validador e vigia reconhecem datas de falta pelo literal 8792, embora o emissor permita remapear a rubrica. | sim | n/a | nenhuma; BUG-8792:1186 trata outro defeito | 11 |
| A04 | lacuna | app/core/regua_cct.py:72; app/core/regua_cct.py:458; app/ponto/motor_calculo_v2.py:51; app/ponto/motor_calculo_v2.py:60; app/ponto/motor_calculo_v2.py:69; app/ponto/motor_calculo_v2.py:2006 | Sete parâmetros CCT não chegam ao motor, que mantém tolerâncias e pisos próprios. | sim | n/a | cobertura parcial: PERCENTUAL-DO-NOTURNO-SEM-LEITOR:1602; TOLERANCIA-SEM-LEITOR:1605; INTERVALO-MINIMO-SEM-LEITOR:1606 | 11,12; também16 |
| A05 | risco | app/ponto/motor_calculo_v2.py:3141; app/ponto/calculador/regras.py:800; app/ponto/calculador/periodo.py:166 | O degrau após 120 minutos divide HE50/HE100 por constante, sem lastro escrito demonstrado na origem. | sim | n/a | nenhuma específica | 11,12 |
| A06 | divergência | app/core/regua_cct.py:378; app/ponto/motor_calculo_v2.py:3119; app/ponto/motor_calculo_v2.py:3131; app/ponto/calculador/regras.py:771 | A política CCT global “legais” limita o turno partido a 480 minutos e diverge do calculador paralelo. | sim | n/a | EXCEDENTE-IGNORA-O-SINDICATO:1604; O281:1523 (escopo relacionado) | 11,12 |
| A07 | defeito | app/folha/export.py:716; app/folha/export.py:981 | Eventos com valor e sem vínculo de rubrica são omitidos tanto na montagem quanto na homologação. | sim | n/a | nenhuma | 11; também16 |
| A08 | lacuna | app/folha/export.py:190; app/folha/export.py:428; app/folha/export.py:711 | O helper de dias abonados 8932 não é consumido pelo emissor de eventos. | sim | n/a | O137-8932-ENTERRAR — app/docs/BACKLOG.md:1491 | 11 |
| A09 | lacuna | app/core/models.py:40; app/core/regua_cct.py:374; app/core/regua_cct.py:458 | Cláusulas escritas apenas em Sindicato.obs não configuram os argumentos do motor. | sim | n/a | nenhuma | 12 |
| A10 | divergência | app/ponto/motor_calculo_v2.py:2767; app/ponto/motor_calculo_v2.py:3232; app/ponto/calculador/regras.py:764 | O portão de intrajornada usa duração bruta no 12x36 e líquida nos outros dois cálculos comparados. | sim | n/a | O281 — app/docs/BACKLOG.md:1523 (quatro gates de intra) | 12 |
| A11 | defeito | app/escala/models.py:17; app/ponto/calculador/regras.py:764; app/ponto/calculador/periodo.py:334 | O piso de pausa de 15 minutos para quatro a seis horas não alcança os portões de indenização. | sim | n/a | O281 — app/docs/BACKLOG.md:1523 (quatro gates de intra; cobertura ampla) | 12 |
| A12 | divergência | app/core/regua_cct.py:449; app/ponto/motor_calculo_v2.py:758 | Sem aplicação CCT vigente, o switch altera a prorrogação noturna da mesma jornada 12x36. | sim | n/a | nenhuma específica | 12 |
| A13 | divergência | app/ponto/motor_calculo_v2.py:2578; app/ponto/motor_calculo_v2.py:2639; app/ponto/motor_calculo_v2.py:3441; app/ponto/motor_calculo_v2.py:3454 | DSR comercial e agrupamento de extras pela data civil podem divergir do dia atribuído à jornada. | sim | 09/2026 sintética | O76:1320; O281:1523 | 12 |
| A14 | lacuna | app/ponto/utils.py:152; app/ponto/utils.py:191; app/ponto/utils.py:211 | A seleção de disponíveis para cobertura não valida o descanso interjornada de 11 horas. | não | n/a | nenhuma | 12 |
| A15 | lacuna | app/config/crons.py:17; app/config/crons.py:1821 | A agenda de crons fixa Juliani e o universo empresarial 2,3,4. | sim | n/a | nenhuma específica | 12; também16 |
| A16 | defeito | app/colaboradores/queries.py:85; app/colaboradores/queries.py:86 | O filtro de empresas visíveis exclui empresas ativas pelo prefixo literal de CNPJ 11222333. | não | n/a | nenhuma | 12 |
| A17 | risco | app/core/views_usuarios.py:331; app/core/views_usuarios.py:341; app/core/credenciais.py:185 | Gerir usuários permite redefinir a senha de um superusuário e receber a senha temporária. | não | n/a | RBAC-ESCRITA-POR-PAPEL:1725 | 13 |
| A18 | defeito | app/colaboradores/views.py:2271; app/colaboradores/views.py:2274; app/colaboradores/views.py:2303 | A escrita de agenda busca colaborador pelo ID sem conferir sua empresa contra o alcance do ator. | sim | n/a | RBAC-ESCRITA-POR-PAPEL:1725 | 13 |
| A19 | defeito | app/ponto/views.py:3032; app/ponto/services/cobertura.py:70; app/ponto/services/cobertura.py:92; app/ponto/services/cobertura.py:100 | A designação de cobertura aceita IDs sem conferir alcance empresarial nem vínculo obrigatório do chamado de origem. | sim | n/a | RBAC-ESCRITA-POR-PAPEL:1725 | 13 |
| A20 | defeito | app/ponto/views.py:436; app/ponto/views.py:456; app/ponto/views.py:471; app/ponto/views.py:477 | A permissão de visualizar painel permite aprovar ou rejeitar batida pendente buscada sem escopo empresarial. | sim | n/a | RBAC-ESCRITA-POR-PAPEL:1725 | 13 |
| A21 | defeito | app/colaboradores/views.py:2153; app/colaboradores/views.py:2165; app/colaboradores/services/beneficios.py:47 | A permissão de ler benefícios autoriza a gravação da configuração monetária de VA. | sim | 11/2026 sintética | RBAC-ESCRITA-POR-PAPEL:1725 | 13 |
| A22 | risco | app/core/views.py:974; app/core/views.py:978; app/core/views.py:1007 | O detalhe de batida devolve identificação, GPS, justificativa, histórico e foto sem escopo de empresa. | não | n/a | nenhuma | 13 |
| A23 | risco | app/ponto/views.py:1937; app/ponto/views.py:3024 | Escritas autenticadas por sessão dispensam CSRF nos fluxos de notificação negada e cobertura. | sim | n/a | nenhuma específica; RBAC não cobre explicitamente CSRF | 13 |
| A24 | risco | app/api/views_core.py:851; app/ponto/registro_batida.py:296; app/api/foto_recebida.py:61; app/api/foto_recebida.py:79 | O upload persiste conteúdo sem validar imagem e pode resolver a pendência foto_ausente. | sim | n/a | nenhuma | 13 |
| A25 | risco | app/api/views_core.py:19; app/api/credencial.py:186; app/config/settings/base.py:47; app/config/settings/saas.py:10 | JWT não carrega vínculo explícito ao tenant e pode atravessar schemas nas condições de credencial e chave descritas. | não | n/a | nenhuma | 13 |
| A26 | risco | app/api/credencial.py:124; app/api/credencial.py:138; app/api/credencial.py:150 | JWT legado é aceito sem iat ou quando falha a consulta ao estado de revogação. | não | n/a | nenhuma | 13 |
| A27 | risco | app/config/settings/base.py:6; mensageria/config/settings.py:3 | A ausência das variáveis de ambiente seleciona segredos literais previsíveis. | não | n/a | nenhuma | 13 |
| A28 | risco | app/core/ia_acoes.py:63; app/core/views_ia.py:864; app/core/views_ia.py:908; mensageria/nucleo/ferramentas.py:394 | A entrada da IA une permissões, mas não transmite autorização específica de domínio às ferramentas. | não | n/a | IA-DOMINIO-POR-FERRAMENTA:736 | 13 |
| A29 | risco | mensageria/nucleo/views.py:26; mensageria/nucleo/views.py:55; mensageria/nucleo/views.py:83 | Console, chat e tratamento de alertas internos não exigem identidade autenticada. | não | n/a | nenhuma | 13 |
| A30 | risco | app/sse_server.py:31; app/sse_server.py:41; app/sse_server.py:73; app/sse_server.py:93 | SSE toma existência de sessão como autorização para receber o canal agregado. | não | n/a | nenhuma | 13 |
| A31 | risco | app/api/views.py:2658; app/api/views.py:2674; app/api/views.py:2677 | Telemetria anônima admite texto livre e o emissor inclui CPF quando o pedido está autenticado. | não | n/a | nenhuma | 13 |
| A32 | defeito | app/chamados/services/regularizacao_ext.py:131; app/chamados/services/regularizacao_ext.py:136; app/chamados/services/regularizacao_ext.py:149; app/chamados/services/regularizacao_ext.py:153 | A regularização externa pode apagar a original ou sobrescrever seu horário e tipo. | sim | n/a | PERGUNTA-SEM-SELO:728 (classe 9 explicitamente); LAUDO-[nome]-COL94-1610:742 (censo de escritores) | 14 |
| A33 | defeito | app/ponto/views.py:477; app/ponto/registro_batida.py:285; app/ponto/registro_batida.py:291 | Rejeitar batida offline pendente executa exclusão física. | sim | n/a | nenhuma específica | 14 |
| A34 | defeito | app/ponto/services/flip_batida.py:25; app/ponto/registro_batida.py:254 | Flip substitui o tipo E/S da própria linha sem preservar seu tipo original nela. | sim | n/a | nenhuma específica | 14 |
| A35 | divergência | app/ponto/models.py:64; app/ponto/services/espelho.py:1022; app/ponto/turnos.py:2903; app/templates/ponto/espelho.html:76 | Espelhos de período omitem retratadas apesar da promessa de exibição com asterisco, e o feed cru não as sinaliza. | não | n/a | nenhuma específica | 14 |
| A36 | divergência | app/escala/utils.py:1123; app/templates/ponto/espelho.html:237; app/api/views.py:1471; app/relatorios/pdf_espelho.py:881 | A área Batidas pode mostrar o tipo do marco enquanto API e PDF mostram o tipo registrado. | não | n/a | nenhuma específica | 14 |
| A37 | defeito | app/api/views.py:557; app/ponto/registro_batida.py:138 | A deduplicação da API pode devolver como sucesso uma batida retratada e suprimir nova tentativa. | sim | n/a | nenhuma específica | 14; também17 |
| A38 | risco | app/ponto/registro_batida.py:21; app/ponto/registro_batida.py:138; app/ponto/services/triagem_batida.py:668 | A porta comum pode recusar uma marcação oposta distinta por proximidade temporal. | sim | n/a | nenhuma específica | 14 |
| A39 | risco | app/api/views.py:415; app/api/views.py:449; app/api/views.py:462 | GPS ausente e situação cadastral podem impedir a gravação de uma tentativa de ponto. | sim | n/a | nenhuma | 14 |
| A40 | defeito | app/api/hora_do_aparelho.py:63; app/api/hora_do_aparelho.py:141; app/api/hora_do_aparelho.py:148 | Uma batida offline com mais de 48 horas recebe como timestamp o horário de chegada. | sim | n/a | nenhuma | 14; também17 |
| A41 | lacuna | app/templates/core/glossario.html:155; app/ponto/views.py:57; app/ponto/views.py:70 | AFD permanece declarado pendente e não foi localizado gerador AEJ, cuja rota encontrada é importação aposentada. | não | n/a | nenhuma | 14 |
| A42 | lacuna | app/ponto/models.py:25; app/ponto/turnos.py:2365; app/ponto/turnos.py:2373 | Sequência reinicia por jornada e não demonstra NSR global sequencial. | não | n/a | nenhuma | 14 |
| A43 | lacuna | app/api/views.py:823; app/api/views.py:834 | A resposta de sucesso auditada não fornece hash, NSR ou comprovante assinado. | não | n/a | nenhuma | 14 |
| A44 | risco | app/ponto/registro_batida.py:254; app/ponto/registro_batida.py:263; app/ponto/registro_batida.py:291; app/core/auditoria.py:17 | Algumas alterações precedem auditoria cujas falhas são capturadas, permitindo perder a trilha correspondente. | sim | n/a | nenhuma específica | 14 |
| A45 | divergência | app/templates/core/glossario.html:144; app/templates/core/glossario.html:145; app/templates/core/glossario.html:149 | O glossário identifica o registrador por programa como REP-C e não sustenta a classificação REP-P solicitada. | não | n/a | nenhuma | 14 |

**Nota de referência:** para CSRF, `app/ponto/views.py:1937` continua sendo o mesmo sítio citado, mas o decorator exato está em **1938**, imediatamente abaixo; se a tabela final exigir a linha exata do decorator, use `1938 (movida)` ou preserve a outra referência original `3024`, que contém exatamente `@csrf_exempt`.

**Contagem desta fatia:** 45 itens na tabela, sendo 8 de origem11; 11 de origem12; 15 de origem13; 14 de origem14; três causas repetidas entre11/12. Portanto `8+11+15+14−3=45`.

**Grupos sem achado, critério sugerido para enumeração reproduzível:**

- **11: 1** — o formulário já avisa que os sete parâmetros não têm efeito (`app/templates/core/config/sindicato_form.html:74`). A descrição neutra de DSR fora do TXT não foi contada.
- **12: 5** — aviso CCT presente (`sindicato_form.html:74`); parâmetro consumido de afastamento da hora reduzida (`regua_cct.py:470`); versões oráculo separadas não demonstram dois pagamentos (`fechamento.py:1249`, `:1278`, `porta_export.py:284`); antigo desconto noturno por intervalo já compartilhado (`motor_calculo_v2.py:983`, `calculador/regras.py:63`, `:85`); seletor monetário por nome/identidade não demonstrado (`regua_cct.py:253`, `:352`).
- **13: 5** — `/me` e bater derivam o titular autenticado (`views_core.py:275`, `:388`); foto exige dono (`:846`); regularização filtra dono/estado (`:1319`); CSRF global ativo (`settings/base.py:58`); SQL injection explorável não demonstrada (`sse_server.py:23`, `:36`, `:77`).
- **14: 1** — retratação comum preserva a linha e `anotar` não permite timestamp (`registro_batida.py:203`, `:267`).

As negativas de exploração e as proteções acima são grupos declarados pela origem, não nova conclusão sobre produção.

## 05/11 17:45 · sessao 7d371a460cd4 · F3-BECO-1 · verificador · origem=Code

A construção fora do `with` está correta: `ListaDoAdmin.__init__` apenas guarda parâmetros e inicializa campos; a carga ocorre na primeira consulta à lista. O comando faz essas consultas pelos censos, dentro do lote. (`app/chamados/services/vigia_fio.py:199`, `app/chamados/services/vigia_fio.py:205`, `app/chamados/management/commands/vigiar_fio_morto.py:171`)

1. **Quando lê C6, tranca e toque**

   `_carregar()` chama `cadastro_realidade.lista(hoje=..., empresas=..., agora=...)` uma vez, quando algum método da lista exige os dados; o construtor não consulta C6, tranca, toque nem `_memo_da_tranca`. (`app/chamados/services/vigia_fio.py:199`, `app/chamados/services/vigia_fio.py:208`, `app/chamados/services/vigia_fio.py:216`, `app/chamados/services/vigia_fio.py:224`)

   No caminho F3/C11, `beco_em_lote()` chama `alimentar_portas()`, que consulta `pergunta_viva()`; o cálculo de vitalidade consulta a tranca e, havendo tranca, o toque humano posterior a `pf.fechado_em`. (`app/escala/services/cadastro_realidade.py:417`, `app/chamados/juizes.py:1302`, `app/chamados/juizes.py:1140`, `app/chamados/juizes.py:954`)

   **Construir dentro ou fora do lote não muda a leitura**, desde que a primeira consulta aconteça no mesmo contexto, como neste comando; o objeto não captura o memo no construtor. (`app/chamados/services/vigia_fio.py:199`, `app/chamados/services/vigia_fio.py:216`, `app/chamados/juizes.py:972`, `app/chamados/management/commands/vigiar_fio_morto.py:167`)

   Executar a carga com lote muda o compartilhamento do memo: o SIM do toque e o usuário de sistema passam a ser compartilhados pela rodada; empresa e tranca continuam no memo do avulso, por `lote=False`. Para estado estável, isso preserva os predicados consultados. (`app/chamados/juizes.py:940`, `app/chamados/juizes.py:966`, `app/chamados/juizes.py:1043`, `app/chamados/juizes.py:1068`)

2. **O destino de `agora` e os limites do congelamento**

   O comando lê `agora`, deriva `hoje` desse instante e entrega ambos à lista; `ListaDoAdmin` guarda e repassa `agora` ao carregador. (`app/chamados/management/commands/vigiar_fio_morto.py:163`, `app/chamados/management/commands/vigiar_fio_morto.py:167`, `app/chamados/services/vigia_fio.py:201`, `app/chamados/services/vigia_fio.py:216`)

   `cadastro_realidade.lista()` usa o instante recebido, encaminha-o ao C11 e à conciliação dos donos; o C11 o entrega a `InsumosDoDono`, cujo eventual caminho para o espelho também recebe esse instante. (`app/escala/services/cadastro_realidade.py:717`, `app/escala/services/cadastro_realidade.py:787`, `app/escala/services/cadastro_realidade.py:801`, `app/escala/services/cadastro_realidade.py:454`, `app/ponto/management/commands/e6_oraculo.py:159`)

   **Isso não estabelece um instante único para todo o grafo:** a lista também chama C2, que lê `date.today()`; o calendário chamado pelos censos lê `timezone.now()`; o C17 usado pelo C11 chega à partição de verbos, que também lê seu relógio. O repasse de F3 preservado pela resolução não elimina essas leituras. (`app/escala/services/cadastro_realidade.py:728`, `app/escala/services/cadastro_realidade.py:207`, `app/chamados/services/vigia_fio.py:271`, `app/colaboradores/services/calendario.py:163`, `app/chamados/juizes.py:1287`, `app/chamados/services/verbos.py:35`)

   O lote não congela `agora`, células, chamados, donos ou o resultado completo da lista: seus valores compartilhados são o usuário de sistema e os SIMs do toque; a chave do toque inclui colaborador, dia e `desde`. Uma tranca com outro `fechado_em` produz outra chave. (`app/chamados/juizes.py:999`, `app/chamados/juizes.py:1044`, `app/chamados/juizes.py:1049`, `app/chamados/juizes.py:1070`, `app/chamados/juizes.py:954`)

   A equivalência não deve ser declarada universal diante de concorrência: o toque também depende dos vínculos **atuais** célula/chamado, além da trilha. Alterações concorrentes desses vínculos ou da identidade do usuário de sistema em produção: **[não verificado]**. (`app/chamados/juizes.py:1071`, `app/chamados/juizes.py:1076`, `app/chamados/juizes.py:1081`)

3. **Escrita seguida de releitura pelo memo**

   Não identifiquei, nos caminhos F3 chamados pelo bloco, escrita persistente de célula, chamado ou trilha seguida de releitura que envelheça o SIM. O C11 consulta portas, âncoras, pais e cadastro e monta sua saída; `um_dono_por_dia()` altera o dicionário da lista em memória. (`app/escala/services/cadastro_realidade.py:417`, `app/escala/services/cadastro_realidade.py:426`, `app/escala/services/cadastro_realidade.py:444`, `app/escala/services/cadastro_realidade.py:458`, `app/escala/services/cadastro_realidade.py:934`)

   A leitura dos pais usa `pais_limbo_vivos()`; o escritor `reatar_perguntas_ao_limbo()` é outra função, que chama `reapontar_resolvedora()`, e não é chamado pelo carregador C11. (`app/escala/services/cadastro_realidade.py:428`, `app/chamados/services/limbo_folgas.py:212`, `app/chamados/services/limbo_folgas.py:224`, `app/chamados/services/limbo_folgas.py:243`)

   As escritas F3 de regeneração/rejulgamento permanecem na porta de célula, fora desse caminho de leitura; a projeção de célula para julgamento apenas cria uma cópia em memória. (`app/ponto/portas/celula.py:253`, `app/ponto/portas/celula.py:270`, `app/ponto/portas/celula.py:289`, `app/ponto/portas/celula.py:301`, `app/ponto/portas/celula.py:705`)

   **Ressalva ao “zero escrita” absoluto:** consultar o usuário de sistema chama `get_user_sistema()`, que executa `User.objects.get_or_create()`. Pode haver criação de usuário, embora isso não seja escrita F3 de célula/chamado/trilha nem o ciclo de invalidação perguntado; existência desse usuário em produção: **[não verificado]**. (`app/chamados/juizes.py:1070`, `app/core/utils.py:18`)

4. **Quem escreve e quem lê**

   | Estado | Quem escreve | Quem lê |
   |---|---|---|
   | `lista.sem_resposta` | A propriedade não tem escritor: retorna o tamanho de `_sem_resposta`; `dia_do_admin()` e `papel_do_dia()` acrescentam pares distintos ao conjunto quando falta resposta. (`app/chamados/services/vigia_fio.py:226`, `app/chamados/services/vigia_fio.py:244`, `app/chamados/services/vigia_fio.py:255`) | `censo()` e, depois dos dois censos, o comando. (`app/chamados/services/vigia_fio.py:489`, `app/chamados/management/commands/vigiar_fio_morto.py:177`) |
   | `d['fio_do_admin_sem_resposta']` | `censo()` inicializa e preenche; o comando sobrescreve com o total final da lista compartilhada, incluindo o alcance. (`app/chamados/services/vigia_fio.py:366`, `app/chamados/services/vigia_fio.py:489`, `app/chamados/management/commands/vigiar_fio_morto.py:177`) | `linhas()` imprime; o comando inclui o dicionário no carimbo. (`app/chamados/services/vigia_fio.py:884`, `app/chamados/management/commands/vigiar_fio_morto.py:221`) |
   | Dict do lote | `tranca_em_lote()` instala `{}`; `_admin_tocou_o_dia_depois()` grava somente SIM; `_toque_humano_no_dia()` grava o PK de sistema. (`app/chamados/juizes.py:999`, `app/chamados/juizes.py:1049`, `app/chamados/juizes.py:1070`) | `_memo_da_tranca()` entrega o dict; o toque consulta o SIM e exclui o usuário de sistema das testemunhas humanas. O contexto restaura o anterior ao sair, inclusive por exceção. (`app/chamados/juizes.py:972`, `app/chamados/juizes.py:1045`, `app/chamados/juizes.py:1072`, `app/chamados/juizes.py:1003`) |

5. **Classes de reprovação, limitadas à resolução**

   - **Segundo juiz / derivação paralela:** não encontrados na costura; ela preserva as chamadas existentes, e o C11 consulta C6/C17 e o juiz do dono. (`app/chamados/management/commands/vigiar_fio_morto.py:167`, `app/chamados/management/commands/vigiar_fio_morto.py:172`, `app/escala/services/cadastro_realidade.py:417`, `app/escala/services/cadastro_realidade.py:459`)
   - **Segundo escritor fora da porta:** não encontrado nos caminhos F3 executados pelo lote; os escritores de reata e regeneração são caminhos separados dos leitores acima. (`app/chamados/services/limbo_folgas.py:212`, `app/chamados/services/limbo_folgas.py:224`, `app/ponto/portas/celula.py:253`)
   - **Relógio fora da borda:** o repasse solicitado pelo F3 foi preservado; persistem os relógios internos explicitados no item 2, sem nova leitura de relógio introduzida pelo lote. (`app/chamados/management/commands/vigiar_fio_morto.py:163`, `app/chamados/management/commands/vigiar_fio_morto.py:167`, `app/chamados/juizes.py:994`)
   - **Ordem de trava:** o contexto instala um `ContextVar`; não abre transação nem adquire trava de banco, portanto não acrescenta ordem de aquisição de locks. (`app/chamados/juizes.py:994`)
   - **Pouso antes do pré-requisito:** a ordem local está satisfeita: lote antes das cinco medições; contador final depois dos dois censos. Execução de testes, pré-requisitos operacionais e situação de produção: **[não verificado]**. (`app/chamados/management/commands/vigiar_fio_morto.py:171`, `app/chamados/management/commands/vigiar_fio_morto.py:177`)

Há testes escritos para paridade lote/avulso, frescor do NÃO, reabertura/tranca, instalação do lote e repasse de `agora`; resultado de execução nesta auditoria: **[não verificado]**. (`app/chamados/tests/test_snc2_toque_em_lote.py:113`, `app/chamados/tests/test_snc2_toque_em_lote.py:137`, `app/chamados/tests/test_snc2_toque_em_lote.py:171`, `app/chamados/tests/test_snc2_toque_em_lote.py:200`, `app/escala/tests/test_f3_beco1_folga_na_lista.py:208`)

**APROVA a resolução de merge. Bloqueantes identificados no hunk: nenhum.** (`app/chamados/services/vigia_fio.py:199`, `app/chamados/services/vigia_fio.py:216`, `app/chamados/juizes.py:940`, `app/chamados/juizes.py:1043`, `app/chamados/management/commands/vigiar_fio_morto.py:167`)

## 05/11 17:45 · sessao 8192d62cfd23 · - · - · origem=Ronald (tmux)

Contagem bruta conferida: **lote6.r: 11 achados / 21 sem achado; lote7.r: 12 / 17; lote8.r: 13 / 16**. São 36 ocorrências antes das fusões; todas têm referências verificáveis na origem. Referências abaixo continuam no HEAD, sem trechos ausentes; linhas mais precisas dentro do mesmo bloco não representam movimentação.

| origem/item | classe | arquivo:linha conferido | afirmação consolidável | dinheiro sugerido | competência | cobertura específica |
|---|---|---|---|---|---|---|
| lote6.r/01 | b | app/docs/LEIS.md:81; app/folha/export.py:1235 | A coluna antiga da L-033 ainda afirma retenção universal, embora o retentor isente furo exclusivamente de pausa. | sim | n/a | nenhuma |
| lote6.r/02 | a | app/ponto/supra_juiz.py:330; app/ponto/supra_juiz.py:348; app/ponto/supra_juiz.py:372 | A justificativa remove `FURO_COM_REGUA_DEGENERADA`, apesar da promessa de preservar acusações de cadastro. | sim | n/a | nenhuma |
| lote6.r/03 | b | app/docs/LEIS.md:71; app/ponto/portas/celula.py:233; app/ponto/portas/celula.py:259 | A correção fora da vigência reescreve DNA sem atualizar DNA anterior, instante e contador de regeneração. | sim | n/a | nenhuma |
| lote6.r/04 | b | app/docs/LEIS.md:93; app/ponto/portas/celula.py:196 | A proibição absoluta de corrigir exportada ficou superada pela exceção `apesar_da_lavra`. | sim | n/a | nenhuma |
| lote6.r/05 | b | app/docs/LEIS.md:149; app/ponto/portas/celula.py:198; app/ponto/portas/celula.py:238 | A regeneração ordinária continua barrada por exportação, apesar da revogação dessa trava pela L-113. | sim | n/a | O121-A1-VINCULO-CORRIGE-O-PASSADO — app/docs/BACKLOG.md:1008 |
| lote6.r/06 | b | app/docs/LEIS.md:100; app/docs/LEIS.md:167; app/ponto/services/cartorio.py:1105; app/ponto/services/cartorio.py:1423; app/ponto/supra_juiz.py:278 | O diagnóstico documental de proteção temporal limitada a hoje ficou antigo diante da consulta de encerramento do turno. | sim | n/a | nenhuma para a divergência documental |
| lote6.r/07 | d | app/escala/models.py:1699; app/ponto/management/commands/gerar_celulas.py:16; app/ponto/management/commands/gerar_celulas.py:96 | O help text anuncia DNA versão 1, mas o produtor grava versão 2. | nao | n/a | nenhuma |
| lote6.r/08 | a | app/ponto/supra_juiz.py:264; app/ponto/supra_juiz.py:530 | O comentário restringe a parcial ao portão do furo, mas ela também impede a classificação de cobrança em dia coberto. | sim | n/a | nenhuma para a divergência documental |
| lote6.r/09 | e | app/ponto/tests/test_carimbo_isencao_lampada.py:29; app/ponto/management/commands/processar_cartorio.py:27; app/ponto/management/commands/processar_cartorio.py:65; app/ponto/services/cartorio.py:900 | A docstring diz que `--forcar` é somente sombra, embora possa ser combinado com escrita. | sim | n/a | nenhuma |
| lote6.r/10 | e | app/ponto/tests/test_carimbo_isencao_lampada.py:23; app/ponto/services/cartorio.py:1362 | O alcance histórico ZERO de 1.202 lâmpadas não é demonstrado pelo dado fornecido. [nao verificado] | sim | n/a | nenhuma |
| lote6.r/11 | a | app/escala/services/leitor_celula.py:204; app/escala/services/leitor_celula.py:213; app/escala/utils.py:850; app/escala/utils.py:858 | A isenção modifica julgamento de batida real e alcança folga e célula sem marco, contrariando a docstring do leitor. | nao | n/a | nenhuma |
| lote7.r/01 | d | app/colaboradores/models.py:46; app/colaboradores/views.py:499; app/ponto/janelas.py:51 | O cadastro anuncia corte entre 1 e 28, mas view e motor elevam 1 para 2. | sim | n/a | FERI-11 — app/docs/BACKLOG.md:2099 |
| lote7.r/02 | a | app/ponto/views.py:1248; app/ponto/services/fechamento.py:37; app/ponto/services/fechamento.py:552 | O comentário promete recálculo irrestrito de exportadas, mas a guarda exclui empresas exportadas de setembro/2026. | sim | 09/2026 | nenhuma para esse comentário |
| lote7.r/03 | a | app/folha/views.py:213; app/folha/views.py:247 | Entrar no TXT não garante aprovação, porque o carimbo exclui itens com determinadas travas. | sim | n/a | nenhuma específica |
| lote7.r/04 | a | app/api/views.py:1662; app/relatorios/cartao_pela_celula.py:340; app/relatorios/cartao_pela_celula.py:344 | O comentário descreve duas fontes de totais, mas o fluxo também fornece `lavratura`. | nao | n/a | nenhuma |
| lote7.r/05 | d | app/ponto/models.py:757; app/ponto/models.py:875; app/ponto/models.py:883 | O help text fixa falta inteira em 720, mas a derivação automática usa o previsto diário e preserva minutos positivos informados. | sim | n/a | nenhuma para o help text |
| lote7.r/06 | d | app/ponto/models.py:584; app/ponto/services/ausencia.py:333; app/ponto/services/ausencia.py:779 | O help text promete recusa sem comprovante, mas a ausência pode nascer aguardando documento. | sim | n/a | nenhuma |
| lote7.r/07 | d | app/ponto/models.py:586; app/api/views_core.py:402; app/api/views_core.py:680 | `bloqueia_ponto` promete impedir registro, mas colaborador afastado pode registrar batida e receber aviso. | sim | n/a | nenhuma |
| lote7.r/08 | b | app/docs/LEIS.md:78; app/docs/LEIS.md:161; app/api/views_core.py:402; app/api/views_core.py:440; app/api/views_core.py:458 | A promessa absoluta de nunca barrar batida conflita com recusas por férias e GPS ausente. | sim | n/a | nenhuma |
| lote7.r/09 | e | app/api/tests/test_api_bater_ponto.py:533; app/api/views.py:600; app/api/views.py:608; app/api/tests/test_api_bater_ponto.py:569 | A docstring espera segunda entrada distante como E e disputa, mas o fluxo ao vivo corrige o tipo e o teste espera E/S. | sim | n/a | nenhuma |
| lote7.r/10 | d | app/colaboradores/models.py:115; app/folha/porta_export.py:230; app/folha/porta_export.py:691 | O help text omite que a trava de HE considera somente dias acima do limite e sem decisão. | sim | n/a | O214, item 3 — app/docs/BACKLOG.md:1134 |
| lote7.r/15 | b | app/docs/LEIS.md:174; app/api/views_mensageria.py:723; app/api/views_mensageria.py:883 | O mês civil também é escolhido com zero cortes, contrariando a descrição de uso somente quando eles divergem. | nao | n/a | nenhuma específica |
| lote7.r/29 | b | app/docs/LEIS.md:99; app/core/tests/test_smoke_chromium.py:35 | O dado fornecido não comprova o smoke humano nas duas cascas antes do push exigido pela L-072. [nao verificado] | nao | n/a | nenhuma |
| lote8.r/01 | a | app/config/crons.py:1614; app/config/crons.py:1669; app/config/crons.py:1701; app/config/crons.py:1802; bin/encadeado.sh:102 | Os comentários dizem que `log` não tem leitor nos encadeados, mas renderização e executor o utilizam. | nao | n/a | nenhuma específica |
| lote8.r/02 | a | app/config/crons.py:2119; app/folha/services/vigia_txt.py:231; app/folha/services/vigia_txt.py:358; app/folha/services/vigia_txt.py:367 | O cadastro diz que o vigia TXT consulta duas autoridades, mas o censo retirou a classificação do dono. | nao | n/a | nenhuma |
| lote8.r/03 | a | app/config/crons.py:1533; app/config/crons.py:1618 | O vigia TXT é descrito como único elo sem escrita, embora também exista o vigia de pausa. | nao | n/a | nenhuma |
| lote8.r/09 | b | app/docs/LEIS.md:94; app/config/crons.py:1483; app/config/crons.py:1995; app/chamados/management/commands/disparar_perguntas_competencia.py:280 | A varredura agenda emissão com `--apply`, contrariando a cláusula de cron que somente alarma. | sim | n/a | DISPARAR-PERGUNTAS-NO-CONTRATO-20 — app/docs/BACKLOG.md:780 |
| lote8.r/12 | b | app/docs/LEIS.md:127; app/config/crons.py:1955; app/config/crons.py:2114 | O diagnóstico de sete papéis sem `prazo` e 27 juízes ficou antigo diante do cadastro atual. | nao | n/a | nenhuma para a prosa velha |
| lote8.r/13 | b | app/docs/LEIS.md:191; app/config/crons.py:2062; app/config/crons.py:2069; app/config/crons.py:2071; app/config/crons.py:2084 | A lista documental ainda trata como juízes de varredura comandos que já têm papel `prazo`. | nao | n/a | nenhuma para a prosa velha |
| lote8.r/14 | b | app/docs/LEIS.md:194; app/config/crons.py:1955; app/config/crons.py:2114; bin/tests/test_papel_prazo_nao_deriva.sh:247; bin/tests/test_papel_prazo_nao_deriva.sh:295 | O índice declara pendentes o papel `prazo` e sua guarda, embora ambos existam. | nao | n/a | nenhuma para a prosa velha |
| lote8.r/18 | a | app/config/crons.py:1922; app/config/crons.py:2069 | O comentário ainda mantém o par relâmpago em `juiz`, mas o cadastro já o atribui a `prazo`. | nao | n/a | nenhuma para esse comentário |
| lote8.r/19 | a | app/ponto/management/commands/censo_fase_12x36.py:270; app/ponto/management/commands/censo_fase_12x36.py:299; app/ponto/management/commands/censo_fase_12x36.py:306; app/ponto/management/commands/censo_fase_12x36.py:314 | A lavra do censo é descrita como somente contador, mas também contém itens, cobertura e critério. | nao | n/a | nenhuma para a divergência documental |
| lote8.r/20 | a | app/config/crons.py:453; app/ponto/management/commands/tripwire_situacao_afastado.py:35; app/ponto/management/commands/tripwire_situacao_afastado.py:39; app/ponto/management/commands/tripwire_situacao_afastado.py:51 | Falta saída datada que demonstre o zero exigido para autorizar os leitores de `situacao`. [nao verificado] | sim | n/a | nenhuma |
| lote8.r/22 | d | app/colaboradores/models.py:370; app/colaboradores/queries.py:53; app/colaboradores/queries.py:65; app/ponto/management/commands/detectar_ausencias.py:98 | O help text exige flag individual de operação, mas a consulta usa rollout da empresa e vínculo vigente. | sim | n/a | nenhuma |
| lote8.r/23 | d | app/colaboradores/models.py:228; app/ponto/management/commands/detectar_ausencias.py:470; app/ponto/management/commands/detectar_ausencias.py:473; app/core/configuracao_efeito.py:123 | `tolerante_offline` promete adiar falta, mas perdeu seu leitor e o gate usa vencimento de marcos e encerramento do dia. | sim | n/a | nenhuma |
| lote8.r/26 | e | app/ponto/tests/test_vigia_reconciliar_vinculo.py:1; app/ponto/tests/test_vigia_reconciliar_vinculo.py:25; app/config/crons.py:2138 | A docstring mantém `reconciliar_vinculo` como juiz, mas o cadastro e a asserção atual o classificam como vigia. | nao | n/a | nenhuma para a divergência documental |

Fusões com outros lotes: lote6/01 = lote1/01 = lote2/28; lote6/03 = lote4/10; lote6/04 = lote1/03 = lote4/11; lote6/05 = lote1/07 = lote4/12; lote6/07 = lote4/05; lote7/02 = lote1/18; lote7/03 = lote1/10; lote7/05 = lote1/33 = lote2/18 = lote3/15; lote7/06 = lote3/12; lote7/07 = lote3/13; lote7/10 compartilha causa com lote1/05.

**Atenção à fusão de lote7/08:** ela agrega a recusa de férias de lote3/16 e a recusa de GPS de lote5/E1; esses dois são causas distintas no mesmo endpoint, enquanto a divergência da L-030 é uma formulação documental mais ampla.

**Coberturas que não considerei específicas:** `LOG-DO-CRON-ORFAS-SUMIU` em BACKLOG:732 trata da falta de log, não dos comentários antigos; `PAPEL-PRAZO-CABECALHO` em BACKLOG:1668 trata de outro arquivo; `FERI-10` em BACKLOG:2098 trata de outro trecho da consulta de competência.

## 05/11 17:45 · sessao 8c6126039d63 · - · - · origem=Ronald (tmux)

Todos os trechos abaixo existem na linha indicada no HEAD e sustentam o mecanismo inventariado; não precisei marcar referência como movida ou ausente.

`B:NN` significa `app/docs/BACKLOG.md:NN`. Não usei correspondência temática ampla como cobertura.

| origem/item | classe | referência conferida | afirmação consolidável | dinheiro | competência | cobertura BACKLOG |
|---|---|---|---|---|---|---|
| 15/1 | risco | app/ponto/management/commands/processar_cartorio.py:61; app/ponto/services/cartorio.py:1306; app/ponto/services/cartorio.py:1307; app/ponto/services/cartorio.py:1362 | O cartório percorre histórico sem limite inferior, consulta ausência e varre batidas por célula antes da guarda de impressão inalterada. | nao | n/a | nenhuma |
| 15/2 | risco | app/ponto/portas/celula.py:598; app/escala/models.py:1286; app/escala/models.py:1310 | O gerador consulta folgas por vínculo/dia antes de pular células e grava criações e alterações individualmente. | nao | n/a | nenhuma |
| 15/3 | risco | app/ponto/portas/celula.py:872; app/ponto/portas/celula.py:875 | A apuração de furos executa atualização individual por dia e renova a avaliação mesmo sem mudança funcional. | nao | n/a | nenhuma |
| 15/4 | risco | app/ponto/services/he_pendente_lavrado.py:92; app/ponto/services/he_pendente_lavrado.py:94; app/ponto/services/he_pendente_lavrado.py:132 | O lavrador de HE monta espelho por ativo antes de descartar dias sem HE pendente. | nao | n/a | LAVRAR-HE-PENDENTE-ESCALA, B:750, **cobertura parcial**, pois a linha registra segunda apuração e piso visual, mas não promete eliminar espelho por ativo |
| 15/5 | risco | app/escala/views_plano_folgas.py:59; app/escala/views_plano_folgas.py:66; app/escala/views_plano_folgas.py:76; app/escala/models.py:1092 | O GET do Plano de Folgas consulta relação por vínculo e célula por dia sem carga prévia. | nao | n/a | nenhuma |
| 15/6 | risco | app/escala/views.py:407; app/escala/views.py:411; app/escala/views.py:412; app/escala/views.py:416; app/escala/folgas.py:192 | Aplicar calendário ao tipo repete a regeneração dos vínculos numa transação coletiva síncrona. | nao | n/a | nenhuma |
| 15/7 | risco | app/core/views.py:177; app/core/views.py:376; app/core/views.py:779; app/core/views.py:1181; app/core/views.py:798 | Quatro painéis consultam turno individualmente, e o filtro de empresa da TV acontece depois dessa apuração. | nao | n/a | nenhuma; O113 é painel situacional, expressamente separado pela origem |
| 15/8 | risco | app/ponto/views.py:376; app/ponto/views.py:392; app/ponto/views.py:405 | A tela de turnos abertos percorre todos os ativos e consulta chamados antes de aplicar o filtro de nível. | nao | n/a | nenhuma |
| 15/9 | risco | app/folha/export.py:942; app/folha/export.py:1188; app/folha/export.py:1194 | O classificador da folha carrega células e vínculos individualmente por fechamento. | nao | n/a | nenhuma |
| 15/10 | risco | app/folha/views.py:723; app/folha/views.py:764; app/folha/previa.py:58; app/folha/previa.py:170 | A prévia classifica e consulta a população inteira antes da paginação e repete retenção para propostas. | nao | n/a | nenhuma |
| 15/11 | risco | app/relatorios/furos.py:124; app/relatorios/furos.py:160; app/colaboradores/services/calendario.py:221 | A auditoria de espelhos realiza carga coletiva e depois recria calendários com cargas individuais por card. | nao | n/a | FUROS-EMPRESA-TIMEOUT, B:1775 |
| 16/1 | defeito | app/colaboradores/views.py:499; app/ponto/janelas.py:15; app/ponto/janelas.py:26 | O corte 1 é convertido em 2 e impede representar competência civil 1–31. | sim | 10/2026 e 11/2026, exemplos sintéticos | FERI-11, B:2099 |
| 16/2 | divergência | app/tenants/views.py:48; Caddyfile:25; app/config/settings/saas.py:115 | O provisionamento e o wildcard aceitam novo domínio que a lista SaaS de hosts não permite. | nao | n/a | nenhuma |
| 16/3 | lacuna | app/config/crons.py:17; app/config/crons.py:18; app/config/crons.py:462; app/config/crons.py:1826 | A agenda fixa tenant Juliani e empresas 2, 3 e 4, deixando novo tenant e novas empresas fora da expansão. | sim | n/a | nenhuma |
| 16/4 | lacuna | app/core/regua_cct.py:72; app/ponto/motor_calculo_v2.py:2006 | Sete parâmetros CCT cadastráveis não são consumidos pelo cálculo. | sim | n/a | PERCENTUAL-DO-NOTURNO-SEM-LEITOR B:1602; TOLERANCIA-SEM-LEITOR B:1605; INTERVALO-MINIMO-SEM-LEITOR B:1606, **cobertura parcial dos sete** |
| 16/5 | divergência | app/core/models.py:85; app/escala/models.py:17; app/escala/models.py:352; app/escala/models.py:597 | O sindicato aceita intervalo mínimo de 30 minutos que a validação fixa da escala recusa acima de seis horas. | sim | n/a | INTERVALO-MINIMO-SEM-LEITOR B:1606 é relacionado, porém não cobre explicitamente a recusa da escala; recomendação conservadora: nenhuma |
| 16/6 | lacuna | app/tenants/bootstrap.py:27; app/tenants/bootstrap.py:34; app/colaboradores/models.py:46; app/colaboradores/models.py:151; app/core/regua_cct.py:444; app/core/regua_cct.py:449; app/core/regua_cct.py:366 | O bootstrap não configura os requisitos trabalhistas e a CCT depende de switch e aplicação vigente sem empate. | sim | n/a | nenhuma; O309 trata aviso do empate, não bootstrap incompleto |
| 16/7 | lacuna | app/folha/export.py:29; app/folha/export.py:846 | Empresa sem integração e pessoa sem código Domínio encontram pré-requisitos cadastrais que impedem exportar. | sim | n/a | nenhuma |
| 16/8 | defeito | app/folha/export.py:716; app/folha/export.py:717; app/folha/export.py:981; app/folha/export.py:982 | Evento com valor sem rubrica vinculada desaparece da montagem e da homologação sem retenção por essa ausência. | sim | n/a | nenhuma |
| 16/9a | risco | app/folha/export.py:996; app/folha/export.py:1037; app/folha/export.py:1042 | A memória da janela usa empresa e competência sem tenant e pode reutilizar janela de outro schema. | sim | 10/2026, exemplo sintético | nenhuma |
| 16/9b | risco | app/pautas/context_processors.py:23; app/pautas/context_processors.py:29; app/pautas/context_processors.py:33 | O cache de pautas usa somente ID do usuário e pode compartilhar número e remetentes entre tenants. | nao | n/a | nenhuma |
| 16/10 | defeito | app/core/views_ia.py:23; app/core/views_ia.py:909; mensageria/nucleo/views.py:136 | O assistente envia o cliente global configurado em vez do tenant da requisição. | nao | n/a | nenhuma; IA-DOMINIO-POR-FERRAMENTA B:736 é autorização por ferramenta, causa distinta |
| 16/11 | lacuna | app/core/constants.py:5; app/ponto/services/fechamento.py:204; app/escala/alimentacao.py:390 | O piso global de operação de Juliani exclui fatias e previsto anteriores a 21/05/2026 de qualquer cliente. | sim | 05/2026, exemplo sintético | nenhuma |
| 16/12 | divergência | app/folha/porta_export.py:809; app/folha/porta_export.py:810; app/folha/porta_export.py:812 | A mensagem de recuperação do export manda executar a medição no schema Juliani mesmo para outro tenant. | sim | n/a | nenhuma |
| 17/1 | defeito | app/static/pwa.js:50; app/static/pwa.js:113; app/static/pwa.js:128; app/api/views_core.py:389 | A fila offline não guarda titular e pode enviar a batida de A como pertencente à sessão de B. | sim | n/a | PWA-OFFLINE-APP-SHELL, B:1745 |
| 17/2a | defeito | app/static/pwa.js:109; app/static/pwa.js:114; app/static/pwa.js:147; app/api/hora_do_aparelho.py:141 | A fila guarda horário corrigido pelo gap, mas envia somente o horário bruto do aparelho. | sim | n/a | nenhuma |
| 17/2b, prosa | defeito | app/api/hora_do_aparelho.py:63; app/api/hora_do_aparelho.py:148; app/static/pwa.js:158 | Batida offline com mais de 48 horas é gravada no horário da chegada e a pendência é apagada após sucesso HTTP. | sim | n/a | nenhuma; **fundir 14/8** |
| 17/3 | lacuna | app/static/pwa.js:143; app/ponto/models.py:106; app/api/hora_do_aparelho.py:148 | O reenvio offline não leva chave estável do evento e pode duplicar tentativa antiga cujo horário vira novo agora. | sim | n/a | nenhuma |
| 17/4 | defeito | app/api/views_core.py:551; app/api/views_core.py:558; app/static/pwa.js:158 | A deduplicação pode devolver batida retratada como sucesso e apagar a nova pendência sem entrada ativa. | sim | n/a | nenhuma; **fundir parcela retratada de 14/6** |
| 17/5 | divergência | app/static/pwa.js:117; app/static/pwa.js:143; app/ponto/services/geofence.py:117; app/ponto/services/geofence.py:121 | O reenvio offline perde a precisão GPS e muda a classificação da mesma localização perante o envio online. | nao | n/a | nenhuma |
| 17/6 | divergência | app/templates/ponto/bater_ponto_v2.html:340; app/api/views_core.py:592; app/api/views_core.py:600 | A tela anuncia sucesso com E/S local sem ler o tipo efetivamente corrigido pelo servidor. | nao | n/a | nenhuma |
| 17/7a | defeito | app/static/sw.js:46; app/static/sw.js:50; app/static/sw.js:67; app/static/sw.js:74 | A navegação retorna antes da gravação do cache HTML e a visita online não prepara a tela para reabertura offline. | nao | n/a | PWA-OFFLINE-APP-SHELL, B:1745 |
| 17/7b, prosa | lacuna | app/static/sw.js:116; app/static/sw.js:121; app/static/sw.js:122 | O Background Sync sem página aberta somente notifica para abrir o aplicativo e não envia a fila. | sim | n/a | nenhuma |
| 17/8 | divergência | app/config/settings/base.py:93; app/core/views.py:14; app/templates/ponto/bater_ponto_v2.html:163; app/templates/ponto/bater_ponto_v2.html:168 | O relógio passa da hora do servidor ao fuso local do aparelho quando entra no fallback. | nao | n/a | nenhuma |
| 17/9 | defeito | app/api/views.py:1071; app/api/views.py:1156; app/ponto/models.py:701; app/api/views.py:3101 | O estado antigo pendente faz o contrato de chamados ocultar anexo faltante em aguardando_documento. | nao | n/a | nenhuma; API-NAO-TEM-PORTA-DE-ANEXAR... B:1052 refere-se à porta, não a esse estado morto |
| 17/10 | divergência | app/api/views_core.py:249; app/api/views_core.py:250; app/api/views.py:939; app/escala/models.py:1124 | As APIs anunciam horários do template mesmo quando a autoridade do vínculo lê horários congelados na célula. | nao | n/a | nenhuma |
| 17/11 | lacuna | app/api/views_core.py:1298; app/api/views_core.py:1302; app/chamados/models.py:3214 | O payload de regularização omite operação, alvo e intervalo e torna operações diferentes indistinguíveis. | sim | n/a | nenhuma |
| 17/12 | divergência | app/api/views_core.py:1332; app/chamados/services/regularizacao_ext.py:58; app/chamados/services/regularizacao_ext.py:352 | A confirmação que falha em materializar batidas devolve HTTP 200 embora o corpo sinalize falha. | sim | n/a | nenhuma |
| 17/13 | divergência | app/api/views.py:1172; app/api/views.py:2553; app/chamados/services/disputa_emissao.py:1267 | Um GET oferece OUTRO_MOTIVO para pergunta de hora enquanto outro remove a opção e o POST a recusa. | nao | n/a | nenhuma; API-CHAMADOS-V2-SERVE-PERGUNTA-SEM-A-LEI B:1577 trata pergunta viva/morta, não filtragem das opções |
| 17/14 | defeito | app/static/pwa.js:246; app/static/pwa.js:251; app/static/pwa.js:367; app/ponto/views.py:2000 | A inscrição push anuncia sucesso sem conferir a recusa HTTP do servidor e a assinatura local impede renovação automática. | nao | n/a | nenhuma |
| 17/15 | risco | app/static/pwa.js:335; app/ponto/views.py:1989; app/chamados/push_utils.py:24 | A associação WebPush permanece no titular anterior após troca da conta no navegador. | nao | n/a | nenhuma |
| 17/16 | divergência | app/chamados/push_utils.py:28; app/static/sw.js:97; app/static/sw.js:107 | O SW ignora URL e requireInteraction enviados pelo emissor e abre a raiz sem persistência solicitada. | nao | n/a | nenhuma |

Observações para a consolidação:

- 16/4 deve fundir com 11/4 e 12/1; seu exemplo de tolerância também encosta em 11/5.
- 16/8 deve fundir com 11/6.
- 16/3 deve fundir com 12/universo fixo dos crons.
- 17/2b deve fundir com 14/8.
- 17/4 deve fundir com a parcela de deduplicação de retratadas de 14/6; a recusa de opostas próximas do mesmo item 14 tem causa adicional.
- Não marquei 16/7 como conforme: a origem a incluiu expressamente na tabela de achados como lacuna CADASTRO, embora delimite que os bloqueios não demonstram defeito estrutural.
- A referência `app/core/constants.py:3` da prosa16 é comentário; a tabela de origem também fornece a referência correta `:5`, portanto usei `:5` sem “movida”.
- 15/7 usa chamadas `:177/:376/:779/:1181`, imediatamente depois dos laços citados pela origem; não houve movimento de trecho, apenas âncora mais específica.
- 15/11 tem a carga coletiva na linha124, continuação da linha123 citada pela origem; não houve movimento de trecho.

Contagem de achados de origem antes das fusões entre anexos: **15=11; 16=13; 17=18**, contando separadamente em17 a transformação >48h e a limitação do Background Sync, ambas afirmadas na prosa.

Contagem sugerida de afirmações expressamente sem achado, por proposição delimitadora: **15=3** (situacional plural; retrato materializado; caderno em processo próprio); **16=6** (motores além do12x36; trava de âncora específica; semente sistema; semente tipos de ausência; semente rubrica HE100; permissões por signal); **17=3** (divergência de fuso não comprova ISO incorreto; FCM distribuído não demonstrado divergente; mudança histórica sem versão não comprovada). Esses números dependem de o consolidado contar controles demonstrados como afirmações sem achado; se o critério for somente rótulo literal de veredito, os três anexos não têm tabelas de “conforme/sem achado”.

## 05/11 17:45 · sessao 683897038ae3 · - · - · origem=Ronald (tmux)

Conferência somente leitura: referências abaixo continuam nas linhas citadas, sem deslocamento ou ausência nos trechos conferidos. Contagem bruta: lote1.r = **17 achados + 17 sem achado**; lote2.r = **20 achados + 16 sem achado**. As contagens derivam dos anexos, não de arquivo local. [nao verificado]

Todos os caminhos abaixo são relativos a `app/`. “Nenhuma” significa que não identifiquei cobertura específica após consulta ao BACKLOG; cobertura não explicitada permanece [nao verificado].

| chave de consolidação | classe original | referências conferidas | afirmação consolidável | dinheiro | competência | cobertura específica conferida | origem |
|---|---|---|---|---|---|---|---|
| Retenção universal de furo | b | docs/LEIS.md:81; folha/export.py:1147; folha/export.py:1235 | A coluna antiga de L-033 ainda descreve retenção universal, enquanto o retentor isenta furo exclusivamente de pausa. | sim | n/a | O236, docs/BACKLOG.md:1164 cobre a implementação da isenção, mas não explicita correção da coluna antiga | lote1.r:01; lote2.r:28 |
| Exportada intocável | b | docs/LEIS.md:93; ponto/portas/regen_exportada.py:106; ponto/portas/regen_exportada.py:174 | A proibição absoluta de corrigir folha emitida diverge da porta de retificação com autor e motivo. | sim | n/a | O121, docs/BACKLOG.md:1189 cobre regeneração em exportada; O121-A1-VINCULO-CORRIGE-O-PASSADO, docs/BACKLOG.md:1008 | lote1.r:03 |
| TXT ainda chama motor | b | docs/LEIS.md:113; folha/views.py:208; ponto/services/fechamento.py:277 | A geração do TXT recalcula em leitura e alcança o motor apesar da promessa de somente ler a lavratura. | sim | n/a | O-DIA-PAGO S4, docs/BACKLOG.md:1095 | lote1.r:04 |
| Trava HE não universal | b | docs/LEIS.md:115; folha/porta_export.py:230; folha/porta_export.py:690 | A trava de HE considera somente dias acima do limite sem decisão e depende do campo da empresa, contrariando a formulação universal. | sim | n/a | O214, docs/BACKLOG.md:1134, item 3 explicitamente cobre limite e configuração | lote1.r:05 |
| Regeneração ordinária ainda bloqueada | b | docs/LEIS.md:149; ponto/portas/celula.py:196; ponto/portas/celula.py:199 | A regeneração ordinária continua submetida à guarda de exportação quando não recebe `apesar_da_lavra`. | sim | n/a | O121-A1-VINCULO-CORRIGE-O-PASSADO, docs/BACKLOG.md:1008; O121, docs/BACKLOG.md:1189 | lote1.r:07 |
| DIFF prévio não demonstrado | b | docs/LEIS.md:149; folha/management/commands/regerar_txt_dominio.py:99; folha/management/commands/regerar_txt_dominio.py:118 | Os artefatos do regerador não demonstram publicação do DIFF antes do ato de frota. | sim | n/a | nenhuma | lote1.r:08 |
| NULL não basta para vigência | d | folha/models.py:97; folha/models.py:134; folha/models.py:137 | `invalidada_em` nulo não garante vigência porque somente a geração mais recente é vigente. | sim | n/a | nenhuma | lote1.r:09; lote2.r:19 |
| TXT não garante aprovação | a | folha/views.py:213; folha/views.py:247; folha/views.py:268 | Um colaborador classificado como `entra` pode permanecer no TXT sem receber aprovação quando tem trava considerada pelo carimbo. | sim | n/a | FECH-05, docs/BACKLOG.md:2069 cobre o sítio `_apv` e a pergunta de aprovação | lote1.r:10 |
| Parcela feriado 12x36 | e | folha/tests/test_export_59a.py:1; folha/tests/test_export_59a.py:48; folha/export.py:411 | A docstring de teste proíbe universalmente parcela de feriado em 12x36, mas a emissão ocorre com cláusula de dobra ligada. | sim | n/a | nenhuma | lote1.r:11 |
| Recalcular qualquer exportada | a | ponto/services/fechamento.py:476; ponto/services/fechamento.py:551; ponto/services/fechamento.py:553 | A generalização de que exportação não barra recálculo omite a guarda específica de setembro de 2026. | sim | 09/2026 | nenhuma específica para essa divergência de comentário | lote1.r:18 |
| Congelamento não universal | a | ponto/services/fechamento.py:1441; ponto/services/fechamento.py:1144; folha/porta_export.py:417 | O comentário de imutabilidade de competência trancada ou paga diverge do escritor que pode atualizar valores fora da guarda de setembro. | sim | 09/2026 é a guarda; 10/2026 é o contraexemplo | nenhuma específica; O80 em docs/BACKLOG.md:1324 é histórico da antiga recusa | lote1.r:19; lote2.r:20 |
| Ninguém lê DiaPago | a | ponto/services/fechamento.py:1182; colaboradores/services/calendario.py:290; ponto/services/dia_pago.py:852 | O comentário de que ninguém lê as linhas de `DiaPago` está desatualizado diante dos leitores existentes. | nao | n/a | O-DIA-PAGO S3, docs/BACKLOG.md:1090 cobre os leitores; não explicita a correção desse comentário | lote1.r:24 |
| Avulso apaga anterior | a | holerite/models.py:138; holerite/views.py:1191; holerite/views.py:1193 | O upload avulso apaga holerite anterior sem ciência apesar da promessa de preservar versões. | nao | n/a | nenhuma; PORT-70 em docs/BACKLOG.md:1987 só cobre porta/smoke genericamente | lote1.r:25 |
| Empresa folha do avulso | d | holerite/models.py:123; holerite/views.py:1199; holerite/views.py:1202 | O upload avulso grava a empresa atual do colaborador apesar do help definir a empresa impressa no documento. | nao | n/a | nenhuma | lote1.r:26 |
| Carta duplicada na republicação | a | holerite/models.py:143; holerite/motor.py:448; holerite/motor.py:475; holerite/motor.py:480 | A primeira publicação conjunta não preenche `anexos_sha`, permitindo anexar novamente a mesma carta na republicação. | nao | n/a | nenhuma | lote1.r:29 |
| CPF e 29/29 históricos | a | holerite/motor.py:180; holerite/motor.py:186 | A docstring afirma ausência de CPF e resultado histórico 29/29 sem documentos ou cadastro verificáveis nos anexos. | nao | n/a | nenhuma | lote1.r:32 |
| Falta inteira 720 | d | ponto/models.py:757; ponto/models.py:875; ponto/models.py:883 | O help fixa falta inteira em 720 minutos, mas a derivação automática soma o previsto diário e preserva valores positivos informados. | sim | n/a | nenhuma para esse help; H06-FALTA-PELA-GRADE em docs/BACKLOG.md:1742 cobre outro caminho, materialização com 720 fixo | lote1.r:33; lote2.r:18 |
| Tolerância 20 versus 10 HE | a | ponto/motor_calculo_v2.py:53; ponto/motor_calculo_v2.py:2845; ponto/motor_calculo_v2.py:2851 | A promessa de tolerar vinte minutos diários sem HE diverge do ramo que elimina excedentes de até dez minutos. | sim | n/a | nenhuma; TOLERANCIA-SEM-LEITOR em docs/BACKLOG.md:1605 trata campos de sindicato, outro defeito | lote2.r:01 |
| Entrada três horas depois | a | ponto/motor_calculo_v2.py:2104; ponto/motor_calculo_v2.py:2122; ponto/motor_calculo_v2.py:2148 | O comentário exclui genericamente atraso após três horas, mas o código restringe essa exclusão ao dia sem marcos de intervalo. | sim | n/a | nenhuma | lote2.r:02 |
| Feriado previsto pode dobrar | a/b | ponto/motor_calculo_v2.py:2807; ponto/motor_calculo_v2.py:934; docs/LEIS.md:118 | O comentário e a cláusula original excluem dobra em feriado previsto, enquanto o helper decide por política de dobra e feriado. | sim | n/a | nenhuma específica para a divergência documental | lote2.r:03; lote2.r:31 |
| Intra 4h–6h | c | ponto/motor_calculo_v2.py:3226; ponto/motor_calculo_v2.py:3232; escala/models.py:19 | O ramo comercial não indeniza pausa faltante em duração acima de quatro e até seis horas apesar do piso consultado pelo motor. | sim | n/a | O270, docs/BACKLOG.md:1511 | lote2.r:04 |
| Dedup núcleo retirado | a | ponto/calculador/alimentacao.py:333; ponto/calculador/nucleo.py:38; ponto/calculador/nucleo.py:186 | A narrativa ainda atribui deduplicação de dez minutos ao núcleo que já registra a retirada e percorre todas as batidas. | sim | n/a | nenhuma | lote2.r:11 |
| Perdão entrada proximidade | e | ponto/tests/test_calculador_portoes_de_marco.py:8; ponto/calculador/regras.py:251; escala/regua_defesa.py:472 | A docstring descreve perdão da entrada por proximidade da volta, mas o código consulta marco ocupado e conformidade. | sim | n/a | nenhuma específica; MOTOR-PASSA-O-TURNO-A-VOLTA em docs/BACKLOG.md:779 trata resíduo do alinhamento por período | lote2.r:16 |
| Flag intra não operacional | b | docs/LEIS.md:79; ponto/motor_calculo_v2.py:822; ponto/motor_calculo_v2.py:2784 | A cláusula original atribui indenização a `intervalo_indenizavel`, enquanto o motor declara não ler essa flag e mantém indenização do suprimido. | sim | n/a | nenhuma | lote2.r:26 |
| Intervalo nem sempre subtraído | b | docs/LEIS.md:80; ponto/motor_calculo_v2.py:2773; ponto/motor_calculo_v2.py:2784 | A afirmação de que intervalo cadastrado sempre sai da jornada diverge do tratamento que preserva trabalho sem pausa reconhecida e indeniza o suprimido separadamente. | sim | n/a | nenhuma específica; BUG-HE-INTRA-DOBRADA em docs/BACKLOG.md:1081 é correção distinta do limite de HE | lote2.r:27 |
| Pós05 sem CLT declarada | b | docs/LEIS.md:83; core/regua_cct.py:453; ponto/motor_calculo_v2.py:758 | O fallback do construtor pode excluir prorrogação pós-05h em 12x36 sem CLT declarada, contrariando o “somente” documental. | sim | n/a | nenhuma | lote2.r:29 |
| Flag em produção sem prova | b | docs/LEIS.md:84; core/regua_cct.py:150 | A constante é falsa na cópia, mas a afirmação sobre seu valor efetivo em produção não está demonstrada. | sim | n/a | nenhuma | lote2.r:30 |
| Tipo gravado decide fallback | b | docs/LEIS.md:119; ponto/motor_calculo_v2.py:573; ponto/turnos.py:1380 | Sem alimentação útil da ata, o pareador pode usar `Batida.tipo`, contrariando a exclusão universal desse campo. | sim | n/a | nenhuma específica; COL369-TRES-RESTANTES em docs/BACKLOG.md:1118 é um caso associado, sem equivalência causal demonstrada | lote2.r:32 |
| Rótulo manda no dispatch | b | docs/LEIS.md:124; ponto/motor_calculo_v2.py:3635; ponto/motor_calculo_v2.py:3668 | O dispatch continua usando `tipo_base` e só aplica correção geométrica a certos rótulos de partido. | sim | n/a | O136, docs/BACKLOG.md:1346 | lote2.r:33 |
| DiaPago substitui passado | b | docs/LEIS.md:113; ponto/services/fechamento.py:1211; ponto/services/dia_pago.py:357; ponto/services/dia_pago.py:358 | Lavraturas sucessivas em `versao='motor'` apagam e recriam as linhas anteriores apesar da promessa de preservar passado em nova versão. | sim | n/a | nenhuma específica; O196 em docs/BACKLOG.md:1407 reconhece delete/recreate para reversão, mas não cobre preservação de versões históricas | lote2.r:34 |
| versao_motor já preenchida | b | docs/LEIS.md:170; ponto/services/fechamento.py:1214; ponto/services/dia_pago.py:345 | A nota de que ninguém preenche `versao_motor` diverge do fechamento que passa o carimbo e do escritor que o grava. | nao | n/a | nenhuma específica; GRAVADO-NAO-CARIMBA-A-REGUA em docs/BACKLOG.md:1586 trata FechamentoMensal sem carimbo, outro sítio | lote2.r:35 |
| Resíduo arredondamento teto | b | docs/LEIS.md:111; ponto/motor_calculo_v2.py:1194; ponto/motor_calculo_v2.py:1196 | O arredondamento separado de atraso e antecipada pode produzir soma ligeiramente superior ao teto aritmético. | sim | n/a | nenhuma | lote2.r:36 |

Duplicatas adicionais com outros lotes, para consolidação central: retenção universal → lote6.r:01; exportada intocável → lote4.r:11/lote6.r:04; regeneração bloqueada → lote4.r:12/lote6.r:05; TXT não garante aprovação → lote7.r:03; recálculo versus guarda09 → lote7.r:02; congelamento não universal → lote5.r:B1; falta inteira720 → lote3.r:15/lote7.r:05; rótulo manda no dispatch → lote4.r:13. Estas equivalências são derivadas dos anexos. [nao verificado]

## 05/11 17:45 · sessao 218d96043d83 · - · - · origem=Ronald (tmux)

Conferi em leitura os anexos 18–20. Todas as referências abaixo existem; a referência documental de `O261-b` mudou de `BACKLOG.md:2098` para `:2115` **(movida)**. Nenhum trecho ficou ausente no HEAD.

`B:NN` abaixo significa `app/docs/BACKLOG.md:NN`.

| origem | classe | arquivo:linha conferido | afirmação consolidável | dinheiro | competência | cobertura conferida |
|---|---|---|---|---|---|---|
| 18 | defeito | `app/folha/export.py:840` | Qualquer demissão não nula exclui do TXT mensal, inclusive quando posterior à janela examinada. | sim | 10/2026 no exemplo | H12-DEMISSAO-NA-JANELA — B:1741 |
| 18 | defeito | `app/chamados/services/materializacao.py:519`; `app/ponto/models.py:875` | A materialização de `FALTA_DIA` informa 720 minutos, impedindo a derivação pela jornada quando os minutos já vieram preenchidos. | sim | n/a | H06-FALTA-PELA-GRADE — B:1742 |
| 18 | defeito | `app/ponto/services/dia_pago.py:190`; `:328` | A rubrica do calculador pode sobrescrever o atraso já reduzido pelo crédito diário com o atraso bruto. | sim | n/a | V3-LAVRAR-ATRASO-CREDITO — B:858 |
| 18 | defeito | `app/ponto/motor_calculo_v2.py:440`; `:110` | O recorte isolado da HE pode perder a prorrogação noturna reconhecida para o turno original. | sim | n/a | V3-HE-NOTURNA-POR-PECA — B:933 |
| 18 | defeito | `app/ponto/motor_calculo_v2.py:94`; `:110`; `:116` | A função noturna pode sobrepor faixas em recortes longos e retorna zero acima de 26 horas. | sim | n/a | V3-NOTURNO-RECORTE-LONGO — B:939 |
| 18 | defeito | `app/ponto/motor_calculo_v2.py:3095`; `:3323`; `:3337`; `:3352` | Os ramos Comercial e TurnoPartido podem atribuir HE100 de feriado sem recompor seu componente noturno antes do `continue`. | sim | n/a; BACKLOG documenta 10/2026 | V3-MOTOR-FERIADO-HE100-NOTURNA — B:937 |
| 18 | defeito | `app/ponto/calculador/alimentacao.py:390`; `:404`; `:528` | O calculador resolve flags CCT na data inicial da janela e reutiliza-as nos dias seguintes. | sim | 11/2026 no exemplo | V3-RETOMA-FILA, item 3 — B:943 |
| 18 | lacuna | `app/ponto/models.py:399`; `app/folha/export.py:668`; `:698` | O fechamento não possui estado `encerrado`, cuja introdução isolada poderia divergir entre classificação e montagem do TXT. | sim | n/a | STATUS-ENCERRADO-DA-COMPETENCIA-NAO-EXISTE — B:2130 |
| 18 | divergência | `app/chamados/services/materializacao.py:78`; `app/chamados/juizes.py:446`; `app/ponto/turnos.py:2632` | Uma ausência parcial sem período pode calar o materializador enquanto o juiz da célula mantém o furo. | sim | n/a | COBPARC-AUSENCIA-CALA-DOIS-JUIZES — B:769 |
| 18 | divergência | `app/escala/models.py:1280`; `:1309`; `app/ponto/precedencia.py:237` | O gerador consulta folgas do próprio vínculo enquanto o juiz aceita folga manual da pessoa vinculada a contrato encerrado. | sim | n/a | GRADE-FOLGA-GERADOR — B:936 |
| 18 | divergência | `app/chamados/management/commands/auditar_invariantes_chamados.py:1531`; `:1541`; `app/ponto/precedencia.py:237` | O auditor C5 pode acusar trabalho correto ao considerar folga de calendário de vínculo encerrado que o juiz rejeita. | não | n/a | GRADE-FOLGA-C5 — B:935 |
| 18 | defeito | `app/escala/services/fase_conflitante.py:71`; `:80`; `app/escala/alimentacao.py:228` | O medidor de fase aplica o vínculo ativo a todos os dias da janela sem escolher o vínculo vigente em cada data. | não | 10–11/2026 no exemplo | FASE-CONFLITANTE-VINCULO-DO-DIA — B:812 |
| 18 | lacuna | `app/ponto/admin.py:18`; `app/ponto/services/justificativa.py:199`; `:214`; `app/ponto/signals.py:161` | A aprovação de justificativa pelo Admin contorna a porta de encerramento da origem, invalidação do fechamento e log de domínio, embora permaneça o signal do dia. | sim | n/a | JUSTIFICATIVA-ADMIN-FORA-DA-PORTA — B:781 |
| 18 | risco | `app/core/constants.py:13`; `app/ponto/services/cartorio.py:1140`; `:1229`; `:1419` | Leituras diretas da flag de isenção coexistem com o juiz declarado, criando risco de divergência caso apenas a função seja alterada. | sim | n/a | ISENTO-PONTO-JUIZ-REGISTRADO — B:920 |
| 18 | lacuna | `app/colaboradores/models.py:294`; `app/core/constants.py:18`; `app/ponto/services/cartorio.py:520` | A isenção não possui vigência e a flag atual pode alterar o insumo de rejulgamento de dias anteriores sob a guarda de versão existente. | sim | 10/2026 no exemplo | ISENTO-FLAG-ATEMPORAL — B:921 |
| 18 | risco | `app/docs/BACKLOG.md:744` | O relato documental registra julgamento diferente da mesma célula entre sombra e produção, sem insumos fornecidos para determinar causa ou estado atual. | sim | 10/2026 | SOMBRA-X-PROD-COL988-124639 — B:744 |
| 18 | risco | `app/docs/BACKLOG.md:764` | O relato de atas defasadas contém medidas contestadas e não permite confirmar quantidade afetada ou causa atual. | sim | 09/2026 | ATA-DEFASADA-COMP09 — B:764 |
| 18 | risco | `app/docs/BACKLOG.md:1618` | O relato O327 registra cobertura incompleta do oráculo por campos sem dono, sem evidência fornecida para confirmar o residual atual. | sim | 10/2026 | O327 — B:1618 |
| 18 | risco | `app/docs/BACKLOG.md:1573` | O relato registra carimbo `celula_absolveu` com célula atualmente acusando, sem comparação temporal que determine passivo e dono. | não | n/a | CARIMBO-QUE-A-CELULA-DESMENTE — B:1573 |
| 18 | risco | `app/docs/BACKLOG.md:2115` **(movida)** | O lote documental de perguntas absolvidas por idade não tem execução, reavaliação e residual atual demonstrados pelos dados fornecidos. | não | 08–09/2026 no relato | O261-b — B:2115 |
| 19 | divergência | `app/escala/utils.py:325`; `app/ponto/services/cartorio.py:677`; `app/escala/services/leitor_celula.py:121`; `app/ponto/juiz_batida.py:86`; `app/ponto/motor_calculo_v2.py:596`; `app/ponto/turnos.py:1653` | A representação `HH:MM` descarta precisão e identidade, levando leitor e juiz a reconstruir instantes e motor e pareador a resolver colisões por minuto de formas distintas. | sim | n/a | nenhuma cobertura específica localizada; O118 em B:1194 documenta colisão textual semelhante, mas não esta obra |
| 19 | risco | `app/ponto/services/espelho.py:267`; `:614`; `:783` | O espelho continua derivando realizado, turno ausente e estado aberto, derivações que o mapa por marco proposto não substituiria sozinho. | sim | n/a | O-PROJECOES — B:733; PROGRAMA-CELULA-E-O-DIA — B:787 |
| 19 | risco | `app/colaboradores/services/calendario.py:718`; `app/ponto/services/dia_decidido.py:179` | O calendário combina minutos da ata com ausência de turno para derivar `ata_sem_turno`. | sim | n/a | O-PROJECOES — B:733; PROGRAMA-CELULA-E-O-DIA — B:787 |
| 19 | risco | `app/ponto/services/espelho.py:762`; `app/colaboradores/services/calendario.py:378`; `app/relatorios/dia_da_frota.py:20` | Espelho, calendário e relatório da frota extraem separadamente os marcos faltantes. | sim | n/a | PROGRAMA-CELULA-E-O-DIA — B:787 |
| 19 | risco | `app/folha/export.py:1124`; `:1127` | A exportação continua derivando retenção por furo somente de pausa dos papéis devolvidos pelo juiz. | sim | n/a | nenhuma específica; não confundir com O254 em B:1442, que registra outra alteração da retenção |
| 19 | risco | `app/ponto/services/cartorio.py:570` | A montagem documenta dependência da ata da passada anterior para obter o papel da batida. | sim | n/a | nenhuma específica localizada |
| 19 | lacuna | `app/ponto/motor_calculo_v2.py:559` | A alimentação autoritativa dos períodos declara cobertura incompleta para turno partido. | sim | n/a | nenhuma específica localizada |
| 20 | risco | `app/core/tests/marca_cron15.py:24`; `app/core/management/commands/crons.py:97`; `app/config/crons_duracao.json:78` | Mantidos prazo e duração da cópia, a exceção de cron termina após 13/11 e o render de instalação volta a propagar `CronNaoCabe`. | não | n/a; data 14/11/2026 | CRON-NAO-CABE-SOMBRA-87-MIN — B:747; F3-PORTA-E-CRON-PRAZO — B:1739 |
| 20 | defeito | `app/ferias/services.py:399`; `app/ponto/services/ausencia.py:1165`; `:1195` | A remarcação comum limpa somente o intervalo novo e pode conservar ausências de férias do intervalo anterior. | sim | 11–12/2026 no exemplo | nenhuma |
| 20 | defeito | `app/holerite/motor.py:323`; `:344`; `:467` | Um recibo publicado pode impedir a importação posterior de outro tipo para o mesmo colaborador e competência. | não | 11/2026 no exemplo | nenhuma |
| 20 | risco | `app/folha/management/commands/pre_fechamento.py:89`; `:91`; `app/pautas/models.py:57` | Após a virada da competência, o pré-fechamento deixa de atualizar a pauta anterior ainda viva. | não | 11–12/2026 no exemplo | nenhuma |
| 20 | defeito | `app/config/crons.py:644`; `app/ponto/management/commands/mapa_divergencia.py:93`; `app/ponto/janelas.py:27` | O cron do mapa envia competência civil e deixa de examinar os primeiros dias da competência seguinte após o corte. | não | 11–12/2026 no exemplo | nenhuma |
| 20 | lacuna | `app/core/feriados.py:27`; `app/ponto/precedencia.py:386` | Sem a linha de feriado cadastrada, a tabela de importação não impede que o juiz conserve o dia como trabalho. | sim | 11/2026 no exemplo | nenhuma |
| 20 | lacuna | `app/core/datas_cravadas.py:25`; `:86`; `app/core/management/commands/crons.py:92` | O censo de datas exclui testes embora um comando de produção importe deles o prazo CRON-15. | não | n/a | nenhuma |

Contagens para consolidar:

- **18:** 20 achados; **22 afirmações resolvidas/conformes** fora da lista: P5; FUROS-LEITOR-PARCIAL; FASE-DONO-AS-OF; FECH-01 a FECH-19. São 15 “AINDA VALE” mais cinco riscos documentais na tabela de origem.
- **19:** 7 itens consolidados, provenientes de 10 manifestações de achado: primeiros quatro da tabela fundidos em um, quatro derivações mantidas como riscos e duas lacunas/riscos da prosa. **Quatro blocos sem achado:** campos já existentes; chamadas à mesma autoridade não equivalem a cópias da lei; limite de cadastro; limite de batida. Se a contagem “por anexo” for anterior à deduplicação, registrar **10**, explicando a fusão.
- **20:** 7 achados, seis numerados mais lacuna do censo na prosa. **Dois blocos sem achado claro:** literal da vigência CCT é comentário e não vence no recorte; funções puras tratam a virada dezembro/janeiro. A nota de férias coletivas não afirma defeito nem conformidade jurídica; apenas ausência de evidência.

Total desta fatia após fusão: **34 itens**, sendo **26 dinheiro=sim**, **0 não verificáveis** e **12 sem cobertura**, usando as coberturas amplas explicitamente indicadas acima. Distribuição: **11 defeitos, 7 lacunas, 4 divergências, 12 riscos**. Confira a classificação/contagem contra as linhas antes de usar: a tabela é a fonte prevalente.

## 05/11 17:45 · sessao 1295076d2e5b · - · - · origem=Ronald (tmux)

No grafo F3 examinado, não encontrei bloqueante introduzido pela resolução: `beco_em_lote` consulta C6/C17 e monta a assinatura; o escritor de reata é função separada, chamada apenas quando explicitamente invocada (`app/escala/services/cadastro_realidade.py:417`, `:421`, `:459`; `app/chamados/services/limbo_folgas.py:224`, `:243`).

- `agora` é armazenado no construtor sem leitura de estado e encaminhado na carga preguiçosa (`app/chamados/services/vigia_fio.py:199`, `:201`, `:216`). A lista o passa a C11 e à conciliação dos donos (`app/escala/services/cadastro_realidade.py:717`, `:787`, `:801`); C11 o entrega a `InsumosDoDono` (`:454`). O fallback do espelho recebe o mesmo instante (`app/ponto/management/commands/e6_oraculo.py:159`).
- C11 exige `folga_nao_definida` (`app/escala/services/cadastro_realidade.py:451`); com esse insumo, o juiz devolve CADASTRO (`app/ponto/management/commands/e6_oraculo.py:98`). `InsumosDoDono.dono` retorna antes de solicitar espelho nessa condição (`:175`, `:180`, `:182`).
- `alimentar_portas` consulta fila, verbos e perguntas, montando dicts (`app/chamados/juizes.py:1287`, `:1290`, `:1300`, `:1304`); `_no_app` chama `perguntas_do_app` (`:1319`), que apenas consulta e filtra por C6 (`app/chamados/services/pergunta_no_app.py:46`, `:53`).
- O pai LIMBO é consultado por `pais_limbo_vivos`, sem reatar perguntas (`app/chamados/services/limbo_folgas.py:212`, `:217`, `:221`). A reata usa outra função e a porta `reapontar_resolvedora` (`:224`, `:243`).
- Os escritores F3 de célula e trilha pertencem a `regenerar_celulas_vinculo`: criação e alteração da célula (`app/ponto/portas/celula.py:253`, `:270`), trilha do rejulgamento do mês (`:301`) e rejulgamento pela porta do cartório (`:402`). Esse caminho não é chamado pela composição de C11 apresentada (`app/escala/services/cadastro_realidade.py:396`, `:469`).
- O SIM consulta vínculos atuais de célula e chamado, portanto sua monotonicidade não deve ser justificada apenas por “a trilha cresce” (`app/chamados/juizes.py:1076`, `:1081`, `:1083`). Não identifiquei escritor F3 desses vínculos invocado pelo lote; afirmar imutabilidade geral desses vínculos seria [não verificado].

Ressalva obrigatória sobre relógio: passar `agora` não uniformiza todos os leitores transitivos do censo. A jornada C2 lê `date.today()` (`app/escala/services/cadastro_realidade.py:207`, `:728`); o calendário chamado pelo censo lê seu próprio `timezone.now()` (`app/chamados/services/vigia_fio.py:270`; `app/colaboradores/services/calendario.py:132`, `:163`, `:250`); C17 consulta os relógios da fila e da partição de verbos (`app/chamados/juizes.py:1287`, `:1290`; `app/chamados/services/painel.py:188`; `app/chamados/services/verbos.py:35`). São leituras independentes do memo da tranca; classificá-las como regressão introduzida pelo hunk exigiria histórico não disponível na cópia [não verificado].

APROVA quanto à interação F3 × lote examinada; bloqueantes identificados: nenhum (`app/chamados/management/commands/vigiar_fio_morto.py:167`, `:171`, `:177`).

## 05/11 17:44 · sessao 482d7ed4e7b6 · - · - · origem=Ronald (tmux)

- `ListaDoAdmin.__init__` só armazena argumentos, inicializa caches e um `set`; nenhuma leitura C6/tranca/toque ocorre ali. A carga acontece em `_carregar`, chamada pelos predicados; ela passa `agora` a `cadastro_realidade.lista`. Portanto construir antes do `with` não antecipa a leitura. (`app/chamados/services/vigia_fio.py:199-218`, `:222-251`)
- O lote é um `ContextVar` com `set({})`, `yield` e `reset`, reentrante; não adquire trava de banco nem abre transação. Não cria ordem nova de locks. (`app/chamados/juizes.py:990-1003`)
- `_memo_da_tranca` devolve o dict do lote quando habilitado; fora dele usa request ou instância do colaborador. Empresa e período fechado usam explicitamente `lote=False`, logo não são compartilhados entre colaboradores pelo novo lote. (`app/chamados/juizes.py:966-979`, `:937-948`)
- Quem escreve o dict do lote: `tranca_em_lote` cria o dict; `_toque_humano_no_dia` escreve `('sistema',): pk`; `_admin_tocou_o_dia_depois` escreve somente `True` na chave `('toque', colab.pk, data, desde)`. Quem lê: `_memo_da_tranca`, o teste `memo.get(k)` e o filtro que exclui o usuário de sistema. O `False` nunca é armazenado. (`app/chamados/juizes.py:999`, `:1043-1049`, `:1068-1072`)
- A chave SIM inclui `desde`, recebido de `pf.fechado_em`; tranca com outro timestamp não reutiliza esse SIM. (`app/chamados/juizes.py:954`, `:1044`)
- A leitura do toque compõe trilha e vínculos atuais: cobrar pelo colaborador/data; resolver pelas células atuais; desarquivar pelos chamados atualmente ligados às células. Assim, “trilha só cresce” não prova sozinho paridade universal diante de alterações concorrentes desses vínculos. A ocorrência dessas alterações em produção está [não verificado]. (`app/chamados/juizes.py:1071-1084`)
- Há uma escrita condicional indireta: `get_user_sistema()` usa `User.objects.get_or_create`, embora sua docstring diga que o usuário deve existir no deploy. Isso pode criar **User** se ausente, e não constitui escrita F3 de célula/chamado/trilha; existência real do usuário está [não verificado]. (`app/chamados/juizes.py:1068-1070`, `app/core/utils.py:12-18`)
- Os leitores adicionais do `with` consultam e contam: `viva_em_disputa_fechada` chama C6; `respondida_sem_resposta` lê texto; `materializada_viva` conta listas produzidas por C6 e pelos juízes da competência. (`app/chamados/services/vigia_fio.py:746-759`, `:782-786`, `:803-819`, `:847-851`)
- O C11 F3 usa `alimentar_portas` e `porta_alcancavel_do_dia`; não reimplementa C6/C17 nesse caminho. (`app/escala/services/cadastro_realidade.py:417-423`, `app/chamados/juizes.py:1300-1306`, `:1375-1387`)
- A projeção de célula é cópia rasa em memória, sem `save`, e devolve a instância original sem insumos de julgamento; não é segundo escritor persistente. (`app/chamados/juizes.py:383-397`, `app/ponto/portas/celula.py:705-721`)
- Existem testes de paridade lote/avulso, NAO→SIM, mudança de vínculo, reabertura/tranca e instalação/limpeza do lote pelo comando. Resultado da execução nesta auditoria: [não verificado]. (`app/chamados/tests/test_snc2_toque_em_lote.py:113-164`, `:171-213`)

**Conclusão do subescopo: APROVA, sem bloqueante concreto de interação F3 × memo no hunk.** Construção lazy e instalação do lote estão combinadas no comando. (`app/chamados/services/vigia_fio.py:199-218`, `app/chamados/management/commands/vigiar_fio_morto.py:167-177`)

## 05/11 17:44 · sessao 76317a1e3b44 · - · - · origem=Ronald (tmux)

Evidência estática dos testes, sem execução: [nao verificado]

- Equivalência lote/avulso: matriz de perguntas vivas e mortas, incluindo toque humano/sistema, toque anterior e posterior, célula e chamado de outro colaborador — `app/chamados/tests/test_snc2_toque_em_lote.py:113-135`.
- Frescor do NÃO: toque escrito após primeira consulta é visto na próxima; chamado desarquivado que ganha célula também é relido — `app/chamados/tests/test_snc2_toque_em_lote.py:137-164`.
- Empresa/tranca fora do lote: testes cobrem reabrir e trancar entre colaboradores diferentes — `app/chamados/tests/test_snc2_toque_em_lote.py:171-198`.
- Costura do comando: teste verifica `censo` dentro do contexto e contexto limpo ao sair — `app/chamados/tests/test_snc2_toque_em_lote.py:200-213`. Esse teste intercepta somente `censo`, não cada uma das cinco chamadas.
- Relógio F3: teste força relógio da máquina em outra competência e passa `agora`; outro verifica repasse por `ListaDoAdmin`; o teste AST exige keyword `agora` no comando — `app/escala/tests/test_f3_beco1_folga_na_lista.py:196-230`.
- Uma lista compartilhada e falha distinta por dia: `app/chamados/tests/test_o261e_f5_alcance.py:227-248`.
- Leitura final da falha, inclusive dia presente somente em alcance: `app/chamados/tests/test_o261e_f5_alcance.py:263-293`.

Pontos para a consolidação:

- `ListaDoAdmin.__init__` só armazena argumentos e inicializa campos; a chamada real está em `_carregar`, acionada pelos métodos de consulta — `app/chamados/services/vigia_fio.py:199-218`, `:222-255`.
- Não afirmar zero escrita universal: o memo chama `get_user_sistema()` e essa função usa `get_or_create`, portanto pode inserir `auth_user` se ausente — `app/chamados/juizes.py:1068-1070`, `app/core/utils.py:18`. Existência desse usuário em produção: `[não verificado]`.
- Falha do carregador chama observabilidade que altera contadores em memória e envia logging; esse corpo não grava `LogAuditoria` — `app/chamados/services/vigia_fio.py:219-220`, `app/core/observ.py:73-82`.
- O helper `tranca_em_lote` só instala/reseta `ContextVar`; não abre transação nem adquire trava de banco — `app/chamados/juizes.py:993-1003`.
- As autoridades F3 usadas no C11 estão compostas por chamadas ao juiz existente: `alimentar_portas`/`porta_alcancavel_do_dia`, `chamado_que_lista`, `cadastro_do_dia`, `InsumosDoDono` — `app/escala/services/cadastro_realidade.py:396-405`, `:417-459`. Nenhuma alteração de autoridade aparece no hunk em `app/chamados/management/commands/vigiar_fio_morto.py:167-177`.

Nenhum bloqueante identificado por esta revisão dos testes e da costura estática; execução dos testes e resultado de produção: `[não verificado]`. [nao verificado]

## 05/11 17:28 · sessao 351339669556 · PD-JUIZ-3 · censo · origem=Code

1. **As citações diretas do censo seguem válidas; nenhuma aponta para corpo alterado pelo delta.** As referências no código novo são:

| Linha do censo | Classificação | Referência nova |
|---|---|---|
| §1 — leitor de `TurnoMaterializado`, testemunha de resolução | Segue válida | `app/chamados/juizes.py:1478`; `app/chamados/juizes.py:1479` |
| §1 — leitor de contexto, `dia_do_fato` | Segue válida | `app/chamados/juizes.py:80`; `app/chamados/juizes.py:93` |
| §1 — prioridade da célula ligada | Segue válida | `app/chamados/juizes.py:91`; `app/chamados/juizes.py:92` |
| §2 — data do fato prioriza célula/contexto | Segue válida | `app/chamados/juizes.py:91`; `app/chamados/juizes.py:93` |
| §3 — armadilha de competência: fato mais recente das perguntas vivas | Segue válida | `app/chamados/juizes.py:2036`; `app/chamados/juizes.py:2050`; `app/chamados/juizes.py:2054` |
| §3 — armadilha da célula sem ata: indeterminação não significa folga | Segue válida | `app/chamados/juizes.py:418`; `app/chamados/juizes.py:428` |
| RED R9 — indeterminado sem ata | Segue válida | `app/chamados/juizes.py:418`; `app/chamados/juizes.py:428` |

A competência, embora preserve seu corpo, tem dependência indireta alterada: seleciona perguntas por `pergunta_viva`, cujo cálculo consulta a tranca e chega ao memo do toque. (`app/chamados/juizes.py:2031`; `app/chamados/juizes.py:1140`; `app/chamados/juizes.py:954`; `app/chamados/juizes.py:1043`)

2. **Há novo estado transitório de memo e uma extração de leitor; não há novo produtor de evento nesses trechos.** O contexto cria o dicionário da rodada; `_admin_tocou_o_dia_depois` grava nele somente o SIM, e `_toque_humano_no_dia` guarda o identificador do usuário de sistema. (`app/chamados/juizes.py:999`; `app/chamados/juizes.py:1049`; `app/chamados/juizes.py:1070`)

O novo helper concentra leituras já presentes no delta fornecido: trilha de auditoria, células do colaborador/dia e chamados ligados a essas células. São estados usados na decisão que o consumidor alcança por `veredito_orfa` → `pergunta_viva` → tranca → toque. (`app/chamados/juizes.py:1071`; `app/chamados/juizes.py:1076`; `app/chamados/juizes.py:1081`; `app/chamados/reconciliador.py:1329`; `app/chamados/reconciliador.py:1103`; `app/chamados/reconciliador.py:1054`; `app/chamados/juizes.py:1140`; `app/chamados/juizes.py:954`)

Há uma escrita persistente **possível e preexistente** por baixo da leitura do usuário: `get_user_sistema` usa `get_or_create`. O delta muda sua frequência e memoriza o PK; não introduz essa possibilidade de criação. (`app/chamados/juizes.py:1068`; `app/chamados/juizes.py:1070`; `app/core/utils.py:18`)

As lacunas produtoras permanecem: `criar` termina com criação e trilha, e `recompute_turnos` termina com exclusão/recriação do materializado, sem emissão de evento nesses corpos. (`app/chamados/models.py:1048`; `app/chamados/models.py:1049`; `app/chamados/models.py:1050`; `app/ponto/nucleo.py:57`; `app/ponto/nucleo.py:60`; `app/ponto/nucleo.py:61`)

3. **Pode conservar um SIM velho, condicionalmente; isso não demonstra ocorrência no outbox.** Quem abre o lote no fluxo mostrado é `vigiar_fio_morto.handle`, envolvendo as cinco medições. O comando do consumidor abre outra trava, `consumidor_exclusivo`, e chama o consumo fora daquele bloco. (`app/chamados/management/commands/vigiar_fio_morto.py:171`; `app/chamados/management/commands/vigiar_fio_morto.py:172`; `app/chamados/management/commands/vigiar_fio_morto.py:176`; `app/chamados/management/commands/consumir_eventos_celula.py:19`; `app/chamados/management/commands/consumir_eventos_celula.py:31`)

Se o consumidor executar dentro de um lote já aquecido, `_memo_da_tranca` devolve o dicionário existente; um SIM retorna imediatamente, sem reler a trilha. O NÃO volta a consultar. Empresa e período ficam fora do memo da rodada, embora continuem memorizados no request/colaborador. (`app/chamados/juizes.py:972`; `app/chamados/juizes.py:974`; `app/chamados/juizes.py:1045`; `app/chamados/juizes.py:1047`; `app/chamados/juizes.py:1049`; `app/chamados/juizes.py:940`; `app/chamados/juizes.py:979`; `app/chamados/juizes.py:947`)

O limite da garantia é este: o SIM também depende dos vínculos atuais célula/chamado, ausentes da chave do memo; além disso, o dicionário só é descartado ao sair do contexto. **Inferência estática:** se um toque consultado sofrer rollback enquanto o mesmo lote continuar aberto, o SIM já guardado poderá sobreviver à reversão. A sequência efetiva com consumidor no mesmo `with`, rollback ou mudança desses vínculos é **[nao verificado]**. (`app/chamados/juizes.py:1044`; `app/chamados/juizes.py:1046`; `app/chamados/juizes.py:1076`; `app/chamados/juizes.py:1081`; `app/chamados/juizes.py:999`; `app/chamados/juizes.py:1003`)

## 05/11 17:28 · sessao d5535b51ef1c · - · - · origem=Ronald (tmux)

O único `with tranca_em_lote()` produtivo encontrado está em `vigiar_fio_morto.Command.handle`; envolve cinco medições e termina antes da impressão/carimbo. O comando do consumidor abre `consumidor_exclusivo`, depois chama `consumir_eventos_celula`; não abre o lote da tranca. (`app/chamados/management/commands/vigiar_fio_morto.py:171`; `app/chamados/management/commands/vigiar_fio_morto.py:172`; `app/chamados/management/commands/vigiar_fio_morto.py:176`; `app/chamados/management/commands/consumir_eventos_celula.py:19`; `app/chamados/management/commands/consumir_eventos_celula.py:31`)

Se o consumidor fosse executado dentro de um lote já aberto, o caminho das órfãs alcançaria seu memo: consumidor → `encerrar_orfas_pela_lampada` → `veredito_orfa` → `pergunta_viva` → `_pergunta_viva_calc` → `dia_da_pergunta_trancado` → `_admin_tocou_o_dia_depois` → `_memo_da_tranca`, que devolve o dict do contexto. (`app/chamados/reconciliador.py:1329`; `app/chamados/reconciliador.py:1103`; `app/chamados/reconciliador.py:1054`; `app/chamados/juizes.py:839`; `app/chamados/juizes.py:1140`; `app/chamados/juizes.py:954`; `app/chamados/juizes.py:1043`; `app/chamados/juizes.py:972`; `app/chamados/juizes.py:974`)

O memo da rodada não guarda **NÃO**; após resultado falso, a consulta é repetida. Logo, a simples inclusão posterior de toque humano não fica escondida por um NÃO desse memo. **SIM** retorna diretamente, sem repetir a consulta. (`app/chamados/juizes.py:1045`; `app/chamados/juizes.py:1047`; `app/chamados/juizes.py:1048`; `app/chamados/juizes.py:1049`)

Há possibilidade estrutural de SIM velho se um toque visível durante uma transação aquecer o memo e essa transação for revertida antes do consumo no mesmo lote: o dict recebe `True`, e sua limpeza só acontece ao sair do `with`; não há vínculo com rollback nesse corpo. Execução produtiva dessa sequência: **[não verificado]**. (`app/chamados/juizes.py:1049`; `app/chamados/juizes.py:999`; `app/chamados/juizes.py:1003`)

A monotonicidade também depende das relações atuais: a consulta cruza células atuais do colaborador/dia e chamados atualmente ligados a essas células; tais relações não integram a chave do SIM. Uma alteração que retire esse suporte pode tornar a consulta fresca falsa enquanto o SIM permanece. Ocorrência e caminho produtivo de tal alteração: **[não verificado]**. (`app/chamados/juizes.py:1076`; `app/chamados/juizes.py:1081`; `app/chamados/juizes.py:1083`; `app/chamados/juizes.py:1044`; `app/chamados/juizes.py:1045`)

Empresa e período trancado ficam fora do memo da rodada, mas continuam memoizados no request ou colaborador. O usuário de sistema entra no memo da rodada na primeira consulta; seu helper usa `get_or_create(username='sistema')`. (`app/chamados/juizes.py:940`; `app/chamados/juizes.py:941`; `app/chamados/juizes.py:947`; `app/chamados/juizes.py:979`; `app/chamados/juizes.py:1068`; `app/chamados/juizes.py:1070`; `app/core/utils.py:18`)

O consumidor não chama diretamente `retratar_perguntas_absolvidas_por_celula`: suas duas chamadas são a retratação de chamados e o encerramento de órfãs. (`app/chamados/reconciliador.py:1326`; `app/chamados/reconciliador.py:1329`)

## 05/11 17:27 · sessao c2a2d06e45cf · - · - · origem=Ronald (tmux)

Nenhuma citação direta do anexo a `app/chamados/juizes.py` aponta para corpo alterado pelo delta. Classificação: **seguem válidas**, com estas referências novas:

| Linha do censo | Referência antiga | Referência nova |
|---|---:|---|
| §1, leitor de `TurnoMaterializado`, escolha da testemunha | 1420 | `app/chamados/juizes.py:1478` e `app/chamados/juizes.py:1479` |
| §1, leitor de `contexto_json`, `dia_do_fato` | 78 | `app/chamados/juizes.py:80` e `app/chamados/juizes.py:93` |
| §1, leitor de `ChamadoColaborador.celula` | 89 | `app/chamados/juizes.py:91` e `app/chamados/juizes.py:92` |
| §2, data/competência/vínculo, prioridade do dia | 78 | `app/chamados/juizes.py:80`, `app/chamados/juizes.py:91` e `app/chamados/juizes.py:93` |
| §3, borda de competência, fato mais recente das perguntas vivas | 1978 | `app/chamados/juizes.py:2036`, `app/chamados/juizes.py:2050` e `app/chamados/juizes.py:2054` |
| §3, célula sem ata: `trabalha=None` não significa folga | 416 e 426 | `app/chamados/juizes.py:418` e `app/chamados/juizes.py:428` |
| RED R9, indeterminado sem ata | 416 e 426 | `app/chamados/juizes.py:418` e `app/chamados/juizes.py:428` |

Nenhuma linha dos sítios irmãos cita diretamente `juizes.py`; essa ausência foi conferida no anexo fornecido, cuja localização em arquivo não foi disponibilizada. [não verificado]

Distinção útil: a regra de competência continua a mesma, mas seu caminho das perguntas vivas alcança indiretamente o trecho alterado: `_dias_das_perguntas_vivas` chama `pergunta_viva`, cujo cálculo pergunta pela tranca. Portanto, “corpo citado não mudou” não significa “nenhuma dependência mudou”. (`app/chamados/juizes.py:2031`; `app/chamados/juizes.py:1140`; `app/chamados/juizes.py:940`; `app/chamados/juizes.py:954`)

## 05/11 17:23 · sessao 4e3cf3fedfbf · C20-O169 · verificador · origem=Code

**A cura elimina o fechamento por janela no coletor do cluster. Não garante que todo fechamento desse módulo dependa da assinatura da ata:** outras portas continuam podendo encerrá-lo sem essa consulta. O corte está em `app/docs/BACKLOG.md:424-426`; a cura local está em `app/ponto/services/cluster_espurio.py:68-77`; a liberação do cluster no gate genérico está em `app/chamados/models.py:680-691`.

A comparação abaixo usa o diff fornecido e a cópia aplicada. Execução da suíte Django, contagem medida de queries, estado de produção e ordem efetiva de implantação: **[nao verificado]**. [nao verificado]

**1. Fechamentos que ainda existem**

No caminho local, chamado sem dia forte é conservado; sem célula ou sem os dois insumos como listas produz fato `None`; assinatura produz `True`; medição sem assinatura ou resposta positiva de `dia_sem_trabalho` produz `False`. O catálogo conserva `None`. Portanto, nesse caminho e sem concorrência, a idade deixou de decidir. `app/ponto/services/cluster_espurio.py:25-37`, `app/ponto/services/cluster_espurio.py:68-77`, `app/chamados/catalogo/premissa.py:127-134`.

A proteção não está na porta de fechamento: o cluster normalmente emitido tem categoria `batida_espuria`, não pertence ao universo de furo e passa diretamente pela guarda de premissa. As guardas de perguntas/disputas continuam podendo impedir um fechamento, mas não perguntam pela assinatura específica do cluster. `app/ponto/services/cluster_espurio.py:135`, `app/chamados/models.py:1194-1200`, `app/chamados/catalogo/modulos.py:235-241`, `app/chamados/models.py:648-655`, `app/chamados/models.py:690-691`.

| Caminho externo que pode fechar o módulo | Condição e prova |
|---|---|
| Resposta livre do gestor e resposta rápida no fio | Transicionam o chamado indicado, sem filtro de módulo; a guarda de regularização é restrita a furo. `app/chamados/services/acoes_chamado.py:313-315`; `app/chamados/services/fio.py:705-717`; `app/chamados/models.py:175-193`. |
| Ação `aprovar_excecao` sobre o PK do cluster | O roteador verifica papel; o serviço registra batida e transiciona para resolvido, sem exigir desaparecimento da assinatura. `app/chamados/views.py:425-427`; `app/chamados/services/acoes_chamado.py:29-44`, `app/chamados/services/acoes_chamado.py:70`. |
| Ação `autorizar_geofence` sobre o PK do cluster | Sem filtro de módulo, inclusive pelo fio; termina em transição para resolvido. `app/chamados/services/fio.py:529-542`; `app/chamados/services/acoes_chamado.py:176-181`, `app/chamados/services/acoes_chamado.py:222`. |
| Resolver/declarar o dia | Seleciona todos os chamados vivos pelo JSON ou FK, carimba perguntas e reconcilia os chamados. Resolver chama o fechamento antes da lavra administrativa. `app/ponto/services/veredito_celula.py:148-156`, `app/ponto/services/veredito_celula.py:174-187`, `app/ponto/services/veredito_celula.py:325-330`; `app/ponto/services/declarar_dia.py:203-207`; `app/chamados/services/validacao.py:270-271`. |
| Desligamento | Encerra os chamados da pessoa sem filtro de módulo; a porta verifica estado/pergunta viva, sem assinatura de cluster. `app/colaboradores/services/desligamento.py:139-155`; `app/chamados/models.py:1524-1538`. Também é alcançado pelo desligamento agendado: `app/config/crons.py:440`. |
| Competência trancada | Seleciona vivos da competência; cluster não está nas exceções. Encerra pela via própria sem consultar sua assinatura. `app/chamados/services/competencia_trancada.py:88-103`, `app/chamados/services/competencia_trancada.py:135-146`, `app/chamados/services/competencia_trancada.py:181-194`; `app/chamados/catalogo/modulos.py:268-272`; `app/chamados/models.py:1547-1558`. Entradas: `app/ponto/services/fechamento.py:1410`; `app/chamados/management/commands/encerrar_por_competencia_trancada.py:29`. |
| `retratar_chamados_lei_cala --modulo cluster_batida_espuria --apply` | `--modulo` é livre; exige dia passado e ausência de falta pelo outro classificador, depois chama `retratar`. `app/chamados/management/commands/retratar_chamados_lei_cala.py:24`, `app/chamados/management/commands/retratar_chamados_lei_cala.py:33-47`, `app/chamados/management/commands/retratar_chamados_lei_cala.py:59`; `app/chamados/reconciliador.py:518-524`. |
| `marcar_feriado_do_posto --apply` | Universo da fila sem filtro de módulo; recorta vínculo/posto/feriado e reconcilia. Não exige ata sem assinatura. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:102-115`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:138-145`. |

Também há caminhos condicionados a vínculos ou valores adicionais; a existência desses casos em produção é **[nao verificado]**:

| Vínculo/valor adicional | Caminho e prova |
|---|---|
| Cluster como pai de disputa | Qualquer fechamento da disputa pode acionar Defesa 1 e reconciliar o pai, sem filtro de módulo. Inclui remoção do último motivo. `app/chamados/signals.py:18-45`; `app/chamados/models.py:1965-1975`. |
| Cluster como destino ou relacionado de pergunta | Materialização reconcilia destino e clones; ausência já decidida também reconcilia o destino. `app/chamados/services/materializacao.py:728-748`; `app/chamados/services/materializacao.py:559-561`. |
| Justificativa aprovada com cluster em `chamado_origem` | Busca por PK sem filtro de módulo e chama `retratar`. `app/ponto/services/justificativa.py:33-43`. |
| Cluster com batida indicada no contexto | `recusar_geofence` verifica pertencimento da batida, retrata e transiciona, sem exigir fim da assinatura. `app/chamados/services/acoes_chamado.py:257-277`. |
| Categoria legada `disputa_supervisao` | `auditar_chamados_orfaos --fechar` filtra categoria, não módulo. `app/chamados/management/commands/auditar_chamados_orfaos.py:34-43`, `app/chamados/management/commands/auditar_chamados_orfaos.py:67`. |
| Categoria legada `escala_aceite` ou `agenda_aceite` | Troca/aceite fecha por categoria. `app/colaboradores/services/vinculo.py:384-387`; `app/chamados/services/acoes_chamado.py:88-108`, `app/chamados/services/acoes_chamado.py:130-149`. |
| Subcategoria legada `vinculo_divergente` | Comando da lista seleciona subcategoria sem filtro de módulo e cancela. `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:42-57`. |
| Regularização externa apontando para cluster | Materializador chama `encerrar`; o escritor normal, porém, cria outro módulo. Relação legada/corrompida com cluster: **[nao verificado]**. `app/chamados/services/regularizacao_ext.py:201-205`, `app/chamados/services/regularizacao_ext.py:246-257`. |

Há ainda uma corrida no próprio caminho corrigido: coleta “sem assinatura”, outra transação grava assinatura e depois o reconciliador consome o candidato antigo. A leitura da célula antecede o atomic do gate; este trava o chamado e não revalida a assinatura do cluster. Ocorrência concreta: **[nao verificado]**. `app/ponto/services/cluster_espurio.py:71-76`, `app/ponto/services/cluster_espurio.py:121-124`; `app/chamados/models.py:616-620`, `app/chamados/models.py:668-691`.

Os caminhos examinados abaixo **não são fechadores adicionais do cluster normal**:

- Reconciliador genérico, signal da batida e cron: não coletam `FATO_CLUSTER_ESPURIO`; o catálogo conserva o fato ausente. `app/chamados/reconciliador.py:262-301`; `app/chamados/catalogo/premissa.py:127-130`; `app/ponto/signals.py:214-215`; `app/config/crons.py:395`.
- Absolvição pelo outbox e laços de cobrança do cartório restringem módulos de turno/furo; cluster fica fora. `app/chamados/reconciliador.py:334-337`, `app/chamados/reconciliador.py:964-966`; `app/ponto/services/cartorio.py:112-117`; `app/chamados/catalogo/furo_modulos.py:15-16`.
- Cobertura passa fatos de falta/turno/intervalo, não o fato de cluster; lastro restringe `batida_ausente`; silêncio de ausência restringe seu próprio universo. `app/ponto/services/cobertura.py:117-124`, `app/ponto/services/cobertura.py:310-317`; `app/ponto/services/lastro.py:239`; `app/chamados/services/silencio_ausencia.py:21`, `app/chamados/services/silencio_ausencia.py:60`.
- O cron do cluster é `--alarme`: não fecha. O `--apply` manual usa o coletor corrigido. `app/config/crons.py:598`; `app/ponto/management/commands/detectar_cluster_espurio.py:34-61`.

**2. Mudanças de comportamento**

Para atas produzidas normalmente pelo cartório, os insumos são listas; nessa população a abertura mantém o predicado anterior, e o fechamento passa a conservar os casos não medidos e os assinados fora da janela. `app/ponto/services/cartorio.py:701-711`; `app/ponto/services/cluster_espurio.py:29-37`, `app/ponto/services/cluster_espurio.py:68-77`.

A comparação completa por classe de entrada é esta; “ausente” significa que o dia não entra na saída de `dias_com_cluster`. O comportamento anterior é o corpo removido no diff fornecido; as referências apontam as operações correspondentes na cópia aplicada. `app/ponto/services/cluster_espurio.py:25-49`.

| Forma/condição da ata | Antes → depois em `dias_com_cluster` |
|---|---|
| `None`, `{}` ou outro valor JSON falso no topo | Ausente → ausente; internamente o novo leitor retorna `None`. `app/ponto/services/cluster_espurio.py:27-30`. |
| `orfas` ausente/`None`, ou `lampadas` ausente/`None` | Ausente → ausente; agora é explicitamente não medido. `app/ponto/services/cluster_espurio.py:29-30`. |
| Ambas listas vazias | Ausente → ausente; agora é medido sem assinatura, `()`. `app/ponto/services/cluster_espurio.py:29-37`. |
| `orfas` lista com não strings ou strings menores que cinco caracteres | Esses elementos são ignorados nos dois corpos; as strings restantes decidem. `app/ponto/services/cluster_espurio.py:31-36`. |
| `lampadas` lista com não dicionários | Ignorados nos dois corpos; só dicionário com `acesa is False` conta. Ausente/`None`/`0` em `acesa` não conta como `False`. `app/ponto/services/cluster_espurio.py:32`. |
| Duas órfãs válidas próximas e apagada | Mesmo `(data, orfas, n_missing)` nos dois corpos. `app/ponto/services/cluster_espurio.py:31-36`, `app/ponto/services/cluster_espurio.py:49`. |
| Órfãs sem par próximo, menos de duas órfãs ou nenhuma apagada | Ausente → ausente. `app/ponto/services/cluster_espurio.py:33-37`. |
| `orfas` **dicionário**, com chaves `18:56` e `18:58`, e lâmpada apagada em lista | **Assinatura antes → ausente depois**: antes iterava as chaves; agora a exigência de lista recusa. `app/ponto/services/cluster_espurio.py:29-31`. |
| Insumo não lista, mas iterável | Antes podia ser percorrido; agora é não medido. Para JSON, strings/dicionários em `lampadas` não forneciam dicionários de lâmpadas. `app/ponto/services/cluster_espurio.py:29-32`. |
| Insumo numérico/booleano verdadeiro, não iterável | Antes podia levantar `TypeError`; agora a validação retorna `None`. `app/ponto/services/cluster_espurio.py:29-32`. |
| Ata verdadeira no topo que não seja dicionário | Continua podendo levantar `AttributeError` no juiz, antes de chegar à validação nova — inclusive com `trabalha=False`. `app/chamados/juizes.py:408`, `app/chamados/juizes.py:420-426`. |
| Strings de hora inválidas, com tamanho ≥5 | Continua podendo levantar `ValueError` se alcançar a conversão com duas órfãs e apagada; não há validação de faixa/formato. `app/ponto/services/cluster_espurio.py:31-35`. |
| `dia_sem_trabalho` responde verdadeiro | Ausente nos dois corpos; agora o leitor retorna `()`, permitindo fecho com esse lastro. `app/ponto/services/cluster_espurio.py:25-26`. |
| Ausência parcial com `trabalha=True` ou `None` | O juiz conserva cobrança do resto nos dois corpos; assinatura depende das listas. `app/chamados/juizes.py:408`, `app/chamados/juizes.py:424-426`. |
| Parcial sem período; ou com período e sem fato novo | Assinatura pode continuar em `achados`; `na_lista` impede abertura, sem matar chamado assinado já vivo. `app/ponto/services/cluster_espurio.py:91-108`; `app/ponto/services/cartorio.py:395-400`. |

**Fechamentos novos:** com entradas regulares, estado estável e execução anterior concluindo, não há ampliação: dentro da janela o falso medido já fechava; fora dela o código anterior já tratava todo dia como falso. A mudança retira fechamentos por ausência de medição/recorte. `app/ponto/services/cluster_espurio.py:54-59`, `app/ponto/services/cluster_espurio.py:74-77`.

Há, entretanto, diferença quando antes ocorria exceção: um insumo não iterável na janela podia abortar `pendencias` antes de qualquer ato; agora pode ser ignorado como não medido, permitindo fechar **outro** chamado medido sem assinatura ou abrir outro achado. No sentido inverso, agora uma ata antiga malformada, fora da janela e pertencente a chamado vivo, pode abortar a coleta que antes não a examinava. `app/ponto/services/cluster_espurio.py:29-35`, `app/ponto/services/cluster_espurio.py:71-76`, `app/ponto/services/cluster_espurio.py:100-108`, `app/ponto/services/cluster_espurio.py:121-130`.

**Abertura/alarme:** a recusa nova de `orfas` como dicionário pode retirar o dia de `achados`, `a_abrir`, `na_lista` e das tentativas reais de abertura. O alarme lê `a_abrir` e `a_fechar`; portanto também muda. A abertura real percorre `achados`, não `a_abrir`, e ainda chama `abrir` para dias com chamado existente. `app/ponto/services/cluster_espurio.py:100-108`, `app/ponto/services/cluster_espurio.py:127-130`; `app/ponto/management/commands/detectar_cluster_espurio.py:51-64`.

**3. Quem escreve e quem lê**

| Campo/valor | Escritores | Leitores relevantes |
|---|---|---|
| `ata.tipo_dia`, `ata.orfas`, `ata.lampadas` | Cartório fabrica tipo, lista de horas e lâmpadas; chama a lavra. A porta atribui `ata`. `app/ponto/services/cartorio.py:701-711`, `app/ponto/services/cartorio.py:1800-1802`; `app/ponto/portas/celula.py:738-749`, `app/ponto/portas/celula.py:775-776`. | Predicado do cluster; juiz sem trabalho; coletor de furo; transporte da grade; lastro. `app/ponto/services/cluster_espurio.py:25-35`; `app/chamados/juizes.py:420-426`, `app/chamados/juizes.py:463-488`; `app/escala/services/leitor_celula.py:374-405`; `app/ponto/services/lastro.py:89`, `app/ponto/services/lastro.py:113`. |
| `lampadas[].acesa` | Construção admite `True`, `False`, `None`; teto temporal pode marcar pendente como `None`. `app/ponto/services/cartorio.py:638-644`, `app/ponto/services/cartorio.py:862-872`. | Cluster conta somente `is False`. `app/ponto/services/cluster_espurio.py:32`. |
| `cobertura_parcial`, `periodo_parcial` | Grade produz; cartório transporta para ata. `app/escala/utils.py:1241-1274`; `app/ponto/services/cartorio.py:729-737`. | Juiz sem trabalho e `parcial_so_lista`. `app/chamados/juizes.py:398-426`; `app/ponto/services/cartorio.py:395-400`. |
| `CelulaDia.trabalha` | Geração/regeneração/reversão pela porta. `app/ponto/portas/celula.py:231-265`, `app/ponto/portas/celula.py:540-547`, `app/ponto/portas/celula.py:598-623`. | Juiz sem trabalho e leitor da grade; `None` não cala sozinho. `app/chamados/juizes.py:424-426`; `app/escala/services/leitor_celula.py:374-399`. |
| `contexto_json.data_turno` | Emissor do cluster no lookup/defaults; comando de carimbo também pode preencher pela porta do contexto. `app/ponto/services/cluster_espurio.py:133`, `app/ponto/services/cluster_espurio.py:145`; `app/chamados/management/commands/carimbar_por_batida_coincidente.py:98-99`; `app/chamados/services/contexto.py:96-102`. | Dedup local e abertura; catálogo/juiz do dia; fechamento amplo do fio. `app/ponto/services/cluster_espurio.py:85`, `app/ponto/services/cluster_espurio.py:133`; `app/chamados/catalogo/modulos.py:115`, `app/chamados/catalogo/modulos.py:219-225`; `app/chamados/juizes.py:91-112`; `app/ponto/services/veredito_celula.py:148-150`. |
| `ChamadoColaborador.celula_id` | É **FK própria**, não chave do JSON. Nasce resolvida por `abrir`/`criar`; posteriormente `apontar_celula` preserva o primeiro vínculo. `app/chamados/models.py:41-46`, `app/chamados/models.py:914-919`, `app/chamados/models.py:1026-1028`; `app/chamados/services/vinculos.py:92-99`. | Juiz prioriza a FK; fechamento amplo também usa a relação. `app/chamados/juizes.py:89-90`; `app/ponto/services/veredito_celula.py:154-156`. |
| `status_local` | Nascimento pelo motor/defaults; transições por `_gravar_estado`, com trilha e save no atomic. `app/chamados/models.py:894-901`, `app/chamados/models.py:932`, `app/chamados/models.py:222`, `app/chamados/models.py:274-279`. | Seleção de vivos; dedup; decisão do gate; signals de encerramento. `app/ponto/services/cluster_espurio.py:66-68`; `app/chamados/models.py:882-885`, `app/chamados/models.py:638-651`; `app/chamados/signals.py:70-81`. |

A FK pode mandar no dia usado pelo fecho enquanto o JSON continua mandando no dedup/abertura; divergência concreta entre os dois em produção é **[nao verificado]**. `app/chamados/juizes.py:89-112`; `app/ponto/services/cluster_espurio.py:68`, `app/ponto/services/cluster_espurio.py:85`, `app/ponto/services/cluster_espurio.py:133`.

Os contratos dos retornos e seus consumidores são:

- `_leitura_da_celula`: `None`, `()` ou `(orfas, n_missing)`; abertura usa sua verdade, fecho preserva a distinção `None`/falso. `app/ponto/services/cluster_espurio.py:15-37`, `app/ponto/services/cluster_espurio.py:47-49`, `app/ponto/services/cluster_espurio.py:74-76`.
- `dias_com_cluster`: lista de triplas, consumida por `pendencias`, reexportada pelo comando e diretamente testada. `app/ponto/services/cluster_espurio.py:49-50`, `app/ponto/services/cluster_espurio.py:100`; `app/ponto/management/commands/detectar_cluster_espurio.py:19`; `app/ponto/tests/test_detectar_cluster_espurio.py:56`.
- `_vivos_que_a_premissa_matou`: lista de chamados, entregue a `pendencias['a_fechar']`; o ato chama `reconciliar`, enquanto o vigia apenas registra pendência. `app/ponto/services/cluster_espurio.py:77-78`, `app/ponto/services/cluster_espurio.py:108`, `app/ponto/services/cluster_espurio.py:123-125`; `app/ponto/management/commands/detectar_cluster_espurio.py:55-64`.
- `na_lista` controla abertura real; `a_abrir` controla a pendência do vigia; `achados` controla exibição e tentativas reais de abertura. `app/ponto/services/cluster_espurio.py:107`, `app/ponto/services/cluster_espurio.py:127-130`; `app/ponto/management/commands/detectar_cluster_espurio.py:51-54`.

**4. Checklist**

Aqui, SIM significa que a classe de problema está presente; quando preexistente, isso está explicitado. `app/ponto/services/cluster_espurio.py:15-108`.

| Classe | Resultado e prova |
|---|---|
| (a) Segundo juiz duplicando autoridade registrada | **NÃO introduzido.** Reusa os juízes de dia, premissa e sem trabalho; a assinatura foi extraída para um corpo compartilhado. Cluster continua declarado em `SEM_JUIZ`, sem alteração de registro. `app/ponto/services/cluster_espurio.py:21-25`, `app/ponto/services/cluster_espurio.py:60-76`; `app/core/juizes.py:261-278`, `app/core/juizes.py:1184-1186`. |
| (b) Segundo escritor fora da porta | **NÃO produtivo no diff.** A coleta só lê e devolve candidatos. A fixture usa `update(ata,trabalha)`, mas o selo exclui testes. `app/ponto/services/cluster_espurio.py:65-78`; `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:54`; `app/ponto/tests/test_selo_veredito_so_pela_porta.py:1-4`. |
| (c) Relógio fora da borda | **SIM, preexistente no fluxo.** Fora do julgamento, `_agora=None`; parcial com período/julgamento anterior pode alcançar `houve_fato_novo`, que lê `now()`. O diff não acrescenta essa leitura. `app/ponto/services/cluster_espurio.py:101-104`; `app/ponto/services/cartorio.py:82-87`, `app/ponto/services/cartorio.py:345`, `app/ponto/services/cartorio.py:400`. |
| (d) Derivação paralela da assinatura exata | **NÃO encontrada no caminho produtivo.** Abertura e fecho chamam o mesmo corpo. O coletor de furo responde outra pergunta, missing **ou** órfã. `app/ponto/services/cluster_espurio.py:31-36`, `app/ponto/services/cluster_espurio.py:47`, `app/ponto/services/cluster_espurio.py:74`; `app/chamados/juizes.py:475-488`. |
| (e) RED sem mordida ou vazio | **NÃO.** Exige criação e estado vivo antes do ato; 15 dias assinado reprova a volt

[truncado em 20000 de 24903 caracteres]

## 05/11 17:20 · sessao 85532525cb37 · - · - · origem=Ronald (tmux)

Censo consolidado, só estático; execução e ocorrência desses vínculos em produção [nao verificado].

O fechamento do cluster **não está protegido na porta**: `reconciliar()` pergunta `_gate_premissa_do_fechamento`, mas esse gate libera imediatamente chamado que não é de furo; cluster não pertence a `CALADOS_POR_ISENCAO`, e o emissor usa categoria `batida_espuria`. Logo os caminhos abaixo podem fechar sem perguntar a assinatura. `app/chamados/models.py:648`, `app/chamados/models.py:690`, `app/chamados/models.py:1194`, `app/chamados/catalogo/modulos.py:235`, `app/ponto/services/cluster_espurio.py:133`.

**Caminhos que alcançam o módulo normal**

| Caminho | Prova |
|---|---|
| Resposta livre do gestor → `transicionar(resolvido/fechado)` | `app/chamados/services/acoes_chamado.py:313`; roteador `app/chamados/views.py:547`; guarda S122 só para furo `app/chamados/models.py:175`; escrita `app/chamados/models.py:193`. |
| Resposta rápida no fio → transição terminal | Busca qualquer chamado da pessoa e verifica RBAC, sem filtro módulo: `app/chamados/services/fio.py:707`, `app/chamados/services/fio.py:715`. |
| POST `aprovar_excecao` sobre chamado cluster | Roteador guarda papel, não categoria/módulo: `app/chamados/views.py:425`; service guarda somente estado: `app/chamados/services/acoes_chamado.py:21`; registra batida e transiciona: `app/chamados/services/acoes_chamado.py:44`, `app/chamados/services/acoes_chamado.py:70`. Persistir assinatura depois da batida é possível pelo código; exemplo executado [nao verificado]. |
| POST `autorizar_geofence` sem advertência | Sem guarda categoria/módulo no roteador ou service: `app/chamados/views.py:540`, `app/chamados/services/acoes_chamado.py:176`; transição direta `app/chamados/services/acoes_chamado.py:222`. Também acessível pelo fio com PK da pessoa e RBAC: `app/chamados/services/fio.py:529`, `app/chamados/services/fio.py:533`, `app/chamados/services/fio.py:542`. |
| Resolver dia / declarar dia → fechar todo fio do dia | Universo inclui qualquer módulo pelo JSON ou FK: `app/ponto/services/veredito_celula.py:148`, `app/ponto/services/veredito_celula.py:155`; carimba perguntas e reconcilia todos: `app/ponto/services/veredito_celula.py:174`, `app/ponto/services/veredito_celula.py:182`. Chamada pelo Resolver dia antes de lavrar veredito admin: `app/ponto/services/veredito_celula.py:325`, `app/ponto/services/veredito_celula.py:330`; declaração de folga: `app/ponto/services/declarar_dia.py:203`; validação de folga já declarada: `app/chamados/services/validacao.py:270`. |
| Desligamento → encerrar todos chamados da pessoa | Disputas primeiro: `app/colaboradores/services/desligamento.py:139`; universo sem módulo e `encerrar()`: `app/colaboradores/services/desligamento.py:150`, `app/colaboradores/services/desligamento.py:155`. Porta não pergunta premissa: `app/chamados/models.py:1524`, `app/chamados/models.py:1536`. |
| Competência trancada → encerramento | Universo todos VIVOS da empresa: `app/chamados/services/competencia_trancada.py:88`; exclusões não excluem cluster: `app/chamados/services/competencia_trancada.py:100`, `app/chamados/catalogo/modulos.py:268`; sem pergunta viva entra: `app/chamados/services/competencia_trancada.py:139`; executa porta: `app/chamados/services/competencia_trancada.py:181`; porta só motor e pergunta viva: `app/chamados/models.py:1547`. Dispara no ato de trancar `app/ponto/services/fechamento.py:1410`; comando manual `app/chamados/management/commands/encerrar_por_competencia_trancada.py:29`. |
| `retratar_chamados_lei_cala --modulo cluster_batida_espuria --apply` | `--modulo` livre `app/chamados/management/commands/retratar_chamados_lei_cala.py:24`; filtro `:33`; exige dia passado e `classificar_falta(...)[1] is None`, sem assinatura `:40`; chama `retratar` `:59` → `app/chamados/reconciliador.py:518`, `app/chamados/reconciliador.py:523`. |
| `marcar_feriado_do_posto --apply` | Candidatos de `qs_na_fila` sem módulo `app/colaboradores/management/commands/marcar_feriado_do_posto.py:102`; recorta vínculo do dia/posto `:105`; feriado `:111`; reconcilia `:138`. Não lê ata nem `dia_sem_trabalho`; fila é vivo menos silêncio/registrado, não lista restrita de módulos: `app/chamados/services/painel.py:158`, `app/chamados/services/painel.py:174`. |

**Caminhos condicionados a relações ou valores adicionais**

- **Disputa com cluster como pai**: qualquer fechamento de disputa converge no receiver sem filtro de módulo → `ch.reconciliar()`. `app/chamados/signals.py:18`, `app/chamados/signals.py:28`, `app/chamados/signals.py:42`. Isso cobre validação/materialização de perguntas, fechos automáticos, exceções admin, tranca, desligamento e remoção do último motivo. Remover motivo apaga perguntas e, se não sobra motivo, fecha disputa: `app/chamados/models.py:1965`, `app/chamados/models.py:1969`, `app/chamados/models.py:1975`.
- **Cluster como destino ou clone de pergunta materializada**: não há filtro módulo; aplica retenção de irmãs e chama `reconciliar`. `app/chamados/services/materializacao.py:728`, `app/chamados/services/materializacao.py:738`, `app/chamados/services/materializacao.py:740`, `app/chamados/services/materializacao.py:744`. Chamadores hora/data_hora/escolha: `app/chamados/services/materializacao.py:941`, `app/chamados/services/materializacao.py:1026`, `app/chamados/services/materializacao.py:1046`.
- **Ausência decidida declarada por pergunta cujo destino é cluster**: `app/chamados/services/materializacao.py:559`, `app/chamados/services/materializacao.py:561`.
- **Justificativa aprovada cujo chamado_origem é cluster**: busca PK sem módulo e retrata: `app/ponto/services/justificativa.py:33`, `app/ponto/services/justificativa.py:38`, `app/ponto/services/justificativa.py:42`.
- **`recusar_geofence` em cluster com batida indicada no contexto**: exige batida pertencente ao chamado, não módulo; retrata e transiciona: `app/chamados/services/acoes_chamado.py:257`, `app/chamados/services/acoes_chamado.py:262`, `app/chamados/services/acoes_chamado.py:269`, `app/chamados/services/acoes_chamado.py:277`.
- **Categoria legada `disputa_supervisao`**: `auditar_chamados_orfaos --fechar` seleciona pela categoria, sem módulo, e reconcilia se não existe disputa aberta. `app/chamados/management/commands/auditar_chamados_orfaos.py:34`, `app/chamados/management/commands/auditar_chamados_orfaos.py:38`, `app/chamados/management/commands/auditar_chamados_orfaos.py:67`.
- **Categoria legada `escala_aceite`**: troca de vínculo encerra pela categoria, sem módulo: `app/colaboradores/services/vinculo.py:384`, `app/colaboradores/services/vinculo.py:387`; resposta do colaborador também transiciona por categoria `app/chamados/services/acoes_chamado.py:130`, `app/chamados/services/acoes_chamado.py:149`. Agenda idem se categoria `agenda_aceite`: `app/chamados/services/acoes_chamado.py:88`, `app/chamados/services/acoes_chamado.py:108`.
- **Subcategoria legada `vinculo_divergente`**: `fechar_chamados_vinculo_na_lista --apply` filtra subcategoria, sem módulo; se pessoa está em C8 cancela. `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:42`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:44`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:57`.
- **Regularização externa com FK legado/corrompido para cluster**: materializador chama `reg.chamado.encerrar()` sem módulo `app/chamados/services/regularizacao_ext.py:201`. Porém escritor normal cria módulo `regularizacao_externa` e aponta esse chamado: `app/chamados/services/regularizacao_ext.py:246`, `app/chamados/services/regularizacao_ext.py:250`, `app/chamados/services/regularizacao_ext.py:256`; relação com cluster [nao verificado].

**Caminhos examinados que não fecham o cluster normal**

- Reconciliador genérico, tanto signal da batida quanto cron: `_veredito_premissa` não coleta `FATO_CLUSTER_ESPURIO`; fica fatos `{}` e catálogo conserva. `app/chamados/reconciliador.py:262`, `app/chamados/reconciliador.py:294`, `app/chamados/reconciliador.py:301`, `app/chamados/catalogo/premissa.py:127`. Chamadores `app/ponto/signals.py:214`, `app/chamados/management/commands/reconciliar_chamados.py:52`; cron efetivo `app/config/crons.py:395`.
- Outbox/absolvição pela célula restringe `MODULOS_DE_TURNO`, cluster fora. `app/chamados/reconciliador.py:334`, `app/chamados/reconciliador.py:964`.
- Loops amplos do cartório usam `chamados_que_falam_de_batida`, que filtra `FURO_MODULES`; lista exclui cluster. `app/ponto/services/cartorio.py:1269`, `app/ponto/services/cartorio.py:117`, `app/chamados/catalogo/furo_modulos.py:15`. Portanto fechamentos `cartorio.py:1488`, `:1501`, `:1522`, `:1600` não são bypass para módulo cluster.
- Lastro só `batida_ausente`: `app/ponto/services/lastro.py:239`.
- Ausência/silêncio só CALADOS e justificativa: `app/chamados/services/silencio_ausencia.py:21`, `app/chamados/services/silencio_ausencia.py:60`.
- Cobertura informa fatos de falta/turno/intervalo e pergunta catálogo, não cluster: `app/ponto/services/cobertura.py:117`, `app/ponto/services/cobertura.py:310`.
- Feriado reavaliar só `batida_ausente`, distinto do comando amplo marcar_feriado_do_posto: `app/chamados/services/feriado_reavaliar.py:17`.
- Fantasmas só turno_aberto/entrada_bloqueada: `app/ponto/management/commands/reconciliar_fantasmas.py:45`, `:156`.
- Meio período só intervalo_pendente/batida_fora_escala_leve: `app/ponto/management/commands/fechar_chamados_meio_periodo.py:30`, `:56`.
- Cancelamento de régua só módulo regua_suspeita: `app/escala/management/commands/fechar_avisos_regua_na_lista.py:34`.
- Rejunção só muda FK, não status: `app/chamados/rejuncao.py:59`, `app/chamados/rejuncao.py:77`.
- Apagador de chamados é porta explicitamente limitada a ensaio, chamada pelo zerador: `app/chamados/models.py:531`, `app/chamados/models.py:537`, `app/chamados/models.py:539`, `app/ponto/management/commands/zerar_dados_operacionais.py:84`. Não é fecho de produção.
- Cron cluster é **vigia**, não executor: `app/config/crons.py:598`; `--alarme` força `aplicar=False` e só mede/alarma: `app/ponto/management/commands/detectar_cluster_espurio.py:34`, `:50`, `:61`. `--apply` manual chama reconciliar_colab: `:45`.
- Evento do cartório chama coletor próprio depois da lavra: `app/ponto/services/cartorio.py:987`, `app/ponto/services/cartorio.py:993`.

Não localizei símbolo `reavaliar_colab_pendencias` na cópia; sua existência [nao verificado].

## 05/11 17:19 · sessao 1eb749f281ac · - · - · origem=Ronald (tmux)

Mapeamento verificado por leitura estática; execução de testes/produção [nao verificado]. [nao verificado]

**Escritores e leitores da ata**

- `CelulaDia.ata` é JSONField anulável; a documentação indica lavra como escritor e folha como leitora (`app/escala/models.py:1774-1778`).
- Produtor de `tipo_dia`: copia o dia da grade; `lampadas`: `lampadas_do_dia`; `orfas`: transforma cada elemento em `str` (`app/ponto/services/cartorio.py:701-711`). Cobertura e período parciais são transportados em `app/ponto/services/cartorio.py:729-737`; sua origem está em `app/escala/utils.py:1241-1274`.
- `lampadas_do_dia` produz `acesa=True`, `None` ou `False` (`app/ponto/services/cartorio.py:638-644`); o teto converte apagadas ainda não vencidas para `acesa=None, pendente=True` (`app/ponto/services/cartorio.py:862-872`).
- O cartório chama a lavra com a ata produzida e, em seguida, carimba a célula (`app/ponto/services/cartorio.py:1800-1802`, `app/ponto/services/cartorio.py:1821-1823`). `CelulaDia.lavrar_veredito` delega para a porta (`app/escala/models.py:1819-1828`); atribuição efetiva em `app/ponto/portas/celula.py:738-749` e `app/ponto/portas/celula.py:775-783`.
- `carimbar_precedencia` também regrava `ata`, preservando as demais chaves e alterando apenas `precedencia` (`app/ponto/portas/celula.py:819-830`).
- Há cópias de ata **somente em memória** na própria porta (`app/ponto/portas/celula.py:675-690`) e na montagem da grade (`app/escala/utils.py:1907-1919`); esta última fabrica somente `lampadas/orfas`, destinada ao juiz de turnos (`app/escala/utils.py:1909-1919`).
- Leitores diretamente relevantes: novo predicado (`app/ponto/services/cluster_espurio.py:25-35`); juiz `dia_sem_trabalho`, incluindo parcial (`app/chamados/juizes.py:398-426`); coletor genérico `_celula_furada` (`app/chamados/juizes.py:463-488`); `parcial_so_lista` (`app/ponto/services/cartorio.py:395-400`); leitor de grade, que transporta tipo/orfas/insumo do juiz/parcial (`app/escala/services/leitor_celula.py:374-405`); lastro lê lâmpadas em `app/ponto/services/lastro.py:89`, `app/ponto/services/lastro.py:113`, `app/ponto/services/lastro.py:128`, `app/ponto/services/lastro.py:149`.

**`CelulaDia.trabalha`**

- Campo triestado, com `None=indeterminado (limbo cobra)` (`app/escala/models.py:1686-1688`).
- Escritores da porta: regeneração fora do vínculo põe `False` (`app/ponto/portas/celula.py:225-233`); criação/regeneração calculam pelo vínculo (`app/ponto/portas/celula.py:243-265`); reversão deriva do DNA anterior (`app/ponto/portas/celula.py:535-547`); geração cria/altera hoje e futuro (`app/ponto/portas/celula.py:598-623`).
- O diff lê indiretamente pelo juiz: `trabalha is False` cala inclusive parcial; `None` não cala sozinho (`app/chamados/juizes.py:420-426`). O leitor da grade também usa o campo ao derivar tipo ausente e transportar o insumo (`app/escala/services/leitor_celula.py:374-399`).

**Contexto/data/célula**

- `celula_id` é a FK própria do chamado, **não uma chave de `contexto_json`** (`app/chamados/models.py:41-46`).
- O emissor cluster escreve `contexto_json.data_turno` tanto no lookup quanto nos defaults (`app/ponto/services/cluster_espurio.py:133`, `app/ponto/services/cluster_espurio.py:145`).
- Nascimento resolve FK em `abrir` (`app/chamados/models.py:914-919`) ou `criar` (`app/chamados/models.py:1026-1028`), por `_celula_de`, que pergunta ao juiz e busca a célula (`app/chamados/models.py:1184-1190`).
- Fora do nascimento, `apontar_celula` grava FK, com primeiro apontamento prevalecendo (`app/chamados/services/vinculos.py:92-99`). Chamadores confirmados: rejunção (`app/chamados/rejuncao.py:68-80`), backfill (`app/chamados/services/celula_carimbada.py:96-109`) e comando por batida coincidente (`app/chamados/management/commands/carimbar_por_batida_coincidente.py:97-100`).
- O último comando também escreve `data_turno`, pela porta do contexto (`app/chamados/management/commands/carimbar_por_batida_coincidente.py:98-99`); a atribuição/salvamento da porta ocorre em `app/chamados/services/contexto.py:96-102`.
- `data_do_chamado` lê chave declarada (`app/chamados/catalogo/modulos.py:219-225`); para cluster a chave é `data_turno` (`app/chamados/catalogo/modulos.py:115-116`).
- `dia_do_fato` prioriza **FK da célula**, depois chave do catálogo e contexto; sem data forte retorna `None` (`app/chamados/juizes.py:89-119`). Logo FK divergente do JSON manda no fecho (`app/ponto/services/cluster_espurio.py:68`), enquanto dedup/abertura continuam usando JSON (`app/ponto/services/cluster_espurio.py:85`, `app/ponto/services/cluster_espurio.py:133`).

**Status e retornos**

- `status_local` nasce pelo motor/defaults e `get_or_create` (`app/chamados/models.py:894-901`, `app/chamados/models.py:932`); a transição central atribui em `app/chamados/models.py:222`, salva no atomic em `app/chamados/models.py:274-279`.
- Leituras relevantes: seleção de vivos do diff (`app/ponto/services/cluster_espurio.py:66-68`), dedup de abertura (`app/chamados/models.py:882-885`) e decisão do fechamento (`app/chamados/models.py:638-651`).
- `dias_com_cluster` é consumido em `pendencias` (`app/ponto/services/cluster_espurio.py:100`), e reexportado pelo comando (`app/ponto/management/commands/detectar_cluster_espurio.py:19`).
- `_vivos_que_a_premissa_matou` alimenta exclusivamente `pendencias['a_fechar']` no código produtivo encontrado (`app/ponto/services/cluster_espurio.py:108`).
- `a_fechar`: reconciliador percorre e chama gate (`app/ponto/services/cluster_espurio.py:123-125`); vigia coleta em dry e dispara alarme (`app/ponto/management/commands/detectar_cluster_espurio.py:55-64`).
- `achados`: exibição dry (`app/ponto/management/commands/detectar_cluster_espurio.py:51-52`) e abertura real (`app/ponto/services/cluster_espurio.py:127-147`); `a_abrir` só alimenta pendência/alarme (`app/ponto/management/commands/detectar_cluster_espurio.py:53-54`). Portanto abertura real percorre `achados`, inclusive pedidos de renascimento via `abrir`, não `a_abrir` (`app/ponto/services/cluster_espurio.py:127-130`, `app/chamados/models.py:882-885`).

**Checklist da minha fatia**

- Segundo juiz duplicando registrado: **NÃO no diff**: usa dia/premissa/vivos existentes (`app/ponto/services/cluster_espurio.py:60-76`), registrados em `app/core/juizes.py:261-278`. Cluster é declarado `SEM_JUIZ` em `app/core/juizes.py:1184-1186`.
- Derivação paralela da assinatura exata: **NÃO encontrada**; abertura e fecho chamam a mesma função (`app/ponto/services/cluster_espurio.py:47`, `app/ponto/services/cluster_espurio.py:74`). `_celula_furada` responde missing **OU** órfã, não rajada **E** apagada (`app/chamados/juizes.py:475-488`). O cluster guard citado em `SEM_JUIZ` trata atribuição a marcos, pergunta distinta (`app/core/juizes.py:1185-1186`).
- Segundo escritor produtivo introduzido: **NÃO**; novo código coleta e devolve chamados (`app/ponto/services/cluster_espurio.py:65-78`). Fixture escreve `ata/trabalha` fora da porta (`app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:53`), mas o selo explicitamente exclui `tests/` e `migrations/` (`app/ponto/tests/test_selo_veredito_so_pela_porta.py:1-4`).
- Pouso da chamada cluster no evento já acontece depois da lavra (`app/ponto/services/cartorio.py:987-994`); o helper não introduz leitura do relógio (`app/ponto/services/cluster_espurio.py:15-37`). O relógio indireto preexistente vem de `agora_deste_julgamento`, simples acesso a thread local (`app/ponto/services/cartorio.py:82-87`).

**Limites de forma da ata**

- JSONField aceita formas não-dict no nível Python (`app/escala/models.py:1778`); ambos os corpos usam `.get` sem validar mapping, portanto ata truthy não-mapping levanta, salvo saída antecipada por `trabalha=False` no juiz (`app/chamados/juizes.py:420-426`, `app/ponto/services/cluster_espurio.py:27-29`).
- Mesmo após filtro `str/len>=5`, horas inválidas podem levantar `ValueError` no `int` quando existem ≥2 órfãs e apagada; o diff conserva esse comportamento (`app/ponto/services/cluster_espurio.py:31-35`).
- Catálogo conserva `None` e só declara morta premissa explicitamente falsa (`app/chamados/catalogo/premissa.py:127-134`, `app/chamados/catalogo/premissa.py:144-150`).

## 05/11 17:18 · sessao 0e757bde9acc · - · - · origem=Ronald (tmux)

Análise estática; execução dos testes e contagem real de queries: [nao verificado]. [nao verificado]

- **RED tem mordida e não é vazio.** A fixture exige chamado criado e vivo antes do ato; o RED exige 15 dias com assinatura tanto fora de `a_fechar` quanto vivo depois. Os testes de fechamento com lastro, dentro e fora da janela, reprovam uma cura que conserve tudo. (`app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:47`, `:58`, `:74`, `:80`, `:97`, `:108`.)
- **Cobertura limitada.** Os sete casos não exercitam `ata=None` com célula existente, insumos não-lista, `cobertura_parcial`, chamados sem dia forte, concorrência ou outros reconciliadores. (`app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:74`, `:80`, `:86`, `:91`, `:97`, `:102`, `:108`.)
- **Nenhum dos testes existentes nomeados muda pelo trecho alterado, por leitura estática.** `detectar` usa o pipeline real que lavra listas; `varredura` cobre abertura, dedup, fecho por perda de assinatura e alarme; COBPARC exige assinatura presente e abertura ausente; F1C4 constrói `orfas` e `lampadas` como listas; C-MIUDOS exige o literal preservado; FECHAR3 mocka `pendencias` e o gate. (`app/ponto/tests/test_detectar_cluster_espurio.py:51`, `:68`; `app/ponto/tests/test_varredura_cluster_evento.py:36`, `:44`, `:50`, `:64`; `app/ponto/tests/test_cobparc7_cluster_le_a_lista.py:39`; `app/chamados/tests/test_f1c4_3_emissores.py:65`, `:113`; `app/chamados/tests/test_c_miudos_1.py:39`; `app/chamados/tests/test_fechar3_retorno_do_gate.py:214`.)
- **Trava/atomic: sem alteração, mas sem proteção da ata até o fechamento.** O coletor lê células antes de executar os gates. O gate abre transação e trava o chamado; não trava essa leitura da célula. Há caminho concorrente possível: ler “sem assinatura”, outra transação gravar assinatura e depois consumir o candidato antigo. Ocorrência concreta: [nao verificado]. (`app/ponto/services/cluster_espurio.py:71`, `:121`, `:124`; `app/chamados/models.py:616`, `:668`, `:690`.)
- **Ordem do evento correta:** o corpo lavra, executa eventual segunda passada e só então chama cluster sob `apply_` e `_julgadas`. Ordem de pouso em produção: [nao verificado]. (`app/ponto/services/cartorio.py:948`, `:973`, `:981`, `:990`, `:994`, `:1802`.)
- **Relógio fora da borda existe antes e depois.** Fora do julgamento, `agora_deste_julgamento()` devolve `None`; parcial com período e julgamento anterior pode chegar a `houve_fato_novo`, que lê `_tzz.now()`. O diff não introduz isso. (`app/ponto/services/cluster_espurio.py:101`; `app/ponto/services/cartorio.py:82`, `:345`, `:400`; `app/ponto/management/commands/detectar_cluster_espurio.py:35`, `:50`.)
- **Queries:** incremento do coletor = **+1 SELECT de células se há algum dia forte entre vivos; +0 se não há**, considerando a eliminação de consulta com `__in` vazio pelo ORM. O N+1 de `ch.celula.data` já existia e continua. (`app/ponto/services/cluster_espurio.py:66`, `:68`, `:71`; `app/chamados/juizes.py:89`.)

Para colaborador ativo com vínculo, antes dos gates: `antes = 3 + I(achados) + K + N + P`; `depois = antes + I(dias_fortes_vivos)`. Os três SELECTs básicos são vínculo, células da janela e vivos; `K` é um `exists` por achado fora da lista; `N` são carregamentos de FK no juiz do dia; `P` são consultas condicionais de parcial/fato novo. O total de `reconciliar_colab` soma gates de fechamento e de abertura, portanto não há inteiro universal sem os dados do ato. (`app/ponto/services/cluster_espurio.py:119`, `:46`, `:66`, `:102`, `:107`, `:84`, `:123`, `:127`; `app/ponto/services/cartorio.py:400`.)

## 05/11 16:52 · sessao 10093ac31db0 · BO-[nome]-0511 · censo · origem=Code

O censo fecha em **72 unidades deduplicadas: 68 agrupadas, 3 não classificadas e 1 excluída por data futura**. A unidade é pessoa+dia explicitado; sem data, conta uma ocorrência; pares de datas contam duas. Isso é contagem do texto fornecido, não resultado de nova consulta. [nao verificado]

**G1 reúne candidatos a divergência de apresentação; os anexos não comprovam qual tela produziu cada alerta.** Eles trazem vereditos, DiaPago e classificação do exportador, mas não trazem URL, resposta HTTP, `status_dia`, retrato gravado ou perfil usado na tela. [nao verificado]

Identidades preservadas: `col253` é **id 253**; `col253 do [nome]` é **id 407**; `col751 de [nome]` é **id 923**. O id 465 aparece nas sondas, mas não constitui ocorrência do relatório. A equivalência da cópia a `main` não pôde ser confirmada pelo Git neste diretório. [nao verificado]

**G1 — tela × cálculo**

Estas são as telas e funções reais que podem responder às alegações; atribuir uma delas a cada relato continua pendente de reprodução. [nao verificado]

| Tela | Onde nasce a indicação | Autoridade lida / LEI-AKITA 2 |
|---|---|---|
| Calendário do perfil | `calendario_ponto` chama `contexto_calendario`; o template pinta `dia.status`. `app/colaboradores/views.py:1935`; `app/colaboradores/services/calendario.py:663`; `app/templates/colaboradores/partials/_calendario_grade.html:69` | Lê `status_dos_dias_lavrados`; o antigo classificador próprio foi removido. `app/colaboradores/services/calendario.py:602`; `app/colaboradores/services/calendario.py:78` |
| Espelho web | `espelho_ponto` chama `espelho_do_colab` e depois `montar_semanas`, que preenche status e rótulo. `app/ponto/views.py:158`; `app/ponto/views.py:184` | `montar_semanas` lê D4 em lote; não escolhe o status. `app/ponto/services/espelho.py:167` |
| API do espelho | `api_espelho_v2`, rota `espelho/v2/`, entrega `status`, `status_dia`, `inconsistente` e `turno_aberto`. `app/api/urls.py:81`; `app/api/views.py:1559` | Lê D4 e traduz por `status_do_app`; `inconsistente` também inclui turno geometricamente aberto. `app/api/views.py:1495`; `app/api/views.py:1514`; `app/ponto/status_dia.py:89` |
| Retrato da folha | `retrato_folha` lê o retrato gravado; `desenho_do_led` fornece cor e rótulo. `app/folha/views_retrato.py:246`; `app/folha/views_retrato.py:280` | Não consulta o exportador ao abrir. `_balde` classifica a **linha gravada**; o lavrador monta a origem pelo exportador. `app/folha/retrato_folha.py:89`; `app/folha/retrato_folha.py:1120`; `app/folha/models.py:293` |
| Lista do fechamento | `montar_resumos` fornece `folha`, `pront` e `travas`; o template também exibe o estado administrativo. `app/ponto/services/fechamento_lista.py:40`; `app/templates/ponto/_fech_linha.html:39` | Entrada no TXT e travas leem `classificar_export`; prontidão lê `prontidao`. `app/ponto/services/fechamento_lista.py:70`; `app/ponto/services/fechamento_lista.py:82` |
| Painel lateral do fechamento | `painel_fechamento` lê `fechamento_lido`; o template mostra “Aprovado”, “T. Aberto” ou “Pendente”. `app/ponto/views.py:993`; `app/templates/ponto/partials/painel_fechamento.html:10` | “Pendente” é estado administrativo; não responde, sozinho, se entra no TXT. O exportador expressamente não usa esse estado como porta. `app/folha/export.py:829` |

O juiz de **status do dia** é `status_do_dia_lavrado`; o de **entrada no TXT** é `classificar_export`; prontidão é outra pergunta. São os endereços de `JUIZES.md`, respectivamente em `app/docs/JUIZES.md:88` e `app/docs/JUIZES.md:74`. Um `veredito=concorde` anexado não substitui a chamada atual de D4, que também considera andamento e disponibilidade da lavra. `app/ponto/status_dia.py:201`

Há uma distinção demonstrável no código: a mesma linha pode mostrar o tique de entrada na folha e o badge **“Pendente”**. O primeiro lê o juiz do TXT; o segundo lê o estado administrativo. Isso não basta para declarar violação da LEI-AKITA 2. `app/templates/ponto/_fech_linha.html:11`; `app/templates/ponto/_fech_linha.html:39`; `app/folha/export.py:777`

Para **col253/id253 e col283/id283**, os anexos mostram entrada nos dois meses, mas não mostram a indicação concreta de “folha bloqueada”. Não há prova de qual tela divergiu, de qual competência ela mostrava ou de seu carimbo. [nao verificado] O retrato possui leitor de frescor, usado pela tela, que permite confrontar essa hipótese sem alterar jornadas. `app/folha/views_retrato.py:202`; `app/folha/retrato_folha.py:1268`

A sonda abaixo fica **somente como texto**, para `manage.py shell` com conta de leitura. Ela chama views reais do calendário, espelho e painel; o endpoint real da API; os montadores reais da lista e do retrato; D4 e o exportador. Não foi executada aqui nem validada contra banco. [nao verificado]

O retrato é consultado pelas funções reais de leitura, pois abrir sua view com `empresa=` chama o escritor de preferência da UI. A sonda não chama esse caminho. `app/folha/views_retrato.py:150`; `app/folha/retrato_folha.py:915`

```bash
# Em app/, com conexão de leitura já configurada.
# Informar o ID de um usuário supervisor existente, sem alterar permissões.
SONDA_SUPERVISOR_ID=123 PYTHONDONTWRITEBYTECODE=1 python -B manage.py shell <<'PY'
import datetime as dt
import json
import os
from html.parser import HTMLParser
from unittest.mock import patch

from django.contrib.auth import get_user_model
from django.db import connection, transaction
from django.template.loader import render_to_string
from django.test import RequestFactory
from django.utils import timezone
from rest_framework.test import APIRequestFactory, force_authenticate

from colaboradores.models import Colaborador
from colaboradores import views as cv
from ponto import views as pv
from api import views as av
from folha.models import RetratoFolha
from folha.export import classificar_export
from folha.retrato_folha import linha_do_colab, desenho_do_led
from ponto.janelas import janela_fechamento
from ponto.status_dia import (
    status_do_dia_lavrado, status_dos_dias_lavrados,
)
from ponto.turnos import CargaDaJanela
from ponto.models import FechamentoMensal
from ponto.services.fechamento import (
    universo_da_competencia, periodo_trancado,
)
from ponto.services.fechamento_lista import montar_resumos

# IDs do JSONL. None significa data não fornecida: não inventar uma.
G1 = [
    (504, "2026-10-21"), (226, "2026-10-21"),
    (709, "2026-10-22"), (255, "2026-10-03"),
    (204, "2026-09-21"), (43, "2026-10-10"),
    (43, "2026-10-26"), (956, "2026-10-16"),
    (956, "2026-10-22"), (502, "2026-10-09"),
    (55, "2026-10-03"), (288, "2026-10-04"),
    (523, "2026-10-25"),
    (253, None), (283, None), (92, None),
    (235, None), (219, None),
]
ANO = 2026
# Fixa somente o relógio; não reconstrói um banco histórico.
AGORA = dt.datetime.fromisoformat("2026-11-05T19:12:47.272817+00:00")
rf, arf = RequestFactory(), APIRequestFactory()
supervisor = get_user_model().objects.get(
    pk=int(os.environ["SONDA_SUPERVISOR_ID"])
)

def selecionar(obj, campos):
    return {k: obj[k] for k in campos if k in obj}

def guarda_sql(execute, sql, params, many, context):
    # Recusa inclusive SQL desconhecido: erro não vira resultado regular.
    if not sql.lstrip().upper().startswith("SELECT"):
        raise RuntimeError("SQL_nao_permitido_na_sonda")
    return execute(sql, params, many, context)

def emitir(cid, dia, mes, fonte, fn):
    out = {
        "colaborador_id": cid, "dia": dia,
        "mes": mes, "ano": ANO, "fonte": fonte,
    }
    try:
        if connection.vendor != "postgresql":
            raise RuntimeError("sonda_exige_postgresql")
        with transaction.atomic():
            with connection.cursor() as cur:
                cur.execute("SET TRANSACTION READ ONLY")
            with connection.execute_wrapper(guarda_sql):
                out["resultado"] = fn()
    except Exception as exc:
        # Não imprimir exceção com possível nome/documento.
        out["erro"] = type(exc).__name__
    print(json.dumps(out, ensure_ascii=False, default=str))

def contexto_view(modulo, funcao, query, **kwargs):
    req = rf.get("/", query)
    req.user = supervisor
    req.session = {}
    # Executa a view; intercepta somente a renderização.
    with patch.object(modulo, "render",
                      side_effect=lambda request, template, context: context):
        result = getattr(modulo, funcao)(req, **kwargs)
    if not isinstance(result, dict):
        raise RuntimeError("view_sem_contexto_autorizado")
    return result

CAMPOS_DIA = (
    "data", "status", "status_rotulo", "status_dia",
    "veredito_dia", "horas_trabalhadas", "dia_pago_lavrada",
    "inconsistente", "turno_aberto", "resolver_dia_oferecido",
)

def dia_da_tela(ctx, dia):
    if dia is None:
        return {"dia_nao_informado": True}
    linhas = [
        selecionar(x, CAMPOS_DIA)
        for x in ctx.get("dias", [])
        if str(x.get("data")) == dia
    ]
    return {"linhas": linhas, "presente": bool(linhas)}

class TextoStatus(HTMLParser):
    def __init__(self):
        super().__init__()
        self.dentro = False
        self.partes = []

    def handle_starttag(self, tag, attrs):
        if tag == "td" and dict(attrs).get("data-col") == "status":
            self.dentro = True

    def handle_endtag(self, tag):
        if tag == "td":
            self.dentro = False

    def handle_data(self, data):
        if self.dentro:
            self.partes.append(data)

def lista_fechamento(c, mes):
    qs = universo_da_competencia(mes, ANO, c.empresa_id).filter(pk=c.pk)
    fechs = {
        f.colaborador_id: f
        for f in FechamentoMensal.objects.filter(
            mes=mes, ano=ANO, colaborador__in=qs,
        ).select_related("colaborador", "colaborador__empresa")
    }
    linhas, _ = montar_resumos(qs, fechs, mes, ANO, c.empresa_id)
    result = []
    for r in linhas:
        html = render_to_string("ponto/_fech_linha.html", {
            "r": r,
            "empresa_id": c.empresa_id,
            "periodo_fechado": periodo_trancado(c.empresa_id, mes, ANO),
            "csrf_token": "NOTPROVIDED",
        })
        texto = TextoStatus()
        texto.feed(html)
        f = r["fechamento"]
        result.append({
            "badge_template": " ".join(" ".join(texto.partes).split()),
            "estado_administrativo": f.status if f else None,
            "turnos_abertos": f.turnos_abertos if f else None,
            "folha": selecionar(r["folha"] or {}, (
                "entra", "motivo", "motivo_juiz",
            )),
            "prontidao": r["pront"],
            "travas": r["travas"],
        })
    return result

def retrato(c, mes):
    r = RetratoFolha.objects.filter(
        empresa_id=c.empresa_id, mes=mes, ano=ANO,
    ).first()
    linha = linha_do_colab(r, c.pk)
    return {
        "retrato_id": r.pk if r else None,
        "lavrado_em": r.lavrado_em if r else None,
        "linha": selecionar(linha or {}, (
            "colab", "status", "motivo", "dias", "datas", "dono", "tipo",
        )),
        "led": desenho_do_led(linha) if linha is not None else None,
    }

def api(c, mes, dia):
    if c.usuario is None:
        return {"sem_usuario_do_colaborador": True}
    req = arf.get("/", {"mes": mes, "ano": ANO})
    force_authenticate(req, user=c.usuario)
    resp = av.api_espelho_v2(req)
    return {
        "http_status": resp.status_code,
        "dia": dia_da_tela(resp.data, dia),
    }

with patch.object(timezone, "now", return_value=AGORA):
    for cid, dia in G1:
        # Busca por PK exclusivamente; nunca por nome.
        c = Colaborador.objects.select_related("empresa", "usuario").get(pk=cid)
        d = dt.date.fromisoformat(dia) if dia else None

        if d:
            def calendario():
                ctx = contexto_view(cv, "calendario_ponto", {
                    "modo": "mes", "mes": d.month, "ano": ANO,
                }, cid=cid)
                carga = CargaDaJanela([c], ctx["data_ini"], ctx["data_fim"])
                juiz = status_dos_dias_lavrados(
                    [(c, d)], agora=AGORA, carga=carga,
                )[(cid, d)]
                return {
                    "janela": [ctx["data_ini"], ctx["data_fim"]],
                    "tela": dia_da_tela(ctx, dia),
                    "juiz_D4_mesma_janela": juiz,
                }

            emitir(cid, dia, d.month, "calendario_ponto", calendario)
            emitir(cid, dia, d.month, "status_do_dia_lavrado",
                   lambda: status_do_dia_lavrado(c, d, agora=AGORA))

        for mes in (10, 11):
            def export():
                ini, fim = janela_fechamento(mes, ANO, c.empresa)
                itens = classificar_export(c.empresa, mes, ANO)
                return {
                    "janela": [ini, fim],
                    "dia_na_competencia": ini <= d <= fim if d else None,
                    "itens": [
                        selecionar(x, (
                            "colaborador_id", "status", "motivo",
                            "travas_aprovacao",
                        ))
                        for x in itens if x["colaborador_id"] == cid
                    ],
                }

            emitir(cid, dia, mes, "classificar_export", export)
            emitir(cid, dia, mes, "montar_resumos+template",
                   lambda: lista_fechamento(c, mes))
            emitir(cid, dia, mes, "linha_do_colab+desenho_do_led",
                   lambda: retrato(c, mes))

            emitir(cid, dia, mes, "espelho_ponto", lambda: dia_da_tela(
                contexto_view(pv, "espelho_ponto", {
                    "colab": cid, "mes": mes, "ano": ANO,
                }), dia,
            ))

            def painel():
                ctx = contexto_view(pv, "painel_fechamento", {
                    "colab": cid, "mes": mes, "ano": ANO,
                })
                f = ctx.get("fechamento")
                return {
                    "estado_administrativo": f.status if f else None,
                    "turnos_abertos": f.turnos_abertos if f else None,
                    "periodo_fechado": ctx.get("periodo_fechado"),
                }

            emitir(cid, dia, mes, "painel_fechamento", painel)

        # API navega por mês civil; a janela de apuração é decidida dentro dela.
        for mes_api in ([d.month] if d else [10, 11]):
            emitir(cid, dia, mes_api, "api_espelho_v2",
                   lambda: api(c, mes_api, dia))
PY
```

A sonda distingue D4 na janela do calendário da chamada unitária. Essa distinção é necessária porque o próprio juiz documenta que o pareamento pode depender da janela carregada. `app/ponto/status_dia.py:225`; `app/ponto/status_dia.py:238` Para os cinco casos G1 sem data, ela consulta as competências, mas não inventa um dia regular. [nao verificado]

**G2 — dia sem porta ou zero hora**

As funções que respondem são:

| Pergunta | Função real e prova |
|---|---|
| O dia efetivamente segura a folha? | `dias_que_seguram`, alimentando `censo_alcance`. `app/chamados/services/vigia_fio.py:576` |
| Há porta operacional alcançável? | `porta_alcancavel_do_dia`: fila, pergunta viva no app, texto e âncora. `app/chamados/juizes.py:1283` |
| Há outro encaminhamento previsto, fora do fio? | `dono_fora_do_fio`: Resolver dia, ausência aberta e demais destinos nomeados. `app/chamados/services/vigia_fio.py:273` |
| Qual classe do alcance? | `censo_alcance`. `app/chamados/services/vigia_fio.py:666` |
| Qual dono L-099? | `dono_da_divergencia`, com insumos reais montados por `InsumosDoDono.dono`. `app/ponto/management/commands/e6_oraculo.py:53`; `app/ponto/management/commands/e6_oraculo.py:168` |

O vocabulário do vigia é: **a** chamado vivo com véu; **b** respondida sem texto; **c** respondida sob destino terminal; **d** colaborador na lista Cadastro×Realidade, mas dia fora; **e** demais detalhes, incluindo sem célula, sem pergunta viva, pergunta sob terminal, sem âncora ou fora do app. Há precedência entre esses ramos; um chamado terminal sozinho não determina a classe. `app/chamados/services/vigia_fio.py:666`

| Caso | Classe do vigia e dono provável | Cobertura existente |
|---|---|---|
| col253 do [nome], **id407**, 21/10 | Zero hora/furo com terminais: classe ainda indeterminada. BATIDA é provável **se** a batida continuar solta no dia atribuído e não houver prioridade cadastral. [nao verificado] | O261 para fio/pergunta sob terminal; enquadramento individual não demonstrado. `app/docs/BACKLOG.md:859`; `app/ponto/management/commands/e6_oraculo.py:98` |
| col836, 13/10 | `indefinida`, zero hora e chamado resolvido não mostram pergunta/âncora. CADASTRO apenas se confirmado o insumo cadastral prioritário. [nao verificado] | F3-BECO-1 condicional; O261. `app/docs/RELATO.md:93`; `app/docs/RELATO.md:101` |
| col836, 14/10 | Mesma insuficiência de insumos, julgada separadamente do dia 13. [nao verificado] | F3-BECO-1 condicional; O261. `app/docs/RELATO.md:101` |
| col909, 16/10 | Furo com horas parciais; BATIDA provável apenas após atribuição real das batidas. `em_analise` não prova alcance. [nao verificado] | O261; JUIZ-3 se o problema for a premissa daquele turno denunciado. `app/chamados/reconciliador.py:117`; `app/chamados/reconciliador.py:264` |
| col292, 21/09 | Relato sem porta; classe e dono não determináveis sem célula/perguntas/fila. [nao verificado] | O261; F3 depende de condições não fornecidas. `app/docs/BACKLOG.md:859` |
| col207, col332 e col446, sem dia | Candidatos a problema de alcance/encerramento; não há dia nem insumos suficientes para emitir a classe. [nao verificado] | O261/FIO MUDO condicionais. `app/chamados/services/vigia_fio.py:576`; `app/chamados/management/commands/disparar_perguntas_competencia.py:141` |

L-099 prioriza folga não definida/ausência de vínculo → CADASTRO; depois batida fora de turno ou quantidade ímpar atribuída → BATIDA; fase deslocada/Cadastro×Realidade → CADASTRO; o restante → ESTRUTURA. `app/ponto/management/commands/e6_oraculo.py:98` **Zero batidas físicas na data não autoriza chamar BATIDA**, e a quantidade física do JSONL não substitui o carregamento ampliado e a atribuição do oráculo. `app/ponto/management/commands/e6_oraculo.py:72`; `app/ponto/management/commands/e6_oraculo.py:138`

As coberturas não são intercambiáveis:

- **F3-BECO-1:** cobre a combinação de passado indefinido, pergunta viva sob terminal e problema de folga; reatar ao LIMBO fornece âncora, não encerramento. Os anexos não demonstram a combinação completa em col836. `app/docs/RELATO.md:101`; `app/docs/RELATO.md:34` [nao verificado quanto ao caso]
- **O261:** cobre alcançabilidade e carimbo com lastro; não equivale a reabrir todo chamado terminal. `app/docs/BACKLOG.md:859`; `app/chamados/reconciliador.py:340`
- **CONTRATO-20 / JUIZ-3:** verifica o turno denunciado, identificado por entrada/contexto ou data; não qualquer turno aberto da janela. `app/chamados/reconciliador.py:117`; `app/chamados/reconciliador.py:145`
- **FIO MUDO — `batida_ausente`:** exige chamado vivo com célula, motivo emitível e ausência de pergunta viva; as guardas também barram fato já perguntado/decidido. `app/chamados/management/commands/disparar_perguntas_competencia.py:141`; `app/chamados/management/commands/disparar_perguntas_competencia.py:211`
- **O168 / `pode_executar_csf`:** protege a conversa respondida antes de executar cobrança-sem-furo. Não há trilha anexada que atribua algum encerramento deste lote àquela porta. `app/ponto/supra_juiz.py:539`; `app/ponto/management/commands/supra_juiz.py:257` [nao verificado quanto aos casos]
- **O169 / cluster:** exige ao menos duas órfãs em rajada de até 15 minutos e marco faltante. Quantidade de batidas por data não prova essa classe em nenhum caso fornecido. `app/ponto/services/cluster_espurio.py:15` [nao verificado quanto aos casos]

**G3 — transnoite, troca e marco**

O caminho decisório é:

| Decisão | Código |
|---|---|
| Marcos previstos do dia | `EscalaColaborador.marcos_do_dia` lê o DNA congelado

[truncado em 20000 de 35526 caracteres]

## 05/11 16:49 · sessao c8b6f3f80328 · PD-FIO-MUDO · censo · origem=Code

HASH LIDO = a026943099b8d88468240fd4b18491141b6810af

## 1 QUEM ESCREVE e QUEM LÊ

O hash acima identifica a cópia informada; sua correspondência com um commit Git é **[não verificado]**. O aval fornecido declara universo **494**, mas não fornece seus registros, distribuição por causa nem estado individual: **[não verificado]**. (`avais.txt:1`, anexo, contando a primeira data como linha 1) [nao verificado]

**Campos e portas de escrita**

| Campo ou conjunto | Quem escreve hoje | Quem lê hoje |
|---|---|---|
| Chamado: `colaborador`, `categoria`, `subcategoria`, `modulo_origem`, `contexto_json`, `celula` | `ChamadoColaborador.abrir/criar`; para `batida_ausente`, detector, grade e C2 fornecem o contexto. Alterações posteriores de contexto passam por `anotar_contexto`, `renovar_contexto`, `retirar_contexto`, `fundir_contexto`. (`app/chamados/models.py:913`; `app/chamados/models.py:1048`; `app/ponto/management/commands/detectar_ausencias.py:693`; `app/ponto/management/commands/reconciliar_grade.py:507`; `app/chamados/reconciliador.py:879`; `app/chamados/services/contexto.py:62`; `app/chamados/services/contexto.py:128`) | Dia pelo juiz `dia_do_fato`; enquadramento pela fábrica; seleção pelo disparador; agrupamento por célula nos vigias. (`app/chamados/signals.py:178`; `app/chamados/services/disputa_emissao.py:683`; `app/chamados/management/commands/disparar_perguntas_competencia.py:141`; `app/chamados/services/vigia_fio.py:337`) |
| Chamado: `status_local`, `resolvido_em`, `via_encerramento` | Nascimento nas portas acima; transições por `_gravar_estado`, que escreve estado, carimbo e via. A entrada em terminal tem guarda de perguntas vivas ancoradas. (`app/chamados/models.py:195`; `app/chamados/models.py:215`; `app/chamados/models.py:222`; `app/chamados/models.py:238`) | Dedup, reabertura, sinais, fábrica, painel, cartório e vigias; `VIVOS`, `COBRANCA` e `TERMINAIS` são conjuntos diferentes. (`app/chamados/models.py:867`; `app/chamados/signals.py:169`; `app/chamados/services/disputa_emissao.py:691`; `app/chamados/services/painel.py:155`; `app/chamados/services/vigia_fio.py:334`; `app/chamados/catalogo/motor.py:24`) |
| Chamado: `silenciado_ate` e motivo do silêncio | Portas `arquivo.silenciar/levantar`; o C11 de ausência chama essas portas. Silenciar conserva o estado vivo. (`app/chamados/services/arquivo.py:141`; `app/chamados/services/arquivo.py:176`; `app/chamados/services/silencio_ausencia.py:86`; `app/chamados/models.py:81`) | `qs_na_fila` compara o prazo ao relógio; a alimentação de alcance usa essa fila. (`app/chamados/services/painel.py:186`; `app/chamados/juizes.py:1232`) |
| Pergunta: `disputa`, `motivo`, `data_evento`, `celula`, `chamado_destino`, texto, tipo e opções | `PerguntaDisputa.abrir` cria; na identidade existente também atualiza célula/destino. `reapontar_resolvedora` altera destino e pode preencher data ausente, relavrando no mesmo `save`. (`app/chamados/models.py:2387`; `app/chamados/models.py:2424`; `app/chamados/models.py:2439`; `app/chamados/models.py:2465`; `app/chamados/services/fio.py:832`; `app/chamados/services/fio.py:845`) | C6, identidade do fato, `resolvedora_valida`, fábrica, materialização, `chamado_que_lista`, app e alcance. A âncora é destino, com fallback para o chamado da disputa. (`app/chamados/models.py:2381`; `app/chamados/models.py:2408`; `app/chamados/juizes.py:1077`; `app/chamados/services/fio.py:116`; `app/chamados/juizes.py:1320`) |
| Pergunta: `resposta_colab`, `respondida_em` | Resposta humana por `responder_pergunta_disputa_core`; resposta automática por `responder_automaticamente`; recomposição de instante por `carimbar_resposta_pela_trilha`. Correção limpa resposta e preserva versão anterior; `carimbar(resposta=...)` também pode preencher instante quando já existe texto. (`app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/services/disputa_emissao.py:1403`; `app/chamados/services/disputa_emissao.py:1428`; `app/chamados/services/disputa_emissao.py:1445`; `app/chamados/models.py:2576`) | C6 distingue resposta com texto; validação exige resposta; crédito do painel separa respondidas; alcance recusa resposta vazia como porta da pergunta. (`app/chamados/juizes.py:1092`; `app/chamados/services/validacao.py:363`; `app/chamados/services/raiox.py:137`; `app/chamados/juizes.py:1322`) |
| Pergunta: `validada_em`, `validada_por` | `_carimbar_validacao`; devolução de validação recusada; resposta nova desfaz assinatura; correção e ressuscitação limpam; `carimbar(resposta='validar')` também preenche assinatura vazia. (`app/chamados/services/validacao.py:127`; `app/chamados/services/validacao.py:188`; `app/chamados/services/disputa_emissao.py:1295`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/models.py:1493`; `app/chamados/models.py:2580`) | Primeiro degrau do C6, propriedade `estado`, seleção de validação e materialização. (`app/chamados/juizes.py:1078`; `app/chamados/models.py:2488`; `app/chamados/services/validacao.py:366`; `app/chamados/services/validacao.py:422`) |
| Pergunta: `materializada_em`, `materializada_por`, `via_resolucao` | `_escrever_carimbo`, pelas portas `carimbar`, `carimbar_identidade`, `descarimbar`; volta em lote por `descarimbar_em_lote`. Existe troca isolada de via em `encerrar_por_fato_no_espelho`. (`app/chamados/models.py:2545`; `app/chamados/models.py:2551`; `app/chamados/models.py:2614`; `app/chamados/models.py:2635`; `app/chamados/models.py:2644`; `app/chamados/models.py:2662`) | C6, `q_vivas`, fábrica, ressuscitação, materialização, raio-X e vigia de carimbos. Carimbo e morte pelo juiz não são equivalentes. (`app/chamados/juizes.py:1087`; `app/chamados/juizes.py:1113`; `app/chamados/models.py:2714`; `app/chamados/services/disputa_emissao.py:695`; `app/chamados/models.py:1423`; `app/chamados/services/raiox.py:173`; `app/chamados/services/vigia_fio.py:462`) |
| Pergunta: `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `veredito_lavrado.lavrar`; persistência em lote por `gravar_lavras_em_lote`; costura relavra junto com destino/data. (`app/chamados/veredito_lavrado.py:197`; `app/chamados/veredito_lavrado.py:211`; `app/chamados/veredito_lavrado.py:227`; `app/chamados/services/fio.py:846`) | Crédito do painel e `pergunta_sem_acao_possivel` leem a lavra; sem lavra, esta última consulta `_pergunta_materializada_de_fato`. (`app/chamados/services/raiox.py:133`; `app/chamados/services/raiox.py:182`) |
| Célula: `data`, `trabalha`, `escala_geradora`, `dna` | Portas de geração, regeneração e reversão da célula; a geradora é insumo do juiz do vínculo do dia. (`app/ponto/portas/celula.py:100`; `app/ponto/portas/celula.py:260`; `app/ponto/portas/celula.py:488`; `app/ponto/portas/celula.py:563`; `app/escala/alimentacao.py:269`) | `marcos_vencidos`, prova do marco, juiz de dia sem trabalho, fábrica e disparador. (`app/ponto/services/cartorio.py:841`; `app/chamados/juizes.py:891`; `app/chamados/juizes.py:411`; `app/chamados/services/fabrica_da_celula.py:91`; `app/chamados/management/commands/disparar_perguntas_competencia.py:150`) |
| Célula: `ata`, `veredito`, `veredito_via`, `veredito_em`, `protestos`, `julgada_em` | `CelulaDia.lavrar_veredito` delega à porta; `lavrar_veredito`, `carimbar` e `carimbar_precedencia` escrevem os respectivos conjuntos. (`app/escala/models.py:1819`; `app/ponto/portas/celula.py:717`; `app/ponto/portas/celula.py:795`; `app/ponto/portas/celula.py:819`) | C6, fábrica pela ata, parcial na lista, ressuscitação e ambos os censos do vigia. (`app/chamados/juizes.py:1089`; `app/chamados/juizes.py:1123`; `app/chamados/services/fabrica_da_celula.py:34`; `app/chamados/services/disputa_emissao.py:868`; `app/chamados/models.py:1441`; `app/chamados/services/vigia_fio.py:377`; `app/chamados/services/vigia_fio.py:595`) |

**Abertura e emissão atuais**

- `abrir()` trava o colaborador dentro de `atomic`, consulta dedup e devolve existente vivo ou tenta renascer encerrado; ambos podem retornar `criado=False`. Seu outbox explícito só cobre `MODULOS_DE_TURNO`, conjunto sem `batida_ausente`. (`app/chamados/models.py:863`; `app/chamados/models.py:882`; `app/chamados/models.py:938`; `app/chamados/reconciliador.py:336`)
- Existe evento indireto: `post_save` do chamado criado ou com mudança de estado chama `julgar_celula`, salvo quando já está em julgamento. A tentativa fica em `engolir`; portanto não equivale a uma entrega incondicional de pergunta. (`app/chamados/signals.py:164`; `app/chamados/signals.py:172`; `app/chamados/signals.py:174`; `app/chamados/signals.py:180`)
- O cartório chama `perguntar_pela_celula` somente para células julgadas naquela passada; a fábrica consulta motivos, teto, parcial, identidade histórica e destino. (`app/ponto/services/cartorio.py:981`; `app/ponto/services/cartorio.py:995`; `app/chamados/services/fabrica_da_celula.py:74`; `app/chamados/services/fabrica_da_celula.py:145`)
- Grade abre e retorna sem emissão direta de pergunta. Detector abre primeiro e tenta construir o fio depois, em bloco que absorve exceção. C2 abre e reaponta pela porta. (`app/ponto/management/commands/reconciliar_grade.py:507`; `app/ponto/management/commands/reconciliar_grade.py:531`; `app/ponto/management/commands/detectar_ausencias.py:693`; `app/ponto/management/commands/detectar_ausencias.py:787`; `app/ponto/management/commands/detectar_ausencias.py:802`; `app/chamados/reconciliador.py:879`; `app/chamados/reconciliador.py:922`)
- O ramo FIO MUDO consulta **qualquer módulo** vivo com célula na janela, não somente `batida_ausente`; verifica resolvedoras vivas, parcial, motivo vencido, situação ativa e identidade histórica. Com `--apply`, escreve pela função `perguntar`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:141`; `app/chamados/management/commands/disparar_perguntas_competencia.py:148`; `app/chamados/management/commands/disparar_perguntas_competencia.py:154`; `app/chamados/management/commands/disparar_perguntas_competencia.py:169`; `app/chamados/management/commands/disparar_perguntas_competencia.py:211`; `app/chamados/management/commands/disparar_perguntas_competencia.py:280`)

**Cinco mecanismos pós-abertura localizáveis a partir de Q2**

A correspondência literal desta enumeração com os “5 atos de perda” do aval é **[não verificado]**; os avais da cópia localizados nomeiam abertura/evento, mas não enumeram esses cinco. (`app/docs/PROMPTS.md:1519`; `app/docs/PROMPTS.md:1853`)

| Mecanismo | Escritor ou leitura que altera a condição do fio |
|---|---|
| Reabertura sem pergunta restaurável | `reabrir` grava vivo e chama ressuscitação; esta só considera materializadas sem texto, consulta marco/C6 e pode devolver zero. Não cria pergunta inexistente. (`app/chamados/models.py:1382`; `app/chamados/models.py:1391`; `app/chamados/models.py:1423`; `app/chamados/models.py:1447`; `app/chamados/models.py:1481`) |
| Identidade/duplicata | `retratar_duplicata` carimba identidade; não encerra diretamente o chamado. Resposta pendente pode manter a duplicata viva porque precede a identidade no C6. (`app/chamados/services/fio.py:878`; `app/chamados/services/fio.py:891`; `app/chamados/juizes.py:1092`; `app/chamados/juizes.py:1113`) |
| Validação/materialização com furo residual | Validação mata no primeiro degrau; materialização tenta reconciliar destino/clones, mas o sobrevivente pode permanecer vivo por premissa residual. Recusa de materialização também pode devolver a validação. (`app/chamados/juizes.py:1078`; `app/chamados/services/materializacao.py:728`; `app/chamados/services/materializacao.py:748`; `app/chamados/models.py:730`; `app/chamados/services/validacao.py:425`) |
| Morte por leitura | Tranca, marco comprovado ou célula não acusadora podem produzir morte sem novo carimbo. Célula acusadora, resposta discordante e pendência do marco alteram a resposta. (`app/chamados/juizes.py:1082`; `app/chamados/juizes.py:1104`; `app/chamados/juizes.py:1119`; `app/chamados/juizes.py:1125`; `app/chamados/juizes.py:1137`) |
| Transferência da última resolvedora | `reapontar_resolvedora` salva novo destino e lavra; seu corpo não encerra o anterior. A guarda examina o novo destino terminal, não a perda da última resolvedora no antigo. Incidência nos 494: **[não verificado]**. (`app/chamados/services/fio.py:764`; `app/chamados/services/fio.py:770`; `app/chamados/services/fio.py:829`; `app/chamados/services/fio.py:845`) |

**Leitores e acionadores fora dos sítios iniciais**

- `fio_vivo`: disputa pode fechar, encerramento da disputa, dedup da emissão, limbo, encerramento por fato, prévia da folha, reconciliação da grade, supra e emissão retroativa. (`app/chamados/models.py:1783`; `app/chamados/juizes.py:2026`; `app/chamados/services/disputa_emissao.py:1244`; `app/chamados/services/limbo_folgas.py:201`; `app/chamados/management/commands/encerrar_perguntas_com_fato.py:83`; `app/folha/previa.py:144`; `app/ponto/management/commands/reconciliar_grade.py:460`; `app/ponto/management/commands/supra_juiz.py:215`; `app/ponto/management/commands/emitir_furo_retroativo.py:98`)
- C6 também alimenta app, portas de resposta web/API, relatórios, aviso de pendência, detalhe do dia, calendário e as propriedades `estado/viva`. (`app/chamados/services/pergunta_no_app.py:53`; `app/chamados/views.py:1238`; `app/api/views.py:2627`; `app/chamados/relatorios_disputa.py:18`; `app/chamados/services/aviso_pendencia.py:13`; `app/ponto/services/dia_do_colab.py:70`; `app/colaboradores/services/calendario.py:468`; `app/chamados/models.py:2491`; `app/chamados/models.py:2762`)
- `porta_alcancavel_do_dia` é lido pela folha e por `censo_alcance`; `vigiar_fio_morto` chama os dois censos com o mesmo dia e a mesma lista administrativa. (`app/folha/fora_da_folha.py:67`; `app/chamados/services/vigia_fio.py:634`; `app/chamados/management/commands/vigiar_fio_morto.py:162`)
- Chamadores de perda/costura incluem dedup por comando, migração de resolvedoras, reconciliador, declaração textual e limbo. Carimbos também vêm de ausência, materialização, decisão administrativa, paridade e colisão de plantio. (`app/chamados/management/commands/dedup_perguntas_fato.py:88`; `app/chamados/services/fio.py:943`; `app/chamados/reconciliador.py:1570`; `app/chamados/services/declaracao_texto.py:320`; `app/chamados/services/limbo_folgas.py:242`; `app/chamados/services/materializacao.py:546`; `app/chamados/models.py:2511`; `app/chamados/services/materializacao.py:891`; `app/chamados/services/materializacao.py:1066`)

## 2 JUIZ ou LEI que já responde

| Pergunta | Autoridades existentes e limite |
|---|---|
| O que significa C11? | No registro vivo, `_C11` é “a ausência suspende, fecha ou levanta a cobrança?”, autoridade `silencio_ausencia.reconciliar_por_ausencia`. O rótulo operacional “FIO-MUDO = C11” aparece no aval; equivalência técnica entre os dois: **[não verificado]**. (`app/core/juizes.py:117`; `app/core/juizes.py:271`; `app/docs/PROMPTS.md:1519`) |
| A pergunta está viva? Há alguma viva? | **C6** é `pergunta_viva`; **C15** é `fio_vivo`, redução `any` sobre a coleção recebida. Ambos participam, mas respondem perguntas diferentes. (`app/core/juizes.py:266`; `app/core/juizes.py:275`; `app/chamados/juizes.py:1184`) |
| Alguém consegue chegar ao dia? | **C17**, `porta_alcancavel_do_dia`: chamado da célula na fila **ou** pergunta viva no app com âncora na fila. C6/C15 não provam sozinhos essa alcançabilidade. (`app/core/juizes.py:277`; `app/chamados/juizes.py:1286`; `app/chamados/juizes.py:1322`) |
| Há ação possível sobre a pergunta? | O raio-X mantém `pergunta_sem_acao_possivel`, baseado em materialização e lavra; C17 compõe fila, app e âncora. O corte manda raio-X, folha e vigia lerem a porta, mas o corpo atual do raio-X ainda responde por seu próprio caminho de lavra. São os dois sítios a registrar, sem tratá-los como predicados equivalentes. (`app/docs/CORTES.md:404`; `app/chamados/services/raiox.py:173`; `app/chamados/juizes.py:1283`) |
| Chamado vivo significa cobrança? | **C1/VIVOS** responde existência não terminal; `COBRANCA` exclui `registrado`; fila também exclui silêncio vigente. O registro já separou “vivo” de “tem fio”. (`app/core/juizes.py:100`; `app/chamados/catalogo/motor.py:24`; `app/chamados/catalogo/motor.py:29`; `app/chamados/services/painel.py:186`) |
| Onde nasce a pergunta? | Aval escolheu evento de abertura de `batida_ausente`; lei existente manda emissão/pergunta/chamado nascerem do sinal da célula, consumidor ler a lâmpada e varredura apenas vigiar. (`app/docs/PROMPTS.md:1853`; `CLAUDE.md:166`; `CLAUDE.md:168`; `CLAUDE.md:169`; `CLAUDE.md:170`) |
| Pode fechar ou deixar morrer? | Corte FIO-MORTO-E-ESTRUTURAL exige fio vivo ou ação administrativa nomeada quando persiste a dívida; chamado nasce da célula, morre por lastro, nunca sem dono. (`app/docs/CORTES.md:253`; `app/docs/ESTRUTURAL.md:46`) |
| O marco venceu? Qual vínculo e dia? | `marcos_vencidos` usa instante e marcos encadeados; `vinculo_do_dia` responde pela geradora dentro da vigência; `dia_do_fato` e `resolvedora_valida` respondem dia/costura. (`app/ponto/services/cartorio.py:831`; `app/ponto/services/cartorio.py:845`; `app/escala/alimentacao.py:228`; `app/escala/alimentacao.py:272`; `app/chamados/models.py:2402`) |
| A competência cala a pergunta? | `dia_da_pergunta_trancado` compõe competência da data e `periodo_trancado`, considerando toque humano posterior. Exportação e tranca não são a mesma pergunta. (`app/chamados/juizes.py:917`; `app/chamados/juizes.py:939`; `app/chamados/juizes.py:949`; `app/docs/CORTES.md:216`) |
| Ausência cala tudo? | C11 suspende ausência aberta; aprovada fecha somente quando a premissa morre; rejeitada/apagada levanta seu silêncio. Parcial sem período vai à lista administrativa; não autoriza julgar dentro/fora nem fabricar falta. (`app/chamados/services/silencio_ausencia.py:77`; `app/chamados/services/silencio_ausencia.py:93`; `app/chamados/services/silencio_ausencia.py:112`; `app/docs/CORTES.md:319`) |
| Qual referência dos RED e alcance do passivo? | L-110 exige regra aplicada ao cadastro/batidas, não código antigo como gabarito; L-092 conserva o gravado exportado; L-124 mantém passado como foto até fato novo e reserva backfill único. (`app/docs/LEIS.md:136`; `app/docs/LEIS.md:125`; `app/docs/CORTES.md:421`) |
| O cron já deixou de julgar FIO MUDO? | Não nesta cópia: o ramo permanece escritor e em `JUIZES_POR_VARREDURA`; a exceção tem saída condicionada à medida e ao pouso do C11 operacional. (`app/config/crons.py:1995`; `app/chamados/management/commands/disparar_perguntas_competencia.py:280`; `app/docs/CORTES.md:305`) |

## 3 CONTRA-EXEMPLOS pela regra do aval

| Leitura aparentemente óbvia | Contra-exemplo demonstrável |
|---|---|
| “Toda abertura já tem pergunta porque existe sinal.” | O sinal pode sair por `em_julgamento`; a fábrica só roda nas células julgadas naquela passada. Existência do sinal não demonstra cobertura universal da abertura. (`app/chamados/signals.py:174`; `app/ponto/services/cartorio.py:995`) |
| “Vivo sem pergunta é sempre defeito.” | `registrado` é vivo sem ação devida; parcial pode estar legitimamente na lista do admin; ausência aberta tem decisão pendente própria. (`app/chamados/catalogo/motor.py:24`; `app/chamados/services/disputa_emissao.py:892`; `app/chamados/services/vigia_fio.py:435`) |
| “Criou há 30 dias, então ficou mudo no nascimento.” | `criado_em` registra nascimento; a vitalidade é leitura atual, afetada por validação, tranca, resposta, identidade e célula. Estado no nascimento dos 494: **[não verificado]**. (`app/chamados/models.py:61`; `app/chamados/juizes.py:1077`) |
| “Virou meia-noite: o turno acabou.” | Turno 22h–06h ainda não venceu a saída às 06h10; o juiz compara marco encadeado mais margem ao instante. Saída de intervalo às 00h30 também não prova saída final. (`CLAUDE.md:185`; `app/chamados/services/fabrica_da_celula.py:18`; `app/ponto/services/cartorio.py:845`; `app/chamados/services/disputa_emissao.py:638`) |
| “No mês civil de outubro vale a mesma tranca.” | Com

[truncado em 20000 de 32727 caracteres]

## 05/11 16:48 · sessao 9b2946a29690 · PD-O168 · censo · origem=Code

HASH LIDO = a026943099b8d88468240fd4b18491141b6810af

## 1 QUEM ESCREVE e QUEM LÊ

O identificador acima é o da cópia indicada no enunciado; sua correspondência com um commit Git é [não verificado]. As referências a `avais.txt` e `backlog_obras.txt` abaixo numeram as linhas de conteúdo dos anexos fornecidos. Medições atuais de produção, DIFF sombra, percentual da fila viva e `CSF_RETRATADO=0`: [não verificado]. [nao verificado]

**A separação atual:** `pode_executar_csf` responde sobre conversa; `classificar_dia` classifica contradições; as portas do chamado decidem e gravam o fechamento com outras guardas. A única chamada de produção encontrada da guarda CSF está no executor do supra (`app/ponto/supra_juiz.py:539`; `app/ponto/supra_juiz.py:204`; `app/chamados/models.py:629`; `app/ponto/management/commands/supra_juiz.py:261`).

| Campo ou relação | Quem escreve | Quem lê, inclusive fora dos sítios iniciais |
|---|---|---|
| `PerguntaDisputa.resposta_colab`, `respondida_em` | Resposta humana; resposta automática; recuperação do carimbo pela trilha; reabertura para correção; ressuscitação; `carimbar` quando existe texto. A reabertura limpa texto/carimbos e preserva resposta anterior (`app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/services/disputa_emissao.py:1407`; `app/chamados/services/disputa_emissao.py:1428`; `app/chamados/services/disputa_emissao.py:1445`; `app/chamados/models.py:1493`; `app/chamados/models.py:2576`). | CSF; C6; respostas sem veredito; validação; materializador; retenção de irmãs; fio; estado da pergunta; verbos e lavra persistida (`app/ponto/supra_juiz.py:545`; `app/chamados/juizes.py:1092`; `app/chamados/models.py:1772`; `app/chamados/services/validacao.py:363`; `app/chamados/services/materializacao.py:1292`; `app/chamados/juizes.py:1691`; `app/chamados/services/fio.py:336`; `app/chamados/models.py:2492`; `app/chamados/services/verbos.py:311`; `app/chamados/veredito_lavrado.py:167`). |
| `validada_em`, `validada_por` | Validação escreve pelo carimbo próprio; `PerguntaDisputa.carimbar` também pode preencher validação; reabertura e ressuscitação limpam esses campos (`app/chamados/services/validacao.py:127`; `app/chamados/models.py:2581`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/models.py:1493`). | C6, guarda de perguntas ancoradas e respostas sem veredito. CSF não consulta esses campos (`app/chamados/juizes.py:1078`; `app/chamados/models.py:327`; `app/chamados/models.py:1773`; `app/ponto/supra_juiz.py:539`). |
| `materializada_em`, `materializada_por`, `via_resolucao` | Atribuição central em `_escrever_carimbo`; `carimbar` verifica C6 e desfaz a alteração quando a pergunta continua viva; `carimbar_identidade` possui protocolo distinto, sem essa consulta (`app/chamados/models.py:2545`; `app/chamados/models.py:2585`; `app/chamados/models.py:2598`; `app/chamados/models.py:2614`). | C6, disputa, materialização, emissão e raio X. CSF continua lendo apenas `respondida_em` (`app/chamados/juizes.py:1087`; `app/chamados/models.py:1773`; `app/chamados/services/materializacao.py:1292`; `app/chamados/services/disputa_emissao.py:909`; `app/chamados/services/raiox.py:173`; `app/ponto/supra_juiz.py:545`). |
| `DisputaSupervisao.chamado` / reversa `disputa_origem` | Nascimentos de disputa atribuem o pai; a relação está declarada no modelo (`app/chamados/services/disputa_emissao.py:523`; `app/chamados/services/disputa_emissao.py:962`; `app/chamados/services/disputa_emissao.py:1182`; `app/chamados/models.py:1668`). | CSF percorre **todas** as perguntas das disputas de origem; fio, retenção, guarda de encerramento, entrada sincronizada e receiver disputa→pai também leem a relação (`app/ponto/supra_juiz.py:543`; `app/chamados/services/fio.py:131`; `app/chamados/juizes.py:1693`; `app/chamados/models.py:328`; `app/ponto/services/entrada_sincronizada.py:56`; `app/chamados/signals.py:28`). |
| `PerguntaDisputa.chamado_destino`, `data_evento` / reversa `perguntas_que_resolvem` | Nascimento/reuso por `PerguntaDisputa.abrir`; reapontamento por `fio.reapontar_resolvedora`. A porta recebe a data antes de julgar e relavrar (`app/chamados/models.py:2387`; `app/chamados/models.py:2439`; `app/chamados/models.py:2466`; `app/chamados/services/fio.py:784`; `app/chamados/services/fio.py:816`). | CSF; âncora do fio; gate de fechamento; retenção; emissão; tranca; materialização (`app/ponto/supra_juiz.py:547`; `app/chamados/services/fio.py:97`; `app/chamados/models.py:328`; `app/chamados/juizes.py:1692`; `app/chamados/services/disputa_emissao.py:929`; `app/chamados/services/competencia_trancada.py:140`; `app/chamados/services/materializacao.py:561`). |
| `FollowupChamado`: conteúdo, autor e instante | `nota.anexar_ocorrencia`; resposta livre só cria mensagem quando há conteúdo; resposta rápida exige conteúdo e cria `mensagem_dp` (`app/chamados/services/nota.py:75`; `app/chamados/services/nota.py:103`; `app/chamados/services/acoes_chamado.py:289`; `app/chamados/services/fio.py:705`; `app/chamados/services/fio.py:714`). | Detalhe, fio, contexto das ações e verbos. **CSF não lê followups** (`app/chamados/views.py:352`; `app/chamados/services/fio.py:172`; `app/chamados/services/acoes_chamado.py:402`; `app/chamados/services/verbos.py:266`; `app/ponto/supra_juiz.py:539`). |
| `ChamadoColaborador.status_local`, `resolvido_em`, `via_encerramento`, urgência e `respondido_por` | `_gravar_estado` concentra a escrita das portas: terminal carimba resolução/via e rebaixa urgência; reabertura limpa resolução/via. A auditoria antecede o save, no mesmo atomic (`app/chamados/models.py:195`; `app/chamados/models.py:222`; `app/chamados/models.py:238`; `app/chamados/models.py:242`; `app/chamados/models.py:274`). | Motor de transição; cartório; supra; gates; receivers; C6 para destino encerrado; fio e filas (`app/chamados/models.py:639`; `app/ponto/services/cartorio.py:1334`; `app/ponto/management/commands/supra_juiz.py:212`; `app/chamados/models.py:215`; `app/chamados/signals.py:81`; `app/chamados/models.py:255`; `app/chamados/services/fio.py:117`). |
| `DisputaSupervisao.fechada_em`, resolução e autor | `DisputaSupervisao.fechar`; portas de reabertura limpam o fechamento (`app/chamados/models.py:1785`; `app/chamados/models.py:1850`; `app/chamados/models.py:2052`; `app/chamados/models.py:2076`). | Receiver pede reconciliação do pai; fio e validação também leem o fechamento (`app/chamados/signals.py:28`; `app/chamados/signals.py:42`; `app/chamados/services/fio.py:135`; `app/chamados/services/validacao.py:139`). |
| Célula: `trabalha`, DNA, vínculo gerador e regeneração | Portas de geração/regeneração escrevem a previsão e o DNA (`app/ponto/portas/celula.py:100`; `app/ponto/portas/celula.py:247`; `app/ponto/portas/celula.py:451`). | `vinculo_do_dia`, grade, cartório, juiz de dia sem trabalho e guarda de cobrança órfã (`app/escala/alimentacao.py:269`; `app/ponto/services/cartorio.py:1126`; `app/ponto/services/cartorio.py:1408`; `app/chamados/juizes.py:420`; `app/ponto/management/commands/supra_juiz.py:275`). |
| Célula: `veredito`, via, instante, `protestos`, `ata`, status do dia | `lavrar_veredito`; cartório e decisão administrativa solicitam a lavra. `carimbar_precedencia` escreve a chave de precedência na mesma ata (`app/ponto/portas/celula.py:717`; `app/ponto/services/cartorio.py:1802`; `app/ponto/services/veredito_celula.py:330`; `app/ponto/portas/celula.py:819`). | Executor supra; `marcos_acesos`/`marcos_inexistentes`; C6; reconciliação/materialização; leitores de porta da folha (`app/ponto/management/commands/supra_juiz.py:246`; `app/ponto/services/lastro.py:98`; `app/ponto/services/lastro.py:153`; `app/chamados/juizes.py:1089`; `app/chamados/reconciliador.py:972`; `app/folha/fora_da_folha.py:65`). |
| Célula: `julgada_em`, `impressao`, ausência, entrada/saída e `insumos_em` | `carimbar`, solicitado pelos dois ramos do cartório (`app/ponto/portas/celula.py:795`; `app/ponto/services/cartorio.py:1264`; `app/ponto/services/cartorio.py:1822`). | Cartório decide se pula o julgamento; fábrica lê o instante anterior do ato. A impressão inclui batidas, status dos chamados, DNA, veto, teto, regra, fio, parcial e isenção (`app/ponto/services/cartorio.py:1358`; `app/ponto/services/cartorio.py:943`; `app/ponto/services/cartorio.py:443`). |
| Contagens `chamados_vivos`, `chamados_total`, `cobrancas_vivas` e saída `cods` | Cartório, supra e retroativo montam os insumos; `classificar_dia` produz contradições sem I/O (`app/ponto/services/cartorio.py:1413`; `app/ponto/management/commands/supra_juiz.py:223`; `app/ponto/management/commands/emitir_furo_retroativo.py:101`; `app/ponto/supra_juiz.py:204`). | Cartório usa códigos para lavra/emissão/fechamento; supra usa classificação para placar e alarme, mas usa a **lavra persistida** para retratar (`app/ponto/services/cartorio.py:1514`; `app/ponto/services/cartorio.py:1802`; `app/ponto/management/commands/supra_juiz.py:239`). |
| `CSF_RETRATADO` | Contador em memória incrementado somente quando `retratar` retorna verdadeiro (`app/ponto/management/commands/supra_juiz.py:286`). | Impressão do placar; não é um motivo persistido no evento `estado_chamado` (`app/ponto/management/commands/supra_juiz.py:299`; `app/chamados/models.py:275`). |

**Funções e encadeamentos atuais:**

- `pode_executar_csf` retorna falso ao encontrar qualquer `respondida_em` na origem ou no destino; não restringe por pergunta viva, validação, materialização ou disputa aberta. Seus testes cobrem origem, destino, ausência de resposta e coleção vazia (`app/ponto/supra_juiz.py:543`; `app/ponto/supra_juiz.py:547`; `app/ponto/tests/test_supra_juiz_executor.py:38`).
- `classificar_dia` possui quatro chamadas de produção: cartório sem batidas, cartório normal, supra e emissão retroativa. Separa chamado vivo de cobrança com fio vivo (`app/ponto/services/cartorio.py:1226`; `app/ponto/services/cartorio.py:1413`; `app/ponto/management/commands/supra_juiz.py:223`; `app/ponto/management/commands/emitir_furo_retroativo.py:101`; `app/ponto/supra_juiz.py:213`).
- Eventos de batida/ausência chamam `julgar_celula`; o lote chama `julgar_colab`. Dentro do cartório, os pedidos de reconciliação estão nos adiados, marcos satisfeitos, marcos inexistentes, CSF/dia sem trabalho e duplicatas, sem chamada CSF nesses pontos (`app/ponto/signals.py:131`; `app/ponto/signals.py:143`; `app/ponto/management/commands/processar_cartorio.py:65`; `app/ponto/services/cartorio.py:963`; `app/ponto/services/cartorio.py:1488`; `app/ponto/services/cartorio.py:1501`; `app/ponto/services/cartorio.py:1522`; `app/ponto/services/cartorio.py:1600`; `app/ponto/services/cartorio.py:1615`).
- As portas são `transicionar/fechar`, `reconciliar`, `encerrar`, `cancelar` e `encerrar_por_competencia_trancada`; convergem na escrita de estado. A reconciliação consulta premissa e pergunta viva; as demais possuem seus protocolos próprios, sem consultar CSF (`app/chamados/models.py:122`; `app/chamados/models.py:430`; `app/chamados/models.py:616`; `app/chamados/models.py:648`; `app/chamados/models.py:651`; `app/chamados/models.py:749`; `app/chamados/models.py:1536`; `app/chamados/models.py:1556`).
- `retratar→_retratar` acrescenta retenção, tratamento de justificativa, resolução das perguntas e nota; `encerrar_com_retencao` consulta retenção antes de encerrar. Materialização também consulta irmãs sem consumo antes de reconciliar destino/clones (`app/chamados/reconciliador.py:492`; `app/chamados/reconciliador.py:518`; `app/chamados/reconciliador.py:523`; `app/chamados/reconciliador.py:532`; `app/chamados/reconciliador.py:536`; `app/chamados/reconciliador.py:540`; `app/chamados/services/materializacao.py:730`; `app/chamados/services/materializacao.py:738`).
- Disputa fechada solicita reconciliação do pai; fechamento do chamado solicita resolução das perguntas em savepoint próprio. Falha nessa resolução é registrada e não necessariamente desfaz o fechamento do chamado (`app/chamados/signals.py:42`; `app/chamados/signals.py:45`; `app/chamados/signals.py:83`; `app/chamados/signals.py:91`).

A exaustão semântica de **todo leitor** desses campos em todo o repositório, além dos leitores nomeados acima e dos fechadores da seção 5, é [não verificado].

## 2 JUIZ ou LEI QUE JÁ RESPONDE

| Pergunta | Autoridade ou lei existente |
|---|---|
| Cobrança sem furo com conversa pode morrer automaticamente? | A guarda nominal é `pode_executar_csf`; a O168 exige sua validade também no evento e condiciona a retirada de `--executar` à guarda no evento e `CSF_RETRATADO` medido em zero (`app/ponto/supra_juiz.py:539`; `app/docs/BACKLOG.md:1379`). |
| Qual é o complemento vigente da obra? | Núcleo, DIFF sombra, `>10% fila viva = PAREI`, padrão juiz único; o anexo repete isso em C20-O168 (`avais.txt:1`, anexo; `backlog_obras.txt:8`, anexo). Denominador operacional de “fila viva” e DIFF medido: [não verificado]. |
| O chamado está vivo? Ele cobra alguém? | `VIVOS` responde ao estado; `fio_vivo` responde ao fio. O registro distingue essas perguntas (`app/core/juizes.py:100`; `app/core/juizes.py:261`; `app/core/juizes.py:275`). |
| A transição é permitida? A premissa morreu? | `decidir` e `premissa_morta` são autoridades distintas; a porta de reconciliação também consulta pergunta viva (`app/core/juizes.py:262`; `app/core/juizes.py:263`; `app/chamados/models.py:648`; `app/chamados/models.py:651`). |
| A pergunta está viva? A disputa pode fechar? | C6=`pergunta_viva`; C7=`DisputaSupervisao.pode_fechar`. C7 exige ausência de resposta sem veredito e de fio vivo (`app/core/juizes.py:266`; `app/core/juizes.py:267`; `app/chamados/models.py:1778`). |
| O dia não tem trabalho? Os marcos cobrados já acenderam? | `dia_sem_trabalho` responde à primeira; `marcos_acesos` responde à segunda. O cartório possui ainda `_protesta`, lendo `trabalha is not False` e marcos acesos (`app/core/juizes.py:278`; `app/ponto/services/lastro.py:153`; `app/ponto/services/cartorio.py:1407`). |
| Quem governa dia, vínculo e janela? | `data_do_chamado`; `vinculo_do_dia`; `janela_atual/janela_fechamento`; `dia_encerrado` para encerramento do turno (`app/core/juizes.py:265`; `app/escala/alimentacao.py:269`; `app/ponto/janelas.py:54`; `app/ponto/janelas.py:60`; `app/ponto/turnos.py:2105`). |
| Evento e varredura podem manter decisões independentes? | A seção 4a manda evento julgar/carimbar e consumidor ler a lâmpada; contrato 4b exige um juiz por pergunta e um escritor por entidade. L-060/L-061 registram cron vigia e consumidor leitor (`CLAUDE.md:166`; `CLAUDE.md:175`; `app/docs/LEIS.md:94`; `app/docs/LEIS.md:95`). |

**Há respostas sobrepostas, mas não equivalentes:** CSF protege qualquer pergunta respondida; C6 decide vida da pergunta; a retenção protege resposta sem consumo; a premissa decide se o fato ainda sustenta cobrança. Hoje um fechamento pode passar por algumas dessas autoridades sem consultar CSF (`app/ponto/supra_juiz.py:545`; `app/chamados/juizes.py:1077`; `app/chamados/reconciliador.py:437`; `app/chamados/models.py:648`).

A tabela JUIZES é consulta, e o registro vivo é `core/juizes.py`; o bloco de autoridades de chamado não registra CSF entre suas entradas (`app/docs/JUIZES.md:9`; `app/docs/JUIZES.md:14`; `app/core/juizes.py:261`).

## 3 CONTRA-EXEMPLOS PELA REGRA DO AVAL

| Leitura que erra | Contraexemplo demonstrado |
|---|---|
| “Pergunta morta significa sem conversa.” | Pergunta validada deixa de estar viva pelo C6, mas `respondida_em` continua impedindo CSF. Materialização também não é consultada pela guarda CSF (`app/chamados/juizes.py:1078`; `app/ponto/supra_juiz.py:545`). |
| “Conversa significa qualquer mensagem que CSF encontra.” | Uma mensagem apenas em `FollowupChamado`, sem pergunta respondida, não é percorrida por CSF. A extensão normativa da palavra “conversa” a todo followup não está explicitada além da guarda nomeada: [não verificado] (`app/chamados/services/nota.py:103`; `app/ponto/supra_juiz.py:543`). |
| “Sem CSF, o caminho necessariamente enterra uma resposta.” | R pode recusar por premissa/pergunta viva; `_retratar` pode reter resposta sem consumo; duplicata pode transferir a pergunta ao sobrevivente; FIO-DUP tem guarda própria por texto (`app/chamados/models.py:648`; `app/chamados/models.py:651`; `app/chamados/reconciliador.py:518`; `app/chamados/models.py:420`; `app/ponto/services/cartorio.py:1597`). |
| “Minutos realizados suficientes provam ausência de furo.” | Pontas casadas podem englobar intervalo sem batida. CSF exige todas as células casadas, além de realizado≥previsto (`app/ponto/supra_juiz.py:458`; `app/ponto/supra_juiz.py:466`; `app/ponto/tests/test_supra_juiz_cobranca.py:32`). |
| “Virou o dia civil, terminou o fato.” | Turno da véspera pode continuar aberto após meia-noite; o encerramento consulta abertura viva e fim previsto. A contagem do cartório atribui madrugada ao dia-turno (`CLAUDE.md:185`; `app/ponto/turnos.py:2105`; `app/ponto/services/cartorio.py:1368`). |
| “Outubro civil é competência outubro.” | Com corte 21, 20/10 encerra uma janela e 21/10 inicia a seguinte; o corte efetivo vem da empresa (`app/ponto/janelas.py:10`; `app/ponto/janelas.py:24`; `app/ponto/janelas.py:36`). |
| “Vínculo ativo atual explica o passado.” | A geradora da célula governa o dia dentro da vigência; geradora fora da vigência não autoriza substituir silenciosamente pelo vínculo novo. DNA refeito depois da cobrança também não prova que o marco nunca existiu (`app/escala/alimentacao.py:269`; `app/ponto/management/commands/supra_juiz.py:264`). |
| “Sem célula, ata ou lavra significa sem furo.” | `sem_celula` é ausência de jurisdição; `trabalha=None` sem ata não declara folga; executor sem lavra conta `CSF_SEM_LAVRA`, sem retratar (`app/ponto/supra_juiz.py:38`; `app/chamados/juizes.py:420`; `app/ponto/management/commands/supra_juiz.py:245`). |
| “Ausência aprovada cala o dia inteiro.” | Ausência parcial com período pode deixar resto missing cobrável (`app/chamados/juizes.py:398`; `app/chamados/juizes.py:424`; `app/ponto/supra_juiz.py:530`). |
| “Dia ausente do carregamento significa fato ausente.” | A alimentação exige janela completa; chave ausente não distingue célula inexistente de célula não carregada. O lote do cartório alcança todas as células passadas; `julgar_colab` não impõe sozinho piso de competência (`app/escala/alimentacao.py:14`; `app/escala/alimentacao.py:240`; `app/ponto/management/commands/processar_cartorio.py:49`; `app/ponto/services/cartorio.py:912`). |
| “Consultar CSF antes garante decisão concorrente atual.” | CSF percorre relações sem trava própria; as portas recarregam/travam o chamado e a costura usa a mesma linha. Resposta concorrente entre a consulta CSF e o fecho, sua serialização e dano efetivo: [não verificado] (`app/ponto/supra_juiz.py:543`; `app/chamados/models.py:668`; `app/chamados/services/fio.py:752`). |
| “`CSF_RETRATADO=0` prova cobertura do evento.” | O contador só sobe no executor, após retorno verdadeiro de `retratar`; fechamentos do cartório usam outros contadores. Zero isolado não demonstra passagem pela guarda no evento (`app/ponto/management/commands/supra_juiz.py:286`; `app/ponto/services/cartorio.py:1524`; `app/chamados/models.py:275`). |

## 4 LISTA DE CASOS RED

Os casos abaixo são casos de contrato; sua ocorrência atual em produção é [não verificado]. As expectativas vêm da O168, do complemento fornecido e das leis indicadas, não de comparação com a implementação antiga (`app/docs/BACKLOG.md:1379`; `avais.txt:1`, anexo).

| Caso | Cadastro + estado + ato | O que a regra exige |
|---|---|---|
| RED-01 — resposta na origem | Dia de trabalho completo; cobrança viva; pergunta de `disputa_origem` respondida; evento tenta fechar como CSF. | Preservar da retratação automática: conversa exige decisão humana (`app/docs/BACKLOG.md:1379`; `app/ponto/supra_juiz.py:540`). |
| RED-02 — resposta no destino | Mesmo cadastro, pergunta respondida ligada por `chamado_destino`; fecho automático pelo cartório. | A mesma proteção do RED-

[truncado em 20000 de 30468 caracteres]

## 05/11 16:48 · sessao a44cf8dd5ae4 · PD-O261-ITEM3-PEP · censo · origem=Code

HASH LIDO = a026943099b8d88468240fd4b18491141b6810af — correspondência com objeto Git [nao verificado].

## 0. Localização de “item 3” e “PEP”

Há três numerações possíveis, sem escolher entre elas:

| Localização | O que “item 3” designa |
|---|---|
| Fila recente **O261-E-F2-FILA** | “carimbar sem select_for_update, carimbo em memoria apos erro pos-C6”. O RELATO separa memória, registrada como curada, de trava, registrada como parada. `app/docs/BACKLOG.md:1154`; `app/docs/RELATO.md:66`. |
| Corte **FIO-MORTO-E-ESTRUTURAL** | Os três leitores: fábrica de perguntas, supra_juiz e registro `_C1`. `app/docs/CORTES.md:64`; `app/docs/PROMPTS.md:1184`; `app/core/juizes.py:100`. |
| Ordem histórica de 18/10 | Contador de fio morto, esperado zero fora do dia de hoje. `app/docs/RELATO-ARQUIVO.md:368`. |

O aval MANHA-0511 escreve literalmente **“O261 item 3 PEP no carimbar”**; BACKLOG e RELATO repetem a ordem. O pouso documentado da F2-FILA informa a restauração da memória, mas não declara a metade trava resolvida. `app/docs/PROMPTS.md:1853`; `app/docs/BACKLOG.md:1738`; `app/docs/RELATO.md:39`; `app/docs/RELATO.md:53`; `app/docs/RELATO.md:66`.

**Policy Enforcement Point** aparece por extenso no aval **O261-VETO-NA-PORTA**, associado à aplicação do veto dentro da porta única. Sua equivalência com “PEP” no item MANHA é uma leitura possível, sem definição literal nesse item **[nao verificado]**. As ocorrências “PEP 562” do arquivo histórico tratam de módulos Python. `app/docs/PROMPTS.md:1656`; `app/docs/PROMPTS.md:1853`; `app/docs/RELATO-ARQUIVO.md:17996`; `app/docs/RELATO-ARQUIVO.md:28267`.

## 1. Quem escreve e quem lê

**Comportamento atual de `carimbar`:** recebe uma instância, aplica o carimbo nela, consulta C6 e devolve `False` restaurando a memória se a pergunta continuar viva. Se C6 aceitar, pode lavrar o veredito e salva os campos. Exceções do juiz, da lavra ou do salvamento restauram a memória. O corpo não relê nem trava a linha recebida. `app/chamados/models.py:2551`; `app/chamados/models.py:2572`; `app/chamados/models.py:2584`; `app/chamados/models.py:2592`; `app/chamados/models.py:2605`.

| Campo ou conjunto | Escritores atuais | Leitores e finalidade |
|---|---|---|
| `materializada_em`, `materializada_por` | `_escrever_carimbo`; limpeza persistida em `descarimbar_em_lote`. As demais portas delegam a esses escritores. `app/chamados/models.py:2545`; `app/chamados/models.py:2635`; `app/chamados/models.py:2644`. | C6 lê o instante; `estado` combina validação, carimbo e C6; procedência lê autor e instante. `app/chamados/juizes.py:809`; `app/chamados/juizes.py:1077`; `app/chamados/models.py:2481`; `app/ponto/services/procedencia_batida.py:69`. |
| `via_resolucao` | Escritor da tripla, limpeza em lote, `encerrar_por_fato_no_espelho` e `relavra_via.relavrar`. Portanto, “porta única da tripla” não significa um único escritor da via isoladamente. `app/chamados/models.py:2549`; `app/chamados/models.py:2652`; `app/chamados/models.py:2662`; `app/chamados/services/relavra_via.py:123`. | C6, juiz do lastro, SQL de vivas, relavra, cartório e apresentação. `app/chamados/juizes.py:645`; `app/chamados/juizes.py:1112`; `app/chamados/models.py:2734`; `app/chamados/services/relavra_via.py:116`; `app/ponto/services/cartorio.py:1599`; `app/templates/chamados/partials/_card_atendimento.html:186`. |
| `respondida_em` | `carimbar`, quando solicitado e há texto; resposta humana; resposta automática; recuperação pela trilha; reabertura e ressuscitação. `app/chamados/models.py:2576`; `app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/services/disputa_emissao.py:1428`; `app/chamados/services/disputa_emissao.py:1445`; `app/chamados/models.py:1493`. | C6, estado, validação, filas e API. `app/chamados/juizes.py:1092`; `app/chamados/models.py:2491`; `app/chamados/services/validacao.py:363`; `app/chamados/juizes.py:1339`; `app/api/views.py:2636`. |
| `validada_em`, `validada_por` | `carimbar` pode preencher ambos com `resposta='validar'`; validação os preenche ou devolve; resposta nova, correção e ressuscitação os limpam. `app/chamados/models.py:2580`; `app/chamados/services/validacao.py:127`; `app/chamados/services/validacao.py:187`; `app/chamados/services/disputa_emissao.py:1295`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/models.py:1493`. | C6 encerra pela validação antes dos demais degraus; estado e materialização também a leem; procedência prefere seu autor ao autor da materialização. `app/chamados/juizes.py:1078`; `app/chamados/models.py:2488`; `app/chamados/services/materializacao.py:1294`; `app/ponto/services/procedencia_batida.py:72`. |
| `resposta_colab` | `carimbar` apenas lê o texto. Escritores: resposta humana, automática e limpeza para correção. `app/chamados/models.py:2576`; `app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/services/disputa_emissao.py:1428`. | C6, derivação do slot, materialização, API, calendário, ficha e prévia da folha. `app/chamados/juizes.py:324`; `app/chamados/juizes.py:1092`; `app/chamados/services/materializacao.py:1183`; `app/api/views.py:2580`; `app/colaboradores/services/calendario.py:474`; `app/colaboradores/services/ficha.py:171`; `app/folha/previa.py:142`. |
| `veredito_lavrado`, `julgado_em`, `janela_expira_em` | A opção `lavrar_veredito` chama `veredito_lavrado.lavrar`; a restauração passa por `desfazer_lavra_em_memoria`; persistência em lote tem porta própria. `app/chamados/models.py:2593`; `app/chamados/veredito_lavrado.py:211`; `app/chamados/veredito_lavrado.py:240`; `app/chamados/veredito_lavrado.py:197`. | `esta_stale` verifica ausência, expiração e mudança de insumos; raio-X consome a lavra para determinar ausência de ação. `app/chamados/veredito_lavrado.py:173`; `app/chamados/services/raiox.py:173`. |
| `recusa_motivo` | A porta de identidade pode escrevê-lo; materialização, resposta de escape e recusa humana também escrevem. `app/chamados/models.py:2630`; `app/chamados/services/materializacao.py:817`; `app/chamados/services/materializacao.py:1069`; `app/chamados/services/disputa_emissao.py:1340`; `app/chamados/services/validacao.py:545`. | Validação fotografa e relê a recusa; passivo a conserva no conjunto de reversão. `app/chamados/services/validacao.py:136`; `app/chamados/services/validacao.py:180`; `app/chamados/services/passivo_validado.py:64`. |
| `_pv_memo` | `pergunta_viva` grava o memo; descarimbo e restauração o removem. `app/chamados/juizes.py:833`; `app/chamados/models.py:2612`; `app/chamados/models.py:2638`. | C6 reutiliza o resultado quando a chave da instância coincide; a chave não contém `materializada_por`. `app/chamados/juizes.py:809`; `app/chamados/juizes.py:835`. |

**Chamadores do carimbo comum — oito pontos atuais:**

| Função chamadora | Ponto de chamada |
|---|---|
| `encerrar_fantasma` | `app/chamados/models.py:2674`. |
| `lavrar_sem_turno_aberto` | `app/chamados/reconciliador.py:405`. |
| `retratar_perguntas_absolvidas_por_celula` | `app/chamados/reconciliador.py:620`. |
| `resolver_perguntas_por_chamado_resolvido`, dois ramos | `app/chamados/services/disputa_emissao.py:1603`; `app/chamados/services/disputa_emissao.py:1675`. |
| `_materializar_ausencia_de_resposta` | `app/chamados/services/materializacao.py:546`. |
| `_materializar_uma` | `app/chamados/services/materializacao.py:1199`. |
| `materializar_pendentes_aprovadas` | `app/chamados/services/materializacao.py:1346`. |

`_materializar_uma` já envolve plantio e carimbo em transação; a recusa do carimbo levanta `_Reprovada` dentro dela. Isso não equivale a uma releitura travada na própria `carimbar`. `app/chamados/services/materializacao.py:1142`; `app/chamados/services/materializacao.py:1197`; `app/chamados/models.py:2572`.

**Identidade:** `carimbar_identidade` aceita quatro vias, salva sem consultar C6 e pode preservar instante/autor. Seus chamadores são decisão administrativa, duplicata, paridade e colisão. Uma resposta por validar pode continuar viva porque sua precedência vem antes da identidade. `app/chamados/models.py:2275`; `app/chamados/models.py:2614`; `app/chamados/models.py:2511`; `app/chamados/services/fio.py:891`; `app/chamados/services/materializacao.py:891`; `app/chamados/services/materializacao.py:1066`; `app/chamados/juizes.py:1092`.

**Caminhos inversos:** descarimbar atende julgamento de cópia, reabertura de fantasma, resposta nova, competência e desfazimento sem lastro; a versão em lote atende ressuscitação e correção. A ressuscitação também é chamada por desarquivamento e cartório. `app/chamados/models.py:1444`; `app/chamados/models.py:2685`; `app/chamados/models.py:2702`; `app/chamados/models.py:2709`; `app/chamados/services/relavra_via.py:231`; `app/chamados/models.py:1493`; `app/chamados/services/disputa_emissao.py:1412`; `app/chamados/views_arquivar.py:86`; `app/ponto/services/cartorio.py:1622`.

**Leitores adicionais do estado produzido:**

- App e tela usam C6 depois de seleção ampla; `q_vivas` declara continência, não igualdade com C6. `app/chamados/services/pergunta_no_app.py:42`; `app/chamados/views.py:1257`; `app/chamados/models.py:2723`.
- Materializador, validação e reconciliador usam presença/ausência do carimbo para selecionar trabalho ou impedir repetição. `app/chamados/services/materializacao.py:433`; `app/chamados/services/materializacao.py:529`; `app/chamados/services/validacao.py:422`; `app/chamados/reconciliador.py:366`; `app/chamados/reconciliador.py:583`.
- Fábrica lê carimbo e célula; seu corpo atual já consulta `VEREDITOS_ACUSAM`, diferentemente do corpo histórico descrito no anexo. `app/chamados/services/disputa_emissao.py:695`; `app/docs/CORTES.md:64`.
- Histórico gera evento do carimbo; painel agrega seu máximo; lavra usa o instante como marco de atualização; passivo guarda campos para reversão. `app/chamados/services/historico.py:103`; `app/chamados/services/painel.py:67`; `app/chamados/veredito_lavrado.py:163`; `app/chamados/services/passivo_validado.py:64`.
- Fora de chamados: fantasmas selecionam sem carimbo; supra_juiz lê resposta; API e mensageria expõem texto/instante; foto delega resposta e validação às portas. `app/ponto/management/commands/reconciliar_fantasmas.py:212`; `app/ponto/supra_juiz.py:545`; `app/api/views.py:1173`; `app/api/views_mensageria.py:567`; `app/api/foto_recebida.py:48`.
- Auditorias leem carimbo sem testemunha, carimbada ainda viva, ausência de via e validação sem materialização. `app/chamados/management/commands/auditar_invariantes_chamados.py:538`; `app/chamados/management/commands/auditar_invariantes_chamados.py:905`; `app/chamados/management/commands/auditar_invariantes_chamados.py:954`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1073`.

## 2. Juiz ou lei que já responde

| Pergunta | Autoridade existente |
|---|---|
| Esta pergunta está viva? | **C6 `pergunta_viva`**; é quem `carimbar` consulta sobre o candidato aplicado. `app/core/juizes.py:266`; `app/chamados/models.py:2585`. |
| Existe testemunha da materialização? | **`_pergunta_materializada_de_fato`**; também alimenta a lavra. É pergunta distinta da vitalidade, embora C6 a consulte no último degrau. `app/chamados/juizes.py:630`; `app/chamados/juizes.py:1155`; `app/chamados/veredito_lavrado.py:219`. |
| Alguma pergunta da coleção vive? O dia tem porta alcançável? | **C15 `fio_vivo`** e **C17 `porta_alcancavel_do_dia`**, respectivamente. `app/core/juizes.py:275`; `app/core/juizes.py:277`. |
| Chamado vivo significa cobrança viva? | O registro atual separa: **C1 `VIVOS`** responde pelo chamado; C6/C15 respondem pela pergunta/coleção. `app/core/juizes.py:100`; `app/core/juizes.py:261`; `app/core/juizes.py:266`; `app/core/juizes.py:275`. |
| O dia dispensa cobrança? | **C18 `dia_sem_trabalho`**; ausência parcial tem exceção explícita para o resto descoberto. `app/core/juizes.py:278`; `app/chamados/juizes.py:398`; `app/chamados/juizes.py:411`. |
| Qual competência e qual vínculo pertencem ao fato? | **`janela_atual`**, respeitando corte empresarial; **`vinculo_do_dia`**, com precedência da geradora da célula. `app/core/juizes.py:283`; `app/ponto/janelas.py:60`; `app/escala/alimentacao.py:228`. |

O próprio JUIZES.md se declara consulta de endereços e manda prevalecer o registro vivo. Ele também nomeia `validar_pergunta` como autoridade de validação; o código atual mostra que `carimbar` pode preencher os campos de validação em seu modo `validar`. Ambas as referências precisam constar no censo de escrita. `app/docs/JUIZES.md:7`; `app/docs/JUIZES.md:14`; `app/docs/JUIZES.md:38`; `app/chamados/models.py:2580`.

As leis/cortes aplicáveis já escritos são:

- **L-061 e corte estrutural O261:** consumidor lê célula/ata; vida do fio deriva da lâmpada; carimbo não substitui o fato. `app/docs/LEIS.md:95`; `app/docs/PROMPTS.md:1184`.
- **L-007:** escritor canônico e trilha de mudança; **ESMERIL-V2-PROPRIEDADES:** invariantes também nas intercalações responder/validar/recusar/fechar/reabrir/lote. `app/docs/LEIS.md:65`; `app/docs/CORTES.md:333`.
- **L-110:** resultado esperado nasce da regra e cadastro, antes do código. `app/docs/LEIS.md:136`.
- **RELOGIO-INJETADO:** juiz e montador recebem o instante do chamador; a fila registra essa parte ainda separada da metade trava. `app/docs/CORTES.md:370`; `app/docs/RELATO.md:66`.
- **Teto e chegada da batida:** célula provisória não absolve o marco ainda pendente; batida que responde ao marco absolve na passada seguinte. `app/chamados/juizes.py:1127`; `app/docs/CORTES.md:413`.

O mecanismo exato de concorrência autorizado pela abreviação “PEP no carimbar” não está explicitado no aval; o RELATO registra uma pergunta de desenho com duas candidatas **[nao verificado]**. `app/docs/PROMPTS.md:1853`; `app/docs/RELATO.md:66`.

## 3. Contra-exemplos pela regra do aval

| Leitura óbvia que erra | Contra-exemplo e comportamento atual |
|---|---|
| “C6 aprovou; a decisão necessariamente descreve a linha que será gravada.” | A porta julga a instância recebida e salva depois; não relê a linha no corpo. Uma alteração entre coleta e gravação não entra automaticamente nessa fotografia. Ocorrência efetiva em produção **[nao verificado]**. `app/chamados/models.py:2572`; `app/chamados/models.py:2585`; `app/chamados/models.py:2598`. |
| “Carimbada é morta.” | Resposta por validar precede identidade; célula acusadora também pode manter carimbo comum vivo. `app/chamados/juizes.py:1092`; `app/chamados/juizes.py:1113`; `app/chamados/juizes.py:1125`. |
| “O dia acusa, então toda pergunta desse dia vive.” | O marco perguntado pode estar provado aceso enquanto outro marco mantém o dia acusador; o marco é consultado antes do veredito diário. `app/chamados/juizes.py:1115`. |
| “Concorde autoriza carimbar.” | Veredito provisório com marco pendente não absolve sem prova do marco perguntado. A chegada da batida altera essa condição. `app/chamados/juizes.py:1127`; `app/chamados/juizes.py:1136`; `app/docs/CORTES.md:413`. |
| “Célula sem ata equivale a dia resolvido.” | `_celula_furada` devolve desconhecido quando não há lâmpadas lavradas. Entretanto, C6 pode absolver célula com veredito não acusador e ata vazia; existe teste esperando essa morte. A expectativa específica do aval para essa combinação é **[nao verificado]**. `app/chamados/juizes.py:471`; `app/chamados/juizes.py:1137`; `app/chamados/tests/test_o261_pergunta_viva_teto.py:80`. |
| “Virou o dia civil; o fato encerrou.” | O corte exige fato encerrado, não `data < hoje`; marcos pendentes continuam relevantes. `app/docs/PROMPTS.md:1180`; `app/chamados/juizes.py:1127`. |
| “20 e 21 pertencem ao mesmo mês de folha.” | Com corte 21, 20/10 encerra competência 10 e 21/10 inicia competência 11. A tranca consulta a competência de `data_evento` com a empresa. `app/ponto/janelas.py:10`; `app/ponto/janelas.py:24`; `app/chamados/juizes.py:933`. |
| “Resposta 00:00 pertence ao dia da pergunta.” | No 12x36 19–07 com intervalo 23–00, a volta 00:00 pertence a D+1. O plantio usa cadeia de marcos; a prova histórica não pode perder sua data por troca posterior de escala. `app/chamados/juizes.py:283`; `app/chamados/juizes.py:692`. |
| “O vínculo ativo hoje julga o passado.” | A geradora da célula tem precedência; célula lavrada continua julgável após encerramento do vínculo. Alimentação de célula de outro dia/pessoa é recusada. `app/escala/alimentacao.py:228`; `app/chamados/juizes.py:458`; `app/chamados/juizes.py:619`. |
| “Fora da janela, toda pergunta morreu” ou “todo carimbo automático ressuscita.” | C6 consulta célula acusadora antes da janela; já o lastro pode aceitar via sem testemunha fora dela. A janela desse ramo inclui anterior, corrente e véspera, calculadas sem empresa explícita. `app/chamados/juizes.py:1125`; `app/chamados/juizes.py:554`; `app/chamados/juizes.py:645`. |
| “C6 morto pela tranca permite carimbo definitivo.” | A tranca mata por leitura e pode ser reversível; os consumidores E2 e resolução de chamado têm guarda própria para não converter isso em carimbo. `app/chamados/juizes.py:917`; `app/chamados/reconciliador.py:612`; `app/chamados/services/disputa_emissao.py:1587`. |

## 4. Lista de casos RED

Os cadastros abaixo são especificações de ensaio pela regra; ocorrência e execução na produção **[nao verificado]**. A fonte exigida para o resultado esperado é regra e cadastro. `app/docs/LEIS.md:136`.

| Caso | Cadastro + estado | Ato | O que a regra exige |
|---|---|---|---|
| **R1 — decisão humana intercalada** | Mesma pergunta; consumidor conserva instância anterior; admin decide o dia entre coleta e escrita. | Consumidor tenta carimbo automático antigo. | A decisão humana não pode desaparecer sob escrita baseada no estado anterior; mudança conserva trilha. A mesma perda está descrita no caminho inverso. `app/docs/LEIS.md:65`; `app/chamados/services/relavra_via.py:214`. |
| **R2 — resposta nova intercalada** | Pergunta previamente validada; colaborador entrega resposta nova enquanto outro ato conserva a versão anterior. | Ato antigo tenta concluir/carimbar. | Validação anterior não vale para resposta nova; sua voz e a mudança de estado permanecem registradas. `app/chamados/services/disputa_emissao.py:1287`; `app/chamados/services/disputa_emissao.py:1303`; `app/docs/CORTES.md:333`. |
| **R3 — erro depois de C6** | Pergunta sem carimbo, com fotografia anterior dos campos e da lavra. | Lavra ou salvamento lança erro após C6. | Instância não fica declarando um ato que não foi gravado. Existem casos para erro de save, lavra e save após lavra. `app/chamados/tests/test_o261e_f2_fila.py:179`; `app/chamados/tests/test_o261e_f2_fila.py:190`; `app/chamados/tests/test_o261e_f2_fila.py:200`. |
| **R4 — identidade com resposta discordante** | Pergunta respondida, não validada; resposta não concorda com a luz; via duplicata. | Carimbar identidade. | Identidade não apaga a resposta que ainda exige decisão administrativa; a precedência da resposta permanece. `app/chamados/juizes.py:1092`; `app/chamados/models.py:2614`. |
| **R5 — marco respondido, outro furo** | Dia acusador; marco desta pergunta aceso; outro marco apagado. | Encerrar a pergunta do marco aceso. | Não continuar perguntando o fato já testemunhado; o outro furo permanece separado. `app/chamados/models.py:2257`; `app/chamados/juizes.py:1115`. |
| **R6 — borda de instante** | Célula provisória não acusadora, com marco da pergunta pendente e sem batida correspondente. | Tentar carimbar antes do fato encerrado; depois entregar a batida e rejulgar. | Antes, não absolver por veredito provisório; depois, reconhecer a batida na passada seguinte. `app/chamados/juizes.py:1127`; `app/docs/CORTES.md:413`. |
| **R7 — competência 21→20** | Empresa corte 21; fatos em 20/10 e 21/10; competência 10 trancada e 11 aberta. | Consultar tranca/carimbar cada pergunta. | Aplicar competências diferentes; tranca não fabrica quitação do fato nem batida. `app/ponto/janelas.py:10`; `app/ponto/janelas.py:24`; `app/chamados/juizes.py:1080`. 

[truncado em 20000 de 24716 caracteres]

_1890 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
