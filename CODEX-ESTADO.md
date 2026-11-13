# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 08:04** · publicado em **13/11/2026 08:04** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 08:04 · sessao c7334fc8cc25 · - · - · origem=Ronald (tmux)

**c — BLOQUEIA:** a área comum continua gravável: `container()` monta `$SAIDA:/sombra` sem `:ro`; a sonda pode sobrescrever `lavra_prod.json`, `repor_lavra.py` ou outra sonda. `PGOPTIONS` não protege arquivos. A montagem permaneceu inalterada no diff. (HEAD `bin/sombra.sh:189-191`; diff `bin/sombra.sh:499-514` usa esse mesmo `container()`.)

Escritas identificadas no caminho:

- Inicialização cria `$SAIDA` quando ausente. (HEAD `bin/sombra.sh:81-84`.)
- Trava cria `logs/`, abre/cria `sombra.lock` em append e acrescenta evento a `sombra_espera.jsonl`. (Diff `bin/sombra_trava.sh:58-65`.)
- `tee` cria/trunca `logs/$_rot.out`; duas execuções do mesmo script sem rótulos distintos usam o mesmo destino. **FILA:** corrida introduzida pela simultaneidade, com perda/mistura dos resultados. (Diff `bin/sombra.sh:502-514`, conforme posição final a conferir; trecho novo iniciado no hunk `@@ -459,6 +479,21 @@`.)
- Django cria `/app/logs` e abre `errors.log` e `ponto.log`; são tmpfs particulares de cada container. (HEAD `app/config/settings/saas.py:118`, `:138-139`, `:162-163`; `bin/sombra.sh:189`.)
- Docker cria/remove o container; camada gravável local permanece permitida, pois inexiste `--read-only`; `/app` e `/sombra_bin` são mounts somente leitura, `/app/media` é tmpfs. (HEAD `bin/sombra.sh:183-191`.)
- Portão tenta `CREATE TEMP TABLE`; escrita efetivamente aceita depende do runtime **[nao verificado]**. Não há atualização de lavra no novo ramo. (Diff `bin/sombra_confere_leitura.py:21-26`; novo ramo do diff.)
- A sonda é código arbitrário carregado por `exec`; suas escritas concretas não constam no dado **[nao verificado]**. (Novo ramo do diff, comando `exec(open('/sombra/$_scr').read())`.)
- `sombra_db.sh --conferir` faz inspeções e `SELECT 1`; retorna antes das rotinas de criação/update. (HEAD `bin/sombra_db.sh:57-84`, `:86-100`.)
- `md5_lavra_prod` usa processo dentro de `saas_core`, cujo startup Django herda os handlers de arquivo; settings efetivos e escritas reais desse container **[nao verificado]**. (Diff `bin/sombra.sh:145-151`; HEAD `app/config/settings/saas.py:138-139`, `:162-163`.)

**BLOQUEIA adicional:** a cura indicada pelo portão, `--lavra`, continua despachando `lavra_de_prod` sem adquirir trava. Ela pode escrever arquivos e snapshots enquanto leitores seguram a compartilhada. É defeito preexistente que passa a furar o novo contrato. (HEAD `bin/sombra.sh:462`, `:144`, `:151`, `:157-163`; diff `bin/sombra_confere_leitura.py:32-34`.)

**e — FILA:** o selo verifica palavras, não comportamento da porta. Substituir integralmente `sombra_confere_leitura.py` por `print('ok')` mantém o selo verde: ele só exige existência e procura referências/flags/`&&` no shell. Também `echo RECUSADA; return 0` passa (d), e `echo NEGADA; return 0` passa os casos de recusa, que ignoram o rc. (Diff `bin/tests/test_sombra_leitura_compartilhada.sh:13-14`, `:35`, `:38`, `:41`, `:44`, `:49-50`, `:55-66`.)

**f — sem regressão demonstrada nesses consumidores:**

- `modo` é aceito pelo leitor conhecido: `json.loads` e `.get` ignoram campos extras; placar usa apenas a saída agregada. (HEAD `bin/sombra_trava.sh:49-57`; `bin/placar_code.sh:166`.)
- Os outros `container()` existentes recebem array vazio quando `SOMBRA_SO_LEITURA` não existe; variável prefixada na chamada nova não muda persistentemente as chamadas seguintes. (Diff `bin/sombra.sh:200-203`; chamadas HEAD `bin/sombra.sh:166`, `:204-206`, `:266-275`, `:346`, `:351`.)
- Selos conhecidos recortam funções/ramo por delimitadores, não números de linha; o diff preserva seus alvos. Execução contra o diff **[nao verificado]**. (HEAD `bin/tests/test_ensaio_de_dinheiro_sincroniza_lavra.sh:17-27`; `bin/tests/test_bloco_repoe_a_lavra.sh:29-48`; `bin/tests/test_sombra_guarda_num_sitio.sh:25-33`.)
- Outros usuários de fd 8 existem; colisão em shell que sobrescreva esse descritor é anterior ao diff, pois o escritor do HEAD já usa fd 8. (HEAD `bin/sombra_trava.sh:26`; `bin/trava.sh:64`; `bin/integrador.sh:41`; `bin/codex_pergunta.sh:368`.)

## 13/11 08:04 · sessao eed45d568fae · - · - · origem=Ronald (tmux)

- **a — exclusão normal correta:** escritor HEAD usa `flock` exclusivo no mesmo `logs/sombra.lock`; leitor novo usa `flock -s`. Escritor herdado conserva exclusiva; leitura repetida retorna antes de reabrir fd 8; escritor novo sob leitura é recusado. `simular_folha par` conserva a herança nas duas fotos. Referências: HEAD `bin/sombra_trava.sh:16,22,26,28,38`; diff `bin/sombra_trava.sh:29,30,53,54,58,60,69`; HEAD `bin/simular_folha.sh:96-99`.

- **BLOQUEIA se a alegação inclui filhos executando HEAD:** pai segura leitura nova; filho executa função escritora HEAD, que ignora `SOMBRA_TRAVA_LEITURA`, reabre seu fd 8 e espera exclusiva; pai ainda tem compartilhada e aguarda o filho. Resultado: autoespera até timeout. Isso não ocorre com escritor velho independente já rodando, que é excluído corretamente. Referências: diff `bin/sombra_trava.sh:58,60,69`; HEAD `bin/sombra_trava.sh:22,26,28`; HEAD `bin/sombra.sh:461` fornece execução síncrona do comando embrulhado. Existência de uma árvore antiga concreta disponível para esse filho: **[não verificado]**.

- **BLOQUEIA — escritor fora do protocolo:** `--lavra` continua chamando diretamente `lavra_de_prod`, sem exclusiva. Portanto pode gravar durante uma leitura, embora o próprio portão mande executá-lo como remédio. Referências: HEAD `bin/sombra.sh:462`, preservado como diff `bin/sombra.sh:503`; diff `bin/sombra_confere_leitura.py:33`.

- **e — FILA, falso verde de status:** mutante `echo "...RECUSADA..."; return 0` passa (d), que compara somente texto. Os chamadores reais aceitam esse retorno e prosseguem como escritores. B/C/C2/D2 também procuram `NEGADA` sem exigir rc de falha. Referências: diff `bin/tests/test_sombra_leitura_compartilhada.sh:34,37,40,43,49-50`; diff `bin/sombra_trava.sh:31`; HEAD `bin/sombra.sh:456,487`; HEAD `bin/simular_folha.sh:67,96`.

- **e — FILA, corrida restante:** `segura` não segura indefinidamente até `solta`; chama `espera_arq`, que desiste após aproximadamente dez segundos. Um host atrasado pode perder o mantenedor durante os casos; `wait` não transforma sua falha em `FALHA`. Referências: diff `bin/tests/test_sombra_leitura_compartilhada.sh:18,25,35,38,41`.

- **e — FILA, lacunas:** (a) realmente exige sucesso do segundo leitor; D2 verifica continuidade da trava e ausência de novo registro, mas não distingue reabertura com reaquisição imediata sem log. Mutante que reabre e readquire compartilhada sem registrar passa D2 quando nenhum escritor entra na janela. A porta (e) continua sendo verificação textual; `false && lavra_de_prod` seria rejeitado apesar de nunca escrever, enquanto escrita por função auxiliar com outro nome não é detectada. Referências: diff `bin/tests/test_sombra_leitura_compartilhada.sh:33,44-50,54-66`.

- **f — compatibilidade observada:** o consumidor JSONL ignora chaves adicionais; `modo` não quebra sua leitura. O selo que conta a guarda aceita usos adicionais de `$E_SOMBRA`. Fd 8 já era ocupado pelo escritor, portanto outros usos existentes não demonstram regressão introduzida especificamente pelo leitor. Referências: HEAD `bin/sombra_trava.sh:51-57`; HEAD `bin/tests/test_sombra_guarda_num_sitio.sh:25-33`; HEAD `bin/sombra_trava.sh:26`; HEAD `bin/trava.sh:64`; HEAD `bin/codex_pergunta.sh:368-374`.

Nenhum selo foi executado, porque ele cria e remove arquivos; resultados de execução: **[não verificado]**.

## 13/11 07:58 · sessao 65e6cfdf0ed3 · ACELERA-DIFF · verificador · origem=Code

**REPROVA.** Há quebra de exclusividade durante a transição e escrita dentro da própria porta compartilhada. (HEAD `bin/sombra_trava.sh:26–28`; diff `bin/sombra_trava.sh:61–63`; diff `bin/sombra.sh:506–508`; HEAD `bin/sombra.sh:157–166`)

Nas referências ao **diff**, uso a numeração de destino do anexo.

1. **BLOQUEIA — transição antigo/novo permite leitura durante `--refazer` (c).** O escritor antigo segura somente `sombra.lock`; o leitor novo disputa somente `sombra.leitura.lock`, portanto entra enquanto aquele escritor trabalha. O escritor novo independente espera primeiro `sombra.lock` e, depois, a trava dos leitores: ele permanece protegido, mas não protege retroativamente o escritor antigo. (HEAD `bin/sombra_trava.sh:26–28`; diff `bin/sombra_trava.sh:35–41`, `:61–63`)

   Assim, um `--refazer` antigo pode executar `dropdb --force` durante uma leitura nova. Um filho novo do escritor antigo também retorna pelo atalho `SOMBRA_TRAVA_HERDADA=1`, sem adquirir a segunda trava. Reverter enquanto leitores novos continuam vivos recria essa incompatibilidade; o protocolo precisa de transição com drenagem. Ocorrência efetiva em produção: **[não verificado]**. (HEAD `bin/sombra.sh:255–256`; HEAD `bin/sombra_trava.sh:38`; diff `bin/sombra_trava.sh:28`, `:57`)

2. **BLOQUEIA — a porta compartilhada escreve antes da sonda.** Depois de adquirir a trava de leitura, o ramo chama `lavra_de_prod`. Essa função sobrescreve os arquivos comuns `lavra_prod.json` e `repor_lavra.py` e executa dois `update_or_create` no banco. Duas “leituras” podem, portanto, disputar arquivos e modificar os registros que a outra consulta, mesmo com ambas as sondas corretamente guardadas. A guarda da sonda não cobre esse processo anterior. (diff `bin/sombra.sh:506–510`; HEAD `bin/sombra.sh:144`, `:151`, `:157–166`)

3. **BLOQUEIA — o `grep` não comprova ausência de escrita (d).** A conferência exige apenas uma ocorrência textual e retorna rc 2 quando ela falta. Passam, por exemplo: uma string contendo `execute_wrapper(guarda_sql)`; uma chamada em ramo nunca executado; um wrapper ativo apenas durante parte do programa; escrita por outra conexão; ou uma `guarda_sql` permissiva. Nenhuma dessas propriedades é examinada pelo lançador. Existência desses casos nas sondas reais: **[não verificado]**. (diff `bin/sombra.sh:503–505`)

4. **FILA — a ordem normal das duas travas não forma ciclo; a herança pressupõe posse válida (a).** Escritores adquirem exclusiva em 8 e depois exclusiva em 9; leitores adquirem somente compartilhada em 9. Dois escritores se serializam em 8. O orçamento restante da segunda aquisição desconta a espera da primeira, com padrão de 1800 segundos. Não aparece ciclo de aquisição nesse protocolo. (diff `bin/sombra_trava.sh:24`, `:35–41`, `:61–63`)

   Depois de sucesso, repetir `trancar_sombra` no mesmo shell, ou chamá-la num filho com herança válida, retorna antes de reabrir descritores. Um escritor solicitado dentro de leitor é recusado antes da aquisição. Entretanto, a marca herdada não verifica os descritores e precede a recusa de leitura — daí a falha de transição do item 1. (diff `bin/sombra_trava.sh:28–31`, `:51–52`, `:57`)

   Já repetir `trancar_sombra_leitura` reabre 9: não existe atalho para `SOMBRA_TRAVA_LEITURA=1`. Se aquela era a última referência à trava anterior, a proteção pode ser interrompida entre as duas aquisições. (diff `bin/sombra_trava.sh:57–63`, `:72`)

5. **FILA — chamadores e descritor 9 (b).** Os chamadores diretos encontrados na cópia são:

   - `sombra.sh`: source em `HEAD bin/sombra.sh:87`; aquisições em `:438`, `:451`, `:456`, `:487`.
   - `simular_folha.sh`: source em `HEAD bin/simular_folha.sh:45`; aquisições em `:67`, `:96`.
   - `diff_janela_he_total.sh`: source e aquisição em `HEAD bin/diff_janela_he_total.sh:17–18`.

   `simular_folha par` inicia dois filhos `foto`, que pedem novamente a trava e dependem do atalho herdado. `--com-a-sombra` também executa um comando filho mantendo o shell pai com as travas. (HEAD `bin/simular_folha.sh:96–99`, `:67`; HEAD `bin/sombra.sh:456–461`)

   Há outros protocolos usando 9, como régua e pre-push, mas isso não demonstra colisão: reabrir o descritor somente num filho não fecha a cópia mantida pelo pai. Colisão concreta nos três chamadores diretos: **[não verificado]**. (HEAD `bin/regua.sh:97–103`; HEAD `bin/pre-push.sh:80–87`; HEAD `bin/sombra.sh:461`)

6. **FILA — o selo morde parte do protocolo, com lacunas (e).**

   - **a/b/c:** as aquisições ocorrem em subshells distintos, abrindo seus próprios descritores; não há falso compartilhamento causado por um descritor aberto pelo filho e herdado pelo pai. Porém a sobreposição depende de sinal seguido de `sleep 4`. Uma pausa de escalonamento pode fazer **a**, isoladamente, passar sem sobreposição; nesse cenário, **b** pode tornar o selo vermelho. (diff `bin/tests/test_sombra_leitura_compartilhada.sh:15–32`)
   - **d:** aceita qualquer retorno diferente de zero como recusa, sem distinguir `RECUSADA`, timeout ou outro erro; também testa a chamada no mesmo shell, sem criar um filho leitor. (diff `bin/tests/test_sombra_leitura_compartilhada.sh:33–35`)
   - **e:** verifica posições de palavras e uma grafia específica da chamada exclusiva. Não executa a porta nem comprova rc 2 sem guarda; uma conferência textual ineficaz pode satisfazer o selo. (diff `bin/tests/test_sombra_leitura_compartilhada.sh:36–41`)
   - Não cobre transição de versões, herança válida, repetição da aquisição, JSONL, CPU ou a escrita da lavra. Execução efetiva do selo: **[não verificado]**. (diff `bin/tests/test_sombra_leitura_compartilhada.sh:26–43`)

7. **FILA — `"modo"` preserva o parsing, mas amplia o contador agregado (f).** O produtor atual é `trancar_sombra`; o patch acrescenta `trancar_sombra_leitura`, com `"modo":"leitura"`. `esperas_longas_hoje` considera data, `espera_s` e `ok`, ignorando campos adicionais: leituras demoradas ou negadas entram na mesma conta. (HEAD `bin/sombra_trava.sh:30–31`, `:49–60`; diff `bin/sombra_trava.sh:65–66`)

   `placar_code.sh` consome esse contador sem separar modos, embora sua descrição ainda mencione somente `sombra.lock`. A recusa de escritor dentro de leitor retorna antes do registro e não entra na conta. (HEAD `bin/placar_code.sh:166–171`; diff `bin/sombra_trava.sh:28–30`, `:44–45`)

8. **FILA — configuração de CPU existe; medida de parede não foi demonstrada.** O container final da sonda recebe `--cpu-shares 256`; o container anterior da lavra não recebe essa atribuição pelo ramo. O JSONL mede espera pela trava; tempo de parede da execução e ganho entre duas sondas: **[não verificado]**. (diff `bin/sombra.sh:188`, `:508–511`; HEAD `bin/sombra.sh:166`; diff `bin/sombra_trava.sh:62–66`)

## 13/11 07:56 · sessao 46fd062e5518 · - · - · origem=Ronald (tmux)

1. **bloqueia — a porta de leitura escreve antes da sonda.** O ramo chama `lavra_de_prod` sob trava compartilhada (`diff bin/sombra.sh:506`, `:508`); essa função sobrescreve arquivos comuns (`HEAD bin/sombra.sh:144`, `:151`) e executa dois `update_or_create` no banco (`HEAD bin/sombra.sh:157`, `:160`, `:166`). A guarda da sonda não cobre esse processo anterior (`diff bin/sombra.sh:509-510`). Duas leituras podem disputar arquivos e atualizar os mesmos registros; ocorrência concreta em produção [não verificado].

2. **bloqueia — a conferência textual não prova ausência de escrita.** O `grep` exige apenas uma ocorrência textual (`diff bin/sombra.sh:504-505`). Contraexemplos que satisfazem a expressão: ocorrência dentro de string/docstring; chamada em `if False`; wrapper instalado apenas dentro de um `with`, com escrita depois; escrita por outra conexão; implementação permissiva de `guarda_sql` (`diff bin/sombra.sh:504`). Existência desses casos nas sondas reais [não verificado]. Se a guarda apenas permite prefixos SELECT/WITH, DML em CTE e funções com efeitos chamados por SELECT também precisam de análise; implementação concreta da guarda [não verificado].

3. **fila — o selo verifica parte do protocolo, sem comprovar o contrato todo.** As tentativas usam subshells distintos: o pai não recebe o descritor aberto pelo filho; portanto os casos a/b/c não têm, por construção, o falso sucesso de dois `flock` sobre o mesmo descritor herdado (`diff bin/tests/test_sombra_leitura_compartilhada.sh:15-24`). Entretanto:
   - A sobreposição depende do sinal seguido de `sleep 4`; uma pausa de escalonamento pode deixar o leitor acabar antes da segunda tentativa. O caso a isoladamente poderia passar sem simultaneidade, e b/c podem produzir falha por temporização (`diff ...:18-19`, `:26-32`).
   - O caso d aceita qualquer falha do escritor, incluindo timeout ou outro erro, sem conferir mensagem RECUSADA ou recusa imediata (`diff ...:33-35`).
   - O caso e verifica posições de palavras e uma grafia específica da chamada exclusiva; não executa a porta nem valida rejeição rc 2 (`diff ...:36-41`).
   - Não existem casos para herança, repetição de aquisição, transição de versão antiga, JSONL ou `--cpu-shares` no teste proposto (`diff ...:26-43`). [nao verificado]

Execução efetiva do selo e comportamento das sondas concretas [não verificado].

## 13/11 07:55 · sessao 19b7cbbbed23 · - · - · origem=Ronald (tmux)

**b — fila:** Os chamadores diretos encontrados são `bin/sombra.sh:87` (aquisições em `:438`, `:451`, `:456`, `:487`), `bin/simular_folha.sh:45` (aquisições em `:67`, `:96`) e `bin/diff_janela_he_total.sh:17-18`. Não há uso próprio de fd9 nesses arquivos; colisão concreta nesses chamadores: **não encontrada**. Outros scripts usam fd9, como `bin/regua.sh:97-103`, mas isso isoladamente não demonstra colisão.

`simular_folha par` chama dois filhos `foto`, que pedem a trava novamente; a marca exportada permite retornar antes de reabrir descritores (`bin/simular_folha.sh:96-98`, `bin/simular_folha.sh:67`, `bin/sombra_trava.sh:22`, `bin/sombra_trava.sh:38`). Depois do patch, essa autorização precisa corresponder à posse dos dois descritores; um pai antigo possui apenas fd8 (`bin/sombra_trava.sh:26-28`; diff `bin/sombra_trava.sh:32-38`).

Um comando de `--com-a-sombra` roda como filho enquanto o shell pai conserva as travas; reabrir fd9 somente nesse filho não libera a cópia do pai (`bin/sombra.sh:456-461`).

**f — fila:** O produtor atual do JSONL é `trancar_sombra`; o patch acrescenta `trancar_sombra_leitura` (`bin/sombra_trava.sh:30-31`; diff `bin/sombra_trava.sh:65-66`). O leitor direto encontrado é `esperas_longas_hoje`, que considera `ts`, `espera_s` e `ok`, ignorando campos adicionais; portanto `"modo":"leitura"` não quebra seu parsing e pedidos de leitura negados ou demorados passam a integrar o mesmo contador (`bin/sombra_trava.sh:49-60`). `bin/placar_code.sh:166-171` consome esse contador agregado; sua descrição ainda menciona somente `logs/sombra.lock` (`bin/placar_code.sh:169`).

A recusa de escritor dentro de leitor retorna antes do `printf` do JSONL; essa recusa não entra no contador (`diff bin/sombra_trava.sh:28-30`, `:42-43`).

## 13/11 01:32 · sessao e6a36b1c5425 · O168 · verificador · origem=Code

**REPROVA. Bloqueantes encontrados:**

1. **Falta a nota exigida no GREEN:** a recusa grava auditoria, enquanto S88 retorna antes de criar a nota do fio. (`app/chamados/models.py:719`; `app/chamados/models.py:408`; `app/chamados/signals.py:43`; `app/chamados/signals.py:46`.)
2. **A catraca deixa sobreviver um sétimo fechador:** reconhece apelido de importação, mas ignora atribuição `t = transicionar_com_pauta`; o selo complementar autoriza esse fechamento por estar dentro da porta. (`app/chamados/tests/test_o168_fechadores_fora_do_gate.py:83`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:87`; `app/chamados/tests/e4b_c12_ast.py:246`.)
3. **A declaração de juiz único não cobre um predicado paralelo divergente:** o cartório decide preservar resposta pelo texto; CSF decide pelo carimbo. (`app/ponto/services/cartorio.py:1637`; `app/ponto/services/cartorio.py:1648`; `app/ponto/services/cartorio.py:1665`; `app/ponto/supra_juiz.py:557`; `app/docs/CORTES.md:448`.)

**A. Gate, disputa e RegularizacaoExterna**

Para o pai comum de categoria `disputa_supervisao`, a cascata chega ao gate, encontra pergunta com `respondida_em` e retorna sem gravar o fechamento. A disputa conserva seu fechamento, com data, autor e resolução; desaparece a trava específica de RegularizacaoExterna quando não sobra outra disputa aberta do colaborador. Isso libera essa precondição, sem garantir o sucesso das demais etapas da regularização. (`app/chamados/signals.py:43`; `app/chamados/models.py:686`; `app/chamados/models.py:713`; `app/chamados/models.py:1962`; `app/chamados/services/regularizacao_ext.py:59`.)

Não encontrei fechador **atual** de sistema fora da lista nominal: desligamento, retenção, regularização externa, vínculo, competência trancada e os três comandos estão enumerados no selo. A falha demonstrada é de cobertura contra fechador novo, descrita em F. (`app/chamados/tests/test_o168_fechadores_fora_do_gate.py:26`; `app/colaboradores/services/desligamento.py:156`; `app/chamados/reconciliador.py:497`; `app/chamados/services/regularizacao_ext.py:206`; `app/colaboradores/services/vinculo.py:388`; `app/chamados/services/competencia_trancada.py:209`.)

Há dois limites: pais de outra categoria passam sem consulta; e, apesar do comentário dizer “só PAI”, a função chamada também examina perguntas nas quais o chamado é **destino**. Ocorrência desses arranjos na produção: [não verificado]. (`app/chamados/models.py:704`; `app/chamados/models.py:713`; `app/ponto/supra_juiz.py:559`.)

**B. Estado na recusa e repetição**

Na recusa O168, `_f1_dedup` já foi retirado da instância; a variável local é descartada, sem publicar `fechamento_por_dedup`. `_f1_ancoradas` vira `None`. O gate da pergunta viva ainda não executou, portanto essa passagem não reaponta perguntas. `reconciliar` retorna antes de `_gravar_estado`, preservando status, carimbo de resolução e urgência. (`app/chamados/models.py:685`; `app/chamados/models.py:687`; `app/chamados/models.py:689`; `app/chamados/models.py:694`; `app/chamados/models.py:652`.)

Duas recusas consecutivas com payload igual e sem mudança posterior de estado deduplicam em uma linha; o dry-run não registra a recusa. Não é garantia de uma linha histórica para sempre: outro motivo ou transição intermediária permite nova linha. A gravação também é best-effort, podendo deixar zero linhas se falhar. (`app/chamados/models.py:403`; `app/chamados/models.py:405`; `app/chamados/models.py:718`; `app/core/observ.py:98`; `app/core/observ.py:139`.)

Existe escrita anterior **condicional**: pai que também seja reconhecido como chamado de furo, sem dia forte, grava `fechamento_sem_dia_do_fato` antes da recusa O168; esse evento não deduplica. Não demonstra fechamento contraditório, mas impede afirmar ausência universal de escrita anterior ou trilha única total. Existência desse caso na produção: [não verificado]. (`app/chamados/models.py:683`; `app/chamados/models.py:757`; `app/chamados/models.py:762`; `app/chamados/models.py:764`; `app/chamados/models.py:1274`.)

A recusa nova não cria nota explicando a retenção: cria `LogAuditoria`; S88 abandona a nota de fechamento. A timeline consulta Followups. Esse é o bloqueante do GREEN “pai aberto com nota” fornecido no aval. (`app/chamados/models.py:408`; `app/chamados/signals.py:43`; `app/chamados/signals.py:46`; `app/chamados/views.py:372`; `app/templates/chamados/detalhe_local.html:112`.)

**C. Depois de decisão humana**

- **Validação:** validação e materialização ficam gravadas; a cauda `_fechar_disputa_se_quitada` fecha a disputa por C7, S88 pede o fechamento do pai e O168 o retém. O gate não desfaz perguntas validadas/materializadas. A mesma cauda também é chamada pelo passivo validado. (`app/chamados/services/validacao.py:435`; `app/chamados/services/validacao.py:463`; `app/chamados/services/validacao.py:469`; `app/chamados/services/materializacao.py:1384`; `app/chamados/services/materializacao.py:1339`; `app/chamados/services/passivo_validado.py:405`.)
- **Retratação por premissa morta:** `_retratar` retorna na recusa, antes de auto-rejeitar a justificativa, resolver perguntas e anexar a nota de retratação. Assim, essa unidade deixa disputa, perguntas e justificativa como estavam na entrada; justificativa pendente permanece pendente. Efeitos anteriores de outros atos não são revertidos por esse retorno. (`app/chamados/reconciliador.py:524`; `app/chamados/reconciliador.py:530`; `app/chamados/reconciliador.py:537`; `app/chamados/reconciliador.py:539`.)

Existe porta humana real: view do detalhe → resposta livre → `transicionar_com_pauta` sem `ato` → `transicionar`. Ela não consulta O168, mas continua sujeita às guardas de justificativa pendente, disputa/pergunta viva e premissa do furo. Portanto há porta posterior, sem promessa de que qualquer estado possa fechá-la imediatamente. (`app/chamados/views.py:546`; `app/chamados/services/acoes_chamado.py:319`; `app/chamados/services/acoes_chamado.py:322`; `app/chamados/services/fecho_runtime.py:181`; `app/chamados/models.py:135`; `app/chamados/models.py:148`; `app/chamados/models.py:181`.)

O supervisor recebe mensagens de pergunta validada/ponto gravado ou disputa regularizada, sem aviso específico do pai retido. No modal pode aparecer chamado vivo junto de “Disputa fechada”; no detalhe, a seção da disputa consulta somente disputa aberta. (`app/chamados/services/acoes_disputa.py:57`; `app/chamados/services/acoes_disputa.py:162`; `app/templates/chamados/partials/modal_fio.html:102`; `app/templates/chamados/partials/modal_fio.html:379`; `app/chamados/services/acoes_chamado.py:342`.)

**D. Predicado, universo e segundo juiz**

O corpo de CSF permanece igual no diff fornecido: procura carimbo de resposta tanto na origem quanto nos destinos. Na cópia há **dois chamadores de produção**, command e gate; o próprio registro declara um antes e dois depois, não três. Confirmação independente do HEAD base pelo histórico Git: [não verificado]. (`app/ponto/supra_juiz.py:551`; `app/ponto/management/commands/supra_juiz.py:261`; `app/chamados/models.py:716`; `app/docs/CORTES.md:448`.)

O predicado não mudou, mas o universo **das invocações** aumentou: o command consulta seus chamados vivos; o gate acrescenta reconciliações de categoria `disputa_supervisao`. “Mesmo universo” não descreve essa mudança de alcance. (`app/ponto/management/commands/supra_juiz.py:260`; `app/chamados/models.py:713`.)

Há predicado paralelo: `_tem_resposta_do_colab` considera texto não vazio nas perguntas que resolvem o chamado e impede sua reconciliação nesses ramos. Em ensaio das funções reais extraídas em memória, texto sem carimbo foi preservado pelo cartório e liberado por CSF; carimbo sem texto produziu o inverso. A divergência está diretamente nos critérios, e não foi coberta pelo censo baseado apenas em `disputa_origem`. (`app/ponto/services/cartorio.py:1641`; `app/ponto/services/cartorio.py:1648`; `app/ponto/services/cartorio.py:1665`; `app/ponto/supra_juiz.py:557`; `app/ponto/supra_juiz.py:560`; `app/docs/CORTES.md:448`.)

**E. Contador**

Lê a trilha, sem rejulgar. Seu universo é exatamente “chamados atualmente VIVOS com **qualquer recusa histórica** desse motivo”. Não exige última recusa, categoria atual nem nova recusa após reabertura; um pai reaberto pode voltar à contagem pela trilha antiga. Portanto corresponde ao universo histórico declarado, não necessariamente a uma retenção no ciclo atual. (`app/chamados/juizes.py:2519`; `app/chamados/juizes.py:2522`.)

Não há leitor de produção encontrado; o consumidor é o teste. A documentação aponta equivocadamente `models.py` como endereço do contador. (`app/chamados/juizes.py:2515`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:59`; `app/docs/JUIZES.md:92`.)

**F. Mordida da catraca**

Detecta sítio novo reconhecido, aumento de chamadas e remoção de sítio listado; reconhece `from ... import ... as apelido` e atribuição de atributo. Não reconhece atribuição entre nomes nem chamada por `getattr`. (`app/chamados/tests/test_o168_fechadores_fora_do_gate.py:83`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:87`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:100`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:139`.)

Este mutante sintético sobreviveu ao reconhecimento O168; o E4B reconheceu a porta e autorizou o ato, sem acusar escritor externo. (`app/chamados/tests/test_o168_fechadores_fora_do_gate.py:87`; `app/chamados/tests/e4b_c12_ast.py:246`.)

```python
from chamados.services.fecho_runtime import transicionar_com_pauta
t = transicionar_com_pauta

def fechar_pelo_sistema(ch):
    t(ch, 'fechado', None, ato=ch.encerrar)
```

Os três comandos constam somente em `FORA_DE_PIPELINE` na fonte atual. O teste verifica pertencimento e ocorrência textual única entre aspas simples; isso confirma a disposição nominal atual, mas não constitui análise semântica de agendamento — aspas duplas escapam da contagem. Cron instalado: [não verificado]. (`app/config/crons.py:1317`; `app/config/crons.py:1441`; `app/config/crons.py:1443`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:169`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:177`.)

**G. Órfãs, deslocamento e custo**

O pai vivo é consequência estática do gate: a recusa impede `_gravar_estado` e interrompe `_retratar`. Entretanto, a explicação exata da troca **2→1** não está demonstrada pelo material: `r['falhas']` conta **lotes de colaborador com falha**, não chamadas a `fechar`. Duas exceções no mesmo lote continuam contando uma; duas unidades exigem outra passagem/lote. A medição alegada no comentário e a origem dessa passagem adicional: [não verificado]. (`app/chamados/models.py:652`; `app/chamados/reconciliador.py:524`; `app/chamados/reconciliador.py:1352`; `app/chamados/reconciliador.py:1403`; `app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:155`.)

`CORPO +29` é deslocamento: preserva os mesmos nove membros e nenhuma exceção adicional. Já os tetos +3 são aumento permitido de custo, com justificativa compatível com as consultas do juiz; os valores empíricos 220/280 e a constância nos cenários não puderam ser confirmados sem os logs citados. (`app/chamados/tests/e4b_c12_ast.py:18`; `app/chamados/models.py:686`; `app/chamados/models.py:699`; `app/ponto/tests/test_selo_chokepoint_escrita.py:196`; `app/ponto/tests/test_selo_chokepoint_escrita.py:210`; `app/ponto/supra_juiz.py:555`.)

**H. Escritores, leitores e significado**

| Campo/valor | Escrita | Leitura e significado |
|---|---|---|
| `status_local` | Núcleo `_gravar_estado`; simulação em `_vivas_entre` restaura a memória. (`app/chamados/models.py:228`; `app/chamados/models.py:356`; `app/chamados/models.py:371`.) | Motor distingue VIVOS, ENCERRADOS e TERMINAIS; contador usa VIVOS. (`app/chamados/catalogo/motor.py:24`; `app/chamados/catalogo/motor.py:30`; `app/chamados/juizes.py:2522`.) |
| `resolvido_em` | Núcleo carimba terminal, preserva resolvido→fechado e limpa ao voltar vivo. (`app/chamados/models.py:242`; `app/chamados/models.py:248`.) | Painel conta histórico; reabertura usa o instante para procurar fato novo. Não significa exclusivamente status `resolvido`. (`app/chamados/services/painel.py:244`; `app/chamados/models.py:1372`.) |
| Trilha de recusa | Gate → `_registrar_recusa` → `evento`. (`app/chamados/models.py:719`; `app/chamados/models.py:408`; `app/core/observ.py:134`.) | Contador lê qualquer ocorrência histórica; deduplicador compara a última recusa e transições posteriores. (`app/chamados/juizes.py:2519`; `app/chamados/models.py:403`.) |
| `respondida_em` | Resposta, resposta automática, recuperação pela trilha e simetria; reabertura/ressuscitação limpam. (`app/chamados/services/disputa_emissao.py:1468`; `app/chamados/services/disputa_emissao.py:1653`; `app/chamados/services/disputa_emissao.py:1674`; `app/chamados/models.py:3162`; `app/chamados/services/disputa_emissao.py:1593`; `app/chamados/models.py:1596`.) | CSF exige só carimbo; C6 exige carimbo e texto no degrau da resposta; cartório exige texto. (`app/ponto/supra_juiz.py:557`; `app/chamados/juizes.py:1417`; `app/ponto/services/cartorio.py:1641`.) |
| `validada_em` | Decisão de validar e simetria; nova resposta, devolução, reabertura e ressuscitação limpam. (`app/chamados/services/validacao.py:134`; `app/chamados/models.py:3165`; `app/chamados/services/disputa_emissao.py:1480`; `app/chamados/services/validacao.py:199`; `app/chamados/services/disputa_emissao.py:1593`; `app/chamados/models.py:1596`.) | C6 encerra a pergunta nesse degrau; estado apresenta VALIDADA/RESOLVIDA; validação evita novo ato. (`app/chamados/juizes.py:1401`; `app/chamados/models.py:3071`; `app/chamados/services/validacao.py:396`.) |

**Não há significado universal idêntico:** `validada_em` também representa simetria, e `respondida_em` sozinho não assegura fala com texto. Esses usos já estão documentados no código; não são escritores novos introduzidos pelo diff. (`app/chamados/services/validacao.py:19`; `app/chamados/services/disputa_emissao.py:1983`; `app/chamados/services/materializacao.py:830`.)

**I. Classes de reprovação**

Presentes: **predicados paralelos divergentes**, **selo sem mordida completa** e **campo interpretado com critérios diferentes pelos leitores**. A nota ausente também deixa a retenção sem explicação no fio apresentado ao supervisor. (`app/ponto/services/cartorio.py:1641`; `app/ponto/supra_juiz.py:557`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:87`; `app/chamados/signals.py:43`; `app/chamados/views.py:372`.)

Não identifiquei novo escritor de status fora da porta, novo relógio ou nova aquisição de trava no trecho acrescentado. Execução concorrente, suite ORM, mutante de retirada da guarda, números de custo e sequência de pouso/pré-requisitos: **[não verificado]**. (`app/chamados/models.py:686`; `app/chamados/models.py:699`.)

## 13/11 01:31 · sessao b845571b3f94 · - · - · origem=Ronald (tmux)

D. O corpo de `pode_executar_csf` continua sendo o predicado “existe `respondida_em` na origem OU nas perguntas que resolvem”; não exige texto, disputa aberta, pergunta viva ou categoria. Há dois chamadores de produção na cópia: command e gate. O registro diz um antes e dois depois; a alegação de três chamadores no aval fornecido não confere com a cópia. O histórico Git da base não está disponível para confirmação independente. (`app/ponto/supra_juiz.py:551`, `app/ponto/management/commands/supra_juiz.py:261`, `app/chamados/models.py:716`, `app/docs/CORTES.md:448`; histórico independente [não verificado].)

O universo **das invocações** aumentou: o command pergunta pelos `vivos`; o novo gate pergunta por chamados com categoria `disputa_supervisao`. A função não mudou, mas “mesmo universo antes e depois” não descreve seus chamadores. (`app/ponto/management/commands/supra_juiz.py:260`, `app/chamados/models.py:713`.)

Existe outro predicado que preserva resposta do colaborador contra reconciliação automática: `_tem_resposta_do_colab` usa **texto não vazio**, apenas nas perguntas que resolvem o chamado; seus dois leitores evitam reconciliar o chamado. Isso diverge do CSF em texto sem carimbo, carimbo sem texto e conversa apenas na origem. A declaração de autoridade única baseada somente no grep de `disputa_origem` não cobre esse predicado. Não encontrei função literal `tem_conversa`; o candidato concreto é este. (`app/ponto/services/cartorio.py:1637`, `app/ponto/services/cartorio.py:1648`, `app/ponto/services/cartorio.py:1665`, `app/ponto/supra_juiz.py:555`, `app/docs/CORTES.md:448`.)

Há ainda uma divergência entre comentário e execução: o gate diz que a metade DESTINO do juiz “não se aplica”, mas chama a função inteira. Um chamado da categoria `disputa_supervisao`, sem disputa de origem e com pergunta respondida apontando para ele como destino, também será recusado como pai. A existência desse arranjo na produção é [não verificado]. (`app/chamados/models.py:704`, `app/chamados/models.py:713`, `app/chamados/models.py:716`, `app/ponto/supra_juiz.py:559`.)

E. O contador lê a trilha e não rejulga. Conta chamados atualmente VIVOS que tenham **qualquer** recusa histórica com aquele motivo; não exige última recusa, nova recusa após reabertura, categoria ou disputa atual. Assim, é exatamente “vivos com trilha histórica”, não necessariamente “recusados no ciclo atual”. Não tem leitor de produção; o teste é seu consumidor. A documentação aponta o módulo errado (`models.py`). (`app/chamados/juizes.py:2510`, `app/chamados/juizes.py:2519`, `app/chamados/juizes.py:2522`, `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:59`, `app/docs/JUIZES.md:92`.)

H. Mapa de escritores e leitores relevantes:

| Campo/valor | Quem escreve | Quem lê / significado |
|---|---|---|
| `status_local` | `_gravar_estado`; `_vivas_entre` apenas simula e restaura memória. (`app/chamados/models.py:228`, `app/chamados/models.py:356`, `app/chamados/models.py:371`.) | Catálogo define VIVOS/ENCERRADOS/TERMINAIS; contador usa VIVOS; gate restringe categoria. (`app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:30`, `app/chamados/juizes.py:2522`, `app/chamados/models.py:713`.) |
| `resolvido_em` | `_gravar_estado`: carimba terminal, preserva resolvido→fechado e limpa ao voltar vivo. (`app/chamados/models.py:242`, `app/chamados/models.py:248`.) | Painel conta histórico pelo carimbo, separando autoria por `respondido_por`. (`app/chamados/services/painel.py:241`, `app/chamados/services/painel.py:244`, `app/chamados/services/painel.py:249`.) |
| Recusa `conversa_exige_decisao_humana` | Gate entrega payload a `_registrar_recusa`; este compara última recusa e mudança posterior de estado; `evento` persiste. (`app/chamados/models.py:719`, `app/chamados/models.py:403`, `app/core/observ.py:134`.) | Contador lê toda a história daquele motivo, não só última linha. (`app/chamados/juizes.py:2519`.) |
| `respondida_em` | Resposta real, resposta automática de fato entregue por outro caminho, recuperação pela trilha e simetria de `carimbar`; reabertura/ressuscitação zeram. (`app/chamados/services/disputa_emissao.py:1468`, `app/chamados/services/disputa_emissao.py:1653`, `app/chamados/services/disputa_emissao.py:1674`, `app/chamados/models.py:3162`, `app/chamados/services/disputa_emissao.py:1593`, `app/chamados/models.py:1596`.) | CSF aceita só timestamp; C6 exige timestamp **e texto** no degrau da resposta; cartório aceita texto. (`app/ponto/supra_juiz.py:557`, `app/chamados/juizes.py:1417`, `app/ponto/services/cartorio.py:1641`.) |
| `validada_em` | Decisão de validar; simetria de carimbo também preenche. Nova resposta, devolução, reabertura e ressuscitação podem limpar. (`app/chamados/services/validacao.py:134`, `app/chamados/models.py:3165`, `app/chamados/services/disputa_emissao.py:1480`, `app/chamados/services/validacao.py:199`, `app/chamados/services/disputa_emissao.py:1593`, `app/chamados/models.py:1596`.) | C6 mata no primeiro degrau; estado exibe VALIDADA/RESOLVIDA; validação evita segundo ato. (`app/chamados/juizes.py:1401`, `app/chamados/models.py:3071`, `app/chamados/services/validacao.py:396`.) |

Não há equivalência universal de significado: `validada_em` também é preenchido por simetria, e `respondida_em` sozinho não prova fala com texto. A documentação já admite ambas as condições; são anteriores ao novo gate. (`app/chamados/services/validacao.py:19`, `app/chamados/services/materializacao.py:830`, `app/chamados/services/disputa_emissao.py:1983`.)

A trilha é best-effort: a recusa pode preservar o pai sem conseguir gravar a linha; nesse caso o contador não o verá. Repetições consecutivas com mesmo payload e sem nova escrita de estado deduplicam; alternância de motivos não garante uma única linha histórica. (`app/core/observ.py:98`, `app/core/observ.py:105`, `app/core/observ.py:139`, `app/chamados/models.py:403`.)

I. Há evidência concreta de predicados paralelos para preservar resposta contra cascata, com critérios distintos; não é sustentada a declaração irrestrita de juiz único. Também há campos com usos distintos já documentados (`validada_em`: decisão/simetria; `respondida_em`: projeção com possível passivo sem texto). A classificação desses achados como bloqueantes da O168 depende da unidade do aval, mas a afirmação de unicidade/mismo universo precisa ser corrigida ou demonstrada com censo abrangente. (`app/ponto/services/cartorio.py:1637`, `app/ponto/supra_juiz.py:551`, `app/docs/CORTES.md:448`, `app/chamados/services/validacao.py:19`, `app/chamados/services/materializacao.py:830`.)

## 13/11 01:31 · sessao a58117558288 · - · - · origem=Ronald (tmux)

**A — árvore atual.** Não achei fechador atual de `ChamadoColaborador` por sistema fora da lista: os atos encontrados estão em `colaboradores/services/desligamento.py:156`, `colaboradores/services/vinculo.py:388`, `chamados/services/competencia_trancada.py:209`, `chamados/services/regularizacao_ext.py:206`, `chamados/reconciliador.py:497` e nos três comandos em `escala/management/commands/fechar_avisos_regua_na_lista.py:48`, `ponto/management/commands/fechar_chamados_vinculo_na_lista.py:58`, `ponto/management/commands/fechar_chamados_meio_periodo.py:110`. O censo puro O168 executado em memória produziu 21 sítios/23 chamadas e nenhuma divergência; o censo E4B integral produziu zero escritores fora da porta — mecanismos em `chamados/tests/test_o168_fechadores_fora_do_gate.py:113` e `chamados/tests/e4b_c12_ast.py:221`.

**F — mordida e bloqueante concreto.** O selo detecta o mutante nominal com importação `as`, chamada adicional e remoção de sítio: reconhecimento em `chamados/tests/test_o168_fechadores_fora_do_gate.py:82`, comparação em `:139`, asserções em `:151`, mutantes em `:201` e `:214`. Também reconhece `t = modulo.transicionar_com_pauta`; não reconhece `t = transicionar_com_pauta` nem `getattr(modulo, 'transicionar_com_pauta')` — o reconhecimento de atribuição aceita apenas `ast.Attribute`, em `:87`, e de chamada aceita apenas `ast.Name` registrado ou `ast.Attribute`, em `:100`.

Reproduzi exclusivamente em memória um **sétimo fechador que passa pelos dois censos**:

```python
from chamados.models import ChamadoColaborador as C
from chamados.services.fecho_runtime import transicionar_com_pauta
t = transicionar_com_pauta

def fechar_pelo_sistema(pk):
    ch = C.objects.get(pk=pk)
    t(ch, 'fechado', None, ato=ch.encerrar)
```

A catraca O168 não o conta, pela limitação acima (`chamados/tests/test_o168_fechadores_fora_do_gate.py:87`); o E4B reconhece a porta canônica e autoriza o ato, portanto também não acusa escritor fora da porta (`chamados/tests/e4b_c12_ast.py:246`). Resultado observado: O168 `{}`; E4B `NEW_WRITER_ROWS=[]`, porta sintética reconhecida e zero violações na árvore completa. Trata-se de **mutante sobrevivente**, não de caminho atual de produção; contraria a catraca geral de novos fechadores exigida no aval fornecido. A execução em memória não possui arquivo persistido de resultado [não verificado].

**Cron.** Os três nomes aparecem somente em `FORA_DE_PIPELINE`, cujo dicionário começa em `app/config/crons.py:769`: `fechar_chamados_meio_periodo` em `:1317`, `fechar_avisos_regua_na_lista` em `:1441`, `fechar_chamados_vinculo_na_lista` em `:1443`. Na fonte atual não há outra ocorrência nominal. O teste verifica pertencimento e uma ocorrência textual entre aspas simples (`chamados/tests/test_o168_fechadores_fora_do_gate.py:169`); cron efetivamente instalado [não verificado]. A contagem textual tem fragilidade futura: uma agenda com aspas duplas escaparia desse critério (`:177`).

**G — deslocamento e custo.** `CORPO` continua com nove entradas e `EXCECOES` vazio; o ajuste mantém as identidades nominais dos mesmos escritores (`chamados/tests/e4b_c12_ast.py:18`). O deslocamento de 29 linhas corresponde às três linhas inseridas na decisão e ao método de 26 linhas (`chamados/models.py:686`, `:699`). O censo integral atual sem violações é compatível com deslocamento, não com ampliação de exceções (`chamados/tests/e4b_c12_ast.py:221`).

Os tetos 220 e 280 e a atribuição de +3 são registrados em `ponto/tests/test_selo_chokepoint_escrita.py:196` e `:210`. O juiz realiza as três leituras descritas para um pai com uma disputa e nenhuma resposta (`ponto/supra_juiz.py:555`, `:556`, `:559`). Os valores ORM medidos, o comparativo 1/3 furos e os logs mencionados não foram fornecidos: medição empírica [não verificado].

A redução 2→1 das falhas é estaticamente compatível com o gate: recusa impede `_gravar_estado` (`chamados/models.py:652`) e `_retratar` retorna antes da auto-rejeição da justificativa, resolver e nota (`chamados/reconciliador.py:524`). Contudo, `r['falhas']` **conta lotes de colaborador com alguma falha**, não exceções nem chamadas a `fechar` (`chamados/reconciliador.py:1352`, `:1393`, `:1403`). Duas batidas presentes no mesmo lote não explicam duas unidades de falha; seria necessário outro lote/evento, cuja origem exata neste ensaio é [não verificado]. O comentário “MEDIDO” do teste não substitui essa prova (`chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:155`).

**I.** Classe comprovada nesta fatia: **selo sem mordida completa**, pelo mutante de sétimo fechador com alias por atribuição; recomendação de bloqueante fundamentada no reconhecimento incompleto de `chamados/tests/test_o168_fechadores_fora_do_gate.py:87` e na autorização complementar de `chamados/tests/e4b_c12_ast.py:246`.

## 13/11 01:30 · sessao 7e4311f26616 · - · - · origem=Ronald (tmux)

A/B/C — resultados de leitura estática:

- O pai passa pelo novo gate antes de `_gate_pergunta_viva` e `_gravar_estado`; a disputa conserva seu fechamento independente. Assim o pai comum de categoria `disputa_supervisao` com conversa fica vivo, enquanto `DisputaSupervisao.fechar` grava `fechada_em`, autor e resolução normalmente (`app/chamados/models.py:686`, `app/chamados/models.py:713`, `app/chamados/models.py:1962`). Sem outra disputa aberta, deixa de incidir a trava específica da RegularizacaoExterna; sucesso integral da regularização continua condicionado às demais guardas (`app/chamados/services/regularizacao_ext.py:59`).

- **Falta a nota pedida no GREEN literal do aval.** A recusa grava somente `LogAuditoria`; S88 retorna antes de anexar seu Followup. A timeline do supervisor lê Followups, portanto não apresenta o motivo novo da retenção (`app/chamados/models.py:719`, `app/chamados/models.py:403`, `app/chamados/signals.py:43`, `app/chamados/signals.py:46`, `app/chamados/views.py:372`, `app/templates/chamados/detalhe_local.html:112`). O aval exige “pai aberto com nota”; referência de linha do anexo [não verificado].

- Na recusa nova, `_f1_dedup` já foi retirado da instância e a variável local é descartada; `_f1_ancoradas` vira `None`. O gate da pergunta viva ainda não rodou: não reata perguntas e não grava trilha de dedup. `reconciliar` retorna antes de alterar status, resolução e urgência (`app/chamados/models.py:685`, `app/chamados/models.py:687`, `app/chamados/models.py:689`, `app/chamados/models.py:694`, `app/chamados/models.py:652`).

- Para duas recusas consecutivas com mesmo payload e sem mudança de estado, `_registrar_recusa` produz uma linha; dry-run não registra. Isso está afirmado pelo teste, mas execução [não verificado] (`app/chamados/models.py:403`, `app/chamados/models.py:405`, `app/chamados/models.py:718`, `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:119`).

- Existe escrita anterior condicional: se esse pai também satisfizer `e_chamado_de_furo` e não tiver dia forte, `_gate_premissa_do_fechamento` escreve `fechamento_sem_dia_do_fato` antes de O168 recusar, e esse evento não deduplica. Não demonstrado nos três tipos normais de pais; ocorrência em produção [não verificado] (`app/chamados/models.py:757`, `app/chamados/models.py:762`, `app/chamados/models.py:764`, `app/chamados/models.py:1274`, `app/chamados/services/disputa_emissao.py:568`, `app/chamados/services/disputa_emissao.py:1104`, `app/chamados/services/disputa_emissao.py:1339`).

- **Validação humana:** carimba validação, materializa a pergunta e chama `materializar_perguntas_validadas_da_disputa`; sua cauda fecha a disputa por C7, acionando S88 e O168. Perguntas e seus carimbos persistem; pai fica vivo; O168 não desfaz materialização nem validação. Outro chamador da cauda é o lote de passivo validado (`app/chamados/services/validacao.py:435`, `app/chamados/services/validacao.py:463`, `app/chamados/services/validacao.py:469`, `app/chamados/services/materializacao.py:1384`, `app/chamados/services/materializacao.py:1322`, `app/chamados/services/materializacao.py:1339`, `app/chamados/services/passivo_validado.py:405`).

- A ação de validação informa “Pergunta validada e ponto gravado no espelho”; regularização informa “Disputa … regularizada totalmente”. Nenhuma traduz o pai retido pelo novo gate. No modal, o supervisor pode ver simultaneamente o status vivo do chamado e “Disputa fechada”; no detalhe, o painel de disputa só carrega disputa aberta (`app/chamados/services/acoes_disputa.py:57`, `app/chamados/services/acoes_disputa.py:162`, `app/templates/chamados/partials/modal_fio.html:102`, `app/templates/chamados/partials/modal_fio.html:379`, `app/chamados/services/acoes_chamado.py:342`).

- **Retratação por premissa morta:** `_retratar` retorna imediatamente se O168 recusar. Permanecem como estavam disputa, perguntas e justificativa automática; justificativa pendente não sofre a auto-rejeição posterior; não nasce nota de retratação (`app/chamados/reconciliador.py:524`, `app/chamados/reconciliador.py:530`, `app/chamados/reconciliador.py:537`, `app/chamados/reconciliador.py:539`). O contador de retratados só aumenta com retorno verdadeiro (`app/chamados/reconciliador.py:1465`).

- **Porta humana real:** detalhe → resposta livre → `transicionar_com_pauta` sem `ato` → `transicionar`, que não lê O168. O dropdown oferece resolvido/fechado. Continuam valendo justificativa pendente, disputa viva, pergunta viva e S122; logo existe porta, mas não promessa de fechamento para qualquer estado (`app/chamados/views.py:546`, `app/chamados/services/acoes_chamado.py:319`, `app/chamados/services/acoes_chamado.py:322`, `app/chamados/services/fecho_runtime.py:181`, `app/chamados/models.py:135`, `app/chamados/models.py:148`, `app/chamados/models.py:181`, `app/templates/chamados/detalhe_local.html:390`).

## 13/11 01:29 · sessao 2e4159f3821a · O168 · sonda · origem=Code

Não executei o envelope: ele cria/reescreve arquivos e acrescenta log, contrariando “só leitura” (bin/sonda_leitura.sh:126–134, :216–218, :248–250).

Transcrição integral do dado fornecido; nova execução [nao verificado]:

```text
sonda_leitura: sonda_cascata.py [468fad0c8bbe7ab8] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

pais_com_nota_defesa1=5027
conversa__defesa1                                2246  amostra=[21443, 2194, 1228, 3992, 1612, 23712, 28085, 16587, 18106, 1305]
conversa__defesa1__ult30d                         150  amostra=[33987, 33994, 34858, 30592, 34861, 1956, 36926, 38028, 28656, 36963]
conversa__defesa1__ult7d                           23  amostra=[38028, 35846, 38503, 37220, 37761, 35138, 37259, 27229, 37424, 37432]
conversa__outra_via                               711  amostra=[9359, 134, 1520, 21447, 1229, 19677, 23623, 18706, 16787, 15221]
conversa__outra_via__ult30d                        54  amostra=[19779, 18213, 29228, 29508, 22837, 8419, 7093, 10504, 18852, 33038]
conversa__outra_via__ult7d                          3  amostra=[22837, 35769, 20312]
sem_conversa__defesa1                            2772  amostra=[16712, 22483, 21446, 24543, 36897, 32600, 33980, 35970, 21457, 25806]
sem_conversa__defesa1__ult30d                     893  amostra=[36897, 32600, 33980, 35970, 33988, 36707, 32601, 32942, 32356, 32602]
sem_conversa__defesa1__ult7d                      144  amostra=[38669, 38102, 38107, 38095, 37644, 37645, 37898, 37650, 38342, 37655]
sem_conversa__outra_via                          1003  amostra=[1230, 18087, 21450, 1231, 19672, 9398, 24546, 21453, 371, 19684]
sem_conversa__outra_via__ult30d                   153  amostra=[29784, 34905, 34050, 33358, 33386, 34668, 34652, 34535, 34031, 34639]
sem_conversa__outra_via__ult7d                     15  amostra=[37207, 37272, 37683, 37420, 37802, 37476, 37481, 37479, 37862, 37870]
total_encerrados                                 6732  amostra=[]
total_erros=0
```

Conferência aritmética do dado: `2246 + 711 + 2772 + 1003 = 6732`; `total_erros=0` informado. Origem em produção [nao verificado].

## 13/11 01:27 · sessao 8f2519eeb11c · O168 · sonda · origem=Code

Não executei o envelope: ele escreve arquivos, contrariando a regra de somente leitura (`bin/sonda_leitura.sh:132`, `bin/sonda_leitura.sh:134`, `bin/sonda_leitura.sh:217`, `bin/sonda_leitura.sh:250`).

No dado fornecido, as classes somam **7058**, igual ao total, e consta **total_erros=0**; execução independente [nao verificado].

Reprodução integral da saída fornecida [nao verificado]:

```text
sonda_leitura: sonda_pais_conversa.py [8e530f330dd1672f] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

aberto__conversa_TODA_validada                               50  amostra=[20239, 19790, 18257, 21649, 20019, 3893, 28437, 4757, 17616, 15394]
aberto__conversa_parcial_validada                            87  amostra=[18157, 18180, 19823, 28962, 21643, 20045, 18566, 30524, 20332, 21946]
aberto__conversa_sem_validacao                               57  amostra=[38217, 25887, 32489, 31901, 20591, 23043, 23034, 38701, 37022, 36871]
aberto__sem_conversa                                         97  amostra=[19962, 17167, 28210, 25895, 22832, 22835, 34689, 18059, 33347, 20584]
cancelado__conversa_TODA_validada                             1  amostra=[2332]
cancelado__sem_conversa                                       1  amostra=[1725]
em_analise__conversa_TODA_validada                            3  amostra=[15622, 22166, 2074]
em_analise__conversa_parcial_validada                        28  amostra=[16640, 17917, 6295, 24943, 10388, 15712, 33492, 17211, 9785, 10155]
em_analise__conversa_sem_validacao                            2  amostra=[23120, 32735]
fechado__conversa_TODA_validada                             562  amostra=[1520, 1229, 3992, 16767, 16782, 14387, 11861, 15967, 15448, 13642]
fechado__conversa_parcial_validada                           55  amostra=[18706, 14017, 13663, 16960, 16989, 19880, 2988, 17165, 18580, 20245]
fechado__conversa_sem_validacao                              30  amostra=[23623, 18713, 17160, 27368, 17839, 29508, 17614, 23060, 23033, 18088]
fechado__sem_conversa                                       447  amostra=[1231, 24546, 21457, 1329, 21470, 1389, 21500, 1234, 15507, 18130]
resolvido__conversa_TODA_validada                          2131  amostra=[21443, 134, 2194, 1228, 19677, 1612, 23712, 28085, 16587, 18106]
resolvido__conversa_parcial_validada                        119  amostra=[9359, 16787, 15221, 14292, 18150, 1577, 19808, 16970, 8353, 19860]
resolvido__conversa_sem_validacao                            60  amostra=[21447, 25811, 20799, 18210, 25862, 19974, 34861, 18484, 17192, 18568]
resolvido__sem_conversa                                    3328  amostra=[16712, 22483, 21446, 24543, 36897, 1230, 18087, 32600, 21450, 33980]
total                                                      7058  amostra=[]
total_erros=0
```

## 13/11 00:49 · sessao eb2adfbaf90a · E3-FORMA-SQL · leitura · origem=Code

```python
# SOMENTE TEXTO; não executado, compilado ou validado em execução. [nao verificado]
# Nenhum arquivo foi criado, editado ou apagado nesta revisão. [nao verificado]
#
# Executar o MESMO texto na BASE e na COPIA, mudando somente PARTE.
# Compatibilidade com a BASE e sombra imutável: [nao verificado].
#
# Particionamento: decisão expressa no enunciado.
# Pergunta tem colaborador próprio: app/chamados/models.py:2454.
# Disputa recebe todas as perguntas: app/chamados/models.py:1859.
#
# Hash novo: Merkle + multiconjunto combinável em faixas fixas de IDs.
# Não é o SHA256 das listas da v1; comparar BASE/COPIA com este mesmo texto.
# Implementação proposta e propriedades em execução: [nao verificado].
#
# O acumulador aceita IDs fora de ordem e repetidos; necessário para:
# app/chamados/relatorios_disputa.py:60;
# app/chamados/services/fio.py:206.
#
# vigia.ramo preserva a guarda da v1: _listar retorna None e não é registrado.
# e3_diff_leitores_v1.py:369; app/chamados/services/vigia_fio.py:629.
#
# PRONTO 7: descrição literal do aval não fornecida. [nao verificado]
# São emitidas categorias reais, respostas sem veredito, booleanos C7,
# classes da tarefa e as aberturas nativas do vigia:
# app/chamados/relatorios_disputa.py:6;
# app/chamados/models.py:1847;
# app/chamados/models.py:1854;
# app/chamados/services/vigia_fio.py:575.
#
# Paridade externa:
# agregado: paridade_face_sql_sem_texto::face
# partes:   paridade_face_sql_sem_texto::juiz
# Confrontar os acumuladores dos mesmos blocos após somar as partes.
# Não se emite um booleano de paridade local sem a face global.
# Decisão expressa no enunciado; juiz/face:
# app/chamados/juizes.py:1515; app/chamados/juizes.py:1531.

PARTE = 0                  # int entre 0 e N_PARTES-1, ou "agregado"
N_PARTES = 12
BLOCO = 1000
TETO_MB = 2048
TETO_DURO_MB = 2560
TRACO = False

import sys
sys.dont_write_bytecode = True

import collections
import collections.abc
import contextlib
import copy
import dataclasses
import datetime as dt
import decimal
import enum
import hashlib
import importlib
import inspect
import io
import json
import logging
import os
import re
import signal
import threading
import time
import uuid

from unittest.mock import patch
from zoneinfo import ZoneInfo

FIXO = dt.datetime.fromisoformat("2026-11-12T23:48:00+00:00")
ZONA = ZoneInfo("America/Sao_Paulo")
HOJE = FIXO.astimezone(ZONA).date()
DIA = dt.timedelta(days=1)
DATA_REAL = dt.date
INSTANTE_REAL = dt.datetime

Model = QuerySet = None
transaction = J = None

ASSINATURAS = {}
AUSENTES = set()
REGISTROS = {}
MANIFESTOS = {}
TEMPOS = {}
UNIVERSO = {}
INCANONIZAVEL = collections.Counter()
SQL_ERROS = collections.Counter()
OBS_ERROS = collections.Counter()
SEQUENCIAS = collections.Counter()
ATUAL = None
ALCANCADO_FIM = False
ROLLBACKS = 0

MODULO_HASH = 1 << 256
MASCARA_HASH = MODULO_HASH - 1

MEMORIA = {
    "pico_mb": 0.0,
    "teto_mb": TETO_MB,
    "teto_duro_mb": TETO_DURO_MB,
}
MEMORIA_LOCK = threading.Lock()
PARAR_VIGIA = threading.Event()

TOKENS = set("""
porta_fila porta_pergunta sem_porta de_hoje_pra_frente
admin colab colaborador sistema cadastro estrutura ninguem nada chamado nao_sei
a b c d e
AGUARDA_COLAB AGUARDA_SUPERVISAO SEM_PERGUNTA PRONTA_FECHAR
decisao_do_dia_pendente aguarda_decisao_de_ausencia
escala_nao_descreve_o_dia fio_do_admin
fora_vinculo isento_ponto competencia_trancada sem_celula nao_venceu
ata_sem_lampadas passivo_sem_linha respondida_sem_texto
aguarda_resposta aguarda_validacao sem_resposta resolvida pendencia_admin
validada_sem_gravar devolvida_ao_colaborador materializacao_recusada
veredito_sem_lavra_vigente validada_sem_efeito efeito_sem_lampada
encerrada_pelo_juiz silencio_decidido absorvida_aprovada
absorcao_rejeitada absorcao_aguarda_documento absorcao_pendente
perguntar responder validar materializar rejulgar decidir_ausencia
entregar_documento emissor cartorio materializacao
furo cobrado discordante indefinida concordante
entra bloqueado modulo_proprio sim nao
regularizar_batida admin-cadastro admin-batida
sem_pergunta_viva pergunta_viva_sob_chamado_terminal
pergunta_viva_sem_ancora pergunta_viva_fora_do_app outra
dia_na_lista_dono_batida dia_na_lista_dono_cadastro
dia_na_lista_dono_estrutura dia_na_lista_dono_nenhum
reabrir_credor registrar_decisao
""".split())
TOKENS.update({
    "", "nenhuma porta", "fora da lista do admin", "lista sem resposta",
})

CHAVES = set("""
pk id colab colaborador_id empresa empresa_id pergunta_id disputa_id
celula_id chamado_id chamado_destino_id data dia dia_do_turno
agora competencia mes ano ini fim
rota fila verbos fio nao_sei razao instancias instancia tipo hora
data_civil lampada vencido motivo acoes acao de porta
pode_fechar todas_respondidas pode_solicitar_correcao resposta_enviada
tem_pergunta_viva perguntas perguntas_disputa itens estado dono resumo
status grupo grupos motivos motivos_retencao dias datas por_fio
total entram fora_por_motivo furo_espelho dias_por_fio
colaboradores esperado universo por_veredito amostra maiores
com_pergunta_aberta com_chamado_vivo com_porta_fila com_porta_pergunta
alcance_universo alcance_competencias alcance_porta_fila
alcance_porta_pergunta alcance_de_hoje_pra_frente fio_morto_alcance
alcance_porta_exige_o_admin alcance_e_detalhe alcance_por_rota_papel
alcance_dias alcance_dias_omitidos alcance_cego
alcance_a alcance_b alcance_c alcance_d alcance_e
alcance_decisao_do_dia_pendente alcance_aguarda_decisao_de_ausencia
alcance_escala_nao_descreve_o_dia alcance_fio_do_admin
causa detalhe papel porta_real perguntas_reais
retido sem_canal candidato vencido_cobranca sem_canal_push
creditada_respondida creditada_pendente pertence
q_vivas viva juiz respondida_sem_texto trancada fora_do_fato
paridade_q_sem_texto
colabs furo dias_ferias_com_batida
dono_do_dia ec_do_dia posto_do_dia pai_limbo
saldo codigo_tipo chamados disputas fica_resposta_sem_veredito
fica_pergunta_viva_fora fica_resgatado perguntas_com_resposta_sem_veredito
disputas_com_pai_vivo_fora sem_dia fica_fora_da_tranca decidir_pelo_dp
vivas_apesar_da_tranca perguntas_lidas_trancadas aplicadas [nao verificado]
ok pendentes prazo_ate canal erros
alvo abrir fechar mantidas alvo_dias estrutura por_dono
abrir_chaves fechar_chaves n chave
q_respondida_sem_texto q_respondida_com_texto
resultado ordem faltam saida_sha256 bytes
categoria pendentes_com_tranca pendentes_ignorando_tranca
""".split())

PRIVADOS = {
    "nome", "nome_completo", "cpf", "username", "email",
    "codigo", "codigo_dominio", "telefone", "matricula",
}

CAMPOS_CLASSE = (
    "rota", "fio", "razao", "estado", "status", "grupo", "motivo",
    "causa", "detalhe", "papel", "dono", "acao", "de", "categoria",
    "pode_fechar", "todas_respondidas", "pode_solicitar_correcao",
    "resposta_enviada", "tem_pergunta_viva", "nao_sei",
    "sem_canal", "vencido_cobranca", "sem_canal_push",
    "candidato", "creditada_respondida", "creditada_pendente",
    "pertence",
)


class DesfazerSonda(Exception):
    pass


class FalhaDaSonda(BaseException):
    pass


class EscritaBloqueada(FalhaDaSonda):
    pass


class TetoDeMemoria(FalhaDaSonda):
    pass


class FormatoInesperado(Exception):
    pass


class Sink:
    def write(self, texto):
        return len(texto)

    def flush(self):
        pass

    def isatty(self):
        return False


class MetaRelogio(type):
    def __instancecheck__(cls, objeto):
        return isinstance(objeto, cls._real)


class DataFixa(DATA_REAL, metaclass=MetaRelogio):
    _real = DATA_REAL

    @classmethod
    def today(cls):
        return HOJE


class InstanteFixo(INSTANTE_REAL, metaclass=MetaRelogio):
    _real = INSTANTE_REAL

    @classmethod
    def now(cls, tz=None):
        if tz is None:
            return FIXO.astimezone(ZONA).replace(tzinfo=None)
        return FIXO.astimezone(tz)

    @classmethod
    def today(cls):
        return cls.now()

    @classmethod
    def utcnow(cls):
        return FIXO.astimezone(dt.timezone.utc).replace(tzinfo=None)


def serial(valor):
    return json.dumps(
        valor, ensure_ascii=True, sort_keys=True,
        separators=(",", ":"), allow_nan=False,
    )


def sha(valor):
    texto = valor if isinstance(valor, str) else serial(valor)
    return hashlib.sha256(texto.encode("utf-8")).hexdigest()


def rss_mb():
    with open("/proc/self/statm", "r") as arquivo:
        paginas = int(arquivo.read().split()[1])
    return paginas * os.sysconf("SC_PAGE_SIZE") / 1048576.0


def atualizar_pico(atual):
    with MEMORIA_LOCK:
        MEMORIA["pico_mb"] = max(MEMORIA["pico_mb"], atual)


def memoria_saida():
    with MEMORIA_LOCK:
        return {
            **MEMORIA,
            "pico_mb": round(MEMORIA["pico_mb"], 1),
        }


def medir_memoria():
    atual = rss_mb()
    atualizar_pico(atual)
    if atual > TETO_MB:
        raise TetoDeMemoria()
    return atual


def traco(nome, unidade, etapa):
    if TRACO:
        os.write(2, (
            "TRACO %s %s rss=%.1f etapa=%s\n"
            % (nome, serial(unidade), rss_mb(), etapa)
        ).encode("ascii"))


def iniciar_vigia_duro():
    def laco():
        while not PARAR_VIGIA.is_set():
            try:
                atual = rss_mb()
                atualizar_pico(atual)
                if atual > TETO_DURO_MB:
                    os.write(1, (
                        "===TETO_DURO_MB_EXCEDIDO===\n"
                        + serial({
                            "fatal": "TetoDuroDeMemoria",
                            "execucao_completa": False,
                            "rss_mb": round(atual, 1),
                            **memoria_saida(),
                        })
                        + "\n"
                    ).encode("ascii"))
                    os.kill(os.getpid(), signal.SIGKILL)
            except Exception:
                os.write(1, b"===FALHA_VIGIA_DURO===\n")
                os.kill(os.getpid(), signal.SIGKILL)
            PARAR_VIGIA.wait(0.25)

    threading.Thread(
        target=laco, name="e3-teto-rss", daemon=True,
    ).start()


def sem_forma(valor, prefixo="sem_forma"):
    tipo = type(valor)
    chave = tipo.__module__ + "." + tipo.__qualname__
    INCANONIZAVEL[prefixo + ":" + chave] += 1
    return ("incanonizavel", tipo.__module__, tipo.__qualname__)


def canonico(valor, ativos=None, memo=None):
    # Memo com referência forte ao original: cura do DAG do anexo.
    # Forma inteira usa tuplas; digest_forma nunca serializa o DAG expandido.
    # Implementação proposta: [nao verificado].
    if ativos is None:
        ativos = set()
    if memo is None:
        memo = {}

    if valor is None or isinstance(valor, (bool, int)):
        return valor
    if isinstance(valor, str):
        if valor in TOKENS or re.fullmatch(r"\d{4}-\d{2}-\d{2}", valor):
            return valor
        return ("str_sha256", len(valor), sha(valor))
    if isinstance(valor, float):
        return ("float", valor.hex())

    identidade = id(valor)
    if identidade in ativos:
        return sem_forma(valor, "ciclo")
    if identidade in memo:
        return memo[identidade][1]

    ativos.add(identidade)
    try:
        try:
            if isinstance(valor, enum.Enum):
                forma = canonico(valor.value, ativos, memo)
            elif isinstance(valor, decimal.Decimal):
                forma = ("decimal", str(valor))
            elif isinstance(valor, (INSTANTE_REAL, DATA_REAL, dt.time)):
                forma = ("iso", valor.isoformat())
            elif isinstance(valor, dt.timedelta):
                forma = (
                    "duracao", valor.days, valor.seconds, valor.microseconds,
                )
            elif isinstance(valor, uuid.UUID):
                forma = ("uuid", str(valor))
            elif isinstance(valor, bytes):
                forma = (
                    "bytes_sha256", len(valor),
                    hashlib.sha256(valor).hexdigest(),
                )
            elif Model is not None and isinstance(valor, Model):
                # Mesma projeção de modelo da E3 v1: somente identidade.
                # e3_diff_leitores_v1.py:265.
                forma = ("model", valor._meta.label_lower, valor.pk)
            elif QuerySet is not None and isinstance(valor, QuerySet):
                itens = [canonico(x, ativos, memo) for x in valor]
                ordem = {}
                forma = (
                    "queryset",
                    tuple(sorted(
                        itens, key=lambda x: digest_forma(x, ordem),
                    )),
                )
            elif isinstance(valor, collections.abc.Mapping):
                itens = []
                for chave, conteudo in valor.items():
                    if isinstance(chave, str) and chave.lower() in PRIVADOS:
                        continue
                    k = (
                        chave
                        if isinstance(chave, str) and chave in CHAVES
                        else canonico(chave, ativos, memo)
                    )
                    itens.append((k, canonico(conteudo, ativos, memo)))
                ordem = {}
                forma = (
                    "dict",
                    tuple(sorted(
                        itens,
                        key=lambda x: digest_forma(x[0], ordem),
                    )),
                )
            elif isinstance(valor, (set, frozenset)):
                itens = [canonico(x, ativos, memo) for x in valor]
                ordem = {}
                forma = (
                    "set",
                    tuple(sorted(
                        itens, key=lambda x: digest_forma(x, ordem),
                    )),
                )
            elif isinstance(valor, (list, tuple)):
                forma = (
                    "list" if isinstance(valor, list) else "tuple",
                    tuple(canonico(x, ativos, memo) for x in valor),
                )
            elif dataclasses.is_dataclass(valor) and not isinstance(valor, type):
                forma = canonico({
                    f.name: getattr(valor, f.name)
                    for f in dataclasses.fields(valor)
                    if f.name.lower() not in PRIVADOS
                }, ativos, memo)
            else:
                forma = sem_forma(valor)
        except Exception:
            forma = sem_forma(valor, "erro_canonizador")
    finally:
        ativos.remove(identidade)

    memo[identidade] = (valor, forma)
    return forma


def digest_forma(forma, memo=None):
    if memo is None:
        memo = {}
    if isinstance(forma, tuple):
        chave = id(forma)
        if chave in memo:
            return memo[chave][1]
        h = hashlib.sha256(b"T%d(" % len(forma))
        for item in forma:
            h.update(digest_forma(item, memo).encode("ascii"))
            h.update(b",")
        h.update(b")")
        digest = h.hexdigest()
        memo[chave] = (forma, digest)
        return digest
    return hashlib.sha256(
        ("P" + repr(forma)).encode("utf-8")
    ).hexdigest()


def bytes_forma(forma, memo=None):
    # Comprimento JSON expandido calculado sem construir esse JSON.
    # Implementação proposta: [nao verificado].
    if memo is None:
        memo = {}
    if not isinstance(forma, tuple):
        return len(serial(forma).encode("utf-8"))
    chave = id(forma)
    if chave in memo:
        return memo[chave][1]
    tamanho = (
        2 + max(0, len(forma) - 1)
        + sum(bytes_forma(x, memo) for x in forma)
    )
    memo[chave] = (forma, tamanho)
    return tamanho


def primeiro_id(unidade):
    if type(unidade) is int:
        return unidade
    if isinstance(unidade, (list, tuple)):
        for item in unidade:
            ident = primeiro_id(item)
            if ident is not None:
                return ident
    return None


def rotulo_classe(valor):
    if valor is None:
        return "None"
    if type(valor) is bool:
        return "True" if valor else "False"
    if isinstance(valor, str):
        return valor if valor in TOKENS else "FORA_DO_VOCABULARIO"
    if isinstance(valor, enum.Enum):
        return rotulo_classe(valor.value)
    return "TIPO:" + type(valor).__name__


class Registro:
    # Não conserva eventos/linhas: somente acumuladores por faixa de IDs,
    # bitmap de IDs da faixa e contadores de classes.
    # Algoritmo proposto; validação em execução: [nao verificado].
    def __init__(self):
        self.chamadas = 0
        self.projecoes = 0
        self.excecoes = collections.Counter()
        self.hist = collections.Counter()
        self.classes = collections.Counter()
        self.metricas = collections.Counter()
        self.blocos = {}

    def contar_classes(self, valor):
        self.hist[rotulo_classe(valor)] += 1
        if not isinstance(valor, collections.abc.Mapping):
            return

        def campos(d, prefixo):
            for campo in CAMPOS_CLASSE:
                if campo in d:
                    v = d[campo]
                    if v is None or isinstance(v, (bool, str, enum.Enum)):
                        self.classes[
                            prefixo + campo + ":" + rotulo_classe(v)
                        ] += 1

        campos(valor, "")
        for instancia in valor.get("instancias") or ():
            if not isinstance(instancia, collections.abc.Mapping):
                continue
            campos(instancia, "instancia.")
            for acao in instancia.get("acoes") or ():
                if isinstance(acao, collections.abc.Mapping):
                    campos(acao, "acao.")

    def adicionar(self, ident, contexto, valor, exc=None, projecao=False):
        if projecao:
            self.projecoes += 1
        else:
            self.chamadas += 1
        if exc:
            self.excecoes[exc] += 1
        self.contar_classes(valor)

        evento = (
            canonico(contexto),
            ("forma_sha256", digest_forma(canonico(valor))),
        )
        digest = digest_forma(evento)
        numero = int(digest, 16)

        if ident is not None and (type(ident) is not int or ident < 0):
            raise FalhaDaSonda()
        faixa = None if ident is None else ident // BLOCO
        bloco = self.blocos.setdefault(faixa, {
            "n": 0, "soma": 0, "quadrados": 0, "xor": 0, "bitmap": 0,
        })
        bloco["n"] += 1
        bloco["soma"] = (bloco["soma"] + numero) & MASCARA_HASH
        bloco["quadrados"] = (
            bloco["quadrados"] + numero * numero
        ) & MASCARA_HASH
        bloco["xor"] ^= numero
        if ident is not None:
            bloco["bitmap"] |= 1 << (ident % BLOCO)

    def resultado(self):
        blocos = []
        for faixa in sorted(
            self.blocos, key=lambda x: (-1 if x is None else x),
        ):
            b = self.blocos[faixa]
            atomos = {
                "n": b["n"],
                "soma256": "%064x" % b["soma"],
                "quadrados256": "%064x" % b["quadrados"],
                "xor256": "%064x" % b["xor"],
            }
            blocos.append({
                "faixa": faixa,
                "id_de": None if faixa is None else faixa * BLOCO,
                "id_ate": (
                    None if faixa is None else (faixa + 1) * BLOCO - 1
                ),
                "ids_distintos": b["bitmap"].bit_count(),
                "ids_bitmap_hex": format(b["bitmap"], "x"),
                **atomos,
                "sha256": sha(atomos),
            })
        return {
            "universo_registros": self.chamadas + self.projecoes,
            "chamadas_ou_observacoes": self.chamadas,
            "projecoes_de_resultado": self.projecoes,
            "excecoes_por_classe": dict(sorted(self.excecoes.items())),
            "histograma": dict(sorted(self.hist.items())),
            "classes": dict(sorted(self.classes.items())),
            "metricas": dict(sorted(self.metricas.items())),
            "blocos": blocos,
        }


def registro(nome):
    return REGISTROS.setdefault(nome, Registro())


def guardar(
    

[truncado em 20000 de 82527 caracteres]

## 13/11 00:47 · sessao fd03853988f3 · BUG-CADASTRO-C5E · censo · origem=Code

As referências aos anexos contam desde a primeira linha de cada bloco, incluindo linhas em branco.

| Achado | Letra | Trecho literal .q | Trecho literal .r | Trecho triagem e motivo |
|---|---|---|---|---|
| **4/6 — duas leituras no signal; ausência no universo** | **b** | “Ache um ato que leia duas vezes ou deixe um destino ler o relogio por conta propria.” (`pv1.q:10–11`) | “Na edição, `existente.save()` pode alcançar os signals: o de vigência lê **duas vezes**; o de âncora lê uma vez.” (`pv1.r:55`) | “FORA (aval: PROIBIDO ampliar aos ~30 interiores): […] escala/signals.py:67/119” (`triagem_v1.txt:11`). **Descarte por escopo do aval.** A dupla leitura foi vista; a omissão específica de `UNIVERSO_RELOGIO` não foi denunciada nesse trecho. (`pv1.r:55`; `app/ponto/tests/test_cadastro_futuro_c5e.py:326–332`) |
| **1 — relógios adicionais no Salvar e destinos** | **b** | “se o MESMO valor desce a todas as portas e a todo efeito do ato” (`pv1.q:9–10`) | “Ele relê `localdate()` e decide os dias julgáveis com esse novo valor”; “`_copiar_folga_padrao` também lê `date.today()` no meio do Salvar.” (`pv1.r:53`; `pv1.r:56`) | “FORA […] rabo _o_chao_do_dia_mudou:464, pos_vinculo:56, escala/signals.py:67/119, _copiar_folga_padrao:163 […] vinculo.py:748 -> RELOGIO-NA-BORDA.” (`triagem_v1.txt:11–12`). **Descarte por proibição de ampliar a cura.** |
| **6 — cron ainda regenera HOJE** | **b** | “Em cada ato que chega a essas portas (view de Salvar, commands […]), onde o relogio e lido” e “Quem LE e quem ESCREVE cada valor que o diff passa a mover” (`pv1.q:8–9`; `pv1.q:14`) | “Uma leitura `date.today()`; mesmo valor para vigia, regeneração de hoje, geração e lavratura.” (`pv1.r:44`) | “DESCARTADO 3 (c5): continua descartado (Codex item 7 concorda).” (`triagem_c5d.txt:6`). O descarte remete à defesa de conservar o alcance de hoje pela juíza, registrada no item 7 de `preverif_c5d.r`; sua pergunta original não foi fornecida **[nao verificado]**. |
| **7 — DNA órfão após exclusão no snapshot** | **e** | O recorte explícito é “o `hoje` e o que ele decide” (`pv1.q:14`). A volta 3 acrescenta “O que mais esta errado na obra inteira (8dc931da9..4b34ed21a)” (`pv3.q:9`), sem nomear exclusão, `SET_NULL` ou integridade do DNA. | “o snapshot mantém vínculos antes de células, com selo exigindo `E → C`” (`pv3.r:67`). Esse trecho examina travas; o problema de DNA após exclusão não é tratado. | Descarte específico desse problema: **[nao verificado]**. Classifico **não perguntado pelo escopo demonstrável**: snapshot aparece como relógio/travas, sem pergunta específica sobre os resíduos do `delete()`. (`pv1.q:14–16`; `pv3.r:67–68`; `app/ponto/tests/test_cadastro_futuro_c5e.py:344`) |
| **8 — comprovação do D7 e reversão em logs** | **c** | “inclusivo/exclusivo em `d > hoje` x `d >= hoje`” (`pv1.q:16`) | “Já a célula ausente de vínculo inativo só nasce em `d > hoje`.” (`pv1.r:95`); “não houve acesso a banco/log nem execução da suíte com banco” (`pv3.r:73`) | Medição anterior/posterior e prova de reversão nas triagens fornecidas: **[nao verificado]**. A comprovação solicitada depende dos dados/logs ausentes. (`pv3.r:73`) | [nao verificado]

A conferência da cópia confirma os sítios citados:

- **4/6 e 1:** leituras em `app/escala/signals.py:67–68` e `:119`; `app/colaboradores/services/vinculo.py:163`, `:194`, `:740` e `:752`; `app/escala/folgas.py:280`; `app/ponto/portas/celula.py:464`. O universo da catraca omite `escala/signals.py`. (`app/ponto/tests/test_cadastro_futuro_c5e.py:326–332`)
- **6:** `_regenerar_hoje_existentes` chama a porta para hoje e é executado pelo command. (`app/ponto/management/commands/gerar_celulas.py:303`, `:323–325`, `:347`)
- **7:** o snapshot executa `remover.delete()` e depois percorre os vínculos do snapshot; a geradora usa `SET_NULL`, e o DNA é JSON separado. (`app/colaboradores/services/vinculo.py:1029`, `:1034–1035`; `app/escala/models.py:1693`, `:1697`). O vigia incrementa o contador de divergência. (`app/ponto/services/vigia_cadastro_futuro.py:60–61`)
- **8:** a seleção D7 existe, com guarda que exclui o inativo em `d <= hoje`. Isso sustenta a leitura estática; resultado em produção e reversão permanecem **[nao verificado]**. (`app/ponto/portas/celula.py:883`, `:908–910`)

Por volta, **pv1** pediu atos, chamadores e leitores/escritores; **pv2** concentrou-se no delta e proibiu ampliar aos interiores; **pv3** declarou delta apenas de teste, acrescentando revisão da obra inteira com o mesmo veto. (`pv1.q:8–16`; `pv2.q:4–8`; `pv3.q:2`, `:9`). A existência exata nos hashes anteriores e o conteúdo dos avais/diffs apenas mencionados nos anexos permanecem **[nao verificado]**. [nao verificado]

**Contagem: a=0 | b=3 | c=1 | d=0 | e=1.**

## 13/11 00:45 · sessao 4ae7d397b4aa · - · - · origem=Ronald (tmux)

Minha classificação independente: **7 → (e); 8 → (c)**, com a ressalva de escopo abaixo.

| Achado | Sustentação literal dos anexos | Conferência na cópia |
|---|---|---|
| **7 — DNA órfão após snapshot → (e)** | Nenhuma pergunta fornecida nomeia exclusão de EC, `SET_NULL` ou DNA órfão. O pedido mais próximo é `pv1.q`, item 4: “cada valor que o diff passa a mover (o `hoje` e o que ele decide: celula futura x passada [...]”. A resposta trata outra propriedade do snapshot: `pv3.r`: “o snapshot mantém vínculos antes de células, com selo exigindo `E → C`”. Nenhuma triagem fornecida descarta especificamente o problema de DNA órfão. A inferência de **(e)** distingue escopo temporal do escopo de integridade do DNA; os diffs e avais originais anexados às perguntas não estão disponíveis no material fornecido, portanto alcance integral histórico **[nao verificado]**. | O snapshot seleciona ECs a remover, recusa referências humanas e chama `remover.delete()` em `app/colaboradores/services/vinculo.py:1022`, `:1027`, `:1029`; depois só percorre ECs remanescentes do snapshot em `:1034`. A FK usa `SET_NULL`, enquanto DNA é JSON independente: `app/escala/models.py:1692`, `:1693`, `:1697`. O vigia conta divergência de ID em `app/ponto/services/vigia_cadastro_futuro.py:60`. A função integra o **censo de relógio**, especificamente, em `app/ponto/tests/test_cadastro_futuro_c5e.py:344`. |
| **8 — prova de não alteração/reversão D7 → (c)** | `pv1.q`, item 4: “inclusivo/exclusivo em `d > hoje` x `d >= hoje`”. `pv1.r`, item 4: “Já a célula ausente de vínculo inativo só nasce em `d > hoje`.” `pv3.r`, limites: “não houve acesso a banco/log nem execução da suíte com banco; implantação e pouso efetivos permanecem [nao verificado]”. Nenhuma triagem fornecida apresenta a comparação de dados ou a reversão solicitada pelo opus. | O D7 e sua mudança de universo estão em `app/ponto/portas/celula.py:877` e `:883`; o filtro impede entrada dos inativos em `d <= hoje` em `:908`. Isso permite conferir a intenção e o comportamento estático dessa guarda; não comprova resultado de produção ou reversão em `logs/` **[nao verificado]**. |

Para **7**, eu evitaria **(a)** sustentado somente por “O que mais esta errado na obra inteira”: esse trecho genérico não demonstra que o tema de DNA órfão estava presente. Também não há base para **(b)**: as triagens descartam relógios interiores, sem descartar esse problema de integridade. **[nao verificado]**

## 13/11 00:43 · sessao 7adadbd22ff9 · BUG-CADASTRO-C5E · sonda · origem=Code

Saída fornecida, reproduzida integralmente; nova execução [nao verificado]:

```text
sonda_leitura: sonda_dna_orfao.py [95a58eba670edb78] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

ecs_existentes=1397
geradora_DIFERE_do_dna_passado_gerada                               1  amostra=[19829]
geradora_ok_futuro_gerada                                        8114  amostra=[138708, 138709, 138710, 138711, 138712, 138713, 138714, 138739, 138740, 138741]
geradora_ok_hoje_gerada                                           607  amostra=[138707, 138738, 138769, 138800, 138831, 138862, 138893, 138924, 138955, 138986]
geradora_ok_passado_gerada                                     135480  amostra=[110646, 111453, 138688, 116363, 113623, 113533, 110654, 16987, 122814, 116366]
total                                                          144202  amostra=[]
total_erros=0
```

Conferência sobre o dado fornecido: `1 + 8114 + 607 + 135480 = 144202`; classes somam o total e `total_erros=0`. [nao verificado]

## 13/11 00:41 · sessao e5e906e62ba5 · - · - · origem=Ronald (tmux)

Cuidados para a reescrita:

- Particionar perguntas pela coluna própria `PD.colaborador_id`; o modelo declara essa identidade e seu vínculo com a disputa. As disputas devem ser particionadas por `DS.colaborador_id`, mantendo todas as perguntas prefetched. [app/chamados/models.py:2448; app/chamados/models.py:2454; app/chamados/models.py:1859]
- `Registro` do tipo assume IDs crescentes e eventos contíguos. A v1 tem relatórios ordenados por categoria/dias, fios por criação decrescente e colaboradores repetidos nas janelas. Usar segmentos por chamada/contexto, ou acumuladores comutativos por faixa de IDs; não aplicar silenciosamente a precondição do tipo. [app/chamados/relatorios_disputa.py:52; app/chamados/relatorios_disputa.py:60; app/chamados/services/fio.py:206; e3_diff_leitores_v1.py:1098]
- Limpar `SEQUENCIAS` por chamada raiz exige salvar/restaurar os contadores ao entrar em `rodar` aninhado, para preservar as ordens das observações do pai. [e3_diff_leitores_v1.py:331; e3_diff_leitores_v1.py:501; e3_diff_leitores_v1.py:1200]
- Preservar `carga_da_tarefa`, `tarefa_do_dia` e `dias_trancados` com a janela e os pares completos no agregado. `agora` é obrigatório nas duas funções da tarefa; a adaptação da v1 o injeta. [app/chamados/services/tarefa_do_dia.py:568; app/chamados/services/tarefa_do_dia.py:573; app/chamados/juizes.py:1004; e3_diff_leitores_v1.py:491]
- `sincronizar` compara o alvo com todas as pautas vivas da família: deve receber o apurado inteiro. [app/ponto/services/pautas_dos_discordantes.py:155; app/ponto/services/pautas_dos_discordantes.py:169]
- O resultado de `relatorios_disputa.coletar` permite contar as quatro categorias fixas, incluindo `PRONTA_FECHAR`, sem nomes. O retorno real contém nomes, mas a projeção da v1 os exclui. [app/chamados/relatorios_disputa.py:6; app/chamados/relatorios_disputa.py:52; e3_diff_leitores_v1.py:1001]
- A descrição literal de “PRONTO 7 do aval” não está no dado fornecido nem foi localizada nos arquivos consultados. [nao verificado]

Referências para cobertura:

| Leitor | Local |
|---|---|
| Juiz e faces SQL sem texto | [app/chamados/juizes.py:1515]; [app/chamados/juizes.py:1531]; [app/chamados/juizes.py:1541] |
| Property `.viva` / queryset global `q_vivas` | [app/chamados/models.py:3417]; [app/chamados/models.py:3368] |
| Disputa inteira / respostas sem veredito | [app/chamados/models.py:1827]; [app/chamados/models.py:1854] |
| Explicador inteiro | [app/chamados/services/explicador.py:108]; [app/chamados/services/explicador.py:139] |
| Fio app/admin/explicação | [app/chamados/services/fio.py:190]; [app/chamados/services/fio.py:343] |
| Flags do cartão / resposta enviada | [app/chamados/services/fio.py:354]; [app/chamados/services/fio.py:377] |
| Raiox créditos globais | [app/chamados/services/raiox.py:51]; [app/chamados/services/raiox.py:135] |
| Cobrança direta e via pauta | [app/chamados/services/cobranca.py:221]; [app/chamados/services/cobranca.py:266] |
| Folha por empresa/janela | [app/folha/fora_da_folha.py:113]; [app/folha/fora_da_folha.py:153]; [app/folha/fora_da_folha.py:215] |
| Retrato e contadores | [app/folha/retrato_folha.py:1120]; [app/folha/retrato_folha.py:114] |
| Beco explícito/corrente | [app/escala/services/cadastro_realidade.py:442]; [app/escala/services/cadastro_realidade.py:553] |
| Furo por competência/corrente | [app/chamados/services/furo_sem_canal.py:43] |
| Pautas apurar/dias/sincronizar | [app/ponto/management/commands/pautas_discordantes.py:56]; [app/ponto/services/pautas_dos_discordantes.py:46]; [app/ponto/services/pautas_dos_discordantes.py:155] |

A observação de tarefa interna continua junto de cada chamador: a função real chama a alimentação global sem filtro adicional. [app/chamados/services/tarefa_do_dia.py:517; app/chamados/services/tarefa_do_dia.py:531; app/chamados/juizes.py:1558]

## 13/11 00:40 · sessao 79ed2ad98727 · - · - · origem=Ronald (tmux)

Recomendações para o script:

- Preserve o `perfil` da v1: o teste inicial `retorno is None` deixa `vigia.ramo` sem eventos porque `_listar` só incrementa contadores e acrescenta linhas, sem `return`. Declare esse limite; não mova a guarda. [app/chamados/services/vigia_fio.py:629] [nao verificado]
- O alcance real devolve linhas `[celula_id, colab_id, data, competencia, rota, causa, detalhe, papel]`. Com `limite=sys.maxsize`, confira `alcance_dias_omitidos == 0` e `len(alcance_dias) == alcance_universo`. [app/chamados/services/vigia_fio.py:549; app/chamados/services/vigia_fio.py:632]
- Para o plano da v1, distribua **todas** essas linhas pelo colaborador, incluindo rotas com porta e baldes legais. Não use a segunda posição de `BF._alcance`: ela contém exclusivamente `sem_porta`. [e3_diff_leitores_v1.py:1349; app/chamados/management/commands/backfill_fio_morto_e4b.py:76]
- `BF._alcance(empresas, hoje, agora)` aceita `agora` obrigatório; o `adaptar` da v1 já o injeta. `ato_do_dia(colab, dia, agora, *, hoje)` chama a tarefa real e a porta de reabertura com `executar=False`. [app/chamados/management/commands/backfill_fio_morto_e4b.py:76; app/chamados/management/commands/backfill_fio_morto_e4b.py:82; app/chamados/management/commands/backfill_fio_morto_e4b.py:90]

Emita os contadores nativos, separados do digest, para explicar mudanças:

| Classe | Campos reais |
|---|---|
| Rotas | `alcance_porta_fila`, `alcance_porta_pergunta`, `alcance_de_hoje_pra_frente`, `fio_morto_alcance`. [app/chamados/services/vigia_fio.py:575] |
| Quatro leis | `alcance_` + cada item de `BALDES_FORA_DO_FIO`; a constante contém decisão pendente, ausência pendente, escala não descreve o dia e fio do admin. [app/chamados/services/vigia_fio.py:536; app/chamados/services/vigia_fio.py:578] |
| Causas | `alcance_a` até `alcance_e`; leia `CAUSAS_ALCANCE`. [app/chamados/services/vigia_fio.py:535; app/chamados/services/vigia_fio.py:705] |
| Detalhes de `e` | `alcance_e_detalhe`; a função distingue sem célula, dono do dia na lista, sem pergunta viva, terminal, sem âncora, fora do app e outra. [app/chamados/services/vigia_fio.py:676; app/chamados/services/vigia_fio.py:690; app/chamados/services/vigia_fio.py:707] |
| Papéis | `alcance_por_rota_papel` e `alcance_porta_exige_o_admin`. [app/chamados/services/vigia_fio.py:630; app/chamados/services/vigia_fio.py:653] |

Identidades para emitir como `{esquerda, direita, igual}`:

1. Portas fila + pergunta + hoje em diante + quatro baldes legais + fio morto = universo. [app/chamados/services/vigia_fio.py:531]
2. Causas `a+b+c+d+e` = fio morto. [app/chamados/services/vigia_fio.py:532]
3. Soma dos detalhes de `e` = `alcance_e`. [app/chamados/services/vigia_fio.py:705]
4. Soma de `alcance_por_rota_papel` = universo. [app/chamados/services/vigia_fio.py:554]
5. Quantidade de dias emitidos + omitidos = universo. [app/chamados/services/vigia_fio.py:592; app/chamados/services/vigia_fio.py:632]
6. No censo antigo: fio vivo + quatro leis + fio morto + hoje em diante + fora do vocabulário = lâmpadas acesas. [app/chamados/services/vigia_fio.py:104]
7. Sem pergunta nenhuma + pergunta morreu = fio morto. [app/chamados/services/vigia_fio.py:449]
8. Com alvo + sem alvo + sem pergunta na porta = fio morto. [app/chamados/services/vigia_fio.py:467]
9. Soma de `porta_<classe>` para `CLASSES_DA_PORTA` = `porta_universo_nos_dias_mortos`. [app/chamados/services/vigia_fio.py:369; app/chamados/services/vigia_fio.py:463]

Papéis são rótulos das fontes: fio da tarefa; permissão do Resolver dia; descrição do status da ausência; ou dono da lista do admin. Para impedir texto livre inesperado, use vocabulário fechado extraído dessas constantes/choices e digest de qualquer rótulo desconhecido. [app/chamados/services/vigia_fio.py:171; app/chamados/services/vigia_fio.py:304; app/chamados/services/vigia_fio.py:238]

A queda real da E3 e o resultado de RSS continuam sem medição nesta tarefa. [nao verificado]

## 13/11 00:39 · sessao 8f78cf174fb0 · BUG-CADASTRO-C5E · verificador · origem=Code

**REPROVA.** Há um contraexemplo estrutural na reversão por snapshot: a exclusão do vínculo pode zerar a geradora da célula futura sem retirar do DNA o vínculo apagado. O vigia apenas conta essa divergência. (`app/colaboradores/services/vinculo.py:1029,1034`; `app/escala/models.py:1693,1697`; `app/ponto/services/vigia_cadastro_futuro.py:60`)

**Limite desta verificação:** a cópia fornecida não contém `.git`; o comando de diff solicitado falhou. Portanto, identidade do HEAD, pertencimento de cada função ao diff inteiro e exaustividade de A/B/G ficam **[não verificado]**. Abaixo separo o delta fornecido dos achados verificáveis no código.

**A. O delta cura o achado 4/6 específico: sim.** `_ec_vigencia_regenera_celulas` lê `localdate()` uma vez, usa esse valor em `_ate` e entrega o mesmo dia aos caminhos de passado e futuro. O arquivo entrou no universo com teto 1 para cada signal. (`app/escala/signals.py:67,68,74,82`; `app/ponto/tests/test_cadastro_futuro_c5e.py:332,352,353`)

O código ainda contém estas funções com múltiplos sítios de leitura dentro do universo da catraca; pertencimento de **cada função** ao diff inteiro: **[não verificado]**.

| Função | Contagem e natureza |
|---|---|
| `julgar_desligamento` | **2** `date.today()`. Ambas executam quando a data é omitida: a demissão pode receber D e a classificação usar D+1. (`app/colaboradores/services/desligamento.py:20,40,58`) |
| `reabrir_vigencia_impossivel` | **2**, com autor: `date.today()` para o histórico e `localdate()` para o suplemento. (`app/colaboradores/services/vinculo.py:740,752`) |
| `regenerar_celulas_vinculo` | **2 sítios** de `now()`, ambos carimbos de regeneração; dentro do laço, podem executar uma vez por célula reescrita. Recebe `hoje`. (`app/ponto/portas/celula.py:113,263,308,319`) |
| `julgar_celula` | **3**: dia local, comparação com fim do turno e instante do julgamento. As três podem executar para hoje, turno vencido e `agora` omitido. (`app/ponto/services/cartorio.py:2016,2037,2040`) |

Funções que **recebem `hoje` e contêm leitura**, no universo examinado; novamente, pertencimento individual ao diff: **[não verificado]**.

| Função | Sítios de leitura |
|---|---|
| `executar_desligamento` | **1**, `now()` para carimbo. (`app/colaboradores/services/desligamento.py:62,135`) |
| `regenerar_celulas_vinculo` | **2**, carimbos. (`app/ponto/portas/celula.py:113,263,308`) |
| `reverter_regeneracao_dia` | **1**, carimbo. (`app/ponto/portas/celula.py:779,834`) |
| `aplicar_folgas_em_lote` | **1**, fallback. (`app/escala/folgas.py:385,401`) |
| `reparentar_folga` | **1**, fallback. (`app/escala/folgas.py:471,512`) |
| `recusa_ancora_futura` | **1**, fallback. (`app/escala/models.py:938,965`) |
| `inicio_do_efeito` | **1**, fallback. (`app/escala/services/escala_auto_executor.py:24,29`) |
| `aplicar_no_vinculo` | **1**, fallback. (`app/escala/services/escala_auto_executor.py:87,98`) |
| `desfazer` | **1**, fallback; a conversão de `historico.criado_em` não é nova leitura. (`app/escala/services/escala_auto_executor.py:118,127,130`) |
| `apurar_colab`, `apurar`, `ranking`, `ranking_colabs`, `universo_empresa` | **1 cada**. (`app/escala/services/furos_diarios.py:25,31,60,67,123,130,167,175,247,258`) |

Nos fallbacks, um `hoje` válido evita a leitura: **sítio AST não significa leitura executada em toda chamada**. Os carimbos continuam lendo o instante mesmo quando o dia foi injetado. (`app/escala/folgas.py:401`; `app/ponto/portas/celula.py:308`)

Fora do universo, também há `vinculos_5x2_6x1_trabalha_feriado(hoje=None)`, com **1** `date.today()`; o recorte de `escala/models.py` cobre somente `recusa_ancora_futura`. (`app/escala/models.py:920,926`; `app/ponto/tests/test_cadastro_futuro_c5e.py:331`)

**B. Cobertura integral do diff: [não verificado].** O censo percorre exclusivamente `UNIVERSO_RELOGIO`; não descobre arquivos pelo diff. Acrescentar leitura em arquivo ausente desse universo não altera sua contagem. (`app/ponto/tests/test_cadastro_futuro_c5e.py:578,580,581`)

Estes arquivos ligados aos caminhos examinados leem relógio e estão fora do universo declarado; **quais efetivamente pertencem ao diff inteiro: [não verificado]**. (`app/ponto/tests/test_cadastro_futuro_c5e.py:326`)

| Arquivo fora do universo | Leitura verificável |
|---|---|
| `app/core/signals.py` | `localdate()` nos dois signals de feriado. (`app/core/signals.py:36,43`) |
| `app/escala/admin.py` | `save_model`: `localdate()`. (`app/escala/admin.py:15`) |
| `app/colaboradores/management/commands/corrigir_escala_retroativa.py` | `_rodar` e `_aplicar`: uma leitura cada. (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:67,227`) |
| `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py` | `localdate()`. (`app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:37`) |
| `app/escala/management/commands/corrigir_vinculo_vigencia.py` | `localdate()`. (`app/escala/management/commands/corrigir_vinculo_vigencia.py:48`) |
| `app/colaboradores/views.py` | Exemplo: desligamento passa `localdate()` à porta. (`app/colaboradores/views.py:1437`) |
| `app/chamados/management/commands/materializar_passivo_validado.py` | `localtime()` sem valor e `now()` no ensaio. (`app/chamados/management/commands/materializar_passivo_validado.py:87,103`) |
| `app/ponto/management/commands/aplicar_col900.py` | `localdate()` e `localtime()` sem valor. (`app/ponto/management/commands/aplicar_col900.py:146,253`) |
| `app/ponto/management/commands/diff_col900.py` | `localdate()` e dois `localtime()` sem valor. (`app/ponto/management/commands/diff_col900.py:57,119,128`) |
| `app/ponto/management/commands/curar_residuo_bug115.py` | `localdate()`. (`app/ponto/management/commands/curar_residuo_bug115.py:74`) |
| `app/api/views_mensageria.py` | Exemplo: dia local e instante. (`app/api/views_mensageria.py:1026,1041`) |

`localtime()` sem valor está separado das quatro formas citadas em A porque também lê o relógio e é contado pela própria catraca. (`app/ponto/tests/test_cadastro_futuro_c5e.py:321,376,562`)

**C. Grafo e efeito dos sítios do achado 1.** Uso D para o dia capturado pelo executor e D+1 para uma leitura posterior; ocorrência desses cruzamentos em produção: **[não verificado]**. O executor captura D e o entrega aos dois fechamentos, à regeneração e ao suplemento. (`app/colaboradores/services/vinculo.py:194,219,247,330,339`)

1. **`_copiar_folga_padrao`: chamada direta do executor**, tanto na atualização quanto na criação. Sua leitura escolhe os meses da foto: dentro do mesmo mês, D→D+1 não muda o par; na virada mensal, passa a fotografar o novo mês e o seguinte, deixando de fotografar o mês que continha D nesse caminho. Só entra se houver calendário padrão e faltar calendário no vínculo. (`app/colaboradores/services/vinculo.py:157,158,163,164,165,166,284,298`)

2. **`relavrar_celulas_da_foto`: chamada transitiva do executor:** cópia → `gerar_foto_folgas` → `_gerar_foto_folgas_no_ato` → relavra. Se sua leitura for D+1, regenera até D+1 pelo caminho passado/hoje e seleciona futuras somente desde D+2. D+1 pode receber DNA novo sem a limpeza reservada a `d > hoje`. (`app/colaboradores/services/vinculo.py:165`; `app/escala/folgas.py:163,215,280,293,298`; `app/ponto/services/cadastro_futuro.py:27`; `app/ponto/portas/celula.py:312`)

3. **Signal de vigência:** o `save()` da atualização pode acioná-lo quando a vigência muda; criação é recusada pelo receiver. Se ele capturar D+1, passado/hoje vai até D+1 e futuras começam em D+2. O delta elimina a divergência **dentro desse receiver**, mas ele continua capturando um dia independente do executor. (`app/colaboradores/services/vinculo.py:283`; `app/escala/signals.py:34,54,58,67,74,82`; `app/ponto/services/cadastro_futuro.py:27`)

4. **Signal de âncora:** outro receiver do `save()`, condicionado à mudança de âncora. Capturando D+1, usa a competência de D+1, regenera até esse dia e suplementa desde D+2; na virada da competência, também muda o piso da regeneração. (`app/escala/signals.py:86,111,115,119,120,121,126,133`; `app/ponto/services/cadastro_futuro.py:27`)

5. **`_o_chao_do_dia_mudou`: chamada direta pela porta `regenerar_celulas_vinculo`**, sem receber o `hoje` dela. Contraexemplo: a porta recebe D, reescreve D+1 e limpa seu julgamento; o helper lê D+1 e chama o juiz para esse mesmo dia. Se D+1 for folga, o cartório pode julgá-lo, pois a espera pelo fim do turno só se aplica quando `trabalha is not False`. Assim pode voltar julgamento numa célula considerada futura pelo dia injetado no ato. (`app/ponto/portas/celula.py:193,312,314,392,404,464,469,472`; `app/ponto/services/cartorio.py:2016,2025,2033,2040`)

   O suplemento final só limpa novamente se encontrar diferença em trabalho, geradora ou DNA; com DNA já igual, não entra nesse ramo. (`app/ponto/portas/celula.py:297,312,314`)

6. **`reabrir_vigencia_impossivel`: outro ato**, chamado pelo command de correção de vigência. Com autor, pode registrar histórico em D e suplementar com D+1; nesse caso, a célula existente de D+1 fica fora do suplemento, que começa em D+2. O command captura seu próprio dia, mas não o entrega à reabertura. (`app/escala/management/commands/corrigir_vinculo_vigencia.py:48,86,107`; `app/colaboradores/services/vinculo.py:740,752`; `app/ponto/services/cadastro_futuro.py:27`)

Há ainda chamada direta ao pós-vínculo sem passagem de `hoje`; ele relê o dia e usa esse novo teto para julgamento e apuração. Não classifico esses caminhos como “resto do ato”; descrevo a passagem efetiva dos valores. (`app/colaboradores/services/vinculo.py:345,347`; `app/colaboradores/services/pos_vinculo.py:56,57,75,82`)

**D. A mudança de universo amplia candidatos; não provoca nova regravação de passado/hoje por si só.** O predicado novo exclui somente `ativa=False AND data_fim IS NULL`. Logo, entram os inativos com fim declarado; nenhum ativo sai. (`app/escala/alimentacao.py:199,202`; `app/ponto/portas/celula.py:883`)

| Classe | Resultado |
|---|---|
| `ativa=True` | Continua candidata. (`app/ponto/portas/celula.py:883`) |
| `ativa=False AND data_fim IS NULL` | Continua fora. (`app/escala/alimentacao.py:202`) |
| `ativa=False AND data_fim IS NOT NULL` | Entra como candidata; só alcança dia coberto pela vigência. (`app/ponto/portas/celula.py:883,906`; `app/escala/alimentacao.py:214`) |

Para o inativo, exige-se adicionalmente **`d > hoje`, escolha pela juíza e célula ausente**. Passado e hoje são barrados; futura existente é pulada. Portanto essa ampliação não passa a regravar passado/hoje. (`app/ponto/portas/celula.py:908,909,913,930,935`)

**E. Achados 6 e 7.**

- **6 procede quanto ao comportamento:** o cron chama `_regenerar_hoje_existentes`, seleciona geradas de hoje e manda regenerar `[hoje, hoje]`. Havendo diferença, a porta reescreve trabalho, geradora e DNA, respeitando suas guardas. Isso é independente de D7. Se o aval proíbe especificamente essa escrita de hoje: **[não verificado]** no literal disponibilizado. (`app/ponto/management/commands/gerar_celulas.py:316,323,324,347`; `app/ponto/portas/celula.py:284,297,306,310`)

- **7 procede como possibilidade estrutural:** o snapshot apaga vínculos ausentes do retrato; `SET_NULL` altera a FK, enquanto o DNA é um JSON independente. Depois, somente os vínculos do snapshot recebem regeneração. (`app/colaboradores/services/vinculo.py:1022,1029,1034,1035`; `app/escala/models.py:1693,1697`)

  Contraexemplo sem depender de snapshot vazio: o vínculo removido gerou uma célula futura, e todos os restaurados terminam antes dela. Após `SET_NULL`, o ramo que limparia a futura fora de vigência exige `escala_geradora_id == ec.id`, condição que já não vale. O DNA pode continuar nomeando o vínculo apagado; o vigia incrementa o contador e o gerador pula a futura existente. Ocorrência em produção: **[não verificado]**. (`app/ponto/portas/celula.py:251,252,930,935`; `app/ponto/services/vigia_cadastro_futuro.py:55,60,61`)

**F. Escritores e leitores no delta literal.**

A única função de produção alterada é `_ec_vigencia_regenera_celulas`: **escreve `_hoje`** com `localdate()`; **leem `_hoje`** o cálculo de `_ate`, os limites da regeneração e os dois argumentos `hoje=`. A função não recebe `hoje` por parâmetro. (`app/escala/signals.py:35,67,68,72,74,82`)

Os destinos usam o valor como **dia civil local que separa passado/hoje de futuro**, não como dia de início do turno: a porta atribui `_hoje = hoje`, e o suplemento começa em `hoje + 1`. (`app/ponto/portas/celula.py:193,312`; `app/ponto/services/cadastro_futuro.py:26,27`)

O outro arquivo do delta altera constantes da catraca, sem adicionar função que produza ou consuma `hoje` em produção. (`app/ponto/tests/test_cadastro_futuro_c5e.py:332,352,353,375`)

Essa equivalência não pode ser estendida automaticamente a toda a obra: existem leitores de `date.today()` e de `timezone.localdate()`; a configuração Django declara São Paulo, mas o fuso efetivo dos processos em produção é **[não verificado]**. (`app/colaboradores/services/vinculo.py:163,194`; `app/config/settings/base.py:93,95`)

**G. Classes adversariais.**

| Classe | Resultado sustentado |
|---|---|
| Relógio fora da borda | **Presente no grafo**, inclusive nos caminhos descritos em C; há permissões explícitas na catraca para esses interiores. Isso não decide sua inclusão no trecho “resto do ato”. (`app/ponto/tests/test_cadastro_futuro_c5e.py:316,340,344,369`; `app/ponto/portas/celula.py:464`) |
| Segundo escritor fora da porta | **Efeito de escrita na célula pelo apagamento da geradora:** o collector aplica `SET_NULL`, sem passar pela regeneração do DNA. É o mecanismo do achado 7. (`app/colaboradores/services/vinculo.py:1029`; `app/escala/models.py:1693`) |
| Selo/RED insuficiente para o enunciado amplo | **Presente:** o teste de meia-noite substitui cópia de folga por lambda, helper de consequências por `Mock` e pós-vínculo por `Mock`. O assert de uma leitura certifica esse caminho reduzido. (`app/ponto/tests/test_cadastro_futuro_c5e.py:38,93`; `app/ponto/tests/test_cadastro_futuro_sem_banco.py:370,384,391`) |
| Segundo juiz / derivação paralela | Os caminhos centrais examinados consultam `vinculo_do_dia` e usam `montar_dna`; ausência dessas classes no diff inteiro: **[não verificado]**. (`app/ponto/services/cadastro_futuro.py:38,39`; `app/ponto/portas/celula.py:290,909`) |
| Ordem de trava | O snapshot adquire vínculos em ordem de PK antes das células; inversão no diff inteiro: **[não verificado]**. (`app/colaboradores/services/vinculo.py:1020,1025`) |
| Pouso antes do pré-requisito | **[não verificado]**: sem histórico Git ou evidência de execução disponibilizada. | [nao verificado]
| Campo com dois significados | No delta, `_hoje` conserva o significado de dia civil; ocorrência nova dessa classe no diff inteiro: **[não verificado]**. (`app/escala/signals.py:67,74,82`) |

**Bloqueante comprovado:** reversão por snapshot capaz de deixar DNA futuro nomeando vínculo apagado, sem reparação pelo caminho subsequente. (`app/colaboradores/services/vinculo.py:1029,1034`; `app/ponto/portas/celula.py:252`; `app/ponto/services/vigia_cadastro_futuro.py:60`)

**Impedimentos adicionais para certificar a obra inteira:** censo exaustivo do diff **[não verificado]**; RED atual não cobre os caminhos de releitura de produção identificados em C. Não transformo automaticamente todos os interiores permitidos pela catraca em novos bloqueantes de escopo. (`app/ponto/tests/test_cadastro_futuro_c5e.py:316`; `app/ponto/tests/test_cadastro_futuro_sem_banco.py:370,384,391`)

## 13/11 00:38 · sessao 631294d63c6e · - · - · origem=Ronald (tmux)

O delta cura especificamente a dupla leitura de `_ec_vigencia_regenera_celulas`: `_hoje` recebe uma única `timezone.localdate()` em `app/escala/signals.py:67`; `_ate` reutiliza esse valor em `:68`; passado e futuro recebem o mesmo dia em `:74` e `:82`. O signal entrou no universo (`app/ponto/tests/test_cadastro_futuro_c5e.py:332`) com teto 1 (`:353`).

Executei somente o extrato AST, em memória, de `leituras_de_relogio` e `censo_relogio`: **44 sítios, 39 funções; contagem igual a `TETO_RELOGIO`**. A implementação do censo está em `app/ponto/tests/test_cadastro_futuro_c5e.py:575`, e as comparações em `:592`. Não executei suite nem banco. [não verificado: execução de testes]

Funções do universo com mais de um sítio de relógio:

| Função | Contagem | Referências e natureza |
|---|---:|---|
| `julgar_desligamento` | 2 | `app/colaboradores/services/desligamento.py:20`, `:40`: `date.today()`; ambas executam se colaborador não desligado e data omitida. A data escolhida pode ser D e a classificação `agendado` usar D+1 (`:58`). |
| `reabrir_vigencia_impossivel` | 2 | `app/colaboradores/services/vinculo.py:740` (`date.today`, histórico; requer autor), `:752` (`localdate`, suplemento futuro). |
| `regenerar_celulas_vinculo` | 2 | `app/ponto/portas/celula.py:263`, `:308`: carimbos `regenerada_em`, não escolha de dia. Recebe `hoje` em `:113`. |
| `julgar_celula` | 3 | `app/ponto/services/cartorio.py:2016`, `:2037`, `:2040`: dia local, teste do fim do turno e instante do julgamento. As três podem executar no mesmo ato para hoje, turno já vencido e `agora` omitido. |

Pertença dessas funções ao diff inteiro: **[não verificado]**, pois a cópia não contém `.git`.

Funções que recebem `hoje` e também contêm leitura:

| Função | Sítios | Arquivo:linha |
|---|---:|---|
| `aplicar_folgas_em_lote` | 1 | `app/escala/folgas.py:385`, `:401` |
| `reparentar_folga` | 1 | `app/escala/folgas.py:512` |
| `recusa_ancora_futura` | 1 | `app/escala/models.py:938`, `:965` |
| `inicio_do_efeito` | 1 | `app/escala/services/escala_auto_executor.py:24`, `:29` |
| `aplicar_no_vinculo` | 1 | `app/escala/services/escala_auto_executor.py:98` |
| `desfazer` | 1 | `app/escala/services/escala_auto_executor.py:127` |
| `apurar_colab` | 1 | `app/escala/services/furos_diarios.py:25`, `:31` |
| `apurar` | 1 | `app/escala/services/furos_diarios.py:60`, `:67` |
| `ranking` | 1 | `app/escala/services/furos_diarios.py:123`, `:130` |
| `ranking_colabs` | 1 | `app/escala/services/furos_diarios.py:167`, `:175` |
| `universo_empresa` | 1 | `app/escala/services/furos_diarios.py:247`, `:258` |
| `executar_desligamento` | 1 | `app/colaboradores/services/desligamento.py:62`, `:135` (`now`, carimbo) |
| `regenerar_celulas_vinculo` | 2 | `app/ponto/portas/celula.py:113`, `:263`, `:308` |
| `reverter_regeneracao_dia` | 1 | `app/ponto/portas/celula.py:779`, `:834` (`now`, carimbo) |

Nos fallbacks `hoje or relógio`/`if hoje is None`, fornecer um `hoje` válido evita a leitura; é preciso distinguir **sítio AST** de **leitura realmente executada**. Exemplos: `app/escala/folgas.py:401`, `app/escala/services/furos_diarios.py:30`.

Há ainda uma função do mesmo arquivo de modelos **não coberta** pelo recorte `escala/models.py::recusa_ancora_futura`: `vinculos_5x2_6x1_trabalha_feriado(hoje=None)`, com uma `date.today()` em `app/escala/models.py:920`, `:926`; o universo restringe esse arquivo a uma única função em `app/ponto/tests/test_cadastro_futuro_c5e.py:331`.

Arquivos candidatos da obra que leem relógio e estão fora do universo; pertença exata ao diff **[não verificado]**:

| Arquivo | Leituras verificadas |
|---|---|
| `app/core/signals.py` | `:36`, `:43`: um `localdate` por signal. O teste carrega o módulo em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:508`. |
| `app/escala/admin.py` | `:15`: um `localdate` em `save_model`; método real extraído em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:493`. |
| `app/colaboradores/management/commands/corrigir_escala_retroativa.py` | `:67` em `_rodar`, `:227` em `_aplicar`; teste carrega `_aplicar` em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:623`. |
| `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py` | `:37`: um `localdate`. |
| `app/escala/management/commands/corrigir_vinculo_vigencia.py` | `:48`: um `localdate`. |
| `app/chamados/management/commands/materializar_passivo_validado.py` | `:87` (`localtime` sem argumento) e `:103` (`now` no ensaio): **2 normalizadas**, **1 na lista literal do usuário**. |
| `app/ponto/management/commands/aplicar_col900.py` | `:146` (`localdate`), `:253` (`localtime` sem argumento): **2 normalizadas**, **1 literal**; auxiliares também leem `now` em `:70`, `:102`. |
| `app/ponto/management/commands/diff_col900.py` | `:57` (`localdate`), `:119`, `:128` (`localtime` sem argumento): **3 normalizadas**, **1 literal**. |
| `app/ponto/management/commands/curar_residuo_bug115.py` | `:74`: um `localdate`. |
| `app/api/views_mensageria.py` | Exemplo relevante: `api_mensageria_pessoa`, **2**, `:1026` (`localdate`) e `:1041` (`now`). Fonte lida pelo teste em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:895`. |
| `app/colaboradores/models.py` | `Colaborador.salario_em`: uma `today` em `:471`; `LinhaTransporte.custo_mensal`: uma `now` em `:824`. |

`app/chamados/signals.py` não apresentou chamada de relógio no censo AST. [não verificado: pertinência ao diff inteiro]

F, **delta literal**: a única função de produção alterada é `_ec_vigencia_regenera_celulas`; escritor de `_hoje` em `app/escala/signals.py:67`, leitores em `:68`, `:72`, `:74`, `:82`. Trata-se de dia civil local que separa passado/hoje de futuro. O outro arquivo do delta altera dados da catraca, não funções que escrevam/leiam o `hoje` de produção (`app/ponto/tests/test_cadastro_futuro_c5e.py:332`, `:352`, `:353`, `:375`).

Limite adversarial do RED: `MeiaNoiteNoAtoTest` reutiliza a fixture em `app/ponto/tests/test_cadastro_futuro_c5e.py:38`; essa fixture troca `_copiar_folga_padrao` por lambda (`app/ponto/tests/test_cadastro_futuro_sem_banco.py:391`), `_o_chao_do_dia_mudou` por `Mock` (`:384`) e `reconciliar_apos_vinculo` por `Mock` (`:370`). Portanto o assert de uma leitura em `app/ponto/tests/test_cadastro_futuro_c5e.py:93` verifica esse caminho reduzido; não prova uma leitura no grafo inteiro de produção. A cobertura do grafo completo é **[não verificado]**.

## 13/11 00:36 · sessao eac820637f03 · - · - · origem=Ronald (tmux)

Para C, tomando D como dia capturado em `executar_vinculo` e D+1 como leitura posterior à meia-noite; ocorrência real em produção [não verificado]. O executor captura D em `app/colaboradores/services/vinculo.py:194`, passa D aos dois fechamentos (`:219`, `:247`), à regeneração até D (`:328`) e ao suplemento futuro (`:339`).

| Sítio | Grafo observado | Efeito concreto se sua leitura devolver D+1 |
|---|---|---|
| `_copiar_folga_padrao` | **Chamada direta** por `executar_vinculo`, tanto na atualização quanto na criação: `app/colaboradores/services/vinculo.py:284`, `:298`. Somente entra com calendário padrão e sem calendário já escolhido: `:157`–`:159`. | `date.today()` escolhe o **mês da foto**, não o dia-turno: `:163`–`:166`. Dentro do mesmo mês, a mudança D→D+1 não muda os dois meses escolhidos; na virada de mês troca o par mês corrente/próximo pelo novo par, deixando de fotografar o mês que continha D nesse caminho (`:164`–`:166`). `date.today()` usa data do processo; equivalência com data civil local do Django [não verificado]. |
| `relavrar_celulas_da_foto` | **Chamada transitiva** do executor: `_copiar_folga_padrao` → `gerar_foto_folgas` (`vinculo.py:165`–`:166`) → `_gerar_foto_folgas_no_ato` (`app/escala/folgas.py:162`–`:163`) → `relavrar_celulas_da_foto` (`:215`–`:218`). | Lê seu próprio `hoje=D+1` (`folgas.py:280`), regenera `desde..D+1` como passado/hoje (`:290`–`:296`) e só seleciona futuras a partir de D+2 (`:298`–`:299`; `app/ponto/services/cadastro_futuro.py:27`). D+1 recebe DNA novo pelo caminho passado/hoje, sem a limpeza reservada a `d > hoje` (`app/ponto/portas/celula.py:289`–`:315`). Pode chegar ao julgamento de D+1 no rabo (`:392`, `:464`–`:472`); julgamento efetivo obedece às guardas descritas abaixo. |
| `_ec_vigencia_regenera_celulas` | **Signal de `post_save`** registrado em `app/escala/signals.py:34`; no executor a atualização existente faz `save()` (`app/colaboradores/services/vinculo.py:283`). Só roda se vigência mudou e não é criação/raw (`signals.py:54`–`:59`). | Uma leitura própria D+1 (`:67`) governa **ambas** as partes: passado/hoje até D+1 (`:72`–`:75`) e futuras desde D+2 (`:81`–`:83`; `cadastro_futuro.py:27`). O buraco interno antigo entre duas leituras foi eliminado; continua tendo um dia próprio distinto do D do executor (`vinculo.py:194`). DNA de D+1 é tratado no ramo passado/hoje; seu julgamento segue as guardas do cartório (`celula.py:312`; `cartorio.py:2025`, `:2033`). |
| `_ec_ancora_regenera_celulas` | **Outro signal de `post_save`**, `app/escala/signals.py:86`; `save()` do executor em `vinculo.py:283` pode acioná-lo quando muda a âncora. Recusa criação/raw, âncora nula ou inalterada (`signals.py:111`–`:116`). | Lê D+1 (`:119`), escolhe janela da competência por D+1 (`:120`), regenera até D+1 (`:124`–`:127`) e suplementa desde D+2 (`:132`–`:134`; `cadastro_futuro.py:27`). Na virada da competência, também muda o início da janela histórica (`signals.py:120`–`:121`). |
| `_o_chao_do_dia_mudou` | **Chamada direta pela porta do aval** `regenerar_celulas_vinculo` (`app/ponto/portas/celula.py:392`); recebe dias tocados, **não recebe `hoje`** (`:404`–`:405`). Também chamado por edição humana (`:665`) e reversão (`:858`). | A porta usa D (`:193`), reescreve DNA futuro D+1 e desfaz julgamento (`:306`–`:315`). Depois o rabo lê D+1 (`:464`), deixa D+1 passar no filtro (`:469`) e chama o juiz (`:472`). O juiz relê o relógio (`app/ponto/services/cartorio.py:2016`), recusa futuro (`:2025`) e, para hoje com trabalho, espera fim do turno (`:2033`–`:2038`). **Contraexemplo concreto: D+1 com `trabalha=False`** não passa pela espera de turno e pode ser julgado novamente (`:2033`, `:2040`), embora seja futuro segundo o D injetado na porta. |
| `reabrir_vigencia_impossivel` | **Outro ato**: chamador de produção encontrado é `app/escala/management/commands/corrigir_vinculo_vigencia.py:86`; recebe `ec`, `data_fim`, autor/motivo, sem `hoje` (`app/colaboradores/services/vinculo.py:674`). Não há chamada a essa função no corpo de `executar_vinculo` (`:170`–`:387`). | Com autor, grava `HistoricoVinculo.data=D` por `date.today()` (`vinculo.py:740`; escritor do campo em `app/colaboradores/services/historico_vinculo.py:15`), depois passa D+1 ao suplemento (`vinculo.py:752`), que só seleciona desde D+2 (`cadastro_futuro.py:27`). Uma célula **existente** de D+1 com DNA antigo fica fora desse suplemento e não há regeneração do passado/hoje nessa função (`vinculo.py:729`–`:753`). O `update()` não emite signal (`:730`, `:746`). O command já havia capturado outro `hoje` na borda (`corrigir_vinculo_vigencia.py:48`), usado no fechamento seguinte (`:107`–`:108`) mas não entregue à reabertura (`:86`–`:88`). |

Atenção à interação: quando um caminho intermediário julga D+1, o suplemento final do executor, usando D, só limpa esse julgamento **se reescrever DNA**; o ramo exige diferença em `trabalha`, geradora ou DNA antes de chamar a limpeza (`app/ponto/portas/celula.py:297`–`:315`). Portanto um julgamento recém-criado pode permanecer se o suplemento encontrar o mesmo DNA (`:297`). Dado de produção demonstrando esse caso [não verificado].

Leituras adicionais no executor: `reconciliar_apos_vinculo` é chamado diretamente sem `hoje` (`app/colaboradores/services/vinculo.py:345`–`:348`), lê D+1 (`app/colaboradores/services/pos_vinculo.py:56`), inclui D+1 no intervalo (`:57`) e tenta julgá-lo (`:64`–`:75`), além de apurar furos até esse teto (`:82`). O próprio comentário do executor declara essa releitura como sítio interior fora da cura (`vinculo.py:189`–`:192`); isso descreve o código, não resolve a interpretação do aval.

_4494 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
