# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 12:13** · publicado em **13/11/2026 12:15** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 12:13 · sessao 921327d13a14 · - · - · origem=Ronald (tmux)

Todos os caminhos abaixo relativos à raiz da cópia.

**9 — EXISTE a porta de ato; DIVERGE a informação da UI para agendado.**

- Rota `POST <cid>/desligar/`: `app/colaboradores/urls.py:52`; exige login, POST e ação `editar_cadastro`, com gate de disputa isento: `app/colaboradores/views.py:1422`. A view consulta `julgar_desligamento`, devolve erro ou chama `executar_desligamento(..., hoje=timezone.localdate())`: `app/colaboradores/views.py:1433`.
- Juiz por **um colaborador/ato**, devolve dict `erro`, `data_demissao`, `motivo`, `situacao_antes`, `agendado`; recusa já desligado, data inválida, data maior que hoje+90 dias e motivo vazio: `app/colaboradores/services/desligamento.py:15`, `:40`, `:47`, `:50`. Futuro **estritamente > hoje**, até hoje+90 inclusive, vira agendado: `:41`, `:58`.
- Agendado grava apenas data/motivo, evento em `HistoricoVinculo` datado da demissão e observabilidade; retorna `None`, preservando situação/acesso/vínculo/chamados: `app/colaboradores/services/desligamento.py:87`. O histórico usa a porta `registrar_evento_de_vinculo`: `app/colaboradores/services/historico_vinculo.py:20`.
- Efetivo, em transação: grava desligado/data/motivo (`:108`), bloqueia credencial pela porta (`:115`), registra histórico (`:118`), fecha vínculos via `fechar_vinculos_ativos` (`:127`), percorre disputas e chamados (`:144`, `:155`), cria chamado DP/RH `auditoria_clt`, `desligamento_revisao`, com contexto de data/motivo/ator/instante (`:168`), dispara push (`:192`); auditoria geral após o bloco transacional (`:201`). Todos em `app/colaboradores/services/desligamento.py`.
- Não fecha todas as disputas: **pula resposta sem veredito** em `app/colaboradores/services/desligamento.py:145`. As demais passam por `DisputaSupervisao.fechar(..., motivo=FECHOU_DESLIGAMENTO)`: `:147`. A própria porta recusa resposta sem veredito (`app/chamados/models.py:1887`), encerra perguntas mudas por `fechar_por_admin` com trilha (`:1919`) e fecha disputa (`:1933`). O signal pede reconciliação ao chamado pai pela porta: `app/chamados/signals.py:20`, `:43`.
- Chamado com pergunta viva pode recusar `encerrar` e permanecer: `app/colaboradores/services/desligamento.py:160`; guarda efetiva em `app/chamados/models.py:1618`. **O50(d) ainda não está realizado nessa cópia**, apesar do aval raspado: não há fechamento da resposta sem veredito nem lista dia/pergunta/resposta no contexto do novo chamado, que contém apenas data/motivo/ator/instante (`app/colaboradores/services/desligamento.py:145`, `:182`).
- Vínculos: `fechar_vinculos_ativos` só oferece `ativa=True` à porta (`app/colaboradores/services/vinculo.py:855`, `:863`); `fechar_vigencia` fecha somente `data_inicio <= demissao`, inclusive, e devolve `(fechados, recusados_pks)` (`:622`, `:639`, `:641`, `:671`); o wrapper descarta recusados e devolve **número** (`:862`). Futuro fica ativo e logado como recusa (`:665`), portanto **O50(a) ainda falta para futuro** (`:858`). Existe outra porta `absorver_vigencias_posteriores` que desativa futuros sem inventar `data_fim`, mas o executor de desligamento não a chama (`:756`, `:788`, `:799`; `app/colaboradores/services/desligamento.py:127`).
- Fechamento de vigência alcança células pela porta `regenerar_futuras_do_cadastro`, somente após o menor fim antigo/novo (`app/colaboradores/services/vinculo.py:657`). Esse suplemento limita-se a células **já existentes**, `origem='gerada'`, janela padrão, de **amanhã inclusive** em diante; chama `ponto.portas.celula.regenerar_celulas_vinculo` (`app/ponto/services/cadastro_futuro.py:26`, `:31`, `:44`). Não equivale a encerrar todo vínculo futuro nem comprova remoção de toda célula após demissão (`app/colaboradores/services/vinculo.py:858`; efeito total no passivo [nao verificado]).
- Cron usa os mesmos juiz/executor, para `data_demissao <= hoje` inclusive e situação diferente de desligado, DRY por padrão e `--apply` para efetivar: `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:22`, `:38`, `:46`, `:51`, `:57`. Agenda escrita às 06:06 com `--apply`: `app/config/crons.py:440`; execução real [nao verificado].
- **DIVERGE apresentação:** view sempre responde “desligado; acesso bloqueado” (`app/colaboradores/views.py:1438`) e confirmação do formulário sempre diz bloqueio imediato (`app/templates/colaboradores/editar.html:272`), mesmo quando executor só agenda (`app/colaboradores/services/desligamento.py:87`).
- Tela futura deveria reutilizar **`julgar_desligamento` + `executar_desligamento`**, seguindo o precedente view/cron, e seus serviços chamados, em vez de duplicar escrita: `app/colaboradores/views.py:1433`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:51`. A porta atual não é calculadora de verbas: recebe data/motivo e executa cadastro/acesso/vínculo/fio (`app/colaboradores/services/desligamento.py:50`, `:108`).

**10 — EXISTEM superfícies parciais; FALTA dossiê único de liquidação comprovado.**

- Cadastro editar mostra formulário de desligamento; desligado mostra data e motivo, e reativação: `app/templates/colaboradores/editar.html:268`, `:281`, `:285`; rotas editar/desligar/reativar em `app/colaboradores/urls.py:50`.
- Drawer do colaborador exibe badge desligado (`app/templates/colaboradores/partials/painel_colaborador.html:90`) e botão reativar (`:163`); contém Dados/Espelho/Mensagens/Férias, sem quinta aba Ficha nessa cópia (`:50`, `:181`, `:188`, `:217`). Documento pede **quinta aba Ficha**, extrato com filtros e PDF, mas é especificação (`app/docs/ADVERTENCIA-FICHA.md:65`, `:77`, `:84`).
- `ficha_do_colab` existe como dict de um colaborador, com vínculo/trocas/horas/pendências; janela é competência atual da empresa, datas da troca inclusivas (`app/colaboradores/services/ficha.py:99`, `:119`, `:132`, `:233`). Horas vêm de `espelho_do_colab`, com fonte dos totais (`:198`, `:209`), não de dossiê `RegistroPDFAuditavel`. Leitores são cabeçalhos e copiloto (`:265`, `:273`); API `/mensageria/ficha-do-colab/` em `app/api/urls.py:27`.
- Cuidado com homônimo: `chamados.services.historico.historico_do_colab` junta eventos de chamados e ausências **criadas** desde `agora - dias`, não período aquisitivo nem dossiê PDF (`app/chamados/services/historico.py:118`, `:126`, `:129`). API o consome em `app/api/views_mensageria.py:2191`.
- Histórico do drawer de ausência **existe**: `_historico_do_colab` consome `dia_decidido.linha_do_tempo`, resumo e período aquisitivo que contém **hoje**, com fallback para o mais recente já iniciado (`app/ponto/views.py:2007`, `:2014`, `:2022`). Tela/painel o lê em `:2100`; template em `app/templates/ponto/partials/painel_ausencia.html:168`; link PDF histórico em `app/ponto/views.py:2036`, rota `app/ponto/urls.py:73`.
- **F2 VISAO-FALTAS-FERIAS** registrada como visão por período aquisitivo, mesma ficha corrida e faixas art.130/art.131: `app/docs/BACKLOG.md:1285`. Não confundir com “F2” dos comentários do histórico de ausências acima: ali a seleção do PA é por hoje e a linha do tempo não é recortada nesse PA (`app/ponto/views.py:2015`, `:2022`).
- Fechamento mostra badge/filtro Rescisão (`app/templates/ponto/fechamento.html:65`), data na linha (`app/templates/ponto/_fech_linha.html:4`) e rótulo “verbas pela rescisão” (`:11`).
- Folha/exportação avisam que rescisão sai no módulo próprio: `app/templates/folha/index.html:57`, `app/templates/folha/exportar.html:16`; rotas folha em `app/folha/urls.py:7`, `:13`, `:23`.
- Fio do colaborador mostra aviso desligada/motivo e consulta histórica: `app/templates/chamados/fio_colaborador.html:70`.
- Manual ainda instrui “altere para demitido — acesso bloqueado automaticamente”: `app/templates/core/manual.html:117`; não descreve a semântica agendada da porta (`app/colaboradores/services/desligamento.py:87`).

**Arquivos em comum com O50 — dependências demonstradas; arquivo futuro efetivamente editado [nao verificado].**

- `(a)` `app/colaboradores/services/desligamento.py:127`; `app/colaboradores/services/vinculo.py:580`, `:855`; `app/ponto/services/cadastro_futuro.py:9`; `app/ponto/portas/celula.py` chamado em `app/ponto/services/cadastro_futuro.py:22`, `:44`.
- `(b)` leitores crus diretamente presentes no circuito: `app/colaboradores/views.py:1417`, `:2848`; `app/colaboradores/services/desligamento.py:18`; `app/colaboradores/services/vinculo.py:91`, `:863`; `app/colaboradores/services/drawer.py:22`; `app/colaboradores/services/ficha.py:234`; `app/ponto/views.py:2061`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`; templates `app/templates/colaboradores/editar.html:211`, `app/templates/colaboradores/partials/painel_colaborador.html:163`.
- `(c)` existência de arquivos de selo foi listada, mas sem leitura interna [nao verificado]; não afirmar que selo novo/AST/mutante de O50 já está implementado.
- `(d)` `app/colaboradores/services/desligamento.py:144`; `app/chamados/models.py:1606`, `:1827`, `:1856`, `:2499`; `app/chamados/signals.py:20`, `:92`; `app/chamados/services/competencia_trancada.py:119` é o precedente citado pelo aval; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:51` compartilha executor.

**Lacuna de lei possível para consolidar:** modalidade de aviso prévio (trabalhado/indenizado), projeção temporal dos direitos e data financeira da liquidação: porta só aplica teto de 90, compara data com hoje e recebe data/motivo; não calcula aviso por anos de casa ou verbas (`app/colaboradores/services/desligamento.py:9`, `:40`, `:50`, `:87`). Não perguntar como lacuna se quem está no aviso segue ativo até a data: isso já está escrito (`:36`, `:55`; cron `:3`).

## 13/11 12:13 · sessao 85f5295c51c5 · - · - · origem=Ronald (tmux)

Itens 1, 6, 7 e 8 — censo somente leitura.

**1. Espelho 21→demissão: FALTA a combinação “período livre + RegistroPDFAuditavel”; EXISTE emissão livre informacional.**

- A referência `folha/services.py` do anexo não corresponde à cópia: o serviço está em `app/relatorios/services.py:165`. Para `data_ini` e `data_fim` informadas, usa essas datas (`:211-214`) e **não tenta o auditável**: marca `motivo='periodo livre'` (`:235-247`), chama `gerar_pdf_espelho_informacional_bytes` (`:250`), devolvendo `(pdf_bytes|None, pulados, n_ok, info, sem_movimento)` (`:260-264`).
- Universo do lote: colaboradores explícitos, ordenados por nome, ou `colaboradores_do_lote` (`app/relatorios/services.py:217`); esse seletor filtra empresa e situações (`:103-105`), com padrão do lote `ativo/afastado/ferias` (`:102`, `:165`). Não inclui desligado automaticamente; seleção explícita permite fornecê-lo (`:217`).
- A porta livre retorna **bytes de um PDF por colaborador**, sem hash e sem registro (`app/relatorios/pdf_espelho.py:1038-1071`). Dias são filtrados inclusivamente por `data_ini <= data <= data_fim` (`:602`). O coletor aplica teto em ontem, com `max(data_ini, ate_ontem(data_fim))` (`:285-287`); o avulso recusa data final hoje/futura pela porta `recusa_por_data_final` (`app/relatorios/views.py:1087-1098`; `app/relatorios/services.py:13-46`) e diferença entre datas superior a 180 dias (`app/relatorios/views.py:1083-1086`).
- Consumidores: avulso (`app/relatorios/views.py:1065-1098`) e lote (`app/relatorios/services.py:250`); o job chama o serviço do lote com as datas gravadas (`app/relatorios/lote_espelho.py:205-207`). Rotas: `app/relatorios/urls.py:24-34`.
- A porta auditável existe, mas recebe **mes/ano**, não intervalo: `gerar_pdf_espelho_bytes` (`app/relatorios/pdf_espelho.py:1074-1114`), embrulhada por `gerar_pdf_espelho` (`app/relatorios/services.py:49-96`). A competência deriva do corte da empresa, não necessariamente 21 (`app/relatorios/pdf_espelho.py:654-663`; `app/relatorios/cartao_pela_celula.py:21-24`; `app/ponto/janelas.py:36-57`).
- Seus filtros persistidos são mes/ano/tipo_escala/dias_renderizados/turnos, sem datas explícitas (`app/relatorios/pdf_espelho.py:1095-1101`). Bloqueio explícito do corpo: disputa aberta **do colaborador**, não recortada ao período (`:1084-1089`). Não afirmar que exige competência fechada: esse bloqueio não está no corpo atual (`:1074-1114`).
- O corpo atual ainda faz cálculos/leitura viva para emissão (`app/relatorios/pdf_espelho.py:1092-1093`, `:1045-1046`); não é uma porta de leitura de documento histórico congelado.

**6. RegistroPDFAuditavel: EXISTE trilha; FALTA porta de dossiê por colaborador/período e FALTA arquivo no próprio registro.**

- Unidade: uma emissão auditável; guarda hash único, tipo, gerado_por/em, colaborador_id/nome/matrícula, filtros_json, total_registros, IP, commit e hostname (`app/relatorios/models.py:14-65`). **Não possui FileField nem bytes do PDF** nesse modelo (`:30-65`).
- Escritor central de produção: `gerar_pdf_auditavel`, que cria registro e devolve `(bytes, registro)` (`app/relatorios/pdf_base.py:92-122`, `:152-180`).
- Chamadores/escritores indiretos:
  - espelho mensal: `app/relatorios/pdf_espelho.py:1103-1114`;
  - histórico de chamados: `app/relatorios/pdf_chamados.py:158-177`;
  - gestão de férias: `app/relatorios/services.py:338-348`;
  - gestão de HE: `app/ponto/views.py:867-875`;
  - retrato da folha: `app/folha/views_retrato.py:436-442`.
- O espelho mensal pode guardar os **bytes fora do registro**, em `FollowupChamado.anexo_pdf`, com hash denormalizado (`app/relatorios/services.py:75-90`). O download lê esse anexo por **followup_id**, não colaborador/período (`app/relatorios/views.py:1020-1045`).
- Leitor direto encontrado: verificação pública por hash inteiro ou prefixo ≥16 caracteres (`app/chamados/views.py:1185-1222`); retorna registro/autêntico, não busca por colaborador/período (`:1209-1221`).
- Não encontrei porta pronta `colaborador + período → registros/PDFs`: **FALTA**; ausência em toda árvore é resultado de busca, portanto, se exigido rigor absoluto de prova negativa, registrar `[nao verificado]`. O modelo permite filtrar `colaborador_id` e JSON, mas não declara período normalizado nem arquivo (`app/relatorios/models.py:43-56`).
- Escritor operacional fora de produção: mascarador de sombra altera `colaborador_nome` em massa (`bin/sombra_mascara.py:87-88`); regras de raspagem tratam `ip_origem` (`bin/sombra_regras_pessoais.py:73`). Não confundir com emissão produtiva.

**7. Motivo `rescisao_modulo_proprio`: EXISTE; consumidores identificados.**

- Produtor: `classificar_export` emite `status='fora'`, motivo e detalhe de saída pelo módulo de rescisão do Domínio (`app/folha/export.py:866-871`).
- `folha/views.py`: vocabulário compartilhado de tela/PDF (`app/folha/views.py:11-22`); Raio-X separa rescisões (`:594`), exclui do balde “outros” (`:598-600`) e mostra pílula própria (`:641`).
- `folha/retrato_folha.py`: `FORA_DA_CONTA` (`app/folha/retrato_folha.py:33`), `_balde` manda para `modulo_proprio` (`:89-95`), fora do bloqueado; contadores leem essas mesmas linhas (`:114-122`). Também exclui dos eixos dono/tipo/saldo (`:364-365`, `:378-379`, `:414-415`).
- `ponto/services/fechamento_lista.py`: traduz motivo para `rescisao` na linha, preservando motivo cru e detalhe (`app/ponto/services/fechamento_lista.py:25-37`, `:100`).
- `relatorios/views.py`: ranking de furos traduz motivo para “rescisao” e marca `folha=False`, consultando `classificar_export` (`app/relatorios/views.py:1655-1669`).
- `mensageria/nucleo/ferramentas.py`: `NAO_E_BLOQUEIO` (`:1196`), tradução (`:1202`), separa em ressalvas em vez de bloqueios (`:1246-1253`), publica total que trava sem rescisão (`:1266-1268`) e agregado de ressalvas de rescisão (`:1288-1290`).
- `app/core/placar_estrutural.py:331` contém o literal apenas em texto de evidência/descrição, não decide saída.

**8. H12: EXISTE e já usa fim da janela nesta cópia; não repetir como situação atual o backlog “qualquer data”.**

- Autoridade `sai_pela_rescisao(colab, fim_janela)` responde **booleano**: data_demissao não nula **e `<= fim_janela`**, sem exigir `>= início` (`app/folha/export.py:54-60`).
- Consumidores diretos encontrados: `classificar_export`, que obtém janela da empresa (`app/folha/export.py:815`, `:866`); `montar_resumos`, que obtém fim por empresa e preenche `rescisao` (`app/ponto/services/fechamento_lista.py:51`, `:103-109`).
- Consumidor do resultado da linha: contador `n_rescisao` do fechamento (`app/ponto/views.py:1212-1215`); indicação visível/filtrável é documentada no mesmo trecho.
- Universo do classificador: fechamentos fornecidos ou todos `FechamentoMensal` da empresa/mes/ano, por nome (`app/folha/export.py:782-788`); portanto a ausência de limite inferior do predicado não significa que ele busca todos os desligados históricos.
- Existem **outras perguntas** com a palavra “rescisao”, mas não cópias do gate de saída do TXT:
  - `prontidao` chama de rescisão o denominador zero com demissão **antes do início**, estrito `<` (`app/folha/export.py:1051-1071`); fallback recorta fim na demissão e responde rescisão se `fim < ini` (`:1081-1091`). Unidade: prontidão/horas previstas, não elegibilidade do TXT.
  - Raio-X recorta fim na demissão (`app/folha/views.py:609-616`).
  - retenção da célula recorta até demissão, inclusive (`app/folha/export.py:1206-1214`, `:1233`), com exceção especial do dia da demissão apenas para célula ausente/sem veredito (`:1237-1241`, `:1263-1264`). Célula acusando ainda pode reter nesse corpo (`:1241-1262`).
- Essas diferenças de perguntas/universos devem ser explicadas; não classificá-las automaticamente como divergência da mesma regra. A divergência atual entre retenção e a nova LEI-DESLIGADO deve ser consolidada pelo agente responsável pelo item 5, considerando que o corpo auditado não tem isenção global de desligado (`app/folha/export.py:1196-1266`).

## 13/11 12:13 · sessao e6ed0e93ffdb · - · - · origem=Ronald (tmux)

Item 2 — **EXISTE**.

- Modelo: uma linha por dia da entrada do turno + linha `ajuste` sem data para DSR/banco; versões `motor` e `oraculo`; rubricas em horas, abonos/previstos/realizados em minutos. Não contém valor monetário em reais. `app/ponto/models.py:293`, `app/ponto/models.py:303`, `app/ponto/models.py:315`, `app/ponto/models.py:325`, `app/ponto/models.py:349`.
- Escritor normal: `lavrar` substitui linhas por colaborador/competência/versão, apagando e recriando. Chamado no fechamento para motor e, desde 10/2026, oráculo. `app/ponto/services/dia_pago.py:135`, `app/ponto/services/dia_pago.py:359`, `app/ponto/services/fechamento.py:1191`, `app/ponto/services/fechamento.py:1200`, `app/ponto/services/fechamento.py:1265`, `app/ponto/services/fechamento.py:1277`.
- Outras portas escritoras: `retratar_exportada`, retrato dos três campos de grade do fechamento gravado; `apurar_furo_da_exportada`, escreve apenas furo/carimbo nas linhas existentes. `app/ponto/services/dia_pago.py:405`, `app/ponto/services/dia_pago.py:425`, `app/ponto/services/dia_pago.py:538`, `app/ponto/services/dia_pago.py:592`, `app/ponto/services/dia_pago.py:671`.
- Leitura arbitrária até demissão **já existe**, passando demissão como `fim`: `linhas_do_periodo(colab,ini,fim)` → `{date: DiaPago}`, versão padrão motor, tipo dia, datas inclusivas; não recorta automaticamente pela demissão. `app/ponto/services/dia_pago.py:833`, `app/ponto/services/dia_pago.py:855`.
- `soma_do_periodo(colaboradores,ini,fim)` → `(somas,sem_lavratura,sem_apuracao)`, agrupado por colaborador, inclusivo, somente tipo dia. Exclui explicitamente ajuste de DSR/banco; não há rateio parcial nessa porta. `app/ponto/services/dia_pago.py:752`, `app/ponto/services/dia_pago.py:755`, `app/ponto/services/dia_pago.py:784`, `app/ponto/services/dia_pago.py:830`.
- `soma_da_competencia` inclui dia+ajuste, arredonda agregados em duas casas; retorna `None` sem lavratura. `app/ponto/services/dia_pago.py:550`.
- Leitores: espelho e, por ele, PDF (`app/ponto/services/espelho.py:406`, `app/relatorios/pdf_espelho.py:835`); calendário (`app/colaboradores/services/calendario.py:290`); app (`app/api/views.py:1439`); extrato parcial (`app/relatorios/views.py:482`); cartão (`app/relatorios/cartao_pela_celula.py:142`); porta exportação/conferência (`app/folha/porta_export.py:75`, `app/folha/porta_export.py:267`, `app/folha/porta_export.py:332`); gestão HE (`app/ponto/services/gestao_he.py:234`); prévia autorização HE (`app/ponto/services/autorizacao_he_periodo.py:172`); foto de passivo (`app/chamados/services/foto_e4b.py:289`).

Item 3 — **EXISTE**; diferentes universos são explícitos, não uma divergência semântica acidental.

- Inventário de todos lançamentos que tocam período: `ausencias_vigentes(de,ate,colaborador=None)` → QuerySet, sem filtro de status/tipo; sobreposição inclusiva. Ausência sem fim é aberta somente nos `TIPOS_EM_ABERTO`, nos demais tipos é dia único. `app/ponto/turnos.py:3142`, `app/ponto/turnos.py:3158`.
- Efeito financeiro: `ausencias_do_periodo(colab,ini,fim,para='folha')` → todos lançamentos **aprovados** que tocam `[ini,fim]`; `para='cobranca'` → não rejeitados. `app/ponto/turnos.py:2511`, `app/ponto/turnos.py:2523`.
- Cobertura por dia: `cobertura_ausencia_periodo` → `{date:Ausencia}`, recortado inclusivamente; sobreposição vence maior `(data,pk)`. Mesmos filtros de efeito. `app/ponto/turnos.py:2735`, `app/ponto/turnos.py:2753`, `app/ponto/turnos.py:2765`.
- Decisões por dia, inclusive rejeitadas: `decididos_do_periodo` → `{date:cartao_do_dia}`, sobreposição inclusiva, maior `(data,pk)`; não equivale a “cobertura financeira”. `app/ponto/services/dia_decidido.py:529`, `app/ponto/services/dia_decidido.py:550`.
- Histórico: `linha_do_tempo` → lista de lançamentos raiz, todos status, sem recorte temporal nativo; leitor em lote aplica sobreposição inclusiva depois da montagem. `app/ponto/services/dia_decidido.py:566`, `app/ponto/services/dia_decidido.py:587`, `app/ponto/services/atestados_lote.py:46`.
- Mesa DP: `ausencias_do_dp` exclui férias e rejeitadas, enquanto lista de ausências permite ambos. Diferença de universo está escrita. `app/ponto/turnos.py:3115`, `app/ponto/turnos.py:3138`, `app/ponto/views.py:2178`.
- Leitores: tela ausências (`app/ponto/views.py:2178`); copiloto/API mensageria (`app/api/views_mensageria.py:703`, `app/api/views_mensageria.py:1032`); fechamento (`app/ponto/services/fechamento.py:638`, `app/ponto/services/fechamento.py:680`); efeito ausência (`app/ponto/services/efeito_ausencia.py:28`); espelho (`app/ponto/services/espelho.py:579`); calendário (`app/colaboradores/services/calendario.py:531`); escala (`app/escala/utils.py:1174`); histórico drawer F2 e lote F3 (`app/ponto/views.py:2014`, `app/ponto/services/atestados_lote.py:54`).
- Histórico anual médico é outro universo: tipos médicos do catálogo, aprovadas, início no ano; conta dias ocupados entre 1º/1 e `min(31/12,hoje)`, inclusivo. `app/ponto/views.py:2837`, `app/ponto/views.py:2870`.

Item 4 — **DIVERGE** para cálculo/bordas do ciclo; saldo/vencimento **EXISTEM**; proporcionais rescisórias **FALTAM** na busca da cópia [nao verificado].

- Modelo armazena período aquisitivo por colaborador: início, fim, limite concessão, direito, concedido, vendido. Unidade: dias. `app/ferias/models.py:105`, `app/ferias/models.py:116`.
- Cálculo de ciclo existe na view: ancora admissão; `[aniversário,aniversário seguinte−1 dia]`; limite=fim+um ano; cria até ciclo vigente em **hoje**, não até demissão. Trata 29/2 com fallback 28/2. Escrita idempotente por `garantir_periodo_aquisitivo`. `app/ferias/views.py:139`, `app/ferias/views.py:145`, `app/ferias/views.py:161`, `app/ferias/views.py:180`, `app/ferias/services.py:254`.
- **Segundo sítio diverge:** edição calcula `fim=inicio+364 dias`, `limite=fim+365 dias`, em vez de aniversário/ano-calendário. Pode mudar borda em ano bissexto. `app/ferias/services.py:523`.
- Saldo = direito−concedido−vendido, sem proporcionalidade temporal. `app/ferias/models.py:216`.
- `status_de_periodo(pa,hoje)` é puro: saldo≤0 → concedido; fim≥referência → em_curso; limite<referência → vencido; demais adquirido. Último dia aquisitivo ainda em curso; dia limite concessão ainda não vencido. Aceita referência demissão, mas as telas passam hoje. `app/ferias/models.py:65`, `app/ferias/models.py:94`, `app/ferias/views.py:109`, `app/ferias/views.py:767`.
- Faltas do aquisitivo: conta registros `tipo='falta'`, aprovados, cujo início está inclusivamente no período; não expande range nessa property. Direito por faixas 30/24/18/12/0; `aplicar_art130` escreve direito. `app/ferias/models.py:52`, `app/ferias/models.py:141`, `app/ferias/services.py:243`.
- Outros sítios de leitura usam estado materializado: `periodos_vencendo` filtra status cru adquirido e limite≤hoje+dias; PDF gestão filtra status cru vencido/adquirido. Inteligência consulta juiz puro, logo podem responder diferente se o estado armazenado divergir. `app/ferias/services.py:111`, `app/relatorios/services.py:285`, `app/inteligencia/views.py:13`.
- Leitores de saldo/períodos: saldo e seção férias (`app/ferias/views.py:110`, `app/ferias/views.py:767`); situação férias (`app/ferias/situacao.py:138`, `app/ferias/situacao.py:162`); inteligência (`app/inteligencia/views.py:13`); relatório gestão (`app/relatorios/services.py:268`); drawer F2 lê só período contendo hoje ou último iniciado, rótulo armazenado (`app/ponto/views.py:2007`); alertas cron sincronizam e leem saldo (`app/ferias/management/commands/alertar_ferias_vencendo.py:22`).
- Nenhuma fórmula de avos, férias proporcionais ou indenização rescisória foi localizada na busca produção da cópia [nao verificado]. Saldo integral do aquisitivo não prova direito proporcional [nao verificado].

Arquivos em comum com O50(b), evidência de leitura crua nestas áreas:

- `app/ponto/turnos.py:2464`, `app/ponto/turnos.py:2483`: escala `ativa=True`.
- `app/ponto/services/fechamento.py:432`, `app/ponto/services/fechamento.py:440`: data demissão/situação.
- `app/colaboradores/services/calendario.py:258`: escala ativa.
- `app/relatorios/views.py:455`, `app/relatorios/views.py:492`, `app/relatorios/views.py:833`: situação/escala ativa.
- `app/relatorios/services.py:105`: situação.
- `app/api/views.py:116`, `app/api/views.py:312`, `app/api/views.py:403`, `app/api/views.py:905`: situação/escala ativa.
- `app/ferias/views.py:605`: exclui desligado.
- `app/ferias/situacao.py:172`: situação ativo.
- Inclusão efetiva de cada arquivo em construção futura não está comprovada [nao verificado].

Lacunas candidatas a perguntas de lei, sem proposta:

1. Férias proporcionais na data de desligamento: avos, fração e reflexo do aviso; saldo atual é integral, fórmula proporcional não localizada. `app/ferias/models.py:216`; [nao verificado].
2. Destino/liquidação de DSR e banco da competência parcial de rescisão: porta de intervalo deliberadamente os exclui e não rateia. `app/ponto/services/dia_pago.py:755`.
3. Valor de liquidação das férias adquiridas/vencidas: código classifica dias/status; cálculo monetário de rescisão não localizado. `app/ferias/models.py:65`, `app/ferias/models.py:216`; [nao verificado].

## 13/11 12:10 · sessao f68b07bfd9af · DESLIGAR-LIQUIDA-TUDO · censo · origem=Code

Censo da cópia, sem alterações. Correspondência da cópia ao HEAD/hash informado: [nao verificado]. O material fornecido não traz linhas nem contagens da frota de produção; passivo, distribuição por competência e contador atual: [nao verificado].

**1. Escritores de `PerguntaDisputa.via_resolucao`**

| Sítio | Escrita |
|---|---|
| `app/chamados/models.py:2563` | Default `''` no nascimento. |
| `app/chamados/models.py:3015`, `app/chamados/models.py:3018` | `_nascer` insere com `**campos`; pode receber a via por esse argumento. Caller de produção fornecendo via assim: [nao verificado]. |
| `app/chamados/models.py:3102` | `_escrever_carimbo` atribui a via recebida em memória, juntamente com materialização e autor. |
| `app/chamados/models.py:3130`, `app/chamados/models.py:3153` | `carimbar` persiste a via recebida se o C6 considerar a pergunta morta. Identidade/tranca geram erro; pergunta ainda viva desfaz a tentativa: `app/chamados/models.py:3119`, `app/chamados/models.py:3144`. |
| `app/chamados/models.py:3163` | `_desfazer_em_memoria` restaura a via anterior após tentativa recusada/falha; não persiste por si. |
| `app/chamados/models.py:3184`, `app/chamados/models.py:3186`, `app/chamados/models.py:3191` | `carimbar_identidade` persiste a via recebida, restrita a `VIAS_DE_IDENTIDADE`; preserva o carimbo anterior quando solicitado. |
| `app/chamados/models.py:3209`, `app/chamados/models.py:3213` | `descarimbar` grava `''`, limpando também materialização e autor. |
| `app/chamados/models.py:3265` | `descarimbar_em_lote` executa `UPDATE` para `via_resolucao=''`; poupa `duplicata` e `motivo_retratado`. |
| `app/chamados/models.py:3276` | `encerrar_por_fato_no_espelho` grava diretamente `fato_materializado`. |
| `app/chamados/services/relavra_via.py:125`, `app/chamados/services/relavra_via.py:127` | `relavrar` troca a via pela calculada por `via_de_hoje`, somente quando a anterior é `declaracao` ou `completude_turno`. |

Cada chamador que determina a escrita:

| Sítio | Via gravada pela porta |
|---|---|
| `app/chamados/models.py:3065` | `fechar_por_admin` → `admin_decidiu`. |
| `app/chamados/models.py:1922` | Fechamento da disputa por desligamento/admin → `fechar_por_admin` → `admin_decidiu`. |
| `app/chamados/services/validacao.py:289` | Aceite quando o dia já é folga → `admin_decidiu`. |
| `app/chamados/services/validacao.py:329` | Aceite que declara a folga → `admin_decidiu`. |
| `app/ponto/services/veredito_celula.py:190` | Decisão do dia → `admin_decidiu`. |
| `app/chamados/models.py:2062` | Remoção do motivo → `motivo_retratado`. |
| `app/chamados/services/fio.py:1360` | Retratação da duplicata → `duplicata`. |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:331` | Retratação pelo backfill → porta `retratar_para_canonica` → `duplicata`; delegação em `app/chamados/services/fio.py:1342`. |
| `app/chamados/services/materializacao.py:930` | Recusa por paridade → `paridade_recusada`. |
| `app/chamados/services/materializacao.py:1105` | Colisão com fato existente → `fato_ocupado`. |
| `app/chamados/reconciliador.py:406` | Reconciliação sem turno aberto → `sem_turno_aberto`. |
| `app/chamados/reconciliador.py:596` | Absolvição pela célula → `celula_absolveu`. |
| `app/chamados/models.py:3288` | `encerrar_fantasma` → `fantasma`; chamado por `app/chamados/reconciliador.py:1148`. |
| `app/chamados/services/materializacao.py:555`, `app/chamados/services/materializacao.py:567` | Absorção pela ausência → `declaracao` se declarou hora; senão `ausencia_dia`. |
| `app/chamados/services/materializacao.py:1283` | Plantio → `declaracao` se declarou hora; senão `via_pelo_fato`, ou `chamado_encerrado` sem destino: `app/chamados/services/materializacao.py:1271`, `app/chamados/services/materializacao.py:1274`. |
| `app/chamados/services/materializacao.py:1437` | Materialização já comprovada pelo juiz → `juiz_aprovou`. |
| `app/chamados/services/disputa_emissao.py:1986` | Resolução pelo chamado → resultado de `via_pelo_fato`. |
| `app/chamados/services/disputa_emissao.py:2058`, `app/chamados/services/disputa_emissao.py:2060` | Escolha sem credor → `via_pelo_fato` do pai, ou `chamado_encerrado` sem pai. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:68` | Fato comprovado no espelho → `fato_materializado`. |
| `app/chamados/models.py:1507` | Simulação da ressuscitação → `''` **somente na cópia em memória**. |
| `app/chamados/models.py:1567` | Ressuscitação → `''` em lote. |
| `app/chamados/services/disputa_emissao.py:1485` | Nova resposta reabre carimbo sem lastro → `''`, restrito pelas vias em `app/chamados/models.py:3314`. |
| `app/chamados/services/disputa_emissao.py:1630` | Reabertura para correção → `''` em lote. |
| `app/chamados/services/relavra_via.py:246` | Desfaz carimbo que o C6 considera vivo → `''`. |
| `app/chamados/models.py:3299` | Reabre `fantasma` → `''`; caller em `app/chamados/management/commands/reabrir_orfas_enterradas.py:157`. |
| `app/chamados/models.py:3323` | Reabre `competencia_trancada` → `''`; caller em `app/chamados/services/competencia_trancada.py:279`. |

Os retornos possíveis de `via_pelo_fato` são, individualmente: `cadastro_folga` em `app/chamados/juizes.py:1749`; `adesao` em `app/chamados/juizes.py:1750`; `ausencia_dia` em `app/chamados/juizes.py:1758`; `completude_turno` em `app/chamados/juizes.py:1766`; `chamado_encerrado` em `app/chamados/juizes.py:1767`.

O antigo escritor de `competencia_trancada` foi removido; a porta atual `carimbar_identidade` não aceita essa via: `app/chamados/models.py:3067`, `app/chamados/models.py:3179`.

**2. Leitores de `via_resolucao`: ramo conhecido e valor novo**

| Leitor | Comportamento atual; valor desconhecido |
|---|---|
| `app/chamados/juizes.py:679` — `_pergunta_materializada_de_fato` | Vias em `VIAS_SEM_TESTEMUNHA`, com materialização, recebem tratamento próprio. Identidade+tranca retornam verdadeiro em `app/chamados/juizes.py:691`; as demais consultam janela/lei. Desconhecida passa aos ramos por tipo: escolha aceita o carimbo; hora/data-hora procuram prova da batida; texto procura completude: `app/chamados/juizes.py:706`, `app/chamados/juizes.py:708`, `app/chamados/juizes.py:816`. |
| `app/chamados/juizes.py:853` — memo do C6 | Inclui a via na chave. Qualquer valor é dado da chave; não classifica nem rejeita. |
| `app/chamados/juizes.py:1386` — projeção temporal | Materialização posterior ao instante consultado é desconsiderada; a via também é apagada nessa projeção, exceto `duplicata`/`motivo_retratado`: `app/chamados/juizes.py:1394`. Nova via seria apagada nessa condição. |
| `app/chamados/juizes.py:1412` — C6, antes da resposta | `competencia_trancada`, `duplicata`, `motivo_retratado`, com materialização, matam antes de ler a resposta. Desconhecida não entra. |
| `app/chamados/juizes.py:1417` — C6, resposta | Resposta carimbada com texto mantém a pergunta viva, salvo decisão admin na célula ou concordância comprovada com a luz. Desconhecida não vence esse ramo: `app/chamados/juizes.py:1425`, `app/chamados/juizes.py:1429`. |
| `app/chamados/juizes.py:1437` — C6, identidade | Identidade+tranca materializadas matam. `admin_decidiu` entra aqui **depois** do ramo da resposta. Desconhecida segue para marco/célula/lei/lastro: `app/chamados/juizes.py:1444`, `app/chamados/juizes.py:1450`, `app/chamados/juizes.py:1476`, `app/chamados/juizes.py:1483`. |
| `app/chamados/models.py:3056` — `fechar_por_admin` | Apenas `admin_decidiu` já materializada é no-op. Outra via pode ser sobrescrita por `admin_decidiu`. |
| `app/chamados/models.py:3119` — `carimbar` | Identidade+tranca geram `ValueError`; desconhecida é submetida ao C6. |
| `app/chamados/models.py:3179` — `carimbar_identidade` | Qualquer via fora das cinco identidades gera `ValueError`; inclui tranca e nova desconhecida. |
| `app/chamados/models.py:3181` — guarda de terminalidade | Linha fora do fato não recebe via que a devolva ao fato. |
| `app/chamados/models.py:3202` — `fora_do_fato` | Verdadeiro somente para `duplicata`/`motivo_retratado`; desconhecida retorna falso. |
| `app/chamados/models.py:3256` — descarimbo em lote | Proíbe passar `via_resolucao` em `**outros`, qualquer que seja o valor. |
| `app/chamados/models.py:3266` — filtro do descarimbo | Poupa apenas fora-do-fato; nova via pode ser apagada se selecionada pelo chamador. |
| `app/chamados/models.py:3274` — encerrar pelo fato | `fato_materializado` ou fora-do-fato são no-op; outra via pode ser sobrescrita. |
| `app/chamados/models.py:3285` — encerrar fantasma | Só `fantasma` já materializada é no-op; outra via passa pela tentativa de carimbar fantasma/C6. |
| `app/chamados/models.py:3297` — reabrir fantasma | Só toca `fantasma`; nova ignorada. |
| `app/chamados/models.py:3314` — reabrir por resposta | Só toca `chamado_encerrado`/`legado_sem_lastro`; nova ignorada. |
| `app/chamados/models.py:3321` — reabrir por competência | Só toca `competencia_trancada`; nova ignorada. |
| `app/chamados/models.py:3351` — `q_vivas` | Identidade+tranca entram no teste de identidade; somente `VIAS_QUE_VENCEM_A_RESPOSTA` vencem resposta: `app/chamados/models.py:3353`. Desconhecida pode continuar no pré-filtro amplo: `app/chamados/models.py:3360`. |
| `app/chamados/services/relavra_via.py:118` — re-lavra | Só reescreve `declaracao`/`completude_turno`; desconhecida ignorada. |
| `app/chamados/services/relavra_via.py:155` — seleção por dia | Seleciona somente essas duas vias; desconhecida ignorada. |
| `app/chamados/services/relavra_via.py:181` — carimbo desfazível | Protege `VIAS_DE_IDENTIDADE`; tranca/desconhecida podem ser candidatas. A decisão de desfazer exige depois C6 vivo: `app/chamados/services/relavra_via.py:242`. |
| `app/chamados/services/materializacao.py:444` — estado de aplicação | Para pergunta de hora com texto e materialização, `paridade_recusada` vira `recusada_paridade`; **qualquer outra via, incluindo admin, tranca e desconhecida, vira `aplicada`**: `app/chamados/services/materializacao.py:448`. |
| `app/chamados/services/materializacao.py:632` — absorvidas por ausência | Só inclui `ausencia_dia`/`declaracao`; desconhecida ignorada. |
| `app/chamados/services/passivo_validado.py:141` — balde da recusa | Qualquer via não vazia que mudou vira `via:<valor>`, inclusive desconhecida. |
| `app/chamados/services/fio.py:1244` — mérito perdedor | Apenas `admin_decidiu` com resposta recebe proteção de mérito contra retratação. Tranca/desconhecida não recebem. |
| `app/chamados/services/fio.py:1351` — retratar duplicata | Só `duplicata` é no-op; desconhecida pode seguir à retratação, sujeita às guardas da porta. |
| `app/chamados/services/fabrica_da_celula.py:149` | Exclui apenas `duplicata`; desconhecida permanece candidata. |
| `app/chamados/services/tarefa_do_dia.py:230` | Exclui fora-do-fato, com exceção temporal da retratação. Desconhecida continua como pergunta do fato; decisões e vida C6 são lidas depois: `app/chamados/services/tarefa_do_dia.py:234`, `app/chamados/services/tarefa_do_dia.py:245`, `app/chamados/services/tarefa_do_dia.py:306`. |
| `app/ponto/services/cartorio.py:1650` | Ramo especial exige que todas sejam `duplicata`; desconhecida impede esse ramo. |
| `app/ponto/services/flip_auto.py:129` | Fila de inversão seleciona apenas `paridade_recusada`; desconhecida ignorada. |
| `app/chamados/services/competencia_trancada.py:276` | Reabertura seleciona apenas `competencia_trancada`; desconhecida ignorada. |
| `app/chamados/models.py:2056` | Remover motivo poupa quem já é `motivo_retratado`; desconhecida não é poupada por esse teste. |
| `app/chamados/models.py:3222` | Mudança no campo integra os campos que acordam a célula, independentemente do valor. |

Em cada sítio abaixo, o teste exclui **somente `duplicata`/`motivo_retratado`**. Admin, tranca e uma via nova permanecem no universo; esse teste não aceita nem recusa a resposta: [nao verificado]

| Sítio | Universo afetado |
|---|---|
| `app/chamados/models.py:1489` | Candidatas à ressuscitação do chamado. |
| `app/chamados/models.py:2725` | `do_fato`, ramo de episódio. |
| `app/chamados/models.py:2756` | `q_dos_fatos`/`linhas_dos_fatos`. |
| `app/chamados/models.py:3486` | Índice único por marco. |
| `app/chamados/models.py:3492` | Índice único por episódio. |
| `app/chamados/services/disputa_emissao.py:328` | Dedup de motivo sem data. |
| `app/chamados/services/disputa_emissao.py:400` | Dedup de motivo datado. |
| `app/chamados/services/disputa_emissao.py:419` | Dedup de furo por dia. |
| `app/chamados/services/disputa_emissao.py:437` | Outro ramo de dedup de furo por dia. |
| `app/chamados/services/fio.py:1101` | Contador de reuso sem rechaveamento. |
| `app/chamados/services/fio.py:1168` | Contador de saída sem dia. |
| `app/chamados/services/fio.py:1434` | Universo `_no_fato`. |
| `app/chamados/services/fio.py:1854` | Contador de fecho cruzando dias. |
| `app/chamados/services/fio.py:1890` | Contador de pergunta sobre batida retratada. |
| `app/chamados/services/fio.py:1912` | Universo do grupo. |
| `app/chamados/services/fio.py:1957` | Acervo considerado nas colisões. |

Apresentação, auditoria e comandos:

| Sítio | Comportamento; desconhecida |
|---|---|
| `app/templates/chamados/partials/_card_atendimento.html:186` | Frases próprias para `chamado_encerrado`, `declaracao`, `completude_turno`, `sem_turno_aberto`, `admin_decidiu`, `adesao`, `fato_ocupado`, `paridade_recusada`, `duplicata`. Demais vias não vazias usam `get_via_resolucao_display`; tranca recebe seu choice. Resultado desse método para valor sem choice na instalação: [nao verificado]. |
| `app/chamados/services/explicador.py:93` | Se o juiz de lastro aprova, mostra resolvida/“Nada a fazer”, com display da via em `app/chamados/services/explicador.py:95`; desconhecida segue o juiz, sem ramo próprio. |
| `app/chamados/models.py:1921`, `app/chamados/models.py:1930` | Via anterior na trilha do fechamento; qualquer valor é transportado. |
| `app/chamados/models.py:2061` | Via anterior no texto da retratação; qualquer valor é transportado. |
| `app/chamados/reconciliador.py:1144`, `app/chamados/reconciliador.py:1155` | Foto antes/depois do fantasma; qualquer valor é transportado. |
| `app/chamados/services/disputa_emissao.py:2050`, `app/chamados/services/disputa_emissao.py:2065`, `app/chamados/services/disputa_emissao.py:2068` | Foto e trilha do fecho sem credor; sem classificação adicional. |
| `app/chamados/services/materializacao.py:674` | Foto reversível da absorção; qualquer valor é transportado. |
| `app/chamados/services/passivo_validado.py:64`, `app/chamados/services/passivo_validado.py:83` | Foto reversível; qualquer valor é transportado. |
| `app/chamados/services/passivo_validado.py:521` | Guarda via anterior para comparar o resultado. |
| `app/chamados/services/relavra_via.py:123`, `app/chamados/services/relavra_via.py:136` | Trilha da re-lavra; transporta os valores anterior/novo. |
| `app/chamados/services/relavra_via.py:245`, `app/chamados/services/relavra_via.py:252` | Trilha do descarimbo; transporta a via anterior. |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:101` | Seleciona `admin_decidiu` materializada sem validação para reconstruir atos; tranca/nova ignoradas. |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:314` | Repete a guarda de `admin_decidiu` sob trava; nova ignorada. |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:330`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:335` | Foto/trilha anterior à retratação. |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:398`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:404` | Diff conserva qualquer via nas intocadas e projeta `duplicata` nas retratadas. |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:504` | Carrega a via para o plano de retratação; classificação delegada às portas citadas. |
| `app/chamados/management/commands/relavrar_via_i6.py:58` | Fora de `declaracao`/`completude_turno`, imprime “via de DECISAO, nao se re-lavra”. Nova recebe esse rótulo, mesmo sem representar decisão. |
| `app/chamados/management/commands/relavrar_via_i6.py:88` | Conta pares de vias; admite qualquer valor. |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py:67` | Seleciona apenas `fantasma`; nova ignorada. |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py:129`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:142`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:160` | Snapshot/trilha da via. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:33` | Exclui `fato_materializado`; nova pode entrar na consulta inicial. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:40` | Só sobrescreve `''`, `legado_sem_lastro`, `None`; nova fica intocada. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:49`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:66`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:75` | Contagem e trilha das vias. |
| `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:70` | Sem batida fonte, `ausencia_dia`/`declaracao` recusam como absorção; outra via recusa como `sem_fonte`: `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:72`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:48` | Piso I6 deriva de `VIAS_SEM_TESTEMUNHA ∪ VIAS_SEM_LASTRO`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:472` | Se uma desconhecida alcançar `_i6_pares`, fica fora do piso e cai em violação: `app/chamados/management/commands/auditar_invariantes_chamados.py:483`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:202` | Contraprova F1t exige via preenchida e pergunta morta; nova morta recebe “carimbo/celula” se não validada/não identidade: `app/chamados/management/commands/auditar_invariantes_chamados.py:205`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:433` | Agrupa contagem por qualquer via. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:542` | Devolve via com PK do candidato I6. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:955` | I9 acusa materializada com via vazia; nova não vazia passa nesse teste. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:1710` | Piso F1t admite via preenchida se C6 declarar morta, inclusive nova reconhecida pelo juiz. |

Enumerações fechadas encontradas:

| Enumeração | Conteúdo/efeito relevante |
|---|---|
| `app/chamados/models.py:2394` — `VIAS_FORA_DO_FATO` | `duplicata`, `motivo_retratado`; altera identidade, dedup e índices. |
| `app/chamados/models.py:2511` — `VIAS_DE_IDENTIDADE` | `admin_decidiu`, `duplicata`, `fato_ocupado`, `paridade_recusada`, `motivo_retratado`; controla a porta de identidade. |
| `app/chamados/models.py:2525` — `VIAS_QUE_VENCEM_A_RESPOSTA` | Tranca + fora-do-fato; fonte do ramo anterior à resposta no C6 e SQL. |
| `app/chamados/models.py:2526` — `VIAS` | Choices do campo; inclui tranca e admin. Campo limitado a 24 caracteres: `app/chamados/models.py:2563`. |
| `app/chamados/models.py:3096` — `vias_que_encerram_por_identidade` | Identidades + tranca; alimenta C6, SQL e `_ABSOLVEM_SEMPRE`. |
| `app/chamados/models.py:3733` — `VIAS_SEM_TESTEMUNHA` | `completude_turno`, `fantasma`, `sem_turno_aberto`, `juiz_aprovou`, `admin_decidiu`, `paridade_recusada`, `duplicata`, `fato_ocupado`, `adesao`, `cadastro_folga`, `celula_absolveu`, `ausencia_dia`, `competencia_trancada`, `motivo_retratado`. |
| `app/chamados/models.py:3769` — `VIAS_SEM_LASTRO` | `chamado_encerrado`, `legado_sem_lastro`; permite reabrir por resposta. |
| `app/chamados/services/relavra_via.py:77` — `VIAS_DERIVADAS` | `declaracao`, `completude_turno`; permite reescrever a via. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:40` — `SEM_HISTORIA` | 

[truncado em 20000 de 43754 caracteres]

## 13/11 12:05 · sessao f2a5216fc256 · - · - · origem=Ronald (tmux)

Censo de `via_resolucao`, somente leitura. Os caminhos abaixo são relativos à raiz da cópia.

**Escritores físicos e portas**

| Sítio | Valor gravado / comportamento |
|---|---|
| `app/chamados/models.py:3015`–`:3018` | `_nascer` insere a pergunta; o campo nasce `''` pelo default de `:2563`, salvo valor recebido via `**campos`. Chamador de produção passando uma via por esse `**campos`: [nao verificado]. |
| `app/chamados/models.py:3098`–`:3102` | `_escrever_carimbo` atribui `materializada_em`, autor e a via recebida; quem persiste é a porta. |
| `app/chamados/models.py:3104`, `:3130`, `:3153` | `carimbar(via, ...)` salva a via recebida somente se o C6 disser que a pergunta fica morta; se viva, desfaz tudo e retorna `False` (`:3144`). Recusa vias de identidade/tranca com `ValueError` (`:3119`). Via desconhecida pode ser tentada, mas passa pelo C6. |
| `app/chamados/models.py:3169`, `:3179`, `:3184`, `:3191` | `carimbar_identidade` grava a via recebida, mas somente as cinco `VIAS_DE_IDENTIDADE`; desconhecida e `competencia_trancada` geram `ValueError`. |
| `app/chamados/models.py:3204`–`:3213` | `descarimbar` grava `''` e limpa materialização/autor; não descarimba `duplicata`/`motivo_retratado`. |
| `app/chamados/models.py:3253`–`:3267` | `descarimbar_em_lote` faz `UPDATE` de via para `''`, com os dois outros campos; exclui `VIAS_FORA_DO_FATO`. |
| `app/chamados/models.py:3269`–`:3277` | `encerrar_por_fato_no_espelho` grava diretamente `fato_materializado`; recusa no-op se já nessa via ou fora do fato. |
| `app/chamados/services/relavra_via.py:118`–`:128` | `relavrar` reescreve somente vias originais `declaracao`/`completude_turno`, para `via_de_hoje`; salva também o veredito lavrado. Destinos: `declaracao` em `:98`, ou os destinos de `via_pelo_fato` em `:103`; conversa/sem destino não são tocados (`:101`). |

**Cada chamador de escrita encontrado**

| Sítio | Via |
|---|---|
| `app/chamados/models.py:3051`–`:3065` | `fechar_por_admin` → `admin_decidiu`. |
| `app/chamados/models.py:1922` | `DisputaSupervisao.fechar` chama `fechar_por_admin` nas perguntas selecionadas; trilha declara `admin_decidiu` em `:1931`. |
| `app/chamados/services/validacao.py:289` | `fechar_por_admin` → `admin_decidiu`. |
| `app/chamados/services/validacao.py:329` | Outro ramo de `validar_pergunta` chama `fechar_por_admin` → `admin_decidiu`. |
| `app/ponto/services/veredito_celula.py:190` | `fechar_por_admin` → `admin_decidiu`. |
| `app/chamados/models.py:2062` | `remover_motivo` → `motivo_retratado`. |
| `app/chamados/services/fio.py:1360` | `retratar_duplicata` → `duplicata`, preservando materialização anterior quando existente. |
| `app/chamados/services/materializacao.py:930` | Guarda de paridade → `paridade_recusada`. |
| `app/chamados/services/materializacao.py:1105` | `_carimbar_recusa`, colisão → `fato_ocupado`. |
| `app/chamados/reconciliador.py:406` | `sem_turno_aberto`. |
| `app/chamados/reconciliador.py:596` | `celula_absolveu`. |
| `app/chamados/models.py:3288` | `encerrar_fantasma` → `fantasma`; chamado no reconciliador em `app/chamados/reconciliador.py:1148`. |
| `app/chamados/services/materializacao.py:555`, `:567` | Absorção por ausência → `declaracao` se `declarou_hora`, senão `ausencia_dia`. |
| `app/chamados/services/materializacao.py:1283` | Materialização → `declaracao` se declarou hora; senão `via_pelo_fato` (`:1271`) ou `chamado_encerrado` sem destino (`:1274`). |
| `app/chamados/services/materializacao.py:1437` | `juiz_aprovou`. |
| `app/chamados/services/disputa_emissao.py:1986` | `via_pelo_fato(chamado, dia, motivo)`. |
| `app/chamados/services/disputa_emissao.py:2058`–`:2060` | Escolha sem credor → `via_pelo_fato(pai, ...)` ou `chamado_encerrado` sem pai. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:68` | `encerrar_por_fato_no_espelho` → `fato_materializado`. |
| `app/chamados/models.py:1507` | `perguntas_corrigiveis` limpa via **em cópia em memória**, sem persistir. |
| `app/chamados/models.py:1567` | `ressuscitar_perguntas` → `''` em lote, junto com limpeza da resposta/validação por carimbo. |
| `app/chamados/services/disputa_emissao.py:1485` | `reabrir_por_resposta` → `''`, somente vias `chamado_encerrado`/`legado_sem_lastro` carimbadas (`models.py:3314`). |
| `app/chamados/services/disputa_emissao.py:1630` | `reabrir_para_correcao` → `''` em lote. |
| `app/chamados/services/relavra_via.py:246` | `desfazer_carimbo_sem_lastro` → `''`, após C6 declarar viva (`:242`). |
| `app/chamados/models.py:3299` | `reabrir_fantasma` → `''`; chamado em `app/chamados/management/commands/reabrir_orfas_enterradas.py:157`. |
| `app/chamados/models.py:3323` | `reabrir_por_competencia` → `''`; chamado em `app/chamados/services/competencia_trancada.py:279`. |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:331` | Chama `retratar_para_canonica`, que chama a porta de `duplicata` em `app/chamados/services/fio.py:1342`. |

`via_pelo_fato` pode devolver `cadastro_folga`, `adesao`, `ausencia_dia`, `completude_turno` ou `chamado_encerrado`; cada retorno está em `app/chamados/juizes.py:1749`, `:1750`, `:1758`, `:1766`, `:1767`. O escritor antigo de `competencia_trancada` foi removido; a lápide registra isso em `app/chamados/models.py:3067`–`:3072`.

**Leitores decisórios: comportamento das vias e de valor desconhecido**

| Leitor | Valores conhecidos e valor novo |
|---|---|
| `app/chamados/juizes.py:679` | `_pergunta_materializada_de_fato`: somente `VIAS_SEM_TESTEMUNHA` + materialização entram no atalho. Dentro dele, identidade+tranca absolvem sempre (`:689`–`:692`); demais vias sem testemunha consultam lei/janela (`:693`–`:705`). Desconhecida fica fora e passa pelos ramos de tipo de resposta: escolha aceita materialização (`:706`), hora exige batida (`:708`), demais exigem testemunha de completude conforme docstring `:663`–`:669`. |
| `app/chamados/juizes.py:853` | Chave do memo de `pergunta_viva`: qualquer string entra como dado; alteração da via invalida a identidade do memo. Não classifica nem rejeita valor novo. |
| `app/chamados/juizes.py:1386`, `:1394` | Projeção temporal: se materialização está no futuro, zera via, exceto `duplicata`/`motivo_retratado`. Via nova seria zerada nessa projeção. |
| `app/chamados/juizes.py:1412` | C6: `competencia_trancada`, `duplicata`, `motivo_retratado` materializadas morrem **antes da resposta**. Via nova desconhecida não entra. |
| `app/chamados/juizes.py:1417`–`:1429` | C6: resposta com texto vence via desconhecida; continua viva salvo decisão admin na célula ou concordância com a luz. |
| `app/chamados/juizes.py:1437`–`:1439` | C6: identidade+tranca materializadas morrem; `admin_decidiu` passa aqui, mas somente **depois** do ramo da resposta. Via desconhecida segue para fato/célula/lei/materialização (`:1444`, `:1450`, `:1476`, `:1483`). Logo não há um default único “desconhecida = morta”. |
| `app/chamados/models.py:3056` | `fechar_por_admin`: somente `admin_decidiu` materializada é no-op; desconhecida pode ser sobrescrita por `admin_decidiu` se chegar a esta porta. |
| `app/chamados/models.py:3119` | `carimbar`: identidade+tranca produzem erro; desconhecida segue ao C6. |
| `app/chamados/models.py:3179` | `carimbar_identidade`: tudo fora das cinco identidades produz erro, incluindo via nova. |
| `app/chamados/models.py:3181` | Pergunta já fora do fato não pode receber via que a devolva ao fato; desconhecida seria recusada nesse caso. |
| `app/chamados/models.py:3202` | `fora_do_fato`: verdadeiro somente para `duplicata`/`motivo_retratado`; nova é falsa. |
| `app/chamados/models.py:3256` | `descarimbar_em_lote` proíbe fornecer `via_resolucao` em `**outros`, independentemente do valor. |
| `app/chamados/models.py:3266` | Descarimbo em lote poupa somente fora-do-fato; via nova fica elegível ao apagamento. |
| `app/chamados/models.py:3274` | Encerrar por fato é no-op em `fato_materializado`/fora-do-fato; desconhecida pode ser sobrescrita pela porta. |
| `app/chamados/models.py:3285` | Encerrar fantasma é no-op somente em `fantasma` materializada; desconhecida pode ser tentada como `fantasma`, sob C6. |
| `app/chamados/models.py:3297` | Reabrir fantasma só toca `fantasma`; nova ignorada. |
| `app/chamados/models.py:3314` | Reabrir por resposta só toca `chamado_encerrado`/`legado_sem_lastro`; nova ignorada. |
| `app/chamados/models.py:3321` | Reabrir por competência só toca `competencia_trancada`; nova ignorada. |
| `app/chamados/models.py:3348`–`:3362` | `q_vivas`: identidade+tranca sem resposta ficam excluídas por `_identidade`; só tranca+fora-do-fato vencem resposta em `_trancada`. Via nova não tem esses privilégios e pode continuar no queryset, que é pré-filtro generoso. |
| `app/chamados/services/relavra_via.py:118` | Re-lavra apenas `declaracao`/`completude_turno`; via nova ignorada. |
| `app/chamados/services/relavra_via.py:155` | Seleção da re-lavra por dia só inclui essas duas; via nova ignorada. |
| `app/chamados/services/relavra_via.py:181` | `q_carimbo_desfazivel` protege apenas `VIAS_DE_IDENTIDADE`; tranca e nova podem entrar como candidatas. Depois o C6 impede desfazer carimbo legítimo (`:242`). |
| `app/chamados/services/materializacao.py:444`–`:448` | **Risco de UI:** pergunta de hora com resposta e `materializada_em`: `paridade_recusada` vira `recusada_paridade`; **toda outra via, inclusive tranca/admin/nova desconhecida, vira `aplicada`**, sem conferir batida neste ramo. |
| `app/chamados/services/materializacao.py:632` | `perguntas_absorvidas_por`: inclui somente `ausencia_dia`/`declaracao`; nova ignorada. |
| `app/chamados/services/passivo_validado.py:141`–`:145` | `_balde_da_recusa`: qualquer via não vazia que mudou vira balde `via:<valor>`; nova seria classificada assim, sem enumeração. |
| `app/chamados/services/fio.py:1244` | `merito_perdedor`: somente `admin_decidiu` com resposta é mérito que impede retratação silenciosa; nova não recebe essa proteção. |
| `app/chamados/services/fio.py:1351` | Idempotência da retratação só reconhece `duplicata`; via nova não é no-op. |
| `app/chamados/services/fabrica_da_celula.py:149` | Lista exclui apenas `duplicata`; nova entra, e a elegibilidade para descarimbar usa `q_carimbo_desfazivel` em `:150`. |
| `app/chamados/services/tarefa_do_dia.py:230`–`:235` | Exclui fora-do-fato, exceto retratação posterior ao instante; via nova segue como pergunta do fato, podendo depois ser excluída por decisão `retratacao`. |
| `app/ponto/services/cartorio.py:1650` | Reconciliação especial somente se **todas** as perguntas forem `duplicata`; nova impede esse ramo especial. |
| `app/ponto/services/flip_auto.py:129` | Fila de inversão só seleciona `paridade_recusada`; nova ignorada. |
| `app/chamados/services/competencia_trancada.py:276` | Reabertura seleciona apenas `competencia_trancada`; nova ignorada. |

**Cada leitor de identidade/dedup/contadores que exclui fora-do-fato**

Nestes sítios, `duplicata`/`motivo_retratado` são excluídas; **todos os demais valores, inclusive admin, tranca e desconhecida, continuam no universo**, sem decisão financeira por esse teste: [nao verificado]

- Perguntas corrigíveis do chamado: `app/chamados/models.py:1489`.
- `do_fato`, ramo de episódio: `app/chamados/models.py:2725`.
- `q_dos_fatos`, usado por `linhas_dos_fatos`: `app/chamados/models.py:2756`.
- Índice único de marco: `app/chamados/models.py:3486`.
- Índice único de episódio: `app/chamados/models.py:3492`.
- Dedup de motivo sem data: `app/chamados/services/disputa_emissao.py:328`.
- Dedup de motivo datado sem destino obrigatório: `app/chamados/services/disputa_emissao.py:400`.
- Dedup de furo por dia: `app/chamados/services/disputa_emissao.py:419`.
- Outro dedup de furo por dia: `app/chamados/services/disputa_emissao.py:437`.
- Contador `reuso_sem_rechave`: `app/chamados/services/fio.py:1101`.
- Contador `sem_dia_na_saida`: `app/chamados/services/fio.py:1168`.
- Universo `_no_fato`: `app/chamados/services/fio.py:1434`.
- Contador `fecho_cruza_dias`: `app/chamados/services/fio.py:1854`.
- Contador `perguntas_sobre_batida_retratada`: `app/chamados/services/fio.py:1890`.
- `universo_do_grupo`: `app/chamados/services/fio.py:1912`.
- Acervo no agrupamento de colisões: `app/chamados/services/fio.py:1957`.

**Cada leitor de apresentação/auditoria/comandos**

| Sítio | Ação e valor novo |
|---|---|
| `app/templates/chamados/partials/_card_atendimento.html:186` | Frases explícitas para `chamado_encerrado`, `declaracao`, `completude_turno`, `sem_turno_aberto`, `admin_decidiu`, `adesao`, `fato_ocupado`/`paridade_recusada`, `duplicata`; demais não vazias usam `get_via_resolucao_display|lower`. Tranca usa o rótulo dos choices. Valor novo sem choices aparecerá pela resolução padrão desse método [nao verificado na implementação do Django desta cópia]. |
| `app/chamados/services/explicador.py:93`–`:95` | Se juiz de materialização aprova, apresenta resolvida/“Nada a fazer”, motivo pelo display da via; vazia vira “gravada”. Nova segue o resultado do juiz, sem ramo próprio. |
| `app/chamados/services/materializacao.py:674` | Captura via em foto de reversão da absorção; não classifica. |
| `app/chamados/services/passivo_validado.py:64`, `:83` | Captura via em foto reversível; qualquer valor é dado. |
| `app/chamados/services/passivo_validado.py:521` | Guarda via anterior para comparação do resultado da tentativa; nova é dado. |
| `app/chamados/models.py:1921`, `:1930` | Captura via anterior na trilha do fecho por admin. |
| `app/chamados/models.py:2056` | `remover_motivo` exclui quem já é `motivo_retratado`; nova não é excluída. |
| `app/chamados/models.py:2061` | Inclui via anterior em `recusa_motivo`; nova é texto de trilha. |
| `app/chamados/reconciliador.py:1144`, `:1155` | Foto antes/depois do encerramento de fantasma; não classifica. |
| `app/chamados/services/disputa_emissao.py:2050`, `:2065`, `:2068` | Antes/depois e descrição do fecho sem credor; não classifica via lida. |
| `app/chamados/services/relavra_via.py:123`, `:134`, `:136` | Trilha da re-lavra, quaisquer valores anterior/novo. |
| `app/chamados/services/relavra_via.py:245`, `:252` | Trilha de descarimbo, qualquer via anterior. |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:101` | Seleciona somente `admin_decidiu` materializada sem validação para criar atos faltantes; nova ignorada. |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:314` | Guarda repetida sob trava: fora de `admin_decidiu` ignora. |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:330`, `:335` | Foto/trilha da via anterior à retratação. |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:398`–`:404` | Diff do plano preserva via qualquer nas intocadas e projeta `duplicata` nas retratadas. |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:504` | Carrega campo na consulta das perguntas; comportamento seguinte depende das portas já citadas. |
| `app/chamados/management/commands/relavrar_via_i6.py:58`–`:69` | Fora de `declaracao`/`completude_turno`, contabiliza “via de DECISAO, nao se re-lavra”. Nova cai nesse ramo, ainda que não represente decisão. |
| `app/chamados/management/commands/relavrar_via_i6.py:88` | Conta pares de vias para apresentação. |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py:67` | Seleciona apenas `fantasma`; nova ignorada. |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py:129`, `:142`, `:160` | Snapshot/trilha de via, sem classificar. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:33` | Exclui `fato_materializado`; nova entra na consulta. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:40`–`:43` | Só `''`, `legado_sem_lastro`, `None` podem ser sobrescritas no comando; nova fica intocada. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:49`, `:66`, `:75`, `:76` | Contagem e trilha das vias; sem ramo adicional. |
| `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:70`–`:72` | Sem batida fonte: `ausencia_dia`/`declaracao` recusam como “absorcao”; qualquer outra, inclusive nova, recusa como “sem_fonte”. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:47`–`:48` | Piso I6 é união de `VIAS_SEM_TESTEMUNHA` e `VIAS_SEM_LASTRO`; desconhecida fica fora desse piso. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:202`–`:207` | Contraprova F1t exige via não vazia + pergunta morta; rótulos: validação, identidade se em `VIAS_DE_IDENTIDADE`, senão “carimbo/celula”. Nova morta cai no último rótulo. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:433` | Agrupa contagem por qualquer via, nova inclusive. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:542` | Retorna via junto ao PK do universo I6; sem enumeração. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:955` | I9 acusa materializada com via vazia; nova não vazia não acusa aqui. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:1710` | Piso F1t admite qualquer via não vazia, desde que C6 declare morta; nova pode entrar. |
| `app/chamados/models.py:3222` | `via_resolucao` consta entre campos que acordam célula; qualquer valor novo altera o mesmo campo. |

**Enumerações fechadas e distinção admin/tranca**

- `VIAS_FORA_DO_FATO=('duplicata','motivo_retratado')`: `app/chamados/models.py:2394`. A classe reexporta em `:2515`; mexer nela muda identidade/dedup/índices, não apenas vitalidade.
- `VIAS_DE_IDENTIDADE=(admin_decidiu, duplicata, fato_ocupado, paridade_recusada, motivo_retratado)`: `app/chamados/models.py:2511`.
- `VIAS_QUE_VENCEM_A_RESPOSTA=(competencia_trancada,)+VIAS_FORA_DO_FATO`: `app/chamados/models.py:2525`. Esta é indispensável para a morte anterior à resposta no C6 e em `q_vivas` (`juizes.py:1412`; `models.py:3353`).
- `VIAS`/choices: lista em `app/chamados/models.py:2526`–`:2543`, campo com `max_length=24` em `:2563`.
- `vias_que_encerram_por_identidade`: soma `competencia_trancada` às identidades, em `app/chamados/models.py:3096`; alimenta C6 (`juizes.py:1437`), SQL (`models.py:3348`) e `_ABSOLVEM_SEMPRE` (`juizes.py:689`).
- `VIAS_SEM_TESTEMUNHA`: `app/chamados/models.py:3733`–`:3764`: `completude_turno`, `fantasma`, `sem_turno_aberto`, `juiz_aprovou`, `admin_decidiu`, `paridade_recusada`, `duplicata`, `fato_ocupado`, `adesao`, `cadastro_folga`, `celula_absolveu`, `ausencia_dia`, `competencia_trancada`, `motivo_retratado`.
- `VIAS_SEM_LASTRO=(chamado_encerrado,legado_sem_lastro)`: `app/chamados/models.py:3769`.
- `VIAS_DERIVADAS=(declaracao,completude_turno)`: `app/chamados/services/relavra_via.py:77`.
- `SEM_HISTORIA=('',legado_sem_lastro,None)`: `app/chamados/management/commands/encerrar_perguntas_com_fato.py:40`.
- Tupla local de absorção `(ausencia_dia,declaracao)`: `app/chamados/services/materializacao.py:632`; tupla igual para motivo de recusa de migração: `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:70`.
- `admin_decidiu` é identidade, sem testemunha e possui proteções de mérito/backfill; tranca adicionalmente vence resposta e tem reabertura própria: `models.py:2511`, `:2525`, `:3321`, `:3735`, `:3760`; `fio.py:1244`; `backfill_decisoes_pergunta.py:101`.
- Nova via **não deve ser inserida em todas essas listas por analogia**: cada lista aciona operações diferentes, comprovadas nos leitores acima (`models.py:2756`, `:3266`, `:3314`; `relavra_via.py:118`).

A busca direta pelo campo não encontrou leitores em folha/motor/TXT/inteligência; ausência absoluta de leitura indireta nesses domínios é [nao verificado]. A principal leitura indireta que eles podem receber é a decisão do C6/juiz de materialização, cujos comportamentos estão discriminados acima (`app/chamados/juizes.py:679`, `:1399`).

## 13/11 12:05 · sessao 7b81833cadb3 · - · - · origem=Ronald (tmux)

Q7 — auditoria `desligamento_revisao`, cada sítio:

- **Emissor:** `app/colaboradores/services/desligamento.py:168` cria o chamado associado ao mesmo colaborador, área `dp_rh`, categoria `auditoria_clt`, subcategoria `registrar_resposta_oficial`, módulo `desligamento_revisao`; descrição atual contém demissão, autor, motivo e instrução de responder, mas nenhuma lista de perguntas/respostas (`:170–188`).
- **JSON atual:** `app/colaboradores/services/desligamento.py:182` grava somente `data_demissao`, `motivo_informado`, `executado_por_username`, `executado_por_id`, `executado_em` (`:183–187`).
- **Notificação:** `app/colaboradores/services/desligamento.py:192` envia push ao DP com URL `/chamados/local/<pk>/` (`:195`). Entrega real do push [nao verificado].
- **Rota existente:** `app/chamados/urls.py:14` liga essa URL a `detalhe_chamado_local`.
- **View existente:** `app/chamados/views.py:363` busca o chamado local; gestores podem acessá-lo e não gestores precisam ser o próprio colaborador (`:364–370`); contexto construído por `montar_contexto_detalhe` e renderização de `detalhe_local.html` em `:549–551`.
- **Contexto do detalhe:** `app/chamados/services/acoes_chamado.py:409` entrega `chamado` e `followups` diretamente (`:411–412`). Perguntas mostradas pelo bloco de disputa vêm apenas da disputa **aberta** (`:345–347`) e filtradas por pergunta viva (`:357–358`); esse bloco não recuperaria automaticamente as perguntas mortas para o novo audit.
- **Template detalhe, descrição completa:** `app/templates/chamados/detalhe_local.html:94` exibe `{{ chamado.descricao }}` sem truncamento (`:97`). É um encaixe existente para a lista textual informativa dia/pergunta/resposta.
- **Template detalhe, timeline:** `app/templates/chamados/detalhe_local.html:112` percorre followups. Uma ocorrência informativa também aproveitaria a timeline existente, pela porta `nota.anexar_ocorrencia`, usada no próprio executor em `app/colaboradores/services/desligamento.py:163`.
- **Painel lateral existente:** `app/chamados/views.py:347` entrega chamado/followups ao template `painel_chamado_turno.html` (`:351–355`); `app/templates/chamados/partials/painel_chamado_turno.html:33` percorre genericamente todo `contexto_json.items` (`:35–37`). Lista estruturada no JSON apareceria ali pela representação padrão do valor, sem formatação própria de itens.
- **Modal DP existente:** `app/chamados/views.py:889` monta fio com explicação, renderizado em `:927`; `app/templates/chamados/partials/modal_fio.html:456` mostra descrição truncada em 200 caracteres. Apenas acrescentar a lista à descrição não garante lista completa nesse modal.
- **Modal DP, timeline:** `app/templates/chamados/partials/modal_fio.html:461` percorre followups e imprime conteúdo completo em `:473`.
- **Fio completo DP:** `app/chamados/views.py:677` monta os chamados e renderiza `fio_colaborador.html` em `:697`; `app/templates/chamados/fio_colaborador.html:231` também trunca descrição em 200 caracteres; followups entram em `:235`.
- **Cartão de atendimento:** `app/templates/chamados/partials/_card_atendimento.html:39` mostra descrição truncada em 200 caracteres.
- **Painel do DP:** `app/chamados/views.py:721` usa `montar_contexto_painel` (`:726–738`); `app/chamados/services/painel.py:306` aceita filtro explícito de status. Sem filtro, restringe a `COBRANCA` (`:316`), excluindo `registrado`; o drill `?status=registrado` é documentado junto à regra (`:310–312`).
- **Natureza do audit:** `app/chamados/catalogo/natureza.py:18` inclui `desligamento_revisao` em `MODULOS_AUDITORIA` (`:20`); `eh_acionavel` retorna falso (`:36–38`) e `dono_do_chamado` retorna `registro` antes das perguntas (`:44–45`).
- **Rótulo:** `app/chamados/services/desenlace.py:30` traduz para “Revisao de desligamento”; ramo `registro` fornece “Registro de auditoria” em `:68–71`.
- **Texto humano de registro:** `app/chamados/services/verbos.py:176`: “Desligamento registrado aguardando conferencia de rotina do RH.”
- **Catálogo do dia:** `app/chamados/catalogo/modulos.py:141` declara `data_demissao` como chave de data, horizonte `DIA`, dono `dp`; leitor genérico `app/chamados/juizes.py:100` passa JSON ao catálogo em `:113–114`.
- **Tranca:** `app/chamados/catalogo/modulos.py:265` inclui esse audit em `ATOS_DO_DP_FORA_DA_TRANCA`; `fica_fora_da_tranca` reconhece-o em `:272`, e `app/chamados/models.py:1042` retorna falso para competência trancada nesse caso.
- **App do colaborador:** `app/chamados/services/janela_colab.py:36` considera-o módulo interno; a lista do colaborador o exclui em `:74`.
- **Resposta oficial DP:** `app/chamados/views.py:546` despacha resposta livre; `app/chamados/services/acoes_chamado.py:301` anexa ocorrência e `:322–325` permite transição de status.

**Encaixe sem tela nova:** lista estruturada no `contexto_json` do audit (`desligamento.py:182`) + apresentação no detalhe já existente junto à descrição (`detalhe_local.html:94`) ou ocorrência informativa na timeline (`modal_fio.html:461–473`, `painel_chamado_turno.html:45–56`). Só preencher JSON não cria apresentação estruturada no detalhe/modal atual: o detalhe entrega o objeto (`acoes_chamado.py:411`) e seus usos específicos de JSON no template são outros blocos, enquanto o único loop genérico encontrado é `painel_chamado_turno.html:35`.

**Tensão concreta com o selo:** cascade encerra os chamados preexistentes em `app/colaboradores/services/desligamento.py:155–163`; o audit nasce **depois**, em `:168`. `ChamadoColaborador.criar` deriva evento `REGISTRAR` pela natureza (`app/chamados/models.py:1047–1065`); o motor o converte para `registrado` (`app/chamados/catalogo/motor.py:133–134`). `registrado` pertence tanto a `VIVOS` (`:24`) quanto a `NAO_FECHADOS` (`:32`). Assim, quando o nascimento é permitido, o próprio ato deixa o audit do desligado não fechado; isso conflita com contador literal zero de todos os `NAO_FECHADOS` do desligado. É inferência estática desses ramos, sem contagem de frota [nao verificado].

Q8 — callers e outras portas de situação:

1. **UI:** `app/colaboradores/views.py:1426::desligar_colaborador`; exige `editar_cadastro` (`:1428`), chama juiz (`:1434`) e executor (`:1437`) com `hoje`.
2. **Cron/comando:** `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38` seleciona demissão preenchida até hoje e situação diferente de desligado (`:40`); DRY sem `--apply` (`:46–47`), revalida pelo juiz (`:51–56`) e executa pela mesma porta (`:57`).
3. **Agenda declarada:** `app/config/crons.py:440` cadastra esse comando às `06:06`, com `--apply`, log `desligamento_agendado.log` (`:441`). Execução real do cron [nao verificado].
4. **Agendamento futuro no executor:** `app/colaboradores/services/desligamento.py:87` grava data/motivo (`:90–91`) e retorna (`:106`), sem atribuir `situacao='desligado'`; consumação concreta atribui em `:109` e persiste via `salvar_inteiro` em `:113`.
5. **Outro escritor concreto de desligado:** nenhum encontrado fora de `executar_desligamento` na busca da cópia; ausência universal de execução dinâmica/SQL externo [nao verificado].
6. **Porta genérica capaz de persistir situação:** `app/colaboradores/services/colaborador.py:32` permite o campo na operação `situacao`; `gravar` aplica valores por `setattr` e salva (`:55–61`). Não veta o valor `desligado` nem executa cascade.
7. **Porta genérica com atribuição externa:** `app/colaboradores/services/colaborador.py:65–69::salvar_campos` salva campos previamente atribuídos. Também não veta desligado nem chama cascade.
8. **Porta genérica de objeto inteiro:** `app/colaboradores/services/colaborador.py:73–76::salvar_inteiro` confere apenas nome da operação e chama `save()` completo; pode regravar situação do objeto sem cascade.
9. **Porta genérica bulk:** `app/colaboradores/services/colaborador.py:80–88::atualizar_em_lote` aceita `situacao` pela mesma declaração e faz `qs.update(**campos)`; nenhum caller concreto fornecendo desligado encontrado.
10. **Porta genérica criação:** `app/colaboradores/services/colaborador.py:91–93::criar_colaborador` passa `**campos` ao ORM; pode criar diretamente com desligado se assim chamada, sem cascade. Caller de admissão encontrado fixa ativo em `app/colaboradores/views.py:1208`.
11. **Edição web:** `app/colaboradores/views.py:1314–1341` atribui campos explicitamente, sem situação, e salva inteiro em `:1345`; nenhum POST de situação encontrado nesse caminho.
12. **Django admin:** `app/colaboradores/admin.py:48` herda `SoLeitura`; `app/core/admin.py:59–66` nega adicionar/alterar/excluir e `:68–75` torna campos somente leitura.
13. **Escritor de afastamento, não desligamento:** `app/ponto/views.py:2435` grava `afastado`.
14. **Reversor de afastamento, não desligamento:** `app/ponto/management/commands/reverter_situacao_afastado.py:73` grava `SITUACAO_VOLTA`; constante é `ativo` em `app/ponto/services/afastado_avisa.py:101`.
15. **Reativação:** `app/colaboradores/services/desligamento.py:215` atribui ativo e persiste em `:219`; não é outro desligador.
16. **Saneamento de vínculo de desligado:** `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:35` busca quem **já** está desligado para fechar vínculo; não chama executor nem grava desligado.
17. **Outros callers de produção do executor, ModelForm/serializer gravando situação, importador gravando desligado ou `.update/bulk_update` concreto com desligado:** não encontrados nas buscas; execução externa à cópia e estado real de produção [nao verificado].

## 13/11 12:04 · sessao 26f32bc54fa1 · - · - · origem=Ronald (tmux)

Censo Q5/Q6, somente leitura, com caminhos relativos à raiz da cópia.

**Q5 — guarda de pergunta viva e campo que mata**

- `ChamadoColaborador.encerrar()` consulta o motor para `ENCERRAR`, devolve `False` se a transição não é permitida e depois consulta `_gate_pergunta_viva`; se o gate recusa, retorna `False` antes de gravar estado. O docstring “sem guard” está desatualizado em relação ao corpo. `app/chamados/models.py:1606-1621`.
- `_ancoradas()` busca perguntas **não validadas**, ancoradas por `chamado_destino=self` ou, sem destino, pelo chamado pai da disputa. `app/chamados/models.py:336-347`.
- `_vivas_entre()` simula o chamado no estado de destino; seleciona perguntas ainda ancoradas, cuja competência é aberta (`competencia_do_dia_entregue(...) is False`) e cujo juiz `pergunta_viva` retorna verdadeiro. `app/chamados/models.py:349-372`.
- O gate só atua ao sair de `VIVOS` para `TERMINAIS`; sem perguntas vivas libera; se todas puderem ser reatadas a outra cobrança válida do mesmo colaborador/dia, reata e libera; caso contrário recusa com trilha `fechamento_recusado`, motivo `pergunta_viva_na_competencia_aberta`, IDs das perguntas e estados de origem/destino. `app/chamados/models.py:374-395`, `app/chamados/models.py:410-445`.
- `pergunta_viva` é a porta com memo por instância, cujo cálculo está em `_pergunta_viva_calc`; a property `PerguntaDisputa.viva` delega diretamente a ela. `app/chamados/juizes.py:862-889`, `app/chamados/models.py:3365-3376`.
- **`validada_em` preenchido mata imediatamente** no primeiro ramo do juiz. `via_resolucao` preenchida, sozinha, não mata. `app/chamados/juizes.py:1399-1413`.
- A tranca do dia, por leitura da autoridade, mata antes de considerar resposta; depois, `via ∈ VIAS_QUE_VENCEM_A_RESPOSTA` com `materializada_em` preenchido também mata antes da resposta. A tupla atual é `(competencia_trancada,) + VIAS_FORA_DO_FATO`. `app/chamados/juizes.py:1403-1413`, `app/chamados/models.py:2517-2525`.
- Resposta com `respondida_em` e texto continua viva se o admin não decidiu o dia e não existe luz concordante na célula; uma via desconhecida com carimbo **não vence esse ramo**. `app/chamados/juizes.py:1414-1429`.
- Depois da resposta, vias retornadas por `vias_que_encerram_por_identidade()` com `materializada_em` matam; outros ramos examinam fato do marco, veredito da célula, falta do dia e lastro externo. Portanto não há um único campo universal: validação mata de imediato; fechamento terminal sem validação precisa de via reconhecida no degrau adequado e carimbo de materialização. `app/chamados/juizes.py:1430-1489`.
- `q_vivas()` é o pré-filtro SQL amplo; exclui validadas e vias que vencem a resposta com materialização, antes de aceitar resposta preenchida. A via nova deve respeitar a mesma fonte usada pelo juiz para não haver divergência. `app/chamados/models.py:3327-3363`.

**Q6 — leitores da resposta com efeito no dia/dinheiro**

1. **Roteador no próprio ato da resposta:** `responder_pergunta_disputa_core` consulta `ESCAPES_QUE_VIRAM_AUSENCIA` e chama `_materializar_ausencia_de_resposta`; `FALTA_DIA → falta/aprovada`, `ATESTADO → atestado/status decidido por estado_inicial`. Isso ocorre quando o colaborador responde, independentemente de uma futura via de desligamento. `app/chamados/services/disputa_emissao.py:1536-1541`, `app/chamados/services/materializacao.py:600-606`.

2. **Materialização de ausência pela resposta:** cria a `Ausencia` pela porta `criar_ausencia`, usando data da pergunta, tipo/status recebidos do roteador; não inventa minutos fixos. Também tenta carimbar irmãs do dia e registra absorção. Uma ausência já criada por esse caminho é fato anterior ao desligamento; conservar ou reverter esse fato por uma via futura é [nao verificado]. `app/chamados/services/materializacao.py:502-535`, `app/chamados/services/materializacao.py:538-579`.

3. **Validação humana de folga:** `_e_aceite_de_folga` compara a resposta com `RESPOSTA_DE_FOLGA`; `_aceitar_a_folga` consulta `checar_declaracao`, carimba validação e chama `declarar_dia(..., tipo='folga')`; em sucesso, fecha a pergunta por admin; em recusa, mantém/desfaz o ato. Não é a morte da pergunta que declara a folga. `app/chamados/services/validacao.py:234-236`, `app/chamados/services/validacao.py:305-343`.

4. **Validação humana geral:** `validar_pergunta` exige `respondida_em`, recusa já validada, consulta plausibilidade, carimba validação e dispara materialização/recalculo. A função não consulta `pergunta_viva` nem recusa uma via de desligamento inexistente antes de validar: um futuro acesso direto a uma pergunta morta ainda precisaria ser auditado conforme a implementação. `app/chamados/services/validacao.py:389-413`, `app/chamados/services/validacao.py:425-469`.

5. **Seleção para materialização normal:** `materializar_perguntas_validadas_da_disputa` seleciona `respondida_em != NULL`, `validada_em != NULL` e `materializada_em == NULL`; somente essas passam à unidade `_materializar_uma`. Uma pergunta encerrada com `materializada_em` e sem validação não entra nesse lote. `app/chamados/services/materializacao.py:1375-1385`.

6. **Unidade de materialização:** escolhe implementação por `tipo_resposta` (`hora`, `data_hora`, `escolha`); para hora/data_hora exige aprovação do juiz do lastro; reprovação levanta dentro do atomic e desfaz o plantio. Não transforma via desconhecida em aprovação automaticamente nesse despacho. `app/chamados/services/materializacao.py:1224-1249`.

7. **Derivação de tipo pelo texto:** `_tipo_pelo_marco` lê a hora da resposta, compara aos marcos da grade e resolve tipo `E/S`; divergência entra em observabilidade. `app/chamados/services/materializacao.py:274-310`.

8. **Derivação de instante:** `_ts_tipo_do_plantio` recusa respostas maiúsculas/`[descartado`, criva defaults de relógio incompatíveis com a régua, deriva tipo e usa a hora respondida; em intervalos fixos pode usar a hora do marco; depois ajusta overnight. É derivação reutilizada tanto para leitura quanto para plantio. `app/chamados/services/materializacao.py:354-428`.

9. **Plantio de hora:** `_materializar_resposta_hora` exige texto; sem ele grava recusa e não planta; com texto deriva instante, reusa batida existente ou passa pelas guardas de plantio. A simples morte da pergunta não chama esta função. `app/chamados/services/materializacao.py:790-800`, `app/chamados/services/materializacao.py:835-865`, `app/chamados/services/materializacao.py:871-888`.

10. **Plantio de data+hora/âncora:** `_materializar_resposta_data_hora` interpreta `resposta_colab` como data/hora, chama `registrar_batida` tipo `E`, já aprovada, e não escreve fase do vínculo. `app/chamados/services/materializacao.py:984-1035`.

11. **Resposta de escolha/falta declarada:** `_materializar_resposta_escolha` chama `_declarar_ausencia_de_pergunta` em `motivo='declara_falta'`; esse leitor usa `_DECLARA_FALTA_MAPA.get(resp)`, cria ausência pelo tipo reconhecido e ignora resposta desconhecida/“Compareci” quando o mapa retorna `None`. `app/chamados/services/materializacao.py:468-499`, `app/chamados/services/materializacao.py:1069-1086`.

12. **Recusa humana é ato próprio:** `recusar_pergunta` exige motivo, recusa já validada, grava `recusa_motivo`, chama reabertura com ato `recusa`, limpa resposta/carimbos pela porta e conserva texto anterior na trilha. Uma via de desligamento preenchida não equivale, por si só, a esse ato de recusar. `app/chamados/services/validacao.py:561-605`.

13. **Juiz de slot/lastro:** `_slot_esperado` lê diretamente a resposta em `data_hora` e delega a `_ts_tipo_do_plantio` para `hora`; `_pergunta_materializada_de_fato` confronta isso com batida real/slot. Pergunta `escolha` com `materializada_em` retorna `True`, mesmo sem validação, pelo ramo próprio. Uma nova via sem tratamento pode, portanto, cair em lastro **positivo para escolha**, mas isso não cria uma ausência/batida; é leitura de resolução da pergunta. `app/chamados/juizes.py:341-374`, `app/chamados/juizes.py:662-708`, `app/chamados/juizes.py:752-761`.

14. **Concordância resposta/luz:** `_resposta_concorda_com_a_luz` compara hora respondida com luz do marco pela régua classe A e exige célula sem veredito acusatório; isso pode dispensar validação quando o fato já está no espelho, não cria o fato a partir da resposta. `app/chamados/juizes.py:1342-1375`, `app/chamados/juizes.py:1425-1429`.

15. **Classificação de resposta para validar:** `delta_da_resposta` compara hora do texto com marco esperado; `classificar_resposta_validacao` classifica escolha afirmativa como A, outras escolhas como B, e horas pelo delta; `motivo_implausivel_validacao` usa a mesma resposta para a recusa de implausibilidade. Não decidem dinheiro diretamente; orientam a validação que pode materializar fato. `app/chamados/juizes.py:1846-1878`, `app/chamados/juizes.py:1952-1993`.

16. **Classificação de lote:** `classe_do_lote` classifica escape/posto/confirmacão sem marco em C, texto ilegível em D, sem marco em grupo próprio, horas fora do envelope/conflito em B, demais em A. A fila global de IA/lote usa `q_vivas` e confirma `pergunta_viva`, portanto uma pergunta reconhecidamente morta sai dessa fila. `app/chamados/services/validacao.py:845-877`, `app/chamados/juizes.py:1678-1684`.

17. **Materializador ao fechar disputa (S144):** percorre perguntas ainda sem `materializada_em`, exige `_pergunta_materializada_de_fato` e tenta carimbar `VIA_JUIZ_APROVOU`; não fabrica batida. Se a porta de desligamento já carimbar materialização, a pergunta não entra neste laço. `app/chamados/services/materializacao.py:1414-1438`.

18. **UI que diria falsamente “aplicada”: ponto crítico.** `estado_aplicacao_pergunta` ignora perguntas que não sejam `hora` ou não tenham texto. Havendo `materializada_em`, só distingue `VIA_PARIDADE_RECUSADA`; para **qualquer outra via, inclusive uma nova desconhecida, retorna `aplicada`**. O carregador do fio atribui esse resultado a `_p.estado_aplicacao`. Esse leitor precisa distinguir a via de desligamento para não apresentar encerramento informativo como hora aplicada. `app/chamados/services/materializacao.py:431-448`, `app/chamados/services/fio.py:334-338`.

19. **Prévia da folha:** carrega todas as perguntas do colaborador/dia, sem filtro de vida/validação; `_material` devolve `'resposta do colaborador'` se houver texto, antes de consultar `fio_vivo`. Uma pergunta morta por desligamento continua aparecendo como material disponível, mas a função é de leitura e não aplica resposta. `app/folha/previa.py:28-31`, `app/folha/previa.py:63-67`, `app/folha/previa.py:139-146`.

20. **Cartório, proteção contra ressuscitação automática:** `_tem_resposta_do_colab` olha apenas texto; o laço de fio morto pula chamado com qualquer resposta, preservando-o para materializador/admin; o mesmo predicado é reutilizado no dedup. A via nova não muda essa isenção enquanto o texto permanecer. `app/ponto/services/cartorio.py:1637-1650`, `app/ponto/services/cartorio.py:1661-1666`.

21. **Cartório, efeito indireto da morte do fio:** conta chamados vivos com `fio_vivo` e entrega `cobrancas_vivas` ao `classificar_dia`. O suprajuiz usa ausência de cobrança viva em ramos de protesto do dia. Matar pergunta/chamado pode mudar o diagnóstico em relavra; não é aceitação nem recusa da resposta. `app/ponto/services/cartorio.py:1384-1389`, `app/ponto/services/cartorio.py:1464-1470`, `app/ponto/supra_juiz.py:313-316`, `app/ponto/supra_juiz.py:454-456`.

22. **Retenção de export/TXT:** `motivos_retencao_celula` lê veredito/protestos da célula, não a resposta da pergunta; retém célula acusatória salvo os protestos isentos e a regra da pausa. Logo não se deve afirmar “zero efeito possível na folha” apenas por não validar: o fio participa indiretamente dos protestos, e os protestos participam da retenção. Efeito concreto no valor/TXT do caso futuro [nao verificado]. `app/folha/export.py:1176-1184`, `app/folha/export.py:1232-1266`.

23. **Saída de rescisão do TXT:** `classificar_export` põe fora do TXT quem `sai_pela_rescisao`, com motivo `rescisao_modulo_proprio`; demais competências continuam pelos crivos seguintes. Não usa a resposta como fato aceito/recusado nesse gate. `app/folha/export.py:851-875`.

24. **Consulta de dia do colaborador:** expõe a resposta e `ainda_viva=pergunta_viva(p)` separadamente; não infere aceite a partir de resposta/morte. `app/ponto/services/dia_do_colab.py:62-71`.

**Conclusão delimitada para Q6:** encerrar com `materializada_em`, via própria e `validada_em=NULL` evita o lote normal de plantio; não equivale ao ato humano de aceite ou recusa. Entretanto existem leituras que confundiriam encerramento com aplicação (`estado_aplicacao_pergunta`), lastro de `escolha` que aceita o carimbo, material informativo da prévia que continua reconhecendo o texto e participação indireta do fio nos protestos/retencão. `app/chamados/services/materializacao.py:1375-1379`, `app/chamados/services/materializacao.py:438-448`, `app/chamados/juizes.py:706-707`, `app/folha/previa.py:139-146`, `app/ponto/services/cartorio.py:1389`, `app/folha/export.py:1241-1264`.

## 13/11 12:03 · sessao c959e78cd199 · DESLIGAR-LIQUIDA-TUDO · sonda · origem=Code

Não executei: `sonda1.py` não foi localizado na cópia. Transcrevo integralmente a saída fornecida, sem reexecução. [nao verificado] [nao verificado]

```text
sonda_leitura: sonda1.py [6159c0e80e54cd34] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

desligados 388
== (a) disputas abertas de desligado 26 {'emp2 dem-comp 08/2026': 5, 'emp2 dem-comp 09/2026': 4, 'emp2 dem-comp 10/2026': 5, 'emp2 dem-comp 11/2026': 4, 'emp3 dem-comp 09/2026': 1, 'emp3 dem-comp 10/2026': 1, 'emp3 dem-comp 11/2026': 3, 'emp4 dem-comp 02/2026': 1, 'emp4 dem-comp 11/2026': 2}
== (b) com respostas_sem_veredito 7 respostas 39 {'emp2 dem-comp 10/2026': 1, 'emp2 dem-comp 11/2026': 4, 'emp3 dem-comp 11/2026': 2}
   col373 dem=2026-10-31 disputas=[(3676, 1)]
   col427 dem=2026-10-28 disputas=[(5149, 16)]
   col457 dem=2026-09-25 disputas=[(2828, 1)]
   col752 dem=2026-10-29 disputas=[(6680, 2)]
   col769 dem=2026-11-03 disputas=[(5100, 4)]
   col893 dem=2026-11-03 disputas=[(4271, 14)]
   col981 dem=2026-10-27 disputas=[(7395, 1)]
== (c) perguntas vivas de desligado 598 com resposta 0 em disputa FECHADA 58 {'emp2 comp 09/2026': 275, 'emp2 comp 10/2026': 130, 'emp2 comp 11/2026': 126, 'emp3 comp 09/2026': 7, 'emp3 comp 10/2026': 19, 'emp3 comp 11/2026': 33, 'emp4 comp 09/2026': 1, 'emp4 comp 10/2026': 3, 'emp4 comp 11/2026': 4}
    col42:4 col104:1 col138:1 col143:1 col270:4 col373:38 col388:1 col393:2 col417:3 col422:4 col427:124 col432:3 col437:38 col457:1 col460:21 col510:4 col518:5 col559:15 col569:6 col659:15 col666:13 col669:1 col698:5 col718:1 col752:18 col769:32 col782:55 col863:1 col878:2 col886:12 col893:150 col906:6 col925:4 col951:1 col963:2 col973:2 col981:2
== (d) chamados nao fechados de desligado 396 {'emp1 registrado desligamento_revisao': 1, 'emp2 aberto batida_ausente': 25, 'emp2 aberto disputa_supervisao': 1, 'emp2 aberto sem_previsao_escala': 1, 'emp2 em_analise batida_ausente': 1, 'emp2 em_analise desligamento_revisao': 24, 'emp2 em_analise disputa_supervisao_manual': 2, 'emp2 em_analise sem_previsao_escala': 2, 'emp2 registrado desligamento_revisao': 88, 'emp2 resolvido ancora_limbo': 1, 'emp2 resolvido desligamento_revisao': 63, 'emp2 resolvido disputa_supervisao': 7, 'emp29 resolvido desligamento_revisao': 2, 'emp3 aberto batida_ausente': 6, 'emp3 em_analise batida_ausente': 4, 'emp3 em_analise desligamento_revisao': 4, 'emp3 registrado desligamento_revisao': 25, 'emp3 resolvido ancora_limbo': 9, 'emp3 resolvido desligamento_revisao': 22, 'emp3 resolvido disputa_supervisao': 1, 'emp3 resolvido disputa_supervisao_manual': 2, 'emp4 aberto batida_ausente': 1, 'emp4 em_analise batida_ausente': 3, 'emp4 em_analise desligamento_revisao': 3, 'emp4 registrado desligamento_revisao': 2, 'emp4 resolvido ancora_limbo': 1, 'emp4 resolvido batida_ausente': 40, 'emp4 resolvido chamados': 2, 'emp4 resolvido desligamento_revisao': 4, 'emp4 resolvido disputa_supervisao': 3, 'emp4 resolvido disputa_supervisao_manual': 20, 'emp4 resolvido entrada_bloqueada_turno_aberto': 10, 'emp4 resolvido intervalo_pendente': 2, 'emp4 resolvido justificativa': 6, 'emp4 resolvido turno_aberto': 8}
    col31:3 col34:78 col35:1 col42:2 col46:1 col47:12 col48:1 col52:1 col57:1 col58:1 col64:1 col65:1 col68:2 col71:1 col72:1 col73:1 col74:1 col77:1 col79:1 col86:1 col88:1 col90:1 col91:1 col96:1 col102:1 col103:1 col104:1 col105:1 col122:1 col124:1 col126:1 col132:1 col135:1 col138:1 col140:1 col143:1 col147:1 col153:1 col156:2 col158:1 col160:1 col162:1 col164:1 col167:3 col172:1 col175:1 col176:1 col178:1 col179:1 col180:1 col182:1 col187:1 col208:1 col223:1 col228:1 col229:1 col240:1 col244:1 col270:1 col273:1 col295:1 col299:1 col305:1 col308:1 col311:1 col314:1 col315:1 col316:1 col318:1 col325:1 col330:1 col333:1 col338:1 col343:1 col350:1 col352:1 col353:1 col356:1 col367:1 col370:1 col372:1 col373:9 col375:1 col378:1 col381:1 col385:1 col386:1 col388:1 col393:1 col395:1 col397:1 col400:1 col403:1 col408:1 col409:1 col412:1 col417:1 col422:1 col426:1 col427:3 col429:1 col430:1 col432:1 col433:1 col437:2 col441:1 col443:1 col447:1 col448:1 col457:1 col458:1 col460:1 col462:1 col463:1 col471:1 col480:1 col481:1 col484:1 col486:1 col492:1 col494:1 col498:1 col506:1 col508:1 col509:1 col510:4 col513:1 col518:1 col519:1 col520:1 col525:1 col528:1 col535:1 col543:1 col544:1 col545:1 col554:1 col555:1 col559:1 col568:1 col569:1 col571:1 col576:1 col581:1 col593:2 col606:1 col609:1 col613:1 col621:1 col623:1 col628:1 col630:1 col640:1 col641:1 col643:1 col651:1 col652:1 col653:1 col656:1 col659:1 col660:1 col661:1 col663:1 col666:1 col669:1 col676:1 col697:1 col698:1 col699:1 col703:1 col705:1 col715:2 col718:1 col720:1 col723:1 col728:1 col729:2 col730:1 col731:2 col738:2 col740:2 col742:1 col743:1 col744:1 col745:1 col746:1 col752:1 col753:1 col754:1 col755:1 col759:1 col760:1 col761:1 col769:11 col778:1 col782:2 col783:1 col786:1 col815:1 col816:1 col817:1 col818:1 col828:1 col831:1 col837:1 col840:1 col842:2 col846:1 col847:1 col853:1 col855:1 col856:1 col858:1 col862:1 col863:1 col870:1 col875:1 col878:1 col880:1 col886:1 col890:1 col893:21 col894:1 col901:1 col904:1 col906:1 col907:1 col912:1 col914:1 col925:1 col927:1 col929:1 col931:1 col937:1 col939:1 col942:1 col943:1 col946:1 col951:1 col954:1 col955:1 col959:1 col963:1 col970:1 col973:1 col977:1 col981:1 col984:1 col997:1
== vivos_de_desligado (a)+(c)+(d) = 1020 colabs distintos 249
ERROS 0
```

(a) 26 disputas abertas de desligado. [nao verificado]  
(b) 7 com respostas_sem_veredito; 39 respostas. [nao verificado]  
(c) 598 perguntas vivas; com resposta 0; em disputa FECHADA 58. [nao verificado]  
(d) 396 chamados não fechados de desligado. [nao verificado]  
vivos_de_desligado: 1020; colaboradores distintos: 249; ERROS 0; nenhum traceback no dado fornecido; causa de erro não apresentada. [nao verificado]

## 13/11 11:48 · sessao 942025444273 · O50-DESLIGAR-ENCERRA-TUDO · censo · origem=Code

**Há leitores que alcançam folha e fio.** Os caminhos comprovados incluem retenção/TXT, universo do fechamento, emissão e reabertura de perguntas, adesão e roteamento do `vigia_retido_calado`. Portanto, existe o achado que aciona a exceção temporal descrita no aval anexado. (`app/folha/export.py:1208`, `app/ponto/services/fechamento.py:440`, `app/chamados/services/fabrica_da_celula.py:112`, `app/chamados/models.py:1006`, `app/chamados/services/adesao.py:67`, `app/folha/vigia_retido_calado.py:43`; prazo do aval: [não verificado na árvore])

O inventário abaixo contém **260 sítios**, contando uma query multilinha como um sítio e contando separadamente os campos quando o mesmo sítio usa dois. Incluí seletores de tela, diagnósticos e escolhas de escala para apresentar o perímetro completo; a coluna de impacto distingue os que chegam às travas. As contagens são a consolidação da AST executada em memória, sem arquivo de resultado persistido. [não verificado por artefato persistido]

O método foi: `ast.parse` dos `.py`; captura de `Attribute` em contexto `Load`, keywords ORM por segmentos de `__`, strings de lookup, `getattr`, dicionários e `values_list`; resolução dos imports e acompanhamento das variáveis que transportam os campos; depois triagem semântica e grep dos templates. Exemplos que exigiram isso: `_ECtv`, `_Qc`, `_EC`, `_Col`, `_Esc`, `_ColU`, a tupla `adm/dem/sit` e `_dem`. (`app/folha/export.py:805`, `app/ponto/services/fechamento.py:428`, `app/ponto/services/triagem_batida.py:40`, `app/colaboradores/views.py:2606`, `app/holerite/views.py:280`, `app/relatorios/views.py:1543`, `app/holerite/matriz.py:87`, `app/escala/services/leitor_celula.py:365`)

A execução local percorreu 889 arquivos Python elegíveis, sem erro de parsing; não importou Django nem consultou banco/log. Nenhum arquivo foi criado, editado ou apagado. [não verificado por artefato persistido]

Para ler as tabelas:

- **S** = `Colaborador.situacao` **atual**; **D** = `data_demissao`; **A** = `EscalaColaborador.ativa`. O modelo também admite `ferias`, por isso os dois filtros exclusivos desse valor aparecem no inventário. (`app/colaboradores/models.py:295`, `app/colaboradores/models.py:323`, `app/colaboradores/models.py:325`)
- **F/Fio** contém **sim/não**, nessa ordem. `não/não` identifica apresentação, relatório ou diagnóstico sem caminho de trava no fluxo indicado; efeitos adicionais não demonstrados são [não verificado].
- **CE S**: situação atual desligado/afastado com vínculo válido em `d`; numa consulta histórica, `d` pode ser anterior à mudança. Em tela exclusivamente atual, basta o cadastro atual discordar da vigência que cobre hoje. A autoridade não consulta `situacao`. (`app/escala/alimentacao.py:269`, `app/escala/alimentacao.py:285`)
- **CE A**: vínculo `ativa=False`, com `data_inicio ≤ d ≤ data_fim`, é omitido pelo filtro ativo e aceito pela autoridade; inversamente, `ativa=True` com início futuro pode ser escolhido pelo filtro seco. (`app/escala/alimentacao.py:196`, `app/escala/alimentacao.py:214`, `app/escala/alimentacao.py:282`)
- **CE D**: demissão em 10/11, vínculo ainda cobrindo 11/11: o corte pela demissão retira o dia e o vínculo pode aceitá-lo. **CE B**: `d = demissão = data_fim`; a vigência inclui a igualdade. São hipóteses de código; ocorrência em produção é [não verificado]. (`app/escala/alimentacao.py:215`, `app/escala/alimentacao.py:282`)

Os caminhos reutilizados nas tabelas são:

| Caminho | Destino comprovado |
|---|---|
| **TXT** | `classificar_export` determina os participantes do TXT. (`app/folha/export.py:714`, `app/folha/porta_export.py:469`) |
| **RET** | `motivos_retencao_celula` → `motivos_retencao_espelho` → `classificar_export` → TXT. (`app/folha/export.py:1275`, `app/folha/export.py:968`, `app/folha/export.py:716`) |
| **UF** | Universo da competência → recálculo do fechamento. (`app/ponto/services/fechamento.py:445`, `app/ponto/services/fechamento.py:554`) |
| **CEL** | Regeneração → julgamento dos dias tocados, recálculo e reavaliação/rejunção de chamados. Assim, a seleção de quem recebe a correção pode preservar uma célula que retém ou um fio incorreto. (`app/ponto/portas/celula.py:392`, `app/ponto/portas/celula.py:472`, `app/ponto/portas/celula.py:496`, `app/ponto/portas/celula.py:536`, `app/ponto/portas/celula.py:539`) |
| **GRADE** | Grade → dias internos do fechamento; `fora_vinculo` é excluído. Na emissão, esse tipo também retira o dia de trabalho. (`app/folha/export.py:338`, `app/ponto/services/fechamento.py:1043`, `app/chamados/services/disputa_emissao.py:83`) |

**Folha: fechamento, exportação, retenção, retrato e TXT.**

| ID | Sítio | Pergunta efetiva e predicado | F/Fio; caminho | Divergência |
|---|---|---|---|---|
| F01 | `app/folha/export.py:60` D | Sai pela rescisão? D não nula e **D ≤ fim da janela**. | **sim/não**; classificação rescisão em `app/folha/export.py:866` → TXT. | CE D; decide rota financeira mesmo havendo vínculo. |
| F02 | `app/folha/export.py:807`, `app/folha/export.py:810` A+S | Trava por âncora? A=True, 12x36, âncora nula, S=ativo, participante dos fechamentos. | **sim/não**; `sem_ancora_12x36` em `app/folha/export.py:835` → aprovação em `app/ponto/views.py:1296`. | CE S/A. |
| F03 | `app/folha/export.py:1068` D | Com previsto zero, chama de rescisão se **D < início da janela**; senão `sem_escala`. | não/não; motivo de prontidão retornado em `app/folha/export.py:1070`. | CE D. |
| F04 | `app/folha/export.py:1087` D | Prontidão: reduz `fim` se **D < fim**; se `fim < ini`, retorna rescisão. A grade posterior é pedida separadamente. | não/não; retorno em `app/folha/export.py:1089`, grade em `app/folha/export.py:1095`. | CE D; o corte pode colapsar a janela. |
| F05 | `app/folha/export.py:1208` D | Quais dias podem reter? Corta a janela até D **inclusive**; D anterior ao início produz vazio. | **sim/sim**; RET, também fornece os dias da folha ao vigia de alcance. (`app/folha/fora_da_folha.py:119`) | CE D. |
| F06 | `app/folha/export.py:1237` D | No próprio dia da demissão, célula ausente não retém por `sem_celula`; exige `d < hoje AND d != D`. | **sim/sim**; RET e dias da folha. | CE B: vínculo existe em D, mas a ausência de célula recebe exceção. |
| F07 | `app/folha/views.py:612` D | Raio-X considera furos até D inclusive; D anterior à janela elimina o trecho. | não/não; linhas de relatório em `app/folha/views.py:616`. | CE D. |
| F08 | `app/folha/services/ranking_he.py:133` A | Entra no filtro de ciclo do ranking conforme EC ativa atual. | não/não; lista em `app/folha/services/ranking_he.py:144`. | CE A. |
| F09 | `app/folha/views_retrato.py:107` A | Qual posto/praça/escala aparece no retrato? Última EC ativa. | não/não; enriquecimento em `app/folha/views_retrato.py:110`. | CE A. |
| P20 | `app/ponto/portas/celula.py:908` A | Materializa o dia? Após `vinculo_cobre_a_data`, EC inativa é pulada se **d ≤ hoje**, ou se não for a escolhida no futuro. | **sim/sim**, via célula criada em `app/ponto/portas/celula.py:914` e seus leitores RET/FIO. | CE A: vínculo encerrado válido hoje é aceito pelo juiz e pulado aqui. |
| P21 | `app/ponto/portas/regen_exportada.py:180` A | Estende horizonte da regeneração somente se A for verdadeira. | **sim/sim**; regeneração e recálculo em `app/ponto/portas/regen_exportada.py:179`, `app/ponto/portas/regen_exportada.py:184`; CEL. | CE A. |
| P34 | `app/ponto/services/fechamento.py:432`, `app/ponto/services/fechamento.py:440` S+D | Entra na competência se **S != desligado OR (S=desligado AND D ≥ início da janela da empresa)**. Sem limite superior; desligado com D nula não entra. | **sim/não**; UF. | Desligado com D nula e EC válido sai; ativo sem vínculo pode entrar. |
| P35 | `app/ponto/services/fechamento_lista.py:60` A | Qual EC rotula a linha do fechamento? A=True, mapa por colaborador. | não/não; classificação TXT é separada em `app/ponto/services/fechamento_lista.py:70`. | CE A. |
| P39 | `app/ponto/services/he_pendente_lavrado.py:92` S | Quem recebe foto de HE pendente na competência? S=ativo. | **sim/não**, condicional: foto → alvos de recusa automática em `app/ponto/management/commands/recusar_ponta_pequena.py:113`, `app/ponto/management/commands/recusar_ponta_pequena.py:170`; pendência viva pode reter TXT quando o portão está ligado. (`app/folha/porta_export.py:495`, `app/folha/porta_export.py:853`) | CE S; a foto pode omitir quem ainda tem dia pendente. |
| C35 | `app/escala/services/leitor_celula.py:389` D | Dia fica fora se **d > D OR (d = D AND não houve fato)**; há também ramo anterior à admissão. `_teve_fato` usa minutos realizados ou lâmpadas não missing. | **sim/sim**; vira `fora_vinculo` em `app/escala/services/leitor_celula.py:393` → GRADE. | CE B: D=data_fim, sem fato; o juiz inclui D e a grade exclui. |

O último sítio **não é o corte atual de retenção por célula**: sua influência comprovada é a grade/universo de dias do fechamento e da emissão. O retentor atual consulta células diretamente e tem os cortes próprios F05/F06. (`app/ponto/services/fechamento.py:1043`, `app/folha/export.py:1275`)

**Chamados, perguntas, adesão e decisões do fio.**

| ID | Sítio | Pergunta efetiva e predicado | F/Fio; caminho | Divergência |
|---|---|---|---|---|
| T03 | `app/colaboradores/queries.py:26` S | Universo comum: S=ativo, empresa ativa e usuário operacional. `em_uso` e `em_operacao` herdam esse corte atual. | **não/sim**; `em_operacao` em `app/colaboradores/queries.py:65` → detector em `app/ponto/management/commands/detectar_ausencias.py:100` → abertura em `app/ponto/management/commands/detectar_ausencias.py:694`. `em_uso` também alimenta alertas de turno. (`app/ponto/management/commands/processar_alertas_turno.py:50`, `app/ponto/management/commands/processar_alertas_turno.py:335`) | CE S, inclusive para ontem. |
| T07 | `app/colaboradores/services/vinculo.py:91` S | Pode receber escala? S=desligado veta o julgamento. | **não/sim**; impede o executor e o ramo de aceite que cria chamado. (`app/colaboradores/services/vinculo.py:390`, `app/colaboradores/services/vinculo.py:410`) | S desligado atual bloqueia pedido referente a início histórico com vínculo válido. |
| P24 | `app/ponto/services/afastado_avisa.py:41` S | Sem ausência confirmada pelo juiz, **S=afastado ainda devolve True**, com `situacao_sem_ausencia`. | **não/sim**; a triagem usa o resultado; o vigia também o interpreta como porta `afastado`. (`app/ponto/services/furos_sem_cobranca.py:90`, `app/ponto/services/furos_sem_cobranca.py:95`) | S=afastado, EC válido, nenhuma ausência cobrindo d. |
| P26 | `app/ponto/services/cartorio.py:1748` S | Emite furo/cobrança morta/limbo apenas se **S != desligado** e emissão não calada. | **não/sim**; guarda da emissão em `app/ponto/services/cartorio.py:1765`. | CE S. |
| P27 | `app/ponto/services/cartorio.py:1813` S | Emite furo parcial se FURO_PARCIAL, **S != desligado** e dentro do piso. | **não/sim**; `_abrir_chamado` em `app/ponto/services/cartorio.py:1828`. | CE S. |
| P28 | `app/ponto/services/cluster_espurio.py:183` S | Reconcilia cluster somente se S=ativo. | **não/sim**; fechamento ou abertura em `app/ponto/services/cluster_espurio.py:189`, `app/ponto/services/cluster_espurio.py:196`. | CE S. |
| P29 | `app/ponto/services/cluster_espurio.py:185` A | Reconcilia cluster somente com A=True **e data_fim nula**. | **não/sim**; mesmo caminho de P28. | EC datada válida é excluída, mesmo que A=True. |
| P36 | `app/ponto/services/furos_sem_cobranca.py:62` S | Vigia enxerga furo da janela atual até ontem somente de S=ativo. | **não/sim**; universo do vigia antes dos testes de chamado vivo/tranca. (`app/ponto/services/furos_sem_cobranca.py:83`) | CE S. |
| P38 | `app/ponto/services/geofence.py:46` S | Pode julgar divergência do vínculo GPS? S diferente de ativo retorna `None`. | **não/sim**; `None` é conservador e pode manter chamado vivo na reconciliação. (`app/ponto/services/geofence.py:67`, `app/chamados/reconciliador.py:300`) | CE S; histórico deixa de ser julgável. |
| P40 | `app/ponto/services/pautas_dos_discordantes.py:70` S | Quais dias discordantes sem fio entram na pauta da competência? Somente S=ativo. | **não/sim**; corta antes de `_fio_do_dia`. (`app/ponto/services/pautas_dos_discordantes.py:75`, `app/ponto/management/commands/pautas_discordantes.py:67`) | CE S. |
| P42 | `app/ponto/services/triagem_batida.py:92` A | Existe escala para acesso? Qualquer A=True; ausência gera diagnóstico de sem escala. | **não/sim**; cria chamado `acesso_bloqueado`. **A batida continua permitida.** (`app/ponto/services/triagem_batida.py:107`, `app/ponto/services/triagem_batida.py:123`) | CE A. |
| P44 | `app/ponto/services/vigia_de_hora.py:117` S | Vigia de hoje/ontem pula S=desligado. | **não/sim**; impede `julgar_celula` e emissão. (`app/ponto/services/vigia_de_hora.py:131`) | CE S, sobretudo ontem. |
| P47 | `app/ponto/services/vinculo_na_lista.py:152` S | Continua em revisão se **S != desligado** e posto continua igual ao da lavra. | **não/sim**, no ramo de geofence: revisão troca acusação/push por registro de divergência cadastral. (`app/ponto/services/geofence.py:77`, `app/ponto/services/geofence.py:308`, `app/ponto/services/geofence.py:319`) | CE S. |
| P48 | `app/ponto/supra_juiz.py:194` D | **Depois de o juiz devolver vínculo=None**, célula de trabalho só acusa cadastro se **d ≤ D ou D nula**. | **sim/sim**; insumo do classificador usado pelo cartório. (`app/ponto/supra_juiz.py:195`, `app/ponto/services/cartorio.py:1145`) | **Não diverge na existência do vínculo**: já recebeu None; acrescenta uma exceção própria de cobrabilidade pela demissão. |
| P52 | `app/chamados/detector_anomalias.py:60` A | Há colisão se mais de uma EC tem A=True e fim nulo, sem recorte pelo dia. | **não/sim**; anomalia → disputa na batida. (`app/api/views.py:434`, `app/api/views.py:757`, `app/api/views.py:764`) | Dois vínculos ativos de vigências disjuntas podem contar colisão; encerrado vigente desaparece. |
| P53 | `app/chamados/juizes.py:1809` A | Qual marco espera uma pergunta datada? EC ativa atual mais recente. | **não/sim**; fallback da defesa/classificação da resposta. (`app/chamados/juizes.py:1860`, `app/chamados/juizes.py:1990`) | Pergunta antiga passa a usar EC nova; CE A. |
| P54 | `app/chamados/models.py:1006` S | Pode renascer? `getattr(S,'') != ativo` retorna False. | **não/sim**; veta `reabrir`. (`app/chamados/models.py:1020`) | CE S. |
| P55 | `app/chamados/reconciliador.py:234` A | Ainda falta âncora se existe EC ativa com âncora nula; sem data do fato. | **não/sim**; premissa → decisão → retratação/fechamento. (`app/chamados/reconciliador.py:292`, `app/chamados/reconciliador.py:524`) | EC atual sem âncora pode sustentar premissa de chamado histórico. |
| P56 | `app/chamados/reconciliador.py:821` S | Reemite chamado do dia somente se S=ativo. | **não/sim**; retorna antes da costura/emissão. (`app/chamados/reconciliador.py:835`) | CE S. |
| P57 | `app/chamados/services/adesao.py:34` S | Universo de adesão na janela: célula `nunca_bateu` de S=ativo. | **não/sim**; sincronização → abertura do pai. (`app/chamados/services/adesao.py:59`, `app/chamados/services/adesao.py:75`) | CE S. |
| P58 | `app/chamados/services/adesao.py:67` S | Segundo gate: S=desligado pula adesão. | **não/sim**; impede abrir/reabrir pai. (`app/chamados/services/adesao.py:75`, `app/chamados/services/adesao.py:93`) | CE S; leitura própria mesmo após P57. |
| P62 | `app/chamados/services/disputa_emissao.py:1419` S | Havendo pergunta prévia, S diferente de ativo equivale a “já perguntou”. | **não/sim**; impede `PerguntaDisputa.abrir`. (`app/chamados/services/disputa_emissao.py:832`, `app/chamados/services/disputa_emissao.py:837`) | CE S. |
| P63 | `app/chamados/services/fabrica_da_celula.py:112` S | Motivos a perguntar ficam vazios se **S != ativo**, ou dia sem trabalho. | **não/sim**; motivos → nascimento da pergunta. (`app/chamados/services/fabrica_da_celula.py:207`, `app/chamados/services/fabrica_da_celula.py:217`) | CE S. |
| P64 | `app/chamados/services/fabrica_da_celula.py:201` S | Pauta cadastro sem turno exige célula passada, **S=ativo**, dia cobrável. | **não/sim**; ramo de pauta antes do retorno. (`app/chamados/services/fabrica_da_celula.py:203`) | CE S. |
| P66 | `app/chamados/services/pergunta_no_app.py:186` S | Vigia de cobradores sem pergunta no app mede somente S=ativo. | **não/sim**; achados → comando perguntar → criação de disputa. (`app/chamados/services/pergunta_no_app.py:200`, `app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:20`, `app/chamados/services/pergunta_no_app.py:240`) | CE S. |
| P67 | `app/chamados/views.py:442` A | Ação de corroborar régua escolhe EC ativa mais recente do template mencionado. | **não/sim**; passa esse vínculo à porta que altera o veto da cobrança. (`app/chamados/views.py:436`, `app/chamados/views.py:443`) | CE A; ação sobre fato antigo pode alcançar EC atual. |
| C23 | `app/escala/services/cadastro_realidade.py:186` S | Para S=desligado, C5 conserva somente dias segurados na lavra de retenção; demais dias são retirados. | **não/sim**; corte em `app/escala/services/cadastro_realidade.py:198` → `vigia_retido_calado` conta roteados/calados. (`app/folha/vigia_retido_calado.py:43`, `app/folha/vigia_retido_calado.py:48`) | CE S. |
| C25 | `app/escala/services/cadastro_realidade.py:477` S | Beco de folga considera células indefinidas da janela inclusiva somente de S=ativo. | **não/sim**; universo do beco → porta que reata perguntas. (`app/escala/services/cadastro_realidade.py:573`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:110`) | CE S. |

Estes sítios do domínio de chamados só alteram apresentação, opção ou diagnóstico no trecho observado:

| ID | Sítio | Pergunta/predicado; saída | F/Fio | Divergência |
|---|---|---|---|---|
| P23 | `app/ponto/services/abriu_nao_bateu.py:194` A | Já passou a hora? Primeira ativa; sem EC retorna True e afeta contador. (`app/ponto/services/abriu_nao_bateu.py:196`) | não/não | CE A. |
| P30 | `app/ponto/services/cluster_espurio.py:260` S | Flag de simulação `ativo = S == ativo`. | não/não | CE S; é flag diagnóstica. |
| P37 | `app/ponto/services/furos_vetados.py:62` S | Relatório de vetados exclui S=desligado. | não/não | CE S. |
| P43 | `app/ponto/services/triagem_batida.py:333` A | Primeira ativa fornece título/rótulo da anuência; o chamado é criado independentemente. (`app/ponto/services/triagem_batida.py:335`) | não/não | CE A. |
| P45 | `app/ponto/services/vinculo_na_lista.py:71` S | Achado GPS/cadastro exclui S=desligado. | não/não | CE S. |
| P46 | `app/ponto/services/vinculo_na_lista.py:83` S | Evidência por colegas do posto considera apenas S=ativo. | não/não | CE S. |
| P59 | `app/chamados/services/desvio_escala.py:44` A | Texto de desvio dos últimos 14 dias exige EC ativa; sem ela retorna None. (`app/chamados/services/desvio_escala.py:45`) | não/não | CE A. |
| P60 | `app/chamados/services/desvio_escala.py:106`, `app/chamados/services/desvio_escala.py:107` A+S | Colegas do mesmo template: A=True e S=ativo; alimentam texto explicador. | não/não | CE S/A. |
| P61 | `app/chamados/services/disputa_emissao.py:341` A | Opções da pergunta usam A=True e fim nulo; não decide nascimento nesse ponto. | não/não | CE A. |
| P65 | `app/chamados/services/limbo_folgas.py:65` A | Alcance do template conta vínculos A=True para texto de sugestão. | não/não | CE A. |

**App/API.**

| ID | Sítio | Pergunta/predicado | F/Fio; caminho | Divergência |
|---|---|---|---|---|
| A01 | `app/api/login_helpers.py:141` S | Autentica somente se `getattr(S,'ativo') == ativo`. | não/não; recusa de login. Efeito em posterior requisição do fio é [não verificado]. | Afastado com vínculo válido hoje é recusado; CE S. |
| A02 | `app/api/views.py:116` S | Refresh exige S=ativo; senão 401. | não/não; acesso. | CE S. |
| A03 | `app/api/views_core.py:172` S | Mesmo gate de refresh no kernel core. | não/não; acesso. | CE S. |
| A04 | `app/api/views.py:403` S | Pode bater somente se **S ∈ {ativo, afastado

[truncado em 20000 de 58730 caracteres]

## 13/11 11:43 · sessao ccecfb700358 · FERIADO-USA-A-LEI-DE-1609 · verificador · origem=Code

**REPROVA:** o delta corrige os casos nomeados do item B, mas restam formas não declaradas que passam pelo selo; além disso, preservar a célula exportada não garante preservar os números dos leitores vivos (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:87`, `:137`; `app/ponto/services/fechamento.py:949`).

**A. O reparo cobre os casos da volta 2, mas a catraca continua incompleta.**

| Caso | Morde pelo motivo anunciado? | Evidência |
|---|---|---|
| t1 | Sim: nascimento pelo reverso `escalas`, sem flag. | `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:127`, `:312`, `:347` |
| t2 | Sim: reverso `vinculos`, inclusive pela cadeia `filter().get_or_create()`. | Mesmo arquivo`:122`, `:313`, `:348` |
| t3 | Sim: `defaults` escreve a flag do existente, embora o nascimento por `create_defaults` seja válido. | Mesmo arquivo`:207`, `:314`, `:349` |
| t4 | Sim: `defaults` sobrescreve a keyword de busca; acusa `True`. | Mesmo arquivo`:159`, `:316`, `:350` |
| t5 | Sim: `**kw` posterior torna a flag opaca. | Mesmo arquivo`:143`, `:318`, `:351` |
| t6 | Sim: última chave literal vence; acusa `True`. | Mesmo arquivo`:145`, `:320`, `:352` |
| t7 | Sim: reconhece `acreate` sem flag. | Mesmo arquivo`:57`, `:322`, `:353` |
| t8 | Sim: reconhece escrita crua por `aupdate`. | Mesmo arquivo`:59`, `:215`, `:323`, `:354` |
| t9 | Sim: encontra a atribuição dentro do desempacotamento. | Mesmo arquivo`:172`, `:224`, `:324`, `:355` |
| u1 | Sim: reconhece `get_model` com nome posicional. | Mesmo arquivo`:96`, `:325`, `:356` |
| u2 | Sim: rastreia o alias anotado. | Mesmo arquivo`:87`, `:326`, `:357` |
| u3 | Não morde corretamente: primeiro argumento só atualiza `ativa`; segundo fornece `create_defaults` com cópia válida. | Mesmo arquivo`:150`, `:164`, `:207`, `:328` |
| u4 | Não morde corretamente: reconhece o reverso e aceita a flag explícita copiada. | Mesmo arquivo`:105`, `:127`, `:330` |

Há, pelo menos, duas formas possíveis de produção omitidas de `LIMITES`:

```python
(EC := EscalaColaborador).objects.create(colaborador=c)
```

O alias por `NamedExpr` não é rastreado: `_apelidos` cobre `Assign` e `AnnAssign`; `_eh_modelo` não reconhece esse receptor. Assim o nascimento pode escapar sem flag explícita, usando o default ainda existente (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:87`, `:96`, `:125`; `app/escala/models.py:896`). Essa forma não consta dos limites (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:34`).

```python
campo = 'trabalha_em_feriado'
EC.objects.get_or_create(
    colaborador=c,
    defaults={
        'trabalha_em_feriado': origem.trabalha_em_feriado,
        campo: True,
    },
)
```

A chave dinâmica posterior sobrescreve a literal, mas `_do_dict` ignora chaves que não sejam constantes ou `**`; conserva a cópia aceita e deixa passar o nascimento com `True`. Também não está declarado em `LIMITES` (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:137`, `:105`, `:199`, `:34`).

São contraexemplos da cobertura do selo; ocorrência dessas formas em um escritor já existente na produção: **[não verificado]**. Os quatro nascimentos explícitos identificados na cópia perguntam ao juiz ou copiam a flag de origem (`app/colaboradores/services/vinculo.py:296`, `:975`, `:1093`, `:1114`).

**B. A-3 e F-6 sustentam-se como FILA; nenhum bloqueia isoladamente este pouso.** [nao verificado]

A-3 é real: `_o_chao_do_dia_mudou` relê `timezone.localdate()` no interior. Porém o teto declara exatamente esse sítio, e o complemento proíbe ampliar a cura naquela volta; o BACKLOG registra a pendência (`app/ponto/portas/celula.py:464`; `app/ponto/tests/test_cadastro_futuro_c5e.py:316`, `:368`; `app/docs/BACKLOG.md:2062`). O lote transmite seu `hoje` à porta e à regeneração (`app/escala/management/commands/aplicar_feriado_lei_1609.py:48`, `:73`; `app/colaboradores/services/vinculo.py:905`).

F-6 também existe: `sincronizar_feriado_admin` decide pelo cargo e aplica pela porta. Esse comando não integra a cadeia do apply desta obra, cujo contador pergunta ao juiz de 16/09 (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:19`, `:47`, `:58`; `app/escala/management/commands/aplicar_feriado_lei_1609.py:46`, `:73`; `app/escala/models.py:960`). A classificação histórica como preexistente vem da triagem fornecida: **[não verificado]** por histórico da cópia.

**C. Escritores, leitores e significado.**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `trabalha_em_feriado` no nascimento | `executar_vinculo` pergunta ao juiz; trecho retroativo, ajuste e desfazer copiam a origem. | `app/colaboradores/services/vinculo.py:296`, `:975`, `:1093`, `:1114` |
| Flag de vínculo existente | Atribuição e `save` em `marcar_trabalha_em_feriado`. | `app/colaboradores/services/vinculo.py:893` |
| Flag para decidir/corrigir | Contador compara com o juiz; comando legado compara com cargo; porta compara antes de salvar. | `app/escala/models.py:960`; `app/colaboradores/management/commands/sincronizar_feriado_admin.py:47`; `app/colaboradores/services/vinculo.py:890` |
| Flag para efeito no dia/dinheiro | `feriado_vira_folga`, usado pelo gerador, precedência e separação das rubricas. | `app/escala/models.py:1406`, `:1427`; `app/ponto/precedencia.py:377`; `app/ponto/services/fechamento.py:949` |
| `minutos_previstos` / `previsto_em` | Recalculo soma a grade e carimba; falha produz `None`; persiste por `save` ou `update`. | `app/ponto/services/fechamento.py:1048`, `:1083`, `:1094`, `:1144`, `:1181`, `:1188` |
| Invalidação de `previsto_em` | Signal chama `invalidar_previsto`, que zera o carimbo e avança `atualizado_em`. | `app/escala/signals.py:138`; `app/ponto/services/fechamento.py:1607` |
| Restauração desses campos | Porta genérica restaura os campos pedidos. | `app/ponto/services/fechamento.py:1380`, `:1393` |
| Leitura do previsto mensal | Prontidão; cartão/espelho; fotografia e hash dos campos gravados. | `app/folha/export.py:1045`; `app/relatorios/cartao_pela_celula.py:117`; `app/chamados/services/foto_e4b.py:290`; `app/ponto/portas/regen_exportada.py:58` |

`previsto_em` significa frescor do previsto; `minutos_previstos` mede minutos da grade, convertidos em horas pelos consumidores. Não demonstram congelamento da competência nem frescor do registro inteiro (`app/ponto/models.py:430`, `:454`; `app/folha/export.py:1050`; `app/relatorios/cartao_pela_celula.py:117`).

A divergência relevante é temporal: a célula conserva a decisão histórica, enquanto a separação de rubricas consulta a flag atual da linha do vínculo que responde pelo dia (`app/escala/models.py:1131`; `app/escala/servico_jornada.py:61`; `app/ponto/services/fechamento.py:949`). Portanto, os leitores não garantem a mesma fotografia histórica.

**D. Classes de reprovação.**

- **Segundo juiz:** existe a decisão por cargo, fora do caminho deste lote; o lote usa o juiz existente (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:47`; `app/escala/models.py:960`).
- **Segundo escritor fora da porta:** não aparece no apply; ele chama a porta, onde ficam atribuição e `save` (`app/escala/management/commands/aplicar_feriado_lei_1609.py:73`; `app/colaboradores/services/vinculo.py:893`). A garantia AST continua incompleta pelos contraexemplos de A (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:87`, `:137`).
- **Relógio interior:** presente, explicitamente em fila, conforme B (`app/ponto/portas/celula.py:464`; `app/docs/BACKLOG.md:2062`).
- **Derivação paralela:** `fechamento_lido` chama o próprio recalculador; não cria outra conta. O problema demonstrado é usar cadastro atual dentro dessa conta histórica (`app/ponto/services/fechamento.py:1759`, `:949`).
- **Selo/prova sem cobertura suficiente:** os novos mutantes têm expectativas específicas; o hash do gravado, porém, não cobre os números derivados pelos leitores vivos (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:347`; `app/ponto/portas/regen_exportada.py:37`, `:71`; `app/chamados/services/foto_e4b.py:328`).
- **Pré-requisito:** o comando verifica o universo e fecha o arquivo exclusivo de reversão antes da primeira mutação (`app/escala/management/commands/aplicar_feriado_lei_1609.py:64`, `:69`, `:73`). Inversão concreta de travas, pouso efetivo e cumprimento operacional dos pré-requisitos: **[não verificado]**.
- **Campo com dois significados:** não demonstrei duas unidades para o previsto; demonstrei mistura entre decisão congelada e flag atual no significado histórico do feriado (`app/ponto/models.py:432`; `app/escala/models.py:1131`, `:1427`; `app/ponto/services/fechamento.py:949`).

**G. A exportada pode mudar nos leitores vivos.**

O encadeamento informado procede: o apply salva por vínculo; o signal invalida todo fechamento do colaborador com `previsto_em`, sem filtro de competência. Essa operação altera carimbos, conservando os campos numéricos gravados (`app/escala/management/commands/aplicar_feriado_lei_1609.py:73`; `app/colaboradores/services/vinculo.py:894`; `app/escala/signals.py:146`; `app/ponto/services/fechamento.py:1611`).

| Leitor, depois do lote | Fonte do número |
|---|---|
| `folha.export.prontidao` | Sem carimbo, calcula pela grade; `trab` continua vindo de `fech.horas_trabalhadas`. `app/folha/export.py:1036`, `:1045`, `:1095` |
| Lista do fechamento | Campos do FM gravado, com prontidão calculada separadamente. `app/ponto/views.py:1135`; `app/ponto/services/fechamento_lista.py:85`, `:107` |
| Painel lateral | Cálculo vivo por `fechamento_lido`. `app/ponto/views.py:993` |
| PDF do TXT histórico | Snapshot de `ExportacaoDominio.conteudo`. `app/folha/pdf_export.py:63` |
| PDF cartão/espelho | Totais vivos por `fechamento_lido`. `app/relatorios/pdf_espelho.py:628`; `app/relatorios/cartao_pela_celula.py:73`, `:323` |
| TXT histórico baixado | Conteúdo gravado. `app/folha/views.py:561` |
| Novo TXT da mesma competência | Cálculo vivo injetado na montagem. `app/folha/views.py:208`, `:268` |
| `foto_e4b` | Guarda FM gravado inteiro **e** `dinheiro_online` vivo. `app/chamados/services/foto_e4b.py:290`, `:328` |
| `regen_exportada.hash_do_gravado` | Campos gravados; exclui `previsto_em` e `atualizado_em`. `app/ponto/portas/regen_exportada.py:37`, `:71` |
| `regenerar_em_exportada`, se executada depois | Ato adicional que regenera apesar da lavra e recalcula. `app/ponto/portas/regen_exportada.py:179`, `:184` |

**O previsto da prontidão lê a célula preservada.** A grade usa `ata.tipo_dia`/`cel.trabalha` e os minutos da ata; no fallback do previsto, `eh_dia_trabalho` devolve diretamente `cel.trabalha`. Portanto, a flag nova, sozinha, não reduz esse previsto quando célula/ata estão preservadas e coerentes (`app/escala/services/leitor_celula.py:397`, `:411`; `app/escala/utils.py:2266`; `app/escala/models.py:1131`). Contudo, se o snapshot mensal já divergir da grade, invalidar o carimbo muda a fonte exibida; a invalidação não verifica essa igualdade (`app/folha/export.py:1045`, `:1095`; `app/ponto/services/fechamento.py:1611`). Existência dessa divergência no lote real: **[não verificado]**.

**O dinheiro vivo também lê a flag atual.** Considere vínculo weekday corrigido de `True` para `False`, em competência exportada, com feriado já congelado como folga, par trabalhado e escala certa. O motor continua colocando o par em `periodos_ft`, pois consulta a célula preservada (`app/ponto/motor_calculo_v2.py:2448`, `:2492`). Mas `feriado_vira_folga` passa de falso para verdadeiro pela alteração da flag (`app/escala/models.py:1427`, `:1432`).

Consequência: as horas desse par saem de `horas_folga_trabalhada` e entram em `horas_extras_100_feriado`, `horas_extras_100` e `horas_extras`; o subconjunto noturno também pode mudar. Isso ocorre na leitura, sem regenerar a célula exportada ([app/ponto/services/fechamento.py:949](/home/ronald/codex_ws/b4e060c4bda347dc5302a7805ff28eb37b07e311/app/ponto/services/fechamento.py:949), `:955`, `:962`, `:965`).

Assim, painel, cartão PDF e fotografia online podem mudar; um novo TXT pode refletir a mudança conforme as rubricas cadastradas. TXT/PDF históricos continuam mostrando o snapshot (`app/ponto/views.py:993`; `app/relatorios/cartao_pela_celula.py:73`; `app/chamados/services/foto_e4b.py:328`; `app/folha/export.py:443`, `:446`; `app/folha/views.py:561`; `app/folha/pdf_export.py:63`). Ocorrência e valores concretos desse cenário na produção: **[não verificado]**.

**REPROVA — A: lacunas não declaradas da catraca (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:87`, `:137`); G/F-4: números vivos de exportada podem mudar, sem cobertura pelo hash do gravado (`app/ponto/services/fechamento.py:949`; `app/ponto/portas/regen_exportada.py:37`).**

## 13/11 11:40 · sessao 7795cd9bc9de · - · - · origem=Ronald (tmux)

G: há cenário de mudança de número de competência exportada, mesmo com célula preservada: o cálculo vivo reclassifica folga trabalhada usando a flag atual do vínculo. A ocorrência desse cenário na produção raspada é **[não verificado]**; o caminho existe no código (`app/ponto/services/fechamento.py:949`, `app/escala/models.py:1427`).

- O apply chama a porta por vínculo (`app/escala/management/commands/aplicar_feriado_lei_1609.py:73`). A porta salva a flag (`app/colaboradores/services/vinculo.py:893`), disparando o signal (`app/escala/signals.py:137`), que invalida **todos** os fechamentos com `previsto_em`, sem filtro de competência exportada: zera o carimbo e altera `atualizado_em`, conservando os números gravados (`app/ponto/services/fechamento.py:1611`).
- A guarda preserva células da exportada: encontra as competências exportadas (`app/ponto/portas/celula.py:221`) e pula seus dias antes da geração (`app/ponto/portas/celula.py:284`).

| Leitor | Fonte após o lote |
|---|---|
| `folha.export.prontidao` | Com carimbo invalidado, calcula pela grade; o snapshot só vale com `previsto_em` (`app/folha/export.py:1045`, `app/folha/export.py:1095`). |
| Lista de fechamento | Conserva FM gravado e chama prontidão para a coluna correspondente (`app/ponto/views.py:1135`, `app/ponto/services/fechamento_lista.py:85`). |
| Painel lateral | `fechamento_lido`, cálculo vivo (`app/ponto/views.py:993`, `app/ponto/services/fechamento.py:1759`). |
| PDF do TXT histórico | `ExportacaoDominio.conteudo`, snapshot (`app/folha/pdf_export.py:63`, `app/folha/views.py:834`). |
| PDF cartão/espelho | Totais vivos: PDF chama `folha_manda`, que chama `totais_da_folha`, que chama `fechamento_lido` (`app/relatorios/pdf_espelho.py:628`, `app/relatorios/cartao_pela_celula.py:323`, `app/relatorios/cartao_pela_celula.py:73`). |
| TXT histórico baixado | Conteúdo gravado intacto (`app/folha/views.py:561`). |
| Novo TXT da mesma competência | Recalcula em leitura e injeta esses objetos na montagem (`app/folha/views.py:208`, `app/folha/views.py:268`). |
| `foto_e4b` | Fotografa FM gravado inteiro **e**, separadamente, `dinheiro_online` vivo (`app/chamados/services/foto_e4b.py:290`, `app/chamados/services/foto_e4b.py:328`). |
| `regen_exportada.hash_do_gravado` | Hash dos números gravados; exclui precisamente os carimbos alterados pela invalidação (`app/ponto/portas/regen_exportada.py:37`, `app/ponto/portas/regen_exportada.py:71`). |
| `regenerar_em_exportada` executada depois | É ato adicional que abre a lavra, regenera e recalcula o FM (`app/ponto/portas/regen_exportada.py:179`, `app/ponto/portas/regen_exportada.py:184`). |

**Previsto da prontidão:** o fallback lê `ata.tipo_dia`/`cel.trabalha` e `ata.minutos_previstos`; sem minutos lavrados, chama o juiz (`app/escala/services/leitor_celula.py:397`, `app/escala/services/leitor_celula.py:411`). O juiz consulta `eh_dia_trabalho` (`app/escala/utils.py:2266`), que devolve diretamente `cel.trabalha` quando existe célula (`app/escala/models.py:1131`). Portanto, **não há cadeia que faça apenas a flag nova reduzir o previsto de uma célula/ata preservada e coerente**; `precedencia.py` não entra nesse leitor (`app/escala/services/leitor_celula.py:397`).

**Contraprova de invariância dos demais números:** considere vínculo do lote weekday com `True → False`, em feriado que já estava congelado como folga, com par trabalhado de `h` horas e escala certa. O contador permite tal correção (`app/escala/models.py:960`); o motor mantém o par em `periodos_ft` porque lê a célula como folga (`app/ponto/motor_calculo_v2.py:2448`, `app/ponto/motor_calculo_v2.py:2492`). Antes, `feriado_vira_folga` devolve `False` pela flag `True`; depois devolve `True` pela flag `False` e feriado do lugar (`app/escala/models.py:1427`, `app/escala/models.py:1432`). O fechamento vivo retira essas `h` horas de `horas_folga_trabalhada`, adiciona-as a `horas_extras_100`, `horas_extras_100_feriado` e `horas_extras`, e pode mover também o subconjunto noturno (`app/ponto/services/fechamento.py:949`, `app/ponto/services/fechamento.py:955`, `app/ponto/services/fechamento.py:962`, `app/ponto/services/fechamento.py:965`).

Assim, painel, cartão PDF, fotografia online e eventual novo TXT podem mudar; TXT/PDF históricos conservam a fotografia anterior (`app/ponto/views.py:993`, `app/relatorios/cartao_pela_celula.py:73`, `app/chamados/services/foto_e4b.py:328`, `app/folha/views.py:208`, `app/folha/views.py:561`, `app/folha/pdf_export.py:63`). O novo TXT distingue os eventos de feriado e folga trabalhada, embora rubricas cadastradas possam fazê-los convergir no arquivo — essa configuração é **[não verificado]** (`app/folha/export.py:443`, `app/folha/export.py:446`).

O hash inalterado do gravado prova apenas que seus campos incluídos não mudaram; **não prova invariância dos leitores vivos** (`app/ponto/portas/regen_exportada.py:37`, `app/ponto/portas/regen_exportada.py:71`).

## 13/11 11:40 · sessao 1911af74687d · - · - · origem=Ronald (tmux)

A cura os casos descritos na volta 2 e os mutantes novos mordem os motivos esperados. Reexecutei o scanner em memória, sem criar arquivos: `achados(app) == []`, contagem do escritor autorizado igual a 1, e a lista esperada integral dos mutantes coincidiu. Evidência da verificação pode ser reproduzida pelo scanner e pelas expectativas em `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:183`, `:253`, `:260`, `:333`.

| Caso | Resultado e motivo | Referências |
|---|---|---|
| t1/t2 | Acusam nascimento sem flag pelos reversos `escalas`/`vinculos`, inclusive cadeia `filter().get_or_create()`. | selo `:122`, `:127`, `:312`, `:313`, `:347`, `:348`; reversos reais `app/escala/models.py:872`, `:892`. |
| t3 | Acusa escrita do existente por `defaults`; nascimento com `create_defaults` copiado permanece aceito. | selo `:159`, `:207`, `:314`, `:349`. |
| t4 | `defaults` sobrescreve a flag de busca e o valor literal `True` é acusado. | selo `:159`, `:205`, `:316`, `:350`. |
| t5 | `**kw` posterior à flag torna o valor opaco e é acusado. | selo `:137`, `:143`, `:203`, `:318`, `:351`. |
| t6 | Última chave literal vence; acusa `True`. | selo `:142`, `:145`, `:320`, `:352`. |
| t7/t8 | `acreate` acusa ausência da flag; `aupdate` acusa escrita crua. | selo `:55`, `:59`, `:196`, `:215`, `:322`, `:353`. |
| t9 | Atribuição desempacotada acusa escrita fora da porta. | selo `:172`, `:224`, `:324`, `:355`. |
| u1/u2 | `get_model` com nome em argumento posicional e alias anotado são reconhecidos e acusam ausência da flag. | selo `:87`, `:96`, `:325`, `:356`. |
| u3 | Limpo pelo motivo certo: segundo argumento fornece `create_defaults` com cópia; primeiro `defaults` só contém `ativa`. | selo `:150`, `:164`, `:207`, `:328`. |
| u4 | Limpo pelo motivo certo: reverso é reconhecido, e nascimento recebe cópia aceita. | selo `:105`, `:127`, `:330`. |

Persistem formas possíveis em código de produção que o selo não acusa e `LIMITES` não declara — **não encontrei ocorrência dessas formas no HEAD**; ocorrência real é `[não verificado]`:

1. **Chave dinâmica posterior em `defaults`:**
   ```python
   campo = 'trabalha_em_feriado'
   EC.objects.get_or_create(
       colaborador=c,
       defaults={'trabalha_em_feriado': origem.trabalha_em_feriado, campo: True},
   )
   ```
   O scanner retorna `[]`: `_do_dict` ignora a chave dinâmica e conserva a cópia aceita, embora a última chave efetiva seja `True`. Trata `**` como opaco e constantes como chaves conhecidas, sem tratamento das demais chaves. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:137`, `:143`, `:145`, `:105`, `:199`; limites enumerados em `:34`.

2. **Alias por expressão de atribuição:**
   ```python
   (EC := EscalaColaborador).objects.create(colaborador=c)
   ```
   Scanner retorna `[]`: rastreia `Assign` e `AnnAssign`, mas não `NamedExpr`; `_eh_modelo` não reconhece esse receptor. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:87`, `:96`, `:125`; limites em `:34`. Esse contraexemplo reproduz diretamente a volta do default proibido, ainda presente em `app/escala/models.py:896`.

3. **`get_model` com keywords:**
   ```python
   apps.get_model(app_label='escala', model_name='EscalaColaborador').objects.create(colaborador=c)
   ```
   Scanner retorna `[]`, pois `_eh_modelo` só consulta `no.args`, não keywords. Compatibilidade da chamada com implementação Django não incluída na cópia: `[não verificado]`; dependência declarada é Django 5.2.14 em `app/requirements.txt:13`. Falha do reconhecimento AST é verificável em `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:96`; ausência no inventário em `:34`.

Os quatro nascimentos explícitos atuais continuam corretos: pergunta ao juiz em `app/colaboradores/services/vinculo.py:287`/`:296`, cópias em `:971`/`:975`, `:1090`/`:1093`, `:1111`/`:1114`.

## 13/11 11:40 · sessao 4aceadf256c0 · - · - · origem=Ronald (tmux)

Censo entregue em cinco partes: **75 sítios**, com **42 leituras de `situacao`, 1 de `data_demissao` e 41 de `ativa`**; nove sítios leem dois campos. A lista completa, predicados, caminhos e exclusões estão nas mensagens anteriores. [não verificado]

Achados prioritários:

- `data_demissao` altera o dia para `fora_vinculo`, alcançando folha e emissão: `app/escala/services/leitor_celula.py:365`, `:389`; `app/folha/export.py:338`; `app/chamados/services/disputa_emissao.py:83`.
- Situação atual modifica o roteamento C5 usado pelo vigia de retenção silenciosa: `app/escala/services/cadastro_realidade.py:186`, `:198`; `app/folha/vigia_retido_calado.py:43`, `:48`.
- Situação atual corta o universo do beco cuja correção reata perguntas: `app/escala/services/cadastro_realidade.py:477`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:110`.
- Há cortes crus nos universos de emissores e reconciliadores: `app/ponto/management/commands/reconciliar_grade.py:106`; `app/ponto/management/commands/supra_juiz.py:110`; `app/ponto/management/commands/detectar_cluster_espurio.py:72`; `app/ponto/management/commands/detectar_par_relampago.py:337`; `app/ponto/management/commands/emitir_furo_retroativo.py:52`.

AST executada em memória sobre 290 arquivos do escopo delegado, sem erros de parsing; nenhum arquivo foi escrito e nenhum acesso a banco/log foi realizado. [não verificado]

## 13/11 11:39 · sessao 3d4ef57f7cae · - · - · origem=Ronald (tmux)

B — **A-3 sustenta-se como FILA.** Há releitura interior em `app/ponto/portas/celula.py:464`; o teto aceita exatamente uma nessa função, expressamente “fora da lista do aval” (`app/ponto/tests/test_cadastro_futuro_c5e.py:368`). A proibição de ampliar a cura está literal em `app/ponto/tests/test_cadastro_futuro_c5e.py:316`–325; a fila nomeada registra inclusive o caminho de update e essa releitura (`app/docs/BACKLOG.md:2062`). O lote lê `hoje` na borda e o passa à porta (`app/escala/management/commands/aplicar_feriado_lei_1609.py:48`, `:73`), que o transmite à regeneração (`app/colaboradores/services/vinculo.py:905`). Logo, o defeito existe no caminho, mas sua correção foi explicitamente deferida; isoladamente não bloqueia esta obra (`app/ponto/tests/test_cadastro_futuro_c5e.py:317`).

**F-6 também tem fundamento de código para permanecer fora deste lote.** `cargo_e_administrativo` decide pelo cadastro de cargo (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:19`), o comando deriva `desejado` por ele (`:47`) e aplica pela porta (`:58`). O apply desta obra importa e chama diretamente `marcar_trabalha_em_feriado` (`app/escala/management/commands/aplicar_feriado_lei_1609.py:46`, `:73`); seu contador pergunta ao juiz de 16/09 (`app/escala/models.py:960`). O comando por cargo não é chamado pelo corpo do apply (`app/escala/management/commands/aplicar_feriado_lei_1609.py:43`). Sua condição histórica de “preexistente” está no anexo fornecido, mas não consegui verificá-la por histórico: a cópia não contém repositório Git [não verificado]. Nenhum registro nominal específico de F-6 foi localizado em BACKLOG/CORTES [não verificado].

D — **Classes presentes nos sítios examinados, com delimitação:**

- **Relógio interior:** presente, porém explicitamente em fila, conforme acima (`app/ponto/portas/celula.py:464`; `app/docs/BACKLOG.md:2062`).
- **Outra regra para decidir a flag:** presente no comando por cargo, fora da cadeia do apply (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:47`; `app/escala/management/commands/aplicar_feriado_lei_1609.py:73`). O lote usa a autoridade existente, com posto declarado vencendo (`app/escala/models.py:911`, `:960`).
- **Segundo escritor introduzido no apply:** não identificado; ele chama a porta (`app/escala/management/commands/aplicar_feriado_lei_1609.py:73`), e a atribuição/save ficam nela (`app/colaboradores/services/vinculo.py:893`–894).
- **Pouso antes da reversão:** o código estabelece a ordem correta: igualdade do universo antes de prosseguir (`app/escala/management/commands/aplicar_feriado_lei_1609.py:64`), arquivo exclusivo de reversão criado e fechado antes do primeiro chamado mutador (`:69`–73). Execução efetiva e pouso não foram demonstrados pelo dado [não verificado].
- **Ordem de trava:** a porta da flag executa em `atomic` (`app/colaboradores/services/vinculo.py:868`), salva a flag antes da regeneração (`:894`, `:905`); o suplemento futuro trava células ordenadas por data (`app/ponto/services/cadastro_futuro.py:30`, `:36`). Não foi demonstrada inversão concreta de travas nesse caminho [não verificado].
- **Semântica/snapshot:** a promessa da porta de que “a exportada fica” é limitada à guarda da célula (`app/colaboradores/services/vinculo.py:878`, `:887`); o save dispara invalidar previsto em qualquer fechamento com carimbo (`app/escala/signals.py:137`, `:146`; `app/ponto/services/fechamento.py:1611`–1613). Portanto, não usar essa promessa como prova de que nenhum número lido muda; G precisa decidir isso pelo leitor.

A fixture semanal agora declara `dias_trabalho_semana` (`app/escala/tests/test_feriado_lei_1609.py:52`), preenchimento exigido pelo modelo para semanal (`app/escala/models.py:712`). Resultado green efetivamente executado nesta cópia [não verificado].

## 13/11 11:39 · sessao 5ddc4f3f350c · - · - · origem=Ronald (tmux)

Censo do recorte `app/folha`, `app/chamados`, `app/ponto`, **fora de management/, tests/, migrations/ e arquivos test*.py**: **67 sítios**, sendo **situacao=33, data_demissao=9, ativa=28**; três sítios leem dois campos. Esta conta inclui o booleano diagnóstico de `cluster_espurio.py:260`; excluindo esse retrato puramente informativo: **66 sítios, S=32, D=9, A=28**. A unidade é um predicado independente, agrupando uma query multilinha e seus `Q` numa linha do censo. [não verificado: resultado da execução AST em memória, sem artefato criado]

Método executado: `ast.parse` de todos os `.py` do recorte, seleção de `Attribute` em `Load` para os três campos, keywords ORM por segmentos de `__` e strings exatas/lookup em `Constant`; inspeção manual do proprietário do campo, aliases de import (`_ECtv`, `_Qc`, `_EC_i`, `_ECr`, `_EC`) e propagação de `data_demissao` ao argumento de `_insumos_do_vinculo`. Não importei Django nem abri banco. [não verificado: procedimento executado em memória]

Convenções dos contraexemplos, todos hipotéticos:

- **S**: situação ATUAL `desligado` ou `afastado`, dia anterior à demissão/afastamento com EC válido; o campo exclui, a juíza ainda pode devolver vínculo. O juiz verifica vigência/geradora, não situação. `app/escala/alimentacao.py:196`, `app/escala/alimentacao.py:250`.
- **A**: EC `ativa=False`, `data_fim` preenchida e cobrindo o dia; query `ativa=True` perde o EC que a regra `ativa or data_fim` admite. Também EC `ativa=True` de início futuro pode ser eleito sem cobrir hoje. `app/escala/alimentacao.py:196`.
- **D**: `Colaborador.data_demissao=10/11`, EC ainda cobrindo 11/11; leitura da demissão exclui o dia, o juiz do vínculo pode aceitá-lo. Não afirmo que este estado existe em produção. `app/escala/alimentacao.py:196`; [não verificado: ocorrência em produção].

Abaixo, **F** significa caminho comprovado até folha/TXT/universo; **I** significa caminho comprovado até fio/chamado/pergunta/adesão; **N** significa sem caminho de trava identificado no código inspecionado. Datas de contraexemplos são hipóteses, não dado de produção. [não verificado: convenção de apresentação]

| # | Sítio/campo | Pergunta e predicado exato | Trava/caminho | Divergência |
|---|---|---|---|---|
| 1 | `app/folha/export.py:60` D | Sai pela rescisão: demissão não nula e `<= fim_janela` | **F**: `sai_pela_rescisao` → `classificar_export` fora/rescisão em `:866-871` → conjunto TXT `:714-727` | D; é juiz de rescisão, não juiz geral de desligado |
| 2 | `app/folha/export.py:807` A+S (`:810`) | Trava de âncora: EC ativa, ciclo12x36, âncora nula, colaborador ATUAL ativo, dentro dos fechamentos | **F**: ids → `sem_ancora_12x36` `:835-848`; aprovação lê em `app/ponto/views.py:1296-1298` | A e S |
| 3 | `app/folha/export.py:1068` D | Prontidão com previsto zero: rótulo rescisão se demissão `< início`, senão sem escala | **N**, devolve motivo de prontidão `:1070-1071`; consumidor de apresentação em `app/ponto/services/fechamento_lista.py:50` | D |
| 4 | `app/folha/export.py:1087` D | Prontidão ao vivo: fim=min(fim,demissão), inclusivo; janela colapsada retorna rescisão `:1089-1091` | **N** direto: cálculo/rótulo de prontidão; a grade é obtida separadamente em `:1095` | D |
| 5 | `app/folha/export.py:1208` D | Universo de retenção cortado até demissão inclusiva; anterior ao início dá vazio `:1210-1213` | **F**: `motivos_retencao_celula` → `motivos_retencao_espelho` `:1275` → `classificar_export` `:968` → TXT `:716-727` | D |
| 6 | `app/folha/export.py:1237` D | Dia igual à demissão não retém por célula ausente: `c is None and d<hoje and not _dia_dem` `:1238-1240` | **F**, mesma cadeia da linha anterior | Juiz pode devolver EC em 10/11, mas sítio abre exceção nesse dia; não é predicado equivalente ao juiz |
| 7 | `app/folha/views.py:612` D | Raio-X: furo apenas até demissão inclusiva; exclui colaborador se demissão antes da janela `:614-616` | **N** direto: organiza linhas de relatório `:616-623` | D |
| 8 | `app/folha/services/ranking_he.py:133` A | Ranking com filtro de ciclo: ciclo vem de EC ativa ATUAL; mantém só ciclo igual ao filtro `:135` | **N**: lista de ranking `:144-155` | A |
| 9 | `app/folha/views_retrato.py:107` A | Retrato mostra cadastro de EC ativa mais recente (`order_by(data_inicio)` e sobrescrita por colaborador) | **N**: enriquece posto/praça/escala `:110-123`, não reclassifica linha | A |
| 10 | `app/ponto/views.py:376` S | Aparece na tela de turnos abertos se ATUAL ativo; busca turnos hoje/ontem `:374-378` | **N**: constrói alertas de tela `:392-402` | S |
| 11 | `app/ponto/views.py:1119` S | Contagem de ativos atuais no universo do fechamento | **N**: contador de tela; universo já vem do juiz específico `:1114` | S |
| 12 | `app/ponto/views.py:1120` S | Contagem de desligados atuais no universo | **N**, contador de tela | S |
| 13 | `app/ponto/views.py:1121` S | Contagem dos que não são atualmente ativo nem desligado (`exclude __in`) | **N**, contador de tela | S |
| 14 | `app/ponto/views.py:2061` A | Cabeçalho drawer: primeiro EC ativa fornece rótulo | **N**: retorna `_rotulo_da_escala` `:2063` | A |
| 15 | `app/ponto/views.py:2225` S | Aparece no seletor da lista de ausências se atualmente ativo | **N** direto: queryset de formulário/tela | S |
| 16 | `app/ponto/views.py:2373` S | Aparece no formulário de lançar ausência se situação atual em `{ativo,afastado}` | **N** direto: POST resolve `colab_id` separado `:2375-2377`; não confundir dropdown com bloqueio servidor | S |
| 17 | `app/ponto/utils.py:170` S | Cobertura, sem empresa: candidato atualmente ativo | **N** direto: lista de candidatos; consumidor `app/ponto/views.py:3087` e `app/ponto/services/cobertura.py:84` | S |
| 18 | `app/ponto/utils.py:172` S | Cobertura, com empresa: candidato atualmente ativo na empresa | **N** direto, mesma cadeia | S |
| 19 | `app/ponto/utils.py:180` D | Cobertura: demissão nula **ou estritamente posterior** a `dt_fim.date()`; demissão no próprio fim exclui | **N** direto: lista de candidatos | Mesmo com EC cobrindo o dia final inclusivamente, a igualdade demissão=fim exclui |
| 20 | `app/ponto/portas/celula.py:908` A | Gerador: EC inativa só segue para dia futuro E quando `vinculo_do_dia` devolve o mesmo EC; `d<=hoje` pula | **F/I potencial pela célula**, a criação ocorre `:913-917`; o predicado bruto decide materializar mesmo após `_cobre` `:906` | A: EC encerrado cobrindo hoje é aceito pela juíza mas pulado |
| 21 | `app/ponto/portas/regen_exportada.py:180` A | Regeneração: estender horizonte somente se EC ativa | **F**: regenerar `:179-180` → recalcular fechamento `:184-185`; impacto exato na retenção depende do horizonte | A: encerrado ainda cobrindo competência não estende |
| 22 | `app/ponto/selecao_periodo.py:278` A | Turno em curso: jornada vem de EC ativa atual; sem ela fallback24h `:287-290` | **N** direto identificado: retorna mapa `:292-293` | A: EC encerrado vigente perde sua jornada |
| 23 | `app/ponto/services/abriu_nao_bateu.py:194` A | Já passou a hora de bater: primeira EC ativa; sem EC retorna True `:196-200` | **N** direto: filtra contador dos mudos `:140-146` | A: EC válido inativo gera True independentemente da hora real |
| 24 | `app/ponto/services/afastado_avisa.py:41` S | Se juiz de afastamento não cobriu, campo atual afastado ainda retorna `(True,'situacao_sem_ausencia')` `:39-43` | **I**: `furos_sem_cobranca` importa o predicado `app/ponto/services/furos_sem_cobranca.py:51`; também triagem da batida; precisa conferir uso final no seu fechamento | Campo afastado com EC válido e sem ausência: retorna afastado por conta própria |
| 25 | `app/ponto/services/bordas_realizado.py:62` A | Universo de auditoria do realizado: EC atualmente ativa, ainda que janela histórica | **N**: contadores de realizado `:76-86` | A |
| 26 | `app/ponto/services/cartorio.py:1748` S | Pode emitir furo/cobrança morta/limbo: atual `!=desligado` e não `_cala_emissao` | **I**: guarda de emissão `:1765` → vínculo juiz `:1768` → emissor no bloco seguinte; seleção crua vem antes do juiz | S |
| 27 | `app/ponto/services/cartorio.py:1813` S | Pode emitir furo parcial: código FURO_PARCIAL, atual `!=desligado`, fora do piso falso | **I**: `:1818` → emissor `_abrir_chamado` `:1828-1831` | S |
| 28 | `app/ponto/services/cluster_espurio.py:183` S | Reconciliar cluster somente atualmente ativo | **I**: pode impedir tanto fechamento de chamados `:189-191` quanto abertura `:196-204` | S |
| 29 | `app/ponto/services/cluster_espurio.py:185` A | Reconciliar cluster exige EC ativa E data_fim nula | **I**, mesma cadeia `:189-204` | A; ainda exclui ativa=True com fim válido |
| 30 | `app/ponto/services/cluster_espurio.py:260` S | Campo informativo `'ativo' = situação atual=='ativo'` na simulação de reparo | **N**: saída diagnóstica; gate real relê e usa porta `:280-283` | S; excluir se seu censo só comporta predicados com efeito |
| 31 | `app/ponto/services/espelho.py:43` A | Piso visual parte do início do EC ativo mais recente; se futuro, limita hoje `:44-47` | **N** direto: `espelho_do_colab` calcula `vis_ini=min(piso_visual,apur_ini)` `:1225`; comentário explicita resumo recortado `:34-35` | A |
| 32 | `app/ponto/services/espelho.py:202` A | Resumo de escala atual, mais recente; sem ativa retorna None `:203-205` | **N**: monta resumo visual `:206-224` | A |
| 33 | `app/ponto/services/faltas_de_hoje.py:35` A | Universo do vigia de falta hoje: última EC ativa; sem ela ou intermitente pula `:37-40` | **N** direto: contador de faltas sem cobrança; se limbo/dia devido determina achado | A |
| 34 | `app/ponto/services/fechamento.py:432` D+S (`:438-441`) | Universo: atual `!=desligado` OU desligado com demissão **>= início** da janela da sua empresa; sem limite superior, demissão nula em desligado não entra | **F**: universo retornado `:445` → recalcular `:554` e tela `app/ponto/views.py:1078`, `:1114` | S/D: desligado com demissão nula mas EC válido sai; atual ativo sem EC entra |
| 35 | `app/ponto/services/fechamento_lista.py:60` A | Rótulo de EC ativa no fechamento; sobrescreve por colaborador `:64` | **N**: rótulo visual; classificador do TXT é chamado separadamente `:70-73` | A |
| 36 | `app/ponto/services/furos_sem_cobranca.py:62` S | Vigia: células de furo da janela atual até ontem somente de atualmente ativo | **I**: cala o universo do vigia de furo sem cobrança; código ainda verifica vivos/tranca `:83-85` | S |
| 37 | `app/ponto/services/furos_vetados.py:62` S | Relatório de furos vetados: exclui atualmente desligado, inclui afastado; janela até hoje | **N** direto: retorna achados `:64-70`; comentário declara lista de cadastro `:51-54` | S |
| 38 | `app/ponto/services/geofence.py:46` S | Premissa de vínculo divergente não julgável se atual `!=ativo` (retorna None) | **I**: `vinculo_diverge` `:66-67` → reconciliador premissa `app/chamados/reconciliador.py:300-302`; None conservador pode manter chamado | S; este sítio pode reter vivo pelo None |
| 39 | `app/ponto/services/he_pendente_lavrado.py:92` S | Foto de HE pendente da competência só de atualmente ativo | **F potencial indireta**: universo lavrado → `recusar_ponta_pequena` lê foto `app/ponto/management/commands/recusar_ponta_pequena.py:69`; portão TXT calcula pendência ao vivo `app/folha/porta_export.py:495`, bloqueia se flag ligada `:853`. Não afirmar que foto é o gate direto | S |
| 40 | `app/ponto/services/pautas_dos_discordantes.py:70` S | Dias discordantes sem fio da competência só de atualmente ativo | **I**: universo de `dias_sem_fio`; cru exclui antes de `_fio_do_dia` `:75-78`; comando consome `app/ponto/management/commands/pautas_discordantes.py:67-68` | S |
| 41 | `app/ponto/services/piso_intervalo.py:29` A | Auditoria de intervalo na janela apenas para EC atualmente ativa e modo duração | **N**: leitura de pares/contador `:18-21`, sem julgar folha `:13` | A |
| 42 | `app/ponto/services/triagem_batida.py:92` A | Existe escala para acesso: qualquer EC ativa; ausência fabrica sinal de sem escala | **I**: cria chamado `acesso_bloqueado` `:101-122`; **não bloqueia batida**, explicitado `:123-126` | A |
| 43 | `app/ponto/services/triagem_batida.py:333` A | Rótulo de chamado de anuência usa primeira EC ativa | **N** como trava: chamado é criado independentemente, só assunto/título dependem do rótulo `:335-340` | A |
| 44 | `app/ponto/services/vigia_de_hora.py:117` S | Vigia hoje/ontem pula atualmente desligado | **I**: impede `julgar_celula`/emissão `:123-133` | S |
| 45 | `app/ponto/services/vinculo_na_lista.py:71` S | Achado de cadastro x GPS exclui atualmente desligado apesar de batidas passadas | **N** emissão direta atual: lista de cadastro; retorna achados `:91-94`; comentário diz emissor antigo retirado `app/chamados/catalogo/modulos.py:164` | S |
| 46 | `app/ponto/services/vinculo_na_lista.py:83` S | Pares/colegas do posto para classificar evidência: somente atualmente ativos | **N** direto: fração alimenta classificador `:85-90` | S |
| 47 | `app/ponto/services/vinculo_na_lista.py:152` S | Cadastro ainda em revisão: mantém só colaborador atual não desligado e posto igual à lavra | **I potencial no geofence**: `app/ponto/services/geofence.py:76-81` consulta revisão para distinguir fluxo; abertura exata precisa rastrear bloco posterior | S |
| 48 | `app/ponto/supra_juiz.py:194` D | Quando juiz do vínculo=None, `sem_vinculo_em_dia_de_trabalho` só se célula trabalha e dia **não posterior** à demissão | **F/I**: insumo do classificador, passado pelo cartório `app/ponto/services/cartorio.py:1145`; sem vínculo em dia de trabalho acusa cadastro, comentários `app/ponto/supra_juiz.py:184-191` | Não contradiz escolha de vínculo (só executa quando juiz=None); diverge na cobrabilidade por demissão, embora célula diga trabalha. Campo chega por `values_list` `:177` e argumento do cartório `:1145` |
| 49 | `app/ponto/turnos.py:2464` A | Regime intermitente da sugestão de próxima batida vem da primeira EC ativa | **N** como trava: sugere E/S por últimas14h `:2466-2473` | A |
| 50 | `app/ponto/turnos.py:2483` A | Demais marcos de sugestão E/S vêm da primeira EC ativa; sem escala retorna E `:2485-2487` | **N** como trava: sugestão tipo de batida `:2488-2495` | A |
| 51 | `app/ponto/worklist.py:49` A | Posto de agrupamento de gaps DP vem da primeira EC ativa | **N**: só rótulo de grupos, gap vem da ata separadamente `:60-67` | A |
| 52 | `app/chamados/detector_anomalias.py:60` A | Colisão se quantidade de EC ativa E data_fim nula >1 `:63` | **I**: detector consumido em `app/api/views.py:434` e `app/api/views_core.py:420`; gera motivo `colisao_escala` `:64-67` | A: EC atuais com datas disjuntas contam colisão, encerrados cobrindo dia somem |
| 53 | `app/chamados/juizes.py:1809` A | Marco esperado da pergunta datada vem de EC atualmente ativa mais recente | **I**: fallback da defesa/classificação de resposta `:1860-1864` e `:1990`; altera marco usado para validar fio | A: pergunta histórica usa escala nova |
| 54 | `app/chamados/models.py:1006` S | Chamado pode renascer somente atualmente ativo; demais situações retornam False | **I**: impede `reabrir` `:1020` e emissão correspondente | S |
| 55 | `app/chamados/reconciliador.py:234` A | Âncora ainda ausente se existir EC ativa com âncora nula (sem data do fato) | **I**: premissa `:292` → catálogo `:302` → retratação/fechamento `:524`; pode manter chamado | A |
| 56 | `app/chamados/reconciliador.py:821` S | Reemitir chamado do dia só de atualmente ativo | **I**: retorna antes da costura/emissão `:835-841`; chamador `:1622` | S |
| 57 | `app/chamados/services/adesao.py:34` S | Universo adesão na janela: célula nunca_bateu de atualmente ativo | **I**: `sincronizar_adesao` consome `:59-63`, abre pai `:75-76` | S |
| 58 | `app/chamados/services/adesao.py:67` S | Segundo gate de adesão pula atualmente desligado | **I**: impede abrir/reabrir pai `:75-93`; pode ser redundante após filtro, mas é segundo sítio e outra leitura de estado | S |
| 59 | `app/chamados/services/desvio_escala.py:44` A | Existe escala para desvio dos últimos14 dias se EC ativa atual; senão None `:45-50` | **N** como trava: texto explicador `app/chamados/services/explicador.py:155`; `flip_automatico` usa texto `app/ponto/management/commands/flip_automatico.py:66` | A |
| 60 | `app/chamados/services/desvio_escala.py:106` A+S (`:107`) | Universo de colegas do mesmo template: EC ativa e colaborador atualmente ativo | **N** direto: conta colegas deslocados/corretos para texto `:108-115` | A/S |
| 61 | `app/chamados/services/disputa_emissao.py:341` A | Opções de dropdown de escala: EC ativa E data_fim nula | **N** como emissão/silêncio: muda opções de pergunta, não decide nascimento nesse ponto `:339-345` | A |
| 62 | `app/chamados/services/disputa_emissao.py:1419` S | Havendo pergunta prévia, atual não ativo é tratado como “já perguntou” (True) | **I**: `_criar_pergunta_se_couber` consulta em `:832-833`, impede `PerguntaDisputa.abrir` `:837` | S |
| 63 | `app/chamados/services/fabrica_da_celula.py:112` S | Motivos a perguntar vazios se atual não ativo (ou dia_sem_trabalho) | **I**: `perguntar_pela_celula` consome `:207`, só pergunta se lista `:217-219` | S |
| 64 | `app/chamados/services/fabrica_da_celula.py:201` S | Pauta cadastro sem turno só para célula passada de atualmente ativo e dia cobravel | **I**: `vencidos=None` → pauta `:203-205`, depois retorno sem pergunta | S |
| 65 | `app/chamados/services/limbo_folgas.py:65` A | Número de colegas no template: conta EC ativa | **N**: texto de sugestão do chamado `:68-70`, não decide fechar/emitir | A |
| 66 | `app/chamados/services/pergunta_no_app.py:186` S | Vigia de chamados cobradores sem pergunta no app só mede atualmente ativos | **I**: achados `:200-208` → comando perguntar `app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:20` → `perguntar` agrupa e gera disputa `:240-246` | S |
| 67 | `app/chamados/views.py:442` A | Ação corroborar régua elege EC ativa do template mencionado, mais recente | **I**: passa EC à porta `corroborar_regua` `:443`; veto da cobrança é efeito declarado imediatamente acima `:436-437` | A |

Exclusões importantes:

- Ordenações `.ativa` em carregadores delegando a escolha ao juiz: `app/ponto/nucleo.py:15`, `app/chamados/services/carga_pergunta_viva.py:97`, `app/ponto/turnos.py:1560`. Não são predicados próprios de existência.
- `Empresa.ativa`: `app/folha/services/vigia_pausa.py:109`, `app/chamados/services/furo_sem_canal.py:52`, `app/ponto/services/fechamento.py:1514`, `app/ponto/services/he_pendente_lavrado.py:241`, `app/ponto/services/pautas_dos_discordantes.py:63`, `app/ponto/services/vigia_ausencia.py:250`.
- `PushSubscription.ativa`: `app/chamados/push_utils.py:24`, `app/ponto/webpush.py:37`; `Advertencia.ativa`: `app/ponto/models.py:1191`, `:1195`.
- `data_demissao` do catálogo é chave de contexto de chamado, não leitura do campo Colaborador: `app/chamados/catalogo/modulos.py:141`.
- Serializações/texto sem predicado de existência: `app/ponto/services/dia_do_colab.py:79`, `app/ponto/utils.py:47`, `app/ponto/services/triagem_batida.py:114`, `:119`, `app/folha/export.py:870`, `app/folha/views.py:658`.
- Universo/contador de **divergência declarada campo versus juiz de afastamento**: `app/ponto/services/afastado_avisa.py:126`, `:167`, `:171`; consultam juiz antes da conclusão `:127-130`, `:164-165`. Distintos do branch `:41`, que efetivamente retorna afastado por conta própria.

O achado extra do outro agente em **`app/escala/services/leitor_celula.py:389`** alcança folha: `grade_do_fechamento` chama a grade `app/folha/export.py:335-338`, e o fechamento exclui `fora_vinculo` do universo de dias `app/ponto/services/fechamento.py:1043-1049`, afetando previsto/abono/realizado `:1060-1064`. **Não dizer que esse `_fora` entra diretamente no atual juiz de retenção**: `motivos_retencao_espelho` ignora a grade e delega à leitura direta de células `app/folha/export.py:1275`; sua própria demissão crua já está nas linhas 5–6 do meu censo.

Lista mínima incontroversa que autoriza antecip

[truncado em 20000 de 20393 caracteres]

## 13/11 11:38 · sessao ef3381631518 · - · - · origem=Ronald (tmux)

Inventário completo enviado em mensagens: **59 sítios**, com **49 leituras de `situacao`**, **12 de `ativa`** e **2 de `data_demissao`**, contando separadamente campos usados no mesmo predicado. [não verificado]

| Área | Sítios | situação | ativa | demissão |
|---|---:|---:|---:|---:|
| API/app | 12 | 9 | 4 | 0 |
| Férias | 2 | 2 | 0 | 0 |
| Holerite | 4 | 2 | 1 | 2 |
| Templates | 2 | 2 | 0 | 0 |
| Relatórios | 15 | 11 | 6 | 0 |
| Inteligência | 24 | 23 | 1 | 0 |

As decisões por `data_demissao` escondidas em `values_list` estão em `app/holerite/matriz.py:97` e `app/holerite/matriz.py:102`; os campos e seu desempacotamento estão em `app/holerite/matriz.py:87`.

As duas decisões cruas dos templates são mostrar **Reativar** (`app/templates/colaboradores/partials/painel_colaborador.html:163`) e mostrar **Transferir posto** (`app/templates/colaboradores/editar.html:211`).

Distinção para consolidação: login/refresh recusam acesso (`app/api/login_helpers.py:141`, `app/api/views.py:116`, `app/api/views_core.py:172`), e os dois kernels recusam batida (`app/api/views.py:403`, `app/api/views_core.py:389`); classificá-los como silêncio **de emissão do FIO** exigiria um caminho adicional. [não verificado]

O universo de holerites é entrega de documentos (`app/holerite/matriz.py:79`); o universo do lote de espelhos é PDF (`app/relatorios/services.py:217`, `app/relatorios/services.py:237`). Não tratei esses dois universos como universo do fechamento/TXT.

## 13/11 11:31 · sessao b8e26257f1cc · LEI-DESLIGADO-NAO-TRAVA · censo · origem=Code

**A cópia ainda permite dias retidos de colaborador desligado:** o retentor não consulta `situacao`; recorta pela demissão e pode reter células acusadoras até essa data. A porta de rescisão do TXT responde outra pergunta, usando apenas `data_demissao <= fim_da_janela`. (`app/folha/export.py:60`, `app/folha/export.py:1200-1266`)

Censo estático **FECHADO nesta cópia**: AST dos 939 arquivos Python fora dos diretórios `tests/` e `migrations/`, sem erro de parse, cruzada com `rg` global, imports, apelidos e chamadas por atributo. Foram encontrados oito sítios de chamada; `_mre` é o único apelido dessas funções. Método e contagem são resultados desta execução, sem artefato persistido [nao verificado]. A identidade da cópia com o HASH informado não pôde ser autenticada por Git [nao verificado].

**1. Todos os chamadores diretos, inclusive por apelido**

| Função | Chamador | O que faz com o retorno |
|---|---|---|
| `motivos_retencao_celula` | `app/folha/export.py:1275` | `motivos_retencao_espelho` devolve os motivos; seus três parâmetros antigos são ignorados. (`:1270-1275`) |
| mesma | `app/folha/fora_da_folha.py:98`, import `:97` | `_dias_que_retem` repassa `(motivos, dias)`. (`:98-99`) |
| mesma | `app/folha/previa.py:170`, import `:169` | Compara as datas retidas com as propostas; alimenta “passaria a entrar” e `mudariam_de_lado`. (`:119-121`, `:133`, `:171-173`) |
| mesma | `app/folha/services/vigia_pausa.py:125`, import `:48` | Alimenta contadores de barrados e dias retidos que seriam somente pausa. (`:126-136`) |
| `motivos_retencao_espelho` | `app/folha/export.py:968` | Retorno não vazio produz `status='fora'`, `motivo='furo_espelho'`, detalhe e `motivos_espelho`; encerra a classificação daquela pessoa. (`:969-973`) |
| mesma | `app/ponto/services/fechamento.py:1067`, import **`as _mre`** em `:1066` | Materializa `FechamentoMensal.motivos_espelho`, depois persiste esse campo. (`:1067-1068`, `:1186`) |
| `_dias_que_retem` | `app/folha/fora_da_folha.py:137` | Guarda motivos/dias por colaborador, somente para os classificados como `furo_espelho`. (`:119-139`) |
| mesma | `app/folha/fora_da_folha.py:217` | `o_que_segura` entrega data, veredito, motivo, fio, ação para destravar e chamados; junta o status/motivo do export. (`:215-231`) |

As cadeias indiretas principais são:

- **TXT:** `classificar_export` fornece os entrantes; `montar_linhas_export` exclui os demais. (`app/folha/export.py:714-727`)
- **Retrato/lavra:** `fora_da_folha` expõe quantidade e datas; `montar_linhas` transfere status, motivo, dias, datas, dono, tipo e saldo para o retrato. (`app/folha/fora_da_folha.py:184-197`, `app/folha/retrato_folha.py:1156-1188`)
- **Vigia de alcance:** lê `dias_que_seguram`, deduplica pares pessoa/data e procura a porta de atendimento. (`app/chamados/services/vigia_fio.py:563`, `:582-618`)
- **Perguntas/chamados:** a fábrica consulta situação e acusação da célula, sem chamar essas três funções. (`app/chamados/services/fabrica_da_celula.py:105-125`)

**2. Sítios que interpretam “retido/bloqueado/furo” sem chamar diretamente as três funções**

É necessário distinguir **leitura do resultado do juiz**, **interpretação de retrato** e **acusação da célula**: esses caminhos têm fontes diferentes. (`app/folha/export.py:968-973`, `app/folha/retrato_folha.py:89-111`, `app/chamados/services/fabrica_da_celula.py:120`)

| Sítio | Fonte e decisão |
|---|---|
| `app/folha/retrato_folha.py:89-95` | `_balde`: `entra` → entrantes; rescisão → módulo próprio; restante → bloqueado. **Interpreta resultado gravado**, sem reler célula. |
| `app/folha/retrato_folha.py:98-121` | Acessor e contadores: leem `retrato.linhas`; contador é o tamanho da mesma lista filtrada. |
| `app/escala/services/cadastro_realidade.py:181-200` | C5 começa em `veredito='nunca_bateu'`; para desligados, conserva exclusivamente datas presentes em linha **bloqueada/furo_espelho do retrato**. **Lê a lavra**, não o juiz vivo. |
| `app/folha/vigia_retido_calado.py:46-55` | Conta bloqueados com fio ninguém e desconta pessoas roteadas à C5. **Lê retrato e C5**. |
| `app/chamados/management/commands/disparar_perguntas_competencia.py:54-72` | Contador `nunca_bateu_sem_adesao`: desligado fora dos pares da C5 é barrado. **Lê C5**, sem recalcular retenção. |
| `app/folha/views.py:259-260` | Seleciona `furo_espelho` para aviso de exportação. **Lê classificação**. |
| `app/folha/views.py:331-341`, `:589-601` | Painel e raio X agrupam status/motivos do juiz. |
| `app/folha/retidos_pdf.py:32-38` | Chama **todo `status='fora'`** de retido, inclusive rescisão. **Lê classificação**. |
| `app/ponto/services/fechamento_lista.py:28-37` | Traduz o resultado do juiz para a tela; motivos sem tradução própria aparecem visualmente como `furo_espelho`, preservando `motivo_juiz`. |
| `app/relatorios/views.py:1658-1669` | Coluna Folha de “Quem cobrar”: traduz o resultado do juiz. |
| `app/relatorios/views_pend.py:58-69` | Pendências cadastrais: seleciona portas cadastrais da classificação. |
| `app/inteligencia/calculadores.py:210-234`, `:271-296` | Métricas encerrada/corrente contam status e motivos do juiz. |
| `app/api/views_mensageria.py:257-275` | Com snapshot, deriva `retidos = denominador − numerador`. **Lê projeção gravada da métrica**. |
| `app/api/views_mensageria.py:283-301` | Sem snapshot, consulta o juiz e conta todos os `fora`. |
| `mensageria/nucleo/ferramentas.py:1196`, `:1247-1268` | Separa rescisão como ressalva e soma outros motivos como “travam de fato”. **Interpreta a resposta da API**. |
| `app/folha/management/commands/simular_folha.py:74-90` | Registra os não entrantes e seus motivos/valores na foto de simulação. |
| `app/folha/services/vigia_txt.py:113-117`, `:231-232` | Compara status/motivo e TXT usando os mesmos itens do juiz. |
| `bin/censo_folha.py:14-16`, `:40-43` | Seleciona os `fora` e conta motivos; os “agravantes” são diagnóstico adicional. |

Há uma contagem com nome enganoso: painel/PDF usam `len(motivos_espelho)` como “dias”, mas o retentor devolve **motivos distintos**, enquanto datas ficam no segundo componente de `com_dias=True`. (`app/folha/views.py:398`, `app/folha/retidos_pdf.py:90`, `app/folha/export.py:1227-1230`, `:1266`)

Os seguintes caminhos **derivam acusação/cobrança, não retenção da folha**, e não recebem automaticamente as isenções do retentor:

- `vigia_fio.censo` antigo lê `VEREDITOS_ACUSAM`; o `censo_alcance` lê os dias do juiz da folha. (`app/chamados/services/vigia_fio.py:331`, `:378-380`, `:585`)
- `furo_sem_canal` seleciona vereditos próprios e ausência de pergunta/chamado. (`app/chamados/services/furo_sem_canal.py:35`, `:70-86`)
- `furos_sem_cobranca` seleciona furo/protestos e situação ativo. (`app/ponto/services/furos_sem_cobranca.py:61-63`)
- Fábrica e emissão de disputa leem acusação para nascimento/alcance de pergunta. (`app/chamados/services/fabrica_da_celula.py:112-125`, `app/chamados/services/disputa_emissao.py:757-773`)
- Vitalidade, resposta e reconciliação também leem acusação diretamente. (`app/chamados/juizes.py:1375`, `:1448-1451`; `app/chamados/models.py:3358-3359`; `app/chamados/reconciliador.py:642`, `:1028`; `app/chamados/services/limbo_folgas.py:195`; `app/chamados/services/resposta_tardia.py:45`; `app/chamados/services/silencio_ausencia.py:42`)
- Auditorias/relatórios de célula fazem leituras semelhantes. (`app/ponto/services/vigia_ausencia_veredito.py:41`; `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:36`; `app/relatorios/dia_da_frota.py:55`; `app/chamados/management/commands/auditar_invariantes_chamados.py:198`, `:343`, `:1391`, `:1605`, `:1647`)

Outro escritor real de `motivo='furo_espelho'`, além de `classificar_export`, não foi encontrado na varredura [nao verificado]; o escritor encontrado está em `app/folha/export.py:970`.

**3. Ordem das portas e os quatro casos de desligado**

O universo de `classificar_export` são os `FechamentoMensal` existentes da empresa/competência, ou os objetos injetados; ele não exige situação ativo. (`app/folha/export.py:783-788`)

Antes das saídas, calcula separadamente `travas_aprovacao`: conta vazia, turno aberto e ausência de âncora 12×36 — esta última somente para ativos. Essas travas acompanham inclusive quem sai pela rescisão. (`app/folha/export.py:806-848`)

| Ordem | Porta |
|---|---|
| 1 | Demissão não nula e `<= fim` → `rescisao_modulo_proprio`. (`app/folha/export.py:60`, `:866-871`) |
| 2 | Código Domínio vazio → `sem_codigo_dominio`. (`:872-875`) |
| 3 | Batida durante férias → `ferias_com_batida`. (`:876-884`) |
| 4 | Horas zero, pelo menos duas batidas e nenhum previsto → `cadastro_zero`. (`:902-967`) |
| 5 | Motivos do retentor não vazios → `furo_espelho`. (`:968-973`) |
| 6 | Código de rubrica fora do catálogo → `nao_homologado`. (`:974-979`) |
| 7 | Caso contrário → `entra`. (`:841-842`, `:980`) |

Para uma pessoa com **situação atual desligado**, havendo fechamento no universo:

| Demissão | Resultado atual | Pode virar `furo_espelho`? |
|---|---|---|
| **(a) Dentro da janela** | Sai na primeira porta, pela rescisão. O retentor chamado isoladamente ainda pode devolver dias anteriores e célula acusadora no dia da demissão. (`app/folha/export.py:866-871`, `:1208-1264`) | **Não na classificação**, porque a rescisão vem antes. (`:866-871`, `:968`) |
| **(b) Depois do fim** | Não sai pela rescisão desta competência; percorre as demais portas. (`:60`, `:866-968`) | **Sim**, se alcançar a porta do espelho e houver motivos. (`:968-973`) |
| **(c) Sem data** | Não sai pela rescisão; percorre as demais portas. Novo fechamento normalmente não nasce para esse desligado, mas fechamento existente/injetado é classificado. (`:60`, `:783-788`; `app/ponto/services/fechamento.py:432-441`) | **Sim**, com fechamento existente/injetado e motivos. (`app/folha/export.py:968-973`) |
| **(d) Antes do início** | Fechamento existente sai pela rescisão; a geração normal o exclui. O retentor isolado retorna vazio pela janela colapsada. (`app/folha/export.py:60`, `:866-871`, `:1208-1213`; `app/ponto/services/fechamento.py:432-441`) | **Não**. (`app/folha/export.py:866-871`) |

**4. Onde se decide “desligado”; afastado/INSS**

**Não há uma função única compartilhada para “desligado”.** Existem decisões pelo campo atual, pelo campo apresentado e exclusivamente pela data de demissão. (`app/colaboradores/services/situacao_exibida.py:56-63`, `app/ponto/services/fechamento.py:432-441`, `app/folha/export.py:60`)

Inventário dos predicados explícitos encontrados, agrupado pelo efeito:

| Domínio | Locais |
|---|---|
| Retenção, universo e contagem da folha | `app/ponto/services/fechamento.py:432`, `:440`; `app/folha/export.py:60`, `:810`; `app/ponto/views.py:1119-1121` |
| Emissão/vigias/listas | `app/ponto/services/cartorio.py:1748`, `:1813`; `app/ponto/services/vigia_de_hora.py:117`; `app/escala/services/cadastro_realidade.py:186`, `:198`, `:603`; `app/chamados/management/commands/disparar_perguntas_competencia.py:59`; `app/ponto/services/furos_vetados.py:62`; `app/ponto/services/vinculo_na_lista.py:71`, `:152` |
| Adesão/invariante de cobrança | `app/chamados/services/adesao.py:67`; `app/chamados/management/commands/auditar_invariantes_chamados.py:653` |
| Cadastro, vínculo, desligamento e reativação | `app/colaboradores/services/vinculo.py:91`; `app/colaboradores/services/desligamento.py:18`; `app/colaboradores/views.py:1417`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:40`; `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:35`; `app/colaboradores/management/commands/tripwire_vinculo_orfao.py:47-48` |
| Holerite, busca, indicadores, relatórios e férias | `app/holerite/matriz.py:97`; `app/core/busca.py:104`; `app/inteligencia/views.py:83`; `app/relatorios/views.py:833`, `:848`, `:857`; `app/ferias/views.py:605` |
| Filtros/painéis de colaboradores | `app/colaboradores/views.py:1046`, `:2850`, `:2900`, `:2916`; `app/colaboradores/services/situacao_exibida.py:33`, `:57` |
| Templates | `app/templates/colaboradores/partials/painel_colaborador.html:90`, `:163`; `app/templates/colaboradores/editar.html:12`, `:211`; `app/templates/colaboradores/partials/_lista_tabela.html:38` |
| Comando operacional fora de `tests/` | `app/ponto/management/commands/testar_pwa.py:150` |

Há guardas mais amplas que **não significam exclusivamente desligado**: API aceita apenas ativo/afastado; login, reabertura e parte da emissão exigem ativo. (`app/api/views.py:403-405`, `app/api/views_core.py:389-391`, `app/api/login_helpers.py:141`, `app/api/views.py:116`, `app/chamados/models.py:1006`, `app/chamados/reconciliador.py:821`, `app/chamados/services/fabrica_da_celula.py:112`)

**INSS é outra condição:** o campo admite `afastado`; o juiz temporal consulta `Ausencia.tipo='afastamento_inss'` cobrindo o dia. Para folha, a ausência precisa estar aprovada; para cobrança, não rejeitada. A apresentação preserva desligado antes de consultar afastamento. (`app/colaboradores/models.py:295-299`, `app/ponto/turnos.py:2516-2518`, `:2704-2732`, `app/colaboradores/services/situacao_exibida.py:56-63`)

**5. Superfícies atuais e informativo DP/rescisão**

| Superfície | Comportamento atual |
|---|---|
| **Supervisor / Quem cobrar** | Seu universo limita a ativos, portanto exclui desligados. A coluna Folha consulta o juiz. (`app/escala/services/furos_diarios.py:259`, `:273-275`; `app/relatorios/views.py:1658-1669`) |
| **Lista administrativa C5 — Cadastro × Realidade** | Pode mostrar desligado com dia `nunca_bateu` quando o retrato conserva aquele dia como bloqueado/furo. (`app/escala/services/cadastro_realidade.py:181-200`, `:876-879`; `app/escala/views.py:487-492`) |
| **Central/chamados/perguntas** | Novas perguntas pela fábrica exigem ativo; os emissores de furo do cartório calam desligados. A vida das perguntas existentes tem decisões próprias por acusação. (`app/chamados/services/fabrica_da_celula.py:112`; `app/ponto/services/cartorio.py:1748`, `:1813`; `app/chamados/juizes.py:1448-1451`) |
| **`o_que_segura`** | Pode devolver dias retidos mesmo quando seu campo global informa saída por rescisão: consulta classificação e dias separadamente. (`app/folha/fora_da_folha.py:215-231`) |
| **Drawer** | Lê o retrato gravado; **não chama `o_que_segura`**. Exibe as datas gravadas. (`app/colaboradores/views.py:977-989`; `app/templates/folha/_retrato_o_que_fazer.html:43-46`) |
| **Retrato/tela da folha** | Rescisão fica em `modulo_proprio`, fora de `bloqueado`; datas de furo vêm dos dias do retentor. (`app/folha/retrato_folha.py:33`, `:89-95`, `:1157-1188`) |
| **`vigia_fio`** | Alcance lê dias retidos do juiz; o contador antigo continua baseado em acusação/chamados. (`app/chamados/services/vigia_fio.py:331`, `:585-588`) |
| **`vigia_retido_calado`** | Lê bloqueados do retrato e desconta a C5 como roteamento. (`app/folha/vigia_retido_calado.py:46-55`) |
| **E-SEM-VIVA-B** | O contador operacional encontrado chama-se `nunca_bateu_sem_adesao`; lê a C5 e barra desligados fora dela. (`app/chamados/management/commands/disparar_perguntas_competencia.py:39-72`) |
| **ESTADO** | Lê o carimbo do vigia; manchete usa `fio_morto_alcance`, e divergências usam o esperado gravado. (`bin/gerar_estado.py:260-261`; `bin/fio_estado.py:121-126`, `:148-154`, `:191-193`) |

Implementação de **`desligados_retidos`** não foi encontrada na cópia [nao verificado]. O carimbo atual inclui `fio_morto_alcance`, `fio_mudo_alvos` e `nunca_bateu_sem_adesao`. (`app/chamados/management/commands/vigiar_fio_morto.py:233-243`)

Há superfícies próximas ao informativo pedido:

- **Módulo próprio/rescisão:** já separa a pessoa da conta de bloqueados, mas não fornece seus dias sem batida. (`app/folha/retrato_folha.py:33`, `:89-95`, `:1171-1183`)
- **Chamado `desligamento_revisao`, para `dp_rh`:** registra desligamento, data, motivo e autoria; não enumera dias sem batida. (`app/colaboradores/services/desligamento.py:164-183`)
- **Pautas:** aceita destinatário DP e âncora pessoa/dia/competência; já existe entrega de outro tipo de dia sem pergunta ao DP. (`app/pautas/models.py:30-40`, `:76-89`; `app/chamados/services/fabrica_da_celula.py:268-284`)

Uma superfície **dedicada a dias sem batida de desligado como informativo rescisório** não foi encontrada [nao verificado]. Usar Pautas ou o chamado de revisão para isso seria alteração de comportamento, não descrição do que existe hoje [nao verificado].

**6. Quem escreve, quem lê e diferenças de significado**

**Escritores de `Colaborador.situacao` e `data_demissao`:**

| Escritor | Escrita |
|---|---|
| Modelo/admissão | Default ativo; admissão cria ativo pela porta. (`app/colaboradores/models.py:323`; `app/colaboradores/views.py:1185`, `:1208`; `app/colaboradores/services/colaborador.py:91-93`) |
| Agendamento de desligamento | Escreve data/motivo, mantendo situação até a efetivação. (`app/colaboradores/services/desligamento.py:87-102`) |
| Desligamento efetivo | Escreve situação desligado e data; salva pela operação situação. (`app/colaboradores/services/desligamento.py:109-113`) |
| Cron agendado | Seleciona demissão `<= hoje`, situação ainda diferente de desligado, e chama o mesmo executor. (`app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38-57`) |
| Reativação | Volta ativo e limpa demissão. (`app/colaboradores/services/desligamento.py:215-219`) |
| Lançamento INSS | Cria ausência aprovada e grava afastado. (`app/ponto/views.py:2420`, `:2433-2435`) |
| Reversor de afastado | Grava ativo quando o juiz não sustenta o afastamento. (`app/ponto/management/commands/reverter_situacao_afastado.py:73`; `app/ponto/services/afastado_avisa.py:126-129`) |

A porta genérica declara ambos os campos na operação `situacao` e executa atribuição/salvamento. O Django admin é somente leitura. (`app/colaboradores/services/colaborador.py:32`, `:55-76`; `app/colaboradores/admin.py:48`; `app/core/admin.py:59-74`)

**Leitores relevantes e semântica:**

| Leitor | Significado usado |
|---|---|
| Universo do fechamento | Situação **atual** combinada com demissão `>= início`; desligado sem data fica fora. (`app/ponto/services/fechamento.py:432-441`) |
| TXT/rescisão | Apenas data `<= fim`; ignora situação. (`app/folha/export.py:60`) |
| Retentor | Demissão limita fim **inclusivamente**. No dia da demissão, dispensa ausência de célula/veredito, mas não dispensa célula acusadora. (`app/folha/export.py:1208-1213`, `:1233-1264`) |
| Grade | Depois da demissão fica fora; no próprio dia, fica fora se não houver fato realizado/lavrado. (`app/escala/services/leitor_celula.py:365`, `:385-389`) |
| Suprajuiz | Sem data é tratado como nunca desligado; a exclusão temporal usa dia **estritamente posterior** à demissão. (`app/ponto/supra_juiz.py:154`, `:177`, `:194-197`) |
| Alimentadores do suprajuiz | Entregam `data_demissao`. (`app/ponto/services/cartorio.py:1145`; `app/escala/services/cadastro_realidade.py:511`; `app/ponto/management/commands/supra_juiz.py:178`; `app/ponto/management/commands/emitir_furo_retroativo.py:73`) |
| Prontidão e raio X | Recortam na demissão; janela colapsada recebe tratamento rescisório. (`app/folha/export.py:1068`, `:1087-1089`; `app/folha/views.py:612-615`) |
| Encerramento de vínculo | Grava `data_fim=data_demissao`; excedentes são células com data `> demissão`; vigência é fechada nas pontas. (`app/colaboradores/services/desligamento.py:128`; `app/colaboradores/services/vinculo.py:514`, `:855-864`; `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:46`, `:56`) |
| Holerite | Demissão igual ao início ainda inclui a competência: exclui com `demissão < início`. (`app/holerite/matriz.py:102`) |
| Cobertura de posto | Exige ativo e demissão nula ou **estritamente posterior** ao fim. (`app/ponto/utils.py:170`, `:180`) |

**RetratoFolha — escritor e leitores diretos fechados:**

- **Escritor:** `lavrar_retrato_folha`, somente com `--apply`, grava `linhas` e `lavrado_em`; `montar_linhas` produz status/motivo/dias/datas. (`app/folha/managemen

[truncado em 20000 de 21101 caracteres]

## 13/11 11:28 · sessao 9cd45b1f4852 · - · - · origem=Ronald (tmux)

Censo 4/6, somente leitura. Varri AST dos 881 `.py` de `app/`, `mensageria/`, `bin/`, excluindo diretórios `tests/`, `migrations/` e arquivos `test_*`/`tests*`; zero falhas de parse. Cruzei `rg` nos demais arquivos e predicados por constante (`DESLIGADO`). Não há `.git` acessível para autenticar o hash [nao verificado].

**Não há função única que responda “desligado” para todos os domínios.** Há o campo atual, um apresentador que preserva esse campo e um predicado de rescisão baseado exclusivamente em data. O retentor não testa situação: recorta pela demissão e continua lendo células acusadoras. Fontes: `app/colaboradores/models.py:323`, `app/colaboradores/services/situacao_exibida.py:56`, `app/folha/export.py:60`, `app/folha/export.py:1208`, `app/folha/export.py:1241`.

**Predicados de desligado relevantes à retenção/cobrança e às superfícies:**

- Universo de apuração: `~Q(situacao='desligado') | (Q(situacao='desligado') & data_demissao>=início)`; aceita demissão posterior ao fim, exclui desligado sem data ou antes do início. `app/ponto/services/fechamento.py:432`, `app/ponto/services/fechamento.py:440`.
- Rescisão/TXT: `data_demissao is not None and data_demissao<=fim`; independe da situação. A função específica é `sai_pela_rescisao`. `app/folha/export.py:54`, `app/folha/export.py:60`.
- Trava por âncora só considera situação ativo. `app/folha/export.py:810`.
- Cartório exclui desligado da emissão de furo e de furo parcial, embora a célula possa acusar/reter. `app/ponto/services/cartorio.py:1748`, `app/ponto/services/cartorio.py:1813`.
- Vigia de hora pula desligado. `app/ponto/services/vigia_de_hora.py:117`.
- C5 “nunca bateu”: identifica desligados pelo campo atual, lê dias de `RetratoFolha` bloqueado/furo_espelho e conserva apenas esses pares. `app/escala/services/cadastro_realidade.py:186`, `app/escala/services/cadastro_realidade.py:191`, `app/escala/services/cadastro_realidade.py:195`, `app/escala/services/cadastro_realidade.py:198`.
- Vigia/contador da competência: desligado fora da C5 é barrado para RH. `app/chamados/management/commands/disparar_perguntas_competencia.py:59`.
- Censo de afastado com furo exclui desligado antes de unir juiz/campo afastado. `app/escala/services/cadastro_realidade.py:603`, `app/escala/services/cadastro_realidade.py:606`.
- Invariante I5a acusa cobrança viva de desligado. `app/chamados/management/commands/auditar_invariantes_chamados.py:653`.
- Adesão recusa desligado. `app/chamados/services/adesao.py:67`.
- Cobranças vetadas excluem desligado. `app/ponto/services/furos_vetados.py:62`.
- Reabertura automática exige situação ativo; logo também barra afastados/férias, não identifica exclusivamente desligado. `app/chamados/models.py:1006`.
- Reconciliador que emite C2 também exige ativo. `app/chamados/reconciliador.py:821`.
- Lista de evidências de vínculo dá precedência ao desligamento atual; carregamento exclui desligados. `app/ponto/services/vinculo_na_lista.py:71`, `app/ponto/services/vinculo_na_lista.py:152`.
- Contadores da tela antiga distinguem ativo/desligado/outros pelo campo atual. `app/ponto/views.py:1119`, `app/ponto/views.py:1120`, `app/ponto/views.py:1121`.

Outros locais de decisão explícita pelo campo: impedimento de novo vínculo `app/colaboradores/services/vinculo.py:91`; guarda do desligamento `app/colaboradores/services/desligamento.py:18`; guarda da reativação `app/colaboradores/views.py:1417`; cron agendado `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:40`; fechamento de vínculo legado `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:35`; tripwire `app/colaboradores/management/commands/tripwire_vinculo_orfao.py:47`, `:48`; matriz de holerite, quando faltam ambas as datas, `app/holerite/matriz.py:97`; busca `app/core/busca.py:104`; indicadores `app/inteligencia/views.py:83`; relatórios `app/relatorios/views.py:833`, `:848`, `:857`; férias `app/ferias/views.py:605`; filtros/painéis de colaboradores `app/colaboradores/views.py:1046`, `:2850`, `:2900`, `:2916`. Comando de teste operacional fora de `tests/`: `app/ponto/management/commands/testar_pwa.py:150`.

Apresentação derivada: `DESLIGADO='desligado'`; `palavra` lê campo cru e faz desligado vencer afastamento. `app/colaboradores/services/situacao_exibida.py:33`, `:56`, `:57`. Templates que decidem esse ramo: `app/templates/colaboradores/partials/painel_colaborador.html:90`, `:163`; `app/templates/colaboradores/editar.html:12`, `:211`; `app/templates/colaboradores/partials/_lista_tabela.html:38`. API usa gate mais amplo “situação não é ativo/afastado” e mapa desligado: `app/api/views.py:403`, `:405`; clone `app/api/views_core.py:389`, `:391`. Login/token exige ativo: `app/api/login_helpers.py:141`, `app/api/views.py:116`.

**Afastado/INSS não é desligado.** O campo admite `afastado`, `ferias`, `ativo`, `desligado`. O juiz temporal consulta `Ausencia.tipo='afastamento_inss'`, com cobertura por dia; `afastado_hoje` devolve ausência ou `None`. Para folha exige aprovada; para cobrança aceita não rejeitada. `app/colaboradores/models.py:295`, `app/ponto/turnos.py:2704`, `:2715`, `:2718`, `:2725`, `:2731`, `:2516`, `:2518`. Badge/filtro usam efeito cobrança; desligado vence. `app/colaboradores/services/situacao_exibida.py:31`, `:46`, `:57`, `:63`. Há ainda leitura de divergência do campo cru: ausência não cobre hoje mas `situacao='afastado'` gera `situacao_sem_ausencia`. `app/ponto/services/afastado_avisa.py:39`, `:41`.

**Escritores de `situacao` / `data_demissao`:**

| Escritor | Escrita |
|---|---|
| Modelo/admissão | Default ativo em `app/colaboradores/models.py:323`; admissão passa ativo a `criar_colaborador` em `app/colaboradores/views.py:1185`, `:1208`; porta cria em `app/colaboradores/services/colaborador.py:91`. |
| Agendamento | Grava só data/motivo, conserva situação e acesso até a data. `app/colaboradores/services/desligamento.py:87`, `:90`, `:91`. |
| Desligamento efetivo | Atribui desligado/data, salva inteiro pela operação situação. `app/colaboradores/services/desligamento.py:109`, `:110`, `:113`. |
| Cron agendado | Seleciona data<=hoje e situação diferente de desligado; chama o mesmo juiz/executor. `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`, `:40`, `:51`, `:57`. |
| Reativação | Volta ativo e limpa demissão. `app/colaboradores/services/desligamento.py:215`, `:216`, `:219`. |
| Lançamento INSS | Após criar ausência aprovada, grava afastado. `app/ponto/views.py:2420`, `:2433`, `:2435`. |
| Reversor afastado | Grava ativo quando ambos os efeitos do juiz não cobrem hoje. `app/ponto/management/commands/reverter_situacao_afastado.py:73`; decisão `app/ponto/services/afastado_avisa.py:126`, `:127`, `:129`. |

Porta genérica atribui/salva em `app/colaboradores/services/colaborador.py:55`, `:60`, `:61`; operação declara os campos em `:32`; `salvar_inteiro` salva o objeto inteiro em `:73`, `:76`. Edição de cadastro salva inteiro, mas não atribui situação/demissão no seu formulário: `app/colaboradores/views.py:1314`, `:1345`. Admin herda `SoLeitura`: `app/colaboradores/admin.py:48`; bloqueia add/change/delete e torna todos os campos readonly: `app/core/admin.py:59`, `:62`, `:65`, `:74`.

**Leitores de data relevantes e semânticas divergentes:**

- Retenção: fim recortado na demissão, laço inclusivo `d<=fim`; no próprio dia da demissão dispensa célula ausente/sem veredito, mas célula acusadora continua retendo. `app/folha/export.py:1208`, `:1233`, `:1237`, `:1239`, `:1241`, `:1261`, `:1263`.
- Grade: data posterior à demissão sempre fora; próprio dia da demissão fora apenas sem fato realizado/lavrado. `app/escala/services/leitor_celula.py:365`, `:385`, `:386`, `:389`.
- Suprajuiz: `None` é tratado como nunca desligado; só dia estritamente posterior à demissão deixa de acusar cadastro por trabalho sem vínculo. `app/ponto/supra_juiz.py:154`, `:177`, `:194`, `:197`. Alimentadores: `app/ponto/services/cartorio.py:1145`, `app/escala/services/cadastro_realidade.py:511`, `app/ponto/management/commands/supra_juiz.py:178`, `app/ponto/management/commands/emitir_furo_retroativo.py:73`.
- Raio-X da folha recorta na demissão inclusivamente; demissão anterior à janela pula colaborador. `app/folha/views.py:612`, `:613`, `:615`.
- Prontidão fallback recorta fim na demissão; janela colapsada vira rescisão. `app/folha/export.py:1087`, `:1088`, `:1089`.
- Prontidão materializada usa demissão anterior ao início como motivo rescisão. `app/folha/export.py:1068`.
- Fechamento de vínculo usa `data_fim=data_demissao`; contador de células excedentes usa `data__gt=demissão`. `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:46`, `:56`; vínculo fecha pelas portas `app/colaboradores/services/desligamento.py:128`, `app/colaboradores/services/vinculo.py:855`, `:864`. Vigência declarada fechada nas duas pontas: `app/colaboradores/services/vinculo.py:514`.
- Holerite inclui competência se demissão coincide com início (`dem<ini` é que exclui). `app/holerite/matriz.py:102`.
- Cobertura de posto exige ativo e demissão nula ou **estritamente posterior** ao fim. `app/ponto/utils.py:170`, `:180`.

Portanto, não supõem todos o mesmo significado: situação é corrente no cadastro/emissão, universo usa situação corrente combinada com início da competência, TXT usa só data contra fim, e próprio dia da demissão tem tratamento diferente na grade e no retentor. Fontes contrastáveis acima: `app/ponto/services/fechamento.py:440`, `app/folha/export.py:60`, `app/escala/services/leitor_celula.py:389`, `app/folha/export.py:1237`.

**Superfície existente para DP/rescisão:** o desligamento cria chamado de auditoria `dp_rh`, módulo `desligamento_revisao`, com data/motivo/autor; não enumera dias sem batida. Pode ser superfície próxima, mas acomodar o informativo pedido é proposta, não comportamento atual. `app/colaboradores/services/desligamento.py:164`, `:168`, `:178`, `:181`, `:183`. A existência de outra superfície dedicada a dias sem batida para rescisão não foi encontrada na varredura [nao verificado].

## 13/11 11:28 · sessao eae6cc30235b · - · - · origem=Ronald (tmux)

Censo deste recorte fechado por `rg` em produção e AST Python (excluindo componentes `tests`, `migrations`, ambientes e `.git`), resolvendo `from ... import RetratoFolha as apelido`; os usos AST de `RetratoFolha` foram exatamente os abaixo. As referências seguintes são à cópia local; números reais de banco/log permanecem [nao verificado].

**RetratoFolha — escritor e todos os leitores diretos**

| Sítio | O que faz |
|---|---|
| `app/folha/management/commands/lavrar_retrato_folha.py:65`, `:74-76` | Único escritor encontrado: monta linhas, depois `update_or_create(empresa,mes,ano, defaults={linhas,lavrado_em})`, somente `--apply`. `:68` instancia retrato sem salvar para imprimir contadores. |
| `app/folha/retrato_folha.py:1156-1188` | Produz `linhas`: chama `fora_da_folha(...,com_itens=True)`; grava status/motivo, `dias`, `datas`, dono/tipo/saldo. Datas de furo vêm de `colabs`; férias com batida usam segunda fonte do item (`:1171-1176`). |
| `app/folha/views_retrato.py:190-196` | Lê retratos por competência/empresas e passa pelo acessor `linhas_do_retrato`. Mesma função de recorte alimenta tela (`:254`), PDF (`:382`) e raio X (`:482`). |
| `app/colaboradores/views.py:977-989` | Drawer: lê o retrato da competência da URL, busca linha, monta `o_que_fazer` e portas. **Não chama `o_que_segura`**. Template mostra as datas gravadas em `app/templates/folha/_retrato_o_que_fazer.html:43-46`; incluído por `app/templates/colaboradores/partials/painel_colaborador.html:26`. |
| `app/folha/vigia_retido_calado.py:33-55` | Lê todos os retratos da última data de lavra por empresa; pelo acessor filtra bloqueados e fio `ninguem`; desconta C5 como roteados. |
| `app/escala/services/cadastro_realidade.py:185-200` | C5: identifica desligados atuais, lê linhas bloqueadas do retrato e mantém somente dias nas `datas` de `motivo=furo_espelho`. É leitura da lavra, não juiz vivo. |
| `app/folha/retrato_folha.py:1296-1309` | Frescor: lê apenas existência/`lavrado_em`, compara ao fechamento mais novo; não julga retenção. |

A porta comum de leitura está em `app/folha/retrato_folha.py:98-111`. O balde local `_balde` lê `status='entra'`, depois `motivo in FORA_DA_CONTA`, senão **bloqueado** (`:89-95`); `FORA_DA_CONTA=('rescisao_modulo_proprio',)` em `:33`. Isso interpreta resultado gravado do juiz, não reconstitui retenção por célula. Contadores são `len` da porta (`:114-121`); recorte conta datas ISO inclusivamente (`:1237-1245`).

**Superfícies**

- **Supervisor / Quem cobrar:** `app/relatorios/views.py:1641-1642` obtém ranking e universo; `app/escala/services/furos_diarios.py:259`, `:273-275` limita a `situacao='ativo'`, eliminando desligados. A coluna Folha lê `classificar_export` em `app/relatorios/views.py:1658`, traduz `furo_espelho` em `:1660` e projeta apto/retido em `:1663-1669`; PDF usa a mesma coluna em `:1672-1677`. Essa lista não deriva retenção por veredito.
- **Lista administrativa Cadastro x Realidade/C5:** `app/escala/services/cadastro_realidade.py:181-200` começa em dias `veredito='nunca_bateu'`; ativo segue sem perguntar ao retrato; desligado só fica se constar no retrato bloqueado/furo. Formata assinatura C5 em `:204-209`, incorpora em `lista` em `:876-879`; tela/PDF em `app/escala/views.py:487-492`.
- **Retido calado:** `app/folha/vigia_retido_calado.py:46-55` conta bloqueados/fio ninguém menos C5; imprime `retidos_calados` em `:66`. Não deriva o dia retido.
- **Vigia fio/alcance:** `app/chamados/services/vigia_fio.py:563`, `:582-588` usa `dias_que_seguram` por empresa/competência aberta, deduplica pares colab/data; depois pergunta pelo alcance da porta (`:600-618`) e classifica sem porta (`:667-699`). Não reconstitui retenção por `VEREDITOS_ACUSAM`.
- **E-SEM-VIVA-B:** nome de teste/comentário, não nome de contador de produção encontrado. O contador vivo é `nunca_bateu_sem_adesao`: `app/chamados/management/commands/disparar_perguntas_competencia.py:39-72` lê C5, barra desligado que não está nela (`:59-61`), compara interseção de pares. O vigia imprime em `app/chamados/management/commands/vigiar_fio_morto.py:210-213`, grava número/esperado/C5 em `:233-243`. Pipeline lê o mesmo carimbo em `app/core/management/commands/pipeline_placar.py:85-106`, `:135-136`.
- **ESTADO:** `bin/gerar_estado.py:260-261` lê `fio_estado`; manchete usa `fio_morto_alcance` e esperado do carimbo (`bin/fio_estado.py:148-154`, `:175-183`). A lista genérica de fora do esperado lê o carimbo inteiro (`:191-193`), portanto pode expor discrepância `nunca_bateu_sem_adesao`. Não encontrei implementação de `desligados_retidos` via buscas textuais em produção; existência fora da cópia [nao verificado].
- **Tela Folha:** `app/folha/views.py:589-600` particiona `classificar_export`; a grade de furo chama `furos_por_dia` após clamp inclusivo na demissão (`:605-616`). Isso dá detalhe de furos, não nova decisão de quem está retido. PDF antigo lê o juiz vivo (`:858-859`); `retidos_pdf.agrupar_retidos` conta **todo** `status='fora'`, incluindo rescisão (`app/folha/retidos_pdf.py:32-38`), diferentemente do retrato que exclui rescisão do balde bloqueado.
- **TXT / aviso:** após gerar TXT, a view registra quantidade de retidos (`app/folha/views.py:289-293`) e emite toast por cookie (`:302-305`).
- **Mensageria:** fallback vivo lê `classificar_export` e conta qualquer `status='fora'` como retido (`app/api/views_mensageria.py:283-301`); portanto também mistura rescisão com bloqueio na terminologia da saída.

**Acusação/chamados não é retenção TXT**

- `app/chamados/services/fabrica_da_celula.py:105-113` cala `nunca_bateu` e situação diferente de ativo; `:120-121` usa `VEREDITOS_ACUSAM` para decidir nascimento de pergunta.
- `app/chamados/services/disputa_emissao.py:757-773` usa nunca_bateu/acusação para alcance da fábrica; não afirma retenção de folha.
- `app/chamados/juizes.py:1375` trata coerência da resposta; `:1448-1451` usa acusação para lastro/vitalidade da pergunta.
- `app/chamados/models.py:3357-3359` constrói predicados de célula acusa/absolve para vitalidade.
- `app/relatorios/pendencias.py:135-158` lista furos por `fur`/`orf` do ranking, não por retenção; o universo vem apenas dos ativos (`app/relatorios/views_pend.py:23-26`, `app/escala/services/furos_diarios.py:259`).
  
Esses sítios decidem cobrança/pergunta/pendência e precisam ser distinguidos de um derivador próprio do retido da folha.

**Informativo DP/rescisão existente**

- Existe balde por **pessoa**, `modulo_proprio`, com ajuda “Rescisão na janela” (`app/folha/retrato_folha.py:33`, `:41`, `:89-95`), e etiquetas de rescisão na tela antiga (`app/templates/ponto/_fech_linha.html:4`); não entrega lista de dias sem batida.
- Existe infraestrutura genérica **Pautas** para DP com âncora dia/colaborador/competência (`app/pautas/models.py:30-40`, `:76-89`); escritor aceita departamento e âncora (`app/pautas/services.py:153-185`). Exemplo vigente de dia sem pergunta entregue ao DP: limbo em competência exportada (`app/chamados/services/fabrica_da_celula.py:268-284`).
- C5 é hoje lista de ação/cadastro vinculada a retenção para desligado (`app/escala/services/cadastro_realidade.py:195-200`), não o informativo rescisório pedido.
- Não encontrei superfície dedicada que entregue **dias sem batida de desligado como informativo rescisório** na cópia. Pautas é uma superfície já existente tecnicamente apta a receber um dia destinado ao DP; decisão de produto de usá-la [nao verificado].
- O comentário de `app/folha/retrato_folha.py:1219` diz que o drawer lê `o_que_segura`, mas o código efetivo lê somente o retrato (`app/colaboradores/views.py:979-987`); não repetir o comentário como fato.

## 13/11 11:28 · sessao 62d12c0494c2 · - · - · origem=Ronald (tmux)

Censo direto FECHADO: AST de todos os `.py` da árvore, excluindo diretórios `tests/` e `migrations/`: 939 arquivos, zero erros de parse; cruzamento com `rg` global dos três nomes, imports e chamadas por atributo. Resultado: oito sítios de chamada. A contagem/método vêm da execução, sem artefato salvo [nao verificado].

| Função chamada | Chamador de produção | Uso do retorno |
|---|---|---|
| `motivos_retencao_celula` | `app/folha/export.py:1275` | Compatibilidade `motivos_retencao_espelho`: retorna os motivos do juiz; ignora os três parâmetros antigos (`:1270`). |
| idem | `app/folha/fora_da_folha.py:98`, import `:97` | `_dias_que_retem` repassa `(motivos,dias)`. |
| idem | `app/folha/previa.py:170`, import `:169` | Compara conjunto de dias retidos com dias propostos (`:171`–`:173`); controla “passaria a entrar” (`:119`–`:121`) e contador `mudariam_de_lado` (`:133`). |
| idem | `app/folha/services/vigia_pausa.py:125`, import `:48` | Motivos/dias alimentam `barrados`, `dias_retidos_que_sao_pausa`, `barrados_so_por_pausa`, `livres_com_dia_so_pausa` (`:126`–`:136`). |
| `motivos_retencao_espelho` | `app/folha/export.py:968` | Retorno verdadeiro determina `fora/furo_espelho`, `motivos_espelho`, detalhe e `continue` (`:969`–`:974`). |
| idem | `app/ponto/services/fechamento.py:1067`, import **`as _mre`** `:1066` | Materializa `fechamento.motivos_espelho`; persiste esse campo em `:1186`. |
| `_dias_que_retem` | `app/folha/fora_da_folha.py:137` | Guarda por colaborador, exclusivamente dos itens que `classificar_export` classificou `furo_espelho` (`:119`–`:138`). |
| idem | `app/folha/fora_da_folha.py:217` | `o_que_segura`: monta datas, veredito, motivo, fio, destrava e chamado (`:218`–`:232`), juntamente com status/motivo do juiz do export (`:215`). |

**Cadeias indiretas e sítios de projeção da retenção:**

- `dias_que_seguram` lê `classificar_export` e seleciona motivo `furo_espelho` (`app/folha/fora_da_folha.py:119`, `:128`). `fora_da_folha` usa esse universo (`:161`), conta grupos/fios/dias e publica `datas` (`:171`–`:197`). Não deriva retenção da célula.
- Retrato: `app/folha/retrato_folha.py:1157` lê `fora_da_folha(com_itens=True)`; grava status/motivo/dias/datas/dono/tipo/saldo nas linhas montadas (`:1162`–`:1188`). Leitura do juiz.
- **C5 de desligado:** `app/escala/services/cadastro_realidade.py:185` seleciona desligados; `:191`–`:196` lê `RetratoFolha` pela porta `linhas_do_retrato(r,'bloqueado')`, motivo `furo_espelho`, acumulando `datas`; `:198` exige o dia nessa lavra. Leitura do retrato, não nova derivação de retenção.
- **Vigia retido calado:** `app/folha/vigia_retido_calado.py:46` lê linhas bloqueadas do retrato, `:47` fio `ninguem`, `:48`–`:53` desconta roteados à C5 e conta calados. Leitura do retrato.
- **Vigia fio — alcance:** `app/chamados/services/vigia_fio.py:585` lê `dias_que_seguram`; `:586`–`:589` deduplica e conta dias retidos. Leitura do juiz.
- **API/métrica:** `app/api/views_mensageria.py:257`–`:275` lê `MetricaSnapshot`, inclusive `retidos=denominador-numerador` (`:272`); sem snapshot, pergunta `classificar_export` (`:283`) e conta status/motivos (`:285`–`:301`). A métrica vem do juiz em `app/inteligencia/calculadores.py:210`–`:234` e `:271`–`:282`.
- **Mensageria:** `mensageria/nucleo/ferramentas.py:1247`–`:1268` separa `rescisao_modulo_proprio` como ressalva (`NAO_E_BLOQUEIO`, `:1196`) e soma os outros motivos como “travam de fato”. Projeção da métrica, não leitura de célula.
- **Tela antiga/painel/raio-X:** `app/folha/views.py:259`–`:260` conta furo para aviso; `:331`–`:341` agrupa fora por motivo; `:589`–`:601` separa furo, férias, rescisão e outros. Leitura de `classificar_export`.
- **PDF retidos:** `app/folha/views.py:858`–`:859` pergunta o juiz; `app/folha/retidos_pdf.py:32`–`:38` agrupa status/motivo. `:90` chama `len(motivos_espelho)` de dias; o painel também (`app/folha/views.py:398`). Isso conta **motivos distintos**, não datas retidas, pois o juiz devolve `sorted(motivos)` (`app/folha/export.py:1266`).
- **Coluna folha do “quem cobrar”:** `app/relatorios/views.py:1658` lê juiz, `:1664`–`:1669` projeta status/motivo. **Pendências cadastro:** `app/relatorios/views_pend.py:58`–`:69` lê somente portas de cadastro do juiz.
- **Simulação:** `app/folha/management/commands/simular_folha.py:74` seleciona os não entrantes dos itens do juiz; `:90` registra motivo/horas/prontidão. **Vigia TXT:** `app/folha/services/vigia_txt.py:231` pergunta juiz, `:232` monta TXT com os mesmos itens; `:113`–`:117` leva status/motivo ao diff.
- **TXT:** `app/folha/export.py:714` pergunta juiz, `:716` monta `entra_set`, `:726` exclui os demais. **Validação PDF:** `app/folha/validacao_pdf.py:65`–`:66` usa o mesmo resultado. **Porta de conferência:** `app/folha/porta_export.py:469`–`:475` limita universo aos entrantes. **Dias em aberto:** `app/ponto/services/dias_em_aberto.py:104`–`:108` faz o mesmo.

**Regras paralelas sobre célula/chamado/pergunta: não são chamadas do juiz de retenção. Não confundir “célula acusa” com “retém folha”:**

- `app/chamados/services/vigia_fio.py:331`, `:378`–`:380`: o `censo` antigo conta lâmpadas acusadoras dos chamados vivos; difere do `censo_alcance` que lê retenção (`:585`).
- `app/chamados/services/furo_sem_canal.py:35`, `:70`–`:86`: universo próprio de vereditos `furo/cobrado` sem pergunta aberta nem chamado vivo; não pergunta retenção.
- `app/ponto/services/furos_sem_cobranca.py:61`–`:63`: filtra `furo`, protestos e colaborador `ativo`; `:89`–`:107` separa afastado/vetos/pergunta/supervisão. Pergunta sobre emissão, não retenção.
- `app/chamados/services/fabrica_da_celula.py:112` exclui situação diferente de `ativo`; `:120` lê `VEREDITOS_ACUSAM`; `:123`–`:125` pode mandar apenas à lista do admin.
- `app/chamados/services/disputa_emissao.py:772` usa `VEREDITOS_ACUSAM` para decidir se chamado cuja pergunta foi encerrada ainda deve entrar na fábrica.
- `app/chamados/juizes.py:1375` usa veredito na concordância da resposta; `:1450`–`:1451` pode conservar pergunta viva por célula acusadora. Não consulta retenção.
- Outros leitores de `VEREDITOS_ACUSAM`: `app/chamados/models.py:3358`–`:3359`, `app/chamados/reconciliador.py:642`, `:1028`, `app/chamados/services/limbo_folgas.py:195`, `app/chamados/services/resposta_tardia.py:45`, `app/chamados/services/silencio_ausencia.py:42`; são decisões de fila/vida/resposta/reconciliação, não novo `furo_espelho`.
- Vigias/auditorias de célula também não são retenção: `app/ponto/services/vigia_ausencia_veredito.py:41`, `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:36`, `app/relatorios/dia_da_frota.py:55`, `app/chamados/management/commands/auditar_invariantes_chamados.py:198`, `:343`, `:1391`, `:1605`, `:1647`.

Não encontrei outro escritor literal de `furo_espelho` no código de produção além do ramo de `classificar_export` (`app/folha/export.py:970`); os demais sítios encontrados são leitura, agrupamento, tradução ou comentário. A inexistência foi verificada por AST/`rg`, sem artefato salvo [nao verificado].

_4568 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
