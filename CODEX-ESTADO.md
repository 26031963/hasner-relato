# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 15:18** · publicado em **11/11/2026 15:18** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 15:18 · sessao ab98b69c3c6e · - · - · origem=Ronald (tmux)

Perguntas 3, 5 e 6 — resultado da exploração, sem gravações.

1. `base_main` amarra ao `main` local, não à base de pouso no remoto nem ao início registrado da obra: `bin/selo_proporcao.py:96-98`. Cenários concretos derivados do algoritmo; ocorrência real nesta cópia [nao verificado], pois não há `.git`.

   - **Falso rc2 com main atrasado:** cadeia `O → M → H`; `main=O`, `origin/main=M`, raia criada de `M`, `H` só altera template. O intervalo correto para pousar sobre o remoto é `M..H`. O selo aceita que `M` seja ancestral (`:94`), mas calcula `merge-base(main,H)=O` e devolve 2 (`:96-98`).
   - **Recorte aceito com main adiantado:** cadeia `O → C1 → C2`; `origin/main=O`, `main=C1`, raia `head=C2`; C1 da mesma obra altera `views.py`, C2 só template. Passar `base=C1` satisfaz a guarda e compara somente C1..C2 (`:100`): B/rc0, omitindo o escritor de C1. Não há leitura de `origin/main` nem identidade da obra em `:83-103`.
   - **Worktree, isoladamente, não causa erro:** a raiz da worktree passa na comparação de `realpath` e o Git é chamado com `-C repo` (`:41-43`, `:85-87`). Uma worktree/clone com apenas `origin/main`, sem uma referência `main` resolvível, falha no `merge-base` por `git_rc`, rc2 (`:44-45`, `:96`, `:104-106`).
   - **Raia que recebeu merge do main não recorta automaticamente:** `O→C1` na raia e `O→M` no main; merge `R=(C1,M)`, seguido de template H, com main=M. A base aceita é M e o diff M..H ainda inclui o saldo de C1 (`:96-100`). Se o mesmo C1 já estiver no main, ou se seu efeito for revertido no head, o diff de pontas não o mostra; o código examina árvores, não todos os commits (`:100-101`). Não chamar isso de perda automática causada por qualquer merge. [nao verificado]

2. **Produtor da lista persistida encontrado e problema de durabilidade:**

   - `app/docs/CORTES.md:3` declara arquivo gerado de `CORTES.json`.
   - O produtor real é `bin/gerar_cortes.py:23-24`, que lê o JSON em `:31-32`, transforma cada entrada em linha em `:106-109` e **sobrescreve integralmente** o Markdown em `:114-116`.
   - A lista hoje está em `app/docs/CORTES.md:444`.
   - O JSON fornecido não contém `PROPORCAO-POR-RISCO` nem os marcadores; termina em `app/docs/CORTES.json:1367`. Verificação somente em memória, chamando `markdown(cortes)` com `python3 -I -B`: Markdown atual tem 1 início/1 fim; Markdown regenerado tem **0 início/0 fim**. Nenhum arquivo foi escrito.
   - Consequência concreta condicional: executar legitimamente o gerador e incorporar o resultado apaga a lista; uma base posterior assim produz rc2 no selo, em `bin/selo_proporcao.py:50-52`. Não afeta retroativamente uma base antiga, porque a leitura é por `git show`.
   - Autoridade humana declarada: módulos novos só por corte do Ronald, `app/docs/CORTES.md:444`; o parser não autentica autoria/corte, apenas formato e presença em base (`bin/selo_proporcao.py:49-58`).

3. **Mapa de escritores/leitores relevante à pergunta 5:**

   | Valor | Quem escreve/fornece | Quem lê |
   |---|---|---|
   | CORTES persistido | `bin/gerar_cortes.py:116`, fonte `:23`, leitura `:32`, frase `:109`; autoridade `app/docs/CORTES.md:444` | `git show base:CORTES` em `bin/selo_proporcao.py:50`; parser `:51-58`; pertinência `:64` |
   | Lista sintética do harness | `bin/tests/test_selo_proporcao.sh:17`, valores `:22`; corrupções `:102-109`; autorização HEAD `:87-88`; disco `:119` | mesmo selo, chamado em `:42`, `:52`, `:120` |
   | `base/head/classe/repo` | argumentos externos `bin/selo_proporcao.py:83`; chamador concreto harness `bin/tests/test_selo_proporcao.sh:42` | revisões resolvidas `bin/selo_proporcao.py:89-91`; base/head `:92-100`; classe `:116`; repo `:85` |
   | `main` sintético | harness `bin/tests/test_selo_proporcao.sh:29`, `:51`, `:53` | `bin/selo_proporcao.py:96` |
   | merge-base | subprocesso Git em `bin/selo_proporcao.py:96` | igualdade com base `:97` |
   | nomes do diff | Git `diff --no-renames --no-ext-diff --name-only -z` em `bin/selo_proporcao.py:100`; ordenação/deduplicação `:101` | vazio `:102`; classificador `:108-112`; impressão `:110`, `:114-115` |
   | rc | `bin/selo_proporcao.py:116-119`, protocolo `:129`, tradução `bin/selo_proporcao.sh:8-11` | harness `bin/tests/test_selo_proporcao.sh:42-43`, `:52`, `:113-114`; pouso humano segundo contrato anexo [nao verificado] |
   | RELATO da medida | nenhum gravador do selo: termina imprimindo e retornando `bin/selo_proporcao.py:110-119` | integração de consumo do resultado no pouso [nao verificado] |

   O harness é executado pelo laço genérico de selos do host em `bin/regua.sh:138-143`; o atalho de régua já verde retorna antes em `:84-94`. Isso testa o instrumento em repositórios sintéticos, não julga o diff real de pouso.

4. **Contrato/comentários versus implementação:**

   - O contrato anexado admite lista **OU** filtros; código corresponde: lista retorna antes das restrições (`bin/selo_proporcao.py:64-69`). Contudo o comentário `:13` diz “só app” e “migrations/management A sempre”, o que não vale para um item previamente listado.
   - A frase absoluta “qualquer .md faz A” do contrato anexado tem exceção pelo algoritmo: hipotético `app/templates/nota.md` seria B (`bin/selo_proporcao.py:70-71`). `app/docs/NOTA.md`, caso efetivamente testado, é A (`bin/tests/test_selo_proporcao.sh:78`).
   - `bin/tests/test_selo_proporcao.sh:4` ainda descreve `tests/ OU test_*.py`; implementação exige ambos (`bin/selo_proporcao.py:74`).
   - `app/docs/CORTES.md:443` ainda descreve selo de linhas adicionadas/escrita; código julga nomes de arquivos (`bin/selo_proporcao.py:100`, `:61-76`); a linha B3 `app/docs/CORTES.md:444` já diz caminho.
   - O contrato agora admite ausência de script de pouso e de contador, corrigindo a promessa documental; não implementa integração/medida. A busca das referências encontra chamadas concretas no harness; a saída do selo permanece stdout/rc (`bin/selo_proporcao.py:110-119`), não escreve RELATO nem contador.

## 11/11 15:17 · sessao 8f4e55fbbe88 · - · - · origem=Ronald (tmux)

Análise somente em memória da função `motivo_b`; não executei o harness que cria arquivos (`bin/tests/test_selo_proporcao.sh:11`, `:13`, `:17`).

1. Casos novos/alterados do harness:

| Caso | Guarda pretendida | Se retirada/afrouxada | Avaliação |
|---|---|---|---|
| `tests_ok` (`:61`) | aceitação `teste` (`selo_proporcao.py:74`) | Retirar aceitação produz rc1 | Mata. | [nao verificado]
| `tests_helper` (`:62`) | exigir nome `test_*.py` (`:74`) | Aceitar qualquer arquivo em `tests/` produz rc0 | Mata o alargamento para pasta apenas. |
| `test_basename` (`:63`) | exigir pasta `tests/` (`:74`) | Aceitar basename apenas produz rc0 | Mata o alargamento para nome apenas. |
| `migration_test_nome` (`:64`) | `carregado_por_pasta` (`:68`) | Sem ela continua rc1, pois o arquivo não fica em `tests/` | Não isola; o `mkdir migrations/tests` não muda o caminho gravado. |
| `migration_tests_dir` (`:65`) | `carregado_por_pasta` (`:68`) | Sem ela produz rc0 por `teste` | Mata. | [nao verificado]
| `commands_test_nome` (`:66`) | `carregado_por_pasta`, braço `management` (`:68`) | Sem ela continua rc1, pois falta `tests/` | Não isola; nenhum caso novo mata a retirada só do braço `management`. |
| `bin_tests` (`:67`) | `fora_app` (`:66`) | Sem ela continua rc1: `base.txt` não atende `teste` | Não isola. | [nao verificado]
| `bin_tests_test_py` (`:68`) | `fora_app` (`:66`) | Sem ela produz rc0 por `teste` | Mata. | [nao verificado]
| `templates_fora_app` (`:69`) | `fora_app` (`:66`) | Sem ela produz rc0 por `templates` | Mata. |
| `recorte` (`:110-115`) | `base_main` (`:96-98`) | Na preparação nominal, retirar guarda dá rc0 no teste que exige2 | Mata nominalmente, mas tem falso positivo de preparo abaixo. | [nao verificado]

Os resultados da tabela seguem diretamente da prioridade das condições em `bin/selo_proporcao.py:64-76`, com rc1 em `:116-118`; confirmei os dois mutantes de função em memória.

2. Falsos positivos e mascaramento:

- **Segundo commit do recorte pode falhar e o caso continuar verde.** `:112` não verifica `echo`, `git add`, `git commit` nem avanço do HEAD. Se `c2` não nascer, HEAD=C1: a primeira chamada retorna2 por `base == head` (`bin/selo_proporcao.py:92-93`), e a segunda retorna1 pelo `views.py` (`bin/tests/test_selo_proporcao.sh:111-114`). Assim o nome “base_recortada” passa por outra guarda.
- **A guarda nova mascara cobertura da ancestralidade.** Desligando `GUARDA:ancestral`, `revisoes_trocadas` e `nao_ancestral` continuam rc2 pela comparação `base_main`, pois a base passada é HEADC enquanto `merge-base(main, head)` é BASE (`bin/tests/test_selo_proporcao.sh:128`, `:131-133`; `bin/selo_proporcao.py:94-98`).
- O caso `base_igual_head` também chegaria a `base_main` se `igual` fosse removida, mas a checagem adicional do texto identifica isso (`bin/tests/test_selo_proporcao.sh:129-130`).
- Os testes de lista continuam verificando somente rc2; mudar `main` para a base defeituosa evita que `base_main` os barre antes da leitura, mas não identifica qual validação da lista produziu o rc (`bin/tests/test_selo_proporcao.sh:45-53`).
- `sem_repo_no_repo_do_script` ainda pode ser silenciosamente omitido se a cópia não tiver histórico Git (`bin/tests/test_selo_proporcao.sh:141-143`).

3. Achados B1–B5, pela evidência que analisei:

- **B1: cura os exemplos concretos.** Migrações e comandos ficam A pela guarda de pasta e, independentemente, nomes `test_*.py` fora de `tests/` deixam de ser aceitos (`bin/selo_proporcao.py:68-75`).
- **B2: cura o helper concreto; cura parcial do problema geral.** `marca_cron15.py` não atende `test_*.py`, mas arquivo de produção chamado `test_*.py` dentro de `tests/` segue B, limite admitido pelo próprio comentário (`bin/selo_proporcao.py:25-26`, `:74-75`).
- **B3: cura os exemplos concretos de `bin/tests/*`**, ausentes da lista fechada atual (`bin/selo_proporcao.py:64-67`; `app/docs/CORTES.md:444`).
- **B4: cura o recorte linear do exemplo apenas relativamente ao `main` local.** Não autentica a referência `main` nem a fronteira histórica da obra (`bin/selo_proporcao.py:96-100`).
- **B5: o delta não integra chamador nem contador.** Os únicos consumidores encontrados na busca por `selo_proporcao` são o wrapper e o harness (`bin/selo_proporcao.sh:7`; `bin/tests/test_selo_proporcao.sh:9`, `:42`). A mudança contratual fornecida corrige a descrição, declarando ambos pendentes; a ocorrência efetiva de conduta manual/RELATO fica [nao verificado].

4. Divergências documentais úteis:

- Cabeçalho do harness ainda diz `tests/ OU test_*.py`, enquanto o código exige ambos (`bin/tests/test_selo_proporcao.sh:4`; `bin/selo_proporcao.py:74`).
- A linha antiga de CORTES ainda descreve selo “nas linhas adicionadas” e “escrita com B declarado = rc1”; o código julga nomes do diff e conteúdo não interfere (`app/docs/CORTES.md:443`; `bin/selo_proporcao.py:100-118`).
- “So caminho sob app pode ser B” e “migrations/management = A sempre” têm exceção técnica pela prioridade da lista: se um caminho desses for incluído na lista da base, retorna B antes das duas guardas (`bin/selo_proporcao.py:13-17`, `:64-69`). A lista atual contém apenas dois caminhos sob `app/chamados/` (`app/docs/CORTES.md:444`).
- O comentário “nenhum .py fora da lista passa” contradiz as aceitações de Python sob templates/static e test_*.py sob tests (`bin/selo_proporcao.py:24`, `:70-75`).

A cópia disponível não contém repositório Git utilizável: `git status`, `git rev-parse HEAD` e `git log` retornaram “not a git repository”; hash e estados reais de branches não foram confirmados [nao verificado].

## 11/11 15:17 · sessao ef11ff740dce · - · - · origem=Ronald (tmux)

Achados concretos ainda classificados B:

| Caminho permitido | Quem carrega/lê e efeito |
|---|---|
| `app/core/tests/test_contract_esmeril.py` | Define `VULTURE_IGNORE_NAMES` em `app/core/tests/test_contract_esmeril.py:15`. `bin/esmeril_da_copia.sh:22` extrai o valor, `:24` passa a `vulture --ignore-names` e `:27` decide LIMPO. `bin/molde_fatia/rodar.sh:20` extrai, `:21` aplica, `:22` barra esmeril sujo. B por `bin/selo_proporcao.py:74`. **Allowlist de portão permanece em arquivo B.** |
| `app/core/tests/test_contract_tblib_no_paralelo.py` | `tblib_instalado()` definido em `app/core/tests/test_contract_tblib_no_paralelo.py:24`; importado pelo command de produção em `app/chamados/management/commands/tblib_ausente_na_imagem.py:17`, chamado em `:18`. `bin/placar_code.sh:463` executa o command; `:465` publica o contador. B por `bin/selo_proporcao.py:74`. |
| `app/static/pwa.js` | Carregado em `app/templates/base.html:20` e `app/templates/base_app.html:20`; registra service worker em `app/static/pwa.js:14`, grava batida offline em `:107`–`:119`, inicia sincronização em `:126`. B por `bin/selo_proporcao.py:72`. |
| `app/static/sw.js` | URL ligada à view em `app/core/urls.py:24`; leitura e resposta JavaScript em `app/core/views.py:570`–`:576`. `_raiz_do_static()` escolhe `/app/staticfiles` se houver SW coletado, senão `/app/static`, em `app/core/views.py:528`–`:530`. B por `bin/selo_proporcao.py:72`; ramo efetivo no host [nao verificado]. |
| `app/templates/base.html`, `base_app.html`, `ponto/espelho.html`, `colaboradores/detalhe.html`, `chamados/painel_gestao.html` | Portão declara as cinco páginas em `bin/prova_de_casca.py:21`, compila em `:49`–`:53`, lê os HTML da árvore de templates em `:30`–`:36`; chamado antes do reload por `bin/deploy.sh:295`–`:297`. B por `bin/selo_proporcao.py:70`. |

O build também lê conteúdo dos cinco recursos `sw.js`, `pwa.js`, `manifest.json`, `icons/icon-192.png`, `icons/icon-512.png`: lista em `app/core/views.py:524`, leitura em `:548`–`:552`, raiz escolhida em `:528`–`:530`; os caminhos sob `app/static/` passam por `bin/selo_proporcao.py:72`.

O antigo exemplo `app/core/tests/marca_cron15.py` continua importado por produção em `app/core/management/commands/crons.py:92` e afeta a aceitação do horário em `:97`, mas **agora recebe A**, pois seu basename não satisfaz `test_*.py` em `bin/selo_proporcao.py:74`.

A leitura de `app/core/tests/testes_sem_relogio.txt` por `bin/placar_code.sh:458` continua, mas o `.txt` recebe A pelo mesmo teste de nome em `bin/selo_proporcao.py:74`.

## 11/11 15:13 · sessao 6f34a2441ef2 · DATA-EVENTO-CRU · leitura · origem=Code

**Veredito: ajustar o contrato antes de considerá-lo fechado.** Os pontos principais são o alcance de `mensageria/`, a identidade dos escopos e a definição de “leitura”; além disso, a cópia contém **cinco** ocorrências diretas nos templates, incompatíveis com o “+4” sem explicação adicional. (`bin/regua.sh:167`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:132`; `app/templates/chamados/partials/_card_atendimento.html:87`, `:90`, `:164`; `app/templates/chamados/partials/_lote_preview.html:24`; `app/templates/chamados/meu_atendimento.html:19`)

1. **`arquivo::funcao -> contagem` é utilizável, mas não é estável contra refatoração.** A casa já possui uma convenção melhor descrita como `arquivo::escopo`: empilha classes e funções aninhadas e usa `<modulo>` quando não há escopo nomeado. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:91`, `:93`, `:132`)

   | Edição/caso | Consequência e nome recomendado |
   |---|---|
   | Inserir linhas/comentários | Preserva a chave; a linha serve apenas ao diagnóstico. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:132`) |
   | Renomear função, classe ou arquivo | Muda a chave: tratar como **reidentificação**, sem registrar como cura. Consequência da composição em `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:93`, `:132`. |
   | Mover função para outro arquivo ou escopo | Também muda a chave: exige correspondência explícita entre origem e destino. Mesma composição em `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:132`. |
   | Código no nível do módulo | `arquivo::<modulo>`. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:132`) |
   | Método de classe / função aninhada | `arquivo::Classe.metodo` / `arquivo::externa.interna`; usar somente o último nome perderia essa distinção. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:93`, `:132`) |
   | Lambda | Recomendo contar no escopo lexical que a contém, declarando essa agregação; é o comportamento do percurso existente, que não cria nome para lambda. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:93`, `:133`) |

   **A contagem mede ocorrências, não identidade dos leitores:** substituir uma ocorrência por outra dentro da mesma chave preservaria o número e escaparia dessa comparação. Limite inferido do formato do anexo; implementação da nova catraca **[não verificado]**.

   Falta definir quais formas entram: há `getattr(p, 'data_evento', None)` com nome literal fora do juiz e extrações explícitas por `values_list(..., 'data_evento')`. Esses casos precisam entrar ou aparecer nominalmente nos limites; não são nomes calculados dinamicamente. (`app/chamados/juizes.py:652`, `:854`; `app/chamados/services/fio.py:1854`, `:2024`)

   A exceção deve ser **somente a função indicada**, preservando os demais leitores de `juizes.py`: existe leitura direta em outra função desse mesmo arquivo. (`app/chamados/juizes.py:250`, `:271`, `:334`)

2. **A label `chamados` permite varrer todo `/app`; não fornece acesso ao serviço irmão `mensageria/`.** O P13 já demonstra um teste dessa label calculando a raiz por `__file__` e percorrendo todo `APP`, inclusive outros apps. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:33`, `:144`)

   - **Quem monta `LABELS`:** a fonte é `bin/regua.sh:23`; o pre-push lê essa declaração em `bin/pre-push.sh:184` e passa as labels ao runner em `:186`.
   - **Cwd configurado:** `/app`, em `app/Dockerfile:8`; os comandos mostrados não fornecem `-w`. (`bin/pre-push.sh:143`; `bin/regua.sh:166`)
   - **Arquivos disponíveis:** régua e pre-push montam apenas `app/` em `/app`; o archive do pre-push também extrai apenas `app`. Assim, `api/` e `templates/` ficam dentro da árvore disponível. (`bin/regua.sh:167`; `bin/pre-push.sh:144`; `bin/arvore_do_push.sh:98`; `app/config/settings/base.py:72`)
   - **Mensageria:** possui montagem própria de `./mensageria` em `/srv`. Para incluí-la, seria necessário ampliar extração, montagem e raiz do censo; trocar a label não resolve. (`docker-compose.mensageria.yml:3`, `:14`; `bin/arvore_do_push.sh:98`)

   Isso descreve a configuração da cópia; mounts e cwd de um container efetivamente executado são **[não verificado]**.

3. **O regex não perde uma leitura direta apenas porque houve `with/as` ou renomeação da variável.** Nos exemplos sintéticos abaixo, a ocorrência continua escrita na atribuição ou no acesso posterior; execução como template Django **[não verificado]**:

   | Forma sintética | Ocorrências de `\.data_evento\b` |
   |---|---:|
   | `{% with d=q.data_evento %}{{ d }}{% endwith %}` | 1 |
   | `{% with q.data_evento as d %}{{ d }}{% endwith %}` | 1 |
   | `{% with q as outra %}{{ outra.data_evento }}{% endwith %}` | 1 |
   | `{{ q\|attr:"data_evento" }}` | 0 |

   `|attr` é, portanto, uma lacuna sintática **se esse filtro estiver disponível**; sua disponibilidade nesta aplicação é **[não verificado]**. Filtros e tags podem esconder o acesso no Python: a casa já possui filtros que recebem a pergunta e delegam a leitura a uma função. (`app/chamados/templatetags/chamados_extras.py:15`, `:30`)

   O contrato deve chamar isso de **censo de acessos diretos no markup**, retirar comentários antes de contar e declarar acessos indiretos como limite. Há helper existente para comentários de bloco/HTML, mas ele não remove `{# ... #}`. (`app/core/censo_fonte.py:125`, `:132`, `:133`)

   O total Python **190** permanece **[não verificado]** sem o detector e o censo correspondentes; o subtotal direto dos templates da cópia é **5**, nos locais citados no veredito.

4. **Existem varredores de `data_evento`, porém os identificados julgam escrita.** O P13 AST procura atribuições e chamadas de escrita e autoriza `fio::gravar_dia_do_fato`; o contrato de estado também registra esse campo e procura escritas. Uma catraca de leitores tem objeto distinto e pode permanecer como instrumento sem recalcular o dia. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:37`, `:97`, `:103`, `:165`; `app/core/tests/test_contract_escritores_estado.py:64`, `:179`)

   Já existe reutilização do scanner P13 pelo selo E1: não convém copiar esse instrumento para reimplementar escrita. (`app/chamados/tests/test_e1_v15d_um_ponto_de_escrita.py:33`, `:51`, `:115`)

   Há uma **colisão concreta numa cura futura**: outro selo exige literalmente `data_pergunta=p.data_evento`. Migrar esse leitor ao juiz exigirá atualizar essa expectativa junto. (`app/chamados/tests/test_selo_derivacao_pela_data_da_pergunta.py:129`, `:130`)

   Não identifiquei outro censo geral de leituras; ausência absoluta **[não verificado]**.

5. **Os varredores de diretório relevantes impõem estas condições:**

   | Selo | Efeito sobre os arquivos propostos |
   |---|---|
   | O20 | Enumera `test_contract_*.py`: o nome `test_catraca_…` fica fora. Se renomeado, passa pelo teto de contratos que leem texto cru. (`app/core/tests/test_o20_contrato_julga_codigo.py:164`, `:176`, `:188`, `:204`) |
   | Relógio | Enumera `test*.py`; chamadas de relógio sem congelamento podem reprovar o novo teste. (`app/core/tests/test_selo_teste_sem_relogio.py:42`, `:53`, `:67`) |
   | CRON15 | Parseia todo `.py` diretamente em `*/tests/`; exige sintaxe válida e mantém fechada a lista de marcas. (`app/core/tests/test_marca_cron15.py:29`, `:33`, `:35`, `:50`) |
   | Vocabulário | Inclui testes e seu texto; aceita `.py/.html/.js/.md`, deixando o JSON fora. (`app/pautas/tests/test_contract_vocabulario.py:19`, `:20`, `:29`, `:41`) |
   | Testes fora do Git | Cobra o `.py` versionado; as colheitas mostradas não incluem JSON. (`bin/testes_fora_do_git.sh:46`, `:49`, `:57`) |
   | P13/escritores | Excluem `tests/` e filtram `.py`; as fixtures desse teste e seu JSON ficam fora dessas varreduras. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:147`, `:150`; `app/core/tests/test_contract_escritores_estado.py:191`, `:194`) |

   Não encontrei uma enumeração geral de `tests/*.json`; garantia de ausência **[não verificado]**. O selo de labels verifica apps com testes, não a permanência desse arquivo específico; o selo de Git tampouco guarda arquivos removidos. Portanto, manter a remoção do guardião como `!` declarado é coerente, mas não prometer proteção automática por esses selos. (`bin/tests/test_labels_fonte_unica.sh:24`, `:30`, `:32`; `app/core/tests/test_selo_testes_no_git.py:42`)

   A promessa de rodar “em segundos”, o caso injetado que fica vermelho e a monotonicidade contra uma edição simultânea que aumente o JSON continuam **[não verificado]** sem implementação e execução do instrumento.

## 11/11 15:12 · sessao 96eb919cc168 · - · - · origem=Ronald (tmux)

Para (5), não há colisão demonstrada apenas por criar os dois nomes propostos; sem implementação, colisões por conteúdo são [não verificado].

- **O20:** enumera somente `*/tests/test_contract_*.py`; `test_catraca_data_evento_cru.py` fica fora desse censo. Se renomeado para `test_contract_*`, as leituras cruas passam pelo teto zero, com tratamento explícito para AST e exceções declaradas. (`app/core/tests/test_o20_contrato_julga_codigo.py:160`, `:176`, `:178`, `:188`, `:197`, `:204`)
- **Relógio:** o novo `.py` entra porque o selo enumera `test*.py`; chamadas reais de relógio sem congelamento podem provocar vermelho. O JSON fica fora. (`app/core/tests/test_selo_teste_sem_relogio.py:32`, `:42`, `:48`, `:53`, `:67`)
- **Marca CRON15:** enumera todos os `.py` diretamente em `*/tests/`, faz `ast.parse` sem tolerar erro, e só admite os 15 métodos marcados. O JSON é excluído pela extensão. (`app/core/tests/test_marca_cron15.py:25`, `:29`, `:32`, `:35`, `:48`)
- **Raio de um sítio:** inclui testes, pois as exclusões são apenas migrations/node_modules/staticfiles; exige Python compilável e procura `or` com leitura de `raio_metros`. Uma fixture textual `"p.data_evento"` não satisfaz esse critério. JSON fica fora do `rglob('*.py')`. (`app/core/tests/test_selo_raio_de_um_sitio.py:44`, `:45`, `:50`, `:66`, `:73`, `:92`, `:97`)
- **Vocabulário:** varre inclusive testes e prosa do `.py`; palavras proibidas podem colidir. Só aceita extensões `.py/.html/.js/.md`, excluindo JSON. (`app/pautas/tests/test_contract_vocabulario.py:19`, `:20`, `:29`, `:32`, `:35`, `:41`)
- **Testes fora do Git:** cobra o novo `.py` versionado; não inclui o JSON nas duas colheitas mostradas. Esse selo não prova a existência obrigatória do novo teste: o próprio teste de comparação declara que arquivo no Git e ausente da árvore está fora de seu escopo. (`bin/testes_fora_do_git.sh:46`, `:48`, `:49`, `:57`; `app/core/tests/test_selo_testes_no_git.py:42`)
- **P13/escritores:** não acusam fixtures dentro de `chamados/tests/`, pois excluem a pasta; JSON também é excluído pelo filtro `.py`. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:146`, `:147`, `:150`; `app/core/tests/test_contract_escritores_estado.py:190`, `:191`, `:194`)

Não encontrei uma varredura geral de `tests/*.json`; afirmar que nenhuma existe em toda a árvore continua **[não verificado]**.

## 11/11 15:10 · sessao 26eb251345fa · - · - · origem=Ronald (tmux)

A label `chamados` pode conter um teste que leia todo `/app`; ela seleciona testes, enquanto o alcance da varredura depende do código do teste. Há precedente: o selo P13 calcula a raiz de `app/` por `__file__` e executa `os.walk(APP)`, atravessando os outros apps. `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:33,144-155`.

| Ponto | Evidência |
|---|---|
| Fonte única das labels, incluindo `api` e `chamados` | `bin/regua.sh:23` |
| Pre-push monta `LABELS` lendo essa fonte | `bin/pre-push.sh:184-185` |
| Pre-push passa labels ao runner Django | `bin/pre-push.sh:186-188` |
| Régua passa `${ONLY:-$LABELS}` ao runner | `bin/regua.sh:168-169` |
| Régua monta somente `~/saas-hasner/app` em `/app` | `bin/regua.sh:14,166-167` |
| Pre-push monta somente `$ARVORE_PUSH/app` em `/app` | `bin/pre-push.sh:142-145` |
| Cópia do pre-push extrai apenas `app` do commit | `bin/arvore_do_push.sh:98` |
| Cwd previsto para a imagem core: `/app` | `app/Dockerfile:8` |
| Templates são de `BASE_DIR / 'templates'`, sendo `BASE_DIR` a raiz de `app` | `app/config/settings/base.py:4,70-74` |
| `mensageria/` é outro serviço, construído de `./mensageria`, montado em `/srv` | `docker-compose.mensageria.yml:3,13-14` |
| Cwd desse outro serviço é `/srv` | `mensageria/Dockerfile:2` |

**Conclusão para o contrato:** uma varredura da raiz `/app` alcança `api/` e `templates/`; não alcança o diretório irmão `mensageria/`, pois ele não é extraído nem montado no container de teste core. `bin/arvore_do_push.sh:98`; `bin/pre-push.sh:144`; `bin/regua.sh:167`; `docker-compose.mensageria.yml:13-14`.

Se “todo o app” inclui o serviço irmão `mensageria/`, o contrato precisa declarar esse limite ou mudar a extração/montagem e a raiz da varredura; trocar apenas a label não fornece os arquivos ausentes. `bin/arvore_do_push.sh:98`; `bin/pre-push.sh:144,186-188`.

O selo de labels cobra apps Django com testes sob `app/*/`, identificados por `apps.py`; ele não prova cobertura do serviço irmão. `bin/tests/test_labels_fonte_unica.sh:24-35`.

Cwd e mounts efetivos da imagem/container em execução: **[não verificado]**; as referências acima descrevem a configuração da cópia.

## 11/11 15:10 · sessao 397f44057cf9 · - · - · origem=Ronald (tmux)

Não encontrei catraca geral de **leituras** de `data_evento`; ausência absoluta fica [não verificado]. Os instrumentos próximos têm objetos distintos:

- **P13 AST controla escrita:** detector em `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:78`, percurso de produção em `:144`, exclusões em `:147`, único ponto autorizado em `:37` e reprovação em `:165`.
- **P13 de estado também controla escrita**, por regex, em seis apps: `app/core/tests/test_contract_escritores_estado.py:12`, `:13`, `:64`, `:179`, `:186`. Cobra novos escritores e entradas mortas em `:203` e `:212`.
- **E1 15d2 reutiliza P13**, inclusive seu censo; já há sobreposição consciente de verificações de escrita: `app/chamados/tests/test_e1_v15d_um_ponto_de_escrita.py:33`, `:51`, `:115`. Uma catraca de leitores não responde à mesma pergunta desses instrumentos. [inferência: referências anteriores]
- **Dia do fato** varre a chave `data_turno` em `.get`/subscript, não `data_evento`: `app/chamados/tests/test_contract_dia_do_fato.py:35`, `:59`. **Vitalidade** varre `materializada_em`/`validada_em`: `app/chamados/tests/test_contract_leitores_vitalidade.py:14`, `:305`.

Há uma colisão concreta ao **curar dívida depois**: o selo exige literalmente `data_pergunta=p.data_evento` no reconciliador. Migrar esse leitor ao juiz exigirá ajustar o selo correspondente, preservando seu caso comportamental: `app/chamados/tests/test_selo_derivacao_pela_data_da_pergunta.py:122`, `:129`, `:130`.

## 11/11 15:08 · sessao 88a351155ddf · DATA-EVENTO-CRU · censo · origem=Code

O censo da cópia resulta em **341 locais leitores Python: 190 operacionais, 149 em testes e 2 em migração**. A unidade é o par único `arquivo:linha`; acessos repetidos na mesma linha contam uma vez. A relação integral está abaixo, incluindo expansões ORM como `values(*chave)` (`app/chamados/services/fio.py:1948`) e consumo de chave serializada (`mensageria/nucleo/ferramentas.py:127`).

O juiz retorna **`None` para conversa**, independentemente do dia civil gravado (`app/chamados/juizes.py:338`). Nos quadros, **S** significa “sim, o cru pode fornecer o dia civil onde o juiz fornece `None`”; **N**, “não, há exclusão/tratamento de conversa”; **NA**, “não se aplica à pergunta sobre dia do fato”. **S\*** significa “sim se conversa alcançar esse ramo”; a ocorrência concreta permanece [nao verificado].

A identidade da cópia com o HEAD solicitado está [nao verificado]: não há metadados Git disponíveis. Execução e impacto em produção estão [nao verificado].

**(a) Python — leitores operacionais: 190 locais.**

Nas tabelas, `arquivo:1,2` enumera dois locais: `arquivo:1` e `arquivo:2`.

| Arquivo:linhas | Função | Uso do valor | Conversa |
|---|---|---|---|
| `app/api/views.py:1165,1166` | `api_chamados_v2` | Repassa ISO e rótulo de dia | S |
| `app/api/views.py:2637` | `api_disputa_responder` | Repassa dia em auditoria | S |
| `app/api/views_mensageria.py:568` | `_item_validar` | Repassa ISO na fila | S |
| `app/chamados/canal.py:65,67,72,79` | `perguntas_mudas_com_resposta` | Filtra não nulo; ordena; associa resposta por dia; repassa | S |
| `app/chamados/juizes.py:80,81` | `_dia_da_disputa` | Reúne dias para escolher o menor; conversa excluída em `:78` | N |
| `app/chamados/juizes.py:214` | `dias_do_fato` | Reúne dias; conversa excluída em `:213` | N |
| `app/chamados/juizes.py:271` | `_plantio_na_hora_do_slot` | Ancora comparação de plantio em D/D+1 | S* |
| `app/chamados/juizes.py:652` | `_celulas_da_pergunta` | Confere dia da célula contra pergunta | S* |
| `app/chamados/juizes.py:854` | `_chave_memo_pv` | Compõe chave de invalidação do memo | NA |
| `app/chamados/juizes.py:1788` | `_marco_esperado_da_grade` | Consulta grade pelo dia cru | S* |
| `app/chamados/juizes.py:2327,2331` | `_dias_das_perguntas_vivas` | Filtra/reúne dias; `conversa=False` em `:2328` | N |
| `app/chamados/models.py:364` | `ChamadoColaborador._vivas_entre` | Consulta competência entregue | S |
| `app/chamados/models.py:390,391` | `ChamadoColaborador._plano_pergunta_viva` | Busca cobrança sobrevivente no dia | S |
| `app/chamados/models.py:2753` | `PerguntaDisputa.q_dos_fatos` | Predicado ORM por dia; exclui conversa em `:2754` | N |
| `app/chamados/models.py:2940,2956,2962` | `PerguntaDisputa._nascer` | Compara rechaveamento; relata recusa; captura estado anterior; ramo `not conversa` em `:2893` | N |
| `app/chamados/models.py:3228` | `PerguntaDisputa.linhas_do_evento` | Projeta dia para acordar células | S |
| `app/chamados/reconciliador.py:370` | `lavrar_sem_turno_aberto` | Consulta turno aberto pelo dia | S* |
| `app/chamados/reconciliador.py:736` | `_dia_fora_do_alcance_c` | Testa presença de data para o alcance | S |
| `app/chamados/reconciliador.py:1050` | `_orfas_abertas` | Filtra data não nula | S |
| `app/chamados/reconciliador.py:1082,1088,1090` | `veredito_orfa` | Testa data; compara com hoje; consulta encerramento do dia | S* |
| `app/chamados/reconciliador.py:1152` | `encerrar_orfas_pela_lampada` | Repassa dia em auditoria | S* |
| `app/chamados/reconciliador.py:1303,1305,1307` | `semear_eventos_celula` | Deduplica e localiza célula por dia | S* |
| `app/chamados/reconciliador.py:1557,1567` | `reatar_pergunta_orfa` | Deriva destino e pesquisa histórico pelo dia | S* |
| `app/chamados/services/aviso_pendencia.py:16` | `aviso_texto_livre` | Exibe dias cobrados na mensagem | S |
| `app/chamados/services/carga_pergunta_viva.py:135` | `CargaDaVida.__init__` | Monta janela de testemunhas D/D+1 | S* |
| `app/chamados/services/carga_pergunta_viva.py:147` | `CargaDaVida.trancada` | Consulta par colaborador/dia trancado | S* |
| `app/chamados/services/cobranca.py:83` | `checar_cobranca` | Reúne/ordena dias das pendências | S |
| `app/chamados/services/competencia_trancada.py:107` | `medir` | Filtra perguntas pela competência | S |
| `app/chamados/services/competencia_trancada.py:252,256` | `reabrir` | Filtra competência; repassa dia na auditoria | S |
| `app/chamados/services/declaracao_texto.py:590` | `insumos_em_regularizacao` | Agrupa por motivo/dia; origem `linhas_dos_fatos` em `:587` | N |
| `app/chamados/services/disputa_emissao.py:399,418` | `gerar_perguntas_disputa` | Filtra pergunta existente pelo dia, com `conversa=False` | N |
| `app/chamados/services/disputa_emissao.py:1074` | `criar_disputa_a_partir_de_chamados_orfaos` | Usa dia da pergunta como fallback do contexto | S* |
| `app/chamados/services/disputa_emissao.py:1415` | `_ja_perguntou` | Filtra motivo/dia com `conversa=False` | N |
| `app/chamados/services/disputa_emissao.py:1498,1531` | `responder_pergunta_disputa_core` | Repassa dia em recusa/auditoria | S |
| `app/chamados/services/disputa_emissao.py:1863,1935` | `_resolver_perguntas_por_chamado_resolvido` | Entrega dia a `via_pelo_fato` | S* |
| `app/chamados/services/explicador.py:70` | `explicar_pergunta` | Exibe dia na explicação | S |
| `app/chamados/services/explicador.py:122,126` | `por_que_nao_fecha` | Ordena por dia; monta janela de batidas | S |
| `app/chamados/services/fabrica_da_celula.py:148` | `motivos_a_perguntar` | Filtra dia com `conversa=False` | N |
| `app/chamados/services/fabrica_da_celula.py:170` | `perguntar` | Filtra dia com `conversa=False` | N |
| `app/chamados/services/fio.py:87` | `data_do_fato_de` | Retorna cru como fato e compara com dia do chamado | S |
| `app/chamados/services/fio.py:776` | `_costura_nao_mata_o_fio` | Consulta competência pelo cru | S |
| `app/chamados/services/fio.py:867,934` | `reapontar_resolvedora` | Testa necessidade de datar; compara célula anterior/posterior | S |
| `app/chamados/services/fio.py:953` | `_colisao_ao_datar` | Confere estado gravado contra data pedida | NA |
| `app/chamados/services/fio.py:1013` | `rechavear_data_evento` | Captura dia anterior; conversa recusada em `:1018` | N |
| `app/chamados/services/fio.py:1068,1072` | `rechavear_no_reuso` | Consulta veto de competência; relata dia anterior | S* |
| `app/chamados/services/fio.py:1109` | `reuso_sem_rechave` | Compara gravado com juiz do turno; universo `conversa=False` em `:1099` | N |
| `app/chamados/services/fio.py:1133,1136` | `violacao_de_saida` | Confere valor persistido contra valor entregue ao escritor | NA |
| `app/chamados/services/fio.py:1362` | `retratar_duplicata` | Repassa dia na justificativa | S* |
| `app/chamados/services/fio.py:1412` | `q_do_horizonte` | Filtra horizonte com `conversa=False` | N |
| `app/chamados/services/fio.py:1419` | `q_do_horizonte_ou_sem_dia` | Acrescenta teste de data nula ao horizonte | S |
| `app/chamados/services/fio.py:1447` | `_vizinhos_do_fato` | Consulta identidade pelo dia; conversa retorna vazio em `:1445` | N |
| `app/chamados/services/fio.py:1460` | `chave_do_dia` | Compõe identidade com o cru | S* |
| `app/chamados/services/fio.py:1495,1497` | `_dia_do_fato` | Usa cru no ramo de instância de marco | S* |
| `app/chamados/services/fio.py:1630` | `dia_do_turno_da_linha` | Fallback do escritor; conserva dia civil da conversa em `:1632` | S — contrato de escrita |
| `app/chamados/services/fio.py:1800` | `universo_do_fato` | Compara identidade; exige `not p.conversa` em `:1801` | N |
| `app/chamados/services/fio.py:1854` | `fecho_cruza_dias` | Projeta dia para agrupar; universo exclui conversa em `:1852` | N |
| `app/chamados/services/fio.py:1946,1948,1949,1950,1955,1958,1970,1985` | `grupos_de_colisao` | Filtra nulo; agrupa/ordena por chave; lê dict de `values`; correlaciona `OuterRef`; filtra/recorta grupos | N — `_no_fato` exclui conversa em `:1433` |
| `app/chamados/services/fio.py:2000,2005` | `fora_do_indice` | Conta perguntas sem dia pelo ORM | N — mesma origem `_no_fato` |
| `app/chamados/services/fio.py:2024` | `batida_sem_origem_no_horizonte` | Projeta dia para distinguir horizonte/acervo | N — mesma origem `_no_fato` |
| `app/chamados/services/historico.py:108` | `_eventos_dos_chamados` | Exibe dia na narrativa do histórico | S |
| `app/chamados/services/limbo_folgas.py:203` | `filhos_no_defeito` | Filtra dia com `conversa=False` | N |
| `app/chamados/services/materializacao.py:280,283,284` | `_tipo_pelo_marco` | Consulta grade pelo dia cru | S* |
| `app/chamados/services/materializacao.py:346,347` | `_ts_tipo_do_plantio` | Usa cru como data-base do plantio | S* |
| `app/chamados/services/materializacao.py:478,479` | `_declarar_ausencia_de_pergunta` | Usa cru como dia da ausência | S* |
| `app/chamados/services/materializacao.py:522,538` | `_materializar_ausencia_de_resposta` | Cria ausência no dia; filtra perguntas irmãs | S* |
| `app/chamados/services/materializacao.py:629` | `perguntas_absorvidas_por` | Filtra perguntas pelo dia da ausência | S* |
| `app/chamados/services/materializacao.py:1270` | `_materializar_uma` | Entrega dia a `via_pelo_fato` | S* |
| `app/chamados/services/passivo_validado.py:113` | `_dia_do_plantio` | Retorna cru ou dia local do instante | S* |
| `app/chamados/services/passivo_validado.py:519,520` | `_plantar_o_dia` | Acrescenta dia cru ao conjunto afetado | S* |
| `app/chamados/services/pergunta_no_app.py:115` | `retencao` | Testa dia, mas exige `not conversa` no mesmo predicado | N |
| `app/chamados/services/relavra_via.py:103` | `via_de_hoje` | Entrega dia ao juiz da via; conversa excluída em `:101` | N |
| `app/chamados/services/relavra_via.py:154` | `relavrar_dia` | Filtra dia com `conversa=False` | N |
| `app/chamados/services/tarefa_do_dia.py:236` | `_tarefa_pura` | Seleciona perguntas do dia | N no carregador operacional, que exclui conversa em `:375` |
| `app/chamados/services/tarefa_do_dia.py:376,380` | `CargaDaTarefa.__init__` | Filtra janela e agrupa por dia; `conversa=False` em `:375` | N |
| `app/chamados/services/tarefa_do_dia.py:486` | `CargaDaTarefa._ler_vida` | Exclui pares exportados pelo dia | N — mesmo carregamento |
| `app/chamados/services/validacao.py:269` | `_aceitar_a_folga` | Usa cru como dia a declarar | S* |
| `app/chamados/services/validacao.py:446` | `validar_pergunta` | Dispara recálculo pelo dia cru | S* |
| `app/chamados/services/validacao.py:497,510` | `validar_pergunta` | Repassa dia na auditoria | S |
| `app/chamados/services/vigia_fio.py:816` | `carimbos_sem_lastro_por_dono` | Classifica competência entregue pelo cru | S |
| `app/chamados/veredito_lavrado.py:273` | `lavrar` | Calcula expiração da janela pelo cru | S |
| `app/chamados/views_cobrar.py:176,185` | `validar_inline` | Ordena perguntas por dia; lê chave serializada do canal | S |
| `app/colaboradores/services/calendario.py:473,477` | `contexto_calendario` | Filtra intervalo e indexa pergunta viva por dia | S |
| `app/folha/previa.py:65,67` | `montar_previa` | Filtra competência e agrupa perguntas por dia | S |
| `app/ponto/management/commands/mapa_divergencia.py:272,273` | `Command.handle` | Filtra/projeta dias; conta furo coberto por pergunta | S |
| `app/ponto/management/commands/vigia_plantio_overnight.py:75,86,90` | `batidas_fora_do_turno_da_pergunta` | Filtra FK; monta janela; compara turno com cru | S* |
| `app/ponto/services/dia_do_colab.py:65` | `dia_do_colab` | Filtra perguntas para o relato do dia | S |
| `app/ponto/services/veredito_celula.py:180` | `fechar_o_fio_do_dia` | Filtra dia com `conversa=False` | N |
| `mensageria/nucleo/ferramentas.py:127` | `fila_de_validacao` | Lê JSON e renomeia para `data_do_caso` | S |

Os comandos operacionais completam os 190 locais:

| Arquivo:linhas | Função | Uso | Conversa |
|---|---|---|---|
| `app/chamados/management/commands/auditar_invariantes_chamados.py:63,73,74` | `_candidatos_i8` | Pesquisa chamados pelo dia | S* |
| Mesmo arquivo`:148` | `_contraprova_i8bx` | Confere competência exportada | S* |
| Mesmo arquivo`:160,162` | `_cobre_ausencia_i21` | Consulta cobertura de ausência | S |
| Mesmo arquivo`:282` | `_piso_f3_antes_do_gerador` | Compara dia com primeira célula | S |
| Mesmo arquivo`:298` | `_contraprova_f3g` | Pesquisa célula até o dia cru | S |
| Mesmo arquivo`:325,326,348,361,373` | `contraprova_piso` | Agrupa por dia; consulta célula, vínculo e competência | S |
| Mesmo arquivo`:892` | `Command.handle`, I8b | Filtra data não nula | S |
| Mesmo arquivo`:914,926,930` | `Command.handle`, I8b | Confere competência/célula após recusa da resolvedora | N para conversa aceita pela resolvedora (`app/chamados/juizes.py:2251`) |
| Mesmo arquivo`:957` | `Command.handle`, I10 | Conta data nula | S |
| Mesmo arquivo`:1665,1708` | `Command.handle`, F1 | Escolhe dia e consulta perguntas desse dia | S |
| Mesmo arquivo`:1778,1779,1804` | `Command.handle`, F3 | Filtra passado e consulta lei do dia | S |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:60,69` | `linhas_a_criar`, `_escoar` | Carrega/lê cru; conversa vai a balde sem competência em `:73` | N no resultado |
| Mesmo arquivo`:104` | `admin_decidiu_sem_ato` | Filtra nulo/horizonte com discriminação de conversa | N |
| Mesmo arquivo`:115,118,120` | `competencia_de` | Calcula competência pelo cru | S* |
| Mesmo arquivo`:282,283` | `aplicar` | Conta perdedoras sem dia/abaixo do horizonte | N — universo do fato |
| Mesmo arquivo`:371` | `_grupos_fora` | Projeta dia para o recorte dos grupos | N — universo do fato |
| `app/chamados/management/commands/backfill_veredito_lavrado.py:111` | `Command.handle` | Calcula expiração pelo cru | S |
| `app/chamados/management/commands/dedup_perguntas_fato.py:85,86` | `Command.handle` | Conta perdedoras com/sem dia | N — grupos do fato |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:74` | `Command.handle` | Repassa dia na justificativa | S* |
| `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:97` | `_fonte_viva` | Compara fonte/turno com dia gravado | N no comando: `conversa=False` em `:206` |
| Mesmo arquivo`:144,148,158,163` | `classificar` | Monta janela; captura/compara dia anterior | N no mesmo universo |
| Mesmo arquivo`:205,209,211` | `Command.handle` | Filtra não nulo e intervalo | N — `:206` |
| Mesmo arquivo`:309` | `Command._veto_do_grupo` | Projeta dias para veto de competência | S* |
| Mesmo arquivo`:392` | `Command._migrar` | Relê dia persistido | N no universo migrado |
| Mesmo arquivo`:442,450` | `Command._descarimbados` | Busca perguntas dos dias antigo/novo e captura dia | S |
| Mesmo arquivo`:481,488` | `Command._reverter` | Lê JSON de reversão e consulta veto; exclui conversa em `:477` | N |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py:68,86,91,102,117,144,145` | `Command.handle` | Filtra não nulo; calcula competência; relata dias | S* |

**Testes: 149 locais leitores.** Todos abaixo verificam campo persistido, snapshots, resultado serializado ou filtros de fixtures; a divergência operacional é **NA**. A função exata de cada linha também é emitida por `--details` no script. Como referência explícita do tratamento civil de conversa: `app/chamados/tests/test_e1_v15.py:79`.

Para compactar os nomes de função, `T:` abaixo significa o prefixo literal `test_`.

| Arquivo:linhas | Função — correspondência das linhas |
|---|---|
| `app/chamados/tests/test_a2_batida_sem_linha.py:32,33` | `T:MORDE_batida_sem_linha_nao_inventa_dia_pelo_timestamp` |
| `app/chamados/tests/test_a2_data_evento_batida.py:27,38,51,56` | `27,38 T:pergunta_de_batida_nasce_datada_e_dedupa`; `51 T:batida_23h30_local_data_do_dia_local_nao_utc`; `56 T:contexto_sem_ultima_batida_nasce_conversa` |
| `app/chamados/tests/test_b2_data_conversa.py:23,24` | `T:conversa_nasce_sempre_e_datada` |
| `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:127` | `_emitido` |
| `app/chamados/tests/test_data_do_fato_divergente.py:197` | `T:GOLDEN_conferir_carimba_e_deixa_trilha` |
| `app/chamados/tests/test_declaracao_texto.py:114` | `T:porta_colab_sem_texto_livre` |
| `app/chamados/tests/test_e1_identidade_corrida.py:77` | `T:MORDE_duas_na_janela_nascem_UMA_pergunta` |
| `app/chamados/tests/test_e1_v13.py:67` | `_estado_das_linhas` |
| `app/chamados/tests/test_e1_v13d_exportada_fora.py:223,224,225,244,246,253,269,270,271,300,404` | `223–225 T:RED_a_linha_da_exportada_fica_fora_contada_e_as_outras_migram`; `244,246 T:RED_materializada_exportada_nao_freia_as_abertas_pelo_descarimbo`; `253 T:RED_o_dry_lista_a_exportada_e_nao_escreve`; `269–271 T:RED_o_GRUPO_sai_inteiro_a_linha_aberta_do_fato_da_exportada_nao_migra`; `300 T:MORDE_virou_exportada_entre_a_classificacao_e_a_linha`; `404 T:RED_reversao_passa_pelo_mesmo_veto` |
| `app/chamados/tests/test_e1_v15.py:31,59,73,79,102,134` | `31 _estado`; `59 T:RED_nasce_no_dia_do_juiz_com_a_celula_dele`; `73 T:pedido_que_o_juiz_confirma_grava_o_pedido`; `79 T:conversa_grava_o_dia_civil`; `102 T:RED_reuso_costura_no_chamado_do_dia_do_turno`; `134 T:RED_datar_grava_o_dia_do_juiz` |
| `app/chamados/tests/test_e1_v15b.py:45,90,106` | `45 _estado`; `90 T:RED_3a_episodio_retratado_recusa_as_duas_chamadas_sem_escrever`; `106 T:RED_datar_episodio_retratado_recusa_sem_gravar` |
| `app/chamados/tests/test_e1_v15c2_descarimbo_efeito.py:58,59,71,78,87` | `58,59 T:RED_passivo_da_linha_FORA_nao_freia_o_apply`; `71 T:MORDE_a_carimbo_vivo_no_dia_NOVO_da_linha_que_migra_e_PAREI`; `78 viva`; `87 T:MORDE_b_delta_do_universo_maior_que_zero_e_PAREI` |
| `app/chamados/tests/test_e1_v15d.py:39,74` | `39 _estado`; `74 T:RED_R1_reuso_rechaveia_no_mesmo_ato_e_2x_e_o_mesmo_estado` |
| `app/chamados/tests/test_e1_v15d_um_ponto_de_escrita.py:200,212,214,218,231,307,313,318,328` | `200 T:RED_N_nascer_grava_o_dia_do_juiz_pelo_ponto`; `212,214,218 T:RED_U_reuso_grava_o_dia_do_juiz_pelo_ponto`; `231 T:RED_C_costura_grava_o_dia_do_juiz_pelo_ponto`; `307,313,318 T:RED_Z_reuso_de_linha_sem_dia_recusa_declarado_sem_escrita`; `328 T:RED_K_contador_ve_a_linha_sem_dia` |
| `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:245,289,291,435,436,444,445,455,457,466,467,469,484` | `245 _estado`; `289,291 T:o_caminho_certo_cumpre_o_contrato`; `435,436 _cen_nascer`; `444,445 _cen_reuso`; `455,457 _cen_nula`; `466,467,469 _cen_outro_episodio`; `484 _cen_noturno` |
| `app/chamados/tests/test_e1_v15f.py:69,83,127,170,246,247,259,266,268,277` | `69 _conversa`; `83 T:RED_o_apply_nao_move_a_conversa`; `127 T:RED_reuso_com_rechave_e_costura_trava_na_ordem`; `170 grava`; `246,247 T:G7_colisao_no_apply_a_segunda_recusa`; `259 porta`; `266,268 T:G8_escrita_parcial_no_grupo_desfaz_o_grupo_inteiro`; `277 _estado` |
| `app/chamados/tests/test_e1_v15g.py:52,94,207` | `52 _estado`; `94 T:contra_exemplo_destino_do_mesmo_colaborador_segue`; `207 T:A5_real_linha_do_episodio_gravada_em_D0_o_juiz_decide_e_reusa` |
| `app/chamados/tests/test_e1_v15g_portas.py:39,106,151,372` | `39 _estado`; `106 T:contra_exemplo_a_linha_de_fato_segue_re_chaveando`; `151 T:contra_exemplo_costurar_a_conversa_sem_datar_segue`; `372 T:DECLARA_a_conversa_viva_do_velho_segue_o_chamado_sem_tocar_o_fato` |
| `app/chamados/tests/test_e1_v15h.py:109` | `T:RED_ramo_que_data_recusa_e_nao_data` |
| `app/chamados/tests/test_e1_v5_indice.py:65` | `T:MORDE_marco_duas_cruas_na_janela_dao_UMA_linha` |
| `app/chamados/tests/test_e1_v5_portas_backfill.py:69,80,114,129,147,193` | `69 T:reapontar_que_data_para_fato_existente_recusa`; `80 T:rechavear_para_fato_existente_recusa`; `114 T:p_canonica_retrata_a_outra_viva_e_e_datada`; `129 T:MORDE_outra_encerrada_recusa_e_nada_se_desfaz`; `147 T:MORDE_destino_recusado_DEPOIS_da_colisao_nao_retrata_ninguem`; `193 _dup_cru` |
| `app/chamados/tests/test_e1_v6_horizonte.py:97,454` | `97 T:RED_o_banco_nao_indexa_o_acervo_abaixo_do_horizonte`; `454 T:RED_conversa_do_episodio_retratado_nao_nasce_duas_vezes` |
| `app/chamados/tests/test_e1_v8.py:148` |

[truncado em 20000 de 39862 caracteres]

## 11/11 15:05 · sessao a0ad56da25bd · PROPORCAO-POR-RISCO · verificador · origem=Code

**APROVADO no delta v6.** A leitura da lista na BASE fecha a autoautorização dentro do intervalo julgado; a confiança na base continua sendo responsabilidade externa ao selo (`bin/selo_proporcao.py:47`, `bin/selo_proporcao.py:78`, `bin/selo_proporcao.py:91`). Bloqueantes comprovados contra a letra B3: nenhum; B3 exige corte do Ronald, mas não exige base igual a `main` nem autenticação automática desse corte (`aval_proporcao_b3_resposta_1111.txt:1`).

Todos os rc abaixo são **deduzidos por leitura**. Execução dos casos, mutantes, vínculo Git da cópia ao hash informado e uso em produção: [nao verificado].

**1) Autoautorização**

O ataque v5, **no mesmo diff e com a base original**, foi fechado: acrescentar escritor e `CORTES.md` à lista de head não altera a lista utilizada. Ambos ficam fora da lista original, produzindo **A/rc 1 quando declarado B**; o novo caso reproduz essa entrada (`bin/selo_proporcao.py:91`, `bin/selo_proporcao.py:60`, `bin/selo_proporcao.py:108`; `bin/tests/test_selo_proporcao.sh:78`).

**Ainda existe o caminho pela escolha de uma base já ampliada.** Contraexemplo mínimo em dois commits, construído para análise; execução: [nao verificado]. [nao verificado]

- **Commit P:** em `app/docs/CORTES.md:444`, inserir ``; `app/chamados/writer.py` `` imediatamente antes do marcador final.
- **Commit H, filho de P:** acrescentar: [nao verificado]

```diff
diff --git a/app/chamados/writer.py b/app/chamados/writer.py
new file mode 100644
--- /dev/null
+++ b/app/chamados/writer.py
@@ -0,0 +1,2 @@
+from chamados.models import ChamadoColaborador
+ChamadoColaborador.objects.update(status_local='fechado')
```

A chamada `bin/selo_proporcao.sh P H B <raiz>` recebe **B/rc 0 deduzido**: P é ancestral, sua lista já contém o escritor, e o diff P→H contém somente esse caminho (`bin/selo_proporcao.py:89`, `bin/selo_proporcao.py:91`, `bin/selo_proporcao.py:60`, `bin/selo_proporcao.py:111`). O campo utilizado existe; efeito dessa entrada em banco: [nao verificado] (`app/chamados/models.py:52`).

Isso **exclui do julgamento o commit que ampliou a lista**. Julgar desde o antecessor de P, com a lista original, inclui `CORTES.md` e o escritor e resulta em A/rc 1 declarado B (`bin/selo_proporcao.py:92`, `bin/selo_proporcao.py:60`, `bin/selo_proporcao.py:108`). Portanto, o contraexemplo demonstra o limite da base escolhida; não reproduz o defeito v5 para o mesmo intervalo (`bin/selo_proporcao.py:91`).

**2) O que a BASE garante**

| Entrada | Resultado deduzido |
|---|---|
| Revisão inexistente ou que não resolve para commit | **2**, na resolução (`bin/selo_proporcao.py:85`, `bin/selo_proporcao.py:98`). |
| Commit forjado, existente e ancestral de head | Pode ser aceito: não há verificação de procedência ou corte (`bin/selo_proporcao.py:84`, `bin/selo_proporcao.py:89`, `bin/selo_proporcao.py:91`). |
| Base diferente de `main` | Pode ser aceita; a validação usa ancestralidade, sem comparação com `main` (`bin/selo_proporcao.py:89`). |
| Commit anterior da própria raia já ampliou a lista | Pode produzir **B/rc 0**, conforme P→H acima (`bin/selo_proporcao.py:91`, `bin/selo_proporcao.py:60`, `bin/selo_proporcao.py:111`). |
| `CORTES.md` ausente na base | **2**: `git show` falha e vira `NaoJulgado`; não há fallback para head ou disco (`bin/selo_proporcao.py:47`, `bin/selo_proporcao.py:41`, `bin/selo_proporcao.py:98`). |

O **chamador escolhe a base**, pelo primeiro argumento; o wrapper repassa os argumentos integralmente (`bin/selo_proporcao.sh:7`, `bin/selo_proporcao.py:78`). No harness, a base normal é o commit sintético inicial; nos casos de lista torta, é o commit sintético que contém a deformação (`bin/tests/test_selo_proporcao.sh:29`, `bin/tests/test_selo_proporcao.sh:47`, `bin/tests/test_selo_proporcao.sh:50`). Quem escolhe e valida a base da obra real: [nao verificado].

O selo comprova resolução para commit, desigualdade, ancestralidade, lista formalmente válida e diff não vazio. **Não comprova corte do Ronald nem que o intervalo abrange a obra inteira**, cuja unidade declarada é “obra” (`bin/selo_proporcao.py:85`, `bin/selo_proporcao.py:87`, `bin/selo_proporcao.py:89`, `bin/selo_proporcao.py:91`, `bin/selo_proporcao.py:94`; `app/docs/CORTES.md:443`, `app/docs/CORTES.md:444`).

**3) Os oito `caso_lista`**

Por leitura, **nenhum dos oito foi deslocado para rc 2 por falha de preparo**: todos alteram a árvore inicial, `prepara` exige sucesso e commit novo, e o head seguinte modifica o template inicial de `<p>a</p>` para `z` (`bin/tests/test_selo_proporcao.sh:24`, `bin/tests/test_selo_proporcao.sh:31`, `bin/tests/test_selo_proporcao.sh:46`, `bin/tests/test_selo_proporcao.sh:48`).

| Caso | Motivo previsto de rc 2 |
|---|---|
| `sem_marcador` | Contagem dos marcadores (`bin/tests/test_selo_proporcao.sh:92`; `bin/selo_proporcao.py:48`). |
| `lista_vazia` | Item vazio, guarda de forma (`bin/tests/test_selo_proporcao.sh:93`; `bin/selo_proporcao.py:51`, `bin/selo_proporcao.py:53`). |
| `so_inicio` | Falta marcador final (`bin/tests/test_selo_proporcao.sh:94`; `bin/selo_proporcao.py:48`). |
| `fim_antes` | Cardinalidade válida; fatia vazia gera item inválido (`bin/tests/test_selo_proporcao.sh:95`; `bin/selo_proporcao.py:50`, `bin/selo_proporcao.py:53`). |
| `marcador_duplo` | Dois exemplares de cada marcador (`bin/tests/test_selo_proporcao.sh:96`; `bin/selo_proporcao.py:48`). |
| `item_sem_crase` | Forma do item (`bin/tests/test_selo_proporcao.sh:97`; `bin/selo_proporcao.py:53`). |
| `item_vazio` | Item vazio entre separadores (`bin/tests/test_selo_proporcao.sh:98`; `bin/selo_proporcao.py:51`, `bin/selo_proporcao.py:53`). |
| `cortes_apagado` | Falha de `git show`, pela guarda `git_rc` (`bin/tests/test_selo_proporcao.sh:99`; `bin/selo_proporcao.py:47`, `bin/selo_proporcao.py:41`). |

Falha no preparo é registrada como falha do harness e retorna **antes de chamar o selo**; não é aceita como rc 2 esperado (`bin/tests/test_selo_proporcao.sh:46`, `bin/tests/test_selo_proporcao.sh:49`). Entretanto, a chamada descarta a saída e compara somente rc; ela **não verifica dinamicamente o motivo** (`bin/tests/test_selo_proporcao.sh:50`).

**4) QUEM ESCREVE e QUEM LÊ**

| Objeto | Escritor/produtor | Leitor/consumidor |
|---|---|---|
| Lista autorizada | Autoridade normativa: **Ronald**, por corte; quem materializou a edição atual: [nao verificado] (`app/docs/CORTES.md:444`). | `lista_fechada`, por `git show <base>:app/docs/CORTES.md`; `motivo_b` consulta pertinência exata (`bin/selo_proporcao.py:47`, `bin/selo_proporcao.py:60`). |
| Lista dos testes | `lista()` escreve o documento; `base()` fornece os itens sintéticos; os casos o deformam (`bin/tests/test_selo_proporcao.sh:17`, `bin/tests/test_selo_proporcao.sh:22`, `bin/tests/test_selo_proporcao.sh:92`). | O mesmo selo, chamado pelo harness (`bin/tests/test_selo_proporcao.sh:41`, `bin/tests/test_selo_proporcao.sh:50`). |
| rc do selo | Python calcula 0/1/2 e emite 10/11/12; wrapper converte 10→0, 11→1 e demais→2 (`bin/selo_proporcao.py:108`, `bin/selo_proporcao.py:111`, `bin/selo_proporcao.py:121`; `bin/selo_proporcao.sh:8`). | Harness captura e compara; régua lê o rc do **harness**, executado por glob (`bin/tests/test_selo_proporcao.sh:41`, `bin/tests/test_selo_proporcao.sh:42`, `bin/tests/test_selo_proporcao.sh:50`; `bin/regua.sh:138`, `bin/regua.sh:140`). |

**Rc 0 significa julgamento concluído**, inclusive para declaração A com arquivos fora; não significa necessariamente classe B (`bin/selo_proporcao.py:105`, `bin/selo_proporcao.py:108`, `bin/selo_proporcao.py:111`). Chamador e leitor do rc sobre a obra real: [nao verificado].

**5) B3, bloqueantes e fila**

O delta mantém classificação por caminho e congela a lista antes do intervalo julgado; isso é compatível com a lista fechada e a exigência de corte do Ronald. A anterioridade do commit, isoladamente, não prova esse corte (`bin/selo_proporcao.py:47`, `bin/selo_proporcao.py:91`; `aval_proporcao_b3_resposta_1111.txt:1`). **Bloqueantes do delta: nenhum demonstrado por essas entradas** (`bin/tests/test_selo_proporcao.sh:78`, `bin/selo_proporcao.py:91`, `bin/selo_proporcao.py:108`).

Fila separada:

- Delimitar “A obra não se autoriza” à base autorizada e ao intervalo completo; a frase atual é mais ampla que a garantia implementada (`bin/selo_proporcao.py:20`, `bin/selo_proporcao.py:78`, `bin/selo_proporcao.py:91`; `app/docs/CORTES.md:443`).
- Corrigir o comentário remanescente “revisao head” (`bin/tests/test_selo_proporcao.sh:91`).
- A alegação “cada guarda [...] caso que só ela pega” continua excessiva: `marcador_duplo` duplica ambos os marcadores, sem isolá-los, e `caso_lista` só verifica rc (`bin/tests/test_selo_proporcao.sh:5`, `bin/tests/test_selo_proporcao.sh:96`, `bin/tests/test_selo_proporcao.sh:50`).
- A cobertura da exceção Python inesperada permanece sem demonstração: os stubs substituem o interpretador e não atravessam essa fronteira (`bin/tests/test_selo_proporcao.sh:145`, `bin/tests/test_selo_proporcao.sh:148`; `bin/selo_proporcao.py:118`, `bin/selo_proporcao.py:120`).

## 11/11 15:03 · sessao 23648d129730 · - · - · origem=Ronald (tmux)

**Eu aprovaria a cura estrita do delta, com limite explícito.** A v6 impede que alterações posteriores à base ampliem a lista usada naquele julgamento (`bin/selo_proporcao.py:47`, `bin/selo_proporcao.py:91`).

O contraexemplo P→H recebe B/rc 0 se P já autoriza o escritor; isso demonstra que o selo **não comprova corte do Ronald nem que o intervalo representa a obra inteira** (`bin/selo_proporcao.py:60`, `bin/selo_proporcao.py:89`, `bin/selo_proporcao.py:111`). O chamador fornece a base; chamador real e validação externa desse início: [nao verificado] (`bin/selo_proporcao.sh:7`, `bin/selo_proporcao.py:78`).

B3 exige corte do Ronald, mas não exige `main` nem autenticação automática desse corte (`app/docs/CORTES.md:444`). Portanto, classificar essa ausência isoladamente como bloqueante acrescentaria uma obrigação não expressa. A frase “A obra nao se autoriza” merece delimitação: vale para o intervalo fornecido, condicionado a uma base autorizada; sua validade incondicional não está demonstrada (`bin/selo_proporcao.py:20`, `app/docs/CORTES.md:443`).

## 11/11 15:02 · sessao 23648d129730 · - · - · origem=Ronald (tmux)

- **Autoridade normativa da lista:** módulo novo só por corte do Ronald; lista atual contém `app/chamados/services/verbos.py` e `app/chamados/templatetags/chamados_tags.py` (`app/docs/CORTES.md:444`). Quem materializou essa edição: [nao verificado].
- **Escritor automatizado localizado:** helper `lista()` do harness escreve `app/docs/CORTES.md` sintético; `base()` fornece os dois módulos sintéticos (`bin/tests/test_selo_proporcao.sh:15`, `bin/tests/test_selo_proporcao.sh:17`, `bin/tests/test_selo_proporcao.sh:22`). Os casos alteram esse documento sintético, inclusive autoautorização e listas tortas (`bin/tests/test_selo_proporcao.sh:77`, `bin/tests/test_selo_proporcao.sh:78`, `bin/tests/test_selo_proporcao.sh:92`).
- **Leitor:** `lista_fechada(repo, base)` faz `git show <base>:app/docs/CORTES.md`; valida apenas cardinalidade dos marcadores e forma dos itens (`bin/selo_proporcao.py:46`, `bin/selo_proporcao.py:47`, `bin/selo_proporcao.py:48`, `bin/selo_proporcao.py:53`). Não há conferência de autor, corte, identidade de `main` ou origem confiável na sequência de validação (`bin/selo_proporcao.py:78`, `bin/selo_proporcao.py:84`, `bin/selo_proporcao.py:89`, `bin/selo_proporcao.py:91`).
- **Quem escolhe base:** o chamador entrega o primeiro argumento; wrapper apenas repassa `"$@"`, e Python o recebe como `base` (`bin/selo_proporcao.sh:7`, `bin/selo_proporcao.py:78`). No harness, `BASE` é o commit inicial sintético; `caso_lista` escolhe como `b` o commit sintético com a lista torta (`bin/tests/test_selo_proporcao.sh:29`, `bin/tests/test_selo_proporcao.sh:47`, `bin/tests/test_selo_proporcao.sh:50`). Pessoa/processo que escolhe a base da obra real: [nao verificado].
- **Garantia efetiva sobre base:** precisa resolver para commit, ser diferente de head e ancestral dele; diff deve ser não vazio; lista precisa existir e ser formalmente válida (`bin/selo_proporcao.py:85`, `bin/selo_proporcao.py:87`, `bin/selo_proporcao.py:89`, `bin/selo_proporcao.py:94`, `bin/selo_proporcao.py:47`). Base anterior da própria raia com lista ampliada satisfaz esses controles; não prova corte do Ronald nem que o intervalo corresponde à obra inteira (`bin/selo_proporcao.py:84`, `bin/selo_proporcao.py:89`, `bin/selo_proporcao.py:91`, `app/docs/CORTES.md:443`, `app/docs/CORTES.md:444`).
- **Quem escreve rc:** Python decide 0/1/2 e emite protocolo privado 10/11/12; wrapper traduz 10→0, 11→1, demais→2 (`bin/selo_proporcao.py:98`, `bin/selo_proporcao.py:108`, `bin/selo_proporcao.py:111`, `bin/selo_proporcao.py:120`, `bin/selo_proporcao.py:121`, `bin/selo_proporcao.sh:8`).
- **Quem lê rc:** `caso()` captura e compara, `caso_lista()` exige 2 e `rcde()` devolve o rc capturado (`bin/tests/test_selo_proporcao.sh:41`, `bin/tests/test_selo_proporcao.sh:42`, `bin/tests/test_selo_proporcao.sh:50`, `bin/tests/test_selo_proporcao.sh:108`). A régua executa o harness por glob e recusa se ele falhar (`bin/regua.sh:138`, `bin/regua.sh:140`, `bin/regua.sh:143`). Isso verifica repos sintéticos criados pelo harness (`bin/tests/test_selo_proporcao.sh:11`, `bin/tests/test_selo_proporcao.sh:13`). Leitor do rc do selo sobre base/head reais: [nao verificado].
- **Prosa obsoleta:** comentário ainda diz “revisao head” imediatamente antes dos oito casos movidos para base (`bin/tests/test_selo_proporcao.sh:91`).

## 11/11 15:01 · sessao 08292a1b6f3e · - · - · origem=Ronald (tmux)

Testes e migrações: **151 locais leitores únicos, 154 nós AST**, com lista e funções já enviadas. A migração filtra `conversa=False`; os testes verificam campos, snapshots e filtros de fixtures. (`app/chamados/migrations/0053_pergunta_unica_por_familia.py:32`, `app/chamados/tests/test_e1_v15.py:79`)

Escritas/entradas de fixtures: **420 locais**, mais **3 dicts de fixtures JSON externos**, total **423**. Há sobreposição leitor/escritor em `app/chamados/tests/test_e1_v5_portas_backfill.py:193`.

`Pergunta` em `test_e4_expand` é um mock derivado de `SimpleNamespace`, incluído como provável interface de PerguntaDisputa. (`app/chamados/tests/test_e4_expand.py:12`, `app/chamados/tests/test_e4_expand.py:62`, `app/chamados/tests/test_e4_expand.py:160`)

Leitor externo relevante: `fila_de_validacao` transforma `i.get('data_evento')` em `data_do_caso`. (`mensageria/nucleo/ferramentas.py:127`)

Outro modelo Django com campo `data_evento`: não encontrado pela varredura AST; a declaração identificada é PerguntaDisputa. (`app/chamados/models.py:2423`)

## 11/11 14:59 · sessao 59a166002cbf · PROPORCAO-POR-RISCO · verificador · origem=Code

**Há um bloqueante: a lista pode autorizar a alteração de si própria e de um escritor no mesmo diff, produzindo B/rc 0.** A pertinência à lista precede qualquer outra classificação (`bin/selo_proporcao.py:46`, `:52`, `:59`, `:107`).

Os rc abaixo são deduzidos por leitura. Resultados de execução, testes e mutantes: [nao verificado]. Referências aos anexos usam as linhas fornecidas no enunciado. [nao verificado]

1. **Bloqueantes dos dois vereditos v4**

| Bloqueante | Situação na v5 | Cura e teste que a cobre | [nao verificado]
|---|---|---|
| Opus B1 — property/outro receptor e método consumido antes de escrita | Deixa de exigir análise de consumo: arquivo fora dos conjuntos implica A; arquivo já autorizado passa pelo caminho. Isso corresponde à substituição por CAMINHO da B3 (`aval_proporcao_b3_resposta_1111.txt:1`). | Classificação em `bin/selo_proporcao.py:57`, `:107`; caso `py_fora`, `bin/tests/test_selo_proporcao.sh:58`. Não há reprodução específica da property nesse teste. |
| Opus B2 — dois saltos | Mesmo tratamento: quantidade de saltos deixou de ser critério (`aval_proporcao_b3_resposta_1111.txt:1`; `bin/selo_proporcao.py:57`). | `py_fora` cobre a recusa genérica por caminho, não os dois saltos: `bin/tests/test_selo_proporcao.sh:58`. |
| Opus B3 — atribuir fuga entre módulos ao “custo aceito” | A atribuição antiga foi corrigida: cabeçalho e contrato dizem expressamente que essa fuga **não** integra o custo aceito (`bin/selo_proporcao.py:20`; `v5_contrato.txt:1`). | `py_fora` e `py_fora_com_b` cobrem arquivos externos aos conjuntos (`bin/tests/test_selo_proporcao.sh:58`, `:59`). Fidelidade da prosa não é verificada pelo harness, que compara rc (`:41`, `:42`). |
| Fable BL1 — repo implícito, igualdade e diff vazio verdes | Fechado no código: repo obrigatório, raiz conferida, igualdade e diff vazio recusados (`bin/selo_proporcao.py:74`, `:80`, `:86`, `:93`). | `sem_repo`, `sem_repo_no_repo_do_script`, `repo_subdir`, `repo_nao_git`, `base_igual_head`, `diff_vazio` (`bin/tests/test_selo_proporcao.sh:104`, `:111`, `:114`, `:116`, `:119`, `:121`). O controle no repo do script é condicional à existência das revisões (`:116`). |
| Fable BL2 — revisões invertidas e exclusões ignoradas | Fechado pelo controle de ancestralidade e pela classificação de todos os nomes, incluindo exclusões; não se lê mais apenas o conteúdo novo (`bin/selo_proporcao.py:88`, `:91`, `:99`). | `revisoes_trocadas`, `nao_ancestral`, `apagou_fora` e renames nos dois sentidos (`bin/tests/test_selo_proporcao.sh:65`, `:66`, `:67`, `:103`, `:108`). Não há caso específico de migration apagada na v5. |

2. **Escapes: separar os aceitos por CAMINHO do defeito da lista**

Nas condições válidas de argumentos, revisões e lista, cada diff abaixo recebe **rc 0 declarado B**: o conteúdo não é inspecionado (`bin/selo_proporcao.py:90`, `:91`, `:100`, `:107`).

| Arquivo e diff mínimo descrito | Escrita alcançada | Texto literal da B3 |
|---|---|---|
| `app/chamados/services/verbos.py`: inserir `vivo_qs.update(status_local='fechado')` antes da linha 34. | A função recebe o queryset e é chamada pelo painel (`app/chamados/services/verbos.py:30`; `app/chamados/services/painel.py:230`); o campo persistido existe (`app/chamados/models.py:52`). | **ACEITA pelo caminho já listado**, sem análise de AST (`app/docs/CORTES.md:444`; `bin/selo_proporcao.py:59`). |
| `app/chamados/templatetags/chamados_tags.py`: antes do retorno de `area_label`, inserir `chamado.status_local='fechado'; chamado.save(update_fields=['status_local'])`. | O filtro recebe o chamado e tem consumidor no template (`app/chamados/templatetags/chamados_tags.py:34`; `app/templates/chamados/detalhe_local.html:56`). | **ACEITA pelo caminho já listado** (`app/docs/CORTES.md:444`; `bin/selo_proporcao.py:59`). |
| `app/static/pwa.js`: trocar `tipo, timestamp: tsCorrigido` por `tipo: 'saida', timestamp: tsCorrigido`. | Altera a batida gravada em IndexedDB e posteriormente enviada por POST (`app/static/pwa.js:112`, `:114`, `:144`, `:153`). | **ACEITA por `static/`** (`bin/selo_proporcao.py:63`; `aval_proporcao_b3_resposta_1111.txt:1`). |
| `app/templates/chamados/partials/_js_cobranca.html`: substituir a chamada de confirmação da linha 60 por `_go();`. | Antecipa o POST de cobrança à confirmação; a view pode registrar a ocorrência após envio bem-sucedido (`app/templates/chamados/partials/_js_cobranca.html:51`, `:60`; `app/chamados/views_cobrar.py:54`). | **ACEITA por `templates/`** (`bin/selo_proporcao.py:61`; `aval_proporcao_b3_resposta_1111.txt:1`). |
| `app/core/tests/test_contract_tblib_no_paralelo.py`: inserir no início de `tblib_instalado` o import de `ChamadoColaborador` e `ChamadoColaborador.objects.update(status_local='fechado')`. | Há consumidor em management command, apesar da localização do helper em testes (`app/core/tests/test_contract_tblib_no_paralelo.py:24`; `app/chamados/management/commands/tblib_ausente_na_imagem.py:17`). | **ACEITA pela identificação de teste por caminho/nome** (`bin/selo_proporcao.py:65`, `:68`; `aval_proporcao_b3_resposta_1111.txt:1`). |

Esses exemplos não exigem perseguir reflexão, alias ou saltos: a B3 determina “O selo checa CAMINHO, nao AST” (`aval_proporcao_b3_resposta_1111.txt:1`). Efeito efetivamente observado em produção: [nao verificado].

**O escape da autoautorização é diferente e é defeito do selo.** Diff mínimo descrito:

- Na lista de `app/docs/CORTES.md:444`, acrescentar, antes do marcador final: ``; `app/chamados/writer.py`; `app/docs/CORTES.md` ``.
- Acrescentar `app/chamados/writer.py` com:

```python
from chamados.models import ChamadoColaborador
ChamadoColaborador.objects.update(status_local='fechado')
```

O parser aceita ambos os itens; tanto o novo escritor quanto o documento alterado pertencem à lista lida de head. Portanto `fora=[]` e o rc previsto é **0**, mesmo declarado B (`bin/selo_proporcao.py:46`, `:52`, `:54`, `:59`, `:104`, `:110`).

Isso contraria “entra modulo novo so por corte meu” e a própria promessa “A obra nao se autoriza” (`aval_proporcao_b3_resposta_1111.txt:1`; `bin/selo_proporcao.py:19`). O caso `lista_se_autoriza` acrescenta somente o módulo; deixa `CORTES.md` fora e não morde esse escape (`bin/tests/test_selo_proporcao.sh:69`).

3. **Quem escreve e quem lê a lista**

A autorização para entrada de módulo novo pertence a Ronald; a lista presente na cópia contém `app/chamados/services/verbos.py` e `app/chamados/templatetags/chamados_tags.py` (`app/docs/CORTES.md:444`). Quem materializou essa edição e sua inclusão efetiva no hash informado: [nao verificado].

O leitor é `lista_fechada`: usa `git show <head>:app/docs/CORTES.md`, exige os dois marcadores únicos e verifica apenas a forma craseada dos itens. Não verifica autor, corte, extensão `.py` nem identidade da linha B3 (`bin/selo_proporcao.py:45`, `:47`, `:52`, `:54`).

Acrescentar **somente** o módulo deixa `CORTES.md` fora dos conjuntos e resulta em rc 1; acrescentar também `CORTES.md` permite a autoautorização descrita acima (`bin/tests/test_selo_proporcao.sh:69`; `bin/selo_proporcao.py:59`, `:107`).

4. **Retornos**

| Condição | Resultado deduzido |
|---|---|
| Declaração B com algum arquivo fora | **1**; é a única condição de retorno lógico 1 (`bin/selo_proporcao.py:107`). |
| Declaração A, com julgamento válido | **0**, inclusive com arquivos fora (`bin/selo_proporcao.py:107`, `:110`). |
| Argumentos inválidos; repo sem Git ou que não seja raiz; revisão inválida | **2** (`bin/selo_proporcao.py:74`, `:79`, `:80`, `:84`, `:97`). |
| `base==head`; base não ancestral; commits distintos com diff vazio | **2** (`bin/selo_proporcao.py:86`, `:88`, `:93`). |
| Lista ausente, vazia ou malformada | **2** (`bin/selo_proporcao.py:46`, `:47`, `:52`, `:97`). |
| Exceção inesperada na implementação Python | **2**, pela fronteira externa (`bin/selo_proporcao.py:117`, `:119`). |
| `GIT_*` herdado | Removido do ambiente de cada subprocesso Git; o harness também o limpa antes de criar seus repos (`bin/selo_proporcao.py:38`; `bin/tests/test_selo_proporcao.sh:6`, `:131`). |

“Repo errado” precisa de distinção: um subdiretório ou diretório sem Git dá 2; **outra raiz Git válida**, contendo as revisões fornecidas, será julgada normalmente. Não há comparação com uma identidade esperada do projeto (`bin/selo_proporcao.py:77`, `:79`, `:84`).

O wrapper preserva os resultados públicos **0/1/2**, traduzindo **10→0, 11→1 e qualquer outro código→2**. Não repassa arbitrariamente o código bruto do Python (`bin/selo_proporcao.sh:7`, `:8`, `:9`, `:10`, `:11`). O harness exige rc 1 também sob `bash -e` (`bin/tests/test_selo_proporcao.sh:134`).

5. **Testes com mecanismos sobrepostos e mutantes** [nao verificado]

- `marcador_duplo` duplica **os dois marcadores**. Remover só a checagem de cardinalidade de início em `bin/selo_proporcao.py:47` deixa a checagem de fim rejeitando o caso; o mutante simétrico também escapa dessa prova. Ausência do marcador ainda acaba em exceção normalizada para 2 (`bin/tests/test_selo_proporcao.sh:83`, `:85`, `:87`; `bin/selo_proporcao.py:49`, `:117`). Sobrevivência por execução: [nao verificado].
- `misto_b` usa `tests/test_b.py`, elegível por diretório **e** basename. Há controles separados: `tests_dir` usa `fixt.py`, enquanto `test_basename` fica fora de `tests/` (`bin/tests/test_selo_proporcao.sh:50`, `:51`, `:54`).
- `migration` prova apenas a classificação genérica de `.py` fora dos conjuntos; não existe mecanismo específico de migration na v5. `apagou_fora` cobre exclusão pelo mesmo mecanismo (`bin/tests/test_selo_proporcao.sh:63`, `:65`; `bin/selo_proporcao.py:57`, `:91`).
- O preparo de `git_dir_herdado` cria no outro repo um único commit e lista inválida. Prova resistência ao desvio que causaria erro; não compara dois diffs válidos, um A e outro B, como sugere o comentário (`bin/tests/test_selo_proporcao.sh:128`, `:129`, `:130`, `:131`).

**O problema de cobertura do antigo F2 permanece:** o mutante mínimo `bin/selo_proporcao.py:119`, `rc = 2` → `rc = 0`, tem sobrevivência dedutível. Os casos normais de erro exercitam argumentos e `NaoJulgado`; os stubs substituem o interpretador e não atravessam essa fronteira Python (`bin/selo_proporcao.py:76`, `:95`, `:117`; `bin/tests/test_selo_proporcao.sh:83`, `:122`, `:136`). Execução do mutante: [nao verificado].

A exigência de **um** marcador de cada tipo e de normalizar **qualquer** exceção, portanto, não fica individualmente demonstrada pela afirmação “cada guarda [...] tem caso que so ela pega” (`bin/tests/test_selo_proporcao.sh:5`; `bin/selo_proporcao.py:47`, `:117`).

6. **QUEM CHAMA, QUEM LÊ e atribuições ao aval**

O chamador concreto do selo localizado é o harness: define `SELO`, chama, captura e compara o rc (`bin/tests/test_selo_proporcao.sh:9`, `:41`, `:42`). O harness tem chamador por glob: `bin/regua.sh` executa `tests/test_*.sh` e recusa a régua se algum falhar (`bin/regua.sh:138`, `:140`, `:143`).

Essa cadeia julga repositórios **sintéticos**, criados pelo teste (`bin/tests/test_selo_proporcao.sh:11`, `:13`). Chamador e leitor de rc sobre base/head da obra real: [nao verificado]. O contrato atribui essa responsabilidade ao pouso B, mas isso é declaração de conduta (`v5_contrato.txt:2`, `:3`).

Quanto aos demais registros v4:

- A exclusão antes ignorada agora entra na classificação por nomes; o teste específico de migration apagada foi substituído pela cobertura genérica de exclusão (`bin/selo_proporcao.py:91`; `bin/tests/test_selo_proporcao.sh:65`).
- O wrapper agora usa `-B`; remoção e situação de versionamento do antigo `bin/__pycache__`: [nao verificado] (`bin/selo_proporcao.sh:7`).
- A frase e a lista constam da cópia (`app/docs/CORTES.md:443`, `:444`). Contrato PROPORCAO no RELATO e commit dessas alterações: [nao verificado].

O cabeçalho chama de “literal” a paráfrase **“qualquer outro arquivo”**; o aval diz **“Qualquer outro .py”**. A consequência para outros arquivos continua derivável da condição anterior sobre **TODO arquivo**, mas a paráfrase não é citação literal (`bin/selo_proporcao.py:10`, `:11`; `aval_proporcao_b3_resposta_1111.txt:1`).

Atribuir os escritores escondidos ao smoke/fila também vai além do que a B3 afirma expressamente. O aval original exige smoke de clique para B; não promete que esse smoke detectará esses escritores (`bin/selo_proporcao.py:21`; `v5_contrato.txt:4`; `aval_proporcao_por_risco_1111.txt:1`). Essa capacidade do smoke: [nao verificado].

7. **`.md` fora dos conjuntos é A pelo texto literal**

A frase decisiva é: **“CLASSE B so se TODO arquivo do diff estiver em templates/, static/, arquivo de teste, ou numa LISTA FECHADA de modulos de leitura declarada no CORTES.md”** (`aval_proporcao_b3_resposta_1111.txt:1`). [nao verificado]

Logo, `.md` fora desses conjuntos é **A**; a frase seguinte sobre `.py` não concede exceção aos documentos. A implementação e `doc_md` seguem essa leitura (`bin/selo_proporcao.py:70`, `:107`; `bin/tests/test_selo_proporcao.sh:60`).

L-106 exige docs no mesmo commit do marco; quando esses documentos ficam fora dos conjuntos, o diff completo recebe A (`CLAUDE.md:332`, `:333`; `bin/selo_proporcao.py:99`, `:104`). A afirmação contratual “qualquer .md [...] faz a obra A” é ampla demais: um `.md` sob `templates/` ou `static/` passa pelo caminho, e o defeito demonstrado permite listar o próprio `CORTES.md` (`v5_contrato.txt:4`; `bin/selo_proporcao.py:59`, `:61`, `:63`).

REPROVADO — bloqueante: autoautorização da obra ao incluir o próprio `CORTES.md` e um escritor na lista de head, obtendo B/rc 0 (`bin/selo_proporcao.py:46`, `:52`, `:59`, `:110`).

## 11/11 14:57 · sessao 6e228e64f5a4 · - · - · origem=Ronald (tmux)

Recorte: Python fora de `app/chamados/**`, sem testes/migrações. **16 linhas leitoras, 21 nós de leitura; 0 escritores; 0 colisões com outro modelo encontradas** nas referências abaixo. “Sim” compara o valor civil não nulo à resposta `None` do juiz para conversa; ocorrência concreta em produção permanece [nao verificado]. Juiz: `app/chamados/juizes.py:334`, `app/chamados/juizes.py:338`.

| Arquivo:linha | Função | Uso | Conversa diverge? |
|---|---|---|---|
| `app/api/views.py:1165` | `api_chamados_v2` | Serializa ISO em `perguntas_disputa[].data_evento`; dois acessos no ternário. | Sim, só repassa |
| `app/api/views.py:1166` | `api_chamados_v2` | Formata `data_evento_label`; dois acessos no ternário. | Sim |
| `app/api/views.py:2637` | `api_disputa_responder` | Insere valor cru em mensagem de auditoria `dia=%s`. | Sim, só repassa |
| `app/api/views_mensageria.py:568` | `_item_validar` | Serializa ISO na fila de validação; dois acessos no ternário. | Sim, só repassa |
| `app/colaboradores/services/calendario.py:473` | `contexto_calendario` | Filtra intervalo de perguntas para indexação de pergunta viva por dia. | Sim |
| `app/colaboradores/services/calendario.py:477` | `contexto_calendario` | Indexa `_perg_viva` pelo dia cru; usado na disponibilidade de “Resolver dia”. | Sim |
| `app/folha/previa.py:65` | `montar_previa` | Filtra perguntas pela competência. | Sim |
| `app/folha/previa.py:67` | `montar_previa` | Agrupa material da prévia por dia cru. | Sim |
| `app/ponto/management/commands/mapa_divergencia.py:272` | `Command.handle` | Dois filtros `gte/lte` e projeção `only('data_evento')`: três nós. | Sim |
| `app/ponto/management/commands/mapa_divergencia.py:273` | `Command.handle` | Constrói conjunto de dias para contador de furos cobertos por pergunta. | Sim |
| `app/ponto/management/commands/vigia_plantio_overnight.py:75` | `batidas_fora_do_turno_da_pergunta` | Lookup `pergunta_origem__data_evento__isnull=False`. | Sim se conversa alcançar o universo; ocorrência [nao verificado] |
| `app/ponto/management/commands/vigia_plantio_overnight.py:86` | mesma | Usa data crua para ampliar janela de busca dos turnos. | Mesma condição |
| `app/ponto/management/commands/vigia_plantio_overnight.py:90` | mesma | Compara com dia das batidas e apresenta diferença. | Mesma condição |
| `app/ponto/services/dia_do_colab.py:65` | `dia_do_colab` | Filtra perguntas para `perguntas_deste_dia`. | Sim |
| `app/ponto/services/veredito_celula.py:180` | `fechar_o_fio_do_dia` | Filtra perguntas do dia para fechamento; contém `conversa=False`. | Não se aplica: conversa excluída |
| `mensageria/nucleo/ferramentas.py:127` | `fila_de_validacao` | Lê chave serializada com `i.get('data_evento')`, renomeando para `data_do_caso`. | Sim, só repassa |

Proveniência e limites:

- A API v2 percorre `consulta['pendentes'] + consulta['historico']`, sem discriminar conversa nessa montagem: `app/api/views.py:1040`. `consulta_do_app` separa apenas por resposta/disputa: `app/chamados/services/pergunta_no_app.py:69`.
- A fila da mensageria vem de `perguntas_respondidas_a_validar`, sem exclusão de conversa: `app/api/views_mensageria.py:540`, `app/chamados/juizes.py:1664`. O cliente lê `/api/mensageria/validar/` e retorna JSON: `mensageria/nucleo/core_client.py:49`, `mensageria/nucleo/core_client.py:55`. Consumo: `mensageria/nucleo/ferramentas.py:107`, `mensageria/nucleo/ferramentas.py:129`.
- No vigia, o filtro exige `tipo_resposta='hora'`, mas não `conversa=False`: `app/ponto/management/commands/vigia_plantio_overnight.py:74`. A relação `Batida.pergunta_origem` aponta para `PerguntaDisputa`, portanto não é colisão de modelo: `app/ponto/models.py:40`, `app/ponto/models.py:41`.
- `app/escala/management/commands/hub_smoke.py:35` e `app/escala/management/commands/hub_smoke.py:51` são nomes de parâmetros usados por introspecção, sem acesso ao campo de modelo.
- O censo anterior documenta explicitamente o problema da prévia e de `dia_do_colab`: `app/chamados/services/fio.py:1398`, `app/chamados/services/fio.py:1399`.

## 11/11 14:57 · sessao 7361ac4aef50 · - · - · origem=Ronald (tmux)

(b) Templates: **5 ocorrências sintáticas em 5 linhas/3 arquivos; 4 leitores com origem demonstrada em PerguntaDisputa e 1 referência sem vínculo de contexto demonstrado**. A relação completa é:

| Arquivo:linha | Contexto e origem | Uso | Diverge para conversa? |
|---|---|---|---|
| `app/templates/chamados/partials/_card_atendimento.html:87` | `p` do laço `chamado.perguntas_disputa` em `:77`; `meu_atendimento` chama `montar_chamados_fio` (`app/chamados/views.py:569`), que consulta PerguntaDisputa (`app/chamados/services/fio.py:225`), agrupa (`:243`) e anexa a `ch.perguntas_disputa` (`:253`); os includes vêm de `app/templates/chamados/meu_atendimento.html:127` e `:144` | Testa existência para exibir cabeçalho do dia | **Sim**, havendo conversa com data: o cru habilita o cabeçalho; juiz retorna None (`app/chamados/juizes.py:338`) |
| `app/templates/chamados/partials/_card_atendimento.html:90` | Mesmo contexto acima | Exibe “Estamos falando do dia” (`:89`) formatando o cru | **Sim**, civil exibido no lugar do dia do fato; juiz None (`app/chamados/juizes.py:338`) |
| `app/templates/chamados/partials/_card_atendimento.html:164` | Mesmo contexto; ramo `p.tipo_resposta == 'hora' and p.opcoes` (`:158`) | Exibe dia na confirmação JS inline | **Sim se conversa alcançar esse ramo**; nenhuma exclusão `conversa` nesse ramo (`:158`); ocorrência real em produção [nao verificado] |
| `app/templates/chamados/partials/_lote_preview.html:24` | `x` de `linhas`; `lote_preview` passa classe A (`app/chamados/views_cobrar.py:363`); `fila_validar_classificada` insere `{'pergunta': p}` (`app/chamados/services/validacao.py:923`); `p` vem de `perguntas_respondidas_a_validar` (`:921`), cujo queryset é PerguntaDisputa (`app/chamados/juizes.py:1664`) | Exibe dia na coluna “Dia” (`_lote_preview.html:23`) | **Sim se conversa alcançar classe A**; classe usa resposta/marco e não exclui `conversa` explicitamente (`app/chamados/services/validacao.py:845`, `:877`); ocorrência real [nao verificado] |
| `app/templates/chamados/meu_atendimento.html:19` | Bloco `for reg in regularizacoes_pendentes` (`:10`); não há laço de `p` ali; view fornece `regularizacoes_pendentes`, mas não `p`/`opt` (`app/chamados/views.py:633`, `:647`, `:651`) | Tentativa de exibir dia em confirmação | **Não se aplica** como leitor demonstrado de PerguntaDisputa; associação de `p` [nao verificado] |

A menção `data_evento` em `app/templates/chamados/partials/modal_fio.html:161` é comentário, sem acesso de variável nessa linha. O local citado no anexo não corresponde a leitor cru na cópia: `app/templates/colaboradores/partials/painel_colaborador.html:198` contém include de `msgs_lista.html`.

(c) Serialização/API: **2 endpoints, 2 chaves exatas `data_evento` + 1 chave derivada `data_evento_label` = 3 posições de emissão**, em 3 linhas:

| Arquivo:linha | Função / caminho | Uso | Diverge para conversa? |
|---|---|---|---|
| `app/api/views.py:1165` | `api_chamados_v2` (`:973`), rota `app/api/urls.py:73` | Repassa cru em ISO ou None, dentro de `chamados[].perguntas_disputa[]` (`views.py:1155`) | **Sim no valor transmitido**: conversa com cru preenchido transmite ISO; juiz None (`juizes.py:338`). Exibição pelo app nativo [nao verificado] |
| `app/api/views.py:1166` | Mesmo endpoint | Repassa cru formatado `dd/mm/aaaa` em `data_evento_label` | **Sim no valor transmitido**; consumo do app nativo [nao verificado] |
| `app/api/views_mensageria.py:568` | `_item_validar` (`:554`) chamado por `api_mensageria_validar` (`:546`), retornado como JSON (`:549`); rota `app/api/urls.py:12` | Repassa cru ISO ou string vazia | **Sim no valor transmitido**; juiz None (`juizes.py:338`) |
| `mensageria/nucleo/ferramentas.py:127` | `fila_de_validacao` (`:98`); chama cliente (`:107`); cliente lê `/api/mensageria/validar/` (`mensageria/nucleo/core_client.py:50`) e retorna JSON (`:55`) | Consome `i.get('data_evento')`, renomeia para `data_do_caso` em casos enviados ao copiloto (`ferramentas.py:116`, `:137`) | **Sim no valor repassado**, se item de conversa; interpretação do copiloto [nao verificado] |

Não encontrei acesso a `data_evento` em arquivos JS/TS/TSX/Dart da cópia por varredura textual [nao verificado quanto a código externo/cliente nativo]. O JS inline de templates é o de `_card_atendimento.html:164` e `meu_atendimento.html:19`, já contado em (b).

Para a allowlist (a), incluir também o leitor de chave serializada **`mensageria/nucleo/ferramentas.py:127`**. As chaves em `mensageria/nucleo/tests/test_ferramenta_validar.py:18`, `:24` e `mensageria/nucleo/tests/test_proposta_lote.py:24` são construção de fixtures de resposta, não leitura do modelo.

## 11/11 14:57 · sessao 029825b73c6b · - · - · origem=Ronald (tmux)

Achado bloqueante: a obra consegue se autodeclarar B adicionando **também o próprio CORTES.md à lista**. A leitura vem de head (`bin/selo_proporcao.py:46`), o parser admite qualquer caminho craseado (`bin/selo_proporcao.py:49`), e `motivo_b` aceita a lista antes de qualquer classificação (`bin/selo_proporcao.py:59`). O teste `lista_se_autoriza` acrescenta somente `views.py`, deixando CORTES fora; portanto não morde este escape (`bin/tests/test_selo_proporcao.sh:69`).

Diff mínimo textual, **rc previsto 0, sem executar** (`bin/selo_proporcao.py:107`):

```diff
--- a/app/docs/CORTES.md
+++ b/app/docs/CORTES.md
@@ linha 444, antes de FIM-LISTA-FECHADA-PROPORCAO-B:
+; `app/chamados/writer.py`; `app/docs/CORTES.md`
--- /dev/null
+++ b/app/chamados/writer.py
@@
+from chamados.models import ChamadoColaborador; ChamadoColaborador.objects.update(status_local='fechado')
```

Aqui a obra muda a autorização em seu próprio diff; isso contraria “modulo novo na lista so por corte do Ronald” (`app/docs/CORTES.md:444`). A afirmação “A obra nao se autoriza” é falsa para esse caso (`bin/selo_proporcao.py:19`). O campo persistido existe (`app/chamados/models.py:52`). Execução efetiva do writer: [nao verificado].

Escapes **aceitos pelo critério de caminho**, com rc previsto 0 e sem execução:

| Diff mínimo | Escrita e caminho | Aval |
|---|---|---|
| Em `particionar_verbos`, acrescentar `vivo_qs.update(status_local='fechado')` antes da linha atual 34 | Queryset recebido pela função (`app/chamados/services/verbos.py:30`); consumidor real no painel (`app/chamados/services/painel.py:230`); módulo listado (`app/docs/CORTES.md:444`), aceito por `bin/selo_proporcao.py:59`. | ACEITA pelo caminho; não é defeito de AST. |
| Em `area_label`, acrescentar `chamado.status_local='fechado'; chamado.save(update_fields=['status_local'])` antes do return | Filtro recebe chamado (`app/chamados/templatetags/chamados_tags.py:34`), consumido em template (`app/templates/chamados/detalhe_local.html:56`); módulo listado (`app/docs/CORTES.md:444`). | ACEITA pelo caminho. |
| Trocar `tipo, timestamp: tsCorrigido` por `tipo: 'saida', timestamp: tsCorrigido` | Grava batida em IndexedDB (`app/static/pwa.js:112`); conteúdo enviado por POST depois (`app/static/pwa.js:144`, `app/static/pwa.js:153`); static aceito (`bin/selo_proporcao.py:63`). | ACEITA pelo caminho. |
| Acrescentar writer na função `tblib_instalado`, por exemplo import do modelo e `.objects.update(...)` | Helper vive sob tests (`app/core/tests/test_contract_tblib_no_paralelo.py:24`) e é chamado por management command de produção (`app/chamados/management/commands/tblib_ausente_na_imagem.py:17`); tests aceito (`bin/selo_proporcao.py:65`). | ACEITA pelo caminho. |

O texto B3 diz “CLASSE B so se TODO arquivo do diff estiver em templates/, static/, arquivo de teste, ou numa LISTA FECHADA” e “O selo checa CAMINHO, nao AST”; não ordena inspecionar esses conteúdos (`app/docs/CORTES.md:444`; literal completo no anexo fornecido [nao verificado no filesystem]).

QUEM CHAMA / QUEM LÊ:

- O harness define `SELO` em `bin/tests/test_selo_proporcao.sh:9`, chama e captura rc em `bin/tests/test_selo_proporcao.sh:41`, compara em `bin/tests/test_selo_proporcao.sh:42`; outras chamadas estão em `:95`, `:99`, `:105`, `:124`, `:134`, `:139`, `:141`.
- O harness tem chamador real: `bin/regua.sh:138` percorre `tests/test_*.sh`, executa em `bin/regua.sh:140` e recusa em `bin/regua.sh:143`.
- Essa cadeia verifica repositórios sintéticos (`bin/tests/test_selo_proporcao.sh:11`, `bin/tests/test_selo_proporcao.sh:13`). Chamador automatizado do selo sobre base/head **da obra real** não foi localizado na cópia; uso humano/prod [nao verificado]. O pre-push explica que o loop de host não roda ali (`bin/pre-push.sh:15`).
- A documentação exige docs no mesmo commit (`CLAUDE.md:333`); isso torna a obra A se o `.md` estiver fora dos conjuntos, conforme `bin/selo_proporcao.py:70` e `bin/selo_proporcao.py:107`. A primeira frase literal do B3 (“TODO arquivo”) sustenta isso; “qualquer outro .py” não constitui permissão para outros sufixos (`app/docs/CORTES.md:444`).
- O cabeçalho chama de “literal” a paráfrase “qualquer outro arquivo”, enquanto B3 escreve “qualquer outro .py” (`bin/selo_proporcao.py:10`, `app/docs/CORTES.md:444`); a consequência para `.md` continua derivável do “TODO”.
- O cabeçalho atribui ao smoke/fila os writers em caminhos permitidos (`bin/selo_proporcao.py:21`); essa atribuição específica não consta do B3 fornecido [nao verificado no filesystem]. B3 autoriza o critério por caminho; não declara expressamente que smoke detectará esses writers.
- Não há contrato PROPORCAO no RELATO da cópia; a única ocorrência de “proporcao” encontrada ali é assunto antigo (`app/docs/RELATO.md:3378`). Existência do contrato v5 no RELATO/prod: [nao verificado].

## 11/11 14:56 · sessao 2b46e4366f87 · - · - · origem=Ronald (tmux)

Achados por leitura; execução dos rc e dos mutantes [nao verificado].

- **Autoautorização passa:** acrescentar à lista head tanto `app/docs/CORTES.md` quanto `app/x/views.py`, e mudar `views.py` para gravar. O parser aceita qualquer item entre crases; os dois arquivos ficam na lista e `fora=[]`, portanto rc previsto **0**. O caso `lista_se_autoriza` acrescenta somente `views.py`, deixando CORTES fora; não prova a defesa contra a alteração conjunta. Isso contradiz a promessa “A obra nao se autoriza”. (`bin/selo_proporcao.py:19`, `:52`, `:54`, `:59`, `:102`, `:110`; `bin/tests/test_selo_proporcao.sh:69`.)

- **Mutante sobrevivente de rc:** em `bin/selo_proporcao.py:119`, trocar `rc = 2` por `rc = 0`. Os erros dos testes entram nos retornos explícitos de argumentos ou em `NaoJulgado`; nenhum caso provoca exceção inesperada na fronteira Python. Os stubs substituem o interpretador, portanto não passam por esse bloco. Sobrevivência prevista por leitura, execução [nao verificado]. (`bin/selo_proporcao.py:74`, `:95`, `:117`; `bin/tests/test_selo_proporcao.sh:83`, `:122`, `:136`.)

- **Mutante sobrevivente de marcador:** em `bin/selo_proporcao.py:47`, remover somente `texto.count(INICIO) != 1 or`. O caso `marcador_duplo` duplica ambos os marcadores; a checagem restante continua rejeitando. A ausência de início ainda termina em `ValueError`, normalizado para rc 2. Não há caso duplicando somente o início. O mutante simétrico, removendo somente a contagem do fim, também tem sobrevivência prevista. (`bin/selo_proporcao.py:47`, `:49`, `:117`; `bin/tests/test_selo_proporcao.sh:83`, `:85`, `:87`.)

- **Dois gatilhos:** `marcador_duplo` mistura as duas violações de cardinalidade; `misto_b` usa `tests/test_b.py`, elegível simultaneamente por diretório e basename. Neste segundo caso existem controles separados: `tests_dir` usa `fixt.py`, e `test_basename` fica fora de `tests/`. (`bin/tests/test_selo_proporcao.sh:50`, `:51`, `:54`, `:87`.)

- **Nome versus mecanismo:** `migration` prova que um `.py` fora dos conjuntos recebe rc 1; não existe guarda específica de migration na v5. `apagou_fora` cobre exclusão pelo mesmo mecanismo genérico. (`bin/tests/test_selo_proporcao.sh:63`, `:65`; `bin/selo_proporcao.py:57`, `:91`, `:107`.)

- **Repo errado:** subdiretório e diretório sem Git recebem rc previsto 2. Uma raiz Git válida de outro repositório não é identificada como “errada”: se resolver as revisões e satisfizer as demais condições, o selo julga aquele repo e pode devolver 0/1. Não há comparação com identidade esperada do projeto. (`bin/selo_proporcao.py:77`, `:79`, `:84`, `:90`; `bin/tests/test_selo_proporcao.sh:119`, `:121`.)

- **rc:** o único retorno lógico 1 exige `classe == 'B' and fora`; argumentos, igualdade, não ancestralidade, lista inválida e diff vazio dão 2. `GIT_*` é filtrado nas chamadas Git. O wrapper traduz 10→0, 11→1 e todo outro código→2; o teste do chamador com `bash -e` exige rc 1. (`bin/selo_proporcao.py:38`, `:74`, `:86`, `:88`, `:93`, `:97`, `:107`, `:120`; `bin/selo_proporcao.sh:8`; `bin/tests/test_selo_proporcao.sh:134`.)

## 11/11 14:53 · sessao 1c94fdcaac33 · FIO-E0-V4-E4B · construtor · origem=Code

Travas separadas em duas fases: [fecho_runtime.py:119](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/fecho_runtime.py:119).

RED c7 e mutante mordem; Ruff verde e 118 testes em memória passaram. Comandos, resultados e fila no [recibo c8](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c8.txt:23). [nao verificado]

ORM, suíte integral e certificação Code/opus: [nao verificado]. [nao verificado]

## 11/11 14:47 · sessao d4ad4297d1f3 · GABARITO-FABLE-SOMBRA-AJUSTE · verificador · origem=Code

**BLOQUEANTE** — comportamento errado na tela exige volta, inclusive em classe B: “achado de comportamento errado na tela continua sendo volta” (anexo fornecido `aval_proporcao_por_risco_1111.txt:1`).

- **Entrada concreta:** conversa aberta com data local `11/11/2026` grava `conversa=True, data_evento=2026-11-11`; `abrir` distingue esse caso da pergunta datada de turno ([models.py:2783](app/chamados/models.py:2783), [models.py:2801](app/chamados/models.py:2801)). A emissão sem dia encaminha para `abrir_conversa` ([disputa_emissao.py:431](app/chamados/services/disputa_emissao.py:431)).
- **Resultado, estando na lista pendente:** painel mostra **“Dia: 11/11/2026”** pelo campo cru; juiz responde **`None` — sem dia de turno** ([painel_colaborador.html:198](app/templates/colaboradores/partials/painel_colaborador.html:198), [juizes.py:338](app/chamados/juizes.py:338)).
- **Enquadramento:** a função única de **rótulo** exigida pelo aval não autoriza regra independente de **dia** (anexo fornecido `aval_ui_painel_raspado.txt:8`). “Nenhum leitor tem regra propria de dia” e “Fatia que fura qualquer linha abaixo NAO FECHA” ([CLAUDE.md:21](CLAUDE.md:21), [CLAUDE.md:15](CLAUDE.md:15)). É divergência concreta da apresentação, não achado sobre a forma de testar.

Ocorrência efetiva em produção: **[nao verificado]**.

## 11/11 14:45 · sessao 5928edd1fdd0 · FIO-E0-V4-E4B · verificador · origem=Code

**REPROVADO.** Há um **BLOQUEANTE novo de ordem de trava**: a serialização por competência intercala travas de âncora com a trava compartilhada do contador DP. A suíte inteira também permanece pendente para encerramento da obra. (`app/chamados/services/fecho_runtime.py:115`; `app/pautas/services.py:188`; `app/pautas/services.py:354`; `CLAUDE.md:375`)

**B1 — BLOQUEANTE: deadlock entre âncoras e contador DP.** O laço faz `travar(A) → escrever(A) → travar(B)`. Dentro de `escrever(A)`, `lavrar` atualiza o contador DP; essa trava permanece na transação externa. Ordenar as âncoras não ordena a trava intermediária do contador. (`app/chamados/services/fecho_runtime.py:116`; `app/chamados/services/fecho_runtime.py:122`; `app/pautas/services.py:188`; `app/pautas/services.py:354`; `app/chamados/services/fecho_runtime.py:142`)

Intercalação permitida pelo código, com contador DP existente e Pautas ainda ausentes:

| Passo | Transação T1: competências A,B | Transação T2: competência B |
|---|---|---|
| 1 | Trava A, escreve e segura contador DP. | |
| 2 | | Trava B; tenta escrever e espera contador DP. |
| 3 | Pede B e espera T2. | Continua esperando T1. |

O lote com duas competências é expressamente suportado; portanto o contraexemplo não depende de inventar outro escritor. Reprodução em duas conexões: **[nao verificado]**. (`app/chamados/services/fecho_runtime.py:105`; `app/chamados/services/fecho_runtime.py:116`; `app/chamados/tests/test_e4b_c6_memoria.py:119`; `app/chamados/tests/test_e4b_c6_memoria.py:128`)

1. Quanto aos achados da c6:

| Achado anterior | Classificação nesta volta |
|---|---|
| Dia civil da conversa virando competência no novo agrupador | **Curado pela CLASSE nesse leitor.** Toda pergunta passa por `_dia_de_turno_da_pergunta`; `None` sai antes de `janela_atual`, sem data substituta. (`app/chamados/services/fecho_runtime.py:100`; `app/chamados/services/fecho_runtime.py:107`; `app/chamados/services/fecho_runtime.py:108`; `app/chamados/juizes.py:336`) |
| Duas Pautas concorrentes para marcador+âncora | **Curado pela CLASSE dos escritores E4B participantes**, pela trava determinística anterior ao `exists`, na mesma transação da criação. A cura introduz B1. **Na porta `escrever`, não achei idempotência:** ela continua fazendo `create`; o modelo não impõe unicidade dessa entrega. (`app/chamados/services/fecho_runtime.py:86`; `app/chamados/services/fecho_runtime.py:115`; `app/chamados/services/fecho_runtime.py:117`; `app/pautas/services.py:180`; `app/pautas/models.py:123`) |
| Motivo limbo truncado pela porta | **Curado pela CLASSE dos formatos produzidos pelo S143.** Reserva os motivos completos e abrevia ambas as listas com identificadores inteiros e contagens. O teste ORM mede antes da porta. (`app/chamados/services/disputa_emissao.py:1840`; `app/chamados/services/fecho_runtime.py:64`; `app/chamados/services/fecho_runtime.py:76`; `app/chamados/tests/test_e4b_c6_orm.py:226`) |
| Coleta em memória sobrevivendo ao rollback do S143 | **Curado pela CLASSE das falhas propagadas pelo resolver ou pela saída do savepoint.** O lote só é incorporado depois dessa saída sem erro; o token sempre é restaurado. (`app/chamados/signals.py:115`; `app/chamados/services/fecho_runtime.py:39`; `app/chamados/services/fecho_runtime.py:40`) |
| Reentrega não acrescenta novos IDs à Pauta existente | **Não curado — FILA.** Encontrar Pauta viva continua executando `continue`; a nota S143 conserva o rastro individual. (`app/chamados/services/fecho_runtime.py:118`; `app/chamados/services/fecho_runtime.py:120`; `app/chamados/services/disputa_emissao.py:1855`) |
| Fechamentos futuros fora das duas bordas sem entrega E4B | **Não curado — FILA de alcance.** `fechar()` continua usando transição direta; sem contexto, o coletor não entrega. O teste de passivo continua partindo de chamado já encerrado. (`app/chamados/models.py:454`; `app/chamados/services/fecho_runtime.py:19`; `app/chamados/tests/test_e4b_c6_orm.py:162`) |
| Relógio fora da borda | **Não curado — FILA preexistente.** S143 fornece `hoje=None`; C6 pode executar `date.today()`. (`app/chamados/services/disputa_emissao.py:1945`; `app/chamados/juizes.py:577`) |
| Falta de prova concorrente e de falha real de escrita da Pauta | **Não curado — FILA de prova.** O ensaio de trava é serial; o erro de memória ocorre durante a transição. Não achei ensaio de falha real de `escrever` nesses testes. (`app/chamados/tests/test_e4b_c6_orm.py:208`; `app/chamados/tests/test_e4b_c6_orm.py:209`; `app/chamados/tests/test_e4b_c6_memoria.py:113`) |
| Suíte inteira pendente | **Não curado — BLOQUEANTE de encerramento.** Os resultados fornecidos não são a suíte inteira exigida antes da leitura final. (`CLAUDE.md:375`; execução integral **[nao verificado]**) | [nao verificado]

A resposta às duas perguntas especiais é distinta: **não achei `data_evento` cru em `fecho_runtime.py`; ainda existe no caminho completo do S143**. A recusa de exportada lê `q.data_evento`; S128/HX passam o atributo cru para determinar a via, e o gate do chamado também o lê. Portanto a c7 cura o leitor acrescentado na c6, sem concluir a migração integral — **FILA preexistente**. (`app/chamados/services/fecho_runtime.py:107`; `app/chamados/services/disputa_emissao.py:1836`; `app/chamados/services/disputa_emissao.py:1984`; `app/chamados/services/disputa_emissao.py:2056`; `app/chamados/models.py:364`)

A retirada do retorno antecipado permanece: S128, HX e C7 seguem seus próprios juízes. O chamado pode fechar enquanto a disputa mista permanece aberta; isso é exigido pelo teste, não uma contradição entre estados. (`app/chamados/services/disputa_emissao.py:1950`; `app/chamados/services/disputa_emissao.py:1964`; `app/chamados/services/disputa_emissao.py:2007`; `app/chamados/services/disputa_emissao.py:2089`; `app/chamados/tests/test_e4b_c6_orm.py:117`; `app/chamados/tests/test_e4b_c6_orm.py:118`)

2. O mapa de escritores e leitores do delta é:

| Campo/valor | Quem escreve ou produz | Quem lê e com qual significado |
|---|---|---|
| `_RECUSAS` e token do ato | Declara `None`, instala o ato e restaura no `finally`. (`app/chamados/services/fecho_runtime.py:13`; `app/chamados/services/fecho_runtime.py:144`; `app/chamados/services/fecho_runtime.py:148`) | `registrar_recusa` e o contexto de confirmação leem o ato corrente. (`app/chamados/services/fecho_runtime.py:18`; `app/chamados/services/fecho_runtime.py:30`) |
| `ato/lote['chamado']` | Ato recebe `chamado.pk`; lote copia esse valor. (`app/chamados/services/fecho_runtime.py:143`; `app/chamados/services/fecho_runtime.py:34`) | Coletor compara com **PK de chamado**, não de pergunta ou disputa. (`app/chamados/services/fecho_runtime.py:19`) |
| Recusas coletadas | S143 produz motivo e chama o coletor; este indexa por **PK da pergunta**, guardando objeto+motivo. (`app/chamados/services/disputa_emissao.py:1840`; `app/chamados/services/disputa_emissao.py:1860`; `app/chamados/services/disputa_emissao.py:1950`; `app/chamados/services/fecho_runtime.py:20`) | Agrupador lê objeto, empresa, dia e motivo; montador interpreta os formatos do S143. (`app/chamados/services/fecho_runtime.py:106`; `app/chamados/services/fecho_runtime.py:110`; `app/chamados/services/fecho_runtime.py:66`) |
| “Savepoint confirmado” | **Não há campo persistido.** É o merge do lote após saída normal dos contextos. (`app/chamados/signals.py:115`; `app/chamados/services/fecho_runtime.py:40`) | Adaptador entrega o dicionário confirmado ao agrupador após a transição. Não significa commit da transação externa. (`app/chamados/services/fecho_runtime.py:142`; `app/chamados/services/fecho_runtime.py:149`) |
| `data_evento` / `conversa` | `abrir` fornece turno e `False`; `abrir_conversa` fornece dia civil e `True`; atribuição central em `gravar_dia_do_fato`. (`app/chamados/models.py:2789`; `app/chamados/models.py:2801`; `app/chamados/services/fio.py:801`) | Juiz distingue os significados; agrupador usa esse juiz. Os leitores crus remanescentes estão apontados acima. (`app/chamados/juizes.py:336`; `app/chamados/services/fecho_runtime.py:107`) |
| Empresa e corte | Relação do colaborador e cadastro de empresa; manutenção pela porta cadastral. (`app/colaboradores/models.py:321`; `app/colaboradores/models.py:46`; `app/colaboradores/services/estrutura.py:19`) | Runtime usa a empresa da pergunta; `janela_atual` consulta o corte efetivo. (`app/chamados/services/fecho_runtime.py:110`; `app/ponto/janelas.py:36`; `app/ponto/janelas.py:60`) |
| Competência, marcador, âncora e chave | Fim da janela produz `<empresa>:AAAA-MM`; marcador+tipo+âncora produzem a chave advisory. (`app/chamados/services/fecho_runtime.py:12`; `app/chamados/services/fecho_runtime.py:111`; `app/chamados/services/fecho_runtime.py:112`; `app/chamados/services/fecho_runtime.py:86`) | Dedup lê marcador+âncora; porta serializa o ID; chip e payload conservam o par. É competência por corte. Não achei leitor desse percurso reinterpretando-a como mês civil. (`app/chamados/services/fecho_runtime.py:118`; `app/pautas/services.py:184`; `app/pautas/services.py:334`; `app/pautas/views.py:40`) |
| Texto e listas | Montador reserva nomes e distribui os 500 caracteres; porta persiste. (`app/chamados/services/fecho_runtime.py:76`; `app/chamados/services/fecho_runtime.py:79`; `app/pautas/services.py:185`) | Payload DP entrega texto; motivo integral permanece na nota original. (`app/pautas/views.py:40`; `app/chamados/services/disputa_emissao.py:1855`) |
| Destinatário, remetente e autor | Runtime fornece DP/TI/autor central; porta valida e grava. (`app/chamados/services/fecho_runtime.py:122`; `app/chamados/services/fecho_runtime.py:124`; `app/pautas/services.py:157`; `app/pautas/services.py:180`) | Listagem filtra departamento; autoria é trilha. (`app/pautas/services.py:308`; `app/pautas/models.py:105`) |
| `feito_em` / `cancelado_em` | Portas centrais de estado da Pauta. (`app/pautas/services.py:247`; `app/pautas/services.py:294`) | Dedup usa `.vivas()`: feita ou cancelada permite nova entrega. (`app/chamados/services/fecho_runtime.py:119`; `app/pautas/models.py:57`; `app/pautas/models.py:64`) |
| `status_local` / destino / usuário | Bordas transportam; `_gravar_estado` atribui e salva. (`app/chamados/services/acoes_chamado.py:316`; `app/chamados/services/fio.py:722`; `app/chamados/models.py:228`; `app/chamados/models.py:296`) | Adaptador lê VIVOS→ENCERRADOS; signal lê estado salvo e se houve escrita do status. (`app/chamados/services/fecho_runtime.py:139`; `app/chamados/signals.py:96`; `app/chamados/signals.py:104`) |

3. Em `signals.py`, quem dispara continua sendo o `post_save` de `ChamadoColaborador`, quando não é carga `raw`, o estado está encerrado e o save escreveu o status. Um `.update()` direto não passa por esse receiver. (`app/chamados/signals.py:90`; `app/chamados/signals.py:96`; `app/chamados/signals.py:104`; `app/chamados/signals.py:95`)

No runtime, a sequência é transação do ato → transação de estado → save/receiver → savepoint S143. A novidade envolve o savepoint com o contexto coletor; Python sai primeiro do `atomic` e depois confirma o lote. Exceção no resolver ou na saída do savepoint descarta esse lote e é capturada fora dos contextos. **Não achei entrega de recusa revertida nesse percurso de falha.** (`app/chamados/services/fecho_runtime.py:142`; `app/chamados/models.py:280`; `app/chamados/models.py:296`; `app/chamados/signals.py:115`; `app/chamados/signals.py:117`; `app/chamados/services/fecho_runtime.py:39`; `app/chamados/services/fecho_runtime.py:40`)

A Pauta é escrita depois do receiver, ainda na transação externa: falha de escrita propaga e desfaz esse ato no banco. Fora das duas bordas, o contexto continua ausente; becos, reconciliador e fantasmas não ganham entrega E4B. (`app/chamados/services/fecho_runtime.py:149`; `app/chamados/services/fecho_runtime.py:19`; `app/chamados/services/becos.py:54`; `app/chamados/reconciliador.py:536`; `app/chamados/reconciliador.py:1648`; `app/ponto/management/commands/reconciliar_fantasmas.py:281`)

4. Das classes já reprovadas, **ordem de trava reaparece como B1**. Relógio fora da borda e campo temporal com dois significados permanecem nos leitores preexistentes, como FILA. **Não achei segundo juiz, fórmula paralela de competência ou persistência de Pauta fora da porta no delta produtivo.** (`app/chamados/services/fecho_runtime.py:117`; `app/pautas/services.py:354`; `app/chamados/juizes.py:577`; `app/chamados/services/disputa_emissao.py:1836`; `app/chamados/services/fecho_runtime.py:107`; `app/chamados/services/fecho_runtime.py:111`; `app/chamados/services/fecho_runtime.py:122`)

5. Nas provas, **não achei falso verde demonstrável nos quatro RED c7**, mas há limites importantes: [nao verificado]

- O fake de memória aceita escrita imediatamente numa lista, e seu `atomic` só controla profundidade. Ele não certifica rollback nem o contador DP — justamente a dependência de B1. (`app/chamados/tests/test_e4b_c6_memoria.py:36`; `app/chamados/tests/test_e4b_c6_memoria.py:62`)
- O teste ORM da trava usa uma conexão, chamadas seriais e segundo chamado/colaborador em memória. Prova `lock→exists`, sem testar a intercalação concorrente nem múltiplas âncoras disputando o contador. (`app/chamados/tests/test_e4b_c6_orm.py:196`; `app/chamados/tests/test_e4b_c6_orm.py:203`; `app/chamados/tests/test_e4b_c6_orm.py:208`)
- O texto não passa só por truncamento: mede antes da porta e compara com o persistido. O rollback exige escrita real revertida, chamado fechado e zero Pauta. (`app/chamados/tests/test_e4b_c6_orm.py:226`; `app/chamados/tests/test_e4b_c6_orm.py:228`; `app/chamados/tests/test_e4b_c6_orm.py:237`; `app/chamados/tests/test_e4b_c6_orm.py:249`)
- Não achei fixture invisível de viva/morta: as precondições consultam C6 e entrega. Já o teste de conversa injeta a recusa e chama diretamente o agrupador; sua prova é desse leitor. (`app/chamados/tests/test_e4b_c6_orm.py:78`; `app/chamados/tests/test_e4b_c6_orm.py:82`; `app/chamados/tests/test_e4b_c6_orm.py:182`)
- O protocolo exige GREEN anterior e assertion-alvo, rejeitando erro e precondição quebrada. Há mordida para as quatro curas, mas **não achei mordida para B1**. (`app/chamados/tests/e4b_c6_verificar.py:187`; `app/chamados/tests/e4b_c6_verificar.py:199`; `app/chamados/tests/e4b_c6_verificar.py:34`)
- O lint fornecido continua vermelho; o import não usado existe. O aviso sobre `falha` não demonstra falso verde: o contexto é consumido na própria iteração. (`app/chamados/tests/test_e4b_c6_memoria.py:7`; `app/chamados/tests/test_e4b_c6_memoria.py:264`; `app/chamados/tests/test_e4b_c6_memoria.py:272`; `c7_orm.log:6`, anexo)

Os GREENs e roteiros fornecidos não eliminam B1 nem substituem a suíte inteira. Execução independente, ocorrência do deadlock em produção e correspondência Git integral ao hash/base: **[nao verificado]**; metadados Git nesta cópia: **não achei**. [nao verificado]

_3934 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
