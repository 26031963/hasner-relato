# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 03:07** · publicado em **11/11/2026 03:07** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 03:07 · sessao 3e4f7f5604cf · - · - · origem=Ronald (tmux)

1. `relato_negacao` extrai a regra inteira por `[A-Z0-9-]+`; portanto grava `NEGOU B3-SEM-OBRA`, distinto de `NEGOU B3` e `NEGOU B3-NAO-SEI` (`bin/disjuntor.py:262`, `bin/disjuntor.py:279`, `bin/disjuntor.py:1094`, `bin/disjuntor.py:1647`).

2. **Há colisão lexical com a gramática de ID**, porque ambos os nomes satisfazem `_OBRA_ID` e `_OBRA_HIF`, sem reserva desses nomes; eles continuam distintos entre si (`bin/disjuntor.py:157`, `bin/disjuntor.py:163`, `bin/disjuntor.py:619`). `obras_da_linha` aceita ambos como tokens de obra (`bin/disjuntor.py:637`, `bin/disjuntor.py:640`).

3. **Ambos são inertes no consumidor decisório existente:** `linhas_libera_b3` consulta o conjunto, mas só acrescenta uma liberação se a linha começar por `DESENHO ` ou `ERRO-BOBO `; nenhuma negativa `DISJUNTOR ...` nem linha `**... disjuntor (NEGOU ...)` satisfaz isso (`bin/disjuntor.py:735`, `bin/disjuntor.py:737`, `bin/disjuntor.py:747`, `bin/disjuntor.py:748`, `bin/disjuntor.py:751`). A referência decisória a `obras_da_linha` encontrada é essa chamada; os demais usos localizados são documentação e testes (`bin/disjuntor.py:735`, `bin/tests/test_disjuntor.sh:746`, `bin/tests/test_disjuntor.sh:1668`, `bin/tests/test_disjuntor.sh:1669`).

4. Reproduzi somente em memória, compilando as constantes e funções do AST da cópia: as negativas dão respectivamente `{'B3-SEM-OBRA'}` e `{'B3-NAO-SEI', 'OBRA-X'}`; `linhas_libera_b3` devolve `[]` para ambas, inclusive quando a obra consultada é literalmente um desses dois nomes. O resultado decorre dos reconhecedores e das exigências de prefixo acima (`bin/disjuntor.py:157`, `bin/disjuntor.py:174`, `bin/disjuntor.py:619`, `bin/disjuntor.py:631`, `bin/disjuntor.py:735`, `bin/disjuntor.py:737`, `bin/disjuntor.py:748`).

5. O delta mantém a porta única de escrita: a negativa chama `relato_negacao`, que monta a linha e chama `ev.no_relato`; não introduz segundo escritor ou derivação paralela nesse trajeto (`bin/disjuntor.py:1907`, `bin/disjuntor.py:279`, `bin/disjuntor.py:292`, `bin/disjuntor.py:294`). A ambiguidade entre negativa sem obra e disparo `DISJUNTOR B3 <obra>` é removida na constante, enquanto a mensagem efetiva de parada conserva o formato anterior (`bin/disjuntor.py:1647`, `bin/disjuntor.py:1149`).

## 11/11 03:07 · sessao b01916492971 · - · - · origem=Ronald (tmux)

Nenhum FILA fornecido demonstra bloqueio novo do item (1) neste delta; a porta do agente `verificador` final exige `_obra_de(prompt)` e nega ausência em `bin/disjuntor.py:1842-1845`.

- **FILA 2:** há tradução explícita entre lista persistida e número publicado: `_b3_sem_obra` lê `b3_sem_obra_obs` e deduplica `aid` (`bin/disjuntor.py:2198-2202`); `ler()` publica esse resultado em `b3_sem_obra` (`bin/disjuntor.py:2045`); histórico é contado separadamente (`bin/disjuntor.py:2205-2208`). Não demonstra campo com dois significados na mesma camada.
- **FILA 3:** os limites permanecem: `_FINAL_RE` admite leitura/final na mesma linha sem identificar intenção (`bin/disjuntor.py:177-179`); a porta restringe-se a `subagent_type == 'verificador'` (`bin/disjuntor.py:1842`); o coletor restringe-se a `agentType == 'verificador'` (`bin/disjuntor.py:840`). Portanto uma garantia irrestrita sobre qualquer agente/modelo seria falsa; são limitações existentes fora do delta fornecido [nao verificado quanto ao histórico Git].
- **Chave:** a dispensa é expressa na arquitetura (`bin/disjuntor.py:35`); `main_pretool()` deixa de chamar `pretool()` quando desligado (`bin/disjuntor.py:1896-1900`). Não constitui nova fuga criada pelo delta.
- **FILA 4:** o contador é cumulativo: só acrescenta IDs novos (`bin/disjuntor.py:2178-2183`), e o leitor não aplica janela (`bin/disjuntor.py:2201-2202`). Isso é compatível com “esperado 0 a partir daqui” do aval fornecido [nao verificado em arquivo]; uma ocorrência posterior deixa corretamente registrado que a meta foi violada.
- **FILA 5:** o selo histórico não chama `linha()` (`bin/tests/test_disjuntor.sh:1484-1498`), e o mutante remove validação de elementos (`bin/tests/test_disjuntor.sh:1542-1544`), permitindo morte incidental por `e['aid']` (`bin/disjuntor.py:2202`). É fraqueza do selo existente; a apresentação atual seleciona contador e histórico separadamente (`bin/disjuntor.py:2271-2275`).
- **FILA 6:** o rótulo mencionado só consta do anexo fornecido [nao verificado]; não demonstra falha funcional da guarda, localizada em `bin/disjuntor.py:1842-1845`.

O zero efetivo de produção e a identificação individual das seis obras históricas não estão demonstrados pelos dados fornecidos [nao verificado]. Não executei a suite, que cria e remove arquivos (`bin/tests/test_disjuntor.sh:39-41`, `bin/tests/test_disjuntor.sh:50-55`).

## 11/11 03:07 · sessao 63b7724082f8 · - · - · origem=Ronald (tmux)

1. A regra muda corretamente para `B3-SEM-OBRA`: o extrator aceita hífens e publica a regra integral no RELATO (`bin/disjuntor.py:262`, `bin/disjuntor.py:279`). A negativa continua passando por `trilha('NEGOU', neg)` e `relato_negacao`, sem chamar disparo (`bin/disjuntor.py:1904`, `bin/disjuntor.py:1907`).

2. `ler()` distingue o evento pelo terceiro token da trilha; `NEGOU` incrementa negativas e somente `DISJUNTOR` incrementa disparos (`bin/disjuntor.py:1984`, `bin/disjuntor.py:1995`, `bin/disjuntor.py:2003`). Portanto a negativa nova não entra como disparo `B3`; o disparo verdadeiro permanece anunciado pela transição do estado com detalhe `B3 <obra>` (`bin/disjuntor.py:1012`, `bin/disjuntor.py:1043`).

3. Há colisão **lexical** com IDs legítimos: `B3-SEM-OBRA` e `B3-NAO-SEI` satisfazem `_OBRA_ID`/`_OBRA_HIF`, pois não há reserva desses nomes (`bin/disjuntor.py:157`, `bin/disjuntor.py:163`, `bin/disjuntor.py:619`). `obras_da_linha` consequentemente aceita os tokens das duas regras (`bin/disjuntor.py:637`). Entretanto, seu consumidor encontrado exige além da obra o prefixo `DESENHO ` ou `ERRO-BOBO `; nenhuma negativa satisfaz isso (`bin/disjuntor.py:735`, `bin/disjuntor.py:737`, `bin/disjuntor.py:748`). **Ambas são inertes nesse consumidor**, mesmo que a obra real tenha exatamente um desses IDs (`bin/disjuntor.py:735`, `bin/disjuntor.py:737`, `bin/disjuntor.py:748`).

4. O Stop usa diretamente o mapa retornado por `julga_b3`: só produz `mensagem_b3(obra)` para as obras desse mapa; a mensagem de incerteza é separada (`bin/hook_stop_fila1.py:1204`, `bin/hook_stop_fila1.py:1206`, `bin/hook_stop_fila1.py:1208`). `gerar_estado.py` delega a leitura e a composição a `disjuntor.ler`/`linha`, sem extrator paralelo (`bin/gerar_estado.py:280`).

5. O delta não introduz outro escritor: `relato_negacao` segue chamando `_no_relato().no_relato` (`bin/disjuntor.py:292`, `bin/disjuntor.py:294`). A guarda continua devolvendo a negativa antes de consultar o estado B3 quando `_obra_de` não reconhece o ID (`bin/disjuntor.py:1842`, `bin/disjuntor.py:1845`, `bin/disjuntor.py:1847`).

Falsa negativa adicional decorrente da troca de prefixo nos leitores examinados: não encontrada pelos caminhos citados; consumidores externos à cópia e efeitos em produção: [nao verificado].

## 11/11 03:03 · sessao a8910a847d4e · FIO-E0-V4-E3 · construtor · origem=Code

Diff c8c escrito para revisão. ORM, suite inteira e certificação pelo opus: **[nao verificado]**. Em `juizes.py`, a alteração apenas transporta `agora` nas três chamadas alimentadas do CF; a comparação AST preservou o restante do predicado. (chamados/juizes.py:692,783,1416)

Por item:

| Item | Cura | RED e mutante |
|---|---|---|
| 1 | Alimenta todas as perguntas dos dias elegíveis, incluindo posições desaparecidas. Sem default para `viva_c6`. (chamados/services/tarefa_do_dia.py:492) | ORM: `test_RED_posicao_sumiu_avulso_e_lote_reais`. Sem banco: `test_RED_posicao_ausente_e_alimentada`. Mutante `universo_incompleto`, **rc=1**. (chamados/tests/test_e3_c8c_orm.py:35; chamados/tests/test_e3_c8c_sem_banco.py:40) |
| 2 | Foto privada recorta resposta, validação e materialização posteriores ao horizonte; o construtor entrega essa foto aos leitores. (chamados/services/tarefa_do_dia.py:338,402) | ORM: `test_RED_T1_inteiro_antes_depois_validacao_real_T2`. Mutantes `validacao_futura`, `materializacao_futura`, `resposta_futura`: **rc=1 cada**. (chamados/tests/test_e3_c8c_orm.py:46; chamados/tests/test_e3_c8c_sem_banco.py:121) |
| 3 | CF exige `agora` quando recebe carga. Avulso conserva a leitura do relógio no teto. (escala/servico_jornada.py:292,387) | ORM: `test_RED_CF_agora_12_independe_do_relogio_ambiente`. Sem banco também verifica o comportamento avulso às 12h/20h. Mutante `relogio_ambiente`, **rc=1**. (chamados/tests/test_e3_c8c_orm.py:76; chamados/tests/test_e3_c8c_sem_banco.py:66) |
| 4 | A carga chama o mesmo `escala_vigente`, alimentado pelo índice de vínculos próprios **e geradoras carregadas**. (chamados/services/carga_pergunta_viva.py:89,162; escala/servico_jornada.py:55) | ORM: `test_RED_geradora_de_outro_colaborador_carga_e_avulso`. Mutante `geradora_so_proprio`, **rc=1**. (chamados/tests/test_e3_c8c_orm.py:63; chamados/tests/test_e3_c8c_sem_banco.py:15) |
| 5 | B5-j segue helpers `self/cls` da mesma classe; comentário corrigido. Teto permanece 8. (ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:132,191; ponto/turnos.py:3015) | `test_MORDE_carregador_extraido_da_classe`. Mutantes `selo_nao_segue_helper` e `batidas_cruas_no_helper`: **rc=1 cada**. (ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:195; chamados/tests/e3_c8c_mutantes.py:39) |
| 6 | K6 levanta `ValueError` para linha de outra tripla. (folha/export.py:584) | `test_RED_K6_recusa_outra_tripla`, cobrindo empresa/mês/ano e `registro`. Mutante `tripla_alheia`, **rc=1**, seis assertions. (chamados/tests/test_e3_c8c_sem_banco.py:91) |
| 8a | Exposição administrativa no ramo dispensado fica restrita ao dia sem trabalho; lâmpada acesa de trabalho responde primeiro. Teste E2 preservado. (chamados/services/tarefa_do_dia.py:263; chamados/tests/test_e2_tarefa_do_dia.py:873) | `test_RED_lampada_acesa_antes_da_pergunta`. Mutante `acesa_expoe_admin`, **rc=1**. (chamados/tests/test_e3_c8c_sem_banco.py:32) |
| 8b | `local.date()` saiu do slot; usa `dia_do_turno_da_celula`. Calendário da alimentação reutiliza `_hoje`. Cat raca e registros preservados. (chamados/services/carga_pergunta_viva.py:135; chamados/services/tarefa_do_dia.py:504) | Cat raca existente: **35 testes, rc=0**. Mutante `dia_derivado_do_slot`, **rc=1**. (core/tests/test_medida_turno_do_par.py:478; chamados/tests/test_e3_c8c_sem_banco.py:151) |
| 8c | Removidos `datetime/timedelta` e `JUIZES` mortos. (chamados/tests/e3_c8b_apoio.py:8; chamados/tests/test_e3_c8b_sem_banco.py:14) | Ruff: **rc=1**, módulo ausente; lint **[nao verificado]**. |

A impossibilidade da geradora estrangeira por todos os escritores de cadastro permanece **[nao verificado]**. A reversão copia `dna_anterior['vinculo_id']`; a paridade foi corrigida independentemente disso. (ponto/portas/celula.py:789)

Execuções locais, todas sem banco:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -v chamados.tests.test_e3_tarefa_admin_sem_banco chamados.tests.test_e3_c8b_sem_banco chamados.tests.test_e3_c8c_sem_banco
# rc=0 — 41 testes [nao verificado]

PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_c8b_mutantes
# rc=0 — agregado dos 14 mutantes anteriores

PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_c8c_mutantes
# rc=0 — agregado dos 11 novos mutantes
```

Os REDs iniciais sem banco deram **rc=1 por assertion**, antes das curas; a catraca inicial deu **rc=1, seis falhas**. Os selos B5-j e turno passaram posteriormente com **7 e 35 testes**, usando adaptação em memória apenas de `SimpleTestCase` para `unittest.TestCase`. (chamados/tests/test_e3_c8c_sem_banco.py:15; ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:194; core/tests/test_medida_turno_do_par.py:478)

Comando individual de cada mutante abaixo: todos foram executados e retornaram **rc=1, FAIL>0, ERROR=0**. Definições e executor: chamados/tests/e3_c8c_mutantes.py:16,79.

```bash
mutar() {
  PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_c8c_mutantes --mutante "$1"
}
mutar universo_incompleto
mutar validacao_futura
mutar materializacao_futura
mutar resposta_futura
mutar relogio_ambiente
mutar geradora_so_proprio
mutar selo_nao_segue_helper
mutar batidas_cruas_no_helper
mutar tripla_alheia
mutar acesa_expoe_admin
mutar dia_derivado_do_slot
```

Os quatro TestCase ORM estão em [test_e3_c8c_orm.py:9](/home/ronald/wt-codex-fio-e0-v4-e3-024128/app/chamados/tests/test_e3_c8c_orm.py:9), herdando o cadastro ORM existente, sem `__new__`. Para a sessão executar:

```bash
PYTHONDONTWRITEBYTECODE=1 bin/suite.sh --only chamados.tests.test_e3_c8c_orm.C8cCargaReal
```

Mutantes ORM preparados, com **rc ainda [nao verificado]**; ativação somente em memória. (chamados/tests/e3_c8c_mutantes.py:54)

```bash
E3_C8C_MUTANTE_ORM=universo_incompleto PYTHONDONTWRITEBYTECODE=1 bin/suite.sh --only chamados.tests.test_e3_c8c_orm.C8cCargaReal.test_RED_posicao_sumiu_avulso_e_lote_reais

E3_C8C_MUTANTE_ORM=sem_foto_na_carga PYTHONDONTWRITEBYTECODE=1 bin/suite.sh --only chamados.tests.test_e3_c8c_orm.C8cCargaReal.test_RED_T1_inteiro_antes_depois_validacao_real_T2

E3_C8C_MUTANTE_ORM=relogio_ambiente PYTHONDONTWRITEBYTECODE=1 bin/suite.sh --only chamados.tests.test_e3_c8c_orm.C8cCargaReal.test_RED_CF_agora_12_independe_do_relogio_ambiente

E3_C8C_MUTANTE_ORM=geradora_so_proprio PYTHONDONTWRITEBYTECODE=1 bin/suite.sh --only chamados.tests.test_e3_c8c_orm.C8cCargaReal.test_RED_geradora_de_outro_colaborador_carga_e_avulso
```

Censo produtivo dos chamadores CF, incluindo aliases:

- chamados/juizes.py:690,781,1414.
- chamados/services/disputa_emissao.py:288,806; wrapper lido em :1200,1237 e reexportado por chamados/models.py:4093.
- chamados/management/commands/retratar_chamados_lei_cala.py:45.
- chamados/management/commands/auditar_invariantes_chamados.py:1683,1709,1804.
- ponto/management/commands/reconciliar_grade.py:243.
- ponto/marcos.py:171.
- escala/servico_jornada.py:566.

Chamadas dinâmicas sem identificador literal: **[nao verificado]**.

Escritores e leitores dos valores transitórios tocados:

| Valor | Quem escreve/alimenta | Quem lê |
|---|---|---|
| Foto da pergunta | `_pergunta_no_instante`. (chamados/services/tarefa_do_dia.py:338) | Construtor, passivo, lavra e tarefa. (mesmo arquivo:402,421,426,538) |
| `viva_c6` | Retorno do C6 na carga. (chamados/services/tarefa_do_dia.py:506) | Exposição e residual. (mesmo arquivo:245,328) |
| `CargaDaVida.agora` / kwarg CF `agora` | Carga recebe `tarefa.agora`; C6 encaminha nas três chamadas. (chamados/services/carga_pergunta_viva.py:88; chamados/juizes.py:692,783,1416) | Guarda obrigatória e teto CF. (escala/servico_jornada.py:292,387) |
| `_escalas_c6` / `escalas_por_id` | Índice dos vínculos e candidatos carregados. (chamados/services/carga_pergunta_viva.py:89,162) | Carregador comum `escala_vigente`. (escala/servico_jornada.py:55,58,64) |

Além do C6, os carimbos são lidos pelo passivo E1, pela validade da lavra, pela identidade da tarefa, pelo slot e pela concordância com a luz. (chamados/models.py:3924,3931,3936; chamados/veredito_lavrado.py:224,228; chamados/services/tarefa_do_dia.py:230; chamados/services/materializacao.py:279,358,411; chamados/juizes.py:1323)

Os escritores persistentes continuam nas portas existentes de resposta, validação e materialização. (chamados/services/disputa_emissao.py:1467,1652; chamados/services/validacao.py:134; chamados/models.py:3097)

AST dos 12 arquivos tocados e `git diff --check`: **rc=0**. Schema da tarefa preservado pelo selo existente. (chamados/tests/test_e3_c8b_sem_banco.py:81) Suite inteira, resultados ORM, impacto produtivo e aprovação final: **[nao verificado]**.

## 11/11 02:52 · sessao d7f2c3787aa4 · FIO-E0-V4-E3 · sonda · origem=Code

1. **174 pares, 15 colaboradores e soma geral de candidatas = 0**, conforme o anexo fornecido. [nao verificado]

| Colaborador | Soma de candidatas |
|---|---:|
| col144 | 0 [nao verificado] |
| col209 | 0 [nao verificado] |
| col237 | 0 [nao verificado] |
| col268 | 0 [nao verificado] |
| col312 | 0 [nao verificado] |
| col348 | 0 [nao verificado] |
| col427 | 0 [nao verificado] |
| col574 | 0 [nao verificado] |
| col650 | 0 [nao verificado] |
| col969 | 0 [nao verificado] |
| col1005 | 0 [nao verificado] |
| col1009 | 0 [nao verificado] |
| col1018 | 0 [nao verificado] |
| col1019 | 0 [nao verificado] |
| col1027 | 0 [nao verificado] |

O único `perg_total=1` está em col237, com zero candidatas; o anexo não identifica qual condição a exclui. [nao verificado]

2. **Não há caminho alternativo de seleção para carimbar essas perguntas sem FK de célula.** O escritor percorre `perguntas_absolvidas_pela_celula` e carimba somente as perguntas recebidas desse gerador (`app/chamados/reconciliador.py:584`, `:595`). O gerador parte de `qs_perguntas_absolviveis` (`app/chamados/reconciliador.py:651`), que exige colaborador da disputa, ambos os carimbos nulos e **`celula__isnull=False`** (`app/chamados/reconciliador.py:627-631`). Compartilhar disputa não substitui esse vínculo (`app/chamados/reconciliador.py:627-642`).

3. **Sim, o universo descrito pela sonda é suficiente para essa pergunta:** ela conta perguntas cuja própria célula pertence ao colab/dia e aplica os dois filtros de nulidade; o resultado informado é zero. A implementação da sonda não foi aberta. [nao verificado]

Essa contagem é um **superconjunto** do queryset efetivo: além desses requisitos, o código exige veredito da célula não nulo, não vazio e fora de `VEREDITOS_ACUSAM`; depois, o gerador ainda consulta o juiz e a tranca (`app/chamados/reconciliador.py:637-641`, `:661-668`). Portanto, zero nesse superconjunto implica zero selecionáveis nesses pares por essa via (`app/chamados/reconciliador.py:627-642`).

Uma pergunta ligada à célula de **outro dia** não integra o recorte desses 174 pares só por compartilhar disputa. [nao verificado] O escritor roda por colaborador e pode selecionar outros dias, mas julga a célula vinculada à própria pergunta (`app/chamados/reconciliador.py:628`, `:637-641`, `:651`).

## 11/11 02:52 · sessao 62e1af2f76c7 · - · - · origem=Ronald (tmux)

Censo produtivo do CF na base:

- `chamados/juizes.py:690,781,1414`.
- `chamados/services/disputa_emissao.py:288,806`; wrapper de `:806` chamado em `:1200,1237` e reexportado em `chamados/models.py:4093`.
- `chamados/management/commands/retratar_chamados_lei_cala.py:45`.
- `chamados/management/commands/auditar_invariantes_chamados.py:1683,1709,1804`.
- `ponto/management/commands/reconciliar_grade.py:243`.
- `ponto/marcos.py:171`.
- `escala/servico_jornada.py:554`.

Hits em API e reconciliador são comentários, sem chamadas diretas. Chamadas dinâmicas sem identificador literal: [nao verificado].

A divergência da geradora estrangeira vem da alimentação: avulso entrega diretamente `celula.escala_geradora` ao juiz (`escala/servico_jornada.py:55-58`); lote mantém vínculos somente no balde do proprietário (`ponto/turnos.py:2990-2991`). `_cand` já inclui geradoras pedidas por `mais` (`ponto/turnos.py:2986-2987`), porém o carregador plural coloca a estrangeira no balde `e.colaborador_id` (`escala/alimentacao.py:155-159`). Assim, existe linha carregada suficiente para corrigir sem N+1.

Proposta: fazer `CargaDaVida.vinculo` chamar `escala_vigente` com alimentação opcional dos vínculos já lidos; o corpo comum resolve a geradora por PK, usando índice composto de `_cand` e `_vinculos`, e usa os candidatos do próprio colaborador quando não existe geradora. A seleção continua exclusivamente em `vinculo_do_dia` (`escala/alimentacao.py:267-289`).

Impossibilidade pelo cadastro não está demonstrada. Geração usual vincula a célula ao proprietário do vínculo (`ponto/portas/celula.py:259-260,855-856`), mas reversão copia diretamente `dna_anterior['vinculo_id']` para a FK (`ponto/portas/celula.py:789-791`). Ocorrência produtiva: [nao verificado].

Leitores adicionais dos carimbos pela carga:

- Passivo: `DecisaoPergunta.linhas_da_projecao_sem_linha` lê `respondida_em`, texto, `validada_em`, `validada_por_id` (`chamados/models.py:3924,3931-3938`), chamado em `chamados/services/tarefa_do_dia.py:394`.
- Lavra: `esta_stale` lê `julgado_em` e `janela_expira_em` (`chamados/veredito_lavrado.py:224,228`), chamado em `tarefa_do_dia.py:399`.
- Identidade da tarefa lê `via_resolucao` (`tarefa_do_dia.py:230-231`).
- C6 lê validação, resposta, materialização e via (`chamados/juizes.py:1346,1357,1362,1383,1389,1421`).
- Auxiliares de materialização leem materialização/via (`chamados/juizes.py:288,671,683,699`).
- Slot e concordância leem texto (`chamados/juizes.py:354,1323`; `chamados/services/materializacao.py:279,358,411`).

Escritores: validação real (`chamados/services/validacao.py:134-141`); resposta (`chamados/services/disputa_emissao.py:1462-1468`), resposta automática (`:1652-1657`); carimbo de materialização/via (`chamados/models.py:3099-3101`).

Nenhuma escrita ou execução ORM foi feita nesta exploração.

## 11/11 02:52 · sessao 62dfbb574916 · - · - · origem=Ronald (tmux)

Use `_Base`, que já é `TestCase` ORM real e fornece cadastro, vínculo, colaborador, admin, célula, pergunta e decisão; os construtores reais são `carga_da_tarefa` e `tarefa_do_dia`. (`chamados/tests/test_e2_tarefa_do_dia.py:75`, `:102`, `:124`, `:133`; `chamados/services/tarefa_do_dia.py:546`, `:551`)

- **RED 1:** célula com DNA `{'marcos': {'hi': '08:00', 'hf': '18:00'}}`, lâmpadas somente E08/S18 e pergunta antiga `intervalo_saida`. Faça chamadas independentes pelo avulso e por `carga_da_tarefa([self.c], D, D, agora).tarefa(self.c,D)`. A pergunta continua no universo `_perg`, mas sua posição sumiu da grade; compare o dict inteiro e a exposição administrativa após decisão RESPOSTA. (`chamados/tests/test_e2_tarefa_do_dia.py:102`, `:124`, `:133`; `chamados/services/tarefa_do_dia.py:373`, `:465`)
- **RED 2:** pergunta textual, célula `trabalha=False`/`tipo_dia='folga'`; resposta real em T1 por `responder_pergunta_disputa_core`; dict inteiro em T1; validação real em T2 por `validar_pergunta`; nova carga e mesmo `agora=T1`; dict deve permanecer inteiro igual. (`chamados/services/disputa_emissao.py:1425`, `:1467`, `:1512`; `chamados/services/validacao.py:373`, `:435`)
- A validação textual completa pode retornar `nao_gravou`: texto não tem ramo materializador; o carimbo e a decisão VALIDACAO permanecem, pois a devolução é específica de resposta HORA. Exija no RED que `validada_em == T2` e que a linha VALIDACAO tenha T2; não exija `res['ok']`. (`chamados/services/materializacao.py:1226`, `:1244`; `chamados/services/validacao.py:457`, `:469`, `:496`)
- O escritor menor `_carimbar_validacao` também grava carimbo+linha reais, já usado diretamente por teste existente; a porta completa acima cobre mais do contrato pedido. (`chamados/services/validacao.py:127`; `chamados/tests/test_e1_entradas.py:95`)
- **RED 4:** depois de criar a célula de `self.c`, troque sua FK para `self.ec2` por ORM e compare `escala_vigente(self.c,D)` com `carga_da_vida(carga_real, perguntas).vinculo(self.c,D)`, sem substituir `escala_vigente`. O avulso entrega a geradora nomeada diretamente. (`escala/servico_jornada.py:55`; `chamados/services/carga_pergunta_viva.py:157`)
- A geração normal usa `colaborador_id=ec.colaborador_id`; edição usa o vínculo resolvido para `_cid`. Isso não prova impossibilidade por todos os escritores; classifique essa impossibilidade como `[nao verificado]`. (`ponto/portas/celula.py:259`, `:603`)

Nenhum arquivo escrito e nenhum ORM executado. [nao verificado]

## 11/11 02:50 · sessao 3ce02936cad3 · VERIFICADOR-EM-FABLE · verificador · origem=Code

APROVA

1. **#6 fechado no delta.** O campo persistido `b3_sem_obra` é histórico de IDs string; `b3_sem_obra_obs` contém observações com `aid` e `ts_observado`. O campo determina o significado; o tipo serve para validar. Não encontrei escritor operacional remanescente que acrescente histórico. (`bin/disjuntor.py:2173`, `bin/disjuntor.py:2174`, `bin/disjuntor.py:2182`, `bin/disjuntor.py:2200`, `bin/disjuntor.py:2206`)

   Atenção ao nome na saída: `ler()['b3_sem_obra']` continua sendo o **número derivado de observações**, não a lista histórica persistida. Os consumidores usam esse número. (`bin/disjuntor.py:2044`, `bin/tests/test_disjuntor.sh:430`)

2. **#4 fechado no delta.** Cada contador valida integralmente seu respectivo campo: não lista, elemento de outro tipo, dict sem `aid` string e `aid` vazio resultam em `None`. JSON quebrado ou raiz não objeto também chegam como `None`; a apresentação mostra “nao sei” nos dois lugares. Histórico só desaparece quando vale zero. (`bin/disjuntor.py:396`, `bin/disjuntor.py:398`, `bin/disjuntor.py:2190`, `bin/disjuntor.py:2192`, `bin/disjuntor.py:2200`, `bin/disjuntor.py:2206`, `bin/disjuntor.py:2274`)

   Reproduzi essas formas em memória, inclusive a apresentação. O zero restante é contratual para arquivo/campo ausente ou lista vazia; não encontrei caminho desses leitores que converta conteúdo ilegível em zero. A validação é independente: histórico inválido não torna uma observação ausente inválida. (`bin/disjuntor.py:394`, `bin/disjuntor.py:2189`, `bin/disjuntor.py:2201`, `bin/disjuntor.py:2207`)

3. **Escritores e leitores encontrados em `bin/`:**

   | Sítio | Papel |
   |---|---|
   | `_anota_sem_obra` | Lê histórico e observações para dedup; cria/acrescenta somente observações. (`bin/disjuntor.py:2173`, `bin/disjuntor.py:2178`, `bin/disjuntor.py:2182`) |
   | `decide_b3` / `_com_estado` | Chama o escritor e persiste o estado inteiro sob trava, preservando o histórico. (`bin/disjuntor.py:990`, `bin/disjuntor.py:1054`, `bin/disjuntor.py:351`, `bin/disjuntor.py:371`) |
   | `_b3_sem_obra` / `_b3_sem_obra_hist` | Validam e contam IDs distintos dos respectivos campos. (`bin/disjuntor.py:2200`, `bin/disjuntor.py:2206`) |
   | `ler` / `linha` / CLI | Propagam os dois números e apresentam o resultado. (`bin/disjuntor.py:2044`, `bin/disjuntor.py:2274`, `bin/disjuntor.py:2345`) |
   | `gerar_estado.py` | Usa `ler` e `linha`, sem derivação própria desses campos. (`bin/gerar_estado.py:280`, `bin/gerar_estado.py:281`) |
   | `hook_stop_fila1.py` | Aciona `julga_b3`, que delega a `decide_b3`; não manipula os campos diretamente. (`bin/hook_stop_fila1.py:1204`, `bin/disjuntor.py:1075`) |
   | Testes | Leem número/apresentação; escrevem fixtures dos dois campos; exercitam o escritor e verificam histórico, IDs e tempos. Há também regravação genérica do estado inteiro. (`bin/tests/test_disjuntor.sh:430`, `bin/tests/test_disjuntor.sh:1368`, `bin/tests/test_disjuntor.sh:1483`, `bin/tests/test_disjuntor.sh:1487`, `bin/tests/test_disjuntor.sh:1490`, `bin/tests/test_disjuntor.sh:1493`, `bin/tests/test_disjuntor.sh:1098`) |

   **B3 antigo segue coerente por inspeção:** cria `s1`, aciona Stop e espera `1`; o scanner extrai esse ID, o escritor registra em observações e o contador conta um ID distinto. Execução integral desse caso: **[nao verificado]**. (`bin/tests/test_disjuntor.sh:428`, `bin/tests/test_disjuntor.sh:429`, `bin/tests/test_disjuntor.sh:431`, `bin/disjuntor.py:843`, `bin/disjuntor.py:845`, `bin/disjuntor.py:2182`, `bin/disjuntor.py:2201`)

4. **Pode perder a anotação, mas há sinal no ESTADO.** Com observações não lista, o helper retorna sem registrar a ocorrência e sem aviso próprio; o campo permanece inválido, seu contador retorna `None` e a linha mostra “nao sei”. Falta aviso imediato dessa ocorrência; não há silêncio global nem zero falso. (`bin/disjuntor.py:2175`, `bin/disjuntor.py:2176`, `bin/disjuntor.py:2192`, `bin/disjuntor.py:2274`)

   O dedup suprime igualdade exata de ID: um ID diferente entra; um ID presente no histórico é intencionalmente suprimido. Reuso/colisão real de IDs: **[nao verificado]**. (`bin/disjuntor.py:2177`, `bin/disjuntor.py:2178`, `bin/disjuntor.py:2180`)

5. **Os três mutantes mordem nas reproduções em memória; as âncoras são únicas.** O controle reproduziu as expectativas do caso. (`bin/tests/test_disjuntor.sh:1483`, `bin/tests/test_disjuntor.sh:1494`, `bin/tests/test_disjuntor.sh:1515`, `bin/tests/test_disjuntor.sh:1538`, `bin/tests/test_disjuntor.sh:1542`)

   - `historico-reconta`: acrescenta `aa6` às observações e derruba a expectativa do escritor. (`bin/tests/test_disjuntor.sh:1491`, `bin/tests/test_disjuntor.sh:1494`, `bin/tests/test_disjuntor.sh:1538`)
   - `duplicata-reconta`: produz `2 1`, contrariando `1 1`. (`bin/tests/test_disjuntor.sh:1484`, `bin/tests/test_disjuntor.sh:1485`, `bin/tests/test_disjuntor.sh:1542`)
   - `forma-invalida-vira-zero`: cai primeiro por `KeyError` em `{}`; isolando a fixture seguinte, produz `0 0` em vez de `None None`. Portanto morde, embora a primeira mordida seja por exceção. (`bin/tests/test_disjuntor.sh:1486`, `bin/tests/test_disjuntor.sh:1487`, `bin/tests/test_disjuntor.sh:1539`, `bin/disjuntor.py:2201`)

   **FILA — lacuna do selo:** apesar do comentário, esse caso não chama `linha()`; também não contém fixtures de `aid` vazio/não string nem retorno antecipado do escritor. A correção desses caminhos foi conferida separadamente em memória. (`bin/tests/test_disjuntor.sh:1480`, `bin/tests/test_disjuntor.sh:1482`, `bin/tests/test_disjuntor.sh:1491`, `bin/disjuntor.py:2175`, `bin/disjuntor.py:2200`, `bin/disjuntor.py:2274`)

6. **Nenhuma das classes citadas reaparece como defeito novo demonstrado.** A escrita permanece na mesma porta sob trava; o helper copia o relógio recebido; os consumidores reutilizam os contadores; os mutantes derrubam expectativas; época deixou de depender do tipo. O fallback interno `time.time()` continua presente e não foi introduzido neste delta. (`bin/disjuntor.py:351`, `bin/disjuntor.py:366`, `bin/disjuntor.py:975`, `bin/disjuntor.py:1054`, `bin/disjuntor.py:2182`, `bin/disjuntor.py:2200`, `bin/disjuntor.py:2206`, `bin/gerar_estado.py:281`, `bin/tests/test_disjuntor.sh:1538`)

Suíte shell integral: **[nao verificado]**, pois cria, altera e remove arquivos. Estado efetivo de produção e identificação das obras históricas no RELATO: **[nao verificado]**. (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:41`, `bin/tests/test_disjuntor.sh:1517`)

## 11/11 02:49 · sessao 902d7db0efa9 · - · - · origem=Ronald (tmux)

#4 e #6 fecham no escopo do delta: os contadores validam a coleção inteira e cada elemento, retornam `None` para forma inválida e contam IDs distintos; os campos separam histórico de observações, sem usar tipo para decidir época. (`bin/disjuntor.py:2185`, `bin/disjuntor.py:2192`, `bin/disjuntor.py:2200`, `bin/disjuntor.py:2206`)

- Reproduzi por AST em memória: `None`, `0`, `False`, string, dict, `[42]`, `[{}]`, `aid=None`, inteiro ou vazio retornam `None` no respectivo contador. Estado ausente/campo ausente é a exceção expressamente definida como vazio; estado ilegível levanta erro convertido em `None`. (`bin/disjuntor.py:389`, `bin/disjuntor.py:394`, `bin/disjuntor.py:398`, `bin/disjuntor.py:2186`, `bin/disjuntor.py:2189`, `bin/disjuntor.py:2190`)
- `linha` mostra `nao sei` tanto no contador principal quanto no histórico; o histórico só é omitido quando igual a zero. (`bin/disjuntor.py:2274`)
- Com `obs` não lista, a ocorrência realmente não é anotada e o helper não emite aviso nem retorna erro; entretanto o campo continua inválido e o ESTADO sinaliza `nao sei`. Portanto, falta um aviso imediato nessa transição, mas não ocorre zero falso nem silêncio na apresentação. (`bin/disjuntor.py:2175`, `bin/disjuntor.py:2176`, `bin/disjuntor.py:2192`, `bin/disjuntor.py:2274`)
- Dedup compara IDs exatos: um ID diferente dos históricos entra; um ID já no histórico é suprimido intencionalmente. Reuso/colisão real de ID: [nao verificado]. (`bin/disjuntor.py:2177`, `bin/disjuntor.py:2178`, `bin/disjuntor.py:2180`)
- Campos legados contendo dicts da volta 2 ficam inválidos no leitor histórico; o escritor ignora esses dicts na dedup histórica e pode reobservá-los no campo novo. Existência desse estado em produção: [nao verificado]. (`bin/disjuntor.py:2177`, `bin/disjuntor.py:2206`)
- Escrita operacional permanece na porta: callback chama helper; `_com_estado` executa callback sob trava e persiste estado inteiro. Nenhum relógio novo aparece no helper: ele copia `agora` para `ts_observado`; fallback interno de relógio já existia. (`bin/disjuntor.py:351`, `bin/disjuntor.py:366`, `bin/disjuntor.py:371`, `bin/disjuntor.py:975`, `bin/disjuntor.py:990`, `bin/disjuntor.py:1054`, `bin/disjuntor.py:2182`)
- Os relógios dos hooks chegam pela borda PreToolUse e Stop. (`bin/disjuntor.py:1894`, `bin/disjuntor.py:1897`, `bin/hook_stop_fila1.py:1198`, `bin/hook_stop_fila1.py:1204`)

## 11/11 02:49 · sessao 1fd278970062 · - · - · origem=Ronald (tmux)

Os três mutantes mordem nas reproduções em memória; cada âncora aparece exatamente uma vez na cópia. A infraestrutura exige âncora única e considera falha do caso uma mordida. (`bin/tests/test_disjuntor.sh:1515`, `bin/tests/test_disjuntor.sh:1519`, `bin/tests/test_disjuntor.sh:1538`, `bin/tests/test_disjuntor.sh:1542`)

| Variante | Resultado observado em memória | Expectativa atingida |
|---|---|---|
| Controle | `(0,2)`, `(1,1)`, `(None,None)`, `(None,None)`; escritor mantém histórico e produz `['n1','n2']`, tempos `[100.0,200.0]` | Todas as expectativas do caso. (`bin/tests/test_disjuntor.sh:1483`, `bin/tests/test_disjuntor.sh:1485`, `bin/tests/test_disjuntor.sh:1486`, `bin/tests/test_disjuntor.sh:1487`, `bin/tests/test_disjuntor.sh:1494`) |
| `historico-reconta` | Escritor produz `['aa6','n1','n2']`, tempos `[100.0,100.0,200.0]` | Derruba a expectativa do escritor. (`bin/tests/test_disjuntor.sh:1491`, `bin/tests/test_disjuntor.sh:1494`, `bin/tests/test_disjuntor.sh:1538`) |
| `duplicata-reconta` | `(2,1)` em vez de `(1,1)` | Derruba a fixture duplicada. (`bin/tests/test_disjuntor.sh:1484`, `bin/tests/test_disjuntor.sh:1485`, `bin/tests/test_disjuntor.sh:1542`) |
| `forma-invalida-vira-zero` | Primeiro falha com `KeyError` no registro `{}`; isolando a fixture não lista, produz `(0,0)` | A primeira mordida acontece por exceção; a fixture seguinte também demonstraria o zero falso nominal. (`bin/tests/test_disjuntor.sh:1486`, `bin/tests/test_disjuntor.sh:1487`, `bin/tests/test_disjuntor.sh:1539`, `bin/disjuntor.py:2201`) |

O caso cobre histórico sem recontagem, duplicatas persistidas, duas chamadas do escritor, preservação do histórico e do primeiro tempo observado. Ele **não testa apresentação por `linha()`**, apesar do comentário afirmar isso: `_q` chama somente os dois contadores, e o trecho seguinte chama somente o escritor. (`bin/tests/test_disjuntor.sh:1480`, `bin/tests/test_disjuntor.sh:1482`, `bin/tests/test_disjuntor.sh:1491`)

Também não há nesse caso fixtures de `aid` vazio ou não string, JSON quebrado, raiz não objeto ou retorno antecipado do escritor com observações não lista. (`bin/tests/test_disjuntor.sh:1483`, `bin/tests/test_disjuntor.sh:1487`, `bin/tests/test_disjuntor.sh:1490`)

Execução integral da suíte shell: **[nao verificado]**. Ela cria, altera e remove arquivos, incompatível com a restrição desta revisão. (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:41`, `bin/tests/test_disjuntor.sh:1508`, `bin/tests/test_disjuntor.sh:1517`)

## 11/11 02:49 · sessao d65773cd15a5 · - · - · origem=Ronald (tmux)

- **Escritor operacional:** `_anota_sem_obra` é o único que cria/acrescenta `b3_sem_obra_obs`: `setdefault` em `bin/disjuntor.py:2174`, inclusão em `bin/disjuntor.py:2182`, chamada por `decide_b3` em `bin/disjuntor.py:990`; persistência pela `_com_estado` em `bin/disjuntor.py:1054`, sob `flock` em `bin/disjuntor.py:351` e serialização em `bin/disjuntor.py:371`.
- **Histórico:** nenhum escritor operacional específico remanescente foi encontrado em `bin/`; `_anota_sem_obra` apenas lê `b3_sem_obra` para dedup (`bin/disjuntor.py:2173`, `bin/disjuntor.py:2177`), e `_b3_sem_obra_hist` lê/conta seus IDs (`bin/disjuntor.py:2206`, `bin/disjuntor.py:2207`). A serialização genérica do estado continua preservando esse campo (`bin/disjuntor.py:371`).
- **Leitores de `obs`:** dedup do escritor (`bin/disjuntor.py:2178`) e `_b3_sem_obra`, que valida e conta IDs distintos (`bin/disjuntor.py:2200`, `bin/disjuntor.py:2201`). O nome `b3_sem_obra` na saída derivada de `ler` continua sendo **número de observações novas**, enquanto a chave persistida homônima contém o histórico; os acessos estão separados, sem despacho por tipo do elemento (`bin/disjuntor.py:2044`, `bin/disjuntor.py:2200`, `bin/disjuntor.py:2206`).
- **Propagação:** `gerar_estado.py` chama somente `ler` e `linha` (`bin/gerar_estado.py:280`, `bin/gerar_estado.py:281`); CLI idem (`bin/disjuntor.py:2345`). Stop captura relógio e chama `julga_b3`, sem manipular os campos (`bin/hook_stop_fila1.py:1198`, `bin/hook_stop_fila1.py:1204`); `julga_b3` delega a `decide_b3` (`bin/disjuntor.py:1075`).
- **Testes:** leitor numérico antigo (`bin/tests/test_disjuntor.sh:430`); leitor de apresentação (`bin/tests/test_disjuntor.sh:1368`); fixtures históricas/obs válidas e inválidas (`bin/tests/test_disjuntor.sh:1483`, `bin/tests/test_disjuntor.sh:1484`, `bin/tests/test_disjuntor.sh:1486`, `bin/tests/test_disjuntor.sh:1487`); fixture histórica e duas chamadas do escritor, com leitura dos IDs, timestamps e preservação histórica (`bin/tests/test_disjuntor.sh:1490`, `bin/tests/test_disjuntor.sh:1491`, `bin/tests/test_disjuntor.sh:1492`, `bin/tests/test_disjuntor.sh:1493`). Há também regravação genérica do estado inteiro (`bin/tests/test_disjuntor.sh:1094`, `bin/tests/test_disjuntor.sh:1098`). Caso principal, controle e três mutantes registrados (`bin/tests/test_disjuntor.sh:1498`, `bin/tests/test_disjuntor.sh:1537`, `bin/tests/test_disjuntor.sh:1538`, `bin/tests/test_disjuntor.sh:1542`).
- **B3 antigo permanece coerente por inspeção:** cria `s1`, aciona Stop e espera `1` (`bin/tests/test_disjuntor.sh:428`, `bin/tests/test_disjuntor.sh:429`, `bin/tests/test_disjuntor.sh:431`); scanner extrai `s1` do basename e o inclui em `sem_obra` (`bin/disjuntor.py:843`, `bin/disjuntor.py:845`), escritor registra em obs (`bin/disjuntor.py:2182`), contador retorna um ID (`bin/disjuntor.py:2201`). Execução integral: [nao verificado].
- O comentário antigo “contador == universo” continua impreciso, pois o histórico está fora desse número (`bin/tests/test_disjuntor.sh:427`, `bin/disjuntor.py:2200`, `bin/disjuntor.py:2206`).

## 11/11 02:42 · sessao 5dde95c07157 · FIO-E0-V4-E3 · construtor · origem=Code

**lei: PARE — C6(i).** O consumidor tem um caminho que pode carimbar perguntas já gravadas e fechar a disputa após a absolvição da célula. Quantidade afetada nos dias X1: **[não verificado]**. [reconciliador.py:595](/home/ronald/wt-codex-fio-e0-v4-e3-022700/app/chamados/reconciliador.py:595), [reconciliador.py:610](/home/ronald/wt-codex-fio-e0-v4-e3-022700/app/chamados/reconciliador.py:610).

A cura foi preparada usando `VEREDITOS_ACUSAM` e a fonte `sem_adesao` na impressão. O teste de preservação agora alcança também o reconciliador. [cartorio.py:1248](/home/ronald/wt-codex-fio-e0-v4-e3-022700/app/ponto/services/cartorio.py:1248), [teste C6:255](/home/ronald/wt-codex-fio-e0-v4-e3-022700/app/ponto/tests/test_x1_folga_sem_adesao.py:255). [nao verificado]

Inventário, censo, leitores, **11 testes ORM**, dois mutantes e comandos estão no [relato](/home/ronald/wt-codex-fio-e0-v4-e3-022700/app/docs/relato_fio_e0_v4_e3_x1.txt:1). RED/GREEN, morte dos mutantes, ruff e suite permanecem **[não verificado]**: banco/rede proibidos nesta volta e dependências ausentes. [nao verificado]

## 11/11 02:42 · sessao 7b6d61289e98 · VERIFICADOR-EM-FABLE · verificador · origem=Code

**BLOQUEIA o delta:** a correção aritmética funciona para o estado válido, mas introduz discriminação histórica pelo tipo do elemento e permite zero falso para conteúdo estruturalmente inválido. (`bin/disjuntor.py:2172`, `bin/disjuntor.py:2182`, `bin/disjuntor.py:2190`)

1. **FILA — o mecanismo cumulativo da volta 1 foi corrigido, com ressalva temporal.** Strings persistidas deixam de entrar no “esperado 0”; dicionários entram. O histórico positivo aparece separado na linha, sem remover as strings da lista. Assim, `["aa6","ac9"]` resulta em contador `0`, histórico `2`. (`bin/disjuntor.py:2176`, `bin/disjuntor.py:2182`, `bin/disjuntor.py:2190`, `bin/disjuntor.py:2259`, `bin/tests/test_disjuntor.sh:1483`)

   Isso distingue **registros já persistidos de registros acrescentados pelo novo escritor**, não a data real da chamada: um transcript antigo ainda não registrado receberá dicionário e contará como novo. Não existe comparação temporal com um marco da cura nesse caminho. Os seis históricos e suas obras efetivamente identificadas no RELATO: **[nao verificado]**. (`bin/disjuntor.py:976`, `bin/disjuntor.py:990`, `bin/disjuntor.py:2172`, `bin/disjuntor.py:2176`, `bin/disjuntor.py:2182`)

2. **FILA — escritores e leitores encontrados em `bin/`; nenhuma suposição residual de string nas operações sobre a lista.**
   
   - **Escritor operacional:** `decide_b3._fn` chama `_anota_sem_obra`; este lê os IDs existentes e acrescenta dicionários. A persistência continua pela mesma porta `_com_estado`, sob `flock`, com serialização do estado inteiro. (`bin/disjuntor.py:990`, `bin/disjuntor.py:2172`, `bin/disjuntor.py:2176`, `bin/disjuntor.py:351`, `bin/disjuntor.py:366`, `bin/disjuntor.py:371`)
   - **Leitores dos elementos:** o próprio escritor, para deduplicação; `_b3_sem_obra`, para contar dicionários; `_b3_sem_obra_hist`, para contar strings. O `in` opera sobre `vistos`, já normalizado para IDs. Esses sítios não fazem `join` nem `sort` dos elementos mistos. (`bin/disjuntor.py:2172`, `bin/disjuntor.py:2174`, `bin/disjuntor.py:2182`, `bin/disjuntor.py:2190`)
   - **Propagação:** `ler` entrega dois números; `linha` os formata; `gerar_estado.py` chama essas duas funções, sem manipular a lista; a CLI `--linha` faz o mesmo. (`bin/disjuntor.py:2044`, `bin/disjuntor.py:2259`, `bin/gerar_estado.py:280`, `bin/gerar_estado.py:281`, `bin/disjuntor.py:2330`)
   - **Testes:** o B3 antigo lê o número; `_xlinha` lê a apresentação completa; o caso novo escreve a fixture histórica, chama ambos os contadores e manipula uma lista pelo escritor. Ele está no conjunto principal, no controle sem mutação e no mutante. (`bin/tests/test_disjuntor.sh:430`, `bin/tests/test_disjuntor.sh:1368`, `bin/tests/test_disjuntor.sh:1481`, `bin/tests/test_disjuntor.sh:1482`, `bin/tests/test_disjuntor.sh:1486`, `bin/tests/test_disjuntor.sh:1492`, `bin/tests/test_disjuntor.sh:1531`, `bin/tests/test_disjuntor.sh:1532`)
   - **JSON genérico:** `ler_estado` apenas decodifica; `_com_estado` serializa; o teste que relê e regrava o estado inteiro preserva a lista sem interpretar seus elementos. Não há conversão implícita para strings nesses caminhos. (`bin/disjuntor.py:393`, `bin/disjuntor.py:371`, `bin/tests/test_disjuntor.sh:1094`, `bin/tests/test_disjuntor.sh:1098`)

   Ficaram comentários com “contador == universo”, agora imprecisos para o contador isolado, que exclui o histórico. (`bin/disjuntor.py:646`, `bin/tests/test_disjuntor.sh:427`, `bin/disjuntor.py:2182`)

3. **FILA — `ts` é o instante da primeira observação registrada, pelo relógio civil do processo.** No PreToolUse, `time.time()` é capturado antes da avaliação e passado até `decide_b3`; no Stop, vem de `_disjuntor_contido`. O escritor copia esse valor, sem consultar outro relógio. Portanto, não representa o horário original do transcript nem o marco da cura. (`bin/disjuntor.py:1894`, `bin/disjuntor.py:1897`, `bin/disjuntor.py:1846`, `bin/disjuntor.py:1777`, `bin/hook_stop_fila1.py:1198`, `bin/hook_stop_fila1.py:1204`, `bin/disjuntor.py:2176`)

   Existe o fallback anterior `time.time()` dentro de `decide_b3` quando `agora=None`; a porta Codex chega sem transcript, portanto sem novos IDs para acrescentar por essa varredura. Os leitores novos ignoram `ts`: não há consumidor atribuindo outro significado temporal a ele. (`bin/disjuntor.py:975`, `bin/disjuntor.py:1137`, `bin/disjuntor.py:836`, `bin/disjuntor.py:2182`, `bin/disjuntor.py:2190`)

4. **FILA — chamadas repetidas não acrescentam novamente o mesmo ID; duplicatas preexistentes não são saneadas.** O conjunto `vistos` reúne IDs de strings e dicionários e é atualizado a cada inclusão; a transição persistente ocorre sob a trava. Duas chamadas preservam o primeiro `ts`. (`bin/disjuntor.py:2172`, `bin/disjuntor.py:2175`, `bin/disjuntor.py:2176`, `bin/disjuntor.py:351`, `bin/disjuntor.py:366`)

   Se já houver `"n1"` e `{"aid":"n1","ts":100}`, permanece um em cada contador; dois dicionários do mesmo ID contam `2`. O leitor conta elementos, não IDs únicos. O escritor novo não produz essas duplicatas a partir de estado válido. (`bin/disjuntor.py:2174`, `bin/disjuntor.py:2182`, `bin/disjuntor.py:2190`)

   **BLOQUEIA — “ilegível nunca vira 0” só vale para parte das falhas.** JSON quebrado ou raiz não objeto gera `None`; porém `{"b3_sem_obra":[42]}` retorna `0` nos dois contadores, e `[{}]` conta como uma ocorrência nova. Falta validar coleção e registros antes de filtrar tipos. Além disso, histórico `None` desaparece da apresentação, pois a condição de exibição testa apenas valor verdadeiro. (`bin/disjuntor.py:396`, `bin/disjuntor.py:398`, `bin/disjuntor.py:2182`, `bin/disjuntor.py:2190`, `bin/disjuntor.py:2259`)

5. **FILA — o mutante morde de verdade, dentro do alcance declarado.** A âncora retira somente `if isinstance(e, dict)` do contador. Reproduzindo essa substituição em memória, a fixture passa de `0 2` para `2 2`, derrubando a primeira expectativa do caso. O controle sem mutação está registrado. (`bin/tests/test_disjuntor.sh:1532`, `bin/disjuntor.py:2182`, `bin/tests/test_disjuntor.sh:1483`, `bin/tests/test_disjuntor.sh:1531`)

   O caso também verifica histórico revisto e repetição do novo ID **na mesma chamada**. Não verifica segunda chamada, `ts`, apresentação por `linha` nem estrutura inválida. (`bin/tests/test_disjuntor.sh:1482`, `bin/tests/test_disjuntor.sh:1486`, `bin/tests/test_disjuntor.sh:1487`)

   O B3 antigo continua coerente: cria transcript `s1`, aciona Stop e espera o número `1`; o novo escritor registra esse ID como dicionário, que o contador inclui. Execução integral desse caso: **[nao verificado]**. (`bin/tests/test_disjuntor.sh:428`, `bin/tests/test_disjuntor.sh:429`, `bin/tests/test_disjuntor.sh:431`, `bin/disjuntor.py:2176`, `bin/disjuntor.py:2182`)

6. **BLOQUEIA — há dois significados implícitos no mesmo campo persistido.** O tipo do elemento passou a decidir sua época: string significa histórico; dicionário significa posterior à cura. Isso está explicitamente documentado e governa os dois leitores, mesmo sem divergência entre eles. (`bin/disjuntor.py:2169`, `bin/disjuntor.py:2170`, `bin/disjuntor.py:2182`, `bin/disjuntor.py:2190`)

   **Alternativa:** separar histórico e novas ocorrências em campos explícitos, migrados pela mesma `_com_estado`; deduplicar IDs entre ambos; validar os formatos; nomear o instante `ts_observado`; exibir falha de leitura histórica como “nao sei”. Isso substitui a discriminação atual por tipo e corrige o silêncio na apresentação. (`bin/disjuntor.py:335`, `bin/disjuntor.py:2172`, `bin/disjuntor.py:2176`, `bin/disjuntor.py:2259`)

   **FILA — as outras classes não reaparecem como defeito novo demonstrado:** a escrita operacional permanece na mesma porta, o escritor recebe o relógio e o mutante derruba sua expectativa específica. (`bin/disjuntor.py:990`, `bin/disjuntor.py:1054`, `bin/disjuntor.py:2176`, `bin/tests/test_disjuntor.sh:1483`, `bin/tests/test_disjuntor.sh:1532`)

A suíte shell integral permanece **[nao verificado]**: cria e remove arquivos, incompatíveis com a restrição desta revisão. As reproduções descritas foram em memória e não comprovam produção. (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:41`, `bin/tests/test_disjuntor.sh:51`)

## 11/11 02:32 · sessao 79a6f5449c81 · FIO-E0-V4-E3 · verificador · origem=Code

**PARE na pré-verificação: c8b tem bloqueios de execução, horizonte temporal, paridade da alimentação e eficácia do selo B5-j.** As formas em memória passam, mas não cobrem esses caminhos. (app/chamados/services/tarefa_do_dia.py:245,313,467; app/chamados/juizes.py:1346; app/escala/servico_jornada.py:376; app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:118)

1. **BLOQUEIA — há leitor de `viva_c6` sem alimentação correspondente.**

   Em dia de trabalho com algumas instâncias, uma pergunta de motivo conhecido cujo marco desapareceu entra na exposição administrativa em `_tarefa_pura`. Porém `_ler_vida` só seleciona perguntas de dia sem trabalho, posição acesa ou motivo fora do mapa. A posição ausente não entra; o atributo nunca é atribuído, e `expor_admin` o acessa diretamente. Reproduzi em memória: `AttributeError: ... has no attribute 'viva_c6'`. (app/chamados/services/tarefa_do_dia.py:245,313,465,467,480)

   **RED necessário:** grade atual sem intervalo, pergunta antiga de intervalo e demais marcos presentes; chamar tarefa avulsa e lote pelo construtor real. O teste atual de integração contorna esse construtor com `__new__`. (app/chamados/tests/test_e3_c8b_sem_banco.py:227)

2. **BLOQUEIA — o C6 atual pode apagar uma decisão exigível no `agora` passado.**

   A carga corta decisões e passivo por `ato_em <= agora`, mas passa ao C6 a pergunta com os carimbos atuais. A primeira guarda do C6 mata por qualquer `validada_em`, inclusive posterior ao instante consultado. (app/chamados/services/tarefa_do_dia.py:385,387,394,478; app/chamados/juizes.py:1346)

   Contraexemplo: RESPOSTA textual em T1; VALIDACAO em T2; consulta com `agora=T1`. A sequência carregada ainda exige `validar/admin`, mas `viva_c6=False` impede a exposição. A porta real grava carimbo e decisão VALIDACAO no mesmo instante, tornando o cenário alcançável pelos escritores existentes. Reproduzi a supressão em memória. (app/chamados/services/tarefa_do_dia.py:133,136,245; app/chamados/services/validacao.py:134,139)

   **RED necessário:** consultar o dict inteiro em T1 antes e depois da validação real em T2, mantendo o mesmo `agora`. O contrato já declara o corte temporal dos fatos datados da pergunta. (app/chamados/services/tarefa_do_dia.py:348)

3. **BLOQUEIA — relógio fora da borda no caminho novo.**

   A tarefa injeta `hoje=localdate(self.agora)`, mas CF compara o fim do turno com `_tz.now()`. Portanto a alimentação nova não depende exclusivamente do relógio recebido pela tarefa. Esse comportamento já existia no CF avulso; c8b passa a alcançá-lo pela nova leitura do C6. (app/chamados/services/tarefa_do_dia.py:478,480; app/chamados/juizes.py:1414; app/escala/servico_jornada.py:375,376)

   Com CF real, vínculo diurno 08–17 e nenhuma ausência/batida, mudar somente o relógio ambiente de 12h para 20h mudou o resultado de ausência de falta para `saida_sem_entrada`. O apoio fixa `now` em `AGORA`, ocultando essa diferença; impacto nos dias produtivos anexados: **[nao verificado]**. (app/chamados/tests/e3_c8b_apoio.py:89,91; app/escala/servico_jornada.py:376,454)

   **FILA adicional:** esse teto chama `fim_turno` sem `ec/celulas`, lendo o template; a contagem posterior lê os marcos congelados da célula. É uma divergência antiga de fonte dentro do CF, preservada pelos dois caminhos. (app/escala/servico_jornada.py:375,427; app/escala/servico_jornada.py:68,71,89)

4. **BLOQUEIA a afirmação irrestrita “carga = avulso”: existe contraexemplo de vínculo.**

   Se a célula de um colaborador aponta para uma geradora de outro colaborador, com vigência cobrindo o dia, `escala_vigente` entrega essa geradora diretamente ao juiz. A carga usa somente os vínculos do colaborador solicitado e devolve `None`. Reproduzi os corpos reais em memória; a FK não impõe igualdade dos colaboradores. Ocorrência produtiva e escritor que tenha produzido essa inconsistência: **[nao verificado]**. (app/escala/servico_jornada.py:55,57; app/chamados/services/carga_pergunta_viva.py:157; app/ponto/turnos.py:2990; app/escala/alimentacao.py:279; app/escala/models.py:1692)

   O teste de paridade substitui justamente `escala_vigente` por um alimentador que chama diretamente `vinculo_do_dia`; assim, não confronta os dois carregadores reais nesse caso. A correção do cadastro inconsistente pode ter fila própria, mas a igualdade universal não está demonstrada. (app/chamados/tests/e3_c8b_apoio.py:97,101)

   Nos demais recortes, a inspeção encontrou reutilização dos corpos e filtros abaixo; paridade ORM integral: **[nao verificado]**.

   | Insumo | Comparação carga × avulso |
   |---|---|
   | Turnos | Mesmo `_turnos_do_colab_puro`, executado novamente para D..D ou D−1..D+1; não recorta turnos previamente pareados na janela larga. (app/chamados/services/carga_pergunta_viva.py:163,180; app/ponto/turnos.py:1918) |
   | Janela | Mesmo pad `[ini−2, fim+1]`; carga recorta batidas por intervalo civil `[00:00 pad_ini, 00:00 pad_fim+1)`. O pareador continua decidindo `data_turno`. (app/ponto/turnos.py:1834,1847; app/chamados/services/carga_pergunta_viva.py:172,175) |
   | Universo de batidas | Carga usa `batidas_apuraveis`; avulso filtra `retratada_em__isnull=True`. A última batida também é apurável. (app/ponto/turnos.py:1863,1894,2999,3017) |
   | Ausências | Mesmo efeito `cobranca`, excluindo rejeitadas; mesmo recorte inclusivo de cobertura, preservando sobrepostas e a data inicial de range malformado. Não escolhe apenas a vencedora. (app/chamados/services/carga_pergunta_viva.py:98,105; app/ponto/turnos.py:2511,2531,2757) |
   | Células/grade/DNA | Células vêm da porta comum; grade e DNA recebem os mapas carregados. Grade exige a lista inteira de vínculos. (app/ponto/turnos.py:2980; app/chamados/services/carga_pergunta_viva.py:149,178; app/escala/services/leitor_celula.py:350,355) |
   | Testemunhas | Plantadas continuam filtradas por origem e retratação; prova sem origem reutiliza a janela inclusiva de ±20 minutos e a ordenação por timestamp. (app/chamados/services/carga_pergunta_viva.py:140,184,187; app/chamados/juizes.py:235,244,744) |
   | Ordenação | Não encontrei outro contraexemplo determinístico por empate: há unicidade de batida por colaborador/timestamp e de vínculo por colaborador/data inicial. (app/ponto/models.py:105; app/escala/models.py:1003) |
   | Exportações | O chamador novo anota a vigência do conjunto e separa cada tripla antes de chamar K6. (app/chamados/services/tarefa_do_dia.py:434,437,438) |

5. **BLOQUEIA — B5-j perdeu a mordida após o split.**

   O universo produtivo continua pela porta, mas o detector procura consultas somente na função que chama `parear_turnos` ou `_turnos_do_colab_puro`. Agora `turnos` chama o corpo puro, enquanto a consulta mora em `_carregar_batidas`. O comentário afirmando que o tripwire ainda vê esse universo ficou incorreto. (app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:63,118,125; app/ponto/turnos.py:3011,3015,3027,3036)

   Mutação somente em memória: substituir a porta desse helper por `Batida.objects`, sem filtro de retratação. O detector retornou exatamente os mesmos universos para original e mutante, ambos com `crus=[]`. O selo precisa observar a chamada ao carregador extraído e matar essa mutação. (app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:103,118; app/ponto/turnos.py:3017)

6. **FILA — K6 aceita alimentação capaz de mudar o veredito para outra tripla.**

   `exportacoes` pula a seleção por empresa/mês/ano e a anotação. Uma linha vigente de outra tripla pode fazer K6 devolver `True` — reproduzido em memória — ou devolver o registro alheio com `registro=True`. Uma lista incompleta também pode produzir falso negativo. (app/folha/export.py:583,586,587)

   Linhas ORM não anotadas consultam a propriedade `e_vigente` individualmente; isso pode reintroduzir N consultas. Não implica automaticamente veredito errado quando a tripla e o conjunto estão corretos. (app/folha/models.py:121,123,134,200)

   O único chamador produtivo novo encontrado fornece linhas anotadas e da tripla correta. Portanto há fragilidade do contrato do juiz; uso produtivo incorreto desse kwarg: **[nao verificado]**. (app/chamados/services/tarefa_do_dia.py:437,438)

7. **Cache, residual A/S e classes de reprovação.**

   `viva_c6` é um cache transitório do retorno do C6, com escritor produtivo em `_ler_vida` e leitores em `expor_admin` e na guarda residual; não constitui um segundo predicado de vida. O caminho com carga também evita ler/escrever o memo avulso. (app/chamados/services/tarefa_do_dia.py:480,245,326; app/chamados/juizes.py:867)

   A carga não tem invalidação depois de `_vida_carregada=True`. No consumidor produtivo literal encontrado, o command de sombra apenas percorre dias e lê resultados; não identifiquei escrita entre a alimentação e esses leitores. Reutilização após escrita ou por leitor dinâmico: **[nao verificado]**. (app/chamados/services/tarefa_do_dia.py:445,481; app/chamados/management/commands/diff_e3_sombra.py:115,125)

   O residual A/S lê literalmente o veredito lavrado `fato_em_ausencia`, exige dia sem trabalho, competência não entregue e nenhuma pergunta viva elegível. Emite `validar/admin` do dia, sem criar nem ressuscitar pergunta. Esse ramo não alcança `nunca_bateu`, `indefinida` ou dia de trabalho que deva continuar com o colaborador. (app/chamados/services/tarefa_do_dia.py:230,234,325,330)

   A classificação da contradição continua no supra-juiz: fato em dia encerrado com cobertura aprovada; parcial sem período recebe outro código. O mapa converte `FATO_EM_AUSENCIA` em `fato_em_ausencia`, e a porta lavra o resultado. Não há segundo classificador da contradição na tarefa. (app/ponto/supra_juiz.py:379,382,400,404; app/escala/models.py:1615,1619,1624; app/ponto/portas/celula.py:923,1035)

   **ESMERIL:** há relógio fora da borda e selo sem mordida, conforme os bloqueios acima. Não identifiquei segundo escritor persistente novo nem novo significado de `data_evento`: a tarefa mantém a identidade pelo dia do turno e escreve as novas ações em dicionários. O teto pelo template permanece uma fonte paralela antiga. (app/chamados/services/tarefa_do_dia.py:218,236,252,331; app/escala/servico_jornada.py:375,427)

8. **Os GREEN/mutantes não certificam os escritores e leitores ORM reais.**

   Reexecutei os **32 testes em memória: RC=0, OK**; o agregado dos **14 mutantes: RC=0**, exigindo FAIL por assertion e nenhum ERROR. Os executores são os indicados na entrega. (app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:115; app/chamados/tests/test_e3_c8b_sem_banco.py:47; app/chamados/tests/e3_c8b_mutantes.py:17,87,105)

   C6, CF e derivação do slot executam corpos extraídos do fonte. Porém são substituídos: `CargaDaJanela`, pareamento avulso e puro, grade, managers/Q, tranca e relógio. A integração da tarefa usa `__new__`; AS/B puros recebem vida e complementos prontos. Assim, não exercitam os escritores reais formando os fatos que a cura consome. (app/chamados/tests/e3_c8b_apoio.py:63,89,102,112,121,123,132; app/chamados/tests/test_e3_c8b_sem_banco.py:41,227; app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:105,119)

   O mutante de janela larga morre pelo stub `Janela.turnos`, que levanta `AssertionError`; ele protege o caminho escolhido, sem provar a geometria real do pareamento. (app/chamados/tests/e3_c8b_apoio.py:75; app/chamados/tests/e3_c8b_mutantes.py:65)

   **RED ORM ainda necessários para certificar esta cura:**

   - **Execução/horizonte:** marco removido sem `viva_c6`; mesma consulta T1 antes/depois da validação em T2; mesmo `agora` com relógio ambiente diferente. (app/chamados/services/tarefa_do_dia.py:313,467; app/chamados/juizes.py:1346; app/escala/servico_jornada.py:376)
   - **B/A/S pelas portas reais:** resposta e decisão E1 reais; ausência, batidas, julgamento e lavra reais; pergunta morta ou inexistente; competência exportada com nenhuma ação nova. Os testes atuais recebem essas condições prontas. (app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:105,119; app/chamados/tests/test_e3_c8b_sem_banco.py:35,119)
   - **Paridade completa:** construtor real, dict inteiro avulso/lote, N=1/N=4 e contagem SQL incluindo relações lazy; o apoio atual substitui o pai e contorna o construtor. (app/chamados/tests/e3_c8b_apoio.py:63; app/chamados/tests/test_e3_c8b_sem_banco.py:227,257)
   - **Geometria/fontes:** D..D e D−1..D+1, bordas do pad, noturno, troca de vínculo, retratadas e geradora de outro colaborador; ambos os pareadores atuais devolvem a mesma lista sintética. (app/chamados/tests/e3_c8b_apoio.py:121,123; app/escala/servico_jornada.py:55)
   - **Ausências reais:** estados de cobrança, catálogo de parcialidade, sobreposição e range malformado; hoje Q/managers e parcialidade são doubles. (app/chamados/tests/e3_c8b_apoio.py:17,39,120)
   - **K6 carregado:** outra tripla, lista incompleta/não anotada, empate, substituição e vigente invalidada sem substituta; o teste novo usa `Registro/Query` sintéticos. (app/chamados/tests/test_e3_c8b_sem_banco.py:437,459,473)
   - **Frescor após escrita:** resposta, validação, materialização/retratação, lavra, tranca/toque e exportação; comparar carga nova, carga antiga e memo avulso. O teste novo altera apenas insumo em memória. (app/chamados/tests/test_e3_c8b_sem_banco.py:290,298)

**Quem escreve e quem lê no caminho tocado:**

| Campo/valor | Escritor/autoridade | Leitor |
|---|---|---|
| `veredito`, `veredito_via`, `ata` | Julgamento e porta de lavra. (app/ponto/supra_juiz.py:404; app/ponto/portas/celula.py:1033,1035,1036) | Grade, C6 e residual. (app/escala/services/leitor_celula.py:424; app/chamados/juizes.py:1359; app/chamados/services/tarefa_do_dia.py:325) |
| `trabalha`, `dna`, `escala_geradora` | Geração/edição pela porta da célula. (app/ponto/portas/celula.py:259,272,603) | Vínculo, marcos, CF e insumo de dia sem trabalho. (app/escala/alimentacao.py:269; app/escala/servico_jornada.py:297,427; app/escala/services/leitor_celula.py:428) |
| Identidade: `colaborador`, `instancia_marco`, `conversa`, `motivo`, `data_evento` | Nascimento; mudança de dia pela porta declarada de rechaveamento. (app/chamados/models.py:2427,2825; app/chamados/veredito_lavrado.py:47) | Seleção da carga, posição e identidade da tarefa/slot. (app/chamados/services/tarefa_do_dia.py:107,230,236,371; app/chamados/services/materializacao.py:280) |
| `resposta_colab`, `respondida_em` | Escritores de resposta e carimbo com resposta. (app/chamados/services/disputa_emissao.py:1467,1652; app/chamados/models.py:3132) | C6, slot e projeção E1. (app/chamados/juizes.py:1362; app/chamados/services/materializacao.py:279; app/chamados/services/tarefa_do_dia.py:394) |
| `validada_em` | Porta de validação. (app/chamados/services/validacao.py:134,137) | Primeira guarda C6 e passivo E1. (app/chamados/juizes.py:1346; app/chamados/services/tarefa_do_dia.py:394) |
| `materializada_em`, `via_resolucao` | `_escrever_carimbo`, persistido pelas portas de carimbo. (app/chamados/models.py:3097,3152,3190) | C6, identidade e testemunho de materialização. (app/chamados/juizes.py:1357,1383,1421; app/chamados/services/tarefa_do_dia.py:230) |
| `DecisaoPergunta`: ato, instante, texto e absorção | `registrar`; validação registra no mesmo ato do carimbo. (app/chamados/models.py:3943; app/chamados/services/validacao.py:139) | Carga ordena/corta; `_pela_decisao` usa a última e lê o estado da absorção. (app/chamados/services/tarefa_do_dia.py:132,153,385) |
| Batida: colaborador, timestamp, tipo, origem da pergunta e retratação | Registro/anotação e retratação pela porta. (app/ponto/registro_batida.py:94,205,267,274) | Pareador, plantadas, janela do slot e efeitos. (app/chamados/services/carga_pergunta_viva.py:140,175,184; app/chamados/services/tarefa_do_dia.py:406) |
| Ausência: tipo, período, minutos e status | Criação, edição e decisão da ausência. (app/ponto/services/ausencia.py:130,465,897) | Filtro de efeito/cobertura, CF e E1. (app/ponto/turnos.py:2511,2531; app/escala/servico_jornada.py:338; app/chamados/services/tarefa_do_dia.py:153) |
| Vínculo: proprietário, atividade, vigência e tipo | Escritores completos de cadastro: **[nao verificado]**. | Carregador e juiz único de vínculo; marcos. (app/ponto/turnos.py:2990; app/escala/alimentacao.py:269,285; app/escala/servico_jornada.py:69) |
| `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `veredito_lavrado.lavrar`; backfill declarado como exceção. (app/chamados/veredito_lavrado.py:58,254) | `esta_stale` e carga da tarefa. (app/chamados/veredito_lavrado.py:213; app/chamados/services/tarefa_do_dia.py:399) |
| Tranca/toque do admin | Escritores completos: **[nao verificado]**. | Autoridade `dias_trancados`; carga repassa o resultado ao C6 e à tarefa. (app/chamados/juizes.py:987; app/chamados/services/tarefa_do_dia.py:418; app/chamados/services/carga_pergunta_viva.py:143) |
| Exportação, tripla e geração | `ExportacaoDominio.registrar`. (app/folha/models.py:174,182) | Anotação de vigência e K6. (app/folha/models.py:209,220; app/folha/export.py:583) |
| `invalidada_em` | `invalidar`, com persistência e trilha. (app/folha/models.py:143,158,162) | K6. (app/folha/export.py:586) |
| `vigente`, `_vigente_anotado` | `anotar_vigencia`, somente em memória. (app/folha/models.py:224,225) | `e_vigente`, `so_vigentes`, K6. (app/folha/models.py:123,200; app/folha/export.py:583) |
| Trilha `retratar_pergunta_obsoleta` | Escritor atual correspondente: **[nao verificado]**. | Carga e `_retratada_com_trilha`. (app/chamados/services/carga_pergunta_viva.py:107; app/chamados/juizes.py:282,292) |
| `viva_c6`, `_vida_carregada`, `_exportadas` | Alimentação transitória pelo C6/K6. (app/chamados/services/tarefa_do_dia.py:472,480,481) | Guardas de alimentação, exposição e residual. (app/chamados/services/tarefa_do_dia.py:445,245,325) |
| Caches de células, vínculos, ausências, slots, testemunhas e turnos | Carregadores e auxiliares reais, em memória. (app/ponto/turnos.py:2980,2990,3016; app/chamados/services/carga_pergunta_viva.py:88,105,115,140,180) | Métodos `ausencias_do_dia/grade/vinculo/turnos/plantadas/batida_na_janela/retratada`. (app/chamados/services/carga_pergunta_viva.py:146,149,157,163,184,187,191) |
| Saída: instâncias, ações, pergunta, razão, incerteza, rota, fila, verbos e fio | `_tarefa_pura` e `_portas_da_tarefa`, em memória. (app/chamados/services/tarefa_do_dia.py:182,249,252,331,534,542) | Projeção externa de sombra; descarta saída com `rota=None`. (app/chamados/management/commands/diff_e3_sombra.py:27,44,125) |

O command atual não mede o diff integral base × cura e torna invisível nessa projeção o novo `admin/porta=None`. Quantidade de mudanças reais entre os 232 dias, K6/E1/FK dos casos e compatibilidade ORM integral permanecem **[nao verificado]**. (app/chamados/management/commands/diff_e3_sombra.py:27,44,124)

## 11/11 02:31 · sessao bb320e41c3f5 · RAIZ-DA-DISPUTA · leitura · origem=Code

**O main tem três sítios persistentes de destino, distribuídos por duas funções; a eleição do destino ocorre também nos chamadores.** Nascimento e reuso escrevem em `_nascer`; reapontamento escreve em `reapontar_resolvedora`. O selo existente permite os dois **arquivos**, portanto não demonstra função escritora única. (`app/chamados/models.py:3014`, `:3017`, `:2982`, `:3006`; `app/chamados/services/fio.py:925`, `:927`; `app/core/tests/test_contract_escritores_estado.py:61`, `:182`.)

**Há uma distinção essencial entre main e c21:** no main, o núcleo pode confirmar a DS antes da geração das perguntas; o protocolo de ensaio, rollback e replay conservado pertence ao delta fornecido. (`app/chamados/services/disputa_emissao.py:587`, `:603`, `:670`, `:682`; materialização da raia anexada: [nao verificado].)

Uso estas abreviações de arquivo nas tabelas:

| Sigla | Arquivo |
|---|---|
| M | [app/chamados/models.py](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/models.py) |
| E | [app/chamados/services/disputa_emissao.py](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/services/disputa_emissao.py) |
| F | [app/chamados/services/fio.py](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/services/fio.py) |
| D | `app/chamados/services/declaracao_texto.py` |
| J | `app/chamados/juizes.py` |
| V | `app/chamados/services/validacao.py` |
| R | `app/chamados/reconciliador.py` |
| A | `app/chamados/services/acoes_disputa.py` |
| L | `app/chamados/services/limbo_folgas.py` |
| MT | `app/chamados/services/materializacao.py` |
| T/nome | `app/chamados/tests/nome` |

As provas abaixo são referências a testes inspecionados; resultado de execução nesta leitura: **[nao verificado]**. Identidade criptográfica da cópia com HEAD/PROD, frequência diária e quantidade atual de casos: **[nao verificado]**. [nao verificado]

**F0 — nomes reais e unidade do censo**

Não há `DisputaSupervisao.status`: aberta significa `fechada_em is None`. Na pergunta, `estado` também é propriedade calculada, não coluna de estado. (`M:1757`, `M:1823`, `M:3033`.)

Há três relações distintas:

| Relação | Significado e política |
|---|---|
| `DisputaSupervisao.chamado` | Chamado-pai; FK nullable, `SET_NULL`. (`M:1749`.) |
| `PerguntaDisputa.disputa` | Dona da pergunta; FK com `CASCADE`. (`M:2416`.) |
| `PerguntaDisputa.chamado_destino` | Destino/resolvedora; FK nullable, `SET_NULL`. **É o campo real de destino da pergunta; não há outro campo `destino`.** (`M:3385`.) |

**Script Python de leitura**

Este enumerador produz posições candidatas, delegações e uma fila explícita de revisão de operações genéricas. A classificação persistência/memória, as eleições e as cadeias estão nas tabelas seguintes; não são inferidas apenas pela presença de um nome no AST. A limitação de inferência está também documentada no censo existente. (`app/core/censo_escritas.py:15`, `:38`, `:43`.)

```python
import ast
from pathlib import Path

FIELDS = {
    "chamado_destino", "chamado_destino_id",
    "disputa", "disputa_id", "fechada_em", "motivo_fechamento",
}
OPS = {
    "create", "get_or_create", "update_or_create",
    "update", "bulk_create", "bulk_update", "save",
}

for path in sorted(Path("app").rglob("*.py")):
    if any(p in {"tests", "__pycache__", "node_modules", ".git"}
           for p in path.parts):
        continue
    if path.name == "tests.py" or path.name.startswith(("test_", "tests_")):
        continue

    source = path.read_text(encoding="utf8")
    tree = ast.parse(source, filename=str(path))
    parents = {
        child: node
        for node in ast.walk(tree)
        for child in ast.iter_child_nodes(node)
    }
    cache = {}

    def owners(node):
        result = []
        while node in parents:
            node = parents[node]
            if isinstance(node, (ast.ClassDef, ast.FunctionDef,
                                 ast.AsyncFunctionDef)):
                result.append(node)
        return result[::-1]

    def terminal(node):
        return node.attr if isinstance(node, ast.Attribute) \
            else getattr(node, "id", "")

    def strings(node):
        return {
            x.value for x in ast.walk(node)
            if isinstance(x, ast.Constant) and isinstance(x.value, str)
        }

    def emit(node, kind):
        scope = ".".join(x.name for x in owners(node))
        print(f"{path}:{node.lineno}|{scope}|{kind}")

    nodes = sorted(ast.walk(tree), key=lambda n: (
        getattr(n, "lineno", 0), getattr(n, "col_offset", 0)))

    for node in nodes:
        if (isinstance(node, ast.Attribute)
                and isinstance(node.ctx, ast.Store)
                and node.attr in FIELDS):
            emit(node, "ATR " + node.attr)

        if not isinstance(node, ast.Call):
            continue

        end = terminal(node.func)
        direct = {k.arg for k in node.keywords} & FIELDS
        own = owners(node)
        fn = next((x for x in reversed(own)
                   if isinstance(x, (ast.FunctionDef,
                                     ast.AsyncFunctionDef))), None)

        if fn not in cache:
            subtree = list(ast.walk(fn or tree))
            aliases = {}
            for x in subtree:
                if isinstance(x, ast.ImportFrom):
                    aliases.update({
                        a.asname or a.name: a.name for a in x.names
                    })
            touched = {
                ast.unparse(x.value) for x in subtree
                if isinstance(x, ast.Attribute)
                and isinstance(x.ctx, ast.Store) and x.attr in FIELDS
            }
            constructed = {
                y.id
                for x in subtree
                if isinstance(x, ast.Assign)
                and isinstance(x.value, ast.Call)
                and {k.arg for k in x.value.keywords} & FIELDS
                for y in x.targets if isinstance(y, ast.Name)
            }
            cache[fn] = aliases, touched, constructed

        aliases, touched, constructed = cache[fn]
        root = ast.unparse(node.func).split(".")[0]
        root = aliases.get(root, root)

        if direct and end in OPS | {
            "abrir", "abrir_conversa", "_sob_contrato", "cls"
        }:
            emit(node, "KW " + ",".join(sorted(direct)))

        if (end in {"create", "get_or_create", "update_or_create"}
                and root == "DisputaSupervisao"):
            emit(node, "DEFAULT DS")

        if (end == "reapontar_resolvedora"
                or aliases.get(end) == "reapontar_resolvedora"):
            emit(node, "DELEGA DESTINO")

        if end == "save" and isinstance(node.func, ast.Attribute):
            receiver = ast.unparse(node.func.value)
            if receiver in touched | constructed or strings(node) & FIELDS:
                emit(node, "SAVE alvo/candidato")

        target_class = any(
            isinstance(x, ast.ClassDef) and x.name == "PerguntaDisputa"
            for x in own
        )
        if (end in OPS and any(k.arg is None for k in node.keywords)
                and (target_class or root == "PerguntaDisputa")):
            emit(node, "REVER **kwargs")

        if end == "setattr":
            emit(node, "REVER setattr")
        if end in {"execute", "executemany", "raw", "RawSQL", "RunSQL"}:
            emit(node, "REVER SQL")
        if end in {"bulk_update", "update"} and strings(node) & FIELDS:
            emit(node, "REVER expressao/lista")
```

O script é um inventário de candidatos, não um analisador interprocedural completo: não resolve universalmente reexports, reflexão, campos montados fora da função, receivers desconhecidos ou SQL dinâmico. Esses casos precisam da revisão adicional abaixo; ausência universal por essas formas: **[nao verificado]**.

**F0 — saída pertinente, classificada após abertura dos arquivos**

Atribuição e seu `save` aparecem reunidos na mesma linha de operação.

| Sítio / função | O que efetivamente escreve | Quem elege | Atomic e `select_for_update` |
|---|---|---|---|
| `M:3014`, `M:3017`, `PerguntaDisputa._nascer` | **Insere `P.disputa` e `P.chamado_destino`.** | Dona e destino recebidos; dia/célula são resolvidos antes. (`M:2875`, `M:2896`.) | `_sob_contrato` abre atomic; `_nascer` trava C. Não há trava autônoma de D/Ch/P em todo nascimento. (`M:2819`, `M:2841`, `M:2845`.) |
| `M:2982`, `M:3006`, `_nascer` | **Troca destino da canônica; não troca dona.** | Canônica do universo do fato; aceita destino solicitado não nulo/diferente, após os dois juízes. (`M:2907`, `M:2921`, `M:2968`, `M:2972`, `M:2980`, `M:3008`.) | Mesmo atomic/C; a costura trava Ch; D/Ch/P são explicitamente pré-travados no ramo de rechaveamento. (`F:768`, `M:2947`.) |
| `F:925`, `F:927`, `reapontar_resolvedora` | **Troca destino, inclusive para `None`.** | Destino do chamador; `resolvedora_valida` e `_costura_nao_mata_o_fio` podem recusar. (`F:907`, `F:909`; `J:2176`.) | Atomic próprio; C; Ch na costura; D/Ch/P adicionais quando há colisão a retratar. (`F:881`, `F:889`, `F:904`, `F:768`.) |
| `M:367`, `_vivas_entre` | Só memória: reassocia instância do **mesmo PK**. | Guarda de identidade; consulta posterior do juiz. (`M:366`, `M:368`.) | Não salva P nessa operação. (`M:349`.) |
| `F:779`, `F:783`, `_costura_nao_mata_o_fio` | Só memória: simula destino e restaura no `finally`. | Julga vitalidade no pós-estado. (`F:778`, `F:781`.) | Não persiste essas atribuições. (`F:783`.) |
| `M:2876`, `_nascer` | Só memória: constrói proxy com `disputa`. | Insumo para `dia_do_turno_da_linha`. (`M:2875`.) | Proxy não é inserido; inserção real é `M:3014`. |
| `M:3387` | Exclusão de chamado pode tornar destino `NULL`. | Política `SET_NULL`; não elege substituto. (`M:3385`.) | Efeito da FK; transação concreta de uma exclusão em produção: [nao verificado]. |
| `M:2418` | Exclusão da dona elimina P. | `CASCADE`; não transfere a pergunta. (`M:2416`.) | Transação concreta de exclusão em produção: [nao verificado]. |

A implementação efetiva de `trava_na_ordem` usa `select_for_update` para C, D, Ch e P, nessa ordem; P só entra quando fornecida em `perguntas=`. (`M:2666`, `M:2675`, `M:2688`, `M:2691`, `M:2692`.)

**Pedidos que chegam ao nascimento/reuso**

| Sítio AST / função | Dona pedida e eleição do destino | Cadeia / fronteira |
|---|---|---|
| `M:2789`, `abrir`; `M:2801`, `abrir_conversa` | Encaminham parâmetros a `_sob_contrato`; não são novas eleições. | `_sob_contrato` → `_nascer`. (`M:2820`.) |
| `E:439`, `E:444`, `gerar_perguntas_disputa` | Dona recebida; destino de `_derivar_chamado_destino`. | Wrapper `E:670`, `E:682`; pendentes `E:294`; fábrica `app/chamados/services/fabrica_da_celula.py:172`. |
| `E:837`, `_criar_pergunta_se_couber` | Dona/destino recebidos. B pede `_chi`; grupo pede primeiro chamado na ordem recebida; ata/complemento pede chamado corrente. | `E:1154`, `E:1197`, `E:1176`, `E:1247`; agrupador HTTP/cron abaixo. |
| `D:376`, `declarar_hora_colab` | Pede a DS obtida pelo núcleo e **o chamado do ato**. | HTTP `app/chamados/views.py:1090`; atomic `D:348`, trava `D:352`, núcleo `D:360`, destino `D:377`. |
| `D:666`, `aplicar_declaracao` | Mesmo padrão, chamado da aplicação administrativa. | HTTP detalhe `app/chamados/views.py:480`; atomic `D:654`, trava `D:656`, núcleo `D:659`, destino `D:667`. |
| `E:1371`, `criar_disputa_ancora_de_chamado` | DS aberta selecionada/criada; destino é `chamado_limbo`. | Atomic/trava `E:1322`, `E:1331`; pedido `E:1377`; ação HTTP `A:189`, despacho `F:524`. |
| `A:263`, `acao_reabrir_disputa_admin` | Conversa na DS reaberta; destino omitido → default `None`. | Despacho HTTP `F:581`; reabertura `A:259`; default `M:2794`. |

A eleição do derivador **não é menor PK**: filtra chamados VIVOS do colaborador, motivos atendidos, exclui candidato já com P do motivo não materializada, compara o dia e desempata pelo turno/estágio; só devolve destino com resultado único. (`E:198`, `E:212`, `E:226`, `E:231`, `E:237`, `E:238`, `E:245`; desempate `E:153`, `E:161`.)

A canônica da **pergunta** é outra eleição: resposta não vazia vence; empate pelo menor PK. Isso não estabelece menor PK para o **chamado-destino**. (`F:1225`, `F:1231`, `F:1232`, `F:1233`.)

**Todos os pedidos nominais a `reapontar_resolvedora` no main**

| Sítio / função | Regra do destino | Cadeia e atomic externo relevante |
|---|---|---|
| `M:437`, `_gate_pergunta_viva` | Sobrevivente do C8: mais novo por criação/PK. (`M:390`; `J:2101`.) | Portas `reconciliar`, `cancelar`, `encerrar`, competência. (`M:685`, `M:786`, `M:1617`, `M:1637`.) Todos os caminhos dessas portas até HTTP/cron: [nao verificado]. |
| `R:954`, `reemitir_chamado_do_dia` | Chamado obtido por `abrir`, exigido VIVO. (`R:910`, `R:936`.) | Chamado por reata `R:1621`; esta chega pela emissão `E:464` ou reconciliação `R:1433`, `R:1526`. |
| `R:1642`, `reatar_pergunta_orfa` | Histórico somente quando há **um** candidato. (`R:1623`.) | Mesmas portas; signal Batida `app/ponto/signals.py:207`, `:215`; signal filho `app/chamados/signals.py:350`, `:368`. |
| `R:1655`, `reatar_pergunta_orfa` | Resultado do derivador. (`R:1556`.) | Mesmas portas; atomic abrangente depende do chamador. [nao verificado] para todos os percursos. |
| `D:370`, `declarar_hora_colab` | Chamado recebido; candidata local sem destino. (`D:363`, `D:364`.) | HTTP `app/chamados/views.py:1090`; atomic/trava `D:348`, `D:352`. |
| `L:244`, `reatar_perguntas_ao_limbo` | Pai LIMBO recebido; seleção mais recente. (`L:220`, `L:221`.) | Command `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:110`; atomic `:99`, trava `:104`. |
| `L:282`, `agrupar_furo` | Pai recebido; no lote, último PK da ordenação crescente. (`L:308`, `L:309`.) | Atomic/trava `L:270`, `L:274`; detector `app/ponto/management/commands/detectar_ausencias.py:761`, cron `app/config/crons.py:364`; lote `L:331`, encadeamento `app/config/crons.py:1509`. |
| `F:2174`, `_migrar_sob_trava` | Novo chamado recebido; só migra a P canônica. (`F:2155`, `F:2165`.) | Atomic/trava `F:2083`, `F:2095`; `Chamado.reabrir` `M:1415`; irmão eleito por estágio/PK `J:2145`. |
| `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:127`, `_reverter` | Destino anterior do JSON, inclusive `None`. (`:118`, `:127`.) | Porta management command; sem atomic externo próprio nesse método; reapontar abre o seu. (`F:881`.) |

A prosa histórica do reconciliador não basta para atribuir essa reata a um cron atual: o corpo declara a migração para evento e o cron como vigia. (`R:1539`, `R:1540`.)

**F0 — estado persistido da DS: todos os valores**

| Sítio / função | Valores gravados | Eleição, atomic e trava |
|---|---|---|
| `E:587`, `_abrir_ou_agregar_disputa` | Nascimento: `fechada_em=NULL`, `fechada_por=NULL`, `resolucao=''`, `motivo_fechamento=''`. Defaults `M:1757`, `M:1766`, `M:1773`, `M:1794`. | Atomic `E:521`; universo travado `E:530`; escolhe aberta por `.first()` e ordering de abertura decrescente. (`E:531`; `M:1804`.) |
| `E:1123`, agrupador de órfãos | Mesmos defaults de nascimento. | Atomic `E:1052`; fecho/travas `E:1059`, `E:1010`; seleção aberta `E:1096`. |
| `E:1358`, âncora | Mesmos defaults de nascimento. | Atomic `E:1322`; universo `E:1331`; seleção aberta `E:1332`. |
| `M:1932`, `M:1936`, `fechar` | `fechada_em=now`, autor, resolução; motivo **`vazia`**, **`veredito`**, **`desligamento`**, **`admin_encerra`** ou **`competencia_trancada`**. (`M:1780`, `M:1792`, `M:1903`, `M:1935`.) | Atomic/trava `M:1905`, `M:1910`. Atenção: respostas/vivas são examinadas **antes** desse atomic interno. (`M:1886`, `M:1895`.) |
| `M:2149`, `M:2152`, save `M:2161`, `reabrir_admin` | `fechada_em=None`, autor `None`, resolução `''`, motivo `''`; contador admin +1, eventual motivo novo. (`M:2150`, `M:2153`, `M:2159`.) | Guardas fechada/limite/usuário `M:2138`; sem atomic/trava próprios. |
| `M:2173`, `M:2176`, save `M:2177`, `reabrir_sistema` | Mesmo reset dos quatro campos, sem consumir contador admin. | Aberta → `False`; fechada → salva e `True`. Atomic/trava pertencem ao chamador. (`M:2167`, `M:2181`.) |

As migrations também entram no levantamento, mas como definição/alteração de schema: criação de `P.disputa`, adição nullable do destino, criação do fechamento nullable e adição do motivo com default vazio. Não são novos escritores de eleição de runtime. (`app/chamados/migrations/0014_perguntadisputa.py:27`; `0016_s84_pergunta_chamado_destino.py:16`; `0013_disputasupervisao.py:24`; `0049_disputa_motivo_fechamento.py:12`.)

**Cadeias dos escritores de fechamento/reabertura**

Todos os pedidos de fechar abaixo chegam a `M:1936`; não são outros escritores da coluna. (`M:1855`, `M:1936`.)

| Chamador direto de `fechar` | Fronteira confirmada / contexto |
|---|---|
| `app/api/foto_recebida.py:80` | Upload HTTP `app/api/views.py:869`, cópia `app/api/views_core.py:854`; atomic externo: [nao verificado]. |
| `R:412` | Reata `R:1619` → emissão ou reconciliação por signal; atomic abrangente de todos os percursos: [nao verificado]. |
| `R:612` | Signal/reconciliação `R:1432` ou cartório `app/ponto/services/cartorio.py:970`; atomic local anterior termina antes desse fechamento. (`R:592`, `R:612`.) |
| `R:1171` | Consumidor `R:1375` → command `app/chamados/management/commands/consumir_eventos_celula.py:31` → cron `app/config/crons.py:394`. |
| `M:1950` | `admin_encerra`; chamador produtivo externo dessa porta: [nao verificado]. |
| `M:2072` | Remoção de motivo por signals Posto/Escala: `app/colaboradores/signals.py:77`, `app/escala/signals.py:161`; atomic local `M:2025` termina antes do fechar. |
| `app/colaboradores/services/desligamento.py:142` | HTTP `app/colaboradores/views.py:1434` ou command agendado `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`; atomic `desligamento.py:103`. |
| `app/ponto/services/entrada_sincronizada.py:59` | Batida HTTP `app/api/views.py:803`, cópia `views_core.py:798`; atomic externo no ponto da chamada: [nao verificado]. |
| `MT:586` | Resposta HTML/REST → `E:1541`; atomic da resposta `E:1503` já terminou nessa chamada. |
| `MT:1333` | Materialização `MT:1378`, validação `V:469`, ação HTTP `A:139` ou passivo `app/chamados/services/passivo_validado.py:405`. |
| `E:1284` | Agrupador HTTP `A:222` ou comandos de alerta/detecção; atomic `E:1052`; crons `app/config/crons.py:362`, `:363`, `:364`. |
| `E:1964` | Resolver por chamado resolvido; wrapper transacional e trava em `E:1796`, `E:1797`; receiver `app/chamados/signals.py:92`. |
| `app/chamados/services/competencia_trancada.py:204` | Trancar período `app/ponto/services/fechamento.py:1428` ou command `app/chamados/management/commands/encerrar_por_competencia_trancada.py:20`; atomic por DS `competencia_trancada.py:197`. |
| `app/chamados/services/becos.py:57` | Signal P após commit `app/chamados/signals.py:238` ou cron `app/config/crons.py:663`; atomic/trava `becos.py:41`, `:51`. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88` | Management command; atomic `:87`; registro de cron: [nao verificado]. |

| Chamador de reabertura | Cadeia e proteção |
|---|---|
| `M:1575`, sistema | Ressuscitação; `Chamado.reabrir` `M:1451`, HTTP `app/chamados/views_arquivar.py:86` ou cartório `app/ponto/services/cartorio.py:1633`; atomic e ordem completa `M:1557`, `M:1562`. |
| `MT:686`, sistema | Ausência rejeitada por signal `app/ponto/signals.py:276`; atomic/trava `MT:675`, `MT:679`. |
| `V:218`, sistema | Validação recusada; no fluxo de validar, atomic/trava `V:425`, `V:431`. O helper isolado não envolve toda a reabertura em atomic. (`V:201`, `V:217`.) |
| `app/chamados/services/relavra_via.py:261`, sistema | Desfaz carimbo sem lastro; atomic por P `:219`, trava `:230`; chamada específica da fábrica pode proibir reabrir fechada. (`app/chamados/services/fabrica_da_celula.py:209`.) |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py:165`, sistema | Management command; atomic `:146`, releitura `:164`; cron automático: [nao verificado]. |
| `A:259

[truncado em 20000 de 47820 caracteres]

## 11/11 02:29 · sessao 0eb5a9c564d8 · - · - · origem=Ronald (tmux)

**BLOQUEIA — B5-j perdeu a mordida no universo extraído.** A produção ainda chama `batidas_apuraveis` em `_carregar_batidas`, mas o tripwire só vigia chamadas a `parear_turnos`/`_turnos_do_colab_puro` e procura queries dentro da função chamadora; a query passou para outro método. O comentário afirmando que está “no escopo [...] para o tripwire [...] ver” ficou falso. `app/ponto/turnos.py:3011`, `app/ponto/turnos.py:3015`, `app/ponto/turnos.py:3017`, `app/ponto/turnos.py:3027`, `app/ponto/turnos.py:3036`; `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:63`, `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:118`, `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:125`.

Reproduzi sem escrever: carreguei as funções AST do tripwire e substituí **em memória**, somente no helper, `batidas_apuraveis(None)` por `Batida.objects`. `universos(original) == universos(mutante)` deu `True`; ambos retornaram `crus=[]`. A lacuna interprocedural já é descrita pelo próprio teste, mas o split introduziu justamente essa forma no novo caminho. `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:27`, `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:103`.

**BLOQUEIA — relógio da consulta não atravessa CF.** A nova vida é lida com `hoje=localdate(self.agora)`, mas CF usa `_tz.now()` no teto e não recebe `agora`; assim o resultado novo da tarefa pode depender do relógio externo, mesmo conservando o mesmo `agora`. `app/chamados/services/tarefa_do_dia.py:478`, `app/chamados/services/tarefa_do_dia.py:480`; `app/chamados/juizes.py:1414`; `app/escala/servico_jornada.py:375`, `app/escala/servico_jornada.py:376`.

Reproduzi com CF real via apoio, vínculo diurno 08–17, sem ausências/batidas e a mesma carga: só mudar `tz.now` de 12h para 20h mudou `(None,None,None)` para `('E','saida_sem_entrada','ENTROU')`, sem erro registrado. Os testes existentes fixam `now=lambda: AGORA`; portanto não confrontam relógio real versus borda injetada. O comportamento já existe em CF avulso, mas c8b passa a consumi-lo na tarefa. `app/chamados/tests/e3_c8b_apoio.py:89`, `app/chamados/tests/e3_c8b_apoio.py:91`; `app/escala/servico_jornada.py:351`, `app/escala/servico_jornada.py:376`, `app/escala/servico_jornada.py:471`.

**FILA — outra alimentação temporal antiga continua pelo template.** CF chama `fim_turno(ec.tipo_escala,data_evento)` sem `ec`/`celulas`; `fim_turno` encaminha isso a `marcos_turno`, que lê o template quando `ec=None`, embora a contagem posterior de CF leia os marcos congelados do vínculo/célula. Não é divergência nova carga versus avulso; é uma segunda fonte já existente no teto, agora alcançável pela tarefa. `app/escala/servico_jornada.py:375`, `app/escala/servico_jornada.py:89`, `app/escala/servico_jornada.py:68`, `app/escala/servico_jornada.py:71`, `app/escala/servico_jornada.py:427`.

**O que os testes realmente exercitam.** [nao verificado]

- Corpos C6, CF, slot/materialização são extraídos do fonte via AST; não são simples lambdas devolvendo veredito. O extrator remove decorators e executa a função isolada, portanto não equivale a importar a aplicação com o ORM. `app/chamados/tests/e3_c8b_apoio.py:93`, `app/chamados/tests/e3_c8b_apoio.py:102`, `app/chamados/tests/e3_c8b_apoio.py:129`; `app/chamados/tests/e4_apoio.py:8`, `app/chamados/tests/e4_apoio.py:13`.
- São doubles: `CargaDaJanela` inteira, carregamento de batidas, turnos avulsos **e** corpo puro do pareamento (ambos devolvem a mesma lista `turnos` recebida), grade, tranca em lote, modelos/managers, timezone e catálogo de parcialidade (`lambda a:a.parcial`). Essa paridade de turnos é estabelecida pela infraestrutura, não pelo pareador real. `app/chamados/tests/e3_c8b_apoio.py:63`, `app/chamados/tests/e3_c8b_apoio.py:72`, `app/chamados/tests/e3_c8b_apoio.py:112`, `app/chamados/tests/e3_c8b_apoio.py:119`, `app/chamados/tests/e3_c8b_apoio.py:123`, `app/chamados/tests/e3_c8b_apoio.py:132`.
- O teste anunciado “carga da tarefa avulso/lote” evita o `__init__` produtivo usando `cls.__new__`, injeta grade/perguntas/decisões/efeitos/fila e substitui K6/`alimentar_fila`. Ele prova ligação dos corpos nessa fixture, não leitura ORM desses insumos. `app/chamados/tests/test_e3_c8b_sem_banco.py:218`, `app/chamados/tests/test_e3_c8b_sem_banco.py:221`, `app/chamados/tests/test_e3_c8b_sem_banco.py:227`, `app/chamados/tests/test_e3_c8b_sem_banco.py:232`.
- O teste básico de batidas usa `_turnos` previamente preenchido: nunca executa o pareador e só verifica uma leitura. `app/chamados/tests/test_e3_c8b_sem_banco.py:505`, `app/chamados/tests/test_e3_c8b_sem_banco.py:507`, `app/chamados/tests/test_e3_c8b_sem_banco.py:512`, `app/chamados/tests/test_e3_c8b_sem_banco.py:514`.
- Os testes AS/B puros injetam o bool `viva_c6`; o juiz “dia sem trabalho” é substituído por `lambda d:True`, e B declara FK/texto/E1 hipotéticos. Eles não provam escritores reais produzindo os dados usados pela cura. `app/chamados/tests/test_e3_c8b_sem_banco.py:41`, `app/chamados/tests/test_e3_c8b_sem_banco.py:53`; `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:91`, `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:105`, `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:119`.

**Execução segura realizada:** `PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -q chamados.tests.test_e3_tarefa_admin_sem_banco chamados.tests.test_e3_c8b_sem_banco`: RC0, 32 testes, OK. Também executei o agregado dos 14 mutantes: RC0; o runner exige falha por assertion, sem `ERROR`. Resultados de execução não têm arquivo de saída por exigência de só leitura; executores em `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:115`, `app/chamados/tests/test_e3_c8b_sem_banco.py:47`, `app/chamados/tests/e3_c8b_mutantes.py:17`, `app/chamados/tests/e3_c8b_mutantes.py:87`, `app/chamados/tests/e3_c8b_mutantes.py:105`.

**Limite dos mutantes:** `C6_pareia_janela_larga` morre porque o stub `Janela.turnos` levanta `AssertionError`, capturado pelo C6 e denunciado por `env.erros`; isso protege o caminho escolhido, sem provar a geometria das janelas. `app/chamados/tests/e3_c8b_apoio.py:75`, `app/chamados/tests/e3_c8b_mutantes.py:65`, `app/chamados/tests/test_e3_c8b_sem_banco.py:417`, `app/chamados/juizes.py:1430`.

**RED ORM que faltam para certificar c8b** — são lacunas dos novos testes indicados, não alegação de inexistência de toda cobertura antiga: [nao verificado]

1. B com pergunta/decisões/FK reais, resposta registrada pelo escritor real, e tarefa completa mostrando `validar/admin`; A/S gerado por ausência+batidas e lavra real, incluindo ausência de perguntas e perguntas mortas. Os atuais recebem essas condições prontas. `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:105`, `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:119`, `app/chamados/tests/test_e3_c8b_sem_banco.py:35`.
2. Igualdade do dict inteiro avulso/lote passando por `CargaDaTarefa.__init__` real, com N=1/N=4 e contagem SQL incluindo relações lazy, grade, cobertura, tranca e K6; atualmente o construtor é contornado e o pai substituído. `app/chamados/tests/test_e3_c8b_sem_banco.py:227`, `app/chamados/tests/e3_c8b_apoio.py:63`.
3. Turnos reais D..D e D−1..D+1, nas bordas do pad, overnight, troca de escala, retratadas, geradora fora da vigência e ordenação empatada. Atualmente avulso/puro devolvem a mesma lista sintética. `app/chamados/tests/e3_c8b_apoio.py:121`, `app/chamados/tests/e3_c8b_apoio.py:123`.
4. Ausências SQL com estados de cobrança, parciais reais/catálogo, sobreposição e range malformado, confrontadas com `ausencias_do_dia`; atualmente managers/Q são simulados e parcialidade é uma flag sintética. `app/chamados/tests/e3_c8b_apoio.py:17`, `app/chamados/tests/e3_c8b_apoio.py:39`, `app/chamados/tests/e3_c8b_apoio.py:120`.
5. K6 carregado com objetos ORM: revogada sem substituta, substituição, empate `gerado_em/pk`, lista parcial/não anotada/outra tripla; teste novo usa objetos `Registro` e `Query` sintéticos. A suíte antiga tem boas formas ORM de revogação/substituição/empate, mas avulsas. `app/chamados/tests/test_e3_c8b_sem_banco.py:437`, `app/chamados/tests/test_e3_c8b_sem_banco.py:459`; `app/folha/tests/test_k6_competencia_entregue.py:72`, `app/folha/tests/test_k6_competencia_entregue.py:78`, `app/folha/tests/test_k6_competencia_entregue.py:85`.
6. Frescor depois dos escritores reais — resposta, validação, materialização/retratação de batida, lavra, tranca/toque e exportação — comparando carga nova/antiga e memo avulso. O teste atual modifica só dicionário de ausências em memória e compara `_pv_memo`; a suíte ORM anterior testa frescor da consulta do app, não `p.viva_c6` da nova carga. `app/chamados/tests/test_e3_c8b_sem_banco.py:290`, `app/chamados/tests/test_e3_c8b_sem_banco.py:298`; `app/chamados/tests/test_e3_v4_orm.py:101`.
7. `agora` fixo antes/depois do fim do turno com relógio ambiente variando, incluindo DNA diferente do template. A infraestrutura atual iguala os relógios. `app/chamados/tests/e3_c8b_apoio.py:89`, `app/escala/servico_jornada.py:375`.

Paridade ORM completa e mudanças reais dos 232 dias: **[não verificado]**.

## 11/11 02:29 · sessao 0a0b269ffb28 · - · - · origem=Ronald (tmux)

Auditoria da carga concluída, somente leitura.

**BLOQUEIA — existe `carga.vinculo != avulso`.** Se a célula do colab 7 aponta para um vínculo do colab 8 cuja vigência cobre o dia, `escala_vigente` passa a geradora diretamente ao juiz e devolve esse vínculo; a carga consulta apenas o balde do colab 7 e devolve `None`. Reproduzi os corpos reais por AST, sem Django/DB/arquivos. A FK não declara igualdade entre os colaboradores. Ocorrência em produção: [não verificado]. Fontes: `app/escala/servico_jornada.py:55`, `app/chamados/services/carga_pergunta_viva.py:157`, `app/ponto/turnos.py:2990`, `app/escala/alimentacao.py:279`, `app/escala/models.py:1692`.

**BLOQUEIA — o teste de paridade substitui justamente esse carregador avulso.** `ambiente_c6` troca `escala_vigente` por `vinculo_avulso`, que chama diretamente `vinculo_do_dia` sobre a lista fornecida. Logo, não prova paridade com a carga real de `escala_vigente`. Fonte: `app/chamados/tests/e3_c8b_apoio.py:97`.

**BLOQUEIA — B5-j ficou cego ao carregador extraído.** O detector examina o escopo que chama `_turnos_do_colab_puro`; a query agora fica em `_carregar_batidas`. Reproduzi uma mutação textual em memória substituindo a porta por `Batida.objects` sem filtro de retratação: original e mutante retornaram os mesmos universos, com `crus=[]`. A produção continua usando a porta. Fontes: `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:118`, `app/ponto/turnos.py:3011`, `app/ponto/turnos.py:3023`.

**BLOQUEIA — relógio fora da borda.** A tarefa passa `hoje=localdate(agora)` ao C6, mas `classificar_falta` continua comparando o teto com `_tz.now()`. A consulta nova não depende exclusivamente do `agora` recebido. Fontes: `app/chamados/services/tarefa_do_dia.py:478`, `app/escala/servico_jornada.py:374`.

Paridade estática restante:

- **Ausências:** mesmo efeito `cobranca`, exclusão de `rejeitada`, mesmo recorte inclusivo de cobertura e preservação de todas as sobrepostas; o resultado usado é existência de ausência inteira, sem desempate por ordem. Fontes: `app/chamados/services/carga_pergunta_viva.py:98`, `app/ponto/turnos.py:2511`, `app/ponto/turnos.py:2531`, `app/ponto/turnos.py:2757`, `app/escala/servico_jornada.py:338`.
- **Turnos:** reutiliza `_turnos_do_colab_puro`, reaplicado à janela exata; pad de dois dias anteriores e um seguinte; batidas recortadas por `[00:00 pad_ini, 00:00 pad_fim+1)`; universo vivo vem da porta; cópias evitam compartilhar carimbos de pareamento. Fontes: `app/chamados/services/carga_pergunta_viva.py:163`, `app/ponto/turnos.py:1834`, `app/ponto/turnos.py:1918`, `app/ponto/turnos.py:3017`.
- **Células/grade/DNA:** carregadas pela porta comum e repassadas ao leitor e ao mesmo derivador de DNA; a grade exige a lista inteira de vínculos. Fontes: `app/ponto/turnos.py:2980`, `app/chamados/services/carga_pergunta_viva.py:149`, `app/chamados/services/carga_pergunta_viva.py:178`, `app/escala/services/leitor_celula.py:350`, `app/escala/utils.py:84`.
- **Ordenação:** batidas têm unicidade `(colaborador,timestamp)`; vínculos têm unicidade `(colaborador,data_inicio)`, eliminando empate legítimo da chave `(ativa,data_inicio)`. Não encontrei outro contraexemplo determinístico por ordenação. Fontes: `app/ponto/models.py:105`, `app/escala/models.py:1003`. Paridade ORM integral: [não verificado].

RED ORM necessário neste recorte: geradora de outro colaborador confrontando `escala_vigente`, `carga.vinculo`, slot e C6; janelas D..D e D−1..D+1 com noturno/troca de vínculo/batida retratada; ausências sobrepostas/rejeitadas/range malformado; execução em dois relógios reais com o mesmo `agora`. Ausência dessa cobertura integral: `app/chamados/tests/e3_c8b_apoio.py:61`, `app/chamados/tests/e3_c8b_apoio.py:121`, `app/chamados/tests/e3_c8b_apoio.py:138`.

## 11/11 02:28 · sessao c70532bdb5bc · - · - · origem=Ronald (tmux)

Achados para consolidar:

1. **BLOQUEIA — consumidor de `viva_c6` fora do universo carregado.** `_tarefa_pura` expõe também pergunta de motivo do mapa cuja posição não existe nas instâncias; `_ler_vida` só seleciona dia sem trabalho, posição acesa ou motivo fora do mapa. Com dia de trabalho, instâncias existentes e pergunta de marco ausente, falta a única atribuição de `viva_c6`; o acesso direto gera `AttributeError`. Reproduzi sem arquivos usando o corpo real, infraestrutura existente, `sem_trabalho=False`, apenas instância início e pergunta fim sem atributo. Referências: `app/chamados/services/tarefa_do_dia.py:245`, `:313`, `:467`, `:480`; infraestrutura usada: `app/chamados/tests/test_e3_c8b_sem_banco.py:19`. **RED ORM faltante:** dia trabalho com grade atual início/fim e pergunta antiga de intervalo removido; tarefa avulsa/lote não pode lançar nem fabricar decisão.

2. **BLOQUEIA — nova exposição desfaz o horizonte da decisão.** `_dec` e passivo obedecem `ato_em <= agora`; C6 novo recebe apenas a data de `agora`, porém mata imediatamente por `validada_em` atual, independentemente de ser posterior. Se RESPOSTA existia em T1 e VALIDACAO nasceu em T2, consulta em T1 preserva RESPOSTA em `_dec`, mas o C6 atual suprime `validar/admin`. A porta real escreve validação e linha E1 com mesmo instante, portanto o cenário é alcançável. Referências: `app/chamados/services/tarefa_do_dia.py:385`, `:394`, `:478`, `:245`, `:133`; `app/chamados/juizes.py:1346`; `app/chamados/services/validacao.py:134`. **RED ORM faltante:** consultar o mesmo `agora=T1` antes/depois de registrar validação em T2; comparar dict inteiro.

3. **FILA/contrato K6 — alimentação não é validada pelo juiz.** `exportacoes` fornecido pula seleção por empresa/mês/ano e anotação; uma linha vigente de outra tripla pode devolver `True`/registro para tripla pedida, e uma lista incompleta pode devolver `False`. Linhas reais não anotadas usam `e_vigente`, que consulta a vigência individual e introduz N consultas; não tornam automaticamente o resultado incorreto quando a tripla é correta. Referências: `app/folha/export.py:583`, `:586`; `app/folha/models.py:121`, `:134`, `:200`. O único caller produtivo novo encontrado anota e filtra a tripla corretamente: `app/chamados/services/tarefa_do_dia.py:437`, `:438`. **RED ORM faltante:** outra tripla, mistura de triplas, linha antiga sem anotação, empate `(gerado_em,pk)`, vigente invalidada sem substituta; o teste atual só fornece listas corretas anotadas em `app/chamados/tests/test_e3_c8b_sem_banco.py:473`.

4. **Cache C6: não achei segundo juiz nem leitor produtivo fora da carga.** Único escritor produtivo é o retorno de `pergunta_viva`; leitores produtivos são `expor_admin` e a guarda residual. A carga não relê após `_vida_carregada`; não possui invalidação. O único consumidor produtivo externo encontrado é o command de sombra, que percorre dias e lê saídas, sem mutação nesse percurso. Referências: `app/chamados/services/tarefa_do_dia.py:480`, `:245`, `:326`, `:445`; `app/chamados/management/commands/diff_e3_sombra.py:115`, `:125`. Uso após escrita/outro lote: **[não verificado]**; não confundir com o bug de atributo não carregado acima.

5. **Residual A/S não deriva contradição nem alcança `nunca_bateu`/`indefinida` por esse ramo.** Exige literalmente `sem_trabalho`, `fato_em_ausencia`, ausência de pergunta viva elegível e competência não entregue; cria ação administrativa do dia sem pergunta. Pergunta fora do fato e última retração saem antes. Em dia de trabalho o residual não executa. Referências: `app/chamados/services/tarefa_do_dia.py:230`, `:234`, `:325`, `:330`. Isso não prova resultado real dos dias anexados: K6, E1 e demais complementos produtivos **[não verificado]**.

Cadeia real do valor A/S e dos campos principais:

| Campo/valor | Quem escreve/decide | Quem lê no caminho novo |
|---|---|---|
| `FATO_EM_AUSENCIA` | Supra-juiz: fato em dia encerrado + cobertura aprovada; parcial sem período sai como cadastro; `app/ponto/supra_juiz.py:379`, `:382`, `:400` | `derivar_veredito`, mapa em `app/escala/models.py:1619`, função `:1624` |
| `CelulaDia.veredito/via/ata` | Cartório chama porta `app/ponto/services/cartorio.py:1813`; atribuição real `app/ponto/portas/celula.py:1033`; admin prevalece em `:1003` | Grade transporta `app/escala/services/leitor_celula.py:424`; tarefa lê `app/chamados/services/tarefa_do_dia.py:325`; C6 lê `app/chamados/juizes.py:1359` |
| `trabalha/dna/escala_geradora` | Operação gerar/editar declaradas na porta `app/ponto/portas/celula.py:7`, `:14` | Grade entrega insumo para juiz de dia sem trabalho `app/escala/services/leitor_celula.py:428`; tarefa consulta `app/chamados/services/tarefa_do_dia.py:207` |
| `materializada_em/via_resolucao` | Atribuição única em memória `_escrever_carimbo`, `app/chamados/models.py:3097`; portas persistem `:3152`, `:3190` | C6 `app/chamados/juizes.py:1357`, `:1383`, `:1421`; identidade tarefa `app/chamados/services/tarefa_do_dia.py:230` |
| `respondida_em/validada_em` | Carimbo com resposta `app/chamados/models.py:3130`; validação especializada `app/chamados/services/validacao.py:134`; desfazer em memória `app/chamados/models.py:3163` | C6 `app/chamados/juizes.py:1346`, `:1362`; tarefa lê decisão E1 em `app/chamados/services/tarefa_do_dia.py:133` |
| `DecisaoPergunta.ato/ato_em` | Exemplo escritor real validação registra em `app/chamados/services/validacao.py:139` | Carga ordena por pergunta/instante/pk e corta horizonte `app/chamados/services/tarefa_do_dia.py:385`; `_pela_decisao` usa última `:132` |
| `ExportacaoDominio` | Registro real `app/folha/models.py:174`, `:182` | K6 `app/folha/export.py:583` |
| `_vigente_anotado/vigente` | Anotador compara `(gerado_em,pk)` e atribui em memória `app/folha/models.py:220`, `:224` | `e_vigente` `app/folha/models.py:123`; `so_vigentes` `:200`; K6 `app/folha/export.py:583` |
| `invalidada_em` | Porta `invalidar` `app/folha/models.py:143`, atribuição `:158`, save `:162` | K6 `app/folha/export.py:586` |
| `viva_c6` | Somente resultado C6 `app/chamados/services/tarefa_do_dia.py:480` | `:245`, `:326` |
| `exportada/_exportadas` | Alimentação pelo K6 `app/chamados/services/tarefa_do_dia.py:438`, `:472` | Exclusão de candidatas `:475`; guardas puras `:245`, `:325` |
| `acoes/pergunta_id/razao` novos | Somente dict em memória `app/chamados/services/tarefa_do_dia.py:249`, `:252`, `:331` | Projeção de portas `:534`; command sombra `app/chamados/management/commands/diff_e3_sombra.py:125` |

Censo completo de todos os escritores/leitores dinâmicos desses campos: **[não verificado]**.

## 11/11 02:28 · sessao a614d13f920e · FIO-E0-V4-E3 · construtor · origem=Code

**lei: PARE — encontrei a condição de parada do C6.**

A cura exigida altera os dois leitores citados:

- `universo_adesao` seleciona exclusivamente células `nunca_bateu` e incrementa `dias` para cada uma. Num contraexemplo com um trabalho NB e uma folga NB, preservar o trabalho e absolver a folga reduz `dias` de **2 para 1**, independentemente dos marcos. [adesao.py:34](/home/ronald/wt-codex-fio-e0-v4-e3-022700/app/chamados/services/adesao.py:34), [adesao.py:40](/home/ronald/wt-codex-fio-e0-v4-e3-022700/app/chamados/services/adesao.py:40).
- `dia_nunca_bateu_sem_adesao` compara somente o literal: para a folga curada, passa necessariamente de **True para False**. [fabrica_da_celula.py:89](/home/ronald/wt-codex-fio-e0-v4-e3-022700/app/chamados/services/fabrica_da_celula.py:89).

A contagem alimenta a descrição usada na abertura do chamado. Isso demonstra mudança no universo e no texto calculado; **não demonstra atualização de um chamado já persistido**. [adesao.py:17](/home/ronald/wt-codex-fio-e0-v4-e3-022700/app/chamados/services/adesao.py:17), [adesao.py:77](/home/ronald/wt-codex-fio-e0-v4-e3-022700/app/chamados/services/adesao.py:77). Fechamento, duplicação ou alteração efetiva dos chamados existentes: [nao verificado].

O retentor solicitado é `folha.export.motivos_retencao_celula`: consulta `CelulaDia.VEREDITOS_ACUSAM` e aplica as ressalvas dos protestos e da pausa. [export.py:1236](/home/ronald/wt-codex-fio-e0-v4-e3-022700/app/folha/export.py:1236), [export.py:1256](/home/ronald/wt-codex-fio-e0-v4-e3-022700/app/folha/export.py:1256).

Parei antes de editar, conforme a ordem expressa do C6. Cura, RED/GREEN ORM, mutantes e ruff não foram executados; não há certificação desta volta. [nao verificado — execução suspensa pela condição de parada].

## 11/11 02:28 · sessao 61a58b3a3472 · FIO-E0-V4-E4B · sonda · origem=Code

Usei a saída raspada fornecida, sem nova execução. Os números abaixo são transcrições dessa saída, marcados **[nao verificado]** por não terem sido medidos nesta sessão. [nao verificado]

**1. FOTO_SOMBRA**

`2026-11-11 02:03:21.722000-03:00`, com `hoje=2026-11-11`: **posterior** ao pouso informado da E4a-2 em 10/11, 14:5x local. Pelo critério temporal solicitado, a foto serve como pós E4a-2. [nao verificado] [nao verificado]

A saída termina com `rc_sombra=0` e imprime ambos os censos. Há mensagens `ERROR tipo_marco_divergente`, com divergências `marcos_faltantes=S x ata=E` e `marcos_faltantes=E x ata=S`. [nao verificado] Nesse ramo, o código registra a divergência e retorna, sem lançar a exceção construída; portanto, essas mensagens não indicam, por si, abortamento da sonda. [materializacao.py:299](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/services/materializacao.py:299)

**2. Contadores impressos**

| Censo | Chave | Valor | Evidência |
|---|---|---:|---|
| Alcance | `fio_morto_alcance` | 579 | [nao verificado] |
| Alcance | `alcance_universo` | 2733 | [nao verificado] |
| Alcance | `alcance_de_hoje_pra_frente` | 0 | [nao verificado] |
| Vivo | `fio_morto` | 4 | [nao verificado] |
| Vivo | `fio_morto_de_hoje_pra_frente` | 0 | [nao verificado] |

**3. Decomposição e conferência**

| Rota\|papel | Quantidade | Evidência |
|---|---:|---|
| aguarda_decisao_de_ausencia\|Aguardando decisao do DP | 2 | [nao verificado] |
| aguarda_decisao_de_ausencia\|Aguardando documento do colaborador | 1 | [nao verificado] |
| decisao_do_dia_pendente\|regularizar_batida | 266 | [nao verificado] |
| fio_do_admin\|admin-batida | 3 | [nao verificado] |
| fio_do_admin\|admin-cadastro | 176 | [nao verificado] |
| porta_fila\|cobrar | 1062 | [nao verificado] |
| porta_fila\|cobrar,decidir | 33 | [nao verificado] |
| porta_fila\|cobrar,validar | 3 | [nao verificado] |
| porta_fila\|decidir | 70 | [nao verificado] |
| porta_fila\|decidir,validar | 15 | [nao verificado] |
| porta_fila\|validar | 144 | [nao verificado] |
| porta_pergunta\|cobrar | 259 | [nao verificado] |
| porta_pergunta\|cobrar,validar | 1 | [nao verificado] |
| porta_pergunta\|historico | 10 | [nao verificado] |
| porta_pergunta\|validar | 109 | [nao verificado] |
| sem_porta\|nenhuma porta | 579 | [nao verificado] |

A soma de **todas as rotas confere com `alcance_universo=2733`**; o componente `sem_porta|nenhuma porta=579` confere com `fio_morto_alcance=579`. [nao verificado] Esse é o significado do mapa no código: ele cobre o universo inteiro. [vigia_fio.py:551](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/services/vigia_fio.py:551)

| Competência — somente `sem_porta` | Total | Causas impressas | Evidência |
|---|---:|---|---|
| 10/2026 | 132 | c=12; d=115; e=5 | [nao verificado] |
| 11/2026 | 447 | a=2; c=4; d=339; e=102 | [nao verificado] |

**Confere:** `132 + 447 = 579`; os subtotais de causas também conferem com cada competência. A saída imprime `SEM_PORTA_LINHAS=579` e `OMITIDOS=2121`. [nao verificado] O código preserva todas as linhas `sem_porta`; a omissão limita outras rotas. [vigia_fio.py:628](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/services/vigia_fio.py:628)

**4. Comparação com o DRY de 10/11**

| Classe do DRY | DRY | Saída atual | Movimento demonstrável | Evidência |
|---|---:|---|---|---|
| Total | 385 | 579 | Subiu | [nao verificado] |
| E3 | 232 | nao separado pela saida | Indeterminável | [nao verificado] |
| segue_sem_porta | 82 | nao separado pela saida | Indeterminável | [nao verificado] |
| classe e | 45 | 107 | Subiu | [nao verificado] |
| nao_sei | 20 | nao separado pela saida | Indeterminável | [nao verificado] |
| outros | 6 | nao separado pela saida | Indeterminável | [nao verificado] |

A classificação atual imprime `alcance_a=2`, `alcance_b=0`, `alcance_c=16`, `alcance_d=454`, `alcance_e=107`. Dentro de `e`: `pergunta_viva_sob_chamado_terminal=53` e `sem_pergunta_viva=54`. [nao verificado] Não há, na saída fornecida, correspondência individual com E3, `segue_sem_porta`, `nao_sei` e `outros`. [nao verificado]

**5. “Hoje pra frente” > 0?**

**Não:** `alcance_de_hoje_pra_frente=0`, `fio_morto_de_hoje_pra_frente=0` e `SEM_PORTA_HOJE_PRA_FRENTE=0`. Nenhuma célula/linha individual de hoje+ é mostrada no trecho fornecido. [nao verificado]

As chaves que contam hoje+ são `alc['alcance_de_hoje_pra_frente']` e `viv['fio_morto_de_hoje_pra_frente']`, incrementadas nos respectivos ramos de data maior ou igual a hoje. [vigia_fio.py:648](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/services/vigia_fio.py:648) [vigia_fio.py:420](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/services/vigia_fio.py:420)

_3640 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
