# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 12:26** · publicado em **06/11/2026 12:29** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 12:26 · sessao 6d24b7e0f74d · CODEX-CONSTRUTOR-NEGOCIO · verificador · origem=Code

**Cura parcial: o delta fecha recusas específicas, introduz uma restrição excessiva no nome da resposta e mantém lacunas no selo.** (`bin/codex_pergunta.sh:64,123,273-275`; `bin/tests/test_codex_pergunta.sh:203-206,309-311,363-364`)

Avaliação estática da cópia e do anexo. Execução da suíte, resultados dos mutantes, isolamento real e correspondência Git da cópia com o commit informado: **[nao verificado]**. [nao verificado]

1. **ALTA — v2.r #1: igual; não fecha as escritas por symlink em LOGD.** `slot.gate` e `slot.N` continuam abrindo com truncamento; `chamadas.log`, JSONL/`.err` e `limite.log` continuam sem proteção individual contra symlink. O fluxo alcança também os quatro papéis de leitura. (`bin/codex_pergunta.sh:139,172,228-239,281-285,359`)

2. **MÉDIA — v2.r #2: igual.** A recusa inicial de `.`/`..` e destinos não regulares permanece; o delta não acrescenta proteção à publicação contra troca posterior de ancestral ou transformação do destino em diretório. A publicação continua sendo `mv -f`, depois das esperas. Exploração efetiva: **[nao verificado]**. (`bin/codex_pergunta.sh:87-94,227-255,289`)

3. **ALTA — v2.r #3: fecha dois subpontos; não fecha a exclusão global.**
   
   A resposta com basename `construtor.vaga*` agora é recusada em qualquer papel, fechando a substituição direta do inode por publicação de resposta. A abertura do fd 6 também passa a recusar explicitamente quando falha. (`bin/codex_pergunta.sh:64,123`)
   
   Permanece uma vaga por **raiz do script**, sem vinculação ao repositório comum: cópias em raízes distintas calculam caminhos distintos para a vaga. Execução concorrente desse contorno: **[nao verificado]**. (`bin/codex_pergunta.sh:49,85,112-124`)

4. **ALTA — v2.r #4: fecha parcialmente.** A revalidação agora recusa status não vazio, erro de `git status` e alteração do backpointer consultado. Isso fecha o arquivo solto surgido durante a espera. (`bin/codex_pergunta.sh:273-275`; `bin/tests/test_codex_pergunta.sh:195-198`)
   
   Ainda não repete registro/common-dir nem compara inodes de WT/app. Uma substituição por diretório real com conteúdo equivalente não é distinguida por esses controles; continua existindo intervalo entre conferência e chamada. A corrida do `.lock` também permanece: teste de symlink seguido de abertura comum, enquanto a triagem usa `O_NOFOLLOW`. Efeitos reais desses contornos: **[nao verificado]**. (`bin/codex_pergunta.sh:109-117,253-255,271-281`; `bin/disjuntor.py:779-788`)

5. **ALTA — v2.r #5: igual.** O inventário continua observando estado final: diff, status, HEAD, checksum de `.git` e links dos caminhos encontrados. Tocar um proibido e restaurá-lo sem alterar HEAD não deixa necessariamente evidência nesses controles. O teste `commit_desfaz` continua exercitando commit/revert, que altera HEAD. (`bin/codex_pergunta.sh:316-341`; `bin/tests/test_codex_pergunta.sh:264`)
   
   Também permanece o limite dos hardlinks: a validação inicial não examina compartilhamento de inode, e o inventário verifica `st_nlink` somente dos caminhos tocados no estado final. O contorno “hardlink preexistente, escrita, substituição por cópia regular” não foi excluído pelo delta; efeito sobre a viva: **[nao verificado]**. (`bin/codex_pergunta.sh:125-128,329-335`)

6. **MÉDIA — v2.r #6: fecha a alteração tardia simples do backpointer; sobra a janela anterior ao checksum.** Agora `$_GD/gitdir` é relido imediatamente antes da chamada. Porém `$_GD` continua sendo o caminho capturado na validação inicial. (`bin/codex_pergunta.sh:115-116,274`)
   
   Se `.git` for reapontado depois daquela validação e antes de `GITF_ANTES`, o checksum pode incorporar o novo apontamento, enquanto a revalidação continua lendo o backpointer da entrada antiga. HEAD/status equivalentes não demonstram que a entrada atual é a mesma. Execução dessa sequência: **[nao verificado]**. (`bin/codex_pergunta.sh:115-128,272-274`)

7. **ALTA — v2.r #7: igual; a prova real permanece sem autenticidade das tentativas.** O bloco continua entregue como texto ao Codex, que também pode escrever os `.out` posteriormente aceitos como evidência. Produzir o controle positivo e arquivos contendo as assinaturas esperadas pode satisfazer essas verificações sem executar os acessos, caso os controles externos passem. Verde efetivo desse contorno: **[nao verificado]**. (`bin/tests/prova_codex_pergunta_real.sh:24-47,58-80,82-100`)
   
   Permanecem também as provas incompletas de socket e sobrevivência: o socket dispensa assinatura de negação; a ausência posterior do processo não confirma que ele chegou a iniciar. Banco e integridade completa dos metadados Git: **[nao verificado]**. (`bin/tests/prova_codex_pergunta_real.sh:40-43,60-72,78-95`)

8. **MÉDIA — v2.r #8 e os dois problemas adicionais do selo: melhora parcial.**
   
   Os casos de migrations/models deixam de exemplificar somente `folha`: entram `ponto/migrations` e `escala/models.py`. A limpeza de captura em `c_absoluto` fecha o uso de captura obsoleta apontado na v2.r. (`bin/tests/test_codex_pergunta.sh:145-149,241-244,277`)
   
   A ausência de `slot=` agora gera falha explícita em `c_revalida`, fechando o verde silencioso do caso original sem essa evidência. Contudo, `_falha` não interrompe o caso, e qualquer `FALHA` continua contando como morte de mutante, inclusive falha de preparação/temporização. (`bin/tests/test_codex_pergunta.sh:21,189-193,362-366`)
   
   As mortes por guarda redundante permanecem: sem `absoluto`, o nome ainda recusa; sem `inv_diff`, o commit ainda altera HEAD. O selo pode contabilizar essas mortes por mensagem ausente, sem demonstrar passagem da operação proibida. (`bin/codex_pergunta.sh:96-101,327-328`; `bin/tests/test_codex_pergunta.sh:99-101,148-149,254,317,340,363-364`)

9. **MÉDIA — v2.r #9: comportamento igual; observação sobre cabeçalho fechada.** O produtor absoluto permanece compatível com o hook; transporte da linha ao topo do RELATO continua **[nao verificado]**. Quando PID e arquivo são declarados, ambos são avaliados, e a primeira observação do arquivo não comprova avanço. (`bin/codex_pergunta.sh:263`; `bin/hook_stop_fila1.py:515-528,640-641,655-680`)
   
   `.rc` continua condicionado a `INI=1`, podendo faltar nas recusas iniciais e usar `chamada=-` antes da atribuição de JSONL. Leitor operacional desse sinal: **[nao verificado]**. O cabeçalho agora descreve corretamente vaga fixa e produtor absoluto. (`bin/codex_pergunta.sh:77-148,172,260,40-46`)

10. **BAIXA — v2.r #10: igual.** A pergunta continua exigindo apenas legibilidade; cabeçalho e dados continuam usando `mktemp`; a raspagem pode chamar `relato_nomes.sh`, cujo código consulta colaboradores via Docker. Execução desses acessos nesta avaliação: **[nao verificado]**. (`bin/codex_pergunta.sh:62,151-156,202`; `bin/relato_nomes.sh:7-21`)

11. **MÉDIA — Regressão nova: reserva de nome excessiva em todos os papéis.** Uma resposta legítima como `/tmp/construtor.vaga.relatorio.txt` passa a ser recusada, embora não possa substituir `<raiz>/logs/construtor.vaga`. A regra compara somente basename, usa prefixo `*` e roda antes da seleção do papel. Portanto afeta leitura, censo, verificador, sonda e construtor. (`bin/codex_pergunta.sh:64,71`)
   
   Para uma WT saudável e inalterada, não identifiquei um falso positivo novo específico: status vazio e backpointer estável satisfazem as novas condições, já exigidas inicialmente. Confirmação executada: **[nao verificado]**. Os argumentos e o cabeçalho dos quatro papéis de leitura permanecem iguais; o reuso de WS existente continua sem autenticação do conteúdo. (`bin/codex_pergunta.sh:116,126-127,175-180,211-213,273-285`)

12. **MÉDIA — O selo novo ainda admite “morte” por motivo diferente e deixa uma condição nova sem exercício independente.**
   
   - **`vaga_abre`: morte possível só pela mensagem.** O caso provoca falha de abertura com um diretório. Neutralizada `_recusa2`, sem fd 6 herdado o `flock` seguinte ainda pode recusar com rc 2, agora por “vaga ocupada”. A exigência de “nao abriu” mata o mutante sem demonstrar continuação sem trava. O cenário relevante de fd 6 herdado não é montado pelo caso. Resultado executado: **[nao verificado]**. (`bin/codex_pergunta.sh:123-124`; `bin/tests/test_codex_pergunta.sh:99-101,203-206,311,363-364`)
   - **`revalida_status`: caso próprio, sem mutante próprio.** O caso adiciona arquivo permitido durante a espera, mas o mutante `revalida` executa apenas a troca de `app` por symlink. Assim, a morte desse mutante não certifica a condição de status. (`bin/tests/test_codex_pergunta.sh:191,195-198,309`)
   - **Backpointer final: cobertura ausente.** `c_gitdir_volta` altera `.git` antes da validação inicial; nenhum caso listado altera `gitdir` durante a espera. Neutralizar somente a comparação final não é exercitado por um cenário que dependa dela. Verde efetivo dessa mutação: **[nao verificado]**. (`bin/tests/test_codex_pergunta.sh:180-206,276-278`; `bin/codex_pergunta.sh:116-117,274`)
   - **`vaga_reservada`: ligação direta adequada por inspeção.** O caso pede exatamente o nome protegido, e `_recusa` exige rc, mensagem e ausência de chamada ao Codex. Isso cobre a recusa direta, embora não reproduza a sequência concorrente com uma vaga já presa. (`bin/tests/test_codex_pergunta.sh:98-101,199-201,310`)

13. **INFORMATIVA — Escritores e leitores dos arquivos/campos envolvidos.**

   | Arquivo/campo | Quem escreve | Quem lê/usa |
   |---|---|---|
   | `<raiz>/logs/construtor.vaga` | Wrapper cria/abre em append no fd 6; não grava conteúdo. O selo também abre, move e substitui por diretório. (`bin/codex_pergunta.sh:122-123`; `bin/tests/test_codex_pergunta.sh:175,204-205`) | Construtor disputa `flock`; guarda global examina o basename da resposta; selo verifica a recusa. (`bin/codex_pergunta.sh:64,124`; `bin/tests/test_codex_pergunta.sh:199-206`) |
   | Status da WT | Não é um arquivo novo: inicialmente vira `_ST`; na revalidação é saída transitória de `git status`. O selo cria o arquivo que o altera. (`bin/codex_pergunta.sh:126-127,273`; `bin/tests/test_codex_pergunta.sh:198`) | Validação inicial, revalidação e inventário final. (`bin/codex_pergunta.sh:126-127,273,320-322`) |
   | `<common-dir>/worktrees/<entrada>/gitdir` | O selo chama `git worktree add` e fabrica outros backpointers. Implementação da escrita operacional pelo Git: **[nao verificado]**. (`bin/tests/test_codex_pergunta.sh:87,257-258`) | Wrapper resolve `_GD`, lê inicialmente e relê o caminho armazenado antes da chamada. (`bin/codex_pergunta.sh:115-117,274`) |
   | `WT/.git`, HEAD, WT/app e pausa | Selo reaponta `.git` e troca `app`; `pausar.sh` publica a pausa por temporário/rename. Escritores concorrentes operacionais: **[nao verificado]**. (`bin/tests/test_codex_pergunta.sh:181,191`; `bin/pausar.sh:39-46`) | Validação inicial, revalidação e controles finais de HEAD/`.git`. (`bin/codex_pergunta.sh:83,102-128,271-275,316,327-328,341`) |
   | `$T/captura`, `$T/argv`, `$T/saida` | Codex falso grava captura/argv; harness redireciona a saída. O delta acrescenta remoção de captura/argv em `c_absoluto`. (`bin/tests/test_codex_pergunta.sh:70-77,91-94,146-148`) | Asserções de chamada, argumentos e sincronização por `slot=`. (`bin/tests/test_codex_pergunta.sh:99-101,189-190,216-228`) |
   | `solto_depois.txt`, migration de `ponto`, model de `escala` | Ações artificiais do selo. (`bin/tests/test_codex_pergunta.sh:198,243-244`) | Status anterior à chamada ou inventário final, conforme o caso. (`bin/codex_pergunta.sh:273,302-303,320-335`) |

## 06/11 12:26 · sessao 152591db75ae · R1-CAUSAD-V3 · sonda · origem=Code

As seis linhas `SOMA` da parte 4 estão `True`. Não há ocorrência de `INCOMPLETO` no dado fornecido. [nao verificado]

Saída integral fornecida [nao verificado]:

```text
sonda_leitura: script.py [22b7b39d0d46bdd1] no cpuset 4-7, lendo sombra como leitor (LEITURA)

PARTE 4 PARTES 6
[0] UNIVERSO 10/2026 624
UNIVERSO_PARTE 10/2026 96
JANELA 10/2026 2026-09-21 2026-10-20
[0] UNIVERSO 11/2026 578
UNIVERSO_PARTE 11/2026 91
JANELA 11/2026 2026-10-21 2026-11-20
ESTIMATIVA processados 25 universo 187 minutos_total 2.7 ate_40_min True ate_15_min True
ESTIMATIVA processados 50 universo 187 minutos_total 2.7 ate_40_min True ate_15_min True
ESTIMATIVA processados 75 universo 187 minutos_total 2.7 ate_40_min True ate_15_min True
COMPETENCIA 10/2026
[1] {'dias_comparados': 1235, 'bate_ate_10min': 1187, 'esp_zero_e6_trabalho': 6, 'e6_zero_esp_trabalho': 1, 'diverge_10_60': 5, 'diverge_acima_60': 36, 'dia_batida_impar': 56, 'impar_que_BATE': 22, 'impar_que_DIVERGE': 34, 'impar_dentro_da_faixa': 30, 'impar_FORA_da_faixa': 4, 'impar_no_conjunto_do_dia': 33, 'impar_SO_POR_CORTE_do_oraculo': 1, 'dia_sem_linha_no_espelho': 0, 'dia_sem_lavratura_no_espelho': 1285, 'dia_isento_ou_ausencia': 305, 'dia_sem_trabalho_ambos': 18, 'esp_desconta_janela_declarada': 37, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 96.1% de 1235 dias comparados
[3] ESTRUTURA     3 dia-colab      15.6 h    3 colabs
[3] CADASTRO     23 dia-colab     107.6 h    8 colabs
[3] BATIDA       22 dia-colab     101.2 h   17 colabs
[4] TOTAL divergentes 48
[5] colabs com divergencia: 26
erros_no_espelho 0
PARCIAL|10/2026|ESTRUTURA|dias=3|horas=15.566319135833336|colabs=3
PARCIAL|10/2026|CADASTRO|dias=23|horas=107.60888681222221|colabs=8
PARCIAL|10/2026|BATIDA|dias=22|horas=101.15787729194444|colabs=17
PARCIAL_TOTAL|10/2026|dias=48|horas=224.333083239999986|colabs=26
ESTIMATIVA processados 100 universo 187 minutos_total 2.7 ate_40_min True ate_15_min True
ESTIMATIVA processados 125 universo 187 minutos_total 2.8 ate_40_min True ate_15_min True
ESTIMATIVA processados 150 universo 187 minutos_total 2.9 ate_40_min True ate_15_min True
ESTIMATIVA processados 175 universo 187 minutos_total 2.9 ate_40_min True ate_15_min True
COMPETENCIA 11/2026
[1] {'dias_comparados': 610, 'bate_ate_10min': 596, 'esp_zero_e6_trabalho': 4, 'e6_zero_esp_trabalho': 0, 'diverge_10_60': 0, 'diverge_acima_60': 10, 'dia_batida_impar': 32, 'impar_que_BATE': 22, 'impar_que_DIVERGE': 10, 'impar_dentro_da_faixa': 9, 'impar_FORA_da_faixa': 1, 'impar_no_conjunto_do_dia': 10, 'impar_SO_POR_CORTE_do_oraculo': 0, 'dia_sem_linha_no_espelho': 1274, 'dia_sem_lavratura_no_espelho': 756, 'dia_isento_ou_ausencia': 148, 'dia_sem_trabalho_ambos': 19, 'esp_desconta_janela_declarada': 14, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 97.7% de 610 dias comparados
[3] ESTRUTURA     0 dia-colab       0.0 h    0 colabs
[3] CADASTRO      5 dia-colab      39.7 h    3 colabs
[3] BATIDA        9 dia-colab      50.7 h    8 colabs
[4] TOTAL divergentes 14
[5] colabs com divergencia: 10
erros_no_espelho 0
PARCIAL|11/2026|ESTRUTURA|dias=0|horas=0.0|colabs=0
PARCIAL|11/2026|CADASTRO|dias=5|horas=39.726189220555554|colabs=3
PARCIAL|11/2026|BATIDA|dias=9|horas=50.69357468888889|colabs=8
PARCIAL_TOTAL|11/2026|dias=14|horas=90.419763909444444|colabs=10
TEMPO_SEGUNDOS 174.9
TRATADA_LOCAL app/escala/alimentacao.py:279 GeneratorExit 220720
TRATADA_LOCAL app/escala/models.py:1282 GeneratorExit 551
TRATADA_LOCAL app/escala/services/fase_12x36.py:114 GeneratorExit 1090
TRATADA_LOCAL app/escala/services/leitor_celula.py:25 ValueError 4668
TRATADA_LOCAL app/escala/services/leitor_celula.py:81 GeneratorExit 49
TRATADA_LOCAL app/escala/utils.py:1521 GeneratorExit 1
TRATADA_LOCAL app/escala/utils.py:1708 GeneratorExit 34
TRATADA_LOCAL app/escala/utils.py:2012 GeneratorExit 6712
TRATADA_LOCAL app/escala/utils.py:2054 GeneratorExit 8
TRATADA_LOCAL app/escala/utils.py:452 GeneratorExit 3
TRATADA_LOCAL app/escala/utils.py:524 GeneratorExit 3792
TRATADA_LOCAL app/escala/utils.py:525 GeneratorExit 3792
TRATADA_LOCAL app/escala/utils.py:888 GeneratorExit 284
TRATADA_LOCAL app/folha/causas.py:58 GeneratorExit 168
TRATADA_LOCAL app/ponto/juiz_batida.py:191 GeneratorExit 1126
TRATADA_LOCAL app/ponto/juiz_batida.py:75 ValueError 409650
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:1131 GeneratorExit 558
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:2604 GeneratorExit 2635
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:677 GeneratorExit 165
TRATADA_LOCAL app/ponto/services/dia_pago.py:890 GeneratorExit 11428
TRATADA_LOCAL app/ponto/services/espelho.py:473 GeneratorExit 89
TRATADA_LOCAL app/ponto/services/espelho.py:657 GeneratorExit 3666
TRATADA_LOCAL app/ponto/services/fechamento.py:1028 GeneratorExit 177
TRATADA_LOCAL app/ponto/turnos.py:1293 GeneratorExit 23759
TRATADA_LOCAL app/ponto/turnos.py:1654 GeneratorExit 3165
TRATADA_LOCAL app/ponto/turnos.py:1697 GeneratorExit 37032
TRATADA_LOCAL app/ponto/turnos.py:677 GeneratorExit 17427
SOMA 10/2026 48   <- tem de ser igual a soma das classes (48) True
SOMA 10/2026 HORAS 224.333083239999986 True
SOMA 10/2026 COLABS_UNIAO 26 True
SOMA 11/2026 14   <- tem de ser igual a soma das classes (14) True
SOMA 11/2026 HORAS 90.419763909444444 True
SOMA 11/2026 COLABS_UNIAO 10 True
rc_sombra=0
```

## 06/11 12:22 · sessao 62301c1ca858 · R1-CAUSAD-V3 · sonda · origem=Code

Transcrição integral da saída fornecida [nao verificado]:

```text
sonda_leitura: script.py [c3a2fcb8e125469d] no cpuset 4-7, lendo sombra como leitor (LEITURA)

PARTE 3 PARTES 6
[0] UNIVERSO 10/2026 624
UNIVERSO_PARTE 10/2026 102
JANELA 10/2026 2026-09-21 2026-10-20
[0] UNIVERSO 11/2026 578
UNIVERSO_PARTE 11/2026 97
JANELA 11/2026 2026-10-21 2026-11-20
ESTIMATIVA processados 25 universo 199 minutos_total 3.2 ate_40_min True ate_15_min True
ESTIMATIVA processados 50 universo 199 minutos_total 2.9 ate_40_min True ate_15_min True
ESTIMATIVA processados 75 universo 199 minutos_total 2.8 ate_40_min True ate_15_min True
ESTIMATIVA processados 100 universo 199 minutos_total 2.8 ate_40_min True ate_15_min True
COMPETENCIA 10/2026
[1] {'dias_comparados': 1232, 'bate_ate_10min': 1165, 'esp_zero_e6_trabalho': 12, 'e6_zero_esp_trabalho': 6, 'diverge_10_60': 2, 'diverge_acima_60': 47, 'dia_batida_impar': 66, 'impar_que_BATE': 24, 'impar_que_DIVERGE': 42, 'impar_dentro_da_faixa': 33, 'impar_FORA_da_faixa': 9, 'impar_no_conjunto_do_dia': 42, 'impar_SO_POR_CORTE_do_oraculo': 0, 'dia_sem_linha_no_espelho': 0, 'dia_sem_lavratura_no_espelho': 1422, 'dia_isento_ou_ausencia': 345, 'dia_sem_trabalho_ambos': 22, 'esp_desconta_janela_declarada': 39, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 94.6% de 1232 dias comparados
[3] ESTRUTURA     3 dia-colab      15.2 h    3 colabs
[3] CADASTRO      7 dia-colab      35.9 h    4 colabs
[3] BATIDA       57 dia-colab     302.4 h   25 colabs
[4] TOTAL divergentes 67
[5] colabs com divergencia: 30
erros_no_espelho 0
PARCIAL|10/2026|ESTRUTURA|dias=3|horas=15.209666666666665|colabs=3
PARCIAL|10/2026|CADASTRO|dias=7|horas=35.88209174|colabs=4
PARCIAL|10/2026|BATIDA|dias=57|horas=302.3993460441667|colabs=25
PARCIAL_TOTAL|10/2026|dias=67|horas=353.491104450833365|colabs=30
ESTIMATIVA processados 125 universo 199 minutos_total 2.9 ate_40_min True ate_15_min True
ESTIMATIVA processados 150 universo 199 minutos_total 3.0 ate_40_min True ate_15_min True
ESTIMATIVA processados 175 universo 199 minutos_total 3.0 ate_40_min True ate_15_min True
COMPETENCIA 11/2026
[1] {'dias_comparados': 629, 'bate_ate_10min': 599, 'esp_zero_e6_trabalho': 4, 'e6_zero_esp_trabalho': 5, 'diverge_10_60': 0, 'diverge_acima_60': 21, 'dia_batida_impar': 42, 'impar_que_BATE': 22, 'impar_que_DIVERGE': 20, 'impar_dentro_da_faixa': 17, 'impar_FORA_da_faixa': 3, 'impar_no_conjunto_do_dia': 19, 'impar_SO_POR_CORTE_do_oraculo': 1, 'dia_sem_linha_no_espelho': 1358, 'dia_sem_lavratura_no_espelho': 814, 'dia_isento_ou_ausencia': 147, 'dia_sem_trabalho_ambos': 21, 'esp_desconta_janela_declarada': 38, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 95.2% de 629 dias comparados
[3] ESTRUTURA     3 dia-colab      11.8 h    2 colabs
[3] CADASTRO      5 dia-colab      15.8 h    5 colabs
[3] BATIDA       22 dia-colab     113.4 h   10 colabs
[4] TOTAL divergentes 30
[5] colabs com divergencia: 15
erros_no_espelho 0
PARCIAL|11/2026|ESTRUTURA|dias=3|horas=11.75|colabs=2
PARCIAL|11/2026|CADASTRO|dias=5|horas=15.826795833333335|colabs=5
PARCIAL|11/2026|BATIDA|dias=22|horas=113.35641307083331|colabs=10
PARCIAL_TOTAL|11/2026|dias=30|horas=140.933208904166645|colabs=15
TEMPO_SEGUNDOS 180.0
TRATADA_LOCAL app/escala/alimentacao.py:279 GeneratorExit 240100
TRATADA_LOCAL app/escala/models.py:1282 GeneratorExit 250
TRATADA_LOCAL app/escala/services/fase_12x36.py:114 GeneratorExit 1365
TRATADA_LOCAL app/escala/services/leitor_celula.py:25 ValueError 4623
TRATADA_LOCAL app/escala/services/leitor_celula.py:81 GeneratorExit 41
TRATADA_LOCAL app/escala/utils.py:1521 GeneratorExit 4
TRATADA_LOCAL app/escala/utils.py:1708 GeneratorExit 2
TRATADA_LOCAL app/escala/utils.py:2012 GeneratorExit 6434
TRATADA_LOCAL app/escala/utils.py:2054 GeneratorExit 4
TRATADA_LOCAL app/escala/utils.py:452 GeneratorExit 6
TRATADA_LOCAL app/escala/utils.py:524 GeneratorExit 4324
TRATADA_LOCAL app/escala/utils.py:525 GeneratorExit 4324
TRATADA_LOCAL app/escala/utils.py:888 GeneratorExit 512
TRATADA_LOCAL app/folha/causas.py:58 GeneratorExit 179
TRATADA_LOCAL app/ponto/juiz_batida.py:191 GeneratorExit 16
TRATADA_LOCAL app/ponto/juiz_batida.py:75 ValueError 452220
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:1131 GeneratorExit 554
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:2604 GeneratorExit 2847
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:677 GeneratorExit 157
TRATADA_LOCAL app/ponto/services/dia_pago.py:890 GeneratorExit 11514
TRATADA_LOCAL app/ponto/services/espelho.py:473 GeneratorExit 101
TRATADA_LOCAL app/ponto/services/espelho.py:657 GeneratorExit 3979
TRATADA_LOCAL app/ponto/services/fechamento.py:1028 GeneratorExit 190
TRATADA_LOCAL app/ponto/services/fechamento.py:1029 _GradeSemCelula 4
TRATADA_LOCAL app/ponto/turnos.py:1293 GeneratorExit 22023
TRATADA_LOCAL app/ponto/turnos.py:1654 GeneratorExit 4399
TRATADA_LOCAL app/ponto/turnos.py:1697 GeneratorExit 36499
TRATADA_LOCAL app/ponto/turnos.py:677 GeneratorExit 16376
SOMA 10/2026 67   <- tem de ser igual a soma das classes (67) True
SOMA 10/2026 HORAS 353.491104450833365 True
SOMA 10/2026 COLABS_UNIAO 30 True
SOMA 11/2026 30   <- tem de ser igual a soma das classes (30) True
SOMA 11/2026 HORAS 140.933208904166645 True
SOMA 11/2026 COLABS_UNIAO 15 True
rc_sombra=0
```

As seis linhas `SOMA` estão como `True` no dado fornecido. [nao verificado]

Não há marcador `INCOMPLETO` na saída fornecida. [nao verificado]

## 06/11 12:18 · sessao 2a0e68fa9623 · R1-CAUSAD-V3 · sonda · origem=Code

As seis linhas `SOMA` estão `True`. Não há `INCOMPLETO` no trecho fornecido. [nao verificado]

Transcrição integral da saída fornecida, parte 2 de 6: [nao verificado]

```text
sonda_leitura: script.py [8f462593598e8306] no cpuset 4-7, lendo sombra como leitor (LEITURA)

PARTE 2 PARTES 6
[0] UNIVERSO 10/2026 624
UNIVERSO_PARTE 10/2026 109
JANELA 10/2026 2026-09-21 2026-10-20
[0] UNIVERSO 11/2026 578
UNIVERSO_PARTE 11/2026 99
JANELA 11/2026 2026-10-21 2026-11-20
ESTIMATIVA processados 25 universo 208 minutos_total 3.1 ate_40_min True ate_15_min True
ESTIMATIVA processados 50 universo 208 minutos_total 3.1 ate_40_min True ate_15_min True
ESTIMATIVA processados 75 universo 208 minutos_total 3.1 ate_40_min True ate_15_min True
ESTIMATIVA processados 100 universo 208 minutos_total 2.9 ate_40_min True ate_15_min True
COMPETENCIA 10/2026
[1] {'dias_comparados': 1267, 'bate_ate_10min': 1226, 'esp_zero_e6_trabalho': 2, 'e6_zero_esp_trabalho': 4, 'diverge_10_60': 7, 'diverge_acima_60': 28, 'dia_batida_impar': 64, 'impar_que_BATE': 34, 'impar_que_DIVERGE': 30, 'impar_dentro_da_faixa': 24, 'impar_FORA_da_faixa': 6, 'impar_no_conjunto_do_dia': 30, 'impar_SO_POR_CORTE_do_oraculo': 0, 'dia_sem_linha_no_espelho': 0, 'dia_sem_lavratura_no_espelho': 1568, 'dia_isento_ou_ausencia': 349, 'dia_sem_trabalho_ambos': 24, 'esp_desconta_janela_declarada': 62, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 96.8% de 1267 dias comparados
[3] ESTRUTURA     5 dia-colab      11.1 h    3 colabs
[3] CADASTRO      8 dia-colab      20.4 h    6 colabs
[3] BATIDA       28 dia-colab     136.0 h   18 colabs
[4] TOTAL divergentes 41
[5] colabs com divergencia: 23
erros_no_espelho 0
PARCIAL|10/2026|ESTRUTURA|dias=5|horas=11.084600277777778|colabs=3
PARCIAL|10/2026|CADASTRO|dias=8|horas=20.397520712222224|colabs=6
PARCIAL|10/2026|BATIDA|dias=28|horas=136.0465618775|colabs=18
PARCIAL_TOTAL|10/2026|dias=41|horas=167.528682867500002|colabs=23
ESTIMATIVA processados 125 universo 208 minutos_total 3.0 ate_40_min True ate_15_min True
ESTIMATIVA processados 150 universo 208 minutos_total 3.1 ate_40_min True ate_15_min True
ESTIMATIVA processados 175 universo 208 minutos_total 3.2 ate_40_min True ate_15_min True
ESTIMATIVA processados 200 universo 208 minutos_total 3.2 ate_40_min True ate_15_min True
COMPETENCIA 11/2026
[1] {'dias_comparados': 675, 'bate_ate_10min': 641, 'esp_zero_e6_trabalho': 3, 'e6_zero_esp_trabalho': 0, 'diverge_10_60': 2, 'diverge_acima_60': 29, 'dia_batida_impar': 52, 'impar_que_BATE': 24, 'impar_que_DIVERGE': 28, 'impar_dentro_da_faixa': 27, 'impar_FORA_da_faixa': 1, 'impar_no_conjunto_do_dia': 28, 'impar_SO_POR_CORTE_do_oraculo': 0, 'dia_sem_linha_no_espelho': 1386, 'dia_sem_lavratura_no_espelho': 834, 'dia_isento_ou_ausencia': 120, 'dia_sem_trabalho_ambos': 17, 'esp_desconta_janela_declarada': 37, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 95.0% de 675 dias comparados
[3] ESTRUTURA     0 dia-colab       0.0 h    0 colabs
[3] CADASTRO     10 dia-colab      28.2 h    7 colabs
[3] BATIDA       24 dia-colab     122.8 h   16 colabs
[4] TOTAL divergentes 34
[5] colabs com divergencia: 21
erros_no_espelho 0
PARCIAL|11/2026|ESTRUTURA|dias=0|horas=0.0|colabs=0
PARCIAL|11/2026|CADASTRO|dias=10|horas=28.19539852888889|colabs=7
PARCIAL|11/2026|BATIDA|dias=24|horas=122.76838809305558|colabs=16
PARCIAL_TOTAL|11/2026|dias=34|horas=150.96378662194447|colabs=21
TEMPO_SEGUNDOS 191.3
TRATADA_LOCAL app/escala/alimentacao.py:279 GeneratorExit 249865
TRATADA_LOCAL app/escala/models.py:1282 GeneratorExit 13
TRATADA_LOCAL app/escala/services/fase_12x36.py:114 GeneratorExit 1341
TRATADA_LOCAL app/escala/services/leitor_celula.py:25 ValueError 5749
TRATADA_LOCAL app/escala/services/leitor_celula.py:81 GeneratorExit 66
TRATADA_LOCAL app/escala/utils.py:1521 GeneratorExit 4
TRATADA_LOCAL app/escala/utils.py:1708 GeneratorExit 8
TRATADA_LOCAL app/escala/utils.py:2012 GeneratorExit 6433
TRATADA_LOCAL app/escala/utils.py:2054 GeneratorExit 2
TRATADA_LOCAL app/escala/utils.py:452 GeneratorExit 6
TRATADA_LOCAL app/escala/utils.py:524 GeneratorExit 4777
TRATADA_LOCAL app/escala/utils.py:525 GeneratorExit 4777
TRATADA_LOCAL app/escala/utils.py:888 GeneratorExit 498
TRATADA_LOCAL app/folha/causas.py:58 GeneratorExit 185
TRATADA_LOCAL app/ponto/juiz_batida.py:75 ValueError 459090
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:1131 GeneratorExit 579
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:2604 GeneratorExit 2775
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:677 GeneratorExit 192
TRATADA_LOCAL app/ponto/services/dia_pago.py:890 GeneratorExit 11736
TRATADA_LOCAL app/ponto/services/espelho.py:473 GeneratorExit 97
TRATADA_LOCAL app/ponto/services/espelho.py:657 GeneratorExit 3584
TRATADA_LOCAL app/ponto/services/fechamento.py:1028 GeneratorExit 197
TRATADA_LOCAL app/ponto/services/fechamento.py:1029 _GradeSemCelula 2
TRATADA_LOCAL app/ponto/turnos.py:1293 GeneratorExit 23442
TRATADA_LOCAL app/ponto/turnos.py:1654 GeneratorExit 5013
TRATADA_LOCAL app/ponto/turnos.py:1697 GeneratorExit 37362
TRATADA_LOCAL app/ponto/turnos.py:677 GeneratorExit 18055
SOMA 10/2026 41   <- tem de ser igual a soma das classes (41) True
SOMA 10/2026 HORAS 167.528682867500002 True
SOMA 10/2026 COLABS_UNIAO 23 True
SOMA 11/2026 34   <- tem de ser igual a soma das classes (34) True
SOMA 11/2026 HORAS 150.96378662194447 True
SOMA 11/2026 COLABS_UNIAO 21 True
rc_sombra=0
```

## 06/11 12:18 · sessao 6c5f6274ee99 · CODEX-CONSTRUTOR-NEGOCIO · verificador · origem=Code

**Parecer: cura parcial; ainda há achados de gravidade ALTA.** O delta fecha exemplos específicos, mas mantém escritas do host sem proteção, inventário limitado ao estado final e prova real que aceita evidências fabricadas pelo próprio construtor. Resultados executados e isolamento efetivo: **[nao verificado]**. (`bin/codex_pergunta.sh:225-236,313-328`; `bin/tests/prova_codex_pergunta_real.sh:24-47,58-95`)

1. **v1 #1 — ALTA — Aberto: arquivos de LOGD continuam seguindo symlinks.** A proteção acrescentada a `.rc.$$` e `.novo.$$` não cobre `slot.gate`, `slot.N`, `chamadas.log`, JSONL/`.err` ou `limite.log`. Um `slot.gate` previamente apontado para uma sentinela ainda causa truncamento pelo wrapper, antes do Codex. Isso alcança também os quatro papéis de leitura. (`bin/codex_pergunta.sh:89-90,168-169,225,231,236,276,280,354`)

2. **v1 #2 — MÉDIA — Fecha o contorno direto de `resposta_nome`.** Agora `.`/`..` e resposta existente não regular são recusados. Permanecem verificações por caminho antes das esperas: trocar posteriormente um diretório ancestral ou transformar o destino em diretório ainda separa a validação da publicação por `mv`. Execução desse contorno: **[nao verificado]**. (`bin/codex_pergunta.sh:84-90,224-252,284`)

3. **v1 #3 — ALTA — Fecha dois LOGDs na mesma raiz; a vaga ainda é contornável.** O caminho fixo elimina o exemplo original, pois LOGDs diferentes disputam o mesmo fd 6. Entretanto, a exclusão é por raiz do **script**, não pelo repositório comum: duas cópias do wrapper em raízes diferentes usam vagas diferentes. (`bin/codex_pergunta.sh:48,82,119-121`)

   **Há também um contorno envolvendo os papéis de leitura:** durante um construtor ativo, uma chamada de leitura pode escolher como resposta `<raiz>/logs/construtor.vaga`. Sua publicação por `mv` substitui o inode que o construtor mantém travado; uma chamada seguinte abre o novo inode e obtém outra vaga. Não existe reserva desse nome. Essa sequência decorre do fluxo do código; execução: **[nao verificado]**. (`bin/codex_pergunta.sh:74-90,119-121,279-284`)

   **MÉDIA — Falha de abertura não é tratada:** `exec 6>>` não verifica retorno. Se falhar e houver fd 6 herdado, o `flock` pode operar esse descriptor anterior. Sem descriptor herdado, a tentativa tende a recusar no `flock`. Execução: **[nao verificado]**. (`bin/codex_pergunta.sh:46,119-121`)

4. **v1 #4 — ALTA — `revalida` fecha somente parte da janela.** A troca de `app` por symlink durante a espera e uma pausa criada nesse intervalo agora são recusadas. Porém não se repetem limpeza, registro, `common-dir` ou referência inversa `gitdir`; tampouco se compara o inode de WT/app. Sujar a WT ou substituir `app` por outro diretório real canônico passa pelos novos testes de identidade. Ainda existe intervalo entre a última conferência e o uso de `-C`. Escrita indevida efetiva pelo sandbox: **[nao verificado]**. (`bin/codex_pergunta.sh:100-125,268-276`)

   A corrida do lock da resposta também continua: teste de symlink seguido de abertura comum em append. O leitor da triagem usa `O_NOFOLLOW`; o escritor não. (`bin/codex_pergunta.sh:250-252`; `bin/disjuntor.py:779-789`)

5. **v1 #5 — ALTA — “Mexeu e restaurou” continua passando pelo inventário.** Diff, status, HEAD e checksum de `.git` observam o estado final. Restaurar os bytes de um proibido sem commit não deixa evidência nesses controles. O caso `commit_desfaz` continua testando commit seguido de revert, que altera HEAD. (`bin/codex_pergunta.sh:311-328,336`; `bin/tests/test_codex_pergunta.sh:248`)

   **MÉDIA — `p_link`/`p_hardlink` fecham links novos que permanecem no conjunto tocado.** Não inspecionam links limpos preexistentes nem registram links criados e removidos durante a chamada. A limpeza inicial também não verifica compartilhamento de inode com a viva. (`bin/codex_pergunta.sh:123-125,313-328`)

   **ALTA — Contorno residual por hardlink:** um arquivo permitido pode começar com bytes iguais ao commit e inode compartilhado com a viva. Depois de escrever nele, substituir o nome da WT por uma cópia regular deixa somente alteração permitida com `nlink=1` no inventário final. O código não exclui essa sequência; efeito real sobre a viva pelo sandbox: **[nao verificado]**. (`bin/codex_pergunta.sh:123-125,324-328`)

6. **v1 #6 — MÉDIA — `gitdir_volta` fecha o reapontamento simples para outra WT.** A entrada agora precisa estar diretamente sob `common-dir/worktrees`, e seu `gitdir` precisa retornar a `WT/.git`. Contudo, essa referência inversa não é revalidada antes da chamada. Também há uma janela entre sua conferência e a captura de `GITF_ANTES`: um reapontamento nesse intervalo pode tornar-se a referência aceita pelo checksum posterior. Execução: **[nao verificado]**. (`bin/codex_pergunta.sh:112-125,268-269`)

7. **v1 #7 — ALTA — A prova real melhorou, mas ainda admite verde sem executar as tentativas.** Agora exige `WRC=0`, todos os `.out` não vazios com `rc=`, assinaturas de negação em parte deles e controles externos de Docker/socket/TCP. Isso fecha os exemplos antigos de arquivos vazios e retorno ignorado. (`bin/tests/prova_codex_pergunta_real.sh:56-72`)

   **O problema de autenticidade permanece:** o bloco é entregue como texto ao Codex, e os `.out` ficam na área que ele escreve. Criar somente `INTRUSO_WT.txt` e os doze arquivos com assinatura de negação e `rc=` satisfaz as verificações sem realizar os acessos. Com controles externos disponíveis, alvos ausentes e Git/processos intactos, o fluxo pode terminar verde. Execução desse contorno: **[nao verificado]**. (`bin/tests/prova_codex_pergunta_real.sh:24-47,58-100`)

   **MÉDIA — Mesmo com execução honesta, sobram provas incompletas:** socket dispensa assinatura do sandbox e aceita erro não zero; falha transitória pode parecer bloqueio. Sobrevivência aceita arquivo com `rc=` e ausência posterior do processo, sem confirmar que `sleep` chegou a iniciar; o `echo lancado` pode retornar zero apesar da falha do lançamento. Banco e integridade completa dos metadados Git continuam **[nao verificado]**. (`bin/tests/prova_codex_pergunta_real.sh:35-43,60-72,78-95`)

8. **v1 #8 — MÉDIA — Cobertura ampliada, placar ainda exagerado.** `inv_falha` agora tem caso que força erro de Git, exige rc 3/`FALHA`/ausência de `.id` e possui mutante correspondente. `inherit=core` entrou no argv exigido; links ganharam casos diretos. O tratamento produtivo de falha de inventário já existia: o delta acrescentou cobertura. (`bin/tests/test_codex_pergunta.sh:202-204,250-256,294-296`; `bin/codex_pergunta.sh:318-320,335-348`)

   Permanecem mortes por mensagem ou guarda redundante: retirado `absoluto`, o nome ainda recusa; retirado `inv_diff`, HEAD ainda recusa o commit. Qualquer `FALHA` conta como morte. Migrations/models continuam exemplificados somente em `folha`, permitindo especialização indevida dos matchers sem contradizer esses casos. (`bin/tests/test_codex_pergunta.sh:98-101,145-149,227-228,238,322,344-346`; `bin/codex_pergunta.sh:93-98,322-323`)

9. **v1 #9 — MÉDIA — O produtor absoluto fecha a incompatibilidade sintática; os demais limites permanecem.** O hook aceita caminho absoluto e verifica sua correspondência com o processo. Mas o wrapper apenas imprime no stderr; transporte dessa linha ao topo do RELATO: **[nao verificado]**. Quando há PID e arquivo, ambos precisam passar, e a primeira observação do JSONL não comprova avanço. (`bin/codex_pergunta.sh:260`; `bin/hook_stop_fila1.py:515-528,640-641,655-680`)

   `.rc` continua ausente nas recusas anteriores ao trap e pode conter `chamada=-` nas saídas posteriores ao início, mas anteriores à atribuição de JSONL. Leitor operacional concreto desse sinal: **[nao verificado]**. (`bin/codex_pergunta.sh:73-125,128-145,169,257`)

10. **v1 #10 — BAIXA — Aberto.** Pergunta só precisa ser legível; cabeçalho e dados continuam usando `mktemp` fora do confinamento anunciado para saídas. A raspagem da pergunta ainda pode chamar `relato_nomes.sh`, cujo código usa Docker e consulta colaboradores. Isso é efeito do wrapper, anterior ao sandbox do construtor. (`bin/codex_pergunta.sh:61,148-169,199`; `bin/relato_nomes.sh:7-21`)

Os casos novos do selo têm estes limites adicionais:

- **MÉDIA — `c_revalida` é racy:** após aproximadamente dez segundos, troca `app` mesmo sem confirmar que encontrou `slot=`. Em execução lenta, pode atingir a validação inicial e falhar por outro motivo; essa falha também pode ser contabilizada como mutante morto. (`bin/tests/test_codex_pergunta.sh:184-192,344-346`)
- **MÉDIA — Captura obsoleta pode falsear morte de mutante:** `c_absoluto` não usa `_roda` e não limpa `$T/captura`. O mutante anterior de `lock_symlink` pode deixar captura de uma chamada anterior, produzindo a acusação errada de que o Codex foi chamado. (`bin/tests/test_codex_pergunta.sh:90-101,145-149,298-299`)
- **Os quatro papéis mantêm sandbox, diretório e cabeçalho de leitura.** Não há mudança nesses pontos no delta; continuam expostos aos arquivos de LOGD e ao reuso de WS existente sem autenticação. O selo usa Codex/disjuntor falsos; a prova real de leitura ainda não exige retorno bem-sucedido nem execução das tentativas. (`bin/codex_pergunta.sh:172-181,208-209,279-280`; `bin/tests/test_codex_pergunta.sh:57,67-77,265-275`; `bin/tests/prova_codex_pergunta_real.sh:108-123`)

O mapa dos artefatos e metadados acrescentados ou alterados pelo delta é:

| Arquivo/campo | Quem escreve | Quem lê/usa |
|---|---|---|
| `<raiz>/logs/construtor.vaga` | Wrapper cria/abre em append, fd 6; não preenche conteúdo. (`bin/codex_pergunta.sh:119-120`) | Wrapper disputa `flock`; selo ocupa a vaga e testa outro LOGD. (`bin/codex_pergunta.sh:121`; `bin/tests/test_codex_pergunta.sh:175-178`) |
| `<common-dir>/worktrees/<entrada>/gitdir` | Implementação operacional do Git: **[nao verificado]**. O selo chama `worktree add` e escreve backpointers artificiais. (`bin/tests/test_codex_pergunta.sh:87,241-242`) | Wrapper resolve `--git-dir`, lê o backpointer e compara com `WT/.git`. (`bin/codex_pergunta.sh:112-114`) |
| `ESPERANDO`: produtor absoluto, PID, arquivo | Wrapper imprime no stderr. (`bin/codex_pergunta.sh:260`) | Selo interpreta; hook interpreta quando está no topo do RELATO. (`bin/tests/test_codex_pergunta.sh:211-214`; `bin/hook_stop_fila1.py:655-679`) |
| JSONL apontado por `arquivo=` | Shell redireciona stdout do CLI. (`bin/codex_pergunta.sh:257,276`) | Wrapper lê sessão/erros; hook mede avanço do arquivo declarado. (`bin/codex_pergunta.sh:347,350,361`; `bin/hook_stop_fila1.py:563-642`) |
| Tipo da resposta e symlinks `.rc.$$`/`.novo.$$` | Resposta nova vem do CLI e é publicada pelo wrapper; trap escreve `.rc.$$`. (`bin/codex_pergunta.sh:138-139,276,284`) | Validação inicial consulta tipo/basename/symlink. (`bin/codex_pergunta.sh:87-90`) |
| WT/app, pausa, bytes de `.git`, HEAD na revalidação | Escritores durante a janela operacional: **[nao verificado]**; selo troca `app`. (`bin/tests/test_codex_pergunta.sh:190`) | Nova guarda imediatamente anterior ao CLI. (`bin/codex_pergunta.sh:268-270`) |
| Tipo de link e `st_nlink` dos caminhos tocados | Construtor pode produzir os arquivos; selo cria symlink/hardlink. (`bin/tests/test_codex_pergunta.sh:250-251`) | Python do inventário consulta e classifica; wrapper transforma recusa em rc 3. (`bin/codex_pergunta.sh:326-340,348`) |
| `.rc`: `chamada`, `rc`, `fim` | Trap escreve temporário e renomeia. (`bin/codex_pergunta.sh:138-139`) | Selo interpreta; prova real copia. Leitor operacional: **[nao verificado]**. (`bin/tests/test_codex_pergunta.sh:109,214`; `bin/tests/prova_codex_pergunta_real.sh:50`) |

**BAIXA — O cabeçalho ficou desatualizado:** ainda descreve vaga em `$LOGD` e produtor relativo, embora o comportamento use `<raiz>/logs` e produtor absoluto. (`bin/codex_pergunta.sh:40,44,119-121,260`)

## 06/11 12:16 · sessao e38464c87e8f · - · - · origem=Ronald (tmux)

Auditoria estática do wrapper, sem execução dos testes que gravam arquivos. [nao verificado]

| v1 | Resultado no delta |
|---|---|
| **1 — ALTA** | **Aberto.** A guarda de symlink cobre resposta/sidecars, mas não `slot.gate`, `slot.N`, `chamadas.log`, JSONL/`.err` ou `limite.log`; aberturas continuam seguindo symlinks, algumas com truncamento. (`bin/codex_pergunta.sh:89-90,168-169,225,231,236,276,354`) |
| **2 — MÉDIA** | **Fecha o bypass determinístico descrito:** recusa basename `.`/`..` e resposta existente não regular. Acrescenta verificação dos temporários `.rc.$$`/`.novo.$$`. Continua sendo verificação anterior às esperas, sem abertura segura do diretório ou publicação por descriptor. (`bin/codex_pergunta.sh:87-90,224-252,284`) |
| **3 — ALTA** | **Fecha dois LOGDs na mesma raiz:** ambos disputam `$RL/construtor.vaga`. Não assegura exclusão entre duas cópias do wrapper em raízes distintas: `R` deriva da localização do script, não do repositório Git comum. Também permanece possível substituir o inode da vaga; não há `O_NOFOLLOW`. (`bin/codex_pergunta.sh:48,82,119-121`) |
| **4 — ALTA** | **Parcial.** A troca `app -> symlink` durante a espera e a pausa criada nesse intervalo agora são recusadas. Porém `revalida` não refaz limpeza, registro, common-dir ou referência inversa, não compara inode de WT/app e continua separada do uso do caminho. Trocar `app` por outro diretório real canônico ou sujar a WT durante a espera passa pela nova guarda. Escrita efetiva indevida pelo sandbox: **[nao verificado]**. (`bin/codex_pergunta.sh:100-125,268-276`) |
| **5 — ALTA / MÉDIA** | **“Mexe e restaura” continua aberto:** inventário só observa estado final. **Links novos que permanecem tocados são recusados**, mas links limpos preexistentes não são examinados; não há varredura inicial de inodes compartilhados. (`bin/codex_pergunta.sh:123-125,311-328`) |
| **6 — MÉDIA** | **Fecha `.git` de outra WT simples:** verifica diretório Git em `common/worktrees` e retorno `gitdir -> WT/.git`. Essa referência inversa só é lida na validação inicial; `revalida` compara bytes de `WT/.git` e HEAD, não os metadados apontados. (`bin/codex_pergunta.sh:112-114,268-269`) |

**ALTA — Contorno de `p_hardlink`:** uma WT inicialmente limpa pode conter arquivo permitido compartilhando inode com a viva, com bytes idênticos ao commit. O status inicial não inspeciona `st_nlink`. Após escrever nesse arquivo, basta substituir o nome na WT por uma cópia regular: a checagem final encontra `nlink=1` e classifica somente `TOCADO` permitido. O efeito sobre a viva pelo sandbox é **[nao verificado]**, mas a guarda nova não exclui essa sequência. (`bin/codex_pergunta.sh:123-125,313-328`)

**MÉDIA — Falha ao abrir a vaga não é tratada:** `exec 6>>...` não tem teste de retorno; com fd 6 herdado, uma falha de abertura pode deixar `flock -n 6` operando o descriptor anterior. Exemplo de precondição: `construtor.vaga` é diretório, portanto passa `! -L`, mas falha no open. Exploração integral **[nao verificado]**. (`bin/codex_pergunta.sh:46,119-121`)

**`inv_falha` fecha a lacuna específica do selo:** agora existe caso que força erro de Git apagando HEAD e exige rc 3, `FALHA` e ausência de `.id`; há mutante `exit(1)->exit(0)`. O tratamento produtivo de inventário não mudou: qualquer `_IRC != 0` provoca recusa. (`bin/tests/test_codex_pergunta.sh:252-256,296`; `bin/codex_pergunta.sh:318-320,335-348`)

**Quatro papéis de leitura:** o delta mantém a seleção do ramo `read-only`, `-C "$WS"` e o cabeçalho de leitura; as guardas novas ficam no ramo construtor. A vulnerabilidade comum dos arquivos de LOGD continua alcançando os quatro papéis. (`bin/codex_pergunta.sh:74,208-209,225-251,267-280`)

## 06/11 12:16 · sessao 59aba8de6f2d · - · - · origem=Ronald (tmux)

**ALTA — v1 #7 fecha os `.out` vazios, mas ainda permite verde sem executar as tentativas.** O bloco de comandos entra apenas como texto no prompt; o verificador confia em arquivos produzidos dentro de `app/`. Um Codex que crie somente o controle positivo e os 12 `.out` com assinatura de negação e `rc=` satisfaz as novas verificações, mantendo alvos ausentes, Git intacto e nenhum processo sobrevivente; com controles externos disponíveis e WRC 0, o fluxo termina verde. Execução desse contorno: **[nao verificado]**. (`bin/tests/prova_codex_pergunta_real.sh:24-47,56-80,81-100`)

**MÉDIA — Mesmo com execução honesta, socket e sobrevivência ainda têm prova incompleta.** O socket dispensa assinatura do sandbox e aceita erro não zero; indisponibilidade transitória dentro seguida de sucesso no controle externo pode passar. Sobrevivência exige apenas arquivo com `rc=` e ausência posterior do processo: o lançamento em segundo plano seguido de `echo lancado` pode retornar zero mesmo se `setsid`/`nohup`/`sleep` falhar. (`bin/tests/prova_codex_pergunta_real.sh:40,43,60-72,85-95`)

**MÉDIA — Novo `c_revalida` tem corrida de sincronização.** Após aproximadamente dez segundos de polling, o teste troca `app` mesmo se não encontrou `slot=`; uma execução lenta pode receber a troca antes da validação inicial, produzindo recusa por outro motivo. O teste marca qualquer `FALHA` do caso como mutante morto. O polling que aguarda o holder também não tem deadline. (`bin/tests/test_codex_pergunta.sh:184-192,344-346`)

**MÉDIA — O placar continua aceitando mortes sem demonstrar perda da proteção.** `c_absoluto` continua recusado pela guarda de nome quando se neutraliza a guarda de absoluto; a mudança do motivo já gera `FALHA`. Além disso, esse caso não passa por `_roda`, portanto não limpa `$T/captura`: no loop de mutantes, o caso anterior `lock_symlink` pode deixar uma captura antiga e produzir outra falsa causa de morte. `inv_diff` continua usando commit, que também ativa a guarda HEAD. (`bin/tests/test_codex_pergunta.sh:90-101,145-149,238,298-299,322,344-346`; `bin/codex_pergunta.sh:93-98,322-323`)

**MÉDIA — A cobertura de “qualquer app” continua limitada a `folha`.** Restringir os matchers de migrations/models a `app/folha/` continua compatível com os únicos exemplos presentes. Execução desses mutantes: **[nao verificado]**. (`bin/tests/test_codex_pergunta.sh:227-228,312-313`)

As curas novas **efetivamente acrescentam cobertura direta**, por inspeção: `p_link`, `p_hardlink` e `inv_falha` possuem casos que atingem suas guardas; `inherit=core` passou a ser exigido no argv positivo. Resultado executado desses testes: **[nao verificado]**. (`bin/tests/test_codex_pergunta.sh:202-204,250-256,294-296`; `bin/codex_pergunta.sh:318-328`)

A prova real dos quatro papéis de leitura segue com a lacuna anterior: as chamadas não têm retorno obrigatório nem controle positivo de execução; igualdade dos hashes/status e ausência das sentinelas bastam para verde. (`bin/tests/prova_codex_pergunta_real.sh:108-123`)

O teste dos quatro papéis confere argv, diretório e cabeçalho, mas usa Codex falso e disjuntor falso; comprovação do sandbox e da porta B3 reais permanece **[nao verificado]**. (`bin/tests/test_codex_pergunta.sh:57,67-77,265-275`)

## 06/11 12:15 · sessao a9594c7a6826 · - · - · origem=Ronald (tmux)

Mapa dos novos artefatos/campos:

| Artefato/campo | Escritor | Leitor/consumidor |
|---|---|---|
| `<raiz>/logs/construtor.vaga` | Wrapper cria/abre append no fd 6; conteúdo não é preenchido. (`bin/codex_pergunta.sh:119-120`) | Wrapper disputa exclusividade com `flock -n 6`. O selo abre e prende a mesma vaga e verifica recusa também com outro LOGD. (`bin/codex_pergunta.sh:121`; `bin/tests/test_codex_pergunta.sh:175-178`) |
| `<common-dir>/worktrees/<entrada>/gitdir` | Escritor operacional é o Git externo: implementação concreta nesta cópia **[nao verificado]**. O selo chama `git worktree add` e também escreve backpointers artificiais diretamente. (`bin/tests/test_codex_pergunta.sh:87,241`) | Wrapper resolve `--git-dir`, exige entrada diretamente sob `common-dir/worktrees`, lê `gitdir` e compara seu caminho resolvido com `$WT/.git`. (`bin/codex_pergunta.sh:112-114`) |
| Linha `ESPERANDO`, produtor absoluto, `pid`, `arquivo` | Wrapper imprime no stderr. (`bin/codex_pergunta.sh:260`) | Selo extrai a linha, verifica existência do JSONL e identidade do `.rc`. (`bin/tests/test_codex_pergunta.sh:211-214`) O hook consome a linha **do topo do RELATO**, valida produtor/PID e avanço do arquivo; transporte automático stderr → RELATO **[nao verificado]**. (`bin/hook_stop_fila1.py:429,655-679`) |
| JSONL indicado em `arquivo=` | Shell redireciona stdout do Codex para o nome calculado pelo wrapper. (`bin/codex_pergunta.sh:257,276`) | Hook mede mtime e inode quando a linha está no topo; wrapper lê sessão e erros. (`bin/hook_stop_fila1.py:563-642`; `bin/codex_pergunta.sh:347,350,361`) |
| `.rc`: `chamada`, `rc`, `fim` | Trap escreve temporário e renomeia. (`bin/codex_pergunta.sh:138-139`) | Selo interpreta identidade/status; prova real somente copia. Leitor operacional concreto **[nao verificado]**. (`bin/tests/test_codex_pergunta.sh:109,214`; `bin/tests/prova_codex_pergunta_real.sh:50`) |

**BAIXA — Documentação interna ficou divergente:** cabeçalho ainda anuncia vaga em `$LOGD/construtor.vaga` e produtor relativo, enquanto o comportamento agora usa `<raiz>/logs/construtor.vaga` e produtor absoluto. (`bin/codex_pergunta.sh:40,44,119-121,260`)

**O produtor absoluto fecha a incompatibilidade sintática apontada na v1**, pois o hook exige absoluto quando há `/` e resolve o caminho completo para conferir o processo. Isso não demonstra integração operacional executada. (`bin/codex_pergunta.sh:260`; `bin/hook_stop_fila1.py:515-528`) Quando PID e arquivo estão presentes, ambos devem passar; na primeira observação do arquivo, o hook retorna “arquivo nao andou”. (`bin/hook_stop_fila1.py:640-641,675-680`)

**Quatro papéis de leitura:** as novas validações e revalidação ficam no ramo construtor; leitura/censo/verificador/sonda continuam com chamada `read-only -C "$WS"` e cabeçalho de leitura. Regressão efetivamente executada **[nao verificado]**. (`bin/codex_pergunta.sh:74,203-210,267-280`) O selo cobre argv, diretório, cabeçalho e ausência de `.rc`/inventário nos quatro papéis, mas usa CLI falso. (`bin/tests/test_codex_pergunta.sh:265-275`)

## 06/11 12:14 · sessao 35fcd9d14771 · O10-B · verificador · origem=Code

**As curas fecham 3a e 4b por inspeção do código:** a guarda consulta a empresa no banco e as duas edições seguem `(colaborador_id, data)` (`app/ponto/portas/celula.py:517`, `app/ponto/portas/celula.py:567`, `app/ponto/portas/celula.py:668`). Prova em PostgreSQL e execução dos testes: [nao verificado].

| Item | Verificação | procedencia sugerida |
|---|---|---|
| **1. Curas 3a/4b** | **nenhum achado remanescente nesses dois caminhos.** As cartas são travadas por PK, somente em `self`; as células passam pela ordenação global (`app/ponto/portas/celula.py:652`, `app/ponto/portas/celula.py:668`, `app/ponto/portas/celula.py:593`). As guardas de regeneração, edição e reversão usam `_empresa_do_colab`, assim como a frase da regeneração (`app/ponto/portas/celula.py:214`, `app/ponto/portas/celula.py:325`, `app/ponto/portas/celula.py:567`, `app/ponto/portas/celula.py:775`). Ainda há empresa da instância no horizonte e no recálculo, fora dessas guardas (`app/ponto/portas/celula.py:190`, `app/ponto/portas/celula.py:449`). | **nada** |
| **2. SQL e trava do Colaborador** | **nenhum defeito identificado na composição ORM:** ela pede o relacionamento carregado e restringe a trava à tabela principal; a FK é obrigatória (`app/ponto/portas/celula.py:652`, `app/ponto/models.py:743`). SQL esperado: `JOIN ... FOR UPDATE OF <tabela/alias de Ausencia>`; SQL efetivamente compilado e aceitação pelo PostgreSQL: **[nao verificado]**. Nenhuma dependência explícita da trava de `Colaborador` foi encontrada: a edição usa PK, consulta novamente empresa e carrega vínculo; o restante julga, agenda recálculo e recarrega o colaborador para rejunção (`app/ponto/portas/celula.py:566`, `app/ponto/portas/celula.py:584`, `app/ponto/portas/celula.py:434`, `app/ponto/portas/celula.py:458`, `app/ponto/portas/celula.py:498`). | **nada** |
| **3. Efeitos observáveis da ordem** | **Há mudança no diagnóstico:** quando ambos recusariam, somente o primeiro na ordem global é consultado; só sua mensagem chega a `barrados`, mantendo `'recusada'` (`app/ponto/portas/celula.py:670`, `app/ponto/portas/celula.py:672`, `app/ponto/portas/celula.py:693`). Papéis continuam corretos: cedente `False`, substituto `True`, resultados e trilha identificados por papel; dois `'igual'` continuam retornando `'igual'` (`app/ponto/portas/celula.py:668`, `app/ponto/portas/celula.py:673`, `app/ponto/portas/celula.py:685`). Também mudam a sequência das trilhas individuais, dos julgamentos e do registro dos callbacks, pois cada edição executa seu restante antes do outro lado (`app/ponto/portas/celula.py:598`, `app/ponto/portas/celula.py:611`, `app/ponto/portas/celula.py:622`). | **nada** |
| **4. Extração do helper** | **nenhum.** O helper reproduz a consulta inline do diff: `filter(pk=...).values_list('empresa_id', flat=True).first()`, preservando o retorno `None` sem colaborador (`app/ponto/portas/celula.py:513`). As três substituições mantêm seus condicionais, inclusive a consulta separada para a frase da regeneração (`app/ponto/portas/celula.py:211`, `app/ponto/portas/celula.py:321`, `app/ponto/portas/celula.py:774`). | **nada** |
| **5. Descartes promovidos a bloqueantes** | **nenhum.** A criação continua protegida por unicidade e `atomic`; a troca não captura `IntegrityError` (`app/escala/models.py:1833`, `app/ponto/portas/celula.py:592`, `app/ponto/portas/celula.py:600`, `app/ponto/portas/celula.py:689`). A exclusão dos vereditos humanos permanece no helper compartilhado; a falta de trilha continua provocando exceção (`app/ponto/portas/celula.py:427`, `app/ponto/portas/celula.py:619`). | **nada** |

Os `on_commit` continuam sendo registrados dentro do `atomic` externo: por lado editado, recálculo e depois reavaliação; esta executa rejunção antes de falta espúria (`app/ponto/portas/celula.py:647`, `app/ponto/portas/celula.py:458`, `app/ponto/services/fechamento.py:1808`, `app/ponto/portas/celula.py:493`, `app/ponto/portas/celula.py:502`). Execução e descarte real desses callbacks em rollback: [nao verificado].

| Pendência preservada | Fundamentação | procedencia sugerida |
|---|---|---|
| **Prova dos selos** | A11 verifica chamadas sequenciais e papéis, sem concorrência; o auxiliar executa `on_commit` imediatamente (`app/ponto/tests/test_o10b_editar_dia_troca.py:449`, `app/ponto/tests/test_o10b_editar_dia_troca.py:455`, `app/ponto/tests/test_o10b_editar_dia_troca.py:115`). | **fila** |
| **Item 7 — cobrança órfã** | Continua sem cura: edição grava `editada_em`, enquanto o reconhecimento histórico procura regeneração e o supra alimenta somente `regenerada_em` antes da possibilidade de retratação (`app/ponto/portas/celula.py:607`, `app/escala/services/regeneracao.py:57`, `app/ponto/management/commands/supra_juiz.py:167`, `app/ponto/management/commands/supra_juiz.py:275`, `app/ponto/management/commands/supra_juiz.py:279`). Ocorrência concreta em produção: [nao verificado]. | **bloqueia**, mantendo a pendência da volta 1 |

## 06/11 12:14 · sessao b3f1ebe4f1be · - · - · origem=Ronald (tmux)

| Item | Conclusão | procedencia sugerida |
|---|---|---|
| 4 — helper nas três chamadas existentes | **nenhum** desvio funcional: `_empresa_do_colab` reproduz a consulta inline do diff, inclusive `.first()` e ausência → `None`; preserva também a consulta separada da frase, sem cache. Referências: `app/ponto/portas/celula.py:513-517`, chamadas `:214`, `:325-326`, `:775`; comparação com o inline: anexo `v2.diff` [nao verificado quanto ao commit ancestral]. | nada |
| 5 — descartes elevados a bloqueantes | **nenhum** novo. A concorrência de criação continua protegida pela unicidade `(colaborador,data)` (`app/escala/models.py:1833`) dentro do `atomic` (`app/ponto/portas/celula.py:592-601`); `_t is None` continua abortando (`:619-621`); `editada_por` continua atribuído e persistido (`:608-609`). | nada |
| Veredito humano/ata velha | O helper exclui vereditos humanos do rejulgamento (`app/ponto/portas/celula.py:427-434`), e o leitor prefere `ata.tipo_dia`/`ata.minutos_previstos` (`app/escala/services/leitor_celula.py:380-396`). Isso sustenta a limitação já triada, sem cura neste delta. | fila |
| Fragilidade dos selos | A01 verifica nomes de métodos por AST (`app/ponto/tests/test_o10b_editar_dia_troca.py:315-323`); `_trocar` força `on_commit` imediato (`:110-116`), inclusive no teste A02 (`:325-342`). Não prova concorrência PostgreSQL nem descarte real de callbacks por rollback. | fila |
| Leitor `dna_reescrito_em` | Continua expondo apenas `regenerada_em`, sem `editada_em` (`app/ponto/services/dia_do_colab.py:90-93`). | fila |

**Item 7 permanece aberto, separadamente dos descartes.** A edição grava `editada_em`, sem atualizar os campos de regeneração (`app/ponto/portas/celula.py:598-609`; contrato/teste em `app/ponto/tests/test_o10b_editar_dia_troca.py:483-488`). As janelas históricas filtram exclusivamente `acao='regenerar_celulas_vinculo'` (`app/escala/services/regeneracao.py:57-66`), e os alimentadores usam apenas `regenerada_em`: marcação `app/ponto/management/commands/gerar_celulas.py:241-249`, fila `app/escala/services/regeneracao.py:143-146`, supra `app/ponto/management/commands/supra_juiz.py:167-170`.

Assim, uma reescrita humana posterior à cobrança não chega ao juiz por esses insumos; o supra pode ultrapassar a guarda de órfã e chamar a retratação (`app/ponto/management/commands/supra_juiz.py:275-287`; trava global em seco está `False` em `:45`). O texto pode afirmar que a cobrança “nasceu sem lastro” quando os marcos antigos não aparecem mais (`app/ponto/supra_juiz.py:131-138`), enquanto o lookup manda preservar cobrança cujo DNA foi reescrito depois dela (`app/docs/JUIZES.md:41-44`). **Procedência sugerida: bloqueia**, mantendo a pergunta de lei já encaminhada; ocorrência concreta em produção: [nao verificado].

## 06/11 12:13 · sessao 311137dd7a1b · R1-CAUSAD-V3 · leitura · origem=Code

A distinção estrutural está nas linhas **7 e 10 de ambos os scripts abaixo**: compara a identidade dos frames, sem lista de arquivos permitidos. A contagem local é provisória; quando a exceção atravessa um frame, ela sai de `TRATADA_LOCAL` e entra em `ERRO`. Assim, A→B com `except: pass` em B continua contando. (`r1_script2.py:7`, `r1_script2.py:10`; `v3_script2.py:7`, `v3_script2.py:10`, versões abaixo)

O diagnóstico tem orçamento de 192 KiB, até cinco exemplos por grupo e resumo explícito do detalhamento omitido; as contagens continuam completas. O teto da **saída inteira** é [não verificado], pois o V3 mantém suas linhas `DIA`. Propostas sem execução; compatibilidade, duração e resultados de uma nova execução: [não verificado].

```python
def cruzou_frame(frame, tb):
    if tb is None:
        return False
    origem = tb
    while origem.tb_next is not None:
        origem = origem.tb_next
    if frame is not None and frame is not origem.tb_frame:
        return True
    while tb is not None:
        if tb.tb_frame is not origem.tb_frame:
            return True
        tb = tb.tb_next
    return False


class RegistroErros:
    def __init__(self):
        self.marca = object()
        self.total = 0
        self.grupos = {}
        self.locais = {}
        self.por_tipo = {}

    def __len__(self):
        return self.total

    def estado(self, exc):
        estado = exc.__dict__.get("_sonda_estado_estrutural")
        if isinstance(estado, list) and estado[0] is self.marca:
            return estado
        estado = [self.marca, None, False]
        exc.__dict__["_sonda_estado_estrutural"] = estado
        return estado

    def ultimo(self, tb):
        if tb is not None:
            while tb.tb_next is not None:
                tb = tb.tb_next
        return tb

    def dados(self, exc, tb, prefixo):
        tipo = type(exc).__name__
        if not tipo.isascii() or not tipo.isidentifier() or len(tipo) > 80:
            tipo = "Excecao"
        if tb is None:
            return "sonda", tipo
        nome = tb.tb_frame.f_code.co_filename
        if prefixo and nome.startswith(prefixo):
            sitio = "app/" + nome[len(prefixo):]
        else:
            sitio = "[fora-do-app]"
        return "%s:%d" % (sitio, tb.tb_lineno), tipo

    def local(self, exc, tb, prefixo):
        estado = self.estado(exc)
        if estado[1] is not None or estado[2]:
            return
        chave = self.dados(exc, tb, prefixo)
        estado[1] = chave
        self.locais[chave] = self.locais.get(chave, 0) + 1

    def erro(self, exc, comp, col=None, dia=None, tb=None, prefixo=None):
        estado = self.estado(exc)
        if estado[2]:
            return
        # A classificação local era provisória: propagação a revoga.
        if estado[1] is not None:
            chave_local = estado[1]
            self.locais[chave_local] -= 1
            if not self.locais[chave_local]:
                del self.locais[chave_local]
            estado[1] = None
        estado[2] = True
        tb = self.ultimo(tb if tb is not None else exc.__traceback__)
        sitio, tipo = self.dados(exc, tb, prefixo)
        chave = (comp or "NA", sitio, tipo)
        grupo = self.grupos.setdefault(chave, [0, []])
        grupo[0] += 1
        self.total += 1
        self.por_tipo[tipo] = self.por_tipo.get(tipo, 0) + 1

        if isinstance(col, int) and not isinstance(col, bool):
            col = "col%d" % col
        if not (
            isinstance(col, str) and col.startswith("col")
            and col[3:].isascii() and col[3:].isdigit()
        ):
            col = "-"
        dia = str(dia) if dia is not None else "-"
        if not (
            len(dia) == 10 and dia[4] == dia[7] == "-"
            and dia.replace("-", "").isascii()
            and dia.replace("-", "").isdigit()
        ):
            dia = "-"
        exemplo = (col, dia)
        if len(grupo[1]) < 5 and exemplo not in grupo[1]:
            grupo[1].append(exemplo)

    def observar(self, frame, arg, prefixo, comp, contexto):
        exc, tb = arg[1], arg[2]
        estado = exc.__dict__.get("_sonda_estado_estrutural")
        marcada = isinstance(estado, list) and estado[0] is self.marca
        no_app = bool(prefixo and frame.f_code.co_filename.startswith(prefixo))
        if not no_app and not marcada:
            return

        ultimo = self.ultimo(tb)
        if cruzou_frame(frame, tb):
            col, dia = contexto(frame)
            self.erro(exc, comp, col, dia, tb, prefixo)
        elif no_app and ultimo is not None:
            # Só fica local se nenhum evento posterior comprovar propagação.
            self.local(exc, ultimo, prefixo)

    def emitir(self, imprimir):
        # Orçamento exclusivo do diagnóstico; demais saídas ficam intactas.
        limite = 192 * 1024
        usados = 0
        omitidos = {"grupos_erro": 0, "erros": 0,
                    "grupos_locais": 0, "tratadas": 0, "tipos": 0}

        def emitir_linha(texto):
            nonlocal usados
            texto = texto.encode("ascii", "backslashreplace").decode("ascii")
            tamanho = len(texto.encode("ascii")) + 1
            if usados + tamanho > limite - 1024:
                return False
            imprimir(texto)
            usados += tamanho
            return True

        for (comp, sitio, tipo), (n, exemplos) in sorted(self.grupos.items()):
            texto = "ERRO %s %s %s %d exemplos=%r" % (
                comp, sitio, tipo, n, exemplos
            )
            if not emitir_linha(texto):
                omitidos["grupos_erro"] += 1
                omitidos["erros"] += n
        for (sitio, tipo), n in sorted(self.locais.items()):
            if not emitir_linha("TRATADA_LOCAL %s %s %d" % (sitio, tipo, n)):
                omitidos["grupos_locais"] += 1
                omitidos["tratadas"] += n
        for tipo, n in sorted(self.por_tipo.items()):
            if not emitir_linha("ERROS_POR_TIPO %s %d" % (tipo, n)):
                omitidos["tipos"] += 1
        if any(omitidos.values()):
            imprimir("DIAGNOSTICO_OMITIDO_POR_TETO", omitidos)


import ast
import datetime as dt
import gc
import io
import logging
import re
import sys
import time
import tokenize
from collections import Counter
from contextlib import redirect_stderr, redirect_stdout
from decimal import Decimal, localcontext
from pathlib import Path
from unittest.mock import patch

PARTE = 1
PARTES = 6

sys.dont_write_bytecode = True


def sonda_r1():
    # Proposta sem execução; desempenho e compatibilidade: [nao verificado].
    saida = sys.stdout
    inicio = time.monotonic()
    limite = inicio + 15 * 60
    rastreador_anterior = sys.gettrace()
    competencia = "-"
    raiz = None
    prefixo_app = None
    codigo_handle = None
    bruto = None
    chamadas = 0
    universo_parte_total = 0
    registro_erros = RegistroErros()
    erros_total = 0
    erros_por_tipo = registro_erros.por_tipo
    somas = {}
    quantidades = {}
    selecionados = Counter()
    pulsos = 0
    resetar_queries = None
    invalidar_catalogo = None

    DONOS = ("ESTRUTURA", "CADASTRO", "BATIDA")
    CHAVES = (
        "dias_comparados", "bate_ate_10min",
        "esp_zero_e6_trabalho", "e6_zero_esp_trabalho",
        "diverge_10_60", "diverge_acima_60",
        "dia_batida_impar", "impar_que_BATE", "impar_que_DIVERGE",
        "impar_dentro_da_faixa", "impar_FORA_da_faixa",
        "impar_no_conjunto_do_dia", "impar_SO_POR_CORTE_do_oraculo",
        "dia_sem_linha_no_espelho", "dia_sem_lavratura_no_espelho",
        "dia_isento_ou_ausencia", "dia_sem_trabalho_ambos",
        "esp_desconta_janela_declarada", "esp_lido_pelo_pago_h",
    )

    class TempoEsgotado(BaseException):
        pass

    class Descartar:
        # Descarte imediato: não acumula stdout/stderr livres em StringIO.
        def write(self, texto):
            return len(texto)

        def flush(self):
            return None

    def imprimir(*partes):
        print(*partes, file=saida, flush=True)

    def data_segura(valor):
        if isinstance(valor, dt.datetime):
            return valor.date().isoformat()
        if isinstance(valor, dt.date):
            return valor.isoformat()
        if isinstance(valor, str) and re.fullmatch(r"\d{4}-\d{2}-\d{2}", valor):
            return dt.date.fromisoformat(valor).isoformat()
        return None

    def contexto(frame):
        col, dia = None, None
        while frame is not None:
            loc = frame.f_locals
            if col is None:
                objetos = [loc.get("colaborador"), loc.get("colab"), loc.get("c")]
                obj_self = loc.get("self")
                if obj_self is not None:
                    objetos.append(getattr(obj_self, "__dict__", {}).get("c"))
                for obj in objetos:
                    meta = getattr(type(obj), "_meta", None)
                    if getattr(meta, "model_name", None) == "colaborador":
                        pk = obj.pk
                        if isinstance(pk, int) and not isinstance(pk, bool):
                            col = "col%d" % pk
                            break
                if col is None:
                    for chave in ("colaborador_id", "colab_id"):
                        pk = loc.get(chave)
                        if isinstance(pk, int) and not isinstance(pk, bool):
                            col = "col%d" % pk
                            break
            if dia is None:
                # k é o dia julgado; dia já foi incrementado.
                # app/ponto/management/commands/e6_oraculo.py:321
                # app/ponto/management/commands/e6_oraculo.py:323
                for chave in ("k", "data", "dia", "data_turno"):
                    valor = data_segura(loc.get(chave))
                    if valor is not None:
                        dia = valor
                        break
            frame = frame.f_back
        return col or "-", dia or "-"

    def registrar(exc, frame=None):
        nonlocal erros_total
        ultimo = registro_erros.ultimo(exc.__traceback__)
        col, dia = contexto(frame or (ultimo.tb_frame if ultimo else None))
        registro_erros.erro(
            exc, competencia, col, dia, prefixo=prefixo_app
        )
        erros_total = registro_erros.total

    def copiar_acumuladores(frame):
        nonlocal bruto
        loc = frame.f_locals
        necessarios = ("T", "DONO_H", "DONO_C", "div", "_div_tot")
        if not all(chave in loc for chave in necessarios):
            return

        # Só lê o que a autoridade já acumulou; não refaz julgamento.
        # app/ponto/management/commands/e6_oraculo.py:367
        # app/ponto/management/commands/e6_oraculo.py:368
        # app/ponto/management/commands/e6_oraculo.py:369
        t, h, c = loc["T"], loc["DONO_H"], loc["DONO_C"]
        uniao = set()
        for dono in DONOS:
            uniao.update(c.get(dono, ()))
        bruto = {
            "dias": {dono: int(t.get("dono_" + dono, 0)) for dono in DONOS},
            # Mesma divisão da autoridade, sem a formatação %.1f.
            # app/ponto/management/commands/e6_oraculo.py:423
            "horas": {dono: repr(h.get(dono, 0) / 60.0) for dono in DONOS},
            "colabs": {dono: len(c.get(dono, ())) for dono in DONOS},
            "total_dias": int(loc["_div_tot"]),
            # Mesma união usada na saída nativa.
            # app/ponto/management/commands/e6_oraculo.py:429
            "total_colabs": len({x[1] for x in loc["div"]}),
            "uniao_colabs": len(uniao),
        }

    def rastrear(frame, evento, arg):
        nonlocal pulsos, erros_total
        if evento == "call":
            pulsos += 1
            if pulsos % 256 == 0 and time.monotonic() >= limite:
                raise TempoEsgotado()
            # Inclui chamadores externos para observar A(app) -> B(externo).
            # Sem eventos line: não depende de entrar num except específico.
            frame.f_trace_lines = False
            return rastrear
        if evento == "exception":
            registro_erros.observar(
                frame, arg, prefixo_app, competencia, contexto
            )
            erros_total = registro_erros.total
        elif evento == "return" and frame.f_code is codigo_handle:
            copiar_acumuladores(frame)
        return rastrear

    def receber_log(logger, registro):
        nonlocal erros_total
        if registro.exc_info and registro.exc_info[1] is not None:
            exc = registro.exc_info[1]
            tb = registro.exc_info[2]
            ultimo = registro_erros.ultimo(tb)
            origem_app = (
                ultimo is not None and prefixo_app is not None
                and ultimo.tb_frame.f_code.co_filename.startswith(prefixo_app)
            )
            if origem_app and not cruzou_frame(None, tb):
                # logger.exception no próprio frame não transforma
                # uma exceção local em erro da medida.
                registro_erros.local(exc, ultimo, prefixo_app)
            else:
                col, dia = contexto(sys._getframe(1))
                registro_erros.erro(
                    exc, competencia, col, dia, tb, prefixo_app
                )
            erros_total = registro_erros.total

    def limpar():
        if resetar_queries is not None:
            resetar_queries()
        if invalidar_catalogo is not None:
            # Cache único de catálogo; não é cache crescente por colaborador.
            # app/ponto/catalogo/ausencias.py:257
            # app/ponto/catalogo/ausencias.py:261
            invalidar_catalogo()

        # Não limpa memos de motores vivos nem os acumuladores do comando.
        # Os memos pertencem à instância/cálculo:
        # app/ponto/motor_calculo_v2.py:848
        # app/ponto/motor_calculo_v2.py:2285
        # O registro de fontes é fraco e morre com os motores:
        # app/core/regua_cct.py:487
        # app/core/regua_cct.py:494
        gc.collect()

    def ler_relatorio(texto, erros_antes):
        linhas = texto.splitlines()
        padrao_counter = (
            r"\{(?:'[A-Za-z0-9_]+'\s*:\s*\d+"
            r"(?:,\s*'[A-Za-z0-9_]+'\s*:\s*\d+)*)?\}"
        )
        contadores = [
            ast.literal_eval(linha)
            for linha in linhas if re.fullmatch(padrao_counter, linha)
        ]
        donos = [
            m for linha in linhas
            if (m := re.match(
                r"^\s*(ESTRUTURA|CADASTRO|BATIDA)\s+"
                r"(\d+) dia-colab\s+(\d+\.\d+) h\s+(\d+) colabs",
                linha
            ))
        ]
        soma = [
            m for linha in linhas
            if (m := re.match(
                r"^\s*SOMA\s+(\d+)\s+"
                r"<- tem de ser igual a soma das classes \((\d+)\)", linha
            ))
        ]
        colabs = [
            m for linha in linhas
            if (m := re.fullmatch(r"colabs com divergencia: (\d+)", linha))
        ]
        falhas = [
            m for linha in linhas
            if (m := re.fullmatch(r"erros no espelho: (\d+)", linha))
        ]
        if not (
            len(contadores) == len(soma) == len(colabs) == len(falhas) == 1
            and len(donos) == 3
            and {m[1] for m in donos} == set(DONOS)
            and bruto is not None
        ):
            raise RuntimeError("FormatoDaAutoridadeIncompleto")

        soma_dias = sum(bruto["dias"].values())
        confere_dias = (
            soma_dias == bruto["total_dias"]
            == int(soma[0][1]) == int(soma[0][2])
        )
        confere_colabs = (
            bruto["uniao_colabs"] == bruto["total_colabs"] == int(colabs[0][1])
        )
        for m in donos:
            dono = m[1]
            if (
                int(m[2]) != bruto["dias"][dono]
                or int(m[4]) != bruto["colabs"][dono]
                or m[3] != format(float(bruto["horas"][dono]), ".1f")
            ):
                raise RuntimeError("AcumuladoresDaAutoridadeDivergentes")

        if not confere_dias or not confere_colabs:
            raise RuntimeError("SomaDaAutoridadeDivergente")
        if selecionados[competencia] != quantidades[competencia]:
            raise RuntimeError("UniversoDaParteDivergente")
        if int(falhas[0][1]) and erros_total == erros_antes:
            raise RuntimeError("ErroDoEspelhoSemDetalhamento")

        imprimir("COMPETENCIA", competencia)
        t = contadores[0]
        imprimir("[1]", {chave: t.get(chave, 0) for chave in CHAVES})
        for linha in linhas:
            if re.fullmatch(
                r"BATE \(ate 10 min\): \d+\.\d+% de \d+ dias comparados", linha
            ):
                imprimir("[2]", linha)
        for m in donos:
            imprimir("[3]", m[0].strip())
        imprimir("[4] TOTAL divergentes", soma[0][2])
        imprimir("[5]", colabs[0][0])
        imprimir("erros_no_espelho", falhas[0][1])

        # Decimal soma exatamente as representações decimais emitidas.
        # Não usa as horas arredondadas da tabela nativa.
        with localcontext() as ctx:
            ctx.prec = 80
            horas = {dono: Decimal(bruto["horas"][dono]) for dono in DONOS}
            total_horas = sum(horas.values(), Decimal(0))
            texto_total_horas = format(total_horas, "f")
            confere_horas = (
                sum((Decimal(bruto["horas"][d]) for d in DONOS), Decimal(0))
                == Decimal(texto_total_horas)
            )

        for dono in DONOS:
            imprimir(
                "PARCIAL|%s|%s|dias=%d|horas=%s|colabs=%d" % (
                    competencia, dono, bruto["dias"][dono],
                    bruto["horas"][dono], bruto["colabs"][dono],
                )
            )
        imprimir(
            "PARCIAL_TOTAL|%s|dias=%d|horas=%s|colabs=%d" % (
                competencia, bruto["total_dias"],
                texto_total_horas, bruto["total_colabs"],
            )
        )
        somas[competencia] = (
            soma[0][0].strip(), confere_dias,
            texto_total_horas, confere_horas,
            bruto["total_colabs"], confere_colabs,
        )

    try:
        with (
            patch.object(logging.Logger, "handle", receber_log),
            redirect_stdout(Descartar()),
            redirect_stderr(Descartar()),
        ):
            from django.apps import apps
            from django.conf import settings
            from django.core.management import call_command
            from django.db import connection, reset_queries, transaction
            from ponto import janelas
            from ponto.catalogo import ausencias
            from ponto.management.commands import e6_oraculo
            from ponto.services import espelho as modulo_espelho

            raiz = Path(e6_oraculo.__file__).resolve().parents[3]
            prefixo_app = str(raiz) + "/"
            codigo_handle = e6_oraculo.Command.handle.__code__
            resetar_queries = reset_queries
            invalidar_catalogo = ausencias.invalidar_cache

            if (
                type(PARTE) is not int
                or type(PARTES) is not int
                or PARTES != 6
                or not 1 <= PARTE <= PARTES
                or settings.SETTINGS_MODULE != "config.settings.sombra"
                or not settings.BANCO_DE_ENSAIO
                or connection.settings_dict["NAME"] != "sombra"
                or connection.schema_name != "juliani"
                or rastreador_anterior is not None
            ):
                raise RuntimeError("AmbienteIncompativel")

            Empresa = apps.get_model("colaboradores", "Empresa")
            FM = apps.get_model("ponto", "FechamentoMensal")
            Colaborador = apps.get_model("colaboradores", "Colaborador")
            gerente = Colaborador.objects
            filtro_real = gerente.filter
            real_espelho = modulo_espelho.espelho_do_colab

            with transaction.atomic():
                try:
                    with connection.cursor() a

[truncado em 20000 de 59909 caracteres]

## 06/11 12:13 · sessao 9ffa7d957ef9 · - · - · origem=Ronald (tmux)

- **1 — Curas:** nenhum caminho restante por papel dentro de `trocar_plantao`: cartas ordenadas por PK e limitadas a `of=('self',)`; chamadas de `editar_dia` ordenadas por `(colaborador_id, data)`; trava da célula dentro de `editar_dia`. (`app/ponto/portas/celula.py:652`, `app/ponto/portas/celula.py:668`, `app/ponto/portas/celula.py:593`). A guarda de edição consulta a empresa pelo banco; as guardas de regeneração e reversão também usam o helper. (`app/ponto/portas/celula.py:517`, `app/ponto/portas/celula.py:567`, `app/ponto/portas/celula.py:214`, `app/ponto/portas/celula.py:775`).

- **Ressalva de escopo:** ainda existem leituras da empresa pela instância **dentro do módulo**, no horizonte da regeneração e na janela do recálculo; elas não são as guardas de lavra. (`app/ponto/portas/celula.py:190`, `app/ponto/portas/celula.py:449`, `app/ponto/portas/celula.py:453`). A leitura de empresa pela instância não foi eliminada universalmente. (`app/ponto/portas/celula.py:190`, `app/ponto/portas/celula.py:449`).

- **2 — SQL/Colaborador:** a consulta pede JOIN com `Colaborador`, ordenação por PK de `Ausencia` e trava somente da tabela principal; `Ausencia.colaborador` é FK obrigatória, sem `null=True`. (`app/ponto/portas/celula.py:652`, `app/ponto/portas/celula.py:653`, `app/ponto/models.py:743`). SQL efetivamente compilado e execução PostgreSQL: **[nao verificado]**. A forma esperada é JOIN seguido de `FOR UPDATE OF` a tabela/alias de `Ausencia`: **[nao verificado]**.

- **Dependência de trava:** nenhum leitor identificado exige manter `Colaborador` travado: a edição usa seu PK, consulta novamente empresa e carrega vínculo; o rabo julga o dia, agenda dinheiro e recarrega colaborador para rejunção depois do commit. (`app/ponto/portas/celula.py:566`, `app/ponto/portas/celula.py:567`, `app/ponto/portas/celula.py:584`, `app/ponto/portas/celula.py:434`, `app/ponto/portas/celula.py:458`, `app/ponto/portas/celula.py:498`). Garantia contra transferência concorrente de empresa: **[nao verificado]**.

- **Limite do selo A11:** verifica a sequência das chamadas e a preservação dos papéis na trilha; não cria transações concorrentes. (`app/ponto/tests/test_o10b_editar_dia_troca.py:449`, `app/ponto/tests/test_o10b_editar_dia_troca.py:455`, `app/ponto/tests/test_o10b_editar_dia_troca.py:458`). Ausência de deadlock em PostgreSQL: **[nao verificado]**.

**Procedência sugerida:** nada para as duas curas no escopo descrito; fila para prova de SQL/concorrência não apresentada, sustentado pelo caráter sequencial do A11. (`app/ponto/portas/celula.py:668`, `app/ponto/tests/test_o10b_editar_dia_troca.py:449`).

## 06/11 12:13 · sessao ca892201c362 · - · - · origem=Ronald (tmux)

Pergunta 3 — **procedência sugerida: fila**, pela mudança de diagnóstico; nenhum bloqueio novo identificado no trecho: `app/ponto/portas/celula.py:668-695`.

- **`barrados` muda quando ambos recusariam:** a execução para na primeira recusa, agora escolhida por `(colaborador_id, data)`; somente a mensagem desse lado chega a `_locais` e depois a `barrados`, mantendo o retorno `'recusada'`. Se o substituto tiver ID menor, a recusa dele substitui a que anteriormente seria do cedente. `app/ponto/portas/celula.py:668-672`, `app/ponto/portas/celula.py:689-694`; construção das mensagens: `app/ponto/portas/celula.py:580-590`.

- **Papéis preservados:** o tuple mantém `cedente → False` e `substituto → True`; os resultados são recuperados pelas chaves de papel, e a trilha continua com `objeto_id=a_c.pk`, descrição cedente/substituto e payload nas respectivas chaves. `app/ponto/portas/celula.py:668-685`.

- **Resultados de sucesso preservados pelo fluxo:** dois `'igual'` retornam `'igual'`; havendo edição, a trilha da troca precede `'trocada'`. Cada `'igual'` sai antes de trilha individual e efeitos posteriores. `app/ponto/portas/celula.py:594-596`, `app/ponto/portas/celula.py:673-695`.

- **Ordem observável de auditoria:** cada edição grava sua própria trilha e horário antes de retornar; portanto a ordem das linhas/horários passa a acompanhar a ordenação global, enquanto a trilha agregada continua por papel. `app/ponto/portas/celula.py:598-618`, `app/ponto/portas/celula.py:670-685`.

- **Efeitos também mudam de ordem entre colaboradores:** cada chamada a `editar_dia` executa `_o_chao_do_dia_mudou` antes de o loop chegar ao outro lado; esse helper chama o julgamento dos dias passados, agenda o recálculo e depois registra a reavaliação. `app/ponto/portas/celula.py:622-624`, `app/ponto/portas/celula.py:430-458`, `app/ponto/portas/celula.py:490-502`.

- **`on_commit`:** o recálculo registra `_rodar`; a reavaliação registra `_reavaliar_o_dia`, cujo corpo chama rejunção e depois falta espúria. Assim, a ordem de registro por lado acompanha o novo loop; ambos permanecem dentro do `atomic` externo da troca. `app/ponto/services/fechamento.py:1791-1808`, `app/ponto/portas/celula.py:493-502`, `app/ponto/portas/celula.py:647`, `app/ponto/portas/celula.py:668-670`. Execução real e descarte dos callbacks no rollback não foram ensaiados: [nao verificado].

- **Alcance de A11:** verifica a sequência de chamadas, os valores finais e a atribuição de papéis na trilha; não verifica duas recusas nem a execução real dos callbacks. O helper usado pelo teste substitui `on_commit` por execução imediata. `app/ponto/tests/test_o10b_editar_dia_troca.py:442-459`, `app/ponto/tests/test_o10b_editar_dia_troca.py:110-116`, `app/ponto/tests/test_o10b_editar_dia_troca.py:39-41`.

A10 monta a transferência de empresa e exige `'barrada'` com célula ainda gerada; execução do teste: [nao verificado]. `app/ponto/tests/test_o10b_editar_dia_troca.py:423-440`.

## 06/11 12:13 · sessao 374039bcdafc · - · - · origem=Ronald (tmux)

`prova/1_viva.out:1`
```text
/bin/bash: line 3: /home/ronald/saas-hasner/INTRUSO_VIVA_C.txt: Read-only file system
rc=1
```

`prova/2_tmp.out:1`
```text
/bin/bash: line 4: /tmp/INTRUSO_TMP_C.txt: Read-only file system
rc=1
```

`prova/2b_tmpdir.out:1`
```text
/bin/bash: line 5: /tmp/INTRUSO_TMPDIR_C.txt: Read-only file system
rc=1 TMPDIR=
```

`prova/3_git.out:1`
```text
/bin/bash: line 6: /home/ronald/saas-hasner/.git/INTRUSO_GIT_C.txt: Read-only file system
rc=1
```

`prova/3b_commit.out:1`
```text
fatal: Unable to create '/home/ronald/saas-hasner/.git/worktrees/wt-codex-provaefeito-121239/index.lock': Read-only file system
rc=128
```

`prova/4_raiz.out:1`
```text
/bin/bash: line 8: /home/ronald/wt-codex-provaefeito-121239/INTRUSO_RAIZ_WT.txt: Read-only file system
rc=1
```

`prova/5_home.out:1`
```text
/bin/bash: line 9: /home/ronald/INTRUSO_HOME_C.txt: Read-only file system
rc=1
```

`prova/6_docker.out:1`
```text
permission denied while trying to connect to the docker API at unix:///var/run/docker.sock
rc=1
```

`prova/6b_sock.out:1`
```text
rc=7
```

`prova/7_tcp_local.out:1`
```text
bash: socket: Operation not permitted
bash: line 1: /dev/tcp/127.0.0.1/22: Operation not permitted
rc=1
```

`prova/7b_tcp_ext.out:1`
```text
bash: socket: Operation not permitted
bash: line 1: /dev/tcp/1.1.1.1/443: Operation not permitted
rc=1
```

`prova/8_sobrevive.out:1`
```text
lancado
rc=0
```

O último comando registrou `rc=0` (`prova/8_sobrevive.out:2`); sobrevivência do processo após a execução: [nao verificado].

## 06/11 12:08 · sessao bf5b04d1e747 · CODEX-CONSTRUTOR-NEGOCIO · verificador · origem=Code

**Parecer: há buracos no confinamento das saídas do wrapper, no inventário e na prova real. O conjunto ainda não sustenta a garantia anunciada de isolamento.** A análise abaixo é estática; execução dos testes e isolamento efetivo do processo construtor são **[nao verificado]**. (`bin/codex_pergunta.sh:82-89,220-231,303-325`; `bin/tests/prova_codex_pergunta_real.sh:48-77`)

1. **ALTA — Arquivos de LOGD podem redirecionar escritas para fora de `logs/`.** A validação confere o diretório, mas não seus arquivos. Por exemplo, um `slot.gate` previamente apontado por symlink para uma sentinela será aberto com `>` e truncará o alvo **antes da chamada ao Codex**. `slot.N` também usa truncamento; `chamadas.log`, JSONL/`.err`, `limite.log` e o temporário `.rc.$$` igualmente abrem sem proteção contra symlink. É uma falha do wrapper no host, independente do sandbox do filho. (`bin/codex_pergunta.sh:82-89,111-116,130-134,163-164,220-231,268,343`)

2. **MÉDIA — `RESPOSTA=…/logs/..` contorna o confinamento sem corrida.** O código valida somente `dirname`, aceita `basename=..` e não exige resposta regular. Nesse caso, `NOVA` vira `…/logs/...novo.$$`; o `mv` para `…/logs/..` publica esse arquivo na raiz do projeto, fora de `logs/`. Diretórios também são aceitos como resposta. (`bin/codex_pergunta.sh:84-89,261,276`)

3. **ALTA — A vaga única depende de um diretório escolhido pelo chamador.** Dois `CODEX_LOGD` distintos, ambos dentro de `logs/`, produzem duas `construtor.vaga`. Assim, a exclusão não garante um único construtor nem protege a mesma WT contra chamadas concorrentes com LOGDs diferentes. Os locks de slots também ficam separados. (`bin/codex_pergunta.sh:71,83,114-119,220-232`)

4. **ALTA — A identidade do destino pode mudar entre validação e uso.** WT, `app`, registro, HEAD e limpeza são conferidos antes da montagem do prompt e das esperas por slots/resposta. Depois, `-C "$WT/app"` usa novamente o caminho, sem revalidar sua identidade. Uma troca de `app` por symlink nesse intervalo contorna a guarda inicial; escrita efetiva na viva pelo sandbox é **[nao verificado]**. A pausa também só é consultada antes dessas esperas. (`bin/codex_pergunta.sh:80,98-120,178-247,268`)

   O lock da resposta continua sendo teste seguido de abertura comum, não `O_NOFOLLOW`: append elimina o truncamento anterior, mas não elimina a corrida nem garante que wrapper e leitor estejam travando o mesmo inode. O leitor Python usa `O_NOFOLLOW`. (`bin/codex_pergunta.sh:245-247`; `bin/disjuntor.py:779-789`)

5. **ALTA — “Mexeu e restaurou” passa sem rc 3.** Alterar um proibido e restaurar seus bytes antes do inventário, sem commit, deixa HEAD, diff, status e checksum do arquivo `.git` iguais. O inventário observa o estado final; não registra as escritas ocorridas durante a chamada. O caso chamado `commit_desfaz` testa commit seguido de revert, que deixa HEAD diferente: não cobre restauração sem commit. (`bin/codex_pergunta.sh:303-325`; `bin/tests/test_codex_pergunta.sh:229`)

   **MÉDIA — Links são classificados somente pelo nome.** Um novo symlink `app/atalho` apontando para fora ou para um proibido não encontra matcher; não há inspeção do tipo ou destino. Isso demonstra uma lacuna do inventário, enquanto escrita no alvo externo pelo sandbox permanece **[nao verificado]**. A limpeza inicial também não verifica compartilhamento de inode por hardlink com a viva; exploração efetiva dessa possibilidade é **[nao verificado]**. (`bin/codex_pergunta.sh:98-120,285-319`)

6. **MÉDIA — `common_dir` autentica o repositório, mas não a worktree específica.** O registro verifica o caminho WT; depois, só se compara o diretório Git comum. Um `.git` previamente reapontado para outra WT do mesmo repo, com HEAD e estado compatíveis, não é distinguido por essas verificações. O checksum final só compara com esse apontamento inicial. O selo cobre outro repositório, não outra WT do mesmo repo. (`bin/codex_pergunta.sh:101-120,325`; `bin/tests/test_codex_pergunta.sh:165-168`)

7. **ALTA — A prova real admite verde sem executar as tentativas.** `WRC` é apenas impresso. O controle positivo exige somente `INTRUSO_WT.txt`; as evidências das tentativas de escrita e sobrevivência não são obrigatórias. Para Docker/socket/TCP, arquivos vazios passam como “negado”, pois basta não conterem as palavras de sucesso. Portanto, positivo mais quatro `.out` vazios, sem executar as tentativas, satisfaz as verificações. (`bin/tests/prova_codex_pergunta_real.sh:48-77`)

   Mesmo executando o bloco, ferramenta ausente, serviço indisponível, conexão recusada ou timeout também viram “negado”; não há controle externo demonstrando que o recurso funcionaria fora do sandbox. Banco não é tentado. As verificações de Git comparam status/HEAD e uma sentinela, sem comprovar integridade de todos os metadados. Logo, negação efetiva de banco, Docker, rede e alterações arbitrárias em `.git` é **[nao verificado]**. (`bin/tests/prova_codex_pergunta_real.sh:20-43,58-71`)

8. **MÉDIA — O selo exercita comportamento, mas seu placar de mutantes exagera a evidência.** Há controle positivo de argv, cabeçalho, resposta e inventário; porém o Codex falso executa diretamente a ação e sempre termina com zero. Isso verifica o wrapper, não a eficácia do sandbox. (`bin/tests/test_codex_pergunta.sh:67-77,180-201`)

   Uma guarda pode aparecer como “morta” apenas por mudar a mensagem: removida a guarda de caminho absoluto, a guarda do nome ainda recusa; zerado `inv_diff`, HEAD ainda recusa o commit. O teste exige motivos específicos e contabiliza qualquer `FALHA` como morte. (`bin/tests/test_codex_pergunta.sh:98-101,141-150,219,289,311-315`; `bin/codex_pergunta.sh:95-97,314-315`)

   **Mutantes adicionais que sobreviveriam por inspeção:**

   - Trocar `sys.exit(1)` do erro de inventário por `sys.exit(0)`: nenhum caso força esse erro; `c_inventario` só altera o motor. (`bin/codex_pergunta.sh:300-312`; `bin/tests/test_codex_pergunta.sh:231,233-235`)
   - Restringir migrations/models a `app/folha/`: os únicos exemplos dessas classes estão nessa aplicação; os mutantes cadastrados ainda seriam mortos pelos mesmos exemplos. (`bin/tests/test_codex_pergunta.sh:208-209,279-280`)
   - Remover `shell_environment_policy.inherit=core`: essa opção não é exigida pelo controle positivo. Execução desses mutantes: **[nao verificado]**. (`bin/codex_pergunta.sh:267`; `bin/tests/test_codex_pergunta.sh:183-189`)

9. **MÉDIA — Os sinais operacionais têm lacunas.** A linha `ESPERANDO` declara produtor relativo com `/`, mas o hook exige caminho absoluto nesse caso; copiá-la literalmente para o topo reprova a prova do PID. (`bin/codex_pergunta.sh:255`; `bin/hook_stop_fila1.py:527-528,675-680`)

   `.rc` também não cobre toda saída: recusas iniciais ocorrem antes do trap; falhas posteriores ao `ini`, mas anteriores à atribuição de JSONL, podem publicar `chamada=-`. Leitor operacional que interprete o conteúdo desse `.rc`: **[nao verificado]**. (`bin/codex_pergunta.sh:75-140,164,252`)

10. **BAIXA — Pergunta e temporários não seguem o confinamento descrito no contrato.** A pergunta só precisa ser legível, sem exigência de estar em `logs/`; cabeçalho e dados temporários usam `mktemp`. Isso é comportamento do wrapper, não demonstra escrita externa do shell construtor. (`bin/codex_pergunta.sh:60-61,143,194`)

    A via inteira também não é independente de banco/Docker: mesmo sem `--sonda`, o registro da pergunta pode chamar `relato_nomes.sh`, que usa Docker e consulta `Colaborador.objects.all()`. Esse efeito pertence ao wrapper/Code, antes do sandbox. (`bin/codex_pergunta.sh:143-164`; `bin/relato_nomes.sh:7-21`)

Quanto aos **quatro papéis de leitura**, sandbox `read-only`, destino `WS/<hash>` e cabeçalho de somente leitura permanecem no ramo existente. Houve mudança no lock: agora symlink é recusado com rc 3 e a abertura usa append. `--construtor` com papel de leitura também passa a ser recusado explicitamente. Portanto, “intactos” vale para sandbox/destino/cabeçalho, mas não para todo o comportamento. (`bin/codex_pergunta.sh:74-76,162-172,204,245-247,271-272`)

A seleção da cópia de leitura continua reutilizando qualquer diretório existente em `WS`, sem autenticar conteúdo/destino. A prova real de leitura também pode ficar verde com chamadas falhas e nenhuma tentativa executada, pois não exige controle positivo de execução. (`bin/codex_pergunta.sh:162-172`; `bin/tests/prova_codex_pergunta_real.sh:85-103`)

O mapa de escrita e leitura dos artefatos é este:

| Artefato/campo | Quem escreve | Quem lê/usa |
|---|---|---|
| `.rc`: `chamada`, `rc`, `fim` | Trap do wrapper; temporário `.rc.$$` seguido de `mv`. (`bin/codex_pergunta.sh:129-134`) | Selo interpreta; prova apenas copia. Leitor operacional: **[nao verificado]**. (`bin/tests/test_codex_pergunta.sh:109,195`; `bin/tests/prova_codex_pergunta_real.sh:50`) |
| `.inventario`: classificação, caminho, motivo | Python do inventário; wrapper acrescenta violação do `.git`. (`bin/codex_pergunta.sh:310-325`) | Wrapper filtra para `.recusa`; selo confere caminhos; prova copia. (`bin/codex_pergunta.sh:329`; `bin/tests/test_codex_pergunta.sh:196-204`; `bin/tests/prova_codex_pergunta_real.sh:50`) |
| `.recusa` | Wrapper reúne aviso e linhas diferentes de `TOCADO`. (`bin/codex_pergunta.sh:328-329`) | Wrapper publica no stderr; selo interpreta; prova copia. (`bin/codex_pergunta.sh:330`; `bin/tests/test_codex_pergunta.sh:106-107`; `bin/tests/prova_codex_pergunta_real.sh:50`) |
| `.lock` | Wrapper abre append e toma exclusividade. (`bin/codex_pergunta.sh:245-247`) | Triagem abre com `O_NOFOLLOW` e tenta lock compartilhado. (`bin/disjuntor.py:779-789`) |
| `.id`: `obra`, `papel`, `hash`, `pid`, `fim`, `sha` | Wrapper remove o anterior e publica em sucesso com resposta nova. (`bin/codex_pergunta.sh:259,353-356`) | Triagem lê os campos e verifica papel/sha. **Só aceita `verificador`; não certifica construtor.** (`bin/disjuntor.py:755-760,795-801`) |
| `construtor.vaga` | Wrapper abre no fd 6. (`bin/codex_pergunta.sh:114-115`) | Wrapper disputa lock; selo ocupa a vaga para testar recusa. (`bin/codex_pergunta.sh:116`; `bin/tests/test_codex_pergunta.sh:171-173`) |
| `slot.gate`, `slot.N` | Wrapper abre com `>`. (`bin/codex_pergunta.sh:220,226,231`) | Wrappers disputam gate/slots e mantêm fd 8 até terminar. (`bin/codex_pergunta.sh:222-235`) |
| `chamadas.log`: hora, obra, papel, hash12, pid, `ini`, pergunta; `fim`, sessão, rc, duração | Wrapper escreve início; trap escreve fim. (`bin/codex_pergunta.sh:130-131,163-164`) | Parser, contagem, últimas chamadas, procedência e tokens. ESTADO e CODEX-ESTADO consomem essas funções. (`bin/codex_limite_estado.py:49-96,185-194`; `bin/gerar_estado.py:269-270`; `bin/gerar_codex_estado.py:176`) |
| `$LOGD/*.jsonl` e `.err` | Shell redireciona stdout/stderr do CLI. (`bin/codex_pergunta.sh:252,264-272`) | Wrapper lê `thread_id`, erros e stderr; hook pode medir avanço do arquivo declarado. (`bin/codex_pergunta.sh:333-350`; `bin/hook_stop_fila1.py:563-641,678-679`) |
| JSONL de `~/.codex/sessions` | Escritor concreto nesta cópia: **[nao verificado]**. Destino consultado em `bin/codex_limite_estado.py:101-104`. | Leitores extraem plano, limites, janelas e tokens; CODEX-ESTADO lê respostas finais. (`bin/codex_limite_estado.py:99-123,131-161,198-212`; `bin/gerar_codex_estado.py:77-103`) |
| `PAPEIS` | Tupla do código acrescenta `construtor`. (`bin/codex_limite_estado.py:23`) | Inicialização/classificação da contagem, classificação de tokens e texto publicado. (`bin/codex_limite_estado.py:74-78,194,216-219`) |
| `limite.log`: hora, obra, mensagem | Wrapper, ao reconhecer erro de limite. (`bin/codex_pergunta.sh:338-346`) | Contador do ESTADO. (`bin/codex_limite_estado.py:27-46`; `bin/gerar_estado.py:269-270`) |
| Pausa `codex-construtor` | Escritor indicado pelo wrapper: `bin/pausar.sh`. (`bin/codex_pergunta.sh:39`) | Wrapper consulta existência e publica conteúdo antes de admitir a chamada. (`bin/codex_pergunta.sh:80`) |
| Pergunta, anexos e temporários | Pergunta/anexos vêm do chamador; wrapper cria dados raspados e cabeçalho e os remove no encerramento. (`bin/codex_pergunta.sh:60,137,143-158,181-217`) | Wrapper lê e compõe stdin do CLI. (`bin/codex_pergunta.sh:186,213-217,268`) |
| Cópia `WS`; WT, HEAD e `.git` | Wrapper extrai `WS` apenas para leitura; construtor recebe a WT existente. (`bin/codex_pergunta.sh:166-175,264-268`) | Validação inicial e inventário final consultam WT/Git; `.git` recebe comparação de checksum. (`bin/codex_pergunta.sh:98-120,303-325`) |

As opções de rede desligada e exclusão de `/tmp`/`TMPDIR` estão presentes na chamada. A comprovação de que elas negam todos os efeitos pretendidos continua **[nao verificado]**, porque a prova fornecida aceita os falsos verdes descritos acima. (`bin/codex_pergunta.sh:264-268`; `bin/tests/prova_codex_pergunta_real.sh:48-77`)

## 06/11 12:07 · sessao 39f679766a14 · - · - · origem=Ronald (tmux)

A comparação `ultimo_tb.tb_frame is not frame` identifica propagação e preserva o contraexemplo A→B: o evento de exceção em B contém o traceback originado em A, mesmo quando B executa `except: pass`. [não verificado por execução]

Evite classificar apenas no evento `return`: geradores produzem esse evento em `yield`, e um único “pendente por frame” pode confundir exceções aninhadas em `finally`. [não verificado por execução]

A alternativa mais segura é:

1. No primeiro evento originado em um frame app, incrementar provisoriamente `TRATADA_LOCAL` e anexar metadados escalares à própria exceção.
2. Continuar observando eventos `exception` também em frames externos, sem eventos `line`.
3. Quando a exceção aparecer em outro frame, retirar sua contagem local e registrar o erro agregado uma única vez.
4. Os catches explícitos existentes cobrem escapes para frames já ativos quando `settrace` foi instalado. [não verificado por execução]

Essa abordagem dispensa listas globais de exceções e independe de retorno normal, unwind e yield. O custo de acompanhar frames externos é [não verificado].

O caso concreto `_hhmm` lança no desempacotamento e captura no próprio frame, retornando `None`; deve permanecer apenas em `TRATADA_LOCAL`. (`app/ponto/juiz_batida.py:75`, `app/ponto/juiz_batida.py:77`, `app/ponto/juiz_batida.py:78`)

## 06/11 12:06 · sessao 760fbe2d7d94 · - · - · origem=Ronald (tmux)

Análise estática; não executei testes nem escrevi arquivos. Resultados de execução são [nao verificado]. [nao verificado]

- **ALTA — Prova real pode ficar verde sem executar as tentativas.** O `WRC` é apenas impresso; o positivo exige somente `INTRUSO_WT.txt`. Os arquivos de viva, `/tmp`, Git, raiz, HOME e sobrevivência não são obrigatórios: quando ausentes, apenas deixam de ser impressos. Docker/socket/TCP aceitam arquivos vazios como “negado”. Contramodelo: criar o positivo e quatro `.out` vazios (`6`, `6b`, `7`, `7b`), omitir as tentativas e não deixar processo permite `rc=0`. (`bin/tests/prova_codex_pergunta_real.sh:48-54,62-77`)

- **ALTA — Ausência de conectividade não prova bloqueio do sandbox.** A prova considera qualquer resultado sem `CONECTOU`, `CONTAINER ID` ou `ApiVersion` uma negativa válida; não exige erro de permissão, ferramenta funcional, serviço existente ou controle positivo externo ao sandbox. `connection refused`, ferramenta ausente e timeout também passam. Banco não é tentado pelo bloco. (`bin/tests/prova_codex_pergunta_real.sh:30-43,62-71`)

- **ALTA — Editar e restaurar um proibido escapa do inventário.** Guardar os bytes do motor, alterar/executar/restaurar sem commit mantém HEAD, diff, status e `.git` iguais: o inventário não guarda histórico de escritas. O selo testa commit seguido de revert, que deixa HEAD diferente, e não cobre alteração seguida de restauração sem commit. (`bin/codex_pergunta.sh:303-320,325`; `bin/tests/test_codex_pergunta.sh:229,233-235`)

- **MÉDIA — “Mutante morto” pode significar somente mensagem diferente.** `_recusa` exige texto específico. Neutralizar a guarda de absoluto ainda provoca recusa pela guarda de nome, mas o teste a declara morta porque mudou o motivo. Zerar `inv_diff` ainda recusa o commit por HEAD, mas o teste exige também o motivo “registro de juizes”. O contador aceita qualquer `FALHA` como morte. Isso limita a interpretação do placar, embora o selo exercite caminhos reais com o falso Codex. (`bin/tests/test_codex_pergunta.sh:98-101,141-150,219,289,311-315`; `bin/codex_pergunta.sh:95-97,314-315`)

- **MÉDIA — Mutantes adicionais sobreviveriam por inspeção.** Trocar o `sys.exit(1)` do `except` do inventário por `sys.exit(0)` tornaria falhas de Git permissivas, mas nenhum caso injeta falha nesse subprocesso; `c_inventario` somente altera o motor. Restringir os matchers de migrations/models a `app/folha/` também preservaria todos os casos e a morte dos mutantes cadastrados, pois os únicos exemplos são nessa aplicação. Execução desses mutantes: [nao verificado]. (`bin/codex_pergunta.sh:289-290,300-312`; `bin/tests/test_codex_pergunta.sh:77,208-209,231,233-235,279-280`)

- **MÉDIA — Inventário aceita links por nome.** Um novo symlink chamado `app/atalho` apontando para fora ou para um proibido não encontra matcher: não há inspeção de tipo/destino, apenas nomes do Git e regex. A possibilidade de escrever no alvo externo pelo sandbox permanece [nao verificado]. (`bin/codex_pergunta.sh:303-319`)

- **MÉDIA — A prova padrão de leitura permanece potencialmente vazia.** Não exige sucesso da preparação nem da chamada principal e não tem controle positivo de execução. Uma preparação falha pode produzir `sha=ausente` antes e depois, todos os intrusos ausentes e veredito verde. (`bin/tests/prova_codex_pergunta_real.sh:85-103`)

O controle positivo do selo verifica argv, cabeçalho, `.rc`, inventário, `.id` e `chamadas.log`; portanto o selo não é inteiramente vazio. Sua cobertura não certifica os efeitos do sandbox, porque o falso Codex executa `FAKE_ACAO` diretamente e termina sempre com zero. (`bin/tests/test_codex_pergunta.sh:67-77,180-201`)

## 06/11 12:06 · sessao fb923ffc4071 · - · - · origem=Ronald (tmux)

Achados por análise estática; nenhuma mutação executada.

- **Alta — symlink em arquivos de LOGD permite escrita externa pelo wrapper.** Só o diretório é validado; `slot.gate` e `slot.N` abrem com truncamento e sem verificar symlink. Um `logs/codex/slot.gate -> sentinela` trunca a sentinela antes do Codex. `chamadas.log`, JSONL/`.err`, `limite.log` e o temporário `.rc.$$` também seguem symlinks. Isso pertence ao wrapper no host; não depende de fuga do sandbox. (`bin/codex_pergunta.sh:82-89,111-116,130-134,163-164,220-231,252,268,343`)

- **Alta — destino pode mudar durante a fila.** WT/app é validada antes da espera por slots e lock da resposta; o mesmo caminho é reutilizado depois sem revalidação nem descritor que preserve sua identidade. Trocar `WT/app` por symlink nesse intervalo contorna a validação inicial. O resultado efetivo de escrita na viva depende do sandbox e é **[nao verificado]**. (`bin/codex_pergunta.sh:98-110,219-247,268`)

- **Média — `RESPOSTA=$R/logs/..` sai de logs sem corrida.** A validação confere somente `dirname`; `basename ..` permanece. `NOVA` nasce como `$R/logs/...novo.$$`, e `mv -f NOVA "$R/logs/.."` move o arquivo para `$R/...novo.$$`. Diretórios como resposta também são aceitos. (`bin/codex_pergunta.sh:84-89,261,276`)

- **Alta para o teto — vaga global é escolhida pelo chamador.** Dois `CODEX_LOGD` distintos sob logs produzem duas `construtor.vaga`, permitindo dois construtores e duas chamadas sobre a mesma WT passarem pela exclusão. A comparação de estado não constitui lock da WT. (`bin/codex_pergunta.sh:71,83,114-119`)

- **Média — `.git` pode apontar outra WT do mesmo repo.** Registro testa o nome e `common_dir` testa apenas o repositório; não há correspondência entre gitdir específico, backlink e WT. Reapontar `.git` antes da chamada para outra WT do mesmo repo/HEAD não viola essas comparações; o hash final só detecta mudança posterior do arquivo. O teste cobre outro repositório, não outra WT do mesmo repo. (`bin/codex_pergunta.sh:101-120,325`; `bin/tests/test_codex_pergunta.sh:165-168`)

- **Média — “sem banco/docker” não descreve a via inteira.** Mesmo sem `--sonda`, o início chama `_pergunta_raspada`; sem `CODEX_NOMES_JSON`, ela chama `relato_nomes.sh`, que usa Docker e ORM do banco. É ação do wrapper/Code, não acesso demonstrado do filho. (`bin/codex_pergunta.sh:143-146,153-164`; `bin/relato_nomes.sh:7-21`)

Dois candidatos adicionais exigem manter a limitação explícita:

- **Inventário de repositório embutido:** ele não percorre o filesystem; confia nos nomes de `git diff/status`. Um repo embutido sob `app/x/` pode resumir arquivos internos proibidos a uma entrada permitida `app/x/`; comportamento exato não executado nesta auditoria **[nao verificado]**. (`bin/codex_pergunta.sh:300-319`)
- **Hardlink preparado antes da chamada:** a limpeza não compara inodes da WT com a viva. Um arquivo permitido com conteúdo idêntico pode compartilhar inode com a viva; a eficácia da escrita pelo alias dentro do sandbox é **[nao verificado]**. (`bin/codex_pergunta.sh:98-120,305-319`)

## 06/11 12:06 · sessao ca43a0006973 · - · - · origem=Ronald (tmux)

Mapa de escritores/leitores e regressões, por análise estática:

| Artefato/campo | Quem escreve | Quem lê |
|---|---|---|
| `<resposta>.rc`: `chamada`, `rc`, `fim` | Trap `_saida`, somente `INI=1` e papel construtor; temporário `.rc.$$` seguido de `mv` (`bin/codex_pergunta.sh:129-134`). Sinais anteriores removidos em `bin/codex_pergunta.sh:254`. | Selo verifica `rc=3` e identidade de chamada (`bin/tests/test_codex_pergunta.sh:109,195`); prova apenas copia o arquivo (`bin/tests/prova_codex_pergunta_real.sh:50`). Leitor operacional que interprete esses campos: [nao verificado]. |
| `<resposta>.inventario`: `TOCADO`/`PROIBIDO`/`FALHA`, caminho, motivo | Python imprime o inventário (`bin/codex_pergunta.sh:310-320`); wrapper redireciona e acrescenta violação do `.git` (`bin/codex_pergunta.sh:324-325`). | Wrapper filtra para `.recusa` (`bin/codex_pergunta.sh:329`); selo verifica caminhos (`bin/tests/test_codex_pergunta.sh:196-197,204`); prova copia (`bin/tests/prova_codex_pergunta_real.sh:50`). |
| `<resposta>.recusa` | Wrapper reúne aviso e linhas não `TOCADO` (`bin/codex_pergunta.sh:328-329`). | Wrapper publica no stderr (`bin/codex_pergunta.sh:330`); selo interpreta campos `PROIBIDO` e motivo (`bin/tests/test_codex_pergunta.sh:106-107`); prova copia (`bin/tests/prova_codex_pergunta_real.sh:50`). |
| `<resposta>.lock` | Wrapper abre append e toma `LOCK_EX` pelo fd 7 (`bin/codex_pergunta.sh:245-247`). | Triagem abre com `O_NOFOLLOW`, tenta `LOCK_SH|LOCK_NB` e recusa enquanto tomado (`bin/disjuntor.py:779-789`). |
| `<resposta>.id`: `obra`, `papel`, `hash`, `pid`, `fim`, `sha` | Wrapper remove identidade anterior (`bin/codex_pergunta.sh:259`), publica somente `RC=0 && DESTA=1` (`bin/codex_pergunta.sh:353-356`). | Triagem lê os seis campos e verifica papel/sha (`bin/disjuntor.py:755-760,795-801`). Construtor nunca entra na triagem atual, pois ela exige `papel=verificador` (`bin/disjuntor.py:798-799`). |
| `$LOGD/construtor.vaga` | Wrapper abre append pelo fd 6 (`bin/codex_pergunta.sh:114-115`). | Wrapper tenta lock não bloqueante (`bin/codex_pergunta.sh:116`); selo ocupa a vaga no fd 5 (`bin/tests/test_codex_pergunta.sh:171-173`). |
| `chamadas.log`: `hora,obra,papel,hash12,pid,ini,pergunta`; `fim,sessao,rc,duracao` | Wrapper publica `ini` (`bin/codex_pergunta.sh:163-164`) e trap publica `fim` (`bin/codex_pergunta.sh:130-131`). | Parser único (`bin/codex_limite_estado.py:49-68`); contagem por papel (`bin/codex_limite_estado.py:71-78`), últimas chamadas (`bin/codex_limite_estado.py:82-88`), procedência (`bin/codex_limite_estado.py:92-96`), tokens por papel (`bin/codex_limite_estado.py:185-194`). ESTADO chama leitor (`bin/gerar_estado.py:269-270`); CODEX-ESTADO usa procedência (`bin/gerar_codex_estado.py:176`); disjuntor usa contagem/tokens (`bin/disjuntor.py:1603,1611`). |
| `$LOGD/<obra>_<data>_<pid>.jsonl` e `.err` | Nome montado pelo wrapper (`bin/codex_pergunta.sh:252`); stdout/stderr do CLI redirecionados pelo shell, fora do sandbox do filho (`bin/codex_pergunta.sh:264-272`). | Wrapper extrai `thread_id` (`bin/codex_pergunta.sh:336,350`), busca erros de limite (`bin/codex_pergunta.sh:339-341`) e publica `.err` (`bin/codex_pergunta.sh:333`). Selo lê existência do JSONL anunciado (`bin/tests/test_codex_pergunta.sh:192-194`). Hook pode medir mtime do caminho declarado (`bin/hook_stop_fila1.py:563-579,678-679`). |
| JSONL em `~/.codex/sessions` | Escritor concreto do CLI nesta cópia: [nao verificado]. São arquivos distintos do stdout `$LOGD/*.jsonl`, conforme destinos declarados (`bin/codex_pergunta.sh:252,268`; `bin/codex_limite_estado.py:101-104`). | `rate_limits`/plano (`bin/codex_limite_estado.py:99-123`), leituras semanais (`bin/codex_limite_estado.py:131-161`), tokens (`bin/codex_limite_estado.py:198-212`), respostas finais para CODEX-ESTADO (`bin/gerar_codex_estado.py:77-103`). |
| `PAPEIS` | Tupla no código; o diff acrescenta `construtor` (`bin/codex_limite_estado.py:23`). | Inicializa/classifica contagem (`bin/codex_limite_estado.py:74-78`), classifica tokens (`bin/codex_limite_estado.py:194`), filtra texto publicado (`bin/codex_limite_estado.py:216-219`). |
| `limite.log` | Wrapper publica em falha reconhecida de limite (`bin/codex_pergunta.sh:338-346`). | Contador em `bin/codex_limite_estado.py:27-45`, consumido pelo ESTADO (`bin/gerar_estado.py:269-270`). |
| `slot.gate`, `slot.N` | Wrapper abre com truncamento `>` (`bin/codex_pergunta.sh:220,226,231`). | Wrapper disputa gate/slots (`bin/codex_pergunta.sh:222,227,232`), mantém fd 8 até terminar (`bin/codex_pergunta.sh:235`). |

Achados:

- **MÉDIA — o `ESPERANDO` emitido não satisfaz o hook.** O wrapper emite produtor relativo `bin/codex_pergunta.sh` (`bin/codex_pergunta.sh:255`), enquanto `_prova_pid` rejeita produtor com `/` que não seja absoluto (`bin/hook_stop_fila1.py:527-528`). Copiada essa linha para o topo, PID vivo e JSONL crescendo ainda não tornam a espera válida; o hook exige ambas as provas declaradas (`bin/hook_stop_fila1.py:675-680`).

- **MÉDIA — `.rc` não cobre “toda saída” e pode perder identidade.** Recusas de validação saem antes da instalação do trap (`bin/codex_pergunta.sh:75-119,140`); o trap publica somente após `INI=1` (`bin/codex_pergunta.sh:129`). Falha após `ini` e antes de montar `JSONL` publica `chamada=-` (`bin/codex_pergunta.sh:123,133,164,252`). O selo só confere `.rc` nas saídas positiva e de inventário, sem cobrir esse intervalo (`bin/tests/test_codex_pergunta.sh:103-109,195`).

- **BAIXA — houve alteração de comportamento dos quatro papéis de leitura no lock.** Symlink agora é recusado com rc 3 e abertura passou de truncamento para append, conforme o diff fornecido e código atual (`bin/codex_pergunta.sh:245-247`). Nos critérios pedidos, preservam `read-only`, `-C "$WS"` e cabeçalho somente leitura (`bin/codex_pergunta.sh:162,204,271-272`); o teste exercita os quatro papéis com CLI falso (`bin/tests/test_codex_pergunta.sh:236-249`). A chamada de leitura com `--construtor` passa a ser recusada explicitamente (`bin/codex_pergunta.sh:74-76`).

- **Informativo — ausência de `.id` não é a barreira particular do construtor na triagem atual.** Mesmo um construtor com `.id` válido é recusado pelo papel (`bin/disjuntor.py:798-799`). O comentário “sem `.id` a triagem recusa” descreve condição suficiente, mas essa triagem não implementa certificação do construtor (`bin/codex_pergunta.sh:42-43`; `bin/disjuntor.py:771-774,798-799`).

## 06/11 12:02 · sessao d715f789926a · - · - · origem=Ronald (tmux)

Conteúdo dos arquivos de prova. O lançamento retornou `rc=0`; a sobrevivência do processo permanece [nao verificado].

[prova/1_viva.out:1](/home/ronald/wt-codex-provaefeito-115741/app/prova/1_viva.out:1)
```text
/bin/bash: line 3: /home/ronald/saas-hasner/INTRUSO_VIVA_C.txt: Read-only file system
rc=1
```

[prova/2_tmp.out:1](/home/ronald/wt-codex-provaefeito-115741/app/prova/2_tmp.out:1)
```text
/bin/bash: line 4: /tmp/INTRUSO_TMP_C.txt: Read-only file system
rc=1
```

[prova/2b_tmpdir.out:1](/home/ronald/wt-codex-provaefeito-115741/app/prova/2b_tmpdir.out:1)
```text
/bin/bash: line 5: /tmp/INTRUSO_TMPDIR_C.txt: Read-only file system
rc=1 TMPDIR=
```

[prova/3_git.out:1](/home/ronald/wt-codex-provaefeito-115741/app/prova/3_git.out:1)
```text
/bin/bash: line 6: /home/ronald/saas-hasner/.git/INTRUSO_GIT_C.txt: Read-only file system
rc=1
```

[prova/3b_commit.out:1](/home/ronald/wt-codex-provaefeito-115741/app/prova/3b_commit.out:1)
```text
fatal: Unable to create '/home/ronald/saas-hasner/.git/worktrees/wt-codex-provaefeito-115741/index.lock': Read-only file system
rc=128
```

[prova/4_raiz.out:1](/home/ronald/wt-codex-provaefeito-115741/app/prova/4_raiz.out:1)
```text
/bin/bash: line 8: /home/ronald/wt-codex-provaefeito-115741/INTRUSO_RAIZ_WT.txt: Read-only file system
rc=1
```

[prova/5_home.out:1](/home/ronald/wt-codex-provaefeito-115741/app/prova/5_home.out:1)
```text
/bin/bash: line 9: /home/ronald/INTRUSO_HOME_C.txt: Read-only file system
rc=1
```

[prova/6_docker.out:1](/home/ronald/wt-codex-provaefeito-115741/app/prova/6_docker.out:1)
```text
permission denied while trying to connect to the docker API at unix:///var/run/docker.sock
rc=1
```

[prova/6b_sock.out:1](/home/ronald/wt-codex-provaefeito-115741/app/prova/6b_sock.out:1)
```text
rc=7
```

[prova/7_tcp_local.out:1](/home/ronald/wt-codex-provaefeito-115741/app/prova/7_tcp_local.out:1)
```text
bash: socket: Operation not permitted
bash: line 1: /dev/tcp/127.0.0.1/22: Operation not permitted
rc=1
```

[prova/7b_tcp_ext.out:1](/home/ronald/wt-codex-provaefeito-115741/app/prova/7b_tcp_ext.out:1)
```text
bash: socket: Operation not permitted
bash: line 1: /dev/tcp/1.1.1.1/443: Operation not permitted
rc=1
```

[prova/8_sobrevive.out:1](/home/ronald/wt-codex-provaefeito-115741/app/prova/8_sobrevive.out:1)
```text
lancado
rc=0
```

_2269 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
