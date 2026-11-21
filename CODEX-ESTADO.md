# CODEX-ESTADO

> Ultima resposta processada: **21/11/2026 05:11** · publicado em **21/11/2026 05:15** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 21/11 05:11 · sessao 7e758928db7f · E4C-F5-CONSTROI · leitura · origem=Code

## 1. PRONTO na árvore final

| Item | Estado | Prova ou pendência |
|---|---|---|
| 01. Matriz e censo de entrada | FALTA | O RELATO registra 15 sítios, amostra 9/9 e zero perguntas de lei. Falta o vínculo nominal entre os 15 sítios do brief, os selos e o PRONTO. `app/docs/RELATO.md:94,1503` |
| 02. Testemunha da leitura | DEMONSTRADO | `leitura` devolve `LeituraDoAto` com 3 campos. `composta` devolve `LeituraComposta` com 5. `app/chamados/models.py:19–25,4348–4390` |
| 03. Conservação do passivo sem ato | DEMONSTRADO | A leitura intercala o passivo sintético com os atos. O teste fixa os dois juízes e o efeito do horizonte no passivo. `app/chamados/models.py:4302–4323,4361–4379`, `app/chamados/tests/test_e4c_f5_volta2_juizes.py:59–75` |
| 04. RED-5 do histórico | DEMONSTRADO | Teste e mutante distinguem instante de resposta, validação e materialização, com texto da projeção nos três eventos. `app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:75–87`, `app/chamados/services/historico.py:96–111` |
| 05. SILÊNCIO e dono ADMIN | RETIRADO-POR-AVAL | 1255 item 3, “o SILENCIO dono admin SAI da F5 e vira fatia propria E4C-F5-SILENCIO-ADMIN”. A regressão permanece. `avais_posteriores.txt:129–131`, `app/docs/BACKLOG.md:1967`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:343–344` |
| 06. Catraca de leitores crus | DEMONSTRADO | Recontagem AST de 13 funções em 8 arquivos, zero acusações. A regra acusa `update_fields` sem atribuição ao campo no mesmo receptor. O controle de escrita passa e três mutantes acusam nos dois modos. A lacuna editorial do mapa está no 01. `app/chamados/tests/test_e4c_f5_selo_catraca.py:17–28,214–228,304–349,402–440`, `execucoes.txt:14–15,24–27` |
| 07. Três passagens | HISTORICO | 1945 item 3 manda valer pelo log e hash de A/B/C. O instrumento da volta está na cópia. Os recibos das três passagens estão [nao verificado]. `avais_depois_do_vPRONTO5.txt:29`, `bin/volta_intermediaria.sh:4,32` |
| 08. Ordem, commits e RED antes da cura | HISTORICO | Valem a ordem de commits e o RED no hash de cada volta. Logs, ordem completa e pureza no primeiro commit da A [nao verificado]. `avais_depois_do_vPRONTO5.txt:29`, `app/chamados/tests/test_e4c_f5_selo_chamada.py:271–304` |
| 09. Verificador do delta por volta | HISTORICO | Valem os laudos nos hashes das voltas. Há registro da triagem c3. Laudos completos de A/B/C [nao verificado]. `avais_depois_do_vPRONTO5.txt:29`, `app/docs/RELATO.md:100` |
| 10. Mutantes tira-lote | SO POR EXECUCAO | 33 seletores, 9 A + 14 B + 10 C. A execução 5 está em curso. Resultado nominal, inclusive K16 e K47, EXECUCAO NAO LISTADA. `execucoes.txt:30`, `app/chamados/tests/e4c_f5_voltaA_mutantes.py:9–19`, `app/chamados/tests/e4c_f5_voltaB_mutantes.py:12–27`, `app/chamados/tests/e4c_f5_voltaC_mutantes.py:12–23` |
| 11. Sombra dos juízes e do passivo | SO POR EXECUCAO | Execução 4 refez `item11` em `9e90d`. O RELATO registra 51.953 julgadas de cada lado, 82 chaves comparadas e zero blocos diferentes. Série completa por volta [nao verificado]. `execucoes.txt:29`, `app/docs/RELATO.md:12,33` |
| 12. Sombra das decisões por porta | SO POR EXECUCAO | Execução 4 refez `item12`. O RELATO registra 141 tentativas, 152 decisões iguais e rollback conferido em 141. A medida é amostral. `execucoes.txt:29`, `app/docs/RELATO.md:12,29,45` |
| 13. Entregues e exportadas | SO POR EXECUCAO | Execução 4 cobre A/B entregue e 6-A1 MAIN × F5. A/B deu 0/0 em 25.766 entregues. O 6-A1 registra três perguntas com movimento de TXT reproduzido no MAIN. Uma lista geral de decisões alteradas nas exportadas está [nao verificado]. `execucoes.txt:29`, `app/docs/RELATO.md:22,27,66,78` |
| 14. CHECK-DE-TELA | ETAPA FUTURA | Comparativo de resultado, consultas e escritas no espelho, Central, tarefa pelo serviço e API. `avais_depois_do_vPRONTO5.txt:29`, `app/ponto/urls.py:10`, `app/chamados/views.py:721`, `app/chamados/services/tarefa_do_dia.py:825–832`, `app/api/urls.py:92` |
| 15. Verificador inteiro com zero bloqueios | HISTORICO | O 1945 preserva o laudo da volta. O teste do memo C6 está presente. Laudo integral com zero bloqueios [nao verificado]. `avais_depois_do_vPRONTO5.txt:29`, `app/chamados/tests/test_e4c_f5_voltaFINAL.py:12–13` |
| 16. Suíte inteira pré-Opus | SO POR EXECUCAO | Execução 1 em `b0935c1f1`, `rc=0`. Execução 2 em `9e90d`, `rc=1`, 17.274 testes e 16 erros. Placar nominal da execução 1, recibo da Pista 2 e precedência da publicação completa [nao verificado]. `execucoes.txt:2–3`, `app/docs/RELATO.md:2–3,10`, `bin/suite.sh:23–33,236–257` |
| 17. Opus final único | ETAPA FUTURA | Estado definido pelo 1945. A tentativa da execução 6 em `b0935c1f1` terminou REPROVADO. `avais_depois_do_vPRONTO5.txt:29`, `execucoes.txt:31`, `opus.r:1` |
| 18. Compositor nos três sítios | DEMONSTRADO | Um compositor tipado. Partição do app, property e fio delegam a ele, com instante do ato e texto da projeção. `app/chamados/models.py:3838–3844,4382–4390`, `app/chamados/services/pergunta_no_app.py:80–92`, `app/chamados/services/fio.py:187–190` |
| 19. Grep publicado | DEMONSTRADO | O RELATO enumera o compositor e as três delegações. As quatro âncoras batem na cópia. `app/docs/RELATO.md:94`, `app/chamados/models.py:3844,4382`, `app/chamados/services/pergunta_no_app.py:88`, `app/chamados/services/fio.py:189` |
| 20. RED da partição do app | SO POR EXECUCAO | `a1_app_coluna` e `a1_app_texto_do_ato` têm teste dirigido. Mordidas nominais no hash final, EXECUCAO NAO LISTADA. `app/chamados/tests/test_e4c_f5_voltaA_sitios.py:32–53` |
| 21. RED da property | HISTORICO | Valem log e hash da base. O teste fixa texto divergente e projeção carimbada após reabertura. RED histórico [nao verificado]. `avais_depois_do_vPRONTO5.txt:29`, `app/chamados/tests/test_e4c_f5_opus_models.py:37–54,87–89` |
| 22. RED do fio | SO POR EXECUCAO | Há três variantes dirigidas, coluna, texto do ato e composição local. Mordidas nominais no hash final, EXECUCAO NAO LISTADA. `app/chamados/tests/test_e4c_f5_voltaA_sitios.py:55–71` |
| 23. RED de `sem_veredito_entre` | HISTORICO | Valem log e hash da base. Há dois casos de divergência no teste. RED histórico [nao verificado]. `avais_depois_do_vPRONTO5.txt:29`, `app/chamados/tests/test_e4c_f5_opus_models.py:76–81` |
| 24. RED de `estado` | HISTORICO | Valem log e hash da base. O teste exige `PENDENTE` nos dois casos divergentes. RED histórico [nao verificado]. `avais_depois_do_vPRONTO5.txt:29`, `app/chamados/tests/test_e4c_f5_opus_models.py:83–85` |
| 25. Consumidores por consequência | DEMONSTRADO | Fechamento, CSF e Pauta chegam ao leitor comum. Identidade histórica dos arquivos [nao verificado]. `app/chamados/models.py:1998–2027,2035,2072`, `app/ponto/supra_juiz.py:573–574`, `app/chamados/services/competencia_trancada.py:124–126` |
| 26. Vigia de texto em lotes | DEMONSTRADO | Prefetch de decisões e `iterator(chunk_size=lote)`, com lote padrão de 2.000. `app/chamados/services/vigia_texto_resposta.py:8–25` |
| 27. Contagem preservada no vigia | SO POR EXECUCAO | O teste fixa `(0,2)`, `(1,2)`, `(1,1)` e `(1,3)`. Comparação antes × depois com a mesma fixture, EXECUCAO NAO LISTADA. `app/chamados/tests/test_e4c_f5_contador_texto.py:26–38` |
| 28. RED de consultas do vigia | SO POR EXECUCAO | O teste exige 3 SELECTs de decisões para 5 perguntas em lotes de 2. RED nominal contra a carga anterior, EXECUCAO NAO LISTADA. `app/chamados/tests/test_e4c_f5_contador_texto.py:87–95` |
| 29. Remoção da chave sem leitor | DEMONSTRADO | O carimbo conserva `divergencia_texto_resposta`. O teste exige a ausência de `divergencia_texto_resposta_universo`. `app/chamados/management/commands/vigiar_fio_morto.py:274–284`, `app/chamados/tests/test_e4c_f5_contador_texto.py:56–70` |
| 30. Decisão técnica publicada | DEMONSTRADO | Remoção, custo e reversão estão registrados. `app/docs/RELATO.md:1382` |
| 31. Selo AST por identidade | DEMONSTRADO | O selo segue parâmetro, aliases e alvo do laço. Dois testes conferem a renomeação e a detecção da pergunta inteira. `app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:164–185,220–251`, `app/chamados/tests/e4c_f5_volta2_mutantes.py:11–38` |
| 32. Triagem de `casefold` | FALTA | O teste de diferença só de caixa existe. O RELATO mantém os pareceres divergentes e registra o sobrevivente como plausível. Falta decisão motivada para o Opus. `app/chamados/tests/test_e4c_f5_contador_texto.py:50–53`, `app/docs/RELATO.md:94,1374,1377` |
| 33. PRONTO publicado | FALTA | A publicação de 113 itens identifica `c8eb8eddf`. A atualização de `9e90d` resume cinco itens, sombra e voltaE. Falta o quadro consolidado do hash integrado com as pendências. `app/docs/RELATO.md:12–13,92–94`, `execucoes.txt:1,30–31` |
| 34. Pureza AST dos dois juízes | DEMONSTRADO | Recontagem AST com zero impurezas e uma isenção nominal no juiz temporal. Identidade textual com a base [nao verificado]. `app/chamados/juizes.py:1383–1403,1551–1560`, `app/chamados/tests/test_e4c_f5_selo_chamada.py:163–185,271–304` |
| 35. Censo dos carregadores e mapa K | FALTA | As tabelas A/B/C identificam seletores e a cópia tem K13 e K40. Falta a conciliação dos 69 K com carregador, classe e ponto de toque. O RELATO explicita 35 K sem carregador nominal, além do vínculo K41/K42. `app/docs/RELATO.md:94`, `app/chamados/tests/e4c_f5_voltaA_mutantes.py:9–19`, `app/chamados/tests/e4c_f5_voltaB_mutantes.py:12–27`, `app/chamados/tests/e4c_f5_voltaC_mutantes.py:12–23`, `app/chamados/tests/test_e4c_f5_voltaC_lotes.py:239–264` |
| 36. Seis R declarados | FALTA | K40 tem percurso identificável. Faltam os vínculos nominais de K3, K19, K20, K22 e K58 e a declaração por sítio. `app/docs/RELATO.md:94`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:448–467` |
| 37. K40 por item | DEMONSTRADO | `_descarimbados` faz `get(pk=pk)` e julga cada candidata. `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:448–467` |
| 38. Sobra por grupo | DEMONSTRADO | Deduplicação carrega as decisões por grupo, com teste 3 × 6 e seletor da sobra. `app/chamados/management/commands/dedup_perguntas_fato.py:75–76`, `app/chamados/tests/test_e4c_f5_voltaC_lotes.py:230–237`, `app/chamados/tests/e4c_f5_voltaC_mutantes.py:22` |
| 39. REDs 3 × 6 da A | SO POR EXECUCAO | Recontados 12 métodos no módulo, com 11 comparativos de lote sobre 9 K e um O261 por candidata. A fixture do fio tem 1 × 5. Placar nominal e RED da passagem, EXECUCAO NAO LISTADA. `app/chamados/tests/test_e4c_f5_voltaA_lotes.py:44–240`, `app/chamados/tests/test_e4c_f5_volta2_juizes.py:154–186` |
| 40. REDs e mutantes da B | FALTA | Recontados 15 métodos para 14 K, com K46 em dois métodos. Há 14 tira-lote e 5 variantes funcionais. Falta conciliar os 29 cobertos com os 27 editados, K41/K42 e 5 L + 3 Q. `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:54–251`, `app/chamados/tests/e4c_f5_voltaB_mutantes.py:12–52`, `app/docs/RELATO.md:94` |
| 41. REDs e mutantes da C | FALTA | Recontados 10 testes de lote, 9 K e uma sobra, mais o teste de releitura K13. Falta vincular nominalmente manutenção, 3 M e sobra. `app/chamados/tests/test_e4c_f5_voltaC_lotes.py:89–264`, `app/chamados/tests/e4c_f5_voltaC_mutantes.py:12–23`, `app/docs/RELATO.md:94` |
| 42. RED novo 3 × 6 do C11 | DEMONSTRADO | K2 e K1 cobram SELECTs de decisões, PKs julgados e PKs devolvidos pelo C11. `app/chamados/tests/test_e4c_f5_voltaA_lotes.py:23–40,207–240` |
| 43. Tetos existentes E2 e C11 | SO POR EXECUCAO | Os dois selos estão na cópia. A suíte final dá verde agregado. Diff de preservação e placar nominal, EXECUCAO NAO LISTADA. `execucoes.txt:2`, `app/chamados/tests/test_e2_tarefa_do_dia.py:913–938`, `app/escala/tests/test_f3_beco1_folga_na_lista.py:378–402` |
| 44. O261 por candidata, teto 9 | SO POR EXECUCAO | Um teste cobra igualdade de custo por candidata e teto 9. Outro cobra um SELECT de decisões por candidata. Remedição nominal final, EXECUCAO NAO LISTADA. `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:187–207`, `app/chamados/tests/test_e4c_f5_voltaA_lotes.py:188–204` |
| 45. Tabela de selos remedidos | FALTA | O RELATO publica os quatro deltas do chokepoint e remete a atribuição nominal ao anexo. Falta consolidar antes/depois, motivo e rodada por selo. `app/docs/RELATO.md:94`, `app/ponto/tests/test_selo_chokepoint_escrita.py:196–267`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:205–207`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:578–593` |
| 46. Quatro tetos do chokepoint | DEMONSTRADO | Tetos anotados com sítio e classe de leitura, 230→234, 145→146, 294→298 e 219→221. `app/ponto/tests/test_selo_chokepoint_escrita.py:196–206,220–230,248–253,260–267` |
| 47. Quatro casos verdes do chokepoint | SO POR EXECUCAO | Existem quatro casos, 1 e 3 furos × fecha e fica. Verde agregado na execução 1. Remedição nominal final, EXECUCAO NAO LISTADA. `execucoes.txt:2`, `app/ponto/tests/test_selo_chokepoint_escrita.py:467–475,502–510` |
| 48. Atribuição SQL e escritores novos | SO POR EXECUCAO | As anotações atribuem o delta anterior a SELECT e registram zero INSERT/UPDATE/DELETE/SAVEPOINT a mais. Captura final por SQL, verbo e sítio, EXECUCAO NAO LISTADA. `app/ponto/tests/test_selo_chokepoint_escrita.py:196–205,220–229,248–252,260–266` |
| 49. Harness dentro das exceções autorizadas | FALTA | O selo cadastra nove módulos e compara contagem de asserts. Falta a partição nominal em 11 famílias e o vínculo de cada alteração à exceção. `app/chamados/tests/test_e4c_f5_duble_conforme.py:14–35,76–80`, `app/docs/RELATO.md:94` |
| 50. Onze famílias verdes | SO POR EXECUCAO | Há 11 métodos em `test_pergunta_viva` e seis no selo do dublê. A execução 1 dá verde agregado. Placar por família, EXECUCAO NAO LISTADA. `execucoes.txt:2`, `app/chamados/tests/test_pergunta_viva.py:16–76`, `app/chamados/tests/test_e4c_f5_duble_conforme.py:51–87` |
| 51. Décima segunda família | FALTA | O RELATO nomeia os dois RED-8. Falta a identificação da família no inventário de 12. `app/docs/RELATO.md:94`, `app/chamados/tests/test_e4c_f5_volta2_juizes.py:145–186` |
| 52. Medida de `respondida` | SO POR EXECUCAO | O RELATO documenta a comparação anterior nos dois sentidos. Sonda específica final da property, EXECUCAO NAO LISTADA. `app/docs/RELATO.md:92,782`, `app/chamados/models.py:3838–3844` |
| 53. Medida de `sem_veredito_entre` | SO POR EXECUCAO | O leitor está na cópia e o RELATO aponta medição anterior. Comparativo específico final por decisão, EXECUCAO NAO LISTADA. `app/docs/RELATO.md:92`, `app/chamados/models.py:2002–2027` |
| 54. Medida de `estado` | SO POR EXECUCAO | O leitor está na cópia e o RELATO aponta medição anterior. Comparativo específico final por rótulo, EXECUCAO NAO LISTADA. `app/docs/RELATO.md:92`, `app/chamados/models.py:3249–3268` |
| 55. Conversão dos 96 chamadores | FALTA | Recontados 44 chamadores diretos em 22 arquivos, 40 do juiz de resposta e 4 do temporal, com 43 compostas e uma exceção. Falta a lista histórica dos 41 diretos + 55 indiretos por volta. `app/chamados/tests/test_e4c_f5_selo_chamada.py:23–28,126–160`, `app/docs/RELATO.md:37,94` |
| 56. Selo de chamada final em zero | DEMONSTRADO | Zero chamadas fora da catraca na recontagem AST, com uma exceção nominal para `creditos_perguntas`. `app/chamados/tests/test_e4c_f5_selo_chamada.py:23–28,126–160,188–193`, `app/chamados/services/raiox.py:135–147` |
| 57. Catraca publicada por volta | SO POR EXECUCAO | O RELATO publica 36 fora das permitidas na base, 24 na A, 10 na B, 0 na C e 0 em `9e90d`, com 44 reconhecidas no final. A execução 4 dá o item por fechado. Origem do número histórico 19 [nao verificado]. `app/docs/RELATO.md:12–13,37`, `execucoes.txt:29` |
| 58. Commit Opus de `models.py` | HISTORICO | Recontados 9 métodos, 6 com RED nominal. Valem log e hash do commit próprio. Ordem anterior à A e vínculo K41/K42 [nao verificado]. `avais_depois_do_vPRONTO5.txt:29`, `app/chamados/tests/test_e4c_f5_opus_models.py:57–167`, `app/docs/RELATO.md:94` |
| 59. Limite da exceção de folha | SO POR EXECUCAO | Execução 4 registra diff restrito ao prefetch e ao argumento composto do juiz. Os dois sítios estão na cópia. `execucoes.txt:29`, `app/folha/previa.py:64–67,139–146` |
| 60. Prévia da competência 11 | SO POR EXECUCAO | O RELATO registra 4 empresas, 450 colaboradores, 5.861 linhas e zero diferenças em cópia anterior. Comparativo final por colaborador e dia, EXECUCAO NAO LISTADA. `app/docs/RELATO.md:782`, `app/folha/previa.py:64–67,135–146` |
| 61. Fixtures dos dois RED-8 | DEMONSTRADO | Há dois casos, com ato pela porta e projeção passiva. O fio chega à quinta pergunta. `app/chamados/tests/test_e4c_f5_volta2_juizes.py:27–35,119–186` |
| 62. Chamadas ORM da volta 2 | DEMONSTRADO | Casos de fonte passam `D.composta(p)`. A instância ORM do passivo serve de controle. `app/chamados/tests/test_e4c_f5_volta2_juizes.py:37–117` |
| 63. Mutante mira o compositor | DEMONSTRADO | A transformação renomeia o parâmetro e troca a chamada ao compositor pela instância ORM. `app/chamados/tests/e4c_f5_volta2_mutantes.py:11–38` |
| 64. Tabela F2 por leitor | DEMONSTRADO | Sete leitores, cinco `False` e dois `True`, com motivo por leitor. `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:498–521` |
| 65. Instante e validação pelo ato | DEMONSTRADO | Dois casos opõem a fonte do ato à projeção. `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:523–544` |
| 66. Leitor F1 B2 | DEMONSTRADO | O teste exige uma delegação ao juiz e argumento `LeituraComposta`. `app/chamados/tests/test_f1nova_b2_leitores.py:87–97` |
| 67. `_ancoradas` | DEMONSTRADO | O selo verifica 2 consultas com 1 e com 5 ancoradas, incluindo o prefetch. `app/chamados/models.py:349–366`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:569–593` |
| 68. Inventário das adaptações | FALTA | O RELATO aponta a tabela e identifica dois usos sem aval individual, contrato de fechamento e `test_e4_expand`. Falta fechar o inventário por uso, antes/depois e aval. `app/docs/RELATO.md:94`, `app/chamados/tests/e4b_c12_ast.py:18–27`, `app/chamados/tests/test_e4c_f5_duble_conforme.py:76–80` |
| 69. Placar raio-X pela face SQL | DEMONSTRADO | `creditos_perguntas` conserva `values_list` e adapta a tupla para o juiz. `app/chamados/services/raiox.py:135–147`, `app/chamados/tests/test_e4c_f5_selo_chamada.py:23–28` |
| 70. Registro nominal do raio-X | DEMONSTRADO | O complemento da E4D nomeia `raiox.py:135` e a face SQL. `app/docs/BACKLOG.md:2048` |
| 71. `resposta_display` lê o ato | DEMONSTRADO | A property formata a resposta lida do ato. A tabela F2 fixa a expectativa. `app/chamados/models.py:3865–3891`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:511–521` |
| 72. Registro nominal de `resposta_display` | DEMONSTRADO | O complemento da E4D nomeia a property e o texto de exibição pelo ato. `app/docs/BACKLOG.md:2048` |
| 73. Medida vigente de display | SO POR EXECUCAO | Execução 4, `item73_display.out`, universo 51.953, erros 0, literal 16, LIKE 0 e CRU 0. A primeira sonda teve 36.212 erros de instrumento e está registrada separadamente como inválida. `execucoes.txt:29`, `app/docs/RELATO.md:12` |
| 74. Pouso único | ETAPA FUTURA | Certificação e pouso integral da F5. A pré-Opus consta no 16. `avais_depois_do_vPRONTO5.txt:29`, `app/docs/BACKLOG.md:2053`, `execucoes.txt:2,31` |
| 75. Smoke de leitura do app | ETAPA FUTURA | GET com badge e pendentes nas duas cascas, por cinco minutos, com registro de traceback. `avais_depois_do_vPRONTO5.txt:29`, `app/api/urls.py:92`, `app/api/views.py:2536–2556` |
| 76. Smoke humano do modal | ETAPA FUTURA | Abertura do modal após pouso. A entrada marca notas como lidas. `avais_depois_do_vPRONTO5.txt:29`, `app/chamados/views.py:869–880`, `app/chamados/services/nota.py:126–133` |
| 77. Censo dos escritores da coluna | DEMONSTRADO | Recontados 9 escopos e 14 ocorrências, 7 atribuições, 5 `save` e 2 descarimbos em lote, com zero acusações pelo analisador. `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:34–44,439–501,536–546`, `app/chamados/models.py:1582,1721,3372,3400,3412`, `app/chamados/services/validacao.p

[truncado em 20000 de 59464 caracteres]

## 21/11 05:00 · sessao ca360470c6b3 · E4C-F5-CONSTROI · censo · origem=Code

| Achado | Classe | Trecho |
|---|---|---|
| **1. Bateria completa de mutantes no hash final** (`opus.r:3–9`) | **(c) Execução** | **Pergunta:** “Pela bateria anexa: existe outra entrada da tabela que so morde num modo e fica verde no outro?” (`volta4_verif.q:5`). **Resposta:** “Na bateria anterior, os 11 mutantes que ficaram na tabela retornaram `rc=1` nos dois modos.” e “A execução Django no hash atual [nao verificado].” (`volta4_verif.r:9,5`). A sessão registrou “155 mutantes = MORDE 145, NAO MORDE 10” em `c8eb8eddf` e prometeu “A bateria RODA DE NOVO no hash final”. A publicação de `9e90d9253` registra “11 entradas mordem nos DOIS modos”. A rerrodagem dos 155 no hash final e um `PAREI LISTA` final com os dez ficam **[nao verificado]**. (`linhas_triagem_relato.txt:66`; `app/docs/RELATO.md:12,62`; `opus.r:5–8`) |
| **2. PRONTO aberto na abertura do opus** (`opus.r:10`) | **(e) Não perguntado** | Ao 2º leitor foi pedido um “delta SO DE TESTE”. Sua resposta delimita a conferência a “quatro `.py`, todos de `app/chamados/tests`”. Faltou pedir o confronto item a item do PRONTO com as provas e os avais no hash integrado, incluindo quais pendências exigiam `PAREI` antes do opus. Essa conferência entrou no brief final como “Algum item do PRONTO esta marcado fechado sem a prova que o aval exige?”. A sessão já havia publicado “NAO FECHA” e mantinha “INCOMPLETO (nada disto toca .py): os FALTA acima”. O `PAREI CODIGO` anterior aparece como respondido para os itens 78 e 06. (`volta3_verif.q:1,8–10`; `volta3_verif.r:44`; `volta4_verif.q:1–5`; `brief_opus.md:34`; `linhas_triagem_relato.txt:4,96,100`; `app/docs/RELATO.md:94,96`) |
| **3. Limites da seção 7 mantidos em fila** (`opus.r:11–17`) | **(b) Visto e deixado fora da rodada** | **3(a) Par de alvo e valor.** Pergunta: “E quais formas vazias reais escapam?” Resposta: “O selo de atos ainda aceita presença da coluna como evidência”, com `alvo_lido.exclude(pk__in=ids)` entre as variantes verdes. Triagem: “Restos descartados (append/extend/**kwargs, filtro por dado) vao anexos ao opus.” A sessão registrou zero sítios de produção acusados. (`volta3_verif.q:4`; `volta3_verif.r:16,48`; `linhas_triagem_relato.txt:21–22`)<br>**3(b) Projeção de chamadas.** Pergunta: “o salto por auxiliar (quantos saltos, mesmo arquivo ou nao)”. Resposta: “Continua de um salto no mesmo arquivo” e “Os gravadores alcançados por essas chamadas entram diretamente no censo.” A triagem aplicou o corte geral da “volta 4 estreita”: “nada da secao 7 do verificador entra, vai anexo ao opus”. Motivo específico na triagem **[nao verificado]**. O laudo final cita o scanner com “0 fora das permitidas”. (`verif_delta.q:6`; `volta3_verif.q:9`; `volta3_verif.r:49`; `linhas_triagem_relato.txt:21`; `app/docs/RELATO.md:13`; `opus.r:13`)<br>**3(c) Catraca parcial.** Pergunta: “Que formas de update_fields escapam do visitante (lista montada fora, soma de listas, variavel, tupla, kwargs)?” Resposta: “Continuam escapando `append`/`extend`, `**kwargs`”, com confirmação no `save` da RECUSA. Triagem: “sem ampliar a lista de funcoes, o resto descartado e anexado ao opus”, pelo escopo literal do aval 1945. (`verif_delta.q:14`; `volta3_verif.q:9`; `volta3_verif.r:50`; `linhas_triagem_relato.txt:22,25`)<br>**3(d) Relógio nos asserts ORM.** Pergunta: “Algum dos tres arquivos le relogio, banco, env ou arquivo fora da arvore de um jeito que faca o veredito variar entre rodadas?” Resposta: “Se a reabertura sair anterior à validação, a validação pode continuar vigente.” Triagem: “pre-existente, fica na fila sem cura nesta volta”, reiterado em “(f) relogio :756/:774 fica (fila)”. (`verif_delta.q:17`; `volta3_verif.q:9`; `volta3_verif.r:51`; `linhas_triagem_relato.txt:22,25`) |
| **4. Comentários `Transitorio (B/C)`** (`opus.r:18`) | **(e) Não perguntado** | A pergunta mais próxima pede “Para cada um campo/valor que o delta le (arquivo:linha), quem escreve e quem le em producao.” A resposta mapeia “`validada_em` e `respondida_em` vigentes” e cita os juízes. Faltou perguntar se os comentários sobre chamadores ORM ainda descreviam o fluxo depois da volta C. A cópia contém literalmente “Transitorio (B/C): chamador que ainda entrega ORM le as colunas da pergunta.” nos dois sítios. (`volta3_verif.q:10`; `volta3_verif.r:57`; `volta4_verif.q:1–5`; `app/chamados/juizes.py:1390,1559`) |
| **5. Escopo declarado como não verificado pelo leitor final** (`opus.r:19`) | **(e) Não perguntado** | O 2º leitor recebeu “Para cada um campo/valor que o delta le [...] quem escreve e quem le em producao” e respondeu, por exemplo, “Para `devolucao`, escrevem as duas portas de devolução.” Faltou pedir o censo completo de escritores e leitores do **diff de produção** em `materializacao.py` e `validacao.py`, o vínculo de cada sítio com a coluna `PROTEGE` e a comparação de `JUIZES_POR_VARREDURA` com a base. O brief final pede “QUEM ESCREVE e QUEM LE” nos dois arquivos. O laudo registra que só confirmou a L-130 e considerou o cron inaplicável. A comparação do cron com a base fica **[nao verificado]**. (`volta3_verif.q:10`; `volta3_verif.r:58`; `verif_delta.q:4–19`; `volta4_verif.q:1–5`; `brief_opus.md:29–31`; `opus.r:19`; `app/docs/LEIS.md:151`; `app/config/crons.py:2061`) |

No **item 3**, a volta 4 teve “0 achado”. O `PAREI SITIO` já constava da triagem como “EXCLUSAO antes da porta, nunca o False do par”, e a sessão publicou a justificativa pelos dois seletores com `validada_em__isnull=True`. O par literal de pergunta e resposta do 2º leitor sobre esses sítios fica **[nao verificado]**. O laudo também registra a falta de entradas nominais no BACKLOG para **3(a)** e **3(d)**. (`linhas_triagem_relato.txt:18,134`; `app/docs/RELATO.md:92,116`; `app/chamados/reconciliador.py:681,1101`; `opus.r:12,15–17`)

**Contagem por item numerado:** (a) **0** · (b) **1** · (c) **1** · (d) **0** · (e) **3**. (`opus.r:3,10–11,18–19`)

**Identidade da cópia:** hash `b0935c1f106d232dc39b263676a588d66eddff54` indicado no brief e no relato raspado. Confirmação do `HEAD` local **[nao verificado]**. (`brief_opus.md:3–4`; `linhas_triagem_relato.txt:4`)

## 21/11 04:55 · sessao ed762561508b · FALTA-APROVADA-MOVE-O-TXT-ENTREGUE · leitura · origem=Code

## 1. Dono dos caminhos A → B

Na saída da sombra, **53 casos de colaborador por competência, de 47 PKs distintos, mudaram as linhas recompostas**. A é a foto com a falta aprovada. B é a foto após `decidir(..., "rejeitada")`, dentro do savepoint revertido. A sonda compara as linhas montadas a partir do `FechamentoMensal` gravado. O efeito sobre reexportação, importação ou pagamento está [nao verificado]. `fa_txt_sonda3.out:3`, `sonda_ab.py:172–188,286–308,311–333,352–370,381–390`, `app/folha/views.py:209–214,270,299–302`.

| Caminho com mudança | Crivo que decidiu | Dono pela L-099 e pelo E6 |
|---|---|---|
| `entra → entra`, **34 casos / 29 PKs** | Passa pelos crivos nas duas fotos. A montagem consulta `datas_falta` e, com a rubrica `falta_diurna`, emite um tipo 10 com a quantidade e um tipo 11 por dia. O delta é compatível com retirar ou reduzir esse bloco. `fa_txt_sonda3.out:3`, `app/folha/export.py:87–92,169–185,215–221,740–744,837–842,968–981`. | **Nenhum dos três responde por esse delta.** A comparação troca um insumo que o montador lê. A L-099 cobra coerência com os dados existentes e o E6 julga uma divergência de dia-colaborador. `CLAUDE.md:179`, `app/docs/LEIS.md:122`, `app/ponto/management/commands/e6_oraculo.py:53–55,98–110,350–378`. |
| `entra → fora / furo_espelho`, **17 casos / 16 PKs** | Em B, `motivos_retencao_espelho` devolve motivo, o classificador encerra ali e o montador pula o fechamento inteiro. `fa_txt_sonda3.out:3`, `app/folha/export.py:716–727,968–973,1301–1307`. | **Nenhum dos três é atribuível pelo caminho observado.** `furo_espelho` é o resultado do crivo da folha. O dono de eventual divergência diária está [nao verificado]. `CLAUDE.md:179`, `app/docs/LEIS.md:122`, `app/folha/export.py:1264–1298`, `app/ponto/management/commands/e6_oraculo.py:53–55,98–110,182–195`. |
| `fora / furo_espelho → entra`, **2 casos / 2 PKs** | A retenção aparece em A. Em B, o espelho passa, a homologação também passa e o montador inclui o fechamento. `fa_txt_sonda3.out:3`, `app/folha/export.py:716–739,837–842,968–981`. | **Nenhum dos três é atribuível pelo caminho observado.** O fim da retenção responde à pergunta de entrada no TXT. O dono de eventual divergência diária está [nao verificado]. `CLAUDE.md:179`, `app/docs/LEIS.md:122`, `app/ponto/management/commands/e6_oraculo.py:53–55,98–110,182–195`. |

A ordem executada por `dono_da_divergencia` é **CADASTRO** para folga indefinida ou falta de vínculo, **BATIDA** para batida fora de turno ou contagem ímpar, **CADASTRO** para fase deslocada ou dia na lista `dias_cadastro_x_realidade`, e **ESTRUTURA** no residual de uma divergência diária. A sonda registra `status` e `motivo` do exportador e agrega o delta por tipo de linha. Os insumos diários do E6 estão [nao verificado]. `app/ponto/management/commands/e6_oraculo.py:98–110,162–195,198–212`, `sonda_ab.py:322–349,357–370`.

## 2. Por que `fora → entra`

**O crivo que deixou de reter foi `furo_espelho`**, nos PKs **161** e **504**. Cada um passou de zero para uma linha tipo 10. O retorno que retinha em A está em `export.py:968–973`, antes da homologação em `export.py:974–979`. `fa_txt_sonda3.out:3`, `app/folha/export.py:968–981`.

O `save` da rejeição aciona o cartório dos dias da ausência. O cartório consulta a cobertura para cobrança, que exclui rejeitadas, e pode relavrar o veredito, os protestos e a ata que o espelho lê. Um caminho previsto no código é um turno encerrado com fato em dia coberto por ausência aprovada. Isso pode gerar `FATO_EM_AUSENCIA`, que vira `fato_em_ausencia` e pode reter o fechamento. O dia, o protesto e o veredito que efetivamente deixaram de reter cada PK estão [nao verificado]. A sonda guardou o motivo geral e descartou `motivos_espelho`. `app/ponto/services/ausencia.py:975–988,1084–1088`, `app/ponto/signals.py:85–93,143–147`, `app/ponto/turnos.py:2523–2532`, `app/ponto/services/cartorio.py:1382–1385,1500–1511,1898–1922`, `app/ponto/supra_juiz.py:392–418`, `app/escala/models.py:1619,1767–1768`, `app/folha/export.py:968–973,1247–1298`, `sonda_ab.py:322–329`.

## 3. O “1 sem classe”

O **PK 676, empresa 4, 07/2026**, tinha falta candidata e nenhum `FechamentoMensal` na competência em A ou B. A sonda lista candidatos pela falta, consulta o fechamento depois e passa `[]` ao classificador. Sem item retornado, grava `status=null` e `motivo=null`. Com zero linhas nas duas fotos e zero exceções, o caso é **conclusivo e sem delta**. O “sem classe” é a ausência de veredito do exportador para esse caso. A razão de o fechamento estar ausente está [nao verificado]. `fa_txt_sonda3.out:3`, `sonda_ab.py:263–283,311–333,394–420`, `app/folha/export.py:716–727,782–788,980–981`.

## 4. Leis e cortes da competência exportada

- **L-092 e L-113.** A redação da L-092 diz “COMPETENCIA EXPORTADA NAO MUDA O GRAVADO” e manda a diferença para “Pauta DP com os dois numeros”. A L-113 diz “o sistema calcula o que tem” e “Se o export ficou diferente, refaz”. Ela revoga a recusa geral de recálculo, a guarda de regeneração **como trava** e a Pauta DP **como precondição**. Conserva a versão anterior do TXT intacta, a trilha e o DIFF de impacto publicado antes de ato de frota. No serviço, a recusa geral deu lugar a `WARNING` quando há escrita em escopo exportado. `app/docs/LEIS.md:125,155`, `app/ponto/services/fechamento.py:485–498,525–547`.
- **L-052 e REGEN-EM-EXPORTADA.** A redação antiga diz “DIA QUE VIROU FOLHA NAO SE TOCA, nem para corrigir” e está marcada como superada em parte. A consolidação de 31/10 diz que “o GRAVADO (`FechamentoMensal`) nao muda por caminho generico” e exige `!`, DIFF de frota publicado antes, reversão em `logs/`, prova depois e passagem pela REGEN-EM-EXPORTADA. Exige staff, trilha com quem, quando, motivo e hashes antes/depois, TXT parcial de retificação com hash publicado e Pauta DP com pago e devido. Na cópia, o regenerador ainda pula a janela lavrada salvo `apesar_da_lavra` com motivo. A porta de retificação recebe um colaborador por chamada e devolve o material antes/depois. O limite original de “UM colaborador por ato” foi revogado no corte de 30/09. `app/docs/LEIS.md:93,155`, `CLAUDE.md:401–417`, `app/ponto/portas/celula.py:213–221,284–288`, `app/ponto/portas/regen_exportada.py:26–27,107–119,174–185,204–225`, `app/docs/LAPIDES.md:45–48,654–658`.
- **TXT-E-FOTOGRAFIA e K6.** “Correcao provada entra em QUALQUER competencia, a qualquer momento.” Há um TXT vigente por empresa e competência. Gerar de novo substitui o vigente, as versões anteriores ficam guardadas com conteúdo e hash, e importação e pagamento são do Domínio. O K6 define entregue como “existe ExportacaoDominio vigente para empresa+competencia”. Para a guarda de geração de células, o outro corte inclui também holerite publicado. `app/docs/PROMPTS.md:286`, `CLAUDE.md:418–422`, `app/docs/CORTES.md:374–375`, `app/folha/export.py:580–594,597–628`, `app/folha/models.py:143–170,174–187`.
- **EXPORTADO-SEM-FRONTEIRA, FIO-EXPORTADA-X-O44 e L-124.** O primeiro corte diz “Dia exportado segue EDITAVEL, celula soberana, folha/TXT recalcula online”. O segundo retira “exportada nunca vai ao app, so leitura” e, para as perguntas vivas em disputa fechada daquele corte, estabelece “so Validar, sem reabrir, sem push”. A L-124 explicita “fato novo rejulga normal em qualquer competencia; correcao provada em exportada segue a porta de hoje”. Os cortes K6 de 15 e 16/11 sobre plantio de metade de intervalo têm esse escopo, e o de 16/11 exclui expressamente `AUSENCIA`. `app/docs/LEIS.md:91,145`, `app/docs/CORTES.md:318,426,461,520,530`.
- **Guarda específica de 09/2026.** O corte BO-B diz “09 nao recalcula por evento ate DIFF da 09 [...] publicado + reversao + Pauta DP” e “TXT novo da 09 so quando o DP pedir”. Na cópia, `GUARDA_COMPETENCIA` ainda exclui do recálculo em modo de escrita os colaboradores das empresas que entregaram 09/2026. A montagem que a sonda chamou lê o fechamento e as fontes do exportador, sem passar por esse recálculo. `app/docs/PROMPTS.md:1370`, `app/ponto/services/fechamento.py:19–43,539–547,558–575`, `sonda_ab.py:311–333`.

O aval desta obra determina “Nada se toca; dono L-099 depois do numero”. O **53** mede mudanças nas linhas recompostas da sombra. O número de alterações no arquivo entregue e o impacto financeiro estão [nao verificado]. A identidade Git da cópia também está [nao verificado]. `respostas_2011_1945.txt:5`, `app/docs/BACKLOG.md:2060`, `fa_txt_sonda3.out:3`, `sonda_ab.py:311–333,357–370`.

## 21/11 03:07 · sessao 21d8d718d744 · CRON15-PRAZO-E-FOLGA-RELOGIO · verificador · origem=Code

## 1. Cumprimento dos avais

- **Item 1, prazo.** O delta muda `PRAZO` de **20/11 para 27/11/2026 inclusive**, cita os dois avais e registra que a prorrogação é única. Em **28/11** o decorador deixa de aplicar `expectedFailure`. A varredura AST encontrou exatamente os 15 nomes da lista. O tripwire foi preservado. No render em memória, 5.188 s e 4.681 s ainda recusaram a sombra, e 4.680 s couberam. A expressão “abaixo de 3120 s” no comentário continua imprecisa na igualdade, que o próprio comentário admite com `json <= 4680 s`. `respostas_2111_0300.txt:3–4` · `app/core/tests/marca_cron15.py:8–18,27–65` · `app/core/tests/test_marca_cron15.py:25–50,67–87` · `app/config/crons.py:259–278`
- **Complemento, instalador.** O prazo muda deliberadamente o comportamento do render usado pelo `install` entre **21 e 27/11**. Com uma única linha ativa da sombra que coincide com a linha renderizada, o caminho pode preservá-la e emitir o aviso com a nova data. Conferi em memória com crontab sintético que o aviso sai, o render coincide com a entrada e a recusa volta em 28/11. O `check` continua no render puro. O `install --seco` contra o host, o diff vazio real e a ocorrência de qualquer instalação estão **[nao verificado]**. `complemento_2111_0305.txt:2–3` · `app/core/management/commands/crons.py:49–60,92–119` · `bin/crons.sh:23–40`
- **Item 2, relógio.** O teste fixa 21/10 dentro da classe, elimina a leitura direta de `timezone.localdate()` e tira a entrada da lista. A comparação do código sustenta o RED/GREEN descrito abaixo. A execução Django do RED e do GREEN está **[nao verificado]**. `respostas_2111_0300.txt:6` · `app/escala/tests/test_folga_segue_a_pessoa.py:26–43,64–75` · `delta.diff:37–49,86–103`
- **Item 3, ordem e registro.** Os três blobs da cópia e os três reconstruídos pela reversão do diff conferem com os prefixos do anexo. O diff tem só os dois `.py` de teste e a lista. Nenhum `.py` de produção, teste adicional marcado, cron, folga 1,5, JSON, vizinho, `SKIP_TESTS` ou horário foi alterado **no delta anexado**. A vaga da sombra depois da F5 e antes da CADEIA, com brief pelo censo, não aparece registrada no delta. A identidade do commit e do pai, o pouso, o registro fora do delta, a integração da F5, a suite inteira verde no hash integrado antes do opus e a condição para dispensar a remedição estão **[nao verificado]**. `delta.diff:1–103` · `respostas_2111_0300.txt:5,7–9` · `app/docs/BACKLOG.md:852,1931,1960` · `CLAUDE.md:320–328,372–381`

## 2. O que os seis testes julgam em 21/10 [nao verificado]

A empresa da fixture usa o corte padrão 21. Com o relógio em 21/10, a janela corrente é **21/10–20/11**, o vínculo novo começa em **16/10** e o antigo termina em **15/10**. As mordidas abaixo são previsões pela leitura do código. A execução com mutações está **[nao verificado]**. `app/colaboradores/models.py:46` · `app/ponto/janelas.py:10–33,60–63` · `app/escala/tests/test_folga_segue_a_pessoa.py:26–59`

| Teste | O que ainda julga e como mostrar a mordida | [nao verificado]
|---|---|
| **Troca automática, 21/10** | A criação do novo vínculo tem de mover a `FolgaDia`, deixar uma trilha que cite o vínculo antigo e gerar a célula com `trabalha=False`. Desligar só o signal de folgas derruba a igualdade do vínculo. Preservar a migração e desligar só a re-lavra deixa o `.get()` sem célula. `app/escala/tests/test_folga_segue_a_pessoa.py:64–75` · `app/escala/signals.py:185–207` · `app/escala/folgas.py:280–299,512–525` |
| **Resíduo, 23/10** | Exercita uma **futura gerada já existente**, plantada como trabalho. Cobra uma migração, segunda chamada vazia, vínculo novo, uma trilha e a virada para folga. Suprimir a migração derruba a primeira contagem. Suprimir só a re-lavra futura deixa `trabalha=True`. `app/escala/tests/test_folga_segue_a_pessoa.py:77–89` · `app/ponto/services/cadastro_futuro.py:19–25,43–59` |
| **Competência anterior, 17/10** | Cobra `{'migrada': 1}`, `trabalha=True` e zero regenerações. Desligar a porta inteira derruba a contagem. Omitir só a gravação do novo vínculo e ainda retornar `migrada` pode passar, pois o teste não relê a `FolgaDia`. Forçar a re-lavra normal derruba a proteção da célula. “Intocada” é mais amplo que as asserções: a porta pode re-julgar a célula de um mês que saiu do defeito sem regenerar seu DNA. `app/escala/tests/test_folga_segue_a_pessoa.py:91–101` · `app/escala/folgas.py:283–296,512–526` · `app/ponto/portas/celula.py:289–311,332–350,460–472` |
| **Sem vínculo, 19/10** | Cobra `sem_vinculo` nas duas chamadas, a folga antiga preservada e exatamente uma trilha. Uma porta vazia falha na primeira chamada. Tirar a deduplicação da trilha falha na última asserção. `app/escala/tests/test_folga_segue_a_pessoa.py:103–113` · `app/escala/folgas.py:493–509,529–542` |
| **Destino ocupado, 24/10** | Com `F1` já no destino, cobra `ocupado` e preservação da `FD` antiga. Uma porta vazia falha no retorno. O teste não confere a identidade da `F1` depois da chamada nem a trilha de ocupado. `app/escala/tests/test_folga_segue_a_pessoa.py:115–122` · `app/escala/folgas.py:497–509` |
| **Folga de calendário, 25/10** | Passa com a migração inteira desligada: `{}` e a folga no vínculo antigo são justamente o resultado esperado. Isso já valia antes do delta. Para mostrar a mordida, faça uma mutação que deixe a folga de calendário entrar **e** retire a guarda que a devolve como `calendario`. A chamada pode então mover a folga para o novo vínculo e derrubar a asserção. Retirar só o filtro do lote deixa a outra guarda de pé. `app/escala/tests/test_folga_segue_a_pessoa.py:124–132` · `app/escala/folgas.py:484–485,535–542` · `delta.diff:50–103` |

**A cura não tornou o caso principal vazio.** O caso de calendário já era uma prova de exclusão, e o caso da competência anterior já tinha a lacuna de conferir o retorno sem conferir o vínculo gravado. `app/escala/tests/test_folga_segue_a_pessoa.py:64–75,91–101,124–132` · `delta.diff:50–103`

## 3. Escritores, leitores e números

- **`testes_sem_relogio.txt`.** A escrita encontrada é a edição versionada que remove a entrada quando o teste é curado. O selo abre o arquivo e compara a lista com a varredura estática. O placar lê e conta as linhas de dados. Executei o varredor em memória na cópia: **453 listados, 453 detectados, nenhuma diferença**. A linha removida leva o placar de **454 para 453**, fora do TOTAL do Code. Os números 465 e 464 no relato são registros históricos. `delta.diff:37–49` · `app/core/tests/testes_sem_relogio.txt:1–3,275–280` · `app/core/tests/test_selo_teste_sem_relogio.py:23,48–74` · `bin/placar_code.sh:456–459` · `app/docs/RELATO-ARQUIVO.md:4980–4982,13145–13149`
- **`PRAZO`.** A atribuição está em `marca_cron15.py:27`. Os leitores diretos são `no_prazo()` e, por ele, o decorador, o render do `install`, o selo da marca e o selo do instalador. Os dois selos calculam a borda a partir da constante. `pre-push`, `suite.sh` e a régua chegam aos testes via `manage.py test`. O contador de divergência de cron no placar usa `check`, que segue pelo render puro. `app/core/tests/marca_cron15.py:27,52–65` · `app/core/management/commands/crons.py:92–119` · `app/core/tests/test_marca_cron15.py:73–87` · `app/core/tests/test_crons_marca_cobre_install.py:94–172` · `bin/pre-push.sh:184–188` · `app/core/hooks/pre-push.sh:80–85` · `bin/suite.sh:319–325` · `bin/regua.sh:23,168` · `bin/placar_code.sh:33–39` · `bin/crons.sh:23–28`
- **Texto desatualizado.** O próprio módulo ainda diz “vermelho no dia 14”, e o docstring do selo diz “14/11”. Três imports comentam “prazo 13/11”. Backlog e Grade ainda associam o fim da marca a 20/11. O docstring de `_SchedDerivado` ainda diz que `check|install` fica vermelho. São referências textuais. A data efetiva usada pelo código e exibida no aviso é 27/11. `app/core/tests/marca_cron15.py:20,27,55–57` · `app/core/tests/test_marca_cron15.py:7` · `app/core/tests/test_contract_crons_duracao_horario.py:11` · `app/chamados/tests/test_contract_crons.py:16` · `app/chamados/tests/test_cron_longo_sai_da_medida.py:27` · `app/docs/BACKLOG.md:2309` · `app/docs/GRADE-MESTRA.md:471` · `app/config/crons.py:70–76` · `app/core/management/commands/crons.py:118–119`

## 4. Relógio que sobrou

O AST do arquivo curado não tem chamada direta a `now`, `today`, `localdate` ou `localtime`. Nos caminhos síncronos rastreados, as leituras ainda existentes são do relógio Python sob o `freeze_time` da classe: recomposição de turnos no save do vínculo, migração e re-lavra, carimbo da regeneração, cartório e invalidação do previsto. O serviço de futuras recebe `hoje` explicitamente. O selo estático sozinho seria evidência insuficiente, pois a presença de `freeze_time` faz o varredor liberar o arquivo inteiro. `app/escala/tests/test_folga_segue_a_pessoa.py:15–132` · `app/ponto/signals.py:53–74` · `app/escala/folgas.py:280–299,512–525` · `app/ponto/portas/celula.py:305–315,460–472` · `app/ponto/services/cartorio.py:2064–2089` · `app/ponto/services/fechamento.py:1607–1613` · `app/ponto/services/cadastro_futuro.py:28–59` · `app/core/tests/test_selo_teste_sem_relogio.py:24–45`

Não identifiquei leitura de relógio real fora do congelamento nessas rotas síncronas. A execução efetiva com `freezegun` e banco está **[nao verificado]**. Recalcular, rejuntar e reavaliar após commit são callbacks agendados, sem execução demonstrada por esses seis `TestCase`. O relógio de importação de `marca_cron15.py` pertence ao prazo do cron. `app/ponto/services/fechamento.py:1798–1824` · `app/ponto/portas/celula.py:528–540` · `app/escala/tests/test_folga_segue_a_pessoa.py:29–30,62–132` · `app/core/tests/marca_cron15.py:20–21,52–57`

## 5. Triagem da revisão

**Procede quanto à escolha do GREEN.** Com corte 21, o antigo `ini + 1` era amanhã no dia 21, hoje no dia 22 e passado nos demais dias da janela. A revisão acertou que **só congelar** manteria o erro, e que plantar uma célula futura permitiria manter aquele cenário. A cura escolhida conserva o dia 21 como data de execução e põe a folga no próprio dia 21, onde a porta pode criar a célula. O caso de resíduo continua cobrindo a re-lavra da futura existente. `triagem.txt:1` · `rev.r`, item 2 **[nao verificado]** quanto à execução · `delta.diff:95–103` · `app/ponto/janelas.py:10–33` · `app/escala/folgas.py:280–299` · `app/ponto/portas/celula.py:289–311` · `app/escala/tests/test_folga_segue_a_pessoa.py:77–89`

A demonstração literal é fixar **o mesmo 21/10 nos dois lados**, incluindo o relógio das chamadas disparadas pela troca. Com o corpo antigo e sem célula plantada, a previsão é `CelulaDia.DoesNotExist` em 22/10 depois das checagens de migração e trilha. Com o delta, a porta pode gerar a célula em 21/10 e a asserção exige `trabalha=False`. Esse RED reproduz a falha de calendário. Desligar o signal no caso curado demonstra a mordida sobre a migração. As duas execuções estão **[nao verificado]**. `respostas_2111_0300.txt:6` · `delta.diff:95–103` · `app/escala/tests/test_folga_segue_a_pessoa.py:64–75` · `app/escala/signals.py:185–207` · `app/ponto/services/cadastro_futuro.py:19–25` · `app/ponto/portas/celula.py:289–311`

## 21/11 02:58 · sessao 7a518789171c · REVISAO-AVAL-0300 · verificador · origem=Code

**AVAL-DEVOLVIDO nos itens 1 e 2. Item 3 — NAO PROCEDE como objeção à ordem.** O item 1 prorroga uma exceção usada pelo instalador de cron. No item 2, congelar o relógio sozinho reproduz a falha, mas a fixture precisa de uma célula futura para o teste passar. A ordem do item 3 é compatível com a exigência de suite verde na árvore que vai ao opus. `app/core/management/commands/crons.py:92–119` · `app/escala/tests/test_folga_segue_a_pessoa.py:53–64` · `app/ponto/services/cadastro_futuro.py:19–25` · `CLAUDE.md:320–328,377–378`

### Item 1 — PROCEDE

1. **Premissa.** `PRAZO` é **20/11/2026 inclusive**. `no_prazo()` compara com a data real capturada no import por `time.time()`. A partir de 21/11, o decorador conserva `__marca_cron15__`, mas deixa de aplicar `unittest.expectedFailure`. A varredura AST confirmou os 15 nomes abaixo. `app/core/tests/marca_cron15.py:24–62` · `app/core/tests/test_marca_cron15.py:25–50,73–87`
2. **Avais.** O CRON-15 original deu prazo até 13/11, e `F3-PORTA-E-CRON-PRAZO` o moveu para 20/11. O anexo dá uma nova data expressa para a marca e preserva as proibições de mexer na folga 1,5, no JSON, no vizinho, em `SKIP_TESTS`, na lista de marcados ou em cron e horário. A vaga proposta depois da F5 e antes da CADEIA é uma mudança de ordem expressa no anexo. O registro de `SOMBRA-NAO-CABE-2` mantém a condição distinta de **sete sombras limpas antes de 20/11**. Cumprimento dessa condição e conteúdo do censo citado: [nao verificado]. `app/core/tests/marca_cron15.py:7–15` · `app/docs/BACKLOG.md:852,1910,1931,1960` · `respostas_2111_0300.txt:3–5,9`
3. **Literal e tripwire.** O JSON da cópia traz **5.188 s já com a folga 1,5**, para um corredor de **4.680 s**. Conferi em memória que o render recusa com 5.188 s e 4.681 s e passa com 4.680 s. Logo, a borda efetiva inclui a igualdade de 3.120 s de p95 sem folga, apesar de o docstring dizer “abaixo”. Durante a marca, um marcado que passe vira `unexpected success`. O selo separado confirma a lista e exercita o render com a sombra reduzida para 4.620 s. O resultado dos 15 na suite inteira e o p95 vivo estão [nao verificado]. `app/config/crons_duracao.json:78,106,108` · `app/config/crons.py:182–183,219,259–278` · `app/core/tests/marca_cron15.py:9–11,26–27,57–62` · `app/core/tests/test_marca_cron15.py:48–50,67–71`
4. **Escopo e `!`.** O bloqueio é `CRON-MARCA-COBRE-INSTALL`. O `install` passa o crontab do host ao comando de produção, que lê **a mesma constante** e, se apenas a sombra não couber e houver uma única linha ativa idêntica à esperada, pode conservá-la. Com um crontab de teste em memória, confirmei que mudar só `PRAZO` reabre essa exceção de **21 a 27/11 inclusive** e volta a recusá-la em 28/11. O `check` usa render puro. Assim, mesmo com o diff em `tests/`, o prazo muda comportamento de produção e estende um fallback fora do alcance declarado de “só teste”. Isso pede `!` explícito para o `install`. O delta proposto não altera dados de competência exportada nem dinheiro. Estado do crontab real: [nao verificado]. `app/docs/RELATO.md:3401,3407` · `app/docs/BACKLOG.md:2309` · `app/core/management/commands/crons.py:49–60,92–119` · `bin/crons.sh:23–40` · `CLAUDE.md:265–268` · `respostas_2111_0300.txt:1,3–4,9–10`

**Os 15 nomes**, agrupados pelo prefixo `módulo.classe`:

| Prefixo | Métodos | Fonte |
|---|---|---|
| `chamados.tests.test_contract_crons.ContratoCronsTest` | `test_por_empresa_expande_universo`, `test_sem_duplicata` | `app/core/tests/marca_cron15.py:30–31` |
| `chamados.tests.test_contract_crons.ContratoOrquestracaoA5Test` | `test_allowlist_nao_esta_podre`, `test_ordem_dos_estagios_diarios` | `app/core/tests/marca_cron15.py:32–33` |
| `chamados.tests.test_contract_crons.ContratoPipelineB6Test` | `test_placar_acusa_dependencia_quebrada`, `test_sem_sobreposicao_no_bloco_diario` | `app/core/tests/marca_cron15.py:34–35` |
| `chamados.tests.test_contract_crons.ContratoPlacarA5Test` | `test_exit2_e_alarme_nao_erro`, `test_MORDE_a_corrente_nao_viaja_INLINE_na_linha`, `test_MORDE_nenhuma_linha_renderizada_passa_do_TETO_do_cron`, `test_placar_acusa_nao_rodou_e_erro`, `test_toda_linha_renderizada_passa_pelo_envelope` | `app/core/tests/marca_cron15.py:36–40` |
| `chamados.tests.test_cron_longo_sai_da_medida.CronLongoSaiDaMedidaTest` | `test_o_derivado_respeita_o_PISO_do_dump`, `test_o_derivado_termina_ANTES_do_vizinho_com_a_tolerancia` | `app/core/tests/marca_cron15.py:41–43` |
| `chamados.tests.test_cron_longo_sai_da_medida.MedicaoAusenteLevantaTest` | `test_MORDE_com_medida_ele_deriva_como_sempre` | `app/core/tests/marca_cron15.py:44` |
| `core.tests.test_contract_crons_duracao_horario.DuracaoSoDoHorarioTest` | `test_horario_livre_e_nome_desconhecido_contam_sempre` | `app/core/tests/marca_cron15.py:45–46` |

**Quem mais lê.** Os leitores diretos são o comando de produção, que chama `no_prazo` e imprime `PRAZO`, o selo da marca, que lê ambos, e o selo do `install`, que injeta `PRAZO` e `PRAZO + 1 dia`. `pre-push`, `suite.sh` e a régua chegam à marca indiretamente pelo `manage.py test`. `app/core/management/commands/crons.py:97,119` · `app/core/tests/test_marca_cron15.py:73–87` · `app/core/tests/test_crons_marca_cobre_install.py:99–172` · `bin/pre-push.sh:184–188` · `app/core/hooks/pre-push.sh:80–85` · `bin/suite.sh:319–325` · `bin/regua.sh:168`

### Item 2 — PROCEDE

1. **Premissa.** A linha 31 lê `timezone.localdate()` no `setUp` comum. Com o corte padrão 21 e o relógio em **21/11/2026**, `self.ini` é 21/11 e o primeiro teste escolhe **22/11**, portanto amanhã. A troca aciona a migração da `FolgaDia`. Para amanhã, a re-lavra só alcança células geradas já existentes. A fixture desse caso não cria uma, então a falha prevista é `CelulaDia.DoesNotExist` no `.get()` da linha 63, após as asserções de migração e trilha. A partir do dia 22 da mesma janela, a data testada já é hoje ou passada e a porta pode criar a célula. `app/escala/tests/test_folga_segue_a_pessoa.py:21–64` · `app/colaboradores/models.py:46` · `app/ponto/janelas.py:10–33,60–63` · `app/escala/signals.py:185–207` · `app/escala/folgas.py:280–299,512–525` · `app/ponto/services/cadastro_futuro.py:19–25` · `app/ponto/portas/celula.py:289–311`
2. **Avais.** A cura na fixture é compatível com a regra de re-lavrar futuras geradas que já existem e preserva o caso da competência anterior lavrada. O repositório já usa `freeze_time` no próprio teste de escala. `app/ponto/services/cadastro_futuro.py:1–4,19–25` · `app/escala/folgas.py:521–525` · `app/escala/tests/test_folga_segue_a_pessoa.py:80–90` · `app/escala/tests/test_contador_vigencia_impossivel.py:22,27–32`
3. **RED/GREEN.** O RED é realizável. Congele em 21/11 desde o `setUp` até as asserções, cobrindo também as leituras de relógio disparadas pela troca. Rode o corpo atual como RED. Para o GREEN, **mantenha 22/11** e plante, antes da troca, uma `CelulaDia` com `origem='gerada'`, `trabalha=True` e `escala_geradora=self.velho`. A porta real deve virar a célula para folga depois de migrar a `FolgaDia`. O próprio teste de resíduo já usa a técnica de plantar a célula. Congelamento sozinho deixa a célula ausente nos dois lados. Execução Django do RED/GREEN: [nao verificado]. `respostas_2111_0300.txt:6` · `app/escala/tests/test_folga_segue_a_pessoa.py:30–48,53–78` · `app/escala/folgas.py:280–299,512–525` · `app/ponto/portas/celula.py:289–311` · `app/escala/models.py:1309–1317`
4. **Escopo e outros testes.** A cura fica no `TestCase` e nas fixtures do banco de teste, sem alteração de `.py` de produção, cron, folha ou gravado exportado que peça `!`. **Todos os outros cinco testes** herdam o relógio da linha 31: resíduo (`:66`), competência anterior (`:80`), sem vínculo (`:92`), destino ocupado (`:104`) e folga de calendário (`:113`). Resíduo e competência anterior também exercitam caminhos cujo resultado depende de a data ser futura ou estar na janela anterior. `app/escala/tests/test_folga_segue_a_pessoa.py:19–32,51,66–121` · `app/escala/folgas.py:280–299,499–525` · `app/config/settings/ci.py:6–17` · `CLAUDE.md:265–268`

### Item 3 — NAO PROCEDE

1. **Premissa e ordem.** O pouso próprio do instrumento, a integração do `main` na cópia da F5 e a suite inteira verde dessa árvore antes do opus são compatíveis com o caminho documentado. `suite.sh` aceita a cópia por `--dir` e exige motivo `pre-opus` ou `pouso` para a inteira. `respostas_2111_0300.txt:7–8` · `CLAUDE.md:320–328,372–381` · `bin/suite.sh:255–262,283–295,316–325`
2. **Avais.** A ordem preserva o pouso separado e o escopo próprio da F5. O corte da F5 continua reservando `!` próprio para o estoque de competência entregue. `CLAUDE.md:325–328` · `app/docs/CORTES.md:560` · `respostas_2111_0300.txt:7–9`
3. **Literal.** A suite inteira é obrigatória no hash integrado mesmo se o delta for apenas de teste. A dispensa de repetir sombra, A/B e instante pressupõe um delta efetivo sem mudança de produção. O item 1 precisa ser resolvido antes de classificar o commit combinado como só teste, pois o `install` consome aquele prazo. Hash integrado, verde da inteira e validade das medidas anteriores na F5: [nao verificado]. `respostas_2111_0300.txt:7–8` · `CLAUDE.md:320–323,377–378` · `app/core/management/commands/crons.py:92–119`
4. **Escopo e `!`.** A sequência de integração e verificação não acrescenta autorização para editar produção, instalar cron, movimentar dinheiro ou tocar o gravado de competência exportada. O item 3 pode seguir como ordem, condicionado à resolução dos dois itens devolvidos. `respostas_2111_0300.txt:2,7–9` · `CLAUDE.md:265–268` · `app/docs/CORTES.md:560`

As linhas de `respostas_2111_0300.txt` foram numeradas a partir de `aval Ronald` no texto colado. A revisão foi em leitura, com verificações em memória e sem executar a suite Django. Confirmação Git do hash informado: [nao verificado]. [nao verificado]

## 21/11 02:51 · sessao 200517c5dfc8 · FALTA-APROVADA-MOVE-O-TXT-ENTREGUE · leitura · origem=Code

```python
"""
FALTA-APROVADA-MOVE-O-TXT-ENTREGUE — volta 3

A sonda foi escrita por leitura da cópia. A execução, o total da frota e a
identidade Git da cópia estão [nao verificado]. O anexo mostra que FALTA_DIA
retirou 3, 3 e 1 registros tipo 10 nas perguntas 33668, 33669 e 33686,
respectivamente, para dois PKs de colaborador. A rejeição devolveu as linhas.
O motivo do classificador nesses três casos está [nao verificado].
f5fin_6a1_main.porta:15–17.

UNIVERSO
Toda tripla de ExportacaoDominio confirmada por competencia_entregue, inclusive
quando tem zero casos. O último registro por (gerado_em, pk) decide a vigência.
Cada caso é um colaborador com ao menos uma FALTA aprovada que cruza a janela
da empresa, mesmo quando o fechamento está ausente ou tem outro status.
app/folha/export.py:47–51,71–92,556–594; app/folha/models.py:192–225;
app/chamados/services/materializacao.py:1095–1097.

PORTA B E CAMPOS
- Ausencia.status. criar_ausencia grava o estado inicial. A sonda usa
  ausencia.decidir(..., "rejeitada"), a mesma porta de serviço do cancelamento
  individual. A transição aprovada → rejeitada é permitida; mudar_estado
  atribui o status e chama Ausencia.save. A folha lê aprovadas em
  ausencias_na_janela. O cartório lê ausencia_cobre para cobrança, que exclui
  rejeitadas. app/ponto/views.py:85–89,2872–2890;
  app/ponto/services/ausencia.py:486,529,560–562,914–923,969–988,1057–1104;
  app/ponto/models.py:918; app/folha/export.py:71–92,169–185;
  app/ponto/turnos.py:2523–2532,2550–2571;
  app/ponto/services/cartorio.py:1382–1385.
- CelulaDia.veredito e protestos. O post_save de Ausencia chama o cartório
  sincronicamente para os dias da ausência. O cartório chama lavrar_veredito
  e carimbar. A porta da célula grava os campos e preserva o veredito de
  admin. O espelho lê veredito e protestos para decidir se retém.
  app/ponto/signals.py:85–93,115–128,143–147;
  app/ponto/services/cartorio.py:1321–1326,1898–1922,2073–2090;
  app/escala/models.py:1819–1828;
  app/ponto/portas/celula.py:957–965,1060–1098,1181–1202;
  app/folha/export.py:1247–1298,1301–1307.
- CelulaDia.ata. O cartório monta a ata e a porta a grava em lavrar_veredito;
  carimbar_precedencia também atualiza a chave precedencia. A exceção de
  pausa da folha lê a ata e o DNA via periodos_do_dia.
  app/ponto/services/cartorio.py:1898–1906,2034–2055;
  app/ponto/portas/celula.py:1049–1067,1090–1098,1205–1217;
  app/folha/export.py:1148–1155,1173,1293;
  app/ponto/juiz_batida.py:149–176.
- FechamentoMensal.status. O motor cria em aberto; aprovar e aprovar_lote
  escrevem aprovado; reabrir, reabrir_lote e reabrir_periodo escrevem aberto.
  A geração chama aprovar_lote. O montador filtra STATUS_NO_TXT. O
  classificador recebe também fechamentos de outros status. Sem fechamento,
  a lista do classificador fica vazia e a saída registra status e motivo nulos.
  app/ponto/models.py:398–410,481–487;
  app/ponto/services/fechamento.py:898–901,1323–1355,1449–1452;
  app/folha/views.py:248–254; app/folha/export.py:687–728,765–788.

O QUE A MEDIÇÃO COBRE
- A → B é o delta da contribuição do próprio colaborador à montagem pelo
  gravado, como no comparador da 6a1. A sonda chama classificar_export e passa
  seu resultado a montar_linhas_export. O status e o motivo são os do
  primeiro crivo que decidiu. Um crivo encoberto por outro fica
  [nao verificado]. A montagem é isolada por fechamento para atribuir
  linhas ao PK mesmo se houver código Domínio repetido. O delta usa
  multiconjuntos e sinaliza também mudança apenas na ordem.
  app/folha/export.py:654–658,690–746,765–788,866–884,946–981;
  app/colaboradores/models.py:311–314.
- O TXT arquivado, a importação no Domínio e a reprodução histórica das
  perguntas ficam [nao verificado]. A rota de geração usa fechamentos
  derivados em memória e valida o layout completo. Esta sonda mede pelo
  gravado, com o relógio da execução e o teto normal do cartório. Rejeitar
  uma falta que cruza a borda muda o status do registro inteiro dentro do
  savepoint. A devolução de perguntas e a reconciliação do consumidor são
  on_commit; seus efeitos ficam fora do ensaio com rollback.
  app/folha/models.py:174–187; app/folha/views.py:209–214,267–281;
  app/ponto/services/ausencia.py:969–988; app/ponto/signals.py:115–128,250–279;
  app/ponto/services/cartorio.py:2073–2090.
- A sonda conta exceções que chegam às etapas e falhas registradas no log
  ou no stderr durante uma exceção. Fallbacks internos sem registro ficam
  [nao verificado]. app/core/observ.py:73–91;
  app/ponto/services/cartorio.py:1954–1965,2094–2101;
  app/ponto/management/commands/detectar_ausencias.py:785–807.

EXECUÇÃO FUTURA
O corpo Python foi preparado para o shell tenant de bin/sombra.sh --rodar.
O lançador, antes de entrar no Python, lê a lavra de produção, grava arquivos
auxiliares e atualiza a sombra; também usa tee. Essas operações do lançador
ficam fora do rollback do Python. bin/sombra.sh:136–167,179–191,483–493.

Dentro do Python, o ensaio preserva os saves e signals e silencia os
transportes de push. Os INSERTs de auditoria e outbox podem consumir [nao verificado]
sequências. Por isso a sonda prepara RESTART transacional nas sequências de
public e juliani sob a transação externa, antes de escrever nos casos.
Permissões e comportamento das sequências na sombra instalada
[nao verificado]. app/chamados/push_utils.py:12–44,89–111;
app/chamados/fcm_utils.py:49–85,101–106;
app/chamados/services/disputa_emissao.py:1777–1798;
app/core/observ.py:133–138; app/ponto/portas/celula.py:945–965;
bin/sombra_db.sh:45.
"""

import io
import json
import logging
import socket
import sys
from collections import Counter, defaultdict
from contextlib import ExitStack, closing, redirect_stderr, redirect_stdout
from unittest.mock import patch

from django.conf import settings
from django.contrib.auth import get_user_model
from django.core.files.storage import FileSystemStorage
from django.db import connection, transaction
from django.db.models import Exists, OuterRef
from django.utils import timezone

from chamados import fcm_utils, push_utils [nao verificado]
from chamados.services.disputa_emissao import push_disputa_silenciado [nao verificado]
from chamados.services.materializacao import ESCAPES_QUE_VIRAM_AUSENCIA
from colaboradores.mixins import tem_acao
from colaboradores.models import Colaborador
from folha import export as E
from folha.models import ExportacaoDominio
from ponto.models import Ausencia, FechamentoMensal
from ponto.services.ausencia import decidir


REGISTROS = ("10", "11")


class GuardaSombra(RuntimeError):
    pass


class SemDecisorDP(RuntimeError):
    pass


class EstadoInesperado(RuntimeError):
    pass


class LayoutInesperado(RuntimeError):
    pass


class TransacaoMarcadaParaRollback(RuntimeError):
    pass


class EfeitoExternoBloqueado(RuntimeError):
    pass


class Descartar(io.TextIOBase):
    def __init__(self, falhas=None):
        super().__init__()
        self.falhas = falhas

    def write(self, texto):
        erro = sys.exc_info()[1]
        if self.falhas is not None and erro is not None:
            self.falhas.append(erro)
        return len(texto)


def contagem_erros(falhas):
    objetos = {id(erro): erro for erro in falhas if isinstance(erro, BaseException)}
    tipos = Counter(type(erro).__name__ for erro in objetos.values())
    tipos.update(erro for erro in falhas if isinstance(erro, str))
    return dict(tipos)


def etapa(falhas, acao, *args, desfazer=False):
    inicio = len(falhas)
    valor = None
    try:
        with transaction.atomic():
            try:
                valor = acao(*args)
                if transaction.get_rollback():
                    raise TransacaoMarcadaParaRollback()
            finally:
                if desfazer:
                    transaction.set_rollback(True)
    except Exception as erro:
        falhas.append(erro)
    erros = contagem_erros(falhas[inicio:])
    del falhas[inicio:]
    return valor, erros


def guardar_sombra():
    if not (
        settings.SETTINGS_MODULE == "config.settings.sombra"
        and getattr(settings, "HASNER_SOMBRA", False) is True
        and set(settings.DATABASES) == {"default"}
        and connection.vendor == "postgresql"
        and connection.settings_dict["NAME"] == "sombra"
        and connection.settings_dict["HOST"] == "sombra_db"
        and getattr(connection, "schema_name", None) == "juliani"
        and not connection.in_atomic_block
        and connection.get_autocommit()
    ):
        raise GuardaSombra()


def preparar_sequencias():
    with connection.cursor() as cursor:
        cursor.execute("""
            DO $sonda$
            DECLARE
                s record;
                qualificada text;
                ultimo bigint;
                usada boolean;
                proximo numeric;
            BEGIN
                FOR s IN
                    SELECT n.nspname, c.relname, p.seqcache, p.seqincrement,
                           p.seqmin, p.seqmax, p.seqcycle
                    FROM pg_catalog.pg_sequence p
                    JOIN pg_catalog.pg_class c ON c.oid = p.seqrelid
                    JOIN pg_catalog.pg_namespace n ON n.oid = c.relnamespace
                    WHERE n.nspname IN ('juliani', 'public')
                    ORDER BY n.nspname, c.relname
                LOOP
                    qualificada := format('%I.%I', s.nspname, s.relname);
                    EXECUTE format('ALTER SEQUENCE %s CACHE %s', qualificada, s.seqcache);
                    EXECUTE 'SELECT last_value, is_called FROM ' || qualificada
                        INTO ultimo, usada;
                    proximo := ultimo::numeric;
                    IF usada THEN
                        proximo := proximo + s.seqincrement;
                    END IF;
                    IF proximo NOT BETWEEN s.seqmin AND s.seqmax THEN
                        IF NOT s.seqcycle THEN
                            CONTINUE;
                        END IF;
                        proximo := CASE WHEN s.seqincrement > 0
                                       THEN s.seqmin ELSE s.seqmax END;
                    END IF;
                    EXECUTE format('ALTER SEQUENCE %s RESTART WITH %s', qualificada, proximo);
                END LOOP;
            END
            $sonda$;
        """)


def decisor_dp():
    usuarios = get_user_model().objects.filter(is_active=True).order_by("-is_superuser", "pk")
    with closing(usuarios.iterator(chunk_size=200)) as ativos:
        for usuario in ativos:
            if tem_acao(usuario, "editar_ausencias"):
                return usuario
    raise SemDecisorDP()


def preparar_b():
    dp = decisor_dp()
    preparar_sequencias()
    return dp


def listar_casos(empresa, mes, ano, tipo, estado):
    ini, fim = E.janela_competencia(empresa, mes, ano)
    falta_na_janela = E.ausencias_na_janela(
        OuterRef("pk"), tipo, ini, fim,
    ).filter(status=estado)
    candidatos = (
        Colaborador.objects.filter(empresa_id=empresa.pk)
        .filter(Exists(falta_na_janela))
        .order_by("pk")
        .values_list("pk", flat=True)
    )
    casos = []
    for colab_pk in candidatos:
        faltas = tuple(
            E.ausencias_na_janela(colab_pk, tipo, ini, fim)
            .filter(status=estado).order_by("pk").values_list("pk", flat=True)
        )
        if not faltas:
            raise EstadoInesperado()
        casos.append((colab_pk, faltas))
    return casos


def rejeitar_faltas(colab_pk, faltas, tipo, estado, dp):
    ausencias = list(
        Ausencia.objects.filter(pk__in=faltas, colaborador_id=colab_pk)
        .select_related("colaborador__empresa").order_by("pk")
    )
    if tuple(a.pk for a in ausencias) != faltas:
        raise EstadoInesperado()
    if any(a.tipo != tipo or a.status != estado for a in ausencias):
        raise EstadoInesperado()

    agora = timezone.now()
    for ausencia in ausencias:
        decidir(
            ausencia, "rejeitada", por=dp, decisor=dp, aprovada_em=agora,
            motivo="sonda de falta aprovada",
        )

    gravadas = list(
        Ausencia.objects.filter(pk__in=faltas).order_by("pk")
        .values_list("pk", "status")
    )
    if gravadas != [(pk, "rejeitada") for pk in faltas]:
        raise EstadoInesperado()


def fotografar(empresa, mes, ano, colab_pk, foto):
    fechs = list(
        FechamentoMensal.objects.filter(colaborador_id=colab_pk, mes=mes, ano=ano)
        .select_related("colaborador__empresa")
    )
    if len(fechs) > 1:
        raise EstadoInesperado()
    fech = fechs[0] if fechs else None
    foto["fechamento_pk"] = fech.pk if fech else None
    foto["fechamento_status"] = fech.status if fech else None

    itens = E.classificar_export(empresa, mes, ano, fechamentos=fechs)
    if [item["colaborador_id"] for item in itens] != [f.colaborador_id for f in fechs]:
        raise EstadoInesperado()
    item = itens[0] if itens else None
    foto["classificador"] = {
        "status": item["status"] if item else None,
        "motivo": item["motivo"] if item else None,
    }

    linhas, _ = E.montar_linhas_export(
        empresa, mes, ano, itens=itens, fechamentos=fechs,
    )
    matricula = item["codigo_dominio"].zfill(10) if item else None
    por_tipo = {reg: Counter() for reg in REGISTROS}
    pai = None
    for linha in linhas:
        partes = E.partes_da_linha(linha)
        if partes is None:
            raise LayoutInesperado()
        reg = partes["reg"]
        if reg == "10":
            pai = partes["matricula"]
            if pai != matricula:
                raise LayoutInesperado()
        elif reg != "11" or pai is None:
            raise LayoutInesperado()
        por_tipo[reg][linha] += 1
    return linhas, por_tipo


def fotografar_b(empresa, mes, ano, colab_pk, faltas, tipo, estado, dp, foto):
    rejeitar_faltas(colab_pk, faltas, tipo, estado, dp)
    return fotografar(empresa, mes, ano, colab_pk, foto)


def delta_txt(a, b):
    linhas_a, tipos_a = a
    linhas_b, tipos_b = b
    delta = {}
    for reg in REGISTROS:
        ca, cb = tipos_a[reg], tipos_b[reg]
        delta[reg] = {
            "A": sum(ca.values()),
            "B": sum(cb.values()),
            "adicionadas": sum((cb - ca).values()),
            "removidas": sum((ca - cb).values()),
        }
    mudou = linhas_a != linhas_b
    return mudou, mudou and tipos_a == tipos_b, delta


def medir_caso(falhas, comp, colab_pk, faltas, tipo, estado, dp, erros_preparo):
    empresa, mes, ano = comp
    caso = {
        "empresa_pk": empresa.pk, "mes": mes, "ano": ano,
        "colaborador_pk": colab_pk, "ausencia_pks": faltas,
        "A": {}, "B": {}, "delta_A_para_B": None,
        "mudou_txt": None, "so_ordem": None, "mudou_so_linhas_com_entra_nos_dois": None,
    }
    a, erros_a = etapa(
        falhas, fotografar, empresa, mes, ano, colab_pk, caso["A"], desfazer=True,
    )
    if erros_preparo:
        b, erros_b = None, erros_preparo
    else:
        b, erros_b = etapa(
            falhas, fotografar_b, empresa, mes, ano, colab_pk,
            faltas, tipo, estado, dp, caso["B"], desfazer=True,
        )
    caso["excecoes_por_etapa"] = {
        nome: erros for nome, erros in (("A", erros_a), ("B", erros_b)) if erros
    }
    if not caso["excecoes_por_etapa"]:
        try:
            mudou, so_ordem, delta = delta_txt(a, b)
            ambos_entram = all(
                caso[e]["classificador"]["status"] == "entra" for e in ("A", "B")
            )
            caso.update(
                delta_A_para_B=delta, mudou_txt=mudou, so_ordem=so_ordem,
                mudou_so_linhas_com_entra_nos_dois=mudou and ambos_entram,
            )
        except Exception as erro:
            caso["excecoes_por_etapa"]["delta"] = contagem_erros([erro])
    caso["status"] = "INCONCLUSIVO" if caso["excecoes_por_etapa"] else "CONCLUSIVO"
    return caso


def contar(casos):
    pks = sorted({caso["colaborador_pk"] for caso in casos})
    return {"casos": len(casos), "colaboradores": len(pks), "colaborador_pks": pks}


def placar(casos):
    conclusivos = [c for c in casos if c["status"] == "CONCLUSIVO"]
    grupos = defaultdict(list)
    for c in conclusivos:
        a, b = c["A"]["classificador"], c["B"]["classificador"]
        grupos[(a["status"], a["motivo"], b["status"], b["motivo"])].append(c)

    por_caminho = []
    for (sa, ma, sb, mb), grupo in grupos.items():
        por_caminho.append({
            "A": {"status": sa, "motivo": ma},
            "B": {"status": sb, "motivo": mb},
            "total": contar(grupo),
            "mudam_txt": contar([c for c in grupo if c["mudou_txt"]]),
            "mudam_so_linhas_com_entra_nos_dois": contar([
                c for c in grupo if c["mudou_so_linhas_com_entra_nos_dois"]
            ]),
        })
    return {
        "total": contar(casos),
        "conclusivos": contar(conclusivos),
        "inconclusivos": contar([c for c in casos if c["status"] == "INCONCLUSIVO"]),
        "mudam_txt": contar([c for c in conclusivos if c["mudou_txt"]]),
        "mudam_so_linhas_com_entra_nos_dois": contar([
            c for c in conclusivos if c["mudou_so_linhas_com_entra_nos_dois"]
        ]),
        "por_caminho": por_caminho,
    }


def censo(falhas, resultado):
    tipo, estado = ESCAPES_QUE_VIRAM_AUSENCIA["FALTA_DIA"]
    triplas = list(
        ExportacaoDominio.objects.order_by("empresa_id", "ano", "mes")
        .values_list("empresa_id", "mes", "ano").distinct()
    )
    entregues = []
    for empresa_pk, mes, ano in triplas:
        vigente, erros = etapa(
            falhas, E.competencia_entregue, empresa_pk, mes, ano, True,
        )
        if erros:
            resultado["erros_de_universo"].append({
                "empresa_pk": empresa_pk, "mes": mes, "ano": ano,
                "etapa": "competencia_entregue", "excecoes_por_tipo": erros,
            })
        elif vigente is not None:
            entregues.append((empresa_pk, mes, ano, vigente))

    competencias_percorridas = []
    chaves_listadas = []
    lotes = []
    for empresa_pk, mes, ano, vigente in entregues:
        chave = (empresa_pk, mes, ano)
        comp = {
            "empresa_pk": empresa_pk, "mes": mes, "ano": ano,
            "exportacao_pk": vigente.pk, "casos_listados": None, "casos_percorridos": 0,
        }
        resultado["competencias"].append(comp)
        competencias_percorridas.append(chave)

        def listar():
            empresa = vigente.empresa
            return empresa, listar_casos(empresa, mes, ano, tipo, estado)

        lote, erros = etapa(falhas, listar)
        if erros:
            comp.update(status="INCONCLUSIVO", excecoes_por_tipo=erros)
            resultado["erros_de_universo"].append({
                "empresa_pk": empresa_pk, "mes": mes, "ano": ano,
                "etapa": "listar_casos", "excecoes_por_tipo": erros,
            })
            continue
        empresa, casos = lote
        comp["casos_listados"] = len(casos)
        chaves_listadas.extend((*chave, colab_pk) for colab_pk, _ in casos)
        lotes.append((comp, (empresa, mes, ano), casos))

    dp, erros_preparo = etapa(falhas, preparar_b) if chaves_listadas else (None, {})
    for comp, contexto, casos in lotes:
        inconclusivos = 0
        for colab_pk, faltas in casos:
            caso = medir_caso(
                falhas, contexto, colab_pk, faltas, tipo, estado, dp, erros_preparo,
            )
            resultado["casos"].append(caso)
            comp["casos_percorridos"] += 1
            inconclusivos += int(caso["status"] == "INCONCLUSIVO")
        comp["status"] = "INCONCLUSIVO" if inconclusivos else "CONCLUSIVO"

    comps_listadas = [(e, m, a) for e, m, a, _ in entregues]
    chaves_percorridas = [
        (c["empresa_pk"], c["mes"], c["ano"], c["colaborador_pk"])
        for c in resultado["casos"]
    ]
    resultado["universo"] = {
        "triplas_de_exportacao": len(triplas),
        "competencias_listadas": l

[truncado em 20000 de 23704 caracteres]

## 21/11 02:04 · sessao 3ed4ecd09de6 · E4C-F5-CONSTROI · verificador · origem=Code

1. **Nenhum leitor da cópia depende da entrada removida ou do tamanho da tabela.** Os dois consumidores encontrados usam `fonte_mutada` para preparar texto para o AST. O helper busca a chave selecionada e, se receber o nome removido, falha no `assert nome in MUTANTES`. A bateria histórica exercitava esse nome e registrava **21 testes Django por execução**. Como o script da bateria escolhia os casos [nao verificado]. [Tabela e seletor](/home/ronald/codex_ws/9e90d9253eaab60cd7aadb023ecdc839f73498bb/app/chamados/tests/e4c_f5_voltaE_mutantes.py:80), [selo do par](/home/ronald/codex_ws/9e90d9253eaab60cd7aadb023ecdc839f73498bb/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:524), [catraca](/home/ronald/codex_ws/9e90d9253eaab60cd7aadb023ecdc839f73498bb/app/chamados/tests/test_e4c_f5_selo_catraca.py:339), `bateria_84e31.txt:20–21`.

2. **Sim, o selo ainda pega a renomeação real de `devolucao` para `devolvida`.** Ele lê `_LIMPAM_VALIDACAO` diretamente de `DecisaoPergunta` importada, monta `ATOS_QUE_LIMPAM` com as constantes dessa classe e obtém `CAMPO` de `PerguntaDisputa.validada_em.field.name`. Com as duas trocas do mutante no modelo importado, a linha 696 falha primeiro. O valor comparado na linha 697 também diverge, e `CAMPO` continua sendo `validada_em`. O seletor só altera o texto enviado ao AST, o que explica o verde histórico nesse modo. [Importação](/home/ronald/codex_ws/9e90d9253eaab60cd7aadb023ecdc839f73498bb/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:12), [asserções literais](/home/ronald/codex_ws/9e90d9253eaab60cd7aadb023ecdc839f73498bb/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:694), [seletor](/home/ronald/codex_ws/9e90d9253eaab60cd7aadb023ecdc839f73498bb/app/chamados/tests/e4c_f5_voltaE_mutantes.py:80), `test_e4c_f5_selo_par_ato_coluna.py:22,29,532`, `delta.diff:9–15`, `bateria_84e31.txt:20–21`.

   O delta preserva todas as asserções. Comparei em memória o helper anterior com o atual sobre os fontes da cópia. Cada um dos **11 mutantes restantes** ainda altera seu alvo e produz exatamente o mesmo texto nas duas versões. A execução Django no hash atual [nao verificado]. [Tabela e aplicação das trocas](/home/ronald/codex_ws/9e90d9253eaab60cd7aadb023ecdc839f73498bb/app/chamados/tests/e4c_f5_voltaE_mutantes.py:6), `delta.diff:1–18`.

3. **No delta fornecido, nenhum `.py` de produção ou teste fora dos selos mudou.** O único arquivo alterado é `app/chamados/tests/e4c_f5_voltaE_mutantes.py`. O blob da cópia bate com `7559ed5e6`, e a reinserção das sete linhas removidas em memória reproduz `86a84cddd`. A completude do delta contra o commit-base [nao verificado]. `delta.diff:1–18`.

4. **Não.** Na bateria anterior, os 11 mutantes que ficaram na tabela retornaram `rc=1` nos dois modos. `d_recusa_sem_coluna_no_lote` e `d_recusa_escrever_so_vazio` tiveram duas falhas no seletor e três no fonte. A única entrada verde em um modo e vermelha no outro foi a removida. `bateria_84e31.txt:2–25`. [nao verificado]

## 21/11 01:54 · sessao 520fcc297fb3 · FALTA-APROVADA-MOVE-O-TXT-ENTREGUE · leitura · origem=Code

### 1. Qual crivo tirou as linhas nos três casos?

**O crivo acionado em cada caso está [nao verificado].** A passagem por `FALTA_DIA` retirou do TXT recomposto 3 linhas tipo 10 na pergunta 33668, 3 na 33669 e 1 na 33686. São duas matrículas na amostra, dos colaboradores 247 e 121. A rejeição devolveu as linhas. A saída agrega o delta por tipo de registro. A permanência de outras linhas da mesma matrícula e o motivo da retirada ficam [nao verificado]. `f5fin_6a1_f5.out:14–16`, [app/folha/export.py:654](/home/ronald/codex_ws/eb1e49c1ec1055e6bf43d7f67df63b17054b474e/app/folha/export.py:654).

- **`furo_espelho` vem primeiro.** Se a leitura das células gravadas produzir um motivo de retenção, `classificar_export` devolve `fora / furo_espelho` e encerra a avaliação do colaborador. `app/folha/export.py:968–973,1247–1296,1301–1307`.
- **`nao_homologado` vem depois.** Se o espelho passar, uma falta na janela pode acionar `fora / nao_homologado` quando há rubrica ativa `falta_diurna`, o catálogo da empresa é preenchido e o código da rubrica está fora dele. Os dois motivos `fora` fazem o montador pular o fechamento inteiro. Se o colaborador continuar em `entra`, o bloco normal de falta acrescenta um tipo 10 e um tipo 11 por dia. `app/folha/export.py:215–221,716–744,974–979,984–1019`.

Para fechar os três casos, falta medir **antes da resposta, após a aprovação e após a rejeição** o `status`, `motivo` e detalhe de `classificar_export` por colaborador e competência, além das linhas por matrícula, rubrica e valor. Para atribuir o efeito aos dois caminhos, faltam também os dias retornados por `datas_falta`, a rubrica ativa e sua homologação, e as células da janela com `veredito`, `protestos` e `ata`. Se sair `furo_espelho`, a homologação precisa ser observada separadamente para saber se também barraria, pois o retorno do espelho vem antes. `f5fin_6a1_f5.out:14–16`, `app/folha/export.py:246–254,837–849,968–1019,1247–1298`.

### 2. A sonda da volta 1 enxerga o caminho do cartório?

**Não.** O `patch` de `medir_fechamento` omite PKs somente do retorno de `folha.export.ausencias_na_janela`. As duas montagens leem a mesma `CelulaDia` gravada. O caminho do cartório começa no `post_save` de `Ausencia`, usa `ausencia_cobre` e pode gravar `veredito`, `protestos` e `ata` que o espelho lerá depois. Se essa célula já retiver o colaborador, as duas montagens podem devolvê-lo fora antes mesmo de consultar a falta na homologação. `r.txt:59–67`, `app/ponto/signals.py:85–93,143–147`, `app/ponto/services/cartorio.py:1382–1385,1898–1922,2073–2089`, `app/folha/export.py:968–979,1247–1296`.

### 3. Qual é o universo e que consultas cobririam os dois caminhos?

**A fonte de competência entregue é `ExportacaoDominio`.** `competencia_entregue` consulta a tripla `(empresa, mês, ano)` e escolhe o registro mais recente por `(gerado_em, pk)`. A tripla conta como entregue se esse registro existe e tem `invalidada_em` nulo. Um último registro invalidado faz a função retornar `False` ou `None`, sem recuar a um TXT mais antigo. Holerite publicado entra na guarda distinta `competencia_fechada_para_celula`. [app/folha/export.py:580](/home/ronald/codex_ws/eb1e49c1ec1055e6bf43d7f67df63b17054b474e/app/folha/export.py:580), `app/folha/export.py:588–594,617–628`, `app/folha/models.py:192–225`.

- **Base comum.** Percorrer as triplas distintas de `ExportacaoDominio` confirmadas por `competencia_entregue`, incluindo as com zero casos. Para o TXT recomposto, cruzar `Empresa`, `Colaborador` e `FechamentoMensal` da tripla, com `aberto` ou `aprovado` na montagem, e aplicar a janela da empresa. A montagem também lê `EmpresaIntegracao`. `app/folha/export.py:23–33,47–51,580–594,687–725`, `app/ponto/janelas.py:42–57`.
- **Leitura direta.** Cruzar `Ausencia` aprovada que intersecta a janela com os tipos de desconto de `TipoAusencia` via catálogo vigente. Ler `RubricaDominio` ativa, com precedência da empresa sobre a global, e os códigos de `CatalogoDominio` da empresa. `app/folha/export.py:63–92,146–185,258–260,984–1019`, `app/ponto/catalogo/ausencias.py:240–252,267–281,331–333`.
- **Espelho e cartório.** Ler `CelulaDia` por colaborador e dia, com `veredito`, `protestos`, `ata`, `dna` e `trabalha`, e os vínculos de `EscalaColaborador` da janela. Para atribuir a retenção à falta, é preciso a célula de antes do ato ou uma reconstrução em memória dos dois estados. Essa reconstrução usa as consultas de cobertura de `Ausencia` para folha e cobrança, `Batida` apurável e primeira batida válida, escala e folgas, `Justificativa` aprovada e `ChamadoColaborador` com suas `PerguntaDisputa`. A comparação deve passar pelos crivos na ordem real, inclusive férias com batida e cadastro zero, que vêm antes do espelho. `app/folha/export.py:482–502,876–979,1148–1173,1231–1298`, `app/ponto/services/cartorio.py:131–147,1169–1234,1382–1387,1413–1433,1500–1511`, `app/ponto/turnos.py:2523–2571,2747–2770`, `app/escala/alimentacao.py:81–102`, `app/escala/utils.py:1185–1210`.

A contagem é por **colaborador e competência entregue**, com deduplicação por PK de colaborador no total da frota. Os três casos demonstram efeito em **dois colaboradores na amostra**. O total da frota está **[nao verificado]**. `r.txt:38`, `app/ponto/models.py:481–487`, `f5fin_6a1_f5.out:3,14–16`.

Conferi o código da cópia disponível e o corpo da sonda transcrito em `r.txt`. A identidade Git da cópia e a versão avulsa de `sonda_frota.py` ficaram [nao verificado].

## 21/11 01:48 · sessao 02771adbc27b · E4C-F5-CONSTROI · verificador · origem=Code

**Veredito da E4C-F5** — Os dois testes da Volta 3 passam em memória, os selos registram zero acusações na cópia sem mutação e quatro dos cinco mutantes novos ficam vermelhos. `d_devolucao_renomeada_no_modelo` fica verde quando acionado pelo seletor entregue. As asserções literais mordem quando o modelo importado recebe a renomeação. `test_e4c_f5_volta3_selos.py:16`, `test_e4c_f5_selo_par_ato_coluna.py:546,555,696`, `test_e4c_f5_selo_catraca.py:349`, `e4c_f5_voltaE_mutantes.py:87`. [nao verificado]

Executei os visitantes e as asserções AST reais em memória, com `unittest` e metadados extraídos do fonte do modelo. Suíte Django completa, Ruff completo, identidade do commit sem `.git`, execução em banco e ocorrências na frota [nao verificado]. [nao verificado]

## 1. Dois ERROR, Ruff e leitores do retorno

- **`test_RED_selo_encontra_escritor_sem_mencao_a_coluna`** volta a encontrar `nova_validacao` com `pareada=False`. `validacoes_gravadas` agora desempacota `(chamada, registro, campos, ato)`, usa `registro.chamada is chamada` para examinar a gravação no próprio escopo e chama `.get` no dicionário da terceira posição. Antes, chamava `.get` no `Registro`. `test_e4c_f5_volta3_selos.py:16,18,20,67,69,70`, `test_e4c_f5_selo_par_ato_coluna.py:328,349`.
- **`test_RED_validacao_iniciada_pelo_ato_tambem_grava_a_coluna`** usa o mesmo helper corrigido. Em memória, encontrou `_carimbar_validacao` e terminou com `violacoes=[]`. O escritor salva a coluna e registra `VALIDACAO` com o instante dessa coluna. `test_e4c_f5_volta3_selos.py:54,58,59`, `services/validacao.py:168,171,173`.
- **Ruff B020** — A primeira troca fica em `primeira_ancora` e `primeira_substituicao`. O `for` usa `ancora` e `substituicao`. Isso elimina as duas sobreposições apontadas no anexo. O contrato continua chamando `ruff check .`. `e4c_f5_voltaE_mutantes.py:92,95`, `core/tests/test_contract_esmeril.py:22`.
- A busca na cópia encontrou **zero leitores restantes com o formato antigo**. Os outros leitores de `_registros` usam `Registro` na segunda posição. `_censo_dos_atos_que_desfazem` rende `(nome, linha, atos, pareado)` e seus quatro consumidores seguem esse formato. `test_e4c_f5_volta3_selos.py:16`, `test_e4c_f5_selo_par_ato_coluna.py:460,507,521,551,593,633,661`.

## 2. Lote vazio

- **Reconhece** lista, tupla e dicionário literais sem elementos. A regra também aceita um `ast.Set` sem elementos, mas `set()` é uma chamada e escapa. Segue um nome até a última definição local anterior, salvo quando ela é `AugAssign`. Reconhece qualquer método chamado `.none()`, `.filter(pk__in=…)` com valor reconhecido como vazio e propaga o vazio pelo receptor de chamadas encadeadas, com exceção de `.union()`. Verifica o receptor escrito e `escrever_so`. `test_e4c_f5_selo_par_ato_coluna.py:31,96,128,234,245,250,257,278,293`.
- **Zero falsos vermelhos nos escritores atuais.** O censo devolveu cinco entradas pareadas. `reabrir` delega à ressurreição, `reabrir_para_correcao` usa `qs` sem `escrever_so`, e as duas devoluções usam `save`. Na ressurreição, `ids` pode ficar vazio depois da releitura sob trava. Nesse caso, o mesmo `ids` restringe o universo dos atos e o UPDATE, enquanto `alvo_lido` ainda serve às guardas. O visitante aceita essa forma real. `models.py:1517,1663,1669,1671,1705,1719,1721,3527,3536,4432`, `services/disputa_emissao.py:1664,1683`, `services/validacao.py:290,293,367,370`, `test_e4c_f5_selo_par_ato_coluna.py:280,555`.
- **Escapam vazios que mudam o resultado no escritor real.** Troquei somente o receptor em `models.py:1721` por `alvo_lido.filter(id__in=[])`, `alvo_lido.filter(pk__in=list())`, `alvo_lido.exclude(pk__in=ids)` ou `alvo_lido.none().union(alvo_lido.none())`. Os dois censos continuaram verdes nas quatro variantes. O terceiro caso exclui precisamente os PKs que o UPDATE volta a exigir. Também ficam fora da regra construtores como `tuple()` e `set()`, compreensões vazias, `filter(Q(pk__in=[]))` e seleções que só ficam vazias em execução. `models.py:1669,1671,1719,1721,3536,3537`, `test_e4c_f5_selo_par_ato_coluna.py:234,248,250,280,293`.

## 3. Literais de atos e de coluna

`test_atos_com_foto_limpam_validacao` compara `set(DecisaoPergunta._LIMPAM_VALIDACAO)` e `ATOS_QUE_LIMPAM` com `{'devolucao', 'recusa', 'reabertura'}`. Exige `ATOS_COM_FOTO` não vazio e contido nesse literal, e `CAMPO == 'validada_em'`. `CAMPO` vem do metadado do campo e `ATOS_QUE_LIMPAM` das constantes do modelo. O censo de atos também compara `vistos` com o conjunto literal. `test_e4c_f5_selo_par_ato_coluna.py:22,29,556,695,696,697,698,699,700`.

Com o **modelo importado renomeado**, a primeira quebra no teste de metadados é a linha **696**. O assert seguinte, na **697**, também diverge por construção. O teste do censo quebra na **556**. Com **apenas o seletor entregue**, todos passam. Ele fornece texto para o AST, enquanto a resolução de constantes e os asserts leem `DecisaoPergunta` já importada. Confirmei os dois cenários em memória. `e4c_f5_voltaE_mutantes.py:65,87,97`, `test_e4c_f5_selo_par_ato_coluna.py:12,208,211,532,556,696,697`. [nao verificado]

## 4. Catraca e aval 1945

Para listas ou tuplas conhecidas, `+` e `+=` ampliam a **sintaxe reconhecida** e preservam o critério do aval. A catraca procura o campo em `update_fields` e exige o mesmo campo em um `Attribute(Store)` cujo receptor tenha o mesmo `ast.dump`. O controle `test_PASSAM_escrita_foto_e_leitura_do_ato` passou em memória. Os sintéticos novos também passaram, inclusive os controles de campo e receptor diferentes. `test_e4c_f5_selo_catraca.py:100,117,200,225,304,308,320,402,427,438,440`. [nao verificado]

**Em execução, a aproximação pode acusar além da regra literal.** Um `campos += ['respondida_em']` seguido de `campos.remove('respondida_em')` antes do `save` ainda acusa, embora o campo já tenha saído da coleção. Confirmei esse caso em memória. A isenção sintática também conserva as lacunas anteriores. Um `Store` em ramo oposto, depois do `save` ou em anotação sem valor pode isentar. Uma atribuição por alias do receptor pode ficar fora da isenção. `test_e4c_f5_selo_catraca.py:184,200,214,225,304,308,320`.

**Sítios de produção acusados: zero no escopo da catraca.** Dos sete `save(update_fields=…)` que incluem esses campos na cópia, só o de `devolver_validacao_recusada` está na lista de funções varridas. Ele atribui `perg.validada_em` antes de salvar. Os outros seis estão nos sítios citados abaixo. `test_e4c_f5_selo_catraca.py:17,23,339,349`, `services/validacao.py:168,171,287,290,365,367`, `models.py:3369,3372,3400`, `services/disputa_emissao.py:1501,1512,1532,1712,1734`.

## 5. Âncoras e mordida

| Mutante | Âncora e efeito | Teste que acusa | [nao verificado]
|---|---|---|
| `d_recusa_escrever_so_vazio` | **1** em `services/disputa_emissao.py:1683`. Mantém o `registrar` de RECUSA/REABERTURA e deixa o UPDATE sem linhas quando há atos a gravar. `services/disputa_emissao.py:1664`, `models.py:3536`. | `test_atos_que_desfazem_gravam_a_coluna` acusa o sítio `disputa_emissao.py:1664`. `test_escritores_fecham_o_par` também falha. `test_e4c_f5_selo_par_ato_coluna.py:546,555`. |
| `d_ressuscitacao_queryset_none` | **1** em `models.py:1721`. Mantém o registro da REABERTURA e esvazia o queryset usado para escrever. `models.py:1719,3537`. | Os mesmos dois testes falham. O censo de atos acusa `models.py:1517` e `models.py:1719`. `test_e4c_f5_selo_par_ato_coluna.py:546,555`. | [nao verificado]
| `d_devolucao_renomeada_no_modelo` | **1 em cada troca**, `models.py:4205` e `models.py:4330`. Muda o código persistido para `devolvida`. Uma linha anterior com `devolucao` deixa de entrar no conjunto de limpeza em Python e SQL. `models.py:3603,4375`, `services/validacao.py:293,371`. | **Verde pelo seletor**. Com metadados mutados, falha em `test_atos_com_foto_limpam_validacao` e `test_atos_que_desfazem_gravam_a_coluna`. Há ainda um assert ORM preexistente sobre o valor gravado, cuja execução ficou [nao verificado]. `test_e4c_f5_selo_par_ato_coluna.py:556,696`, `test_e1_volta2.py:111`. |
| `c_recusa_update_fields_concat` | **1** em `services/validacao.py:940`. Inclui `respondida_em` no `save` do motivo, sem atribuição nesse receptor na função. O `save` passa a considerar esse campo na guarda do par. `e4c_f5_voltaE_mutantes.py:72`, `models.py:3732,3749,3759`. | `test_producao_sem_leituras_cruas` acusa `update_fields(respondida_em)` na linha mutada 940. `test_e4c_f5_selo_catraca.py:225,349`. |
| `c_recusa_update_fields_acrescimo` | **1** na mesma linha. Produz a mesma escrita extra via `+=`. `e4c_f5_voltaE_mutantes.py:77`, `services/validacao.py:934,940`. | O mesmo teste acusa o `save` na linha mutada 942. `test_e4c_f5_selo_catraca.py:200,225,349`. |

## 6. Alcance do delta

O `delta.diff` fornecido contém quatro `.py`, todos de `app/chamados/tests`, e os quatro blobs finais coincidem com a cópia. O quarto arquivo é um selo **já existente da Volta 3 da F5**, fora dos dois módulos centrais desta volta. Mudou o helper usado por dois testes existentes. As asserções desses dois testes e a do Ruff ficaram intactas. O anexo mostra zero alterações em testes alheios aos selos da obra e zero `.py` de produção. Completude contra o commit-base [nao verificado]. `e4c_f5_voltaE_mutantes.py:55,92`, `test_e4c_f5_selo_catraca.py:427`, `test_e4c_f5_selo_par_ato_coluna.py:548,639,694`, `test_e4c_f5_volta3_selos.py:12,54,69`, `core/tests/test_contract_esmeril.py:24`.

## 7. Achados anteriores ainda abertos

- **Par de alvo e valor.** O selo de atos ainda aceita presença da coluna como evidência e aproxima identidade pelo receptor e pelo PK. Filtros adicionais e o modelo da chamada `.update()` ficam fora dessa prova. A comparação de contexto ainda exige os mesmos nós `atomic`, `if` e `except`. Save integral, `bulk_update`, alias do objeto e transação só no chamador ou decorador continuam fora ou sujeitos a falso vermelho. **Falso verde em escritor real demonstrado** pelas variantes vazias de `models.py:1721`. `test_e4c_f5_selo_par_ato_coluna.py:86,92,148,167,275,299,514`, `models.py:3536`. [nao verificado]
- **Projeção de chamadas.** Continua de um salto no mesmo arquivo e exige `registrar` posicional e fora de `if`/`except` no auxiliar. A chamada da RECUSA, os aliases entre arquivos e `validar_pergunta → devolver_validacao_recusada` seguem sem entrada própria. Os gravadores alcançados por essas chamadas entram diretamente no censo. A migração histórica continua excluída. `test_e4c_f5_selo_par_ato_coluna.py:322,333,342,524`, `services/validacao.py:284,293,798,943`, `models.py:2405,2423`, `migrations/0051_decisao_pergunta.py:49`.
- **Catraca parcial.** Continuam escapando `append`/`extend`, `**kwargs`, coleções desconhecidas e certas mutações por alias. Confirmei que acrescentar `respondida_em` via `append` ou `**{'update_fields': ...}` no **`save` real da RECUSA** deixa o visitante verde. Também permanece a lista nominal de funções, com sítios reais fora dela. Um exemplo é `carimbar`, que já usa `append` para `respondida_em`. `test_e4c_f5_selo_catraca.py:17,87,100,200,214,339`, `services/validacao.py:940`, `models.py:3370,3400`.
- **Expectativa literal e relógio.** Os literais foram restaurados, mas a renomeação ainda escapa pelo seletor, como no item 3. A fragilidade dos dois asserts ORM permanece. O teste fixa `VALIDACAO_EM`, a reabertura usa `timezone.now()` e a leitura ordena por instante. Se a reabertura sair anterior à validação, a validação pode continuar vigente. É dependência do teste, distinta dos falsos verdes dos visitantes. `test_e4c_f5_selo_par_ato_coluna.py:696,756,774`, `e4c_f5_voltaE_mutantes.py:87`, `cortes_f2_f9_apoio.py:32`, `services/disputa_emissao.py:1648,1668`, `models.py:4363,4367,4375`.

## 8. Checklist de classes e fluxo dos valores

| Classe | Quem escreve e quem lê em produção |
|---|---|
| **Segundo juiz** | `validada_em` e `respondida_em` vigentes vêm de `DecisaoPergunta.leitura` e chegam aos juízes por `composta`. `pergunta_respondida` lê instante e texto, e o C6 usa `projecao_temporal_da_pergunta`. O vigia lê a presença no ato e na coluna para contar divergência. O delta cria zero juízes de produção. `test_e4c_f5_selo_catraca.py:13,225`, `models.py:4348,4382,4399`, `juizes.py:1391,1408,1428,1560`, `services/vigia_ato_x_coluna.py:16`. |
| **Segundo escritor** | Para `devolucao`, escrevem as duas portas de devolução. Para `recusa`, `recusar_pergunta` fornece o ato à porta de correção. Para `reabertura`, escrevem a porta de correção e a ressurreição. Todas entregam a linha a `registrar`, que faz o `bulk_create`. A projeção passa pelos `save` e pelo UPDATE da porta de lote. `test_e4c_f5_selo_par_ato_coluna.py:29,504`, `services/validacao.py:290,293,367,371,943`, `services/disputa_emissao.py:1597,1664,1683`, `models.py:1719,1721,3537,4438`. |
| **Relógio fora da borda** | Atribuem `ato_em` nas portas de validação, devolução, correção e ressurreição. A leitura ordena os atos, e o juiz temporal recebe `agora` do chamador. O AST usa linhas de fonte. A dependência de relógio observada está nos dois testes ORM do item 7. `services/validacao.py:168,173,293,371`, `services/disputa_emissao.py:1648,1668`, `models.py:1702,1714,4367`, `juizes.py:1394,1409`, `test_e4c_f5_selo_par_ato_coluna.py:521,756,774`. |
| **Derivação paralela** | `_LIMPAM_VALIDACAO` é declarado no modelo e lido tanto pelo redutor Python quanto pela face SQL. `ATOS_COM_FOTO` é declarado no modelo e lido pela guarda da porta de correção e pelo histórico de respostas. `pk`/`pk__in` ligam o ato ao alvo. A ressurreição fornece `escrever_so`, e a porta o lê como restrição adicional do UPDATE. `test_e4c_f5_selo_par_ato_coluna.py:248,257,268,695`, `models.py:1669,1671,3536,3603,3859,4217,4330,4375`, `services/disputa_emissao.py:1640`. |
| **Selo/RED sem mordida** | Quatro REDs mordem nos escritores reais listados no item 5. O quinto muda a declaração do valor persistido, mas a execução pelo seletor deixa os leitores de metadados com os valores originais. `e4c_f5_voltaE_mutantes.py:55,65,72,87`, `test_e4c_f5_selo_par_ato_coluna.py:211,532,555,696`, `test_e4c_f5_selo_catraca.py:342,349`. |
| **Campo com dois significados** | `PerguntaDisputa.validada_em` e `respondida_em` são colunas de projeção. `_carimbar_validacao`, as portas de resposta e `carimbar` escrevem os instantes. As devoluções e o lote limpam os campos pertinentes. A projeção é lida para o passivo, a guarda e a comparação do par. As fotos usam `*_original_em`, escritas nas portas dos atos com foto. `update_fields` é a seleção de colunas consumida por `save` e pela guarda do par. `recusa_motivo`, usado como âncora, é escrito pela RECUSA e lido pela guarda de devolução. `test_e4c_f5_selo_par_ato_coluna.py:22,700`, `test_e4c_f5_selo_catraca.py:225`, `models.py:2658,3369,3372,3732,3748,4225,4304,4399`, `services/disputa_emissao.py:1501,1665,1710,1732`, `services/validacao.py:168,273,287,365,934,940`. |

## 21/11 01:41 · sessao 36325ce6ae1e · E4C-F5-CONSTROI · construtor · origem=Code

**E4C-F5, volta 3 do item 1.** Os 20 testes focados passaram em memória. Os cinco mutantes novos passaram na base `99a74feaf` e falharam nas asserções esperadas com o delta. Nas fontes sem mutação, os dois censos do par e a catraca não acusaram sítio de produção. [Selo do par](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:535) · [Catraca](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_catraca.py:338) · [Tabela dos mutantes](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e4c_f5_voltaE_mutantes.py:55)

O delta ficou na cópia, na base `99a74feaf6544c643fd6bacefa5ee02ad5c7c05d`. A entrega pede commit, mas `REGRAS` proíbe expressamente `git commit` e escrita em `.git`. O hash continua sendo o da base. Django e Ruff seguem **[nao verificado]** nos comandos abaixo.

### Arquivos tocados

- [test_e4c_f5_volta3_selos.py](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_volta3_selos.py:16)
- [test_e4c_f5_selo_par_ato_coluna.py](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:234)
- [test_e4c_f5_selo_catraca.py](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_catraca.py:100)
- [e4c_f5_voltaE_mutantes.py](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e4c_f5_voltaE_mutantes.py:55)

### Achados (a)–(f)

- **(a) Procede.** Reproduzi os dois `AttributeError` com o leitor da base. O leitor agora usa o terceiro elemento para os campos e compara `registro.chamada` com a chamada enumerada para continuar julgando o gravador no próprio escopo. Os dois testes do anexo passaram. `v_ato_sem_coluna` falhou na asserção de violações e apontou `_carimbar_validacao`. [`test_e4c_f5_volta3_selos.py:16`](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_volta3_selos.py:16) · [`test_e4c_f5_volta3_selos.py:59`](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_volta3_selos.py:59) [nao verificado]
- **(b) Procede.** As duas variáveis do laço original também apareciam no iterável. Renomeei os nomes da primeira troca e os do laço. A verificação AST encontrou as duas colisões na base e zero no delta. A prova em memória `a → b → c` passou e os casos com âncora ausente ou duplicada falharam, inclusive na segunda troca. O comando de Ruff retornou `command not found`. A execução em memória do binário localizado no cache retornou `PermissionError`. Resultado do Ruff **[nao verificado]**. [`e4c_f5_voltaE_mutantes.py:92`](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e4c_f5_voltaE_mutantes.py:92) [nao verificado]
- **(c) Procede.** Reproduzi os dois falsos verdes da base. Os censos agora rejeitam seleção explicitamente vazia, incluindo `escrever_so=[]`, `escrever_so=()`, `.none()` e `filter(pk__in=[])`. `test_MORDE_lote_que_nao_alcanca_a_pergunta_do_ato` também conserva os controles `qs` e `escrever_so=None`. O mutante `d_recusa_escrever_so_vazio` acusa RECUSA e REABERTURA em `disputa_emissao.py:1664`, com escrita em `:1683`. `d_ressuscitacao_queryset_none` acusa REABERTURA em `models.py:1517` e `:1719`, com escrita em `:1721`. Ambos falham nos dois censos. Confirmei duas lacunas restantes em memória, um filtro adicional `alvo_lido.filter(pk=0)` no caminho que compara `escrever_so` e um UPDATE direto com `pk` correspondente seguido de `filter(pk__in=list())`. O visitante não resolve filtros dependentes dos dados nem coleções produzidas por chamadas arbitrárias. [`models.py:3536`](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:3536) · [`test_e4c_f5_selo_par_ato_coluna.py:234`](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:234) · [`test_e4c_f5_selo_par_ato_coluna.py:278`](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:278) · [`test_e4c_f5_selo_par_ato_coluna.py:639`](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:639)
- **(d) Procede.** As asserções voltaram a fixar `{'devolucao', 'recusa', 'reabertura'}` e `'validada_em'`. `d_devolucao_renomeada_no_modelo` altera `DEVOLUCAO` e `_LIMPAM_VALIDACAO` para `devolvida` na fonte em memória. `test_atos_com_foto_limpam_validacao` passou na base com essa alteração e agora falha na comparação literal. [`test_e4c_f5_selo_par_ato_coluna.py:556`](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:556) · [`test_e4c_f5_selo_par_ato_coluna.py:694`](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:694) · [`e4c_f5_voltaE_mutantes.py:65`](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e4c_f5_voltaE_mutantes.py:65) [nao verificado]
- **(e) Procede.** `+` e `+=` sobre coleções locais conhecidas agora alimentam a mesma regra de campo e receptor do aval 1945. A lista varrida é a mesma. O controle `test_PASSAM_escrita_foto_e_leitura_do_ato` passou, e o novo sintético exercita campo diferente, receptor diferente e atribuição correspondente. Os mutantes `c_recusa_update_fields_concat` e `c_recusa_update_fields_acrescimo` acusam `update_fields(respondida_em)` em `validacao.py:940` e na linha `:942` da fonte mutada, respectivamente. Confirmei que ainda escapam `append`, `extend`, `**kwargs` inclusive literal, soma com operando sem coleção conhecida e `+=` por outro nome que aponta para a mesma lista. A atribuição continua sendo julgada sintaticamente no corpo da função, inclusive se vier depois do `save` ou estiver em outro ramo. [`test_e4c_f5_selo_catraca.py:17`](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_catraca.py:17) · [`test_e4c_f5_selo_catraca.py:100`](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_catraca.py:100) · [`test_e4c_f5_selo_catraca.py:304`](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_catraca.py:304) · [`test_e4c_f5_selo_catraca.py:427`](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_catraca.py:427) [nao verificado]
- **(f) Procede por leitura do código.** Os asserts originais `:703` e `:721`, agora `:756` e `:774`, ficaram intactos. A validação tem data fixa e a reabertura usa `timezone.now()`. Se a reabertura for anterior à validação, a ordenação por instante pode processar a validação por último e mantê-la vigente. Execução ORM **[nao verificado]**. [`cortes_f2_f9_apoio.py:32`](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/cortes_f2_f9_apoio.py:32) · [`disputa_emissao.py:1648`](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/disputa_emissao.py:1648) · [`models.py:4367`](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:4367) · [`test_e4c_f5_selo_par_ato_coluna.py:756`](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:756)

### Inventário de leitores

As referências abaixo são relativas a `app/chamados/tests/`.

| Retorno | Leitores e adaptação |
|---|---|
| `_registros` | `test_e4c_f5_volta3_selos.py:16` foi adaptado. `_erro` em `test_e4c_f5_selo_par_ato_coluna.py:460` e `_censo_dos_atos_que_desfazem` em `:507` já consumiam `Registro`. |
| `_censo_dos_atos_que_desfazem` | `test_e4c_f5_selo_par_ato_coluna.py:551`, `:593` e `:633` já recebiam a mesma tupla de quatro elementos. O novo sintético usa o mesmo retorno em `:661`. |
| `_ato`, `_registros_locais` e `_mesma_pergunta` | `_registros` lê `_ato` em `test_e4c_f5_selo_par_ato_coluna.py:328`, `:346` e `:349`, e `_registros_locais` em `:327` e `:345`. `_erro` e o censo dos atos leem `_mesma_pergunta` em `:463` e `:518`. Nenhum desses leitores precisou de adaptação de retorno. |
| `fonte_mutada` e `_fontes_de_producao` | `fonte_mutada` continua devolvendo texto aos leitores em `test_e4c_f5_selo_par_ato_coluna.py:532` e `test_e4c_f5_selo_catraca.py:342`. `_fontes_de_producao` continua entregando `(arquivo, fonte)` em `test_e4c_f5_selo_par_ato_coluna.py:540`, `:550` e `test_e4c_f5_volta3_selos.py:43`. |
| Tabelas anteriores | `e4c_f5_voltaA_mutantes.py:24`, `e4c_f5_voltaB_mutantes.py:64`, `e4c_f5_voltaC_mutantes.py:31`, `e4c_f5_voltaD_mutantes.py:50`, `e4c_f5_volta2_mutantes.py:5` e `e4c_f5_volta3_mutantes.py:106` não consomem esses retornos. O mutante `v_ato_sem_coluna` continua passando texto por `substituir`, chamado em `test_e4c_f5_volta3_selos.py:52`. |

### Comandos

O comando Django focado parou em `ModuleNotFoundError: django.core`, antes dos testes. O comando Ruff usou o `ruff.toml` da cópia e parou antes do lint. **[nao verificado]** [`manage.py:11`](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/manage.py:11) · [`ruff.toml:4`](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/ruff.toml:4) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados.tests.test_e4c_f5_selo_par_ato_coluna.SeloParAtoColunaTest \
  chamados.tests.test_e4c_f5_selo_catraca.SeloCatracaTest \
  chamados.tests.test_e4c_f5_volta3_selos.SelosVolta3Test

ruff check --no-cache --config ruff.toml \
  chamados/tests/{test_e4c_f5_volta3_selos,test_e4c_f5_selo_par_ato_coluna,test_e4c_f5_selo_catraca,e4c_f5_voltaE_mutantes}.py
```

A execução em memória usou `unittest`, os visitantes da cópia e as declarações extraídas do modelo. Para a comparação com a base, carreguei os três módulos de teste de `git show 99a74feaf:app/<arquivo>` em memória. [`test_e4c_f5_selo_par_ato_coluna.py:495`](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:495) · [`test_e4c_f5_selo_catraca.py:332`](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_catraca.py:332)

```bash
f5() {
  PYTHONDONTWRITEBYTECODE=1 python3 - "$@" <<'PY'
import ast, sys, unittest
from pathlib import Path
from types import SimpleNamespace as NS
from chamados.tests.e4c_f5_voltaE_mutantes import fonte_mutada

arq = 'chamados/models.py'
arvore = ast.parse(fonte_mutada(arq, Path(arq).read_text()))
classes = {n.name: n for n in arvore.body if isinstance(n, ast.ClassDef)}
nomes = {'RESPOSTA', 'REABERTURA', 'ATOS_COM_FOTO', '_LIMPAM_VALIDACAO'}
nos = [n for n in classes['DecisaoPergunta'].body if isinstance(n, ast.Assign)
       and any(isinstance(a, ast.Name) and a.id in nomes for t in n.targets for a in ast.walk(t))]
dados = {}
exec(compile(ast.Module(body=nos, type_ignores=[]), arq, 'exec'), {}, dados)
campo = next(t.id for n in classes['PerguntaDisputa'].body if isinstance(n, ast.Assign)
             for t in n.targets if isinstance(t, ast.Name) and t.id == 'validada_em')
sys.modules.update({
    'django.test': NS(SimpleTestCase=unittest.TestCase),
    'chamados.models': NS(DecisaoPergunta=NS(**dados), PerguntaDisputa=NS(validada_em=NS(field=NS(name=campo)))),
    'chamados.services.disputa_emissao': NS(reabrir_para_correcao=None),
    'chamados.tests.cortes_f2_f9_apoio': NS(RESPOSTA_EM=None, VALIDACAO_EM=None, BaseCortes=object),
    'ponto.services.cartorio': NS(calado=None),
})
from chamados.tests.test_e4c_f5_selo_par_ato_coluna import SeloParAtoColunaTest
from chamados.tests.test_e4c_f5_selo_catraca import SeloCatracaTest
from chamados.tests.test_e4c_f5_volta3_selos import SelosVolta3Test

selecionados = sys.argv[1:]
c = (SeloParAtoColunaTest, SeloCatracaTest, SelosVolta3Test)
testes = [cls(t) for cls in c for t in unittest.defaultTestLoader.getTestCaseNames(cls) [nao verificado]
          if t in selecionados or (not selecionados and not t.startswith('test_carimbo_sintetico'))]
assert testes and (not selecionados or len(testes) == len(selecionados)) [nao verificado]
sys.exit(not unittest.TextTestRunner().run(unittest.TestSuite(testes)).wasSuccessful()) [nao verificado]
PY
}

f5

F5_VOLTA3_MUTANTE=v_ato_sem_coluna f5 test_RED_validacao_iniciada_pelo_ato_tambem_grava_a_coluna
F5_VOLTA3_MUTANTE=m20_s14_perde_literal f5 test_RED_s14_conserva_literal_do_cadastro_c12
F5_VOLTA_E_MUTANTE=d_recusa_escrever_so_vazio f5 test_atos_que_desfazem_gravam_a_coluna test_escritores_fecham_o_par
F5_VOLTA_E_MUTANTE=d_ressuscitacao_queryset_none f5 test_atos_que_desfazem_gravam_a_coluna test_escritores_fecham_o_par
F5_VOLTA_E_MUTANTE=d_devolucao_renomeada_no_modelo f5 test_atos_com_foto_limpam_validacao
F5_VOLTA_E_MUTANTE=c_recusa_update_fields_concat f5 test_producao_sem_leituras_cruas
F5_VOLTA_E_MUTANTE=c_recusa_update_fields_acrescimo f5 test_producao_sem_leituras_cruas

for m in limpa_sem_ato preenche_e_desfaz escritor_novo; do
  F5_VOLTA_E_MUTANTE="$m" f5 test_escritores_fecham_o_par
done
for m in d_paridade_devolucao_sem_coluna d_ressuscitacao_reabertura_sem_coluna d_recusa_sem_coluna_no_lote; do
  F5_VOLTA_E_MUTANTE="$m" f5 test_atos_que_desfazem_gravam_a_coluna test_escritores_fecham_o_par
done
F5_VOLTA_E_MUTANTE=c_recusa_update_fields_cru f5 test_producao_sem_leituras_cruas
```

## 20/11 20:07 · sessao dfc7ddd27c8d · E4C-F5-CONSTROI · verificador · origem=Code

Os dois selos passaram sobre a cópia sem mutação. Os quatro mutantes novos falharam nas asserções esperadas. Encontrei falsos verdes reproduzíveis nos dois selos. Executei os visitantes reais em memória, com os metadados extraídos do código do modelo e `unittest` no lugar de `SimpleTestCase`. A suíte Django, o banco de teste e o hash do commit [nao verificado]. A cópia veio sem `.git`. `test_e4c_f5_selo_par_ato_coluna.py:479–531`, `test_e4c_f5_selo_catraca.py:299–321`, `e4c_f5_voltaE_mutantes.py:58–69`. [nao verificado]

## A. Par ato–coluna

**1. Quais sítios o censo enumera?** Cinco entradas, todas com `pareado=True`. A primeira é uma chamada ao mesmo escritor da segunda. `test_e4c_f5_selo_par_ato_coluna.py:479–496`.

| Sítio enumerado | Função e ato | Gravação de `validada_em` |
|---|---|---|
| `app/chamados/models.py:1517` | `ChamadoColaborador.reabrir` · REABERTURA | Delega ao lote em `models.py:1721`; UPDATE em `models.py:3537–3539`. |
| `app/chamados/models.py:1719` | `ChamadoColaborador.ressuscitar_perguntas` · REABERTURA | Monta `_compensa` em `models.py:1713–1718`; passa `validada_em=None` em `models.py:1721`; UPDATE em `models.py:3537–3539`. |
| `app/chamados/services/disputa_emissao.py:1664` | `reabrir_para_correcao` · REABERTURA e RECUSA | Guarda em `disputa_emissao.py:1640–1641`, conjunto em `models.py:4217`, coluna em `disputa_emissao.py:1630`, lote em `disputa_emissao.py:1683` e UPDATE em `models.py:3537–3539`. |
| `app/chamados/services/validacao.py:293` | `devolver_validacao_recusada` · DEVOLUÇÃO | Atribuição em `validacao.py:287` e `save` em `validacao.py:290`. |
| `app/chamados/services/validacao.py:370` | `devolver_paridade_bug_par_103` · DEVOLUÇÃO | Atribuição em `validacao.py:365` e `save` em `validacao.py:367`. |

Os dois `save` repassam `update_fields` ao ORM em `app/chamados/models.py:3719–3735`.

**2. Há escritor fora do censo?** Na busca por `registrar`, nomes e literais dos atos, aliases de import, montagem de listas, construtores, operações ORM, relação reversa e nome SQL da tabela, não achei outro escritor operacional desses atos. O `bulk_create` genérico recebe as linhas dos chamadores em `models.py:4428–4438`. `linhas_da_projecao_sem_linha` só produz RESPOSTA e VALIDAÇÃO, em `models.py:4313–4323`. A migração histórica `0051_decisao_pergunta.py:41,49–62` atribui REABERTURA às linhas antigas via default. O censo exclui migrações em `test_e4c_f5_selo_par_ato_coluna.py:499–507`.

Há chamadas de produção sem entrada própria no censo. A RECUSA explícita em `validacao.py:943–944` e a chamada por alias em `models.py:2405,2423` chegam ao escritor de `disputa_emissao.py:1664`. Também ficam fora as entradas de `views.py:1432`, `acoes_disputa.py:106`, `materializacao.py:1213`, `curar_perguntas_em_limbo.py:70`, `views_arquivar.py:86` e `ponto/services/cartorio.py:1721`, além da cadeia de dois saltos `models.py:1085 → :1517 → :1719`. `validacao.py:798` delega dentro do mesmo arquivo, mas o `registrar` da função chamada está sob o `if` de `validacao.py:284`. A projeção o descarta por esse ramo. `test_e4c_f5_selo_par_ato_coluna.py:297–324`.

**3. O que conta como par?**

- `save(update_fields=…)` com o campo e o mesmo receptor ou seu `.pk`, `.update(…)` com o campo e `filter(pk=…)` sintaticamente correspondente, ou lote cujo vínculo com o PK o visitante consegue reconstruir. O teste novo verifica a presença da coluna, sem verificar se o valor a limpa. `test_e4c_f5_selo_par_ato_coluna.py:47–48,128–172,234–278,489–494`. [nao verificado]
- Exige conjuntos idênticos e não vazios de nós `with transaction.atomic()` e os mesmos lados dos mesmos `if` e os mesmos `except`. Um savepoint adicional, um `atomic` só no chamador ou no decorador e condições equivalentes em `if` separados podem dar falso vermelho. Laços e outros desvios de fluxo ficam fora da comparação de ramos. `test_e4c_f5_selo_par_ato_coluna.py:86–94,486–494`.
- Projeta um salto no mesmo arquivo. Resolve `self.aux()` e `cls.aux()` na classe léxica e procura um `registrar` direto, com argumento posicional e fora de `if`/`except`. O par tem de estar no corpo que contém esse `registrar`. Import com `as` é resolvido. `self.registrar`, `cls.registrar` e alias por atribuição escapam como chamadas diretas. `test_e4c_f5_selo_par_ato_coluna.py:291–324,479–494`, `test_e4c_f5_selo_chamada.py:38–57`.
- `save()` integral, `bulk_update`, receptor com alias e queryset filtrado antes e guardado em variável podem dar falso vermelho. No outro sentido, um `filter(pk=…)` coincidente pode bastar mesmo em outro modelo ou com filtros adicionais que eliminem a linha. Reproduzi o falso verde nos dois lotes reais em memória. Acrescentar `escrever_so=[]` em `disputa_emissao.py:1683` ou usar `alvo_lido.none()` em `models.py:1721` manteve verdes os dois censos do par, embora o UPDATE fique sem linhas. `test_e4c_f5_selo_par_ato_coluna.py:148–172,234–278,489–494`, `models.py:3536–3539`. [nao verificado]

**4. Âncoras e mordida dos mutantes.** Cada âncora ocorreu exatamente uma vez, inclusive nas trocas sucessivas do primeiro mutante. `e4c_f5_voltaE_mutantes.py:26–54,63–68`.

| Mutante | Âncora na cópia e efeito | Asserção que falha |
|---|---|---|
| `d_paridade_devolucao_sem_coluna` | `validacao.py:365–367` remove a atribuição e o campo do `save`. As outras três trocas estão em `validacao.py:332,359,379` e alteram a descrição e as fotos. | `assertTrue(pareado)`, com DEVOLUÇÃO em `validacao.py:369` na fonte mutada. |
| `d_ressuscitacao_reabertura_sem_coluna` | `models.py:1721` tira `validada_em=None` do lote. | `assertTrue(pareado)` nas duas entradas, `models.py:1517,1719`. |
| `d_recusa_sem_coluna_no_lote` | `disputa_emissao.py:1630` tira `validada_em` de `campos`, usado em `:1683`. Afeta RECUSA e REABERTURA. | `assertTrue(pareado)` em `disputa_emissao.py:1664`. |
| `c_recusa_update_fields_cru` | `validacao.py:940` acrescenta `respondida_em` ao `save`, sem atribuição. | `assertEqual(vermelhos, [])`, com `update_fields(respondida_em)` em `validacao.py:940`. |

Nos três mutantes do par também falha a asserção antiga de sítio ausente do censo de escritores. A mordida exigida pelo item 78 vem da asserção nova de pareamento. `test_e4c_f5_selo_par_ato_coluna.py:520,530`, `test_e4c_f5_selo_catraca.py:316`.

**5. Os sintéticos usam o visitante real?** Sim. Os dois novos casos do par chamam `_censo_dos_atos_que_desfazem`. O da catraca chama `_julgar`, que passa a função gerada a `_vermelhos` e a `Catraca.visit`. `test_e4c_f5_selo_par_ato_coluna.py:566–575,605–612`, `test_e4c_f5_selo_catraca.py:299–321,380–392`.

## B. Catraca

**6. Qual é a regra literal?** Para cada keyword explícita `update_fields`, o visitante calcula `CAMPOS & _textos(kw.value)` e acusa cada campo ausente de `campos_do_escritor` daquela chamada. `CAMPOS` contém os nomes exatos `respondida_em` e `validada_em`. A isenção exige um `ast.Attribute(Store)` em algum ponto do corpo da função com o mesmo `ast.dump` do receptor. O item 1 do aval 1945 coincide com essa regra sob leitura sintática de “atribui” e “mesmo receptor”. A implementação também aceita um `Store` posterior ao `save`, em ramo oposto ou numa anotação sem valor. Em sentido inverso, `setattr` e atribuição por alias do objeto ficam fora da isenção. A regra se aplica a qualquer chamada com a keyword, inclusive métodos diferentes de `save`. `respostas_2011_1945.txt:2` (anexo), `test_e4c_f5_selo_catraca.py:13,56–61,83–94,192–196,271–294`.

**7. O controle passa?** Sim, na execução em memória. O controle de `:360–369` da base é hoje `test_PASSAM_escrita_foto_e_leitura_do_ato`. `test_e4c_f5_selo_catraca.py:369–378`.

**8. Quais sítios de produção usam os campos em `update_fields`?** Encontrei sete. Todos atribuem ao mesmo receptor e campo. Só `devolver_validacao_recusada` entra na lista de funções da catraca. Passou, e a catraca não acusou nenhum sítio de produção na cópia. `test_e4c_f5_selo_catraca.py:17–28,306–316`. [nao verificado]

| `save` | Função | Atribuição no mesmo receptor |
|---|---|---|
| `models.py:3400` | `PerguntaDisputa.carimbar` | `self.respondida_em` em `:3369`, `self.validada_em` em `:3372`. |
| `disputa_emissao.py:1532` | `responder_pergunta_disputa_core` | `p.respondida_em` em `:1501`, `p.validada_em` em `:1512`. |
| `disputa_emissao.py:1712` | `responder_automaticamente` | `p.respondida_em` em `:1710`. |
| `disputa_emissao.py:1734` | `carimbar_resposta_pela_trilha` | `p.respondida_em` em `:1732`. |
| `validacao.py:171` | `_carimbar_validacao` | `perg.validada_em` em `:168`. |
| `validacao.py:290` | `devolver_validacao_recusada` | `perg.validada_em` em `:287`. |
| `validacao.py:367` | `devolver_paridade_bug_par_103` | `perg.validada_em` em `:365`. |

**9. Que formas escapam?** Lista, tupla, set, variável local ligada antes à coleção conhecida, expansão `*` dentro da coleção e alternativas de expressão condicional são lidos. Escapam `+`, acréscimos via `+=`, `append` ou `extend`, valores vindos de parâmetro, módulo ou chamada sem vínculo conhecido e `**kwargs`, inclusive dicionário literal. Na produção, isso oculta os dois campos em `models.py:3365–3374,3400`, a inclusão de `validada_em` em `disputa_emissao.py:1514,1532` e a concatenação de `validacao.py:367`. `test_e4c_f5_selo_catraca.py:83–94,141–174,192–196,288–294,308–315`.

## C. Geral

**10. O veredito varia com ambiente, relógio ou banco?** `F5_VOLTA_E_MUTANTE` muda a fonte entregue aos dois selos. Os censos AST leem fontes sob `app`, sem consulta de banco ou relógio e sem leitura explícita de arquivo fora da árvore. O arquivo do par também contém testes ORM preexistentes com banco de teste. Neles, uma reabertura carimbada antes de `VALIDACAO_EM` pode deixar a validação vigente e derrubar os asserts de `:703` e `:721`. A porta usa `timezone.now()` e a leitura ordena por instante e PK. `e4c_f5_voltaE_mutantes.py:5,58–69`, `test_e4c_f5_selo_chamada.py:9`, `test_e4c_f5_selo_catraca.py:12,306–315`, `test_e4c_f5_selo_par_ato_coluna.py:499–507,682–722`, `cortes_f2_f9_apoio.py:32,37–74`, `disputa_emissao.py:1648,1664–1668`, `models.py:4363–4379`. [nao verificado]

**11. O delta toca produção ou afrouxa asserções?** O delta anexado toca apenas os três `.py` sob `app/chamados/tests`. Os identificadores dos blobs da base reconstituída e dos arquivos finais coincidem com o anexo. A completude do delta frente ao commit-base [nao verificado]. Nenhuma expressão de asserção foi removida. Estas mudaram de alcance pelo dado que recebem ou pelo ponto onde são executadas. `delta.diff` (anexo), `test_e4c_f5_selo_par_ato_coluna.py:29,435–440,520–521,645–647`, `test_e4c_f5_selo_catraca.py:306–316`, `e4c_f5_voltaE_mutantes.py:63–68`. [nao verificado]

| Asserção existente | Antes | Depois |
|---|---|---|
| Igualdade em `test_e4c_f5_selo_par_ato_coluna.py:645` | `_LIMPAM_VALIDACAO` igual ao conjunto literal dos três atos. | Igual ao conjunto das três constantes do modelo. |
| Subconjunto em `test_e4c_f5_selo_par_ato_coluna.py:647` | `ATOS_COM_FOTO` contido no conjunto literal. | Contido no conjunto derivado do modelo. |
| Âncora em `e4c_f5_voltaE_mutantes.py:67` | `fonte.count(antes) == 1` uma vez por mutante. | A mesma checagem em cada troca, na fonte resultante da troca anterior. |
| `assertEqual(erros, [])` em `test_e4c_f5_selo_par_ato_coluna.py:521` | Pareamento anterior em `_erro`. | A mesma asserção, com a nova comparação de alvos em `:435–440`. |
| `assertEqual(vermelhos, [])` em `test_e4c_f5_selo_catraca.py:316` | Fonte direta e regras anteriores do visitante. | A mesma asserção, com fonte selecionável pelo mutante e a regra de `update_fields`. |

**12. Quais classes reaparecem?** Há mordida nos quatro mutantes, com lacunas demonstradas em A3 e B9. Os julgamentos usam AST. A busca textual está na aplicação das âncoras. A expectativa dos atos passou a derivar da produção, e o próprio nome da coluna vem do metadado do modelo. O delta não introduz campo de dupla semântica. A coluna de projeção, o valor vigente da composta e a foto histórica têm papéis explícitos. `e4c_f5_voltaE_mutantes.py:66–68`, `test_e4c_f5_selo_par_ato_coluna.py:22–29,208–214,479–494,645–647`, `test_e4c_f5_selo_catraca.py:192–196,299–321`, `models.py:2659,4225–4228,4382–4399`. [nao verificado]

## ACHADOS

- **Alta — O selo do par aceita lote vazio.** Reproduzi em memória a preservação do ato com zero perguntas no UPDATE e os dois censos verdes. A comparação pode aceitar a base sem validar `escrever_so`, ou aceitar `escrever_so` sem validar o queryset efetivamente escrito. `test_e4c_f5_selo_par_ato_coluna.py:251–260`, `disputa_emissao.py:1683`, `models.py:1721,3536–3539`.
- **Média — A catraca deixa passar formas de `update_fields` usadas em produção.** Concatenação, mutação de lista e expansão de kwargs passaram nos exemplos em memória. Também passaram a atribuição posterior e a de ramo oposto. Só um dos sete sítios atuais faz parte da lista varrida. `test_e4c_f5_selo_catraca.py:17–28,83–94,192–196,271–287`, `models.py:3365–3374,3400`, `disputa_emissao.py:1514,1532`, `validacao.py:367`.
- **Média — Duas asserções antigas perderam a expectativa literal.** Uma mudança coordenada em `DEVOLUCAO` e na entrada correspondente de `_LIMPAM_VALIDACAO` pode passar onde a igualdade antiga acusaria. `test_e4c_f5_selo_par_ato_coluna.py:29,645–647`, `models.py:4205,4330`.
- **Baixa — Dois asserts ORM preexistentes dependem da ordem entre a data fixa e o relógio.** A leitura pode manter a validação quando a reabertura sai com instante anterior. `test_e4c_f5_selo_par_ato_coluna.py:690–705,713–722`, `cortes_f2_f9_apoio.py:32`, `disputa_emissao.py:1648,1664–1668`, `models.py:4363–4379`.

## 20/11 20:05 · sessao 04a20458663e · - · - · origem=Ronald (tmux)

**C10.** O seletor `F5_VOLTA_E_MUTANTE` muda o texto entregue ao AST. O par já o consultava e a catraca passou a consultá-lo. Os censos leem fontes sob `app`; a referência a `timezone.now` no visitante do par é um reconhecimento sintático. Fontes: `e4c_f5_voltaE_mutantes.py:5,58–69`, `test_e4c_f5_selo_par_ato_coluna.py:345–351,499–507`, `test_e4c_f5_selo_chamada.py:9`, `test_e4c_f5_selo_catraca.py:12,306–316`. [nao verificado]

O arquivo do par também contém testes ORM preexistentes, com banco de teste. Dois vereditos dependem indiretamente do relógio: a porta carimba a reabertura com `timezone.now()`, e a leitura ordena por `ato_em` e PK. Em memória, executei a leitura real com a fixture. Uma reabertura às 09:00 UTC de 18/10 deixa vigente a validação das 10:00 UTC. Às 10:00 com PK posterior ou às 11:00, ela a zera. Fontes: `cortes_f2_f9_apoio.py:32,37–74`, `test_e4c_f5_selo_par_ato_coluna.py:687–705,713–722`, `disputa_emissao.py:1648,1664–1668`, `models.py:4363–4379`. [nao verificado]

**C11.** O delta fornecido altera três arquivos sob `app/chamados/tests`. Reconstituí os três blobs da base em memória. Os IDs da base e do final coincidem com o anexo. Identidade do commit e completude do delta contra a base fora do anexo [nao verificado], pois a cópia não contém `.git`. [nao verificado]

- Nenhuma expressão `assert` ou `assert*` existente foi removida ou substituída. A asserção de unicidade da âncora, antes única em `e4c_f5_voltaE_mutantes.py:37` da base, agora é executada por substituição em `e4c_f5_voltaE_mutantes.py:66–68`.
- As duas asserções existentes que usam `ATOS_QUE_LIMPAM` perderam a expectativa literal. Antes, igualdade e subconjunto contra `{'devolucao', 'recusa', 'reabertura'}`. Agora, contra `{DecisaoPergunta.DEVOLUCAO, DecisaoPergunta.RECUSA, DecisaoPergunta.REABERTURA}`. Base `test_e4c_f5_selo_par_ato_coluna.py:29,498,500`, final `test_e4c_f5_selo_par_ato_coluna.py:29,645,647`.
- `assertEqual(erros, [])` e `assertEqual(vermelhos, [])` mantêm as expressões. Mudaram o pareamento dos registros e a preparação da fonte, respectivamente. Par `:465` na base, `:435–440,521` no final. Catraca `:307` na base, `:306–316` no final.

**C12.** Os julgamentos usam AST. A substituição textual aplica os mutantes antes de analisar a fonte. O oráculo dos atos passou a acompanhar o valor de produção. Renomear `DEVOLUCAO` e a entrada correspondente em `_LIMPAM_VALIDACAO` mantém verdes as duas asserções citadas. O nome da coluna vem do modelo, mas o acesso nominal e a fixture do censo ainda fixam `validada_em`. Fontes: `e4c_f5_voltaE_mutantes.py:66–68`, `test_e4c_f5_selo_catraca.py:193–196,299–315`, `test_e4c_f5_selo_par_ato_coluna.py:22,29,208–214,470–494,624–633,645–647`, `models.py:4205–4206,4330`. [nao verificado]

O delta não introduz campo de dupla semântica. A coluna da projeção e o valor vigente da composta são comparados explicitamente. A foto histórica e o sinal de manutenção da validação têm campos próprios. Fontes: `models.py:2658–2659,4225–4228,4241–4248,4382–4399`.

## 20/11 20:05 · sessao d92fd4adbc79 · - · - · origem=Ronald (tmux)

A1. O visitante real, executado em memória sobre as 829 fontes de `_fontes_de_producao`, produziu cinco entradas pareadas. A regra de inclusão está em `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:479–507`.

| Entrada | Ato | Gravação |
|---|---|---|
| `app/chamados/models.py:1517` `ChamadoColaborador.reabrir` | REABERTURA | Delega a `ressuscitar_perguntas`, com chamada do lote em `models.py:1721` e UPDATE físico em `models.py:3537–3539`. |
| `app/chamados/models.py:1719` `ChamadoColaborador.ressuscitar_perguntas` | REABERTURA | Lista `_compensa` em `models.py:1713–1718`, lote em `models.py:1721` e UPDATE em `models.py:3537–3539`. |
| `app/chamados/services/disputa_emissao.py:1664` `reabrir_para_correcao` | REABERTURA, RECUSA | Guarda em `disputa_emissao.py:1640` e conjunto em `models.py:4217`. Campos em `disputa_emissao.py:1630`, lote em `disputa_emissao.py:1683` e UPDATE em `models.py:3537–3539`. |
| `app/chamados/services/validacao.py:293` `devolver_validacao_recusada` | DEVOLUÇÃO | Atribuição em `validacao.py:287` e save em `validacao.py:290`. |
| `app/chamados/services/validacao.py:370` `devolver_paridade_bug_par_103` | DEVOLUÇÃO | Atribuição em `validacao.py:365` e save em `validacao.py:367`. |

A2. Procurei chamadas `.registrar`, nomes e literais dos atos, aliases de import, construtores, ORM e relação reversa, referências à tabela, montagem das listas e chamadas aos helpers. O único INSERT localizado é o `bulk_create` genérico de `DecisaoPergunta.registrar` em `app/chamados/models.py:4427–4438`. `linhas_da_projecao_sem_linha` só fornece RESPOSTA e VALIDAÇÃO em `models.py:4313–4323`. Há delegadores fora da lista: RECUSA explícita em `app/chamados/services/validacao.py:943–944`, REABERTURA em `models.py:2423`, `app/chamados/views.py:1432`, `app/chamados/services/acoes_disputa.py:106`, `app/chamados/services/materializacao.py:1213` e `app/chamados/management/commands/curar_perguntas_em_limbo.py:70`; ressuscitação em `app/chamados/views_arquivar.py:86`, `app/ponto/services/cartorio.py:1721` e dois saltos em `models.py:1085` → `:1517` → `:1719`. A migração histórica excluída do censo rotula as linhas antigas como REABERTURA via default em `app/chamados/migrations/0051_decisao_pergunta.py:41,49–62`.

A3. O selo aceita `save(update_fields=…)`, `update(…)` e `PerguntaDisputa.descarimbar_em_lote(…)`. Exige um conjunto não vazio e idêntico de nós `with transaction.atomic()` e os mesmos lados de `if` e mesmos `except` no escopo que contém `registrar`, além da igualdade sintática de receptor ou PK ou ligação a lote. `save()` integral, `bulk_update`, variável com queryset filtrado antes, alias de receptor, atomic de decorador/chamador e condições equivalentes escritas em `if` distintos podem acusar um par válido. `update` aceita qualquer receptor com `.filter(pk=…)` coincidente, e o teste novo testa presença do campo sem verificar o valor. A projeção usa um salto no mesmo arquivo, resolve `self.aux` e `cls.aux` para a classe léxica e só projeta `registrar` interno com posição e fora de `if`/`except`. `self.registrar`, `cls.registrar` e alias local por atribuição escapam. A resolução de import com `as` funciona. `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:47–48,86–94,134–172,234–278,291–324,479–495`; `app/chamados/tests/test_e4c_f5_selo_chamada.py:38–57`.

**Achado A3, alto.** Executei variantes em memória nas quais os atos permanecem e o lote fica vazio. Acrescentar `escrever_so=[]` em `app/chamados/services/disputa_emissao.py:1683` ou trocar `alvo_lido` por `alvo_lido.none()` em `app/chamados/models.py:1721` preserva `pareado=True` e a lista vazia de erros no censo antigo. O UPDATE efetivo é limitado pelo queryset e por `escrever_so` em `models.py:3536–3539`, mas o selo aceita coincidência da base ignorando `escrever_so` ou coincidência de `escrever_so` ignorando o queryset em `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:251–260`.

## 20/11 20:04 · sessao d8ff15d33f72 · CADEIA-20-PASSO-1 · leitura · origem=Code

# PAREI RASCUNHO — CADEIA-20-PASSO-1

O Desenho A prevê proteção para **ao menos um caminho de 16 dos 64 sítios**. A cobertura integral desses sítios, a identidade Git da cópia, o pouso da F5 e os resultados dos REDs ficam **[nao verificado]**. As linhas de código abaixo são da cópia lida. As linhas `r.txt:574–587` são da ficha 1545 anexada. `r.txt:574–587` `app/ponto/services/cartorio.py:959–1027` `app/chamados/models.py:3674–3683`

## A. Fatias por escritor

Nas três fatias, o dia é resolvido pela janela da empresa. O cadastro da empresa escreve `dia_inicio_competencia`, `ExportacaoDominio.registrar` e `invalidar` escrevem a entrega e sua invalidação, e `janela_atual` e `_K6` leem esses fatos. O retorno de `_K6` é leitura de TXT vigente. A tranca que o E2 já consulta vem de `_K7`. `app/colaboradores/views.py:515–557` `app/ponto/janelas.py:36–63` `app/folha/models.py:143–189` `app/folha/export.py:556–594` `app/chamados/services/vigia_fio.py:786–808` `app/core/juizes.py:342–343`

### 1. Cartório + relógio

**A1 e A5 — Sítios e arquivos.** **05**, nas duas lavras do corpo, e **19**, na travessia de estado que passa pelo relógio A1. A estimativa de produção é `app/ponto/services/cartorio.py`, `app/chamados/juizes.py` e o `pre_save` existente do chamado em `app/chamados/signals.py`. `app/chamados/services/fecho_runtime.py` entra se hospedar o envelope de A1. Os arquivos de alteração na **F5** são **cartório e juízes**. `models.py` é porta e leitor atingido, também na F5. A ficha manda parar se o encaixe exigir mudar o modelo ou a cláusula. A lista exata de arquivos de teste do rol V3/COBERTURA‑V1 fica [nao verificado]. `r.txt:580,583–587` `app/ponto/services/cartorio.py:1034,1343,1921` `app/chamados/models.py:216–234,302,343–384` `app/chamados/signals.py:177–186` `app/chamados/services/fecho_runtime.py:146–169` `arquivos_da_f5.txt:3,14,115`

**A2 — Unidade.** Para **05**, a unidade é a lavra de uma **célula-dia**, do exame de `_K6` antes da primeira mutação dessa célula até a porta que salva os campos e publica `EventoCelula` no mesmo `atomic`. A janela de batidas e o pareamento continuam inteiros. Dentro da unidade, colher o C6 inicial e o C6 sobre a própria célula referenciada pela pergunta, com **a ata e o par que a porta efetivamente gravaria**. A segunda passada ou a releitura existente consome a divergência na etapa em que ela fica observável. A saída da última leitura é o ponto de `PAREI` para resíduo. Para **19**, o contexto temporal abre antes de `ato()` no fecho do chamado do cartório, cobre gate e tripwire e termina para a mesma instância no `pre_save`, com restauração também em recusa ou exceção. O `atomic` da Pauta de fecho tem vida própria. `r.txt:580,583–585` `app/ponto/services/cartorio.py:1016–1049,1101–1126,1176–1204,1901–1922` `app/ponto/portas/celula.py:957–1006,1181–1202` `app/chamados/services/fecho_runtime.py:146–169` `app/chamados/models.py:216–234,361–384` `app/chamados/signals.py:180–186`

**A3 — RED de primeira lavra e controles do relógio.**

- **Primeira lavra.** Colaborador ativo, vínculo 5×2, DNA 08/12/13/17, E/S de adesão uma semana antes e zero batidas no alvo aberto. Semear `concorde` com ata sem pendências, uma cobrança viva que protesta e sua pergunta ancorada sem carimbo nem validação. Entrar pelo cron `processar_cartorio --apply`. Observar, no mesmo PK e ato, as cinco presenças consumidas de verdade, C6 **0 → 1** no par antes/lavra, a saída coerente e `emitidos` sem aumento. Na árvore atual o fio é lido antes da ata e do par novos, a pergunta sem materialização fica fora da ressurreição, e a releitura pós-fábrica só abre por aumento de `emitidos`. Em outro braço, manter uma célula entregue na margem de um turno que cruza a meia-noite. Exigir a foto persistida da entregue intacta e a célula aberta julgada com o pareamento íntegro. Mutantes de falso verde **só corrigir a foto final em outro cron**, ou **retirar os dias entregues da janela de leitura** quando o teste só compara a foto entregue. `r.txt:580,585` `app/ponto/tests/test_o261_cobrado_le_o_fio.py:243–292` `app/config/crons.py:616` `app/ponto/management/commands/processar_cartorio.py:38–65` `app/ponto/services/cartorio.py:1045–1049,1101–1114,1413–1438,1493–1547,1901–1922` `app/chamados/models.py:1542–1547` `app/chamados/juizes.py:1443–1469`
- **A1.** Entrar pela porta `julgar_celula(..., agora=10:00)` com ambiente às 10:02. Preparar uma cobrança que o cartório tenha motivo real para fechar, uma pergunta de fato ancorada em competência aberta, elegível no SQL, com célula acusatória e carimbo de identidade às 10:01, sem destino C8 válido. Esperar recusa pela cláusula existente. Num segundo caso, elegível e com fecho permitido, observar a chamada real ao C6 com 10:00 **no gate e no tripwire**, e sem esse contexto nas leituras de persistência após `pre_save`. A recusa do primeiro caso para antes do tripwire. Hoje os dois pontos chamam C6 sem o instante do ato. Validação futura é controle negativo da seleção, pois o SQL pode tirá-la antes do C6. Mutantes de falso verde **dar o instante só ao gate**, **deixá-lo vazar pelo `save`**, ou **usar o mesmo relógio nos dois lados**. O alcance ponta a ponta dessas fixtures fica [nao verificado]. `r.txt:583–585` `app/ponto/services/cartorio.py:1015,1034,1569–1581,2059–2089` `app/chamados/models.py:216–234,343–407,422–449,3539–3566` `app/chamados/juizes.py:866–898,1382–1400,1443–1457` `app/chamados/signals.py:180–186`

**A4 — Pouso próprio.** As assinaturas de entrada do E2, da fábrica e do repasse já existem. A fatia pode ser montada sem as outras duas, preservando essas chamadas e seus retornos. Sozinha, ela não fecha as escritas cruzadas dos escritores legados. O aceite de A1 e a compatibilidade com a correção de dia entregue permanecem condicionados aos pontos de D e E. `app/ponto/services/cartorio.py:1019–1049,1061–1114` `app/chamados/juizes.py:1510–1519` `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:94–131` `r.txt:580–587`

**A6 — Escritores e leitores.**

| Campo ou valor | Quem escreve e quem lê |
|---|---|
| Janela, `agora`, `turno_encerrado` e ata candidata | Cron ou porta unitária fornece `agora`. Grade e juiz de turno calculam o encaixe e encerramento. O corpo monta a ata e lê a janela, o encerramento e os marcos no cálculo da impressão e da classificação. C6 lê a célula referenciada e a ata/par projetados no ato. `app/ponto/management/commands/processar_cartorio.py:38–65` `app/ponto/services/cartorio.py:1011–1016,1176–1204,1425–1433,1464–1469,1493–1547,2088–2090` `app/chamados/juizes.py:415–429,1453–1469` |
| Total, vivo, protesta, fio e fio no mesmo protestante | O corpo deriva os cinco de `lista`, `status_local`, marcos e C6. `classificar_dia` recebe as contagens. A impressão recebe o fio e a escolha de `cobrado` usa a interseção entre fio e protesto. O corpo põe `cods` e `rejulgar` em `out`, e o orquestrador consome essas chaves e `emitidos`. `app/ponto/services/cartorio.py:1019–1075,1101–1117,1413–1433,1493–1539,1694–1721,2021–2031` |
| `ata`, `veredito`, `veredito_via`, `veredito_em`, `protestos`, `status_dia`, `impressao`, `julgada_em`, `insumos_em`, `ausencia`, `tem_entrada`, `tem_saida` | A porta da célula prepara a lavra e o carimbo e salva juntos. A via admin participa da precedência. O próximo corpo lê o carimbo para o pulo, C6 e o E2 leem a palavra da célula, e fábrica e lastro leem os marcos da ata. `app/ponto/portas/celula.py:970–987,1042–1102,1181–1202` `app/ponto/services/cartorio.py:1438–1444,1901–1922` `app/chamados/juizes.py:1453–1469` `app/chamados/reconciliador.py:675–722` `app/chamados/services/fabrica_da_celula.py:119–148` `app/chamados/reconciliador.py:1430–1438` |
| Estado do chamado e horizonte do fecho | `_gravar_estado` escreve `status_local`, `resolvido_em`, via e demais campos da transição. Gate e tripwire leem estado e ancoradas no pós-estado em memória. C6 lê validação, resposta, materialização e via no instante fornecido. `pre_save` lê o estado anterior e os receivers e a relavra leem após a escrita. `app/chamados/models.py:209–257,279–319,343–384` `app/chamados/juizes.py:1382–1406` `app/chamados/signals.py:91–117,180–218` |

### 2. E2 + fábrica

**A1 e A5 — Sítios e arquivos.** **37** no carimbo E2, **26** no fecho da disputa e **19** no efeito sobre o pai, com os efeitos síncronos desse fecho no mesmo perímetro. **39** nos descarimbos da fábrica, inclusive o que precede a saída do limbo. **35, 36, 42 e 43** são também alcançáveis pelo nascimento/reuso da fábrica. A ficha detalha a proteção de **39**, mas a cerca de nascimento da fábrica, especialmente o **36**, fica [nao verificado]. Alterações propostas em `app/chamados/reconciliador.py` e `app/chamados/services/fabrica_da_celula.py`. O **reconciliador** está na F5. `models.py` e `fio.py` estão na F5 e são portas chamadas. `r.txt:581–582,585–587` `app/chamados/reconciliador.py:634–667` `app/chamados/services/fabrica_da_celula.py:160–174,206–231` `app/chamados/signals.py:19–46,91–117,149–165` `app/chamados/models.py:3195–3211,3348,3408` `arquivos_da_f5.txt:14–15,25`

**A2 — Unidades.** No E2, uma **candidata** por unidade, com `_K6` pelo dia dela antes de gravar, trava e releitura da porta, término no sucesso ou na recusa desse carimbo. Após o laço, cada **fecho de disputa** tem unidade própria, incluindo o pai e os receivers síncronos que o ato aciona. Na fábrica, a verificação antecede a porta de cada descarimbo atingido, inclusive os dois pontos que chamam `desfazer_carimbo_sem_lastro`. A porta já trava e relê a pergunta e a disputa. O veto de um carimbo entregue precisa deixar disponível a saída existente de Pauta DP do limbo, quando aplicável. A extensão ao nascimento e ao contêiner entra nas decisões de E. `r.txt:581–582` `app/chamados/reconciliador.py:634–667` `app/chamados/services/fio.py:1389–1459` `app/chamados/services/fabrica_da_celula.py:160–174,206–231` `app/chamados/services/relavra_via.py:215–261` `app/chamados/signals.py:19–46,91–117,149–165`

**A3 — RED de veto por candidata com ato aberto sobrevivente.**

- **E2.** No mesmo colaborador ativo com vínculo e adesão, ter um dia aberto com chamado e pergunta vivos, saída já registrada e entrada faltante, e outra pergunta elegível em célula absolutória de dia entregue e sem tranca. Registrar pela porta real a entrada que completa os marcos do dia aberto. O molde existente usa `registrar_batida` e comprova o percurso até o núcleo de fecho. Observar a sequência **lavra → carimbo aberto → tentativa de fecho adiado**, o chamado aberto chegando ao estado terminal e a pergunta entregue conservando os três campos. Variar a ordem dos PKs e incluir fecho de disputa/pai com alcance misto, observando também S143/S144. Hoje o E2 varre o colaborador, filtra por C6 e `_K7` e pode carimbar a candidata entregue sem perguntar a `_K6`. Mutante de falso verde **vetar todo o colaborador** quando o teste só olha a entregue, ou aceitar um fecho produzido depois pelo segundo receiver de batida como se fosse o fecho adiado. `app/chamados/tests/test_o261e_f1_porta_encerramento.py:384–446` `app/ponto/signals.py:131–135,206–215` `app/ponto/services/cartorio.py:1019–1049` `app/chamados/reconciliador.py:634–722` `app/chamados/signals.py:19–46,91–117,149–165`
- **Fábrica.** Pelo cron do cartório, com a fatia isolada, usar dois dias com trabalho previsto, `furo` e `S` isolada no alvo, pergunta de entrada com `VIA_SEM_TURNO_ABERTO` sem lastro e disputa aberta. Um dia com TXT vigente, outro aberto. Exigir que ambos cheguem ao ponto da guarda, só a aberta perca a tripla e que a saída do limbo preserve a Pauta devida. A fábrica atual desfaz o carimbo antes de consultar a saída do limbo. O teste existente de acesso direto até exige o descarimbo entregue. Mutantes de falso verde **guardar apenas depois da saída do limbo**, **inferir zero escrita de `ms=[]`** ou **ganhar verde porque o cartório anterior pulou a entregue**. Se a decisão de escopo incluir a fila, repetir pelo command real `consumir_eventos_celula`, que chega à fábrica sem esse pulo. `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:49–103,225–241` `app/ponto/services/cartorio.py:1061–1075` `app/chamados/services/fabrica_da_celula.py:199–231,268–301` `app/chamados/management/commands/consumir_eventos_celula.py:26–31` `app/chamados/reconciliador.py:1413–1438`

**A4 — Pouso próprio.** E2 e descarimbo podem usar as portas B2 já disponíveis, sem depender de alteração no cartório ou no emissor. O carimbo aberto precisa continuar chegando antes do fecho adiado mesmo com um veto no lote. O pouso que também se comprometer a guardar **INSERT da fábrica** precisa da guarda compartilhada ativa na unidade da fábrica. Esse alcance adicional não está fechado na ficha. `r.txt:581–585` `app/ponto/services/cartorio.py:1019–1049` `app/chamados/services/fio.py:1389–1459` `app/chamados/models.py:3205–3211,3674–3683`

**A6 — Escritores e leitores.**

| Campo ou valor | Quem escreve e quem lê |
|---|---|
| `data_evento`, `conversa`, `celula_id`, `motivo`, dono da pergunta | `_nascer` e o escritor de dia do fato fixam a identidade, as portas de reuso e costura podem atualizar a ligação e a data. E2 e fábrica leem identidade, célula e carimbos para seleção. C6 e B2 leem o dia do turno pelo mesmo acesso que distingue conversa. `app/chamados/models.py:3143–3211` `app/chamados/services/fio.py:839–858,968–976,1069–1092,1405–1425` `app/chamados/juizes.py:338–342` `app/chamados/reconciliador.py:675–722` `app/chamados/services/fabrica_da_celula.py:139–157` |
| `materializada_em`, `materializada_por`, `via_resolucao`; `validada_em`, resposta e via de identidade | E2 usa `PerguntaDisputa.carimbar` para a tripla. Fábrica usa `descarimbar` para limpá-la. Seletores, C6, C7, materialização e outbox leem esses campos. Validação e resposta continuam vindo das portas de validação, resposta e sequência. E2 e fábrica as usam para selecionar ou preservar o ato. `app/chamados/models.py:2002–2015,3293–3353,3364–3409` `app/chamados/services/validacao.py:171,276` `app/chamados/services/disputa_emissao.py:1529–1532` `app/chamados/juizes.py:1382–1451` `app/chamados/signals.py:252–259` |
| `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento`, estado do pai, motivos e retorno | C7 lê perguntas e respostas sem veredito. `DisputaSupervisao.fechar` grava os quatro campos. Defesa 1 lê o fecho e passa o pai à porta de estado. A fábrica deriva motivos da ata, vencimento e identidade; as portas de disputa/pergunta os persistem, ou a porta de Pauta escreve o destino DP. O cartório usa a lista retornada no contador, e a fila soma o retorno do E2. `app/chamados/models.py:2002–2015,2095–2104` `app/chamados/signals.py:19–46,149–165` `app/chamados/services/fabrica_da_celula.py:139–174,199–231,268–284` `app/ponto/services/cartorio.py:1071–1076` `app/chamados/reconciliador.py:1415–1416` |

### 3. Emissor + repasse com guarda no `pre_save`

**A1 e A5 — Sítios e arquivos.** **20, 19 e 22** na abertura, reabertura, espelho e `analisar`. **35, 36, 42, 43 e 60–63** no reuso, nascimento, costura e motivos do repasse. **40** fica protegido **se** o UPDATE for alcançado dentro da unidade cercada. A porta tem o vigia, mas a chamada direta ao UPDATE nos laços de repasse fica [nao verificado]. O fecho final pode atingir **26, 19 e 37** pela cascata. **23, 33 e 58** são efeitos do repasse cuja posição na unidade precisa ser resolvida. Arquivos propostos `app/ponto/management/commands/detectar_ausencias.py`, `app/chamados/services/disputa_emissao.py` e `app/chamados/signals.py`. **`disputa_emissao.py`** está na F5. `models.py` e `fio.py` também estão na lista e são portas já chamadas. `r.txt:582,585–587` `app/ponto/management/commands/detectar_ausencias.py:544–569,694–745,788–807` `app/chamados/services/disputa_emissao.py:1095–1175,1227–1237,1258–1305` `app/chamados/models.py:302,3195–3211,3477–3487` `arquivos_da_f5.txt:14,23,25`

**A2 — Unidades.** No emissor, verificar o dia do **chamado efetivamente aberto, reaberto ou espelhado**, antes da respectiva porta. No repasse, manter a trava existente **Colaborador → Disputa → Chamado → Pergunta**. Abrir a cerca antes da primeira mutação da unidade reversível, com os chamados do mesmo episódio juntos. Fechar depois da pergunta e dos efeitos daquele episódio, inclusive o `analisar` e os motivos que ele pode publicar. No **36**, o `pre_save` da pergunta em inserção, na unidade cercada do cartório, chama o vigia existente depois de `_nascer` atribuir o dia efetivo e antes do INSERT. A recusa precisa sair do bloco antes dos efeitos seguintes. O pai, o contêiner e os motivos iniciais já podem ter sido gravados nessa altura. Um savepoint iniciado só na pergunta deixa essas escritas anteriores de fora. `r.txt:582` `app/ponto/management/commands/detectar_ausencias.py:544–569,671–745` `app/chamados/services/disputa_emissao.py:1007–1073,1095–1175,1194–1237,1263–1305` `app/chamados/models.py:3205–3211,3674–3683` `app/chamados/services/fio.py:1389–1459`

**A3 — RED de pré-INSERT com origem aberta e alvo entregue.** Empresa com corte 21, colaborador ativo com vínculo 5×2 e adesão E/S anterior. Preparar dia aberto A com furo sem chamado, e um órfão vivo do mesmo colaborador no dia E entregue, com ata lavrada e falta real, sem pergunta daquele fato. Incluir outro episódio aberto que deve prosperar e, em um braço, dois chamados da mesma batida-origem. Entrar por `julgar_celula(A)` ou pelo cron do cartório. O emissor abre A e colhe os órfãos do colaborador inteiro. Testemunhar o **pré-save real** com `adding` e dia efetivo E, ausência do INSERT de E, de `analisar` e de motivos atribuídos à unidade vetada. Conferir o INSERT e a costura do episódio aberto, o estado do chamado de A e o contador. Hoje a guarda de `save` pula criação, e o receiver do outbox recebe `created` depois da escrita e retorna. Mutantes de falso verde **filtrar pelo dia do chamado antes de `_nascer`**, **inserir e só depois dar rollback**, ou **vetar o lote todo**. Acrescentar controle noturno em que a data civil da batida e o dia do turno diferem, confirmando que o nascimento proposto chegou ao dia efetivo. A fixture final com todos os portões abertos fica [nao verificado]. `app/ponto/services/cartorio.py:1783–1841,1873–1897,2021–2031,2059–2089` `app/ponto/management/commands/detectar_ausencias.py:694–745,788–807` `app/chamados/services/disputa_emissao.py:837–861,1180–1266` `app/chamados/models.py:3054–3088,3205–3223,3674–3683` `app/chamados/signals.py:252–259` `app/chamados/tests/test_plantio_noturno_mais1.py:39–76,100–105`

No mesmo RED, testar o veto do **emissor** abrindo, reabrindo e espelhando pela porta do cartório numa base sem a primeira fatia, com controle aberto e foto de PK, estado, `marcos_faltantes` e contador. Do contrário, o veto de **05** pode esconder o ramo do emissor. O cron autônomo do detector serve de controle da fronteira externa, conforme a decisão de escopo. `app/ponto/services/cartorio.py:1839–1897` `app/ponto/management/commands/detectar_ausencias.py:424–450,544–569,694–745` `app/config/crons.py:364` `r.txt:580,582,585`

**A4 — Pouso próprio.** O escopo do cartório e o vigia B2 já têm pontos de entrada na árvore. A fatia pode pousar com os consumidores existentes se conservar a distinção entre emissão contada, nenhum trabalho e recusa, e se a recusa parcial tiver o contrato de E. Hoje `REPASSE_RECUSADO` declara **“nada feito”**, enquanto `_contar_emissao` conta retorno não textual do emissor. Uma cobrança aberta de A pode sobreviver a uma recusa do repasse, e esse fato precisa continuar observável. `app/ponto/services/cartorio.py:59,991–994,1150–1153,2021–2031` `app/chamados/services/fio.py:1431–1459` `app/chamados/services/disputa_emissao.py:1036–1049,1303–1313` `app/ponto/management/commands/detectar_ausencias.py:788–807`

**A6 — Escritores e leitores.**

| Campo ou valor | Quem escreve e quem lê |
|---|---|
| `data_turno`, `marcos_faltantes`, `_marco_por`, horário previsto e atraso | Emissor escreve os defaults de a

[truncado em 20000 de 41091 caracteres]

## 20/11 20:02 · sessao 0a4110ebdf00 · - · - · origem=Ronald (tmux)

**D.** A ficha usa os juízes já registrados `_K6`, C6 e C8, os escritores atuais e uma guarda sem persistência. A guarda de INSERT deve ser ativada apenas na unidade do cartório e chamar o vigia existente depois de `_nascer` fixar o dia. Uma ativação global mudaria também regras vigentes de atos externos. [app/docs/JUIZES.md:37–42] [app/chamados/models.py:386–407,3205–3211,3674–3683] [app/chamados/services/fio.py:1389–1428] [app/docs/CORTES.md:426]

1. O encaixe A1 pode fornecer o relógio ao C6, mas a seleção SQL hoje elimina uma validação futura antes do C6. A lista selecionada serve também ao tripwire em memória e à relavra posterior ao `save`. O `pre_save` atual só captura o status anterior. Se o controle exigido depender de mudar esse filtro, aplicam-se o PAREI da ficha e a proibição de alterar a cláusula. O carimbo de identidade futuro é controle alternativo potencialmente alcançável, pois independe daquele filtro de validação. [app/chamados/models.py:216–234,309–319,343–380,386–395,3539–3566] [app/chamados/signals.py:180–186] [app/chamados/juizes.py:1382–1406,1443–1457] [ficha r.txt:583–585]
2. A passagem B5.3b e a releitura pós-emissão são os dois lugares existentes. Usá-los e reportar a divergência residual atende os limites. Redrenar a fila ou remendar no fim seria acrescentar o que o aval veda. O cálculo já retira o resíduo e publica contagem. [app/ponto/services/cartorio.py:1045–1056,1101–1126] [ficha r.txt:580,585]
3. O O44 vigente conserva o fio corrigível com o TXT intacto e a L-113 permite nascimento em competência paga. A fábrica manda limbo entregue à Pauta, conserva nascimento em `trabalha=True` e hoje tenta descarimbar antes da bifurcação. O selo F5 de exportada chama a fábrica diretamente e exige o descarimbo. Um veto global nesse escritor muda o contrato comprovadamente. Um veto restrito ao cartório conserva a passagem direta do selo e executa a escolha local do desenho A. [app/docs/LEIS.md:91,125,155] [app/docs/CORTES.md:426] [app/chamados/services/fabrica_da_celula.py:206–231] [app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:85–87,225–241] [arquivos_da_f5.txt:100]
4. A cerca do escritor de perguntas não decide sozinha o status da disputa, o do pai, seus contextos, a precedência após `julgar_colab`, as ações entregues fora da unidade nova ou o instante concorrente de publicação do TXT. `_K6` consulta a vigência e pode usar memo. O escritor da exportação insere seu registro separadamente. Garantia contra mudança concorrente de vigência entre consulta e escrita [nao verificado]. [app/chamados/reconciliador.py:649–667] [app/chamados/signals.py:19–46] [app/ponto/services/cartorio.py:2088–2096] [app/chamados/services/vigia_fio.py:786–808] [app/folha/export.py:580–592] [app/folha/models.py:173–188]
5. A catraca da cópia registra `1234/1387` e bate com as duas expressões do cartório. Se o diff deslocar essas linhas, o teste exige registro no mesmo commit, outro toque em arquivo listado pela F5. A ficha traz números de outra versão. Resultado de execução [nao verificado]. [app/ponto/services/cartorio.py:1234,1387] [app/core/tests/test_medida_turno_do_par.py:319,558–562] [arquivos_da_f5.txt:109] [nao verificado]

**E.** Quatro decisões observáveis que podem ser explicitadas no brief. Alternativas de implementação como `ContextVar` versus passagem explícita, forma do savepoint, posição do hook ou atualização da catraca não são leis. O uso de `_K6`, C6 e C8, a antecedência do savepoint, o veto B2 de dia indecidível, a exceção conversa e a sequência de travas já estão escritos. [CLAUDE.md:269–281] [app/docs/CORTES.md:558] [app/chamados/juizes.py:338–342] [app/chamados/services/fio.py:1389–1459] [app/chamados/services/disputa_emissao.py:1007–1033]

1. **Alcance das guardas nos escritores compartilhados.** Ativar somente nas unidades originadas no cartório conserva a execução direta compartilhada. Ativar também nas entradas automáticas de cron, signal ou fila altera o E2 e a fábrica que essas entradas chamam, e pode alterar efeitos da política anterior de exportada. A guarda de INSERT continua limitada à unidade do cartório pelo aval. [ficha r.txt:581–585] [app/chamados/reconciliador.py:1413–1438,1483–1493] [app/ponto/management/commands/detectar_ausencias.py:694–745,788–807] [app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:85–87,225–241]
2. **Destino de pai/disputa compartilhados entre atos admitidos e recusados.** Persistir o agregado apenas com unidades admitidas e fechar pelo universo legítimo preserva o progresso independente. Manter o agregado aberto enquanto representa a unidade que recusou preserva a pendência compartilhada. Se nenhum episódio gravar, o código hoje já pode ter criado pai/disputa e motivos, pois antecedem os nascimentos, o que exige definir se há agregado legítimo sem episódio aceito. Nenhuma alternativa autoriza escrever dia entregue. [app/chamados/services/disputa_emissao.py:1110–1175,1194–1237,1270–1305] [app/chamados/reconciliador.py:649–667] [app/chamados/signals.py:19–46]
3. **Contrato observável de sucesso parcial do repasse no emissor.** Conservar a emissão aberta já válida e publicar sucesso parcial por unidade, com veto separado, conserva o `emitidos` como retrato do que abriu. Tratar a emissão e o repasse como inseparáveis implica abortar também os efeitos da origem e reexaminar o push, hoje anterior ao repasse. O sentinela atual diz «nada feito», e `None` pode ocorrer depois de complemento e conta como emissão na outra porta. A forma do retorno é técnica, mas a promessa de efeito parcial é substantiva. [app/chamados/services/disputa_emissao.py:1036–1049,1095–1108,1303–1313] [app/ponto/management/commands/detectar_ausencias.py:775–807] [app/ponto/services/cartorio.py:2021–2031] [nao verificado]
4. **Efeito operacional do PAREI residual, se a obra pretende torná-lo comportamento de execução.** Reportar célula e etapa e travar o pouso conserva atos independentes admissíveis. Abortar a unidade afetada durante execução requer transação aberta antes da primeira escrita e muda quais atos abertos persistem. Se «PAREI da obra» for lido literalmente como condição de aceite pré-pouso, a primeira opção já está escrita e esta decisão sai da contagem. [ficha r.txt:580,585] [app/ponto/services/cartorio.py:1016–1049,1061–1126,1901–1923] [app/chamados/services/fio.py:1431–1459] [nao verificado]

**F.** A F5 é anterior por aval. Entre as três fatias, nenhum código atual obriga ordem total. A1 e a guarda de INSERT compartilham `signals.py`, o que exige conciliar edições. Se «E2 + fábrica» prometer proteger também o nascimento da fábrica no #36, depende da guarda de INSERT ativa naquela unidade ou de pousar antes uma infraestrutura equivalente comum. Se a fatia se limitar ao descarimbo #39, a dependência não existe. O E2 vem antes do fecho adiado dentro da execução, independentemente da ordem de deploy. [app/ponto/services/cartorio.py:1019–1049,1061–1073] [app/chamados/models.py:3205–3211,3674–3683] [app/chamados/services/fabrica_da_celula.py:160–174,206–220] [app/chamados/signals.py:177–186,247–259] [respostas_2011_1950.txt:5–10] [nao verificado]

## 20/11 20:00 · sessao 0dfd8d6ec562 · - · - · origem=Ronald (tmux)

- **Cartório e relógio.** RED pelo cron `processar_cartorio --apply`, com vínculo 5x2, duas batidas anteriores de adesão, zero no dia-alvo, `concorde` prévio e pergunta sem carimbo ancorada em chamado vivo. Observar F/FP realmente consumidos, C6 antes e imediatamente após a primeira lavra e coerência da saída sem depender de emissão. Hoje a leitura vem antes do par/ata da lavra, o chamado existente cala emissão e a releitura pós-emissão depende de incremento em `emitidos`. Controles de célula entregue com TXT vigente e de célula aberta preservam a janela. Um mutante que ajuste o veredito só no fim pode enganar a asserção do estado final. `app/ponto/management/commands/processar_cartorio.py:38–65` `app/ponto/tests/test_o261_cobrado_le_o_fio.py:243–292` `app/ponto/services/cartorio.py:1045–1049,1101–1114,1413–1438,1493–1511,1538–1547,1783,1901–1922` `app/folha/export.py:556–594`
- **Controle do relógio A1.** Porta unitária pública com borda 10:00, ambiente 10:02 e `VIA_ADMIN_DECIDIU` às 10:01 em pergunta ancorada de competência aberta, célula `furo` e sem destino C8 válido. Observar a tentativa real, o C6 do gate, e um controle que consegue fechar e testemunha também o tripwire e o fim do escopo antes dos receivers. A validação futura é controle separado, pois o SQL atual pode retirar a pergunta antes do C6. Relógios iguais, checar só a recusa ou só o gate deixam o mutante passar. Atingimento integral da fixture [nao verificado]. `app/ponto/services/cartorio.py:2059–2089` `app/chamados/models.py:216–234,302–319,343–380,422–449,664–701,3551–3566` `app/chamados/juizes.py:1382–1406,1443–1457` `app/chamados/signals.py:91–118,177–186`
- **E2 e fábrica.** E2 pelo julgamento público do dia aberto ou pelo registro real da última batida. O precedente usa 12x36, saída já registrada, chamado de furo com marco E e última entrada registrada pela porta. Plantar candidata entregue absolvida sem tranca no mesmo colaborador, observar o carimbo aberto antes do fecho adiado e a linha entregue íntegra. Hoje E2 percorre o colaborador e lê C6/K7 antes de escrever sem K6. O mutante que aborta o colaborador inteiro parece verde se a única asserção for a linha entregue. Fábrica com carimbo `sem_turno_aberto` sem lastro, `furo`, disputa aberta e saída isolada, pela execução pública que realmente alcança a fábrica. Observar os três campos e a trilha no mesmo PK, com controle aberto e controle da saída do limbo. Hoje descarimba antes da rota de Pauta. A cerca anterior do cartório pode tornar esse RED vazio. Na base isolada use o cron, e no conjunto use a fila somente se o alcance externo estiver aprovado. A caracterização corrente exige descarimbo no acesso direto em exportada. `app/ponto/tests/test_dst_l2_jz1_rejeita_justificativa.py:200–253` `app/ponto/services/cartorio.py:1019–1049,1061–1073` `app/chamados/reconciliador.py:634–667,675–722,1413–1438` `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:49–103,225–241` `app/chamados/services/fabrica_da_celula.py:206–231,288–301` `app/chamados/services/relavra_via.py:171–184,215–252`
- **Emissor e repasse.** Julgamento público de A aberta com furo e adesão, E entregue com ata acusatória prévia e órfão regularizável sem pergunta. A emissão de A coleta órfãos do colaborador e alcança E. Exigir testemunha do pré-save com `adding` e dia efetivamente atribuído, nenhum SQL INSERT vetado, nenhuma alteração de `analisar`/motivos/contexto da unidade vetada e persistência do aberto. Variação noturna distingue dia civil de dia do turno e exige resolvedora válida. Hoje `save` só chama vigia em atualização e o receiver atual sai em `created`. Vetar o lote inteiro, admitir pelo dia do pedido ou inserir e desfazer depois parece verde se observar só a ausência final da linha. Controle do emissor mede separadamente nascimento, reabertura/espelho, contadores e efeitos de push, pois o cron detector pula existente. `app/ponto/services/cartorio.py:1783–1841,1873–1897,2021–2031` `app/ponto/management/commands/detectar_ausencias.py:424–450,544–569,694–745,788–807` `app/chamados/services/disputa_emissao.py:1059–1073,1100–1146,1180–1266` `app/chamados/models.py:3054–3088,3205–3223,3674–3683` `app/chamados/signals.py:252–259` `app/chamados/tests/test_plantio_noturno_mais1.py:39–76,100–105`
- **Conflitos de alcance.** L-113 permite chamado em competência paga e L-124 conserva fato novo em qualquer competência. Há caracterizações de relavra do cartório entregado, preservando TXT/Gravado, e de fábrica em dia de trabalho entregue. A extensão da cerca a correção ou a chamadas externas exige delimitação. `app/docs/LEIS.md:145,155` `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:77–85,94–96,113–131` `app/chamados/tests/test_f1c4_3_emissores.py:199–218`

## 20/11 20:00 · sessao 805d943d4d4b · - · - · origem=Ronald (tmux)

No sítio **05**, as chamadas de produção identificadas para o carimbo partem do próprio cartório. Um ato externo pode chegar por `julgar_celula`, mas entra em `julgar_colab` antes do carimbo. O save de precedência após a volta é o sítio **06**. Bypass direto de 05 [nao verificado]. `app/ponto/services/cartorio.py:30,1343,1921,2088–2096` `app/ponto/portas/celula.py:1201,1216`

| Sítios | Caminhos externos à unidade proposta |
|---|---|
| **19, 26, 37, 42** | Segundo receiver de batida ou callback de filho terminal → reconciliação por colaborador → E2, órfãs e retratação. O worker também entra diretamente no E2 e em órfãs, premissa e lastro. O fecho de disputa chama o pai e S144. O save terminal do chamado chama S143. Beco pós-commit, atos humanos e materialização também chegam às mesmas portas. `app/ponto/signals.py:206–215` `app/chamados/signals.py:19–46,91–117,149–165,229–240,351–371` `app/chamados/reconciliador.py:642–667,1415–1438,1483–1493,1697–1718` `app/chamados/services/becos.py:55–91` `app/chamados/services/materializacao.py:1059,1084,1981,2043,2180` |
| **19, 20, 22, 35–36, 42–43, 60–63** | Cron detector e emissão retroativa chegam ao emissor e ao repasse. Alertas, início web de órfãos e command de perguntas chegam diretamente ao repasse. Cobrar Dia repassa após retornar do cartório unitário. O detector também fecha por afastamento, cobertura e batida. O espelho, ressalva de limbo e reconciliação de grade compartilham o escritor de contexto. `app/config/crons.py:362–364` `app/ponto/management/commands/detectar_ausencias.py:440,559,694–745,759–767,797–803,821–854` `app/ponto/management/commands/emitir_furo_retroativo.py:154` `app/ponto/management/commands/processar_alertas_turno.py:385` `app/ponto/management/commands/processar_alertas_avancados.py:159` `app/chamados/urls.py:21,32,39` `app/chamados/views.py:674,954` `app/chamados/services/fio.py:595–598` `app/chamados/services/acoes_disputa.py:214–226` `app/chamados/services/cobrar_dia.py:41–49` `app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:19–29` `app/chamados/services/pergunta_no_app.py:253–261` `app/chamados/services/disputa_emissao.py:1041–1073,1095–1099,1118–1175,1227–1237,1258–1298` `app/ponto/management/commands/reconciliar_grade.py:473` |
| **20, 35–36, 39, 42–43** | Worker e backfill chamam a fábrica diretamente. API, browser e retro de foto entram pela emissão de disputa e chegam à geração, inclusive à costura de órfã. Declaração do colaborador e do admin abre ou reusa pergunta datada. Reabertura admin e âncora chegam a 36 pela porta de conversa. `app/chamados/reconciliador.py:1435–1438` `app/chamados/services/fabrica_da_celula.py:160–174,206–219,288–301` `app/chamados/management/commands/backfill_fio_morto_e4b.py:125–143,305–318` `app/api/views.py:768` `app/api/views_core.py:762` `app/ponto/views.py:352` `app/ponto/services/triagem_batida.py:734` `app/ponto/management/commands/marcar_foto_ausente_retro.py:96` `app/chamados/services/disputa_emissao.py:448–473,676–699,1385` `app/chamados/views.py:477–480,1089–1090` `app/chamados/services/declaracao_texto.py:361–378,660–668` `app/chamados/services/acoes_disputa.py:193,263–267` |
| **39, 40, 43** | Reabrir período devolve perguntas pela via de competência. Renascimento, desarquivamento, correção HTML/API/mesa, comando de cura e callback de ausência alcançam o lote 40. Command de migração alcança 43, com veto próprio. `app/ponto/views.py:1523` `app/ponto/services/fechamento.py:1432–1448` `app/chamados/services/competencia_trancada.py:275–282` `app/chamados/models.py:981,1501–1510,1700–1705,3530–3536` `app/chamados/views_arquivar.py:86` `app/chamados/views.py:1329` `app/api/views.py:2706` `app/chamados/services/acoes_disputa.py:105–106` `app/chamados/management/commands/curar_perguntas_em_limbo.py:70` `app/ponto/signals.py:254–272` `app/chamados/services/materializacao.py:1173–1185` `app/chamados/services/disputa_emissao.py:1656` `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:309–319,416,509–522` |

O sítio 20 tem ainda portas diretas de abertura rápida, ausência, adesão, reemissão C2, âncora, régua, alertas, par-relâmpago, turno retroativo e API. A eventual entrada no cartório por receiver se dá depois do save. `app/chamados/views.py:973` `app/ponto/services/ausencia.py:805` `app/chamados/services/adesao.py:75` `app/chamados/reconciliador.py:960–972` `app/chamados/services/disputa_emissao.py:1353` `app/escala/regua_defesa.py:51` `app/ponto/management/commands/processar_alertas_turno.py:335` `app/ponto/management/commands/detectar_par_relampago.py:368` `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151` `app/api/views.py:1985` `app/chamados/models.py:961–1030` `app/chamados/signals.py:189–218`

O sítio 36 recebe o dia efetivo antes de `save(force_insert=True)`. A cópia só vigia perguntas existentes, e a guarda proposta vale para o cartório cercado. Conversa fica fora do conjunto de fatos da cerca. `app/chamados/models.py:2963–3009,3205–3211,3674–3683` `app/chamados/services/fio.py:1389–1404,1414–1418` `r.txt:582–585`

A unidade B2 já existe em retratação, beco, resolver de S143 e no ramo histórico de reatamento. Uma unidade nova aninhada reaproveita a cerca externa e o veto pode desfazer tudo que ela abrange. O callback pode encontrar ou não `em_julgamento` conforme a fronteira transacional. A ocorrência real fica [nao verificado]. `app/chamados/reconciliador.py:562,1707` `app/chamados/services/becos.py:65–66` `app/chamados/services/disputa_emissao.py:1911–1919` `app/chamados/services/fio.py:1431–1459` `app/ponto/services/cartorio.py:991–994,1150–1153`

## 20/11 19:56 · sessao 5bc6b8895944 · E4C-F5-CONSTROI · construtor · origem=Code

**Selo (78) corrigido.** O censo passou de **5 sítios e 1 acusação** para **5 sítios e 0 acusações**. O teste alvo e o novo sintético passaram em memória. Os mutantes isolados produziram **2 falhas** na ressurreição e **1 falha** no lote usado pela RECUSA. [Censo:479](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:479) · [Asserção:530](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:530) [nao verificado]

**Arquivos tocados**

- [test_e4c_f5_selo_par_ato_coluna.py:281](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:281), visitante e teste sintético do (78). [nao verificado]
- [e4c_f5_voltaE_mutantes.py:45](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e4c_f5_voltaE_mutantes.py:45), duas novas trocas de fonte em memória.

### (a) Sítios enumerados

| Sítio | Escopo atribuído | Ato(s) | Pareado antes → depois | UPDATE pareado |
|---|---|---|---|---|
| [chamados/models.py:1517](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:1517) | `ChamadoColaborador.reabrir` | REABERTURA | Não → sim | `descarimbar_em_lote(..., escrever_so=ids, validada_em=None)` no auxiliar. [models.py:1721](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:1721) |
| [chamados/models.py:1719](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:1719) | `ChamadoColaborador.ressuscitar_perguntas` | REABERTURA | Sim → sim | A mesma chamada à porta do lote. [models.py:1721](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:1721) |
| [chamados/services/disputa_emissao.py:1664](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/disputa_emissao.py:1664) | `reabrir_para_correcao` | REABERTURA, RECUSA | Sim → sim | `descarimbar_em_lote(qs, **campos)`, com `validada_em=None` em `campos`. [disputa_emissao.py:1630](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/disputa_emissao.py:1630) · [disputa_emissao.py:1683](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/disputa_emissao.py:1683) |
| [chamados/services/validacao.py:293](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:293) | `devolver_validacao_recusada` | DEVOLUCAO | Sim → sim | `perg.save(update_fields=['validada_em', 'validada_por'])`. [validacao.py:290](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:290) |
| [chamados/services/validacao.py:370](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:370) | `devolver_paridade_bug_par_103` | DEVOLUCAO | Sim → sim | `perg.save(update_fields=campos + ['validada_em', 'validada_por'])`. [validacao.py:367](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:367) |

**(b) Sim, o escritor já aparecia no próprio escopo.** O censo resolve `registrar(_compensa)` seguindo o `append(dict(...))` dentro do `for`. A linha fornece `pergunta_id=_q.pk` e `ato=DecisaoPergunta.REABERTURA`. A origem do `append` permite ao visitante ligar `_q` ao lote iterado. [models.py:1705](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:1705) · [models.py:1713](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:1713) · [Resolução das linhas:174](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:174) · [Resolução do lote:234](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:234)

**(c) Três chamadores diretos em produção na cópia.** O visitante procura auxiliares entre os escopos do mesmo arquivo e qualifica `self` e `cls` pela classe. [Resolução do auxiliar:297](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:297) · [Censo por arquivo:479](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:479)

- `ChamadoColaborador.reabrir` recebe a atribuição porque `self.ressuscitar_perguntas()` resolve para o método da própria classe. [chamados/models.py:1517](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:1517)
- `desarquivar_chamado` fica fora das atribuições indiretas. A chamada usa `ch` em outro arquivo. [chamados/views_arquivar.py:58](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/views_arquivar.py:58) · [chamados/views_arquivar.py:86](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/views_arquivar.py:86)
- `_julgar_colab_corpo` fica fora das atribuições indiretas. A chamada usa `x` em outro arquivo. [ponto/services/cartorio.py:1155](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/ponto/services/cartorio.py:1155) · [ponto/services/cartorio.py:1721](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/ponto/services/cartorio.py:1721)

**(d) Mesmo conjunto e mesmo `atomic` externo.** Os `ids` relidos vêm de `alvo_lido` sob a trava, com materialização presente e exclusão das vias fora do fato. A REABERTURA percorre `alvo` reconstruído com esses `ids` e a mesma exclusão. O UPDATE intersecta `alvo_lido` com `escrever_so=ids` e aplica a mesma exclusão. As duas chamadas estão no corpo do `atomic` de `ressuscitar_perguntas`. O `bulk_create` do registro também tem um `atomic` interno. [models.py:1639](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:1639) · [models.py:1646](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:1646) · [models.py:1669](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:1669) · [models.py:1705](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:1705) · [models.py:1719](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:1719) · [models.py:3536](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:3536) · [models.py:4437](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:4437)

### Antes e depois do visitante

Na base `ae40b281`, `_registros` devolvia a chamada do auxiliar como origem do ato indireto, e o censo buscava o UPDATE nas escritas do chamador (`test_e4c_f5_selo_par_ato_coluna.py:304` e `:469` na base). Agora o censo continua emitindo a linha atribuída ao chamador e carrega o escopo, a chamada a `registrar` e a origem da linha gravada. É nesse escopo que exige a mesma pergunta ou lote, o mesmo `atomic` e os mesmos ramos. [Atribuição:297](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:297) · [Pareamento e emissão:486](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:486)

O sintético mantém `Porta.chamar` e `Porta.desfazer` enumerados em todos os casos. O par completo no auxiliar passa nos dois. A retirada da coluna acusa os dois, assim como o UPDATE de outra pergunta no chamador, pois falta a escrita pareada no escopo que registra. O sintético também rejeita a escrita da mesma pergunta só no chamador, de outra pergunta no auxiliar, em outro `atomic` ou em outro ramo. Contra o visitante da base, houve **2 falhas na linha 609**, nos casos “mesma pergunta no auxiliar” e “mesma pergunta no chamador”. [Casos:595](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:595) · [Asserção:609](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:609)

### Mutantes e labels

- **`d_ressuscitacao_reabertura_sem_coluna`** remove só `validada_em=None` da chamada real à porta do lote. Em `test_atos_que_desfazem_gravam_a_coluna`, deu **2 falhas na linha 530**, uma para `reabrir` em `models.py:1517` e outra para `ressuscitar_perguntas` em `models.py:1719`. [Tabela:45](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e4c_f5_voltaE_mutantes.py:45) · [Alvo:1721](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:1721) · [Asserção:530](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:530)
- **`d_recusa_sem_coluna_no_lote`** remove `validada_em=None` de `campos` no escritor real chamado com `ato='recusa'`. Deu **1 falha na linha 530** do mesmo teste, no sítio `disputa_emissao.py:1664`, que enumera REABERTURA e RECUSA juntas. Antes, o `c_recusa_update_fields_cru` da tabela Volta E exercitava a catraca e deixou este selo verde na execução isolada. Os mutantes `recusa_foto_do_ato` da Volta B e `recusar` da Volta D trocam leituras. [Tabela:40](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e4c_f5_voltaE_mutantes.py:40) · [Novo mutante:50](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e4c_f5_voltaE_mutantes.py:50) · [Alvo:1630](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/disputa_emissao.py:1630) · [RECUSA:943](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:943) · [Volta B:34](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e4c_f5_voltaB_mutantes.py:34) · [Volta D:30](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e4c_f5_voltaD_mutantes.py:30) [nao verificado]

```bash
selo=chamados.tests.test_e4c_f5_selo_par_ato_coluna.SeloParAtoColunaTest
alvo="$selo.test_atos_que_desfazem_gravam_a_coluna"

env -u F5_VOLTA_E_MUTANTE PYTHONDONTWRITEBYTECODE=1 \
  DJANGO_SETTINGS_MODULE=config.settings_test \
  python3 manage.py test "$alvo" \
  "$selo.test_MORDE_update_de_ato_indireto_no_escopo_que_grava" --noinput

PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings_test \
  F5_VOLTA_E_MUTANTE=d_ressuscitacao_reabertura_sem_coluna \
  python3 manage.py test "$alvo" --noinput

PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings_test \
  F5_VOLTA_E_MUTANTE=d_recusa_sem_coluna_no_lote \
  python3 manage.py test "$alvo" --noinput
```

**Limites.** A execução local usou `unittest` em memória, com as constantes extraídas do AST dos modelos. O Python da cópia retornou `ModuleNotFoundError` para `django.core`. Execução dos labels pelo Django, suíte, banco, logs e comportamento em produção **[nao verificado]**. A resolução estática de auxiliares cobre um salto no mesmo arquivo, pelas referências que o visitante reconhece. [manage.py:11](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/manage.py:11) · [Resolução de auxiliares:297](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:297) [nao verificado]

## 20/11 19:56 · sessao 81e83a5485e5 · FALTA-APROVADA-MOVE-O-TXT-ENTREGUE · leitura · origem=Code

As perguntas 33668, 33669 e 33686 correspondem a **dois colaboradores distintos** na amostra: PK 247 na empresa 2 e PK 121 na empresa 3. A passagem por `FALTA_DIA` retirou respectivamente 3, 3 e 1 linhas tipo 10 do TXT que o comparador montou naquele momento. A rejeição restaurou o hash integral e a contagem de linhas da primeira foto. O total na frota é [nao verificado]. `f5_6a1_v2_main.out:14,15,16`, `f5_6a1_v2_f5.out:14,15,16`, `app/folha/export.py:654`.

## 1. Da resposta à retirada das linhas

1. **A porta grava a falta no dia do fato.** A sonda chama `api_disputa_responder` autenticada como o colaborador da pergunta. A API verifica a titularidade e se a pergunta está viva, então chama `responder_pergunta_disputa_core`. O core grava a resposta e encaminha `FALTA_DIA` a `_materializar_ausencia_de_resposta` com `('falta', 'aprovada')`. Essa função chama `criar_ausencia` com o colaborador da disputa e `data=p.data_evento`. Nos três ensaios a linha de `Ausencia` era nova na foto. `app/api/views.py:2631,2646`, `app/chamados/services/disputa_emissao.py:1529,1560`, `app/chamados/services/materializacao.py:1015,1019,1095`, `app/ponto/services/ausencia.py:483,560`, `f5_6a1_v2_main.out:14,15,16`.
2. **A leitura direta da falta pode fechar a porta de homologação.** `txt_vigente_x_gravado` chama `montar_linhas_export`, que chama `classificar_export`. Depois dos crivos anteriores, `classificar_export` chama `_codigos_nao_homologados`. Esse crivo lê `datas_falta` → `datas_falta_janela` → `_datas_ausencia_tipo_janela` → `ausencias_na_janela` e `datas_no_periodo` → `dias_da_ausencia`. O dado lido é `Ausencia` do colaborador, com `status='aprovada'`, `tipo` no conjunto de descontos do catálogo vigente e `data`/`data_fim` que alcançam a janela. Se há dias de falta, há rubrica ativa `falta_diurna` e o código dela está fora do catálogo **não vazio** da empresa, o veredito é `fora / nao_homologado`. Com esse veredito o montador pula o fechamento inteiro, antes de emitir qualquer registro. `app/folha/export.py:63,71,146,169,184,258,654,714,725,974,984,994,1013`, `app/ponto/catalogo/ausencias.py:267,331`, `app/ponto/turnos.py:2802`.
3. **A escrita também pode mudar a porta do espelho.** O `save` de `Ausencia` chama `_cartorio_post_save_ausencia` → `_julgar_dias` → `julgar_celula` → `julgar_colab` → `_julgar_colab_corpo`. O cartório lê a cobertura da ausência e as batidas apuráveis, passa o estado da cobertura a `classificar_dia` e pode gravar outro `veredito`, `protestos` e `ata` por `lavrar_veredito` e `carimbar`. Por exemplo, fato em turno encerrado coberto por ausência aprovada pode produzir `FATO_EM_AUSENCIA`, que vira `fato_em_ausencia`, um veredito que acusa. Na montagem, `classificar_export` → `motivos_retencao_espelho` → `motivos_retencao_celula` → `_dias_sem_decisao` lê a célula gravada. Se restar motivo de retenção, o resultado é `fora / furo_espelho` e o montador pula o colaborador inteiro. A sonda não publicou a célula nem o `status`/`motivo` do classificador. O crivo efetivamente acionado em cada um dos três casos é [nao verificado]. `app/ponto/signals.py:85,143`, `app/ponto/services/cartorio.py:1016,1199,1385,1500,1901,1921,2059,2088`, `app/ponto/supra_juiz.py:268,392,413`, `app/escala/models.py:1619,1624,1767`, `app/ponto/portas/celula.py:957,1090,1181`, `app/folha/export.py:725,968,1176,1247,1273,1293,1301`.
4. **A emissão tem condições por fechamento e por evento.** Para um fechamento `aberto` ou `aprovado`, o colaborador precisa estar no conjunto `entra`. Aí `eventos_do_fechamento` fornece os valores de horas e o montador chama `montar_linha_10` para cada evento com valor não zero e rubrica ativa. Se houver dias de falta e rubrica `falta_diurna`, `montar_bloco_falta` emite um tipo 10 com a quantidade de dias e um tipo 11 por dia. A rejeição passa por `decidir` → `_decidir_no_ato` → `mudar_estado`, grava `rejeitada` e tira a ausência da leitura direta de faltas aprovadas. A sonda também chama explicitamente `devolver_absorvidas_da_ausencia_rejeitada`, serviço que o signal normalmente agenda para depois do commit. `app/folha/export.py:34,87,215,402,687,716,725,730,740`, `app/ponto/services/ausencia.py:975,988,1057,1084`, `app/ponto/signals.py:254,261,272`, `app/chamados/services/materializacao.py:1137`, `f5_6a1_v2_main.out:14,15,16`.

## 2. O que separa os 3 dos outros 13

Os campos que separam os grupos são `diferencas_txt.antes_para_passagem.matricula_do_caso_afetada`, `matriculas_distintas_afetadas`, `por_tipo` e a mudança de `fotos.*.hash_hoje` e `linhas_hoje`. As duas saídas dão os mesmos valores abaixo. `f5_6a1_v2_main.out:4–19`, `f5_6a1_v2_f5.out:4–19`.

| Tentativa | `ausencia.tipo` / `status_apos_passagem` | Matrícula do caso afetada / distintas | Tipo 10 retirados na passagem | `linhas_hoje` antes → passagem → rejeição |
|---|---|---|---:|---|
| 33668 `FALTA_DIA`, empresa 2, PK 247 | `falta` / `aprovada` | `true` / 1 | 3 | 429 → 426 → 429 |
| 33669 `FALTA_DIA`, empresa 2, PK 247 | `falta` / `aprovada` | `true` / 1 | 3 | 429 → 426 → 429 |
| 33686 `FALTA_DIA`, empresa 3, PK 121 | `falta` / `aprovada` | `true` / 1 | 1 | 118 → 117 → 118 |
| 5 outros `FALTA_DIA` | `falta` / `aprovada` | `false` / 0 | 0 | 429 ou 118 em todas as fotos |
| 8 `ATESTADO` | `atestado` / `aguardando_documento` | `false` / 0 | 0 | 429 ou 118 em todas as fotos |

Nos três, o hash publicado muda de `732033257f1c` para `08a1e2ab780f` na empresa 2 e de `4715b21f2af4` para `de6886947941` na empresa 3. A passagem só retira linhas tipo 10 e a rejeição só as devolve. Nos 13, todas as contagens de adição e remoção são zero nas duas etapas, e o par `hash_hoje`/`linhas_hoje` fica igual. Nos 16, `porta=ACEITA`, `nova_na_foto=true`, `status_apos_rejeicao=rejeitada`, `fechamento_mensal_status=aberto`, `divergente=true` e `foto_3_igual_a_1=true`. As fotos não registraram diferenças em `FechamentoMensal`, `Batida` do dia ou no registro vigente de `ExportacaoDominio`. A chave `TXT_do_gravado` na sonda fotografa o par calculado hoje. `f5_6a1_v2_main.out:4–19`, `f5_6a1_v2_f5.out:4–19`, `app/folha/export.py:654,673`.

Pelo código, `aguardando_documento` fica fora da leitura direta de faltas. Para `falta/aprovada`, ainda é preciso a falta alcançar a janela e mudar uma decisão que afete linhas. Na homologação, por exemplo, uma falta adicional pode deixar o resultado igual se já havia dia de desconto, se falta a rubrica, se o catálogo está vazio ou se um crivo anterior já tirou o colaborador. Se a rubrica passa e o colaborador entra, o caminho normal da falta acrescenta seu bloco. Os cinco `FALTA_DIA` sem delta têm o mesmo tipo e estado dos três com delta. O motivo individual dos cinco é [nao verificado]. `app/folha/export.py:71,725,740,866,968,994,1013`, `f5_6a1_v2_main.out:12–19`.

## 3. Qual dado cada caminho usa em competência entregue

| Caminho | Fonte e ponto de decisão |
|---|---|
| **TXT de hoje medido pela sonda** | `txt_vigente_x_gravado` passa sem `fechamentos=`. `classificar_export` e `montar_linhas_export` leem `FechamentoMensal` do banco. As horas vêm desse fechamento. A porta do espelho lê célula gravada, outros crivos consultam batidas e cadastro, e `datas_falta` lê `Ausencia` na hora. `competencia_entregue` seleciona o registro vigente para a comparação de hash. A chamada ao montador é a mesma com ou sem vigente. `app/folha/export.py:556,654,673,714,730,740,782,791,914,1247`. |
| **Botão `Gerar TXT`** | `gerar_dominio_txt` confere a porta de exportação e a existência de fechamentos gravados elegíveis. Em seguida chama `recalcular_fechamento_mes(somente_leitura=True, retornar_objeto=True)` e injeta os objetos recalculados em `classificar_export` e `montar_linhas_export`. O recálculo consulta a cobertura atual de ausências. A escolha dessa fonte é da view, inclusive se já existe TXT entregue. Se a geração prossegue, a view registra uma exportação. `app/folha/views.py:130,149,165,209,214,270,299`, `app/ponto/services/fechamento.py:539,680,1128`. |
| **Download de uma exportação histórica** | `baixar_exportacao` serve `ExportacaoDominio.conteudo` do registro solicitado. `app/folha/views.py:569,575,582`. |

## 4. Censo de leitura da frota

`classificar_export` responde **quem passa pela porta e com qual motivo**, sem montar linhas. `montar_linhas_export` responde **quais linhas saem** e aceita uma lista de fechamentos. `txt_vigente_x_gravado` e `vigia_txt.q1_vigente_x_gravado` comparam com a entrega histórica. Entre esses leitores, nenhum devolve o efeito causal de tirar uma falta do TXT de hoje. O efeito **direto** pode ser medido chamando o montador real duas vezes para um único fechamento, com a mesma foto do banco, omitindo as faltas candidatas apenas da consulta de ausências na segunda montagem. `app/folha/export.py:631,690,714,765,782`, `app/folha/services/vigia_txt.py:207,231,244,254`.

O universo abaixo é cada `(empresa, ano, mês)` que `competencia_entregue` confirma, incluindo competências com zero casos. Dentro da tripla, entram os fechamentos aptos à montagem com pelo menos uma `falta/aprovada` que intersecta a janela. A unidade é o colaborador por tripla, e o total da frota deduplica PKs entre competências. A omissão é **conjunta por colaborador e competência**, inclusive quando duas faltas sustentam o mesmo bloqueio. A sonda mede a contribuição direta das linhas de `Ausencia` com as células gravadas da foto. O efeito histórico de desfazer a lavra que o ato possa ter provocado fica [nao verificado]. `app/chamados/services/materializacao.py:1095`, `app/folha/export.py:47,71,556,687,717,1013,1247`, `app/ponto/models.py:481,485`, `app/ponto/signals.py:143`.

Em uma montagem integral, o classificador percorre os fechamentos da empresa/competência, carrega os dados em lote e faz leituras adicionais por colaborador. O montador percorre os fechamentos elegíveis, resolve eventos e falta para quem entra e guarda as linhas em memória. O comparador ainda serializa e calcula o SHA-256. Duas fotos integrais repetem esse trabalho. O corpo abaixo restringe as duas montagens a cada fechamento candidato e dispensa serializar e hashear o TXT da empresa. Tempo e volume reais na frota são [nao verificado]. `app/folha/export.py:525,542,654,712,725,782,805,994,1247`.

Corpo para um shell Django já na sombra, apresentado sem execução [nao verificado]. A guarda confere configuração, banco e schema em transação `READ ONLY`. O lançador `--rodar` do anexo faz a lavra e grava a saída. `app/config/settings/sombra.py:29,47,60`, `bin/sombra.sh:136,151,157,483,489,492`.

```python
import json
from collections import Counter
from unittest.mock import patch

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone

from chamados.services.materializacao import ESCAPES_QUE_VIRAM_AUSENCIA
from folha import export as E
from folha.models import ExportacaoDominio
from ponto.models import FechamentoMensal


def medir_fechamento(empresa, mes, ano, fech, pks_omitidos):
    leitor = E.ausencias_na_janela
    com_faltas, _ = E.montar_linhas_export(empresa, mes, ano, fechamentos=[fech])

    def leitor_sem_faltas(*args, **kwargs):
        return leitor(*args, **kwargs).exclude(pk__in=pks_omitidos)

    with patch.object(E, "ausencias_na_janela", leitor_sem_faltas):
        sem_faltas, _ = E.montar_linhas_export(empresa, mes, ano, fechamentos=[fech])

    retiradas = Counter(sem_faltas) - Counter(com_faltas)
    adicionadas = Counter(com_faltas) - Counter(sem_faltas)
    so_perde_tipo_10 = (
        bool(retiradas)
        and not adicionadas
        and all(linha.startswith("10") for linha in retiradas)
    )
    return com_faltas != sem_faltas, so_perde_tipo_10


def contagem(pks):
    return {"n": len(pks), "colaborador_pks": sorted(pks)}


def censo():
    if not (
        settings.SETTINGS_MODULE == "config.settings.sombra"
        and getattr(settings, "HASNER_SOMBRA", False) is True
        and set(settings.DATABASES) == {"default"}
        and connection.vendor == "postgresql"
        and connection.settings_dict["NAME"] == "sombra"
        and connection.settings_dict["HOST"] == "sombra_db"
        and getattr(connection, "schema_name", None) == "juliani"
        and not connection.in_atomic_block
        and connection.get_autocommit()
    ):
        raise RuntimeError("GuardaSombra")

    tipo, estado = ESCAPES_QUE_VIRAM_AUSENCIA["FALTA_DIA"]
    por_competencia = []
    frota_muda, frota_perde_10 = set(), set()
    incomparaveis = 0

    with timezone.override(settings.TIME_ZONE), transaction.atomic():
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            cursor.execute("SELECT pg_catalog.current_database(), pg_catalog.current_schema()")
            if cursor.fetchone() != ("sombra", "juliani"):
                raise RuntimeError("GuardaSombra")

        triplas = list(
            ExportacaoDominio.objects.order_by("empresa_id", "ano", "mes")
            .values_list("empresa_id", "ano", "mes").distinct()
        )
        for empresa_pk, ano, mes in triplas:
            vigente = E.competencia_entregue(empresa_pk, mes, ano, registro=True)
            if vigente is None:
                continue
            empresa = vigente.empresa
            base = {"empresa_pk": empresa_pk, "ano": ano, "mes": mes}
            try:
                E.codigo_no_sistema_dominio(empresa)
            except ValueError:
                incomparaveis += 1
                por_competencia.append({
                    **base, "status": "INCOMPARAVEL", "motivo": "sem_integracao_dominio",
                })
                continue

            ini, fim = E.janela_competencia(empresa, mes, ano)
            fechs = FechamentoMensal.objects.filter(
                colaborador__empresa_id=empresa_pk, ano=ano, mes=mes,
                status__in=E.STATUS_NO_TXT,
            ).select_related("colaborador__empresa").order_by("pk")

            candidatas, muda, perde_10 = set(), set(), set()
            for fech in fechs.iterator(chunk_size=200):
                pks = tuple(
                    E.ausencias_na_janela(fech.colaborador, tipo, ini, fim)
                    .filter(status=estado).order_by("pk").values_list("pk", flat=True)
                )
                if not pks:
                    continue
                pk = fech.colaborador_id
                candidatas.add(pk)
                mudou, perdeu_10 = medir_fechamento(empresa, mes, ano, fech, pks)
                if mudou:
                    muda.add(pk)
                if perdeu_10:
                    perde_10.add(pk)

            frota_muda.update(muda)
            frota_perde_10.update(perde_10)
            por_competencia.append({
                **base, "status": "COMPARADA",
                "com_falta_aprovada": contagem(candidatas),
                "muda_txt": contagem(muda),
                "so_perde_tipo_10": contagem(perde_10),
            })

    return {
        "competencias_incomparaveis": incomparaveis,
        "frota_confirmada": {
            "muda_txt": contagem(frota_muda),
            "so_perde_tipo_10": contagem(frota_perde_10),
        },
        "por_empresa_competencia": por_competencia,
    }


try:
    print(json.dumps(censo(), sort_keys=True))
except Exception as erro:
    print(json.dumps({"status": "INCONCLUSIVO", "erro": type(erro).__name__}))
    raise SystemExit(1) from None
```

## 5. O que fica em aberto

- [nao verificado] O motivo exato de retenção de cada caso, os vereditos da célula antes e depois e a configuração efetiva de `falta_diurna` e do catálogo homologado. A saída anexa mede o delta de linhas e não expõe esses insumos. `f5_6a1_v2_main.out:12–19`, `app/folha/export.py:968,974,1013`.
- [nao verificado] As rubricas e valores das linhas retiradas, se ainda havia linha com a mesma matrícula e quais eram os dias e competências dos casos. A saída publica contagens por tipo, PKs e hashes abreviados. `f5_6a1_v2_main.out:14,15,16`, `app/folha/export.py:34,246,674`.
- [nao verificado] A contagem efetiva da frota, a origem de todas as faltas existentes, o resultado do botão para esses três casos e qualquer alteração de valor pago ou de arquivo importado no Domínio. As `15.715` do resumo são perguntas candidatas da sonda original. Foram exercitadas oito perguntas em 16 tentativas. `f5_6a1_v2_main.out:3`, `f5_6a1_v2_f5.out:3`, `app/folha/views.py:130,209,270,299`.

## 20/11 19:54 · sessao 63ff5f06c7e3 · CADEIA-20-PASSO-1 · censo · origem=Code

# CADEIA-20-PASSO-1 — censo estático do desenho A

O censo encontrou **64 sítios de escrita**, dos quais **61 têm entrada externa identificada**, **27 são alcançáveis durante um callback de `on_commit`** e **33 são alcançáveis por receiver ou fila**. A ficha prevê cobertura para pelo menos um caminho de **16 sítios** e não descreve cobertura para os **48 restantes**. O cruzamento exato com a F5 dá **8 arquivos no perímetro censado**, dos quais **4 constam da estimativa de alteração da ficha**. As contagens usam os IDs das tabelas e o critério abaixo. `r.txt:574–587` `arquivos_da_f5.txt:1–119` `app/core/censo_escritas.py:6–56`

A identidade Git da cópia é **[nao verificado]**. O diretório tem o nome do hash fornecido, mas `git rev-parse HEAD` devolveu `not a git repository`. As localizações executáveis abaixo são da cópia lida. Por exemplo, nela `julgar_colab` começa na linha 959 e chama o E2 na 1027. As referências `r.txt` são da ficha anexada. `app/ponto/services/cartorio.py:959,1019–1027` `r.txt:574–587`

## 1. Escritores, entradas e cobertura

**Como reproduzir.** Rode o scanner abaixo. Para cada ocorrência, confirme o modelo, os campos e a operação persistida. Revise também `entidade=None`, que contém o `save` genérico da célula e três nascimentos por variável ou alias. Nos seis ramos que delegam a `salvar_e_emitir`, conte a chamada de cada operação, pois os campos e as entradas diferem. Conte os demais sítios pelo `save`, `INSERT`, `UPDATE`, `DELETE` ou operação ORM equivalente. Siga referências aos nomes das funções com `rg -n`, incluindo imports com alias, até `urls.py`, `handle`, `config/crons.py`, registro de signal ou consumidor. Inclua `management/` e os chamadores em `bin/`. Use o filtro de código de produção do scanner e confirme caminhos que apenas simulam ou revertem antes de atribuir uma entrada persistente. `app/core/censo_escritas.py:6–82,451–492` `app/ponto/portas/celula.py:957–965` `app/chamados/models.py:1030,1154,3209`

```sh
PYTHONPATH=app python3 -B -c '
from core.censo_escritas import varrer_arvore
alvos = {"CelulaDia", "FuroDiario", "ChamadoColaborador", "DisputaSupervisao",
         "PerguntaDisputa", "DecisaoPergunta", None}
for x in varrer_arvore("app"):
    if x["entidade"] in alvos:
        print(x)
'
```

O recorte conta as operações de negócio sobre `CelulaDia`, `FuroDiario`, o estado e a junção da cobrança, o ciclo da disputa e a identidade, o carimbo e a decisão da pergunta. Inclui os motivos e o contexto que o repasse ou a fábrica persistem como parte da abertura ou da costura. As cinco presenças da ficha são leituras calculadas. `EventoCelula` é a fila que liga os atos. A trilha de auditoria, a Pauta DP, o véu sem mudança de estado e o fechamento financeiro são efeitos adjacentes ao recorte. `app/ponto/portas/celula.py:7–21,945–965,1241–1269` `app/ponto/services/cartorio.py:1413–1433,1493–1503,1538–1539` `app/chamados/services/disputa_emissao.py:1095–1099,1263–1284` `app/chamados/services/fabrica_da_celula.py:201–219,268–284,354–373` `app/chamados/services/arquivo.py:141–173` `app/ponto/services/fechamento.py:1798–1824`

Nas tabelas, **X** significa que existe entrada iniciada fora do cartório, inclusive quando o ato externo depois entra nele. **C** significa que existe uma pilha síncrona que chega ao sítio durante um callback de `on_commit`. **E** significa que existe caminho por signal ou fila. As letras marcam possibilidade estática por sítio, e podem coexistir com a entrada síncrona do cartório. A coluna **A** distingue o trecho que a ficha cobre da cobertura das outras chegadas. `app/ponto/services/cartorio.py:959–1027,2059–2096` `app/chamados/signals.py:122–146,189–218,229–259,351–371` `r.txt:580–585`

### Entradas compartilhadas

| Entrada | Cadeia executável |
|---|---|
| **J — cartório em lote** | Cron `processar_cartorio --apply` → `handle` → `julgar_colab` → corpo, E2, fechos adiados, segunda passada, cluster, fábrica e releitura. A via D5 é `lavrar_status_competencia` → `julgar_colab(completar_status=True)` → `_completar_status`. `app/config/crons.py:616` `app/ponto/management/commands/processar_cartorio.py:65` `app/ponto/management/commands/lavrar_status_competencia.py:56` `app/ponto/services/cartorio.py:940–955,1016–1073,1101–1114` |
| **U — cartório unitário** | `julgar_celula` → `julgar_colab` → precedência após o retorno. Entram os receivers de batida, ausência, justificativa e chamado, o vigia de hora, cobrar dia, pós-vínculo, regeneração e reemissão C2. Também entram os comandos de rejulgamento e de correção. `app/ponto/services/cartorio.py:2059–2096` `app/ponto/signals.py:85–93,131–165` `app/chamados/signals.py:189–218` `app/config/crons.py:367` `app/ponto/services/vigia_de_hora.py:134` `app/chamados/views_cobrar.py:327` `app/chamados/services/cobrar_dia.py:41` `app/colaboradores/services/pos_vinculo.py:73` `app/ponto/portas/celula.py:472` `app/chamados/reconciliador.py:752,970` `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:78` `app/ponto/management/commands/curar_residuo_bug115.py:146` |
| **J/U — outras chamadas de manutenção** | O command de veredito velho, o de régua de defesa, a cura de furo sem cobrança e o script pontual também chamam a porta do cartório. `app/ponto/management/commands/celula_veredito_velho.py:144` `app/escala/management/commands/desfazer_corroboracao_regua.py:44` `app/escala/regua_defesa.py:695,728` `app/ponto/services/furos_sem_cobranca.py:128` `bin/rejulgar_o118_col221.py:24` |
| **G — geração e regeneração** | Cron `gerar_celulas` → command → portas de geração. Também chegam saves de vínculo, calendário, reparentamento de folga, serviços de alteração de vínculo ou de template e commands corretivos. `app/config/crons.py:426` `app/ponto/management/commands/gerar_celulas.py:323,349` `app/escala/signals.py:34–134,185–204` `app/core/signals.py:28–43` `app/escala/folgas.py:293–298,523–541` `app/colaboradores/services/vinculo.py:321–340,657–662` `app/escala/services/cadastro_tipo.py:323,353,376` `app/ponto/management/commands/aplicar_col900.py:234` |
| **M — emissor** | Corpo do cartório → `_abrir_chamado` ou `_espelhar_marcos`. Por fora, cron `detectar_ausencias` → `handle` e command `emitir_furo_retroativo` → `_abrir_chamado`. O próprio detector também tem os fechos por afastamento, cobertura e batida. `app/ponto/services/cartorio.py:1839,1877,1888,1897` `app/config/crons.py:364` `app/ponto/management/commands/detectar_ausencias.py:61,440,544–569,694–745,821–854` `app/ponto/management/commands/emitir_furo_retroativo.py:154` |
| **R — repasse** | M → `_abrir_chamado` → repasse; também os commands de alerta de turno e avançado, a ação web de iniciar órfãos, cobrar dia **após** o retorno do cartório e o command `perguntar_chamados_sem_pergunta`. `app/ponto/management/commands/detectar_ausencias.py:788–807` `app/config/crons.py:362–364` `app/ponto/management/commands/processar_alertas_turno.py:385` `app/ponto/management/commands/processar_alertas_avancados.py:159` `app/chamados/urls.py:21,32,39` `app/chamados/views.py:674,954` `app/chamados/services/acoes_disputa.py:226` `app/chamados/services/cobrar_dia.py:41–49` `app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:19–29` `app/chamados/services/pergunta_no_app.py:261` |
| **F — fábrica** | J/U → `perguntar_pela_celula` → `perguntar` → descarimbo, disputa e geração de perguntas. Também entram Fila e `backfill_fio_morto_e4b`. A porta de criação/agregação de disputa recebe ainda entrada de API, browser e command de foto ausente. `app/ponto/services/cartorio.py:1061–1073` `app/chamados/services/fabrica_da_celula.py:160–174,206–219,288–301` `app/chamados/management/commands/backfill_fio_morto_e4b.py:125–143,316` `app/api/views.py:768` `app/api/views_core.py:762` `app/ponto/views.py:352` `app/ponto/services/triagem_batida.py:734` `app/ponto/management/commands/marcar_foto_ausente_retro.py:96` |
| **E2 — absolvição pela célula** | J/U → E2 antes do fecho adiado. Também Batida `post_save` ou filho terminal pós-commit → `reconciliar_chamados_colab` → E2. Fila chama E2 diretamente. E2 → carimbar pergunta e, se C7 permitir, fechar disputa → receivers do pai e S144. `app/ponto/services/cartorio.py:1019–1049` `app/ponto/signals.py:206–215` `app/chamados/signals.py:19–46,149–165,351–371` `app/chamados/reconciliador.py:634–667,1416,1483` |
| **Fila — `EventoCelula`** | Cron ou command → `consumir_eventos_celula` → rejunção, E2, retratação por célula ou premissa, carimbo de órfãs, fecho por lastro e fábrica. `app/config/crons.py:394` `app/chamados/management/commands/consumir_eventos_celula.py:26–31` `app/chamados/reconciliador.py:1366–1438` |
| **H — atos humanos de pergunta e dia** | Views HTML/API de resposta e correção → portas da pergunta. Ações da mesa → validação, recusa, materialização ou abertura de conversa. View `resolver_dia` → fecha fio e lavra pela via admin. `app/chamados/views.py:1278,1307,1329` `app/api/views.py:2647,2706` `app/chamados/services/acoes_disputa.py:19,193,263–267,375–383` `app/ponto/urls.py:58` `app/ponto/views.py:3466,3494` `app/ponto/services/veredito_celula.py:190,347–352` |

**10 linhas de entradas compartilhadas.** A expansão começa por `julgar_colab`, `julgar_celula`, `_abrir_chamado`, `criar_disputa_a_partir_de_chamados_orfaos`, `perguntar_pela_celula`, `retratar_perguntas_absolvidas_por_celula` e `consumir_eventos_celula`. Os IDs abaixo remetem a essas cadeias e citam a chamada específica. `app/ponto/services/cartorio.py:959,2059` `app/chamados/reconciliador.py:599,1366` `app/chamados/services/fabrica_da_celula.py:178` `app/chamados/services/disputa_emissao.py:1041`

### 1a. Célula e projeção `FuroDiario`

| ID | Sítio e função | Entrada, flags | A |
|---|---|---|---|
| 01 | `app/ponto/portas/celula.py:1066` `lavrar_veredito`, ata/status sob veredito admin | D5 pelo cartório, `app/ponto/services/cartorio.py:940–955`. Sem flags. | A ficha não diz sobre D5. `r.txt:580,585` |
| 02 | `app/ponto/portas/celula.py:1083` `lavrar_veredito`, só status | D5 e a porta humana quando apenas o status difere. **X**. `app/ponto/services/cartorio.py:953` `app/ponto/services/veredito_celula.py:352` | A ficha não diz. `r.txt:580,585` |
| 03 | `app/ponto/portas/celula.py:1098` `lavrar_veredito`, veredito e campos associados | H → `resolver_dia` → lavra admin. **X**. `app/ponto/views.py:3494` `app/ponto/services/veredito_celula.py:352` | A ficha não diz sobre a lavra admin. `r.txt:580,585` |
| 04 | `app/ponto/portas/celula.py:1167` `desfazer_julgamento_da_futura` | G ou reversão → apaga julgamento do DNA anterior. **X E**. `app/ponto/portas/celula.py:267,314,842` `app/escala/signals.py:34–83` | A ficha não diz. `r.txt:580,585` |
| 05 | `app/ponto/portas/celula.py:1201` `carimbar`, incluindo a lavra preparada com `save=False` | J/U → carimba `nunca_bateu` ou o ramo normal. **X C E**. `app/ponto/services/cartorio.py:1321–1345,1901–1922` `app/chamados/signals.py:134–146` `app/ponto/signals.py:131–165` | **Sim** para a lavra do cartório. A ficha deixa as origens externas e os callbacks em aberto. `r.txt:580,585` |
| 06 | `app/ponto/portas/celula.py:1216` `carimbar_precedencia`, chave da ata | U → `aplicar_precedencia_na_celula` depois de voltar de `julgar_colab`. **X C E**. `app/ponto/services/cartorio.py:2046–2055,2088–2096` `app/chamados/signals.py:134–146,218` | A ficha não diz sobre esse save posterior. `r.txt:580,585` |
| 07 | `app/ponto/portas/celula.py:265` `regenerar_celulas_vinculo`, futura fora da vigência | G → ramo sem vínculo. **X E**. `app/escala/signals.py:73–81` | A ficha não diz. `r.txt:580,585` |
| 08 | `app/ponto/portas/celula.py:279` `regenerar_celulas_vinculo`, dia até hoje fora da vigência | G → atualiza `trabalha` e DNA. **X E**. `app/escala/signals.py:73–81` | A ficha não diz. `r.txt:580,585` |
| 09 | `app/ponto/portas/celula.py:293` `regenerar_celulas_vinculo`, INSERT | G, cron de geração ou signal de vínculo. **X E**. `app/ponto/management/commands/gerar_celulas.py:323` `app/escala/signals.py:73–81,125–132` | A ficha não diz. `r.txt:580,585` |
| 10 | `app/ponto/portas/celula.py:310` `regenerar_celulas_vinculo`, regrava geradora/DNA | G, inclusive alteração de âncora por signal. **X E**. `app/escala/signals.py:86–134` | A ficha não diz. `r.txt:580,585` |
| 11 | `app/ponto/portas/celula.py:638` `editar_dia`, INSERT | `trocar_plantao` chama em `app/ponto/portas/celula.py:713`. Entrada de produção anterior a essa porta **[nao verificado]**. | A ficha não diz. `r.txt:580,585` |
| 12 | `app/ponto/portas/celula.py:647` `editar_dia`, UPDATE | `trocar_plantao` chama em `app/ponto/portas/celula.py:713`. Entrada de produção anterior a essa porta **[nao verificado]**. | A ficha não diz. `r.txt:580,585` |
| 13 | `app/ponto/portas/celula.py:836` `reverter_regeneracao_dia` | Command `aplicar_col900`. **X**. `app/ponto/management/commands/aplicar_col900.py:186,198–201` | A ficha não diz. `r.txt:580,585` |
| 14 | `app/ponto/portas/celula.py:914` `gerar_celulas_periodo`, INSERT | G ou command corretivo. **X**. `app/ponto/management/commands/gerar_celulas.py:349` `app/ponto/management/commands/curar_residuo_bug115.py:138` | A ficha não diz. `r.txt:580,585` |
| 15 | `app/ponto/portas/celula.py:926` `gerar_celulas_periodo`, UPDATE de DNA | G ou command corretivo. **X**. `app/ponto/management/commands/gerar_celulas.py:349` `app/ponto/management/commands/curar_residuo_bug115.py:138` | A ficha não diz. `r.txt:580,585` |
| 16 | `app/ponto/portas/celula.py:1237` `carimbar_insumos_em_lote` | Command `backfill_b5_insumos`. **X**. `app/ponto/management/commands/backfill_b5_insumos.py:81,84` | A ficha não diz. `r.txt:580,585` |
| 17 | `app/ponto/portas/celula.py:1258` `gravar_furos`, UPSERT | Cron encadeado `apurar_furos_diarios` ou pós-vínculo/view `resolver_dia`. **X**. `app/config/crons.py:1527` `app/escala/management/commands/apurar_furos_diarios.py:13` `app/escala/services/furos_diarios.py:108` `app/colaboradores/services/pos_vinculo.py:85` `app/ponto/views.py:3505` | A ficha não diz. `r.txt:580,585` |
| 18 | `app/ponto/portas/celula.py:1268` `gravar_furos`, DELETE | Mesmas entradas do UPSERT, com recorte `apagar_em`. **X**. `app/ponto/portas/celula.py:1241–1269` `app/escala/services/furos_diarios.py:108` `app/colaboradores/services/pos_vinculo.py:85` | A ficha não diz. `r.txt:580,585` |

**18 linhas**, correspondentes a **13 sítios ORM** quando se reúne a persistência comum de 01–06 em `salvar_e_emitir`. `app/ponto/portas/celula.py:957–965,1066,1083,1098,1167,1201,1216`

### 1b. Chamado, disputa e pergunta

| ID | Sítio e função | Entrada, flags | A |
|---|---|---|---|
| 19 | `app/chamados/models.py:302` `_gravar_estado` | J/U → fechos direto e adiado, cluster, M/R, E2 → pai, Fila → lastro/retratação, H. **X C E**. `app/ponto/services/cartorio.py:1034,1070,1581,1599,1620,1699,1714` `app/ponto/services/cluster_espurio.py:190` `app/chamados/services/disputa_emissao.py:1099,1173,1192,1237,1266` `app/chamados/signals.py:43,365–371` `app/ponto/services/lastro.py:401` | **Sim** no fecho do pai no E2, na ordenação de `analisar` do repasse e no relógio A1. A ficha não diz sobre cluster, os demais ramos e as origens externas. `r.txt:581–585` |
| 20 | `app/chamados/models.py:1030` `ChamadoColaborador.abrir`, `get_or_create` | M, F e R abrem filho ou pai. J/U também abre cluster. Fila pode chegar pela fábrica e o callback de regularização pode chegar por Batida → U. **X C E**. `app/ponto/management/commands/detectar_ausencias.py:694` `app/chamados/services/disputa_emissao.py:582,1118` `app/ponto/services/cluster_espurio.py:196` `app/chamados/signals.py:134–146` `app/ponto/signals.py:131–135` | **Sim** no emissor cercado. A ficha não diz sobre cluster e as outras origens. `r.txt:582,585` |
| 21 | `app/chamados/models.py:1154` `ChamadoColaborador.criar`, INSERT | Views/API de abertura, command de alertas avançados e portas de abertura. **X**. `app/chamados/views.py:136,197` `app/chamados/services/abertura.py:47,172` `app/api/views.py:2189` `app/ponto/management/commands/processar_alertas_avancados.py:121` | A ficha não diz sobre esta porta de nascimento. `r.txt:582,585` |
| 22 | `app/chamados/services/contexto.py:104` `renovar_contexto` | M → `_espelhar_marcos`, ressalva e agrupamento de limbo. Também command `reconciliar_grade`. **X C E**. `app/ponto/management/commands/detectar_ausencias.py:559,762–767` `app/chamados/services/limbo_folgas.py:250–280` `app/ponto/management/commands/reconciliar_grade.py:473` | **Sim** para o espelho cercado. A ficha não diz sobre os outros contextos. `r.txt:582,585` |
| 23 | `app/chamados/services/contexto.py:163` `fundir_contexto` | R/F → ocorrência → funde os IDs dos órfãos ou o contexto de recorrência no pai. **X C E**. `app/chamados/services/disputa_emissao.py:600,1136,1371` `app/chamados/services/nota.py:110–123` | A ficha não diz sobre o save do contexto do pai. `r.txt:582,585` |
| 24 | `app/chamados/services/contexto.py:127` `retirar_contexto` | Command `reconciliar_grade` retira a marca `orfa_regeneracao` da cobrança. **X**. `app/ponto/management/commands/reconciliar_grade.py:454,474` | A ficha não diz. `r.txt:582,585` |
| 25 | `app/chamados/services/vinculos.py:101` `apontar_celula` | Fila ou callback de regeneração → `rejuntar_celula_dos_vivos`. Também command de carimbo por batida. **X C E**. `app/chamados/reconciliador.py:1415` `app/ponto/portas/celula.py:531–540` `app/chamados/rejuncao.py:98–107` `app/chamados/management/commands/carimbar_por_batida_coincidente.py:100` | A ficha não diz. `r.txt:582,585` |
| 26 | `app/chamados/models.py:2099` `DisputaSupervisao.fechar` | E2, R, Fila, S143, beco pós-commit e ato humano. **X C E**. `app/chamados/reconciliador.py:662,1222` `app/chamados/services/disputa_emissao.py:1298,2114–2131` `app/chamados/services/becos.py:55–66` `app/chamados/models.py:2106–2113` | **Sim** para pôr o efeito do E2 em perímetro próprio. A ficha não diz sobre o beco e os demais ramos. `r.txt:581,585` |
| 27 | `app/chamados/models.py:2324` `reabrir_admin` | H → ação de abrir conversa com motivo novo. **X**. `app/chamados/services/acoes_disputa.py:263–267` | A ficha não diz. `r.txt:582,585` |
| 28 | `app/chamados/models.py:2340` `reabrir_sistema` | Ressuscitação, F → descarimbo com reabertura habilitada, e callback da ausência rejeitada. **X C E**. `app/chamados/models.py:1700–1712` `app/chamados/services/relavra_via.py:259–261` `app/ponto/signals.py:254–272` `app/chamados/services/materializacao.py:1173–1185` | A ficha não diz sobre o save que reabre a disputa. `r.txt:581–585` |
| 29 | `app/chamados/models.py:2403` `reabrir_por_colab`, contador persistido no ato de reabrir o questionário | H, views HTML/API → correção → descarimbo em lote no mesmo atomic. **X**. `app/chamados/views.py:1329` `app/api/views.py:2706` `app/chamados/models.py:2391–2403` | A ficha não diz. `r.txt:585` |
| 30 | `app/chamados/models.py:2131` `aparar_motivo_respondido` | API de foto recebida → retira motivo já respondido por essa via. **X**. `app/api/foto_recebida.py:39–74` | A ficha não diz. `r.txt:582,585` |
| 31 | `app/chamados/models.py:2228` `remover_motivo` | Signals de vínculo/colaborador → retratação por identidade → atualiza motivos e pode fechar. **X E**. `app/escala/signals.py:154–181` `app/colaboradores/signals.py:49–82` `app/chamados/models.py:2217–2235` | A ficha não diz. `r.txt:581–585` |
| 32 | `app/chamados/services/disputa_emissao.py:601` `_abrir_ou_agregar_disputa`, INSERT da fábrica | F e as entradas API/browser/command da mesma porta. **X C E**. `app/chamados/services/fabrica_da_celula.py:172–174` `app/chamados/services/disputa_emissao.py:641,676–699` | A ficha não diz sobre o INSERT do contêiner da fábrica. `r.txt:581–582,585` |
| 33 | `app/chamados/services/disputa_emissao.py:1137` `criar_disputa_a_partir_de_chamados_orfaos`, INSERT da disputa | R → abre pai e disputa antes das perguntas. **X C E**. `app/chamados/services/disputa_emissao.py:1065–1073,1117–1142` `app/ponto/management/commands/detectar_ausencias.py

[truncado em 20000 de 46561 caracteres]

_7620 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
