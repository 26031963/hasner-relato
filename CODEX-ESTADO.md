# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 20:40** · publicado em **13/11/2026 20:43** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 20:40 · sessao 9027c836a629 · - · - · origem=Ronald (tmux)

O anexo contém **13 blocos FAIL**, embora informe 12. Classificação pela cópia:

| Selo | Classe | Mudança recomendada |
|---|---|---|
| bloqueantes `RED_8...` | I | Exigir `porta_fila` e ausência no contador sem canal; manter controles silenciada/registrada. `chamados/tests/test_e3_bloqueantes.py:148` |
| diff `AB_categorizado...` | I | Agora C17 e candidata conservam A+B: esperar `campos={}` e `iguais=1`, mantendo universo não vazio. `chamados/tests/test_e3_diff_sombra.py:163` |
| escala `contraex1...` | I | Exigir porta da tarefa e saída do beco; conservar contraexemplo fora da fila. `escala/tests/test_f3_beco1_folga_na_lista.py:166` |
| destino registrado | II | Preservar asserts; restringir donos das ações às portas publicadas. `chamados/tests/test_o261e_f5b_juiz_porta.py:255`, `chamados/services/tarefa_do_dia.py:578` |
| destino silenciado | II | Mesmo defeito e correção. `chamados/tests/test_o261e_f5b_juiz_porta.py:251` |
| F5B `so_a_fila...` | I | Porta e fio `chamado`; preservar nenhuma pergunta. `chamados/tests/test_o261e_f5b_juiz_porta.py:159` |
| alcance `mesmo_dia...` | I | Esperar zero mortos, uma porta de fila e soma do universo preservada. `chamados/tests/test_o261e_f5_alcance.py:152` |
| alcance `papel_SEGUE...` | I | Expectativa inicial `porta_fila/chamado`; manter patch e expectativa `fio-xyz`. `chamados/tests/test_o261e_f5_alcance.py:366` |
| alcance `teto_de_hoje` | I | Esperar `(1,0)` com porta; acrescentar controle sem chamado `(0,1)`. Produção já consulta porta antes do teto. `chamados/tests/test_o261e_f5_alcance.py:466`, `chamados/services/vigia_fio.py:648` |
| O66 vivo | I | Esperar ausência da amostra sem canal; manter terminal incluído. `chamados/tests/test_o66_furo_sem_canal.py:105` |
| fachada candidata | III | Comparar quatro campos protegidos e afirmar `nao_sei` separadamente em True/False. `chamados/tests/test_e3_diff_sombra.py:131` |
| alvos dos mutantes | II, infraestrutura | Atualizar três literais obsoletos e registrar os 15 v6 na catraca; III seria imprópria se restrita ao novo campo. `chamados/tests/e3_mutantes.py:89`, `chamados/tests/test_e3_mutantes_alvo.py:16` |

A prova do teto deve matar três mutantes diferentes: omissão do residual, teto aplicado antes da porta, remoção do teto no controle sem porta. A ordem original porta→teto consta em `chamados/services/vigia_fio.py:648` e `chamados/services/vigia_fio.py:656`.

O teste v6 que hoje exige `admin` para ação com `porta=None` e residual Decidir precisa passar a exigir `chamado`; seu mutante deve voltar a incluir donos de todas as ações. `chamados/tests/test_e3_portas_v6_sem_banco.py:44`, `chamados/tests/e3_portas_v6_mutantes.py:67`.

Nenhum arquivo alterado. Execução Django: [nao verificado].

## 13/11 20:38 · sessao 134b78c397e0 · HJ-090 · construtor · origem=Code

Cura escrita nas três portas, com `hoje` obrigatório e propagado ao status; a resposta HTMX reutiliza o mesmo dia ([entrega:28](/home/ronald/wt-codex-hj-090-152256/app/HJ090_VOLTA6_ENTREGA.md:28)).

**RED/GREEN e mordida dos mutantes: [nao verificado].** Todas as tentativas falharam antes da coleta por falta de Django; `Ran` não foi emitido ([execuções:1](/home/ronald/wt-codex-hj-090-152256/app/HJ090_VOLTA6_EXECUCOES.txt:1)). Compilação estática e `git diff --check` passaram ([execuções:131](/home/ronald/wt-codex-hj-090-152256/app/HJ090_VOLTA6_EXECUCOES.txt:131)).

Para revisão:

- [Censo dos 16 sítios, classificação e chamadores](/home/ronald/wt-codex-hj-090-152256/app/HJ090_VOLTA6_ENTREGA.md:7).
- [Testes por valor com `freeze_time`](/home/ronald/wt-codex-hj-090-152256/app/ferias/test_hj090_volta6.py:24). [nao verificado]
- [Quatro mutantes em memória](/home/ronald/wt-codex-hj-090-152256/app/ferias/hj090_volta6_mutantes.py:18) e [comandos individuais](/home/ronald/wt-codex-hj-090-152256/app/HJ090_VOLTA6_ENTREGA.md:55).
- [Quem escreve e quem lê cada campo de data/status](/home/ronald/wt-codex-hj-090-152256/app/HJ090_VOLTA6_ENTREGA.md:70).

Fila conservada, com linhas finais e motivos individuais: `services.py:64`, `:77`, `:224`, `:344`, `:597`, `:624`, `:629`, `:698`, `:720`, `:727`, `:739`, `:846`, `:869`. São funções e chamadores diretos não tocados no intervalo; na sincronização indireta, o caminho já entrega `hoje` ([fila detalhada:84](/home/ronald/wt-codex-hj-090-152256/app/HJ090_VOLTA6_ENTREGA.md:84)).

## 13/11 20:38 · sessao b71b24ce67b8 · GRADE-MESTRA · leitura · origem=Code

=== LINHAS ===
| id | balde | entrega (1 frase de leigo) | resolve (HJ-NNN, caso NN ou -) | depende de | grade | estado | fonte (BACKLOG:linha) |
| CRONS-DURACAO-CRESCERAM | INSTRUMENTO | Evita sobreposição das tarefas agendadas. | - | investigar duração do auditor | ......... | REGISTRADA | BACKLOG:1763 |
| R1-ESTRUTURA-91 | ESPINHA-E | Corrige famílias de divergência entre cálculo e regra. | - | E4; E5; motor; classes cegas | .+......+ | REGISTRADA (restante pendente) | BACKLOG:1765 |
| CARIMBO-DA-CONVENCAO | UI | Mostra qual convenção rege cada trecho calculado. | - | regra usada em cada fatia | .+....... | REGISTRADA | BACKLOG:1766 |
| REGUA-VOLTA-VIGILANTES | LATERAL-ESTRUTURA | Reaplica a convenção indicada com motivo e histórico. | - | cura da porta; deploy | .+~...... | REGISTRADA | BACKLOG:1767 | [nao verificado]
| AUDITOR-ESPELHO | INSTRUMENTO | Confere as contas dos espelhos e separa anomalias. | - | CARIMBO-DA-CONVENCAO; cartão do DP | .+....... | REGISTRADA | BACKLOG:1768 |
| RBAC-ESCRITA-POR-PAPEL | LATERAL-ESTRUTURA | Restringe alterações ao papel autorizado e registra o operador. | - | ESMERIL-V2; UIs | .+....... | REGISTRADA | BACKLOG:1769 |
| V-FASE-FILA-PENDENTE-912 | INSTRUMENTO | Retira uma referência antiga de autoridade. | - | O141; L-118 | ......... | REGISTRADA | BACKLOG:1770 |
| V-FASE-FILA-DONO-DEPENDE-DA-JANELA | ESPINHA-E | Impede mudar o responsável pelo erro passado conforme a janela. | - | decisão de lei | ......... | PARADA (aguarda lei) | BACKLOG:1771 |
| V-FASE-FILA-FIO-ADMIN-SEM-PORTA | LATERAL-ESTRUTURA | Evita contar uma ação administrativa que não pode ser executada. | - | F5; juiz da porta | ......... | REGISTRADA | BACKLOG:1772 |
| V-FASE-FILA-MIUDOS | LATERAL-ESTRUTURA | Ajusta propostas, atualização e provas do diagnóstico de fase. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1773 |
| V-FASE-DONO-AS-OF | ESPINHA-E | Classifica o dia pelo diagnóstico que efetivamente o cobriu. | - | último diagnóstico; gatilho de mudança | .+~...... | REGISTRADA (construtor antigo) | BACKLOG:1774 |
| V-FASE-FILA-LAVRA-TODAS-AS-COMPS | INSTRUMENTO | Mede o custo de atualizar todas as competências. | - | medição; eventual aval | ......... | REGISTRADA | BACKLOG:1775 |
| HOLERITE-SHARE-WEBVIEW | UI | Permite compartilhar o holerite pelo servidor no aplicativo. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1776 |
| AUDIT-[nome]-0311 | INSTRUMENTO | Organiza dez problemas relatados e suas lacunas de conferência. | - | tradução do item 7; clique do item 10 | ......... | REGISTRADA | BACKLOG:1777 |
| F3-RECORTE-E-CAUDA | ESPINHA-E | Mede perguntas sob chamados encerrados e migra o leitor restante. | - | MODO-COMUM; item 5 | +........ | REGISTRADA (A publicada; B pendente) | BACKLOG:1779 | [nao verificado]
| TOAST-CORRIDA-2O-POST | LATERAL-ESTRUTURA | Evita erro quando dois envios simultâneos repetem o nome. | - | ? (precedência não indicada) | .+....... | REGISTRADA | BACKLOG:1780 |
| NOITE-0411 | ? (ordem com vários baldes) | Ordena entregas de juízes, leitores, otimização e motor. | - | sequência dos itens 1–9 | .+....... | REGISTRADA (ordem substituída) | BACKLOG:1781; BACKLOG:1782 |
| MANHA-0511 | ? (ordem com vários baldes) | Ordena as entregas de fila e infraestrutura. | - | sequência dos itens 1–8 | .+.....++ | REGISTRADA (ordem substituída) | BACKLOG:1782; BACKLOG:1786 |
| ORDEM-POS-E4A2 | ? (ordem com vários baldes) | Define a sequência de correções após E4A2. | - | BOs; E4B; E4C; E4D | .+....... | REGISTRADA (sequência alterada depois) | BACKLOG:1783; BACKLOG:1784 |
| PLANO-JANELA-POS-RESET | ? (ordem com vários baldes) | Define a fila e os limites após o reset. | - | auditoria; sequência registrada | .+....... | REGISTRADA (auditoria reposicionada pelo aval atual) | BACKLOG:1784; anexo:aval.txt:1 |
| VERIFICADOR-EM-FABLE | INSTRUMENTO | Usa o verificador indicado e mede seus resultados. | - | virada do reset; medição de três dias | .++~+.... | REGISTRADA | BACKLOG:1785; BACKLOG:1911 |
| ORDEM-VIVA-0811 | ? (ordem com vários baldes) | Prioriza fio, leitores e cálculo na sequência definida. | - | E1→E2→E3; ATA-X-TURNO-PAUSA | .+.....+. | REGISTRADA | BACKLOG:1786 |
| F3-PORTA-E-CRON-PRAZO | LATERAL-ESTRUTURA | Devolve perguntas ao destino correto e separa tarefas de prazo. | - | F3-BECO-1; vaga; autorização do motor | .+....... | REGISTRADA | BACKLOG:1787 |
| LASTRO-PARTE-2 | LATERAL-ESTRUTURA | Fecha cobranças pelo marco correto e transforma o fechador em vigia. | - | JUIZ-1; CRONS-INSTALL-SOB-MARCA | .+....... | REGISTRADA | BACKLOG:1788 |
| LASTRO-PARTE-2-FILA-OPUS | LATERAL-ESTRUTURA | Corrige diferenças de isenção, sinais e eventos. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1789 |
| LASTRO-PARTE-3 | LATERAL-ESTRUTURA | Publica evento quando a resposta permite fechar o chamado. | - | ? (precedência não indicada) | ..+..+++. | REGISTRADA (POUSADO e ABERTA na linha) | BACKLOG:1790 |
| PEP-REENTRADA-FILA | LATERAL-ESTRUTURA | Completa proteções contra reentrada e consultas desnecessárias. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1792 |
| R1-E6PLACAR-2O-NUMERO | INSTRUMENTO | Retira um segundo número antigo da mesma pergunta. | - | pouso próprio | ......... | REGISTRADA | BACKLOG:1797 | [nao verificado]
| R1-ROTULOS-FILA | INSTRUMENTO | Alinha rótulos, datas e selos às fontes do placar. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1798 |
| V3-IMPACTO-COM-DONO | CEREJA | Mostra o impacto da troca de motor por responsável. | - | cobertura integral; autorização das exportadas; O10-B | +........ | REGISTRADA (medida incompleta) | BACKLOG:1800 |
| V3-AUTORIDADE2-DIVERGE | CEREJA | Explica por que duas leituras do motor dão horas diferentes. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1801 |
| V3-ALIMENTACAO-VAZIA | CEREJA | Declara dias sem pares para evitar pagamento sem lavratura. | - | autoridade dos dias | ......... | REGISTRADA | BACKLOG:1802 |
| CODEX-CONSTRUTOR-NEGOCIO | INSTRUMENTO | Permite construção isolada com regras e acompanhamento do piloto. | - | próximo piloto MOTOR-LE-O-TURNO-INSTRUMENTO | .++..+++. | REGISTRADA (POUSADO e ABERTA na linha) | BACKLOG:1803 |
| CODEX-BANCO-DIRETO | INSTRUMENTO | Oferece leitura isolada do banco com credencial protegida e raspagem. | - | executor dedicado; vaga de instrumento | ++....... | REGISTRADA | BACKLOG:1804 |
| C6-RELOGIO-BORDA | LATERAL-ESTRUTURA | Faz a decisão de pergunta viva receber o relógio externo. | - | censo de chamadores; RED | ......... | REGISTRADA | BACKLOG:1805 |
| H12-DEMISSAO-NA-JANELA | LATERAL-ESTRUTURA | Considera a demissão na competência correta da folha. | - | vaga de negócio; antes da exportação 11 | .+....... | REGISTRADA | BACKLOG:1806 |
| H06-FALTA-PELA-GRADE | LATERAL-ESTRUTURA | Calcula a falta pela jornada prevista, evitando duração fixa. | - | H12; antes da exportação 11 | ......... | REGISTRADA | BACKLOG:1807 |
| SOMBRA-NAO-CABE-2 | INSTRUMENTO | Reduz verificações para caberem na rotina diária. | - | alarmes; previsto em lote; sete sombras | .+.....++ | REGISTRADA (restante pendente) | BACKLOG:1808 |
| BATERIA-QUEIXAS-ADMIN | ESPINHA-E | Testa quatro queixas nas escalas canônicas pela regra. | - | casos; validação do DP; FIO MUDO | .+....... | REGISTRADA | BACKLOG:1809 |
| PWA-OFFLINE-APP-SHELL | UI | Mantém navegação e fila de batidas sem rede. | - | identidade na fila; smoke | .+....... | REGISTRADA | BACKLOG:1810 |
| CONFIG-RAIZ-INERTE-EM-APP | INSTRUMENTO | Explica por que a configuração não alcança a sessão do aplicativo. | - | reinício na raiz | +........ | REGISTRADA (leitura feita; reinício pendente) | BACKLOG:1811 |
| CODEX-PRE-DESENHO-PILOTO | INSTRUMENTO | Examina o desenho antes da construção de oito obras. | - | conclusão das oito obras | ++....... | REGISTRADA | BACKLOG:1812 |
| MODO-COMUM-PASSE-LIVRE-PELO-DIA | LATERAL-ESTRUTURA | Reconhece a isenção que valia no dia consultado. | - | MODO-COMUM; censo do piloto | .+....... | REGISTRADA | BACKLOG:1813 |
| BO-[nome]-0511-CENSO | INSTRUMENTO | Agrupa problemas relatados e lista destinos por colaborador. | - | censo e sonda | ......... | REGISTRADA | BACKLOG:1814 |
| EFEITO-NONE-UM-LEITOR | LATERAL-ESTRUTURA | Dá resposta única para ausência com tipo fora do catálogo. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1815 |
| SNC2-FILA | LATERAL-ESTRUTURA | Completa testes e trata resíduos de relógio e criação. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1816 | [nao verificado]
| MEDE-CLEAR-X-COMPACT | INSTRUMENTO | Mede consumo e efeitos de limpar ou compactar contexto. | - | medição | ......... | REGISTRADA | BACKLOG:1819 |
| CODEX-QUEIMA-B | INSTRUMENTO | Confere divergências registradas de consumo e diagnóstico. | - | sondas | ......... | REGISTRADA | BACKLOG:1820 |
| O169-ATA-VAZIA-FOLGA | ESPINHA-E | Prova o fechamento da cobrança em folga com ata vazia. | - | caso com mutante | ......... | REGISTRADA | BACKLOG:1822 |
| O169-CONTADOR-NAO-MEDIDO | ESPINHA-E | Expõe chamados vivos sem medição do fato. | - | contador; vigia; caso | ......... | REGISTRADA | BACKLOG:1823 |
| O169-LIMIAR-15-DUPLO | ESPINHA-E | Centraliza o limite usado para classificar agrupamentos. | - | juiz único da assinatura | ......... | REGISTRADA | BACKLOG:1824 |
| HJ-INGESTAO-0511 | INSTRUMENTO | Organiza relatos por colaborador e dia com estado conferido. | - | sonda das funções reais | .+....... | REGISTRADA | BACKLOG:1825 |
| F-346 | LATERAL-ESTRUTURA | Faz a remarcação de férias limpar ausências pelo mesmo caminho. | - | fila de concorrência, trilha, veto e selos | .++..++++ | REGISTRADA (no ar antigo; residual pendente) | BACKLOG:1826 |
| H12H06-SEPARA-E-CRONTAB | LATERAL-ESTRUTURA | Separa a correção da falta da atualização do passivo. | - | censo de trava; desenho da parte B | ++...++++ | PARADA (parte B aguarda desenho) | BACKLOG:1827 |
| H12H06-B-SIMPLES | LATERAL-ESTRUTURA | Atualiza trinta linhas com reversão e repetição segura. | - | vaga; complemento B | ++....... | REGISTRADA | BACKLOG:1828 |
| H12H06-B-COMPLEMENTO | LATERAL-ESTRUTURA | Alinha leitores de pagamento parcial ao critério do juiz. | - | H12H06-B-SIMPLES | .+....... | REGISTRADA | BACKLOG:1829 |
| LEI-H12H06-B-A | LATERAL-ESTRUTURA | Aplica a correção e encaminha diferenças da exportada ao DP. | - | cópia de reversão; apply; frota; pauta 08 | .++....+. | REGISTRADA (aplicação e pauta pendentes) | BACKLOG:1830 |
| H12H06-B-PAUTA-08 | LATERAL-ESTRUTURA | Mostra ao DP o pago e o devido na exportada. | - | comando; dry; reversão | .+....... | REGISTRADA | BACKLOG:1831 |
| PAUTA-08-FILA-OPUS | LATERAL-ESTRUTURA | Corrige reversão, transação e documentação das pautas. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1832 |
| ESPURIA-E-O-QUE-E | INSTRUMENTO | Classifica dias com batidas órfãs e propõe tratamento. | - | medição; desenho; proposta de tela | .+....... | REGISTRADA | BACKLOG:1833 |
| H12H06-A-FILA | LATERAL-ESTRUTURA | Corrige o rótulo de desligamento e o teste da falta. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1834 | [nao verificado]
| LASTRO-FOLGA-FORA-DO-CATALOGO | 20/20 | Evita tratar tipo inválido de ausência como atestado. | - | medir escritor e frota | ......... | REGISTRADA | BACKLOG:1835; BACKLOG:1840 |
| TIPO-NA-FONTE | ESPINHA-E | Entrega a ata aos leitores por acesso de significado explícito. | - | corte; E3; DESLIGADO; FERIADO; O168; HJ-090 | ++++.+... | PARADA | BACKLOG:1836; RELATO:112; RELATO:149 |
| TIPO-ITEM3-NAOBLOQ-V5C | LATERAL-ESTRUTURA | Trata resíduos de escrita, leitura e prova da ata tipada. | - | desenho da TIPO-NA-FONTE | ......... | PARADA (aguarda corte) | BACKLOG:1837 |
| TIPO-ITEM1-DIMENSAO | INSTRUMENTO | Mede acessos à ata e declara limites da ferramenta. | - | conciliar registro do pouso | ..+....+. | REGISTRADA (pouso antigo; linha divergente) | BACKLOG:1838; BACKLOG:1836 | [nao verificado]
| FIO-DUP | 20/20 | Corrige os caminhos de duplicação do fio no cartório. | - | pouso O168 | .+....... | REGISTRADA | BACKLOG:1839 | [nao verificado]
| LASTRO-FAIXA-SEM-DEFAULT | 20/20 | Recusa tipos sem faixa definida em vez de escolher silenciosamente. | - | TIPO-NA-FONTE; pouso antes da relavra | .++...... | REGISTRADA | BACKLOG:1840 | [nao verificado]
| PDF-UNIAO-HHMM-ENTRE-DATAS | UI | Evita esconder turno aberto por horários iguais em datas diferentes. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1841 |
| TRIAGEM-FILA-OPUS-V6 | UI | Completa proteções e provas de câmera, selfie e armazenamento. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1842 |
| C20-FIO-MUDO | ESPINHA-E | Gera eventos quando a cobrança nasce ou perde fundamento. | - | cinco atos de perda | .+~...... | REGISTRADA (construtor antigo) | BACKLOG:1844 |
| C20-FIO-MUDO-FILA-CODEX | ESPINHA-E | Centraliza o reapontamento e torna a reabertura atômica. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1845 |
| C20-FIO-MUDO-FILA-OPUS | ESPINHA-E | Corrige concorrência, ordem, travas e relógio do fio. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1846 |
| LEI-FIO-MUDO-B | 20/20 | Preserva o caminho da cobrança sem reabrir continentes automaticamente. | - | fechamento pela E3 do FIO-E0-V4 | ++++.++.. | PARADA (aval manda não pousar) | BACKLOG:1847 |
| DIA-SEM-TRABALHO-PORTA | LATERAL-ESTRUTURA | Oferece decisão administrativa para batida em folga ou feriado. | - | LEI-FIO-MUDO-B | .+.+..... | REGISTRADA | BACKLOG:1848 |
| AUSENCIA-UMA-VIGENTE | LATERAL-ESTRUTURA | Evita descontar dias apresentados como abonados. | - | lei da família; autorização das exportadas | .+....... | REGISTRADA | BACKLOG:1849 |
| ESPURIA-TELA (+ESPELHO-DECIDIU-ICONE) | UI | Diferencia folga, feriado, ausência e batida sem decisão. | - | censo fora da L-061; smoke | .+....... | REGISTRADA | BACKLOG:1850 |
| PAINEL-SITUACIONAL-UM-MONTADOR | LATERAL-ESTRUTURA | Usa o mesmo montador na atualização individual e na lista. | - | censo; vaga de negócio | .+....... | REGISTRADA | BACKLOG:1851 |
| BUG-PLANTIO-NOTURNO-MAIS-1 | LATERAL-ESTRUTURA | Evita plantar a saída noturna um dia depois do correto. | caso 13–14 | E1-ENTRADAS; BO-0611-05; migração autorizada | .++...... | REGISTRADA (absorvida na E1; complemento pendente) | BACKLOG:1852; BACKLOG:1975 |
| PUSH-DISPUTA-SEM-PERGUNTA | LATERAL-ESTRUTURA | Evita convidar a responder pergunta que não foi criada. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1853 | [nao verificado]
| VIGIA-PLANTIO-CRIADO-EM-NAO-E-REAPLICACAO | INSTRUMENTO | Faz a vigilância enxergar reaplicações de batidas antigas. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1854 |
| SELO-SEM-RELOGIO-JULGA-ARQUIVO | INSTRUMENTO | Evita um teste ocultar relógios soltos no arquivo. | - | ESMERIL-V2 | ......... | REGISTRADA | BACKLOG:1855 | [nao verificado]
| ESMERIL-SEMANTICA | INSTRUMENTO | Inventaria significados de datas e horários para orientar correções. | - | aprovação de tela; obras das classes | +........ | REGISTRADA | BACKLOG:1856 |
| ESMERIL-SEM-INSTANTE | LATERAL-ESTRUTURA | Unifica o significado dos instantes e das datas locais. | - | RED; vaga; rebase; opus; pouso | ..~...... | REGISTRADA (suite antiga não certifica) | BACKLOG:1857 | [nao verificado]
| ESMERIL-SEM-DIA-DO-TURNO | LATERAL-ESTRUTURA | Faz leitores perguntarem ao juiz qual é o dia do turno. | - | BUG-PLANTIO; autorização do motor | ......... | REGISTRADA | BACKLOG:1858 |
| ESMERIL-SEM-BORDA | LATERAL-ESTRUTURA | Unifica datas finais e limites das janelas. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1859 |
| ESMERIL-SEM-DIAS-PREVISTOS | LATERAL-ESTRUTURA | Conta dias previstos somente quando a célula exige trabalho. | - | cura; selo O155; vaga | ++....... | REGISTRADA | BACKLOG:1860 |
| PUSH-PRESO-14 | INSTRUMENTO | Destrava a publicação dos commits pela régua vigente. | - | próximo marco fechado; apply | ......... | REGISTRADA | BACKLOG:1861 | [nao verificado]
| V2-BUGS-DO-MOTOR | LATERAL-ESTRUTURA | Corrige prorrogação, feriado, passagem de turno e dias abertos. | - | itens 1,3,4,5; apply 11; DIFF/pauta 10 | .++...+++ | REGISTRADA (item 2 no ar; restante pendente) | BACKLOG:1862 |
| L-123-MEDIDA-EM-PY | INSTRUMENTO | Distingue dados medidos de código na publicação dos placares. | - | volta 5; vaga; push | .++.+.+.. | REGISTRADA (retomada autorizada) | BACKLOG:1863 | [nao verificado]
| ESMERIL-V2-PLACAR-DADO-GERADO | INSTRUMENTO | Faz o placar ler medições de arquivo gerado. | - | ESMERIL-V2 | .+....... | REGISTRADA | BACKLOG:1864 |
| ESMERIL-SEM-POLITICA (SILENCIADO-ATE-E-PRAZO) | LATERAL-ESTRUTURA | Alinha o silêncio temporário e remove campos sem consumo. | - | censo; vaga | .+....... | REGISTRADA | BACKLOG:1865 |
| LOTE-VALIDAR-VIRA-JOB | LATERAL-ESTRUTURA | Executa a validação em segundo plano com resultado na tela. | caso 03 | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1866 |
| E-SEM-VIVA-B-VIGIA-POR-DIA | INSTRUMENTO | Mede a retenção de desligados por dia. | - | reavaliar após LEI-DESLIGADO-NAO-TRAVA | ......... | REGISTRADA | BACKLOG:1867 |
| BO-[nome]-0611 | INSTRUMENTO | Classifica catorze itens relatados e indica seus destinos. | caso 01–14 | triagem | .+....... | REGISTRADA | BACKLOG:1868 |
| BO-[nome]-0611-V2 | INSTRUMENTO | Confere problemas pelas ações de tela, espelho e pagamento. | caso 02,03,05,07,08,09,10,11,12 | O261; O10-C; L-030; LOTE-VALIDAR | ++....... | REGISTRADA | BACKLOG:1869 |
| PWA-FILA-OFFLINE | UI | Confere se batidas sem rede ficam guardadas até o envio. | - | leitura do envio e armazenamento | .+....... | REGISTRADA | BACKLOG:1870 |
| FERIAS-SALDO-SEM-TRAVA | LATERAL-ESTRUTURA | Protege o saldo de férias contra alterações simultâneas. | HJ-090 | HJ-090 | .+....... | REGISTRADA (absorvida na HJ-090) | BACKLOG:1873 |
| HJ-090 | LATERAL-ESTRUTURA | Faz alterações de férias respeitarem o mesmo saldo e trava. | HJ-090; HJ-091 | datas do DP; aplicação após pouso | +++++~... | EM VOO | BACKLOG:1874; RELATO:10; RELATO:17; RELATO:19; RELATO:53 | [nao verificado]
| JUSTIFICATIVA-PAGA-O-PERIODO | LATERAL-ESTRUTURA | Paga somente o período do turno coberto pela justificativa. | HJ-093 | HJ-090; DIFF; motor e folha | ++....... | INTERROMPIDA (brief pronto; ordem viva divergente) | BACKLOG:1875; RELATO:11; RELATO:23; anexo:interrompidas.txt:25 |
| FERIAS-2X-FILA | LATERAL-ESTRUTURA | Completa provas de concorrência, trilha e cancelamento de férias. | - | ? (precedência não indicada) | ......... | REGISTRADA | BACKLOG:1876 |
| V2-I2-FILA | LATERAL-ESTRUTURA | Corrige divergências entre cálculo, espelho e exportação. | - | corte sobre pontas do intervalo | ......... | REGISTRADA | BACKLOG:1877 |
| V2-I2-COL270-DONO | LATERAL-ESTRUTURA | Identifica a origem da mudança do previsto antes do apply. | - | investigação do previsto | ......... | REGISTRADA | BACKLOG:1878 |
| MOTOR-LE-O-TURNO | ESPINHA-E | Faz cálculo e pagamento usarem o mesmo turno oficial. | - | E2; E3; DIFF; autorização das exportadas | .+....... | REGISTRADA | BACKLOG:1879 |
| MOTOR-LE-O-TURNO-INSTRUMENTO | INSTRUMENTO | Mede cálculos que usam par ou dia fora da autoridade. | - | MOTOR-TURNO-INSTR-FILA; FATIA 1 | +++..++++ | REGISTRADA (pousado; residual pendente) | BACKLOG:1880 |
| MOTOR-TURNO-INSTR-FILA | INSTRUMENTO | Completa provas e limites do medidor de turnos. | - | FATIA 1 de MOTOR-LE-O-TURNO | ......... | REGISTRADA | BACKLOG:1881 |
| DISJUNTOR-B3-TARDIO | INSTRUMENTO | Mostra quando o alarme chega depois da publicação. | - | medir latência da varredura | ......... | REGISTRADA | BACKLOG:1882 |
| HOLERITE-ZIP-MATRICULA | LATERAL-ESTRUTURA | Retira o envio antigo de holerites sem uso pela tela. | - | E1; E2 | ++....... | REGISTRADA | BACKLOG:1883 |
| HOLERITE-CAMPO-MATCH-LIVRE | LATERAL-ESTRUTURA | Impede enviar holerites com

[truncado em 20000 de 58736 caracteres]

## 13/11 20:38 · sessao 19ba10373f74 · GRADE-MESTRA · leitura · origem=Code

=== LINHAS ===
| id | balde | entrega (1 frase de leigo) | resolve (HJ-NNN, caso NN ou -) | depende de | grade | estado | fonte (BACKLOG:linha) |
| CONTRATO-20-E2-QUAL-ATO | ESPINHA-E | Evita cobrar ausência quando feriado ou folga já explica o dia. | - | O-OUTBOX | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1073 |
| JUSTIFICATIVA-ENCERRA-ORIGEM | LATERAL-ESTRUTURA | Encerra a cobrança de origem quando a justificativa é aprovada. | - | grupo (1), sítio 1; irmã para cinco casos | + + + . . . . . + | REGISTRADA (parte no ar; cinco casos retidos) | app/docs/BACKLOG.md:1074 |
| EXPIRACAO-DA-ABERTA-COPIA-ANTIGA | ESPINHA-E | Faz os turnos antigos usarem a mesma regra de expiração dos demais. | - | ORFAS | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1076 |
| JUIZ-PERGUNTA-VIVA-MUDA-SEM-FATO | ESPINHA-E | Impede que a mesma pergunta mude de estado sem um fato registrado. | - | ? (precedência da retomada não escrita) | + + + + . + . . + | REGISTRADA (cura no ar; passivo e trilha pendentes) | app/docs/BACKLOG.md:1077 |
| PASSIVO-VIVA-EM-DISPUTA-FECHADA | LATERAL-ESTRUTURA | Devolve ao fluxo correto perguntas vivas dentro de disputas encerradas. | - | DESLIGAMENTO-NAO-CARIMBA-RESPONDIDA; acervo | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1078 |
| DESLIGAMENTO-NAO-CARIMBA-RESPONDIDA | LATERAL-ESTRUTURA | Corrige o tratamento das respostas quando ocorre desligamento. | - | DESLIGAR-LIQUIDA-TUDO | + + . . . . . . . | REGISTRADA (superada em 13/11; sucessora definida) | app/docs/BACKLOG.md:1079 |
| JUIZ-DE-DONO-DO-DIA | ESPINHA-E | Separa problemas do sistema de problemas de cadastro ou batida. | - | corte para predicado de dono | + . . . . . . . . | PARADA (sem predicado autorizado) | app/docs/BACKLOG.md:1080 |
| O210-COL417-COL931 | LATERAL-ESTRUTURA | Atualiza a apuração de dois colaboradores cujo fechamento ficou antigo. | - | O210 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1081 |
| GATE10-ESTRUTURA-EM-4-RUBRICAS-DA-10 | ESPINHA-E | Corrige diferenças entre quatro verbas calculadas e a regra aplicável. | - | corte de predicado; autorização se alcançar motor v2 | + + . . . . . . . | PARADA (predicado depende de corte) | app/docs/BACKLOG.md:1082 |
| LOTE-PLANTA-MARCO-AVULSO-E-ABRE-TURNO | LATERAL-ESTRUTURA | Evita que um reparo em lote abra turnos e retire horas trabalhadas. | - | ? (precedência do residual não escrita) | + + + + . . . . . | REGISTRADA (fechada na origem; quatro casos residuais) | app/docs/BACKLOG.md:1084 |
| LOTE-AGRUPA-POR-DIA-E-RASGA-O-PAR-DA-MEIA-NOITE | LATERAL-ESTRUTURA | Mantém juntas as pontas de um turno que atravessa a meia-noite. | - | remedição dos 51 casos em 32 dias | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1085 |
| BO-INSS-LEITORES-QUE-NAO-MIGRARAM | LATERAL-ESTRUTURA | Faz as cobranças concordarem sobre ausências que cobrem parte do dia. | - | ? (seis resíduos sem causa medida) | + + + . . . . . + | REGISTRADA (cura no ar; seis resíduos sem causa) | app/docs/BACKLOG.md:1087 |
| BO-ATESTADO-ANDROID-ERRO-AO-ENVIAR | LATERAL-ESTRUTURA | Permite enviar atestado e explica corretamente os erros de reenvio. | - | smoke de reenvio | + + + + . . . . + | REGISTRADA (parte no ar; falta smoke) | app/docs/BACKLOG.md:1091 |
| REABRIR-NAO-DESTROI | LATERAL-ESTRUTURA | Preserva respostas anteriores quando uma pergunta é reaberta. | - | smoke de clique | + + + . . . . . + | REGISTRADA (no ar desde 25/10; falta smoke) | app/docs/BACKLOG.md:1094 |
| GATE-EXPORT-10 | INSTRUMENTO | Libera o arquivo da folha depois das conferências e decisões exigidas. | - | decisão do DP sobre HE; autorização | + + + . . . . . . | PARADA (exportação barrada pela porta) | app/docs/BACKLOG.md:1095 |
| FILA-FORA-DO-BLOCO-QUE-O-HOOK-LE | INSTRUMENTO | Impede que obras desapareçam ao gerar novamente a fila. | - | ? (precedência não escrita) | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1096 |
| PARADO-CITADO-E-PROSA-QUE-EXPLICA | INSTRUMENTO | Evita alarmes falsos quando um registro apenas explica uma obra parada. | - | próxima leitura de tickets_placar | + + + + . . . . + | REGISTRADA (instrumento no ar; contador a remedir) | app/docs/BACKLOG.md:1097 |
| UNIVERSO-DE-PERGUNTA-DO-DIA-SAO-DOIS | LATERAL-ESTRUTURA | Faz cartório e vigia considerarem as mesmas perguntas de cada dia. | - | remedição; correção da FK no escritor | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1098 |
| O256 | UI | Reúne filtros, responsáveis e explicações do retrato da folha. | - | porta de PDF; O261 para complemento de dono; smoke final | + + + . . . . . + | PARADA (PDF sem porta; outras partes no ar) | app/docs/BACKLOG.md:1100 |
| DISPUTA-FECHADA-SEM-RESOLUCAO | LATERAL-ESTRUTURA | Exige que o encerramento da disputa registre o motivo. | - | corte sobre a porta de encerramento | + . . . . . . . . | PARADA (não construir sem corte) | app/docs/BACKLOG.md:1101 |
| VIGENCIA-SOBREPOSTA-COL515 | LATERAL-ESTRUTURA | Trata vínculos antigos que cobrem simultaneamente os mesmos dias. | - | corte sobre cadastro | + + . . . . . . . | PARADA (não construir sem corte) | app/docs/BACKLOG.md:1102 |
| HE-DE-14H-NUM-DIA (col945 12/09) | LATERAL-ESTRUTURA | Apura por que um dia recebeu quase quinze horas extras. | - | corte; medição da causa | + . . . . . . . . | PARADA (aguarda corte para entrar na fila) | app/docs/BACKLOG.md:1103 |
| ESMERIL familia (2) batida | ESPINHA-E | Detecta batidas escritas fora da porta ou lidas sem respeitar retratações. | - | família (1); suíte; merge | + + + + + . . . . | REGISTRADA (fechada na raia; suíte e merge pendentes) | app/docs/BACKLOG.md:1104 |
| ~~PROVA-DOMINIO~~ | INSTRUMENTO | Compararia o cálculo atual com recibos antigos pagos fora do sistema. | - | ? (execução cortada) | . + . . . . . . . | PARADA (cortada; estado fora da lista de exclusão) | app/docs/BACKLOG.md:1117 |
| BUG-ANCORA-NAO-REGENERA | LATERAL-ESTRUTURA | Faz a correção da fase de trabalho chegar aos dias afetados. | - | decisão sobre col824 e col899 | + + + + . . . . . | PARADA (cura fechada; decisões de cadastro pendentes) | app/docs/BACKLOG.md:1118 |
| WIZARD-FASE-ALCANCA-A-NAO-EXPORTADA | UI | Permite corrigir a fase desde o vínculo que cobre o dia errado. | - | VINCULO-LINHA-DO-TEMPO | + + + . . . . . . | PARADA (congelada; absorvida por outra obra) | app/docs/BACKLOG.md:1119 |
| AVISO-ENTRADA-ANTECIPADA | UI | Avisa quanto falta para começar a jornada quando o colaborador bate antes. | - | descongelamento pela L-096 | . + . . . . . . . | PARADA (congelada) | app/docs/BACKLOG.md:1122 |
| AVISO-ESPURIA-JANELA (desenho) | UI | Define o aviso do aplicativo para batidas fora da janela esperada. | - | confirmação do admin da folha e do dono | . . . . . . . . . | PARADA (desenho aguardando confirmação) | app/docs/BACKLOG.md:1123 |
| ABONO-NO-AR-NAO-FECHOU | ESPINHA-E | Faz os valores diários reproduzirem corretamente o fechamento gravado. | - | autorização; tratamento da competência 10 | + + + . . . . . . | PARADA (09 fechada; pendência truncada de autorização) | app/docs/BACKLOG.md:1126 |
| VINCULO-LINHA-DO-TEMPO | ESPINHA-E | Mantém uma sequência de vínculos válidos sem sobreposição. | - | ESMERIL-2; censo de leitores; autorização para col899 | + + + + . . . . . | PARADA (porta pronta; tripwire e constraint pendentes) | app/docs/BACKLOG.md:1127 |
| FAMILIA-FASE-12x36 | LATERAL-ESTRUTURA | Encontra fases incompatíveis com o trabalho realizado e prepara sua correção. | - | autorização para propostas | + + + . . . . . . | PARADA (itens feitos; propostas aguardam autorização) | app/docs/BACKLOG.md:1128 |
| O-HAIKU-INVESTIGA | LATERAL-ESTRUTURA | Dá ao assistente consultas para investigar escalas e preparar propostas. | - | O-DIA-PAGO S4; VINCULO-LINHA-DO-TEMPO | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1130 |
| O-DIA-PAGO | ESPINHA-E | Faz todas as telas lerem um único valor diário de pagamento. | - | fatias S3, S4, S5 e S5b | + + + . . . . . . | REGISTRADA (fechada por fatias; S5b citada em voo antigo) | app/docs/BACKLOG.md:1131 |
| O-DIA-PAGO S5 | CEREJA | Constrói o calculador independente que poderá substituir o cálculo antigo. | - | S1-S2; S5a; S3; autorização da troca | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1132 |
| O-JANELA-HE | LATERAL-ESTRUTURA | Conta hora extra fora da jornada conforme a regra ou autorização. | - | smoke dos treze passos | + + + . . . . + + | REGISTRADA (no ar desde 01/10; falta smoke) | app/docs/BACKLOG.md:1134 |
| CHAMADO-NASCE-EM-EXPORTADA | LATERAL-ESTRUTURA | Investiga cobranças novas abertas para competências já pagas. | - | censo dos emissores; decisão de desenho | + + . . . . . . . | REGISTRADA (não construir) | app/docs/BACKLOG.md:1137 |
| DISPUTA-4418-ABERTA-COM-TUDO-RESOLVIDO | LATERAL-ESTRUTURA | Trata disputas abertas que já não têm pergunta viva. | - | censo de disputas abertas sem pergunta viva | + + . . . . . . . | REGISTRADA (não construir) | app/docs/BACKLOG.md:1138 |
| COL900-VINCULO-X-CELULA | LATERAL-ESTRUTURA | Mostra a diferença entre a escala corrigida e os dias antigos pagos. | - | autorização; REGEN-EM-EXPORTADA | + + . . . . . . . | PARADA (aguarda autorização) | app/docs/BACKLOG.md:1140 |
| S5b-CALCULADOR-ESCREVE | CEREJA | Grava os resultados do calculador novo preservando a folha exportada. | - | autorização da troca | + + + . . . . . . | REGISTRADA (escrita exercida; troca aguarda autorização) | app/docs/BACKLOG.md:1141 |
| COL882-O-PROXIMO-COL515 | LATERAL-ESTRUTURA | Prepara a correção de dias trabalhados sem vínculo que os cubra. | - | autorização de vínculo | + + . . . . . . . | PARADA (aguarda autorização de cadastro) | app/docs/BACKLOG.md:1142 |
| DISPUTA-S84-RETRO-PASSIVO | LATERAL-ESTRUTURA | Separa batidas retroativas entre retratação e registro visível do trabalho. | - | cura da origem; autorização; REGEN-EM-EXPORTADA | + + . . . . . . . | PARADA (fonte conserva espera de autorização) | app/docs/BACKLOG.md:1144 |
| ESMERIL-MECANICO-POR-TRECHO | INSTRUMENTO | Confere mecanicamente os leitores e escritores por trecho de código. | - | ? (espera truncada de autorização) | + + + . . . . . . | PARADA (fechada na origem; espera truncada preservada) | app/docs/BACKLOG.md:1146 |
| BUG-APP-SESSAO-401 | UI | Avisa quando a perda de sessão impede registrar o ponto. | - | autorização; smoke nas duas cascas | + + + . . . . . + | PARADA (admin no ar; app aguarda smoke) | app/docs/BACKLOG.md:1148 |
| UI-ANEXO-AUSENCIA | UI | Mostra anexos pelo conteúdo e prepara a conversão de fotos HEIC. | - | catálogo; O30 F3; rebuild com decodificador HEIC | + + . . . . . . . | PARADA (conversão bloqueada pelo rebuild) | app/docs/BACKLOG.md:1149 |
| UI-PAINEL-MOSTRA-O-APP | UI | Faz o painel mostrar o mesmo rótulo e contagem do aplicativo. | - | lote UI-ANEXO-AUSENCIA | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1150 |
| DASHBOARD-V2-ROTULO-A-MAO | UI | Faz o painel alternativo ler o rótulo comum dos chamados. | - | pouso de UI-PAINEL-MOSTRA-O-APP | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1151 |
| ABSOLVIDA-TEXTO-PLANTAO | UI | Explica as datas do plantão e da saída após meia-noite. | - | UI-PAINEL-MOSTRA-O-APP; POS-20 | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1152 |
| UI-REGULARIZAR-FALA-A-LINGUA-DO-ADMIN | UI | Troca termos técnicos por ações claras para corrigir batidas. | - | proposta de tela; POS-20 | + + . . . . . . . | REGISTRADA (não construir) | app/docs/BACKLOG.md:1153 |
| O-JANELA-HE Gestao de HE (LISTAGEM) | UI | Lista horas extras pendentes com os mesmos dados do bloqueio da folha. | - | smoke de clique; merge | + + + + + . . . . | PARADA (pronta na raia; falta smoke para merge) | app/docs/BACKLOG.md:1155 |
| COL369-TRES-RESTANTES | LATERAL-ESTRUTURA | Apura três diferenças de intervalo, pagamento e vínculo do colaborador. | - | smoke da Gestão de HE; autorização para vínculo | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1156 |
| GESTAO-HE-FORMA | UI | Organiza a decisão de horas extras por colaborador e dias expansíveis. | - | desenho aprovado; corte sobre autorização de uma ponta | + + + . . . . . + | PARADA (expandido devolvido à bancada; pergunta aberta) | app/docs/BACKLOG.md:1157 |
| O-JANELA-HE parte B2 (AUTORIZAR) | LATERAL-ESTRUTURA | Permite autorizar hora extra e atualizar o pagamento pela porta correta. | - | B1; S4; smoke | + + + + . . . + + | REGISTRADA (no ar desde 01/10; falta smoke) | app/docs/BACKLOG.md:1158 |
| E4-CALENDARIO | ESPINHA-E | Faz o calendário usar o mesmo pagamento diário dos demais leitores. | - | próximo deploy; O-DIA-PAGO S3b | . + + + . . . . . | REGISTRADA (fechada; deploy ainda citado como próximo) | app/docs/BACKLOG.md:1159 |
| BO-354 | LATERAL-ESTRUTURA | Evita perguntas de saída erradas quando as batidas têm tipos invertidos. | - | descongelamento pela L-096 | + + . . . . . . . | PARADA (congelada) | app/docs/BACKLOG.md:1161 |
| E6-14 | ESPINHA-E | Identifica a causa de cada colaborador ainda fora da certificação. | - | troca da S3; descongelamento | . + . . . . . . . | PARADA (congelada) | app/docs/BACKLOG.md:1162 |
| O-FOLGA-DIA-ERRADO | LATERAL-ESTRUTURA | Sugere corrigir a escala quando o trabalho recorrente aponta folga cadastrada errada. | - | BO-354; corte de lei; autorização de vínculo | . . . . . . . . . | PARADA (congelada; espera corte) | app/docs/BACKLOG.md:1163 |
| AVISO-DO-DONO | UI | Avisa o responsável quando uma obra fecha ou precisa de autorização. | - | portão da 09; descongelamento; smoke no celular | . + . . . . . . . | PARADA (congelada) | app/docs/BACKLOG.md:1164 |
| O-GEO-DECISAO | LATERAL-ESTRUTURA | Registra decisões de localização e avisa quando um turno fica sem sinal. | - | corte das etapas 1-2; O198 antes do complemento | . + . . . . . . . | PARADA (etapas congeladas; complemento registrado) | app/docs/BACKLOG.md:1165 |
| O73b | LATERAL-ESTRUTURA | Corrige a cobrança de atraso na volta da pausa do turno partido. | - | descongelamento; autorização sobre a 09 | . + . . . . . . . | PARADA (congelada) | app/docs/BACKLOG.md:1166 |
| CORTE-B-30 | LATERAL-ESTRUTURA | Explica individualmente os casos separados do lote de correção. | - | descongelamento; corte do lote B | + + . . . . . . . | PARADA (congelada) | app/docs/BACKLOG.md:1167 |
| O145 | LATERAL-ESTRUTURA | Faz a Gestão de HE enxergar trabalho que não casou com o marco. | - | O229 para parte residual | + + + . . . . . + | REGISTRADA (cura no ar; 33 resíduos) | app/docs/BACKLOG.md:1170 |
| O146 | LATERAL-ESTRUTURA | Paga a extra declarada na escala sem aumentar a jornada devida. | - | linha do copiloto | + + + + . . . . + | REGISTRADA (no ar; falta linha do copiloto) | app/docs/BACKLOG.md:1171 |
| O214 | LATERAL-ESTRUTURA | Permite decidir horas extras por colaborador e período antes da folha. | - | autorização do apply do item 1; smoke do item 2 | + + + . . . . . + | PARADA (itens no ar; apply e smoke pendentes) | app/docs/BACKLOG.md:1172 |
| PLACAR-ESTRUTURAL | ESPINHA-E | Mostra se cálculo, telas e folha permanecem coerentes com o cadastro. | - | autorização dos casos da 09 exportada; lote do R6 | + + + . . . . . . | PARADA (resultados parciais; espera autorização) | app/docs/BACKLOG.md:1173 |
| O98 | INSTRUMENTO | Explica os termos da casa e aponta onde existem no código. | - | vaga da raia | + + . . . . . . . | REGISTRADA (arquivo parcial existente) | app/docs/BACKLOG.md:1177 |
| R1 | LATERAL-ESTRUTURA | Apura o caso em que o papel da batida altera um registro coerente. | - | autorização sobre o caso restante | + + . . . . . . . | PARADA (censo dissolveu a premissa geral; resta um caso) | app/docs/BACKLOG.md:1178 |
| SUITE-DO-NUCLEO-ENTRA-NA-REGUA | INSTRUMENTO | Inclui os testes do núcleo de mensageria nas verificações obrigatórias. | - | O135; pouso | + + + + . . . . . | REGISTRADA (fechada na raia; pouso condicionado) | app/docs/BACKLOG.md:1180 |
| L111-ORFAOS-DO-MOTOR | ESPINHA-E | Remove duas respostas antigas de cálculo que já não têm leitores. | - | conferência da O275/O277 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1181 |
| SOMBRA_BLOCO_NA_JANELA-SEM-LEITOR | INSTRUMENTO | Impede certificar uma sombra cujo bloco não cobriu a janela utilizada. | - | pouso próprio de instrumento | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1182 |
| PAR-NAO-ADJACENTE | LATERAL-ESTRUTURA | Preserva o par correto quando há uma batida solta entre suas pontas. | - | LEI-PAR-ANINHADO; volta 5 | . + + + . . . . . | PARADA (versão verde contradiz a lei) | app/docs/BACKLOG.md:1192 |
| O261-E-F2-FILA | LATERAL-ESTRUTURA | Corrige recusas caladas, relógios e identidades no tratamento das perguntas. | - | pouso O261-E-F2 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1193 |
| O261-E-F2-FILA-V2 | LATERAL-ESTRUTURA | Completa testes e corrige informações inconsistentes das perguntas. | - | pouso O261-E-F2 | + . . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1194 |
| REABERTURA-RECUSADA-DEDUP | LATERAL-ESTRUTURA | Evita repetir o registro da mesma reabertura recusada. | - | vez na fila | + . . . . . . . . | PARADA (construtor cancelado sem commit) | app/docs/BACKLOG.md:1195 |
| O232 | LATERAL-ESTRUTURA | Faz a ata reconhecer a saída tardia do mesmo turno. | - | complemento de 25/10; impacto de frota | + + + . . . . + + | REGISTRADA (A/B no ar; complemento da ata pendente) | app/docs/BACKLOG.md:1196 |
| O219 | ESPINHA-E | Rejulga dias quando muda a regra, respeitando seu alcance e período. | - | TXT novo da 09; Pauta DP | + + + . . . . + + | REGISTRADA (regra no ar; TXT e pauta pendentes) | app/docs/BACKLOG.md:1197 |
| O223 | UI | Permite cadastrar pela tela a convenção e o regime da empresa. | - | smoke do pouso A para push | + + + . . . . + + | REGISTRADA (pousos no ar; smoke para push pendente) | app/docs/BACKLOG.md:1198 |
| ADMIN-ESCREVE-EMPRESA-FORA-DA-PORTA | ? (classificação não decidida) | Faz o administrador alterar a empresa pelo caminho auditável da produção. | - | decidir forma; contratos | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1199 |
| O228 | ESPINHA-E | Remove código obsoleto de célula e turno com prova de desuso. | - | - | + + + + . . . + + | REGISTRADA (inteira no ar; data do fecho incerta) | app/docs/BACKLOG.md:1201 |
| O234 | INSTRUMENTO | Impede publicar contagens de cenários e testes já vencidas. | - | vez de instrumento | + . . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1202 |
| O236 | LATERAL-ESTRUTURA | Identifica por que colaboradores ficam fora do arquivo da folha. | - | medição por motivo; autorização se tocar exportada | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1203 |
| O237 | INSTRUMENTO | Faz os scripts usarem uma única porta de medição na sombra. | - | ? (migração dos scripts é trabalho da própria obra) | + . . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1204 |
| O238 | LATERAL-ESTRUTURA | Faz a trilha apontar o registro certo e distinguir escrita de mudança. | - | O219 A0-b | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1205 |
| O240 | INSTRUMENTO | Evita aprovar suíte interrompida confundindo mensagem de log com resultado. | - | ? (pré-requisito externo não escrito) | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1207 |
| O241 | INSTRUMENTO | Identifica alterações de duas raias ainda ausentes do código principal. | - | leitura dos 7+7 commits | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1208 |
| O242 | INSTRUMENTO | Faz o teste da corrente usar a própria cópia e preservar carimbos reais. | - | pouso próprio após produto | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1209 |
| O227 | INSTRUMENTO | Permite consultar competência exportada sem pedir autorização de escrita. | - | instrumento após O211 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1210 |
| O226 | INSTRUMENTO | Unifica a resposta sobre quais parâmetros afetam o cálculo. | - | censo dos leitores; decidir autoridade | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1211 |
| O225 | UI | Faz os avisos de parâmetro sem efeito acompanharem a declaração oficial. | - | vez de UI | + + . . . 

[truncado em 20000 de 53547 caracteres]

## 13/11 20:37 · sessao 10f0b8ffec02 · GRADE-MESTRA · leitura · origem=Code

=== LINHAS ===
| id | balde | entrega (1 frase de leigo) | resolve (HJ-NNN, caso NN ou -) | depende de | grade | estado | fonte (BACKLOG:linha) |
| FERIADO-ESCALA | LATERAL-ESTRUTURA | Corrige o trabalho em feriados conforme a regra vigente. | HJ-074 | FERIADO-USA-A-LEI-DE-1609 | +++~.~... | EM VOO (escopo anterior superado) | BACKLOG:728; app/docs/RELATO.md:134 |
| FERIADO-1609-FILA | LATERAL-ESTRUTURA | Corrige falhas restantes do tratamento de feriados. | HJ-074 | pouso FERIADO | .+....... | REGISTRADA | BACKLOG:729 | [nao verificado]
| LEI-DESLIGADO-NAO-TRAVA | LATERAL-ESTRUTURA | Impede que pendências de desligados bloqueiem a folha. | - | ? (não indicada) | ++++++~++ | NO AR hoje | BACKLOG:730; app/docs/RELATO.md:54 |
| DESLIGAR-LIQUIDA-TUDO | LATERAL-ESTRUTURA | Encerra cobranças do desligado e encaminha respostas ao DP. | - | LEI-DESLIGADO-NAO-TRAVA; O50 | ++....... | REGISTRADA (censo entregue) | BACKLOG:731; app/docs/RELATO.md:125 |
| DESLIGADOS-RETIDOS-CONTADOR | INSTRUMENTO | Mostra quantos desligados ainda ficam retidos na folha. | - | LEI-DESLIGADO-NAO-TRAVA | .+++.+.++ | NO AR hoje (complemento no mesmo pouso) | BACKLOG:732; app/docs/RELATO.md:103; app/docs/RELATO.md:54 |
| PARIDADE-ABSOLVIDAS-168 | LATERAL-ESTRUTURA | Devolve ao admin respostas de intervalo encerradas sem julgamento. | - | BUG-PAR-DESCONGELA; condições do apply | ++~+.~... | EM VOO (103 incorporadas ao BUG-PAR) | BACKLOG:733; app/docs/RELATO.md:95; app/docs/RELATO.md:35 |
| RAIOX-DIAS-DERIVA | LATERAL-ESTRUTURA | Faz o Raio-X mostrar os dias apontados pela autoridade central. | - | pouso LEI-DESLIGADO-NAO-TRAVA | .+....... | REGISTRADA | BACKLOG:734 | [nao verificado]
| SITUACAO-LEITOR-CRU | LATERAL-ESTRUTURA | Unifica como o sistema reconhece um desligamento. | - | O50 | .+....... | REGISTRADA | BACKLOG:735 |
| DESLIGADOS-RETIDOS-UNIVERSO | INSTRUMENTO | Faz a contagem de desligados retidos corresponder ao seu rótulo. | - | pouso LEI-DESLIGADO-NAO-TRAVA | .+....... | REGISTRADA | BACKLOG:736 | [nao verificado]
| RETRATO-DOIS-RELOGIOS | LATERAL-ESTRUTURA | Usa uma única data para montar o retrato da folha. | - | pouso LEI-DESLIGADO-NAO-TRAVA | .+....... | REGISTRADA | BACKLOG:737 | [nao verificado]
| OTIMIZA-1-COMPLEMENTO-CARGA | INSTRUMENTO | Reduz a memória retida e mede o pico do vigia. | - | pouso E3; contrato do instrumento | ++....... | BRIEF PRONTO (complemento coberto pelo brief OTIMIZA-1) | BACKLOG:738; app/docs/RELATO.md:25 |
| RAIA-PREPARACAO | INSTRUMENTO | Prepara levantamentos e instruções para as próximas obras. | HJ-091 | marco LEI-DESLIGADO; sequência 1→4 | ++....... | REGISTRADA (levantamentos entregues) | BACKLOG:739; app/docs/RELATO.md:12; app/docs/RELATO.md:28 |
| BO-[nome]-1311-ROTEIRO | INSTRUMENTO | Classifica os casos relatados e identifica o que falta resolver. | casos 01–10 | declaração do colaborador; projeção do retentor | ++....... | REGISTRADA (roteiro parcial) | BACKLOG:740; app/docs/RELATO.md:37 |
| HJ-092 | LATERAL-ESTRUTURA | Encaminha ao admin as batidas que exigem decisão humana. | HJ-092 | decisão do admin | ++....... | REGISTRADA (fechada como BATIDA; deploy não comprovado) | BACKLOG:741 | [nao verificado]
| TIPO-DE-BATIDA-DE-PERGUNTA-SEM-AVISO | UI | Avisa quando uma pergunta altera o tipo de batida. | HJ-092 | ? (não indicada) | .+....... | REGISTRADA | BACKLOG:742 |
| HJ-093 | LATERAL-ESTRUTURA | Corrige horas zeradas apesar de justificativa aprovada. | HJ-093 | JUSTIFICATIVA-PAGA-O-PERIODO; HJ-090; admin | ++....... | REGISTRADA (classe confirmada) | BACKLOG:743 |
| API-RETIDOS-ROTULO | UI | Distingue quem está retido de quem está fora da folha. | - | ? (não indicada) | .+....... | REGISTRADA | BACKLOG:744 |
| LANCAR-AUSENCIA-AFASTA-DESLIGADO | LATERAL-ESTRUTURA | Impede que lançar ausência transforme um desligado em afastado. | - | corte para ampliar o escopo | .+....... | REGISTRADA | BACKLOG:745 |
| FERIADO-RUBRICA-DO-FLAG-CORRENTE | LATERAL-ESTRUTURA | Evita que a escolha atual de feriado altere a rubrica histórica. | HJ-074 | impacto_g; decisão de desenho | ......... | REGISTRADA (desenho candidato) | BACKLOG:746 |
| SELOS-CANON-I1 | INSTRUMENTO | Exige levantamento aprovado antes de abrir uma construção. | - | SELOS-CANON-I1-FILA | .+++..++~ | REGISTRADA (núcleo no ar; fila restante) | BACKLOG:747 |
| SELOS-CANON-I1-FILA | INSTRUMENTO | Fecha lacunas na fiscalização do levantamento obrigatório. | - | SELOS-CANON-I1 | .+....... | REGISTRADA | BACKLOG:748 |
| SELOS-CANON-I3 | INSTRUMENTO | Exige etapas e resultados observáveis nas instruções da obra. | - | matriz; vaga | ++....... | REGISTRADA (brief antigo fora dos anexos admitidos) | BACKLOG:749 |
| SELOS-CANON-I2 | INSTRUMENTO | Recusa autorizações de dados sem universo e exclusões definidos. | - | SELOS-CANON-I3 | .+....... | PARADA | BACKLOG:750 |
| PERGUNTA-SEM-SELO | ESPINHA-E | Faz leitores estruturais consultarem a autoridade correta. | - | FECHAR2; autorização sobre exportada | +++...... | REGISTRADA (construção histórica) | BACKLOG:751 |
| PILHA-DA-BATIDA-RECALCULO-INLINE | LATERAL-ESTRUTURA | Reduz o cálculo feito durante o registro da batida. | - | medir latência; O-OUTBOX | +........ | REGISTRADA | BACKLOG:753 |
| O-PROJECOES | ESPINHA-E | Faz telas e documentos lerem resultados reconstruíveis da célula. | - | REJULGAR-COMP10-RESTO | .+....... | REGISTRADA | BACKLOG:756 |
| ADR-NAS-LEIS | INSTRUMENTO | Registra contexto e consequências de cada lei nova. | - | ? (não indicada) | .+....... | REGISTRADA (conduta vigente) | BACKLOG:757 |
| O-BITEMPORAL-2 | LATERAL-ESTRUTURA | Preserva quando vínculos e regras valeram e foram registrados. | - | contratos_estruturais 20/20 | .+....... | REGISTRADA | BACKLOG:758 |
| IA-DOMINIO-POR-FERRAMENTA | LATERAL-ESTRUTURA | Limita respostas da mensageria aos dados permitidos ao usuário. | - | 20/20 | .+....... | REGISTRADA | BACKLOG:759 |
| RECUSA-DO-ADMIN-DIZ-O-QUE-FAZER | UI | Explica a recusa de ausência e as ações possíveis. | - | BOs anteriores | .+....... | REGISTRADA | BACKLOG:760 |
| TELA-6X1-NAO-MOSTRA-HORARIOS-POR-DIA | UI | Mostra os horários por dia na edição da escala 6x1. | - | fila 2 | .+....... | REGISTRADA | BACKLOG:761 |
| JORNADA-EXCEPCIONAL-POR-DATA | CEREJA | Permite jornada excepcional para uma data específica. | - | 20/20; TROCA-DE-PLANTAO | .+....... | REGISTRADA | BACKLOG:762 |
| LISTA-DE-QUARENTENA | UI | Mostra quem está em quarentena e o caminho para levantá-la. | - | QUARENTENA-DO-COLAB | .+....... | REGISTRADA (incorporada à QUARENTENA) | BACKLOG:763 |
| O148-EDITAR | UI | Permite editar ou arquivar calendários de folgas com histórico. | - | corte sobre edição por versão | ......... | PARADA (lei nova pendente) | BACKLOG:764 |
| LAUDO-[nome]-COL94-1610 | INSTRUMENTO | Mede divergências de batida e perguntas invisíveis ao admin. | - | medidas e resposta por item | .+....... | REGISTRADA (medição histórica) | BACKLOG:765 |
| REJULGAR-COMP10-4-REVERTIDOS | LATERAL-ESTRUTURA | Reaplica correções revertidas com prova do que muda. | - | medida atual; SOMBRA-X-PROD-COL988-124639 | .+.....+. | REGISTRADA (aplicação feita; divergência restante) | BACKLOG:766 |
| SOMBRA-X-PROD-COL988-124639 | INSTRUMENTO | Identifica por que ensaio e produção julgam diferente. | - | medir o insumo divergente | +........ | REGISTRADA | BACKLOG:767 |
| TERMOMETRO-GRAVA-SEM-TRILHA | LATERAL-ESTRUTURA | Registra quem alterou o escore do vínculo e o motivo. | - | push junto do FIO | .++..+.+~ | REGISTRADA (no ar antigo; push pendente) | BACKLOG:768 | [nao verificado]
| SOMBRA-NAO-CABE-ANTES-DO-VIZINHO | INSTRUMENTO | Evita que o ensaio ultrapasse a janela dos comandos seguintes. | - | SOMBRA-PORTAO-POR-COMANDO | ++.....+. | REGISTRADA (instrumento parcial pousado) | BACKLOG:769 |
| CRON-NAO-CABE-SOMBRA-87-MIN | INSTRUMENTO | Faz o ensaio caber no horário disponível. | - | N1-PERGUNTAS-DO-APP; LAVRAR-HE-PENDENTE-ESCALA | ++....... | REGISTRADA (cura não comprovada) | BACKLOG:770 |
| TROCA-DE-POSTO-SEM-VINCULO | LATERAL-ESTRUTURA | Mantém coerentes o posto do cadastro e o vínculo vigente. | - | ? (não indicada) | +........ | REGISTRADA | BACKLOG:771 |
| LAVRAR-HE-PENDENTE-ESCALA | LATERAL-ESTRUTURA | Acelera a apuração de horas extras reutilizando dados carregados. | - | resíduos de leitores; decisão sobre segunda apuração | ..++.+.+~ | REGISTRADA (núcleo no ar; resíduos pendentes) | BACKLOG:773 |
| N1-ALIMENTACAO-UNICA | LATERAL-ESTRUTURA | Evita carregar repetidamente os mesmos dados das portas. | - | N1; interface E3 se ampliada | +........ | REGISTRADA (não aplicável ao corte atual da E3) | BACKLOG:774; app/docs/RELATO.md:71 |
| SOMBRA-PORTAO-POR-COMANDO | INSTRUMENTO | Exige ensaio atual dos comandos atingidos pela mudança. | - | medir comandos; SOMBRA-INCREMENTAL | .+....... | REGISTRADA | BACKLOG:775 |
| JUIZ-DO-PAR-DUAS-ENTRADAS | ESPINHA-E | Faz motor e ata formarem os mesmos turnos. | - | MOTOR-PASSA-O-TURNO-A-VOLTA; grupo 1 | ++.....+~ | REGISTRADA (cura no ar; resíduos pendentes) | BACKLOG:776 |
| BO-ADMIN-2710 | LATERAL-ESTRUTURA | Classifica problemas do admin e os encaminha às obras responsáveis. | - | medidas; dono e destino por item | .+....... | REGISTRADA (medição histórica) | BACKLOG:777 |
| TRILHA-DO-VINCULO-MENTE | LATERAL-ESTRUTURA | Faz o histórico do vínculo registrar corretamente as mudanças. | - | residual RT-03; push FIO | ++...+.+~ | REGISTRADA (núcleo no ar; RT-03 pendente) | BACKLOG:778 | [nao verificado]
| TENTATIVA-DO-ADMIN-VIRA-COBRANCA | UI | Mostra o efeito da edição antes de gerar novas cobranças. | - | medir regenerações; proposta de tela; corte | ......... | REGISTRADA (parte exige lei) | BACKLOG:779 |
| REGISTRADO-NAO-E-VIVO | LATERAL-ESTRUTURA | Evita mostrar auditoria como pendência de ação. | - | instrumento de fiscalização | .+.....+~ | REGISTRADA (produto no ar; instrumento pendente) | BACKLOG:780 |
| PENDENCIA-SEM-JUIZ-3 | ESPINHA-E | Faz três contadores de pendências usarem a mesma fila. | - | SILENCIO-VENCIDO-VOLTA | ++.....+~ | REGISTRADA (dois leitores no ar; falta um) | BACKLOG:782 |
| SILENCIO-VENCIDO-VOLTA | LATERAL-ESTRUTURA | Faz cobranças reaparecerem quando termina seu silêncio. | - | QUARENTENA-DO-COLAB; E3 | .++...... | INTERROMPIDA | BACKLOG:783; anexo:interrompidas.txt:1 |
| QUARENTENA-DO-COLAB | LATERAL-ESTRUTURA | Isola cobranças por período e permite seu retorno. | - | reaplicar SPEC; DIFF; E3 | .+++..~.. | INTERROMPIDA | BACKLOG:784; anexo:interrompidas.txt:3 |
| COBERTURA-PARCIAL-UM-JUIZ | ESPINHA-E | Faz ausências parciais cobrirem apenas o trecho devido. | - | COBPARC-DIFF-ISOLADO; autorização sobre exportada | .+++.+~+~ | PARADA (reaberta; PAREI de impacto) | BACKLOG:786 |
| ATA-DEFASADA-COMP09 | LATERAL-ESTRUTURA | Distingue atas antigas de mudanças causadas pela correção. | - | COBPARC-DIFF-ISOLADO | ++....~.. | PARADA (itens devolvidos ao PAREI) | BACKLOG:787 |
| GRADE-POS-POUSO | ESPINHA-E | Confere o efeito da nova grade após o deploy. | - | cartório pós-deploy; rebase COBPARC | .+....... | REGISTRADA | BACKLOG:789 | [nao verificado]
| COBPARC-CONTROLE | INSTRUMENTO | Compara a correção com um ensaio sem a correção. | - | explicar chamados adicionais | ++....... | PARADA (COBPARC-DET-NAO-EXPLICA) | BACKLOG:790 |
| COBPARC-ATA-DEFASADA-09 | LATERAL-ESTRUTURA | Trata divergências estruturais restantes da competência exportada. | - | nova medida; Pauta DP | ++....... | REGISTRADA | BACKLOG:791 |
| COBPARC-AUSENCIA-CALA-DOIS-JUIZES | ESPINHA-E | Faz dois caminhos concordarem sobre cobrar ausência parcial. | - | COBPARC | .+....... | REGISTRADA | BACKLOG:792 |
| COBPARC-FUROS-LEITOR-PARCIAL | ESPINHA-E | Faz leitores reconhecerem os furos restantes da ausência parcial. | - | COBPARC | .+....... | REGISTRADA | BACKLOG:793 |
| FIO-LE-O-JUIZ | ESPINHA-E | Mostra perguntas vivas mesmo em disputas encerradas. | - | clique de validação; push | .++....+~ | REGISTRADA (no ar antigo; push pendente) | BACKLOG:794 | [nao verificado]
| O245-CAMADA-COBERTURA | INSTRUMENTO | Mostra autoridades e leitores ainda sem cobertura. | - | push FIO | .++..+.+~ | REGISTRADA (no ar antigo; push pendente) | BACKLOG:795 | [nao verificado]
| FECHAR3-SOBREVIVENTE-NO-RECONCILIAR | LATERAL-ESTRUTURA | Preserva o chamado correto na conciliação de duplicados. | - | medir ocorrência | .+....... | REGISTRADA | BACKLOG:797 |
| CXR-TELA-LE-O-MOTOR | ESPINHA-E | Mostra ao admin os dias divergentes apontados pelo motor. | - | ATA-X-TURNO-PAUSA; CXR-PRONTO5-DEFINICAO; lavra | .+++..++~ | REGISTRADA (no ar; conclusão parcial) | BACKLOG:798 |
| CXR-PRONTO1-COL841 | ESPINHA-E | Faz sete dias pendentes chegarem à lista do admin. | - | ATA-X-TURNO-PAUSA | .+....... | REGISTRADA | BACKLOG:799 |
| E1-ACERVO-5-GRUPOS-09 | ESPINHA-E | Trata cinco grupos antigos preservando a competência exportada. | - | REGEN-EM-EXPORTADA; DIFF; autorização | .+....... | PARADA (retida por aval) | BACKLOG:800 |
| REJULGAR-61-COBPARC | LATERAL-ESTRUTURA | Reavalia atas antigas de ausência parcial pela regra atual. | - | autorização das 55 atas exportadas | .+....... | PARADA (aguarda autorização) | BACKLOG:801 |
| RETRATADA-CONVERSA-E-REABRIR | ESPINHA-E | Impede conversas sobre batidas já retratadas. | - | pouso E1 | .+....... | REGISTRADA | BACKLOG:802 | [nao verificado]
| E1-A2-VAZIO-MEDIR | ESPINHA-E | Identifica perguntas cuja origem não permite determinar o dia. | - | pouso E1; medida na sombra | .+....... | REGISTRADA | BACKLOG:803 | [nao verificado]
| E1-RECUSAS-SEM-CONTADOR | INSTRUMENTO | Conta recusas de perguntas com origem inválida ou sem dia. | - | pouso E1 | ......... | REGISTRADA | BACKLOG:804 | [nao verificado]
| E1-DUPLICATA-SOBRE-RETRATADA | ESPINHA-E | Trata perguntas duplicadas sobre episódio retratado. | - | pouso E1; decisão junto da E3 | ......... | REGISTRADA | BACKLOG:805 | [nao verificado]
| CXR-PRONTO5-DEFINICAO | ESPINHA-E | Define e mede quando um dia ganhou dono na lista. | - | definição do resultado | .+....... | REGISTRADA | BACKLOG:806 |
| CXR-MEDIDO-EM-AUTO-NOW | LATERAL-ESTRUTURA | Distingue a hora da medição da hora de gravação. | - | ? (não indicada) | .+....... | REGISTRADA | BACKLOG:807 |
| CXR-BECO-SEM-BATIDA-FORA-DA-LAVRA | LATERAL-ESTRUTURA | Inclui colaboradores sem batidas na medição dos dias sem caminho. | - | medir universo faltante | ......... | REGISTRADA | BACKLOG:808 |
| TETO-NA-LISTA-DOIS-RELOGIOS | LATERAL-ESTRUTURA | Usa uma única data para medir e publicar o teto. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:809 |
| TRAVA-COLAB-X-DISPUTA | LATERAL-ESTRUTURA | Investiga possível conflito entre travas de colaborador e disputa. | - | reproduzir com transação externa | ......... | REGISTRADA (hipótese não reproduzida) | BACKLOG:811 |
| JUIZ-DO-PAR-GRUPO-1 | ESPINHA-E | Faz leitores de batida e marco consultarem a mesma autoridade. | - | detector; LASTRO-DEGRAU-2 | ++.....+~ | REGISTRADA (primeiro pouso no ar; grupo incompleto) | BACKLOG:812 | [nao verificado]
| MOTOR-PASSA-O-TURNO-A-VOLTA | ESPINHA-E | Faz o cálculo reconhecer corretamente a volta do intervalo. | - | grupo 1; DIFF; reversão | ++....... | REGISTRADA | BACKLOG:813 |
| DISPARAR-PERGUNTAS-NO-CONTRATO-20 | 20/20 | Faz o disparo de perguntas seguir a célula e o caminho devido. | - | E3-A; PAR v5; fio mudo; C11 | ++....... | REGISTRADA (censo fechado; obra aberta) | BACKLOG:814; BACKLOG:2076 |
| JUSTIFICATIVA-ADMIN-FORA-DA-PORTA | LATERAL-ESTRUTURA | Impede aprovações sem efeitos e histórico completos. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:815 |
| O261-COMPLEMENTO-[nome] | ESPINHA-E | Faz perguntas vivas chegarem ao colaborador por chamado vivo. | - | O261; nascimento; contador de alcance | .++...... | REGISTRADA (parte construída) | BACKLOG:816 |
| REATAR-ESCREVE-EM-DIA-TRANCADO | LATERAL-ESTRUTURA | Evita alterar perguntas de dias protegidos pela tranca. | - | medir na sombra | .+....... | REGISTRADA | BACKLOG:817 |
| C6-TETO-MARCO-PENDENTE | ESPINHA-E | Evita cobrar um marco antes de terminar seu prazo. | - | cura no juiz C6 | .+....... | REGISTRADA | BACKLOG:818 |
| PASSIVO-VALIDADO-K6 | ESPINHA-E | Usa a autoridade correta para reconhecer competência entregue. | - | juiz K6 | .+....... | REGISTRADA | BACKLOG:819 |
| PROGRAMA-CELULA-E-O-DIA | ESPINHA-E | Faz todas as telas lerem o mesmo julgamento do dia. | - | E1; E2; E3; E4 | .++...... | REGISTRADA (programa parcial) | BACKLOG:821 |
| PROGRAMA-CELULA-E-O-DIA-DUAS-PISTAS | ESPINHA-E | Organiza a unificação do status e a certificação do cálculo. | - | E2; E3-STATUS; E4; casos E1 | .++...... | REGISTRADA (construção histórica) | BACKLOG:822 |
| E4-CAL-RETIDO-FOLHA-NO-LEITOR | ESPINHA-E | Faz o calendário ler o status pronto da folha. | - | E4-CALENDARIO-L1; D4 | .+....... | REGISTRADA (complemento) | BACKLOG:823 |
| E4-CAL-FALLBACK-STATUS-ROTULO | ESPINHA-E | Faz o calendário usar um único rótulo do dia. | - | E4-CALENDARIO-L1 | .+....... | REGISTRADA (complemento) | BACKLOG:824 |
| E4-CAL-DOIS-CONTRATOS-DE-CELULAS | ESPINHA-E | Unifica o formato das células recebidas pelo calendário. | - | E4-CALENDARIO-L1 | .+....... | REGISTRADA (complemento) | BACKLOG:825 |
| E4-INCLUI-CONTRATO-20 | ESPINHA-E | Faz telas e rotinas lerem o status lavrado na célula. | - | E3; lista dos leitores | .+....... | REGISTRADA | BACKLOG:826 |
| GRADE-FOLGA-DO-VINCULO | ESPINHA-E | Impede folgas de vínculos encerrados de reaparecerem na grade. | - | E4-1 | .++...... | REGISTRADA (construção histórica) | BACKLOG:827 |
| ODOMETRO-DE-REGRAS | INSTRUMENTO | Detecta regras duplicadas ou excessivas antes da publicação. | - | CLAUDE-MD-RACIONAL; contrato | .+....... | REGISTRADA | BACKLOG:829 |
| REPROVA-E-DESENHO | INSTRUMENTO | Para obra reprovada até registrar sua decisão de desenho. | - | linha DESENHO ou ERRO-BOBO | .++...... | REGISTRADA (construção histórica) | BACKLOG:830 |
| REPROVA-E-DESENHO-CODEX | INSTRUMENTO | Exige decisão quando revisões acumulam achados procedentes. | - | REPROVA-E-DESENHO; suíte v4 | .+....... | REGISTRADA | BACKLOG:831 | [nao verificado]
| TRAVA-MENOS-FILA | INSTRUMENTO | Reduz testes repetidos e mostra a espera na fila. | - | contrato do instrumento | .+....... | REGISTRADA (conduta parcial vigente) | BACKLOG:832 | [nao verificado]
| TRAVA-COBERTURA | INSTRUMENTO | Vincula testes à obra e remove esperas superadas. | - | vaga; sete curas; COPIA-DO-HEAD | .++..~... | REGISTRADA (volta 4 aguardando vaga) | BACKLOG:834 | [nao verificado]
| ADR-0003-CELULA-TRES-NATUREZAS | INSTRUMENTO | Documenta os fatos e as referências vivas da célula. | - | CORRECAO-ADR-0003; censo | ~+....... | REGISTRADA (desenho corrigido) | BACKLOG:835; BACKLOG:836 |
| CORRECAO-ADR-0003 | INSTRUMENTO | Distingue a célula viva da fotografia do fechamento. | - | ADR-0003-CELULA-TRES-NATUREZAS | .+....... | REGISTRADA | BACKLOG:836 |
| PADRAO-DE-CIMA | INSTRUMENTO | Exige rever desenhos que se repetem sem resolver o problema. | - | decisão PDC-DESENHO | .++..~~.. | INTERROMPIDA | BACKLOG:837; anexo:interrompidas.txt:5 |
| DISJUNTOR-B1-ESPERA-OCIOSA | INSTRUMENTO | Detecta agentes apenas esperando testes ou sem produzir. | - | PADRAO-DE-CIMA; contrato | .+....... | REGISTRADA | BACKLOG:838 | [nao verificado]
| LEI-AKITA-RESIDUO | INSTRUMENTO | Fecha lacunas na verificação dos compromissos da mudança. | - | ? (não indicada) | ......... | REGISTRADA | BACKLOG:839 |
| B3-CODEX-VOLTA-NUNCA-TRIADA | INSTRUMENTO | Conta revisões sem triagem para que não desapareçam da fiscalização. | - | REPROVA-E-DESENHO-CODEX | .+....... | REGISTRADA | BACKLOG:840 |
| GRADE-V5-SEM-PORTAO | ESPINHA-E | Corrige folga de vínculo encerrado preservando a folha entregue. | - | ? (não indicada) | .++...... | REGISTRADA (construída; curso atual não comprovado) | BACKLOG:844 |
| GRADE-FOLGA-JUIZ | ESPINHA-E | Faz os leitores usarem a mesma decisão sobre folga lançada. | - | decisão GRADE-TETO | .++...~.. | REGISTRADA (construção antiga; opus reprovado) | BACKLOG:845 |
| FASE-CONFLITANTE-VINCULO-DO-DIA | INSTRUMENTO | Faz o vigia conferir o vínculo corret

[truncado em 20000 de 61014 caracteres]

## 13/11 20:36 · sessao 2c9649399978 · GRADE-MESTRA · leitura · origem=Code

=== LINHAS ===
| id | balde | entrega (1 frase de leigo) | resolve (HJ-NNN, caso NN ou -) | depende de | grade | estado | fonte (BACKLOG:linha) |
| O164 | ESPINHA-E | Centraliza alterações de chamados em portas rastreáveis. | - | C2 → C3 → C4 → C1 | ++~...... | REGISTRADA (construção antiga) | BACKLOG:1418 |
| O165 | INSTRUMENTO | Identifica alterações que podem ser publicadas sem esperar autenticação. | - | Censo das dependências da raia | ~+....... | REGISTRADA (leitura antiga) | BACKLOG:1419 | [nao verificado]
| O166 | ESPINHA-E | Faz tarefas de prazo consultarem a autoridade responsável. | - | O139; juiz do prazo | .+~...... | REGISTRADA (construção antiga) | BACKLOG:1420 |
| O167 | UI | Impede correções sem trilha pelo painel administrativo. | - | Censo de quem corrige dados pelo painel | ++....... | REGISTRADA | BACKLOG:1421 |
| O168 | 20/20 | Preserva a cobrança enquanto uma resposta aguarda julgamento. | - | Guarda no evento antes de retirar o executor do supra | +++..++++ | NO AR hoje | BACKLOG:1422; BACKLOG:2076; app/docs/RELATO.md:5; app/docs/RELATO.md:7; app/docs/RELATO.md:31 |
| O168-FILA-OPUS | 20/20 | Corrige oito problemas restantes na proteção das conversas. | - | Itens 3 e 4 antes do executor; item 5 no marco seguinte | ++....... | REGISTRADA | BACKLOG:1423; BACKLOG:2076 |
| O169 | 20/20 | Impede encerrar cobranças apenas porque envelheceram. | - | ? (célula antiga conserva pergunta já superada) | +........ | REGISTRADA (pousada em outra fonte) | BACKLOG:1424; BACKLOG:1821; BACKLOG:2076 |
| O170 | ESPINHA-E | Faz cobranças acompanharem os acontecimentos sem esperar varreduras. | - | O167; unicidade; ordem dos eventos; O168; O169 | ++....... | REGISTRADA | BACKLOG:1425 |
| O171 | ESPINHA-E | Faz a folha acompanhar o julgamento atualizado do dia. | - | Medir primeiro evento posterior ao deploy | ++....... | REGISTRADA | BACKLOG:1426 | [nao verificado]
| O172 | LATERAL-ESTRUTURA | Inclui a data na identificação dos insumos do julgamento. | - | ? (não indicada) | +........ | REGISTRADA | BACKLOG:1427 |
| O173 | INSTRUMENTO | Detecta imports incompatíveis entre disco e código servido. | - | Deploy do O142 | ++....... | REGISTRADA | BACKLOG:1428 | [nao verificado]
| O174 | INSTRUMENTO | Explica quando tarefas e servidores leem versões diferentes. | - | ? (não indicada) | +........ | REGISTRADA | BACKLOG:1429 |
| O175 | LATERAL-ESTRUTURA | Evita reconstruir turnos em competências já exportadas. | - | Teto de competência aberta ou corte sobre o cache | ++....... | PARADA (espera corte) | BACKLOG:1430 |
| O176 | INSTRUMENTO | Impede liberar uma fatia por coincidência de título. | - | Corte para conferir o conteúdo do pacote | ++....... | PARADA (espera corte) | BACKLOG:1431 |
| O177 | ESPINHA-E | Faz o cálculo considerar todas as pausas cadastradas. | - | DIFF de frota; reversão; exportadas intactas | ++....... | REGISTRADA | BACKLOG:1432 |
| O178 | ESPINHA-E | Dá responsável e prazo às pendências do administrador. | - | Prazo, dono e contador de leitura da pauta | ++....... | REGISTRADA | BACKLOG:1433 |
| O179 | ESPINHA-E | Investiga dias com batidas que chegam zerados à folha. | - | Medir horas e decidir o dono pela L-099 | ++....... | REGISTRADA | BACKLOG:1434 |
| O180 | ESPINHA-E | Entrega ao administrador dias gerados além do vínculo. | - | Lista dos colaboradores; ! para regenerar passado | ++....... | PARADA (ato de passado espera !) | BACKLOG:1435 |
| O181 | LATERAL-ESTRUTURA | Preserva a explicação das horas incluídas na folha. | - | Destino persistente para a trilha | ++....... | REGISTRADA | BACKLOG:1436 |
| O183 | ESPINHA-E | Impede chamados com urgências desconhecidas pelo sistema. | - | Merge; DIFF; tratamento do estoque | ++++..... | REGISTRADA (cura na raia; estoque pendente) | BACKLOG:1438 |
| O184 | LATERAL-ESTRUTURA | Remove ou corrige uma função que ensina uma medição errada. | - | Decidir remoção ou correção da documentação | ++....... | REGISTRADA | BACKLOG:1439 |
| O185 | INSTRUMENTO | Compara a autenticação com a versão realmente servida. | - | Fatia própria posterior | ++....... | REGISTRADA | BACKLOG:1440 |
| O187 | INSTRUMENTO | Mostra quem consulta cada campo importante do sistema. | - | Fechamento da matriz estrutural | ++....... | REGISTRADA | BACKLOG:1442 |
| O188 | INSTRUMENTO | Impede que renomear código pareça resolver uma pendência. | - | Fechamento da matriz estrutural | .+....... | REGISTRADA | BACKLOG:1443 |
| O189 | ESPINHA-E | Publica o efeito de distinguir ausência de informação de zero. | - | Decisão da L-102 | ++....... | REGISTRADA (medida publicada; nada aplicado) | BACKLOG:1444 | [nao verificado]
| O190 | INSTRUMENTO | Inclui mensagens na verificação periódica da árvore. | - | Pouso do cert-ast | +........ | REGISTRADA | BACKLOG:1445 | [nao verificado]
| O192 | INSTRUMENTO | Evita arquivos de outro usuário nas cópias de teste. | - | Pouso do cert-ast | ++....... | REGISTRADA | BACKLOG:1447 | [nao verificado]
| O193 | INSTRUMENTO | Faz o vigia respeitar pausas declaradas com responsável. | - | Pouso próprio junto do cert-ast | ++....... | REGISTRADA | BACKLOG:1448 | [nao verificado]
| O194 | ESPINHA-E | Faz a lavra da folga esperar o encerramento real do turno. | - | Fatia própria da lavra | ++....... | REGISTRADA | BACKLOG:1449 |
| O196 | INSTRUMENTO | Mostra todas as tabelas alteradas pela relavratura da folha. | - | CELULA-TURNO-FECHA; O145; fatia de dinheiro | ++....... | REGISTRADA | BACKLOG:1451 |
| O198 | LATERAL-ESTRUTURA | Publica faltas aprovadas em dias sem jornada. | - | Corte posterior ao censo | +........ | PARADA (censo publicado; ordem de parar) | BACKLOG:1453 | [nao verificado]
| O199 | LATERAL-ESTRUTURA | Faz o aceite da folga decidir o dia e encerrar a cobrança. | - | ! para estoque antigo; O246 | +++....++ | REGISTRADA (ato publicado; estoque pendente) | BACKLOG:1454 | [nao verificado]
| O201 | INSTRUMENTO | Evita confundir comentários com regras ainda executadas. | - | Decisão sobre a regra ou as lápides | ++....... | REGISTRADA | BACKLOG:1456 |
| O202 | LATERAL-ESTRUTURA | Faz pings consultarem a mesma avaliação oficial de localização. | - | ? (não indicada) | ++....... | REGISTRADA | BACKLOG:1457 |
| O203 | INSTRUMENTO | Impede apagar código e proteção no mesmo commit silenciosamente. | - | Selo consultar a proteção da base | ++....... | REGISTRADA | BACKLOG:1458 | [nao verificado]
| O204 | LATERAL-ESTRUTURA | Preserva a hora enviada pelo aparelho e registra descartes. | - | Linha do placar em pouso próprio | ++++...++ | REGISTRADA (produto publicado; placar pendente) | BACKLOG:1459 | [nao verificado]
| O205 | INSTRUMENTO | Confere a árvore deixada por uma reversão automática. | - | Fila de instrumento posterior ao produto | ++....... | REGISTRADA | BACKLOG:1460 |
| O206 | UI | Oferece cobrança com prévia fiel e preservação das respostas. | - | Correção da prévia; smoke das duas cascas | +++....++ | PARADA (smoke segurado) | BACKLOG:1461 |
| O210 | ESPINHA-E | Atualiza fechamentos que ficaram atrás do cálculo atual. | - | Foto nova; DIFF; reversão; prova posterior | ++....... | REGISTRADA | BACKLOG:1464 |
| O213 | LATERAL-ESTRUTURA | Conta perguntas cujo tipo de marcação discorda da ata. | - | Publicação do censo antes da cura | ++....... | REGISTRADA (censo feito; nada alterado) | BACKLOG:1465 |
| O229 | ESPINHA-E | Corrige diferenças entre marcos e leitores de horas. | - | Censo e RED por classe; O197 no item 9 | ++....... | REGISTRADA | BACKLOG:1466 |
| O215 | ESPINHA-E | Compara o arquivo pago com o cálculo atual da folha. | - | TXT na sombra; decisão sobre os dois valores | .+....... | REGISTRADA | BACKLOG:1467 |
| O216 | ESPINHA-E | Refaz a medição após a correção do juiz dos turnos. | - | O195; medição pela função real | ++....... | REGISTRADA | BACKLOG:1468 |
| O217 | ESPINHA-E | Verifica se repetir o julgamento estabiliza a ata. | - | Cura e bateria da O218 | ++....... | REGISTRADA (medição feita; cura na O218) | BACKLOG:1469 |
| O230 | INSTRUMENTO | Permite sondas com acesso restrito de leitura e trilha. | - | Pouso 2; REDs c/d/f; selo na régua | ++++...~. | REGISTRADA (pouso 1 fechado; segundo pendente) | BACKLOG:1470 | [nao verificado]
| O233 | INSTRUMENTO | Mostra raias abertas há mais de seis horas sem commit. | - | Pouso 2 da O230 | .+....... | REGISTRADA | BACKLOG:1472 | [nao verificado]
| O235 | UI | Explica o efeito de intervalo vazio ou zero no formulário. | - | O207 | ++....... | REGISTRADA | BACKLOG:1473 |
| O243 | LATERAL-ESTRUTURA | Revisa setores críticos e a utilidade dos respectivos testes. | - | Contratos estruturais 20/20; censo antes da poda | .+....... | REGISTRADA | BACKLOG:1474 | [nao verificado]
| RELATO-CURTO-SELO | INSTRUMENTO | Impede novos relatos acima do limite de linhas. | - | O219; pouso de instrumento da O228 | ++....... | REGISTRADA | BACKLOG:1475 | [nao verificado]
| O244 | INSTRUMENTO | Explica em documentos curtos quem manda em cada família. | - | Contratos estruturais 20/20 | .+....... | REGISTRADA | BACKLOG:1476 |
| O245 | INSTRUMENTO | Gera um diagrama de regras, responsáveis e pendências. | - | Contratos estruturais 20/20 | .+....... | REGISTRADA | BACKLOG:1477 |
| O246 | LATERAL-ESTRUTURA | Registra aceites antigos de folga que não decidiram o dia. | - | O48 passo 2; RECALC-09; O219; DIFF e reversão | ++....... | REGISTRADA | BACKLOG:1478 |
| O248 | ESPINHA-E | Centraliza a resposta sobre o encerramento real do dia. | - | O258; item 3 da fila | ++....... | REGISTRADA (complemento da O258) | BACKLOG:1480 |
| O249 | ESPINHA-E | Faz a detecção de pares consultar uma única autoridade. | - | O258; juiz antes da mudança do emissor | ++....... | REGISTRADA (complemento da O258) | BACKLOG:1481 |
| O250 | INSTRUMENTO | Mostra por raia a obra, o agente e o que falta pousar. | - | Dois pousos de produto; simular_folha --lido | +++...... | REGISTRADA (construída; pouso pendente) | BACKLOG:1482 | [nao verificado]
| O251 | UI | Alinha a prévia da cobrança e expõe uma configuração de cálculo. | - | O207; raia própria; DIFF e reversão | ++....... | REGISTRADA | BACKLOG:1483 |
| O252 | ESPINHA-E | Faz a aprovação seguir o mesmo critério da exportação. | - | Contrato batida × juiz; O48 passo 5; DIFF | ++....... | REGISTRADA | BACKLOG:1484 |
| O254 | ESPINHA-E | Dá testes ao contrato de batidas e coerência à retenção. | - | Corte do primeiro pendente; O48 passo 5; pendente 2 | +++....+. | REGISTRADA (marco B fechado; pendente permanece) | BACKLOG:1486 | [nao verificado]
| O257 | ESPINHA-E | Centraliza a confirmação de entrega de uma competência. | - | O252; O48 passo 5; censo dos leitores | ++....... | REGISTRADA | BACKLOG:1487 |
| O258 | ESPINHA-E | Substitui decisões espalhadas por consultas aos juízes oficiais. | - | Item 2; três predicados novos; duas listas vazias | ++~....~+ | REGISTRADA (item 1 publicado; restante pendente) | BACKLOG:1488 | [nao verificado]
| O260 | LATERAL-ESTRUTURA | Faz a consulta de retenções devolver o formato prometido. | - | Fatia própria; preservar retorno sem dias | ++~~..... | REGISTRADA (outra linha informa pouso) | BACKLOG:1490; BACKLOG:1494 | [nao verificado]
| SOBRA-NA-BORDA-O-QUE-FICOU | ESPINHA-E | Reúne problemas restantes após mudanças em batidas antigas. | - | Obra própria do item a; censos por classe | ++....... | REGISTRADA | BACKLOG:1493 |
| BO-FERIADO-VINCULO-ANTIGO | LATERAL-ESTRUTURA | Alinha vínculos antigos à decisão sobre trabalho em feriados. | - | Casos mistos e postos encaminhados ao DP | +++...... | REGISTRADA (aplicado; casos do DP pendentes) | BACKLOG:1495 | [nao verificado]
| C7-DISPUTA-FECHA-POR-UM-JUIZ | ESPINHA-E | Faz disputas encerrarem por uma única decisão verificável. | - | C6; escritor único de chamado; lotes após L0 | ++~...... | REGISTRADA (construção antiga) | BACKLOG:1496 |
| BUG-DIA-SEM-SAIDA | ESPINHA-E | Oferece ação aos dias retidos sem pergunta viva. | - | Medições residuais; casos do DP | +++....++ | REGISTRADA (partes publicadas; resíduo pendente) | BACKLOG:1497 | [nao verificado]
| BUG-RETRATO-VELHO-NA-TELA | UI | Avisa quando o retrato da folha está desatualizado. | - | O48 passo 6 após 20/20; medição residual | ++~...... | REGISTRADA | BACKLOG:1498 |
| C7-TELA-ADMIN-ENCERRA | UI | Permite encerrar uma disputa com motivo obrigatório. | - | Porta admin_encerra; smoke das duas cascas | ++....... | REGISTRADA | BACKLOG:1499 |
| BO-ATESTADO-409-MUDO | LATERAL-ESTRUTURA | Confirma reenvio de atestado e explica recusas reais. | - | Recontagem 48 h; reenvio real; app; linha HAIKU | .++....++ | REGISTRADA (API publicada; verificações pendentes) | BACKLOG:1500 | [nao verificado]
| BO-B-DIAPAGO-SE-CONTRADIZ | ESPINHA-E | Impede campos incompatíveis de horas no mesmo dia. | - | BO-A; ! do motor; impacto por rubrica | ++....... | REGISTRADA | BACKLOG:1501 |
| PORTA-CELULA-EDITADA | LATERAL-ESTRUTURA | Permite registrar pela tela o turno autorizado do dia. | - | ? (não indicada) | .+....... | REGISTRADA | BACKLOG:1502 |
| INTERVALO-DECLARADO | LATERAL-ESTRUTURA | Pergunta sobre intervalo incompleto e registra a resposta aprovada. | - | BOs em curso; sinal da célula; validação | ++....... | REGISTRADA | BACKLOG:1503 |
| col155-HE-NAO-POR-MINUTO | UI | Limita a recusa de horas extras aos minutos decididos. | - | Caso pela regra; prova de dinheiro zero | ++....... | REGISTRADA | BACKLOG:1504 (id anonimizado) |
| VARREDURA-VIRA-EVENTO-24-CRONS | ESPINHA-E | Faz tarefas reagirem aos eventos e deixa varreduras vigiarem. | - | O168; O170 C1; C7; medição zero por classe | ++~....~+ | REGISTRADA (migração parcial) | BACKLOG:1505 |
| CONTRATOS-RESTANTES-E1-E2-FOLHA-K6 | ESPINHA-E | Alinha escala e folha às mesmas autoridades. | - | Exportação da 10 para E1; corte de folha | ++~....~+ | PARADA (restantes esperam exportação e lei) | BACKLOG:1506 |
| E6-NONE-E-CADASTRO | ESPINHA-E | Encaminha ao cadastro o dia sem vínculo definido. | - | ? (não indicada) | +++...... | REGISTRADA (marcada FEITA, sem encerramento literal) | BACKLOG:1507 |
| GRADE-ESC-DO-DIA-E1 | ESPINHA-E | Faz a previsão consultar o vínculo oficial do dia. | - | Suíte; pouso; smoke | +++...... | REGISTRADA (cura na cópia; publicação pendente) | BACKLOG:1508 | [nao verificado]
| CARIMBO-SEM-VIGENCIA | ESPINHA-E | Faz mudanças de vigência provocarem novo julgamento do dia. | - | Medir vínculos editados após o carimbo | +........ | REGISTRADA | BACKLOG:1509 |
| CONTADOR-PAREADOR-X-CELULA | ESPINHA-E | Detecta turno fechado cuja célula ainda acusa borda órfã. | - | Cura D2; comboio com TURNO-ABERTO item 3 | ++~...... | REGISTRADA (fontes divergem sobre publicação) | BACKLOG:1510; BACKLOG:1506 |
| CONTADOR-AUSENCIA-X-VEREDITO | ESPINHA-E | Detecta ausência aprovada contradita pelo julgamento do dia. | - | ! para quatro casos exportados | +++...... | PARADA (casos exportados esperam !) | BACKLOG:1511 |
| REGEN-EXPORTADA-SEM-EFEITO | LATERAL-ESTRUTURA | Permite corrigir células exportadas sem outros efeitos medidos. | - | Seis condições; reversão; janela de autorização | ++....... | REGISTRADA (validade atual da janela ?) | BACKLOG:1512 |
| TURNO-ABERTO-UM-EMISSOR | ESPINHA-E | Elimina decisões duplicadas sobre turnos e suas perguntas. | - | Lei do item 4; diagnósticos suspensos; corte de foto | ++~....~+ | PARADA (item 4 em lei; demais suspensos) | BACKLOG:1513 |
| ATA-NAO-DISPENSA-JUSTIFICADA | ESPINHA-E | Faz a ata reconhecer o período abonado pela justificativa. | - | Acesso para aplicar o acervo da 10 | ++++...++ | PARADA (cura publicada; aplicação sem permissão) | BACKLOG:1515 | [nao verificado]
| CONCORDE-PREVISTO-ZERO | ESPINHA-E | Investiga dias corretos com batidas e previsão zerada. | - | Medir a causa da previsão zero | ++....... | REGISTRADA | BACKLOG:1517 |
| VIVA-DELEGA-AO-JUIZ | ESPINHA-E | Faz a indicação de pergunta viva consultar o juiz comum. | - | Pouso; censo dos leitores | +++...... | REGISTRADA (construída; pouso pendente) | BACKLOG:1518 | [nao verificado]
| FOLGA-CONTESTA-DIZ-MANDAR-PARA-CADASTRO | UI | Explica que a folga contestada exige correção cadastral. | - | Raia UI; contador do caso col666 | ++....... | REGISTRADA | BACKLOG:1520 |
| Q-VIVAS-LEITORES-SEM-JUIZ | ESPINHA-E | Distingue perguntas realmente vivas das candidatas do filtro. | - | Medir o escritor de fechamento primeiro | +........ | REGISTRADA | BACKLOG:1521 |
| ESPELHO-VERDADE-REMEDIDO | ESPINHA-E | Atualiza o placar com testes e medições comprovados. | - | K6/E1; termômetro R1; antes da triagem | ++....... | REGISTRADA | BACKLOG:1522 | [nao verificado]
| VARREDURA-TRIAGEM | ESPINHA-E | Classifica tarefas entre vigia, prazo e evento. | - | Segunda medição; medir vínculo e termômetro | ++~....~+ | REGISTRADA (triagem e lote parciais) | BACKLOG:1523 |
| BUG-CELULA-SEM-VEREDITO | ESPINHA-E | Separa células reais sem julgamento dos dados de teste. | - | SEM-CELULA-NA-JANELA-INTEIRA | ++....... | REGISTRADA (cancelada; resíduo encaminhado) | BACKLOG:1524 | [nao verificado]
| BUG-FOLGA-COM-MARCOS | ESPINHA-E | Verifica folgas geradas com marcações indevidas. | - | Casos de dono BATIDA/CADASTRO | ++....... | REGISTRADA (premissa refutada) | BACKLOG:1525 |
| INTERMITENTE-BATIDA-SOLTA | LATERAL-ESTRUTURA | Investiga batida isolada de intermitente mostrada como dia correto. | - | Censo de frota; juiz de turno aberto | ++....... | REGISTRADA | BACKLOG:1526 |
| INTERMITENTE-K-FORA-DO-DNA | LATERAL-ESTRUTURA | Preserva o desenho histórico do intervalo do intermitente. | - | Medição e decisão | +........ | REGISTRADA | BACKLOG:1527 |
| INTERMITENTE-CONVOCACAO-CADASTRO | LATERAL-ESTRUTURA | Registra a convocação que define o trabalho do intermitente. | - | Cadastro; leitor na porta da célula | .+....... | REGISTRADA | BACKLOG:1528 |
| C5-LISTA-DE-FROTA | ESPINHA-E | Corrige faltas descontadas acima da jornada prevista. | - | Medir os 27 casos; invariante falta ≤ previsto | ++....... | REGISTRADA | BACKLOG:1529 |
| HAIKU-CELULA-E-FOLGA | INSTRUMENTO | Expõe contadores de células sem julgamento e folgas com marcos. | - | BUG-CELULA-SEM-VEREDITO; BUG-FOLGA-COM-MARCOS | .+....... | REGISTRADA | BACKLOG:1530 |
| FERIADO-DIA-CHAVE | ESPINHA-E | Faz cálculo e fechamento reconhecerem o mesmo dia de feriado. | - | Dia-chave único; caso atravessando meia-noite | ++....... | REGISTRADA | BACKLOG:1532 |
| BUG-117-COMPLEMENTO | ESPINHA-E | Impede cobrar novamente uma resposta já aplicada. | - | Fila anterior | ++....... | REGISTRADA | BACKLOG:1533 | [nao verificado]
| LISTA-POR-COMPETENCIA | ESPINHA-E | Separa problemas cadastrais por competência da folha. | - | Apply da competência 10 | .+....... | REGISTRADA | BACKLOG:1534 |
| O137-8932-ENTERRAR | LATERAL-ESTRUTURA | Remove uma cadeia antiga de abonos sem uso. | - | ? (não indicada) | .+....... | REGISTRADA | BACKLOG:1535 |
| O137-ESTA-EXPORTADA-K6 | ESPINHA-E | Faz a regularização consultar a autoridade de entrega da folha. | - | ? (não indicada) | .+....... | REGISTRADA | BACKLOG:1536 |
| PORTAO-FATURAMENTO-A1 | INSTRUMENTO | Aponta dias com problema sem caminho de resolução. | - | Apply da competência 10 | .+....... | REGISTRADA | BACKLOG:1537 |
| PORTAO-FATURAMENTO-RESTO | INSTRUMENTO | Confere condições de coerência antes do fechamento. | - | Lote EVENTO | .+....... | REGISTRADA | BACKLOG:1538 |
| SOMBRA-SH-PROSA-DA-LAVRA | INSTRUMENTO | Corrige a explicação do ensaio sobre a lavratura. | - | ? (não indicada) | +........ | REGISTRADA | BACKLOG:1539 |
| FALTA-ACIMA-DO-PREVISTO-[nome removido] | ? (dono não medido) | Investiga falta descontada acima da jornada prevista. | - | Medir dono pela L-099 | ++....... | REGISTRADA | BACKLOG:1540 (id anonimizado) |
| GEOFENCE-PERGUNTA-AO-JUIZ | ESPINHA-E | Faz alertas e localização usarem a mesma avaliação oficial. | - | DRY; declaração do juiz | ++~...... | REGISTRADA (construção antiga) | BACKLOG:1541 |
| AUTORIDADE-DE-FOTO-AUSENTE | ESPINHA-E | Centraliza a decisão sobre ausência de foto. | - | Autoridade de foto antes de retirar o julgador | .+....... | REGISTRADA | BACKLOG:1542 |
| CRON-SOMBRA-NAO-CABE | INSTRUMENTO | Investiga 

[truncado em 20000 de 52676 caracteres]

## 13/11 20:31 · sessao 565df2ae6e9a · - · - · origem=Ronald (tmux)

Mantidas as duas exclusões, com datas confirmadas por referência cruzada:

- SEM-CELULA-NA-JANELA-INTEIRA: encerramento pelo hash a391c6e4, publicado em 25/10 10:19. app/docs/BACKLOG.md:1089; app/docs/BACKLOG.md:1664.
- E5-FINAL: absorvida pela O-DIA-PAGO S4, fechada em 30/09 11:0x. app/docs/BACKLOG.md:1168; app/docs/BACKLOG.md:1129.

Contagem permanece: **69 incluídas e 41 linhas excluídas**.

## 13/11 20:31 · sessao 9badbaf3f25b · - · - · origem=Ronald (tmux)

| O222 | ESPINHA-E | Faz o dia terminar com o veredito coerente com a cobrança criada no mesmo ato. | - | deploy do ensaio | + + + + . . . . . | REGISTRADA (curada; deploy pendente) | app/docs/BACKLOG.md:1213 |
| O118 | LATERAL-ESTRUTURA | Faz a grade casar as batidas de todas as pausas do dia. | - | print do calendário | + + + . . . . + + | REGISTRADA (código no ar antigo; print pendente) | app/docs/BACKLOG.md:1233 |
| O2 | UI | Recusa no cartão uma data final posterior a ontem com uma mensagem clara. | - | ? (PAREI preservado em histórico truncado) | . . + . . . . . . | PARADA (fechada com PAREI preservado; motivo incompleto) | app/docs/BACKLOG.md:1255 |

Nova contagem: **111 obras examinadas; 60 incluídas; 51 excluídas**. Retirar O222, O118 e O2 da lista anterior de excluídas. Fontes: app/docs/BACKLOG.md:1213; app/docs/BACKLOG.md:1233; app/docs/BACKLOG.md:1255.

## 13/11 20:31 · sessao b1fb4ff003ff · - · - · origem=Ronald (tmux)

| MOTOR-LE-O-TURNO-INSTRUMENTO | INSTRUMENTO | Mede quantos cálculos usam par ou dia fora da autoridade oficial. | - | MOTOR-TURNO-INSTR-FILA; FATIA 1 | +-+-+‑.-.-+‑+‑+‑+ | REGISTRADA (pousado; fila pendente) | BACKLOG:1880 |
| FIO-E0-V4-E4A-2 | ESPINHA-E | Faz a fábrica agir por eventos e mostra os dias sem pergunta emitida. | - | O168/O169/LASTRO; fila de revisão | .-+-+‑.-.-+‑+‑+‑+ | REGISTRADA (no ar; fila pendente) | BACKLOG:1923 |

Contagem corrigida: 108 incluídas, quatro excluídas e duas duplicatas remetidas à consolidação. (BACKLOG:1877; BACKLOG:1990; BACKLOG:1880; BACKLOG:1923)

## 13/11 20:31 · sessao 159b8226e60b · - · - · origem=Ronald (tmux)

| F1 | UI | Reúne atestados e imagens dos anexos num PDF por período. | - | O30 F3 | . + . . . . . . . | REGISTRADA (fechada por absorção, sem data do fecho) | app/docs/BACKLOG.md:1303 |
| F2 | UI | Mostra as faltas que reduzem o direito a férias no período aquisitivo. | - | contratos 22/22; O157 | + + . . . . . . . | PARADA (portão contratual) | app/docs/BACKLOG.md:1304 |
| O24 | UI | Permite filtrar, reabrir e recalcular fechamentos pela tela. | - | exportação de 09; CARTAO=ESPELHO; O39 | . + . . . . . . . | REGISTRADA (superada pela O39, sem data do fecho) | app/docs/BACKLOG.md:1305; app/docs/BACKLOG.md:1320 |
| O27 | ESPINHA-E | Faz o cartão usar a janela correta e mostrar um total reconstituível pelas linhas. | - | decisão sobre total; O156 | + + . . . . . . . | PARADA (espera !) | app/docs/BACKLOG.md:1308 |
| O28 | LATERAL-ESTRUTURA | Desconta a saída antecipada conforme o catálogo e explica o desconto. | - | DIFF de dinheiro; ! | + + . . . . . . . | PARADA (espera !) | app/docs/BACKLOG.md:1309 |
| O29 | UI | Entrega os documentos dos atestados em relatório por colaborador ou empresa. | - | O30 F3 | . + . . . . . . . | REGISTRADA (fechada por absorção, sem data do fecho) | app/docs/BACKLOG.md:1310 |
| O30 | UI | Organiza o histórico de ausências e permite baixar documentos em lote. | - | vale da F1; pillow_heif; rebuild | . + ~ . . . . + + | PARADA (F1 no ar; F2/F3 aguardam vale) | app/docs/BACKLOG.md:1311 |
| O31 | INSTRUMENTO | Mantém a fabricação de correções andando com limites e alarmes visíveis. | - | fabricante; capacidade de produção | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1312 |
| O32 | INSTRUMENTO | Detecta a esteira parada e impede relançamentos repetidos sem causa. | - | O31; fabricante | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1313 |
| O33 | ESPINHA-E | Faz o total do cartão somar os minutos exibidos nas linhas. | - | O27; O156 | + + . . . . . . . | PARADA (espera ! da O27) | app/docs/BACKLOG.md:1314 |
| O34 | INSTRUMENTO | Dá destino às correções que envelhecem enquanto aguardam aplicação. | - | corte de desenho | + . . . . . . . . | PARADA (espera corte) | app/docs/BACKLOG.md:1315 |
| O35 | ESPINHA-E | Mostra e cobra o que falta para fechar os contratos estruturais. | - | O134; famílias pendentes | + + ~ + . . . . . | REGISTRADA (partes construídas; contratos ainda incompletos) | app/docs/BACKLOG.md:1316 |
| O36 | INSTRUMENTO | Impede que pacotes caídos sejam tratados como prontos ou publicados. | - | fabricante seco em obra própria | + + + + . . . . + | REGISTRADA (no ar sem data explícita do marco) | app/docs/BACKLOG.md:1317 |
| O37 | ESPINHA-E | Preserva a alternância do plantão quando existe uma folga avulsa no mês. | - | ! do passivo exportado | + + + . . . . . + | PARADA (cura no ar; passivo espera !) | app/docs/BACKLOG.md:1318 |
| O38 | LATERAL-ESTRUTURA | Explica por que uma resposta validada não virou marcação de ponto. | - | O39 para item a; smoke | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1319 |
| O39 | UI | Dá ao DP uma porta visível para reabrir a linha de fechamento. | - | O44; smoke nas duas cascas | . + . . . . . . . | REGISTRADA (suspensa pela O44) | app/docs/BACKLOG.md:1320; app/docs/BACKLOG.md:1325 |
| O40 | ESPINHA-E | Usa a competência correta ao decidir se um dia está trancado. | - | definição da fronteira FM/PeriodoFechado; O44 | + + ~ + . . . . + | REGISTRADA (tripwire no ar; fronteira pendente) | app/docs/BACKLOG.md:1321 |
| O42 | INSTRUMENTO | Corrige a seleção e os sinais de funcionamento da fábrica de correções. | - | fabricante; itens 1 e 2 | + + ~ + . . . . . | REGISTRADA (descrição diz 1/2 curados; estado diz em cura) | app/docs/BACKLOG.md:1323 |
| O43 | LATERAL-ESTRUTURA | Registra os atos e perdas de sessão do administrador para permitir auditoria. | - | O47; esteira seca | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1324 |
| O44 | ESPINHA-E | Permite ao administrador retomar um dia antigo sem destrancar outros dias. | - | smoke do caso 1; critérios do próprio item | + + + + . . . + . | REGISTRADA (pousada sem data explícita do marco) | app/docs/BACKLOG.md:1325 |
| O45 | ESPINHA-E | Faz tela e cartão mostrarem os mesmos dias em aberto. | - | cura na fonte identificada | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1326 |
| O46 | INSTRUMENTO | Encontra sinais de sucesso que não comprovam o fato anunciado. | - | esteira; bugs 2–5 | . + ~ . . . . . . | REGISTRADA (dois casos de abertura curados) | app/docs/BACKLOG.md:1327 |
| O47 | LATERAL-ESTRUTURA | Faz os atos administrativos guardarem usuário e endereço de origem. | - | O43 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1328 |
| O48 | ESPINHA-E | Faz o fechamento ler a situação atual do dia em vez de um cálculo envelhecido. | - | status encerrado; migration; ! do passo 5 | + + ~ . . . . + + | REGISTRADA (passos no ar; status e exportação pendentes) | app/docs/BACKLOG.md:1329 |
| O49 | ESPINHA-E | Impede que o intervalo cadastrado seja pago como hora extra indevida. | - | E2; DIFF; ! | + + . . . . . . . | PARADA (espera !) | app/docs/BACKLOG.md:1330 |
| O50 | ESPINHA-E | Impede vínculos com fim anterior ao início e encaminha o passivo. | - | saneamento dos 62; !; constraint | + + + . . . . . . | PARADA (passivo espera !) | app/docs/BACKLOG.md:1331 |
| O50-DESLIGAR-ENCERRA-TUDO | ESPINHA-E | Faz o desligamento encerrar vínculos, células futuras e pendências pela porta correta. | - | LEI-DESLIGADO; vaga serial; ! retroativo | + + . . . . . . . | REGISTRADA (censo concluído; construção na próxima vaga) | app/docs/BACKLOG.md:1332 |
| O50-CELULA-APOS-DESLIGAMENTO | ESPINHA-E | Impede a geração de trabalho previsto após o desligamento. | - | O50-DESLIGAR-ENCERRA-TUDO; ! retroativo | + + . . . . . . . | REGISTRADA (absorvida pela variante da O50) | app/docs/BACKLOG.md:1333; app/docs/BACKLOG.md:1332 |
| O50-COMPLEMENTO-LAUDO-1311 | ESPINHA-E | Preserva o trabalho anterior e elimina previsões futuras indevidas ao desligar. | - | O50-DESLIGAR-ENCERRA-TUDO; ! retroativo | + + . . . . . . . | REGISTRADA (absorvida pela variante da O50) | app/docs/BACKLOG.md:1334; app/docs/BACKLOG.md:1332 |
| O51 | ESPINHA-E | Mostra a saída antecipada em coluna própria no cartão. | - | E4 | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1335; app/docs/BACKLOG.md:1338 |
| O52 | ESPINHA-E | Impede que o cartão rotule folga como dia abonado por treino. | - | E4; leitura da célula | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1336; app/docs/BACKLOG.md:1338 |
| O53 | ESPINHA-E | Faz cálculo e telas usarem uma única interpretação das marcações do dia. | - | quatro itens da FILA; O37 antes da fase 2 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1337 |
| O54 | ESPINHA-E | Faz previsão, cálculo, telas e fechamento concordarem sobre cada dia. | - | portas E0→E6 provadas em sequência | + + ~ . . . . . . | REGISTRADA (programa parcial; curso de 13/11 não demonstrado para este id) | app/docs/BACKLOG.md:1338 |
| O55 | INSTRUMENTO | Evita que a passagem da meia-noite bloqueie o push por uma idade gravada. | - | renderizador do placar | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1339 |
| O57 | INSTRUMENTO | Impede que um commit leve alterações preparadas por outra sessão. | - | decisão da guarda de commit | + . ~ . . . . . . | REGISTRADA (selos restaurados; causa de desenho pendente) | app/docs/BACKLOG.md:1341 |
| O58 | LATERAL-ESTRUTURA | Registra a emissão do espelho e permite auditá-lo por período livre. | - | parecer jurídico para hash; Card 4 | + + . . . . . . . | PARADA (hash espera parecer; registro ainda pode avançar) | app/docs/BACKLOG.md:1342 |
| O59 | UI | Monta o dossiê de rescisão a partir dos documentos já emitidos. | - | O58; UI-RESCISAO | . + . . . . . . . | REGISTRADA (absorvida em 13/11 pela UI-RESCISAO) | app/docs/BACKLOG.md:1343 |
| UI-RESCISAO | UI | Reúne a liquidação e os documentos do desligamento numa única tela. | - | pós-20; O58; três perguntas de lei | + + . . . . . . . | REGISTRADA (censo pronto; sem brief demonstrado) | app/docs/BACKLOG.md:1344 |
| O60 | ESPINHA-E | Faz o motor calcular pelos períodos e pela jornada declarados pelos juízes. | - | DIFF; ! da competência 09 | + + . . . . . . . | PARADA (espera ! de critério) | app/docs/BACKLOG.md:1345 |
| O61 | ESPINHA-E | Mostra ao DP a diferença entre o TXT entregue e o motor curado da competência 09. | - | O60; identificação do TXT correto | + + . . . . . . . | REGISTRADA (07/08 excluídas do acerto por corte) | app/docs/BACKLOG.md:1346 |
| O62 | ESPINHA-E | Faz o juiz reconhecer o intervalo cadastrado mesmo quando faltam marcos congelados. | - | corte de alcance do juiz | + . . . . . . . . | PARADA (espera corte) | app/docs/BACKLOG.md:1347 |
| O63 | ESPINHA-E | Impede que a folha pague folga de modo diferente por faltar um arquivo na sombra. | - | corte sobre a fonte da decisão financeira | + . . . . . . . . | PARADA (espera corte) | app/docs/BACKLOG.md:1348 |
| O64 | INSTRUMENTO | Explica a divergência do contador de inconsistências entre produção e sombra. | - | investigação de ambiente; ! | + . . . . . . . . | PARADA (apply espera explicação) | app/docs/BACKLOG.md:1349 |
| O65 | ESPINHA-E | Faz a geometria do turno ler o papel que a ata atribuiu às batidas. | - | O134 item 1; O142 para classe C | + + + . . . . . . | REGISTRADA (BACKLOG aberto; placar diz aplicada) | app/docs/BACKLOG.md:1350; app/core/espelho_verdade.py:77 |
| O66 | ESPINHA-E | Garante um caminho de resolução para todo dia ainda em furo ou cobrança. | - | O65; ! do passivo | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1351 |
| O67 | ESPINHA-E | Corrige classes de dias com tipo errado, julgamento prematuro ou abono em folga. | - | O66; medições por classe | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1352 |
| O67b | ESPINHA-E | Faz os totais, datas e rótulos do cartão corresponderem ao trabalho apurado. | - | O66; O67; medição de frota | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1353 |
| O68 | ESPINHA-E | Impede pagamento silencioso de dias que a ata não explica. | - | O71; O65; ! da parte b | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1354 |
| O51b | ESPINHA-E | Mostra atraso e saída antecipada com os mesmos valores da folha. | - | O68; antes do envio de 09 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1355 |
| O70 | ESPINHA-E | Faz toda célula informar qual vínculo gerou a previsão daquele dia. | - | O68; DIFF; ! se mudar número | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1357 |
| O71 | ESPINHA-E | Atualiza os vereditos antigos antes de decidir quem entra na folha. | - | DIFF; ! se alguém sair do TXT | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1358 |
| O72 | ESPINHA-E | Impede que uma saída após meia-noite vire desconto de um dia inteiro. | - | antes do apply da O68; censo de frota | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1359 |
| O73 | ESPINHA-E | Impede cobrança de atraso na volta da pausa e certifica os resíduos do intervalo. | - | bateria; baldes restantes; L-115 | + + ~ . . . . + . | REGISTRADA (núcleo FEITO no placar; complemento pendente) | app/docs/BACKLOG.md:1360; app/core/espelho_verdade.py:117 |
| O74 | UI | Mostra o tipo de turno conforme sua definição real. | - | smoke | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1361 |
| O75 | INSTRUMENTO | Faz o DIFF revelar todos os campos financeiros alterados. | - | medidores de fechamento | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1362 |
| O76 | ESPINHA-E | Atribui o plantão ao dia de início mesmo com entrada após meia-noite. | - | censo de frota | + + + + . . . . . | REGISTRADA (BACKLOG aberto; placar diz aplicada em 27/09) | app/docs/BACKLOG.md:1363; app/core/espelho_verdade.py:80 |
| O77 | INSTRUMENTO | Cobra a aplicação da regra de pontualidade com dois casos opostos. | - | L-084 | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1364 |
| O79 | INSTRUMENTO | Impede que leitores confundam falta descontável com pergunta de marco faltante. | - | censo AST; L-088 | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1366 |
| O88 | ESPINHA-E | Evita que cadastro deslocado impeça o pareamento de um dia trabalhado. | - | O86; L-084 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1368 |
| O87 | ESPINHA-E | Recusa cadastro com intervalo fora do turno e declara o passivo existente. | - | push E1; ! para corrigir os sete templates | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1369 |
| O86 | ESPINHA-E | Separa saída realmente ausente de falha no pareamento do turno. | - | O85; O84; causas O87/O88 | + + ~ + . . . . . | REGISTRADA (guarda com DIFF zero; investigação residual) | app/docs/BACKLOG.md:1370 |
| O95 | ESPINHA-E | Impede absorver uma saída real como pausa sem prova de retorno. | - | censo; DIFF de frota | + . . . . . . . . | PARADA (cura candidata revertida; espera censo) | app/docs/BACKLOG.md:1371 |
| O96 | LATERAL-ESTRUTURA | Permite cadastrar o limite de pausa que hoje está fixo no código. | - | exportação de 09 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1372 |
| O94 | ESPINHA-E | Submete à decisão o pagamento antigo de turnos sem saída. | - | ! sobre o gravado | + + + . . . . . + | PARADA (código no ar; gravado espera !) | app/docs/BACKLOG.md:1373 |
| O93 | ESPINHA-E | Impede que trabalho em folga herde um dia anterior sem turno real. | - | decisão sobre o dinheiro de 09 | + + . . . . . . . | PARADA (apply parado) | app/docs/BACKLOG.md:1374 |
| O92 | ESPINHA-E | Alimenta a previsão em lote para evitar consultas repetidas por dia. | - | quatro leitores; DIFF da cópia inline | + + ~ . . . . . . | REGISTRADA (chamador da tela fechado; demais pendentes) | app/docs/BACKLOG.md:1375 |
| O90 | INSTRUMENTO | Evita que o hash gravado do último push bloqueie sempre o push seguinte. | - | renderizador; régua de tickets | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1377 |
| O89 | ESPINHA-E | Mantém o resultado igual ao recalcular com os mesmos objetos de batida. | - | ! de desenho segundo o BACKLOG | + + + + . . . . + | REGISTRADA (BACKLOG espera !; placar diz no ar em 27/09) | app/docs/BACKLOG.md:1378; app/core/espelho_verdade.py:109 |
| O85 | ESPINHA-E | Faz horas de dias sem vínculo cobrindo a data entrarem na apuração. | - | dado de vínculo pelo propositor; L-009 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1379 |
| O83 | ESPINHA-E | Separa por causa as horas que a folha deixa de reconhecer. | - | obras das classes A/B/C/D | + + . . . . . . . | REGISTRADA (curso atual deste id não demonstrado) | app/docs/BACKLOG.md:1381 |
| O82 | INSTRUMENTO | Permite preparar uma sombra válida entre meia-noite e o backup da madrugada. | - | deploy das sete curas | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1382 |
| O132 | INSTRUMENTO | Impede que medições ruins definam o horário dos crons. | - | decisão sobre amostras; filtro obrigatório | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1385 |
| O133 | INSTRUMENTO | Limpa o contexto ao fechar um marco e retoma pelo handoff. | - | janela sem tarefa viva; ciclo completo provado | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1386 |
| O136 | ESPINHA-E | Faz a escolha do motor reconhecer turno partido nos dois sentidos. | - | O134 item 5; DIFF; ! do motor | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1389 |
| O137 | INSTRUMENTO | Abre a revisão de cada família assim que os contratos dela fecham. | - | família verde; trava de testes | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1390 |
| O137-ESCALA | INSTRUMENTO | Revê código, leitores e documentação da família escala já fechada. | - | fim da fila de pouso | + + . . . . . . . | REGISTRADA (entregue a agente em registro antigo) | app/docs/BACKLOG.md:1391 |
| O138 | INSTRUMENTO | Deriva o hash do git na leitura para evitar um rodapé sempre vencido. | - | primeira janela sem fila 1 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1392 |
| O139 | ESPINHA-E | Distingue crons que só cobram prazo daqueles que julgam por conta própria. | - | selo de host no pouso 2 | + + ~ . . . . + + | REGISTRADA (produto no ar; selo pendente) | app/docs/BACKLOG.md:1393 |
| CHAMADO-EM-RAIA | ESPINHA-E | Concentra as mudanças de estado dos chamados numa única porta. | - | O123; régua da trilha principal | . + . . . . . . . | REGISTRADA (voo só registrado em 03/10) | app/docs/BACKLOG.md:1394 |
| O141 | ESPINHA-E | Faz aprovação e entrega de fechamento usarem as autoridades declaradas. | - | baixa justificada de K4; K6 | + + ~ . . . . . . | REGISTRADA (K1 curado; K4/K6 pendentes de registro) | app/docs/BACKLOG.md:1395 |
| DOSSIES-DAS-CELULAS | ESPINHA-E | Organiza a cura dos contratos e dos leitores das famílias estruturais. | - | ordem O134; obras dos seis itens | + + ~ . . . . . + | REGISTRADA (parte no ar; restante sem curso atual comprovado) | app/docs/BACKLOG.md:1396 |
| SITUACAO-E-LAMPADA | ESPINHA-E | Faz os leitores interpretarem o afastamento pela autoridade correta. | - | censo dos dez filtros; migração dos leitores | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1397 |
| O142 | ESPINHA-E | Faz o mesmo dia produzir o mesmo turno em janelas diferentes. | - | vínculo do dia; provas de invariância | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1398 |
| O143 | LATERAL-ESTRUTURA | Encaminha ao administrador os dias trabalhados sem vínculo que os cubra. | - | censo de frota; nome da palavra; pauta com prazo | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1399 |
| O144 | LATERAL-ESTRUTURA | Faz duas telas contarem os mesmos dias para o limite de atestados da empresa. | - | prova do leitor absenteísmo; censo de derivações | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1400 |
| O147 | LATERAL-ESTRUTURA | Mede o efeito financeiro dos atrasos ainda lançados na competência 09. | - | !; porta REGEN-EM-EXPORTADA | . + . . . . . . . | PARADA (competência exportada) | app/docs/BACKLOG.md:1401 |
| O148 | UI | Permite administrar calendários de folgas sem duplicidade e com busca adequada. | - | O152; medição; foto contínua | + + ~ . . . . + + | REGISTRADA (item 1 no ar; restante não começado) | app/docs/BACKLOG.md:1402 |
| O149 | LATERAL-ESTRUTURA | Mostra ao administrador os vínculos cujo fim vem antes do início. | - | lista administrativa; ! apenas para mudar dado | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1403 |
| O150 | INSTRUMENTO | Fecha o caso que permite ao hook encerrar com fila estrutural pendente. | - | reprodução do furo | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1404 |
| O151 | LATERAL-ESTRUTURA | Julga os chamados nascidos do rejulgamento forçado e retrata os sem lastro. | - | porta de retratação; trilha | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1405 |
| O152 | UI | Oferece uma busca comum que procura o mesmo texto mostrado nas listas. | - | O148; depois dos BOs | + + . . . . . . . | REGISTRADA (parte da obra conjunta com O148) | app/docs/BACKLOG.md:1406 |
| O155 | INSTRUMENTO | Confere se os dias previstos no fechamento correspondem aos dias de trabalho da célula. | - | item em curso; medição antes de cura | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1408 |
| O156 | ESPINHA-E | Faz o cartão totalizar exatamente os valores que imprime nas linhas. | - | item em curso; O27/O33 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1409 |
| O157 | LATERAL-ESTRUTURA | Conta os tipos corretos de ausência ao projetar o direito de férias. | - | DIFF por colaborador; catálogo | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1410 |
| O159 | LATERAL-ESTRUTURA | Faz ausência de dia inteiro usar o previsto do dia em vez de 720 minutos fixos. | - | DIFF; reversão; prova posterior | + + . . . . . . . | REGISTRADA (! concedido; construção não iniciada) | app/docs/BACKLOG.md:1412 |
| O153 | INSTRUMENTO | Confere se um colaborador consegue acessar dados de outro. | - | cadastro de rotas públicas; selo de autorização | + . . . . . . . . 

[truncado em 20000 de 22923 caracteres]

## 13/11 20:30 · sessao 8ff1b9749296 · - · - · origem=Ronald (tmux)

| O169-JUIZES-760 | ESPINHA-E | Evita aceitar encerramento sem prova de aplicação. | - | medir frota | ......... | REGISTRADA | BACKLOG:1991 |
| REVERTER-LEITOR-JANELAS | LATERAL-ESTRUTURA | Faz a leitura reconhecer também as regenerações desfeitas. | - | - | ......... | REGISTRADA | BACKLOG:1992 |
| REVERTER-SEM-SELECT-FOR-UPDATE | LATERAL-ESTRUTURA | Evita alterações simultâneas durante a reversão. | - | medir ordem das travas | ......... | REGISTRADA | BACKLOG:1993 |
| REVERTER-VOLTA-TRILHA-NA-PORTA | LATERAL-ESTRUTURA | Registra na reversão a autorização usada para alterar o dia. | - | - | ......... | REGISTRADA | BACKLOG:1994 |
| REVERTER-IDA-TRILHA-FRACA | LATERAL-ESTRUTURA | Guarda prova completa de cada aplicação e evita trabalho repetido. | - | - | ......... | REGISTRADA | BACKLOG:1995 |
| REVERTER-ANTES-LOG-ANTES-DO-JSON | LATERAL-ESTRUTURA | Evita registrar uma cópia de reversão que não foi salva. | - | - | ......... | REGISTRADA | BACKLOG:1996 |
| O169-NAO-MEDIDO-ESCAPA | ESPINHA-E | Mostra os encerramentos que o contador não conseguiu avaliar. | - | - | .+....... | REGISTRADA | BACKLOG:1997 |
| O169-RC2-ENGOLIDO | INSTRUMENTO | Faz o alerta de falha chegar ao painel da esteira. | - | - | ......... | REGISTRADA | BACKLOG:1998 |
| O169-TESTE-ORM-RC2 | INSTRUMENTO | Prova o alerta usando banco de teste real. | - | - | ......... | REGISTRADA | BACKLOG:1999 | [nao verificado]
| O169-DONO-VOCABULARIO | INSTRUMENTO | Usa no diagnóstico os mesmos responsáveis do placar estrutural. | - | - | .+....... | REGISTRADA | BACKLOG:2000 |
| O169-CRONS-MD-ALARME | INSTRUMENTO | Alinha a documentação ao comando de alerta instalado. | - | marco do pouso | ......... | REGISTRADA | BACKLOG:2001 | [nao verificado]
| O169-V5-GRAVAR-ESTADO-SEM-TRILHA | ESPINHA-E | Deixa histórico de cada mudança de estado do chamado. | - | - | ......... | REGISTRADA | BACKLOG:2002 |
| O169-V5-SEM-TESTE-CONCORRENTE | INSTRUMENTO | Prova o comportamento com dois aplicadores simultâneos. | - | - | ......... | REGISTRADA | BACKLOG:2003 | [nao verificado]
| PERGUNTA-VIVA-COM-CELULA-ABSOLVEU | LATERAL-ESTRUTURA | Trata a pergunta que continua viva após a absolvição do dia. | - | - | ......... | REGISTRADA | BACKLOG:2004 |
| PLACAR-A5-RC2-SEM-ALARME | INSTRUMENTO | Impede que uma falha de comando apareça como painel normal. | - | - | ......... | REGISTRADA | BACKLOG:2005 |
| PLACAR-FABRICA-UNIDADE | INSTRUMENTO | Distingue perguntas de pautas na contagem publicada. | - | - | ......... | REGISTRADA | BACKLOG:2006 | [nao verificado]
| E4A2-JANELA-POR-EMPRESA | ESPINHA-E | Considera o fechamento próprio de cada empresa na vigilância. | - | - | ......... | REGISTRADA | BACKLOG:2007 |
| E4A2-LIMBO-EXPORTADO-COMO-ALVO | ESPINHA-E | Evita cobrar como falha estrutural o que já foi enviado ao DP. | - | - | ......... | REGISTRADA | BACKLOG:2008 |
| E4A2-SELO-6G-SUBSTRING | INSTRUMENTO | Faz o teste conferir a contagem exata do alerta. | - | - | ......... | REGISTRADA | BACKLOG:2009 | [nao verificado]
| O169-FECHADA | ESPINHA-E | Impede encerrar chamados apenas pela idade e trata o legado. | - | pouso E4a-2; corte de desenho | ......... | PARADA (B3 desenho) | BACKLOG:2010 | [nao verificado]
| SELO-MENSAGEM-NO-COMMIT-MSG | INSTRUMENTO | Confere a mensagem antes de gravar o commit. | - | série E | ......... | REGISTRADA | BACKLOG:2011 | [nao verificado]
| PAUTA-SEM-TURNO-PREFIXO-RESERVADO | INSTRUMENTO | Evita que texto livre se passe por pauta automática. | - | corte de desenho | ..++..... | PARADA (congelada) | BACKLOG:2012 |
| PAUTA-PREFIXO-IDENTIDADE-NO-TEXTO | INSTRUMENTO | Evita identificar pautas erradas por trechos ambíguos do texto. | - | corte de desenho | ......... | PARADA (congelada) | BACKLOG:2013 |
| PASSADO-SEM-TURNO-SEM-PAUTA-NAO-CONTADO | ESPINHA-E | Conta os dias passados que ficaram sem pergunta nem pauta. | - | - | ......... | REGISTRADA | BACKLOG:2014 |
| E3-ENTRADA-ORFA-SEM-TURNO | 20/20 | Define o tratamento da entrada real sem turno previsto. | - | lei para dia sem turno com batida | ......... | PARADA (aguarda lei) | BACKLOG:2016 |
| ACHADO-POR-CASA-TOKEN-EM-PROSA | INSTRUMENTO | Evita que citar um bug no texto bloqueie um envio indevidamente. | - | série E; contrato | ......... | REGISTRADA | BACKLOG:2017 |
| BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS | LATERAL-ESTRUTURA | Permite julgar juntos a saída e o retorno do intervalo. | - | condições do passivo exportado | +++++~~.. | EM VOO | BACKLOG:2018; app/docs/RELATO.md:18; app/docs/RELATO.md:36; app/docs/RELATO.md:42 |
| BUG-PAR-ESTADO-EXPLICITO | LATERAL-ESTRUTURA | Torna explícita a espera pela outra metade do intervalo. | - | BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS | ......... | REGISTRADA (superada e absorvida) | BACKLOG:2019 |
| SONDA-SEM-GRANT | INSTRUMENTO | Faz a sonda de leitura consultar sem alterar permissões. | - | próxima vaga | ......... | REGISTRADA | BACKLOG:2020 |
| SONDA-MATRIZ-DE-TRANSICOES | INSTRUMENTO | Expõe as transições e permissões antes de retomar a sonda. | - | CODEX-2-RAIAS | +........ | REGISTRADA | BACKLOG:2021 |
| AUDITORIA-0911 | INSTRUMENTO | Separa as causas dos resíduos e identifica o responsável por cada classe. | - | SONDA-SEM-GRANT | ......... | REGISTRADA | BACKLOG:2022 |
| SEGREDO-ARGV-RESIDUO | INSTRUMENTO | Retira senhas dos argumentos visíveis dos processos. | - | SONDA | .+....... | REGISTRADA | BACKLOG:2023 |
| SOMBRA-CARIMBO-MENTE | INSTRUMENTO | Impede certificar uma sombra alterada sem invalidar sua prova. | - | SONDA | ......... | REGISTRADA | BACKLOG:2024 |
| SOMBRA-HOJE-NA-BORDA | INSTRUMENTO | Usa o dia correto mesmo quando a execução espera até depois da meia-noite. | - | - | ......... | REGISTRADA | BACKLOG:2025 |
| PLACAR-RETIDO-DO-LASTRO | INSTRUMENTO | Faz o placar representar corretamente a retenção pelo lastro. | - | medir pela porta | ......... | REGISTRADA | BACKLOG:2026 |
| CODEX-2-RAIAS-PORTAO-CPU | INSTRUMENTO | Abre uma segunda construção apenas quando há capacidade disponível. | - | matriz; suite; verificações | ..+...... | REGISTRADA | BACKLOG:2027 | [nao verificado]
| CODEX-ALIVIA-1-WRAPPER-BIN | INSTRUMENTO | Permite construir em bin preservando os arquivos protegidos. | - | CODEX-2-RAIAS | ......... | REGISTRADA | BACKLOG:2028 |
| CODEX-ALIVIA-3-ESMERIL | INSTRUMENTO | Entrega ao responsável uma lista agrupada de problemas do código. | - | ALIVIA-1; contrato; capacidade | ......... | REGISTRADA | BACKLOG:2029 |
| PECAS-APOSENTAVEIS-NO-ESTADO | INSTRUMENTO | Mostra as peças sem uso e os limites dessa medição. | - | CODEX-2-RAIAS; fila estrutural | ......... | REGISTRADA | BACKLOG:2030 |
| PLANTIO-SILENCIO-SEM-DIA | LATERAL-ESTRUTURA | Dá destino visível ao plantio descartado por faltar o dia do turno. | - | desenho da obra mãe | ......... | REGISTRADA | BACKLOG:2031 |
| PLANTIO-DIA-CIVIL-VIZINHO | ESPINHA-E | Usa o dia do turno no lugar da data civil da batida. | - | desenho da obra mãe | ......... | REGISTRADA | BACKLOG:2032 |
| PWA-FILA-OFFLINE-400 | UI | Evita perder silenciosamente batidas recusadas ou não enviadas. | - | sonda da quantidade | ......... | REGISTRADA | BACKLOG:2033 |
| CXR-NA-FILA | INSTRUMENTO | Mostra quais obras cobrem as classes ainda abertas do comparativo. | - | - | ......... | REGISTRADA | BACKLOG:2034 |
| G6-PAGINA | UI | Mantém a página da lista depois de uma ação administrativa. | - | vaga; smoke nas duas cascas | ......... | REGISTRADA | BACKLOG:2035 |
| O10-TROCA-DE-PLANTAO-ENTRA | LATERAL-ESTRUTURA | Permite corrigir trocas históricas pela porta existente. | - | O10-C; lista do admin | .+++..++. | REGISTRADA (B no ar; C pendente) | BACKLOG:2036 |
| O10-B-FILA | LATERAL-ESTRUTURA | Fecha lacunas de concorrência, autorização e histórico nas trocas. | - | - | .+....... | REGISTRADA | BACKLOG:2037 |
| O10-D-PERMUTA-RECIPROCA | LATERAL-ESTRUTURA | Troca os plantões de duas pessoas como uma única operação segura. | - | fila 1 | ++....... | REGISTRADA | BACKLOG:2038 |
| RETRATO-POR-EVENTO | ESPINHA-E | Atualiza o retrato da folha quando um ato altera a apuração. | - | E2; E3 | +........ | REGISTRADA | BACKLOG:2039 |
| AKITA-MERGE-SEM-LINHA | INSTRUMENTO | Confere também os merges que não declaram a linha de padrão. | - | contrato | ......... | REGISTRADA | BACKLOG:2041 |
| BO-0511-G2-TRIAGEM | INSTRUMENTO | Classifica os casos do BO e mostra o destino de cada um. | - | - | +........ | REGISTRADA (medida pronta) | BACKLOG:2042 |
| E-SEM-VIVA-EMPRESA-1 | INSTRUMENTO | Verifica se a empresa de teste deve entrar na medição operacional. | - | confirmar operação da empresa | ......... | REGISTRADA | BACKLOG:2043 | [nao verificado]
| E-SEM-VIVA-CACA | INSTRUMENTO | Procura a causa dos dias sem pergunta viva e aponta seu destino. | - | lei; E-SEM-VIVA-EMPRESA-1 | +........ | REGISTRADA (medida pronta) | BACKLOG:2044 |
| E-SEM-VIVA-B | LATERAL-ESTRUTURA | Mostra ao admin os dias retentores anteriores ao desligamento. | - | LEI-DESLIGADO-NAO-TRAVA | .++...++. | REGISTRADA (fechada; parte superada em 13/11; B3 antigo conflitante) | BACKLOG:2045 |
| G1-PORTA | INSTRUMENTO | Identifica quais dias retêm os casos e se existe ação disponível. | - | - | +........ | REGISTRADA (medida pronta) | BACKLOG:2046 |
| H12H06-B-FILA-OPUS | LATERAL-ESTRUTURA | Expõe lacunas de ausência, cálculo e cobertura ainda não tratadas. | - | medir alcance | ......... | REGISTRADA | BACKLOG:2047 |
| G3G4-ESTRUTURA-RESIDUO | LATERAL-ESTRUTURA | Dá uma obra aos resíduos estruturais que ficaram sem cobertura. | - | nomear padrão | .+....... | REGISTRADA | BACKLOG:2048 |
| C20-O168 | 20/20 | Impede fechar o chamado pai enquanto uma resposta ainda não foi julgada. | - | - | +++++++++ | NO AR hoje | BACKLOG:2049; app/docs/RELATO.md:5; app/docs/RELATO.md:7; anexo:relato_1311.md:180 |
| F3-BECO-1-VOLTA-3 | ESPINHA-E | Rejulga o mês corrigido e reata perguntas apenas ao pai na fila. | - | fila do opus itens 1–8 | .+++.+++. | REGISTRADA (no ar antigo; fila pendente) | BACKLOG:2050 |
| F3-BECO-2-COBRANCA-MORTA | ESPINHA-E | Trata os dias que continuam sem caminho por cobrança morta. | - | - | ......... | REGISTRADA | BACKLOG:2051 |
| C6-IDENTITY-MAP | LATERAL-ESTRUTURA | Reduz consultas repetidas ao montar a lista de perguntas. | - | - | ......... | REGISTRADA | BACKLOG:2052 |
| CRONS-MARCA-RELOGIO | INSTRUMENTO | Faz a marca temporária dos crons usar o relógio da execução. | - | marca termina em 20/11 | ......... | REGISTRADA | BACKLOG:2053 |
| CRIVO-AINDA-VALE | INSTRUMENTO | Identifica textos antigos que prometem comportamento diferente do código. | - | LASTRO; MODO-COMUM; BATERIA; contrato | ......... | REGISTRADA | BACKLOG:2054 |
| CODEX-FUNIL-28 | INSTRUMENTO | Filtra o inventário para destacar erros de valor sem obra correspondente. | - | - | ......... | REGISTRADA | BACKLOG:2055 |
| FUNIL-28-PROCEDENTES | ? (áreas não discriminadas) | Guarda os achados procedentes para abrir obras com padrão definido. | - | nomear padrão por obra | ......... | REGISTRADA | BACKLOG:2056 |
| G3G4-DONO-MEDIDO | INSTRUMENTO | Classifica os casos por responsável e encaminha os resíduos estruturais. | - | G3G4-ESTRUTURA-RESIDUO | +........ | REGISTRADA (medida pronta) | BACKLOG:2057 |
| CODEX-RED-POR-EXEMPLO | INSTRUMENTO | Entrega exemplos da regra para orientar os testes do construtor. | - | abertura das obras | ......... | REGISTRADA | BACKLOG:2058 | [nao verificado]
| F-346-FILA-OPUS | LATERAL-ESTRUTURA | Trata lacunas de concorrência, histórico e erro na remarcação de férias. | - | nomear padrão por item | ......... | REGISTRADA | BACKLOG:2059 |
| CODEX-PRE-CODIGO-DUPLO | INSTRUMENTO | Compara duas leituras antes da construção das obras selecionadas. | - | construtor ainda não aberto | ......... | REGISTRADA | BACKLOG:2060 |
| JV1-MERGE-FILA | ESPINHA-E | Fecha lacunas de janela e alimentação dos feriados nos leitores. | - | medir dinheiro antes da alteração | ......... | REGISTRADA | BACKLOG:2061 |
| MARCO-DE-CLEAR-NAO-GRAVOU | INSTRUMENTO | Faz o contador de contexto reconhecer a limpeza efetivamente realizada. | - | vaga de instrumento; contrato | +........ | REGISTRADA | BACKLOG:2062 |
| TOAST-B-SELO-FINO | INSTRUMENTO | Prova os limites e os códigos de erro exibidos nos avisos. | - | - | ......... | REGISTRADA | BACKLOG:2063 |
| BUG-SW-RESPOND-UNDEFINED | UI | Devolve uma resposta válida quando a rede falha sem cópia local. | - | smoke real de batida pela PWA | ..+....++ | REGISTRADA (no ar antigo; homologação pendente) | BACKLOG:2064 |
| SELOS-HOST-VERMELHOS-NO-MAIN | INSTRUMENTO | Mede e trata os selos de host que permanecem vermelhos. | - | medição pela régua | ......... | REGISTRADA | BACKLOG:2065 |
| E4-8-FORA-VINCULO | ESPINHA-E | Identifica dias cobrados fora da vigência do vínculo. | - | medir frota | .+....... | REGISTRADA | BACKLOG:2066 |
| E4-8-COMP-ANTIGA | ESPINHA-E | Mede a falta de certificação em competências antigas antes de usá-las. | - | medir competências 06–09 | ......... | REGISTRADA | BACKLOG:2067 |
| E4-8-FORMA | INSTRUMENTO | Alinha nomes e referências dos testes e leitores da E4-8. | - | - | ......... | REGISTRADA | BACKLOG:2068 | [nao verificado]
| E4-8-ROTULO-OK | LATERAL-ESTRUTURA | Faz o rótulo do dia explicar corretamente a ausência de cobrança. | - | medir células; aval para exportada | ......... | REGISTRADA | BACKLOG:2069 |
| FUROS-EMPRESA-TIMEOUT | LATERAL-ESTRUTURA | Evita que o relatório da empresa inteira ultrapasse o tempo da tela. | - | medir produção; execução em job | ......... | REGISTRADA | BACKLOG:2070 |
| TOAST-SELO-APELIDO | INSTRUMENTO | Prova que o selo detecta também escritas feitas por apelidos. | - | - | ......... | REGISTRADA | BACKLOG:2071 |
| OTIMIZA-DUPLA | INSTRUMENTO | Melhora testes, wrapper e verificações com um contrato por conjunto. | - | DEPLOY-ELO; cota Codex | ..~...... | REGISTRADA (item 0 feito; 1–5 pendentes) | BACKLOG:2072 | [nao verificado]
| OTIMIZA-2-WRAPPER-CODEX | INSTRUMENTO | Melhora a preparação e o retorno das chamadas ao Codex. | - | cota Codex | ......... | PARADA | BACKLOG:2073 |
| O261-E-F3-DORMENTE | ESPINHA-E | Dá caminho aos chamados dormentes sem reabrir ou encerrar indevidamente. | - | matriz de becos; classificação de dono | ......... | REGISTRADA | BACKLOG:2074 |
| CORTES-JSON-DEFASADO | INSTRUMENTO | Impede que regenerar o documento apague decisões existentes apenas no texto. | - | preenchimento do JSON; selo | ......... | REGISTRADA | BACKLOG:2075 |
| CONGELA-O-20 | INSTRUMENTO | Mantém o mapa limitado às obras autorizadas do conjunto fechado. | - | MAPA-20-DERIVADO, substituído pela GRADE-MESTRA | .+....... | REGISTRADA (destino alterado pelo aval) | BACKLOG:2076; anexo:aval.txt:1 |
| CICLO-C-CEL-TEMPLATE | LATERAL-ESTRUTURA | Trata uma possível espera circular entre cadastro e célula. | - | reproduzir e medir | ......... | REGISTRADA | BACKLOG:2077 |
| FAIXA-NO-CADASTRO | LATERAL-ESTRUTURA | Permite cadastrar a faixa de precedência do tipo de ausência. | - | corte para construir | ......... | REGISTRADA | BACKLOG:2078 |
| EFEITO-SEM-FALLBACK-CATALOGO | LATERAL-ESTRUTURA | Evita voltar silenciosamente ao efeito antigo quando a leitura falha. | - | medir alcance | ......... | REGISTRADA | BACKLOG:2079 |
| LASTRO-FILA-PREVERIF-C11 | LATERAL-ESTRUTURA | Expõe recusas e falhas hoje descartadas pelos leitores e montadores. | - | EFEITO-SEM-FALLBACK-CATALOGO | ......... | REGISTRADA | BACKLOG:2080 |
| BO-ROTULO-REABRIR-SO-NO-DETALHE | UI | Oferece apenas mudanças de estado que a ação realmente executa. | - | corte para construir | +........ | REGISTRADA | BACKLOG:2081 |
| PROPORCAO-FILA-OPUS-V11 | INSTRUMENTO | Fecha lacunas de alcance e consumo do resultado do selo. | - | AUDITORIA-DE-TRAVAS | ......... | REGISTRADA | BACKLOG:2082 |
| O261-PAI-TERMINAL | LATERAL-ESTRUTURA | Encaminha o resíduo do pai terminal sem retomar o desenho vedado. | - | O261-E-PAI-TERMINAL-RESIDUAL | ++....... | REGISTRADA (DEMOLIR em 13/11; raia só referência) | BACKLOG:2083 |
| V3-E1 | CEREJA | Certifica o motor v3 com sua bateria de cenários. | - | esclarecer motivo; revalidar bateria | ......... | INTERROMPIDA | BACKLOG:2084; anexo:interrompidas.txt:27 |
| O10-C | LATERAL-ESTRUTURA | Entrega ao admin a lista histórica e uma ação alcançável para a troca. | - | lista do admin; porta de UI | ......... | INTERROMPIDA | BACKLOG:2085; anexo:interrompidas.txt:29 |
| OTIMIZA-1 | INSTRUMENTO | Mostra no painel as suítes inteiras e os pousos efetivamente concluídos. | - | SUITE-POR-IMPACTO; replay; verificações; pouso | +........ | INTERROMPIDA (brief pronto) | BACKLOG:2086; app/docs/RELATO.md:25; anexo:interrompidas.txt:31 |
| TIPO-NA-FONTE-LAMPADA-INSTANTE | LATERAL-ESTRUTURA | Guarda com fuso o instante da lâmpada usado pelo plantio. | - | TIPO-DNA-9-CHAVES | ......... | REGISTRADA (absorvida) | BACKLOG:2087 |
| RELOGIO-NA-BORDA | LATERAL-ESTRUTURA | Faz o cadastro usar um único instante informado na entrada da operação. | - | catraca da c5e | ......... | REGISTRADA | BACKLOG:2088 |
| RELOGIO-NA-BORDA-OPUS-C5E | LATERAL-ESTRUTURA | Trata releituras de relógio e falhas de transação que sobraram na c5e. | - | RELOGIO-NA-BORDA | ......... | REGISTRADA | BACKLOG:2089 |
| VIGIA-PAREADOR-DIA-CIVIL | LATERAL-ESTRUTURA | Faz a vigilância comparar pelo dia do turno. | - | fora do pouso TIPO item 3 | .+....... | REGISTRADA | BACKLOG:2090 | [nao verificado]
| TIPO-ATA-12-CHAVES-A | LATERAL-ESTRUTURA | Faz todas as leituras da ata passar pelo mesmo acesso tipado. | - | TIPO item 3 no ar; c5e; censo por chave | ......... | REGISTRADA | BACKLOG:2091 |
| TIPO-ATA-12-CHAVES-B | LATERAL-ESTRUTURA | Converte cada hora de fato da ata em instante com fuso. | - | TIPO-ATA-12-CHAVES-A | .+....... | REGISTRADA | BACKLOG:2092 |
| TIPO-DNA-9-CHAVES | LATERAL-ESTRUTURA | Aplica ao DNA da célula o acesso tipado e os instantes com fuso. | - | programa das 12 chaves da ata | ......... | REGISTRADA | BACKLOG:2093 |
| ACESA-ORFA-PROXIMIDADE | ESPINHA-E | Trata a batida órfã usada simultaneamente para acender uma lâmpada. | - | TIPO item 3 | .+....... | REGISTRADA | BACKLOG:2094 |
| TRANCA-VIVAS-08-FILA | LATERAL-ESTRUTURA | Alinha o significado do resgate e mede seus efeitos nos leitores. | - | medir universo e impacto | ......... | REGISTRADA | BACKLOG:2095 |
| NAO-LAVRADA-PASSADO | INSTRUMENTO | Mostra no painel os dias passados que ainda não receberam ata. | - | pouso TIPO item 3; medir dono | .+....... | REGISTRADA | BACKLOG:2096 | [nao verificado]
| E3-PENDENTE-NAO-BOOL | LATERAL-ESTRUTURA | Impede interpretar valores indevidos como lâmpada pendente. | - | pouso A das 12 chaves | ......... | REGISTRADA | BACKLOG:2097 | [nao verificado]
| SOMBRA-COM-TETO | INSTRUMENTO | Limita a memória usada por sondas na sombra. | - | contrato de instrumento; pouso próprio | ......... | REGISTRADA | BACKLOG:2098 | [nao verificado]
| SETNULL-ORFA-DNA | LATERAL-ESTRUTURA | Trata a célula futura cujo vínculo foi apagado e ficou no DNA. | - | corte de desenho | +........ | PARADA (pergunta de desenho) | BACKLOG:2099 |
| TIPO-ITEM3-CONTRACT | LATERAL-ESTRUTURA | Passa a recusar atas antigas fora do formato após zerar o passivo. | - | item 3 expand no ar; contador zero | .+....... | REGISTRADA | BACKLOG:2100 |
| SOMBRA-LEITURA-V3 | INSTRUMENTO | Permite leituras concorrentes na sombra com isolamento e prova corretos. | - | corrigir quatro falhas do instrumento | ..+..+... | REGISTRADA (v1 e v2 reprovadas) | BACKLOG:2101 |
| TIPO-V5-NAO-BLOQUEANTES | LATERAL-ESTRUTURA | Trata relógios internos e a comparação pelo dia civil nos vigias. | - | - | ......... | REGISTRADA | BACKLOG:2102 |
| CONTRATO-DE-ENTRADA-DO-FIO | LATERAL-ESTRUTURA | Faz o emissor entregar a batida correta para nascer a pergunta. | HJ-095 | E3; DESLIGADO/FERIADO | ++....... | BRIEF PRONTO | BACKLOG:2103; app/docs/RELATO.md:26 |
| CODEX-CENSO-CONTRATOS-DE-ENTRADA | INSTRUMENTO | Lista os desencontros entre quem produz e quem lê os dados. | - | corte para abrir obras | +........ | REGISTRADA (censo feito; universo incompleto) | BACKLOG:2104; app/docs/RELATO.md:12 |

Exclusões deste t

[truncado em 20000 de 21419 caracteres]

## 13/11 20:30 · sessao 565df2ae6e9a · - · - · origem=Ronald (tmux)

69 incluídas; 41 linhas excluídas. Grade em nove posições: censo, lei, construção, RED/GREEN, mutantes, verificador, opus, pouso, ar. Nenhum estado EM VOO foi atribuído sem prova de atividade no RELATO de 13/11. [nao verificado]

| CONTRATO-20-E2-QUAL-ATO | ESPINHA-E | Evita cobrar ausência quando feriado ou folga já explica o dia. | - | O-OUTBOX | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1073 |
| JUSTIFICATIVA-ENCERRA-ORIGEM | LATERAL-ESTRUTURA | Encerra a cobrança de origem quando a justificativa é aprovada. | - | grupo (1), sítio 1; irmã para cinco casos | + + + . . . . . + | REGISTRADA (parte no ar; cinco casos retidos) | app/docs/BACKLOG.md:1074 |
| EXPIRACAO-DA-ABERTA-COPIA-ANTIGA | ESPINHA-E | Faz os turnos antigos usarem a mesma regra de expiração dos demais. | - | ORFAS | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1076 |
| JUIZ-PERGUNTA-VIVA-MUDA-SEM-FATO | ESPINHA-E | Impede que a mesma pergunta mude de viva para morta sem um fato registrado. | - | ? (retomada sem precedência explícita) | + + + + . + . . + | REGISTRADA (cura no ar; passivo e trilha pendentes) | app/docs/BACKLOG.md:1077 |
| PASSIVO-VIVA-EM-DISPUTA-FECHADA | LATERAL-ESTRUTURA | Devolve ao fluxo correto perguntas ainda vivas dentro de disputas encerradas. | - | DESLIGAMENTO-NAO-CARIMBA-RESPONDIDA; acervo | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1078 |
| DESLIGAMENTO-NAO-CARIMBA-RESPONDIDA | LATERAL-ESTRUTURA | Corrige o tratamento das respostas quando ocorre desligamento. | - | DESLIGAR-LIQUIDA-TUDO | + + . . . . . . . | REGISTRADA (superada em 13/11; sucessora definida) | app/docs/BACKLOG.md:1079 |
| JUIZ-DE-DONO-DO-DIA | ESPINHA-E | Separa os problemas do sistema dos problemas de cadastro ou batida. | - | corte para predicado de dono | + . . . . . . . . | PARADA (sem predicado autorizado) | app/docs/BACKLOG.md:1080 |
| O210-COL417-COL931 | LATERAL-ESTRUTURA | Atualiza a apuração de dois colaboradores cujo fechamento ficou antigo. | - | O210 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1081 |
| GATE10-ESTRUTURA-EM-4-RUBRICAS-DA-10 | ESPINHA-E | Corrige diferenças entre quatro verbas calculadas e a regra aplicável. | - | corte de predicado; autorização se alcançar motor v2 | + + . . . . . . . | PARADA (predicado depende de corte) | app/docs/BACKLOG.md:1082 |
| LOTE-PLANTA-MARCO-AVULSO-E-ABRE-TURNO | LATERAL-ESTRUTURA | Evita que um reparo em lote abra turnos e retire horas de quem trabalhou. | - | ? (residual sem precedência escrita) | + + + + . . . . . | REGISTRADA (fechada na origem; quatro casos residuais) | app/docs/BACKLOG.md:1084 |
| LOTE-AGRUPA-POR-DIA-E-RASGA-O-PAR-DA-MEIA-NOITE | LATERAL-ESTRUTURA | Mantém juntas as duas pontas de um turno que atravessa a meia-noite. | - | remedicação dos 51 casos em 32 dias | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1085 |
| BO-INSS-LEITORES-QUE-NAO-MIGRARAM | LATERAL-ESTRUTURA | Faz as cobranças concordarem sobre ausências que cobrem apenas parte do dia. | - | ? (seis resíduos sem causa medida) | + + + . . . . . + | REGISTRADA (cura no ar; seis resíduos sem causa) | app/docs/BACKLOG.md:1087 |
| BO-ATESTADO-ANDROID-ERRO-AO-ENVIAR | LATERAL-ESTRUTURA | Permite enviar atestado e explica corretamente os erros de reenvio. | - | smoke de reenvio | + + + + . . . . + | REGISTRADA (parte no ar; falta smoke) | app/docs/BACKLOG.md:1091 |
| REABRIR-NAO-DESTROI | LATERAL-ESTRUTURA | Preserva respostas anteriores quando uma pergunta é reaberta. | - | smoke de clique | + + + . . . . . + | REGISTRADA (no ar desde 25/10; falta smoke) | app/docs/BACKLOG.md:1094 |
| GATE-EXPORT-10 | INSTRUMENTO | Libera o arquivo da folha somente depois das conferências e decisões exigidas. | - | decisão do DP sobre HE; autorização | + + + . . . . . . | PARADA (exportação barrada pela porta) | app/docs/BACKLOG.md:1095 |
| FILA-FORA-DO-BLOCO-QUE-O-HOOK-LE | INSTRUMENTO | Impede que obras desapareçam ao gerar novamente a fila. | - | ? (sem precedência explícita) | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1096 |
| PARADO-CITADO-E-PROSA-QUE-EXPLICA | INSTRUMENTO | Evita alarmes falsos quando um registro apenas explica uma obra parada. | - | próxima leitura de tickets_placar | + + + + . . . . + | REGISTRADA (instrumento no ar; contador a remedir) | app/docs/BACKLOG.md:1097 |
| UNIVERSO-DE-PERGUNTA-DO-DIA-SAO-DOIS | LATERAL-ESTRUTURA | Faz o cartório e o vigia considerarem as mesmas perguntas de cada dia. | - | remedicação; correção da FK no escritor | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1098 |
| O256 | UI | Reúne filtros, responsáveis e explicações do retrato da folha em uma tela coerente. | - | porta de PDF; O261 para complemento de dono; smoke final | + + + . . . . . + | PARADA (PDF sem porta; outras partes no ar) | app/docs/BACKLOG.md:1100 |
| DISPUTA-FECHADA-SEM-RESOLUCAO | LATERAL-ESTRUTURA | Exige que o encerramento da disputa registre o motivo. | - | corte sobre a porta de encerramento | + . . . . . . . . | PARADA (não construir sem corte) | app/docs/BACKLOG.md:1101 |
| VIGENCIA-SOBREPOSTA-COL515 | LATERAL-ESTRUTURA | Trata vínculos antigos que cobrem simultaneamente os mesmos dias. | - | corte sobre cadastro | + + . . . . . . . | PARADA (não construir sem corte) | app/docs/BACKLOG.md:1102 |
| HE-DE-14H-NUM-DIA (col945 12/09) | LATERAL-ESTRUTURA | Apura por que um único dia recebeu quase quinze horas extras. | - | corte; medição da causa | + . . . . . . . . | PARADA (aguarda corte para entrar na fila) | app/docs/BACKLOG.md:1103 |
| ESMERIL familia (2) batida | ESPINHA-E | Detecta batidas escritas fora da porta ou lidas sem respeitar retratações. | - | família (1); suíte; merge | + + + + + . . . . | REGISTRADA (fechada na raia; suíte e merge pendentes) | app/docs/BACKLOG.md:1104 |
| ~~PROVA-DOMINIO~~ | INSTRUMENTO | Compararia o cálculo atual com recibos antigos já pagos fora do sistema. | - | ? (execução cortada) | . + . . . . . . . | PARADA (cortada; estado fora da lista de exclusão) | app/docs/BACKLOG.md:1117 |
| BUG-ANCORA-NAO-REGENERA | LATERAL-ESTRUTURA | Faz a correção da fase de trabalho chegar às células dos dias afetados. | - | decisão sobre col824 e col899 | + + + + . . . . . | PARADA (cura fechada; decisões de cadastro pendentes) | app/docs/BACKLOG.md:1118 |
| WIZARD-FASE-ALCANCA-A-NAO-EXPORTADA | UI | Permite corrigir a fase desde o vínculo que cobre o dia errado. | - | VINCULO-LINHA-DO-TEMPO | + + + . . . . . . | PARADA (congelada; absorvida por outra obra) | app/docs/BACKLOG.md:1119 |
| AVISO-ENTRADA-ANTECIPADA | UI | Avisa ao colaborador quanto falta para começar sua jornada quando bate antes. | - | descongelamento pela L-096 | . + . . . . . . . | PARADA (congelada) | app/docs/BACKLOG.md:1122 |
| AVISO-ESPURIA-JANELA (desenho) | UI | Define o aviso que o aplicativo dará para batidas fora da janela esperada. | - | confirmação do admin da folha e do dono | . . . . . . . . . | PARADA (desenho aguardando confirmação) | app/docs/BACKLOG.md:1123 |
| ABONO-NO-AR-NAO-FECHOU | ESPINHA-E | Faz os valores diários reproduzirem corretamente o fechamento já gravado. | - | autorização; tratamento da competência 10 | + + + . . . . . . | PARADA (09 fechada; pendência truncada de autorização) | app/docs/BACKLOG.md:1126 |
| VINCULO-LINHA-DO-TEMPO | ESPINHA-E | Mantém uma única sequência de vínculos válidos sem sobreposição. | - | ESMERIL-2; censo de leitores; autorização para col899 | + + + + . . . . . | PARADA (porta pronta; tripwire e constraint pendentes) | app/docs/BACKLOG.md:1127 |
| FAMILIA-FASE-12x36 | LATERAL-ESTRUTURA | Encontra fases de escala incompatíveis com o trabalho realizado e prepara sua correção. | - | autorização para propostas | + + + . . . . . . | PARADA (itens feitos; propostas aguardam autorização) | app/docs/BACKLOG.md:1128 |
| O-HAIKU-INVESTIGA | LATERAL-ESTRUTURA | Dá ao assistente consultas para investigar escalas e preparar propostas de correção. | - | O-DIA-PAGO S4; VINCULO-LINHA-DO-TEMPO | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1130 |
| O-DIA-PAGO | ESPINHA-E | Faz todas as telas lerem um único valor diário de pagamento. | - | fatias S3, S4, S5 e S5b | + + + . . . . . . | REGISTRADA (fechada por fatias; S5b ainda citada em voo antigo) | app/docs/BACKLOG.md:1131 |
| O-DIA-PAGO S5 | CEREJA | Constrói o calculador independente que poderá substituir o cálculo antigo. | - | S1-S2; S5a; S3; autorização da troca | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1132 |
| O-JANELA-HE | LATERAL-ESTRUTURA | Só conta a hora extra fora da jornada quando a regra ou a autorização permite. | - | smoke dos treze passos | + + + . . . . + + | REGISTRADA (no ar desde 01/10; falta smoke) | app/docs/BACKLOG.md:1134 |
| CHAMADO-NASCE-EM-EXPORTADA | LATERAL-ESTRUTURA | Investiga cobranças novas abertas para competências já pagas. | - | censo dos emissores; decisão de desenho | + + . . . . . . . | REGISTRADA (não construir) | app/docs/BACKLOG.md:1137 |
| DISPUTA-4418-ABERTA-COM-TUDO-RESOLVIDO | LATERAL-ESTRUTURA | Trata disputas abertas que já não têm nenhuma pergunta viva. | - | censo de disputas abertas sem pergunta viva | + + . . . . . . . | REGISTRADA (não construir) | app/docs/BACKLOG.md:1138 |
| COL900-VINCULO-X-CELULA | LATERAL-ESTRUTURA | Mostra a diferença entre a escala corrigida e as células antigas da folha paga. | - | autorização; REGEN-EM-EXPORTADA | + + . . . . . . . | PARADA (aguarda autorização) | app/docs/BACKLOG.md:1140 |
| S5b-CALCULADOR-ESCREVE | CEREJA | Grava os resultados do calculador novo sem alterar a folha exportada. | - | autorização da troca | + + + . . . . . . | REGISTRADA (escrita exercida; troca ainda depende de autorização) | app/docs/BACKLOG.md:1141 |
| COL882-O-PROXIMO-COL515 | LATERAL-ESTRUTURA | Prepara a correção de dias trabalhados sem vínculo que os cubra. | - | autorização de vínculo | + + . . . . . . . | PARADA (aguarda autorização de cadastro) | app/docs/BACKLOG.md:1142 |
| DISPUTA-S84-RETRO-PASSIVO | LATERAL-ESTRUTURA | Separa batidas retroativas sem resposta entre retratação e registro visível do trabalho. | - | cura da origem; autorização; REGEN-EM-EXPORTADA | + + . . . . . . . | PARADA (texto mantém espera de autorização) | app/docs/BACKLOG.md:1144 |
| ESMERIL-MECANICO-POR-TRECHO | INSTRUMENTO | Confere mecanicamente os leitores e escritores por trecho de código. | - | ? (trecho truncado conserva espera de autorização) | + + + . . . . . . | PARADA (fechada na origem; espera truncada preservada) | app/docs/BACKLOG.md:1146 |
| BUG-APP-SESSAO-401 | UI | Avisa quando o aplicativo perde a sessão e deixa o colaborador sem registrar ponto. | - | autorização; smoke nas duas cascas | + + + . . . . . + | PARADA (admin no ar; parte do app aguarda smoke) | app/docs/BACKLOG.md:1148 |
| UI-ANEXO-AUSENCIA | UI | Mostra anexos pelo conteúdo e prepara a conversão de fotos HEIC. | - | catálogo; O30 F3; rebuild com decodificador HEIC | + + . . . . . . . | PARADA (conversão bloqueada pelo rebuild) | app/docs/BACKLOG.md:1149 |
| UI-PAINEL-MOSTRA-O-APP | UI | Faz o painel mostrar o mesmo rótulo e a mesma contagem do aplicativo. | - | lote UI-ANEXO-AUSENCIA | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1150 |
| DASHBOARD-V2-ROTULO-A-MAO | UI | Faz o painel alternativo ler o rótulo comum dos chamados. | - | pouso de UI-PAINEL-MOSTRA-O-APP | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1151 |
| ABSOLVIDA-TEXTO-PLANTAO | UI | Explica a data do plantão e a data da saída quando atravessam a meia-noite. | - | UI-PAINEL-MOSTRA-O-APP; POS-20 | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1152 |
| UI-REGULARIZAR-FALA-A-LINGUA-DO-ADMIN | UI | Troca termos técnicos por ações claras para corrigir batidas. | - | proposta de tela; POS-20 | + + . . . . . . . | REGISTRADA (não construir) | app/docs/BACKLOG.md:1153 |
| O-JANELA-HE Gestao de HE (LISTAGEM) | UI | Lista as horas extras pendentes usando os mesmos dados do bloqueio da folha. | - | smoke de clique; merge | + + + + + . . . . | PARADA (pronta na raia; falta smoke para merge) | app/docs/BACKLOG.md:1155 |
| COL369-TRES-RESTANTES | LATERAL-ESTRUTURA | Apura três diferenças de intervalo, pagamento e vínculo do mesmo colaborador. | - | smoke da Gestão de HE; autorização para vínculo | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1156 |
| GESTAO-HE-FORMA | UI | Organiza a decisão de horas extras por colaborador e dias expansíveis. | - | desenho aprovado; corte sobre autorização de uma ponta | + + + . . . . . + | PARADA (expandido devolvido à bancada; pergunta aberta) | app/docs/BACKLOG.md:1157 |
| O-JANELA-HE parte B2 (AUTORIZAR) | LATERAL-ESTRUTURA | Permite autorizar a hora extra e atualizar o pagamento pela porta correta. | - | B1; S4; smoke | + + + + . . . + + | REGISTRADA (no ar desde 01/10; falta smoke) | app/docs/BACKLOG.md:1158 |
| E4-CALENDARIO | ESPINHA-E | Faz o calendário usar o mesmo valor diário de pagamento dos demais leitores. | - | próximo deploy; O-DIA-PAGO S3b | . + + + . . . . . | REGISTRADA (fechada; deploy ainda citado como próximo) | app/docs/BACKLOG.md:1159 |
| BO-354 | LATERAL-ESTRUTURA | Evita perguntas de saída erradas quando as batidas estão com tipos invertidos. | - | descongelamento pela L-096 | + + . . . . . . . | PARADA (congelada) | app/docs/BACKLOG.md:1161 |
| E6-14 | ESPINHA-E | Identifica a causa de cada colaborador que ainda não passou na certificação. | - | troca da S3; descongelamento | . + . . . . . . . | PARADA (congelada) | app/docs/BACKLOG.md:1162 |
| O-FOLGA-DIA-ERRADO | LATERAL-ESTRUTURA | Sugere corrigir a escala quando o trabalho recorrente aponta folga cadastrada errada. | - | BO-354; corte de lei; autorização de vínculo | . . . . . . . . . | PARADA (congelada; espera corte) | app/docs/BACKLOG.md:1163 |
| AVISO-DO-DONO | UI | Envia um aviso ao responsável quando uma obra fecha ou precisa de autorização. | - | portão da 09; descongelamento; smoke no celular | . + . . . . . . . | PARADA (congelada) | app/docs/BACKLOG.md:1164 |
| O-GEO-DECISAO | LATERAL-ESTRUTURA | Registra decisões de localização e avisa quando um turno monitorado fica sem sinal. | - | corte das etapas 1-2; O198 antes do complemento; O197 depois | . + . . . . . . . | PARADA (etapas congeladas; complemento registrado) | app/docs/BACKLOG.md:1165 |
| O73b | LATERAL-ESTRUTURA | Corrige a cobrança de atraso na volta da pausa do turno partido. | - | descongelamento; autorização sobre a 09 | . + . . . . . . . | PARADA (congelada) | app/docs/BACKLOG.md:1166 |
| CORTE-B-30 | LATERAL-ESTRUTURA | Explica individualmente os casos separados do lote de correção. | - | descongelamento; corte do lote B | + + . . . . . . . | PARADA (congelada) | app/docs/BACKLOG.md:1167 |
| O145 | LATERAL-ESTRUTURA | Faz a Gestão de HE enxergar pontas de trabalho que não casaram com o marco. | - | O229 para parte residual | + + + . . . . . + | REGISTRADA (cura no ar; 33 resíduos) | app/docs/BACKLOG.md:1170 |
| O146 | LATERAL-ESTRUTURA | Paga a extra declarada na escala sem aumentar a jornada devida. | - | linha do copiloto | + + + + . . . . + | REGISTRADA (no ar; falta linha do copiloto) | app/docs/BACKLOG.md:1171 |
| O214 | LATERAL-ESTRUTURA | Permite decidir horas extras por colaborador e período antes de liberar a folha. | - | autorização do apply do item 1; smoke do item 2 | + + + . . . . . + | PARADA (itens no ar; apply e smoke pendentes) | app/docs/BACKLOG.md:1172 |
| PLACAR-ESTRUTURAL | ESPINHA-E | Mostra se cálculo, telas e folha permanecem coerentes com o cadastro existente. | - | autorização dos casos da 09 exportada; lote do R6 | + + + . . . . . . | PARADA (R3, R4 e R6 parciais; espera autorização) | app/docs/BACKLOG.md:1173 |
| O98 | INSTRUMENTO | Explica os termos da casa e aponta onde cada conceito existe no código. | - | vaga da raia | + + . . . . . . . | REGISTRADA (arquivo parcial existente) | app/docs/BACKLOG.md:1177 |
| R1 | LATERAL-ESTRUTURA | Apura o caso em que o papel atribuído à batida altera um registro coerente. | - | autorização sobre o caso restante | + + . . . . . . . | PARADA (censo dissolveu a premissa geral; resta um caso) | app/docs/BACKLOG.md:1178 |
| SUITE-DO-NUCLEO-ENTRA-NA-REGUA | INSTRUMENTO | Inclui os testes do núcleo de mensageria nas verificações obrigatórias. | - | O135; pouso | + + + + . . . . . | REGISTRADA (fechada na raia; pouso condicionado) | app/docs/BACKLOG.md:1180 |
| L111-ORFAOS-DO-MOTOR | ESPINHA-E | Remove duas respostas antigas de cálculo que já não têm leitores. | - | conferência da O275/O277 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1181 |
| SOMBRA_BLOCO_NA_JANELA-SEM-LEITOR | INSTRUMENTO | Impede certificar uma sombra cujo bloco não cobriu a janela utilizada. | - | pouso próprio de instrumento | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1182 |

Excluídas, com respectivas linhas: COL129-DONO-PELO-JUIZ 1075; BO-INSS-NUNCA-BATEU 1083; API-NAO-TEM-PORTA-DE-ANEXAR-EM-AUSENCIA-EXISTENTE 1086; LOG-BEST-EFFORT-EM-DOBRO 1088; SEM-CELULA-NA-JANELA-INTEIRA 1089; VIGIA-NAO-VE-350-DOS-369-DIAS-CALADOS 1090; RETRATO-PARADO-2010 1092; SAIDA-VALIDA-VIRA-ORFA 1093; O255 1099; O-JANELA-HE parte A2 (BLOQUEIO TOTAL) 1105; R2 1106; R3 1107; R2b 1108; IMPAR 1109; E5 1110; R4 1111; R5 1112; SELO-TXT 1113; E6-IMPAR 1114; BUG-HE-INTRA-DOBRADA 1115; O-JANELA-HE parte A (REGRA) 1116; UI-CAL-COMPETENCIA polimento 1120; UI-GRADE-CALENDARIO 1121; O-DIA-PAGO S3 1124; QUEM-ESCREVE-NO-GRAVADO-DA-EXPORTADA 1125; O-DIA-PAGO S4 1129; COBRAR-SO-O-QUE-MUDA-A-FOLHA 1133; ESMERIL-MECANICO 1135; HAIKU-FOLHA 1136; GRUPO-D-DIA-ACUSA-SEM-FIO 1139; COL515-VINCULO 1143; VEREDITO-VELHO-APOS-REGEN 1145; BUG-DISPUTA-ZUMBI 1147; O-JANELA-HE parte B1 (LEITURA) 1154; HOOK-PAREI-SE-APAGA 1160; E5-FINAL 1168; CELULA-TURNO-FECHA 1169; E6-CAUDA-2 1174; E6-CAUDA-1 1175; PLACAR-E 1176; O81 1179. Fontes: app/docs/BACKLOG.md nas linhas indicadas.

Duplicata identificada pelo root: O81, linhas 1179 e 1383; esta faixa exclui a ocorrência 1179. Fonte da segunda ocorrência informada pelo root, não relida por este agente.

Divergências úteis:

- PLACAR-ESTRUTURAL — BACKLOG:1173 diz 3 fechados e 3 parciais de 6; anexo ESTADO.md, seção PLACAR-ESTRUTURAL, diz 3 fechados e 4 parciais de 7, com termômetro R1 vencido.
- O214 — BACKLOG:1172 registra etapas no ar em 09/10 e conserva, em outra célula, o antigo “nada construído”; texto posterior de implantação prevalece para a grade, mantendo apply e smoke pendentes.
- O145 — BACKLOG:1170 diz NO AR e CURADA na célula de estado e “não começada” na célula seguinte.
- DISPUTA-S84-RETRO-PASSIVO — BACKLOG:1144 registra “espera !” no estado e “! DADO” na outra célula; mantida PARADA pela divergência.
- WIZARD-FASE-ALCANCA-A-NAO-EXPORTADA — BACKLOG:1119 mantém CONGELADA no estado e diz em outra célula que foi absorvida e já não é item próprio; incluída conservadoramente.
- E6-14, O73b e CORTE-B-30 — células de estado continuam CONGELADAS (BACKLOG:1162,1166,1167), enquanto a medição antiga do PLACAR-E diz E6-14 19, O73b curado e CORTE-B-30 reduzido a 16 (BACKLOG:1176); mantidas porque não há baixa posterior na própria célula de estado.

## 13/11 20:30 · sessao 36a9be65171a · - · - · origem=Ronald (tmux)

| BUG-117-COMPLEMENTO | ESPINHA-E | Impede que resposta já aplicada continue sendo cobrada. | - | fila anterior | +/+/././././././. | REGISTRADA | BACKLOG:1533 | [nao verificado]
| LISTA-POR-COMPETENCIA | ESPINHA-E | Mostra os problemas de cadastro separados por competência da folha. | - | apply da competência 10 | ./+/././././././. | REGISTRADA | BACKLOG:1534 |
| O137-8932-ENTERRAR | LATERAL-ESTRUTURA | Remove uma cadeia antiga de regras de abono sem uso. | - | ? (não indicada) | ./+/././././././. | REGISTRADA | BACKLOG:1535 |
| O137-ESTA-EXPORTADA-K6 | ESPINHA-E | Faz a regularização consultar a autoridade que confirma a entrega da folha. | - | ? (não indicada) | ./+/././././././. | REGISTRADA | BACKLOG:1536 |
| PORTAO-FATURAMENTO-A1 | INSTRUMENTO | Aponta dias com problema que não oferecem caminho de resolução. | - | apply da competência 10 | ./+/././././././. | REGISTRADA | BACKLOG:1537 |
| PORTAO-FATURAMENTO-RESTO | INSTRUMENTO | Confere quinze condições de coerência antes do fechamento da folha. | - | lote EVENTO | ./+/././././././. | REGISTRADA | BACKLOG:1538 |
| SOMBRA-SH-PROSA-DA-LAVRA | INSTRUMENTO | Corrige a explicação do ensaio sobre o que a lavratura decide. | - | ? (não indicada) | +/./././././././. | REGISTRADA | BACKLOG:1539 |
| FALTA-ACIMA-DO-PREVISTO-[nome removido] | ? (dono não medido) | Verifica se a folha desconta falta acima da jornada prevista. | - | medir dono pela L-099 | +/+/././././././. | REGISTRADA | BACKLOG:1540 |
| GEOFENCE-PERGUNTA-AO-JUIZ | ESPINHA-E | Faz alertas e localização usarem a mesma avaliação de GPS. | - | DRY e declaração do juiz | +/+/~ঁ/./././././. | REGISTRADA (construção histórica, sem curso atual provado) | BACKLOG:1541 |
| AUTORIDADE-DE-FOTO-AUSENTE | ESPINHA-E | Centraliza a decisão sobre ausência de foto antes de retirar o cron julgador. | - | autoridade de foto ausente | ./+/././././././. | REGISTRADA | BACKLOG:1542 |
| CRON-SOMBRA-NAO-CABE | INSTRUMENTO | Verifica por que o ensaio ultrapassa o horário reservado. | - | medir tendência da duração | +/./././././././. | REGISTRADA | BACKLOG:1544 |
| VIGIA-FIO-BEST-EFFORT-TIPO-MARCO | INSTRUMENTO | Identifica por que uma fiscalização tenta executar uma escrita. | - | censo dos chamadores | +/./././././././. | REGISTRADA | BACKLOG:1545 |
| O262 | ESPINHA-E | Declara quem responde pelas perguntas sobre a escala do dia. | - | export da competência 10 e decisão do nome | +/+/+/+/./././+/+ | PARADA (declaração no ar; migração pausada) | BACKLOG:1546 |
| O264 | LATERAL-ESTRUTURA | Encaminha a correção do feriado intermitente conforme a decisão posterior do DP. | - | O277 | +/+/././././././. | REGISTRADA (cura cancelada e residual migrado) | BACKLOG:1549 |
| O265 | ESPINHA-E | Atualiza o retrato da folha quando o dia termina. | - | export da competência 10; BUG-LINHA-X-BADGE | ./+/././././././. | REGISTRADA | BACKLOG:1550 |
| O266 | ESPINHA-E | Faz a geração de dias consultar as mesmas autoridades de vínculo. | - | DIFF das duas cópias na frota | +/+/././././././. | REGISTRADA | BACKLOG:1551 |
| O267 | UI | Mostra quantos dias ainda seguram a folha. | - | ? (não indicada) | +/+/././././././. | REGISTRADA | BACKLOG:1552 |
| O268 | ESPINHA-E | Faz quatro rotinas consultarem prazos e encerramento pelas autoridades responsáveis. | - | export da competência 10 | ./+/././././././. | REGISTRADA | BACKLOG:1553 |
| O269 | LATERAL-ESTRUTURA | Atualiza o cálculo de setembro preservando o arquivo já pago e nomeando as diferenças. | - | prova do retrato; DIFF; reversão; Pauta DP | ./+/././././././. | REGISTRADA | BACKLOG:1554 |
| O270 | LATERAL-ESTRUTURA | Calcula a indenização de intervalo em jornadas entre quatro e seis horas. | - | O263-FT; conferência de CCT; DIFF | +/+/././././././. | REGISTRADA | BACKLOG:1555 |
| O271 | LATERAL-ESTRUTURA | Aplica a mesma regra de atraso e saída antecipada ao trabalho comercial. | - | DIFF da competência 10 e condições de apply | +/+/././././././. | REGISTRADA | BACKLOG:1556 |
| O273 | LATERAL-ESTRUTURA | Devolve ao administrador horas extras cuja recusa automática perdeu a justificativa. | - | elo do cron para as 17 revogações | +/+/+/+/./././+/+ | REGISTRADA (cura no ar; revogações pendentes) | BACKLOG:1558 |
| O274 | UI | Faz os botões do fio funcionarem dentro do painel lateral. | - | smoke de clique | +/+/././././././. | REGISTRADA | BACKLOG:1559 |
| O275 | LATERAL-ESTRUTURA | Permite cadastrar quando um feriado deve ser dia previsto de trabalho. | - | decisão FORMA A x FORMA B | +/+/+/+/././././. | INTERROMPIDA | BACKLOG:1560; anexo:interrompidas.txt:19 |
| O276 | ESPINHA-E | Impede que uma saída feche um turno maior que o teto da jornada. | - | destino do número na zona inviolável; medição 24x48 | +/+/+/./././././. | REGISTRADA (destravada; pouso não provado) | BACKLOG:1561 | [nao verificado]
| O277 | LATERAL-ESTRUTURA | Ajusta a dobra de feriado de SDF e intermitente conforme o corte registrado. | - | pouso da O275 | +/+/././././././. | REGISTRADA | BACKLOG:1562 | [nao verificado]
| O279 | INSTRUMENTO | Retoma a sessão após queda de conexão respeitando as paradas legítimas. | - | liberação ou cura da regra operacional | +/+/././././././. | PARADA | BACKLOG:1564 |
| O280 | CEREJA | Reúne as regras de cálculo em um único cálculo de período. | - | bateria; esmeril; export da competência 10; autorização da troca | +/+/+/././././~/./. | PARADA | BACKLOG:1565 |
| V3-BATERIA-DE-CASOS-PELA-REGRA | CEREJA | Confere o novo motor com casos cuja resposta vem da regra escrita. | - | perguntas de lei dos intervalos e tolerância | +/+/+/+/././././. | REGISTRADA (recorte pronto, não commitado) | BACKLOG:1566 | [nao verificado]
| O281 | LATERAL-ESTRUTURA | Mede quatro suspeitas de erro antes de decidir quais exigem correção. | - | medição do bloco A | ./+/././././././. | REGISTRADA (leitura histórica, curso atual não provado) | BACKLOG:1567 |
| O282 | LATERAL-ESTRUTURA | Refaz estados que continuam antigos depois de mudanças nos fatos. | - | O281, bloco A item 3 | ./+/././././././. | REGISTRADA | BACKLOG:1568 |
| O283 | INSTRUMENTO | Confere se os botões das duas interfaces apontam para elementos existentes. | - | O274 e três hosts corrigidos | ./+/././././././. | REGISTRADA | BACKLOG:1569 |
| O284 | UI | Gera o caderno de espelhos em segundo plano com progresso e download. | - | smoke de clique; parte B na O280 | +/+/+/././././+/+ | REGISTRADA (parte A no ar; smoke pendente) | BACKLOG:1570 |
| O285 | INSTRUMENTO | Permite comparar folhas sem alterar o gravado usado como referência. | - | ? (não indicada) | +/+/././././././. | REGISTRADA | BACKLOG:1571 |
| O287 | CEREJA | Audita o novo motor com casos independentes antes da troca. | - | esmeril sem itens abertos; autorização da troca | +/+/+/./././././. | REGISTRADA | BACKLOG:1573 |
| O290 | INSTRUMENTO | Mostra também os motivos pelos quais uma comparação não foi medida. | - | ? (não indicada) | +/+/+/+/././././. | REGISTRADA (feito, sem pouso confirmado) | BACKLOG:1576 | [nao verificado]
| O293 | INSTRUMENTO | Guarda separadamente as falhas da empresa e as da competência. | - | pouso da raia O280 | +/+/././././././. | REGISTRADA | BACKLOG:1579 | [nao verificado]
| O294 | INSTRUMENTO | Investiga a origem de duas mudanças no arquivo já exportado de setembro. | - | leitura da trilha; decisão sobre Pauta DP | +/+/././././././. | REGISTRADA | BACKLOG:1580 |
| O296 | INSTRUMENTO | Retira identificações pessoais de comentários de código. | - | autorização para o sítio na zona inviolável | +/+/././././././. | PARADA | BACKLOG:1582 |
| O297 | INSTRUMENTO | Alerta quando o relato público deixa de atualizar por retenção. | - | ? (não indicada) | +/+/././././././. | REGISTRADA | BACKLOG:1583 |
| O298 | INSTRUMENTO | Confere se a medição citada realmente executa o comportamento prometido. | - | ? (não indicada) | +/+/~ঁ/./././././. | REGISTRADA (correções parciais; guarda pendente) | BACKLOG:1584 |
| O299 | INSTRUMENTO | Faz o topo do backlog ler o estado na coluna correta. | - | ? (não indicada) | +/+/././././././. | REGISTRADA | BACKLOG:1585 |
| O300 | CEREJA | Evita consultas repetidas passando ao cálculo os dados já carregados. | - | troca da O280; autorização para o v2 | +/+/././././././. | REGISTRADA | BACKLOG:1586 |
| O301 | INSTRUMENTO | Detecta identificadores repetidos que apagam casos de teste em silêncio. | - | ? (não indicada) | +/+/././././././. | REGISTRADA | BACKLOG:1587 | [nao verificado]
| O302 | INSTRUMENTO | Dá identidade e responsável aos containers de teste que sobrevivem ao lançador. | - | ? (não indicada) | +/+/././././././. | REGISTRADA | BACKLOG:1588 | [nao verificado]
| O303 | INSTRUMENTO | Protege a condição que torna segura a recusa de períodos sem carimbo. | - | ? (não indicada) | +/+/././././././. | REGISTRADA | BACKLOG:1589 |
| O304 | CEREJA | Evita contar duas vezes a pontualidade de um par duplicado. | - | autorização da reâncora | +/+/././././././. | REGISTRADA | BACKLOG:1590 |
| O305 | INSTRUMENTO | Corrige referências de documentação que passaram a apontar para outras funções. | - | próximo toque em periodo.py | +/+/././././././. | REGISTRADA | BACKLOG:1591 |
| O306 | LATERAL-ESTRUTURA | Faz respostas de horário persistirem e informa recusas honestamente. | - | O317 para o passivo; repetição do smoke | +/+/+/././././+/+ | REGISTRADA (cura no ar; passivo e smoke pendentes) | BACKLOG:1592 |
| O308 | UI | Reabre o assistente mostrando a data realmente gravada no vínculo. | - | cura do contexto e smoke | +/+/././././././. | REGISTRADA | BACKLOG:1594 |
| O310 | UI | Mantém a orientação do retrato visível em todas as abas. | - | O310-RAIOX | +/+/+/././././+/+ | REGISTRADA (parte no ar; Raio-X separado na fila) | BACKLOG:1596 |
| O311 | UI | Completa o recebimento de push nativo e a cobrança de perguntas de disputa. | - | cruzamento com O306 | +/+/~ঁ/./././././. | REGISTRADA | BACKLOG:1597 | [nao verificado]
| O312 | INSTRUMENTO | Prepara um caso sintético de hora extra que possa ser autorizado no smoke. | - | caso sintético autorizável | ./+/././././././. | REGISTRADA | BACKLOG:1598 |
| O314 | LATERAL-ESTRUTURA | Verifica se um mesmo dia de falta é descontado duas vezes. | - | medir as 28 linhas | ./+/././././././. | REGISTRADA | BACKLOG:1600 |
| O315 | CEREJA | Reserva a mensagem de par quebrado aos casos em que ela realmente se aplica. | - | O280 | +/+/././././././. | REGISTRADA | BACKLOG:1601 |
| O316 | CEREJA | Fecha cinco pendências de comportamento antes da troca do motor. | - | autorização B13/B14; troca para I17 | +/+/~ঁ/././././~/~ | PARADA (entrega parcial; autorização e troca pendentes) | BACKLOG:1602 |
| O140-FURO-COMPOSTO | ESPINHA-E | Faz o fechamento composto conservar os dias de falta apurados. | - | liberação de fechamento.py; re-apuração 10/11/12; autorização da 09 | +/+/././././././. | PARADA | BACKLOG:1603 |
| CARIMBO-POR-ORIGEM | LATERAL-ESTRUTURA | Mantém os horários de atualização corretos na origem das gravações. | - | autorização de desenho | +/+/././././././. | PARADA | BACKLOG:1604 |
| O310-RAIOX | UI | Mostra uma lente de consulta baseada no retrato oficial da folha. | - | corte para história, modelo e migration | +/./+/././././+/+ | PARADA (lente no ar; história incompleta) | BACKLOG:1605 |
| L7-AJUDA-DA-REGUA-CCT | UI | Faz a ajuda da tela acompanhar a autoridade que escolhe a convenção. | - | alteração em core/regua_cct.py | +/+/././././././. | REGISTRADA | BACKLOG:1606 |
| EMPATES-SEM-VIGENCIA | LATERAL-ESTRUTURA | Impede o aviso de empate entre convenções quando uma já perdeu vigência. | - | decisão da competência de referência | +/+/././././././. | PARADA | BACKLOG:1607 |
| CITACAO-POR-LINHA-DERIVA | INSTRUMENTO | Substitui referências frágeis por referências conferidas pelo conteúdo da função. | - | ? (não indicada) | +/+/././././././. | REGISTRADA | BACKLOG:1608 |
| EXPIRA-EM-ANDA-COM-HOJE | LATERAL-ESTRUTURA | Guarda a data real de expiração sem fazê-la avançar a cada dia. | - | censo dos leitores | +/+/././././././. | REGISTRADA | BACKLOG:1609 |
| NOME-EM-COMENTARIO-DE-CODIGO | INSTRUMENTO | Remove identificação de colaborador dos comentários publicados. | - | censo de comentários e docstrings | +/+/././././././. | REGISTRADA | BACKLOG:1610 | [nao verificado]
| FIO-MORTO-NASCE-NO-DIA-21 | LATERAL-ESTRUTURA | Mede se o primeiro dia da competência concentra perda do caminho de resolução. | - | censo por data da competência | +/+/././././././. | REGISTRADA (registrar, não construir) | BACKLOG:1611 |
| O317 | LATERAL-ESTRUTURA | Regulariza horários já validados e encaminha ao DP os que a porta recusa. | - | O306; autorização e DIFF por competência; O269 para a 09 | +/+/././././././. | REGISTRADA | BACKLOG:1612 |
| O318 | UI | Retira o formulário de horários já validados que aguardam regularização. | - | O317; pouso de O319; smoke | +/+/+/+/./././+/+ | REGISTRADA (card no ar; smoke pendente) | BACKLOG:1613 | [nao verificado]
| O318-DIA-MISTO | UI | Oferece formulário somente para os marcos que a porta permite registrar. | - | item anterior em curso | +/+/././././././. | REGISTRADA | BACKLOG:1614 |
| TELA-MENOR-QUE-O-GRAVADO | ESPINHA-E | Faz a tela e o escritor apresentarem a mesma quantidade de horas. | - | corte da adição de 27/09 | +/+/././././././. | REGISTRADA | BACKLOG:1616 |
| CARIMBO-QUE-A-CELULA-DESMENTE | ? (dono não medido) | Investiga respostas retiradas da fila embora o dia ainda acuse problema. | - | medir dono pela L-099 | +/+/././././././. | REGISTRADA | BACKLOG:1617 |
| O319 | INSTRUMENTO | Confere se uma resposta é gravada, sobrevive ao retorno e chega ao supervisor. | - | O317; O318; ação humana no restante | +/+/././././././. | REGISTRADA | BACKLOG:1618 |
| O261-PREC-NO-LOTE | LATERAL-ESTRUTURA | Define quem registra a mudança de precedência no julgamento de vários dias. | - | decisão entre lote e unidade | +/+/././././././. | PARADA | BACKLOG:1619 |
| CARD-SEM-DIA-OFERECE-FORMULARIO | UI | Impede oferecer um formulário quando a porta não consegue determinar o dia. | - | ? (não indicada) | +/+/././././././. | REGISTRADA | BACKLOG:1620 |
| API-CHAMADOS-V2-SERVE-PERGUNTA-SEM-A-LEI | LATERAL-ESTRUTURA | Faz a API entregar apenas perguntas reconhecidas como vivas pela autoridade. | - | ? (não indicada) | +/+/././././././. | REGISTRADA | BACKLOG:1621 |
| I28-NO-V2 | CEREJA | Registra a revogação da cura antecipada e mantém o caso preso à troca do motor. | - | troca da v3 | ./+/././././././. | PARADA (revogada; não construir) | BACKLOG:1622 |
| O320 | LATERAL-ESTRUTURA | Encaminha dias discordantes ao DP com o responsável e o veredito explícitos. | - | ? (incompleto não detalhado na célula) | +/+/+/+/./././+/./. | REGISTRADA (pousada; incompleto declarado) | BACKLOG:1623 |
| FURO-APURADO-FALSO-EM-20378-LINHAS | ESPINHA-E | Faz o gravado indicar corretamente os dias com falta apurada. | - | re-carimbo; DIFF; reversão; autorização da exportada | +/+/~ঁ/./././././. | REGISTRADA | BACKLOG:1625 |
| TRAVA-FILA1-STRING-E-TRUTHY | INSTRUMENTO | Impede que texto seja interpretado como autorização booleana de fila. | - | ? (não indicada) | +/+/././././././. | REGISTRADA | BACKLOG:1626 |
| CELULA-DE-ESTADO-TEM-FORMA | INSTRUMENTO | Detecta linhas de backlog cuja forma impede ler o estado corretamente. | - | pouso próprio do selo | +/+/~ঁ/./././././. | REGISTRADA | BACKLOG:1627 | [nao verificado]
| CONTROLE-SEM-PLANTIO-SEPARA-AS-DUAS-METADES | INSTRUMENTO | Separa no ensaio o efeito do horário registrado e o da relavratura. | - | autorização O317-APPLY-MOVE-MAIS-QUE-A-HORA | +/+/././././././. | REGISTRADA | BACKLOG:1628 |
| BO-CARTAO-ESTRELA-X-REALIZADO | ESPINHA-E | Confere por que uma batida marcada como órfã também soma horas no cartão. | - | quatro itens restantes; condições do apply O317 | +/+/././././././. | REGISTRADA | BACKLOG:1629 |
| GRAVADO-NAO-CARIMBA-A-REGUA | LATERAL-ESTRUTURA | Registra qual régua foi usada no cálculo de cada competência. | - | decisão do local e da obrigação do carimbo | +/+/././././././. | PARADA | BACKLOG:1630 |
| CADASTRO-DE-CCT-SEM-CARTORIO | LATERAL-ESTRUTURA | Dá trilha às alterações de convenção e torna visível a mudança de régua. | - | export de 21/10; corte do item c; instrumento próprio do item b | +/+/~ঁ/./././././. | PARADA (itens a e d construídos, sem pouso) | BACKLOG:1631 | [nao verificado]
| DECIMO-LEITOR-A-PRACA-SAI-DA-ESCALA-DE-HOJE | LATERAL-ESTRUTURA | Verifica quando a praça de hoje altera a régua de um período passado. | - | pouso próprio do vigia | +/+/././././././. | REGISTRADA | BACKLOG:1632 | [nao verificado]
| PROTEGE-VAZIA-EM-55-LEIS | INSTRUMENTO | Faz o selo acusar leis que nomeiam um sítio sem protegê-lo. | - | autorização para alterar LEIS.md | +/+/././././././. | PARADA | BACKLOG:1633 |
| REGUA-DO-PERIODO-COMPLEMENTO-19-10 | ESPINHA-E | Confere a régua por fatia da competência e dá destino às divergências. | - | itens 1 e 2; pergunta 18 para cadastro; item 5 depois do 2 | +/+/././././././. | REGISTRADA | BACKLOG:1634 |
| BO-CARTAO-ESTRELA-COMPLEMENTO-19-10 | ESPINHA-E | Mede a diferença de adicional noturno e corrige a estrela na origem. | - | item 3 de medição; RED do item 4 | +/+/~ঁ/./././././. | REGISTRADA | BACKLOG:1635 |
| ESCREVER-CORTA-PAUTA-EM-SILENCIO | LATERAL-ESTRUTURA | Impede perder parte da orientação ao gravar uma pauta longa. | - | censo dos 22 chamadores | +/+/././././././. | REGISTRADA | BACKLOG:1636 |
| PAUTA-DIA-EM-ABERTO-USA-O-PACKER | LATERAL-ESTRUTURA | Faz pautas longas usarem o mesmo empacotador oficial. | - | partir_em_pautas no ar | +/+/././././././. | REGISTRADA | BACKLOG:1637 |
| EXCECAO-DE-IMPORT-TARDIO-VIRA-INSTRUMENTO | INSTRUMENTO | Confere exceções de importação entre o commit e o deploy. | - | ? (não indicada) | +/+/+/./././././. | REGISTRADA (dispositivo existe fora de bin/) | BACKLOG:1638 | [nao verificado]
| RESIDUO-PREMIG-NO-BIND-MOUNT | INSTRUMENTO | Dá destino aos arquivos antigos que confundem o censo da árvore servida. | - | autorização para apagar | +/+/././././././. | PARADA | BACKLOG:1639 |
| LAVRA-REGUA-VAZIA-NO-GRAVADO-DA-09 | ? (dono não medido) | Investiga linhas pagas sem identificação da régua usada. | - | medir dono pela L-099 | +/+/././././././. | REGISTRADA | BACKLOG:1640 |
| REVERSAO-DA-10-NAO-COBRE-O-CARIMBO-DO-ESPELHO | INSTRUMENTO | Faz a reversão guardar e restaurar todos os campos que a relavratura altera. | - | antes do apply da 10; REGUA10-DUAS-CCT-NA-MESMA-COMPETENCIA | +/+/././././././. | REGISTRADA | BACKLOG:1641 |
| APLICACAO-CONVENCAO-SEM-VIGENCIA | LATERAL-ESTRUTURA | Cadastra desde quando cada empresa segue uma convenção. | - | pouso REGUA-DO-PERIODO; PDFs; resposta do DP | +/+/././././././. | REGISTRADA | BACKLOG:1642 | [nao verificado]
| APLICACAO-ATIVA-EM-EMPRESA-SEM-CCT | INSTRUMENTO | Conta aplicações de convenção ativas em empresas que não usam CCT. | - | ? (não indicada) | +/+/././././././. | REGISTRADA | BACKLOG:1643 |
| COL30-FORA-DOS-TRES-CONJUNTOS-DA-FOLHA | LATERAL-ESTRUTURA | Investiga um colaborador ausente de todas as categorias da prévia da folha. | - | localizar a causa antes de qualquer relavratura | +/+/././././././. | REGISTRADA | BACKLOG:1644 |
| ABRANGENCIA-DOS-QUATRO-INSTRUMENTOS-SEM-PDF | LATERAL-ESTRUTURA | Confere o enquadramento territorial com os documentos das convenções. | - | chegada dos PDFs | +/+/./././././././. | PARADA | BACKLOG:1645 |
| PERCENTUAL-DO-NOTURNO-SEM-LEITOR | LATERAL-ESTRUTURA | Faz o cálculo consumir o percentual noturno cadastrado no instrumento. | - | cadastro e leitor do instrumento | +/+/././././././. | REGISTRADA | BACKLOG:1646 |
| JANELA-DO-NOTURNO-CRAVADA | LATERAL-ESTRUTURA | Permite cadastrar a janela noturna própria da convenção. | - | autorização para zona inviolável | +/+/././././././. | PARADA | BACKLOG:1647 |

Consolidação deste subtrecho: 115 obras, 99 incluídas, 16 excluídas; duplicatas no universo total 1418..1762: nenhuma. [BACKLOG:1533; BACKLOG:1647]

Excluídas: UI-PILULAS-QUEBRAM-LINHA:1543; O263:1547; O263-FT:1548; O272:1557; O278:1563; O286:1572; O288:1574; O289:1575; O291:1577; O292:1578; O295:1

[truncado em 20000 de 21164 caracteres]

## 13/11 20:30 · sessao 2e894c7753f9 · - · - · origem=Ronald (tmux)

| O164 | ESPINHA-E | Faz cada alteração de chamado passar por uma única porta rastreável. | - | C2 → C3 → C4 → C1 | +/+/~/./././././. | REGISTRADA (construção só datada de 03/10) | BACKLOG:1418 |
| O165 | INSTRUMENTO | Verifica quais alterações podem ser publicadas sem esperar as de autenticação. | - | censo das dependências da raia | ~/+/././././././. | REGISTRADA (leitura só datada de 03/10) | BACKLOG:1419 | [nao verificado]
| O166 | ESPINHA-E | Faz as tarefas de prazo consultarem a regra oficial antes de agir. | - | O139; juiz do prazo | ./+/~/./././././. | REGISTRADA (construção só datada de 03/10) | BACKLOG:1420 |
| O167 | UI | Impede alterações de cadastros pelo painel administrativo sem trilha. | - | censo de quem usa o painel para corrigir dados | +/+/././././././. | REGISTRADA | BACKLOG:1421 |
| O168 | 20/20 | Mantém a cobrança aberta enquanto houver resposta aguardando julgamento. | - | guarda no evento antes de retirar o executor do supra | +/+/+/././+/+/+/+ | NO AR hoje | BACKLOG:1422; BACKLOG:2076; app/docs/RELATO.md:5; app/docs/RELATO.md:7; app/docs/RELATO.md:31 |
| O168-FILA-OPUS | 20/20 | Corrige os oito problemas restantes na proteção de conversas e respostas. | - | itens 3 e 4 antes do executor do supra; item 5 no marco seguinte | +/+/././././././. | REGISTRADA | BACKLOG:1423; BACKLOG:2076 |
| O169 | 20/20 | Impede que uma cobrança desapareça apenas porque envelheceu. | - | decisão sobre encerramento por idade | +/./././././././. | REGISTRADA (lista cita pousada; célula conserva pergunta antiga) | BACKLOG:1424; BACKLOG:2076 |
| O170 | ESPINHA-E | Faz as cobranças acompanharem os acontecimentos sem esperar varreduras. | - | O167; unicidade; ordem dos eventos; deduplicação; O168; O169 | +/+/././././././. | REGISTRADA | BACKLOG:1425 |
| O171 | ESPINHA-E | Faz a folha acompanhar o resultado atualizado do dia. | - | medir primeiro evento posterior ao deploy; eventual DIFF e reversão | +/+/././././././. | REGISTRADA | BACKLOG:1426 | [nao verificado]
| O172 | LATERAL-ESTRUTURA | Inclui o dia na identificação dos dados usados no julgamento. | - | ? (não indicada) | +/./././././././. | REGISTRADA | BACKLOG:1427 |
| O173 | INSTRUMENTO | Detecta importações do código servido que deixaram de existir no disco. | - | deploy do O142; preservar direção atual do selo | +/+/././././././. | REGISTRADA | BACKLOG:1428 | [nao verificado]
| O174 | INSTRUMENTO | Documenta quando tarefas agendadas e servidores usam versões diferentes. | - | ? (não indicada) | +/./././././././. | REGISTRADA | BACKLOG:1429 |
| O175 | LATERAL-ESTRUTURA | Evita que uma reconstrução de turnos altere competências já exportadas. | - | teto de competência aberta ou corte sobre reescrita do cache | +/+/././././././. | PARADA (corte pendente) | BACKLOG:1430 |
| O176 | INSTRUMENTO | Impede liberar uma fatia apenas porque outra usou o mesmo título. | - | corte para autoridade que confira conteúdo | +/+/././././././. | PARADA (espera corte) | BACKLOG:1431 |
| O177 | ESPINHA-E | Faz o cálculo considerar todas as pausas cadastradas no turno. | - | DIFF de frota; reversão; competências exportadas intactas | +/+/././././././. | REGISTRADA | BACKLOG:1432 |
| O178 | ESPINHA-E | Dá responsável e prazo às pendências encaminhadas ao administrador. | - | prazo e dono na pauta; contador de falta de leitura | +/+/././././././. | REGISTRADA | BACKLOG:1433 |
| O179 | ESPINHA-E | Investiga por que dias com previsão e batidas chegam zerados à folha. | - | medir horas reais; decidir dono ESTRUTURA ou BATIDA | +/+/././././././. | REGISTRADA | BACKLOG:1434 |
| O180 | ESPINHA-E | Entrega ao administrador os dias gerados além do fim do vínculo. | - | lista dos 102 colaboradores; autorização para regenerar passado | +/+/././././././. | PARADA (ato de passado depende de autorização) | BACKLOG:1435 |
| O181 | LATERAL-ESTRUTURA | Preserva a explicação de por que horas entraram na folha. | - | configurar destino persistente da trilha | +/+/././././././. | REGISTRADA | BACKLOG:1436 |
| O183 | ESPINHA-E | Impede criar chamados com urgências que o sistema não reconhece. | - | guarda → merge com O130/R4 → DIFF → tratamento do estoque | +/+/+/+/././././. | REGISTRADA (cura na raia; estoque e publicação pendentes) | BACKLOG:1438 |
| O184 | LATERAL-ESTRUTURA | Remove ou corrige uma função sem uso que ensina a medir previsão errada. | - | decidir remoção ou correção da documentação | +/+/././././././. | REGISTRADA | BACKLOG:1439 |
| O185 | INSTRUMENTO | Faz a proteção de autenticação comparar o deploy com a versão servida. | - | fatia própria posterior; referência do código no ar | +/+/././././././. | REGISTRADA | BACKLOG:1440 | [nao verificado]
| O187 | INSTRUMENTO | Mostra quem consulta cada campo importante do sistema. | - | fechamento da matriz estrutural | +/+/././././././. | REGISTRADA (não construir antes da matriz) | BACKLOG:1442 |
| O188 | INSTRUMENTO | Impede que renomear código pareça resolver uma pendência estrutural. | - | fechamento da matriz estrutural | ./+/././././././. | REGISTRADA (não construir antes da matriz) | BACKLOG:1443 |
| O189 | ESPINHA-E | Publica o efeito de distinguir ausência de informação de valor zero. | - | decisão da L-102 | +/+/././././././. | REGISTRADA (medida e publicada; nada aplicado) | BACKLOG:1444 | [nao verificado]
| O190 | INSTRUMENTO | Inclui o núcleo de mensagens na verificação periódica da árvore. | - | pouso do cert-ast | +/./././././././. | REGISTRADA | BACKLOG:1445 | [nao verificado]
| O192 | INSTRUMENTO | Evita que ferramentas deixem arquivos de outro usuário nas cópias de teste. | - | pouso do cert-ast | +/+/././././././. | REGISTRADA | BACKLOG:1447 | [nao verificado]
| O193 | INSTRUMENTO | Faz o vigia respeitar uma pausa declarada sem alarmar ou reverter publicações. | - | pouso próprio junto do cert-ast | +/+/././././././. | REGISTRADA | BACKLOG:1448 | [nao verificado]
| O194 | ESPINHA-E | Faz o julgamento da folga esperar o encerramento real do turno. | - | fatia própria da lavra; não embarcar na O191 | +/+/././././././. | REGISTRADA | BACKLOG:1449 |
| O195 | ESPINHA-E | Atribui o turno ao dia correto mesmo perto da meia-noite. | - | autorização para relavrar a competência 09 | +/+/+/././././+/+. | REGISTRADA (cura fechada; relavratura da 09 pendente) | BACKLOG:1450 |
| O196 | INSTRUMENTO | Mostra todas as tabelas alteradas quando a folha é relavrada. | - | CELULA-TURNO-FECHA; O145; fatia própria de dinheiro | +/+/././././././. | REGISTRADA | BACKLOG:1451 |
| O197 | ESPINHA-E | Evita chamar de aberto um dia futuro ou ainda em andamento. | - | medição posterior ao deploy; caso 5 encaminhado ao corte próprio | +/+/+/././././+/ . | REGISTRADA (fechada na sombra; medição posterior ainda escrita como pendente) | BACKLOG:1452 | [nao verificado]
| O198 | LATERAL-ESTRUTURA | Conta faltas aprovadas em dias sem jornada e entrega o diagnóstico. | - | corte posterior ao censo publicado | +/./././././././. | PARADA (censo publicado; ordem de parar) | BACKLOG:1453 | [nao verificado]
| O199 | LATERAL-ESTRUTURA | Faz o aceite de uma folga decidir o dia e fechar a cobrança. | - | autorização para o estoque antigo; O246 | +/+/+/././././+/+ | REGISTRADA (ato no ar; estoque antigo pendente) | BACKLOG:1454 |
| O201 | INSTRUMENTO | Evita que comentários sejam confundidos com regras ainda executadas. | - | decidir alteração da regra ou da forma das lápides | +/+/././././././. | REGISTRADA | BACKLOG:1456 |
| O202 | LATERAL-ESTRUTURA | Faz os pings de localização usarem a mesma avaliação oficial de distância. | - | ? (não indicada) | +/+/././././././. | REGISTRADA | BACKLOG:1457 |
| O203 | INSTRUMENTO | Impede remover código protegido apagando sua proteção no mesmo commit. | - | selo ler a proteção da base | +/+/././././././. | REGISTRADA | BACKLOG:1458 | [nao verificado]
| O204 | LATERAL-ESTRUTURA | Preserva a hora correta enviada pelo aparelho e registra descartes. | - | publicar a linha do placar em pouso de instrumento | +/+/+/+/./././+/+ | REGISTRADA (produto no ar; linha do placar pendente) | BACKLOG:1459 | [nao verificado]
| O205 | INSTRUMENTO | Faz a reversão automática conferir a árvore inteira que deixou para trás. | - | fila de instrumento posterior ao produto | +/+/././././././. | REGISTRADA | BACKLOG:1460 |
| O206 | UI | Oferece cobrança sem apagar respostas e mostra uma prévia fiel do aviso. | - | corrigir prévia; smoke nas duas cascas | +/+/+/././././+/+ | PARADA (smoke segurado pelo complemento de 12/10) | BACKLOG:1461 |
| O210 | ESPINHA-E | Atualiza os fechamentos que ficaram atrás do cálculo atual da competência. | - | nova foto; DIFF; reversão; prova posterior | +/+/././././././. | REGISTRADA | BACKLOG:1464 |
| O213 | LATERAL-ESTRUTURA | Conta perguntas cujo tipo de marcação discorda da ata do dia. | - | contar e publicar antes de qualquer cura | +/+/././././././. | REGISTRADA (censo feito; nada alterado) | BACKLOG:1465 |
| O229 | ESPINHA-E | Corrige diferenças restantes entre os marcos e os leitores de horas. | - | censo e RED por classe; O197 no item 9 | +/+/././././././. | REGISTRADA | BACKLOG:1466 |
| O215 | ESPINHA-E | Compara linha a linha o arquivo pago com o que a folha atual produziria. | - | gerar TXT na sombra; decisão sobre os dois números | ./+/././././././. | REGISTRADA | BACKLOG:1467 |
| O216 | ESPINHA-E | Refaz a medição anterior depois da correção do juiz dos turnos. | - | O195; medição pela porta real | +/+/././././././. | REGISTRADA | BACKLOG:1468 |
| O217 | ESPINHA-E | Verifica se repetir o julgamento estabiliza a ata do dia. | - | cura e bateria na O218 | +/+/././././././. | REGISTRADA (medição concluída; cura na O218) | BACKLOG:1469 |
| O230 | INSTRUMENTO | Permite executar sondas com acesso restrito de leitura e trilha. | - | pouso 2; REDs c/d/f; selo na régua | +/+/+/+/./././~/ . | REGISTRADA (pouso 1 fechado; pouso 2 pendente) | BACKLOG:1470 | [nao verificado]
| O233 | INSTRUMENTO | Mostra raias abertas que ficaram mais de seis horas sem commit. | - | pouso 2 da O230 | ./+/././././././. | REGISTRADA | BACKLOG:1472 | [nao verificado]
| O235 | UI | Faz o formulário explicar corretamente o efeito de intervalo vazio ou zero. | - | O207 | +/+/././././././. | REGISTRADA | BACKLOG:1473 |
| O243 | LATERAL-ESTRUTURA | Revisa por família os setores críticos e a utilidade de seus testes. | - | contratos estruturais 20/20; censo antes de podar testes | ./+/././././././. | REGISTRADA (aguarda gatilho 20/20) | BACKLOG:1474 | [nao verificado]
| RELATO-CURTO-SELO | INSTRUMENTO | Impede publicar novos relatos acima do limite de linhas. | - | O219; pouso de instrumento da O228 | +/+/././././././. | REGISTRADA | BACKLOG:1475 | [nao verificado]
| O244 | INSTRUMENTO | Explica em documentos curtos quem manda em cada família do sistema. | - | contratos estruturais 20/20 | ./+/././././././. | REGISTRADA (aguarda gatilho 20/20) | BACKLOG:1476 |
| O245 | INSTRUMENTO | Gera um diagrama que explica regras, responsáveis e pendências de cada bloco. | - | contratos estruturais 20/20; fontes já existentes | ./+/././././././. | REGISTRADA (aguarda gatilho 20/20) | BACKLOG:1477 |
| O246 | LATERAL-ESTRUTURA | Registra pela porta os aceites antigos de folga que nunca decidiram o dia. | - | O48 passo 2; RECALC-09; O219; DIFF e reversão | +/+/././././././. | REGISTRADA | BACKLOG:1478 |
| O248 | ESPINHA-E | Dá uma resposta única sobre o encerramento real de um dia. | - | O258; item 3 da fila antes do registro compartilhado | +/+/././././././. | REGISTRADA (complemento da O258) | BACKLOG:1480 |
| O249 | ESPINHA-E | Faz a detecção de pares consultar uma única autoridade. | - | O258; juiz antes da mudança do emissor | +/+/././././././. | REGISTRADA (complemento da O258) | BACKLOG:1481 |
| O250 | INSTRUMENTO | Mostra por raia a obra, o agente declarado e o que falta publicar. | - | dois pousos de produto; instrumento simular_folha --lido | +/+/+/./././././. | REGISTRADA (construída; pouso pendente) | BACKLOG:1482 | [nao verificado]
| O251 | UI | Alinha a prévia da cobrança ao envio e expõe uma configuração de cálculo na tela. | - | O207; raia própria para configuração; DIFF e reversão do ato | +/+/././././././. | REGISTRADA | BACKLOG:1483 |
| O252 | ESPINHA-E | Faz o botão de aprovação seguir o mesmo critério da exportação. | - | contrato batida × juiz; O48 passo 5; DIFF da competência 10 | +/+/././././././. | REGISTRADA | BACKLOG:1484 |
| O254 | ESPINHA-E | Faz o contrato de batidas ter testes e retenção coerente com os turnos. | - | corte do primeiro pendente; O48 passo 5; pendente 2 | +/+/+/././././+/ . | REGISTRADA (marco B fechado; pendente 2 permanece) | BACKLOG:1486 | [nao verificado]
| O257 | ESPINHA-E | Centraliza a resposta sobre se uma competência já foi entregue. | - | O252; O48 passo 5; censo de leitores de exportação | +/+/././././././. | REGISTRADA | BACKLOG:1487 |
| O258 | ESPINHA-E | Substitui decisões espalhadas nas tarefas por consultas aos juízes oficiais. | - | item 2; três tarefas que precisam de predicado novo; duas listas vazias | +/+/~/././././~/+ | REGISTRADA (item 1 no ar; restante pendente) | BACKLOG:1488 |
| O260 | LATERAL-ESTRUTURA | Faz a consulta de retenções devolver sempre o formato prometido. | - | fatia própria; preservar retorno sem dias | +/+/~/~/././././. | REGISTRADA (construção só datada de 12/10) | BACKLOG:1490 |
| SOBRA-NA-BORDA-O-QUE-FICOU | ESPINHA-E | Guarda os problemas restantes de reação e alinhamento após mudar batidas antigas. | - | obra própria para item a; censos por classe | +/+/././././././. | REGISTRADA | BACKLOG:1493 |
| BO-FERIADO-VINCULO-ANTIGO | LATERAL-ESTRUTURA | Alinha vínculos antigos à decisão cadastrada sobre trabalho em feriados. | - | casos mistos e nove postos encaminhados ao DP | +/+/+/./././././. | REGISTRADA (aplicado; casos de cadastro encaminhados ao DP) | BACKLOG:1495 | [nao verificado]
| C7-DISPUTA-FECHA-POR-UM-JUIZ | ESPINHA-E | Faz o encerramento de disputas obedecer a uma única decisão verificável. | - | C6; escritor único de chamado; lotes após L0 | +/+/~/./././././. | REGISTRADA (construção só datada de 22/10) | BACKLOG:1496 |
| BUG-DIA-SEM-SAIDA | ESPINHA-E | Dá um caminho de ação aos dias que continuam retidos sem pergunta viva. | - | medir 25 sem pai e 30 com defeito curado; casos do DP | +/+/+/././././+/+ | REGISTRADA (partes no ar; fila residual) | BACKLOG:1497 |
| BUG-RETRATO-VELHO-NA-TELA | UI | Avisa quando o retrato da folha está desatualizado. | - | O48 passo 6 após 20/20; medida dos casos restantes | +/+/~/./././././. | REGISTRADA | BACKLOG:1498 |
| C7-TELA-ADMIN-ENCERRA | UI | Oferece ao administrador encerrar uma disputa com motivo obrigatório. | - | porta admin_encerra; smoke nas duas cascas | +/+/././././././. | REGISTRADA | BACKLOG:1499 |
| BO-ATESTADO-409-MUDO | LATERAL-ESTRUTURA | Confirma o reenvio de atestado e explica recusas reais. | - | recontagem 48 h; reenvio real; diálogo no app; linha HAIKU | ./+/+/././././+/+ | REGISTRADA (API no ar; verificação e app pendentes) | BACKLOG:1500 |
| BO-B-DIAPAGO-SE-CONTRADIZ | ESPINHA-E | Impede que os campos do mesmo dia mostrem quantidades incompatíveis de horas. | - | BO-A; autorização no motor; impacto por rubrica | +/+/././././././. | REGISTRADA | BACKLOG:1501 |
| PORTA-CELULA-EDITADA | LATERAL-ESTRUTURA | Permite registrar pela tela o turno autorizado para um dia. | - | ? (não indicada) | ./+/././././././. | REGISTRADA | BACKLOG:1502 |
| INTERVALO-DECLARADO | LATERAL-ESTRUTURA | Pergunta sobre o intervalo incompleto e registra a resposta aprovada. | - | BOs em curso; sinal da célula; validação do administrador | +/+/././././././. | REGISTRADA | BACKLOG:1503 |
| [nome removido]-HE-NAO-POR-MINUTO | UI | Faz a recusa de horas extras cobrir apenas os minutos efetivamente decididos. | - | caso pela regra; prova de dinheiro zero | +/+/././././././. | REGISTRADA | BACKLOG:1504 |
| VARREDURA-VIRA-EVENTO-24-CRONS | ESPINHA-E | Faz tarefas reagirem ao sinal do dia e deixa varreduras apenas vigiarem. | - | O168; O170 C1; C7; medição zero por classe | +/+/~/././././~/+ | REGISTRADA (partes publicadas; migração restante) | BACKLOG:1505 | [nao verificado]
| CONTRATOS-RESTANTES-E1-E2-FOLHA-K6 | ESPINHA-E | Alinha escala e folha às mesmas autoridades e mede discordâncias. | - | exportação da 10 para E1 restante; corte de folha | +/+/~/././././~/+ | PARADA (restantes condicionados a exportação e lei) | BACKLOG:1506 |
| E6-NONE-E-CADASTRO | ESPINHA-E | Encaminha ao cadastro o dia cujo vínculo não foi definido. | - | ? (não indicada) | +/+/+/./././././. | REGISTRADA (FEITA; termo fora da lista literal de exclusão) | BACKLOG:1507 |
| GRADE-ESC-DO-DIA-E1 | ESPINHA-E | Faz a previsão da grade consultar o vínculo oficial do dia. | - | suíte; pouso; smoke | +/+/+/./././././. | REGISTRADA (cura na cópia; suíte e pouso pendentes) | BACKLOG:1508 | [nao verificado]
| CARIMBO-SEM-VIGENCIA | ESPINHA-E | Faz mudanças na vigência do vínculo provocarem novo julgamento do dia. | - | medir vínculos editados após o carimbo | +/./././././././. | REGISTRADA | BACKLOG:1509 |
| CONTADOR-PAREADOR-X-CELULA | ESPINHA-E | Mostra dias em que o turno está fechado, mas a célula ainda acusa borda órfã. | - | cura D2; comboio com item 3 do TURNO-ABERTO | +/+/~/./././././. | REGISTRADA (versão na cópia; suíte e pouso pendentes) | BACKLOG:1510 | [nao verificado]
| CONTADOR-AUSENCIA-X-VEREDITO | ESPINHA-E | Detecta ausência aprovada que o julgamento do dia ainda contradiz. | - | autorização para quatro casos em competência exportada | +/+/+/./././././. | PARADA (cura e suíte prontas; casos exportados dependem de autorização) | BACKLOG:1511 | [nao verificado]
| REGEN-EXPORTADA-SEM-EFEITO | LATERAL-ESTRUTURA | Permite corrigir células exportadas quando a medição prova ausência de outros efeitos. | - | seis condições na sombra; reversão; janela de autorização | +/+/././././././. | REGISTRADA (janela antiga; validade atual ?) | BACKLOG:1512 |
| TURNO-ABERTO-UM-EMISSOR | ESPINHA-E | Elimina decisões duplicadas sobre emissão e encerramento de turnos abertos. | - | lei do item 4; diagnóstico dos itens suspensos; corte do selfie | +/+/~/././././~/+ | PARADA (item 4 em lei; outros itens suspensos) | BACKLOG:1513 |
| ATA-NAO-DISPENSA-JUSTIFICADA | ESPINHA-E | Faz a ata reconhecer o período abonado por justificativa aprovada. | - | acesso para aplicar o acervo da competência 10 | +/+/+/+/./././+/+ | PARADA (cura no ar; aplicação do acervo sem permissão) | BACKLOG:1515 |
| CONCORDE-PREVISTO-ZERO | ESPINHA-E | Investiga dias considerados corretos que têm batidas e previsão zerada. | - | medir a causa de previsto zero | +/+/././././././. | REGISTRADA | BACKLOG:1517 |
| VIVA-DELEGA-AO-JUIZ | ESPINHA-E | Faz a indicação de pergunta viva consultar o mesmo juiz das demais telas. | - | pouso; censo dos leitores da propriedade | +/+/+/./././././. | REGISTRADA (construída; pouso pendente) | BACKLOG:1518 | [nao verificado]
| FOLGA-CONTESTA-DIZ-MANDAR-PARA-CADASTRO | UI | Explica que a folga contestada precisa de correção cadastral. | - | raia de tela; contador junto do caso col666 | +/+/././././././. | REGISTRADA | BACKLOG:1520 |
| Q-VIVAS-LEITORES-SEM-JUIZ | ESPINHA-E | Faz os leitores distinguirem perguntas realmente vivas das candidatas do filtro. | - | medir primeiro o efeito do escritor de fechamento | +/./././././././. | REGISTRADA | BACKLOG:1521 |
| ESPELHO-VERDADE-REMEDIDO | ESPINHA-E | Atualiza o placar com o que os testes e as medições realmente comprovam. | - | K6/E1; depois termômetro R1; antes da triagem de varreduras | +/+/././././././. | REGISTRADA | BACKLOG:1522 | [nao verificado]
| VARREDURA-TRIAGEM | ESPINHA-E | Classifica tarefas que devem vigiar, esperar prazo ou reagir a eventos. | - | segunda medição de reconciliar; medir vínculo e termômetro | +/+/~/././././~/+ | REGISTRADA (triagem e lote parcial feitos) | BACKLOG:1523 |
| BUG-CELULA-SEM-VEREDITO | ESPINHA-E | Verifica células sem julgamento e separa dados de teste do universo real. | - | residual em SEM-CELULA-NA-JANELA-INTEIRA | +/+/././././././. | REGISTRADA (CANCELADA; termo fora da exclusão literal) 

[truncado em 20000 de 23352 caracteres]

## 13/11 20:30 · sessao f69929b0788a · - · - · origem=Ronald (tmux)

114 incluídas, 2 excluídas; sem duplicatas internas. Balde 20/20 só pela lista fechada de app/docs/BACKLOG.md:2076; nenhum vínculo suficiente com essa lista neste subrecorte.

| REVERSAO-PELA-PORTA | INSTRUMENTO | Permitir desfazer alterações pela mesma porta que registra sua história. | - | contrato da reversão | .+....... | REGISTRADA | app/docs/BACKLOG.md:957 |
| E3-L02-PENDENTE | ESPINHA-E | Mostrar o status usando a lista de problemas já calculada. | - | v3; saída lavrada | ++....... | REGISTRADA | app/docs/BACKLOG.md:958 |
| B3-TOPO-FILA | INSTRUMENTO | Identificar corretamente qual obra está bloqueada. | - | DISJUNTOR-B3-TOPO | ......~.. | REGISTRADA | app/docs/BACKLOG.md:959 |
| CLAUDE-MD-ENXUTO | INSTRUMENTO | Reduzir o documento de instruções preservando suas regras. | - | corte para a meta de tamanho | .++++..+~ | PARADA (parte no ar; meta 8k pendente) | app/docs/BACKLOG.md:960 |
| CLAUDE-MD-PISTA-NO-ANEXO | INSTRUMENTO | Entregar ao leitor também as instruções que passaram para outro documento. | - | CLAUDE-MD-ENXUTO; contrato | ......... | REGISTRADA | app/docs/BACKLOG.md:961 |
| CLAUDE-MD-RACIONAL | INSTRUMENTO | Fundir instruções repetidas sem alterar as regras. | - | produto; B3 | .+....... | REGISTRADA | app/docs/BACKLOG.md:962 |
| CODEX-CONTEXTO-SEM-CONDUTA | INSTRUMENTO | Fornecer contexto de fatos e leis ao leitor automático. | - | CLAUDE-MD-ENXUTO; corte do teto | .+....... | PARADA (fontes acima do teto; pergunta pendente) | app/docs/BACKLOG.md:963 |
| CODEX-LICOES | INSTRUMENTO | Reutilizar lições comprovadas nas próximas leituras. | - | CODEX-CONTEXTO-SEM-CONDUTA | ++....... | REGISTRADA | app/docs/BACKLOG.md:964 |
| V3-RETOMA | CEREJA | Retomar o novo cálculo de adicionais e folgas com impacto identificado. | - | certificação; autorização da troca | .+....... | REGISTRADA | app/docs/BACKLOG.md:965 |
| V3-RETOMA-PERIODOS-DA-CELULA | CEREJA | Calcular períodos e pausas com a mesma fonte do espelho. | - | P4; autorização do motor | .++....~. | INTERROMPIDA | app/docs/BACKLOG.md:966; anexo:interrompidas.txt:11 |
| V3-P5-PRORROGACAO-POR-INTERVALO | LATERAL-ESTRUTURA | Aplicar ao intervalo a mesma prorrogação noturna do turno. | - | cópia do main; certificação; impacto | .++...~.. | REGISTRADA (corpo aprovado; nova certificação pendente) | app/docs/BACKLOG.md:967 |
| V3-HE-NOTURNA-POR-PECA | LATERAL-ESTRUTURA | Contar a prorrogação noturna também na hora extra. | - | V3-P5 | ++....... | REGISTRADA (impacto medido zero) | app/docs/BACKLOG.md:968 |
| EMIS-FALSE-ATA-TRABALHO | LATERAL-ESTRUTURA | Eliminar a contradição entre dia de folga e ata de trabalho. | - | censo próprio | ......... | REGISTRADA | app/docs/BACKLOG.md:969 |
| GRADE-FOLGA-C5 | LATERAL-ESTRUTURA | Conferir folgas de calendário apenas dentro do vínculo válido. | - | ? (precedência ausente) | ......... | REGISTRADA | app/docs/BACKLOG.md:970 |
| GRADE-FOLGA-GERADOR | LATERAL-ESTRUTURA | Fazer a nova escala respeitar a folga pessoal já lançada. | - | juiz da folga; ensaio; autorização do passado | .+....... | REGISTRADA | app/docs/BACKLOG.md:971 |
| V3-MOTOR-FERIADO-HE100-NOTURNA | LATERAL-ESTRUTURA | Reconhecer o adicional noturno das horas extras de feriado. | - | autorização do motor | ......... | REGISTRADA | app/docs/BACKLOG.md:972 |
| COBPARC-B-CAUSA-NO-MOTOR | LATERAL-ESTRUTURA | Levar à lista do admin as ausências parciais sem período definido. | - | autorização do motor | .+....... | PARADA (aguarda autorização do motor) | app/docs/BACKLOG.md:973 |
| V3-NOTURNO-RECORTE-LONGO | LATERAL-ESTRUTURA | Evitar erros e duplicação de horas em jornadas noturnas longas. | - | autorização do motor | ......... | PARADA (aguarda autorização do motor) | app/docs/BACKLOG.md:974 |
| F1C4-3-EMISSORES-FILA | ESPINHA-E | Fazer os emissores concordarem sobre dias sem cobrança. | - | censo dos juízes | ......... | REGISTRADA | app/docs/BACKLOG.md:975 |
| REVERTER-REGEN-TRIESTADO | LATERAL-ESTRUTURA | Preservar o dia indefinido ao desfazer a geração da escala. | - | ? (precedência ausente) | ......... | REGISTRADA | app/docs/BACKLOG.md:976 |
| E3-OPUS-FILA | ESPINHA-E | Completar as garantias de status, trilha e atualização do dia. | - | E4 para os leitores indicados | ......~.. | REGISTRADA | app/docs/BACKLOG.md:977 |
| V3-RETOMA-FILA | CEREJA | Completar a comparação e as fontes do novo cálculo. | - | pouso da V3 | .++...... | INTERROMPIDA | app/docs/BACKLOG.md:978; anexo:interrompidas.txt:13 |
| LEI-AKITA-UNIVERSO-DO-PUSH | INSTRUMENTO | Conferir os padrões apenas nos commits ainda não publicados. | - | ? (precedência ausente) | .+....... | REGISTRADA | app/docs/BACKLOG.md:979 |
| LEI-AKITA-NO-PRE-PUSH | INSTRUMENTO | Impedir publicação de commits que escaparam da conferência dos padrões. | - | LEI-AKITA-UNIVERSO-DO-PUSH | ......... | REGISTRADA | app/docs/BACKLOG.md:980 |
| SELO-SETUP-COPIADO-DE-FREEZE | INSTRUMENTO | Detectar testes que deixam o relógio congelado escapar para outros testes. | - | ? (precedência ausente) | ......... | REGISTRADA | app/docs/BACKLOG.md:981 |
| HX-AUTOR-MUDO-REGEX | INSTRUMENTO | Detectar autor vazio mesmo quando escrito de outra forma. | - | ? (precedência ausente) | ......... | REGISTRADA | app/docs/BACKLOG.md:982 |
| F1C4-FILA-OPUS | ESPINHA-E | Corrigir os resíduos de travas, vínculos e reabertura de perguntas. | - | ? (precedência ausente) | ......~.. | REGISTRADA | app/docs/BACKLOG.md:983 |
| E6-POR-DONO | ESPINHA-E | Separar os resultados do oráculo pelo responsável pela divergência. | - | pouso da F1C4 | .+....... | REGISTRADA | app/docs/BACKLOG.md:984 |
| P14-CONFIRMAR-IMPLAUSIVEL-SEM-MOTIVO | LATERAL-ESTRUTURA | Exigir motivo escrito para confirmar uma resposta improvável. | - | fatia em curso; exame das permissões | .+....... | REGISTRADA | app/docs/BACKLOG.md:985 |
| FIO-ESTADO-LE-O-ALCANCE | INSTRUMENTO | Mostrar no painel o alcance real dos dias sem atendimento. | - | F5 | .......++ | REGISTRADA (fechada; data do encerramento incerta) | app/docs/BACKLOG.md:986 |
| F5-RELOGIO-DO-EXPORT | LATERAL-ESTRUTURA | Usar o mesmo instante em toda a conferência da folha. | - | alteração no juiz da exportação | ......... | REGISTRADA | app/docs/BACKLOG.md:987 |
| RESPONDIDA-SEM-TEXTO-DUAS-COPIAS | ESPINHA-E | Unificar a identificação de resposta sem texto. | - | ? (precedência ausente) | ......... | REGISTRADA | app/docs/BACKLOG.md:988 |
| PERGUNTA-VIVA-LOGA-PREVISTO-COMO-FALHA | LATERAL-ESTRUTURA | Evitar alarmes de erro para divergências já previstas. | - | ? (precedência ausente) | ......... | REGISTRADA | app/docs/BACKLOG.md:989 |
| E2-D3-TRABALHA-FORA-DA-IMPRESSAO | ESPINHA-E | Rejulgar o dia quando mudar sua condição de trabalho. | - | medir rejulgamentos | .+.+..... | REGISTRADA | app/docs/BACKLOG.md:990 |
| E2-D3-VINCULO-NA-IMPRESSAO | ESPINHA-E | Rejulgar o dia quando o vínculo ou sua escala mudar. | - | medir rejulgamentos | .+.+..... | REGISTRADA | app/docs/BACKLOG.md:991 |
| E2-D3-FERIADO-NA-IMPRESSAO | ESPINHA-E | Atualizar o julgamento quando mudar um feriado aplicável. | - | medir rejulgamentos | .+.+..... | REGISTRADA | app/docs/BACKLOG.md:992 |
| E2-D3-CCT-NA-IMPRESSAO | ESPINHA-E | Atualizar o julgamento quando mudar a convenção aplicável. | - | medir rejulgamentos | .+.+..... | REGISTRADA | app/docs/BACKLOG.md:993 |
| E2-D3-ISENCAO-NA-IMPRESSAO | ESPINHA-E | Atualizar o julgamento quando mudar a isenção de ponto. | - | medir rejulgamentos | .+.+..... | REGISTRADA | app/docs/BACKLOG.md:994 |
| E2-D3-JUSTIFICATIVA-APROVADA | ESPINHA-E | Rejulgar o dia após a aprovação de uma justificativa. | - | medir rejulgamentos | .+.+..... | REGISTRADA | app/docs/BACKLOG.md:995 |
| E2-D3-BATIDA-DA-VESPERA | ESPINHA-E | Atualizar o dia afetado por uma batida da véspera. | - | medir rejulgamentos | .+.+..... | REGISTRADA | app/docs/BACKLOG.md:996 |
| E2-D3-EDICAO-HUMANA-SEM-PORTA | ESPINHA-E | Registrar a edição humana do dia por uma porta identificável. | - | porta de edição; medir rejulgamentos | .+.+..... | REGISTRADA | app/docs/BACKLOG.md:997 |
| O261-FIO-MORTO-PELO-RETRATO | ESPINHA-E | Encontrar e encaminhar dias que seguram a folha sem atendimento acessível. | - | O261-E-CELULA-20-UM-DONO | .++....~~ | REGISTRADA (curas no ar; residual absorvido pela O261-E) | app/docs/BACKLOG.md:998 |
| O261-V-FASE-NA-LISTA | LATERAL-ESTRUTURA | Mostrar ao admin os dias de escala com fase deslocada. | - | pouso A do VETO; nova construção | ++....~.. | REGISTRADA (cópia reprovada) | app/docs/BACKLOG.md:999 |
| JUIZ-UNICO-DA-ANCORA | LATERAL-ESTRUTURA | Usar uma única referência para a fase da escala 12x36. | - | V-FASE | .+....... | REGISTRADA | app/docs/BACKLOG.md:1000 |
| DONO-NAO-VE-FASE-DESLOCADA | ESPINHA-E | Atribuir corretamente ao cadastro os dias com fase de escala errada. | - | ? (precedência ausente) | .+....... | REGISTRADA | app/docs/BACKLOG.md:1001 |
| FOLGA-TRABALHADA-RESOLVE | LATERAL-ESTRUTURA | Permitir ao admin resolver em um clique uma folga trabalhada. | - | juiz do botão; batidas em quantidade par | .+....... | REGISTRADA | app/docs/BACKLOG.md:1002 |
| BO-[nome]-DESTRAVA | LATERAL-ESTRUTURA | Indicar a correção de cadastro que libera o dia na folha. | - | lavrador do retrato; dono do dia | .+....... | REGISTRADA | app/docs/BACKLOG.md:1003 |
| ALERTA-DESCARTES-DO-CODEX | INSTRUMENTO | Mostrar os achados descartados e cobrar a justificativa da classificação. | - | contrato do instrumento | .+....... | REGISTRADA | app/docs/BACKLOG.md:1004 |
| CODEX-ESCREVE-RED-WRAPPER | INSTRUMENTO | Permitir escrever testes de falha em uma cópia isolada. | - | contrato; verificação do instrumento | .+....... | REGISTRADA | app/docs/BACKLOG.md:1005 |
| FIO-DO-ADMIN-NO-ESTADO | INSTRUMENTO | Mostrar no painel os dias que dependem do admin. | - | FIO-MORTO-DONO-NA-LISTA | .+....... | REGISTRADA | app/docs/BACKLOG.md:1006 |
| FIO-DO-ADMIN-LAVRA-VELHA | LATERAL-ESTRUTURA | Alertar quando a lista do admin estiver baseada em medição antiga. | - | ? (precedência ausente) | ......... | REGISTRADA | app/docs/BACKLOG.md:1007 |
| VETO-IMPRESSAO-NUNCA-BATEU | ESPINHA-E | Rejulgar quem nunca bateu ponto quando mudar o veto aplicável. | - | VETO-A | ......... | REGISTRADA | app/docs/BACKLOG.md:1008 |
| VETO-TRILHA-CICLO | ESPINHA-E | Registrar novamente um veto que voltou depois de corrigido. | - | VETO-A | ......... | REGISTRADA | app/docs/BACKLOG.md:1009 |
| VETO-VIGIA-NO-PLACAR | INSTRUMENTO | Mostrar no placar os chamados vivos em dias vetados. | - | VETO-A | ......... | REGISTRADA | app/docs/BACKLOG.md:1010 |
| AVISO-ATIVO-SEM-VINCULO-NO-DIA | LATERAL-ESTRUTURA | Avisar quando faltar vínculo para um dia de colaborador ativo. | - | ? (precedência ausente) | .+....... | REGISTRADA | app/docs/BACKLOG.md:1011 |
| TESTES-QUE-NAO-PROVAM-[nome] | INSTRUMENTO | Fazer os testes comprovarem o nascimento do chamado prometido. | - | ? (precedência ausente) | ......... | REGISTRADA | app/docs/BACKLOG.md:1012 |
| CLEAR-SEM-MARCO | INSTRUMENTO | Registrar a limpeza de contexto para evitar cobranças de um ciclo antigo. | - | ? (precedência ausente) | ......... | REGISTRADA | app/docs/BACKLOG.md:1013 |
| ANALISAR-RELE-O-BANCO | LATERAL-ESTRUTURA | Impedir que uma análise reative chamado encerrado por outro ato. | - | ? (precedência ausente) | ......... | REGISTRADA | app/docs/BACKLOG.md:1014 |
| REABRIR-DISPUTA-ADMIN-IGNORA-RETORNO | LATERAL-ESTRUTURA | Evitar anunciar reabertura quando o chamado permaneceu encerrado. | - | ? (precedência ausente) | ......... | REGISTRADA | app/docs/BACKLOG.md:1015 |
| VETO-ATO-HUMANO-MOSTRA-E-TRILHA | ESPINHA-E | Mostrar e registrar o veto que o admin decidiu ultrapassar. | - | pouso B do O261-VETO-NA-PORTA | .+....... | REGISTRADA | app/docs/BACKLOG.md:1016 |
| TESTE-DESLIGADO-C2-VACUO | INSTRUMENTO | Fazer o teste detectar a retirada da proteção do desligado. | - | ? (precedência ausente) | ......... | REGISTRADA | app/docs/BACKLOG.md:1017 |
| DIAS-ENCERRADOS-CHAVE-DO-CARTORIO | LATERAL-ESTRUTURA | Entregar as células com a chave que o juiz de turnos espera. | - | ? (precedência ausente) | ......... | REGISTRADA | app/docs/BACKLOG.md:1018 |
| VETO-TRILHA-FRASE-DO-REABRIR | CEREJA | Descrever corretamente na história uma reabertura recusada. | - | ? (precedência ausente) | ......... | REGISTRADA | app/docs/BACKLOG.md:1019 |
| VETO-NA-PORTA-POR-ELIMINACAO | LATERAL-ESTRUTURA | Distinguir um veto de uma absolvição ao explicar um chamado não criado. | - | ? (precedência ausente) | .+....... | REGISTRADA | app/docs/BACKLOG.md:1020 |
| SELO-CARIMBO-VIRADA | INSTRUMENTO | Testar a virada da meia-noite no carimbo do vigia. | - | ? (precedência ausente) | ......... | REGISTRADA | app/docs/BACKLOG.md:1021 |
| CXR-FALHA-CALADA | LATERAL-ESTRUTURA | Mostrar falhas de leitura em vez de apresentar uma lista vazia. | - | ? (precedência ausente) | ......... | REGISTRADA | app/docs/BACKLOG.md:1022 |
| FASE-12X36-SEM-PAPEL-DA-ATA | LATERAL-ESTRUTURA | Conferir a fase da escala usando os turnos reconhecidos pela ata. | - | juiz dos turnos | ......... | REGISTRADA | app/docs/BACKLOG.md:1023 |
| PLANTIO-ENVELOPE-PELO-GERADOR | LATERAL-ESTRUTURA | Usar os horários congelados do dia para orientar a batida gerada. | - | fonte da célula | .+....... | REGISTRADA | app/docs/BACKLOG.md:1024 |
| DIA-DA-PERGUNTA-E-O-TURNO | ESPINHA-E | Ligar cada pergunta ao dia correto do turno. | - | pouso A do VETO | .+....~.. | REGISTRADA (cópia anterior reprovada) | app/docs/BACKLOG.md:1025 |
| O261-VETO-NA-PORTA | ESPINHA-E | Impedir cobrança automática em dia vetado e preservar a decisão humana. | - | comparação posterior; lotes B e C | .++....~~ | REGISTRADA (pouso A no ar; lotes B/C pendentes) | app/docs/BACKLOG.md:1026 |
| COL309-SETEMBRO | LATERAL-ESTRUTURA | Corrigir as células antigas usando o vínculo autorizado. | - | COL309-09-RECALCULO-DEPOIS-DO-ADMIN | .+.....+. | REGISTRADA (células aplicadas; recálculo em obra própria) | app/docs/BACKLOG.md:1027 |
| COL309-09-RECALCULO-DEPOIS-DO-ADMIN | LATERAL-ESTRUTURA | Recalcular setembro após o admin decidir os quatro dias pendentes. | - | quatro decisões do admin lavradas | .+....... | PARADA (aguarda admin) | app/docs/BACKLOG.md:1028 |
| CAL-PALAVRA-BANDAID | INSTRUMENTO | Corrigir a documentação que descreve uma solução fora das regras. | - | ? (precedência ausente) | .+....... | REGISTRADA | app/docs/BACKLOG.md:1030 |
| CAL-PREDICADO-D-MENOR-HOJE-2-COPIAS | LATERAL-ESTRUTURA | Unificar o critério de datas usado pelo calendário e pela folha. | - | ? (precedência ausente) | ......... | REGISTRADA | app/docs/BACKLOG.md:1031 |
| CAL-CUSTO-NO-MAPA-DE-FUROS | LATERAL-ESTRUTURA | Medir e reduzir consultas repetidas no mapa de furos. | - | medição do custo | ......... | REGISTRADA | app/docs/BACKLOG.md:1032 |
| CAL-DOIS-RELOGIOS | LATERAL-ESTRUTURA | Usar uma única leitura do relógio ao montar o calendário. | - | ? (precedência ausente) | ......... | REGISTRADA | app/docs/BACKLOG.md:1033 |
| CAL-TESTE-HOJE-SEM-PREVISTO | INSTRUMENTO | Cobrir em teste o dia de hoje sem previsão de trabalho. | - | ? (precedência ausente) | ......... | REGISTRADA | app/docs/BACKLOG.md:1034 |
| CAL-RODAPE-E-PDF-SEM-CERTIDAO | UI | Mostrar e contar corretamente os dias sem informação certificada. | - | ? (precedência ausente) | ......... | REGISTRADA | app/docs/BACKLOG.md:1035 |
| DISJUNTOR-E-CLEAR-RAPIDO | INSTRUMENTO | Interromper ciclos improdutivos e preparar a troca de contexto. | - | PRONTO 1–6 | .++...... | REGISTRADA (construção antiga; curso atual não comprovado) | app/docs/BACKLOG.md:1036 |
| DISJUNTOR-E-CLEAR-RAPIDO-COMPLEMENTO | INSTRUMENTO | Acrescentar limites de orçamento e medidas ao disjuntor. | - | DISJUNTOR-E-CLEAR-RAPIDO | .++...... | REGISTRADA (construção antiga; curso atual não comprovado) | app/docs/BACKLOG.md:1037 |
| SUITE-POR-IMPACTO | INSTRUMENTO | Conferir apenas os testes e dias afetados quando a classe permitir. | - | PRONTO-5; rebase; E3; corte do gate | .++...... | INTERROMPIDA | app/docs/BACKLOG.md:1038; anexo:interrompidas.txt:15 |
| SELOS-DE-HOST-VERMELHOS-NO-MAIN | INSTRUMENTO | Impedir que verificações de host vermelhas escapem da publicação. | - | SUITE-POR-IMPACTO | ......... | REGISTRADA | app/docs/BACKLOG.md:1039 |
| CODEX-CONTEXTO-DA-CASA | INSTRUMENTO | Gerar contexto verificável e perguntas neutras para o leitor automático. | - | briefs dos construtores em voo | .++...... | INTERROMPIDA | app/docs/BACKLOG.md:1040; anexo:interrompidas.txt:17 |
| CODEX-ASTRA-NA-VOLTA-FINAL | INSTRUMENTO | Selecionar explicitamente o modelo da verificação final. | - | decisão posterior ao comparativo | .+....... | REGISTRADA (cancelada após empate) | app/docs/BACKLOG.md:1041 |
| O121-A1-VINCULO-CORRIGE-O-PASSADO | ESPINHA-E | Permitir corrigir o vínculo passado pela porta autorizada. | - | marco em curso; VETO A | .++...... | REGISTRADA | app/docs/BACKLOG.md:1043 |
| CODEX-SONDA-LEITURA | INSTRUMENTO | Consultar dados de produção por um envelope de leitura e raspagem. | - | acesso ao envelope; selo | .++...... | REGISTRADA (construção antiga; curso atual não comprovado) | app/docs/BACKLOG.md:1044 |
| CODEX-PADRAO-PRO | INSTRUMENTO | Padronizar a leitura automática e mostrar seu uso no painel. | - | medição de capacidade | .++...... | REGISTRADA (construção antiga; curso atual não comprovado) | app/docs/BACKLOG.md:1045 |
| MAIS-CODEX-PLACAR | INSTRUMENTO | Mostrar o uso de leitores automáticos e os motivos dos desvios. | - | duas curas de produto no ar | .+....... | PARADA (moratória registrada) | app/docs/BACKLOG.md:1046 |
| ESPERA-SEM-TURNO | INSTRUMENTO | Aceitar espera somente com trabalho vivo comprovado. | - | contrato da linha do ESTADO | .+.....~~ | REGISTRADA (parte no ar; item do ESTADO pendente) | app/docs/BACKLOG.md:1047 |
| ENTRADA-BLOQUEADA-SEM-DATA-TURNO | LATERAL-ESTRUTURA | Registrar o dia do turno no chamado de entrada bloqueada. | - | JUSTIFICATIVA-ENCERRA-ORIGEM | ......... | REGISTRADA | app/docs/BACKLOG.md:1048 |
| POSICAO-TOTAL-ATE-A-DATA | LATERAL-ESTRUTURA | Somar o espelho até a data escolhida por uma única fonte. | - | fonte do total até a data | .+....... | REGISTRADA | app/docs/BACKLOG.md:1049 |
| POSICAO-FALTAS-E-AUSENCIAS | LATERAL-ESTRUTURA | Reunir faltas e ausências com seus tipos. | - | fonte unificada | .+....... | REGISTRADA | app/docs/BACKLOG.md:1050 |
| POSICAO-FERIAS-ATE-A-DATA | LATERAL-ESTRUTURA | Apurar férias proporcionais até a data escolhida. | - | regra de proporcionais e avos | .+....... | REGISTRADA | app/docs/BACKLOG.md:1051 |
| POSICAO-PORTA-POR-MOTIVO | LATERAL-ESTRUTURA | Oferecer o acesso correto para resolver cada impedimento. | - | fonte de URL e pergunta | .+....... | REGISTRADA | app/docs/BACKLOG.md:1052 |
| CODEX-CONSTRUTOR-PILOTO | INSTRUMENTO | Habilitar o piloto de construção em cópia isolada. | - | verificadores; pouso do wrapper | .++...... | REGISTRADA (wrapper em cópia; piloto ausente) | app/docs/BACKLOG.md:1053 |
| CODEX-RETORNO-0511 | INSTRUMENTO | Medir os resultados e o custo do uso semanal do leitor automático. | - | medição prevista para 05/11 | .+....... | REGISTRADA | app/docs/BACKLOG.md:1054 |
| VIGENCIA-FUTURA | LATERAL-ESTRUTURA | Agendar mudanças de posto ou escala para uma data futura. | caso 09 | complemento vinculo_do_dia do grupo 1 | .+....... | REGISTRADA | app/docs/BACKLOG.md:1055 |
| SELO-DIA-PELO-SIGNIFICADO | INSTRUMENTO | Detectar leitores que inventam o dia fora do juiz autorizado. | - | grupo 1, item 4; antes da LASTRO | .+....... | REGISTRADA | app/docs/BACKLOG.md:1056 |
| RELOGIO-INJETADO | ESPINHA-E | Fazer os juízes receberem o instante usado no julgamento. | - | SELO-DIA-PELO-SIGNIFICADO; censo | .+....... | REGISTRADA | app/docs/BACKLOG.md:1057 |
| CODE-CHAMA-CODEX | INSTRUMENTO | Enviar leituras a uma cópia identificada do código. | - | ? (precedência ausente) | .++...... | REGISTRADA (construção antiga; curso atual não comprovado) | app/docs/BACKLOG.md:1058 |
| BO2910-ESTRUTURA | LATERAL-ESTRUTURA | Corrigir as classes de cadastro, ausência e julgamento apontadas na triagem. | casos 01, 02, 05, 0

[truncado em 20000 de 23382 caracteres]

## 13/11 20:30 · sessao ac9ad38e801e · - · - · origem=Ronald (tmux)

| REABRIR-PERGUNTA-A-PREMISSA | ESPINHA-E | Evita reabrir cobranças encerradas sem fato novo no dia. | - | Medição do contador e conferência de sete dias sem ciclo | +/+/+/././././+/+ | REGISTRADA (medição do portão pendente) | BACKLOG:1686 |

## 13/11 20:30 · sessao e750f1defa46 · - · - · origem=Ronald (tmux)

| FERIADO-ESCALA | LATERAL-ESTRUTURA | Corrige a regra de trabalho em feriados no vínculo. | HJ-074 | FERIADO-USA-A-LEI-DE-1609 | +++~.~... | EM VOO (escopo substituído pelo aval de 16/09) | app/docs/BACKLOG.md:728; app/docs/RELATO.md:134 |
| FERIADO-1609-FILA | LATERAL-ESTRUTURA | Corrige falhas restantes do tratamento de feriados. | HJ-074 | pouso FERIADO | .+....... | REGISTRADA | app/docs/BACKLOG.md:729 |
| LEI-DESLIGADO-NAO-TRAVA | LATERAL-ESTRUTURA | Impede que pendências de desligados bloqueiem a folha. | - | - | ++++++~++ | NO AR hoje | app/docs/BACKLOG.md:730; app/docs/RELATO.md:54; app/docs/RELATO.md:75 |
| DESLIGAR-LIQUIDA-TUDO | LATERAL-ESTRUTURA | Encerra cobranças do desligado e encaminha respostas ao DP. | - | LEI-DESLIGADO-NAO-TRAVA; O50 | ++....... | REGISTRADA (censo entregue; aguarda vaga) | app/docs/BACKLOG.md:731; app/docs/RELATO.md:125 |
| DESLIGADOS-RETIDOS-CONTADOR | INSTRUMENTO | Mostra quantos desligados ainda ficam retidos na folha. | - | LEI-DESLIGADO-NAO-TRAVA | .+++.+.++ | NO AR hoje (complemento no mesmo pouso) | app/docs/BACKLOG.md:732; app/docs/RELATO.md:103; app/docs/RELATO.md:54 |
| PARIDADE-ABSOLVIDAS-168 | LATERAL-ESTRUTURA | Devolve ao admin respostas de intervalo encerradas sem julgamento. | - | BUG-PAR-DESCONGELA; condições do apply exportado | ++~+.~... | EM VOO (103 incorporadas ao BUG-PAR) | app/docs/BACKLOG.md:733; app/docs/RELATO.md:95; app/docs/RELATO.md:35 |
| RAIOX-DIAS-DERIVA | LATERAL-ESTRUTURA | Faz o Raio-X mostrar os dias apontados pela mesma autoridade. | - | pouso LEI-DESLIGADO-NAO-TRAVA | .+....... | REGISTRADA | app/docs/BACKLOG.md:734 |
| SITUACAO-LEITOR-CRU | LATERAL-ESTRUTURA | Unifica como o sistema reconhece um desligamento. | - | O50 | .+....... | REGISTRADA | app/docs/BACKLOG.md:735 |
| DESLIGADOS-RETIDOS-UNIVERSO | INSTRUMENTO | Faz a contagem de desligados retidos corresponder ao seu rótulo. | - | pouso LEI-DESLIGADO-NAO-TRAVA | .+....... | REGISTRADA | app/docs/BACKLOG.md:736 |
| RETRATO-DOIS-RELOGIOS | LATERAL-ESTRUTURA | Usa uma única data para montar o retrato da folha. | - | pouso LEI-DESLIGADO-NAO-TRAVA | .+....... | REGISTRADA | app/docs/BACKLOG.md:737 |
| OTIMIZA-1-COMPLEMENTO-CARGA | INSTRUMENTO | Reduz a memória retida e mede o pico do vigia. | - | pouso E3; contrato do instrumento | .+....... | REGISTRADA | app/docs/BLOG.md:738 |
| RAIA-PREPARACAO | INSTRUMENTO | Prepara levantamentos e instruções para as próximas obras. | HJ-091 | marco LEI-DESLIGADO; sequência 1→4 | ++....... | REGISTRADA (levantamentos entregues) | app/docs/BACKLOG.md:739; app/docs/RELATO.md:12; app/docs/RELATO.md:21; app/docs/RELATO.md:24; app/docs/RELATO.md:28 |
| BO-[nome]-1311-ROTEIRO | INSTRUMENTO | Classifica os casos relatados e identifica o que falta resolver. | caso 01; caso 02; caso 03; caso 04; caso 05; caso 06; caso 07; caso 08; caso 09; caso 10 | declaração do colaborador; projeção do retentor | ++....... | REGISTRADA (roteiro parcial; faltam medidas) | app/docs/BACKLOG.md:740; app/docs/RELATO.md:37 |
| HJ-092 | LATERAL-ESTRUTURA | Encaminha ao admin as batidas cuja correção exige decisão humana. | HJ-092 | admin; cadastro DP | ++....... | REGISTRADA (fechada como BATIDA, sem deploy) | app/docs/BACKLOG.md:741 |
| TIPO-DE-BATIDA-DE-PERGUNTA-SEM-AVISO | UI | Avisa quando uma pergunta altera o tipo de batida. | HJ-092 | - | .+....... | REGISTRADA | app/docs/BACKLOG.md:742 |
| HJ-093 | LATERAL-ESTRUTURA | Corrige horas zeradas apesar de justificativa aprovada. | HJ-093 | JUSTIFICATIVA-PAGA-O-PERIODO; HJ-090; decisão admin | ++....... | REGISTRADA (classe confirmada) | app/docs/BACKLOG.md:743 |
| API-RETIDOS-ROTULO | UI | Distingue quem está retido de quem está fora da folha. | - | - | .+....... | REGISTRADA | app/docs/BACKLOG.md:744 |
| LANCAR-AUSENCIA-AFASTA-DESLIGADO | LATERAL-ESTRUTURA | Impede que lançar uma ausência transforme um desligado em afastado. | - | corte para ampliar o escopo | .+....... | REGISTRADA | app/docs/BACKLOG.md:745 |
| FERIADO-RUBRICA-DO-FLAG-CORRENTE | LATERAL-ESTRUTURA | Evita que a escolha atual de feriado altere a rubrica histórica. | HJ-074 | impacto_g; decisão de desenho | ......... | REGISTRADA (desenho candidato) | app/docs/BACKLOG.md:746 |
| SELOS-CANON-I1 | INSTRUMENTO | Exige um levantamento aprovado antes de abrir uma construção. | - | SELOS-CANON-I1-FILA | .+++..++~ | REGISTRADA (núcleo no ar; fila restante) | app/docs/BACKLOG.md:747 |
| SELOS-CANON-I1-FILA | INSTRUMENTO | Fecha lacunas na fiscalização do levantamento obrigatório. | - | SELOS-CANON-I1 | .+....... | REGISTRADA | app/docs/BACKLOG.md:748 |
| SELOS-CANON-I3 | INSTRUMENTO | Exige etapas e resultados observáveis nas instruções da obra. | - | matriz antes da próxima volta; vaga | ++....... | REGISTRADA (brief antigo, fora dos anexos autorizados) | app/docs/BACKLOG.md:749 |
| SELOS-CANON-I2 | INSTRUMENTO | Recusa autorizações de dados sem universo e exclusões definidos. | - | SELOS-CANON-I3 | .+....... | PARADA | app/docs/BACKLOG.md:750 |
| PERGUNTA-SEM-SELO | ESPINHA-E | Faz leitores estruturais consultarem a autoridade correta. | - | FECHAR2; autorização sobre exportada | +++...... | REGISTRADA (construção histórica; atividade hoje não provada) | app/docs/BACKLOG.md:751 |
| PILHA-DA-BATIDA-RECALCULO-INLINE | LATERAL-ESTRUTURA | Reduz o trabalho de cálculo feito durante o registro da batida. | - | medir latência real; O-OUTBOX | +........ | REGISTRADA | app/docs/BACKLOG.md:753 |
| O-PROJECOES | ESPINHA-E | Faz telas e documentos lerem resultados reconstruíveis da célula. | - | REJULGAR-COMP10-RESTO | .+....... | REGISTRADA | app/docs/BACKLOG.md:756 |
| ADR-NAS-LEIS | INSTRUMENTO | Registra contexto e consequências de cada lei nova. | - | - | .+....... | REGISTRADA (conduta vigente) | app/docs/BACKLOG.md:757 |
| O-BITEMPORAL-2 | LATERAL-ESTRUTURA | Preserva quando vínculos e regras valeram e foram registrados. | - | contratos_estruturais 20/20 | .+....... | REGISTRADA | app/docs/BACKLOG.md:758 |
| IA-DOMINIO-POR-FERRAMENTA | LATERAL-ESTRUTURA | Limita respostas da mensageria aos dados que o usuário pode ver. | - | 20/20 | .+....... | REGISTRADA | app/docs/BACKLOG.md:759 |
| RECUSA-DO-ADMIN-DIZ-O-QUE-FAZER | UI | Explica ao admin a recusa de ausência e as ações possíveis. | - | BOs anteriores | .+....... | REGISTRADA | app/docs/BACKLOG.md:760 |
| TELA-6X1-NAO-MOSTRA-HORARIOS-POR-DIA | UI | Mostra os horários por dia na edição da escala 6x1. | - | fila 2 | .+....... | REGISTRADA | app/docs/BACKLOG.md:761 |
| JORNADA-EXCEPCIONAL-POR-DATA | CEREJA | Permite uma jornada excepcional para uma data específica. | - | 20/20; TROCA-DE-PLANTAO | .+....... | REGISTRADA | app/docs/BACKLOG.md:762 |
| LISTA-DE-QUARENTENA | UI | Mostra quem está em quarentena e permite levantá-la. | - | QUARENTENA-DO-COLAB | .+....... | REGISTRADA (incorporada à QUARENTENA) | app/docs/BACKLOG.md:763 |
| O148-EDITAR | UI | Permite editar ou arquivar calendários de folgas com histórico seguro. | - | corte sobre edição por versão | ......... | PARADA (lei nova pendente) | app/docs/BACKLOG.md:764 |
| LAUDO-[nome]-COL94-1610 | INSTRUMENTO | Mede divergências de batida e perguntas invisíveis ao admin. | - | resposta por item; medidas | .+....... | REGISTRADA (medição histórica) | app/docs/BACKLOG.md:765 |
| REJULGAR-COMP10-4-REVERTIDOS | LATERAL-ESTRUTURA | Reaplica correções revertidas com prova do que muda. | - | medida atual; SOMBRA-X-PROD-COL988-124639 | .+.....+. | REGISTRADA (aplicação feita; divergência restante) | app/docs/BACKLOG.md:766 |
| SOMBRA-X-PROD-COL988-124639 | INSTRUMENTO | Identifica por que o ensaio e a produção julgam diferente. | - | medir o insumo divergente | +........ | REGISTRADA | app/docs/BACKLOG.md:767 |
| TERMOMETRO-GRAVA-SEM-TRILHA | LATERAL-ESTRUTURA | Registra quem alterou o escore do vínculo e o motivo. | - | push junto do FIO | .++..+.+~ | REGISTRADA (no ar antigo; push pendente) | app/docs/BACKLOG.md:768 |
| SOMBRA-NAO-CABE-ANTES-DO-VIZINHO | INSTRUMENTO | Evita que o ensaio ultrapasse a janela dos comandos seguintes. | - | SOMBRA-PORTAO-POR-COMANDO | ++.....+. | REGISTRADA (instrumento parcial pousado) | app/docs/BACKLOG.md:769 |
| CRON-NAO-CABE-SOMBRA-87-MIN | INSTRUMENTO | Faz a rotina de ensaio caber no horário disponível. | - | N1-PERGUNTAS-DO-APP; LAVRAR-HE-PENDENTE-ESCALA; possível portão por comando | ++....... | REGISTRADA (prazo 13/11; cura não provada) | app/docs/BACKLOG.md:770 |
| TROCA-DE-POSTO-SEM-VINCULO | LATERAL-ESTRUTURA | Mantém coerentes o posto do cadastro e o vínculo vigente. | - | ? (sem precedência escrita) | +........ | REGISTRADA | app/docs/BACKLOG.md:771 |
| LAVRAR-HE-PENDENTE-ESCALA | LATERAL-ESTRUTURA | Acelera o cálculo de horas extras lendo a célula já carregada. | - | resíduos de leitores; decisão sobre segunda apuração | ..++.+.+~ | REGISTRADA (núcleo no ar; resíduos pendentes) | app/docs/BACKLOG.md:773 |
| N1-ALIMENTACAO-UNICA | LATERAL-ESTRUTURA | Evita carregar repetidamente os mesmos dados das portas. | - | N1; interface E3 se houver ampliação | +........ | REGISTRADA (não aplicável ao corte atual da E3) | app/docs/BACKLOG.md:774; app/docs/RELATO.md:71 |
| SOMBRA-PORTAO-POR-COMANDO | INSTRUMENTO | Exige ensaio atual dos comandos atingidos por cada mudança. | - | medir comandos tocados; SOMBRA-INCREMENTAL | .+....... | REGISTRADA | app/docs/BACKLOG.md:775 |
| JUIZ-DO-PAR-DUAS-ENTRADAS | ESPINHA-E | Faz motor e ata formarem os mesmos turnos. | - | MOTOR-PASSA-O-TURNO-A-VOLTA; grupo 1 | ++.....+~ | REGISTRADA (cura no ar; resíduos pendentes) | app/docs/BACKLOG.md:776 |
| BO-ADMIN-2710 | LATERAL-ESTRUTURA | Classifica os problemas do admin e os encaminha às obras responsáveis. | - | medidas; dono e destino por item | .+....... | REGISTRADA (medição histórica) | app/docs/BACKLOG.md:777 |
| TRILHA-DO-VINCULO-MENTE | LATERAL-ESTRUTURA | Faz o histórico do vínculo registrar corretamente as mudanças. | - | cura RT-03 restante; push FIO | ++...+.+~ | REGISTRADA (núcleo no ar; RT-03 não curado) | app/docs/BACKLOG.md:778 |
| TENTATIVA-DO-ADMIN-VIRA-COBRANCA | UI | Mostra o efeito da edição antes de gerar novas cobranças. | - | medir regenerações; proposta de tela; corte sobre confirmação | ......... | REGISTRADA (parte exige lei) | app/docs/BACKLOG.md:779 |
| REGISTRADO-NAO-E-VIVO | LATERAL-ESTRUTURA | Evita mostrar registros de auditoria como pendências de ação. | - | instrumento de fiscalização | .+.....+~ | REGISTRADA (produto no ar; instrumento pendente) | app/docs/BACKLOG.md:780 |
| PENDENCIA-SEM-JUIZ-3 | ESPINHA-E | Faz os três contadores de pendências usarem a mesma fila. | - | SILENCIO-VENCIDO-VOLTA | ++.....+~ | REGISTRADA (dois leitores no ar; falta um) | app/docs/BACKLOG.md:782 |
| SILENCIO-VENCIDO-VOLTA | LATERAL-ESTRUTURA | Faz cobranças reaparecerem quando termina seu silêncio. | - | QUARENTENA-DO-COLAB; E3 | .++...... | INTERROMPIDA | app/docs/BACKLOG.md:783; anexo:interrompidas.txt:1 |
| QUARENTENA-DO-COLAB | LATERAL-ESTRUTURA | Isola cobranças por período e as devolve quando a quarentena acaba. | - | reaplicar SPEC; DIFF de frota; E3 | .+++..~.. | INTERROMPIDA | app/docs/BACKLOG.md:784; anexo:interrompidas.txt:3 |
| COBERTURA-PARCIAL-UM-JUIZ | ESPINHA-E | Faz ausências e justificativas parciais cobrirem apenas o trecho devido. | - | COBPARC-DIFF-ISOLADO; autorização sobre exportada | .+++.+~+~ | PARADA (reaberta; PAREI de impacto) | app/docs/BACKLOG.md:786 |
| ATA-DEFASADA-COMP09 | LATERAL-ESTRUTURA | Distingue atas antigas de mudanças causadas pela correção. | - | COBPARC-DIFF-ISOLADO | ++....~.. | PARADA (hipótese refutada; itens voltam ao PAREI) | app/docs/BACKLOG.md:787 |
| GRADE-POS-POUSO | ESPINHA-E | Confere o efeito da nova grade após sua entrada em produção. | - | primeiro cartório pós-deploy; rebase COBPARC | .+....... | REGISTRADA | app/docs/BACKLOG.md:789 |
| COBPARC-CONTROLE | INSTRUMENTO | Compara a correção com um ensaio sem a correção. | - | explicar os chamados adicionais | ++....... | PARADA (COBPARC-DET-NAO-EXPLICA) | app/docs/BACKLOG.md:790 |
| COBPARC-ATA-DEFASADA-09 | LATERAL-ESTRUTURA | Corrige divergências estruturais restantes da competência exportada. | - | nova medida; Pauta DP | ++....... | REGISTRADA | app/docs/BACKLOG.md:791 |
| COBPARC-AUSENCIA-CALA-DOIS-JUIZES | ESPINHA-E | Faz a ausência parcial produzir a mesma cobrança em dois caminhos. | - | COBPARC | .+....... | REGISTRADA | app/docs/BACKLOG.md:792 |
| COBPARC-FUROS-LEITOR-PARCIAL | ESPINHA-E | Faz os leitores reconhecerem os furos restantes da ausência parcial. | - | COBPARC | .+....... | REGISTRADA | app/docs/BACKLOG.md:793 |
| FIO-LE-O-JUIZ | ESPINHA-E | Mostra ao admin perguntas vivas mesmo em disputas encerradas. | - | clique de validação; push | .++....+~ | REGISTRADA (no ar antigo; push pendente) | app/docs/BACKLOG.md:794 |
| O245-CAMADA-COBERTURA | INSTRUMENTO | Mostra quais autoridades e leitores ainda carecem de cobertura. | - | push FIO | .++..+.+~ | REGISTRADA (no ar antigo; push pendente) | app/docs/BACKLOG.md:795 |
| FECHAR3-SOBREVIVENTE-NO-RECONCILIAR | LATERAL-ESTRUTURA | Preserva o chamado correto quando dois registros são conciliados. | - | medir a ocorrência | .+....... | REGISTRADA | app/docs/BACKLOG.md:797 |
| CXR-TELA-LE-O-MOTOR | ESPINHA-E | Mostra ao admin os dias de cadastro divergentes apontados pelo motor. | - | ATA-X-TURNO-PAUSA; CXR-PRONTO5-DEFINICAO; conferir lavra | .+++..++~ | REGISTRADA (no ar; conclusão parcial) | app/docs/BACKLOG.md:798 |
| CXR-PRONTO1-COL841 | ESPINHA-E | Faz os sete dias pendentes chegarem à lista do admin. | - | ATA-X-TURNO-PAUSA | .+....... | REGISTRADA | app/docs/BACKLOG.md:799 |
| E1-ACERVO-5-GRUPOS-09 | ESPINHA-E | Trata cinco grupos antigos preservando a competência exportada. | - | REGEN-EM-EXPORTADA; DIFF; autorização própria | .+....... | PARADA (retida por aval) | app/docs/BACKLOG.md:800 |
| REJULGAR-61-COBPARC | LATERAL-ESTRUTURA | Reavalia atas antigas de ausência parcial pela regra atual. | - | autorização própria para 55 atas exportadas | .+....... | PARADA (aguarda autorização sobre exportada) | app/docs/BACKLOG.md:801 |
| RETRATADA-CONVERSA-E-REABRIR | ESPINHA-E | Impede abrir conversas sobre batidas já retratadas. | - | pouso E1 | .+....... | REGISTRADA | app/docs/BACKLOG.md:802 |
| E1-A2-VAZIO-MEDIR | ESPINHA-E | Identifica perguntas cuja origem não permite determinar o dia. | - | pouso E1; medida na sombra | .+....... | REGISTRADA | app/docs/BACKLOG.md:803 |
| E1-RECUSAS-SEM-CONTADOR | INSTRUMENTO | Conta recusas de perguntas com origem inválida ou sem dia. | - | pouso E1 | ......... | REGISTRADA | app/docs/BACKLOG.md:804 |
| E1-DUPLICATA-SOBRE-RETRATADA | ESPINHA-E | Trata perguntas duplicadas sobre um episódio retratado. | - | pouso E1; decisão junto da E3 | ......... | REGISTRADA | app/docs/BACKLOG.md:805 |
| CXR-PRONTO5-DEFINICAO | ESPINHA-E | Define e mede quando um dia ganhou dono na lista do admin. | - | definição do resultado esperado | .+....... | REGISTRADA | app/docs/BACKLOG.md:806 |
| CXR-MEDIDO-EM-AUTO-NOW | LATERAL-ESTRUTURA | Mostra a hora da medição sem confundi-la com a gravação. | - | - | .+....... | REGISTRADA | app/docs/BACKLOG.md:807 |
| CXR-BECO-SEM-BATIDA-FORA-DA-LAVRA | LATERAL-ESTRUTURA | Inclui na medição os colaboradores sem batidas que estão sem caminho. | - | medir universo faltante | ......... | REGISTRADA | app/docs/BACKLOG.md:808 |
| TETO-NA-LISTA-DOIS-RELOGIOS | LATERAL-ESTRUTURA | Usa uma única data para medir e publicar o teto do dia. | - | - | ......... | REGISTRADA | app/docs/BACKLOG.md:809 |
| TRAVA-COLAB-X-DISPUTA | LATERAL-ESTRUTURA | Investiga uma possível disputa entre travas de colaborador e conversa. | - | reproduzir com transação externa | ......... | REGISTRADA (hipótese não reproduzida) | app/docs/BACKLOG.md:811 |
| JUIZ-DO-PAR-GRUPO-1 | ESPINHA-E | Faz os leitores de batida e marco consultarem a mesma autoridade. | - | pouso 2 detector; LASTRO-DEGRAU-2 | ++.....+~ | REGISTRADA (primeiro pouso no ar; grupo incompleto) | app/docs/BACKLOG.md:812 |
| MOTOR-PASSA-O-TURNO-A-VOLTA | ESPINHA-E | Faz o cálculo reconhecer corretamente a volta do intervalo. | - | JUIZ-DO-PAR-GRUPO-1; DIFF dinheiro; reversão | ++....... | REGISTRADA | app/docs/BACKLOG.md:813 |
| DISPARAR-PERGUNTAS-NO-CONTRATO-20 | 20/20 | Faz o disparo de perguntas depender da célula e do caminho devido. | - | E3-A; PAR v5; fio mudo; C11 | ++....... | REGISTRADA (censo fechado; obra aberta) | app/docs/BACKLOG.md:814; app/docs/BACKLOG.md:2076 |
| JUSTIFICATIVA-ADMIN-FORA-DA-PORTA | LATERAL-ESTRUTURA | Impede aprovações de justificativa sem efeitos e histórico completos. | - | - | ......... | REGISTRADA | app/docs/BACKLOG.md:815 |
| O261-COMPLEMENTO-[nome] | ESPINHA-E | Faz perguntas vivas chegarem ao colaborador por um chamado vivo. | - | O261; cura do nascimento; contador de alcance | .++...... | REGISTRADA (parte construída; restante pendente) | app/docs/BACKLOG.md:816 |
| REATAR-ESCREVE-EM-DIA-TRANCADO | LATERAL-ESTRUTURA | Evita alterar perguntas de dias protegidos pela tranca. | - | medir na sombra | .+....... | REGISTRADA | app/docs/BACKLOG.md:817 |
| C6-TETO-MARCO-PENDENTE | ESPINHA-E | Evita cobrar um marco antes de terminar seu prazo. | - | cura no juiz C6 | .+....... | REGISTRADA | app/docs/BACKLOG.md:818 |
| PASSIVO-VALIDADO-K6 | ESPINHA-E | Usa a autoridade correta para reconhecer competência entregue. | - | juiz K6 | .+....... | REGISTRADA | app/docs/BACKLOG.md:819 |
| PROGRAMA-CELULA-E-O-DIA | ESPINHA-E | Faz todas as telas lerem o mesmo julgamento do dia. | - | E1; E2; E3; E4 | .++...... | REGISTRADA (programa parcial) | app/docs/BACKLOG.md:821 |
| PROGRAMA-CELULA-E-O-DIA-DUAS-PISTAS | ESPINHA-E | Organiza a unificação do status e a certificação do cálculo. | - | E2; E3-STATUS; E4; casos E1; prioridade STATUS | .++...... | REGISTRADA (construção histórica; atividade hoje não provada) | app/docs/BACKLOG.md:822 |
| E4-CAL-RETIDO-FOLHA-NO-LEITOR | ESPINHA-E | Faz o calendário ler o status pronto da folha. | - | E4-CALENDARIO-L1; D4 ADR 0002 | .+....... | REGISTRADA (complemento) | app/docs/BACKLOG.md:823 |
| E4-CAL-FALLBACK-STATUS-ROTULO | ESPINHA-E | Faz o calendário usar um único rótulo do dia. | - | E4-CALENDARIO-L1 | .+....... | REGISTRADA (complemento) | app/docs/BACKLOG.md:824 |
| E4-CAL-DOIS-CONTRATOS-DE-CELULAS | ESPINHA-E | Unifica o formato das células recebidas pelo calendário. | - | E4-CALENDARIO-L1 | .+....... | REGISTRADA (complemento) | app/docs/BACKLOG.md:825 |
| E4-INCLUI-CONTRATO-20 | ESPINHA-E | Faz telas e rotinas lerem o status lavrado na célula. | - | E3; lista única dos leitores | .+....... | REGISTRADA | app/docs/BACKLOG.md:826 |
| GRADE-FOLGA-DO-VINCULO | ESPINHA-E | Impede que folgas de vínculos encerrados reapareçam na grade. | - | E4-1 | .++...... | REGISTRADA (construção histórica; atividade hoje não provada) | app/docs/BACKLOG.md:827 |
| ODOMETRO-DE-REGRAS | INSTRUMENTO | Detecta regras duplicadas ou excessivas antes da publicação. | - | pouso CLAUDE-MD-RACIONAL; contrato de instrumento | .+....... | REGISTRADA | app/docs/BACKLOG.md:829 |
| REPROVA-E-DESENHO | INSTRUMENTO | Para uma obra reprovada até registrar sua decisão de desenho. | - | linha DESENHO ou ERRO-BOBO | .++...... | REGISTRADA (contrato e construção históricos) | app/docs/BACKLOG.md:830 |
| REPROVA-E-DESENHO-CODEX | INSTRUMENTO | Exige decisão quando as revisões acumulam achados procedentes. | - | REPROVA-E-DESENHO; suite v4 | .+....... | REGISTRADA | app/docs/BACKLOG.md:831 |
| TRAVA-MENOS-FILA | INSTRUMENTO | Reduz testes repetidos e mostra a espera na fila de testes. | - | contrato do instrumento | .+....... | REGISTRADA (conduta parcial vigente) | app/docs/BACKLOG.md:832 |
| TRAVA-COBERTURA | INSTRUMENTO | Mantém os testes vinculados à obra e remove esperas superadas. | - | vaga de instrumento; sete curas; COPIA-DO-HEAD | .++..~... | REGISTRADA (volta 4 aguardando vaga) | app/docs/BACKLOG.md:834 |
| ADR-0003-CELULA-TRES-NATUREZAS | INSTRUMENTO | Documenta quais dados da célula são fatos ou referências vivas. | - | CORRECAO-ADR-0003; censo | ~+....... | RE

[truncado em 20000 de 22390 caracteres]

## 13/11 20:30 · sessao b1fb4ff003ff · - · - · origem=Ronald (tmux)

| V2-I2-FILA | LATERAL-ESTRUTURA | Corrige divergências entre cálculo, espelho e exportação. | - | corte sobre pontas do intervalo | .-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1877 |
| V2-I2-COL270-DONO | LATERAL-ESTRUTURA | Identifica a origem da mudança no previsto antes de aplicar a correção. | - | investigação do previsto | .-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1878 |
| MOTOR-LE-O-TURNO | ESPINHA-E | Faz cálculo e pagamento usarem o mesmo turno oficial. | - | E2 e E3; DIFF; autorização para exportadas | .-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1879 |
| MOTOR-TURNO-INSTR-FILA | INSTRUMENTO | Completa as provas e os limites do medidor de turnos. | - | FATIA 1 de MOTOR-LE-O-TURNO | .-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1881 |
| DISJUNTOR-B3-TARDIO | INSTRUMENTO | Mostra quando o alarme chega depois da publicação. | - | medir latência da varredura | .-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1882 |
| HOLERITE-ZIP-MATRICULA | LATERAL-ESTRUTURA | Retira o envio antigo de holerites sem uso pela tela. | - | E1 e E2 | +-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1883 |
| HOLERITE-CAMPO-MATCH-LIVRE | LATERAL-ESTRUTURA | Impede o envio de holerites com identificação escolhida livremente. | - | HOLERITE-ZIP-MATRICULA; E1 e E2 | +-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1884 |
| PLACAR-LIVENESS | INSTRUMENTO | Mostra cinco sinais de pendências reais antes do faturamento. | - | contrato do instrumento; primeira medida | .-.-+-.-.-.-.-.-. | REGISTRADA | BACKLOG:1885 |
| FIO-COM-ASSENTO-E0-V3 | INSTRUMENTO | Registra a avaliação do desenho anterior das obrigações. | - | desenho v4 sucessor | +-.-.-.-.-+-.-.-. | REGISTRADA (rejeitada em 06/11) | BACKLOG:1886 |
| FIO-E0-V4-E1-ENTRADAS | ESPINHA-E | Faz as perguntas nascerem ligadas ao fato correto. | - | decisões de desenho; suite; sombra; opus | +-+-.-+-.-+-.-.-. | PARADA (último estado da linha; retomada própria não comprovada em 13/11) | BACKLOG:1888 | [nao verificado]
| FIO-E0-V4-E1-FILA | ESPINHA-E | Corrige travas e leitores restantes do nascimento das perguntas. | - | resolver achados f1–f4 e ordem das travas | .-.-.-+-.-+-+‑+‑+ | REGISTRADA (parte no ar; resíduos pendentes) | BACKLOG:1889 |
| BUG-DEPLOY-PORTAO-ANTES-DO-MIGRATE | INSTRUMENTO | Confere as condições de publicação antes de alterar o banco. | - | DEPLOY-PORTAO-RESTOS-V6 | .-.-.-+-.-+-+‑+‑+ | REGISTRADA (pouso anterior; restos pendentes) | BACKLOG:1890 | [nao verificado]
| DEPLOY-RESPAWN-SEM-PORTAO | INSTRUMENTO | Mede o risco de processos reiniciarem com código sem conferência. | - | medir reinícios por dia | .-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1891 | [nao verificado]
| ATOMICIDADE-DA-PORTA-DIVIDA | INSTRUMENTO | Torna visível a dívida de operações que escapam da transação. | - | cura das violações medidas | +-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1892 |
| SIGPIPE-PRINTF-CAT-GREP-Q | INSTRUMENTO | Evita falhas indevidas nas verificações por comandos encadeados. | - | ? (sem precedência escrita) | .-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1893 |
| E1-FILA-V2-3-A-6 | ESPINHA-E | Corrige leituras, relógios e travas restantes das perguntas. | - | fila da E1 | .-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1894 |
| E1-FILA-OPUS6-FILA | ESPINHA-E | Apura riscos e custos que sobraram da revisão da E1. | - | medir acervo e custo | .-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1895 |
| E1-FILA-V2-2-TRAVA | ESPINHA-E | Trava todos os registros envolvidos antes de transferir perguntas. | - | pendências A3/A4/B3/B4 | .-.-.-+-.-+-+‑+‑+ | REGISTRADA (parte no ar; resíduos pendentes) | BACKLOG:1896 |
| E1-FILA-V2-2-OUTRAS-PORTAS | ESPINHA-E | Alinha as demais entradas à mesma ordem de travas. | - | censo das portas | +-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1897 |
| E1-FILA-V2-2-RESIDUOS | ESPINHA-E | Esclarece documentos e mede o custo da transferência. | - | medição SQL | .-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1898 |
| DEPLOY-PORTAO-RESTOS-V6 | INSTRUMENTO | Corrige mensagens e testes restantes da publicação. | - | BUG-DEPLOY-PORTAO-ANTES-DO-MIGRATE | .-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1899 | [nao verificado]
| DEPLOY-EXCLUSAO-ENVELOPE | INSTRUMENTO | Evita que publicação e tarefas agendadas concorram na mesma janela. | - | E2; contrato do instrumento | .-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1900 | [nao verificado]
| FIO-E0-V4-E3-LEITORES | 20/20 | Faz telas e contadores mostrarem a mesma tarefa do dia. | - | cura da porta; preverif; suite; sombra; opus | +-+-+-+-.-~‑.-.-. | EM VOO | BACKLOG:1902; RELATO:6; RELATO:33 | [nao verificado]
| E3-MATRIZ-DA-RESPOSTA | 20/20 | Documenta e protege as entradas que gravam respostas. | - | duas portas Q1/Q2; continuidade da E3 | +-+-+-.-.-.-.-.-. | REGISTRADA | BACKLOG:1903 |
| E4B-D2-ANCORA-RAIOX | LATERAL-ESTRUTURA | Faz o diagnóstico apontar o mesmo chamado usado pelo fio. | - | correção da âncora | .-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1904 |
| E4B-D3-ARQUIVAR-PRESCRITOS | LATERAL-ESTRUTURA | Mede quais pendências ficam sem responsável com o arquivamento desligado. | - | medição do comando desativado | .-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1905 |
| E3-X1-FOLGA-NAO-E-SEM-COBERTURA | 20/20 | Distingue folga de falta de cobertura no julgamento do dia. | - | c8b | +-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1906 |
| E3-V15-A-S | 20/20 | Encaminha ao administrador os fatos sem emissor ou responsável definido. | - | c8b; preservar C6 | +-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1907 |
| E3-CADASTRO-ADMIN | 20/20 | Entrega ao administrador os cadastros que precisam de decisão humana. | - | correção de cadastro pelo administrador | +-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1908 |
| E3-X2-NUNCA-JULGADAS | 20/20 | Identifica células que ainda não receberam julgamento. | - | possível comboio com X1 | +-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1909 |
| E3-COL962-C6-X-LAMPADA | 20/20 | Apura a discordância entre a pergunta viva e a marcação do dia. | - | medição da divergência | .-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1910 |
| RAIZ-DA-DISPUTA-F0F1 | ESPINHA-E | Define uma única origem e um único destino para as disputas. | - | E3; E4B c4; F2 para corrigir produção | +-+-.-.-.-+-.-.-. | REGISTRADA | BACKLOG:1912 |
| ORDEM-ATE-O-ZERO | INSTRUMENTO | Ordena as obras até zerar os dias sem caminho de resolução. | - | E3; E4B; E4C; E4D; O169/O168/LASTRO | ~‑.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1913 |
| INTERMITENTE-SEM-CONVOCACAO | LATERAL-ESTRUTURA | Define como o dia convocado aparece na escala intermitente. | - | decisão sobre convocação | .-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1914 |
| FIO-E0-V4-E4-ESCRITORES | ESPINHA-E | Substitui estados paralelos por atos registrados da pergunta. | - | E4B; E4C; E4D | +-+-.-+-.-+-+‑+‑+ | REGISTRADA (E4a-1 no ar; restante pendente) | BACKLOG:1915 |
| FIO-E0-V4-E4-RETIRADA-FISICA | ESPINHA-E | Retira os campos antigos depois que ninguém mais os usar. | - | série E fechada; zero leitores | +-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1916 |
| FIO-E0-V4-FATO-A03 | ESPINHA-E | Registra a reabertura do administrador como fato próprio. | - | E4D | .-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1917 |
| FIO-E0-V4-FATO-A05 | ESPINHA-E | Registra a assinatura como fato próprio da pergunta. | - | E4D | .-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1918 |
| FIO-E0-V4-FATO-A06 | ESPINHA-E | Registra a identidade e a duplicação como fatos próprios. | - | E4D | .-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1919 |
| FIO-E0-V4-FATO-A17 | ESPINHA-E | Registra a pendência técnica sem depender do estado antigo. | - | E4D | .-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1920 |
| FIO-E0-V4-FATO-A18 | ESPINHA-E | Registra a recusa técnica sem depender do estado antigo. | - | E4D | .-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1921 |
| FIO-E0-V4-FATO-A20 | ESPINHA-E | Registra a conferência de data com significado explícito. | - | E4D; decisão sobre mudança dos dias | .-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1922 |
| FIO-E0-V4-E4B | 20/20 | Repara perguntas antigas pela mesma entrada usada nas correções normais. | - | itens 18–19 no fim do programa; autorização para exportadas | +-+-+-+-.-+-+‑+‑+ | REGISTRADA (pousou em 12/11; partes finais pendentes) | BACKLOG:1924 | [nao verificado]
| E4B-C12-DIFF-DE-PAUTA | 20/20 | Confere se a correção cria somente as pautas permitidas. | - | comparação c12 com base | .-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1925 |
| E4B-FILA-OPUS-C14 | LATERAL-ESTRUTURA | Corrige vínculos de pautas e provas restantes do reparo antigo. | - | fila após o 20 | .-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1926 |
| BO-0611-05 | LATERAL-ESTRUTURA | Permite registrar ponto sem coordenadas de GPS. | - | liberação B3; revisão; smoke | +-+-.-+-.-+-.-.-. | PARADA (estado da linha; encerramento próprio não verificado) | BACKLOG:1927 |
| BO-DISPUTA-SEM-PERGUNTA | ESPINHA-E | Impede uma disputa nascer sem pergunta para responder. | - | RAIZ-DA-DISPUTA F1/F2; BO-0611-05 | +-+-.-.-.-.-.-.-. | REGISTRADA (brief antigo fora dos anexos autorizados para BRIEF PRONTO) | BACKLOG:1928 |
| BO-DISPUTA-MATRIZ-ANTES-DO-OPUS | ESPINHA-E | Confere todos os caminhos de nascimento da disputa antes da revisão final. | - | matriz pelo Codex; BO-DISPUTA-SEM-PERGUNTA | ~‑+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1929 |
| MAPA-DA-FILA-BO | INSTRUMENTO | Liga cada problema registrado à sua correção estrutural. | - | lote restante de achados | +-.-.-.-.-+-.-.-. | REGISTRADA | BACKLOG:1930 |
| BO-0611-05-COMPLEMENTO-POUSO | LATERAL-ESTRUTURA | Confere férias, desligamento e passos de teste antes da publicação. | - | BO-0611-05; smoke pela saída A | +-+-.-+-.-+-.-.-. | REGISTRADA | BACKLOG:1931 | [nao verificado]
| BO-0611-05-DESLIGADO-FERIAS | LATERAL-ESTRUTURA | Define o efeito financeiro do ponto em férias ou após desligamento. | - | duas decisões de lei pelo DP | +-.-.-+-.-.-.-.-. | REGISTRADA | BACKLOG:1932 |
| BUG-PAR-FROTA-1110 | ESPINHA-E | Resolve pausas com uma metade validada e a outra ainda pendente. | - | BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS; RAIZ F3/F4 | +-.-.-.-.-.-.-.-. | REGISTRADA (absorvida pela obra indicada) | BACKLOG:1933 |
| RAIZ-DA-DISPUTA | ESPINHA-E | Faz perguntas e disputas reagirem juntas aos mesmos atos. | - | F0–F7; decisões de F1; E4D em F5 | +-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1934 |
| REGULARIZAR-TRAVA-PELO-DIA | LATERAL-ESTRUTURA | Impede uma disputa de outro dia bloquear a regularização pedida. | - | RAIZ F1; UI-REGULARIZAR item 4 | .-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1935 |
| PORTA-QUE-NAO-AGE | INSTRUMENTO | Mede dias que oferecem uma ação sem caminho real para executá-la. | - | MAPA-DO-FIO-MORTO sucessor | ~‑.-.-.-.-.-.-.-. | REGISTRADA (substituída; despacho aberto na mesma célula) | BACKLOG:1936 |
| SAIDA-NOTURNA-NO-DIA-CERTO | LATERAL-ESTRUTURA | Evita rejeitar uma saída noturna por usar o dia errado. | - | pouso E3-LEITORES; matriz D1–D3 | ~‑+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1937 | [nao verificado]
| RESPOSTAS-1211-1230 | INSTRUMENTO | Reúne encaminhamentos para cadastro, fila, limites e lastro. | - | obras correspondentes dos cinco encaminhamentos | +-+-.-.-.-~‑.-.-. | REGISTRADA | BACKLOG:1938 |
| DECLARADO-X-GRAVADO | LATERAL-ESTRUTURA | Faz o horário gravado respeitar o que foi declarado na resposta. | - | censo; construtor; passivo separado | +-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1939 |
| DECLARADO-X-GRAVADO-PASSIVO-11 | LATERAL-ESTRUTURA | Corrige o passivo da competência aberta após conferir o impacto. | - | DIFF antes do apply; exportadas em pauta | .-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1940 |
| E3-10-CURA-NO-ESCRITOR | 20/20 | Corrige na origem as respostas carimbadas sem texto. | - | censo do escritor; prova do efeito no aplicativo | ~‑+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1941 |
| O168-GUARDA-NO-GATE-DO-PAI | 20/20 | Mantém o chamado aberto somente enquanto houver resposta sem julgamento. | - | - | +-+-+-+-+-+-+‑+‑+ | NO AR hoje | BACKLOG:1942; RELATO:5; RELATO:7; RELATO:31; RELATO:34 |
| E3-FORMA-SQL-DO-JUIZ | 20/20 | Faz consultas ao banco usarem a mesma regra de resposta sem texto. | - | cura da porta; suite; DIFF; opus | +-+-.-+-.-~‑.-.-. | EM VOO | BACKLOG:1943; RELATO:6; RELATO:14; RELATO:38 | [nao verificado]
| E3-IMPACTO-E-CONTRATOS-DE-ENTRADA | 20/20 | Confere mudanças de ação e memória antes de publicar a E3. | - | conclusão A1–A3 antes do opus | ~‑.-.-.-.-~‑.-.-. | EM VOO | BACKLOG:1944; RELATO:38 |
| HOOK-B3-PAREI-LEGITIMO | INSTRUMENTO | Permite encerrar a espera quando a liberação depende do administrador. | - | contrato do instrumento | .-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1945 |
| F4-PAR-PELA-CELULA | ESPINHA-E | Usa os marcos da célula para identificar o par da pausa. | - | RAIZ F3/F4; decisão D8 | +-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1946 |
| RAIA-2-BO-VISIVEL | INSTRUMENTO | Ordena a correção dos problemas visíveis na segunda fila. | - | O169; rótulo; triagem; regressão do lote | ~‑.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1947 |
| MAPA-ATE-O-20 | INSTRUMENTO | Publica um mapa das obras e dos dias sem resolução. | - | censo; sonda; selo do publicador | ~‑.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1948 | [nao verificado]
| GABARITO-FABLE-VERIFICADOR | INSTRUMENTO | Compara revisores usando os mesmos casos e uma regra prévia. | - | seleção de casos; regra de corte | ~‑.-+-.-.-.-.-.-. | REGISTRADA | BACKLOG:1949 |
| BO-ROTULO-ENCERRADO | UI | Mostra nomes diferentes para chamado resolvido e fechado. | - | selo da saída renderizada | .-+-.-.-.-~‑.-.-. | REGISTRADA | BACKLOG:1950 |
| SONDA3-METODO-CORRECAO | INSTRUMENTO | Mede correções com a permissão real de quem executa a ação. | - | nova sonda com perfil real | .-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1951 |
| DISPUTA-ZUMBI-1110 | ESPINHA-E | Faz a disputa encerrar pelo mesmo juiz em todos os caminhos. | - | C7; RAIZ F3/F4 | +-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1952 |
| PERGUNTA-DUPLICATA-1110 | ESPINHA-E | Apura perguntas repetidas e seus efeitos nos contadores. | - | E4D; medir idade e efeito | +-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1953 |
| AUDITORIA-TRAVAS-POS-RESET-PADRAO-NA | INSTRUMENTO | Avalia o bloqueio de publicações que alteram somente comentários. | - | AUDITORIA-DE-TRAVAS | .-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1954 |
| BO-0611-05-RESTO-DA-CLASSE | LATERAL-ESTRUTURA | Apura outras barreiras que podem impedir o registro do ponto. | - | MOTOR-LE-O-TURNO; decisões de lei indicadas | +-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1955 |
| TRIAGEM-OFFLINE-SEM-CAMERA | UI | Permite guardar o ponto offline quando a câmera falha. | - | classe nomeada; RED offline; smoke nas duas cascas | +-+-.-.-.-+-.-.-. | PARADA (revertida; retomada condicionada) | BACKLOG:1956 |
| APP-MENSAGENS-MSG-0 | LATERAL-ESTRUTURA | Liga a justificativa à cobrança correta pela mesma função. | - | TRIAGEM-OFFLINE-SEM-CAMERA; arquivos livres | +-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1957 |
| APP-MENSAGENS (MSG-1..4) | UI | Reúne acesso a mensagens e envio de mídia no aplicativo. | - | E4C D05; RAIZ F1 | .-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1958 |
| GABARITO-FABLE-VIRA-SOMBRA | INSTRUMENTO | Compara cinco revisões independentes sem travar a publicação. | - | próximas cinco finais; primeira UI-PAINEL | .-.-+-.-.-.-.-.-. | REGISTRADA | BACKLOG:1959 |
| GABARITO-FABLE-SOMBRA-AJUSTE | INSTRUMENTO | Separa falhas perdidas de diferenças de gravidade na comparação. | - | GABARITO-FABLE-VIRA-SOMBRA | .-.-.-.-.-~‑.-.-. | REGISTRADA | BACKLOG:1960 |
| DATA-EVENTO-CRU | ESPINHA-E | Identifica leituras de data que escapam do juiz do turno. | - | construir catraca a partir do censo | +-+-.-.-.-+-.-.-. | REGISTRADA | BACKLOG:1962 |
| BATIDA-ID-DO-CLIQUE | LATERAL-ESTRUTURA | Identifica cada clique para evitar registros duplicados de ponto. | - | migration; mudança PWA; build nativo posterior | .-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1963 |
| MAPA-20-DERIVADO | INSTRUMENTO | Calcula o avanço das obras a partir de provas referenciadas. | - | GRADE-MESTRA sucessora | .-.-.-.-.-+-.-.-. | REGISTRADA (substituída hoje pela GRADE-MESTRA) | BACKLOG:1964; anexo:aval.txt:1 |
| BO-ROTULO-GLPI-SELETOR-MORTO | UI | Faz a opção de mudar estado produzir a mudança prometida. | - | aval próprio | .-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1965 |
| BO-ROTULO-REABRIR-RECUSADO | UI | Evita oferecer reabertura que o perfil não pode executar. | - | correção da oferta de ação | .-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1966 |
| BO-ROTULO-TERCEIRO-VOCABULARIO | UI | Usa os mesmos nomes de estado nas diferentes telas. | - | unificação dos rótulos | .-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1967 |
| BO-ROTULO-FILA-DA-FORMA | INSTRUMENTO | Completa a conferência das formas de mudar estado pela tela. | - | fila de achados; PROPORCAO-POR-RISCO | .-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1968 |
| TRIAGEM-OFFLINE-FILA | UI | Corrige pendências de foto, localização e textos da triagem. | - | fila da triagem offline | .-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1969 |
| PRONTO-AUSENTE-RASCUNHO | INSTRUMENTO | Lista o que falta em cada obra para o administrador conferir. | - | conferência humana | +-.-.-.-.-+-.-.-. | REGISTRADA | BACKLOG:1970 |
| BO-0611-05-TEM-GPS-LEITORES | LATERAL-ESTRUTURA | Faz todos os leitores concordarem sobre a existência de GPS. | - | nascimento do juiz tem_gps na volta 7 | +-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1971 |
| GPS-ESTADO-NA-TELA | UI | Mostra se a localização está atual, indisponível ou desatualizada. | - | BO-DISPUTA; ocupação E4B; smoke | +-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1972 |
| ADMIN-FECHA-SEM-MOTIVO | LATERAL-ESTRUTURA | Exige motivo e trilha ao encerrar um chamado. | - | correção da porta administrativa | +-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1973 |
| BRIEF-COM-PROCEDENCIA | INSTRUMENTO | Exige que a orientação da obra traga a origem de cada decisão. | - | AUDITORIA-DE-TRAVAS; replay do brief | +-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1974 |
| ABSOLVIDA-PELA-FOLGA | LATERAL-ESTRUTURA | Evita encerrar uma cobrança noturna pela folga do dia errado. | - | autorização para exportadas; construção futura | +-+-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1976 |
| LOTE-VALIDAR-REGRESSAO-0311 | LATERAL-ESTRUTURA | Apura a regressão da validação em lote antes de corrigi-la. | - | fila BO anterior; medir causa | .-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1977 |
| PREVIEW-REOFERECE-RECUSADA | LATERAL-ESTRUTURA | Evita oferecer novamente respostas já recusadas no lote. | - | LOTE-VALIDAR-VIRA-JOB | +-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1978 |
| BO-0611-07-E4B-COBRE | 20/20 | Confere se o reparo antigo cobre o problema registrado. | caso 07 | DRY da E4B | .-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1979 |
| BO-0611-10-TELA-DA-TROCA | UI | Acrescenta uma tela para a troca registrada no problema. | caso 10 | fim da série E | .-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1980 |
| LAVRA-PREMATURA-CONCORDE | ESPINHA-E | Define se o dia pode ser confirmado antes de terminar o turno. | - | decisão de desenho | .-.-.-+-.-+-.-.-. | PARADA (desenho registrado; construção vedada) | BACKLOG:1981 |
| HOOK-ESPERANDO-PID-MORTO | INSTRUMENTO | Avisa quando o processo aguardado termina sem destravar a fila. | - | contrato e construção futura | +-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1982 |
| PLACAR-FABRICA-MTIME | INSTRUMENTO | Corrige a comparação de datas usada pelo placar da fábrica. | - | pouso próprio | .-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1983 | [nao verificado]
| FIO-ESTADO-CARIMBO-VELHO | INSTRUMENTO | Impede carimbo antigo de esconder campos obrigatórios novos. | - | pouso próprio | .-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1984 | [nao verificado]
| CARIMBO-DIA-CIVIL-X-TURNO | INSTRUMENTO | Distingue o dia civil do dia do turno no placar do fio. | - | medir antes | .-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1985 |
| FIO-E0-V4-VIGIA-FABRICA-POR-EVENTO-CRESCE | ESPINHA-E | Apura perguntas que crescem no vigia sem serem emitidas. | - | conferência da cobertura C20 | +-.-.-.-.-.-.-.-. | REGISTRADA | BACKLOG:1986 |
| E4A1-C6-X-PASSADO-SEM-TURNO | 20/20 | Corrige a retirada de carimbo em dias passados sem turno. | - | CONTRACT E4B/E4C | +-.

[truncado em 20000 de 21524 caracteres]

## 13/11 20:30 · sessao 9badbaf3f25b · - · - · origem=Ronald (tmux)

| PAR-NAO-ADJACENTE | LATERAL-ESTRUTURA | Preserva o par correto quando há uma batida solta entre suas pontas. | - | LEI-PAR-ANINHADO; volta 5 | . + + + . . . . . | PARADA (versão verde contradiz a lei) | app/docs/BACKLOG.md:1192 |
| O261-E-F2-FILA | LATERAL-ESTRUTURA | Corrige recusas caladas, relógios e identidades no tratamento das perguntas. | - | pouso O261-E-F2 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1193 |
| O261-E-F2-FILA-V2 | LATERAL-ESTRUTURA | Completa os testes e corrige informações inconsistentes das perguntas. | - | pouso O261-E-F2 | + . . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1194 |
| REABERTURA-RECUSADA-DEDUP | LATERAL-ESTRUTURA | Evita repetir o registro da mesma reabertura recusada. | - | vez na fila | + . . . . . . . . | PARADA (construtor cancelado sem commit) | app/docs/BACKLOG.md:1195 |
| O232 | LATERAL-ESTRUTURA | Faz a ata reconhecer a saída tardia do mesmo turno. | - | complemento de 25/10; impacto de frota | + + + . . . . + + | REGISTRADA (A/B no ar; complemento da ata pendente) | app/docs/BACKLOG.md:1196 |
| O219 | ESPINHA-E | Rejulga os dias quando muda a regra, respeitando seu alcance e período. | - | TXT novo da 09; Pauta DP | + + + . . . . + + | REGISTRADA (regra no ar; ato do TXT e pauta pendentes) | app/docs/BACKLOG.md:1197 |
| O223 | UI | Permite cadastrar pela tela a convenção e o regime da empresa. | - | smoke do pouso A para push | + + + . . . . + + | NO AR hoje (smoke para push pendente) | app/docs/BACKLOG.md:1198 |
| ADMIN-ESCREVE-EMPRESA-FORA-DA-PORTA | ? (classificação estrutural ainda por decidir) | Faz o administrador alterar a empresa pelo mesmo caminho auditável da produção. | - | decidir forma; contratos | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1199 |
| O228 | ESPINHA-E | Remove código obsoleto das famílias de célula e turno com prova de desuso. | - | - | + + + + . . . + + | REGISTRADA (inteira no ar; data do fecho ?) | app/docs/BACKLOG.md:1201 |
| O234 | INSTRUMENTO | Impede publicar contagens de cenários e testes que já ficaram antigas. | - | vez de instrumento | + . . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1202 |
| O236 | LATERAL-ESTRUTURA | Identifica por que colaboradores ficam fora do arquivo da folha. | - | medição por motivo; autorização se tocar exportada | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1203 |
| O237 | INSTRUMENTO | Faz os scripts usarem uma única porta de medição na sombra. | - | migrar três scripts; verificar 19 chamadores | + . . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1204 |
| O238 | LATERAL-ESTRUTURA | Faz a trilha apontar o registro certo e distinguir escrita de mudança de valor. | - | O219 A0-b | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1205 |
| O240 | INSTRUMENTO | Evita aprovar uma suíte interrompida por confundir mensagem de log com resultado. | - | função única de leitura do veredito | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1207 |
| O241 | INSTRUMENTO | Identifica quais alterações de duas raias ainda faltam no código principal. | - | leitura dos 7+7 commits | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1208 |
| O242 | INSTRUMENTO | Faz o teste da corrente usar a própria cópia e preservar carimbos reais. | - | pouso próprio após produto | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1209 |
| O227 | INSTRUMENTO | Permite consultar uma competência exportada sem pedir autorização de escrita. | - | instrumento após O211 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1210 |
| O226 | INSTRUMENTO | Unifica a resposta sobre quais parâmetros realmente afetam o cálculo. | - | censo dos leitores; decidir autoridade | . + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1211 |
| O225 | UI | Faz os avisos de parâmetro sem efeito acompanharem a declaração oficial. | - | vez de UI | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1212 |
| O218 | ESPINHA-E | Certifica que a lavratura produz uma ata correta e estável em diferentes cenários. | - | cura O217; cenários; relavra de col146 | + + . . . . . . . | REGISTRADA (instrumento no ar; bateria incompleta) | app/docs/BACKLOG.md:1214 |
| O221 | ESPINHA-E | Integra ao código principal as curas de escrita única e de julgamento pelas autoridades. | - | marco O218; suíte da junção | + + + . . . . + + | REGISTRADA (fonte diz pousos no ar e também nada juntado) | app/docs/BACKLOG.md:1215 |
| O212 | INSTRUMENTO | Mantém na sombra os vínculos necessários para medir os mesmos dias da produção. | - | pouso próprio de instrumento | + . . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1218 |
| O128 | UI | Mostra na lista de escalas os quadros que a página já prepara. | - | smoke O122 ou retorno do template | + . . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1219 |
| O129 | ESPINHA-E | Declara o efeito dos 28 campos editáveis do assistente de escalas. | - | O127 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1220 |
| O127 | LATERAL-ESTRUTURA | Faz a classificação das batidas considerar todas as pausas cadastradas. | - | O124; DIFF de frota | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1222 |
| O140 | ESPINHA-E | Grava o furo do dia uma vez para as telas não recalcularem a mesma resposta. | - | campo e porta de furo_apurado; backfill autorizado | + . . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1224 |
| BUG-8792 | ESPINHA-E | Faz a folha ler as faltas do dia já julgado pela célula. | - | O140 no mesmo marco | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1225 |
| O121 | LATERAL-ESTRUTURA | Permite corrigir o vínculo passado pela tela com regeneração e trilha. | - | remedicação; O65; inverter seis testes antigos | + + + + . . . . . | REGISTRADA (A1 construído; suíte com seis falhas) | app/docs/BACKLOG.md:1228 |
| O122 | UI | Melhora a navegação do plano de escalas. | - | nova entrega e smoke de UI | . + + . . . . . . | REGISTRADA (etapa 1 desfeita pelo gate) | app/docs/BACKLOG.md:1229 |
| O123 | LATERAL-ESTRUTURA | Carrega as pendências de disputas sem multiplicar consultas por pergunta. | - | pista de testes após fila 1 | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1230 |
| O125 | ESPINHA-E | Dá destino aos colaboradores de cadastro ou batida que faltam na lista do administrador. | - | mesma fonte Cadastro x Realidade; emissores existentes | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1232 |
| O117 | ESPINHA-E | Resolve a divergência de regra sobre o previsto de células sem marcos. | - | corte se ainda desejado | + + . . . . . . . | PARADA (zero casos na frota; corte pendente) | app/docs/BACKLOG.md:1234 |
| O108 | ESPINHA-E | Faz o espelho mostrar os furos e abonos usando a alimentação completa do cálculo. | - | autorização para aplicar | + + + . . . . . . | PARADA (DIFF completo; espera autorização) | app/docs/BACKLOG.md:1243 |
| O105 | ESPINHA-E | Faz o arquivo da folha refletir de forma estável o valor gravado. | - | decidir substituição ou autópsia de datas_falta | + + . . . . . . . | PARADA (PAREI por divergência no TXT) | app/docs/BACKLOG.md:1246 |
| O103 | LATERAL-ESTRUTURA | Permite regenerar somente o dia autorizado, preservando os demais. | - | O105 para o TXT residual | + + + . . . . + . | REGISTRADA (parte do aval fechada; TXT residual pendente) | app/docs/BACKLOG.md:1248 |
| O101 | LATERAL-ESTRUTURA | Exige motivo e registra antes e depois ao mudar a isenção de ponto. | - | pista para executar o selo | + + + . . . . + + | NO AR hoje (selo ainda espera pista na fonte) | app/docs/BACKLOG.md:1250 |
| O99 | LATERAL-ESTRUTURA | Aplica correções provadas preservando a história do arquivo da folha. | - | decisão sobre 61 dias; TXT | + + + . . . . + + | PARADA (cura 2 no ar; cura 1 e TXT pendentes) | app/docs/BACKLOG.md:1252 |
| O8 | UI | Mostra no PDF a identificação das ausências parciais e ainda não decididas. | - | tratamento da ausência rejeitada | + + + . . . . . . | REGISTRADA (parcial e pendente fechados; rejeitada registrada) | app/docs/BACKLOG.md:1261 |
| O9 | ESPINHA-E | Faz PDF, tela e aplicativo mostrarem os mesmos dados do espelho. | - | pista para DIFF de frota; passos restantes | + + + . . . . . . | REGISTRADA (passo 1 fechado; restantes pendentes) | app/docs/BACKLOG.md:1262 |
| O10 | LATERAL-ESTRUTURA | Registra a troca de plantão com os dois dias, documentos e confirmação do administrador. | - | credencial; confirmação administrativa com dois anexos | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1263 |
| HIGIENE-DE-CONTEXTO | INSTRUMENTO | Preserva um resumo verificável da sessão e reduz a carga de documentação. | - | autorização do item 5 | + + + . . . . + . | PARADA (itens 1–4 e 6 fechados; item 5 pendente) | app/docs/BACKLOG.md:1267 |
| GESTAO-HE-CALENDARIO-CONTROLE | UI | Permite selecionar no calendário os dias de hora extra que precisam de decisão. | - | fatia 2 após troca S5b | + + + . . . . + + | REGISTRADA (fatia 1 no ar; fatia 2 pendente) | app/docs/BACKLOG.md:1268 |
| GESTAO-HE-FATIA-2-LOTE-E-LIMITE | UI | Permite decidir horas extras de vários dias com limite e controles na tela. | - | troca S5b; conciliar limite já entregue pela O214 | + + + . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1269 |
| HE-OCORRENCIA-COMO-LINHA | LATERAL-ESTRUTURA | Guarda cada ocorrência de hora extra e sua história numa linha consultável. | - | autorização da troca S5b | . + . . . . . . . | REGISTRADA (desenho publicado; nenhuma migration) | app/docs/BACKLOG.md:1271 |
| PREVISTO-EM-DUAS-[nome] | ESPINHA-E | Faz o previsto exibido do dia concordar com a autoridade do previsto. | - | censo de escritores de minutos_previstos | + + . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1276 |
| LOTE-1-PENDENTES | LATERAL-ESTRUTURA | Entrega os censos, diferenças e pautas dos cinco grupos de pendências. | - | autorização da troca; decisões dos itens 2 e 5; cadastro do item 3 | + + . . . . . . . | PARADA (item 1 feito; restante espera autorização) | app/docs/BACKLOG.md:1279 |
| FOLGA-TRABALHADA-TRES-NUMEROS | ? (mistura divergência do gravado, motor e calculador) | Explica e corrige a diferença entre os três totais de folga trabalhada. | - | causa da diferença; autorização financeira | + . + . . . . . . | PARADA (medido; espera autorização) | app/docs/BACKLOG.md:1282 |
| LOTE-ATRASO-ADMITIDO | LATERAL-ESTRUTURA | Permite validar em lote os atrasos admitidos pelo colaborador. | - | S5b; UI-RESPOSTA-DIZ-O-QUE-E com smoke; autorização | . . . . . . . . . | REGISTRADA | app/docs/BACKLOG.md:1284 |
| FECHAMENTO-ZERO-COM-BATIDA | LATERAL-ESTRUTURA | Encaminha a correção dos cadastros que deixam a folha zerada apesar das batidas. | - | correção autorizada de vínculo | + + . . . . . . . | PARADA (cadastro de vínculo espera autorização) | app/docs/BACKLOG.md:1285 |
| S5b-TROCA-NAO-CRIA-DESCONTO | CEREJA | Troca o calculador sem criar descontos que o motor atual não cobra. | - | explicar pareamento e dupla contagem; refazer DIFF; autorização | + + + . . . . . . | PARADA (PAREI no critério por colaborador) | app/docs/BACKLOG.md:1287 |
| S5b-4-REGRAS | CEREJA | Completa quatro regras do calculador usando as funções já existentes do motor. | - | autorização da troca; item 5 | + + + + . . . . . | PARADA (cinco de seis observáveis feitos; espera autorização) | app/docs/BACKLOG.md:1288 |
| O14 | ESPINHA-E | Faz o aplicativo atribuir as batidas ao mesmo dia de turno do espelho. | - | smoke no celular | + + + . . . . + + | NO AR hoje (smoke pendente) | app/docs/BACKLOG.md:1293 |
| O15 | LATERAL-ESTRUTURA | Entrega ao DP os dias em aberto e as ações necessárias por classe. | - | - | + . + + . . . . + | REGISTRADA (fechada; data do fecho ?) | app/docs/BACKLOG.md:1294 |
| O16 | INSTRUMENTO | Dá causa e destino aos pacotes que terminavam sem explicação. | - | - | + . + . . . . . . | REGISTRADA (fechada; data do fecho ?) | app/docs/BACKLOG.md:1295 |
| O17 | INSTRUMENTO | Mostra há quanto tempo a trava de fabricação está vazia e sua causa. | - | - | + . + . . . . + . | REGISTRADA (fechada; data do fecho ?) | app/docs/BACKLOG.md:1296 |
| O18 | INSTRUMENTO | Separa os pacotes parados por causa e encaminha os resíduos. | - | quatro resíduos presos por arquivo de root | + . + . . . . . . | REGISTRADA (fechada sem data; quatro resíduos permanecem) | app/docs/BACKLOG.md:1297 |
| O19 | LATERAL-ESTRUTURA | Explica as falhas e permite enviar o documento de ausência com foto pelo aplicativo. | - | autorização e retomada do lado APP | + . + . . . . . + | PARADA (servidor no ar; APP espera autorização) | app/docs/BACKLOG.md:1298 |

Contagem: **111 obras examinadas; 57 incluídas; 54 excluídas**. Cabeçalho e separador das linhas 1190–1191 não são obras. Fontes: app/docs/BACKLOG.md:1190–1302.

Excluídas, por terminal anterior a 13/11 sem parte pendente posterior própria conservada na célula: O224:1200, O239:1206, O222:1213, O220:1216, O211:1217, O126:1221, O130:1223, O119:1226, O120:1227, O124:1231, O118:1233, O116:1235, O115:1236, O114:1237, O113:1238, O112:1239, O111:1240, O110:1241, O109:1242, O107:1244, O106:1245, O104:1247, O102:1249, O100:1251, O97:1253, O1:1254, O2:1255, O3:1256, O4:1257, O5:1258, O6:1259, O7:1260, O11:1264, O12:1265, PONTUALIDADE-SO-NO-12X36:1266, ATALHO-HE-NA-CENTRAL:1270, L-084-VENCE-O-T8:1272, DOIS-JUIZES-DA-ENTRADA-LONGE:1273, PONTUALIDADE-EM-TURNO-ABERTO:1274, AVAIS-NA-MESA:1275, 09-TURNO-ABERTO-EXPOSTA:1277, COL369-VINCULO-1296:1278, SUITE-UMA-VEZ-POR-ARVORE:1280, L-093-TRABALHADO-REAL:1281, UI-RESPOSTA-DIZ-O-QUE-E:1283, FILA-DA-NOITE:1286, COL369-FOLGA-NAO-DECLARADA:1289, COL369-TIPOS-INVERTIDOS:1290, JANELA-DA-AUTORIDADE-PERDE-O-DIA-20:1291, O13:1292, O20:1299, O21:1300, O22:1301, O23:1302. Todos os números são linhas de app/docs/BACKLOG.md.

Duplicatas no próprio subtrecho: nenhuma. Fonte: app/docs/BACKLOG.md:1192–1302.

Divergências:

- O221 — a célula de portão diz três pousos no ar; a célula de localização diz “EM FILA, nada juntado”. app/docs/BACKLOG.md:1215.
- O223 — portão diz dois pousos no ar e encerramento da L-006; localização ainda diz “nada construído”. app/docs/BACKLOG.md:1198.
- O218 — BACKLOG afirma que a bateria e cenários continuam não escritos; placar raspado declara bateria turno/marcos de 16 cenários e 64 testes. A equivalência integral de escopo entre os dois registros é [nao verificado]. app/docs/BACKLOG.md:1214; anexo:ESTADO.md, PLACAR-ESTRUTURAL, linha BATERIA [linha não numerada].
- GESTAO-HE-FATIA-2-LOTE-E-LIMITE — descrição pede default 10 e efeito só de tela; localização registra entrega pela O214 com default 15 e efeito real. app/docs/BACKLOG.md:1269.

## 13/11 20:30 · sessao 824c5a9cccdf · - · - · origem=Ronald (tmux)

| GRADE-V5-SEM-PORTAO | ESPINHA-E | Corrige a folga de vínculo encerrado preservando a folha já entregue. | - | ? (não declarada) | .++...... | REGISTRADA (construída; sem curso em 13/11) | app/docs/BACKLOG.md:844 |
| GRADE-FOLGA-JUIZ | ESPINHA-E | Faz os leitores usarem a mesma decisão sobre folga lançada. | - | decisão GRADE-TETO | .++...~.. | REGISTRADA (construção antiga; opus reprovado) | app/docs/BACKLOG.md:845 |
| FASE-CONFLITANTE-VINCULO-DO-DIA | INSTRUMENTO | Faz o vigia conferir o vínculo correto de cada dia. | - | GRADE-FOLGA-JUIZ | ++....... | REGISTRADA | app/docs/BACKLOG.md:846 |
| E4-7-D4-ABERTO-SEM-TURNO | ESPINHA-E | Investiga dias que aparecem abertos sem existir turno aberto. | - | ? (não declarada) | .+....... | REGISTRADA | app/docs/BACKLOG.md:848 |
| B3-RAIZ-DO-WRAPPER | INSTRUMENTO | Impede uma cópia de consultar o estado de contenção no lugar errado. | - | ? (não declarada) | ......... | REGISTRADA | app/docs/BACKLOG.md:849 |
| B3-CHAVE-DA-OBRA-NOS-DOIS-CONTADORES | INSTRUMENTO | Faz os dois contadores reconhecerem a mesma identificação de obra. | - | ? (não declarada) | ......... | REGISTRADA | app/docs/BACKLOG.md:850 |
| B3-NAO-SEI-SEM-SAIDA | INSTRUMENTO | Dá tratamento explícito ao estado de contenção que não pôde ser lido. | - | ? (não declarada) | ......... | REGISTRADA | app/docs/BACKLOG.md:851 |
| E4-8-SAIDA-I | ESPINHA-E | Mostra na tela a mesma ausência de cobrança registrada pela célula. | - | decisão de manter ou reverter; E4-D5-PROD | .+++++++~ | PARADA (parte no ar; manter ou reverter aguarda decisão) | app/docs/BACKLOG.md:852 |
| B3-CODEX-PREEXISTENTES | INSTRUMENTO | Corrige brechas dos controles e a publicação de zero sem leitura. | - | ? (não declarada) | ......... | REGISTRADA | app/docs/BACKLOG.md:853 |
| CODEX-CUSTO-POR-OBRA | INSTRUMENTO | Publica o custo e o resultado das chamadas por obra. | - | CLAUDE-MD-ENXUTO; vaga de instrumento | .++...... | REGISTRADA (contrato escrito; sem brief admitido) | app/docs/BACKLOG.md:854 |
| E4-CAL-DIA-COM-FIM-LE-A-FOLHA | ESPINHA-E | Faz o calendário mostrar a mesma pendência que a folha. | - | E4-CALENDARIO-L1 | .+....... | REGISTRADA (complemento da L1) | app/docs/BACKLOG.md:855 |
| E4-CALENDARIO-L1 | ESPINHA-E | Faz o calendário ler o estado oficial do dia sem recalculá-lo. | casos 01–10 | ISENTO-H10; complementos da L1 | .+++~.... | PARADA (interrupção antiga fora do anexo; novo complemento registrado) | app/docs/BACKLOG.md:856; app/docs/RELATO.md:37 |
| PARCIAL-NA-PORTA | LATERAL-ESTRUTURA | Impede cobranças indevidas de ausência parcial já no nascimento da pergunta. | - | E4-CALENDARIO-L1 | .+....... | REGISTRADA | app/docs/BACKLOG.md:857 |
| COBPARC-ROTULO-FILA-V2 | LATERAL-ESTRUTURA | Corrige rótulos e contagens de marcos dispensados por ausência ou justificativa. | - | ? (não declarada) | ......... | REGISTRADA | app/docs/BACKLOG.md:859 |
| COBPARC-PORTAO-PARCIAL-JUIZ-UNICO | ESPINHA-E | Faz os leitores usarem uma única regra para cobrar ausências parciais. | - | ? (não declarada) | .+....... | REGISTRADA | app/docs/BACKLOG.md:860 |
| COBPARC-FILA-RELOGIO | LATERAL-ESTRUTURA | Faz cada ato usar um único instante ao decidir se houve fato novo. | - | resposta do padrão; mutantes; suíte | .++...... | INTERROMPIDA | app/docs/BACKLOG.md:861; anexo:interrompidas.txt:7 |
| COBPARC-FILA-MATERIALIZACAO | ESPINHA-E | Faz a cobertura da ausência ser lida da decisão registrada na célula. | - | COBERTURA-PARCIAL-UM-JUIZ; possível PARCIAL-NA-PORTA | ......... | REGISTRADA | app/docs/BACKLOG.md:862 |
| COBPARC-FILA-CENSO-V7 | LATERAL-ESTRUTURA | Corrige leitores restantes que interpretam ausências parciais por caminhos diferentes. | - | COBERTURA-PARCIAL-UM-JUIZ; autorização para dinheiro | ......... | REGISTRADA | app/docs/BACKLOG.md:863 |
| COBPARC-FILA-V8-PARCIAL-DEMISSAO | LATERAL-ESTRUTURA | Confere a ausência parcial no dia da demissão sem batida. | - | primeira rodada do cartório após COBPARC | ......... | REGISTRADA | app/docs/BACKLOG.md:864 |
| COBPARC-FILA-V8-VOCABULARIO | LATERAL-ESTRUTURA | Evita confundir ausência chamada justificativa com justificativa aprovada. | - | COBERTURA-PARCIAL-UM-JUIZ | ......... | REGISTRADA | app/docs/BACKLOG.md:865 |
| COBPARC-FILA-V8-PARCIAL-JUSTIF | LATERAL-ESTRUTURA | Corrige a cobrança restante quando ausência parcial e justificativa coincidem. | - | COBPARC-FILA-RELOGIO; vaga | ......... | PARADA (construtor estacionado; cópia vazia) | app/docs/BACKLOG.md:866 |
| COBPARC-FILA-H10-PARCIAIS-09 | LATERAL-ESTRUTURA | Trata ausências parciais antigas sem alterar silenciosamente competência entregue. | - | fato novo ou porta REGEN | .+....... | REGISTRADA | app/docs/BACKLOG.md:867 |
| COBPARC-FILA-H10-COL926 | LATERAL-ESTRUTURA | Encaminha ao DP o caso de batida separado do pouso da ausência parcial. | - | ? (não declarada) | .+....... | REGISTRADA | app/docs/BACKLOG.md:868 |
| E4-CALENDARIO-QUEM-PUBLICA-EH-TRABALHO | ESPINHA-E | Faz o calendário perguntar à decisão oficial se o dia é de trabalho. | - | E4-CALENDARIO-L1, passo 3 | .++...... | REGISTRADA (obra antiga; sem curso em 13/11) | app/docs/BACKLOG.md:869 |
| D5-VIA-NULA | ESPINHA-E | Impede completar o estado do dia por uma via sem identificação. | - | ? (não declarada) | +........ | REGISTRADA | app/docs/BACKLOG.md:870 |
| E4CAL-P1-REVERTER-AMPLO | INSTRUMENTO | Permite desfazer todos os campos alterados pela porta de preenchimento do estado. | - | D5-VIA-NULA | ......... | REGISTRADA | app/docs/BACKLOG.md:871 |
| E4CAL-P1-PAREI-FM-CORRENTE | INSTRUMENTO | Evita falso alarme ao comparar a folha corrente que continua mudando. | - | ? (não declarada) | ......... | REGISTRADA | app/docs/BACKLOG.md:872 |
| E4CAL-P1-SELO-CAMPOS-ACORDAM | INSTRUMENTO | Faz o teste detectar eventos ou auditoria alterados fora do esperado. | - | ? (não declarada) | ......... | REGISTRADA | app/docs/BACKLOG.md:873 |
| E4CAL-P1-ROTULO-SEM-CERTIDAO | ESPINHA-E | Investiga células com estado preenchido e rótulo antigo de falta de certidão. | - | medir dono L-099 | ......... | REGISTRADA | app/docs/BACKLOG.md:874 |
| E4CAL-P3-ULTIMO-RECURSO-ENGOLE | ESPINHA-E | Impede o calendário de esconder falhas na decisão sobre o dia. | - | ? (não declarada) | ......... | REGISTRADA | app/docs/BACKLOG.md:875 |
| E4CAL-P3-CENSO-HOJE-PORTA-CURTA | ESPINHA-E | Faz a porta curta receber o mesmo relógio usado pelo censo. | - | ? (não declarada) | ......... | REGISTRADA | app/docs/BACKLOG.md:876 |
| E4CAL-P3-LEITURA-ESCREVE | ESPINHA-E | Evita que a simples consulta do calendário tente materializar perguntas. | - | ? (não declarada) | ......... | REGISTRADA | app/docs/BACKLOG.md:877 |
| E4CAL-P3-DOCS-VELHAS | INSTRUMENTO | Atualiza comentários e registros que ainda descrevem a decisão antiga do calendário. | - | pouso da L1 | ......... | REGISTRADA (complemento não incluído no pouso) | app/docs/BACKLOG.md:879; app/docs/RELATO.md:1775 |
| PAREADOR-JANELA-ORFA | ESPINHA-E | Faz o mesmo dia receber a mesma decisão em consulta mensal ou isolada. | - | ? (não declarada) | ...~..... | PARADA (RED antigo estacionado) | app/docs/BACKLOG.md:880 |
| E4CAL-CARD-LE-NUCLEO | ESPINHA-E | Faz os cartões do calendário usarem os rótulos oficiais do núcleo. | - | retomada da L1 | ......... | REGISTRADA | app/docs/BACKLOG.md:881 |
| STATUS-ESCALA-SUSPEITA-VOCAB | ESPINHA-E | Corrige o rótulo que diz ausência de batida quando existe batida. | - | aval para mudar a lavra | ......... | PARADA (depende de aval) | app/docs/BACKLOG.md:882 |
| A2-RAISE-NO-FUROS | ESPINHA-E | Evita erro de tela quando a regra A2 recusa um estado do dia. | - | ? (não declarada) | ......... | REGISTRADA | app/docs/BACKLOG.md:883 |
| TT6-PENDENTE-VELHO | INSTRUMENTO | Reconcilia a descrição da pendência TT6 com a mudança do calendário. | - | conferir L-118 após L1 | ......... | REGISTRADA | app/docs/BACKLOG.md:884 |
| E4-APP-MAPA | ESPINHA-E | Faz o aplicativo mostrar o estado oficial do dia por um único adaptador. | - | leitores 1–3 da E4 | .+++++.++ | NO AR hoje (pouso de 04/11 relatado; BACKLOG antigo) | app/docs/BACKLOG.md:885; app/docs/RELATO.md:1720; app/docs/RELATO.md:1718 |
| CONTENCAO-QUE-MORDE-V2 | INSTRUMENTO | Aplica controles de voltas, agentes, revisão e certificação antes dos pousos. | - | contrato 20; contrato novo | .+++..~.. | PARADA (estacionada por aval) | app/docs/BACKLOG.md:889 |
| CONTENCAO-V2-HORA-DE-LINHA | INSTRUMENTO | Impede publicar horários futuros também em linhas de lista. | - | CONTENCAO-QUE-MORDE-V2 | .+....... | REGISTRADA | app/docs/BACKLOG.md:890 |
| V3-DOBRA-SEM-NOTURNA | CEREJA | Corrige a dobra do calculador mantendo o motor e a folha exportada estáveis. | - | localizar residual LMT | ..++..++~ | REGISTRADA (parte no ar; residual LMT pendente) | app/docs/BACKLOG.md:891 |
| V3-LAVRAR-ATRASO-CREDITO | CEREJA | Preserva o desconto do crédito ao gravar o atraso calculado. | - | medir frota | ......... | REGISTRADA | app/docs/BACKLOG.md:892 |
| O261-E-CELULA-20-UM-DONO | ESPINHA-E | Garante que perguntas vivas tenham um caminho disponível para alguém agir. | - | decisões da F3; etapas F4–F5 | ++++++++~ | PARADA (F1–F2 no ar; F3 aguarda desenho) | app/docs/BACKLOG.md:893 |
| O261-E-PAI-TERMINAL-RESIDUAL | ESPINHA-E | Trata o que falta da ligação entre pergunta e chamado vivo por dia. | - | corte da tabela de triagem | +........ | PARADA (não entra na ordem antes do corte) | app/docs/BACKLOG.md:894 |
| O261-E-F5B-JUIZ-PORTA-ALCANCAVEL | ESPINHA-E | Faz os leitores concordarem sobre o caminho para agir no dia. | - | antes da F5 por código; posição diverge na célula | .+....... | REGISTRADA | app/docs/BACKLOG.md:895 |
| O261-E-F5C-PERGUNTA-SEM-ACAO-POSSIVEL | ESPINHA-E | Nomeia a decisão que identifica uma pergunta cujo clique já não produz efeito. | - | registro do juiz com corte | .+....... | REGISTRADA | app/docs/BACKLOG.md:896 |
| O261-E-F5C-FILA-DO-OPUS | ESPINHA-E | Corrige divergências restantes entre fila, auditoria, responsáveis e caminhos de ação. | - | cortes F5C-1 e F5C-2 | ......... | REGISTRADA | app/docs/BACKLOG.md:897 |
| E1-PERGUNTA-AO-DP-CONTADOR | ESPINHA-E | Obtém as respostas pendentes sobre horas extras, intervalo e convenção. | - | resposta do DP/contador | .+....... | PARADA (três respostas pendentes) | app/docs/BACKLOG.md:898 |
| E1-STATUS-VERMELHOS | ESPINHA-E | Corrige pela E3 e E4 os leitores reprovados na certificação do estado. | - | E3/E4; um leitor por pouso | .+....... | REGISTRADA | app/docs/BACKLOG.md:899 |
| V-FASE-AS-OF-TRANSFERIDO | LATERAL-ESTRUTURA | Usa a empresa correta do dia ao conferir fase de colaborador transferido. | - | medir transferidos | ......... | REGISTRADA | app/docs/BACKLOG.md:900 |
| V-FASE-AS-OF-PARIDADE-UNIVERSO | LATERAL-ESTRUTURA | Faz a previsão comparada usar o mesmo conjunto de dias e folgas. | - | ? (não declarada) | ......... | REGISTRADA | app/docs/BACKLOG.md:901 |
| V-FASE-AS-OF-CUSTO | LATERAL-ESTRUTURA | Mede o custo de procurar decisões anteriores quando falta cobertura do dia. | - | medição na sombra | ......... | REGISTRADA | app/docs/BACKLOG.md:902 |
| V-FASE-AS-OF-CADASTRO-SEM-LINHA | ESPINHA-E | Leva ao admin os problemas de cadastro encontrados em dias passados. | - | ? (não declarada) | ......... | REGISTRADA | app/docs/BACKLOG.md:903 |
| DEGRAU-HE-100-CADASTRO | LATERAL-ESTRUTURA | Torna configurável pela convenção o limite usado na hora extra de cem por cento. | - | cadastro e leitor na UI | .+....... | REGISTRADA | app/docs/BACKLOG.md:904 |
| V-FASE-AS-OF-SEM-CELULA | LATERAL-ESTRUTURA | Reabre a conferência quando muda um dia passado que ficou sem célula. | - | lei SEM-CELULA | .+....... | REGISTRADA | app/docs/BACKLOG.md:905 |
| O261-E-F1C4-LEN-VIVOS | ESPINHA-E | Faz o suprajuiz ler a cobrança já decidida na célula. | - | LENVIVOS-A6-ALARME residual | .+++++++~ | REGISTRADA (item c pousado; A6 pendente) | app/docs/BACKLOG.md:906 |
| LENVIVOS-A6-ALARME | ESPINHA-E | Faz o alarme ler a cobrança lavrada em vez de recalculá-la. | - | ? (não declarada) | ......... | REGISTRADA | app/docs/BACKLOG.md:907 |
| LENVIVOS-SOBRAS-ESMERIL | LATERAL-ESTRUTURA | Trata sobras de teste, alarme e cobertura do julgamento de cobrança. | - | ESMERIL-V2 | .+....... | REGISTRADA | app/docs/BACKLOG.md:908 |
| LENVIVOS-EMITIR-INERTE | LATERAL-ESTRUTURA | Remove ou corrige um parâmetro que não interfere na emissão de cobrança. | - | ? (não declarada) | +........ | REGISTRADA | app/docs/BACKLOG.md:909 |
| LENVIVOS-CHAVE-DO-DIA | LATERAL-ESTRUTURA | Faz emissor e cartório associarem a cobrança ao mesmo dia. | - | ? (não declarada) | +........ | REGISTRADA | app/docs/BACKLOG.md:910 |
| LENVIVOS-COBRANCAS-VIVAS | LATERAL-ESTRUTURA | Confere se a cobrança viva recebe o mesmo filtro aplicado aos chamados vivos. | - | conferir antes da cura | +........ | REGISTRADA (candidata ainda em conferência) | app/docs/BACKLOG.md:911 |
| O261-E-F1C4-MEMO-C6 | ESPINHA-E | Evita reutilizar uma resposta antiga após mudar a célula no mesmo ato. | - | próxima obra que tocar C6 | +........ | REGISTRADA (falha latente; caminho real não demonstrado) | app/docs/BACKLOG.md:912 |
| MEMO-C6-BATIDAS-PREFETCH | ESPINHA-E | Confere se uma mudança de batidas deixa a decisão em memória desatualizada. | - | medir composição com transação externa | ......... | REGISTRADA (hipótese não verificada) | app/docs/BACKLOG.md:913 |
| V-FASE-AS-OF-ATRASO | LATERAL-ESTRUTURA | Reduz o atraso entre a mudança do dia e a nova conferência da fase. | - | próxima lavra e dependências do cron | ......... | REGISTRADA | app/docs/BACKLOG.md:916 |
| V-FASE-AS-OF-UNIVERSO | LATERAL-ESTRUTURA | Reconcilia decisões antigas quando o colaborador deixa a escala de doze por trinta e seis. | - | ? (não declarada) | ......... | REGISTRADA | app/docs/BACKLOG.md:917 |
| V-FASE-AS-OF-SOBREPOSTA | LATERAL-ESTRUTURA | Trata janelas sobrepostas criadas por mudança do início da competência. | - | ? (não declarada) | ......... | REGISTRADA | app/docs/BACKLOG.md:918 |
| V-FASE-AS-OF-INTRADIA | LATERAL-ESTRUTURA | Preserva versões anteriores de decisões feitas no mesmo dia. | - | ? (não declarada) | ......... | REGISTRADA | app/docs/BACKLOG.md:919 |
| PROPOSITOR-JANELA-LONGA | LATERAL-ESTRUTURA | Mantém a proposta de escala coerente quando a consulta ultrapassa duas janelas. | - | ? (não declarada) | ......... | REGISTRADA | app/docs/BACKLOG.md:920 |
| FILA-POR-CAUSA-AS-OF | LATERAL-ESTRUTURA | Faz a fila de chamados passados ler a decisão correspondente àquela data. | - | pouso F1/F5 | ......... | REGISTRADA | app/docs/BACKLOG.md:921 |
| F1C4-3-NONE-LEITORES | ESPINHA-E | Faz os leitores restantes tratarem trabalho indefinido pela mesma regra central. | - | censo; um leitor por pouso | ......... | REGISTRADA | app/docs/BACKLOG.md:923 |
| CARTORIO-PROTESTA-1A-CLAUSULA | ESPINHA-E | Define com números como o cartório deve aplicar a regra de dia sem trabalho. | - | corte sobre o novo alcance do predicado | ......... | PARADA (pergunta necessária antes da migração) | app/docs/BACKLOG.md:925 |
| F1C4-3-RG-SEM-CELULA-CASO | INSTRUMENTO | Acrescenta o caso de teste de dia sem âncora e sem célula. | - | ESMERIL-V2 | ......... | REGISTRADA (volta antiga; sem curso em 13/11) | app/docs/BACKLOG.md:926 |
| E3-GRADE-SUBTIPO-FOLGA | ESPINHA-E | Preserva na grade o subtipo de folga necessário ao estado oficial. | - | cura da grade fora da E3 | ......... | REGISTRADA | app/docs/BACKLOG.md:927 |
| E3-GRADE-POR-TURNO-MEDIR | INSTRUMENTO | Mede se a opção alternativa de grade ainda é usada antes de removê-la. | - | medição do valor em produção | ......... | REGISTRADA | app/docs/BACKLOG.md:928 |
| E3-HASH-A2 | ESPINHA-E | Faz mudanças de feriado e folga invalidarem a decisão antiga do dia. | - | E2 | ......... | REGISTRADA | app/docs/BACKLOG.md:929 |
| EMIS-LEITORES-RESTANTES | ESPINHA-E | Migra os últimos leitores para a regra única de dia sem trabalho. | - | COBERTURA-PARCIAL-UM-JUIZ | .+.+..... | PARADA (aguarda precedência registrada) | app/docs/BACKLOG.md:930 |
| EMIS-PAUTA-DEDUP | LATERAL-ESTRUTURA | Evita duplicar a pauta do DP em julgamentos concorrentes. | - | nova medição na sombra | ......... | REGISTRADA | app/docs/BACKLOG.md:931 |
| EMIS-CATRACA-FLUXO | INSTRUMENTO | Faz o selo conferir o percurso completo do dado de dia sem trabalho. | - | ? (não declarada) | ......... | REGISTRADA | app/docs/BACKLOG.md:932 |
| BUG-ENC-CELULAS-FORMA | ESPINHA-E | Corrige o formato das células repassadas ao julgamento de turnos. | - | F2; E4-CARGA-PARIDADE | ++....... | INTERROMPIDA | app/docs/BACKLOG.md:933; anexo:interrompidas.txt:9 |
| E4-CARGA-PARIDADE | INSTRUMENTO | Prova que consulta em lote e consulta isolada carregam os mesmos dados. | - | F2; antes de BUG-ENC-CELULAS-FORMA | +........ | REGISTRADA | app/docs/BACKLOG.md:934 |
| RELATO-ESCRITOR-FORA-DO-FLOCK | INSTRUMENTO | Faz todos os escritores do relato respeitarem a trava central. | - | ? (não declarada) | ......... | REGISTRADA | app/docs/BACKLOG.md:935 |
| PDC-RELOGIO-NA-CLI | INSTRUMENTO | Faz a linha de comando receber o relógio na entrada da decisão. | - | ? (não declarada) | ......... | REGISTRADA | app/docs/BACKLOG.md:936 |
| PDC-PAINEL-SEM-DUVIDA | INSTRUMENTO | Mostra quando o painel não conseguiu determinar a quantidade de disparos. | - | ? (não declarada) | ......... | REGISTRADA | app/docs/BACKLOG.md:937 |
| E4-S4-ENCERRADOS | INSTRUMENTO | Mede o limite de consultas do painel com turnos já encerrados. | - | fixture com turno encerrado lavrado | ......... | REGISTRADA | app/docs/BACKLOG.md:938 |
| E4-NAO-LAVRADO-PASSADO | ESPINHA-E | Corrige a apresentação de dias passados sem decisão registrada. | - | migração dos leitores E4 | ......... | REGISTRADA | app/docs/BACKLOG.md:939 |
| GRADE-09-PAUTA | LATERAL-ESTRUTURA | Entrega ao DP a diferença entre o pago e o devido sem regravar a folha entregue. | - | GRADE-v5; ensaio e diferença medida | ......... | REGISTRADA | app/docs/BACKLOG.md:940 |
| E3-STATUS-EXPORTADA | ESPINHA-E | Completa o estado dos dias antigos sem modificar valores pagos ou arquivos entregues. | - | pouso E3 | .+....... | REGISTRADA | app/docs/BACKLOG.md:942 |
| ISENTO-H10-MENORES | LATERAL-ESTRUTURA | Trata leitores, testes e decisões antigas que restaram da regra de isenção. | - | ? (não declarada) | ......... | REGISTRADA | app/docs/BACKLOG.md:945 |
| BACKFILL-UNICO | ESPINHA-E | Centraliza o tratamento de correções antigas com responsável e destino explícitos. | - | antes do último horizonte; corte de exportada | .+....... | REGISTRADA | app/docs/BACKLOG.md:947 |
| CODS-ORFAO14H-SEM-GATILHO | INSTRUMENTO | Confere a emissão de perguntas atrasadas e corrige o nome de um teste de controle. | - | renomear o teste residual | +........ | REGISTRADA (emissão conferida; renomeação pendente) | app/docs/BACKLOG.md:948 |
| ENCADEADO-0111-SEM-CARIMBO | INSTRUMENTO | Identifica por que etapas da corrente matinal ficaram sem prova de execução. | - | NOREVERSE-UI-NO-CORE; investigar outros carimbos | +........ | REGISTRADA (uma causa identificada; outros dias sem explicação) | app/docs/BACKLOG.md:949 |
| DEPLOY-RELOAD-RESIDUOS | INSTRUMENTO | Corrige falhas restantes na conferência e propagação do resultado de deploy. | - | contrato de instrumento; pouso próprio | ......... | REGISTRADA | app/docs/BACKLOG.md:951 |
| TELA-PREFIXO-SEM-GUARDA | INSTRUMENTO | Detecta mudanças de endereço que quebrariam links entre o núcleo e as telas. | - | ? (não declarada) | ......... | REGISTRADA | app/docs/BACKLOG.md:952 |
| NOREVERSE-UI-NO-CORE | LATERAL-ESTRUTURA | Impede que a geração de links de tela derrube o processamento do núcleo. | - | resultado 6 após primeira corrente | ++++++++~ | REGISTRADA (cura no ar; resultado posterior pendente) | app/docs/BACKLOG.md:953; app/docs/RELATO.md:1810 |
| E3-ISENTO-INSUMOS | ESPINHA-E | Faz a decisão do dia considerar a isenção de ponto na impressão dos dados. | - | ISENTO-H10 substituta | .+++..~.. | PARADA (SUPERADA por ISENTO-H10; não excluída pela lista literal) | app/docs/BACKLOG.md:954 |
| ISENTO-PONTO-JUIZ-REGISTRADO | ESPIN

[truncado em 20000 de 22468 caracteres]

_4822 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
