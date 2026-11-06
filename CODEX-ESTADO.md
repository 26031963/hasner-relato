# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 19:07** · publicado em **06/11/2026 19:07** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 19:07 · sessao a1ed65fe462d · - · - · origem=Ronald (tmux)

Contagem: 35 campos de `colaboradores/models.py` e 32 parâmetros do inventário, contando por `(função, nome)`; 22 desses parâmetros satisfazem estritamente `data_*|dia`. A contagem 32 inclui também `data`, `ini`, `fim` e `inicio`, conforme assinaturas citadas abaixo. As quantidades W/L abaixo são **sítios efetivamente lidos**, não totais universais [nao verificado].

| Campo | Significado encontrado | W lidos | L lidos; amostra | Juízo |
|---|---|---:|---|---|
| Empresa.janela_he_desde | Dia inicial da vigência, incluindo primeiro dia de competência: `app/colaboradores/models.py:117`; leitor carrega por `getattr`: `app/colaboradores/services/calendario.py:351`. | 2 portas genéricas: `app/colaboradores/services/estrutura.py:14`, `app/colaboradores/services/estrutura.py:19` | 1: `app/colaboradores/services/calendario.py:351` | Não sei: portas `**campos` não fecham lista de valores/escritores. |
| PreferenciaUI.atualizado_em | Instante de alteração automática: `app/colaboradores/models.py:202`. | 1: `app/colaboradores/models.py:216` | 0 | Não sei: sem leitor temporal lido. |
| Colaborador.data_admissao | Data civil de admissão: `app/colaboradores/models.py:324`; entrada do formulário: `app/colaboradores/views.py:1188`. | 2: `app/colaboradores/views.py:1188`, `app/colaboradores/views.py:1314` | 2: preservação na edição `app/colaboradores/views.py:1314`; histórico inicial `app/colaboradores/views.py:1221` | Sem desacordo na amostra; total de escritores dinâmicos [nao verificado]. |
| Colaborador.data_demissao | Dia de desligamento; futuro agenda, chegada do dia efetiva: `app/colaboradores/services/desligamento.py:57`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:39`. Fim de vínculo recebe esse mesmo dia: `app/colaboradores/services/desligamento.py:123`. | 3: agendamento `app/colaboradores/services/desligamento.py:86`; efetivação `app/colaboradores/services/desligamento.py:105`; limpeza `app/colaboradores/services/desligamento.py:211` | 3: cron `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:39`; células posteriores usam `>` em `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:46`; fechamento `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:56` | Sem desacordo demonstrado: desligamento e última data do vínculo têm funções diferentes, não basta compará-los para afirmar bug. |
| Colaborador.data_nascimento | Data civil fornecida no formulário: `app/colaboradores/views.py:1189`. | 2: `app/colaboradores/views.py:1189`, `app/colaboradores/views.py:1315` | 1: preservação na edição `app/colaboradores/views.py:1315` | Não sei: nenhum leitor de cálculo temporal lido. |
| Colaborador.fcm_removido_em | Instante de remoção do token, `timezone.now()`: `app/colaboradores/fcm.py:61`. | 1: `app/colaboradores/fcm.py:61`, pela porta genérica `app/colaboradores/services/colaborador.py:88` | 0 | Não sei: sem leitor lido. O inventário ESC=0 omite esse escritor por kwargs. |
| Colaborador.notificacao_negada_em | Instante da primeira negativa: `app/ponto/views.py:1959`, `app/ponto/views.py:1962`. | 1: `app/ponto/views.py:1962` | 1, presença do carimbo: `app/ponto/views.py:1959` | Sem desacordo na amostra; leitores B ausentes. |
| Colaborador.aceite_termos_em | Instante do aceite: `app/core/views.py:1106`. | 1: `app/core/views.py:1106` | 0 | Não sei: sem leitor lido. |
| Colaborador.gps_concedido_em | Instante da concessão; leitura booleana de conclusão: `app/core/views.py:1083`, `app/colaboradores/models.py:413`. | 1: `app/core/views.py:1083` | 2: mapa `app/colaboradores/models.py:408` consumido em `app/colaboradores/models.py:413`; `app/core/views.py:483` | Sem desacordo na amostra. |
| Colaborador.camera_concedida_em | Instante da concessão; leitura booleana: `app/core/views.py:1086`, `app/colaboradores/models.py:413`. | 1: `app/core/views.py:1086` | 2: `app/colaboradores/models.py:407`, `app/core/views.py:485` | Sem desacordo na amostra. |
| Colaborador.notif_concedida_em | Instante da concessão: `app/core/views.py:1080`. | 1: `app/core/views.py:1080` | 0 | Não sei: sem leitor lido. |
| Colaborador.telefone_confirmado_em | Instante da confirmação: `app/core/views.py:1128`. | 1: `app/core/views.py:1128` | 0 | Não sei: sem leitor lido. |
| Colaborador.email_confirmado_em | Instante da confirmação: `app/core/views.py:1132`. | 1: `app/core/views.py:1132` | 0 | Não sei: sem leitor lido. |
| Colaborador.criado_em | Instante automático de criação: `app/colaboradores/models.py:380`. | 1 declaração automática: `app/colaboradores/models.py:380` | 0 atribuídos com segurança ao modelo | Não sei: nome genérico; RESUMO não distingue modelo. |
| Colaborador.atualizado_em | Instante automático de gravação: `app/colaboradores/models.py:381`. | 1 declaração automática + porta genérica `app/colaboradores/services/colaborador.py:61` | 0 atribuídos com segurança ao modelo | Não sei: escritas `update()`/`update_fields` não equivalem a toda gravação. |
| HistoricoSalario.data_inicio | Início inclusivo de vigência: `app/colaboradores/models.py:492`; consulta `<=`: `app/colaboradores/models.py:473`. | 0 produtores encontrados fora migrations/tests | 3: busca `app/colaboradores/models.py:473`; bordas `app/colaboradores/models.py:534`, `app/colaboradores/models.py:537` | Não sei: falta produtor efetivo identificado. |
| HistoricoSalario.data_fim | Fim inclusivo na validação: `app/colaboradores/models.py:534`, `app/colaboradores/models.py:537`; `salario_em` ignora-o: `app/colaboradores/models.py:472`. | 0 produtores encontrados | 3: `app/colaboradores/models.py:531`, `app/colaboradores/models.py:532`, ausência de filtro em `app/colaboradores/models.py:472` | **Não sei**, não divergente provado sem escritor. Caso condicional: único salário 01/01–31/01 consultado em 01/02 continua retornando; existência/possibilidade pelo produtor [nao verificado]. |
| HistoricoSalario.criado_em | Instante automático: `app/colaboradores/models.py:507`. | 1 declaração automática | 0 | Não sei: produtor e leitores não identificados. |
| HistoricoVinculo.vigencia_ini | Dia inicial do trecho descrito: `app/colaboradores/services/historico_vinculo.py:15`; usado para localizar vínculo do ajuste: `app/escala/services/escala_auto_executor.py:130`. | 1: `app/colaboradores/services/historico_vinculo.py:15` | 1: `app/escala/services/escala_auto_executor.py:130` | Sem desacordo na amostra. |
| HistoricoVinculo.vigencia_fim | Dia final do trecho descrito, repassado: `app/colaboradores/services/historico_vinculo.py:15`. | 1: `app/colaboradores/services/historico_vinculo.py:15` | 0 | Não sei: sem leitor temporal lido. |
| HistoricoVinculo.data | Data do evento/efeito, não necessariamente data de gravação: agendamento futuro `app/colaboradores/services/desligamento.py:90`; mudança retroativa `app/colaboradores/services/vinculo.py:337`; reativação hoje `app/colaboradores/services/desligamento.py:222`. | 2 produtores: `app/colaboradores/services/historico_vinculo.py:15`, `app/colaboradores/services/historico_vinculo.py:26` | 1: texto “em” `app/colaboradores/models.py:592` | Sem desacordo demonstrado; gravação é campo separado. |
| HistoricoVinculo.criado_em | Instante da gravação; prazo para desfazer usa seu dia local: `app/colaboradores/models.py:584`, `app/escala/services/escala_auto_executor.py:127`. | 1 declaração automática | 2: prazo `app/escala/services/escala_auto_executor.py:127`; recentes `app/escala/services/escala_auto_executor.py:157` | Sem desacordo na amostra; dia local explicitado. |
| Setor.criado_em | Instante automático de criação: `app/colaboradores/models.py:625`. | 1 declaração automática | 0 | Não sei: sem leitor temporal lido. |
| Setor.atualizado_em | Instante automático de gravação: `app/colaboradores/models.py:626`. | 1 declaração automática | 0 | Não sei: sem leitor temporal lido. |
| PostoSupervisor.criado_em | Instante automático: `app/colaboradores/models.py:659`. | 1 declaração automática | 0 | Não sei: sem leitor temporal lido. |
| ContaBancaria.atualizado_em | Instante automático: `app/colaboradores/models.py:685`. | 1 declaração automática | 0 | Não sei: sem leitor temporal lido. |
| HistoricoContaBancaria.alterado_em | Instante automático; `__str__` imprime datetime cru, sem `localtime`: `app/colaboradores/models.py:709`, `app/colaboradores/models.py:720`. | 1 declaração automática | 1: `app/colaboradores/models.py:720` | Não sei: intenção de fuso da apresentação não declarada. Não converter automaticamente isso em divergência. |
| Beneficio.criado_em | Instante automático: `app/colaboradores/models.py:743`. | 1 declaração automática | 0 | Não sei: sem leitor temporal lido. |
| SolicitacaoBeneficio.avaliado_em | Instante da avaliação concluída, `timezone.now()`: `app/colaboradores/services/beneficios.py:31`. | 1: `app/colaboradores/services/beneficios.py:31` | 0 | Não sei: sem leitor temporal lido. |
| SolicitacaoBeneficio.criado_em | Instante de criação, ordenação cronológica: `app/colaboradores/models.py:791`, `app/colaboradores/views.py:2017`. | 1 declaração automática + criação genérica `app/colaboradores/services/beneficios.py:22` | 2: `app/colaboradores/views.py:2004`, `app/colaboradores/views.py:2017` | Sem desacordo na amostra. |
| LinhaTransporte.criado_em | Instante automático: `app/colaboradores/models.py:810`. | 1 declaração automática + criação genérica `app/colaboradores/services/beneficios.py:38` | 0 | Não sei: sem leitor temporal lido. |
| ConfiguracaoVA.criado_em | Instante automático: `app/colaboradores/models.py:839`. | 1 declaração automática + upsert `app/colaboradores/services/beneficios.py:50` | 0 | Não sei: sem leitor temporal lido. |
| DeviceColaborador.primeiro_login | Instante automático do primeiro registro: `app/colaboradores/models.py:919`, criação `app/colaboradores/services/aparelho.py:28`. | 1 declaração automática + criação | 0 | Não sei: sem leitor temporal lido. |
| DeviceColaborador.ultimo_login | Instante automático de registro/relogin: `app/colaboradores/models.py:920`; atualizado explicitamente no save: `app/colaboradores/services/aparelho.py:53`. | 1 criação + 1 save: `app/colaboradores/services/aparelho.py:28`, `app/colaboradores/services/aparelho.py:53` | 4: `app/colaboradores/management/commands/backfill_plataforma_fossil.py:50`, `app/colaboradores/painel_cell.py:29`, `app/colaboradores/services/ficha.py:140`, `app/colaboradores/services/situacional.py:21` | Sem desacordo na amostra: todos escolhem aparelho por recência. |
| DeviceColaborador.revogado_em | Carimbo anulável, limpo ao reativar aparelho: `app/colaboradores/services/aparelho.py:50`. | 1 limpeza: `app/colaboradores/services/aparelho.py:50` | 0 temporal | Não sei: só encontrei limpeza; escritor de revogação positiva não localizado. |

Os oito sítios persistentes de `EscalaColaborador.data_ancora_colaborador` lidos estão em `app/colaboradores/services/vinculo.py:263`, `:272`, `:284`, `:915`, `:923`, `:937`, `:1012`, `:1027`. `app/colaboradores/management/commands/corrigir_escala_retroativa.py:110` só altera `_ec_proj` em memória; `app/colaboradores/services/vinculo.py:925` apenas inclui o campo já atribuído em `update_fields`. Não são dez escritores persistentes.

Os cinco sítios persistentes de `EscalaColaborador.data_inicio` lidos estão em `app/colaboradores/services/vinculo.py:282`, `:914`, `:955`, `:1011`, `:1026`. Os oito de `data_fim` estão em `app/colaboradores/services/vinculo.py:268`, `:615`, `:691`, `:798`, `:914`, `:924`, `:955`, `:1021`. `:705` atualiza memória após o update de `:691`; `:799`/`:925` são salvamentos das atribuições já contadas. Fim é inclusivo: troca fecha em D−1 em `app/colaboradores/services/vinculo.py:202`, e vigência aceita igualdade em `app/colaboradores/services/vinculo.py:546`.

| Parâmetros, por função | Quantidade | Semântica e evidência | Juízo na amostra |
|---|---:|---|---|
| `Colaborador.salario_em(data)` | 1 | Dia civil, default `date.today()`, início `<=`: `app/colaboradores/models.py:465`, `:471`, `:473`. | Não sei: problema de `data_fim` acima. |
| `agenda_bloco_criar(data)`; `agenda_edit_dia_view(data)` | 2 | String ISO da URL convertida para `date`: `app/colaboradores/views.py:2276`, `:2422`. | Sem desacordo demonstrado. |
| `resolver_dia_oferecido(dia)` | 1 | **Dict de apresentação**, não unidade temporal; compara `dia['data']` com hoje: `app/colaboradores/services/calendario.py:97`, `:103`. | Sem desacordo; não contar como campo date. |
| `afastados_hoje(dia)`; `palavra(dia)`; `anotar(dia)` | 3 | Dia civil local/de referência, passado ao mesmo juiz: `app/colaboradores/services/situacao_exibida.py:45`, `:46`, `:63`, `:75`, `:77`. | Sem desacordo demonstrado. |
| `_post(data_inicio,data_ancora)`; `precisa_inicio_do_turno(data_inicio)`; `checar_troca(data_inicio,data_ancora)`; `trocar_escala_porta(data_inicio,data_ancora)` | 7 | Início da vigência e dia de fase do ciclo; serialização ISO e normalização: `app/colaboradores/services/troca_pelo_chat.py:12`, `:14`, `:47`, `:58`, `:69`, `:71`. | Sem desacordo demonstrado. |
| `validar_vigencia(data_inicio,data_fim)`; `fechar_vigencia(data_fim)`; `reabrir_vigencia_impossivel(data_fim)`; `absorver_vigencias_posteriores(data_inicio)`; `encerrar_vinculo(data_fim)`; `fechar_vinculos_ativos(data_fim)`; `criar_trecho_retroativo(data_inicio,data_fim)`; `corrigir_vinculo_retroativo(data_fim)`; `invalidar_ancoras_incoerentes(data_inicio)`; `ajustar_vinculo_pelo_sistema(inicio)` | 12 | Dias de vigência com bordas inclusivas; antes do começo usa D−1; invalidação da âncora usa limiar `<`: `app/colaboradores/services/vinculo.py:546`, `:615`, `:685`, `:743`, `:789`, `:818`, `:911`, `:921`, `:936`, `:1009`. | Sem desacordo demonstrado. Âncora anterior ao início novo pode ser preservada como fase: `:1012`, `:1027`. |
| `registrar_mudanca(data)`; `registrar_evento_de_vinculo(data)` | 2 | Dia do evento/efeito, repassado: `app/colaboradores/services/historico_vinculo.py:15`, `:26`. | Sem desacordo demonstrado; não confundir com criado_em. |
| `eh_feriado_do_posto(data)`; `cobra_no_dia(data)`; `presenca_em_feriado(ini,fim)` | 4 | Dia civil; janela de dias inclusiva: `app/colaboradores/services/feriado_do_posto.py:57`, `:71`, `:95`. | Sem desacordo demonstrado. |

Divergências comprovadas em leitores do grupo B:

1. **`ponto.Ausencia.data_fim` — família `ausencia/ferias`.** Escritor deriva o último dia incluído como `data + dias − 1` em `app/ponto/services/ausencia.py:77`, usa o resultado em `app/ponto/services/ausencia.py:197` e grava em `app/ponto/services/ausencia.py:343`. Leitor usa esse último dia como `retorno` em `app/colaboradores/views.py:2924`; apresentação imprime “retorno est.” em `app/templates/colaboradores/partials/disponibilidade.html:55`. Juiz confirma cobertura inclusiva em `app/ponto/turnos.py:2554`. Caso deduzido: ausência 06/11/2026 por três dias grava fim 08/11; tela informa retorno 08/11, ainda coberto; primeiro dia fora da ausência seria 09/11. Ocorrência em produção [nao verificado]. Família consta de `app/core/contratos_estruturais.py:50`.

2. **Extra fora das declarações CAMPO: `ferias.AgendamentoFerias.data_fim` — família `ausencia/ferias`.** Escritor calcula último dia como `início + dias − 1` em `app/ferias/services.py:313` e grava em `app/ferias/services.py:322`; leitor apresenta esse dia como retorno em `app/colaboradores/views.py:2904`, via o mesmo template `app/templates/colaboradores/partials/disponibilidade.html:55`. A própria rota, no outro bloco, usa corretamente fim+1 em `app/colaboradores/views.py:2853`. Entrada 01/11/2026, dez dias → fim 10/11 → “retorno est. 10/11”; outro bloco informa 11/11. Ocorrência em produção [nao verificado].

Não promover `HistoricoSalario.data_fim` nem o formato cru de `HistoricoContaBancaria.alterado_em` a divergência comprovada sem as evidências faltantes descritas no quadro.

## 06/11 19:07 · sessao 98323398dab6 · - · - · origem=Ronald (tmux)

Contagens abaixo são sítios explícitos encontrados, incluindo portas dinâmicas; não são o `ESC` sintático do anexo. O caso `restaurar_fechamento` pode escrever qualquer campo recebido, exceto quatro identificadores (`app/ponto/services/fechamento.py:1364`, `:1377`).

| Campo | Significado/escritores abertos | Leitores abertos, amostra | Divergente |
|---|---|---|---|
| `Batida.timestamp_dispositivo` | Instante bruto do celular, nullable (`app/ponto/models.py:75`). **0 escritor identificado em ponto/chamados**; porta genérica `registrar_batida(..., **campos)` aceita-o (`app/ponto/registro_batida.py:94`, `:153`, `:158`). Alimentação encontrada fora A: `app/api/views.py:695`, `app/api/views_core.py:692`. | Não identifiquei leitor temporal em A. [nao verificado] | **Não sei**, cobertura A insuficiente. |
| `Batida.sincronizado_em` | Instante de chegada ao servidor declarado no modelo (`app/ponto/models.py:77`); porta dinâmica de criação `registro_batida.py:153`, `:158`. **0 alimentação concreta identificada**. [nao verificado] | 0 leitor identificado. [nao verificado] | **Não sei**. |
| `Batida.criado_em` | Instante automático de criação (`app/ponto/models.py:79`); criação pelas duas ramificações `registro_batida.py:153`, `:158` e fábrica dinâmica `:335`. `**campos` também permite valor explicitamente passado, mas não localizei alimentação de `criado_em`. [nao verificado] | Último instante de mutação (`app/chamados/veredito_lavrado.py:137`); sequência criada após um instante (`app/ponto/turnos.py:2382`). **2 amostras**. | Sem discordância na amostra. |
| `Batida.aprovada_em` | Instante de aprovação agora: **5 alimentações concretas** — `app/ponto/views.py:473` → `anotar` → `registro_batida.py:253`; `app/ponto/services/justificativa.py:352`; `app/chamados/services/materializacao.py:908`, `:993`; `app/chamados/services/regularizacao_ext.py:80` → criação `registro_batida.py:153`, `:158`. | Nenhum leitor temporal identificado em A. [nao verificado] | **Não sei**. |
| `Batida.retratada_em` | Instante da retratação, agora (`app/ponto/registro_batida.py:274`) → escritor dinâmico `:253`/`:254`; retorno a `None` ao desfazer (`app/ponto/services/retratar_humano.py:207`). **2 alimentações**, 1 porta física. | Vigência por null (`app/ponto/turnos.py:2864`); prazo datetime `retratada_em+timedelta(days=...)` (`app/ponto/services/retratar_humano.py:53`), limite permitido inclusive: só recusa `now()>prazo` (`:203`); máximo de mutação (`app/chamados/veredito_lavrado.py:140`). **3 amostras**. | Sem discordância na amostra. |
| `ImportacaoAEJ.criado_em` | Automático, criação (`app/ponto/models.py:215`). Não localizei criação concreta em A. [nao verificado] | Ordenação pelo instante (`app/ponto/models.py:221`); exibição admin (`app/ponto/admin.py:27`). **2 amostras**, admin não constava no anexo A. | Sem discordância na amostra; cobertura de escritores **não sei**. |
| `ImportacaoAEJ.concluido_em` | Nullable sem escritor concreto identificado em A (`app/ponto/models.py:216`). | Nenhum leitor identificado. [nao verificado] | **Não sei**. O resumo `ESC=11` casa homônimos de holerite/lote, não autoriza atribuí-los a este modelo (`app/relatorios/lote_espelho.py:253`; `app/holerite/views.py:694`). |
| `AlertaGeofence.criado_em` | Automático (`app/ponto/models.py:244`); **5 criações abertas** em `app/ponto/services/geofence.py:249`, `:273`, `:280`, `:291`, `:298`. | Ordenação e string do instante (`app/ponto/models.py:275`, `:278`), **2 amostras**. | Sem discordância na amostra. |
| `AlertaGeofence.obsoleto_em` | Instante da classificação de obsolescência, `timezone.now()` (`app/ponto/management/commands/reconciliar_alertas_geofence.py:50`, `:52`), **1 escritor**. | Null = ainda não classificado pelo comando (`:33`), **1 leitor**. | Sem discordância na amostra. |
| `PingGeo.timestamp` | Instante de recepção no servidor: `agora=tz.now()` (`app/ponto/views.py:2946`) → criação (`:2989`), **1 escritor concreto**. | Ordenação (`app/ponto/views.py:3008`; `app/ponto/presenca.py:109`) e exposição do mesmo datetime (`presenca.py:120`), **3 amostras**. | Sem discordância na amostra. |
| `PingGeo.criado_em` | Automático de criação (`app/ponto/models.py:1015`), criação concreta `app/ponto/views.py:2986`. | Nenhum leitor identificado. [nao verificado] | **Não sei**. |
| `FotoBatida.criado_em` | Automático (`app/ponto/models.py:1031`); **2 portas de criação** `app/ponto/registro_batida.py:306`, `:311`. | Nenhum leitor identificado. [nao verificado] | **Não sei**. |
| `PushSubscription.criado_em` | Automático (`app/ponto/models.py:552`), criação via upsert `app/ponto/views.py:1988`. | Nenhum leitor identificado. [nao verificado] | **Não sei**. |
| `Advertencia.criada_em` | Automático (`app/ponto/models.py:1179`), criação concreta `app/chamados/services/acoes_chamado.py:193`. | Nenhum leitor temporal identificado. [nao verificado] | **Não sei**. |
| `PerfilApuracao.criado_em` | Automático (`app/ponto/models.py:983`); escritor concreto não identificado em A. [nao verificado] | Nenhum leitor temporal identificado. [nao verificado] | **Não sei**. |
| `PerfilApuracao.atualizado_em` | Automático a cada save (`app/ponto/models.py:984`); escritor concreto não identificado em A. [nao verificado] | Nenhum leitor temporal identificado. [nao verificado] | **Não sei**. |
| `FechamentoMensal.previsto_em` | Instante da derivação do previsto ou `None`: atribuições `app/ponto/services/fechamento.py:1070`, `:1083`; persistências alternativas `:1128`, `:1172`; invalidação `:1597`; restauração dinâmica `:1377`. **5 expressões escritoras**, sem contar save como escritor adicional. | Presença do carimbo como previsto ainda válido (`:1595`); leitura para persistir o mesmo valor (`:1172`). **2 amostras**. | Sem discordância na amostra. |
| `FechamentoMensal.derivado_em` | Instante da derivação do registro inteiro ou `None` (`app/ponto/models.py:454`–`:464`). **4 sítios**: `app/ponto/services/fechamento.py:1127`, `:1173`, `:1640`, restauração `:1375`/`:1377`. | Null = projeção vencida (`:1637`, `:1680`), **2 amostras**. | Sem discordância na amostra. |
| `FechamentoMensal.aprovado_em` | Instante de aprovação agora ou `None`. **6 sítios**: dict dinâmico `app/ponto/services/fechamento.py:1313` → `:1316`; `:1323`, `:1330`, `:1337`, `:1436`; restauração dinâmica `:1377`. | Nenhum leitor temporal identificado em A. [nao verificado] | **Não sei**. |
| `FechamentoMensal.criado_em` | Automático de nascimento (`app/ponto/models.py:478`), mais restauração dinâmica potencial `app/ponto/services/fechamento.py:1377`. | Nenhum leitor temporal identificado em A. [nao verificado] | **Não sei**. |
| `FechamentoMensal.atualizado_em` | Instante de toque, não de derivação (`app/ponto/models.py:457`–`:459`). **1 automático** (`:479`) + **5 writes explícitos** `app/ponto/services/fechamento.py:1145`, `:1377`, `:1436`, `:1597`, `:1640`. | Nenhum leitor temporal desse modelo identificado em A. [nao verificado] | **Não sei**. |
| `PeriodoFechado.fechado_em` | Instante da tranca atual: automático ao nascer (`app/ponto/models.py:926`, criação `app/ponto/services/fechamento.py:1392`) e sobrescrito ao retrancar (`:1399`). **2 mecanismos**. | Comparação com resgate posterior (`app/chamados/services/competencia_trancada.py:104`) e limite para toque humano posterior (`app/chamados/juizes.py:954`), **2 amostras**. | Sem discordância na amostra. |
| `PeriodoFechado.reaberto_em` | Instante da reabertura `now()` (`app/ponto/services/fechamento.py:1423`), limpo ao retrancar (`:1401`), **2 sítios**. | Null = tranca viva (`:1454`, `:1464`), **2 amostras**. | Sem discordância na amostra. |

A única divergência comprovada por mim continua a de `Batida.timestamp` no recorte cru de `fechamento.py:245`/`:252`, já enviada. Os demais “sem discordância” significam apenas a amostra declarada, sem prova universal de todos os leitores. [nao verificado]

## 06/11 19:07 · sessao c8c62c65720e · - · - · origem=Ronald (tmux)

Achado confirmado no recorte: `contexto_json.data_turno` do chamado C9 significa dia civil da batida na escrita e dia da célula na leitura. Escritores: `app/api/views.py:745` e `app/api/views_core.py:745`; leitor: `app/chamados/models.py:1195` e `app/chamados/models.py:1201`, via `app/chamados/juizes.py:111`. Família: **chamado**, com efeito em **celula/precedencia** (`app/core/contratos_estruturais.py:48`).

Caso concreto condicional: turno iniciado em **05/11/2026 19h**, saída prevista **06/11 07h**, saída efetiva **06/11 07h20**. O detector encontra o turno de **05/11**, calcula desvio de 20 minutos e retorna `leve`; a API escreve `data_turno="2026-11-06"` e a junção procura a célula de **06/11**. É consequência estática do código, sem confirmação de ocorrência em produção (`app/chamados/detector_anomalias.py:161`, `app/chamados/detector_anomalias.py:197`, `app/chamados/detector_anomalias.py:225`, `app/chamados/detector_anomalias.py:243`, `app/api/views.py:745`, `app/chamados/models.py:1201`).

| Campo efetivamente lido | Escritores locais abertos | Amostra de leitores aberta | Resultado no recorte |
|---|---|---|---|
| `Feriado.data` | **3**: `app/core/views_config.py:63`, `app/core/views_config.py:82`, `app/core/views_config.py:142`; entrada convertida com `date.fromisoformat` em `:45` e `:134` | **3 expressões**: limites `>=`/`<=` em `app/core/feriados.py:29`; conjunto de dates em `:83` | Não encontrada divergência: date civil e intervalo inclusivo (`app/core/feriados.py:29`). |
| `PingGeo.timestamp` | **2**: `app/api/views.py:2432`, `app/api/views_core.py:970`; origem `tz.now()` em `:2370`/`:908` | **4 expressões**: `app/api/views.py:2449`, `:2451`; `app/api/views_core.py:987`, `:989` | Não encontrada divergência: instante e janela de duas horas com início inclusivo (`app/api/views_core.py:987`). |
| `Justificativa.data_turno` | **1**: `app/api/views.py:2010`; entrada date em `:1963` | **3 expressões**: igualdade em `app/api/views.py:1998`, duas cargas na apresentação em `:2112` | Amostra preserva o date recebido; outros escritores fora do recorte não foram integralmente lidos: **não sei** quanto ao conjunto total (`app/api/views.py:1963`, `:2010`, `:2112`). |
| `Batida.timestamp` | **0 diretos locais**; duas chamadas semânticas da API encaminham `ts_efetivo`; porta tem dois `create`: `app/ponto/registro_batida.py:153`, `:158` | `app/api/views_core.py:284`, `:287`, `:553`, `:557`, `:562`; `app/core/views.py:182`; `app/core/services/painel_op.py:175`, `:220` | Amostra coerente entre instante, apresentação local e dia civil local. **Não sei** global: chamadores externos da porta não foram todos lidos; semeadura aceita dicts em ensaio (`app/ponto/registro_batida.py:335`). |
| `Batida.timestamp_dispositivo` | Duas chamadas semânticas encaminham o datetime aware lido; exemplo `app/api/views_core.py:692`; parser torna naive aware em `app/api/hora_do_aparelho.py:116` | Diferença entre instantes em `app/api/hora_do_aparelho.py:179` e filtro não nulo em `:182` | Amostra coerente; **não sei** global por cobertura externa incompleta (`app/api/hora_do_aparelho.py:179`). |
| `Colaborador.notif_concedida_em` | **1**: `app/core/views.py:1080`, com `agora=timezone.now()` em `:1077` | **0 leitores locais no inventário** [não verificado fora dele] | Sem par divergente disponível [não verificado]. |
| `Colaborador.gps_concedido_em` | **1**: `app/core/views.py:1083` | **1**, teste de presença em `app/core/views.py:483` | Instante de concessão escrito; leitor só verifica existência (`app/core/views.py:1083`, `:483`). |
| `Colaborador.camera_concedida_em` | **1**: `app/core/views.py:1086` | **1**, teste de presença em `app/core/views.py:485` | Instante de concessão escrito; leitor só verifica existência (`app/core/views.py:1086`, `:485`). |
| `Colaborador.telefone_confirmado_em` | **1**: `app/core/views.py:1128`, `timezone.now()` | **0 leitores locais no inventário** [não verificado fora dele] | Sem par divergente disponível [não verificado]. |
| `Colaborador.email_confirmado_em` | **1**: `app/core/views.py:1132`, `timezone.now()` | **0 leitores locais no inventário** [não verificado fora dele] | Sem par divergente disponível [não verificado]. |
| `Colaborador.aceite_termos_em` | **1 escrita em dict**, invisível ao ESC anexo: `app/core/views.py:1106`, `timezone.now()` | **0 leitores locais no inventário** [não verificado fora dele] | Sem par divergente disponível [não verificado]. |
| `PlacarNavbar.lavrado_em` | **1**: `app/core/management/commands/lavrar_badge_navbar.py:93`, `timezone.now()` | **3 expressões**: presença e idade em `app/core/context_processors.py:124`, `:126`; apresentação em `app/core/models.py:476` | Instante de lavra e idade em minutos concordam (`app/core/context_processors.py:126`). |
| `CredencialUsuario.atualizado_em` | **2 saves**: `app/api/credencial.py:101`, `:165`; `auto_now` em `app/core/models.py:275` | **0 locais inventariados** [não verificado fora deles] | Não atribuir aos modelos homônimos CAMPO; `_estado` retorna `CredencialUsuario` (`app/api/credencial.py:79`, `:81`). |
| `PerguntaDisputa.data_evento` | Nenhum escritor local nos arquivos API abertos; origem externa examinada pelo agente principal [não verificado neste relatório] | **4 sítios / 7 cargas**: `app/api/views.py:1168`, `:1169`, `:2648`; `app/api/views_mensageria.py:568` | API apresenta fielmente o date com ISO/strftime; não transforma em instante nem muda fuso (`app/api/views.py:1168`, `:1169`). |
| `PerguntaDisputa.respondida_em` | Escrita delegada à porta em `app/api/views.py:2643`; escritores externos não integralmente lidos [não verificado] | ISO em `app/api/views.py:1175`, `:2653`; existência em `:1187`, `:2636`; duração em `app/api/views_mensageria.py:570` | Amostra preserva instante; **não sei** global por cobertura de escritores (`app/api/views_mensageria.py:569`, `:570`). |
| `ChamadoColaborador.criado_em` / `atualizado_em` | Escritores externos não integralmente lidos [não verificado] | Agrupamento por dia local e ordem por instante em `app/api/views.py:1020`, `:1021`; apresentação local em `:1151`, `:1152`, `:1153` | **Não sei** global; amostra distingue explicitamente dia local de instante (`app/api/views.py:1020`, `:1021`). |
| `FollowupChamado.criado_em` / `lido_colaborador_em` | Escritores externos não integralmente lidos [não verificado] | Ordem em `app/api/views.py:1128`, data local em `:1138`, presença em `:1131` | **Não sei** global; sem desacordo na amostra (`app/api/views.py:1128`, `:1131`, `:1138`). |
| `Ausencia.data` / `data_fim` | API calcula duração inclusiva e delega: `app/api/views.py:1806`, `:1808`, `:1821`; escritores externos não integralmente lidos [não verificado] | Data apresentada sem conversão em `app/api/views.py:1874` | **Não sei** global; borda da entrada API inclusiva (`app/api/views.py:1808`). |
| `RegularizacaoItemBatida.data` / `RegularizacaoExterna.criada_em` | Escritores externos não integralmente lidos [não verificado] | Serialização direta ISO em `app/api/views.py:2981`, `:2986`; clone `app/api/views_core.py:1299`, `:1304` | **Não sei** global; amostra não muda unidade ou dia (`app/api/views.py:2981`, `:2986`). |

Os **25 parâmetros nomeados** efetivamente abertos, contados por identidade função+nome, são:

- `_motivo_da_lei.data`, dois arquivos: `app/api/views.py:387` e `app/api/views_core.py:374`; ambos delegam o dia ao juiz; a implementação delegada chama `classificar_falta(colaborador, data)` (`app/escala/servico_jornada.py:549`, `:557`).
- `_aviso_recusa_ausencia.data`: dia da ausência, usado como ISO em chave de deduplicação; o dia da emissão do aviso é separadamente `localdate()` (`app/api/views.py:1921`, `:1932`, `:1933`).
- Dez em `core/feriados`: `_qs.data_ini/data_fim` (`app/core/feriados.py:27`); `feriados_de.data_ini/data_fim` (`:70`); `carregar.data_ini/data_fim` (`:86`); `eh_feriado.data` (`:91`); `nome_do_feriado.data` (`:98`); `feriados_do_colab.data_ini/data_fim` (`:125`). São dates e intervalos inclusivos (`:29`, `:140`).
- Três em `core/intake`: `proximo_dia.dia` (`app/core/intake.py:315`), `_data_ou_nada.dia` (`:376`), `dias_da_frase._para_tras.dia` (`:411`). São **inteiros do dia do mês**, não dates; o primeiro procura estritamente depois de hoje (`:321`), o último resolve retrospectivamente (`:415`).
- Quatro `dia` em `pipeline_placar`: `momentos` (`app/core/management/commands/pipeline_placar.py:29`), `esperados` (`:38`), `medidos` (`:60`), `placar` (`:84`). São dates da execução; `medidos` compara o prefixo ISO do registro (`:70`). Fuso real dos registros não conferido: **não sei** [não verificado].
- Dois `data` em `placar_tickets`: `_dia` (`app/core/placar_tickets.py:157`) e `idade_em_dias` (`:170`). São **strings `dd/mm`**, com ano inferido e recuado se futuro (`:164`, `:167`).
- Três `dia` em `recusa_critica`: `registrar` recebe dia do **fato** (`app/core/recusa_critica.py:66`, `:75`); `contar` e `resolver_dia_recusado` recebem dia local do **registro**, filtrando `d['data']` (`:104`, `:113`, `:122`). São parâmetros de funções distintas, com significados documentados distintos; isso isoladamente não constitui escritor e leitor discordando sobre o mesmo dado (`:70`, `:105`).

Adicionais do inventário, fora desses 25 nomes: `sombra_cobertura.cobre.ini/fim`, dois dates com contenção inclusiva (`app/core/sombra_cobertura.py:32`, `:47`); os argumentos `resposta_acao.dia` e `resposta_render.dia` apenas encaminham o dia do fato para `registrar` (`app/core/respostas.py:56`, `:80`).

Não confundir o `timestamp` do inventário em `app/core/esteira_vigia.py:708` com `Batida.timestamp`: ali é a chamada ao método `datetime.timestamp()` de uma linha JSON (`app/core/esteira_vigia.py:708`). Também não confundir `request.data` com DateField; exemplo explícito em `app/api/views.py:1955`.

## 06/11 19:06 · sessao 5a9319f4b229 · - · - · origem=Ronald (tmux)

Achados consolidados para o quadro:

| Campo | Significados e escritores abertos | Amostra de leitores aberta | Resultado |
|---|---|---|---|
| `PerguntaDisputa.data_evento` | **Dia civil da saída esperada**: `app/escala/servico_jornada.py:483`, derivação `:491` → `data_saida_esperada:203–209`, usada em `app/chamados/services/disputa_emissao.py:1041`, transmitida a `abrir` em `:777`. **Dia da célula/jornada**: `app/chamados/services/declaracao_texto.py:123`, `:299`, `:326`, `:615`. **Dia local da conversa**: `app/chamados/models.py:2427`. Persistências efetivas abertas: **2 sítios**, `app/chamados/models.py:2476` e `app/chamados/services/fio.py:834` → `:847`; `fio.py:843` desfaz somente em memória. | **6 sítios**: identidade `app/chamados/models.py:2393`; célula `:2430`; grade `app/chamados/services/materializacao.py:282`; slot e overnight `:414`, `:420`; jornada `app/chamados/juizes.py:371`; igualdade com TM `:1479`. | **Sim**. Entrada 05/11/2026 19h, saída esperada 06/11 07h: produtor grava `06/11`, porém o turno fechado tem `data_turno=05/11`. `_turno_tem_saida` procura somente `06/11` e devolve `False` (`juizes.py:371–372`); `via_pelo_fato` pode cair em `chamado_encerrado` (`:1479`, `:1486`). Incidência em produção [nao verificado]. |
| `PerguntaDisputa.janela_expira_em` | **Data civil de virada da janela**: `app/chamados/veredito_lavrado.py:112–118`, escrita `:229`; backfill calcula mesma derivação em `app/chamados/management/commands/backfill_veredito_lavrado.py:111`, atribui `:140`. **2 atribuições persistidas**, mais restauração em memória `veredito_lavrado.py:245`. | **3 sítios**: `app/chamados/veredito_lavrado.py:182`; `app/chamados/services/raiox.py:36–41`; relatório usa `timezone.localdate()` em `app/chamados/management/commands/backfill_veredito_lavrado.py:94`, compara em `:113`. | **Sim, quanto ao calendário empregado na comparação**. `esta_stale` e raio-X usam `.date()` de `timezone.now()`; relatório usa `localdate()`. Configuração da cópia: São Paulo, `USE_TZ=True`, `app/config/settings/base.py:93`, `:95`. |
| `PerguntaDisputa.julgado_em` | **Instante da lavra**: `app/chamados/veredito_lavrado.py:228`; backfill `app/chamados/management/commands/backfill_veredito_lavrado.py:95`, `:139`. **2 atribuições persistidas**, mais restauração em memória `veredito_lavrado.py:244`. | Instante comparado corretamente com marco em `app/chamados/veredito_lavrado.py:189`; convertido diretamente em dia em `:185` e `app/chamados/services/raiox.py:41`. | **Sim**. Para `expira=21/11`, `agora=21/11 01:00Z` (=20/11 22h local), `julgado=20/11 20:00Z`, `esta_stale=True` três horas antes da virada local (`veredito_lavrado.py:182–186`). Uma lavra nessa faixa ganha dia UTC 21 e pode deixar de ser invalidada na virada local (`:185`, `:228`). TZ real do processo e incidência em produção [nao verificado]. |

Os pares divergentes pertencem à família **`chamado`**, declarada em `app/core/contratos_estruturais.py:51`, com consulta do domínio em `app/docs/JUIZES.md:54–58`. A borda de meia-noite aparece expressamente como “SEM juiz” em `app/docs/JUIZES.md:76–77`.

Os outros **25 campos** de modelos de chamados foram abertos; na amostra abaixo não observei divergência dos eixos pedidos. Isso não comprova universalidade dos leitores [nao verificado]:

| Campo | Escritores abertos, sem contar `update_fields` novamente | Leitores abertos |
|---|---|---|
| `ChamadoColaborador.atribuido_em` | 2: `app/chamados/services/vinculos.py:130`, `:141`; instante/null | `:137` |
| `.atualizado_em` | auto `app/chamados/models.py:62`; 8 operações: `:466`, `:520`, `:557`, `:575`, `:603`; `app/chamados/services/arquivo.py:169`, `:207`; `app/chamados/services/texto.py:34` | `app/chamados/models.py:1139` |
| `.criado_em` | auto `app/chamados/models.py:61` | `app/chamados/services/painel.py:354–356` recorte local por dia |
| `.escalonado_em` | 1: `app/chamados/models.py:574`, instante recebido | `app/chamados/services/fila.py:181` |
| `.prazo_sla` | **≥6 fontes**: `app/chamados/models.py:556`; `app/chamados/services/disputa_emissao.py:517`, `:956`, `:1176`; `app/ponto/management/commands/detectar_ausencias.py:704`; **omitido no inventário** `app/chamados/services/abertura.py:154`, `:182` | `app/chamados/services/cobranca.py:85–86`, instante > agora |
| `.resgatado_em` | 1: `app/chamados/services/arquivo.py:205`, primeiro instante | `:196`, `:204` |
| `.resolvido_em` | 2: `app/chamados/models.py:238`, `:242`, instante/null | `:1303`, `:1307` |
| `.silenciado_ate` | 2: `app/chamados/services/arquivo.py:166`, `:201`, limite/null | `app/chamados/models.py:480`, estrito `> agora` |
| `DisputaSupervisao.aberta_em` | auto `app/chamados/models.py:1686` | `:1760` ordenação |
| `.fechada_em` | 3: `app/chamados/models.py:1861`, `:2063`, `:2087`, instante/null | `:1765` |
| `FollowupChamado.criado_em` | auto `app/chamados/models.py:1604` | `app/chamados/services/painel.py:354–356` |
| `.lido_dp_em` | **1 escritor dinâmico omitido no inventário**: `app/chamados/services/nota.py:59`, `:131–133`, now | `app/chamados/services/painel.py:363` |
| `.lido_colaborador_em` | mesmo sítio dinâmico, `app/chamados/services/nota.py:59`, `:131–133` | `app/chamados/views.py:603` |
| `PerguntaDisputa.materializada_em` | 2 atribuições: `app/chamados/models.py:2558`, `:2700`; quando/now/null, chamadores `:2581`, `:2639`, `:2648` | `app/chamados/juizes.py:647`, `:673` |
| `.respondida_em` | 5 persistentes: `app/chamados/models.py:2589`; `app/chamados/services/disputa_emissao.py:1283`, `:1393`→`:1416`, `:1434`, `:1450`; memória `models.py:1457`, `:2620` | `app/chamados/models.py:2503`, `app/chamados/juizes.py:1150` |
| `.validada_em` | 5 persistentes: `app/chamados/models.py:2592`; `app/chamados/services/disputa_emissao.py:1295`, `:1393`→`:1416`; `app/chamados/services/validacao.py:131`, `:188`; memória `models.py:1456`, `:2621` | `app/chamados/models.py:2499`, `app/chamados/juizes.py:1136` |
| `.data_conferida_em` | 1: `app/chamados/services/validacao.py:617`, now | `app/chamados/services/materializacao.py:1168` |
| `RegularizacaoExterna.criada_em` | auto `app/chamados/models.py:3218` | `app/chamados/services/regularizacao_ext.py:287` |
| `.confirmada_em` | 1: `app/chamados/services/regularizacao_ext.py:354`, now | Sem leitor mostrado no anexo [nao verificado] |
| `.materializada_em` | 1: `app/chamados/services/regularizacao_ext.py:198`, now | `:219` |
| `RegularizacaoItemBatida.aplicada_em` | 2: `app/chamados/services/regularizacao_ext.py:138`, `:162`, now | `:125` |
| `.data` | 2: `app/chamados/services/regularizacao_ext.py:242`, `:326`, dia civil fornecido | `:72`, `:149` compõem instante local; `:172`, `:176` intervalo no mesmo dia |
| `RespostaAnterior.respondida_original_em` | 1: `app/chamados/services/disputa_emissao.py:1407`, copia instante anterior | Sem leitor no anexo [nao verificado] |
| `.validada_original_em` | 1: `app/chamados/services/disputa_emissao.py:1408`, copia instante anterior | Sem leitor no anexo [nao verificado] |
| `.versao_reaberta_em` | 1: `app/chamados/services/disputa_emissao.py:1402`, `:1408`, now | Ordenação em `app/chamados/models.py:3191` |

Observações para o denominador:

- `data` de FCM é **payload dict**, sem significado temporal: `app/chamados/fcm_utils.py:50`, `:75`, `:102`.
- `data_ini`/`data_fim` em `chamados_sem_dia_do_fato` são **ignorados pelo corpo**: `app/chamados/services/juizes_do_dia.py:75`, consulta sem recorte `:92–99`. Classificar significado como **não sei**, sem consumidor.
- `prazo_sla` de abertura recebe **23:59:00**, embora a docstring diga “fim do dia”: `app/chamados/services/abertura.py:133`, `:154`. Às 23:59:30 já fica fora de `> agora` (`app/chamados/services/cobranca.py:86`). Política desejada para esses 59 segundos [nao verificado].
- Contagem global de leitores por modelo não foi resolvida; os `RESUMO` genéricos não permitem esse número [nao verificado].

## 06/11 19:06 · sessao b0eb35b9c7d3 · - · - · origem=Ronald (tmux)

Resultados para consolidação, somente leitura.

| Campo | Significados e evidência | Escritores abertos | Leitores B: contagem nominal; amostra aberta | Divergente? | Caso concreto |
|---|---|---|---|---|---|
| `Batida.timestamp` | Instante usado em janelas de ±60s, `app/ponto/registro_batida.py:106`; correção combina dia e hora locais aware, `app/chamados/services/regularizacao_ext.py:149`; apresentação converte para local, `app/colaboradores/services/detalhe.py:85`; `hub_smoke` extrai dia cru, `app/escala/management/commands/hub_smoke.py:100`. Fuso configurado São Paulo e `USE_TZ=True`, `app/config/settings/base.py:93`, `app/config/settings/base.py:95`. | **4 sítios persistentes encontrados**: duas criações `app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`; correção `app/chamados/services/regularizacao_ext.py:155`; semeadura dinâmica `app/ponto/registro_batida.py:335`. Atribuições de `motor_calculo_v2.py:461` e `turnos.py:1529` são alterações em memória. Exaustividade de valores que chegam por `**campos`/`**d`: [nao verificado]. | **81 ocorrências pelo nome no B**, incluindo objetos intermediários e possíveis homônimos; atribuição integral por modelo [nao verificado]. Amostra: detalhe `:36`, `:85`; drawer `:35`, `:49`, `:51`, `:61`; calendário `:241`, `:244`, `:625`; detector `:57`; hub_smoke `:98`, `:100`. | **sim** | `timestamp=2026-11-07T01:00:00Z`, local `06/11 22h`: filtro `hub_smoke.py:98` inclui no dia 06, mas `:100` registra `(colab,"2026-11-07")`. Se essa batida nasce/some entre as fotos, diferenças locais do dia 06 não são retiradas como tráfego por `:124`–`:132`; o diagnóstico pode acusar `IDEMPOTENTE=False`, `:148`. Tráfego real nessa condição: [nao verificado]. |
| `PerguntaDisputa.data_evento` | Dia civil da saída esperada em `orfao_14h`: `app/escala/servico_jornada.py:483`, `:491`, chamado pela emissão em `app/chamados/services/disputa_emissao.py:1041`; dia local da conversa sem fato informado: `app/chamados/models.py:2424`, `:2427`; consumidor do calendário usa o campo para bloquear o quadrinho da chave, `app/colaboradores/services/calendario.py:477`, `:738`, enquanto turnos são indexados pelo dia da entrada, `:214`, `:224`. | **2 sítios persistentes**: nascimento `app/chamados/models.py:2476`; preenchimento de pergunta sem data `app/chamados/services/fio.py:834`, persistido em `:847`. O `_px.data_evento` em `models.py:2418` é proxy, não escrita de pergunta; `fio.py:843` desfaz mudança em memória quando recusa. | **2 ocorrências B**, ambas abertas: consulta `app/colaboradores/services/calendario.py:473`, chave `:477`. | **sim** | Hoje `06/11/2026`, turno `18/10 22h → saída prevista 19/10 06h`; pergunta `orfao_14h`, resposta `"06:00"` aguardando admin, sem validação, sem materialização, sem tranca e sem luz concordante. Escritor guarda `19/10`; leitor torna `pode_ultimo_recurso=False` em `19/10`, permitindo `True` em `18/10` se não houver outra pergunta (`calendario.py:738`). A guarda de pergunta viva aceita esse estado em `app/chamados/juizes.py:1136`, `:1140`, `:1150`, `:1162`. A oferta final também depende do status em `calendario.py:97`; portanto não afirme botão visível incondicionalmente. |
| `DisputaSupervisao.fechada_em` | Instante de fechamento; `NULL` significa aberta: escritor `app/chamados/models.py:1861`; reabrir zera em `:2063`, `:2087`; leitor `app/colaboradores/services/drawer.py:70`. | **3 sítios** acima; o antigo lote de desligamento foi retirado, e atualmente chama `fechar()`, `app/colaboradores/services/desligamento.py:132`, `:142`. | **6 ocorrências B**, amostra aberta drawer `:70` e desligamento `:139`. Demais localizadores: `painel_cell.py:32`, `signals.py:73`, `services/situacional.py:117`, `escala/signals.py:157`. | **não na amostra** | Nenhum demonstrado pelos leitores abertos. |
| `DisputaSupervisao.aberta_em` | Instante de criação automático, `app/chamados/models.py:1686`; ordena disputas mais recentes, `app/colaboradores/services/drawer.py:71`. | **1 implícito**, declaração. | **1 B**, aberto. | **não na amostra** | Nenhum demonstrado. |
| `AlertaGeofence.obsoleto_em` | Instante em que a acusação caducou; `NULL` ainda vale, `app/ponto/management/commands/reconciliar_alertas_geofence.py:49`, `:52`; leitor usa `isnull=True`, `app/colaboradores/services/detalhe.py:61`, `:65`, `:68`. | **1**, `reconciliar_alertas_geofence.py:52`, valor `timezone.now()` em `:50`. | **3 B**, todos abertos. | **não na amostra** | Nenhum demonstrado. |
| `AlertaGeofence.criado_em` | Instante de criação automático, `app/ponto/models.py:244`; ordenação recente em `app/colaboradores/services/detalhe.py:62`. | **1 implícito**, declaração. | **1 modelo confirmado na amostra**; o total global pelo nome não distingue modelo. | **não na amostra** | Nenhum demonstrado. |
| `ChamadoColaborador.criado_em` | Instante automático `app/chamados/models.py:61`; leitor calcula idade transcorrida em dias completos, `app/colaboradores/services/ficha.py:162`, com um instante capturado em `:117`. | **1 implícito**, declaração; outros valores explícitos possíveis [nao verificado]. | Amostra de **2 ocorrências** no mesmo leitor: ordenação `ficha.py:157` e idade `:162`. Total por entidade [nao verificado]. | **não sei** | Amostra concorda; exaustividade dos escritores não fechada. |
| `Batida.retratada_em` | Instante aware de retratação, `app/ponto/registro_batida.py:274`; `NULL` mantém no universo, `app/colaboradores/services/ficha.py:145`, `app/colaboradores/services/calendario.py:621`; retratadas exibidas por dia civil local, `:239`, `:244`. | Escrita dinâmica `anotar()` em `app/ponto/registro_batida.py:253`, chamada de retratar `:274`; construtores aceitam campos dinâmicos em `:152`, `:157`, `:335`. Exaustividade dos valores e chamadores [nao verificado]. | **8 B**; amostra aberta ficha `:145`, calendário `:239`, `:621`. | **não sei** | Sem desacordo demonstrado; escritores dinâmicos impedem fechar universal. |
| `Justificativa.data_turno` | Data de entrada local explícita em `app/ponto/services/regularizacao.py:73`; triagem usa entrada já local, `app/ponto/services/triagem_batida.py:252`, `:260`; formas HTTP aceitam `date` fornecido, `app/ponto/views.py:1597`, `app/api/views.py:1963`; cron recebe dia do turno, `app/ponto/management/commands/processar_alertas_turno.py:389`. Leitor abona por data, intervalo inclusivo, `app/escala/utils.py:1219`, `:1220`. | **5 sítios** abertos: `ponto/views.py:1635`; `api/views.py:2007`; `services/triagem_batida.py:258`; `services/regularizacao.py:91`; `management/commands/processar_alertas_turno.py:391`. | **3 ocorrências confirmadas de modelo no B**, todas abertas em `utils.py:1219`, `:1220`; outras leituras `t.data_turno` são majoritariamente objetos `Turno`, não esse modelo. | **não na amostra** | Nenhum desacordo temporal demonstrado; payload errado não é prova de interpretação diferente. |
| `Ausencia.data`, `Ausencia.data_fim` | Dias de período inclusivo; escritor deriva fim como `data + dias - 1`, `app/ponto/services/ausencia.py:66`, `:77`; `NULL` depende do tipo: aberto ou dia único, `:70`, `:78`. Escrita de criação em `:342`, `:343`, edição `:499`, `:500`. Autoridade consumida pelo calendário expande inclusivamente e distingue nulo por tipo, `app/ponto/turnos.py:2753`, `:2760`, `:2768`; calendário delega em `app/colaboradores/services/calendario.py:485`. | Criação/edição abertas; há materialização de férias em `app/ponto/services/ausencia.py:1203`, e a exaustividade integral não foi fechada [nao verificado]. | Amostra aberta: calendário `:485`, `:676`; detalhe `app/colaboradores/services/detalhe.py:106`, listagem por início, não teste de cobertura. Total atribuído por modelo [nao verificado]. | **não sei** | Amostra de cobertura concorda; não declare consenso universal sem terminar escritores e disponibilidade. |

Divergentes e famílias:

- `Batida.timestamp`: escritor `app/ponto/registro_batida.py:158` × leitor `app/escala/management/commands/hub_smoke.py:100`; família **batida**, declarada em `app/core/contratos_estruturais.py:46`. O sítio consumidor é uma sonda de idempotência, `hub_smoke.py:147`, não folha.
- `PerguntaDisputa.data_evento`: escritor persistente `app/chamados/models.py:2476`, significado fornecido por `app/chamados/services/disputa_emissao.py:1041` e `app/escala/servico_jornada.py:491` × leitor `app/colaboradores/services/calendario.py:477`, decisão em `:738`; família **chamado**, `app/core/contratos_estruturais.py:51`. O calendário trabalha com geometria por dia de entrada, `calendario.py:214`, `:224`, família **turno/marcos**, `app/docs/JUIZES.md:20`, `:25`.

Cuidados para a contagem final:

- AST dos modelos encontrou **83 campos externos**: chamados 28, ponto 52, folha 3. Contagem executada em memória [nao verificado]; os sítios de declaração estão abaixo.
- **12 campos** receberam abertura semântica nesta subanálise: os dez primeiros individuais da tabela e os dois de Ausência. Destes, 2 divergentes, 6 sem divergência na amostra, 4 não sei; a contagem não prova universalidade dos seis [nao verificado].
- Restam **71 campos externos sem auditoria semântica nesta subanálise**; isso não afirma ausência de uso no B [nao verificado].
- `procedencia_fina` é chamada dinamicamente pela sonda `hub_smoke`, mas não encontrei ligação de produção direta com `PerguntaDisputa.data_evento`; não use essa ligação como par provado [nao verificado].
- Agrupar `detector_proposta` por dia civil é limitação expressamente declarada em `app/escala/detector_proposta.py:18`; não o contei isoladamente como divergência de significado do instante.

Declarações externas restantes, para linhas “não sei” sem perder entidades:

- Chamado: `resolvido_em` `app/chamados/models.py:54`; `atribuido_em` `:60`; `atualizado_em` `:62`; `prazo_sla` `:69`; `escalonado_em` `:77`; `silenciado_ate` `:84`; `resgatado_em` `:95`.
- Followup: `lido_dp_em` `app/chamados/models.py:1602`; `lido_colaborador_em` `:1603`; `criado_em` `:1604`.
- Pergunta: `respondida_em` `app/chamados/models.py:2228`; `validada_em` `:2229`; `data_conferida_em` `:2238`; `julgado_em` `:2351`; `janela_expira_em` `:2357`; `materializada_em` `:2827`.
- RespostaAnterior: `respondida_original_em` `app/chamados/models.py:3181`; `validada_original_em` `:3182`; `versao_reaberta_em` `:3185`.
- Regularização: `RegularizacaoExterna.criada_em` `app/chamados/models.py:3218`; `confirmada_em` `:3219`; `materializada_em` `:3220`; `RegularizacaoItemBatida.data` `:3260`; `aplicada_em` `:3284`.
- Batida: `aprovada_em` `app/ponto/models.py:73`; `timestamp_dispositivo` `:75`; `sincronizado_em` `:77`; `criado_em` `:79`.
- Justificativa: `saida_informada` `app/ponto/models.py:149`; `aprovada_em` `:164`; `criado_em` `:179`; `atualizado_em` `:180`.
- Importação: `criado_em` `app/ponto/models.py:215`; `concluido_em` `:216`.
- DiaPago: `data` `app/ponto/models.py:319`; `lavrado_em` `:380`.
- Fechamento: `previsto_em` `app/ponto/models.py:453`; `derivado_em` `:464`; `aprovado_em` `:475`; `criado_em` `:478`; `atualizado_em` `:479`.
- AlertaTurno: `data_turno` `app/ponto/models.py:527`; `criado_em` `:530`.
- `PushSubscription.criado_em` `app/ponto/models.py:552`; `MotivoRetratacao.criado_em` `:649`.
- Ausência: `prazo_documento` `app/ponto/models.py:782`; `aprovada_em` `:810`; `criado_em` `:813`; `atualizado_em` `:814`.
- Período fechado: `fechado_em` `app/ponto/models.py:926`; `reaberto_em` `:931`.
- Perfil: `criado_em` `app/ponto/models.py:983`; `atualizado_em` `:984`.
- Ping: `timestamp` `app/ponto/models.py:1004`; `criado_em` `:1015`; `FotoBatida.criado_em` `:1031`.
- Substituição: `data_inicio` `app/ponto/models.py:1067`; `data_fim` `:1068`; `designada_em` `:1074`; `cancelada_em` `:1089`; `push_enviado_em` `:1105`.
- `Advertencia.criada_em` `app/ponto/models.py:1179`.
- TurnoMaterializado: `data_turno` `app/ponto/models.py:1205`; `dt_fim_previsto` `:1212`; `atualizado_em` `:1213`.
- DecisaoHE: `data` `app/ponto/models.py:1292`; `decidida_em` `:1306`; `atualizada_em` `:1307`; `revogada_em` `:1325`.
- Folha: `ExportacaoDominio.gerado_em` `app/folha/models.py:80`; `invalidada_em` `:96`; `RetratoFolha.lavrado_em` `:354`.

## 06/11 19:06 · sessao 70a1d30076c1 · - · - · origem=Ronald (tmux)

Escopo concluído: **21 campos próprios de escala**, mais leitura dos carimbos de `EscalaColaborador` delegados ao outro explorer, e **94 parâmetros em 73 funções**; contagem AST feita apenas em memória, excluindo `utils.py`, `servico_jornada.py`, testes e migrações. [nao verificado: esta contagem é resultado da inspeção, sem arquivo de inventário persistido] [nao verificado]

Não confirmei divergência escritor × leitor nestes 21 campos. Os casos sem leitor efetivamente identificado devem permanecer **não sei**, sem converter ausência de leitura em concordância. [nao verificado: conclusão da amostra abaixo]

| Campo | Significado encontrado | Escritores abertos — número de sítios lidos | Leitores abertos — amostra | Resultado |
|---|---|---|---|---|
| `TipoEscala.data_ancora` | Referência de ciclo declarada, `DateField`; template não fornece fase 12x36/24x48. `app/escala/models.py:135`, `app/escala/models.py:824` | 0 atribuições explícitas; cópia dinâmica integral de tipo em `app/escala/services/cadastro_tipo.py:405` e alteração dinâmica em `app/escala/services/cadastro_tipo.py:414`; contagem completa [nao verificado] | 0 leitores efetivos do campo encontrados; a função de trabalho do template retorna `None` em vez de ler a âncora. `app/escala/models.py:824` | **não sei**: campo sem leitura efetiva identificada |
| `AgendaDia.data` | Dia específico da agenda, sem instante. `app/escala/models.py:1429` | 1: recebe `data` diretamente. `app/escala/services/agenda.py:18` | 1: apresentação da data. `app/escala/models.py:1460`; leitores de views ficam com explorer colaboradores | Não na amostra; total global [nao verificado] |
| `AgendaDia.criado_em` | Instante de criação automático. `app/escala/models.py:1441` | 1 operação de criação aberta: `app/escala/services/agenda.py:17` | 0 leitores próprios escala; views ficam com explorer colaboradores | **não sei** nesta subamostra |
| `AgendaDia.atualizado_em` | Instante da alteração, automático ou `timezone.now()`. `app/escala/models.py:1442`, `app/escala/services/agenda.py:34` | 3 operações abertas: criação `app/escala/services/agenda.py:17`; proposição `app/escala/services/agenda.py:34`; resposta `app/escala/services/agenda.py:45` | 0 leituras efetivas próprias escala | **não sei** |
| `FolgaCalendario.criado_em` | Instante automático de criação. `app/escala/models.py:1547` | 3 nascimentos abertos: construção seguida de save `app/escala/folgas.py:557`, `app/escala/folgas.py:560`; dois creates `app/escala/folgas.py:587`, `app/escala/folgas.py:589` | 0 leituras efetivas identificadas | **não sei** |
| `FolgaCalendario.atualizado_em` | Instante automático de save. `app/escala/models.py:1548` | 5 operações abertas: saves `app/escala/folgas.py:560`, `app/escala/folgas.py:564`, `app/escala/folgas.py:569`; creates `app/escala/folgas.py:587`, `app/escala/folgas.py:589` | 0 leituras efetivas identificadas | **não sei** |
| `FolgaDia.data` | Dia de folga, também pertencente ao mês calendário da foto; não é instante. `app/escala/models.py:1588`, `app/escala/folgas.py:210` | **3**: calendário `app/escala/folgas.py:185`; manual get/create `app/escala/folgas.py:313`; manual create `app/escala/folgas.py:337` | Amostra 4: mês completo `app/escala/alimentacao.py:91`, `app/escala/alimentacao.py:100`; teste de foto no mês `app/escala/models.py:1250`; leitura de fase `app/escala/services/fase_conflitante.py:74` | Não na amostra; total por modelo [nao verificado] |
| `FolgaDia.gerado_em` | Instante automático de criação da foto. `app/escala/models.py:1598` | As mesmas 3 criações acima | 0 leituras efetivas identificadas | **não sei** |
| `CelulaDia.data` | Dia da célula/escala, indexado por `date`, com vigência inclusiva. `app/escala/alimentacao.py:214`, `app/ponto/portas/celula.py:827` | **3**: regeneração cria `app/ponto/portas/celula.py:259`; edição cria `app/ponto/portas/celula.py:600`; geração cria `app/ponto/portas/celula.py:850` | Amostra 5: carga inclusiva `app/escala/alimentacao.py:77`; grade inclusiva `app/escala/services/leitor_celula.py:350`; período inclusivo `app/escala/services/regeneracao.py:103`; censos de fase `app/escala/services/fase_12x36.py:188`; propositor ontem incluído via `< hoje` `app/escala/services/propositor.py:473` | Não na amostra. Há recorte inadequado de competência em `_jornada_que_mente`, tratado abaixo como limitação de consulta |
| `CelulaDia.gerada_em` | Instante do nascimento, não da reescrita. `app/escala/models.py:1702`, `app/ponto/portas/celula.py:264` | Automático nas 3 criações acima | 1 leitura semântica: comparado como instante de fato novo. `app/escala/services/fase_12x36.py:190`, `app/escala/services/fase_12x36.py:192` | Não na amostra |
| `CelulaDia.regenerada_em` | Instante da reescrita por regeneração, exclui edição humana. `app/escala/models.py:1718` | **2** atribuições: `app/ponto/portas/celula.py:274`, `app/ponto/portas/celula.py:792` | Amostra 3: máximo do par de reescrita `app/escala/services/regeneracao.py:84`; posterior estrito à cobrança `app/escala/services/regeneracao.py:120`; posterior estrito à lavra `app/escala/services/fase_12x36.py:192` | Não na amostra; ressalva de omissão de carimbo abaixo |
| `CelulaDia.editada_em` | Instante da edição humana do dia. `app/escala/models.py:1732`, `app/ponto/portas/celula.py:598` | **2**: criação editada `app/ponto/portas/celula.py:601`; edição existente `app/ponto/portas/celula.py:607` | Mesma amostra de 3 comparações de instante acima: `app/escala/services/regeneracao.py:84`, `app/escala/services/regeneracao.py:120`, `app/escala/services/fase_12x36.py:192` | Não na amostra |
| `CelulaDia.julgada_em` | Instante em que o cartório viu/imprimiu os insumos do dia; não anda em repetição idêntica. `app/ponto/portas/celula.py:1054`, `app/ponto/portas/celula.py:1060` | **1**: `app/ponto/portas/celula.py:1060` | 1: marco de fato novo, com `>` estrito. `app/escala/services/fase_12x36.py:189`, `app/escala/services/fase_12x36.py:192` | Não na amostra |
| `CelulaDia.veredito_em` | Instante de lavratura do veredito. `app/ponto/portas/celula.py:1032` | **1**: `app/ponto/portas/celula.py:1032` | 0 próprios escala; calendário fica com explorer colaboradores | **não sei** nesta subamostra |
| `CelulaDia.insumos_em` | Instante da observação dos insumos, distingue nunca visto de visto sem ausência. `app/ponto/portas/celula.py:1052` | **2**: cartório `app/ponto/portas/celula.py:1064`; backfill em lote `app/ponto/portas/celula.py:1100` | 2 guardas de presença/idempotência, sem conversão de unidade: `app/ponto/portas/celula.py:1056`, `app/ponto/portas/celula.py:1095` | Não na amostra |
| `EventoCelula.data` | Cópia do dia da célula. `app/ponto/portas/celula.py:891` | **1**: `app/ponto/portas/celula.py:891` | 0 leitores próprios escala identificados | **não sei** |
| `EventoCelula.criado_em` | Instante de nascimento automático. `app/escala/models.py:1860` | **1** criação aberta: `app/ponto/portas/celula.py:891` | Uso em índice de pendências, sem interpretação de dia. `app/escala/models.py:1867` | **não sei** sobre todos leitores |
| `EventoCelula.processado_em` | Instante do processamento concluído; nulo significa pendente. `app/chamados/reconciliador.py:1378`, `app/chamados/reconciliador.py:1386` | **1**: `app/chamados/reconciliador.py:1386` | Amostra 3 condições de índices: `app/escala/models.py:1867`, `app/escala/models.py:1869`, `app/escala/models.py:1871`; consumidor também testa nulo `app/chamados/reconciliador.py:1378` | Não na amostra |
| `FuroDiario.data` | Dia da grade/célula, intervalo inclusivo limitado a ontem na apuração. `app/escala/services/furos_diarios.py:27`, `app/escala/services/furos_diarios.py:52` | **1**: `app/ponto/portas/celula.py:1125` | Amostra 2: ranking inclusivo `app/escala/services/furos_diarios.py:133`; ranking de colaboradores `app/escala/services/furos_diarios.py:178` | Não na amostra |
| `FuroDiario.avaliada_em` | Instante da apuração; pode andar ao reapurar os mesmos números. `app/ponto/portas/celula.py:31`, `app/ponto/portas/celula.py:1127` | **1**: `app/ponto/portas/celula.py:1127` | 0 leitores identificados no grupo | **não sei** |
| `DecisaoProposta.decidida_em` | Instante da decisão; silêncio por duração de 30 dias, borda inferior inclusiva. `app/escala/models.py:1926`, `app/escala/services/propositor.py:700` | Automático em **2** criações abertas: recusa `app/escala/services/propositor.py:717`; aceite `app/escala/services/propositor.py:773` | 1 filtro: `decidida_em__gte=corte`. `app/escala/services/propositor.py:702` | Não na amostra |

Os outros **5 campos** são `EscalaColaborador.data_inicio`, `data_fim`, `regua_avaliada_em`, `regua_corroborada_em`, `data_ancora_colaborador`: declarações em `app/escala/models.py:875`, `app/escala/models.py:876`, `app/escala/models.py:884`, `app/escala/models.py:888`, `app/escala/models.py:974`. Seus escritores persistentes ficam com o explorer de colaboradores; os seguintes trechos foram abertos aqui:

- `regua_avaliada_em`: o valor persistente vem de `_tz.now()` em `app/escala/regua_defesa.py:660`, passado à porta em `app/escala/regua_defesa.py:666`, ou nova avaliação em `app/escala/regua_defesa.py:767`. Leitores testam presença em `app/escala/regua_defesa.py:560` e `app/escala/regua_defesa.py:626`.
- `regua_corroborada_em`: instante de confirmação ou `None` após desfazer/expirar; escrita solicitada em `app/escala/regua_defesa.py:667`, `app/escala/regua_defesa.py:722`, `app/escala/regua_defesa.py:743`; leitura usa `localtime` para apresentar o instante em `app/escala/regua_defesa.py:658`.
- `data_ancora_colaborador`: dia de trabalho do ciclo; a fase calcula `(data-anc).days % 2/3` em `app/escala/services/fase_conflitante.py:45` e `app/escala/services/fase_conflitante.py:47`. Aceitação de âncora futura para vínculo futuro está explicitamente contemplada em `app/escala/models.py:951`.
- `data_inicio/data_fim`: vigência inclusiva `<= data` e `>= data` em `app/escala/alimentacao.py:214`; `None` não basta sem vigência declarada, `app/escala/alimentacao.py:196`.

Os **94 parâmetros** se repartem assim:

| Conjunto | Quantidade | Semântica / referências |
|---|---:|---|
| Parâmetros `dia` que são **dict da grade**, não datas | **4** | `orfa_classe._tem_missing` `app/escala/orfa_classe.py:35`; `classificar_orfas` `app/escala/orfa_classe.py:53`; `_overlay_isencao` `app/escala/services/leitor_celula.py:188`; `_furos_do_dia` `app/escala/services/furos_diarios.py:12` |
| `dia_iso` | **1** | String ISO de dia, comparada inclusivamente com `ini/fim` ISO: `app/escala/services/fase_12x36.py:113` |
| Datas e limites de período, incluindo `ini/fim/inicio` que o inventário fornecido também lista | **89** | A amostra cobre todos os corpos abaixo; os intervalos consultados são inclusivos, ou convertidos a instantes aware de início/fim do dia. Exemplos `app/escala/alimentacao.py:77`, `app/escala/services/fase_na_tela.py:45`, `app/escala/services/fase_na_tela.py:47`, `app/escala/services/semana_do_posto.py:56` |

Enumeração dos 73 corpos, com quantidades de parâmetros entre parênteses quando maiores que um:

`detector_proposta.py:49 medir_padrao(2)`, `:118 propor_escala(2)`; `folgas.py:280 lancar_folga`, `:320 _foto_do_dia`, `:346 alterar_folga`, `:419 vinculo_que_cobre`; `regua_defesa.py:16 regua_degenerada`, `:36 vetar_cobranca_se_degenerada`, `:459 cumpriu_a_volta_do_intervalo`; `models.py:224 _override_do_dia`, `:281 pausas_do_dia`, `:310 extra_declarada_do_dia`, `:326 marcos_do_dia`, `:816 eh_dia_trabalho`, `:938 recusa_ancora_futura`, `:1048 _celulas_do_ato_para`, `:1069 eh_dia_trabalho`, `:1098 marcos_do_dia`, `:1135 intervalo_do_dia`, `:1166 pausas_do_dia`, `:1210 extra_declarada_do_dia`, `:1241 defeito_folga_nao_definida`, `:1252 eh_dia_trabalho_calculado`, `:1373 feriado_vira_folga`; `orfa_classe.py:35 _tem_missing`, `:53 classificar_orfas`; `alimentacao.py:24 celulas_do_ato(2)`, `:54 celulas_do_periodo(2)`, `:81 folgas_do_periodo(2)`, `:105 escalas_do_periodo_por_colab(2)`, `:163 escalas_do_periodo(2)`, `:205 vinculo_cobre_a_data`, `:219 q_vinculo_cobre_a_data`, `:228 vinculo_do_dia`, `:289 vinculo_do_dia_carregado`, `:320 vinculos_do_dia_carregado`, `:361 previstas_do_periodo(2)`; `proposta_pdf.py:27 vinculos_em_limbo`, `:43 coletar(2)`, `:53 gerar_pdf_propostas_bytes(2)`; `services/regeneracao.py:87 carimbos_de_reescrita(2)`, `:108 dia_reescrito_apos`; `services/leitor_celula.py:188 _overlay_isencao`, `:345 grade_da_celula(2)`; `services/escala_certa.py:19 motivo_escala_errada`, `:49 escala_certa_no_dia`; `services/fase_conflitante.py:32 _ancora_diz`, `:51 _foto_diz`; `services/impacto_troca.py:16 _trabalhados(2)`, `:22 impacto_da_troca`; `services/previsto_cego.py:78 _causa_do_colab(2)`; `services/propositor.py:136 _horario_mediano`; `services/cadastro_realidade.py:405 dias_folga_nao_definida_sem_porta(2)`; `services/fase_na_tela.py:23 teto_da_ancora`, `:28 ancora_da_marcacao(2)`, `:40 _trabalhados(2)`, `:125 inicio_do_vinculo`, `:216 calendario_do_ciclo`; `services/fase_12x36.py:113 _cobre`, `:117 dias_de_fase_as_of(2)`; `services/escala_auto_executor.py:52 _ate`; `services/semana_do_posto.py:51 limites_da_semana`, `:126 grade_do_posto`, `:168 _fato_do_dia`, `:182 propostas_cruzadas`, `:219 contador_por_posto`; `services/agenda.py:11 adicionar_bloco`; `services/furos_diarios.py:12 _furos_do_dia`, `:25 apurar_colab(2)`, `:123 ranking(2)`, `:167 ranking_colabs(2)`; `management/commands/hub_smoke.py:47 chama`; `management/commands/lavrar_foto_folgas.py:85 _meio_dia`. Todos os caminhos nesta enumeração têm prefixo `app/escala/`.

Limitações concretas para o quadro, sem promovê-las a divergências de campo sem par comprovado:

- `_jornada_que_mente` monta período com **mês civil de hoje e corte fixo 21**, em `app/escala/services/cadastro_realidade.py:246` e `app/escala/services/cadastro_realidade.py:247`; aplica isso a todos os colaboradores selecionados em `app/escala/services/cadastro_realidade.py:255`. Em 25/11/2026, consulta 21/10–20/11, enquanto `janela_atual` com corte 21 responde 21/11–20/12. O erro é do recorte da consulta; classificar `CelulaDia.data` ou um parâmetro divergente exige vínculo adicional que não demonstrei. Família de janela: `folha/export`, conforme `app/docs/JUIZES.md:35`.
- `detector_proposta` agrupa por **dia civil local** em `app/escala/detector_proposta.py:58`, e calcula entrada/saída pelas extremas desse dia em `app/escala/detector_proposta.py:67` e `app/escala/detector_proposta.py:72`. A limitação para turnos noturnos está declarada no próprio módulo, conforme a leitura do root. Um par 05/11 19h → 06/11 07h resulta em dois dias com um marco e sem duração; isso não demonstra que o escritor de `timestamp` discorde sobre o significado de instante.
- Há alteração de DNA sem `regenerada_em` no ramo de fora da vigência, `app/ponto/portas/celula.py:243` e `app/ponto/portas/celula.py:245`; outro ramo escreve o carimbo em `app/ponto/portas/celula.py:274`. O leitor de fase só vê carimbos em `app/escala/services/fase_12x36.py:189`. Falta verificar se o rejulgamento posterior sempre move `julgada_em`; efeito errado permanece [nao verificado].
- `hub_smoke.win_map` usa `ts.date()` cru em `app/escala/management/commands/hub_smoke.py:100`, após filtrar `timestamp__date` em `app/escala/management/commands/hub_smoke.py:98`; o mapa só compara a mesma fotografia antes/depois, conforme `app/escala/management/commands/hub_smoke.py:112`, portanto não confirmei saída operacional errada.
- O inventário não captura os nomes dentro de `Max(...)` de `app/escala/services/fase_12x36.py:189`, nem os pedidos persistentes via kwargs de `gravar_regua` em `app/escala/regua_defesa.py:666` e `app/escala/regua_defesa.py:767`; contagens globais por modelo não podem ser inferidas dos RESUMO homônimos.

## 06/11 19:06 · sessao 47a279e309b9 · - · - · origem=Ronald (tmux)

Auditoria de 9 campos; contagens abaixo são sítios reais de atribuição/construção, sem repetir `update_fields`. O inventário por nome não fornece total de leitores por modelo; esses totais permanecem [nao verificado].

| Campo | Significado e escritores abertos | Amostra de leitores aberta | Divergente? / caso concreto deduzido do código |
|---|---|---|---|
| `Ausencia.data` | Início do período, `date`; 2 sítios: `app/ponto/services/ausencia.py:342`, `:499`. | `app/ponto/turnos.py:2553`, `:2766`; `app/ponto/services/dia_decidido.py:551`; `app/ponto/views.py:1004`. | **Sim, junto com `data_fim`**: leitores de início dentro do recorte omitem períodos que começaram antes dele (`app/ponto/services/dia_decidido.py:552`; `app/ponto/views.py:1004`). |
| `Ausencia.data_fim` | Fim **inclusivo**; `None` significa aberto para `TIPOS_EM_ABERTO`, dia único para outros; derivação em `app/ponto/services/ausencia.py:66`; 3 escritores: `:343`, `:500`, `app/ponto/models.py:907`. | 9 funções: `Ausencia.save` (`app/ponto/models.py:882`); `ausencias_do_periodo` (`app/ponto/turnos.py:2529`); `ausencia_cobre` (`:2553`); `cobertura_ausencia_periodo` (`:2760`); `dias_da_ausencia` (`:2779`); `fatos_do_periodo` (`app/ponto/precedencia.py:338`); `houve_fato_novo` (`app/ponto/services/cartorio.py:355`); `decididos_do_periodo` (`app/ponto/services/dia_decidido.py:558`); painel (`app/ponto/views.py:1004`). | **Sim**. Tipo configurado em aberto, início `2026-11-01`, fim `None`, consulta `05..06/11`: cobertura inclui 05 e 06 (`app/ponto/turnos.py:2762`), `decididos_do_periodo` devolve `{}` (`app/ponto/services/dia_decidido.py:552`). Mesmo com lista alimentada, `fim = a.data_fim or a.data` reduz ao dia 01 (`:558`). |
| `Ausencia.prazo_documento` | Último dia local permitido; 4 escritores: criação `app/ponto/services/ausencia.py:339`/`:345`; rejulgamento `:551`; limpeza `:826`; reabertura `:1146`. | 2 funções: contadores usam `< hoje` (`app/ponto/services/ausencia.py:961`/`:969`), escalonamento usa `hoje > prazo` (`:991`/`:999`). | **Não na amostra**: prazo `06/11` vence em `07/11`, coerentemente nesses leitores (`app/ponto/services/ausencia.py:969`, `:999`). Todos os leitores: [nao verificado]. |
| `Justificativa.data_turno` | Data julgada do turno no cron; data civil local da entrada na triagem/regularização. 4 escritores: `app/ponto/management/commands/processar_alertas_turno.py:393`; `app/ponto/services/regularizacao.py:94`; `app/ponto/services/triagem_batida.py:260`; `app/ponto/views.py:1638`. | `app/ponto/services/efeito_ausencia.py:40`; `app/ponto/services/cartorio.py:359`; `app/ponto/services/justificativa.py:53`, `:149`, `:200`. | **Sim**. Jornada `05/11 22h → 06/11 06h`, entrada `06/11 00:10`, sem saída: juiz atribui 05/11 (`app/ponto/turnos.py:297`–`:306`, limite 90 min em `:53`). Cron escreve 05 (`app/ponto/management/commands/processar_alertas_turno.py:321` → `:393`); triagem descarta `t.data_turno`, extrai entrada local (`app/ponto/services/triagem_batida.py:178`–`:185`) e escreve 06 (`:260`). Após aprovação, `efeitos_da_ausencia` justifica 06 (`app/ponto/services/efeito_ausencia.py:43`) e cascata procura chamado 06 (`app/ponto/services/justificativa.py:53`). |
| `Justificativa.saida_informada` | Instante completo de saída; 6 escritores: `app/ponto/services/regularizacao.py:97`, `:103`; `app/ponto/services/triagem_batida.py:264`; `app/ponto/views.py:1628`, `:1640`; `app/ponto/services/justificativa.py:367`. | Aprovação compara instantes e conserva a data completa (`app/ponto/services/justificativa.py:137`–`:174`). | **Não na amostra**: regularização aceita aware ou torna naive local (`app/ponto/services/regularizacao.py:81`); triagem compõe data/hora local e avança dia no overnight (`app/ponto/services/triagem_batida.py:247`–`:256`); leitor mantém instante (`app/ponto/services/justificativa.py:165`). Universal: [nao verificado]. |
| `OrdemSubstituicao.data_inicio` | Instante inicial; ISO com offset conservado, naive tornado local (`app/ponto/services/cobertura.py:57`–`:65`). 2 escritores: `:105`, `:291`. | `esta_ativa_em` e `ativa_para` (`app/ponto/models.py:1147`, `:1155`); disponibilidade (`app/ponto/utils.py:205`); detector (`app/chamados/detector_anomalias.py:76`). | **Sim, bordas em conjunto com fim**. Disponibilidade usa interseção aberta nas pontas, atividade inclui ambas (`app/ponto/utils.py:205`–`:206`; `app/ponto/models.py:1147`). |
| `OrdemSubstituicao.data_fim` | Instante final previsto; 2 escritores: `app/ponto/services/cobertura.py:106`, `:291`. | `app/ponto/models.py:1147`, `:1156`; `app/ponto/utils.py:206`; `app/chamados/detector_anomalias.py:83`. | **Sim**. Ordem A `06/11 06:00–18:00 -03`, pedido B começando `18:00 -03`: disponibilidade aceita B porque `A.fim > B.início` é falso (`app/ponto/utils.py:206`), mas às 18h A e B estão ativas pelo teste fechado (`app/ponto/models.py:1147`). |
| `DecisaoHE.data` | Dia cuja HE foi decidida (`app/ponto/models.py:1292`). 1 escritor: `app/ponto/portas/he.py:67`. | 3 funções: mapa do motor (`app/ponto/motor_calculo_v2.py:1417`), `estado_por_dia` (`app/ponto/services/gestao_he.py:88`), `_decisoes` (`:106`). | **Não na amostra**: chaves são dias e intervalos incluem início/fim (`app/ponto/services/gestao_he.py:88`). Universal: [nao verificado]. |
| `DiaPago.data` | Dia da jornada; `None` na linha de ajuste. 4 escritores: `app/ponto/services/dia_pago.py:343`, `:355`, `:471`, `:508`. Mapa da jornada prevalece, fallback usa data local (`:128`–`:132`). | Retrato por dia (`app/ponto/services/dia_pago.py:464`), soma do período (`:782`–`:783`). | **Não na amostra**: soma inclui só linhas de dia e `[ini,fim]`; ajuste sem dia é explicitamente excluído (`app/ponto/services/dia_pago.py:754`–`:757`). Fechamento fornece mapa em ambos os caminhos (`app/ponto/services/fechamento.py:1201`, `:1271`). Universal: [nao verificado]. |

Divergentes e famílias:

- **Ausência aberta → dia único / início dentro do recorte:** escritor `app/ponto/services/ausencia.py:70`/`:343` × leitor `app/ponto/services/dia_decidido.py:552`/`:558`. Família **ausencia/ferias**, nominalmente declarada em `app/core/contratos_estruturais.py:134`. Outro leitor incompatível: `houve_fato_novo`, que só inclui `NULL` no próprio dia inicial (`app/ponto/services/cartorio.py:355`–`:357`); família do sítio cartorial **celula/precedencia** (`app/core/contratos_estruturais.py:165`).
- **Justificativa, dia civil da entrada → dia da jornada:** escritor `app/ponto/services/triagem_batida.py:260` × leitor `app/ponto/services/efeito_ausencia.py:43`; escritor do cron usa outra semântica (`app/ponto/management/commands/processar_alertas_turno.py:321`, `:393`). Geometria pertence a **turno/marcos** (`app/docs/JUIZES.md:20`); associação nominal de `Justificativa` a família na matriz: [nao verificado].
- **Ordem, fim exclusivo na disponibilidade → inclusivo na atividade:** escritor `app/ponto/services/cobertura.py:105`–`:106`, precedido de disponibilidade em `:83`, × leitor `app/ponto/models.py:1147`/`:1156`; condição divergente em `app/ponto/utils.py:205`–`:206`. Família **escala**, nominalmente inclui Ordem na cobertura (`app/core/contratos_estruturais.py:149`).

Parâmetros lidos relacionados:

- `derivar_data_fim.data`, `criar_ausencia.data`, `editar_ausencia_registro.data`: dia inicial; duração transforma fim em inclusivo (`app/ponto/services/ausencia.py:66`, `:77`, `:197`, `:477`).
- `dias_da_ausencia.data/data_fim/ini/fim`: recorte inclusivo e abertura dependente de tipo (`app/ponto/turnos.py:2774`–`:2788`).
- `decididos_do_periodo.data_ini/data_fim`: recorte inclusivo, divergência aberta acima (`app/ponto/services/dia_decidido.py:529`, `:552`, `:558`).
- `efeitos_da_ausencia.data_ini/data_fim`: dias de jornada inclusivos, recebe a divergência de `Justificativa.data_turno` (`app/ponto/services/efeito_ausencia.py:28`, `:43`).
- `apagar_ausencias_de_ferias.data_inicio/data_fim` e `rejeitar_ausencias_de_ferias.data_inicio/data_fim`: seleção inclusiva pelo início (`app/ponto/services/ausencia.py:1262`, `:1267`, `:1273`, `:1282`).
- `_gravar.data`, `decidir_he.data`, `recusar_ponta_pequena.data`: dia da decisão de HE (`app/ponto/portas/he.py:44`, `:67`, `:138`, `:174`, `:269`, `:318`).
- `lavrar.dia_da_jornada`: mapa período→dia, consumido em `_dia_de` (`app/ponto/services/dia_pago.py:140`, `:174`).
- `declarar_cobertura_passada.dia`: dia previsto; marcos produzem instantes inicial/final (`app/ponto/services/cobertura.py:246`, `:276`–`:291`).

Advertências úteis para consolidar:

- `saida_informada` em `defaults=dict(...)` da triagem não consta como `ESC` no anexo, mas escreve o campo (`app/ponto/services/triagem_batida.py:262`–`:264`).
- Criação de `Ausencia` passa os temporais por `kwargs`; o anexo não a identifica como `ESC data/data_fim/prazo_documento`, embora esses valores entrem na construção (`app/ponto/services/ausencia.py:341`–`:345`).
- O caso da regularização com entrada deslocada à véspera pode **recusar antes de escrever**: consulta turnos a partir da data civil (`app/ponto/services/regularizacao.py:60`–`:65`), enquanto a autoridade filtra pelo dia julgado (`app/ponto/turnos.py:1993`). Use a **triagem** como escritor comprovado do exemplo, sem afirmar gravação pela regularização.
- O código congelado declara `afastamento_inss` indeterminado (`app/ponto/catalogo/ausencias.py:93`), mas o conjunto em aberto é vivo (`:430`–`:435`). Estado desse cadastro em produção: [nao verificado].

## 06/11 19:06 · sessao 71c27c2c5a93 · BUG-PLANTIO-NOTURNO-MAIS-1 · sonda · origem=Code

Não houve nova execução; `sonda_frota3.py` não foi localizado na cópia [nao verificado]. O executor também cria e reescreve arquivos, contrariando a regra de somente leitura (`bin/sonda_leitura.sh:126`, `bin/sonda_leitura.sh:134`, `bin/sonda_leitura.sh:216`, `bin/sonda_leitura.sh:250`). Segue a transcrição integral do dado fornecido; execução e resultados no banco [nao verificado].

```text
=== SOMBRA (sombra_db, leitor) 06/11 19:03:09 ===
sonda_leitura: sonda_frota3.py [0114f8fe0d06f5fd] no cpuset 4-7, lendo sombra como leitor (LEITURA)

UNIVERSO perguntas 9729 batidas plantadas vivas 668
N orfao_14h|E|ALEM>24h|mes_batida=09|batida-data_evento=+0 3
N orfao_14h|E|ALEM>24h|mes_batida=10|batida-data_evento=+0 2
N orfao_14h|E|longo16-24h|mes_batida=09|batida-data_evento=+0 1
N orfao_14h|E|longo16-24h|mes_batida=10|batida-data_evento=+0 1
N orfao_14h|E|ok<=16h|mes_batida=09|batida-data_evento=+0 2
N orfao_14h|S|ALEM>24h|mes_batida=09|batida-data_evento=+0 16
N orfao_14h|S|ALEM>24h|mes_batida=09|batida-data_evento=+1 10
N orfao_14h|S|ALEM>24h|mes_batida=10|batida-data_evento=+0 42
N orfao_14h|S|ALEM>24h|mes_batida=10|batida-data_evento=+1 29
N orfao_14h|S|ALEM>24h|mes_batida=11|batida-data_evento=+0 2
N orfao_14h|S|longo16-24h|mes_batida=09|batida-data_evento=+0 2
N orfao_14h|S|longo16-24h|mes_batida=09|batida-data_evento=+1 1
N orfao_14h|S|longo16-24h|mes_batida=10|batida-data_evento=+0 1
N orfao_14h|S|longo16-24h|mes_batida=10|batida-data_evento=+1 1
N orfao_14h|S|ok<=16h|mes_batida=09|batida-data_evento=+0 38
N orfao_14h|S|ok<=16h|mes_batida=09|batida-data_evento=+1 10
N orfao_14h|S|ok<=16h|mes_batida=10|batida-data_evento=+0 77
N orfao_14h|S|ok<=16h|mes_batida=10|batida-data_evento=+1 39
N orfao_14h|S|ok<=16h|mes_batida=11|batida-data_evento=+0 4
N orfao_14h|S|sem_vizinha|mes_batida=09|batida-data_evento=+0 11
N orfao_14h|S|sem_vizinha|mes_batida=09|batida-data_evento=+1 10
N orfao_14h|S|sem_vizinha|mes_batida=10|batida-data_evento=+0 42
N orfao_14h|S|sem_vizinha|mes_batida=10|batida-data_evento=+1 20
N orfao_14h|S|sem_vizinha|mes_batida=11|batida-data_evento=+0 3
N saida_sem_entrada|E|ALEM>24h|mes_batida=09|batida-data_evento=+0 39
N saida_sem_entrada|E|ALEM>24h|mes_batida=10|batida-data_evento=+0 65
N saida_sem_entrada|E|ALEM>24h|mes_batida=11|batida-data_evento=+0 6
N saida_sem_entrada|E|longo16-24h|mes_batida=10|batida-data_evento=+0 1
N saida_sem_entrada|E|ok<=16h|mes_batida=09|batida-data_evento=+0 37
N saida_sem_entrada|E|ok<=16h|mes_batida=10|batida-data_evento=+0 51
N saida_sem_entrada|E|ok<=16h|mes_batida=10|batida-data_evento=+1 1
N saida_sem_entrada|E|ok<=16h|mes_batida=11|batida-data_evento=+0 6
N saida_sem_entrada|E|sem_vizinha|mes_batida=09|batida-data_evento=+0 27
N saida_sem_entrada|E|sem_vizinha|mes_batida=10|batida-data_evento=+0 59
N saida_sem_entrada|E|sem_vizinha|mes_batida=10|batida-data_evento=+1 1
N saida_sem_entrada|E|sem_vizinha|mes_batida=11|batida-data_evento=+0 4
N saida_sem_entrada|S|ALEM>24h|mes_batida=10|batida-data_evento=+0 1
N saida_sem_entrada|S|ok<=16h|mes_batida=10|batida-data_evento=+0 1
N saida_sem_entrada|S|sem_vizinha|mes_batida=10|batida-data_evento=+0 2
EX col32 p52436 saida_sem_entrada D=2026-11-01 plantada=01/11 07:00 gap=53.4h
EX col32 p52690 orfao_14h D=2026-11-01 plantada=01/11 19:00 gap=53.6h
EX col37 p40288 orfao_14h D=2026-10-05 plantada=06/10 07:01 gap=52.0h
EX col39 p39602 saida_sem_entrada D=2026-10-03 plantada=03/10 07:00 gap=53.0h
EX col39 p39605 orfao_14h D=2026-10-03 plantada=03/10 19:00 gap=54.0h
EX col49 p39734 saida_sem_entrada D=2026-10-04 plantada=04/10 07:00 gap=59.9h
EX col50 p35205 saida_sem_entrada D=2026-09-22 plantada=22/09 07:00 gap=55.1h
EX col55 p34709 saida_sem_entrada D=2026-09-21 plantada=21/09 07:00 gap=55.0h
EX col55 p34712 orfao_14h D=2026-09-21 plantada=21/09 19:00 gap=61.0h
EX col55 p38916 saida_sem_entrada D=2026-10-01 plantada=01/10 07:00 gap=60.0h
EX col67 p46661 saida_sem_entrada D=2026-10-21 plantada=21/10 19:00 gap=52.9h
EX col67 p46664 orfao_14h D=2026-10-21 plantada=22/10 07:00 gap=54.8h
EX col98 p39998 saida_sem_entrada D=2026-10-04 plantada=04/10 11:00 gap=50.2h
EX col98 p40001 orfao_14h D=2026-10-04 plantada=04/10 19:00 gap=54.6h
EX col137 p38531 orfao_14h D=2026-09-30 plantada=30/09 19:00 gap=54.6h
EX col137 p39193 saida_sem_entrada D=2026-10-02 plantada=02/10 07:00 gap=52.4h
EX col169 p36384 orfao_14h D=2026-09-25 plantada=26/09 06:02 gap=60.1h
EX col169 p53244 saida_sem_entrada D=2026-11-02 plantada=02/11 18:00 gap=60.0h
EX col171 p42632 saida_sem_entrada D=2026-10-11 plantada=11/10 06:55 gap=52.7h
EX col171 p42635 orfao_14h D=2026-10-11 plantada=11/10 19:00 gap=54.4h
EX col207 p49238 saida_sem_entrada D=2026-09-22 plantada=22/09 06:30 gap=30.1h
EX col207 p49240 saida_sem_entrada D=2026-09-30 plantada=30/09 06:30 gap=54.1h
EX col207 p49241 saida_sem_entrada D=2026-10-01 plantada=01/10 06:30 gap=30.1h
EX col207 p49242 saida_sem_entrada D=2026-10-06 plantada=06/10 06:30 gap=30.1h
EX col238 p40049 orfao_14h D=2026-10-04 plantada=04/10 19:00 gap=52.0h
EX col243 p37396 saida_sem_entrada D=2026-09-27 plantada=27/09 07:00 gap=52.3h
EX col243 p37781 saida_sem_entrada D=2026-09-28 plantada=28/09 07:00 gap=28.3h
EX col255 p34509 saida_sem_entrada D=2026-09-21 plantada=21/09 17:53 gap=54.6h
EX col255 p34510 orfao_14h D=2026-09-21 plantada=22/09 06:00 gap=59.6h
EX col263 p42177 saida_sem_entrada D=2026-10-10 plantada=10/10 08:50 gap=55.2h
EX col290 p38462 saida_sem_entrada D=2026-09-30 plantada=30/09 19:07 gap=59.6h
EX col297 p49153 saida_sem_entrada D=2026-09-23 plantada=23/09 23:00 gap=27.0h
EX col297 p49157 saida_sem_entrada D=2026-09-27 plantada=27/09 23:01 gap=27.1h
EX col297 p49160 orfao_14h D=2026-09-27 plantada=28/09 07:00 gap=32.9h
EX col297 p49161 saida_sem_entrada D=2026-09-30 plantada=30/09 23:09 gap=27.4h
EX col297 p49164 orfao_14h D=2026-09-30 plantada=01/10 07:00 gap=28.0h
EX col302 p45925 saida_sem_entrada D=2026-10-19 plantada=19/10 18:00 gap=54.2h
EX col302 p45928 orfao_14h D=2026-10-19 plantada=20/10 06:00 gap=52.8h
EX col354 p37874 saida_sem_entrada D=2026-09-28 plantada=28/09 07:00 gap=28.2h
EX col354 p37877 orfao_14h D=2026-09-28 plantada=28/09 16:00 gap=56.9h
ERROS {}
rc_sombra=0
```

**(a)** Consolidação das linhas `N`, somando E e S. Todas as células derivam exclusivamente do raspado; confirmação no banco [nao verificado].

| Motivo | Mês da batida | ok<=16h | longo16-24h | ALEM>24h | sem_vizinha | Total |
|---|---|---:|---:|---:|---:|---:|
| orfao_14h | 09 | 50 | 4 | 29 | 21 | 104 |
| orfao_14h | 10 | 116 | 3 | 73 | 62 | 254 |
| orfao_14h | 11 | 4 | 0 | 2 | 3 | 9 |
| **orfao_14h** | **Todos** | **170** | **7** | **104** | **86** | **367** |
| saida_sem_entrada | 09 | 37 | 0 | 39 | 27 | 103 |
| saida_sem_entrada | 10 | 53 | 1 | 66 | 62 | 182 |
| saida_sem_entrada | 11 | 6 | 0 | 6 | 4 | 16 |
| **saida_sem_entrada** | **Todos** | **96** | **1** | **111** | **93** | **301** |
| **Todos** | **Todos** | **266** | **8** | **215** | **179** | **668** |

**(b)** Faixa × diferença de dias `batida − data_evento`. Todas as células derivam exclusivamente do raspado; confirmação no banco [nao verificado].

| Faixa | +0 | +1 | Total |
|---|---:|---:|---:|
| ok<=16h | 216 | 50 | 266 |
| longo16-24h | 6 | 2 | 8 |
| ALEM>24h | 176 | 39 | 215 |
| sem_vizinha | 148 | 31 | 179 |
| **Total** | **546** | **122** | **668** |

`+1` pode ser esperado em turno noturno: o ajuste considera candidatos em `data_evento` e `data_evento+1`, conforme os marcos da escala (`app/chamados/juizes.py:305`, `app/chamados/juizes.py:310`). A correção de cada plantio do raspado permanece [nao verificado].

**(c)** O raspado apresenta `ERROS {}` e `rc_sombra=0`; confirmação por nova execução [nao verificado]. Portanto, a condição informada de declarar “sem conclusao” por `ERROS` não vazio não foi acionada no dado fornecido [nao verificado].

**O vizinho descrito pode produzir uma faixa enganosa como duração da jornada.** O código distingue proximidade cronológica de pareamento:

- **Intervalo pode encurtar o gap:** a última E pode ser retorno do intervalo; a primeira S pode ser ida ao intervalo. O pareador absorve essas marcas sem abrir ou fechar a jornada (`app/ponto/turnos.py:1324`, `app/ponto/turnos.py:1369`, `app/ponto/turnos.py:1406`). Exemplo hipotético: jornada de 18h, retorno após 4h e S plantada no fim → gap de 14h, classificado `ok<=16h`; ocorrência na frota [nao verificado].
- **Vizinho pode pertencer a outro turno:** a entrada anterior pode já ter sido encerrada por outra saída. O pareador encerra o turno e limpa seu estado; uma S posterior sem entrada fica órfã (`app/ponto/turnos.py:1490`, `app/ponto/turnos.py:1491`, `app/ponto/turnos.py:1494`). A heurística descrita não demonstra esse vínculo; casos concretos [nao verificado].
- **Tipo gravado pode diferir do papel:** o pareador consulta o papel da ATA e o prioriza sobre E/S gravado (`app/ponto/turnos.py:1308`, `app/ponto/turnos.py:1380`). A seleção exclusivamente por tipo pode divergir desse pareamento; ocorrência no raspado [nao verificado].
- **`sem_vizinha` tem alcance limitado:** pela janela de três dias descrita, significa ausência de candidata nessa janela, não ausência em todo o histórico; implementação exata da sonda [nao verificado]. Além disso, o pareador carrega batidas não retratadas sem excluir as plantadas por origem (`app/ponto/turnos.py:1861`).

Assim, `ALEM>24h` não basta para afirmar jornada superior a 24h: o próprio pareador separa pontas que excedem o teto aplicável (`app/ponto/turnos.py:1445`, `app/ponto/turnos.py:1451`, `app/ponto/turnos.py:1452`). Quais dos 215 registros representam plantio incorreto, vizinho de outro turno ou outra situação permanece [nao verificado].

## 06/11 19:05 · sessao 3b51a9f9db90 · - · - · origem=Ronald (tmux)

Resultados de `app/relatorios`, com contagens locais separadas das contagens globais por nome do anexo. Os casos abaixo são deduções do código; ocorrência em produção não foi medida. [não verificado]

| Campo | Significados encontrados | Escritores examinados | Leitores: amostra lida | Divergente? | Caso que quebra |
|---|---|---|---|---|---|
| `ChamadoColaborador.criado_em` | Instante de criação automático (`app/chamados/models.py:61`); apresentação local na API (`app/api/views.py:1151`); apresentação do datetime cru no PDF (`app/relatorios/pdf_chamados.py:114`). | 1 automático: `app/chamados/models.py:61`. Outros escritores não censados por este agente. [não verificado] | 3 sítios: filtro por dia em `app/relatorios/views.py:924`, formatação crua em `app/relatorios/pdf_chamados.py:114`, formatação local em `app/api/views.py:1151`. | **sim** | `2026-11-07T01:30Z` → PDF `07/11/2026 01:30`; API `06/11/2026 22:30`. A diferença segue das duas formatações (`app/relatorios/pdf_chamados.py:114`; `app/api/views.py:1151`) e do fuso configurado (`app/config/settings/base.py:93`). |
| `FollowupChamado.criado_em` | Instante automático (`app/chamados/models.py:1604`); datetime cru no histórico PDF (`app/relatorios/pdf_chamados.py:141`); local na API (`app/api/views.py:1138`). | 1 automático: `app/chamados/models.py:1604`. Demais escritores não censados por este agente. [não verificado] | 2 sítios: `app/relatorios/pdf_chamados.py:141`; `app/api/views.py:1138`. | **sim** | Mesmo instante acima → datas/horas diferentes, pelos dois formatadores (`app/relatorios/pdf_chamados.py:141`; `app/api/views.py:1138`). |
| `Ausencia.data` | Início de um registro com range (`app/ponto/services/ausencia.py:342`); relatório anual trata o ano do início como universo (`app/relatorios/views.py:278`), embora some ocupação no ano (`app/relatorios/views.py:292`). | 2 atos examinados: criação (`app/ponto/services/ausencia.py:342`), edição (`app/ponto/services/ausencia.py:499`). Total global não apurado. [não verificado] | 3 sítios: absenteísmo por sobreposição (`app/relatorios/views.py:197`), atestados pelo ano inicial (`app/relatorios/views.py:278`), expansão inclusiva (`app/ponto/turnos.py:2785`). | **sim** | Médico aprovado, início `31/12/2025`, 3 dias → fim `02/01/2026` (`app/ponto/services/ausencia.py:76`). Relatório de 2026 exclui a linha (`app/relatorios/views.py:278`), apesar de ocupar 01 e 02/01 pela expansão (`app/ponto/turnos.py:2785`). |
| `Ausencia.data_fim` | Fim inclusivo (`app/ponto/services/ausencia.py:66`); ausência que atravessa o ano perde seus dias finais no filtro anual pelo início (`app/relatorios/views.py:278`). | 2 atos examinados: criação (`app/ponto/services/ausencia.py:343`), edição (`app/ponto/services/ausencia.py:500`). Total global não apurado. [não verificado] | 3 sítios: filtro mensal por sobreposição (`app/relatorios/views.py:198`), cálculo anual (`app/relatorios/views.py:292`), expansão inclusiva (`app/ponto/turnos.py:2787`). | **sim**, mesmo mecanismo da linha anterior | Mesmo caso: 2 dias ocupados em 2026 → linha ausente no relatório (`app/relatorios/views.py:278`; `app/ponto/turnos.py:2787`). |
| `LoteEspelho.data_ini` | Data inicial do pedido, conservada no job (`app/relatorios/models.py:110`; `app/relatorios/lote_espelho.py:105`). | **1**: `app/relatorios/lote_espelho.py:105`. | **3 atributos lidos**: condição e rótulo (`app/relatorios/lote_espelho.py:171`, `:172`), passagem ao gerador (`:207`). | não na amostra | Nenhum demonstrado; gerador conserva o período explícito (`app/relatorios/services.py:211`). |
| `LoteEspelho.data_fim` | Data final inclusiva do pedido (`app/relatorios/models.py:111`; `app/relatorios/lote_espelho.py:105`); geração inclui a data final da janela (`app/relatorios/pdf_espelho.py:602`). | **1**: `app/relatorios/lote_espelho.py:105`. | **3 atributos lidos**: `app/relatorios/lote_espelho.py:171`, `:172`, `:207`. | não na amostra | Nenhum demonstrado (`app/relatorios/services.py:211`; `app/relatorios/pdf_espelho.py:602`). |
| `LoteEspelho.iniciado_em` | Instante aware do disparo/assunção (`app/relatorios/lote_espelho.py:145`, `:160`); carência medida em segundos (`app/relatorios/models.py:153`). | **2**: `app/relatorios/lote_espelho.py:145`, `:160`. | **2 atributos**, mesma expressão: `app/relatorios/models.py:153`. | não na amostra | Nenhum demonstrado; compara instantes (`app/relatorios/models.py:153`). |
| `LoteEspelho.concluido_em` | Instante aware de término, inclusive falha (`app/relatorios/lote_espelho.py:76`, `:253`, `:266`). | **3**: esses sítios. | **0** leitores Python explícitos encontrados neste recorte. [não verificado] | não sei | Sem leitor para comparar. [não verificado] |
| `LoteEspelho.criado_em` | Instante automático de criação (`app/relatorios/models.py:130`). | **1 automático**: declaração citada. | **0** leitores Python explícitos encontrados neste recorte. [não verificado] | não sei | Sem leitor para comparar. [não verificado] |
| `RegistroPDFAuditavel.gerado_em` | Instante automático (`app/relatorios/models.py:42`); string sem conversão local em `__str__` (`app/relatorios/models.py:68`). | **1 automático**: declaração citada; criação da entidade em `app/relatorios/pdf_base.py:152`. | **1 atributo**: `app/relatorios/models.py:68`. | não sei | Não usei o rodapé como segundo leitor deste campo: ele recebe outro objeto/atributo `gerado_em` (`app/relatorios/pdf_base.py:55`, `:139`). |

Os seis campos locais de `relatorios` acima não aparecem nas linhas `CAMPO` fornecidas, mas existem na cópia: `app/relatorios/models.py:42`, `:110`, `:111`, `:130`, `:131`, `:132`.

Somente divergentes:

- **Família `chamado`**: escritor automático `app/chamados/models.py:61` × leitor cru `app/relatorios/pdf_chamados.py:114`; o leitor local discordante está em `app/api/views.py:1151`. As instâncias chegam diretamente do queryset (`app/relatorios/views.py:935`) ao construtor do PDF (`app/relatorios/pdf_chamados.py:171`).
- **Família `chamado`**: escritor automático `app/chamados/models.py:1604` × leitor cru `app/relatorios/pdf_chamados.py:141`; o leitor local discordante está em `app/api/views.py:1138`.
- **Família `ausencia/ferias`**, dois campos afetados pelo mesmo mecanismo: escritor do range `app/ponto/services/ausencia.py:342` e `:343`, ou edição `:499` e `:500`, × filtro do leitor `app/relatorios/views.py:278`. O escritor permite atravessar o ano por duração (`app/ponto/services/ausencia.py:76`); o leitor elimina o registro antes de calcular os dias ocupados (`app/relatorios/views.py:292`).

Parâmetros examinados: **29 declarações em 16 funções** do inventário. Relação para consolidar sem duplicação:

| Função | Parâmetros | Semântica lida |
|---|---:|---|
| `cartao_pela_celula.competencia_do_periodo` | 2 | Janela completa inclusiva, reconhecida pela autoridade de competência (`app/relatorios/cartao_pela_celula.py:27`, `:32`, `:34`). |
| `cartao_pela_celula.ate_ontem` | 1 | Fim `date`, teto em ontem local (`app/relatorios/cartao_pela_celula.py:37`, `:40`, `:41`). |
| `cartao_pela_celula.orfas_e_isencao_da_ata` | 2 | Range inclusivo de `CelulaDia.data` (`app/relatorios/cartao_pela_celula.py:277`, `:286`). |
| `cartao_pela_celula.folha_manda` | 2 | Período pedido completo usado para reconhecer competência (`app/relatorios/cartao_pela_celula.py:296`, `:321`). |
| `furos.dataset_furos` | 2 | Dates inclusivos, expansão com `+1` (`app/relatorios/furos.py:103`, `:122`). |
| `furos.semanas_calendario` | 2 | Dates inclusivos (`app/relatorios/furos.py:170`, `:174`). |
| `furos.furos_por_dia` | 2 | Dates inclusivos (`app/relatorios/furos.py:186`, `:198`). |
| `furos_pdf._card_flow` | 2 | Datas apenas formatadas (`app/relatorios/furos_pdf.py:25`, `:36`). |
| `furos_pdf.gerar_pdf_furos_bytes` | 2 | Datas conservadas no título e repassadas (`app/relatorios/furos_pdf.py:73`, `:80`, `:84`). |
| `lote_espelho.chave_do_pedido` | 2 | Datas do pedido, serialização ISO (`app/relatorios/lote_espelho.py:49`, `:59`, `:60`). |
| `lote_espelho.pedir` | 2 | Datas conservadas na entidade (`app/relatorios/lote_espelho.py:83`, `:105`). |
| `vinculo_do_dia_divergentes.Command._minutos` | 1 | Dia do cálculo, repassado à autoridade (`app/relatorios/management/commands/vinculo_do_dia_divergentes.py:32`, `:43`). |
| `pdf_espelho._coletar_dados_espelho` | 2 | Dates do período; envelope datetime para batidas, teto local em ontem (`app/relatorios/pdf_espelho.py:187`, `:264`, `:287`), dias finais incluídos (`:602`). |
| `pdf_espelho.gerar_pdf_espelho_informacional_bytes` | 2 | Dates repassados; competência somente quando a janela coincide (`app/relatorios/pdf_espelho.py:1038`, `:1046`, `:1053`). |
| `services.recusa_por_data_final` | 1 | Fim inclusivo no máximo ontem (`app/relatorios/services.py:13`, `:38`, `:39`). O relógio padrão usa `date.today()` (`:35`), enquanto `ate_ontem` usa `localdate()` (`app/relatorios/cartao_pela_celula.py:40`); diferença efetiva de TZ de processo não demonstrada. [não verificado] |
| `services.gerar_pdf_espelho_lote_bytes` | 2 | Dates explícitos conservados; ausentes, usa competência da empresa (`app/relatorios/services.py:165`, `:211`, `:214`). |

Amostra de parâmetros: **0 divergências demonstradas**, com ressalva de relógio de `recusa_por_data_final` acima; não afirmar igualdade de todos os chamadores sem censo deles. [não verificado] Para os **10 campos da tabela**, são **4 campos divergentes**, agrupados em **3 mecanismos**, **3 “não sei”** e **3 sem divergência na amostra**; essas contagens correspondem às linhas e referências da tabela, não ao universo global. [não verificado]

## 06/11 19:05 · sessao 98323398dab6 · - · - · origem=Ronald (tmux)

| Campo/parâmetros | Significados lidos | Escritores abertos | Amostra de leitores abertos | Divergente? | Caso concreto |
|---|---|---|---|---|---|
| `Batida.timestamp` | Instante recebido, sem truncamento (`app/ponto/registro_batida.py:153`, `:158`); composição de hora/data local na correção (`app/chamados/services/regularizacao_ext.py:149`); dia local pela conversão explícita (`app/ponto/turnos.py:46`, `:284`); **data do datetime cru** no recorte das fatias (`app/ponto/services/fechamento.py:245`, `:252`). | **3 sítios persistentes explícitos encontrados:** `registro_batida.py:153`, `:158`; `regularizacao_ext.py:154`, salvo em `:155`. Mais **2 transformações em memória:** `motor_calculo_v2.py:461` e `turnos.py:1529`; a segunda escreve numa cópia (`turnos.py:1525`), e o motor também trabalha em cópias (`motor_calculo_v2.py:2268`). | `turnos.py:46`, `:284`, `:1981`, `:2866`, `:2868`, `:2916`, `:2918`; `motor_calculo_v2.py:73`, `:894`, `:2403`; `fechamento.py:245`, `:252`; `services/adesao.py:45`; `signals.py:23`; `adaptador_motor.py:37`. Contagem total por modelo não estabelecida; `621` do anexo mistura `PingGeo.timestamp`. [nao verificado] | **Sim**: localdate × date cru. | `2026-10-03T01:00:00+00:00` equivale a `02/10 22h` no fuso configurado (`app/config/settings/base.py:93`). Com fatia A abrangendo 01–04/10, `_meus={01,02,04}` e dia03 atribuído a B, `fechamento.py:252` descarta essa batida de A porque o `date()` cru é 03; o dia local é 02 (`turnos.py:46`). A lista chega diretamente da carga ORM (`fechamento.py:624`), e não há localização antes do filtro (`:242`–`:253`). Consequência demonstrável: perda da batida na alimentação da fatia; valor monetário final não calculado. [nao verificado] |
| `TurnoMaterializado.data_turno` | `date` da jornada produzido pelo juiz; pode ser véspera da data civil de entrada ou saída (`app/ponto/turnos.py:284`, `:295`, `:306`, `:316`). O núcleo copia esse resultado (`app/ponto/nucleo.py:50`). | **1:** `app/ponto/nucleo.py:50`, persistência por `bulk_create` em `:59`. | `processar_alertas_turno.py:97`, `:106`, `:109`; `turnos.py:3122`; `motor_calculo_v2.py:894`; `turnos.py:2916`. Total por modelo não estabelecido: `187` do anexo mistura três modelos. [nao verificado] | **Não na amostra; universal não sei.** | Nenhum contraexemplo confirmado na amostra. [nao verificado] |
| `TurnoMaterializado.dt_fim_previsto` | Instante aware do fim previsto, ancorado em `data_turno`; overnight acrescenta um dia (`app/ponto/nucleo.py:31`, `:37`; `app/ponto/selecao_periodo.py:144`, `:146`, `:149`, `:164`, `:167`). | **1:** `app/ponto/nucleo.py:55`, persistência em `:59`. | `app/ponto/selecao_periodo.py:290`; `app/ponto/management/commands/processar_alertas_turno.py:100`, `:109`. | **Não na amostra; universal não sei.** | Não há contraexemplo temporal confirmado. O escritor chama `janela_turno_de` sem `ec` (`nucleo.py:37`), enquanto o fallback do cron passa `ec` (`processar_alertas_turno.py:109`): isso muda a fonte dos marcos, template × DNA (`selecao_periodo.py:136`, `:151`), mas não demonstrei discordância nos quatro eixos solicitados. |
| `dia_corte` e `data` de `periodo_apuracao[_atual]`, `janela_atual`, `janela_anterior` | `data` é dia de calendário; retorno é par de `date`, corte até corte−1, inclusivos; dia igual ao corte pertence à competência seguinte (`app/ponto/janelas.py:15`, `:16`, `:20`, `:27`, `:32`). Anterior parte do dia imediatamente anterior ao início da atual (`:71`, `:72`). | Funções puras: `janelas.py:10`, `:24`, `:60`, `:66`. | Chamador de fechamento compõe início/fim locais com esse retorno (`app/ponto/services/fechamento.py:612`–`:614`). | **Não na amostra.** | Corte21: `20/10` → `[21/09,20/10]`; `21/10` → `[21/10,20/11]`, conforme `janelas.py:27`–`:33`. |
| `ini/fim` de `turnos_do_colab`, `janela_com_pad`, `dias_encerrados`, `par_completo_da_janela`, `dia_das_batidas`, `CargaDaJanela`; `data_turno` de `dt_fim_previsto_de`; `dia` de `batidas_do_dia_turno` e `turno_aberto_ha` | Dias/jornadas, intervalo fechado; pad de dois dias antes e um depois (`app/ponto/turnos.py:1845`, `:1993`, `:2163`, `:2968`); consulta do dia por igualdade (`:3034`, `:3122`); núcleo também usa recorte fechado no SQL (`app/ponto/nucleo.py:81`). | Alimentação sem escrita própria desses parâmetros (`turnos.py:1848`, `:2125`, `:2171`, `:2874`, `:2936`, `:3013`, `:3115`; `nucleo.py:31`). | Corpos e chamadas internas citados. | **Não na amostra; universal não sei.** | Nenhum caso quebrado demonstrado. [nao verificado] |
| `ini/fim` de `batidas_apuraveis` e `ids_com_adesao` | **Instantes datetime**, janela fechada `gte/lte`; `None` desliga a respectiva borda (`app/ponto/turnos.py:2865`–`:2868`; `app/ponto/services/adesao.py:59`–`:62`). Diferem legitimamente dos parâmetros homônimos de janelas de dias. | Funções de consulta (`turnos.py:2844`; `services/adesao.py:48`). | Fechamento passa datetimes aware ampliados (`fechamento.py:613`–`:624`). | **Não na amostra.** | Não tratar homônimos `ini/fim` como uma única entidade temporal; os contratos locais são distintos (`turnos.py:2847`, `:3017`). |
| `data_ini/data_fim` e `dia` do motor/calculador lidos | Dias da jornada, associados pelo mapa instante→`data_turno` (`app/ponto/motor_calculo_v2.py:884`–`:894`); fallback civil quando o mapa não respondeu (`:2401`–`:2403`); overrides preservam mapa (`:2942`, `:3313`); calculador recebe dia da jornada (`app/ponto/calculador/periodo.py:340`–`:346`) e transmite `dia=k` (`app/ponto/calculador/alimentacao.py:525`). | Parâmetros em `motor_calculo_v2.py:867`, `:884`, `:2265`, `:2899`, `:3295`; `calculador/alimentacao.py:163`, `:212`, `:305`, `:353`; `calculador/periodo.py:291`, `:340`; `calculador/regras.py:578`. | Corpos indicados e `alimentacao.py:315`, `:316`. | **Não sei universalmente**: fallback civil existe, mas não demonstrei entrada em que mapa ausente deva responder véspera. | Não acusar divergência só pela existência do fallback (`motor_calculo_v2.py:2401`). |

Divergência confirmada: escritor `app/ponto/registro_batida.py:153`/`:158` e corretor `app/chamados/services/regularizacao_ext.py:154` × leitor `app/ponto/services/fechamento.py:245`/`:252`. Família do escritor: **batida** (`app/core/contratos_estruturais.py:92`); leitor: **folha/export**, cujo escritor de fechamento é nomeado em `app/core/contratos_estruturais.py:155`; geometria/localização de referência: **turno/marcos** (`app/docs/JUIZES.md:22`).

Não confundir `dia encerrado` em `agora >= fim` (`app/ponto/turnos.py:2122`) com `fim vencido` em `agora > fim` (`:3133`): são perguntas diferentes, não prova de discordância do significado do campo.

Falso positivo temporal do inventário: `dia_de_trabalho=True` é booleano (`app/ponto/calculador/regras.py:577`), e chega como predicado (`app/ponto/calculador/alimentacao.py:519`).

Não concluí censo integral de parâmetros/leitores de todos os módulos atribuídos; as linhas acima delimitam a amostra efetivamente aberta. [nao verificado]

## 06/11 19:04 · sessao 1758632a6288 · - · - · origem=Ronald (tmux)

Sim, a heurística pode classificar um gap que não corresponde ao par da jornada.

- **Intervalo encurta o gap:** a última `E` pode ser retorno do intervalo; a primeira `S`, ida ao intervalo. O pareador trata essas marcas como internas e não como bordas: `app/ponto/turnos.py:1324`, `app/ponto/turnos.py:1369`, `app/ponto/turnos.py:1406`. Exemplo hipotético: jornada de 18h, retorno após 4h e saída plantada no fim → heurística mede 14h, passando de `longo16-24h` para `ok<=16h`; ocorrência concreta na frota [nao verificado].
- **Outro turno fornece o vizinho:** uma entrada anterior já encerrada continua sendo a última entrada real quando falta a entrada do turno da saída plantada. O pareador encerra e limpa `cur` em `app/ponto/turnos.py:1489`; uma saída sem entrada recebe turno órfão em `app/ponto/turnos.py:1493`. A heurística descrita não verifica esse consumo; ocorrência concreta [nao verificado].
- **Tipo gravado não determina papel:** o pareador lê o papel da ATA e pode substituir `E/S` gravado: `app/ponto/turnos.py:1308`, `app/ponto/turnos.py:1380`. Assim, selecionar somente por `tipo` pode escolher uma marca interna ou a borda oposta; ocorrência concreta [nao verificado].
- **Gap grande não prova jornada longa:** o pareador separa entrada e saída quando o intervalo excede o teto aplicável: `app/ponto/turnos.py:1445`, `app/ponto/turnos.py:1451`. Portanto `ALEM>24h` na sonda pode indicar distância a outro turno, sem demonstrar um turno de mais de 24h; enquadramento dos EX concretos [nao verificado].
- **Excluir todas as plantadas muda o universo:** o pareador carrega todas as batidas não retratadas, sem filtro de origem, em `app/ponto/turnos.py:1861`. Se a sonda usa somente vizinhas “reais”, pode ignorar a contraparte plantada correta e buscar outra jornada ou produzir `sem_vizinha`; filtro exato da sonda e ocorrências [nao verificado].

A data overnight, por sua vez, vem da cadeia de marcos do vínculo: o código testa candidatos em `data_evento` e `data_evento+1`, em `app/chamados/juizes.py:305`, `app/chamados/juizes.py:310`. Portanto `batida-data_evento=+1` isoladamente é compatível com o funcionamento previsto; legitimidade de cada caso [nao verificado].

## 06/11 19:02 · sessao 1d43b1606e53 · BUG-PLANTIO-NOTURNO-MAIS-1 · sonda · origem=Code

Não reexecutei: foi fornecida a saída raspada, sem o fonte da `sonda_frota2.py`; sua implementação e seu hash não foram conferidos. [nao verificado] O wrapper disponível cria e reescreve arquivos, contrariando a restrição desta tarefa: `bin/sonda_leitura.sh:126`, `bin/sonda_leitura.sh:134`, `bin/sonda_leitura.sh:216`.

Reprodução integral da saída fornecida; ela não contém linhas `EX`. [nao verificado]

```text
=== SOMBRA (sombra_db, leitor) 06/11 19:00:48 ===
sonda_leitura: sonda_frota2.py [b288535e39c3395b] no cpuset 4-7, lendo sombra como leitor (LEITURA)

UNIVERSO perguntas 9729 batidas plantadas vivas 668
N orfao_14h|E|sem_vizinha|mes_batida=09|batida-data_evento=+0 6
N orfao_14h|E|sem_vizinha|mes_batida=10|batida-data_evento=+0 3
N orfao_14h|S|sem_vizinha|mes_batida=09|batida-data_evento=+0 67
N orfao_14h|S|sem_vizinha|mes_batida=09|batida-data_evento=+1 31
N orfao_14h|S|sem_vizinha|mes_batida=10|batida-data_evento=+0 162
N orfao_14h|S|sem_vizinha|mes_batida=10|batida-data_evento=+1 89
N orfao_14h|S|sem_vizinha|mes_batida=11|batida-data_evento=+0 9
N saida_sem_entrada|E|sem_vizinha|mes_batida=09|batida-data_evento=+0 103
N saida_sem_entrada|E|sem_vizinha|mes_batida=10|batida-data_evento=+0 176
N saida_sem_entrada|E|sem_vizinha|mes_batida=10|batida-data_evento=+1 2
N saida_sem_entrada|E|sem_vizinha|mes_batida=11|batida-data_evento=+0 16
N saida_sem_entrada|S|sem_vizinha|mes_batida=10|batida-data_evento=+0 4
ERROS {}
rc_sombra=0
```

(a) Totais calculados exclusivamente das linhas `N`; os zeros representam ausência de contagens nessas faixas no raspado. [nao verificado]

| Motivo | Mês da batida | ok≤16h | longo16–24h | ALEM>24h | sem_vizinha | Total |
|---|---|---:|---:|---:|---:|---:|
| orfao_14h | 09 | 0 | 0 | 0 | 104 | 104 |
| orfao_14h | 10 | 0 | 0 | 0 | 254 | 254 |
| orfao_14h | 11 | 0 | 0 | 0 | 9 | 9 |
| **orfao_14h** | **Todos** | **0** | **0** | **0** | **367** | **367** |
| saida_sem_entrada | 09 | 0 | 0 | 0 | 103 | 103 |
| saida_sem_entrada | 10 | 0 | 0 | 0 | 182 | 182 |
| saida_sem_entrada | 11 | 0 | 0 | 0 | 16 | 16 |
| **saida_sem_entrada** | **Todos** | **0** | **0** | **0** | **301** | **301** |
| **Ambos** | **Todos** | **0** | **0** | **0** | **668** | **668** |

Totais por mês: **09=207; 10=436; 11=25**. A soma das linhas `N` coincide com as 668 batidas declaradas no `UNIVERSO`. [nao verificado]

(b) Cruzamento faixa × `batida-data_evento`, calculado do raspado. [nao verificado]

| Faixa | +0 | +1 | Total |
|---|---:|---:|---:|
| ok≤16h | 0 | 0 | 0 |
| longo16–24h | 0 | 0 | 0 |
| ALEM>24h | 0 | 0 | 0 |
| sem_vizinha | 546 | 122 | 668 |

Dos 122 registros em `+1`, 120 são `orfao_14h` e 2 são `saida_sem_entrada`. Não há duração demonstrada para esses registros no dado fornecido. [nao verificado] O deslocamento `+1` pode ser legítimo: o ajuste noturno considera a hora tanto em `data_evento` quanto no dia seguinte, dentro do envelope previsto. `app/chamados/juizes.py:306`, `app/chamados/juizes.py:310`, `app/chamados/juizes.py:313`.

(c) O raspado apresenta `ERROS {}`, portanto a condição solicitada de declarar “sem conclusao” por erros não foi acionada. Isso não valida o pareamento nem demonstra ausência de erro de plantio. [nao verificado]

**Sim, a escolha de vizinhos descrita pode classificar incorretamente**, pelos seguintes mecanismos presentes na cópia:

- **Intervalo encurta a duração aparente:** a última E pode ser volta de intervalo; a primeira S pode ser ida ao intervalo. O pareador trata essas batidas como internas, sem abrir ou fechar turno. `app/ponto/turnos.py:1324`, `app/ponto/turnos.py:1369`.
- **Tipo gravado pode divergir do papel efetivo:** o pareador consulta a ata e usa seu papel antes de recorrer ao campo `tipo`. Uma busca apenas por E/S gravado pode escolher a ponta errada. `app/ponto/turnos.py:1308`, `app/ponto/turnos.py:1380`.
- **Proximidade não garante mesmo turno:** uma nova E deixa a anterior aberta; uma S além do teto permitido não fecha a entrada. Logo, encontrar uma batida oposta dentro de três dias não comprova um par válido. `app/ponto/turnos.py:1415`, `app/ponto/turnos.py:1445`, `app/ponto/turnos.py:1451`.
- **“Real” exige conferir o filtro:** se significar `pergunta_origem IS NULL`, o próprio campo admite também plantio anterior a 06/09. Além disso, o universo apurável inclui batidas vivas sem excluir origem plantada. `app/ponto/models.py:40`, `app/ponto/models.py:44`, `app/ponto/turnos.py:2863`.
- **A janela limita `sem_vizinha`:** ausência no recorte de três dias não prova ausência fora dele; a consulta apurável aplica explicitamente as bordas recebidas. `app/ponto/turnos.py:2865`, `app/ponto/turnos.py:2867`.

Os filtros efetivos da sonda e a ocorrência desses mecanismos entre as 668 batidas permanecem **[nao verificado]**.

## 06/11 19:01 · sessao a41448ae7296 · - · - · origem=Ronald (tmux)

A data da batida pode legitimamente diferir de `data_evento`: o código monta inicialmente o timestamp na data do evento e depois aplica o ajuste noturno; o ajuste consulta os marcos da escala e aceita a hora no evento ou no dia seguinte. (`app/chamados/services/materializacao.py:412-420`; `app/chamados/juizes.py:302-314`)

O teste da volta de intervalo de um turno 19h–07h exige que a entrada de 00h vá para o dia seguinte; portanto, `batida-data_evento=+1` isoladamente não identifica erro. (`app/chamados/tests/test_plantio_intervalo_overnight.py:19-21`; `app/chamados/tests/test_plantio_intervalo_overnight.py:27-33`)

Riscos concretos da escolha de vizinho, a conferir contra os filtros efetivos da sonda:

- Se o filtro “real” for `pergunta_origem IS NULL`, ele admite também plantio anterior a 06/09, conforme a definição do campo. (`app/ponto/models.py:40-44`)
- A materialização pode reutilizar uma batida real existente e gravar nela `pergunta_origem`. Assim, excluir toda batida com pergunta de origem também pode excluir uma batida real reutilizada. (`app/chamados/services/materializacao.py:832-834`; `app/chamados/services/materializacao.py:916-924`)
- Há batidas E/S intrajornada; o modelo declara expressamente que `is_intervalo` não forma turno. Selecionar só pelo tipo e pela proximidade, sem considerar intervalo, pode tomar uma volta E como entrada do turno ou uma saída de intervalo S como saída final. (`app/ponto/models.py:9-11`; `app/ponto/models.py:26`)
- A identidade de turno usada pelo código é colaborador + timestamp de entrada; o próprio juiz distingue dois turnos no mesmo dia. A proximidade temporal dentro de três dias, por si, não verifica essa identidade. (`app/chamados/juizes.py:1880-1883`; `app/chamados/juizes.py:1911-1922`)

Não verifiquei a ocorrência desses cenários nas 668 batidas nem os filtros da `sonda_frota2.py`; os agregados fornecidos não exibem pares individuais. [nao verificado]

## 06/11 19:01 · sessao 7ce4087313eb · - · - · origem=Ronald (tmux)

Sim: a escolha descrita pode divergir do pareamento da cópia. Os riscos são demonstráveis no código; a ocorrência entre as 668 batidas do dado é [nao verificado].

- **Tipo gravado diferente do papel efetivo:** o pareador consulta o papel da ata e o usa antes de recorrer a `tipo`. Buscar exclusivamente E/S gravados pode ignorar uma vizinha válida ou selecionar uma inválida. `app/ponto/turnos.py:1308`, `app/ponto/turnos.py:1380`. Há teste explícito de plantio com “vizinha tipo mentiroso”: `app/ponto/tests/test_plantio_paridade.py:8`.
- **Intervalo confundido com borda:** batidas de intervalo entram como internas e não abrem/fecham turno. Uma última E pode ser volta de intervalo; uma primeira S pode ser ida ao intervalo, encurtando a duração medida. `app/ponto/turnos.py:1324`, `app/ponto/turnos.py:1369`. O teste preserva um único turno de 06:57 a 15:51 apesar de S 13:02 e E 14:31: `app/ponto/tests/test_parear_borda_vs_intervalo.py:24`, `app/ponto/tests/test_parear_borda_vs_intervalo.py:28`.
- **Vizinha já pertencente a outro turno:** ao encontrar uma nova E, o pareador mantém o turno anterior aberto e abre outro; uma S posterior não é automaticamente a saída de qualquer E anterior. `app/ponto/turnos.py:1415`. Além disso, S acima do teto da jornada deixa E aberta e vira órfã. Uma distância calculável dentro de três dias não prova par válido. `app/ponto/turnos.py:1445`.
- **Filtro “real” pode excluir contraparte apurável plantada:** a autoridade do universo apurável exige `retratada_em` nulo e aplica colaborador/janela, sem filtro por origem. Se “real” excluir batidas plantadas, `sem_vizinha` não equivale a “sem contraparte apurável”. `app/ponto/turnos.py:2863`. O filtro efetivo da sonda, ausente no material recebido por este agente, é [nao verificado].
- **Janela de três dias:** `sem_vizinha` nessa regra expressa ausência apenas no recorte pesquisado; ausência em toda a vida é [nao verificado]. A função apurável permite desativar as bordas da janela, justamente para a pergunta sobre entrada sem saída. `app/ponto/turnos.py:2855`.

Para o cruzamento calendário/duração, a cópia tem teste no qual batidas da madrugada pertencem ao turno da véspera; portanto deslocamento de calendário +1 não estabelece duração superior a 24 horas. `app/ponto/tests/test_turno_da_madrugada_pertence_a_vespera.py:25`.

## 06/11 19:00 · sessao 4e9cf1fe587d · ESMERIL-SEMANTICA · censo · origem=Code

```python
#!/usr/bin/env python3
"""Uso: python3 -I inventario_datas.py <raiz_do_repo>."""
import ast
from collections import defaultdict
from pathlib import Path
import re
import sys
import tokenize
import warnings

APPS = {"ponto", "escala", "chamados", "colaboradores", "folha"}
PARAMETRO = re.compile(
    r"^(data(_\w+)?|dia(_\w+)?|data_turno|data_evento|ini|fim|inicio|competencia_\w+)$"
)
ESCRITA = {
    "create", "get_or_create", "update_or_create", "update", "bulk_create", "abrir"
}
FILTROS = {"filter", "exclude", "get", "Q"}
STRINGS = {"values", "values_list", "order_by", "only", "F"}


def celula(value):
    return (str(value).replace("\\", "\\\\").replace("\t", "\\t")
            .replace("\r", "\\r").replace("\n", "\\n"))


def emitir(*values):
    print("\t".join(celula(value) for value in values))


def canon(nome):
    return nome[4:] if nome.startswith("app.") else nome


def pontilhado(node):
    if isinstance(node, ast.Name):
        return node.id
    if isinstance(node, ast.Attribute):
        base = pontilhado(node.value)
        return base + "." + node.attr if base else node.attr
    return ""


def literal_string(node):
    if isinstance(node, ast.Constant) and isinstance(node.value, str):
        return node.value
    return None


def itens_literais(node):
    if isinstance(node, (ast.List, ast.Tuple, ast.Set)):
        return node.elts
    return ()


def trecho(node, source, limite=160):
    if isinstance(node, ast.Constant):
        text = repr(node.value)
    else:
        text = ast.get_source_segment(source, node) or ast.dump(node)
    text = " ".join(text.split())
    return text if len(text) <= limite else text[:limite - 3] + "..."


class Escopo:
    def __init__(self, parent=None, partes=(), tipo="modulo"):
        self.parent = parent
        self.partes = partes
        self.tipo = tipo
        self.bindings = {}

    def resolver(self, node):
        name = pontilhado(node)
        if not name:
            return ""
        first, dot, rest = name.partition(".")
        scope = self
        while scope is not None:
            if first in scope.bindings:
                base = scope.bindings[first]
                return canon(base + (dot + rest if dot else ""))
            scope = scope.parent
        return canon(name)


class Indice(ast.NodeVisitor):
    """Resolve imports lexicais e classes; nao executa o modulo."""
    def __init__(self, tree, modulo, pacote):
        self.modulo = modulo
        self.pacote = pacote
        self.scope = Escopo()
        self.at = {}
        self.classes = {}
        self.visit(tree)

    def visit(self, node):
        self.at[node] = self.scope
        return super().visit(node)

    def visit_Import(self, node):
        for item in node.names:
            if item.asname:
                self.scope.bindings[item.asname] = item.name
            else:
                first = item.name.split(".")[0]
                self.scope.bindings[first] = first

    def visit_ImportFrom(self, node):
        if node.level:
            parts = self.pacote.split(".") if self.pacote else []
            drop = node.level - 1
            parts = parts[:len(parts) - drop] if drop <= len(parts) else []
            if node.module:
                parts.extend(node.module.split("."))
            base = ".".join(parts)
        else:
            base = node.module or ""
        for item in node.names:
            if item.name != "*":
                self.scope.bindings[item.asname or item.name] = (
                    base + "." + item.name if base else item.name
                )

    def entrar(self, node, tipo):
        parent = self.scope
        partes = parent.partes + (node.name,)
        ident = ".".join(filter(None, (self.modulo, ".".join(partes))))
        parent.bindings[node.name] = ident
        self.scope = Escopo(parent, partes, tipo)
        if tipo == "classe":
            self.classes[ident] = (node, parent)
        self.generic_visit(node)
        self.scope = parent

    def visit_ClassDef(self, node):
        self.entrar(node, "classe")

    def visit_FunctionDef(self, node):
        self.entrar(node, "funcao")

    visit_AsyncFunctionDef = visit_FunctionDef


class Usos(ast.NodeVisitor):
    def __init__(self, arquivo, indice, campos, modelos, eventos):
        self.arquivo = arquivo
        self.indice = indice
        self.campos = campos
        self.modelos = modelos
        self.nomes_modelos = {name.rsplit(".", 1)[-1] for name in modelos}
        self.eventos = eventos
        self.pilha = []

    def contexto(self):
        return ".".join(self.pilha) or "<modulo>"

    def registrar(self, tipo, nome, node, forma=""):
        if tipo in {"ESC", "LEI"} and nome not in self.campos:
            return
        self.eventos.append((
            tipo, nome, self.arquivo, node.lineno, node.col_offset,
            self.contexto(), forma
        ))

    def entrar(self, node):
        self.pilha.append(node.name)
        if isinstance(node, (ast.FunctionDef, ast.AsyncFunctionDef)):
            args = node.args
            params = list(args.posonlyargs) + list(args.args) + list(args.kwonlyargs)
            params += [arg for arg in (args.vararg, args.kwarg) if arg is not None]
            for arg in params:
                if PARAMETRO.fullmatch(arg.arg):
                    self.registrar("PARAM", arg.arg, arg)
        self.generic_visit(node)
        self.pilha.pop()

    visit_ClassDef = entrar
    visit_FunctionDef = entrar
    visit_AsyncFunctionDef = entrar

    def visit_Attribute(self, node):
        if isinstance(node.ctx, ast.Store):
            self.registrar("ESC", node.attr, node, "atributo.Store")
        elif isinstance(node.ctx, ast.Load):
            self.registrar("LEI", node.attr, node, "atributo.Load")
        self.generic_visit(node)

    def visit_Call(self, node):
        scope = self.indice.at[node]
        raw = pontilhado(node.func)
        resolved = scope.resolver(node.func)
        # Attribute funciona tambem em chamadas encadeadas: qs().filter(...).
        final = (node.func.attr if isinstance(node.func, ast.Attribute)
                 else raw.rsplit(".", 1)[-1])
        resolved_final = resolved.rsplit(".", 1)[-1]
        called = resolved or raw or "<chamada dinamica>"
        constructor = (resolved in self.modelos
                       or final in self.nomes_modelos)
        filtro = final in FILTROS or resolved_final == "Q"
        strings = final in STRINGS or resolved_final == "F"

        for kw in node.keywords:
            if kw.arg and PARAMETRO.fullmatch(kw.arg):
                self.registrar("ARG", kw.arg, kw, called)

            if kw.arg in self.campos and (final in ESCRITA or constructor):
                self.registrar("ESC", kw.arg, kw, "kwarg:" + (final or called))

            if kw.arg and filtro:
                field = kw.arg.split("__", 1)[0]
                self.registrar("LEI", field, kw, "filtro:" + kw.arg)

            if kw.arg == "defaults" and isinstance(kw.value, ast.Dict):
                for key in kw.value.keys:
                    field = literal_string(key)
                    if field is not None:
                        self.registrar("ESC", field, key, "defaults:" + called)

            if kw.arg == "update_fields":
                for item in itens_literais(kw.value):
                    field = literal_string(item)
                    if field is not None:
                        self.registrar("ESC", field, item, "update_fields")

        if final == "setattr" and len(node.args) >= 2:
            field = literal_string(node.args[1])
            if field is not None:
                self.registrar("ESC", field, node.args[1], "setattr")

        if strings:
            # Expressoes nos kwargs continuam sendo visitadas abaixo.
            for arg in node.args:
                value = literal_string(arg)
                if value is None:
                    continue
                text = (
                    value[1:]
                    if final == "order_by" and value.startswith("-")
                    else value
                )
                field = text.split("__", 1)[0]
                self.registrar(
                    "LEI", field, arg,
                    "string:" + (final or called) + ":" + value
                )

        self.generic_visit(node)


def excluido(rel):
    return ("migrations" in rel.parts[:-1] or "tests" in rel.parts[:-1]
            or rel.name.startswith("test_"))


def inventariar(root):
    erros = []
    fontes = []
    app_dir = root / "app"
    if not app_dir.is_dir():
        emitir("ERRO", app_dir, "FileNotFoundError: diretorio app inexistente")
        emitir("ERROS=1")
        return 2

    # A exclusao de migrations/tests aplica-se aos usos; nao aos modelos.
    for path in sorted(app_dir.rglob("*.py"), key=lambda p: p.as_posix()):
        rel_app = path.relative_to(app_dir)
        candidato = bool(rel_app.parts) and rel_app.parts[0] in APPS
        usos = not excluido(rel_app)
        if not (candidato or usos):
            continue

        arquivo = path.relative_to(root).as_posix()
        try:
            with tokenize.open(str(path)) as handle:
                source = handle.read()
            with warnings.catch_warnings():
                warnings.simplefilter("ignore", SyntaxWarning)
                tree = ast.parse(source, filename=arquivo)

            parts = list(rel_app.with_suffix("").parts)
            init = parts[-1] == "__init__"
            if init:
                parts.pop()
            modulo = ".".join(parts)
            pacote = modulo if init else ".".join(parts[:-1])
            indice = Indice(tree, modulo, pacote)
        except Exception as exc:
            erros.append((arquivo, type(exc).__name__ + ": " + str(exc)))
            continue

        fontes.append((arquivo, source, tree, indice, candidato, usos))

    classes = {}
    for _, _, _, indice, _, _ in fontes:
        classes.update(indice.classes)

    modelos = {"django.db.models.Model"}
    while True:
        novos = {
            ident for ident, (node, scope) in classes.items()
            if ident not in modelos
            and any(scope.resolver(base) in modelos for base in node.bases)
        }
        if not novos:
            break
        modelos.update(novos)
    modelos.discard("django.db.models.Model")

    campos = []
    donos = defaultdict(set)
    declaracoes = defaultdict(set)

    for arquivo, source, tree, indice, candidato, _ in fontes:
        if not candidato:
            continue
        app = Path(arquivo).parts[1]

        for node in ast.walk(tree):
            if not isinstance(node, (ast.Assign, ast.AnnAssign)):
                continue
            scope = indice.at[node]
            if scope.tipo != "classe":
                continue

            ident = ".".join(filter(None, (
                indice.modulo, ".".join(scope.partes)
            )))
            value = node.value
            if ident not in modelos or not isinstance(value, ast.Call):
                continue

            tipo = scope.resolver(value.func)
            if tipo not in {
                "django.db.models.DateField",
                "django.db.models.DateTimeField",
            }:
                continue

            targets = (
                node.targets if isinstance(node, ast.Assign) else [node.target]
            )
            kwargs = {kw.arg: kw.value for kw in value.keywords if kw.arg}
            relevantes = []
            for key in ("auto_now", "auto_now_add", "null", "help_text"):
                if key in kwargs:
                    relevantes.append(key + "=" + trecho(
                        kwargs[key], source, 100 if key == "help_text" else 160
                    ))

            for target in targets:
                if not isinstance(target, ast.Name):
                    continue
                owner = app + "." + ".".join(scope.partes)
                campos.append((
                    owner + "." + target.id,
                    tipo.rsplit(".", 1)[-1],
                    arquivo,
                    target.lineno,
                    "; ".join(relevantes) or "-",
                ))
                donos[target.id].add(ident)
                declaracoes[target.id].add(
                    arquivo + ":" + str(target.lineno)
                )

    eventos = []
    for arquivo, _, tree, indice, _, usos in fontes:
        if usos:
            Usos(arquivo, indice, set(donos), modelos, eventos).visit(tree)

    emitir(
        "NOTA",
        "[nao verificado] Inventario AST por nome; "
        "o casamento nao distingue modelo."
    )
    emitir(
        "NOTA",
        "[nao verificado] Contagens de ocorrencias sintaticas, "
        "sem inferencia de execucao."
    )
    emitir(
        "NOTA",
        "[nao verificado] GENERICO significa nome declarado "
        "em pelo menos dois modelos."
    )
    emitir(
        "NOTA",
        "[nao verificado] ARQUIVOS conta a uniao dos arquivos "
        "com ESC/LEI, sem declaracoes isoladas."
    )

    for nome, tipo, arquivo, linha, kwargs in sorted(campos):
        emitir("CAMPO", nome, tipo, arquivo + ":" + str(linha), kwargs)

    contagens = {
        nome: {"ESC": 0, "LEI": 0, "arquivos": set()} for nome in donos
    }

    for tag, nome, arquivo, linha, coluna, contexto, forma in sorted(
        eventos, key=lambda e: (e[2], e[3], e[4], e[0], e[1], e[6])
    ):
        row = [tag, nome, arquivo + ":" + str(linha), contexto]
        if tag != "PARAM":
            row.append(forma)
        emitir(*row)

        if tag in {"ESC", "LEI"}:
            contagens[nome][tag] += 1
            contagens[nome]["arquivos"].add(arquivo)

    for arquivo, exc in sorted(erros):
        emitir("ERRO", arquivo, exc)

    for nome in sorted(donos, key=lambda name: (-contagens[name]["LEI"], name)):
        counts = contagens[nome]
        emitir(
            "RESUMO",
            nome,
            "ESC=" + str(counts["ESC"]),
            "LEI=" + str(counts["LEI"]),
            "ARQUIVOS=" + str(len(counts["arquivos"])),
            "GENERICO" if len(donos[nome]) >= 2 else "-",
            "MODELOS=" + str(len(donos[nome])),
            "DECLARACOES=" + ",".join(sorted(declaracoes[nome])),
        )

    emitir("ERROS=" + str(len(erros)))
    return 1 if erros else 0


def main():
    if len(sys.argv) != 2:
        print(__doc__, file=sys.stderr)
        return 2
    return inventariar(Path(sys.argv[1]).expanduser().resolve())


if __name__ == "__main__":
    raise SystemExit(main())
```

- [nao verificado] O casamento por nome pode incluir objetos e métodos sem relação com os modelos; não infere tipos dos receptores.
- [nao verificado] Não detecta `getattr`, `setattr` com nome calculado, acesso por `__dict__`, SQL cru ou código gerado.
- [nao verificado] Não expande `**kwargs`, `*args`, dicionários ou listas guardados em variáveis, nem propaga valores entre funções.
- [nao verificado] Reconhece aliases explícitos de imports; não resolve aliases por atribuição, reexportações, imports `*` ou rebinding conforme o fluxo.
- [nao verificado] Identifica modelos pela cadeia estática até `django.db.models.Model`; bases externas, fábricas e metaclasses podem escapar.
- [nao verificado] Reconhece o primeiro componente de `campo__lookup`; não percorre relações como `relacao__campo`.
- [nao verificado] Conta ocorrências sintáticas, inclusive código morto; atribuição e `update_fields` podem contar separadamente a mesma operação.
- [nao verificado] `auto_now`/`auto_now_add` entram em CAMPOS, sem gerar escritores implícitos; `AugAssign` entra como Store conforme o AST.
- [nao verificado] PARAM/ARG registram nomes e locais; não comprovam transferência de valores ou significado temporal.
- [nao verificado] Erros tornam o inventário parcial; a sintaxe aceita depende da versão do Python usada.

## 06/11 18:58 · sessao 24c9748813fc · BUG-PLANTIO-NOTURNO-MAIS-1 · sonda · origem=Code

Consolidação da saída fornecida, sem nova execução da sonda. O cabeçalho informa **06/11 18:56:15**, mas não informa a data do dump da sombra. [nao verificado]

**(a) UNIVERSO: 9.729**, sendo 6.733 em 10/2026 e 2.996 em 11/2026. A soma da tabela abaixo fecha nesse universo. [nao verificado]

**(b) Motivo × classe × estado × competência.** `D_e_dia_da_saida` significa `data_evento` no dia da saída, conforme a definição fornecida. [nao verificado]

| Motivo | Classe | Estado | 10/2026 | 11/2026 | Evidência |
|---|---|---|---:|---:|---|
| orfao_14h | D_e_dia_da_saida | aberta | 25 | 22 | [nao verificado] |
| orfao_14h | D_e_dia_da_saida | materializada | 92 | 30 | [nao verificado] |
| orfao_14h | D_e_dia_da_saida | respondida | 9 | 4 | [nao verificado] |
| orfao_14h | coerente | aberta | 32 | 53 | [nao verificado] |
| orfao_14h | coerente | materializada | 485 | 162 | [nao verificado] |
| orfao_14h | coerente | respondida | 11 | 28 | [nao verificado] |
| orfao_14h | outro | materializada | 19 | 4 | [nao verificado] |
| orfao_14h | sem_turno | aberta | 755 | 640 | [nao verificado] |
| orfao_14h | sem_turno | materializada | 2.660 | 712 | [nao verificado] |
| orfao_14h | sem_turno | respondida | 85 | 59 | [nao verificado] |
| saida_sem_entrada | D_e_dia_da_saida | aberta | 5 | 2 | [nao verificado] |
| saida_sem_entrada | D_e_dia_da_saida | materializada | 10 | 3 | [nao verificado] |
| saida_sem_entrada | D_e_dia_da_saida | respondida | 13 | 1 | [nao verificado] |
| saida_sem_entrada | coerente | aberta | 35 | 7 | [nao verificado] |
| saida_sem_entrada | coerente | materializada | 282 | 66 | [nao verificado] |
| saida_sem_entrada | coerente | respondida | 9 | 6 | [nao verificado] |
| saida_sem_entrada | sem_turno | aberta | 731 | 646 | [nao verificado] |
| saida_sem_entrada | sem_turno | materializada | 1.420 | 492 | [nao verificado] |
| saida_sem_entrada | sem_turno | respondida | 55 | 59 | [nao verificado] |

**(c) EFEITO — distribuição de `delta_civil` das batidas plantadas.** [nao verificado]

| Motivo | Competência | delta=0 | delta=1 | delta≥2 | Total | Evidência |
|---|---|---:|---:|---:|---:|---|
| orfao_14h | 10/2026 | 213 | 82 | 0* | 295 | [nao verificado] |
| orfao_14h | 11/2026 | 56 | 15 | 0* | 71 | [nao verificado] |
| saida_sem_entrada | 10/2026 | 241 | 0* | 0* | 241 | [nao verificado] |
| saida_sem_entrada | 11/2026 | 56 | 1 | 0* | 57 | [nao verificado] |

`0*` significa nenhuma linha correspondente no bloco EFEITO fornecido. Portanto, **nenhuma `orfao_14h` com `delta_civil≥2` aparece nessa saída**, em nenhuma das duas competências; o número fornecido não demonstra saída plantada um dia além. [nao verificado]

**(d) VIVAS em `D_e_dia_da_saida`: 81**, distribuídas abaixo. São as classificadas pela sonda no recorte indicado para mudança de leitura. [nao verificado]

| Motivo | Estado | 10/2026 | 11/2026 | Total | Evidência |
|---|---|---:|---:|---:|---|
| orfao_14h | aberta | 25 | 22 | 47 | [nao verificado] |
| orfao_14h | respondida | 9 | 4 | 13 | [nao verificado] |
| saida_sem_entrada | aberta | 5 | 2 | 7 | [nao verificado] |
| saida_sem_entrada | respondida | 13 | 1 | 14 | [nao verificado] |
| **Total** | **aberta/respondida** | **52** | **29** | **81** | [nao verificado] |

**(e) ERROS:** `{}`, isto é, nenhum erro reportado; `rc_sombra=0`. A saída não aciona a condição “ERROS>0 = sem conclusão”. [nao verificado]

Sobre **T, turno incompleto em `{D−1,D}`**: esse predicado, sozinho, **não garante associação ao episódio correto**. O pareador preserva o turno anterior incompleto quando chega outra entrada; a emissão, por sua vez, agrupa o episódio pela identidade da batida-origem. Assim, presença de turno incompleto na vizinhança não estabelece que ele seja a origem daquela pergunta. [turnos.py:1415](/home/ronald/codex_ws/40a0054572dd35f01266dac318e2ddc50eb6edaa/app/ponto/turnos.py:1415), [disputa_emissao.py:1021](/home/ronald/codex_ws/40a0054572dd35f01266dac318e2ddc50eb6edaa/app/chamados/services/disputa_emissao.py:1021)

O exemplo de dois plantões consecutivos de **12x36** não demonstra, por si, dois candidatos em `{D−1,D}`: o ciclo é descrito como um dia de trabalho e outro de folga. A ocorrência de classificação errada nesse exemplo, neste dump, permanece [nao verificado]. [models.py:37](/home/ronald/codex_ws/40a0054572dd35f01266dac318e2ddc50eb6edaa/app/escala/models.py:37)

A divergência de contrato está no código: o emissor resolve `data_evento` pelo **dia da saída**, enquanto `_turno_tem_saida` procura `data_turno == data_evento`. O plantio também usa essa data como base e o ajuste overnight testa `D` e `D+1`. [servico_jornada.py:483](/home/ronald/codex_ws/40a0054572dd35f01266dac318e2ddc50eb6edaa/app/escala/servico_jornada.py:483), [juizes.py:362](/home/ronald/codex_ws/40a0054572dd35f01266dac318e2ddc50eb6edaa/app/chamados/juizes.py:362), [materializacao.py:414](/home/ronald/codex_ws/40a0054572dd35f01266dac318e2ddc50eb6edaa/app/chamados/services/materializacao.py:414), [juizes.py:302](/home/ronald/codex_ws/40a0054572dd35f01266dac318e2ddc50eb6edaa/app/chamados/juizes.py:302)

Não localizei o corpo de `sonda_frota.py` na cópia. Seu desempate de T, a correspondência com o hash `a38dd1d8efe5a829` e quantas das 81 classificações estariam erradas permanecem **[nao verificado]**.

## 06/11 18:57 · sessao b72bad75e38d · - · - · origem=Ronald (tmux)

- O emissor `resolver_data_evento` retorna o dia da **saída esperada** em `orfao_14h` e o dia da **saída órfã** em `saida_sem_entrada`. [app/escala/servico_jornada.py:483](/home/ronald/codex_ws/40a0054572dd35f01266dac318e2ddc50eb6edaa/app/escala/servico_jornada.py:483)
- O detector também grava `contexto['orfao_14h']['data']` com a data da saída esperada. [app/chamados/detector_anomalias.py:45](/home/ronald/codex_ws/40a0054572dd35f01266dac318e2ddc50eb6edaa/app/chamados/detector_anomalias.py:45)
- Essa saída esperada é o último marco encadeado: o encadeamento avança um dia quando o horário seguinte cruza a meia-noite e a função retorna `fim.date()`. [app/escala/utils.py:936](/home/ronald/codex_ws/40a0054572dd35f01266dac318e2ddc50eb6edaa/app/escala/utils.py:936), [app/escala/utils.py:968](/home/ronald/codex_ws/40a0054572dd35f01266dac318e2ddc50eb6edaa/app/escala/utils.py:968)
- O plantio usa `p.data_evento` como data-base, combina nela a hora declarada e chama o ajuste overnight. [app/chamados/services/materializacao.py:344](/home/ronald/codex_ws/40a0054572dd35f01266dac318e2ddc50eb6edaa/app/chamados/services/materializacao.py:344), [app/chamados/services/materializacao.py:413](/home/ronald/codex_ws/40a0054572dd35f01266dac318e2ddc50eb6edaa/app/chamados/services/materializacao.py:413)
- O ajuste busca a escala vigente nessa data e testa candidatos em `data_evento` e `data_evento+1` contra o envelope dos marcos. [app/chamados/juizes.py:302](/home/ronald/codex_ws/40a0054572dd35f01266dac318e2ddc50eb6edaa/app/chamados/juizes.py:302)
- O leitor de completude `_turno_tem_saida` consulta exclusivamente o turno com `t.data_turno == p.data_evento`. Portanto, o código contém duas leituras distintas da data: saída no emissor e turno nesse leitor. [app/chamados/juizes.py:362](/home/ronald/codex_ws/40a0054572dd35f01266dac318e2ddc50eb6edaa/app/chamados/juizes.py:362)
- O juiz de batida plantada aceita a hora do slot nas datas `data_evento−1`, `data_evento` e `data_evento+1`; logo, esse julgamento não demonstra sozinho que o dia civil está correto. [app/chamados/juizes.py:263](/home/ronald/codex_ws/40a0054572dd35f01266dac318e2ddc50eb6edaa/app/chamados/juizes.py:263)
- Não localizei `sonda_frota.py` na cópia; só os wrappers `bin/sonda_frota.sh` e `bin/sonda_leitura.sh`. Correspondência do hash `a38dd1d8efe5a829` com algum código executável presente: [nao verificado].
- Atribuir os números raspados a um desses caminhos específicos de emissão/plantio: [nao verificado].

## 06/11 18:57 · sessao 03ab1999c4a9 · - · - · origem=Ronald (tmux)

- O código identifica o episódio pela **batida-origem**, agrupando chamados por `origem.pk`; portanto, apenas achar algum turno incompleto em `{D-1,D}` não demonstra que ele pertence à pergunta examinada. A associação usada pela sonda permanece [nao verificado], pois seu `.py` não foi localizado na cópia. Evidência do vínculo real: `app/chamados/services/disputa_emissao.py:1021-1048`.

- O pareador preserva turnos incompletos anteriores quando chega outra entrada: emite o turno anterior e abre o seguinte. Logo, “há turno incompleto na vizinhança” não estabelece sozinho uma origem única. `app/ponto/turnos.py:1415-1418`. O retorno contém os turnos encontrados e é ordenado por `data_turno`, sem deduplicação nessa etapa. `app/ponto/turnos.py:1990-1995`.

- O exemplo “dois plantões consecutivos de 12x36” **não demonstra sozinho dois candidatos em dois dias civis consecutivos**: o modelo descreve 12x36 como “um dia trabalha, um dia folga”. `app/escala/models.py:37`. Existência desses casos e quantidade de classificações erradas no dump: [nao verificado].

- A divergência de contrato é comprovável: `resolver_data_evento` devolve a data da saída esperada para `orfao_14h`. `app/escala/servico_jornada.py:483-491`. O materializador usa essa data para compor a resposta e passa ao ajuste overnight. `app/chamados/services/materializacao.py:344-347`, `app/chamados/services/materializacao.py:414-420`. O ajuste ancora os marcos em `data_evento` e testa as datas `D` e `D+1`. `app/chamados/juizes.py:302-313`. Isso prova o mecanismo no código; incidência efetiva de saída plantada um dia além deve vir dos números `delta_civil`, não apenas da classe da pergunta. [nao verificado]

## 06/11 18:56 · sessao 9d4d73a5e5aa · BO-[nome]-0611 · sonda · origem=Code

Saída fornecida, sem reexecução da sonda. [nao verificado]

- **(a)** `TOTAL_RECUSAS 0 COLABS 0`: zero recusas, zero colaboradores. O recorte desde 07/10 não aparece na saída. [nao verificado]
- **(b)** `RECUSAS_SEM_BATIDA_EM_30MIN 0 COLABS 0`: zero candidatas a batida perdida, zero colaboradores. [nao verificado]
- **(c)** Não há série diária apresentada; existência de degrau e sua data: [nao verificado].
- **(d)** Não aparece linha de `col155`. [nao verificado]
- **(e)** `ERROS {}`: nenhum erro listado. [nao verificado]

**O caminho que escreve o log não é o único bloqueio por falta de GPS.** Na tela V2, coordenadas ausentes fazem `garantirRequisitos()` retornar `false`; o chamador encerra antes do envio à API. [app/templates/ponto/bater_ponto_v2.html:297](app/templates/ponto/bater_ponto_v2.html#L297), [app/templates/ponto/bater_ponto_v2.html:325](app/templates/ponto/bater_ponto_v2.html#L325).

Nas duas views, `if not latitude or not longitude` chama `registrar_log` com `batida_recusada_sem_gps` e retorna HTTP 403, código `gps_obrigatorio`. [app/api/views_core.py:440](app/api/views_core.py#L440), [app/api/views.py:449](app/api/views.py#L449). Se a auditoria lançar exceção, `registrar_log` captura e registra a falha; a view segue para o 403. [app/core/auditoria.py:25](app/core/auditoria.py#L25), [app/api/views_core.py:454](app/api/views_core.py#L454), [app/api/views.py:462](app/api/views.py#L462).

**A fila offline mantém a batida após 403 e reenvia na próxima sincronização.** Remove o item somente em resposta de sucesso ou HTTP 400; o 403 não executa essas exclusões. A sincronização lê novamente todos os pendentes e envia cada um. [app/static/pwa.js:130](app/static/pwa.js#L130), [app/static/pwa.js:153](app/static/pwa.js#L153), [app/static/pwa.js:158](app/static/pwa.js#L158), [app/static/pwa.js:166](app/static/pwa.js#L166). Há chamadas de sincronização no evento `online` e na inicialização, quando conectado. [app/static/pwa.js:320](app/static/pwa.js#L320), [app/static/pwa.js:328](app/static/pwa.js#L328).

## 06/11 18:54 · sessao 6c1044e8ea77 · BO-[nome]-0611 · sonda · origem=Code

**BO-[nome]-0611 — triagem somente leitura.** Os fatos das FICHAS/anexo estão identificados com **[nao verificado]**, pois foram fornecidos no texto, sem arquivo:linha acessível. As referências abaixo sustentam o comportamento da cópia; a separação de donos segue `CLAUDE.md:176`.

**01+04 — col39, 15/10: producao batida, dono BATIDA.** É o candidato de maior atividade, 20 contra 11. O atestado **virou Ausência 4879 e está aprovado** desde 16/10 16:36:56; há três marcações em 15/10, sem saída final, e a retenção informada é **`espelho_fato_em_ausencia`**, não “atestado que não chegou”. (FICHA 01+04 [nao verificado].)

O código retém fato em dia coberto por ausência; portanto, o conflito informado explica a retenção. A porta do documento é **Ausências**; o calendário também oferece **Resolver dia**, mas não se deve presumir que “Trabalhou” aceitará um dia já coberto por ausência. (`app/escala/models.py:1753`; `app/folha/export.py:1233`; `app/ponto/services/ausencia.py:637`; `app/ponto/services/veredito_celula.py:221`.)

**02 — col993 comparado com col1023: falta informacao.** Mantive col993, atividade 102 contra 91 e 19: a FICHA mostra várias correções materializadas em 03/11, com origem `disputa_s84_retro`. O que aparece parado é a volta de intervalo de **24/10, pergunta 53441**, ainda sem resposta, além das perguntas de 03/11 e 05/11; a Justificativa 3465 também está pendente, sem saída informada. (FICHAS 02a/02b [nao verificado].)

Os caminhos são diferentes:

- **Justificativa:** aprovar só gera saída se houver `saida_informada` e `batida_entrada`; pode ficar aprovada sem gerar batida. (`app/ponto/services/justificativa.py:137`; `app/ponto/services/justificativa.py:190`.)
- **RegularizacaoExterna:** exige confirmação do colaborador e produz origem `reg_externa`; ambas as FICHAS têm `regularizacoes=[]`. (`app/chamados/services/regularizacao_ext.py:36`; `app/chamados/services/regularizacao_ext.py:76`; FICHAS [nao verificado].)
- **PerguntaDisputa:** exige resposta para validar e materializar. Em intervalo fixo, o plantio pode usar a hora do marco, não a hora livre digitada. (`app/chamados/services/validacao.py:363`; `app/chamados/services/materializacao.py:404`.)

Col1023 mostra abonos em 03–04/11, uma batida retratada e marcações do app em 05–06/11; não identifica a suposta correção de horário que “chegou”. Falta o dia/hora ou ID da solicitação comparada para concluir onde **a correção alegada** parou. (FICHA 02b [nao verificado].)

**03 — lote: nosso com obra, dono ESTRUTURA; cobertura específica: sem obra.** A view executa o lote **sincronamente**, antes de devolver o resultado; o serviço percorre e valida as referências individualmente. Isso mantém trabalho longo na requisição, contrariando a orientação de job da infraestrutura. Enquadramento de padrão: **Event-driven architecture**, aplicado à retirada desse processamento da pilha HTTP. (`app/chamados/views_cobrar.py:379`; `app/chamados/services/validacao.py:680`; `CLAUDE.md:89`; `app/docs/GLOSSARIO.md:16`.)

O **Caddyfile não configura timeout específico para essa rota**. Os 30 segundos do `gthread` não constituem limite por requisição: a própria documentação da O284 distingue requisição longa de worker travado. Não há fundamento para afirmar “passou de 30 s, o usuário recebe erro”. (`Caddyfile:8`; `Caddyfile:10`; `app/gunicorn.conf.py:19`; `app/gunicorn.conf.py:21`; `app/docs/TICKETS.md:132`.)

O formulário HTMX não declara timeout próprio; o resultado aparece quando a resposta chega. Em erro HTTP ou de transporte há mensagens diferentes, mas **não foi identificado um X segundos para desistência da tela**. (`app/templates/chamados/partials/_lote_preview.html:21`; `app/templates/chamados/partials/_lote_preview.html:5`; `app/templates/core/_dialogo.html:61`; `app/templates/core/_dialogo.html:75`.)

Os 35 retornos anexados são 200, entre 10,435 e 122,373 s; **200 não prova validação integral**, pois o resultado também comporta recusadas e validadas sem gravação. A causa da subida a partir de 03/11 permanece desconhecida: faltam tamanho/referências dos lotes e perfil da execução. (Anexo `ui_lote.log` [nao verificado]; `app/templates/chamados/partials/_lote_preview.html:7`; `app/templates/chamados/partials/_lote_preview.html:9`.)

Não localizei obra específica que cubra esse endpoint. **O284 cobre PDF do espelho**, não validar chamados; atribuir sua cobertura a esta rota seria indevido. (`app/docs/TICKETS.md:132`; cobertura específica [nao verificado].)

**05 — col155: nosso com obra, dono ESTRUTURA; sem obra específica localizada.** É o maior candidato, atividade 23 contra 15; tem dois devices ativos e as marcações apresentadas chegaram com geolocalização. O anexo não identifica uma tentativa recusada nem sua versão efetiva do aplicativo. (FICHA 05 [nao verificado].)

**Sim, há bloqueio:** o helper do PWA retorna `null` quando não consegue GPS; a tela V2 interrompe o envio sem coordenadas; a API efetiva retorna **403 `gps_obrigatorio` antes do registro**, inclusive para envio pela fila offline. Isso contraria a regra interna de batida nunca barrada. (`app/static/pwa.js:289`; `app/templates/ponto/bater_ponto_v2.html:297`; `app/templates/ponto/bater_ponto_v2.html:325`; `app/static/pwa.js:153`; `app/api/views_core.py:440`; `CLAUDE.md:140`.)

Padrão quebrado: **Duplicated business logic / logic drift** — a tela antiga permite prosseguir sem GPS, enquanto V2/API barram. O bloqueio está provado no código; **uma batida individual perdida de col155 não está demonstrada**. (`app/docs/GLOSSARIO.md:23`; `app/templates/ponto/bater_ponto.html:507`; `app/api/views_core.py:454`; perda individual [nao verificado].)

**06 — col950: producao cadastro, dono CADASTRO.** O backlog o identifica expressamente como conta de teste; TESTE06 mostra devices iOS, seis batidas, **em comuns=true e em operação=false**. (`app/docs/BACKLOG.md:1797`; TESTE06 [nao verificado].)

“Comuns” exige situação ativa, empresa ativa e username de 11 dígitos; “operação” acrescenta rollout e vínculo vigente. **`situacao='desligado'` o retiraria dos dois universos**; `isento_ponto` não participa desses filtros. Qual condição adicional o exclui hoje da operação não aparece no anexo. (`app/colaboradores/queries.py:25`; `app/colaboradores/queries.py:65`; motivo específico [nao verificado].)

**07 — col836, 14/10: ja OK hoje quanto à exclusão da lista inicial.** O chamado **31151 existe e está resolvido via sistema desde 14/10 08:15:08**; não tem `superado_por` nem silenciamento, possui célula e pertence à empresa 2. (FICHA 07 [nao verificado].)

A lista pertinente é **Painel de gestão, `/chamados/`**: por padrão mostra cobrança, aberto/em análise; **Histórico** inclui resolvido/fechado. Esse status basta para excluir o registro da lista inicial; filtros efetivamente escolhidos e acesso concreto do usuário não foram fornecidos. (`app/chamados/urls.py:9`; `app/chamados/views.py:721`; `app/chamados/services/painel.py:298`; `app/chamados/services/painel.py:314`; filtros/acesso [nao verificado].)

O queryset citado não exclui automaticamente por célula, `superado_por` ou “empresa visível”; empresa, praça e colaborador entram por filtros explícitos. A pendência de escala indicada na célula é separada da visibilidade desse chamado resolvido. (`app/chamados/services/painel.py:96`; FICHA 07 [nao verificado].)

**08 — col617, 05–06/11: ja OK hoje quanto à alegação de autoaceite.** A ausência 5380 está rejeitada; a **5399 está aprovada**, cobrindo 05–06/11. Ambas foram registradas pelo colaborador, mas `aprov_proprio=false`: o dado não sustenta aprovação pelo próprio colaborador; tampouco mostra a data correta do documento ou a identidade do decisor. (FICHA 08 [nao verificado].)

Atestado com documento nasce **aguardando decisão**. A aprovação automática da declaração **`FALTA_DIA` não se estende ao atestado**: `ATESTADO` usa o estado inicial do serviço. A auditoria da tela distingue “Lançado por” de “Decidido por”. (`app/ponto/services/ausencia.py:779`; `app/chamados/services/materializacao.py:579`; `app/templates/ponto/partials/painel_ausencia.html:257`.)

**09 — col1000: ja OK hoje para os dias sem batida apresentados.** Em **26/10** há abono aprovado; em **31/10** o dia é folga e há decisões `admin_decidiu` registradas em 06/11. Isso explica dias sem marcação que não permanecem como falta pendente; o anexo não identifica o usuário que decidiu. (FICHA 09 [nao verificado].)

“Resolvido” pode ser o **status do chamado**, não uma afirmação de jornada completa; chamado filho agrupado também pode estar resolvido enquanto outra pendência continua. O calendário separa o vínculo do chamado da marca de decisão humana. (`app/chamados/models.py:772`; `app/templates/colaboradores/partials/_calendario_grade.html:108`; `app/templates/colaboradores/partials/_calendario_grade.html:109`.)

A alegação não descreve integralmente o estado atual: **27/10 tem perguntas pendentes; 29 e 30/10 têm chamados próprios em análise**, criados hoje. A tela exata originalmente chamada de “resolvido” permanece sem identificação. (FICHA 09 [nao verificado].)

**10 — col399: falta informacao para a cronologia da alteração alegada.** As batidas de 31/10 foram criadas às **06:20:44 e 18:22:42**; a ausência de troca de 24/10 foi criada em **04/11 16:13:01**. Hoje a célula de 31/10 informa `folga_trabalhada`, sem retenção; a de 24/10 informa ausência. (FICHA 10 [nao verificado].)

Esses registros antecedem o deploy O10-B documentado em **06/11 13:20**. Porém a FICHA não contém `editada_em`, `regenerada_em`, antes/depois nem trilha de mudança de tipo: **não permite datar quando as células/batidas teriam mudado ou virado espúrias**. (`app/docs/RELATO.md:17`; cronologia da alteração [nao verificado].)

O `git log main` solicitado não pôde ser consultado porque a cópia não tem repositório Git disponível. A O10-B existente escreve o par da troca pela porta da célula, mas seu código não prova aplicação ao caso. (`app/ponto/portas/celula.py:632`; histórico Git/aplicação [nao verificado].)

**11 — col921: ja OK hoje; regra de competência.** `resolver_dia=true` em **14, 15, 16, 18, 19 e 20/10**; falso nas folgas **17 e 24/10**, e também nos dias **21–23 e 25–28/10**, ainda irregulares. (FICHA 11+12 [nao verificado].)

O botão exige passado, status que pede decisão e **último recurso em competência já encerrada**; dentro da corrente, o caminho é chamado/pergunta. A mudança em 21/10 é compatível com a competência 21–20 apresentada na FICHA, não demonstra defeito. (`app/colaboradores/services/calendario.py:97`; `app/colaboradores/services/calendario.py:455`; `app/colaboradores/services/calendario.py:738`; `app/ponto/janelas.py:10`.)

**12 — col921: ja OK hoje quanto à recusa de falta em folga.** O veto recusa falta quando a autoridade da escala diz que não existe jornada prevista. As folgas mostradas são 17 e 24/10; as seis faltas aprovadas apresentadas recaem em dias de trabalho. (`app/ponto/precedencia.py:536`; FICHA 11+12 [nao verificado].)

**Não há lançamento específico de “abandono de posto em folga” demonstrado neste caso.** O comando de abandono provisório usa o próprio tipo `falta`; o modelo de Advertência tem “Ausência/Outro”, mas isso não comprova uma porta de tela para o lançamento pretendido nem autoriza tratar folga como falta. (`app/ponto/management/commands/marcar_abandono_provisorio.py:76`; `app/ponto/models.py:1164`; porta pretendida [nao verificado].)

**13+14 — col407: nosso com obra, dono ESTRUTURA.** Aqui há evidência contrária ao maior candidato: col114 apresenta caso diurno e perguntas de intervalo; **col407 corresponde à assinatura noturna relatada**. A pergunta **47210**, vinculada a 22/10, respondeu 07:00 em 22/10 e foi validada hoje às 14:45:08; gerou a batida **147824, saída em 23/10 07:00, origem `disputa_s84_retro`**, enquanto 21/10 continua com furo. (FICHAS 13+14 [nao verificado].)

Isso confirma a assinatura do aval informado: data da pergunta tratada como base do turno, com saída deslocada mais um dia. O plantio combina `data_evento` com a hora e passa pelo ajuste noturno; o backlog reconhece expressamente as semânticas diferentes desse campo. (`app/chamados/services/materializacao.py:344`; `app/chamados/services/materializacao.py:414`; `app/chamados/juizes.py:310`; `app/docs/BACKLOG.md:990`.)

Padrão: **Single Writer Principle**. A obra existente que cobre é **DIA-DA-PERGUNTA-E-O-TURNO, complemento O261**; o identificador literal `BUG-PLANTIO-NOTURNO-MAIS-1` veio no pedido, sem correspondência localizada nos dois registros pesquisados. (`app/docs/GLOSSARIO.md:12`; `app/docs/BACKLOG.md:990`; correspondência literal [nao verificado].)

A batida anômala veio da resposta validada, **não do aparelho**. `timestamp_dispositivo` e `sincronizado_em` são campos anuláveis; seus nulos não provam perda, e o anexo não traz uma tentativa local ausente do servidor nem valores monetários para demonstrar dinheiro errado. (`app/ponto/models.py:75`; `app/ponto/models.py:77`; FICHA/perda/dinheiro [nao verificado].)

**ERROS da sonda:** veio `ERROS {}`; nenhum item ficou inconclusivo por erro declarado da sonda. As lacunas centrais de **02 e 10**, a causa da lentidão de 03 e as identidades não expostas permanecem sem conclusão por falta de evidência. (Anexo [nao verificado].)

**(1) Tabela consolidada.** Datas/colaboradores abaixo vêm do anexo [nao verificado]; os enquadramentos remetem às evidências acima. “Não provado” em SOBE significa ausência de prova de dinheiro errado ou batida perdida [nao verificado].

| item | colNNN + dia | classe | padrão quebrado | obra | SOBE? |
|---|---|---|---|---|---|
| 01 | col39 · 15/10 | producao batida | n/a — BATIDA | n/a | Não provado |
| 02 | col993 × col1023 · pedido sem dia identificado | falta informacao | não determinado | não determinada | Não provado |
| 03 | col não informado · 28/10–06/11 | nosso com obra | Event-driven architecture | **sem obra** específica localizada | Não provado |
| 04 | col39 · 15/10 | producao batida | n/a — BATIDA | n/a | Não provado |
| 05 | col155 · 28/10–06/11 | nosso com obra | Duplicated business logic / logic drift | **sem obra** específica localizada | Não provado |
| 06 | col950 · TESTE06 | producao cadastro | n/a — CADASTRO | n/a | Não provado |
| 07 | col836 · 14/10 | ja OK hoje | n/a | n/a | Não provado |
| 08 | col617 · 05–06/11 | ja OK hoje | n/a | n/a | Não provado |
| 09 | col1000 · 26 e 31/10 sem batida | ja OK hoje | n/a | n/a | Não provado |
| 10 | col399 · 24 e 31/10 | falta informacao | não determinado | O10-B relacionada; aplicação não provada | Não provado |
| 11 | col921 · 14–28/10 | ja OK hoje | n/a | n/a | Não provado |
| 12 | col921 · folgas 17 e 24/10 | ja OK hoje | n/a | n/a | Não provado |
| 13 | col407 · turno 21/10 / saída plantada 23/10 | nosso com obra | Single Writer Principle | DIA-DA-PERGUNTA-E-O-TURNO / O261 | Não provado |
| 14 | col407 · pergunta 47210 de 22/10 | nosso com obra | Single Writer Principle | DIA-DA-PERGUNTA-E-O-TURNO / O261 | Não provado |

**(2) Contagem:** ja OK hoje **5**; nosso com obra **4**; producao batida **2**; producao cadastro **1**; falta informacao **2**. **SOMA = 14**, contando 01/04 e 13/14 separadamente, conforme as linhas da tabela.

**(3) Textos para o DP**

**01.** Abra o atestado em **Ausências** e confira a data e o período junto das marcações de 15/10. O documento já consta aprovado, mas existem marcações na mesma data. (`app/templates/ponto/partials/painel_ausencia.html:130`; FICHA [nao verificado].)

**02.** Abra o chamado da correção e confira a data da pergunta e se ela já recebeu resposta. Há horários registrados e perguntas ainda sem resposta; a aprovação de justificativa e a confirmação de lote são etapas diferentes. (`app/chamados/services/validacao.py:363`; `app/chamados/services/regularizacao_ext.py:36`; FICHAS [nao verificado].)

**03.** Em **Validar em lote**, aguarde o quadro de resultado. Confira nele quantas perguntas foram validadas, quais ficaram de fora e quais foram validadas sem gravar o ponto. (`app/templates/chamados/partials/_lote_preview.html:5`.)

**04.** No calendário de 15/10, consulte **Resolver dia** e compare as marcações com o atestado em Ausências. A pendência indicada é a existência de marcações em um dia já coberto pela ausência. (`app/templates/colaboradores/partials/_calendario_grade.html:125`; FICHA [nao verificado].)

**05.** Confira se a tentativa terminou com confirmação de registro. Quando aparece a mensagem de GPS obrigatório, aquela tentativa não é registrada; as marcações apresentadas neste levantamento chegaram com localização. (`app/api/views_core.py:454`; FICHA [nao verificado].)

**06.** Consulte a situação cadastral da conta de teste. Ela ainda aparece como ativa e entra nos colaboradores comuns, embora já esteja fora do universo de operação. (`app/colaboradores/queries.py:25`; TESTE06 [nao verificado].)

**07.** No painel **Chamados**, selecione **Histórico** para localizar o registro de 14/10. A lista inicial mostra os chamados que aguardam ação, e esse registro consta como resolvido. (`app/chamados/services/painel.py:298`; FICHA [nao verificado].)

**08.** Abra o segundo atestado em **Ausências** e confira o período 05–06/11 e a guia **Auditoria**. O registro está aprovado, e essa guia distingue quem lançou de quem decidiu. (`app/templates/ponto/partials/painel_ausencia.html:257`; FICHA [nao verificado].)

**09.** Confira separadamente os dias no calendário e abra o chamado pelo número exibido. Em 26/10 há abono e em 31/10 há folga; 29 e 30/10 têm chamados próprios em análise. (`app/templates/colaboradores/partials/_calendario_grade.html:108`; FICHA [nao verificado].)

**10.** No calendário, compare 24/10 e 31/10 com o pedido de troca. O levantamento mostra ausência no primeiro dia e folga trabalhada no segundo, mas não informa quando essa classificação mudou. (`app/colaboradores/services/calendario.py:662`; FICHA [nao verificado].)

**11.** Para os dias depois de 20/10, abra o chamado e acompanhe as perguntas. **Resolver dia** fica disponível como último recurso para competência já encerrada. (`app/colaboradores/services/calendario.py:455`.)

**12.** Confira no calendário se o dia está previsto como trabalho ou folga. **Falta** só aceita dia com jornada prevista; o levantamento não identifica um lançamento específico de abandono para esses dias de folga. (`app/ponto/precedencia.py:536`; lançamento específico [nao verificado].)

**13.** Confira no espelho o turno iniciado em 21/10 e a saída da manhã seguinte. A marca das 07:00 exibida em 23/10 veio da resposta validada no chamado, não de uma nova marcação do aparelho. (`app/chamados/services/materializacao.py:414`; FICHA [nao verificado].)

**14.** Abra o chamado em análise e confira a data indicada junto da pergunta respondida. A resposta consta validada, enquanto o dia 21/10 continua irregular no levantamento. (`app/chamados/services/fio.py:40`; FICHA [nao verificado].)

_2431 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
