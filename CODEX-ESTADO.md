# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 08:19** · publicado em **17/11/2026 08:21** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 08:19 · sessao 728030717fda · E4C-F2 · verificador · origem=Code

**Parecer: bloqueia.** Há contraexemplos com pares de resposta legais que rompem a conservação: ressuscitação de pergunta validada, resposta automática posterior à validação e texto composto apenas de brancos. (`app/chamados/models.py:1647`; `app/chamados/services/disputa_emissao.py:1677`; `app/chamados/models.py:4173`)

Execução Django/SQL, incidência em produção, contagens mencionadas em comentários e identidade Git da cópia: **[nao verificado]**. Os contraexemplos abaixo são de código, sem afirmar ocorrência em produção.

1. **[bloqueia] Ressuscitar pergunta validada conserva justamente a validação que a porta apaga — perguntas 1 e 6.**

   Contraexemplo: pergunta com `validada_em=T2`, `materializada_em=T2`, resposta `(None, '')`, via `completude_turno` e célula furada. A ressuscitação grava a foto `VALIDACAO(T2)` e depois zera validação, resposta e carimbo; não grava ato que desfaça a validação. A dobra conserva `T2`, portanto o novo `estado` é **VALIDADA**, embora a projeção persistida esteja sem validação. O próprio teste exige esse resultado. (`app/chamados/models.py:1629`; `app/chamados/models.py:1647`; `app/chamados/models.py:4230`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:377`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:381`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:383`)

   Esse par de resposta é legal: não tem instante nem texto após o trim. Logo, a exceção dos pares ilegais não cobre a mudança. (`app/chamados/juizes.py:1534`; `app/chamados/juizes.py:1541`)

   Outro caminho com o mesmo formato é `devolver_instante_sem_texto_l130`: fotografa a resposta e zera o par sem ato limpador. Nesse caminho, porém, a guarda exclui perguntas validadas; permanece divergência do instante bruto, mas a resposta continua falsa pelo texto vazio. (`app/chamados/services/validacao.py:392`; `app/chamados/services/validacao.py:436`; `app/chamados/services/validacao.py:439`; `app/chamados/services/validacao.py:440`; `app/chamados/juizes.py:1552`)

2. **[bloqueia] RESPOSTA recebe um efeito que nem todos os seus escritores gravam — perguntas 1, 2 e 5.**

   | Escritor de RESPOSTA | O que faz com a projeção | Coerência com a regra nova |
   |---|---|---|
   | Resposta humana | Grava texto/instante e zera validação existente. (`app/chamados/services/disputa_emissao.py:1481`; `app/chamados/services/disputa_emissao.py:1494`; `app/chamados/services/disputa_emissao.py:1525`) | Coerente com RESPOSTA desfazendo validação. (`app/chamados/models.py:4201`) |
   | Resposta automática | Grava somente texto/instante; preserva validação. (`app/chamados/services/disputa_emissao.py:1674`; `app/chamados/services/disputa_emissao.py:1677`; `app/chamados/services/disputa_emissao.py:1678`) | Incoerente quando a resposta fica depois da validação. (`app/chamados/models.py:4232`) |
   | Resposta pela trilha | Grava somente instante; preserva texto e validação. (`app/chamados/services/disputa_emissao.py:1696`; `app/chamados/services/disputa_emissao.py:1698`; `app/chamados/services/disputa_emissao.py:1699`) | Mesma incoerência se o instante recuperado for posterior à validação. (`app/chamados/models.py:4232`) |
   | Foto da projeção, inclusive backfill/nascimento | Copia o par, com motivo especial. (`app/chamados/models.py:4175`; `app/chamados/models.py:4176`) | Excepcionalmente não desfaz validação, pelo conteúdo de `ato_motivo`. (`app/chamados/models.py:4201`) |

   Contraexemplo legal da automática: projeção inicial `(validada_em=T1, respondida_em=None, resposta_colab='')`; executar automática com texto e `T2>T1`. A projeção termina `(T1,T2,texto)`, mas a dobra termina `(None,T2,texto)`. A única guarda da automática é texto já preenchido; não há guarda de validação. (`app/chamados/services/disputa_emissao.py:1663`; `app/chamados/services/disputa_emissao.py:1673`; `app/chamados/services/disputa_emissao.py:1677`; `app/chamados/models.py:4201`)

   O chamador real também não impede: responde automaticamente **antes** de chamar `validar_pergunta`; esta encontra o carimbo preservado e devolve `ja_validada`. (`app/api/foto_recebida.py:48`; `app/api/foto_recebida.py:51`; `app/chamados/services/validacao.py:685`)

   A trilha também não guarda validação: só exige texto presente e instante ausente. Sua entrada pertence ao par ilegal excluído do corte, mas continua refutando a afirmação de que todo escritor de RESPOSTA grava o efeito atribuído pela dobra. (`app/chamados/services/disputa_emissao.py:1692`; `app/chamados/juizes.py:1541`)

   Portanto, há o problema de semântica pedido: **RESPOSTA humana desfaz validação; automática/trilha não; foto é distinguida por uma frase em `ato_motivo`**. (`app/chamados/services/disputa_emissao.py:1494`; `app/chamados/services/disputa_emissao.py:1677`; `app/chamados/services/disputa_emissao.py:1698`; `app/chamados/models.py:4201`)

3. **[bloqueia] Brancos sem instante são legais, mas deixam de ser conservados — pergunta 3.**

   Contraexemplo: sem linhas, `respondida_em=None`, `resposta_colab=' \t '`. Os predicados consideram esse par legal, porque brancos são texto vazio. `_sem_correspondencia` não fabrica RESPOSTA sem instante, e a dobra devolve `resposta_colab=''`. (`app/chamados/juizes.py:1528`; `app/chamados/juizes.py:1534`; `app/chamados/juizes.py:1541`; `app/chamados/models.py:4173`; `app/chamados/models.py:4226`)

   Pelo diff fornecido, `resposta_display` antes devolvia os brancos; agora devolve `''`. As fotos de SILENCIO e RETRATACAO também trocam os brancos por vazio. Isso viola conservação literal fora dos pares ilegais. (`app/chamados/models.py:3753`; `app/chamados/models.py:3755`; `app/chamados/models.py:2032`; `app/chamados/models.py:2164`)

   O M15 monta precisamente `None + brancos` e exige SILENCIO vazio: ele certifica essa mudança, não sua conservação. (`app/chamados/tests/test_cortes_f2_f9_m15.py:50`; `app/chamados/tests/test_cortes_f2_f9_m15.py:57`)

4. **[bloqueia] `q_vivas` deixa de conter toda pergunta viva pelo C6 — pergunta 4.**

   No contraexemplo da ressuscitação validada, o C6 recebe `validada_em=None` da projeção e pode declarar viva a pergunta da célula furada; o novo grau 0 SQL a exclui pela VALIDACAO conservada no ato. Isso contradiz o contrato explícito de continência. (`app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1405`; `app/chamados/juizes.py:1456`; `app/chamados/models.py:3556`; `app/chamados/models.py:3525`)

   O efeito alcança seleções anteriores ao juiz: o app filtra por `q_vivas` antes de chamar C6; o fio faz isso para disputas fechadas. A pergunta excluída não chega ao juiz. (`app/chamados/services/pergunta_no_app.py:52`; `app/chamados/services/pergunta_no_app.py:61`; `app/chamados/services/fio.py:255`; `app/chamados/services/fio.py:269`)

   Outros leitores que discordam no mesmo domínio:

   | Leitor que permanece | Discordância concreta |
   |---|---|
   | Card administrativo, lendo `validada_em` | Ressuscitada aparece sem selo de validação e pode oferecer ações, enquanto `estado` diz VALIDADA. (`app/templates/chamados/partials/_pergunta_card.html:20`; `app/templates/chamados/partials/_pergunta_card.html:23`; `app/templates/chamados/partials/_pergunta_card.html:42`; `app/chamados/models.py:3179`) |
   | Porta de validação, lendo projeção | No caso da automática, responde `ja_validada`, enquanto o estado pelo ato volta a RESPONDIDA. (`app/chamados/services/validacao.py:685`; `app/chamados/models.py:3187`) |
   | Fila de materialização | Continua exigindo validação projetada e não reconhece a VALIDADA apenas pelo ato da ressuscitada. (`app/chamados/services/materializacao.py:2093`; `app/chamados/services/materializacao.py:2110`) |
   | Partição do app e lista `respondidas` da API | Julgam a projeção, enquanto `.respondida` passou a julgar a dobra. (`app/chamados/services/pergunta_no_app.py:82`; `app/api/views.py:2578`; `app/chamados/models.py:3729`) |
   | Payload da API | Mistura texto/instante projetados com `resposta_display` migrado; por exemplo, pode devolver brancos em `resposta_colab` e vazio em `resposta_display`. (`app/api/views.py:2561`; `app/api/views.py:2562`; `app/api/views.py:2563`) |

   **`aguarda_admin` não é leitor independente da projeção:** continua exatamente derivado de `estado == RESPONDIDA`. Sua fonte está preservada, mas herda as mudanças de `estado`. (`app/api/views.py:2550`; `app/api/views.py:2553`)

   `tarefa_do_dia` já lê atos, porém julga a última linha, não a dobra: com V(T1) seguida de RESPOSTA-foto(T2), a dobra preserva validação e a tarefa pede VALIDAR pela última RESPOSTA. Essa divergência existe sem alterar `tarefa_do_dia`. (`app/chamados/models.py:4201`; `app/chamados/services/tarefa_do_dia.py:158`; `app/chamados/services/tarefa_do_dia.py:162`)

5. **[bloqueia] Prefetch velho altera o resultado da leitura — pergunta 5.**

   Contraexemplo concreto de operações:

   - Carregar `p` com decisões prefetched contendo V(T1); há fornecedor desse formato no fio. (`app/chamados/services/fio.py:263`)
   - Responder novamente em T2 pelo core: ele atualiza a instância, zera a validação e registra R(T2). (`app/chamados/services/disputa_emissao.py:1481`; `app/chamados/services/disputa_emissao.py:1494`; `app/chamados/services/disputa_emissao.py:1525`)
   - `registrar` usa `bulk_create` e não invalida o cache relacionado da instância. `leitura(p)` continua lendo o cache anterior. (`app/chamados/models.py:4247`; `app/chamados/models.py:4249`; `app/chamados/models.py:4217`)
   - Como R(T2) falta no cache, o novo par projetado entra como passivo com `desfaz=False`: V(T1) sobrevive. Em instância fresca, a RESPOSTA real desfaz V(T1). (`app/chamados/models.py:4221`; `app/chamados/models.py:4201`; `app/chamados/models.py:4232`)

   Resultado: mesmo banco, `leitura` da instância antiga validada e leitura fresca não validada; a face SQL acompanha a fresca. `prefetch_related_objects` também não resolve por si a instância que já chegou com decisões carregadas. (`app/chamados/models.py:1945`; `app/chamados/models.py:3494`; `app/chamados/models.py:4217`)

   O core aceita a instância e volta a lê-la após registrar, em `resposta_display`; encadeamento HTTP concreto passando uma instância prefetched até esse core: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:1439`; `app/chamados/services/disputa_emissao.py:1560`)

6. **[fila] Há N+1 novo em caminhos sem prefetch — pergunta 5.**

   O carregamento do app não faz prefetch de decisões. Cada `_item` da API lê `estado` e `resposta_display`; ambos executam `leitura`, que busca `decisoes.all()` sem cache. São duas buscas adicionais de decisões por pergunta nesse percurso, nos loops de histórico e pendentes. (`app/chamados/services/pergunta_no_app.py:52`; `app/api/views.py:2550`; `app/api/views.py:2562`; `app/chamados/models.py:3178`; `app/chamados/models.py:3753`; `app/chamados/models.py:4217`; `app/api/views.py:2567`; `app/api/views.py:2577`)

   Também há busca por pergunta nas fotos de fechamento e retratação: os respectivos lotes não recebem prefetch nesse carregamento. O prefetch de `sem_veredito_entre` não garante cache nas novas instâncias buscadas depois em `self.perguntas.all()`. (`app/chamados/models.py:1945`; `app/chamados/models.py:2000`; `app/chamados/models.py:2032`; `app/chamados/models.py:2160`; `app/chamados/models.py:2164`)

   Contagem executada e impacto de latência: **[nao verificado]**.

7. **[fila] Os testes não passam por universo vazio, mas não fecham a conservação — pergunta 5.** [nao verificado]

   Há asserções que impedem vazio: universo com 13 perguntas e oito validadas; registro com sete leitores; descarimbo efetivo; ressuscitação de exatamente uma pergunta; M18 exigindo exatamente uma chamada observada ao C6. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:165`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:168`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:303`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:326`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:353`; `app/chamados/tests/test_cortes_f2_f9_m18.py:72`)

   As lacunas concretas são:

   - Automática e trilha são exercitadas **antes** da validação; não mordem os escritores que preservam validação. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:188`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:191`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:235`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:239`)
   - Os helpers recarregam a pergunta, retirando do teste o cache velho. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:38`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:176`)
   - O controle da ressuscitação usa instante sem texto, justamente uma exceção do corte. A caracterização seguinte exige a quebra com pergunta validada legal. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:352`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:381`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:383`)
   - A mordida do selo conserva projeção e atos alinhados; ela demonstra retorno aos leitores após descarimbo, mas não distingue leitura antiga da projeção de leitura nova dos atos nesses casos. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:315`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:328`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:331`)
   - M4/M13/M14 mordem mudança para ato com projeção vazia; essa mudança não é, por si, prova de conservação. M15 exige a normalização de brancos identificada no achado 3. (`app/chamados/tests/test_cortes_f2_f9_m4.py:51`; `app/chamados/tests/test_cortes_f2_f9_m13.py:43`; `app/chamados/tests/test_cortes_f2_f9_m14.py:48`; `app/chamados/tests/test_cortes_f2_f9_m15.py:57`)

   Execução dos testes e dos mutantes: **[nao verificado]**. [nao verificado]

8. **[nota] Face SQL e dobra concordam com dados frescos; isso não resolve os bloqueios — pergunta 3.**

   A equivalência estrutural da validação é:

   | Combinação | Resultado das duas faces |
   |---|---|
   | VALIDACAO real sem limpador posterior | Validada. (`app/chamados/models.py:3497`; `app/chamados/models.py:4230`) |
   | Empate entre linhas reais | Desempate por pk. (`app/chamados/models.py:3496`; `app/chamados/models.py:4219`; `app/chamados/models.py:4224`) |
   | Validação passiva empatada com limpador real | Passivo vem depois e permanece. (`app/chamados/models.py:3501`; `app/chamados/models.py:4221`) |
   | Carimbo casado com VALIDACAO posteriormente desfeita | Não recria passivo. (`app/chamados/models.py:3499`; `app/chamados/models.py:4177`) |
   | RESPOSTA-foto sem VALIDACAO-foto | Não limpa validação anterior. (`app/chamados/models.py:3494`; `app/chamados/models.py:4201`) |
   | Texto apenas de brancos | Não participa do predicado de validação; afeta resposta/display separadamente. (`app/chamados/models.py:3494`; `app/chamados/models.py:4232`) |

   `_sem_correspondencia` é efetivamente o mesmo corpo usado pela porta de fotos e pela dobra; a diferença relevante é a origem de `ja`: banco na porta, decisões carregadas na dobra. Com cache velho, essa origem pode divergir, como no achado 5. (`app/chamados/models.py:4160`; `app/chamados/models.py:4162`; `app/chamados/models.py:4218`; `app/chamados/models.py:4222`)

   A verificação em memória das funções extraídas por AST não encontrou divergência em 50.895 combinações finitas; execução do SQL real: **[nao verificado]**. A conclusão geral acima decorre dos dois ramos do predicado e da ordenação da dobra, não dessa amostra isoladamente. (`app/chamados/models.py:3494`; `app/chamados/models.py:3503`; `app/chamados/models.py:4224`; `app/chamados/models.py:4232`)

9. **[nota] Demais classes de reprovação — pergunta 5.**

   - **Segundo juiz:** `respondida` continua chamando o C21 registrado; mudou seu insumo. O bloqueio demonstrado é a divergência com C6 e com os escritores, não a criação de outro predicado de “instante E texto”. (`app/core/juizes.py:291`; `app/core/juizes.py:306`; `app/chamados/models.py:3729`)
   - **Derivação paralela:** a correspondência passiva foi compartilhada. A condição de RESPOSTA limpadora permanece expressa em Python e SQL; hoje as expressões concordam entre si, mas não com todos os escritores. (`app/chamados/models.py:4162`; `app/chamados/models.py:3494`; `app/chamados/models.py:4201`; `app/chamados/services/disputa_emissao.py:1677`)
   - **Relógio fora da borda:** os novos corpos da face SQL e da dobra não leem relógio. A dobra lê inclusive atos futuros; `tarefa_do_dia` corta pelo horizonte recebido, portanto são leituras temporalmente diferentes. (`app/chamados/models.py:3490`; `app/chamados/models.py:4217`; `app/chamados/services/tarefa_do_dia.py:423`)
   - **Ordem de trava:** os novos leitores não adquirem travas; os blocos de escrita examinados continuam adquirindo a ordem centralizada antes de escrever. Não demonstrei inversão nova causada pela F2. (`app/chamados/models.py:1628`; `app/chamados/models.py:2015`; `app/chamados/models.py:2132`; `app/chamados/models.py:2134`; `app/chamados/models.py:2796`)
   - **Segundo escritor da sequência:** a escrita física de produção localizada continua em `registrar`; o modelo também permite inserção inicial por `save`, embora recuse reescrita. Não apareceu produtor novo fora da porta no diff fornecido. (`app/chamados/models.py:4249`; `app/chamados/models.py:4136`; `app/chamados/models.py:4139`)
   - **Ato/campo conservado novo:** os atos continuam os oito declarados; `LeituraDoAto` é estrutura de retorno, não campo persistido. (`app/chamados/models.py:19`; `app/chamados/models.py:4090`; `app/chamados/models.py:4091`)

10. **[nota] Censo dos escritores e dos resets — pergunta 1.**

    Nesta tabela, V=`validada_em`, R=`respondida_em`, T=`resposta_colab`, M=`materializada_em`, via=`via_resolucao`.

    | Escritor | Campos/linha registrada no mesmo ato | A leitura reproduz a projeção? |
    |---|---|---|
    | Nascimento | Recebe campos por `**campos`, salva e grava fotos R/V. (`app/chamados/models.py:3149`; `app/chamados/models.py:3152`; `app/chamados/models.py:3154`) | Sim para as fotos correspondentes. (`app/chamados/models.py:4173`; `app/chamados/models.py:4177`) |
    | Resposta humana | T/R; V=None quando validada; fotos antigas + RESPOSTA. (`app/chamados/services/disputa_emissao.py:1480`; `app/chamados/services/disputa_emissao.py:1481`; `app/chamados/services/disputa_emissao.py:1494`; `app/chamados/services/disputa_emissao.py:1525`) | Na ordenação cronológica correspondente, sim. (`app/chamados/models.py:4224`; `app/chamados/models.py:4232`) |
    | Automática | T/R; fotos + RESPOSTA. (`app/chamados/services/disputa_emissao.py:1673`; `app/chamados/services/disputa_emissao.py:1674`; `app/chamados/services/disputa_emissao.py:1678`) | Não no contraexemplo do achado 2. |
    | Pela trilha | R; RESPOSTA, preservando V. (`app/chamados/services/disputa_emissao.py:1696`; `app/chamados/services/disputa_emissao.py:1699`) | Não necessariamente; achado 2. |
    | Validação | V=agora; fotos + VALIDACAO no mesmo instante. (`app/chamados/services/validacao.py:168`; `app/chamados/services/validacao.py:173`) | VALIDACAO grava V na dobra. (`app/chamados/models.py:4230`) |
    | Devolução por recusa de materialização | **V=None**; fotos + DEVOLUCAO. (`app/chamados/services/validacao.py:273`; `app/chamados/services/validacao.py:279`) | DEVOLUCAO limpa V e preserva R/T. (`app/chamados/models.py:4189`; `app/chamados/models.py:4232`) |
    | Devolução BUG-PAR-103 | Descarimbo e **V=None**; fotos + DEVOLUCAO. (`app/chamados/services/validacao.py:346`; `app/chamados/services/validacao.py:349`; `app/chamados/services/validacao.py:354`) | Mesmo efeito sobre V/R/T. (`app/chamados/models.py:4232`; `app/chamados/models.py:4234`) |
    | Reabertura/recusa | **V=None, R=None, T=''**, mais descarimbo; RECUSA ou REABERTURA com foto. (`app/chamados/services/disputa_emissao.py:1609`; `app/chamados/services/disputa_emissao.py:1631`; `app/chamados/services/disputa_emissao.py:1648`) | Esses atos limpam V/R/T. (`app/chamados/models.py:4189`; `app/ch

[truncado em 20000 de 35429 caracteres]

## 17/11 08:19 · sessao 3c73bf371cb4 · PISTA-VOLTOU-1711 · verificador · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 17/11 08:19 · sessao 0cef7d1edeea · - · - · origem=Ronald (tmux)

- **Pré-opus** = suíte **inteira da árvore da obra verde antes do opus final**; não basta recorte, nem suíte rodada depois da leitura final. A regra não exige VM explicitamente: `CLAUDE.md:377-378`; `app/docs/PISTA.md:113-114`.
- **Não concluir colisão automática entre pré-opus na pista e pouso certificado na VM.** O contrato antigo dizia pista não certificar nem pré-opus (`app/docs/BACKLOG.md:2098`), mas registro posterior diz expressamente “PISTA-2 certifica pre-opus” (`app/docs/BACKLOG.md:2266`). O precedente de lateral também separa pista e certificação de pouso na VM: `app/docs/CORTES.md:493`. Isso exige reconhecer a evolução documental, sem transportar a proibição antiga como absoluta.
- **SUITE-UMA-VEZ-POR-ARVORE:** a parte operativa exige identidade da árvore, ausência de mudanças em `.py` entre veredito e merge, `cmp` contra a cópia e uma suíte para comboio; árvore diversa não herda certificação: `CLAUDE.md:320-324`; `app/docs/PISTA.md:35-38`. Duas execuções determinadas pelo arranjo pista/VM não são, por si, violação provada; o precedente da VM após pista está em `app/docs/CORTES.md:493`.
- **VM antes do pouso:** pode certificar o pouso e ser a suíte pré-opus se ocorrer **antes** desse opus, inteira e verde da mesma árvore (`CLAUDE.md:377-378`, `CLAUDE.md:320-321`). Se ocorrer depois, não sana retroativamente pré-opus ausente, recortado, vermelho ou de outra árvore (`app/docs/PISTA.md:113-114`, `app/docs/PISTA.md:38`).
- **PISTA-2 item 2:** o literal vigente permite espinha nas duas quando “VM livre e pista livre”, enquanto o item 3 proposto envia à pista com VM ocupada/fila. Há mudança operacional delimitada que precisa ser lida como exceção do novo complemento; não é dedutível da regra anterior: `app/docs/CORTES.md:490`.
- **“2 construtores”** mede vagas/papéis de construção simultâneos, teto padrão `1 opus + 1 Codex`; medidas, censos, sondas e RED por exemplo ficam fora desse teto (`app/docs/LEIS.md:147`). O teto é conferido antes de abrir (`CLAUDE.md:291-292`); o wrapper guarda apenas um Codex construtor por repositório e não guarda o limite de opus (`CLAUDE.md:355-356`). “Dois construtores da espinha abertos agora”, com nomes e obras, não foi demonstrado pelo dado colado: `[nao verificado]`.
- **Decisor da rota:** decisões técnicas e encaixe de raia são do Code (`CLAUDE.md:276-281`). A rota em `suite.sh` continua apenas registrada, construção mediante novo aval; até lá, conduta (`app/docs/BACKLOG.md:2100`). Não atribuir decisão automática a esse script.
- **Estado externo:** `app/docs/BACKLOG.md:2101` e `app/docs/RELATO.md:99` registram censo parcial e peças faltantes; são registros da cópia, não medições atuais desta revisão. Não afirmar paridade atual da pista como provada.

## 17/11 08:18 · sessao b1472cd961fc · - · - · origem=Ronald (tmux)

- Porta atual da pista é manual e exige árvore commitada: `git update-ref refs/pista/<sha> <sha>` e `ssh pista pede <sha> [--only <labels>] [--parallel N]`; espera por `ssh pista espera <id>`, consulta por `log`/`estado`, fila própria com uma suite por vez fora da trava VM — `app/docs/BACKLOG.md:2098`.
- O contrato atual é explícito: pista é **PRE-FILTRO**, “não certifica pouso nem a suite que o opus recebe (essas seguem na VM pela trava)” — `app/docs/BACKLOG.md:2098`. Portanto o problema do item 3 não se limita à certificação do pouso: usar pista como a única suite anterior ao opus altera também essa cláusula.
- A rota dentro de `bin/suite.sh` está somente registrada, construção depende de novo aval; até lá o uso é conduta — `app/docs/BACKLOG.md:2100`; o executor atual roda `docker run` local sob `trava_teste.sh` — `bin/suite.sh:317`.
- `--quem` apenas lê `/tmp/juliani_db_test.lock.quem` — `bin/trava_teste.sh:21`, `bin/trava_teste.sh:28`. A trava real é adquirida com `flock -w`; dono é escrito somente após aquisição e substituído por “livre” ao sair; nenhum registro de esperadores aparece nesse fluxo — `bin/trava_teste.sh:35`, `bin/trava_teste.sh:40`, `bin/trava_teste.sh:41`. A documentação avisa que responde chamador, não suite em voo — `CLAUDE.md:131`.
- O histórico/bloco de fila no ESTADO ainda é obra registrada aberta — `app/docs/BACKLOG.md:2654`. Não há nesse mecanismo leitura comprovada de “com fila” na hora do pedido; situação real em produção `[nao verificado]`.
- `pista_lock.sh` usa outro arquivo (`logs/.pista_lock`), registra dono por existência do arquivo e não usa o flock da VM; a prioridade é explicitamente não automatizada — `bin/pista_lock.sh:11`, `bin/pista_lock.sh:14`, `bin/pista_lock.sh:18`, `bin/pista_lock.sh:29`.
- O teto antigo é 2 construtores = 1 opus + 1 Codex; leituras/censos/sondas/RED por exemplo ficam fora do teto — `app/docs/LEIS.md:147`. O wrapper só guarda uma vaga Codex; “1 opus” é conduta — `CLAUDE.md:355`. Dois construtores abertos não mede ocupação nem fila de suites; o disparador “na espinha” ainda precisaria identificar as duas obras no momento do pedido `[nao verificado]`.
- O censo da certificação está registrado como parcial, com três peças faltantes e scripts remotos não lidos — `app/docs/BACKLOG.md:2101`. Isso é registro documental, não medição nova do estado atual `[nao verificado]`.

## 17/11 08:14 · sessao 0c92dca9b7d6 · - · - · origem=Ronald (tmux)

Auditoria somente de código, sem banco/log/testes. Os números de produção que aparecem em comentários do código são **[nao verificado]**; por exemplo, os 187 casos em `app/chamados/models.py:4200` e os 15.114/50.829 em `app/chamados/models.py:4213`.

1. **Bloqueia — quebra da continência de `q_vivas` e do C6.** Ressuscitação zera `validada_em` conservando a linha VALIDACAO (`app/chamados/models.py:1629`, `app/chamados/models.py:1647`). A nova dobra mantém VALIDACAO (`app/chamados/models.py:4228`), `estado` devolve VALIDADA (`app/chamados/models.py:3179`), e `q_vivas` exclui a pergunta (`app/chamados/models.py:3556`). Entretanto, o C6 lê `validada_em=None` da projeção (`app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1405`) e, numa célula `furo`, devolve viva (`app/chamados/juizes.py:1456`). O contrato escrito exige que toda viva esteja no SQL (`app/chamados/models.py:3525`). O app usa o SQL antes do C6, portanto perde a pergunta antes de julgá-la (`app/chamados/services/pergunta_no_app.py:52`, `app/chamados/services/pergunta_no_app.py:61`); o mesmo ocorre no fio para disputas fechadas (`app/chamados/services/fio.py:255`, `app/chamados/services/fio.py:269`).

2. **Bloqueia — leitura pode depender de cache anterior ao ato.** `leitura` usa `p.decisoes.all()` (`app/chamados/models.py:4217`); `registrar` faz `bulk_create` e não invalida o cache da instância recebida (`app/chamados/models.py:4247`, `app/chamados/models.py:4249`). Contraexemplo: objeto prefetched com VALIDACAO em T1; resposta nova em T2 pelo core zera a projeção de validação (`app/chamados/services/disputa_emissao.py:1481`, `app/chamados/services/disputa_emissao.py:1494`) e registra RESPOSTA (`app/chamados/services/disputa_emissao.py:1525`). Na instância com cache, falta a linha R(T2), então a resposta projetada entra como passivo com `desfaz=False` (`app/chamados/models.py:4221`), conservando V(T1); numa instância fresca, R(T2) desfaz V(T1) (`app/chamados/models.py:4201`). Existe fornecedor real de instâncias prefetched no fio (`app/chamados/services/fio.py:263`) e o core relê o mesmo objeto depois de registrar, em `resposta_display` (`app/chamados/services/disputa_emissao.py:1560`). Encadeamento HTTP concreto que passa uma dessas instâncias prefetched ao core: **[nao verificado]**.

3. **Fila — N+1 introduzido na API e nas fotos.** O carregamento do app não prefetch decisões (`app/chamados/services/pergunta_no_app.py:52`). `_item` chama `estado` e `resposta_display` (`app/api/views.py:2550`, `app/api/views.py:2562`), ambos chamando `leitura` (`app/chamados/models.py:3178`, `app/chamados/models.py:3753`), que consulta decisões sem cache (`app/chamados/models.py:4217`): até **2N consultas adicionais** nos loops de histórico e pendentes (`app/api/views.py:2567`, `app/api/views.py:2577`). As fotos novas também consultam por pergunta no fechamento (`app/chamados/models.py:2000`, `app/chamados/models.py:2032`) e retratação (`app/chamados/models.py:2164`). O prefetch feito dentro de `sem_veredito_entre` (`app/chamados/models.py:1945`) não garante cache nas novas instâncias de `self.perguntas.all()` em `app/chamados/models.py:2000`. Medição efetiva de queries/latência: **[nao verificado]**.

4. **Bloqueia — duas verdades na mesma tela/API.** No contraexemplo da ressuscitação validada, `estado=VALIDADA`, mas o card testa `perg.validada_em` e oferece ações como não validada (`app/templates/chamados/partials/_pergunta_card.html:20`, `app/templates/chamados/partials/_pergunta_card.html:23`, `app/templates/chamados/partials/_pergunta_card.html:42`). Correção administrativa também aceita a projeção sem validação (`app/chamados/services/acoes_disputa.py:349`), embora o estado diga VALIDADA (`app/chamados/models.py:3179`). Materialização filtra pelo carimbo projetado, portanto não trata essa VALIDADA pelo ato (`app/chamados/services/materializacao.py:2093`, `app/chamados/services/materializacao.py:2110`). `aguarda_admin` continua derivado de `estado == RESPONDIDA`, exatamente na fonte proibida de alterar (`app/api/views.py:2550`, `app/api/views.py:2553`); **não** é leitor independente da projeção. Entretanto, a mesma resposta inclui texto/instante projetados junto do display migrado (`app/api/views.py:2561`, `app/api/views.py:2562`, `app/api/views.py:2563`), permitindo payload incoerente quando projeção e atos divergem. A seleção `respondidas` continua julgando a projeção (`app/api/views.py:2578`).

5. **Nota — tarefa do dia já interpreta atos, mas de outra maneira.** Ela julga a última linha, não a dobra: RESPOSTA → VALIDAR (`app/chamados/services/tarefa_do_dia.py:158`, `app/chamados/services/tarefa_do_dia.py:159`, `app/chamados/services/tarefa_do_dia.py:162`); VALIDACAO → conferir lavra/efeito (`app/chamados/services/tarefa_do_dia.py:167`); SILENCIO → encerrada (`app/chamados/services/tarefa_do_dia.py:176`). Contraexemplo: V(T1) seguida de R-foto(T2); a dobra preserva validade pelo motivo especial (`app/chamados/models.py:4201`), mas a tarefa pede VALIDAR pela última RESPOSTA (`app/chamados/services/tarefa_do_dia.py:159`). Seu horizonte corta atos futuros (`app/chamados/services/tarefa_do_dia.py:423`); a nova leitura não corta (`app/chamados/models.py:4217`). Alteração de tarefa_do_dia não consta do diff fornecido; identidade do diff com o commit real **[nao verificado]**, pois a cópia não possui `.git`.

Censo de leitores diretos abaixo. Notação: caminhos relativos a `app/`; múltiplas linhas na mesma entrada indicam sítios no mesmo arquivo. Não confundir leitores do resultado `LeituraDoAto` com leitores da projeção: `models.py:1948`, `models.py:3179`, `models.py:3753` já recebem o resultado migrado.

**`validada_em`:**

- `chamados/models.py`: `_universo_da_ressuscitacao` 1554; `ressuscitar_perguntas` 1630; `sem_veredito_entre` 1948; `estado` 3179; `carimbar` 3266,3273; SQL 3500,3501,3503; passivo 4157,4177,4178,4179; dobra 4223.
- `chamados/juizes.py`: memo 856; projeção temporal/C6 1389.
- `chamados/reconciliador.py`: 632,1051.
- `chamados/services/materializacao.py`: 199,201,1155,2093,2110.
- `chamados/services/validacao.py`: 173,181,269,343,392,640,685,898,1064,1085.
- `chamados/services/disputa_emissao.py`: 1484,1485,1636,2062.
- `chamados/services/declaracao_texto.py:228`; `chamados/services/vigia_carimbo.py:98`; `chamados/services/verbos.py:264,270`; `chamados/services/painel.py:67,270,271,272,273`; `chamados/services/acoes_disputa.py:349`.
- `ponto/services/flip_auto.py:130`; `ponto/services/procedencia_batida.py:74`.
- Histórico dinâmico: `chamados/services/historico.py:101,105`.
- Comandos: `chamados/management/commands/backfill_decisoes_pergunta.py:56,60,102`; `reabrir_orfas_enterradas.py:68`; `encerrar_perguntas_com_fato.py:32`; `auditar_invariantes_chamados.py:206,1077,1578,1607,1772,1784`; `bug_par_103.py:68`.
- Templates: `templates/chamados/partials/_pergunta_card.html:20,23,42`; `modal_fio.html:228`.
- Fotografias genéricas: `chamados/services/foto_e4b.py:250,251,272`; reversão `chamados/services/passivo_validado.py:64,66,83`.

**`respondida_em`:**

- `chamados/models.py`: 1554,1630,2974,3266,3270,3409,3411,3644,3645,4157,4173,4174,4175,4223.
- `chamados/juizes.py`: `_slot_esperado` 352; memo 856; projeção/C6 1389; `respondida_sem_texto` 1534; `texto_sem_carimbo` 1541; juiz 1552; SQL 1572,1580,1591; fila/ordenação 1765.
- `chamados/veredito_lavrado.py:207,208`; `chamados/services/painel.py:67`; `chamados/services/vigia_fio.py:747`.
- `chamados/services/materializacao.py:1154`; `chamados/services/validacao.py:434,913,1256`; `chamados/services/disputa_emissao.py:1635,1692,1861,2074,2091`; `chamados/services/fio.py:187`; `chamados/services/raiox.py:138`.
- `chamados/services/verbos.py:337,338,344,347`.
- Histórico dinâmico `chamados/services/historico.py:99,105`; reversão `chamados/services/passivo_validado.py:66,83`; fotografia genérica `chamados/services/foto_e4b.py:251,272` e detector de ressuscitação `chamados/services/foto_e4b.py:180`.
- API: `api/views.py:1165,2563,2639`; `api/views_mensageria.py:569,570`.
- Comandos: `chamados/management/commands/estoque_l130_instante_sem_texto.py:58`; `curar_perguntas_em_limbo.py:46,69`; `auditar_invariantes_chamados.py:1107`; `backfill_decisoes_pergunta.py:56,60,504`.
- Templates: `templates/chamados/partials/modal_fio.html:220`; `_card_atendimento.html:106,143,391`; `_pergunta_card.html:32`.

**`resposta_colab`:**

- `chamados/models.py`: 1554,1630,2032,2164,2975,3269,3408,3412,3644,3646,3753,4176,4180.
- `chamados/juizes.py`: 364,857,1360,1389,1534,1541,1552,1572,1580,1791,1938,2042,2047,2064.
- `chamados/services/materializacao.py`: 546,574,625,671,678,718,733,770,953,1050,1112,1153,1372,1554,1580,1589,1641.
- `chamados/services/validacao.py`: 175,281,356,434,467,912,1183,1185,1187,1192,1193,1194,1195,1198.
- `chamados/services/disputa_emissao.py`: 1635,1663,1692,1700,1861,2079.
- `chamados/services/declaracao_texto.py:418`; `chamados/services/explicador.py:55,102,103`; `chamados/services/acoes_disputa.py:357`; `chamados/services/fio.py:377`; `chamados/services/historico.py:109`; `chamados/services/vigia_fio.py:750`; `chamados/services/raiox.py:138`; `chamados/views_cobrar.py:293`.
- `chamados/templatetags/chamados_tags.py:139,166`; `ponto/services/veredito_celula.py:197`; `ponto/services/dia_do_colab.py:70`.
- API: `api/views.py:1163,2561`; `api/views_mensageria.py:567`.
- Comandos: `chamados/management/commands/estoque_l130_instante_sem_texto.py:58`; `curar_perguntas_em_limbo.py:32,46`; `validar_classe_a.py:80`; `auditar_invariantes_chamados.py:1108`; `backfill_decisoes_pergunta.py:61,326,504`.
- Templates: `templates/chamados/partials/modal_fio.html:219`; `_card_atendimento.html:106,114,141,143,148,154,182,183,391`; `_lote_preview.html:24`.
- Fotografia genérica: `chamados/services/foto_e4b.py:251,272`.

**`materializada_em`:**

- `chamados/models.py`: 1539,1948,2031,3181,3185,3195,3265,3321,3322,3440,3452,3469,3542,3544.
- `chamados/juizes.py`: 292,683,711,858,1390,1502.
- `chamados/reconciliador.py`: 368,631,1051,1147,1157,1507.
- `chamados/veredito_lavrado.py:203,204`; `chamados/views.py:1257`.
- `chamados/services/materializacao.py`: 148,261,262,289,326,862,874,1018,1110,1112,1117,1156,1680,2094,2107,2155.
- `chamados/services/validacao.py`: 264,341,394,643,769,805.
- `chamados/services/disputa_emissao.py`: 228,776,1084,1104,1340,1637,1930,2062.
- `chamados/services/fabrica_da_celula.py:171`; `chamados/services/raiox.py:135,182`; `chamados/services/painel.py:67`; `chamados/services/relavra_via.py:181,244`; `chamados/services/passivo_validado.py:534,536`.
- `ponto/services/veredito_celula.py:196`; `ponto/services/procedencia_batida.py:74`; `ponto/management/commands/reconciliar_fantasmas.py:213`.
- Histórico dinâmico `chamados/services/historico.py:103,105`; fotografia genérica `chamados/services/foto_e4b.py:251,272` e detector `foto_e4b.py:180`; reversão `chamados/services/passivo_validado.py:64,83`.
- Comandos: `chamados/management/commands/migrar_data_evento_dia_do_turno.py:147`; `encerrar_perguntas_com_fato.py:32`; `auditar_disputas.py:71,150`; `reabrir_orfas_enterradas.py:67,88,91,129,142`; `backfill_decisoes_pergunta.py:101,325`; `bug_par_103.py:66`; `auditar_invariantes_chamados.py:471,539,589,907,957,1077,1158,1579,1607,1773`.
- `chamados/services/regularizacao_ext.py:220` lê **outro modelo**, RegularizacaoExterna, e não integra esse domínio.

**`via_resolucao`:**

- `chamados/models.py`: 1543,2025,2035,2161,2166,2854,2885,3194,3265,3321,3340,3421,3429,3440,3452,3469,3476,3542,3544,3712,3718.
- `chamados/juizes.py`: 683,695,857,1390.
- `chamados/reconciliador.py:1146,1157`; `chamados/services/fabrica_da_celula.py:149`; `chamados/services/competencia_trancada.py:276`.
- `chamados/services/materializacao.py`: 261,292,862,868,1111,1153.
- `chamados/services/validacao.py:341,435`; `chamados/services/disputa_emissao.py:337,409,428,446,1637,2072,2087,2090`.
- `chamados/services/fio.py`: 1148,1215,1294,1401,1484,1925,1961,1983,2028.
- `chamados/services/tarefa_do_dia.py:256,257`; `chamados/services/explicador.py:118`; `chamados/services/relavra_via.py:118,121,123,155,181,245`; `chamados/services/passivo_validado.py:141,142,530`; `ponto/services/cartorio.py:1651`.
- Fotografia genérica `chamados/services/foto_e4b.py:251,272`; reversão `chamados/services/passivo_validado.py:64,83`.
- Comandos: `chamados/management/commands/estoque_l130_instante_sem_texto.py:59`; `migrar_data_evento_dia_do_turno.py:70,71,72`; `encerrar_perguntas_com_fato.py:33,42,49,66,76`; `auditar_invariantes_chamados.py:203,207,434,543,957,1716`; `reabrir_orfas_enterradas.py:67,129,142,160`; `relavrar_via_i6.py:58,59,62,66,68,88`; `backfill_decisoes_pergunta.py:101,314,330,398,504`; `bug_par_103.py:66`.
- Template `templates/chamados/partials/_card_atendimento.html:192`.

**Campos da DecisaoPergunta:**

- `ato`: SQL em `chamados/models.py:3494,3497,3499`; histórico em `chamados/models.py:3740,3744`; correspondência em `chamados/models.py:4160,4161`; dobra em `chamados/models.py:4218,4219`; tarefa em `chamados/services/tarefa_do_dia.py:159,163,165,167,176,178,187,260,425`; BUG-PAR em `chamados/services/materializacao.py:275`; backfill em `chamados/management/commands/backfill_decisoes_pergunta.py:99,315`.
- `ato_em`: SQL `chamados/models.py:3496,3500,3501`; ordenação do modelo `chamados/models.py:4122`; correspondência `chamados/models.py:4161`; dobra `chamados/models.py:4218,4219,4221`; carga da tarefa `chamados/services/tarefa_do_dia.py:422,423,431`; template `templates/chamados/partials/modal_fio.html:210,212`.
- `ato_texto`: dobra `chamados/models.py:4219,4221`; tarefa `chamados/services/tarefa_do_dia.py:160`; template `templates/chamados/partials/modal_fio.html:210,212`.
- `ato_motivo`: SQL `chamados/models.py:3494`; dobra `chamados/models.py:4219`; BUG-PAR `chamados/services/materializacao.py:275`; template `templates/chamados/partials/modal_fio.html:212`.
- `pk` da decisão: desempate SQL `chamados/models.py:3496`; ordenação `chamados/models.py:4122`; desempate da dobra `chamados/models.py:4219`. A tarefa ordena por `ato_em,pk` (`chamados/services/tarefa_do_dia.py:422`).
- Todos esses campos, incluindo pk, também são lidos pela fotografia genérica `chamados/services/foto_e4b.py:250,251,280`.

Completude absoluta para acessos indiretos construídos dinamicamente: **[nao verificado]**. O censo acima combina ocorrências por `rg`, cargas AST e inspeção dos leitores genéricos `foto_e4b`, `historico` e `passivo_validado`; não inclui comentários nem código de testes como leitores de produção. [nao verificado]

## 17/11 08:13 · sessao 356668db39e3 · - · - · origem=Ronald (tmux)

1. **Bloqueia — ressuscitar mantém VALIDACAO vigente na dobra após apagar a projeção.** `ressuscitar_perguntas` fotografa os campos antes do reset, sem ato limpador: `app/chamados/models.py:1629`, `:1647`. Com `validada_em=T2`, `materializada_em=T2`, par vazio e célula furada, a projeção final fica sem validação, mas a dobra lê `T2`, `estado=VALIDADA`, e a pergunta sai de `q_vivas`. O teste exige expressamente essa divergência: `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:377`, `:381`, `:382`, `:383`, `:384`. O par de resposta desse caso é legal, portanto não cabe na exceção dos pares ilegais: `app/chamados/juizes.py:1534`, `:1541`.

2. **Bloqueia — conservação literal também muda num par legal com brancos.** Cenário: `respondida_em=None`, `resposta_colab=' \t '`, nenhuma linha. Brancos são texto vazio, portanto esse par é legal: `app/chamados/juizes.py:1526`, `:1534`, `:1541`; `app/chamados/services/fio.py:1617`. `_sem_correspondencia` não cria RESPOSTA sem instante; a dobra retorna `''`: `app/chamados/models.py:4173`, `:4225`, `:4236`. Assim `resposta_display` passa a `''`, enquanto antes devolvia os brancos segundo o diff fornecido: `app/chamados/models.py:3753`. O mesmo acontece nas fotos SILENCIO/RETRATACAO: `app/chamados/models.py:2032`, `:2164`. M15 cria precisamente None+brancos e exige SILENCIO vazio: `app/chamados/tests/test_cortes_f2_f9_m15.py:46`, `:50`, `:55`.

3. **Nota — não encontrei contraexemplo entre a face SQL e a dobra com dados frescos.** A equivalência é estrutural: validação vigente significa alguma VALIDACAO real sem limpador posterior por `(ato_em,pk)`, ou VALIDACAO passiva sem linha correspondente e sem limpador estritamente posterior ao carimbo. São os dois ramos SQL: `app/chamados/models.py:3494`, `:3495`, `:3497`, `:3499`, `:3501`, `:3503`; e a ordenação/redução da dobra: `:4217`, `:4221`, `:4224`, `:4230`, `:4232`. Empate real usa pk; passivo empatado vem depois. Passivo casado com VALIDACAO desfeita não é recriado: `:4177`. RESPOSTA foto não limpa validação mesmo sem VALIDACAO foto: `:4201`, `:4221`. Brancos não participam desse predicado de validação: `:3494`, `:4232`.

   Executei em memória as três funções reais extraídas por AST, comparadas com tradução literal do predicado SQL, sobre 50.895 combinações de 0–3 linhas, dois instantes com empates, seis tipos/efeitos e projeções vazias/preenchidas; nenhum contraexemplo. Execução SQL real e equivalência na presença de cache velho: **[nao verificado]**. [nao verificado]

4. **Fila — os testes têm mordidas, mas deixam fora os casos que ameaçam a conservação.** O conjunto SQL não pode passar vazio: exige 13 perguntas e oito validadas; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:165`, `:168`. O selo exige sete leitores reais, descarimbo efetivo e sete resultados verdadeiros: `:303`, `:310`, `:326`, `:331`. O controle exige ressuscitação de exatamente uma pergunta: `:353`. M18 exige exatamente uma chamada observada ao C6: `app/chamados/tests/test_cortes_f2_f9_m18.py:72`. Portanto não identifiquei passagem por universo vazio nessas asserções.

   Entretanto, os auxiliares recarregam a pergunta, ocultando prefetch velho: `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:38`, `:176`. Automática e trilha são testadas somente **antes** da validação: `:188`, `:191`, `:235`, `:239`. O controle de ressuscitação usa justamente o par ilegal instante sem texto: `:352`. A caracterização valida a quebra do item 1: `:381`, `:383`. O registro dos sete não inclui `_ancoradas`, relavra nem as duas fotos: `:277`; M13/M14/M15 cobrem os três primeiros, enquanto cobertura da foto RETRATACAO no diff: **[nao verificado]**.

5. **Nota — outro reset sem ato limpador existe, mas sua guarda exclui validação.** `devolver_instante_sem_texto_l130` fotografa o passivo e zera o par sem REABERTURA/DEVOLUCAO: `app/chamados/services/validacao.py:416`, `:436`, `:439`, `:440`. A guarda recusa `validada_em` preenchido: `:392`. A dobra conserva instante/texto da linha enquanto a projeção fica None/`''`; geralmente a resposta continua falsa porque o texto é vazio após trim: `app/chamados/juizes.py:1552`. Incidência atual: **[nao verificado]**.

6. **Nota — não caracterizei juiz duplicado, relógio novo fora da borda ou inversão nova de trava no trecho F2.** `respondida` continua delegando ao C21 registrado; o que muda é seu insumo: `app/core/juizes.py:306`; `app/chamados/models.py:3730`. A dobra não consulta relógio: `app/chamados/models.py:4217`. A ordem global continua centralizada e a ressuscitação a adquire antes das escritas: `app/chamados/models.py:2759`, `:2796`, `:1628`. A nova regra que RESPOSTA desfaz validação deve, contudo, ser confrontada com cada escritor; ela depende de `ato_motivo` para distinguir efeitos: `app/chamados/models.py:4201`. Isso é mudança semântica verificável, não prova de duplicação do C21.

## 17/11 08:12 · sessao 77b07fcbb0f9 · CODEX-RESERVA-R7 · leitura · origem=Code

**Os mapas precisam de atualização:** HE revogada foi curada; LOTE7 curou dois achados de ausência; os três defeitos centrais de férias permanecem; o fallback do calendário passou a declarar desconhecimento. (`app/ponto/portas/he.py:197`; `app/ponto/services/ausencia.py:1498`; `app/ponto/services/ausencia.py:1756`; `app/ferias/models.py:234`; `app/ferias/services.py:263`; `app/ferias/services.py:652`; `app/colaboradores/services/calendario.py:800`)

Commits abaixo são **atribuições documentais**: a cópia não contém histórico Git; ancestralidade, commit introdutor exato e ocorrências em produção nesta auditoria: **[nao verificado]**. O fluxo documentado entrega a cópia por `git archive`. (`CLAUDE.md:344`) [nao verificado]

Convenção: **PROCEDE** preserva a conclusão, inclusive composição ou desenho; **MUDOU** também identifica premissa do inventário refutada, sem presumir mudança posterior. **—** significa cura/commit não identificado **[nao verificado]**. [nao verificado]

Obras referenciadas nas tabelas:

| Sigla | Obra e commit registrado | [nao verificado]
|---|---|
| H | HE-REVOGADA-NA-PORTA: merge `271ecba2e`, origem `5937d715b`. (`app/docs/RELATO.md:259`) |
| L7 | LOTE7 c7: merge `42fd467a0`, origem `a094967ee`. (`app/docs/RELATO.md:164`) |
| R3 | E3-RESTO-R3/LU3: registro de pouso c17 a partir de `690fde88b`; commit introdutor da alteração específica **[nao verificado]**. (`app/docs/RELATO.md:235`; `app/colaboradores/services/calendario.py:135`) |

**Mapa 07 — HE**

Abreviações: `he` = `app/ponto/portas/he.py`; `hpl` = `app/ponto/services/he_pendente_lavrado.py`; `pexp` = `app/folha/porta_export.py`; `fech` = `app/ponto/services/fechamento.py`. (`he:130`; `hpl:209`; `pexp:800`; `fech:448`)

| item | estado | arquivo:linha HEAD | obra |
|---|---|---|---|
| T1 — período × dia: piso e escritor compartilhado | PROCEDE | `he:577`, `he:595`, `he:206` | — |
| T1b — período contava revogada como decisão | CURADO | Trava linha crua, consulta `_vigente`, só barra vigente: `he:583`, `he:589`, `he:590` | H |
| T2 — “Não” humano sobre revogada dava `no_op` | CURADO | Idempotência compara vigente; gravação limpa revogação: `he:195`, `he:197`, `he:83` | H |
| T3 — lavrador × gestão: soma compartilhada | PROCEDE | `hpl:55`, `hpl:92`, `pexp:194` | — |
| T4 — gestão × trava TXT × motor usam vigentes | PROCEDE | `app/ponto/services/gestao_he.py:87`, `pexp:684`, `app/ponto/motor_calculo_v2.py:1417` | — |
| T5 — callback × porta de revogação | PROCEDE | `app/ponto/management/commands/lavrar_he_pendente.py:56`, `he:481`, `he:494`, `he:500` | H migrou também a guarda do revogador |
| T6 — relavratura após decisão e recuperação pelo cron | MUDOU | Callback captura falha: `fech:1818`, `fech:1824`. Os crons citados lavram retrato/recusam ponta; não recompõem fechamento: `app/config/crons.py:1153`, `:1577`, `:1587` | Recuperação alegada pelo cron não demonstrada; mudança temporal **[nao verificado]** |
| T7 — `_exportada` delega à autoridade de entrega | PROCEDE | `app/ponto/services/autorizacao_he_periodo.py:113`, `:117` | — |
| T8 — TXT da tela × comando | PROCEDE | Tela confere e injeta cálculo lido: `app/folha/views.py:128`, `:209`, `:268`; comando monta gravado e registra sem conferir: `app/folha/management/commands/regerar_txt_dominio.py:85`, `:118` | — |
| E1 — `_gravar` escreve estado | PROCEDE | `he:44`, `he:66`, `he:75`, `he:84` | — |
| E2 — `_marcar_revogada` escreve marca | PROCEDE | `he:375`, `he:401`, `he:403` | — |
| E3 — “selos escrevem direto” | MUDOU | Referências são inspeções AST/texto, não fixtures escritoras: `app/ponto/tests/test_o214_ponta_pequena.py:325`; `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:426`, `:445` | Premissa refutada; alteração posterior **[nao verificado]** |
| E4 — `.delete()` no comando é prosa | PROCEDE | `app/ponto/management/commands/recusar_ponta_pequena.py:219` | — |
| E5 — restore de DiaPago fora da porta | PROCEDE | Delete e `bulk_create`: `bin/restore_relavratura_10_2026.py:85`, `:86` | — |
| E6 — script passa argumento inexistente | PROCEDE | Chamada: `bin/relavrar_o120_col221_09.py:74`; assinatura: `fech:448` | — |
| E7 — trilha best-effort | PROCEDE | `he:100`, `he:414` | — |
| S1 — autorizado → não tem saída | PROCEDE | `he:197`, `he:206` | — |
| S2 — não → autorizado tem saída | PROCEDE | `he:175`, `he:197`, `he:206` | — |
| S3 — revogada sem saída pelo “Não” humano | CURADO | `he:196`, `he:200`; caso estático no teste: `app/ponto/tests/test_he_revogada_na_porta.py:50` | H |
| S4 — revogada barrava período | CURADO | `he:589`, `he:590`; caso estático no teste: `app/ponto/tests/test_he_revogada_na_porta.py:92` | H |

Sítios que o inventário não individualizou:

| leitor/escritor adicional | resultado |
|---|---|
| `_vigente` dentro da porta | Centraliza a consulta também para recusa automática e revogação, além dos dois caminhos defeituosos: `he:148`, `he:347`, `he:481`. |
| Escritor e leitor do retrato HE | Persistem/devolvem fotografia, separada da decisão viva: `hpl:177`, `hpl:182`, `hpl:193`. |
| Escritor final do TXT | `registrar` cria exportação sem consultar trava HE; depende do chamador: `app/folha/models.py:174`, `:182`. |
| Persistência e leitura do carimbo do portão | Flag/lista são persistidas e depois lidas de `MetricaSnapshot`: `pexp:795`, `pexp:807`, `pexp:814`, `pexp:853`. Envelhecimento entre atos é possível pelo mecanismo; ocorrência **[nao verificado]**. |
| Outros consumidores da janela HE | Cadastro-realidade, vigia e pauta usam `competencias_a_lavrar`: `app/escala/services/cadastro_realidade.py:553`; `app/chamados/services/vigia_fio.py:511`; `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:96`. |

Perguntas: **Q1–Q2** foram resolvidas no executável pela H; **Q3** permanece no comando; **Q4** teve comentário da relavratura corrigido, mas o cabeçalho ainda diz “não paga”; **Q5** lê fotografia persistida; **Q6** mantém trilha best-effort; **Q7** implementa corrente + anterior não entregue; **Q8** mantém relógio dentro da porta. (`he:200`; `he:589`; `app/folha/management/commands/regerar_txt_dominio.py:85`; `he:18`; `he:218`; `pexp:807`; `he:414`; `hpl:220`; `hpl:223`; `he:401`)

**Resumo 07:** T1b/T2/S3/S4 curados pela H; emissão alternativa do TXT permanece; E3 e a promessa de recuperação pelo cron precisam de correção no inventário. (`he:197`; `he:590`; `app/folha/management/commands/regerar_txt_dominio.py:85`; `app/ponto/tests/test_o214_ponta_pequena.py:325`; `app/config/crons.py:1153`)

**Mapa 08 — ausência/atestado**

Abreviações: `aus` = `app/ponto/services/ausencia.py`; `pv` = `app/ponto/views.py`; `mod` = `app/ponto/models.py`. (`aus:955`; `pv:2745`; `mod:660`)

| item | estado | arquivo:linha HEAD | obra |
|---|---|---|---|
| L1 — matriz única e chamadores | PROCEDE | Matriz/porta: `aus:914`, `aus:955`; férias continuam recusadas nessa porta: `aus:968` | L7 acrescentou acordamento: `aus:989` |
| Nascimento explícito × reabertura | PROCEDE | Reabrir não recebe `status`: `aus:433`; nascimento usa pedido: `aus:486`; reabertura deriva: `aus:1298` | — |
| Pré-veto PWA × criação | PROCEDE | Veto singular PWA: `pv:2670`; precedência na porta: `aus:465`; API chama diretamente: `app/api/views.py:1812`, `:1818` | — |
| Efeito `None`: folha × espelho | PROCEDE | Retorno possível: `app/ponto/catalogo/ausencias.py:295`; folha suprime: `app/ponto/services/fechamento.py:700`; coletor só suprime `SUPRIME`: `app/ponto/services/efeito_ausencia.py:61` | EFEITO-NONE-UM-LEITOR ainda em fila: `app/docs/BACKLOG.md:1907` |
| Documento: matriz × gate × vencimento | PROCEDE | `aus:915`, `aus:773`, `aus:1177`; gate admite confirmação explícita: `pv:2754` | — |
| Contradição documento × rejulgamento por tipo | PROCEDE | Perguntas distintas: `aus:757`, `aus:689`; edição rejulga quando muda tipo: `aus:674` | — |
| Fechar chamado da ausência decidida | MUDOU | Leitor permanece: `aus:851`, `aus:863`; decisão agora atomic, transição/carimbos e acordamento: `aus:1079`, `aus:1088`, `aus:1123` | L7 |
| Docstring `retornar_mesmo_tipo` | PROCEDE | Texto antigo: `aus:295`; rejeitada reabre antes: `aus:399`, `aus:433`; retorno vem depois: `aus:436` | — |
| L2 — criar: save e trilha | MUDOU | Save em atomic: `aus:561`, `aus:562`; evento: `aus:585`; novo acordamento: `aus:597` | L7 |
| Editar período/tipo fora de `mudar_estado` | MUDOU | Escrita permanece: `aus:648`, `aus:659`; invalidação agora compara insumo antes/depois: `aus:669`; rejulgamento: `aus:674` | L7 para propagação; escritor separado permanece |
| Rejeitar férias por update sem trilha/signal | CURADO | Trava: `aus:1488`; save por linha: `aus:1498`; trilha obrigatória/rollback: `aus:1499`, `aus:1506`; acordamento: `aus:1508` | L7 |
| Apagar/espelhar férias | MUDOU | Deletes: `aus:1374`, `aus:1401`, `aus:1461`; novos acordamentos: `aus:1376`, `aus:1403`, `aus:1463`; existente rejeitada ainda preservada: `aus:1385` | L7 |
| `rederivar_fim` sem invalidar fechamento | CURADO | Captura antes, salva/refresca e acorda projeção: `aus:1751`, `aus:1752`, `aus:1756` | L7; descrição antiga ainda em `app/docs/BACKLOG.md:2207` |
| Migration 0057 forward | PROCEDE | Lista congelada, update e trilha: `app/ponto/migrations/0057_a4_backfill_pendente.py:30`, `:43`, `:44` | — |
| Migration 0057 reverse | PROCEDE | Escreve `pendente`, sem trilha nesse corpo: `app/ponto/migrations/0057_a4_backfill_pendente.py:58`, `:65` | — |
| Admin Django, antes não reaberto | PROCEDE | `AusenciaAdmin(SoLeitura)`: `app/ponto/admin.py:38`; bloqueios: `app/core/admin.py:59`, `:62`, `:65` | O167 declarado: `app/ponto/admin.py:39` |
| L3 — quatro estados atuais sem terminais | PROCEDE | `aus:914`, `aus:922` | — |
| Rejeitada → aprovada sem chamador identificado | PROCEDE | Seta: `aus:922`; aprovadores selecionam abertas: `pv:2369`, `pv:2752`, `pv:2764`; reabertura deriva aguardando: `aus:1298` | Chamador externo/dinâmico **[nao verificado]** |
| `pendente` legado sem saída runtime | PROCEDE | Reverse escreve: `app/ponto/migrations/0057_a4_backfill_pendente.py:65`; filas/matriz excluem: `aus:912`, `aus:914` | Existência de linhas **[nao verificado]** |
| Carimbos antigos após reabrir | PROCEDE | Reabertura não limpa: `aus:1298`, `aus:1320`; transição não limpa aprovador/data: `aus:975`, `aus:986` | — |

Sítios que o inventário não individualizou:

| leitor/escritor adicional | resultado |
|---|---|
| API de chamados lê status legado | “Anexar comprovante” exige `status == 'pendente'`: `app/api/views.py:1061`. A tarefa usa estados atuais: `app/chamados/services/tarefa_do_dia.py:312`. |
| `Ausencia.save` deriva campos | Escreve `aprovada_em` inclusive para rejeitada, minutos e `data_fim`: `mod:882`, `mod:887`, `mod:912`, `mod:917`. |
| Copiloto pede aprovação explícita | Também alcança o defeito da reabertura: `app/ponto/services/checar_ausencia.py:39`, `:47`; `aus:433`. |
| Veto na edição | Edição usa sobreposição singular sem `linha_que_veta`, diferentemente da criação: `aus:637`, `aus:465`; precedência: `app/ponto/precedencia.py:136`. |
| Leitor/invalidador da L7 | Lê insumo pelo predicado da folha e invalida dias antes/depois: `aus:138`, `aus:192`, `aus:195`. |
| Signals de save/delete | Rejulgam células e acordam consumidores: `app/ponto/signals.py:143`, `:150`, `:256`, `:269`, `:272`. |
| Leitores dos carimbos antigos | Histórico usa aprovador/data como quem decidiu/quando: `app/ponto/services/dia_decidido.py:514`, `:520`, `:522`; vigia lê aprovação sem documento/aprovador: `app/ponto/services/vigia_ausencia.py:115`, `:120`. |
| Outros leitores de estado/período | TXT: `app/folha/export.py:87`; relatório: `app/relatorios/views.py:197`, `:205`; score usa não rejeitada: `app/inteligencia/management/commands/calcular_scores.py:12`, `:14`. |
| Residual documentado da L7 | Há fila para erro de banco engolido dentro do atomic e relógio da linha aberta: `app/docs/BACKLOG.md:858`, `:859`. Reprodução **[nao verificado]**. |

Perguntas: **Q1–Q4, Q6–Q7 e Q9** mantêm os mecanismos apontados; decisão normativa adicional **[nao verificado]**. **Q5** ganhou porta de férias com trilha/signal próprios; **Q8** ganhou acordador nos escritores listados. (`aus:433`; `pv:2670`; `app/ponto/services/fechamento.py:700`; `aus:922`; `aus:773`; `aus:1285`; `app/ponto/migrations/0057_a4_backfill_pendente.py:65`; `aus:1498`; `aus:1506`; `aus:669`; `aus:1756`)

**Resumo 08:** os três defeitos centrais permanecem; L7 curou rejeição de férias sem trilha/signal e rederivação de fim sem invalidação, além de mudar a propagação das escritas. (`aus:433`; `pv:2670`; `app/ponto/services/fechamento.py:700`; `aus:1498`; `aus:1756`)

**Mapa 09 — férias**

Abreviações: `fm` = `app/ferias/models.py`; `fs` = `app/ferias/services.py`; `fv` = `app/ferias/views.py`. (`fm:65`; `fs:644`; `fv:245`)

| item | estado | arquivo:linha HEAD | obra |
|---|---|---|---|
| L1.1 — P1–P3, nascimento do PA | PROCEDE | `fv:178`, `fs:416`, `fs:425`, `fm:204` | — |
| L1.2 — P4–P16, alterações do PA | PROCEDE | Juiz/setter: `fm:65`, `fm:207`; persistências: `fs:199`, `fs:236`, `fs:399`, `fs:434`, `fs:855` | — |
| P-a — pode agendar: status × porta | PROCEDE | Property exige adquirido: `fm:234`; porta normal não exige: `fv:245`, `fs:578`; solicitação exige: `fs:764` | Residual registrado: `app/docs/RELATO.md:847` |
| P-b — vencendo: data × gravado | PROCEDE | Juiz: `fm:260`; chip/listas: `fv:36`, `fv:38`, `fv:64`; serviço: `fs:263`; mensageria: `app/api/views_mensageria.py:905` | RC1-2 registrada, sem construtor: `app/docs/BACKLOG.md:759` |
| L1.3 — A1–A3, nascimentos do AG | PROCEDE | `fs:273`, `fs:504`, `fs:556`, `fs:748`, `fm:357` | — |
| A-AP — duplicata na aprovação | PROCEDE | Normal: `fs:569`; aprovação faz `full_clean`: `fs:785`; sobreposição: `fm:396`, `fm:433` | Divergência residual de efeito **[nao verificado]** |
| L1.4 — janela/status; comentário de concluído | PROCEDE | Juiz: `fm:276`; edição/cron: `fs:708`, `fs:721`, `fs:953`; comentário inexato: `fm:304` | — |
| L1.5 — cancelamento DP de férias iniciadas | PROCEDE | Botão por data: `fm:340`, `app/templates/ferias/_secao_colab.html:159`; botão por gravado: `app/templates/ferias/saldo.html:70`; porta aceita: `fs:652` | Caso mantido em fila/lei: `app/docs/RELATO.md:842` |
| L1.5b — regra admin repetida | PROCEDE | `fm:348`, `fm:308`, `fs:678` | FERIAS-2X-FILA: `app/docs/BACKLOG.md:1994` |
| L1.6 — A12, correção excepcional de concluído | MUDOU | Permissão preservada: `fv:438`, `fs:678`; agora traduz falha da trilha: `fv:449`, `fv:452` | L7 |
| L1.7 — C1–C2, envio/reenvio | PROCEDE | Filtro externo: `fv:945`; escritor só veta cancelado e zera carimbo: `fs:887`, `fs:889` | Corrida real **[nao verificado]** |
| L1.8 — C3–C4, resposta do colaborador | PROCEDE | Filtro externo: `fv:994`; recusa não limpa carimbo: `fs:907`, `fs:908`; confirmação carimba: `fs:911` | Corrida real **[nao verificado]** |
| L1.9 — S1, aprovação sob trava | PROCEDE | `fv:800`, `fv:801`, `fv:817`; PA sob trava: `fs:764` | HJ-090; absorção registrada: `app/docs/BACKLOG.md:1965` |
| L1.10 — S2, rejeição | PROCEDE | `fv:801`, `fv:825`, `fs:795` | — |
| L2 — portas por arquivo e selo | PROCEDE | `app/core/portas.py:488`; selo reaberto: `app/ponto/tests/test_chokepoint_ausencia_gate.py:25`, `:53` | Execução do selo **[nao verificado]** |
| Admin sem PA/AG/SOL | PROCEDE | Registra somente Afastamento: `app/ferias/admin.py:2`, `:4` | — |
| Setter em memória | PROCEDE | Setter: `fm:215`; persistências: `fs:199`, `fs:587`, `fs:935` | — |
| “17 funções persistindo” | MUDOU | Censo estático: 18 funções com save/create/get_or_create, mais uma com delete. A amostra omitia `fs:316`, `fs:412`; exclusão em `fs:876`, `fs:877` | Contagem refutada; novidade temporal **[nao verificado]** |
| Limites do censo | PROCEDE | Não atribuíveis/exclusões: `app/core/censo_escritas.py:43`, `:49`, `:54` | — |
| Trilha admin/concluída × J | MUDOU | Novas capturas: `fv:395`, `fv:449`; logs de AG ainda após a porta: `fv:410`, `fv:459`; J dentro do atomic: `fv:543` | L7 não curou a separação do log de AG |
| Relógio dentro da porta | PROCEDE | `fs:654`, `fs:681`, `fs:686`, `fs:934`, `fm:94`, `fv:29`, `fv:117` | Residual registrado: `app/docs/RELATO.md:847` |
| Views sem `login_required` | PROCEDE | Controle por ação: `fv:435`, `fv:499`; recusa anônimo: `app/colaboradores/mixins.py:326` | Continua refutada a ausência de controle apenas pelo decorator |
| L3 — AG.cancelado terminal | PROCEDE | Catálogo/guardas: `fm:307`, `fs:678`, `fs:954`; exclusão do registro: `fs:875` | Por desenho no código |
| AG.concluído não terminal | PROCEDE | `fs:721`, `fs:954`, `fv:451`; comentário contraditório: `fm:304` | — |
| SOL.pendente sem nascente produtiva identificada | PROCEDE | Default: `fm:481`; leitores: `fv:41`, `fv:785`; escritores de avaliação: `fs:756`, `fs:797` | Canal externo/linhas existentes **[nao verificado]** |
| SOL.aprovada/rejeitada terminais pela nascente | PROCEDE | Guarda pendente: `fv:802`; destinos: `fs:756`, `fs:797` | Por desenho no código |
| CONF.confirmado terminal na view, não no escritor | PROCEDE | `fv:945`, `fs:887`, `fs:888` | — |
| Batida em férias: auxiliar por datas | PROCEDE | `app/ponto/utils.py:87`, `:89`; detector reaberto: `app/chamados/detector_anomalias.py:115` | Conclusão global sobre todas as guardas **[nao verificado]** |
| Colaborador.situacao × férias | PROCEDE | Leitura: `app/api/views.py:403`; campos do espelho: `fm:442` | Escritor de férias alterando situação não identificado **[nao verificado]** |

Sítios que o inventário não individualizou:

| leitor/escritor adicional | resultado |
|---|---|
| Relatório/PDF de férias | Também filtra vencido pelo gravado e vencendo por adquirido + literal 60: `app/relatorios/services.py:286`, `:288`. |
| Alertas de inteligência | Vencidas pelo gravado; vencendo pelo juiz: `app/inteligencia/management/commands/gerar_alertas.py:86`, `:100`, `:103`. |
| API do colaborador | Expõe status gravado do AG: `app/api/views.py:2244`, `:2254`. |
| Cron de aviso | Seleciona em_curso/adquirido e sincroniza pela porta: `app/ferias/management/commands/alertar_ferias_vencendo.py:22`, `:32`. |
| Preview/intake | Pergunta ao validador da view, propagando P-a: `fs:346`, `fs:347`; contraste com `fm:234`. |
| Catálogo das frações no aviso | Continua copiado: `app/ferias/situacao.py:119`, `fm:364`; família documentada: `app/docs/BACKLOG.md:747`. |
| Persistências omitidas da amostra L2 | Intake grava dias vendidos/status e art.130 grava direito: `fs:316`, `fs:412`; já mencionados em L1, mas ausentes da amostra de persistências. |
| Template de confirmação | Lê diretamente confirmação para badge/envio: `app/templates/ferias/_secao_colab.html:122`, `:124`. |

Perguntas: **Q1–Q3** preservam as divergências de cancelamento, agendamento e vencimento; **Q4–Q5** preservam carimbo e guardas externas; **Q6** segue sem nascente produtiva identificada; **Q7** mantém relógios internos; **Q8** mantém comentário inexato. Sentido normativo dos campos e ocorrências reais: **[nao verificado]**. (`fs:652`; `fm:234`; `fs:263`; `fs:908`; `fv:945`; `fv:994`; `fv:785`; `fs:654`; `fm:304`)

**Resumo 09:** os três defeitos centrais permanecem; L7 mudou a apresentação da falha de trilha nas views administrativas; o censo de “17 funções” estava incompleto. (`fm:234`; `fs:263`; `fs:652`; `fv:395`; `fv:449`; `fs:316`; `fs:412`; `fs:876`)

**Mapa 10 — feriado/calendário**

Abreviações: `vc` = `app/core/views_config.py`; `fer` = `app/core/feriados.py`; `cal` = `app/colaboradores/services/calendario.py`; `m13` = `app/core/migrations/0013_remove_feriado_recorrente_feriado_fonte.py`. (`vc:41`; `fer:57`; `cal:97`; `m13:20`)

| item | estado | arquivo:linha HEAD | obra |
|---|---|---|---|
| T1 — nacional manual × importação | PROCEDE | Form aceita abrangência/fonte: `vc:50`, `vc:55`, `vc:83`; importação exige allowlist: `vc:113`, `vc:140` | — |
| T1b — unicidade nacional × UF/município | PROCEDE | Form preserva campos: `vc:56`, `vc:85`; importação usa vazios: `vc:145`; chave: `app/core/mod

[truncado em 20000 de 26104 caracteres]

## 17/11 08:12 · sessao e339e34d0a75 · - · - · origem=Ronald (tmux)

Inventário de escritores por leitura estática; execução e incidência em produção [nao verificado].

1. **Bloqueia — RESPOSTA automática conserva validação que a F2 remove.** `responder_automaticamente` só recusa texto já existente em `app/chamados/services/disputa_emissao.py:1663`, escreve resposta/instante em `:1674`, salva só esses campos em `:1677` e registra RESPOSTA comum em `:1678`. Não há guarda de `validada_em`. O chamador declara explicitamente que não filtra validação em `app/api/foto_recebida.py:32`, responde em `:48` e só depois chama o dono em `:51`; este retorna `ja_validada` sem alterar carimbo em `app/chamados/services/validacao.py:685`. Contraexemplo: projeção legal `(validada_em=T1, respondida_em=None, resposta_colab='')`; resposta automática em `T2>T1`. Projeção termina `(T1,T2,texto)`, mas a leitura termina `(None,T2,texto)`, porque a foto VALIDACAO preservada em `disputa_emissao.py:1673` vem antes da RESPOSTA que desfaz em `app/chamados/models.py:4201` e `:4232`. O teste cobre automática **antes** de validar, não automática sobre validada (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:184`, `:191`).

2. **Bloqueia para coerência do escritor; incidência fora dos ilegais [nao verificado] — trilha também conserva validação.** A guarda só pergunta instante/texto em `app/chamados/services/disputa_emissao.py:1692`; escreve só instante em `:1696`, salva em `:1698` e registra RESPOSTA comum em `:1699`. Se houver validação em T1 e a nota testemunhar resposta T2>T1, a projeção conserva T1 e a leitura a limpa (`app/chamados/models.py:4232`). A entrada desse escritor exige texto sem instante, portanto pertence ao par ilegal excluído do corte (`disputa_emissao.py:1692`). O chamador do comando não guarda validação (`app/chamados/management/commands/curar_perguntas_em_limbo.py:55`, `:67`).

3. **Escritores de V/R/T e respectivos atos:**

| Escritor | Campos escritos | Linha gravada no mesmo ato | Coerência |
|---|---|---|---|
| Nascimento `app/chamados/models.py:3149`, `:3152` | Campos recebidos por `**campos` | Fotos RESPOSTA/VALIDACAO por `:3154`, corpo em `:4173`/`:4177` | Fotos mantêm projeção; futuros atos podem removê-la (`:4224`, `:4232`). |
| Resposta humana `app/chamados/services/disputa_emissao.py:1481` | Texto e instante; V=None em `:1494` | Fotos antigas em `:1480`, RESPOSTA em `:1525` | Coerente na ordem cronológica usual: RESPOSTA desfaz V (`models.py:4201`). |
| Automática `disputa_emissao.py:1674` | Texto/instante; não altera V | Fotos + RESPOSTA em `:1678` | Contraexemplo no achado 1. |
| Pela trilha `disputa_emissao.py:1696` | Instante; não altera V/T | RESPOSTA em `:1699` | Contraexemplo no achado 2. |
| Validação `app/chamados/services/validacao.py:168` | V=agora | Fotos + VALIDACAO em `:173` | VALIDACAO atribui V em `models.py:4230`. |
| Devolução da recusa `validacao.py:273` | V=None | Fotos + DEVOLUCAO em `:279` | DEVOLUCAO limpa V (`models.py:4189`, `:4232`). |
| BUG-PAR-103 `validacao.py:349` | V=None e descarimbo em `:346` | Fotos + DEVOLUCAO em `:354` | Limpa V; preserva par (`models.py:4232`, `:4234`). |
| Reabrir/recusar `disputa_emissao.py:1609`, `:1648` | V=None, R=None, T='' | REABERTURA/RECUSA com foto em `:1631`; guardas do universo em `:1645` | A dobra limpa os três (`models.py:4189`, `:4190`, `:4232`, `:4234`). |
| Ressuscitar `models.py:1647` | V=None, R=None, T='' | Só fotos anteriores em `:1629`; nenhuma limpeza | Não conserva validação; teste caracteriza expressamente (`test_e4c_f2_leitura_do_ato.py:369`). | [nao verificado]
| Estoque I-sem-T `validacao.py:440` | R=None, T='' | Só fotos em `:439`; nenhuma limpeza | Leitura conserva instante anterior, embora o juiz dê falso pelo texto vazio (`models.py:4228`). Entrada é ilegal (`validacao.py:408`). |
| Carimbar `models.py:3271`, `:3274` | R=quando e/ou V=quando se faltantes e texto presente | Nenhuma linha própria em `:3291`; leitura usa passivo `:4221` | Funciona enquanto não houver correspondência histórica conflitante no mesmo instante (`:4173`, `:4177`). |
| Simulação de ressuscitação `models.py:1569` | V=None, R=None, T='' em cópia | Nenhuma | Só memória; cópia em `:1556`. |
| Desfazer carimbo recusado `models.py:3302` | R/V retornam aos valores anteriores | Nenhuma | Só memória; chamada em `:3280`/`:3283`/`:3294`. |

4. **Escritores físicos de materializada/via.** `_escrever_carimbo` atribui `materializada_em` e `via_resolucao` em `app/chamados/models.py:3238` e `:3240`; persistências pelas portas `carimbar` em `:3291`, `carimbar_identidade` em `:3329`, `descarimbar` em `:3351`; lote grava `materializada_em=None, via_resolucao=''` em `:3420`. Além disso, `encerrar_por_fato_no_espelho` muda só via em `:3431`/`:3432`, e `relavrar` muda só via em `app/chamados/services/relavra_via.py:125`/`:127`. Nascimento recebe quaisquer desses campos por `**campos` (`models.py:3149`). A F2 não dobra materializada/via (`models.py:4236`).

5. **Chamadores das portas do carimbo**, para inventário completo dos responsáveis sem duplicar atribuições:

- `app/chamados/models.py:1647`, `:2026`, `:2167`, `:3203`, `:3443`, `:3454`, `:3471`, `:3478`.
- `app/chamados/reconciliador.py:407`, `:597`, `:1150`.
- `app/chamados/services/relavra_via.py:246`.
- `app/chamados/services/fio.py:1410`.
- `app/chamados/services/validacao.py:346`, `:440`, `:520`, `:560`.
- `app/chamados/services/competencia_trancada.py:279`.
- `app/chamados/services/materializacao.py:1046`, `:1461`, `:1686`, `:1968`, `:2167`.
- `app/chamados/services/disputa_emissao.py:1499`, `:1648`, `:2008`, `:2082`.
- `app/ponto/services/veredito_celula.py:190`.
- Comandos: `app/chamados/management/commands/reabrir_orfas_enterradas.py:157`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`.

6. **Escritores de DecisaoPergunta.** Escrita física única localizada é `app/chamados/models.py:4249`: `bulk_create([cls(**ln)...])`; recebe ato/instante/texto/motivo dos produtores seguintes. A linha existente recusa `save` em `:4136`; lote recusa update/delete/bulk_update em `:4062`. PK é gerado na inserção [nao verificado: mecanismo do Django/banco não foi executado]; nenhum produtor listado atribui PK explicitamente (`:4249`).

| Produtor | Atos |
|---|---|
| `app/chamados/models.py:1629` | Fotos RESPOSTA/VALIDACAO antes da ressuscitação |
| `models.py:2030` | SILENCIO do fechamento |
| `models.py:2162` | RETRATACAO |
/page| `models.py:3154` | Fotos do nascimento |
| `models.py:4175`, `:4179` | Corpo que fabrica fotos com motivo especial |
| `app/chamados/services/disputa_emissao.py:1525` | Fotos + RESPOSTA humana |
| `disputa_emissao.py:1631` | RECUSA/REABERTURA |
| `disputa_emissao.py:1678` | Fotos + RESPOSTA automática |
| `disputa_emissao.py:1699` | RESPOSTA pela trilha |
| `app/chamados/services/validacao.py:173` | Fotos + VALIDACAO |
| `validacao.py:279`, `:354` | Fotos + DEVOLUCAO |
| `validacao.py:439` | Fotos do estoque I-sem-T |
| `app/chamados/services/materializacao.py:1048` | ABSORCAO |
| `app/ponto/services/veredito_celula.py:195` | SILENCIO |
| `app/chamados/management/commands/backfill_fio_morto_e4b.py:141` | Fotos |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:324` | SILENCIO |

A migração também muda a representação histórica: renomeia texto/instante/motivo e introduz `ato='reabertura'` nas linhas anteriores, em `app/chamados/migrations/0051_decisao_pergunta.py:42` e `:50`. Não introduz escritor novo no diff F2 apresentado [nao verificado: ausência de metadados git na cópia].

## 17/11 08:11 · sessao 9ff7a0da1629 · CODEX-RESERVA-R3 · leitura · origem=Code

**Os 12 permanecem “sem causa” fechada.** O resumo R3 informa o sintoma, mas não traz as batidas com tipos, papéis, carimbos e `data_turno` necessários para identificar o ramo executado em cada caso. [não verificado]

O caminho do código a discriminar é:

1. `turnos_do_colab` carrega os papéis da ata, segmenta pelo vínculo do dia e chama `parear_turnos` sobre cópias das batidas. (`app/ponto/turnos.py:1944`, `:1965-2000`)
2. O pareador prioriza o papel da ata; a pausa depende dos carimbos e das bordas. Há reconhecimento por duração mesmo sem marcos, e pares declarados/ata podem passar apesar dos tipos invertidos. (`app/ponto/turnos.py:392-418`, `:811-883`, `:1310-1326`, `:1382`)
3. `_data_do_turno` decide a atribuição à véspera; o realizado seleciona por `data_turno` e desconta pausa reconhecida ou janela declarada. (`app/ponto/turnos.py:285-309`, `:719-771`)
4. A tela monta os cards pelos períodos do motor, enquanto `minutos_realizados` vem do realizado do juiz, com intervalo do vínculo escolhido. (`app/ponto/services/espelho.py:286-294`, `:450-460`, `:675`, `:783`)

Na tabela, **“?” significa hipótese de triagem**, sem dono comprovado pela L-099. (`app/docs/LEIS.md:122`)

| colNNN | Dia | Causa | Arquivo:linha — regra a discriminar | Dono provável | Precisa sonda |
|---|---|---|---|---|---|
| col369 | 2026-10-12 | **sem causa** — falta identificar os carimbos/papéis que recusaram o vão e o intervalo efetivo. [não verificado] | `app/ponto/turnos.py:811-883`, `:756-766` | ESTRUTURA? [não verificado] | S |
| col864 | 2026-10-12 | **sem causa** — folga/feriado não demonstra, sozinho, ausência de reconhecimento por duração. [não verificado] | `app/ponto/turnos.py:392-418`, `:1217-1221`, `:756-766` | ESTRUTURA? [não verificado] | S |
| col114 | 2026-11-08 | **sem causa** — o vão relatado não revela se ambas as pontas eram interiores e reconhecidas pela autoridade. [não verificado] | `app/ponto/turnos.py:811-883`, `:545-546` | ESTRUTURA? [não verificado] | S |
| col114 | 2026-11-15 | **sem causa** — faltam papéis, marcas e pausa efetivamente declarada pelo dia. [não verificado] | `app/ponto/turnos.py:811-883`; `app/ponto/services/espelho.py:286-294` | ESTRUTURA? [não verificado] | S |
| col723 | 2026-10-08 | **sem causa** — falta distinguir recusa do par, reconhecimento de outro par e desconto da janela declarada. [não verificado] | `app/ponto/turnos.py:811-883`, `:756-766` | ESTRUTURA? [não verificado] | S |
| col746 | 2026-09-24 | **sem causa** — faltam bordas, carimbos e intervalo efetivo para explicar o desconto do vão relatado. [não verificado] | `app/ponto/turnos.py:811-883`, `:756-766` | ESTRUTURA? [não verificado] | S |
| col876 | 2026-09-21 | **sem causa** — “fora do turno” não identifica tipo, papel, motivo da separação nem dia atribuído à ponta. [não verificado] | `app/ponto/turnos.py:1382`, `:1402-1419`, `:285-309` | BATIDA? [não verificado] | S |
| col130 | 2026-10-29 | **sem causa** — faltam os turnos completos para distinguir ponta solta de segmento atribuído a outro turno/dia. [não verificado] | `app/ponto/turnos.py:285-309`, `:1417-1419`, `:719-721` | BATIDA? [não verificado] | S |
| col220 | 2026-11-10 | **sem causa** — falta saber qual decisão de papel, absorção ou fechamento deixou a batida fora. [não verificado] | `app/ponto/turnos.py:1310-1326`, `:1371-1382`, `:1430-1457` | BATIDA? [não verificado] | S |
| col868 | 2026-09-22 | **sem causa** — escala diurna diante de batidas noturnas sustenta candidata CADASTRO; faltam vínculo escolhido, geradora/DNA e papéis para comprová-la. [não verificado] | `app/ponto/turnos.py:1695-1700`, `:1965-2000`; `app/docs/LEIS.md:122` | CADASTRO? [não verificado] | S |
| col516 | 2026-10-23 | **sem causa** — folga com realizado e nenhum turno E6 não distingue atribuição do dia, pareamento e fonte do número. [não verificado] | `app/ponto/turnos.py:719-771`; `app/ponto/services/espelho.py:450-460`; `app/ponto/management/commands/e6_oraculo.py:263-269` | Indeterminado [não verificado] | S |
| col884 | 2026-11-08 | **sem causa** — inversão de tipos não prova recusa: pares declarados/ata/mistos podem passar. Atestado rejeitado não explica sozinho o realizado. [não verificado] | `app/ponto/turnos.py:811-815`, `:875-883`, `:2528-2531` | BATIDA? [não verificado] | S |

**Total por causa:** sem causa = **12**; causas nomeadas confirmadas por caso = **0**. Consolidação do dado fornecido, sem reconsulta de produção. [não verificado]

Duas ressalvas da cópia: `turnos.py:502-567` contém leitores de intervalo; `parear_turnos` começa em `:1192`. O trecho `:2798-2814` expande datas de ausência; o filtro de rejeitada está em `:2528-2531`. Além disso, realizado não comprova pagamento: a linha financeira da tela imprime `rubricas`. (`app/templates/ponto/espelho.html:268-286`)

A sonda única abaixo cobre os 12. Usa as funções reais, captura as **cópias carimbadas** e informa a fonte do número. (`app/ponto/services/espelho.py:1194`; `app/ponto/turnos.py:1984-2000`; `app/ponto/management/commands/e6_oraculo.py:263-269`)

Execute em contexto Django **já inicializado**, no tenant correto, com `PK_POR_COL` confirmado. A equivalência entre o rótulo raspado `colNNN` e o PK não está demonstrada no dado; por isso o script não inventa essa correspondência. Execução e resultado em produção: [não verificado].

```python
# Contexto Django JA inicializado, tenant correto, sem redirecionar para arquivo.
# Definir previamente PK_POR_COL: mapa confirmado dos rotulos abaixo para PKs.
import json
import logging
from contextlib import ExitStack
from datetime import date, datetime, time
from decimal import Decimal

from django.apps import apps
from django.db import connections, transaction, DatabaseError
from django.db.models import Q
from django.utils import timezone

if not apps.ready:
    raise RuntimeError("Requer contexto Django ja inicializado")

CASOS = [
    ("col369", "2026-10-12", 10), ("col864", "2026-10-12", 10),
    ("col114", "2026-11-08", 11), ("col114", "2026-11-15", 11),
    ("col723", "2026-10-08", 10), ("col746", "2026-09-24", 10),
    ("col876", "2026-09-21", 10), ("col130", "2026-10-29", 11),
    ("col220", "2026-11-10", 11), ("col868", "2026-09-22", 10),
    ("col516", "2026-10-23", 11), ("col884", "2026-11-08", 11),
]
mapa = globals().get("PK_POR_COL", {})
if any(col not in mapa for col, _, _ in CASOS):
    raise RuntimeError("Falta PK_POR_COL confirmado para todos os rotulos")

schema = getattr(connections["default"], "schema_name", None)
if schema == "public":
    raise RuntimeError("Selecionar o tenant correto antes da sonda")

def serializar(x):
    if isinstance(x, (date, datetime, time)):
        return x.isoformat()
    if isinstance(x, Decimal):
        return str(x)
    # Impede serializacao acidental de modelo por __str__.
    raise TypeError("Objeto nao permitido: " + type(x).__name__)

def emitir(tipo, **dados):
    print(json.dumps(dict(tipo=tipo, **dados), default=serializar,
                     ensure_ascii=False))

def campos(obj, nomes):
    return {k: getattr(obj, k) for k in nomes.split()}

falhas_sql = []

def vigiar(execute, sql, params, many, context):
    try:
        return execute(sql, params, many, context)
    except DatabaseError as exc:
        # Falha capturada internamente pelo espelho tambem impede FIM_OK.
        falhas_sql.append(dict(alias=context["connection"].alias,
                               classe=type(exc).__name__))
        raise

BATIDA = (
    "pk timestamp tipo sequencia is_intervalo origem "
    "aprovada retratada_em escala_id"
)
FLAGS = "_papel_ata _intra_dur _intra_ata _intra_declarado _eco_flush"
LINHA = (
    "minutos_realizados pago_h rubricas realizado_sem_turno realizado_aberto "
    "realizado_longo realizado_janela_descontada folga feriado veredito_dia"
)
GRADE = "tipo hora hora_marco tipo_real missing delta status"
EC = "pk tipo_escala_id data_inicio data_fim ativa trabalha_em_feriado"
TE = (
    "pk tipo_ciclo intervalo_modo hora_inicio hora_fim "
    "hora_inicio_intervalo hora_fim_intervalo intervalo_duracao_min"
)

nivel = logging.root.manager.disable
logging.disable(logging.CRITICAL)
try:
    from colaboradores.models import Colaborador
    from escala.alimentacao import (
        celulas_do_periodo, escalas_do_periodo, vinculo_do_dia,
    )
    from ponto.models import Batida, Ausencia
    from ponto.janelas import janela_fechamento
    from ponto.services.espelho import (
        espelho_do_colab, _realizado_do_dia_tela,
    )
    from ponto.management.commands.e6_oraculo import (
        InsumosDoDono, minutos_do_espelho,
    )
    from ponto.juiz_batida import periodos_do_dia
    from ponto.turnos import (
        turnos_do_colab, janela_com_pad, papel_por_minuto_da_ata,
        dia_das_batidas, _papel_da_ata, _pares_marcados, _fim_fechado,
        intervalos_do_turno, pausas_abaixo_do_piso,
    )

    agora = timezone.now()
    hoje = timezone.localdate(agora)

    with ExitStack() as stack:
        aliases = list(connections)
        for alias in aliases:
            conn = connections[alias]
            if conn.vendor != "postgresql" or conn.in_atomic_block:
                raise RuntimeError(
                    alias + ": requer PostgreSQL sem transacao aberta"
                )
            stack.enter_context(transaction.atomic(using=alias))
            with conn.cursor() as cur:
                cur.execute(
                    "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
                )
                cur.execute("SHOW transaction_read_only")
                if cur.fetchone()[0] != "on":
                    raise RuntimeError(alias + ": READ ONLY nao ativado")
            stack.enter_context(conn.execute_wrapper(vigiar))

        emitir("contexto", agora=agora, schema=schema,
               fotografia="estado atual; nao restaura o snapshot R3")
        cache = {}

        for col, iso, mes in CASOS:
            d = date.fromisoformat(iso)
            chave = (col, mes, d.year)

            if chave not in cache:
                c = Colaborador.objects.get(pk=mapa[col])
                ini, fim = janela_fechamento(mes, d.year, c.empresa)

                # Mesma entrada publica usada pelo E6:
                # app/ponto/management/commands/e6_oraculo.py:316
                esp = espelho_do_colab(c, hoje, ini, fim, agora=agora)
                vi, vf = esp["data_ini"], esp["data_fim"]
                lo, hi = janela_com_pad(vi, vf)

                cels = celulas_do_periodo([c.pk], lo, hi)
                por_data = {dd: ce for (_, dd), ce in cels.items()}
                tela_cels = {
                    k: ce for k, ce in cels.items() if vi <= k[1] <= vf
                }
                ecs = sorted(escalas_do_periodo(
                    c, vi, vf,
                    mais=[ce.escala_geradora_id
                          for ce in tela_cels.values()],
                ), key=lambda e: e.data_inicio)

                bs = list(Batida.objects.filter(
                    colaborador=c, timestamp__date__range=(lo, hi),
                ).order_by("timestamp", "pk"))

                papel = papel_por_minuto_da_ata(
                    c, lo, hi, celulas=por_data,
                    batidas=[b for b in bs if b.retratada_em is None],
                )
                ts = turnos_do_colab(c, vi, vf, celulas=por_data)
                dias_b = dia_das_batidas(c, vi, vf, turnos=ts)
                ins = InsumosDoDono(
                    c, ini, fim, hoje, espelho=esp, agora=agora,
                )
                cache[chave] = (
                    c, ini, fim, esp, cels, tela_cels, ecs,
                    bs, papel, ts, dias_b, ins,
                )

            (c, ini, fim, esp, cels, tela_cels, ecs,
             bs, papel, ts, dias_b, ins) = cache[chave]

            linhas = [x for x in esp["dias"] if x["data"] == d]
            if len(linhas) != 1:
                raise RuntimeError(col + " " + iso + ": falta linha unica")

            de = linhas[0]
            numero, fonte = minutos_do_espelho(de)
            releitura = _realizado_do_dia_tela(
                c, d, ts, ecs, tela_cels,
            )

            emitir(
                "alvo", col=col, dia=d, competencia=[mes, d.year],
                janela=[ini, fim],
                visual=[esp["data_ini"], esp["data_fim"]],
                numero=numero, fonte=fonte,
                linha={k: de.get(k) for k in LINHA.split()},
                grade=[
                    {k: x.get(k) for k in GRADE.split()}
                    for x in (de.get("realizado_grade") or [])
                ],
                realizado_releitura=releitura._asdict(),
                e6_tem_chave=iso in ins.oraculo.minutos,
                e6_minutos=ins.oraculo.minutos.get(iso),
                e6_horas=ins.horas.get(iso),
                e6_envelope=ins.oraculo.envelope.get(iso),
                e6_corte=iso in ins.oraculo.cortes,
                # Classificacao automatica; nao confirma a causa factual.
                dono_automatico=ins.dono(d),
                cards=[
                    dict(entrada=p.entrada, saida=getattr(p, "saida", None),
                         aberto=p.turno_aberto)
                    for p in de["turnos"]
                ],
            )

            flo, fhi = janela_com_pad(ini, fim)
            pks_foco = set()

            for b in bs:
                if flo <= timezone.localdate(b.timestamp) <= fhi:
                    pks_foco.add(b.pk)
                    emitir(
                        "batida", col=col, alvo=d, dados=campos(b, BATIDA),
                        instante_local=timezone.localtime(b.timestamp),
                        papel_consultado=_papel_da_ata(b, papel),
                        dia_exibicao=dias_b.get(b.pk),
                    )

            # Carimbos nas COPIAS usadas pelo pareador:
            # app/ponto/turnos.py:1984-2000
            for t in ts:
                if not (
                    flo <= t.data_turno <= fhi
                    or any(b.pk in pks_foco for b in t.batidas)
                ):
                    continue
                emitir(
                    "turno", col=col, alvo=d, data_turno=t.data_turno,
                    entrada_pk=getattr(t.entrada, "pk", None),
                    saida_pk=getattr(t.saida, "pk", None),
                    fim_contado_pk=getattr(_fim_fechado(t), "pk", None),
                    aberto=t.aberto, cross_meianoite=t.cross_meianoite,
                    pares_marcados=_pares_marcados(t),
                    pausas_validas=intervalos_do_turno(t),
                    pausas_abaixo_piso=pausas_abaixo_do_piso(t),
                    batidas=[
                        dict(campos(b, BATIDA), **{
                            k: getattr(b, k, None) for k in FLAGS.split()
                        })
                        for b in t.batidas
                    ],
                )

            contexto_ecs = escalas_do_periodo(
                c, flo, fhi,
                mais=[ce.escala_geradora_id for ce in cels.values()],
            )
            for e in contexto_ecs:
                emitir(
                    "vinculo_candidato", col=col, alvo=d,
                    dados=campos(e, EC), template=campos(e.tipo_escala, TE),
                )

            datas = {d} | {
                dd for _, dd in cels if flo <= dd <= fhi
            }
            for dd in sorted(datas):
                ce = cels.get((c.pk, dd))
                e = vinculo_do_dia(c.pk, dd, contexto_ecs, celulas=cels)
                te = e.tipo_escala if e else None
                emitir(
                    "celula", col=col, alvo=d, dia=dd,
                    celula=(
                        campos(ce, "pk trabalha origem escala_geradora_id")
                        if ce else None
                    ),
                    dna_marcos=((ce.dna or {}).get("marcos") if ce else None),
                    juiz=periodos_do_dia(ce, te) if ce else None,
                    vinculo_escolhido=campos(e, EC) if e else None,
                    livre=te._intervalo_livre() if te else None,
                    intervalo_efetivo=(
                        e.intervalo_do_dia(dd, celulas=cels) if e else None
                    ),
                )

            aus = Ausencia.objects.filter(
                colaborador=c, data__lte=fhi,
            ).filter(
                Q(data__gte=flo) | Q(data_fim__gte=flo)
                | Q(data_fim__isnull=True, tipo__in=Ausencia.TIPOS_EM_ABERTO)
            )
            for a in aus:
                emitir(
                    "ausencia", col=col, alvo=d,
                    dados=campos(
                        a, "pk tipo data data_fim status minutos hora_ini hora_fim"
                    ),
                )

        if falhas_sql:
            raise RuntimeError(
                "Consulta falhou, inclusive em ramo capturado: "
                + json.dumps(falhas_sql)
            )
        for alias in aliases:
            transaction.set_rollback(True, using=alias)

    print("FIM_OK")
finally:
    logging.disable(nivel)
```

## 17/11 08:11 · sessao 8794266ec695 · CODEX-RESERVA-R6 · leitura · origem=Code

**CODEX-RESERVA-R6 — leituras herdadas**

Os números dos relatos abaixo são **registros históricos**, com suas fontes. Os scripts são propostas para execução no shell Django do tenant correto; seus resultados em produção estão **[nao verificado]**. `FIM_OK` indica conclusão da sonda, sem significar aprovação do critério de negócio.

---

**O275**

- **Pedido:** cadastrar “feriado é dia de trabalho” no `TipoEscala`, desligado por padrão, com leitura única em `eh_dia_trabalho_calculado`. A leitura deveria distinguir esse cadastro do flag do vínculo e esclarecer quais ciclos seriam alcançados. (`app/docs/BACKLOG.md:1651`; `app/docs/RELATO-ARQUIVO.md:750`)
- **Estado:** INTERROMPIDA; pouso retido na escolha FORMA A × FORMA B. (`app/docs/BACKLOG.md:1651`; `app/docs/GRADE-MESTRA.md:1769`)
- **Resposta:** “tipo × vínculo” **já foi respondido pelo censo da própria O275**: perguntas disjuntas, com precedência por `base`. Continua aberta a escolha entre alcançar todo ciclo salvo intermitente — A — ou somente `6x1/5x2/semanal` — B. (`app/docs/RELATO-ARQUIVO.md:750`; `app/docs/RELATO-ARQUIVO.md:751`)
- **Prova no código atual:** a fase de `12x36/24x48` retorna diretamente; no restante, o leitor apenas transforma trabalho em folga por `feriado_vira_folga`. O flag existente pertence ao vínculo e sua aplicação restringe ciclos semanais. O comportamento inverso pedido pela O275 ainda não está nesse leitor. (`app/escala/models.py:1289`; `app/escala/models.py:1365`; `app/escala/models.py:1369`; `app/escala/models.py:896`; `app/escala/models.py:1392`)
- **Próximo passo:** resolver A/B e retomar a construção registrada na raia; produção não decide essa escolha de lei. A pendência permanece na lista posterior de leis abertas. (`app/docs/GRADE-MESTRA.md:1769`; `app/docs/RELATO.md:837`)

---

**LAUDO-col33 — LAUDO-[nome]-COL33-0310**

- **Pedido:** rejulgar células antigas da competência 10 após a cura ATA-X-TURNO; caso nominal: célula `120035`, chamado `28365`. A leitura deveria responder se faltava cura, rejulgamento ou corte para o restante. (`app/docs/BACKLOG.md:1813`)
- **Estado herdado:** universo A aplicado; restante aguardando corte. A GRADE ainda registra PARADA. (`app/docs/BACKLOG.md:1813`; `app/docs/GRADE-MESTRA.md:250`)
- **Resposta:** **o caso nominal já foi respondido**: o universo A registrou célula concorde e chamado resolvido. O restante recebeu depois o corte **REJULGAR-COMP10-RESTO-DA-FROTA**, com atos G1/G2 e G3-col444. Portanto, “restante aguarda corte” envelheceu. (`app/docs/RELATO.md:4264`; `app/docs/RELATO.md:4265`; `app/docs/CORTES.md:360`; `app/docs/RELATO.md:4078`; `app/docs/RELATO.md:4079`; `app/docs/RELATO.md:4083`)
- **Prova no código atual:** a cura procura a saída órfã no dia seguinte quando o turno cruza a meia-noite e a transfere para o marco correspondente. O teste descreve precisamente col33, entrada em 03/10 e saída 08:59 de 04/10. (`app/escala/utils.py:1971`; `app/escala/utils.py:1996`; `app/escala/utils.py:2039`; `app/escala/tests/test_saida_valida_cross_meia_noite.py:5`)
- **Próximo passo:** acompanhar o residual **SOMBRA-X-PROD-COL988**, ainda aberto no estado revisado de CORTES. Os pendentes históricos col865/923/991 e col988 exigiam nova medida após reversão; situação viva **[nao verificado]**. (`app/docs/CORTES.md:102`; `app/docs/RELATO.md:4081`)

Sonda do caso nominal e das células residuais citadas nos atos; consulta somente estado persistido. (`app/docs/BACKLOG.md:1813`; `app/docs/RELATO.md:4078`; `app/docs/RELATO.md:4079`; `app/docs/RELATO.md:4081`)

```python
from django.db import connection, transaction
from escala.models import CelulaDia
from chamados.models import ChamadoColaborador

ids = {
    120035, 124639, 124645, 108785, 106809,
    124666, 124669, 124670,
}
with transaction.atomic():
    with connection.cursor() as cur:
        cur.execute("SET TRANSACTION READ ONLY")
    vistas = set()
    for c in CelulaDia.objects.filter(pk__in=ids).order_by("pk"):
        vistas.add(c.pk)
        ata = c.ata or {}
        print("CELULA", f"col{c.colaborador_id}", c.pk, c.data,
              c.veredito, c.veredito_via, c.julgada_em,
              {"n_missing": ata.get("n_missing"),
               "n_orfas": ata.get("n_orfas"),
               "minutos_realizados": ata.get("minutos_realizados")})
    print("CELULAS_NAO_LOCALIZADAS", sorted(ids - vistas))
    for ch in ChamadoColaborador.objects.filter(
        pk=28365, colaborador_id=33
    ):
        print("CHAMADO", "col33", ch.pk,
              ch.status_local, ch.via_encerramento)
print("FIM_OK")
```

---

**V3-E1**

- **Pedido:** bateria/arnês da v3; esclarecer a interrupção e revalidar os lotes. (`app/docs/BACKLOG.md:2244`; `app/docs/GRADE-MESTRA.md:1007`)
- **Estado:** INTERROMPIDA, etiqueta `MOTIVO-NAO-SEI`; bloqueios registrados C06/C16/24x48. A GRADE ainda fala em lotes não validados. (`app/docs/BACKLOG.md:2244`; `app/docs/GRADE-MESTRA.md:1775`)
- **Resposta:** o motivo pode ser especificado pelo registro posterior: **25 casos VALIDADOS verdes na v3; C05/C06/C16/C17 PENDENTE_DP; STATUS com 14 falhas em 11 casos**. O antigo `VALIDADOS=0` foi superado. **C17 já recebeu resposta:** HE 0 e intra 60, permanecendo pendente de conferência DP/contador. (`app/docs/RELATO.md:2985`; `app/docs/RELATO.md:2987`; `app/docs/RELATO.md:2994`)
- **Prova no código atual:** intra usa o máximo entre intervalo declarado, cadastrado e piso; o limite de HE é `previsto + min(suprimido, declarado)`. Assim, indenização de intra não gera automaticamente HE adicional. Para noturnas, o calculador declara falta de entrada quando não recebe os parâmetros de prorrogação e hora reduzida. (`app/ponto/calculador/regras.py:760`; `app/ponto/calculador/regras.py:764`; `app/ponto/calculador/regras.py:771`; `app/ponto/calculador/regras.py:613`)
- **Próximo passo:** recuperar o arnês da referência registrada, separar falhas STATUS das dúvidas de valor e resolver C06/C16/24x48 com o dono/DP. Os casos 24x48 perguntam qual norma fixa intervalo, noturno, extra e antecipada; o número produzido pelo código não resolve essa escolha. (`app/docs/BACKLOG.md:2244`; `app/docs/RELATO.md:2985`; `app/docs/RELATO.md:3231`; `app/docs/RELATO.md:3232`)

---

**V3-RETOMA-PERIODOS**

- **Pedido:** eliminar reconstruções por pontas, usando os períodos/pausas da mesma fonte do espelho; HE por período, transbordo reverso e parada quando faltar período na célula. A leitura deveria verificar a cura da classe inteira. (`app/docs/BACKLOG.md:1056`; `app/docs/RELATO.md:2778`)
- **Estado:** INTERROMPIDA; P4 incompleto, P5 pousou isoladamente. (`app/docs/BACKLOG.md:1056`; `app/docs/GRADE-MESTRA.md:1777`)
- **Resposta:** **a cura não está integral**. A alimentação já lê pontas efetivas e intervalos carimbados pelo motor, mas o núcleo ainda deriva pausa pelo gap entre pares. No fluxo de rubricas, célula ausente não provoca a parada exigida; a lavratura do oráculo ainda captura exceção genericamente e registra erro. (`app/ponto/calculador/alimentacao.py:76`; `app/ponto/calculador/alimentacao.py:83`; `app/ponto/calculador/alimentacao.py:107`; `app/ponto/calculador/nucleo.py:149`; `app/ponto/calculador/alimentacao.py:476`; `app/ponto/calculador/alimentacao.py:489`; `app/ponto/services/fechamento.py:1297`)
- **Prova posterior:** **P5-PRORROGACAO-POR-INTERVALO**, caminho b, pousou como `16c27de8`. A função atual recorta o desconto dos intervalos e limita a parte ao todo; esse pouso resolve P5, sem concluir P4. (`app/docs/RELATO.md:8703`; `app/docs/RELATO.md:8710`; `app/ponto/motor_calculo_v2.py:162`; `app/ponto/motor_calculo_v2.py:191`; `app/ponto/motor_calculo_v2.py:196`; `app/docs/BACKLOG.md:1056`)
- **Próximo passo:** resolver o período não guardado e concluir P4. Essa conclusão estrutural já é possível pela leitura do código. (`app/docs/GRADE-MESTRA.md:1777`; `app/ponto/services/fechamento.py:1297`)

---

**JANELA-DO-NOTURNO**

- **Pedido:** permitir cadastrar a janela noturna da CCT. A leitura deveria responder se existe configuração efetivamente consumida pelo cálculo. (`app/docs/BACKLOG.md:1738`)
- **Estado:** FILA no BACKLOG, PARADA na GRADE; exige aval para o motor. (`app/docs/BACKLOG.md:1738`; `app/docs/GRADE-MESTRA.md:234`)
- **Resposta:** **continua cravada em 22h–05h**. Além das constantes, a função usa diretamente `time(5, 0)` e `time(22, 0)`, sem parâmetro de janela. Alterar somente as constantes não altera esses literais. (`app/ponto/motor_calculo_v2.py:44`; `app/ponto/motor_calculo_v2.py:82`; `app/ponto/motor_calculo_v2.py:101`; `app/ponto/motor_calculo_v2.py:103`)
- **Prova:** o calculador reutiliza essa função. A P5 posterior trata intervalos/prorrogação e continua chamando `minutos_noturnos`; ela não acrescenta janela parametrizada a essa função. (`app/ponto/calculador/regras.py:85`; `app/ponto/motor_calculo_v2.py:191`; `app/ponto/motor_calculo_v2.py:196`)
- **Próximo passo:** manter a pendência de cadastro e consumo da janela, com o aval registrado para o sítio. A resposta estrutural dispensa dado de produção. (`app/docs/BACKLOG.md:1738`; `app/ponto/motor_calculo_v2.py:82`)

---

**troca S5b**

- **Pedido:** trocar sem criar desconto novo; comparar contra o gravado, inclusive por colaborador, explicando pareamento e dupla contagem. A leitura deveria verificar a fidelidade do DIFF à lavratura e distinguir escrita paralela de troca dos leitores. (`app/docs/BACKLOG.md:1231`; `app/docs/BACKLOG.md:1377`; `app/ponto/management/commands/diff_calculador.py:49`)
- **Estado herdado:** PAREI pelo critério individual de col516/col174; GRADE repete PARADA. (`app/docs/BACKLOG.md:1377`; `app/docs/GRADE-MESTRA.md:787`)
- **Resposta:** **o bloqueio individual antigo foi respondido pela porta única de alimentação**. A causa era diferença de pareamento, descrita no código atual; o fecho posterior do critério registra zero colaborador divergente naquela medição. O primeiro anúncio de fecho teve retratação por erro UTC e não deve ser usado sozinho. (`app/ponto/calculador/alimentacao.py:4`; `app/ponto/calculador/alimentacao.py:12`; `app/docs/RELATO-ARQUIVO.md:24452`; `app/docs/RELATO-ARQUIVO.md:24867`)
- **Prova no código atual:** a escrita `oraculo` existe ao lado de `motor`, desde 10/2026; os somadores e leitores consultados continuam usando `motor`. **A escrita paralela está implementada; a troca desses leitores não ocorreu.** O relato posterior separa o fecho da escrita do bloqueio da troca por O119. (`app/ponto/services/fechamento.py:1265`; `app/ponto/services/fechamento.py:1294`; `app/ponto/services/dia_pago.py:550`; `app/folha/porta_export.py:284`; `app/docs/RELATO-ARQUIVO.md:24247`; `app/docs/RELATO-ARQUIVO.md:24288`)
- **Próximo passo:** remedir com o instrumento real contra gravado e períodos do fechamento, expondo erros e cegueiras antes de concluir sobre a troca. A paridade atual está **[nao verificado]**. (`app/ponto/management/commands/diff_calculador.py:45`; `app/ponto/management/commands/diff_calculador.py:49`; `app/ponto/management/commands/diff_calculador.py:592`)

Sonda sem CSV, usando as opções existentes do instrumento; a saída identifica pessoas por `colNNN`. (`app/ponto/management/commands/diff_calculador.py:31`; `app/ponto/management/commands/diff_calculador.py:604`; `app/ponto/management/commands/diff_calculador.py:656`)

```python
import ast
import io
from django.core.management import call_command
from django.db import connection, transaction

saida = io.StringIO()
with transaction.atomic():
    with connection.cursor() as cur:
        cur.execute("SET TRANSACTION READ ONLY")
    call_command(
        "diff_calculador",
        mes=10, ano=2026,
        contra_gravado=True,
        periodos_do_fechamento=True,
        limite_colabs=0,
        csv=None,
        stdout=saida,
    )
    texto = saida.getvalue()
    print(texto)
    linha = next(x for x in texto.splitlines()
                 if x.startswith("dia-colab comparados:"))
    contadores = ast.literal_eval(linha[linha.index("{"):])
    assert contadores.get("dia_colab_comparado", 0) > 0, "universo vazio"
    erros = {k: v for k, v in contadores.items()
             if v and (k.startswith("erro")
                       or k == "sem_resultado_do_fechamento"
                       or "FALHOU" in k)}
    assert not erros, erros
print("FIM_OK")
```

---

**COL369**

- **Pedido:** explicar inversões em 23/09 versus 26–27/09; trabalho com realizado zero sob Ausência 4538; vínculo 1296 impossível versus 1313 sem folga. (`app/docs/BACKLOG.md:1246`)
- **Estado herdado:** “ABERTO — nenhum medido”, REGISTRADA na GRADE. Foi superado parcialmente por **COL369-TIPOS-INVERTIDOS**, **COL369-VINCULO-1296** e pauta da folga. (`app/docs/BACKLOG.md:1246`; `app/docs/GRADE-MESTRA.md:182`; `app/docs/BACKLOG.md:1380`; `app/docs/BACKLOG.md:1368`; `app/docs/BACKLOG.md:1379`)
- **Resposta:**
  - **Inversão:** a medição posterior explicou a diferença pela pausa real: 59 minutos em 23/09, 73/64 em 26–27/09, diante de cadastro de 60. O contrafactual explicou 2,00 h de intra e 2,29 h trabalhadas; a obra era medir, sem aplicar cura. Hoje o motor e o pareador consultam a autoridade da ata quando legível. (`app/docs/RELATO-ARQUIVO.md:23361`; `app/docs/RELATO-ARQUIVO.md:23370`; `app/docs/RELATO-ARQUIVO.md:23379`; `app/ponto/motor_calculo_v2.py:534`; `app/ponto/turnos.py:823`)
  - **Realizado zero:** **BO-B DIAPAGO-SE-CONTRADIZ / B-B21** corrigiu a classe: trabalho em ausência entra no realizado e abono cobre o restante do previsto. A prova histórica preservou a 09; o estoque específico de col369 em 29–30/09 permanece **[nao verificado]**. (`app/ponto/services/dia_pago.py:375`; `app/ponto/services/dia_pago.py:393`; `app/ponto/services/dia_pago.py:400`; `app/docs/RELATO.md:5475`; `app/docs/CORTES.md:344`)
  - **Vínculo:** ficou decidido **NÃO TROCAR**; pauta 908 pediu à supervisão o dia de folga. O DIFF completo corrigiu a previsão inicial: troca isolada moveria **−7,07 h e removeria zero furos**, pois as células exigiriam regeneração separada. (`app/docs/BACKLOG.md:1368`; `app/docs/BACKLOG.md:1379`; `app/docs/RELATO-ARQUIVO.md:26535`; `app/docs/RELATO-ARQUIVO.md:26546`)
- **Prova:** o leitor atual devolve `cel.trabalha`; a correção do mecanismo de ausência calcula `coberto = min(realizado, previsto)` e `abono = previsto − coberto`. (`app/escala/models.py:1095`; `app/ponto/services/dia_pago.py:394`; `app/ponto/services/dia_pago.py:396`)
- **Próximo passo:** conferir estoque e cadastro atuais por sonda, preservando a decisão registrada de não trocar o vínculo. (`app/docs/BACKLOG.md:1368`)

Sonda dos campos gravados e da mesma derivação de grade usada pela folha. (`app/folha/export.py:329`; `app/ponto/services/dia_pago.py:364`; `app/ponto/models.py:349`)

```python
from datetime import date
from django.db import connection, transaction
from ponto.models import DiaPago, FechamentoMensal
from escala.models import EscalaColaborador
from folha.export import grade_do_fechamento
from ponto.services.dia_pago import por_dia_da_grade

with transaction.atomic():
    with connection.cursor() as cur:
        cur.execute("SET TRANSACTION READ ONLY")
    for e in EscalaColaborador.objects.filter(
        colaborador_id=369
    ).order_by("data_inicio", "pk"):
        print("VINCULO", "col369", e.pk, e.ativa,
              e.data_inicio, e.data_fim, e.tipo_escala_id)
    for r in DiaPago.objects.filter(
        colaborador_id=369,
        data__range=(date(2026, 9, 19), date(2026, 10, 1)),
    ).order_by("data", "versao").values(
        "data", "mes", "ano", "versao", "horas_trabalhadas",
        "horas_intra_indenizada", "minutos_realizados",
        "minutos_abonados", "minutos_previstos",
    ):
        print("DIAPAGO", "col369", r)
    for f in FechamentoMensal.objects.filter(
        colaborador_id=369, mes=9, ano=2026
    ).select_related("colaborador__empresa"):
        abono, previsto, realizado = por_dia_da_grade(grade_do_fechamento(f))
        for d in (date(2026, 9, 29), date(2026, 9, 30)):
            print("GRADE", "col369", d, previsto.get(d),
                  realizado.get(d), abono.get(d))
print("FIM_OK")
```

---

**O236**

- **Pedido:** decompor por motivo os retidos históricos 200/238/94, pela função real, e medir quantos a L-033 libera. (`app/docs/BACKLOG.md:1293`)
- **Estado herdado:** achado medido, não curado; abertura por motivo não feita; REGISTRADA na GRADE. (`app/docs/BACKLOG.md:1293`; `app/docs/GRADE-MESTRA.md:185`)
- **Resposta:** **a premissa “L-033 só no papel” foi superada por O254/O259**. A porta atual isenta o dia com exclusivamente `FURO_PARCIAL` e confirmação de que somente marcos de pausa estão apagados. Dias mistos continuam retidos; desligados e isentos também têm saída explícita. A decomposição atual da população depende de produção. (`app/docs/RELATO-ARQUIVO.md:43115`; `app/docs/RELATO-ARQUIVO.md:42950`; `app/folha/export.py:1173`; `app/folha/export.py:1207`; `app/folha/export.py:1293`)
- **Prova:** `com_dias=True` fornece os próprios dias e motivos do juiz; o vigia chama a mesma isenção. CORTES também corrige números herdados sem lastro no arquivo alegado, que não devem ser reciclados como prova atual. (`app/folha/export.py:1186`; `app/folha/export.py:1212`; `app/folha/services/vigia_pausa.py:19`; `app/docs/CORTES.md:327`; `app/docs/CORTES.md:328`)
- **Próximo passo:** medir motivos e universo da isenção, mantendo o critério existente. A quantidade atual liberada/retida está **[nao verificado]**. (`app/docs/BACKLOG.md:1293`)

Sonda de todas as empresa-competências existentes em `FechamentoMensal`, com classificação real do export e censo real da pausa. (`app/folha/export.py:765`; `app/folha/services/vigia_pausa.py:100`)

```python
from collections import Counter
from django.db import connection, transaction
from colaboradores.models import Empresa
from ponto.models import FechamentoMensal
from folha.export import classificar_export
from folha.services.vigia_pausa import censo

with transaction.atomic():
    with connection.cursor() as cur:
        cur.execute("SET TRANSACTION READ ONLY")
    competencias = list(
        FechamentoMensal.objects.order_by().values_list(
            "colaborador__empresa_id", "mes", "ano"
        ).distinct()
    )
    print("UNIVERSO_EMPRESA_COMPETENCIA", len(competencias))
    for eid, mes, ano in sorted(competencias):
        empresa = Empresa.objects.get(pk=eid)
        itens = classificar_export(empresa, mes, ano)
        resumo = Counter()
        for item in itens:
            motivos = tuple(item.get("motivos_espelho") or ())
            chave = (item["status"], item["motivo"], motivos)
            resumo[chave] += 1
            print("O236", f"col{item['colaborador_id']}",
                  eid, mes, ano, chave)
        print("RESUMO", eid, mes, ano, dict(resumo))
        pausa = censo(empresas=[empresa], competencias=[(mes, ano)])
        for universo in pausa["universos"]:
            universo.pop("cnpj8", None)
        print("L033", eid, mes, ano, pausa)
print("FIM_OK")
```

---

**AUSENCIA-UMA-VIGENTE**

- **Pedido:** resolver abono+falta que desconta, pela lei da família, sem apagar registro; aplicação em exportada depende da autorização registrada. A leitura deveria localizar os produtores da sobreposição e a divergência entre cobertura e desconto. (`app/docs/BACKLOG.md:1941`; `app/docs/RELATO.md:2258`)
- **Estado:** REGISTRADA. O relato histórico já respondeu a leitura, medindo quatro dias contraditórios e identificando os escritores. (`app/docs/GRADE-MESTRA.md:259`; `app/docs/RELATO.md:2258`)
- **Resposta no código atual:** **a contradição continua possível**: desconto soma todas as aprovadas que descontam; cobertura escolhe uma, pela maior `(data, pk)`. A constraint limita colaborador/tipo/data, permitindo tipos diferentes no mesmo dia. (`app/folha/export.py:87`; `app/folha/export.py:140`; `app/ponto/turnos.py:2765`; `app/ponto/turnos.py:2793`; `app/ponto/models.py:836`)
- **Prova dos produtores:** `retornar_mesmo_tipo` permite tipo diferente ao lado e continua usado pela materialização; a ap

[truncado em 20000 de 24796 caracteres]

## 17/11 08:10 · sessao 7fe12853c20b · CODEX-RESERVA-R4 · leitura · origem=Code

Os **dois “Nosso, sem obra” são os mesmos dois do GRADE-RESOLVE**, não quatro casos distintos. Ambos ganharam complemento de investigação; nenhuma cura específica está demonstrada para esses dias. (`app/docs/RELATO.md:666`; `app/docs/BACKLOG.md:865`; `app/docs/BACKLOG.md:1589`)

**Limite do histórico:** esta cópia não contém metadados Git. Portanto, `git log`, ancestralidade e diferenças desde 05/11 ficam **[nao verificado]**; os hashes abaixo têm lastro documental. A própria GRADE registra essa ausência. (`app/docs/GRADE-MESTRA.md:1711`)

**(a) Causa, cobertura e menor escopo**

A coluna “menor escopo” contém propostas de investigação, não curas já demonstradas: **[nao verificado]**.

| Caso | Causa demonstrável no código e limite | Obra registrada que cobre | Menor escopo |
|---|---|---|---|
| **HJ-005-U1 — col42, 01/10** | O ramo de fato real em ausência aprovada pode produzir `FATO_EM_AUSENCIA`, convertido em irregularidade/retenção. A ausência efetivamente usada, seu período e a posição da batida nesse período estão **[nao verificado]**. (`app/ponto/supra_juiz.py:392`; `app/ponto/supra_juiz.py:413`; `app/ponto/status_dia.py:68`; `app/folha/export.py:1293`) | **BO-ADMIN-2710**, complemento de censo. **Sem obra de cura demonstrada**: COBERTURA-PARCIAL nomeia 08/15/22-10; o registro ressalva que sua cobertura de 01/10 não foi provada. (`app/docs/BACKLOG.md:865`; `app/docs/BACKLOG.md:874`; `app/docs/GRADE-MESTRA.md:137`) | Reconferir retenção, lavra, ausência/período e batidas de 01/10 na mesma leitura; resolver dono/destino. Só depois delimitar correção do ramo que divergir. (`app/docs/BACKLOG.md:865`) |
| **HJ-012-U2 — col239, 12/10** | `ESTRUTURA` é também retorno **residual** do juiz: não ter predicado CADASTRO/BATIDA não prova cobrança indevida. A ingestão e a medição anterior discordam: “espelho_cobrado/ESTRUTURA” versus “legítimo → DP”. Defeito específico **[nao verificado]**. (`app/ponto/management/commands/e6_oraculo.py:98`; `app/ponto/management/commands/e6_oraculo.py:110`; `app/docs/BACKLOG.md:1589`) | **BUG-RETRATO-VELHO-NA-TELA**, complemento de reconciliação. **Sem cura específica demonstrada** para esse dia. (`app/docs/BACKLOG.md:1589`; `app/docs/GRADE-MESTRA.md:845`) | Reconciliar retenção atual, lavra, dono e regra do feriado. Aviso de frescor do retrato não basta para provar correção da cobrança. (`app/docs/BACKLOG.md:1589`; `app/folha/retrato_folha.py:1413`) |
| **G3-10 — col163, 10/10** | Classificação residual ESTRUTURA; na medição, não segurava e tinha status `ausencia`. Causa de defeito **[nao verificado]**. (`app/docs/RELATO.md:9274`; `app/docs/RELATO.md:9281`; `app/ponto/management/commands/e6_oraculo.py:110`) | **G3G4-ESTRUTURA-RESIDUO** registra o levantamento; a própria linha diz **sem obra**, padrão não nomeado. (`app/docs/GRADE-MESTRA.md:274`; `app/docs/BACKLOG.md:2208`) | Primeiro demonstrar divergência positiva entre autoridades; localizar o primeiro resultado contraditório e nomear uma classe antes de construir. (`app/docs/RELATO.md:9281`) |
| **G3-11 — col163, 17/10** | Residual ESTRUTURA; não segurava, status `folga_trabalhada`. Defeito **[nao verificado]**. (`app/docs/RELATO.md:9275`; `app/docs/RELATO.md:9281`) | Mesmo registro de levantamento; **sem obra de cura demonstrada**. (`app/docs/BACKLOG.md:2208`) | Reconferir trabalho/folga, retenção e pagamento; só abrir cura se houver contradição demonstrada. (`app/docs/RELATO.md:9281`) |
| **G3-12 — col873, 26/09** | Residual ESTRUTURA; não segurava, status `ausencia`. Defeito **[nao verificado]**. (`app/docs/RELATO.md:9276`; `app/docs/RELATO.md:9281`) | Mesmo registro; **sem obra de cura demonstrada**. (`app/docs/BACKLOG.md:2208`) | Reconferir autoridades e natureza da ausência; eventual tratamento de competência entregue depende do alcance efetivamente demonstrado. (`app/docs/RELATO.md:9281`) |

**Totais de (a):** 2 casos do quadro GRADE-RESOLVE, com 2 complementos de investigação; mais 3 resíduos históricos registrados na GRADE. São **5 casos examinados e nenhuma cura específica demonstrada** nesses registros. Os três resíduos não aumentam os “2 sem obra” do quadro de 18 ESTRUTURA. (`app/docs/RELATO.md:666`; `app/docs/BACKLOG.md:865`; `app/docs/BACKLOG.md:1589`; `app/docs/BACKLOG.md:2208`)

**(b) O que mudou desde 05/11**

Estas mudanças justificam reconferência; sua aplicação a cada pessoa/dia continua **[nao verificado]** sem nova sonda.

| Referência | Mudanças documentadas posteriores à ingestão | O que pode mudar |
|---|---|---|
| **A — ausências** | H12H06-A, 05/11 22:1x, `ca278fc4`; H12H06-B, 06/11, raia `97754f8d`/comboio `62017e70`. (`app/docs/RELATO.md:9210`; `app/docs/RELATO.md:9212`; `app/docs/RELATO.md:2324`) | Faltas pela jornada da grade e leitores de cobertura parcial; houve aplicação em 28 linhas da competência 11, sem vínculo individual demonstrado com estes casos. (`app/docs/RELATO.md:9211`; `app/docs/RELATO.md:2395`; `app/docs/RELATO.md:3107`) |
| **M — motor e turnos** | V2 item 2, 07/11, `a23831c3`; BUG-ENC-CELULAS-FORMA, 15/11 23:0x, `7825fec5b`. (`app/docs/BACKLOG.md:1954`; `app/docs/RELATO.md:256`) | Feriado/HE100 noturna; correção da forma das células fornecidas ao juiz de turnos. (`app/docs/BACKLOG.md:1954`; `app/docs/BACKLOG.md:1021`) |
| **F — fatos e fio** | E1 `a8a692f55` (08/11); E1-FILA `7aa48401` e E2/primeira E3 `7c16da320` (09/11); E4a-1 `892159c5f`, E4a-2 `613a51498`, E4b `087fe758a`. (`app/docs/BACKLOG.md:2008`; `app/docs/BACKLOG.md:2009`; `app/docs/BACKLOG.md:2021`; `app/docs/BACKLOG.md:2035`; `app/docs/BACKLOG.md:2043`; `app/docs/BACKLOG.md:2044`) | Nascimento de fatos/perguntas, tarefa do dia, atos e recuperação do fio. Pouso de código não comprova aplicação do acervo por caso. (`app/docs/BACKLOG.md:2022`; `app/docs/BACKLOG.md:2044`) |
| **P — leitores e portas** | CXR `94d41ea5` (08/11); E3 X1 `e70a54f7e` (11/11); E3 v10 merge `90f3fa66b`, deploy 14/11 22:42; R3 c17 `f8ee770a5` (16/11). (`app/docs/BACKLOG.md:886`; `app/docs/RELATO.md:1446`; `app/docs/RELATO.md:614`; `app/docs/RELATO.md:597`; `app/docs/BACKLOG.md:770`) | Visibilidade do cadastro apontado pelo motor, retenção de células e ação alcançável no fio. (`app/docs/BACKLOG.md:886`; `app/docs/RELATO.md:1439`; `app/docs/RELATO.md:610`; `app/docs/BACKLOG.md:770`) |
| **D — desligados** | LEI-DESLIGADO-NAO-TRAVA `9e2c54017` (13/11); LOTE3 c7 `be4a6219e` (16/11). (`app/docs/BACKLOG.md:731`; `app/docs/RELATO.md:184`) | Desligado deixa de reter a folha; tratamento da situação sem porta. Ser desligado em qualquer caso listado: **[nao verificado]**. (`app/folha/export.py:1197`; `app/docs/RELATO.md:184`) |
| **J — resposta e férias** | O168 `d33c0a899` (13/11); HJ-091 `8376da7aa` (14/11); F1 B1 `3adf703e3` (16/11). (`app/docs/BACKLOG.md:2093`; `app/docs/BACKLOG.md:750`; `app/docs/RELATO.md:174`) | Reconciliação de conversa/resposta, origem das ausências de férias e juiz de resposta. (`app/docs/BACKLOG.md:2093`; `app/docs/BACKLOG.md:750`; `app/docs/RELATO.md:174`) |
| **B — batida e intervalo** | Dedup `b6d268dfd` e CONTRATO-DE-ENTRADA-DO-FIO `77dfa94fa` (15/11); BUG-PAR `41f5648af` (16/11 21:41). (`app/docs/RELATO.md:475`; `app/docs/RELATO.md:32`) | Entrada de novos fatos, horário/tipo entregue ao fio e respostas de intervalo. Não comprova correção retroativa dos dias antigos. (`app/docs/RELATO.md:475`; `app/docs/BACKLOG.md:2084`) |
| **L — porta de ausência** | LOTE7 c7 `42fd467a0` (16/11). (`app/docs/RELATO.md:164`) | Porta de ausência/regularização; o controle documentado do kernel não demonstra mudança nos casos deste quadro. (`app/docs/RELATO.md:175`) |

**Não contam como cura posterior à ingestão:** F3-BECO-1 já estava NO AR às 17:5x de 05/11; C20-JUIZ-3 está registrado às 20:2x, enquanto a ingestão aparece às 20:4x. A nota “remedir depois do F3” não comprova que aquele pouso ocorreu depois da sonda. (`app/docs/BACKLOG.md:2210`; `app/docs/RELATO.md:9154`; `app/docs/RELATO.md:9165`; `app/docs/RELATO.md:9177`)

**Tabela nominal CADASTRO/BATIDA**

O agregado é **11 CADASTRO + 9 BATIDA**, com **4 CADASTRO e 1 BATIDA sem individualizar**. A cópia não fornece o manifesto completo que liga cada linha ao quadro v2. Portanto, a associação dos registros do levantamento dos 67 ao balde v2 é **[nao verificado]**; eles estão separados abaixo pela origem documental. (`app/docs/RELATO.md:240`; `app/docs/RELATO.md:585`)

| Caso documentado | Identificação disponível | Mudança que justifica conferir; limite |
|---|---|---|
| HJ-001-U1 — CADASTRO | col836; dia por unidade **[nao verificado]**. (`app/docs/RELATO.md:9172`) | Escala suspeita/espelho indefinido; verificar **P**, além do F3 já citado na própria pendência. Resultado atual **[nao verificado]**. (`app/docs/RELATO.md:9177`) |
| HJ-001-U2 — CADASTRO | col836; dia por unidade **[nao verificado]**. (`app/docs/RELATO.md:9172`) | Mesmo encaminhamento documental; igualdade da causa entre U1/U2 **[nao verificado]**. (`app/docs/RELATO.md:9177`) |
| HJ-016-U1 — CADASTRO | col297; dia **[nao verificado]**. (`app/docs/RELATO.md:9172`) | **M/P** podem alterar insumos ou leitura; mudança específica **[nao verificado]**. |
| BO-0611/07 — CADASTRO | Pessoa/dia **[nao verificado]**. (`app/docs/RELATO.md:585`) | Sem pessoa/dia/causa, atribuir uma mudança específica seria **[nao verificado]**. |
| BO-1311-ROTEIRO/04 — CADASTRO | Pessoa/dia **[nao verificado]**. (`app/docs/RELATO.md:585`) | Mudança específica **[nao verificado]**. |
| BO-1311-ROTEIRO/10 — CADASTRO | Pessoa/dia **[nao verificado]**. (`app/docs/RELATO.md:585`) | Mudança específica **[nao verificado]**. |
| BO-ADMIN-2710/8.2 — CADASTRO | col789; datas exatas do caso **[nao verificado]**. (`app/docs/BACKLOG.md:866`; `app/docs/RELATO.md:585`) | Reexaminar vínculo/folgas e **P**; cura do resíduo não está demonstrada na obra da trilha. (`app/docs/BACKLOG.md:866`) |
| **4 CADASTRO sem ID** | Individualização ausente. (`app/docs/RELATO.md:240`) | Pessoa, dia, causa e impacto de commit **[nao verificado]**. |
| HJ-003-U1 — BATIDA | col407; data da unidade **[nao verificado]**. (`app/docs/RELATO.md:9173`) | **M/F/P** são candidatos; há par col407 21/10 em outro quadro, mas identidade com esta unidade **[nao verificado]**. (`app/docs/RELATO.md:2348`) |
| HJ-005-U5 — BATIDA | col42; data exata da unidade **[nao verificado]**. (`app/docs/RELATO.md:9173`) | **A/M/P**. Um relato diz BATIDA→sem divergência, porém conserva a remedição sem prova; não fechar por ele. (`app/docs/RELATO.md:752`) |
| HJ-015-U1 — BATIDA | col39; data da unidade **[nao verificado]**. (`app/docs/RELATO.md:9173`) | **M/F/P**. Equivalência com G4-01, col39 15/10, **[nao verificado]**. (`app/docs/RELATO.md:9278`) |
| HJ-027-U1 — BATIDA | col292; data da unidade **[nao verificado]**. (`app/docs/RELATO.md:9173`) | **M/F/P**. Outro quadro contém col292 21/09; equivalência com a unidade **[nao verificado]**. (`app/docs/RELATO.md:2348`) |
| BO-0611/01 — BATIDA | Pessoa/dia **[nao verificado]**. (`app/docs/RELATO.md:585`) | Mudança específica **[nao verificado]**. |
| BO-0611/04 — BATIDA | Pessoa/dia **[nao verificado]**. (`app/docs/RELATO.md:585`) | Mudança específica **[nao verificado]**. |
| BO-0611/13 — BATIDA | Pessoa/dia **[nao verificado]**. (`app/docs/RELATO.md:585`) | Mudança específica **[nao verificado]**. |
| HJ-093 — BATIDA no levantamento dos 67 | col1004, 27/10. Outro registro confirma classe JUSTIFICATIVA-PAGA e pergunta viva por decisão humana. (`app/docs/RELATO.md:585`; `app/docs/BACKLOG.md:798`) | Reconferir **A/J/P**; a divergência de enquadramento entre registros impede repetir “BATIDA” como diagnóstico atual. (`app/docs/BACKLOG.md:798`) |
| **1 BATIDA sem ID** | Individualização ausente. (`app/docs/RELATO.md:240`) | Pessoa, dia, causa e impacto de commit **[nao verificado]**. |

Há ainda pares CADASTRO/BATIDA no levantamento G3/G4, mas sua pertença aos baldes v2 não está comprovada; não os somei novamente. Isso inclui dias que já **não seguravam** apesar do dono CADASTRO/BATIDA. (`app/docs/RELATO.md:9268`; `app/docs/RELATO.md:9269`; `app/docs/RELATO.md:9270`; `app/docs/RELATO.md:9271`; `app/docs/RELATO.md:9272`; `app/docs/RELATO.md:9277`; `app/docs/RELATO.md:9278`)

**Os nove casos vinculados a obras antigas NO AR**

São **9 casos em 5 obras**, cujos pousos citados são de outubro. Esses pousos não constituem mudanças posteriores à ingestão de 05/11. (`app/docs/BACKLOG.md:874`; `app/docs/BACKLOG.md:864`; `app/docs/BACKLOG.md:882`; `app/docs/BACKLOG.md:868`; `app/docs/BACKLOG.md:866`)

| Caso | Obra antiga | Última situação fornecida; reconferência necessária |
|---|---|---|
| HJ-005-U2 | COBERTURA-PARCIAL, `fb813587`, 28/10. (`app/docs/BACKLOG.md:874`) | ABERTO no anexo C1; verificar **A/M/P**. Resultado posterior **[nao verificado]**. (`app/docs/RELATO.md:240`) |
| HJ-005-U4 | Mesma obra. (`app/docs/BACKLOG.md:874`) | ABERTO no anexo C1; mesmos candidatos, sem fechamento posterior demonstrado. (`app/docs/RELATO.md:240`) |
| BO-ADMIN-2710/1 | Mesma obra; col42 08/15/22-10. (`app/docs/BACKLOG.md:874`) | ABERTO, parcial sem período no anexo C1; conferir os três dias separadamente. (`app/docs/RELATO.md:240`) |
| BO-ADMIN-2710/2 | JUIZ-DO-PAR, `b3bf529e`, 29/10. (`app/docs/BACKLOG.md:864`) | Faltam os sete dias exatos no anexo; impacto individual **[nao verificado]**. |
| BO-ADMIN-2710/5 | Mesma obra; col247 24/10. (`app/docs/BACKLOG.md:864`) | ABERTO no C1; **M/P** e BUG-PAR posterior justificam nova leitura, sem presumir pertença ao universo curado. (`app/docs/RELATO.md:240`; `app/docs/BACKLOG.md:2084`) |
| HJ-030-U1 | FIO-LE-O-JUIZ, `7c9e4c27`, 27/10. (`app/docs/BACKLOG.md:882`) | FECHADO **somente o sintoma de invisibilidade**, conforme anexo; não equivale a fechar todos os problemas do dia. (`app/docs/RELATO.md:240`) |
| BO-ADMIN-2710/6 | Mesma obra. (`app/docs/BACKLOG.md:882`) | Falta tela/perfil do supervisor no anexo; **P** pode mudar a porta, alcance por perfil **[nao verificado]**. |
| BO-ADMIN-2710/7 | REGISTRADO-NAO-E-VIVO, 27/10; col130 05/10. (`app/docs/BACKLOG.md:868`) | FECHADA somente a pendência de fila, conforme anexo; demais sintomas **[nao verificado]**. (`app/docs/RELATO.md:240`) |
| BO-ADMIN-2710/8.1 | TRILHA-DO-VINCULO-MENTE, `5e9a0a51`, 28/10. (`app/docs/BACKLOG.md:866`) | Continua sem juiz leitor da fidelidade histórica no anexo. Sonda de retenção/dono não certifica a trilha. (`app/docs/RELATO.md:240`; `app/docs/BACKLOG.md:866`) |

**Sonda proposta — somente leitura, sem execução nesta reserva**

O corpo abaixo chama as autoridades reais: retenção por `dias_que_seguram`, status lavrado e `InsumosDoDono.dono`. Mantém ausências de pessoa/dia como lacunas; não transforma dono em prova de divergência. (`app/folha/fora_da_folha.py:125`; `app/ponto/status_dia.py:201`; `app/ponto/management/commands/e6_oraculo.py:182`; `app/docs/RELATO.md:9168`)

É um **payload para runtime Django já inicializado**, com tenant selecionado e role `leitor`; não chama `django.setup()` nem o lançador que grava arquivos. A configuração SaaS cria diretório/configura log em arquivo, e `sonda_leitura.sh` regrava credenciais. Execução e resultado deste payload: **[nao verificado]**. (`app/config/settings/saas.py:118`; `app/config/settings/saas.py:138`; `bin/sonda_leitura.sh:121`)

```python
# Executar em runtime Django JA inicializado, Python sem bytecode,
# tenant ja selecionado e credencial leitor. Nao salvar arquivo.
# Fontes das autoridades:
# app/folha/fora_da_folha.py:125
# app/ponto/status_dia.py:201
# app/ponto/management/commands/e6_oraculo.py:128 e :182
# app/ponto/janelas.py:60
import sys
sys.dont_write_bytecode = True

import contextlib
import datetime as dt
import io
import json
import logging
import re

from django.apps import apps
from django.db import connection, transaction
from django.utils import timezone

assert apps.ready, "Runtime Django nao inicializado"
assert not connection.in_atomic_block, "Exige transacao propria"
assert getattr(connection, "schema_name", None) not in (None, "public"), \
    "Tenant nao selecionado"

from colaboradores.models import Colaborador
from folha.fora_da_folha import dias_que_seguram
from ponto.janelas import janela_atual
from ponto.status_dia import status_do_dia_lavrado
from ponto.management.commands.e6_oraculo import InsumosDoDono

# (caso, colNNN, datas comprovadas).
# () significa DATA AUSENTE; nunca preencher por ordem presumida.
# HJ: app/docs/RELATO.md:9172 e :9173.
# BO/roteiro: app/docs/RELATO.md:585.
# HJ-093: app/docs/BACKLOG.md:798.
CASOS = [
    ("HJ-001-U1", 836, ()),
    ("HJ-001-U2", 836, ()),
    ("HJ-016-U1", 297, ()),
    ("BO-0611/07", None, ()),
    ("BO-1311-ROTEIRO/04", None, ()),
    ("BO-1311-ROTEIRO/10", None, ()),
    ("BO-ADMIN-2710/8.2", 789, ()),
    ("HJ-003-U1", 407, ()),
    ("HJ-005-U5", 42, ()),
    ("HJ-015-U1", 39, ()),
    ("HJ-027-U1", 292, ()),
    ("BO-0611/01", None, ()),
    ("BO-0611/04", None, ()),
    ("BO-0611/13", None, ()),
    ("HJ-093", 1004, ("2026-10-27",)),

    # Casos antigos NO AR; datas apenas quando comprovadas.
    # app/docs/BACKLOG.md:874, :864, :838, :868, :866.
    ("HJ-005-U2", 42, ()),
    ("HJ-005-U4", 42, ()),
    ("BO-ADMIN-2710/1", 42,
     ("2026-10-08", "2026-10-15", "2026-10-22")),
    ("BO-ADMIN-2710/2", None, ()),
    ("BO-ADMIN-2710/5", 247, ("2026-10-24",)),
    ("HJ-030-U1", 94, ("2026-10-16",)),
    ("BO-ADMIN-2710/6", None, ()),
    ("BO-ADMIN-2710/7", 130, ("2026-10-05",)),
]
# /8.1 exige leitor da fidelidade da trilha: nao certificado aqui.
# Manifesto completo v2 e ligacao dos aliases: [nao verificado].
# Lacunas expressamente registradas em app/docs/RELATO.md:240.
SEM_ID = {"CADASTRO": 4, "BATIDA": 1}

def emitir(obj):
    print(json.dumps(obj, ensure_ascii=True, sort_keys=True))

def apenas_select(execute, sql, params, many, context):
    # Bloqueio conservador: nao permite SET, DML, DDL, WITH ou EXPLAIN.
    # A transacao READ ONLY permanece como segunda barreira.
    if not re.match(r"^\s*SELECT\b", sql, flags=re.I):
        raise RuntimeError("SQL_NAO_SELECT_BLOQUEADO")
    return execute(sql, params, many, context)

resultados = []
erros = 0
cache = {}
agora = timezone.now()
hoje = timezone.localtime(agora).date()
nivel_log = logging.root.manager.disable
logging.disable(logging.CRITICAL)

try:
    with transaction.atomic():
        with connection.cursor() as cur:
            cur.execute(
                "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
            )
            cur.execute("""
                SELECT current_user, rolsuper, rolbypassrls
                FROM pg_roles WHERE rolname = current_user
            """)
            role, superuser, bypass = cur.fetchone()
            assert role == "leitor" and not superuser and not bypass

            cur.execute("""
                SELECT EXISTS (
                    SELECT 1
                    FROM information_schema.tables
                    WHERE table_type = 'BASE TABLE'
                      AND table_schema NOT IN
                          ('pg_catalog', 'information_schema')
                      AND has_table_privilege(
                          current_user,
                          format('%I.%I', table_schema, table_name),
                          'INSERT,UPDATE,DELETE,TRUNCATE,TRIGGER'
                      )
                )
            """)
            assert cur.fetchone()[0] is False, "Role possui escrita"

        with connection.execute_wrapper(apenas_select):
            for caso, cid, datas in CASOS:
                if cid is None or not datas:
                    resultados.append({
                        "caso": caso,
                        "col": None if cid is None else f"col{cid}",
                        "resultado": "NAO_VERIFICADO",
                        "motivo": "pessoa_ou_data_ausente",
                    })
                    continue

                for texto in datas:
                    base = {"caso": caso, "col": f"col{cid}", "dia": texto}
                    try:
                        dia = dt.date.fromisoformat(texto)
      

[truncado em 20000 de 24415 caracteres]

## 17/11 08:09 · sessao 1b51d1bc7272 · CODEX-RESERVA-R5 · leitura · origem=Code

**São 43 obras PARADA fora do balde ESPINHA-E: 19 LATERAL-ESTRUTURA, 14 INSTRUMENTO, 9 UI e 1 de 20/20.** A contagem considera IDs distintos nas tabelas principais `| id | balde |`, com estado iniciado por `PARADA`; exclui complementos, obsoletas e a lista de raias do rodapé. Esse é o critério do parser da GRADE. Portanto, **22 não corresponde à contagem desta cópia**. Se “espinha” também incluir o balde 20/20, o recorte fica em **42**. (bin/placar_grade.py:69; bin/placar_grade.py:80; bin/placar_grade.py:118; app/docs/GRADE-MESTRA.md:527; app/docs/GRADE-MESTRA.md:1757)

Abaixo, referências sem diretório pertencem a **`app/docs/`**. **DESTRAVADA-JA** significa que a trava nominal foi resolvida ou que a obra já pousou/fechou; pode restar implementação, revisão ou baixa documental. A cópia é preparada por `git archive`; **`git log` e ancestralidade dos hashes: [não verificado]**. Os hashes citados são os registrados nos arquivos, sem consulta à árvore viva. (CLAUDE.md:344) [nao verificado]

| obra | linha GRADE | trava | destrava | conferido HEAD | classe |
|---|---|---|---|---|---|
| HE-DE-14H-NUM-DIA — col945 12/09 | GRADE-MESTRA.md:177 | `! REGEN-EM-EXPORTADA` para o gravado da 09 | `!` específico para o ato na exportada | **Existe para o gravado.** Causa já lida; HEAD documentado já calcula HE zero. A 09 continua condicionada ao `!`; valor original de produção não foi reproduzido integralmente. (RELATO.md:120) | **ESPERA-RONALD** |
| BUG-ANCORA-NAO-REGENERA | GRADE-MESTRA.md:178 | Decisão sobre col824 e col899 | Encaminhar o resíduo ao cadastro/admin | **Resolvida:** FECHADA pelo CORTE B em 16/11; pendência marcada respondida, nenhum vínculo alterado. Signal da cura permanece presente. (BACKLOG.md:1208; RELATO.md:133; PENDENTES_RONALD.json:295; app/escala/signals.py:113) | **DESTRAVADA-JA — baixa** |
| O-GEO-DECISAO | GRADE-MESTRA.md:183 | Fecho do ESPELHO-VERDADE; O198 antes do complemento | Fechar ESPELHO-VERDADE; depois O198 para GEO-SILENCIO | **Existe.** Portão reafirmado; V0 em voo, V1–V4 registradas. O198 conserva ordem de parar após o censo. (RELATO.md:123; GRADE-MESTRA.md:889; GRADE-MESTRA.md:893; BACKLOG.md:1544) | **ESPERA-OBRA — ESPELHO-VERDADE; O198 no complemento** |
| O99 | GRADE-MESTRA.md:190 | Decisão sobre 61 dias; TXT | CORTE B: resíduo ao cadastro/admin | **Resolvida como obra:** FECHADA em 16/11. A linha ainda conserva esperas históricas de cura1/TXT, mas o fechamento posterior é explícito. (BACKLOG.md:1342; RELATO.md:133) | **DESTRAVADA-JA — baixa** |
| LOTE-1-PENDENTES | GRADE-MESTRA.md:193 | Autorização da troca; decisões dos itens 2/5; cadastro do item 3 | Respostas/`!` dos itens 2, 4 e 5; dado cadastral do item 3 | **Preservada:** só item 1 feito; demais atos continuam condicionados às respostas. Item 3 também depende do fim da ausência #3186. (BACKLOG.md:1369) | **ESPERA-RONALD**, com dado adicional |
| O28 | GRADE-MESTRA.md:195 | DIFF na aberta; paga ao DP; `!` depois do número | Medir DIFF e obter `!` nominal | **Existe.** Corte C restringiu o escopo, mas o registro diz “nada medido”; não equivale ao `!` do ato. (BACKLOG.md:1399; RELATO.md:125) | **ESPERA-RONALD**, precedida da medida |
| O58 | GRADE-MESTRA.md:198 | Parecer jurídico para hash; Card 4 | Parecer para hash; registro de emissão pode avançar separadamente | **Parcial:** fonte mantém exigência do parecer e libera o registro independentemente dele. Parecer posterior recebido: **[não verificado]**. (BACKLOG.md:1433) | **ESPERA-DADO — parecer jurídico** |
| O147 | GRADE-MESTRA.md:201 | `!`; REGEN-EM-EXPORTADA | Medir efeito financeiro dos três atrasados e apresentar para decisão | **Preservada na fonte:** “não começada”, primeiro o número, depois a decisão sobre a 09. Medida/aval posterior: **[não verificado]**. (BACKLOG.md:1492) | **ESPERA-RONALD**, precedida da medida |
| O175 | GRADE-MESTRA.md:204 | Corte sobre exportada/paga; censo para construção | Regra já definida: recusar exportada/paga; exceção com `!` nominal | **Corte resolvido; defeito presente.** Comando e núcleo ainda recomputam sem essa guarda. Pode retomar censo/brief; não é autorização para apply. (BACKLOG.md:1521; RELATO.md:121; app/ponto/management/commands/materializar_turnos.py:16; app/ponto/nucleo.py:40) | **DESTRAVADA-JA — preparação/construção condicionada ao censo** |
| CARIMBO-POR-ORIGEM | GRADE-MESTRA.md:225 | Censo no brief; atrás do ESPELHO-VERDADE | Fecho do espelho e censo dos modelos/escritores | **Existe o portão de obra.** Desenho já registrado: corrigir save/QuerySet na origem. Exemplo do problema permanece: `reabrir` salva sem atualizar o carimbo. (BACKLOG.md:1695; app/ponto/services/fechamento.py:1354; GRADE-MESTRA.md:889) | **ESPERA-OBRA — ESPELHO-VERDADE** |
| EMPATES-SEM-VIGENCIA | GRADE-MESTRA.md:226 | Censo no brief; atrás do ESPELHO-VERDADE | Fecho do espelho, censo e competência de referência definida | **Existe.** Desenho registrado, porém contador ainda filtra apenas `ativo`; juiz filtra vigência. A competência de referência continua indicada como pergunta dele. (BACKLOG.md:1698; app/core/services/aplicacao_convencao.py:60; app/core/regua_cct.py:259) | **ESPERA-OBRA — ESPELHO-VERDADE**, com decisão residual |
| GRAVADO-NAO-CARIMBA-A-REGUA | GRADE-MESTRA.md:229 | Censo no brief; atrás do ESPELHO-VERDADE | Fecho do espelho e censo; carimbo no ato da lavra | **Portão permanece.** Decisão de desenho já registrada; a fonte conserva a medição de 0/17 competências nomeáveis pela autoridade, sem cura. Medida atual desses 17: **[não verificado]**. (BACKLOG.md:1721; GRADE-MESTRA.md:889) | **ESPERA-OBRA — ESPELHO-VERDADE** |
| ABRANGENCIA-DOS-QUATRO-INSTRUMENTOS-SEM-PDF | GRADE-MESTRA.md:232 | Chegada dos PDFs | Receber os quatro instrumentos e conferir a base territorial | **Preservada na fonte:** declara expressamente que não fecha por nova medição, mas pela chegada dos PDFs. Recebimento posterior: **[não verificado]**. (BACKLOG.md:1736) | **ESPERA-DADO — PDFs** |
| JANELA-DO-NOTURNO-CRAVADA | GRADE-MESTRA.md:234 | `!` para zona inviolável | Aval nominal para alterar o motor e tornar a janela cadastrável | **Existe.** Janela continua constante 22h–05h no motor; fonte exige aval para esse sítio. Aval específico posterior: **[não verificado]**. (app/ponto/motor_calculo_v2.py:44; app/ponto/motor_calculo_v2.py:45; BACKLOG.md:1738) | **ESPERA-RONALD** |
| SEM-CELULA-11-DEVE-TER-CELULA | GRADE-MESTRA.md:243 | `!` para col305 | Decisão sobre o vínculo do col305 | **Resolvida:** `! COL305=(b)` aplicado em 25/10, desativando ec965. Remedição de 16/11 confirma ausência de vínculo cobrindo a 11 e conclui “nada a aplicar”. (RELATO.md:3982; RELATO.md:130) | **DESTRAVADA-JA — baixa** | [nao verificado]
| LAUDO-col33-0310 | GRADE-MESTRA.md:250 | Corte sobre o restante da frota | Corte REJULGAR-COMP10-RESTO-DA-FROTA | **Resolvida:** corte dado em 27/10; CORTES registra atos aplicados e lista HE regenerada. Resíduo col988 ficou em obra própria. (CORTES.md:360; CORTES.md:102; BACKLOG.md:839) | **DESTRAVADA-JA — reconciliar fechamento/resíduo** | [nao verificado]
| H12H06-SEPARA-E-CRONTAB | GRADE-MESTRA.md:257 | Censo de trava; desenho da parte B | Resposta H12H06-B-SIMPLES e pouso da B | **Resolvida:** pergunta respondida, B pousada e aplicação das 28 linhas registrada. O comando correspondente está nesta cópia. (RELATO.md:2401; RELATO.md:2324; RELATO.md:3107; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:2) | **DESTRAVADA-JA — baixa da espera antiga** |
| BO-0611-05 | GRADE-MESTRA.md:270 | Liberação B3; revisão; smoke | LIBERA-B3, certificação e pouso | **PARADA vencida:** corte LIBERA-B3 registrado; opus aprovou, merge `fb9269ea9`, deploy e push registrados. Smoke a/b/d continuava devido; isso não desfaz o pouso já realizado. (CORTES.md:433; RELATO.md:9354; RELATO.md:9368) | **DESTRAVADA-JA — pousada, com smoke residual** |
| SETNULL-ORFA-DNA | GRADE-MESTRA.md:280 | Censo no brief; atrás do ESPELHO-VERDADE | Fecho do espelho e deleção de EC pela porta da célula | **Portão existe; desenho já dado.** Relação ainda usa `SET_NULL`, mantendo presente o sítio descrito no achado. (BACKLOG.md:2259; app/escala/models.py:1692; GRADE-MESTRA.md:889) | **ESPERA-OBRA — ESPELHO-VERDADE** |
| SELOS-CANON-I2 | GRADE-MESTRA.md:292 | SELOS-CANON-I3 | Pouso do I3 | **Existe:** I3 conserva matriz/volta 4 e certificações/pouso pendentes. (BACKLOG.md:805; RELATO.md:1770) | **ESPERA-OBRA — SELOS-CANON-I3** | [nao verificado]
| CONTENCAO-QUE-MORDE-V2 | GRADE-MESTRA.md:304 | Contrato 20; contrato novo | Fechar contrato 20 e apresentar novo contrato de cinco linhas | **Existe:** estacionamento preservado; contrato “chamado, um juiz” ainda não está marcado verde e conserva dependências O168/O169/LASTRO. (BACKLOG.md:977; RELATO.md:2763; app/core/contratos_estruturais.py:262; app/core/contratos_estruturais.py:270) | **ESPERA-OBRA — contrato 20**, depois contrato novo |
| MAIS-CODEX-PLACAR | GRADE-MESTRA.md:331 | Duas curas de produto no ar | Segunda cura e encerramento da moratória | **Resolvida expressamente:** moratória caiu com `9ed75130` + `77eb28ce`; placar voltou à fila sob CONTRATO-DE-INSTRUMENTO. (RELATO.md:3463) | **DESTRAVADA-JA** |
| GATE-EXPORT-10 | GRADE-MESTRA.md:337 | Decisão do DP sobre HE; autorização | Decisões/`!`, relavrar carimbo e executar o ato | **Fonte ainda bloqueia:** registro específico conserva 184 dias sem decisão. Estado atual desses dias **[não verificado]**; há contradição posterior entre “10 exportada” no aval e juiz dizendo não entregue. (BACKLOG.md:1185; RELATO.md:4038; RELATO.md:130) | **ESPERA-RONALD — conservadora** |
| ~~PROVA-DOMINIO~~ | GRADE-MESTRA.md:339 | Execução cortada | Baixa para estado terminal | **Já CORTADA**, fora da fila desde 28/09; fonte manda “Não executar”. (BACKLOG.md:1207) | **DESTRAVADA-JA — somente baixa para CORTADA** |
| HIGIENE-DE-CONTEXTO | GRADE-MESTRA.md:350 | Autorização do item 5, CLAUDE→LAPIDES | Aval para mover a história | **Espera antiga superada:** CLAUDE-MD-ENXUTO recebeu aval e pousou `9ce5181d`, com história em LAPIDES e pistas em PISTA. Meta integral de tamanho não fechou e foi parada pelo aval. (BACKLOG.md:1050; BACKLOG.md:1357; RELATO.md:2879) | **DESTRAVADA-JA — remover espera antiga** |
| O64 | GRADE-MESTRA.md:357 | Investigação do ambiente; `!` | Explicar divergência prod/sombra; depois autorizar apply | **Preservada na fonte:** 10 em produção contra 14 na sombra, sem causa definida. Repetição atual ou solução posterior: **[não verificado]**. (BACKLOG.md:1440) | **ESPERA-DADO — causa da divergência**, depois `!` |
| O91 | GRADE-MESTRA.md:360 | `!` de instalação | Reconciliar com corte “não instala” | **Espera contradita pelo próprio cabeçalho:** FECHADA pelo corte “não instala”, enquanto a célula antiga ainda pede `!`. Isso não autoriza instalar. (BACKLOG.md:1467) | **DESTRAVADA-JA — baixa, sem instalação** |
| O176 | GRADE-MESTRA.md:371 | Corte para conferir conteúdo do pacote | Corte sobre o critério por conteúdo | **Existe:** fonte mantém ESPERA CORTE; código ainda lê títulos e verifica presença do rótulo. (BACKLOG.md:1522; app/core/esteira_vigia.py:296; app/core/esteira_vigia.py:312) | **ESPERA-RONALD** |
| O279 | GRADE-MESTRA.md:388 | Liberação ou cura da regra operacional | Resolver a restrição que impediu corrigir o religador | **Código antigo presente:** `while true` e detecção textual de `PAREI`. Restrição operacional atual do agente: **[não verificado]**. (BACKLOG.md:1655; bin/keepalive.sh:3; bin/keepalive.sh:7) | **ESPERA-RONALD** |
| O296 | GRADE-MESTRA.md:393 | `!` para sítio na zona inviolável | `!` nominal para o comentário no motor | **Fonte preserva a espera** para o décimo sítio; nove externos podem ser separados. Contagem atual e aval posterior: **[não verificado]**. (BACKLOG.md:1673) | **ESPERA-RONALD** |
| PROTEGE-VAZIA-EM-55-LEIS | GRADE-MESTRA.md:403 | Autorização para alterar LEIS.md | Autorizar movimentação entre colunas existentes | **Defeito presente:** L-006 tem sítios em DONO e PROTEGE vazio; selo ainda pula célula vazia. Total atual “55”: **[não verificado]**. (BACKLOG.md:1724; LEIS.md:64; bin/tests/test_lei_protege_sitio.sh:77) | **ESPERA-RONALD** |
| RESIDUO-PREMIG-NO-BIND-MOUNT | GRADE-MESTRA.md:404 | Autorização para apagar | Decisão nominal sobre os arquivos do bind mount | **Autorização continua exigida na fonte.** Os 64 arquivos foram descritos como ignorados pelo git; presença atual no bind mount **[não verificado]** nesta cópia arquivada. (BACKLOG.md:1730) | **ESPERA-RONALD**, com existência atual não verificada |
| PAUTA-PREFIXO-IDENTIDADE-NO-TEXTO | GRADE-MESTRA.md:445 | Corte de desenho; congelamento | Decidir desenho da identidade textual | **Congelamento preservado; defeitos presentes:** motivo vazio retorna normalmente; identidade textual continua cortada em `[:480]`. (BACKLOG.md:2173; app/core/management/commands/alarme_esteira.py:34; app/core/management/commands/alarme_esteira.py:56) | **ESPERA-RONALD** |
| O256 | GRADE-MESTRA.md:476 | Porta PDF; O261; smoke final | PDF publicado, O261 curada; smoke como revisão final | **Travas de avanço vencidas:** rota PDF existe; O261 consta fechada; O310 e lente pousaram. Corte diz explicitamente que smoke do retrato não segura push. Smoke positivo posterior aos últimos pousos: **[não verificado]**. (app/folha/urls.py:24; BACKLOG.md:1652; BACKLOG.md:1687; BACKLOG.md:1696; CORTES.md:65) | **DESTRAVADA-JA — revisão de smoke residual** |
| AVISO-ENTRADA-ANTECIPADA | GRADE-MESTRA.md:477 | Descongelamento pela L-096 | Aval que alcance o serviço Python ou encerramento do congelamento | **Existe:** BACKLOG mantém CONGELADA. Exceção da raia UI cobre templates/static/testes, enquanto esta entrega inclui `aviso_da_batida.py`. (BACKLOG.md:1212; CLAUDE.md:311; LEIS.md:114) | **ESPERA-RONALD** | [nao verificado]
| AVISO-ESPURIA-JANELA — desenho | GRADE-MESTRA.md:478 | Confirmação do admin da folha e do dono | Confirmação de ambos sobre janela e mensagem | **Preservada:** “REGISTRADO, NAO CONSTRUIR”, aguardando confirmação. Resposta posterior: **[não verificado]**. (BACKLOG.md:1213) | **ESPERA-RONALD**, também admin |
| BUG-APP-SESSAO-401 | GRADE-MESTRA.md:479 | Autorização; smoke nas duas cascas | `!` e smoke para a metade APP | **Preservada:** admin no ar, APP aguardando `!`/smoke. Autorização e smoke posteriores: **[não verificado]**. (BACKLOG.md:1238) | **ESPERA-RONALD** |
| AVISO-DO-DONO | GRADE-MESTRA.md:481 | Portão da 09; descongelamento; smoke no celular | Liberação específica e smoke do push | **Congelamento explícito permanece.** Exceção de UI não cobre automaticamente o emissor de push por evento. Portão da 09 cumprido e smoke específico: **[não verificado]**. (BACKLOG.md:1254; CLAUDE.md:311) | **ESPERA-RONALD** | [nao verificado]
| F2 | GRADE-MESTRA.md:486 | Contratos 22/22; O157 | Fecho estrutural e O157 | **Existe, com teto antigo:** teto atual é 20. O157 não começou; código ainda conta somente `tipo='falta'`, sem a suspensão exigida pelo corte. (app/core/contratos_estruturais.py:1041; BACKLOG.md:1501; app/ferias/models.py:144) | **ESPERA-OBRA — O157 + fecho estrutural** |
| O30 | GRADE-MESTRA.md:487 | Vale F1; `pillow_heif`; rebuild | Rebuild HEIC e liberação do complemento F3 POS-20 | **Parcial:** vale F1 já dado em 03/10, “A F2 ABRE”. F3 continua POS-20, dependente do decodificador/próximo rebuild. Rebuild concluído: **[não verificado]**. (PENDENTES_RONALD.json:2093; RELATO.md:1112; BACKLOG.md:1401) | **ESPERA-OBRA — rebuild HEIC / F3 POS-20** |
| O206 | GRADE-MESTRA.md:493 | Correção da prévia; smoke das duas cascas | Prévia pela autoridade e smoke aceito | **Resolvida:** template lê texto da autoridade; comunicação aprovada em 17/10. Restos de push nativo/disputa foram separados na O311. (app/templates/chamados/partials/_js_cobranca.html:30; PENDENTES_RONALD.json:2360; BACKLOG.md:1688) | **DESTRAVADA-JA** |
| O310-RAIOX | GRADE-MESTRA.md:499 | Corte para história, modelo e migration | Corte específico para histórico | **Existe para o complemento:** lente já NO AR `3ea519d7`, mas fonte mantém “história (modelo + migration) é corte dele”. Corte posterior: **[não verificado]**. (BACKLOG.md:1696) | **ESPERA-RONALD** |
| E3-ENTRADA-ORFA-SEM-TURNO — balde 20/20 | GRADE-MESTRA.md:121 | Lei para dia sem turno com batida | Corte sobre a entrada real órfã sem turno previsto | **Preservada:** fonte distingue esse caso da lei do dia sem nada; ainda aparece na fila como pergunta de lei. Corte posterior: **[não verificado]**. (BACKLOG.md:2176; RELATO.md:1729) | **ESPERA-RONALD** |

**Totais da classificação acima**, mantendo o recorte das 43 linhas principais e as ressalvas de cada linha. (GRADE-MESTRA.md:121; GRADE-MESTRA.md:177; GRADE-MESTRA.md:280; GRADE-MESTRA.md:292; GRADE-MESTRA.md:445; GRADE-MESTRA.md:476; GRADE-MESTRA.md:499)

| balde | DESTRAVADA-JA | ESPERA-RONALD | ESPERA-OBRA | ESPERA-DADO | total |
|---|---:|---:|---:|---:|---:|
| LATERAL-ESTRUTURA | 7 | 5 | 5 | 2 | **19** |
| INSTRUMENTO | 4 | 7 | 2 | 1 | **14** |
| UI | 2 | 5 | 2 | 0 | **9** |
| 20/20 | 0 | 1 | 0 | 0 | **1** |
| **Total** | **13** | **18** | **9** | **3** | **43** |

## 17/11 08:08 · sessao 320413038536 · - · - · origem=Ronald (tmux)

Confirmado: corrija L2 para **MUDOU — contagem incorreta**, sem atribuir isso a uma obra posterior. Há **18 funções com save/create/get_or_create**, mais **1 função com delete**, total **19** incluindo exclusão; os sítios adicionais à amostra são `agendar_ferias_porta`, `app/ferias/services.py:316`; `aplicar_art130`, `app/ferias/services.py:412`; e `excluir_periodo_aquisitivo`, `app/ferias/services.py:876`.

Esses sítios podem já existir no momento do inventário; novidade temporal/commit [nao verificado]. [nao verificado]

Delta das perguntas 1–8:

| pergunta | conferência atual |
|---|---|
| 1 — cancelamento iniciado | Permanece: botão por data `app/ferias/models.py:340`, botão por status `app/templates/ferias/saldo.html:70`, porta aceita `em_gozo` `app/ferias/services.py:652`; fila/lei explicitada em `app/docs/RELATO.md:842`. |
| 2 — PA em_curso pode agendar | Permanece: `pode_agendar` exige adquirido `app/ferias/models.py:234`; porta normal não exige esse estado `app/ferias/views.py:245`, `app/ferias/services.py:578`; solicitação exige `app/ferias/services.py:765`. |
| 3 — vencendo/vencido | Permanece: juiz por data `app/ferias/models.py:260`, filtros gravados/literal 60 `app/ferias/views.py:64`, `app/ferias/services.py:263`; RC1-2 sem construtor `app/docs/BACKLOG.md:759`. |
| 4 — confirmado_em na recusa | Permanece: recusa salva somente confirmação `app/ferias/services.py:908`; sentido histórico ou vigente [nao verificado]. |
| 5 — guardas sob trava | Permanece: guardas externas `app/ferias/views.py:945`, `app/ferias/views.py:994`; escritores só vetam cancelado `app/ferias/services.py:887`, `app/ferias/services.py:905`. |
| 6 — SOL sem nascente | Criação produtiva não identificada; leitura permanece `app/ferias/views.py:41`, `app/ferias/views.py:785`; canal externo/dado existente [nao verificado]. |
| 7 — relógio residual | Permanece `app/ferias/services.py:654`, `app/ferias/services.py:681`, `app/ferias/services.py:686`, `app/ferias/services.py:934`, `app/ferias/views.py:29`, `app/ferias/views.py:117`; residual explicitamente em fila `app/docs/RELATO.md:847`. |
| 8 — comentário concluído | Permanece inexato `app/ferias/models.py:304`; troca de janela e cron podem mover concluído `app/ferias/services.py:721`, `app/ferias/services.py:954`. |

## 17/11 08:07 · sessao 0f8ca87488d3 · CODEX-RESERVA-R2 · leitura · origem=Code

`D` = arquivo e linha **nova** no diff anexado; `C` = arquivo e linha na cópia. Análise por leitura; execução, morte de mutantes e identidade do HEAD Git: **[nao verificado]**. Os pontos abaixo são encaminhamentos para a entrega parcial.

| achado | classe (1-5 ou extra) | arquivo:linha | gravidade | o que o construtor deve fazer |
|---|---|---|---|---|
| **Resposta corrigida fica validada na leitura nova.** A porta humana limpa a validação anterior e registra RESPOSTA; o redutor conserva aquela validação. `estado`, `_ancoradas` e `q_vivas` passam a tratar a correção como já validada. | 2 / extra: derivação paralela | D `app/chamados/models.py:4186`, `:4213`; C `app/chamados/services/disputa_emissao.py:1484-1496`, `:1523-1526` | **Alta** | RED pela porta real: VALIDACAO → resposta corrigida → leitura sem validação, estado RESPONDIDA e retorno aos universos pertinentes. Corrigir redutor e face SQL juntos. **Não basta adicionar toda RESPOSTA aos limpadores:** a porta automática preserva validação em C `app/chamados/services/disputa_emissao.py:1674-1679`. |
| **Ressuscitação limpa a projeção, mas a leitura recupera a validação preservada na trilha.** O próprio controle novo começa com validação, que a porta transforma em linha antes de zerar os campos. A leitura proposta devolve essa validação, contrariando o controle. | 2 / extra | D `app/chamados/models.py:4203-4221`; D `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:296-318`; C `app/chamados/models.py:1621-1622`, `:1632-1640` | **Alta** | Resolver a representação desse efeito antes de certificar conservação. Respeitar a decisão já documentada de não transformar indiscriminadamente ressuscitação em REABERTURA. Falha do controle prevista por leitura; execução **[nao verificado]**. |
| **`q_vivas` perde a continência do C6; `_ancoradas` pode retirar a pergunta antes do tripwire.** Contraexemplo: VALIDACAO somente na sequência, coluna nula e célula acusatória. C6 diz viva; os filtros novos excluem. | 2 | D `app/chamados/models.py:351`, `:3553`; C `app/chamados/juizes.py:1389`, `:1404-1406`, `:1454-1457`; C `app/chamados/models.py:223-226`, `:3475-3481` | **Alta** | RED conjunto C6/SQL e RED de fechamento com essa pergunta ancorada. Resolver a fronteira F2/C6 antes de estreitar os universos. |
| **Foto da RETRATACAO muda de fonte sem RED que distinga ato de projeção.** O teste existente exige texto da projeção; o arquivo novo não acrescenta cenário divergente para essa foto. | 1 | D `app/chamados/models.py:2164`; D `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:168-203`; C `app/chamados/models.py:2147-2153`; C `app/chamados/tests/test_porta_remocao_motivo.py:373-381` | **Média** | Criar cenário com texto vigente somente no ato e outro com resposta reaberta/projeção residual. Exigir texto, ator e ato correto. A reversão para `resposta_colab` deve falhar por asserção. |
| **Selo dos sete leitores conserva comportamento, mas não prova a migração de fonte.** `_mordida` mantém instante/texto alinhados na projeção, com ou sem linha. Reverter esses leitores para a base continua atendendo esse cenário, por leitura. | 3 | D `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:236-294`; C `app/chamados/models.py:1933-1936`, `:3507-3509`, `:3674-3731` | **Alta para certificação** | Manter a caracterização e acrescentar divergência ato/projeção por leitor. Medir reversão individual no código da base e na entrega; não contabilizar esse cenário alinhado como prova da fonte nova. |
| **M15 não distingue morte do mutante na base.** Seu único teste já falha sem mutante porque o SILENCIO da base fotografa os brancos residuais. | 3 | D `app/chamados/tests/test_cortes_f2_f9_m15.py:38-58`; C `app/chamados/models.py:2015-2017` | **Média** | Acrescentar controle verde na base, sem resíduo, que alcance SILENCIO e seja mordido pelo mutante. Registrar asserção efetivamente atingida; medição atual **[nao verificado]**. |
| **M4/M13/M14/M18: nada adicional por leitura quanto ao alcance dos controles.** M4 alcança a queda final; M13 preserva âncora; M14 observa carimbo da lavra real; M18 observa o argumento antes do retorno do juiz. | 3 | D `app/chamados/tests/test_cortes_f2_f9_m4.py:58`; D `app/chamados/tests/test_cortes_f2_f9_m13.py:47`; D `app/chamados/tests/test_cortes_f2_f9_m14.py:58`; D `app/chamados/tests/test_cortes_f2_f9_m18.py:56-75`; C `app/chamados/models.py:3169`, `:344`, `:309-312`, `:1548-1564`; C `app/chamados/veredito_lavrado.py:270-275` | — | Medir os controles e mutantes na base identificada. Aplicabilidade e mordida são plausíveis por leitura; execução no HEAD **[nao verificado]**. |
| **O selo exige seis validadas, mas seus casos produzem sete pela própria regra.** São os casos 1, 2, 4, 5, 7, 8 e 9. | 3 / 4 | D `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:127-150` | **Média** | Corrigir a expectativa após conferir os casos. Não tratar essa falha de contagem como mordida de negócio. |
| **Fixture exige SILENCIO de pergunta já validada.** C6 a exclui de `vivas`; `fechar` registra SILENCIO somente nas vivas. O `.get(SILENCIO)` tende a terminar em `DoesNotExist`. | 3 | D `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:188-203`; C `app/chamados/services/validacao.py:168`; C `app/chamados/juizes.py:1404-1406`; C `app/chamados/models.py:1986-1987`, `:2009-2017` | **Média** | Corrigir a fixture para atingir o ato pretendido e conferir suas precondições. Execução **[nao verificado]**. |
| **Comentário afirma reproduzir o efeito dos escritores, mas RESPOSTA tem efeitos diferentes conforme a porta.** `_ancoradas` também conserva docstring explicando o universo antigo por coluna nula. | 4 | D `app/chamados/models.py:4182-4186`; C `app/chamados/services/disputa_emissao.py:1494-1496`, `:1674-1679`; C `app/chamados/models.py:339`; C `app/docs/JUIZES.md:38-39` | **Média / baixa** | Resolver a diferença semântica e ajustar a afirmação universal; atualizar a definição de `_ancoradas`. |
| **Dia exato × intervalo: nada no trecho novo examinado.** Igualdade e `gt` novos são correspondência de instante e ordenação de atos, com empates explicitados. | 5 | D `app/chamados/models.py:3493-3503`; D `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:77-90` | — | Conservar testes dos dois empates. Guardas de outros módulos e efeitos em produção **[nao verificado]**. |
| **Segundo juiz: C21 continua delegado; a divergência demonstrada está na nova derivação de validação frente aos escritores e ao C6.** | extra | D `app/chamados/models.py:3721-3728`, `:4186-4221`; C `app/docs/JUIZES.md:38-39`, `:102-110`; C `app/chamados/juizes.py:1552`, `:1404-1406` | **Alta**, pelos contraexemplos acima | Fechar a semântica das duas faces com os escritores existentes. Não usar apenas paridade SQL/redutor como prova: ambos podem conservar a mesma validação indevida. |
| **Segundo escritor fora da porta e relógio novo fora da borda: nada encontrado no diff examinado.** A sequência continua escrita por `registrar`; a leitura ordena instantes recebidos. | extra | D `app/chamados/models.py:4203-4221`; C `app/chamados/models.py:4123-4137`; C `app/chamados/juizes.py:1382-1400` | — | Preservar a porta e explicitar a fronteira entre leitura atual e projeção temporal do C6. |

| achado — quem mais lê e coerência | classe (1-5 ou extra) | arquivo:linha | gravidade | o que o construtor deve fazer |
|---|---|---|---|---|
| **Leitores de `_ancoradas`:** todos herdam a exclusão nova antes de consultar C6. | 2 | D `app/chamados/models.py:351`; C `app/chamados/models.py:223`, `:334`, `:381`; C `app/chamados/services/disputa_emissao.py:1821`, `:1962` | **Alta** | Selar cada caminho de fechamento/reabertura com validação somente no ato; C6 posterior em C `app/chamados/models.py:368` não recupera pergunta previamente excluída. |
| **Leitores de `q_vivas`:** todos herdam o estreitamento; filtro C6 posterior não recupera a linha eliminada pelo SQL. | 2 | D `app/chamados/models.py:3553`; C `app/chamados/juizes.py:1763`; C `app/chamados/services/fio.py:256`; C `app/chamados/services/janela_colab.py:63`; C `app/chamados/services/raiox.py:74`; C `app/chamados/services/pergunta_no_app.py:53`, `:61`; C `app/chamados/services/vigia_fio.py:711`, `:718`; C `app/chamados/services/cobranca.py:237`, `:241`; C `app/ponto/services/veredito_celula.py:181` | **Alta** | Conferir fila, janela, crédito, cobrança, auditoria e fechamento com o contraexemplo C6/SQL indicado acima. |
| **`estado`:** API, fio, vigia, explicador e card herdam a leitura nova. O explicador classifica pelo ato e descreve o texto cru. | 2 | D `app/chamados/models.py:3176`; C `app/api/views.py:2550`; C `app/chamados/services/fio.py:660`; C `app/chamados/services/vigia_fio.py:259`; C `app/chamados/services/explicador.py:95`, `:102-103`; C `app/templates/chamados/partials/_card_atendimento.html:189` | **Média** | Conferir classificação, texto e ação na mesma fixture divergente. |
| **`respondida`:** templates seguem a propriedade nova; declaração ainda usa juiz/texto crus antes de passar a propriedade ao core. | 2 | D `app/chamados/models.py:3728`; C `app/chamados/services/declaracao_texto.py:418-420`; C `app/templates/chamados/partials/modal_fio.html:185`, `:217`; C `app/templates/chamados/partials/_pergunta_card.html:24`, `:29` | **Média** | Selar declaração repetida/corrigida e ação apresentada com resposta somente na sequência. |
| **Tela anuncia resposta, porta recusa por ausência na projeção.** Validação e recusa continuam lendo o par antigo; filas também permanecem na face SQL da projeção. | 2 | D `app/chamados/models.py:3176`, `:3728`; C `app/chamados/services/validacao.py:682-687`, `:898-905`, `:1253`; C `app/chamados/juizes.py:1580`, `:1756`, `:1763`; C `app/api/views_mensageria.py:540` | **Alta — pendência da parcial** | Conferir o fluxo completo apresentação → fila → validar/recusar. Com resposta apenas no ato, a porta atual retorna `sem_resposta`/`ja_pendente`. |
| **`resposta_display`:** APIs misturam display do ato com texto/instante da coluna; cards usam fallback que pode repor texto residual. | 2 | D `app/chamados/models.py:3750`; C `app/api/views.py:1163-1165`, `:2561-2563`; C `app/templates/chamados/partials/_card_atendimento.html:106`, `:143`, `:183`, `:391`; C `app/templates/chamados/partials/_pergunta_card.html:31`; C `app/chamados/services/disputa_emissao.py:1560` | **Média** | Conferir payload, data e texto exibidos após REABERTURA com projeção residual; revisar os fallbacks. |
| **Consumidor de teste de display muda de resultado:** constrói pergunta sem instante e espera rótulo/texto; a leitura nova não sintetiza resposta nesse caso. | 2 | D `app/chamados/models.py:4171`, `:3750`; C `app/chamados/tests/test_valor_rotulo.py:7`, `:12`, `:16` | **Média** | Definir o contrato de display para objeto sem instante e ajustar fixture ou comportamento conforme esse contrato. Execução **[nao verificado]**. |
| **`sem_veredito_entre`: delegações preservadas**, incluindo CSF e irmãs. `respostas_sem_veredito` transmite o predicado novo aos demais leitores. | 2 | D `app/chamados/models.py:1938-1950`; C `app/chamados/models.py:1916`, `:1944`, `:1977`; C `app/ponto/supra_juiz.py:573-574`; C `app/chamados/juizes.py:2117`; C `app/chamados/reconciliador.py:1166`; C `app/chamados/services/fila_por_causa.py:226`; C `app/chamados/services/competencia_trancada.py:126`; C `app/colaboradores/services/ficha.py:169`; C `app/colaboradores/services/desligamento.py:165`; C `app/chamados/management/commands/validar_classe_a.py:54` | **Alta para efeitos herdados** | Acrescentar divergência por validação/resposta nesses consumidores. A ficha ainda particiona “sem resposta” pelo juiz cru em C `app/colaboradores/services/ficha.py:172`. |
| **Cascata das irmãs alcança retenção, materialização, reabertura e auditoria.** O selo novo mede a lista de irmãs, mas não esses efeitos finais. | 2 | D `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:236-244`; C `app/chamados/reconciliador.py:449`; C `app/chamados/services/materializacao.py:1247`, `:1256`; C `app/chamados/services/disputa_emissao.py:2031`, `:2033`; C `app/chamados/management/commands/auditar_invariantes_chamados.py:986` | **Alta — pendência da parcial** | Exigir efeito no chamado e trilha, além da lista de PKs. |
| **Cascata omitida na reprova B2 continua relevante:** retenção chega à aprovação de justificativa, liberação de bloqueio e fechamento retroativo. | 2 | D `app/chamados/models.py:1938-1950`; C `app/chamados/reconciliador.py:494-498`, `:520-522`; C `app/ponto/services/justificativa.py:46`, `:87`, `:400` | **Alta — pendência da parcial** | RED com ato/projeção divergentes em cada entrada; conferir chamado retido/fechado e trilha da retenção. |
| **Outra foto de SILENCIO permanece na projeção**, enquanto a foto de `DisputaSupervisao.fechar` migra. | 2 / extra | D `app/chamados/models.py:2032`; C `app/ponto/services/veredito_celula.py:195-197`; C `app/chamados/models.py:2015-2017` | **Média — pendência da parcial** | Conferir coerência das duas portas com a mesma resposta vigente. Não afirmar migração integral das fotos de SILENCIO. |
| **Checklist B2: nada a reapontar como ausência de selo da trilha L-130 na cópia.** Existem asserções de ator, antes/depois, rollback e lote com falha de trilha. | 1 / 3 | C `app/chamados/tests/test_f1nova_b2_leitores.py:430-487`; C `app/chamados/services/validacao.py:449-457`; C `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:103-113` | — | Usar esses testes na nova medição do mutante da conta. Morte efetivamente medida no HEAD **[nao verificado]**. |
| **Checklist B2: docstring antiga das irmãs e duplicação da face negada já estão corrigidas na cópia.** | 4 / extra | C `app/chamados/juizes.py:2105-2107`, `:1588-1591` | — | Não repetir os dois achados antigos como defeitos atuais; manter a conferência dos efeitos herdados listados acima. |

## 17/11 08:07 · sessao 3d1b593c61ec · - · - · origem=Ronald (tmux)

**COL369**

- **Pedido:** apurar (2) inversão que funciona em 23/09 e falha em 26–27/09; (3) horas trabalhadas com `minutos_realizados=0` sob Ausência 4538; (4) vínculo 1296 impossível versus 1313 sem folga. `app/docs/BACKLOG.md:1246`.
- **Estado:** agregado continua “ABERTO, nenhum medido”; grade “REGISTRADA”. Esse estado envelheceu: tipos foram medidos e fechados, vínculo foi decidido e pauta da folga aberta. `app/docs/BACKLOG.md:1246`, `app/docs/GRADE-MESTRA.md:182`, `app/docs/BACKLOG.md:1368`, `app/docs/BACKLOG.md:1379`, `app/docs/BACKLOG.md:1380`.
- **Resposta (2), posterior:** **COL369-TIPOS-INVERTIDOS**, medida 02/10: pausa 59 minutos em 23/09 versus 73/64 em 26–27/09, diante de intervalo cadastrado de 60. O contrafactual explicou as 2h de intra e 2,29h trabalhadas; não explicou as 5,07h da troca de vínculo. Nenhuma cura foi aplicada nessa obra. `app/docs/RELATO-ARQUIVO.md:23347`, `app/docs/RELATO-ARQUIVO.md:23361`, `app/docs/RELATO-ARQUIVO.md:23370`, `app/docs/RELATO-ARQUIVO.md:23379`.
- **Resposta (2), código atual:** motor lê papel da ata contra tipo gravado quando a ata é legível; dias sem autoridade suficiente seguem a geometria. O pareador de pausa também aceita autoridade da ata/mista contra o tipo gravado. Portanto não transportar o diagnóstico numérico antigo para o código atual sem reler as células e batidas. `app/ponto/motor_calculo_v2.py:534`, `app/ponto/motor_calculo_v2.py:549`, `app/ponto/motor_calculo_v2.py:622`, `app/ponto/turnos.py:823`, `app/ponto/turnos.py:842`.
- **Resposta (3), posterior:** **BO-B DIAPAGO-SE-CONTRADIZ / B-B21**, em 23/10, corrigiu o mecanismo: antes o realizado de dia de ausência não entrava; agora trabalho conta, abono cobre apenas o restante do previsto. Isso responde a classe do achado, mas não comprova a situação gravada atual dos dias específicos de col369. A prova de apply diz que a competência 09 ficou intacta. `app/ponto/services/dia_pago.py:375`, `app/ponto/services/dia_pago.py:393`, `app/ponto/services/dia_pago.py:400`, `app/docs/RELATO.md:5468`, `app/docs/RELATO.md:5475`, `app/docs/CORTES.md:344`.
- **Resposta (4), posterior:** **COL369-VINCULO-1296** fechou com “NÃO TROCAR”; fica 1313 e 1296 inativo. A pauta **908** levou à supervisão a pergunta sobre dia de folga. O DIFF completo refutou a primeira previsão: troca isolada moveria −7,07h e removeria zero furos, porque células congeladas exigiriam regeneração separada. `app/docs/BACKLOG.md:1368`, `app/docs/BACKLOG.md:1379`, `app/docs/RELATO-ARQUIVO.md:26503`, `app/docs/RELATO-ARQUIVO.md:26535`, `app/docs/RELATO-ARQUIVO.md:26546`.
- **Próximo passo:** sonda abaixo para estoque atual; não executar troca nem regeneração. A decisão registrada é não trocar. `app/docs/BACKLOG.md:1368`.

**O236**

- **Pedido:** abrir por motivo os retidos, pela função real, para descobrir quantos a L-033 libera; os números 200/238/94 são o achado histórico, não medição atual. `app/docs/BACKLOG.md:1293`.
- **Estado:** “achado medido, não curado; abertura por motivo não feita”; grade “REGISTRADA”. `app/docs/BACKLOG.md:1293`, `app/docs/GRADE-MESTRA.md:185`.
- **Resposta atual:** há isenção explícita de pausa: exige exclusivamente `FURO_PARCIAL` e confirmação pelo juiz de que só marcos de pausa estão apagados. Dias mistos continuam retidos. O juiz hoje também dispensa desligados e isentos. `app/folha/export.py:1158`, `app/folha/export.py:1173`, `app/folha/export.py:1207`, `app/folha/export.py:1287`, `app/folha/export.py:1293`.
- **Obras posteriores:** **O254** implementou a primeira cláusula da L-033; **O259** criou o vigia dessa isenção. Isso supera a premissa “L-033 só no papel”, sem responder o censo atual por motivo dos antigos 200/238/94. `app/docs/RELATO-ARQUIVO.md:43115`, `app/docs/RELATO-ARQUIVO.md:43119`, `app/docs/RELATO-ARQUIVO.md:42950`, `app/folha/services/vigia_pausa.py:100`.
- **Prova/limite:** `com_dias=True` devolve motivos e dias pela própria função de retenção. CORTES registra que os números herdados 8/8, 664/580/58 não tinham lastro no arquivo alegado; não os repetir como medição comprovada. Há outra medição histórica da O259 em 13/10, também não atual. `app/folha/export.py:1186`, `app/folha/export.py:1212`, `app/docs/CORTES.md:327`, `app/docs/CORTES.md:328`, `app/docs/RELATO-ARQUIVO.md:42954`.
- **Próximo passo:** medir pela sonda, sem afrouxar retenção. `app/docs/BACKLOG.md:1293`.

**AUSENCIA-UMA-VIGENTE**

- **Pedido:** eliminar contradição abono/falta que desconta, pela lei da família e sem apagar registro; aplicação em exportada exige autorização. `app/docs/BACKLOG.md:1941`.
- **Estado:** “REGISTRADA” na grade; relato histórico mediu quatro dias com abono+falta, três em competência exportada. São números históricos, não censo atual. `app/docs/GRADE-MESTRA.md:259`, `app/docs/RELATO.md:2258`.
- **Resposta atual:** a contradição continua possível por construção: desconto soma toda ausência aprovada que desconta; cobertura escolhe uma, a maior `(data,pk)`. A constraint restringe apenas mesmo colaborador/tipo/data, permitindo tipos diferentes no mesmo dia. `app/folha/export.py:87`, `app/folha/export.py:140`, `app/ponto/turnos.py:2765`, `app/ponto/turnos.py:2793`, `app/ponto/models.py:836`.
- **Escritores atuais:** `retornar_mesmo_tipo` deixa tipo diferente criar ao lado, e materialização continua chamando esse modo; `decidir`/`mudar_estado` aprovam a linha sem consultar sobreposição nessa transição. `app/ponto/services/ausencia.py:436`, `app/chamados/services/materializacao.py:962`, `app/chamados/services/materializacao.py:973`, `app/ponto/services/ausencia.py:1079`, `app/ponto/services/ausencia.py:1088`, `app/ponto/services/ausencia.py:968`, `app/ponto/services/ausencia.py:988`.
- **Posterior que não equivale à cura:** **AUSENCIA-EFEITO-PELO-ESTADO** corrigiu a coluna/chips/filtro para refletirem o estado, especialmente rejeitadas. Não substitui a regra de uma ausência vigente nem o somatório acima. `app/docs/BACKLOG.md:826`, `app/docs/BACKLOG.md:836`, `app/docs/RELATO.md:13`, `app/docs/RELATO.md:81`, `app/folha/export.py:140`.
- **Próximo passo:** medir pares atuais com o mesmo juiz de cobertura e desconto; só então definir/apply da cura registrada. `app/docs/BACKLOG.md:1941`.

Sonda conjunta proposta, **não executada** [nao verificado]. Usa as portas atuais de retenção, janela, cobertura e desconto citadas acima; universo declarado: colaboradores com `FechamentoMensal` 09 ou 10/2026, sem excluir empresa ou estado. Para censo de ausências além dessa população, ampliar explicitamente o universo [nao verificado].

```python
# Executar em shell Django já conectado ao tenant correto; não usar --apply.
from collections import Counter, defaultdict
from datetime import date
from django.db import connection, transaction
from django.utils import timezone
from ponto.models import FechamentoMensal, DiaPago
from escala.models import EscalaColaborador
from folha.export import (
    janela_competencia, motivos_retencao_celula,
    minutos_de_falta_por_dia, grade_do_fechamento,
)
from ponto.turnos import (
    ausencias_sobrepostas, dias_da_ausencia, cobertura_ausencia_periodo,
)
from ponto.catalogo.ausencias import abonam_vigentes
from ponto.services.dia_pago import por_dia_da_grade

with transaction.atomic():
    with connection.cursor() as cur:
        cur.execute("SET TRANSACTION READ ONLY")
    hoje = timezone.localdate()
    fs = list(FechamentoMensal.objects.filter(ano=2026, mes__in=(9, 10))
              .select_related("colaborador__empresa").order_by("mes", "colaborador_id"))
    print("UNIVERSO", "FM_09_10_2026", len(fs), "HOJE", hoje.isoformat())
    totais = Counter()
    abona = set(abonam_vigentes())

    for f in fs:
        c = f.colaborador
        ini, fim = janela_competencia(c.empresa, f.mes, f.ano)
        chave = (c.empresa_id, f.mes, f.ano)
        totais[chave + ("fechamentos",)] += 1
        motivos, dias = motivos_retencao_celula(c, ini, fim, hoje=hoje, com_dias=True)
        if motivos:
            totais[chave + ("retidos",)] += 1
            for m in motivos:
                totais[chave + (m,)] += 1
            print("O236", f"col{c.pk}", chave, motivos,
                  [(d["data"].isoformat(), d["motivo"], d["veredito"]) for d in dias])

        por_dia = defaultdict(list)
        for a in ausencias_sobrepostas(c, ini, fim).filter(status="aprovada"):
            for d in dias_da_ausencia(a.tipo, a.data, a.data_fim, ini, fim):
                por_dia[d].append(a)
        cobertura = cobertura_ausencia_periodo(c, ini, fim, para="folha")
        faltas = minutos_de_falta_por_dia(c, ini, fim)
        for d, aa in sorted(por_dia.items()):
            if len(aa) < 2:
                continue
            vence = cobertura.get(d)
            print("AUSENCIA-UMA-VIGENTE", f"col{c.pk}", chave, d.isoformat(),
                  "registros", [(a.pk, a.tipo, a.minutos) for a in aa],
                  "vence", (vence.pk, vence.tipo) if vence else None,
                  "falta_min", faltas.get(d, 0),
                  "abono_com_desconto", bool(
                      vence and vence.tipo in abona and faltas.get(d, 0) > 0))

        if c.pk == 369:
            ab, pv, rl = por_dia_da_grade(grade_do_fechamento(f))
            print("COL369_GRADE", chave,
                  [(d.isoformat(), pv.get(d), rl.get(d), ab.get(d))
                   for d in (date(2026, 9, 29), date(2026, 9, 30)) if d in pv])

    for chave, n in sorted(totais.items()):
        print("O236_RESUMO", chave, n)
    print("COL369_VINCULOS", list(
        EscalaColaborador.objects.filter(colaborador_id=369)
        .order_by("data_inicio", "pk")
        .values("pk", "ativa", "data_inicio", "data_fim", "tipo_escala_id")))
    for r in DiaPago.objects.filter(
        colaborador_id=369, data__range=(date(2026, 9, 19), date(2026, 10, 1))
    ).order_by("data", "versao").values(
        "data", "mes", "ano", "versao", "horas_trabalhadas",
        "horas_intra_indenizada", "minutos_realizados", "minutos_abonados"
    ):
        print("COL369_DIAPAGO", "col369", r)
print("FIM_OK")
```

| ID | Conclusão |
|---|---|
| COL369 | Agregado desatualizado: tipos medidos, vínculo decidido; ausência+trabalho corrigida posteriormente por BO-B, estoque específico da 09 precisa sonda. `app/docs/BACKLOG.md:1380`, `app/docs/BACKLOG.md:1368`, `app/docs/RELATO.md:5475`. |
| O236 | L-033 implementada por O254/O259; decomposição atual por motivo ainda exige dados. `app/docs/RELATO-ARQUIVO.md:43115`, `app/docs/RELATO-ARQUIVO.md:42950`, `app/folha/export.py:1212`. |
| AUSENCIA-UMA-VIGENTE | Regra segue registrada; código ainda admite soma de desconto versus cobertura única. `app/docs/GRADE-MESTRA.md:259`, `app/folha/export.py:140`, `app/ponto/turnos.py:2793`. |

## 17/11 08:07 · sessao 8752fb8e3d52 · - · - · origem=Ronald (tmux)

A sonda deve rodar em Django já inicializado, com tenant correto e mapa `PK_POR_COL` confirmado pelo operador; a equivalência entre sufixo `colNNN` e PK não consta do dado fornecido. [não verificado]

`InsumosDoDono.dono()` consulta turnos, cadastro e lavra de fase; os caminhos lidos são consultas, sem chamada ao escritor de fase. Pode ser impresso como classificação automática, sem tratá-lo como comprovação do dono L-099. (`app/ponto/management/commands/e6_oraculo.py:182`; `app/ponto/supra_juiz.py:179`; `app/escala/services/fase_12x36.py:117`)

O bloco abaixo usa as funções reais do espelho, do E6 e do pareador. Captura as instâncias carimbadas da janela visual completa; imprime a ata pelo juiz, as batidas cruas da janela acolchoada, todos os turnos e a linha do alvo. (`app/ponto/services/espelho.py:1194`; `app/ponto/management/commands/e6_oraculo.py:128`; `app/ponto/turnos.py:1850`; `app/ponto/turnos.py:1984`; `app/ponto/juiz_batida.py:149`)

```python
# Django já inicializado e tenant já selecionado.
# Antes de executar, definir PK_POR_COL = {"col369": <PK confirmado>, ...}.
import json
import logging
from contextlib import ExitStack
from datetime import date, timedelta

from django.apps import apps
from django.db import connections, transaction, DatabaseError
from django.db.models import Q
from django.utils import timezone

assert apps.ready, "Django não inicializado"
assert connections["default"].schema_name != "public", "Selecionar tenant correto"

from colaboradores.models import Colaborador
from escala.alimentacao import (
    celulas_do_periodo, escalas_do_periodo, vinculo_do_dia,
)
from ponto.models import Batida, Ausencia
from ponto.janelas import janela_fechamento
from ponto.services.espelho import espelho_do_colab
from ponto.management.commands.e6_oraculo import (
    InsumosDoDono, minutos_do_espelho,
)
from ponto.juiz_batida import periodos_do_dia
from ponto.turnos import (
    turnos_do_colab, janela_com_pad, papel_por_minuto_da_ata,
    _papel_da_ata, _pares_marcados, intervalos_do_turno,
    pausas_abaixo_do_piso,
)

CASOS = [
    ("col369", "2026-10-12", 10),
    ("col864", "2026-10-12", 10),
    ("col114", "2026-11-08", 11),
    ("col114", "2026-11-15", 11),
    ("col723", "2026-10-08", 10),
    ("col746", "2026-09-24", 10),
    ("col876", "2026-09-21", 10),
    ("col130", "2026-10-29", 11),
    ("col220", "2026-11-10", 11),
    ("col868", "2026-09-22", 10),
    ("col516", "2026-10-23", 11),
    ("col884", "2026-11-08", 11),
]
assert "PK_POR_COL" in globals(), "Falta mapa colNNN -> PK confirmado"
assert all(col in PK_POR_COL for col, _, _ in CASOS)

def campos(obj, nomes):
    return {n: getattr(obj, n) for n in nomes.split()}

def emitir(tipo, **dados):
    print(json.dumps({"tipo": tipo, **dados}, default=str, ensure_ascii=False))

FALHAS_SQL = []

def vigiar(execute, sql, params, many, context):
    try:
        return execute(sql, params, many, context)
    except DatabaseError as exc:
        # Mesmo erro que o espelho capturar impede FIM_OK.
        FALHAS_SQL.append({
            "alias": context["connection"].alias,
            "classe": type(exc).__name__,
        })
        raise

BATIDA = "pk timestamp tipo sequencia is_intervalo origem aprovada retratada_em escala_id"
FLAGS = "_papel_ata _intra_dur _intra_ata _intra_declarado _eco_flush"
LINHA = (
    "minutos_realizados pago_h realizado_sem_turno realizado_aberto "
    "realizado_longo realizado_janela_descontada folga feriado "
    "veredito_dia falta_marcos previsto_canon realizado_grade orfas_grade"
)
JUÍZ = (
    "papeis entrada saida em_aberto porque intervalo orfas "
    "nao_sei perguntas desalinhado"
)

nivel_anterior = logging.root.manager.disable
logging.disable(logging.CRITICAL)
agora = timezone.now()

try:
    with ExitStack() as stack:
        aliases = list(connections)
        for alias in aliases:
            conn = connections[alias]
            assert conn.vendor == "postgresql", f"{alias}: requer PostgreSQL"
            assert not conn.in_atomic_block, f"{alias}: transação já aberta"
            stack.enter_context(transaction.atomic(using=alias))
            with conn.cursor() as cur:
                cur.execute(
                    "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
                )
                cur.execute("SHOW transaction_read_only")
                assert cur.fetchone()[0] == "on"
            stack.enter_context(conn.execute_wrapper(vigiar))

        emitir("contexto", agora=agora,
               schema=connections["default"].schema_name,
               observacao="Estado atual; não restaura a fotografia histórica R3")

        for col, iso, mes in CASOS:
            d = date.fromisoformat(iso)
            c = Colaborador.objects.get(pk=PK_POR_COL[col])
            ini, fim = janela_fechamento(mes, d.year, c.empresa)
            hoje = timezone.localtime(agora).date()

            esp = espelho_do_colab(c, hoje, ini, fim, agora=agora)
            linhas = [x for x in esp["dias"] if x["data"] == d]
            assert len(linhas) == 1, f"{col} {iso}: alvo sem linha única"
            de = linhas[0]
            numero, fonte = minutos_do_espelho(de)
            ins = InsumosDoDono(c, ini, fim, hoje, espelho=esp, agora=agora)
            emitir("alvo", col=col, dia=d, competencia=mes,
                   janela=[ini, fim], visual=[esp["data_ini"], esp["data_fim"]],
                   numero=numero, fonte=fonte,
                   linha={k: de.get(k) for k in LINHA.split()},
                   e6_minutos=ins.oraculo.minutos.get(iso),
                   e6_horas=ins.horas.get(iso),
                   e6_envelope=ins.oraculo.envelope.get(iso),
                   dono_automatico=ins.dono(d))

            lo, hi = janela_com_pad(esp["data_ini"], esp["data_fim"])
            cels = celulas_do_periodo([c.pk], lo, hi)
            por_data = {dt: cel for (_, dt), cel in cels.items()}
            escalas = escalas_do_periodo(
                c.pk, lo, hi,
                mais=[cel.escala_geradora_id for cel in cels.values()
                      if cel.escala_geradora_id],
            )
            bs = list(Batida.objects.filter(
                colaborador=c, timestamp__date__range=(lo, hi)
            ).order_by("timestamp", "pk"))
            vivas = [b for b in bs if b.retratada_em is None]
            papel = papel_por_minuto_da_ata(
                c, lo, hi, celulas=por_data, batidas=vivas
            )
            for b in bs:
                emitir("batida", col=col, alvo=d,
                       **campos(b, BATIDA),
                       papel_consultado=_papel_da_ata(b, papel))

            turnos = turnos_do_colab(
                c, esp["data_ini"], esp["data_fim"], celulas=por_data
            )
            for t in turnos:
                emitir("turno", col=col, alvo=d, data_turno=t.data_turno,
                       entrada_pk=getattr(t.entrada, "pk", None),
                       saida_pk=getattr(t.saida, "pk", None),
                       aberto=t.aberto, cross_meianoite=t.cross_meianoite,
                       pares_marcados=_pares_marcados(t),
                       pausas_validas=intervalos_do_turno(t),
                       pausas_abaixo_piso=pausas_abaixo_do_piso(t),
                       batidas=[
                           {**campos(b, BATIDA),
                            **{k: getattr(b, k, None) for k in FLAGS.split()}}
                           for b in t.batidas
                       ])

            for dt in sorted(por_data):
                cel = por_data[dt]
                ec = vinculo_do_dia(c.pk, dt, escalas, celulas=cels)
                te = ec.tipo_escala if ec else None
                juiz = periodos_do_dia(cel, te)
                emitir("celula", col=col, alvo=d, dia=dt,
                       **campos(cel, "pk trabalha origem escala_geradora_id"),
                       dna_marcos=(cel.dna or {}).get("marcos"),
                       juiz={k: juiz.get(k) for k in JUÍZ.split()},
                       vinculo=(campos(
                           ec, "pk tipo_escala_id data_inicio data_fim ativa"
                       ) if ec else None),
                       template=(campos(
                           te, "pk tipo_ciclo intervalo_modo hora_inicio "
                           "hora_fim hora_inicio_intervalo hora_fim_intervalo"
                       ) if te else None),
                       intervalo_efetivo=(
                           ec.intervalo_do_dia(dt, celulas=cels) if ec else None
                       ))

            ausencias = Ausencia.objects.filter(
                colaborador=c, data__lte=hi
            ).filter(
                Q(data__gte=lo) | Q(data_fim__gte=lo)
                | Q(data_fim__isnull=True, tipo__in=Ausencia.TIPOS_EM_ABERTO)
            )
            for a in ausencias:
                emitir("ausencia", col=col, alvo=d, **campos(
                    a, "pk tipo data data_fim status minutos hora_ini hora_fim"
                ))

            emitir("cards_tela", col=col, alvo=d, cards=[
                {
                    "entrada": p.entrada,
                    "saida": getattr(p, "saida", None),
                    "turno_aberto": p.turno_aberto,
                }
                for p in de["turnos"]
            ])

        for alias in aliases:
            transaction.set_rollback(True, using=alias)

    if FALHAS_SQL:
        emitir("ERRO_SQL", falhas=FALHAS_SQL)
    else:
        print("FIM_OK")
finally:
    logging.disable(nivel_anterior)
```

Observações para consolidar:

- `minutos_do_espelho` preserva `None` e informa a fonte: `minutos_realizados`, depois `pago_h`, depois ausência de lavratura. (`app/ponto/management/commands/e6_oraculo.py:263`)
- O mapa de células de `turnos_do_colab` usa somente data; o de `vinculo_do_dia` e `intervalo_do_dia` usa `(colaborador_id, data)`. (`app/ponto/turnos.py:1865`; `app/escala/alimentacao.py:228`; `app/escala/models.py:1184`)
- `periodos_do_dia` declara leitura e fornece a razão de abstenção, papéis e pausas; assim a sonda não depende de imprimir o JSON bruto da ata. (`app/ponto/juiz_batida.py:149`; `app/ponto/juiz_batida.py:163`; `app/ponto/juiz_batida.py:238`)
- Não envolver o bloco em novo `manage.py shell`: o carregamento dos settings SaaS chama `os.makedirs` e configura handlers de arquivo. (`app/config/settings/saas.py:118`; `app/config/settings/saas.py:138`; `app/config/settings/saas.py:162`)
- A sonda proposta não foi executada. [não verificado]

## 17/11 08:07 · sessao b766e941be2a · - · - · origem=Ronald (tmux)

Mapa08 reconferido na cópia; “L7” abaixo = LOTE7 c7, pouso **documentado** como merge `42fd467a0` de `a094967ee`; ancestralidade Git [nao verificado]. (`app/docs/RELATO.md:164`)

Abreviações nesta tabela: `aus` = `app/ponto/services/ausencia.py`; `pv` = `app/ponto/views.py`; `mod` = `app/ponto/models.py`. (`app/ponto/services/ausencia.py:262`; `app/ponto/views.py:2613`; `app/ponto/models.py:660`)

| Item do inventário | Estado | arquivo:linha HEAD / resultado | Obra |
|---|---|---|---|
| L1 Matriz única e chamadores | PROCEDE | `aus:914`, `aus:955`, `aus:692`, `aus:1034`, `aus:1088`, `aus:1180`, `aus:1320`: matriz e porta continuam únicas para ausência comum; férias recusadas na porta `aus:968`. | Acordador posterior L7 em `aus:989`; pouso documental `app/docs/RELATO.md:164`. |
| L1 Nascimento explícito × reabertura | PROCEDE | `aus:433` chama `_reabrir` sem `status`; nova usa `status or _st_inicial` em `aus:486`; reaberta deriva em `aus:1298`. Admin ainda pede aprovada `pv:2552`; veredito também `app/ponto/services/veredito_celula.py:338`; escape FALTA_DIA continua aprovada `app/chamados/services/materializacao.py:1080`. | Cura/commit [nao verificado]. |
| L1 Pré-veto PWA × criar | PROCEDE | PWA veta qualquer conflito singular em `pv:2670`; porta aplica `linha_que_veta` em `aus:465`; API chama diretamente com `devolver_mesmo_fato` em `app/api/views.py:1812`, `:1818`. | Cura/commit [nao verificado]. |
| L1 Efeito `None`: folha × espelho | PROCEDE | `app/ponto/catalogo/ausencias.py:295` pode devolver `None`; folha suprime por `!= DESCONTA` em `app/ponto/services/fechamento.py:700`; coletor exige `== SUPRIME` em `app/ponto/services/efeito_ausencia.py:61`. | EFEITO-NONE-UM-LEITOR segue fila `app/docs/BACKLOG.md:1907`; cura/commit [nao verificado]. |
| L1 Documento: matriz × gate × vencimento | PROCEDE | Matriz `aus:915` impede doc→aprovada; gate só cobra autor-colab em `aus:773`; vencimento entrega sem documento em `aus:1177`. Aprovação individual permite confirmação explícita `pv:2754`. | Cura/commit [nao verificado]. | [nao verificado]
| L1 Contradição documento × rejulgamento por tipo | PROCEDE | São perguntas distintas: efeito desconta `aus:757`; estado derivado `aus:689`; edição só rejulga se mudou tipo `aus:674`; comando segue unidirecional com inverso apenas contado `app/ponto/management/commands/rejulgar_status_por_tipo.py:36`. | Alteração dessa regra/commit [nao verificado]. |
| L1 Fechar chamado da ausência decidida | MUDOU | Leitor segue em `aus:851`, `aus:863`; `decidir` agora tem atomic próprio `aus:1079`, transição+carimbos em `aus:1088`, acordador no fim `aus:1123`. | L7; `app/docs/RELATO.md:164`. |
| L1 Docstring `retornar_mesmo_tipo` | PROCEDE | Texto ainda inclui rejeitada como devolvida `aus:295`, mas ramo rejeitada reabre antes `aus:399`, `aus:433`; busca retornar só depois `aus:436`. | Correção documental/commit [nao verificado]. | [nao verificado]
| L2 Criar: save e trilha | MUDOU | Nascimento salva em atomic `aus:561`, `aus:562`; evento `aus:585`; agora acorda projeção se a linha entra na folha `aus:597`. | L7 para acordador; `app/docs/RELATO.md:164`. |
| L2 Editar período/tipo fora de `mudar_estado` | MUDOU | Continua escritor separado de tipo/data/fim `aus:648`, `aus:650`, save `aus:659`; invalidação agora compara insumo antes/depois `aus:669`; tipo rejulga `aus:674`. | H12H06-B documentada `app/docs/RELATO.md:9295`; acordador posterior L7 `aus:667`, `app/docs/RELATO.md:164`. |
| L2 Rejeitar férias: update sem trilha/signal | CURADO | Agora atomic+trava por PK `aus:1484`, `aus:1488`; save por linha dispara signal `aus:1498`; trilha obrigatória, falha desfaz `aus:1499`, `aus:1506`; acorda projeção `aus:1508`. Continua porta separada de férias, não chama `mudar_estado`, que as recusa `aus:968`. | L7; `app/docs/RELATO.md:164`; testes de rollback `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:231`, `:256`. |
| L2 Apagar/espelhar férias | MUDOU | Exclusões continuam na porta `aus:1374`, `aus:1401`, `aus:1461`; agora acordam projeção `aus:1376`, `aus:1403`, `aus:1463`; nascimento aprovado `aus:1391`; existente rejeitada ainda preservada `aus:1385`. | L7; `app/docs/RELATO.md:164`. |
| L2 `rederivar_fim` sem invalidar fechamento | CURADO | Captura insumo anterior `aus:1751`, salva/refresca `aus:1752`, acorda antes/depois `aus:1756`; comando ainda entra por essa porta `app/ponto/management/commands/curar_ausencia_sem_fim.py:50`. | L7; `app/docs/RELATO.md:164`. BACKLOG antigo ainda descreve ausência de invalidação em `app/docs/BACKLOG.md:2207`. |
| L2 Migration 0057 forward | PROCEDE | Atualização direta `app/ponto/migrations/0057_a4_backfill_pendente.py:43`, trilha por linha `:44`, lista histórica `:30`. | Dependência de LogAuditoria foi corrigida/documentada no próprio arquivo `:72`; commit [nao verificado]. |
| L2 Migration 0057 reverse | PROCEDE | Reverte para `pendente`, sem trilha nesse corpo `app/ponto/migrations/0057_a4_backfill_pendente.py:58`, `:65`. | Cura/commit [nao verificado]. |
| L2 Admin Django (antes não reaberto) | PROCEDE | `AusenciaAdmin(SoLeitura)` em `app/ponto/admin.py:38`; mixin recusa add/change/delete `app/core/admin.py:59`, `:62`, `:65`. | O167 identificado em `app/ponto/admin.py:39`; commit [nao verificado]. |
| L3 Quatro estados atuais sem terminais | PROCEDE | Todos têm saída pela matriz `aus:914`; rejeitada volta a abertas ou aprovada `aus:922`. | Mudança dessa matriz/commit [nao verificado]. | [nao verificado]
| L3 Seta rejeitada→aprovada sem chamador | PROCEDE | Seta segue `aus:922`; aprovadores humanos selecionam abertas `pv:2369`, `pv:2752`, `pv:2764`; reabertura deriva aguardando `aus:1298`, `aus:1320`. Chamador externo/dinâmico [nao verificado]. | Remoção/commit [nao verificado]. | [nao verificado]
| L3 `pendente` legado sem saída no runtime atual | PROCEDE | Reverse pode escrevê-lo `app/ponto/migrations/0057_a4_backfill_pendente.py:65`; `ABERTAS` e matriz continuam sem ele `aus:912`, `aus:914`. Linhas em produção [nao verificado]. | Cura/commit [nao verificado]. |
| L3 Carimbos após reabrir | PROCEDE | `_reabrir` só altera documento/descrição/estado/prazo `aus:1298`, `aus:1307`, `aus:1316`, `aus:1320`, `aus:1325`; `mudar_estado` não limpa aprovador/data `aus:975`, `aus:986`. | Cura/commit [nao verificado]. | [nao verificado]

**Resumo mapa08:** os três defeitos centrais continuam — reabertura ignora estado pedido, PWA tem veto mais restritivo, efeito `None` diverge —; L7 curou rejeição de férias sem trilha/signal e rederivação de fim sem invalidação, além de mudar a propagação de todas as escritas relevantes. (`aus:433`; `pv:2670`; `app/ponto/services/fechamento.py:700`; `app/ponto/services/efeito_ausencia.py:61`; `aus:1498`; `aus:1756`; `app/docs/RELATO.md:164`)

O mapa não viu estes sítios/crossovers:

| Sítio fora do inventário | Resultado e referência |
|---|---|
| **API de chamados ainda lê `pendente`** | Oferta “Anexar comprovante” só nasce se `_a.status == 'pendente'`, excluindo `aguardando_documento` atual: `app/api/views.py:1061`, `:1064`; estados atuais `mod:700`. Já o leitor de tarefa oferece entregar documento por estado atual `app/chamados/services/tarefa_do_dia.py:312`, `:314`. Ocorrência/impacto em produção [nao verificado]. |
| **Modelo escreve campos derivados** | `Ausencia.save` atribui `aprovada_em` para aprovada **ou rejeitada** `mod:882`, `mod:883`, deriva minutos de falta `mod:887`, `mod:897`, e `data_fim` `mod:912`, `mod:917`. É derivador usado pelas portas, ausente como linha própria no censo L2. (`aus:980`) |
| **Copiloto também sofre reabertura** | Prévia anuncia aprovada `app/ponto/services/checar_ausencia.py:39`; escrita solicita `status='aprovada'` `:47`; ramo rejeitada ignora esse pedido `aus:433`. |
| **Veto da edição também diverge da criação** | Edição ainda veta conflito singular sem `linha_que_veta` `aus:637`; criação aplica precedência por linha `aus:465`. Caso estático: editar atestado sobre falta recebe veto singular, enquanto criação permite absolvição por tipo. (`app/ponto/precedencia.py:136`; `aus:637`; `aus:465`) |
| **Novos leitores/invalidador L7** | `_insumo_da_folha` lê tipo/data/fim/minutos/período pelo predicado da folha `aus:119`, `aus:138`; `_acordar_projecao` pergunta os dias ao juiz `aus:192` e invalida os dois lados `aus:195`; lê relógio/FechamentoMensal na linha sem fim `aus:147`, `aus:191`. Fila própria `app/docs/BACKLOG.md:859`. |
| **Signals são leitores de toda escrita** | Save/delete da ausência rejulgam células `app/ponto/signals.py:143`, `:150`; consumidor on_commit devolve perguntas absorvidas, reconcilia cobrança e fecha chamado `:256`, `:262`, `:269`, `:271`, `:272`. |
| **Consumidores de carimbos e falta de documento** | Cartão/histórico usa `aprovado_por` e `aprovada_em` como “quem decidiu/quando” `app/ponto/services/dia_decidido.py:514`, `:520`, `:522`; cartório do cartão recebe esses valores `:529`. Vigia lê aprovada sem documento e sem aprovador `app/ponto/services/vigia_ausencia.py:115`, `:120`. |
| **Outros leitores de status/range** | TXT filtra aprovada e expande range/aberto `app/folha/export.py:87`; relatórios filtram aprovada e perguntam dias ao juiz `app/relatorios/views.py:197`, `:205`; score de assiduidade usa não-rejeitada `app/inteligencia/management/commands/calcular_scores.py:12`, `:14`. São universos distintos explicitados no código. |
| **Risco novo documentado após L7** | `decidir` e rejeição de férias agora têm atomic próprio `aus:1079`, `aus:1484`; fila declara erro de banco engolido no post_save que envenena transação `app/docs/BACKLOG.md:858`. Reprodução nesta auditoria [nao verificado]. |

As perguntas 1–4, 6–7 e 9 continuam sem resposta documental de cura comprovada; seus mecanismos permanecem nos sítios acima. (`aus:433`; `pv:2670`; `app/ponto/services/fechamento.py:700`; `aus:922`; `aus:773`; `aus:1285`; `app/ponto/migrations/0057_a4_backfill_pendente.py:65`) A pergunta 5 ganhou porta de férias com trilha/signal próprios, e a 8 ganhou acordador comum aos escritores listados; ambos pela L7. (`aus:1468`; `aus:1498`; `aus:1506`; `aus:154`; `aus:669`; `aus:1756`; `app/docs/RELATO.md:164`)

## 17/11 08:06 · sessao 1e96e55f537e · - · - · origem=Ronald (tmux)

**Git:** não há metadados Git nesta cópia; não é possível validar `git log`, ancestralidade nem diff desde 05/11. A própria GRADE registra essa limitação em `app/docs/GRADE-MESTRA.md:1711`. Hashes abaixo são evidência documental.

| Mudança documentada | Marco | Possível efeito na reconferência |
|---|---|---|
| F3-BECO-1-VOLTA-3 | 05/11 17:5x, `3b8d4a3e` — `app/docs/BACKLOG.md:2210` | Rejulga mês após corrigir folga e reata pai na fila. Precede a ingestão documentada às 20:4x; mudança **posterior à ingestão [nao verificado]**. `app/docs/RELATO.md:9165` |
| C20-JUIZ-3 | 05/11 20:2x, `de9fd69b` — `app/docs/BACKLOG.md:1935` | Eventos em criar/recompor turnos e consumidor outbox; igualmente próximo/anterior à ingestão. `app/docs/RELATO.md:9154` |
| H12H06-A | 05/11 22:1x, `ca278fc4` — `app/docs/RELATO.md:9210` e `:9212` | Faltas passam pela jornada da grade; altera filtro de rescisão. `app/docs/RELATO.md:9211` |
| H12H06-B | 06/11 09:3x, raia `97754f8d`, comboio `62017e70` — `app/docs/RELATO.md:2324` | Leitores de pagamento parcial passam a `cobre_parte_do_dia`; aplicação em 28 linhas da competência 11 às 20:01. `app/docs/RELATO.md:2395`, `:3107` |
| V2, item 2: FERIADO-HE100-NOTURNA | 07/11 05:2x, raia `a23831c3` — `app/docs/BACKLOG.md:1954` | Motor/DiaPago/fechamento de feriado noturno; implantação confirmada documentalmente. `app/docs/RELATO.md:2078` |
| CXR-TELA-LE-O-MOTOR | 08/11 08:5x, `94d41ea5` — `app/docs/BACKLOG.md:886` | Tela passa a exibir os dias CxR publicados pelo motor; altera visibilidade/porta para CADASTRO. `app/docs/BACKLOG.md:886` |
| E1-ENTRADAS | 08/11 22:46, `a8a692f55` — `app/docs/BACKLOG.md:2008` | Entradas/eventos do fio, incluindo classe de plantio noturno. `app/docs/BACKLOG.md:1944`, `:2008` |
| E1-FILA | 09/11 01:5x, `7aa48401` — `app/docs/BACKLOG.md:2009` | Travas, ordem e atualização dos fatos do fio. `app/docs/BACKLOG.md:2009` |
| E2 + primeira E3 | 09/11 16:1x, `7c16da320` — `app/docs/BACKLOG.md:2021` | Tarefa do dia/estado da pergunta; a migração dos cinco leitores ainda não estava completa nesse pouso. `app/docs/RELATO.md:1764` |
| E4a-1 | 09/11 20:5x, `892159c5f` — `app/docs/BACKLOG.md:2035` | Portas registram atos mantendo projeção legada. `app/docs/BACKLOG.md:2035` |
| E4a-2 | 10/11 14:5x, `613a51498` — `app/docs/BACKLOG.md:2043` | Emissor periódico vira vigia; elimina julgamento por varredura. `app/docs/BACKLOG.md:2043` |
| E3 fase X1 | 11/11 11:0x, `e70a54f7e` — `app/docs/RELATO.md:1446` | Rejulgamento de células; houve mudanças `nunca_bateu → concorde` em desligados, documentadas como CADASTRO. `app/docs/RELATO.md:1439` |
| E4b | 12/11 08:46, `087fe758a` — `app/docs/BACKLOG.md:2044` | Código de recuperação do fio morto; pouso não comprova aplicação individual dos casos. `app/docs/BACKLOG.md:2044` |
| LEI-DESLIGADO-NAO-TRAVA | 13/11 16:48, `9e2c54017` — `app/docs/BACKLOG.md:731` | Desligado deixa de reter a folha; relevante quando a condição existir no caso. `app/docs/BACKLOG.md:731` |
| O168 | 13/11 19:2x, `d33c0a899` — `app/docs/BACKLOG.md:2093` | Guarda conversa/resposta sem julgamento no gate de reconciliação do pai. `app/docs/BACKLOG.md:2093` |
| HJ-091 | 14/11 13:54, `8376da7aa` — `app/docs/BACKLOG.md:750` | Origem das ausências de férias e preservação dos vetos/rejeições; vínculo individual aos 20 casos **[nao verificado]**. `app/docs/RELATO.md:725` |
| E3 v10 | Merge `90f3fa66b`; deploy 14/11 22:42 — `app/docs/RELATO.md:614`, `:597` | Dono, porta alcançável, resposta sem texto e fio; relevante para retenção/ação alcançável. `app/docs/RELATO.md:610`, `:612` |
| BUG-ENC-CELULAS-FORMA | 15/11 23:0x, `7825fec5b` — `app/docs/BACKLOG.md:1021` | Corrige chaves de células enviadas ao juiz de turnos; pode alterar veredito do cartório. `app/docs/BACKLOG.md:1021` |
| E3-RESTO-R3 c17 | 16/11 01:3x, `f8ee770a5` — `app/docs/BACKLOG.md:770` | Resíduos dos leitores/portas/retido calado; veredito por caso **[nao verificado]**. `app/docs/BACKLOG.md:770` |
| LOTE3 c7 | 16/11 10:58, `be4a6219e` — `app/docs/RELATO.md:184` | SITUACAO-DESLIGADO-SEM-PORTA. `app/docs/RELATO.md:184` |
| F1 B1 | 16/11 12:14, `3adf703e3` — `app/docs/RELATO.md:174` | Migração do juiz de resposta; prova documental de 283 casos sem diferença nesse recorte. `app/docs/RELATO.md:174` |
 다음| LOTE7 c7 | 16/11 13:07, `42fd467a0` — `app/docs/RELATO.md:164` | Porta de ausência/regularização; kernel medido com escritas iguais e sem invalidar fechamento nos cinco controles. `app/docs/RELATO.md:175` |
| BUG-PAR c4+c4c | 16/11 21:41, `41f5648af` — `app/docs/RELATO.md:32` | Respostas de intervalo e universo 163; aplica-se apenas se o caso pertencer à classe/universo, **[nao verificado]**. `app/docs/BACKLOG.md:2084` |

**Os nove “NO AR antes de 14/11” são nove casos vinculados a cinco obras antigas**, não nove mudanças posteriores a 05/11: COBPARC (3), JUIZ-DO-PAR (2), FIO (2), REGISTRADO (1), TRILHA (1), pelo anexo fornecido. As implantações documentadas são respectivamente 28/10 `fb813587`, 29/10 `b3bf529e`, 27/10 `7c9e4c27`, 27/10 e 28/10 `5e9a0a51`. `app/docs/BACKLOG.md:874`, `:864`, `:882`, `:868`, `:866`. Portanto os marcos antigos, sozinhos, não demonstram correção após a ingestão de 05/11.

| ESTRUTURA adicional | Causa provada | Cobertura / menor escopo |
|---|---|---|
| G3-10, col163 10/10 | Dono ESTRUTURA foi retorno residual; defeito causal específico **[nao verificado]**. `app/docs/RELATO.md:9274`, `:9281`; ramo residual `app/ponto/management/commands/e6_oraculo.py:110` | `G3G4-ESTRUTURA-RESIDUO` é registro para nomear padrão, não cura demonstrada. `app/docs/GRADE-MESTRA.md:274`, `app/docs/BACKLOG.md:2208`. Menor escopo: reconferir testemunhas, isolar primeiro ponto divergente e nomear padrão antes da cura **[proposta; nao verificado]**. |
| G3-11, col163 17/10 | Idem. `app/docs/RELATO.md:9275`, `:9281`; `app/ponto/management/commands/e6_oraculo.py:110` | Idem. `app/docs/BACKLOG.md:2208` |
| G3-12, col873 26/09 | Idem. `app/docs/RELATO.md:9276`, `:9281`; `app/ponto/management/commands/e6_oraculo.py:110` | Idem. `app/docs/BACKLOG.md:2208` |

Não há base para promover qualquer um dos 20 CADASTRO/BATIDA a FECHADO apenas pelo histórico documental; resultado atual exige a sonda individual **[nao verificado]**.

## 17/11 08:06 · sessao 4edf3b7f970b · - · - · origem=Ronald (tmux)

Achados para consolidação, todos por leitura; execução e resultado no HEAD são [nao verificado].

| Achado | Classe | Referências | Gravidade | Ação |
|---|---|---|---|---|
| `q_vivas` passa a excluir VALIDACAO somente no ato, enquanto C6 continua lendo `validada_em` da projeção e pode dar a mesma pergunta por viva. Quebra a continência documentada. | 2 / extra | Diff `models.py:3553`; cópia `app/chamados/models.py:3475-3481`; `app/chamados/juizes.py:1404-1406,1454-1457` | Alta | Selar VALIDACAO só no ato × C6 × queryset e reconciliar o contrato. |
| Ressuscitação conserva a linha VALIDACAO, limpa a projeção e não acrescenta ato que a nova leitura reconheça como limpeza. O controle novo espera leitura vazia, mas a implementação conservará validação. | 2 | Diff `test_e4c_f2_leitura_do_ato.py`, `test_controle_ressuscitar_zera_o_par_e_nao_volta`; cópia `app/chamados/models.py:1621-1622,1638-1640` | Alta | Resolver a semântica da ressuscitação preservando a decisão documentada de não devolver ao colaborador; cópia `app/chamados/models.py:1632-1637`. |
| Teste “resposta pela trilha e silêncio” valida primeiro; a validada não integra `vivas`, portanto não recebe SILENCIO. `.get(SILENCIO)` tende a gerar ERROR. | 3 | Diff `test_e4c_f2_leitura_do_ato.py`, `test_resposta_pela_trilha_e_silencio`; cópia `app/chamados/services/validacao.py:168`; `app/chamados/juizes.py:1404-1406`; `app/chamados/models.py:1986-1987,2009-2017` | Alta | Criar fixture que efetivamente alcance SILENCIO e exigir sua precondição antes da asserção. |
| Contagem da face exige seis, mas os casos enumerados validam sete: 1, 2, 4, 5, 7, 8 e 9. | 3 | Diff `test_e4c_f2_leitura_do_ato.py`, `test_face_casa_leitura_em_todas_as_perguntas_do_caso`, lista `casos` e `assertEqual(len(face), 6)` | Média | Corrigir expectativa para sete, após conferir cada caso. |
| Selo de conservação usa respostas projetadas alinhadas em todos os leitores. Reverter individualmente `respondida`, `resposta_display`, `sem_veredito_entre` ou `q_vivas` à base não é distinguido por esse selo. | 1 / 3 | Diff `test_e4c_f2_leitura_do_ato.py`, `LEITORES` e `_mordida`; cópia `app/chamados/models.py:1933-1936,3507-3509,3674-3731` | Alta | Acrescentar divergência entre ato e projeção por leitor; medir reversão individual. A mordida efetiva desses mutantes é [nao verificado]. |
| M15 tem somente um teste que já é RED na base, com e sem mutante: a base fotografa o residual `' \t '`. Não existe controle verde que discrimine o mutante em HEAD. | 3 | Diff `test_cortes_f2_f9_m15.py:39-58`; cópia `app/chamados/models.py:2017` | Média | Acrescentar controle sem residual: base fotografa `''`, mutante fotografa `'07:13'`; medir ambos. |
| Docstring de `_ancoradas` continua descrevendo o universo como `validada_em` nulo, depois de migrar seu filtro para a sequência. | 4 | Diff hunk `models.py:348`; cópia `app/chamados/models.py:337-340` | Baixa | Atualizar a descrição do universo. |

Conferência dos demais mutantes: M4 alcança o último retorno com controle alinhado; M13 remove o filtro e inclui a validada alinhada; M14 relavra a alinhada e altera `julgado_em`; M18 observa o argumento antes do juiz, independentemente de seu retorno. As mordidas são plausíveis por leitura, mas não foram executadas. Referências: diff `test_cortes_f2_f9_m4.py:35-62`, `m13.py:36-55`, `m14.py:39-65`, `m18.py:39-75`; cópia `app/chamados/models.py:309-312,344-347,1548-1564,3161-3169`; `app/chamados/veredito_lavrado.py:270-275`.

Classe 5: nada identificado no diff fornecido. As novas igualdades de `ato_em` fazem correspondência e desempate entre atos; não introduzem guarda por dia civil ou intervalo. Referência: diff `models.py:3478-3505`. Certificação do hash do HEAD: [nao verificado].

## 17/11 08:06 · sessao 3f497742a17d · - · - · origem=Ronald (tmux)

Mapa 07 — referências abaixo são relativas à raiz da cópia. O commit da cura é **documental**, registrado em `app/docs/RELATO.md:259`; ancestralidade Git e execução em produção nesta auditoria: [nao verificado].

| item | estado | arquivo:linha HEAD | obra |
|---|---|---|---|
| L1/T1 — período × dia, piso e escritor | PROCEDE: composição preservada; período usa o piso e chama a mesma decisão individual | `app/ponto/portas/he.py:577`, `app/ponto/portas/he.py:595`, `app/ponto/portas/he.py:206` | O214, descrita em `app/ponto/portas/he.py:515` |
| L1/T1b — período conta recusa revogada como decisão | **CURADO**: trava linha crua, mas pergunta vigência ao juiz antes de barrar | `app/ponto/portas/he.py:583`, `app/ponto/portas/he.py:589`, `app/ponto/portas/he.py:148` | HE-REVOGADA-NA-PORTA; merge documental **271ecba2e**, origem **5937d715b** — `app/docs/RELATO.md:259` |
| L1/T2 — “Não” humano sobre recusa revogada dá `no_op` | **CURADO**: idempotência só contra decisão vigente; `_gravar` revive a linha | `app/ponto/portas/he.py:195`, `app/ponto/portas/he.py:197`, `app/ponto/portas/he.py:83` | Mesma cura/commits — `app/docs/RELATO.md:259` |
| L1/T3 — lavrador × gestão, soma de minutos | PROCEDE: soma compartilhada; universo ativo preservado | `app/ponto/services/he_pendente_lavrado.py:55`, `app/ponto/services/he_pendente_lavrado.py:92`, `app/folha/porta_export.py:194` | Cura posterior: [nao verificado] |
| L1/T4 — gestão × trava × motor | PROCEDE: todos leem vigentes; o motor foi extraído para helper/cache | `app/ponto/services/gestao_he.py:87`, `app/folha/porta_export.py:684`, `app/ponto/motor_calculo_v2.py:1398`, `app/ponto/motor_calculo_v2.py:1417` | Commit da extração: [nao verificado] |
| L1/T5 — callback de revogação × porta | PROCEDE: callback pergunta ao mesmo limite e chama a porta | `app/ponto/management/commands/lavrar_he_pendente.py:56`, `app/ponto/management/commands/lavrar_he_pendente.py:59`, `app/ponto/portas/he.py:494`, `app/ponto/portas/he.py:500` | O273, documentada em `app/ponto/portas/he.py:426`; guardas migradas pela HE-REVOGADA — `app/ponto/portas/he.py:481` |
| L1/T6 — relavratura após decisão e recuperação pelo cron | **MUDOU**: callback e captura persistem; os crons citados lavram retrato/recusam HE, não regeneram fechamento; o cadastro declara explicitamente ausência de cron de recálculo | `app/ponto/services/fechamento.py:1810`, `app/ponto/services/fechamento.py:1818`, `app/ponto/services/fechamento.py:1824`, `app/config/crons.py:1153`, `app/config/crons.py:1577`, `app/config/crons.py:1587` | Mudança posterior versus erro do inventário: [nao verificado] |
| L1/T7 — `_exportada` × competência entregue | PROCEDE: `_exportada` delega a `empresas_exportadas_no_escopo` | `app/ponto/services/autorizacao_he_periodo.py:113`, `app/ponto/services/autorizacao_he_periodo.py:117` | Cura posterior: [nao verificado] |
| L1/T8 — TXT da tela × comando | **PROCEDE**: tela confere carimbo e injeta cálculo lido; comando monta pelo gravado e registra sem `conferir` | `app/folha/views.py:128`, `app/folha/views.py:209`, `app/folha/views.py:268`, `app/folha/management/commands/regerar_txt_dominio.py:85`, `app/folha/management/commands/regerar_txt_dominio.py:118`, `app/folha/export.py:717` | Cura: [nao verificado] |
| L2/E1 — `_gravar` escreve estado | PROCEDE: criação e atualização continuam nesse escritor | `app/ponto/portas/he.py:44`, `app/ponto/portas/he.py:66`, `app/ponto/portas/he.py:75`, `app/ponto/portas/he.py:84` | O214 — `app/ponto/portas/he.py:47` |
| L2/E2 — `_marcar_revogada` escreve marca | PROCEDE: marca e grava dentro da porta; relógio ainda interno | `app/ponto/portas/he.py:375`, `app/ponto/portas/he.py:401`, `app/ponto/portas/he.py:403` | O273 — `app/ponto/portas/he.py:383` |
| L2/E3 — “selos escrevem direto” | **MUDOU — classificação refutada**: os sítios citados são inspeções AST/texto, não fixtures escrevendo `DecisaoHE` | `app/ponto/tests/test_o214_ponta_pequena.py:325`, `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:426`, `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:445` | Não demonstrada alteração de código; commit: [nao verificado] |
| L2/E4 — `.delete()` do comando | PROCEDE: continua apenas texto de reversão | `app/ponto/management/commands/recusar_ponta_pequena.py:219` | Cura posterior: [nao verificado] |
| L2/E5 — restore escreve `DiaPago` fora da porta | PROCEDE: exclusão e `bulk_create` diretos | `bin/restore_relavratura_10_2026.py:85`, `bin/restore_relavratura_10_2026.py:86` | Cura: [nao verificado] |
| L2/E6 — script passa assinatura inexistente | PROCEDE: passa `permitir_exportada`, ausente na assinatura atual | `bin/relavrar_o120_col221_09.py:74`, `app/ponto/services/fechamento.py:448` | Cura: [nao verificado] |
| L2/E7 — trilha best-effort | PROCEDE: exceção de auditoria é capturada após escrita | `app/ponto/portas/he.py:100`, `app/ponto/portas/he.py:414` | Cura: [nao verificado] |
| L3/S1 — autorizado → não | PROCEDE: mudança de estado chega ao escritor | `app/ponto/portas/he.py:197`, `app/ponto/portas/he.py:206` | Cura posterior: [nao verificado] |
| L3/S2 — não → autorizado | PROCEDE: mesma porta permite troca | `app/ponto/portas/he.py:175`, `app/ponto/portas/he.py:197`, `app/ponto/portas/he.py:206` | Cura posterior: [nao verificado] |
| L3/S3 — recusa revogada sem saída pelo “Não” humano | **CURADO**: decisão nova revive a linha, sem `no_op` | `app/ponto/portas/he.py:196`, `app/ponto/portas/he.py:200`, `app/ponto/tests/test_he_revogada_na_porta.py:50` | HE-REVOGADA-NA-PORTA — `app/docs/RELATO.md:259` |
| L3/S4 — recusa revogada barra período | **CURADO**: somente decisão vigente diferente de autorizado barra | `app/ponto/portas/he.py:589`, `app/ponto/portas/he.py:590`, `app/ponto/tests/test_he_revogada_na_porta.py:92` | HE-REVOGADA-NA-PORTA — `app/docs/RELATO.md:259` |

Resumo: T1b/T2/S3/S4 foram curados pela HE-REVOGADA-NA-PORTA; a divergência de emissão do TXT permanece; a alegação de recuperação pelo cron e a classificação de E3 precisam ser corrigidas. (`app/ponto/portas/he.py:148`, `app/ponto/portas/he.py:197`, `app/ponto/portas/he.py:590`, `app/folha/management/commands/regerar_txt_dominio.py:85`, `app/config/crons.py:1153`, `app/ponto/tests/test_o214_ponta_pequena.py:325`)

Leitores/escritores que o inventário não individualizou:

| sítio adicional | leitura/escrita e consequência |
|---|---|
| Novo `_vigente`, chamado por **quatro** políticas da porta | Lê vigência por `vigentes()`; além dos dois defeitos inventariados, sistema e revogador também migraram para o helper. `app/ponto/portas/he.py:148`, `app/ponto/portas/he.py:347`, `app/ponto/portas/he.py:481` |
| Persistência do retrato HE | `lavrar` escreve `numerador`, `denominador`, `valor` e `detalhe`; `ler` devolve a fotografia com carimbo. `app/ponto/services/he_pendente_lavrado.py:177`, `app/ponto/services/he_pendente_lavrado.py:182`, `app/ponto/services/he_pendente_lavrado.py:193` |
| Escritor final de TXT | `ExportacaoDominio.registrar` cria o registro sem consultar trava HE; a fiscalização continua dependente do chamador. `app/folha/models.py:174`, `app/folha/models.py:182` |
| Foto do portão | `lavrar` persiste lista/flag HE; `conferir` lê `MetricaSnapshot`, não a decisão viva. Portanto a pergunta 5 já tem suporte estático para fotografia que envelhece entre atos; ocorrência em produção: [nao verificado]. `app/folha/porta_export.py:795`, `app/folha/porta_export.py:807`, `app/folha/porta_export.py:814`, `app/folha/porta_export.py:853` |
| Outros consumidores da janela HE | Cadastro versus realidade, vigia do fio e pauta de folga usam `competencias_a_lavrar`; uma mudança nessa janela alcança esses três fluxos. `app/escala/services/cadastro_realidade.py:553`, `app/chamados/services/vigia_fio.py:511`, `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:96` |
| Vigia de fotografias paradas | Compara carimbos dos retratos corrente/anterior, sem relavrar dinheiro. `app/ponto/services/he_pendente_lavrado.py:228`, `app/ponto/services/he_pendente_lavrado.py:249` |

Perguntas finais do inventário: 1/2 respondidas pelo código da cura; 3 permanece; 4 foi parcialmente corrigida, pois o comentário da relavratura agora admite exportada, mas o cabeçalho continua dizendo “não paga”; 5 tem mecanismo estático demonstrado acima; 6 permanece best-effort; 7 continua contraditória, pois a implementação cobre corrente + anterior não entregue; 8 mantém relógio interno, embora `apurar` agora leia um instante único e o injete no espelho. (`app/ponto/portas/he.py:200`, `app/ponto/portas/he.py:589`, `app/folha/management/commands/regerar_txt_dominio.py:85`, `app/ponto/portas/he.py:18`, `app/ponto/portas/he.py:218`, `app/ponto/portas/he.py:414`, `app/ponto/management/commands/lavrar_he_pendente.py:42`, `app/ponto/services/he_pendente_lavrado.py:220`, `app/ponto/services/he_pendente_lavrado.py:84`, `app/ponto/services/he_pendente_lavrado.py:94`, `app/ponto/portas/he.py:401`)

_6392 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
