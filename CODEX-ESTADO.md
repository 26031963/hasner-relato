# CODEX-ESTADO

> Ultima resposta processada: **21/11/2026 18:56** · publicado em **21/11/2026 18:56** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 21/11 18:56 · sessao 4f6a2bf25e06 · E4C-F5-CONSTROI · construtor · origem=Code

Implementei a cura do item 1.2 no laço do passivo. `reparar` fotografa os sete valores no laço de classificação e os reconfirma após `travar_o_ato_do_plantio`, dentro do mesmo `atomic`. Qualquer divergência recusa o grupo inteiro em `escopo_do_plantio_mudou` antes de chamar a unidade de trabalho. Classe e slot saem da mesma função nas duas leituras. [Foto e trava](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/passivo_validado.py:362) · [Comparações](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/passivo_validado.py:456) · [Classificador](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/vigia_carimbo.py:65)

**K6 e dry.** A leitura inicial usa `_esta_exportada`. Para o último TXT invalidado sem substituto, `haiku_export` ainda preenche `vigente`, enquanto K6 devolve `False`. Nesse caso, o dry exibe `exportada=True` e o apply recusa o grupo após reler pelo K6. Uma troca na leitura inicial mudaria a marcação de exportada no dry. A incidência desse caso em produção é [nao verificado]. `chamados/services/passivo_validado.py:368` · `:376` · `:404` · `folha/haiku_export.py:69` · `:74` · `folha/export.py:588` · `core/juizes.py:342`

### Cinco perguntas por campo

| Campo | Fonte | Escritor | Instante | Leitores | Significado |
|---|---|---|---|---|---|
| Composta | Sequência de decisões, texto projetado, carimbo e via. `chamados/models.py:4382` | Resposta, assinatura, devolução e carimbo. `chamados/services/disputa_emissao.py:1532` · `chamados/services/validacao.py:161` · `:269` · `chamados/services/materializacao.py:2042` | Foto e reconfirmação. `chamados/services/passivo_validado.py:363` · `:457` | Guarda do grupo e eleição da irmã. `chamados/services/passivo_validado.py:457` · `chamados/services/materializacao.py:153` | Estado efetivo de resposta, validação e materialização. `chamados/models.py:4385` |
| Classe | Juiz de materialização, slot esperado e batida vinculada viva. `chamados/services/vigia_carimbo.py:65` | O vigia deriva a classe. Plantio, vínculo e retratação alteram a evidência. `chamados/services/materializacao.py:1531` · `:1588` · `ponto/registro_batida.py:287` | Classificação e releitura. `chamados/services/passivo_validado.py:335` · `:459` | Censo e seleção do passivo. `chamados/services/vigia_carimbo.py:129` · `chamados/services/passivo_validado.py:337` | Presença de registro ou motivo da falta dele. `chamados/services/vigia_carimbo.py:70` |
| `ts` | Derivador de `hora` e parser de `data_hora`. `chamados/juizes.py:344` | O slot é derivado da resposta e dos dados do fato e do marco. `chamados/services/disputa_emissao.py:1532` · `chamados/services/fio.py:1096` · `chamados/services/materializacao.py:701` | Junto da classe, nas duas leituras. `chamados/services/passivo_validado.py:335` · `:459` | Juiz de materialização e derivação do dia. `chamados/juizes.py:757` · `chamados/services/passivo_validado.py:118` | Instante esperado da batida. `chamados/services/vigia_carimbo.py:89` |
| Dia | `data_evento` ou a data local do slot. `chamados/services/passivo_validado.py:118` | Emissão e rechaveamento gravam `data_evento`. `chamados/services/disputa_emissao.py:453` · `chamados/services/fio.py:1096` | Antes do agrupamento e após a trava. `chamados/services/passivo_validado.py:352` · `:462` | Competência e geometria diária. `chamados/services/passivo_validado.py:353` · `:522` · `:574` | Dia do turno usado pelo passivo. `chamados/services/passivo_validado.py:103` |
| Competência | Mês e ano do fim da janela que contém o dia. `chamados/services/passivo_validado.py:91` · `ponto/janelas.py:60` | O cadastro grava o corte da empresa. `colaboradores/views.py:539` · `colaboradores/services/estrutura.py:18` | Na classificação e com a empresa relida sob a trava. `chamados/services/passivo_validado.py:353` · `:399` · `:463` | Recorte, agrupamento e regeneração. `chamados/services/passivo_validado.py:358` · `:371` · `:500` | Competência à qual pertence o dia do turno. `chamados/services/passivo_validado.py:91` |
| Exportada | Histórico do TXT na foto e K6 na releitura. `folha/haiku_export.py:59` · `folha/export.py:556` | `ExportacaoDominio.registrar` e `invalidar`. `folha/models.py:174` · `:143` | Na foto da classificação e após a trava. `chamados/services/passivo_validado.py:368` · `:404` | Relato e decisão de regenerar. `chamados/services/passivo_validado.py:377` · `:491` | Presença de TXT na leitura histórica e entrega vigente no K6. `folha/haiku_export.py:74` · `folha/export.py:591` |
| `data_conferida_em` | Coluna da pergunta. `chamados/models.py:2668` | `conferir_data_do_fato`. `chamados/services/validacao.py:1014` | Na foto da classificação e na pergunta relida sob a trava. `chamados/services/passivo_validado.py:369` · `:464` | Guardas de data da avulsa e da irmã. `chamados/services/materializacao.py:171` · `:1949` | Instante da confirmação humana do dia em disputa. `chamados/services/validacao.py:975` · `:1014` |

### Diff e verificações

| Arquivo | Adições | Remoções |
|---|---:|---:|
| `chamados/services/passivo_validado.py:362` | 34 | 7 |
| `chamados/services/vigia_carimbo.py:65` | 18 | 13 |
| `chamados/tests/test_e4c_f5_passivo_releitura.py:1` | 209 | 0 |
| `core/tests/test_medida_turno_do_par.py:270` | 5 | 5 |
| `ponto/tests/test_cadastro_futuro_c5e.py:204` | 10 | 2 |
| **Total** | **276** | **27** |

- **`except` adicionados: 0 em produção e 0 em testes.** A divergência usa a captura já existente. `chamados/services/passivo_validado.py:410` · `:456` · `chamados/tests/test_e4c_f5_passivo_releitura.py:48`
- **Catraca de linhas:** o censo reproduziu as quatro entradas do selo na base `b0935c1f1` e os quatro deslocamentos previstos no HEAD. Atualizei as posições das mesmas ocorrências, conforme a igualdade exigida pelo selo. Rodada em memória: `Ran 35 tests in 39.067s`, `OK`, RC 0. `core/tests/test_medida_turno_do_par.py:17` · `:271` · `:558`
- **C5E:** completei o stub da trava e da releitura para o controle que chega ao plantio. `Ran 30 tests in 9.631s`, `OK`, RC 0. `ponto/tests/test_cadastro_futuro_c5e.py:204` · `:238`
- **RED e mutantes:** o teste congela o relógio nas duas threads, exige PIDs distintos e aguarda o commit da outra conexão antes da trava real. No caso que isola o dia, a trava real recebe o escopo estrutural atualizado para que a comparação de `data_evento` já existente não masque a nova guarda. O caso de slot derivado do contexto também muda dia e competência e usa a chamada normal à trava. Em memória, as seis mutações suprimiram as respectivas recusas da guarda isolada. [RED](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_passivo_releitura.py:48) · `chamados/tests/test_e4c_f5_passivo_releitura.py:82` · `:136` · `:157` · `chamados/services/materializacao.py:2116`

### Comandos para a pista

No HEAD `6a5ab520c`, com apenas o arquivo do RED, o primeiro `provar "$red"` deve falhar na asserção de recusa do grupo. Na cópia com a cura, o mesmo comando deve passar e cada mutante deve falhar nessa asserção. Resultados com PostgreSQL [nao verificado]. `chamados/tests/test_e4c_f5_passivo_releitura.py:69` · `:103` · `config/settings/ci.py:6`

```bash
provar() {
    PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test \
        --settings=config.settings.ci --noinput "$@"
    rc=$?
    printf 'RC=%s\n' "$rc"
    return "$rc"
}

red=chamados.tests.test_e4c_f5_passivo_releitura
caso="$red.PassivoReleituraTest"

# Rodar no HEAD com apenas o RED e repetir com a cura aplicada. [nao verificado]
provar "$red"

CORTES_F2_MUTANTE=f5_passivo_sem_data_conferida_em provar "$caso.test_RED_conferencia_da_avulsa_recusa_o_grupo_sem_DATA_EM_DISPUTA"
CORTES_F2_MUTANTE=f5_passivo_sem_exportada provar "$caso.test_RED_exportacao_nova_recusa_o_grupo"
CORTES_F2_MUTANTE=f5_passivo_sem_dia provar "$caso.test_RED_data_evento_muda_o_dia_com_TS_e_competencia_iguais"
CORTES_F2_MUTANTE=f5_passivo_sem_competencia provar "$caso.test_RED_corte_da_empresa_muda_a_competencia_com_dia_e_TS_iguais"
CORTES_F2_MUTANTE=f5_passivo_sem_ts provar "$caso.test_RED_marco_fixo_muda_o_TS_com_dia_e_competencia_iguais"
CORTES_F2_MUTANTE=f5_passivo_sem_classe provar "$caso.test_RED_batida_fora_do_slot_muda_a_classe_com_TS_igual"

provar chamados.tests.test_e4c_f5_guarda_plantio_s144 \
    chamados.tests.test_e4c_f5_plantio_concorrente \
    chamados.tests.test_e4c_f5_voltaD_portas \
    chamados.tests.test_bug_par_projetado \
    chamados.tests.test_o306_vigia_carimbo \
    chamados.tests.test_o317_passivo_pela_porta \
    ponto.tests.test_cadastro_futuro_c5e \
    core.tests.test_medida_turno_do_par
```

**Incompleto.** Os commits separados de RED e cura ficaram pendentes pela proibição expressa de `git commit`. Antes da edição de produção, a tentativa de carregar o RED terminou com `ModuleNotFoundError: django.db`, RC 1, na importação. O RED de negócio no HEAD, o verde com a cura, os mutantes concorrentes, os vizinhos com banco e a suíte inteira seguem [nao verificado]. `chamados/tests/test_e4c_f5_passivo_releitura.py:7` · `config/settings/ci.py:6`

## 21/11 18:56 · sessao c9da406da684 · E4C-F5-CONSTROI · verificador · origem=Code

**Veredito da pré-verificação: bloqueia.** O diff implementa a fórmula do aval para as decisões que já existem quando a ressuscitação lê a pergunta sob a trava. A leitura pode, porém, apagar uma resposta gravada **depois** da ressuscitação. Dois testes existentes ainda exigem os carimbos que o `+1 µs` substituiu, e o I23 repete o predicado que identifica a ressuscitação. [app/chamados/models.py:1717](/home/ronald/codex_ws/f0be31c63502311204d23bf44ae0b4757cdf811b/app/chamados/models.py:1717) [app/chamados/models.py:4384](/home/ronald/codex_ws/f0be31c63502311204d23bf44ae0b4757cdf811b/app/chamados/models.py:4384) [app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:380](/home/ronald/codex_ws/f0be31c63502311204d23bf44ae0b4757cdf811b/app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:380) [app/chamados/management/commands/auditar_invariantes_chamados.py:1127](/home/ronald/codex_ws/f0be31c63502311204d23bf44ae0b4757cdf811b/app/chamados/management/commands/auditar_invariantes_chamados.py:1127)

Conferi os quatro blobs presentes contra os índices do diff e reconstruí em memória os quatro blobs anteriores. Também compilei em memória os três mutantes novos e avaliei a expressão do HLC com os horários das fixtures. Identidade do commit inteiro, execução da suíte, concorrência real e valores de produção ficam **[nao verificado]**. [nao verificado]

## 1. Cláusulas do item (1)

| Cláusula | Conferência |
|---|---|
| Ler o relógio uma vez sob a trava | **Sim.** A chamada de trava está fora do laço. `_agora_r = _tz_r.now()` roda uma vez no `atomic`, antes do laço de escrita. A leitura já existia nessa posição no delta-base. `app/chamados/models.py:1652–1657,1715–1722`. |
| Gravar `max(agora sob trava, último ato da mesma pergunta + 1 µs)` | **Sim.** O `max` usa as decisões de `_q` e o relógio único. Com histórico vazio, o conjunto do `max` contém apenas o relógio. O resultado vai a `registrar` na REABERTURA marcada. `app/chamados/models.py:1717–1736`. |
| Curar r1 e r3 pela vista sob a trava | **Sim para os atos anteriores visíveis nessa consulta.** O passivo é registrado antes do novo prefetch. O `+1 µs` coloca a REABERTURA depois de cada linha lida, inclusive a validação inserida entre lote e trava nos cenários de teste. `app/chamados/models.py:1675–1680,1721–1736`; `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:222–228,255–275`. |
| Sonda só como pré-filtro | **Sim.** `_agora_sonda` retém na simulação apenas decisões com `ato_em > _agora_sonda`. Saem da função três listas de PKs. O carimbo persistido vem de `_agora_r` e do prefetch de escrita. `app/chamados/models.py:1555–1567,1587–1602,1717–1736`. |
| Travas do aval 1445(1) | **Mesmo conjunto e ordem do delta-base**, conforme seção 3. O texto do aval 1445(1) está **[nao verificado]**. `app/chamados/models.py:1638–1678,2892–2921`. |
| r1, r3 e par verdes na cura e vermelhos em C e B | **Parcial.** A matriz completa está ausente. O r3 sem recuo e o caso do par de `+1,5 s` também satisfazem suas asserções em C. Dois controles de empate do par conservam os valores antigos e o HLC produz `+1 µs` a mais. A medida de B foi removida. `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:222–228,255–275`; `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:364–389`; `app/chamados/tests/test_e4c_f5_4c1_medida_b.py:39–50,72–92` na versão removida do anexo. |
| Mutante de relógio antes da trava e mutante sem `max` | **Presentes e discriminantes pela leitura.** M3 muda o instante de r3. M4 põe a REABERTURA antes da validação de r1. M5 também cobre a vista velha. `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:113–137,279–298`. |
| Aceitar carimbo adiantado e contar `> agora + 1 min`, esperado zero | **Presente para REABERTURAs da ressuscitação.** O I23 lê a marca, o tipo e o limite estrito. Há dupla implementação do predicado de pertencimento, detalhada na seção 6. `app/chamados/models.py:1729–1736,4282–4295`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1122–1129,1862–1866`. |

## 2. Quem escreve e quem lê

| Valor | Escrita e leitura |
|---|---|
| **(i) `_agora_r`** | Variável local atribuída por `ressuscitar_perguntas` sob a trava. O consumidor é o `max`. É o relógio da borda de escrita e fica separado do carimbo persistido. `app/chamados/models.py:1657,1715–1729`. |
| **(ii) `ato_em` das decisões para o `max`** | `DecisaoPergunta.registrar` persiste o valor recebido dos chamadores sem recalculá-lo. Fornecem relógio cru a resposta humana, a validação/devolução, a reabertura/recusa comum e a retratação. Respostas automáticas ou pela trilha, silêncio, absorção e passivo também podem fornecer instante recebido ou carimbo existente. O `max` lê todas as decisões persistidas da pergunta, inclusive REABERTURAs anteriores e o passivo recém-promovido. `app/chamados/models.py:1679–1680,1721–1729,2127–2129,2227,2259–2261,4321–4339,4445–4455`; `app/chamados/services/disputa_emissao.py:1499–1501,1535–1537,1648–1668,1689–1737`; `app/chamados/services/validacao.py:168–175,287–296,365–374`; `app/chamados/services/materializacao.py:1087`; `app/ponto/services/veredito_celula.py:195–196`. |
| **(iii) `ato_em` da REABERTURA compensadora** | `ressuscitar_perguntas` calcula e fornece o carimbo e a marca. `registrar` insere. A leitura vigente, a face SQL da validação, o histórico do modal e o I23 o consomem com semânticas distintas, detalhadas abaixo. `app/chamados/models.py:1729–1736,3608–3629,3863–3879,4378–4396,4445–4455`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1127–1129`. |
| **(iv) `PerguntaDisputa.validada_em`, coluna** | `_carimbar_validacao` a preenche e grava a VALIDACAO com o mesmo instante. `carimbar(resposta='validar')` também a preenche pela materialização. Zeram a coluna a ressuscitação, a resposta humana, a reabertura/recusa comum e as devoluções em `validacao.py`. A simulação da prévia zera só a cópia em memória. `app/chamados/models.py:1587–1590,1738–1739,2676,3384–3391,3548–3556`; `app/chamados/services/materializacao.py:1073,2015–2016`; `app/chamados/services/disputa_emissao.py:1493–1514,1532,1630,1683`; `app/chamados/services/validacao.py:168–175,287–296,365–374`. |
| **(iv) `validada_em` derivada** | `leitura` dobra as linhas e o passivo. VALIDACAO atribui o instante respectivo. REABERTURA, RECUSA, DEVOLUCAO e a resposta humana que desfaz validação limpam o resultado. `composta` propaga o valor. A ressuscitação usa o valor na foto do ato. Leem-no também estado, C6, validação/recusa, materialização, histórico e vigia. `app/chamados/models.py:1723–1734,3276–3285,4342–4362,4378–4416`; `app/chamados/juizes.py:1383–1411`; `app/chamados/services/validacao.py:178–187,275–284,656–657,698–704,915–923`; `app/chamados/services/materializacao.py:201–203,2161`; `app/chamados/services/historico.py:96–108`; `app/chamados/services/vigia_ato_x_coluna.py:8–23`. |
| **(v) Prévias** | `_universo_da_ressuscitacao` monta `ids`, `marco_aceso` e `c6_morta`. Os dois métodos públicos devolvem partes dessa computação, sem persistência. A porta chama o mesmo produtor diretamente. O emissor usa `ids` na carga para avaliar a reabertura do credor. O cartório, ao receber zero da porta, recomputa as listas de puladas para contadores e mensagens. `app/chamados/models.py:1520–1532,1556,1583–1602,1638–1646`; `app/chamados/services/disputa_emissao.py:1851–1861,1889–1911`; `app/ponto/services/cartorio.py:1721–1741`. |

**Semântica por leitor.** A configuração da cópia tem `USE_TZ=True` e zona padrão `America/Sao_Paulo`. Os recortes abaixo comparam instantes. O modal aplica o filtro de apresentação `date:"d/m H:i"`. A zona efetiva de um request em produção está **[nao verificado]**. `app/config/settings/base.py:92–95`; `app/templates/chamados/partials/modal_fio.html:204–212`.

- **`leitura` e face SQL.** `(ato_em, pk)` ordena os atos reais. No empate, o passivo sintético fica depois dos reais daquele instante. `leitura` aplica efeitos sem horizonte de relógio. A face SQL usa `>` ou empate com `pk` maior para limpar uma linha real e `>` estrito para limpar o passivo. Ambas aplicam a REABERTURA adiantada assim que ela está visível. `app/chamados/models.py:3608–3629,4365–4396`.
- **C6 e prévias.** A sonda descarta `<= _agora_sonda` e preserva `>` para o C6 simulado. O emissor e o cartório recebem listas ou contagens. O C6 com `agora` corta os valores derivados `> agora` **depois** da dobra e, por isso, o corte não restaura uma validação que uma REABERTURA futura já limpou na dobra. `app/chamados/models.py:1587–1602`; `app/chamados/services/disputa_emissao.py:1857–1861`; `app/ponto/services/cartorio.py:1721–1741`; `app/chamados/juizes.py:1390–1411`.
- **Desarquivar.** A segunda chamada da porta vem depois do registro do toque para a competência trancada. A view ignora o retorno da ressuscitação e segue para a nota. Ela não interpreta `ato_em` nem consome as listas. `app/chamados/views_arquivar.py:63–89`.
- **Modal.** `respostas_anteriores` percorre as decisões na ordem do modelo, põe REABERTURA/RECUSA no histórico e devolve as mais recentes primeiro. O template mostra o carimbo como hora da ação, com precisão de minuto e sem corte pelo presente. Um carimbo lógico adiantado pode aparecer como hora futura. `app/chamados/models.py:3863–3879,4269–4270`; `app/templates/chamados/partials/modal_fio.html:204–214`.
- **Validação e leitores da coluna.** As guardas de validar e recusar usam a composta sem horizonte. A foto da devolução prefere a coluna e recorre à composta quando a coluna está vazia. A coluna também serve ao I18, ao cache do painel, à procedência da batida e ao placar de validações, que usa limites inferiores inclusivos e meia-noite em hora local. `app/chamados/services/validacao.py:178–187,269–284,698–704,915–923`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1081–1086`; `app/chamados/services/painel.py:62–68`; `app/ponto/services/procedencia_batida.py:69–75`; `app/chamados/services/verbos.py:247–271`.
- **Tarefa do dia.** Exclui a REABERTURA marcada pela autoridade `es_da_ressuscitacao` **antes** de cortar pelo horizonte. Para os atos restantes, `ato_em <= agora` é inclusivo. Passivo e projeção temporal seguem a mesma inclusão. Assim, a tarefa nunca usa a REABERTURA marcada como a última decisão, mesmo depois que o relógio alcança seu carimbo. `app/chamados/services/tarefa_do_dia.py:152–174,387–434,533–547`.

## 3. Momento da leitura, travas e elegibilidade

1. **Antes da chamada de `trava_na_ordem`**, a porta avalia `_universo_da_ressuscitacao`. O gerador entra no queryset, que avalia o lote e o prefetch antes da primeira leitura de `_agora_sonda`. A porta obtém os IDs e as disputas do pré-filtro. `app/chamados/models.py:1549–1567,1638–1657`; `app/chamados/juizes.py:1174–1195`.
2. **Depois da chamada de trava**, a porta relê materialização e vias fora do fato, fixa os IDs graváveis, monta outro queryset e registra o passivo. Lê **(i)** depois disso. Lê **(ii)** ao avaliar o novo prefetch e percorrer `_q.decisoes.all()`. A vista da sonda e seu cache filtrado ficam nas instâncias do pré-filtro. O caminho de escrita não reutiliza essas instâncias. Isolamento efetivo do banco e visibilidade de commits em transações reais ficam **[nao verificado]**. `app/chamados/models.py:1597–1602,1675–1680,1715–1729,4374–4378`.
3. **Conjunto e ordem preservados no delta.** Trava-se o colaborador, as disputas obtidas dos IDs prévios, o chamado e as perguntas desses IDs, por grupo em PK crescente. O subconjunto relido manda na escrita. A guarda do lote ainda recebe `alvo_lido`. Os filtros e a sonda das prévias permanecem os mesmos, inclusive o corte estrito. O novo carimbo pode virar insumo de uma prévia futura porque a sonda compara os `ato_em` persistidos. `app/chamados/models.py:1549–1602,1645–1678,1738–1739,2892–2921,3542–3556`.

## 4. Ato posterior que fica antes na leitura

**Há uma sequência concreta, mesmo com a trava correta.** Use o r1 como partida. A validação tem `T + 500 ms`, o relógio volta a `T` e a ressuscitação grava `T + 500 ms + 1 µs`. Depois de a ressuscitação terminar, uma resposta humana consegue a trava e lê, por exemplo, `T + 100 ms`. Ela grava esse mesmo instante na coluna e na linha RESPOSTA. Seu PK é posterior, mas a leitura ordena primeiro por `ato_em`. A REABERTURA é aplicada depois da RESPOSTA e limpa os valores derivados. A coluna conserva texto e instante. O avanço posterior do relógio deixa a ordem gravada intacta. `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:222–228`; `app/chamados/services/disputa_emissao.py:1478–1502,1532–1537`; `app/chamados/models.py:1729–1736,4347–4348,4378–4396`.

O **vigia de texto** conta essa divergência. O **vigia ato × coluna** conta somente presença de validação, portanto o exemplo só de resposta não entra em A/B. Se uma VALIDACAO de relógio cru for gravada com carimbo abaixo da REABERTURA e sua coluna permanecer preenchida, o par é **B `(False, True)`**. O I23 conta apenas o adiantamento superior a um minuto. No exemplo de `500 ms + 1 µs`, ele fica em zero apesar da divergência. `app/chamados/services/vigia_texto_resposta.py:17–25`; `app/chamados/services/vigia_ato_x_coluna.py:14–23`; `app/chamados/management/commands/vigiar_fio_morto.py:194–198,231–234`; `app/chamados/services/validacao.py:168–175`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1127–1129`.

## 5. Testes, mutantes e relógio solto [nao verificado]

| Caso | O que a asserção discrimina |
|---|---|
| **r1** | A cura exige `T + 500 ms + 1 µs`. C sem `max` grava `T` e falha em `_coerente`. B grava `T + 500 ms` e falha na nova igualdade exata, embora o empate por PK permita a coerência antiga. `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:205–228`; `app/chamados/tests/test_e4c_f5_4c1_medida_b.py:45–50` na versão removida. |
| **r3 sem recuo / com recuo** | Sem recuo, cura e C gravam `T + 1 s`. B usa `T`. Com recuo, cura exige `T + 500 ms + 1 µs` e C/B usam `T`. O segundo caso distingue a vista fresca da antiga. `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:255–275`; `app/chamados/tests/test_e4c_f5_4c1_medida_b.py:72–82` na versão removida. |
| **Par de `+1,5 s` e controles** | O caso de `+1,5 s` é compatível com cura e C. A medida removida de B cobrava `+0,5 s`. Os outros dois testes exigem `T + 500 ms` e `T`; a expressão da cura devolve `T + 500 ms + 1 µs` e `T + 1 µs`. A igualdade de `_alvo` reprova esses resultados. `app/chamados/models.py:1729`; `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:364–389`; `app/chamados/tests/test_e4c_f5_4c1_medida_b.py:83–92` na versão removida. |
| **M1–M5** | M1/M2 exigem `n=0` contra `n=1` de r2. M3 exige o mutante em `T + 500 ms + 1 µs` e reprova a expectativa de `T + 1 s`. M4/M5 exigem uma REABERTURA real em `T` antes de cobrar a falha de `_coerente`. Nesses dois, a primeira falha esperada é a ordem em `:211`. O teste do mutante não chega a cobrar separadamente a divergência em `:212`. As trocas AST ocorrem fora do `assertRaises`, com verificação de sítio único. `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:34–50,89–137,205–215,234–252,279–298`. |
| **I23 e selo C12** | O caso positivo do I23 exige uma linha do auditor e total `1`. O negativo confirma que existe uma REABERTURA dentro da janela e cobra `0`. Falta caso de igualdade exata a um minuto e caso de REABERTURA comum adiantada. C12 atualiza dois endereços que o censo usa como parte da identidade. `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:301–319`; `app/chamados/tests/e4b_c12_ast.py:17–29,221–224`; `app/chamados/models.py:1792,1812`. |

Os testes novos exercitam a porta com injeção síncrona por mock. A validação da fixture é `UPDATE` + `registrar` na mesma conexão, antes de chamar a trava real. Ela discrimina a ordem das leituras no código. Commit concorrente e espera de lock entre duas conexões ficam **[nao verificado]**. `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:152–195`.

**Há relógio solto indireto.** Nas duas `_caso`, o chamado e a pergunta são criados antes do `freeze_time` local. A criação tem carimbos `auto_now_add` e o sinal do chamado pode chamar o cartório, que lê `localdate()` e, chegando ao lote sem `agora`, `now()`. A injeção, as chamadas da porta e a chamada do auditor estão sob seus respectivos freezes. O efeito do relógio solto no resultado da suíte fica **[nao verificado]**. `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:143–152,192–195,301–305`; `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:312–323`; `app/chamados/models.py:79,1929`; `app/chamados/signals.py:189–218`; `app/ponto/services/cartorio.py:2059–2088`.

## 6. I23

- **Predicado e universo.** Conta linhas de decisão marcadas como REABERTURA da ressuscitação, em todo o queryset, com `ato_em > timezone.now() + 1 minuto`. Igualdade fica fora. O universo corresponde ao contexto do aval sobre a REABERTURA compensadora. Uma leitura literal da frase isolada que incluísse qualquer REABERTURA seria mais ampla. O resultado vai à saída comum do auditor, com esperado zero. `app/chamados/management/commands/auditar_invariantes_chamados.py:1118–1129,1862–1866`.
- **Duplicação.** `ato=REABERTURA` mais `da_ressuscitacao=True` repete em SQL a parte de identidade de `es_da_ressuscitacao`, que se declara predicado único e é usado pela tarefa. O limite de um minuto é a condição nova de monitoramento. A constraint existente também prende a marca ao tipo. O diff não acrescenta entrada ao registro vivo. A duplicação é da autoridade efetiva do modelo, ainda que uma inscrição nominal desse método em `core/juizes.py` esteja **[nao verificado]**. `app/chamados/models.py:4250–4257,4282–4295`; `app/chamados/services/tarefa_do_dia.py:393–403`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1127–1129`; `app/docs/LEIS.md:60`.
- **Contrato de saída.** O teste novo exige exatamente uma linha I23 e verifica seus totais. O emissor do auditor percorre `res`, sem lista fixa para imprimir as linhas. O contrato F9 enumera usos específicos de colunas no auditor. Uma enumeração fechada de todos os IDs que precise receber I23 está **[nao verificado]**. `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:301–319`; `app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:272–284`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1862–1884`.

## 7. Checklist

**137 linhas `+` de conteúdo** no anexo. São 8 no auditor, 12 no modelo, 2 no selo C12 e 115 no teste de instante. **0 `except` novo, 0 fallback que esconda erro e 0 ramo novo que silencie erro em produção.** O relógio como único candidato para histórico vazio é parte da fórmula. O `or ''` da foto é contexto preexistente. Os três `assertRaises(AssertionError)` novos são verificações de teste. `app/chamados/management/commands/auditar_invariantes_chamados.py:1122–1129`; `app/chamados/models.py:1707–1736`; `app/chamados/tests/e4b_c12_ast.py:26–27`; `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:31,89–139,227–228,255–319`.

**Efeito do tratamento preexistente no sítio novo.** Falha na avaliação da consulta I23 dentro de `add` vira linha `ERR` pelo `except` existente. O erro fica visível e não sobe por esse caminho. I23 sozinho não muda o código de saída. O teste descarta `_rc`, mas `ERR` no próprio I23 fornece `-` e faz falhar a conversão para `int`. `app/chamados/management/commands/auditar_invariantes_chamados.py:564–573,1127–1129,1862–1884`; `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:301–308`.

| Classe | Resultado |
|---|---|
| Segundo juiz | **Reincide no predicado de pertencimento do I23.** `app/chamados/models.py:4290–4295`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1127–1129`. |
| Segundo escritor fora da porta | **0 novos.** A REABERTURA continua por `registrar` e a limpeza continua por `descarimbar_em_lote`. `app/chamados/models.py:1730–1739,4445–4455`. |
| Relógio fora da borda | **0 novas leituras de relógio de escrita fora da trava.** A sonda conserva a bord

[truncado em 20000 de 24188 caracteres]

## 21/11 18:56 · sessao ffa419294217 · E4C-F5-CONSTROI · verificador · origem=Code

BLOQUEIA

**NOVO contra a base descrita no anexo.** Uma pergunta `hora` com resposta desfeita pode passar a ser considerada materializada só porque tem uma batida vinculada, mesmo quando a batida está fora do slot. Reproduzi em memória com o juiz e o derivador da cópia. A pergunta tinha `06:30` na coluna, instante físico presente, resposta ausente na composta, carimbo de materialização sem via e uma batida viva vinculada às `05:24`. O hash final devolveu `PLANTIO_SEM_TS` e julgou `True`. Mantendo o mesmo caso e repondo apenas a alimentação pelo texto cru descrita para a base avulsa, obtive `SLOT(06:30, S)` e `False`. O retorno por vínculo já existia. **A regressão é encaminhar uma resposta desfeita para esse retorno.** Um destino encerrado também pode passar a valer pelo mesmo caminho, quando a lei cala ou fica fora da janela. Execução da base e incidência em produção [nao verificado]. `app/chamados/juizes.py:374–382,408–420,798–845,860–863` · `app/chamados/services/materializacao.py:691–710` · `app/chamados/tests/e3_c8b_apoio.py:116–124,143–147`

### 1. Achados da volta 1

| Achado | Situação no hash final |
|---|---|
| **S1 — erro de banco virava classe B** | **Fechado no código.** A consulta de conflito sobe com a própria exceção. O teste injeta `DatabaseError`, exige a chamada de `.exists()` e a identidade do erro propagado. Desapareceu o ramo novo que retornava `CONSULTA_CONFLITO_INDISPONIVEL`. Execução ORM e efeito em transação real [nao verificado]. `app/chamados/services/validacao.py:1224–1232` · `app/chamados/tests/test_fila_validar_lote.py:110–123` |
| **S2 — explicador, vínculo, destino e retratação** | **Os quatro casos pedidos estão no código.** O explicador exige `validada_sem_gravar`, “sem resposta vigente” e ausência do texto residual. Vínculo e destino têm controles sob `SEM_RESPOSTA_VIGENTE`. A retratação exige carimbo mais trilha, com leitura avulsa e pela carga. Os três controles de materialização usam `data_hora`. O teste de `hora` verifica só o slot e deixa passar a regressão acima. Execução ORM dos REDs [nao verificado]. `app/chamados/tests/test_explicador_disputa.py:80–101` · `app/chamados/tests/test_marco_por_motivo.py:167–173,231–282` · `app/chamados/juizes.py:798–845` |
| **S2b — fixture do HX** | **Fechado no código.** O teste exige `PLANTIO_SEM_TS` antes dos controles de lei acusando, lei calada e dia fora da janela. Execução ORM [nao verificado]. `app/chamados/tests/test_hx_fallback_sem_lei.py:61–86` |
| **S3 — foto velha no passivo** | **Fechado só para o cache de decisões.** O vigia expurga `decisoes` antes do `yield`. O RED de devolução entre classificação e trava exige que o ato novo seja visto. O passivo ainda conserva a seleção e outras colunas pré-trava, detalhadas abaixo. Execução ORM do RED [nao verificado]. `app/chamados/services/vigia_carimbo.py:103–123` · `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:95–129,146–156` · `app/chamados/services/passivo_validado.py:330–356,377–384,532–545` |
| **S3 — PRONTO** | **Aberto como comprovação.** O anexo informa sonda A→B com soma **1**, abaixo de **50**. Publicação no RELATO, registro dos dois SHAs, censo de divergência de horizonte em produção anterior à cura, integração com a volta 3, medição ORM do vigia, suíte inteira verde e opus final [nao verificado]. Rodei **65 testes sem banco**, todos verdes, entre C8b, C8c e as duas catracas. `triagem_v1.txt:5` · `CLAUDE.md:35–38,372–381` · `app/chamados/tests/test_e3_c8b_sem_banco.py:210–546` · `app/chamados/tests/test_e3_c8c_sem_banco.py:69–132` · `app/chamados/tests/test_e4c_f5_selo_chamada.py:188–299` · `app/chamados/tests/test_e3_catraca_respondedores.py:482–879` |

Os dois descartes da triagem seguem **PREEXISTENTES e de fila** pelo escopo nominal informado. O parser de `data_hora` continua em dois corpos. A API ainda pega os detalhes da classificação anterior e substitui só `classe`. O efeito novo na API é que os motivos de exclusão recém-produzidos pelo lote continuam sem chegar ao payload. A autorização literal completa do aval [nao verificado]. `triagem_v1.txt:6` · `app/chamados/juizes.py:389–402` · `app/chamados/services/materializacao.py:1624–1637` · `app/chamados/services/validacao.py:1224–1232` · `app/api/views_mensageria.py:542–575`

### 2. `carimbo_legado` e significado dos campos

**Entre `aeb326255` e `f3d23021f`, nenhum dos casos muda o veredito.** A única mudança de valor da guarda é de verdadeira para falsa com texto vazio após `strip()`, `respondida_em` físico ausente, ambas as validações ausentes, via vazia e materialização visível. O derivador recebe vazio ou brancos antes e `''` agora. Ambos deixam o marco fixo disponível e ambos falham como hora livre. Conferi **4.608 combinações** em memória com o derivador real, dependências simuladas e a guarda anterior reconstruída pela troca informada. `app/chamados/juizes.py:368–420,1433–1446,1576–1591` · `app/chamados/models.py:4382–4390` · `app/chamados/services/materializacao.py:549–563,641–663,691–710`

| Estado na entrada da guarda | `aeb326255` → `f3d23021f` |
|---|---|
| Texto vazio ou só brancos, demais condições satisfeitas | Guarda `True → False`. Mesmo slot ou mesma ausência de slot. `app/chamados/juizes.py:411–420,1576–1591` · `app/chamados/services/materializacao.py:691–710` |
| Texto não vazio, instante físico ausente, as duas validações ausentes, via vazia e materialização visível | Guarda `True → True`. O mesmo texto chega ao derivador. Com instante vigente na composta, C21 já permite esse texto. `app/chamados/juizes.py:374–382,411–420,1594–1603` |
| Instante físico presente, validação presente em qualquer uma das duas fontes, via preenchida ou materialização ausente ou posterior ao horizonte | Guarda `False → False`. Uma resposta futura na composta é vetada antes da guarda. `app/chamados/juizes.py:371–387,411–420,1433–1446` |
| `data_hora`, outro tipo ou resultado já encontrado no cache | A guarda fica fora do caminho. As saídas anteriores continuam decidindo. `app/chamados/juizes.py:374–402` |

`texto_sem_carimbo` conserva o significado de **meio-par ilegal** da L-130. A porta o usa para recusar a gravação. A guarda passa a usar o mesmo predicado como condição para aproveitar o texto ilegal na busca de lastro. C21 continua decidindo a vigência e a porta de plantio continua exigindo C21. **Esse caminho alternativo é NOVO contra a base**, embora recupere uma parte da antiga leitura de texto cru. O lote para em C21 antes do slot. O universo do vigia exige validação física preenchida e, por isso, também exclui o ramo legado. `app/docs/LEIS.md:151` · `app/chamados/juizes.py:379–382,408–420,1587–1603` · `app/chamados/services/fio.py:1704–1722` · `app/chamados/services/materializacao.py:1332–1341` · `app/chamados/services/validacao.py:1224–1228` · `app/chamados/services/vigia_carimbo.py:97–104`

**ESMERIL-SEMÂNTICA.** A composta usa instantes do ato e texto da coluna. `resposta_display` usa o texto vigente da sequência. A porta de limbo lê texto sem instante para reparar o par pela hora da nota. `carimbar` pode completar os instantes físicos quando há texto. Esses usos são **PREEXISTENTES**. A guarda **NOVA** compara diretamente validação física e do ato, em outro sítio além de `par_ato_coluna`, e usa a validação composta **anterior ao corte temporal**. Assim, uma validação só no ato e posterior a `agora` pode desabilitar o legado numa consulta histórica, mesmo quando a projeção já a retirou. Isso vale nos dois hashes da pergunta. Ocorrência no acervo [nao verificado]. `app/chamados/models.py:3361–3374,3865–3869,4382–4399` · `app/chamados/services/disputa_emissao.py:1719–1737` · `app/chamados/juizes.py:372,411–412,1433–1446`

### 3. Lista do C8b e mordida da catraca

**O nome acrescentado à lista do apoio não amplia a tolerância da catraca.** A lista carrega os corpos reais por AST no namespace dos testes, usada por C8b, C8c e pós-suíte. A catraca varre fontes de produção, exclui diretórios de teste e usa outra lista, `FORA_PELA_PERGUNTA`, com contagem exata por sítio. `app/chamados/tests/e3_c8b_apoio.py:116–124` · `app/chamados/tests/e4_apoio.py:8–16` · `app/chamados/tests/test_e3_c8b_sem_banco.py:15,29–30` · `app/chamados/tests/test_e3_c8c_sem_banco.py:17–24,69–78,124–132` · `app/chamados/tests/test_e3_pos_suite_sem_banco.py:16–17,103–120` · `app/chamados/tests/test_e3_pendentes_app.py:96–115` · `app/chamados/tests/test_e3_catraca_respondedores.py:66–86,437–457,544–557`

**A mudança de produção altera o que o selo enxerga.** Repor `p.respondida_em is None` na guarda em memória dá duas leituras `DECLARADO` em `_slot_esperado`, onde se espera uma. Com a chamada a `texto_sem_carimbo`, a leitura do meio-par aparece no predicado como `PAR` e a contagem volta a um. O selo morde a segunda leitura direta. Retirar `or carimbo_legado` da foto deixou verdes os **38 casos** rodados de C8b, C8c e pós-suíte selecionado. A fixture noturna usa par legal. Há um teste ORM de fechamento com meio-par legado, cuja mordida isolada [nao verificado]. `app/chamados/juizes.py:374,411–415,1587–1591` · `app/chamados/tests/test_e3_catraca_respondedores.py:116–155,393–395,544–557` · `app/chamados/tests/test_e3_c8b_sem_banco.py:404–433` · `app/chamados/tests/test_fechar_disputa_materializada.py:38–68`

### 4. Escritores e leitores

| Campo, valor ou função | Quem escreve e quem lê |
|---|---|
| **`resposta_colab`, `respondida_em`, `validada_em`** | A resposta humana grava o par e zera a validação antiga. A automática grava o par e preserva validação. A cura pela trilha completa o instante. Reabertura e recusa registram o ato e limpam pela porta em lote. Validação e devolução escrevem o carimbo e o ato correspondentes. `carimbar` também pode preencher instantes físicos. Leem a composta C6, a property `respondida`, o estado, a retenção, a validação e a guarda de anuência. Leem as colunas a integridade do `save`, o lote para escape e delta, o vigia para universo e a API para a resposta e a idade. `app/chamados/services/disputa_emissao.py:1491–1517,1630–1635,1664–1684,1689–1737` · `app/chamados/services/validacao.py:161–175,270–296,357–374,698–704,1200–1228` · `app/chamados/models.py:2002–2026,3256–3268,3361–3374,3744–3764,3838–3844` · `app/chamados/juizes.py:1449–1471` · `app/chamados/services/materializacao.py:1332–1341` · `app/chamados/services/vigia_carimbo.py:97–104` · `app/api/views_mensageria.py:569–575` |
| **`decisoes`, `materializada_em`, `via_resolucao`** | `DecisaoPergunta.registrar` grava a trilha e invalida o cache e o memo na instância recebida. `leitura` combina sequência com projeção sem linha. `composta` entrega instantes lidos e os campos físicos de texto, carimbo e via. `_escrever_carimbo` atribui materialização, via e autor para as portas do modelo. Julgamento de fato, C6, explicador e passivo leem esses dados. O vigia escreve e remove só o prefetch de `decisoes` da instância entregue. `app/chamados/models.py:3324–3328,3363–3374,3416–3424,4291–4323,4348–4399,4428–4450` · `app/chamados/juizes.py:369–375,411–415,723–739,1449–1465` · `app/chamados/services/explicador.py:98–125` · `app/chamados/services/passivo_validado.py:532–540` · `app/chamados/services/vigia_carimbo.py:103–123` |
| **`tipo_resposta`, `motivo`, `data_evento`, destino, contexto, grade e batidas** | Emissão e nascimento fornecem identidade e contexto. As portas do fio gravam o dia e o destino. O slot usa esses insumos e o derivador de `hora` lê também marco, grade e vínculo. `registrar_batida` grava a batida, `anotar` grava a origem no reuso/plantio e a porta de retratação grava a anulação. Juiz, vigia, carga e consulta de conflito leem as batidas em seus respectivos recortes. `app/chamados/services/disputa_emissao.py:291–296,448–461,559–579` · `app/chamados/services/fio.py:843–855,975–981` · `app/chamados/juizes.py:240–284,314–336,368–420` · `app/chamados/services/materializacao.py:556–592,624–645,691–710` · `app/ponto/registro_batida.py:118–180,255–297` · `app/ponto/turnos.py:2872–2897` |
| **`agora`, `slots[pk]`, testemunhas e `retratar_pergunta_obsoleta`** | A tarefa fornece o horizonte e carrega a sequência. A carga copia `agora`, escreve resultado ou exceção por PK e traz batidas por origem ou janela. `_slot_esperado` lê o mapa após as guardas. O juiz lê plantadas, janela e trilha da carga, ou consulta as fontes no avulso. Os testes escrevem a chave literal de retratação. Escritor dessa chave literal em produção [nao verificado]. `app/chamados/services/tarefa_do_dia.py:520–539,630–637` · `app/chamados/services/carga_pergunta_viva.py:88–95,110–145,186–195` · `app/chamados/juizes.py:287–298,362–387,798–845,860–863,1530–1543` · `app/chamados/tests/test_marco_por_motivo.py:260–269` · `app/chamados/tests/test_juiz_le_retratacao.py:49–61` |
| **`motivo`, `ts`, `tipo` do slot e booleano de lastro** | `_slot_esperado` produz as seis saídas. Juiz de fato, carga, lote, explicador e vigia consomem o resultado. O booleano alimenta C6, lavra, backfill, auditoria, raio-X, emissão e materialização. O vigia produz a tripla consumida pelo censo, passivo e vigia do fio. O explicador traduz o motivo para o item que chega ao fio. `app/chamados/juizes.py:346–420,798–863,1530–1543` · `app/chamados/services/carga_pergunta_viva.py:117–128` · `app/chamados/services/validacao.py:1224–1232` · `app/chamados/services/explicador.py:48–95,138–150,173–185` · `app/chamados/services/vigia_carimbo.py:94–155` · `app/chamados/veredito_lavrado.py:263` · `app/chamados/management/commands/backfill_veredito_lavrado.py:105` · `app/chamados/management/commands/auditar_invariantes_chamados.py:510,542,1166,1182` · `app/chamados/services/raiox.py:194` · `app/chamados/services/disputa_emissao.py:2052` · `app/chamados/services/materializacao.py:1971,2216` · `app/chamados/services/passivo_validado.py:330–356` · `app/chamados/services/vigia_fio.py:415–418` · `app/chamados/services/fio.py:340–351` |
| **Classe, `detalhe`, `porque`, `conflito`, `refs_lote`** | `delta_da_resposta` produz a comparação com o marco. A tolerância vem da folha e a janela de conflito de `CONFLITO_LOTE_MIN`. `classe_do_lote` produz classe e detalhe. A fila guarda ambos e seleciona refs A. Proposta e prévia exibem detalhes da A e agregam as exclusões. Executor e API consomem só a classe do lote. O teste de `refs_lote` ainda usa um caso fora do envelope como exclusão, sem exercitar uma nova exclusão por falta de slot. `app/chamados/juizes.py:1980–2028` · `app/chamados/services/validacao.py:1082–1096,1147–1154,1196–1232,1273–1289` · `app/chamados/views_cobrar.py:289–300,345–370` · `app/api/views_mensageria.py:542–575` · `app/chamados/tests/test_fila_validar_lote.py:79–108,125–130` |

### 5. Checklist de pouso [nao verificado]

| Classe | NOVO × PREEXISTENTE |
|---|---|
| Segundo juiz / segundo escritor | **Nenhum novo identificado nos sítios conferidos.** Slot e lote chamam C21 registrado. A guarda de legado acrescenta uma decisão de elegibilidade para usar texto. Foto, resultado, cache e tripla são de leitura. A escrita do lote continua na porta de validação. `app/core/juizes.py:305` · `app/chamados/juizes.py:379–420` · `app/chamados/services/carga_pergunta_viva.py:115–145` · `app/chamados/services/validacao.py:1094–1096,1224–1232` · `app/chamados/services/acoes_disputa.py:13–22` |
| Relógio / derivação paralela | O slot recebe o horizonte da tarefa. `date.today()` na janela viva e o parser paralelo de `data_hora` são **PREEXISTENTES**. O caminho **NOVO** que transforma `hora` desfeita em `PLANTIO_SEM_TS` pode alcançar a janela do destino encerrado. Em `hora`, o cálculo do instante continua no derivador compartilhado. `app/chamados/services/tarefa_do_dia.py:630–637` · `app/chamados/juizes.py:362–420,630–639,813–845` · `app/chamados/services/materializacao.py:691–710,1624–1637` |
| Ordem de trava / leitura antes da trava | Ordem global **PREEXISTENTE**, Colaborador → Disputa → Chamado → Pergunta nos conjuntos solicitados. O lote já decide classe e conflito antes da trava e a porta não os repete sob ela. No passivo, classe, dia, grupo e reversão continuam pré-trava. O `pop` **NOVO** remove a sequência velha, mas sob trava só são relidos materialização, via e autor. Uma resposta concorrente pode deixar a composta com instante novo e texto velho, que o plantio lê. A comparação booleana de presença de validação não cobre troca de texto com ambas as validações presentes. Ocorrência em produção [nao verificado]. `app/chamados/models.py:2838–2848,3384–3393,4382–4399` · `app/chamados/services/validacao.py:689–704,743–760,1074–1096,1224–1232` · `app/chamados/services/vigia_carimbo.py:121–123` · `app/chamados/services/passivo_validado.py:330–356,372–384,532–545` · `app/chamados/services/materializacao.py:1332–1341,1389–1395,2085–2103` |
| Selo / dois significados | **NOVO** uso do meio-par ilegal como pista de lastro, com diferença demonstrada contra o controle avulso e sem mordida no recorte que removeu o ramo. O teste de `hora` fixa o motivo do slot e deixa o resultado do juiz sem asserção. `detalhe['porque']` mistura rótulos antigos com novos códigos de exclusão. A mistura de indisponibilidade técnica com `LOTE_B` foi retirada. `app/chamados/juizes.py:411–420,798–845` · `app/chamados/tests/test_marco_por_motivo.py:271–282` · `app/chamados/services/validacao.py:1203–1232` |
| Pouso antes do pré-requisito | Pouso efetivo e cumprimento da ordem suite inteira → opus → pouso [nao verificado]. `CLAUDE.md:325–329,372–381` | [nao verificado]

**Autoconferência BRIEF-SEM-EXCEPT.** No `delta.patch` fornecido, **0 cláusulas `except` adicionadas em produção e 0 em teste**. Foram retiradas uma captura de produção que convertia a falha em B e uma de teste que exigia esse comportamento. A cópia coincide com os oito pós-estados do anexo pelos prefixos de blob. `git diff 5cc451ac3 f3d23021f -- | grep -n except` falhou ao acessar a base. **Total exato de `except` adicionados em `base..hash`, produção e teste [nao verificado].** Quanto a fallback semântico, há **1 novo identificado**, `carimbo_legado`, que permite alimentar o derivador com o texto do meio-par quando C21 não o habilita. O retorno por mero vínculo ou destino é preexistente e passou a ser alcançável no contraexemplo acima. `frase_brief.txt:1–2` · `CLAUDE.md:17–19,344–347` · `app/chamados/services/validacao.py:1224–1232` · `app/chamados/tests/test_fila_validar_lote.py:110–123` · `app/chamados/juizes.py:409–420,798–845`

## 21/11 18:54 · sessao 74048e093798 · E4C-F5-CONSTROI · verificador · origem=Code

BLOQUEIA

**A 3.3 ainda usa estado de antes da trava para decidir o carimbo.** A pergunta, a célula e os atos são relidos, mas a chamada nova ao C20 reaproveita o mesmo colaborador e o memo da pré-seleção. Um `None` cacheado para a competência continua valendo depois da trava, mesmo se a competência tiver sido trancada no intervalo. O selo também sobe com custo por candidata e deixa o commit de atribuição em aberto. `app/chamados/reconciliador.py:644–656,712–734` · `app/chamados/juizes.py:1067–1075,1101–1117` · `app/ponto/services/fechamento.py:1400–1429` · `app/ponto/tests/test_selo_chokepoint_escrita.py:206–209,233–236`

Usei a cópia e o diff fornecido para classificar **NOVO** e **PREEXISTENTE**. A cópia está em formato de arquivo exportado, sem metadados Git. A identidade dos commits e a cadeia base → RED → Y ficam **[nao verificado]**. `CLAUDE.md:344–348` [nao verificado]

## 1. Escopo do aval e complementos

| Item | Conferência |
|---|---|
| Reaplicar o universo depois da trava, pelo PK e pela disputa original | **Entregue, NOVO.** Reusa `qs_perguntas_absolviveis` com o mesmo filtro físico de validação. Se a candidata sair do universo, segue sem passar pela porta. `app/chamados/reconciliador.py:644–652,681–703` |
| Julgar com dono, célula, atos e C20 frescos | **Parcial, NOVO.** A releitura traz a FK do dono, a célula e `decisoes`. **Falta** frescor da empresa, do corte e da tranca usados pelo C20. `app/chamados/reconciliador.py:647–656` · `app/chamados/juizes.py:1037–1075,1101–1117` |
| Carimbar pela porta única e contar somente gravação | **Entregue, NOVO no receptor; PREEXISTENTE na porta e no contrato.** `fresca` vai ao `carimbar`. Só `True` incrementa `n` e permite enfileirar a disputa relida. `app/chamados/reconciliador.py:656–675` · `app/chamados/models.py:3330–3349,3375–3405` |
| RED da resposta interposta e controle | **Entregues no diff base → Y.** A prova usa a porta real de resposta e exige tentativa de carimbo com `RECUSA_C6`. O controle exige uma gravação pelo usuário de sistema. GREEN e execução do mutante **[nao verificado]**. `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:115–167` |
| A2 invertido, gêmeo órfão e literalidade | **NOVO.** O A2 `celula_absolveu` foi invertido e passou a chamar o consumidor diretamente. O gêmeo órfão conserva o comando e a expectativa anterior. A volta 2 registrou que o A2 estava fora dos arquivos autorizados. **Falta** o aval nominal posterior que resolva esse ponto e o texto integral do rascunho 3.3 citado no APROVA. A afirmação “por inteiro e nada além” fica **[nao verificado]**. `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:200–247,268–274` · `app/chamados/management/commands/consumir_eventos_celula.py:17–35` · `CLAUDE.md:37–38` |
| Complementos 1 e 2 | A matriz de 26 campos consta do `r.txt` fornecido, com numeração de linha **[nao verificado]**. A célula de **instante do C20** reconhece o memo, mas aponta a chamada sob a trava como conferência de frescor. O código demonstra a leitura anterior. O Complemento 2 substitui os itens 1–3 do primeiro. Brief efetivo, linha `BRIEF COM 4 PERGUNTAS 3.3: sim, campos=N` no RELATO e registro no mapa de falhas **[nao verificado]**. O diff fornecido contém quatro arquivos e deixa wrapper e aquisição da trava fora das alterações. `diff.patch:1` · `app/chamados/reconciliador.py:644–656` · `app/chamados/juizes.py:1067–1075,1101–1117` |
| Teto medido e atribuído | **Parcial, NOVO.** Os valores batem com os dois totais fornecidos. **Falta** preencher o commit nas quatro linhas de atribuição. A contagem por verbo e a ausência de delta de escrita **[nao verificado]**. `medida.txt:3,6` · `app/ponto/tests/test_selo_chokepoint_escrita.py:206–209,233–236` |

## 2. Leituras antes e depois da trava

- **PREEXISTENTE — pré-seleção.** Antes da trava própria do E2, o gerador aplica o universo SQL, traz relações e atos, consulta C6 e C20 e só então emite `p`. Isso decide quem chega à trava. Uma pergunta descartada nessa fase fica para outra passagem. `_cid` vem do argumento, e `p.pk` e `p.disputa_id` vêm da pré-seleção e são usados para delimitar a releitura. O autor `sistema` também é obtido antes. `app/chamados/reconciliador.py:632–649,712–734`
- **PREEXISTENTE — trava.** O `atomic` entra em `:644`, a porta é chamada em `:645` e o `SELECT FOR UPDATE` efetivo do colaborador está em `models.py:2876`. **NOVO — releitura.** A consulta em `:647–649` reaplica dono, `materializada_em IS NULL`, `validada_em IS NULL`, FK da célula e veredito elegível. Ela lê a disputa, a célula com veredito, via, DNA e ata, o chamado de destino e, no prefetch, os atos. A conferência do dono usa a FK da disputa relida. `app/chamados/reconciliador.py:644–652,688–703` · `app/chamados/models.py:2830–2848,2874–2878`
- **NOVO — TOCTOU do C20.** A foto do dono é descartada como objeto em `:653`, que instala `colab` da pré-seleção. Pelo mesmo PK, C20 pode usar a empresa e o corte que já estavam em memória. O memo da instância ou do request armazena também o `None` de `periodo_trancado`. Se o gerador leu `None` e outra transação grava a tranca antes de o E2 obter a linha do colaborador, a segunda chamada usa o mesmo `None`. O C6 da porta volta ao mesmo C20 e pode aceitar o carimbo. O memo e o uso pré-trava são **PREEXISTENTES**. O reaproveitamento como suposta releitura sob a trava é **NOVO**. Ocorrência em produção **[nao verificado]**. `app/chamados/reconciliador.py:653–656,716–734` · `app/chamados/juizes.py:991–997,1037–1075,1101–1117,1406–1415` · `app/ponto/services/fechamento.py:1400–1429,1464–1470`
- **NOVO — C6 recebe a pergunta relida.** A composição lê `decisoes` da instância nova, texto da coluna, veredito e lâmpada da célula relida. O julgamento roda dentro da porta com o carimbo aplicado em memória. A trava do colaborador serializa, por exemplo, a resposta humana e a validação pelos caminhos citados. **PREEXISTENTE — limite da trava.** A lavra do cartório tem caminho de gravação da célula sem adquirir essa trava na porta da célula. A releitura melhora a foto da lâmpada, mas a trava do E2 por si só não a mantém fixa até o `save`. `app/chamados/reconciliador.py:648–656` · `app/chamados/models.py:3361–3405,4361–4367,4382–4390` · `app/chamados/juizes.py:1335–1380,1423–1474` · `app/chamados/services/disputa_emissao.py:1478–1485` · `app/chamados/services/validacao.py:751–760` · `app/ponto/services/cartorio.py:1898–1922` · `app/ponto/portas/celula.py:957–965,1181–1202`

## 3. Escritores, leitores e significado

Na matriz, `R` é `app/chamados/reconciliador.py`, `M` é `app/chamados/models.py` e `J` é `app/chamados/juizes.py`.

| Campo, valor ou função | Fonte, escritor e leitores |
|---|---|
| `p.pk`, `disputa_id`, `colaborador_id`, `_cid` e registro da trava | A pergunta recebe disputa no nascimento. O E2 guarda o PK do argumento e confere o dono da disputa relida no registro transacional. `trava_na_ordem` escreve o registro, e E2 e resposta o leem pela guarda. A associação em `R:653` escreve a relação em memória, com o mesmo PK e potencialmente dados antigos. `M:1892–1896,3224–3234,2874–2878` · `R:635–653` · `app/chamados/services/trava_do_colaborador.py:29–63` · `app/chamados/services/disputa_emissao.py:1478–1485` |
| Universo e emissão | `qs_perguntas_absolviveis` define o filtro SQL. O gerador aplica C6 e C20 e emite candidatas. O E2 grava pela porta e o vigia conta emissões de colaboradores sem evento pendente. São contagens de etapas distintas. `R:644–659,681–734` · `app/chamados/management/commands/reconciliar_chamados.py:89–93,109` |
| `decisoes` e `_pv_memo` | `DecisaoPergunta.registrar` insere os atos e invalida o prefetch e o memo do C6 na instância entregue. `leitura` ordena os atos, incorpora o passivo e fornece a composta para C6, C7 e o vigia. C6 escreve e lê seu memo por instância. A instância nova resolve a reutilização desse memo do C6, enquanto C20 usa outro memo. `M:2017–2035,4347–4399,4428–4453` · `J:895–908,1101–1117,1406–1414` · `app/chamados/services/vigia_ato_x_coluna.py:8–24` |
| `respondida_em` e `resposta_colab` | A resposta humana grava par e ato. As portas automática, de recomposição pela trilha e de reabertura também escrevem o par ou uma de suas partes conforme a operação. A composta dá ao C6/C21 e ao C7 o instante do ato e o texto da projeção. A comparação com a lâmpada lê esse texto. A face SQL de resposta usa as colunas físicas. `app/chamados/services/disputa_emissao.py:1499–1537,1630–1679,1689–1737` · `M:2022–2027,3649–3667,4382–4390` · `J:1359–1364,1426–1440,1551–1560,1583` |
| `validada_em`, `validada_por_id` e ato `VALIDACAO` | A porta de validação grava a projeção e o ato. Resposta, devolução, reabertura e `carimbar` no modo validar também mexem nas colunas. O seletor lê presença física. C6 e C7 leem validação vigente pela composta. O vigia compara explicitamente as duas fontes. **PREEXISTENTE:** `(False, True)` fica fora do SQL, embora o ato diga “sem validação”. `(True, False)` entra no SQL, o C6 retorna morta pelo ato e a guarda condicionada à coluna preenchida não roda. Se os demais filtros passarem, a porta pode gravar `celula_absolveu` nesse segundo par. Incidência **[nao verificado]**. `R:688–703` · `M:3367–3405,4382–4399` · `J:1406–1411` · `app/chamados/services/validacao.py:161–175,284–296` · `app/chamados/services/disputa_emissao.py:1503–1514,1630–1648` · `app/chamados/services/vigia_ato_x_coluna.py:14–20` |
| `celula_id`, `data_evento`, `conversa`, `motivo`, `tipo_resposta` | Nascimento e rechaveamento gravam vínculo e dia. `gravar_dia_do_fato` atribui o dia. C20 e C6 leem com o discriminador `conversa`, pois o dia civil da conversa não identifica competência de fato. Motivo e tipo guiam o marco, a comparação da resposta e a porta de resposta. O outbox lê o vínculo e o dia. `M:2623–2652,3224–3234,3472–3496` · `app/chamados/services/fio.py:843–854,1059–1096` · `app/chamados/services/disputa_emissao.py:1449–1473` · `J:337–341,927–953,1347–1364` |
| `celula.veredito`, `veredito_via`, DNA, ata e lâmpadas | A porta da célula leva DNA e lavra. O cartório monta a ata, inclusive `pendente`; o admin também lavra pela porta da célula. O seletor lê o veredito. C6, o juiz do marco e o lastro leem veredito, via e lâmpadas. “Veredito não acusa” delimita o universo. “Marco provado” exige a leitura do juiz. `R:692–703` · `app/ponto/portas/celula.py:294–306,1032–1067,1090–1098` · `app/ponto/services/cartorio.py:925–935,1898–1922` · `app/ponto/services/veredito_celula.py:352–356` · `app/ponto/services/lastro.py:138–150` · `J:935–976,1423–1474` |
| Empresa, corte, tranca, toque e memo do C20 | Cadastro muda o corte e transferência muda a empresa. As portas de fechamento escrevem a tranca e a reabertura. K7 lê `reaberto_em` e C20 usa `fechado_em` para pesquisar a trilha de cobrar, resolver e desarquivar. Gerador, E2, C6, C7 e tarefa do dia leem o mesmo juiz. O significado é tranca vigente sem toque posterior. O E2 mistura instantes. `app/colaboradores/views.py:538–542,1413–1418` · `app/ponto/janelas.py:36–63` · `app/ponto/services/fechamento.py:1400–1441,1464–1470` · `app/chamados/services/cobrar_dia.py:39` · `app/ponto/services/veredito_celula.py:356` · `app/chamados/views_arquivar.py:77` · `J:991–997,1067–1085,1217–1331,1414` · `M:2027` · `app/chamados/services/tarefa_do_dia.py:25` |
| `chamado_destino`, `status_local` | Nascimento/reuso e costura gravam a FK, com `SET_NULL` na exclusão. A transição grava `status_local`. O memo do C6 lê o estado do destino carregado, e o outbox acompanha a FK. `M:213–240,3463–3476,3691–3698` · `app/chamados/services/fio.py:857–876` · `J:846–864` |
| Autor, relógio, trio e retorno da porta | `get_user_sistema` fornece o autor. `carimbar` lê seu relógio, aplica o trio pelo escritor do modelo e salva após C6. As portas de identidade e descarimbo também operam no trio. O seletor, a composta, o lastro e o outbox leem o carimbo. `_gravou` é o retorno da porta; `recusa_do_carimbo` é diagnóstico da tentativa em memória, lido pelo RED e pela materialização. A porta lê também a foto da lavra para restauração em falha. `app/core/utils.py:6–18` · `M:3320–3405,3416–3460,3463–3476,3537–3539,4388–4390` · `J:684–710` · `app/chamados/services/materializacao.py:2222–2228` · `app/chamados/veredito_lavrado.py:270–289` · `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:126–129,153–167` |
| `n`, `_disputas`, `fechada_em`, C7 e evento | O E2 conta retornos `True` e deduplica a fila por PK. O consumidor soma `out['perguntas']`, a reconciliação do colaborador soma ao total e o cartório segue para o gate sem usar o retorno. C7 lê as perguntas e os atos; `fechar` grava instante, autor, resolução e motivo, que alimentam a trilha e o sinal do pai. Reabertura limpa os campos. A marca do `EventoCelula` é do consumidor e não prova releitura da pergunta. `R:657–678,1427–1429,1455–1473,1496–1506` · `M:2022–2035,2071–2097,2118–2126,2335–2363` · `app/chamados/signals.py:20–46` · `app/ponto/services/cartorio.py:1018–1033` |

## 4. Mordida e relógio dos testes [nao verificado]

- **RED — morde a reversão do reconciliador por inspeção.** A resposta real `19:45` precisa estar no banco e o C6 precisa considerá-la viva. A asserção exige trio vazio, contagem zero e exatamente uma tentativa com `RECUSA_C6`. O caminho antigo entrega `p` com a projeção anterior. O relato do construtor cita falha no trio e na contagem, mas o `red_tail.txt` bruto e a execução do mutante em Y estão **[nao verificado]**. `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:126–167` · `app/chamados/reconciliador.py:636,647–656`
- **A2 — morde a reversão à base, mas pode passar sem provar a releitura.** A validação com ato e timestamp só acontece depois que o gerador real emite o PK alvo. Se o gerador nunca emitir, o teste falha na validação esperada. Se o E2 descartar a candidata **depois** da emissão e **antes** da releitura, as asserções podem passar. Um mutante estático para demonstrar a lacuna é pular candidatas com `p.respondida_em` preenchido antes da consulta nova. A fixture do A2 já nasce respondida. O espião também só anota depois que a porta retorna, então uma exceção engolida pelo E2 pode produzir a lista vazia esperada. `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:71–91,169–188,200–247` · `app/chamados/reconciliador.py:636–663,1455–1465`
- **C20 — sem mordida para a janela de corrida nova.** O teste de tranca já nasce trancado, logo o gerador pode vetar antes de o E2 alcançar a chamada nova. As duas outras intercalações mudam resposta ou validação. Esses casos não distinguem remover a chamada em `:654` nem manter o memo antigo depois da trava. `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:89–108,131–157` · `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:169–188` · `app/chamados/reconciliador.py:653–656,723–730`
- **Relógio.** RED e A2 têm congelamento de classe. No RED, o `yield` fica dentro do segundo congelamento, que cobre o carimbo do E2. O selo SQL ainda usa `timezone.localdate()` para montar o dia da fixture, **PREEXISTENTE**. Em produção, a porta lê `now()` depois da trava. O C6 ainda pode recorrer a `date.today()` no degrau da janela, **PREEXISTENTE**, com o mesmo caminho alcançável na chamada nova. `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:36,138–151` · `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:31–46` · `app/ponto/tests/test_selo_chokepoint_escrita.py:304–323` · `app/chamados/models.py:3361,3375–3376` · `app/chamados/juizes.py:589–598,1475–1482`

## 5. Selo de chokepoint

**NOVO — linear por candidata.** Para `N` candidatas emitidas e `K` que sobrevivem ao filtro relido, o trecho acrescenta `N + K` SELECTs de pergunta e atos. Uma excluída custa um SELECT, uma elegível custa dois, antes de eventual custo do C20. O teste varia o número de **chamados** que fecham e verifica esse número. Ele não varia nem afirma a cardinalidade de candidatas do E2. Dois deltas iguais nesses cenários não demonstram custo fixo por batida. A desigualdade do selo morde excesso de queries nesses dois casos, e deixa passar uma reversão que reduza o total. `app/chamados/reconciliador.py:636–656` · `app/ponto/tests/test_selo_chokepoint_escrita.py:325–334,444–481`

**NOVO — tetos sem folga nesses dois cenários, atribuição incompleta.** No diff, só sobem `234 → 236` e `298 → 300`, exatamente os totais informados. A anotação identifica os dois sítios de leitura e diz que o custo é por candidata. As quatro anotações ainda têm `commit <a preencher pela sessao>`. O anexo mede totais, sem discriminar `SELECT`, escrita ou candidatos. A atribuição dos dois SELECTs vem da leitura do código. Delta de escrita medido e GREEN com o teto final **[nao verificado]**. `medida.txt:3,6` · `app/ponto/tests/test_selo_chokepoint_escrita.py:206–209,233–236` · `app/chamados/reconciliador.py:647–649`

## 6. Checklist final

- **Segundo juiz e derivação paralela — zero novos no diff.** O código reaplica o seletor existente e chama C20 e C6 registrados. A segunda chamada ao C20 acrescenta uma consulta ao mesmo juiz com insumo potencialmente velho. `app/core/juizes.py:290–304` · `app/chamados/reconciliador.py:647–656,681–703`
- **Segundo escritor fora da porta — zero novos em produção no diff.** A gravação continua em `carimbar`. Os escritores preexistentes dos campos compartilhados e os limites de suas travas constam da matriz. `app/chamados/reconciliador.py:652–661` · `app/chamados/models.py:3324–3328,3361–3405`
- **Ordem de trava — aquisição PREEXISTENTE, nenhuma inversão nova no hunk.** Colaborador primeiro. A guarda de fecho da disputa julga antes do seu próprio `trava_na_ordem`, janela **PREEXISTENTE**. `app/chamados/reconciliador.py:644–656,669–675` · `app/chamados/models.py:2071–2097,2830–2848`
- **Campo com dois significados — PREEXISTENTE.** Validação física delimita o universo e validação vigente é julgada pela composta. O E2 novo reaplica essa diferença. `data_evento` distingue dia civil de conversa e dia de turno através do discriminador existente. `app/chamados/reconciliador.py:688–703` · `app/chamados/models.py:4382–4399` · `app/chamados/juizes.py:337–341,1406–1411`
- **Pouso e pré-requisitos — [nao verificado].** A ordem exigida é pré-verificação, suíte inteira verde e opus final. Os anexos não demonstram o cumprimento dessa sequência. `CLAUDE.md:372–381` [nao verificado]

**AUTOCONFERÊNCIA BRIEF-SEM-EXCEPT — `except` adicionados: produção 0, testes 0. Fallbacks adicionados: 0.** No diff fornecido, o `except Exception` do laço aparece como contexto. Os tratadores de carimbo, fecho e seleção são **PREEXISTENTES**. O primeiro agora também captura falhas dos novos SELECTs e da nova chamada ao C20, registra e segue. A contagem foi feita nas linhas adicionadas do patch fornecido. A execução de `git diff da copia | grep -n except` fica **[nao verificado]** nesta cópia exportada. `app/chamados/reconciliador.py:644–663,669–677,729–733` · `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:115–167` · `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:200–247` · `app/ponto/tests/test_selo_chokepoint_escrita.py:206–209,233–236`

## 21/11 18:53 · sessao 68978cf9ffb4 · - · - · origem=Ronald (tmux)

A troca de `p.respondida_em is None` por `texto_sem_carimbo(p)` em `f3d23021f` restringe a guarda somente quando o texto está vazio após `strip()`. Com `hora`, materialização visível, via vazia e ambas as validações ausentes, aeb repassava texto vazio ou espaços e f3 repassa `''`. O derivador trata ambos da mesma forma. A composta fornece o próprio texto da coluna e o veto temporal antecede a guarda. O predicado de integridade continua reconhecendo o meio-par ilegal que a porta recusa gravar. O uso do texto como coordenada histórica pertence à guarda, enquanto C21 continua a decidir vigência. Executei 18.144 combinações em memória com o derivador real e a composta simulada, com zero resultados divergentes entre aeb reconstruído e f3. [juizes.py](/home/ronald/codex_ws/f3d23021f4cc0aa6f6e72efa1d6350647488ded9/app/chamados/juizes.py:369) · [models.py](/home/ronald/codex_ws/f3d23021f4cc0aa6f6e72efa1d6350647488ded9/app/chamados/models.py:4382) · [materializacao.py](/home/ronald/codex_ws/f3d23021f4cc0aa6f6e72efa1d6350647488ded9/app/chamados/services/materializacao.py:691) · [fio.py](/home/ronald/codex_ws/f3d23021f4cc0aa6f6e72efa1d6350647488ded9/app/chamados/services/fio.py:1704)

**Novo contra a base descrita no anexo.** Reproduzi em memória `hora`, sem marco fixo, com `06:30` residual, resposta desfeita na composta e batida viva vinculada às `05:24`. O hash final retorna `PLANTIO_SEM_TS` e confirma a materialização só pelo vínculo. Mantendo todo o resto e repondo a leitura do texto cru descrita para a base, o juiz recebe `SLOT(06:30, S)` e rejeita a batida fora da janela. O teste de `hora` afirma apenas o resultado do slot. Os novos testes que barram vínculo e destino são de `data_hora`. [juizes.py](/home/ronald/codex_ws/f3d23021f4cc0aa6f6e72efa1d6350647488ded9/app/chamados/juizes.py:409) · [juizes.py](/home/ronald/codex_ws/f3d23021f4cc0aa6f6e72efa1d6350647488ded9/app/chamados/juizes.py:798) · [test_marco_por_motivo.py](/home/ronald/codex_ws/f3d23021f4cc0aa6f6e72efa1d6350647488ded9/app/chamados/tests/test_marco_por_motivo.py:231)

O S3 elimina somente a sequência velha do `prefetch`. O passivo seleciona pela validação física pré-trava, depois relê materialização, via e autor. Uma nova resposta entre as duas etapas pode invalidar a anterior e alterar seu texto. A composta então junta o ato atual ao texto antigo, e o materializador usa esse texto para plantar sob a trava. A seleção e a leitura de colunas velhas são preexistentes. O `prefetch` acrescentou a sequência velha e o `pop` corrige essa parte. Ocorrência em produção [nao verificado]. [vigia_carimbo.py](/home/ronald/codex_ws/f3d23021f4cc0aa6f6e72efa1d6350647488ded9/app/chamados/services/vigia_carimbo.py:97) · [passivo_validado.py](/home/ronald/codex_ws/f3d23021f4cc0aa6f6e72efa1d6350647488ded9/app/chamados/services/passivo_validado.py:532) · [disputa_emissao.py](/home/ronald/codex_ws/f3d23021f4cc0aa6f6e72efa1d6350647488ded9/app/chamados/services/disputa_emissao.py:1499) · [materializacao.py](/home/ronald/codex_ws/f3d23021f4cc0aa6f6e72efa1d6350647488ded9/app/chamados/services/materializacao.py:1389)

A lista no apoio apenas carrega funções reais via AST para C8b, C8c e pós-suíte. A catraca varre fontes de produção. Sua regra por sítio conta `respondida_em` direto, e o aeb reconstruído produz duas leituras declaradas onde espera uma. O final passa com uma leitura declarada e `texto_sem_carimbo` classificado como `PAR`; retirar a influência da guarda também deixa essa catraca verde. A regra estrutural deixa de alcançar o novo uso por delegação. A fixture de fechamento legado oferece controle positivo indireto, sem execução ORM aqui. A suíte sem banco de C8b e da catraca passou com 46 testes. [e3_c8b_apoio.py](/home/ronald/codex_ws/f3d23021f4cc0aa6f6e72efa1d6350647488ded9/app/chamados/tests/e3_c8b_apoio.py:116) · [test_e3_pendentes_app.py](/home/ronald/codex_ws/f3d23021f4cc0aa6f6e72efa1d6350647488ded9/app/chamados/tests/test_e3_pendentes_app.py:96) · [test_e3_catraca_respondedores.py](/home/ronald/codex_ws/f3d23021f4cc0aa6f6e72efa1d6350647488ded9/app/chamados/tests/test_e3_catraca_respondedores.py:545) · [test_fechar_disputa_materializada.py](/home/ronald/codex_ws/f3d23021f4cc0aa6f6e72efa1d6350647488ded9/app/chamados/tests/test_fechar_disputa_materializada.py:38)

## 21/11 18:53 · sessao 86e7880ec7da · - · - · origem=Ronald (tmux)

**Catraca.** A lista de `e3_c8b_apoio.py:116–124` carrega funções reais por AST no ambiente de testes. A catraca lê fontes de produção e exclui diretórios `test*`. A tolerância efetiva é a lista `FORA_PELA_PERGUNTA`, com contagem exata por sítio. Restaurar `p.respondida_em is None` em memória manteve zero respondedores, mas fez `_slot_esperado` saltar de uma para duas leituras declaradas e a asserção reprovar. `app/chamados/tests/e4_apoio.py:8–16` · `app/chamados/tests/test_e3_pendentes_app.py:96–115` · `app/chamados/tests/test_e3_catraca_respondedores.py:66–86,393–395,437–457,544–557` · `app/chamados/juizes.py:374,411`

**S2 e S2b.** Os casos pedidos estão presentes. O explicador exige a classe, o motivo e a ausência do texto residual. Vínculo e destino caem no retorno `SEM_RESPOSTA_VIGENTE`. Remover esse motivo do retorno permitiria a absolvição pelos dois atalhos. A retratação exige carimbo mais trilha e cobre avulso e carga. O teste HX confirma `PLANTIO_SEM_TS` antes de aplicar os três controles. Execução ORM [nao verificado]. `app/chamados/tests/test_explicador_disputa.py:80–101` · `app/chamados/tests/test_marco_por_motivo.py:167–173,231–269` · `app/chamados/juizes.py:339–343,798–845` · `app/chamados/tests/test_hx_fallback_sem_lei.py:61–86`

**Mordida do legado.** Os 46 casos da catraca e do C8b passaram. Os 38 casos C8b, C8c e pós-suíte selecionado também passaram com `or carimbo_legado` removido da foto. A fixture noturna fornece um par legal. O teste de fio com texto sem instante pode absolver pelo veredito da célula antes do slot. Cobertura ORM específica da exceção de legado [nao verificado]. `app/chamados/juizes.py:411–415,1498–1517` · `app/chamados/tests/e3_c8b_apoio.py:190–205` · `app/chamados/tests/test_e3_c8b_sem_banco.py:404–433` · `app/chamados/tests/test_fio_disputa_resolvido.py:44–50,94–106`

**Foto velha.** O vigia expurga apenas o prefetch da sequência. O RED existente injeta devolução entre classificação e trava e cobra que o ato novo seja visto. A seleção, o agrupamento e as colunas da resposta permanecem pré-trava. O passivo relê só campos da materialização, e a composta combina sequência atual com texto da instância. A guarda posterior compara presença da validação e pode aceitar duas validações presentes de instantes distintos. `app/chamados/services/vigia_carimbo.py:121–123` · `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:95–129,146–156` · `app/chamados/services/passivo_validado.py:330–356,377–384,532–540` · `app/chamados/models.py:3384–3393,4382–4399` · `app/chamados/services/materializacao.py:1332–1336,1389–1395,1956–1971`

Os oito blobs finais coincidem com os oito prefixos `index` do `delta.patch` fornecido. O comando para comparar com `5cc451ac3` falhou ao acessar a base. Total exato de `except` contra a base [nao verificado]. O delta fornecido elimina as duas capturas de `DatabaseError` e acrescenta a exceção semântica que deixa o texto legado alimentar o derivador sem C21. `app/chamados/services/validacao.py:1224–1232` · `app/chamados/tests/test_fila_validar_lote.py:110–123` · `app/chamados/juizes.py:409–419`

## 21/11 18:53 · sessao 66ffcd4f6928 · - · - · origem=Ronald (tmux)

**BLOQUEIA.** Reproduzi em memória uma pergunta `hora` reaberta com texto físico `08:00`, composta sem instante, sem marco fixo e batida vinculada viva às `12:00`. O hash final devolve `PLANTIO_SEM_TS` e aceita a batida pela origem. O controle com o mesmo texto alimentando o derivador devolve `SLOT 08:00 S` e rejeita a batida fora do slot. O relato da volta 1 atribui a leitura crua à base no caminho avulso. A execução da base e a incidência em produção ficam [nao verificado]. O teste que cobre a hora desfeita verifica apenas o slot. `app/chamados/juizes.py:379–382,411–420,798–809` · `app/chamados/tests/test_marco_por_motivo.py:271–282` · `app/chamados/tests/e3_c8b_apoio.py:116–124,143–147`

- **NOVO — fallback de negócio.** `carimbo_legado` deixa o meio-par ilegal informar a busca de testemunha. C21 e `texto_sem_carimbo` conservam seus predicados. A guarda acrescenta um uso de política do predicado de integridade e lê diretamente validação física e do ato, combinação documentada em `par_ato_coluna`. O vigia seleciona coluna de validação preenchida e o lote exige C21 antes do slot, portanto ambos excluem esse uso legado. `app/chamados/juizes.py:379–382,409–419,1587–1603` · `app/chamados/models.py:4382–4399` · `app/chamados/services/vigia_carimbo.py:97–98` · `app/chamados/services/validacao.py:1224–1228`
- **Foto velha — correção parcial.** O vigia limpa o prefetch `decisoes` antes do `yield`. O passivo ainda decide classe, dia, competência, grupo e reversão antes da trava. Sob trava atualiza apenas materialização, via e autor. A composta então pode juntar o instante novo do ato ao texto antigo da instância, e a porta pode usar esse texto no plantio. O reaproveitamento pré-trava é anterior ao delta exibido e a ocorrência em produção fica [nao verificado]. `app/chamados/services/vigia_carimbo.py:103–123` · `app/chamados/models.py:2841–2848,4355–4366,4382–4390` · `app/chamados/services/passivo_validado.py:330–356,372–383,532–545` · `app/chamados/services/materializacao.py:1327–1341,1395,1624–1639,2085–2103`
- **Ordem de trava preservada no delta.** O lote lê classe e conflito antes da trava. A porta individual relê a pergunta antes da própria trava e, dentro dela, revalida K6 antes de carimbar. A janela de concorrência consta da volta 1. `app/chamados/services/validacao.py:689–704,743–760,1074–1096,1224–1232` · `app/chamados/services/acoes_disputa.py:13–22`
- **Rastreio.** A tarefa escreve `agora`, carrega decisões e chama a carga e C6. A carga cria mapa de slot, trilha e testemunhas. O juiz consome os três e a leitura avulsa consulta as fontes diretamente. A tripla do vigia alimenta censo, passivo e vigia do fio. O escritor literal de `retratar_pergunta_obsoleta` em produção fica [nao verificado]. `app/chamados/services/tarefa_do_dia.py:525,533–539,630–637` · `app/chamados/services/carga_pergunta_viva.py:88–95,110–145,186–195` · `app/chamados/juizes.py:287–298,362–387,798–810,845,860–866,1530–1543` · `app/chamados/services/vigia_carimbo.py:94–155` · `app/chamados/services/passivo_validado.py:330–356` · `app/chamados/services/vigia_fio.py:415–418` · `app/chamados/tests/test_marco_por_motivo.py:262–263`
- **Prova sem banco.** Rodei C8b e duas catracas A1 e de respondedores em modo sem escrita. Passaram 56 testes. Suite integral, ORM, PRONTO e número exato de adições `except` desde a base ficam [nao verificado]. `app/chamados/tests/test_e3_c8b_sem_banco.py:210–546` · `app/chamados/tests/test_e4c_f5_selo_chamada.py:188–299` · `app/chamados/tests/test_e3_catraca_respondedores.py:482–879` · `CLAUDE.md:372–381`

## 21/11 18:37 · sessao 528e4946c43e · E4C-F5-CONSTROI · verificador · origem=Code

**BLOQUEIA — E4C-F5 item 1.2.**

1. A validação pública ainda carimba e planta por uma entrada que contorna `travar_o_ato_do_plantio`. `validacao.py:697–760,786–805`
2. O passivo ainda usa dados decisórios lidos antes da trava, incluindo competência, exportação e a conferência de data das perguntas avulsas. `passivo_validado.py:334–396,462–478,549–557` · `materializacao.py:1922–1928,1947–1970`
3. O caminho do flip pode adquirir Batida → Colaborador, enquanto o plantio pode adquirir Colaborador → Batida. Deadlock observado **[nao verificado]**. `flip_automatico.py:62–79` · `ponto/signals.py:206–218` · `chamados/reconciliador.py:635–646,1486–1487` · `materializacao.py:1418,1586–1590,2145–2146`
4. O censo AST da cópia diverge em quatro entradas da catraca de linhas. O RED, os mutantes e a suíte inteira deste hash, com veredito terminal, `Ran` e RC, estão **[nao verificado]**. `core/medida_turno_do_par.py:448–454` · `core/tests/test_medida_turno_do_par.py:271–274,558–562` · `CLAUDE.md:119–123,372–381`

### 1. P1, P3, P5, P7 e descartados

A correspondência individual de **P1, P3, P5 e P7** e a lista nominal dos descartados estão **[nao verificado]**. O `r.txt` fornecido descreve os achados por seção, sem a triagem numerada. Não localizei o diff integral `b0935c1f1..6a5ab520c` na cópia exportada. `bin/codex_pergunta.sh:311–319`

| Achado identificável no `verif1` | Situação na cópia |
|---|---|
| Candidatas fora da trava | **Fechado para as linhas de `PerguntaDisputa` examinadas.** Todas as candidatas da consulta entram na trava, inclusive sem resposta ou com eleição ambígua. O helper relê a eleição e o conjunto, e recusa mudança ou PK faltante. As linhas de `DecisaoPergunta` vêm por `prefetch` e ficam fora do `FOR UPDATE OF self`. `materializacao.py:139–157,2123–2158` · `models.py:2900–2904` |
| Foto velha da principal no passivo | **Fechado para mudanças nos cinco valores da composta.** A comparação ocorre antes da unidade de trabalho. Os demais insumos continuam com as lacunas do item 3. `passivo_validado.py:360–362,382–401` · `models.py:4382–4390` |
| Barreira, ordem e selos | **Corrigidos no código dos testes.** A barreira rompida agora sobe como falha. O teste inverso usa o mapa retornado, verifica o SQL das quatro travas e cobra duas batidas com uma trilha por batida. Há casos dirigidos para as guardas e para o reset do contexto. A execução neste hash está **[nao verificado]**. `test_e4c_f5_plantio_concorrente.py:63–132,189–263` | [nao verificado]
| Entrada pública e fila inicial | **Continuam abertas.** A entrada pública escreve antes de chegar ao laço protegido. O laço ainda seleciona pela face SQL da resposta e pela coluna física `validada_em`. O passivo também começa pela coluna física. Uma principal excluída nessa etapa fica fora da releitura. `validacao.py:743–760,786–805` · `materializacao.py:2194–2225` · `vigia_carimbo.py:94–116` |

O uso do derivador pela tela pode ficar em fila, por ser leitura. O uso da mesma informação para escolher travas, validar e plantar faz parte do ato coberto pelo aval. A inversão com o flip também permanece pertinente à verificação da ordem. `aval.txt:1` · `materializacao.py:795–799` · `validacao.py:714–723,743–760,786–803` · `flip_automatico.py:62–79` · `chamados/reconciliador.py:635–646`

### 2. Quem escreve, quem lê e para onde vai o erro

- **`estados_classificados`.** `reparar` cria o dicionário local e é seu único escritor. Em `apply`, guarda a composta por PK após a classificação e os recortes. O único leitor é a comparação em `:392`. Depois de travar, `reparar` carrega novas instâncias com `prefetch_related('decisoes')`. As instâncias de `atuais` servem à comparação. `_trabalhar_o_par` recebe os `itens` anteriores. `passivo_validado.py:332–362,388–396`
- **Fontes da composta.** `DecisaoPergunta.composta` lê os instantes de resposta e validação pela sequência, o texto da projeção e o carimbo e via das colunas. Resposta humana, automática e pela trilha alimentam resposta e sequência. Validação e devolução assinam ou limpam a validação e registram o ato. `registrar` insere a sequência. O modelo escreve e desfaz carimbo e via. `models.py:24–25,3324–3328,3363–3374,3394–3405,3451–3456,4361–4390,4428–4453` · `disputa_emissao.py:1499–1537,1707–1737` · `validacao.py:161–175,269–296`
- **Quem levanta.** O helper levanta `AtoDoPlantioMudou` por mudança de escopo da principal, troca da irmã ou inconsistência no conjunto de candidatas. O passivo levanta a mesma exceção quando a composta da principal difere da foto. `materializacao.py:2112–2113,2150–2158` · `passivo_validado.py:388–393`
- **Quem captura no passivo.** `reparar` captura após sair do `atomic`, recusa o grupo inteiro no balde `escopo_do_plantio_mudou`, guarda a mensagem e segue. O comando salva o resultado e imprime o balde e `ERRO`. A exceção fica absorvida, com diagnóstico. Esse caminho termina normalmente se o restante do comando completar. RC observado **[nao verificado]**. `passivo_validado.py:382–401,437–440` · `materializar_passivo_validado.py:100–141,241–249,272–274`
- **Quem captura no laço da disputa.** O laço captura antes da escrita, acrescenta a mensagem a `erros` e passa à próxima pergunta. A validação pública distingue erros próprios e de irmãs. As cascas de regularização podem retornar antes de detalhar o motivo. O backfill descarta o retorno do laço. `materializacao.py:2210–2236` · `validacao.py:803–854` · `acoes_disputa.py:143–149,164–179` · `fio.py:722–728,745–758` · `backfill_fio_morto_e4b.py:153–159`

### 3. TOCTOU

- **Validação pública.** A composta, os gates de resposta e implausibilidade e a irmã inicial são lidos antes da trava. `trava_na_ordem` recebe os chamados dessa irmã, sem os PKs das perguntas. O código carimba a principal antiga e `_validar_o_par` pode eleger outra irmã e plantar antes da varredura protegida. `validacao.py:648–659,693–760,786–805` · `models.py:2900–2903`
- **Passivo.** `classe` e `ts` são obtidos antes da trava. O `ts` pode determinar o dia quando falta `data_evento`. Dia, competência, corte e `exportada` ficam no agrupamento usado para decidir geometria, regeneração e pauta. A unidade pode recalcular a hora da batida e ainda trabalhar com aquele agrupamento. A exportação tem escritor independente da trava do colaborador. A foto de reversão também é anterior à trava e permanece no resultado mesmo quando o grupo é recusado. `vigia_carimbo.py:97–116` · `passivo_validado.py:102–132,334–401,462–478,495–505,547–577` · `materializacao.py:1404–1418` · `folha/views.py:297–302` · `folha/models.py:174–188`
- **Contraexemplo na principal avulsa.** Uma confirmação de data pode gravar `data_conferida_em` entre a classificação e a trava, sem alterar a composta ou a foto estrutural. O passivo atualiza somente carimbo, via e autor naquela instância. O refresh total da unidade é restrito às metades de intervalo. A avulsa pode então gravar uma recusa `DATA_EM_DISPUTA` baseada na conferência antiga. `validacao.py:1008–1016` · `models.py:4382–4390` · `passivo_validado.py:549–557` · `materializacao.py:1922–1925,1947–1970,2116–2120`
- **Outras leituras da escrita.** O fecho do destino e dos clones consulta outras perguntas creditadas, e o par consulta batidas, vizinhança e célula. As quatro travas cobrem as tabelas e os PKs enumerados pelo helper. A proteção integral dessas outras leituras está **[nao verificado]**. `materializacao.py:219–247,447–474,1302–1322,2135–2146` · `juizes.py:2121–2126` · `models.py:2875–2904`

### 4. RED de `6a5ab520c`

- **Outra conexão, commit antes da trava.** O teste usa `TransactionTestCase`, chama a porta real de devolução em outra thread e abre `atomic(durable=True)`. A saída do `atomic` efetiva o commit antes de o `Future` devolver o PK. Só então o passivo chama a trava real. A corrida reproduzida é a devolução já confirmada entre classificação e trava. O PID desse RED específico está **[nao verificado]**. `test_e4c_f5_guarda_plantio_s144.py:101–111,298–327` · `validacao.py:269–296` [nao verificado]
- **`if False` em `:392` deve ficar VERMELHO por leitura estática.** A fixture usa `orfao_14h`, sem eleição de irmã, e a devolução muda a validação e a sequência sem mudar a foto estrutural. Retirada a reconfirmação, o único produtor do balde esperado deixa de ser acionado. O teste exige exatamente `{'escopo_do_plantio_mudou': 1}` e nenhuma tentativa de carimbo. O teste isolado também exige que `_trabalhar_o_par` fique sem chamada. Execução do mutante **[nao verificado]**. `passivo_validado.py:389–401` · `materializacao.py:139–145,2116–2127` · `test_e4c_f5_guarda_plantio_s144.py:44–58,113–121,165–202` [nao verificado]
- **Limpeza e relógio.** O worker fecha a conexão em `finally`, e o executor aguarda a thread. A devolução fica commitada para a asserção por PK. A limpeza das linhas cabe ao `flush` do `TransactionTestCase` no banco de teste. Teardown efetivo **[nao verificado]**. Nesse RED o relógio de negócio está congelado na thread principal e no worker, e `hoje` é explícito. O worker fixa `lock_timeout` de 4 s. O limite de 15 s do `Future` limita a espera e não cancela a thread. `test_e4c_f5_guarda_plantio_s144.py:30–58,110–111,133–155,298–327` · `config/settings/ci.py:6–17`

### 5. Checklist 5(a)

| Classe | Resultado |
|---|---|
| `except` e fallback | **Total contra a base [nao verificado].** No delta fornecido há **2 `except` acrescentados em teste, 1 removido e 0 acrescentados em produção**. `BrokenBarrierError` sobe pelo `Future` como `AssertionError` com causa. `AtoDoPlantioMudou` vira retorno do worker. Em produção, a nova reconfirmação alimenta a captura do passivo, que registra a recusa e segue. O delta fornecido não acrescenta plantio alternativo em caso de erro. `test_e4c_f5_plantio_concorrente.py:75–103` · `passivo_validado.py:388–401` | [nao verificado]
| Segundo juiz | A eleição continua chamando o C21 registrado sobre a composta. Há uma divergência no leitor de exportação do passivo. `_esta_exportada` aceita o último TXT marcado vigente mesmo quando invalidado. O K6 registrado exclui esse caso. A precedência desse achado em relação à base está **[nao verificado]**. `core/juizes.py:305,342` · `materializacao.py:146–154` · `passivo_validado.py:120–132` · `folha/haiku_export.py:38–54,59–75` · `folha/models.py:203–225` · `folha/export.py:580–594` |
| Segundo escritor fora da porta | A validação pública reutiliza os escritores de validação, batida e carimbo. A segunda entrada para esses escritores continua sem a mesma porta de trava. `validacao.py:648–659,743–760,786–803` · `materializacao.py:1531–1539,2042–2046` |
| Relógio fora da borda | A reconfirmação não lê relógio, e o `hoje` do passivo chega do comando. O fluxo de escrita ainda lê instantes distintos na validação, aprovação da batida e carimbo. O teste inverso congela a resposta, mas valida e materializa fora desse congelamento. `models.py:4348–4390` · `materializar_passivo_validado.py:85–89,103–114` · `validacao.py:161–175` · `materializacao.py:1531–1539,2009` · `test_e4c_f5_plantio_concorrente.py:31–39,75–89` | [nao verificado]
| Derivação paralela | Para `hora`, classificação e plantio usam `_ts_tipo_do_plantio`. Para `data_hora`, a cópia tem dois pontos de parse do slot. A novidade contra a base está **[nao verificado]**. `juizes.py:344–379` · `materializacao.py:1404–1405,1634–1650` |
| Selo e ordem de trava | O teste inverso inspeciona SQL real em `atomic`, exige PIDs distintos e `ASC` nas quatro tabelas, uma tentativa vencedora e uma trilha por batida. Ele chama a unidade diretamente e não cruza com o flip. A rodada com mutante real da ordem está **[nao verificado]**. `models.py:2875–2904` · `test_e4c_f5_plantio_concorrente.py:41–47,63–132` · `flip_automatico.py:62–79` | [nao verificado]
| Campo com dois significados | O `ContextVar` faz dois papéis. Guarda a eleição por PK e sinaliza que a principal deve ser relida. O `.get(p.pk)` produz o mesmo `None` para PK ausente e eleição confirmada sem irmã. O token é restaurado em `finally`. `materializacao.py:202–207,1832–1842,1922–1925` |

**Catraca medida em memória.** O censo da cópia encontrou `[448, 469]` em `_plantio_preenche_furo_na_data`, `[117]` em `_dia_do_plantio`, `[567, 569]` em `_plantar_o_dia` e `[355]` em `reparar`. O selo ainda exige `[438, 459]`, `[113]`, `[550, 552]` e `[351]`, respectivamente. Essa comparação exata já diverge na cópia examinada. `core/tests/test_medida_turno_do_par.py:271–274,558–562` · `materializacao.py:448,469` · `passivo_validado.py:117,355,567–569`

## 21/11 18:36 · sessao 4bb167c1a815 · - · - · origem=Ronald (tmux)

- `estados_classificados` nasce e é preenchido apenas em `reparar` com `apply`, após classificar e aplicar os recortes. O único leitor compara a composta atual após a trava. A nova consulta não substitui as instâncias de `itens`. A composta inclui validação e resposta do ato, texto da projeção, carimbo e via. `passivo_validado.py:332–362,382–396` · `models.py:24–25,4361–4390`
- `AtoDoPlantioMudou` sai do helper por mudança da principal, eleição ou composição do conjunto e do passivo quando a composta muda. O passivo captura fora do `atomic`, conta o grupo inteiro, conserva o motivo em `erros` e segue. O comando grava e imprime o erro sem convertê-lo em `CommandError`. No laço da disputa a captura ocorre antes da escrita e o erro vira item da lista retornada. `materializacao.py:2112–2113,2152–2158,2210–2236` · `passivo_validado.py:382–401,429–440` · `materializar_passivo_validado.py:100–141,248–249,272–274`
- Validação, ações da disputa e fio consomem a lista retornada. Validação separa erros próprios e de irmãs. Ações e fio registram a contagem, mas retornam avisos ou sucesso sem detalhar erros em certos ramos. A validação direta elege a irmã antes de travar, não fornece os PKs de perguntas à trava e pode plantar antes do laço. `validacao.py:743–760,786–854` · `acoes_disputa.py:143–149,164–179` · `fio.py:722–728,745–758`
- A conferência de cinco atributos não cobre classe, dia, competência, exportada, foto de reversão ou campos do gate de data. `exportada` fica em cache antes da trava e decide se haverá regeneração e pauta. A folha escreve exportação concorrente por sua própria porta. Para uma principal avulsa, `data_conferida_em` pode permanecer antigo mesmo com composta igual, pois o refresh total da unidade é restrito às metades de intervalo. `passivo_validado.py:102–132,334–379,390–396,462–478,545–577` · `folha/views.py:297–302` · `folha/models.py:174–188` · `materializacao.py:1922–1928,1947–1970,2116–2120` · `validacao.py:1008–1016`
- O RED usa a porta de devolução em thread separada, com `atomic(durable=True)` e fechamento da conexão em `finally`. A `Future` termina antes da trava real. A mutação em `passivo_validado.py:392` deve falhar estaticamente porque o `orfao_14h` não tem eleição de irmã e a devolução não muda as coordenadas da foto. O teste exige a recusa exata e nenhuma chamada de carimbo. Execução, PID desse RED, limpeza efetiva e RC **[nao verificado]**. `test_e4c_f5_guarda_plantio_s144.py:51–57,101–121,298–327` · `materializacao.py:143–145,2116–2120` · `validacao.py:269–296` [nao verificado]
- Os cinco blobs coincidem com o lado novo do delta fornecido. Diff inteiro contra a base e triagem numerada **[nao verificado]**. O delta fornecido acrescenta duas linhas `except` de teste e remove um `except RuntimeError`. A barreira sobe como `AssertionError` por `Future`. A recusa esperada vira retorno do worker. `bin/codex_pergunta.sh:311–319` · `test_e4c_f5_plantio_concorrente.py:75–103`

## 21/11 18:35 · sessao 2fc389242459 · FERIADO-POR-POSTO · verificador · origem=Code

**BLOQUEIA.** A volta 4 corrige o `rc 0` quando `MarcaInvalida` sai da porta. Na comparação com `main`, seguem três achados **NOVOS**: `--apply` sem motivo ainda imprime erro e retorna normalmente, a prévia classifica um par antes do encerramento do turno e a API da prévia lê o relógio quando o chamador omite `agora`. A releitura em `celula.py:464` é **PREEXISTENTE**, com prova pareada em memória. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:118–130` · `app/colaboradores/services/feriado_do_posto.py:370–385,449–474` · `app/ponto/portas/celula.py:460–474` nas duas árvores · aval, item 2.

### 1. Erros e saídas do comando

- **O item nominal está fechado.** Na volta 3, o `except MarcaInvalida` imprimia e retornava. Na volta 4, o mesmo `except` levanta `CommandError` com a causa original, tanto no `--apply` quanto no dry-run na sombra. `PreviaForaDaSombra` também vira `CommandError`. Exercitei os dois ramos em memória e confirmei que um `RuntimeError` saído de qualquer uma das portas sobe intacto. `reaplicar_marca` delega diretamente e a prévia só captura sua sentinela de rollback. `b95a383d6` e `d72b72a76`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:118–130` · `app/colaboradores/services/feriado_do_posto.py:357–359,397–406`.
- **Ainda há três retornos normais com mensagem de erro antes da porta.** `--fecha` e `--opera` iguais, e PK inexistente, são **PREEXISTENTES**. `--apply` sem motivo, inclusive motivo só com espaços, é **NOVO** na obra. Essa guarda impede que a porta levante a `MarcaInvalida` prevista para o mesmo caso. A comparação em memória confirmou os três retornos normais. Cópia, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:70–75,111–124` e `app/colaboradores/services/feriado_do_posto.py:305–306`. Main, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:57–60,76–80,124–137`.
- **Há erros consumidos dentro das dependências.** Uma falha ao criar `LogAuditoria` é registrada e consumida por `evento`, com savepoint próprio. Uma falha no julgamento ou recálculo da célula também pode ser registrada e consumida. Nesses casos, a porta pode devolver resultado e o comando terminar normalmente. A obra também passou a chamar o helper best-effort `registrar_log` para a nova trilha da chave do vínculo. `app/colaboradores/services/feriado_do_posto.py:318–323,344–354` · `app/core/observ.py:85–98,124–139` · `app/ponto/portas/celula.py:460–500` · `app/colaboradores/services/vinculo.py:896–904` · `app/core/auditoria.py:17–30`.

### 2. Quem levanta, captura e lê o retorno

- **Seis pontos levantam `MarcaInvalida`.** `plano_da_marca` levanta por valor diferente de `True`/`False`, `hoje=None`, FECHA sem município e retomada sem um posto já FECHA. `aplicar_marca` levanta por motivo vazio e reaplicação de posto sem marca. A única captura produtiva localizada dessa classe é a do comando. Os testes da porta capturam a superclasse `ValueError`. O teste novo captura o `CommandError` convertido. `app/colaboradores/services/feriado_do_posto.py:257–275,305–315` · `app/colaboradores/management/commands/marcar_feriado_do_posto.py:124–130` · `app/colaboradores/tests/test_feriado_por_posto.py:171–176,249–274,356–362` · `app/colaboradores/tests/test_feriado_volta4.py:17–25`.
- **Zero leitores de `rc` específicos localizados na cópia.** O comando está em `FORA_DE_PIPELINE`. `bin/cron_run.sh` lê `$?` genericamente. Os testes localizados chamam `call_command` e leem exceção, saída ou estado. Nenhum exige retorno normal para `MarcaInvalida`. O teste de `--apply` sem motivo depende de retorno normal para chegar à asserção de que a marca segue vazia. O `manage.py` delega ao Django, e a expectativa registrada é `rc 1` para `CommandError`. O `rc` numérico em subprocesso real, o crontab instalado e consumidores externos ficam [nao verificado]. `app/config/crons.py:769,1350–1355` · `bin/cron_run.sh:4–13` · `app/manage.py:7–18` · `app/colaboradores/tests/test_feriado_por_posto.py:629–662` · `app/colaboradores/tests/test_feriado_previa_da_marca.py:274–288` · `app/chamados/tests/test_jv1_volta5_posto_do_dia_nos_vetos.py:83–101`.

### 3. O teste novo morde [nao verificado]

**VERDE na cópia e VERMELHO com o mutante em memória.** Executei o corpo do teste com o comando e a validação reais, usando substitutos para Django, ORM e relógio. Restaurar `w(ERROR); return` reproduziu exatamente o fonte da volta 3 e falhou com `AssertionError: CommandError not raised`. O teste fixa o relógio com `freeze_time`, usa a fixture `_posto` existente e chama `call_command` e `refresh_from_db`, APIs já usadas nos testes vizinhos. A execução do `TestCase` com Django e banco reais nesta árvore fica [nao verificado]. `app/colaboradores/tests/test_feriado_volta4.py:8–25` · `app/colaboradores/tests/test_feriado_por_posto.py:51–57,82–86,627–649` · `app/requirements.txt:13,43` · `b95a383d6`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:129–131`.

### 4. Checklist do delta `b95a383d6..d72b72a76`

- **`except` adicionados: 0. Fallbacks adicionados: 0.** O corpo de um handler existente trocou o retorno normal por `raise ... from e`. O teste acrescentado exige a exceção. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:127–130` nas duas voltas · `app/colaboradores/tests/test_feriado_volta4.py:15–25`.
- **Segundo juiz e derivação paralela.** Zero introduzidos pelo delta. Na obra, a mesma divergência cobre as duas classes: a prévia usa entrada e saída para incluir em `(a) PASSAM A PAGAR`, enquanto o juiz registrado também exige dia encerrado. A seleção dos vínculos da marca usa o mesmo universo na porta e no A4. `app/colaboradores/services/feriado_do_posto.py:203–234,283–285,449–474` · `app/colaboradores/management/commands/marcar_feriado_do_posto.py:78–85` · `app/core/juizes.py:243–248`.
- **Segundo escritor e duplo significado.** O censo AST localizou um escritor da marca e um chamador. `opera_em_feriado` é a declaração do posto. `trabalha_em_feriado` é a chave efetiva do vínculo, lida com as condições de ciclo e feriado. `alvo` conta candidatos e `retratados` conta o efeito aceito pelo reconciliador. `app/colaboradores/services/estrutura.py:90–95` · `app/colaboradores/tests/test_selo_marca_do_posto_um_escritor.py:226–238` · `app/escala/models.py:1384–1406` · `app/colaboradores/services/feriado_do_posto.py:277–285,318–341`.
- **Relógio e selo.** Zero leituras acrescentadas pelo delta. O selo novo morde a regressão pedida. O selo anterior de meia-noite observa os argumentos entregues à porta do vínculo, e a releitura na célula fica abaixo dessa observação. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:41–50,75,117–130` · `app/colaboradores/tests/test_feriado_volta4.py:15–25` · `app/colaboradores/tests/test_feriado_a5_volta3.py:243–266` · `app/ponto/portas/celula.py:460–474`.

### 5. Achados da volta 3 contra `main ab351aa9b`

| Achado aberto | Classificação e evidência nas duas árvores |
|---|---|
| Releitura em `_o_chao_do_dia_mudou` e no cartório | **PREEXISTENTE.** Rodei o mesmo caso em memória com os fontes das duas árvores. Sem virada, a célula de 16/12 foi tratada como futura e ficou fora do julgamento. Com virada de 23h59 para 00h01, a regeneração ainda usou 15/12 e o cartório recebeu 16/12 nas duas. A porta do vínculo no main faz uma leitura inicial própria, que a cópia substitui pela injeção. **Sai do bloqueio pelo aval, item 2.** Main e cópia, `app/ponto/services/cadastro_futuro.py:43–59`, `app/ponto/portas/celula.py:306–315,390–394,460–474` e `app/ponto/services/cartorio.py:2059–2089`. Porta do vínculo, main `app/colaboradores/services/vinculo.py:895–913`, cópia `:905–924`. |
| Outros relógios | **PREEXISTENTE:** presença sem limites e sincronizador sem `hoje`. Main `app/colaboradores/services/feriado_do_posto.py:74–88`, cópia `:74–89`. Main `app/colaboradores/management/commands/sincronizar_feriado_admin.py:53–57` → `app/colaboradores/services/vinculo.py:895–897`, cópia `:68–72` → `:905–908`. **NOVO:** `previa_da_marca` lê `now()` sem `agora`; a casca passa o valor e os testes diretos exercitam a omissão. Main `app/colaboradores/management/commands/marcar_feriado_do_posto.py:124–126`; cópia `app/colaboradores/services/feriado_do_posto.py:370–385`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:124–126` e `app/colaboradores/tests/test_feriado_previa_da_marca.py:99`. O A4 lê na própria borda, em `app/colaboradores/management/commands/vinculos_divergentes_da_marca_do_posto.py:14–18`, comando ausente no main. |
| Segundo juiz da prévia | **NOVO.** Com saída às 16h55 e turno previsto até 17h, o cenário em memória deu um item em `(a)` às 16h56 na cópia. O juiz registrado devolveu `False` às 16h56 nas duas árvores. O main tinha apenas a impressão do plano no dry-run. Main e cópia `app/core/juizes.py:243–248` e `app/ponto/turnos.py:2113–2130,2185–2214`. Main `app/colaboradores/management/commands/marcar_feriado_do_posto.py:117–126`; cópia `app/colaboradores/services/feriado_do_posto.py:449–474`. |
| Janela do sincronizador | **PREEXISTENTE.** Em memória, com chave inicialmente `False`, o sincronizador enfileirou `True`, a marca passou a FECHA antes da escrita e o lote gravou `True` nas duas árvores. O filtro novo protege quando a marca já está declarada na seleção. Main `app/colaboradores/management/commands/sincronizar_feriado_admin.py:42–57` e `app/colaboradores/management/commands/marcar_feriado_do_posto.py:128–137`; cópia `app/colaboradores/management/commands/sincronizar_feriado_admin.py:43–71` e `app/colaboradores/services/feriado_do_posto.py:318–323,344–352`. |
| Auditoria best-effort | **PREEXISTENTE** para o evento da marca e para julgamento/recálculo da célula. **NOVO alcance** na trilha da chave, que passa a usar o helper existente. Main `app/colaboradores/management/commands/marcar_feriado_do_posto.py:130–137` e `app/colaboradores/services/vinculo.py:890–897`; cópia `app/colaboradores/services/feriado_do_posto.py:318–323` e `app/colaboradores/services/vinculo.py:896–904`; nas duas `app/core/observ.py:85–98,124–139`, `app/core/auditoria.py:17–30` e `app/ponto/portas/celula.py:460–500`. |

**F1–F5.** Reexecutei as varreduras estáticas de F1 e F2, com zero ocorrências abertas. A forma antiga `type(posto)` voltou a acusar `?type` em memória. Na retratação a trava de `Posto` vem antes do reconciliador, que usa a ordem canônica das outras quatro entidades. F3/F4 mantêm os testes adversariais `1 | 1` e `0 | 0` com o rótulo novo. Em F5, `CLASSIFICAR` bate e os contadores seguem em 9 e 85. O censo B2 exige operando explícito e deixa de fora as releituras acima. Contenção real, suíte inteira, `sombra_c2b`, opus final e produção ficam [nao verificado]. `app/ponto/tests/test_contract_no_except_pass.py:50–78` · `app/chamados/tests/test_e1_v7_catracas.py:385–415` · `app/colaboradores/services/feriado_do_posto.py:307–309,325–335` · `app/chamados/models.py:754–770,2816–2825` · `app/chamados/tests/test_jv1_volta5_posto_do_dia_nos_vetos.py:83–101` · `app/core/tests/test_medida_turno_do_par.py:100,389–390,548–571` · `app/core/medida_turno_do_par.py:217–226` · `app/docs/BACKLOG.md:1966` · `CLAUDE.md:372–381`.

## 21/11 18:35 · sessao 31469f47b34d · - · - · origem=Ronald (tmux)

**BLOQUEIA.** A validação pública ainda carimba e planta diretamente, usando a irmã e os gates calculados antes do `atomic` e sem passar PKs de perguntas na chamada da trava. A varredura que usa o helper comum vem depois da escrita. `validacao.py:697–744,746–760,786–805,648–659`

- **P1/P3/P5/P7 e descartes.** O `r.txt` descreve os achados sem ligar os quatro IDs aos respectivos textos ou listar a triagem dos descartes. Vínculo nominal e descarte por ID [nao verificado]. A correção das candidatas cobre os PKs examinados, inclusive candidatas inelegíveis, e compara a eleição, o conjunto e a inclusão nas travas. As linhas de decisão ainda vêm por prefetch separado e as demais leituras decisórias continuam com cobertura parcial. `materializacao.py:139–157,2116–2158` · `models.py:2875–2904,4382–4390`
- **TOCTOU no passivo.** O `exportada` obtido antes da trava controla regen e pauta depois dela. O exportador registra a entrega fora dessa trava. Para perguntas avulsas, `data_conferida_em` fica na instância antiga e pode levar ao carimbo indevido de recusa após a confirmação concorrente do dia. A reversão do grupo recusado ainda conserva a foto anterior no resultado. `passivo_validado.py:351–401,462–474,549–557` · `folha/views.py:297–302` · `folha/models.py:174–188` · `validacao.py:1008–1016` · `materializacao.py:1922–1925,1947–1970` · `materializar_passivo_validado.py:117–130`
- **Capturas e fallback.** No delta fornecido `f3e48a445..6a5ab520c`, há dois `except` adicionados em teste e um removido, o `RuntimeError` amplo. `BrokenBarrierError` sobe como `AssertionError` encadeado e `AtoDoPlantioMudou` vira resultado do worker. O delta não adiciona captura de produção nem caminho alternativo de plantio em caso de erro. `test_e4c_f5_plantio_concorrente.py:79–103` · `materializacao.py:2150–2160` · `passivo_validado.py:389–401`. O total de `b0935c1f1..6a5ab520c` [nao verificado]. A cópia é compatível com o `git archive` do wrapper e a busca não encontrou a base, `.git` ou o diff integral. `bin/codex_pergunta.sh:311–319`
- **Chamadores.** O passivo transforma a exceção em recusa de grupo e o comando imprime `ERRO`, mantém o resultado no JSON e termina pelo fluxo normal. O laço da disputa agrega a mensagem em `erros`; a validação pública traduz esse retorno. As duas cascas de regularização podem retornar aviso ou sucesso antes de anexar o motivo, e o backfill descarta o retorno. `passivo_validado.py:397–401,437–440` · `materializar_passivo_validado.py:117–141,272–274` · `materializacao.py:2210–2236` · `validacao.py:803–848` · `acoes_disputa.py:143–149,164–179` · `fio.py:722–728,745–758` · `backfill_fio_morto_e4b.py:153–159`
- **Ordem.** O teste atual verifica as quatro tabelas em sequência, SQL ascendente para cada uma, PIDs distintos e ausência de duplicação das duas batidas/trilhas no cenário. A inversão `Batida → Colaborador` do flip contra `Colaborador → Batida` continua possível no código, com execução [nao verificado]. `test_e4c_f5_plantio_concorrente.py:58–132` · `flip_automatico.py:62–79` · `flip_batida.py:25–33` · `registro_batida.py:264–274` · `ponto/signals.py:206–218` · `reconciliador.py:635–646,1486–1487` · `materializacao.py:1586–1590,2145–2146`

## 21/11 18:34 · sessao fb9068025542 · - · - · origem=Ronald (tmux)

O RED reproduz a janela do passivo por leitura estática. A devolução usa a porta real em outra thread, grava em `atomic(durable=True)` e fecha a conexão antes de entregar seu PK. O passivo aguarda o `Future` antes de chamar a trava real. O cenário específico usa a conexão Django vinculada à thread e não mede PIDs. `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:101–117,298–327` · `app/chamados/services/validacao.py:269–296,309–321`

O mutante `if False` em `passivo_validado.py:392` deve ficar VERMELHO. A principal avulsa tem motivo `orfao_14h` e a devolução altera a validação e a sequência, que ficam fora da foto estrutural do helper. Sem a comparação da composta, a execução alcança a unidade e o balde exato exigido pelo teste não é produzido. O teste isolado também exige que `_trabalhar_o_par` não seja chamado. Execuções do RED, do mutante e o trio veredito, `Ran` e RC neste hash [nao verificado]. `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:44–58,113–119,165–202` · `app/chamados/services/materializacao.py:139–147,2116–2127,2152–2158` · `app/chamados/services/passivo_validado.py:389–401` · `CLAUDE.md:119–123`

O executor espera o worker e o worker fecha sua conexão em `finally`. O commit da devolução persiste durante o teste e é identificado pelo PK. A limpeza dessas linhas cabe ao `flush` de `TransactionTestCase`, com banco de ensaio configurado. A execução do teardown [nao verificado]. O relógio de negócio está congelado, inclusive na devolução, e `hoje` chega explicitamente. O worker tem `lock_timeout` de 4s e o `Future` aguarda 15s. Esse timeout não cancela o worker. `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:30–58,110–111,133–144,298–327` · `app/config/settings/ci.py:6–17`

O teste inverso assertou por código PIDs distintos, as quatro classes de SQL em ordem, `atomic`, `ASC`, uma tentativa vencedora, duas batidas e uma trilha por batida. O worker instala o contexto retornado, mas chama diretamente a unidade. `BrokenBarrierError` agora vira `AssertionError` com causa e escapa pelo `Future`. Um `order_by('-pk')` real nas quatro tabelas deve falhar por análise estática. Retirar simplesmente a ordenação explícita de Pergunta pode preservar o resultado porque `Meta.ordering = ['id']`. Execução e mutação [nao verificado]. O risco de inversão com flip permanece no código: flip adquire Batida e pode pedir Colaborador no reconciliador, enquanto plantio detém Colaborador e pode atualizar a mesma Batida reutilizada. Deadlock observado [nao verificado]. `app/chamados/tests/test_e4c_f5_plantio_concorrente.py:41–47,63–132` · `app/chamados/models.py:2875–2903,3813` · `app/ponto/management/commands/flip_automatico.py:62–79` · `app/ponto/services/flip_batida.py:25–33` · `app/ponto/registro_batida.py:264–274` · `app/ponto/signals.py:206–218` · `app/chamados/reconciliador.py:635–646,1486–1487` · `app/chamados/services/materializacao.py:1418,1586–1590,2145–2146`

## 21/11 18:34 · sessao 109c7d33a892 · E4C-F5-CONSTROI · construtor · origem=Code

Corrigi `_slot_esperado` para usar `texto_sem_carimbo(p)` na guarda do legado de hora. A catraca continua exigindo exatamente uma leitura declarada no slot, a do veto temporal. Reproduzi as cinco falhas da base. Após a cura, passaram os 47 testes do recorte local sem banco. [chamados/juizes.py:374](/home/ronald/wt-codex-e4c-f5-constroi-141636/app/chamados/juizes.py:374) · [chamados/juizes.py:411](/home/ronald/wt-codex-e4c-f5-constroi-141636/app/chamados/juizes.py:411) · [chamados/tests/test_e3_catraca_respondedores.py:544](/home/ronald/wt-codex-e4c-f5-constroi-141636/app/chamados/tests/test_e3_catraca_respondedores.py:544) [nao verificado]

### 1. O que o selo mede

`(2, 1)` significa **duas ocorrências medidas pelo censo AST e uma declarada**. A chave identifica a função. O censo registra cada ocorrência classificada como `DECLARADO`, agrupa por `arquivo::função` e compara o total exato com `LEITURAS_DECLARADAS`. `chamados/tests/test_e3_catraca_respondedores.py:81–85`, `:393–395`, `:422–430`, `:444–458`.

### 2. Por que a cura ficou no código

O delta foi **`1 → 2`**. O censo de `93ab82d08` encontrou o veto temporal em `_lido.respondida_em`. O de `aeb326255` encontrou também `p.respondida_em is None`, acrescentado na guarda do legado. A declaração existente cobre a pergunta temporal. A segunda leitura veio de outra pergunta: se há texto de hora com o instante físico ausente. `93ab82d08:app/chamados/juizes.py:374` · `chamados/juizes.py:374`, `:409–416` · `chamados/tests/test_e3_catraca_respondedores.py:67–69`.

O `r.txt` fornecido exige a ausência do instante físico para usar o texto legado na busca da batida. `texto_sem_carimbo(p)` aplica essa condição junto com a presença de texto, sobre a própria instância. O C21 segue julgando o par composto, e o juiz de fato exige a testemunha. Em memória, a batida determinou o resultado com o legado elegível. Preencher só a coluna física bloqueou o uso do texto mesmo com o instante composto vazio. O texto integral da FICHA e do aval está [nao verificado]. `chamados/juizes.py:371–382`, `:409–416`, `:860–863`, `:1587–1603` · `chamados/tests/test_fechar_disputa_materializada.py:38–52`.

### 3. As quatro falhas `MORDE`

| Teste | Causa | [nao verificado]
|---|---|
| `leitura_booleana_nova_DENTRO_de_um_declarado` | Injeta um teste de nulo no slot. O censo sobe de dois para três e devolve `(3, 1)`, enquanto a prova espera `(2, 1)`. `chamados/tests/test_e3_catraca_respondedores.py:588–595` |
| `volta4_a_corpo_diferido_nao_e_uso_de_VALOR` | O `isoformat()` na lambda diferida deixa o teste de nulo externo contar. A injeção também produz `(3, 1)`. `chamados/tests/test_e3_catraca_respondedores.py:792–798` |
| `volta4_b_disjuncao_no_ORM_nao_e_guarda` | O lookup de ordem no outro ramo do `\|` deixa o teste `isnull` contar. Dentro do slot, a injeção produz `(3, 1)`. `chamados/tests/test_e3_catraca_respondedores.py:339–357`, `:824–839` |
| `volta4_c_compensacao_entre_declarados` | Adiciona uma leitura no slot e retira uma no modelo. A troca conserva o total de partida, que já era seis, e falha na asserção de total cinco. `chamados/tests/test_e3_catraca_respondedores.py:866–880` |

### Diff

```diff
--- a/app/chamados/juizes.py
+++ b/app/chamados/juizes.py
@@ -411 +411 @@
-    carimbo_legado = (materializada is not None and not via and p.respondida_em is None
+    carimbo_legado = (materializada is not None and not via and texto_sem_carimbo(p)
--- a/app/chamados/tests/e3_c8b_apoio.py
+++ b/app/chamados/tests/e3_c8b_apoio.py
@@ -117 +117 @@
-                 'texto_vazio', 'pergunta_respondida', 'ausencia_parcial_cobra_o_resto',
+                 'texto_vazio', 'texto_sem_carimbo', 'pergunta_respondida', 'ausencia_parcial_cobra_o_resto',
```

### Porta do Code e prova RED

```bash
bash bin/suite.sh --dir /home/ronald/wt-codex-e4c-f5-constroi-141636 --only chamados --espera 7200 -- -k test_e3_catraca_respondedores
```

Da pasta `app`, este comando executa o selo e os `MORDE` com as injeções existentes. Na base, o módulo deu cinco falhas em 18 testes. Com a cura, os 18 passaram no recorte local. `chamados/tests/test_e3_catraca_respondedores.py:544–574`, `:588–595`, `:792–880`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest chamados.tests.test_e3_catraca_respondedores
```

O mutante abaixo restaura a leitura crua em memória. O próprio selo fica vermelho na igualdade de contagem, com `(2, 1)` e `rc 1`. `chamados/juizes.py:411` · `chamados/tests/test_e3_catraca_respondedores.py:556`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'
import unittest
from unittest.mock import patch
from chamados.tests import test_e3_catraca_respondedores as selo

arquivo = 'chamados/juizes.py'
fontes = selo.production_sources()
antes = 'and texto_sem_carimbo(p)'
assert fontes[arquivo].count(antes) == 1
fontes[arquivo] = fontes[arquivo].replace(antes, 'and p.respondida_em is None', 1)

teste = selo.ContadorRespondedoresTest('test_os_quatro_declarados_pela_pergunta') [nao verificado]
with patch.object(selo, 'production_sources', return_value=fontes):
    resultado = unittest.TextTestRunner().run(unittest.TestSuite([teste])) [nao verificado]
raise SystemExit(2 if resultado.errors else int(not resultado.wasSuccessful()))
PY
```

O recorte local mais amplo encontrou dois erros de importação de `django.utils` nos subcasos do teste de hash legado. Reproduzi ambos com a fonte da base. A execução ORM e a certificação pelo Code e pelo Opus final estão [nao verificado]. `chamados/tests/test_e3_pos_suite_sem_banco.py:175` · `core/fmt.py:8`.

**Autoconferência — except/fallback novos no diff: 0, esperado 0.** `chamados/juizes.py:411` · `chamados/tests/e3_c8b_apoio.py:117`.

## 21/11 18:28 · sessao 24f049b85777 · FERIADO-POR-POSTO · leitura · origem=Code

**Leitura do código: a releitura é preexistente na porta da célula, mas a marca do posto ganha um caminho até ela na cópia.** A linha 464 é idêntica nas duas árvores. No main, o comando grava a marca do posto e pode retratar chamados. Na cópia, ele também propaga a chave aos vínculos e, por essa propagação, pode julgar uma célula que ainda é futura para o `hoje` do ato. A igualdade no caminho comum, por si só, não demonstra igualdade no caminho da marca. `app/ponto/portas/celula.py:460–478` (ambas) · `app/colaboradores/management/commands/marcar_feriado_do_posto.py:124–149` (main) · `app/colaboradores/management/commands/marcar_feriado_do_posto.py:117–126` e `app/colaboradores/services/feriado_do_posto.py:343–354` (cópia)

Conferi as duas árvores locais e compilei em memória o texto do teste abaixo. O Python disponível em ambas falhou ao importar `freezegun`, declarado em `app/requirements.txt:43`. A execução Django, as linhas `PAREADA:` efetivamente emitidas e o efeito em produção ficam **[nao verificado]**.

## 1. Caminhos até a releitura

| Salto | Main `ab351aa9b` | Cópia `b95a383d6` |
|---|---|---|
| `Command.handle` → `_marcar` | `handle` chama `_marcar(o, w)`. `_marcar` lê `localdate()` para a janela de feriados usada na seleção dos chamados. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:33–37,57–67,93–116` | `handle` lê `agora`, deriva `hoje` e passa os dois a `_marcar`. Sugestões recebem `hoje` e leem a presença. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:41–50,75` · `app/colaboradores/services/feriado_do_posto.py:74–97,137–144` |
| `_marcar` → escrita ou prévia | O dry-run retorna. `--apply` chama `marcar_opera_em_feriado`, que grava só o campo do posto, e percorre o alvo da retratação. O corpo termina aí, sem chamada à porta do vínculo. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:124–149` · `app/colaboradores/services/estrutura.py:83–86` | `--apply` chama `aplicar_marca(hoje=...)`. O dry-run passa `hoje` e `agora` a `previa_da_marca`, que, somente na sombra, chama a mesma `aplicar_marca` com `hoje` dentro do savepoint. `agora` segue para a classificação da prévia. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:117–126` · `app/colaboradores/services/feriado_do_posto.py:370–385,397–406` |
| `aplicar_marca` → `marcar_trabalha_em_feriado` | O serviço da marca ainda não existe nessa árvore. A porta do vínculo já existe, com `hoje` lido dentro dela. O chamador de produção localizado é `sincronizar_feriado_admin --apply`. `app/colaboradores/services/feriado_do_posto.py:146–158` · `app/colaboradores/services/vinculo.py:869,890–914` · `app/colaboradores/management/commands/sincronizar_feriado_admin.py:53–57` | O plano seleciona vínculos vigentes em `hoje`. Sob a trava do posto, a propagação reconfirma a marca e o vínculo divergente e chama `marcar_trabalha_em_feriado(hoje=...)`. `reaplicar_marca` também delega à mesma porta. `app/colaboradores/services/feriado_do_posto.py:203–219,283–285,307–315,343–359` |
| Porta do vínculo → regeneração | Usa o `hoje` que acabou de ler nos dois ramos. Regenera até `hoje` diretamente e chama `regenerar_futuras_do_cadastro` para o restante. `app/colaboradores/services/vinculo.py:895–914` | Usa o `hoje` recebido da marca nos mesmos dois ramos. A leitura local fica para os chamadores que omitem o argumento, como o sincronizador. `app/colaboradores/services/vinculo.py:905–925` · `app/colaboradores/management/commands/sincronizar_feriado_admin.py:68–72` |
| Regeneração → linha 464 → cartório | Em ambas, o suplemento seleciona células **geradas já existentes** com `data > hoje` e chama `regenerar_celulas_vinculo` para cada dia, com esse mesmo `hoje`. A porta usa o dia recebido para desfazer o julgamento anterior da futura reescrita. Em seguida chama `_o_chao_do_dia_mudou` sem `hoje`. Havendo dias tocados e recálculo, a linha 464 relê o dia e a linha 472 despacha ao cartório. | Mesmos arquivos e linhas nas duas árvores. `app/ponto/services/cadastro_futuro.py:8–25,43–59` · `app/ponto/portas/celula.py:112–114,193–199,306–318,390–394,460–478` · `app/ponto/services/cartorio.py:2059–2089` |

A retratação de `--fecha` tem outro caminho nas duas árvores. `transicionar_com_pauta` chama `reconciliar`. Se o fechamento passar pelo gate, `_gravar_estado` salva o chamado e o `post_save` pode chamar `cartorio.julgar_celula` diretamente. Esse salto contorna a linha 464 e não carrega o dia da marca. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:139–147` (main) · `app/colaboradores/services/feriado_do_posto.py:325–341` (cópia) · `app/chamados/services/fecho_runtime.py:146–169`, `app/chamados/models.py:654–668,286–302` e `app/chamados/signals.py:189–218` (ambas)

## 2. `app/colaboradores/tests/test_pareada_hoje_injetado.py`

Um único método mede o comando real com a virada após `sugestoes` e, em outro vínculo, o caminho comum mais próximo com `hoje` explícito. No comando, a chamada direta a `handle(**opções)` usa a assinatura das duas árvores e fornece os campos que a cópia lê. No caminho comum, a única escrita direta da chave prepara a entrada da fixture. A seleção das futuras, a regeneração e o julgamento rodam pelo código real. A observação separa datas enviadas pela chamada forçada da linha 472 de datas para as quais o cartório devolveu resultado. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:33–37,60–62` (main), `:41–50,117–126` (cópia) · `app/colaboradores/tests/test_feriado_a5_volta3.py:243–266` (cópia) · `app/ponto/services/cadastro_futuro.py:43–59`, `app/ponto/portas/celula.py:460–478` e `app/ponto/services/cartorio.py:2073–2102` (ambas)

| Símbolo importado | Main | Cópia |
|---|---|---|
| `datetime`, `StringIO`, `TestCase`, `User` | `app/colaboradores/tests/test_feriado_admin_sync.py:1–5` | Mesmas linhas |
| `mock`, `freeze_time` | `app/colaboradores/tests/test_relogio_riscado_calendario.py:27–30` · `app/requirements.txt:43` | Mesmas linhas |
| `timezone` | `app/ponto/tests/test_porta_celula.py:9` | Mesma linha |
| `Command` | `app/colaboradores/management/commands/marcar_feriado_do_posto.py:19,33` | `app/colaboradores/management/commands/marcar_feriado_do_posto.py:23,41` |
| `Empresa`, `Praca`, `Posto`, `Colaborador`, `Municipio` | `app/colaboradores/models.py:40,177,219,283,962` | Mesmas linhas |
| `feriado_do_posto` | `app/colaboradores/tests/test_feriado_do_posto.py:108` · `app/colaboradores/services/feriado_do_posto.py:136` | `app/colaboradores/tests/test_feriado_do_posto.py:108` · `app/colaboradores/services/feriado_do_posto.py:137` |
| `Feriado` | `app/core/models.py:5` | Mesma linha |
| `CelulaDia`, `EscalaColaborador`, `TipoEscala` | `app/escala/models.py:1635,871,24` | `app/escala/models.py:1646,871,24` |
| `celula` | `app/ponto/tests/test_porta_celula_dia.py:147` · `app/ponto/portas/celula.py:112` | Mesmas linhas |
| `cartorio` | `app/chamados/tests/test_e2_celula_regenerada_reavalia.py:178–182` · `app/ponto/services/cartorio.py:2059` | Mesmas linhas |
| `regenerar_futuras_do_cadastro` | `app/ponto/services/cadastro_futuro.py:28` | Mesma linha |

```python
import datetime
from io import StringIO
from unittest import mock

from django.contrib.auth.models import User
from django.test import TestCase
from django.utils import timezone
from freezegun import freeze_time

from colaboradores.management.commands.marcar_feriado_do_posto import Command
from colaboradores.models import Colaborador, Empresa, Municipio, Posto, Praca
from colaboradores.services import feriado_do_posto
from core.models import Feriado
from escala.models import CelulaDia, EscalaColaborador, TipoEscala
from ponto.portas import celula
from ponto.services import cartorio
from ponto.services.cadastro_futuro import regenerar_futuras_do_cadastro


D = datetime.date(2026, 12, 16)
UM_DIA = datetime.timedelta(days=1)
HOJE = D - UM_DIA
DIAS = {HOJE, D, D + UM_DIA}


def datas(dias):
    return [dia.isoformat() for dia in sorted(dias)]


class PareadaHojeInjetadoTest(TestCase):
    def _cenario(self, empresa, municipio, praca, tipo, nome, cpf, matricula):
        posto = Posto.objects.create(
            nome=nome, empresa=empresa, municipio=municipio, praca=praca,
            opera_em_feriado=True,
        )
        colab = Colaborador.objects.create(
            nome=nome, cpf=cpf, matricula_hasner=matricula, empresa=empresa,
            posto=posto, situacao='ativo', data_admissao=HOJE, cargo='Vigilante',
        )
        ec = EscalaColaborador.objects.create(
            colaborador=colab, posto=posto, tipo_escala=tipo, ativa=True,
            data_inicio=HOJE, trabalha_em_feriado=True,
        )
        self.assertEqual(celula.regenerar_celulas_vinculo(
            ec, HOJE, max(DIAS), hoje=HOJE, estender_horizonte=False, recalculo=False,
        ), len(DIAS))
        self.assertEqual(
            dict(self._dias(ec).values_list('data', 'trabalha')),
            dict.fromkeys(DIAS, True),
        )
        return posto, ec

    def _dias(self, ec):
        return CelulaDia.objects.filter(
            colaborador_id=ec.colaborador_id, data__in=DIAS, origem='gerada',
        )

    def _observar(self, nome, ec, ato):
        enviados = set()
        julgados = set()
        julgar_real = cartorio.julgar_celula

        def julgar(colab, dia, **opcoes):
            alvo = colab.pk == ec.colaborador_id and dia in DIAS and opcoes.get('forcar')
            if alvo:
                enviados.add(dia)
            resultado = julgar_real(colab, dia, **opcoes)
            if alvo and resultado is not None:
                julgados.add(dia)
            return resultado

        with mock.patch.object(cartorio, 'julgar_celula', side_effect=julgar), \
                mock.patch.object(celula, 'regenerar_celulas_vinculo',
                                  wraps=celula.regenerar_celulas_vinculo) as regenerar:
            ato()

        alterados = set(self._dias(ec).filter(trabalha=False).values_list('data', flat=True))
        hoje_porta = sorted({c.kwargs['hoje'].isoformat() for c in regenerar.call_args_list})
        print(
            f'PAREADA: {nome} hoje_porta={hoje_porta} alterados={datas(alterados)} '
            f'enviados={datas(enviados)} julgados={datas(julgados)} '
            f'pulados={datas(alterados - enviados)} intactos={datas(DIAS - alterados)}',
            flush=True,
        )
        return alterados, enviados, julgados

    def test_hoje_da_borda_governa_a_regeneracao(self):
        with timezone.override('America/Sao_Paulo'), freeze_time(
            '2026-12-15 23:59:00-03:00',
        ) as relogio:
            User.objects.get_or_create(username='sistema')
            empresa = Empresa.objects.create(
                razao_social='Pareada', cnpj='19131243000359', ativa=True,
                dia_inicio_competencia=21,
            )
            municipio, _ = Municipio.objects.get_or_create(
                codigo_ibge=4113700, defaults={'nome': 'Londrina', 'uf': 'PR'},
            )
            praca = Praca.objects.create(nome='Pareada', cidade='Londrina', estado='PR')
            tipo = TipoEscala.objects.create(
                codigo='PAREADA-5X2', descricao='Pareada 5x2', tipo_ciclo='5x2',
                tipo_base='comercial', hora_inicio=datetime.time(8),
                hora_fim=datetime.time(17), folga_dia_semana='5,6',
            )
            for dia in sorted(DIAS):
                Feriado.objects.get_or_create(
                    data=dia, abrangencia='nacional', uf='', municipio='',
                    defaults={'nome': 'Feriado sintetico', 'fonte': 'teste'}, [nao verificado]
                )
            posto, ec_marca = self._cenario(
                empresa, municipio, praca, tipo, 'Marca', '[cpf]', 993001,
            )
            _, ec_comum = self._cenario(
                empresa, municipio, praca, tipo, 'Comum', '[cpf]', 993002,
            )
            self.assertEqual(timezone.localdate(), HOJE)

            sugerir_real = feriado_do_posto.sugestoes

            def sugerir_e_virar_dia(**opcoes):
                resultado = sugerir_real(**opcoes)
                relogio.tick(delta=datetime.timedelta(minutes=2))
                self.assertEqual(timezone.localdate(), D)
                return resultado

            def marcar():
                Command(stdout=StringIO()).handle(
                    sugestoes=True, posto=posto.pk, fecha=True, opera=False,
                    por='sistema', apply=True, motivo='Prova pareada de relogio',
                    retomar_retratacao=False,
                )

            with mock.patch.object(feriado_do_posto, 'sugestoes',
                                   side_effect=sugerir_e_virar_dia) as sugestoes:
                marca = self._observar('marca', ec_marca, marcar)

            sugestoes.assert_called_once()
            self.assertEqual(timezone.localdate(), D)
            posto.refresh_from_db()
            self.assertIs(posto.opera_em_feriado, False)

            def regenerar():
                EscalaColaborador.objects.filter(pk=ec_comum.pk).update(trabalha_em_feriado=False)
                ec_comum.refresh_from_db()
                celula.regenerar_celulas_vinculo(
                    ec_comum, HOJE, HOJE, hoje=HOJE, estender_horizonte=False,
                )
                regenerar_futuras_do_cadastro(ec_comum, hoje=HOJE)

            comum = self._observar('comum', ec_comum, regenerar)

        self.assertEqual(comum[0], DIAS, 'a prova precisa reescrever os tres dias')
        for nome, (alterados, enviados, julgados) in (('marca', marca), ('comum', comum)):
            with self.subTest(caminho=nome):
                self.assertEqual(julgados, enviados, 'o cartorio precisa concluir o julgamento enviado')
                self.assertEqual(
                    julgados, alterados & {HOJE},
                    'Clock Injection: D e D+1 ainda sao futuros para o ato',
                )
```

## 3. Como decidir pela saída

Com `hoje=15/12` e relógio congelado em `16/12` na regeneração, a regra permite julgar **15/12** e manda pular **16/12 e 17/12**, mesmo quando as três células são reescritas. O cenário usa `FECHA` para que as células reescritas virem folga e a guarda pelo fim do turno não mascare a releitura de `16/12`. `app/docs/PLANO-PADROES.md:97–103` (ambas) · `app/escala/models.py:1369–1395` (main), `:1380–1406` (cópia) · `app/ponto/services/cartorio.py:2073–2089` (ambas)

| Linha | Main, previsão pela leitura | Cópia, previsão pela leitura |
|---|---|---|
| `PAREADA: comum` | `hoje_porta=15/12`. Altera os três dias, julga `15/12` e `16/12`, pula `17/12`. | Mesma previsão. `app/ponto/services/cadastro_futuro.py:43–59` · `app/ponto/portas/celula.py:306–318,390–394,460–478` (ambas) |
| `PAREADA: marca` | `hoje_porta=[]`. Os três dias ficam intactos, sem despacho pela regeneração. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:124–149` (main) | `hoje_porta=15/12`. Altera os três dias, julga `15/12` e `16/12`, pula `17/12`. `app/colaboradores/services/feriado_do_posto.py:343–354` · `app/colaboradores/services/vinculo.py:905–925` (cópia) · `app/ponto/portas/celula.py:460–478` (ambas) |

- **MESMO na porta compartilhada:** as duas linhas `comum` coincidem, com regeneração efetiva dos três dias, `15/12` julgado como controle positivo e o mesmo destino para `16/12` e `17/12`. Se ambas julgarem `16/12`, a violação é preexistente nessa porta. `app/ponto/services/cadastro_futuro.py:43–59` · `app/ponto/portas/celula.py:460–478` (ambas)
- **NOVO/PIORA no caminho da marca:** `16/12` fica intacto no main e é julgado na cópia na linha `marca`, embora `hoje_porta` registre `15/12`. Esse par demonstra que o ato da marca passou a expor a releitura preexistente. Se a divergência aparecer também na linha `comum`, com `16/12` pulado no main e julgado na cópia, a piora aparece também no caminho compartilhado. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:124–149` (main) · `app/colaboradores/services/feriado_do_posto.py:343–354` (cópia) · `app/ponto/portas/celula.py:460–478` (ambas)

Para o item 2 do aval, o resultado previsto é **mesmo defeito na porta comum e exposição nova no ato da marca**. A prova pareada executada continua **[nao verificado]**.

## 21/11 18:25 · sessao e6269bff1d70 · E4C-F5-CONSTROI · construtor · origem=Code

A falha da volta 2 vem da simulação. O teste agora confirma a devolução em outra conexão antes de o passivo adquirir a trava. O diff da volta 3 está em dois arquivos de teste. [guarda_plantio_s144.py:298](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:298) · [cortes_f2_f9_apoio.py:37](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/cortes_f2_f9_apoio.py:37) [nao verificado]

### Respostas

1. **Por que o par volta a `(True, True)`.** Na base, o gancho chamava `devolver_validacao_recusada` na própria conexão do passivo, já dentro do `atomic()` da competência. A devolução zerava `validada_em` e `validada_por` e inseria `DEVOLUCAO` em savepoints aninhados. Depois da trava, a composta relida diferia da foto classificada. O `raise` saía do `atomic()` da competência e desfazia a limpeza e a inserção. O `except` externo contabilizava `escopo_do_plantio_mudou`. Assim, a leitura do ato e a coluna voltavam a mostrar a validação original. Em `f3e48a445`, o fluxo seguia da trava direto para `_trabalhar_o_par`. `1d0b5058b:app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:96` · `f3e48a445:app/chamados/services/passivo_validado.py:385` · [validacao.py:284](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/validacao.py:284) · [models.py:4437](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/models.py:4437) · [passivo_validado.py:383](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/passivo_validado.py:383) · [passivo_validado.py:397](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/passivo_validado.py:397) · [models.py:4399](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/models.py:4399)

2. **Onde está o defeito.** O rollback da competência alcançava a devolução injetada porque ambas as escritas pertenciam àquela transação. Com o outro ato já confirmado, a releitura ocorre após a trava e a divergência encerra a tentativa antes de `_trabalhar_o_par`. A captura altera apenas o resultado em memória. O commit da outra conexão fica preservado. [passivo_validado.py:361](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/passivo_validado.py:361) · [passivo_validado.py:389](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/passivo_validado.py:389) [nao verificado]

3. **O que fica gravado.** Pela regra, `validada_em` e `validada_por_id` ficam nulos, `recusa_motivo` continua `'Recusa anterior do plantio'` e há três decisões. A fixture grava `RESPOSTA` e `VALIDACAO`. A devolução acrescenta `DEVOLUCAO` e copia o motivo preexistente para `ato_motivo`. Nesse cenário, a fixture é quem preenche `recusa_motivo`. `_carimbar_recusa` é o escritor desse campo no caminho de materialização, que a recusa de escopo antecede. [guarda_plantio_s144.py:52](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:52) · [models.py:4313](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/models.py:4313) · [validacao.py:287](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/validacao.py:287) · [materializacao.py:1733](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/materializacao.py:1733) · [passivo_validado.py:392](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/passivo_validado.py:392)

### Diff e RED

- [cortes_f2_f9_apoio.py:37](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/cortes_f2_f9_apoio.py:37) extrai a criação da empresa e do colaborador para reuso entre `TestCase` e `TransactionTestCase`.
- [guarda_plantio_s144.py:101](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:101) intercala a devolução antes da trava real. O RED usa um worker com relógio congelado e `atomic(durable=True)`. O passivo espera o worker retornar após o commit. [guarda_plantio_s144.py:305](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:305) [nao verificado]
- A asserção de uma devolução bem-sucedida agora confere `True` dentro do worker e o PK exato da decisão depois do rollback do passivo. O RED exige o balde `escopo_do_plantio_mudou`, zero plantio, zero tentativa de carimbo e as três decisões. Em seguida, a mesa valida e planta pelo mesmo PK. [guarda_plantio_s144.py:113](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:113) · [guarda_plantio_s144.py:138](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:138) · [guarda_plantio_s144.py:143](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:143) · [guarda_plantio_s144.py:311](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:311)

### Porta para o Code

O recorte foi reconstruído a partir do anexo e do delta da volta 2. A seleção estática encontrou três testes do passivo e oito do plantio concorrente. Execução pela porta e veredito [nao verificado]. [guarda_plantio_s144.py:159](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:159) · [guarda_plantio_s144.py:317](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:317) · [plantio_concorrente.py:98](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_plantio_concorrente.py:98) · [plantio_concorrente.py:189](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_plantio_concorrente.py:189) [nao verificado]

```bash
wt=/home/ronald/wt-codex-e4c-f5-constroi-160129
bash bin/suite.sh --dir "$wt" --only chamados -- \
  -k '*test_e4c_f5_guarda_plantio_s144.*.test_RED_passivo*' \
  -k '*test_e4c_f5_plantio_concorrente.*.test_RED_*' \
  -k '*test_e4c_f5_plantio_concorrente.*.test_MORDE_*' \
  -k test_duas_materializacoes_inversas_travam_o_mesmo_par_em_ordem
```

### Mutantes para o Code

- **M1, devolução na conexão do passivo.** Reintroduz a simulação da volta 2. A mordida esperada é `(True, True)` na leitura após o rollback. [guarda_plantio_s144.py:126](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:126)
- **M2, worker sem commit.** O worker executa a devolução e faz rollback. O RED deve acusar que o passivo deixou de recusar pelo escopo mudado. [guarda_plantio_s144.py:114](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:114) · [guarda_plantio_s144.py:313](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:313) [nao verificado]
- **M3, P3 sem reconfirmação.** Mantém a devolução confirmada e suprime a recusa da foto velha. O RED deve acusar o desvio no resultado do passivo. [passivo_validado.py:392](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/passivo_validado.py:392) · [guarda_plantio_s144.py:114](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:114)

Os três mutantes têm âncora única e compilam em memória. Mordidas pela porta [nao verificado]. O bloco abaixo restaura o arquivo após cada chamada.

```bash
red=test_RED_passivo_foto_velha_recusa_escopo_e_mesa_valida_mesmo_pk
teste=chamados/tests/test_e4c_f5_guarda_plantio_s144.py [nao verificado]

mutar() {
  PYTHONDONTWRITEBYTECODE=1 python3 - "$wt" "$red" "$@" <<'PY'
from pathlib import Path
import subprocess
import sys

wt, red, rel, antes, depois = sys.argv[1:]
arquivo = Path(wt) / 'app' / rel
original = arquivo.read_bytes()
assert original.count(antes.encode()) == 1, rel
mutado = original.replace(antes.encode(), depois.encode())
compile(mutado, str(arquivo), 'exec')
try:
    arquivo.write_bytes(mutado)
    resultado = subprocess.run(
        ['bash', 'bin/suite.sh', '--dir', wt, '--only', 'chamados', '--', '-k', red] [nao verificado]
    )
finally:
    arquivo.write_bytes(original)
raise SystemExit(resultado.returncode)
PY
}

# M1
mutar "$teste" \ [nao verificado]
  'return executor.submit(self._devolver_em_outra_transacao, p.pk).result(timeout=15)' \
  $'with freeze_time(DEVOLUCAO_EM):\n                    atual = PerguntaDisputa.objects.get(pk=p.pk)\n                    self.assertIs(validacao.devolver_validacao_recusada(atual, self.admin), True)\n                    return atual.decisoes.get(ato=D.DEVOLUCAO).pk'

# M2
mutar "$teste" 'return devolucao.pk' \ [nao verificado]
  $'transaction.set_rollback(True)\n                return devolucao.pk'

# M3
mutar chamados/services/passivo_validado.py \
  'if DecisaoPergunta.composta(atuais[p.pk]) != estados_classificados[p.pk]:' \
  'if False:'
```

**LEI-AKITA** `origem=validacao.py:289` · `testemunha=models.py:4393` · `quem-mais-le=passivo_validado.py:390` · `RED=test_e4c_f5_guarda_plantio_s144.py:317` · `juizes novos=0` · `padrao=Single Writer Principle`

**AUTOCONFERÊNCIA** · except/fallback novos no diff: **0**. `git diff | grep -n except` sem ocorrências. O `finally` novo fecha a conexão do worker e deixa o erro subir. [guarda_plantio_s144.py:305](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:305)

## 21/11 18:13 · sessao 82fd2448f20e · E4C-F5-CONSTROI · construtor · origem=Code

Apliquei S1, S2, S2b e as duas curas de S3 na cópia `93ab82d08`. As provas em memória passaram com a cura, e os oito mutantes falharam por asserção. O recorte sem banco de C8b e do veto temporal passou com 29 testes. Execução ORM e suíte certificadora [nao verificado]. `chamados/tests/test_e3_c8b_sem_banco.py:369` · `chamados/tests/test_e3_pos_suite_sem_banco.py:103`

## 1. Alterações e mordidas

| Item | Alteração e mutante que o teste mata | [nao verificado]
|---|---|
| **S1** | Removi a captura da consulta de conflito. O teste exige `DatabaseError`, a mesma instância injetada e uma chamada a `.exists()`. Restaurar a conversão para `LOTE_B` falha no `assertRaises`. `chamados/services/validacao.py:1230` · `chamados/tests/test_fila_validar_lote.py:110` |
| **S2a** | O explicador recebe uma foto com `REABERTURA` na sequência e texto ainda na coluna. O teste exige `sem resposta vigente` e verifica a ausência da data e hora antigas. Acrescentar `p.resposta_colab` à mensagem falha no `assertNotIn`. `chamados/services/explicador.py:61` · `chamados/tests/test_explicador_disputa.py:80` |
| **S2, vínculo** | A pergunta tem `SEM_RESPOSTA_VIGENTE` e uma batida viva vinculada às 08h, com destino ausente. Deixar esse motivo passar pela guarda reabre o atalho do vínculo e muda o veredito para `True`. `chamados/juizes.py:799` · `chamados/juizes.py:805` · `chamados/tests/test_marco_por_motivo.py:231` |
| **S2, destino** | A pergunta tem `SEM_RESPOSTA_VIGENTE` e destino resolvido, com vínculo ausente. Deixar esse motivo passar pela guarda reabre o atalho do destino e muda o veredito para `True`. `chamados/juizes.py:799` · `chamados/juizes.py:827` · `chamados/tests/test_marco_por_motivo.py:245` |
| **S2, retratação** | Sob `SEM_RESPOSTA_VIGENTE`, carimbo mais trilha absolvem na leitura avulsa e na carga. Os controles com apenas uma das duas testemunhas reprovam. Trocar o retorno antecipado por `False` derruba o controle positivo. `chamados/juizes.py:293` · `chamados/juizes.py:800` · `chamados/tests/test_marco_por_motivo.py:253` |
| **S2b** | A fixture afirma `PLANTIO_SEM_TS` antes de cada julgamento da lei. Trocar esse motivo por `DATA_HORA_INVALIDA` deixa a asserção vermelha mesmo quando os dois motivos seguem o mesmo caminho no juiz de fato. `chamados/juizes.py:419` · `chamados/tests/test_hx_fallback_sem_lei.py:62` |

## 2. S3 — origem, cura e mutantes

**(i) Passivo com foto velha.** A linha do delta é `chamados/services/vigia_carimbo.py:104`, commit `93ab82d08`. O `prefetch_related('decisoes')` passou a acompanhar a instância entregue ao passivo. A devolução gravada entre classificação e trava entra por outra instância. O refresh parcial do passivo atualiza o carimbo e a via, e a composta ainda encontra a sequência antiga na instância classificada. É uma causa de classe de **reuso da foto após a classificação**. `chamados/tests/test_e4c_f5_guarda_plantio_s144.py:95` · `chamados/services/passivo_validado.py:382` · `chamados/services/passivo_validado.py:538` · `chamados/models.py:4361` · `chamados/models.py:4446`

A cura mantém o carregamento em lote e descarta `decisoes` em cache antes de devolver a tripla. Remover esse descarte reproduziu `((True, True), True, 'validar', True, None)` em memória. Com a cura, a leitura reproduziu `((False, True), True, 'validar', False, 'par_ato_coluna')`. O teste existente também exige a recusa no placar e a validação posterior pela mesa no mesmo PK. `chamados/services/vigia_carimbo.py:104` · `chamados/services/vigia_carimbo.py:122` · `chamados/tests/test_e4c_f5_guarda_plantio_s144.py:111` · `chamados/tests/test_e4c_f5_guarda_plantio_s144.py:132`

**(ii) Fechamento com uma pergunta já materializada.** A linha do delta é `chamados/juizes.py:410` em `93ab82d08`. A foto de `hora` passou a entregar texto vazio ao derivador quando o C21 recusava o par. A fixture tem `07:00`, carimbo de materialização e uma batida `E` às 07h, com os instantes de resposta e validação vazios. O texto vazio leva a `PLANTIO_SEM_TS`, o juiz de fato perde a comparação com a batida, e a pergunta volta viva ao gate. É uma causa de classe de **leitura do legado de hora no fechamento**. Nos recortes em memória, as duas regressões passaram em `c8eb8eddf` e `41d21a249` e falharam em `93ab82d08`. `chamados/tests/test_fechar_disputa_materializada.py:38` · `chamados/services/materializacao.py:695` · `chamados/juizes.py:1537` · `chamados/services/acoes_disputa.py:134` · `chamados/models.py:2035`

A cura usa o texto legado para localizar a batida somente no ramo `hora`, com carimbo visível no horizonte, via vazia, instante físico de resposta ausente e validação ausente na coluna e na composta. O C21 continua exigindo o par, e a materialização ainda exige testemunha. O mutante que restaura `resposta if vigente else ''` deixou **uma pergunta viva** e o C7 devolveu `False` em memória. A asserção ORM correspondente exige `fechada_em` preenchido. `chamados/juizes.py:411` · `chamados/juizes.py:860` · `chamados/juizes.py:1603` · `chamados/tests/test_fechar_disputa_materializada.py:64`

## 3. Quem escreve e quem lê

| Campo ou valor | Escrita e leitura |
|---|---|
| `resposta_colab`, `respondida_em`, `validada_em` e sequência de atos | A porta de resposta escreve o par. A resposta automática também o escreve, e a recuperação pela trilha completa o instante. A validação grava `VALIDACAO`, a devolução grava `DEVOLUCAO`, e a reabertura registra o ato e limpa par e carimbo pela porta em lote. `DecisaoPergunta.registrar` persiste as linhas. `leitura` ordena e aplica os atos, `composta` combina instantes do ato com texto da coluna, e slot, C21, lote e explicador consomem essa leitura. `chamados/services/disputa_emissao.py:1499` · `chamados/services/disputa_emissao.py:1664` · `chamados/services/disputa_emissao.py:1709` · `chamados/services/disputa_emissao.py:1732` · `chamados/services/validacao.py:168` · `chamados/services/validacao.py:287` · `chamados/models.py:4361` · `chamados/models.py:4388` · `chamados/models.py:4438` · `chamados/juizes.py:371` · `chamados/services/validacao.py:1224` · `chamados/services/explicador.py:52` |
| `materializada_em`, `via_resolucao`, `agora` e `slots[pk]` | `_escrever_carimbo` atribui carimbo e via. A projeção lê ambos e aplica o horizonte. A tarefa fornece `agora`, a carga copia o horizonte e grava resultado ou exceção por PK, e o slot lê o cache. O C6 lê a materialização projetada antes de consultar o juiz de fato. `chamados/models.py:3324` · `chamados/juizes.py:1434` · `chamados/juizes.py:1525` · `chamados/services/tarefa_do_dia.py:525` · `chamados/services/carga_pergunta_viva.py:88` · `chamados/services/carga_pergunta_viva.py:119` · `chamados/juizes.py:383` |
| Identidade da pergunta e insumos do derivador | O nascimento fornece `tipo_resposta`, `motivo`, `disputa` e destino. `gravar_dia_do_fato` atribui `data_evento`, e a costura pode atualizar o destino. A transição do chamado atribui `status_local`. O slot lê a identidade para montar a foto, o derivador lê também o contexto da disputa e o marco, e o juiz de fato lê o estado do destino. `chamados/services/disputa_emissao.py:448` · `chamados/services/fio.py:853` · `chamados/services/fio.py:978` · `chamados/models.py:240` · `chamados/juizes.py:413` · `chamados/services/materializacao.py:556` · `chamados/services/materializacao.py:625` · `chamados/juizes.py:827` |
| Batida e trilha de retratação | `registrar_batida` grava instante e tipo, o materializador anota `pergunta_origem`, e `retratar_batida` grava `retratada_em`. Juiz e vigia leem o vínculo vivo, o juiz lê a janela e o lote consulta `batidas_apuraveis`. A chave de auditoria `retratar_pergunta_obsoleta` é gravada pela fixture e lida pelo juiz avulso e pela carga. Escritor de produção dessa chave literal [nao verificado]. `ponto/registro_batida.py:177` · `ponto/registro_batida.py:294` · `chamados/services/materializacao.py:1578` · `chamados/juizes.py:803` · `chamados/juizes.py:860` · `chamados/services/vigia_carimbo.py:117` · `chamados/services/validacao.py:1230` · `chamados/tests/test_marco_por_motivo.py:262` · `chamados/juizes.py:296` · `chamados/services/carga_pergunta_viva.py:110` |
| `slot.motivo`, `slot.ts`, `slot.tipo`, classe e detalhe | `_slot_esperado` produz o resultado. Leem o juiz de fato, a carga, o lote, o explicador e o vigia. `classe_do_lote` produz classe e detalhe. `classificar` produz `(pergunta, classe, ts)` e o consomem o passivo e os dois vigias. `chamados/juizes.py:360` · `chamados/juizes.py:798` · `chamados/services/carga_pergunta_viva.py:119` · `chamados/services/validacao.py:1226` · `chamados/services/explicador.py:52` · `chamados/services/vigia_carimbo.py:112` · `chamados/services/vigia_carimbo.py:123` · `chamados/services/vigia_carimbo.py:131` · `chamados/services/passivo_validado.py:330` · `chamados/services/vigia_fio.py:415` |
| Cache de `decisoes`, `recusa_do_carimbo` e `fechada_em` | O prefetch do vigia preenche o cache, `leitura` o consome e `classificar` o descarta antes do `yield`. `carimbar` lê o par e grava a recusa em memória, que o passivo traduz em balde. C6 e C7 alimentam o fechamento e `DisputaSupervisao.fechar` grava `fechada_em`. `chamados/services/vigia_carimbo.py:104` · `chamados/models.py:4361` · `chamados/services/vigia_carimbo.py:122` · `chamados/models.py:3388` · `chamados/services/passivo_validado.py:145` · `chamados/models.py:2035` · `chamados/models.py:2118` |

## 4. Censo S1

`git grep -n -E 'classe_do_lote\(' -- api chamados/services` encontrou três consumidores. Dependência específica de `LOTE_B` **na falha** encontrada no censo estático: zero. O executor recusa qualquer classe diferente de A, a fila agrupa a classe devolvida e a API serializa a classe. No `93ab82d08`, os três consumiam genericamente o B produzido pela captura, e `git grep -n 'CONSULTA_CONFLITO_INDISPONIVEL' 93ab82d08 -- '*.py'` encontrou apenas o produtor. `chamados/services/validacao.py:1085` · `chamados/services/validacao.py:1277` · `api/views_mensageria.py:546` · `93ab82d08:app/chamados/services/validacao.py:1234`

## Comandos para o Code

O executor abaixo carrega cada mutante em memória. Os comandos ORM abaixo seguem [nao verificado]. Os alvos são as asserções de S1, S2, S2b e S3 listadas acima.

```bash
prova() {
  PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --settings=config.settings_test "$@"
}

mutante() {
  PYTHONDONTWRITEBYTECODE=1 python3 - "$@" <<'PY'
from importlib.machinery import SourceFileLoader
from pathlib import Path
import runpy
import sys

arquivo, teste, antes, depois = sys.argv[1:] [nao verificado]
alvo = Path(arquivo).resolve()
get_code = SourceFileLoader.get_code

def com_mutante(loader, nome):
    if Path(loader.path).resolve() != alvo:
        return get_code(loader, nome)
    fonte = alvo.read_text()
    if fonte.count(antes) != 1:
        raise ValueError(f'{alvo}: trecho do mutante não é único')
    return compile(fonte.replace(antes, depois), loader.path, 'exec')

SourceFileLoader.get_code = com_mutante
sys.argv = ['manage.py', 'test', '--settings=config.settings_test', teste] [nao verificado]
runpy.run_path('manage.py', run_name='__main__')
PY
}

t=chamados.tests
j=chamados/juizes.py
v=chamados/services/validacao.py
e=chamados/services/explicador.py
c=chamados/services/vigia_carimbo.py

s1=$t.test_fila_validar_lote.FilaValidarLoteTest.test_RED_lote_propaga_falha_da_consulta_de_conflito
s2a=$t.test_explicador_disputa.ExplicadorDisputaTest.test_RED_explicador_nao_cita_texto_residual_como_resposta_vigente
marco=$t.test_marco_por_motivo.MarcoPorMotivoTest
vinculo=$marco.test_RED_sem_resposta_vigente_nao_absolve_so_pelo_vinculo
destino=$marco.test_RED_sem_resposta_vigente_nao_absolve_so_pelo_destino_encerrado
trilha=$marco.test_RED_sem_resposta_vigente_preserva_retratacao_com_carimbo_e_trilha
s2b=$t.test_hx_fallback_sem_lei.FallbackSemLeiTest
s3i=$t.test_e4c_f5_guarda_plantio_s144.GuardaPlantioS144Test.test_RED_passivo_foto_velha_recusa_par_e_mesa_valida_mesmo_pk
s3ii=$t.test_fechar_disputa_materializada.FecharDisputaPerguntaMaterializadaTest.test_pergunta_materializada_nao_bloqueia_fechamento

prova "$s1" "$s2a" "$vinculo" "$destino" "$trilha" "$s2b" "$s3i" "$s3ii"

mutante "$v" "$s1" \
  '    if batidas_apuraveis(perg.disputa.colaborador, slot.ts - perto, slot.ts + perto).exists():' \
  $'    from django.db import DatabaseError\n    try:\n        conflito = batidas_apuraveis(perg.disputa.colaborador, slot.ts - perto, slot.ts + perto).exists()\n    except DatabaseError:\n        return LOTE_B, dict(d, porque="CONSULTA_CONFLITO_INDISPONIVEL")\n    if conflito:'

mutante "$e" "$s2a" $' % motivo\n' $' % motivo + " " + p.resposta_colab\n'

guarda="if slot.motivo in ('VETO_TEMPORAL', 'SEM_RESPOSTA_VIGENTE'):"
mutante "$j" "$vinculo" "$guarda" \
  "if slot.motivo == 'VETO_TEMPORAL' or (slot.motivo == 'SEM_RESPOSTA_VIGENTE' and p.chamado_destino is not None):"
mutante "$j" "$destino" "$guarda" \
  "if slot.motivo == 'VETO_TEMPORAL' or (slot.motivo == 'SEM_RESPOSTA_VIGENTE' and p.chamado_destino is None):"
mutante "$j" "$trilha" \
  "            return _retratada_com_trilha(p) if carga is None else carga.retratada(p)" \
  '            return False'

mutante "$j" "$s2b" "return _ResultadoSlot('PLANTIO_SEM_TS')" "return _ResultadoSlot('DATA_HORA_INVALIDA')"
mutante "$c" "$s3i" $'        getattr(p, \'_prefetched_objects_cache\', {}).pop(\'decisoes\', None)\n' ''
mutante "$j" "$s3ii" 'resposta if vigente or carimbo_legado' 'resposta if vigente'
```

## 21/11 18:09 · sessao c8af26ea4af5 · MEDIDA-FOLHA-[nome] · sonda · origem=Code

## Item (d) — segunda medida

| Chave da saída | Valor devolvido | Referência no código |
|---|---:|---|
| `gravado.badge_chamados` | 1420 | `app/core/context_processors.py:84,110-111` |
| `gravado.lavrado_em` | 21/11 18:06:10 | `app/core/context_processors.py:110-111` |
| `agora` | 21/11 18:08:29 | Relógio informado na saída fornecida. Linha da sonda [nao verificado]. |
| `vivo.exige_o_admin` | 1420 | `app/chamados/services/verbos.py:22-27` |
| `vivo.arquivado.n` | 136 | `app/chamados/services/painel.py:229-231,284-292` |
| `vivo.cobrar.n` | 2553 | `app/chamados/services/painel.py:229-231,284-292` |
| `vivo.decidir.n` | 712 | `app/chamados/services/painel.py:229-231,284-292` |
| `vivo.historico.n` | 8 | `app/chamados/services/painel.py:229-231,284-292` |
| `vivo.validar.n` | 708 | `app/chamados/services/painel.py:229-231,284-292` |

**1. O badge gravado bate com o vivo.** Na saída fornecida, `1420 = 708 + 712`. O carimbo tinha **2 min 19 s** de idade e a diferença era **0**. A navbar lê `PlacarNavbar.badge_chamados`. A lavra e a medida viva aplicam `exige_o_admin` às contagens de `montar_contexto_painel` sem filtros. A unidade é o chamado vivo classificado como Validar ou Decidir. Cobrar, Arquivado e Histórico ficam fora da soma. A composição por verbo na hora da lavra e as variações durante o intervalo ficam [nao verificado]. `app/core/context_processors.py:68-84,110-111` · `app/core/management/commands/lavrar_badge_navbar.py:39-41,72-74,91-93` · `app/chamados/services/painel.py:153-155,229-231` · `app/chamados/services/verbos.py:22-27,44-56`.

**2. Laudo versus sonda.** Pelos valores fornecidos, Validar foi de **707 para 708** (+1), Decidir de **705 para 712** (+7) e o badge de **1413 para 1420** (+7). No laudo, as pílulas somavam **1412**, um abaixo do badge. Na sonda, a soma e o badge fecham em **1420**. O código permite que as pílulas usem um recorte filtrado e que o badge venha de uma lavra anterior. Os filtros efetivos, as fontes que abasteceram as pílulas, o `lavrado_em` do laudo e a causa daquele 1 ficam [nao verificado]. `app/chamados/views.py:727-739,813-819,840-861` · `app/chamados/services/painel.py:117-130,229-231` · `app/core/context_processors.py:68-84`.

**3. Sim, há caminhos com fontes diferentes mesmo sem filtro.**

- **No GET inicial**, o cabeçalho renderiza `badge_chamados` da lavra, quando disponível, e as pílulas renderizam `verbos_contagens` calculadas ao vivo pelo GET. O recorte sem filtro segue a mesma regra de classificação em leituras de instantes distintos. `app/core/context_processors.py:68-84,154` · `app/chamados/views.py:727-739,767` · `app/templates/chamados/painel_gestao.html:42,79,191`.
- **Na atualização por JSON**, sem filtro e com `payload_placar`, as pílulas passam a ler o payload lavrado. Sem payload, a rota calcula ao vivo. O JavaScript atualiza as pílulas e mantém no cabeçalho o valor do render inicial. Se houver uma lavra entre o GET e a atualização, cabeçalho e pílulas podem exibir lavras distintas. `app/chamados/views.py:840-862` · `app/templates/chamados/painel_gestao.html:191,302-325`.
- **Se o cálculo vivo da navbar falhar**, o último fallback conta `qs_na_fila()`, que usa status de cobrança e exclui silenciados no futuro. Esse ramo tem outro critério. Sua ocorrência no laudo fica [nao verificado]. `app/core/context_processors.py:87-93` · `app/chamados/services/painel.py:188-190` · `app/chamados/services/verbos.py:22-27`.

A confirmação independente do hash da cópia e do isolamento da transação em produção fica [nao verificado].

## 21/11 18:07 · sessao c38aa6f720a9 · MEDIDA-FOLHA-[nome] · sonda · origem=Code

**Na sonda, “Espelho pendente” soma 207 em outubro e 298 em novembro na prévia. Em novembro, a `emp2` tem a única defasagem observada entre o retrato gravado e a prévia. Na Central, a diferença de 1 é aritmética confirmada pelo aval, e a igualdade de universo e instante exigida para provar pílula dissonante está [nao verificado].** `C.<competência>.<empresa>.furo_espelho_colabs`, `E.2026-11.emp2.retrato_gravado_x_previa.mudaram`, `E.2026-11.emp2.retrato_em_memoria_x_previa.mudaram`, `aval.txt:1`, `app/chamados/services/painel.py:230`, `app/core/context_processors.py:68`

Nos quadros, a primeira coluna dá o prefixo `P = competência.empresa`. Cada legenda indica como compor a chave da saída para cada número. As referências ao aval e à linha do log apontam para os anexos fornecidos. O carimbo de lavratura das fotos lidas está [nao verificado].

## (a) Termômetro R1

O E6 compara o espelho com o oráculo por dia-colab e atribui um dono a cada divergência. O universo é o de empresas ativas com fechamentos na competência, sem filtro de aptidão para o TXT. A comparação exclui dia sem linha, isento ou com ausência, sem valor lavrado, zero dos dois lados ou diferença explicada pela pausa declarada. Diferenças de até 10 minutos contam como concordância. `app/ponto/management/commands/e6_oraculo.py:303`, `app/ponto/management/commands/e6_oraculo.py:316`, `app/ponto/management/commands/e6_oraculo.py:331`, `app/ponto/management/commands/e6_oraculo.py:354`, `app/ponto/management/commands/e6_oraculo.py:366`, `app/ponto/management/commands/e6_oraculo.py:378`

Chave `A.P.<coluna>`. Nas colunas dos donos, `dias/colabs` corresponde a `A.P.<dono>.dias` e `A.P.<dono>.colabs`. A união conta pessoas distintas e as duas últimas colunas mostram a sobreposição exata. `app/ponto/management/commands/e6_oraculo.py:378`, `app/ponto/management/commands/e6_oraculo.py:401`

| P | `universo_colabs` | `dias_divergentes` | `ESTRUTURA` dias/colabs | `CADASTRO` dias/colabs | `BATIDA` dias/colabs | `uniao_colabs` | `em_exatamente_2_donos` | `em_exatamente_3_donos` |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| 2026-10.emp1 | 4 | 4 | 0/0 | 0/0 | 4/1 | 1 | 0 | 0 |
| 2026-10.emp2 | 477 | 265 | 18/10 | 68/28 | 179/94 | 117 | 11 | 2 |
| 2026-10.emp3 | 119 | 40 | 6/5 | 7/3 | 27/16 | 21 | 1 | 1 |
| 2026-10.emp4 | 24 | 9 | 2/1 | 0/0 | 7/6 | 7 | 0 | 0 |
| 2026-11.emp1 | 3 | 4 | 1/1 | 0/0 | 3/1 | 1 | 1 | 0 |
| 2026-11.emp2 | 460 | 288 | 24/16 | 63/30 | 201/101 | 128 | 15 | 2 |
| 2026-11.emp3 | 115 | 92 | 11/7 | 16/7 | 65/22 | 28 | 8 | 0 |
| 2026-11.emp4 | 24 | 9 | 1/1 | 0/0 | 8/5 | 6 | 0 | 0 |

## (b) Prévia e retrato × donos do R1

A prévia usa `classificar_export` sobre os fechamentos da empresa e competência, sem filtro de status. O retrato lê as linhas já gravadas. A ponte conta colabs por competência. As colunas dos donos são interseções com o R1 e podem se sobrepor. `app/folha/views.py:350`, `app/folha/export.py:782`, `app/folha/retrato_folha.py:98`, `app/ponto/management/commands/e6_oraculo.py:305`, `app/ponto/management/commands/e6_oraculo.py:378`

Cada célula segue a ordem **`no_r1 / sem_divergencia_r1 / ESTRUTURA / CADASTRO / BATIDA / so_no_leitor`**. A chave é `B.P.<leitor>.<coluna>.<métrica>`. Nas linhas sem leitor indicado, o valor vale para as duas chaves, `previa` e `retrato`.

| P | Leitor | `universo` | `entram` | `fora_txt` | `furo_espelho` | `sem_codigo_dominio` | `ferias_com_batida` |
|---|---|---|---|---|---|---|---|
| 2026-10.emp1 | Ambos | 4/3/0/0/1/0 | 0/0/0/0/0/0 | 4/3/0/0/1/0 | 2/1/0/0/1/0 | 1/1/0/0/0/0 | 0/0/0/0/0/0 |
| 2026-10.emp2 | Ambos | 477/360/10/28/94/0 | 244/196/4/9/41/0 | 233/164/6/19/53/0 | 173/106/6/19/51/0 | 1/1/0/0/0/0 | 0/0/0/0/0/0 |
| 2026-10.emp3 | Ambos | 119/98/5/3/16/0 | 82/71/1/0/11/0 | 37/27/4/3/5/0 | 24/17/3/2/4/0 | 1/1/0/0/0/0 | 1/1/0/0/0/0 |
| 2026-10.emp4 | Ambos | 24/17/1/0/6/0 | 14/12/0/0/2/0 | 10/5/1/0/4/0 | 8/4/1/0/3/0 | 1/1/0/0/0/0 | 0/0/0/0/0/0 |
| 2026-11.emp1 | Ambos | 3/2/1/0/1/0 | 0/0/0/0/0/0 | 3/2/1/0/1/0 | 2/1/1/0/1/0 | 1/1/0/0/0/0 | 0/0/0/0/0/0 |
| 2026-11.emp2 | `previa` | 460/332/16/30/101/0 | 191/157/5/4/28/0 | 269/175/11/26/73/0 | 240/146/11/26/73/0 | 1/1/0/0/0/0 | 1/1/0/0/0/0 |
| 2026-11.emp2 | `retrato` | 460/332/16/30/101/0 | 192/158/5/4/28/0 | 268/174/11/26/73/0 | 239/145/11/26/73/0 | 1/1/0/0/0/0 | 1/1/0/0/0/0 |
| 2026-11.emp3 | Ambos | 115/87/7/7/22/0 | 51/47/0/1/3/0 | 64/40/7/6/19/0 | 51/28/7/6/18/0 | 0/0/0/0/0/0 | 1/1/0/0/0/0 |
| 2026-11.emp4 | Ambos | 24/18/1/0/5/0 | 16/14/1/0/1/0 | 8/4/0/0/4/0 | 5/1/0/0/4/0 | 1/1/0/0/0/0 | 0/0/0/0/0/0 |

A barra do retrato separa `bloqueado` de `modulo_proprio`, que recebe as rescisões na janela. A prévia inclui essas rescisões em `fora_txt`. Mesma ordem de métricas, com chave `B.P.retrato.<coluna>.<métrica>`. `app/folha/retrato_folha.py:27`, `app/folha/retrato_folha.py:89`, `app/folha/views.py:351`

| P | `bloqueado` | `modulo_proprio` |
|---|---|---|
| 2026-10.emp1 | 3/2/0/0/1/0 | 1/1/0/0/0/0 |
| 2026-10.emp2 | 174/107/6/19/51/0 | 59/57/0/0/2/0 |
| 2026-10.emp3 | 26/19/3/2/4/0 | 11/8/1/1/1/0 |
| 2026-10.emp4 | 9/5/1/0/3/0 | 1/0/0/0/1/0 |
| 2026-11.emp1 | 3/2/1/0/1/0 | 0/0/0/0/0/0 |
| 2026-11.emp2 | 241/147/11/26/73/0 | 27/27/0/0/0/0 |
| 2026-11.emp3 | 52/29/7/6/18/0 | 12/11/0/0/1/0 |
| 2026-11.emp4 | 6/2/0/0/4/0 | 2/2/0/0/0/0 |

- **Mesmo universo de pessoas, sim**, em cada linha do quadro. Em ambos os leitores, `B.P.<leitor>.r1_sem_linha = 0` e `B.P.<leitor>.universo.so_no_leitor = 0`. Para cada `P`, `B.P.empresa_no_seletor = 1` e `B.P.retrato_presente = 1`. `app/ponto/management/commands/e6_oraculo.py:305`, `app/folha/export.py:782`, `app/folha/retrato_folha.py:98`
- **Mesma competência, sim.** R1 e prévia foram lidos na mesma rodada. A leitura do retrato nessa rodada usa a foto gravada, cujo `lavrado_em` está [nao verificado]. A coincidência de datas individuais entre o dia divergente do R1 e o dia que reteve na folha também está [nao verificado]. Em outubro, a igualdade acima é dos agregados da ponte. `app/ponto/janelas.py:54`, `app/ponto/management/commands/e6_oraculo.py:284`, `app/folha/fora_da_folha.py:146`, `app/folha/models.py:356`, `app/folha/retrato_folha.py:1237`

## (c) Espelho pendente por motivo diário e rota

A sonda abre o detalhe diário dos retidos por `furo_espelho` no universo comum com o R1. O leitor respeita a janela do vínculo e as dispensas por isenção, desligamento e vereditos dispensados. A unidade do detalhe é dia-colab, com pessoas distintas por célula da tabela. Uma pessoa pode aparecer em vários motivos ou rotas. `app/folha/fora_da_folha.py:115`, `app/folha/fora_da_folha.py:146`, `app/folha/export.py:1205`, `app/folha/export.py:1238`, `app/folha/export.py:1264`, `app/folha/export.py:1293`, `app/folha/retrato_folha.py:1024`

| P | `furo_espelho_colabs` | `retidos_sem_dia_detalhado` |
|---|---:|---:|
| 2026-10.emp1 | 2 | 0 |
| 2026-10.emp2 | 173 | 0 |
| 2026-10.emp3 | 24 | 0 |
| 2026-10.emp4 | 8 | 0 |
| 2026-11.emp1 | 2 | 0 |
| 2026-11.emp2 | 240 | 0 |
| 2026-11.emp3 | 51 | 0 |
| 2026-11.emp4 | 5 | 0 |

O quadro acima usa `C.P.<coluna>`. No próximo, cada par é `dias/colabs`. A chave do Total é `C.P.<motivo>.<dias ou colabs>`. Nas outras colunas é `C.P.<motivo>.<rota>.<dias ou colabs>`. `—` indica chave ausente no bloco fornecido. As rotas são `chamados:modal_fio` para o fio, `colaboradores:calendario_ponto` para Resolver dia e `ponto:painel_ausencia` para Decidir ausência. A sonda identificou as rotas publicadas. A abertura e a resolução pela tela estão [nao verificado]. `app/folha/retrato_folha.py:989`, `app/folha/retrato_folha.py:1016`, `app/chamados/urls.py:39`, `app/colaboradores/urls.py:24`, `app/ponto/urls.py:70`

| P | Motivo | Total | Fio | Calendário | Ausência | **`sem_porta`** | **`sem_leitura`** |
|---|---|---:|---:|---:|---:|---:|---:|
| 2026-10.emp1 | `espelho_furo` | 1/1 | — | 1/1 | — | — | — |
| 2026-10.emp1 | `espelho_nunca_bateu` | 14/1 | 10/1 | — | — | **4/1** | — |
| 2026-10.emp1 | `sem_celula` | 24/2 | — | 15/2 | — | **8/2** | **1/1** |
| 2026-10.emp2 | `espelho_cobrado` | 212/68 | 206/65 | 1/1 | 5/4 | — | — |
| 2026-10.emp2 | `espelho_cobranca_indevida` | 5/1 | — | 2/1 | — | **3/1** | — |
| 2026-10.emp2 | `espelho_discordante` | 13/10 | 2/2 | 7/7 | — | — | **4/2** |
| 2026-10.emp2 | `espelho_fato_em_ausencia` | 17/15 | 3/3 | 11/10 | — | **1/1** | **2/2** |
| 2026-10.emp2 | `espelho_furo` | 197/84 | 179/78 | 14/8 | 4/2 | — | — |
| 2026-10.emp2 | `espelho_indefinida` | 173/40 | 153/32 | 5/1 | — | **13/8** | **2/1** |
| 2026-10.emp2 | `espelho_nunca_bateu` | 157/6 | 96/6 | — | 1/1 | **60/4** | — |
| 2026-10.emp3 | `espelho_cobrado` | 50/16 | 50/16 | — | — | — | — |
| 2026-10.emp3 | `espelho_discordante` | 3/1 | 2/1 | 1/1 | — | — | — |
| 2026-10.emp3 | `espelho_furo` | 58/13 | 58/13 | — | — | — | — |
| 2026-10.emp3 | `espelho_indefinida` | 16/7 | 12/6 | — | — | **1/1** | **3/1** |
| 2026-10.emp3 | `espelho_nunca_bateu` | 30/1 | 15/1 | — | — | **15/1** | — |
| 2026-10.emp4 | `espelho_cobrado` | 5/4 | 5/4 | — | — | — | — |
| 2026-10.emp4 | `espelho_fato_em_ausencia` | 1/1 | — | 1/1 | — | — | — |
| 2026-10.emp4 | `espelho_furo` | 6/5 | 6/5 | — | — | — | — |
| 2026-11.emp1 | `espelho_cobrado` | 1/1 | 1/1 | — | — | — | — |
| 2026-11.emp1 | `sem_celula` | 41/2 | — | 22/2 | — | **19/2** | — |
| 2026-11.emp2 | `espelho_cobrado` | 505/128 | 484/121 | 17/9 | 3/3 | — | **1/1** |
| 2026-11.emp2 | `espelho_cobranca_indevida` | 11/6 | 9/4 | — | — | **2/2** | — |
| 2026-11.emp2 | `espelho_discordante` | 12/9 | 5/5 | 7/4 | — | — | — |
| 2026-11.emp2 | `espelho_fato_em_ausencia` | 12/9 | 5/3 | 7/6 | — | — | — |
| 2026-11.emp2 | `espelho_furo` | 222/117 | 206/111 | 12/6 | 1/1 | **1/1** | **2/2** |
| 2026-11.emp2 | `espelho_indefinida` | 250/58 | 227/52 | 1/1 | — | **22/7** | — |
| 2026-11.emp2 | `espelho_nunca_bateu` | 119/9 | 51/7 | 68/9 | — | — | — |
| 2026-11.emp3 | `espelho_cobrado` | 103/30 | 103/30 | — | — | — | — |
| 2026-11.emp3 | `espelho_discordante` | 4/2 | 3/1 | 1/1 | — | — | — |
| 2026-11.emp3 | `espelho_fato_em_ausencia` | 1/1 | — | 1/1 | — | — | — |
| 2026-11.emp3 | `espelho_furo` | 80/30 | 79/29 | 1/1 | — | — | — |
| 2026-11.emp3 | `espelho_indefinida` | 34/11 | 34/11 | — | — | — | — |
| 2026-11.emp3 | `espelho_nunca_bateu` | 15/1 | 7/1 | 8/1 | — | — | — |
| 2026-11.emp4 | `espelho_cobrado` | 7/3 | 5/3 | 2/1 | — | — | — |
| 2026-11.emp4 | `espelho_furo` | 5/3 | 5/3 | — | — | — | — |

| Motivo | O que retém o dia |
|---|---|
| `sem_celula` | Dia passado com vínculo sem célula, ou célula com previsão definida e sem veredito. `app/folha/export.py:1264`, `app/folha/export.py:1293` |
| `espelho_furo` | Furo integral ou parcial com veredito ainda acusador. `app/escala/models.py:1612`, `app/escala/models.py:1767` |
| `espelho_cobrado` | A cobrança foi reconhecida e o dia segue acusado. `app/ponto/services/cartorio.py:1534`, `app/escala/models.py:1767` |
| `espelho_cobranca_indevida` | Cobrança em dia sem trabalho ou sem furo. `app/escala/models.py:1613` |
| `espelho_discordante` | Batida órfã fora da tolerância, realizado inflado ou realizado zero com turno. `app/escala/models.py:1616` |
| `espelho_fato_em_ausencia` | Trabalho em dia coberto por ausência. `app/escala/models.py:1617`, `app/escala/models.py:1754` |
| `espelho_indefinida` | Dia previsto indefinido, fase por declarar ou ausência parcial sem período. `app/escala/models.py:1615` |
| `espelho_nunca_bateu` | Falta de adesão ao ponto em dia de trabalho sem cobertura integral. `app/ponto/services/cartorio.py:1312`, `app/ponto/services/cartorio.py:1318` |

**Achado de desenho para o BO:** as células `sem_porta` registram ocorrências em que o motivo reteve o dia e o mapa deixou o admin sem rota publicada para a ocorrência. `sem_leitura` registra separadamente os dias sem rota em que o fio ficou `nao_sei`. `C.P.<motivo>.sem_porta.{dias,colabs}`, `C.P.<motivo>.sem_leitura.{dias,colabs}`, `app/folha/fora_da_folha.py:86`, `app/folha/retrato_folha.py:989`, `app/folha/retrato_folha.py:1024`

| Competência | Aval `aval.txt:1` | Prévia `Σ C.P.furo_espelho_colabs` | Retrato `Σ B.P.retrato.furo_espelho.no_r1` |
|---|---:|---:|---:|
| 2026-10 | 207 | 2 + 173 + 24 + 8 = **207** | 2 + 173 + 24 + 8 = **207** |
| 2026-11 | 298 | 2 + 240 + 51 + 5 = **298** | 2 + 239 + 51 + 5 = **297** |

As somas usam `emp1` a `emp4` na competência da linha. Em outubro, o 207 bate nos dois leitores. Em novembro, o 298 bate na prévia e o retrato fica um abaixo, na `emp2`. `C.2026-11.emp2.furo_espelho_colabs`, `B.2026-11.emp2.retrato.furo_espelho.no_r1`, `aval.txt:1`

## (d) Central

| Contador | Valor citado | Fonte, unidade e universo |
|---|---:|---|
| **Validar** | 707 · `aval.txt:1` | `verbos_contagens.validar.n`. Conta chamados vivos classificados nesse verbo, dentro do recorte efetivo da Central. `app/templates/chamados/painel_gestao.html:42`, `app/chamados/services/painel.py:198`, `app/chamados/services/painel.py:230`, `app/chamados/services/verbos.py:30` |
| **Decidir** | 705 · `aval.txt:1` | `verbos_contagens.decidir.n`. Conta chamados do mesmo recorte classificados no verbo Decidir. O juiz atribui um verbo por chamado e remapeia os silenciados. `app/templates/chamados/painel_gestao.html:79`, `app/chamados/services/painel.py:230`, `app/chamados/services/verbos.py:46` |
| **Pra decidir** | 1413 · `aval.txt:1` | `badge_chamados` de `PlacarNavbar`. O cron computa Validar + Decidir para o tenant sem filtros e grava a soma com o `payload_placar` da mesma rodada. `app/templates/chamados/painel_gestao.html:191`, `app/core/context_processors.py:68`, `app/core/management/commands/lavrar_badge_navbar.py:39`, `app/core/management/commands/lavrar_badge_navbar.py:72`, `app/core/management/commands/lavrar_badge_navbar.py:91` |

- **Mesmo universo e mesmo instante.** Para o mesmo payload gravado, `badge_chamados = Validar + Decidir` por construção. O aval traz `707 + 705 = 1412`, contra `1413` no cabeçalho. A igualdade de universo e instante para esses valores está [nao verificado]. A diferença observada, portanto, **não prova pílula dissonante** no sentido exigido pelo aval. `aval.txt:1`, `app/chamados/services/verbos.py:22`, `app/chamados/services/verbos.py:25`, `app/core/management/commands/lavrar_badge_navbar.py:72`, `app/core/management/commands/lavrar_badge_navbar.py:91`
- **Diferença de universo ou instante.** O código permite pílulas ao vivo com filtro e cabeçalho global lavrado. Mesmo sem filtro, a renderização inicial pode contar ao vivo enquanto o cabeçalho lê a lavra. O refresh sem filtros pode buscar as pílulas no payload lavrado. Se faltar lavra, o cabeçalho tenta o mesmo juiz ao vivo para o tenant inteiro e, em caso de exceção, usa `qs_na_fila().count()`, de outro recorte. Qual desses caminhos produziu a diferença de 1 está [nao verificado]. `app/chamados/views.py:727`, `app/chamados/views.py:840`, `app/chamados/views.py:849`, `app/templates/chamados/painel_gestao.html:302`, `app/core/context_processors.py:68`, `app/core/context_processors.py:87`, `app/chamados/services/painel.py:188`, `aval.txt:1`

## (e) Competência 11 — gravado × prévia × reconstrução em memória

O retrato gravado vem de `linhas_do_retrato`. A reconstrução usa `montar_linhas`, chamada pelo lavrador antes de persistir, e deriva `status` e `motivo` do mesmo classificador da prévia. A sonda compara presença e o par `status/motivo` por colab-competência, na mesma leitura e com relógio comum para os leitores atuais. Datas dos dias, ações e fios ficam fora dessa comparação. `app/folha/retrato_folha.py:98`, `app/folha/retrato_folha.py:1212`, `app/folha/retrato_folha.py:1222`, `app/folha/retrato_folha.py:1237`, `app/folha/fora_da_folha.py:146`, `app/folha/management/commands/lavrar_retrato_folha.py:65`, `app/folha/management/commands/lavrar_retrato_folha.py:77`

Cada célula segue a ordem **`so_esquerda / so_direita / mudaram`**, com chave `E.P.<coluna>.<métrica>`.

| P | `retrato_em_memoria_x_previa` | `retrato_gravado_x_previa` | `retrato_gravado_x_em_memoria` | Leitura |
|---|---:|---:|---:|---|
| 2026-11.emp1 | 0/0/0 | 0/0/0 | 0/0/0 | Coincidem no recorte medido |
| 2026-11.emp2 | 0/0/0 | 0/0/1 | 0/0/1 | **Defasagem** |
| 2026-11.emp3 | 0/0/0 | 0/0/0 | 0/0/0 | Coincidem no recorte medido |
| 2026-11.emp4 | 0/0/0 | 0/0/0 | 0/0/0 | Coincidem no recorte medido |

Na `emp2`, a mudança é de `entra` na foto para `fora` por `furo_espelho` nos dois leitores atuais. É defasagem do veredito gravado, com o mesmo conjunto de pessoas. A comparação em memória coincide com a prévia em todas as empresas nos campos medidos. A idade da defasagem e o evento que a produziu estão [nao verificado]. `E.2026-11.emp2.retrato_gravado_x_previa.entra.-__fora.furo_espelho`, `E.2026-11.emp2.retrato_gravado_x_em_memoria.entra.-__fora.furo_espelho`, `E.P.retrato_em_memoria_x_previa.{so_esquerda,so_direita,mudaram}`, `app/folha/models.py:356`

**Onde batem 302 e 303.** A tabela usa as chaves `B.P.<leitor>.<balde>.no_r1`. A última coluna aplica à prévia a regra de **Bloqueados** do retrato. Ela subtrai `retrato.modulo_proprio` de `previa.fora_txt` porque os pares de `status/motivo` preservam esse módulo na comparação E. A única mudança foi de `entra` para `fora.furo_espelho`. Pela igualdade memória × prévia nesses campos, a última coluna também é o valor derivado para a reconstrução em memória. `app/folha/retrato_folha.py:27`, `app/folha/retrato_folha.py:89`, `app/folha/views.py:351`, `E.P.retrato_em_memoria_x_previa.mudaram`, `E.2026-11.emp2.retrato_gravado_x_previa.entra.-__fora.furo_espelho`

| P | `retrato.bloqueado` | `retrato.modulo_proprio` | `retrato.fora_txt` | `previa.fora_txt` | Prévia/memória no recorte de Bloqueados |
|---|---:|---:|---:|---:|---:|
| 2026-11.emp1 | 3 | 0 | 3 | 3 | 3 − 0 = 3 |
| 2026-11.emp2 | 241 | 27 | 268 | 269 | 269 − 27 = 242 |
| 2026-11.emp3 | 52 | 12 | 64 | 64 | 64 − 12 = 52 |
| 2026-11.emp4 | 6 | 2 | 8 | 8 | 8 − 2 = 6 |
| Soma das chaves acima | **302** | **41** | **343** | **344** | **303** |

O **302** do aval bate com **Bloqueados** do retrato. O **303** bate com a prévia e a memória quando se aplica esse mesmo recorte. Se a referência for o contador literal **Retidos do TXT** da prévia, a sonda soma **344**. No recorte equivalente do retrato, `fora_txt` soma **343**. A tela, os filtros e o carimbo que deram origem aos números citados no aval estão [nao verificado]. `aval.txt:1`, `B.P.retrato.bloqueado.no_r1`, `B.P.retrato.modulo_proprio.no_r1`, `B.P.retrato.fora_txt.no_r1`, `B.P.previa.fora_txt.no_r1`, `app/folha/views.py:408`, `app/folha/retrato_folha.py:89`

## (f) Porta do export

**Linha fornecida, com o identificador da tentativa omitido.** `f_linhas_export.txt:1`

```text
2026-11-21 07:04:59,754 WARNING ponto.recusa_critica RECUSA-ACAO-CRITICA porta=porta_export […] objeto=emp3:2026-12:[…] tipo=erro motivo=antes=em_curso depois=concluido origem=cron […] os leitores discordam em 1 ponto(s)
```

- **Contagem e causa.** A linha fornecida corresponde a uma tentativa da `emp3`, competência **12/2026**, concluída pelo cron com resultado divergente e `n=1`. O ESTADO conta recusas dessa porta pela data local das linhas de `ponto.log` e dos rotados. O anexo sustenta uma ocorrência. O total nos arquivos completos e qual teste compôs o `1` estão [nao verificado]. `aval.txt:1`, `f_linhas_export.txt:1`, `bin/recusa_estado.py:60`, `app/core/recusa_critica.py:95`, `app/core/recusa_critica.py:108`, `app/core/recusa_critica.py:133`, `app/config/crons.py:1809`, `app/folha/porta_export.py:1235`, `app/folha/porta_export.py:1273`
- **Impacto.** A tentativa do cron mediu e gravou o carimbo. Com esse carimbo vigente, a porta retorna `DIVERGENTE` e o POST é recusado antes da montagem do TXT. Exportação anterior ou posterior dessa empresa e competência está [nao verificado]. `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:218`, `app/folha/porta_export.py:1273`, `app/folha/porta_export.py:1388`, `app/folha/views.py:130`, `app/folha/views.py:270`

## (g) «Gerar TXT para conferir»

**Achado de desenho.** O GE

[truncado em 20000 de 21098 caracteres]

## 21/11 18:03 · sessao e19f8d4e02f8 · FERIADO-POR-POSTO · verificador · origem=Code

1. **Bloqueia — F5 corrige o registro do censo. O caminho da marca ainda relê o relógio.** `Command.handle` lê `timezone.now()` uma vez e passa `hoje = timezone.localdate(agora)` às sugestões, à marca e à prévia. A prévia também recebe `agora`. Esse é o dia civil da execução. O dia de cada chamado vem de `dias_do_fato`, e o dia de cada turno vem de `t.data_turno` entregue pelo pareador. A entrada em `B2_menos_B1` descreve corretamente a conversão da borda. Rodei o censo estático em memória: `CLASSIFICAR` bate integralmente, `LinhaTransporte.custo_mensal` continua na lista e os contadores com catraca continuam em 9 e 85. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:41–50,75,117–126` · `app/colaboradores/services/feriado_do_posto.py:248–253,449–459` · `app/core/medida_turno_do_par.py:217–226,432–440` · `app/core/tests/test_medida_turno_do_par.py:389–390,548–571`

   **Contraexemplo de meia-noite.** A marca começa às 23h59 de D. Uma célula gerada de D+1 muda na propagação, que ainda usa D e trata a célula como futura. Se o relógio virar antes de `_o_chao_do_dia_mudou`, `_hoje_j = timezone.localdate()` já vale D+1 e a mesma célula fica elegível para julgamento naquele ato. `julgar_celula` consulta o dia novamente e, conforme o ramo, consulta também o instante. O RED existente confere os dois argumentos `hoje` entregues à porta do vínculo. A releitura ocorre abaixo desse ponto, em conflito com a injeção de relógio da L-128. `app/colaboradores/services/feriado_do_posto.py:344–352` · `app/colaboradores/services/vinculo.py:905–924` · `app/ponto/services/cadastro_futuro.py:43–59` · `app/ponto/portas/celula.py:306–314,390–394,460–478` · `app/ponto/services/cartorio.py:2059–2089` · `app/colaboradores/tests/test_feriado_a5_volta3.py:244–266` · `app/docs/PLANO-PADROES.md:97–103`

   **Outros pontos fora dessa classificação.** O vigia A4 lê `localdate()` na própria borda. `presenca_em_feriado` lê `localdate()` quando faltam `hoje` e algum limite. `previa_da_marca` lê `now()` se chamada sem `agora`. `sincronizar_feriado_admin --apply` chama a porta do vínculo sem `hoje`, permitindo uma leitura por vínculo alterado. O censo B2 exige um operando explícito em `localdate` e não mede chamadas isoladas a `now()`. `app/colaboradores/management/commands/vinculos_divergentes_da_marca_do_posto.py:14–18` · `app/colaboradores/services/feriado_do_posto.py:74–89,378–385` · `app/colaboradores/management/commands/sincronizar_feriado_admin.py:68–71` · `app/colaboradores/services/vinculo.py:896–908` · `app/core/medida_turno_do_par.py:217–226`

2. **Fila — F1 cura o `except: pass` na origem.** Na saída normal, a prévia monta o resultado, lança `_DesfazAPrevia` dentro do `atomic` e devolve o dicionário no `except`, depois de sair do bloco transacional. Fora da sombra, levanta `PreviaForaDaSombra` antes da porta. Se `_classificar_previa` levantar `ValueError`, por exemplo, o `atomic` é desfeito e o erro sobe. No corpo da prévia, todo retorno é um dicionário e a única exceção consumida é a sentinela. A varredura estática de F1 retornou zero infrações novas. A mutação `pos_rollback` ainda compila e ainda move a classificação para depois do rollback. `app/colaboradores/services/feriado_do_posto.py:366–406` · `app/ponto/tests/test_contract_no_except_pass.py:50–78` · `app/colaboradores/tests/test_feriado_a5_volta3.py:197–229,323–328,372–379`

   Há supressão de erros **nas dependências**. Por exemplo, uma falha ao gravar `LogAuditoria` é registrada e consumida por `evento`, que tem savepoint próprio. A marca pode continuar e a prévia pode devolver o plano. Os testes da prévia conferem a foto do banco e os callbacks `on_commit`, inclusive no ramo da retratação. `app/core/observ.py:85–98,124–139` · `app/colaboradores/services/feriado_do_posto.py:318–323,397–406` · `app/colaboradores/tests/test_feriado_previa_da_marca.py:157–205,246–270`

3. **Fila — F2 resolve a identificação do modelo pela catraca. A ordem de aquisição permanece.** O mesmo `.get(pk=posto.pk)` avalia a consulta dentro de cada `atomic`. Na retratação, a aquisição de `Posto` precede o reconciliador, que chama `trava_na_ordem` para `Colaborador → DisputaSupervisao → ChamadoColaborador → PerguntaDisputa`. `Posto` fica fora desse conjunto. Reapliquei em memória a expressão antiga e a catraca acusou `?type`. Com `Posto` nomeado, a varredura da cópia retornou zero ocorrências. `app/colaboradores/services/feriado_do_posto.py:307–315,325–351` · `app/chamados/models.py:663–667,684–686,754–770,2816–2825,2852–2880` · `app/chamados/tests/test_e1_v7_catracas.py:70–72,116–132,385–390`

   A busca na cópia localizou o `select_for_update` explícito de `Posto` em `aplicar_marca`. Os outros escritores localizados adquirem a linha pelo `UPDATE`: `atualizar_posto`, `definir_municipio_posto` e `gravar_posto_ativo`. Por exemplo, a troca de município grava o posto antes de disparar a reavaliação dos vínculos. A troca de `type(posto)` por `Posto` conserva essa relação. Contenção efetiva e travas indiretas no banco ficam [nao verificado]. `app/colaboradores/services/feriado_do_posto.py:307–309` · `app/colaboradores/services/estrutura.py:53–86,98–107`

4. **Fila — F3/F4 corrigem os consumidores do rótulo e preservam a prova do universo.** Os testes continuam exigindo `1 | 1` para vínculo do dia no posto que fecha, com cadastro em outro posto, e `0 | 0` para o inverso. Além deles, o teste da casca lê o literal novo. O comando imprime a mesma linha no dry-run e no apply. A ocorrência localizada do literal antigo está na base histórica congelada da catraca C12, lida pelo verificador como código-fonte. A busca na cópia não encontrou consumidor do literal em documentação, outro comando ou template. Consumidores externos ficam [nao verificado]. `app/chamados/tests/test_jv1_volta5_posto_do_dia_nos_vetos.py:83–101` · `app/colaboradores/tests/test_feriado_por_posto.py:629–639` · `app/colaboradores/management/commands/marcar_feriado_do_posto.py:132–152` · `app/chamados/tests/e4b_c12_base.json:13` · `app/chamados/tests/e4b_c12_verificar.py:13–18`

   O novo rótulo atende à LEI-AKITA 8: a saída imprime `len(plano['alvo'])`, produzido pela porta real. `alvo` contém os vivos do posto do dia do fato em feriado alcançado, quando o ato muda a marca para FECHA ou retoma explicitamente uma retratação. Um chamado vivo em feriado dá `1 | 0` numa operação OPERA. Numa operação FECHA ele pode entrar como candidato e a premissa ainda recusar a retratação. O teste R3 cobre essa recusa. `CLAUDE.md:35–36` · `app/colaboradores/services/feriado_do_posto.py:237–254,272–285,325–341,397–406` · `app/colaboradores/management/commands/marcar_feriado_do_posto.py:136–138` · `app/colaboradores/tests/test_feriado_por_posto.py:231–247`

5. **Fila — Escritores e leitores dos valores exercitados pelo delta.**

   | Campo ou valor | Quem escreve ou produz | Quem lê |
   |---|---|---|
   | `Posto.pk`, `nome`, `municipio_id`, `opera_em_feriado` | A criação e a edição do cadastro passam por `estrutura`. O backfill escreve `municipio`. `marcar_opera_em_feriado` grava a marca a pedido de `aplicar_marca`. `app/colaboradores/views.py:852–884` · `app/colaboradores/services/estrutura.py:43–45,67–79,90–107` · `app/colaboradores/services/feriado_do_posto.py:318–323` | A porta relê a linha pelo PK e usa município e marca no plano. A casca imprime PK, nome, município e marca. Sugestões, veto de cobrança, vigia A4, sincronizador e juiz do vínculo novo também leem a marca. `app/colaboradores/services/feriado_do_posto.py:47–71,124–139,188–200,222–234,269–285,307–315` · `app/colaboradores/management/commands/marcar_feriado_do_posto.py:111–117,132–147` · `app/ponto/management/commands/detectar_ausencias.py:624–630` · `app/colaboradores/management/commands/sincronizar_feriado_admin.py:50–61` · `app/escala/models.py:905–928` |
   | `EscalaColaborador.posto_id`, vigência, ciclo, cargo e `trabalha_em_feriado` | A porta de vínculo grava posto, vigência e template. No nascimento consulta o juiz da chave, e os copiadores chamam `chave_ao_nascer`. A troca da chave em vínculo existente passa por `marcar_trabalha_em_feriado`. `app/colaboradores/services/vinculo.py:259–299,870–904,1118–1127,1145–1153` | Plano e A4 usam `vinculos_que_seguem_a_marca`. O plano usa `q_vinculo_cobre_a_data` e o predicado de ciclo e cargo. A grade, a precedência e o fechamento leem a chave por `feriado_vira_folga`. `app/colaboradores/services/feriado_do_posto.py:175–185,203–234,283–285` · `app/escala/alimentacao.py:219–225` · `app/escala/models.py:1380–1406` · `app/ponto/precedencia.py:377` · `app/ponto/services/fechamento.py:949` · `app/docs/JUIZES.md:114–118,142–146` |
   | `ChamadoColaborador.status_local`, `colaborador_id`, `modulo_origem`, dia do fato e feriado | O emissor entrega identidade e `contexto_json.data_turno` a `ChamadoColaborador.abrir`. O reconciliador passa a mudança de status a `_gravar_estado`. O cadastro de feriados grava data, alcance e nome. `app/ponto/management/commands/detectar_ausencias.py:694–710` · `app/chamados/models.py:207–235,654–668` · `app/core/views_config.py:41–89,144–153` | O plano usa `qs_vivos`/`VIVOS`, `dias_do_fato` e `vinculos_dos_dias` para achar o posto da data. `core.feriados` decide o alcance e fornece o nome. A casca lê o módulo para listar e agrupar o alvo. `app/colaboradores/services/feriado_do_posto.py:47–57,237–254,277–285,462` · `app/chamados/services/painel.py:135–155` · `app/chamados/juizes.py:175–208` · `app/escala/alimentacao.py:334–358` · `app/core/feriados.py:70–112` · `app/colaboradores/management/commands/marcar_feriado_do_posto.py:139–153` · `app/docs/JUIZES.md:62–65,127–130,142–146` |
   | Foto da célula, `data_turno`, batidas e `a/b/outras` | A porta da célula grava `trabalha`, `dna` e `escala_geradora`; a lavra escreve `ata`, `veredito` e `veredito_em`; `carimbar` grava `julgada_em` e `impressao`. O pareador produz `data_turno`, entrada, saída e batidas. `_classificar_previa` produz as três listas. `app/ponto/portas/celula.py:289–314,1089–1097,1181–1202` · `app/ponto/turnos.py:312–324` · `app/colaboradores/services/feriado_do_posto.py:420–475` | A prévia captura as fotos por `(colaborador_id, data)`, usa os juízes de vínculo, janela e turno e devolve as listas. A casca só as formata e conta. `app/colaboradores/services/feriado_do_posto.py:390–417,425–475` · `app/colaboradores/management/commands/marcar_feriado_do_posto.py:77–100,148–150` · `app/docs/JUIZES.md:20–25,114–121` |
   | `plano`, contagens, trilha e nota | `plano_da_marca` produz `marca_antes`, `marca_muda`, `candidatos`, `sem_dia`, `vivos`, `alvo` e `propagar`. `aplicar_marca` conta `retratados` e `propagados`. `evento` escreve autor, motivo e antes/depois na trilha; `nota.anexar_ocorrencia` grava a nota no fio após a retratação aceita. `app/colaboradores/services/feriado_do_posto.py:261–285,318–354` · `app/core/observ.py:94–102,133–138` · `app/chamados/services/nota.py:75–109` | A prévia lê `propagar` e `alvo` para definir quem fotografar. A casca lê o plano e as contagens para imprimir. A auditoria lê a trilha e os templates do fio leem os follow-ups. `app/colaboradores/services/feriado_do_posto.py:390–406` · `app/colaboradores/management/commands/marcar_feriado_do_posto.py:132–154` · `app/core/views.py:705` · `app/templates/chamados/fio_colaborador.html:235` |

6. **Fila — Outras classes, fora das mudanças do patch.**

   - **Segundo juiz.** O classificador da prévia usa entrada e saída presentes como condição para `(a) PASSAM A PAGAR`. O juiz registrado de par completo exige também dia encerrado. Contraexemplo: o pareador já devolveu entrada e saída às 16h55, a prévia roda às 16h56 e o turno previsto termina às 17h. A condição local aceita o par e o juiz ainda recusa o encerramento. A divergência está no classificador já presente, fora da linha alterada por F1. `app/colaboradores/services/feriado_do_posto.py:449–474` · `app/colaboradores/management/commands/marcar_feriado_do_posto.py:78–85` · `app/core/juizes.py:243–248` · `app/ponto/turnos.py:2113–2130,2185–2214`
   - **Segundo escritor fora da porta.** O delta conserva o escritor único da marca. Na chave do vínculo há uma janela concorrente anterior ao patch: o sincronizador pode selecionar um vínculo quando o posto está sem marca, a porta pode então gravar FECHA e propagar `False`, e o sincronizador pode em seguida gravar o `True` já calculado. Os dois usam a porta da chave. O sincronizador toma a decisão sobre a marca antes do lote de escritas, fora da trava do posto. `app/colaboradores/services/estrutura.py:90–95` · `app/colaboradores/services/feriado_do_posto.py:344–352` · `app/colaboradores/management/commands/sincronizar_feriado_admin.py:43–71` · `app/colaboradores/services/vinculo.py:896–904`
   - **Selo, trava e significado dos campos.** F1 e F2 preservam as catracas existentes, F3/F4 preservam os dois casos adversariais e F5 atualiza a publicação informativa, com a lacuna de relógio do item 1. O rótulo novo distingue a candidatura do efeito, e a marca do posto continua separada da chave efetiva do vínculo, inclusive nos casos em que a propagação é excluída por ciclo ou cargo. `app/ponto/tests/test_contract_no_except_pass.py:64–78` · `app/chamados/tests/test_e1_v7_catracas.py:387–415` · `app/chamados/tests/test_jv1_volta5_posto_do_dia_nos_vetos.py:91–101` · `app/core/tests/test_medida_turno_do_par.py:12–16,565–571` · `app/colaboradores/services/feriado_do_posto.py:175–185,272–285,325–354`
   - **Pré-requisitos de pouso.** O registro exige `sombra_c2b = 0`, e o processo exige suíte inteira verde na árvore da obra antes do opus final. Na cópia pude conferir os trechos do patch e rodar as verificações estáticas descritas acima. Hash Git, suíte inteira pós-delta, `sombra_c2b`, opus, pouso e efeito em produção ficam [nao verificado]. `app/docs/BACKLOG.md:1965–1966` · `CLAUDE.md:372–381`

## 21/11 17:56 · sessao 16374add9c62 · FERIADO-POR-POSTO · construtor · origem=Code

F1–F5 estão na cópia em três arquivos. Os módulos estáticos de F1, F2 e F5 passaram isoladamente. Em F3 e F4, as duas asserções do rótulo passaram em memória. O módulo Django parou na importação de `django.core`, então os dois testes completos seguem [nao verificado]. [manage.py:11](/home/ronald/wt-codex-feriado-por-posto-120132/app/manage.py:11) [nao verificado]

| Item | Mudança | Mesmo teste do log | [nao verificado]
|---|---|---|
| F1 | `previa_da_marca` retorna dentro do `except _DesfazAPrevia`, após a sentinela sair do `atomic`. [feriado_do_posto.py:398](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/feriado_do_posto.py:398) | `test_sem_except_pass_novo` verde, **1/1** no módulo. O mutante que restaura `except: pass` falhou. [test_contract_no_except_pass.py:73](/home/ronald/wt-codex-feriado-por-posto-120132/app/ponto/tests/test_contract_no_except_pass.py:73) | [nao verificado]
| F2 | `aplicar_marca` nomeia `Posto` no `select_for_update`. [feriado_do_posto.py:307](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/feriado_do_posto.py:307) | `test_zero_trava_fora_de_trava_na_ordem` verde, **22/22** no módulo. O mutante com `type(posto)` voltou a acusar `?type`. [test_e1_v7_catracas.py:387](/home/ronald/wt-codex-feriado-por-posto-120132/app/chamados/tests/test_e1_v7_catracas.py:387) | [nao verificado]
| F3 | A asserção de vínculo em P espera o rótulo atual e conserva `1/1`. [test_jv1_volta5_posto_do_dia_nos_vetos.py:94](/home/ronald/wt-codex-feriado-por-posto-120132/app/chamados/tests/test_jv1_volta5_posto_do_dia_nos_vetos.py:94) | `test_RED_vinculo_em_P_cadastro_em_Maringa_entra_no_universo`. A asserção passou em memória e falhou com o literal antigo. Teste completo [nao verificado]. | [nao verificado]
| F4 | A asserção de cadastro em P espera o rótulo atual e conserva `0/0`. [test_jv1_volta5_posto_do_dia_nos_vetos.py:100](/home/ronald/wt-codex-feriado-por-posto-120132/app/chamados/tests/test_jv1_volta5_posto_do_dia_nos_vetos.py:100) | `test_RED_cadastro_em_P_vinculo_em_Maringa_fica_fora`. A asserção passou em memória e falhou com o literal antigo. Teste completo [nao verificado]. | [nao verificado]
| F5 | Registrei `Command.handle` em `B2_menos_B1`, imediatamente antes de `LinhaTransporte.custo_mensal`, que permaneceu na lista. [test_medida_turno_do_par.py:389](/home/ronald/wt-codex-feriado-por-posto-120132/app/core/tests/test_medida_turno_do_par.py:389) | `test_a_classificar_publicado_sem_entrar_como_criterio_amplo` verde, **35/35** no módulo. O mutante que omite só a entrada nova falhou na igualdade exata. [test_medida_turno_do_par.py:565](/home/ronald/wt-codex-feriado-por-posto-120132/app/core/tests/test_medida_turno_do_par.py:565) | [nao verificado]

### F2 — posição da trava

O modelo é `colaboradores.models.Posto`. Ele é travado primeiro em cada segmento de `aplicar_marca`, inclusive antes de chamar a retratação. A ordem de `PerguntaDisputa.trava_na_ordem` abrange **Colaborador → DisputaSupervisao → ChamadoColaborador → PerguntaDisputa**. A catraca cobra essas quatro classes e raízes ilegíveis. Nomear `Posto` preserva a trava que já existia. [feriado_do_posto.py:307](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/feriado_do_posto.py:307) · [feriado_do_posto.py:325](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/feriado_do_posto.py:325) · [chamados/models.py:2816](/home/ronald/wt-codex-feriado-por-posto-120132/app/chamados/models.py:2816) · [test_e1_v7_catracas.py:71](/home/ronald/wt-codex-feriado-por-posto-120132/app/chamados/tests/test_e1_v7_catracas.py:71)

### F5 — fonte do dia e do par

A única entrada nova foi `colaboradores/management/commands/marcar_feriado_do_posto.py::Command.handle`, em **B2−B1**, o critério largo de dia. O detector marca `localdate(agora)`. O comando obtém `agora` de `timezone.now()` e passa o dia civil da execução à porta. O **dia do fato** vem de `dias_do_fato`, o **vínculo desse dia** de `vinculos_dos_dias`, e os **turnos da prévia** de `turnos_do_colab`. A prévia agrupa os turnos pelo `data_turno` já retornado e lê `entrada` e `saida` dos pares entregues. [core/medida_turno_do_par.py:220](/home/ronald/wt-codex-feriado-por-posto-120132/app/core/medida_turno_do_par.py:220) · [marcar_feriado_do_posto.py:46](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/management/commands/marcar_feriado_do_posto.py:46) · [feriado_do_posto.py:248](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/feriado_do_posto.py:248) · [feriado_do_posto.py:453](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/feriado_do_posto.py:453)

### Escritores e leitores

| Campo ou valor | Quem escreve e quem lê |
|---|---|
| `Posto.pk`, `nome`, `municipio`, `opera_em_feriado` | `criar_posto` insere a linha e `definir_municipio_posto` grava a FK. `marcar_opera_em_feriado` grava a marca. O comando lê o posto pelo `pk` e imprime nome, município e marca anterior. `aplicar_marca` relê a marca sob trava para planejar e para interromper segmentos que tenham sido superados por outra marca. `eh_feriado_do_posto` usa o município ao consultar `feriados_de`. `colaboradores/services/estrutura.py:43`, `:90`, `:99` · `colaboradores/management/commands/marcar_feriado_do_posto.py:111`, `:134` · `colaboradores/services/feriado_do_posto.py:55`, `:309`, `:328`, `:346` |
| `EscalaColaborador.posto`, `trabalha_em_feriado`, `ChamadoColaborador.status_local` e dia do fato | A porta de vínculo grava o posto ao criar ou reaproveitar o vínculo e grava a chave de feriado em `marcar_trabalha_em_feriado`. `_chamados_do_posto` lê o posto do vínculo devolvido para o dia do fato e recebe os vivos de `qs_vivos`, que filtra `status_local`. A fixture grava `aberto` e `contexto_json.data_turno`. `dias_do_fato` lê `celula.data` ou, no degrau seguinte, `contexto_json.data_turno`. No ato de retratar, `reconciliar` delega a escrita do estado a `_gravar_estado`. `colaboradores/services/vinculo.py:274`, `:287`, `:898` · `colaboradores/services/feriado_do_posto.py:244`, `:283`, `:350` · `chamados/services/painel.py:155` · `chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:96` · `chamados/juizes.py:187` · `chamados/models.py:234`, `:667` |
| `plano` e totais da aplicação | `plano_da_marca` produz `candidatos`, `sem_dia`, `vivos`, `alvo`, `propagar`, `marca_antes` e `marca_muda`. `aplicar_marca` consome o plano e devolve `marca_muda`, `retratados`, `propagados` e `plano`. A prévia repassa esses valores, e o comando lê o plano e imprime os totais do `--apply`. `colaboradores/services/feriado_do_posto.py:277`, `:318`, `:325`, `:343`, `:354`, `:406` · `colaboradores/management/commands/marcar_feriado_do_posto.py:132`, `:152` |
| Foto da célula e `a`, `b`, `outras` | A porta da célula grava `colaborador_id`, `data`, `trabalha`, `escala_geradora` e `dna` na geração, `ata`, `veredito` e `veredito_em` na lavra, e `julgada_em` e `impressao` no carimbo. `_foto_das_celulas` lê a foto, `_classificar_previa` produz as três listas, e `_imprimir_previa` as lê. `ponto/portas/celula.py:293`, `:306`, `:1090`, `:1194` · `colaboradores/services/feriado_do_posto.py:403`, `:409`, `:431`, `:462` · `colaboradores/management/commands/marcar_feriado_do_posto.py:78` |
| Rótulo e `CLASSIFICAR` | O comando escreve o rótulo em `stdout`; `_dry` seleciona a linha, e as duas asserções passam a lê-la com o literal atual. O censo publica `a_classificar_pela_regra`; a lista manual `CLASSIFICAR` guarda a expectativa e o teste compara as duas por igualdade. `colaboradores/management/commands/marcar_feriado_do_posto.py:137` · `chamados/tests/test_jv1_volta5_posto_do_dia_nos_vetos.py:83`, `:94`, `:100` · `core/medida_turno_do_par.py:432` · `core/tests/test_medida_turno_do_par.py:389`, `:566` |

**LEI-AKITA 8.** O rótulo `candidatos a retratacao no feriado` descreve `len(plano['alvo'])`. O plano inclui chamados vivos no posto do dia do fato, alcançados pelo feriado, numa mudança para FECHA ou na retomada explícita. O total efetivamente retratado é contado depois de passar pelo gate de reconciliação. [feriado_do_posto.py:277](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/feriado_do_posto.py:277) · [feriado_do_posto.py:331](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/feriado_do_posto.py:331) · [marcar_feriado_do_posto.py:137](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/management/commands/marcar_feriado_do_posto.py:137)

**Vizinho do rótulo antigo.** O `git grep` na wt inteira encontrou somente `chamados/tests/e4b_c12_base.json:13` após a correção. Ali está o **emissor histórico** congelado para os testes c12. O verificador lê a fonte congelada, e o manifesto aponta nela o sítio S19 da retratação. O outro teste de saída já espera o rótulo atual. [e4b_c12_base.json:13](/home/ronald/wt-codex-feriado-por-posto-120132/app/chamados/tests/e4b_c12_base.json:13) · [e4b_c12_verificar.py:18](/home/ronald/wt-codex-feriado-por-posto-120132/app/chamados/tests/e4b_c12_verificar.py:18) · [e4b_c12_sitios.json:148](/home/ronald/wt-codex-feriado-por-posto-120132/app/chamados/tests/e4b_c12_sitios.json:148) · [test_feriado_por_posto.py:636](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/tests/test_feriado_por_posto.py:636)

### Comandos

De `app/`, a função abaixo reproduz as provas em memória. Nas catracas estáticas, `unittest.TestCase` serve de base para executar os módulos originais. Em F3 e F4, a prova executa a linha de saída e a asserção original com as contagens do anexo. [test_contract_no_except_pass.py:2](/home/ronald/wt-codex-feriado-por-posto-120132/app/ponto/tests/test_contract_no_except_pass.py:2) · [test_e1_v7_catracas.py:51](/home/ronald/wt-codex-feriado-por-posto-120132/app/chamados/tests/test_e1_v7_catracas.py:51) · [test_medida_turno_do_par.py:95](/home/ronald/wt-codex-feriado-por-posto-120132/app/core/tests/test_medida_turno_do_par.py:95)

```bash
prova() {
  PYTHONDONTWRITEBYTECODE=1 python3 - "$@" <<'PY'
from importlib import import_module
from io import StringIO
from pathlib import Path
import sys
from textwrap import dedent
from types import ModuleType
import unittest
from unittest.mock import patch

case, mode, *targets = sys.argv[1:]
command = 'colaboradores/management/commands/marcar_feriado_do_posto.py'

if case in ('F3', 'F4'):
    line, count = {'F3': (94, 1), 'F4': (100, 0)}[case]
    rows, out = [None] * count, []
    reader = Path('chamados/tests/test_jv1_volta5_posto_do_dia_nos_vetos.py')
    expr = dedent(''.join(reader.read_text().splitlines(True)[line-1:line+1]))
    if mode == 'mutante':
        expr = expr.replace('candidatos a retratacao no feriado', 'em feriado que o alcanca')
    emit = dedent(''.join(Path(command).read_text().splitlines(True)[136:138]))
    exec(emit, dict(w=out.append, plano={'vivos': rows}, alvo=rows))
    check = unittest.TestCase()
    check._dry = lambda: out[0]
    exec(compile('\n' * (line - 1) + expr, str(reader), 'exec'), {'self': check})
    print(out[0])
else:
    django, test = ModuleType('django'), ModuleType('django.test')
    test.SimpleTestCase = unittest.TestCase
    sys.modules.update({'django': django, 'django.test': test})
    path = Path('colaboradores/services/feriado_do_posto.py').resolve()
    source = path.read_text().splitlines(True)
    if mode == 'mutante' and case == 'F1':
        source[405] = '        pass\n' + source[405][4:]
    if mode == 'mutante' and case == 'F2':
        source[306] = source[306].replace('Posto.objects', 'type(posto).objects')
    if mode == 'mutante' and case == 'F5':
        module = import_module('core.tests.test_medida_turno_do_par')
        module.CLASSIFICAR['B2_menos_B1'].remove(command + '::Command.handle')
    real_open = open

    def read(name, *args, **kwargs):
        if isinstance(name, (str, Path)) and Path(name).resolve() == path:
            return StringIO(''.join(source))
        return real_open(name, *args, **kwargs)

    with patch('builtins.open', read):
        unittest.main(module=None, argv=['unittest', *targets], warnings='ignore')
PY
}

prova F1 cura ponto.tests.test_contract_no_except_pass
prova F2 cura chamados.tests.test_e1_v7_catracas
prova F3 cura
prova F4 cura
prova F5 cura core.tests.test_medida_turno_do_par

prova F1 mutante ponto.tests.test_contract_no_except_pass.ContractNoExceptPassTest.test_sem_except_pass_novo
prova F2 mutante chamados.tests.test_e1_v7_catracas.CatracaTravaTest.test_zero_trava_fora_de_trava_na_ordem
prova F3 mutante
prova F4 mutante
prova F5 mutante core.tests.test_medida_turno_do_par.CatracaTurnoDoParTest.test_a_classificar_publicado_sem_entrar_como_criterio_amplo

git grep -ni -F -e 'em feriado que o alcanca' -e 'em feriado que o alcança' -- :/
```

Comando do módulo F3/F4, que parou na importação local. [manage.py:11](/home/ronald/wt-codex-feriado-por-posto-120132/app/manage.py:11)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados.tests.test_jv1_volta5_posto_do_dia_nos_vetos --settings=config.settings_test
```

_7738 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
