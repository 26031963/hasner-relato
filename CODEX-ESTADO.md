# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 09:47** · publicado em **17/11/2026 09:47** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 09:47 · sessao a37b28740f37 · - · - · origem=Ronald (tmux)

Achados confirmados no código e por simulação somente em memória:

- **O caminho completo ainda pode derrubar o ESTADO.** JSON de sessão válido com `payload: [1]`, `rate_limits: [1]` ou raiz `["rate_limits"]` produz `AttributeError`; `ler_semanal()` captura somente `ValueError` do parsing, enquanto `ler()` captura somente `OSError` dessa leitura. A chamada em `gerar_estado.py` não protege leitura nem composição. (`bin/codex_limite_estado.py:325`, `bin/codex_limite_estado.py:326`, `bin/codex_limite_estado.py:328`, `bin/codex_limite_estado.py:87`, `bin/gerar_estado.py:270`)
- **Também há queda na composição.** `used_percent: "lixo"` passa pelo leitor e produz `ValueError` em `float(...)`; não há guarda na composição da frase. (`bin/codex_limite_estado.py:331`, `bin/codex_limite_estado.py:448`, `bin/codex_limite_estado.py:453`, `bin/gerar_estado.py:270`)
- **Entrada malformada ainda pode virar zero/ok.** Uma linha `fim` com `rc=lixo` e `dur=lixo`, mas quantidade suficiente de campos, é aceita sem incrementar descartadas. Ela encerra a chamada pelo simples casamento da chave; com fila legível sem ABERTO, obtém-se `em_curso=0`, `estado=ok`, além de `rc=lixo` publicado em últimas chamadas. Reprodução em memória confirmou esses valores. (`bin/codex_limite_estado.py:129`, `bin/codex_limite_estado.py:139`, `bin/codex_limite_estado.py:205`, `bin/codex_limite_estado.py:273`, `bin/codex_limite_estado.py:296`, `bin/codex_limite_estado.py:178`)
- **Ausência do log ainda se apresenta como zeros nas seções auxiliares.** `_registro()` devolve registro vazio, `ler_chamadas()` devolve todos os papéis zero e `ultimas_chamadas()` devolve lista vazia; a ociosidade, corretamente, continua SEM DADO. (`bin/codex_limite_estado.py:115`, `bin/codex_limite_estado.py:161`, `bin/codex_limite_estado.py:174`, `bin/codex_limite_estado.py:270`)

Reparos de v1 efetivos:

- O segundo juiz de últimas chamadas foi removido no caminho `ler()`: a mesma foto alimenta papéis, últimas e ociosidade; `_em_curso` é carimbado e seu valor é consumido por últimas. (`bin/codex_limite_estado.py:93`, `bin/codex_limite_estado.py:153`, `bin/codex_limite_estado.py:176`)
- A janela do módulo passou a uma função única `(agora−24h, agora]`, usada por limite, papéis e contexto por hora. (`bin/codex_limite_estado.py:59`, `bin/codex_limite_estado.py:83`, `bin/codex_limite_estado.py:164`, `bin/codex_limite_estado.py:283`)
- `30min59s` agora acusa; a idade está em segundos, comparada a 1800. (`bin/codex_limite_estado.py:243`, `bin/codex_limite_estado.py:296`)
- `rc` e `situacao` agora ocupam posições distintas e o consumidor foi adaptado, embora `rc` de entrada não seja validado. (`bin/codex_limite_estado.py:178`, `bin/codex_limite_estado.py:179`, `bin/codex_limite_estado.py:486`, `bin/codex_limite_estado.py:139`)

Classes ainda presentes:

- **Relógio fora da borda:** o disjuntor chama `ler_chamadas(raiz)` sem transmitir seu `agora`; a função captura outro relógio. (`bin/disjuntor.py:2036`, `bin/codex_limite_estado.py:159`)
- **Derivação paralela:** o censo mantém janela inclusiva nas duas bordas, diferente da função única do módulo, e usa `_linhas_chamadas()`, que descarta os contadores de linhas inválidas/futuras do registro. (`bin/disjuntor.py:2132`, `bin/codex_limite_estado.py:59`, `bin/codex_limite_estado.py:145`, `bin/codex_limite_estado.py:146`)

Ocorrência desses casos em produção: [nao verificado].

## 17/11 09:46 · sessao 0a302ad15775 · - · - · origem=Ronald (tmux)

- **`chamadas.log/ini`**: `bin/codex_pergunta.sh:308` grava hora local sem offset, obra, papel, hash de 12 caracteres, PID `$$`, tipo `ini` e pergunta raspada; a pergunta é reduzida a uma linha em `bin/codex_pergunta.sh:295`. O registro precede a espera por slot em `bin/codex_pergunta.sh:364`; portanto, conta tentativa/espera do wrapper, sem provar execução do Codex (`bin/disjuntor.py:2073`). `_registro` lê hora/obra/papel/hash/PID/tipo, ignora conteúdo da pergunta e usa `(obra,hash,pid)` como chave (`bin/codex_limite_estado.py:124`, `bin/codex_limite_estado.py:128`, `bin/codex_limite_estado.py:135`, `bin/codex_limite_estado.py:137`).

- **`chamadas.log/fim`**: `_saida`, via trap EXIT, grava hora, mesma chave, papel, sessão ou `-`, código de saída e duração em segundos de epoch (`bin/codex_pergunta.sh:265`, `bin/codex_pergunta.sh:269`, `bin/codex_pergunta.sh:272`, `bin/codex_pergunta.sh:282`). O parser valida timestamp e quantidade de campos, mas **descarta a hora do fim** e guarda sessão/rc/duração como strings sem validação de domínio (`bin/codex_limite_estado.py:124`, `bin/codex_limite_estado.py:129`, `bin/codex_limite_estado.py:139`). `_em_curso` considera qualquer `fim` da chave suficiente para encerrar, sem testar se ocorreu depois do `ini` (`bin/codex_limite_estado.py:205`).

- **Consumidores de `ini/fim`**: contagem por papel (`bin/codex_limite_estado.py:157`), últimas chamadas (`bin/codex_limite_estado.py:169`), ociosidade (`bin/codex_limite_estado.py:273`), origem de sessões (`bin/codex_limite_estado.py:304`, `bin/gerar_codex_estado.py:176`), tokens por papel (`bin/codex_limite_estado.py:399`) e censo do disjuntor (`bin/disjuntor.py:2126`). Não encontrei consumidor da duração além de seu armazenamento; consumidor externo: [nao verificado].

- **`fila.md`**: escritor demonstrado somente na fixture, que grava o argumento `fila` (`bin/tests/test_codex_ociosidade.sh:39`). Produtor real, responsável pela transição para ABERTO e garantia do carimbo no nascimento: [nao verificado]. O leitor exige prefixo de estado conhecido e carimbo `AAAA-MM-DD HH:MM` para ABERTO, calcula idade em segundos inteiros e devolve quantidade/idade/erro (`bin/codex_limite_estado.py:53`, `bin/codex_limite_estado.py:54`, `bin/codex_limite_estado.py:227`, `bin/codex_limite_estado.py:235`, `bin/codex_limite_estado.py:243`, `bin/codex_limite_estado.py:251`). Ociosidade consome esses resultados (`bin/codex_limite_estado.py:287`).

- **`limite.log`**: wrapper escreve hora, obra e primeira mensagem de erro reconhecida como limite/indisponibilidade, com tabs/newlines removidos da mensagem (`bin/codex_pergunta.sh:451`, `bin/codex_pergunta.sh:452`, `bin/codex_pergunta.sh:454`, `bin/codex_pergunta.sh:456`). `ler()` lê os três campos, valida somente hora/quantidade mínima, conta linhas e publica total/janela/última hora; não usa obra nem mensagem para classificar o evento (`bin/codex_limite_estado.py:73`, `bin/codex_limite_estado.py:75`, `bin/codex_limite_estado.py:81`, `bin/codex_limite_estado.py:97`). A fonte de escrita pode ser alterada por `CODEX_LOGD`, enquanto o leitor usa `<raiz>/logs/codex` (`bin/codex_pergunta.sh:101`, `bin/codex_limite_estado.py:45`, `bin/codex_limite_estado.py:64`); configuração real alinhada: [nao verificado].

- **Tupla `ultimas_chamadas`**: escritor agora produz `(hora,obra,papel,rc,situacao)`, com `rc=None` sem fim e situação separada (`bin/codex_limite_estado.py:175`, `bin/codex_limite_estado.py:177`, `bin/codex_limite_estado.py:179`). `ler()` fornece a mesma `_foto` para as últimas e a contagem, eliminando a duplicação do predicado nessa composição (`bin/codex_limite_estado.py:93`, `bin/codex_limite_estado.py:94`, `bin/codex_limite_estado.py:101`, `bin/codex_limite_estado.py:153`). O renderizador desempacota cinco campos corretamente (`bin/codex_limite_estado.py:486`); testes consultam a situação no índice 4 e rc no índice 3 (`bin/tests/test_codex_ociosidade.sh:67`, `bin/tests/test_codex_ociosidade.sh:71`). Entretanto, rc inválido do log continua sendo aceito e publicado, inclusive texto sintético se vier na entrada (`bin/codex_limite_estado.py:139`, `bin/codex_limite_estado.py:178`, `bin/codex_limite_estado.py:486`).

- **Bordas ainda diferentes entre consumidores**: módulo usa `(agora−24h,agora]` para limite, papéis e contexto horário (`bin/codex_limite_estado.py:59`, `bin/codex_limite_estado.py:83`, `bin/codex_limite_estado.py:164`, `bin/codex_limite_estado.py:283`), mas censo do disjuntor mantém ambas as bordas inclusivas (`bin/disjuntor.py:2132`). Além disso, disjuntor chama `ler_chamadas(raiz)` sem passar seu relógio (`bin/disjuntor.py:2036`), fazendo o leitor capturar outro `now` (`bin/codex_limite_estado.py:159`). Portanto, as classes **derivação paralela** e **relógio fora da borda** permanecem demonstradas nesses consumidores (`bin/disjuntor.py:2036`, `bin/disjuntor.py:2132`).

- **Fuso e precisão**: gravações de hora usam `date` sem offset, leitores usam `strptime` ingênuo e `datetime.now()` local (`bin/codex_pergunta.sh:272`, `bin/codex_pergunta.sh:308`, `bin/codex_pergunta.sh:456`, `bin/codex_limite_estado.py:63`, `bin/codex_limite_estado.py:124`, `bin/codex_limite_estado.py:235`). Igualdade efetiva entre os ambientes produtores/leitores: [nao verificado]. A correção resolve 30min59s, mas ainda trunca frações de segundo: `30min00.500s` vira 1800 e não acusa, embora a comparação final seja `s > 1800` (`bin/codex_limite_estado.py:51`, `bin/codex_limite_estado.py:243`, `bin/codex_limite_estado.py:296`).

## 17/11 09:32 · sessao daeb5df7e465 · E4C-F2 · verificador · origem=Code

1. **[nota] O mutador preserva exatamente a âncora atual.** A conferência AST em memória mostrou apenas a retirada de `q_nao_validada_pelo_ato()`: permanecem o OR da âncora, `pk__in`, relacionamentos e ordenação. Não há atributo com `'validad'` nos argumentos atuais da âncora; ali aparece `self.pk` (`app/chamados/models.py:355–359`; `app/chamados/tests/test_cortes_f2_f9_m13.py:28–43`).

   Para a forma antiga documentada, `validada_em__isnull=True`, a regra também remove somente esse keyword (`app/chamados/tests/test_cortes_f2_f9_m13.py:5`, `:39`). Aplicação ao código exato de **708162b15**: **[nao verificado]**; a sondagem dessa forma sintética não comprova a revisão histórica.

2. **[fila] AncoradasCustoTest distingue validação só no ATO, mas deixa sobreviver uma dependência indevida da projeção.** Agora exige excluir `so_no_ato`, mesmo com `validada_em=None` (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:487–494`). Contudo, este mutante no filtro externo ainda satisfaz custo e M13, por leitura das fixtures (`app/chamados/models.py:357`; `app/chamados/tests/test_cortes_f2_f9_m13.py:49–64`):

   ```python
   PerguntaDisputa.objects.filter(
       PerguntaDisputa.q_nao_validada_pelo_ato() & Q(validada_em__isnull=True),
       pk__in=_ancora,
   )
   ```

   Falta a divergência inversa: carimbo residual, VALIDACAO em T1 e RECUSA em T2; a pergunta deveria entrar, mas esse mutante a excluiria. O caso existe contra leitura/face, sem chamar `_ancoradas` (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:51–55`, `:105–109`). O seletor M13 ainda retiraria um único argumento composto e produziria a mordida esperada por PK (`app/chamados/tests/test_cortes_f2_f9_m13.py:33–43`, `:54–56`). Execução desse mutante: **[nao verificado]**.

3. **[fila] Há também um mutante sobrevivente da âncora:** reduzir o OR a `Q(chamado_destino_id=self.pk)`. Todas as perguntas desses dois testes usam `destino=ch`, deixando sem cobertura o ramo “sem destino, pelo chamado da disputa” (`app/chamados/models.py:355–356`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:484–488`; `app/chamados/tests/test_cortes_f2_f9_m13.py:49–62`). Esse ramo é exigido pelo juiz da âncora (`app/chamados/services/fio.py:100–102`, `:117`). Sobrevivência deduzida por leitura; execução **[nao verificado]**.

4. **[nota] No delta fornecido, não muda lógica produtiva além da docstring, nem se deslocam linhas de produção.** A substituição ocupa a mesma linha (`app/chamados/models.py:346`). O c12 inclui linhas na identidade, mas exclui arquivos de testes, onde ocorreram os acréscimos (`app/chamados/tests/e4b_c12_ast.py:17–27`, `:32–39`, `:222–223`). Executei, sem banco e sem escrita de bytecode, os dois testes do censo e corpo nominal: **2 passaram** (`app/chamados/tests/test_e4b_c12_selo.py:22`, `:34`). Suíte completa e efeito em produção: **[nao verificado]**.

## 17/11 09:31 · sessao db4eb04e80f0 · - · - · origem=Ronald (tmux)

[ fila ] Confirmado por leitura: o mutante abaixo em `app/chamados/models.py:357` passa nos dois testes normais:

```python
PerguntaDisputa.objects.filter(
    PerguntaDisputa.q_nao_validada_pelo_ato() & Q(validada_em__isnull=True),
    pk__in=_ancora,
)
```

As pendentes têm projeção nula; as validadas já saem pela face. Referências: `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:485-494`; `app/chamados/tests/test_cortes_f2_f9_m13.py:50-55,60-64`.

Com o seletor M13, `_incluir_validadas` remove o argumento posicional composto inteiro ao encontrar `q_nao_validada_pelo_ato`; conta uma retirada e conserva `pk__in`. Assim, continua produzindo a mordida esperada por PK. Referências: `app/chamados/tests/test_cortes_f2_f9_m13.py:33-43,54-56`.

O caso ausente é a **divergência inversa**: projeção `validada_em=T1`, VALIDACAO em T1 e RECUSA em T2. A pergunta deve entrar em `_ancoradas`, mas o mutante a exclui pelo carimbo residual. Esse caso existe somente contra leitura e face: `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:105-109,51-55`.

Adversarial mínimo: repetir esse caso com `destino=ch` e exigir seu PK em `ch._ancoradas()`. Execução [nao verificado].

## 17/11 09:30 · sessao db4eb04e80f0 · - · - · origem=Ronald (tmux)

1. [nota] **Sim, o teste agora distingue validação só no ato.** `so_no_ato` nasce com projeção vazia, recebe RESPOSTA e VALIDACAO, continua com `validada_em=None` e deve sair de `_ancoradas` junto com a validada alinhada. Referências: `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:487-494`; `app/chamados/tests/cortes_f2_f9_apoio.py:66-74`.

2. [fila] **A âncora ainda tem mutante sobrevivente nos dois testes.** Em `app/chamados/models.py:355-356`, substituir o filtro por:

   ```python
   _ancora = PerguntaDisputa.objects.filter(
       Q(chamado_destino_id=self.pk)).values('pk')
   ```

   preserva a subquery, o `pk__in`, a face e uma query (`app/chamados/models.py:357-359`). Todas as perguntas desses testes têm `destino=ch`, logo o mutante conserva seus resultados esperados: `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:484-494`; `app/chamados/tests/test_cortes_f2_f9_m13.py:49-55,59-64`. Entretanto, perde perguntas sem destino ancoradas pelo chamado da disputa, contrariando `app/chamados/services/fio.py:100-102,116-117`. Sobrevivência deduzida por leitura; execução [nao verificado].

   Para fechar essa lacuna, incluir pergunta sem destino com `disputa.chamado=ch`, além de pergunta destinada a outro chamado. A fixture atual cria a disputa sem preencher `chamado`: `app/chamados/tests/cortes_f2_f9_apoio.py:58-64`.

## 17/11 09:30 · sessao 3b3e67c2f1be · - · - · origem=Ronald (tmux)

3. **[nota] No delta fornecido, a única mudança produtiva é o texto da docstring**, na mesma linha; o corpo de `_ancoradas` permanece abaixo dela, sem deslocamento (`app/chamados/models.py:343-359`, especialmente `:346`). Comportamento produtivo adicional ao delta fornecido: [nao verificado].

4. **[nota] O selo c12 por linha não foi afetado por este delta.** Ele inclui o número da linha na identidade do `CORPO` (`app/chamados/tests/e4b_c12_ast.py:17-27,222-223`); conferência AST somente em memória encontrou todas as nove chamadas nas linhas cadastradas: 205, 302, 462, 667, 847, 865, 1501, 1705 e 1725 (`app/chamados/tests/e4b_c12_ast.py:19-27`). Os acréscimos nos dois testes não entram no universo produtivo: a pasta `tests` é excluída (`app/chamados/tests/e4b_c12_ast.py:32-39`). O mecanismo dos sítios também resolve a linha atual pelo AST, usando `linha_base` apenas para a fonte-base (`app/chamados/tests/e4b_c12_verificar.py:84-92,106-115`); o sítio E01 continua sendo a chamada de `fechar` (`app/chamados/tests/e4b_c12_sitios.json:370-376`; `app/chamados/models.py:464-467`).

Não executei a suíte c12; resultado integral de execução: [nao verificado]. [nao verificado]

## 17/11 09:27 · sessao 4ee5805a2bd9 · EXPORT-UM-JUIZ-DO-CODIGO · censo · origem=Code

**A divergência confere:** o export aceita qualquer código não vazio após `strip()`, enquanto o layout exige matrícula numérica em dez posições; qualquer erro de layout interrompe o TXT inteiro. `app/folha/export.py:840`, `app/folha/export.py:872`, `app/folha/export.py:273`, `app/folha/export.py:277`, `app/folha/views.py:275`.

**1. Escritores**

Contagem por sítio funcional que persiste o campo: **8 = duas atribuições explícitas + quatro regravações integrais + duas inicializações**. As portas comuns estão discriminadas abaixo, sem contá-las novamente.

| # | Sítio | O que grava |
|---|---|---|
| 1 | Admissão | `codigo_folha.strip()`, somente se não vazio; atribui e salva. `app/colaboradores/views.py:1131`, `app/colaboradores/views.py:1232`, `app/colaboradores/views.py:1237` |
| 2 | Edição | Mesmo tratamento; enviar vazio **preserva o código anterior**. `app/colaboradores/views.py:1353`, `app/colaboradores/views.py:1355`, `app/colaboradores/views.py:1365` |
| 3 | Vincular posto | `save()` integral regrava também o código carregado. `app/colaboradores/views.py:345`, `app/colaboradores/services/colaborador.py:160` |
| 4 | Transferir posto | Mesma regravação integral. `app/colaboradores/views.py:1418`, `app/colaboradores/services/colaborador.py:160` |
| 5 | Desligamento | Mesma regravação integral. `app/colaboradores/services/desligamento.py:133`, `app/colaboradores/services/colaborador.py:160` |
| 6 | Reativação | Regrava o objeto inteiro pela porta própria. `app/colaboradores/services/desligamento.py:247`, `app/colaboradores/services/colaborador.py:200` |
| 7 | Comando `smoke_chamados` | Cria colaborador de fixture sem código explícito, recebendo `default=''`. `app/chamados/management/commands/smoke_chamados.py:78`, `app/colaboradores/models.py:312` |
| 8 | Migration `0026` | Adiciona o campo com `default=''`; é `AddField`, não migration de dados com `RunPython`. `app/colaboradores/migrations/0026_colaborador_codigo_dominio.py:13` |

As persistências comuns são `salvar_campos`, `salvar_inteiro`, `salvar_reativacao`, `criar_colaborador` e, finalmente, `Colaborador.save()`. Não validam a forma do código. `app/colaboradores/services/colaborador.py:140`, `app/colaboradores/services/colaborador.py:152`, `app/colaboradores/services/colaborador.py:184`, `app/colaboradores/services/colaborador.py:204`, `app/colaboradores/models.py:415`.

As portas genéricas `gravar` e `atualizar_em_lote` também permitem esse campo na operação `cadastro`; a existência de outro chamador dinâmico que lhes entregue o código é **[nao verificado]**. `app/colaboradores/services/colaborador.py:46`, `app/colaboradores/services/colaborador.py:126`, `app/colaboradores/services/colaborador.py:164`.

O Django admin **não é escritor**: `ColaboradorAdmin` herda `SoLeitura`, que nega inclusão, alteração e exclusão. Os aliases usados para reexibir formulários recusados alteram apenas memória. `app/colaboradores/admin.py:48`, `app/core/admin.py:59`, `app/core/admin.py:62`, `app/core/admin.py:65`, `app/colaboradores/views.py:1117`, `app/colaboradores/views.py:1282`, `app/core/reexibir.py:21`.

Importador especializado do código ou outra migration de dados que o preencha: **[nao verificado]**.

**2. Leitores e ESMERIL-SEMÂNTICA**

Contagem: **20 sítios funcionais**, incluindo leitura do valor, projeção, matrícula serializada e admin; consumidores apenas de status/motivo ficam separados.

| # | Leitor | Suposição |
|---|---|---|
| 1 | `classificar_export` | Vazio após `strip()` fica fora; texto não vazio supera esse crivo. `app/folha/export.py:840`, `app/folha/export.py:872` |
| 2 | `montar_linhas_export` | Aplica `strip().zfill(10)`; não exige número nem trunca. `app/folha/export.py:728` |
| 3 | Pendências cadastrais | `None`, vazio e só espaços significam ausente. `app/colaboradores/services/cadastro.py:50`, `app/colaboradores/services/cadastro.py:82` |
| 4 | API `_cadastro_incompleto` | Somente `NULL` ou `''` significam ausente; espaços passam. `app/api/views_mensageria.py:1281` |
| 5 | Tela de edição | Exibe texto; vazio aparece vazio; JS exige preenchimento. `app/templates/colaboradores/editar.html:62`, `app/templates/colaboradores/editar.html:298` |
| 6 | Diferença entre exportações | Chave global `strip().zfill(10)`, sem recorte de empresa; colisão sobrescreve. `app/folha/haiku_export.py:115`, `app/folha/haiku_export.py:117` |
| 7 | Vigia do TXT | Ignora vazio após `strip()`; usa `zfill(10)` para associar motivo. `app/folha/services/vigia_txt.py:114` |
| 8 | Tradutor do PDF | Exige matrícula numérica no registro de 43 caracteres; resolve nome por empresa e código preenchido com zeros. `app/folha/pdf_export.py:112`, `app/folha/pdf_export.py:147` |
| 9 | PDF de validação | Código preenchido com zeros vira chave para ID; colisão sobrescreve. `app/folha/validacao_pdf.py:72` |
| 10 | Montagem da prévia | Copia código do juiz; vazio vira `''`. `app/folha/previa.py:100` |
| 11 | PDF de retidos | Exibição textual; vazio vira `'-'`. `app/folha/retidos_pdf.py:93` |
| 12 | Preview do export | Exibição textual escapada; vazio vira `'-'`. `app/folha/views.py:401` |
| 13 | Prévia de propostas | Código cru no CSV; escapado no HTML; sem validação própria. `app/folha/views.py:736`, `app/folha/views.py:742`, `app/folha/views.py:787`, `app/folha/views.py:811` |
| 14 | Retrato da folha | Copia para a chave `codigo`; vazio vira `''`. `app/folha/retrato_folha.py:1239` |
| 15 | Match configurável de holerite | Igualdade exata, com empresa quando há CNPJ; exige exatamente um candidato. `app/templates/holerite/upload_lote_v2.html:46`, `app/holerite/motor.py:218`, `app/holerite/motor.py:225`, `app/holerite/motor.py:298` |
| 16 | `montar_linha_10` | Converte para texto e aplica `zfill(10)`; não valida. `app/folha/export.py:36` |
| 17 | `partes_da_linha` | Reconhece matrícula numérica em dez posições de registro com 43 caracteres. `app/folha/export.py:247` |
| 18 | `validar_layout_dominio` | Exige comprimento 43 e matrícula `.isdigit()`. `app/folha/export.py:273`, `app/folha/export.py:277` |
| 19 | Agregador do export | Exige registro de 43 caracteres; usa a fatia da matrícula como chave sem exigir que seja numérica. `app/folha/export.py:313`, `app/folha/export.py:319` |
| 20 | Django admin | Exibe o campo como somente leitura, derivado dos metadados do modelo. `app/colaboradores/admin.py:48`, `app/core/admin.py:50`, `app/core/admin.py:68` |

O raio-x da folha, os relatórios de pendências e “quem cobrar”, e o tique do fechamento **herdam o julgamento**, sem verificar novamente o formato do código. `app/folha/views.py:589`, `app/relatorios/views_pend.py:49`, `app/relatorios/views_pend.py:58`, `app/relatorios/views.py:1660`, `app/ponto/services/fechamento_lista.py:29`.

Os selos de cartão/TXT e PDF/espelho selecionam quem o juiz declarou `entra`; o selo integrado delega à porta, cujo universo também usa esse status. `app/relatorios/management/commands/cartao_x_txt_divergentes.py:53`, `app/relatorios/management/commands/pdf_x_espelho_divergentes.py:58`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:33`, `app/folha/porta_export.py:469`.

**ESMERIL-SEMÂNTICA não fecha:** há texto de até vinte caracteres no modelo, presença de texto no export, matrícula numérica de dez posições no layout e conceitos diferentes de vazio na API e no cadastro. `app/colaboradores/models.py:311`, `app/folha/export.py:872`, `app/folha/export.py:277`, `app/api/views_mensageria.py:1281`, `app/colaboradores/services/cadastro.py:83`.

**3. Validação no cadastro e no modelo**

Os dois inputs são `type="text"`, sem `pattern` nem `maxlength`; os scripts os tornam obrigatórios. Não exigem número ou dez dígitos. `app/templates/colaboradores/admitir.html:58`, `app/templates/colaboradores/admitir.html:204`, `app/templates/colaboradores/editar.html:62`, `app/templates/colaboradores/editar.html:298`.

No servidor, obrigatórios são nome/CPF, com dispensa do CPF na edição; código ausente vira pendência. A lista antiga que contém `codigo_folha` deixou de ser o juiz da gravação. `app/colaboradores/services/cadastro.py:22`, `app/colaboradores/services/cadastro.py:53`, `app/colaboradores/services/cadastro.py:29`, `app/colaboradores/views.py:3225`.

A guarda genérica de tamanho **não cobre o alias normal da tela**: admissão omite o código do dicionário examinado; edição entrega `codigo_folha`, enquanto o helper procura o nome exato `codigo_dominio`. `app/colaboradores/views.py:1180`, `app/colaboradores/views.py:1298`, `app/core/validacao.py:49`.

O modelo declara `CharField(max_length=20, blank=True, default='')`, sem validator específico ou unicidade. Seu `save()` não valida o código. `app/colaboradores/models.py:311`, `app/colaboradores/models.py:400`, `app/colaboradores/models.py:415`.

**4. Exigência do layout e predicado atual**

`montar_linha_10` preenche a matrícula à esquerda até dez posições; código maior permanece maior. O validador exige registro de 43 caracteres, constante `11` e campos numéricos, incluindo matrícula nas posições `[2:12]`. `app/folha/export.py:36`, `app/folha/export.py:45`, `app/folha/export.py:273`, `app/folha/export.py:275`, `app/folha/export.py:277`.

O predicado exato de `sem_codigo_dominio` é:

```python
item['codigo_dominio'] = (colab.codigo_dominio or '').strip()
if not item['codigo_dominio']:
    # status='fora', motivo='sem_codigo_dominio'
```

Fontes: `app/folha/export.py:840`, `app/folha/export.py:872`. Esse ramo vem depois de rescisão; não pergunta número, tamanho ou duplicidade. `app/folha/export.py:866`, `app/folha/export.py:872`.

**Correção da premissa do anexo:** o layout não verifica repetição nem vazio original. Vazio montado diretamente vira dez zeros; repetição não é comparada entre colaboradores. `app/folha/export.py:36`, `app/folha/export.py:266`. Que ambos devam ser recusados é **[nao verificado]**.

**5. Juízes registrados e predicados vivos**

**Nenhum juiz específico registrado responde “código do Domínio válido?”** O registro aponta `classificar_export` para aprovação/entrada no TXT e para a leitura de tela; o documento aponta o mesmo juiz de entrada. `app/core/juizes.py:329`, `app/core/juizes.py:330`, `app/core/juizes.py:743`, `app/docs/JUIZES.md:118`.

Há **quatro implementações vivas de juízo sobre entrada/forma do código no TXT**, contando sítios, não expressões distintas:

- Presença após `strip()`: `app/folha/export.py:840`, `app/folha/export.py:872`.
- Matrícula numérica/largura no layout: `app/folha/export.py:273`, `app/folha/export.py:277`.
- Matrícula numérica/largura no parser comum: `app/folha/export.py:247`.
- Matrícula numérica/largura no parser do PDF: `app/folha/pdf_export.py:112`.

As verificações de completude cadastral, SQL e `required` do navegador são adicionais, discriminadas acima, fora dessa contagem de juízos de entrada/forma do TXT. `app/colaboradores/services/cadastro.py:83`, `app/api/views_mensageria.py:1281`, `app/templates/colaboradores/admitir.html:204`, `app/templates/colaboradores/editar.html:298`.

**6. Perguntas de lei**

**lei 0 para o PREPARA solicitado:** coerência e consumidor lendo a autoridade já têm lei escrita; juiz único já consta como padrão estrutural. `app/docs/LEIS.md:122`, `app/docs/LEIS.md:95`, `app/docs/CORTES.md:378`.

Política nova para duplicidade, novo motivo de retenção ou nascimento de nova autoridade não ficam decididos por este censo; sua autorização específica é **[nao verificado]**. Regra nova não pode ser deduzida apenas do código existente, e autoridade nova exige frase de nascimento no CORTES. `app/docs/LEIS.md:136`, `app/docs/LEIS.md:69`.

CENSO: escritores 8, leitores 20, predicados vivos 4, lei 0

## 17/11 09:26 · sessao c49d7242cbfa · CLASSE-D-NA-FRENTE-DA-E4D · leitura · origem=Code

A R1 continua por construir: C5 ainda sai sem `data`, e `dias_na_lista` continua aceitando apenas assinaturas datadas. O complemento LU2-07 inclui alteração no vigia; portanto, o cruzamento nominal é **1 com B2+delta e 1 com F2**. `app/escala/services/cadastro_realidade.py:180`, `app/escala/services/cadastro_realidade.py:1047`; listas anexadas [nao verificado].

A cópia não contém `.git` nem `logs/`; não executei git. A identidade do HEAD, os arquivos históricos citados e a correspondência das listas aos commits ficam [nao verificado]. Os cruzamentos abaixo usam os anexos fornecidos [nao verificado]. [nao verificado]

**1. Remedida do brief**

| Referência antiga | Sítio neste HEAD e resultado |
|---|---|
| BACKLOG:1942 | Registro agora em `app/docs/BACKLOG.md:1990`; continua “REGISTRADA, não construída”. |
| `colabs_nunca_bateu:137–173` | Confere: janela anterior até ontem, leitura de `CelulaDia`, exclusão pelo juiz de desligamento e retorno por colaborador com datas. `app/escala/services/cadastro_realidade.py:137`, `app/escala/services/cadastro_realidade.py:166`, `app/escala/services/cadastro_realidade.py:169`. |
| `_nunca_bateu:176–182` | Confere integralmente o defeito: um item por colaborador, sem `data` nem `dono_do_dia`; três datas no detalhe, depois reticências. `app/escala/services/cadastro_realidade.py:176`, `app/escala/services/cadastro_realidade.py:180`. |
| C9:342–355 | `_teto_do_dia` começa agora em 344; copia data, vínculo, template e dono da lavra. `app/escala/services/cadastro_realidade.py:344`, `app/escala/services/cadastro_realidade.py:350`. |
| C12:358–376 | `_cxr_do_motor` começa em 360. **Correção do brief:** admite item sem data, conforme publicado pelo motor; não garante sempre um item datado. `app/escala/services/cadastro_realidade.py:360`, `app/escala/services/cadastro_realidade.py:374`. |
| C10:383–397 | `_fase_deslocada` começa em 386; preserva leitura da lavra, data e dono. `app/escala/services/cadastro_realidade.py:386`, `app/escala/services/cadastro_realidade.py:393`. |
| `beco_em_lote:414`; queryset:450–453 | Função permanece em 414; queryset está em 452–455. Continua exigindo célula ativa, `indefinida` e protesto correspondente. `app/escala/services/cadastro_realidade.py:414`, `app/escala/services/cadastro_realidade.py:452`. |
| Guarda C11:469 | Agora em 472: continua excluindo dia sem pergunta viva ou com porta. Além da âncora terminal, admite M1: todas as vivas respondidas e sem âncora; exige folga não definida. `app/escala/services/cadastro_realidade.py:472`, `app/escala/services/cadastro_realidade.py:502`. |
| Montagem do dono:503–507; item:512 | Agora montagem em 510–511 e item em 516. Mantém `sem_motor=True`, CXR lavrado e cadastro carregado. `app/escala/services/cadastro_realidade.py:510`, `app/escala/services/cadastro_realidade.py:516`. |
| `lista:768`; C5:857–863 | `lista` começa em 773; C5 entra pelo `try/except` em 863–868. `app/escala/services/cadastro_realidade.py:773`, `app/escala/services/cadastro_realidade.py:863`. |
| `um_dono_por_dia:866` | Chamada agora em 871; definição em 971. Continua conciliando assinaturas datadas pelo mesmo juiz. `app/escala/services/cadastro_realidade.py:871`, `app/escala/services/cadastro_realidade.py:971`. |
| Laço:885–916; acesso perigoso:910–912 | Laço começa em 890; ramos C9/C10/C11/C12 em 892–909; acesso a `_extra['dono']` em 917. **O risco permanece:** acrescentar C5 ao conjunto sem seu ramo próprio causa `KeyError`, fora do `try` da produção C5. `app/escala/services/cadastro_realidade.py:890`, `app/escala/services/cadastro_realidade.py:915`. |
| `por_ass` ocorrências:941–947 | Agregado agora em 946–952; soma uma ocorrência por assinatura emitida. `app/escala/services/cadastro_realidade.py:921`, `app/escala/services/cadastro_realidade.py:946`. |
| `DONO_PELO_JUIZ:963` | Agora em 968, ainda somente C9/C10/C11/C12. `app/escala/services/cadastro_realidade.py:968`. |
| `_dono_resolvido:1014–1022` | Agora em 1019–1027; dono do juiz prevalece, divergência no mesmo nível devolve `None`. `app/escala/services/cadastro_realidade.py:1019`. |
| `dias_na_lista:1025–1045`; guarda:1042 | Agora começa em 1030; guarda de `data` em 1047. `app/escala/services/cadastro_realidade.py:1030`, `app/escala/services/cadastro_realidade.py:1047`. |
| C5:78; ação:671; chave no app:35 | Assinatura permanece em 78; ação mudou para 676–678, ainda `TELA_COLAB`; chave no app permanece em 35. `app/escala/services/cadastro_realidade.py:78`, `app/escala/services/cadastro_realidade.py:676`, `app/chamados/services/pergunta_no_app.py:35`. |

Os leitores e autoridades também mudaram de linha:

| Referência antiga | Sítio atual |
|---|---|
| `ListaDoAdmin:188–255` | Classe em 166; `dia_do_admin` em 200, `dia_na_lista` em 208, `colab_na_lista` em 226. `app/chamados/services/vigia_fio.py:166`, `app/chamados/services/vigia_fio.py:200`, `app/chamados/services/vigia_fio.py:208`, `app/chamados/services/vigia_fio.py:226`. |
| `dono_fora_do_fio:274–315`; fio admin:313 | Função em 240; encaminhamento ao fio do admin em 261–262. `app/chamados/services/vigia_fio.py:240`, `app/chamados/services/vigia_fio.py:261`. |
| `censo_alcance:542` | Começa em 487; universo em 537, vivas em 566, `ps` em 597, causa d em 638, causa e datada sem dono em 642–645, contadores em 656–659. `app/chamados/services/vigia_fio.py:487`, `app/chamados/services/vigia_fio.py:537`, `app/chamados/services/vigia_fio.py:566`, `app/chamados/services/vigia_fio.py:597`, `app/chamados/services/vigia_fio.py:638`, `app/chamados/services/vigia_fio.py:642`, `app/chamados/services/vigia_fio.py:656`. |
| Chamadores do censo | Conferem os sítios citados: `app/chamados/management/commands/vigiar_fio_morto.py:175`, `app/chamados/management/commands/vigiar_fio_morto.py:181`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:78`. |
| `porta_da_tarefa:638`; carga:648 | Agora em `app/chamados/services/tarefa_do_dia.py:712` e `app/chamados/services/tarefa_do_dia.py:728`. |
| `alimentar_portas:1595`; universo:109 | Agora em `app/chamados/juizes.py:1616` e `app/folha/fora_da_folha.py:125`. |
| Juiz e6:98–110; `InsumosDoDono:113–195` | Corpo decisório e classe conferem; a definição do juiz começa em 53. **A chamada exige o dia:** `dono(self, data)`, não `.dono()` sem argumento. `app/ponto/management/commands/e6_oraculo.py:53`, `app/ponto/management/commands/e6_oraculo.py:98`, `app/ponto/management/commands/e6_oraculo.py:113`, `app/ponto/management/commands/e6_oraculo.py:182`. |
| `sem_motor` sem CXR; gate:351–353 | Conferem; ausência de CXR pode produzir `None`. O gate do command continua separado dessa montagem do dono. `app/ponto/management/commands/e6_oraculo.py:189`, `app/ponto/management/commands/e6_oraculo.py:351`. |

Há duas correções materiais adicionais:

- **Leitores diretos:** `contador_nunca_bateu` expande as datas; `vigia_retido_calado.censo` usa apenas o conjunto de colaboradores. Ambos continuam consumindo `colabs_nunca_bateu`, mas o brief erra ao dizer que ambos leem por dia. `app/chamados/management/commands/disparar_perguntas_competencia.py:56`, `app/folha/vigia_retido_calado.py:71`.
- **Tela/PDF:** o agregado `ocorrencias` passaria a contar dias, mas a etiqueta da tela mostra `colabs`. Tela e PDF mostram `detalhe`, usando ocorrências somente como alternativa; não procede dizer que aquele agregado já determina o número visível nos dois. `app/escala/services/cadastro_realidade.py:949`, `app/templates/escala/cadastro_x_realidade.html:19`, `app/templates/escala/cadastro_x_realidade.html:28`, `app/escala/services/cadastro_realidade.py:1090`.

D1 persiste: C11 exige vivas, enquanto causa d exige ausência delas. Os consumidores compartilhados do predicado continuam nos sítios citados. `app/escala/services/cadastro_realidade.py:472`, `app/chamados/services/vigia_fio.py:638`, `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:82`, `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:105`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:51`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:54`.

Conferem o molde do teste de custo, os três sítios do motor e o padrão “lâmpada”; isso confirma sua existência, não uma execução dos testes neste preparo. `app/escala/tests/test_f3_beco1_folga_na_lista.py:395`, `app/ponto/motor_calculo_v2.py:1528`, `app/ponto/motor_calculo_v2.py:1623`, `app/ponto/motor_calculo_v2.py:1997`, `app/docs/GLOSSARIO.md:13`.

Quanto aos números:

| Número do brief | Remedida possível |
|---|---|
| 212 dias, 35 colaboradores; competências 97 + 115 | Constam no registro histórico; atualidade neste HEAD [nao verificado]. `app/docs/RELATO.md:393`. |
| Matriz `13+113+28+14+1+14+(7+4)+9+9=212` | A soma fecha no anexo; pertencimento de cada dia, inclusive os 23+5 dentro dos 28, [nao verificado]. |
| R1: 126 datados = 13+113; causa d 212→86 | Coerente com a matriz histórica; efeito atual e classificação individual [nao verificado]. Registro: `app/docs/RELATO.md:421`. |
| CADASTRO 22; sem divergência 13; ramos A/B/C = 168/26/18 | Constam no registro histórico; população atual [nao verificado]. `app/docs/RELATO.md:393`. |
| Emp1: 46 como subconjunto; alcance 80 | Registro histórico confirma a distinção; nenhuma recontagem pela função real foi possível [nao verificado]. `app/docs/RELATO.md:448`. |
| j4: 16 SEM_PORTA + 3 SEM_LEITURA = 19 | Contagem do raspado fornecido; correspondência desses 16 com os 126 alcançáveis pela R1 [nao verificado]. |

A pergunta “R1 sozinha ou espera LU2?” **já tem decisão técnica registrada: R1 sozinha**, mantendo D1/D2 em espera. A ordem antiga “CLASSE-D antes de F2” também já recebeu exceção: F2 abre sem esperar CLASSE-D. `app/docs/RELATO.md:421`, `app/docs/RELATO.md:4`.

`competencia_ja_paga` continua registrada como obra; implementação com esse nome nesta cópia [nao verificado]. Não cabe substituí-la por predicado paralelo. `app/docs/BACKLOG.md:1992`, `app/docs/LEIS.md:60`.

O complemento LU2-07 ainda encontra o vigia lendo `empresas_visiveis`, sem `em_rollout`. Cadastro, tela e literal do cron conferem; alterar a consulta global alcançaria o lavrador da folha. `app/chamados/services/vigia_fio.py:517`, `app/colaboradores/queries.py:85`, `app/colaboradores/models.py:45`, `app/templates/colaboradores/empresa_form.html:35`, `app/config/crons.py:17`, `app/folha/management/commands/lavrar_retrato_folha.py:51`.

**2. LU2-08 que permanece aberta e toca a obra**

- **PARCIAL_SEM_PERIODO:** a lei já responde **CADASTRO**, sem julgar batida dentro/fora e nunca FATO_EM_AUSENCIA. A referência antiga `CORTES.md:370` está agora em `app/docs/CORTES.md:386`.
- **Implementação pendente:** o registro manda levar esse insumo por `InsumosDoDono.dono()`, antes da decisão BATIDA; registra 14 parciais ativos, 10 da CLASSE-D, e resultado diferente se introduzido depois. Esses quantitativos atuais são [nao verificado]. `app/docs/RELATO.md:422`. O juiz atual ainda não recebe insumo específico dessa parcial. `app/ponto/management/commands/e6_oraculo.py:53`, `app/ponto/management/commands/e6_oraculo.py:185`.
- **FATO_EM_AUSENCIA:** segue sem corte entre CADASTRO, BATIDA e ESTRUTURA; registro menciona 10 na classe e 57 na frota, cuja atualidade é [nao verificado]. `app/docs/RELATO.md:352`, `app/docs/RELATO.md:422`.
- O número histórico 21 significa 10 parciais + 10 FATO_EM_AUSENCIA + 1 colaborador de teste; não são 21 casos igualmente abertos. `app/docs/RELATO.md:352`.

Para col265, col838 e col407, a origem individual do desconhecimento é **não recuperável pelo raspado fornecido** [nao verificado]. O código publicar `nao_sei` sem rota não identifica qual caminho ocorreu para cada pessoa. `app/chamados/services/tarefa_do_dia.py:719`.

**3. Cruzamento por arquivo**

Sem `.git`, estas são interseções **nominais das listas anexadas**, não diffs autenticados [nao verificado].

| Arquivo que a obra tocaria | B2+delta | F2 | Fundamento do escopo |
|---|---:|---:|---|
| `app/escala/services/cadastro_realidade.py` | 0 | 0 | R1 registrada em `app/docs/RELATO.md:421`; presença nas listas [nao verificado]. |
| Teste R1 em `app/escala/tests/`, nome não definido | — | — | Nome e eventual colisão [nao verificado]; nenhuma entrada desse diretório nas listas anexadas [nao verificado]. | [nao verificado]
| **`app/chamados/services/vigia_fio.py`** | **1** | **1** | Complemento LU2-07 do anexo [nao verificado]; sítio atual em `app/chamados/services/vigia_fio.py:517`; presença nas duas listas [nao verificado]. |
| `app/ponto/management/commands/e6_oraculo.py`, se incorporar LU2-08 | 0 | 0 | Complemento registrado em `app/docs/RELATO.md:422`; presença nas listas [nao verificado]. |
| Testes dos complementos | — | — | Arquivos não nomeados [nao verificado]. | [nao verificado]

O delta nomeia `app/chamados/reconciliador.py` e seus testes; o escritor existe em `app/chamados/reconciliador.py:504`, com transição em `app/chamados/reconciliador.py:524`. Nenhum desses arquivos foi nomeado como alvo da CLASSE-D; nomes dos testes do delta [nao verificado].

Não acrescentei `config/crons.py` ao escopo: o complemento manda registrar sua futura migração, não executá-la nesta obra [nao verificado: anexo]. Também não há obrigação demonstrada de editar o template para rotular o detalhe diário, pois ele lê o texto produzido. `app/templates/escala/cadastro_x_realidade.html:28`.

**Contagem nominal conhecida: B2+delta 1; F2 1**, ambos pelo mesmo `app/chamados/services/vigia_fio.py` [nao verificado: listas anexadas]. R1 isolada daria 0/0; o brief completo inclui o complemento local LU2-07 [nao verificado: anexo].

**4. Perguntas de lei**

**lei: qual dono deve responder por FATO_EM_AUSENCIA — CADASTRO, BATIDA ou ESTRUTURA?** O registro mantém expressamente essa escolha aberta; L-099 define os donos e destinos, mas não resolve essa classe específica. `app/docs/RELATO.md:352`, `app/docs/RELATO.md:422`, `app/docs/LEIS.md:122`.

Para o recorte R1 + exclusão local LU2-07 + parcial sem período, não identifiquei outra lei faltante: origem e autoridade única estão escritas; parcial sem período já é CADASTRO; R1 independente já foi decidida. D1/D2 permanecem desenho reservado, sem solução presumida neste preparo. `app/docs/LEIS.md:59`, `app/docs/LEIS.md:60`, `app/docs/CORTES.md:386`, `app/docs/RELATO.md:421`.

A contagem final considera o preparo integral e sua pendência LU2-08; não declara os 16 SEM_PORTA resolvidos. `app/docs/RELATO.md:422`; cobertura individual [nao verificado].

PREPARO: lei 1; cruzamento B2+delta 1, F2 1 -- app/chamados/services/vigia_fio.py [nao verificado: listas anexadas]

## 17/11 09:26 · sessao 1b03fefdaae1 · E4C-F2 · verificador · origem=Code

1. **[bloqueia] O delta quebra a aplicação do mutante M13.** `_incluir_validadas` exige exatamente um `PerguntaDisputa.objects.filter`; agora `_ancoradas` contém dois. Reproduzido com AST em memória: ocorre `RuntimeError: mutante m13_inclui_validadas sem filtro unico; isto nao e mordida`, antes da asserção de negócio. O próprio apoio proíbe contar esse erro como mordida. Fontes: `app/chamados/models.py:355`, `app/chamados/models.py:357`, `app/chamados/tests/test_cortes_f2_f9_m13.py:25`, `app/chamados/tests/test_cortes_f2_f9_m13.py:31`, `app/chamados/tests/cortes_f2_f9_apoio.py:81`, `app/chamados/tests/cortes_f2_f9_apoio.py:93`.

   O GREEN normal não revela a quebra: sem o seletor do mutante, o transformador não roda. É necessário adaptar o mutador para retirar somente a face, conservar a âncora e voltar a morrer na asserção de negócio. Fontes: `app/chamados/tests/cortes_f2_f9_apoio.py:84`, `app/chamados/tests/test_cortes_f2_f9_m13.py:45`.

2. **[nota] O conjunto e a ordem são equivalentes, por inspeção do código.** A transformação conserva `face(p) AND âncora(p)`: a subquery enumera as PKs pela mesma âncora, e a face continua aplicada à pergunta externa. Fontes: `app/chamados/models.py:355`, `app/chamados/models.py:357`, `app/chamados/models.py:3513`.

   | Caso | Resultado nas duas formas |
   |---|---|
   | Destino é outro chamado, mas `disputa.chamado=self` | Fora: o fallback exige destino nulo. `app/chamados/models.py:356` |
   | Destino nulo e `disputa.chamado=self` | Entra se passar pela face. `app/chamados/models.py:356`, `app/chamados/models.py:357` |
   | Destino nulo e `disputa.chamado` nulo | Fora. Esse chamado da disputa pode ser nulo. `app/chamados/models.py:356`, `app/chamados/models.py:1835` |
   | Destino nulo e própria disputa nula | Fora; além disso, `PerguntaDisputa.disputa` não admite nulo no modelo. `app/chamados/models.py:2552`, `app/chamados/models.py:356` |

   Não há join multiplicador nessa composição: a âncora percorre FK, a face usa `Exists` e os relacionamentos carregados são FKs. Portanto o `IN` não elimina duplicatas que a versão anterior produzisse nesse caminho. Fontes: `app/chamados/models.py:2552`, `app/chamados/models.py:3503`, `app/chamados/models.py:3505`, `app/chamados/models.py:358`.

   Permanecem o mesmo `select_related`, a materialização imediata por `list` e a ordenação explícita por `pk`. A subquery não é avaliada separadamente: há um statement para chamado com PK; sem PK, retorna `[]` antes da consulta. Fontes: `app/chamados/models.py:348`, `app/chamados/models.py:355`, `app/chamados/models.py:357`, `app/chamados/models.py:358`, `app/chamados/models.py:359`.

3. **[nota] Há cinco chamadores diretos de produção.**

   | Chamador | Local da chamada |
   |---|---|
   | `_gravar_estado` | `app/chamados/models.py:229` |
   | `_vivas_ancoradas` | `app/chamados/models.py:341` |
   | `_plano_pergunta_viva` | `app/chamados/models.py:393` |
   | `_perguntas_relavradas_ou_ressuscitadas` | `app/chamados/services/disputa_emissao.py:1821` |
   | `_resolver_perguntas_por_chamado_resolvido` | `app/chamados/services/disputa_emissao.py:1962` |

   Os testes também chamam diretamente: `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:411`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:488`, `app/chamados/tests/test_cortes_f2_f9_m13.py:45`, `app/chamados/tests/test_cortes_f2_f9_m13.py:54`.

   Nenhum desses consumidores recebe um QuerySet preguiçoso ou acrescenta `select_for_update`: recebem a lista pronta. O cache plano→núcleo continua reaproveitando essa lista. Fontes: `app/chamados/models.py:357`, `app/chamados/models.py:222`, `app/chamados/models.py:229`, `app/chamados/models.py:394`.

   Há dependência de contagem nos selos: o novo exige uma consulta; o chokepoint mede `reconciliar` sem perguntas e cruzando a fronteira com três perguntas. A subquery preserva a estrutura de um statement, mas o resultado desses testes nesta execução é **[nao verificado]**. Fontes: `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:489`, `app/ponto/tests/test_selo_chokepoint_escrita.py:363`, `app/ponto/tests/test_selo_chokepoint_escrita.py:369`.

4. **[fila] O teste novo morde a forma anterior, mas não distingue ato de projeção.** [nao verificado]

   **Voltar ao OR no mesmo WHERE:** falha, por inspeção, na exigência literal de `"chamados_perguntadisputa"."id" IN (SELECT`. A face contém `Exists`, não esse `IN` da âncora. Fontes: `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:493`, `app/chamados/models.py:3503`, `app/chamados/models.py:3505`.

   **Trocar somente a face por `Q(validada_em__isnull=True)`, conservando a subquery:** esse mutante passa no teste novo, por inspeção. As únicas fixtures são uma pergunta sem carimbo e outra com carimbo; ambas fazem ato/passivo e projeção concordarem. Fontes: `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:484`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:486`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:490`, `app/chamados/models.py:3509`.

   Isso não significa sobrevivência à suíte inteira: o M13 normal já exige excluir uma pergunta validada somente no ato, com projeção vazia. Essa asserção distingue as fontes; seu mecanismo de certificação por mutante é que foi quebrado pelo delta, conforme achado 1. Fontes: `app/chamados/tests/test_cortes_f2_f9_m13.py:42`, `app/chamados/tests/test_cortes_f2_f9_m13.py:43`, `app/chamados/tests/test_cortes_f2_f9_m13.py:45`. Execução Django dos dois mutantes: **[nao verificado]**.

5. **[nota] Checklist das classes de reprovação, restrito a este delta.**

   | Classe | Avaliação |
   |---|---|
   | Segundo juiz duplicando autoridade | Não introduzido: o retorno continua chamando a mesma face, que nega `q_validada_pelo_ato`. `app/chamados/models.py:357`, `app/chamados/models.py:3515` |
   | Segundo escritor fora da porta | Não introduzido pelo método alterado: ele apenas consulta. As nove identidades de `CORPO` correspondem ao AST real; `EXCECOES` segue vazio. `app/chamados/models.py:355`, `app/chamados/tests/e4b_c12_ast.py:18`, `app/chamados/tests/e4b_c12_ast.py:29` |
   | Relógio fora da borda | Nenhuma leitura de relógio acrescentada no caminho alterado. `app/chamados/models.py:348`, `app/chamados/models.py:355`, `app/chamados/models.py:359` |
   | Derivação paralela de fonte única | Não acrescentada: a subquery trata somente da âncora; a validação permanece na face existente. `app/chamados/models.py:355`, `app/chamados/models.py:357` |
   | Selo/RED sem mordida | **Reprovado na aplicação do M13**; o novo teste tem mordida estrutural, mas admite o mutante da projeção isoladamente. `app/chamados/tests/test_cortes_f2_f9_m13.py:31`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:486`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:493` |
   | Ordem de trava | Nenhuma inversão introduzida: `_ancoradas` continua leitura simples; as travas explícitas seguem na porta, na ordem Colaborador→Disputa→Chamado→Pergunta. `app/chamados/models.py:357`, `app/chamados/models.py:2802`, `app/chamados/models.py:2811`, `app/chamados/models.py:2824`, `app/chamados/models.py:2828`, `app/chamados/services/disputa_emissao.py:1902` |
   | Campo com dois significados | Nenhum significado novo introduzido. Há documentação residual incorreta: `_ancoradas` ainda descreve seu universo como `validada_em` nulo, embora use a sequência do ato. `app/chamados/models.py:346`, `app/chamados/models.py:357` |

6. **[nota] O ganho de desempenho não está provado pelo dado fornecido.** Tempos, estimativa de linhas e custo de JIT aparecem como alegações nos comentários; o teste cobra quantidade, resultado e fragmento SQL, sem medir plano ou tempo. Ganho de 1,8 s→0,02 s, igualdade observada em produção e execução física da face “só sobre a âncora”: **[nao verificado]**. Fontes: `app/chamados/models.py:352`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:476`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:487`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:493`.

## 17/11 09:25 · sessao c83ef10efcdf · - · - · origem=Ronald (tmux)

Contagem proposta: **20 leitores funcionais = 15 leitores de valor/projeção + 4 funções da matrícula serializada + admin genérico**, conforme os sítios enumerados abaixo. Não inclui leitores apenas de status/motivo, testes, documentação ou POST para reexibição. [app/folha/export.py:728; app/core/admin.py:50]

| # | Leitor | Suposição |
|---|---|---|
| 1 | `classificar_export`, app/folha/export.py:840 e :872 | Normaliza `(codigo_dominio or '').strip()`; vazio fica fora por `sem_codigo_dominio`; qualquer texto não vazio supera esse crivo. |
| 2 | `montar_linhas_export`, app/folha/export.py:728 | `strip().zfill(10)`; completa com zeros, não limita a dez caracteres nem exige número. |
| 3 | `pendencias_do_colaborador`, app/colaboradores/services/cadastro.py:74 | Alias em :50; ausente se `None`, `''` ou string só de espaços (:82-85); não exige número/tamanho. |
| 4 | API `_cadastro_incompleto`, app/api/views_mensageria.py:1273 | Só `NULL` ou `''` são ausentes (:1281); espaços passam. |
| 5 | Template de edição, app/templates/colaboradores/editar.html:62 | Mostra código como texto; vazio usa `''`; JS torna obrigatório (:298-299), sem exigir número/tamanho. |
| 6 | `diferenca_entre_exportacoes`, app/folha/haiku_export.py:115 | Exclui código `''`/NULL e monta chave global `strip().zfill(10)` (:117); repetidos sobrescrevem no dicionário, sem filtro de empresa. |
| 7 | `_motivo_por_matricula`, app/folha/services/vigia_txt.py:98 | Lê projeção; código vazio após strip é ignorado; demais viram chave `zfill(10)` (:114-116). |
| 8 | `traduzir_texto`, app/folha/pdf_export.py:69 | Parser exige reg10 de 43 caracteres, const11 e matrícula `.isdigit()` (:112-115); volta matrícula→nome por empresa com `strip().zfill(10)` (:147-150), sem checar colisões. |
| 9 | `gerar_pdf_validacao_bytes`, app/folha/validacao_pdf.py:58 | Dicionário matrícula→ID usa `str(código or '').strip().zfill(10)` (:70-72); vazio vira zeros e colisão sobrescreve. |
| 10 | `montar_previa`, app/folha/previa.py:28 | Copia a projeção do juiz, vazio→`''`; não valida (:100). |
| 11 | `gerar_pdf_retidos_bytes`, app/folha/retidos_pdf.py:41 | Exibição textual; vazio→`'-'`, sem validação (:93). |
| 12 | `preview_exportacao`, app/folha/views.py:313 | Exibe a projeção textual escapada, vazio→`'-'` (:401). |
| 13 | `previa_proposta`, app/folha/views.py:701 | CSV usa valor cru (:736, :742); HTML usa código escapado, vazio→`'-'` (:787, :811). CSV/HTML contam como uma função. |
| 14 | `montar_linhas`, app/folha/retrato_folha.py:1174 | Copia projeção para chave `codigo`, vazio→`''`, sem validar (:1239). |
| 15 | `_casar` do holerite configurável, app/holerite/motor.py:177 | Selecionável `codigo_dominio` no template upload_lote_v2.html:46 e view app/holerite/views.py:1741; `processar` aceita override (:298); filtro de igualdade com empresa quando há CNPJ, contando 0/1/mais de um candidato (:218-230). Padrão Domínio atual usa nome completo (:161). |
| 16 | `montar_linha_10`, app/folha/export.py:34 | `str(matricula).zfill(10)` (:36), sem validar número ou truncar. |
| 17 | `partes_da_linha`, app/folha/export.py:224 | Reg10 só é reconhecido com 43 caracteres e matrícula de dez posições `.isdigit()` (:247-252). |
| 18 | `validar_layout_dominio`, app/folha/export.py:266 | Reg10 exige 43 caracteres (:273-274) e posições 2:12 numéricas (:277-279). Não verifica código repetido ou vazio na origem. |
| 19 | `agregar_export_dominio`, app/folha/export.py:310 | Só agrega reg10 de 43 caracteres (:313); matrícula é fatia 2:12 usada como chave (:319); não verifica que matrícula seja numérica. |
| 20 | Django admin de Colaborador, app/colaboradores/admin.py:47 | Registro `ColaboradorAdmin(SoLeitura, ModelAdmin)` (:48); campos derivados integralmente de `_meta.fields` em app/core/admin.py:46-50 e tornados readonly em :68-75. O dado é visível; add/change/delete negados (:59-66). |

**Consumidores do julgamento, fora dos 20:** raio-x da folha chama juiz e usa status/motivo, sem revalidar código (app/folha/views.py:589-600); relatório pendências cadastrais herda especificamente `sem_codigo_dominio` (app/relatorios/views_pend.py:49 e :53-59); “quem cobrar” herda o juiz e traduz o motivo (app/relatorios/views.py:1660-1671); tique da lista de fechamento traduz motivo do juiz (app/ponto/services/fechamento_lista.py:25-36). Selos `cartao_x_txt_divergentes` e `pdf_x_espelho_divergentes` selecionam `status='entra'`, sem validar código (app/relatorios/management/commands/cartao_x_txt_divergentes.py:53-58; app/relatorios/management/commands/pdf_x_espelho_divergentes.py:58-61); selo integrado delega a `porta_export.medir` (app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:33-38), cujo universo também é `status='entra'` (app/folha/porta_export.py:469-471).

**Suportes dinâmicos separados:** `preencher_em_memoria` recebe alias `codigo_folha→codigo_dominio`, mas escreve somente na cópia em memória para reexibir POST (app/colaboradores/views.py:1116-1117, :1281-1282; app/core/reexibir.py:21-49). Template de admissão lê apenas o POST, não um código persistido (app/templates/colaboradores/admitir.html:58), e também impõe `required` via JS (:204-205).

**Guarda genérica de tamanho não cobre o campo na tela normal:** admissão omite código de `_dados_tam` (app/colaboradores/views.py:1180-1185); edição passa o nome do POST `codigo_folha` sem remapeamento (app/colaboradores/views.py:1298-1300), enquanto `erros_de_tamanho` só examina campos cujo nome do modelo está no dicionário (app/core/validacao.py:49-54). Portanto `max_length=20` não equivale a validação da porta de cadastro para esse alias (app/colaboradores/models.py:311-314).

**Predicados principais de aptidão/forma TXT: quatro implementações vivas:** não vazio após strip (`classificar_export`, app/folha/export.py:840 e :872); matrícula numérica/largura via layout (`validar_layout_dominio`, app/folha/export.py:273-279); matrícula numérica/largura via parser comum (`partes_da_linha`, app/folha/export.py:247-252); matrícula numérica/largura via parser do PDF (`traduzir_texto`, app/folha/pdf_export.py:112-115). As perguntas de completude cadastral, SQL da API e `required` do navegador são decisões adicionais com semânticas diferentes (app/colaboradores/services/cadastro.py:82-85; app/api/views_mensageria.py:1281; app/templates/colaboradores/editar.html:298-299).

ESMERIL-SEMÂNTICA não fecha: cadastro/modelo comportam texto de até vinte caracteres, export usa existência de texto, layout exige matrícula numérica de dez posições e API distingue espaços de vazio de forma diferente do juiz (app/colaboradores/models.py:311-314; app/folha/export.py:840, :872, :277-279; app/api/views_mensageria.py:1281).

Selo de teste existente confirma apenas código vazio fora; não demonstra que texto não numérico seja barrado (app/folha/tests/test_classificar_export.py:40-45). Testes não foram executados. [não verificado]

## 17/11 09:25 · sessao f62e78945988 · E4C-F1-B2 · verificador · origem=Code

**AVAL-DEVOLVIDO — item 6:** `savepoint + raise`, sozinho, não prova “nada de folha muda”: o fechamento agenda efeitos por `on_commit`, e o medidor existente recusa justamente a retomada de regularizações. (`app/chamados/signals.py:146`; `app/chamados/services/foto_e4b.py:37`.)

Nas referências ao aval transcrito, conto “aval Ronald…” como linha 1; os itens 1/2/3/6/7 ficam nas linhas 2/3/4/7/8. Existência dos arquivos originais dos anexos nesta cópia: **[nao verificado]**.

**Item 1 — CONFERE, com obrigações concretas de implementação.**

1. **Premissas:** existe `veto_de_competencia(empresa, dias, memo)`. Recebe empresa, coleção de datas e dicionário de cache; não recebe nem seleciona perguntas. Retorna `None` se qualquer data for desconhecida; caso contrário, `True` se alguma competência estiver entregue, ou `False`. Não contém recorte por motivo, família ou fora163. (`app/chamados/services/fio.py:1357`; `app/chamados/services/fio.py:1365`.)

   Ele compõe **dia → janela da empresa → K6**. K6 pergunta pela existência de `ExportacaoDominio` vigente não invalidada; C21 pergunta se há instante **e** texto após trim. Portanto, é **o mesmo K6, não um segundo predicado de entrega; C21 é outra pergunta**. (`app/chamados/services/vigia_fio.py:802`; `app/folha/export.py:588`; `app/core/juizes.py:306`; `app/core/juizes.py:343`; `app/chamados/juizes.py:1552`.)

2. **Colisão:** consultar essa composição no escritor respeita juiz único. Copiar `_k6_que_barra` perderia alcance, porque ela consulta somente metades de intervalo fora163. A cerca específica também não transforma K6 em veto universal de escrita: L-113 alterou L-092 e revogou a recusa genérica de recálculo em exportada. (`CLAUDE.md:182`; `app/chamados/services/materializacao.py:792`; `app/chamados/services/materializacao.py:805`; `app/docs/LEIS.md:155`.)

3. **Literal e unidade:** a unidade externa é `transaction.atomic()` em `_retratar:524`, abrangendo transição, rejeição da justificativa, resolver e ocorrência. A retenção atual ocorre **antes** desse bloco e pode registrar auditoria. `transicionar_com_pauta` está em `services/fecho_runtime.py:146`; chama `ch.reconciliar` e pode persistir Pauta antes de retornar. A consulta precisa anteceder essa chamada. (`app/chamados/reconciliador.py:520`; `app/chamados/reconciliador.py:524`; `app/chamados/reconciliador.py:531`; `app/chamados/reconciliador.py:538`; `app/chamados/services/fecho_runtime.py:165`; `app/chamados/services/fecho_runtime.py:168`.)

   O censo mínimo de alcance é:

   | Ramo | Seleção/efeito que precisa entrar |
   |---|---|
   | Resolver direto | `chamado_destino=ch`, materialização nula, disputa aberta; inclui validadas. (`app/chamados/services/disputa_emissao.py:1928`; `app/chamados/services/disputa_emissao.py:1950`.) |
   | Ancoradas/reabertura | Destino igual ao chamado ou destino nulo com pai da disputa igual ao chamado; reabrir pode ressuscitar e relavrar. Existe enumerador específico desse ramo. (`app/chamados/models.py:344`; `app/chamados/services/disputa_emissao.py:1814`; `app/chamados/services/disputa_emissao.py:1969`.) |
   | Relavra | Destino igual ao chamado, validação nula, **sem filtro de materialização ou disputa aberta**. (`app/chamados/models.py:303`; `app/chamados/models.py:309`.) |
   | S143/HX | Todas as disputas abertas do colaborador, inclusive perguntas sem destino de outras disputas órfãs; pode carimbar e fechar. (`app/chamados/services/disputa_emissao.py:2048`; `app/chamados/services/disputa_emissao.py:2057`; `app/chamados/services/disputa_emissao.py:2082`; `app/chamados/services/disputa_emissao.py:2114`.) |
   | Cascata | Fechar disputa pode reconciliar outro pai; saves de chamado e justificativa também acordam cartório. (`app/chamados/signals.py:43`; `app/chamados/signals.py:218`; `app/ponto/signals.py:165`.) |

   Logo, unir apenas “perguntas do destino” e “perguntas da disputa original” é insuficiente. A enumeração precisa acompanhar a cascata dos escritores, deduplicando identidades; suficiência do futuro delta: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:2050`; `app/chamados/signals.py:43`.)

   Duas armadilhas fecham sem juiz novo: passar **lista/tupla**, porque os dois `any` consomem um gerador; e tratar `None` explicitamente. Em `[None, dia_ENTREGUE]`, o retorno é `None`, portanto `if veto:` falharia. Já existe o padrão conservador `is not False`. (`app/chamados/services/fio.py:1365`; `app/chamados/services/fio.py:1367`; `app/chamados/services/fio.py:1116`; `app/chamados/services/fio.py:1118`.)

4. **Exportada/dinheiro/!:** a cerca antes da transição preserva os efeitos declarados, inclusive evita chegar à escrita de Pauta desse ato. Não prova ausência de efeitos financeiros nos caminhos permitidos, que ainda podem fechar disputas e agendar retomadas; isso depende da medida do item 6. (`app/chamados/services/fecho_runtime.py:168`; `app/chamados/services/disputa_emissao.py:2114`; `app/chamados/signals.py:146`.)

**Item 2 — CONFERE; o indireto é montável, mas o RED ainda não está demonstrado.**

1. **Premissas:** há dois desenhos concretos para (b):

   - **Receiver, sem vínculo direto:** pergunta sem destino em outra disputa órfã do mesmo colaborador, C6 morta, com dia pertencente a competência entregue. S143 a alcança pela varredura global. A fixture existente já constrói a órfã sem destino e verifica seu carimbo/fechamento; a adaptação para ENTREGUE e entrada por `_retratar` ainda é **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:2048`; `app/chamados/tests/test_e4b_c6_orm.py:82`; `app/chamados/tests/test_e4b_c6_orm.py:86`; `app/chamados/tests/test_e4b_c6_orm.py:120`.)
   - **Relavra, fora do resolver direto:** pergunta ENTREGUE com destino no chamado, validação nula, materialização preenchida e disputa fechada. Fica fora do resolver direto, mas pode entrar na relavra da travessia de estado. Mesmo sem mudar o veredito, a lavra escreve `julgado_em`; relógio posterior ao snapshot torna o efeito observável. Execução desse cenário: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:1928`; `app/chamados/models.py:303`; `app/chamados/models.py:309`; `app/chamados/veredito_lavrado.py:270`.)

2. **Colisão:** exigir RED anterior à cura e mutante que morde coincide com LEI-AKITA; nenhuma colisão identificada nesse requisito. (`CLAUDE.md:28`; `CLAUDE.md:194`.)

3. **Literal:** “nada muda” precisa ser medido na **unidade de `_retratar`**. O consumidor do evento já rejunta células e carimba perguntas antes dela, além de executar outros escritores depois. A cerca localizada não garante imutabilidade do evento inteiro. Os casos (a)/(b)/(c), os snapshots e a mordida do mutante ainda são **[nao verificado]**. (`app/chamados/reconciliador.py:1366`; `app/chamados/reconciliador.py:1368`; `app/chamados/reconciliador.py:1378`; `app/chamados/reconciliador.py:1386`.)

4. **Exportada/dinheiro/!:** o teste deve verificar também lavra e efeitos indiretos; conferir apenas `materializada_em` deixaria passar escrita em ENTREGUE pela relavra. Resultado financeiro real: **[nao verificado]**. (`app/chamados/veredito_lavrado.py:270`; `app/chamados/veredito_lavrado.py:275`.)

**Item 3 — CONFERE.**

1. **Premissas:** construtor → verificador → suíte inteira → opus final corresponde ao fluxo vigente. (`CLAUDE.md:362`; `CLAUDE.md:372`; `CLAUDE.md:378`.) [nao verificado]
2. **Colisão:** reutilizar K6 e conservar C21 respeita as autoridades registradas e a proibição de duplicação/fallback. (`app/core/juizes.py:306`; `app/core/juizes.py:343`; `CLAUDE.md:17`.)
3. **Literal:** “opus final só do delta” não pode excluir escritores e leitores dos campos/funções atingidos: a regra inclui esses sítios mesmo fora do diff. Commit, suíte e aprovação futuros: **[nao verificado]**. (`CLAUDE.md:379`; `CLAUDE.md:380`.) [nao verificado]
4. **Exportada/dinheiro/!:** o procedimento de construção não amplia autorização de aplicação; o escopo e as condições continuam literais. (`CLAUDE.md:37`; `CLAUDE.md:265`.)

**Item 6 — PROCEDE.**

1. **Premissas:** as três condições são verificáveis em princípio, mas o dado fornecido não apresenta opus final do delta, hash antes/depois do `FechamentoMensal` exportado nem resultado da medida do item 4: **[nao verificado]**. Há fotografia dos modelos financeiros e captura de DML aproveitáveis para a medida. (`app/chamados/services/foto_e4b.py:18`; `app/chamados/services/foto_e4b.py:87`.)

2. **Colisão:** o caráter condicional do pouso coincide com a regra vigente. Tratar o aval como `!` já liberado, sem fechar as três condições, colidiria com o escopo literal. (`anexo corte_b2_cerca_no_escritor_1711.txt:7`; `CLAUDE.md:37`; `CLAUDE.md:265`.) [nao verificado]

3. **Condição que não fecha com o protocolo simples:** `fechar` salva a disputa; o receiver agenda retomada de regularizações confirmadas do colaborador. Essa retomada pode criar batida e agendar recálculo, inclusive em competência exportada. Esses efeitos não rodam automaticamente numa transação terminada por rollback. (`app/chamados/models.py:2027`; `app/chamados/signals.py:146`; `app/chamados/services/regularizacao_ext.py:219`; `app/chamados/services/regularizacao_ext.py:77`; `app/ponto/services/fechamento.py:1790`; `app/ponto/services/fechamento.py:1824`.)

   O instrumento existente **recusa** essa retomada; não serve para executá-la e concluir “nada mudou”. A prova precisa contemplar os callbacks reais e seus descendentes, comparar o estado após os efeitos **antes do desfazimento**, e registrar qualquer efeito não coberto como impedimento. Hash igual somente depois do rollback não demonstra ausência de escrita. (`app/chamados/services/foto_e4b.py:37`; `app/chamados/services/foto_e4b.py:57`; `app/chamados/services/foto_e4b.py:65`; `app/chamados/services/foto_e4b.py:77`.)

4. **Exportada/dinheiro/!:** há caminho concreto para alcançar dinheiro; sua ocorrência nas disputas raspadas é **[nao verificado]**. Hash do TXT/`ExportacaoDominio` não substitui fotografia/hash do `FechamentoMensal`, `DiaPago` e células exigidos pela medida. Portanto, o ramo “nada de folha muda” permanece **[nao verificado]**. (`app/folha/export.py:572`; `app/chamados/services/foto_e4b.py:25`; `anexo corte_b2_cerca_no_escritor_1711.txt:5`.)

**Item 7 — CONFERE como continuação condicionada ao rebase e à recertificação.**

1. **Premissas:** a comparação encontrou `reconciliador.py` idêntico entre B2 e `wt-f2-073854`, inclusive `_retratar`. Não há sobreposição textual atual com uma cerca inserida somente nesse ponto. Conflito com o delta ainda inexistente: **[nao verificado]**. (`app/chamados/reconciliador.py:520`; `/home/ronald/wt-f2-073854/app/chamados/reconciliador.py:520`.)

2. **Colisão:** a exceção nominal fornecida substitui a parada da F2 por mudança da B2. Conferência do texto integral do aval anterior: **[nao verificado]**. A recertificação da árvore resultante continua obrigatória. (`anexo corte_b2_cerca_no_escritor_1711.txt:8`; `CLAUDE.md:320`.)

3. **Literal:** existe interação semântica em `models.py`: F2 troca a seleção da relavra e de `_ancoradas` para `q_nao_validada_pelo_ato`; também altera a leitura da retenção para a sequência de decisões. A enumeração da cerca deve ser reavaliada nessa base. Se o delta compartilhar enumeração em `models.py`, haverá arquivo comum; isso ainda não prova conflito de merge. (`/home/ronald/wt-f2-073854/app/chamados/models.py:315`; `/home/ronald/wt-f2-073854/app/chamados/models.py:351`; `/home/ronald/wt-f2-073854/app/chamados/models.py:1945`; `CLAUDE.md:309`.)

4. **Exportada/dinheiro/!:** continuar construindo não dispensa as condições do pouso nem autoriza efeitos adicionais em exportada. Resultado da árvore combinada: **[nao verificado]**. (`anexo corte_b2_cerca_no_escritor_1711.txt:7`; `anexo corte_b2_cerca_no_escritor_1711.txt:8`; `CLAUDE.md:265`.) [nao verificado]

VEREDITO: item 1 CONFERE; item 2 CONFERE; item 3 CONFERE; item 6 PROCEDE; item 7 CONFERE -- item 6: rollback simples e medidor que recusa a retomada não provam os efeitos pós-commit nem fecham “nada de folha muda” (app/chamados/signals.py:146; app/chamados/services/foto_e4b.py:37).

## 17/11 09:25 · sessao 0b5e2282f266 · - · - · origem=Ronald (tmux)

- **Juiz específico registrado “código do Domínio válido?”: nenhum.** O registro declara `classificar_export` para aprovação e entrada no TXT, além da leitura de tela; o lookup também aponta apenas quem entra no TXT (`app/core/juizes.py:181`, `app/core/juizes.py:182`, `app/core/juizes.py:329`, `app/core/juizes.py:330`, `app/core/juizes.py:743`; `app/docs/JUIZES.md:118`). O próprio lookup distingue tabela documental de registro vivo (`app/docs/JUIZES.md:10`).

- **Há múltiplos sítios vivos de julgamento.** `classificar_export` rejeita somente código vazio após `(codigo_dominio or '').strip()` (`app/folha/export.py:840`, `app/folha/export.py:872`). O layout exige linha 43 caracteres e matrícula de dez posições com `.isdigit()` (`app/folha/export.py:273`, `app/folha/export.py:277`). `partes_da_linha` repete comprimento e matrícula numérica (`app/folha/export.py:247`). O PDF repete comprimento, constante `11` e matrícula numérica (`app/folha/pdf_export.py:112`). Contagem defensável para **juízos de entrada/forma: 4 sítios**; não confundir com as verificações adicionais de mera completude (`app/api/views_mensageria.py:1281`; `app/colaboradores/services/cadastro.py:71`, `app/colaboradores/services/cadastro.py:83`; `app/templates/colaboradores/admitir.html:204`; `app/templates/colaboradores/editar.html:298`).

- **O layout não verifica duplicidade.** Não há consulta de colaboradores nem conjunto de matrículas no corpo do validador (`app/folha/export.py:266`). Também não reconhece vazio original: `montar_linha_10('')` preenche com zeros; `zfill(10)` não trunca código longo (`app/folha/export.py:36`). Assim, “repetido seria recusado pelo validar_layout” não é sustentado pelo código; política desejada de unicidade: **[não verificado]**.

- **A recusa derruba a geração inteira:** a view retorna erro se houver qualquer erro de layout, antes de produzir conteúdo; o command lança `CommandError` (`app/folha/views.py:275`, `app/folha/views.py:280`; `app/folha/management/commands/regerar_txt_dominio.py:89`).

- **Proposta: “lei 0” para este PREPARA**, limitado ao censo e à reconciliação do contrato existente, sem escolher nova política de duplicados, motivo ou destino individual. Coerência já é lei; consumidor deve ler autoridade; regra nova não pode ser deduzida só do código (`app/docs/LEIS.md:122`, `app/docs/LEIS.md:95`, `app/docs/LEIS.md:136`). O corte PADRAO-NA-CURA já nomeia juiz único como padrão estrutural (`app/docs/CORTES.md:378`). Se a construção criar autoridade nova, há exigência documental própria de frase de nascimento; isto não equivale automaticamente a lei de negócio ausente (`app/docs/LEIS.md:69`).

## 17/11 09:25 · sessao 46e6a122e364 · - · - · origem=Ronald (tmux)

Escritores primários: **2**, contando origens que atribuem um novo `codigo_dominio`:

1. **Admissão:** lê `codigo_folha` com `.strip()` em `app/colaboradores/views.py:1131`; se não vazio, atribui em `:1232` e persiste por `salvar_campos` em `:1237`.
2. **Edição:** lê e aplica `.strip()` em `app/colaboradores/views.py:1353`; só atribui se não vazio em `:1355-1356`; persiste por `salvar_inteiro` em `:1365`. Portanto, enviar vazio preserva o código anterior. `app/colaboradores/views.py:1353-1365`

Persistências e regravações que precisam constar separadamente, sem inflar as duas origens:

- `salvar_campos` executa `save(update_fields=nomes)`: `app/colaboradores/services/colaborador.py:140-148`.
- `salvar_inteiro` executa `save()` completo, incluindo o código carregado: `app/colaboradores/services/colaborador.py:152-160`. Chamadores concretos: vincular posto (`app/colaboradores/views.py:345`), editar cadastro (`:1365`), transferir posto (`:1418`) e desligar (`app/colaboradores/services/desligamento.py:133`).
- Reativação também faz `save()` completo: chamador `app/colaboradores/services/desligamento.py:247`; persistência `app/colaboradores/services/colaborador.py:200`.
- `criar_colaborador` usa `objects.create(**campos)`: `app/colaboradores/services/colaborador.py:204-206`. A admissão primeiro cria sem código em `app/colaboradores/views.py:1204`, recebendo o default vazio do modelo (`app/colaboradores/models.py:312`), e depois atribui o código. O comando `smoke_chamados` cria fixture sem código em `app/chamados/management/commands/smoke_chamados.py:78-80`.
- Portas genéricas também capazes de escrever o campo: `gravar` (`app/colaboradores/services/colaborador.py:126-136`) e `atualizar_em_lote` (`:164-181`), permitido por `OPERACOES['cadastro']` em `:46`. Não encontrei chamador que lhes passe `codigo_dominio`; completude dessa ausência é **[nao verificado]**.
- Migration `0026` adiciona o campo com `default=''`, não contém `RunPython`: `app/colaboradores/migrations/0026_colaborador_codigo_dominio.py:12-18`.
- Django admin é somente leitura: `ColaboradorAdmin(SoLeitura, ...)` em `app/colaboradores/admin.py:48`; bloqueio de add/change/delete e readonly de todos os campos em `app/core/admin.py:59-75`.
- Aliases usados para devolver formulários recusados alteram apenas memória: `app/colaboradores/views.py:1117`, `:1282`; implementação explicitamente sem salvar em `app/core/reexibir.py:21-49`.
- Importação Nexti encontrada escreve somente telefone/email: `app/colaboradores/management/commands/importar_telefones_nexti.py:85-113`; não escreve o código nesse caminho.

Validação:

- Modelo: `CharField(max_length=20, blank=True, default='')`, sem validator específico, sem unicidade; `app/colaboradores/models.py:311-314`. `Colaborador.Meta` só declara nomes e ordenação: `:400-403`. O `save` normaliza CPF e matrícula Hasner, sem validar código: `:415-439`.
- HTML das duas telas usa `type="text"`, sem `pattern` ou `maxlength`: `app/templates/colaboradores/admitir.html:58`; `app/templates/colaboradores/editar.html:62`.
- JS ainda marca `codigo_folha` como obrigatório nas duas telas: `app/templates/colaboradores/admitir.html:204-205`; `app/templates/colaboradores/editar.html:298-299`.
- Backend permite salvar sem código: obrigatórios são nome/CPF, com dispensa do CPF na edição; `app/colaboradores/services/cadastro.py:22`, `:53-65`. Código vazio vira pendência que declara travar folha; `:29-30`, `:74-86`.
- Guarda de tamanho não protege o alias: admissão não inclui o código em `_dados_tam` (`app/colaboradores/views.py:1180-1185`); edição passa POST cru com `codigo_folha` (`:1298-1300`), enquanto helper só examina chaves que coincidam com `f.name` (`app/core/validacao.py:49-53`). Nenhuma dessas duas origens verifica numérico, máximo de 10 dígitos ou duplicidade antes da gravação: `app/colaboradores/views.py:1131`, `:1231-1237`, `:1353-1365`.

Selo relevante: `test_tamanho_campos` só procura chamadas/marcadores da guarda e testa outros campos; não demonstra proteção do código/alias: `app/colaboradores/tests/test_tamanho_campos.py:41-51`, `:76-82`.

## 17/11 09:24 · sessao 7dc300d0891f · - · - · origem=Ronald (tmux)

1. **Mordida do OR: sim, por inspeção.** O SQL da versão anterior não contém o trecho exigido `"chamados_perguntadisputa"."id" IN (SELECT`; a face atual usa `Exists`, e o teste exige literalmente esse trecho. Portanto o mutante OR morre na asserção de forma, mesmo conservando o conjunto. Execução Django [nao verificado]. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:493`; `app/chamados/models.py:3503`; `app/chamados/models.py:355`.)

2. **Mordida da projeção: não.** Preservando a subquery e substituindo a face por `Q(validada_em__isnull=True)`, o teste novo passa por inspeção: cria apenas uma pergunta sem carimbo e outra com `validada_em=T2`, sem registrar decisões divergentes. A fixture cria perguntas diretamente, e a face também classifica essa segunda como passivo validado. Execução Django [nao verificado]. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:484`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:490`; `app/chamados/tests/cortes_f2_f9_apoio.py:58`; `app/chamados/models.py:3505`.)

3. **A suíte já contém proteção de negócio contra esse mutante**, em M13: pergunta validada somente no ato, projeção vazia, precisa ficar fora de `_ancoradas`. Portanto a lacuna é do teste novo isolado; não significa que o mutante sobreviva à suíte inteira. Execução Django [nao verificado]. (`app/chamados/tests/test_cortes_f2_f9_m13.py:38`; `app/chamados/tests/test_cortes_f2_f9_m13.py:43`; `app/chamados/tests/test_cortes_f2_f9_m13.py:45`.)

4. **Regressão concreta na certificação do mutante M13:** `_incluir_validadas` exige exatamente um `PerguntaDisputa.objects.filter`, mas agora encontra dois, nas linhas 355 e 357. Executei somente AST/helper extraído em memória com `python3 -B`; resultado: `RuntimeError: mutante m13_inclui_validadas sem filtro unico; isto nao e mordida`. O caminho com `CORTES_F2_MUTANTE=m13_inclui_validadas` falha antes da asserção de negócio. A execução normal sem seletor não aplica o helper. (`app/chamados/models.py:355`; `app/chamados/models.py:357`; `app/chamados/tests/test_cortes_f2_f9_m13.py:25`; `app/chamados/tests/test_cortes_f2_f9_m13.py:31`; `app/chamados/tests/cortes_f2_f9_apoio.py:84`; `app/chamados/tests/cortes_f2_f9_apoio.py:93`.)

5. **CORPO atualizado corretamente:** comparação AST em memória confirmou os nove tuplos; os sete posteriores a `_ancoradas` correspondem ao deslocamento +4, enquanto `transicionar:205` e `_gravar_estado:302` permanecem correspondentes. `EXCECOES` continua vazio; não houve ampliação nominal do corpo. (`app/chamados/tests/e4b_c12_ast.py:18`; `app/chamados/tests/e4b_c12_ast.py:29`; chamadas em `app/chamados/models.py:205`, `:302`, `:462`, `:667`, `:847`, `:865`, `:1501`, `:1705`, `:1725`.)

6. **Escopo do selo de custo:** ele cobra uma consulta, dois resultados esperados e um fragmento textual; não mede tempo, custo, JIT nem plano. As medições mencionadas no comentário permanecem [nao verificado]. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:476`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:487`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:493`.)

## 17/11 09:24 · sessao 7adad91dcf6e · - · - · origem=Ronald (tmux)

1. **[bloqueia] O delta invalida o mutante M13 existente.** `_incluir_validadas` exige exatamente um `PerguntaDisputa.objects.filter` no método; a nova versão contém dois. A transformação levanta `RuntimeError` antes de exercitar a asserção de negócio. Reproduzi executando apenas o transformador AST em memória, sem Django, banco ou escrita. Fontes: `app/chamados/tests/test_cortes_f2_f9_m13.py:25-33`; `app/chamados/models.py:355-357`; `app/chamados/tests/cortes_f2_f9_apoio.py:90-100`. O próprio apoio diz que erro de aplicação não conta como mordida: `app/chamados/tests/cortes_f2_f9_apoio.py:81-82`. Sem seletor, o GREEN não detecta isso: `app/chamados/tests/cortes_f2_f9_apoio.py:84-87`.

2. **[nota] Censo dos cinco chamadores diretos de produção:**

   - `_gravar_estado`: `app/chamados/models.py:229`.
   - `_vivas_ancoradas`: `app/chamados/models.py:341`.
   - `_plano_pergunta_viva`: `app/chamados/models.py:393`.
   - `_perguntas_relavradas_ou_ressuscitadas`: `app/chamados/services/disputa_emissao.py:1821`.
   - `_resolver_perguntas_por_chamado_resolvido`: `app/chamados/services/disputa_emissao.py:1962`.

   Chamadas diretas nos testes: `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:411,488`; `app/chamados/tests/test_cortes_f2_f9_m13.py:45,54`.

3. **[nota] Os chamadores recebem lista pronta, preservando materialização e relações carregadas.** O `list(...)`, `select_related('disputa', 'disputa__colaborador', 'disputa__colaborador__empresa', 'celula')` e `order_by('pk')` continuam no retorno: `app/chamados/models.py:357-359`. Nenhum chamador direto encadeia `select_for_update` nesse retorno; eles consomem/combinam a lista: `app/chamados/models.py:229-230,341,393-395`; `app/chamados/services/disputa_emissao.py:1821-1824,1962-1965`.

4. **[nota] A query de leitura não adquire as travas do protocolo.** As travas são explícitas e separadas em `trava_na_ordem`, com `SELECT FOR UPDATE` de colaborador, disputas, chamados e perguntas: `app/chamados/models.py:2765,2802,2811,2824,2828`. O escritor de estado chama essa porta antes do save: `app/chamados/models.py:295-302`; o wrapper do S143 trava antes do corpo que chama `_ancoradas`: `app/chamados/services/disputa_emissao.py:1901-1903,1962`. O delta não altera esses pontos: `app/chamados/models.py:355-359`.

5. **[nota] O reaproveitamento da lista gate→núcleo permanece.** O plano guarda `(novo, lista)`; o núcleo recupera e só busca novamente se não houver cache compatível: `app/chamados/models.py:393-394,222-230`. A subquery é passada como QuerySet ao filtro externo, sem materialização própria; existe apenas o `list` externo: `app/chamados/models.py:355-359`. Para chamado sem PK, continuam zero queries: `app/chamados/models.py:348-349`. A contagem efetivamente executada nesta cópia é [nao verificado].

6. **[nota] Há selos que dependem da contagem de queries.** O novo teste exige uma query: `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:487-489`; o selo do chokepoint mede `reconciliar` sem perguntas e cruzando a fronteira com três perguntas: `app/ponto/tests/test_selo_chokepoint_escrita.py:360-371`. O histórico do teto identifica `_ancoradas` como um SELECT adicional por S143: `app/ponto/tests/test_selo_chokepoint_escrita.py:116-124`. Resultado desses selos nesta execução: [nao verificado].

## 17/11 09:23 · sessao a78558220a8f · QUEIMA-UTIL-1711 · sonda · origem=Code

- **col265:** em **2026-10-31**, a sonda mostra `passivo_sem_linha`, célula 135811 e `fio='nao_sei'`. [nao verificado] — saída fornecida, sem arquivo:linha.
- **col838:** **nao reproduz hoje**: nenhuma linha de dia `nao_sei` na saída fornecida. [nao verificado]
- **col407:** em **2026-10-21, 2026-10-29 e 2026-11-02**, a sonda mostra `passivo_sem_linha`, respectivamente nas células 133662, 133670 e 133674. [nao verificado] — saída fornecida, sem arquivo:linha.

Para **todos os dias listados de col265 e col407**, o ramo identificado é **decisão/passivo**: `_pela_decisao` devolve `('passivo_sem_linha', None)`; o chamador copia a razão e marca `nao_sei=True`. (`app/chamados/services/tarefa_do_dia.py:154`, `app/chamados/services/tarefa_do_dia.py:155`, `app/chamados/services/tarefa_do_dia.py:334`, `app/chamados/services/tarefa_do_dia.py:339`)

O registro faltante é **resposta e/ou validação na sequência E1**, por correspondência de ato e instante. O código liga explicitamente `passivo_sem_linha` a `registrar_decisao`, executado por `DecisaoPergunta.registrar(...)`. (`app/chamados/models.py:4098`, `app/chamados/models.py:4102`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:98`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`)

Portanto, o escritor pendente identificável é **o registro da decisão E1**. A razão apresentada não identifica falha de `gerar_celulas`, cartório/ata ou lavra do veredito: o teste de lavra do veredito ocorre depois do retorno por passivo. (`app/chamados/services/tarefa_do_dia.py:154`, `app/chamados/services/tarefa_do_dia.py:167`) Qual chamada histórica deixou essa lacuna, e se faltou resposta, validação ou ambas, é **nao recuperavel pela saída**. [nao verificado]

ORIGEM: col265 decisão/passivo_sem_linha → registro E1, DecisaoPergunta.registrar (`app/chamados/management/commands/backfill_fio_morto_e4b.py:141`); col838 nao reproduz hoje [nao verificado]; col407 decisão/passivo_sem_linha → registro E1, DecisaoPergunta.registrar (`app/chamados/management/commands/backfill_fio_morto_e4b.py:141`).

## 17/11 09:23 · sessao 0dbb64bf6991 · - · - · origem=Ronald (tmux)

Recomendo **item 1 CONFERE; item 3 CONFERE**, como instruções de construção condicionadas à prova exigida, sem confundir ausência futura de RED/delta com premissa falsa. (`CLAUDE.md:28`; `CLAUDE.md:37`; `CLAUDE.md:372`.)

- **Veto existente:** `veto_de_competencia(empresa, dias, memo)` recebe empresa, coleção de dias e cache; devolve `None` se houver dia desconhecido, `True` se alguma competência estiver entregue, `False` nos demais casos. Não seleciona perguntas nem restringe motivos/famílias. (`app/chamados/services/fio.py:1357`; `app/chamados/services/fio.py:1365`.)
- **Mesmo K6, outra pergunta que C21:** compõe dia → janela da empresa → `competencia_entregue`; C21 julga instante **e** texto da resposta. Consultar K6 no escritor não duplica C21. (`app/chamados/services/vigia_fio.py:786`; `app/chamados/services/vigia_fio.py:802`; `app/core/juizes.py:306`; `app/core/juizes.py:343`; `app/chamados/juizes.py:1552`.)
- **Armadilha literal concreta:** em `[None, dia_ENTREGUE]`, o veto devolve `None` antes de consultar K6; `if veto:` permitiria continuar. O padrão existente `is not False` recusa também competência desconhecida, sem novo juiz. (`app/chamados/services/fio.py:1365`; `app/chamados/services/fio.py:1116`; `app/chamados/services/fio.py:1118`.)
- **Passar coleção reiterável:** os dois `any` percorrem `dias`; um generator sem `None` seria consumido pelo primeiro e entregaria vazio ao segundo. Usar lista/tupla evita essa perda de universo. (`app/chamados/services/fio.py:1365`; `app/chamados/services/fio.py:1367`.)
- **Não copiar fora163:** `_k6_que_barra` consulta somente metades de intervalo fora163; perderia outras perguntas alcançadas. (`app/chamados/services/materializacao.py:792`; `app/chamados/services/materializacao.py:805`.)
- **Suficiência continua exigível:** a seleção deve cobrir também relavra sem filtro de materialização/disputa aberta e perguntas sem destino em outras disputas abertas do colaborador; limitar por destino ou pela disputa original não fecha o literal “TODAS”. (`app/chamados/models.py:309`; `app/chamados/services/disputa_emissao.py:2048`; `app/chamados/services/disputa_emissao.py:2057`.) Prova do futuro delta: **[nao verificado]**.
- **Item 3:** fluxo verificador → suite inteira → opus final confere; “só do delta” precisa conservar o alcance dos escritores/leitores das funções e campos tocados, mesmo fora do diff. (`CLAUDE.md:372`; `CLAUDE.md:378`; `CLAUDE.md:379`.) [nao verificado]

## 17/11 09:23 · sessao b4ac020c69a9 · - · - · origem=Ronald (tmux)

Item 6 — **PROCEDE quanto à suficiência literal da medida**. As condições são instrumentáveis, mas nenhuma está demonstrada pelo dado fornecido: opus final do delta, hash de `FechamentoMensal` exportado e resultado do item 4 **[nao verificado]**. A regra exige condição literal fechada, não mera ausência de movimento financeiro (`CLAUDE.md:37`, `CLAUDE.md:265`).

O obstáculo concreto é o pós-commit: `fechar` salva a disputa (`app/chamados/models.py:2027`); o receiver agenda retomada por `on_commit` (`app/chamados/signals.py:122`, `app/chamados/signals.py:146`). Ela seleciona regularizações confirmadas sem materialização de todo o colaborador (`app/chamados/services/regularizacao_ext.py:219`), pode registrar batidas (`app/chamados/services/regularizacao_ext.py:77`) e chegar ao recálculo de competência exportada (`app/ponto/registro_batida.py:180`, `app/ponto/services/fechamento.py:1790`, `app/ponto/services/fechamento.py:1809`, `app/ponto/services/fechamento.py:1824`). Portanto, savepoint+raise simples não demonstra o efeito completo; comparar depois do rollback tampouco demonstra ausência de escrita tentada. A medida precisa incluir os callbacks reais e seus descendentes e comparar antes do desfazimento (`app/chamados/services/foto_e4b.py:30`, `app/chamados/services/foto_e4b.py:65`).

Já existe fotografia de FM/DiaPago/células, mas seu executor de callbacks **recusa** precisamente a retomada, não a executa: não basta reutilizá-lo e concluir “nada de folha muda” (`app/chamados/services/foto_e4b.py:18`, `app/chamados/services/foto_e4b.py:37`, `app/chamados/services/foto_e4b.py:56`, `app/chamados/services/foto_e4b.py:75`). Há precedente de execução explícita de callbacks em teste (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:556`).

Item 7 — **CONFERE como autorização nominal para continuar e recertificar**, sem conflito textual demonstrado com uma cerca localizada apenas em `_retratar`. A comparação em leitura mostrou `reconciliador.py` idêntico entre esta B2 e `wt-f2-073854`, incluindo o ponto de inserção (`app/chamados/reconciliador.py:520`; `/home/ronald/wt-f2-073854/app/chamados/reconciliador.py:520`). Conflito de merge com o delta futuro **[nao verificado]**.

Existe interação semântica relevante: F2 troca a seleção da relavra de coluna `validada_em` para `q_nao_validada_pelo_ato`, e faz o mesmo em `_ancoradas` (`/home/ronald/wt-f2-073854/app/chamados/models.py:315`, `/home/ronald/wt-f2-073854/app/chamados/models.py:351`). Também muda a retenção para ler `DecisaoPergunta.leitura`, conservando C21 e `materializada_em` da coluna (`/home/ronald/wt-f2-073854/app/chamados/models.py:1945`). A enumeração da cerca precisa acompanhar esse universo após o rebase; editar `models.py` para compartilhá-la criaria interseção de arquivo, mas isso ainda não é um conflito provado (`CLAUDE.md:309`, `CLAUDE.md:320`).

## 17/11 09:23 · sessao cb0cd920166d · - · - · origem=Ronald (tmux)

`passivo_sem_linha` identifica **projeção de resposta/validação sem a linha correspondente da sequência E1** (`app/chamados/models.py:4098`, `app/chamados/models.py:4102`).

- `_pela_decisao` retorna `('passivo_sem_linha', None)` em `app/chamados/services/tarefa_do_dia.py:154`; o chamador grava a razão e levanta `nao_sei` em `app/chamados/services/tarefa_do_dia.py:334` e `app/chamados/services/tarefa_do_dia.py:338`.
- A ligação explícita ao escritor existe: `passivo_sem_linha` → `registrar_decisao` em `app/chamados/management/commands/backfill_fio_morto_e4b.py:98`; execução por `DecisaoPergunta.registrar(linhas_da_projecao_sem_linha(ps))` em `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`.
- A porta única que escreve essa sequência é `DecisaoPergunta.registrar`, usando `bulk_create` (`app/chamados/models.py:4110`, `app/chamados/models.py:4120`).
- O ramo de lavra do veredito é outro: `veredito_sem_lavra_vigente` em `app/chamados/services/tarefa_do_dia.py:167`; `passivo_sem_linha` retorna antes dele (`app/chamados/services/tarefa_do_dia.py:154`).
- A saída fornecida permite identificar a falta de registro E1, mas não discrimina se falta resposta, validação ou ambas, nem qual escritor histórico originou a lacuna: **não recuperável [não verificado]**.

## 17/11 09:22 · sessao 463517ba4a9f · - · - · origem=Ronald (tmux)

Remedida dos leitores e autoridades:

| Citação do brief | Neste HEAD | Resultado |
|---|---|---|
| `ListaDoAdmin:188-255` | `app/chamados/services/vigia_fio.py:166` | Classe preservada; `dia_do_admin:200`, `dia_na_lista:208`, `colab_na_lista:226`. |
| `dono_fora_do_fio:274-315` | `app/chamados/services/vigia_fio.py:240` | Continua consultando a lista; balde `fio_do_admin` em `:261-262`. |
| `censo_alcance:542` | `app/chamados/services/vigia_fio.py:487` | Universo via `dias_que_seguram:537`; perguntas vivas `:566`, `ps:597`; causa d `:638-639`; causa e de dono ausente `:642-645`; contador `:656-659`; rótulos `:483`. |
| Chamadores do censo | `app/chamados/management/commands/vigiar_fio_morto.py:175`, `:181`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:78` | Conferem. |
| `porta_da_tarefa:638`, `carga_da_tarefa:648` | `app/chamados/services/tarefa_do_dia.py:712`, `:728` | Nomes e contrato preservados; desconhecimento sem rota em `:719-722`. |
| `alimentar_portas:1595` | `app/chamados/juizes.py:1616` | Nome preservado. |
| `dias_que_seguram:109` | `app/folha/fora_da_folha.py:125` | Universo sem consultar porta; contrato em `:126-139`. |
| Juiz e6 | `app/ponto/management/commands/e6_oraculo.py:53` | Definição começa em 53; corpo decisório continua `:98-110`. `InsumosDoDono:113`, `.dono:182`, `sem_motor` sem CXR retorna `None:193-194`. |
| Gate e6 | `app/ponto/management/commands/e6_oraculo.py:351` | Continua descartando zero nos dois lados em `:351-353`; distinto da montagem de dono na lista em `app/escala/services/cadastro_realidade.py:510`. |
| Consumidores do beco | `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:82`, `:105`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:51`, `:54` | Conferem. |
| Chave C5 em pergunta_no_app | `app/chamados/services/pergunta_no_app.py:35` | Confere. |
| Motor: sentidos de dia | `app/ponto/motor_calculo_v2.py:1528`, `:1623`, `:1997` | Conferem os sítios mencionados. |
| Padrão lâmpada | `app/docs/GLOSSARIO.md:13` | Confere. |
| Selo de custo | `app/escala/tests/test_f3_beco1_folga_na_lista.py:395` | Teste citado existe. |

Duas inexatidões materiais do brief:

- Os leitores diretos não leem ambos o dicionário por dia: `contador_nunca_bateu` expande seus pares em `app/chamados/management/commands/disparar_perguntas_competencia.py:56-57`; `vigia_retido_calado.censo` usa somente o conjunto de colaboradores, `app/folha/vigia_retido_calado.py:71-72`.
- O agregado `por_ass['C5']['ocorrencias']` existe em `app/escala/services/cadastro_realidade.py:946-950`, mas a etiqueta da tela lê `colabs`, `app/templates/escala/cadastro_x_realidade.html:19`. As linhas da tela/PDF mostram `detalhe` e só usam `ocorrencias` como alternativa, respectivamente `app/templates/escala/cadastro_x_realidade.html:28` e `app/escala/services/cadastro_realidade.py:1090-1092`. Portanto não procede afirmar que esse agregado já determina o número mostrado no PDF/tela; a mudança efetiva será sobretudo multiplicar assinaturas e trocar seus detalhes, conforme a implementação futura [nao verificado].

O recorte local da LU2-07 continua por construir: `censo_alcance` usa `empresas_visiveis()` em `app/chamados/services/vigia_fio.py:517-520`; essa consulta filtra apenas ativa/CNPJ em `app/colaboradores/queries.py:85-87`. Cadastro e rótulo continuam `app/colaboradores/models.py:45` e `app/templates/colaboradores/empresa_form.html:35`; terceiro sítio manual em `app/config/crons.py:17`. Alteração global alcançaria o lavrador em `app/folha/management/commands/lavrar_retrato_folha.py:51`.

C11 continua exigindo perguntas vivas, `app/escala/services/cadastro_realidade.py:464-472`, e o ramo d continua exigindo ausência delas, `app/chamados/services/vigia_fio.py:638`; a disjunção D1 persiste.

`competencia_ja_paga` segue obra aberta em `app/docs/BACKLOG.md:1992`; implementação com esse nome não encontrada [nao verificado]. Existem a composição de entregue, `app/chamados/services/vigia_fio.py:785-807`, o juiz `competencia_entregue`, `app/folha/export.py:556`, e o veto `app/chamados/services/fio.py:1353-1363`.

_6443 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
